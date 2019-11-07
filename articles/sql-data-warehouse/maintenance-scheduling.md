---
title: Azure bakım zamanlamaları
description: Bakım zamanlaması, müşterilerin Azure SQL veri ambarı hizmeti 'nin yeni özellikleri, yükseltmeleri ve düzeltme eklerini almak için kullandığı gerekli zamanlanmış bakım olaylarını planlayabilmesini sağlar.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 07/16/2019
ms.author: anvang
ms.reviewer: jrasnick
ms.openlocfilehash: 91b202f8a5df841fa3d6aa1f0903999b395f8137
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73686057"
---
# <a name="use-maintenance-schedules-to-manage-service-updates-and-maintenance"></a>Bakım zamanlamalarını kullanarak hizmet güncelleştirmelerini ve bakımını yönetme

Bakım zamanlamaları artık tüm Azure SQL veri ambarı bölgelerinde kullanılabilir. Bakım zamanlaması özelliği, hizmet durumu planlı bakım bildirimlerini, Kaynak Durumu denetim Izleyicisini ve Azure SQL veri ambarı bakım zamanlaması hizmetini tümleştirir.

Yeni özellikler, yükseltmeler ve düzeltme ekleri almak için uygun olduğunda bir zaman penceresi seçmek için bakım zamanlamasını kullanırsınız. Birincil ve ikincil bakım penceresini yedi günlük bir süre içinde seçersiniz. Bu özelliği kullanmak için, ayrı gün aralıklarında bir birincil ve ikincil pencere belirlemeniz gerekir.

Örneğin, Cumartesi 22:00 ' nin birincil bir penceresini Pazar 01:00 ' e zamanlayabilir ve sonra da Çarşamba 19:00 ikincil penceresini 22:00 olarak zamanlayabilirsiniz. Birincil bakım pencereniz sırasında SQL Data Warehouse bakım gerçekleştiremediği takdirde, bakım, ikincil bakım pencereniz sırasında yeniden denenecek. Hizmet bakımı hem birincil hem de ikincil pencereler sırasında gerçekleşebilir. Tüm bakım işlemlerinin hızlı bir şekilde tamamlanmasını sağlamak için, DW400 (c) ve daha düşük veri ambarı katmanları, belirli bir bakım penceresi dışında bakım işlemini tamamlayabilir.

Yeni oluşturulan tüm Azure SQL veri ambarı örneklerinin, dağıtım sırasında uygulanan sistem tanımlı bir bakım zamanlaması olacaktır. Zamanlama, dağıtım tamamlandıktan hemen sonra düzenlenebilir.

Her bakım penceresi üç ve sekiz saat arasında olabilir. Bakım, pencerenin içinde herhangi bir zamanda gerçekleşebilir. Bakım başladığında, tüm etkin oturumlar iptal edilir ve teslim edilmemiş işlemler geri alınacaktır. Hizmet, veri ambarınıza yeni kod dağıttığı için bağlantıda birden çok kısa kayıp beklemeniz gerekir. Veri ambarı bakım işlemi tamamlandıktan hemen sonra bilgilendirilirsiniz.

 Tüm bakım işlemleri, zamanlanmış bakım pencereleri içinde bitmelidir. Belirtilen bakım pencerelerinin dışında, önceden bildirimde bulunmadan hiçbir bakım yapılmaz. Zamanlanan Bakım sırasında veri ambarınız duraklatıldıysa, sürdürme işlemi sırasında güncelleştirilir. 

## <a name="alerts-and-monitoring"></a>Uyarılar ve izleme

Hizmet durumu bildirimleri ve Kaynak Durumu Check Monitor ile tümleştirme, müşterilerin yaklaşan bakım etkinliklerinden haberdar olmasına olanak sağlar. Yeni otomasyon, Azure Izleyici 'den yararlanır. Yaklaşan bakım olayları hakkında nasıl bilgilendirilmek istediğinize karar verebilirsiniz. Ayrıca, hangi otomatik akışların kapalı kalma süresini yönetmenize ve işlemsel etkiyi en aza indirmenize yardımcı olacağını seçebilirsiniz.

24 saatlik bir öncelikli bildirim, DWC400c ve alt katmanlar için olmayan tüm bakım olaylarının önüne gelir. Örnek kapalı kalma süresini en aza indirmek için, veri ambarınızın seçtiğiniz bakım süresinden önce uzun süre çalışan işlemlere sahip olmadığından emin olun.

> [!NOTE]
> Zaman açısından kritik güncelleştirme dağıtımı için gerekli olan olayda gelişmiş bildirim süreleri önemli ölçüde azaltılabilir.

Bakımın gerçekleştireceğiz, ancak SQL veri ambarı bu süre içinde bakım gerçekleştiremediği halde bir iptal bildirimi alacaksınız. Bakım daha sonra zamanlanan bir sonraki bakım döneminde sürdürülecek.

Tüm etkin bakım olayları, **hizmet durumu-planlı bakım** bölümünde görünür. Hizmet durumu geçmişi geçmiş olayların tam kaydını içerir. Etkin bir olay sırasında Azure hizmet durumu denetimi Portal panosu aracılığıyla Bakımı izleyebilirsiniz.

### <a name="maintenance-schedule-availability"></a>Bakım zamanlaması kullanılabilirliği

Bakım zamanlaması seçtiğiniz bölgede mevcut olmasa bile, bakım zamanlamanızı dilediğiniz zaman görüntüleyebilir ve düzenleyebilirsiniz. Bölgede bakım zamanlaması kullanılabilir hale geldiğinde, belirlenen zamanlama hemen veri Ambarınızda etkin olur.

## <a name="view-a-maintenance-schedule"></a>Bakım zamanlaması görüntüleme 

### <a name="portal"></a>Portal

Varsayılan olarak yeni oluşturulan tüm Azure SQL Veri Ambarı örneklerinin dağıtım sırasında uygulanan sekiz saatlik birincil ve ikincil bakım pencereleri vardır. Yukarıda belirtildiği gibi, en kısa sürede dağıtımı tamamlandıktan sonra pencereleri değiştirebilirsiniz. Belirtilen bakım pencerelerinin dışında, önceden bildirimde bulunmadan hiçbir bakım yapılmaz.

Veri ambarınıza uygulanan bakım zamanlamasını görüntülemek için aşağıdaki adımları tamamlayın:

1.  [Azure portalında](https://portal.azure.com/) oturum açın.
2.  Görüntülemek istediğiniz veri ambarını seçin. 
3.  Seçilen veri ambarı genel bakış dikey penceresinde açılır. Veri ambarına uygulanan bakım zamanlaması **bakım zamanlaması**altında görünür.

![Genel Bakış dikey penceresi](media/sql-data-warehouse-maintenance-scheduling/clear-overview-blade.PNG)

## <a name="change-a-maintenance-schedule"></a>Bakım zamanlamasını değiştirme 

### <a name="portal"></a>Portal
Bakım zamanlaması dilediğiniz zaman güncelleştirilebilirler veya değiştirilebilir. Seçili örnek etkin bir bakım döngüsüyle gezinecekse, ayarlar kaydedilir. Bir sonraki tanımlanan bakım döneminde etkin hale gelirler. Etkin bir bakım olayı sırasında veri Ambarınızı izleme hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-health/resource-health-overview) . 

### <a name="identifying-the-primary-and-secondary-windows"></a>Birincil ve ikincil pencereleri tanımlama

Birincil ve ikincil pencerelerin ayrı gün aralıkları olmalıdır. Örnek olarak, Salı – Perşembe ve Cumartesi – Pazar ayının ikincili bir pencere yer aldığı bir örnektir.

Veri ambarınızın bakım zamanlamasını değiştirmek için aşağıdaki adımları izleyin:
1.  [Azure portalında](https://portal.azure.com/) oturum açın.
2.  Güncelleştirmek istediğiniz veri ambarını seçin. Sayfa, genel bakış dikey penceresinde açılır. 
3.  Genel Bakış dikey penceresinde **bakım zamanlaması (Önizleme) Özeti** bağlantısını seçerek bakım zamanlaması ayarları sayfasını açın. Ya da sol taraftaki kaynak menüsünde **bakım zamanlaması** seçeneğini belirleyin.  

    ![Genel Bakış dikey penceresi seçenekleri](media/sql-data-warehouse-maintenance-scheduling/maintenance-change-option.png)

4. Sayfanın üst kısmındaki seçenekleri kullanarak birincil bakım pencereniz için tercih edilen gün aralığını belirler. Bu seçim, birincil pencerenizin hafta içi veya hafta sonu üzerinde mi gerçekleşeceğini belirler. Seçiminiz, açılan değerleri güncelleştirecek. Önizleme süresince bazı bölgeler, kullanılabilir **gün** seçeneklerinin tam kümesini henüz desteklemeyebilir.

   ![Bakım ayarları dikey penceresi](media/sql-data-warehouse-maintenance-scheduling/maintenance-settings-page.png)

5. Açılan liste kutularını kullanarak tercih ettiğiniz birincil ve ikincil bakım pencerelerini seçin:
   - **Gün**: seçili pencere sırasında bakım gerçekleştirmek için tercih edilen gün.
   - **Başlangıç zamanı**: bakım penceresi için tercih edilen başlangıç zamanı.
   - **Zaman penceresi**: zaman pencerenizin tercih edilen süresi.

   Dikey pencerenin alt kısmındaki **zamanlama Özeti** alanı, seçtiğiniz değerlere göre güncelleştirilir. 
  
6. **Kaydet**’i seçin. Yeni zamanlamanız artık etkin olduğunu onaylayan bir ileti görüntülenir. 

   Bakım zamanlamasını desteklemeyen bir bölgede bir zamanlama kaydediyorsanız aşağıdaki ileti görüntülenir. Ayarlarınız kaydedilir ve bu özellik seçtiğiniz bölgede kullanılabilir hale geldiğinde etkin hale gelir.    

   ![Bölge kullanılabilirliği hakkında ileti](media/sql-data-warehouse-maintenance-scheduling/maintenance-notactive-toast.png)

## <a name="next-steps"></a>Sonraki adımlar
- Azure Izleyici 'yi kullanarak uyarı oluşturma, görüntüleme ve yönetme hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) .
- Günlük uyarı kuralları için Web kancası eylemleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) .
- [Daha fazla bilgi](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) Eylem grupları oluşturma ve yönetme.
- Azure hizmet durumu hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-health/service-health-overview) .