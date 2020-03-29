---
title: Uzaktan İzleme çözümü mimari seçenekleri - Azure | Microsoft Dokümanlar
description: Bu makalede, Uzaktan İzleme'de yapılan mimari ve teknik seçimler açıklanmaktadır.
author: timlaverty
manager: camerons
ms.author: timlav
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/20/2018
ms.topic: conceptual
ms.openlocfilehash: 1bd08596a30db7322a72b4269fddfe0b9df19119
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61447185"
---
# <a name="remote-monitoring-architectural-choices"></a>Uzaktan İzleme mimari seçenekleri

Azure IoT Uzaktan İzleme çözüm hızlandırıcısı açık kaynak kodlu, MIT lisanslı, çözüm hızlandırıcıdır. IoT geliştirme sürecinizi hızlandırmanıza yardımcı olmak için, şu gibi yaygın IoT senaryoları gösterir:

- Cihaz bağlantısı
- Cihaz yönetimi
- Akış işleme

Uzaktan İzleme çözümü, önerilen [Azure IoT başvuru mimarisini](https://aka.ms/iotrefarchitecture)izler.

Bu makalede, Uzaktan İzleme alt sistemlerinin her birinde yapılan temel mimari ve teknik seçimler açıklanmaktadır. Ancak, Microsoft'un Uzaktan İzleme çözümünde yaptığı teknik seçimler, uzaktan izleme IoT çözümünü uygulamanın tek yolu değildir. Teknik uygulamayı başarılı bir uygulama oluşturmak için bir temel olarak kabul etmeli ve aşağıdakileri yapmalısınız:

- Kuruluşunuzdaki mevcut beceri ve deneyimi sığdırın.
- Dikey uygulama ihtiyaçlarınızı karşılar.

## <a name="architectural-choices"></a>Mimari seçenekleri

Microsoft'un bir IoT uygulaması için önerdiği mimari bulut, microservice ve sunucusuz tabanlıdır. Bir IoT uygulamasının farklı alt sistemlerini, bağımsız olarak dağıtabileceğiniz ve ölçeklendirebileceğiniz ayrı hizmetler olarak oluşturmalısınız. Bu öznitelikler, tek tek alt sistemleri güncelleştirmede daha fazla ölçek, daha fazla esneklik sağlar ve her alt sistem için uygun bir teknoloji seçme esnekliği sağlar.

Birden fazla teknoloji kullanarak mikro hizmetleri uygulayabilirsiniz. Örneğin, bir mikro hizmeti uygulamak için aşağıdaki seçeneklerden birini seçebilirsiniz:

- Azure Fonksiyonları gibi sunucusuz teknolojiye sahip Docker gibi bir kapsayıcı teknolojisi kullanın.
- Azure Uygulama Hizmetleri gibi PaaS hizmetlerinde mikro hizmetlerinizi barındırın.

## <a name="technology-choices"></a>Teknoloji seçimleri

Bu bölümde, temel alt sistemlerin her biri için Uzaktan İzleme çözümünde yapılan teknoloji seçimleri ayrıntılı olarak anlatılıyor.

![Çekirdek Diyagramı](./media/iot-accelerators-remote-monitoring-architectural-choices/subsystem.png)

### <a name="cloud-gateway"></a>Bulut Ağ Geçidi

Azure IoT Hub, Uzaktan İzleme çözümü bulut ağ geçidi olarak kullanılır. [IoT Hub,](https://azure.microsoft.com/services/iot-hub/) cihazlarla güvenli, çift yönlü iletişim sunar.

IoT aygıt bağlantısı için şunları kullanabilirsiniz:

- IoT Hub aygıtı, aygıtınız için yerel bir istemci uygulaması uygulamak için [SDK'lar.](../iot-hub/iot-hub-devguide-sdks.md#azure-iot-hub-device-sdks) SDK'lar IoT Hub REST API'nin etrafında paketleyiciler sunar ve yeniden deneme ler gibi senaryoları işler.
- Aygıtlarınızda kapsayıcılarda çalışan özel modülleri dağıtmak ve yönetmek için Azure IoT Edge ile tümleştirme.
- Bağlı cihazları toplu olarak yönetmek için IoT Hub'da otomatik aygıt yönetimi ile entegrasyon.

### <a name="stream-processing"></a>Akış işleme

Akış işleme için Uzaktan İzleme çözümü, karmaşık kural işleme için Azure Akış Analizi'ni kullanır. Daha basit kurallar kullanmak istiyorsanız, bu kurulum kullanıma hazır dağıtımın bir parçası olmasa da, basit kural işleme desteğine sahip özel bir microservice vardır. Başvuru mimarisi, basit kural işleme için Azure İşlevleri ve karmaşık kural işleme için Azure Akış Analizi önerir.

### <a name="storage"></a>Depolama

Depolama için Uzaktan İzleme çözüm hızlandırıcısı hem Azure Time Series Öngörülerini hem de Azure Cosmos DB'yi kullanır. Azure Time Series Insights, IoT Hub üzerinden gelen iletileri bağlı aygıtlarınızdan saklar. Çözüm hızlandırıcı, soğuk depolama, kural tanımları, uyarılar ve yapılandırma ayarları gibi diğer tüm depolama alanları için Azure Cosmos DB kullanır.

Azure Cosmos DB, IoT uygulamaları için önerilen genel amaçlı sıcak depolama çözümüdür. Ancak, Azure Zaman Serisi Öngörüleri ve Azure Veri Gölü gibi çözümler birçok kullanım durumu için uygundur. Azure Time Series Öngörüleri ile eğilimleri ve anormallikleri tespit ederek zaman serisi sensör verilerinize daha derin bakışaçısılar elde edebilirsiniz. Bu özellik, kök neden analizleri yapmanızı ve maliyetli kapalı kalma sürelerinden kaçınmanızı sağlar.

> [!NOTE]
> Zaman Serisi Öngörüleri şu anda Azure Çin bulutunda kullanılamıyor. Azure Çin bulutundaki yeni Uzaktan İzleme çözüm hızlandırıcı dağıtımları, tüm depolama için Cosmos DB'yi kullanır.

### <a name="business-integration"></a>İş entegrasyonu

Uzaktan İzleme çözümündeki iş entegrasyonu, sıcak depolama alanına yerleştirilen uyarıların üretimiyle sınırlıdır. Daha derin iş tümleştirme senaryoları uygulamak için çözümü Azure Logic Apps ile bağlayın.

### <a name="user-interface"></a>Kullanıcı Arabirimi

Web UI JavaScript React ile oluşturulmuşdur. React yaygın olarak kullanılan bir endüstri web Web Web çerçevesi sunar ve Açısal gibi diğer popüler çerçeveler benzer.

### <a name="runtime-and-orchestration"></a>Çalışma zamanı ve orkestrasyon

Uzaktan İzleme çözümü, yatay ölçek için orkestratör olarak Kubernetes ile alt sistemleri çalıştırmak için Docker kapları kullanır. Bu mimari, her alt sistem için ayrı ölçek tanımları sağlar. Ancak, bu mimari sanal makineleri ve kapsayıcıları güncel ve güvenli tutmak için DevOps maliyetleri ne tabidir yok.

Docker'a alternatifler arasında Azure Uygulama Hizmeti gibi PaaS hizmetlerinde mikro hizmetler barındırma yer alıyor. Kubernetes alternatifleri Service Fabric, DC / OS veya Swarm gibi orkestratörleri içerir.

## <a name="next-steps"></a>Sonraki adımlar

* Uzaktan İzleme çözümünüzü [buradan](https://www.azureiotsolutions.com/)dağıtın.
* [C#](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) ve [Java'da](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java/)GitHub kodunu keşfedin.  
* Burada IoT Başvuru [Mimarisi](https://aka.ms/iotrefarchitecture)hakkında daha fazla bilgi edinin.
