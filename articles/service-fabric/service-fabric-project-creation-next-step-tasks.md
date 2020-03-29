---
title: Service Fabric proje oluşturma sonraki adımlar
description: Visual Studio'da oluşturduğunuz uygulama projesi hakkında bilgi edinin.  Öğreticileri kullanarak nasıl hizmet oluşturabileceğinizi ve Service Fabric için hizmet geliştirme hakkında daha fazla bilgi edinin.
ms.topic: conceptual
ms.date: 12/07/2017
ms.openlocfilehash: e8fc105657bda6114851f4819be4658926ad621c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75349408"
---
# <a name="your-service-fabric-application-and-next-steps"></a>Servis Kumaşı uygulamanız ve sonraki adımlar
Azure Hizmet Kumaşı uygulamanız oluşturuldu. Bu makalede, denemek için bazı öğreticiler açıklanır, projenizin makyaj, ilginizi çekebilir bazı daha fazla bilgi, ve potansiyel sonraki adımlar.

## <a name="get-started-with-tutorials-walk-throughs-and-samples"></a>Öğreticiler, geçişler ve örneklerle başlayın
Başlamaya hazır mısınız?  

.NET uygulama öğreticisini incele. ASP.NET Core ön uç ve durumsal bir arka uç içeren bir uygulamayı nasıl [oluşturup,](service-fabric-tutorial-create-dotnet-app.md) uygulamayı bir kümeye [dağıtmayı,](service-fabric-tutorial-deploy-app-to-party-cluster.md) [CI/CD'yi yapılandırmayı](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)ve [izleme ve tanılamayı nasıl ayarlayayizleyeceğini](service-fabric-tutorial-monitoring-aspnet.md)öğrenin.

Ya da, aşağıdaki walk-throughs birini deneyin ve ilk oluşturun ...
- [Windows'da C# Güvenilir Hizmetler hizmeti](service-fabric-reliable-services-quick-start.md) 
- [Windows'da C# Güvenilir Aktörler hizmeti](service-fabric-reliable-actors-get-started.md) 
- [Windows'da konuk çalıştırılabilir hizmet](quickstart-guest-app.md) 
- [Windows kapsayıcı uygulaması](service-fabric-get-started-containers.md) 

Ayrıca [örnek uygulamalarımızı](https://aka.ms/servicefabricsamples)denemek de ilginizi çekebilir.

## <a name="have-questions-or-feedback--need-to-report-an-issue"></a>Sorularınız veya geri bildiriminiz mi var?  Bir sorunu bildirmenize mi gerek var?
Sık [sorulan soruları](service-fabric-common-questions.md) okuyun ve Service Fabric'in neler yapabileceği ve nasıl kullanılması gerektiği hakkında yanıtlar bulun.

[Destek seçenekleri,](service-fabric-support.md) StackOverflow ve MSDN'deki forumları, sorunları bildirme, destek alma ve ürün geri bildirimi gönderme seçeneklerinin yanı sıra soru sormak için listeler.

## <a name="the-application-project"></a>Uygulama projesi
Her yeni uygulama bir uygulama projesi içerir. Seçilen hizmet türüne bağlı olarak bir veya iki ek proje olabilir.

Uygulama projesi aşağıdakilerden oluşur:

* Uygulamanızı oluşturan hizmetlere yapılan başvurular kümesi.
* Küme bitiş noktasıyla ilgili tercihler ve varsayılan olarak yükseltme dağıtımlarının yapılıp yapılmadığı gibi farklı ortamlarla çalışma tercihlerini korumak için kullanabileceğiniz üç yayımlama profili (1-Node Local, 5-Node Local ve Cloud).
* Bir hizmet için oluşturulacak bölüm sayısı gibi ortama özgü uygulama yapılandırmalarını korumak için kullanabileceğiniz üç uygulama parametre dosyası (yukarıdakiyle aynı). Uygulamanızı birden çok ortam için nasıl [yapılandırılamayı](service-fabric-manage-multiple-environment-app-configuration.md)öğrenin.
* Uygulamanızı komut satırından veya otomatik sürekli tümleştirme ve dağıtım ardışık bir düzeneğinin bir parçası olarak dağıtmak için kullanabileceğiniz bir dağıtım komut dosyası. [PowerShell'i kullanarak uygulamaları dağıtma](service-fabric-deploy-remove-applications.md)hakkında daha fazla bilgi edinin.
* Uygulamayı açıklayan uygulama bildirimi. Manifeste'yi ApplicationPackageRoot klasörü altında bulabilirsiniz. [Uygulama ve hizmet bildirimleri](service-fabric-application-model.md)hakkında daha fazla bilgi edinin.



## <a name="learn-more-about-the-programming-models"></a>Programlama modelleri hakkında daha fazla bilgi edinin
Service Fabric, hizmetlerinizi yazmanın ve yönetmenin birden çok yolunu sunar.  Burada genel bakış ve [devletsiz ve stateful Güvenilir Hizmetler,](service-fabric-reliable-services-introduction.md) [Güvenilir Aktörler,](service-fabric-reliable-actors-introduction.md) [konteynerler,](service-fabric-containers-overview.md) [konuk yürütülebilir,](service-fabric-guest-executables-introduction.md)ve [stateless ve stateful ASP.NET Core hizmetleri](service-fabric-reliable-services-communication-aspnetcore.md)kavramsal bilgiler .

## <a name="learn-about-service-communication"></a>Hizmet iletişimi hakkında bilgi edinin
Hizmet Kumaşı uygulaması, her hizmetin özel bir görev yürüttüğü farklı hizmetlerden oluşur. Bu hizmetler birbiriyle iletişim kurabilir ve kümenin dışında hizmetlere bağlanan ve hizmetlerle iletişim kuran istemci uygulamaları olabilir. Service Fabric'te [hizmetlerinizle ve hizmetleriniz arasında nasıl iletişim kurabileceğinizi](service-fabric-connect-and-communicate-with-services.md) öğrenin. 

## <a name="learn-about-configuring-application-security"></a>Uygulama güvenliğini yapılandırma hakkında bilgi edinin
Kümede çalışan uygulamaları farklı kullanıcı hesapları altında güvenebilirsiniz. Hizmet Kumaşı, dosyalar, dizinler ve sertifikalar gibi kullanıcı hesapları altında dağıtım sırasında uygulamalar tarafından kullanılan kaynakların güvenliğini sağlamaya da yardımcı olur. Bu, paylaşılan barındırılan bir ortamda bile çalışan uygulamaları birbirinden daha güvenli hale getirir.  [Uygulamanız için güvenlik ilkelerini nasıl yapılandırıştırılamayı](service-fabric-application-runas-security.md)öğrenin.

Uygulamanız, depolama bağlantı dizeleri, parolalar veya düz metinolarak işlenmemesi gereken diğer değerler gibi hassas bilgiler içerebilir. Uygulamanızdaki sırları nasıl [yöneteceklerini](service-fabric-application-secret-management.md)öğrenin.

## <a name="learn-about-the-application-lifecycle"></a>Uygulama yaşam döngüsü hakkında bilgi edinin
Diğer platformlarda olduğu gibi, Service Fabric uygulaması genellikle aşağıdaki aşamalardan geçer: tasarım, geliştirme, test, dağıtım, yükseltme, bakım ve kaldırma. [Bu makalede,](service-fabric-application-lifecycle.md) API'ler ve bunların Hizmet Kumaşı uygulama yaşam döngüsünün aşamalarında farklı roller tarafından nasıl kullanıldığına genel bir bakış sunulmaktadır.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure'da bir Windows kümesi oluşturun.](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
- [Hizmet Kumaş Explorer](service-fabric-visualizing-your-cluster.md)ile dağıtılmış uygulamalar ve fiziksel düzen de dahil olmak üzere kümenizi görselleştirin.
- [Hizmetlerinizi sürüm ve yükseltme](service-fabric-application-upgrade-tutorial.md)


