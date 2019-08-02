---
title: Paper varlık öznitelikleri-Akademik Bilgi API
titlesuffix: Azure Cognitive Services
description: Akademik Bilgi API 'sindeki kağıt varlığıyla kullanabileceğiniz öznitelikleri öğrenin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 4b9431469a7925d26003ad9c34f6b401e5767f6d
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704936"
---
# <a name="paper-entity"></a>Kağıt varlığı

<sub>* Aşağıdaki öznitelikler kağıt varlığına özeldir. (Ty = ' 0 ')</sub>


Ad    |Açıklama                                        |Type       | İşlemler
------- | ------------------------------------------------- | --------- | ----------------------------
Id      |Varlık Kimliği                                          |Int64      |Eşittir
Varyantı      |Kağıt başlığı                                        |Dize     |Eşittir<br/>StartsWith
Ç       |"\@\@" İle ayrılmışKağıtdilikodu\@          |Dize     |Eşittir
E       |Kağıt yılı                                         |Int32      |Eşittir<br/>Isbetween
D       |Kağıt tarihi                                         |Date       |Eşittir<br/>Isbetween
BİLGİ      |Alıntı sayısı                                     |Int32      |yok  
ECC     |Tahmini alıntı sayısı                           |Int32      |yok
AA. AuN  |Yazar adı                                        |Dize     |Eşittir<br/>StartsWith
AA. Auıd |Yazar KIMLIĞI                                          |Int64      |Eşittir
AA. AfN  |Yazar ilişki adı                            |Dize     |Eşittir<br/>StartsWith
AA. AID |Yazar ilişki KIMLIĞI                              |Int64      |Eşittir
AA. MALAR    |Kağıt için yazar sırası                         |Int32      |Eşittir
F. FN    |Çalışma adı alanı                                |Dize     |Eşittir<br/>StartsWith
F. FID   |Çalışma KIMLIĞI alanı                                  |Int64      |Eşittir
J. JN    |Günlük adı                                       |Dize     |Eşittir<br/>StartsWith
J.JId   |Günlük KIMLIĞI                                         |Int64      |Eşittir
C.CN    |Konferans Serisi adı                             |Dize     |Eşittir<br/>StartsWith
C. CID   |Konferans Serisi KIMLIĞI                               |Int64      |Eşittir
RId     |Başvurulan kağıtların KIMLIĞI                              |Int64 []    |Eşittir
Ç       |Kağıt başlığından ve özetten sözcükler                |String[]   |Eşittir
E       |Genişletilmiş meta veriler (aşağıdaki tabloya bakın)                |Dize     |yok  
        


## <a name="extended-metadata-attributes"></a>Genişletilmiş meta veri öznitelikleri ##

Ad    | Açıklama               
--------|---------------------------    
DN      | Kağıdın görünen adı 
P       | Kaynaklar-kağıdın statik dereceye göre sıralanmış Web kaynakları listesi
S. Ty    | Kaynak türü (1: HTML, 2: metin, 3: PDF, 4: DOC, 5: PPT, 6: XLS, 7: PS)
S. U     | Kaynak URL
VFN     | Mekan tam adı-günlüğün veya konferansının tam adı
VSN     | Mekan kısa adı-günlüğün veya konferansın kısa adı
YÖNETİM       | Birim-günlük birimi
BV      | Günlük adı
BT      | 
PB      | Günlük kısaltmaları
I       | Sorun-günlük sorunu
FC      | FirstPage-kağıdın ilk sayfası
'NIN      | LastPage-kağıdın son sayfası
DOI     | Dijital nesne tanımlayıcısı
BİLGİ      | Alıntı bağlamları – başvurulan kağıt KIMLIĞI ve yazılı bağlamın (örn. [{123: ["kahverengi parçalar, kağıt 123 ' de başvurulduğu için," yavaş köpekler "kağıt 123 ' de gösterilen bir geçmiş yanıltıcı,"]}) listesi
IA      | Ters soyut
IA. Dizin uzunluğu| Dizindeki öğe sayısı (soyut sözcük sayısı)
IA. Ters Tedindex| Soyut sözcüklerin ve bunlara karşılık gelen konumlarından oluşan bir liste özgün soyut (örn. [{"": [0, 15, 30]}, {"kahverengi": [1]}, {"Fox": [2]}])
