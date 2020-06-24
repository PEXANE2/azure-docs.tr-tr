---
title: Common Data Model biçimi
description: Ortak veri modeli meta veri sistemini kullanarak verileri dönüştürme
author: djpmsft
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: daperlov
ms.openlocfilehash: 5e75f2203552a69e50ed16176525429c6c9d8810
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84807810"
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

Aşağıdaki tabloda bir CDM kaynağı tarafından desteklenen özellikler listelenmiştir. Bu özellikleri **kaynak seçenekleri** sekmesinde düzenleyebilirsiniz.

| Name | Description | Gerekli | İzin verilen değerler | Veri akışı betiği özelliği |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Biçimlendir | Biçim olmalıdır`cdm` | evet | `cdm` | biçim |
| Meta veri biçimi | Verilerin Varlık başvurusunun bulunduğu yer. CDM sürüm 1,0 kullanıyorsanız, bildirim ' ı seçin. 1,0 öncesi bir CDM sürümü kullanıyorsanız, üzerinde model.js' yi seçin. | Yes | `'manifest'` veya `'model'` | manifestType |
| Kök konumu: kapsayıcı | CDM klasörünün kapsayıcı adı | evet | Dize | Biçimlendiri |
| Kök konumu: klasör yolu | CDM klasörünün kök klasör konumu | evet | Dize | folderPath |
| Bildirim dosyası: varlık yolu | Kök klasör içindeki varlığın klasör yolu | hayır | Dize | entityPath |
| Bildirim dosyası: bildirim adı | Bildirim dosyasının adı. Varsayılan değer ' default ' değeridir  | No | Dize | manifestName |
| Son değiştirme ölçütü | En son değiştirildiklerinde dosyaları filtrelemek için seçin | hayır | Zaman damgası | Modıfıedafter <br> modifiedBefore | 
| Şemaya bağlı hizmet | Yapı 'nin bulunduğu bağlı hizmet | Evet, bildirim kullanılıyorsa | `'adlsgen2'` veya `'github'` | corpusStore | 
| Varlık başvurusu kapsayıcısı | Kapsayıcı yapı | Evet, bildirim ve yapı ADLS 2. kullanılıyorsa | Dize | adlsgen2_fileSystem |
| Varlık başvuru deposu | GitHub deposu adı | Evet, GitHub 'da bildirim ve yapı kullanılıyorsa | Dize | github_repository |
| Varlık başvuru dalı | GitHub depo dalı | Evet, GitHub 'da bildirim ve yapı kullanılıyorsa | Dize |  github_branch |
| Corpus klasörü | Yapı 'nin kök konumu | Evet, bildirim kullanılıyorsa | Dize | corpusPath |
| Corpus varlığı | Varlık başvurusunun yolu | evet | Dize | varlık |
| Dosya bulunamamış izin ver | True ise bir dosya bulunmazsa bir hata oluşturulmaz | hayır | `true` veya `false` | ıgnorenofilesfound |

#### <a name="import-schema"></a>Şemayı içeri aktar

CDM yalnızca satır içi veri kümesi olarak kullanılabilir ve varsayılan olarak ilişkili bir şemaya sahip değildir. Sütun meta verilerini almak için **İzdüşüm** sekmesindeki **şemayı içeri aktar** düğmesine tıklayın. Bu, Corpus tarafından belirtilen sütun adlarına ve veri türlerine başvuruda bulunmak için izin verir. Şemayı içeri aktarmak için bir [veri akışı hata ayıklama oturumunun](concepts-data-flow-debug-mode.md) etkin olması gerekir.


### <a name="cdm-source-data-flow-script-example"></a>CDM kaynak veri akışı betiği örneği

```
source(output(
        ProductSizeId as integer,
        ProductColor as integer,
        CustomerId as string,
        Note as string,
        LastModifiedDate as timestamp
    ),
    allowSchemaDrift: true,
    validateSchema: false,
    entity: 'Product.cdm.json/Product',
    format: 'cdm',
    manifestType: 'manifest',
    manifestName: 'ProductManifest',
    entityPath: 'Product',
    corpusPath: 'Products',
    corpusStore: 'adlsgen2',
    adlsgen2_fileSystem: 'models',
    folderPath: 'ProductData',
    fileSystem: 'data') ~> CDMSource
```

### <a name="sink-properties"></a>Havuz özellikleri

Aşağıdaki tabloda bir CDM havuzu tarafından desteklenen özellikler listelenmiştir. Bu özellikleri **Ayarlar** sekmesinde düzenleyebilirsiniz.

| Name | Description | Gerekli | İzin verilen değerler | Veri akışı betiği özelliği |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Biçimlendir | Biçim olmalıdır`cdm` | evet | `cdm` | biçim |
| Kök konumu: kapsayıcı | CDM klasörünün kapsayıcı adı | evet | Dize | Biçimlendiri |
| Kök konumu: klasör yolu | CDM klasörünün kök klasör konumu | evet | Dize | folderPath |
| Bildirim dosyası: varlık yolu | Kök klasör içindeki varlığın klasör yolu | hayır | Dize | entityPath |
| Bildirim dosyası: bildirim adı | Bildirim dosyasının adı. Varsayılan değer ' default ' değeridir | No | Dize | manifestName |
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

### <a name="cdm-sink-data-flow-script-example"></a>CDM havuz veri akışı betiği örneği

İlişkili veri akışı betiği:

```
CDMSource sink(allowSchemaDrift: true,
    validateSchema: false,
    entity: 'Product.cdm.json/Product',
    format: 'cdm',
    entityPath: 'ProductSize',
    manifestName: 'ProductSizeManifest',
    corpusPath: 'Products',
    partitionPath: 'adf',
    folderPath: 'ProductSizeData',
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
