---
title: Azure ön kapısı Standart/Premium için izin verilen sertifika yetkilileri (Önizleme)
description: Bu makalede, kendi sertifikanızı oluştururken izin verilen tüm sertifika yetkilileri listelenir.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: troubleshooting
ms.date: 02/18/2021
ms.author: qixwang
ms.openlocfilehash: 35c1e4f6c700333c72b97289cda1772335037211
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "101100583"
---
# <a name="allowed-certificate-authorities-for-azure-front-door-standardpremium-preview"></a>Azure ön kapısı Standart/Premium için izin verilen sertifika yetkilileri (Önizleme)

Azure ön kapısının Standart/Premium özel etki alanı için kendi sertifikanızı kullanarak HTTPS özelliğini etkinleştirdiğinizde. TLS/SSL sertifikanızı oluşturmak için izin verilen bir sertifika yetkilisine (CA) ihtiyacınız vardır. Aksi takdirde, izin verilmeyen bir CA veya otomatik olarak imzalanan bir sertifika kullanıyorsanız isteğiniz reddedilir.

Kendi sertifikanızı oluştururken aşağıdaki CA 'Lara izin verilir:

- AddTrust dış CA kökü
- Harflerden SSL kök CA 'sı
- AME Infra CA 01
- AME Infra CA 02
- Ameroot
- APCA-DM3P
- Atos TrustedRoot 2011
- Autopilot kök CA
- Baltimore Sitrust kökü
- Sınıf 3 genel birincil sertifika yetkilisi
- COMODO RSA sertifika yetkilisi
- COMODO RSA etki alanı doğrulaması güvenli sunucu CA
- D-TRUST kök sınıfı 3 CA 2 2009
- DigiCert Cloud Services CA-1
- DigiCert genel kök CA 'sı
- DigiCert genel kök G2
- DigiCert yüksek güvence CA-3
- DigiCert yüksek güvence EV kök CA
- DigiCert SHA2 genişletilmiş doğrulama sunucusu CA 'sı
- DigiCert SHA2 yüksek güvence sunucu CA 'sı
- DigiCert SHA2 Secure Server CA
- DST kök CA x3
- D-Trust kök sınıfı 3 CA 2 2009
- Her yerde şifreleme DV TLS CA
- Kök sertifika yetkilisine güvenin
- Entrust kök sertifika yetkilisi-G2
- Entrust.net sertifika yetkilisi (2048)
- GeoTrust küresel CA
- Coğrafi güven birincil sertifika yetkilisi
- GeoTrust birincil sertifika yetkilisi-G2
- GeoTrust RSA CA 2018
- GlobalSign
- GlobalSign Genişletilmiş Doğrulama CA-SHA256-G2
- GlobalSign kuruluş doğrulama CA-G2
- GlobalSign kök CA
- Go Daddy kök sertifika yetkilisi-G2
- Go Daddy güvenli sertifika yetkilisi-G2
- Yetkili x3 'yi şifreleyelim
- Mikro sn e-Szigno kök CA 2009
- Quovadsıs root CA2 G3
- RapidSSL RSA CA 2018
- Güvenlik Iletişimi RootCA1
- Güvenlik Iletişimi RootCA2
- Güvenlik Iletişimi RootCA3
- Symantec Class 3 EV SSL CA-G3
- Symantec Class 3 güvenli sunucu CA-G4
- Microsoft için Symantec Enterprise Mobile Root
- Thawte birincil kök CA
- Thawte birincil kök CA-G2
- Thawte birincil kök CA-G3
- Thawte RSA CA 2018
- Thawte damgalama CA 'sı
- Trustasıtls RSA CA
- VeriSign Class 3 Genişletilmiş Doğrulama SSL CA 'sı
- VeriSign Class 3 Genişletilmiş Doğrulama SSL SGC CA
- VeriSign sınıfı 3 genel birincil sertifika yetkilisi-G5
- VeriSign Uluslararası sunucu CA 'sı-sınıf 3
- VeriSign zaman damgalama hizmeti kökü
- VeriSign Universal kök sertifika yetkilisi
