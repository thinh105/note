---
id: 0fq7brmku4tzseqryc3yglm
title: Setting Ngrok
desc: ''
updated: 1663409164560
created: 1663409164560
isDir: false
latitude: 21.0313
longitude: 105.8516
altitude: 0
title_imported: Setting ngrok
updated_imported: '2022-03-21T07:29:35.000Z'
created_imported: '2022-03-21T07:19:03.000Z'
---

# Free Setting

run command:

```
ngrok http 8080 -host-header=rewrite
```


# Setting Ianthe

```bash
region: ap
authtoken: 26gZ5EM5R6Jies3851ROVCAeL2G_4xCeV9L7xttvZrE1xpqWf

tunnels:
  store:
    proto: http
    hostname: store-dev.ap.ngrok.io
    addr: localhost:3000
    host_header: localhost:3000

  api:
    proto: http
    hostname: api-dev.ap.ngrok.io
    addr: localhost:3010
    host_header: localhost:3010

  dash:
    proto: http
    hostname: dash-dev.ap.ngrok.io
    addr: localhost:3001
    host_header: localhost:3001
```

run command:

```
ngrok start --all
```


