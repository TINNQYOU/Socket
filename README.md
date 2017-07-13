# Socket
ServerSocketandSocket
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class ServerSocketDemo {
	public static void main(String[] args) throws Exception {
		ServerSocket server=null;
		//开启服务端
		try {
			 server = new ServerSocket(12345);
			System.out.println("服务器开启");
			//持续监听客户端请求
			while (true) {
				Socket socket = server.accept();
				System.out.println("连接的客户端是:" + socket.getInetAddress());
				//输入输出流
				BufferedReader ins = new BufferedReader(
						new InputStreamReader(System.in)); //读取控制台信息
				BufferedReader in = new BufferedReader(
						new InputStreamReader(socket.getInputStream()));
				PrintWriter out = new PrintWriter(socket.getOutputStream());

				//循环接收和回复客户信息
				String line = null;
				while ((line = 
						in.readLine()) 
						!= null) {
					if (line.equals("bye")) {
						System.out.println("再见");
						break;
					} else {
						System.out.println(line);
						out.println(ins.readLine());
						out.flush();
					}
				}
				ins.close();
				in.close();
				socket.close();
			}
		} catch (Exception e) {

		} finally {
			try {
				server.close();
			} catch (Exception e) {
				e.printStackTrace();
			}
		}
	}
}

#SocketDemo
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;

public class SocketDemo {
	public static void main(String[] args) throws Exception {
		Socket socket = new Socket("localhost", 12345);
		System.out.println("连接上服务器");
		//输入输出流
		BufferedReader sin = new BufferedReader(
				new InputStreamReader(System.in)); //读取控制台数据
		BufferedReader is = new BufferedReader(
				new InputStreamReader(socket.getInputStream()));
		PrintWriter os = new PrintWriter(socket.getOutputStream());
		//循环接收和回复消息
		String responeLine = null;
		while ((responeLine = sin.readLine()) != null) {
			os.println(responeLine);
			os.flush();
			String str = is.readLine();
			System.out.println(str); //print和println 效果完全不一样
		}
		sin.close();
		os.close();
		socket.close();
	}
}
