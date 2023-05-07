## 补充：Spring文件下载

### 	下载过程	

​	我们在页面上指定下载资源`<a href="/download?filename=dsb.jpg">资源下载</a>`后，还需要再Controller中编写我们将要进行的操作：

```java
 /**
     * 文件下载
     */
    @RequestMapping("/download")
    @ResponseBody
    public Map<String, Object> download(String filename,HttpServletResponse response) {
        System.out.println(filename);
        //定义一个返回值
        HashMap<String, Object> map = new HashMap();
//        指定提供资源的磁盘的目录
        String dir="F:\\Typora文件\\SpringBoot\\img\\";
//        拼接该文件地址-判定该文件是否存在
        File file = new File(dir + filename);
//进行判断是否存在
        if (file.exists()){
//            存在--读取文件--响应文件--关闭流
            try {
//            创建输入流对象--读取磁盘文件
                InputStream inputStream = new FileInputStream(file);
//             创建一个获取文件输出流对象  可以看成FileOutputStream
                ServletOutputStream outputStream = response.getOutputStream();
//                指定文件的名称
                response.setHeader("Content-Disposition","filename="+filename);
//                文件类型
                response.setHeader("Content-Type","application/jpeg");
//读取过程
                byte []bytes=new byte[2048];
                int len=0;
                while ((len=inputStream.read(bytes))!=-1){
                    outputStream.write(bytes,0,len);
                }
//                关闭流
                outputStream.close();
                inputStream.close();
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }else {
//            如果文件不存--响应改文件不存在
            map.put("msg", "文件下载失败--文件不存在");
        }
        return map;
    }
```



​	而在链接中我们给了定了资源为dsb.jpg,我们在Controller中获取到后，还需要进行指定这个图片资源的位置在哪里。

```java
//            创建输入流对象--读取磁盘文件
                InputStream inputStream = new FileInputStream(file);
```

​	在指定完后，我们还需要进行判断文件资源是否存在。

```java
//        指定提供资源的磁盘的目录
        String dir="F:\\Typora文件\\SpringBoot\\img\\";
//        拼接该文件地址-判定该文件是否存在
        File file = new File(dir + filename);
//进行判断是否存在
        if (file.exists()){
          ....
        }
```

​	在判断完成后若是存在，需要去读取磁盘中的文件，创建一个文件输出流。

```java
//             创建一个获取文件输出流对象  可以看成FileOutputStream
                ServletOutputStream outputStream = response.getOutputStream();
```

​	同时我们还需要返回响应头的，让浏览器可以返回下载框，并给其设置下载文件的名称：

```java
//                指定文件的名称
                response.setHeader("Content-Disposition","filename="+filename);
//                文件类型
                response.setHeader("Content-Type","application/jpeg");
```

​	然后在读取下载资源并写入文件

```java
//读取过程
                byte []bytes=new byte[2048];
                int len=0;
                while ((len=inputStream.read(bytes))!=-1){
                    outputStream.write(bytes,0,len);
                }
```



### 遇到的问题

1. 使用restful风格无法获取到我们我们所需的资源。
   在路径上我们无法去读取`<a href="/download/dsb.jpg">资源下载</a>`到dsb.jpg的文件，会报一个406的错误。
2. 获取资源类型的格式。

```java
//                文件类型
                response.setHeader("Content-Type","application/jpeg");
```

这里jpg格式的一般来说是要在找到相应的格式去进行下载，找到一个资源是：image/jpeg 使用这个格式。

