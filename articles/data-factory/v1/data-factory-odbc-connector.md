---
title: ODBC veri depolarından veri taşıma
description: Azure Veri Fabrikası'nı kullanarak ODBC veri depolarından verileri nasıl taşıyabildiğini öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: ad70a598-c031-4339-a883-c6125403cb76
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 11/19/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: e1735c2d2ed107f7ec65d68a6826267ee83a93f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281398"
---
# <a name="move-data-from-odbc-data-stores-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Verileri ODBC veri depolarından taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-odbc-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-odbc.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki ODBC konektörüne](../connector-odbc.md)bakın.


Bu makalede, verileri şirket içi ODBC veri deposundan taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır.

Verileri Bir ODBC veri deposundan desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablosuna bakın. Veri fabrikası şu anda yalnızca bir ODBC veri deposundan diğer veri depolarına veri taşımayı destekler, ancak verileri diğer veri depolarından ODBC veri deposuna taşımak için değildir.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="enabling-connectivity"></a>Bağlantı sağlama
Veri Fabrikası hizmeti, Veri Yönetimi Ağ Geçidi'ni kullanarak şirket içi ODBC kaynaklarına bağlanmayı destekler. Veri Yönetimi Ağ Geçidi ve ağ geçidini ayarlama yla ilgili adım adım talimatlar hakkında bilgi edinmek için [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında taşınan verileri görün. Bir Azure IaaS VM'de barındırılan olsa bile bir ODBC veri deposuna bağlanmak için ağ geçidini kullanın.

Ağ geçidini şirket içi makineye veya ODBC veri deposuyla aynı Azure VM'ye yükleyebilirsiniz. Ancak, kaynak çekişmesini önlemek ve daha iyi performans için ağ geçidini ayrı bir makineye/Azure IaaS VM'ye yüklemenizi öneririz. Ağ geçidini ayrı bir makineye yüklediğinizde, makine ODBC veri deposuyla makineye erişebilmeli.

Veri Yönetimi Ağ Geçidi'nin yanı sıra, ağ geçidi makinesinde veri deposu için ODBC sürücüsünü de yüklemeniz gerekir.

> [!NOTE]
> Sorun giderme bağlantısı/ağ geçidi yle ilgili sorunlarla ilgili ipuçları için [sorun giderme ağ geçidi sorunlarına](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) bakın.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak verileri ODBC veri deposundan hareket ettiren bir kopyalama etkinliği içeren bir ardışık hatlar oluşturabilirsiniz.

Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun.
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.**

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız. ODBC veri deposundan veri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı bir örnek [için Bkz. JSON örneği: Verileri ODBC veri deposundan azure Blob bölümüne](#json-example-copy-data-from-odbc-data-store-to-azure-blob) kopyalayın.

Aşağıdaki bölümler, ODBC veri deposuna özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tabloda, ODBC bağlantılı hizmete özgü JSON öğeleri için açıklama sağlanmış olur.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği ayarlanmalıdır: **OnPremisesOdbc** |Evet |
| Connectionstring |Bağlantı dizesinin erişmeyen kimlik bilgileri bölümü ve isteğe bağlı olarak şifrelenmiş kimlik bilgisi. Aşağıdaki bölümlerdeki örneklere bakın. <br/><br/>Ağ geçidi makinesinde ayarladığınız DSN (Veri Kaynağı Adı) gibi desenli `"Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;"`bağlantı `"DSN=<name of the DSN>;"` dizesini belirtebilir veya sistem DSN'yi (Veri Kaynak Adı) kullanabilir (yine de bağlantılı hizmetteki kimlik bölümünü buna göre belirtmeniz gerekir). |Evet |
| kimlik bilgisi |Bağlantı dizesinin sürücüye özgü özellik değeri biçiminde belirtilen erişim kimlik bilgileri bölümü. Örnek: `"Uid=<user ID>;Pwd=<password>;RefreshToken=<secret refresh token>;"`. |Hayır |
| authenticationType |ODBC veri deposuna bağlanmak için kullanılan kimlik doğrulama türü. Olası değerler şunlardır: Anonim ve Temel. |Evet |
| userName |Temel kimlik doğrulamasını kullanıyorsanız kullanıcı adını belirtin. |Hayır |
| password |Kullanıcı Adı için belirlediğiniz kullanıcı hesabının parolasını belirtin. |Hayır |
| ağ geçidiAdı |Veri Fabrikası hizmetinin ODBC veri deposuna bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |

### <a name="using-basic-authentication"></a>Temel kimlik doğrulamasını kullanma

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```
### <a name="using-basic-authentication-with-encrypted-credentials"></a>Şifreli kimlik bilgileriyle Temel kimlik doğrulamasını kullanma
Kimlik bilgilerini [New-AzDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/az.datafactory/new-azdatafactoryencryptvalue) (Azure PowerShell'in 1.0 sürümü) cmdlet'i veya [Yeni AzureDataFactoryEncryptValue'ı](https://msdn.microsoft.com/library/dn834940.aspx) (Azure PowerShell'in 0,9 veya önceki sürümü) kullanarak şifreleyebilirsiniz.

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-anonymous-authentication"></a>Anonim kimlik doğrulamasını kullanma

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "mygateway"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablosu, vb.) için benzerdir.

**typeProperties** bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **Tip İlişkisel Table** veri kümesi için typeProperties bölümü (ODBC dataset içerir) aşağıdaki özelliklere sahiptir

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |ODBC veri deposundaki tablonun adı. |Evet |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları ve ilkeler gibi özellikler tüm etkinlik türleri için kullanılabilir.

Diğer taraftan etkinliğin **typeProperties** bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir.

Kopyalama etkinliğinde, kaynak **RelationalSource** türünden (ODBC'yi içerir) olduğunda, typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin: MyTable'dan * seçin. |Evet |


## <a name="json-example-copy-data-from-odbc-data-store-to-azure-blob"></a>JSON örneği: Verileri ODBC veri deposundan Azure Blob'a kopyalama
Bu örnek, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık iş hattı oluşturmak için kullanabileceğiniz JSON tanımları sağlar. Bir ODBC kaynağından Azure Blob Depolama'ya verilerin nasıl kopyalanır olduğunu gösterir. Ancak, veriler Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) belirtilen lavabolardan herhangi biri için kopyalanabilir.

Örnekte aşağıdaki veri fabrikası varlıkları vardır:

1. [OnPremisesOdbc](#linked-service-properties)türüne bağlı bir hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
3. [İlişkisel Tablo](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [RelationalSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Copy Activity içeren bir [ardışık kaynak.](data-factory-create-pipelines.md)

Örnek, bir sorgu sonucundaki verileri her saat başı bir blob'a bir ODBC veri deposuna kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

İlk adım olarak, veri yönetimi ağ geçidini ayarlayın. Yönergeler, [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasındaki hareketli verilerde dir.

**ODBC bağlantılı hizmet** Bu örnek, Temel kimlik doğrulamasını kullanır. Kullanabileceğiniz farklı kimlik doğrulama türleri için [ODBC bağlantılı hizmet](#linked-service-properties) bölümüne bakın.

```json
{
    "name": "OnPremOdbcLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

**Azure Depolama bağlantılı hizmet**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

**ODBC giriş veri seti**

Örnek, ODBC veritabanında "MyTable" tablosu oluşturduğunuzu ve zaman serisi verileri için "zaman damgası sütunu" adlı bir sütun içerdiğini varsayar.

"Dış"ı ayarlamak: "true" veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini Veri Fabrikası hizmetine bildirir.

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "published": false,
        "type": "RelationalTable",
        "linkedServiceName": "OnPremOdbcLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

**Azure Blob çıktı veri kümesi**

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```json
{
    "name": "AzureBlobOdbcDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/odbc/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**ODBC kaynağı (RelationalSource) ve Blob lavabo (BlobSink) ile bir boru hattında kopyalama etkinliği**

Ardışık iş, bu giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **RelationalSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. **Sorgu** özelliği için belirtilen SQL sorgusu, kopyalanacak son saatteki verileri seçer.

```json
{
    "name": "CopyODBCToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "OdbcDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOdbcDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "OdbcToBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-odbc"></a>ODBC için tür eşleme
[Veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesinde belirtildiği gibi, Kopyalama etkinliği aşağıdaki iki adımlı yaklaşımla kaynak türlerinden lavabo türlerine otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürme
2. .NET türünden yerel lavabo türüne dönüştürme

ODBC veri depolarından veri taşınırken, ODBC veri türleri [ODBC Veri Türü Eşlemeleri](https://msdn.microsoft.com/library/cc668763.aspx) konusunda belirtildiği gibi .NET türlerine eşlenir.

## <a name="map-source-to-sink-columns"></a>Sütunları batırmak için harita kaynağı
Kaynak veri kümesindeki sütunları lavabo veri kümesindeki sütunlara eşleme hakkında bilgi edinmek için Azure [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri depolarından veri kopyalarken, istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği aklınızda bulundurun. Azure Veri Fabrikası'nda, bir dilimi el ile yeniden çalıştırabilirsiniz. Bir hata oluştuğunda dilimin yeniden çalıştırılaması için bir veri kümesi için yeniden deneme ilkesini de yapılandırabilirsiniz. Bir dilim her iki şekilde de yeniden çalıştırıldığında, bir dilim kaç kez çalıştırılırsa çalıştırın aynı verilerin okunduğundan emin olmanız gerekir. Bkz. [İlişkisel kaynaklardan tekrarlanabilir okuma.](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="troubleshoot-connectivity-issues"></a>Bağlantı sorunlarını giderme sorunları
Bağlantı sorunlarını gidermek için Veri Yönetimi **Ağ Geçidi Yapılandırma Yöneticisi'nin** **Tanılama** sekmesini kullanın.

1. **Başlat Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi**. Aşağıdaki resimde gösterildiği gibi **Microsoft Veri Yönetimi Ağ Geçidi** uygulamasına bağlantı bulmak için doğrudan "C:\Program Files\Microsoft Data Management Gateway\1.0\Shared\ConfigManager.exe" aramasını (veya) Ağ **Geçidi'ni** çalıştırabilirsiniz.

    ![Ağ geçidini ara](./media/data-factory-odbc-connector/search-gateway.png)
2. **Tanılama** sekmesine geçin.

    ![Ağ geçidi tanılaması](./media/data-factory-odbc-connector/data-factory-gateway-diagnostics.png)
3. Veri deposu **türünü** (bağlantılı hizmet) seçin.
4. **Kimlik doğrulamasını** belirtin ve veri deposuna bağlanmak için kullanılan **bağlantı dizesini** girin (veya) **kimlik bilgilerini** girin.
5. Veri deposuna olan bağlantıyı sınamak için **Test bağlantısını** tıklatın.

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.
