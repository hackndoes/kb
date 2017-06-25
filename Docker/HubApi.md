# List Tags in Repository V2
API calls with the GET method

## V2

### Authentication
``` bash
curl https://auth.docker.io/token\?service="registry.docker.io"\&scope="repository:repositories/<scope>/<repository>":pull;push
```

## List Tags in Repository
``` bash
curl -H 'Accept: application/json' -H 'Authorization: Bearer <requestedToken>' https://index.docker.io/v1/repositories/<namespace>/<repo>/tags/list
```
