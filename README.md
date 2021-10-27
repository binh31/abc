# abc
import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

/**
 *
 * @author phamb_v9k01k6
 */
public class JavaApplication30 {

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) throws Exception{
        // TODO code application logic here
        try (ServerSocket serverSocket =new ServerSocket(8080)){
            System.out.println("Server started. \n listening for messages.");
            while(true){
                try(Socket client = serverSocket.accept()){
//                    System.out.println("massages:" + client.toString());
                    InputStreamReader isr=new InputStreamReader(client.getInputStream());
                    
                    BufferedReader br=new BufferedReader(isr);
                    StringBuilder request =new StringBuilder();
                    String line;
                    line=br.readLine();
                    while(!line.isBlank()){
                        request.append(line+"\r\n");
                        line =br.readLine();
                    }
//                    System.out.println("--request--");
//                    System.out.println("  re "+request);
//                    
                 
                    
                    String firstline =request.toString().split("\n")[0];
                    String resource = firstline.split(" ")[1];
                    System.out.println(resource);
                    if(resource.equals("/anh")){
                         FileInputStream image =new FileInputStream("anh.jpg");
                        OutputStream clientOutput =client.getOutputStream();
                        clientOutput.write(("HTTP/1.1 200 ok\r\n").getBytes());
                        clientOutput.write(("\r\n").getBytes());
                        clientOutput.write((image.readAllBytes()));
                        clientOutput.close();
                    }else if(resource.equals("/tl")){
                         FileInputStream image =new FileInputStream("tl.html");
                        OutputStream clientOutput =client.getOutputStream();
                        clientOutput.write(("HTTP/1.1 200 ok\r\n").getBytes());
                        clientOutput.write(("\r\n").getBytes());
                        clientOutput.write((image.readAllBytes()));
                        clientOutput.close();
                    }else{
                         OutputStream clientOutput =client.getOutputStream();
                        clientOutput.write(("HTTP/1.1 200 ok\r\n").getBytes());
                        clientOutput.write(("\r\n").getBytes());
                        clientOutput.write(("hello").getBytes());
                        clientOutput.close();
                    }
//                    OutputStream clientOutput =client.getOutputStream();
//                    clientOutput.write(("HTTP/1.1 200 ok\r\n").getBytes());
//                    clientOutput.write(("\r\n").getBytes());
//                    clientOutput.write((image.readAllBytes()));
//                    clientOutput.flush();
                    
                    
                    client.close();
                }
            }
        }
        
    }
    
}
