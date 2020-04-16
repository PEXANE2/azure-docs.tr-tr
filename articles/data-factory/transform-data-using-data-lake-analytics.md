---
title: U-SQL komut dosyalarını kullanarak verileri dönüştürme
description: Azure Veri Gölü Analizi bilgi işlem hizmetinde U-SQL komut dosyalarını çalıştırarak verileri nasıl işleyerek veya dönüştüreceklerini öğrenin.
services: data-factory
documentationcenter: ''
ms.author: abnarain
author: nabhishek
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/01/2018
ms.openlocfilehash: 427b7fff7b8f76412d7bd9d63aeb64583637779c
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418975"
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a>Azure Data Lake Analytics'te U-SQL betiklerini çalıştırarak verileri dönüştürme 
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-usql-activity.md)
> * [Geçerli sürüm](transform-data-using-data-lake-analytics.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure veri fabrikasındaki bir ardışık işlem, bağlantılı bilgi işlem hizmetlerini kullanarak bağlantılı depolama hizmetlerindeki verileri işler. Her etkinliğin belirli bir işleme işlemi gerçekleştirdiği bir dizi etkinlik içerir. Bu makalede, **Azure Veri Gölü Analytics** bilgi işlem bağlantılı bir hizmette **U-SQL** komut dosyası çalıştıran **Veri Gölü Analizi U-SQL Etkinliği** açıklanmaktadır. 

Data Lake Analytics U-SQL Etkinliği içeren bir ardışık hatlar oluşturmadan önce bir Azure Veri Gölü Analizi hesabı oluşturun. Azure Veri Gölü Analitiği hakkında bilgi edinmek için Azure [Veri Gölü Analitiği'ni](../data-lake-analytics/data-lake-analytics-get-started-portal.md)başlatın.


## <a name="azure-data-lake-analytics-linked-service"></a>Azure Veri Gölü Analizi bağlantılı hizmet
Bir Azure Veri Gölü Analizi bilgi işlem hizmetini bir Azure veri fabrikasına bağlamak için **Azure Veri Gölü Analytics** bağlantılı bir hizmet oluşturursunuz. Ardışık işlem hattındaki Data Lake Analytics U-SQL etkinliği bu bağlantılı hizmete başvurur. 

Aşağıdaki tablo, JSON tanımında kullanılan genel özelliklerin açıklamalarını sağlar. 

| Özellik                 | Açıklama                              | Gerekli                                 |
| ------------------------ | ---------------------------------------- | ---------------------------------------- |
| **Türü**                 | Tür özelliği şu şekilde ayarlanmalıdır: **AzureDataLakeAnalytics**. | Evet                                      |
| **Accountname**          | Azure Veri Gölü Analizi Hesap Adı.  | Evet                                      |
| **dataLakeAnalyticsUri** | Azure Veri Gölü Analytics URI.           | Hayır                                       |
| **subscriptionId**       | Azure abonelik kimliği                    | Hayır                                       |
| **resourceGroupName**    | Azure kaynak grubu adı                | Hayır                                       |

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması
Azure Veri Gölü Analizi bağlantılı hizmet, Azure Veri Gölü Analytics hizmetine bağlanmak için bir hizmet temel kimlik doğrulaması gerektirir. Hizmet temel kimlik doğrulamasını kullanmak için, bir uygulama varlığını Azure Etkin Dizini'ne (Azure AD) kaydedin ve hem Veri Gölü Analitiği'ne hem de kullandığı Veri Gölü Deposu'na erişim hakkı tanıyın. Ayrıntılı adımlar için [hizmete kimlik doğrulamasına](../data-lake-store/data-lake-store-authenticate-using-active-directory.md)bakın. Bağlantılı hizmeti tanımlamak için kullandığınız aşağıdaki değerlere dikkat edin:

* Uygulama Kimliği
* Uygulama anahtarı 
* Kiracı Kimliği

Kullanıcı Ekle Sihirbazı'nı kullanarak Azure Veri Gölü [Add User Wizard](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#add-a-new-user)Anatlyic'inize hizmet temel izni verin.

Aşağıdaki özellikleri belirterek hizmet temel kimlik doğrulamasını kullanın:

| Özellik                | Açıklama                              | Gerekli |
| :---------------------- | :--------------------------------------- | :------- |
| **hizmetPrincipalId**  | Uygulamanın istemci kimliğini belirtin.     | Evet      |
| **servicePrincipalKey** | Uygulamanın anahtarını belirtin.           | Evet      |
| **Kiracı**              | Uygulamanızın bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı kimliği) belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek geri alabilirsiniz. | Evet      |

**Örnek: Hizmet temel kimlik doğrulaması**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "<account name>",
            "dataLakeAnalyticsUri": "<azure data lake analytics URI>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "value": "<service principal key>",
                "type": "SecureString"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }       
    }
}
```

Bağlantılı hizmet hakkında daha fazla bilgi edinmek için [Bkz. Compute bağlantılı hizmetler.](compute-linked-services.md)

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL Etkinliği
Aşağıdaki JSON snippet, Data Lake Analytics U-SQL Activity içeren bir ardışık birimi tanımlar. Etkinlik tanımında, daha önce oluşturduğunuz Azure Veri Gölü Analizi bağlantılı hizmete bir başvuru vardır. Veri Gölü Analytics U-SQL komut dosyasını yürütmek için, Veri Fabrikası Belirttiğiniz komut dosyasını Veri Gölü Analytics'e gönderir ve veri gölü analizinin getirilip çıkarılabilmesi için gerekli giriş ve çıktılar komut dosyasında tanımlanır. 

```json
{
    "name": "ADLA U-SQL Activity",
    "description": "description",
    "type": "DataLakeAnalyticsU-SQL",
    "linkedServiceName": {
        "referenceName": "<linked service name of Azure Data Lake Analytics>",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "scriptLinkedService": {
            "referenceName": "<linked service name of Azure Data Lake Store or Azure Storage which contains the U-SQL script>",
            "type": "LinkedServiceReference"
        },
        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
        "degreeOfParallelism": 3,
        "priority": 100,
        "parameters": {
            "in": "/datalake/input/SearchLog.tsv",
            "out": "/datalake/output/Result.tsv"
        }
    }   
}
```

Aşağıdaki tabloda, bu faaliyete özgü özelliklerin adları ve açıklamaları açıklanmaktadır. 

| Özellik            | Açıklama                              | Gerekli |
| :------------------ | :--------------------------------------- | :------- |
| ad                | Boru hattındaki etkinliğin adı     | Evet      |
| açıklama         | Etkinliğin ne yaptığını açıklayan metin.  | Hayır       |
| type                | Data Lake Analytics U-SQL etkinliği için etkinlik türü **DataLakeAnalyticsU-SQL'dir.** | Evet      |
| linkedServiceName   | Azure Veri Gölü Analitiğine Bağlı Hizmet. Bu bağlantılı hizmet hakkında bilgi edinmek için [Bkz. Compute bağlantılı hizmetler](compute-linked-services.md) makalesine bakın.  |Evet       |
| scriptPath          | U-SQL komut dosyası içeren klasöre giden yol. Dosyanın adı büyük/küçük harf duyarlıdır. | Evet      |
| komut dosyasıLinkedService | Komut dosyasını içeren **Azure Veri Gölü Deposu'nu** veya **Azure Depolama'yı** veri fabrikasına bağlayan bağlantılı hizmet | Evet      |
| dereceOfParallelism | İşi çalıştırmak için aynı anda kullanılan en fazla düğüm sayısı. | Hayır       |
| Öncelik            | Önce çalıştırmak için sıraya giren tüm işlerin hangilerinin seçileceğini belirler. Sayı ne kadar düşükse, öncelik de o kadar yüksektir. | Hayır       |
| parametreler          | U-SQL komut dosyasına geçecek parametreler.    | Hayır       |
| runtimeVersion      | U-SQL altyapısının çalışma zamanı sürümü kullanılır. | Hayır       |
| Compilationmode     | <p>U-SQL derleme modu. Bu değerlerden biri olmalıdır: **Anlamsal:** Yalnızca anlamsal denetimler ve gerekli akıl sağlığı denetimleri gerçekleştirin, **Tam:** Sözdizimi denetimi, optimizasyon, kod oluşturma, vb. dahil olmak üzere tam derlemeyi gerçekleştirin, **SingleBox: SingleBox'a** TargetType ayarı ile tam derlemeyi gerçekleştirin. Bu özellik için bir değer belirtmezseniz, sunucu en iyi derleme modunu belirler. | Hayır |

Komut dosyası tanımı için [SearchLogProcessing.txt'ye](#sample-u-sql-script) bakın. 

## <a name="sample-u-sql-script"></a>Örnek U-SQL komut dosyası

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int,
            Urls            string,
            ClickedUrls     string
    FROM @in
    USING Extractors.Tsv(nullEscape:"#NULL#");

@rs1 =
    SELECT Start, Region, Duration
    FROM @searchlog
WHERE Region == "en-gb";

@rs1 =
    SELECT Start, Region, Duration
    FROM @rs1
    WHERE Start <= DateTime.Parse("2012/02/19");

OUTPUT @rs1   
    TO @out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

Yukarıdaki komut dosyası örneğinde, komut dosyasına giriş ve çıktı giriş ve ** \@çıkış** parametrelerinin ** \@içinde** ve dışında tanımlanır. U-SQL komut dosyasındaki ** \@in** ve ** \@çıkış** parametrelerinin değerleri Veri Fabrikası tarafından 'parametreler' bölümü kullanılarak dinamik olarak geçirilir. 

Azure Veri Gölü Analizi hizmetinde çalışan işler için boru hattı tanımınızda DereceBenzersizliği ve önceliği gibi diğer özellikleri de belirtebilirsiniz.

## <a name="dynamic-parameters"></a>Dinamik parametreler
Örnek boru hattı tanımında, içi ve dışı parametreler sabit kodlu değerlerle atanır. 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

Bunun yerine dinamik parametreleri kullanmak mümkündür. Örneğin: 

```json
"parameters": {
    "in": "/datalake/input/@{formatDateTime(pipeline().parameters.WindowStart,'yyyy/MM/dd')}/data.tsv",
    "out": "/datalake/output/@{formatDateTime(pipeline().parameters.WindowStart,'yyyy/MM/dd')}/result.tsv"
}
```

Bu durumda, giriş dosyaları hala /datalake/input klasöründen alınır ve çıkış dosyaları /datalake/çıktı klasöründe oluşturulur. Dosya adları, ardışık iş aksama tetiklendiğinde geçirilen pencere başlangıç saatine göre dinamiktir.  

## <a name="next-steps"></a>Sonraki adımlar
Verileri başka şekillerde nasıl dönüştüreceklerini açıklayan aşağıdaki makalelere bakın: 

* [Kovan aktivitesi](transform-data-using-hadoop-hive.md)
* [Domuz aktivitesi](transform-data-using-hadoop-pig.md)
* [MapAz etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop Akış etkinliği](transform-data-using-hadoop-streaming.md)
* [Kıvılcım etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Makine Öğrenimi Toplu Yürütme Etkinliği](transform-data-using-machine-learning.md)
* [Depolanan yordam etkinliği](transform-data-using-stored-procedure.md)
