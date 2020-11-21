---
title: 'Nasıl yapılır: özel sesli konuşma hizmeti ile özel komutları kullanma'
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel komutlar uygulamasının çıktı sesini belirtirsiniz.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: sausin
ms.openlocfilehash: 4a5c14909606dcb862fcf53d99bc5bc00fba63bd
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95025693"
---
# <a name="use-custom-commands-with-custom-voice"></a>Özel Ses ile Özel Komutlar’ı Kullanma

Bu makalede, özel komutlar uygulaması için özel bir çıkış sesi seçme hakkında bilgi edineceksiniz.

## <a name="select-a-custom-voice"></a>Özel bir ses seçin

1. Özel komutlar uygulamanızda, sol bölmeden **Ayarlar** ' ı seçin.
1. Orta bölmeden **özel ses** ' i seçin.
1. Tablodan istenen özel veya genel sesi seçin.
1. **Kaydet**’i seçin.

> [!div class="mx-imgBorder"]
> ![Parametrelere sahip örnek cümleler](media/custom-commands/select-custom-voice.png)

> [!NOTE]
> - **Genel sesler** için **sinir türleri** yalnızca belirli bölgelerde kullanılabilir. Kullanılabilirliği denetlemek için [bölgeye/uç noktaya göre standart ve sinir seslikleri](./regions.md#standard-and-neural-voices)bölümüne bakın.
> - **Özel sesler** Için özel ses projesi sayfasından oluşturulabilirler. Bkz. [özel sesle çalışmaya başlama](./how-to-custom-voice.md).

Artık uygulama, varsayılan ses yerine seçili sesle yanıt verir.