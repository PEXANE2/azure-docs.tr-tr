---
title: Service Fabric programlama modeline genel bakış
description: 'Service Fabric, bina hizmetleri için iki çerçeve sunar: aktör çerçevesi ve hizmetler çerçevesi. Onlar basitlik ve kontrol farklı trade-off sunuyoruz.'
author: vturecek
ms.topic: conceptual
ms.date: 01/07/2020
ms.custom: sfrev
ms.openlocfilehash: 11e32c9d1290227e638a314ed8417b1bed906842
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75749542"
---
# <a name="service-fabric-programming-model-overview"></a>Service Fabric programlama modeline genel bakış

Service Fabric, hizmetlerinizi yazmanın ve yönetmenin birden çok yolunu sunar. Hizmetler, platformun özelliklerinden ve uygulama çerçevelerinden tam olarak yararlanmak için Service Fabric API'leri kullanmayı seçebilir. Hizmetler ayrıca, Hizmet Kumaşı kümesinde barındırılan bir kapsayıcıda çalışan herhangi bir dilde veya kodda yazılmış derlenmiş yürütülebilir program da olabilir.

## <a name="guest-executables"></a>Konuk yürütülebilir dosyaları

[Yürütülebilir konuk,](service-fabric-guest-executables-introduction.md) uygulamanızda hizmet olarak çalıştırılabilen varolan, rasgele yürütülebilir (herhangi bir dilde yazılmış) dır. Konuk çalıştırılabilir ler Doğrudan Servis Kumaşı SDK API'lerini aramaz. Bununla birlikte, Service Fabric tarafından açığa çıkarılan REST API'lerini arayarak hizmet keşfedilebilirliği, özel sağlık durumu ve yük raporlaması gibi platform sunduğu özelliklerden yararlanmaya devam etmektedirler. Ayrıca tam uygulama yaşam döngüsü desteğine de sahiptir.

İlk [konuk çalıştırılabilir uygulamanızı](service-fabric-deploy-existing-app.md)dağıtarak konuk çalıştırılabilir uygulamalarla başlayın.

## <a name="containers"></a>Kapsayıcılar

Varsayılan olarak, Service Fabric hizmetleri işlem olarak dağıtır ve etkinleştirir. Service Fabric de [kapsayıcılarda](service-fabric-containers-overview.md)hizmetleri dağıtabilirsiniz. Service Fabric, Windows Server 2016 ve sonraki sürümlerinde Linux kapsayıcılarının ve Windows kapsayıcılarının dağıtımını destekler. Konteyner görüntüleri herhangi bir konteyner deposundan çıkarılabilir ve makineye dağıtılabilir. Varolan uygulamaları konuk yürütülebilir ler, Servis Kumaşı ve durum dışı güvenilir hizmetler veya güvenilir aktörler olarak kapsayıcılara dağıtabilir ve aynı uygulamada kapsayıcılarda süreçleri ve hizmetleri karıştırabilirsiniz.

[Windows veya Linux'ta hizmetlerinizi kapsayıcılaştırma hakkında daha fazla bilgi edinin](service-fabric-deploy-container.md)

## <a name="reliable-services"></a>Reliable Services

Güvenilir Hizmetler, Service Fabric platformuyla bütünleşen ve tüm platform özelliklerinden yararlanan yazma hizmetleri için hafif bir çerçevedir. Güvenilir Hizmetler, Hizmet Kumaşı çalışma zamanının hizmetlerinizin yaşam döngüsünü yönetmesine ve hizmetlerinizin çalışma zamanıyla etkileşimkurmasını sağlayan en az API kümesi sağlar. Uygulama çerçevesi en az düzeydedir ve tasarım ve uygulama seçenekleri üzerinde tam denetim sağlar ve ASP.NET Core gibi diğer uygulama çerçevelerini barındırmak için kullanılabilir.

Güvenilir Hizmetler, hizmetin her örneğinin eşit olarak oluşturulduğu ve durum durumunun Azure DB veya Azure Tablo Depolama gibi harici bir çözümde kalıcı olduğu web sunucuları gibi çoğu hizmet platformuna benzer şekilde durum durumu olmayabilir.

Hizmet Kumaşına Özel, Güvenilir Hizmetler, güvenilir koleksiyonlar kullanılarak devletin doğrudan hizmetin kendisinde kalıcı olduğu durum da belirtilebilir. Durum çoğaltma yoluyla yüksek kullanılabilir hale getirilir ve bölümleme yoluyla dağıtılır, tüm Service Fabric tarafından otomatik olarak yönetilir.

[Güvenilir Hizmetler hakkında daha fazla bilgi edinin](service-fabric-reliable-services-introduction.md) veya ilk Güvenilir [Hizmetinizi yazarak](service-fabric-reliable-services-quick-start.md)başlayın.

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core, web uygulamaları, IoT uygulamaları ve mobil arka uçlar gibi bulut tabanlı Internet tabanlı uygulamalar oluşturmak için açık kaynak kodlu, çapraz platform çerçevesidir. Service Fabric, ASP.NET Core ile entegre edilebilmekte, böylece Güvenilir Koleksiyonlar ve Service Fabric'in gelişmiş orkestrasyon özelliklerinden yararlanan hem stateless hem de stateful ASP.NET Core uygulamaları yazabilirsiniz.

[Hizmet KumaşASP.NET Core hakkında daha fazla bilgi edinin](service-fabric-reliable-services-communication-aspnetcore.md) veya ilk ASP.NET Core Service Fabric [uygulamanızı yazarak](service-fabric-tutorial-create-dotnet-app.md)başlayın.

## <a name="reliable-actors"></a>Reliable Actors

Güvenilir Hizmetler üzerine inşa edilen Güvenilir Aktör çerçevesi, hesaplamalı [aktör modeline](https://en.wikipedia.org/wiki/Actor_model)dayalı [Sanal Aktör](https://research.microsoft.com/en-us/projects/orleans/) deseni uygulayan bir uygulama çerçevesidir. Güvenilir Aktör *çerçevesi, bağımsız*bilgi işlem ve durum birimlerini, aktörler olarak adlandırılan tek iş parçacığı yürütme ile kullanır. Güvenilir Aktör çerçevesi aktörler ve önceden ayarlanmış durum kalıcılığı ve ölçeklendirme yapılandırmaları için yerleşik iletişim sağlar.

Güvenilir Aktörler Güvenilir Hizmetler üzerine kurulu bir uygulama çerçevesi olduğundan, Hizmet Kumaşı platformu ile tam olarak entegre edilmiştir ve platform tarafından sunulan tüm özelliklerden yararlanır.

[Güvenilir Aktörler hakkında daha fazla bilgi edinin](service-fabric-reliable-actors-introduction.md) veya ilk Güvenilir Aktör [hizmetinizi yazarak](service-fabric-reliable-actors-get-started.md) başlayın

[ASP.NET Core'u kullanarak bir ön uç hizmeti oluşturun](service-fabric-reliable-services-communication-aspnetcore.md)

## <a name="next-steps"></a>Sonraki adımlar

[Servis Kumaş ve konteynerler genel bakış](service-fabric-containers-overview.md)

[Reliable Services özelliğine genel bakış](service-fabric-reliable-services-introduction.md)

[Reliable Actors hizmetine genel bakış](service-fabric-reliable-actors-introduction.md)

[Servis Kumaş ve ASP.NET Çekirdek](service-fabric-reliable-services-communication-aspnetcore.md)
