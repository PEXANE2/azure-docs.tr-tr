---
title: Azure Izleyici Application Insights çalışma alanı tabanlı kaynak şeması
description: Azure Izleyici Application Insights çalışma alanı tabanlı kaynakların yeni tablo yapısı ve şeması hakkında bilgi edinin.
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 05/09/2020
ms.openlocfilehash: 1d7275c928b4d25e200a3a8d3d690c7575c056e7
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87323188"
---
# <a name="workspace-based-resource-changes-preview"></a>Çalışma alanı tabanlı kaynak değişiklikleri (Önizleme)

[Çalışma alanı tabanlı Application Insights kaynaklarının](create-workspace-resource.md)kullanıma sunulmasından önce Application Insights verileri Azure izleyici 'deki diğer günlük verilerinden ayrı olarak depolandı. Her ikisi de Azure Veri Gezgini tabanlıdır ve aynı kusto sorgu dilini (KQL) kullanır. Bu, [Azure izleyici 'de günlüklerde](../platform/data-platform-logs.md)açıklanmıştır.

Çalışma alanı tabanlı Application Insights kaynak verileri, diğer izleme verileri ve uygulama verileriyle birlikte bir Log Analytics çalışma alanında depolanır. Bu, birden çok çözüm genelinde verileri daha kolay analiz etmenize ve çalışma alanlarının özelliklerinden yararlanmasını sağlayarak yapılandırmanızı basitleştirir.

## <a name="table-structure"></a>Tablo yapısı

| Eski tablo adı | Yeni tablo adı | Açıklama |
|:---|:---|:---|
| availabilityResults | AppAvailabilityResults |  Kullanılabilirlik testlerinden özet veriler.|
| Browserzamanlamalar | Appbrowserzamanlamalar | Gelen verileri işlemek için geçen süre gibi istemci performansı hakkındaki veriler.|
| bağımlılıklar | AppDependencies | Uygulamadan, TrackDependency () ile kaydedilen diğer bileşenlere (Dış bileşenler dahil) yapılan çağrılar (örneğin, REST API, veritabanı veya dosya sistemine yapılan çağrılar).  |
| customEvents | AppEvents | Uygulamanız tarafından oluşturulan özel olaylar. |
| customMetrics | Appölçümler | Uygulamanız tarafından oluşturulan özel ölçümler. |
| pageViews | AppPageViews| Tarayıcı bilgileriyle her bir Web sitesi görünümüyle ilgili veriler. |
| performanceCounters | AppPerformanceCounters | Uygulamayı destekleyen işlem kaynaklarından performans ölçümleri, örneğin, Windows performans sayaçları. |
| istekleri | AppRequests | Uygulamanız tarafından alınan istekler. Örneğin, Web uygulamanızın aldığı her HTTP isteği için ayrı bir istek kaydı kaydedilir.  |
| larý | AppSystemEvents | Uygulama çalışma zamanı tarafından oluşturulan özel durumlar, hem sunucu tarafı hem de istemci tarafı (tarayıcı) özel durumlarını yakalar. |
| lerin | Appizlemeler | TrackTrace () ile kaydedilen uygulama kodu/günlük çerçeveleri aracılığıyla yayılan ayrıntılı Günlükler (izlemeler). |

## <a name="table-schemas"></a>Tablo şemaları

Aşağıdaki bölümlerde, klasik Özellik adları ve yeni çalışma alanı tabanlı Application Insights Özellik adları arasındaki eşleme gösterilmektedir.  Eski tabloları kullanarak herhangi bir sorguyu dönüştürmek için bu bilgileri kullanın.

Sütunların çoğu, farklı bir büyük harfle aynı ada sahiptir. KQL büyük/küçük harfe duyarlı olduğundan, her bir sütun adını, varolan sorgularda tablo adlarıyla birlikte değiştirmeniz gerekir. Büyük/küçük harfe ek olarak değişiklik içeren sütunlar vurgulanır. Klasik Application Insights sorgularınızı, çalışma alanı tabanlı bir kaynak olsa bile, Application Insights kaynağınızın **Günlükler** bölmesinde kullanmaya devam edebilirsiniz. Log Analytics çalışma alanı deneyimi bağlamı içinden sorgulama yaparken yeni özellik adları gereklidir.

### <a name="appavailabilityresults"></a>AppAvailabilityResults

Eski tablo: kullanılabilirlik

|ApplicationInsights|Tür|Günlüğe kaydetme Analizi|Tür|
|:---|:---|:---|:---|
|appId|dize|\_ResourceGUID|dize|
|application_Version|dize|AppVersion|dize|
|appName|dize|\_ResourceId|dize|
|client_Browser|dize|ClientBrowser|dize|
|client_City|dize|ClientCity|dize|
|client_CountryOrRegion|dize|ClientCountryOrRegion|dize|
|client_IP|dize|ClientIP|dize|
|client_Model|dize|ClientModel|dize|
|client_OS|dize|ClientOS|dize|
|client_StateOrProvince|dize|Clientstateoril|dize|
|client_Type|dize|ClientType|dize|
|cloud_RoleInstance|dize|Approtaınstance|dize|
|cloud_RoleName|dize|Approtaname|dize|
|customDimensions|dinamik|Özellikler|Dinamik|
|Customölçüler|dinamik|Ölçümler|Dinamik|
|süre|real|DurationMs|real|
|`id`|string|`Id`|dize|
|iKey|dize|IKey|dize|
|ItemCount|int|ItemCount|int|
|ID|dize|\_ID|dize|
|ItemType|dize|Tür|Dize|
|location|dize|Konum|dize|
|message|dize|İleti|dize|
|name|dize|Ad|dize|
|operation_Id|dize|OperationId|dize|
|operation_Name|dize|OperationName|dize|
|operation_ParentId|dize|Operationparentıd|dize|
|operation_SyntheticSource|dize|OperationSyntheticSource|dize|
|performanceBucket|dize|PerformanceBucket|dize|
|SDK sürümü|dize|SdkVersion|dize|
|session_Id|dize|SessionId|dize|
|size|real|Boyut|real|
|başarılı|dize|Başarılı|Bool|
|timestamp|datetime|TimeGenerated|datetime|
|user_AccountId|dize|Useraccountıd|dize|
|user_AuthenticatedId|dize|Userauthenticatedıd|dize|
|user_Id|dize|UserId|dize|

### <a name="appbrowsertimings"></a>Appbrowserzamanlamalar

Eski tablo: Browserzamanlamalar

|ApplicationInsights|Tür|Günlüğe kaydetme Analizi|Tür|
|:---|:---|:---|:---|
|appId|dize|\_ResourceGUID|dize|
|application_Version|dize|AppVersion|dize|
|appName|dize|\_ResourceId|dize|
|client_Browser|dize|ClientBrowser|dize|
|client_City|dize|ClientCity|dize|
|client_CountryOrRegion|dize|ClientCountryOrRegion|dize|
|client_IP|dize|ClientIP|dize|
|client_Model|dize|ClientModel|dize|
|client_OS|dize|ClientOS|dize|
|client_StateOrProvince|dize|Clientstateoril|dize|
|client_Type|dize|ClientType|dize|
|cloud_RoleInstance|dize|Approtaınstance|dize|
|cloud_RoleName|dize|Approtaname|dize|
|customDimensions|dinamik|Özellikler|Dinamik|
|Customölçüler|dinamik|Ölçümler|Dinamik|
|iKey|dize|IKey|dize|
|ItemCount|int|ItemCount|int|
|ID|dize|\_ID|dize|
|ItemType|dize|Tür|string|
|name|dize|Ad|datetime|
|networkDuration|real|NetworkDurationMs|real|
|operation_Id|dize|OperationId|dize|
|operation_Name|dize|OperationName|dize|
|operation_ParentId|dize|Operationparentıd|dize|
|operation_SyntheticSource|dize|OperationSyntheticSource|dize|
|performanceBucket|dize|PerformanceBucket|dize|
|processingDuration|real|ProcessingDurationMs|real|
|receiveDuration|real|ReceiveDurationMs|real|
|SDK sürümü|dize|SdkVersion|dize|
|sendDuration|real|SendDurationMs|real|
|session_Id|dize|SessionId|dize|
|timestamp|datetime|TimeGenerated|datetime|
|Toplam süre|real|TotalDurationMs|real|
|url|dize|Url|dize|
|user_AccountId|dize|Useraccountıd|dize|
|user_AuthenticatedId|dize|Userauthenticatedıd|dize|
|user_Id|dize|UserId|dize|

### <a name="appdependencies"></a>AppDependencies

Eski tablo: bağımlılıklar

|ApplicationInsights|Tür|Günlüğe kaydetme Analizi|Tür|
|:---|:---|:---|:---|
|appId|dize|\_ResourceGUID|dize|
|application_Version|dize|AppVersion|dize|
|appName|dize|\_ResourceId|dize|
|client_Browser|dize|ClientBrowser|dize|
|client_City|dize|ClientCity|dize|
|client_CountryOrRegion|dize|ClientCountryOrRegion|dize|
|client_IP|dize|ClientIP|dize|
|client_Model|dize|ClientModel|dize|
|client_OS|dize|ClientOS|dize|
|client_StateOrProvince|dize|Clientstateoril|dize|
|client_Type|dize|ClientType|dize|
|cloud_RoleInstance|dize|Approtaınstance|dize|
|cloud_RoleName|dize|Approtaname|dize|
|customDimensions|dinamik|Özellikler|Dinamik|
|Customölçüler|dinamik|Ölçümler|Dinamik|
|veriler|dize|Veriler|dize|
|süre|real|DurationMs|real|
|`id`|string|`Id`|dize|
|iKey|dize|IKey|dize|
|ItemCount|int|ItemCount|int|
|ID|dize|\_ID|dize|
|ItemType|dize|Tür|Dize|
|name|dize|Ad|dize|
|operation_Id|dize|OperationId|dize|
|operation_Name|dize|OperationName|dize|
|operation_ParentId|dize|Operationparentıd|dize|
|operation_SyntheticSource|dize|OperationSyntheticSource|dize|
|performanceBucket|dize|PerformanceBucket|dize|
|resultCode|dize|ResultCode|dize|
|SDK sürümü|dize|SdkVersion|dize|
|session_Id|dize|SessionId|dize|
|başarılı|dize|Başarılı|Bool|
|hedef|dize|Hedef|dize|
|timestamp|datetime|TimeGenerated|datetime|
|tür|dize|DependencyType|dize|
|user_AccountId|dize|Useraccountıd|dize|
|user_AuthenticatedId|dize|Userauthenticatedıd|dize|
|user_Id|dize|UserId|dize|

### <a name="appevents"></a>AppEvents

Eski tablo: customEvents

|ApplicationInsights|Tür|Günlüğe kaydetme Analizi|Tür|
|:---|:---|:---|:---|
|appId|dize|\_ResourceGUID|dize|
|application_Version|dize|AppVersion|dize|
|appName|dize|\_ResourceId|dize|
|client_Browser|dize|ClientBrowser|dize|
|client_City|dize|ClientCity|dize|
|client_CountryOrRegion|dize|ClientCountryOrRegion|dize|
|client_IP|dize|ClientIP|dize|
|client_Model|dize|ClientModel|dize|
|client_OS|dize|ClientOS|dize|
|client_StateOrProvince|dize|Clientstateoril|dize|
|client_Type|dize|ClientType|dize|
|cloud_RoleInstance|dize|Approtaınstance|dize|
|cloud_RoleName|dize|Approtaname|dize|
|customDimensions|dinamik|Özellikler|Dinamik|
|Customölçüler|dinamik|Ölçümler|Dinamik|
|iKey|dize|IKey|dize|
|ItemCount|int|ItemCount|int|
|ID|dize|\_ID|dize|
|ItemType|dize|Tür|string|
|name|dize|Ad|dize|
|operation_Id|dize|OperationId|dize|
|operation_Name|dize|OperationName|dize|
|operation_ParentId|dize|Operationparentıd|dize|
|operation_SyntheticSource|dize|OperationSyntheticSource|dize|
|SDK sürümü|dize|SdkVersion|dize|
|session_Id|dize|SessionId|dize|
|timestamp|datetime|TimeGenerated|datetime|
|user_AccountId|dize|Useraccountıd|dize|
|user_AuthenticatedId|dize|Userauthenticatedıd|dize|
|user_Id|dize|UserId|dize|

### <a name="appmetrics"></a>Appölçümler

Eski tablo: Customölçümler

|ApplicationInsights|Tür|Günlüğe kaydetme Analizi|Tür|
|:---|:---|:---|:---|
|appId|dize|\_ResourceGUID|dize|
|application_Version|dize|AppVersion|dize|
|appName|dize|\_ResourceId|dize|
|client_Browser|dize|ClientBrowser|dize|
|client_City|dize|ClientCity|dize|
|client_CountryOrRegion|dize|ClientCountryOrRegion|dize|
|client_IP|dize|ClientIP|dize|
|client_Model|dize|ClientModel|dize|
|client_OS|dize|ClientOS|dize|
|client_StateOrProvince|dize|Clientstateoril|dize|
|client_Type|dize|ClientType|dize|
|cloud_RoleInstance|dize|Approtaınstance|dize|
|cloud_RoleName|dize|Approtaname|dize|
|customDimensions|dinamik|Özellikler|Dinamik|
|iKey|dize|IKey|dize|
|ID|dize|\_ID|dize|
|ItemType|dize|Tür|string|
|name|dize|Ad|dize|
|operation_Id|dize|OperationId|dize|
|operation_Name|dize|OperationName|dize|
|operation_ParentId|dize|Operationparentıd|dize|
|operation_SyntheticSource|dize|OperationSyntheticSource|dize|
|SDK sürümü|dize|SdkVersion|dize|
|session_Id|dize|SessionId|dize|
|timestamp|datetime|TimeGenerated|datetime|
|user_AccountId|dize|Useraccountıd|dize|
|user_AuthenticatedId|dize|Userauthenticatedıd|dize|
|user_Id|dize|UserId|dize|
|değer|real|kaldırıldı||
|valueCount|int|ValueCount|int|
|valueMax|real|ValueMax|real|
|valueMin|real|ValueMin|real|
|valueStdDev|real|ValueStdDev|real|
|valueSum|real|ValueSum|real|

### <a name="apppageviews"></a>AppPageViews

Eski tablo: pageViews

|ApplicationInsights|Tür|Günlüğe kaydetme Analizi|Tür|
|:---|:---|:---|:---|
|appId|dize|\_ResourceGUID|dize|
|application_Version|dize|AppVersion|dize|
|appName|dize|\_ResourceId|dize|
|client_Browser|dize|ClientBrowser|dize|
|client_City|dize|ClientCity|dize|
|client_CountryOrRegion|dize|ClientCountryOrRegion|dize|
|client_IP|dize|ClientIP|dize|
|client_Model|dize|ClientModel|dize|
|client_OS|dize|ClientOS|dize|
|client_StateOrProvince|dize|Clientstateoril|dize|
|client_Type|dize|ClientType|dize|
|cloud_RoleInstance|dize|Approtaınstance|dize|
|cloud_RoleName|dize|Approtaname|dize|
|customDimensions|dinamik|Özellikler|Dinamik|
|Customölçüler|dinamik|Ölçümler|Dinamik|
|süre|real|DurationMs|real|
|`id`|string|`Id`|dize|
|iKey|dize|IKey|dize|
|ItemCount|int|ItemCount|int|
|ID|dize|\_ID|dize|
|ItemType|dize|Tür|Dize|
|name|dize|Ad|dize|
|operation_Id|dize|OperationId|dize|
|operation_Name|dize|OperationName|dize|
|operation_ParentId|dize|Operationparentıd|dize|
|operation_SyntheticSource|dize|OperationSyntheticSource|dize|
|performanceBucket|dize|PerformanceBucket|dize|
|SDK sürümü|dize|SdkVersion|dize|
|session_Id|dize|SessionId|dize|
|timestamp|datetime|TimeGenerated|datetime|
|url|dize|Url|dize|
|user_AccountId|dize|Useraccountıd|dize|
|user_AuthenticatedId|dize|Userauthenticatedıd|dize|
|user_Id|dize|UserId|dize|

### <a name="appperformancecounters"></a>AppPerformanceCounters

Eski tablo: performanceCounters

|ApplicationInsights|Tür|Günlüğe kaydetme Analizi|Tür|
|:---|:---|:---|:---|
|appId|dize|\_ResourceGUID|dize|
|application_Version|dize|AppVersion|dize|
|appName|dize|\_ResourceId|dize|
|category|dize|Kategori|dize|
|client_Browser|dize|ClientBrowser|dize|
|client_City|dize|ClientCity|dize|
|client_CountryOrRegion|dize|ClientCountryOrRegion|dize|
|client_IP|dize|ClientIP|dize|
|client_Model|dize|ClientModel|dize|
|client_OS|dize|ClientOS|dize|
|client_StateOrProvince|dize|Clientstateoril|dize|
|client_Type|dize|ClientType|dize|
|cloud_RoleInstance|dize|Approtaınstance|dize|
|cloud_RoleName|dize|Approtaname|dize|
|counter|dize|kaldırıldı||
|customDimensions|dinamik|Özellikler|Dinamik|
|iKey|dize|IKey|dize|
|örnek|dize|Örnek|dize|
|ID|dize|\_ID|dize|
|ItemType|dize|Tür|string|
|name|dize|Ad|dize|
|operation_Id|dize|OperationId|dize|
|operation_Name|dize|OperationName|dize|
|operation_ParentId|dize|Operationparentıd|dize|
|operation_SyntheticSource|dize|OperationSyntheticSource|dize|
|SDK sürümü|dize|SdkVersion|dize|
|session_Id|dize|SessionId|dize|
|timestamp|datetime|TimeGenerated|datetime|
|user_AccountId|dize|Useraccountıd|dize|
|user_AuthenticatedId|dize|Userauthenticatedıd|dize|
|user_Id|dize|UserId|dize|
|değer|real|Değer|real|

### <a name="apprequests"></a>AppRequests

Eski tablo: istekler

|ApplicationInsights|Tür|Günlüğe kaydetme Analizi|Tür|
|:---|:---|:---|:---|
|appId|dize|\_ResourceGUID|dize|
|application_Version|dize|AppVersion|dize|
|appName|dize|\_ResourceId|dize|
|client_Browser|dize|ClientBrowser|dize|
|client_City|dize|ClientCity|dize|
|client_CountryOrRegion|dize|ClientCountryOrRegion|dize|
|client_IP|dize|ClientIP|dize|
|client_Model|dize|ClientModel|dize|
|client_OS|dize|ClientOS|dize|
|client_StateOrProvince|dize|Clientstateoril|dize|
|client_Type|dize|ClientType|dize|
|cloud_RoleInstance|dize|Approtaınstance|dize|
|cloud_RoleName|dize|Approtaname|dize|
|customDimensions|dinamik|Özellikler|Dinamik|
|Customölçüler|dinamik|Ölçümler|Dinamik|
|süre|real|DurationMs|Gerçek|
|`id`|dize|`Id`|Dize|
|iKey|dize|IKey|dize|
|ItemCount|int|ItemCount|int|
|ID|dize|\_ID|dize|
|ItemType|dize|Tür|Dize|
|name|dize|Ad|Dize|
|operation_Id|dize|OperationId|dize|
|operation_Name|dize|OperationName|dize|
|operation_ParentId|dize|Operationparentıd|dize|
|operation_SyntheticSource|dize|OperationSyntheticSource|dize|
|performanceBucket|dize|PerformanceBucket|Dize|
|resultCode|dize|ResultCode|Dize|
|SDK sürümü|dize|SdkVersion|dize|
|session_Id|dize|SessionId|dize|
|kaynak|dize|Kaynak|Dize|
|başarılı|dize|Başarılı|Bool|
|timestamp|datetime|TimeGenerated|datetime|
|url|dize|Url|Dize|
|user_AccountId|dize|Useraccountıd|dize|
|user_AuthenticatedId|dize|Userauthenticatedıd|dize|
|user_Id|dize|UserId|dize|

### <a name="appsystemevents"></a>AppSystemEvents

Eski tablo: özel durumlar

|ApplicationInsights|Tür|Günlüğe kaydetme Analizi|Tür|
|:---|:---|:---|:---|
|appId|dize|\_ResourceGUID|dize|
|application_Version|dize|AppVersion|dize|
|appName|dize|\_ResourceId|dize|
|derleme|dize|Bütünleştirilmiş Kod|dize|
|client_Browser|dize|ClientBrowser|dize|
|client_City|dize|ClientCity|dize|
|client_CountryOrRegion|dize|ClientCountryOrRegion|dize|
|client_IP|dize|ClientIP|dize|
|client_Model|dize|ClientModel|dize|
|client_OS|dize|ClientOS|dize|
|client_StateOrProvince|dize|Clientstateoril|dize|
|client_Type|dize|ClientType|dize|
|cloud_RoleInstance|dize|Approtaınstance|dize|
|cloud_RoleName|dize|Approtaname|dize|
|customDimensions|dinamik|Özellikler|dinamik|
|Customölçüler|dinamik|Ölçümler|dinamik|
|bilgileri|dinamik|Ayrıntılar|dinamik|
|handledAt|dize|HandledAt|dize|
|iKey|dize|IKey|dize|
|ınnermostassembly|dize|Innermostassembly|dize|
|ınnermostmessage|dize|Innermostmessage|dize|
|ınnermostmethod|dize|Innermostmethod|dize|
|ınnermosttype|dize|Innermosttype|dize|
|ItemCount|int|ItemCount|int|
|ID|dize|\_ID|dize|
|ItemType|dize|Tür|string|
|message|dize|İleti|dize|
|method|dize|Yöntem|dize|
|operation_Id|dize|OperationId|dize|
|operation_Name|dize|OperationName|dize|
|operation_ParentId|dize|Operationparentıd|dize|
|operation_SyntheticSource|dize|OperationSyntheticSource|dize|
|outerAssembly|dize|OuterAssembly|dize|
|outerMessage|dize|OuterMessage|dize|
|outerMethod|dize|OuterMethod|dize|
|outerType|dize|OuterType|dize|
|problemId|dize|ProblemId|dize|
|SDK sürümü|dize|SdkVersion|dize|
|session_Id|dize|SessionId|dize|
|Severıtylevel|int|Severıtylevel|int|
|timestamp|datetime|TimeGenerated|datetime|
|tür|dize|Türü|dize|
|user_AccountId|dize|Useraccountıd|dize|
|user_AuthenticatedId|dize|Userauthenticatedıd|dize|
|user_Id|dize|UserId|dize|

### <a name="apptraces"></a>Appizlemeler

Eski tablo: izlemeler

|ApplicationInsights|Tür|Günlüğe kaydetme Analizi|Tür|
|:---|:---|:---|:---|
|appId|dize|\_ResourceGUID|dize|
|application_Version|dize|AppVersion|dize|
|appName|dize|\_ResourceId|dize|
|client_Browser|dize|ClientBrowser|dize|
|client_City|dize|ClientCity|dize|
|client_CountryOrRegion|dize|ClientCountryOrRegion|dize|
|client_IP|dize|ClientIP|dize|
|client_Model|dize|ClientModel|dize|
|client_OS|dize|ClientOS|dize|
|client_StateOrProvince|dize|Clientstateoril|dize|
|client_Type|dize|ClientType|dize|
|cloud_RoleInstance|dize|Approtaınstance|dize|
|cloud_RoleName|dize|Approtaname|dize|
|customDimensions|dinamik|Özellikler|dinamik|
|Customölçüler|dinamik|Ölçümler|dinamik|
|iKey|dize|IKey|dize|
|ItemCount|int|ItemCount|int|
|ID|dize|\_ID|dize|
|ItemType|dize|Tür|string|
|message|dize|İleti|dize|
|operation_Id|dize|OperationId|dize|
|operation_Name|dize|OperationName|dize|
|operation_ParentId|dize|Operationparentıd|dize|
|operation_SyntheticSource|dize|OperationSyntheticSource|dize|
|SDK sürümü|dize|SdkVersion|dize|
|session_Id|dize|SessionId|dize|
|Severıtylevel|int|Severıtylevel|int|
|timestamp|datetime|TimeGenerated|datetime|
|user_AccountId|dize|Useraccountıd|dize|
|user_AuthenticatedId|dize|Userauthenticatedıd|dize|
|user_Id|dize|UserId|dize|

## <a name="next-steps"></a>Sonraki adımlar

* [Ölçümleri keşfetme](../platform/metrics-charts.md)
* [Analytics sorguları yazma](../log-query/log-query-overview.md)

