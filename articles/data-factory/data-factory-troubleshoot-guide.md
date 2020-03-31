---
title: Sorun Giderme Azure Veri Fabrikası | Microsoft Dokümanlar
description: Azure Veri Fabrikası'nda dış denetim etkinliklerini nasıl gidererini öğrenin.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 8/26/2019
ms.author: abnarain
ms.reviewer: craigg
ms.openlocfilehash: e284060893e00ed7459edd0d1a03075c813dc5b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80065388"
---
# <a name="troubleshoot-azure-data-factory"></a>Sorun Giderme Azure Veri Fabrikası

Bu makalede, Azure Veri Fabrikası'nda dış denetim etkinlikleri için sık karşılaşılan sorun giderme yöntemleri incelemektedir.

## <a name="connector-and-copy-activity"></a>Bağlayıcı ve kopyalama etkinliği

Bağlayıcı sorunları için örneğin kopyalama etkinliğini kullanarak hatayla karşılaşmak için [Azure Veri Fabrikası Bağlayıcıları Sorun Giderme](connector-troubleshoot-guide.md)konusuna bakın.
  

## <a name="azure-databricks"></a>Azure Databricks

### <a name="error-code--3200"></a>Hata kodu: 3200

- **Mesaj**: Hata 403.

- **Sebep**:`The Databricks access token has expired.`

- **Öneri**: Varsayılan olarak, Azure Databricks erişim jetonu 90 gün boyunca geçerlidir. Yeni bir belirteç oluşturun ve bağlantılı hizmeti güncelleştirin.


### <a name="error-code--3201"></a>Hata kodu: 3201

- **Mesaj**:`Missing required field: settings.task.notebook_task.notebook_path.`

- **Sebep**:`Bad authoring: Notebook path not specified correctly.`

- **Öneri**: Databricks etkinliğindeki not defteri yolunu belirtin.

<br/>  

- **Mesaj**:`Cluster... does not exist.`

- **Sebep**:`Authoring error: Databricks cluster does not exist or has been deleted.`

- **Öneri**: Databricks kümesinin var olduğunu doğrulayın.

<br/>  

- **Mesaj**:`Invalid Python file URI... Please visit Databricks user guide for supported URI schemes.`

- **Sebep**:`Bad authoring.`

- **Öneri**: Çalışma alanı adresleme düzenleri veya `dbfs:/folder/subfolder/foo.py` Databricks Dosya Sistemi'nde depolanan dosyalar için mutlak yolları belirtin.

<br/>  

- **Mesaj**:`{0} LinkedService should have domain and accessToken as required properties.`

- **Sebep**:`Bad authoring.`

- **Öneri**: [Bağlantılı hizmet tanımını](compute-linked-services.md#azure-databricks-linked-service)doğrulayın.

<br/>  

- **Mesaj**:`{0} LinkedService should specify either existing cluster ID or new cluster information for creation.`

- **Sebep**:`Bad authoring.`

- **Öneri**: [Bağlantılı hizmet tanımını](compute-linked-services.md#azure-databricks-linked-service)doğrulayın.

<br/>  

- **Mesaj**:`Node type Standard_D16S_v3 is not supported. Supported node types:   Standard_DS3_v2, Standard_DS4_v2, Standard_DS5_v2, Standard_D8s_v3,   Standard_D16s_v3, Standard_D32s_v3, Standard_D64s_v3, Standard_D3_v2,   Standard_D8_v3, Standard_D16_v3, Standard_D32_v3, Standard_D64_v3,   Standard_D12_v2, Standard_D13_v2, Standard_D14_v2, Standard_D15_v2,   Standard_DS12_v2, Standard_DS13_v2, Standard_DS14_v2, Standard_DS15_v2,   Standard_E8s_v3, Standard_E16s_v3, Standard_E32s_v3, Standard_E64s_v3,   Standard_L4s, Standard_L8s, Standard_L16s, Standard_L32s, Standard_F4s,   Standard_F8s, Standard_F16s, Standard_H16, Standard_F4s_v2, Standard_F8s_v2,   Standard_F16s_v2, Standard_F32s_v2, Standard_F64s_v2, Standard_F72s_v2,   Standard_NC12, Standard_NC24, Standard_NC6s_v3, Standard_NC12s_v3,   Standard_NC24s_v3, Standard_L8s_v2, Standard_L16s_v2, Standard_L32s_v2,   Standard_L64s_v2, Standard_L80s_v2.`

- **Sebep**:`Bad authoring.`

- **Öneri**: Hata iletisine bakın.

<br/>

### <a name="error-code--3202"></a>Hata kodu: 3202

- **Mesaj**:`There were already 1000 jobs created in past 3600 seconds, exceeding rate limit:   1000 job creations per 3600 seconds.`

- **Sebep**:`Too many Databricks runs in an hour.`

- **Öneri**: Bu Databricks çalışma alanını kullanan tüm ardışık lıkları iş oluşturma oranı için denetleyin.  Çok fazla Veri tuğlası başlatılan ardışık hatlar toplu olarak çalıştırılırsa, bazı ardışık işleri yeni bir çalışma alanına geçirin.

<br/>  

- **Mesaj**:`Could not parse request object: Expected 'key' and 'value' to be set for JSON map field base_parameters, got 'key: "..."' instead.`

- **Sebep**:`Authoring error: No value provided for the parameter.`

- **Öneri**: JSON ardışık hattını inceleyin ve baseParameters not defterindeki tüm parametrelerin boş olmayan bir değer belirtdiğinden emin olun.

<br/>  

- **Mesaj** `User: `: SimpleUserContext{userId=...,user@company.comname= , orgId=...}` is not   authorized to access cluster.`

- **Neden**: Erişim jetonunu oluşturan kullanıcının bağlantılı hizmette belirtilen Databricks kümesine erişmesine izin verilmez.

- **Öneri**: Kullanıcının çalışma alanında gerekli izinlere sahip olduğundan emin olun.


### <a name="error-code--3203"></a>Hata kodu: 3203

- **Mesaj**:`The cluster is in Terminated state, not available to receive jobs. Please fix the cluster or retry later.`

- **Neden**: Küme sonlandırıldı. Etkileşimli kümeler için bu bir yarış koşulu olabilir.

- **Öneri**: Bu hatayı önlemenin en iyi yolu iş kümelerini kullanmaktır.


### <a name="error-code--3204"></a>Hata kodu: 3204

- **Mesaj**:`Job execution failed.`

- **Neden**: Hata iletileri beklenmeyen küme durumu veya belirli bir etkinlik gibi çeşitli sorunları gösterir. Çoğu zaman hiçbir hata iletisi görünür.

- **Öneri**: N/A
            

## <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics

Aşağıdaki tablo U-SQL için geçerlidir.
      
### <a name="error-code--2709"></a>Hata kodu: 2709

- **Mesaj**:`The access token is from the wrong tenant.`

- **Neden**: Yanlış Azure Etkin Dizini (Azure AD) kiracı.

- **Öneri**: Yanlış Azure Etkin Dizini (Azure AD) kiracı.

<br/>

- **Mesaj**:`We cannot accept your job at this moment. The maximum number of queued jobs for   your account is 200. `

- **Neden**: Bu hata, Data Lake Analytics'te daraltma neden olur.

- **Öneri**: Data Factory tetikleyicilerini ve etkinliklerdeki eşzamanlılık ayarlarını değiştirerek Data Lake Analytics'e gönderilen iş sayısını azaltın. Veya Data Lake Analytics'in sınırlarını artırın.

<br/>  

- **Mesaj**:`This job was rejected because it requires 24 AUs. This account's administrator-defined policy prevents a job from using more than 5 AUs.`

- **Neden**: Bu hata, Data Lake Analytics'te daraltma neden olur.

- **Öneri**: Data Factory tetikleyicilerini ve etkinliklerdeki eşzamanlılık ayarlarını değiştirerek Data Lake Analytics'e gönderilen iş sayısını azaltın. Veya Data Lake Analytics'in sınırlarını artırın.


### <a name="error-code--2705"></a>Hata kodu: 2705

- **Mesaj**:`Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/>  <br/>  User is   not able to access Data Lake Store.  <br/>  <br/>  User is  not authorized to use Data Lake Analytics.`

- **Neden**: Servis sorumlusunun veya sertifikanın depolama alanında dosyaya erişimi yoktur.

- **Öneri**: Data Lake Analytics işleri için kullanıcının sağladığı hizmet sorumlusunun veya sertifikasının, Kök klasöründen Veri Gölü Analizi hesabına ve varsayılan Veri Gölü Depolama örneğine erişebilmesini unutmayın.


### <a name="error-code--2711"></a>Hata kodu: 2711

- **Mesaj**:`Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/>  <br/>  User is   not able to access Data Lake Store.  <br/>  <br/>  User is  not authorized to use Data Lake Analytics.`

- **Neden**: Servis sorumlusunun veya sertifikanın depolama alanında dosyaya erişimi yoktur.

- **Öneri**: Data Lake Analytics işleri için kullanıcının sağladığı hizmet sorumlusunun veya sertifikasının, Kök klasöründen Veri Gölü Analizi hesabına ve varsayılan Veri Gölü Depolama örneğine erişebilmesini unutmayın.

<br/>  

- **Mesaj**:`Cannot find the 'Azure Data Lake Store' file or folder.`

- **Neden**: U-SQL dosyasına giden yol yanlış veya bağlı hizmet kimlik bilgilerinin erişimi yok.

- **Öneri**: Bağlantılı hizmette sağlanan yolu ve kimlik bilgilerini doğrulayın.


### <a name="error-code--2704"></a>Hata kodu: 2704

- **Mesaj**:`Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/>  <br/>  User is   not able to access Data Lake Store.  <br/>  <br/>  User is  not authorized to use Data Lake Analytics.`

- **Neden**: Servis sorumlusunun veya sertifikanın depolama alanında dosyaya erişimi yoktur.

- **Öneri**: Data Lake Analytics işleri için kullanıcının sağladığı hizmet sorumlusunun veya sertifikasının, Kök klasöründen Veri Gölü Analizi hesabına ve varsayılan Veri Gölü Depolama örneğine erişebilmesini unutmayın.


### <a name="error-code--2707"></a>Hata kodu: 2707

- **Mesaj**:`Cannot resolve the account of AzureDataLakeAnalytics. Please check 'AccountName' and   'DataLakeAnalyticsUri'.`

- **Neden**: Bağlı hizmetteki Data Lake Analytics hesabı yanlıştır.

- **Öneri**: Doğru hesabın sağlandığını doğrulayın.


### <a name="error-code--2703"></a>Hata kodu: 2703

- **Mesaj**:`Error Id: E_CQO_SYSTEM_INTERNAL_ERROR (or any error that starts with "Error   Id:").`

- **Neden**: Hata Data Lake Analytics'ten dir.

- **Öneri**: Örnek teki gibi bir hata, işin Data Lake Analytics'e gönderildiği ve oradaki komut dosyasının başarısız olduğu anlamına gelir. Data Lake Analytics'te araştırma. Portalda, Veri Gölü Analizi hesabına gidin ve Veri Fabrikası etkinlik çalıştırkimliği (boru hattı çalıştıran kimliği değil) kullanarak işi arayın. Oradaki iş hata hakkında daha fazla bilgi sağlar ve sorun gidermenize yardımcı olur. Karar net değilse, Data Lake Analytics destek ekibine başvurun ve hesap adınızı ve iş kimliğiniiçeren iş URL'sini sağlayın.
          

## <a name="azure-functions"></a>Azure işlevleri

### <a name="error-code--3602"></a>Hata kodu: 3602

- **Mesaj**:`Invalid HttpMethod: '%method;'.`

- **Neden**: Etkinlik yükünde belirtilen Http yöntemi Azure İşlev Etkinliği tarafından desteklenmez.

- **Öneri**: Desteklenen Http yöntemleri PUT, POST, GET, DELETE, OPTIONS, HEAD ve TRACE'dir.


### <a name="error-code--3603"></a>Hata kodu: 3603

- **Mesaj**:`Response Content is not a valid JObject.`

- **Neden**: Çağrılan Azure işlevi yanıtta JSON Yükü döndürmedi. ADF Azure işlev etkinliği yalnızca JSON yanıt içeriğini destekler.

- **Öneri**: Geçerli bir JSON Yükü döndürmek için Azure işlevini güncelleştirme örneğin bir C# işlevi\"(ActionResult)yeni OkObjectResult("{ Id\":\"123\"}");


### <a name="error-code--3606"></a>Hata kodu: 3606

- **İleti**: Azure işlev etkinliği eksik işlev anahtarı.

- **Neden**: Azure işlev etkinliği tanımı tamamlanmadı.

- **Öneri**: Lütfen Azureİş etkinliği JSON tanımının 'functionKey' adlı özelliğine sahip olduğunu kontrol edin.


### <a name="error-code--3607"></a>Hata kodu: 3607

- **Mesaj**:`Azure function activity missing function name.`

- **Neden**: Azure işlev etkinliği tanımı tamamlanmadı.

- **Öneri**: Lütfen Azureİş etkinliği JSON tanımının 'functionName' adlı özelliğine sahip olan girişi kontrol edin.


### <a name="error-code--3608"></a>Hata kodu: 3608

- **Mesaj**:`Call to provided Azure function '%FunctionName;' failed with status-'%statusCode;' and message - '%message;'.`

- **Neden**: Etkinlik tanımındaki Azure işlev ayrıntıları yanlış olabilir.

- **Öneri**: Azure işlev ayrıntılarını düzeltin ve yeniden deneyin.


### <a name="error-code--3609"></a>Hata kodu: 3609

- **Mesaj**:`Azure function activity missing functionAppUrl.`

- **Neden**: Azure işlev etkinliği tanımı tamamlanmadı.

- **Öneri**: Lütfen Azureİş etkinliği JSON tanımının 'functionAppUrl' adlı özelliğine sahip olan girişi kontrol edin.


### <a name="error-code--3610"></a>Hata kodu: 3610

- **Mesaj**:`There was an error while calling endpoint.`

- **Neden**: İşlev URL'si yanlış olabilir.

- **Öneri**: Lütfen Etkinlik JSON'daki 'functionAppUrl' değerinin doğru olduğundan emin olun ve yeniden deneyin.


### <a name="error-code--3611"></a>Hata kodu: 3611

- **Mesaj**:`Azure function activity missing Method in JSON.`

- **Neden**: Azure işlev etkinliği tanımı tamamlanmadı.

- **Öneri**: Lütfen Azureİş etkinliği JSON tanımının 'yöntem' adlı özelliğini işaretleyin.


### <a name="error-code--3612"></a>Hata kodu: 3612

- **Mesaj**:`Azure function activity missing LinkedService definition in JSON.`

- **Neden**: Azure işlev etkinliği tanımı tamamlanmadı.

- **Öneri**: Lütfen Azureİş etkinliği JSON tanımının hizmet ayrıntılarını bağlayıştırdığı girişi kontrol edin.



## <a name="azure-machine-learning"></a>Azure Machine Learning

### <a name="error-code--4101"></a>Hata kodu: 4101

- **Mesaj**:`AzureMLExecutePipeline activity '%activityName;' has invalid value for property '%propertyName;'.`

- **Neden**: Kötü biçim veya özellik '%propertyName;' tanımı eksik.

- **Öneri**: Lütfen '%activityName;' etkinliğinin '%propertyName;' özelliğinin doğru verilerle tanımlandığını kontrol edin.


### <a name="error-code--4110"></a>Hata kodu: 4110

- **Mesaj**:`AzureMLExecutePipeline activity missing LinkedService definition in JSON.`

- **Neden**: AzureMLExecutePipeline etkinlik tanımı tamamlanmadı.

- **Öneri**: Lütfen azuremlexecutepipeline etkinliği JSON tanımının hizmet ayrıntılarını bağlayıp bağlasa olmadığını kontrol edin.


### <a name="error-code--4111"></a>Hata kodu: 4111

- **Mesaj**:`AzureMLExecutePipeline activity has wrong LinkedService type in JSON. Expected LinkedService type: '%expectedLinkedServiceType;', current LinkedService type: Expected LinkedService type: '%currentLinkedServiceType;'.`

- **Neden**: Yanlış etkinlik tanımı.

- **Öneri**: Lütfen azuremlexecutepipeline etkinliği JSON tanımının bağlantılı hizmet ayrıntılarını doğru olup olmadığını kontrol edin.


### <a name="error-code--4112"></a>Hata kodu: 4112

- **Mesaj**:`AzureMLService linked service has invalid value for property '%propertyName;'.`

- **Neden**: Kötü biçim veya özellik '%propertyName;' tanımı eksik.

- **Öneri**: Lütfen bağlantılı hizmetin doğru verilerle tanımlanan '%propertyName;' özelliği olup olmadığını kontrol edin.


### <a name="error-code--4121"></a>Hata kodu: 4121

- **Mesaj**:`Request sent to Azure Machine Learning for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure Machine Learning: '%externalMessage;'.`

- **Neden :** Azure Machine Learning'e erişmek için kullanılan kimlik belgesinin süresi doldu.

- **Öneri**: Lütfen kimlik bilgisinin geçerli olduğunu doğrulayın ve yeniden deneyin


### <a name="error-code--4122"></a>Hata kodu: 4122

- **Mesaj**:`Request sent to Azure Machine Learning for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure Machine Learning: '%externalMessage;'.`

- **Neden**: Azure Machine Learning Bağlantılı Hizmeti'nde sağlanan kimlik bilgileri geçersizdir veya işlem izni yoktur.

- **Öneri**: Lütfen Bağlantılı Hizmetteki kimlik bilgisinin geçerli olduğunu ve Azure Machine Learning'e erişim izni olduğunu doğrulayın.


### <a name="error-code--4123"></a>Hata kodu: 4123

- **Mesaj**:`Request sent to Azure Machine Learning for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure Machine Learning: '%externalMessage;'.`

- **Neden**: PipelineParametreler gibi etkinliğin özellikleri Azure ML ardışık hatlar için geçersizdir.

- **Öneri**: Lütfen Bağlantılı Hizmette belirtilen yayımlanmış Azure ML ardışık hattının beklenen yüküyle eşleşecek etkinlik özelliklerinin değerini denetleyin.


### <a name="error-code--4124"></a>Hata kodu: 4124

- **Mesaj**:`Request sent to Azure Machine Learning for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure Machine Learning: '%externalMessage;'.`

- **Neden**: Yayınlanan Azure ML ardışık nokta sonu yok.

- **Öneri**: Lütfen Bağlantılı Hizmette belirtilen azure machine learning ardışık noktasının Azure Machine Learning'de mevcut olduğunu doğrulayın.


### <a name="error-code--4125"></a>Hata kodu: 4125

- **Mesaj**:`Request sent to Azure Machine Learning for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure Machine Learning: '%externalMessage;'.`

- **Neden**: Azure Machine Learning'de sunucu hatası.

- **Öneri**: Lütfen daha sonra yeniden deneyin. Sorun devam ederse yardım için Azure Machine Learning ekibine başvurun.


### <a name="error-code--4126"></a>Hata kodu: 4126

- **Mesaj**:`Azure ML pipeline run failed with status: '%amlPipelineRunStatus;'. Azure ML pipeline run Id: '%amlPipelineRunId;'. Please check in Azure Machine Learning for more error logs.`

- **Neden**: Azure ML ardışık hat lar çalışır başarısız oldu.

- **Öneri**: Lütfen daha fazla hata günlüğü için Azure Machine Learning'i kontrol edin ve ML ardışık hattını düzeltin.



## <a name="common"></a>Common

### <a name="error-code--2103"></a>Hata kodu: 2103

- **Mesaj**:`Please provide value for the required property '%propertyName;'.`

- **Neden**: Özelliğin değeri sağlanmadı, ancak senaryoda gerekli.

- **Öneri**: İletideki değeri sağlayın ve yeniden deneyin.


### <a name="error-code--2104"></a>Hata kodu: 2104

- **Mesaj**:`The type of the property '%propertyName;' is incorrect.`

- **Neden**: Sağlanan özelliğin türü beklendiği gibi değildir.

- **Öneri**: Lütfen özelliğin türünü düzeltin ve yeniden deneyin.


### <a name="error-code--2105"></a>Hata kodu: 2105

- **Mesaj**:`An invalid json is provided for property '%propertyName;'. Encountered an error while trying to parse: '%message;'.`

- **Neden**: Özelliğin değeri geçersiz dir veya beklenen biçime sahip değildir.

- **Öneri**: Lütfen mülkün belgelerini arayın ve sağlanan değerin beklenen biçim ve türe sahip olduğundan emin olun.


### <a name="error-code--2106"></a>Hata kodu: 2106

- **Mesaj**:`The storage connection string is invalid. %errorMessage;`

- **Neden**: Depolama alanının bağlantı dizesi geçersiz veya yanlış biçime sahiptir.

- **Öneri**: Lütfen Azure portalına gidin, depolama alanınızı bulun, bağlantı dizesini kopyalayın ve bağlantılı hizmetinizde yapıştırın ve yeniden deneyin.


### <a name="error-code--2108"></a>Hata kodu: 2108

- **Mesaj**:`Error calling the endpoint '%url;'. Response status code: '%code;'`

- **Neden**: Ağ bağlantısı, DNS hatası, sunucu sertifikası doğrulama veya zaman acısı gibi altta yatan bir sorun nedeniyle istek başarısız oldu.

- **Öneri**: İsteği doğrulamak için Fiddler/Postman'ı kullanın.


### <a name="error-code--2110"></a>Hata kodu: 2110

- **Mesaj**:`The linked service type '%linkedServiceType;' is not supported for '%executorType;' activities.`

- **Neden**: Etkinlikte belirtilen bağlantılı hizmet hatalıydı.

- **Öneri**: Lütfen bağlantılı hizmet türünün etkinlik için desteklenen türlerden biri olduğundan emin olun. Örneğin, HDI etkinlikleri için bağlantılı hizmet türü HDInsight veya HDInsightOnDemand olabilir.


### <a name="error-code--2111"></a>Hata kodu: 2111

- **Mesaj**:`The type of the property '%propertyName;' is incorrect. The expected type is %expectedType;.`

- **Neden**: Sağlanan özelliğin türü beklendiği gibi değildir.

- **Öneri**: Lütfen özelliğin türünü düzeltin ve yeniden deneyin.


### <a name="error-code--2112"></a>Hata kodu: 2112

- **Mesaj**:`The cloud type is unsupported or could not be determined for storage from the EndpointSuffix '%endpointSuffix;'.`

- **Neden**: Bulut türü desteklenmez veya EndpointSuffix'ten depolama için belirlenemedi.

- **Öneri**: Lütfen depolamayı başka bir bulutta kullanın ve yeniden deneyin.


### <a name="error-code--2128"></a>Hata kodu: 2128

- **Mesaj**:`No response from the endpoint. Possible causes: network connectivity, DNS failure, server certificate validation or timeout.`

- **Neden**: Ağ bağlantısı, DNS hatası, sunucu sertifikası doğrulama veya zaman acısı.

- **Öneri**: Vurmaya çalıştığınız bitiş noktasının isteklere yanıt verdiğini doğrulayın. Fiddler/Postman gibi araçları kullanabilirsiniz.



## <a name="custom"></a>Özel

Aşağıdaki tablo Azure Toplu İş için geçerlidir.
      
### <a name="error-code--2500"></a>Hata kodu: 2500

- **Mesaj**:`Hit unexpected exception and execution failed.`

- **Sebep**:`Can't launch command, or the program returned an error code.`

- **Öneri**: Çalıştırılabilir dosyanın var olduğundan emin olun. Program başladıysa, *stdout.txt* ve *stderr.txt'nin* depolama hesabına yüklendiğinden emin olun. Hata ayıklama için kodunuzda bol miktarda günlük ler yontmak iyi bir uygulamadır.


### <a name="error-code--2501"></a>Hata kodu: 2501

- **Mesaj**:`Cannot access user batch account; please check batch account settings.`

- **Neden**: Hatalı Toplu erişim anahtarı veya havuz adı.

- **Öneri**: Bağlantılı hizmetteki havuz adını ve Toplu İşlem erişim anahtarını doğrulayın.


### <a name="error-code--2502"></a>Hata kodu: 2502

- **Mesaj**:`Cannot access user storage account; please check storage account settings.`

- **Neden**: Yanlış depolama hesabı adı veya erişim anahtarı.

- **Öneri**: Bağlantılı hizmetteki depolama hesabı adını ve erişim anahtarını doğrulayın.


### <a name="error-code--2504"></a>Hata kodu: 2504

- **Mesaj**:`Operation returned an invalid status code 'BadRequest'.`

- **Neden**: Özel etkinliğin folderPath'inde çok fazla dosya. Kaynak Dosyalarının toplam boyutu 32.768 karakterden fazla olamaz.

- **Öneri**: Gereksiz dosyaları kaldırın. Ya da zip onları ve onları ayıklamak için bir unzip komutu ekleyin. Örneğin,`powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;  $folder\yourProgram.exe`


### <a name="error-code--2505"></a>Hata kodu: 2505

- **Mesaj**:`Cannot create Shared Access Signature unless Account Key credentials are used.`

- **Neden**: Özel etkinlikler yalnızca erişim anahtarı kullanan depolama hesaplarını destekler.

- **Öneri**: Hata açıklamasına bakın.


### <a name="error-code--2507"></a>Hata kodu: 2507

- **Mesaj**:`The folder path does not exist or is empty: ...`

- **Neden**: Belirtilen yoldaki depolama hesabında dosya yok.

- **Öneri**: Klasör yolu çalıştırmak istediğiniz yürütülebilir dosyaları içermelidir.


### <a name="error-code--2508"></a>Hata kodu: 2508

- **Mesaj**:`There are duplicate files in the resource folder.`

- **Neden**: Aynı adı taşıyan birden çok dosya folderPath'in farklı alt klasörlerinde bulunmaktadır.

- **Öneri**: Özel etkinlikler folderPath altında klasör yapısını düzleştirin.  Klasör yapısını korumanız gerekiyorsa, dosyaları zip'siz yapın ve zip'siz komut kullanarak Azure Toplu İşlem'de ayıklayın. Örneğin,`powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;   $folder\yourProgram.exe`


### <a name="error-code--2509"></a>Hata kodu: 2509

- **Mesaj**:`Batch   url ... is invalid; it must be in Uri format.`

- **Neden**: Toplu URL'ler benzer olmalıdır`https://mybatchaccount.eastus.batch.azure.com`

- **Öneri**: Hata açıklamasına bakın.


### <a name="error-code--2510"></a>Hata kodu: 2510

- **Mesaj**:`An   error occurred while sending the request.`

- **Neden**: Toplu iş URL'si geçersizdir.

- **Öneri**: Toplu IŞ URL'sini doğrulayın.
            

## <a name="hdinsight"></a>HDInsight

### <a name="error-code--200"></a>Hata kodu: 200

- **Mesaj**:`Unexpected error happened: '%error;'.`

- **Neden**: Bir iç hizmet sorunu vardır.

- **Tavsiye**: Daha fazla yardım için lütfen ADF desteğine başvurun.


### <a name="error-code--201"></a>Hata kodu: 201

- **Mesaj**:`JobType %jobType; is not found.`

- **Neden**: ADF tarafından desteklenmeyen yeni bir iş türü vardır.

- **Tavsiye**: Daha fazla yardım için lütfen ADF destek ekibiyle irtibata geçiniz.


### <a name="error-code--202"></a>Hata kodu: 202

- **Mesaj**:`Failed to create on demand HDI cluster. Cluster name or linked service name: '%clusterName;', error: '%message;'`

- **Neden**: Hata iletisi neyin yanlış gittiğinin ayrıntılarını göstermelidir.

- **Öneri**: Hata iletisi sorunu gidermek için yardımcı olmalıdır. Yeterli bilgi yoksa, daha fazla yardım için lütfen ADF desteğine başvurun.


### <a name="error-code--203"></a>Hata kodu: 203

- **Mesaj**:`Failed to delete on demand HDI cluster. Cluster name or linked service name: '%clusterName;', error: '%message;'`

- **Neden**: Hata iletisi neyin yanlış gittiğinin ayrıntılarını göstermelidir.

- **Öneri**: Hata iletisi sorunu gidermek için yardımcı olmalıdır. Yeterli bilgi yoksa, daha fazla yardım için lütfen ADF desteğine başvurun.


### <a name="error-code--204"></a>Hata kodu: 204

- **Mesaj**:`The resumption token is missing for runId '%runId;'.`

- **Neden**: Bir iç hizmet sorunu vardır.

- **Tavsiye**: Daha fazla yardım için lütfen ADF desteğine başvurun.


### <a name="error-code--205"></a>Hata kodu: 205

- **Mesaj**:`Failed to prepare cluster for LinkedService '%linkedServiceName;', the current resource status is '%status;'.`

- **Neden**: İsteğe bağlı HDI kümesi oluşturulurken bir hata oluştu.

- **Tavsiye**: Daha fazla yardım için lütfen ADF desteğine başvurun.


### <a name="error-code--206"></a>Hata kodu: 206

- **Mesaj**:`The batch ID for Spark job is invalid. Please retry your job, and if the problem persists, contact the ADF support for further assistance.`

- **Neden**: Buna neden olan hizmette bir iç sorun vardı.

- **Öneri**: Bu geçici bir sorun olabilir. Lütfen işinizi yeniden deneyin ve sorun devam ederse, daha fazla yardım için ADF desteğine başvurun.


### <a name="error-code--207"></a>Hata kodu: 207

- **Mesaj**:`Could not determine the region from the provided storage account. Please try using another primary storage account for the on demand HDI or contact ADF support team and provide the activity run ID.`

- **Neden**: Birincil depolama hesabından bölgeyi belirlemeye çalışırken bir iç hata oluştu.

- **Öneri**: Başka bir depolama alanı deneyin. Bunun kabul edilebilir bir çözüm olmaması durumunda, daha fazla yardım için ADF destek ekibine başvurun.


### <a name="error-code--208"></a>Hata kodu: 208

- **Mesaj**:`Service Principal or the MSI authenticator are not instantiated. Please consider providing a Service Principal in the HDI on demand linked service which has permissions to create an HDInsight cluster in the provided subscription and try again. In case if this is not an acceptable solution, contact ADF support team for further assistance.`

- **Neden**: Servis Sorumlusu'nu okumaya çalışırken veya MSI kimlik doğrulamasını anında okurken bir iç hata oluştu.

- **Öneri**: Lütfen sağlanan abonelikte bir HDInsight kümesi oluşturma ve yeniden deneme izni olan bir Hizmet Sorumlusu sağlamayı düşünün. Bu kabul edilebilir bir çözüm değilse, daha fazla yardım için ADF destek ekibine başvurun.


### <a name="error-code--2300"></a>Hata kodu: 2300

- **Mesaj**:`Failed to submit the job '%jobId;' to the cluster '%cluster;'. Error: %errorMessage;.`

<br>

- **Neden :** Hata iletisi 'Uzak ad çözülemedi'ye benzer bir ileti içeriyorsa, bu sağlanan küme URI'nin geçersiz olduğu anlamına gelebilir.


- **Öneri**: Kümenin silinmediğinden ve sağlanan URI'nin doğru olduğundan emin olun. Uri'yi bir tarayıcıda açtığınızda Ambari UI'yi görmeniz gerekir. Küme sanal bir ağdaysa, URI özel URI olmalıdır. Açmak için, aynı sanal ağın parçası olan bir VM kullanın. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment#directly-connect-to-apache-hadoop-services)
                  

<br>

- **Neden :** Hata iletisi 'Bir görev iptal edildi'ye benzer bir ileti içeriyorsa, bu iş göndermenin zaman dolduğu anlamına gelir.

- **Öneri**: Sorun genel HDInsight bağlantısı veya ağ bağlantısı olabilir. Öncelikle HDInsight Ambari UI'nin herhangi bir tarayıcıdan edinilebilen bir uyr'luk bilgi edinin. Kimlik bilgilerinizin hala geçerli olduğunu doğrulayın. Kendi kendine barındırılan entegre çalışma süresi (IR) kullanıyorsanız, bunu vm'den veya kendi barındırılan IR'nin yüklü olduğu makineden yaptığınızdan emin olun. Ardından işi Veri Fabrikası'ndan yeniden göndermeyi deneyin. Hala başarısız olursa, destek için Veri Fabrikası ekibine başvurun.

<br>

- **Neden**: Hata iletisi 'Kullanıcı yöneticisi Ambari'de kilitlendi' veya 'Yetkisiz: Ambari kullanıcı adı veya şifresi yanlış' gibi bir ileti içeriyorsa, bu HDInsight kimlik bilgilerinin yanlış olduğu veya süresinin dolduğu anlamına gelir.

- **Öneri**: Kimlik bilgilerini düzeltin ve bağlantılı hizmeti yeniden dağıtın. Önce herhangi bir tarayıcıda URI kümesini açıp oturum açmaya çalışarak kimlik bilgilerinin HDInsight'ta çalıştığından emin olun. Kimlik bilgileri çalışmıyorsa, bunları Azure portalından sıfırlayabilirsiniz.

<br>

- **Neden**: Hata iletisi '502 - Web sunucusu ağ geçidi veya proxy sunucusu gibi hareket ederken geçersiz bir yanıt aldı' gibi bir ileti içeriyorsa, bu hata HDInsight hizmeti tarafından döndürülür.


- **Öneri**: Azure HDInsight sorun giderme https://hdinsight.github.io/ambari/ambari-ui-502-error.htmlbelgelerine https://docs.microsoft.com/azure/application-gateway/application-gateway-troubleshooting-502( örneğin, . https://hdinsight.github.io/spark/spark-thriftserver-errors.html
                  

<br>

- **Neden**: Hata iletisi 'Templeton hizmeti çok fazla iş isteği göndermekle meşgul olduğu için iş isteği gönderemiyor' veya 'Queue root.joblauncher zaten 500 başvurusu var, başvuru sunulmasını kabul edemiyor' gibi bir ileti içeriyorsa, bu aynı anda HDInsight'a çok fazla iş gönderildiği anlamına gelir.

- **Öneri**: HDInsight'a gönderilen eşzamanlı iş sayısını sınırlamayı düşünün. İşler aynı etkinlik tarafından gönderiliyorsa Veri Fabrikası etkinlik eşzamanlılığı'na bakın. Eşzamanlı ardışık ardışık boru hattı çalışır zaman ayılmak için tetikleyicileri değiştirin. Templeton.parallellism.job.submit'i hatadan da anlaşılacağı gibi ayarlamak için HDInsight belgelerine bakın.


### <a name="error-code--2301"></a>Hata kodu: 2301

- **Mesaj**:`Could not get the status of the application '%physicalJobId;' from the HDInsight service. Received the following error: %message;. Please refer to HDInsight troubleshooting documentation or contact their support for further assistance.`

- **Neden**: HDInsight küme veya hizmet sorunları vardır.


- **Öneri**: Bu hata, Çalışan işin durumunu almaya çalışırken ADF HDInsight kümesinden yanıt almazsa olur. Kümenin kendisiyle ilgili sorunlara neden olabilir veya HDInsight hizmetinde bir kesinti olabilir. Lütfen HDInsight sorun giderme https://docs.microsoft.com/azure/hdinsight/hdinsight-troubleshoot-guidebelgelerine bakın veya daha fazla yardım için desteklerine başvurun.
                


### <a name="error-code--2302"></a>Hata kodu: 2302

- **Mesaj**:`Hadoop job failed with exit code '%exitCode;'. See '%logPath;/stderr' for more details. Alternatively, open the Ambari UI on the HDI cluster and find the logs for the job '%jobId;'. Contact HDInsight team for further support.`

- **Neden**: İş HDI kümesine gönderildi ve orada başarısız oldu.

- **Öneri**: Etkinlik çalıştır Çıktısındaki İplik günlükleri bağlantısını takip edin ve HDI çıkışındaki hataları arayın. Gerekirse destek için HDInsight ekibine başvurun.


### <a name="error-code--2303"></a>Hata kodu: 2303

- **Mesaj**:`Hadoop job failed with transient exit code '%exitCode;'. See '%logPath;/stderr' for more details. Alternatively, open the Ambari UI on the HDI cluster and find the logs for the job '%jobId;'. Try again or contact HDInsight team for further support.`

- **Neden**: İş HDI kümesine gönderildi ve orada başarısız oldu.

- **Öneri**: Etkinlik çalıştır Çıktısındaki İplik günlükleri bağlantısını takip edin ve HDI çıkışındaki hataları arayın. Yeniden deneyin veya gerekirse destek için HDInsight ekibine başvurun.


### <a name="error-code--2304"></a>Hata kodu: 2304

- **Mesaj**:`MSI authentication is not supported on storages for HDI activities.`

- **Neden**: HDI bağlantılı hizmet veya HDI etkinliğinde kullanılan depolama bağlantılı hizmet(ler), desteklenmeyen MSI kimlik doğrulaması ile yapılandırılır.

- **Öneri**: HdI bağlantılı hizmet veya HDI etkinliğinde kullanılan depolama hesapları için lütfen tam bağlantı dizeleri sağlayın.


### <a name="error-code--2305"></a>Hata kodu: 2305

- **Mesaj**:`Failed to initialize the HDInsight client for the cluster '%cluster;'. Error: '%message;'`

- **Neden**: HDI kümesi için bağlantı bilgileri yanlıştır, sağlanan kullanıcının gerekli eylemi gerçekleştirme izinleri yoktur veya HDInsight hizmetinde ADF'den gelen isteklere yanıt veren sorunlar vardır.

- **Öneri**: Lütfen kullanıcı bilgilerinin doğru olduğundan emin olun. Ayrıca, HDI kümesi için Ambari UI'sinin, IR'nin kendi kendine barındırılan IR durumunda yüklü olduğu VM'den bir tarayıcıda açılabilir veya Azure IR durumunda herhangi bir makineden açılabilir olduğunu da doğrulayın.


### <a name="error-code--2306"></a>Hata kodu: 2306

- **Mesaj**:`An invalid json is provided for script action '%scriptActionName;'. Error: '%message;'`

- **Neden**: Komut dosyası eylemi için sağlanan json geçersizdir.


- **Öneri**: Hata iletisi sorunu belirlemeye yardımcı olmalıdır. Lütfen json yapılandırmasını düzeltin ve yeniden deneyin. Daha https://docs.microsoft.com/azure/data-factory/compute-linked-services#azure-hdinsight-on-demand-linked-service fazla bilgi için kontrol edin.
                


### <a name="error-code--2310"></a>Hata kodu: 2310

- **Mesaj**:`Failed to submit Spark job. Error: '%message;'`

- **Neden**: ADF, Livy API (livy/batch) kullanarak Bir Kıvılcım kümesinde toplu iş oluşturmaya çalıştı, ancak bir hata aldı.

- **Öneri**: Sorunu gidermek için lütfen hata iletisini izleyin. Bunu çözmek için yeterli bilgi yoksa, HDI ekibine başvurun ve onlara ADF İzleme sayfasındaki etkinlik çalıştırın Çıktı'da bulunabilecek toplu iş kimliği ve iş kimliği sağlayın.


### <a name="error-code--2312"></a>Hata kodu: 2312

- **Mesaj**:`Spark job failed, batch id:%batchId;. Please follow the links in the activity run Output from ADF Monitoring page to troubleshoot the run on HDInsight Spark cluster. Please contact HDInsight support team for further assistance.`

- **Neden**: HdInsight Spark kümesinde iş başarısız oldu.

- **Öneri**: HDInsight Spark kümesindeki çalıştırmayı gidermek için lütfen ADF İzleme sayfasındaki Etkinlik çalıştır Çıktısı'ndaki bağlantıları izleyin. Daha fazla yardım için lütfen HDInsight destek ekibiyle iletişime geçin.


### <a name="error-code--2313"></a>Hata kodu: 2313

- **Mesaj**:`The batch with ID '%batchId;' was not found on Spark cluster. Open the Spark History UI and try to find it there. Contact HDInsight support for further assistance.`

- **Neden**: Parti HDInsight Spark kümesinde silindi.

- **Öneri**: HDInsight Spark kümesindeki sorun giderme toplu işlerini giderin. Daha fazla yardım için HDInsight desteğine başvurun. 


### <a name="error-code--2328"></a>Hata kodu: 2328

- **Mesaj**:`Failed to create the on demand HDI cluster. Cluster or linked service name: '%clusterName;', error: '%message;'`

- **Sebep**:`The error message should show the details of what went wrong.`

- **Öneri**: Hata iletisi sorunu gidermek için yardımcı olmalıdır.


### <a name="error-code--2329"></a>Hata kodu: 2329

- **Mesaj**:`Failed to delete the on demand HDI cluster. Cluster or linked service name: '%clusterName;', error: '%message;'`

- **Neden**: Hata iletisi neyin yanlış gittiğinin ayrıntılarını göstermelidir.

- **Öneri**: Hata iletisi sorunu gidermek için yardımcı olmalıdır.


### <a name="error-code--2331"></a>Hata kodu: 2331

- **Mesaj**:`The file path should not be null or empty.`

- **Neden**: Sağlanan dosya yolu boş.

- **Öneri**: Lütfen var olan bir dosya için bir yol sağlayın.


### <a name="error-code--2340"></a>Hata kodu: 2340

- **Mesaj**:`HDInsightOnDemand linked service does not support execution via SelfHosted IR. Your IR name is '%IRName;'. Please select an Azure IR instead.`

- **Neden**: HDInsightOnDemand bağlantılı hizmet SelfHosted IR üzerinden yürütme desteklemez.

- **Öneri**: Lütfen bir Azure IR seçin ve yeniden deneyin.


### <a name="error-code--2341"></a>Hata kodu: 2341

- **Mesaj**:`HDInsight cluster URL '%clusterUrl;' is incorrect, it must be in URI format and the scheme must be 'https'.`

- **Neden**: Sağlanan URL doğru biçimde değildir.

- **Öneri**: Lütfen küme URL'sini düzeltin ve yeniden deneyin.


### <a name="error-code--2342"></a>Hata kodu: 2342

- **Mesaj**:`Failed to connect to HDInsight cluster: '%errorMessage;'.`

- **Neden**: Sağlanan kimlik bilgileri küme için yanlış veya bir ağ yapılandırması veya bağlantı sorunu vardı veya IR kümeye bağlanma sorunları yaşıyor.

- **Tavsiye**:  
      1. HDInsight kümesinin Ambari UI'sini bir tarayıcıda açarak kimlik bilgilerinin doğru olduğunu doğrulayın.
      2. Küme VNet'teyse ve kendi kendine barındırılan IR kullanılıyorsa, HdI URL'sinin VNets'teki özel URL olması gerekir, bu da küme adından sonra '-int' olması gerektiği anlamına gelir. Örneğin "https://mycluster.azurehdinsight.net/" "https://mycluster-int.azurehdinsight.net/olarak değiştirilmelidir.
      2. Küme VNet'teyse, kendi kendine barındırılan IR kullanılıyorsa ve özel URL kullanılıyorsa ve bağlantı hala başarısızsa, IR'nin yüklü olduğu VM HDI'ye bağlanmada sorun yaşıyordemektir. IR yüklü VM'ye bağlanın ve Ambari UI'yi bir tarayıcıda açın. Küme için özel URL'yi kullanın. Bu bağlantı tarayıcıdan çalışmalıdır. Yoksa, daha fazla yardım için HDInsight destek ekibine başvurun.
      3. Kendi barındırılan IR kullanılmıyorsa, HDI kümesine genel olarak erişilebilir olmalıdır. Ambari UI'yi bir tarayıcıda açın ve açıldığından emin olun. Küme veya üzerindeki hizmetlerle ilgili herhangi bir sorun varsa, yardım için HDInsight destek ekibine başvurun.
      Bu nedenle, test bağlantısının geçebilmesi ve çalıştırmaların çalışabilmesi için genel olarak ADF bağlantılı hizmette kullanılan HDI küme URL'sinin ADF IR (kendi kendine barındırılan veya Azure) için erişilebilir olması gerekir. Bu, bu URL'yi VM'den veya herhangi bir ortak makineden bir tarayıcıdan açarak kolayca doğrulanabilir.
    


### <a name="error-code--2343"></a>Hata kodu: 2343

- **Mesaj**:`User name and password cannot be null or empty to connect to the HDInsight cluster.`

- **Neden**: Kullanıcı adı veya parola boş.

- **Öneri**: HDI'ye bağlanmak ve yeniden denemek için doğru kimlik bilgilerini sağlayın.


### <a name="error-code--2345"></a>Hata kodu: 2345

- **Mesaj**:`Failed to read the content of the hive script. Error: '%message;'`

- **Neden**: Komut dosyası dosyası yok veya ADF komut dosyasının konumuna bağlanamadı.

- **Öneri**: Lütfen komut dosyasının var olduğunu ve ilişkili bağlantılı hizmetin bağlantı için uygun kimlik bilgilerine sahip olduğunu doğrulayın.


### <a name="error-code--2346"></a>Hata kodu: 2346

- **Mesaj**:`Failed to create ODBC connection to the HDI cluster with error message '%message;'.`

- **Neden**: ADF HDI kümesine bir ODBC bağlantısı kurmaya çalıştı ve hata ile başarısız oldu.

- **Öneri**: Hata iletisi ve hata kodu, ODBC bağlantı hatalarını gidermede yardımcı olmalıdır. Sorunu çözmek için yeterli değillerse, destek için lütfen Azure HDInsight ekibine başvurun.


### <a name="error-code--2347"></a>Hata kodu: 2347

- **Mesaj**:`Hive execution through ODBC failed with error message '%message;'.`

- **Neden**: ADF, yürütme için kovan komut dosyasını ODBC bağlantısı üzerinden HDI kümesine gönderdi ve komut dosyası HDI'de başarısız oldu.

- **Öneri**: Kovan komut dosyasının yürütülmesi HDI küme ve hata iletisi ve hata kodu başarısız sorun giderme yardımcı olmalıdır. Sorunu çözmek için yeterli değillerse, destek için lütfen Azure HDInsight ekibine başvurun.


### <a name="error-code--2348"></a>Hata kodu: 2348

- **Mesaj**:`The main storage has not been initialized. Please check the properties of the storage linked service in the HDI linked service.`

- **Neden**: Depolamaya bağlı hizmet özellikleri doğru ayarlanmaz.

- **Öneri**: HDI etkinlikleri için ana depolama bağlantılı hizmette yalnızca tam bağlantı dizeleri desteklenir. Lütfen MSI auth veya uygulamaları kullanmadığınızdan emin olun.


### <a name="error-code--2350"></a>Hata kodu: 2350

- **Mesaj**:`Failed to prepare the files for the run '%jobId;'. HDI cluster: '%cluster;', Error: '%errorMessage;'`

- **Neden**: Dosyaların bulunması gereken depolama alanına bağlanmak için sağlanan kimlik bilgileri yanlış veya dosyalar orada bulunmaz.

- **Öneri**: ADF HDI etkinlikleri için hazırlık adımları yaptığında bu hata oluşur. İşi HDI'ye göndermeden önce dosyaları ana depolama alanına kopyalamaya çalışır. Dosyaların sağlanan konumda olduğundan emin olun, depolama bağlantısı doğrudur. ADF HDI etkinlikleri, HDI etkinlikleriile ilgili depolama hesaplarında MSI kimlik doğrulamasını desteklemez, bu nedenle bağlantılı hizmetlerin tam anahtarlara sahip olduğundan veya Azure Key Vault kullanıyorolduğundan emin olun.


### <a name="error-code--2351"></a>Hata kodu: 2351

- **Mesaj**:`Could not open the file '%filePath;' in container/fileSystem '%container;'.`

- **Neden**: Dosya belirtilen yolda yok.

- **Öneri**: Lütfen dosyanın gerçekten var olup olmadığını ve bu dosyayı işaret eden bağlantı bilgilerine sahip bağlantılı hizmetin doğru kimlik bilgilerine sahip olup olmadığını kontrol edin.


### <a name="error-code--2352"></a>Hata kodu: 2352

- **Mesaj**:`The file storage has not been initialized. Please check the properties of the file storage linked service in the HDI activity.`

- **Neden**: Dosya depolamasına bağlı hizmet özellikleri doğru ayarlanmaz.

- **Öneri**: Lütfen dosya depolamasına bağlı hizmetin özelliklerinin düzgün şekilde yapılandırıldığından emin olun.


### <a name="error-code--2353"></a>Hata kodu: 2353

- **Mesaj**:`The script storage has not been initialized. Please check the properties of the script storage linked service in the HDI activity.`

- **Neden**: Komut dosyası depolamabağlantılı hizmet özellikleri doğru ayarlanmaz.

- **Öneri**: Lütfen komut dosyası depolamabağlantılı hizmetin özelliklerinin düzgün şekilde yapılandırıldığından emin olun.


### <a name="error-code--2354"></a>Hata kodu: 2354

- **Mesaj**:`The storage linked service type '%linkedServiceType;' is not supported for '%executorType;' activities for property '%linkedServicePropertyName;'.`

- **Neden**: Depolamaya bağlı hizmet türü etkinlik tarafından desteklenmez.

- **Öneri**: Lütfen seçili bağlantılı hizmetin etkinlik için desteklenen türlerden birine sahip olduğundan emin olun. HDI etkinlikleri AzureBlobStorage ve AzureBlobFSStorage bağlantılı hizmetleri destekler.


### <a name="error-code--2355"></a>Hata kodu: 2355

- **Mesaj**:`The '%value' provided for commandEnvironment is incorrect. The expected value should be an array of strings where each string has the format CmdEnvVarName=CmdEnvVarValue.`

- **Neden**: Çevre komutu için sağlanan yanlıştır.

- **Tavsiye**:  
      Lütfen sağlanan değerin aşağıdakine \"benzer\"olduğundan \"emin olun:\" commandEnvironment : [ variableName=variableValue ] Ve her değişken listede yalnızca bir kez görünür.
    


### <a name="error-code--2356"></a>Hata kodu: 2356

- **Mesaj**:`The commandEnvironment already contains a variable named '%variableName;'.`

- **Neden**: Değişken, Çevre komutunda iki kez sağlanmıştır.

- **Tavsiye**:  
      Lütfen sağlanan değerin aşağıdakine \"benzer\"olduğundan \"emin olun:\" commandEnvironment : [ variableName=variableValue ] Ve her değişken listede yalnızca bir kez görünür.
    


### <a name="error-code--2357"></a>Hata kodu: 2357

- **Mesaj**:`The certificate or password is wrong for ADLS Gen 1 storage.`

- **Neden**: Sağlanan kimlik bilgileri yanlış.

- **Öneri**: Lütfen ADLS Gen 1 bağlantılı servisteki bağlantı bilgilerini doğrulayın ve test bağlantısının başarılı olduğundan emin olun.


### <a name="error-code--2358"></a>Hata kodu: 2358

- **Mesaj**:`The value '%value;' for the required property 'TimeToLive' in the on demand HDInsight linked service '%linkedServiceName;' has invalid format. It should be a timespan between '00:05:00' and '24:00:00'.`

- **Neden**: Gerekli özellik 'TimeToLive' için sağlanan değer geçersiz biçimivardır. 

- **Öneri**: Lütfen önerilen aralıkta olması gereken değeri güncelleyin ve yeniden deneyin.


### <a name="error-code--2359"></a>Hata kodu: 2359

- **Mesaj**:`The value '%value;' for the property 'roles' is invalid. Expected types are 'zookeeper', 'headnode', and 'workernode'.`

- **Neden**: Özellik 'rolleri' için sağlanan değer geçersizdir.

- **Öneri**: Lütfen önerilerden biri olacak değeri güncelleyin ve yeniden deneyin.


### <a name="error-code--2360"></a>Hata kodu: 2360

- **Mesaj**:`The connection string in HCatalogLinkedService is invalid. Encountered an error while trying to parse: '%message;'.`

- **Neden**: HCatalogLinkedService için sağlanan bağlantı dizesi geçersizdir.

- **Öneri**: Lütfen değeri doğru bir Azure SQL bağlantı dizesine güncelleyin ve yeniden deneyin.


### <a name="error-code--2361"></a>Hata kodu: 2361

- **Mesaj**:`Failed to create on demand HDI cluster. Cluster name is '%clusterName;'.`

- **Neden**: Küme oluşturma başarısız oldu ve ADF HDInsight hizmetinden bir hata geri almadı.

- **Öneri**: Azure portalını açın ve sağlanan adla HDI kaynağını bulmaya çalışın ve sağlama durumunu denetleyin. Daha fazla yardım için HDInsight destek ekibine başvurun.


### <a name="error-code--2362"></a>Hata kodu: 2362

- **Mesaj**:`Only Azure Blob storage accounts are supported as additional storages for HDInsight on demand linked service.`

- **Neden**: Sağlanan ek depolama Azure Blob depolama değildi.

- **Öneri**: Azure Blob depolama hesabını isteğe bağlı hizmette HDInsight için ek bir depolama alanı olarak sağlayın.



## <a name="web-activity"></a>Web Etkinliği

### <a name="error-code--2128"></a>Hata kodu: 2128

- **Mesaj**:`No response from the endpoint. Possible causes: network connectivity, DNS failure, server certificate validation or timeout.`

- **Neden**: Ağ bağlantısı, DNS hatası, sunucu sertifikası doğrulama veya zaman acısı.

- **Öneri**: Vurmaya çalıştığınız bitiş noktasının isteklere yanıt verdiğini doğrulayın. Fiddler/Postman gibi araçları kullanabilirsiniz.


### <a name="error-code--2108"></a>Hata kodu: 2108

- **Mesaj**:`Error calling the endpoint '%url;'. Response status code: '%code;'`

- **Neden**: Ağ bağlantısı, DNS hatası, sunucu sertifikası doğrulama veya zaman acısı gibi altta yatan bir sorun nedeniyle istek başarısız oldu.

- **Öneri**: İsteği doğrulamak için Fiddler/Postman'ı kullanın.
<br>


#### <a name="more-details"></a>Daha fazla ayrıntı’yı seçin
İzlenen web uygulamasının HTTP oturumunu oluşturmak için Fiddler'ı kullanmak için:

1. [Fiddler'ı](https://www.telerik.com/download/fiddler)indirin, kurun ve açın.

1. Web uygulamanız HTTPS kullanıyorsa, **Tools** > **Fiddler Options** > **HTTPS**adresine gidin. **HTTPS'yi Yakalayı** ve **HTTPS trafiğini çöz'ü**seçin.

   ![Kemancı seçenekleri](media/data-factory-troubleshoot-guide/fiddler-options.png)

1. Uygulamanız TLS/SSL sertifikaları kullanıyorsa, Cihazınıza Fiddler sertifikasını ekleyin. **Araçlar** > **Fiddler Seçenekleri** > **HTTPS** > **Eylemler** > **Masaüstüne Kök Sertifikası Dışa aktarın**gidin.

1. **Dosya** > **Yakalama Trafiği'ne**giderek yakalamayı kapatın. Veya **F12 tuşuna**basın.

1. Önbelleğe alınan tüm öğelerin kaldırılacak ve yeniden indirilmesi için tarayıcınızın önbelleğini temizleyin.

1. İstek oluşturma:

   1. **Besteci** sekmesini seçin.

   1. HTTP yöntemini ve URL'sini ayarlayın.
   
   1. Gerekirse üstbilgi ve istek gövdesi ekleyin.

   1. **Yürüt**’ü seçin.

1. Trafik yakalamayı yeniden açın ve sayfanızdaki sorunlu işlemi tamamlayın.

1. **Tüm Oturumları****Kaydet** >  **Dosyaya** > Gidin.

Daha fazla bilgi için Bkz. [Fiddler ile başlarken.](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureFiddler)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorun giderme yardımı için şu kaynakları deneyin:

*  [Veri Fabrikası günlüğü](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Veri Fabrikası özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [MSDN forumu](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Veri Fabrikası için Yığın Taşma forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Veri Fabrikası hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)


            
