---
title: Azure Lab Services için maliyet yönetimi Kılavuzu
description: Laboratuvar Hizmetleri için maliyetleri görüntülemenin farklı yollarını anlayın.
author: rbest
ms.author: rbest
ms.date: 06/26/2020
ms.topic: article
ms.openlocfilehash: fbbaf4a3646260fc09467e214b82fd0213415635
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85445313"
---
# <a name="cost-management-for-azure-lab-services"></a>Azure Lab Services için maliyet yönetimi

Maliyet yönetimi iki farklı alana ayrılabilir: maliyet tahmini ve maliyet analizi.  Laboratuvarın ilk yapısının beklenen bütçeye sığmasını sağlamak için laboratuvar ayarlanırken maliyet tahmini gerçekleşir.  Maliyet Analizi genellikle ayın sonunda, maliyetleri analiz etmek ve sonraki ay için gerekli eylemleri belirlemede oluşur.

## <a name="estimating-the-lab-costs"></a>Laboratuvar maliyetlerini tahmin etme

Her laboratuvar panosunda, laboratuvarın ay için maliyeti olan kabaca bir tahmin sunan bir **maliyetler & faturalandırma** bölümü vardır.  Maliyet tahmini, en fazla kullanıcı sayısı ile saat başına tahmini maliyet kadar olan saat kullanımını özetler.  [Zamanlama](how-to-create-schedules.md)dahil olmak üzere laboratuvarın en doğru tahminini sağlamak için, pano tahmini maliyeti yansıtır.  

Bu tahmin olası tüm maliyetlerden olmayabilir, dahil olmayan birkaç kaynak vardır.  Şablon hazırlama maliyeti, maliyet tahminine uygun değildir.  Şablonu oluşturmak için gereken süre miktarının önemli ölçüde farklılık gösterebilir. Şablonu çalıştırmanın maliyeti, saat başına genel laboratuvar maliyetiyle aynıdır. Tüm [paylaşılan görüntü Galerisi](how-to-use-shared-image-gallery.md) ücretleri laboratuvar panosuna dahil değildir, çünkü Galeri birden çok laboratuvarda paylaşılabilir.  Son olarak, laboratuvar Oluşturucu başlatıldığında bir makine bu tahmine dahil edildiğinde tahakkuk eden saatler.

> [!div class="mx-imgBorder"]
> ![Pano maliyeti tahmini](./media/cost-management-guide/dashboard-cost-estimation.png)

## <a name="analyzing-previous-months-usage"></a>Önceki ayların kullanımını analiz etme

Maliyet analizi, laboratuvara yönelik ayarlamaları belirlemede yardımcı olması için önceki ayların kullanımını gözden geçirmeye yöneliktir.  Geçmişte maliyetlerin dökümü, [abonelik maliyeti analizi](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis)' nde bulunabilir.  Azure portal, üst arama alanına "abonelikler" yazabilir, ardından abonelikler seçeneğini belirleyebilirsiniz.  

> [!div class="mx-imgBorder"]
> ![Abonelik arama](./media/cost-management-guide/subscription-search.png)

Gözden geçirilecek belirli bir abonelik seçin.

> [!div class="mx-imgBorder"]
> ![Abonelik seçimi](./media/cost-management-guide/subscription-select.png)

 **Maliyet yönetimi**altındaki sol bölmede "maliyet analizi" ni seçin.

 > [!div class="mx-imgBorder"]
> ![Abonelik maliyeti Analizi](./media/cost-management-guide/subscription-cost-analysis.png)

Bu Pano, bir zamanlamaya göre farklı dosya türlerine dışarı aktarma özelliği de dahil olmak üzere derinlemesine maliyet analizine izin verir.  Maliyet yönetiminin çok fazla özelliği vardır, bkz. [maliyet yönetimi faturalandırmaya genel bakış](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview)

Kaynak türüne göre filtreleme: `microsoft.labservices/labaccounts` yalnızca laboratuvar hizmetleriyle ilişkili maliyeti gösterir.

## <a name="understanding-the-usage"></a>Kullanımı anlama

Maliyet analizinin bir örneği aşağıda verilmiştir.

> [!div class="mx-imgBorder"]
> ![Abonelik maliyeti Analizi](./media/cost-management-guide/cost-analysis.png)

Varsayılan olarak, altı sütun vardır: kaynak, kaynak türü, konum, kaynak grubu adı, Etiketler, maliyet.  Kaynak sütunu, laboratuvar hesabı, laboratuvar adı ve VM hakkındaki bilgileri içerir.  Laboratuvar hesabı/laboratuvar adı/varsayılan olan satırlar, laboratuvarın ikinci ve üçüncü satırlarda görünebilen maliyetlidir.  Kullanılan VM 'Lerin laboratuvar hesabı/laboratuvar adı/varsayılan/VM adı altında bir maliyeti olacaktır.  Bu örnekte, her ikisi de "aaalab/dockerlab" ile başlayarak ikinci satıra sahip ilk satırı eklemek, "aaalab" Laboratuvar hesabında "dockerlab" Laboratuvarı için toplam maliyeti size verecektir.

Paylaşılan görüntü Galerisi bilgilerini almak için kaynak türünü olarak değiştirin `Microsoft.Compute/Galleries` , bu da görüntü galerisinin genel maliyetini verir.  Görüntü paylaşma galerileri, galerinin nerede depolandığına bağlı olarak maliyetlerde görünmeyebilir.

> [!NOTE]
> Paylaşılan görüntü Galerisi laboratuvar hesabına bağlıdır.  Bu, birden çok laboratuvarın aynı görüntüyü kullanabileceği anlamına gelir.

## <a name="separating-costs"></a>Maliyetleri ayırma

Bazı üniversiteler, laboratuvar hesabı ve kaynak grubunu farklı sınıfları birbirinden ayıran şekilde kullandı.  Her sınıfın kendi laboratuvar hesabı ve kaynak grubu olacaktır. Maliyet Analizi bölmesinde, sınıf için uygun kaynak grubu adına sahip kaynak grubu adına dayanan bir filtre ekleyin ve yalnızca söz konusu sınıfa ait maliyetler görünür olur.  Bu, maliyetleri görüntülerken farklı sınıflar arasında daha net bir geri alma olanağı sağlar.  Maliyet analizinin [zamanlanan dışa aktarma](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data) özelliği, her sınıfın maliyetlerinin ayrı dosyalarda indirilebilmelerine izin verir.

## <a name="managing-costs"></a>Maliyetleri yönetme

Sınıfın türüne bağlı olarak, makineyi kullanarak bir öğrenci olmadan VM 'Lerin çalıştığını azaltmak için maliyetleri yönetmenin yolları vardır.

### <a name="auto-shutdown-on-disconnect"></a>Bağlantı kesildiğinde otomatik olarak kapatılıyor

Laboratuvar ortamında, laboratuvar sahibi [sanal makineye YÖNELIK RDP bağlantısı kesildiğinde](how-to-enable-shutdown-disconnect.md)laboratuvardaki VM 'leri kapatacak şekilde ayarlayabilir.  Bu ayar, öğrencinin bağlantısını kestiği ancak VM 'yi durdurmayı unutduğunuz senaryoyu azaltır.

### <a name="quota-vs-scheduled-time"></a>Kota ile zamanlanan süre

[Kota süresini](classroom-labs-concepts.md#quota) ve [zamanlanan süreyi](classroom-labs-concepts.md#schedules) anlamak, laboratuvar sahibinin Laboratuvarı, Mesleme ve öğrencilerinin ihtiyaçlarına daha iyi uyacak şekilde yapılandırmasına izin verir.  Zamanlanan saat, tüm öğrenci VM 'lerinin başlatıldığı ve bağlanabileceği bir zaman kümesidir.  Yaygın olarak zamanlanan, tüm öğrenciler kendi sanal makinesine sahip olacağı ve bu durumda, sınıf saatleri gibi gün içinde bir ayarlanan sürede en çok bir zaman olan meslem 'nin yönlerini takip eden durumlarda kullanılır.  Bu durumda, bir öğrenci VM 'de oturum açmasa bile tüm öğrenci VM 'lerinin başlatıldığı ve maliyet tahakkuk etmeleriyse.  Kota süresi her öğrenciye, her öğrenciye tahsis ettikleri ve genellikle bağımsız bir birleştirme için kullanılan süredir. Sanal makineler, öğrenci VM 'yi başlatana kadar başlatılmaz.  

Laboratuvar, kota süresi, zamanlanan süre veya her ikisinin birleşimini kullanabilir. Bir sınıfa zamanlanan süre gerekmiyorsa, VM 'lerin en etkili kullanımı için yalnızca kota süresi kullanın.

### <a name="scheduled-event---stop-only"></a>Zamanlanan olay-yalnızca durdur

Zamanlamada, yalnızca durdur olay türünü ekleyebilirsiniz, bu, belirli bir zamanda tüm makineleri durdurur.  Bazı laboratuvar sahipleri, bir öğrenci kullanmakta oldukları VM 'yi kapatmayı unutursa maliyet ve kota kullanımını azaltmak için her gün için yalnızca durdur olayı ayarladı.  Bu olay türünün alt tarafı, öğrenci VM 'yi kullanıyor olsa bile tüm VM 'Lerin kapanacaktır.

### <a name="other-costs-related-to-labs"></a>Laboratuvarlarla ilgili diğer maliyetler 

Laboratuvar hizmetlerine eklenmeyen ancak laboratuvar hizmetine bağlı olabilecek maliyetler vardır.  Paylaşılan görüntü Galerisi laboratuvarlara bağlanabilir, ancak Laboratuvar Hizmetleri maliyetlerinde gösterilmez ve maliyetleri vardır.  Genel maliyetleri devam ettirmede yardımcı olması için, görüntülerin depolama maliyetini devralma olduğundan, kullanılmayan görüntüleri Galeriden kaldırmanız gerekir.  Laboratuvarlar bir laboratuvar kaldırıldığında sanal ağ (VNet) ile diğer Azure kaynaklarına bağlantılara sahip olabilir VNet 'i ve diğer kaynakları kaldırmanız gerekir.

## <a name="conclusion"></a>Sonuç

Bundan sonra, Yukarıdaki bilgiler Kullanım maliyetlerini daha iyi anlamak ve araçların daha fazla maliyet sağlamak için nasıl kullanılacağını daha iyi anlamanızı sağlar.
