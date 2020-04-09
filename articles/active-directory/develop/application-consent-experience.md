---
title: Azure AD uygulaması onay deneyimleri
titleSuffix: Microsoft identity platform
description: Azure AD'deki uygulamaları yönetirken ve geliştirirken nasıl kullanabileceğinizi görmek için Azure AD onay deneyimleri hakkında daha fazla bilgi edinin
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: ryanwi
ms.reviewer: zachowd
ms.openlocfilehash: e96442be50a075ebf2cd81bf1b6fb0f58f883bad
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885591"
---
# <a name="understanding-azure-ad-application-consent-experiences"></a>Azure AD uygulama onayı deneyimlerini anlama

Azure Active Directory (Azure AD) uygulama izni kullanıcı deneyimi hakkında daha fazla bilgi edinin. Böylece kuruluşunuz için uygulamaları akıllıca yönetebilir ve/veya daha sorunsuz bir onay deneyimiyle uygulamaları geliştirebilirsiniz.

## <a name="consent-and-permissions"></a>İzin ve izinler

İzin, kullanıcının bir uygulamaya kendi adlarına korunan kaynaklara erişmek için yetki verme işlemidir. Bir yönetici veya kullanıcı, kendi kuruluş / bireysel verilere erişim sağlamak için izin istenebilir.

İzin vermenin gerçek kullanıcı deneyimi, kullanıcının kiracısı üzerinde belirlenen ilkelere, kullanıcının yetki kapsamına (veya rolüne) ve istemci uygulaması tarafından istenen [izinlerin](https://docs.microsoft.com/azure/active-directory/azuread-dev/v1-permissions-consent) türüne bağlı olarak farklılık gösterir. Bu, uygulama geliştiricilerin ve kiracı yöneticilerinin onay deneyimi üzerinde bazı denetimlere sahip olduğu anlamına gelir. Yöneticiler, kiracılarındaki onay deneyimini denetlemek için kiracı veya uygulama üzerindeki ilkeleri ayarlama ve devre dışı bırakma esnekliğine sahiptir. Uygulama geliştiricileri, hangi izin türlerinin istendiğini ve kullanıcılara kullanıcı onayı akışı veya yönetici onayı akışı nda rehberlik etmek isteyip istemediklerini belirleyebilir.

- **Kullanıcı onayı akışı,** bir uygulama geliştiricisinin yalnızca geçerli kullanıcı için onay kaydetmek amacıyla kullanıcıları yetkilendirme bitiş noktasına yönlendirmesidir.
- **Yönetici onayı akışı,** bir uygulama geliştiricisinin tüm kiracıiçin onay kaydetmek amacıyla kullanıcıları yönetici onayı bitiş noktasına yönlendirmesidir. Yönetici onayı akışının düzgün çalıştığından emin olmak için, `RequiredResourceAccess` uygulama geliştiricilerin indeki tüm izinleri uygulama bildiriminde listelemesi gerekir. Daha fazla bilgi için [Uygulama bildirimine](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest)bakın.

## <a name="building-blocks-of-the-consent-prompt"></a>Onay isteminin yapı taşları

Onay istemi, kullanıcıların istemci uygulamasına kendi adlarına korumalı kaynaklara erişecek şekilde güvenip güvenmediklerini belirlemek için yeterli bilgiye sahip olmalarını sağlamak için tasarlanmıştır. Yapı taşlarının anlaşılması, onay veren kullanıcıların daha bilinçli kararlar almalarına ve geliştiricilerin daha iyi kullanıcı deneyimleri oluşturmalarına yardımcı olacaktır.

Aşağıdaki diyagram ve tablo, onay isteminin yapı taşları hakkında bilgi sağlar.

![Onay isteminin yapı taşları](./media/application-consent-experience/consent_prompt.png)

| # | Bileşen | Amaç |
| ----- | ----- | ----- |
| 1 | Kullanıcı tanımlayıcısı | Bu tanımlayıcı, istemci uygulamasının adına korumalı kaynaklara erişmek istediği kullanıcıyı temsil eder. |
| 2 | Başlık | Başlık, kullanıcıların kullanıcı veya yönetici onay akışından geçip geçmediğine bağlı olarak değişir. Kullanıcı onayı akışında başlık "İstenilen izinler" olurken, yönetici onayı akışında başlıkta "Kuruluşunuz için kabul et" satırı ek olacaktır. |
| 3 | Uygulama logosu | Bu resim, kullanıcıların bu uygulamanın erişmek istedikleri uygulama olup olmadığına işaret etmelerine yardımcı olmalıdır. Bu görüntü uygulama geliştiricileri tarafından sağlanır ve bu görüntünün sahipliği doğrulanmaz. |
| 4 | Uygulama adı | Bu değer, kullanıcılara hangi uygulamanın verilerine erişim istediğini bildirmelidir. Bu adın geliştiriciler tarafından sağlandığını ve bu uygulama adının sahipliğinin doğrulanmadığını unutmayın. |
| 5 | Yayımcı etki alanı | Bu değer, kullanıcılara güvenilirliği için değerlendirebilecekleri bir etki alanı sağlamalıdır. Bu etki alanı geliştiriciler tarafından sağlanır ve bu yayımcı etki alanının sahipliği doğrulanır. |
| 6 | İzinler | Bu liste, istemci uygulaması tarafından istenen izinleri içerir. Kullanıcılar, istemci uygulamasının kabul etmeleri halinde kendi adlarına hangi verilere erişmeye yetkili olacağını anlamak için her zaman istenen izin türlerini değerlendirmelidir. Bir uygulama geliştiricisi olarak en az ayrıcalığa sahip izinlere erişim istemek en iyisidir. |
| 7 | İzin açıklaması | Bu değer, izinleri açığa çıkaran hizmet tarafından sağlanır. İzin açıklamalarını görmek için, izin yanındaki köşeli ayraç tanınmalısınız. |
| 8 | Uygulama terimleri | Bu koşullar, uygulamanın hizmet koşulları ve gizlilik bildirimine bağlantılar içerir. Yayıncı, kendi hizmet koşullarına göre kurallarını ana hatlarıyla özetlemelerinden sorumludur. Ayrıca, yayımcı, kullanıcı verilerini kullanma ve paylaşma biçimini gizlilik bildiriminde ifşa etmekten de sorumludur. Yayımcı, çok kiracılı uygulamalar için bu değerlere bağlantı sağlamazsa, onay isteminde kalın bir uyarı olacaktır. |
| 9 | https://myapps.microsoft.com | Bu, kullanıcıların şu anda verilerine erişimi olan Microsoft dışı uygulamaları gözden geçirip kaldırabilecekleri bağlantıdır. |

## <a name="common-consent-scenarios"></a>Yaygın onay senaryoları

Bir kullanıcının ortak izin senaryolarında görebileceği onay deneyimleri şunlardır:

1. Yetki kapsamlarında bir izin kümesi gerektiren bir uygulama erişen kişiler, kullanıcı onayı akışına yönlendirir.
    
    1. Yöneticiler, tüm kiracı adına rıza göstermelerine olanak sağlayacak geleneksel onay istemiüzerinde ek bir denetim görürler. Denetim varsayılan olarak kapatılır, bu nedenle yalnızca yöneticiler kutuyu açıkça işaretlediğinde tüm kiracı adına izin verilir. Bugün itibariyle, bu onay kutusu yalnızca Global Admin rolü için gösterecektir, bu nedenle Bulut Yöneticisi ve App Admin bu onay kutusunu görmez.

        ![Senaryo 1a için onay istemi](./media/application-consent-experience/consent_prompt_1a.png)
    
    2. Kullanıcılar geleneksel onay istemini görür.

        ![Senaryo 1b için onay istemi](./media/application-consent-experience/consent_prompt_1b.png)

2. Yetki kapsamları dışında en az bir izin gerektiren bir uygulamaya erişen kişiler.
    1. Yöneticiler yukarıda gösterilen 1.i ile aynı istemi görürler.
    2. Kullanıcıların uygulamaya onay vermesi engellenir ve yöneticilerinden uygulamaya erişim istemeleri söylenir. 
                
        ![Senaryo 1b için onay istemi](./media/application-consent-experience/consent_prompt_2b.png)

3. Yönetici onay akışında gezinen veya yönlendirilen kişiler.
    1. Yönetici kullanıcıları yönetici onay istemini görür. Başlık ve izin açıklamaları bu istekte değiştirildi, değişiklikler bu istemi kabul uygulamanın tüm kiracı adına istenen verilere erişim izni verecektir gerçeğini vurgulamak.
        
        ![Senaryo 1b için onay istemi](./media/application-consent-experience/consent_prompt_3a.png)
        
    1. Yönetici olmayan kullanıcılar yukarıda gösterilen 2.ii ile aynı ekranı görürler.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure AD onay çerçevesinin onayı nasıl uyguladığına adım](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)adım genel bir bakış alın.
- Daha fazla derinlik için, çok kiracılı bir uygulamanın daha gelişmiş çok katmanlı uygulama modellerini destekleyerek "kullanıcı" ve "yönetici" onayı uygulamak için [onay çerçevesini nasıl kullanabileceğini](active-directory-devhowto-multi-tenant-overview.md) öğrenin.
- [Uygulamanın yayımcı etki alanını nasıl yapılandırıştırılamayı](howto-configure-publisher-domain.md)öğrenin.
