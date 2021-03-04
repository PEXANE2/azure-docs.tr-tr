---
title: Azure Percept ses düğmesi ve LED davranışı
description: Azure Percept Audio düğme ve LED durumları hakkında daha fazla bilgi edinin
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: 1d956e33a84b5509c16400c8f5f8e813d116411a
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102095757"
---
# <a name="azure-percept-audio-button-and-led-behavior"></a>Azure Percept ses düğmesi ve LED davranışı

Azure Percept sesinin düğme ve LED durumları hakkında bilgi için aşağıdaki kılavuza bakın.

## <a name="button-behavior"></a>Düğme davranışı

Bu düğmeleri, cihazın davranışını denetlemek için kullanabilirsiniz.

|Düğme durumu|  Davranış|
|------------|----------|
|Mikrofon|  MIC dizisinin sesini kapatmak/açmak için tuşuna basın. Düğme olayı, basıldığında tetiklenir.|
|PTT/PTS|   Anahtar sözcük biriktirme durumunu atlayıp komut dinleme durumunu etkinleştirmek için PTT tuşuna basın. Aracının etkin iletişim kutusunu durdurmak ve anahtar sözcük biriktirme durumuna dönmek için bir kez daha tuşuna basın. Düğme olayı, basıldığında tetiklenir. PTS, aracı dinleme yaparken veya düşünirken değil, yalnızca aracı konuşurken düğme basıldığında işe yarar.|

## <a name="led-behavior"></a>LED davranışı

Cihazın hangi durumda olduğunu anlamak için LED göstergelerini kullanabilirsiniz.

|GELIŞTIRMESINE|   LED durumu|  Ear SoM durumu|
|---|------------|----------------| 
|L02|   1x beyaz, statik açık |Açma |
|L02|   1x beyaz, 0,5 Hz yanıp sönen|  Kimlik doğrulama devam ediyor |
|L01 & L02 & L03|   3x mavisi, statik|     Anahtar sözcük bekleniyor|
|L01 & L02 & L03|   LED dizisi yanıp sönme, 20 fps | Dinleme veya konuşuyor|
|L01 & L02 & L03|   LED dizisi yarış, 20 fps|    Düğün|
|L01 & L02 & L03|   3x kırmızı, statik | Mikrofon|

## <a name="next-steps"></a>Sonraki adımlar

Azure Percept ses cihazınız için sorun giderme ipuçları için bu [kılavuza](./troubleshoot-audio-accessory-speech-module.md)bakın.