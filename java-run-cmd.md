# Java에서 Command Line 명령어 실행

- Java에서 wkhtmltoimage를 실행시키기 위해 외부 프로그램 실행 샘플 코드를 찾았다.


```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

public class RunProgram {

    public static void main(String[] args) {
       
        Runtime run = Runtime.getRuntime();
        Process p;
        String strCmd = "wkhtmltopdf http://google.com google.pdf";
        try {
            p = run.exec(strCmd);
        
            StreamPrintThread errprint = new StreamPrintThread(p.getErrorStream());
            StreamPrintThread okprint = new StreamPrintThread(p.getInputStream());
            p.getOutputStream().close();
            
            errprint.start();
            okprint.start(); 
    
            int rst = p.waitFor();
            if ( 0 == rst) {
                System.out.println("RunProgram success : " + strCmd);
            }
            else {
                System.out.println("RunProgram fail (rst:"+ rst +") : " + strCmd);
            }

        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}


class StreamPrintThread extends Thread {
    BufferedReader br = null;
    
    private StreamPrintThread() {}
    
    public StreamPrintThread(InputStream is) {
        br = new BufferedReader(new InputStreamReader(is));
    }
    
    void close() {
        try {
            if(br != null)
                br.close();
        }
        catch(Exception e) {
            e.printStackTrace();
            System.out.println(e);
        }
    }
    
    public void run() {
        
        try {
            String line = null;
            
            while((line = br.readLine()) != null) {
                System.out.println(line);
            }
        }
        catch(Exception e) {
            e.printStackTrace();
            System.out.println(e);
        }
        finally {
            close();
        }
    }
}
```

- 해당 코드를 활용해서 명령을 실행하고 정상 실행 여부도 확인할 수 있었다.
