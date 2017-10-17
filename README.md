# vpnc
Cisco vpn client

## Environment Variables

- `VPNC_GATEWAY`: IP/name of your IPSec gateway
- `VPNC_ID`: Group name
- `VPNC_SECRET`: Group password
- `VPNC_USERNAME`: XAUTH username
- `VPNC_PASSWORD`: XAUTH password

## Example

```
docker run -d --net=host --privileged=true --name vpn --restart always -e "VPNC_GATEWAY=1.1.1.1" -e "VPNC_PASSWORD=sdfadsdf" -e "VPNC_ID=vpn-id" -e "VPNC_SECRET=asdfasdf" -e "VPNC_USERNAME=myuser" jsecchiero/vpnc
```
