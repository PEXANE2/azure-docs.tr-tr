---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/10/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 2daaf9bbdf90029f0aad4333ab94e2d1d1d3d7ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128620"
---
## <a name="limitations"></a>Sınırlamalar

- Sanal makine ölçek kümeleri şu anda özel ana bilgisayarlarda desteklenmez.

## <a name="benefits"></a>Avantajlar 

Ana bilgisayarın tamamının rezerve edilebilenler aşağıdaki avantajları sağlar:

-   Fiziksel sunucu düzeyinde donanım yalıtımı. Ana bilgisayarlarınıza başka hiçbir VM yerleştirilmez. Özel ana bilgisayarlar aynı veri merkezlerinde dağıtılır ve aynı ağ ve altta yatan depolama altyapısını diğer, yalıtılmış olmayan ana bilgisayarlarla paylaşır.
-   Azure platformu tarafından başlatılan bakım etkinlikleri üzerinde denetim. Bakım olaylarının çoğunun sanal makineleriniz üzerinde çok az veya hiç etkisi olmamasına neden olsa da, duraklamanın her saniyesinin bir etkisi olabileceği bazı hassas iş yükleri vardır. Özel ana bilgisayarlarla, hizmetinize olan etkisini azaltmak için bir bakım penceresini tercih edebilirsiniz.
-   Azure karma avantajıyla, Windows ve SQL için kendi lisanslarınızı Azure'a getirebilirsiniz. Hibrit avantajları kullanmak size ek avantajlar sağlar. Daha fazla bilgi için Azure [Karma Avantajı'na](https://azure.microsoft.com/pricing/hybrid-benefit/)bakın.


## <a name="groups-hosts-and-vms"></a>Gruplar, ana bilgisayarlar ve VM'ler  

![Özel ana bilgisayarlar için yeni kaynakların görünümü.](./media/virtual-machines-common-dedicated-hosts/dedicated-hosts2.png)

**Ana bilgisayar grubu,** özel ana bilgisayarkoleksiyonunu temsil eden bir kaynaktır. Bir bölgede ve kullanılabilirlik bölgesinde bir ana bilgisayar grubu oluşturur ve ona ana bilgisayarlar eklersiniz.

**Ana bilgisayar,** Azure veri merkezindeki fiziksel bir sunucuya eşlenen bir kaynaktır. Ana bilgisayar oluşturulduğunda fiziksel sunucu ayrılır. Ana bilgisayar grubu içinde bir ana bilgisayar oluşturulur. Ana bilgisayarda hangi VM boyutlarının oluşturulabileceğini açıklayan bir SKU vardır. Her ana bilgisayar, aynı boyuttaki seriden oldukları sürece farklı boyutlarda birden fazla VM barındırabilir.

Azure'da bir VM oluştururken, VM'iniz için hangi özel ana bilgisayarı kullanacağınızı seçebilirsiniz. Ev sahiplerinize hangi VM'lerin yerleştirildiği konusunda tam kontrole sahipsiniz.


## <a name="high-availability-considerations"></a>Yüksek Kullanılabilirlik hususları 

Yüksek kullanılabilirlik için, birden çok VM dağıtmanız gerekir, birden çok ana bilgisayara yayılmış (en az 2). Azure Özel Ana Bilgisayar ları ile, hata yalıtım sınırlarınızı şekillendirmek için altyapınızı sağlamak için çeşitli seçenekleriniz vardır.

### <a name="use-availability-zones-for-fault-isolation"></a>Hata yalıtımı için Kullanılabilirlik Bölgelerini Kullanma

Kullanılabilirlik bölgeleri, Azure bölgesindeki benzersiz fiziksel konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Bir ana bilgisayar grubu tek bir kullanılabilirlik bölgesinde oluşturulur. Oluşturulduktan sonra, tüm ana bilgisayarlar bu bölgeye yerleştirilir. Bölgeler arasında yüksek kullanılabilirlik sağlamak için birden çok ana bilgisayar grubu (bölge başına bir tane) oluşturmanız ve ev sahiplerinizi buna göre yaymanız gerekir.

Bir kullanılabilirlik bölgesine bir ana bilgisayar grubu atarsanız, bu ana bilgisayarda oluşturulan tüm VM'lerin aynı bölgede oluşturulması gerekir.

### <a name="use-fault-domains-for-fault-isolation"></a>Hata yalıtımı için Hata Etki Alanlarını Kullanma

Ana bilgisayar belirli bir hata etki alanında oluşturulabilir. Bir ölçek kümesindeki VM veya kullanılabilirlik kümesinde olduğu gibi, farklı hata etki alanlarındaki ana bilgisayarlar da veri merkezindeki farklı fiziksel raflara yerleştirilir. Bir ana bilgisayar grubu oluşturduğunuzda, hata etki alanı sayısını belirtmeniz gerekir. Ana bilgisayar grubu içinde ana bilgisayar oluştururken, her ana bilgisayar için hata etki alanı atarsınız. VM'ler herhangi bir hata etki alanı ataması gerektirmez.

Hata etki alanları, harmanlama ile aynı değildir. İki ana bilgisayar için aynı hata etki alanına sahip olmak, birbirlerine yakın oldukları anlamına gelmez.

Hata etki alanları ana bilgisayar grubuna kapsamlıdır. İki ana bilgisayar grubu arasındaki yakınlık la ilgili herhangi bir varsayımda bulunmamalısınız (farklı kullanılabilirlik bölgelerinde olmadıkları sürece).

Farklı hata etki alanları olan ana bilgisayarlara dağıtılan VM'ler, hata yalıtımı korumasını artırmak için altta yatan yönetilen disk hizmetleri birden çok depolama puluna sahip olacaktır.

### <a name="using-availability-zones-and-fault-domains"></a>Kullanılabilirlik Bölgelerini ve Hata Etki Alanlarını Kullanma

Daha fazla hata yalıtımı elde etmek için her iki özelliği de birlikte kullanabilirsiniz. Bu durumda, her ana bilgisayar grubu için kullanılabilirlik bölgesi ni ve hata etki alanı sayısını belirtecek, gruptaki her ana bilgisayarınız için bir hata etki alanı atayacak ve Her VM'lerinize bir kullanılabilirlik bölgesi atayacaksınız

[Burada](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md) bulunan Kaynak Yöneticisi örnek şablonu, bir bölgede maksimum esneklik için ana bilgisayarları yaymak için bölgeleri ve fay etki alanlarını kullanır.

## <a name="maintenance-control"></a>Bakım denetimi

Sanal makinelerinizi destekleyen altyapı bazen güvenilirliği, performansı, güvenliği artırmak ve yeni özellikleri başlatmak için güncellenebilir. Azure platformu mümkün olduğunda platform bakımının etkisini en aza indirmeye çalışır, ancak *bakıma duyarlı* iş yüklerine sahip müşteriler, VM'nin bakım için dondurulması veya kesilmesi gereken birkaç saniyeyi bile kaldıramaz.

**Bakım Denetimi,** müşterilere özel ana bilgisayarlarında zamanlanan düzenli platform güncelleştirmelerini atlama ve ardından 35 günlük bir haddeleme penceresi içinde istedikleri anda uygulama seçeneği sunar.

> [!NOTE]
>  Bakım denetimi şu anda genel önizlemede. Daha fazla bilgi için ** [CLI](https://docs.microsoft.com/azure/virtual-machines/maintenance-control-cli?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json) veya [PowerShell](https://docs.microsoft.com/azure/virtual-machines/maintenance-control-powershell?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json)kullanarak Bakım Denetimi ile Denetim güncelleştirmeleri'ne**bakın.

## <a name="capacity-considerations"></a>Kapasiteye ilişkin önemli noktalar

Özel bir ana bilgisayar sağlandıktan sonra, Azure bu bilgisayarı fiziksel sunucuya atar. Bu, VM'nizi sağlamanız gerektiğinde kapasitenin kullanılabilirliğini garanti eder. Azure, ana bilgisayarınız için fiziksel bir sunucu seçmek için bölgedeki (veya bölgedeki) tüm kapasiteyi kullanır. Bu aynı zamanda, müşterilerin kümede yer kalma endişesi olmadan özel ana bilgisayar ayak izlerini büyütmeyi bekleyebileceği anlamına gelir.

## <a name="quotas"></a>Kotalar

Bölge başına ayrılmış ana bilgisayarlar için 3000 vCPUs varsayılan kota sınırı vardır. Ancak, dağıtabileceğiniz ana bilgisayar sayısı, ana bilgisayar için kullanılan VM boyutu ailesi için kullanılan kotayla da sınırlıdır. Örneğin, bir **Pay-as-you-go** aboneliği, Doğu ABD bölgesinde Dsv3 boyutu serisi için yalnızca 10 vCPUs kotası kullanılabilir olabilir. Bu durumda, özel bir ana bilgisayar dağıtmak için önce en az 64 vCPUs için bir kota artışı talep etmek gerekir. Gerekirse istekte bulunmak için sağ üst köşedeki **İstek artır** düğmesini seçin.

![Portaldaki kullanım ve kotalar sayfasının ekran görüntüsü](./media/virtual-machines-common-dedicated-hosts/quotas.png)

Daha fazla bilgi için [Sanal makine vCPU kotalarına](/azure/virtual-machines/windows/quotas)bakın.

Ücretsiz deneme sürümü ve MSDN aboneliklerinin Azure Özel Ana Bilgisayarlar için kotası yoktur.

## <a name="pricing"></a>Fiyatlandırma

Kullanıcılar, kaç VM dağıtıldıkolursa olsun, ilgili ana bilgisayar başına ücretlendirilir. Aylık ekstrenizde, faturalandırılabilir yeni bir kaynak türü göreceksiniz. Özel bir ana bilgisayardaki VM'ler ekstrenizde gösterilmeye devam edecektir, ancak 0 fiyat taşır.

Ana bilgisayar fiyatı VM ailesine, türüne (donanım boyutuna) ve bölgeye göre ayarlanır. Ana bilgisayar fiyatı, ana bilgisayarda desteklenen en büyük VM boyutuna göredir.

Yazılım lisanslama, depolama ve ağ kullanımı ana bilgisayar ve VM'lerden ayrı olarak faturalandırılır. Faturalandırılabilir maddelerde değişiklik yoktur.

Daha fazla bilgi için Azure [Özel Ana Bilgisayar fiyatlandırması'na](https://aka.ms/ADHPricing)bakın.

[Ayrıca, Ayrılmış](../articles/virtual-machines/prepay-dedicated-hosts-reserved-instances.md)Azure Özel Ana Bilgisayar Ları Örneği ile maliyetlerden de tasarruf edebilirsiniz.
 
## <a name="sizes-and-hardware-generations"></a>Boyutlar ve donanım nesilleri

Bir SKU bir ana bilgisayar için tanımlanır ve VM boyutu serisini ve türünü temsil eder. Aynı boyuttaki seride oldukları sürece, farklı boyutlardaki birden çok VM'yi tek bir ana bilgisayar içinde karıştırabilirsiniz. 

*Türü* donanım üretimidir. Aynı VM serisi için farklı donanım türleri farklı CPU satıcılarından olacaktır ve farklı CPU nesilleri ve çekirdek sayısı na sahip olacaktır. 

Boyutlar ve donanım türleri bölgeye göre değişir. Daha fazla bilgi edinmek için ana bilgisayar [fiyatlandırma sayfasına](https://aka.ms/ADHPricing) bakın.


## <a name="host-life-cycle"></a>Ev sahibi yaşam döngüsü


Azure, ev sahiplerinizin sistem durumu durumunu izler ve yönetir. Ana bilgisayarınızı sorguladığınızda aşağıdaki durumlar döndürülür:

| Sistem Durumu   | Açıklama       |
|----------|----------------|
| Ana Bilgisayar Kullanılabilir     | Ev sahibinizle ilgili bilinen bir sorun yok.   |
| Host Soruşturma Altında  | Ev sahibiyle bazı sorunlarımız var. Bu, Azure'un tanımlanan sorunun kapsamını ve kök nedenini tanımlamaya çalışması için gereken bir geçiş durumudur. Ana bilgisayarda çalışan sanal makineler etkilenebilir. |
| Ev Sahibi Bekleyen Deallocate   | Azure, ana bilgisayarını sağlıklı bir duruma geri döndüremez ve sanal makinelerinizi bu ana bilgisayardan yeniden dağıtmanızı isteyemez. Etkinleştirilirse, `autoReplaceOnFailure` sanal makineleriniz sağlıklı donanıma *iyileşir.* Aksi takdirde, sanal makineniz başarısız olmak üzere olan bir ana bilgisayarda çalışıyor olabilir.|
| Ana bilgisayar tahsis edildi  | Tüm sanal makineler ana bilgisayardan kaldırıldı. Donanım döndürme dışında çıkarıldığından beri artık bu ana bilgisayar için ücretlendirilmiyordunuz.   |

