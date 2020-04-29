---
title: Kimlik Doğrulaması
description: Kimlik doğrulamanın nasıl çalıştığını açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/12/2019
ms.topic: how-to
ms.openlocfilehash: fbc27f2fa3b7517151c4bcdbec5b146e83dd868e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80681642"
---
# <a name="configure-authentication"></a>Kimlik doğrulamasını yapılandırma

Azure uzaktan Işleme, [Azure uzamsal bağlayıcı (ASA)](https://docs.microsoft.com/azure/spatial-anchors/concepts/authentication?tabs=csharp)ile aynı kimlik doğrulama mekanizmasını kullanır. İstemcilerin REST API 'Leri başarıyla çağırmak için *Accountkey*, *Authenticationtoken*veya *accesstoken* ayarlaması gerekir. *Accountkey* , Azure Portal uzaktan işleme hesabının "anahtarlar" sekmesinde elde edilebilir. *Authenticationtoken* , [adal kitaplığı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries)kullanılarak elde edilebilir bir Azure AD belirtecidir. *Accesstoken* , Azure Karma Gerçeklik güvenlik belirteci hizmeti 'NDEN (STS) elde EDILEBILIR bir Mr belirtecidir.

## <a name="authentication-for-deployed-applications"></a>Dağıtılan uygulamalar için kimlik doğrulaması

 Hesap anahtarlarının kullanımı hızlı bir şekilde taslak için önerilir, ancak yalnızca geliştirme/prototip oluşturma sırasında önerilir. Uygulamanızı bir ekli hesap anahtarı kullanarak üretime göndermemelidir ve bunun yerine Kullanıcı tabanlı veya hizmet tabanlı Azure AD kimlik doğrulama yaklaşımlarını kullanın.

 Azure AD kimlik doğrulaması, Azure [uzamsal bağlayıcı (ASA)](https://docs.microsoft.com/azure/spatial-anchors/) hizmeti 'nın [Azure AD Kullanıcı kimlik doğrulaması](https://docs.microsoft.com/azure/spatial-anchors/concepts/authentication?tabs=csharp#azure-ad-user-authentication) bölümünde açıklanmaktadır.

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Hizmetinizin uygulamalara, hizmetlerine veya Azure AD kullanıcılarına verilen erişim düzeyini denetlemeye yardımcı olmak için, Azure uzaktan Işleme hesaplarınıza göre gereken şekilde atamanız için aşağıdaki roller oluşturulmuştur:

* **Uzaktan Işleme Yöneticisi**: Azure uzaktan işleme için Kullanıcı dönüştürme, oturum, işleme ve Tanılama özelliklerini yönetme olanağı sunar.
* **Uzaktan Işleme istemcisi**: kullanıcıya Azure uzaktan işleme yönelik oturum, işleme ve Tanılama özelliklerini yönetme olanağı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Hesap oluşturma](create-an-account.md)
* [Kimlik doğrulaması için Azure ön uç API 'Lerini kullanma](frontend-apis.md)
* [Örnek PowerShell betikleri](../samples/powershell-example-scripts.md)
