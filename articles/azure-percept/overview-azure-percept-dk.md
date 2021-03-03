---
title: Azure Percept DK genel bakış
description: Azure Percept DK hakkında daha fazla bilgi edinin
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: 4fd0a7cb575a109d1393527b48de3fa4e3446167
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101664021"
---
# <a name="azure-percept-dk-overview"></a>Azure Percept DK genel bakış

Azure Percept DK, Vision AI kanıtı kavramlarının geliştirilmesi için tasarlanan bir Edge AI geliştirme setidir. [Azure Percept Studio](./overview-azure-percept-studio.md)ile birleştirildiğinde, çok ÇEŞITLI Vision AI uygulamaları IÇIN Edge AI çözümleri oluşturmaya yönelik güçlü, kullanımı kolay bir platform haline gelir. [Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270)'da satın alınabilir.

:::image type="content" source="./media/overview-azure-percept-dk/dk-image.png" alt-text="Görüntüyle.":::

## <a name="key-features"></a>Temel Özellikler

- **Kenarda AI çalıştırma özelliği**. Yerleşik donanım hızlandırma sayesinde, bulut bağlantısı olmadan Vision AI modellerini çalıştırabilir.
- **Yerleşik güven güvenliği donanım kökü**. Daha fazla bilgi için bkz. [Azure Percept Security](./overview-percept-security.md) 'ye genel bakış.
- **[Azure Percept Studio](./overview-azure-percept-studio.md)** ve diğer Azure hizmetleriyle sorunsuz tümleştirme. Azure IoT Hub, Azure bilişsel hizmetler ve [canlı video analizi](https://docs.microsoft.com/azure/media-services/live-video-analytics-edge/overview) gibi
- **En üst AI platformları Için destek**. ONNX ve TensorFlow gibi.
- **80/20 kenle sistemiyle tümleştirme**. Üretim ortamlarında Prototiplerde derleme yapmayı kolaylaştırın. [8/20 tümleştirmesi](./overview-8020-integration.md)hakkında daha fazla bilgi edinin.

## <a name="hardware-components"></a>Donanım bileşenleri

- Azure Percept DK taşıyıcı panosu
    - NXP iMX8m işlemcisi
    - Güvenilir Platform Modülü (TPM) sürüm 2,0
    - WiFi ve Bluetooth bağlantısı
    - Tam [veri sayfasına](./azure-percept-dk-datasheet.md) bakın
- Modül üzerinde Azure Percept Vision sistemi (SoM)
    - Intel Movidius Myriad X (MA2085) Vision işleme birimi (VPU)
    - İkinci bir ekleyebilme özelliğine sahip RGB kamera algılayıcısı
    - Tam [veri sayfasına](./azure-percept-vision-datasheet.md) bakın

## <a name="get-started-with-the-azure-percept-dk"></a>Azure Percept DK ile çalışmaya başlama

- Bu hızlı başlar
    - [Azure Percept DK 'yi kutudan kaldırma ve birleştirme](./quickstart-percept-dk-unboxing.md)
    - [Azure Percept DK 'yi ayarlama ve ilk Vision AI modelinizi çalıştırma](./quickstart-percept-dk-set-up.md)
- Bu öğreticilerle kavram kanıtı oluşturmaya başlayın
    - [Azure Percept Studio 'da kod içermeyen bir Vision çözümü oluşturma](./tutorial-nocode-vision.md)
    - [Azure Percept Studio 'da bir ses Yardımcısı oluşturma](./tutorial-no-code-speech.md)

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft Online Mağazası](https://go.microsoft.com/fwlink/p/?LinkId=2155270)'Nda bir Azure Percept dk siparişi.