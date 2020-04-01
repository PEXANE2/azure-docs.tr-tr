---
title: Azure Veri Fabrikası'nda kopyalama etkinliği
description: Azure Veri Fabrikası'ndaki Kopyalama etkinliği hakkında bilgi edinin. Desteklenen bir kaynak veri deposundan desteklenen bir lavabo veri deposuna verileri kopyalamak için kullanabilirsiniz.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: jingwang
ms.openlocfilehash: edb80c604951a140d21e3775eec3f1dc6d55af73
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80421406"
---
# <a name="copy-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda kopyalama etkinliği

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Veri Fabrikası sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-data-movement-activities.md)
> * [Geçerli sürüm](copy-activity-overview.md)

Azure Veri Fabrikası'nda, şirket içinde ve bulutta bulunan veri depoları arasında verileri kopyalamak için Kopyala etkinliğini kullanabilirsiniz. Verileri kopyaladıktan sonra, daha fazla dönüştürmek ve analiz etmek için diğer etkinlikleri kullanabilirsiniz. İş zekası (BI) ve uygulama tüketimi için dönüşüm ve analiz sonuçlarını yayımlamak için Kopyalama etkinliğini de kullanabilirsiniz.

![Kopyalama etkinliğinin rolü](media/copy-activity-overview/copy-activity.png)

Kopyalama etkinliği [tümleştirme çalışma zamanında](concepts-integration-runtime.md)yürütülür. Farklı veri kopyalama senaryoları için farklı türde tümleştirme çalışma süreleri kullanabilirsiniz:

* Herhangi bir IP'den internet üzerinden herkese açık olan iki veri deposu arasındaki verileri kopyalarken, kopyalama etkinliği için Azure tümleştirme çalışma süresini kullanabilirsiniz. Bu tümleştirme çalışma süresi güvenli, güvenilir, ölçeklenebilir ve [genel olarak kullanılabilir.](concepts-integration-runtime.md#integration-runtime-location)
* Şirket içinde veya erişim denetimi olan bir ağa (örneğin, Azure sanal ağı) bulunan veri depolarına veri kopyalarken, kendi kendine barındırılan bir tümleştirme çalışma zamanı ayarlamanız gerekir.

Tümleştirme çalışma zamanının her kaynak ve lavabo veri deposuyla ilişkilendirilmesi gerekir. Kopyalama etkinliğinin hangi tümleştirme çalışma zamanının kullanılacağını nasıl belirlediği hakkında bilgi [için](concepts-integration-runtime.md#determining-which-ir-to-use)bkz.

Verileri bir kaynaktan lavaboya kopyalamak için, Kopyalama etkinliğini çalıştıran hizmet aşağıdaki adımları gerçekleştirir:

1. Kaynak veri deposundan gelen verileri okur.
2. Serileştirme/deserialization, sıkıştırma/dekompresyon, sütun eşleme ve benzeri gerçekleştirir. Bu işlemleri giriş veri kümesi, çıktı veri kümesi ve Kopyalama etkinliğinin yapılandırmasını temel alır.
3. Verileri lavabo/hedef veri deposuna yazar.

![Kopyalama etkinliğine genel bakış](media/copy-activity-overview/copy-activity-overview.png)

## <a name="supported-data-stores-and-formats"></a>Desteklenen veri depoları ve biçimleri

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

### <a name="supported-file-formats"></a>Desteklenen dosya biçimleri

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Dosyaları iki dosya tabanlı veri deposu arasında olduğu gibi kopyalamak için Kopyala etkinliğini kullanabilirsiniz ve bu durumda veriler herhangi bir serileştirme veya deserialization olmadan verimli bir şekilde kopyalanır. Buna ek olarak, belirli bir biçimdeki dosyaları ayrışturabilir veya oluşturabilirsiniz, örneğin, aşağıdakileri gerçekleştirebilirsiniz:

* Şirket içi BIR SQL Server veritabanındaki verileri kopyalayın ve Parke formatında Azure Veri Gölü Depolama Gen2'ye yazın.
* Dosyaları şirket içi bir dosya sisteminden metin (CSV) biçiminde kopyalayın ve Avro biçiminde Azure Blob depolamasına yazın.
* Sıkıştırılmış dosyaları şirket içi bir dosya sisteminden kopyalayın, anında sıkıştırmayı debzorlayın ve çıkarılan dosyaları Azure Veri Gölü Depolama Gen2'ye yazın.
* Verileri Azure Blob depolamaalanından Gzip sıkıştırılmış metin (CSV) biçiminde kopyalayın ve Azure SQL Veritabanı'na yazın.
* Serileştirme/deserialization veya sıkıştırma/dekompresyon gerektiren daha birçok etkinlik.

## <a name="supported-regions"></a>Desteklenen bölgeler

Kopyalama etkinliğini etkinleştiren hizmet, [Azure tümleştirme çalışma zamanı konumlarında](concepts-integration-runtime.md#integration-runtime-location)listelenen bölgelerve coğrafyalarda genel olarak kullanılabilir. Küresel olarak kullanılabilir topoloji, genellikle bölgeler arası atlamaları önleyen verimli veri hareketi sağlar. Belirli bir bölgedeki Veri Fabrikası'nın kullanılabilirliğini ve veri hareketini denetlemek için [bölgelere göre ürünlere](https://azure.microsoft.com/regions/#services) bakın.

## <a name="configuration"></a>Yapılandırma

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Genel olarak, Azure Veri Fabrikası'ndaki Kopyalama etkinliğini kullanmak için şunları yapmanız gerekir:

1. **Kaynak veri deposu ve lavabo veri deposu için bağlantılı hizmetler oluşturun.** Desteklenen bağlayıcıların listesini desteklenen [veri depoları ve biçimleri](#supported-data-stores-and-formats) bölümünde bulabilirsiniz. Yapılandırma bilgileri ve desteklenen özellikler için bağlayıcı makalenin "Bağlantılı hizmet özellikleri" bölümüne bakın. 
2. **Kaynak ve lavabo için veri kümeleri oluşturun.** Yapılandırma bilgileri ve desteklenen özellikler için kaynak ve lavabo bağlayıcısı makalelerinin "Veri kümesi özellikleri" bölümlerine bakın.
3. **Kopyalama etkinliği yle birlikte bir ardışık hatlar hattı oluşturun.** Sonraki bölüm bir örnek sağlar.

### <a name="syntax"></a>Sözdizimi

Bir Kopya etkinliği aşağıdaki şablondesteklenen özelliklerin tam listesini içerir. Senaryonuza uyan olanları belirtin.

```json
"activities":[
    {
        "name": "CopyActivityTemplate",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<source dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<sink dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>",
                <properties>
            },
            "sink": {
                "type": "<sink type>"
                <properties>
            },
            "translator":
            {
                "type": "TabularTranslator",
                "columnMappings": "<column mapping>"
            },
            "dataIntegrationUnits": <number>,
            "parallelCopies": <number>,
            "enableStaging": true/false,
            "stagingSettings": {
                <properties>
            },
            "enableSkipIncompatibleRow": true/false,
            "redirectIncompatibleRowSettings": {
                <properties>
            }
        }
    }
]
```

#### <a name="syntax-details"></a>Sözdizimi ayrıntıları

| Özellik | Açıklama | Gerekli mi? |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği için,`Copy` | Evet |
| Giriş | Kaynak verilere işaret eden oluşturduğunuz veri kümesini belirtin. Kopyalama etkinliği yalnızca tek bir girişi destekler. | Evet |
| Çıkış | Oluşturduğunuz veri kümesini, lavabo verilerine işaret eden belirtin. Kopyalama etkinliği yalnızca tek bir çıktıyı destekler. | Evet |
| typeProperties | Kopya etkinliğini yapılandırmak için özellikleri belirtin. | Evet |
| source | Veri almak için kopya kaynağı türünü ve ilgili özellikleri belirtin.<br/>Daha fazla bilgi için, [Desteklenen veri depolarında ve biçimlerinde](#supported-data-stores-and-formats)listelenen bağlayıcı makalesindeki "Etkinlik özelliklerini kopyalama" bölümüne bakın. | Evet |
| Lavabo | Kopya lavabo türünü ve veri yazmak için ilgili özellikleri belirtin.<br/>Daha fazla bilgi için, [Desteklenen veri depolarında ve biçimlerinde](#supported-data-stores-and-formats)listelenen bağlayıcı makalesindeki "Etkinlik özelliklerini kopyalama" bölümüne bakın. | Evet |
| çevirmen | Kaynaktan batmaya açık sütun eşlemelerini belirtin. Bu özellik, varsayılan kopyalama davranışı gereksinimlerinizi karşılamadığında geçerlidir.<br/>Daha fazla bilgi [için, kopya etkinliğinde Şema eşleme](copy-activity-schema-and-type-mapping.md)bakın. | Hayır |
| dataIntegrationUnits | [Azure tümleştirme çalışma zamanının](concepts-integration-runtime.md) veri kopyalama için kullandığı güç miktarını temsil eden bir ölçü belirtin. Bu birimler eskiden bulut Veri Hareketi Birimleri (DMU) olarak biliniyordu. <br/>Daha fazla bilgi için [Bkz. Veri Tümleştirme Birimleri.](copy-activity-performance-features.md#data-integration-units) | Hayır |
| parallelKopya | Kaynaktan gelen verileri okurken ve verileri lavaboya yazarken Kopyalama etkinliğinin kullanmasını istediğiniz paralelliği belirtin.<br/>Daha fazla bilgi için [Paralel kopyaya](copy-activity-performance-features.md#parallel-copy)bakın. | Hayır |
| Korumak | Veri kopyalama sırasında meta verilerin/ALA'ların korunup korunmayacağını belirtin. <br/>Daha fazla bilgi için [bkz.](copy-activity-preserve-metadata.md) |Hayır |
| enableStaging<br/>evrelemeAyarlar | Verileri kaynaktan lavaboya doğrudan kopyalamak yerine geçici verileri Blob depolama alanında sahneleyip sahnelemeyeceğiniz belirtin.<br/>Yararlı senaryolar ve yapılandırma ayrıntıları hakkında daha fazla bilgi için [Aşamalı kopyaya](copy-activity-performance-features.md#staged-copy)bakın. | Hayır |
| etkinleştirinSkipIncompatibleRow<br/>yeniden YönlendirmeUyumsuzSatır Ayarları| Verileri kaynaktan batmaya kopyalarken uyumsuz satırları nasıl işleyeceğinizi seçin.<br/>Daha fazla bilgi için [bkz.](copy-activity-fault-tolerance.md) | Hayır |

## <a name="monitoring"></a>İzleme

Azure Veri Fabrikası'nda çalışan Kopyalama etkinliğini hem görsel hem de programlı olarak izleyebilirsiniz. Ayrıntılar için [bkz.](copy-activity-monitoring.md)

## <a name="incremental-copy"></a>Artımlı kopya

Veri Fabrikası, delta verilerini bir kaynak veri deposundan bir lavabo veri deposuna artımlı olarak kopyalamanızı sağlar. Ayrıntılar için [Bkz. Öğretici: Verileri aşamalı olarak kopyalayın.](tutorial-incremental-copy-overview.md)

## <a name="performance-and-tuning"></a>Performans ve ayar

[Kopyalama etkinliği izleme](copy-activity-monitoring.md) deneyimi, etkinlik çalışmalarınızın her biri için kopyalama performansı istatistiklerini gösterir. [Kopyalama etkinliği performansı ve ölçeklenebilirlik kılavuzu,](copy-activity-performance.md) Azure Veri Fabrikası'ndaki Kopyalama etkinliği aracılığıyla veri hareketinin performansını etkileyen önemli faktörleri açıklar. Ayrıca, test sırasında gözlenen performans değerlerini listeler ve Kopyalama etkinliğinin performansını nasıl optimize edilebildiğini tartışır.

## <a name="resume-from-last-failed-run"></a>Son başarısız çalıştırmadan devam

Kopyalama etkinliği, dosya tabanlı mağazalar arasında ikili biçimde olduğu gibi büyük boyutlu dosyaları kopyaladiğinizde ve klasör/dosya hiyerarşisini kaynaktan lavaboya ,örneğin Amazon S3'ten Azure Veri Gölü Depolama Gen2'ye geçirmek gibi korumayı seçtiğinizde, son başarısız çalışmadan devam etmeyi destekler. Aşağıdaki dosya tabanlı bağlayıcılar için geçerlidir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Veri Gölü Depolama Gen1](connector-azure-data-lake-store.md), [Azure Veri Gölü Depolama Gen2](connector-azure-data-lake-storage.md), [Azure Dosya Depolama](connector-azure-file-storage.md), [Dosya Sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Bulut Depolama](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), ve [SFTP](connector-sftp.md).

Kopyalama etkinliği devamını aşağıdaki iki şekilde kullanabilirsiniz:

- **Etkinlik düzeyi yeniden deneme:** Kopya etkinliği yeniden deneme sayısını ayarlayabilirsiniz. Ardışık işlem sırasında, bu kopyalama etkinliği çalıştırılırsa, sonraki otomatik yeniden deneme son denemenin hata noktasından başlar.
- **Başarısız olan etkinlikten yeniden çalıştırma:** Ardışık yürütme tamamlandıktan sonra, ADF Kullanıcı Arabirimi izleme görünümünde veya programlı olarak başarısız olan etkinlikten yeniden çalıştırmayı da tetikleyebilirsiniz. Başarısız olan etkinlik bir kopyalama etkinliğiyse, ardışık işlem yalnızca bu etkinlikten yeniden çalışmaz, aynı zamanda önceki çalıştırmanın hata noktasından devam eder.

    ![Özgeçmişi kopyala](media/copy-activity-overview/resume-copy.png)

Unutulmaması gereken birkaç nokta:

- Özgeçmiş dosya düzeyinde olur. Bir sonraki çalıştırmada bir dosyayı kopyalarken kopyalama etkinliği başarısız olursa, bu özel dosya yeniden kopyalanır.
- Özgeçmişin düzgün çalışması için, yeniden çalıştırmalar arasındaki kopyalama etkinliği ayarlarını değiştirmeyin.
- Amazon S3, Azure Blob, Azure Veri Gölü Depolama Gen2 ve Google Cloud Depolama'dan verileri kopyaladiğinizde, kopyalama etkinliği rasgele sayıda kopyalanan dosyadan devam edebilir. Dosya tabanlı bağlayıcıların geri kalanı kaynak olarak olsa da, şu anda kopyalama etkinliği genellikle on binlerce dosya aralığında sınırlı sayıdan devam destekler ve dosya yollarının uzunluğuna bağlı olarak değişir; bu sayının dışındaki dosyalar yeniden gösterimsırasında yeniden kopyalanır.

İkili dosya kopyalama dışındaki diğer senaryolar için, kopyalama etkinliği yeniden çalıştırma baştan başlar.

## <a name="preserve-metadata-along-with-data"></a>Meta verileri verilerle birlikte koruma

Verileri kaynaktan batmaya kopyalarken, veri gölü geçişi gibi senaryolarda, kopyalama etkinliğini kullanarak verilerle birlikte meta verileri ve AK'ları da korumayı seçebilirsiniz. Bkz. Ayrıntılar için [meta verileri koruyun.](copy-activity-preserve-metadata.md)

## <a name="schema-and-data-type-mapping"></a>Şema ve veri türü eşleme

Kopya etkinliği kaynak verilerinizi lavabonuzla nasıl eşler hakkında bilgi almak için [Şema ve veri türü eşlemeyi](copy-activity-schema-and-type-mapping.md) ne resilere bakın.

## <a name="add-additional-columns-during-copy"></a>Kopyalama sırasında ek sütunekleme

Kaynak veri deposundan batmaya kadar veri kopyalamanın yanı sıra, batmaya kadar kopyalamak üzere ek veri sütunları eklemek için de yapılandırabilirsiniz. Örnek:

- Dosya tabanlı kaynaktan kopyalandığında, verilerin hangi dosyadan geldiğini izlemek için göreli dosya yolunu ek bir sütun olarak depolayın.
- Ardışık ad/ardışık sistem kimliği gibi ADF sistem değişkenlerini eklemek veya yukarı akış etkinliğinin çıktısından diğer dinamik değeri depolamak için ADF ifadesini içeren bir sütun ekleyin.
- Akış aşağı tüketim gereksiniminizi karşılamak için statik değeri olan bir sütun ekleyin.

Kopyalama etkinliği kaynak sekmesinde aşağıdaki yapılandırmayı bulabilirsiniz: 

![Kopyalama etkinliğinde ek sütunlar ekleme](./media/copy-activity-overview/copy-activity-add-additional-columns.png)

>[!TIP]
>Bu özellik en son dataset modeliile çalışır. UI'den bu seçeneği görmüyorsanız, yeni bir veri kümesi oluşturmayı deneyin.

Programlı olarak yapılandırmak için, `additionalColumns` özelliği kopya etkinlik kaynağınıza ekleyin:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| ekSütunlar | Batmak için kopyalamak için ek veri sütunları ekleyin.<br><br>`additionalColumns` Dizinin altındaki her nesne fazladan bir sütunu temsil eder. Sütun `name` adını tanımlar ve bu `value` sütunun veri değerini gösterir.<br><br>İzin verilen veri değerleri şunlardır:<br>- **`$$FILEPATH`**- ayrılmış bir değişken, kaynak dosyaların göreli yolunu veri kümesinde belirtilen klasör yoluna depolamayı gösterir. Dosya tabanlı kaynağa uygulayın.<br>- **Ifa -de**<br>- **Statik değer** | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyWithAdditionalColumns",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "<source type>",
                "additionalColumns": [
                    {
                        "name": "filePath",
                        "value": "$$FILEPATH"
                    },
                    {
                        "name": "pipelineName",
                        "value": {
                            "value": "@pipeline().Pipeline",
                            "type": "Expression"
                        }
                    },
                    {
                        "name": "staticValue",
                        "value": "sampleValue"
                    }
                ],
                ...
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="fault-tolerance"></a>Hataya dayanıklılık

Varsayılan olarak, Kopya etkinliği veri kopyalamayı durdurur ve kaynak veri satırları büzerek veri satırlarıyla uyumsuz olduğunda bir hata döndürür. Kopyanın başarılı olması için, Kopya etkinliği uyumsuz satırları atlayacak ve günlüğe kaydacak şekilde yapılandırabilir ve yalnızca uyumlu verileri kopyalayabilirsiniz. Ayrıntılar için [kopyalama etkinliği hata toleransı](copy-activity-fault-tolerance.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki hızlı başlangıçlara, öğreticilere ve örneklere bakın:

- [Aynı Azure Blob depolama hesabında verileri bir konumdan başka bir konuma kopyalama](quickstart-create-data-factory-dot-net.md)
- [Azure Blob depolamadan Azure SQL Veritabanı'na veri kopyalama](tutorial-copy-data-dot-net.md)
- [Şirket içi SQL Server veritabanındaki verileri Azure'a kopyalama](tutorial-hybrid-copy-powershell.md)
