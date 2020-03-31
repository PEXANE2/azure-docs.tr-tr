---
title: Azure Veri Gezgini tarafından desteklenen veri biçimleri.
description: Azure Veri Gezgini tarafından desteklenen çeşitli veri ve sıkıştırma biçimleri hakkında bilgi edinin.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/19/2020
ms.openlocfilehash: 16ecdf7ac3f0062637e4bbea86d26e2560f38222
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235295"
---
# <a name="data-formats-supported-by-azure-data-explorer-for-ingestion"></a>Azure Veri Gezgini tarafından desteklenen veri biçimleri

Veri alma işlemi, verilerin bir tabloya eklendiği ve Azure Veri Gezgini'nde sorgu için kullanılabilir hale getirildiği işlemdir. Sorgudan alma dışında tüm yutma yöntemleri için, veriler desteklenen biçimlerden birinde olmalıdır. Aşağıdaki tablo, Azure Veri Gezgini'nin veri alımı için desteklediği biçimleri listeler ve açıklar.

|Biçimlendir   |Dahili numara   |Açıklama|
|---------|------------|-----------|
|Avro     |`.avro`     |Bir [Avro kapsayıcı dosyası](https://avro.apache.org/docs/current/). Aşağıdaki kodlar `null`desteklenir: `deflate` `snappy` , ( şu anda desteklenmiyor).|
|CSV      |`.csv`      |Virgülle ayrılmış değerlere sahip`,`bir metin dosyası ( ). Bkz. [RFC 4180: _Virgülle Ayrılmış Değerler (CSV) Dosyaları için Ortak Biçim ve MIME Türü._](https://www.ietf.org/rfc/rfc4180.txt)|
|JSON     |`.json`     |JSON nesnelerinin sınırlandırılabEdilmiş `\n` `\r\n`olduğu bir metin dosyası veya . [Bkz. JSON Hatları (JSONL)](http://jsonlines.org/).|
|MultiJSON|`.multijson`|Bir JSON özellik torbaları dizisi (her biri bir kaydı temsil eden) veya beyaz `\n` alanla sınırlandırılmış herhangi bir sayıda özellik torbalı bir metin dosyası veya. `\r\n` Her özellik çantası birden fazla satıra yayılabilir. Bu biçim, `JSON`veriler özellik olmayan çantalar olmadığı sürece tercih edilir.|
|Orc      |`.orc`      |Bir [Ork dosyası.](https://en.wikipedia.org/wiki/Apache_ORC)|
|Parke  |`.parquet`  |Parke [dosyası.](https://en.wikipedia.org/wiki/Apache_Parquet)|
|Psv      |`.psv`      |Boruyla ayrılmış değerlere sahip<code>&#124;</code>bir metin dosyası ( ).|
|Ham      |`.raw`      |Tüm içeriği tek bir dize değeri olan bir metin dosyası.|
|SCsv     |`.scsv`     |Yarı sütunlu ayrılmış değerlere`;`sahip bir metin dosyası ( ).|
|SOHsv    |`.sohsv`    |SOH ayrılmış değerleri ile bir metin dosyası. (SOH ASCII codepoint 1; bu biçim HIve tarafından HDInsight'ta kullanılır.)|
|Tsv      |`.tsv`      |Sekme ayrılmış değerlere sahip`\t`bir metin dosyası ( ).|
|TSVE     |`.tsv`      |Sekme ayrılmış değerlere sahip`\t`bir metin dosyası ( ). Bir ters çizgi`\`karakteri ( ) kaçmak için kullanılır.|
|TXT      |`.txt`      |Satırları n ' ile `\n`sınırlandırılandırılan bir metin dosyası Boş satırlar atlanır.|

## <a name="supported-data-compression-formats"></a>Desteklenen veri sıkıştırma biçimleri

Blobs ve dosyaları aşağıdaki sıkıştırma algoritmaları herhangi biri ile sıkıştırılabilir:

|Sıkıştırma|Dahili numara|
|-----------|---------|
|Gzip       |Gz      |
|Zip        |.zip     |

Blob veya dosyanın adına uzantısı ekleyerek sıkıştırma gösterin.

Örnek:
* `MyData.csv.zip`bir blob veya CSV olarak biçimlendirilmiş bir dosyayı, ZIP (arşiv veya tek bir dosya) ile sıkıştırılmış gösterir
* `MyData.csv.gz`GZip ile sıkıştırılmış bir blob veya CSV olarak biçimlendirilmiş bir dosya gösterir

Biçim uzantılarını içermeyen ancak sıkıştırma (örneğin,) içermeyen blob veya dosya adları da desteklenir. Bu durumda, dosya biçimi çıkarılamaz çünkü bir yutma özelliği olarak belirtilmelidir.

> [!NOTE]
> Bazı sıkıştırma biçimleri sıkıştırılmış akışın bir parçası olarak özgün dosya uzantısını izler. Bu uzantı genellikle dosya biçimini belirlemek için yoksayılır. Dosya biçimi (sıkıştırılmış) blob veya dosya adından belirlenemezse, `format` yutma özelliği ile belirtilmelidir.

## <a name="next-steps"></a>Sonraki adımlar

* [Veri alma](/azure/data-explorer/ingest-data-overview) hakkında daha fazla bilgi edinin
* [Azure Veri Gezgini veri alma özellikleri](ingestion-properties.md) hakkında daha fazla bilgi edinin
