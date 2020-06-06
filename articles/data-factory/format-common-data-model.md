---
title: Common Data Model biçimi
description: Ortak veri modeli meta veri sistemini kullanarak verileri dönüştürme
author: djpmsft
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/05/2020
ms.author: daperlov
ms.openlocfilehash: 0dce717461754ac1259bc666adf4eb9f7ef9d6c2
ms.sourcegitcommit: 813f7126ed140a0dff7658553a80b266249d302f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84465278"
---
# <a name="common-data-model-format-in-azure-data-factory"></a>Azure Data Factory ortak veri modeli biçimi
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Common Data Model (CDM) meta veri sistemi, verilerin ve anlamının uygulamalar ve iş süreçlerinde kolayca paylaşılmasını olanaklı kılar. Daha fazla bilgi için bkz. [ortak veri modeline](https://docs.microsoft.com/common-data-model/) genel bakış.

Azure Data Factory, kullanıcılar, veri akışlarını eşleme kullanarak [Azure Data Lake Store Gen2](connector-azure-data-lake-storage.md) (ADLS 2.) içinde depolanan CDM varlıklarına ve bu varlıkları dönüştürebilir.

> [!NOTE]
> ADF veri akışları için ortak veri modeli (CDM) biçim Bağlayıcısı Şu anda genel önizleme olarak kullanılabilir.

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Ortak veri modeli, kaynak ve havuz olarak eşleme verilerinde bir [satır içi](data-flow-source.md#inline-datasets) veri akışı olarak kullanılabilir.

### <a name="source-properties"></a>Kaynak özellikleri

Aşağıdaki tabloda bir CDM kaynağı tarafından desteklenen özellikler listelenmiştir.

| Adı | Açıklama | Gerekli | İzin verilen değerler | Veri akışı betiği özelliği |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Biçimlendir | Biçim olmalıdır`cdm` | evet | `cdm` | biçim |
| Meta veri biçimi | Verilerin Varlık başvurusunun bulunduğu yer. CDM sürüm 1,0 kullanıyorsanız, bildirim ' ı seçin. 1,0 öncesi bir CDM sürümü kullanıyorsanız, model. json ' ı seçin. | Yes | `'manifest'` veya `'model'` | manifestType |
| Kök konumu: kapsayıcı | CDM klasörünün kapsayıcı adı | evet | Dize | Biçimlendiri |
| Kök konumu: klasör yolu | CDM klasörünün kök klasör konumu | evet | Dize | folderPath |
| Bildirim dosyası: varlık yolu | Kök klasör içindeki varlığın klasör yolu | hayır | Dize | entityPath |
| Bildirim dosyası: bildirim adı | Bildirim dosyasının adı. Varsayılan değer ' default ' değeridir  | Hayır | Dize | manifestName |
| Son değiştirme ölçütü | En son değiştirildiklerinde dosyaları filtrelemek için seçin | hayır | Zaman damgası | Modıfıedafter <br> modifiedBefore | 
| Şemaya bağlı hizmet | Yapı 'nin bulunduğu bağlı hizmet | Evet, bildirim kullanılıyorsa | `'adlsgen2'` veya `'github'` | corpusStore | 
| Varlık başvurusu kapsayıcısı | Kapsayıcı yapı | Evet, bildirim ve yapı ADLS 2. kullanılıyorsa | Dize | adlsgen2_fileSystem |
| Varlık başvuru deposu | GitHub deposu adı | Evet, GitHub 'da bildirim ve yapı kullanılıyorsa | Dize | github_repository |
| Varlık başvuru dalı | GitHub depo dalı | Evet, GitHub 'da bildirim ve yapı kullanılıyorsa | Dize |  github_branch |
| Corpus klasörü | Yapı 'nin kök konumu | Evet, bildirim kullanılıyorsa | Dize | corpusPath |
| Corpus varlığı | Varlık başvurusunun yolu | evet | Dize | varlık |
| Dosya bulunamamış izin ver | True ise bir dosya bulunmazsa bir hata oluşturulmaz | hayır | `true` veya `false` | ıgnorenofilesfound |

#### <a name="cdm-source-example"></a>CDM kaynak örneği

Aşağıdaki görüntü, veri akışları eşleme içindeki bir CDM kaynak yapılandırmasına örnektir.

![CDM kaynağı](media/format-common-data-model/data-flow-source.png)

İlişkili veri akışı betiği:

```
source(output(
        ServingSizeId as integer,
        ServingSize as integer,
        ServingSizeUomId as string,
        ServingSizeNote as string,
        LastModifiedDate as timestamp
    ),
    allowSchemaDrift: true,
    validateSchema: false,
    entity: 'ServingSize.cdm.json/ServingSize',
    format: 'cdm',
    manifestType: 'manifest',
    manifestName: 'ServingSizeManifest',
    entityPath: 'ServingSize',
    corpusPath: 'ProductAhold_Updated',
    corpusStore: 'adlsgen2',
    adlsgen2_fileSystem: 'models',
    folderPath: 'ServingSizeData',
    fileSystem: 'data') ~> CDMSource
```


### <a name="sink-properties"></a>Havuz özellikleri

Aşağıdaki tabloda bir CDM havuzu tarafından desteklenen özellikler listelenmiştir.

| Adı | Açıklama | Gerekli | İzin verilen değerler | Veri akışı betiği özelliği |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Biçimlendir | Biçim olmalıdır`cdm` | evet | `cdm` | biçim |
| Kök konumu: kapsayıcı | CDM klasörünün kapsayıcı adı | evet | Dize | Biçimlendiri |
| Kök konumu: klasör yolu | CDM klasörünün kök klasör konumu | evet | Dize | folderPath |
| Bildirim dosyası: varlık yolu | Kök klasör içindeki varlığın klasör yolu | hayır | Dize | entityPath |
| Bildirim dosyası: bildirim adı | Bildirim dosyasının adı. Varsayılan değer ' default ' değeridir | Hayır | Dize | manifestName |
| Şemaya bağlı hizmet | Yapı 'nin bulunduğu bağlı hizmet | evet | `'adlsgen2'` veya `'github'` | corpusStore | 
| Varlık başvurusu kapsayıcısı | Kapsayıcı yapı | Evet, yapı ADLS 2. | Dize | adlsgen2_fileSystem |
| Varlık başvuru deposu | GitHub deposu adı | Evet, GitHub 'da yapı | Dize | github_repository |
| Varlık başvuru dalı | GitHub depo dalı | Evet, GitHub 'da yapı | Dize |  github_branch |
| Corpus klasörü | Yapı 'nin kök konumu | evet | Dize | corpusPath |
| Corpus varlığı | Varlık başvurusunun yolu | evet | Dize | varlık |
| Bölüm yolu | Bölümün yazılacağı konum | hayır | Dize | partitionPath |
| Klasörü temizle | Hedef klasör, yazma işleminden önce silinirse | hayır | `true` veya `false` | kesilemedi |
| Biçim türü | Parquet biçimini belirtmeyi seçin | hayır | `parquet`belirtilmişse | alt biçim |
| Sütun sınırlayıcısı | DelimitedText 'e yazıyorsanız sütunları sınırlandırmak | Evet, DelimitedText 'e yazıyorsanız | Dize | columnDelimiter |
| Üst bilgi olarak ilk satır | DelimitedText kullanılıyorsa, sütun adlarının üst bilgi olarak eklenip eklenmeyeceğini belirtir | hayır | `true` veya `false` | columnNamesAsHeader |

#### <a name="cdm-sink-example"></a>CDM havuz örneği

Aşağıdaki görüntü, veri akışları eşleme içindeki bir CDM havuz yapılandırması örneğidir.

![CDM kaynağı](media/format-common-data-model/data-flow-sink.png)

İlişkili veri akışı betiği:

```
CDMSource sink(allowSchemaDrift: true,
    validateSchema: false,
    entity: 'ServingSize.cdm.json/ServingSize',
    format: 'cdm',
    entityPath: 'ServingSize',
    manifestName: 'ServingSizeManifest',
    corpusPath: 'ProductAhold_Updated',
    partitionPath: 'adf',
    folderPath: 'ServingSizeData',
    fileSystem: 'cdm',
    subformat: 'parquet',
    corpusStore: 'adlsgen2',
    adlsgen2_fileSystem: 'models',
    truncate: true,
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> CDMSink

```

## <a name="next-steps"></a>Sonraki adımlar

Eşleme veri akışında bir [kaynak dönüşümü](data-flow-source.md) oluşturun.
