title: Java备份和还原MySQL数据库
author: ergz
tags:
  - Java
categories:
  - Java基础
date: 2015-05-16 10:11:00
---
**使用Java代码备份和还原MySQL数据库**
<!--more-->

### 备份

```Java
	public static void backup(String path) throws IOException {
        Runtime runtime = Runtime.getRuntime();
        Process process = runtime.exec("mysqldump -u root -p123456 db");
        InputStream inputStream = process.getInputStream();
        InputStreamReader reader = new InputStreamReader(inputStream);
        BufferedReader br = new BufferedReader(reader);
        String s = null;
        StringBuffer sb = new StringBuffer();
        while ((s = br.readLine()) != null) {
            sb.append(s + "\r\n");
        }
        s = sb.toString();
        System.out.println(s);
        File file = new File(path);
        file.getParentFile().mkdirs();
        FileOutputStream fileOutputStream = new FileOutputStream(file);
        fileOutputStream.write(s.getBytes());
        fileOutputStream.close();
        br.close();
        reader.close();
        inputStream.close();
    }
```
### 还原
```Java
	public static void recover(String path) throws IOException {
        Runtime runtime = Runtime.getRuntime();
        Process process = runtime.exec("mysql -u root -p123456 --default-character-set=utf8 db");
        OutputStream outputStream = process.getOutputStream();
        BufferedReader br = new BufferedReader(new InputStreamReader(new FileInputStream(path)));
        String str = null;
        StringBuffer sb = new StringBuffer();
        while ((str = br.readLine()) != null) {
            sb.append(str + "\r\n");
        }
        str = sb.toString();
        System.out.println(str);
        OutputStreamWriter writer = new OutputStreamWriter(outputStream, "utf-8");
        writer.write(str);
        writer.flush();
        outputStream.close();
        br.close();
        writer.close();
    }
```


