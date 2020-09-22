---
title: SYNAPSE bağlantısı ile Azure Cosmos DB verileri çözümlemek için Power BI ve SQL sunucusuz
description: Azure Cosmos DB için SYNAPSE SQL sunucusuz veritabanı ve görünümleri oluşturma hakkında bilgi edinin, Azure Cosmos kapsayıcılarını sorgulayın ve sonra bu görünümler üzerinde Power BI olan bir model oluşturun.
author: ArnoMicrosoft
ms.service: cosmos-db
ms.topic: how-to
ms.date: 09/22/2020
ms.author: acomet
ms.openlocfilehash: 03ea1b0cdfef30935b38078d0811d1408a78c41e
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941183"
---
# <a name="use-power-bi-and-synapse-sql-serverless-to-analyze-azure-cosmos-db-data-with-synapse-link-preview"></a>SYNAPSE bağlantısı ile Azure Cosmos DB verileri çözümlemek için Power BI ve SQL sunucusuz kullanın (Önizleme)

Bu makalede, Azure Cosmos DB için bir Synapse SQL sunucusuz (daha önce **SQL isteğe**bağlı olarak adlandırılan) veritabanı ve görünümleri oluşturma hakkında bilgi edineceksiniz. Azure Cosmos kapsayıcılarını sorgular ve ardından bu görünümler üzerinde Power BI olan ve bu sorguyu yansıtan bir model oluşturacaksınız.

> [!NOTE]
> SYNAPSE SQL sunucusuz ile Azure Cosmos DB analitik deposunun kullanılması şu anda geçitli önizleme aşamasındadır. Erişim istemek için [Azure Cosmos DB ekibine](mailto:cosmosdbsynapselink@microsoft.com)ulaşın.

Bu senaryoda, bir iş ortağı perakende deposunda Surface ürün satışları hakkında kukla verileri kullanacaksınız. Her mağaza için gelir, büyük bir evye yakınlık ve belirli bir hafta için tanıtım etkisi doğrultusunda analiz edersiniz. Bu makalede, **RetailSales** ve **storedemographics** adlı iki görünüm ve aralarında bir sorgu oluşturacaksınız. Bu [GitHub](https://github.com/Azure-Samples/Synapse/tree/master/Notebooks/PySpark/Synapse%20Link%20for%20Cosmos%20DB%20samples/Retail/RetailData) deposundan örnek ürün verilerini alabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki kaynakları oluşturduğunuzdan emin olun:

* [SQL (Core) veya MongoDB türünde bir Azure Cosmos DB hesabı oluşturun.](create-cosmosdb-resources-portal.md)

* [Azure Cosmos hesabınız](configure-synapse-link.md#enable-synapse-link) Için Azure SYNAPSE bağlantısını etkinleştirme

* Azure Cosmos hesabı ve [analitik deponun etkinleştirildiği](configure-synapse-link.md#create-analytical-ttl) iki kapsayıcı içinde bir veritabanı oluşturun.

* Ürün verilerini, bu [toplu veri](https://github.com/Azure-Samples/Synapse/blob/master/Notebooks/PySpark/Synapse%20Link%20for%20Cosmos%20DB%20samples/Retail/spark-notebooks/pyspark/1CosmoDBSynapseSparkBatchIngestion.ipynb) alma not defterinde açıklandığı gibi Azure Cosmos kapsayıcılarına yükleyin.

* **SynapseLinkBI**adlı [bir Synapse çalışma alanı oluşturun](../synapse-analytics/quickstart-create-workspace.md) .

* [Azure Cosmos veritabanını SYNAPSE çalışma alanına bağlayın](../synapse-analytics/synapse-link/how-to-connect-synapse-link-cosmos-db.md?toc=/azure/cosmos-db/toc.json&bc=/azure/cosmos-db/breadcrumb/toc.json).

## <a name="create-a-database-and-views"></a>Veritabanı ve görünümler oluşturma

SYNAPSE çalışma alanından **Geliştir** sekmesine gidip, **+** simgeyi seçin ve **SQL betiği**' ni seçin.

:::image type="content" source="./media/synapse-link-power-bi/add-sql-script.png" alt-text="SYNAPSE Analytics çalışma alanına bir SQL betiği ekleyin":::

Her çalışma alanı bir Synapse SQL sunucusuz uç noktasıyla gelir. SQL betiği oluşturduktan sonra, üstteki araç çubuğundan **isteğe bağlı SQL**'e bağlanın.

:::image type="content" source="./media/synapse-link-power-bi/enable-sql-on-demand-endpoint.png" alt-text="SQL betiğini, çalışma alanında SYNAPSE SQL sunucusuz uç noktasını kullanacak şekilde etkinleştirin":::

**RetailCosmosDB**adlı yeni bir veritabanı ve SYNAPSE bağlantısı etkinleştirilmiş kapsayıcılar ÜZERINDE bir SQL görünümü oluşturun. Aşağıdaki komut, bir veritabanının nasıl oluşturulacağını gösterir:

```sql
-- Create database
Create database RetailCosmosDB
```

Daha sonra, farklı SYNAPSE bağlantısı etkin Azure Cosmos kapsayıcıları üzerinde birden çok görünüm oluşturun. Bu, farklı kapsayıcılarda oturmuş Azure Cosmos DB verileri birleştirmek ve sorgulamak için T-SQL ' i kullanmanıza imkan tanır.  Görünümleri oluştururken **RetailCosmosDB** veritabanını seçtiğinizden emin olun.

Aşağıdaki betikler her kapsayıcıda görünümlerin nasıl oluşturulacağını göstermektedir. Basitlik için, SYNAPSE SQL Server 'ın [Otomatik Şema çıkarımı](analytical-store-introduction.md#analytical-schema) özelliğini SYNAPSE bağlantısı etkinleştirilmiş kapsayıcılardan kullanalım:


### <a name="retailsales-view"></a>RetailSales görünümü:

```sql
-- Create view for RetailSales container
CREATE VIEW  RetailSales
AS  
SELECT  *
FROM OPENROWSET (
    'CosmosDB', N'account=<Your Azure Cosmos account name>;database=<Your Azure Cosmos database name>;region=<Your Azure Cosmos DB Region>;key=<Your Azure Cosmos DB key here>',RetailSales)
AS q1
```

Önceki SQL betiğine Azure Cosmos DB bölgenizi ve birincil anahtarı eklediğinizden emin olun. Bölge adındaki tüm karakterlerin boşluklar olmadan küçük bir durumda olması gerekir. Komutun diğer parametrelerinin aksine `OPENROWSET` , kapsayıcı adı parametresi etrafında tırnak işareti olmadan belirtilmelidir.

### <a name="storedemographics-view"></a>StoreDemographics görünümü:

```sql
-- Create view for StoreDemographics container
CREATE VIEW StoreDemographics
AS  
SELECT  *
FROM OPENROWSET (
    'CosmosDB', N'account=<Your Azure Cosmos account name>;database=<Your Azure Cosmos database name>;region=<Your Azure Cosmos DB Region>;key=<Your Azure Cosmos DB key here>', StoreDemographics)
AS q1
```

Şimdi **Çalıştır** komutunu seçerek SQL betiğini çalıştırın.

## <a name="query-the-views"></a>Görünümleri sorgulama

Bu iki görünüm oluşturuldığına göre, bu iki görünümü şu şekilde birleştiren sorguyu tanımlayalim:

```sql
SELECT 
sum(p.[revenue]) as revenue
,p.[advertising]
,p.[storeId]
,p.[weekStarting]
,q.[largeHH]
 FROM [dbo].[RetailSales] as p
INNER JOIN [dbo].[StoreDemographics] as q ON q.[storeId] = p.[storeId]
GROUP BY p.[advertising], p.[storeId], p.[weekStarting], q.[largeHH]
```

Sonuç olarak aşağıdaki tabloyu sağlayan **Çalıştır** ' ı seçin:

:::image type="content" source="./media/synapse-link-power-bi/join-views-query-results.png" alt-text="StoreDemographics ve RetailSales görünümlerine katıldıktan sonra sorgu sonuçları":::

## <a name="model-views-over-containers-with-power-bi"></a>Power BI olan kapsayıcılar üzerinde model görüntüleme

Ardından Power BI Masaüstünü açın ve aşağıdaki adımları kullanarak SYNAPSE SQL sunucusuz uç noktasına bağlanın:

1. Power BI Desktop uygulamasını açın. **Veri al** ' ı seçin ve **daha fazlasını**seçin.

1. Bağlantı seçenekleri listesinden **Azure SYNAPSE Analytics (SQL DW)** öğesini seçin.

1. Veritabanının bulunduğu SQL uç noktasının adını girin. `SynapseLinkBI-ondemand.sql.azuresynapse.net` **Sunucu** alanına girin. Bu örnekte,  **SynapseLinkBI** , çalışma alanının adıdır. Çalışma alanınıza farklı bir ad vermiş olmanız durumunda bunu değiştirin. Veri bağlantısı modu için **doğrudan sorgu** seçeneğini belirleyin ve ardından **Tamam**' ı seçin.

1. Azure AD gibi tercih edilen kimlik doğrulama yöntemini seçin.

1. **RetailCosmosDB** veritabanını ve **RetailSales**, **storedemographics** görünümlerini seçin.

1. İki görünümü doğrudan sorgu moduna yüklemek için **Yükle** ' yi seçin.

1. **StoreID** sütunu aracılığıyla iki görünüm arasında bir ilişki oluşturmak için **model** ' i seçin.

1. **StoreID** sütununu **storedemographics** görünümündeki **StoreID** sütununa doğru **RetailSales** görünümünden sürükleyin.

1. **RetailSales** görünümünde aynı depo kimliğine sahip birden çok satır olduğundan ancak **storedemographics** 'ın yalnızca bir depo kimliği satırı (bir boyut tablosu olduğu) olduğundan, birden çok-tek (*: 1) ilişkisi seçin

Şimdi **rapor** penceresine giderek, ev boyutunun, gelir ve largehh dizininin dağınık gösterimine dayanarak mağaza başına düşen ortalama gelire göre karşılaştırmak için bir rapor oluşturun:

1. **Dağılım grafiği**' ni seçin.

1. **Largehh** 'Yi **Storedemographics** görünümünden X eksenine sürükleyip bırakın.

1. **RetailSales** görünümünden **gelir** sürükleyip Y eksenine sürükleyin ve bırakın. Her mağazaya ve haftaya göre ürün başına ortalama satışları almak için **Ortalama** ' ı seçin.

1. Belirli bir ürün satırını seçmek için **productCode** from **RetailSales** View öğesini göstergeye sürükleyip bırakın.
Bu seçenekleri belirledikten sonra, aşağıdaki ekran görüntüsüne benzer bir grafik görmeniz gerekir:

:::image type="content" source="./media/synapse-link-power-bi/household-size-average-revenue-report.png" alt-text="Ev boyutunun göreli önem derecesini mağaza başına ortalama gelire göre karşılaştıran rapor":::

## <a name="next-steps"></a>Sonraki adımlar

[Azure açık veri kümelerini analiz etmek ve Azure SYNAPSE Studio 'da sonuçları görselleştirmek](../synapse-analytics/sql/tutorial-data-analyst.md) IÇIN SYNAPSE SQL sunucusuz kullanın
