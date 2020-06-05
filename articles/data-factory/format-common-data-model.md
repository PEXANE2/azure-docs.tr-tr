---
title: Ortak veri modeli biçimi
description: Ortak veri modeli meta veri sistemini kullanarak verileri dönüştürme
author: djpmsft
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/03/2020
ms.author: daperlov
ms.openlocfilehash: 2faa14ef7724bb9cccfe425569539f5ef0621a28
ms.sourcegitcommit: b55d1d1e336c1bcd1c1a71695b2fd0ca62f9d625
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84435397"
---
# <a name="common-data-model-format-in-azure-data-factory"></a>Azure Data Factory ortak veri modeli biçimi
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Common Data Model (CDM) meta veri sistemi, verilerin ve anlamının uygulamalar ve iş süreçlerinde kolayca paylaşılmasını olanaklı kılar. Daha fazla bilgi için bkz. [ortak veri modeline](https://docs.microsoft.com/common-data-model/) genel bakış.

Azure Data Factory, kullanıcılar, veri akışlarını eşleme kullanarak [Azure Data Lake Store Gen2](connector-azure-data-lake-storage.md) (ADLS 2.) içinde depolanan CDM varlıklarına ve bu varlıkları dönüştürebilir.

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Ortak veri modeli, kaynak ve havuz olarak eşleme verilerinde bir [satır içi](data-flow-source.md#inline-datasets) veri akışı olarak kullanılabilir.

### <a name="source-properties"></a>Kaynak özellikleri

Aşağıdaki tabloda bir CDM kaynağı tarafından desteklenen özellikler listelenmiştir.

| Name | Description | Gerekli | İzin verilen değerler | Veri akışı betiği özelliği |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Biçimlendir | Her zaman`cdm` | evet | `cdm` | biçim |
| Meta veri biçimi | Verilerin Varlık başvurusunun bulunduğu yer. CDM sürüm 1,0 kullanıyorsanız, bildirim ' ı seçin. 1,0 öncesi bir CDM sürümü kullanıyorsanız, model. json ' ı seçin. | Yes | `'manifest'` veya `'model'` | manifestType |
| Kök konumu: kapsayıcı | CDM klasörünün kapsayıcı adı | evet | Dize | Biçimlendiri |
| Kök konumu: klasör yolu | CDM klasörünün kök klasör konumu | evet | Dize | folderPath |
| Bildirim dosyası: varlık yolu | Kök klasör içindeki varlığın klasör yolu | hayır | Dize | entityPath |
| Bildirim dosyası: bildirim adı | Bildirim dosyasının adı  | Evet, bildirim kullanılıyorsa | Dize | manifestName |
| Son değiştirme ölçütü | En son değiştirildiklerinde dosyaları filtrelemek için seçin | hayır | Zaman damgası | Modıfıedafter <br> modifiedBefore | 
| Şemaya bağlı hizmet | Yapı 'nin bulunduğu bağlı hizmet | Evet, bildirim kullanılıyorsa | `'adlsgen2'` veya `'github'` | corpusStore | 
| Varlık başvurusu kapsayıcısı | Kapsayıcı yapı | Evet, bildirim ve yapı ADLS 2. kullanılıyorsa | Dize | adlsgen2_fileSystem |
| Varlık başvuru deposu | GitHub deposu adı | Evet, GitHub 'da bildirim ve yapı kullanılıyorsa | Dize | github_repository |
| Varlık başvuru dalı | GitHub depo dalı | Evet, GitHub 'da bildirim ve yapı kullanılıyorsa | Dize |  github_branch |
| Corpus klasörü | Yapı 'nin kök konumu | Evet, bildirim kullanılıyorsa | Dize | corpusPath |
| Corpus varlığı | Varlık başvurusunun yolu | evet | Dize | varlık |
| Dosya bulunamamış izin ver | True ise bir dosya bulunmazsa bir hata oluşturulmaz | hayır | `true` veya `false` | ıgnorenofilesfound |

![CDM kaynağı](media/format-common-data-model/data-flow-source.png)

### <a name="sink-properties"></a>Havuz özellikleri

Aşağıdaki tabloda bir CDM havuzu tarafından desteklenen özellikler listelenmiştir.

| Name | Description | Gerekli | İzin verilen değerler | Veri akışı betiği özelliği |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Biçimlendir | Her zaman`cdm` | evet | `cdm` | biçim |
| Kök konumu: kapsayıcı | CDM klasörünün kapsayıcı adı | evet | Dize | Biçimlendiri |
| Kök konumu: klasör yolu | CDM klasörünün kök klasör konumu | evet | Dize | folderPath |
| Bildirim dosyası: varlık yolu | Kök klasör içindeki varlığın klasör yolu | hayır | Dize | entityPath |
| Bildirim dosyası: bildirim adı | Bildirim dosyasının adı  | evet | Dize | manifestName |
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

![CDM kaynağı](media/format-common-data-model/data-flow-sink.png)

## <a name="next-steps"></a>Sonraki adımlar

Eşleme veri akışında bir [kaynak dönüşümü](data-flow-source.md) oluşturun.