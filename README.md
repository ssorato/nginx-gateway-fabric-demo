# NGINX Gateway Fabric demo

[F5 NGINX Gateway Fabric](https://docs.nginx.com/nginx-gateway-fabric/)

[Gateway API](https://gateway-api.sigs.k8s.io/)

## Requirements

[The Modern Task Runner](https://taskfile.dev/)

## Deploy

```bash
task
```

## Test endpoints

```bash
DEMO_URL="http://demo.`ifconfig > /dev/null 2>&1 && ifconfig | grep 'inet ' | grep -v '127.0.0.1' | awk '{print $2}' | head -1 || hostname -I | cut -d" " -f 1`.nip.io"
```

Root endpoint:

```bash
curl -skL $DEMO_URL/
```

Blue-green endpoint:

```bash
blue=0
green=0
for (( i=1; i<=100; i++ ))
do
  response=$(curl -skL $DEMO_URL/blue-green)
  echo $response | grep blue > /dev/null 2>&1
  if [ $? -eq 0 ]
  then
    blue=$((blue+1))
  else 
    green=$((green+1))
  fi
  echo "[$i] $response"
  echo "blue  = $blue%"
  echo "green = $green%"
done
```

VIP endpoint:

```bash
curl -skL -H 'X-User-Type: vip' $DEMO_URL/vip
```

## Clenup

```bash
task destroy
```
