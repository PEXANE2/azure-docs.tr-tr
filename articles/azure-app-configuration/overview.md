---
title: Azure Uygulama Yapılandırması nedir?
description: Azure Uygulama Yapılandırma hizmetine genel bakış.
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: overview
ms.date: 02/19/2020
ms.openlocfilehash: 1f1cec68813d33e7fa19a414a30adfc9a41df91f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77523484"
---
# <a name="what-is-azure-app-configuration"></a>Azure Uygulama Yapılandırması nedir?

Azure Uygulama Yapılandırması, uygulama ayarlarını ve özellik bayraklarını merkezi olarak yönetmek için bir hizmet sağlar. Modern programlar, özellikle bir bulutta çalışan programlar, genellikle doğada dağıtılan birçok bileşene sahiptir. Yapılandırma ayarlarını bu bileşenler arasında yaymak, uygulama dağıtımı sırasında sorun giderme de zor giderme hatalarına neden olabilir. Uygulamanızın tüm ayarlarını depolamak ve erişimlerini tek bir yerde güvence altına almak için Uygulama Yapılandırmasını kullanın.

## <a name="why-use-app-configuration"></a>Neden App Configuration'ı kullanıyorsun?

Bulut tabanlı uygulamalar genellikle birden çok bölgede birden çok sanal makine veya kapsayıcıda çalışır ve birden çok harici hizmet kullanır. Dağıtılmış bir ortamda sağlam ve ölçeklenebilir bir uygulama oluşturmak önemli bir zorluk sunar.

Çeşitli programlama metodolojileri, geliştiricilerin bina uygulamalarının artan karmaşıklığıyla başa çıkmalarına yardımcı olur. Örneğin, [Oniki Faktör uygulaması,](https://12factor.net/) bulut uygulamalarında kullanılmak üzere iyi test edilmiş birçok mimari desenleri ve en iyi uygulamaları açıklar. Bu kılavuzdan önemli bir öneri, yapılandırmayı koddan ayırmaktır. Bir uygulamanın yapılandırma ayarları çalıştırılabilir dışında tutulmalı ve çalışma zamanı ortamından veya harici bir kaynaktan okunmalıdır.

Herhangi bir uygulama Uygulama Yapılandırmasından yararlanabilse de, aşağıdaki örnekler uygulamanın kullanımından yararlanan uygulama türleridir:

* Azure Kubernetes Hizmeti, Azure Hizmet Kumaşı veya bir veya daha fazla coğrafyada dağıtılan diğer konteyner uygulamaları temel alan mikro hizmetler
* Azure İşlevlerini veya diğer olay tabanlı durum dışı bilgi işlem uygulamalarını içeren sunucusuz uygulamalar
* Sürekli dağıtım boru hattı

Uygulama Yapılandırması aşağıdaki avantajları sunar:

* Dakikalar içinde kurulabilen tam yönetilen bir hizmet
* Esnek anahtar gösterimleri ve haritalamalar
* Etiketlerle etiketleme
* Ayarların zamanında tekrarı
* Özellik bayrak yönetimi için özel ui
* Özel tanımlı boyutlarda iki yapılandırma kümesinin karşılaştırılması
* Azure tarafından yönetilen kimlikler aracılığıyla gelişmiş güvenlik
* Hassas bilgilerin istirahatte ve aktarımsırasında şifrelemesi
* Popüler çerçevelerle yerel entegrasyon

Uygulama Yapılandırması, uygulama sırlarını depolamak için kullanılan [Azure Anahtar Kasası'nı](https://azure.microsoft.com/services/key-vault/)tamamlar. Uygulama Yapılandırması, aşağıdaki senaryoların uygulanmasını kolaylaştırır:

* Farklı ortamlar ve coğrafyalar için hiyerarşik yapılandırma verilerinin yönetimini ve dağıtımını merkezileştirme
* Bir uygulamayı yeniden dağıtmaya veya yeniden başlatmaya gerek kalmadan uygulama ayarlarını dinamik olarak değiştirin
* Gerçek zamanlı olarak özellik kullanılabilirliğini kontrol edin

## <a name="use-app-configuration"></a>Uygulama Yapılandırması'nı kullanma

Uygulamanıza Bir Uygulama Yapılandırma deposu eklemenin en kolay yolu, Microsoft tarafından sağlanan bir istemci kitaplığıdır. Seçtiğiniz dil ve çerçeveye bağlı olarak, uygulamanızla bağlantı kurmak için aşağıdaki yöntemler mevcuttur

| Programlama dili ve çerçevesi | Bağlanma |
|---|---|
| .NET Core ve ASP.NET Core | .NET Core için Uygulama Yapılandırma sağlayıcısı |
| .NET Çerçeve ve ASP.NET | .NET için Uygulama Yapılandırma oluşturucusu |
| Java Spring | Spring Cloud için Uygulama Yapılandırma istemcisi |
| Diğer | Uygulama YapılandırmaSı REST API |

## <a name="next-steps"></a>Sonraki adımlar

* [ASP.NET Core quickstart](./quickstart-aspnet-core-app.md)
* [.NET Çekirdek hızlı başlat](./quickstart-dotnet-core-app.md)
* [.NET Framework quickstart](./quickstart-dotnet-app.md)
* [Azure İşlevleri hızlı başlat](./quickstart-azure-functions-csharp.md)
* [Java Bahar quickstart](./quickstart-java-spring-app.md)
* [ASP.NET Core özelliği bayrak quickstart](./quickstart-feature-flag-aspnet-core.md)
* [Bahar Önyükleme özelliği bayrak quickstart](./quickstart-feature-flag-spring-boot.md)
