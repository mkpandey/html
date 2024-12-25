import io.zipkin.brave.Tracer;
import io.zipkin.brave.sampler.Sampler;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class TracingConfig {

    @Bean
    public Tracer tracer() {
        return io.zipkin.brave.Brave.newBuilder()
                .localServiceName("your-service-name")
                .sampler(Sampler.ALWAYS_SAMPLE) // You can use different sampling strategies
                .build()
                .tracer();
    }
}


spring.sleuth.enabled=true
spring.sleuth.sampler.probability=1.0
