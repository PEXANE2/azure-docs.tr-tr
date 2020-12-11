---
title: Azure Service Fabric'e genel bakış
description: Service Fabric ölçeklenebilir, güvenilir ve kolayca yönetilen mikro hizmetler oluşturmak için dağıtılmış bir sistem platformudur.
ms.topic: overview
ms.date: 09/22/2020
ms.custom: contperf-fy21q1
ms.openlocfilehash: b6a14ba3f905fbef3fc3796b616cd594941a3d60
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97028640"
---
# <a name="overview-of-azure-service-fabric"></a>Azure Service Fabric'e genel bakış

Azure Service Fabric, ölçeklenebilir ve güvenilir mikro hizmetleri ve kapsayıcıları paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştıran bir [Dağıtılmış sistemler platformudur](#container-orchestration) . Service Fabric Ayrıca, bulut Yerel uygulamaları [geliştirme ve yönetme](#application-lifecycle-management) konularında önemli zorluk ortadan de ele alır.

Service Fabric bir temel farklılama, durum bilgisi olan hizmetler oluşturmaya yönelik güçlü odadır. Service Fabric [programlama modelini](#stateless-and-stateful-microservices) kullanabilir veya herhangi bir dilde veya kodda yazılmış Kapsayıcılı durum bilgisi olan hizmetler çalıştırabilirsiniz. Azure 'a ek olarak, şirket içi ve diğer genel bulutlarda Windows Server ve Linux dahil olmak üzere [dilediğiniz yerde Service Fabric kümeleri](#any-os-any-cloud)oluşturabilirsiniz.

![Service Fabric platformu yaşam döngüsü yönetimi, kullanılabilirlik, düzenleme, programlama modelleri, sistem durumu ve izleme, geliştirme ve Ops araçları, Azure 'da, şirket içinde, diğer bulutlarda ve geliştirme makinenizde otomatik ölçeklendirme sağlar][Image1]

Service Fabric bugün Azure SQL Veritabanı, Azure Cosmos DB, Cortana, Microsoft Power BI, Microsoft Intune, Azure Event Hubs, Azure IoT Hub, Dynamics 365, Skype Kurumsal ve birçok temel Azure hizmeti gibi çok sayıda Microsoft hizmetini güçlendirir.

## <a name="container-orchestration"></a>Kapsayıcı düzenleme

Service Fabric, Microsoft hizmetlerinin büyük ölçekte çalışmasını öğrenerek yararlanırken, mikro hizmetleri bir makine kümesi genelinde dağıtmak ve yönetmek için Microsoft 'un [kapsayıcı Orchestrator](service-fabric-cluster-resource-manager-introduction.md) ' dır. Service Fabric, her makine için yüzlerce veya binlerce uygulama ya da kapsayıcı ile yüksek yoğunluklu uygulamaları saniyeler içinde dağıtabilir. Service Fabric, her iki hizmeti de aynı uygulamadaki kapsayıcılardaki süreçler ve hizmetlerde karıştırabilirsiniz.

Service Fabric temel kavramlar, programlama modelleri, uygulama yaşam döngüsü, test, kümeler ve sistem durumu izleme [hakkında daha fazla bilgi edinin](service-fabric-content-roadmap.md) .

## <a name="stateless-and-stateful-microservices"></a>Durum bilgisiz ve durum bilgisi olan mikro hizmetler

Service Fabric durum bilgisiz ve durum bilgisi olan mikro hizmetleri destekleyen gelişmiş, hafif bir çalışma zamanı sağlar. Service Fabric temel farklılık bir örneği, durum bilgisi olan hizmetler oluşturmak için Service Fabric [yerleşik programlama modelleriyle](service-fabric-choose-framework.md) veya Kapsayıcılı durum bilgisi olan hizmetlerden oluşan sağlam bir dağıtımdır.

Service Fabric durum bilgisi olan hizmetlerden faydalanabilir [uygulama senaryoları](service-fabric-application-scenarios.md) hakkında daha fazla bilgi edinin.

## <a name="application-lifecycle-management"></a>Uygulama yaşam döngüsü yönetimi

Service Fabric, kapsayıcılar dahil olmak üzere bulut uygulamalarının tam uygulama yaşam döngüsü ve CI/CD desteği sağlar: dağıtım, günlük izleme, yönetim ve bakım aracılığıyla geliştirme, son kullanımdan alma. [Azure Pipelines](https://www.visualstudio.com/team-services/), [Jenkins](https://jenkins.io/index.html) ve [Octopus Deploy](https://octopus.com/) gibi CI/CD araçlarıyla tümleştirilmiş olan Service Fabric, diğer herhangi bir popüler CI/CD aracıyla da kullanılabilir.

Uygulama yaşam döngüsü yönetimi hakkında daha fazla bilgi için, [Uygulama yaşam döngüsü](service-fabric-application-lifecycle.md) konusunu okuyun. Mevcut uygulamaları Service Fabric dağıtmak için bkz. [Konuk yürütülebilir dosyası dağıtma](service-fabric-deploy-existing-app.md).

## <a name="any-os-any-cloud"></a>Tüm işletim sistemleri, tüm bulutlar

[Windows Server veya Linux](service-fabric-linux-windows-differences.md)'ta [Azure veya şirket içi](service-fabric-deploy-anywhere.md)gibi birçok ortamda Service Fabric için kümeler oluşturabilirsiniz. Hatta diğer genel bulutlarda bile kümeler oluşturabilirsiniz. Service Fabric SDK 'daki geliştirme ortamı, hiçbir öykünücü dahil olmak üzere üretim ortamıyla aynıdır. Diğer bir deyişle, yerel geliştirme kümenizde çalışan, diğer ortamlardaki kümelerinize ne şekilde dağıtılır.

[Windows geliştirme](service-fabric-get-started.md)için SERVICE fabrıc .NET SDK, Visual Studio ve PowerShell ile tümleşiktir. [Linux geliştirme](service-fabric-get-started-linux.md)Için Service Fabric Java SDK 'Sı, çakışan küreler ile tümleşiktir ve Java, .NET Core ve kapsayıcı uygulamalarına yönelik şablonlar oluşturmak Için yeumman kullanılır.

## <a name="compliance"></a>Uyumluluk

Azure Service Fabric kaynak sağlayıcısı tüm Azure bölgelerinde kullanılabilir ve aşağıdakiler de dahil olmak üzere tüm Azure uyumluluk sertifikalarla uyumludur: SOC, ISO, PCI DSS, HIPAA ve GDPR. Tüm liste için bkz. [Microsoft Uyumluluk teklifleri](https://www.microsoft.com/trustcenter/compliance/complianceofferings).

## <a name="next-steps"></a>Sonraki adımlar

Azure Service Fabric ilk uygulamanızı oluşturun ve dağıtın:

> [!div class="nextstepaction"]
> [Hızlı başlangıç Service Fabric][sf-quickstart]

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
[sf-quickstart]: ./service-fabric-quickstart-dotnet.md
