---
title: Maliyet analizi ve bütçe - Azure Toplu İş
description: Toplu iş yüklerinizi çalıştırmak için kullanılan temel bilgi işlem kaynakları ve yazılım lisansları için nasıl bir maliyet analizi alacağınızı ve bütçeyi nasıl belirleyin.
services: batch
author: LauraBrenner
manager: evansma
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 07/19/2019
ms.author: labrenne
ms.openlocfilehash: 819b5e16f4730e9a1998234288e181772f7c1996
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022724"
---
# <a name="cost-analysis-and-budgets-for-azure-batch"></a>Azure Toplu İşi için maliyet analizi ve bütçeler

Azure Toplu İş'in kendisi için ücret alınmaz, yalnızca Toplu Iş yükünü çalıştırmak için kullanılan temel işlem kaynakları ve yazılım lisansları. Yüksek düzeyde, maliyetler havuzdaki sanal makinelerden (VM'ler), VM'den veri aktarımı veya bulutta depolanan herhangi bir giriş veya çıktı verisinden karşılanır. Maliyetlerin nereden geldiğini, havuz veya hesap için bütçe nin nasıl ayarlanacağını ve Toplu iş yüklerinizi daha uygun maliyetli hale getirmek için bazı teknikler belirlemek için Toplu İşlem'in bazı önemli bileşenlerine göz atalım.

## <a name="batch-resources"></a>Toplu iş kaynakları

Sanal makineler Toplu işlem için kullanılan en önemli kaynaktır. Toplu İşlemler için VM kullanma maliyeti, türüne, miktarına ve kullanım süresine göre hesaplanır. VM faturalandırma seçenekleri arasında [You-You-Go Veya](https://azure.microsoft.com/offers/ms-azr-0003p/) [Rezervasyon](../cost-management-billing/reservations/save-compute-costs-reservations.md) (peşin ödeme) yer almaktadır. Her iki ödeme seçeneğinin de işlem iş yükünüze bağlı olarak farklı avantajları vardır ve her iki ödeme modeli de faturanızı farklı şekilde etkiler.

Uygulamalar, [uygulama paketlerini](batch-application-packages.md)kullanarak Toplu İş düğümlerine (VM'ler) dağıtıldığında, uygulama paketlerinizin tükettiği Azure Depolama kaynakları için faturalandırılırsınız. Ayrıca, kaynak dosyaları ve diğer günlük verileri gibi herhangi bir giriş veya çıktı dosyasının depolanması için faturalandırılırsınız. Genel olarak, Toplu İşlem ile ilişkili depolama verilerinin maliyeti, bilgi işlem kaynaklarının maliyetinden çok daha düşüktür. **VirtualMachineConfiguration** ile oluşturulan havuzdaki her VM'de Azure yönetilen diskler kullanan ilişkili bir işletim sistemi diski bulunur. Azure tarafından yönetilen disklerin ek bir maliyeti vardır ve diğer disk performans katmanlarının da farklı maliyetleri vardır.

Toplu iş havuzları ağ kaynaklarını kullanır. Özellikle, **VirtualMachineConfiguration** havuzları için statik IP adresleri gerektiren standart yük dengeleyicileri kullanılır. Toplu İşlem tarafından kullanılan yük dengeleyicileri **Kullanıcı Aboneliği** hesapları için görünür, ancak **Toplu Servis** hesapları için görünmez. Standart yük dengeleyicileri, Toplu Havuz VM'lerine ve Toplu Havuz VM'lerinden aktarılan tüm veriler için ücrete tabidir; havuz düğümlerinden (Görev Al/Düğüm Dosyasını Al gibi), görev uygulama paketlerinden, kaynak/çıktı dosyalarından ve kapsayıcı görüntülerinden veri alan Toplu İş API'leri seçin.

### <a name="additional-services"></a>Ek hizmetler

VM'ler ve depolama dahil olmayan hizmetler Toplu hesabınızın maliyetini de hesaba katabilir.

Toplu İşlem ile yaygın olarak kullanılan diğer hizmetler şunları içerebilir:

- Application Insights
- Data Factory
- Azure İzleyici
- Sanal Ağ
- Grafik uygulamaları ile VM'ler

Toplu İşlem çözümünüzle hangi hizmetleri kullandığınıza bağlı olarak ek ücrete tabi olabilirsiniz. Her ek hizmetin maliyetini belirlemek için [Fiyatlandırma Hesaplayıcısına](https://azure.microsoft.com/pricing/calculator/) bakın.

## <a name="cost-analysis-and-budget-for-a-pool"></a>Havuz için maliyet analizi ve bütçe

Azure portalı aracılığıyla, Toplu Havuz(lar) veya Toplu İş hesabınız için bütçeler ve harcama uyarıları oluşturabilirsiniz. Bütçeler ve uyarılar, paydaşları aşırı harcama riskleri konusunda bilgilendirmek için yararlıdır. Harcama uyarılarında bir gecikme olması ve bütçeyi biraz aşması mümkündür. Bu örnekte, tek bir Toplu Iş havuzunun maliyet çözümlemesi görüntülenebilir.

1. Azure portalında, soldaki gezinti çubuğundan **Maliyet Yönetimi + Faturalandırma'yı** seçin.
1. **Aboneliklerim Bölümü'nden** aboneliğinizi seçin
1. Sol gezinme çubuğunun **Maliyet Yönetimi** bölümü altında **Maliyet analizine** gidin ve bu görünüm aşağıdaki gibi bir görünüm gösterir:
1. **Filtre Ekle'yi**seçin. İlk açılır açılır durumda Kaynak **Seç'i** ![seçin kaynak filtresini seçin](./media/batch-budget/resource-filter.png)
1. İkinci açılır durumda Toplu İşlem havuzunu seçin. Havuz seçildiğinde, maliyet çözümlemesi aşağıdaki çözümlemeye benzer.
    ![Havuzun maliyet analizi](./media/batch-budget/pool-cost-analysis.png)

Ortaya çıkan maliyet çözümlemesi, havuzun maliyetini ve bu maliyete katkıda bulunan kaynakları gösterir. Bu örnekte, havuzda kullanılan VM'ler en pahalı kaynaktır.

Havuz için bütçe oluşturmak için **Bütçe'yi seçin: yok**ve ardından **yeni bütçe >oluştur'u **seçin. Şimdi, havuzunuz için özel olarak bir bütçe yapılandırmak için pencereyi kullanın.

Bütçe yapılandırma hakkında daha fazla bilgi için Azure [bütçelerini oluştur ve yönet'](../cost-management-billing/costs/tutorial-acm-create-budgets.md)e bakın.

> [!NOTE]
> Azure Toplu İşlem, Azure Bulut Hizmetleri ve Azure Sanal Makineler teknolojisi üzerine kuruludur. **Bulut Hizmetleri Yapılandırması'nı**seçtiğinizde, Bulut Hizmetleri fiyatlandırma yapısına göre ücretlendirilirsiniz. **Sanal Makine Yapılandırması'nı**seçtiğinizde, Sanal Makineler fiyatlandırma yapısına göre ücretlendirilirsiniz. Bu sayfadaki örnekte **Sanal Makine Yapılandırması**kullanır.

## <a name="minimize-cost"></a>Maliyeti en aza indirin

Uzun süreler boyunca birden fazla VM ve Azure hizmeti kullanmak maliyetli olabilir. Neyse ki, harcamalarınızı azaltmaya yardımcı olacak hizmetlerin yanı sıra iş yükünüzün verimliliğini en üst düzeye çıkarma stratejileri de mevcuttur.

### <a name="low-priority-virtual-machines"></a>Düşük öncelikli sanal makineler

Düşük öncelikli VM'ler, Azure'daki fazla bilgi işlem kapasitesinden yararlanarak Toplu Iş Yükünün maliyetini azaltır. Havuzlarınızda düşük öncelikli VM'ler belirttiğinizde, Toplu Iş Yükü'nüzü çalıştırmak için bu fazlayı kullanır. Özel VM'ler yerine düşük öncelikli VM'ler kullanılarak önemli bir maliyet tasarrufu sağlar.

Toplu İş Ile Düşük [Öncelikli VM'leri Kullan'da](batch-low-pri-vms.md)iş yükünüz için düşük öncelikli VM'ler nasıl ayarlayişiniz hakkında daha fazla bilgi edinin.

### <a name="virtual-machine-os-disk-type"></a>Sanal makine işletim sistemi disk türü

Birden çok [VM işletim sistemi disk türü](../virtual-machines/windows/disks-types.md)vardır. Çoğu VM serisi, hem premium hem de standart depolamayı destekleyen boyutlara sahiptir. Havuz için bir 's' VM boyutu seçildiğinde, Toplu Işlem premium SSD işletim sistemi disklerini yapılandırır. 'Olmayanlar' VM boyutu seçildiğinde, daha ucuz, standart HDD disk türü kullanılır. Örneğin, birinci sınıf SSD işletim `Standard_D2s_v3` sistemi diskleri için kullanılır ve `Standard_D2_v3`standart HDD işletim sistemi diskleri için kullanılır.

Premium SSD işletim sistemi diskleri daha pahalıdır, ancak daha yüksek performansa sahiptir ve premium disklere sahip VM'ler standart HDD işletim sistemi diskleriyle VM'lerden biraz daha hızlı başlayabilir. Toplu İşlem ile, uygulamalar ve görev dosyaları VM'ler geçici SSD diskinde bulunduğundan, işletim sistemi diski genellikle çok fazla kullanılmaz. Bu nedenle, birçok durumda, bir 's' VM boyutu belirtildiğinde sağlanan prim SSD için artan maliyet ödemek için gerek yoktur.

### <a name="reserved-virtual-machine-instances"></a>Ayrılmış sanal makine örnekleri

Toplu İş'i uzun süre kullanmayı planlıyorsanız, iş yükleriniçin Azure [Rezervasyonları'nı](../cost-management-billing/reservations/save-compute-costs-reservations.md) kullanarak VM'lerin maliyetinden tasarruf edebilirsiniz. Rezervasyon oranı, bir ödeme-as-you-go oranı çok daha düşüktür. Rezervasyonsuz kullanılan sanal makine örnekleri kullandıkça öde fiyatıüzerinden ücretlendirilir. Bir rezervasyon satın alıyorsanız, rezervasyon indirimi uygulanır ve artık kullandıkça öde fiyatları üzerinden ücretlendirilmezsiniz.

### <a name="automatic-scaling"></a>Otomatik ölçeklendirme

[Otomatik ölçeklendirme,](batch-automatic-scaling.md) mevcut işin taleplerine göre Toplu İşlem havuzunuzdaki VM sayısını dinamik olarak ölçeklendiriyor. Otomatik ölçeklendirme, havuzu bir işin ömrüne göre ölçeklendirerek, VM'lerin ölçeklendirildiğinde ve yalnızca gerçekleşecek bir iş olduğunda kullanılmasını sağlar. İş tamamlandığında veya iş olmadığında, VM'ler otomatik olarak hesaplama kaynaklarını kaydetmek için küçültülir. Ölçekleme, yalnızca ihtiyacınız olan kaynakları kullanarak Toplu İşlem çözümünüzün toplam maliyetini düşürmenize olanak tanır.

Otomatik ölçeklendirme hakkında daha fazla bilgi için [bkz.](batch-automatic-scaling.md)

## <a name="next-steps"></a>Sonraki adımlar

- Toplu İş çözümleri oluşturmak ve izlemek için mevcut [Toplu API'ler ve araçlar](batch-apis-tools.md) hakkında daha fazla bilgi edinin.  

- Toplu [İşlem ile düşük öncelikli VM'ler](batch-low-pri-vms.md)hakkında bilgi edinin.
