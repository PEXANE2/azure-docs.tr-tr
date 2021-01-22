---
title: Maliyet analizi ve bütçeler
description: Maliyet analizini nasıl alabileceğinizi, bir bütçe ayarlayacağınızı ve toplu iş yüklerinizi çalıştırmak için kullanılan temel işlem kaynakları ve yazılım lisansları için maliyetleri nasıl azaltacağınızı öğrenin.
ms.topic: how-to
ms.date: 01/21/2021
ms.openlocfilehash: 1a950f23b7ecee11dd7da5414b7eed9e87277850
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98679329"
---
# <a name="cost-analysis-and-budgets-for-azure-batch"></a>Azure Batch için maliyet analizi ve bütçeler

Toplu iş yüklerini çalıştırmak için kullanılan temeldeki işlem kaynakları ve yazılım lisansları için ücretler de kullanılabilse de Azure Batch kendisini kullanmanın bir maliyeti yoktur. Bir havuzdaki sanal makinelerden (VM), VM 'den veri aktarımına veya bulutta depolanan herhangi bir giriş ya da çıkış verilerinden maliyetler tahakkuk edebilir. Bu konu, maliyetlerin nereden geldiği, bir Batch havuzu veya hesabı için nasıl bütçe ayarlanacağı ve Batch iş yükleri için maliyetleri azaltmanın yollarını anlamanıza yardımcı olur.

## <a name="batch-resources"></a>Batch kaynakları

Sanal makineler toplu işleme için kullanılan en önemli kaynaktır. Batch için VM kullanmanın maliyeti, tür, miktar ve kullanım süresi temel alınarak hesaplanır. VM faturalandırma seçenekleri [, Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) veya [rezervasyon](../cost-management-billing/reservations/save-compute-costs-reservations.md) (önceden ödeme) içerir. Her iki ödeme seçeneğinin de, işlem iş yükünüze bağlı olarak farklı avantajları vardır ve faturanızı farklı şekilde etkiler.

Uygulamalar, [uygulama paketleri](batch-application-packages.md)kullanılarak toplu iş düğümlerine (VM 'ler) dağıtıldığında, uygulama paketlerinizin kullanacağı Azure depolama kaynakları için faturalandırılırsınız. Ayrıca, kaynak dosyaları ve diğer günlük verileri gibi herhangi bir giriş veya çıkış dosyası depolaması için faturalandırılırsınız. Genel olarak, Batch ile ilişkili depolama verilerinin maliyeti, işlem kaynakları maliyetinden çok daha düşüktür. [Sanal makine yapılandırmasıyla](nodes-and-pools.md#virtual-machine-configuration) oluşturulan HAVUZDAKI her VM 'Nin, Azure tarafından yönetilen diskleri kullanan ilişkili bir işletim sistemi diski vardır. Azure tarafından yönetilen disklerin ek bir maliyeti vardır ve diğer disk performansı katmanlarında de farklı maliyetler de vardır.

Batch havuzları ağ kaynaklarını kullanır. Özellikle, **Virtualmachineconfiguration** havuzları IÇIN statik IP adresleri gerektiren standart yük dengeleyiciler kullanılır. Batch tarafından kullanılan yük dengeleyiciler **Kullanıcı abonelik** hesapları için görünür, ancak **Batch hizmeti** hesapları için görünür değildir. Standart yük dengeleyiciler, Batch havuzu VM 'lerine ve bu VM 'lerden aktarılan tüm veriler için ücretlendirilir; Havuz düğümlerinden (görev/düğüm dosyası alma gibi) veri alan Batch API 'Lerini seçin, görev uygulama paketleri, kaynak/çıkış dosyaları ve kapsayıcı görüntüleri ücretlendirilecektir.

### <a name="additional-services"></a>Ek hizmetler

VM 'Leri ve depolamayı dahil etme hizmetleri, Batch hesabınızın maliyetine göre ücretlendirme yapabilir.

Batch ile yaygın olarak kullanılan diğer hizmetler şunlar olabilir:

- Application Insights
- Data Factory
- Azure İzleyici
- Sanal Ağ
- Grafik uygulamalarına sahip VM 'Ler

Batch çözümünüz ile kullandığınız hizmetlere bağlı olarak, ek ücretler uygulanabilir. Her ek hizmetin maliyetini öğrenmek için [Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) ' na bakın.

## <a name="cost-analysis-and-budget-for-a-pool"></a>Bir havuz için maliyet analizi ve bütçe

Azure portal, toplu Iş havuzlarınız veya Batch hesaplarınız için bütçeler ve harcama uyarıları oluşturabilir. Bütçeler ve uyarılar, fazla harcama riskleri konusunda bir gecikme süresi olması ve bir bütçeyi büyük bir süre aşmamak mümkün olsa da, paydaşların tüm risklerini bilgilendirmek için yararlıdır.

Bu örnekte, tek bir Batch havuzunun maliyet analizini inceleyeceğiz.

1. Azure portal, **maliyet yönetimi + faturalandırma** yazın veya seçin.
1. **Faturalandırma kapsamları** bölümünde aboneliğinizi seçin.
1. **Maliyet Yönetimi** bölümünde **Maliyet analizi**'ni seçin.
1. **Filtre Ekle**' yi seçin. İlk açılan kutuda **kaynak** ' ı seçin.
1. İkinci açılan kutuda Batch havuzunu seçin. Havuz seçildiğinde, havuzun maliyet analizini burada gösterilen örneğe benzer şekilde görürsünüz.
    ![Azure portal havuzun maliyet analizini gösteren ekran görüntüsü.](./media/batch-budget/pool-cost-analysis.png)

Elde edilen maliyet analizi, havuzun maliyetini ve bu maliyete katkıda bulunan kaynakları gösterir. Bu örnekte, havuzda kullanılan VM 'Ler en pahalı kaynaktır.

Havuzun bütçesini oluşturmak için **Bütçe: yok**' u seçin ve ardından **Yeni bütçe >oluştur**' u seçin. Şimdi havuzunuzun özel [bir bütçesini yapılandırmak](../cost-management-billing/costs/tutorial-acm-create-budgets.md) için pencereyi kullanın.

> [!NOTE]
> Azure Batch Azure Cloud Services ve Azure sanal makineler teknolojisinden oluşturulmuştur. **Cloud Services yapılandırma**' yı seçtiğinizde, Cloud Services fiyatlandırma yapısına göre ücretlendirilirsiniz. **Sanal makine yapılandırması**' nı seçtiğinizde, sanal makinelerin fiyatlandırma yapısına göre ücretlendirilirsiniz. Bu sayfadaki örnek, çoğu Batch havuzu için önerilen **sanal makine yapılandırmasını** kullanır.

## <a name="minimize-cost"></a>Maliyeti en aza indir

Uzun süreler için birkaç VM ve Azure hizmeti kullanmak maliyetli olabilir. İş yüklerinizin verimliliğini en üst düzeye çıkarmak ve maliyetlerinizi azaltmak için bu stratejileri kullanmayı göz önünde bulundurun.

### <a name="low-priority-virtual-machines"></a>Düşük öncelikli sanal makineler

[Düşük öncelikli VM 'ler](batch-low-pri-vms.md) , Azure 'daki daha fazla bilgi işlem kapasitesinden yararlanarak Batch iş yüklerinin maliyetini azaltır. Havuzlarınızda düşük öncelikli VM 'Ler belirttiğinizde Batch, iş yükünüzü çalıştırmak için bu fazlalığı kullanır. Adanmış VM 'ler yerine düşük öncelikli VM 'Ler kullandığınızda önemli maliyet tasarrufları olabilir.

### <a name="virtual-machine-os-disk-type"></a>Sanal makine işletim sistemi disk türü

Azure birden çok [VM işletim sistemi disk türü](../virtual-machines/disks-types.md)sunar. Çoğu VM Serisi hem Premium hem de standart depolamayı destekleyen boyutlarda. Bir havuz için bir ' VM boyutu seçildiğinde Batch, Premium SSD işletim sistemi disklerini yapılandırır. ' S ' olmayan VM boyutu seçildiğinde, standart HDD Disk türü kullanılır. Örneğin, Premium SSD işletim sistemi diskleri için kullanılır `Standard_D2s_v3` ve standart HDD işletim sistemi diskleri için kullanılır `Standard_D2_v3` .

Premium SSD işletim sistemi diskleri daha pahalıdır, ancak daha yüksek performansa sahiptir. Premium disklere sahip VM 'ler, standart HDD işletim sistemi diskleri olan VM 'lerden biraz daha hızlı başlayabilir. Batch ile, uygulamalar ve görev dosyaları VM 'nin geçici SSD disk bulunduğundan, işletim sistemi diski genellikle çoğu zaman kullanılmaz. Bu nedenle, bir ' VM boyutu belirtildiğinde sağlanan Premium SSD 'nin artan maliyetini ödemekten kaçınmak için genellikle ' s olmayan ' VM boyutunu seçebilirsiniz.

### <a name="reserved-virtual-machine-instances"></a>Ayrılmış sanal makine örnekleri

Toplu Işi uzun bir süre kullanmayı düşünüyorsanız, iş yükleriniz için [Azure ayırmaları](../cost-management-billing/reservations/save-compute-costs-reservations.md) kullanarak VM 'lerin maliyetini azaltabilirsiniz. Rezervasyon oranı, Kullandıkça Öde tarifesine göre önemli ölçüde düşüktür. Rezervasyon olmadan kullanılan sanal makine örnekleri, Kullandıkça Öde fiyatı üzerinden ücretlendirilir. Bir rezervasyon satın aldığınızda, rezervasyon indirimi uygulanır.

### <a name="automatic-scaling"></a>Otomatik ölçeklendirme

[Otomatik ölçeklendirme](batch-automatic-scaling.md) , geçerli iş taleplerine göre Batch havuzunuzdaki VM sayısını dinamik olarak ölçeklendirir. Havuzu bir işin kullanım süresine göre ölçeklendirerek, otomatik ölçeklendirme VM 'Lerin ölçeğini ve yalnızca gerçekleştirilecek bir iş olduğunda kullanılmasını sağlar. İş tamamlandığında veya iş olmadığında, işlem kaynaklarını kaydetmek için VM 'Ler otomatik olarak ölçeklendirilir. Ölçeklendirme, yalnızca ihtiyacınız olan kaynakları kullanarak Batch çözümünüzün genel maliyetini düşürmenize olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

- Batch çözümleri oluşturmak ve izlemek için kullanılabilen [Batch API 'leri ve araçları](batch-apis-tools.md) hakkında daha fazla bilgi edinin.  
- [Batch ile düşük öncelikli VM 'ler kullanma](batch-low-pri-vms.md)hakkında bilgi edinin.
