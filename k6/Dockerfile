# Use the official Go image to build k6 with the InfluxDB v2 extension
FROM golang:1.23 AS builder

# Install xk6
RUN go install go.k6.io/xk6/cmd/xk6@latest

# Build k6 with the xk6-output-influxdb extension
RUN xk6 build latest \
    --with github.com/grafana/xk6-output-influxdb@latest \
    --output /k6

# Use a minimal base image for the final container
FROM alpine:3.20

# Install ca-certificates for secure HTTP requests
RUN apk add --no-cache ca-certificates

# Copy the built k6 binary
COPY --from=builder /k6 /usr/bin/k6

# Set the entrypoint to k6
ENTRYPOINT ["k6"]