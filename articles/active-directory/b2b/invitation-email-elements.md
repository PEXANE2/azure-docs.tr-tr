---
title: B2B davetiyesi e-postası öğeleri-Azure Active Directory | Microsoft Docs
description: Azure Active Directory B2B işbirliği davetiyesi e-posta şablonu
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0429cfb62c319675806d76b4759b776a7b32dbcb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81407205"
---
# <a name="the-elements-of-the-b2b-collaboration-invitation-email---azure-active-directory"></a>B2B işbirliği davetiyesi e-postası öğeleri-Azure Active Directory

Davet e-postaları, Azure AD 'de B2B işbirliği kullanıcıları olarak iş ortaklarını panoda getirecek kritik bir bileşendir. [B2B işbirliğiyle birini davet etmek için bir e-posta göndermeniz gerekli olmasa](add-user-without-invite.md)da, bunu yapmanız, kullanıcıya, davetinizi kabul edip etmemeyi öğrenmek için gereken tüm bilgileri verir. Ayrıca, kaynaklara geri dönmeleri gerektiğinde gelecekte her zaman başvurabileceği bir bağlantı sağlar.

![B2B davet e-postasını gösteren ekran görüntüsü](media/invitation-email-elements/invitation-email.png)

> [!NOTE]
> Bu yeni e-posta şablonu hala tüm kiracılara alındı, bu nedenle bazı kiracılar hala eski bir tasarım kullanıyor. 2020 Mayıs 'un sonuna kadar tüm kiracılardan davetiye bu şablonu kullanacaktır.

## <a name="explaining-the-email"></a>E-postayı açıklayan

Özelliklerini kullanmanın en iyi şekilde haberdar olmak için e-postanın birkaç öğesine göz atalım.

### <a name="subject"></a>Özne

E-postanın konusu bu düzene uyar:

&lt;Kullanıcı&gt; adı, BT 'yi kuruluşlarındaki uygulamalara erişmeye davet etti.

### <a name="from-address"></a>Kimden adresi

Kimden adresi için LinkedIn benzeri bir model kullanıyoruz. Bu düzen, e-postanın kaynağından invites@microsoft.comgeldiğinden, davetin başka bir kuruluştan geldiğinden emin olmak için bunu temizlemelidir. Biçim: Microsoft <invites@microsoft.com> davetleri veya Microsoft davetleri, &lt;TenantName&gt; <invites@microsoft.com>adına. 

### <a name="reply-to"></a>Yanıtla

Yanıt e-postası, mevcut olduğunda davet edilen e-postasına ayarlanır, böylece e-postayı yanıtlamak davet eden e-posta gönderir.

### <a name="phishing-warning"></a>Kimlik avı uyarısı

E-posta, kullanıcıya kimlik avı hakkında kısa bir uyarıyla başlar ve yalnızca beklediğiniz davetleri kabul etmeleri gerektiğini uyarır. Davet ettiğiniz iş ortaklarının, sizin tarafınızdan bir süre önce kendisine bahseterek davetiniz tarafından şaşırmayacak olduğundan emin olmak iyi bir uygulamadır.

![E-postadaki kimlik avı uyarısı görüntüsü](media/invitation-email-elements/phishing-warning.png)

### <a name="inviters-information"></a>Davet eden kişinin bilgileri

E-posta, davet eden ve daveti gönderdikleri kuruluş hakkındaki bilgileri içerir. Bu, gönderenin adı ve e-posta adresinin yanı sıra kuruluşla ilişkili ad ve birincil etki alanını da içerir. Bu bilgilerin tümü, davetinin daveti kabul etme konusunda bilinçli bir karar vermesini sağlamaya yardımcı olmalıdır.

![E-postadaki davet eden bilgilerin görüntüsü](media/invitation-email-elements/inviters-information.png)

### <a name="invitation-message"></a>Davet iletisi

Davet eden, [bir konuk kullanıcıyı Dizin, Grup veya uygulamaya davet](add-users-administrator.md) ettikleri ya da [davet API 'sini kullandıklarında](customize-invitation-api.md), davetinin bir parçası olarak bir ileti içeriyorsa, ileti e-postanın ana bölümünde vurgulanır. Ayrıca, davet eden kişinin adı ve profil görüntüsü bir tane ayarlandıklarında de bulunur. İletinin kendisi bir metin alanıdır, bu nedenle güvenlik nedenleriyle, HTML etiketlerini işlemez.

![E-postadaki davet iletisi görüntüsü](media/invitation-email-elements/invitation-message.png)

### <a name="accept-button-and-redirect-url"></a>Kabul et düğmesi ve yönlendirme URL 'SI

E-postanın sonraki bölümü, davetinin daveti kabul ettikten sonra nereye alınacağını öğrenmek için bir düğme içerir.  Gelecekte, davetli bu bağlantıyı, kaynaklarınıza doğrudan geri dönmek için kullanabilir.

![E-postadaki kabul düğmesi ve yeniden yönlendirme URL 'SI görüntüsü](media/invitation-email-elements/accept-button.png)

### <a name="footer-section"></a>Altbilgi bölümü

Alt bilgi, gönderilmekte olan davet hakkında daha fazla bilgi içerir. Davetli bir sonraki davetleri engellemek için her zaman bir seçenek vardır. Kuruluş [bir gizlilik bildirimi ayarlandıysa](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-properties-area), deyimin bağlantısı burada görüntülenir.  Aksi halde, kuruluşun bir gizlilik bildirimi ayarlayamadığını gösteren bir nottur.

![E-postadaki altbilgi bölümünün görüntüsü](media/invitation-email-elements/footer-section.png)
 
## <a name="how-the-language-is-determined"></a>Dilin nasıl belirlendiği

Davet e-postasında Konuk kullanıcıya sunulan dil aşağıdaki ayarlara göre belirlenir. Bu ayarlar öncelik sırasına göre listelenir. Bir ayar yapılandırılmamışsa, listedeki bir sonraki ayar dili belirler.

- Davet oluşturma API 'SI kullanılıyorsa, [Davettedusermessageınfo](https://docs.microsoft.com/graph/api/resources/invitedusermessageinfo?view=graph-rest-1.0) nesnesinin **MessageLanguage** özelliği
-   Konuğun [Kullanıcı nesnesinde](https://docs.microsoft.com/graph/api/resources/user?view=graph-rest-1.0) belirtilen **PreferredLanguage** özelliği
-   Konuk kullanıcının ana kiracının özelliklerinde ayarlanan **bildirim dili** (yalnızca Azure AD kiracılar için)
-   Kaynak kiracının özelliklerinde ayarlanan **bildirim dili**

Bu ayarlardan hiçbiri yapılandırılmazsa, dil varsayılan olarak Ingilizce (US) olur.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [Azure Active Directory yöneticileri B2B işbirliği kullanıcılarını nasıl ekler?](add-users-administrator.md)
- [Bilgi çalışanları B2B işbirliği kullanıcılarını nasıl ekler?](add-users-information-worker.md)
- [B2B işbirliği daveti satın alma](redemption-experience.md)
- [Bir davetiye olmadan B2B işbirliği kullanıcıları ekleyin](add-user-without-invite.md)
