---
title: B2B davetiye e-postasının öğeleri - Azure Active Directory | Microsoft Dokümanlar
description: Azure Active Directory B2B işbirliği daveti e-posta şablonu
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
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81407205"
---
# <a name="the-elements-of-the-b2b-collaboration-invitation-email---azure-active-directory"></a>B2B işbirliği daveti e-postasının öğeleri - Azure Active Directory

Davet e-postaları, Iş ortaklarını Azure AD'deki B2B işbirliği kullanıcıları olarak gemiye getirmek için önemli bir bileşendir. [B2B işbirliğini kullanarak birini davet etmek için bir e-posta göndermeniz gerekli olmasa](add-user-without-invite.md)da, bunu yapmak kullanıcıya davetinizi kabul edip etmeme konusunda karar vermesi için gereken tüm bilgileri verir. Ayrıca, kaynaklarınıza geri dönmeleri gerektiğinde gelecekte her zaman başvurabilecekleri bir bağlantı da sağlar.

![B2B davet e-postasını gösteren ekran görüntüsü](media/invitation-email-elements/invitation-email.png)

> [!NOTE]
> Bu yeni e-posta şablonu hala tüm kiracılariçin dışarı haddelenmiş ediliyor, bu nedenle bazı kiracılar hala eski bir tasarım kullanıyor. Mayıs 2020 sonuna kadar, tüm kiracıların davetiyeleri bu şablonu kullanacaktır.

## <a name="explaining-the-email"></a>E-postayı açıklama

E-postanın birkaç öğesine bakalım, böylece yeteneklerini en iyi nasıl kullanacağınızı biliyorsunuz.

### <a name="subject"></a>Özne

E-postanın konusu aşağıdaki leri izler:

&lt;kullanıcı&gt; adı, sizi kendi kuruluşlarındaki uygulamalara erişmeye davet etti.

### <a name="from-address"></a>Adresten

From adresi için LinkedIn benzeri bir desen kullanıyoruz. Bu desen, e-posta , davet invites@microsoft.combaşka bir kuruluştan geliyor olsa da açıkça belirtmelidir. Biçimi: Microsoft Davetiyeler <invites@microsoft.com> veya Microsoft davetleri &lt;kiracı&gt; <invites@microsoft.com>adı adına. 

### <a name="reply-to"></a>Yanıtla

Yanıtlayan e-posta, hazır olduğunda davetlinin e-postasına ayarlanır, böylece e-postayı yanıtlamak davetliye bir e-posta gönderir.

### <a name="phishing-warning"></a>Kimlik avı uyarısı

E-posta, kullanıcıya kimlik avı hakkında kısa bir uyarı yla başlar ve onları yalnızca bekledikleri davetleri kabul etmeleri gerektiği konusunda uyarır. Davet ettiğiniz ortakların davetinize şaşırmayan ve bunu önceden onlara belirtmemenizi sağlamak iyi bir uygulamadır.

![E-postadaki kimlik avı uyarısının görüntüsü](media/invitation-email-elements/phishing-warning.png)

### <a name="inviters-information"></a>Davetli bilgileri

E-posta, daveteden kişi ve daveti gönderdikleri kuruluş hakkında bilgi içerir. Bu, gönderenin adını ve e-posta adresini ve kuruluşla ilişkili adı ve birincil etki alanını içerir. Tüm bu bilgiler, davetlinin daveti kabul etme konusunda bilinçli bir karar vermesine yardımcı olmalıdır.

![Davetlinin e-postadaki bilgilerinin görüntüsü](media/invitation-email-elements/inviters-information.png)

### <a name="invitation-message"></a>Davet iletisi

Davet eden kişi, [konuk kullanıcıyı dizine, gruba veya uygulamaya davet](add-users-administrator.md) ettiklerinde veya davet [API'sini kullandıklarında](customize-invitation-api.md)davetinin bir parçası olarak bir ileti içeriyorsa, ileti e-postanın ana bölümünde vurgulanır. Ayrıca, davetlinin adı ve profil görüntüsü de dahil. İletinin kendisi bir metin alanıdır, bu nedenle güvenlik nedenleriyle HTML etiketlerini işlemez.

![E-postadaki davet iletisinin görüntüsü](media/invitation-email-elements/invitation-message.png)

### <a name="accept-button-and-redirect-url"></a>Kabul düğmesini kabul edin ve URL'yi yeniden yönlendirin

E-postanın bir sonraki bölümünde, daveti kabul ettikten sonra davetlinin nerede alınacağı hakkında bilgi ve bunu yapmak için bir düğme yer almaktadır.  Gelecekte, davetli her zaman doğrudan kaynaklarınıza dönmek için bu bağlantıyı kullanabilirsiniz.

![Kabul düğmesinin görüntüsü ve e-postadaki URL'yi yeniden yönlendirme](media/invitation-email-elements/accept-button.png)

### <a name="footer-section"></a>Altbilgi bölümü

Altbilgi, gönderilen davet hakkında daha fazla bilgi içerir. Davetlinin gelecekteki davetlerini engellemesi her zaman bir seçenek vardır. Kuruluş bir [gizlilik bildirimi ayarladıysa,](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-properties-area)bildirimin bağlantısı burada görüntülenir.  Aksi takdirde, bir not kuruluşun bir gizlilik bildirimi ayarlamadığını gösterir.

![E-postadaki altbilgi bölümünün görüntüsü](media/invitation-email-elements/footer-section.png)
 
## <a name="how-the-language-is-determined"></a>Dil nasıl belirlenir?

Davet e-postasında konuk kullanıcıya sunulan dil aşağıdaki ayarlarla belirlenir. Bu ayarlar öncelik sırasına göre listelenir. Ayar yapılandırılmamışsa, listedeki bir sonraki ayar dili belirler.

- Davet API'si kullanılırsa [davet edilenUserMessageInfo](https://docs.microsoft.com/graph/api/resources/invitedusermessageinfo?view=graph-rest-1.0) nesnesinin **iletiDili** özelliği
-   Konuğun [kullanıcı nesnesinde](https://docs.microsoft.com/graph/api/resources/user?view=graph-rest-1.0) belirtilen **tercih edilenDil** özelliği
-   Konuk kullanıcının ev kiracısının özelliklerinde ayarlanan **Bildirim dili** (yalnızca Azure AD kiracıları için)
-   Kaynak kiracının özelliklerinde ayarlanan **Bildirim dili**

Bu ayarların hiçbiri yapılandırılmamışsa, dil varsayılan olarak İngilizce (ABD) olarak tanımlanır.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir](what-is-b2b.md)
- [Azure Active Directory yöneticileri B2B işbirliği kullanıcılarını nasıl ekler?](add-users-administrator.md)
- [Bilgi çalışanları B2B işbirliği kullanıcılarını nasıl ekler?](add-users-information-worker.md)
- [B2B işbirliği daveti itfa](redemption-experience.md)
- [B2B işbirliği kullanıcılarını davetsiz ekleme](add-user-without-invite.md)
