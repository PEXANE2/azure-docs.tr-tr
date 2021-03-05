---
title: Azure Percept DK genel bakış
description: Azure Percept DK hakkında daha fazla bilgi edinin
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: c93ecdc138e757f84e995a69a6c3d7f1b5460abe
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102179383"
---
# <a name="azure-percept-dk-overview"></a>Azure Percept DK genel bakış

Azure Percept DK, kavram ve ses AI kanıtı kavramlarının geliştirilmesi için tasarlanan bir Edge AI ve IoT geliştirme setidir. [Azure Percept Studio](./overview-azure-percept-studio.md) ve [Azure Percept sesiyle](./overview-azure-percept-audio.md)birleştirildiğinde, çok ÇEŞITLI Vision veya Audio AI uygulamaları için Edge AI çözümleri oluşturmaya yönelik güçlü, kullanımı kolay bir platform haline gelir. [Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270)'da satın alınabilir.

:::image type="content" source="./media/overview-azure-percept-dk/dk-image.png" alt-text="Azure Percept DK cihazı.":::

## <a name="key-features"></a>Temel Özellikler

- **Kenarda AI çalıştırma özelliği**. Yerleşik donanım hızlandırma sayesinde, bulut bağlantısı olmadan Vision AI modellerini çalıştırabilir.
- **Yerleşik güven güvenliği donanım kökü**. Daha fazla bilgi için bkz. [Azure Percept Security](./overview-percept-security.md) 'ye genel bakış.
- **[Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819)** ve diğer Azure hizmetleriyle sorunsuz tümleştirme. Azure IoT Hub, Azure bilişsel hizmetler ve [canlı video analizi](https://docs.microsoft.com/azure/media-services/live-video-analytics-edge/overview) gibi
- **İsteğe bağlı [Azure Percept Audio](./overview-azure-percept-audio.md) ile sorunsuz tümleştirme**
- **En üst AI platformları Için destek**. ONNX ve TensorFlow gibi.
- **80/20 kenle sistemiyle tümleştirme**. Üretim ortamlarında Prototiplerde derleme yapmayı kolaylaştırın. [80/20 tümleştirmesi](./overview-8020-integration.md)hakkında daha fazla bilgi edinin.

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
