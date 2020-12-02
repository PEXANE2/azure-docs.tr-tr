---
title: Azure Data Factory değişken etkinliğini sona Ekle
description: Bir Data Factory işlem hattında tanımlanmış var olan bir dizi değişkenine değer eklemek için değişken ekleme etkinliğinin nasıl ayarlanacağını öğrenin
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: dcstwh
ms.author: weetok
manager: jroth
ms.reviewer: maghan
ms.date: 10/09/2018
ms.openlocfilehash: 16bdd1d31440ed440faf67e939485da613e3886f
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96490950"
---
# <a name="append-variable-activity-in-azure-data-factory"></a>Azure Data Factory değişken etkinliğini sona Ekle
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]
Bir Data Factory işlem hattında tanımlanmış var olan bir dizi değişkenine değer eklemek için değişken ekleme etkinliğini kullanın.

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
name | İşlem hattının etkinlik adı | Evet
açıklama | Etkinliğin ne yaptığını açıklayan metin | hayır
tür | Etkinlik türü AppendVariable | yes
değer | Belirtilen değişkene eklemek için kullanılan dize sabiti veya ifade nesnesi değeri | yes
variableName | Etkinliğe göre değiştirilecek değişkenin adı, değişkenin ' Array ' türünde olması gerekir | evet

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen ilgili bir denetim akışı etkinliği hakkında bilgi edinin: 

- [Değişken Etkinlik Ayarlama](control-flow-set-variable-activity.md)
