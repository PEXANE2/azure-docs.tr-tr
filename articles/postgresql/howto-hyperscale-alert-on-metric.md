---
title: Uyarıları yapılandırma - Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: Bu makalede, PostgreSQL için Azure Veritabanı için metrik uyarıların nasıl yapılandırılabildiğini ve bunlara erişilenleri açıklar - Hyperscale (Citus)
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 3/16/2020
ms.openlocfilehash: 80c061f72ce827df8f8354a5881c032c6f874fe1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063138"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure Veritabanı ölçümleri hakkında uyarılar ayarlamak için Azure portalını kullanın - Hyperscale (Citus)

Bu makalede, Azure portalını kullanarak PostgreSQL uyarıları için Azure Veritabanı'nı nasıl ayarlayabileceğiniz gösterilmektedir. Azure hizmetleriniz için [izleme ölçümlerine](concepts-hyperscale-monitoring.md) dayalı bir uyarı alabilirsiniz.

Belirli bir metnin değeri bir eşiği geçtiğinde tetiklemek için bir uyarı ayarlayacağız. Durum ilk karşılaştığında uyarı tetikler ve daha sonra tetiklemeye devam eder.

Bir uyarıyı tetiklediğinde aşağıdaki eylemleri yapmak üzere yapılandırabilirsiniz:
* Hizmet yöneticisine ve yardımcı yöneticilere e-posta bildirimleri gönderin.
* Belirttiğiniz ek e-postalara e-posta gönderin.
* Bir webhook çağırın.

Şu bilgileri kullanarak uyarı kurallarını yapılandırabilir ve bilgi alabilirsiniz:
* [Azure portalında](../azure-monitor/platform/alerts-metric.md#create-with-azure-portal)
* [Azure CLI](../azure-monitor/platform/alerts-metric.md#with-azure-cli)
* [Azure İzleyici REST API'si](https://docs.microsoft.com/rest/api/monitor/metricalerts)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Azure portalından bir metrik te uyarı kuralı oluşturma
1. Azure [portalında,](https://portal.azure.com/)izlemek istediğiniz PostgreSQL sunucusu için Azure Veritabanı'nı seçin.

2. Kenar çubuğunun **İzleme** bölümü altında, gösterildiği gibi **Uyarılar'ı** seçin:

   ![Uyarı Kurallarını Seçin](./media/howto-hyperscale-alert-on-metric/2-alert-rules.png)

3. **Yeni uyarı kuralı** (+ simgesi) seçeneğini belirleyin.

4. **Oluştur kuralı** sayfası aşağıda gösterildiği gibi açılır. Gerekli bilgileri doldurun:

   ![Metrik uyarı formu ekleme](./media/howto-hyperscale-alert-on-metric/4-add-rule-form.png)

5. **Koşul** bölümünde **Ekle'yi**seçin.

6. Uyarılacak sinyaller listesinden bir metrik seçin. Bu örnekte, "Depolama yüzdesi"ni seçin.
   
   ![Ölçüm seçin](./media/howto-hyperscale-alert-on-metric/6-configure-signal-logic.png)

7. Uyarı mantığını yapılandırın:

    * **Operatör** (ör. "Büyüktür")
    * **Eşik değeri** (ör. Yüzde 85)
    * Uyarı tetiklenmeden önce metrik kuralın karşılanması gereken süre **yi ayırma** miktarı (ör. "Son 30 dakika içinde")
    * ve **değerlendirme sıklığı** (ör. "1 dakika")
   
   Tamamlandığında **Bitti'yi** seçin.

   ![Ölçüm seçin](./media/howto-hyperscale-alert-on-metric/7-set-threshold-time.png)

8. Eylem **Grupları** bölümünde, uyarıda bildirim almak için yeni bir grup oluşturmak için **Yeni** Oluştur'u'nu seçin.

9. "Eylem grubu ekle" formunu bir ad, kısa ad, abonelik ve kaynak grubuyla doldurun.

    ![Eylem grubu](./media/howto-hyperscale-alert-on-metric/9-add-action-group.png)

10. **E-posta/SMS/Push/Voice** eylem türünü yapılandırın.
    
    Abonelik sahiplerine, katkıda bulunanlara ve okuyuculara bildirim göndermek için "Azure Kaynak Yöneticisi Rolü'ne e-posta gönder"i seçin.
   
    Tamamlandığında **Tamam'ı** seçin.

    ![Eylem grubu](./media/howto-hyperscale-alert-on-metric/10-action-group-type.png)

11. Uyarı kural adı, Açıklama ve Önem belirtin.

    ![Eylem grubu](./media/howto-hyperscale-alert-on-metric/11-name-description-severity.png) 

12. Uyarıyı oluşturmak için **uyarı oluştur kuralını** seçin.

    Birkaç dakika içinde, uyarı etkin dir ve daha önce açıklandığı gibi tetikler.

### <a name="managing-alerts"></a>Uyarıları yönetme

Bir uyarı oluşturduktan sonra, uyarıyı seçebilir ve aşağıdaki eylemleri yapabilirsiniz:

* Bu uyarıyla ilgili önceki güne ait metrik eşiği ve gerçek değerleri gösteren bir grafiği görüntüleyin.
* Uyarı kuralını **düzenle** veya **sil.**
* Bildirimleri geçici olarak durdurmak veya bildirim almaya devam etmek istiyorsanız, uyarıyı **devre dışı bırakın** veya **etkinleştirin.**

## <a name="suggested-alerts"></a>Önerilen uyarılar

### <a name="disk-space"></a>Disk alanı

İzleme ve uyarı her üretim Hyperscale (Citus) sunucu grubu için önemlidir. Altta yatan PostgreSQL veritabanının düzgün çalışması için boş disk alanı gerekir. Disk dolduğunda, veritabanı sunucusu düğümü çevrimdışı duruma gelir ve alan kullanılabilir olana kadar başlatmayı reddeder. Bu noktada, durumu düzeltmek için bir Microsoft destek isteği gerektirir.

Üretim dışı kullanımda bile her sunucu grubundaki her düğümde disk alanı uyarıları ayarlamanızı öneririz. Disk alanı kullanım uyarıları, düğümlere müdahale etmek ve düğümleri sağlıklı tutmak için gereken önceden uyarıyı sağlar. En iyi sonuçlar için % 75, %85 ve %95 kullanımda bir dizi uyarı deneyin. Hızlı veri alımı diski daha hızlı doldurduğundan, seçilecek yüzdeler veri alma hızına bağlıdır.

Disk alan sınırına yaklaştıkça, daha fazla boş alan elde etmek için şu teknikleri deneyin:

* Veri saklama ilkesini gözden geçirin. Mümkünse eski verileri soğuk depolamaya taşıyın.
* Sunucu grubuna [düğüm eklemeyi](howto-hyperscale-scaling.md#add-worker-nodes) ve parçaları yeniden dengelemeyi düşünün. Yeniden dengeleme, verileri daha fazla bilgisayara dağıtır.
* İşçi düğümlerinin [kapasitesini büyütmeyi](howto-hyperscale-scaling.md#increase-or-decrease-vcores-on-nodes) düşünün. Her çalışanın en fazla 2 TiB depolama alanı olabilir. Ancak düğüm ekleme daha hızlı tamamlandığı için düğümleri yeniden boyutlandırmadan önce düğüm ekleme denilmelidir.

### <a name="cpu-usage"></a>CPU kullanımı

CPU kullanımını izlemek, performans için bir temel oluşturmak için yararlıdır. Örneğin, CPU kullanımının genellikle %40-60 civarında olduğunu fark edebilirsiniz. CPU kullanımı aniden %95 civarında gezinmeye başlarsa, bir anomaliyi tanıyabilirsiniz. CPU kullanımı organik büyümeyi yansıtabilir, ancak başıboş bir sorguyu da ortaya çıkarabilir. CPU uyarısı oluştururken, uzun süreli artışları yakalamak ve anlık ani artışları yok saymak için uzun bir toplama tanecikliliği ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar
* [Uyarılardaki web hook'ları yapılandırma](../azure-monitor/platform/alerts-webhooks.md)hakkında daha fazla bilgi edinin.
* Hizmetinizin kullanılabilir ve yanıt veren olduğundan emin olmak için [metrik koleksiyonuna genel bir bakış](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) alın.
