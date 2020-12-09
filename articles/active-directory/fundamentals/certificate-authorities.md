---
title: Azure Active Directory Sertifika yetkilileri
description: Azure 'da kullanılan güvenilir sertifikaların listelenmesi
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: baselden
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: bea6f4161bcca063cd2c58d4c463473426f159ba
ms.sourcegitcommit: 21c3363797fb4d008fbd54f25ea0d6b24f88af9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2020
ms.locfileid: "96859140"
---
# <a name="certificate-authorities-used-by-azure-active-directory"></a>Azure Active Directory tarafından kullanılan sertifika yetkilileri

> [!IMPORTANT]
> Bu sayfadaki bilgiler yalnızca, kabul edilebilir sertifika yetkilileri (CA 'Lar) listesini açıkça belirten varlıklar için geçerlidir. Sertifika sabitleme olarak bilinen bu uygulama, başka bir seçenek olmadığı sürece kaçınılmalıdır.

TLS/SSL protokolleri aracılığıyla Azure Active Directory (Azure AD) kimlik hizmetlerine erişmeye çalışan herhangi bir varlık, aşağıda listelenen CA 'lardan sertifikalarla birlikte sunulacaktır. Varlık bu CA 'Lara güveniyorsa, kimlik hizmetlerinin kimliğini ve yasallığını doğrulamak ve güvenli bağlantılar oluşturmak için sertifikaları kullanabilir.

Sertifika yetkilileri, kök CA 'Lar ve ara CA 'Lar halinde sınıflandırılabilir. Genellikle kök CA 'Ların bir veya daha fazla ilişkili ara CA 'Sı vardır. Bu makalede, Azure AD kimlik hizmetleri tarafından kullanılan kök CA 'Ları ve bu köklerin her biriyle ilişkili ara CA 'Lar listelenmektedir. Her CA için, ilişkili yetkili bilgi erişimi (AIA) ve sertifika Iptal listesi dağıtım noktası (CDP) dosyalarını indirmek üzere Tekdüzen Kaynak tanımlayıcıları (URI 'Ler) dahil ediyoruz. Uygun olduğunda, çevrimiçi sertifika durumu Protokolü (OCSP) uç noktası için de bir URI sunuyoruz.

## <a name="cas-used-in-azure-public-and-azure-us-government-clouds"></a>Azure Kamu ve Azure ABD kamu bulutlarında kullanılan CA 'Lar

Farklı hizmetler, farklı kök veya ara CA 'Lar kullanabilir. Bu nedenle, aşağıda listelenen tüm girişler gerekli olabilir.

### <a name="digicert-global-root-g2"></a>DigiCert genel kök G2


| Kök CA| Seri Numarası| Sorun tarihi bitiş tarihi| SHA1 parmak Izi| URI'ler |
| - |- |-|-|-|-|
| DigiCert genel kök G2| 033af1e6a711a 9a0bb2864b11d09fae5| 1 Ağustos 2013 <br>15 Ocak 2038| df3c24f9bfd666761b268 073fe06d1cc8d4f82a4| [AIA](http://cacerts.digicert.com/DigiCertGlobalRootG2.crt)<br>['SI](http://crl3.digicert.com/DigiCertGlobalRootG2.crl) |


#### <a name="associated-intermediate-cas"></a>İlişkili ara CA 'Lar

| Verme ve ara CA| Seri Numarası| Sorun tarihi bitiş tarihi| SHA1 parmak Izi| URI'ler |
| - | - | - | - | - | 
| Microsoft Azure TLS veren CA 01| 0aafa6c5ca63c45141 ea3be1f7c75317| 29 Temmuz 2020<br>27 Haziran 2024| 2f2877c5g778c31e0f29c 7e371df5471bd673173| [AIA](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2001%20-%20xsign.crt)<br>['SI](https://www.microsoft.com/pkiops/crl/Microsoft%20Azure%20TLS%20Issuing%20CA%2001.crl)|
|Microsoft Azure TLS veren CA 02| 0c6ae97cced59983 8690a00a9ea53214| 29 Temmuz 2020<br>27 Haziran 2024| e7eea674ca718e3befd 90858e09f8372ad0ae2aa| [AIA](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2002%20-%20xsign.crt)<br>['SI](https://www.microsoft.com/pkiops/crl/Microsoft%20Azure%20TLS%20Issuing%20CA%2002.crl) |
| Microsoft Azure TLS veren CA 05| 0d7bede97d8209967a 52631b8bdd18bd| 29 Temmuz 2020<br>27 Haziran 2024| 6c3af02e7f269aa73a fd0eff2a88a4a1f04ed1e5| [AIA](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2005%20-%20xsign.crt)<br>['SI](https://www.microsoft.com/pkiops/crl/Microsoft%20Azure%20TLS%20Issuing%20CA%2005.crl) |
| Microsoft Azure TLS veren CA 06| 02e79171fb8021e93fe 2d983834c50c0| 29 Temmuz 2020<br>27 Haziran 2024| 30e01761ab97e59a06b 41ef20af6f2de7ef4f7b0| [AIA](https://www.microsoft.com/pkiops/certs/Microsoft%20Azure%20TLS%20Issuing%20CA%2006.cer)<br>['SI](https://www.microsoft.com/pkiops/crl/Microsoft%20Azure%20TLS%20Issuing%20CA%2006.crl) |


 ### <a name="baltimore-cybertrust-root"></a>Baltimore Sitrust kökü

| Kök CA| Seri Numarası| Sorun tarihi bitiş tarihi| SHA1 parmak Izi| URI'ler |
| - | - | - | - | - | 
| Baltimore Sitrust kökü| 020000b9| 12 Mayıs 2000<br>12 Mayıs 2025| d4de20d05e66fc53fe 1a50882c78db2852cae474|<br>['SI](http://crl3.digicert.com/Omniroot2025.crl)<br>[OCSP](http://ocsp.digicert.com/) |


#### <a name="associated-intermediate-cas"></a>İlişkili ara CA 'Lar

| Verme ve ara CA| Seri Numarası| Sorun tarihi bitiş tarihi| SHA1 parmak Izi| URI'ler |
| - | - | - | - | - | 
| Microsoft RSA TLS CA 01| 703d7a8f0ebf55aaa 59f98eaf4a206004eb2516a| 21 Temmuz 2020<br>8 Ekim 2024| 417e225037fbfaa4f9 5761d5ae729e1aea7e3a42| [AIA](https://www.microsoft.com/pki/mscorp/Microsoft%20RSA%20TLS%20CA%2001.crt)<br>['SI](https://mscrl.microsoft.com/pki/mscorp/crl/Microsoft%20RSA%20TLS%20CA%2001.crl)<br>[OCSP](http://ocsp.msocsp.com/) |
| Microsoft RSA TLS CA 02| b0c2d2d13cdd56cdaa 6ab6e2c04440be4a429c75| 21 Temmuz 2020<br>20 Mayıs 2024| 54d9d20239080c32316ed 9ff980a48988f4adf2d| [AIA](https://www.microsoft.com/pki/mscorp/Microsoft%20RSA%20TLS%20CA%2002.crt)<br>['SI](https://mscrl.microsoft.com/pki/mscorp/crl/Microsoft%20RSA%20TLS%20CA%2002.crl)<br>[OCSP](http://ocsp.msocsp.com/) |


 ### <a name="digicert-global-root-ca"></a>DigiCert genel kök CA 'sı

| Kök CA| Seri Numarası| Sorun tarihi bitiş tarihi| SHA1 parmak Izi| URI'ler |
| - | - | - | - | - | 
| DigiCert genel kök CA 'sı| 083be056904246 b1a1756ac95991c74a| 9 Kasım 2006<br>9 Kasım 2031| a8985d3a65e5e5c4b2d7 d66d40c6dd2fb19c5436| ['SI](http://crl3.digicert.com/DigiCertGlobalRootCA.crl)<br>[OCSP](http://ocsp.digicert.com/) |


#### <a name="associated-intermediate-cas"></a>İlişkili ara CA 'Lar

| Verme ve ara CA| Seri Numarası| Sorun tarihi bitiş tarihi| SHA1 parmak Izi| URI'ler |
| - | - | - | - | - |
| DigiCert SHA2 Secure Server CA| 01fda3eb6eca75c 888438b724bcfbc91| 8 Mart 2013, 2023 Mart| 1fb86b1168ec743154062 e8c9cc5b171a4b7ccb4| [AIA](http://cacerts.digicert.com/DigiCertSHA2SecureServerCA.crt)<br>['SI](http://crl3.digicert.com/ssca-sha2-g6.crl)<br>[OCSP](http://ocsp.digicert.com/) |
| DigiCert SHA2 Secure Server CA |02742eaa17ca8e21 c717bb1ffcfd0ca0 |22 Eylül 2020<br>22 Eylül 2030|626d44e704d1gizlenir be3bf 0d53397464ac8080142c|[AIA](http://cacerts.digicert.com/DigiCertSHA2SecureServerCA-2.crt)<br>['SI](http://crl3.digicert.com/DigiCertSHA2SecureServerCA.crl)<br>[OCSP](http://ocsp.digicert.com/)|


## <a name="cas-used-in-azure-china-21vianet-cloud"></a>Azure Çin 21Vianet bulutu 'nda kullanılan CA 'Lar

### <a name="digicert-global-root-ca"></a>DigiCert genel kök CA 'sı


| Kök CA| Seri Numarası| Sorun tarihi bitiş tarihi| SHA1 parmak Izi| URI'ler |
| - | - | - | - | - |
| DigiCert genel kök CA 'sı| 083be056904246b 1a1756ac95991c74a| Kas. 9, 2006<br>Kas. 9, 2031| a8985d3a65e5e5c4b2d7 d66d40c6dd2fb19c5436| ['SI](http://ocsp.digicert.com/)<br>[OCSP](http://crl3.digicert.com/DigiCertGlobalRootCA.crl) |


#### <a name="associated-intermediate-ca"></a>İlişkili ara CA

| Verme ve ara CA| Seri Numarası| Sorun tarihi bitiş tarihi| SHA1 parmak Izi| URI'ler |
| - | - | - | - | - | - |
| DigiCert temel RSA CN CA G2| 02f7e1f982bad 009aff47dc95741b2f6| 4 Mart 2020<br>4 Mart 2030| 4d1fa5d1fb1ac3917c08e 43f65015e6aea571179| [AIA](http://cacerts.digicert.cn/DigiCertBasicRSACNCAG2.crt)<br>['SI](http://crl.digicert.cn/DigiCertBasicRSACNCAG2.crl)<br>[OCSP](http://ocsp.digicert.cn/) |

## <a name="next-steps"></a>Sonraki Adımlar
[Microsoft 365 şifreleme zincirlerini öğrenin](/microsoft-365/compliance/encryption-office-365-certificate-chains)
