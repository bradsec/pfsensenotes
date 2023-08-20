## pfSense Configuration Notes

### Dynamic DNS with Cloudflare Managed Domain

- [x] Working with pfSense 2.7.0-RELEASE as of 20 August 2023
- [x] Cloudflare proxy protection can also be enabled on DNS record.

**IN CLOUDFLARE ACCOUNT**

1. In CloudFlare account select Domain name then select `DNS > Records`
2. `Add Record` (Name below can be any preferred subdomain, example is ddns)
```terminal
Type    Name  IPv4 address (required)                        Proxy status (can be enabled)  
A       ddns  {any placeholder IPV4 address example 1.1.1.1} Enabled
```
3. In Cloudflare account goto `My Profile` and select `API Tokens`
4. Copy the `Global API Key`

- Have not tested with custom API keys, this may be a more secure option rather than using the Global API Key.

**IN PFSENSE**

1. Goto `Services > Dynamic DNS`
2. Fill in details:
```terminal
Disable             [UNTICK] Disable this client
Service Type:       CloudFlare
Hostname:           ddns        yourcloudflaredomainname.com
Cloudflare Proxy:   [TICK] Enable Proxy

## IMPORTANT - Despite text - Username is required for all providers except Cloudflare (it is required pfSense 2.7.0)
Username:           {Enter CloudFlare account management email address}
Password:           {Paste in Global API Key} 
```
3. Click `Save` or `Save & Force Update`

**TROUBLESHOOTING**

I found if you leave the Username blank it will fail and the System Log will show a message similar to below. 

`/services_dyndns_edit.php: Response Data: {"success":false,"errors":[{"code":6003,"message":"Invalid request headers","error_chain":[{"code":6111,"message":"Invalid format for Authorization header"}]}],"messages":[],"result":null}`

