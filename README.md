Docker image which echoes various HTTP request properties back to client, as well as in docker logs. 

![browser](https://raw.githubusercontent.com/mendhak/docker-http-https-echo/master/screenshots/screenshot.png)

## Usage

Run with Docker

    docker run -p 8080:80 -p 8443:443 --rm -t mendhak/http-https-echo

Or run with Docker Compose

    docker-compose up

Then, issue a request via your browser or curl, and watch the response, as well as container log output.

    curl -k -X PUT -H "Arbitrary:Header" -d aaa=bbb https://localhost:8443/hello-world


## Use your own certificates

You can substitute the certificate and private key with your own. This example uses the snakeoil cert.

    my-http-listener:
        image: mendhak/http-https-echo
        ports:
            - "8080:80"
            - "8443:443"
        volumes:
            - /etc/ssl/certs/ssl-cert-snakeoil.pem:/app/fullchain.pem
            - /etc/ssl/private/ssl-cert-snakeoil.key:/app/privkey.pem


## Choose your ports

You can choose a different internal port instead of 80 and 443 with the `HTTP_PORT` and `HTTPS_PORT` environment variables. 

In this example I'm setting http to listen on 8888, and https to listen on 9999.  

     docker run -e HTTP_PORT=8888 -e HTTPS_PORT=9999 -p 8080:8888 -p 8443:9999 --rm -t mendhak/http-https-echo


With docker compose, this would be:

    my-http-listener:
        image: mendhak/http-https-echo
        environment: 
            - HTTP_PORT=8888
            - HTTPS_PORT=9999
        ports:
            - "8080:8888"
            - "8443:9999"

## Do not log specific path

Set the environment variable `LOG_IGNORE_PATH` to a path you would like to exclude from verbose logging to stdout. 
This can help reduce noise from healthchecks in orchestration/infrastructure like Swarm, Kubernetes, ALBs, etc. 

     docker run -e LOG_IGNORE_PATH=/ping -e HTTP_PORT=8888 -e HTTPS_PORT=9999 -p 8080:8888 -p 8443:9999 --rm -t mendhak/http-https-echo


With docker compose, this would be:

    my-http-listener:
        image: mendhak/http-https-echo
        environment:
            - LOG_IGNORE_PATH=/ping
        ports:
            - "8080:80"
            - "8443:443"


## Output

#### Curl output

![curl](https://raw.githubusercontent.com/mendhak/docker-http-https-echo/master/screenshots/screenshot2.png)

#### `docker logs` output

![dockerlogs](https://raw.githubusercontent.com/mendhak/docker-http-https-echo/master/screenshots/screenshot3.png)



## Building

    docker build -t mendhak/http-https-echo .


