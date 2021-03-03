---
title: Azure Percept ses cihazına genel bakış
description: Azure Percept ses hakkında daha fazla bilgi edinin
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: a63f471498667f58fc80f89323ad93b0feb8bc95
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101664093"
---
# <a name="introduction-to-azure-percept-audio"></a>Azure Percept ses 'e giriş

Azure Percept Audio, Azure Percept DK 'ye konuşma AI özellikleri ekleyen bir aksesuar aygıtıdır. Önceden yapılandırılmış bir konuşma AI hızlandırıcısı ve dört mikrofonda oluşan bir doğrusal dizi içerir ve yerel dinleme cihazlarına özel komutlar, anahtar sözcük biriktirme ve alan konuşmayı uygulamanıza olanak tanır. Azure Percept Audio, cihaz üreticilerinin Azure Percept DK Vision yeteneklerini yeni, akıllı sesli özellikli cihazlara genişletmelerine olanak sağlar. Azure Percept DK, Azure Percept Studio ve diğer Azure Edge Yönetim Hizmetleri ile tümleştirilmiştir. [Microsoft Online Store](https://go.microsoft.com/fwlink/p/?LinkId=2155270)'da satın alınabilir.

![Azure Percept ses cihazı.](./media/overview-azure-percept-audio/percept-audio.png)


## <a name="azure-percept-audio-components"></a>Azure Percept ses bileşenleri

Azure Percept Audio aşağıdaki ana bileşenleri içerir:

- Dört mikrofonla bir doğrusal dizi ile üretime Ready Azure Percept ses cihazı (SoM)
- Geliştirici Panosu (2x düğme, 3x LED, USB Micro ve 3,5 mm ses jakı dahildir)
- Gerekli kablolar: Flex kablosu, USB mikro tür-B-A
- Hoş geldiniz kartı
- Tümleşik 80/20 1010 serisi bağlama ile mekanik bağlama plakası


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

### <a name="build-a-no-code-prototype"></a>Kod içermeyen prototip oluşturma

Barındırma, sağlık, envanter ve oto senaryosu için Azure Percept Voice Yardımcısı şablonlarını kullanarak [kod içermeyen bir konuşma çözümü](./tutorial-no-code-speech.md) oluşturun.

## <a name="additional-technical-information"></a>Ek teknik bilgiler

- [Azure Percept ses veri sayfası](./azure-percept-audio-datasheet.md)

## <a name="next-steps"></a>Sonraki adımlar

Bir Azure Percept ses cihazını [Microsoft Online Mağazası](https://go.microsoft.com/fwlink/p/?LinkId=2155270)'nda sıralama.