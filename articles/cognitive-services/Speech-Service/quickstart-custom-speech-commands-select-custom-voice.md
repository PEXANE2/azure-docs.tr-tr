---
title: 'Hızlı başlangıç: özel ses (Önizleme) ile özel komutları kullanma-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel komutlar uygulamasının çıktı sesini belirtirsiniz.
services: cognitive-services
author: anhoang
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: anhoang
ms.openlocfilehash: c2b9b4d51e89975d988ed94bf85695bd8a1cc770
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872448"
---
# <a name="quickstart-use-custom-commands-with-custom-voice-preview"></a>Hızlı başlangıç: özel sesle özel komutlar kullanma (Önizleme)

[Önceki makalede](./quickstart-custom-speech-commands-create-parameters.md), parametrelere sahip komutlara yanıt vermek için yeni bir özel komutlar projesi oluşturduk.

Bu makalede, oluşturduğumuz uygulama için özel bir çıkış sesi seçeceğiz.

## <a name="select-a-custom-voice"></a>Özel bir ses seçin

1. [Daha önce oluşturduğumuz](./quickstart-custom-speech-commands-create-parameters.md) projeyi açın
1. Sol bölmedeki **ayarları** seçin
1. Orta bölmeden **özel ses** ' i seçin
1. Tablodan istenen özel veya genel sesi seçin
1. **Kaydet** 'i seçin

> [!div class="mx-imgBorder"]
> ![Parametrelere sahip örnek cümleler](media/custom-speech-commands/select-custom-voice.png)

> [!NOTE]
> - **Genel sesler**için **sinir türleri** yalnızca belirli bölgelerde kullanılabilir. Kullanılabilirliği denetlemek için [bölgeye/uç noktaya göre standart ve sinir seslikleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices)bölümüne bakın.
> - **Özel sesler**Için özel ses projesi sayfasından oluşturulabilirler. Bkz. [özel sesle çalışmaya başlama](./how-to-custom-voice.md).

Artık uygulama, varsayılan ses yerine seçili sesle yanıt verir.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Hızlı başlangıç: konuşma SDK 'Sı ile özel bir komut uygulamasına bağlanma (Önizleme)](./quickstart-custom-speech-commands-speech-sdk.md)

