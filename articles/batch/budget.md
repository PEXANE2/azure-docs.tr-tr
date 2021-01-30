---
title: Azure Batch için maliyet analizi edinme ve bütçeleri ayarlama
description: Maliyet analizini nasıl alabileceğinizi, bir bütçe ayarlayacağınızı ve toplu iş yüklerinizi çalıştırmak için kullanılan temel işlem kaynakları ve yazılım lisansları için maliyetleri nasıl azaltacağınızı öğrenin.
ms.topic: how-to
ms.date: 01/29/2021
ms.openlocfilehash: d1fc2d15a7037e56a8056efa67d2017badb77ffd
ms.sourcegitcommit: b4e6b2627842a1183fce78bce6c6c7e088d6157b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99091336"
---
# <a name="get-cost-analysis-and-set-budgets-for-azure-batch"></a>Azure Batch için maliyet analizi edinme ve bütçeleri ayarlama

Bu konu, Azure Batch ilişkili olabilecek maliyetleri, bir Batch havuzu veya hesabı için nasıl bir bütçe ayarlanacağını ve Batch iş yükleri için maliyetleri azaltmanın yollarını anlamanıza yardımcı olur.

## <a name="understand-costs-associated-with-batch-resources"></a>Batch kaynaklarıyla ilişkili maliyetleri anlayın

Toplu iş yüklerini çalıştırmak için kullanılan temeldeki işlem kaynakları ve yazılım lisansları için ücretler de kullanılabilse de Azure Batch kendisini kullanmanın bir maliyeti yoktur. Bir havuzdaki sanal makinelerden (VM), VM 'den veri aktarımına veya bulutta depolanan herhangi bir giriş ya da çıkış verilerinden maliyetler tahakkuk edebilir.

### <a name="virtual-machines"></a>Sanal makineler

Sanal makineler toplu işleme için kullanılan en önemli kaynaktır. Batch için VM kullanmanın maliyeti, tür, miktar ve kullanım süresi temel alınarak hesaplanır. VM faturalandırma seçenekleri [, Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) veya [rezervasyon](../cost-management-billing/reservations/save-compute-costs-reservations.md) (önceden ödeme) içerir. Her iki ödeme seçeneğinin de, işlem iş yükünüze bağlı olarak farklı avantajları vardır ve faturanızı farklı şekilde etkiler.

[Sanal makine yapılandırmasıyla](nodes-and-pools.md#virtual-machine-configuration) oluşturulan HAVUZDAKI her VM 'Nin, Azure tarafından yönetilen diskleri kullanan ilişkili bir işletim sistemi diski vardır. Azure tarafından yönetilen disklerin ek bir maliyeti vardır ve diğer disk performansı katmanlarında de farklı maliyetler de vardır.

### <a name="storage"></a>Depolama

Uygulamalar, [uygulama paketleri](batch-application-packages.md)kullanılarak toplu iş düğümlerine (VM 'ler) dağıtıldığında, uygulama paketlerinizin kullanacağı Azure depolama kaynakları için faturalandırılırsınız. Kaynak dosyaları ve diğer günlük verileri gibi herhangi bir giriş veya çıkış dosyası depolaması için de faturalandırılırsınız.

Genel olarak, Batch ile ilişkili depolama verilerinin maliyeti, işlem kaynakları maliyetinden çok daha düşüktür.

### <a name="networking-resources"></a>Ağ kaynakları

Batch havuzları, bazılarının ilişkili maliyetleri olan ağ kaynaklarını kullanır. Özellikle, sanal makine yapılandırma havuzları için, statik IP adresleri gerektiren standart yük dengeleyiciler kullanılır. Batch tarafından kullanılan yük dengeleyiciler, kullanıcı aboneliği modunda yapılandırılan [hesaplar](accounts.md#batch-accounts) için görünür, ancak Batch hizmeti modunda değildir.

Standart yük dengeleyiciler, Batch havuzu VM 'lerine ve bu VM 'lerden aktarılan tüm veriler için ücretlendirilir. Havuz düğümlerinden (görev/düğüm dosyası alma gibi) veri alan Batch API 'Lerini seçin, görev uygulama paketleri, kaynak/çıkış dosyaları ve kapsayıcı görüntüleri de ücretlendirilir.

### <a name="additional-services"></a>Ek hizmetler

Batch çözümünüz ile kullandığınız hizmetlere bağlı olarak, ek ücretler uygulanabilir. Her ek hizmetin maliyetini öğrenmek için [Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) ' na bakın. İlişkili maliyetleri olabilecek Batch ile yaygın olarak kullanılan hizmetler şunlardır:

- Application Insights
- Data Factory
- Azure İzleyici
- Sanal Ağ
- Grafik uygulamalarına sahip VM 'Ler

## <a name="view-cost-analysis-and-create-a-budget-for-a-pool"></a>Maliyet analizini görüntüleme ve havuz için bir bütçe oluşturma

Azure portal, toplu Iş havuzlarınız veya Batch hesaplarınız için bütçeler ve harcama uyarıları oluşturabilir. Bütçeler ve uyarılar, fazla harcama riskleri konusunda bir gecikme süresi olması ve bir bütçeyi büyük bir süre aşmamak mümkün olsa da, paydaşların tüm risklerini bilgilendirmek için yararlıdır.

> [!NOTE]
> Bu örnekteki havuz, çoğu havuz için önerilen **sanal makine yapılandırmasını** kullanır ve sanal makinelerin fiyatlandırma yapısına bağlı olarak ücretlendirmesi vardır. **Cloud Services yapılandırma** kullanan havuzlar Cloud Services fiyatlandırma yapısına göre ücretlendirilir.

### <a name="view-cost-analysis-for-a-batch-pool"></a>Batch havuzu için maliyet analizini görüntüleme

1. Azure portal, **maliyet yönetimi + faturalandırma** yazın veya seçin.
1. **Faturalandırma kapsamları** bölümünde aboneliğinizi seçin.
1. **Maliyet Yönetimi** bölümünde **Maliyet analizi**'ni seçin.
1. **Filtre Ekle**' yi seçin. İlk açılan kutuda **kaynak** ' ı seçin.
1. İkinci açılan kutuda Batch havuzunu seçin. Havuz seçildiğinde, havuzun maliyet analizini burada gösterilen örneğe benzer şekilde görürsünüz.
    ![Azure portal havuzun maliyet analizini gösteren ekran görüntüsü.](./media/batch-budget/pool-cost-analysis.png)

Elde edilen maliyet analizi, havuzun maliyetini ve bu maliyete katkıda bulunan kaynakları gösterir. Bu örnekte, havuzda kullanılan VM 'Ler en pahalı kaynaktır.

### <a name="create-a-budget-for-a-batch-pool"></a>Batch havuzu için bir bütçe oluşturma

1. **Maliyet Analizi** sayfasında, **Bütçe: yok**' u seçin.
1. **Yeni bütçe >oluştur**' u seçin.
1. Özel olarak, havuzunuzun bir bütçesini yapılandırmak için ortaya çıkan pencereyi kullanın. Daha fazla bilgi için bkz. [öğretici: Azure bütçeleri oluşturma ve yönetme](../cost-management-billing/costs/tutorial-acm-create-budgets.md).

## <a name="minimize-costs-associated-with-azure-batch"></a>Azure Batch ilişkili maliyetleri en aza indirin

Senaryonuza bağlı olarak, maliyetleri olabildiğince fazla azaltmak isteyebilirsiniz. İş yüklerinizin verimliliğini en üst düzeye çıkarmak ve olası maliyetleri azaltmak için bu stratejilerin bir veya daha fazlasını kullanmayı deneyin.

### <a name="use-low-priority-virtual-machines"></a>Düşük öncelikli sanal makineler kullanma

[Düşük öncelikli VM 'ler](batch-low-pri-vms.md) , Azure 'daki daha fazla bilgi işlem kapasitesinden yararlanarak Batch iş yüklerinin maliyetini azaltır. Havuzlarınızda düşük öncelikli VM 'Ler belirttiğinizde Batch, iş yükünüzü çalıştırmak için bu fazlalığı kullanır. Adanmış VM 'ler yerine düşük öncelikli VM 'Ler kullandığınızda önemli maliyet tasarrufları olabilir.

### <a name="select-a-standard-virtual-machine-os-disk-type"></a>Standart bir sanal makine işletim sistemi disk türü seçin

Azure birden çok [VM işletim sistemi disk türü](../virtual-machines/disks-types.md)sunar. Çoğu VM Serisi hem Premium hem de standart depolamayı destekleyen boyutlarda. Bir havuz için bir ' VM boyutu seçildiğinde Batch, Premium SSD işletim sistemi disklerini yapılandırır. ' S ' olmayan VM boyutu seçildiğinde, standart HDD Disk türü kullanılır. Örneğin, Premium SSD işletim sistemi diskleri için kullanılır `Standard_D2s_v3` ve standart HDD işletim sistemi diskleri için kullanılır `Standard_D2_v3` .

Premium SSD işletim sistemi diskleri daha pahalıdır, ancak daha yüksek performansa sahiptir. Premium disklere sahip VM 'ler, standart HDD işletim sistemi diskleri olan VM 'lerden biraz daha hızlı başlayabilir. Batch ile, uygulamalar ve görev dosyaları VM 'nin geçici SSD disk bulunduğundan, işletim sistemi diski genellikle çoğu zaman kullanılmaz. Bu nedenle, bir ' VM boyutu belirtildiğinde sağlanan Premium SSD 'nin artan maliyetini ödemekten kaçınmak için genellikle ' s olmayan ' VM boyutunu seçebilirsiniz.

### <a name="purchase-reservations-for-virtual-machine-instances"></a>Sanal makine örnekleri için satın alma ayırmaları

Toplu Işi uzun bir süre kullanmayı düşünüyorsanız, iş yükleriniz için [Azure ayırmaları](../cost-management-billing/reservations/save-compute-costs-reservations.md) kullanarak VM 'lerin maliyetini azaltabilirsiniz. Rezervasyon oranı, Kullandıkça Öde tarifesine göre önemli ölçüde düşüktür. Rezervasyon olmadan kullanılan sanal makine örnekleri, Kullandıkça Öde fiyatı üzerinden ücretlendirilir. Bir rezervasyon satın aldığınızda, rezervasyon indirimi uygulanır.

### <a name="use-automatic-scaling"></a>Otomatik ölçeklendirmeyi kullanma

[Otomatik ölçeklendirme](batch-automatic-scaling.md) , geçerli iş taleplerine göre Batch havuzunuzdaki VM sayısını dinamik olarak ölçeklendirir. Havuzu bir işin kullanım süresine göre ölçeklendirerek, otomatik ölçeklendirme VM 'Lerin ölçeğini ve yalnızca gerçekleştirilecek bir iş olduğunda kullanılmasını sağlar. İş tamamlandığında veya iş olmadığında, işlem kaynaklarını kaydetmek için VM 'Ler otomatik olarak ölçeklendirilir. Ölçeklendirme, yalnızca ihtiyacınız olan kaynakları kullanarak Batch çözümünüzün genel maliyetini düşürmenize olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure maliyet yönetimi + faturalandırma](../cost-management-billing/cost-management-billing-overview.md) hakkında daha fazla bilgi edinin
- [Batch ile düşük öncelikli VM 'ler kullanma](batch-low-pri-vms.md)hakkında bilgi edinin.
