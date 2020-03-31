---
title: Azure Etkin Dizin Koşullu Erişim SSS | Microsoft Dokümanlar
description: Azure Active Directory'de Koşullu Erişim hakkında sık sorulan soruların yanıtlarını alın.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6842338bd27e4bea3436f0b249380ab773d60de6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77368078"
---
# <a name="azure-active-directory-conditional-access-faqs"></a>Azure Etkin Dizin Koşullu Erişim SSS'leri

## <a name="which-applications-work-with-conditional-access-policies"></a>Koşullu Erişim ilkeleriyle hangi uygulamalar çalışır?

Koşullu Erişim ilkeleriyle çalışan uygulamalar hakkında bilgi için [Azure Etkin Dizini'nde Koşullu Erişim kurallarını kullanan Uygulamalar ve tarayıcılara](concept-conditional-access-cloud-apps.md)bakın.

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>B2B işbirliği ve konuk kullanıcılar için Koşullu Erişim ilkeleri zorlanıyor mu?

İlkeler, işletmeler arası (B2B) işbirliği kullanıcıları için uygulanır. Ancak, bazı durumlarda, bir kullanıcı ilke gereksinimlerini karşılayamayabilir. Örneğin, konuk kullanıcının kuruluşu çok faktörlü kimlik doğrulamasını desteklemeyebilir. 

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>SharePoint Online politikası İş için OneDrive için de geçerli midir?

Evet. SharePoint Online politikası, OneDrive for Business için de geçerlidir.

## <a name="why-cant-i-set-a-policy-directly-on-client-apps-like-word-or-outlook"></a>Neden doğrudan Word veya Outlook gibi istemci uygulamalarına bir ilke ayarlamıyorum?

Koşullu Erişim ilkesi, bir hizmete erişmek için gereksinimleri belirler. Bu hizmetin kimlik doğrulaması gerçekleştiğinde uygulanır. İlke doğrudan bir istemci uygulaması üzerinde ayarlanmaz. Bunun yerine, bir istemci bir hizmeti aradığında uygulanır. Örneğin, SharePoint'te ayarlanmış bir ilke, SharePoint'i arayan istemciler için geçerlidir. Exchange'de ayarlanan bir ilke Outlook için geçerlidir.

## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Şartlı Erişim ilkesi hizmet hesapları için geçerli midir?

Koşullu Erişim ilkeleri tüm kullanıcı hesapları için geçerlidir. Bu, hizmet hesabı olarak kullanılan kullanıcı hesaplarını içerir. Genellikle, gözetimsiz çalışan bir hizmet hesabı Koşullu Erişim ilkesinin gereksinimlerini karşılayamıyor. Örneğin, çok faktörlü kimlik doğrulama gerekebilir. Hizmet hesapları, Koşullu Erişim ilkesi yönetim ayarlarını kullanarak bir ilkenin dışında tutulabilir. 

## <a name="are-microsoft-graph-apis-available-for-configuring-conditional-access-policies"></a>Koşullu Erişim ilkelerini yapılandırmak için Microsoft Grafik API'leri kullanılabilir mi?

Şu anda, hayır. 

## <a name="what-is-the-default-exclusion-policy-for-unsupported-device-platforms"></a>Desteklenmeyen aygıt platformları için varsayılan dışlama ilkesi nedir?

Şu anda, Koşullu Erişim ilkeleri iOS ve Android aygıtkullanıcıları üzerinde seçici olarak uygulanır. Diğer aygıt platformlarında uygulamalar, varsayılan olarak, iOS ve Android cihazlar için Koşullu Erişim ilkesinden etkilenmez. Kiracı yönetici, desteklenmeyen platformlardaki kullanıcılara erişime izin vermemek için genel ilkeyi geçersiz kılmayı seçebilir.

## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a>Koşullu Erişim ilkeleri Microsoft Ekipleri için nasıl çalışır?

Microsoft Teams, toplantılar, takvimler ve dosya paylaşımı gibi temel üretkenlik senaryoları için büyük ölçüde Exchange Online ve SharePoint Online'a güvenir. Bu bulut uygulamaları için ayarlanan Koşullu Erişim ilkeleri, bir kullanıcı doğrudan Microsoft Teams'e girdiğinde Microsoft Ekipleri için geçerlidir.

Microsoft Teams, Azure Etkin Dizin Koşullu Erişim ilkelerinde bulut uygulaması olarak ayrı ayrı desteklenir. Bir bulut uygulaması için ayarlanan Koşullu Erişim ilkeleri, kullanıcı giriş yaptığında Microsoft Ekipleri için geçerlidir. Ancak, Exchange Online ve SharePoint Online gibi diğer uygulamalarda doğru ilkeler olmadan kullanıcılar bu kaynaklara doğrudan erişebilir.

Windows ve Mac için Microsoft Teams masaüstü istemcileri modern kimlik doğrulamasını destekler. Modern kimlik doğrulaması, platformlar daki Microsoft Office istemci uygulamalarına Azure Active Directory Authentication Library 'yi (ADAL) temel alan oturum açma sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Ortamınız için Koşullu Erişim ilkelerini yapılandırmak [için Azure Etkin Dizini'nde Koşullu Erişim için En İyi uygulamalara](best-practices.md)bakın. 
