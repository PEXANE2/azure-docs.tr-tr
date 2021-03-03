---
title: Azure Percept Audio ile çalışmaya başlama
description: Azure Percept ses cihazınızı Azure Percept DK 'nize bağlamayı öğrenin
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: quickstart
ms.date: 02/18/2021
ms.custom: template-quickstart
ms.openlocfilehash: 575107859f56df742ab41a299269c250511022b3
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101665628"
---
# <a name="azure-percept-audio-setup"></a>Azure Percept ses kurulumu

Azure Percept Audio, Azure Percept DK ile kullanıma hazır. Benzersiz kurulum gerekmez.

## <a name="prerequisites"></a>Önkoşullar

- Azure Percept DK (devkit)
- Azure Percept sesi
- [Azure aboneliği](https://azure.microsoft.com/free/)
- [Azure PERCEPT dk kurulum deneyimi](./quickstart-percept-dk-set-up.md): devkit 'i bir Wi-Fi ağına bağladınız, bir IoT Hub oluşturdunuz ve devkit 'e IoT Hub

## <a name="connecting-your-devices"></a>Cihazlarınızı bağlama

1. Azure Percept Audio cihazını, dahil edilen USB mikro tür-B ' i USB türünde bir kabloyla Azure Percept DK taşıyıcısı panosuna bağlayın. Kablonun mikro tür-B sonunu, Percept DK taşıyıcısı panosuna ve tür-A sonunu bir uca bağlayın.

1. Devkit üzerinde güç.

    - Ses SoM üzerinde LED L01, cihazın açık olduğunu göstermek için düz yeşil olarak değişir.
    - LED L02, ses SoM 'un kimlik doğrulaması olduğunu göstermek için yanıp sönen yeşil olarak değişir.

1. Kimlik doğrulama işleminin tamamlanmasını bekleyin--bu, 3 dakikaya kadar sürebilir.

1. Aşağıdakilerden birini gördüğünüzde prototipsiz başlamaya hazırsınız:

    - LED L01 kapanır ve L02 dönüşler beyaz. Bu, kimlik doğrulamasının tamamlandığını ve devkit 'in henüz bir anahtar sözcükle yapılandırılmadığını gösterir.
    - Üç LED 'in hepsi mavi. Bu, kimlik doğrulamanın tamamlandığını ve devkit 'in bir anahtar sözcükle yapılandırıldığını gösterir.

    > [!NOTE]
    > Devkit 'in kimlik doğrulaması yapamadığından desteğe ulaşın.

## <a name="next-steps"></a>Sonraki adımlar

[Kod içermeyen bir konuşma çözümü](./tutorial-no-code-speech.md)oluşturun.