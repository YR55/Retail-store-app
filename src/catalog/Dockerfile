# =========================
# Build Stage
# =========================
FROM golang:1.23-alpine AS build

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN go build -o main main.go


# =========================
# Runtime Stage
# =========================
FROM alpine:3.20

WORKDIR /app

COPY --from=build /app/main .

ENV GIN_MODE=release

EXPOSE 8080

ENTRYPOINT ["/app/main"]