---
title: Varlık özniteliklerini yazar-Akademik Bilgi API
titlesuffix: Azure Cognitive Services
description: Akademik Bilgi API 'sindeki Author varlığıyla kullanabileceğiniz öznitelikleri öğrenin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: e63e9d3f8f17a2473caf3d31b83e318ddb132b43
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705101"
---
# <a name="author-entity"></a>Yazar varlığı
<sub>* Aşağıdaki öznitelikler yazar varlığına özeldir. (Ty = ' 1 ')</sub>

Ad    |Açıklama                            |Type       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Id      |Varlık Kimliği                              |Int64      |Eşittir
AuN     |Yazar normalleştirilmiş adı                 |Dize     |Eşittir
Davun    |Yazar görünen adı                    |Dize     |yok
BİLGİ      |Yazar toplam alıntı sayısı            |Int32      |yok  
ECC     |Yazar toplam tahmini alıntı sayısı  |Int32      |yok
E       |Genişletilmiş meta veriler (bkz. "genişletilmiş meta öznitelikler" tablosu)  |Dize     |yok  


## <a name="extended-metadata-attributes"></a>Genişletilmiş meta veri öznitelikleri ##

Ad    | Açıklama               
--------|---------------------------    
LKA. AFN     | Yazarın yazar ile ilişkili görünen adı  
LKA. AID        | Yazarın yazar ile ilişkili varlık KIMLIĞI
