FROM goland:1.23

WORKDIR /app

COPY go.mod go.sum .

RUN go mod tidy 

COPY . .

EXPOSE 5001

CMD ["go", "run", "main.go"]
