keytool -import -trustcacerts -alias server_cert -file server.pem -keystore my_truststore.jks
@Configuration
public class RestTemplateConfig {

    @Bean
    public RestTemplate restTemplate(TrustManager[] trustManagers) throws Exception {
        SSLContext sslContext = SSLContext.getInstance("TLS");
        sslContext.init(null, trustManagers, null);

        SSLConnectionSocketFactory socketFactory = new SSLConnectionSocketFactory(sslContext);
        HttpClient httpClient = HttpClientBuilder.create()
            .setSSLSocketFactory(socketFactory)
            .build();

        return new RestTemplateBuilder()
            .setHttpClient(httpClient)
            .build();
    }

    @Bean
    public TrustManager[] trustManagers() throws Exception {
        KeyStore trustStore = KeyStore.getInstance("JKS");
        trustStore.load(new FileInputStream("my_truststore.jks"), "password".toCharArray());

        TrustManagerFactory trustManagerFactory = TrustManagerFactory.getInstance(TrustManagerFactory.getDefaultAlgorithm());
        trustManagerFactory.init(trustStore);

        return trustManagerFactory.getTrustManagers();
    }
}
