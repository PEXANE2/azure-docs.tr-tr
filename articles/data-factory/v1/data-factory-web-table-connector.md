---
title: Azure Veri Fabrikası'nı kullanarak verileri Web Tablosundan taşıma
description: Azure Veri Fabrikası'nı kullanarak Web sayfasındaki bir tablodaki verileri nasıl taşıyabilirsiniz hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: f54a26a4-baa4-4255-9791-5a8f935898e2
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/05/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: d2ea038c7d7212529185d77a6ba9e64deacb1c9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265720"
---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Web tablosu kaynağından taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-web-table-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-web-table.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Web tablo bağlayıcısı bölümüne](../connector-web-table.md)bakın.

Bu makalede, Verileri Web sayfasındaki bir tablodan desteklenen bir lavabo veri deposuna taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı özetlenmiştir. Bu makalede, kopya etkinliği ile veri hareketinin genel bir genel bakış ve kaynak /lavabo olarak desteklenen veri depolarının listesini sunan [veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesi üzerine bir temel oluşturur.

Veri fabrikası şu anda yalnızca web tablosundan diğer veri depolarına veri taşımayı destekler, ancak verileri diğer veri depolarından Web tablosu hedefine taşımaz.

> [!IMPORTANT]
> Bu Web bağlayıcısı şu anda yalnızca bir HTML sayfasından tablo içeriği ayıklama yı destekler. HTTP/s bitiş noktasından veri almak için, bunun yerine [HTTP bağlayıcısını](data-factory-http-connector.md) kullanın.

## <a name="prerequisites"></a>Ön koşullar

Bu Web tablosu bağlayıcısını kullanmak için, kendi kendine barındırılan Bir Tümleştirme Çalışma Süresi `gatewayName` (diğer adıyla Veri Yönetimi Ağ Geçidi) ayarlamanız ve bağlağa bağlı hizmetteki özelliği yapılandırmanız gerekir. Örneğin, Web tablosundan Azure Blob depolamaalanına kopyalamak için Azure Depolama bağlantılı hizmeti aşağıdaki gibi yapılandırın:

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>",
      "gatewayName": "<gateway name>"
    }
  }
}
```

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak şirket içi Cassandra veri deposundan veri aktaran bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz. 

- Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md) 
- Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın. 

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun. 
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.** 

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız.  Bir web tablosundan verileri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı bir örnek [için Bkz. JSON örneği: Verileri Web tablosundan Azure Blob bölümüne kopyalayın.](#json-example-copy-data-from-web-table-to-azure-blob) 

Aşağıdaki bölümler, Web tablosuna özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tablo, Web bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği ayarlanmalıdır: **Web** |Evet |
| Url |Web kaynağına URL |Evet |
| authenticationType |Anonim. |Evet |

### <a name="using-anonymous-authentication"></a>Anonim kimlik doğrulamasını kullanma

```json
{
    "name": "web",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablosu, vb.) için benzerdir.

**typeProperties** bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **WebTable** türü veri kümesi için typeProperties bölümü aşağıdaki özelliklere sahiptir

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |veri kümesinin türü. **WebTable** olarak ayarlanmalıdır |Evet |
| yol |Tabloyu içeren kaynağa göreli bir URL. |Hayır. Yol belirtilmediğinde, yalnızca bağlantılı hizmet tanımında belirtilen URL kullanılır. |
| dizin |Kaynaktaki tablonun dizini. Bkz. HTML sayfasındaki tablonun dizinini alma adımları için HTML sayfa bölümündeki tablonun [dizinini alın.](#get-index-of-a-table-in-an-html-page) |Evet |

**Örnek:**

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları ve ilke gibi özellikler tüm etkinlik türleri için kullanılabilir.

Oysa, etkinliğin typeProperties bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir.

Şu anda, kopyalama etkinliğindeki kaynak **WebSource**türünde olduğunda, ek özellikler desteklenmez.


## <a name="json-example-copy-data-from-web-table-to-azure-blob"></a>JSON örneği: Web tablosundan Azure Blob'a veri kopyalama
Aşağıdaki örnek gösterir:

1. [Web](#linked-service-properties)türüne bağlı bir hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
3. [WebTable](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [WebSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Kopyalama Etkinliği içeren bir [ardışık kaynak.](data-factory-create-pipelines.md)

Örnek, verileri Bir Web tablosundan her saat başı Azure blob'una kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

Aşağıdaki örnek, web tablosundaki verilerin Azure örneğine nasıl kopyalanılabildiğini gösterir. Ancak veriler, Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesinde belirtilen lavabolardan herhangi biri için doğrudan kopyalanabilir.

**Web bağlantılı hizmet** Bu örnek, anonim kimlik doğrulaması ile Web bağlantılı hizmeti kullanır. Kullanabileceğiniz farklı kimlik doğrulama türleri için [Web bağlantılı hizmet](#linked-service-properties) bölümüne bakın.

```json
{
    "name": "WebLinkedService",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
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
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>",
      "gatewayName": "<gateway name>"
    }
  }
}
```

**WebTable giriş veri kümesi** **True'nun** **dışında** ayar, Veri Fabrikası hizmetine veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.

> [!NOTE]
> Bkz. HTML sayfasındaki tablonun dizinini alma adımları için HTML sayfa bölümündeki tablonun [dizinini alın.](#get-index-of-a-table-in-an-html-page)  
>
>

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```


**Azure Blob çıktı veri kümesi**

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1).

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```



**Kopyalama etkinliği olan boru hattı**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **WebSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır.

WebSource tarafından desteklenen özellikler listesi için WebSource türü özelliklerine bakın.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "WebTableToAzureBlob",
        "description": "Copy from a Web table to an Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "WebTableInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "WebSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```

## <a name="get-index-of-a-table-in-an-html-page"></a>HTML sayfasındaki tablonun dizinini alma
1. **Excel 2016'yı** başlatın ve **Veri** sekmesine geçin.  
2. Araç çubuğunda **Yeni Sorgu'yu** tıklatın, **Diğer Kaynaklardan'a** işaret edin ve **Web'den'i**tıklatın.

    ![Güç Sorgusu menüsü](./media/data-factory-web-table-connector/PowerQuery-Menu.png)
3. **Web'den gelen** iletişim kutusuna, bağlantılı hizmet JSON'da kullanacağınız https://en.wikipedia.org/wiki/) **URL'yi** girin (örneğin: veri kümesi için belirteceğiniz yol ile birlikte (örneğin: AFI%27s_100_Years... 100_Movies) ve **Tamam'ı**tıklatın.

    ![Web iletişim kutusundan](./media/data-factory-web-table-connector/FromWeb-DialogBox.png)

    Bu örnekte kullanılan URL:https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies
4. **Access Web içerik** iletişim kutusunu görürseniz, doğru URL, kimlik **doğrulaması**seçin ve **Bağlan'ı**tıklatın. **URL**

   ![Web içeriği iletişim kutusuna erişin](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. Tablodaki içeriği görmek için ağaç görünümünde bir **tablo** öğesini tıklatın ve ardından alttaki **Düzenleme** düğmesini tıklatın.  

   ![Gezgin iletişim kutusu](./media/data-factory-web-table-connector/Navigator-DialogBox.png)
6. Sorgu **Düzenleyicisi** penceresinde, araç çubuğundaki **Gelişmiş Düzenleyici** düğmesini tıklatın.

    ![Gelişmiş Editör düğmesi](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. Gelişmiş Düzenleyici iletişim kutusunda, "Kaynak" yanındaki sayı dizindir.

    ![Gelişmiş Editör - Dizin](./media/data-factory-web-table-connector/AdvancedEditor-Index.png)

Excel 2013 kullanıyorsanız, dizini almak [için Excel için Microsoft Power Query'yi](https://www.microsoft.com/download/details.aspx?id=39379) kullanın. Ayrıntılar [için bir web sayfası makalesine bağlan'a](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) bakın. Masaüstü için Microsoft Power [BI](https://powerbi.microsoft.com/desktop/)kullanıyorsanız adımlar benzer.

> [!NOTE]
> Kaynak veri kümesinden sütunlara kadar sütunları haritalamak için Azure [Veri Fabrikası'nda veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.
