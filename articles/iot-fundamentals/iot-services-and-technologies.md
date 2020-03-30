---
title: Azure Nesnelerin İnterneti (IoT) teknolojileri ve çözümleri
description: Azure IoT çözümü oluşturmak için kullanabileceğiniz teknoloji ve hizmet koleksiyonunu açıklar.
author: dominicbetts
ms.service: iot-fundamentals
services: iot-fundamentals
ms.topic: overview
ms.date: 01/15/2020
ms.author: dobett
ms.openlocfilehash: 0b04b5170c13f6f6c3fd74976461f03e4367060a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77046055"
---
# <a name="azure-technologies-and-services-for-creating-iot-solutions"></a>IoT çözümleri oluşturmak için azure teknolojileri ve hizmetleri

Azure IoT teknolojileri ve hizmetleri, kuruluşunuz için dijital dönüşüm sağlayan çok çeşitli IoT çözümleri oluşturmanız için seçenekler sunar. Örneğin, şunları yapabilirsiniz:

- Güvenli, kurumsal kalitede bir IoT çözümü oluşturmak ve dağıtmak için yönetilen bir IoT uygulama platformu olan [Azure IoT Central'ı](https://apps.azureiotcentral.com)kullanın. IoT Central, çözüm geliştirme sürecinizi hızlandırmak için perakende ve sağlık hizmetleri gibi sektöre özel uygulama şablonları koleksiyonuna sahiptir.
- Uzaktan izleme veya tahmine dayalı bakım gibi ortak bir IoT senaryosu uygulamak için Azure IoT [çözüm hızlandırıcısının](https://www.azureiotsolutions.com) açık kaynak kod tabanını genişletin.
- Sıfırdan özel bir IoT çözümü oluşturmak için [Azure IoT Hub](../iot-hub/about-iot-hub.md) ve Azure [IoT aygıt SDK'ları](../iot-hub/iot-hub-devguide-sdks.md) gibi Azure IoT platform hizmetlerini kullanın.

![Azure IoT teknolojileri, hizmetleri ve çözümleri](./media/iot-services-and-technologies/iot-technologies-services.png)

## <a name="azure-iot-central"></a>Azure IoT Central

[IoT Central uygulama platformu,](https://apps.azureiotcentral.com) kurumsal kalitede IoT çözümleri geliştirme, yönetme ve sürdürme yükünü ve maliyetini azaltır. IoT Central'ın özelleştirilebilir web ui'si, cihaz koşullarını izlemenize, kurallar oluşturmanıza ve yaşam döngüleri boyunca milyonlarca cihazı ve verilerini yönetmenize olanak tanır. IoT Central içindeki API yüzeyi, IoT çözümünüzü yapılandırmanız ve bunlarla etkileşimde bulmanız için programlı erişim sağlar.

Azure IoT Central, özel IoT çözümleri oluşturmak için kullanabileceğiniz tam olarak yönetilen bir uygulama platformudur. IoT Central, çözümler oluşturmak için uygulama şablonlarını kullanır. Genel çözümler ve enerji, sağlık, devlet ve perakende gibi belirli endüstriler için şablonlar vardır. IoT Merkezi uygulama şablonları, temalar, panolar ve görünümlerle özelleştirebileceğiniz bir IoT Merkezi uygulamasını dakikalar içinde dağıtmanıza izin tanır.

Çözümünüze hızlı bir şekilde bağlanmak [için Azure Sertifikalı IoT aygıt kataloğundan](https://catalog.azureiotsolutions.com) aygıtlar seçin. Cihazlarınızı sağlıklı ve bağlı tutmak için cihazlarınızı izlemek ve yönetmek için IoT Merkezi web Kullanıcı Birliğini kullanın. IoT Central uygulamanızı diğer iş uygulamalarıyla tümleştirmek için bağlayıcıları ve API'leri kullanın.

Tam olarak yönetilen bir uygulama platformu olarak IoT Central basit ve öngörülebilir bir fiyatlandırma modeline sahiptir.

## <a name="azure-iot-solution-accelerators"></a>Azure IoT çözüm hızlandırıcıları

[Azure IoT çözüm hızlandırıcıları,](https://www.azureiotsolutions.com) özelleştirilebilir kurumsal kalite çözümler topluluğu. Bu çözümleri olduğu gibi dağıtabilir veya açık kaynak Kodlu Java veya .NET kaynak kodunu kullanarak özel bir IoT çözümü geliştirebilirsiniz.

Azure IoT çözüm hızlandırıcıları, IoT çözümünüz üzerinde yüksek düzeyde denetim sağlar. Çözüm hızlandırıcıları, Azure aboneliğinize dakikalar içinde dağıtabileceğiniz yaygın IoT senaryoları için önceden oluşturulmuş çözümleri içerir. Senaryolar şunlardır:

  - Uzaktan izleme
  - Bağlı fabrika
  - Tahmine dayalı bakım
  - Cihaz benzetimi

Tüm çözüm hızlandırıcıları için açık kaynak kod tabanı GitHub'da kullanılabilir. Belirli IoT gereksinimlerinizi karşılamak için bir çözüm hızlandırıcısını özelleştirmek için kodu indirin.

Çözüm hızlandırıcıları, Azure aboneliğinizde yönetmeniz gereken Azure IoT Hub ve Azure Depolama gibi Azure hizmetlerini kullanır.

## <a name="custom-solutions"></a>Özel çözümler

Sıfırdan bir IoT çözümü oluşturmak veya IoT Central veya çözüm hızlandırıcısı kullanılarak oluşturulan bir çözümü genişletmek için aşağıdaki Azure IoT teknoloji ve hizmetlerinden birini veya birkaçını kullanın:

### <a name="devices"></a>Cihazlar

IoT aygıtlarınızı [Azure IoT Başlangıç Kitlerinden](https://catalog.azureiotsolutions.com/kits) birini kullanarak geliştirin veya [IoT için Azure Sertifikalı aygıt kataloğundan](https://catalog.azureiotsolutions.com)kullanmak üzere bir aygıt seçin. Katıştırılmış kodunuzu açık kaynak [aygıt SDK'larını](../iot-hub/iot-hub-devguide-sdks.md)kullanarak uygulayın. Aygıt SDK'ları Linux, Windows ve gerçek zamanlı işletim sistemleri gibi birden çok işletim sistemi destekler. [C,](https://github.com/Azure/azure-iot-sdk-c) [Node.js,](https://github.com/Azure/azure-iot-sdk-node) [Java](https://github.com/Azure/azure-iot-sdk-java), [.NET](https://github.com/Azure/azure-iot-sdk-csharp)ve [Python](https://github.com/Azure/azure-iot-sdk-python)gibi birden çok programlama dili için SDK'lar vardır.

[IoT Tak ve Çalıştır Önizleme](../iot-pnp/overview-iot-plug-and-play.md) hizmetini kullanarak aygıtlarınız için gömülü kodu oluşturma şeklinizi daha da basitleştirebilirsiniz. IoT Tak ve Çalıştır, çözüm geliştiricilerin insaları gömülü kod yazmadan aygıtları çözümleriyle tümleştirmelerine olanak tanır. IoT Tak ve Çalıştır'ın özünde, aygıt özelliklerini açıklayan bir _aygıt yeteneği modeli_ şeması yer alan bir model şeması yer alan bir modeldir. Katıştırılmış aygıt kodunuzu oluşturmak ve IoT Merkezi uygulaması gibi bulut tabanlı bir çözümü yapılandırmak için aygıt yetenek modelini kullanın.

[Azure IoT Edge,](../iot-edge/about-iot-edge.md) IoT iş yükünüzün parçalarını Azure bulut hizmetlerinizden aygıtlarınıza boşaltmanıza olanak tanır. IoT Edge, çözümünüzdeki gecikme süresini azaltabilir, aygıtlarınızın bulutla alışverişi yapılan veri miktarını azaltabilir ve çevrimdışı senaryoları etkinleştirebilir. IoT Edge cihazlarını IoT Central'dan ve bazı çözüm hızlandırıcılarından yönetebilirsiniz.

[Azure Sphere,](https://docs.microsoft.com/azure-sphere/product-overview/what-is-azure-sphere) Internet'e bağlı cihazlar için yerleşik iletişim ve güvenlik özelliklerine sahip güvenli, üst düzey bir uygulama platformudur. Güvenli bir mikrodenetleyici ünitesi, özel bir Linux tabanlı işletim sistemi ve sürekli, yenilenebilir güvenlik sağlayan bulut tabanlı bir güvenlik hizmeti içerir.

### <a name="cloud-connectivity"></a>Bulut bağlantısı

[Azure IoT Hub](../iot-hub/about-iot-hub.md) hizmeti, milyonlarca IoT aygıtı ve bulut tabanlı bir çözüm arasında güvenilir ve güvenli çift yönlü iletişim sağlar. [Azure IoT Hub Aygıt Sağlama Hizmeti,](../iot-dps/about-iot-dps.md) IoT Hub için yardımcı bir hizmettir. Hizmet, insan müdahalesi gerektirmeden cihazların doğru IoT hub'ına sıfır dokunuşla, tam zamanında sağlanmasını sağlar. Bu özellikler, müşterilerin milyonlarca cihazı güvenli ve ölçeklenebilir bir şekilde sağlamasını sağlar.

IoT Hub çözüm hızlandırıcılarının temel bileşenidir ve aşağıdakiler gibi IoT uygulama zorluklarını karşılamak için kullanabilirsiniz:

* Yüksek hacimli cihaz bağlantısı ve yönetimi.
* Yüksek hacimli telemetri alımı.
* Cihazların komuta ve denetimi.
* Cihaz güvenliği uygulama.

### <a name="bridging-the-gap-between-the-physical-and-digital-worlds"></a>Fiziksel ve dijital dünyalar arasındaki boşluğu doldurma

[Azure Digital Twins,](../digital-twins/about-digital-twins.md) fiziksel bir ortamı modellemenizi sağlayan bir IoT hizmetidir. İnsanlar, boşluklar ve aygıtlar arasındaki ilişkileri modellemek için bir uzamsal zeka grafiği kullanır. Dijital ve fiziksel dünyalar arasında verileri bir arada tutarak bağlamsal olarak farkında çözümler oluşturabilirsiniz.

Iot Central, gerçek dünyadaki cihazları ve verileri kullanıcıların bağlı cihazları izlemesini ve yönetmesini sağlayan dijital modellerle senkronize etmek için dijital ikizleri kullanır.

### <a name="data-and-analytics"></a>Veri ve analiz

IoT aygıtları genellikle sensörlerden gelen sıcaklık ölçümleri gibi büyük miktarda zaman serisi veri üretir. [Azure Time Series Öngörüleri](../time-series-insights/time-series-insights-overview.md) bir IoT hub'ına bağlanabilir, aygıtlarınızdan gelen telemetri akışını okuyabilir, bu verileri depolayabilir ve sorgulayıp görselleştirmenizi sağlayabilir.

[Azure Haritalar,](/azure/azure-maps) web ve mobil uygulamalara doğru coğrafi bağlam sağlamak için yeni haritalama verileri kullanan bir coğrafi hizmetler topluluğudur. Uygulamalarınızı oluşturmak için REST API, web tabanlı JavaScript denetimi veya Android SDK kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalı bir deneyim için, hızlı başlangıçlardan birini deneyin:

- [Azure IoT Central uygulaması oluşturma](../iot-central/core/quick-deploy-iot-central.md)
- [Bir aygıttan ioT hub'ına telemetri gönderme](../iot-hub/quickstart-send-telemetry-cli.md)
- [Bulut tabanlı uzaktan izleme çözümünü deneyin](../iot-accelerators/quickstart-remote-monitoring-deploy.md)
