## Debug message
```
location /api/ {
    add_header X-Debug-Message $destination always;
}
```

## Proxy by request method

```
map $request_method $destination {
    default     service-a:80;
    POST        service-b:8000;
    PUT         service-b:8000;
    PATCH       service-b:8000;
    DELETE      service-b:8000;
}

server {
    ...
    location /rest/ {
        proxy_pass http://$destination;
    }
}
```
