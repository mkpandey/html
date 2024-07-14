
<dependency>
    <groupId>org.bouncycastle</groupId>
    <artifactId>bcprov-jdk15on</artifactId>
    <version>1.90</version>
</dependency>


import java.io.FileReader; // For reading from the PEM file
import org.bouncycastle.openssl.PEMReader; // For reading PEM-encoded data (from Bouncy Castle)
import javax.net.ssl.SSLContext; // For configuring SSL context
import javax.net.ssl.X509TrustManager; // For custom trust management (optional)
import javax.net.ssl.TrustManagerFactory; // For creating TrustManager instances
import javax.net.ssl.SSLConnectionSocketFactory; // For creating SSL socket factory
import org.apache.http.impl.client.HttpClientBuilder; // For building HttpClient
import org.springframework.web.client.RestTemplate;

@Configuration
public class RestTemplateConfig {

    @Bean
    public RestTemplate restTemplate() throws Exception {
        SSLContext sslContext = SSLContext.getInstance("TLS");

        PEMReader reader = new PEMReader(new FileReader("server.pem"));
        X509Certificate certificate = (X509Certificate) reader.readObject();
        reader.close();

        TrustManagerFactory trustManagerFactory = TrustManagerFactory.getInstance(TrustManagerFactory.getDefaultAlgorithm());
        trustManagerFactory.init(null, new TrustManager[] {new X509TrustManager() {
            @Override
            public void checkClientTrusted(X509Certificate[] chain, String authType) throws CertificateException {
                // Handle client-side trust verification (optional)
            }

            @Override
            public void checkServerTrusted(X509Certificate[] chain, String authType) throws CertificateException {
                // Implement custom trust verification logic here (optional)
            }

            @Override
            public getAcceptedIssuers() {
                return new X509Certificate[]{certificate};
            }
        }});

        TrustManager[] trustManagers = trustManagerFactory.getTrustManagers();
        sslContext.init(null, trustManagers, null);

        SSLConnectionSocketFactory socketFactory = new SSLConnectionSocketFactory(sslContext);
        HttpClient httpClient = HttpClientBuilder.create()
            .setSSLSocketFactory(socketFactory)
            .build();

        return new RestTemplateBuilder()
            .setHttpClient(httpClient)
            .build();
    }
}
