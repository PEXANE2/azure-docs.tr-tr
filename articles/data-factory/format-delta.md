---
title: Azure Data Factory Delta biçimi
description: Delta biçimini kullanarak bir Delta Gölü verileri dönüştürme ve taşıma
author: djpmsft
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 07/09/2020
ms.author: daperlov
ms.openlocfilehash: 74c2e738153b1afa5c90f4769b6d9b0e982af363
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86225375"
---
# <a name="delta-format-in-azure-data-factory"></a>Azure Data Factory Delta biçimi

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Delta biçimini kullanarak [Azure Data Lake Store Gen2](connector-azure-data-lake-storage.md) veya [Azure Blob depolama](connector-azure-blob-storage.md) alanında depolanan bir Delta Gölü veya buradan veri kopyalama konuları gösterilmektedir. Bu bağlayıcı, veri akışlarını hem kaynak hem de havuz olarak eşlemek için bir [satır içi veri kümesi](data-flow-source.md#inline-datasets) olarak kullanılabilir.

> [!NOTE]
> Veri akışlarını eşlemek için Delta biçimi Bağlayıcısı Şu anda genel önizleme olarak kullanılabilir.

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Bu bağlayıcı, veri akışlarını hem kaynak hem de havuz olarak eşlemek için bir [satır içi veri kümesi](data-flow-source.md#inline-datasets) olarak kullanılabilir.

### <a name="source-properties"></a>Kaynak özellikleri

Aşağıdaki tabloda bir Delta kaynağı tarafından desteklenen özellikler listelenmiştir. Bu özellikleri **kaynak seçenekleri** sekmesinde düzenleyebilirsiniz.

| Ad | Açıklama | Gerekli | İzin verilen değerler | Veri akışı betiği özelliği |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Biçimlendir | Biçim olmalıdır`delta` | yes | `delta` | biçim |
| Dosya sistemi | Delta Gölü kapsayıcı/dosya sistemi | yes | Dize | Biçimlendiri |
| Klasör yolu | Delta Gölü doğrudan | yes | Dize | folderPath |
| Sıkıştırma türü | Delta tablosunun sıkıştırma türü | hayır | `bzip2`<br>`gzip`<br>`deflate`<br>`ZipDeflate`<br>`snappy`<br>`lz4` | compressionType |
| Sıkıştırma düzeyi | Sıkıştırmanın olabildiğince çabuk veya elde edilen dosyanın en iyi şekilde sıkıştırılıp tamamlanmayacağını seçin. | belirtilmişse gereklidir `compressedType` . | compressionLevel |
| Seyahat süresi | Bir Delta tablosunun daha eski bir anlık görüntüsünü sorgulamak isteyip istemediğinizi seçin | hayır | Zaman damgasına göre sorgula: zaman damgası <br> Sürüme göre sorgula: tamsayı | timestampAsOf <br> versionAsOf |

#### <a name="import-schema"></a>Şemayı içeri aktar

Delta yalnızca satır içi veri kümesi olarak kullanılabilir ve varsayılan olarak ilişkili bir şemaya sahip değildir. Sütun meta verilerini almak için **İzdüşüm** sekmesindeki **şemayı içeri aktar** düğmesine tıklayın. Bu, Corpus tarafından belirtilen sütun adlarına ve veri türlerine başvuruda bulunmak için izin verir. Şemayı içeri aktarmak için bir [veri akışı hata ayıklama oturumunun](concepts-data-flow-debug-mode.md) etkin olması ve işaret etmek için var olan bir CDM varlık tanımı dosyanızın olması gerekir.
 

### <a name="delta-source-script-example"></a>Delta kaynak betiği örneği

```
source(output(movieId as integer,
            title as string,
            releaseDate as date,
            rated as boolean,
            screenedOn as timestamp,
            ticketPrice as decimal(10,2)
            ),
    store: 'local',
    format: 'delta',
    versionAsOf: 0,
    allowSchemaDrift: false,
    folderPath: $tempPath + '/delta'
  ) ~> movies
```

### <a name="sink-properties"></a>Havuz özellikleri

Aşağıdaki tabloda bir Delta havuzu tarafından desteklenen özellikler listelenmiştir. Bu özellikleri **Ayarlar** sekmesinde düzenleyebilirsiniz.

| Ad | Açıklama | Gerekli | İzin verilen değerler | Veri akışı betiği özelliği |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Biçimlendir | Biçim olmalıdır`delta` | yes | `delta` | biçim |
| Dosya sistemi | Delta Gölü kapsayıcı/dosya sistemi | yes | Dize | Biçimlendiri |
| Klasör yolu | Delta Gölü doğrudan | yes | Dize | folderPath |
| Sıkıştırma türü | Delta tablosunun sıkıştırma türü | hayır | `bzip2`<br>`gzip`<br>`deflate`<br>`ZipDeflate`<br>`snappy`<br>`lz4` | compressionType |
| Sıkıştırma düzeyi | Sıkıştırmanın olabildiğince çabuk veya elde edilen dosyanın en iyi şekilde sıkıştırılıp tamamlanmayacağını seçin. | belirtilmişse gereklidir `compressedType` . | compressionLevel |
| Vakum | Eski tablo sürümleri için bekletme eşiğini saat cinsinden belirtin. 0 veya daha az varsayılan değer 30 gündür | yes | Tamsayı | Vakum |
| Yöntemi Güncelleştir | Delta Gölü üzerinde hangi güncelleştirme işlemlerine izin verileceğini belirtin. INSERT olmayan yöntemler için, satırları işaretlemek için önceki bir alter Row dönüşümü gerekir. | yes | `true` veya `false` | siler <br> eklenebilir <br> güncellenebilir <br> upsertable |

### <a name="delta-sink-script-example"></a>Delta havuz betiği örneği

İlişkili veri akışı betiği:

```
moviesAltered sink(
          input(movieId as integer,
                title as string
            ),
           mapColumn(
                movieId,
                title
            ),
           insertable: true,
           updateable: true,
           deletable: true,
           upsertable: false,
           keys: ['movieId'],
            store: 'local',
           format: 'delta',
           vacuum: 180,
           folderPath: $tempPath + '/delta'
           ) ~> movieDB
```

### <a name="known-limitations"></a>Bilinen sınırlamalar

Bir Delta havuzuna yazarken, yazılan satır sayılarının izleme çıktısına döndürülmeyeceği bilinen bir sınırlama vardır.

## <a name="next-steps"></a>Sonraki adımlar

* Eşleme veri akışında bir [kaynak dönüşümü](data-flow-source.md) oluşturun.
* Eşleme veri akışında bir [Havuz dönüştürmesi](data-flow-sink.md) oluşturun.
* Satırları INSERT, Update, upsert veya delete olarak işaretlemek için bir [alter Row dönüşümü](data-flow-alter-row.md) oluşturun.
