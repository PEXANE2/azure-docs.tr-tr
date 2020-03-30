---
title: Azure Danışmanı'nı kullanarak hizmet maliyetlerini azaltın
description: Azure dağıtımlarınızın maliyetini optimize etmek için Azure Danışmanı'nı kullanın.
ms.topic: article
ms.date: 01/29/2019
ms.openlocfilehash: 0237feab59551ecab87d78b0d4d66b9fc7b47e90
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259701"
---
# <a name="reduce-service-costs-using-azure-advisor"></a>Azure Danışmanı'nı kullanarak hizmet maliyetlerini azaltın

Danışman, boşta kalan ve yeterince kullanılmayan kaynakları tanımlayarak genel Azure harcamanızı optimize etmenize ve azaltmanıza yardımcı olur.Danışman panosundaki **Maliyet** sekmesinden maliyet önerileri alabilirsiniz.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>Az kullanılan örnekleri yeniden boyutlandırarak veya kapatarak sanal makineyi harcamalarını iyileştirin 

Bazı uygulama senaryoları tasarım gereği düşük kullanıma neden olsa da, genellikle sanal makinelerinizin boyutunu ve sayısını yöneterek paradan tasarruf edebilirsiniz. Danışman gelişmiş değerlendirme modelleri, CPU kullanımının maksimum değerinin maksimum değerinin P95'inin %3'ten az olması ve ağ kullanımının 7 günlük bir süre içinde %2'den az olması nedeniyle sanal makineleri kapatma için dikkate alır. Sanal makineler, mevcut yükü daha küçük bir SKU'ya (aynı SKU ailesi içinde) veya daha küçük bir sayıda ki, kullanıcı olmayan iş yüküyle karşılaştığında %80'in üzerinde, kullanıcıya dönük iş yükünde ise %40'ın üzerinde olmayan bir şekilde sığdırmak mümkün olduğunda doğru boyut için kabul edilir. Burada, iş yükü türü iş yükünün CPU kullanım özellikleri analiz edilerek belirlenir.

Önerilen eylemler, önerilen kaynağa özgü olarak kapatılır veya yeniden boyutlandırılır. Danışman, önerilen eylemler için tahmini maliyet tasarruflarını gösterir - yeniden boyutlandırma veya kapatma. Ayrıca, önerilen eylemi yeniden boyutlandırmak için Danışman güncel ve hedef SKU bilgileri sağlar. 

Az kullanılan sanal makineleri tanımlamada daha agresif olmak istiyorsanız, abonelik başına CPU kullanım kuralını ayarlayabilirsiniz.

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Hükümsüz ExpressRoute devrelerini ortadan kaldırarak maliyetleri azaltın

Danışman, bir aydan uzun süredir *Prothe sayılmayan* sağlayıcı durumunda olan ExpressRoute devrelerini tanımlar ve devreyi bağlantı sağlayıcınızla birlikte vermeyi planlamadığınız takdirde devrenin silmesiönerir.

## <a name="reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways"></a>Boştaki sanal ağ geçitlerini silerek veya yeniden yapılandırarak maliyetleri azaltın

Danışman, 90 günden uzun süredir boşta olan sanal ağ ağ ağ geçitlerini tanımlar. Bu ağ geçitleri saatlik olarak faturalandırıldığından, artık kullanmak istemiyorsanız yeniden yapılandırmayı veya silmeyi düşünmelisiniz. 

## <a name="buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs"></a>Kullandıkça öde maliyetlerinden tasarruf sağlamak için ayrılmış sanal makine örnekleri satın alın

Danışman, son 30 gün içinde sanal makine kullanımınızı gözden geçirecek ve bir Azure rezervasyonu satın alarak paradan tasarruf edip edemeyeceğinize karar verecektir. Danışman size potansiyel olarak en fazla tasarruf yaptığınız bölgeleri ve boyutları gösterecek ve satın alma rezervasyonlarından elde edilen tahmini tasarrufları size gösterecektir. Azure rezervasyonları ile sanal makinelerinizin temel maliyetlerini önceden satın alabilirsiniz. İndirimler, rezervasyonlarınız ile aynı boyut ve bölgeye sahip yeni veya mevcut VM'ler için otomatik olarak geçerli olacaktır. [Azure Ayrılmış VM Örnekleri hakkında daha fazla bilgi edinin.](https://azure.microsoft.com/pricing/reserved-vm-instances/)

Danışman ayrıca, önümüzdeki 30 gün içinde süresi dolacak olan ayrılmış örnekleri size bildirir. Bir anda öde fiyatlandırması ödemekten kaçınmak için yeni ayrılmış örnekler satın almanızı önerir.

## <a name="delete-unassociated-public-ip-addresses-to-save-money"></a>Para kazanmak için ilişkili olmayan genel IP adreslerini silme

Danışman, Yük Dengeleyicileri veya VM'ler gibi Şu anda Azure kaynaklarıyla ilişkili olmayan genel IP adreslerini tanımlar. Bu genel IP adresleri nominal bir ücretle birlikte gelir. Bunları kullanmayı planlamamanız, bunları silmeniz maliyet tasarrufuna neden olabilir.

## <a name="delete-azure-data-factory-pipelines-that-are-failing"></a>Başarısız olan Azure Data Factory işlem hatlarını silin

Azure Danışmanı, sürekli olarak başarısız olan Azure Veri Fabrikası ardışık hatlarını algılar ve artık ihtiyaç duyulmadığı takdirde sorunları çözmenizi veya başarısız olan ardışık hatları silmenizi önerir. Başarısız olurken size hizmet etmeseler bile bu boru hatları için faturalandırılırsınız. 

## <a name="use-standard-snapshots-for-managed-disks"></a>Yönetilen Diskler için Standart Anlık Görüntüleri Kullanma
%60 maliyet tasarrufu elde etmek için, üst diskin depolama türünden bağımsız olarak anlık görüntülerinizi Standart Depolama’da depolamanızı öneririz. Bu seçenek Yönetilen Diskler anlık görüntüler için varsayılan seçenektir. Azure Danışmanı, Premium Depolama'da depolanan anlık görüntüleri tanımlar ve anlık görüntünüzün Premium'dan Standart Depolama alanına geçişini önerir. [Yönetilen Disk fiyatlandırması hakkında daha fazla bilgi edinin](https://aka.ms/aa_manageddisksnapshot_learnmore)

## <a name="utilize-lifecycle-management"></a>Yaşam Döngüsü Yönetimi'ni Kullanma
Azure Danışmanı, azure blob depolama nesnesayınız, toplam boyutunuz ve işlemlerinize ilişkin bilgileri kullanarak, depolama hesaplarınızdan birinin veya daha fazlasının katman verilerine yaşam döngüsü yönetimini etkinleştirmek için en uygun olup olmadığını algılar. Uygulama uyumluluğu için verilerinizi Azure blob depolamaalanında tutarken depolama maliyetlerinizi optimize etmek için verilerinizi otomatik olarak Cool veya Archive'e katmanlamak için Yaşam Döngüsü Yönetimi kuralları oluşturmanızı ister.

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Azure Danışmanı'nda Maliyet önerilerine nasıl erişilir?

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Herhangi bir sayfadan [**Danışman'ı**](https://aka.ms/azureadvisordashboard) arayın ve seçin.

1. **Danışman** panosunda **Maliyet** sekmesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi edinmek için bkz:
* [Danışmana Giriş](advisor-overview.md)
* [Başlayın](advisor-get-started.md)
* [Danışman Performans önerileri](advisor-performance-recommendations.md)
* [Danışman Yüksek Kullanılabilirlik önerileri](advisor-high-availability-recommendations.md)
* [Danışman Güvenlik önerileri](advisor-security-recommendations.md)
* [Danışman Operasyonel Mükemmellik önerileri](advisor-operational-excellence-recommendations.md)
