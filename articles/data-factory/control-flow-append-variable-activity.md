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
ms.openlocfilehash: bcecd3411b6e6e751032f33862a671d8adbca046
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73679987"
---
# <a name="append-variable-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Ek Değişken Etkinlik

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
