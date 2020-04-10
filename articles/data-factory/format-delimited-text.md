---
title: Azure Veri Fabrikası'nda metin biçimini sınırlandırın
description: Bu konu, Azure Veri Fabrikası'nda sınırlı metin biçimiyle nasıl başa çıkılabildiğini açıklar.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: jingwang
ms.openlocfilehash: 47d26ad452b8494e591ee919076e5ade8bf19cd7
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81011404"
---
# <a name="delimited-text-format-in-azure-data-factory"></a>Azure Veri Fabrikası'nda metin biçimini sınırlandırın

**Sınırlı metin dosyalarını ayrışdırmak veya verileri sınırlı metin biçimine yazmak**istediğinizde bu makaleyi izleyin. 

Sınırlı metin biçimi aşağıdaki bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Veri Gölü Depolama Gen1](connector-azure-data-lake-store.md), [Azure Veri Gölü Depolama Gen2](connector-azure-data-lake-storage.md), [Azure Dosya Depolama](connector-azure-file-storage.md), Dosya [Sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Bulut Depolama](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md), ve [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, sınırlandırılmış metin veri kümesi tarafından desteklenen özelliklerin bir listesi sağlar.

| Özellik         | Açıklama                                                  | Gerekli |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Veri kümesinin tür özelliği **DelimitedText**olarak ayarlanmalıdır. | Evet      |
| location         | Dosya(lar) konum ayarları. Her dosya tabanlı bağlayıcının kendi konum türü ve `location`desteklenen özellikleri vardır.  | Evet      |
| columnDelimiter  | Karakter(ler) bir dosyadaki sütunları ayırmak için kullanılır. <br>Varsayılan değer ** `,`virgüldür. ** Sütun delimiter boş dize olarak tanımlandığında, hangi hiçbir sınırlayıcı anlamına gelir, tüm satır tek bir sütun olarak alınır.<br>Şu anda, sütun delimiter boş dize veya multi-char olarak yalnızca veri akışı eşleme için desteklenir, ancak kopyalama etkinliği değil.  | Hayır       |
| rowDelimiter     | Dosyadaki satırları ayırmak için kullanılan tek karakter veya "\r\n". <br>Varsayılan değer, okumadaki aşağıdaki değerlerden herhangi **biridir: ["\r\n", "\r", "\n"]** ve **"\n" veya "\r\n" sırasıyla** veri akışı ve Kopyalama etkinliğini eşleyerek yazmada. <br>Satır delimiter hiçbir delimiter (boş dize) olarak ayarlandığında, sütun delimiter de hiçbir delimiter (boş dize) olarak ayarlanmalıdır, hangi tek bir değer olarak tüm içeriği tedavi etmek anlamına gelir.<br>Şu anda, boş dize olarak satır delimiter yalnızca veri akışı eşleme için desteklenen ancak Kopyalama etkinliği değil. | Hayır       |
| quoteChar        | Sütun delimiter içeriyorsa sütun değerlerini alıntılamak için tek karakter. <br>Varsayılan değer **çift tırnak .** `"` <br>Veri akışını eşleme için boş `quoteChar` bir dize olamaz. <br>Kopyaetkinliği için, `quoteChar` boş dize olarak tanımlandığında, hiçbir teklif char ve sütun `escapeChar` değeri alıntı olmadığı anlamına gelir ve sütun sınırlayıcı ve kendisi kaçmak için kullanılır. | Hayır       |
| escapeChar       | Teklif edilen bir değerin içindeki alıntılardan kaçmak için tek karakter.<br>Varsayılan değer **ters `\`eğik çizgidir. ** <br>Veri akışını eşleme için boş `escapeChar` bir dize olamaz. <br/>Boş dize `escapeChar` olarak tanımlandığında Kopyalama `quoteChar` etkinliği için, boş dize olarak da ayarlanması gerekir, bu durumda tüm sütun değerlerinin sınırlayıcıları içermediğinden emin olun. | Hayır       |
| firstRowAsHeader | İlk satırı sütun adlarıyla bir üstbilgi satırı olarak ele alıp alıp açılacağını belirtir.<br>İzin verilen değerler **doğru** ve **yanlıştır** (varsayılan). | Hayır       |
| nullValue        | Null değerinin dize temsilini belirtir. <br>Varsayılan değer **boş dizedir.** | Hayır       |
| encodingName     | Test dosyalarını okumak/yazmak için kullanılan kodlama türü. <br>Allowed values are as follows: "UTF-8", "UTF-16", "UTF-16BE", "UTF-32", "UTF-32BE", "US-ASCII", "UTF-7", "BIG5", "EUC-JP", "EUC-KR", "GB2312", "GB18030", "JOHAB", "SHIFT-JIS", "CP875", "CP866", "IBM00858", "IBM037", "IBM273", "IBM437", "IBM500", "IBM737", "IBM775", "IBM850", "IBM852", "IBM855", "IBM857", "IBM860", "IBM861", "IBM863", "IBM864", "IBM865", "IBM869", "IBM870", "IBM01140", "IBM01141", "IBM01142", "IBM01143", "IBM01144", "IBM01145", "IBM01146", "IBM01147", "IBM01148", "IBM01149", "ISO-2022-JP", "ISO-2022-KR", "ISO-8859-1", "ISO-8859-2", "ISO-8859-3", "ISO-8859-4", "ISO-8859-5", "ISO-8859-6", "ISO-8859-7", "ISO-8859-8", "ISO-8859-9", "ISO-8859-13" " ISO-8859-15", "WINDOWS-874", "WINDOWS-1250", "WINDOWS-1251", "WINDOWS-1252", "WINDOWS-1253", "WINDOWS-1254", "WINDOWS-1255", "WINDOWS-1257", "WINDOWS-1258".<br>Not eşleme veri akışı UTF-7 kodlama desteklemez. | Hayır       |
| sıkıştırmaCodec | Metin dosyalarını okumak/yazmak için kullanılan sıkıştırma codec'i. <br>İzin verilen değerler **bzip2,** **gzip**, **deflate**, **ZipDeflate**, **snappy**, veya **lz4**. Varsayılan sıkıştırılmış değil. <br>**Not** şu anda Kopyalama etkinliği "snappy" & "lz4" desteklemez ve veri akışını eşleme "ZipDeflate" desteklemez. <br>ZipDeflate dosya(lar) decompress ve dosya tabanlı lavabo veri deposuna yazmak için kopyalama etkinliği kullanırken **not,** dosyalar klasöre ayıklanır: `<path specified in dataset>/<folder named as source zip file>/`. | Hayır       |
| Compressionlevel | Sıkıştırma oranı. <br>İzin verilen değerler **En Uygun** veya **En Hızlı**değerlerdir.<br>- **En hızlı:** Elde edilen dosya en iyi şekilde sıkıştırılmamış olsa bile sıkıştırma işlemi mümkün olan en kısa sürede tamamlanmalıdır.<br>- **Optimal**: Sıkıştırma işlemi, operasyonun tamamlanması daha uzun sürse bile en iyi şekilde sıkıştırılmalıdır. Daha fazla bilgi için [Sıkıştırma Düzeyi](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) konusuna bakın. | Hayır       |

Aşağıda Azure Blob Depolama'daki sınırlı metin veri kümesine bir örnek verilmiştir:

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "escapeChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, sınırlı metin kaynağı ve lavabo tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="delimited-text-as-source"></a>Sınırlı metin kaynak olarak 

Aşağıdaki özellikler kopyalama etkinliği *** \*kaynak\* *** bölümünde desteklenir.

| Özellik       | Açıklama                                                  | Gerekli |
| -------------- | ------------------------------------------------------------ | -------- |
| type           | Kopyalama etkinlik kaynağının türü özelliği **DelimitedTextSource**olarak ayarlanmalıdır. | Evet      |
| formatAyarlar | Bir grup özellik. Aşağıdaki **Sınırlı metin okuma ayarları** tablosuna bakın. | Hayır       |
| mağazaAyarları  | Veri deposundan verilerin nasıl okunacaklarına ilişkin özellikler grubu. Her dosya tabanlı bağlayıcının altında `storeSettings`kendi desteklenen okuma ayarları vardır. | Hayır       |

Desteklenen **sınırlı metin okuma** `formatSettings`ayarları altında:

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Biçimlendirme TürüAyarlar **DelimitedTextReadSettings**olarak ayarlanmalıdır. | Evet      |
| skipLineCount | Giriş dosyalarından verileri okurken atlayacak **boş olmayan** satır sayısını gösterir. <br>Hem skipLineCount hem de firstRowAsHeader parametresi belirtilirse önce satırlar atlanır, ardından giriş dosyasındaki üst bilgi bilgileri okunur. | Hayır       |

### <a name="delimited-text-as-sink"></a>Büzerek olarak sınırlı metin

Aşağıdaki özellikler kopyalama etkinliği *** \*lavabo\* *** bölümünde desteklenir.

| Özellik       | Açıklama                                                  | Gerekli |
| -------------- | ------------------------------------------------------------ | -------- |
| type           | Kopyalama etkinlik kaynağının tür özelliği **DelimitedTextSink**olarak ayarlanmalıdır. | Evet      |
| formatAyarlar | Bir grup özellik. Aşağıdaki **Sınırlı metin yazma ayarları** tablosuna bakın. |          |
| mağazaAyarları  | Veri deposuna nasıl veri yazılailgili bir grup özellik. Her dosya tabanlı bağlayıcının altında `storeSettings`kendi desteklenen yazma ayarları vardır.  | Hayır       |

Desteklenen **sınırlı metin yazma** `formatSettings`ayarları altında:

| Özellik      | Açıklama                                                  | Gerekli                                              |
| ------------- | ------------------------------------------------------------ | ----------------------------------------------------- |
| type          | Biçimayarlarının türü **DelimitedTextWriteSettings**olarak ayarlanmalıdır. | Evet                                                   |
| Dosyauzantısı | Çıktı dosyalarını adlandırmak için kullanılan dosya `.csv`uzantısı, örneğin . `.txt` Çıkış DelimitedText `fileName` veri kümesinde belirtilmediğinde belirtilmelidir. Çıktı veri kümesinde dosya adı yapılandırıldığında, lavabo dosya adı olarak kullanılır ve dosya uzantısı ayarı yoksayılır.  | Çıktı veri kümesinde dosya adı belirtilmediğinde evet |

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Veri akışını eşlemede [kaynak dönüşümü](data-flow-source.md) ve [lavabo dönüşümünden](data-flow-sink.md) ayrıntıları öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)
