## Proxy to upstream without path rewriting

```
location /proxy {
  proxy_pass http://up;
}
```

`http://localhost:80/proxy/here/1` => `http://up/proxy/here/1`


## Proxy to upstream with path rewriting

```
location /proxy {
  proxy_pass http://up/another/prefix;
}
```

`http://localhost:80/proxy/here/1` => `http://up/another/refix/here/1`

## Handle redirects from upstream

```
server {
  listen 8091;

  location /proxy/redirect {
    return 302 'http://0.0.0.0:8091/final';
  }

  location /final {
    return 200 'Final';
  }
}

server {
  listen 8092;

  location /proxy {
    proxy_pass http://0.0.0.0:8091;

    proxy_intercept_errors on;
    error_page 301 302 307 = @handle_redirect;
  }

  location @handle_redirect {
    set $redirect_location $upstream_http_location;

    proxy_pass $redirect_location;
  }
}
```

`http://localhost:8092/proxy/redirect` => `200 Final`

## Misc

* `proxy_max_temp_file_size 0` – only in-memory proxying
* `proxy_pass_request_headers off` – do not pass any of the headers from the original request to the upstream
