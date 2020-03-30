---
title: Veri erişimi sağlamak için güvenliği yapılandırın - Azure Time Series Öngörüleri Önizleme | Microsoft Dokümanlar
description: Azure Zaman Serisi Öngörüler Önizleme ortamınızda güvenliği, izinleri nasıl yapılandırabileceğinizi ve veri erişim ilkelerini nasıl yöneteceklerinizi öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 01/10/2020
ms.custom: seodec18
ms.openlocfilehash: 1c8f14bb1bca082a9d887e5d6d88aec213448c3e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254358"
---
# <a name="grant-data-access-to-an-environment"></a>Bir ortama veri erişimi verme

Bu makalede, iki tür Azure Zaman Serisi Öngörüleri Önizleme erişim ilkeleri anlatılmaktadır.

> [!TIP]
> Azure Active Directory uygulaması kayıt adımları için [Kimlik Doğrulama ve Yetkilendirme'yi](time-series-insights-authentication-and-authorization.md) okuyun.

## <a name="sign-in-to-time-series-insights"></a>Time Series Insights'ta oturum açın

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. Time Series Insights ortamınızı bulun. `Time Series` **Arama** kutusuna girin. Arama sonuçlarında **Zaman Serisi Ortamı'nı** seçin.
1. Listeden Zaman Serisi Görüşleri ortamınızı seçin.

## <a name="grant-data-access"></a>Veri erişim izni verme

Bir kullanıcı ilkesi için veri erişimi sağlamak için aşağıdaki adımları izleyin.

1. **Veri Erişim İlkeleri'ni**seçin ve sonra **+ Ekle'yi**seçin.

    [![Veri Erişim İlkesi seçin ve ekleyin](media/data-access/data-access-select-add-button.png)](media/data-access/data-access-select-add-button.png#lightbox)

1. **Kullanıcı seç'i**seçin. Eklemek istediğiniz kullanıcıyı bulmak için kullanıcı adını veya e-posta adresini arayın. Seçimi onaylamak için **Seç'i** seçin.

    [![Eklemek için bir kullanıcı seçin](media/data-access/data-access-select-user-to-confirm.png)](media/data-access/data-access-select-user-to-confirm.png#lightbox)

1. **Rolü Seç'i**seçin. Kullanıcı için uygun erişim rolünü seçin:

    * Kullanıcının başvuru verilerini değiştirmesine ve kaydedilen sorguları ve perspektifleri ortamın diğer kullanıcılarıyla paylaşmasına izin vermek istiyorsanız **Katılımcı'yı** seçin.

    * Aksi takdirde, kullanıcının verileri ortamda sorgulamasına ve paylaşılan olmayan, çevrimiçi sorguları kaydetmesine izin vermek için **Reader'ı** seçin.

   Rol seçimini onaylamak için **Tamam'ı** seçin.

    [![Seçili rolü onaylama](media/data-access/data-access-select-a-role.png)](media/data-access/data-access-select-a-role.png#lightbox)

1. Kullanıcı Rolü **Seç** sayfasında **Tamam'ı** seçin.

    [![Kullanıcı Rolü Seç sayfasında Tamam'ı seçin](media/data-access/data-access-confirm-user-and-role.png)](media/data-access/data-access-confirm-user-and-role.png#lightbox)

1. **Veri Erişim İlkeleri** sayfasının her kullanıcı için kullanıcıları ve rolleri listelediğini doğrulayın.

    [![Doğru kullanıcıları ve rolleri doğrulama](media/data-access/data-access-verify-and-confirm-assignments.png)](media/data-access/data-access-verify-and-confirm-assignments.png#lightbox)

## <a name="provide-guest-access-from-another-azure-ad-tenant"></a>Başka bir Azure AD kiracısından konuk erişimi sağlama

Rol `Guest` bir yönetim rolü değildir. Bir kiracıdan diğerine davet edilen bir hesap için kullanılan bir terimdir. Konuk hesabı kiracının dizinine davet edildikten sonra, diğer tüm hesaplar gibi aynı erişim denetimine sahip olabilir. Erişim Denetimi (IAM) bıçaklarını kullanarak bir Zaman Serisi Öngörüler Ortamına yönetim erişimi verebilirsiniz. Ya da Veri Erişim İlkeleri bıçağı aracılığıyla ortamdaki verilere erişim izni verebilirsiniz. Azure Active Directory (Azure AD) kiracı konuk erişimi hakkında daha fazla bilgi için Azure [portalındaki Azure Etkin Dizin B2B işbirliği kullanıcılarını ekle'yi](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator)okuyun.

Bir Zaman Serisi Öngörüleri ortamına başka bir kiracıdan gelen bir Azure REKLAM kullanıcısına konuk erişimi sağlamak için aşağıdaki adımları izleyin.

1. **Veri Erişim İlkeleri'ni**seçin ve ardından **+ Davet et'i**seçin.

    [![Veri Erişim Polisleri'ni seçin, ardından + Davet Et](media/data-access/data-access-invite-another-aad-tenant.png)](media/data-access/data-access-invite-another-aad-tenant.png#lightbox)

1. Davet etmek istediğiniz kullanıcının e-posta adresini girin. Bu e-posta adresi Azure AD ile ilişkilendirilmelidir. İsteğe bağlı olarak davete kişisel bir ileti ekleyebilirsiniz.

    [![Seçili kullanıcıyı bulmak için e-posta adresini girin](media/data-access/data-access-invite-guest-by-email.png)](media/data-access/data-access-invite-guest-by-email.png#lightbox)

1. Ekranda görünen onay baloncuğuna bakın.

    [![Görünmesi için onay balonu arayın](media/data-access/data-access-confirmation-bubble.png)](media/data-access/data-access-confirmation-bubble.png#lightbox)

1. **Kullanıcı seç'i**seçin. Eklemek istediğiniz kullanıcıyı bulmak için davet ettiğiniz konuk kullanıcının e-posta adresini arayın. Ardından, seçimi onaylamak için **seçin.**

    [![Kullanıcıyı seçin ve seçimi onaylayın](media/data-access/data-access-select-invited-person-confirmation.png)](media/data-access/data-access-select-invited-person-confirmation.png#lightbox)

1. **Rolü Seç'i**seçin. Konuk kullanıcı için uygun erişim rolünü seçin:

    * Kullanıcının başvuru verilerini değiştirmesine ve kaydedilen sorguları ve perspektifleri ortamın diğer kullanıcılarıyla paylaşmasına izin vermek istiyorsanız **Katılımcı'yı** seçin.

    * Aksi takdirde, kullanıcının verileri ortamda sorgulamasına ve paylaşılan olmayan, çevrimiçi sorguları kaydetmesine izin vermek için **Reader'ı** seçin.

   Rol seçimini onaylamak için **Tamam'ı** seçin.

    [![Rol seçimini onaylama](media/data-access/data-access-select-ok-and-confirm.png)](media/data-access/data-access-select-ok-and-confirm.png#lightbox)

1. Kullanıcı Rolü **Seç** sayfasında **Tamam'ı** seçin.

1. **Veri Erişim İlkeleri** sayfasının konuk kullanıcıyı ve her konuk kullanıcı için rolleri listelediğini doğrulayın.

    [![Kullanıcıların ve rollerin doğru şekilde atandığını doğrulama](media/data-access/data-access-confirm-invited-users-and-roles.png)](media/data-access/data-access-confirm-invited-users-and-roles.png#lightbox)

1. Şimdi, konuk kullanıcı yukarıda belirtilen e-posta adresinden bir davet e-posta alacaksınız. Konuk kullanıcı, kabullerini onaylamak ve Azure Bulut'a bağlanmak için **Başlat'ı** seçer.

    [![Konuk kabul etmeye başla'yı seçer](media/data-access/data-access-email-invitation.png)](media/data-access/data-access-email-invitation.png#lightbox)

1. **Başlat'ı**seçtikten sonra konuk kullanıcıya yöneticinin organizasyonuyla ilişkili bir izin kutusu sunulur. **Kabul'i**seçerek izin verildikten sonra, oturum açılacaktır.

    [![Konuk, izinleri gözden geçirir ve kabul eder](media/data-access/data-access-grant-permission-sign-in.png)](media/data-access/data-access-grant-permission-sign-in.png#lightbox)

1. Yönetici, [çevre URL'sini](time-series-insights-parameterized-urls.md) konuklarıyla paylaşır.

1. Konuk kullanıcı, davet ettiğiniz e-posta adresinde oturum açtıktan ve daveti kabul ettikten sonra Azure portalına yönlendirilir. 

1. Konuk artık yönetici tarafından sağlanan ortam URL'sini kullanarak paylaşılan ortama erişebilir. Anında erişmek için bu URL'yi web tarayıcılarına girebilirler.

1. Yöneticinin kiracısı, Zaman Serisi gezgininin sağ üst köşesindeki profil simgesini seçtikten sonra konuk kullanıcıya görüntülenir.

    [![insights.azure.com avatar seçimi](media/data-access/data-access-select-tenant-and-instance.png)](media/data-access/data-access-select-tenant-and-instance.png#lightbox)


    Konuk kullanıcı yöneticinin kiracısını seçtikten sonra paylaşılan Time Series Öngörüleri ortamını seçebilme yeteneğine sahip olur. 
    
    Onlar şimdi **adım 5**onlara sağlanan rolü ile ilgili tüm yetenekleri var.

    [![Konuk kullanıcı Azure kiracınızı açılır yerden seçer](media/data-access/data-access-all-capabilities.png)](media/data-access/data-access-all-capabilities.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

* Zaman Serisi Öngörüleri ortamınıza [Azure Etkinlik Hub'ları etkinlik kaynağını nasıl ekleyeceğinizi](./time-series-insights-how-to-add-an-event-source-eventhub.md) öğrenin.

* [Olayları olay kaynağına](./time-series-insights-send-events.md)gönderin.

* [Zaman Serisi Öngörüler Önizleme gezgininde ortamınızı görüntüleyin.](./time-series-insights-update-explorer.md)
