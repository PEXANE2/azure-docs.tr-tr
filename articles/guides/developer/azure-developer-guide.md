---
title: Azure'daki geliştiriciler için başlangıç kılavuzunu alın | Microsoft Dokümanlar
description: Bu makalede, geliştirme gereksinimleri için Microsoft Azure platformlarını kullanmaya başlamak isteyen geliştiriciler için temel bilgiler sağlanmaktadır.
services: ''
cloud: ''
documentationcenter: ''
author: ggailey777
manager: erikre
ms.assetid: ''
ms.service: azure
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/18/2019
ms.author: glenga
ms.openlocfilehash: d74fd2e3f6b5cc090c6313aba67a1e139c713b85
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79245011"
---
# <a name="get-started-guide-for-azure-developers"></a>Azure geliştiricileri için başlangıç kılavuzunu başlatın

## <a name="what-is-azure"></a>Azure nedir?

Azure, mevcut uygulamalarınızı barındırabilen ve yeni uygulama geliştirmeyi kolaylaştırabilen eksiksiz bir bulut platformudur. Azure, şirket içi uygulamaları bile geliştirebilir. Azure, bulut bilgi işlemin verimliliklerinden yararlanırken uygulamalarınızı geliştirmeniz, test etmek, dağıtmak ve yönetmek için gereken bulut hizmetlerini entegre eder.

Uygulamalarınızı Azure'da barındırarak, müşteri talebiniz arttıkça küçük ve kolayca ölçeklendirebilirsiniz. Azure, farklı bölgeler arasındaki başarısızlık da dahil olmak üzere yüksek kullanılabilirlik uygulamaları için gereken güvenilirliği de sunar. [Azure portalı,](https://portal.azure.com) tüm Azure hizmetlerinizi kolayca yönetmenize olanak tanır. Ayrıca, hizmete özel API'leri ve şablonları kullanarak hizmetlerinizi programlı olarak yönetebilirsiniz.

Bu kılavuz, uygulama geliştiricileri için Azure platformuna giriştir. Azure'da yeni uygulamalar oluşturmaya veya varolan uygulamaları Azure'a geçirmeye başlamak için ihtiyacınız olan kılavuz ve yönü sağlar.

## <a name="where-do-i-start"></a>Nereden başlamalıyım?

Azure'un sunduğu tüm hizmetlerle, çözüm mimarinizi desteklemek için hangi hizmetlere ihtiyacınız olduğunu bulmak korkutucu bir görev olabilir. Bu bölümde, geliştiricilerin sık kullandığı Azure hizmetleri vurgulanmaktadır. Tüm Azure hizmetlerinin listesi için [Azure belgelerine](../../index.yml)bakın.

Öncelikle, uygulamanızı Azure'da nasıl barındırabileceğinize karar vermelisiniz. Tüm altyapınızı sanal bir makine (VM) olarak yönetmeniz gerekiyor mu? Azure'un sağladığı platform yönetim olanaklarını kullanabilir misiniz? Belki de yalnızca kod yürütme barındırmak için sunucusuz bir çerçeve gerekir?

Uygulamanızın, Azure'un çeşitli seçenekler sunduğu bulut depolamasına ihtiyacı vardır. Azure'un kurumsal kimlik doğrulamadan yararlanabilirsiniz. Bulut tabanlı geliştirme ve izleme araçları da vardır ve çoğu barındırma hizmeti DevOps entegrasyonu sunar.

Şimdi, uygulamalarınız için araştırmanızı önerdiğimiz belirli hizmetlerden bazılarına bakalım.

### <a name="application-hosting"></a>Uygulama barındırma

Azure, altyapı ayrıntıları hakkında endişelenmenize gerek kalmaması için uygulamanızı çalıştırmak için bulut tabanlı birkaç bilgi işlem teklifi sağlar. Uygulama kullanımınız arttıkça kaynaklarınızı kolayca büyütebilir veya ölçeklendirebilirsiniz.

Azure, uygulama geliştirme ve barındırma ihtiyaçlarınızı destekleyen hizmetler sunar. Azure, uygulama barındırma üzerinde tam kontrol sağlamak için Hizmet Olarak Altyapı (IaaS) sağlar. Azure'un Hizmet Olarak Platformu (PaaS) teklifleri, uygulamalarınıza güç sağlamak için gereken tam yönetilen hizmetleri sağlar. Azure'da tek yapmanız gereken kodunuzu yazmak olduğu gerçek sunucusuz barındırma bile vardır.

![Azure uygulama barındırma seçenekleri](./media/azure-developer-guide/azure-developer-hosting-options.png)


#### <a name="azure-app-service"></a>Azure App Service

Web tabanlı projelerinizi yayınlamak için en hızlı yolu istediğinizde Azure Uygulama Hizmeti'ni düşünün. Uygulama Hizmeti, mobil müşterilerinizi desteklemek ve kolayca tüketilen REST API'lerini yayınlamak için web uygulamalarınızı genişletmenizi kolaylaştırır. Bu platform, sosyal sağlayıcıları, trafik tabanlı otomatik ölçeklendirmeyi, üretimde test ve sürekli ve kapsayıcı tabanlı dağıtımları kullanarak kimlik doğrulaması sağlar.

Web uygulamaları, mobil uygulama arka uçları ve API uygulamaları oluşturabilirsiniz.

Her üç uygulama türü de App Service çalışma süresini paylaştığından, bir web sitesi barındırabilir, mobil istemcileri destekleyebilir ve API'lerinizi aynı proje veya çözümden Azure'da ifşa edebilirsiniz. Uygulama Hizmeti hakkında daha fazla bilgi edinmek için [Azure Web Uygulamaları nedir'e](../../app-service/overview.md)bakın.

Uygulama Hizmeti DevOps düşünülarak tasarlanmıştır. Yayımlama ve sürekli tümleştirme dağıtımları için çeşitli araçları destekler. Bu araçlar arasında GitHub webhooks, Jenkins, Azure DevOps, TeamCity ve diğerleri yer almaktadır.

Çevrimiçi [geçiş aracını](https://appmigration.microsoft.com/)kullanarak mevcut uygulamalarınızı App Service'e geçirebilirsiniz.

> **Ne zaman kullanılır**: Mevcut web uygulamalarını Azure'a geçirdiğinizde ve web uygulamalarınız için tam olarak yönetilen bir barındırma platformuna ihtiyaç duyduğunuzda Uygulama Hizmeti'ni kullanın. Mobil istemcileri desteklemeniz veya UYGULAMANıZLA REST API'lerini ortaya çıkarmanız gerektiğinde De App Service'i kullanabilirsiniz.
>
> **Başlayın**: Uygulama Hizmeti, ilk web uygulamanızı, [mobil](../../app-service-mobile/app-service-mobile-ios-get-started.md) [uygulamanızı](../../app-service/app-service-web-get-started-dotnet.md)veya [API uygulamanızı](../../app-service/app-service-web-tutorial-rest-api.md)oluşturmanızı ve dağıtmanızı kolaylaştırır.
>
> **Şimdi deneyin**: Uygulama Hizmeti, bir Azure hesabına kaydolmak zorunda kalmadan platformu denemek için kısa ömürlü bir uygulama sağlamanıza olanak tanır. Platformu deneyin ve [Azure Uygulama Hizmeti uygulamanızı oluşturun.](https://tryappservice.azure.com/)

#### <a name="azure-virtual-machines"></a>Azure Sanal Makineler

Hizmet Olarak Altyapı (IaaS) sağlayıcısı olarak Azure, uygulamanızı Windows veya Linux VM'lerine dağıtmanıza veya geçiş yapmanızı sağlar. Azure Sanal Ağı ile birlikte Azure Sanal Makineler, Windows veya Linux Sanal Makinelerinin Azure'a dağıtımını destekler. VM'ler ile makinenin yapılandırması üzerinde tam denetime sahipsiniz. VM'leri kullanırken, tüm sunucu yazılım yükleme, yapılandırma, bakım ve işletim sistemi yamaları sizin sorumluluğunuzdadır.

VM'lerle sahip olduğunuz denetim düzeyi nedeniyle, Azure'da PaaS modeline uymayan çok çeşitli sunucu iş yüklerini çalıştırabilirsiniz. Bu iş yükleri veritabanı sunucularını, Windows Server Active Directory'yi ve Microsoft SharePoint'i içerir. Daha fazla bilgi için [Linux](/azure/virtual-machines/linux/) veya [Windows](/azure/virtual-machines/windows/)için Sanal Makineler belgelerine bakın.

> **Ne zaman kullanılır**: Uygulama altyapınız üzerinde tam denetim istediğinizde Sanal Makineler'i kullanın veya şirket içi uygulama iş yüklerini değişiklik yapmak zorunda kalmadan Azure'a geçirin.
>
> **Başlayın**: Azure portalından bir [Linux VM](../../virtual-machines/virtual-machines-linux-quick-create-portal.md) veya [Windows VM](../../virtual-machines/virtual-machines-windows-hero-tutorial.md) oluşturun.

#### <a name="azure-functions-serverless"></a>Azure Fonksiyonları (sunucusuz)

Kodunuzu çalıştırmak için tüm bir uygulamayı veya altyapıyı oluşturmak ve yönetmek konusunda endişelenmek yerine, ya kodunuzu yazıp olaylara yanıt olarak veya bir programda çalıştırabilseydiniz?  [Azure İşlevler,](../../azure-functions/functions-overview.md) ihtiyacınız olan kodu yazmanızı sağlayan "sunucusuz" tarzı bir tekliftir. İşlevler ile, HTTP istekleri, web hooks, bulut hizmeti olayları veya bir zamanlama ile kod yürütme tetikleyebilirsiniz. C,\#F,\#Node.js, Python veya PHP gibi seçtiğiniz geliştirme dilinizde kodlayabilirsiniz. Tüketim tabanlı faturalandırmayla, yalnızca kodunuzu yürüttüğü süre için ödeme yaptığınızda, Azure gerektiğinde ölçeklendirin.

> **Ne zaman kullanılır**: Diğer Azure hizmetleri tarafından tetiklenen kodunuz olduğunda, web tabanlı olaylar tarafından veya zamanlamada Azure İşlerini kullanın. Tam bir barındırılan projenin ek yüküne ihtiyacınız olmadığında veya yalnızca kodunuzun çalıştığı süre için ödeme yapmak istediğinizde De işlevleri kullanabilirsiniz. Daha fazla bilgi için Azure [İşlevlerine Genel Bakış'a](../../azure-functions/functions-overview.md)bakın.
>
> **Başlayın**: Portaldan [ilk işlevinizi oluşturmak](../../azure-functions/functions-create-first-azure-function.md) için Fonksiyonlar quickstart öğretici izleyin.
>
> **Şimdi deneyin**: Azure İşlevleri, bir Azure hesabına kaydolmak zorunda kalmadan kodunuzu çalıştırmanızı sağlar. Şimdi deneyin ve [ilk Azure İşlevioluşturun.](https://tryappservice.azure.com/)

#### <a name="azure-service-fabric"></a>Azure Service Fabric

Azure Service Fabric dağıtılmış bir sistem platformudur. Bu platform, ölçeklenebilir ve güvenilir mikro hizmetler oluşturmayı, paketlemeyi, dağıtmayı ve yönetmeyi kolaylaştırır. Ayrıca, şu lar gibi kapsamlı uygulama yönetimi yetenekleri de sağlar:

* Sağlama
* Dağıtma
* İzleme
* Yükseltme/yama
* Silme

Ortak bir makine havuzunda çalışan uygulamalar küçük başlayıp gerektiğinde yüzlerce veya binlerce makineye ölçeklendirilebilir.

Service Fabric, WebAPI'yi .NET (OWIN) ve ASP.NET Core için Açık Web Arabirimi ile destekler. Hem .NET Core hem de Java'da Linux'ta hizmet oluşturmak için SDK'lar sağlar. Service Fabric hakkında daha fazla bilgi edinmek için [Service Fabric belgelerine](https://docs.microsoft.com/azure/service-fabric/)bakın.

> **Ne zaman kullanılır:** Service Fabric, bir uygulama oluştururken veya bir mikro hizmet mimarisini kullanmak için varolan bir uygulamayı yeniden yazarken iyi bir seçimdir. Temel altyapı üzerinde veya doğrudan erişime daha fazla denetime ihtiyacınız olduğunda Hizmet Kumaşı'nı kullanın.
>
> **Başlayın:** [İlk Azure Hizmet Kumaşı uygulamanızı oluşturun.](../../service-fabric/service-fabric-create-your-first-application-in-visual-studio.md)

### <a name="enhance-your-applications-with-azure-services"></a>Azure hizmetleriyle uygulamalarınızı geliştirin

Azure, uygulama barındırma nın yanı sıra işlevselliği artırabilecek hizmet teklifleri de sunar. Azure, uygulamalarınızın hem bulutta hem de şirket içinde geliştirilmesini ve bakımını da artırabilir.

#### <a name="hosted-storage-and-data-access"></a>Barındırılan depolama ve veri erişimi

Çoğu uygulamanın veri depolaması gerekir, bu nedenle uygulamanızı Azure'da barındırmaya karar verirseniz, aşağıdaki depolama ve veri hizmetlerinden birini veya birkaçını göz önünde bulundurun.

- **Azure Cosmos DB**: Küresel olarak dağıtılan, çok modelli veritabanı hizmeti. Bu veritabanı, kapsamlı bir SLA ile herhangi bir sayıda coğrafi bölgede iş ve depolamayı elastik olarak ölçeklendirmenize olanak tanır.

  > **Ne zaman kullanılır:** Uygulamanız, mongoDB veritabanları da dahil olmak üzere belge, tablo veya grafik veritabanlarına gerektiğinde, birden çok iyi tanımlanmış tutarlılık modeline sahip olur.
  >
  > **Başlayın:** [Bir Azure Cosmos DB web uygulaması oluşturun.](../../cosmos-db/create-sql-api-dotnet.md) MongoDB geliştiricisiyseniz, [Azure Cosmos DB ile Bir MongoDB web uygulaması oluşturun'a](../../cosmos-db/create-mongodb-dotnet.md)bakın.

- **Azure Depolama**: Lekeler, kuyruklar, dosyalar ve diğer ilişkisel olmayan veriler için dayanıklı ve yüksek kullanılabilir depolama alanı sunar. Depolama, VM'ler için depolama temeli sağlar.

  > **Ne zaman kullanılır**: Uygulamanız anahtar değer çiftleri (tablolar), blobs, dosya paylaşımları veya iletiler (kuyruklar) gibi ilişkisel olmayan verileri depoladığında.
  >
  > **Başlatın**: Bu tür depolama biri seçin: [blobs,](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) [tablolar,](../../cosmos-db/table-storage-how-to-use-dotnet.md) [kuyruklar,](../../storage/queues/storage-dotnet-how-to-use-queues.md)veya [dosyalar](../../storage/files/storage-dotnet-how-to-use-files.md).

- **Azure SQL Veritabanı**: İlişkisel tabular verileri bulutta depolamak için Microsoft SQL Server altyapısının Azure tabanlı sürümü. SQL Veritabanı, hiçbir kesinti, iş sürekliliği ve veri koruması olmadan öngörülebilir performans, ölçeklenebilirlik sağlar.

  > **Ne zaman kullanılır**: Uygulamanız başvuru bütünlüğü, işlem desteği ve TSQL sorguları için destek ile veri depolama gerektirir.
  >
  > **Başlayın**: [Azure portalını kullanarak dakikalar içinde bir SQL veritabanı oluşturun.](../../sql-database/sql-database-get-started.md)


Mevcut şirket içi verileri Azure'a taşımak için [Azure Veri Fabrikası'nı](../../data-factory/introduction.md) kullanabilirsiniz. Verileri buluta taşımaya hazır değilseniz, Azure Uygulama Hizmetindeki [Karma Bağlantılar,](../../app-service/app-service-hybrid-connections.md) Uygulama Hizmeti barındırılan uygulamanızı şirket içi kaynaklara bağlamanızı sağlar. Ayrıca, şirket içi uygulamalarınızdan Azure veri ve depolama hizmetlerine de bağlanabilirsiniz.

#### <a name="docker-support"></a>Docker desteği

Bir işletim sistemi sanallaştırma biçimi olan Docker kapsayıcıları, uygulamaları daha verimli ve öngörülebilir bir şekilde dağıtmanıza izin verebiliyor. Konteynerleştirilmiş bir uygulama, geliştirme ve test sistemlerinizde olduğu gibi üretimde de çalışır. Standart Docker araçlarını kullanarak konteynerleri yönetebilirsiniz. Azure'da kapsayıcı tabanlı uygulamaları dağıtmak ve yönetmek için mevcut becerilerinizi ve popüler açık kaynak araçlarını kullanabilirsiniz.

Azure, uygulamalarınızda kapsayıcıları kullanmanın çeşitli yollarını sağlar.

- **Azure Docker VM uzantısı**: Docker ana bilgisayar ı olarak hareket edecek şekilde VM'nizi Docker araçlarıyla yapılandırmanızı sağlar.

  > **Ne zaman kullanılır**: VM'deki uygulamalarınız için tutarlı kapsayıcı dağıtımları oluşturmak istediğinizde veya [Docker Compose'u](https://docs.docker.com/compose/overview/)kullanmak istediğinizde.
  >
  > **Başlayın**: [Docker VM uzantısını kullanarak Azure'da docker ortamı oluşturun.](../../virtual-machines/virtual-machines-linux-dockerextension.md)

- **Azure Kubernetes Hizmeti**: Kapsayıcı uygulamaları çalıştırmak için önceden yapılandırılmış bir sanal makine kümesi oluşturmanıza, yapılandırmanıza ve yönetmenize olanak tanır. Azure Kubernetes Hizmeti hakkında daha fazla bilgi edinmek için [Azure Kubernetes Hizmeti tanıtımına](../../aks/intro-kubernetes.md)bakın.

  > **Ne zaman kullanılır**: Ek zamanlama ve yönetim araçları sağlayan üretime hazır, ölçeklenebilir ortamlar oluşturmanız gerektiğinde veya Docker Swarm kümesini dağıtırken.
  >
  > **Başlayın**: [Bir Kubernetes Hizmet kümesi dağıtın.](../../aks/tutorial-kubernetes-deploy-cluster.md)

- **Docker Machine**: Docker-machine komutlarını kullanarak sanal ana bilgisayarlara Docker Engine yüklemenizi ve yönetmeniz sağlar.

  >**Ne zaman kullanılır**: Tek bir Docker ana bilgisayar oluşturarak bir uygulamanın prototipini hızlı bir şekilde yapmanız gerektiğinde.

- **Uygulama Hizmeti için Özel Docker görüntüsü**: Linux'ta bir web uygulaması dağıttığınızda bir konteyner kayıt defterinden veya müşteri konteynerinden Docker kapsayıcılarını kullanmanıza olanak tanır.

  > **Ne zaman kullanılır**: Bir Docker resmine Linux'ta bir web uygulaması dağıtırken.
  >
  > **Başlayın:** [Linux'ta Uygulama Hizmeti için özel bir Docker görüntüsü kullanın.](../../app-service/containers/quickstart-docker-go.md)

### <a name="authentication"></a>Kimlik doğrulaması

Yalnızca uygulamalarınızı kimin kullandığını bilmekle birlikte, aynı zamanda kaynaklarınıza yetkisiz erişimi de önlemek için çok önemlidir. Azure, uygulama istemcilerinizin kimliğini doğrulamak için çeşitli yollar sağlar.

- **Azure Active Directory (Azure AD)**: Microsoft çok kiracılı, bulut tabanlı kimlik ve erişim yönetimi hizmeti. Azure AD ile tümleştirerek uygulamalarınıza tek oturum açma (SSO) ekleyebilirsiniz. Doğrudan Azure AD Graph API'sini veya Microsoft Graph API'sini kullanarak dizin özelliklerine erişebilirsiniz. OAuth2.0 yetkilendirme çerçevesi için Azure AD desteği ve Açık ID Bağlantısı ile yerel HTTP/REST bitiş noktalarını ve çok platformlu Azure AD kimlik doğrulama kitaplıklarını kullanarak entegre edebilirsiniz.

  > **Ne zaman kullanılır**: Bir SSO deneyimi sağlamak istediğinizde, Grafik tabanlı verilerle çalışın veya etki alanı tabanlı kullanıcıların kimliğini doğrulayın.
  >
  > **Başlayın**: Daha fazla bilgi edinmek için [Azure Active Directory geliştiricikılavuzuna](../../active-directory/develop/v2-overview.md)bakın.

- **Uygulama Hizmeti Kimlik Doğrulaması**: Uygulamanızı barındırmak için Uygulama Hizmeti'ni seçtiğinizde, Azure AD için yerleşik kimlik doğrulama desteğinin yanı sıra Facebook, Google, Microsoft ve Twitter gibi sosyal kimlik sağlayıcılarıda da alırsınız.

  > **Ne zaman kullanılır**: Azure AD,sosyal kimlik sağlayıcıları veya her ikisini birden kullanarak bir Uygulama Hizmeti uygulamasında kimlik doğrulamasını etkinleştirmek istediğinizde.
  >
  > **Başlayın**: Uygulama Hizmeti'nde kimlik doğrulama hakkında daha fazla bilgi edinmek için [Azure Uygulama Hizmeti'nde kimlik doğrulama ve yetkilendirme](../../app-service/overview-authentication-authorization.md)'ye bakın.

Azure'daki en iyi güvenlik uygulamaları hakkında daha fazla bilgi edinmek için [Azure güvenlik en iyi uygulamaları ve desenleri](../../security/fundamentals/best-practices-and-patterns.md)hakkında bilgi edinin.

### <a name="monitoring"></a>İzleme

Uygulamanız Azure'da çalışır durumda olduğundan, performansı izlemeniz, sorunları izlemeniz ve müşterilerin uygulamanızı nasıl kullandığını görmeniz gerekir. Azure çeşitli izleme seçenekleri sağlar.

-   **Uygulama Öngörüleri**: Canlı web uygulamalarınızı izlemek için Visual Studio ile entegre olan Azure barındırılan genişletilebilir bir analiz hizmetidir. Uygulamalarınızın performansını ve kullanılabilirliğini sürekli olarak geliştirmek için ihtiyacınız olan verileri sağlar. Bu iyileştirme, uygulamalarınızı Azure'da barındırıp barındırmamanız olur.

    >**Başlayın**: [Uygulama Öngörüleri öğreticisini](../../azure-monitor/app/app-insights-overview.md)takip edin.

-   **Azure Monitörü**: Azure altyapınız ve kaynaklarınız ile oluşturduğunuz ölçümleri ve günlükleri görselleştirmenize, sorgulamanıza, yönlendirmenize, arşivlemenize ve hareket etmenize yardımcı olan hizmettir. Monitör, Azure kaynaklarını izlemek için tek bir kaynaktır ve Azure portalında gördüğünüz veri görünümlerini sağlar.

    >**Başlayın**: [Azure Monitor ile başlayın.](../../monitoring-and-diagnostics/monitoring-get-started.md)

### <a name="devops-integration"></a>DevOps entegrasyonu

İster VM'ler sağlarken ister web uygulamalarınızı sürekli tümleştirmeyle yayımlamak olsun, Azure popüler DevOps araçlarının çoğuyla tümleşir. Sahip olduğunuz araçlarla çalışabilir ve aşağıdaki gibi araçlar için destekle mevcut deneyiminizi en üst düzeye çıkarabilirsiniz:

* Jenkins
* GitHub
* Puppet
* Chef
* TeamCity
* Ansible
* Azure DevOps

> **Başlayın**: Bir Uygulama Hizmeti uygulaması için DevOps seçeneklerini görmek için [Azure Uygulama Hizmetine Sürekli Dağıtım'a](../../app-service/deploy-continuous-deployment.md)bakın.
>
> **Şimdi deneyin:** [DevOps entegrasyonları birkaç deneyin.](https://azure.microsoft.com/try/devops/)


## <a name="azure-regions"></a>Azure bölgeleri

Azure, genellikle dünyanın birçok bölgesinde kullanılabilen küresel bir bulut platformudur. Azure'da bir hizmet, uygulama veya VM sağladığınızda, bir bölge seçmeniz istenir. Bu bölge, uygulamanızın çalıştığı veya verilerinizin depolandığı belirli bir veri merkezini temsil eder. Bu bölgeler, [Azure bölgeleri](https://azure.microsoft.com/regions/) sayfasında yayınlanan belirli konumlara karşılık gelir.

### <a name="choose-the-best-region-for-your-application-and-data"></a>Uygulamanız ve verileriniz için en iyi bölgeyi seçin

Azure'u kullanmanın avantajlarından biri, uygulamalarınızı dünyanın çeşitli veri merkezlerine dağıtabilmektir. Seçtiğiniz bölge uygulamanızın performansını etkileyebilir. Örneğin, ağ isteklerindeki gecikme süresini azaltmak için müşterilerinizin çoğuna daha yakın bir bölge seçmek daha iyidir. Uygulamanızı belirli ülkelerde/bölgelerde dağıtmak için yasal gereklilikleri karşılamak için bölgenizi seçmek de isteyebilirsiniz. Uygulama verilerini aynı veri merkezinde veya uygulamanızı barındıran veri merkezine mümkün olduğunca yakın bir veri merkezinde depolamak her zaman en iyi yöntemdir.

### <a name="multi-region-apps"></a>Çok bölgeli uygulamalar

Olası olmasa da, doğal afet veya Internet hatası gibi bir olay nedeniyle tüm bir veri merkezinin çevrimdışı olması imkansız değildir. Maksimum kullanılabilirlik sağlamak için birden fazla veri merkezinde önemli iş uygulamalarını barındırmak en iyi yöntemdir. Birden çok bölge kullanmak, genel kullanıcılar için gecikme süresini azaltabilir ve uygulamaları güncellerken esneklik için ek fırsatlar sağlayabilir.

Sanal Makine ve Uygulama Hizmetleri gibi bazı hizmetler, yüksek kullanılabilirlik liyakat kurumsal uygulamaları desteklemek için bölgeler arasında başarısız bir şekilde çok bölgeli desteği etkinleştirmek için [Azure Trafik Yöneticisi'ni](../../traffic-manager/traffic-manager-overview.md) kullanır. Örneğin, [Bkz. Azure başvuru mimarisi: Birden çok bölgede bir web uygulaması çalıştırın.](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region)

>**Ne zaman kullanılır**: Başarısız ve çoğaltmadan yararlanan kurumsal ve yüksek kullanılabilirlik uygulamaları nız olduğunda.

## <a name="how-do-i-manage-my-applications-and-projects"></a>Uygulamalarımı ve projelerimi nasıl yönetebilirim?

Azure, Azure kaynaklarınızı, uygulamalarınızı ve projelerinizi hem programlı hem de [Azure portalında](https://portal.azure.com/)oluşturmanız ve yönetmeniz için zengin bir deneyim kümesi sağlar.

### <a name="command-line-interfaces-and-powershell"></a>Komut hattı arayüzleri ve PowerShell

Azure, uygulamalarınızı ve hizmetlerinizi komut satırından yönetmenin iki yolunu sağlar. Bash, Terminal, komut istemi veya seçtiğiniz komut satırı aracı gibi araçları kullanabilirsiniz. Genellikle, sanal makineler, sanal ağlar, web uygulamaları ve diğer hizmetler oluşturma ve yapılandırma gibi komut satırından da aynı görevleri yapabilirsiniz.

-   [Azure Komut Satırı Arabirimi (CLI)](../../xplat-cli-install.md): Bir Azure aboneliğine bağlanmanızı ve komut satırından Azure kaynaklarına karşı çeşitli görevleri programlamanızı sağlar.

-   [Azure PowerShell](../../powershell-install-configure.md): Windows PowerShell'i kullanarak Azure kaynaklarını yönetmenize olanak tanıyan cmdletiçeren bir modül kümesi sağlar.

### <a name="azure-portal"></a>Azure portalında

[Azure portalı](https://portal.azure.com) web tabanlı bir uygulamadır. Azure portalını, Azure kaynakları ve hizmetleri oluşturmak, yönetmek ve kaldırmak için kullanabilirsiniz. Bu içerir:

* Yapılandırılabilir pano
* Azure kaynak yönetimi araçları
* Abonelik ayarlarına ve fatura bilgilerine erişim. Daha fazla bilgi için [Azure portalına genel bakış](../../azure-portal-overview.md)alabiliyorum.

### <a name="rest-apis"></a>REST API'leri

Azure, Azure portalı ui'sini destekleyen bir REST API kümesi üzerine oluşturulmuştür. Bu REST API'lerinin çoğu, Azure kaynaklarınızı ve uygulamalarınızı Internet özellikli herhangi bir cihazdan programlı olarak sağlamanıza ve yönetmenize olanak sağlamak için de desteklenir. REST API belgelerinin tamamı için [Azure REST SDK başvurusuna](https://docs.microsoft.com/rest/api/)bakın.

### <a name="apis"></a>API'ler

REST API'leri ile birlikte, birçok Azure hizmeti, aşağıdaki geliştirme platformları için SDK'lar da dahil olmak üzere platforma özgü Azure SDK'larını kullanarak uygulamalarınızdaki kaynakları programlı olarak yönetmenize de izin vetir:

-   [.NET](https://go.microsoft.com/fwlink/?linkid=834925)
-   [Node.js](https://docs.microsoft.com/azure/javascript/)
-   [Java](https://docs.microsoft.com/java/azure)
-   [PHP](https://github.com/Azure/azure-sdk-for-php/blob/master/README.md)
-   [Python](/azure/python/)
-   [Ruby](https://github.com/Azure/azure-sdk-for-ruby/blob/master/README.md)
-   [Git](https://docs.microsoft.com/azure/go)

[Mobil Uygulamalar](../../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md) ve [Azure Medya Hizmetleri](../../media-services/previous/media-services-dotnet-how-to-use.md) gibi hizmetler, web ve mobil istemci uygulamalarından hizmetlere erişmenize izin vermek için istemci tarafındaki SDK'lar sağlar.

### <a name="azure-resource-manager"></a>Azure Resource Manager

Uygulamanızı Azure'da çalıştırmak büyük olasılıkla birden çok Azure hizmetiyle çalışmayı içerir. Bu hizmetler aynı yaşam döngüsünü takip ve mantıksal bir birim olarak düşünülebilir. Örneğin, bir web uygulaması Web Apps, SQL Veritabanı, Depolama, Redis için Azure Önbelleği ve Azure İçerik Dağıtım Ağı hizmetlerini kullanabilir. [Azure Kaynak Yöneticisi,](../../azure-resource-manager/management/overview.md) uygulamanızdaki kaynaklarla grup olarak çalışmanıza olanak tanır. Tüm kaynakları tek bir eşgüdümlü işlemde dağıtabilir, güncelleyebilir veya silebilirsiniz.

Azure Kaynak Yöneticisi, ilgili kaynakları mantıksal olarak gruplandırma ve yönetmenin yanı sıra, ilgili kaynakların dağıtımını ve yapılandırmasını özelleştirmenize olanak sağlayan dağıtım özellikleri içerir. Örneğin, Kaynak Yöneticisi dağıtma ve bir uygulamayı yapılandırma kullanabilirsiniz. Bu uygulama, birden çok sanal makine, yük dengeleyicisi ve tek bir birim olarak Bir Azure SQL veritabanından oluşabilir.

Bu dağıtımları, JSON biçimlendirilmiş bir belge olan Azure Kaynak Yöneticisi şablonu kullanarak geliştirirsiniz. Şablonlar, komut dosyaları yerine bildirimşablonları kullanarak dağıtımı tanımlamanızı ve uygulamalarınızı yönetmenize izin verir. Şablonlarınız sınama, hazırlama ve üretim gibi farklı ortamlar için çalışabilir. Örneğin, repo'daki kodu tek bir tıklamayla bir Azure hizmeti kümesine dağıtan GitHub repo'ya düğme eklemek için şablonlar kullanabilirsiniz.

> **Ne zaman kullanılır**: UYGULAMANIZ İçİn REST API'leri, Azure CLI ve Azure PowerShell'i kullanarak programlı olarak yönetebileceğiniz şablon tabanlı bir dağıtım istediğinizde Kaynak Yöneticisi şablonlarını kullanın.
>
> **Başlayın**: Şablonları kullanmaya başlamak için Bkz. [Azure Kaynak Yöneticisi şablonları yazma.](../../resource-group-authoring-templates.md)

## <a name="understanding-accounts-subscriptions-and-billing"></a>Hesapları, abonelikleri ve faturalamayı anlama

Geliştiriciler olarak, kodun içine dalmayı ve uygulamalarımızın çalışmasını sağlayarak mümkün olduğunca hızlı bir şekilde başlamayı seviyoruz. Azure'da mümkün olduğunca kolay çalışmaya başlamanızı kesinlikle teşvik etmek istiyoruz. Azure, bunu kolaylaştırmak için [ücretsiz](https://azure.microsoft.com/free/)deneme sürümü sunar. Bazı hizmetlerde Azure [Uygulama Hizmeti](https://tryappservice.azure.com/)gibi "Ücretsiz olarak deneyin" işlevi bile vardır ve bu işlevbir hesap oluşturmanızı bile gerektirmez. Uygulamanızı kodlamaya ve Azure'a dağıtmaya dalmak ne kadar eğlenceli olsa da, Azure'un nasıl çalıştığını anlamak için biraz zaman almak da önemlidir. Özellikle, kullanıcı hesapları, abonelikler ve faturalandırma açısından nasıl çalıştığını anlamanız gerekir.

### <a name="what-is-an-azure-account"></a>Azure hesabı nedir?

Bir Azure aboneliği oluşturmak veya onunla çalışmak için bir Azure hesabınız olması gerekir. Azure hesabı, Azure AD'de veya Azure AD'nin güvendiği bir iş veya okul organizasyonu gibi bir dizinde yalnızca bir kimliktir. Böyle bir kuruluşa ait değilseniz, Azure AD tarafından güvenilen Microsoft Hesabınızı kullanarak her zaman bir abonelik oluşturabilirsiniz. Şirket içi Windows Server Active Directory'yi Azure AD ile tümleştirme hakkında daha fazla bilgi edinmek için, [şirket içi kimliklerinizi Azure Etkin Dizini ile tümleştirme](../../active-directory/hybrid/whatis-hybrid-identity.md)'ye bakın.

Her Azure aboneliği bir Azure AD örneğiyle güven ilişkisine sahiptir. Bu; Azure aboneliğinin kullanıcılar, hizmetler ve cihazlar için kimlik doğrulaması yapmak üzere bu dizine güvendiği anlamına gelir. Birden çok abonelik aynı dizine güvenebilir ancak bir abonelik yalnızca bir dizine güvenir. Daha fazla bilgi için Azure [aboneliklerinin Azure Etkin Dizini ile nasıl ilişkili olduğunu](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)öğrenin.

*Kullanıcılar*olarak da adlandırılan tek tek Azure hesap kimliklerini tanımlamanın yanı sıra, Azure AD'deki *grupları* tanımlayabilirsiniz. Kullanıcı grupları oluşturmak, rol tabanlı erişim denetimi (RBAC) kullanarak abonelikteki kaynaklara erişimi yönetmenin iyi bir yoludur. Grup oluşturmayı öğrenmek için Azure [Etkin Dizin önizlemesinde bir grup oluşturma'ya](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)bakın. Ayrıca [PowerShell'i kullanarak](../../active-directory/users-groups-roles/groups-settings-v2-cmdlets.md)gruplar oluşturabilir ve yönetebilirsiniz.

### <a name="manage-your-subscriptions"></a>Aboneliklerinizi yönetme

Abonelik, Azure hesabına bağlı Azure hizmetlerinin mantıksal bir gruplandırmasIdır. Tek bir Azure hesabı birden çok abonelik içerebilir. Azure hizmetlerinin faturalandırması abonelik başına yapılır. Türüne göre kullanılabilir abonelik tekliflerinin listesi için [Microsoft Azure Teklif Ayrıntıları'na](https://azure.microsoft.com/support/legal/offer-details/)bakın. Azure abonelikleri, abonelik üzerinde tam denetime sahip bir Hesap Yöneticisine sahiptir. Ayrıca, abonelikteki tüm hizmetleri denetleyen bir Hizmet Yöneticisi de vardır. Klasik abonelik yöneticileri hakkında bilgi için Azure [abonelik yöneticileri ekle veya değiştir'e](../../cost-management-billing/manage/add-change-subscription-administrator.md)bakın. Tek tek hesaplara [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md)kullanılarak Azure kaynaklarının ayrıntılı denetimi verilebilir.

#### <a name="resource-groups"></a>Kaynak grupları

Yeni Azure hizmetleri sağladığınızda, bunu belirli bir abonelikte yaparsınız. Kaynak olarak da adlandırılan tek tek Azure hizmetleri, bir kaynak grubu bağlamında oluşturulur. Kaynak grupları, uygulamanızın kaynaklarını dağıtmayı ve yönetmeyi kolaylaştırır. Kaynak grubu, bir birim olarak çalışmak istediğiniz uygulamanız için tüm kaynakları içermelidir. Kaynakları kaynak grupları ve hatta farklı abonelikler arasında taşıyabilirsiniz. Kaynakları taşıma hakkında bilgi edinmek [için](../../azure-resource-manager/management/move-resource-group-and-subscription.md)bkz.

Azure Kaynak Gezgini, aboneliğinizde zaten oluşturduğunuz kaynakları görselleştirmek için harika bir araçtır. Daha fazla bilgi edinmek [için kaynakları görüntülemek ve değiştirmek için Azure Kaynak Gezgini'ni kullanın'a](../../resource-manager-resource-explorer.md)bakın.

#### <a name="grant-access-to-resources"></a>Kaynaklara erişim izni verme

Azure kaynaklarına erişime izin verdiğinizde, kullanıcılara belirli bir görevi yapmak için gereken en az ayrıcalığı sağlamak her zaman en iyi yöntemdir.

- **Rol tabanlı erişim denetimi (RBAC)**: Azure'da, kullanıcı hesaplarına (asıllar) abonelik, kaynak grubu veya tek tek kaynaklar gibi belirli bir kapsamda erişim izni verebilirsiniz. RBAC, kaynakları bir kaynak grubuna dağıtmanıza ve belirli bir kullanıcıya veya gruba izin vermenize olanak tanır. Ayrıca, yalnızca hedef kaynak grubuna ait kaynaklara erişimi sınırlamanızı sağlar. Sanal makine veya sanal ağ gibi tek bir kaynağa erişim izni de verebilirsiniz. Erişim sağlamak için kullanıcıya, gruba veya hizmet ilkesine bir rol atarsınız. Önceden tanımlanmış birçok rol vardır ve kendi özel rollerinizi de tanımlayabilirsiniz. Daha fazla bilgi edinmek için [bkz.](../../role-based-access-control/overview.md)

  > **Ne zaman kullanılır**: Kullanıcılar ve gruplar için ince taneli erişim yönetimine ihtiyaç duyduğunuzda veya bir kullanıcıyı aboneliğin sahibi yapmanız gerektiğinde.
  >
  > **Başlayın**: Daha fazla bilgi edinmek için [RBAC ve Azure portalını kullanarak erişimi yönet'e](../../role-based-access-control/role-assignments-portal.md)bakın.

- **Hizmet temel nesneleri**: Kullanıcı ilkelerine ve gruplarına erişim sağlamanın yanı sıra, aynı hizmeti temel e-devlet eve de verebilirsiniz.

  > **Ne zaman kullanılır**: Azure kaynaklarını programlı bir şekilde yönetirken veya uygulamalara erişim izni verirken. Daha fazla bilgi için Active [Directory uygulaması ve hizmet ilkesi oluştur'a](../../active-directory/develop/howto-create-service-principal-portal.md)bakın.

#### <a name="tags"></a>Etiketler

Azure Kaynak Yöneticisi, tek tek kaynaklara özel etiketler atamanızı sağlar. Anahtar değer çiftleri olan etiketler, faturalandırma veya izleme için kaynakları düzenlemeniz gerektiğinde yararlı olabilir. Etiketler, birden çok kaynak grubundaki kaynakları izlemeniz için bir yol sağlar. Etiketleri aşağıdaki yollarla atayabilirsiniz:

* Portalda
* Azure Kaynak Yöneticisi şablonunda
* REST API’sini kullanma
* Azure CLI kullanma
* PowerShell’i kullanma

Her kaynağa birden çok etiket atayabilirsiniz. Daha fazla bilgi edinmek için Azure [kaynaklarınızı düzenlemek için etiketleri kullanma'ya](../../resource-group-using-tags.md)bakın.

### <a name="billing"></a>Faturalandırma

Şirket içi bilgi işlemden bulut barındırılan hizmetlere geçerken, hizmet kullanımını ve ilgili maliyetleri izlemek ve tahmin etmek önemli endişelerdir. Aylık olarak çalıştırılmak için hangi yeni kaynakların maliyetini tahmin etmek önemlidir. Ayrıca, geçerli harcamalara bağlı olarak faturalandırmanın belirli bir ayı nasıl göründüğünü de tahmin edebilirsiniz.

#### <a name="get-resource-usage-data"></a>Kaynak kullanım verilerini alma

Azure, Azure abonelikleri için kaynak tüketimine ve meta veri bilgilerine erişim sağlayan bir dizi FaturaLANDıRMA REST API'si sağlar. Bu Faturalandırma AP'leri, Azure maliyetlerini daha iyi tahmin etme ve yönetme olanağı sağlar. Harcamaları saatlik artışlarla izleyebilir ve analiz edebilir ve harcama uyarıları oluşturabilirsiniz. Ayrıca, geçerli kullanım eğilimlerine göre gelecekteki faturalandırmayı da tahmin edebilirsiniz.

>**Başlayın:** Faturalama API'lerini kullanma hakkında daha fazla bilgi edinmek için [Azure Fatura Kullanımı ve RateCard API'lerine genel bakış](../../cost-management-billing/manage/usage-rate-card-overview.md)'a bakın.

#### <a name="predict-future-costs"></a>Gelecekteki maliyetleri tahmin edin

Maliyetleri önceden tahmin etmek zor olsa da, Azure'da yardımcı olabilecek araçlar vardır. Dağıtılan kaynakların maliyetini tahmin etmeye yardımcı olacak bir [fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) vardır. Portaldaki Faturalandırma kaynaklarını ve FaturaLAMA REST API'lerini, geçerli tüketime bağlı olarak gelecekteki maliyetleri tahmin etmek için de kullanabilirsiniz.

>**Başlayın**: [Azure Fatura Kullanımı ve RateCard API'leri genel bakış](../../cost-management-billing/manage/usage-rate-card-overview.md)bakın.
