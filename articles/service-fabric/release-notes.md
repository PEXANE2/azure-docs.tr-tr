---
title: Azure Service Fabric yayınları
description: Azure Service Fabric için sürüm notları. Service Fabric 'teki en son özellikler ve iyileştirmeler hakkında bilgi içerir.
ms.date: 06/10/2019
ms.topic: conceptual
hide_comments: true
hideEdit: true
ms.openlocfilehash: 28870a197af07e964a50a06ffeef08f3b71451f4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82891718"
---
# <a name="service-fabric-releases"></a>Service Fabric yayınları

| <a href="https://github.com/Azure/Service-Fabric-Troubleshooting-Guides" target="blank">Sorun giderme kılavuzu</a>  
|  <a href="https://github.com/Azure/service-fabric-issues" target="blank">Sorun izleme</a>  
|  <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-support" target="blank">Destek seçenekleri</a>  
|  <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-versions" target="blank">Desteklenen sürümler</a>  
|  <a href="https://azure.microsoft.com/resources/samples/?service=service-fabric&sort=0" target="blank">Kod örnekleri</a>

Bu makalede, Service Fabric çalışma zamanına ve SDK 'lara yönelik en son yayınlar ve güncelleştirmeler hakkında daha fazla bilgi sağlanmaktadır.

## <a name="whats-new-in-service-fabric"></a>Service Fabric yenilikleri

### <a name="service-fabric-71"></a>Service Fabric 7,1
Geçerli COVı-19 çapraz olduğu ve müşterilerimizin karşılaştığı zorluk göz önünde bulundurarak 7,1 kullanılabilir hale getirdik, ancak Otomatik yükseltmeleri alacak şekilde kümeleri otomatik olarak yükseltmeyecektir. Müşterilerin, bu kullanıcılara en uygun olan yükseltmeleri uygulayabilmesi için, beklenmeyen kesintilere engel olmak üzere Otomatik yükseltmeleri durakladık.

[Azure Portal](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-version-azure#upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal) aracılığıyla veya bir [Azure Resource Manager dağıtımı](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-version-azure#set-the-upgrade-mode-using-a-resource-manager-template)aracılığıyla 7,1 ' e güncelleştirebilirsiniz.

Otomatik yükseltmelere sahip Service Fabric kümeleri, standart dağıtım yordamını sürdürtikten sonra otomatik olarak 7,1 güncelleştirmesini almaya başlayacaktır. Standart dağıtım [Service Fabric Tech Community sitesinde](https://techcommunity.microsoft.com/t5/azure-service-fabric/bg-p/Service-Fabric)başlamadan önce başka bir duyuru sağlayacağız.
Ayrıca, 7,1 6,5 ' den başlayan önemli yayınlar için destek tarihinin sonunda güncelleştirme yayımladık [.](https://docs.microsoft.com/azure/service-fabric/service-fabric-versions#supported-versions) 

## <a name="what-is-new-in-service-fabric-71"></a>Service Fabric 7,1 ' deki yenilikler nelerdir?
Service Fabric sonraki sürümünü duyurmaktan mutluluk duyuyoruz. Bu sürüm, temel özellikler ve geliştirmeler ile yüklenir. Önemli özelliklerden bazıları aşağıda vurgulanır:
## <a name="key-announcements"></a>Anahtar Duyurular
- **General Availability** [ **Service Fabric uygulamalar Için Service Fabric yönetilen kimliklerin** genel kullanılabilirliği](https://docs.microsoft.com/azure/service-fabric/concepts-managed-identity)
- [**Ubuntu 18,04 desteği**](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-create-vnet-and-linux-cluster)
 - [**Önizleme: sanal makine ölçek kümesi kısa ömürlü işletim sistemi disk desteği**](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-azure-deployment-preparation#use-ephemeral-os-disks-for-virtual-machine-scale-sets)* *: kısa ömürlü işletim sistemi diskleri yerel sanal makinede oluşturulur ve uzak Azure depolama 'ya kaydedilmez. Geleneksel kalıcı işletim sistemi disklerine kıyasla, kısa ömürlü işletim sistemi disklerine kıyasla tüm Service Fabric düğüm türleri (birincil ve Ikincil) için önerilir:
      -  İşletim sistemi diskine okuma/yazma gecikmesini azaltma
      -  Daha hızlı sıfırlama/yeniden görüntü düğüm yönetimi işlemlerini etkinleştirme
      -  Genel maliyetleri düşürün (diskler ücretsizdir ve ek depolama maliyeti yoktur)
- [**Konu ortak adına göre Service Fabric uygulamalara yönelik hizmet uç noktası sertifikalarının**](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-manifest-resources)bildirimi desteği.
- [**Kapsayıcılı hizmetler Için sistem durumu araştırmaları desteği**](https://docs.microsoft.com/azure/service-fabric/probes-codepackage): Kapsayıcılı uygulamalar için Lizleştirme araştırma mekanizması desteği. Lizur araştırması Kapsayıcılı uygulamanın tam olarak duyurulmasına ve zamanında yanıt vermediklerinde yeniden başlatmaya neden olur. 
- [Kapsayıcılar](https://review.docs.microsoft.com/azure/service-fabric/service-fabric-containers-overview) ve [Konuk yürütülebilir](https://review.docs.microsoft.com/azure/service-fabric/service-fabric-guest-executables-introduction) uygulamalar Için [**Başlatıcı kod paketleri desteği**](https://docs.microsoft.com/azure/service-fabric/initializer-codepackages) . Bu, hizmet paketi başlatmayı gerçekleştirmek için kod paketlerinin (ör. kapsayıcılar) belirtilen sırada yürütülmesini sağlar.
- **Fabricgözlemci ve ClusterObserver** , bir SF kümesinin farklı yönlerine Ilişkin Service Fabric telemetri yakalayan durum bilgisiz uygulamalardır. Bu uygulamalar, ApplicationInsights, EventSource ve LogAnalytics için uygulanan destek ile zengin telemetri yakalamak üzere Windows üretim kümelerine dağıtım için hazırlardır.
    - [**Fabricgözlemci (FO) 2,0**](https://github.com/microsoft/service-fabric-observer)-tüm düğümlerde çalışır, sistem durumu olayları oluşturur, Kullanıcı tarafından yapılandırılan kaynak kullanım eşiklerine ulaşıldığında Telemetriyi yayar. Bu sürüm, izleme, veri yönetimi, sağlık olayı ayrıntıları, yapılandırılmış telemetri genelinde çeşitli geliştirmeler içerir.
     - [**Clustergözlemci (Co) 1,1**](https://github.com/microsoft/service-fabric-observer/tree/master/ClusterObserver) -tek bir düğümde çalışır, küme düzeyi durum telemetrisi yakalar. Bu sürümde, Kümegözlemci Ayrıca düğüm durumunu izler ve düğüm devre dışı bırakıldığında ve Kullanıcı tarafından belirtilen süre boyunca devre dışı bırakıldığında Telemetriyi yayar.

### <a name="improve-application-life-cycle-experience"></a>Uygulama yaşam döngüsü deneyimini geliştirme

- **[Önizleme: istek boşaltma](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-advanced#avoid-connection-drops-during-planned-downtime-of-stateless-services)**: hizmet yükseltmeleri veya düğüm devre dışı bırakma gibi planlı hizmet bakımı sırasında hizmetlerin bağlantıları düzgün bir şekilde boşaltmasını sağlamak istersiniz. Bu özellik, hizmet yapılandırmasında bir örnek kapatma gecikme süresi ekler. Planlanan işlemler sırasında, SF hizmetin adresini bulma işleminden kaldırır ve ardından hizmeti kapatmadan önce bu süreyi bekler.
- **[Otomatik alt küme algılama ve dengeleme](https://docs.microsoft.com/azure/service-fabric/cluster-resource-manager-subclustering )**: farklı yerleştirme kısıtlamalarına sahip hizmetlerde ortak bir [yük ölçümü](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-metrics)olduğunda, alt kümeleme gerçekleşir. Farklı düğüm kümelerindeki yükün önemli ölçüde farklı olduğu durumlarda Service Fabric kümesi, yerleştirme kısıtlamaları nedeniyle mümkün olan en iyi dengeyi sahip olsa bile kümenin imse olduğunu düşündü Kaynak Yöneticisi. Sonuç olarak, büyük olasılıkla gereksiz hizmet hareketlerine neden olan ("ımbilanço" önemli ölçüde iyileşebildiğinden) kümeyi yeniden dengelemeye çalışır. Bu sürümden itibaren, Küme Kaynak Yöneticisi artık bu yapılandırma türlerini otomatik olarak algılamaya çalışır ve ne zaman, önemli bir geliştirme yapamayacağı için bu tür yapılandırmayı otomatik olarak algılamaya ve ne zaman tek tek bırakması gerektiğine çalışacaktır.  
- [**İkincil çoğaltmalar Için farklı taşıma maliyeti**](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-movement-cost): ikincil çoğaltmalar için ayrı bir taşıma maliyetinin kullanılması gerekip gerekmediğini tanımlamak üzere bazı senaryolarda ek esneklik sağlayan yeni taşıma maliyeti değeri VeryHigh.
- Kapsayıcılı uygulamalar için etkin hale [**getirme araştırması**](https://docs.microsoft.com/azure/service-fabric/probes-codepackage ) mekanizması. Lizur araştırması Kapsayıcılı uygulamanın tam olarak duyurulmasına ve zamanında yanıt vermediklerinde yeniden başlatmaya neden olur.
- [**Hizmetler için tamamlama/bir kez çalıştırma**](https://docs.microsoft.com/azure/service-fabric/run-to-completion)**

### <a name="image-store-improvements"></a>Görüntü Deposu iyileştirmeler
 - Service Fabric 7,1, **düğümler arasında dosya aktarımını varsayılan olarak güvenli hale getirmek için özel aktarım**kullanır. SMB dosya paylaşımındaki bağımlılık 7,1 sürümünden kaldırılmıştır. Güvenli SMB dosya paylaşımları, müşterinin varsayılan olarak devre dışı bırakmak ve eski sürüme yükseltmek için Görüntü Deposu hizmet çoğaltmasını içeren düğümlerde hala mevcut.
       
 ### <a name="reliable-collections-improvements"></a>Güvenilir koleksiyonlar geliştirmeleri

- [**Bellekte yalnızca güvenilir koleksiyonlar kullanılarak durum bilgisi olan hizmetler için bir destek**](https://docs.microsoft.com/azure/service-fabric/service-fabric-work-with-reliable-collections#volatile-reliable-collections)sağlanır: geçici güvenilir koleksiyonlar, verilerin büyük ölçekli kesintilere karşı dayanıklılık açısından diskte kalıcı olmasını sağlar; örneğin, büyük ölçekte veri kaybı olduğu gibi, çoğaltılan önbellek gibi iş yükleri için de kullanılabilir. [Geçici güvenilir koleksiyonların sınırlamalarına ve kısıtlamalarına](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-reliable-collections-guidelines#volatile-reliable-collections)bağlı olarak, çekirdek kaybının nadir olarak görülen hizmetler için bunu kalıcılığı gerektirmeyen iş yükleri için öneririz.
- [**Önizleme: yedekleme gezgini Service Fabric**](https://github.com/microsoft/service-fabric-backup-explorer): Service Fabric durum bilgisi olan uygulamalar Için güvenilir koleksiyonlar yedeklemelerinin yönetimini kolaylaştırmak için, Service Fabric yedekleme Gezgini kullanıcıların
    - Güvenilir koleksiyonların içeriğini denetleyin ve gözden geçirin,
    - Geçerli durumu tutarlı bir görünüm olarak güncelleştir
    - Güvenilir koleksiyonların geçerli anlık görüntüsünün yedeklemesini oluştur
    - Veri bozulmasını çözme
                 
### <a name="service-fabric-71-releases"></a>Service Fabric 7,1 yayınları
| Sürüm tarihi | Sürüm | Daha fazla bilgi |
|---|---|---|
| 20 Nisan 2020 | [Azure Service Fabric 7,1](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-7-1-release/ba-p/1311373)  | [Sürüm notları](https://github.com/microsoft/service-fabric/tree/master/release_notes/Service-Fabric-71-releasenotes.md)|


### <a name="service-fabric-70"></a>Service Fabric 7,0

Azure Service Fabric 7,0 artık kullanılabilir! Azure portal veya Azure Resource Manager dağıtımı aracılığıyla 7,0 ' e güncelleştirebilirsiniz. Tatil döneminde yer alan yayınlarda müşteri geri bildirimleri nedeniyle, kümeler otomatik olarak güncelleştirme için ayarlanan, Ocak tarihine kadar otomatik yükseltmeleri alacak şekilde başlamacağız.
Ocak ayında, standart çıkış prosedürü ve otomatik yükseltmeler etkinleştirilmiş kümeler, 7,0 güncelleştirmesini otomatik olarak almaya başlayacaktır. Kullanıma alma başlamadan önce başka bir duyuru sağlayacağız.
Ayrıca, bu ilkenin dikkate alınması gerektiğini göstermek için planlanmış yayın tarihlerimizi de güncelleştireceğiz. Gelecekteki [Sürüm zamanlamalarımızla](https://github.com/Microsoft/service-fabric/#service-fabric-release-schedule)ilgili güncelleştirmeler için buraya bakın.
 
Bu, Service Fabric en son sürümüdür ve temel özellikler ve geliştirmeler ile yüklenir.

### <a name="key-announcements"></a>Anahtar Duyurular
 - [**Uygulama gizli dizileri Için Keyvaultreference desteği (Önizleme)**](https://docs.microsoft.com/azure/service-fabric/service-fabric-keyvault-references): Service Fabric [yönetilen kimlikleri](https://docs.microsoft.com/azure/service-fabric/concepts-managed-identity) etkinleştirilmiş olan uygulamalar artık bir Key Vault gizli URL 'sine doğrudan bir ortam değişkeni, uygulama parametresi veya kapsayıcı deposu kimlik bilgileri olarak başvurabilir. Service Fabric, uygulamanın yönetilen kimliğini kullanarak gizli anahtarı otomatik olarak çözer. 
     
- **Durum bilgisi olmayan hizmetler Için geliştirilmiş yükseltme güvenliği**: bir uygulama yükseltmesi sırasında kullanılabilirliği garantilemek için, [durum bilgisi olmayan hizmetler için en az sayıda örnek](https://docs.microsoft.com/dotnet/api/system.fabric.description.statelessservicedescription?view=azure-dotnet) belirlemek için kullanılabilir kabul edilecek yeni yapılandırmaların tanıtıldık. Bu değer daha önce tüm hizmetler için 1 idi ve değiştirilemez. Bu yeni hizmet başına güvenlik denetimi sayesinde, hizmetlerinizin uygulama yükseltmeleri sırasında, küme yükseltmeleri sırasında ve Service Fabric sistem durumu ve güvenlik denetimlerine dayanan diğer bakımda en az sayıda örnek korumasını sağlayabilirsiniz.
  
- [**Kullanıcı Hizmetleri Için kaynak sınırları**](https://docs.microsoft.com/azure/service-fabric/service-fabric-resource-governance#enforcing-the-resource-limits-for-user-services): kullanıcılar, Service Fabric sistem hizmetlerinin kaynak tükenmesi gibi senaryoları engellemek için bir düğümdeki Kullanıcı Hizmetleri için kaynak sınırları ayarlayabilir. 
  
- Bir çoğaltma türü için [**çok yüksek hizmet taşıma maliyeti**](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-movement-cost) . Çok yüksek taşıma maliyeti olan çoğaltmalar yalnızca kümede başka bir şekilde sabitlenemez bir kısıtlama ihlali varsa taşınır. "Çok yüksek" taşıma maliyetinin kullanımının makul olduğu ve ek konular hakkında daha fazla bilgi için bağlantılı belgeye bakın.
  
-  **Ek küme güvenliği denetimleri**: Bu sürümde, yapılandırılabilir bir çekirdek düğümü çekirdek güvenlik denetimi sunuyoruz. Bu, küme yaşam döngüsü ve yönetim senaryolarında kaç çekirdek düğümünün kullanılabilir olması gerektiğini özelleştirmenizi sağlar. Yapılandırılan değerin altında kümeyi alacak işlemler engellenir. Günümüzde varsayılan değer her zaman çekirdek düğümlerinin bir çekirdektir. Örneğin, 7 çekirdek düğümünüz varsa, 5 tohum düğümü altına almanız gereken bir işlem varsayılan olarak engellenir. Bu değişiklik ile, en az bir çekirdek düğümün aynı anda kapatılmasını sağlayan 6 Minimum güvenli değerini yapabilirsiniz.
   
- [**Service Fabric Explorer 'de yedekleme ve geri yükleme hizmetinin yönetilmesine**](https://docs.microsoft.com/azure/service-fabric/service-fabric-backuprestoreservice-quickstart-azurecluster)yönelik destek eklendi. Bu, aşağıdaki etkinlikleri doğrudan SFX içinden mümkün hale getirir: yedekleme ve geri yükleme hizmetini keşfetme, yedekleme ilkesi oluşturma, otomatik yedeklemeleri etkinleştirme, geçici yedeklemeler alma, geri yükleme işlemlerini tetikleme ve var olan yedeklemelere göz atma.

- [**Reliablecollectionsmissingtypestool**](https://github.com/hiadusum/ReliableCollectionsMissingTypesTool)'un kullanılabilirliği duyuruldu: Bu araç, çalışan bir uygulama yükseltmesi sırasında güvenilir koleksiyonlarda kullanılan türlerin ileriye ve geriye dönük olarak uyumlu olduğunu doğrulamaya yardımcı olur. Bu, eksik veya uyumsuz türler nedeniyle yükseltme hatalarının veya veri kaybını ve verilerin bozulmasını önlemeye yardımcı olur.

- [**İkincil çoğaltmalarda kararlı okumaları etkinleştirin**](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-configuration#configuration-names-1): kararlı okumalar, ikincil çoğaltmaları, çekirdek erişimi olan değerleri döndürmeye yönelik olarak kısıtlar.

Ayrıca, bu sürüm diğer yeni özellikler, hata düzeltmeleri ve desteklenebilirlik, güvenilirlik ve performans iyileştirmeleri içerir. Değişikliklerin tam listesi için lütfen [sürüm notlarına](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_70.md)bakın.

### <a name="service-fabric-70-releases"></a>Service Fabric 7,0 yayınları

| Sürüm tarihi | Sürüm | Daha fazla bilgi |
|---|---|---|
| 18 Kasım 2019 | [Azure Service Fabric 7,0](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Service-Fabric-7-0-Release/ba-p/1015482)  | [Sürüm notları](https://github.com/microsoft/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_70.md)|
| 30 Ocak 2020 | [Azure Service Fabric 7,0 yenileme sürümü](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-7-0-second-refresh-release/ba-p/1137690)  | [Sürüm notları](https://github.com/microsoft/service-fabric/blob/master/release_notes/Service-Fabric-70CU2-releasenotes.md)|
| 6 Şubat 2020 | [Azure Service Fabric 7,0 yenileme sürümü](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-7-0-third-refresh-release/ba-p/1156508)  | [Sürüm notları](https://github.com/microsoft/service-fabric/blob/master/release_notes/Service-Fabric-70CU3-releasenotes.md)|
| 2 Mart 2020 | [Azure Service Fabric 7,0 yenileme sürümü](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-7-0-fourth-refresh-release/ba-p/1205414)  | [Sürüm notları](https://github.com/microsoft/service-fabric/blob/master/release_notes/Service-Fabric-70CU4-releasenotes.md)

### <a name="service-fabric-65"></a>Service Fabric 6,5

Bu sürüm, küme ve uygulama yaşam döngüsü yönetimini kolaylaştırmak için desteklenebilirlik, güvenilirlik ve performans iyileştirmeleri, yeni özellikler, hata düzeltmeleri ve geliştirmeler içerir.

> [!IMPORTANT]
> Service Fabric 6,5, Visual Studio 2015 ' de Service Fabric araçları desteğiyle son sürümdür. Müşterilerin ileri giderek [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) ' e taşınması önerilir.

Service Fabric 6,5 ' deki yenilikler aşağıda verilmiştir:

- Service Fabric Explorer, görüntü deposuna yüklediğiniz uygulamaları incelemek için bir [görüntü deposu Görüntüleyici](service-fabric-visualizing-your-cluster.md#image-store-viewer) içerir.

- [Düzeltme Eki düzenleme uygulaması (POA)](service-fabric-patch-orchestration-application.md) sürüm [1.4.0](https://github.com/microsoft/Service-Fabric-POA/releases/tag/v1.4.0) , birçok kendi kendine tanılama geliştirmesi içerir. POA müşterilerinin bu sürüme taşınması önerilir.

- [Eventstore hizmeti,](service-fabric-visualizing-your-cluster.md#event-store) siz kabul etmediğiniz müddetçe Service Fabric 6,5 kümeleri için varsayılan olarak etkinleştirilmiştir.

- Durum bilgisi olan hizmetler için [çoğaltma yaşam döngüsü olayları](service-fabric-diagnostics-event-generation-operational.md#replica-events) eklendi.

- Çekirdek düğümünün sağlıksız olması durumunda (*aşağı*, *kaldırılmış* veya *bilinmiyor*) küme düzeyinde uyarılar da dahil olmak üzere [çekirdek düğüm durumunun daha iyi görünebilirliği](service-fabric-understand-and-troubleshoot-with-system-health-reports.md#seed-node-status).

- [Service Fabric uygulama olağanüstü durum kurtarma aracı](https://github.com/Microsoft/Service-Fabric-AppDRTool) , birincil küme bir olağanüstü durum ile karşılaştığında, durum bilgisi olan hizmetlerin hızla kurtarılmasını Service Fabric sağlar. Birincil kümeden alınan veriler, düzenli yedekleme ve geri yükleme kullanılarak ikincil bekleme uygulamasıyla sürekli olarak eşitlenir.

- [.NET Core uygulamalarını Linux tabanlı kümelere yayımlamak](service-fabric-how-to-publish-linux-app-vs.md)Için Visual Studio desteği.

- Azure 'da yeni bir Linux kümesi yükselttiğinizde veya oluşturduğunuzda, [azure SERVICE fabrıc CLI (SFCTL)](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) , Service Fabric 6,5 (ve sonraki sürümler) için otomatik olarak yüklenir.

- [Sfctl](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) , MacOS/Linux Onebox kümelerinde varsayılan olarak yüklenir.

Daha fazla ayrıntı için [Service Fabric 6,5 sürüm notlarına](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65.pdf)bakın.

### <a name="service-fabric-65-releases"></a>Service Fabric 6,5 yayınları

| Sürüm tarihi | Sürüm | Daha fazla bilgi |
|---|---|---|
| 11 Haziran 2019 | [Azure Service Fabric 6,5](https://blogs.msdn.microsoft.com/azureservicefabric/2019/06/11/azure-service-fabric-6-5-release/)  | [Sürüm notları](https://github.com/microsoft/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65.pdf)|
| 2 Temmuz 2019 | [Azure Service Fabric 6,5 yenileme sürümü](https://blogs.msdn.microsoft.com/azureservicefabric/2019/07/04/azure-service-fabric-6-5-refresh-release/)  | [Sürüm notları](https://github.com/microsoft/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU1.pdf)  |
| 29 Temmuz 2019 | [Azure Service Fabric 6,5 yenileme sürümü](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Second-Refresh-Release/ba-p/800523)  | [Sürüm notları](https://github.com/microsoft/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU2.pdf)  |
| 23 Ağu 2019 | [Azure Service Fabric 6,5 yenileme sürümü](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Third-Refresh-Release/ba-p/818599)  | [Sürüm notları](https://github.com/microsoft/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU3.pdf)  |
| 14 Eki, 2019 | [Azure Service Fabric 6,5 yenileme sürümü](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Fifth-Refresh-Release/ba-p/913296)  | [Sürüm notları] (https://github.com/microsoft/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU5.md  |


## <a name="previous-versions"></a>Önceki sürümler

### <a name="service-fabric-64-releases"></a>Service Fabric 6,4 yayınları

| Sürüm tarihi | Sürüm | Daha fazla bilgi |
|---|---|---|
| 30 Kasım 2018 | [Azure Service Fabric 6,4](https://blogs.msdn.microsoft.com/azureservicefabric/2018/11/30/azure-service-fabric-6-4-release/)  | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2018/12/Service-Fabric-6.4-Release.pdf)|
| 12 Aralık 2018 | [Windows kümeleri için Azure Service Fabric 6,4 yenileme sürümü](https://blogs.msdn.microsoft.com/azureservicefabric/2018/12/12/azure-service-fabric-6-4-refresh-for-windows-clusters/)  | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2018/12/Links.pdf)  |
| 4 Şubat 2019 | [Azure Service Fabric 6,4 yenileme sürümü](https://blogs.msdn.microsoft.com/azureservicefabric/2019/02/04/azure-service-fabric-6-4-refresh-release/) | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2019/02/Service-Fabric-6.4CU3-Release-Notes.pdf) |
| 4 Mart 2019 | [Azure Service Fabric 6,4 yenileme sürümü](https://blogs.msdn.microsoft.com/azureservicefabric/2019/03/12/azure-service-fabric-6-4-refresh-release-2/) | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2019/03/Service-Fabric-6.4CU4-Release-Notes.pdf)
| 8 Nisan 2019 | [Azure Service Fabric 6,4 yenileme sürümü](https://blogs.msdn.microsoft.com/azureservicefabric/2019/04/08/azure-service-fabric-6-4-refresh-release-5/) | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2019/04/Service-Fabric-6.4CU5-ReleaseNotes3.pdf)
| 2 Mayıs 2019 | [Azure Service Fabric 6,4 yenileme sürümü](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/02/azure-service-fabric-6-4-refresh-release-3/) | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2019/05/Service-Fabric-64CU6-Release-Notes-V2.pdf)
| 28 Mayıs 2019 | [Azure Service Fabric 6,4 yenileme sürümü](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/28/azure-service-fabric-6-4-refresh-release-4/) | [Sürüm notları](https://msdnshared.blob.core.windows.net/media/2019/05/Service_Fabric_64CU7_Release_Notes1.pdf)
