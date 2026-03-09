# ==========================
# Build Stage!
# =========================
FROM golang:1.23-alpine AS build

# SQLite + CGO ke liye required packages
RUN apk add --no-cache gcc musl-dev

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .

# CGO enable karke build
ENV CGO_ENABLED=1
RUN go build -o main main.go


# =========================
# Runtime Stage
# =========================
FROM alpine:3.20

# SQLite runtime dependency
RUN apk add --no-cache libc6-compat

WORKDIR /app

COPY --from=build /app/main .

ENV GIN_MODE=release

EXPOSE 8080

ENTRYPOINT ["/app/main"]
