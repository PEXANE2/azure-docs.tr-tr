---
title: Azure Hizmet Kumaş terminolojisi öğrenin
description: Belgelerin geri kalanında kullanılan anahtar Hizmet Kumaş terminolojisi ve kavramları öğrenin.
author: masnider
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: masnider
ms.custom: sfrev
ms.openlocfilehash: a9266c2a8d2ad179cfdb12e367a14f37d1abc9b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258245"
---
# <a name="service-fabric-terminology-overview"></a>Hizmet Kumaş terminolojisi genel bakış

Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırmayı sağlayan bir dağıtılmış sistemler platformudur.  Service [Fabric kümelerini her yerde barındırabilirsiniz](service-fabric-deploy-anywhere.md): Azure, şirket içi veri merkezinde veya herhangi bir bulut sağlayıcısında.  Service Fabric, Azure Servis [Kumaş Örgü'ye](/azure/service-fabric-mesh)güç veren orkestratördür. Hizmetlerinizi yazmak ve uygulamayı birden çok ortam seçeneğinden çalıştıracağınızı seçmek için istediğiniz çerçeveyi kullanabilirsiniz. Bu makalede, belgelerde kullanılan terimleri anlamak için Service Fabric tarafından kullanılan terminoloji ayrıntılı olarak anlatılmıştır.

## <a name="infrastructure-concepts"></a>Altyapı kavramları

**Küme**: Mikro hizmetlerinizin dağıtıldığı ve yönetildiği ağa bağlı sanal veya fiziksel makineler kümesidir.  Kümeler binlerce makine içerecek şekilde ölçeklendirilebilir.

**Düğüm**: Kümenin bir parçası olan bir makine ye veya *VM'ye düğüm*denir. Her düğüme bir düğüm adı (dize) atanır. Düğümlerin yerleşim özellikleri gibi özellikleri vardır. Her makine veya VM otomatik başlatma `FabricHost.exe`Windows hizmeti vardır, önyükleme üzerine çalışmaya `Fabric.exe` başlar `FabricGateway.exe`ve sonra iki çalıştırılabilir başlar: ve . Bu iki yürütülebilir düğüm makyaj. Senaryoları test etmek için, birden çok örneği çalıştırarak tek bir makinede `Fabric.exe` `FabricGateway.exe`veya VM'de birden çok düğüm barındırabilirsiniz.

## <a name="application-and-service-concepts"></a>Uygulama ve hizmet kavramları

**Servis Kumaş Örgü Uygulaması**: Servis Kumaşı Örgü Uygulamaları Kaynak Modeli (YAML ve JSON kaynak dosyaları) tarafından tanımlanır ve Servis Kumaşı'nın çalıştığı herhangi bir ortama dağıtılabilir.

**Hizmet Kumaş**ıSlama Uygulaması : Service Fabric Native Applications, Native Application Model (XML tabanlı uygulama ve hizmet beyannameleri) ile açıklanmaktadır.  Servis Kumaş Yerli Uygulamaları Hizmet Kumaş Örgü çalıştırılamaz.

### <a name="service-fabric-mesh-application-concepts"></a>Servis Kumaş Örgü Uygulama konseptleri

**Uygulama**: Bir mesh uygulamasının dağıtım, sürüm ve kullanım ömrü birimidir. Her uygulama örneğinin yaşam döngüsü bağımsız olarak yönetilebilir.  Uygulamalar bir veya daha fazla servis kodu paketi ve ayarlarından oluşur. Bir uygulama Azure Kaynak Modeli (RM) şeması kullanılarak tanımlanır.  Hizmetler, RM şablonundaki uygulama kaynağının özellikleri olarak tanımlanır.  Uygulama tarafından kullanılan ağlar ve birimler uygulama tarafından başvurulmaktadır.  Bir uygulama oluştururken, uygulama, hizmet(ler), ağ ve birim(ler) Service Fabric Resource Model kullanılarak modellenir.

**Hizmet**: Bir uygulamadaki hizmet bir microservice temsil eder ve tam ve bağımsız bir işlevi gerçekleştirir. Her hizmet, kod paketiyle ilişkili kapsayıcı görüntüsünü çalıştırmak için gereken her şeyi açıklayan bir veya daha fazla kod paketinden oluşur.  Bir uygulamadaki hizmet sayısı yukarı ve aşağı ölçeklendirilebilir.

**Ağ**: Ağ kaynağı uygulamalarınız için özel bir ağ oluşturur ve bu ağ için başvurabilecek uygulamalardan veya hizmetlerden bağımsızdır. Farklı uygulamalardan gelen birden çok hizmet aynı ağın parçası olabilir. Ağlar, uygulamalar tarafından başvurulan dağıtılabilir kaynaklardır.

**Kod paketi**: Kod paketleri, kod paketiyle ilişkili kapsayıcı görüntüsünü çalıştırmak için gereken her şeyi açıklar:

* Kapsayıcı adı, sürümü ve kayıt defteri
* Her kapsayıcı için gerekli CPU ve bellek kaynakları
* Ağ uç noktaları
* Ayrı bir birim kaynağa atıfta bulunarak, kapsayıcıya monte edilen birimler.

Uygulama kaynağının bir parçası olarak tanımlanan tüm kod paketleri dağıtılır ve grup olarak birlikte etkinleştirilir.

Birim : **Birimler,** durumu sürdürmek için kullanabileceğiniz kapsayıcı örneklerinin içine monte edilmiş dizinlerdir. Azure Dosyaları toplu sürücü, bir Azure Files paylaşımını bir kapsayıcıya bağlar ve dosya depolamayı destekleyen tüm API'ler aracılığıyla güvenilir veri depolama sağlar. Birimler, uygulamalar tarafından başvurulan dağıtılabilir kaynaklardır.

### <a name="service-fabric-native-application-concepts"></a>Hizmet Kumaş Yerli Uygulama kavramları

**Uygulama**: Uygulama, belirli bir işlevi veya işlevleri gerçekleştiren kurucu hizmetler topluluğudur. Her uygulama örneğinin yaşam döngüsü bağımsız olarak yönetilebilir.

**Hizmet**: Bir hizmet tam ve bağımsız bir işlev icra eder ve diğer hizmetlerden bağımsız olarak başlayıp çalıştırılabilir. Hizmet kod, yapılandırma ve verilerden oluşur. Her hizmet için kod yürütülebilir ikililerden oluşur, yapılandırma çalışma zamanında yüklenebilen hizmet ayarlarından oluşur ve veriler hizmet tarafından tüketilecek rasgele statik verilerden oluşur.

**Uygulama türü**: Hizmet türleri koleksiyonuna atanan ad/sürüm. Bir `ApplicationManifest.xml` dosyada tanımlanır ve bir uygulama paketi dizinine katıştırılır. Dizin daha sonra Service Fabric kümesinin görüntü deposuna kopyalanır. Daha sonra küme içinde bu uygulama türünden adlandırılmış bir uygulama oluşturabilirsiniz.

Daha fazla bilgi için [Uygulama modeli](service-fabric-application-model.md) makalesini okuyun.

**Uygulama paketi**: Uygulama türünün `ApplicationManifest.xml` dosyasını içeren bir disk dizini. Uygulama türünü oluşturan her hizmet türü için hizmet paketlerine başvurur. Uygulama paketi dizinindeki dosyalar Service Fabric cluster'ın resim deposuna kopyalanır. Örneğin, bir e-posta uygulama türü için bir uygulama paketi, bir sıra hizmet paketine, bir ön uç hizmet paketine ve veritabanı hizmet paketine başvurular içerebilir.

**Adlandırılmış uygulama**: Bir uygulama paketini görüntü deposuna kopyaladıktan sonra, küme içindeki uygulamanın bir örneğini oluşturursunuz. Uygulama paketinin uygulama türünü belirtirken, adını veya sürümünü kullanarak bir örnek oluşturursunuz. Her uygulama türü örneği gibi görünen bir tek düzen kaynak tanımlayıcısı `"fabric:/MyNamedApp"`(URI) adı atanır: . Bir küme içinde, tek bir uygulama türünden birden çok adlandırılmış uygulama oluşturabilirsiniz. Farklı uygulama türlerinden adlandırılmış uygulamalar da oluşturabilirsiniz. Adlandırılmış her uygulama yönetilir ve bağımsız olarak sürülür.

**Hizmet türü**: Bir hizmetin kod paketlerine, veri paketlerine ve yapılandırma paketlerine atanan ad/sürüm. Servis türü `ServiceManifest.xml` dosyada tanımlanır ve bir hizmet paketi dizinine katıştırılır. Servis paketi dizini daha sonra bir uygulama `ApplicationManifest.xml` paketinin dosyasına başvurulur. Küme içinde, adlandırılmış bir uygulama oluşturduktan sonra, uygulama türünün hizmet türlerinden birinden adlandırılmış bir hizmet oluşturabilirsiniz. Hizmet türü `ServiceManifest.xml` dosyası hizmeti açıklar.

Daha fazla bilgi için [Uygulama modeli](service-fabric-application-model.md) makalesini okuyun.

İki tür hizmet vardır:

* Stateless : Hizmetin kalıcı durumu Azure Depolama, Azure SQL Veritabanı veya Azure Cosmos DB gibi harici bir depolama hizmetinde depolandığında, **kullanamaz**bir hizmet kullanın. Hizmetin kalıcı depolama alanı yoksa, devletsiz bir hizmet kullanın. Örneğin, değerlerin hizmete aktarıldığı bir hesap makinesi hizmeti için, bu değerleri kullanan bir hesaplama gerçekleştirilir ve sonra bir sonuç döndürülür.
* **Stateful**: Servis Kumaşı'nın Güvenilir Koleksiyonları veya Güvenilir Aktörler programlama modelleri aracılığıyla hizmetinizin durumunu yönetmesini istediğinizde devletli bir hizmet kullanın. Adlandırılmış bir hizmet oluşturduğunuzda, ölçeklenebilirlik için durumunuzu kaç bölüme yaymak istediğinizi belirtin. Ayrıca, güvenilirlik için durumunuzu düğümler arasında kaç kez kopyalayacaklarını da belirtin. Adlandırılmış her hizmetin tek bir birincil yinelemesi ve birden çok ikincil yinelemesi vardır. Birincil yinelemeye yazarken adlandırılmış hizmetin durumunu değiştirirsiniz. Service Fabric daha sonra durumunuzu eşit tutmak için bu durumu tüm ikincil yinelemelere çoğaltır. Hizmet Kumaşı, birincil yineleme başarısız olduğunda otomatik olarak algılar ve varolan ikincil bir yinelemeyi birincil yinelemeye teşvik eder. Service Fabric sonra yeni bir ikincil yineleme oluşturur.  

**Yinelemeler veya örnekler,** dağıtılan ve çalışan bir hizmetin koduna (ve devlet hizmetleri için durum durumuna) başvurur. Bkz. [Yinelemeler ve örnekler.](service-fabric-concepts-replica-lifecycle.md)

**Yeniden yapılandırma,** bir hizmetin yineleme kümesindeki herhangi bir değişikliğin işlemini ifade eder. Bkz. [Yeniden Yapılandırma.](service-fabric-concepts-reconfiguration.md)

**Hizmet paketi**: Hizmet türünün `ServiceManifest.xml` dosyasını içeren bir disk dizini. Bu dosya, hizmet türü için kod, statik veri ve yapılandırma paketlerine başvurur. Hizmet paketi dizinindeki dosyalara başvuru türü `ApplicationManifest.xml` dosyasına başvurulur. Örneğin, bir hizmet paketi, veritabanı hizmetini oluşturan kod, statik veri ve yapılandırma paketlerine başvurabilir.

**Adlandırılmış hizmet**: Adlandırılmış bir uygulama oluşturduktan sonra, küme içindeki hizmet türlerinden birinin örneğini oluşturabilirsiniz. Adını/sürümünü kullanarak hizmet türünü belirtirsiniz. Her hizmet türü örneği, adlandırılmış uygulamanın URI altında kapsamlı bir URI adı atanır. Örneğin, "MyNamedApp" adlı bir uygulama içinde "MyDatabase" adlı bir hizmet `"fabric:/MyNamedApp/MyDatabase"`oluşturursanız, URI aşağıdaki gibi görünür: . Adlandırılmış bir uygulama içinde, birkaç adlandırılmış hizmet oluşturabilirsiniz. Adlandırılmış her hizmetin kendi bölüm düzeni ve örnek veya yineleme sayıları olabilir.

**Kod paketi**: Genellikle EXE/DLL dosyaları olan hizmet türünün yürütülebilir dosyalarını içeren bir disk dizini. Kod paketi dizinindeki dosyalara servis türü `ServiceManifest.xml` dosyası tarafından başvurulur. Adlandırılmış bir hizmet oluşturduğunuzda, kod paketi adlandırılmış hizmeti çalıştırmak için seçilen düğüm veya düğümlere kopyalanır. Sonra kod çalıştırmaya başlar. İki tür kod paketi yürütülebilir vardır:

* **Konuk çalıştırılabilir :** Ana bilgisayar işletim sisteminde (Windows veya Linux) olduğu gibi çalıştırılabilir ler. Bu yürütülebilir uygulamalar herhangi bir Service Fabric çalışma zamanı dosyasına bağlantı vermez veya başvuruyapmaz ve bu nedenle herhangi bir Service Fabric programlama modeli kullanmaz. Bu yürütülebilir ler, bitiş noktası bulma hizmeti gibi bazı Hizmet Dokusu özelliklerini kullanamaz. Konuk çalıştırılabilirler, her hizmet örneğine özgü yük ölçümlerini bildiremez.
* **Servis ana bilgisayar icra edilebilirleri**: Service Fabric çalışma zamanı dosyalarına bağlanarak Service Fabric programlama modellerini kullanan ve Service Fabric özelliklerini etkinleştiren yürütülebilir ler. Örneğin, adlandırılmış bir hizmet örneği Hizmet Kumaşı'nın Adlandırma Hizmeti'ne uç noktaları kaydedebilir ve yük ölçümlerini de bildirebilir.

**Veri paketi**: Hizmet türünün statik, salt okunur veri dosyalarını, genellikle fotoğraf, ses ve video dosyalarını içeren bir disk dizini. Veri paketi dizinindeki dosyalara hizmet türü `ServiceManifest.xml` dosyası tarafından başvurulur. Adlandırılmış bir hizmet oluşturduğunuzda, veri paketi adlandırılmış hizmeti çalıştırmak için seçilen düğüm veya düğümlere kopyalanır. Kod çalışmaya başlar ve artık veri dosyalarına erişebilir.

**Yapılandırma paketi**: Hizmet türünün statik, salt okunur yapılandırma dosyalarını, genellikle metin dosyalarını içeren bir disk dizini. Yapılandırma paketi dizinindeki dosyalara hizmet türü `ServiceManifest.xml` dosyası tarafından başvurulur. Adlandırılmış bir hizmet oluşturduğunuzda, yapılandırma paketindeki dosyalar, adlandırılmış hizmeti çalıştırmak için seçilen bir veya daha fazla düğüme kopyalanır. Sonra kod çalıştırmaya başlar ve artık yapılandırma dosyalarına erişebilir.

**Kapsayıcılar**: Varsayılan olarak, Service Fabric hizmetleri işlem olarak dağıtır ve etkinleştirir. Service Fabric, kapsayıcı görüntülerinde de hizmetleri dağıtabilir. Kapsayıcılar, temel işletim sistemini uygulamalardan soyutlayan bir sanallaştırma teknolojisidir. Uygulama ve çalışma zamanı, bağımlılıklar ve sistem kitaplıkları bir kapsayıcının içinde çalışır. Kapsayıcı, kapsayıcının işletim sistemi yapılarının kendi izole görünümüne tam ve özel erişime sahiptir. Service Fabric, Linux'taki Windows Server kapsayıcılarını ve Docker kapsayıcılarını destekler. Daha fazla bilgi için [Servis Kumaşı ve kapları](service-fabric-containers-overview.md)okuyun.

**Bölüm düzeni**: Adlandırılmış bir hizmet oluşturduğunuzda, bir bölüm düzeni belirtirsiniz. Önemli miktarda durum içeren hizmetler, verileri bölümlere böler ve bu da durumu kümenin düğümlerine yayır. Verileri bölümler arasında bölerek, adlandırılmış hizmetin durumu ölçeklenebilir. Bir bölüm içinde, adsız adlandırılmış hizmetlerin örnekleri, stateful adlandırılmış hizmetlerin yinelemeleri ise vardır. Genellikle, iç durumları olmadığından, adsız adlandırılmış hizmetlerin yalnızca bir bölümü yoktur. Bölüm örnekleri kullanılabilirlik sağlar. Bir örnek başarısız olursa, diğer örnekler normal çalışmaya devam eder ve ardından Service Fabric yeni bir örnek oluşturur. Stateful adlı hizmetler yinelemeler içinde durumlarını korumak ve durum eşittutulur, böylece her bölümün kendi yineleme kümesi vardır. Bir yineleme başarısız olursa, Hizmet Kumaşı varolan yinelemelerden yeni bir yineleme oluşturur.

Daha fazla bilgi için [Bölüm Hizmeti Kumaş güvenilir hizmetler](service-fabric-concepts-partitioning.md) makalesini okuyun.

## <a name="system-services"></a>Sistem hizmetleri

Hizmet Kumaşı'nın platform özelliklerini sağlayan her kümede oluşturulan sistem hizmetleri vardır.

**Adlandırma Hizmeti**: Her Hizmet Kumaşı kümesinde, hizmet adlarını kümedeki bir konuma göre çözen bir Adlandırma Hizmeti vardır. Küme için bir internet Alan Adı Sistemi (DNS) gibi hizmet adlarını ve özelliklerini yönetirsiniz. İstemciler, bir hizmet adını ve konumunu çözmek için Adlandırma Hizmeti'ni kullanarak kümedeki herhangi bir düğümle güvenli bir şekilde iletişim kurar. Uygulamalar küme içinde hareket eder. Örneğin, bunun nedeni hatalar, kaynak dengelemeveya kümenin yeniden boyutlandırılması olabilir. Geçerli ağ konumunu çözen hizmetler ve istemciler geliştirebilirsiniz. İstemciler, şu anda çalıştığı gerçek makine IP adresini ve bağlantı noktasını elde eder.

Adlandırma Hizmeti ile çalışan istemci ve hizmet iletişimi API'leri hakkında daha fazla bilgi için [hizmetlerle iletişim](service-fabric-connect-and-communicate-with-services.md) iletişimi'ni okuyun.

**Resim Deposu hizmeti**: Her Service Fabric kümesinde dağıtılan, sürümlü uygulama paketlerinin tutulduğu bir Image Store hizmeti vardır. Bir uygulama paketini Resim Mağazası'na kopyalayın ve ardından bu uygulama paketinde bulunan uygulama türünü kaydedin. Uygulama türü sağlandıktan sonra, ondan adlandırılmış bir uygulama oluşturursunuz. Bir uygulama türünü, adlandırılmış tüm uygulamaları silindikten sonra Görüntü Deposu hizmetinden silebilirsiniz.

Image Store hizmeti hakkında daha fazla bilgi için [ImageStoreConnectionString ayarını anlayın.](service-fabric-image-store-connection-string.md)

Uygulamaları Image Store hizmetine dağıtma hakkında daha fazla bilgi için bir uygulama makalesini [dağıt'ı](service-fabric-deploy-remove-applications.md) okuyun.

**Failover Manager hizmeti**: Her Service Fabric kümesinde aşağıdaki eylemlerden sorumlu bir Failover Manager hizmeti vardır:

 - Hizmetlerin yüksek kullanılabilirliği ve tutarlılığı ile ilgili işlevleri gerçekleştirir.
 - Uygulamayı ve küme yükseltmelerini yönetir.
 - Diğer sistem bileşenleriyle etkileşime geçer.

**Onarım Yöneticisi hizmeti**: Bir kümeüzerinde güvenli, otomatikleştirilebilir ve saydam bir şekilde onarım eylemlerinin gerçekleştirilmesine olanak tanıyan isteğe bağlı bir sistem hizmetidir. Onarım yöneticisi kullanılır:

   - Gümüş ve Altın [dayanıklılık](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) Azure Servis Kumaş kümelerinde Azure bakım onarımları gerçekleştirme.
   - [Patch Orkestrasyon Uygulaması](service-fabric-patch-orchestration-application.md) için onarım işlemlerinin gerçekleştirilmesi

## <a name="deployment-and-application-models"></a>Dağıtım ve uygulama modelleri

Hizmetlerinizi dağıtmak için, bunların nasıl çalışması gerektiğini açıklamanız gerekir. Service Fabric üç farklı dağıtım modelini destekler:

### <a name="resource-model-preview"></a>Kaynak modeli (önizleme)

Hizmet Kumaş Kaynakları, Service Fabric'e ayrı ayrı dağıtılabilen her şeydir; uygulamalar, hizmetler, ağlar ve birimler de dahil olmak üzere. Kaynaklar, küme bitiş noktasına dağıtılabilen bir JSON dosyası kullanılarak tanımlanır.  Hizmet Kumaş Kafesi için Azure Kaynak Modeli şeması kullanılır. Bir YAML dosya şeması da daha kolay tanım dosyaları yazar için kullanılabilir. Kaynaklar Service Fabric'in çalıştığı her yerde dağıtılabilir. Kaynak modeli, Service Fabric uygulamalarınızı tanımlamanın en basit yoludur. Ana odak noktası, konteynerize hizmetlerin basit dağıtımı ve yönetimidir. Daha fazla bilgi için [Hizmet Kumaş Kaynak Modeline Giriş 'i](/azure/service-fabric-mesh/service-fabric-mesh-service-fabric-resources)okuyun.

### <a name="native-model"></a>Yerli model

Yerel uygulama modeli, uygulamalarınıza Hizmet Kumaşı'na tam düşük seviyeli erişim sağlar. Uygulamalar ve hizmetler XML bildirim dosyalarında kayıtlı türler olarak tanımlanır.

Yerel model, C# ve Java'daki Servis Dokusu çalışma zamanı API'lerine ve küme yönetimi API'lerine erişim sağlayan Güvenilir Hizmetler ve Güvenilir Aktörler çerçevelerini destekler. Yerel model de rasgele kapsayıcılar ve yürütülebilir destekler. Yerel model [Servis Kumaş Örgü ortamında](/azure/service-fabric-mesh/service-fabric-mesh-overview)desteklenmez.

**Güvenilir Hizmetler**: Devletsiz ve devlethizmeti oluşturmak için bir API. Durum lu hizmetler durumlarını sözlük veya sıra gibi Güvenilir Koleksiyonlarda saklar. Ayrıca, Web API ve Windows Communication Foundation (WCF) gibi çeşitli iletişim yığınlarını da takabilirsiniz.

**Güvenilir Aktörler**: Sanal Aktör programlama modeli aracılığıyla durumsuz ve durumlu nesneler oluşturmak için bir API. Bu model, çok sayıda bağımsız hesaplama veya durum birimi olduğunda yararlıdır. Bu model sıra tabanlı iş parçacığı modeli kullanır, bu nedenle tek tek bir aktör tüm giden istekleri tamamlanana kadar diğer gelen istekleri işleyemediğinden, diğer aktörlere veya hizmetlere sesgetiren kodlardan kaçınmak en iyisidir.

Mevcut uygulamalarınızı Service Fabric'te de çalıştırabilirsiniz:

**Kapsayıcılar**: Service Fabric, Hyper-V yalıtım modu desteğiyle birlikte Windows Server 2016'da Linux ve Windows Server kapsayıcılarında Docker kapsayıcılarının dağıtımını destekler. Service Fabric [uygulama modelinde,](service-fabric-application-model.md)bir kapsayıcı birden çok hizmet yinelemesinin yerleştirildiği bir uygulama ana bilgisayarını temsil eder. Service Fabric herhangi bir kapsayıcıyı çalıştırabilir ve senaryo, varolan bir uygulamayı bir kapsayıcının içinde paketlediğiniz konuk çalıştırılabilir senaryoya benzer. Buna ek olarak, [konteyner içinde De Servis Kumaş hizmetleri çalıştırabilirsiniz.](service-fabric-services-inside-containers.md)

**Konuk çalıştırılabilir ler**: Azure Hizmet Kumaşı'nda Node.js, Python, Java veya C++ gibi her türlü kodu hizmet olarak çalıştırabilirsiniz. Service Fabric, bu tür hizmetleri, devletsiz hizmet olarak kabul edilen konuk yürütülebilir hizmetler olarak ifade eder. Service Fabric kümesinde çalıştırılabilir bir konuğu çalıştırmanın avantajları arasında yüksek kullanılabilirlik, sistem durumu izleme, uygulama yaşam döngüsü yönetimi, yüksek yoğunluk ve bulunabilirlik yer almaktadır.

Daha fazla bilgi [için hizmet makaleniz için bir programlama modeli seçin'i](service-fabric-choose-framework.md) okuyun.

### <a name="docker-compose"></a>Docker Oluştur 

[Docker Compose,](https://docs.docker.com/compose/) Docker projesinin bir parçasıdır. Service [Fabric, Docker Compose modelini kullanarak uygulamaları dağıtmak](service-fabric-docker-compose.md)için sınırlı destek sağlar.

## <a name="environments"></a>Ortamlar

Service Fabric, birçok farklı hizmet ve ürünün dayandığı açık kaynak kodlu bir platform teknolojisidir. Microsoft aşağıdaki seçenekleri sağlar:

 - **Azure Hizmet Kumaş Kafesi**: Microsoft Azure'da Service Fabric uygulamalarını çalıştırmak için tam olarak yönetilen bir hizmettir.
 - **Azure Hizmet Kumaşı**: Azure barındırılan Hizmet Kumaşı küme teklifi. Service Fabric ve Azure altyapısı arasında tümleştirmenin yanı sıra Service Fabric kümelerinin yükseltme ve yapılandırma yönetimini sağlar.
 - **Hizmet Kumaşı tek başına**: Service [Fabric kümelerini her yerde](/azure/service-fabric/service-fabric-deploy-anywhere) (şirket içinde veya herhangi bir bulut sağlayıcısında) dağıtmak için bir dizi yükleme ve yapılandırma aracı. Azure tarafından yönetilmez.
 - **Service Fabric geliştirme kümesi**: Service Fabric uygulamalarının geliştirilmesi için Windows, Linux veya Mac'te yerel bir geliştirme deneyimi sağlar.

## <a name="environment-framework-and-deployment-model-support-matrix"></a>Çevre, çerçeve ve dağıtım modeli destek matrisi

Farklı ortamlar, çerçeveler ve dağıtım modelleri için farklı destek düzeylerine sahiptir. Aşağıdaki tabloda desteklenen çerçeve ve dağıtım modeli birleşimleri açıklanmaktadır.

| Uygulama Türü | Tarafından açıklanan | Azure Servis Kumaş Örgü | Azure Hizmet Kumaş Kümeleri (herhangi bir işletim sistemi)| Yerel küme | Tek başına küme |
|---|---|---|---|---|---|
| Servis Kumaş Örgü Uygulamaları | Kaynak Modeli (YAML & JSON) | Destekleniyor |Desteklenmiyor | Windows destekli, Linux ve Mac- desteklenmez | Windows- desteklenmiyor |
|Servis Kumaş ı Yerli Uygulamalar | Yerel Uygulama Modeli (XML) | Desteklenmiyor| Destekleniyor|Destekleniyor|Windows destekli|

Aşağıdaki tabloda farklı uygulama modelleri ve Servis Kumaş karşı onlar için var olan takım açıklanır.

| Uygulama Türü | Tarafından açıklanan | Visual Studio | Eclipse | SFCTL | AZ CLI | PowerShell|
|---|---|---|---|---|---|---|
| Servis Kumaş Örgü Uygulamaları | Kaynak Modeli (YAML & JSON) | VS 2017 |Desteklenmiyor |Desteklenmiyor | Desteklenen - Yalnızca Kafes ortamı | Desteklenmiyor|
|Servis Kumaş ı Yerli Uygulamalar | Yerel Uygulama Modeli (XML) | VS 2017 ve VS 2015| Destekleniyor|Destekleniyor|Destekleniyor|Destekleniyor|

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric hakkında daha fazla bilgi edinmek için:

* [Service Fabric’e Genel Bakış](service-fabric-overview.md)
* [Uygulamaları oluşturmak için neden mikro hizmetler yaklaşımı öneriliyor?](service-fabric-overview-microservices.md)
* [Uygulama senaryoları](service-fabric-application-scenarios.md)

Service Fabric Mesh hakkında daha fazla bilgi için:

* [Servis Kumaş Örgü genel bakış](/azure/service-fabric-mesh/service-fabric-mesh-overview)
