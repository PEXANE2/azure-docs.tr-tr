---
title: Azure Bulut Hizmetleri Nedir | Microsoft Dokümanlar
description: Azure Bulut Hizmetleri'nin ne olduğu hakkında bilgi edinin.
services: cloud-services
author: tgore03
ms.service: multiple
ms.topic: article
ms.date: 04/19/2017
ms.author: tagore
ms.openlocfilehash: c531e02656c9f6342670024b2220386e789a2d98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75386859"
---
# <a name="overview-of-azure-cloud-services"></a>Azure Bulut Hizmetlerine Genel Bakış
Azure Bulut Hizmetleri, hizmet olarak bir [platforma](https://azure.microsoft.com/overview/what-is-paas/) (PaaS) örnektir. [Azure Uygulama Hizmeti](../app-service/overview.md)gibi bu teknoloji de ölçeklenebilir, güvenilir ve kullanımı ucuz uygulamaları desteklemek üzere tasarlanmıştır. Uygulama Hizmetinin sanal makinelerde (VM) barındırıldığı gibi, Azure Bulut Hizmetleri de öyle. Ancak, VM'ler üzerinde daha fazla denetime sahipsiniz. Azure Bulut Hizmetleri'ni kullanan VM'lere kendi yazılımınızı yükleyebilir ve bunlara uzaktan erişebilirsiniz.

![Azure Bulut Hizmetleri diyagramı](./media/cloud-services-choose-me/diagram.png)

Daha fazla kontrol aynı zamanda daha az kullanım kolaylığı anlamına gelir. Ek denetim seçeneklerine ihtiyacınız olmadığı sürece, Uygulama Hizmeti'nin Web Apps özelliğinde Azure Bulut Hizmetleri'ne kıyasla bir web uygulamasını çalışır hale getirmek genellikle daha hızlı ve daha kolaydır.

İki tür Azure Bulut Hizmeti rolü vardır. İkisi arasındaki tek fark, rolünüzün VM'lerde nasıl barındırılan olduğudur:

* **Web rolü**: Uygulamanızı IIS üzerinden otomatik olarak dağıtır ve barındırabilir.

* **İşçi rolü**: IIS kullanmaz ve uygulamanızı tek başına çalıştırın.

Örneğin, basit bir uygulama, bir web sitesine hizmet veren tek bir web rolü kullanabilir. Daha karmaşık bir uygulama, kullanıcılardan gelen istekleri işlemek için bir web rolü kullanabilir ve ardından bu istekleri işleme için bir alt role geçirebilir. (Bu [iletişim, Azure Hizmet Veri Servisi](../service-bus-messaging/service-bus-messaging-overview.md) veya Azure Sıra depolama [alanı](../storage/common/storage-introduction.md)kullanabilir.)

Önceki şekilde belirtildiği gibi, tek bir uygulamadaki tüm VM'ler aynı bulut hizmetinde çalışır. Kullanıcılar uygulamaya tek bir genel IP adresi üzerinden erişir ve istekleri niçin uygulamanın VM'leri arasında otomatik olarak yüklenir. Platform, Bir Azure Bulut Hizmetleri uygulamasındaki VM'leri tek bir donanım hatası noktasını önleyebilecek şekilde [ölçeklendirin ve dağıtıyor.](cloud-services-how-to-scale-portal.md)

Uygulamalar VM'lerde çalıştırılabilse de, Azure Bulut Hizmetleri'nin hizmet olarak altyapıyı (IaaS) değil, PaaS sağladığını anlamak önemlidir. Bunu düşünmenin bir yolu var. Azure Sanal Makineler gibi IaaS ile, önce uygulamanızın çalıştığı ortamı oluşturur ve yapılandırırsınız. Daha sonra uygulamanızı bu ortama dağısınız. Her VM işletim sisteminin yeni yamalı sürümlerini dağıtmak gibi şeyler yaparak bu dünyanın büyük bir kısmını yönetmekten siz sorumlusunuz. PaaS'ta, tam tersine, sanki ortam zaten var. Tek yapmanız gereken uygulamanızı dağıtmak. İşletim sisteminin yeni sürümlerini dağıtma da dahil olmak üzere çalıştığı platformun yönetimi sizin için işlenir.

## <a name="scaling-and-management"></a>Ölçekleme ve yönetim
Azure Bulut Hizmetleri ile sanal makineler oluşturmazsınız. Bunun yerine, Azure'a "üç web rol örneği" ve "iki alt rol örneği" gibi her birini istediğinizden kaç tane istediğinizi söyleyen bir yapılandırma dosyası sağlarsınız. Platform daha sonra sizin için oluşturur. Hala bu destek VM'lerin [boyutunun ne](cloud-services-sizes-specs.md) olması gerektiğini seçersiniz, ancak bunları açıkça kendiniz oluşturamazsınız. Uygulamanızın daha büyük bir yükü işlemesi gerekiyorsa, daha fazla VM isteyebilirsiniz ve Azure bu örnekleri oluşturur. Yük azalırsa, bu örnekleri kapatabilir ve ödemeyapmayı durdurabilirsiniz.

Azure Bulut Hizmetleri uygulaması genellikle iki adımlı bir işlem le kullanıcıların kullanımına sunulur. Bir geliştirici uygulamayı ilk olarak platformun evreleme alanına [yükler.](cloud-services-how-to-create-deploy-portal.md) Geliştirici uygulamayı canlı yapmaya hazır olduğunda, düzenlemeyi üretimle değiştirmek için Azure portalını kullanır. [Evreleme ve üretim arasındaki](cloud-services-how-to-manage-portal.md#swap-deployments-to-promote-a-staged-deployment-to-production) bu geçiş, çalışan bir uygulamanın kullanıcılarını rahatsız etmeden yeni bir sürüme yükseltilmesine olanak tanıyan hiçbir kapalı kalma süresi olmadan yapılabilir.

## <a name="monitoring"></a>İzleme
Azure Bulut Hizmetleri de izleme sağlar. Sanal Makineler gibi, başarısız bir fiziksel sunucu algılar ve yeni bir makinede bu sunucuda çalışan VM'leri yeniden başlatır. Ancak Azure Bulut Hizmetleri, başarısız VM'leri ve uygulamaları da algılar, sadece donanım hatalarını değil. Sanal Makineler aksine, her web ve işçi rolü içinde bir aracı vardır ve bu yüzden hatalar oluştuğunda yeni VM'ler ve uygulama örnekleri başlatmak mümkün.

Azure Bulut Hizmetleri'nin PaaS doğasının başka etkileri de vardır. En önemlilerinden biri, herhangi bir web veya işçi rol örneği başarısız olduğunda bu teknoloji üzerinde inşa edilen uygulamaların doğru çalışacak şekilde yazılması gerektiğidir. Bunu başarmak için, bir Azure Bulut Hizmetleri uygulaması kendi VM'lerinin dosya sisteminde durumu korumamalıdır. Sanal Makinelerle oluşturulan Sanal Makineler'den farklı olarak, Azure Bulut Hizmetleri Sanal MM'lerine yapılan yazılar kalıcı değildir. Sanal Makineler veri diski gibisi yoktur. Bunun yerine, bir Azure Bulut Hizmetleri uygulaması açıkça Azure SQL Veritabanı, blobs, tablolar veya başka bir dış depolama tüm durumu yazmalıdır. Uygulamaları bu şekilde oluşturmak, azure Bulut Hizmetleri'nin hem de önemli hedefleri olan ölçeklendirmeyi kolaylaştırır ve hataya karşı daha dayanıklıdır.

## <a name="next-steps"></a>Sonraki adımlar
* [.NET'te bulut hizmeti uygulaması oluşturma](cloud-services-dotnet-get-started.md) 
* [Düğüm.js'de bulut hizmeti uygulaması oluşturma](cloud-services-nodejs-develop-deploy-app.md) 
* [PHP'de bulut hizmeti uygulaması oluşturma](../cloud-services-php-create-web-role.md) 
* [Python'da bulut hizmeti uygulaması oluşturma](cloud-services-python-ptvs.md)






