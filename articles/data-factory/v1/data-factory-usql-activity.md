---
title: U-SQL komut dosyası kullanarak verileri dönüştürme - Azure
description: Azure Veri Gölü Analizi bilgi işlem hizmetinde U-SQL komut dosyalarını çalıştırarak verileri nasıl işleyerek veya dönüştüreceklerini öğrenin.
services: data-factory
documentationcenter: ''
ms.assetid: e17c1255-62c2-4e2e-bb60-d25274903e80
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/01/2017
author: nabhishek
ms.author: abnarain
manager: anandsub
robots: noindex
ms.openlocfilehash: c6d3510dfdd02bf2eb07d656c706c44d895c582d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74927896"
---
# <a name="transform-data-by-running-u-sql-scripts-on-azure-data-lake-analytics"></a>Azure Data Lake Analytics'te U-SQL betiklerini çalıştırarak verileri dönüştürme 
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-usql-activity.md)
> * [Sürüm 2 (geçerli sürüm)](../transform-data-using-data-lake-analytics.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki U-SQL Etkinliği'ne](../transform-data-using-data-lake-analytics.md)bakın.

Azure veri fabrikasındaki bir ardışık işlem, bağlantılı bilgi işlem hizmetlerini kullanarak bağlantılı depolama hizmetlerindeki verileri işler. Her etkinliğin belirli bir işleme işlemi gerçekleştirdiği bir dizi etkinlik içerir. Bu makalede, **Azure Veri Gölü Analytics** bilgi işlem bağlantılı bir hizmette **U-SQL** komut dosyası çalıştıran **Veri Gölü Analizi U-SQL Etkinliği** açıklanmaktadır. 

Data Lake Analytics U-SQL Etkinliği içeren bir ardışık hatlar oluşturmadan önce bir Azure Veri Gölü Analizi hesabı oluşturun. Azure Veri Gölü Analitiği hakkında bilgi edinmek için Azure [Veri Gölü Analitiği'ni](../../data-lake-analytics/data-lake-analytics-get-started-portal.md)başlatın.

Bir veri fabrikası, bağlantılı hizmetler, veri kümeleri ve bir ardışık hatlar oluşturmak için ayrıntılı adımlar için [ilk ardışık yapı öğreticinizi](data-factory-build-your-first-pipeline.md) gözden geçirin. Veri Fabrikası varlıkları oluşturmak için Veri Fabrikası Düzenleyicisi veya Visual Studio veya Azure PowerShell ile JSON parçacıklarını kullanın.

## <a name="supported-authentication-types"></a>Desteklenen kimlik doğrulama türleri
U-SQL etkinliği, Veri Gölü Analizi'ne karşı kimlik doğrulama türlerinin altında destekler:
* Hizmet sorumlusu kimlik doğrulaması
* Kullanıcı kimlik bilgisi (OAuth) kimlik doğrulaması 

Özellikle zamanlanmış bir U-SQL yürütmesi için hizmet temel kimlik doğrulaması kullanmanızı öneririz. Kullanıcı kimlik bilgisi kimlik doğrulaması ile belirteç sona erme davranışı oluşabilir. Yapılandırma ayrıntıları için [Bağlantılı hizmet özellikleri](#azure-data-lake-analytics-linked-service) bölümüne bakın.

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Veri Gölü Analizi Bağlantılı Hizmet
Bir Azure Veri Gölü Analizi bilgi işlem hizmetini bir Azure veri fabrikasına bağlamak için **Azure Veri Gölü Analytics** bağlantılı bir hizmet oluşturursunuz. Ardışık işlem hattındaki Data Lake Analytics U-SQL etkinliği bu bağlantılı hizmete başvurur. 

Aşağıdaki tablo, JSON tanımında kullanılan genel özelliklerin açıklamalarını sağlar. Ayrıca hizmet sorumlusu ile kullanıcı kimlik bilgisi kimlik doğrulaması arasında seçim yapabilirsiniz.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| **Türü** |Tür özelliği şu şekilde ayarlanmalıdır: **AzureDataLakeAnalytics**. |Evet |
| **Accountname** |Azure Veri Gölü Analizi Hesap Adı. |Evet |
| **dataLakeAnalyticsUri** |Azure Veri Gölü Analytics URI. |Hayır |
| **subscriptionId** |Azure abonelik kimliği |Hayır (Belirtilmemişse, veri fabrikasıaboneliği kullanılır). |
| **resourceGroupName** |Azure kaynak grubu adı |Hayır (Belirtilmemişse, veri fabrikasının kaynak grubu kullanılır). |

### <a name="service-principal-authentication-recommended"></a>Hizmet temel kimlik doğrulaması (önerilir)
Hizmet temel kimlik doğrulamasını kullanmak için, azure Etkin Dizin'e (Azure AD) bir uygulama varlığı kaydedin ve veri gölü deposuna erişim izni verin. Ayrıntılı adımlar için [hizmete kimlik doğrulamasına](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md)bakın. Bağlantılı hizmeti tanımlamak için kullandığınız aşağıdaki değerlere dikkat edin:
* Uygulama Kimliği
* Uygulama anahtarı 
* Kiracı Kimliği

Aşağıdaki özellikleri belirterek hizmet temel kimlik doğrulamasını kullanın:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **hizmetPrincipalId** | Uygulamanın istemci kimliğini belirtin. | Evet |
| **servicePrincipalKey** | Uygulamanın anahtarını belirtin. | Evet |
| **Kiracı** | Uygulamanızın bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı kimliği) belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek geri alabilirsiniz. | Evet |

**Örnek: Hizmet temel kimlik doğrulaması**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Kullanıcı kimlik bilgisi kimlik doğrulaması
Alternatif olarak, aşağıdaki özellikleri belirterek Data Lake Analytics için kullanıcı kimlik bilgilerini kullanabilirsiniz:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| **Yetkilendirme** | Veri Fabrikası Düzenleyicisi'ndeki **Yetkilendirme** düğmesini tıklatın ve bu özelliğe otomatik olarak oluşturulmuş yetkilendirme URL'sini atayan kimlik bilgilerinizi girin. | Evet |
| **Sessionıd** | OAuth yetkilendirme oturumundan OAuth oturum kimliği. Her oturum kimliği benzersizdir ve yalnızca bir kez kullanılabilir. Veri Fabrikası Düzenleyicisi'ni kullandığınızda bu ayar otomatik olarak oluşturulur. | Evet |

**Örnek: Kullanıcı kimlik bilgisi kimlik doğrulaması**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a>Belirteç sona ermesi
**Yetkilendirme** düğmesini kullanarak oluşturduğunuz yetkilendirme kodunun süresi bir süre sonra sona erer. Farklı kullanıcı hesabı türleri için son kullanma saatleri için aşağıdaki tabloya bakın. Kimlik doğrulama **belirteci sona erdiğinde**aşağıdaki hata iletisini görebilirsiniz : Kimlik doğrulama işlemi hatası: invalid_grant - AADSTS70002: Hata doğrulama kimlik bilgileri. AADSTS70008: Sağlanan erişim izninin süresi doldu veya iptal edildi. İz ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korelasyon Id: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Timestamp: 2015-12-15 21:09:31Z

| Kullanıcı türü | Sonra sona erer |
|:--- |:--- |
| Azure Active Directory tarafından yönetilmeyen kullanıcı hesapları (@hotmail.com, , @live.comvb.) |12 saat |
| Azure Active Directory (AAD) tarafından yönetilen kullanıcı hesapları |Son dilim çalışmasından 14 gün sonra. <br/><br/>OAuth tabanlı bağlantılı hizmeti temel alan bir dilim en az 14 günde bir çalışıyorsa, 90 gün. |

Bu hatayı önlemek/gidermek için, **belirteç süresi dolduğunda** **Yetkilendirme** düğmesini kullanarak yeniden yetkilendirmeyi ve bağlantılı hizmeti yeniden dağıtın. Ayrıca, aşağıdaki gibi kodu kullanarak programlı olarak **sessionId** ve **yetkilendirme** özellikleri için değerler oluşturabilirsiniz:

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

Kodda kullanılan Veri Fabrikası sınıfları hakkında ayrıntılı bilgi için [AzureDataLakeStoreLinkedService Sınıfı,](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx) [AzureDataLakeAnalyticsLinkedService Sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)ve [YetkilendirmeSessionGetResponse Sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) konularına bakın. WindowsFormsWebAuthenticationDialog sınıfı için: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll adresine başvuru ekleyin. 

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL Etkinliği
Aşağıdaki JSON snippet, Data Lake Analytics U-SQL Activity içeren bir ardışık birimi tanımlar. Etkinlik tanımında, daha önce oluşturduğunuz Azure Veri Gölü Analizi bağlantılı hizmete bir başvuru vardır.   

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00Z",
        "end": "2015-08-08T01:00:00Z",
        "isPaused": false
    }
}
```

Aşağıdaki tabloda, bu faaliyete özgü özelliklerin adları ve açıklamaları açıklanmaktadır. 

| Özellik            | Açıklama                              | Gerekli                                 |
| :------------------ | :--------------------------------------- | :--------------------------------------- |
| type                | Tür özelliği **DataLakeAnalyticsU-SQL**olarak ayarlanmalıdır. | Evet                                      |
| linkedServiceName   | Veri Fabrikası'nda bağlantılı hizmet olarak kayıtlı Azure Veri Gölü Analytics'e başvuru | Evet                                      |
| scriptPath          | U-SQL komut dosyası içeren klasöre giden yol. Dosyanın adı büyük/küçük harf duyarlıdır. | Hayır (komut dosyası kullanıyorsanız)                   |
| komut dosyasıLinkedService | Komut dosyasını içeren depolamayı veri fabrikasına bağlayan bağlantılı hizmet | Hayır (komut dosyası kullanıyorsanız)                   |
| betiğini çalıştırın              | ScriptPath ve scriptLinkedService belirtmek yerine satır içi komut dosyası belirtin. Örneğin: `"script": "CREATE DATABASE test"`. | Hayır (scriptPath ve scriptLinkedService kullanıyorsanız) |
| dereceOfParallelism | İşi çalıştırmak için aynı anda kullanılan en fazla düğüm sayısı. | Hayır                                       |
| Öncelik            | Önce çalıştırmak için sıraya giren tüm işlerin hangilerinin seçileceğini belirler. Sayı ne kadar düşükse, öncelik de o kadar yüksektir. | Hayır                                       |
| parametreler          | U-SQL komut dosyası için parametreler          | Hayır                                       |
| runtimeVersion      | U-SQL altyapısının kullanım zamanı sürümü | Hayır                                       |
| Compilationmode     | <p>U-SQL derleme modu. Bu değerlerden biri olmalıdır:</p> <ul><li>**Anlamsal:** Yalnızca anlamsal kontroller ve gerekli akıl sağlığı kontrollerini gerçekleştirin.</li><li>**Tam olarak:** Sözdizimi denetimi, en iyi duruma getirme, kod oluşturma vb. dahil olmak üzere tam derlemeyi gerçekleştirin.</li><li>**SingleBox:** TargetType ayarı ile SingleBox'a tam derlemeyi gerçekleştirin.</li></ul><p>Bu özellik için bir değer belirtmezseniz, sunucu en iyi derleme modunu belirler. </p> | Hayır                                       |

Komut dosyası tanımı için [SearchLogProcessing.txt Script Tanımı'na](#sample-u-sql-script) bakın. 

## <a name="sample-input-and-output-datasets"></a>Örnek giriş ve çıktı veri kümeleri
### <a name="input-dataset"></a>Giriş veri kümesi
Bu örnekte, giriş verileri bir Azure Veri Gölü Deposu'nda bulunur (Datalake/input klasöründeki SearchLog.tsv dosyası). 

```json
{
    "name": "DataLakeTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}    
```

### <a name="output-dataset"></a>Çıktı veri kümesi
Bu örnekte, U-SQL komut dosyası tarafından üretilen çıktı verileri bir Azure Veri Gölü Deposunda (datalake/çıktı klasörü) depolanır. 

```json
{
    "name": "EventsByRegionTable",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="sample-data-lake-store-linked-service"></a>Örnek Veri Gölü Deposu Bağlantılı Servis
Burada, giriş/çıktı veri kümeleri tarafından kullanılan örnek Azure Veri Gölü Deposu bağlantılı hizmetin tanımı verilmiştir. 

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
        }
    }
}
```

JSON özelliklerinin açıklamaları için verileri Azure Veri Gölü Deposu makalesine [taşıyın](data-factory-azure-datalake-connector.md) ve buradan bakın. 

## <a name="sample-u-sql-script"></a>Örnek U-SQL Script

```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
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

U-SQL komut dosyasındaki ** \@in** ve ** \@çıkış** parametrelerinin değerleri ADF tarafından 'parametreler' bölümü kullanılarak dinamik olarak geçirilir. Boru hattı tanımındaki 'parametreler' bölümüne bakın.

Azure Veri Gölü Analizi hizmetinde çalışan işler için boru hattı tanımınızda DereceBenzersizliği ve önceliği gibi diğer özellikleri de belirtebilirsiniz.

## <a name="dynamic-parameters"></a>Dinamik parametreler
Örnek boru hattı tanımında, içi ve dışı parametreler sabit kodlu değerlerle atanır. 

```json
"parameters": {
    "in": "/datalake/input/SearchLog.tsv",
    "out": "/datalake/output/Result.tsv"
}
```

Bunun yerine dinamik parametreleri kullanmak mümkündür. Örnek: 

```json
"parameters": {
    "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
    "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
}
```

Bu durumda, giriş dosyaları hala /datalake/input klasöründen alınır ve çıkış dosyaları /datalake/çıktı klasöründe oluşturulur. Dosya adları, dilim başlangıç saatine göre dinamiktir.  


