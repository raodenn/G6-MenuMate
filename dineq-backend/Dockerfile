# Stage 1: Build the Go app
FROM golang:1.23-alpine AS builder

# Set working directory inside the container
WORKDIR /app

# Copy go.mod and go.sum files to cache dependencies
COPY go.mod go.sum ./

# Download dependencies
RUN go mod download

# Copy the rest of the source code
COPY . .

# Build the binary, output named "app"
RUN go build -o app ./cmd/api

# Stage 2: Create a lightweight image
FROM alpine:latest

# Install ca-certificates (needed for HTTPS calls)
RUN apk --no-cache add ca-certificates

WORKDIR /app

# Copy the compiled binary from the builder stage
COPY --from=builder /app/app .

# Expose the port your app listens on (default 8080)
EXPOSE 8080

# Run the binary
CMD ["./app"]
