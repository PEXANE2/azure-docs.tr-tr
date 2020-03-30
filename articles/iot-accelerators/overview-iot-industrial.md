---
title: Azure endüstriyel IoT'ye genel bakış | Microsoft Dokümanlar
description: Bu makalede, endüstriyel IoT genel bir bakış sağlar. IIoT'deki bağlı fabrikayı, fabrika zemin bağlantısını ve güvenlik bileşenlerini açıklar.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: c5d70e5a8784e88d3f2906d500768735dced1ee0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73828139"
---
# <a name="what-is-industrial-iot-iiot"></a>Endüstriyel IoT (IIoT) nedir

IIoT Nesnelerin Endüstriyel İnternetidir. IIoT, IoT'nin imalat sanayiinde uygulanması yla endüstriyel verimliliği artırır. 

## <a name="improve-industrial-efficiencies"></a>Endüstriyel verimliliği artırmak

Bağlı bir fabrika çözüm hızlandırıcısı ile operasyonel üretkenliğinizi ve karlılığınızı artırın. Fabrika zemininde halihazırda çalışan makineleriniz de dahil olmak üzere endüstriyel ekipmanınızı ve cihazlarınızı bulutta bağlayın ve izleyin. Tüm fabrika tabanının performansını artırmanıza yardımcı olacak öngörüler için IoT verilerinizi analiz edin.

OPC Twin ile fabrika zemin makinelerine erişim sürecini azaltın ve zamanınızı IIoT çözümleri oluşturmaya odaklayın. Sertifika yönetimini ve Endüstriyel varlık entegrasyonunu OPC Vault ile kolaylaştırın ve varlık bağlantısının güvence altına alınacağından emin olun. Bu mikro hizmetler, [Azure Endüstriyel IoT bileşenlerinin](https://github.com/Azure/azure-iiot-opc-ua)üzerinde REST benzeri bir API sağlar. Hizmet API'si, kenar modülü işlevselliğini kontrol sağlar. 

![Endüstriyel IoT’ye genel bakış](media/overview-iot-industrial/overview.png)

> [!NOTE]
> Azure Endüstriyel IoT hizmetleri hakkında daha fazla bilgi için GitHub [deposuna](https://github.com/Azure/azure-iiot-services)bakın.
Azure IoT Edge modüllerinin nasıl çalıştığını bilmiyorsanız, aşağıdaki makalelerle başlayın:
- [Azure IoT Edge hakkında](../iot-edge/about-iot-edge.md)
- [Azure IoT Edge modülleri](../iot-edge/iot-edge-modules.md)

## <a name="connected-factory"></a>Bağlı fabrika

[Connected Factory,](../iot-accelerators/iot-accelerators-connected-factory-features.md) Microsoft'un Azure Endüstriyel IoT başvuru mimarisinin belirli iş gereksinimlerini karşılamak üzere özelleştirilebilen bir uygulamasıdır. Tam çözüm kodu açık kaynak koddur ve Bağlı Fabrika çözüm hızlandırıcıSı GitHub deposunda mevcuttur. Bunu ticari bir ürün için başlangıç noktası olarak kullanabilir ve azure aboneliğinize birkaç dakika içinde önceden oluşturulmuş bir çözüm dağıtabilirsiniz. 

## <a name="factory-floor-connectivity"></a>Fabrika zemin bağlantısı

OPC Twin, cihaz keşfini ve kaydıotomatikleştiren ve REST API'leri aracılığıyla endüstriyel cihazların uzaktan kontrolünü sunan bir IIoT bileşenidir. OPC Twin, bulutu ve fabrika ağını bağlamak için Azure IoT Edge ve IoT Hub'ı kullanır. OPC Twin, IIoT geliştiricilerin şirket içi makinelere güvenli bir şekilde nasıl erişeceklerini düşünmeden IIoT uygulamaları oluşturmaya odaklanmalarını sağlar.

## <a name="security"></a>Güvenlik

OPC Vault, OPC UA sunucusu ve buluttaki istemci uygulamaları için sertifika yaşam döngüsünü yapılandırabilen, kaydedilebilen ve yönetebilen OPC UA Global Discovery Server'ın (GDS) bir uygulamasıdır. OPC Vault, endüstriyel alanda güvenli varlık bağlantısının uygulanmasını ve bakımını kolaylaştırır. OPC Vault, sertifika yönetimini otomatikleştirerek fabrika operatörlerini bağlantı ve sertifika yönetimiyle ilişkili manuel ve karmaşık işlemlerden kurtarır.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi endüstriyel IoT ve bileşenleri için bir giriş oldu, burada önerilen bir sonraki adımdır:

> [!div class="nextstepaction"]
> [OPC Twin nedir?](overview-opc-twin.md)
