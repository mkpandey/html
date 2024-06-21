


Here's the modified PL/SQL code with a hardcoded array of country names:
DECLARE
  -- Array of country names (replace with your actual countries)
  country_names VARCHAR2(20) := 'India,USA,China,France,Germany,Japan,Canada,UK,Australia,Brazil';
  
  l_country_name VARCHAR2(20);
  
  -- Assuming the new table has a 'name' column for country names 
  new_table_insert new_table%ROWTYPE;
BEGIN
  -- Split the comma-separated string into an array
  country_names := REPLACE(country_names, CHR(10), ',');  -- Remove line breaks if present
  FOR l_country_name IN SPLIT(country_names, ',') LOOP
    -- Insert logic for the new table with name
    new_table_insert.name := l_country_name;
    INSERT INTO new_table VALUES new_table_insert;
  END LOOP;
  COMMIT;
END;

Changes made:
 * We declare a VARCHAR2 variable country_names to hold the comma-separated list of countries. Replace the list with your actual countries.
 * We declare another VARCHAR2 variable l_country_name to hold each individual country name during the loop.
 * We use the SPLIT function to split the country_names string into an array based on the comma delimiter.
 * We use a loop to iterate through each element in the split array (l_country_name).
 * Inside the loop, we create a record variable (new_table_insert) of the new_table type and assign the current country name to the name column.
 * We insert the record into the new_table.
 * We commit the changes at the end.
Note:
 * This code assumes the new table has a column named name to store the country names. Modify it if your column name is different.
 * Make sure the maximum length of the country names doesn't exceed the VARCHAR2(20) size defined. Adjust as needed.













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
