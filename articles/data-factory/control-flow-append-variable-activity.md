---
title: Azure Data Factory değişken etkinliğini Ekle | Microsoft Docs
description: Bir Data Factory işlem hattında tanımlanmış var olan bir dizi değişkenine değer eklemek için değişken ekleme etkinliğinin nasıl ayarlanacağını öğrenin
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.date: 10/09/2018
ms.openlocfilehash: 24808c9ed4bda5e8d3b0ce23ab93bc59eb260374
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70141718"
---
# <a name="append-variable-activity-in-azure-data-factory"></a>Azure Data Factory değişken etkinliğini sona Ekle

Bir Data Factory işlem hattında tanımlanmış var olan bir dizi değişkenine değer eklemek için değişken ekleme etkinliğini kullanın.

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
name | İşlem hattının etkinlik adı | Evet
description | Etkinliğin ne yaptığını açıklayan metin | hayır
type | Etkinlik türü AppendVariable | evet
value | Belirtilen değişkene eklemek için kullanılan dize sabiti veya ifade nesnesi değeri | evet
variableName | Etkinliğe göre değiştirilecek değişkenin adı, değişkenin ' Array ' türünde olması gerekir | evet

## <a name="next-steps"></a>Sonraki adımlar
Data Factory tarafından desteklenen ilgili bir denetim akışı etkinliği hakkında bilgi edinin: 

- [Değişken etkinliğini ayarla](control-flow-set-variable-activity.md)
