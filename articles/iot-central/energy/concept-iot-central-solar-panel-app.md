---
title: Azure IoT Central'da mimari kavramlar - Enerji | Microsoft Dokümanlar
description: Bu makale, Azure IoT Central mimarisiyle ilgili temel kavramları tanıtır
author: op-ravi
ms.author: omravi
ms.date: 10/23/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: 44171a08e69cfa058e0a9e75e3220fb996b7789d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77018015"
---
# <a name="azure-iot-central---solar-panel-app-architecture"></a>Azure IoT Central - solar panel uygulama mimarisi




Bu makalede, güneş paneli izleme uygulaması şablon mimarisine genel bir bakış sağlar. Aşağıdaki diyagram, IoT Central platform'u kullanarak Azure'da güneş paneli uygulaması için yaygın olarak kullanılan bir mimariyi göstermektedir.

> [!div class="mx-imgBorder"]
> ![akıllı sayaç mimarisi](media/concept-iot-central-solar-panel/solar-panel-app-architecture.png)

Bu mimari aşağıdaki bileşenlerden oluşur. Bazı uygulamalara burada listelenen bileşenlerin tümü gerekmeyebilir.

## <a name="solar-panels-and-connectivity"></a>Güneş panelleri ve bağlantı 

Güneş panelleri yenilenebilir enerji önemli kaynaklarından biridir. Güneş paneli türüne ve kurulumuna bağlı olarak, ağ geçitleri veya diğer ara cihazlar ve tescilli sistemleri kullanarak bağlayabilirsiniz. Doğrudan bağlanamayan aygıtları bağlamak için IoT Merkezi aygıt köprüsü oluşturmanız gerekebilir. IoT Merkezi cihaz köprü açık kaynak çözümdür ve [burada](https://docs.microsoft.com/azure/iot-central/core/howto-build-iotc-device-bridge)tam detayları bulabilirsiniz. 



## <a name="iot-central-platform"></a>IoT Merkezi platformu
Azure IoT Central, IoT çözümünüzü oluşturmayı kolaylaştıran ve IoT yönetimi, işlemleri ve geliştirme nin yükünü ve maliyetlerini azaltmaya yardımcı olan bir platformdur. IoT Central ile Nesnelerin İnterneti (IoT) varlıklarınızı ölçekte kolayca bağlayabilir, izleyebilir ve yönetebilirsiniz. Güneş panellerinizi IoT Central'a bağladıktan sonra, uygulama şablonu cihaz modelleri, komutlar ve panolar gibi yerleşik özellikleri kullanır. Uygulama şablonu ayrıca, yakın gerçek zamanlı sayaç veri izleme, analitik, kurallar ve görselleştirme gibi sıcak yol senaryoları için IoT Merkezi depolama kullanır.


## <a name="extensibility-options-to-build-with-iot-central"></a>IoT Central ile oluşturmak için genişletilebilirlik seçenekleri
IoT Merkezi platformu iki genişletilebilirlik seçeneği sunar: Sürekli Veri Dışa aktarım (CDE) ve API'ler. Müşteriler ve iş ortakları, çözümlerini belirli ihtiyaçlara göre özelleştirmek için bu seçenekler arasında seçim yapabilir. Örneğin, ortaklarımızdan biri CDE'yi Azure Veri Gölü Depolama (ADLS) ile yapılandırıldı. AdLS'yi uzun vadeli veri saklama ve toplu işlem, denetim ve raporlama gibi diğer soğuk yol depolama senaryoları için kullanıyorlar. 

## <a name="next-steps"></a>Sonraki adımlar

* Şimdi mimari hakkında öğrendim, [ücretsiz güneş paneli uygulaması oluşturmak](https://apps.azureiotcentral.com/build/new/solar-panel-monitoring)
* IoT Central hakkında daha fazla bilgi edinmek için [IoT Central genel bakış](https://docs.microsoft.com/azure/iot-central/)