---
title: Azure Veri Fabrikası'nı kullanarak Web Tablosundan veri kopyalama
description: Veri Fabrikası tarafından desteklenen veri depolarına bir web tablosundan verileri lavabo olarak kopyalamanızı sağlayan Azure Veri Fabrikası Web Tablo Bağlayıcısı hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: jingwang
ms.openlocfilehash: 0eb4d37342685c13027a69bb6cb85f618fa63f20
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81410210"
---
# <a name="copy-data-from-web-table-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Web tablosundan veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-web-table-connector.md)
> * [Geçerli sürüm](connector-web-table.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Web tablosu veritabanındaki verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

Bu Web tablo bağlayıcısı, [REST bağlayıcısı](connector-rest.md) ve [HTTP bağlayıcısı](connector-http.md) arasındaki fark şunlardır:

- **Web tablosu bağlayıcısı** tablo içeriğini bir HTML web sayfasından ayıklar.
- **REST bağlayıcısı** özellikle RESTful API'lerden veri kopyalamayı destekler.
- **HTTP bağlayıcısı,** herhangi bir HTTP bitiş noktasından (örneğin dosyayı indirmek için) veri almak için geneldir. 

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Web tablosu bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Verileri Web tablosu veritabanından desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu Web tablosu **bağlayıcısı bir HTML sayfasından tablo içeriği ayıklama**destekler.

## <a name="prerequisites"></a>Ön koşullar

Bu Web tablosu bağlayıcısını kullanmak için, kendi kendine barındırılan Bir Tümleştirme Çalışma Süresi ayarlamanız gerekir. Ayrıntılar için [Kendi barındırılan Tümleştirme Çalışma Zamanı](create-self-hosted-integration-runtime.md) makalesine bakın.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Web tablosu bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Web tablosuna bağlı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **Web** |Evet |
| url | Web kaynağına URL |Evet |
| authenticationType | İzin verilen değer: **Anonim**. |Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) [Önkoşullarda](#prerequisites)belirtildiği gibi Kendi kendine barındırılan Tümleştirme Çalışma Süresi gereklidir. |Evet |

**Örnek:**

```json
{
    "name": "WebLinkedService",
    "properties": {
        "type": "Web",
        "typeProperties": {
            "url" : "https://en.wikipedia.org/wiki/",
            "authenticationType": "Anonymous"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, Web tablosu veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Web tablosundaki verileri kopyalamak için, veri kümesinin tür özelliğini **WebTable**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **WebTable** | Evet |
| yol |Tabloyu içeren kaynağa göreli bir URL. |Hayır. Yol belirtilmediğinde, yalnızca bağlantılı hizmet tanımında belirtilen URL kullanılır. |
| dizin |Kaynaktaki tablonun dizini. Bkz. HTML sayfasındaki tablonun dizinini alma adımları için HTML sayfa bölümündeki tablonun [dizinini alın.](#get-index-of-a-table-in-an-html-page) |Evet |

**Örnek:**

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Web linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, Web tablo kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="web-table-as-source"></a>Kaynak olarak Web tablosu

Web tablosundaki verileri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **WebSource**olarak ayarlayın, ek özellikler desteklenmez.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromWebTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Web table input dataset name>",
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
                "type": "WebSource"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="get-index-of-a-table-in-an-html-page"></a>HTML sayfasındaki tablonun dizinini alma

[Veri kümesi özelliklerinde](#dataset-properties)yapılandırmanız gereken bir tablonun dizinini almak için, örneğin Excel 2016'yı aşağıdaki araç olarak kullanabilirsiniz:

1. **Excel 2016'yı** başlatın ve **Veri** sekmesine geçin.
2. Araç çubuğunda **Yeni Sorgu'yu** tıklatın, **Diğer Kaynaklardan'a** işaret edin ve **Web'den'i**tıklatın.

    ![Güç Sorgusu menüsü](./media/copy-data-from-web-table/PowerQuery-Menu.png)
3. **Web'den gelen** iletişim kutusuna, bağlantılı hizmet JSON'da kullanacağınız https://en.wikipedia.org/wiki/) **URL'yi** girin (örneğin: veri kümesi için belirteceğiniz yol ile birlikte (örneğin: AFI%27s_100_Years... 100_Movies) ve **Tamam'ı**tıklatın.

    ![Web iletişim kutusundan](./media/copy-data-from-web-table/FromWeb-DialogBox.png)

    Bu örnekte kullanılan URL:https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies
4. **Access Web içerik** iletişim kutusunu görürseniz, doğru URL, kimlik **doğrulaması**seçin ve **Bağlan'ı**tıklatın. **URL**

   ![Web içeriği iletişim kutusuna erişin](./media/copy-data-from-web-table/AccessWebContentDialog.png)
5. Tablodaki içeriği görmek için ağaç görünümünde bir **tablo** öğesini tıklatın ve ardından alttaki **Düzenleme** düğmesini tıklatın.  

   ![Gezgin iletişim kutusu](./media/copy-data-from-web-table/Navigator-DialogBox.png)
6. Sorgu **Düzenleyicisi** penceresinde, araç çubuğundaki **Gelişmiş Düzenleyici** düğmesini tıklatın.

    ![Gelişmiş Editör düğmesi](./media/copy-data-from-web-table/QueryEditor-AdvancedEditorButton.png)
7. Gelişmiş Düzenleyici iletişim kutusunda, "Kaynak" yanındaki sayı dizindir.

    ![Gelişmiş Editör - Dizin](./media/copy-data-from-web-table/AdvancedEditor-Index.png)

Excel 2013 kullanıyorsanız, dizini almak [için Excel için Microsoft Power Query'yi](https://www.microsoft.com/download/details.aspx?id=39379) kullanın. Ayrıntılar [için bir web sayfası makalesine bağlan'a](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) bakın. Masaüstü için Microsoft Power [BI](https://powerbi.microsoft.com/desktop/)kullanıyorsanız adımlar benzer.


## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
