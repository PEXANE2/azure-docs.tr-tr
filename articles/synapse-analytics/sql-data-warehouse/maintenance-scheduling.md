---
title: Synapse SQL havuzu için bakım programları
description: Bakım zamanlama, müşterilerin Azure Synapse Analytics'in yeni özellikleri, yükseltmeleri ve yamaları kullanıma çıkarmak için kullandığı zamanlanmış bakım etkinliklerini planlamalarını sağlar.
services: synapse-analytics
author: antvgski
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/02/2019
ms.author: anvang
ms.reviewer: jrasnick
ms.openlocfilehash: f193580ca03d4b1805f3c044658a34f468f3f44f
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80346553"
---
# <a name="use-maintenance-schedules-to-manage-service-updates-and-maintenance"></a>Servis güncelleştirmelerini ve bakımlarını yönetmek için bakım zamanlamalarını kullanma

Bakım çizelgesi özelliği, Azure Synapse Analytics içindeki Synapse SQL havuzu (veri ambarı) için Hizmet Durumu Planlı Bakım Bildirimleri, Kaynak Durumu Denetimi Monitörü ve bakım zamanlama hizmetini entegre eder. 

Yeni özellikler, yükseltmeler ve düzeltme eklerinde uygun olduğunda bir zaman penceresi seçmek için bakım zamanlamasını kullanmanız gerekir. Yedi günlük bir süre içinde bir birincil ve ikincil bakım penceresi seçmeniz gerekir, her pencere ayrı gün aralıkları içinde olmalıdır.

Örneğin, Cumartesi 22:00'den Pazar 01:00'e kadar birincil pencere zamanlayabilir ve ardından Çarşamba 19:00-22:00 arası ikincil bir pencere zamanlayabilirsiniz. Birincil bakım pencereniz sırasında bakım yapılamazsa, ikincil bakım pencereniz sırasında bakımı tekrar dener. Servis bakımı, hem birincil hem de ikincil pencereler sırasında zaman zaman oluşabilir. Tüm bakım işlemlerinin hızlı bir şekilde tamamlanmasını sağlamak için, DW400c ve daha düşük veri ambarı katmanları, belirlenmiş bir bakım penceresi dışında bakımı tamamlayabilir.

Yeni oluşturulan tüm veri ambarı örnekleri, dağıtım sırasında sistem tanımlı bir bakım zamanlamasına sahip olur. Zamanlama dağıtım tamamlanır tamamlanmaz düzenlenebilir.

Bakım penceresi üç ila sekiz saat arasında olsa da bu, veri ambarı süresi boyunca çevrimdışı olacağı anlamına gelmez. Bakım bu pencere içinde herhangi bir zamanda oluşabilir ve hizmet veri ambarınıza yeni kod dağıtırken bu süre boyunca ~5 -6 dakika süren tek bir bağlantı kesme beklemelisiniz. DW400c ve daha düşük bakım penceresi sırasında çeşitli zamanlarda bağlantı birden fazla kısa kayıp karşılaşabilirsiniz. Bakım başladığında, tüm etkin oturumlar iptal edilir ve taahhüt edilmeyen hareketler geri alınır. Örnek kapalı kalma süresini en aza indirmek için, seçtiğiniz bakım döneminden önce veri ambarınızın uzun süren hareketleri olmadığından emin olun.

Zamana duyarlı bir güncelleştirme dağıtmamız gerekmediği sürece, tüm bakım işlemleri belirtilen bakım pencerelerinin içinde tamamlanmalıdır. Veri ambarınız zamanlanan bir bakım sırasında duraklatılırsa, devam işlemi sırasında güncelleştirilir. Veri ambarı bakımınız tamamlandıktan hemen sonra size bildirilir.

## <a name="alerts-and-monitoring"></a>Uyarılar ve izleme

Hizmet Durumu bildirimleri ve Kaynak Durumu Denetimi Monitörü ile entegrasyon, müşterilerin yaklaşan bakım faaliyetlerinden haberdar olmasını sağlar. Bu otomasyon Azure Monitor'dan yararlanır. Yaklaşan bakım olaylarından nasıl haberdar olmak istediğinize karar verebilirsiniz. Ayrıca, hangi otomatik akışların kapalı kalma süresini yönetmenize ve operasyonel etkiyi en aza indirmenize yardımcı olacağını seçebilirsiniz.

24 saatlik bir avans bildirimi, DWC400c ve alt katmanlar için olmayan tüm bakım olaylarından önce gelir.

> [!NOTE]
> Zaman açısından kritik bir güncelleştirme dağıtmamız gerektiğinde, gelişmiş bildirim süreleri önemli ölçüde azaltılabilir.

Bakım yapılacağına dair önceden bildirim aldıysanız, ancak bakım bildirimdeki süre içinde gerçekleştirilemezse, bir iptal bildirimi alırsınız. Bakım daha sonra bir sonraki zamanlanan bakım döneminde devam edecektir.

Tüm etkin bakım olayları **Servis Durumu - Planlı Bakım** bölümünde görünür. Hizmet Durumu geçmişi, geçmiş olayların tam bir kaydını içerir. Etkin bir etkinlik sırasında Azure Hizmet Durumu denetim portalı panosu üzerinden bakım durumunu izleyebilirsiniz.

### <a name="maintenance-schedule-availability"></a>Bakım çizelgesi kullanılabilirliği

Seçtiğiniz bölgede bakım zamanlama kullanılamıyorsa bile, bakım zamanlamanızı istediğiniz zaman görüntüleyebilir ve düzenleyebilirsiniz. Bölgenizde bakım zamanlama kullanılabilir olduğunda, tanımlanan zamanlama Synapse SQL havuzunuzda hemen etkin hale gelir.

## <a name="view-a-maintenance-schedule"></a>Bakım zamanlamasını görüntüleme 

Varsayılan olarak, yeni oluşturulan tüm veri ambarı örnekleri, dağıtım sırasında sekiz saatlik birincil ve ikincil bakım penceresi uygulanır. Yukarıda belirtildiği gibi, dağıtım tamamlanır tamamlanmaz pencereleri değiştirebilirsiniz. Belirtilen bakım pencerelerinin dışında, önceden bildirimde bulunmadan hiçbir bakım yapılmaz.

Synapse SQL havuzunuza uygulanan bakım zamanlamasını görüntülemek için aşağıdaki adımları tamamlayın:

1.    [Azure portalında](https://portal.azure.com/)oturum açın.
2.    Görüntülemek istediğiniz Synapse SQL havuzunu seçin. 
3.    Seçilen Synapse SQL havuzu genel bakış bıçağında açılır. Veri ambarına uygulanan bakım çizelgesi Bakım **zamanlamasının**altında görünür.

![Genel bakış bıçağı](./media/maintenance-scheduling/clear-overview-blade.PNG)

## <a name="change-a-maintenance-schedule"></a>Bakım zamanlamasını değiştirme 

Bakım çizelgesi herhangi bir zamanda güncellenebilir veya değiştirilebilir. Seçili örnek etkin bir bakım döngüsünden geçiyorsa, ayarlar kaydedilir. Bir sonraki tanımlanmış bakım döneminde aktif hale gelecekler. Etkin bir bakım etkinliği sırasında veri ambarınızı izleme hakkında [daha fazla bilgi edinin.](../../service-health/resource-health-overview.md) 

## <a name="identifying-the-primary-and-secondary-windows"></a>Birincil ve ikincil pencereleri tanımlama

Birincil ve ikincil pencerelerayrı gün aralıklarına sahip olmalıdır. Buna bir örnek, Salı-Perşembe'nin birincil penceresi ve Cumartesi-Pazar penceresinin ikincil penceresidir.

Synapse SQL havuzunuzun bakım zamanlamasını değiştirmek için aşağıdaki adımları tamamlayın:
1.    [Azure portalında](https://portal.azure.com/)oturum açın.
2.    Güncelleştirmek istediğiniz Synapse SQL havuzunu seçin. Sayfa genel bakış bıçağında açılır. 
3.    Genel bakış bıçağındaki Bakım Çizelgesi **özet** bağlantısını seçerek bakım çizelgesi ayarları için sayfayı açın. Veya sol taraftaki kaynak menüsünde **Bakım Çizelgesi** seçeneğini belirleyin.  

    ![Genel bakış bıçak seçenekleri](./media/maintenance-scheduling/maintenance-change-option.png)

4. Sayfanın üst kısmındaki seçenekleri kullanarak birincil bakım pencereniz için tercih edilen gün aralığını belirleyin. Bu seçim, birincil pencerenizin hafta içi mi yoksa hafta sonu mı olacağını belirler. Seçiminiz açılır değerleri güncelleştirecektir. Önizleme sırasında, bazı bölgeler kullanılabilir **Gün** seçeneklerinin tam kümesini henüz desteklemeyebilir.

   ![Bakım ayarları blade](./media/maintenance-scheduling/maintenance-settings-page.png)

5. Açılan liste kutularını kullanarak tercih ettiğiniz birincil ve ikincil bakım pencerelerini seçin:
   - **Gün**: Seçilen pencerede bakım yapmak için tercih edilen gün.
   - **Başlangıç saati**: Bakım penceresi için tercih edilen başlangıç zamanı.
   - **Zaman penceresi**: Zaman pencerenizin tercih edilen süresi.

   Bıçağın altındaki **Zamanlama özet** alanı seçtiğiniz değerlere göre güncelleştirilir. 
  
6. **Kaydet'i**seçin. Yeni zamanlamanızın artık etkin olduğunu onaylayan bir ileti görüntülenir. 

   Bakım zamanlamasını desteklemeyen bir bölgede zamanlama kaydediyorsanız, aşağıdaki ileti görüntülenir. Özellik seçtiğiniz bölgede kullanıma sunulduğunda ayarlarınız kaydedilir ve etkin hale gelir.    

   ![Bölge kullanılabilirliği ile ilgili ileti](./media/maintenance-scheduling/maintenance-not-active-toast.png)

## <a name="next-steps"></a>Sonraki adımlar
- Azure Monitor'u kullanarak uyarı oluşturma, görüntüleme ve yönetme hakkında [daha fazla bilgi edinin.](../../azure-monitor/platform/alerts-metric.md)
- Günlük uyarı kuralları için webhook eylemleri hakkında [daha fazla bilgi edinin.](../..//azure-monitor/platform/alerts-log-webhook.md)
- [Daha fazla bilgi edinin](../..//azure-monitor/platform/action-groups.md) Eylem Grupları oluşturma ve yönetme.
- Azure Hizmet Durumu hakkında [daha fazla bilgi edinin.](../../service-health/service-health-overview.md)
