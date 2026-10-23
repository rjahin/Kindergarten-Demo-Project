FROM golang as build 

WORKDIR /app

COPY . .

RUN go mod tidy

EXPOSE 5001

CMD ["go", "run", "main.go"]