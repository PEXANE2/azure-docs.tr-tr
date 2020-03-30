---
title: Koşullu Erişim hizmeti bağımlılıkları - Azure Etkin Dizini
description: Bir ilkeyi tetiklemek için Azure Active Directory Koşullu Erişim'de koşulların nasıl kullanıldığını öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: b39238575c05d35a2d87999e08c49c0c77e99bfb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74380021"
---
# <a name="what-are-service-dependencies-in-azure-active-directory-conditional-access"></a>Azure Etkin Dizin Koşullu Erişimi'nde hizmet bağımlılıkları nelerdir? 

Koşullu Erişim ilkeleriyle, web sitelerine ve hizmetlere erişim gereksinimlerini belirtebilirsiniz. Örneğin, erişim gereksinimleriniz çok faktörlü kimlik doğrulama (MFA) veya [yönetilen aygıtlar](require-managed-devices.md)gerektirmeyi içerebilir. 

Bir siteye veya hizmete doğrudan erişdiğinizde, ilgili bir ilkenin etkisini değerlendirmek genellikle kolaydır. Örneğin, SharePoint Online yapılandırılmış için MFA gerektiren bir ilkeniz varsa, SharePoint web portalında oturum açan her oturum açma için MFA uygulanır. Ancak, diğer bulut uygulamalarına bağımlı bulut uygulamaları olduğundan, bir politikanın etkisini değerlendirmek her zaman açık değildir. Örneğin, Microsoft Teams SharePoint Online'daki kaynaklara erişim sağlayabilir. Bu nedenle, geçerli senaryomuzda Microsoft Teams'e erişdiğinizde, SharePoint MFA ilkesine de tabi olursunuz.   

## <a name="policy-enforcement"></a>İlke zorlama 

Bir hizmet bağımlılığı yapılandırılan varsa, ilke erken bağlı veya geç bağlı zorlama kullanılarak uygulanabilir. 

- **Erken zorunlu ilke zorlama,** bir kullanıcının arama uygulamasına erişmeden önce bağımlı hizmet ilkesini karşılaması gerektiği anlamına gelir. Örneğin, bir kullanıcının MS Teams'de oturum açmadan önce SharePoint ilkesini karşılaması gerekir. 
- Kullanıcı arama uygulamasına girdikten sonra **geç bağlı ilke uygulaması** oluşur. Uygulama, uygulama isteklerini ararken, akış aşağı hizmeti için bir belirteç olarak ertelenir. Bunlara örnek olarak PLANlayıcıya erişen MS Teams ve SharePoint'e Office.com verilebilir. 

Aşağıdaki diyagram, MS Teams hizmet bağımlılıklarını göstermektedir. Katı oklar, Planlayıcı için kesikli ok, geç bağlı zorlamayı gösterir. 

![MS Teams hizmet bağımlılıkları](./media/service-dependencies/01.png)

En iyi uygulama olarak, mümkün olduğunda ilgili uygulamalar ve hizmetler arasında ortak ilkeler belirlemeniz gerekir. Tutarlı bir güvenlik duruşuna sahip olmak size en iyi kullanıcı deneyimini sunar. Örneğin, Exchange Online, SharePoint Online, Microsoft Teams ve Skype for business genelinde ortak bir ilke ayarlamak, akış aşağı hizmetlerine uygulanan farklı ilkelerden kaynaklanabilecek beklenmeyen istemleri önemli ölçüde azaltır. 

Aşağıdaki tabloda, istemci uygulamaların tatmin etmesi gereken ek hizmet bağımlılıkları listelanmaktadır  

| İstemci uygulamaları         | Downstream hizmeti                          | Zorlama |
| :--                 | :--                                         | ---         | 
| Azure Data Lake     | Microsoft Azure Yönetimi (portal ve API) | Erken bağlı |
| Microsoft Sınıfı | Exchange                                    | Erken bağlı |
|                     | SharePoint                                  | Erken bağlı |
| Microsoft Teams     | Exchange                                    | Erken bağlı |
|                     | MS Planlayıcısı                                  | Geç bağlı  |
|                     | SharePoint                                  | Erken bağlı |
|                     | Skype Kurumsal Çevrimiçi Sürüm                   | Erken bağlı |
| Ofis Portalı       | Exchange                                    | Geç bağlı  |
|                     | SharePoint                                  | Geç bağlı  |
| Outlook grupları      | Exchange                                    | Erken bağlı |
|                     | SharePoint                                  | Erken bağlı |
| PowerApps           | Microsoft Azure Yönetimi (portal ve API) | Erken bağlı |
|                     | Windows Azure Active Directory              | Erken bağlı |
| Project             | Dynamics CRM                                | Erken bağlı |
| Skype Kurumsal  | Exchange                                    | Erken bağlı |
| Visual Studio       | Microsoft Azure Yönetimi (portal ve API) | Erken bağlı |
| Microsoft Forms     | Exchange                                    | Erken bağlı |
|                     | SharePoint                                  | Erken bağlı |
| Microsoft To-Do     | Exchange                                    | Erken bağlı |

## <a name="next-steps"></a>Sonraki adımlar

Ortamınızda Koşullu Erişim'i nasıl uygulayacağınızı öğrenmek için Azure [Etkin Dizini'nde Koşullu Erişim dağıtımınızı](plan-conditional-access.md)planlayın'a bakın.
