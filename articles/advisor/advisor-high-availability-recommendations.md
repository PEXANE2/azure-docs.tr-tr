---
title: Azure Danışmanı ile uygulamanızın kullanılabilirliğini artırın
description: Azure dağıtımlarınızın yüksek kullanılabilirliğini artırmak için Azure Danışmanı'nı kullanın.
ms.topic: article
ms.date: 01/29/2019
ms.openlocfilehash: 997681ed62fa9985e3122ece22565dbae0e65b53
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75443104"
---
# <a name="improve-availability-of-your-application-with-azure-advisor"></a>Azure Danışmanı ile uygulamanızın kullanılabilirliğini artırın

Azure Danışmanı, iş açısından kritik uygulamalarınızın sürekliliğini sağlamanıza ve geliştirmenize yardımcı olur. Danışman panosunun **Yüksek Kullanılabilirlik** sekmesinden Danışman tarafından yüksek kullanılabilirlik önerileri alabilirsiniz.

## <a name="ensure-virtual-machine-fault-tolerance"></a>Sanal makine hata toleransı sağlayın

Uygulamanıza yedeklilik sağlamak için bir kullanılabilirlik kümesinde iki veya daha fazla sanal makinenin gruplandırılması önerilir. Danışman, kullanılabilirlik kümesinin parçası olmayan sanal makineleri tanımlar ve bunları kullanılabilirlik kümesine taşımanızı önerir. Bu yapılandırma, planlı veya planlanmamış bir bakım olayı sırasında en az bir sanal makinenin kullanılabilir olmasını ve Azure sanal makine SLA ile tanışmasını sağlar. Sanal makine için bir kullanılabilirlik kümesi oluşturmayı veya sanal makineyi varolan kullanılabilirlik kümesine eklemeyi seçebilirsiniz.

> [!NOTE]
> Bir kullanılabilirlik kümesi oluşturmayı seçerseniz, en az bir sanal makine daha eklemeniz gerekir. Bir kesinti sırasında en az bir makinenin kullanılabilir olduğundan emin olmak için iki veya daha fazla sanal makineyi kullanılabilirlik kümesinde gruplandırmanızı öneririz.

## <a name="ensure-availability-set-fault-tolerance"></a>Kullanılabilirlik ayarlı arıza toleransı sağlayın

Uygulamanıza yedeklilik sağlamak için bir kullanılabilirlik kümesinde iki veya daha fazla sanal makinenin gruplandırılması önerilir. Danışman, tek bir sanal makine içeren kullanılabilirlik kümelerini tanımlar ve ona bir veya daha fazla sanal makine eklemenizi önerir.Bu yapılandırma, planlı veya planlanmamış bir bakım olayı sırasında en az bir sanal makinenin kullanılabilir olmasını ve Azure sanal makine SLA ile tanışmasını sağlar.Sanal bir makine oluşturmayı veya kullanılabilirlik kümesine varolan bir sanal makine eklemeyi seçebilirsiniz.  

## <a name="use-managed-disks-to-improve-data-reliability"></a>Veri güvenilirliğini geliştirmek için Yönetilen Diskler kullanın

Depolama hesaplarını veya depolama ölçek birimlerini paylaşan disklerle doluluk kümesinde bulunan sanal makineler, kesintiler sırasında tek depolama ölçeği birim hatalarına karşı esnek değildir. Danışman bu kullanılabilirlik kümelerini tanımlar ve Azure Yönetilen Disklere geçiş yapmanızı önerir. Bu, kullanılabilirlik kümesindeki farklı sanal makinelerin disklerinin tek bir hata noktasını önlemek için yeterince yalıtılmış olmasını sağlar. 

## <a name="ensure-application-gateway-fault-tolerance"></a>Uygulama ağ geçidi hata toleransı sağlayın

Bu öneri, uygulama ağ geçitleri tarafından desteklenen görev açısından kritik uygulamaların iş sürekliliğini sağlar. Danışman, hata toleransı için yapılandırılmamış uygulama ağ geçidi örneklerini tanımlar ve yapabileceğiniz düzeltme eylemlerini önerir. Danışman orta veya büyük tek örnekli uygulama ağ geçitlerini tanımlar ve en az bir örnek daha eklemeyi önerir. Ayrıca, tek veya çok örnekli küçük uygulama ağ geçitlerini tanımlar ve orta veya büyük SUS'lara geçiş yapmanızı önerir. Danışman, uygulama ağ geçidi örneklerinizin bu kaynaklar için geçerli SLA gereksinimlerini karşılayacak şekilde yapılandırıldığından emin olmak için bu eylemleri önerir.

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>Sanal makine verilerinizi yanlışlıkla silmeişlemine karşı koruyun

Sanal makine yedeklemesinin ayarlanması, iş açısından kritik verilerinizin kullanılabilirliğini sağlar ve yanlışlıkla silinme veya bozulmaya karşı koruma sağlar. Danışman, yedeklemenin etkinleştirilen olmadığı sanal makineleri tanımlar ve yedeklemeyi etkinleştirmenizi önerir. 

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>İhtiyacınız olduğunda Azure bulut uzmanlarına erişebildiğinizden emin olun

İş açısından kritik bir iş yükü çalıştırırken, gerektiğinde teknik desteğe erişebilmek önemlidir. Danışman, destek planında teknik destek olmayan olası iş açısından kritik abonelikleri tanımlar ve teknik destek içeren bir seçenek için yükseltme önerir.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-issues-affect-you"></a>Azure sorunları sizi etkilediğinde haberdar olmak için Azure Hizmet Durumu uyarıları oluşturun

Azure hizmet sorunları sizi etkilediğinde bildirilecek Azure Hizmet Durumu uyarıları ayarlamanızı öneririz. [Azure Hizmet Durumu,](https://azure.microsoft.com/features/service-health/) bir Azure hizmet sorunundan etkilendiğinde kişiselleştirilmiş rehberlik ve destek sağlayan ücretsiz bir hizmettir. Danışman, uyarıyapılmayı olmayan abonelikleri tanımlar ve bir tane oluşturmayı önerir.

## <a name="configure-traffic-manager-endpoints-for-resiliency"></a>Esneklik için Trafik Yöneticisi uç noktalarını yapılandırın

Verilen herhangi bir bitiş noktası başarısız olursa, birden fazla uç nokta deneyimi daha yüksek kullanılabilirliği olan Trafik Yöneticisi profilleri. Uç noktaların farklı bölgelere yerleştirilmesi hizmet güvenilirliğini daha da artırır. Danışman, yalnızca bir bitiş noktasının olduğu Traffic Manger profillerini tanımlar ve başka bir bölgeye en az bir bitiş noktası daha eklemenizi önerir.

Yakınlık yönlendirmesi için yapılandırılan Trafik Yöneticisi profilindeki tüm uç noktalar aynı bölgedeyse, diğer bölgelerdeki kullanıcılar bağlantı gecikmeleri yaşayabilir. Bir bitiş noktasının başka bir bölgeye eklenmesi veya taşınması genel performansı artırır ve bir bölgedeki tüm uç noktalar başarısız olursa daha iyi kullanılabilirlik sağlar. Danışman, tüm uç noktaların aynı bölgede olduğu yakınlık yönlendirmesi için yapılandırılan Trafik Yöneticisi profillerini tanımlar. Bitiş noktasının başka bir Azure bölgesine eklenmesini veya taşınmasını önerir.

Trafik Yöneticisi profili coğrafi yönlendirme için yapılandırılırsa, trafik tanımlanan bölgelere göre bitiş noktalarına yönlendirilir. Bir bölge başarısız olursa, önceden tanımlanmış bir hata yoktur. Bölgesel Gruplandırmanın "Tümü (Dünya)" olarak yapılandırıldığı bir bitiş noktasına sahip olmak, trafiğin düşmesini önler ve hizmet kullanılabilirliğini artırır. Danışman, Coğrafi yönlendirme için yapılandırılan Trafik Yöneticisi profillerini, Bölgesel Gruplandırmayı "Tümü (Dünya)" olarak yapılandıracak şekilde yapılandırılmamış olduğu yerlerde tanımlar. Bir bitiş noktası "Tüm (Dünya) yapmak için yapılandırmayı değiştirmenizi önerir.

## <a name="use-soft-delete-on-your-azure-storage-account-to-save-and-recover-data-after-accidental-overwrite-or-deletion"></a>Yanlışlıkla üzerine yazdıktan veya silindikten sonra verileri kaydetmek ve kurtarmak için Azure Depolama Hesabınızda yumuşak silme'yi kullanın

Silinen lekelerin kalıcı olarak silinmek yerine yumuşak silinmiş bir duruma geçmesi için depolama hesabınızda [yumuşak silmeyi](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) etkinleştirin. Verilerin üzerine yazıldığında, üzerine yazılan verilerin durumunu kaydetmek için geçici silinen bir anlık görüntü oluşturulur. Yumuşak silme kullanmak, yanlışlıkla silme veya üzerine yazma varsa kurtarmanızı sağlar. Danışman, yumuşak silme özelliği olmayan Azure Depolama hesaplarını tanımlar ve etkinleştirmenizi önerir.

## <a name="configure-your-vpn-gateway-to-active-active-for-connection-resiliency"></a>Vpn ağ geçidinizi bağlantı esnekliği için etkin-etkin olarak yapılandırın

Etkin-etkin yapılandırmada, VPN ağ geçidinin her iki örneği de şirket içi VPN aygıtınıza S2S VPN tünelleri kurar. Planlı bir bakım olayı veya planlanmamış olay bir ağ geçidi örneğine geldiğinde, trafik otomatik olarak diğer etkin IPsec tüneline geçer. Azure Danışmanı, etkin olarak yapılandırılmamış VPN ağ geçitlerini tanımlar ve yüksek kullanılabilirlik için yapılandırmanızı önerir.

## <a name="use-production-vpn-gateways-to-run-your-production-workloads"></a>Üretim iş yüklerinizi çalıştırmak için üretim VPN ağ geçitlerini kullanın

Azure Danışmanı, Temel SKU olan VPN ağ geçitlerini denetler ve bunun yerine bir üretim SKU'su kullanmanızı önerir. Temel SKU geliştirme ve test amacıyla tasarlanmıştır. Üretim SUK'ları daha yüksek sayıda tünel, BGP desteği, etkin-etkin yapılandırma seçenekleri, özel Ipsec/IKE ilkesi ve daha yüksek kararlılık ve kullanılabilirlik sunar.

## <a name="repair-invalid-log-alert-rules"></a>Geçersiz günlük uyarı kurallarını onar

Azure Danışmanı, durum bölümünde belirtilen geçersiz sorguları olan uyarı kurallarını algılar. Günlük uyarı kuralları Azure İzleyici'de oluşturulur ve belirtilen zaman aralıklarında analiz sorguları çalıştırmak için kullanılır. Sorgu sonuçları, uyarıyı tetiklemenin gerekip gerekmediğini belirler. Başvurulan kaynaklarda, tablolarla veya komutlarda zamanla değişiklik oldukça analiz sorguları geçersiz hale gelir. Danışman, otomatik devre dışı kalmasını önlemek ve Azure'daki kaynaklarınızın kapsama alanını izlemeyi sağlamak için uyarı kuralındaki sorguyu düzeltmenizi önerir. [Sorun giderme uyarısı kuralları hakkında daha fazla bilgi edinin](https://aka.ms/aa_logalerts_queryrepair)

## <a name="configure-consistent-indexing-mode-on-your-cosmos-db-collection"></a>Cosmos DB koleksiyonunuzda tutarlı dizin oluşturma modunu yapılandırma

Lazy dizin oluşturma moduyla yapılandırılan Azure Cosmos DB kapsayıcıları sorgu sonuçlarının tazeliğini etkileyebilir. Danışman bu şekilde yapılandırılan kapsayıcıları algılar ve Tutarlı moduna geçmeyi önerir. [Cosmos DB'de dizine iştinleme hakkında daha fazla bilgi edinin](https://aka.ms/cosmosdb/how-to-manage-indexing-policy)

## <a name="configure-your-azure-cosmos-db-containers-with-a-partition-key"></a>Azure Cosmos DB kapsayıcılarınızı bölüm anahtarı ile yapılandırın

Azure Danışmanı, sağlanan depolama kotasına yaklaşan Azure Cosmos DB bölümlenmemiş koleksiyonları tanımlar. Bu koleksiyonları, hizmet tarafından otomatik olarak ölçeklendirilebilmeleri için bölüm anahtarı tanımıyla yeni koleksiyonlara geçirmenizi önerir. [Bölüm anahtarı seçme hakkında daha fazla bilgi edinin](https://aka.ms/cosmosdb/choose-partitionkey)

## <a name="upgrade-your-azure-cosmos-db-net-sdk-to-the-latest-version-from-nuget"></a>Azure Cosmos DB .NET SDK'nızı Nuget'ten en son sürüme yükseltin

Azure Danışmanı, .NET SDK'nın eski sürümlerini kullanan Azure Cosmos DB hesaplarını tanımlar ve en son düzeltmeler, performans iyileştirmeleri ve yeni özellik özellikleri için Nuget'ten en son sürüme yükseltmenizi önerir. [Cosmos DB .NET SDK hakkında daha fazla bilgi edinin](https://aka.ms/cosmosdb/sql-api-sdk-dotnet)

## <a name="upgrade-your-azure-cosmos-db-java-sdk-to-the-latest-version-from-maven"></a>Azure Cosmos DB Java SDK'nızı Maven'dan en son sürüme yükseltin

Azure Danışmanı, Java SDK'nın eski sürümlerini kullanan Azure Cosmos DB hesaplarını tanımlar ve en son düzeltmeler, performans iyileştirmeleri ve yeni özellik özellikleri için Maven'den en son sürüme yükseltmenizi önerir. [Cosmos DB Java SDK hakkında daha fazla bilgi edinin](https://aka.ms/cosmosdb/sql-api-sdk-dotnet)

## <a name="upgrade-your-azure-cosmos-db-spark-connector-to-the-latest-version-from-maven"></a>Azure Cosmos DB Spark Bağlayıcınızı Maven'dan en son sürüme yükseltin

Azure Danışmanı, Cosmos DB Spark konektörünün eski sürümlerini kullanan Azure Cosmos DB hesaplarını tanımlar ve en son düzeltmeler, performans iyileştirmeleri ve yeni özellik özellikleri için Maven'den en son sürüme yükseltmenizi önerir. [Cosmos DB Spark konektörü hakkında daha fazla bilgi edinin](https://aka.ms/cosmosdb/spark-connector)

## <a name="enable-virtual-machine-replication"></a>Sanal makine çoğaltmasını etkinleştirme
Başka bir bölgeye çoğaltma özelliği olmayan sanal makineler bölgesel kesintilere dayanıklı değildir. Sanal makinelerin çoğaltılamaz hale türü, Azure bölgesinde kesinti sırasında ki olumsuz iş etkilerini azaltır. Danışman, çoğaltma etkin leştirilmiş olmayan VM'leri algılar ve bir kesinti durumunda sanal makinelerinizi uzak bir Azure bölgesinde hızla getirebilmeniz için çoğaltmayı etkinleştirmenizi önerir. [Sanal makine çoğaltma hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart)

## <a name="how-to-access-high-availability-recommendations-in-advisor"></a>Danışman'da Yüksek Kullanılabilirlik önerilerine nasıl erişilir?

1. [Azure portalında](https://portal.azure.com)oturum açın ve [ardından Danışman'ı](https://aka.ms/azureadvisordashboard)açın.

2.  Danışman panosunda Yüksek **Kullanılabilirlik** sekmesini tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi için bkz:
* [Azure Danışmanına Giriş](advisor-overview.md)
* [Danışman’ı kullanmaya başlama](advisor-get-started.md)
* [Danışman Maliyet önerileri](advisor-cost-recommendations.md)
* [Danışman Performans önerileri](advisor-performance-recommendations.md)
* [Danışman Güvenlik önerileri](advisor-security-recommendations.md)
* [Danışman Operasyonel Mükemmellik önerileri](advisor-operational-excellence-recommendations.md)
