---
title: Çalışma varlığı öznitelikleri alanı-Akademik Bilgi API
titlesuffix: Azure Cognitive Services
description: Akademik Bilgi API 'sindeki çalışma varlığı alanı ile kullanabileceğiniz öznitelikleri öğrenin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: d54f3c72462e6702b09068092b7c18ea50f12048
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704992"
---
# <a name="field-of-study-entity"></a>Çalışma varlığının alanı

<sub>* Aşağıdaki öznitelikler, çalışma varlığının alanına özgüdür. (Ty = ' 6 ')</sub>

Ad    |Açıklama                            |Type       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Id      |Varlık Kimliği                              |Int64      |Eşittir
FN      |Çalışma alanı normalleştirilmiş adı alanı         |Dize     |Eşittir
DFN     |Çalışma görünen adının alanı            |Dize     |yok
BİLGİ      |Çalışma alanı toplam alıntı sayısı    |Int32      |yok  
ECC     |Toplam tahmini alıntı sayısı alanı|Int32      |yok
FLORIDA      |Çalışma hiyerarşisi alanlarındaki düzey     |Int32      |Eşittir <br/>Isbetween
FC. FN   |Çalışma adının üst alanı             |Dize     |Eşittir
FC. Var  |Çalışma KIMLIĞININ üst alanı               |Int64      |Eşittir
FC. FN   |Çalışma adının alt alanı              |Dize     |Eşittir
FC. Var  |Çalışma KIMLIĞININ alt alanı                |Int64      |Eşittir
