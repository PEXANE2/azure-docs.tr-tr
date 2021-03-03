---
title: Planlı bakım olayları için bildirimleri (Önizleme) Ön Izleme
description: Azure SQL veritabanı veya Azure SQL yönetilen örneği için planlı bakımın önüne bildirim alın.
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 03/02/2021
ms.openlocfilehash: 3f63a8d2f75a58a771d837128c2b2498c28948ef
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101664070"
---
# <a name="advance-notifications-for-planned-maintenance-events-preview"></a>Planlı bakım olayları için bildirimleri ilerlet (Önizleme)
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Gelişmiş bildirimler (Önizleme), bakım penceresi için yapılandırılmış veritabanları için kullanılabilir [(Önizleme)](maintenance-window.md). Ön bildirimler müşterilerin, planlı herhangi bir olay için 24 saate kadar gönderilmek üzere bildirimleri yapılandırmasına olanak sağlar.

Bildirimler, planlı bakımın sonraki 24 saatte başlaması nedeniyle metinler, e-postalar, Azure anında iletme bildirimleri ve voicepostalarını alabilmeniz için yapılandırılabilir. Bakım başladığında ve bakım sona erdiğinde ek bildirimler gönderilir.


## <a name="create-an-advance-notification"></a>Ön bildirim oluştur

Gelişmiş bildirimler Azure SQL veritabanları ve bakım penceresi yapılandırılmış yönetilen örnekler için kullanılabilir. 

Bir bildirimi etkinleştirmek için aşağıdaki adımları izleyin.  

1. [Planlı bakım](https://portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/plannedMaintenance) sayfasına gidin, **sistem durumu uyarıları**' nı ve ardından **hizmet durumu uyarısı Ekle**' yi seçin.

    :::image type="content" source="media/advance-notifications/health-alerts.png" alt-text="Yeni sistem durumu uyarısı menü seçeneği oluştur":::

2. **Eylemler** bölümünde **eylem grupları Ekle**' yi seçin. 

    :::image type="content" source="media/advance-notifications/add-action-group.png" alt-text="eylem grubu menü seçeneği Ekle":::

3. **Eylem grubu oluştur** formunu tamamlayıp **İleri: bildirimler**' i seçin.  

    :::image type="content" source="media/advance-notifications/create-action-group.png" alt-text="eylem grubu formu oluştur":::

1. **Bildirimler** sekmesinde **bildirim türünü** seçin. **E-posta/SMS ileti/gönderim/ses** seçeneği en çok esnekliği sunar ve önerilen seçenektir. Bildirimi yapılandırmak için kalemi seçin.  

    :::image type="content" source="media/advance-notifications/notifications.png" alt-text="bildirimleri Yapılandır":::



   1. Açılan *Ekle veya Düzenle bildirim* formunu doldurun ve **Tamam**' ı seçin: 

   2. Eylemler ve Etiketler isteğe bağlıdır. Burada, tetiklenecek ek eylemleri yapılandırabilir veya Azure kaynaklarınızı kategorilere ayırmak ve düzenlemek için Etiketler kullanabilirsiniz. 

   4. **Gözden geçir + oluştur** sekmesinde ayrıntıları denetleyip **Oluştur**' u seçin. 

7. Oluştur ' u seçtikten sonra, uyarı kuralı yapılandırması ekranı açılır ve eylem grubu seçilir. Yeni uyarı kuralınız için bir ad verin, ardından kaynak grubunu seçin ve **Uyarı kuralı oluştur**' u seçin. 

8. **Sistem durumu uyarıları** menü öğesine yeniden tıklayın ve uyarıların listesi artık yeni uyarınızı içerir. 


İşinizi tamamlandı. Daha sonra planlı bir Azure SQL bakım olayı olduğunda, öncelikli bir bildirim alırsınız.

## <a name="receiving-notifications"></a>Bildirimleri alma

Aşağıdaki tabloda alabileceğiniz genel bilgi bildirimleri gösterilmektedir: 

|Durum|Açıklama|
|:---|:---|
|**Planlı dağıtım**| Bakım olayından 24 saat önce alındı. Bakım, DB XYZ için 5 pm-8:00 (yerel saat) arasında planlanmaktadır.|
|**Devam eden** |  *Xyz* veritabanı için bakım   başlatılıyor.| 
|**Tamamla** | *Xyz* veritabanının bakımı tamamlanmıştır. |

Aşağıdaki tabloda bakım devam ederken gönderilebilecek ek bildirimler gösterilmektedir: 

|Durum|Açıklama|
|:---|:---|
|**Genişletilmiş** | Bakım devam ediyor, ancak *xyz* veritabanı için tamamlanmadı. Bakım, sonraki bakım penceresinde devam edecektir.| 
|**İptal edildi**| *Xyz* veritabanı için bakım iptal edildi ve daha sonra yeniden zamanlanacak. |
|**Engellendi**|*Xyz* veritabanı için bakım sırasında bir sorun oluştu. Biz sürdürüyoruz, sizi bilgilendireceğiz.| 
|**Sürdürülüyor**|Sorun çözüldü ve bakım bir sonraki bakım penceresinde devam edecek.|


## <a name="next-steps"></a>Sonraki adımlar

- [Bakım penceresi](maintenance-window.md)
- [Bakım penceresi hakkında SSS](maintenance-window-faq.yml)
- [Microsoft Azure'da uyarılara genel bakış](../../azure-monitor/platform/alerts-overview.md)
- [Azure Resource Manager Rolüne E-posta Gönder](../../azure-monitor/platform/action-groups.md#email-azure-resource-manager-role)