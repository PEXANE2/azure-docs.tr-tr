---
title: Konferans örneği varlık öznitelikleri-Akademik Bilgi API
titlesuffix: Azure Cognitive Services
description: Akademik Bilgi API 'sindeki konferans örneği varlığıyla kullanabileceğiniz öznitelikleri öğrenin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 37a353fbb86ca199b2316dcfba5904f4b46b0276
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705051"
---
# <a name="conference-instance-entity"></a>Konferans örneği varlığı

<sub>* Aşağıdaki öznitelikler, konferans örneği varlığına özeldir. (Ty = ' 4 ')</sub>

Ad    |Açıklama                            |Type       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Id      |Varlık Kimliği                              |Int64      |Eşittir
CIN     |Konferans örneği normalleştirilmiş adı ({ConferenceSeriesNormalizedName} {ConferenceInstanceYear})        |Dize     |Eşittir
DCN     |Konferans örneği görünen adı ({ConferenceSeriesName}: {ConferenceInstanceYear})       |Dize     |yok
CIL     |Konferans örneğinin konumu    |Dize     |Eşittir<br/>StartsWith
CID    |Konferans örneğinin başlangıç tarihi  |Date       |Eşittir<br/>Isbetween
CIED    |Konferans örneğinin bitiş tarihi    |Date       |Eşittir<br/>Isbetween
CIARD   |Konferans örneğinin Özet kayıt bitiş tarihi  |Date       |Eşittir<br/>Isbetween
CIDD   |Konferans örneğinin gönderim bitiş tarihi     |Date       |Eşittir<br/>Isbetween
CIFVD   |Konferans örneğinin son sürüm bitiş tarihi  |Date       |Eşittir<br/>Isbetween
CINDD   |Konferans örneğinin bildirim tarihi   |Date       |Eşittir<br/>Isbetween
CD. ŞI    |Bir konferans örneği olayının başlığı   |Date       |Eşittir<br/>Isbetween
CD. TID    |Bir konferans örneği olayının tarihi    |Date       |Eşittir<br/>Isbetween
AÇILA. ,  |Örneğin konferans serisi adı |Dize     |Eşittir
Açıla. CID |Örneğin konferans serisi KIMLIĞI |Int64    |Eşittir
BİLGİ      |Konferans örneği toplam alıntı sayısı           |Int32      |yok  
ECC     |Konferans örneği toplam tahmini alıntı sayısı |Int32      |yok


## <a name="extended-metadata-attributes"></a>Genişletilmiş meta veri öznitelikleri ##

Ad    | Açıklama               
--------|---------------------------    
FN      | Konferans örneği tam adı
