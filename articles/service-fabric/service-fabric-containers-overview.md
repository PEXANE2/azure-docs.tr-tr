---
title: Servis Kumaşı ve konteynerler genel bakış
description: Service Fabric'e genel bir bakış ve mikro hizmet uygulamalarını dağıtmak için kapların kullanımı. Bu makalede, kapsayıcıların nasıl kullanılabileceğine ve Hizmet Kumaşı'ndaki kullanılabilir yeteneklere genel bir bakış sunulmaktadır.
ms.topic: conceptual
ms.date: 8/8/2018
ms.openlocfilehash: 884cefa3d6a60f55269afac73c40b9f6b21518f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75458213"
---
# <a name="service-fabric-and-containers"></a>Servis Kumaş ve konteynerler

## <a name="introduction"></a>Giriş

Azure Service Fabric; ölçeklenebilir ve güvenilir mikro hizmetleri ve kapsayıcıları paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırmayı sağlayan bir dağıtılmış sistemler platformudur.

Service Fabric, microsoft'un mikro hizmetleri bir makine kümesi ne kadar dağıtan [konteyner orkestratörüdür.](service-fabric-cluster-resource-manager-introduction.md) Service Fabric, Microsoft'ta büyük ölçekte hizmet yürüttüğü yıllarda öğrenilen derslerden yararlanır.

Mikro hizmetler, [Service Fabric programlama modelleri](service-fabric-choose-framework.md), [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) kullanmaktan [tercih ettiğiniz herhangi bir kodun](service-fabric-guest-executables-introduction.md) dağıtılmasına kadar birçok yolla geliştirilebilir. Veya, sadece dağıtmak [ve konteyner yönetmek](service-fabric-containers-overview.md)istiyorsanız, Servis Kumaş da harika bir seçimdir.

Varsayılan olarak, Service Fabric bu hizmetleri işlem olarak dağıtır ve etkinleştirir. İşlemler, kümedeki kaynakların en hızlı etkinleştirme ve en yüksek yoğunluklu kullanımını sağlar. Service Fabric, kapsayıcı görüntülerinde de hizmetleri dağıtabilir. Aynı uygulamada, süreçlerdeki hizmetleri ve kapsayıcıdaki hizmetleri de karıştırabilirsiniz.

Hemen atlamak ve Servis Kumaşı'ndaki kapları denemek için hızlı bir başlangıç, öğretici veya örnek deneyin:  

[Quickstart: Service Fabric'e bir Linux konteyner uygulaması dağıtma](service-fabric-quickstart-containers-linux.md)  
[Quickstart: Service Fabric'e Windows kapsayıcı uygulaması dağıtma](service-fabric-quickstart-containers.md)  
[Mevcut bir .NET uygulamasını kapsayıcılı hale getirme](service-fabric-host-app-in-a-container.md)  
[Service Fabric Kapsayıcı Örnekleri](https://azure.microsoft.com/resources/samples/service-fabric-containers/)  

## <a name="what-are-containers"></a>Konteynerler nelerdir

Kapsayıcılar, uygulamanın çalışması için değişmez bir ortam sağlayarak uygulamaları farklı bilgi işlem ortamlarında güvenilir bir şekilde çalıştırma sorununu çözer. Kapsayıcılar bir uygulamayı ve kitaplıklar ve yapılandırma dosyaları gibi tüm bağımlılıklarını, yazılımı kapsayıcının içinde çalıştırmak için gereken her şeyi içeren kendi yalıtılmış 'kutusuna' sarar. Kapsayıcı nerede çalışırsa çalışırsa çalışırsa çalıştır, içindeki uygulama her zaman bağımlı kitaplıklarının doğru sürümleri, yapılandırma dosyaları ve çalışması gereken diğer şeyler gibi çalışması için gereken her şeye sahiptir.

Kapsayıcılar doğrudan çekirdeğin üstünde çalışır ve dosya sisteminin ve diğer kaynakların yalıtılmış görünümüne sahiptir. Bir kapsayıcıdaki bir uygulamanın, kapsayıcının dışındaki diğer uygulamalar veya işlemler hakkında hiçbir bilgisi yoktur. Her uygulama ve çalışma süresi, bağımlılıklar ve sistem kitaplıkları, kapsayıcının kendi işletim sisteminin yalıtılmış görünümüne tam, özel erişim sağlayan bir kapsayıcının içinde çalışır. Uygulamanızın tüm bağımlılıklarını sağlamayı kolaylaştırmanın yanı sıra, farklı bilgi işlem ortamlarında çalışması için gereken güvenlik ve kaynak yalıtımı, hizmet veren bir ortamda hizmet çalıştıran Service Fabric ile kapsayıcıkullanmanın önemli yararlarıdır. Işlem.

Sanal makinelerle karşılaştırıldığında, konteynerlerin aşağıdaki avantajları vardır:

* **Küçük**: Kapsayıcılar verimliliği artırmak için tek bir depolama alanı ve katman sürümleri ve güncelleştirmeleri kullanır.
* **Hızlı**: Kapsayıcıların tüm işletim sistemini önyüklemesi gerekmez, böylece çok daha hızlı başlayabilirler-- genellikle saniyeler içinde.
* **Taşınabilirlik**: Konteynerleştirilmiş bir uygulama görüntüsü bulutta, şirket içinde, sanal makinelerin içinde veya doğrudan fiziksel makinelerde çalışacak şekilde taşınabilir.
* **Kaynak yönetimi**: Kapsayıcı, ana bilgisayarda tüketebileceği fiziksel kaynakları sınırlayabilir.

### <a name="container-types-and-supported-environments"></a>Konteyner tipleri ve desteklenen ortamlar

Service Fabric hem Linux hem de Windows'daki kapsayıcıları destekler ve Windows'da Hyper-V yalıtım modunu destekler.

#### <a name="docker-containers-on-linux"></a>Linux üzerinde Docker konteynerler

Docker, Linux çekirdek kaplarının üstünde kapsayıcıoluşturmak ve yönetmek için API'ler sağlar. Docker Hub, kapsayıcı görüntülerini depolamak ve almak için merkezi bir depo sağlar.
Linux tabanlı bir öğretici için, [Linux'taki ilk Service Fabric konteyner uygulamanızı oluşturun'a](service-fabric-get-started-containers-linux.md)bakın.

#### <a name="windows-server-containers"></a>Windows Server kapsayıcıları

Windows Server 2016, yalıtım düzeyine göre farklılık gösteren iki farklı kapsayıcı türü sağlar. Windows Server kapsayıcıları ve Docker kapsayıcıları benzer, çünkü her ikisi de ad alanı ve dosya sistemi yalıtımına sahipken, çekirdeği üzerinde çalıştıkları ana bilgisayarla paylaşır. Linux'ta bu yalıtım geleneksel olarak cgroups ve ad alanları tarafından sağlanmıştır ve Windows Server kapsayıcıları da benzer şekilde şekilde kullanılır.

Hyper-V desteğine sahip Windows kapsayıcıları, işletim sistemi çekirdeğini başka bir kapsayıcıyla veya ana bilgisayarla paylaşmadığından daha fazla yalıtım ve güvenlik sağlar. Bu yüksek güvenlik yalıtımı düzeyiyle, Hyper-V etkinleştirilmiş kapsayıcılar potansiyel olarak düşmanca, çok kiracılı senaryoları hedefalır.
Windows tabanlı bir öğretici için [windows'da ilk Service Fabric kapsayıcı uygulamanızı oluşturun'a](service-fabric-get-started-containers.md)bakın.

Aşağıdaki şekilde, kullanılabilir farklı türde sanallaştırma ve yalıtım düzeyleri gösterilmektedir.
![Service Fabric platformu][Image1]

## <a name="scenarios-for-using-containers"></a>Kapsayıcıları kullanmak için senaryolar

Bir kapsayıcının iyi bir seçim olduğu tipik örnekler şunlardır:

* **IIS lift and shift**: Varolan bir [ASP.NET MVC](https://www.asp.net/mvc) uygulamasını ASP.NET Core'a geçirmek yerine bir konteynerin içine koyabilirsiniz. Bu ASP.NET MVC uygulamaları Internet Information Services 'e (IIS) bağlıdır. Bu uygulamaları önceden oluşturulmuş IIS görüntüsünden konteyner görüntülerine paketleyebilir ve Service Fabric ile dağıtabilirsiniz. Windows kapsayıcıları hakkında bilgi için [Windows Server'daki Kapsayıcı Resimleri'ne](https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-server) bakın.

* **Konteynerleri ve Servis Kumaşı mikrohizmetlerini karıştırın**: Uygulamanızın bir parçası için mevcut bir konteyner görüntüsü kullanın. Örneğin, uygulamanızın web ön ucu için [NGINX kapsayıcısını](https://hub.docker.com/_/nginx/) ve daha yoğun arka uç hesaplaması için devlet hizmetleri için kullanabilirsiniz.

* **"Gürültülü komşular" hizmetlerinin etkisini azaltma**: Bir hizmetin ana bilgisayarda kullandığı kaynakları kısıtlamak için kapsayıcıların kaynak yönetim yeteneğini kullanabilirsiniz. Hizmetler çok sayıda kaynak tüketiyorsa ve başkalarının performansını (uzun süren, sorgu benzeri bir işlem gibi) etkileyebilseydi, bu hizmetleri kaynak yönetimi olan kapsayıcılara koymayı düşünün.

## <a name="service-fabric-support-for-containers"></a>Kapsayıcılar için Service Fabric desteği

Service Fabric, Hyper-V yalıtım modu desteğiyle birlikte Linux'ta Docker kapsayıcılarının ve Windows Server kapsayıcılarının Windows Server 2016'da dağıtımını destekler. 

Service Fabric, bir kapsayıcının birden çok hizmet yinelemesi yerleştirilen bir uygulama ana bilgisayarına sahip olduğu bir [uygulama modeli](service-fabric-application-model.md) sağlar. Service Fabric ayrıca yerleşik Service Fabric programlama modellerini kullanmadığınız, bunun yerine herhangi bir dil veya çerçeve kullanılarak yazılmış varolan bir uygulamayı bir kapsayıcının içinde paketlediğiniz [bir konuk çalıştırılabilir senaryoyu](service-fabric-guest-executables-introduction.md) da destekler. Bu senaryo kapsayıcılar için ortak kullanım örneğidir.

Ayrıca bir [konteyner içinde Service Fabric hizmetleri](service-fabric-services-inside-containers.md)çalıştırabilirsiniz. Konteynerler içinde Service Fabric hizmetlerinin çalıştırılamı için destek şu anda sınırlıdır.

Service Fabric, konteynerleştirilmiş mikro hizmetlerden oluşan uygulamalar oluşturmanıza yardımcı olan çeşitli konteyner özellikleri sağlar:

* Kapsayıcı görüntü dağıtımı ve etkinleştirme.
* Azure kümelerinde varsayılan olarak kaynak değerlerini ayarlamayı da içeren kaynak yönetimi.
* Depo kimlik doğrulaması.
* Bağlantı noktası eşleme barındırmak için konteyner bağlantı noktası.
* Konteynerden konteynere keşif ve iletişim.
* Çevre değişkenlerini yapılandırma ve ayarlama becerisi.
* Kapsayıcıda güvenlik kimlik bilgilerini ayarlama yeteneği.
* Kapsayıcılar için farklı ağ modları seçeneği.

Azure'daki kapsayıcı desteğine kapsamlı bir genel bakış için (Örneğin, Azure Kubernetes Hizmeti ile Nasıl Bir Kubernetes kümesi oluşturulacak, Azure Kapsayıcı Kayıt Defteri'nde özel bir Docker kayıt defteri nasıl oluşturulur ve daha fazlası [için Kapsayıcılar için Azure'a](https://docs.microsoft.com/azure/containers/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Hizmet Kumaşı'nın kapsayıcıların çalıştırılamıriçin sağladığı destek hakkında bilgi edinilmişsinizdir. Daha sonra, bunları nasıl kullanacağınızı göstermek için her bir özelliğin örneklerini ele alacağız.

[Linux üzerinde ilk Service Fabric kapsayıcı uygulamanızı oluşturma](service-fabric-get-started-containers-linux.md)  
[Windows üzerinde ilk Service Fabric kapsayıcı uygulamanızı oluşturma](service-fabric-get-started-containers.md)  
[Windows Kapsayıcıları hakkında daha fazla bilgi edinin](https://docs.microsoft.com/virtualization/windowscontainers/about/)

[Image1]: media/service-fabric-containers/Service-Fabric-Types-of-Isolation.png