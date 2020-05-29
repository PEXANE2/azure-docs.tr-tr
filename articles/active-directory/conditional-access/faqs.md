---
title: Koşullu erişim SSS Azure Active Directory | Microsoft Docs
description: Azure Active Directory ' de koşullu erişim hakkında sık sorulan soruların yanıtlarını alın.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 05/28/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.custom: has-adal-ref
ms.openlocfilehash: 1704fb50586168be6f960e62b918019cb67ce4c8
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84167817"
---
# <a name="azure-active-directory-conditional-access-faqs"></a>Azure Active Directory Koşullu erişim SSS

## <a name="which-applications-work-with-conditional-access-policies"></a>Hangi uygulamalar koşullu erişim ilkeleriyle çalışır?

Koşullu erişim ilkeleriyle çalışan uygulamalar hakkında daha fazla bilgi için, bkz. [Azure Active Directory Koşullu erişim kuralları kullanan uygulamalar ve tarayıcılar](concept-conditional-access-cloud-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>B2B işbirliği ve Konuk kullanıcılar için koşullu erişim ilkeleri uygulandı mi?

İlkeler, işletmeler arası (B2B) işbirliği kullanıcıları için zorlanır. Ancak, bazı durumlarda, bir kullanıcı ilke gereksinimlerini karşılayamayabilir. Örneğin, bir Konuk kullanıcının organizasyonu çok faktörlü kimlik doğrulamasını desteklemeyebilir. 

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>OneDrive Iş için bir SharePoint Online ilkesi de geçerlidir mı?

Evet. Bir SharePoint Online ilkesi, OneDrive Iş için de geçerlidir. Daha fazla bilgi için, [koşullu erişim hizmeti bağımlılıkları](service-dependencies.md) makalesine bakın ve bunun yerine ilkeleri [Office 365 uygulamasına](concept-conditional-access-cloud-apps.md#office-365-preview) hedeflemeyi düşünün.

## <a name="why-cant-i-set-a-policy-directly-on-client-apps-like-word-or-outlook"></a>Neden bir ilkeyi Word veya Outlook gibi istemci uygulamalarında doğrudan ayarlayamıyorum?

Koşullu erişim ilkesi, bir hizmete erişmek için gereksinimleri ayarlar. Bu hizmet için kimlik doğrulaması gerçekleştiğinde zorlanır. İlke doğrudan bir istemci uygulamasında ayarlanmadı. Bunun yerine, istemci bir hizmet çağırdığında uygulanır. Örneğin, SharePoint 'te ayarlanan bir ilke, SharePoint 'i çağıran istemciler için geçerlidir. Exchange üzerinde ayarlanan bir ilke Outlook için geçerlidir. Daha fazla bilgi için, [koşullu erişim hizmeti bağımlılıkları](service-dependencies.md) makalesine bakın ve bunun yerine ilkeleri [Office 365 uygulamasına](concept-conditional-access-cloud-apps.md#office-365-preview) hedeflemeyi düşünün.

## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Koşullu erişim ilkesi hizmet hesaplarına uygulanabilir mi?

Koşullu erişim ilkeleri tüm Kullanıcı hesapları için geçerlidir. Bu, hizmet hesapları olarak kullanılan Kullanıcı hesaplarını içerir. Genellikle, katılımsız çalıştıran bir hizmet hesabı, koşullu erişim ilkesinin gereksinimlerini karşılayamaz. Örneğin, çok faktörlü kimlik doğrulaması gerekebilir. Hizmet hesapları, bir [Kullanıcı veya grup dışlaması](concept-conditional-access-users-groups.md#exclude-users)kullanılarak bir ilkeden dışlanamaz. 

## <a name="what-is-the-default-exclusion-policy-for-unsupported-device-platforms"></a>Desteklenmeyen cihaz platformları için varsayılan dışlama ilkesi nedir?

Şu anda, koşullu erişim ilkeleri iOS ve Android cihazlarının kullanıcıları için seçmeli olarak zorlanır. Diğer cihaz platformlarındaki uygulamalar, varsayılan olarak iOS ve Android cihazlar için koşullu erişim ilkesinden etkilenmez. Bir kiracı yöneticisi, desteklenmeyen platformlardaki kullanıcılara erişim izni vermemek için genel ilkeyi geçersiz kılmayı tercih edebilir.

## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a>Koşullu erişim ilkeleri Microsoft ekipleri için nasıl çalışır?

Microsoft ekipleri, toplantılar, takvimler ve dosya paylaşımı gibi temel üretkenlik senaryoları için Exchange Online ve SharePoint Online 'ı yoğun bir şekilde kullanır. Bu bulut uygulamaları için ayarlanan koşullu erişim ilkeleri, bir kullanıcı doğrudan Microsoft ekiplerine kaydolduğunda Microsoft ekipleri için geçerlidir.

Microsoft ekipleri Ayrıca koşullu erişim ilkelerinde bir bulut uygulaması olarak da desteklenir. Bir bulut uygulaması için ayarlanan koşullu erişim ilkeleri, bir Kullanıcı oturum açtığında Microsoft ekipleri için geçerlidir. Ancak, Exchange Online ve SharePoint Online kullanıcıları gibi diğer uygulamalarda doğru ilkeler olmadan bu kaynaklara doğrudan erişebiliyor olabilir.

Windows ve Mac için Microsoft ekipleri masaüstü istemcileri modern kimlik doğrulamayı destekler. Modern kimlik doğrulaması, platformlar arası istemci uygulamalarını Microsoft Office için Azure Active Directory kimlik doğrulama kitaplığı 'nı (ADAL) temel alan oturum açma olanağı sunar.

Daha fazla bilgi için, [koşullu erişim hizmeti bağımlılıkları](service-dependencies.md) makalesine bakın ve bunun yerine ilkeleri [Office 365 uygulamasına](concept-conditional-access-cloud-apps.md#office-365-preview) hedeflemeyi düşünün.

## <a name="next-steps"></a>Sonraki adımlar

- Ortamınız için koşullu erişim ilkelerini yapılandırmak üzere [Azure Active Directory ' de koşullu erişim Için en iyi yöntemlere](best-practices.md)bakın. 
