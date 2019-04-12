---
layout:     post
title:      FileChannel处理下载文件
subtitle:   
date:       2019-04-11
author:     mszhe
catalog:    true
categories:
    - java
tags:
    - FileChannel
    - NIO
---

- 示例
```java
public static void main(String[] args) {
        String originFileUrl = "";
        String savePath = "";
        String fileName = "";
        InputStream is = null;
        FileOutputStream fos = null;
        try {
            URL url = new URL(originFileUrl);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestProperty(USER_AGENT, USER_AGENT_VALUE);
            is = conn.getInputStream();

            //文件保存位置
            File saveDir = new File(savePath);
            if (!saveDir.exists()) {
                boolean mkdir = saveDir.mkdir();
                logger.info("mkdir : {}, result: {}", savePath, mkdir);
            }
            File file = new File(saveDir + File.separator + fileName);
            fos = new FileOutputStream(file);

            fos.getChannel().transferFrom(Channels.newChannel(is), 0, Integer.MAX_VALUE);
            
            fos.flush();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            IOUtils.closeQuietly(fos);
            IOUtils.closeQuietly(is);
        }
    }
```