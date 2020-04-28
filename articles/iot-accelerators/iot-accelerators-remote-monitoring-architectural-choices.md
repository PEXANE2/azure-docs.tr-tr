---
title: Uzaktan Izleme çözümü mimari seçimleri-Azure | Microsoft Docs
description: Bu makalede, uzaktan Izlemede yapılan mimari ve teknik seçimler açıklanmaktadır
author: timlaverty
manager: camerons
ms.author: timlav
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/20/2018
ms.topic: conceptual
ms.openlocfilehash: 1bd08596a30db7322a72b4269fddfe0b9df19119
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "61447185"
---
# <a name="remote-monitoring-architectural-choices"></a>Uzaktan İzleme mimari seçenekleri

Azure IoT uzaktan Izleme çözümü hızlandırıcısı, açık kaynaklı, MıT lisanslanmış ve çözüm hızlandırıcısıdır. IoT geliştirme sürecinizi hızlandırmanıza yardımcı olmak için şu gibi yaygın IoT senaryolarını gösterir:

- Cihaz bağlantısı
- Cihaz yönetimi
- Akış işleme

Uzaktan Izleme çözümü, önerilen [Azure IoT başvuru mimarisini](https://aka.ms/iotrefarchitecture)izler.

Bu makalede, uzak Izleme alt sistemlerinde yapılan temel mimari ve teknik seçimler açıklanmaktadır. Ancak, uzaktan izleme çözümünde yapılan Microsoft Teknik seçimleri, uzaktan izleme IoT çözümünü kullanmanın tek yoludur. Teknik uygulamayı başarılı bir uygulama oluşturmaya yönelik bir taban çizgisi olarak görmeniz gerekir ve bunu şöyle değiştirmelisiniz:

- Kuruluşunuzdaki kullanılabilir becerileri ve deneyimi uygun hale göre yapın.
- Dikey uygulama gereksinimlerinizi karşılayın.

## <a name="architectural-choices"></a>Mimari seçenekleri

Microsoft 'un bir IoT uygulaması için önerdiği mimari, bulutta yerel, mikro hizmet ve sunucusuz tabanlıdır. Bir IoT uygulamasının farklı alt sistemlerini ayrı ayrı dağıtabileceğiniz ve ölçeklendirebilmeniz için ayrı hizmetler olarak oluşturmanız gerekir. Bu öznitelikler daha fazla ölçeklendirmeyi etkinleştirir, tek alt sistemleri güncelleştirmede daha fazla esneklik sağlar ve her alt sistem için uygun bir teknoloji seçme esnekliği sağlar.

Mikro Hizmetleri birden fazla teknoloji kullanarak uygulayabilirsiniz. Örneğin, bir mikro hizmeti uygulamak için aşağıdaki seçeneklerden birini seçebilirsiniz:

- Azure Işlevleri gibi sunucusuz teknolojiyle Docker gibi bir kapsayıcı teknolojisini kullanın.
- Mikro hizmetlerinizi Azure uygulama hizmetleri gibi PaaS hizmetlerinde barındırın.

## <a name="technology-choices"></a>Teknoloji seçimleri

Bu bölümde, çekirdek alt sistemlerinin her biri için uzaktan Izleme çözümünde yapılan teknoloji seçimleri ayrıntılı olarak sunulur.

![Temel diyagram](./media/iot-accelerators-remote-monitoring-architectural-choices/subsystem.png)

### <a name="cloud-gateway"></a>Bulut ağ geçidi

Azure IoT Hub, uzaktan Izleme çözümü bulut ağ geçidi olarak kullanılır. [IoT Hub](https://azure.microsoft.com/services/iot-hub/) cihazlarla güvenli, çift yönlü iletişim sağlar.

IoT cihaz bağlantısı için şunu kullanabilirsiniz:

- Cihazınız için bir yerel istemci uygulaması uygulamak üzere [IoT Hub cihaz SDK 'ları](../iot-hub/iot-hub-devguide-sdks.md#azure-iot-hub-device-sdks) . SDK 'lar IoT Hub REST API etrafında sarmalayıcılar sunar ve yeniden denemeler gibi senaryoları işler.
- Cihazlarınızdaki kapsayıcılar üzerinde çalışan özel modülleri dağıtmak ve yönetmek için Azure IoT Edge ile tümleştirme.
- Bağlı cihazları toplu olarak yönetmek için IoT Hub ' de otomatik cihaz yönetimiyle tümleştirme.

### <a name="stream-processing"></a>Akış işleme

Akış işleme için, uzaktan Izleme çözümü karmaşık kural işleme için Azure Stream Analytics kullanır. Daha basit kurallar kullanmak istiyorsanız, basit kural işleme desteğiyle özel bir mikro hizmet vardır, ancak bu kurulum, kullanıma hazır dağıtımın bir parçası değildir. Başvuru mimarisi basit kural işleme için Azure Işlevleri ve karmaşık kural işleme için Azure Stream Analytics önerir.

### <a name="storage"></a>Depolama

Depolama için, uzaktan Izleme çözümü Hızlandırıcısı hem Azure Time Series Insights hem de Azure Cosmos DB kullanır. Azure Time Series Insights, bağlı cihazlarınızdan IoT Hub üzerinden gelen iletileri depolar. Çözüm Hızlandırıcısı, soğuk depolama, kural tanımları, uyarılar ve yapılandırma ayarları gibi diğer tüm depolama için Azure Cosmos DB kullanır.

Azure Cosmos DB IoT uygulamaları için önerilen genel amaçlı ısınma depolama çözümüdür. Ancak, Azure Time Series Insights ve Azure Data Lake gibi çözümler birçok kullanım durumu için uygundur. Azure Time Series Insights, eğilimleri ve anormallikleri birleştirerek zaman serisi algılayıcı verilerinize daha derin Öngörüler elde edebilirsiniz. Bu özellik, kök neden analizleri gerçekleştirmenizi ve maliyetli kapalı kalma süresini önlemenize olanak sağlar.

> [!NOTE]
> Time Series Insights Azure Çin bulutu 'nda Şu anda kullanılamıyor. Azure Çin bulutu 'ndaki yeni uzaktan Izleme çözümü Hızlandırıcısı dağıtımları tüm depolama için Cosmos DB kullanır.

### <a name="business-integration"></a>İş tümleştirmesi

Uzaktan Izleme çözümünde iş tümleştirmesi, normal depolamaya yerleştirilmiş uyarıların nesli ile sınırlıdır. Daha derin iş tümleştirme senaryolarını uygulamak için çözümü Azure Logic Apps bağlayın.

### <a name="user-interface"></a>Kullanıcı Arabirimi

Web Kullanıcı arabirimi, JavaScript 'e tepki vererek oluşturulur. Yanıt verme, yaygın olarak kullanılan bir sektör Web UI çerçevesi sunar ve angular gibi diğer popüler çerçevelerle benzerdir.

### <a name="runtime-and-orchestration"></a>Çalışma zamanı ve düzenleme

Uzaktan Izleme çözümü, Kubernetes ile alt sistemleri yatay ölçek için Orchestrator olarak çalıştırmak için Docker kapsayıcılarını kullanır. Bu mimari her alt sistem için ayrı ölçek tanımlarının kullanılmasına izin vermez. Ancak bu mimari, sanal makineleri ve kapsayıcıları güncel ve güvenli tutmak için DevOps maliyetlerine neden olacak.

Docker alternatifleri, Azure App Service gibi PaaS hizmetlerinde mikro hizmetleri barındırmakla birlikte bulunur. Kubernetes alternatifleri Service Fabric, DC/OS veya Sısınma gibi düzenleyiciler içerir.

## <a name="next-steps"></a>Sonraki adımlar

* Uzaktan Izleme çözümünüzü [buraya](https://www.azureiotsolutions.com/)dağıtın.
* [C#](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) ve [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java/)'da GitHub kodunu araştırma.  
* IoT [başvuru mimarisi hakkında](https://aka.ms/iotrefarchitecture)daha fazla bilgi edinin.
