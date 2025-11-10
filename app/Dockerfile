# OpenTelemetry configuration
ENV OTEL_EXPORTER_OTLP_ENDPOINT=https://otlp-gateway-prod-ap-south-1.grafana.net/otlp/v1/traces
ENV OTEL_EXPORTER_OTLP_METRICS_ENDPOINT=https://otlp-gateway-prod-ap-south-1.grafana.net/otlp/v1/metrics
ENV OTEL_EXPORTER_OTLP_LOGS_ENDPOINT=https://otlp-gateway-prod-ap-south-1.grafana.net/otlp/v1/logs
ENV OTEL_EXPORTER_OTLP_PROTOCOL=http/protobuf
ENV OTEL_LOG_LEVEL=debug

# Use sh -c so the Render environment variable $GRAFANA_API_KEY is expanded at runtime
ENTRYPOINT ["sh", "-c", "java \
  -javaagent:/opentelemetry-javaagent.jar \
  -Dotel.exporter.otlp.endpoint=$OTEL_EXPORTER_OTLP_ENDPOINT \
  -Dotel.exporter.otlp.metrics.endpoint=$OTEL_EXPORTER_OTLP_METRICS_ENDPOINT \
  -Dotel.exporter.otlp.logs.endpoint=$OTEL_EXPORTER_OTLP_LOGS_ENDPOINT \
  -Dotel.exporter.otlp.headers=Authorization=Bearer $OTEL_EXPORTER_OTLP_HEADERS \
  -jar /app.jar"]