## Java IO体系 ##
1.关于IO的概述
本质就是数据的传输，需要明确的是数据源和数据目的地。进而根据不同的需求产生出许多操作数据的流。

2.基本IO流分类
  
  基于字节的IO操作流InputStream/OutputStream
  
  ![image](C:\Users\acer\Desktop\use\InputStream类层次结构.JPG)
  
  ![image](C:\Users\acer\Desktop\use\OutputStream类层次结构.JPG)

  基于字符的IO操作流Reader/Writer
  
  ![image](C:\Users\acer\Desktop\use\Reader类层次结构.JPG)

  
  ![image](C:\Users\acer\Desktop\use\Writer类层次结构.JPG)

3.字节与字符之间的转化

  存在的原因：磁盘和网络IO操作的最小单位是字节，而程序中经常操作字符串。所以需要实现字符与字节之间的转化。注意：读和写要用同一种编码或者解码的字符集。通常不建议使用操作系统的默认字符集，因为若要跨平台，可能会导致乱码情况出现。
  
  ![image](C:\Users\acer\Desktop\use\字符解码相关类.JPG)

  Reader是Java中读字符的父类，InputStream是读字节的父类
  InputStreamReader是将字节转换为字符的类，该类实现的时候需要
  借助StreamDecoder类去实现解码过程，该过程需要指定Charset,解码的方式，假如没有指定，会使用平台自己的默认字符编码方式，中文默认是GBK.读的过程将字节转字符的过程--解码

  
  ![image](C:\Users\acer\Desktop\use\字节编码相关类.JPG)

  Writer是Java中写字符的父类，OutputStream是写字节的父类
  InputStreamWriter是将写字节转换为字符的类，写的过程其实就是将字符转字节的过程，要借组StreamEncoder类实现编码。

  eg:文件的读写--字符字节之间的转换

  可以直接使用FileWriter和FileReader就可以实现，因为这两个类继承了转换流类，是对下列代码的封装。

  `public class Test {

	public static void main(String[] args) throws IOException {
		//先创建写数据到文件中的对象--字符转字节
		String charset = "UTF-8";
		File file = new File("demo5.txt");
		FileOutputStream fos = new FileOutputStream(file);
		OutputStreamWriter osw = new OutputStreamWriter(fos, charset);
		//写入文件，并关闭资源(实际是字节流涉及的底层资源)
		osw.write("今天是国庆节");
		osw.close();
		
        //再创建在文件中读数据的对象---字节转字符
		FileInputStream fis = new FileInputStream(file);
		InputStreamReader isr = new InputStreamReader(fis,charset);
		char[] c = new char[1024];
		StringBuffer s = new StringBuffer();
		int len = 0;
		//读数据关闭资源并打印数据
		while((len = isr.read(c)) != -1){
			s.append(c, 0, len);
		}
        isr.close();
		System.out.println(s.toString());
		
	}

}`
  
  
