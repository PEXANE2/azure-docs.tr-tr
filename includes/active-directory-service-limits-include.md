---
title: include dosyası
description: include dosyası
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 05/22/2019
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: e6e1014ac20ef70f21344ec6ece5627eccb7ba66
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "67148472"
---
Azure Active Directory (Azure AD) hizmetine yönelik kullanım kısıtlamalarını ve diğer hizmet sınırlarını burada bulabilirsiniz.

| Category | Sınırlar |
| --- | --- |
| Dizinler | Tek bir kullanıcı, üye veya konuk olarak en fazla 500 Azure AD dizinine ait olabilir.<br/>Tek bir kullanıcı, en fazla 20 dizin oluşturabilir. |
| Etki Alanları | En fazla 900 yönetilen etki alanı adı ekleyebilirsiniz. Şirket içi Active Directory ile tüm etki alanlarınızı federasyon için ayarlarsanız her dizinde en fazla 450 etki alanı adı ekleyebilirsiniz. |
| Nesneler |<ul><li>Azure Active Directory'nin Ücretsiz sürümünün kullanıcıları, tek bir dizinde varsayılan olarak en fazla 50.000 nesne oluşturabilir. Doğrulanmış en az bir etki alanınız varsa Azure AD’deki varsayılan dizin hizmeti kotası 300.000 nesneye genişletilir. </li><li>Yönetici olmayan bir kullanıcı en fazla 250 nesne oluşturabilir. Geri yüklenebilir durumdaki etkin nesneler ve silinmiş nesneler bu kotaya dahil edilir. Yalnızca silinmesinin üzerinden 30 günden az zaman geçen nesneler geri yüklenebilir. Geri yüklenemez durumdaki silinmiş nesneler, 30 gün boyunca dörtte bir değerde bu kotaya dahil edilir. Normal görevleri sırasında bu kotayı sürekli aşabilecek, yönetici olmayan kullanıcılara [yönetici rolü atayabilirsiniz](../articles/active-directory/users-groups-roles/directory-assign-admin-roles.md).</li></ul> |
| Şema uzantıları |<ul><li>Dize türü uzantılar en fazla 256 karakter olabilir. </li><li>İkili tür uzantılar 256 bayt ile sınırlıdır.</li><li>Herhangi bir tek nesneye *tüm* türlerde ve *tüm* uygulamalarda yalnızca 100 uzantı değeri yazılabilir.</li><li>Dize türü veya ikili tür tek değerli özniteliklerle yalnızca User, Group, TenantDetail, Device, Application ve ServicePrincipal varlıkları uzatılabilir.</li><li>Şema uzantıları yalnızca Graph API'si 1.21 önizleme sürümünde kullanılabilir. Bir uzantıyı kaydetmek için uygulamaya yazma erişimi verilmelidir.</li></ul> |
| Uygulamalar |Tek bir uygulamanın sahibi en fazla 100 kullanıcı olabilir. |
| Gruplar |<ul><li>Tek bir grubun sahibi olarak en fazla 100 kullanıcı atanabilir.</li><li>Tek bir gruba üye olarak herhangi bir miktarda nesne atanabilir.</li><li>Bir kullanıcı, herhangi bir sayıda grubun üyesi olabilir.</li><li>Bir grupta, Azure AD Connect kullanarak şirket içi Active Directory'den Azure Active Directory'ye eşitleyebileceğiniz üye sayısı 50.000 ile sınırlıdır.</li></ul> |
| Uygulama Proxy'si | <ul><li>Her uygulama proxy uygulaması için saniyede en fazla 500 işlem</li><li>Kiracı için saniye başına en fazla 750 işlem</li></ul><br/>Bir işlem, benzersiz bir kaynak için tek bir http isteği ve yanıtı olarak tanımlanır. Kısıtlandığınızda, istemciler bir 429 yanıtı (çok fazla istek) alır. |
| Erişim Paneli |<ul><li>Erişim Paneli'nde kullanıcı başına görülebilecek uygulama sayısı sınırı yoktur. Bu, Azure AD Premium veya Enterprise Mobility Suite lisansları atanmış kullanıcılar için geçerlidir.</li><li>Erişim Paneli'nde her kullanıcı için en fazla 10 uygulama kutucuğu görülebilir. Bu sınır, Azure Active Directory'nin Ücretsiz veya Azure AD Temel sürümü lisansları atanmış kullanıcılar için geçerlidir. Uygulama kutucuklarına Box, Salesforce veya Dropbox örnek verilebilir. Bu sınır yönetici hesapları için geçerli değildir.</li></ul> |
| Raporlar | Herhangi bir raporda en fazla 1.000 satır görüntülenebilir veya indirilebilir. Bunun üzerindeki veriler kesilir. |
| Yönetim birimleri | Bir nesne en fazla 30 idari birimin üyesi olabilir. |
| Yönetici rolleri ve izinleri | <ul><li>Bir grup, [sahip](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context#object-ownership)olarak eklenemez.</li><li>Bir grup [role](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)atanamaz.</li><li>Yönetici olmayan kullanıcıların tüm dizin bilgilerine erişimini devre dışı bırakmak için kullanıcıların diğer kullanıcıların dizin bilgilerini okuma yeteneği, kiracı genelinde anahtar dışında kısıtlanamaz. [Burada](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context#to-restrict-the-default-permissions-for-member-users)varsayılan izinler hakkında daha fazla bilgi.</li><li>Yönetici rolü üyelik eklemeleri ve geri alma işlemleri yürürlüğe girmeden önce 15 dakika kadar sürebilir veya oturum kapatıp oturum açıp oturum açabilirsiniz.</li></ul> |
