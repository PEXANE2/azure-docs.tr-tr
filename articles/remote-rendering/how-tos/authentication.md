---
title: Kimlik Doğrulaması
description: Kimlik doğrulamanın nasıl çalıştığını açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/12/2019
ms.topic: how-to
ms.openlocfilehash: fbc27f2fa3b7517151c4bcdbec5b146e83dd868e
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681642"
---
# <a name="configure-authentication"></a>Kimlik doğrulamayı yapılandırma

Azure Uzaktan İşleme, [Azure Uzamsal Bağlantı Çapaları (ASA)](https://docs.microsoft.com/azure/spatial-anchors/concepts/authentication?tabs=csharp)ile aynı kimlik doğrulama mekanizmasını kullanır. İstemcilerin, REST API'lerini başarılı bir şekilde aramak için *AccountKey,* *AuthenticationToken*veya *AccessToken'ı* ayarlaması gerekir. *AccountKey,* Azure portalındaki Uzaktan İşlem hesabıiçin "Anahtarlar" sekmesinden edinilebilir. *Kimlik Doğrulama Token,* [ADAL kitaplığı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries)kullanılarak elde edilebilen bir Azure AD belirtecidir. *AccessToken,* Azure Karma Gerçeklik Güvenliği Belirteci Hizmeti'nden (STS) elde edilebilen bir MR belirtecidir.

## <a name="authentication-for-deployed-applications"></a>Dağıtılan uygulamalar için kimlik doğrulaması

 Hızlı biniş için hesap anahtarlarının kullanılması önerilir, ancak geliştirme/prototipleme sırasında yalnızca. Uygulamanızı gömülü bir hesap anahtarı kullanarak üretime göndermemeniz ve bunun yerine kullanıcı tabanlı veya hizmet tabanlı Azure AD kimlik doğrulama yaklaşımlarını kullanmanız önerilir.

 Azure AD kimlik doğrulaması, [Azure Uzamsal Çapalar (ASA)](https://docs.microsoft.com/azure/spatial-anchors/) hizmetinin [Azure AD kullanıcı kimlik doğrulaması](https://docs.microsoft.com/azure/spatial-anchors/concepts/authentication?tabs=csharp#azure-ad-user-authentication) bölümünde açıklanmıştır.

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Hizmetinizin uygulamalarına, hizmetlerine veya Azure REKLAM kullanıcılarına verilen erişim düzeyini denetlemeye yardımcı olmak için, Azure Uzaktan İşleme hesaplarınıza gerektiği şekilde atamanız için aşağıdaki roller oluşturulmuştur:

* **Uzaktan İşleme Yöneticisi**: Azure Uzaktan İşleme için kullanıcıya dönüştürme, oturumu yönetme, oluşturma ve tanılama özellikleri sağlar.
* **Uzaktan İşleme İstemci**: Azure Uzaktan İşleme için kullanıcıya oturum, oluşturma ve tanılama özellikleri sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Hesap oluşturma](create-an-account.md)
* [Kimlik doğrulama için Azure Frontend API'lerini kullanma](frontend-apis.md)
* [Örnek PowerShell komut dosyaları](../samples/powershell-example-scripts.md)
