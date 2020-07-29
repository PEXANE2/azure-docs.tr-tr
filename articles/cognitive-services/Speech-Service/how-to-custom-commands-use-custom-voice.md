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
ms.openlocfilehash: b1e488653f210089ee5ed6757fb2f3d1e4fb0fe4
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87294550"
---
# <a name="use-custom-commands-with-custom-voice"></a>Özel Ses ile Özel Komutlar’ı Kullanma

Bu makalede, özel komutlar uygulaması için özel bir çıkış sesi seçme hakkında bilgi edineceksiniz.

## <a name="select-a-custom-voice"></a>Özel bir ses seçin

1. Özel komutlar uygulamanızda, sol bölmeden **Ayarlar** ' ı seçin.
1. Orta bölmeden **özel ses** ' i seçin.
1. Tablodan istenen özel veya genel sesi seçin.
1. **Kaydet**'i seçin.

> [!div class="mx-imgBorder"]
> ![Parametrelere sahip örnek cümleler](media/custom-commands/select-custom-voice.png)

> [!NOTE]
> - **Genel sesler**için **sinir türleri** yalnızca belirli bölgelerde kullanılabilir. Kullanılabilirliği denetlemek için [bölgeye/uç noktaya göre standart ve sinir seslikleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices)bölümüne bakın.
> - **Özel sesler**Için özel ses projesi sayfasından oluşturulabilirler. Bkz. [özel sesle çalışmaya başlama](./how-to-custom-voice.md).

Artık uygulama, varsayılan ses yerine seçili sesle yanıt verir.
