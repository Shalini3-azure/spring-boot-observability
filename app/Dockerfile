FROM curlimages/curl:8.13.0 AS download
ARG OTEL_AGENT_VERSION="2.16.0"
RUN curl --silent --fail --insecure -L "https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/download/v${OTEL_AGENT_VERSION}/opentelemetry-javaagent.jar" \
    -o "$HOME/opentelemetry-javaagent.jar"

FROM maven:3-amazoncorretto-24-alpine AS build
WORKDIR /build
COPY . .
RUN mvn clean package -DskipTests --quiet

FROM amazoncorretto:24-alpine3.21
COPY --from=build /build/target/*.jar /app.jar
COPY --from=download /home/curl_user/opentelemetry-javaagent.jar /opentelemetry-javaagent.jar

# Set OpenTelemetry environment variables
ENV OTEL_EXPORTER_OTLP_ENDPOINT=https://otlp-gateway-prod-ap-south-1.grafana.net/otlp/v1/traces
ENV OTEL_EXPORTER_OTLP_METRICS_ENDPOINT=https://otlp-gateway-prod-ap-south-1.grafana.net/otlp/v1/metrics
ENV OTEL_EXPORTER_OTLP_LOGS_ENDPOINT=https://otlp-gateway-prod-ap-south-1.grafana.net/otlp/v1/logs
ENV OTEL_EXPORTER_OTLP_PROTOCOL=http/protobuf
ENV OTEL_EXPORTER_OTLP_HEADERS="Authorization=Bearer $OTEL_EXPORTER_OTLP_HEADERS"
ENV OTEL_LOG_LEVEL=debug
# ENTRYPOINT ["java", \
#   "-javaagent:/opentelemetry-javaagent.jar", \
#   "-jar", "/app.jar" \
#   ]
# Runtime ENTRYPOINT
ENTRYPOINT ["sh", "-c", "java \
  -javaagent:/opentelemetry-javaagent.jar \
  -Dotel.exporter.otlp.endpoint=https://otlp-gateway-prod-ap-south-1.grafana.net/otlp/v1/traces \
  -Dotel.exporter.otlp.headers=Authorization=Bearer $GRAFANA_API_KEY \
  -jar /app.jar"]