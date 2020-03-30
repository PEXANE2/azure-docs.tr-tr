---
title: Azure Hizmet Kumaşı hakkında daha fazla bilgi edinin
description: Azure Hizmet Kumaşı'nın temel kavramları ve temel alanları hakkında bilgi edinin. Service Fabric ve mikro hizmetlerin nasıl oluşturulabildiğini hakkında geniş bir genel bakış sağlar.
ms.topic: conceptual
ms.date: 12/08/2017
ms.openlocfilehash: 4e6e21f5f9ebfeddb5292e00dc8a929341e77372
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75458154"
---
# <a name="so-you-want-to-learn-about-service-fabric"></a>Service Fabric hakkında bilgi edinmek ister misiniz?
Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırmayı sağlayan bir dağıtılmış sistemler platformudur.  Hizmet Kumaş geniş bir yüzey alanına sahiptir, ancak, ve öğrenmek için çok şey var.  Bu makalede, Hizmet Kumaş bir özet sağlar ve temel kavramları, programlama modelleri, uygulama yaşam döngüsü, test, kümeleri ve sistem durumu izleme açıklar. Genel [Bakış](service-fabric-overview.md) ve [Mikro hizmetler nelerdir?](service-fabric-overview-microservices.md) Bu makale kapsamlı bir içerik listesi içermez, ancak Hizmet Kumaşı'nın her alanı için genel bakış ve makaleleri başlatma bağlantısı içerir. 

## <a name="core-concepts"></a>Temel kavramlar
[Hizmet Kumaş terminolojisi](service-fabric-technical-overview.md), [Uygulama modeli](service-fabric-application-model.md)ve [Desteklenen programlama modelleri](service-fabric-choose-framework.md) daha fazla kavram ve açıklamalar sağlamak, ancak burada temelleri vardır.

### <a name="design-time-service-type-service-package-and-manifest-application-type-application-package-and-manifest"></a>Tasarım süresi: servis türü, servis paketi ve manifestosu, uygulama türü, uygulama paketi ve manifesto
Hizmet türü, bir hizmetin kod paketlerine, veri paketlerine ve yapılandırma paketlerine atanan ad/sürümdür. Bu, ServiceManifest.xml dosyasında tanımlanır. Hizmet türü, çalıştırılabilir kod ve hizmet yapılandırma ayarlarından, çalışma zamanında yüklenen ayarlardan ve hizmet tarafından tüketilen statik verilerden oluşur.

Hizmet paketi, hizmet türüiçin kod, statik veri ve yapılandırma paketlerine başvuran hizmet türünün ServiceManifest.xml dosyasını içeren bir disk dizinidir. Örneğin, bir hizmet paketi, veritabanı hizmetini oluşturan kod, statik veri ve yapılandırma paketlerine başvurabilir.

Uygulama türü, hizmet türleri koleksiyonuna atanan ad/sürümdür. Bu, ApplicationManifest.xml dosyasında tanımlanır.

![Servis Kumaş ı uygulama türleri ve servis türleri][cluster-imagestore-apptypes]

Uygulama paketi, uygulama türünü oluşturan her hizmet türü için servis paketlerine başvuran uygulama türünün ApplicationManifest.xml dosyasını içeren bir disk dizinidir. Örneğin, bir e-posta uygulama türü için bir uygulama paketi, kuyruk hizmet paketine, ön uç hizmet paketine ve veritabanı hizmet paketine başvurular içerebilir.  

Uygulama paketi dizinindeki dosyalar Service Fabric kümesinin görüntü deposuna kopyalanır. Daha sonra küme içinde çalışan bu uygulama türünden adlandırılmış bir uygulama oluşturabilirsiniz. Adlandırılmış bir uygulama oluşturduktan sonra, uygulama türünün hizmet türlerinden birinden adlandırılmış bir hizmet oluşturabilirsiniz. 

### <a name="run-time-clusters-and-nodes-named-applications-named-services-partitions-and-replicas"></a>Çalışma süresi: kümeler ve düğümler, adlandırılmış uygulamalar, adlandırılmış hizmetler, bölümler ve yinelemeler
[Service Fabric kümesi,](service-fabric-deploy-anywhere.md) mikro hizmetlerinizin dağıtıldığı ve yönetildiği ağa bağlı sanal veya fiziksel makineler kümesidir. Kümeler binlerce makine içerecek şekilde ölçeklendirilebilir.

Bir kümenin parçası olan makine veya VM’lere düğüm denir. Her düğüme bir düğüm adı (bir dize) atanır. Düğümlerin yerleşim özellikleri gibi özellikleri vardır. Her makine veya VM bir otomatik `FabricHost.exe`başlatma Windows hizmeti vardır, önyükleme üzerine `Fabric.exe` çalışmaya `FabricGateway.exe`başlar ve sonra iki çalıştırılabilir başlar: ve . Bu iki yürütülebilir düğüm makyaj. Geliştirme veya sınama senaryoları için, birden çok örneği çalıştırarak tek bir `Fabric.exe` makinede veya VM'de birden çok düğüm barındırabilirsiniz. `FabricGateway.exe`

Adlandırılmış uygulama, belirli bir işlev veya işlevleri gerçekleştiren adlandırılmış hizmetler topluluğudur. Bir hizmet tam ve bağımsız bir işlev gerçekleştirir (diğer hizmetlerden bağımsız olarak başlatılabilir ve çalıştırılabilir) ve kod, yapılandırma ve verilerden oluşur. Bir uygulama paketi görüntü deposuna kopyalandıktan sonra, uygulama paketinin uygulama türünü (adını/sürümünü kullanarak) belirterek küme içindeki uygulamanın bir örneğini oluşturursunuz. Her uygulama türü örneği *kumaş atanır:/MyNamedApp*. Bir küme içinde, tek bir uygulama türünden birden çok adlandırılmış uygulama oluşturabilirsiniz. Farklı uygulama türlerinden adlandırılmış uygulamalar da oluşturabilirsiniz. Adlandırılmış her uygulama yönetilir ve bağımsız olarak sürülür.

Adlandırılmış bir uygulama oluşturduktan sonra, hizmet türünü (adını/sürümünü kullanarak) belirterek kümeiçindeki hizmet türlerinden birinin (adlandırılmış hizmet) bir örneğini oluşturabilirsiniz. Her hizmet türü örneği, adlandırılmış uygulamanın URI altında kapsamlı bir URI adı atanır. Örneğin, "MyNamedApp" adlı bir uygulama içinde "MyDatabase" adlı bir hizmet oluşturursanız, URI aşağıdaki gibi görünür: *kumaş:/MyNamedApp/MyDatabase*. Adlandırılmış bir uygulama içinde, bir veya daha fazla adlandırılmış hizmet oluşturabilirsiniz. Adlandırılmış her hizmetin kendi bölüm düzeni ve örnek/çoğaltma sayıları olabilir. 

İki tür hizmet vardır: devletsiz ve durumlu. Devletsiz hizmetler, hizmet içinde durumu depolamaz. Azure Depolama, Azure SQL Veritabanı veya Azure Cosmos DB gibi harici bir depolama hizmetinde sürekli depolama alanı veya depolama durumu olmadan hizmet yoktur. Devlet hizmeti, hizmet içinde durumu saklar ve durumu yönetmek için Güvenilir Koleksiyonlar veya Güvenilir Aktörler programlama modellerini kullanır. 

Adlandırılmış bir hizmet oluştururken, bir bölüm düzeni belirtirsiniz. Büyük miktarda durum içeren hizmetler verileri bölümlere böler. Her bölüm, kümenin düğümlerine yayılan hizmetin tam durumunun bir kısmından sorumludur.  

Aşağıdaki diyagram, uygulamalar ve hizmet örnekleri, bölümler ve yinelemeler arasındaki ilişkiyi gösterir.

![Bir hizmet içindeki bölümler ve yinelemeler][cluster-application-instances]

### <a name="partitioning-scaling-and-availability"></a>Bölümleme, ölçekleme ve kullanılabilirlik
[Bölümleme,](service-fabric-concepts-partitioning.md) Service Fabric'e özgü değildir. Iyi bilinen bir bölümleme biçimi veri bölümleme veya parçalamadır. Büyük miktarda durum içeren devlet hizmetleri verileri bölümlere böler. Her bölüm, hizmetin tam durumunun bir kısmından sorumludur. 

Her bölümün kopyaları kümenin düğümlerine yayılır ve bu da adlandırılmış hizmetin [durumunun ölçeklendirmesine](service-fabric-concepts-scalability.md)olanak tanır. Veri gereksinimleri büyüdükçe, bölümler büyür ve Service Fabric donanım kaynaklarını verimli bir şekilde kullanmak için düğümler boyunca bölümleri yeniden dengeler. Kümeye yeni düğümler eklerseniz, Hizmet Kumaşı artan düğüm sayısı boyunca bölüm yinelemelerini yeniden dengeler. Genel uygulama performansı artırır ve belleğe erişim için çekişme azalır. Kümedeki düğümler verimli bir şekilde kullanılıyorsa, kümedeki düğüm sayısını azaltabilirsiniz. Service Fabric, her düğümdeki donanımı daha iyi kullanmak için azalan düğüm sayısı boyunca bölüm yinelemelerini yeniden dengeler.

Bir bölüm içinde, stateful adlandırılmış hizmetlerin yinelemeleri varken, adsız adlandırılmış hizmetlerin örnekleri vardır. Genellikle, stateless adlı hizmetler hiçbir iç durumu olduğundan sadece bir bölüm var. Bölüm örnekleri [kullanılabilirlik](service-fabric-availability-services.md)sağlar. Bir örnek başarısız olursa, diğer örnekler normal çalışmaya devam eder ve ardından Service Fabric yeni bir örnek oluşturur. Stateful adlı hizmetler yinelemeler içinde durumlarını korumak ve her bölüm kendi yineleme kümesi vardır. Okuma ve yazma işlemleri tek bir yinelemede (Birincil olarak adlandırılır) gerçekleştirilir. Yazma işlemlerinden durum değişiklikleri birden çok diğer yinelemelere (Etkin İkinciller olarak adlandırılır) kopyalanır. Bir yineleme başarısız olursa, Hizmet Kumaşı varolan yinelemelerden yeni bir yineleme oluşturur.

## <a name="stateless-and-stateful-microservices-for-service-fabric"></a>Service Fabric için durum bilgisi olan ve olmayan mikro hizmetler
Service Fabric, mikro hizmetlerden veya kapsayıcılardan oluşmuş uygulamalar hazırlamanıza olanak tanır. Durum bilgisi olmayan mikro hizmetler (protokol ağ geçitleri ve web ara sunucuları gibi), isteğin veya hizmetten gelen yanıtının dışında değişebilir bir durum bilgisi bulundurmaz. Azure Cloud Services çalışan rolleri, durum bilgisi olmayan hizmet örnekleridir. Durum bilgisi olan mikro hizmetler (kullanıcı hesapları, veritabanları, cihazlar, alışveriş sepetleri ve kuyruklar gibi), isteğin ve yanıtının ötesinde değişebilir, yetkili bir durum bilgisi bulundurur. Günümüzün İnternet ölçeğindeki uygulamaları, durum bilgisi olan ve olmayan mikro hizmetlerin bileşiminden oluşur. 

Service Fabric ile önemli bir farklılaşma, programlama [modelleri yerleşik](service-fabric-choose-framework.md) ya da konteyner devlet hizmetleri ile, devlet hizmetleri bina güçlü odak noktasıdır. [Uygulama senaryoları](service-fabric-application-scenarios.md), durum bilgisi olan hizmetlerin kullanıldığı senaryoları açıklar.

Neden devletsiz olanlar ile birlikte devlet mikro hizmetler var? İki ana nedeni şunlardır:

* Kodu ve verileri aynı makinede kapalı tutarak yüksek iş gücü, düşük gecikme, hataya dayanıklı çevrimiçi işlem işleme (OLTP) hizmetleri oluşturabilirsiniz. Bazı örnekler interaktif vitrinler, arama, Nesnelerin İnterneti (IoT) sistemleri, ticaret sistemleri, kredi kartı işleme ve dolandırıcılık algılama sistemleri ve kişisel kayıt yönetimidir.
* Uygulama tasarımını basitleştirebilirsiniz. Devlete uygun mikro hizmetler, tamamen durum suz bir uygulamanın kullanılabilirlik ve gecikme gereksinimlerini gidermek için geleneksel olarak gerekli olan ek kuyruklar ve önbellekler gereksinimini ortadan kaldırır. Devlet hizmetleri doğal olarak yüksek kullanılabilirlik ve düşük gecikme süresidir, bu da uygulamanızda bir bütün olarak yönetilmesi gereken hareketli parça sayısını azaltır.

## <a name="supported-programming-models"></a>Desteklenen programlama modelleri
Service Fabric, hizmetlerinizi yazmanın ve yönetmenin birden çok yolunu sunar. Hizmetler, platformun özelliklerinden ve uygulama çerçevelerinden tam olarak yararlanmak için Service Fabric API'lerini kullanabilir. Hizmetler ayrıca herhangi bir dilde yazılmış ve Bir Hizmet Kumaş kümesi üzerinde barındırılan herhangi bir derlenebilir yürütülebilir program olabilir. Daha fazla bilgi için [desteklenen programlama modellerine](service-fabric-choose-framework.md)bakın.

### <a name="containers"></a>Kapsayıcılar
Varsayılan olarak, Service Fabric hizmetleri işlem olarak dağıtır ve etkinleştirir. Service Fabric de [kapsayıcılarda](service-fabric-containers-overview.md)hizmetleri dağıtabilirsiniz. Daha da önemlisi, aynı uygulamada kapsayıcılarda süreçler ve hizmetlerdeki hizmetleri karıştırabilirsiniz. Service Fabric, Windows Server 2016'da Linux kapsayıcılarının ve Windows kapsayıcılarının dağıtımını destekler. Varolan uygulamaları, durum ifadebilmeyen hizmetleri veya durum hizmetlerini kapsayıcılara dağıtabilirsiniz. 

### <a name="reliable-services"></a>Reliable Services
[Güvenilir Hizmetler,](service-fabric-reliable-services-introduction.md) Service Fabric platformuyla bütünleşen ve tüm platform özelliklerinden yararlanan yazma hizmetleri için hafif bir çerçevedir. Güvenilir Hizmetler, devlet durumunun Azure DB veya Azure Tablo Depolama gibi harici bir çözümde kalıcı olduğu (web sunucuları veya Azure Bulut Hizmetlerinde Çalışan Rolleri gibi çoğu hizmet platformuna benzer) durum ifade edilemez olabilir. Güvenilir Hizmetler, durumunun Güvenilir Koleksiyonlar kullanılarak doğrudan hizmetin kendisinde kalıcı olduğu durum durumu da kullanılabilir. Durum çoğaltma yoluyla [son derece kullanılabilir](service-fabric-availability-services.md) hale getirilir ve [bölümleme](service-fabric-concepts-partitioning.md)yoluyla dağıtılır , tüm Service Fabric tarafından otomatik olarak yönetilir.

### <a name="reliable-actors"></a>Reliable Actors
Güvenilir Hizmetler üzerine inşa edilen [Güvenilir Aktör](service-fabric-reliable-actors-introduction.md) çerçevesi, aktör tasarım desenine dayalı Sanal Aktör deseni uygulayan bir uygulama çerçevesidir. Güvenilir Aktör çerçevesi, bağımsız bilgi işlem ve durum birimlerini, aktörler adı verilen tek iş parçacığı yürütmeile kullanır. Güvenilir Aktör çerçevesi aktörler ve önceden ayarlanmış durum kalıcılığı ve ölçeklendirme yapılandırmaları için iletişim yerleşik sağlar.

### <a name="aspnet-core"></a>ASP.NET Core
Service Fabric, web ve API uygulamaları oluşturmak için birinci sınıf bir programlama modeli olarak [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) ile bütünleşir.  ASP.NET Core Servis Kumaşı'nda iki farklı şekilde kullanılabilir:

- Bir konuk yürütülebilir olarak barındırılan. Bu, öncelikle hizmet kumaşında varolan ASP.NET Çekirdek uygulamalarını kod değişikliği olmadan çalıştırmak için kullanılır.
- Güvenilir bir hizmetin içinde çalıştırın. Bu, Service Fabric çalışma zamanı ile daha iyi tümleştirme sağlar ve özel ASP.NET Core hizmetleri sağlar.

### <a name="guest-executables"></a>Konuk yürütülebilir dosyaları
[Yürütülebilir konuk,](service-fabric-guest-executables-introduction.md) diğer hizmetlerin yanı sıra hizmet kumaşı kümesinde barındırılan varolan, rasgele yürütülebilir (herhangi bir dilde yazılmış) dır. Konuk çalıştırılabilir uygulamalar doğrudan Service Fabric API'lerle bütünleşmez. Bununla birlikte, REST API'lerini arayarak özel sağlık ve yük raporlama ve hizmet keşfedilebilirliği gibi platform sunduğu özelliklerden hala yararlanırlar. Ayrıca tam uygulama yaşam döngüsü desteğine de sahiptir. 

## <a name="application-lifecycle"></a>Uygulama yaşam döngüsü
Diğer platformlarda olduğu gibi, Service Fabric'teki bir uygulama genellikle aşağıdaki aşamalardan geçer: tasarım, geliştirme, test, dağıtım, yükseltme, bakım ve kaldırma. Service Fabric, geliştirmeden dağıtıma, günlük yönetime ve bakıma ve nihai kullanımdan kaldırmaya kadar bulut uygulamalarının tam uygulama yaşam döngüsü için birinci sınıf destek sağlar. Hizmet modeli, uygulama yaşam döngüsüne bağımsız olarak birkaç farklı rolün katılmasını sağlar. [Service Fabric uygulama yaşam döngüsü,](service-fabric-application-lifecycle.md) API'lere ve Hizmet Kumaşı uygulama yaşam döngüsünün aşamaları boyunca farklı roller tarafından nasıl kullanıldığına genel bir bakış sağlar. 

Tüm uygulama yaşam döngüsü [PowerShell cmdlets](/powershell/module/ServiceFabric/)kullanılarak yönetilebilir , [CLI komutları](service-fabric-sfctl.md), [C # API'ler](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient), Java [API'ler](/java/api/overview/azure/servicefabric), ve [REST API'ler](/rest/api/servicefabric/). Ayrıca, [Azure Ardışık Hatları](service-fabric-set-up-continuous-integration.md) veya [Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md)gibi araçları kullanarak sürekli tümleştirme/sürekli dağıtım ardışık lıkları da ayarlayabilirsiniz.

## <a name="test-applications-and-services"></a>Uygulama ve hizmetleri test etme
Gerçek bulut ölçekli hizmetler oluşturmak için, uygulamalarınızın ve hizmetlerinizin gerçek dünyadaki hatalara dayanabileceğini doğrulamak çok önemlidir. Arıza Analizi Hizmeti, Service Fabric üzerine kurulu hizmetleri test etmek için tasarlanmıştır. Hata [Analizi Hizmeti](service-fabric-testability-overview.md)ile anlamlı hatalara neden olabilir ve uygulamalarınıza karşı tam test senaryoları çalıştırabilirsiniz. Bu hatalar ve senaryolar, bir hizmetin ömrü boyunca, tümü kontrollü, güvenli ve tutarlı bir şekilde yaşayacağı çok sayıda durumu ve geçişi kullanır ve doğrular.

[Eylemler,](service-fabric-testability-actions.md) tek tek hataları kullanarak sınama için bir hizmeti hedeflemez. Bir hizmet geliştiricisi karmaşık senaryolar yazmak için bunları yapı taşları olarak kullanabilir. Simüle edilen hatalara örnek olarak şunlar verilebilir:

* Makine veya VM'nin yeniden başlatıldığı durumların herhangi bir sayısını simüle etmek için düğümü yeniden başlatın.
* Yük dengeleme, başarısızlık veya uygulama yükseltmesini simüle etmek için devlet hizmetinizin bir kopyasını taşıyın.
* Yeni verileri kabul etmek için yeterli "yedekleme" veya "ikincil" yineleme olmadığından, yazma işlemlerinin devam edemediği bir durum oluşturmak için durum lu bir hizmette yeterli çoğunluk kaybını başlatın.
* Tüm bellek durumunun tamamen silindiği bir durum oluşturmak için durum bilgisine yönelik bir hizmette veri kaybı nı çağırın.

[Senaryolar,](service-fabric-testability-scenarios.md) bir veya daha fazla eylemden oluşan karmaşık işlemlerdir. Hata Çözümleme Hizmeti iki yerleşik tam senaryo sağlar:

* [Kaos senaryosu](service-fabric-controlled-chaos.md)- uzun süreler boyunca küme boyunca sürekli, ara sıra gelen hataları (hem zarif hem de zarif olmayan) simüle eder.
* [Failover senaryosu](service-fabric-testability-scenarios.md#failover-test)- diğer hizmetleri etkilenmeden bırakırken belirli bir hizmet bölümü hedefleyen kaos testi senaryosunun bir sürümü.

## <a name="clusters"></a>Kümeler
[Service Fabric kümesi,](service-fabric-deploy-anywhere.md) mikro hizmetlerinizin dağıtıldığı ve yönetildiği ağa bağlı sanal veya fiziksel makineler kümesidir. Kümeler binlerce makine içerecek şekilde ölçeklendirilebilir. Kümenin bir parçası olan bir makine veya VM'ye küme düğümü denir. Her düğüme bir düğüm adı (bir dize) atanır. Düğümlerin yerleşim özellikleri gibi özellikleri vardır. Fabric.exe ve FabricGateway.exe: Her makine veya VM bir otomatik başlatma hizmeti vardır, `FabricHost.exe`önyükleme üzerine çalışan ve daha sonra iki çalıştırılabilir başlar. Bu iki yürütülebilir düğüm makyaj. Senaryoları test etmek için, birden çok örneği çalıştırarak tek bir makinede `Fabric.exe` `FabricGateway.exe`veya VM'de birden çok düğüm barındırabilirsiniz.

Service Fabric kümeleri, Windows Server veya Linux çalıştıran sanal veya fiziksel makinelerde oluşturulabilir. Service Fabric uygulamalarını, şirket içinde, Microsoft Azure'da veya herhangi bir bulut sağlayıcısında birbirine bağlı bir windows server veya Linux bilgisayar kümesine sahip olduğunuz herhangi bir ortamda dağıtabilir ve çalıştırabilirsiniz.

### <a name="clusters-on-azure"></a>Azure’da kümeler
Azure'da Hizmet Kumaşı kümelerini çalıştırmak, kümenin işlemlerini ve yönetimini daha kolay ve daha güvenilir hale getiren diğer Azure özellikleri ve hizmetleriyle tümleştirme sağlar. Küme bir Azure Kaynak Yöneticisi kaynağıdır, böylece kümeleri Azure'daki diğer kaynaklar gibi modelleyebilirsiniz. Kaynak Yöneticisi ayrıca küme tarafından tek bir birim olarak kullanılan tüm kaynakların kolay yönetimini de sağlar. Azure'daki kümeler Azure tanılama ve Azure Monitör günlükleriyle tümleştirilir. Küme düğüm türleri [sanal makine ölçek kümeleridir,](/azure/virtual-machine-scale-sets/index)bu nedenle otomatik ölçeklendirme işlevi yerleşiktir.

[Azure portalı](service-fabric-cluster-creation-via-portal.md)aracılığıyla, bir [şablondan](service-fabric-cluster-creation-via-arm.md)veya [Visual Studio'dan](service-fabric-cluster-creation-via-visual-studio.md)Azure'da bir küme oluşturabilirsiniz.

Linux'taki Service Fabric, Tıpkı Windows'da olduğu gibi Linux'ta da yüksek oranda kullanılabilir, yüksek ölçeklenebilir uygulamalar oluşturmanıza, dağıtmanıza ve yönetmenize olanak tanır. Service Fabric çerçeveleri (Güvenilir Hizmetler ve Güvenilir Aktörler) C# (.NET Core) ek olarak Linux'ta Java'da mevcuttur. Ayrıca herhangi bir dil veya çerçeve ile [konuk yürütülebilir hizmetler](service-fabric-guest-executables-introduction.md) oluşturabilirsiniz. Docker konteynerlerin düzenlenmesi de desteklenir. Docker konteynerleri, Hizmet Kumaşı çerçevelerini kullanan konuk çalıştırılabilir veya yerel Service Fabric hizmetlerini çalıştırabilir. Daha fazla bilgi [için, Linux'ta Service Fabric](service-fabric-deploy-anywhere.md)hakkında bilgi edinin.

Windows'da desteklenen ancak Linux'ta desteklenmeyen bazı özellikler vardır. Daha fazla bilgi edinmek [için, Linux ve Windows'da Hizmet Kumaşı arasındaki farkları](service-fabric-linux-windows-differences.md)okuyun.

### <a name="standalone-clusters"></a>Tek başına kümeler
Service Fabric, şirket içinde veya herhangi bir bulut sağlayıcısında bağımsız Service Fabric kümeleri oluşturmanız için bir yükleme paketi sağlar. Bağımsız kümeler, bir kümeyi istediğiniz yerde barındırma özgürlüğü verir. Verileriniz uyumluluk veya düzenleyici kısıtlamalara tabiyse veya verilerinizi yerel tutmak istiyorsanız, kendi kümenizi ve uygulamalarınızı barındırabilirsiniz. Service Fabric uygulamaları hiçbir değişiklik olmadan birden fazla barındırma ortamında çalıştırılabilir, böylece bina uygulamaları bilginiz bir barındırma ortamından diğerine taşınır. 

[İlk tek başına Service Fabric kümenizi oluşturma](service-fabric-cluster-creation-for-windows-server.md)

Linux bağımsız kümeleri henüz desteklenmez.

### <a name="cluster-security"></a>Küme güvenliği
Kümeler, yetkisiz kullanıcıların kümenize bağlanmasını önlemek için, özellikle de üzerinde üretim iş yükleri varsa güvenli olmalıdır. Güvenli olmayan bir küme oluşturmak mümkün olsa da, yönetim uç noktaları genel internete maruz kaldığında anonim kullanıcıların bu kümeye bağlanmasına olanak tanır. Daha sonra güvenli olmayan bir kümede güvenliği etkinleştirmek mümkün değildir: küme oluşturma zamanında küme güvenliği etkinleştirilir.

Küme güvenlik senaryoları şunlardır:
* Düğümden düğüme güvenlik
* İstemciden düğüme güvenlik
* Rol tabanlı erişim denetimi (RBAC)

Daha fazla bilgi için [bir kümeyi güvenli olarak](service-fabric-cluster-security.md)okuyun.

### <a name="scaling"></a>Ölçeklendirme
Kümeye yeni düğümler eklerseniz, Service Fabric artan düğüm sayısı boyunca bölüm yinelemelerini ve örneklerini yeniden dengeler. Genel uygulama performansı artırır ve belleğe erişim için çekişme azalır. Kümedeki düğümler verimli bir şekilde kullanılıyorsa, kümedeki düğüm sayısını azaltabilirsiniz. Service Fabric, her düğümdeki donanımı daha iyi kullanmak için bölüm yinelemelerini ve örnekleri azalan düğüm sayısı boyunca yeniden dengeler. Azure'daki kümeleri [el ile](service-fabric-cluster-scale-up-down.md) veya [programlı olarak](service-fabric-cluster-programmatic-scaling.md)ölçeklendirebilirsiniz. Bağımsız kümeler [el ile](service-fabric-cluster-windows-server-add-remove-nodes.md)ölçeklendirilebilir.

### <a name="cluster-upgrades"></a>Küme yükseltmeleri
Periyodik olarak, Service Fabric çalışma zamanının yeni sürümleri yayımlanır. Her zaman desteklenen bir [sürümü](service-fabric-support.md)çalıştıran böylece kümenizde çalışma zamanı veya kumaş yükseltmeleri gerçekleştirin. Kumaş yükseltmelerine ek olarak, sertifikalar veya uygulama bağlantı noktaları gibi küme yapılandırmasını da güncelleştirebilirsiniz.

Hizmet Kumaşı kümesi, sahip olduğunuz ancak kısmen Microsoft tarafından yönetilen bir kaynaktır. Microsoft, kümenizde temel işletim sistemi yama ve kumaş yükseltmeleri gerçekleştirmekten sorumludur. Microsoft yeni bir sürüm yayımladığında, kümenizi otomatik kumaş yükseltmeleri alacak şekilde ayarlayabilir veya istediğiniz desteklenen kumaş sürümünü seçmeyi seçebilirsiniz. Kumaş ve yapılandırma yükseltmeleri Azure portalı veya Kaynak Yöneticisi aracılığıyla ayarlanabilir. Daha fazla bilgi için [Hizmet Kumaşı kümesini Yükselt'i](service-fabric-cluster-upgrade.md)okuyun. 

Bağımsız küme, tamamen sahip olduğunuz bir kaynaktır. Altta yatan işletim sistemi yama ve kumaş yükseltmeleri başlatılması sorumludur. Kümenize [https://www.microsoft.com/download](https://www.microsoft.com/download)bağlanabiliyorsa, kümenizi yeni Hizmet Kumaşı çalışma zamanı paketini otomatik olarak karşıdan yükleyip sağlayacak şekilde ayarlayabilirsiniz. Daha sonra yükseltme başlatAcak. Kümeniz erişemiyorsa, [https://www.microsoft.com/download](https://www.microsoft.com/download)yeni çalışma zamanı paketini internete bağlı bir makineden el ile indirebilir ve yükseltmeyi başlatabilirsiniz. Daha fazla bilgi için tek [başına hizmet kumaş küme yükseltme](service-fabric-cluster-upgrade-windows-server.md)okuyun.

## <a name="health-monitoring"></a>Sistem durumu izleme
Service Fabric, belirli varlıklardaki (küme düğümleri ve hizmet yinelemeleri gibi) sağlıksız küme ve uygulama koşullarını işaretlemek üzere tasarlanmış bir [sistem durumu modelini](service-fabric-health-introduction.md) sunar. Sağlık modeli sağlık muhabirleri (sistem bileşenleri ve watchdogs) kullanır. Amaç kolay ve hızlı tanı ve onarım. Hizmet yazarları sağlık ve nasıl sağlık [raporlama tasarımı](service-fabric-report-health.md#design-health-reporting)hakkında önceden düşünmek gerekir. Özellikle köke yakın bayrak sorunlarına yardımcı olabilir, sağlık etkileyebilir herhangi bir durum rapor edilmelidir. Sağlık bilgileri, hizmet üretimde ölçekte çalışmaya başladıktan sonra hata ayıklama ve soruşturma konusunda zaman ve çabadan tasarruf edebilir.

Service Fabric muhabirleri belirlenen ilgi koşullarını izlerler. Yerel görüşlerine göre bu koşulları rapor ediyorlar. [Sistem durumu deposu,](service-fabric-health-introduction.md#health-store) varlıkların genel olarak sağlıklı olup olmadığını belirlemek için tüm muhabirler tarafından gönderilen sağlık verilerini toplar. Modelzengin, esnek ve kullanımı kolay olması amaçlanmıştır. Sistem durumu raporlarının kalitesi kümenin sistem durumu görünümünün doğruluğunu belirler. Sağlıksız sorunları yanlış gösteren yanlış pozitif sonuçlar, yükseltmeleri veya sistem durumu verilerini kullanan diğer hizmetleri olumsuz etkileyebilir. Bu tür hizmetlere örnek olarak onarım hizmetleri ve uyarı mekanizmaları verilebilir. Bu nedenle, bazı düşünce mümkün olan en iyi şekilde ilgi koşulları yakalamak raporlar sağlamak için gereklidir.

Raporlama şu andan itibaren yapılabilir:
* İzlenen Hizmet Kumaşı hizmet çoğaltma veya örneği.
* Service Fabric hizmeti olarak dağıtılan dahili izleme örgütleri (örneğin, koşulları izleyen ve rapor veren bir Service Fabric stateless hizmeti). Watchdogs tüm düğümleri üzerinde dağıtılabilir veya izlenen hizmetaffinitized olabilir.
* Service Fabric düğümleri üzerinde çalışan ancak Service Fabric hizmetleri olarak uygulanmayan dahili watchdogs.
* Hizmeti Kumaş kümesi dışından kaynak probe dış watchdogs (örneğin, Gomez gibi izleme hizmeti).

Kutunun dışında, Service Fabric bileşenleri kümedeki tüm varlıklarda sistem durumunu bildirir. [Sistem sistem durumu raporları](service-fabric-understand-and-troubleshoot-with-system-health-reports.md) küme ve uygulama işlevselliği ve bayrak sorunları sağlık yoluyla görünürlük sağlar. Uygulamalar ve hizmetler için sistem sağlık raporları, varlıkların uygulandığını ve Hizmet Kumaşı çalışma süresi açısından doğru davrandığını doğrular. Raporlar, hizmetin iş mantığının herhangi bir sistem durumu izlemesini sağlamaz veya yanıt vermeyi durduran işlemleri algılamaz. Hizmetinizin mantığına özel sağlık bilgileri eklemek için hizmetlerinizde [özel sistem durumu raporlaması uygulayın.](service-fabric-report-health.md)

Service Fabric, sağlık deposunda toplanan [sistem durumu raporlarını görüntülemek](service-fabric-view-entities-aggregated-health.md) için birden çok yol sağlar:
* [Hizmet Kumaş Explorer](service-fabric-visualizing-your-cluster.md) veya diğer görselleştirme araçları.
* Sistem durumu sorguları [(PowerShell,](/powershell/module/ServiceFabric/) [CLI,](service-fabric-sfctl.md) [C# FabricClient API'leri](/dotnet/api/system.fabric.fabricclient.healthclient) ve [Java FabricClient API'leri](/java/api/system.fabric)veya [REST API'leri](/rest/api/servicefabric)aracılığıyla).
* Özelliklerden biri olarak sağlık durumu olan varlıkların listesini döndüren genel sorgular (PowerShell, CLI, API'ler veya REST aracılığıyla).

## <a name="monitoring-and-diagnostics"></a>İzleme ve tanılama
[İzleme ve tanılama,](service-fabric-diagnostics-overview.md) uygulamaların ve hizmetlerin herhangi bir ortamda geliştirilmesi, test edilmesi ve dağıtılması açısından çok önemlidir. Service Fabric çözümleri, uygulamaların ve hizmetlerin yerel geliştirme ortamında veya üretimde beklendiği gibi çalışmasını sağlamaya yardımcı olan izleme ve tanılamayı planlarken ve uygularken en iyi şekilde çalışır.

İzleme ve tanılamanın temel hedefleri şunlardır:

- Donanım ve altyapı sorunlarını algılama ve tanılama
- Yazılım ve uygulama sorunlarını algılama, hizmet inme süresini azaltma
- Kaynak tüketimini anlama ve operasyon kararlarını artırmaya yardımcı olun
- Uygulama, hizmet ve altyapı performansını optimize edin
- İş öngörüleri oluşturun ve iyileştirme alanlarını belirleyin
 
İzleme ve tanılamanın genel iş akışı üç adımdan oluşur:

1. Olay oluşturma: Bu altyapı (küme), platform ve uygulama / hizmet düzeyinde olaylar (günlükleri, izlemeler, özel olaylar) içerir
2. Olay toplama: oluşturulan olayların görüntülenmeden önce toplanması ve toplanması gerekir
3. Analiz: olayların gerektiğinde analiz ve görüntülenmesine olanak sağlamak için bazı biçimlerde görselleştirilmeve erişilebilir olması gerekir

Bu üç alanı kapsayan birden fazla ürün mevcuttur ve her biri için farklı teknolojiler seçmekte özgürdür. Daha fazla bilgi için [Azure Hizmet Kumaşı için İzleme ve tanılama](service-fabric-diagnostics-overview.md)'yı okuyun.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure’da küme](service-fabric-cluster-creation-via-portal.md) veya [Windows’ta tek başına küme](service-fabric-cluster-creation-for-windows-server.md) oluşturma hakkında bilgi edinin.
* [Reliable Services](service-fabric-reliable-services-quick-start.md) veya [Reliable Actors](service-fabric-reliable-actors-get-started.md) programlama modelini kullanan bir hizmet oluşturmayı deneyin.
* [Bulut Hizmetlerinden](service-fabric-cloud-services-migration-differences.md)nasıl geçiş yapılacağını öğrenin.
* Hizmetleri [izlemeyi ve tanılamayı](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)öğrenin. 
* [Uygulamalarınızı ve hizmetlerinizi test](service-fabric-testability-overview.md)etmeyi öğrenin.
* Küme [kaynaklarını yönetmeyi ve düzenlemeyi](service-fabric-cluster-resource-manager-introduction.md)öğrenin.
* [Service Fabric örneklerine](https://aka.ms/servicefabricsamples)bakın.
* Service [Fabric destek seçenekleri](service-fabric-support.md)hakkında bilgi edinin.
* Makaleler ve duyurular için [takım blogunu](https://blogs.msdn.microsoft.com/azureservicefabric/) okuyun.


[cluster-application-instances]: media/service-fabric-content-roadmap/cluster-application-instances.png
[cluster-imagestore-apptypes]: ./media/service-fabric-content-roadmap/cluster-imagestore-apptypes.png
