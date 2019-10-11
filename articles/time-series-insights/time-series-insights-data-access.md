---
title: Azure Time Series Insights önizlemeye erişmek ve bunları yönetmek için güvenliği yapılandırma | Microsoft Docs
description: Bu makalede, Azure Time Series Insights önizlemenin güvenliğini sağlamak için yönetim erişim ilkeleri ve veri erişim ilkeleri olarak güvenlik ve izinlerin nasıl yapılandırılacağı açıklanır.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 10/10/2019
ms.custom: seodec18
ms.openlocfilehash: 670615980b7fd78441a08ba987073dc139b3792a
ms.sourcegitcommit: f272ba8ecdbc126d22a596863d49e55bc7b22d37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72274446"
---
# <a name="grant-data-access-to-an-environment"></a>Bir ortama veri erişimi verme

Bu makalede, Azure Time Series Insights önizleme erişim ilkelerinin iki türü açıklanmaktadır.

## <a name="sign-in-to-time-series-insights"></a>Time Series Insights oturum açın

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.
1. Time Series Insights ortamınızı bulun. **Arama** kutusuna `Time Series` girin. Arama sonuçlarında **zaman serisi ortamını** seçin.
1. Listeden Zaman Serisi Görüşleri ortamınızı seçin.

## <a name="grant-data-access"></a>Veri erişim izni verme

Bir Kullanıcı sorumlusu için veri erişimi sağlamak üzere bu adımları izleyin.

1. **Veri erişim ilkeleri**' ni seçin ve **+ Ekle**' yi seçin.

    [![Data-Access-One](media/data-access/data-access-one.png)](media/data-access/data-access-one.png#lightbox)

1. **Kullanıcı Seç ' i**seçin. Eklemek istediğiniz kullanıcıyı bulmak için Kullanıcı adını veya e-posta adresini arayın. Seçimi onaylamak için **Seç** ' i seçin.

    [![Veri-erişim-iki](media/data-access/data-access-two.png)](media/data-access/data-access-two.png#lightbox)

1. **Rol Seç ' i**seçin. Kullanıcı için uygun erişim rolünü seçin:

    * Kullanıcının başvuru verilerini değiştirmesine ve kaydedilmiş sorguları ve perspektifleri ortamın diğer kullanıcılarıyla paylaşmasına izin vermek istiyorsanız **katılımcı** ' ı seçin.

    * Aksi takdirde, kullanıcının ortamdaki verileri sorgulayave kişisel, paylaşılmayan sorguları ortamda kaydetmesine izin vermek için **okuyucu** ' ı seçin.

   Rol seçimini onaylamak için **Tamam ' ı** seçin.

    [![ veri-erişim-üç](media/data-access/data-access-three.png)](media/data-access/data-access-three.png#lightbox)

1. **Kullanıcı rolü Seç** sayfasında **Tamam ' ı** seçin.

    [![Data-Access-dört](media/data-access/data-access-four.png)](media/data-access/data-access-four.png#lightbox)

1. **Veri erişim ilkeleri** sayfasının, her kullanıcı için kullanıcıları ve rolleri listelediğinden emin olun.

    [![Veri-erişim-beş](media/data-access/data-access-five.png)](media/data-access/data-access-five.png#lightbox)

## <a name="provide-guest-access-from-another-aad-tenant"></a>Başka bir AAD kiracısından konuk erişimi sağlama

`Guest` bir yönetim rolü değildir. Bir kiracıdan diğerine davet edilen bir hesap için kullanılan bir terimdir. Konuk hesabı kiracının dizinine davet edildikten sonra, aynı erişim denetimine diğer tüm hesap gibi uygulanabilirler. Access Control (ıAM) dikey penceresini kullanarak Time Series Insights ortamına yönetim erişimi verebilirsiniz. Ya da veri erişim Ilkeleri dikey penceresi aracılığıyla ortamdaki verilere erişim izni verebilirsiniz. Azure Active Directory (Azure AD) kiracı konuk erişimi hakkında daha fazla bilgi için [Azure Portal Add Azure ACTIVE DIRECTORY B2B işbirliği kullanıcıları](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator)' nı okuyun.

Başka bir kiracıdan bir Azure AD kullanıcısına Time Series Insights ortamına konuk erişimi sağlamak için aşağıdaki adımları izleyin.

1. **Veri erişim ilkeleri**' ni seçin ve **+ davet et**' i seçin.

    [![ veri-erişim-altı](media/data-access/data-access-six.png)](media/data-access/data-access-six.png#lightbox)

1. Davet etmek istediğiniz kullanıcının e-posta adresini girin. Bu e-posta adresinin Azure AD ile ilişkilendirilmesi gerekir. İsteğe bağlı olarak davete kişisel bir ileti ekleyebilirsiniz.

    [![ veri-erişim-yedi](media/data-access/data-access-seven.png)](media/data-access/data-access-seven.png#lightbox)

1. Ekranda görünen onay balonu olup olmadığına bakın.

    [![ veri-erişim-sekiz](media/data-access/data-access-eight.png)](media/data-access/data-access-eight.png#lightbox)

1. **Kullanıcı Seç ' i**seçin. Eklemek istediğiniz kullanıcıyı bulmak için davet ettiğiniz Konuk kullanıcının e-posta adresini arayın. Sonra, seçimi onaylamak için öğesini **seçin** .

    [![ veri-erişim-dokuz](media/data-access/data-access-nine.png)](media/data-access/data-access-nine.png#lightbox)

1. **Rol Seç ' i**seçin. Konuk Kullanıcı için uygun erişim rolünü seçin:

    * Kullanıcının başvuru verilerini değiştirmesine ve kaydedilmiş sorguları ve perspektifleri ortamın diğer kullanıcılarıyla paylaşmasına izin vermek istiyorsanız **katılımcı** ' ı seçin.

    * Aksi takdirde, kullanıcının ortamdaki verileri sorgulayave kişisel, paylaşılmayan sorguları ortamda kaydetmesine izin vermek için **okuyucu** ' ı seçin.

   Rol seçimini onaylamak için **Tamam ' ı** seçin.

    [![Veri-erişim-on](media/data-access/data-access-ten.png)](media/data-access/data-access-ten.png#lightbox)

1. **Kullanıcı rolü Seç** sayfasında **Tamam ' ı** seçin.

1. **Veri erişim ilkeleri** sayfasının Konuk kullanıcıyı ve her Konuk Kullanıcı için rolleri listelediğinden emin olun.

    [![ veri erişimi-on bir](media/data-access/data-access-eleven.png)](media/data-access/data-access-eleven.png#lightbox)

1. Artık Konuk Kullanıcı, davet ettiğiniz Azure kiracısında bulunan ortama erişmek için gereken adımları izlemelidir. İlk olarak, gönderdiğiniz daveti kabul etmiş olursunuz. Bu davet, 5. adımda kullandığınız e-posta adresine e-posta yoluyla gönderilir. Kabul etmek için **kullanmaya başlayın** ' ı seçer.

    [![Data-Access-On iki](media/data-access/data-access-twelve.png)](media/data-access/data-access-twelve.png#lightbox)

1. Sonra, Konuk Kullanıcı, yöneticinin organizasyonu ile ilişkili izinleri kabul eder.

    [![ veri erişimi-üçüncü dört](media/data-access/data-access-thirteen.png)](media/data-access/data-access-thirteen.png#lightbox)

1. Konuk Kullanıcı, davet etmek için kullandığınız e-posta adresine kaydolduktan sonra daveti kabul ettiğinde insights.azure.com adresine gider. Bu durumda, ekranın sağ üst köşesindeki e-posta adresinin yanındaki avatarını seçer.

    [![ veri erişimi-on dört](media/data-access/data-access-fourteen.png)](media/data-access/data-access-fourteen.png#lightbox)

1. Sonra, Konuk Kullanıcı, Dizin açılan menüsünden Azure kiracınızı seçer. Bu kiracı, davet ettiğiniz bir kiracdır.

    [![ veri erişimi-on beş](media/data-access/data-access-fifteen.png)](media/data-access/data-access-fifteen.png#lightbox)

Konuk Kullanıcı kiracınızı seçtikten sonra, erişimi verdiğiniz Time Series Insights ortamı görürler. Artık, **Adım 5**' te size sağladıkları rolle ilişkili tüm yetenekler vardır.

## <a name="next-steps"></a>Sonraki adımlar

* Time Series Insights ortamınıza [Azure Event Hubs olay kaynağı eklemeyi](./time-series-insights-how-to-add-an-event-source-eventhub.md) öğrenin.

* [Olayları olay kaynağına](./time-series-insights-send-events.md)gönderin.

* [Time Series Insights önizleme Gezgini ' nde ortamınızı](./time-series-insights-update-explorer.md)görüntüleyin.
