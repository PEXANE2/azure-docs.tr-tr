---
title: Cihaz simülasyonuna genel bakış - Azure | Microsoft Dokümanlar
description: Cihaz Simülasyonu çözüm hızlandırıcısının ve yeteneklerinin açıklaması.
author: dominicbetts
manager: philmea
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.custom: mvc
ms.date: 12/03/2018
ms.author: dobett
ms.openlocfilehash: f58eb05ed582cf18157a76f4d637d72a228f4e96
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65464890"
---
# <a name="device-simulation-solution-accelerator-overview"></a>Cihaz Simülasyonu çözüm hızlandırıcısına genel bakış

Bulut tabanlı bir IoT çözümünde, aygıtlarınız sıcaklık, konum ve durum gibi telemetrigöndermek için bulut bitiş noktasına bağlanır. Çözümünüz bu telemetriyi tüketerek eylemde kalmanızı veya ondan öngörüler elde etmenizi sağlar.

Bir IoT Çözümü geliştirdiğinizde, deneme ve test bu sürecin temel parçalarıdır. Simülasyon bu süreç boyunca önemli bir araçtır. Cihaz Simülasyonu ile şunları yapabilirsiniz:

* Hızlı bir şekilde bir prototip olsun ve çalışan ve daha sonra anında simüle cihaz davranışı ayarlayarak yineleyin. Bu işlem, pahalı donanımyatırım yapmadan önce fikri kanıtlamak sağlar. Saniyeler içinde bir prototip aygıtı oluşturmak için web kullanıcı aracı üzerinden özel aygıtlar oluşturabilirsiniz.
* Gerçek aygıt davranışlarını simüle ederek çözümü cihazdan çözüme beklendiği gibi doğrulayın. Gerçekçi simüle telemetri oluşturmak için JavaScript kullanarak karmaşık aygıt davranışları komut dosyası olabilir.
* Normal, tepe noktası ve en yüksek yük koşullarını simüle ederek çözümünüzü ölçeklendirin. Ölçek testleri, çözümünüzü çalıştırmak için gereken Azure kaynaklarını doğru boyutlandırmanıza da yardımcı olur.

![Örnek drone simülasyonu](media/iot-accelerators-device-simulation-overview/dronesimulation.png)

Aygıt Simülasyonu ile, gerçek cihazlarınızı simüle etmek için aygıt modellerini tanımlayabilirsiniz. Bu model ileti biçimlerini, ikiz özelliklerini ve yöntemleri içerir. JavaScript ile karmaşık aygıt davranışlarını da simüle edebilirsiniz.

Herhangi bir IoT hub'ına bağlanan bir ila binlerce aygıt için simülasyonlar çalıştırabilirsiniz. Teste yardımcı olmak için, tek başına bir ortam için Aygıt Simülasyonu ile birlikte isteğe bağlı olarak bir IoT hub'ı dağıtabilirsiniz.

Cihaz Simülasyonu ücretsizdir. Ancak, Aygıt Simülasyonu bulutta Azure aboneliğinize dağıtır ve Azure kaynaklarını tüketir. Aygıt Simülasyonu gereksinimlerinizi karşılamıyorsa, kaynak kodu kopyalayıp değiştirmeniz için [GitHub'da da kullanılabilir.](https://github.com/Azure/device-simulation-dotnet)

## <a name="sample-simulations"></a>Örnek simülasyonlar

Aygıt Simülasyonu'ni dağıttığınızda, bazı örnek simülasyonlar ve örnek aygıtlar elde elabilirsiniz. Aygıt Simülasyonu'nun nasıl kullanılacağını öğrenmek için bu örnekleri kullanabilirsiniz. Başlamak için, [10 kamyon simüle](quickstart-device-simulation-deploy.md)eden bir örnek simülasyon çalıştırın. Ayrıca [sağlanan birçok örnek cihazlardan birini kullanarak kendi simülasyon oluşturabilirsiniz.](iot-accelerators-device-simulation-create-simulation.md)

![Benzetim yapılandırması](media/iot-accelerators-device-simulation-overview/samplesimulation1.png)

## <a name="custom-simulated-devices"></a>Özel simüle edilmiş cihazlar

Aygıt Simülasyonu'nu kullanarak simülasyonlarınızda kullanmak üzere [özel aygıt modelleri oluşturabilirsiniz.](iot-accelerators-device-simulation-create-custom-device.md) Örneğin, sıcaklık ve nem telemetrisi gönderen yeni bir buzdolabı cihazı modeli tanımlayabilirsiniz. Özel simüle edilmiş aygıtlar, rastgele, artan veya atere telemetri değerlerine sahip basit aygıt davranışları için idealdir.

![Cihaz modelini oluşturma](media/iot-accelerators-device-simulation-overview/adddevicemodel.png)

## <a name="advanced-simulated-devices"></a>Gelişmiş simüle edilmiş cihazlar

Bir aygıtın gönderdiği telemetri değerleri üzerinde daha fazla denetime ihtiyacınız olduğunda, gelişmiş bir aygıt modeli kullanabilirsiniz. Gelişmiş aygıt modelleri, gönderilen telemetri değerlerini işlemek için JavaScript desteğini etkinleştirin. Örneğin, sıcak güneşli bir günde park halindeki bir arabanın iç sıcaklığını simüle edebilirsiniz - dış sıcaklık arttıkça, iç sıcaklık katlanarak artar.

Gelişmiş aygıt modelleri, JSON aygıt tanım dosyası ve ilgili JavaScript dosyalarından oluşan [kendi aygıt modellerinizi oluşturmanıza ve yüklemenize](iot-accelerators-device-simulation-advanced-device.md) olanak tanır.

Gelişmiş cihaz modelleri şunları sağlar:

* Telemetri türleri ile birlikte aygıttan gönderilen ileti biçimini belirtin.
* Zaman içinde aygıtın durumunu koruyan telemetri değerleri oluşturmak için özel komut dosyası kullanın.
* Benzetimli aygıtın yöntemlere nasıl yanıt veriş olduğunu belirtmek için özel komut dosyası kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Aygıt Simülasyonu çözüm hızlandırıcısını ve yeteneklerini öğrendiniz. Çözüm hızlandırıcısını kullanmaya başlamak için hızlı başlatmaya devam edin:

> [!div class="nextstepaction"]
> [Azure'da bir IoT aygıt simülasyonu dağıtma ve çalıştırma](quickstart-device-simulation-deploy.md)
