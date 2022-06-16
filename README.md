# ZimbraSSLinstallation-LetsEncrypt

Ücretli SSL temin ettiğinizde, chain dosyasını dosya içerisinde gösterdiğim adım gibi yapmamalısınız.
Misal GlogalSign'den SSL temin ettiğinizde, öncelikle chain dosyasının ilk olarak Intermediate sertifikayı, onun altınada root sertifikayı kopyalamalısınız.
Geri kalan adımlar aynıdır. Sadece "chain" dosyasını yukarıda belirttiğim örnek gibi yapılmalıdır.

- Intermediate için > https://support.globalsign.com/ca-certificates/intermediate-certificates/alphassl-intermediate-certificates
- 
- Root için > https://support.globalsign.com/ca-certificates/root-certificates/globalsign-root-certificates

> Global Sign için örnek bir sertifika repository'e ekliyorum.
