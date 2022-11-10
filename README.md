# Nginx Playground

A repo for trying out Nginx configurations.


## Using this Repo


### (Optional) Setup Hosts File Redirect for Example.com

This step is optional, but it makes the testing feel a little more realistic by
allowing you to reference `example.com` instead of just
`localhost`.

Edit your `/etc/hosts` file and add the following line to the bottom:

```txt
127.0.0.1       example.com
```


### Run the Nginx Docker Container

Use docker-compose to run the Nginx docker container and view the logs:

```sh
docker-compose up
```

**Note**: That docker-compose command does not use the detach (`--detach` or
`-d`) argument, so you will need to open other terminals to run the curl and
netcat commands below.


### Curl Requests

If you didn't do the optional hosts file override above, you can switch
`example.com` to `localhost`.

The default example Nginx configuration in this repo proxies all requests to
http://jsonplaceholder.typicode.com which is a free, fake API for testing and
prototyping.

See https://jsonplaceholder.typicode.com/ for the full list of API resources
you can interact with.

- Get all posts

```sh
curl example.com/posts
```


### Check Nginx Proxied Values with Netcat

If you would like to test different Nginx configurations to see what url paths
and other information get proxied from Nginx to the upstream server, you can
run a local netcat server and update the proxy_pass to point at that.

- In a new shell, start a local netcat server on port 3000:

```sh
nc -l 127.0.0.1 3000
```

- Update the `nginx.conf` file to comment out the JSON Placeholder proxy_pass
  url and enable the localhost:3000 proxy_pass url
- In your nginx shell, restart nginx by killing the docker-compose up session
  and re-running it

```sh
docker-compose up
```

- Run the curl requests above to see what information is sent from Nginx to the
  upstream server(s). You'll need to restart the Netcat server for each request.
