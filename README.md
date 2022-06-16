# ZimbraSSLinstallation-LetsEncrypt

Ücretli SSL temin ettiğinizde, chain dosyasını dosya içerisinde gösterdiğim adım gibi yapmamalısınız.
Misal GlogalSign'den SSL temin ettiğinizde, öncelikle chain dosyasının içerisine, ilk olarak Intermediate sertifikayı, onun altınada root sertifikayı kopyalamalısınız.
Özetle, Dilerseniz letsencrypt, dilerseniz ücretli SSL temin edin, geri kalan tüm adımlar aynıdır. Ücretli SSL temin ettiyseniz, "chain.pem" dosyası yukarıda belirttiğim gibi yapmanız yeterli olacaktır.

- Intermediate için > https://support.globalsign.com/ca-certificates/intermediate-certificates/alphassl-intermediate-certificates
- Root için > https://support.globalsign.com/ca-certificates/root-certificates/globalsign-root-certificates

> Global Sign için örnek bir sertifika repository'e ekliyorum.
