import io.opentelemetry.api.trace.Tracer;
import io.opentelemetry.api.trace.propagation.W3CTraceContextPropagator;
import io.opentelemetry.context.propagation.ContextPropagators;
import io.opentelemetry.exporter.otlp.trace.OtlpGrpcSpanExporter;
import io.opentelemetry.sdk.trace.SdkTracerProvider;
import io.opentelemetry.sdk.trace.export.BatchSpanProcessor;
import io.opentelemetry.sdk.trace.samplers.AlwaysOnSampler;

// Configure the OTLP exporter
OtlpGrpcSpanExporter spanExporter = OtlpGrpcSpanExporter.builder()
    .setEndpoint("http://localhost:4317") // Replace with your collector endpoint
    .build();

// Create a TracerProvider
SdkTracerProvider tracerProvider = SdkTracerProvider.builder()
    .setSampler(AlwaysOnSampler.getInstance()) // Sample all traces
    .addSpanProcessor(BatchSpanProcessor.builder(spanExporter).build())
    .build();

// Build the tracer
io.opentelemetry.api.OpenTelemetry openTelemetry = io.opentelemetry.api.OpenTelemetry.builder()
    .setTracerProvider(tracerProvider)
    .setPropagators(ContextPropagators.create(W3CTraceContextPropagator.getInstance()))
    .build();

Tracer tracer = openTelemetry.getTracer("your-instrumentation-library-name", "1.0.0");
