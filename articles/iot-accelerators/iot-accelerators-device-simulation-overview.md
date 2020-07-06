---
title: Cihaz simülasyonu genel bakış-Azure | Microsoft Docs
description: Cihaz benzetimi çözüm hızlandırıcısının açıklaması ve özellikleri.
author: dominicbetts
manager: philmea
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.custom: mvc
ms.date: 12/03/2018
ms.author: dobett
ms.openlocfilehash: f58eb05ed582cf18157a76f4d637d72a228f4e96
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "65464890"
---
# <a name="device-simulation-solution-accelerator-overview"></a>Cihaz Simülasyonu çözüm hızlandırıcısına genel bakış

Bulut tabanlı bir IoT çözümünde, cihazlarınız sıcaklık, konum ve durum gibi telemetri göndermek için bir bulut uç noktasına bağlanır. Çözümünüz bu Telemetriyi tüketir ve bu telemetri aracılığıyla eylemler gerçekleştirebilir veya bundan Öngörüler türetebilirsiniz.

IoT çözümü geliştirirken, deneme ve test bu işlemin önemli parçalarından biridir. Benzetim, bu süreç boyunca önemli bir araçtır. Cihaz benzetimi ile şunları yapabilirsiniz:

* Hızlı bir şekilde bir prototip oluşturun ve çalıştırın ve sonra sanal cihaz davranışını anında ayarlayarak yineleyin. Bu işlem, maliyetli donanıma yatırım yapmadan önce fikri kanıtlamanızı sağlar. Saniyeler içinde bir prototip cihaz oluşturmak için Web Kullanıcı arabirimi aracılığıyla özel cihazlar oluşturabilirsiniz.
* Gerçek dünyada cihaz davranışlarına benzetim yaparak, çözümün cihazdan çözüme beklendiğini doğrulayın. Gerçekçi sanal telemetri oluşturmak için JavaScript kullanarak karmaşık cihaz davranışlarına komut dosyası ekleyebilirsiniz.
* Normal, yoğun ve en yoğun yük koşullarına benzetilerek çözümünüze test ölçeklendirin. Ölçek testleri, çözümünüzü çalıştırmak için gereken Azure kaynaklarını doğru boyuta getirmenize de yardımcı olur.

![Örnek drone simülasyonu](media/iot-accelerators-device-simulation-overview/dronesimulation.png)

Cihaz benzetimi ile gerçek cihazlarınızın benzetimini yapmak için cihaz modellerini tanımlayabilirsiniz. Bu model İleti biçimlerini, ikizi özelliklerini ve yöntemlerini içerir. JavaScript ile karmaşık cihaz davranışlarının benzetimini de yapabilirsiniz.

Herhangi bir IoT Hub 'ına bağlanan binlerce cihaza yönelik benzetimleri çalıştırabilirsiniz. Test ile ilgili yardım için, isteğe bağlı olarak tek başına bir ortam için cihaz benzetimine sahip bir IoT Hub 'ı dağıtabilirsiniz.

Cihaz benzetimi ücretsizdir. Ancak, cihaz benzetimi Bulutta Azure aboneliğinize dağıtılır ve Azure kaynaklarını kullanır. Cihaz benzetimi gereksinimlerinizi karşılamıyorsa, kopyalama ve değiştirme için [GitHub 'da kaynak kodu da mevcuttur](https://github.com/Azure/device-simulation-dotnet) .

## <a name="sample-simulations"></a>Örnek benzetimler

Cihaz simülasyonu dağıtırken, bazı örnek benzetimleri ve örnek cihazları alırsınız. Bu örnekleri, cihaz benzetimi kullanımını öğrenmek için kullanabilirsiniz. Başlamak için [10 structuralks benzetimi yapan bir örnek simülasyonu](quickstart-device-simulation-deploy.md)çalıştırın. Ayrıca, [belirtilen birçok örnek cihazdan birini kullanarak kendi simülasyoninizi oluşturabilirsiniz](iot-accelerators-device-simulation-create-simulation.md).

![Benzetim yapılandırması](media/iot-accelerators-device-simulation-overview/samplesimulation1.png)

## <a name="custom-simulated-devices"></a>Özel sanal cihazlar

Benzetimlerinizde kullanmak üzere [özel cihaz modelleri oluşturmak](iot-accelerators-device-simulation-create-custom-device.md) Için cihaz simülasyonu ' nu kullanabilirsiniz. Örneğin, sıcaklık ve nem telemetrisi gönderen yeni bir soğutucu cihaz modeli tanımlayabilirsiniz. Özel sanal cihazlar, telemetri değerlerini rastgele, artırma veya azaltma ile basit cihaz davranışları için idealdir.

![Cihaz modelini oluşturma](media/iot-accelerators-device-simulation-overview/adddevicemodel.png)

## <a name="advanced-simulated-devices"></a>Gelişmiş sanal cihazlar

Bir cihazın gönderdiği telemetri değerleri üzerinde daha fazla denetime ihtiyacınız olduğunda, gelişmiş bir cihaz modeli kullanabilirsiniz. Gelişmiş cihaz modelleri, gönderilen telemetri değerlerini değiştirmek için JavaScript desteğini etkinleştirir. Örneğin, dış sıcaklık arttığı için, bir park eden arabasının iç sıcaklığını taklit edebilirsiniz. dış sıcaklık arttıkça, iç sıcaklık katlanarak üstel olarak artar.

Gelişmiş cihaz modelleri, bir JSON cihaz tanımı dosyasından ve ilgili JavaScript dosyalarından oluşan [kendi cihaz modellerinizi oluşturup karşıya yüklemenizi](iot-accelerators-device-simulation-advanced-device.md) sağlar.

Gelişmiş cihaz modelleri şunları yapmanızı sağlar:

* Cihazdan, telemetri türleriyle birlikte gönderilen ileti biçimini belirtin.
* Zaman içinde cihazın durumunu koruyacak telemetri değerlerini oluşturmak için özel betik kullanımı kullanın.
* Sanal cihazın yöntemlere nasıl yanıt verdiğini belirtmek için özel betik kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede cihaz benzetimi Çözüm Hızlandırıcısı ve özellikleri hakkında bilgi edindiniz. Çözüm hızlandırıcıyı kullanmaya başlamak için hızlı başlangıç ile devam edin:

> [!div class="nextstepaction"]
> [Azure 'da IoT cihaz benzetimi dağıtma ve çalıştırma](quickstart-device-simulation-deploy.md)
