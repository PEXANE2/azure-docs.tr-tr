---
title: Data Factory kullanarak Azure Data Lake Storage 1. veri kopyalama | Microsoft Docs
description: Data Factory kullanarak desteklenen kaynak veri depolarından verileri Azure Data Lake Store veya Data Lake Store desteklenen havuz depolarına kopyalamayı öğrenin.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: jingwang
ms.openlocfilehash: d64a8431cb0331b58afc635bf8cf9d0fe0f1f225
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70276040"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen1-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Data Lake Storage 1. veri kopyalama
> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Azure Data Factory sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-datalake-connector.md)
> * [Geçerli sürüm](connector-azure-data-lake-store.md)

Bu makalede, Azure Data Lake Storage 1. verileri içine ve öğesinden kopyalama özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [giriş makalesi](introduction.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Azure Data Lake Storage 1. Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak veya havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)

Özellikle, bu bağlayıcıyla şunları yapabilirsiniz:

- Aşağıdaki kimlik doğrulama yöntemlerinden birini kullanarak dosyaları kopyalayın: Azure kaynakları için hizmet sorumlusu veya yönetilen kimlikler.
- Dosyaları olduğu gibi kopyalayın veya [Desteklenen dosya biçimleri ve sıkıştırma codec bileşenleri](supported-file-formats-and-compression-codecs.md)ile dosyaları ayrıştırır veya oluşturun.

> [!IMPORTANT]
> Şirket içinde barındırılan tümleştirme çalışma zamanını kullanarak verileri kopyalarsanız, şirket güvenlik duvarını 443 numaralı bağlantı noktasına giden trafiğe `<ADLS account name>.azuredatalakestore.net` `login.microsoftonline.com/<tenant>/oauth2/token` izin verecek şekilde yapılandırın. İkincisi, tümleştirme çalışma zamanının erişim belirtecini almak için iletişim kurması gereken Azure Güvenlik belirteci hizmetidir.

## <a name="get-started"></a>başlarken

> [!TIP]
> Azure Data Lake Store bağlayıcısını nasıl kullanacağınızı öğrenmek için bkz. [Azure Data Lake Store verileri yükleme](load-azure-data-lake-store.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler Azure Data Lake Store özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında bilgiler sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Data Lake Store bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Özelliğin AzureDataLakeStore olarak ayarlanması gerekir. `type` | Evet |
| dataLakeStoreUri | Azure Data Lake Store hesabı hakkında bilgi. Bu bilgiler aşağıdaki biçimlerden birini alır: `https://[accountname].azuredatalakestore.net/webhdfs/v1` veya `adl://[accountname].azuredatalakestore.net/`. | Evet |
| subscriptionId | Data Lake Store hesabının ait olduğu Azure abonelik KIMLIĞI. | Havuz için gerekli |
| resourceGroupName | Data Lake Store hesabının ait olduğu Azure Kaynak grubu adı. | Havuz için gerekli |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Veri depolubir özel ağda yer alıyorsa Azure tümleştirme çalışma zamanını veya şirket içinde barındırılan tümleştirme çalışma zamanını kullanabilirsiniz. Bu özellik belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

### <a name="use-service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulamasını kullanma

Hizmet sorumlusu kimlik doğrulamasını kullanmak için Azure Active Directory bir uygulama varlığı kaydedin ve Data Lake Store erişim izni verin. Ayrıntılı adımlar için bkz. [hizmetten hizmete kimlik doğrulaması](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:

- Uygulama Kimliği
- Uygulama anahtarı
- Kiracı Kimliği

>[!IMPORTANT]
> Data Lake Store için hizmet sorumlusu uygun izni verin:
>- **Kaynak olarak**: **Veri Gezgini** > **erişimi**'nde, klasörleri ve alt klasörlerdeki dosyaları listelemek ve kopyalamak için en az **okuma ve yürütme** iznini verin. Ya da tek bir dosyayı kopyalamak için **okuma** izni verebilirsiniz. Özyinelemeli için **Bu klasöre ve tüm alt öğelere** ekleme yapabilir ve **erişim izni olarak ve varsayılan izin girişi**olarak ekleyebilirsiniz. Hesap düzeyinde erişim denetimi (ıAM) için gereksinim yoktur.
>- **Havuz olarak**: **Veri Gezgini** > **erişimi**'nde, klasörde alt öğeler oluşturmak için en az **yazma + yürütme** iznini verin. Özyinelemeli için **Bu klasöre ve tüm alt öğelere** ekleme yapabilir ve **erişim izni olarak ve varsayılan izin girişi**olarak ekleyebilirsiniz. Kopyalamak için bir Azure tümleştirme çalışma zamanı kullanıyorsanız (hem kaynak hem de havuz bulutta ise), ıAM 'de Data Lake Store bölge Data Factory algılamasını sağlamak için en az **okuyucu** rolü verin. Bu ıAM rolünü önlemek istiyorsanız, açıkça Data Lake Store konumuyla [bir Azure tümleştirme çalışma zamanı oluşturun](create-azure-integration-runtime.md#create-azure-ir) . Örneğin, Data Lake Store Batı Avrupa ise, konum "Batı Avrupa" olarak ayarlanmış bir Azure tümleştirme çalışma zamanı oluşturun. Bunları, aşağıdaki örnekte gösterildiği gibi Data Lake Store bağlantılı hizmette ilişkilendirin.

>[!NOTE]
>Kök bilgisayardan başlayarak klasörleri listelemek için, **kök düzeyinde "yürütme" izniyle**verilen hizmet sorumlusunun iznini ayarlamanız gerekir. Bu, şunu kullandığınızda geçerlidir:
>- **Veri aracını** yazar kopyalama Işlem hattına Kopyala.
>- Yazma sırasında bağlantıyı test etmek ve klasörlerde gezinmek için **Kullanıcı arabirimi Data Factory** .
>Kök düzeyinde izin verme hakkında endişeleriniz varsa, yazma sırasında bağlantıyı test etmeyi atlayın ve izin verildiğinde bir parametre yolu girin ve ardından belirtilen yoldan göz atmayı seçin. Kopyalama etkinliği, hizmet sorumlusu kopyalanacak dosyalar üzerinde uygun izinlerle verildiği sürece çalışıyor olur.

Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet |
| servicePrincipalKey | Uygulama anahtarını belirtin. Data Factory güvenli bir şekilde depolamak `SecureString` için bu alanı bir olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Evet |
| tenant | Uygulamanızın bulunduğu etki alanı adı veya kiracı KIMLIĞI gibi kiracı bilgilerini belirtin. Azure portalının sağ üst köşedeki fare getirerek geri alabilirsiniz. | Evet |

**Örnek:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a>Azure kaynakları kimlik doğrulaması için Yönetilen kimlikler kullanma

Veri Fabrikası ile ilişkilendirilebilen bir [yönetilen Azure kaynakları için kimliği](data-factory-service-identity.md), bu belirli veri fabrikası temsil eder. Bu yönetilen kimliği, kendi hizmet sorumlunuzu kullanmaya benzer şekilde Data Lake Store kimlik doğrulaması için doğrudan kullanabilirsiniz. Bu belirlenen fabrikasının Data Lake Store erişimine ve veri kopyalamasına izin verir.

Azure kaynakları ile kimlik doğrulaması için yönetilen kimlikleri kullanmak için:

1. Fabrikalarınızla birlikte üretilen "hizmet kimliği uygulama KIMLIĞI" değerini kopyalayarak [Veri Fabrikası tarafından yönetilen kimlik bilgilerini alın](data-factory-service-identity.md#retrieve-managed-identity) .
2. Bu notlardan sonra, hizmet sorumlusu için yaptığınız gibi Data Lake Store yönetilen kimlik erişimine izin verin.

>[!IMPORTANT]
> Data Lake Store ' de Data Factory tarafından yönetilen kimlik uygun iznini verdiğinizden emin olun:
>- **Kaynak olarak**: **Veri Gezgini** > **erişimi**'nde, klasörleri ve alt klasörlerdeki dosyaları listelemek ve kopyalamak için en az **okuma ve yürütme** iznini verin. Ya da tek bir dosyayı kopyalamak için **okuma** izni verebilirsiniz. Özyinelemeli için **Bu klasöre ve tüm alt öğelere** ekleme yapabilir ve **erişim izni olarak ve varsayılan izin girişi**olarak ekleyebilirsiniz. Hesap düzeyinde erişim denetimi (ıAM) için gereksinim yoktur.
>- **Havuz olarak**: **Veri Gezgini** > **erişimi**'nde, klasörde alt öğeler oluşturmak için en az **yazma + yürütme** iznini verin. Özyinelemeli için **Bu klasöre ve tüm alt öğelere** ekleme yapabilir ve **erişim izni olarak ve varsayılan izin girişi**olarak ekleyebilirsiniz. Kopyalamak için bir Azure tümleştirme çalışma zamanı kullanıyorsanız (hem kaynak hem de havuz bulutta ise), ıAM 'de Data Lake Store bölge Data Factory algılamasını sağlamak için en az **okuyucu** rolü verin. Bu ıAM rolünü önlemek istiyorsanız, açıkça Data Lake Store konumuyla [bir Azure tümleştirme çalışma zamanı oluşturun](create-azure-integration-runtime.md#create-azure-ir) . Bunları, aşağıdaki örnekte gösterildiği gibi Data Lake Store bağlantılı hizmette ilişkilendirin.

>[!NOTE]
>Kök bilgisayardan başlayarak klasörleri listelemek için, **kök düzeyinde "yürütme" izniyle**verilen yönetilen kimliğin iznini ayarlamanız gerekir. Bu, şunu kullandığınızda geçerlidir:
>- **Veri aracını** yazar kopyalama Işlem hattına Kopyala.
>- Yazma sırasında bağlantıyı test etmek ve klasörlerde gezinmek için **Kullanıcı arabirimi Data Factory** .
>Kök düzeyinde izin verme hakkında endişeleriniz varsa, yazma sırasında bağlantıyı test etmeyi atlayın ve izin verilen bir üst yolu girin ve ardından belirtilen yoldan gözatmayı seçin. Kopyalama etkinliği, hizmet sorumlusu kopyalanacak dosyalar üzerinde uygun izinlerle verildiği sürece çalışıyor olur.

Azure Data Factory, bağlantılı hizmette genel Data Lake Store bilgilerinin yanı sıra herhangi bir özellik belirtmeniz gerekmez.

**Örnek:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. 

- **Parquet, sınırlandırılmış metin, avro ve ikili biçimi**Için, [Parquet, sınırlandırılmış metin, avro ve ikili biçim veri kümesi](#format-based-dataset) bölümüne bakın.
- **Orc/JSON biçimi**gibi diğer biçimler için [diğer biçim veri kümesi](#other-format-dataset) bölümüne bakın.

### <a name="format-based-dataset"></a>Parquet, sınırlandırılmış metin, avro ve ikili biçim veri kümesi

**Parquet, sınırlandırılmış metin, Avro veya binary biçiminden**veri kopyalamak için, biçim tabanlı veri kümesinde ve desteklenen ayarlarda [Parquet biçimine](format-parquet.md), [sınırlandırılmış metin biçimine](format-delimited-text.md), [avro biçimine](format-avro.md) ve [ikili biçim](format-binary.md) makalesine bakın.
Biçim tabanlı veri kümesindeki ayarlar altında `location` Azure Data Lake Store Gen1 için aşağıdaki özellikler desteklenir:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Veri kümesinde `location` bulunan tür özelliğinin **AzureDataLakeStoreLocation**olarak ayarlanması gerekir. | Evet      |
| folderPath | Bir klasörün yolu. Klasörleri filtrelemek için bir joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları 'nda belirtin. | Hayır       |
| fileName   | Verilen folderPath altındaki dosya adı. Dosyaları filtrelemek için bir joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları 'nda belirtin. | Hayır       |

> [!NOTE]
>
> Aşağıdaki bölümde belirtilen Parquet veya metin biçimindeki **AzureDataLakeStoreFile** Type veri kümesi, geriye dönük uyumluluk için kopyalama, arama ve GetMetadata etkinliğinin olduğu gibi hala desteklenmektedir. Ancak veri akışı eşleme özelliğiyle çalışmaz. Bu yeni modeli ileride kullanmanızı öneririz. Data Factory yazma Kullanıcı arabirimi bu yeni türleri oluşturur.

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<ADLS Gen1 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureDataLakeStoreLocation",
                "folderPath": "root/folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="other-format-dataset"></a>Diğer biçim veri kümesi

**Orc/JSON biçiminde**Azure Data Lake Store Gen1 'e veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin Type özelliği **AzureDataLakeStoreFile**olarak ayarlanmalıdır. |Evet |
| folderPath | Data Lake Store klasörünün yolu. Belirtilmezse, kök dizinine işaret eder. <br/><br/>Joker karakter filtresi desteklenir. İzin verilen joker `*` karakterler (sıfır veya daha fazla karakterle eşleşir `?` ) ve (sıfır veya tek karakterle eşleşir). Gerçek `^` klasör adınızın bir joker karakter veya içinde bu kaçış karakteri varsa kaçış için kullanın. <br/><br/>Örneğin: RootFolder/alt klasör/. [Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. |Hayır |
| fileName | Belirtilen "folderPath" altındaki dosyalar için ad veya joker karakter filtresi. Bu özellik için bir değer belirtmezseniz, klasördeki tüm dosyaları için veri kümesini işaret eder. <br/><br/>Filtre için, izin verilen `*` joker karakterler (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir).<br/>-Örnek 1: `"fileName": "*.csv"`<br/>-Örnek 2: `"fileName": "???20180427.txt"`<br/>Gerçek `^` dosya adınızın bir joker karakter veya içinde bu kaçış karakteri varsa kaçış için kullanın.<br/><br/>Bir çıkış veri kümesi için dosya adı belirtilmediğinde ve etkinlik havuzunda **Preservehierarchy** belirtilmemişse, kopyalama etkinliği dosya adını aşağıdaki Düzenle otomatik olarak oluşturur: "*Veri. [etkinlik çalıştırma KIMLIĞI GUID]. [DÜZEDEN hiyerarşi varsa GUID]. [yapılandırıldıysa Biçimlendir]. [yapılandırıldıysa sıkıştırma]* "Örneğin," Data. 0a405f8a-93ff-4c6f-B3BE-f69616f1df7a. txt. gz ". Sorgu yerine tablo adı kullanarak tablosal kaynaktan kopyalama yaparsanız, ad deseninin " *[tablo adı]" olması gerekir. [ Biçim]. [yapılandırıldıysa sıkıştırma]* "Örneğin," MyTable. csv ". |Hayır |
| modifiedDatetimeStart | Son değiştirilen özniteliğe göre dosya filtresi. Son değiştirilme zamanı, ve `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığı içinde ise dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Veri hareketinin genel performansı, çok büyük miktarlarda dosya filtresi yapmak istediğinizde bu ayarın etkinleştirilmesiyle etkilenir. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmadığı anlamına gelir. Ne `modifiedDatetimeStart` zaman bir tarih saat değeri `modifiedDatetimeEnd` olduğunda, ancak null ise, son değiştirilen özniteliği DateTime değerinden büyük veya ona eşit olan dosyalar seçilir. Ne `modifiedDatetimeEnd` zaman bir tarih saat değeri `modifiedDatetimeStart` olduğunda, ancak null ise, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir.| Hayır |
| modifiedDatetimeEnd | Son değiştirilen özniteliğe göre dosya filtresi. Son değiştirilme zamanı, ve `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığı içinde ise dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Veri hareketinin genel performansı, çok büyük miktarlarda dosya filtresi yapmak istediğinizde bu ayarın etkinleştirilmesiyle etkilenir. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmadığı anlamına gelir. Ne `modifiedDatetimeStart` zaman bir tarih saat değeri `modifiedDatetimeEnd` olduğunda, ancak null ise, son değiştirilen özniteliği DateTime değerinden büyük veya ona eşit olan dosyalar seçilir. Ne `modifiedDatetimeEnd` zaman bir tarih saat değeri `modifiedDatetimeStart` olduğunda, ancak null ise, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir.| Hayır |
| format | Dosyaları dosya tabanlı depolarla (ikili kopya) olduğu gibi kopyalamak istiyorsanız hem giriş hem de çıkış veri kümesi tanımlarının biçim bölümünü atlayın.<br/><br/>Belirli bir biçimdeki dosyaları ayrıştırmak veya oluşturmak isterseniz, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **jsonformat**, **avroformat**, **orcformat**ve **parquetformat**. Ayarlama **türü** özelliği altında **biçimi** şu değerlerden biri olarak. Daha fazla bilgi için [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [JSON biçimine](supported-file-formats-and-compression-codecs.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquet biçimi ](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopya senaryosu için) |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyleri **Optimal** ve **en hızlı**. |Hayır |


>[!TIP]
>Bir klasör altındaki tüm dosyaları kopyalamak için belirtin **folderPath** yalnızca.<br>Belirli bir ada sahip tek bir dosyayı kopyalamak için, klasör **bölümü ve dosya** adı ile bir dosya adı ile **FolderPath** belirtin.<br>Bir klasör altındaki bir dosya alt kümesini kopyalamak için, bir klasör bölümü ve **dosya adı** ile bir joker karakter filtresi içeren **FolderPath** öğesini belirtin. 

**Örnek:**

```json
{
    "name": "ADLSDataset",
    "properties": {
        "type": "AzureDataLakeStoreFile",
        "linkedServiceName":{
            "referenceName": "<ADLS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "datalake/myfolder/",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md). Bu bölüm Azure Data Lake Store kaynak ve havuz tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-data-lake-store-as-source"></a>Kaynak olarak Azure Data Lake Store

- **Parquet, sınırlandırılmış metin, avro ve binary biçiminden**kopyalamak Için, [Parquet, sınırlandırılmış metin, avro ve ikili biçim kaynağı](#format-based-source) bölümüne bakın.
- **Orc/JSON biçimi**gibi diğer biçimlerden kopyalamak için [diğer biçim kaynağı](#other-format-source) bölümüne bakın.

#### <a name="format-based-source"></a>Parquet, sınırlandırılmış metin, avro ve ikili biçim kaynağı

**Parquet, sınırlandırılmış metin, Avro veya binary biçimindeki**verileri kopyalamak için, biçim tabanlı kopyalama etkinliği kaynağı ve desteklenen ayarlar üzerinde [Parquet biçimi](format-parquet.md), [sınırlandırılmış metin biçimi](format-delimited-text.md), [avro Format](format-avro.md) ve [ikili biçim](format-binary.md) makalesine bakın .  Biçim tabanlı kopyalama kaynağındaki ayarlar altında `storeSettings` Azure Data Lake Store Gen1 için aşağıdaki özellikler desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | İçindeki tür özelliği `storeSettings` **AzureDataLakeStoreReadSetting**olarak ayarlanmalıdır. | Evet                                           |
| recursive                | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli değeri true olarak ayarlandığında ve havuz dosya tabanlı bir deposa, havuzda boş bir klasör veya alt klasör kopyalanmaz veya oluşturulmaz. İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır                                            |
| Yavaya Cardfolderpath       | Kaynak klasörlerin filtreleneceği joker karakter içeren klasör yolu. <br>İzin verilen joker `*` karakterler (sıfır veya daha fazla karakterle eşleşir `?` ) ve (sıfır veya tek karakterle eşleşir). Gerçek `^` klasör adınızın bir joker karakter veya içinde bu kaçış karakteri varsa kaçış için kullanın. <br>[Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | Hayır                                            |
| Yavaya Cardfilename         | Kaynak dosyalarını filtrelemek için, belirtilen folderPath/, Cardfolderpath altındaki joker karakterlerle dosya adı. <br>İzin verilen joker `*` karakterler (sıfır veya daha fazla karakterle eşleşir `?` ) ve (sıfır veya tek karakterle eşleşir). Gerçek `^` klasör adınızın bir joker karakter veya içinde bu kaçış karakteri varsa kaçış için kullanın. [Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | `fileName` Veri kümesinde belirtilmemişse Evet |
| modifiedDatetimeStart    | Son değiştirilen özniteliğe göre dosya filtresi. Son değiştirilme zamanı, ve `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığı içinde ise dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmadığı anlamına gelir. Ne `modifiedDatetimeStart` zaman bir tarih saat değeri `modifiedDatetimeEnd` olduğunda, ancak null ise, son değiştirilen özniteliği DateTime değerinden büyük veya ona eşit olan dosyalar seçilir. Ne `modifiedDatetimeEnd` zaman bir tarih saat değeri `modifiedDatetimeStart` olduğunda, ancak null ise, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir. | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdaki gibi.                                               | Hayır                                            |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır                                            |

> [!NOTE]
> Parquet veya sınırlandırılmış metin biçimi için, aşağıdaki bölümde bahsedilen **AzureDataLakeStoreSource** tür kopyalama etkinliği kaynağı, geriye dönük uyumluluk için olduğu gibi hala desteklenmektedir. Bu yeni modeli ileride kullanmanızı öneririz. Data Factory yazma Kullanıcı arabirimi bu yeni türleri oluşturur.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen1",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AzureDataLakeStoreReadSetting",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="other-format-source"></a>Diğer biçim kaynağı

**Orc veya JSON biçiminde**Azure Data Lake Store Gen1 'tan veri kopyalamak için, etkinlik **kaynağını** kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının özelliğiAzureDataLakeStoreSourceolarak`type` ayarlanmalıdır. |Evet |
| recursive | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. `recursive` Doğru olarak ayarlandığında ve havuz dosya tabanlı bir deposa, havuzda boş bir klasör veya alt klasör kopyalanmaz veya oluşturulmaz. İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır |
| maxConcurrentConnections | Aynı anda veri deposuna bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen1",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS Gen1 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "AzureDataLakeStoreSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-store-as-sink"></a>Havuz olarak Azure Data Lake Store

- **Parquet, sınırlandırılmış metin, Avro veya binary biçimine**kopyalamak Için, [Parquet, sınırlandırılmış metin, avro ve ikili biçim havuzu](#format-based-sink) bölümüne bakın.
- **Orc/JSON biçimi**gibi diğer biçimlere kopyalamak için [diğer biçim havuzu](#other-format-sink) bölümüne bakın.

#### <a name="format-based-sink"></a>Parquet, sınırlandırılmış metin, avro ve ikili biçim havuzu

Verileri **Parquet, ayrılmış metin, Avro veya ikili biçime**kopyalamak için, biçim tabanlı kopyalama etkinlik havuzunda ve desteklenen ayarlarda [Parquet biçimine](format-parquet.md), [sınırlandırılmış metin biçimine](format-delimited-text.md), [avro biçimine](format-avro.md) ve [ikili biçime](format-binary.md) bakın.  Biçim tabanlı kopya havuzunda ayarlar altında `storeSettings` Azure Data Lake Store Gen1 için aşağıdaki özellikler desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | İçindeki tür özelliği `storeSettings` **AzureDataLakeStoreWriteSetting**olarak ayarlanmalıdır. | Evet      |
| copyBehavior             | Kaynak dosyaları bir dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-Preservehierarchy (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu, hedef dosyanın göreli yoluyla hedef klasöre aynıdır.<br/><b>-DÜZEDEN hiyerarşi</b>: Kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyalar otomatik olarak oluşturulan adlarına sahip. <br/><b>-Mergefiles</b>: Kaynak klasördeki tüm dosyaları tek bir dosya ile birleştirir. Dosya adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, bir otomatik olarak oluşturulan dosya adı değil. | Hayır       |
| maxConcurrentConnections | Aynı anda veri deposuna bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır       |

> [!NOTE]
> Parquet veya sınırlandırılmış metin biçimi için, aşağıdaki bölümde bahsedilen **AzureDataLakeStoreSink** tür kopyalama etkinliği havuzu, geriye dönük uyumluluk için olduğu gibi hala desteklenmektedir. Bu yeni modeli ileride kullanmanızı öneririz. Data Factory yazma Kullanıcı arabirimi bu yeni türleri oluşturur.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToADLSGen1",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Parquet output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "ParquetSink",
                "storeSettings":{
                    "type": "AzureDataLakeStoreWriteSetting",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

#### <a name="other-format-sink"></a>Diğer biçim havuzu

**Orc veya JSON biçiminde**Azure Data Lake Store Gen1 'e veri kopyalamak için, **Havuz** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuzunun özelliğiAzureDataLakeStoreSinkolarak`type` ayarlanmalıdır. |Evet |
| copyBehavior | Kaynak dosyaları bir dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-Preservehierarchy (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu, hedef dosyanın göreli yoluyla hedef klasöre aynıdır.<br/><b>-DÜZEDEN hiyerarşi</b>: Kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyalar otomatik olarak oluşturulan adlarına sahip. <br/><b>-Mergefiles</b>: Kaynak klasördeki tüm dosyaları tek bir dosya ile birleştirir. Dosya adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, dosya adı otomatik olarak oluşturulur. | Hayır |
| maxConcurrentConnections | Aynı anda veri deposuna bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToADLSGen1",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS Gen1 output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataLakeStoreSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Klasör ve dosya filtresi örnekleri

Bu bölümde, klasör yolu ve dosya adının joker karakter filtreleriyle elde edilen davranışı açıklanmaktadır.

| folderPath | fileName | recursive | Kaynak klasör yapısı ve filtre sonucu ( **kalın** olan dosyalar alınır)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (Boş, Varsayılanı kullan) | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**FILE1. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya2. JSON**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3. csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6. csv |
| `Folder*` | (Boş, Varsayılanı kullan) | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**FILE1. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Dosya2. JSON**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4. JSON**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5. csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6. csv |
| `Folder*` | `*.csv` | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**FILE1. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3. csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6. csv |
| `Folder*` | `*.csv` | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**FILE1. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3. csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. JSON<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5. csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6. csv |

### <a name="examples-of-behavior-of-the-copy-operation"></a>Kopyalama işleminin davranış örnekleri

Bu bölümde, `recursive` ve `copyBehavior` değerlerinin farklı birleşimleri için kopyalama işleminin ortaya çıkan davranışı açıklanmaktadır.

| recursive | copyBehavior | Kaynak klasör yapısı | Sonuçta elde edilen hedef |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1, kaynak ile aynı yapıyla oluşturulur:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| true |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| true |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;FILE1 + dosya2 + File3 + File4 + File5 içerikler, otomatik olarak oluşturulan bir dosya adı ile tek bir dosyada birleştirilir. |
| false |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Subfolder1, File3, File4 ve File5 ile birlikte çekilmemektedir. |
| false |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur:<br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/><br/>Subfolder1, File3, File4 ve File5 ile birlikte çekilmemektedir. |
| false |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;FILE1 + dosya2 içerikleri, otomatik olarak oluşturulan dosya adıyla tek bir dosyada birleştirilir. fıle1'de otomatik olarak oluşturulan adı<br/><br/>Subfolder1, File3, File4 ve File5 ile birlikte çekilmemektedir. |

## <a name="preserve-acls-to-data-lake-storage-gen2"></a>ACL 'Leri Data Lake Storage 2. koruma

Data Lake Storage 1. 'den Data Lake Storage 2. sürümüne yükselttiğinizde erişim denetim listelerini (ACL 'Ler) ve veri dosyalarıyla çoğaltmak istiyorsanız bkz. [Data Lake Storage 1. ACL 'Leri koruma](connector-azure-data-lake-storage.md#preserve-acls-from-data-lake-storage-gen1).

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Veri akışı eşleme özelliğinde [kaynak dönüştürme](data-flow-source.md) ve [Havuz dönüştürme](data-flow-sink.md) hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
