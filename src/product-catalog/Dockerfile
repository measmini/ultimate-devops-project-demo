#Mutistage Docker build to follow Devsecops practice.
#Build the Go binary in stage1

FROM golang:1.22-alpine AS builder

#Set the working directory inside a container
WORKDIR /usr/src/app

#Copy go.mod and go.sum to leverage Docker's layer caching.
COPY go.mod go.sum ./

#Download dependancies 
RUN go mod download

# Copy rest of the source code.
COPY . .

#Build The Go App
RUN go build -o product-catalog .

#Create a lightweight runtime container in stage 2 by copying the build image in stage1 
FROM alpine AS release

WORKDIR /usr/src/app 

COPY ./products ./products/ 

#Copy the built binary from the builder stage
COPY --from=builder /usr/src/app/product-catalog/ ./

RUN chmod +x product-catalog

#App export to unused port 

ENV PRODUCT_CATALOG_PORT=8088

ENTRYPOINT [ "./product-catalog" ]

