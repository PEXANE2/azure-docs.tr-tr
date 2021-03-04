---
title: Azure Percept Audio ile çalışmaya başlama
description: Azure Percept ses cihazınızı Azure Percept DK 'nize bağlamayı öğrenin
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: quickstart
ms.date: 02/18/2021
ms.custom: template-quickstart
ms.openlocfilehash: 588ebde85b6012ddbfb88ca8305fc735b7a0ba41
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102098001"
---
# <a name="azure-percept-audio-setup"></a>Azure Percept ses kurulumu

Azure Percept Audio, Azure Percept DK ile kullanıma hazır. Benzersiz kurulum gerekmez.

## <a name="prerequisites"></a>Önkoşullar

- Azure Percept DK (devkit)
- Azure Percept sesi
- [Azure aboneliği](https://azure.microsoft.com/free/)
- [Azure PERCEPT dk kurulum deneyimi](./quickstart-percept-dk-set-up.md): devkit 'i bir Wi-Fi ağına bağladınız, bir IoT Hub oluşturdunuz ve devkit 'e IoT Hub
- 3,5 mm ses jakına bağlanabilecek konuşmacı veya kulaklıklar (isteğe bağlı)

## <a name="connecting-your-devices"></a>Cihazlarınızı bağlama

1. Azure Percept Audio cihazını, dahil edilen mikro USB ile USB türü-bir kabloyla Azure Percept DK taşıyıcı panosuna bağlayın. Kablonun mikro USB bitiş düzeyini ınterposer (Geliştirici) panosuna ve tür-A sonunu Percept DK taşıyıcı panosuna bağlayın.
1. (İsteğe bağlı) hoparlör veya kulaklığınızdan "satır dışı" etiketli ses jakı aracılığıyla Azure Percept seslerinizi bağlayın. Bu, sesli yardımcınızın ses yanıtlarını duymanıza imkan tanır. Konuşmacı veya kulaklıklar bağlantısını yapmazsanız, yanıtları demo penceresinde metin olarak görebilirsiniz. 

1. Devkit üzerinde güç. Interposer panosundaki LED L02, cihazın açık olduğunu ve ses SoM 'un kimlik doğrulaması olduğunu göstermek için yanıp sönen beyaz olarak değişir.

1. Kimlik doğrulama işleminin tamamlanmasını bekleyin--bu, 3 dakikaya kadar sürebilir.

1. Aşağıdakilerden birini gördüğünüzde prototipsiz başlamaya hazırsınız:

    - LED L02, düz beyaz olarak değişecektir. Bu, kimlik doğrulamasının tamamlandığını ve devkit 'in henüz bir anahtar sözcükle yapılandırılmadığını gösterir.
    - Üç LED 'in hepsi mavi. Bu, kimlik doğrulamanın tamamlandığını ve devkit 'in bir anahtar sözcükle yapılandırıldığını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

[Kod içermeyen bir konuşma çözümü](./tutorial-no-code-speech.md)oluşturun.
