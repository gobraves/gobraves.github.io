---
layout: post
title: 输入/输出
date: 2015-07-04
tags: Note
categories: Java
---
### 输入/输出
1. 文件系统和路径
2. 文件和目录的处理及操作
3. 输入/输出流  
4. 读取二进制数据  
5. 写入二进制数据  
6. 写入文本  
7. 读取文本  
8. 用PrintStream记录日志  
9. 随机访问文件  
10. 对象序列化  

#### 1.文件系统和路径  
1. 文件系统中的对象可以用一条路径作为惟一的识别  
2. 路径分为绝对路径和相对路径
3. 文件或者目录一般用java.io.File对象表示，java7中，用java.nio.file.Path接口代替它   

> FileSystem类 :

  >  - `FileSystems.getDefault()`获取当前的文件系统   
  >  - `getSeparator()`获取当前系统的分隔符   
  >  - `getRootDirectories()`返回一个Iterable,遍历根目录     
4. 创建path
    1. `FileSystems.getPath(...)`  
    2. `Path path = Paths.get(...)`
    3. `getNameCount()`,`getName(int index)`从0开始
    4. path其他方法`getFileName()`,`getParent()`,`getRoot()`

#### 2.文件和目录的处理及操作
1. 创建和删除文件及目录   
   `createFile()`,`createDirectory()`,`delete()`,`deleteIfExists()`,如果用`delteIfExists()`删除目录，则目录必须为空
2. 获取目录的对象  
   使用newDirectoryStream()方法,`public static DirectoryStream<Path> newDirectoryStream(Path path)`  

   ---
   >  代码实例：

    >	    Path parent = Paths.get("/home/simple_chen/下载");
        	try (DirectoryStream<Path> children = Files.newDirectoryStream(parent)){
        	  for (Path child : children) {
           	  System.out.println(child);
              }
            } catch (IOException e) {
             		e.printStackTrace();
            }   
   ---        

3. 复制和文件移动   
     1. 复制
       `public static Path copy(Path source, Path target, CopyOption...options) throws java.io.IOException`   
       CopyOption是java.nio.file包的一个接口。`StandardCopyOption`是CopyOption接口的一个实现。  
       StandardCopyOption有三个复制选项:
        - ATOMIC_MOVE  
        - COPY_ATTRIBUTES   
        - REPLACE_EXISTING  
     2. 移动
        `public static Path move(Path source, Path target, CopyOption...options) throws java.io.IOException`   
4. 文件读取和写入
    对于较小的文件，File类提供了从二进制文件和文本文件读取和写入的方法
    - 二进制  
    `public static byte[] readAllBytes(Path path) throws java.io.IOException`  
    `public static write(Path path, byte[] bytes, OpenOption...options) throws java.io.IOException`
    - 文本文件  
    `public static List<String> readAllLines(Path path, java.nio.charset.Charset charset) throws java.io.IOException`
    `public static write(Path path, java.lang.Iterable<? extends CharSequence> lines, java.nio.charset.Charset charset, OpenOption...options) throws java.io.IOException`  

    >  这两个方法都重载了OpenOption,第二个方法还重载了一个Charset  	
    >  		
    - StandardOpenOption实现了OpenOption接口:  
      -  APPEND  
      -  CREATE  
      -  CREATE_NEW  
      -  DELETE_ON_CLOSE   
      -  DSYNC  
      -  READ  
      -  SPARSE  
      -  SYNC  
      -  TRUNCATE_EXISTING  
      -  WRITE   

    >    - java.nio.charset.Charset是一个表示字符集的抽象类。需要指定在将**字符编码成字节**和将**字节解码成字符**时要使用的字符集。
    创建一个charset   
    `Charset usAscii = Charset.forName("US-ASCII")`    

    > ***代码实例:***  
    >		         
        	Path textfile = Paths.get("/home/simple_chen/textfile");
        	Charset charset = Charset.forName("US-ASCII");
        	String line1 = "Easy read and write";
        	String line2 = "Easy read and write";
        	List<String> lines = Arrays.asList(line1,line2);
        	try {
            	Files.write(textfile,lines,charset);
        	} catch (IOException e) {
            	e.printStackTrace();
        	}  
    >
        	List<String> linesRead = null;
        	try {
            	linesRead = Files.readAllLines(textfile, charset);
        	} catch (IOException e) {
            	e.printStackTrace();
        	}  
    >
        	if(linesRead != null) {
            	for (String str : linesRead) {
                	System.out.println(str);
            	}
        	}	   

#### 3. 输入/输出流
1. What is Stream?  
个人理解：用一种统一的方式使的数据在不同的接收装置中传输的机制吧。打个比方，流就是管道，接收装置就是一个个“水库”，而管道将各个水库连接起来。
2. 流的分类  
   根据数据流向，可分为输入流和输出流，而数据又可以分为二进制数据和字符，因此每种又可分为两种，即：
			- Reader: 从一个接收装置中读取字符的流
			- Writer: 将一个字符写入一个接收装置的流
			- InputStream: 从一个接收装置中读取二进制数据的流
			- OutputStream: 将二进制数据写入到一个接收装置的流		
3. 流的一般操作顺序：
			1. 创建一个流。得到的对象已经处于打开状态，因此没有open方法可以调用
			2. 执行读取或者写入操作
			3. 通过调用close方法关闭流。由于当前大多数流都实现了java.lang.AutoCloseable接口，因此可以利用try-with-resources语句创建一个流，并且让流自动关闭
4. 使用流的好处
 			1. 为数据的读取和写入定义了方法，无论数据源还是数据目标都可以使用。
 			2. 为了连接一个专门的接收装置，利用java.nio.file.Files类提供的方法，正确构造流即可。

#### 4. 读取二进制数据
