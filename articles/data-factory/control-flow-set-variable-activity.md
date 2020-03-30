---
title: Azure Veri Fabrikası'nda Değişken Etkinliği Ayarlama
description: Veri Fabrikası ardışık hattında tanımlanan varolan bir değişkenin değerini ayarlamak için Değişken Kümesi etkinliğini nasıl kullanacağınızı öğrenin
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74930654"
---
# <a name="set-variable-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Değişken Etkinliği Ayarlama

Veri Fabrikası ardışık dizisinde tanımlanan String, Bool veya Array türünden varolan bir değişkenin değerini ayarlamak için Değişken Ayarını Ayarla etkinliğini kullanın.

## <a name="type-properties"></a>Tür özellikleri

Özellik | Açıklama | Gerekli
-------- | ----------- | --------
ad | Boru hattındaki etkinliğin adı | Evet
açıklama | Etkinliğin ne yaptığını açıklayan metin | hayır
type | Etkinlik Türü SetVariable | evet
value | Belirtilen değişkeni ayarlamak için kullanılan string literal veya ifade nesnesi değeri | evet
değişkenAd | Bu etkinlik tarafından ayarlanacak değişkenin adı | evet


## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası tarafından desteklenen ilgili bir kontrol akışı etkinliği hakkında bilgi edinin: 

- [Değişken Etkinlik Ekleme](control-flow-append-variable-activity.md)
