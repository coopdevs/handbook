 ## List active certs issued for CoopDevs

We can get a list of SSL certificates issued for coopdevs' domains abusing the Certificate Transparency logs:  
  * https://crt.sh/
  * http://www.certificate-transparency.org/

To do that in a programmatic way, we've tweak [this Github project](https://github.com/UnaPibaGeek/ctfr), [adding the ability](https://github.com/coopdevs/ctfr) to only show certificates that are not expired.