# ZimbraSSLinstallation-LetsEncrypt

Ücretli SSL temin ettiğinizde, chain dosyasını dosya içerisinde gösterdiğim adım gibi yapmamalısınız.
Misal GlogalSign'den SSL temin ettiğinizde, öncelikle chain dosyasının içerisine, ilk olarak ücretli sertifikaya ait Intermediate sertifikayı, onun altına da ücretli sertifikaya ait root sertifikayı kopyalamalısınız.
Özetle, Dilerseniz letsencrypt, dilerseniz ücretli SSL temin edin, geri kalan tüm adımlar aynıdır. Ücretli SSL temin ettiyseniz, "chain.pem" dosyası yukarıda belirttiğim gibi yapmanız yeterli olacaktır.

- Intermediate için > https://support.globalsign.com/ca-certificates/intermediate-certificates/alphassl-intermediate-certificates
- Root için > https://support.globalsign.com/ca-certificates/root-certificates/globalsign-root-certificates

> Global Sign için örnek bir sertifika repository'e ekliyorum.




# Multiple SSL install

> Zimbra'da birden fazla domain kullanıp, ve bu domainlerde de SSL kullanımı yapacaksak, öncelikle yeni domainimizi Zimbra Arayüzünden eklememiz gerekiyor.

> Ardından, sunucumuza bağlanıp, zimbra kullanıcısına geçip, aşağıdaki komutu yazıyoruz. (domain ismi) - (sslin kurulacağı domain adı) - (zimbra ip adresi)
-  zmprov md yenidomain.com zimbraVirtualHostName mail.yenidomain.com zimbraVirtualIPAddress 192.168.1.11


> Otorite dosyalarımızı birleştirip, bir dosya haline getiriyoruz. Eğer letsencrypt kullanıyorsak, bu dosyalar R3 ve ISRG dosyalarının birleşimi olacaktır.
-  cat yenidomain.com.root.crt yenidomain.com.intermediate.crt >> yenidomain.com_ca.crt

> root kullanıcısına geçip; Dosyalarımızı /tmp dizinine taşıyıp, dosyalarımıza zimbra servisinin okuyabilmesi için yetkiler veriyoruz. En son olarak dosyalarımızın doğruluğunu ve durumunu kontrol ediyoruz.
- cp yenidomain.com.key yenidomain.com.crt yenidomain.com_ca.crt /tmp
- chmod 777 /tmp/example.com/yenidomain.com.key /tmp/example.com/yenidomain.com.crt /tmp/example.com/yenidomain.com_ca.crt
- chown zimbra:zimbra /tmp/example.com/yenidomain.com.key /tmp/example.com/yenidomain.com.crt /tmp/example.com/yenidomain.com_ca.crt 
- /opt/zimbra/bin/zmcertmgr verifycrt comm /tmp/example.com/yenidomain.com.key /tmp/example.com/yenidomain.com.crt /tmp/example.com/yenidomain.com_ca.crt

>  root kullanıcısına geçip, sertifikamızı ve otorite dosyamızı birleştirip, bir dosya haline getiriyoruz. Ardından, zimbra kullanıcına geçip, sertifikalarımızı save ediyoruz. Ancak bunu yapmadan önce, yukarıdaki verdiğimiz yetkilerinin aynısını bu bundle dosyasına da vermemiz gerekiyor.
- cat /tmp/yenidomain.com.crt /tmp/yenidomain.com_ca.crt >> /tmp/yenidomain.com.bundle
- chmod 777 /tmp/yenidomain.com.bundle && chown zimbra:zimbra /tmp/yenidomain.com.bundle
- /opt/zimbra/libexec/zmdomaincertmgr savecrt yenidomain.com /tmp/yenidomain.com.bundle /tmp/yenidomain.com.key

> Sertifikaları kaydettik, şimdi sertifikaları deploy edeceğiz, zimbra kullanıcısına geçip, aşağıdaki komutu çalıştırıyoruz.
-  /opt/zimbra/libexec/zmdomaincertmgr deploycrts

> SNI desteğini açıyoruz.
- zmprov mcf zimbraReverseProxySNIEnabled TRUE

> Zimbra proxy servisini yeniden başlatıyoruz.
-  zmproxyctl restart

İşlemler bu kadar.

Döküman : https://wiki.zimbra.com/wiki/Multiple_SSL_Certificates,_Server_Name_Indication_(SNI)_for_HTTPS

