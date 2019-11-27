---
title: Sorun giderme Azure Data Factory | Microsoft Docs
description: Azure Data Factory 'de dış denetim etkinliklerinin nasıl giderileceğini öğrenin.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 8/26/2019
ms.author: abnarain
ms.reviewer: craigg
ms.openlocfilehash: 0026aa377a58f6b766a400860692a35440deb962
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748366"
---
# <a name="troubleshoot-azure-data-factory"></a>Azure Data Factory sorunlarını giderme

Bu makalede Azure Data Factory içindeki dış denetim etkinlikleri için genel sorun giderme yöntemleri incelenmektedir.

## <a name="connector-and-copy-activity"></a>Bağlayıcı ve kopyalama etkinliği

Bağlayıcı sorunları için ör. kopyalama etkinliğini kullanarak hatayla karşılaşırsanız, [Azure Data Factory bağlayıcılarına sorun giderme](connector-troubleshoot-guide.md)bölümüne bakın.
  

## <a name="azure-databricks"></a>Azure Databricks

### <a name="error-code--3200"></a>Hata kodu: 3200

- **İleti**: hata 403.

- **Neden**: `The Databricks access token has expired.`

- **Öneri**: varsayılan olarak, Azure Databricks erişim belirteci 90 gün için geçerlidir. Yeni bir belirteç oluşturun ve bağlı hizmeti güncelleştirin.


### <a name="error-code--3201"></a>Hata kodu: 3201

- **İleti**: `Missing required field: settings.task.notebook_task.notebook_path.`

- **Neden**: `Bad authoring: Notebook path not specified correctly.`

- **Öneri**: Databricks etkinliğinde Not defteri yolunu belirtin.

<br/>    
              
- **İleti**: `Cluster   ... does not exist.`

- **Neden**: `Authoring error: Databricks cluster does not exist or has been deleted.`

- **Öneri**: Databricks kümesinin var olduğunu doğrulayın.

<br/>  

- **İleti**: `Invalid Python file URI.... Please visit Databricks user guide for supported URI schemes.`

- **Neden**: `Bad authoring.`

- **Öneri**: çalışma alanı adresleme şemaları için mutlak yollar ya da Databricks dosya sisteminde depolanan dosyalar için `dbfs:/folder/subfolder/foo.py` belirtin.

<br/>  

- **İleti**: `{0} LinkedService should have domain and accessToken as required properties.`

- **Neden**: `Bad authoring.`

- **Öneri**: [bağlı hizmet tanımını](compute-linked-services.md#azure-databricks-linked-service)doğrulayın.

<br/>  

- **İleti**: `{0} LinkedService should specify either existing cluster ID or new cluster information for creation.`

- **Neden**: `Bad authoring.`

- **Öneri**: [bağlı hizmet tanımını](compute-linked-services.md#azure-databricks-linked-service)doğrulayın.

<br/>  

- **İleti**: `Node type Standard_D16S_v3 is not supported. Supported node types:   Standard_DS3_v2, Standard_DS4_v2, Standard_DS5_v2, Standard_D8s_v3,   Standard_D16s_v3, Standard_D32s_v3, Standard_D64s_v3, Standard_D3_v2,   Standard_D8_v3, Standard_D16_v3, Standard_D32_v3, Standard_D64_v3,   Standard_D12_v2, Standard_D13_v2, Standard_D14_v2, Standard_D15_v2,   Standard_DS12_v2, Standard_DS13_v2, Standard_DS14_v2, Standard_DS15_v2,   Standard_E8s_v3, Standard_E16s_v3, Standard_E32s_v3, Standard_E64s_v3,   Standard_L4s, Standard_L8s, Standard_L16s, Standard_L32s, Standard_F4s,   Standard_F8s, Standard_F16s, Standard_H16, Standard_F4s_v2, Standard_F8s_v2,   Standard_F16s_v2, Standard_F32s_v2, Standard_F64s_v2, Standard_F72s_v2,   Standard_NC12, Standard_NC24, Standard_NC6s_v3, Standard_NC12s_v3,   Standard_NC24s_v3, Standard_L8s_v2, Standard_L16s_v2, Standard_L32s_v2,   Standard_L64s_v2, Standard_L80s_v2.`

- **Neden**: `Bad authoring.`

- **Öneri**: hata iletisine bakın.

<br/>

### <a name="error-code--3202"></a>Hata kodu: 3202

- **İleti**: `There were already 1000 jobs created in past 3600 seconds, exceeding rate limit:   1000 job creations per 3600 seconds.`

- **Neden**: `Too many Databricks runs in an hour.`

- **Öneri**: Bu Databricks çalışma alanını kullanan tüm işlem hatlarını iş oluşturma oranı için denetleyin.  İşlem hatları çok fazla Databricks çalıştırıyorsa, bazı işlem hatlarını yeni bir çalışma alanına geçirin.

<br/>  

- **İleti**: `Could not parse request object: Expected 'key' and 'value' to be set for JSON map field base_parameters, got 'key: "..."' instead.`

- **Neden**: `Authoring error: No value provided for the parameter.`

- **Öneri**: JSON işlem hattını inceleyin ve baseparameters not defterindeki tüm parametrelerin boş olmayan bir değer belirttiğinden emin olun.

<br/>  

- **İleti**: `User: `SimpleUserContext {UserID =..., name =user@company.com, OrgID =...}` is not   authorized to access cluster.`

- **Neden**: erişim belirtecini oluşturan kullanıcının bağlı hizmette belirtilen Databricks kümesine erişmesine izin verilmiyor.

- **Öneri**: kullanıcının çalışma alanında gerekli izinlere sahip olduğundan emin olun.


### <a name="error-code--3203"></a>Hata kodu: 3203

- **İleti**: `The cluster is in Terminated state, not available to receive jobs. Please fix the cluster or retry later.`

- **Neden**: küme sonlandırıldı. Etkileşimli kümeler için bu bir yarış durumu olabilir.

- **Öneri**: Bu hatayı önlemenin en iyi yolu iş kümelerini kullanmaktır.


### <a name="error-code--3204"></a>Hata kodu: 3204

- **İleti**: `Job execution failed.`

- **Neden**: hata iletileri beklenmeyen küme durumu veya belirli bir etkinlik gibi çeşitli sorunları gösterir. Çoğu zaman hiç bir hata iletisi görüntülenmez.

- **Öneri**: yok
            

## <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics

Aşağıdaki tablo U-SQL için geçerlidir.
      
### <a name="error-code--2709"></a>Hata kodu: 2709

- **İleti**: `The access token is from the wrong tenant.`

- **Neden**: yanlış Azure Active Directory (Azure AD) kiracısı.

- **Öneri**: yanlış Azure Active Directory (Azure AD) kiracısı.

<br/>

- **İleti**: `We cannot accept your job at this moment. The maximum number of queued jobs for   your account is 200. `

- **Neden**: Bu hata, Data Lake Analytics azaltma nedeniyle oluşur.

- **Öneri**: Data Factory Tetikleyicileri ve etkinliklerdeki eşzamanlılık ayarlarını değiştirerek Data Lake Analytics gönderilen işlerin sayısını azaltın. Veya Data Lake Analytics limitlerini artırın.

<br/>  

- **İleti**: `This job was rejected because it requires 24 AUs. This account's administrator-defined policy prevents a job from using more than 5 AUs.`

- **Neden**: Bu hata, Data Lake Analytics azaltma nedeniyle oluşur.

- **Öneri**: Data Factory Tetikleyicileri ve etkinliklerdeki eşzamanlılık ayarlarını değiştirerek Data Lake Analytics gönderilen işlerin sayısını azaltın. Veya Data Lake Analytics limitlerini artırın.


### <a name="error-code--2705"></a>Hata kodu: 2705

- **İleti**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/>  <br/>  User is   not able to access Data Lake Store.  <br/>  <br/>  User is  not authorized to use Data Lake Analytics.`

- **Neden**: hizmet sorumlusu veya sertifikasının depolama alanındaki dosyaya erişimi yok.

- **Öneri**: kullanıcının Data Lake Analytics işlerin Data Lake Analytics hesabına ve kök klasörden varsayılan Data Lake Storage örneğine erişimi olduğundan emin olun.


### <a name="error-code--2711"></a>Hata kodu: 2711

- **İleti**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/>  <br/>  User is   not able to access Data Lake Store.  <br/>  <br/>  User is  not authorized to use Data Lake Analytics.`

- **Neden**: hizmet sorumlusu veya sertifikasının depolama alanındaki dosyaya erişimi yok.

- **Öneri**: kullanıcının Data Lake Analytics işlerin Data Lake Analytics hesabına ve kök klasörden varsayılan Data Lake Storage örneğine erişimi olduğundan emin olun.

<br/>  

- **İleti**: `Cannot find the 'Azure Data Lake Store' file or folder.`

- **Neden**: U-SQL dosyasının yolu yanlış veya bağlı hizmet kimlik bilgilerinin erişimi yok.

- **Öneri**: bağlı hizmette belirtilen yolu ve kimlik bilgilerini doğrulayın.


### <a name="error-code--2704"></a>Hata kodu: 2704

- **İleti**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/>  <br/>  User is   not able to access Data Lake Store.  <br/>  <br/>  User is  not authorized to use Data Lake Analytics.`

- **Neden**: hizmet sorumlusu veya sertifikasının depolama alanındaki dosyaya erişimi yok.

- **Öneri**: kullanıcının Data Lake Analytics işlerin Data Lake Analytics hesabına ve kök klasörden varsayılan Data Lake Storage örneğine erişimi olduğundan emin olun.


### <a name="error-code--2707"></a>Hata kodu: 2707

- **İleti**: `Cannot resolve the account of AzureDataLakeAnalytics. Please check 'AccountName' and   'DataLakeAnalyticsUri'.`

- **Neden**: bağlı hizmette Data Lake Analytics hesabı yanlış.

- **Öneri**: doğru hesabın sağlandığını doğrulayın.


### <a name="error-code--2703"></a>Hata kodu: 2703

- **İleti**: `Error Id: E_CQO_SYSTEM_INTERNAL_ERROR (or any error that starts with "Error   Id:").`

- **Neden**: hata Data Lake Analytics.

- **Öneri**: örnek gibi bir hata, işin Data Lake Analytics gönderildiği ve betiği başarısız olduğu anlamına gelir. Data Lake Analytics araştırın. Portalda Data Lake Analytics hesabına gidin ve Data Factory etkinlik çalıştırma KIMLIĞINI (işlem hattı çalıştırma KIMLIĞI değil) kullanarak işi arayın. İş, hata hakkında daha fazla bilgi sağlar ve sorun gidermenize yardımcı olur. Çözüm açık değilse, Data Lake Analytics destek ekibine başvurun ve hesap adınızı ve iş KIMLIĞINI içeren iş URL 'sini sağlayın.
          

## <a name="azure-functions"></a>Azure functions (Azure işlevleri)

### <a name="error-code--3602"></a>Hata kodu: 3602

- **İleti**: `Invalid HttpMethod: '%method;'.`

- **Neden**: etkinlik yükünde belirtilen http yöntemi Azure işlevi etkinliği tarafından desteklenmiyor.

- **Öneri**: desteklenen HTTP yöntemleri put, Post, Get, DELETE, OPTIONS, Head ve Trace ' dir.


### <a name="error-code--3603"></a>Hata kodu: 3603

- **İleti**: `Response Content is not a valid JObject.`

- **Neden**: çağrılan Azure işlevi YANıTTA bir JSON yükü döndürmedi. ADF Azure işlevi etkinliği yalnızca JSON yanıt içeriğini destekler.

- **Öneri**: Azure işlevini GEÇERLI bir JSON yükü döndürecek şekilde güncelleştirin, örn. C# bir işlev (ActionResult) yeni okobjectresult ("{\"ıd\":\"123\"}");


### <a name="error-code--3606"></a>Hata kodu: 3606

- **İleti**: Azure işlevi etkinliğinde işlev anahtarı eksik.

- **Neden**: Azure işlevi etkinlik tanımı tamamlanmadı.

- **Öneri**: lütfen Input AZUREFUNCTION Activity JSON tanımının ' functionkey ' adlı özelliği olduğunu denetleyin.


### <a name="error-code--3607"></a>Hata kodu: 3607

- **İleti**: `Azure function activity missing function name.`

- **Neden**: Azure işlevi etkinlik tanımı tamamlanmadı.

- **Öneri**: lütfen Input AZUREFUNCTION Activity JSON tanımının ' fonksiyonadı ' adlı özelliği olduğunu denetleyin.


### <a name="error-code--3608"></a>Hata kodu: 3608

- **İleti**: `Call to provided Azure function '%FunctionName;' failed with status-'%statusCode;' and message - '%message;'.`

- **Neden**: etkinlik tanımındaki Azure işlevi ayrıntıları hatalı olabilir.

- **Öneri**: Azure işlev ayrıntılarını düzeltip yeniden deneyin.


### <a name="error-code--3609"></a>Hata kodu: 3609

- **İleti**: `Azure function activity missing functionAppUrl.`

- **Neden**: Azure işlevi etkinlik tanımı tamamlanmadı.

- **Öneri**: lütfen Input AZUREFUNCTION Activity JSON tanımının ' functionAppUrl ' adlı özelliği olduğunu denetleyin.


### <a name="error-code--3610"></a>Hata kodu: 3610

- **İleti**: `There was an error while calling endpoint.`

- **Neden**: Işlev URL 'si yanlış olabilir.

- **Öneri**: lütfen JSON etkinliğinde ' functionAppUrl ' değerinin doğru olduğundan emin olun ve yeniden deneyin.


### <a name="error-code--3611"></a>Hata kodu: 3611

- **İleti**: `Azure function activity missing Method in JSON.`

- **Neden**: Azure işlevi etkinlik tanımı tamamlanmadı.

- **Öneri**: lütfen Input AZUREFUNCTION Activity JSON tanımının ' Method ' adlı özelliği olduğunu denetleyin.


### <a name="error-code--3612"></a>Hata kodu: 3612

- **İleti**: `Azure function activity missing LinkedService definition in JSON.`

- **Neden**: Azure işlevi etkinlik tanımı tamamlanmadı.

- **Öneri**: lütfen Input AZUREFUNCTION Activity JSON tanımının bağlı hizmet ayrıntılarına sahip olduğunu denetleyin.



## <a name="azure-machine-learning"></a>Azure Machine Learning

### <a name="error-code--4101"></a>Hata kodu: 4101

- **İleti**: `AzureMLExecutePipeline activity '%activityName;' has invalid value for property '%propertyName;'.`

- **Neden**: '% PropertyName; ' özelliğinin hatalı biçimi veya tanımı eksik.

- **Öneri**: lütfen '% ActivityName; ' etkinliğinin doğru verilerle tanımlanmış '% PropertyName; ' özelliğine sahip olup olmadığını denetleyin.


### <a name="error-code--4110"></a>Hata kodu: 4110

- **İleti**: `AzureMLExecutePipeline activity missing LinkedService definition in JSON.`

- **Neden**: AzureMLExecutePipeline etkinlik tanımı tamamlanmadı.

- **Öneri**: lütfen Input AZUREMLEXECUTEPIPELINE Activity JSON tanımının bağlı hizmet ayrıntılarına sahip olup olmadığını denetleyin.


### <a name="error-code--4111"></a>Hata kodu: 4111

- **İleti**: `AzureMLExecutePipeline activity has wrong LinkedService type in JSON. Expected LinkedService type: '%expectedLinkedServiceType;', current LinkedService type: Expected LinkedService type: '%currentLinkedServiceType;'.`

- **Neden**: etkinlik tanımı yanlış.

- **Öneri**: lütfen Input AZUREMLEXECUTEPIPELINE Activity JSON tanımının doğru bağlı hizmet ayrıntılarına sahip olup olmadığını denetleyin.


### <a name="error-code--4112"></a>Hata kodu: 4112

- **İleti**: `AzureMLService linked service has invalid value for property '%propertyName;'.`

- **Neden**: '% PropertyName; ' özelliğinin hatalı biçimi veya tanımı eksik.

- **Öneri**: lütfen bağlı hizmetin doğru verilerle tanımlanmış '% PropertyName; ' özelliğine sahip olup olmadığını denetleyin.


### <a name="error-code--4121"></a>Hata kodu: 4121

- **İleti**: `Request sent to Azure ML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure ML Service: '%externalMessage;'.`

- **Neden**: Azure ML hizmetine erişmek için kullanılan kimlik bilgilerinin süresi doldu.

- **Öneri**: Lütfen kimlik bilgisinin geçerli olduğunu doğrulayın ve yeniden deneyin


### <a name="error-code--4122"></a>Hata kodu: 4122

- **İleti**: `Request sent to Azure ML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure ML Service: '%externalMessage;'.`

- **Neden**: Azure ML hizmeti bağlantılı hizmette belirtilen kimlik bilgileri geçersiz veya işlem için izne sahip değil.

- **Öneri**: lütfen bağlı hizmette kimlik bilgisinin geçerli olduğunu ve Azure ML hizmetine erişim izni olduğunu doğrulayın.


### <a name="error-code--4123"></a>Hata kodu: 4123

- **İleti**: `Request sent to Azure ML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure ML Service: '%externalMessage;'.`

- **Neden**: `Properties of the activity such as pipelineParamters are invalid for the Azure ML pipeline.`

- **Öneri**: Lütfen, bağlı hizmette belirtilen YAYıMLANAN Azure ML işlem hattının beklenen yükünü eşleştirmek için etkinlik özelliklerinin değerini denetleyin.


### <a name="error-code--4124"></a>Hata kodu: 4124

- **İleti**: `Request sent to Azure ML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure ML Service: '%externalMessage;'.`

- **Neden**: Yayınlanan Azure ML ardışık düzen uç noktası yok.

- **Öneri**: lütfen Azure ML hizmeti 'Nde bağlı hizmette belirtilen YAYıMLANAN Azure ML ardışık düzen uç noktasının mevcut olduğunu doğrulayın.


### <a name="error-code--4125"></a>Hata kodu: 4125

- **İleti**: `Request sent to Azure ML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure ML Service: '%externalMessage;'.`

- **Neden**: Azure ML hizmetinde sunucu hatası.

- **Öneri**: lütfen daha sonra yeniden deneyin. Sorun devam ettiğinden yardım için Azure ML hizmeti ekibine başvurun.


### <a name="error-code--4126"></a>Hata kodu: 4126

- **İleti**: `Azure ML pipeline run failed with status: '%amlPipelineRunStatus;'. Azure ML pipeline run Id: '%amlPipelineRunId;'. Please check in Azure ML Service for more error logs.`

- **Neden**: Azure ML işlem hattı çalıştırması başarısız oldu.

- **Öneri**: lütfen daha fazla hata günlüğü IÇIN Azure ML hizmetini DENETLEYIN ve ml ardışık düzenini onarın



## <a name="common"></a>Common

### <a name="error-code--2103"></a>Hata kodu: 2103

- **İleti**: `Please provide value for the required property '%propertyName;'.`

- **Neden**: özelliğin değeri sağlanmadı, ancak senaryoda gereklidir.

- **Öneri**: iletiden değeri girip yeniden deneyin.


### <a name="error-code--2104"></a>Hata kodu: 2104

- **İleti**: `The type of the property '%propertyName;' is incorrect.`

- **Neden**: belirtilen özelliğin türü beklenen şekilde değil.

- **Öneri**: Lütfen özelliğin türünü düzelttikten sonra yeniden deneyin.


### <a name="error-code--2105"></a>Hata kodu: 2105

- **İleti**: `An invalid json is provided for property '%propertyName;'. Encountered an error while trying to parse: '%message;'.`

- **Neden**: özelliğin değeri geçersiz veya beklenen biçime sahip değil.

- **Öneri**: Lütfen özelliğin belgelerini arayın ve girilen değerin biçimi ve türü beklenen biçimde olduğundan emin olun.


### <a name="error-code--2106"></a>Hata kodu: 2106

- **İleti**: `The storage connection string is invalid. %errorMessage;`

- **Neden**: depolama için bağlantı dizesi geçersiz veya biçimi yanlış.

- **Öneri**: lütfen Azure portalına gidin, depolama alanınızı bulun, bağlantı dizesini kopyalayın ve bağlı hizmetinize yapıştırın ve yeniden deneyin.


### <a name="error-code--2108"></a>Hata kodu: 2108

- **İleti**: `Error calling the endpoint '%url;'. Response status code: '%code;'`

- **Neden**: ağ BAĞLANTıSı, DNS hatası, sunucu sertifikası doğrulama veya zaman aşımı gibi temel bir sorun nedeniyle istek başarısız oldu.

- **Öneri**: isteği doğrulamak Için Fiddler/Postman kullanın.


### <a name="error-code--2110"></a>Hata kodu: 2110

- **İleti**: `The linked service type '%linkedServiceType;' is not supported for '%executorType;' activities.`

- **Neden**: etkinlikte belirtilen bağlı hizmet hatalıydı.

- **Öneri**: lütfen bağlı hizmet türünün etkinlik için desteklenen türlerden biri olduğundan emin olun. Örneğin, HDI etkinlikleri için bağlı hizmet türü HDInsight veya Hdınsightondemand olabilir.


### <a name="error-code--2111"></a>Hata kodu: 2111

- **İleti**: `The type of the property '%propertyName;' is incorrect. The expected type is %expectedType;.`

- **Neden**: belirtilen özelliğin türü beklenen şekilde değil.

- **Öneri**: Lütfen özelliğin türünü düzelttikten sonra yeniden deneyin.


### <a name="error-code--2112"></a>Hata kodu: 2112

- **İleti**: `The cloud type is unsupported or could not be determined for storage from the EndpointSuffix '%endpointSuffix;'.`

- **Neden**: bulut türü desteklenmiyor veya endpointsuffix 'dan depolama için belirlenemedi.

- **Öneri**: lütfen depolama alanını başka bir bulutta kullanın ve yeniden deneyin.


### <a name="error-code--2128"></a>Hata kodu: 2128

- **İleti**: `No response from the endpoint. Possible causes: network connectivity, DNS failure, server certificate validation or timeout.`

- **Neden**: ağ BAĞLANTıSı, DNS hatası, sunucu sertifikası doğrulama veya zaman aşımı.

- **Öneri**: vurılmaya çalıştığınız bitiş noktasının isteklere yanıt verdiğini doğrulayın. Fiddler/Postman gibi araçları kullanabilirsiniz.



## <a name="custom"></a>Özel

Aşağıdaki tablo Azure Batch için geçerlidir.
      
### <a name="error-code--2500"></a>Hata kodu: 2500

- **İleti**: `Hit unexpected exception and execution failed.`

- **Neden**: `Can't launch command, or the program returned an error code.`

- **Öneri**: yürütülebilir dosyanın var olduğundan emin olun. Program başlatılmışsa, *stdout. txt* ve *stderr. txt* ' nin depolama hesabına yüklendiğinden emin olun. Hata ayıklama için kodunuzda bir kopan günlüklerini yayma iyi bir uygulamadır.


### <a name="error-code--2501"></a>Hata kodu: 2501

- **İleti**: `Cannot access user batch account; please check batch account settings.`

- **Neden**: Hatalı toplu erişim anahtarı veya havuz adı.

- **Öneri**: bağlı hizmette havuz adını ve Batch erişim anahtarını doğrulayın.


### <a name="error-code--2502"></a>Hata kodu: 2502

- **İleti**: `Cannot access user storage account; please check storage account settings.`

- **Neden**: Hatalı depolama hesabı adı veya erişim anahtarı.

- **Öneri**: depolama hesabı adını ve bağlı hizmette erişim anahtarını doğrulayın.


### <a name="error-code--2504"></a>Hata kodu: 2504

- **İleti**: `Operation returned an invalid status code 'BadRequest'.`

- **Neden**: özel etkinliğin FolderPath öğesinde çok fazla dosya yok. ResourceFiles 'ın toplam boyutu 32.768 karakterden fazla olamaz.

- **Öneri**: gereksiz dosyaları kaldırın. Ya da bunları ayıklayın ve ayıklamak için bir unzip komutu ekleyin. Örneğin, `powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;  $folder\yourProgram.exe` kullanın


### <a name="error-code--2505"></a>Hata kodu: 2505

- **İleti**: `Cannot create Shared Access Signature unless Account Key credentials are used.`

- **Neden**: özel etkinlikler yalnızca bir erişim anahtarı kullanan depolama hesaplarını destekler.

- **Öneri**: hata açıklamasına bakın.


### <a name="error-code--2507"></a>Hata kodu: 2507

- **İleti**: `The folder path does not exist or is empty: ....`

- **Neden**: belirtilen yoldaki depolama hesabında hiçbir dosya yok.

- **Öneri**: klasör yolu çalıştırmak istediğiniz yürütülebilir dosyaları içermelidir.


### <a name="error-code--2508"></a>Hata kodu: 2508

- **İleti**: `There are duplicate files in the resource folder.`

- **Neden**: aynı ada sahip birden çok dosya FolderPath 'in farklı alt klasörlerinde.

- **Öneri**: özel etkinlikler FolderPath altındaki klasör yapısını düzleştirebilir.  Klasör yapısını korumanız gerekiyorsa, dosyaları zip halinde ve bir unzip komutu kullanarak Azure Batch ayıklayın. Örneğin, `powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;   $folder\yourProgram.exe` kullanın


### <a name="error-code--2509"></a>Hata kodu: 2509

- **İleti**: `Batch   url ... is invalid; it must be in Uri format.`

- **Neden**: Batch url 'lerinin `https://mybatchaccount.eastus.batch.azure.com` benzer olması gerekir

- **Öneri**: hata açıklamasına bakın.


### <a name="error-code--2510"></a>Hata kodu: 2510

- **İleti**: `An   error occurred while sending the request.`

- **Neden**: Batch URL 'si geçersiz.

- **Öneri**: Batch URL 'sini doğrulayın.
            

## <a name="hdinsight"></a>HDInsight

### <a name="error-code--200"></a>Hata kodu: 200

- **İleti**: `Unexpected error happened: '%error;'.`

- **Neden**: bir iç hizmet sorunu var.

- **Öneri**: lütfen daha fazla yardım için ADF desteğine başvurun.


### <a name="error-code--201"></a>Hata kodu: 201

- **İleti**: `JobType %jobType; is not found.`

- **Neden**: ADF tarafından desteklenmeyen yeni bir iş türü var.

- **Öneri**: daha fazla yardım IÇIN lütfen ADF destek ekibine başvurun.


### <a name="error-code--202"></a>Hata kodu: 202

- **İleti**: `Failed to create on demand HDI cluster. Cluster name or linked service name: '%clusterName;', error: '%message;'`

- **Neden**: hata iletisinde neyin yanlış olduğuna ilişkin ayrıntılar gösterilmelidir.

- **Öneri**: hata iletisi sorunu gidermeye yardımcı olmalıdır. Yeterli bilgi yoksa, daha fazla yardım için lütfen ADF desteğine başvurun.


### <a name="error-code--203"></a>Hata kodu: 203

- **İleti**: `Failed to delete on demand HDI cluster. Cluster name or linked service name: '%clusterName;', error: '%message;'`

- **Neden**: hata iletisinde neyin yanlış olduğuna ilişkin ayrıntılar gösterilmelidir.

- **Öneri**: hata iletisi sorunu gidermeye yardımcı olmalıdır. Yeterli bilgi yoksa, daha fazla yardım için lütfen ADF desteğine başvurun.


### <a name="error-code--204"></a>Hata kodu: 204

- **İleti**: `The resumption token is missing for runId '%runId;'.`

- **Neden**: bir iç hizmet sorunu var.

- **Öneri**: lütfen daha fazla yardım için ADF desteğine başvurun.


### <a name="error-code--205"></a>Hata kodu: 205

- **İleti**: `Failed to prepare cluster for LinkedService '%linkedServiceName;', the current resource status is '%status;'.`

- **Neden**: HDI isteğe bağlı kümesi oluşturulurken bir hata oluştu.

- **Öneri**: lütfen daha fazla yardım için ADF desteğine başvurun.


### <a name="error-code--206"></a>Hata kodu: 206

- **İleti**: `The batch ID for Spark job is invalid. Please retry your job, and if the problem persists, contact the ADF support for further assistance.`

- **Neden**: Bu soruna neden olan hizmette bir iç sorun oluştu.

- **Öneri**: Bu geçici bir sorun olabilir. Lütfen işinizi yeniden deneyin ve sorun devam ederse, daha fazla yardım için ADF desteği ile iletişime geçin.


### <a name="error-code--207"></a>Hata kodu: 207

- **İleti**: `Could not determine the region from the provided storage account. Please try using another primary storage account for the on demand HDI or contact ADF support team and provide the activity run ID.`

- **Neden**: birincil depolama hesabından bölge belirlemeye çalışırken bir iç hata oluştu.

- **Öneri**: başka bir depolama alanı deneyin. Bunun kabul edilebilir bir çözüm olmaması durumunda daha fazla yardım için ADF destek ekibine başvurun.


### <a name="error-code--208"></a>Hata kodu: 208

- **İleti**: `Service Principal or the MSI authenticator are not instantiated. Please consider providing a Service Principal in the HDI on demand linked service which has permissions to create an HDInsight cluster in the provided subscription and try again. In case if this is not an acceptable solution, contact ADF support team for further assistance.`

- **Neden**: hizmet sorumlusu okunmaya ÇALıŞıLıRKEN veya MSI kimlik doğrulamasının örneği oluşturulurken bir iç hata oluştu.

- **Öneri**: lütfen sağlanan abonelikte HDInsight kümesi oluşturma ve yeniden deneme izinlerine sahip bir hizmet sorumlusu sağlamayı düşünün. Bu, kabul edilebilir bir çözüm değilse, daha fazla yardım için ADF destek ekibine başvurun.


### <a name="error-code--2300"></a>Hata kodu: 2300

- **İleti**: `Failed to submit the job '%jobId;' to the cluster '%cluster;'. Error: %errorMessage;.`

<br>

- **Neden**: hata iletisi, ' uzak ad çözümlenemedi. ' şuna benzer bir ileti içerdiğinde, bu, belirtilen küme URI 'si geçersiz anlamına gelebilir.


- **Öneri**: kümenin silinmediğinden ve belirtilen URI 'nin doğru olduğundan emin olun. URI 'yi bir tarayıcıda açtığınızda, ambarı Kullanıcı arabirimini görmeniz gerekir. Küme bir sanal ağda ise, URI özel URI olmalıdır. Açmak için, aynı sanal ağın parçası olan bir VM kullanın. Daha fazla bilgi için, [bkz.](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment#directly-connect-to-apache-hadoop-services).
                  

<br>

- **Neden**: hata Iletisi ' bir görev iptal edildi. ' öğesine benzer bir ileti içerdiğinde, bu, iş gönderimi zaman aşımına uğradığını gösterir.

- **Öneri**: sorun genel HDInsight bağlantısı ya da ağ bağlantısı olabilir. Önce HDInsight ambarı Kullanıcı arabiriminin herhangi bir tarayıcıdan kullanılabilir olduğunu doğrulayın. Kimlik bilgilerinizin hala geçerli olduğunu doğrulayın. Şirket içinde barındırılan tümleşik çalışma zamanı (IR) kullanıyorsanız bunu, şirket içinde barındırılan IR 'nin yüklü olduğu VM veya makineden gerçekleştirdiğinizden emin olun. Sonra Data Factory işi yeniden göndermeyi deneyin. Hala başarısız olduysa, destek için Data Factory ekibine başvurun.

<br>

- **Neden**: hata iletisi bir ileti içerdiğinde, ' Kullanıcı Yöneticisi, ambarı 'nda kilitli ' veya ' yetkilendirilmemiş: ambarı Kullanıcı adı veya parola yanlış ' olduğunda, HDInsight için kimlik bilgilerinin yanlış veya kullanım dışı olduğu anlamına gelir.

- **Öneri**: kimlik bilgilerini düzeltin ve bağlı hizmeti yeniden dağıtın. Önce, herhangi bir tarayıcıda küme URI 'sini açarak ve oturum açmaya çalışarak, kimlik bilgilerinin HDInsight üzerinde çalıştığından emin olun. Kimlik bilgileri çalışmazsa, bunları Azure portal sıfırlayabilirsiniz.

<br>

- **Neden**: hata iletisi, ' 502-Web sunucusu bir ağ geçidi veya proxy sunucusu görevi gören geçersiz bir yanıt aldı ' ile benzer bir ileti içerdiğinde, bu hata HDInsight hizmeti tarafından döndürülür.


- **Öneri**: Azure HDInsight sorun giderme belgelerine bakın; örneğin, https://hdinsight.github.io/ambari/ambari-ui-502-error.html, https://hdinsight.github.io/spark/spark-thriftserver-errors.htmlhttps://docs.microsoft.com/azure/application-gateway/application-gateway-troubleshooting-502.
                  

<br>

- **Neden**: hata iletisi bir ileti içerdiğinde ' şuna benzer bir ileti içerdiğinde, temptaton hizmeti çok fazla sayıda iş isteği gönderme ' veya ' sıra 500 kökü ile meşgul olduğundan, Bu, aynı anda HDInsight 'a çok fazla iş gönderildiği anlamına gelir.

- **Öneri**: HDInsight 'a gönderilen eşzamanlı iş sayısını sınırlamayı düşünün. İşlerin aynı etkinlik tarafından gönderiliyorsa Data Factory etkinlik eşzamanlılık bölümüne bakın. Yinelenen işlem hattı çalıştırmaları zaman içinde yayma için Tetikleyicileri değiştirin. Hatanın önereceği şekilde temptaton. parallellilik. job. Gönder ' i ayarlamak için HDInsight belgelerine bakın.


### <a name="error-code--2301"></a>Hata kodu: 2301

- **İleti**: `Could not get the status of the application '%physicalJobId;' from the HDInsight service. Received the following error: %message;. Please refer to HDInsight troubleshooting documentation or contact their support for further assistance.`

- **Neden**: HDInsight kümesinde veya hizmetinde sorunlar vardır.


- **Öneri**: Bu hata, ADF, çalışan işin durumunu almaya çalışırken HDInsight kümesinden yanıt alamazsanız oluşur. Küme üzerinde sorunların nedeni olabilir veya HDInsight hizmeti bir kesinti olabilir. Lütfen https://docs.microsoft.com/azure/hdinsight/hdinsight-troubleshoot-guide'de HDInsight sorun giderme belgelerine başvurun veya daha fazla yardım için desteğe başvurun.
                


### <a name="error-code--2302"></a>Hata kodu: 2302

- **İleti**: `Hadoop job failed with exit code '%exitCode;'. See '%logPath;/stderr' for more details. Alternatively, open the Ambari UI on the HDI cluster and find the logs for the job '%jobId;'. Contact HDInsight team for further support.`

- **Neden**: Iş HDI kümesine gönderildi ve burada başarısız oldu.

- **Öneri**: etkinlik çalıştırma çıkışında Yarn günlükleri bağlantısını izleyin ve HDI çıktısındaki hataları arayın. Gerekirse destek için HDInsight ekibine başvurun.


### <a name="error-code--2303"></a>Hata kodu: 2303

- **İleti**: `Hadoop job failed with transient exit code '%exitCode;'. See '%logPath;/stderr' for more details. Alternatively, open the Ambari UI on the HDI cluster and find the logs for the job '%jobId;'. Try again or contact HDInsight team for further support.`

- **Neden**: Iş HDI kümesine gönderildi ve burada başarısız oldu.

- **Öneri**: etkinlik çalıştırma çıkışında Yarn günlükleri bağlantısını izleyin ve HDI çıktısındaki hataları arayın. Yeniden deneyin veya gerekirse destek için HDInsight ekibine başvurun.


### <a name="error-code--2304"></a>Hata kodu: 2304

- **İleti**: `MSI authentication is not supported on storages for HDI activities.`

- **Neden**: HDI bağlı hizmetinde veya HDI etkinliğinde kullanılan depolama bağlı hizmetleri, desteklenmeyen MSI kimlik doğrulaması ile yapılandırılmış.

- **Öneri**: lütfen HDI bağlı hizmeti veya HDI etkinliğinde kullanılan depolama hesapları için tam bağlantı dizeleri sağlayın.


### <a name="error-code--2305"></a>Hata kodu: 2305

- **İleti**: `Failed to initialize the HDInsight client for the cluster '%cluster;'. Error: '%message;'`

- **Neden**: HDI kümesi için bağlantı bilgileri yanlış, belirtilen kullanıcının gerekli eylemi gerçekleştirme izni yok veya HDıNSIGHT hizmeti ADF 'den gelen isteklere yanıt veriyor.

- **Öneri**: lütfen Kullanıcı bilgilerinin doğru olduğundan emin olun. Ayrıca, HDI kümesinin ambarı Kullanıcı arabiriminin, şirket içinde barındırılan IR durumunda IR 'nin yüklendiği VM 'den bir tarayıcıda açılabildiğini veya Azure IR durumda herhangi bir makineden açılabildiğini doğrulayın.


### <a name="error-code--2306"></a>Hata kodu: 2306

- **İleti**: `An invalid json is provided for script action '%scriptActionName;'. Error: '%message;'`

- **Neden**: betik eylemi için belirtilen JSON geçersiz.


- **Öneri**: hata iletisi sorunu tanımaya yardımcı olmalıdır. Lütfen JSON yapılandırmasını düzelttikten sonra yeniden deneyin. Daha fazla bilgi için https://docs.microsoft.com/azure/data-factory/compute-linked-services#azure-hdinsight-on-demand-linked-service denetleyin.
                


### <a name="error-code--2310"></a>Hata kodu: 2310

- **İleti**: `Failed to submit Spark job. Error: '%message;'`

- **Neden**: ADF, LIVY API (Livy/Batch) kullanarak Spark kümesinde toplu işlem oluşturmaya çalıştı, ancak bir hata alındı.

- **Öneri**: sorunu onarmak için lütfen hata iletisini izleyin. Çözümlenmeden ulaşmak için yeterli bilgi yoksa HDI ekibine başvurun ve bu kişilere, ADF Izleme sırasında etkinlik çalıştırma çıktısı sayfasında bulunabileceği toplu iş KIMLIĞI ve iş KIMLIĞI sağlayın.


### <a name="error-code--2312"></a>Hata kodu: 2312

- **İleti**: `Spark job failed, batch id:%batchId;. Please follow the links in the activity run Output from ADF Monitoring page to troubleshoot the run on HDInsight Spark cluster. Please contact HDInsight support team for further assistance.`

- **Neden**: Iş HDInsight Spark kümesinde başarısız oldu.

- **Öneri**: HDInsight Spark kümesinde çalıştırma sorunlarını gidermek IÇIN lütfen ADF izleme sırasında etkinlik çalıştırma çıktısı sayfasında bulunan bağlantıları izleyin. Daha fazla yardım için lütfen HDInsight destek ekibine başvurun.


### <a name="error-code--2313"></a>Hata kodu: 2313

- **İleti**: `The batch with ID '%batchId;' was not found on Spark cluster. Open the Spark History UI and try to find it there. Contact HDInsight support for further assistance.`

- **Neden**: Batch, HDInsight Spark kümesinde silindi.

- **Öneri**: HDInsight Spark kümesindeki toplu işlerle ilgili sorunları giderin. Daha fazla yardım için HDInsight desteğiyle iletişim kurun. 


### <a name="error-code--2328"></a>Hata kodu: 2328

- **İleti**: `Failed to create the on demand HDI cluster. Cluster or linked service name: '%clusterName;', error: '%message;'`

- **Neden**: `The error message should show the details of what went wrong.`

- **Öneri**: hata iletisi sorunu gidermeye yardımcı olmalıdır.


### <a name="error-code--2329"></a>Hata kodu: 2329

- **İleti**: `Failed to delete the on demand HDI cluster. Cluster or linked service name: '%clusterName;', error: '%message;'`

- **Neden**: hata iletisinde neyin yanlış olduğuna ilişkin ayrıntılar gösterilmelidir.

- **Öneri**: hata iletisi sorunu gidermeye yardımcı olmalıdır.


### <a name="error-code--2331"></a>Hata kodu: 2331

- **İleti**: `The file path should not be null or empty.`

- **Neden**: belirtilen dosya yolu boş.

- **Öneri**: lütfen var olan bir dosya için bir yol belirtin.


### <a name="error-code--2340"></a>Hata kodu: 2340

- **İleti**: `HDInsightOnDemand linked service does not support execution via SelfHosted IR. Your IR name is '%IRName;'. Please select an Azure IR instead.`

- **Neden**: hdınsightondemand bağlı hizmeti, kendını barındıran IR aracılığıyla yürütmeyi desteklemez.

- **Öneri**: lütfen bir Azure IR seçip yeniden deneyin.


### <a name="error-code--2341"></a>Hata kodu: 2341

- **İleti**: `HDInsight cluster URL '%clusterUrl;' is incorrect, it must be in URI format and the scheme must be 'https'.`

- **Neden**: belirtilen URL doğru biçimde değil.

- **Öneri**: lütfen küme URL 'sini düzelttikten sonra yeniden deneyin.


### <a name="error-code--2342"></a>Hata kodu: 2342

- **İleti**: `Failed to connect to HDInsight cluster: '%errorMessage;'.`

- **Neden**: belirtilen kimlik bilgileri küme için yanlış veya bir ağ yapılandırması ya da bağlantı sorunu veya IR, kümeye bağlanırken sorun yaşıyor.

- **Öneri**:  
      1. HDInsight kümesinin ambarı Kullanıcı arabirimini bir tarayıcıda açarak kimlik bilgilerinin doğru olduğundan emin olun.
      2. Küme VNet 'se ve şirket içinde barındırılan IR kullanılıyorsa, HDI URL 'sinin sanal ağlarda özel URL olması gerekir. Bu, küme adından sonra '-int ' olması gerektiği anlamına gelir. Örneğin "https://mycluster.azurehdinsight.net/", "https://mycluster-int.azurehdinsight.net/" olarak değiştirilmelidir.
      2. Küme VNet 'daysa, şirket içinde barındırılan IR kullanılır ve özel URL kullanılmıştır ve bağlantı hala başarısız olduysa, IR 'nin yüklü olduğu VM HDI 'ya bağlanmada sorun yaşadı. IR 'nin yüklendiği VM 'ye bağlanın ve bir tarayıcıda ambarı Kullanıcı arabirimini açın. Küme için özel URL 'YI kullanın. Bu bağlantı tarayıcıdan çalışmalıdır. Değilse, daha fazla yardım için HDInsight destek ekibine başvurun.
      3. Şirket içinde barındırılan IR kullanılmıyorsa, HDI kümesine genel olarak erişilebilir olması gerekir. Bir tarayıcıda ambarı Kullanıcı arabirimini açın ve açıldığından emin olun. Küme veya hizmetlerle ilgili herhangi bir sorun varsa, yardım için HDInsight destek ekibine başvurun.
      Bu nedenle, genellikle ADF bağlantılı hizmette kullanılan HDI kümesi URL 'sine, test bağlantısının geçişine ve çalıştırmaların çalışması için ADF IR (Şirket içinde barındırılan veya Azure) için erişilebilir olması gerekir. Bu URL, VM 'den ya da herhangi bir ortak makineden bir tarayıcıdan açılarak kolayca doğrulanabilir.
    


### <a name="error-code--2343"></a>Hata kodu: 2343

- **İleti**: `User name and password cannot be null or empty to connect to the HDInsight cluster.`

- **Neden**: Kullanıcı adı ya da parola boş.

- **Öneri**: HDI 'ya bağlanmak için doğru kimlik bilgilerini sağlayın ve yeniden deneyin.


### <a name="error-code--2345"></a>Hata kodu: 2345

- **İleti**: `Failed to read the content of the hive script. Error: '%message;'`

- **Neden**: betik dosyası yok veya ADF betiğin konumuna bağlanamadı.

- **Öneri**: lütfen betiğin var olduğunu ve ilişkili bağlı hizmetin bağlantı için uygun kimlik bilgilerine sahip olduğunu doğrulayın.


### <a name="error-code--2346"></a>Hata kodu: 2346

- **İleti**: `Failed to create ODBC connection to the HDI cluster with error message '%message;'.`

- **Neden**: ADF, HDI KÜMESINE bir ODBC bağlantısı kurmaya çalıştı ve hata vererek başarısız oldu.

- **Öneri**: hata iletisi ve hata kodu, ODBC bağlantı hatalarında sorun gidermeye yardımcı olmalıdır. Sorunu çözmek için yeterli olmadığı durumlarda lütfen destek için Azure HDInsight ekibine başvurun.


### <a name="error-code--2347"></a>Hata kodu: 2347

- **İleti**: `Hive execution through ODBC failed with error message '%message;'.`

- **Neden**: ADF, ODBC bağlantısı aracılığıyla HDI kümesine yürütülmek üzere Hive betiği gönderdi ve komut dosyası HDI üzerinde başarısız oldu.

- **Öneri**: Hive betiğinin YÜRÜTÜLMESI HDI kümesinde başarısız oldu ve hata iletisi ve hata kodu sorun gidermeye yardımcı olmalıdır. Sorunu çözmek için yeterli olmadığı durumlarda lütfen destek için Azure HDInsight ekibine başvurun.


### <a name="error-code--2348"></a>Hata kodu: 2348

- **İleti**: `The main storage has not been initialized. Please check the properties of the storage linked service in the HDI linked service.`

- **Neden**: depolama bağlı hizmeti özellikleri doğru ayarlanmadı.

- **Öneri**: HDI etkinlikleri için ana depolama bağlı hizmetinde yalnızca tam bağlantı dizeleri desteklenir. Lütfen MSI auth veya uygulamalar kullandığınızdan emin olun.


### <a name="error-code--2350"></a>Hata kodu: 2350

- **İleti**: `Failed to prepare the files for the run '%jobId;'. HDI cluster: '%cluster;', Error: '%errorMessage;'`

- **Neden**: dosyaların bulunması gereken depolama alanına bağlanmak için girilen kimlik bilgileri yanlış veya dosyalar burada yok.

- **Öneri**: ADF, HDI etkinlikleri için hazırlık adımlarını yaparken bu hata oluşur. İşi HDI 'ye göndermeden önce dosyaları ana depolamaya kopyalamaya çalışır. Dosyaların belirtilen konumda mevcut olduğundan, depolama bağlantısının doğru olduğundan emin olun. ADF HDI etkinlikleri, HDI etkinlikleri ile ilgili depolama hesaplarında MSI kimlik doğrulamasını desteklemez, bu nedenle bu bağlı hizmetlerin tam anahtarlara sahip olduğundan veya Azure Key Vault kullanmakta olduğundan emin olun.


### <a name="error-code--2351"></a>Hata kodu: 2351

- **İleti**: `Could not open the file '%filePath;' in container/fileSystem '%container;'.`

- **Neden**: dosya belirtilen yolda yok.

- **Öneri**: Lütfen dosyanın gerçekten varolup olmadığını ve bu dosyaya işaret eden bağlantı bilgilerine sahip bağlantılı hizmetin doğru kimlik bilgilerine sahip olduğunu denetleyin.


### <a name="error-code--2352"></a>Hata kodu: 2352

- **İleti**: `The file storage has not been initialized. Please check the properties of the file storage linked service in the HDI activity.`

- **Neden**: dosya depolama bağlı hizmeti özellikleri doğru ayarlanmadı.

- **Öneri**: Lütfen dosya depolama bağlı hizmetinin özelliklerinin düzgün yapılandırıldığından emin olun.


### <a name="error-code--2353"></a>Hata kodu: 2353

- **İleti**: `The script storage has not been initialized. Please check the properties of the script storage linked service in the HDI activity.`

- **Neden**: betik depolama bağlı hizmeti özellikleri doğru ayarlanmadı.

- **Öneri**: lütfen betik depolama bağlı hizmetinin özelliklerinin düzgün yapılandırıldığından emin olun.


### <a name="error-code--2354"></a>Hata kodu: 2354

- **İleti**: `The storage linked service type '%linkedServiceType;' is not supported for '%executorType;' activities for property '%linkedServicePropertyName;'.`

- **Neden**: depolama bağlı hizmet türü etkinlik tarafından desteklenmiyor.

- **Öneri**: Lütfen seçili bağlı hizmetin etkinlik için desteklenen türlerden birine sahip olduğundan emin olun. HDI etkinlikleri AzureBlobStorage ve AzureBlobFSStorage bağlı hizmetlerini destekler.


### <a name="error-code--2355"></a>Hata kodu: 2355

- **İleti**: `The '%value' provided for commandEnvironment is incorrect. The expected value should be an array of strings where each string has the format CmdEnvVarName=CmdEnvVarValue.`

- **Neden**: commandEnvironment için girilen değer yanlış.

- **Öneri**:  
      Lütfen belirtilen değerin şuna benzer olduğundan emin olun: \"commandEnvironment\": [\"variableName = variableValue\"] ve her değişken listede yalnızca bir kez görünür.
    


### <a name="error-code--2356"></a>Hata kodu: 2356

- **İleti**: `The commandEnvironment already contains a variable named '%variableName;'.`

- **Neden**: değişken commandEnvironment içinde iki kez sağlandı.

- **Öneri**:  
      Lütfen belirtilen değerin şuna benzer olduğundan emin olun: \"commandEnvironment\": [\"variableName = variableValue\"] ve her değişken listede yalnızca bir kez görünür.
    


### <a name="error-code--2357"></a>Hata kodu: 2357

- **İleti**: `The certificate or password is wrong for ADLS Gen 1 storage.`

- **Neden**: belirtilen kimlik bilgileri yanlış.

- **Öneri**: lütfen ADLS Gen 1 bağlantılı hizmetindeki bağlantı bilgilerini doğrulayın ve test bağlantısının başarılı olduğundan emin olun.


### <a name="error-code--2358"></a>Hata kodu: 2358

- **İleti**: `The value '%value;' for the required property 'TimeToLive' in the on demand HDInsight linked service '%linkedServiceName;' has invalid format. It should be a timespan between '00:05:00' and '24:00:00'.`

- **Neden**: gerekli ' TimeToLive ' özelliği için belirtilen değer geçersiz biçime sahip. 

- **Öneri**: Lütfen değeri önerilen aralıkta olacak şekilde güncelleştirin ve yeniden deneyin.


### <a name="error-code--2359"></a>Hata kodu: 2359

- **İleti**: `The value '%value;' for the property 'roles' is invalid. Expected types are 'zookeeper', 'headnode', and 'workernode'.`

- **Neden**: ' Roles ' özelliği için belirtilen değer geçersiz.

- **Öneri**: lütfen bu değeri önerilerden biri olacak şekilde güncelleştirip yeniden deneyin.


### <a name="error-code--2360"></a>Hata kodu: 2360

- **İleti**: `The connection string in HCatalogLinkedService is invalid. Encountered an error while trying to parse: '%message;'.`

- **Neden**: HCatalogLinkedService için girilen bağlantı dizesi geçersiz.

- **Öneri**: Lütfen değeri doğru BIR Azure SQL bağlantı dizesi olarak güncelleştirip yeniden deneyin.


### <a name="error-code--2361"></a>Hata kodu: 2361

- **İleti**: `Failed to create on demand HDI cluster. Cluster name is '%clusterName;'.`

- **Neden**: küme oluşturma başarısız oldu ve ADF, HDInsight hizmetinden geri bir hata almadı.

- **Öneri**: Azure portalını açın ve girilen ada sahip HDI kaynağını bulmayı deneyin ve sağlama durumunu denetleyin. Daha fazla yardım için HDInsight destek ekibine başvurun.


### <a name="error-code--2362"></a>Hata kodu: 2362

- **İleti**: `Only Azure Blob storage accounts are supported as additional storages for HDInsight on demand linked service.`

- **Neden**: belirtilen ek depolama alanı Azure Blob depolama değildi.

- **Öneri**: Azure Blob Depolama hesabını Isteğe bağlı HDInsight için ek depolama alanı olarak sağlayın.



## <a name="web-activity"></a>Web Etkinliği

### <a name="error-code--2128"></a>Hata kodu: 2128

- **İleti**: `No response from the endpoint. Possible causes: network connectivity, DNS failure, server certificate validation or timeout.`

- **Neden**: ağ BAĞLANTıSı, DNS hatası, sunucu sertifikası doğrulama veya zaman aşımı.

- **Öneri**: vurılmaya çalıştığınız bitiş noktasının isteklere yanıt verdiğini doğrulayın. Fiddler/Postman gibi araçları kullanabilirsiniz.


### <a name="error-code--2108"></a>Hata kodu: 2108

- **İleti**: `Error calling the endpoint '%url;'. Response status code: '%code;'`

- **Neden**: ağ BAĞLANTıSı, DNS hatası, sunucu sertifikası doğrulama veya zaman aşımı gibi temel bir sorun nedeniyle istek başarısız oldu.

- **Öneri**: isteği doğrulamak Için Fiddler/Postman kullanın.
<br>


#### <a name="more-details"></a>Diğer ayrıntılar
Fiddler 'i kullanarak izlenen Web uygulamasının HTTP oturumunu oluşturma:

1. [Fiddler](https://www.telerik.com/download/fiddler)'i indirin, yükleyin ve açın.

1. Web **UYGULAMANıZ https kullanıyorsa**, **Araçlar** >  > **Fiddler seçenekleri** ' ne gidin. **Yakalama HTTPS bağlantıları** ve **https trafiğinin şifresini çöz**' ü seçin.

![Fiddler seçenekleri](media/data-factory-troubleshoot-guide/fiddler-options.png)

1. Uygulamanız SSL sertifikaları kullanıyorsa, Fiddler sertifikasını cihazınıza ekleyin. **HTTPS** > **eylemleri** > , > **kök sertifikayı masaüstüne dışarı aktarmak**Için **Araçlar** > **Fiddler seçenekleri** ' ne gidin.

1. **Dosya** > **yakalama trafiği**' ne giderek yakalamayı kapatın. Ya da **F12**tuşuna basın.

1. Tüm önbelleğe alınmış öğelerin kaldırılması ve yeniden indirilmesi gereken şekilde tarayıcınızın önbelleğini temizleyin.

1. İstek oluştur:

a. **Besteci** sekmesini seçin.

b. HTTP yöntemini ve URL 'YI ayarlayın.

c. Gerekiyorsa üstbilgiler ve istek gövdesi ekleyin.

d. **Yürüt**’ü seçin.

9. Trafiği yeniden yakalamayı açın ve sayfada sorunlu işlemi doldurun.

10. **Tüm oturumları** > **kaydetmek** > **Dosya** ' ya gidin.

Daha fazla bilgi için bkz. [Fiddler ile çalışmaya](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureFiddler)başlama.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorun giderme Yardım için şu kaynakları deneyin:

*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [MSDN forumu](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Data Factory için Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)


            
