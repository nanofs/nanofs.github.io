
注释: Socket-->亦称作"套接字",是java网络编程基础,用来描叙IP地址和端口。

Socket:建立网络连接时使用。
ServerSocket:用于服务器端。

##### 一个简单的单线程的"Client-Server"例子

Server(服务器端):使用ServerSocket监听指定的端口,等待客户连接请求,客户连接后,会话产生.

Client(客户端):使用Socket对服务器的端口发出连接请求,一旦连接成功,打开会话.



**服务端**
```Java
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
public class Server{
public static void main(String []args){
    try{
        ServerSocket ss = new ServerSocket(8099);
        System.out.println("服务器已启动,等到客户端的连接...");
        Socket socket = ss.accept();// -->服务器收到客户端的数据后,创建与此客户端对话的Socket 
        DataInputStream in=new DataInputStream(socket.getInputStream());
        // -->用于接收客户端 发来的数据的输入流
        System.out.println("服务器接受到客户端的连接请求:"+in.readUTF());
        DataOutputStream out = new DataOutputStream(socket.getOutputStream());
        out.writeUTF("接受连接请求,连接成功");
        socket.close();
        ss.close();
    }catch(Exception e){
        e.printStackTrace(); }
    }
}
```

**客户端**
```Java
*************************Client******************************
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.Socket;
public class Client{
public static void main(String []args){
    try{
        Socket socket = new Socket("localhost",8099);
        DataOutputStream out = new DataOutputStream(socket.getOutputStream());
        out.writeUTF("客户端请求服务器的连接...");
        DataInputStream in = new DataInputStream(socket.getInputStream()); 
        System.out.println(in.readUTF()); socket.close();
    }catch(Exception e){
        e.printStackTrace();
    }
} //注释:8099-->自定义端口,最好大于1024均可
```
