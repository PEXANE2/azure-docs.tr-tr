---
title: Common Data Model biçimi
description: Ortak veri modeli meta veri sistemini kullanarak verileri dönüştürme
author: djpmsft
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 08/05/2020
ms.author: daperlov
ms.openlocfilehash: 483e26cf4044b909c8d7923cfd74bd6fcf871e2a
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87905314"
---
# <a name="common-data-model-format-in-azure-data-factory"></a>Azure Data Factory ortak veri modeli biçimi
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Common Data Model (CDM) meta veri sistemi, verilerin ve anlamının uygulamalar ve iş süreçlerinde kolayca paylaşılmasını olanaklı kılar. Daha fazla bilgi için bkz. [ortak veri modeline](https://docs.microsoft.com/common-data-model/) genel bakış.

Azure Data Factory, kullanıcılar, CDM varlıklarındaki verileri, veri akışlarını eşleme kullanarak [Azure Data Lake Store Gen2](connector-azure-data-lake-storage.md) (ADLS 2.) içinde depolanan hem model.jshem de bildirim formunda dönüştürebilir. Ayrıca, verilerinizi CSV veya Parquet biçiminde bölümlenmiş klasörlerde barındıracak CDM varlık başvurularını kullanarak CDM biçiminde veri havuzu oluşturabilirsiniz. 

> [!NOTE]
> ADF veri akışları için ortak veri modeli (CDM) biçim Bağlayıcısı Şu anda genel önizleme olarak kullanılabilir.

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Ortak veri modeli, kaynak ve havuz olarak eşleme verilerinde bir [satır içi](data-flow-source.md#inline-datasets) veri akışı olarak kullanılabilir.

> [!NOTE]
> CDM varlıklarını yazarken, zaten bir başvuru olarak kullanmak üzere tanımlanmış bir CDM varlık tanımına (meta veri şeması) sahip olmanız gerekir. ADF veri akışı havuzu bu CDM varlık dosyasını okur ve alan eşlemesi için şemayı havuza aktarır.

### <a name="source-properties"></a>Kaynak özellikleri

Aşağıdaki tabloda bir CDM kaynağı tarafından desteklenen özellikler listelenmiştir. Bu özellikleri **kaynak seçenekleri** sekmesinde düzenleyebilirsiniz.

| Ad | Açıklama | Gerekli | İzin verilen değerler | Veri akışı betiği özelliği |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Biçim | Biçim olmalıdır`cdm` | evet | `cdm` | biçim |
| Meta veri biçimi | Verilerin Varlık başvurusunun bulunduğu yer. CDM sürüm 1,0 kullanıyorsanız, bildirim ' ı seçin. 1,0 öncesi bir CDM sürümü kullanıyorsanız, üzerinde model.js' yi seçin. | Evet | `'manifest'` veya `'model'` | manifestType |
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

### <a name="sink-settings"></a>Havuz ayarları

* Yazmak istediğiniz varlığın tanımını içeren CDM varlık başvuru dosyasının üzerine gelin.

![varlık ayarları](media/data-flow/common-data-model-111.png "Varlık başvurusu")

* ADF 'nin varlıklarınızı yazmak için kullanmasını istediğiniz çıkış dosyalarının Bölüm yolunu ve biçimini tanımlayın.

![varlık biçimi](media/data-flow/common-data-model-222.png "Varlık biçimi")

* Çıktı dosyasının konumunu ve bildirim dosyasının konumunu ve adını ayarlayın.

![CDM konumu](media/data-flow/common-data-model-333.png "CDM konumu")


#### <a name="import-schema"></a>Şemayı içeri aktar

CDM yalnızca satır içi veri kümesi olarak kullanılabilir ve varsayılan olarak ilişkili bir şemaya sahip değildir. Sütun meta verilerini almak için **İzdüşüm** sekmesindeki **şemayı içeri aktar** düğmesine tıklayın. Bu, Corpus tarafından belirtilen sütun adlarına ve veri türlerine başvuruda bulunmak için izin verir. Şemayı içeri aktarmak için bir [veri akışı hata ayıklama oturumunun](concepts-data-flow-debug-mode.md) etkin olması ve işaret etmek için var olan bir CDM varlık tanımı dosyanızın olması gerekir.

Havuz dönüşümünde veri akışı sütunlarını varlık özellikleriyle eşlerken, "eşleme" sekmesine tıklayın ve "şemayı Içeri aktar" seçeneğini belirleyin. ADF, havuz seçeneklerinizde işaret ettiğiniz varlık başvurusunu okuyarak hedef CDM şemasına eşlemenizi sağlar.

![CDM havuz ayarları](media/data-flow/common-data-model-444.png "CDM eşleme")

> [!NOTE]
>  Power BI veya güç platformu veri akışlarından kaynaklanan kaynak türünde model.jskullanırken, kaynak dönüşümünde "Corpus yolu null veya boş" hatalarla karşılaşabilirsiniz. Bunun nedeni, dosyadaki model.jsbölüm konumu yolunun biçimlendirme sorunlarından kaynaklanıyor olabilir. Bu hatayı onarmak için aşağıdaki adımları izleyin: 

1. Dosyadaki model.jsmetin düzenleyicisinde aç
2. Bölümleri bulun. Location özelliği 
3. "Blob.core.windows.net" öğesini "dfs.core.windows.net" olarak değiştir
4. URL 'deki "% 2F" kodlamasını "/" olarak düzeltin
 

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

| Ad | Açıklama | Gerekli | İzin verilen değerler | Veri akışı betiği özelliği |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Biçim | Biçim olmalıdır`cdm` | evet | `cdm` | biçim |
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
