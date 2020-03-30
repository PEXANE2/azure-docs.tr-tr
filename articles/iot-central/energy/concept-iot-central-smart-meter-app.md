---
title: Azure IoT Central'da mimari kavramlar - Enerji | Microsoft Dokümanlar
description: Bu makalede, Azure IoT Merkezi enerji uygulama şablonunun mimarisiyle ilgili temel kavramlar
author: op-ravi
ms.author: omravi
ms.date: 10/22/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: 8f3772c1d65780337c421cfaaa7b70d7ac7186cf
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77024322"
---
# <a name="azure-iot-central---smart-meter-app-architecture"></a>Azure IoT Central - akıllı sayaç uygulaması mimarisi



Bu makalede, akıllı sayaç izleme uygulaması şablon mimarisine genel bir bakış sağlar. Aşağıdaki diyagram, IoT Central platform'u kullanarak Azure'daki akıllı sayaç uygulaması için yaygın olarak kullanılan bir mimariyi gösterir.

> [!div class="mx-imgBorder"]
> ![akıllı sayaç mimarisi](media/concept-iot-central-smart-meter/smart-meter-app-architecture.png)

Bu mimari aşağıdaki bileşenlerden oluşur. Bazı çözümler burada listelenen her bileşeni gerektirmeyebilir.

## <a name="smart-meters-and-connectivity"></a>Akıllı sayaçlar ve bağlantı 

Akıllı sayaç tüm enerji varlıkları arasında en önemli cihazlardan biridir. Enerji tüketimi verilerini izleme ve faturalandırma ve talep yanıtı gibi diğer kullanım durumları için yardımcı programlara kaydeder ve ileter. Sayaç türüne bağlı olarak, ioT Central'a ağ geçitleri veya diğer ara aygıtlar veya sistemler, bu tür kenar aygıtları ve kafa ucu sistemleri kullanarak bağlanabilir. Doğrudan bağlanabilen aygıtları bağlamak için IoT Merkezi aygıt köprüsü oluşturun. IoT Merkezi cihaz köprü açık kaynak çözümdür ve [burada](https://docs.microsoft.com/azure/iot-central/core/howto-build-iotc-device-bridge)tam detayları bulabilirsiniz. 


## <a name="iot-central-platform"></a>IoT Merkezi platformu

Azure IoT Central, IoT çözümünüzü oluşturmayı kolaylaştıran ve IoT yönetimi, işlemleri ve geliştirme nin yükünü ve maliyetlerini azaltmaya yardımcı olan bir platformdur. IoT Central ile Nesnelerin İnterneti (IoT) varlıklarınızı ölçekte kolayca bağlayabilir, izleyebilir ve yönetebilirsiniz. Akıllı sayaçlarınızı IoT Central'a bağladıktan sonra uygulama şablonu, aygıt modelleri, komutlar ve panolar gibi yerleşik özellikleri kullanır. Uygulama şablonu ayrıca, yakın gerçek zamanlı sayaç veri izleme, analitik, kurallar ve görselleştirme gibi sıcak yol senaryoları için IoT Merkezi depolama kullanır. 


## <a name="extensibility-options-to-build-with-iot-central"></a>IoT Central ile oluşturmak için genişletilebilirlik seçenekleri
IoT Merkezi platformu iki genişletilebilirlik seçeneği sunar: Sürekli Veri Dışa aktarım (CDE) ve API'ler. Müşteriler ve iş ortakları, çözümlerini belirli ihtiyaçlara göre özelleştirmek için bu seçenekler arasında seçim yapabilir. Örneğin, ortaklarımızdan biri CDE'yi Azure Veri Gölü Depolama (ADLS) ile yapılandırıldı. AdLS'yi uzun vadeli veri saklama ve toplu işlem, denetim ve raporlama gibi diğer soğuk yol depolama senaryoları için kullanıyorlar. 

## <a name="next-steps"></a>Sonraki adımlar

* Artık mimariyi öğrendiğiniz [için, akıllı sayaç uygulamasını ücretsiz olarak oluşturun](https://apps.azureiotcentral.com/build/new/smart-meter-monitoring)
* IoT Central hakkında daha fazla bilgi edinmek için [IoT Central genel bakış](https://docs.microsoft.com/azure/iot-central/)
