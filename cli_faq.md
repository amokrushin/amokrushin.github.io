# CLI FAQ

- #### Test port is open without telnet
  ```sh
  cat < /dev/tcp/localhost/5680
  ```
  
# Docker Service Logs

```
cat ./cf1cfdb0f2d3fcedc1be722a5d5c2802b5d2bf11d1752314f14f44a65fea4575-json.log.2 | jq '.log' -rj | less -R
```
