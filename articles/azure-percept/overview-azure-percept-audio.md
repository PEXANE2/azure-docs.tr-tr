---
title: Azure Percept ses cihazına genel bakış
description: Azure Percept ses hakkında daha fazla bilgi edinin
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: 8884663b3f0e861e62f48c3aab680f0f31e74428
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102098392"
---
# <a name="introduction-to-azure-percept-audio"></a>Azure Percept ses 'e giriş

Azure Percept Audio, Azure Percept DK 'ye konuşma AI özellikleri ekleyen bir aksesuar aygıtıdır. Önceden yapılandırılmış bir ses işlemcisi ve dört mikrofonda oluşan bir doğrusal dizi içerir ve Azure bilişsel hizmetler 'i kullanarak yerel dinleme cihazlarına ses verme, anahtar sözcük biriktirme ve en çok alan konuşmayı uygulamanızı sağlar. Azure Percept Audio, cihaz üreticilerinin Azure Percept DK 'nin görme yeteneklerini ötesinde yeni, akıllı sesli özellikli cihazlara genişletmesini sağlar. Azure Percept DK, Azure Percept Studio ve diğer Azure Edge Yönetim Hizmetleri ile tümleştirilmiştir. [Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270)'da satın alınabilir.

:::image type="content" source="./media/overview-azure-percept-audio/percept-audio.png" alt-text="Azure Percept ses cihazı.":::

## <a name="azure-percept-audio-components"></a>Azure Percept ses bileşenleri

Azure Percept Audio aşağıdaki ana bileşenleri içerir:

- Dört mikrofonla bir XMOS codec bileşeni tarafından yapılan dört mikrofonla bir doğrusal dizi ve ses işleme için üretime yönelik Azure Percept ses cihazı (SoM)
- Geliştirici (ınterposer) panosu (2x düğme, 3x LED, mikro USB ve 3,5 mm ses jakı dahildir)
- Gerekli kablolar: FPC kablosu, USB mikro tür-B-USB-A
- Hoş geldiniz kartı
- Tümleşik 80/20 1010 serisi bağlama ile mekanik bağlama plakası

## <a name="compute-capabilities"></a>İşlem özellikleri 

Azure Percept Audio, Azure Percept DK 'nin bir karma uç bulutu halinde taşıyıcı panosunun CPU 'SU üzerinde çalışan konuşma yığını aracılığıyla ses girişini geçirir. Bu nedenle, Azure Percept Audio, gerçekleştirmek için konuşma yığınını destekleyen bir işletim sistemi içeren bir taşıyıcı panosu gerektirir. 

İşlem şu şekilde yapılır: 

- Azure Percept Audio: kirişleme ve yankı iptali gerçekleştirir ve konuşmayı iyileştirmek ve DK 'ye göndermek için gelen sesi işler.  

- Azure Percept DK: konuşma yığını, anahtar sözcük biriktirmesinin yerine getirir.  

- Bulut: doğal dil komutlarını ve tümcecikleri, anahtar sözcük doğrulamasını ve yeniden eğitimi işler. 

- Çevrimdışı: cihaz çevrimdışıysa, anahtar sözcüğünü algılayacak ve internet bağlantısı durum telemetrisini yakalayacak. Bulutta anahtar sözcük doğrulaması gerçekleştirilemediği için anahtar sözcük biriktirme için artan bir yanlış kabul hızı gözlemlenebilir. 

<!---

## How it works

Azure Percept Audio passes the audio input to the Azure Percept DK carrier board in a hybrid edge-cloud manner. Specifically,

- The Azure Percept Audio device: processes the incoming speech input to the clearest format by executing beam forming and echo cancellation befor sending the input to the Azure Percept DK. 
- The Azure Percept DK uses edge processing to perform keyword spotting and then sends the relevant inputs to Azure speech services.
- Cloud: Processing of natural language commands and phrases, in addition to keyword verification and retraining.
- Offline: If the device is offline it will detect the keyword and capture telemetry that there is no internet connection at the time of the command. It will not be able to weed out false accepts since it cannot perform keyword verification.

-->

## <a name="getting-started"></a>Başlarken

- [Azure Percept DK 'yi birleştirin](./quickstart-percept-dk-unboxing.md)
- [Azure Percept DK kurulum deneyimini doldurun](./quickstart-percept-dk-set-up.md)
- [Azure Percept ses cihazınızı devkit 'e bağlama](./quickstart-percept-audio-setup.md)

## <a name="build-a-no-code-prototype"></a>Kod içermeyen prototip oluşturma

Barındırma, sağlık, envanter ve oto senaryosu için Azure Percept Voice Yardımcısı şablonlarını kullanarak [kod içermeyen bir konuşma çözümü](./tutorial-no-code-speech.md) oluşturun.

### <a name="manage-your-no-code-speech-solution"></a>Kod içermeyen konuşma çözümünüzü yönetin

- [IoT Hub 'da sesli yardımcınızı yapılandırma](./how-to-manage-voice-assistant.md)
- [Azure Percept Studio 'da sesli yardımcınızı yapılandırma](./how-to-configure-voice-assistant.md)
- [Azure Percept ses sorunlarını giderme](./troubleshoot-audio-accessory-speech-module.md)

## <a name="additional-technical-information"></a>Ek teknik bilgiler

- [Azure Percept ses veri sayfası](./azure-percept-audio-datasheet.md)
- [Düğme ve LED davranışı](./audio-button-led-behavior.md)

## <a name="next-steps"></a>Sonraki adımlar

Bir Azure Percept ses cihazını [Microsoft Online Mağazası](https://go.microsoft.com/fwlink/p/?LinkId=2155270)'nda sıralama.
