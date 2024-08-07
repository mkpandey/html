import org.springframework.http.client.reactive.ClientHttpConnector;
import org.springframework.http.client.reactive.ReactorClientHttpConnector;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.netty.http.client.HttpClient;
import reactor.netty.tcp.SslContext;
import reactor.netty.tcp.SslProvider;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.security.KeyStore;
import java.security.cert.CertificateFactory;
import java.security.cert.X509Certificate;

// ...

public class MyService {

    private final WebClient webClient;

    public MyService() throws IOException {
        // Load .pem and .key files
        String pemContent = new String(Files.readAllBytes(Paths.get("your_cert.pem")));
        String keyContent = new String(Files.readAllBytes(Paths.get("your_key.pem")));

        // Create KeyStore
        KeyStore keyStore = KeyStore.getInstance("PKCS12");
        keyStore.load(null, null);

        // Add certificate to KeyStore
        CertificateFactory certFactory = CertificateFactory.getInstance("X.509");
        X509Certificate cert = (X509Certificate) certFactory.generateCertificate(new ByteArrayInputStream(pemContent.getBytes()));
        keyStore.setCertificateEntry("mycert", cert);

        // Add private key to KeyStore (you'll need to provide the key password)
        // ...

        // Create SSL Context
        SslContext sslContext = SslContext.newDefault()
                .keyStoreForClient(keyStore, "your_key_password".toCharArray())
                .build();

        // Create WebClient
        ClientHttpConnector connector = new ReactorClientHttpConnector(HttpClient.create().secure(sslContext));
        webClient = WebClient.builder()
                .baseUrl("https://your-api-endpoint")
                .clientConnector(connector)
                .build();
    }

    public String fetchData() {
        return webClient.get()
                .uri("/data")
                .retrieve()
                .bodyToMono(String.class)
                .block();
    }
}
