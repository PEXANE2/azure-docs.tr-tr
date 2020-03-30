---
title: Bakım ve güncelleştirmeler
description: Azure'da çalışan sanal makineler için bakım ve güncelleştirmelere genel bakış.
author: shants123
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/18/2019
ms.author: shants
ms.openlocfilehash: eaf7616b3bd69828829342b4dca9247c009d3475
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250237"
---
# <a name="maintenance-for-virtual-machines-in-azure"></a>Azure’da sanal makineler için bakım

Azure, sanal makineler için ana bilgisayar altyapısının güvenilirliğini, performansını ve güvenliğini artırmak için platformunu düzenli aralıklarla güncelleştirir. Bu güncelleştirmelerin amacı, barındırma ortamındaki yazılım bileşenlerine düzeltme eki uygulamadan ağ bileşenlerini yükseltmeye veya donanımı kullanımdan kaldırmaya kadar değişiklik gösterir. 

Güncelleştirmeler barındırılan VM'leri nadiren etkiler. Güncelleştirmelerin bir etkisi olduğunda, Azure güncelleştirmeler için en az etkili yöntemi seçer:

- Güncelleştirme yeniden başlatma gerektirmiyorsa, ana bilgisayar güncelleştirilirken VM duraklatıldı veya VM zaten güncelleştirilmiş bir ana bilgisayara taşınır. 
- Bakım yeniden başlatma gerektiriyorsa, planlanan bakım size bildirilir. Azure, sizin için uygun bir zamanda bakımı kendiniz başlatabileceğiniz bir zaman penceresi de sağlar. Bakım acil olmadığı sürece kendi kendine bakım penceresi genellikle 30 gündür. Azure, planlanan platform bakımının VM'lerin yeniden başlatılmasını gerektirdiği servis taleplerini azaltmak için teknolojilere yatırım yapmaktadır. Planlı bakımı yönetme yönergeleri için Bkz. Azure [CLI,](maintenance-notifications-cli.md) [PowerShell](maintenance-notifications-powershell.md) veya [portalını](maintenance-notifications-portal.md)kullanarak planlı bakım bildirimlerini işleme.

Bu sayfa, Azure'un her iki bakım türünü nasıl gerçekleştirdiğini açıklar. Planlanmamış olaylar (kesintiler) hakkında daha fazla bilgi için windows [için VM'lerin kullanılabilirliğini](./windows/manage-availability.md) veya [Linux](./linux/manage-availability.md)için ilgili makaleyi yönet'e bakın.

Bir VM içinde, Windows veya [Linux](./linux/scheduled-events.md) [için Zamanlanmış Olaylar kullanarak](./windows/scheduled-events.md) yaklaşan bakım hakkında bildirimler alabilirsiniz.



## <a name="maintenance-that-doesnt-require-a-reboot"></a>Yeniden başlatma gerektirmeyen bakım

Çoğu platform güncelleştirmesi müşteri VM'lerini etkilemez. Etkisi olmayan bir güncelleştirme mümkün olmadığında, Azure müşteri VM'leri için en az etkili olan güncelleştirme mekanizmasını seçer. 

Sıfır olmayan etki gerektirmeyen bakım, VM'yi 10 saniyeden kısa bir süre duraklatabilir. Bazı durumlarda Azure bellek koruma bakım mekanizmaları kullanır. Bu mekanizmalar VM'yi 30 saniyeye kadar duraklatabilir ve BELLEği RAM'de korur. VM daha sonra devam eder ve saati otomatik olarak eşitlenir. 

Azure VM'lerinin yüzde 90'ından fazlası için bellek koruma bakımı çalışır. G, M, N ve H serilerinde işe yaramaz. Azure, duraklama sürelerini azaltmak için canlı geçiş teknolojilerini giderek daha fazla kullanır ve bellek koruma lı bakım mekanizmalarını geliştirir.  

Yeniden başlatma gerektirmeyen bu bakım işlemleri, aynı anda bir hata etki alanı uygulanır. Herhangi bir uyarı sağlık sinyalleri alırlarsa dururlar. 

Bu tür güncelleştirmeler bazı uygulamaları etkileyebilir. VM farklı bir ana bilgisayara canlı olarak geçirildiğinde, bazı hassas iş yükleri VM duraklamasına kadar olan birkaç dakika içinde hafif bir performans bozulması gösterebilir. VM bakımına hazırlanmak ve Azure bakımı sırasında etkiyi azaltmak için, bu tür uygulamalar için Windows veya [Linux](./linux/scheduled-events.md) [için Zamanlanmış Etkinlikler'i kullanmayı](./windows/scheduled-events.md) deneyin. 

Ayrıca, genel önizlemede yeniden başlatma gerektirmeyen bakımı yönetmenize yardımcı olabilecek bir özellik, bakım denetimi de vardır. [Azure Özel Ana Bilgisayar ları](./linux/dedicated-hosts.md) veya [yalıtılmış bir VM](../security/fundamentals/isolation-choices.md)kullanıyor olmalısınız. Bakım denetimi, platform güncelleştirmelerini atlama ve güncelleştirmeleri 35 günlük bir haddeleme süresi içinde istediğiniz zaman uygulama seçeneği sunar. Daha fazla bilgi için [Bakım Denetimi ve Azure CLI ile Denetim güncelleştirmeleri'ne](maintenance-control-cli.md)bakın.


### <a name="live-migration"></a>Canlı geçiş

Canlı geçiş, yeniden başlatma gerektirmeyen ve VM için belleği koruyan bir işlemdir. Genellikle en fazla 5 saniye süren bir duraklama veya dondurma neden olur. G, M, N ve H serileri dışında, hizmet (IaaS) VM'ler olarak tüm altyapıcanlı geçiş için uygundur. Uygun VM'ler, Azure filosuna dağıtılan IaaS VM'lerinin yüzde 90'ından fazlasını temsil eder. 

Azure platformu aşağıdaki senaryolarda canlı geçiş başlatır:
- Planlı bakım
- Donanım hatası
- Ayırma optimizasyonları

Bazı planlı bakım senaryoları canlı geçiş kullanır ve zamanlanmış olayları canlı geçiş işlemlerinin ne zaman başlayacağını önceden bilmek için kullanabilirsiniz.

Azure Machine Learning algoritmaları yaklaşan bir donanım hatasını tahmin ettiğinde veya VM ayırmalarını optimize etmek istediğinizde VM'leri taşımak için canlı geçiş de kullanılabilir. Bozulmuş donanım örneklerini algılayan tahmine dayalı modelleme hakkında daha fazla bilgi [için](https://azure.microsoft.com/blog/improving-azure-virtual-machine-resiliency-with-predictive-ml-and-live-migration/?WT.mc_id=thomasmaurer-blog-thmaure)bkz. Canlı geçiş bildirimleri, bu hizmetleri kullanıyorsanız, Monitör ve Hizmet Durumu günlüklerinde ve Zamanlanan Etkinlikler'de Azure portalında görünür.

## <a name="maintenance-that-requires-a-reboot"></a>Yeniden başlatma gerektiren bakım

VM'lerin planlı bakım için yeniden başlatılması gereken nadir durumlarda, önceden bilgilendirilirsiniz. Planlı bakım iki aşamadan sahiptir: self servis aşaması ve zamanlanmış bakım aşaması.

Genellikle dört hafta süren *self servis aşamasında,* VM'lerinizin bakımını başlayırsınız. Self servisin bir parçası olarak, her VM'yi durumunu ve son bakım isteğinizin sonucunu görmek için sorgulayabilirsiniz.

Self servis bakıma başladığınızda, VM'iniz zaten güncellenmiş bir düğüme yeniden dağıtılır. VM yeniden başlatTığı için, geçici disk kaybolur ve sanal ağ arabirimi ile ilişkili dinamik IP adresleri güncelleştirilir.

Self servis bakım sırasında bir hata ortaya çıkarsa, işlem durur, VM güncelleştirilemiyor ve self servis bakımı yeniden deneme seçeneğine siniz. 

Self servis aşaması sona erdiğinde, *zamanlanan bakım aşaması* başlar. Bu aşamada, bakım aşaması nı sorgulamaya devam edebilirsiniz, ancak bakımı kendiniz başlatamazsınız.

Yeniden başlatma gerektiren bakımı yönetme hakkında daha fazla bilgi için Azure [CLI](maintenance-notifications-cli.md), [PowerShell](maintenance-notifications-powershell.md) veya [portalını](maintenance-notifications-portal.md)kullanarak **planlı bakım bildirimlerini işleme'ye** bakın. 

### <a name="availability-considerations-during-scheduled-maintenance"></a>Zamanlanmış bakım sırasında kullanılabilirlik hususları 

Zamanlanan bakım aşamasına kadar beklemeye karar verirseniz, VM'lerinizin en yüksek kullanılabilirliğini korumak için göz önünde bulundurmanız gereken birkaç şey vardır. 

#### <a name="paired-regions"></a>Eşleştirilmiş bölgeler

Her Azure bölgesi, aynı coğrafi bölge içindeki başka bir bölgeyle eşlenir. Birlikte, bir bölge çifti olun. Zamanlanan bakım aşamasında Azure, yalnızca bir bölge çiftinin tek bir bölgesindeki VM'leri güncelleştirir. Örneğin, Kuzey Orta ABD'deki VM güncellenirken, Azure Aynı anda Güney Orta ABD'deki vm'leri güncellemez. Ancak, Kuzey Avrupa gibi diğer bölgelerde, Doğu ABD ile aynı zamanda bakım yapılabilir. Bölge çiftleri nasıl çalıştığını anlamak, VM'lerinizi bölgeler arasında daha iyi dağıtmanıza yardımcı olabilir. Daha fazla bilgi için [Azure bölge çiftleri'ne](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)bakın.

#### <a name="availability-sets-and-scale-sets"></a>Kullanılabilirlik kümeleri ve ölçek kümeleri

Azure VM'lerinde iş yükü dağıtırken, uygulamanızda yüksek kullanılabilirlik sağlamak için bir *kullanılabilirlik kümesi* içinde VM'ler oluşturabilirsiniz. Kullanılabilirlik kümelerini kullanarak, yeniden başlatma gerektiren bir kesinti veya bakım olayları sırasında en az bir VM kullanılabilir olduğundan emin olabilirsiniz.

Kullanılabilirlik kümesi içinde, tek tek VM'ler en fazla 20 güncelleştirme etki alanına yayılır. Zamanlanan bakım sırasında, herhangi bir zamanda yalnızca bir güncelleştirme etki alanı güncelleştirilir. Güncelleştirme etki alanları mutlaka sırayla güncelleştirilemez. 

Sanal makine *ölçek kümeleri,* tek bir kaynak olarak aynı VM'leri dağıtmak ve yönetmek için kullanabileceğiniz bir Azure bilgi işlem kaynağıdır. Ölçek kümesi, kullanılabilirlik kümesindeki VM'ler gibi ABD'ler arasında otomatik olarak dağıtılır. Kullanılabilirlik kümeleri gibi, ölçek kümelerini kullandığınızda, zamanlanan bakım sırasında herhangi bir zamanda yalnızca bir UD güncelleştirilir.

Yüksek kullanılabilirlik için VM'lerinizi ayarlama hakkında daha fazla bilgi için [bkz.](./windows/manage-availability.md) [Linux](./linux/manage-availability.md)

#### <a name="availability-zones"></a>Kullanılabilirlik alanları

Kullanılabilirlik bölgeleri, Azure bölgesindeki benzersiz fiziksel konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Esnekliği sağlamak için, tüm etkin bölgelerde en az üç ayrı bölge vardır. 

Kullanılabilirlik bölgesi, hata etki alanı ve güncelleştirme etki alanının birleşimidir. Bir Azure bölgesinde üç bölgede üç veya daha fazla VM oluşturursanız, VM'leriniz üç hata etki alanına ve üç güncelleştirme etki alanına etkili bir şekilde dağıtılır. Azure platformu, farklı bölgelerdeki VM'lerin aynı anda güncelleştirilemediğinden emin olmak için bu dağıtımı güncelleştirme etki alanlarında tanır.

Her altyapı güncelleştirmesi, tek bir bölge içinde bölgeye göre olur. Ancak, Bölge 1'de dağıtım devam edebilir ve aynı anda Bölge 2'de farklı dağıtım lar devam edebilir. Dağıtımların tümü seri hale getirilemedi. Ancak, tek bir dağıtım riski azaltmak için aynı anda yalnızca bir bölge dışarı rulo.

## <a name="next-steps"></a>Sonraki adımlar 

Planlanan bakımı yönetmek için [Azure CLI,](maintenance-notifications-cli.md) [Azure PowerShell](maintenance-notifications-powershell.md) veya [portalı](maintenance-notifications-portal.md) kullanabilirsiniz. 