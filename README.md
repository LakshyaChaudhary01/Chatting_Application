# Chatting_Application
Backend of a chatting application


creating Server


package chatting_application;

import javax.swing.*;
import java.awt.*;
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class server extends  JFrame {

    static ServerSocket server;
    Socket socket;
    BufferedReader br;
    PrintWriter out;
    private JLabel heading= new JLabel("clint area");
    private  JTextArea msgArea= new JTextArea();
    private JTextField msgInput= new JTextField();
    private  Font font= new Font("Roboto",Font.PLAIN,20);

    public void server(){
        try {
            server=  new ServerSocket(7777);
            System.out.println("server is ready to execpt connection");
            System.out.println("waiting...");
            socket= server.accept();

            br=new BufferedReader(new InputStreamReader(socket.getInputStream()));
            out= new PrintWriter(socket.getOutputStream());

            createGUI();
            StartReading();
            StartWriting();
        }
        catch (Exception e){
            e.printStackTrace();
        }
    }

    private  void createGUI(){
        this.setTitle("Clint Messager");
        this.setSize(500,500);
        this.setLocation(null);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
    public void StartReading(){
        //help us to read the data

        Runnable r1=()->{
            System.out.println(("reader started..."));
            while (true){
                try {
                    String msg=  br.readLine();
                    if (msg.equals(("exit"))){

                        System.out.println("clint terminated the chat");
                        break;
                    }
                    System.out.println("clint :"+ msg);

                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        };
        new Thread(r1).start();
    }
    public void StartWriting(){
        //help us to write and send the data
        Runnable r2= ()->{
            System.out.println("writer started...");
            while (true){
                try {
                    BufferedReader br1= new BufferedReader((new InputStreamReader(System.in)));
                    String  content=br1.readLine();
                    out.println(content);
                    out.flush();

                }
                catch (Exception e){
                    e.printStackTrace();
                }
            }
        };
        new Thread(r2).start();
    }

    public static void  main(String []args){
        System.out.println("this is server");
        new server();
    }
}


