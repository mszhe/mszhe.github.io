---
layout:     post
title: golang基于master/worker模式的高并发
date: '2017-11-17'
description:
categories:
    - golang
tags:
    - master/worker
    - 高并发

---

# master调度

```
type Master struct {
	maxWorkers int
	workerPool chan chan Job
}

func NewMaster(maxWorkers int) *Master {
	return &Master{
		maxWorkers: maxWorkers,
		workerPool: make(chan chan Job, maxWorkers),
	}
}

func (m *Master) dispatch() {
	for i := 1; i < m.maxWorkers+1; i++ {
		worker := NewWorker(i, m.workerPool)
		worker.Run()
	}
	go func() {
		for {
			select {
			case job := <-JobQueue:
				go func(job Job) {
					jobChannel := <-m.workerPool
					jobChannel <- job
				}(job)
			}
		}
	}()
}
```


# job

```go
var (
	MaxWorkers  = 100
	JobQueueLen = 100
	JobQueue    = make(chan Job, JobQueueLen)
)

type Job struct {
	Payload Payload
}

type Payload struct {
	Name string
}

func (p *Payload) Do(id int) {
	fmt.Printf("worker-%v now working, job: %v\n", id, p.Name)
	time.Sleep(time.Second * 5)
}
```

# worker

```go
type Worker struct {
	Id         int
	quit       chan bool
	jobChannel chan Job
	workerPool chan chan Job
}

func NewWorker(id int, workerPool chan chan Job) Worker {
	return Worker{
		Id:         id,
		quit:       make(chan bool),
		jobChannel: make(chan Job),
		workerPool: workerPool,
	}
}

func (w *Worker) Run() {
	go func() {
		for {
			w.workerPool <- w.jobChannel
			select {
			case job := <-w.jobChannel:
				job.Payload.Do(w.Id)
			case <-w.quit:
				return
			}
		}
	}()
}

func (w *Worker) Stop() {
	go func() {
		w.quit <- true
	}()
}
```


# main
```go
func main() {
	master := NewMaster(MaxWorkers)
	master.dispatch()

	for i := 1; i <= 100000; i++ {
		JobQueue <- Job{Payload: Payload{Name: fmt.Sprintf("Job%v", i)}}
	}

	time.Sleep(time.Second * 1000)
}
```