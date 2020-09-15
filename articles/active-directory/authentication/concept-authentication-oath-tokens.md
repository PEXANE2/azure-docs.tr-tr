---
title: OATH belirteçleri kimlik doğrulama yöntemi-Azure Active Directory
description: Oturum açma olaylarını geliştirmek ve güvenli hale getirmek için Azure Active Directory OATH belirteçlerini kullanma hakkında bilgi edinin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 09/02/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.collection: M365-identity-device-management
ms.openlocfilehash: c62b531b0d189ad698a930d65506fff0ebee3a69
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90532751"
---
# <a name="authentication-methods-in-azure-active-directory---oath-tokens"></a>Azure Active Directory-OATH belirteçlerindeki kimlik doğrulama yöntemleri

OATH TOTP (zaman tabanlı bir kerelik parola), bir kerelik parola (OTP) kodlarının nasıl oluşturulduğunu belirten bir açık standarttır. OATH TOTP, kodları oluşturmak için yazılım veya donanım kullanılarak uygulanabilir. Azure AD, farklı bir kod oluşturma standardı olan OATH HOTP 'yi desteklemez.

## <a name="oath-software-tokens"></a>OATH yazılım belirteçleri

Yazılım OATH belirteçleri genellikle Microsoft Authenticator uygulaması ve diğer kimlik doğrulayıcı uygulamaları gibi uygulamalardır. Azure AD, uygulamaya giriş yapan ve her OTP oluşturmak için kullanılan, gizli anahtar veya çekirdek oluşturur.

Kimlik doğrulayıcı uygulaması, bir kullanıcının cihazının bağlantısı olmasa bile bir yedeklemesi olması için, anında iletme bildirimleri için ayarlandığında, otomatik olarak kodlar üretir. Kod oluşturmak için OATH TOTP kullanan üçüncü taraf uygulamalar da kullanılabilir.

Bazı OATH TOTP Donanım belirteçleri programlanabilir, yani gizli anahtar veya çekirdek önceden programlanabilir olarak gelmeyecektir. Bu programlanabilir donanım belirteçleri, yazılım belirteci kurulum akışından alınan gizli anahtar veya çekirdek kullanılarak ayarlanabilir. Müşteriler bu belirteçleri kendi tercih ettiği satıcıdan satın alabilir ve kendi satıcısının kurulum sürecinde gizli anahtarı veya kaynağı kullanabilir.

## <a name="oath-hardware-tokens-preview"></a>OATH Donanım belirteçleri (Önizleme)

Azure AD, her 30 veya 60 saniyede bir kodu yenileyen OATH-TOTP SHA-1 belirteçleri kullanımını destekler. Müşteriler bu belirteçleri kendi tercih ettiği satıcıdan satın alabilir.

OATH TOTP Donanım belirteçleri genellikle, belirteçte önceden programlanabilir bir gizli anahtar veya çekirdek ile gelir. Bu anahtarların aşağıdaki adımlarda açıklandığı gibi Azure AD 'ye giriş olması gerekir. Gizli anahtarlar 128 karakterle sınırlıdır ve bu, tüm belirteçlerle uyumlu olmayabilir. Gizli anahtar yalnızca *a-z* veya *a-z* karakterleri ve *1-7*rakamları içerebilir ve *Base32*içinde kodlanmalıdır.

Yeniden kullanılabilen programlanabilir OATH TOTP Donanım belirteçleri, yazılım belirteci kurulum akışında Azure AD ile de ayarlanabilir.

OATH Donanım belirteçleri, genel önizlemenin bir parçası olarak desteklenir. Önizlemeler hakkında daha fazla bilgi için bkz.  [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

![OATH belirteçlerini MFA OATH belirteçleri dikey penceresine yükleme](media/concept-authentication-methods/mfa-server-oath-tokens-azure-ad.png)

Belirteçler alındıktan sonra, aşağıdaki örnekte gösterildiği gibi, UPN, seri numarası, gizli anahtar, zaman aralığı, üretici ve model dahil olmak üzere bir virgülle ayrılmış değerler (CSV) dosya biçiminde karşıya yüklenmelidir:

```csv
upn,serial number,secret key,time interval,manufacturer,model
Helga@contoso.com,1234567,1234567abcdef1234567abcdef,60,Contoso,HardwareKey
```

> [!NOTE]
> Üst bilgi satırını CSV dosyanıza eklediğinizden emin olun.

Bir CSV dosyası olarak düzgün biçimlendirildikten sonra, bir yönetici Azure portal oturum açabilir, **Azure Active Directory > güvenlik > MFA > Oath belirteçleri**' ne gidebilir ve elde edilen CSV dosyasını karşıya yükleyebilir.

CSV dosyasının boyutuna bağlı olarak, işlem birkaç dakika sürebilir. Geçerli durumu almak için **Yenile** düğmesini seçin. Dosyada herhangi bir hata varsa, çözmeniz için herhangi bir hatayı listeleyen bir CSV dosyası indirebilirsiniz. İndirilen CSV dosyasındaki alan adları karşıya yüklenen sürümden farklı.

Herhangi bir hata çözüldükten sonra, yönetici belirteç için **Etkinleştir** ' i seçip BELIRTEÇTE görünen OTP 'yi girerek her anahtarı etkinleştirebilir.

Kullanıcılar, her zaman kullanılmak üzere yapılandırılmış Microsoft Authenticator uygulaması gibi beş OATH donanım belirtecinin veya Authenticator uygulamasının bir birleşimine sahip olabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft Graph REST API Beta](/graph/api/resources/authenticationmethods-overview?view=graph-rest-beta)kullanarak kimlik doğrulama yöntemlerini yapılandırma hakkında daha fazla bilgi edinin.
