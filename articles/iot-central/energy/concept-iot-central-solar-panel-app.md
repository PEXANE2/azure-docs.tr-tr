---
title: Azure IoT Central mimari kavramları-enerji | Microsoft Docs
description: Bu makalede, Azure IoT Central mimarisiyle ilgili temel kavramlar tanıtılmaktadır
author: op-ravi
ms.author: omravi
ms.date: 10/23/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: afb6b0a5dc2204686872abb65666e6cdaab081a8
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72958059"
---
# <a name="azure-iot-central---solar-panel-app-architecture"></a>Azure IoT Central-Solar paneli uygulama mimarisi

Bu makalede, güneş paneli izleme uygulama şablonu mimarisine genel bir bakış sunulmaktadır. Aşağıdaki diyagramda, Azure 'da IoT Central platformu kullanarak bir Solar Panel uygulaması için yaygın olarak kullanılan bir mimari gösterilmektedir.

> [!div class="mx-imgBorder"]
> ![akıllı ölçüm mimarisi](media/concept-iot-central-solar-panel/solar-panel-app-architecture.png)

Bu mimari aşağıdaki bileşenlerden oluşur. Bazı uygulamalara burada listelenen bileşenlerin tümü gerekmeyebilir.

## <a name="solar-panels-and-connectivity"></a>Güneş bölmeleri ve bağlantısı 

Güneş bölmeleri, yenilenebilir enerji açısından önemli kaynaklardan biridir. Solar paneli türüne ve ayarına bağlı olarak, ağ geçitleri veya diğer ara cihazlar ve özel sistemler aracılığıyla bağlanabilir. Cihazları bağlamak için doğrudan bağlanamamış IoT Central cihaz Köprüsü oluşturmanız gerekebilir. IoT Central cihaz Köprüsü açık kaynaklı bir çözümdür ve tüm ayrıntıları [burada](https://docs.microsoft.com/azure/iot-central/howto-build-iotc-device-bridge)bulabilirsiniz. 



## <a name="iot-central-platform"></a>IoT Central platform
Azure IoT Central IoT çözümünüzü oluşturmayı kolaylaştıran ve IoT yönetimi, işlemler ve geliştirmenin yükünü ve maliyetlerini azaltmaya yardımcı olan bir platformdur. IoT Central, Nesnelerin İnterneti (IoT) varlıklarınızı ölçeklendirmek için kolayca bağlayabilirsiniz, izleyebilir ve yönetebilirsiniz. Solar panellerinizi IoT Central 'e bağladığınızda, uygulama şablonu cihaz modelleri, komutlar ve panolar gibi yerleşik özellikleri kullanır. Uygulama şablonu Ayrıca, neredeyse gerçek zamanlı ölçüm verileri izleme, analiz, kurallar ve görselleştirme gibi sıcak yol senaryoları için IoT Central depolama alanını kullanır.


## <a name="extensibility-options-to-build-with-iot-central"></a>IoT Central ile derlemek için genişletilebilirlik seçenekleri
IoT Central platformu iki genişletilebilirlik seçeneği sağlar: sürekli veri dışa aktarma (CDE) ve API 'Ler. Müşteriler ve iş ortakları, çözümlerini belirli gereksinimlere göre özelleştirmek için bu seçenekler arasında seçim yapabilir. Örneğin, iş ortaklarımızın biri Azure Data Lake Storage (ADLS) ile CDE olarak yapılandırıldı. Bunlar, uzun süreli veri saklama ve diğer soğuk yol depolama senaryoları (örneğin, toplu işleme, denetim ve raporlama amaçları) için ADLS kullanıyor. 

## <a name="next-steps"></a>Sonraki adımlar

* Artık mimari hakkında bilgi edindiğinize göre, [güneş panel uygulamasını ücretsiz oluşturun](https://apps.azureiotcentral.com/build/new/solar-panel-monitoring)
* IoT Central hakkında daha fazla bilgi edinmek için bkz. [IoT Central genel bakış](https://docs.microsoft.com/azure/iot-central/overview-iot-central)