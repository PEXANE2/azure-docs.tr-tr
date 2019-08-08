---
title: Data Factory kullanarak Azure Data Lake Storage 2. veri kopyalama | Microsoft Docs
description: Azure Data Factory kullanarak Azure Data Lake Storage 2. veri kopyalama hakkında bilgi edinin.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: jingwang
ms.openlocfilehash: a0a7a413d6c3344ccf5c3f7e4d14dd3d82715034
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68840314"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen2-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Data Lake Storage 2. veri kopyalama

Azure Data Lake Storage 2., [Azure Blob depolamada](../storage/blobs/storage-blobs-introduction.md)yerleşik olan büyük veri analizlerine adanmış bir yetenekler kümesidir. Hem dosya sistemi hem de nesne depolama paradigmalarına kullanarak verilerinize arabirim atamak için kullanabilirsiniz.

Bu makalede, Azure Data Lake Storage 2. verileri içine ve öğesinden kopyalama özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [giriş makalesi](introduction.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Azure Data Lake Storage 2. Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak veya havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)

Özellikle, bu bağlayıcıyla şunları yapabilirsiniz:

- Azure kaynakları kimlik doğrulamaları için hesap anahtarı, hizmet sorumlusu veya yönetilen kimlikler kullanarak verileri kopyalayın.
- Dosyaları olduğu gibi kopyalayın veya [Desteklenen dosya biçimlerine ve sıkıştırma codec bileşenleriyle](supported-file-formats-and-compression-codecs.md)dosya oluşturun veya oluşturun.

>[!TIP]
>Hiyerarşik ad alanını etkinleştirirseniz, şu anda blob ve Data Lake Storage 2. API 'Ler arasındaki işlemlerden birlikte çalışabilirlik özelliği yoktur. "ErrorCode = FilesystemNotFound" hatasını "belirtilen dosya sistemi yok" iletisiyle karşılaşırsanız, bunun nedeni API 'nin başka bir yerinde Data Lake Storage 2., blob API 'SI aracılığıyla oluşturulmuş belirtilen havuz dosya sistemidir. Sorunu onarmak için, bir blob kapsayıcısının adı olarak mevcut olmayan yeni bir dosya sistemi belirtin. Data Factory, veri kopyalama sırasında otomatik olarak bu dosya sistemini oluşturur.

>[!NOTE]
>Azure Storage güvenlik duvarı ayarları 'ndaki **Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine Izin ver** seçeneğini etkinleştirirseniz Azure tümleştirme çalışma zamanı Data Lake Storage 2. bağlanamaz ve yasaklanmış bir hatayı gösterir. Data Factory güvenilir bir Microsoft hizmeti olarak değerlendirilmediği için hata iletisi görüntülenir. Bunun yerine bağlanmak için şirket içinde barındırılan tümleştirme çalışma zamanını kullanın.

## <a name="get-started"></a>başlarken

>[!TIP]
>Data Lake Storage 2. bağlayıcısını nasıl kullanacağınızı öğrenmek için bkz. [Azure Data Lake Storage 2. verileri yükleme](load-azure-data-lake-storage-gen2.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler Data Lake Storage 2. özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında bilgiler sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Data Lake Storage 2. Bağlayıcısı aşağıdaki kimlik doğrulama türlerini destekler. Ayrıntılar için ilgili bölümlere bakın:

- [Hesap anahtarı kimlik doğrulaması](#account-key-authentication)
- [Hizmet sorumlusu kimlik doğrulaması](#service-principal-authentication)
- [Azure kaynaklarında kimlik doğrulaması için yönetilen kimlik](#managed-identity)

>[!NOTE]
>SQL veri ambarı 'na veri yüklemek için PolyBase kullanılırken, kaynak Data Lake Storage 2. sanal ağ uç noktası ile yapılandırıldıysa, PolyBase için gereken yönetilen kimlik kimlik doğrulamasını kullanmanız gerekir. Daha fazla yapılandırma önkoşulları ile [yönetilen kimlik doğrulama](#managed-identity) bölümüne bakın.

### <a name="account-key-authentication"></a>Hesap anahtarı kimlik doğrulaması

Depolama hesabı anahtarı kimlik doğrulaması kullanmak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AzureBlobFS**. |Evet |
| url | Düzenine sahip Data Lake Storage 2. için uç nokta `https://<accountname>.dfs.core.windows.net`. | Evet |
| accountKey | Data Lake Storage 2. için hesap anahtarı. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Veri depolubir özel ağda ise Azure tümleştirme çalışma zamanını veya şirket içinde barındırılan tümleştirme çalışma zamanını kullanabilirsiniz. Bu özellik belirtilmezse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

**Örnek:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
            "accountkey": { 
                "type": "SecureString", 
                "value": "<accountkey>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Hizmet sorumlusu kimlik doğrulamasını kullanmak için aşağıdaki adımları izleyin.

1. Uygulamanızı [bir Azure AD kiracısıyla kaydetme](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant)bölümündeki adımları izleyerek Azure Active Directory (Azure AD) uygulamasına bir uygulama varlığı kaydedin. Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Hizmet sorumlusu uygun iznini verin. [Dosya ve dizinlerdeki erişim denetim listelerinden](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories) izin Data Lake Storage 2. çalışma hakkında daha fazla bilgi edinin

    - **Kaynak olarak**: Depolama Gezgini, kaynak dosya sisteminden başlayarak, kopyalanacak dosyalar için **okuma** izniyle birlikte en az **yürütme** iznini verin. Alternatif olarak, erişim denetimi 'nde (ıAM) en azından **Depolama Blobu veri okuyucu** rolü verin.
    - **Havuz olarak**: Depolama Gezgini, havuz dosya sisteminden başlayarak havuz klasörü için **yazma** izniyle birlikte en az **yürütme** izni verin. Alternatif olarak, erişim denetimi 'nde (ıAM), en azından **Depolama Blobu veri katılımcısı** rolüne izin verin.

>[!NOTE]
>Hesap düzeyinden başlayan veya bağlantıyı test eden klasörleri listelemek için, **IAM 'de "Depolama Blobu veri okuyucu" izniyle depolama hesabına**verilen hizmet sorumlusu iznini ayarlamanız gerekir. Bu, şunu kullandığınızda geçerlidir:
>- **Veri aracını** yazar kopyalama Işlem hattına Kopyala.
>- Yazma sırasında bağlantıyı test etmek ve klasörlerde gezinmek için **Kullanıcı arabirimi Data Factory** . 
>Hesap düzeyinde izin verme hakkında endişeleriniz varsa, yazma sırasında bağlantıyı test etmeyi atlayın ve izin verilen bir üst yolu girin ve ardından belirtilen yoldan gözatmayı seçin. Kopyalama etkinliği, hizmet sorumlusu kopyalanacak dosyalar üzerinde uygun izinlerle verildiği sürece çalışıyor olur.

Bu özellikler bağlı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AzureBlobFS**. |Evet |
| url | Düzenine sahip Data Lake Storage 2. için uç nokta `https://<accountname>.dfs.core.windows.net`. | Evet |
| servicePrincipalId | Uygulamanın istemci kimliği belirtin. | Evet |
| servicePrincipalKey | Uygulama anahtarını belirtin. Data Factory güvenli bir şekilde depolamak `SecureString` için bu alanı bir olarak işaretleyin. Veya [Azure Key Vault depolanan bir gizli](store-credentials-in-key-vault.md)dizi ile başvurabilirsiniz. | Evet |
| tenant | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Fareyi, Azure portal sağ üst köşesine getirerek alın. | Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Veri depolubir özel ağda ise Azure tümleştirme çalışma zamanını veya şirket içinde barındırılan tümleştirme çalışma zamanını kullanabilirsiniz. Belirtilmemişse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

**Örnek:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>" 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Azure kaynaklarında kimlik doğrulaması için yönetilen kimlik

Veri Fabrikası ile ilişkilendirilebilen bir [yönetilen Azure kaynakları için kimliği](data-factory-service-identity.md), bu belirli veri fabrikası temsil eder. Bu yönetilen kimliği, kendi hizmet sorumlunuzu kullanmaya benzer şekilde Data Lake Storage 2. kimlik doğrulaması için doğrudan kullanabilirsiniz. Bu belirlenen fabrikasının Data Lake Storage 2. erişmesine ve bu verilere veri kopyalamasına izin verir.

Azure Kaynak kimlik doğrulaması için yönetilen kimlikleri kullanmak için aşağıdaki adımları izleyin.

1. Fabrikalarınızla birlikte oluşturulan **hizmet kimliği uygulama kimliği** değerini kopyalayarak [Data Factory yönetilen kimlik bilgilerini alın](data-factory-service-identity.md#retrieve-managed-identity) .

2. Yönetilen kimliğe uygun izni verin. İzinlerin [dosya ve dizinlerin erişim denetim listelerinden](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories)Data Lake Storage 2. nasıl çalıştığı hakkında daha fazla bilgi edinin.

    - **Kaynak olarak**: Depolama Gezgini, kaynak dosya sisteminden başlayarak, kopyalanacak dosyalar için **okuma** izniyle birlikte en az **yürütme** iznini verin. Alternatif olarak, erişim denetimi 'nde (ıAM) en azından **Depolama Blobu veri okuyucu** rolü verin.
    - **Havuz olarak**: Depolama Gezgini, havuz dosya sisteminden başlayarak havuz klasörü için **yazma** izniyle birlikte en az **yürütme** izni verin. Alternatif olarak, erişim denetimi 'nde (ıAM), en azından **Depolama Blobu veri katılımcısı** rolüne izin verin.

>[!NOTE]
>Hesap düzeyinden başlayan veya bağlantıyı test eden klasörleri listelemek için, **IAM 'de "Depolama Blobu veri okuyucu" izniyle depolama hesabına**verilen yönetilen kimliğin iznini ayarlamanız gerekir. Bu, şunu kullandığınızda geçerlidir:
>- **Veri aracını** yazar kopyalama Işlem hattına Kopyala.
>- Yazma sırasında bağlantıyı test etmek ve klasörlerde gezinmek için **Kullanıcı arabirimi Data Factory** . 
>Hesap düzeyinde izin verme hakkında endişeleriniz varsa, yazma sırasında bağlantıyı test etmeyi atlayın ve izin verilen bir üst yolu girin ve ardından belirtilen yoldan gözatmayı seçin. Kopyalama etkinliği, hizmet sorumlusu kopyalanacak dosyalar üzerinde uygun izinlerle verildiği sürece çalışıyor olur.

>[!IMPORTANT]
>Data Lake Storage 2. verileri SQL veri ambarı 'na yüklemek için PolyBase kullanırsanız, Data Lake Storage 2. için yönetilen kimlik kimlik doğrulamasını kullanırken, 1. ve 2. adımları izleyin. [](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage) Azure Active Directory (Azure AD) ve 2) SQL veritabanı sunucunuza Depolama Blobu veri katılımcısı rolünü atayın; Rest Data Factory tarafından işlenir. Data Lake Storage 2. Azure sanal ağ uç noktası ile yapılandırıldıysa, verileri dosyadan yüklemek için PolyBase kullanmak istiyorsanız, PolyBase 'in gerektirdiği şekilde yönetilen kimlik kimlik doğrulamasını kullanmanız gerekir.

Bu özellikler bağlı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır **AzureBlobFS**. |Evet |
| url | Düzenine sahip Data Lake Storage 2. için uç nokta `https://<accountname>.dfs.core.windows.net`. | Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Veri depolubir özel ağda ise Azure tümleştirme çalışma zamanını veya şirket içinde barındırılan tümleştirme çalışma zamanını kullanabilirsiniz. Belirtilmemişse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

**Örnek:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md).

- **Parquet, sınırlandırılmış metin ve ikili biçimi**Için, [Parquet, sınırlandırılmış metin ve ikili biçim veri kümesi](#format-based-dataset) bölümüne bakın.
- **Orc/avro/JSON biçimi**gibi diğer biçimler için [diğer biçim veri kümesi](#other-format-dataset) bölümüne bakın.

### <a name="format-based-dataset"></a>Parquet, sınırlandırılmış metin ve ikili biçim veri kümesi

**Parquet, sınırlandırılmış metin veya ikili biçimine**veri kopyalamak için, biçim tabanlı veri kümesinde ve desteklenen ayarlarda [Parquet biçimine](format-parquet.md), [sınırlandırılmış metin biçimine](format-delimited-text.md) ve [ikili biçim](format-binary.md) makalesine başvurun. Biçim tabanlı veri kümesindeki ayarlar altında `location` Data Lake Storage 2. için aşağıdaki özellikler desteklenir:

| Özellik   | Açıklama                                                  | Gerekli |
| ---------- | ------------------------------------------------------------ | -------- |
| türü       | Veri kümesinde `location` bulunan tür özelliğinin **AzureBlobFSLocation**olarak ayarlanması gerekir. | Evet      |
| fileSystem | Data Lake Storage 2. dosya sistemi adı.                              | Hayır       |
| folderPath | Verilen dosya sistemi altındaki bir klasörün yolu. Klasörleri filtrelemek için bir joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları 'nda belirtin. | Hayır       |
| fileName   | Verilen fileSystem + folderPath altındaki dosya adı. Dosyaları filtrelemek için bir joker karakter kullanmak istiyorsanız, bu ayarı atlayın ve etkinlik kaynağı ayarları 'nda belirtin. | Hayır       |

> [!NOTE]
> Aşağıdaki bölümde belirtilen Parquet veya metin biçimine sahip **AzureBlobFSFile** Type veri kümesi, geriye dönük uyumluluk için kopyalama, arama veya GetMetadata etkinliğinin olduğu gibi hala desteklenmektedir. Ancak veri akışı eşleme özelliğiyle çalışmaz. Bu yeni modeli ileride kullanmanızı öneririz. Data Factory yazma Kullanıcı arabirimi bu yeni türleri oluşturur.

**Örnek:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Data Lake Storage Gen2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobFSLocation",
                "fileSystem": "filesystemname",
                "folderPath": "folder/subfolder"
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

**Orc/avro/JSON biçimindeki**Data Lake Storage 2. veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır **AzureBlobFSFile**. |Evet |
| folderPath | Data Lake Storage 2. klasörünün yolu. Belirtilmezse, kök dizinine işaret eder. <br/><br/>Joker karakter filtresi desteklenir. İzin verilen joker `*` karakterler (sıfır veya daha fazla karakterle eşleşir `?` ) ve (sıfır veya tek karakterle eşleşir). Gerçek `^` klasör adınızın bir joker karakter varsa veya bu kaçış karakteri içindeyse kaçış için kullanın. <br/><br/>Örnekler: FileSystem/Folder/. [Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. |Hayır |
| fileName | Belirtilen "folderPath" altındaki dosyalar için ad veya joker karakter filtresi. Bu özellik için bir değer belirtmezseniz, klasördeki tüm dosyaları için veri kümesini işaret eder. <br/><br/>Filtre için, izin verilen `*` joker karakterler (sıfır veya daha fazla karakterle eşleşir) ve `?` (sıfır veya tek karakterle eşleşir).<br/>-Örnek 1: `"fileName": "*.csv"`<br/>-Örnek 2: `"fileName": "???20180427.txt"`<br/>Gerçek `^` dosya adınızın bir joker karakter varsa veya bu kaçış karakteri içindeyse kaçış için kullanın.<br/><br/>Bir çıkış veri kümesi için dosya adı belirtilmediğinde ve etkinlik havuzunda **Preservehierarchy** belirtilmemişse, kopyalama etkinliği dosya adını aşağıdaki Düzenle otomatik olarak oluşturur: "*Veri. [etkinlik çalıştırma KIMLIĞI GUID]. [DÜZEDEN hiyerarşi varsa GUID]. [yapılandırıldıysa Biçimlendir]. [yapılandırıldıysa sıkıştırma]* "Örneğin," Data. 0a405f8a-93ff-4c6f-B3BE-f69616f1df7a. txt. gz ". Sorgu yerine tablo adı kullanarak tablosal kaynaktan kopyalama yaparsanız, ad deseninin " *[tablo adı]" olması gerekir. [ Biçim]. [yapılandırıldıysa sıkıştırma]* "Örneğin," MyTable. csv ". |Hayır |
| modifiedDatetimeStart | Son değiştirilen özniteliğe göre dosya filtresi. Son değiştirilme zamanı, ve `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığı içinde ise dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Veri hareketinin genel performansı, çok büyük miktarlarda dosya filtresi yapmak istediğinizde bu ayarın etkinleştirilmesiyle etkilenir. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmadığı anlamına gelir. Ne `modifiedDatetimeStart` zaman bir tarih saat değeri `modifiedDatetimeEnd` olduğunda, ancak null ise, son değiştirilen özniteliği DateTime değerinden büyük veya ona eşit olan dosyalar seçilir. Ne `modifiedDatetimeEnd` zaman bir tarih saat değeri `modifiedDatetimeStart` olduğunda, ancak null ise, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir.| Hayır |
| modifiedDatetimeEnd | Son değiştirilen özniteliğe göre dosya filtresi. Son değiştirilme zamanı, ve `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığı içinde ise dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br/><br/> Veri hareketinin genel performansı, çok büyük miktarlarda dosya filtresi yapmak istediğinizde bu ayarın etkinleştirilmesiyle etkilenir. <br/><br/> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmadığı anlamına gelir. Ne `modifiedDatetimeStart` zaman bir tarih saat değeri `modifiedDatetimeEnd` olduğunda, ancak null ise, son değiştirilen özniteliği DateTime değerinden büyük veya ona eşit olan dosyalar seçilir. Ne `modifiedDatetimeEnd` zaman bir tarih saat değeri `modifiedDatetimeStart` olduğunda, ancak null ise, son değiştirilen özniteliği DateTime değerinden küçük olan dosyalar seçilir.| Hayır |
| format | (İkili kopya) depoları arasında dosya tabanlı olduğu gibi dosyaları kopyalamak girdi ve çıktı veri kümesi tanımları biçimi bölümüne atlayın.<br/><br/>Belirli bir biçimdeki dosyaları ayrıştırmak veya oluşturmak isterseniz, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **jsonformat**, **avroformat**, **orcformat**ve **parquetformat**. Ayarlama **türü** özelliği altında **biçimi** şu değerlerden biri olarak. Daha fazla bilgi için bkz. [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [JSON biçimi](supported-file-formats-and-compression-codecs.md#json-format), [avro Format](supported-file-formats-and-compression-codecs.md#avro-format), [orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format)ve [Parquet biçim](supported-file-formats-and-compression-codecs.md#parquet-format) bölümleri. |Hayır (yalnızca ikili kopya senaryosu için) |
| compression | Veri sıkıştırma düzeyi ve türünü belirtin. Daha fazla bilgi için [desteklenen dosya biçimleri ve codec sıkıştırma](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyleri **Optimal** ve **en hızlı**. |Hayır |

>[!TIP]
>Bir klasör altındaki tüm dosyaları kopyalamak için belirtin **folderPath** yalnızca.<br>Belirli bir ada sahip tek bir dosyayı kopyalamak için, klasör bölüm ve dosya adı ile bir dosya adı ile **FolderPath** öğesini belirtin.<br>Bir klasör altındaki bir dosya alt kümesini kopyalamak için, bir klasör bölümü ve **dosya adı** ile bir joker karakter filtresi içeren **FolderPath** öğesini belirtin. 

**Örnek:**

```json
{
    "name": "ADLSGen2Dataset",
    "properties": {
        "type": "AzureBlobFSFile",
        "linkedServiceName": {
            "referenceName": "<Azure Data Lake Storage Gen2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "myfilesystem/myfolder",
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

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [kopyalama etkinliği yapılandırması](copy-activity-overview.md#configuration) ve işlem [hatları ve etkinlikleri](concepts-pipelines-activities.md). Bu bölümde, Data Lake depolama 2. nesil kaynak ve havuz desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-data-lake-storage-gen2-as-a-source-type"></a>Kaynak türü olarak Azure Data Lake Storage 2.

- **Parquet, sınırlandırılmış metin ve ikili biçiminden**kopyalamak Için, [Parquet, sınırlandırılmış metin ve ikili biçim kaynağı](#format-based-source) bölümüne bakın.
- **Orc/avro/JSON biçimi**gibi diğer biçimlerden kopyalamak için [diğer biçim kaynağı](#other-format-source) bölümüne bakın.

#### <a name="format-based-source"></a>Parquet, sınırlandırılmış metin ve ikili biçim kaynağı

**Parquet, sınırlandırılmış metin veya ikili biçiminden**verileri kopyalamak için, biçim tabanlı kopyalama etkinliği kaynağı ve desteklenen ayarlar ' da [Parquet biçimine](format-parquet.md), [sınırlandırılmış metin biçimine](format-delimited-text.md) ve [ikili biçim](format-binary.md) makalesine başvurun. Aşağıdaki özellikler, biçim tabanlı kopyalama kaynağı ayarları `storeSettings` altındaki Data Lake Storage 2. için desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| türü                     | İçindeki tür özelliği `storeSettings` **AzureBlobFSReadSetting**olarak ayarlanmalıdır. | Evet                                           |
| recursive                | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli değeri true olarak ayarlandığında ve havuz dosya tabanlı bir deposa, havuzda boş bir klasör veya alt klasör kopyalanmaz veya oluşturulmaz. İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır                                            |
| Yavaya Cardfolderpath       | Kaynak klasörleri filtrelemek için veri kümesinde yapılandırılan belirtilen dosya sistemi altındaki joker karakterlerle klasör yolu. <br>İzin verilen joker `*` karakterler (sıfır veya daha fazla karakterle eşleşir `?` ) ve (sıfır veya tek karakterle eşleşir). Gerçek `^` klasör adınızın bir joker karakter veya içinde bu kaçış karakteri varsa kaçış için kullanın. <br>[Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | Hayır                                            |
| Yavaya Cardfilename         | Kaynak dosyaları filtrelemek için, belirtilen dosya sistemi + folderPath/, Cardfolderpath altındaki joker karakterlerle dosya adı. <br>İzin verilen joker `*` karakterler (sıfır veya daha fazla karakterle eşleşir `?` ) ve (sıfır veya tek karakterle eşleşir). Gerçek `^` klasör adınızın bir joker karakter veya içinde bu kaçış karakteri varsa kaçış için kullanın. [Klasör ve dosya filtresi örneklerinde](#folder-and-file-filter-examples)daha fazla örnek görüntüleyin. | `fileName` Veri kümesinde belirtilmemişse Evet |
| modifiedDatetimeStart    | Son değiştirilen özniteliğe göre dosya filtresi. Son değiştirilme zamanı, ve `modifiedDatetimeStart` `modifiedDatetimeEnd`arasındaki zaman aralığı içinde ise dosyalar seçilir. Saat, UTC saat dilimine "2018-12-01T05:00:00Z" biçiminde uygulanır. <br> Özellikler NULL olabilir, bu da veri kümesine hiçbir dosya özniteliği filtresinin uygulanmadığı anlamına gelir. Ne `modifiedDatetimeStart` zaman bir tarih saat değeri `modifiedDatetimeEnd` olduğunda, ancak null ise, son değiştirilen özniteliği DateTime değerinden büyük veya buna eşit olan dosyaların seçildiği anlamına gelir. Ne `modifiedDatetimeEnd` zaman bir tarih saat değeri `modifiedDatetimeStart` olduğunda, ancak null ise, son değiştirilen özniteliği tarih saat değerinden küçük olan dosyaların seçildiği anlamına gelir. | Hayır                                            |
| modifiedDatetimeEnd      | Yukarıdaki gibi.                                               | Hayır                                            |
| maxConcurrentConnections | Depolama deposuna aynı anda bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır                                            |

> [!NOTE]
> Parquet veya delimited metin biçimi için, aşağıdaki bölümde bahsedilen **AzureBlobFSSource** tür kopyalama etkinliği kaynağı, geriye dönük uyumluluk için olduğu gibi hala desteklenmektedir. Bu yeni modeli ileride kullanmanızı öneririz. Data Factory yazma Kullanıcı arabirimi bu yeni türleri oluşturur.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen2",
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
                    "type": "AzureBlobFSReadSetting",
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

**Orc/avro/JSON biçimindeki**Data Lake Storage 2. verileri kopyalamak için, etkinlik **kaynağını** kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır **AzureBlobFSSource**. |Evet |
| recursive | Belirtilen klasörün alt klasörleri ya da yalnızca veri yinelemeli olarak okunur olup olmadığını belirtir. Özyinelemeli değeri true olarak ayarlandığında ve havuz dosya tabanlı bir deposa, havuzda boş bir klasör veya alt klasör kopyalanmaz veya oluşturulmaz.<br/>İzin verilen değerler **true** (varsayılan) ve **false**. | Hayır |
| maxConcurrentConnections | Aynı anda veri deposuna bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS Gen2 input dataset name>",
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
                "type": "AzureBlobFSSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-storage-gen2-as-a-sink-type"></a>Bir havuz türü olarak Azure Data Lake depolama Gen2

- **Parquet, sınırlandırılmış metin veya ikili biçimi**kopyalamak Için, [Parquet, sınırlandırılmış metin ve ikili biçim havuzu](#format-based-sink) bölümüne bakın.
- **Orc/avro/JSON biçimi**gibi diğer biçimlere kopyalamak için [diğer biçim havuzu](#other-format-sink) bölümüne bakın.

#### <a name="format-based-sink"></a>Parquet, ayrılmış metin ve ikili biçim havuzu

Verileri **Parquet, sınırlandırılmış metin veya ikili biçime**kopyalamak için, biçim tabanlı kopyalama etkinlik havuzunda ve desteklenen ayarlarda [Parquet biçimine](format-parquet.md), [sınırlandırılmış metin biçimine](format-delimited-text.md) ve [ikili biçim](format-binary.md) makalesine başvurun. Biçim tabanlı kopya havuzunda ayarlar altında `storeSettings` Data Lake Storage 2. için aşağıdaki özellikler desteklenir:

| Özellik                 | Açıklama                                                  | Gerekli |
| ------------------------ | ------------------------------------------------------------ | -------- |
| türü                     | İçindeki tür özelliği `storeSettings` **AzureBlobFSWriteSetting**olarak ayarlanmalıdır. | Evet      |
| copyBehavior             | Kaynak dosyaları bir dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-Preservehierarchy (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu, hedef dosyanın göreli yoluyla hedef klasöre aynıdır.<br/><b>-DÜZEDEN hiyerarşi</b>: Kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyalar otomatik olarak oluşturulan adlarına sahip. <br/><b>-Mergefiles</b>: Kaynak klasördeki tüm dosyaları tek bir dosya ile birleştirir. Dosya adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, bir otomatik olarak oluşturulan dosya adı değil. | Hayır       |
| maxConcurrentConnections | Aynı anda veri deposuna bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır       |

> [!NOTE]
> Parquet veya delimited metin biçimi için, aşağıdaki bölümde bahsedilen **AzureBlobFSSink** tür kopyalama etkinliği havuzu, geriye dönük uyumluluk için olduğu gibi hala desteklenmektedir. Bu yeni modeli ileride kullanmanızı öneririz. Data Factory yazma Kullanıcı arabirimi bu yeni türleri oluşturur.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToADLSGen2",
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
                    "type": "AzureBlobFSWriteSetting",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

#### <a name="other-format-sink"></a>Diğer biçim havuzu

**Orc/avro/JSON biçimindeki**Data Lake Storage 2. verileri kopyalamak için, **Havuz** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuz öğesinin type özelliği ayarlanmalıdır **AzureBlobFSSink**. |Evet |
| copyBehavior | Kaynak dosyaları bir dosya tabanlı veri deposundan olduğunda kopyalama davranışını tanımlar.<br/><br/>İzin verilen değerler şunlardır:<br/><b>-Preservehierarchy (varsayılan)</b>: Hedef klasördeki dosya hiyerarşisini korur. Kaynak dosyanın kaynak klasöre göreli yolu, hedef dosyanın göreli yoluyla hedef klasöre aynıdır.<br/><b>-DÜZEDEN hiyerarşi</b>: Kaynak klasördeki tüm dosyalar hedef klasörün ilk düzeyindedir. Hedef dosyalar otomatik olarak oluşturulan adlarına sahip. <br/><b>-Mergefiles</b>: Kaynak klasördeki tüm dosyaları tek bir dosya ile birleştirir. Dosya adı belirtilirse, birleştirilmiş dosya adı belirtilen adıdır. Aksi takdirde, bir otomatik olarak oluşturulan dosya adı değil. | Hayır |
| maxConcurrentConnections | Aynı anda veri deposuna bağlanacak bağlantı sayısı. Yalnızca veri deposuyla eşzamanlı bağlantıyı sınırlandırmak istediğinizde belirtin. | Hayır |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS Gen2 output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureBlobFSSink",
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

### <a name="some-recursive-and-copybehavior-examples"></a>Özyinelemeli ve copyBehavior örnekler

Bu bölümde, özyinelemeli ve copyBehavior değerlerinin farklı birleşimleri için kopyalama işleminin ortaya çıkan davranışı açıklanmaktadır.

| recursive | copyBehavior | Kaynak klasör yapısı | Sonuçta elde edilen hedef |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1, kaynak ile aynı yapıyla oluşturulur:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| true |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya3 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File4 için otomatik olarak oluşturulan ad<br/>&nbsp;&nbsp;&nbsp;&nbsp;File5 için otomatik olarak oluşturulan ad |
| true |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de dosya2 + dosya3 + File4 + File5 içeriği otomatik olarak oluşturulan dosya adıyla bir dosya halinde birleştirilir. |
| false |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/><br/>Subfolder1, File3, File4 ve File5 ile birlikte çekilmez. |
| false |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Klasör1<br/>&nbsp;&nbsp;&nbsp;&nbsp;fıle1'de otomatik olarak oluşturulan adı<br/>&nbsp;&nbsp;&nbsp;&nbsp;dosya2 için otomatik olarak oluşturulan ad<br/><br/>Subfolder1, File3, File4 ve File5 ile birlikte çekilmez. |
| false |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de<br/>&nbsp;&nbsp;&nbsp;&nbsp;Dosya2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Dosya3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | Hedef Klasör1 aşağıdaki yapısı ile oluşturulur: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Fıle1'de + dosya2 içeriği otomatik olarak oluşturulan dosya adıyla bir dosya halinde birleştirilir. fıle1'de otomatik olarak oluşturulan adı<br/><br/>Subfolder1, File3, File4 ve File5 ile birlikte çekilmez. |

## <a name="preserve-acls-from-data-lake-storage-gen1"></a>Data Lake Storage 1. ACL 'Leri koru

>[!TIP]
>Azure Data Lake Storage 1. verileri genel olarak Gen2 'e kopyalamak için bkz. bir adım adım ve en iyi uygulamalar için [Azure Data Factory ile Azure Data Lake Storage 1. verileri Gen2 'e kopyalama](load-azure-data-lake-storage-gen2-from-gen1.md) .

Dosyaları Azure Data Lake Storage 1. 'den Gen2 'e kopyaladığınızda, POSIX erişim denetim listelerini (ACL 'Ler) verilerle birlikte korumayı seçebilirsiniz. Access Control hakkında daha fazla bilgi için Azure Data Lake Storage 2. Azure Data Lake Storage 1. ve [erişim denetimi](../storage/blobs/data-lake-storage-access-control.md) [' nde erişim denetimi](../data-lake-store/data-lake-store-access-control.md) ' ne bakın.

Aşağıdaki ACL türleri Azure Data Factory kopyalama etkinliği kullanılarak korunabilir. Bir veya daha fazla tür seçebilirsiniz:

- **ACL**: Dosya ve dizinlerdeki POSIX erişim denetim listelerini kopyalayın ve koruyun. Var olan tüm ACL 'Leri kaynaktan havuza kopyalar. 
- **Sahip**: Dosya ve dizinlerin sahip olduğu kullanıcıyı kopyalayın ve koruyun. Data Lake Storage 2. havuza Süper Kullanıcı erişimi gerekiyor.
- **Grup**: Sahip olan dosya ve dizinlerin grubunu kopyalayın ve koruyun. Havuz Data Lake Storage 2. veya sahip olan kullanıcıya yönelik süper kullanıcı erişimi (sahip olan kullanıcı aynı zamanda hedef grubun üyesiyse) gerekli.

Bir klasörden kopyalamayı belirtirseniz Data Factory, söz konusu klasör için ACL 'leri ve altındaki dosyaları ve dizinleri çoğaltır, `recursive` true olarak ayarlanır. Tek bir dosyadan kopyalamayı belirtirseniz, bu dosyadaki ACL 'Ler kopyalanır.

>[!IMPORTANT]
>ACL 'Leri korumayı seçtiğinizde, Data Factory havuz Data Lake Storage 2. hesabınızda çalışması için yeterince yüksek izinler verdiğinizden emin olun. Örneğin, hesap anahtarı kimlik doğrulamasını kullanın veya Depolama Blobu veri sahibi rolünü hizmet sorumlusu veya yönetilen kimliğe atayın.

Kaynağı ikili kopyalama seçeneği veya ikili biçimi ve havuz Data Lake Storage 2. ikili kopya seçeneği veya ikili biçimle Data Lake Storage 1. olarak yapılandırdığınızda, **veri kopyalama aracı ayarları** sayfasında veya üzerinde **koru** seçeneğini bulabilirsiniz. Etkinlik yazma için **etkinlik** > **ayarlarını** kopyalama sekmesi.

![ACL 'yi korumak için Data Lake Storage 1. Gen2](./media/connector-azure-data-lake-storage/adls-gen2-preserve-acl.png)

JSON yapılandırmasına bir örnek aşağıda verilmiştir (bkz `preserve`.): 

```json
"activities":[
    {
        "name": "CopyFromGen1ToGen2",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "AzureDataLakeStoreSource",
                "recursive": true
            },
            "sink": {
                "type": "AzureBlobFSSink",
                "copyBehavior": "PreserveHierarchy"
            },
            "preserve": [
                "ACL",
                "Owner",
                "Group"
            ]
        },
        "inputs": [
            {
                "referenceName": "<Azure Data Lake Storage Gen1 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Data Lake Storage Gen2 output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Veri akışı eşleme özelliğinde [kaynak dönüştürme](data-flow-source.md) ve [Havuz dönüştürme](data-flow-sink.md) hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
