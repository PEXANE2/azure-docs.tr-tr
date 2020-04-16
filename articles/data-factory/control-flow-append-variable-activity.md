---
title: Azure Veri Fabrikası'nda Ek Değişken Etkinlik
description: Veri Fabrikası ardışık dizisinde tanımlanan varolan bir dizi değişkenine değer katmak için Ek Değişken etkinliğini nasıl ayarleyeceğinizi öğrenin
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
ms.openlocfilehash: 5d74cd0fcd524f00d79eb3fbab386c602a413766
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414208"
---
# <a name="append-variable-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Ek Değişken Etkinlik
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]
Veri Fabrikası ardışık dizisinde tanımlanan varolan bir dizi değişkenine değer katmak için Ek Değişken etkinliğini kullanın.

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
ad | Boru hattındaki etkinliğin adı | Evet
açıklama | Etkinliğin ne yaptığını açıklayan metin | hayır
type | Etkinlik Türü EkDeğişkendir | evet
value | Belirtilen değişkene eklemek için kullanılan string literal veya ifade nesnesi değeri | evet
değişkenAd | Etkinlik tarafından değiştirilecek değişkenin adı, değişken 'Dizi' türünden olmalıdır | evet

## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası tarafından desteklenen ilgili bir kontrol akışı etkinliği hakkında bilgi edinin: 

- [Değişken Etkinlik Ayarlama](control-flow-set-variable-activity.md)
