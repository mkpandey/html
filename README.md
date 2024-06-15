

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.client.ClientHttpRequestFactory;
import org.springframework.http.client.SimpleClientHttpRequestFactory;
import org.springframework.web.client.RestTemplate;

import javax.net.ssl.SSLContext;
import javax.net.ssl.TrustManagerFactory;
import java.io.FileInputStream;
import java.security.KeyStore;
import java.security.cert.CertificateFactory;

@Configuration
public class SSLConfiguration {

    @Bean
    public RestTemplate restTemplate() throws Exception {
        // Load the .pem certificate
        CertificateFactory cf = CertificateFactory.getInstance("X.509");
        FileInputStream fileInputStream = new FileInputStream("path/to/your.pem");
        Certificate cert = cf.generateCertificate(fileInputStream);
        fileInputStream.close();

        // Load the .key file
        KeyStore keyStore = KeyStore.getInstance("PKCS12");
        FileInputStream keyInput = new FileInputStream("path/to/your.key");
        keyStore.load(keyInput, "yourKeyPassword".toCharArray());
        keyInput.close();

        // Create and initialize the SSLContext
        TrustManagerFactory tmf = TrustManagerFactory.getInstance(TrustManagerFactory.getDefaultAlgorithm());
        tmf.init((KeyStore) null);
        SSLContext sslContext = SSLContext.getInstance("TLS");
        sslContext.init(null, tmf.getTrustManagers(), null);

        // Create a ClientHttpRequestFactory with the custom SSLContext
        SimpleClientHttpRequestFactory requestFactory = new SimpleClientHttpRequestFactory();
        requestFactory.setReadTimeout(5000);
        requestFactory.setConnectTimeout(5000);
        requestFactory.setSslContext(sslContext);

        // Create a RestTemplate using the custom ClientHttpRequestFactory
        return new RestTemplate(requestFactory);
    }
}
