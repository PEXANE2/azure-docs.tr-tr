---
title: Azure Percept Vision veri sayfası
description: Ayrıntılı cihaz belirtimleri için Azure Percept Vision veri sayfasına göz atın
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: reference
ms.date: 02/16/2021
ms.openlocfilehash: 8814192274a81938d53ffc68c02dfaa9ac1da8ad
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101663805"
---
# <a name="azure-percept-vision-datasheet"></a>Azure Percept Vision veri sayfası

Aşağıda listelenen belirtimler, Azure [PERCEPT dk](./azure-percept-dk-datasheet.md)'ye dahil edilen Azure Percept Vision cihazından yöneliktir.

|Ürün belirtimi           |Değer     |
|--------------------------------|---------------------|
|Hedef endüstriler               |Üretim <br> Akıllı binalar <br> Otomatik <br> Retail |
|Hero senaryoları                  |Alışverişçinin Analizi <br> Raf üzerine kullanılabilirlik <br> Küçültme azaltma <br> Güvenlik/araştırma <br> Çalışma alanı güvenliği|
|Boyutlar                      |42 mm x 42 mm x 40mm (Muhafazası ile Azure Percept Vision SoM derlemesi) <br> 42 mm x 42 mm x 6mm (Vision SoM yongası)|
|Yönetim Denetim düzlemi        |Azure cihaz Güncelleştirmesi (ADU)          |
|Desteklenen yazılımlar ve hizmetler |[Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) <br> [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/) <br> [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) <br> [ONNX Çalışma Zamanı](https://www.onnxruntime.ai/) <br> [OpenVINO](https://docs.openvinotoolkit.org/latest/index.html) <br> Azure cihaz güncelleştirmesi |
|AI hızlandırma                 |Intel Movidius Myriad X (MA2085) Vision Işleme birimi (VPU) Intel kamera ISS tümleşik, 0,7 La |
|Algılayıcılar ve görsel göstergeler   |OMNI Vision 5670 kamera <br> GSO GS8882AA lens (Çözüm: 5 MP, 30FPS 'de, uzaklık: 50cm-Infinity, FoV: 120 derece, renk: geniş dinamik Aralık, sabit odak kayan perde)          |
|Kamera desteği                  |RGB, IR ve derinlik kameraları <br> 2 kamera aynı anda çalıştırılabilir |
|Güvenlik Crypto-Controller      |ST-Micro STM32L462CE      |
|Sürüm oluşturma/KIMLIK bileşeni       |64 KB EEPROM |
|Bellek                          |LPDDR4 2GB     |
|Güç                           |3,5 W     |
|Bağlantı noktaları                           |1x USB 3,0 tür C <br> 2x MıPı 4 Lane (Lane başına 1,5 Gbps 'e kadar)     |
|Denetim arabirimleri              |2x ı2C <br> 2x SPI <br> 6x PWM (GPIOs: 2x saat, 2x çerçeve eşitleme, 2x kullanılmamış) <br> 2x yedek GıO |
|Sertifikasyon                   |CE <br> A      |
|İşletim sıcaklığı           |0-27 derece C (Azure Percept Vision SoM derlemesi, Muhafazası ile) <br> -10 ila 70 derece C (Vision SoM yongası) |
|Dokunma sıcaklığı               |<= 48 derece C |
|Bağıl nem               |%90-%    |