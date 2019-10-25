---
title: Paper varlık öznitelikleri-Akademik Bilgi API
titlesuffix: Azure Cognitive Services
description: Akademik Bilgi API 'sindeki kağıt varlığıyla kullanabileceğiniz öznitelikleri öğrenin.
services: cognitive-services
author: DarrinEide
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: darrine
ROBOTS: NOINDEX
ms.openlocfilehash: c300a6477daa5759a68d5d11d40b1a71b46bd808
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72793830"
---
# <a name="paper-entity"></a>Kağıt varlığı

<sub>* Aşağıdaki öznitelikler kağıt varlığına özeldir. (Ty = ' 0 ')</sub>

Adı | Açıklama | Tür | Operations
--- | --- | --- | ---
AA. AID | Yazar ilişki KIMLIĞI | Int64 | Eşittir
AA. AfN | Yazar ilişki adı | Dize | Eşittir, StartsWith
AA. Auıd | Yazar KIMLIĞI | Int64 | Eşittir
AA. AuN | Normalleştirilmiş yazar adı | Dize | Eşittir, StartsWith
AA. Davun | Özgün yazar adı | Dize | Hiçbiri
AA. Bafn | Özgün ilişki adı | Dize | Hiçbiri
AA. Malar | Yazar listesinde sayısal konum | Int32 | Eşittir
BILGISI | Alıntı sayısı | Int32 | Hiçbiri  
C. CID | Konferans Serisi KIMLIĞI | Int64 | Eşittir
C.CN | Konferans Serisi adı | Dize | Eşittir, StartsWith
D | Yayımlanma tarihi YYYY-AA-GG biçiminde | Tarih | Eşittir, ısbetween
A | Genişletilmiş meta veriler (aşağıdaki tabloya bakın) | Dize | Yok  
ECC | Tahmini alıntı sayısı | Int32 | Hiçbiri
F. DFN | Özgün çalışma adı alanı | Dize | Hiçbiri
F. FID | Çalışma KIMLIĞI alanı | Int64 | Eşittir
F. FN | Çalışma adının normalleştirilmiş alanı | Dize | Eşittir, StartsWith
Kimlik | Kağıt KIMLIĞI | Int64 | Eşittir
J. JID | Günlük KIMLIĞI | Int64 | Eşittir
J. JN | Günlük adı | Dize | Eşittir, StartsWith
Yönergelerinin | Yayın türü (0: bilinmiyor, 1: günlük makalesi, 2: patent, 3: konferans kağıdı, 4: kitap bölümü, 5: kitap, 6: kitap başvuru girişi, 7: veri kümesi, 8: depo | Dize | Eşittir
RId | Başvurulan kağıt kimliklerinin listesi | Int64 [] | Eşittir
Varyantı | Normalleştirilmiş başlık | Dize | Eşittir, StartsWith
W | Başlıktaki benzersiz sözcükler | String [] | Eşittir
E | Yayımlanan yıl | Int32 | Eşittir, ısbetween

## <a name="extended-metadata-attributes"></a>Genişletilmiş meta veri öznitelikleri ##

Adı | Açıklama               
--- | ---
BT | BibTex belge türü (' a ': Journal makalesi, ' b ': Book, ' c ': kitap bölümü, ' p ': konferans kağıdı)
BV | BibTex mekan adı
BILGISI | Alıntı bağlamları – başvurulan kağıt KIMLIĞI ve yazılı bağlamın (örn. [{123: ["kahverengi parçalar, kağıt 123 ' de başvurulduğu için," yavaş köpekler "kağıt 123 ' de gösterilen bir geçmiş yanıltıcı,"]}) listesi
DEĞERI | Özgün kağıt başlığı
DOı | Dijital nesne tanımlayıcısı
FC | Yayındaki kağıdın ilk sayfası
I | Yayın sorunu
IA | Ters soyut
IA. Dizin uzunluğu | Dizindeki öğe sayısı (soyut sözcük sayısı)
IA. Ters Tedindex | Soyut sözcüklerin ve bunlara karşılık gelen konumlarından oluşan bir liste özgün soyut (örn. [{"": [0, 15, 30]}, {"kahverengi": [1]}, {"Fox": [2]}])
'Nın | Yayındaki kağıdın son sayfası
IÇERMIYOR | Yayımcı
S | Kaynaklar-kağıdın statik dereceye göre sıralanmış Web kaynakları listesi
S. Ty | Kaynak türü (1: HTML, 2: metin, 3: PDF, 4: DOC, 5: PPT, 6: XLS, 7: PS)
S. U | Kaynak URL 'SI
Yönetim | Yayın hacmi
VFN | Günlüğün veya konferans mekanın tam adı
VSN | Günlüğün veya konferans mekanın kısa adı
