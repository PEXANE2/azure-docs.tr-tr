---
title: Konferans Serisi varlık öznitelikleri-Akademik Bilgi API
titlesuffix: Azure Cognitive Services
description: Konferans Serisi varlığıyla kullanabileceğiniz öznitelikler hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: feed324202f6a75ceb7e9089875b899c51cd8ae6
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705039"
---
# <a name="conference-series-entity"></a>Konferans Serisi varlığı

<sub>* Aşağıdaki öznitelikler, konferans serisi varlığına özgüdür. (Ty = ' 3 ')</sub>

Ad    |Açıklama                            |Type       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Id      |Varlık Kimliği                              |Int64      |Eşittir
CN      |Konferans Serisi normalleştirilmiş adı      |Dize     |Eşittir
DCN     |Konferans Serisi görünen adı         |Dize     |yok
BİLGİ      |Konferans Serisi Toplam alıntı sayısı         |Int32      |yok  
ECC     |Konferans Serisi Toplam tahmini alıntı sayısı   |Int32      |yok
F. FID   |Konferans serisiyle ilişkili çalışma varlık KIMLIĞI alanı |Int64  | Eşittir
F. FN    |Konferans serisiyle ilişkili çalışma adı alanı  | Eşittir<br/>StartsWith
