---
title: Azure Servis Kumaş Örgü için Terminoloji
description: Bu makalede, belgelerde kullanılan terimleri daha iyi anlamanıza yardımcı olmak için Azure Service Fabric Mesh tarafından kullanılan terminoloji ayrıntılı olarak anlatılmıştır.
author: dkkapur
ms.author: dekapur
ms.date: 11/28/2018
ms.topic: conceptual
ms.openlocfilehash: 6df7a6b708bca81f1390ac879f24ea4c22c38bee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75351969"
---
# <a name="service-fabric-mesh-terminology"></a>Servis Kumaş Örgü terminolojisi

Azure Service Fabric Mesh, geliştiricilerin sanal makineleri, depolama alanını veya ağ bileşenlerini yönetmeden mikro hizmet uygulamaları dağıtmasını sağlayan tam olarak yönetilen bir hizmettir. Bu makalede, belgelerde kullanılan terimleri daha iyi anlamanıza yardımcı olmak için Azure Service Fabric Mesh tarafından kullanılan terminoloji ayrıntılı olarak anlatılmıştır.

## <a name="service-fabric"></a>Service Fabric

[Service Fabric,](/azure/service-fabric/) ölçeklenebilir ve güvenilir mikro hizmetlerin paketlenin, dağıtılmasını ve yönetilmesini kolaylaştıran açık kaynaklı dağıtılmış sistemler platformudur. Service Fabric, Service Fabric Mesh'e güç veren orkestratördür. Service Fabric, mikrohizmet uygulamalarınızı nasıl oluşturabileceğiniz ve çalıştırabileceğinize yönelik seçenekler sunar. Hizmetlerinizi yazmak ve uygulamayı birden çok ortam seçeneğinden çalıştıracağınızı seçmek için istediğiniz çerçeveyi kullanabilirsiniz.

## <a name="application-and-service-concepts"></a>Uygulama ve hizmet kavramları

**Servis Kumaş Örgü Uygulaması**: Servis Kumaşı Örgü Uygulamaları [Kaynak Modeli](/azure/service-fabric-mesh/service-fabric-mesh-service-fabric-resources) (YAML ve JSON kaynak dosyaları) tarafından tanımlanır ve Servis Kumaşı'nın çalıştığı herhangi bir ortama dağıtılabilir.

**Hizmet Kumaş**ıSlama Uygulaması : Service Fabric Native [Applications, Native Application Model](/azure/service-fabric/service-fabric-application-model) (XML tabanlı uygulama ve hizmet beyannameleri) ile açıklanmaktadır.  Servis Kumaş Yerli Uygulamaları Hizmet Kumaş Örgü çalıştırılamaz.

**Uygulama**: Service Fabric Mesh uygulaması, Mesh uygulamasının dağıtım, sürüm ve kullanım ömrü birimidir. Her uygulama örneğinin yaşam döngüsü bağımsız olarak yönetilebilir.  Uygulamalar bir veya daha fazla servis kodu paketi ve ayarlarından oluşur. Bir uygulama Azure Kaynak Modeli (RM) şeması kullanılarak tanımlanır.  Hizmetler, RM şablonundaki uygulama kaynağının özellikleri olarak tanımlanır.  Uygulama tarafından kullanılan ağlar ve birimler uygulama tarafından başvurulmaktadır.  Bir uygulama oluştururken, uygulama, hizmet(ler), ağ ve birim(ler) Service Fabric Resource Model kullanılarak modellenir.

**Hizmet**: Bir uygulamadaki hizmet bir microservice temsil eder ve tam ve bağımsız bir işlevi gerçekleştirir. Her hizmet, kod paketiyle ilişkili kapsayıcı görüntüsünü çalıştırmak için gereken her şeyi açıklayan bir veya daha fazla kod paketinden oluşur.  Bir uygulamadaki hizmet yinelemelerinin sayısı girip çıkabilir.

**Kod paketi**: Kod paketleri, kod paketiyle ilişkili kapsayıcı görüntüsünü çalıştırmak için gereken her şeyi açıklar:

* Kapsayıcı adı, sürümü ve kayıt defteri
* Her kapsayıcı için gerekli CPU ve bellek kaynakları
* Ağ uç noktaları
* Ayrı bir birim kaynağa atıfta bulunarak, kapsayıcıya monte edilen birimler.

## <a name="deployment-and-application-models"></a>Dağıtım ve uygulama modelleri 

Hizmetlerinizi dağıtmak için, bunların nasıl çalışması gerektiğini açıklamanız gerekir. Service Fabric üç farklı dağıtım modelini destekler:

### <a name="resource-model"></a>Kaynak modeli
Hizmet Kumaş Kaynakları, Service Fabric'e ayrı ayrı dağıtılabilen her şeydir; uygulamalar, hizmetler, ağlar ve birimler de dahil olmak üzere. Kaynaklar, küme bitiş noktasına dağıtılabilen bir JSON dosyası kullanılarak tanımlanır.  Hizmet Kumaş Kafesi için Azure Kaynak Modeli şeması kullanılır. Bir YAML dosya şeması da daha kolay tanım dosyaları yazar için kullanılabilir. Kaynaklar Service Fabric'in çalıştığı her yerde dağıtılabilir. Kaynak modeli, Service Fabric uygulamalarınızı tanımlamanın en basit yoludur. Ana odak noktası, konteynerize hizmetlerin basit dağıtımı ve yönetimidir. Daha fazla bilgi için [Hizmet Kumaş Kaynak Modeline Giriş 'i](/azure/service-fabric-mesh/service-fabric-mesh-service-fabric-resources)okuyun.

### <a name="native-model"></a>Yerli model
Yerel uygulama modeli, uygulamalarınıza Hizmet Kumaşı'na tam düşük seviyeli erişim sağlar. Uygulamalar ve hizmetler XML bildirim dosyalarında kayıtlı türler olarak tanımlanır.

Yerel model, C# ve Java'daki Hizmet Dokusu çalışma zamanı API'lerine ve küme yönetimi API'lerine erişim sağlayan Güvenilir Hizmetler çerçevesini destekler. Yerel model de rasgele kapsayıcılar ve yürütülebilir destekler.

Yerel model Servis Kumaş Örgü ortamında desteklenmez.  Daha fazla bilgi için [programlama modeline genel bakış](/azure/service-fabric/service-fabric-choose-framework)alabakın.

### <a name="docker-compose"></a>Docker Oluştur 
[Docker Compose,](https://docs.docker.com/compose/) Docker projesinin bir parçasıdır. Service Fabric, Docker Compose modelini kullanarak uygulamaları dağıtmak için sınırlı destek sağlar.

## <a name="environments"></a>Ortamlar

Service Fabric, birçok farklı hizmet ve ürünün dayandığı açık kaynak kodlu bir platform teknolojisidir. Microsoft aşağıdaki seçenekleri sağlar:

 - **Service Fabric Mesh**: Service Fabric uygulamalarını Microsoft Azure'da çalıştırmak için tam olarak yönetilen bir hizmettir.
 - **Azure Hizmet Kumaşı**: Azure barındırılan Hizmet Kumaşı küme teklifi. Service Fabric ve Azure altyapısı arasında tümleştirmenin yanı sıra Service Fabric kümelerinin yükseltme ve yapılandırma yönetimini sağlar.
 - **Hizmet Kumaşı tek başına**: Service [Fabric kümelerini her yerde](/azure/service-fabric/service-fabric-deploy-anywhere) (şirket içinde veya herhangi bir bulut sağlayıcısında) dağıtmak için bir dizi yükleme ve yapılandırma aracı. Azure tarafından yönetilmez.
 - **Service Fabric geliştirme kümesi**: Service Fabric uygulamalarının geliştirilmesi için Windows, Linux veya Mac'te yerel bir geliştirme deneyimi sağlar.

## <a name="environment-framework-and-deployment-model-support-matrix"></a>Çevre, çerçeve ve dağıtım modeli destek matrisi
Farklı ortamlar, çerçeveler ve dağıtım modelleri için farklı düzeyde destek düzeyine sahiptir. Aşağıdaki tabloda desteklenen çerçeve ve dağıtım modeli birleşimleri açıklanmaktadır.

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

Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh’e genel bakış](service-fabric-mesh-overview.md) makalesini okuyun.

[Sık sorulan sorulara](service-fabric-mesh-faq.md) yanıtlar bulun.
