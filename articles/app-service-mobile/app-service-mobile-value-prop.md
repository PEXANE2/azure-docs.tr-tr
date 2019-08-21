---
title: Azure Uygulama Hizmetinde Mobile Apps Hakkında
description: App Service’in kurumsal mobil uygulamalarınıza sağladığı avantajları öğrenin.
services: app-service\mobile
documentationcenter: ''
author: elamalani
manager: yochayk
editor: ''
ms.assetid: 4e96cb9d-a632-4cf6-8219-0810d8ade3f9
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-multiple
ms.devlang: na
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: a3817b3e9f62a863f3fea9305ef169db2eb84491
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69637955"
---
# <a name="getting-started"> </a>Azure Uygulama Hizmetinde Mobile Apps Hakkında

> [!NOTE]
> Visual Studio App Center, mobil uygulama geliştirmeye yönelik yeni ve tümleşik hizmetler merkezi 'ne yatırım yapıyor. Geliştiriciler, sürekli tümleştirme ve teslim işlem hattı ayarlamak için **oluşturma**, **Test** etme ve **dağıtma** hizmetlerini kullanabilir. Uygulama dağıtıldıktan sonra, geliştiriciler **analiz** ve **Tanılama** hizmetlerini kullanarak uygulamasının durumunu ve kullanımını izleyebilir ve **Push** hizmetini kullanarak kullanıcılarla etkileşime geçebilir. Geliştiriciler, uygulama verilerini bulutta kalıcı hale getirmek ve eşitlemek için kullanıcıların ve **veri** hizmetinin kimliklerini doğrulamak üzere **kimlik** doğrulamasından faydalanabilir. Bugün [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-value-prop) kullanıma alma.
>

Azure Uygulama Hizmeti, profesyonel geliştiricilere yönelik tam olarak yönetilen bir [hizmet olarak platform](https://azure.microsoft.com/overview/what-is-paas/) (PaaS) teklifidir. Bu hizmet, web, mobil ve tümleştirme senaryoları için zengin bir özellik kümesi sağlar. 

Azure Uygulama Hizmeti’ndeki Mobile Apps özelliği, kurumsal geliştiriciler ve sistem entegratörleri için yüksek düzeyde ölçeklenebilir ve küresel olarak kullanılabilir bir platformdur.

![Mobile Apps özelliklerine görsel bir genel bakış](./media/app-service-mobile-value-prop/overview.png)

## <a name="why-mobile-apps"></a>Neden Mobile Apps?
Mobile Apps özelliği ile şunları yapabilirsiniz:

* **Yerel ve platformlar arası uygulamalar oluşturun**: Yerel iOS, Android ve Windows uygulamaları ya da platformlar arası Xamarin veya Cordova (PhoneGap) uygulamaları oluşturuyor olmanız fark etmeksizin, yerel SDK 'Ları kullanarak App Service avantajlarından faydalanabilirsiniz.
* **Kurumsal sistemlerinize bağlanın**: Mobile Apps özelliği sayesinde, dakikalar içinde kurumsal oturum açma ekleyebilir ve şirket içi veya bulut kaynaklarınıza bağlanabilirsiniz.
* **Veri eşitleme ile çevrimdışı kullanmaya yönelik uygulamalar oluşturun**: Çevrimdışı çalışan uygulamalar oluşturarak mobil iş gücünüzü daha üretken hale getirin ve kurumsal veri kaynaklarınızdan veya hizmet olarak yazılım (SaaS) API 'Lerinde bağlantı varsa arka planda verileri eşitlemek için Mobile Apps kullanın.
* **Saniyeler içinde milyonlarca kullanıcıya anında iletme bildirimleri gönderin**: Müşterilerinize tüm cihazlarda anında iletme bildirimleri ile, ihtiyaçlarına göre kişiselleştirilmesi ve zaman hakkı geldiğinde gönderilmesini sağlar.

## <a name="mobile-apps-features"></a>Mobile Apps özellikleri
Aşağıdaki özellikler, bulut etkin mobil geliştirme için önemlidir:

* **Kimlik doğrulama ve yetkilendirme**: Kurumsal kimlik doğrulaması için Azure Active Directory ve Facebook, Google, Twitter ve Microsoft hesapları gibi sosyal sağlayıcılar gibi kimlik sağlayıcıları için destek. Mobile Apps tüm sağlayıcılar için bir OAuth 2.0 hizmeti sunar. Ayrıca sağlayıcıya özel işlev için kimlik sağlayıcısına SDK tümleştirebilirsiniz.

    [Kimlik doğrulama özelliklerimiz] hakkında daha fazlasını keşfedin.

* **Veri erişimi**: Mobile Apps, Azure SQL veritabanı veya şirket içi SQL Server ile bağlantılı, mobil olarak kolay bir OData v3 veri kaynağı sağlar. Bu hizmet, [Azure Tablo depolama], MongoDB ve [Azure Cosmos DB]’nin yanı sıra Office 365 ve Salesforce.com gibi SaaS API’si sağlayıcıları dahil, diğer NoSQL ve SQL veri sağlayıcılarıyla kolayca tümleştirmenizi sağlayarak Entity Framework’ü temel alabilir.

* **Çevrimdışı eşitleme**: İstemci SDK 'Ları, çevrimdışı bir veri kümesi ile çalışan sağlam ve hızlı mobil uygulamalar oluşturmanızı kolaylaştırır. Bu veri kümesini, çakışma çözümü desteği de dahil olmak üzere arka uç verileriyle otomatik olarak eşitleyebilirsiniz.

  [Veri özellikleri] hakkında daha fazlasını keşfedin.

* **Anında iletme bildirimleri**: İstemci SDK 'Ları, Azure Notification Hubs 'ın kayıt özellikleri ile sorunsuz bir şekilde tümleşir, böylece milyonlarca kullanıcıya aynı anda anında iletme bildirimleri gönderebilirsiniz.

  [Anında iletme bildirimi özellikleri] hakkında daha fazlasını keşfedin.

* **Istemci SDK 'ları**: Yerel geliştirmeyi ([iOS], [Android]ve [Windows]), platformlar arası geliştirmeyi ([Xamarin.iOS ve Xamarin.Android], [Xamarin.Forms]) ve karma uygulamayı kapsayan, tam bir istemci SDK 'sı kümesi vardır Geliştirme ([Apache Cordova]). Her istemci SDK’sı ile bir MIT lisansı ile birlikte sunulur ve açık kaynaklıdır.

## <a name="azure-app-service-features"></a>Azure Uygulama Hizmeti özellikleri
Aşağıdaki platform özellikleri mobil üretim siteleri için yararlıdır:

* **Otomatik ölçeklendirme**: App Service, tüm gelen müşteri yükünü işlemek için hızla ölçeklendirebilir veya ölçeklendirebilirsiniz. Yük ya da zamanlama temelinden mobil uygulamanızın arka ucunu ölçeklendirmek için VM’nin sayısını ya da boyutunu el ile seçin ya da otomatik ölçeklendirmeyi ayarlayın.

  [Otomatik ölçeklendirme] hakkında daha fazlasını keşfedin.

* **Hazırlama ortamları**: App Service sitenizin birden fazla sürümünü çalıştırabilir, böylece bir/B testi gerçekleştirebilir, daha büyük bir DevOps planının parçası olarak üretimde test edebilir ve yeni bir arka ucun yerinde hazırlanmasını sağlayabilirsiniz.

  [hazırlık ortamları] hakkında daha fazlasını keşfedin.

* **Sürekli dağıtım**: App Service, ortak _kaynak denetimi yönetimi_ (SCM) sistemleriyle tümleştirilebilir ve arka ucun yeni bir sürümünü kolayca dağıtmanıza olanak tanır.

  [dağıtım seçenekleri](../app-service/deploy-local-git.md) hakkında daha fazlasını keşfedin.

* **Sanal ağ**: App Service, sanal ağ, Azure ExpressRoute ya da karma bağlantılar kullanarak şirket içi kaynaklara bağlanabilir.

  [karma bağlantılar], [sanal ağlar], ve [ExpressRoute] hakkında daha fazlasını keşfedin.

* **Yalıtılmış ve ayrılmış ortamlar**: Azure App Service uygulamaları güvenli bir şekilde çalıştırmak için, tam yalıtılmış ve ayrılmış bir ortamda App Service çalıştırabilirsiniz. Bu ortam, büyük ölçekli, yalıtım veya güvenli ağ erişimi gerektiren uygulama iş yükleri için idealdir.

  [App Service ortamları] hakkında daha fazlasını öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Uygulama Hizmeti'nde Mobile Apps kullanmaya başlamak için [başlarken] öğreticisini tamamlayın. Bu öğretici, tercih ettiğiniz mobil arka ucu ve istemciyi oluşturma konusunda temel kavramları kapsar. Ayrıca kimlik doğrulama, çevrimdışı eşitleme ve anında iletme bildirimlerini tümleştirme konularını ele alır. Öğreticiyi her istemci uygulaması için birden çok kez tamamlayabilirsiniz.

Mobile Apps hakkında daha fazla bilgi için [öğrenme haritamızı] gözden geçirin.
Azure Uygulama Hizmeti platformu hakkında daha fazla bilgi için bkz. [Azure uygulama hizmeti].

<!-- URLs. -->
[Migrate your mobile service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[başlarken]: app-service-mobile-ios-get-started.md
[Azure Tablo Depolama]:../cosmos-db/table-storage-how-to-use-dotnet.md
[Azure Cosmos DB]: ../cosmos-db/sql-api-get-started.md
[Kimlik doğrulama özelliklerimiz]: ./app-service-mobile-auth.md
[veri özellikleri]: ./app-service-mobile-offline-data-sync.md
[anında iletme bildirimi özellikleri]: ../notification-hubs/notification-hubs-push-notification-overview.md
[iOS]: ./app-service-mobile-ios-how-to-use-client-library.md
[Android]: ./app-service-mobile-android-how-to-use-client-library.md
[Windows]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin.iOS ve Xamarin.Android]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin.Forms]: ./app-service-mobile-xamarin-forms-get-started.md
[Apache Cordova]: ./app-service-mobile-cordova-how-to-use-client-library.md
[otomatik ölçeklendirme]: ../app-service/manage-scale-up.md
[hazırlık ortamları]: ../app-service/deploy-staging-slots.md
[karma bağlantılar]: ../biztalk-services/integration-hybrid-connection-overview.md
[sanal ağlar]: ../app-service/web-sites-integrate-with-vnet.md
[ExpressRoute]: ../app-service/environment/app-service-app-service-environment-network-configuration-expressroute.md
[App Service ortamları]: ../app-service/environment/intro.md
[öğrenme haritamızı]: https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/
[Azure uygulama hizmeti]: ../app-service/overview.md
