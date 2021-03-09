---
title: Azure Güvenlik Duvarı Premium önizleme sertifikaları
description: Azure Güvenlik Duvarı Premium önizlemesinde TLS incelemesini doğru şekilde yapılandırmak için ara CA sertifikalarını yapılandırmanız ve kurmanız gerekir.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: conceptual
ms.date: 03/09/2021
ms.author: victorh
ms.openlocfilehash: 621bf6138e4336c63ca137a6a8c54f77a4a99d61
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102520294"
---
# <a name="azure-firewall-premium-preview-certificates"></a>Azure Güvenlik Duvarı Premium önizleme sertifikaları 

> [!IMPORTANT]
> Azure Güvenlik Duvarı Premium Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

 Azure Güvenlik Duvarı Premium önizleme TLS incelemesini doğru şekilde yapılandırmak için, geçerli bir ara CA sertifikası sağlamalı ve Azure Anahtar Kasası 'na yatırmalısınız.

## <a name="certificates-used-by-azure-firewall-premium-preview"></a>Azure Güvenlik Duvarı Premium önizleme tarafından kullanılan sertifikalar

Tipik bir dağıtımda kullanılan üç tür sertifika vardır:

- **Ara CA sertifikası (CA sertifikası)**

   Sertifika yetkilisi (CA), dijital sertifikaları imzalamak için güvenilen bir kuruluştur. Bir CA, bir şirketin veya bir sertifika isteyen kişinin kimliğini ve yasallığını doğrular. Doğrulama başarılı olursa, CA imzalı bir sertifika verir. Sunucu, bir SSL/TLS anlaşması sırasında sertifikayı istemciye (örneğin, Web tarayıcınız) sunduğunda, istemci imzayı *bilinen iyi* imzalayanların bir listesine karşı doğrulamaya çalışır. Web tarayıcıları normalde ana bilgisayarları tanımlamak için örtülü olarak güvendikleri CA listeleriyle birlikte gelir. Yetkili, kendi sertifikalarını imzalayan bazı sitelerde olduğu gibi listede değilse, tarayıcıda kullanıcının sertifikayı tanınan bir yetkili tarafından imzalanmadığını ve doğrulanmamış siteyle iletişime devam etmek isteyip istemediğini sorar.

- **Sunucu sertifikası (Web sitesi sertifikası)**

   Belirli bir etki alanı adıyla ilişkili bir sertifika. Bir Web sitesinin geçerli bir sertifikası varsa, bu, bir sertifika yetkilisinin, Web adresinin gerçekten o kuruluşa ait olduğunu doğrulamak için adımlar gerçekleştirdiğini gösterir. Bir URL yazdığınızda veya güvenli bir Web sitesinin bağlantısını izlediğinizde, tarayıcınız sertifikayı aşağıdaki özelliklerle denetler:
   - Web sitesi adresi, sertifikadaki adresle eşleşiyor.
   - Sertifika, tarayıcının *güvenilir* bir yetkili olarak tanıdığı bir sertifika yetkilisi tarafından imzalanır.
   
   Bazen kullanıcılar güvenilmeyen sertifikayla bir sunucuya bağlanabilir. Sunucu bağlantıyı sonlandırdığından Azure Güvenlik duvarı bağlantıyı bırakacak.

- **Kök CA sertifikası (kök sertifika)**

   Bir sertifika yetkilisi, ağaç yapısı biçiminde birden çok sertifika verebilir. Kök sertifika, ağacın en yüksek sertifikasıdır.

Azure Güvenlik Duvarı Premium önizleme, giden HTTP/S trafiğini ve için bir sunucu sertifikasını otomatik olarak oluşturmayı yakalayabilir `www.website.com` . Bu sertifika, sağladığınız ara CA sertifikası kullanılarak oluşturulur. Son Kullanıcı tarayıcısı ve istemci uygulamaları, bu yordamın çalışması için kuruluşunuzun kök CA sertifikasına veya ara CA sertifikasına güvenmelidir. 

:::image type="content" source="media/premium-certificates/certificate-process.png" alt-text="Sertifika işlemi":::

## <a name="intermediate-ca-certificate-requirements"></a>Ara CA sertifika gereksinimleri

CA sertifikanızın aşağıdaki gereksinimlerle uyumlu olduğundan emin olun:

- Key Vault gizli dizi olarak dağıtıldığında, bir sertifika ve özel anahtarla parola kullanmayan PFX (PKCS12) kullanmanız gerekir.

- Tek bir sertifika olmalıdır ve tüm sertifika zincirini içermemelidir.  

- Bir yıllık ileri için geçerli olmalıdır.  

- En az 4096 bayt boyutunda bir RSA özel anahtarı olmalıdır.  

- `KeyUsage`Uzantının, `KeyCertSign` BAYRAĞıN (RFC 5280; 4.2.1.3 Key Usage) kritik olarak işaretlenmiş olması gerekir.

- `BasicContraints`Uzantının kritik (RFC 5280; 4.2.1.9 Basic kısıtlamaları) olarak işaretlenmiş olması gerekir.  

- `CA`Bayrak true olarak ayarlanmalıdır.

- Yolun uzunluğu bire büyük veya buna eşit olmalıdır.

## <a name="azure-key-vault"></a>Azure Key Vault

[Azure Key Vault](../key-vault/general/overview.md) , gizli dizileri, anahtarları ve TLS/SSL sertifikalarını korumak için kullanabileceğiniz, platform tarafından yönetilen bir gizli depodır. Azure Güvenlik Duvarı Premium, bir güvenlik duvarı Ilkesine eklenen sunucu sertifikaları için Key Vault tümleştirmeyi destekler.
 
Anahtar kasanızı yapılandırmak için:

- Anahtar çiftine sahip mevcut bir sertifikayı anahtar kasanıza aktarmanız gerekir. 
- Alternatif olarak, parola daha az, temel 64 kodlamalı PFX dosyası olarak depolanan bir Anahtar Kasası gizli anahtarını da kullanabilirsiniz.  PFX dosyası, hem özel anahtar hem de ortak anahtar içeren dijital bir sertifikadır.
- Sertifika sona erme tarihini temel alarak bir uyarı yapılandırmanıza izin verdiğinden, CA sertifikası içeri aktarmanın kullanılması önerilir.
- Bir sertifikayı veya parolayı içeri aktardıktan sonra, kimliğe sertifikaya/parolaya erişim izni verilmesini sağlamak için anahtar kasasında erişim ilkeleri tanımlamanız gerekir.
- Azure iş yükünüz tarafından, belirtilen CA sertifikasının güvenilir olması gerekir. Doğru şekilde dağıtıldığından emin olun.

Azure Güvenlik duvarının sizin adınıza Key Vault sertifikaları almak için kullandığı, Kullanıcı tarafından atanan mevcut bir yönetilen kimliği oluşturabilir veya yeniden kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](../active-directory/managed-identities-azure-resources/overview.md) 

## <a name="configure-a-certificate-in-your-policy"></a>İlkenizde bir sertifika yapılandırma

Güvenlik Duvarı Premium ilkenizde bir CA sertifikası yapılandırmak için ilkenizi seçin ve ardından **TLS incelemesi (Önizleme)** öğesini seçin. **TLS İnceleme** sayfasında **etkin** ' i seçin. Ardından, aşağıdaki şekilde gösterildiği gibi Azure Key Vault CA sertifikanızı seçin:

:::image type="content" source="media/premium-certificates/tls-inspection.png" alt-text="Azure Güvenlik Duvarı Premium genel bakış Diyagramı":::
 
> [!IMPORTANT]
> Azure portal bir sertifikayı görmek ve yapılandırmak için, Azure kullanıcı hesabınızı Key Vault erişim ilkesine eklemeniz gerekir. **Gizli izinler** altında Kullanıcı hesabınıza **Al** ve **Listele** ' ye izin verin.
   :::image type="content" source="media/premium-certificates/secret-permissions.png" alt-text="Azure Key Vault erişim ilkesi":::


## <a name="create-your-own-self-signed-ca-certificate"></a>Kendi otomatik olarak imzalanan CA sertifikanızı oluşturma

TLS incelemesini test etmenize ve doğrulamanıza yardımcı olması için, kendi otomatik olarak imzalanan kök SERTIFIKA yetkilinizi ve ara CA 'nizi oluşturmak üzere aşağıdaki komut dosyalarını kullanabilirsiniz.

> [!IMPORTANT]
> Üretim için, ara CA sertifikası oluşturmak üzere Kurumsal PKI 'nizi kullanmanız gerekir. Bir Kurumsal PKI, mevcut altyapıya yararlanır ve kök CA dağıtımını tüm uç nokta makinelerine işler. Daha fazla bilgi için bkz. [Azure Güvenlik Duvarı önizlemesi Için kuruluş CA sertifikalarını dağıtma ve yapılandırma](premium-deploy-certificates-enterprise-ca.md).

Bu betiğin iki sürümü vardır:
- Bash betiği `cert.sh` 
- bir PowerShell betiği `cert.ps1` 

 Ayrıca, her iki betik de `openssl.cnf` yapılandırma dosyasını kullanır. Betikleri kullanmak için `openssl.cnf` , `cert.sh` veya içeriğini `cert.ps1` yerel bilgisayarınıza kopyalayın.

Betikler aşağıdaki dosyaları oluşturur:
- rootCA. CRT/rootCA. anahtar kök CA genel sertifikası ve özel anahtar.
- ınterca. CRT/ınterca. anahtar ara CA genel sertifikası ve özel anahtar
- güvenlik duvarı tarafından kullanılacak ınterca. pfx-ara CA PKCS12 paketi

> [!IMPORTANT]
> rootCA. Key güvenli bir çevrimdışı konumda depolanmalıdır. Betikler 1024 günlük geçerliliği olan bir sertifika oluşturur.

Sertifikalar oluşturulduktan sonra, bunları aşağıdaki konumlara dağıtın:
- rootCA. CRT-uç nokta makinelerinde dağıtma (yalnızca ortak sertifika).
- ınterca. pfx-Key Vault bir sertifika olarak Içeri aktarın ve güvenlik duvarı ilkesine atayın.

### <a name="opensslcnf"></a>**OpenSSL. cnf**
```
[ req ]
default_bits        = 4096
distinguished_name  = req_distinguished_name
string_mask         = utf8only
default_md          = sha512

[ req_distinguished_name ]
countryName                     = Country Name (2 letter code)
stateOrProvinceName             = State or Province Name
localityName                    = Locality Name
0.organizationName              = Organization Name
organizationalUnitName          = Organizational Unit Name
commonName                      = Common Name
emailAddress                    = Email Address

[ rootCA_ext ]
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints = critical, CA:true
keyUsage = critical, digitalSignature, cRLSign, keyCertSign

[ interCA_ext ]
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints = critical, CA:true, pathlen:1
keyUsage = critical, digitalSignature, cRLSign, keyCertSign

[ server_ext ]
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints = critical, CA:false
keyUsage = critical, digitalSignature
extendedKeyUsage = serverAuth
```

###  <a name="bash-script---certsh"></a>Bash betiği-cert.sh 
```bash
#!/bin/bash

# Create root CA
openssl req -x509 -new -nodes -newkey rsa:4096 -keyout rootCA.key -sha256 -days 1024 -out rootCA.crt -subj "/C=US/ST=US/O=Self Signed/CN=Self Signed Root CA" -config openssl.cnf -extensions rootCA_ext

# Create intermediate CA request
openssl req -new -nodes -newkey rsa:4096 -keyout interCA.key -sha256 -out interCA.csr -subj "/C=US/ST=US/O=Self Signed/CN=Self Signed Intermediate CA"

# Sign on the intermediate CA
openssl x509 -req -in interCA.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out interCA.crt -days 1024 -sha256 -extfile openssl.cnf -extensions interCA_ext

# Export the intermediate CA into PFX
openssl pkcs12 -export -out interCA.pfx -inkey interCA.key -in interCA.crt -password "pass:"

echo ""
echo "================"
echo "Successfully generated root and intermediate CA certificates"
echo "   - rootCA.crt/rootCA.key - Root CA public certificate and private key"
echo "   - interCA.crt/interCA.key - Intermediate CA public certificate and private key"
echo "   - interCA.pfx - Intermediate CA pkcs12 package which could be uploaded to Key Vault"
echo "================"
```

### <a name="powershell---certps1"></a>PowerShell-cert.ps1
```powershell
# Create root CA
openssl req -x509 -new -nodes -newkey rsa:4096 -keyout rootCA.key -sha256 -days 3650 -out rootCA.crt -subj '/C=US/ST=US/O=Self Signed/CN=Self Signed Root CA' -config openssl.cnf -extensions rootCA_ext

# Create intermediate CA request
openssl req -new -nodes -newkey rsa:4096 -keyout interCA.key -sha256 -out interCA.csr -subj '/C=US/ST=US/O=Self Signed/CN=Self Signed Intermediate CA'

# Sign on the intermediate CA
openssl x509 -req -in interCA.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out interCA.crt -days 3650 -sha256 -extfile openssl.cnf -extensions interCA_ext

# Export the intermediate CA into PFX
openssl pkcs12 -export -out interCA.pfx -inkey interCA.key -in interCA.crt -password 'pass:'

Write-Host ""
Write-Host "================"
Write-Host "Successfully generated root and intermediate CA certificates"
Write-Host "   - rootCA.crt/rootCA.key - Root CA public certificate and private key"
Write-Host "   - interCA.crt/interCA.key - Intermediate CA public certificate and private key"
Write-Host "   - interCA.pfx - Intermediate CA pkcs12 package which could be uploaded to Key Vault"
Write-Host "================"

```

## <a name="troubleshooting"></a>Sorun giderme

CA sertifikanız geçerliyse, ancak TLS incelemesi altındaki FQDN 'Lere veya URL 'Lere erişemiyorsanız, aşağıdaki öğeleri kontrol edin:

- Web sunucusu sertifikasının geçerli olduğundan emin olun.  

- Kök CA sertifikasının istemci işletim sisteminde yüklü olduğundan emin olun.  

- Tarayıcının veya HTTPS istemcisinin geçerli bir kök sertifika içerdiğinden emin olun. Firefox ve diğer tarayıcıların bazıları özel sertifika ilkelerine sahip olabilir.  

- Uygulama kuralınızın URL hedef türünün doğru yolu ve hedef HTML sayfasına katıştırılmış diğer köprüleri kapsadığından emin olun. Tüm gerekli URL yolunun kolay kapsamı için joker karakterler kullanabilirsiniz.  


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Duvarı Premium özellikleri hakkında daha fazla bilgi edinin](premium-features.md)
