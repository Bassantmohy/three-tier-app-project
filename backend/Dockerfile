#builder stage 
FROM golang:1.25.3-alpine As builder

WORKDIR /app

COPY go.mod go.sum ./

RUN go mod download

COPY . .

RUN go build -v -o app .

#Run stage
FROM alpine

WORKDIR /app

COPY --from=builder /app/app .

CMD [ "./app" ]