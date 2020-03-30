---
title: include dosyası
description: include dosyası
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 02/28/2020
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: b65feeb99bb51419e19a8e4b537515be14c54ba8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334647"
---
Azure Active Directory (Azure AD) hizmetine yönelik kullanım kısıtlamalarını ve diğer hizmet sınırlarını burada bulabilirsiniz.

| Kategori | Sınır |
| --- | --- |
| Dizinler | Tek bir kullanıcı, üye veya konuk olarak en fazla 500 Azure AD dizinine ait olabilir.<br/>Tek bir kullanıcı, en fazla 20 dizin oluşturabilir. |
| Etki Alanları | En fazla 900 yönetilen etki alanı adı ekleyebilirsiniz. Şirket içi Active Directory ile tüm etki alanlarınızı federasyon için ayarlarsanız her dizinde en fazla 450 etki alanı adı ekleyebilirsiniz. |
|Kaynaklar |<ul><li>Varsayılan olarak Azure Active Directory'nin Ücretsiz sürümü kullanıcıları tarafından tek bir dizin içinde en fazla 50.000 Azure AD kaynağı oluşturulabilir. En az bir doğrulanmış etki alanınız varsa, kuruluşunuz için varsayılan Azure REKLAM hizmet kotası 300.000 Azure REKLAM kaynağına genişletilir. Bu hizmet sınırı, Azure AD fiyatlandırma sayfasındaki 500.000 kaynağın fiyatlandırma katmanı sınırıyla ilgili değildir. Varsayılan kotanın ötesine geçmek için Microsoft Destek'e başvurmanız gerekir.</li><li>Yönetici olmayan bir kullanıcı en fazla 250 Azure AD kaynağı oluşturabilir. Hem etkin kaynaklar hem de bu kotaya doğru saymak için kullanılabilir silinmiş kaynaklar. Yalnızca 30 gün önce silinen Silinen Azure REKLAM kaynakları geri yüklenir. 30 gün boyunca üç aylık bir değerle bu kotaya doğru saymak için artık kullanılamayan Silinmiş Azure REKLAM kaynakları. Normal görevleri süresince bu kotayı tekrar tekrar aşma olasılığı olan geliştiricileriniz varsa, sınırsız sayıda uygulama kaydı oluşturmak için izinle [özel bir rol oluşturabilir ve atayabilirsiniz.](../articles/active-directory/users-groups-roles/roles-quickstart-app-registration-limits.md)</li></ul> |
| Şema uzantıları |<ul><li>Dize türü uzantılar en fazla 256 karakter olabilir. </li><li>İkili tür uzantılar 256 bayt ile sınırlıdır.</li><li>*Tüm* türlerde ve *tüm* uygulamalarda yalnızca 100 uzantı değeri tek bir Azure AD kaynağına yazılabilir.</li><li>Dize türü veya ikili tür tek değerli özniteliklerle yalnızca User, Group, TenantDetail, Device, Application ve ServicePrincipal varlıkları uzatılabilir.</li><li>Şema uzantıları yalnızca Graph API'si 1.21 önizleme sürümünde kullanılabilir. Bir uzantıyı kaydetmek için uygulamaya yazma erişimi verilmelidir.</li></ul> |
| Uygulamalar |Tek bir uygulamanın sahibi en fazla 100 kullanıcı olabilir. |
|Başvuru Bildirimi |Uygulama Bildirimi'ne en fazla 1200 giriş eklenebilir. |
| Gruplar |<ul><li>Bir kullanıcı bir Azure REKLAM kuruluşunda en fazla 250 grup oluşturabilir.</li><li>Bir Azure REKLAM kuruluşunun en fazla 5000 dinamik grubu olabilir.<li>Tek bir grubun sahibi olarak en fazla 100 kullanıcı atanabilir.</li><li>Herhangi bir sayıda Azure REKLAM kaynağı tek bir grubun üyesi olabilir.</li><li>Bir kullanıcı, herhangi bir sayıda grubun üyesi olabilir.</li><li>Bir grupta, Azure AD Connect kullanarak şirket içi Active Directory'den Azure Active Directory'ye eşitleyebileceğiniz üye sayısı 50.000 ile sınırlıdır.</li><li>Azure AD'deki İç Içe Gruplar tüm senaryolarda desteklenmez</li></ul><br/> Şu anda aşağıdaki iç içe gruplar ile desteklenen senaryolar vardır.<ul><li> Bir grup başka bir grubun üyesi olarak eklenebilir ve grup iç içe geçme elde edebilirsiniz.</li><li> Grup üyelik talepleri (bir uygulama jetondaki grup üyelik taleplerini alacak şekilde yapılandırıldığında, oturum açmış kullanıcının üyesi olduğu iç içe olan gruplar dahildir)</li><li>Koşullu erişim (bir gruba koşullu erişim ilkesini kapsamalırken)</li><li>Self servis parola sıfırlama erişimi kısıtlama</li><li>Hangi kullanıcıların Azure AD Join ve cihaz kaydı yapabileceğini kısıtlama</li></ul><br/>Aşağıdaki senaryolar iç içe grupları desteklemeyin:<ul><li> Uygulama rol ataması (bir uygulamaya grup atamak desteklenir, ancak doğrudan atanan grup içinde iç içe olan grupların erişimi olmaz), hem erişim hem de sağlama için</li><li>Grup tabanlı lisanslama (bir grubun tüm üyelerine otomatik olarak lisans atama)</li><li>Office 365 Grupları.</li></ul> |
| Uygulama Ara Sunucusu | <ul><li>App Proxy uygulaması başına saniyede en fazla 500 işlem</li><li>Azure REKLAM kuruluşu için saniyede en fazla 750 işlem</li></ul><br/>Bir hareket, benzersiz bir kaynak için tek bir http isteği ve yanıtı olarak tanımlanır. Daraltıldığında, istemcilere 429 yanıt (çok fazla istek) verilecektir. |
| Erişim Paneli |<ul><li>Erişim Paneli'nde kullanıcı başına görülebilecek uygulama sayısı sınırı yoktur. Bu, Azure AD Premium veya Enterprise Mobility Suite lisansları atanmış kullanıcılar için geçerlidir.</li><li>Erişim Paneli'nde her kullanıcı için en fazla 10 uygulama kutucuğu görülebilir. Bu sınır, Azure AD Free lisans planı için lisans atanan kullanıcılar için geçerlidir. Uygulama kutucuklarına Box, Salesforce veya Dropbox örnek verilebilir. Bu sınır yönetici hesapları için geçerli değildir.</li></ul> |
| Reports | Herhangi bir raporda en fazla 1.000 satır görüntülenebilir veya indirilebilir. Bunun üzerindeki veriler kesilir. |
| Yönetim birimleri | Azure AD kaynağı en fazla 30 yönetim biriminin üyesi olabilir. |
| Yönetici rolleri ve izinleri | <ul><li>Bir grup [sahibi](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context#object-ownership)olarak eklenemez.</li><li>Bir gruba bir [role](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)atanamaz.</li><li>Kullanıcıların diğer kullanıcıların dizin bilgilerini okuma yeteneği, yönetici olmayan tüm kullanıcıların tüm dizin bilgilerine erişimini devre dışı etmek için Azure AD organizasyonu genelindeki geçiş dışında kısıtlanamaz (önerilmez). Varsayılan izinler hakkında daha fazla bilgi [burada](https://docs.microsoft.com/azure/active-directory/fundamentals/users-default-permissions?context=azure/active-directory/users-groups-roles/context/ugr-context#to-restrict-the-default-permissions-for-member-users).</li><li>Yönetici rol üyelik eklemeleri ve iptalleri yürürlüğe girmeden önce oturum açma/imzalama 15 dakika kadar sürebilir.</li></ul> |
