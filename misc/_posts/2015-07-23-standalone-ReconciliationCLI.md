import java.io.IOException;

import javax.xml.bind.JAXBException;

import com.voiinnov.dwlc.yeepay.YeePayDeal;
import com.voiinnov.dwlc.yeepay.direct.info.AccountInfo;
import com.voiinnov.dwlc.yeepay.direct.param.AccountParam;

// javac -cp ".:/home/dwlc/webapps/dwlc/WEB-INF/lib/*" ReconciliationCLI.java
// java -cp ".:/home/dwlc/webapps/dwlc/WEB-INF/lib/*" ReconciliationCLI
public class ReconciliationCLI {
    public static void main(String[] args) {
        try {
            AccountInfo accountInfo = findaccount("20748");
            System.out.println(accountInfo.getAvailableAmount());
        } catch (Exception e) {
            System.out.println("Error:" + e.getMessage());
        }
    }

    public static AccountInfo findaccount(String platformUserNo) throws IOException, JAXBException {
        AccountParam ac = new AccountParam();
        ac.setPlatformUserNo(platformUserNo);
        YeePayDeal yp = new YeePayDeal();
        AccountInfo accountInfo = yp.accountQuery(ac);
        return accountInfo;
    }
}


date "+%s.%N"; java -cp ".:/home/dwlc/tomcat_be_dev/ROOT/WEB-INF/lib/*" ReconciliationCLI 20748
启动 JAVA 需要 181毫秒，访问易宝需要 2.5秒。