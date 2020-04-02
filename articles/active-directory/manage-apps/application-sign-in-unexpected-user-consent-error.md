---
title: Bir uygulamaya onay verirken beklenmeyen hata | Microsoft Dokümanlar
description: Bir uygulamaya onay verme işlemi sırasında oluşabilecek hataları ve bunlar hakkında yapabileceklerinizi tartışır
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea14e02920cf7ba6c5e0a7b415cb92137c915576
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80519700"
---
# <a name="unexpected-error-when-performing-consent-to-an-application"></a>Bir uygulamaya onay verirken beklenmeyen hata

Bu makalede, bir uygulamaya onay verme işlemi sırasında oluşabilecek hatalar açıklanabilir. Herhangi bir hata iletisi içermeyen beklenmeyen onay istemlerini gidermeye zahmet ediyorsanız, [Azure AD için Kimlik Doğrulama Senaryoları'na](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios)bakın.

Azure Etkin Dizinile tümlebilen birçok uygulama, çalışması için diğer kaynaklara erişmek için izingerektirir. Bu kaynaklar Azure Etkin Dizini ile de entegre edildiğinde, bunlara erişmek için izinler genellikle ortak onay çerçevesi kullanılarak istenir. Genellikle bir uygulama ilk kez kullanıldığında oluşan ancak uygulamanın sonraki kullanımında da oluşabilecek bir onay istemi görüntülenir.

Bir kullanıcının bir uygulamanın gerektirdiği izinleri kabul edemesi için belirli koşullar geçerli olmalıdır. Bu koşullar karşılanmazsa, aşağıdaki hatalar oluşabilir.

## <a name="requesting-not-authorized-permissions-error"></a>İzin sizizin hatası isteme
* **AADSTS90093:** &lt;clientAppDisplayName,&gt; vermeye yetkili olmadığınız bir veya daha fazla izin talep ediyor. Bu başvuruyu sizin adınıza onaylayan bir yöneticiye başvurun.
* **AADSTS90094:** &lt;clientAppDisplayName'nin&gt; kuruluşunuzdaki kaynaklara erişmek için yalnızca bir yöneticinin verebileceği kaynaklara erişmesi için izin alması gerekir. Kullanabilmek için önce lütfen yöneticiden bu uygulamaya izin vermesini isteyin.

Bu hata, şirket yöneticisi olmayan bir kullanıcı yalnızca yöneticinin verebileceği izinleri isteyen bir uygulamayı kullanmaya çalıştığında oluşur. Bu hata, kendi kuruluşu adına uygulamaya erişim veren bir yönetici tarafından çözülebilir.

Bu hata, Microsoft'un izin isteğinin riskli olduğunu algılaması nedeniyle bir kullanıcının bir uygulamayı onaylayarak kabul etmesi engellendiğinde de oluşabilir. Bu durumda, bir denetim olayı da "Uygulama Yönetimi", "Başvuru ya da onay" faaliyet türü ve "Riskli uygulama tespit" Durum Nedeni kategorisi ile günlüğe kaydedilir.

## <a name="policy-prevents-granting-permissions-error"></a>İlke, izin verme hatası nın engellenmesini
* **AADSTS90093:** TenantDisplayName'nin &lt;&gt; bir yöneticisi, uygulamanın &lt;&gt; adını istediği izinleri vermenizi engelleyen bir ilke belirlemiştir. Sizin adınıza &lt;bu&gt;uygulamaya izin verebilecek kiracıDisplayName'nin bir yöneticisine başvurun.

Bu hata, bir şirket yöneticisi kullanıcıların uygulamaları onaylayabilme yeteneğini kapattığında, yönetici olmayan bir kullanıcı onay gerektiren bir uygulamayı kullanmaya çalıştığında oluşur. Bu hata, kendi kuruluşu adına uygulamaya erişim veren bir yönetici tarafından çözülebilir.

## <a name="intermittent-problem-error"></a>Aralıklı sorun hatası
* **AADSTS90090:** Bu oturum açma &lt;işlemi, clientAppDisplayName'e&gt;vermeye çalışmadığınız izinleri kaydederek aralıklı bir sorunla karşılaşmış gibi görünüyor. daha sonra tekrar deneyin.

Bu hata, aralıklı bir hizmet tarafı sorunu oluştuğunu gösterir. Uygulamayı yeniden onaylamaya çalışarak çözülebilir.

## <a name="resource-not-available-error"></a>Kaynak yok hatası
* **AADSTS65005:** Uygulama &lt;istemcisiAppDisplayName,&gt; mevcut olmayan &lt;bir kaynak&gt; kaynağına erişmek için izin istediAppDisplayName. 

Uygulama geliştiricisine başvurun.

##  <a name="resource-not-available-in-tenant-error"></a>Kaynak kiracı hatasında kullanılamıyor
* **AADSTS65005:** &lt;&gt; clientAppDisplayName, kuruluşunuztenantDisplayName'de &lt;&gt; &lt;&gt;bulunmayan bir kaynak kaynağına erişim talep ediyor. 

Bu kaynağın kullanılabilir olduğundan emin &lt;olun veya&gt;tenantDisplayName'nin bir yöneticisine başvurun.

## <a name="permissions-mismatch-error"></a>İzinler uyuşmazlığı hatası
* **AADSTS65005:** Uygulama kaynak kaynakAppDisplayName &lt;&gt;erişmek için izin istedi. Bu istek, uygulamanın uygulama kaydı sırasında önceden yapılandırılma şekliyle eşleşmediği için başarısız oldu. Uygulama satıcısına başvurun.**

Bu hataların tümü, kullanıcının onay vermeye çalıştığı uygulama, kuruluşun dizininde (kiracı) bulunamayan bir kaynak uygulamasına erişmek için izin istediğinde oluşur. Bu durum çeşitli nedenlerle oluşabilir:

-   İstemci uygulama geliştiricisi uygulamalarını yanlış yapılandırarak geçersiz bir kaynağa erişim isteğinde bulundu. Bu durumda, uygulama geliştiricisi bu sorunu gidermek için istemci uygulamasıyapılandırmasını güncelleştirmelidir.

-   Hedef kaynak uygulamasını temsil eden bir Hizmet Sorumlusu kuruluşta bulunmaz veya geçmişte var olmuş, ancak kaldırılmıştır. Bu sorunu gidermek için, istemci uygulamasının bu konuda izin isteyemesi için kaynak uygulaması için bir Hizmet Sorumlusunun kuruluşta sağlanması gerekir. Hizmet Sorumlusu, uygulama türüne bağlı olarak çeşitli şekillerde sağlanabilir:

    -   Kaynak uygulaması için abonelik edinme (Microsoft yayımlanmış uygulamalar)

    -   Kaynak başvurusuna onay verme

    -   Azure portalı üzerinden uygulama izinlerinin verilmesi

    -   Azure AD Uygulama Galerisi'nden uygulama ekleme

## <a name="next-steps"></a>Sonraki adımlar 

[Azure Etkin Dizin 'de (v1 bitiş noktası) uygulamalar, izinler ve onay](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)<br>

[Azure Etkin Dizini'nde kapsamlar, izinler ve onay (v2.0 bitiş noktası)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


