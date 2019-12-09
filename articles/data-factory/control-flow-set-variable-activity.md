---
title: Azure Data Factory değişken etkinliğini ayarla
description: Bir Data Factory işlem hattında tanımlanmış var olan bir değişkenin değerini ayarlamak için değişken ayarla etkinliğini nasıl kullanacağınızı öğrenin
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/10/2018
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.openlocfilehash: 88500ecbc56b34551a0cbd3ca94727ba4bbcda9f
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74930654"
---
# <a name="set-variable-activity-in-azure-data-factory"></a>Azure Data Factory değişken etkinliğini ayarla

Bir Data Factory işlem hattında tanımlanmış dize, bool veya dizi türünde bir değişkenin değerini ayarlamak için değişken ayarla etkinliğini kullanın.

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | Gereklidir
-------- | ----------- | --------
ad | İşlem hattının etkinlik adı | Yes
açıklama | Etkinliğin ne yaptığını açıklayan metin | hayır
type | Etkinlik türü SetVariable | evet
değer | Belirtilen değişkeni ayarlamak için kullanılan dize sabiti veya ifade nesnesi değeri | evet
variableName | Bu etkinlik tarafından ayarlanacak değişkenin adı | evet


## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen ilgili bir denetim akışı etkinliği hakkında bilgi edinin: 

- [Değişken ekleme etkinliği](control-flow-append-variable-activity.md)
