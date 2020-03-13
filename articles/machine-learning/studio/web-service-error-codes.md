---
title: REST API hata kodları-Azure Machine Learning Studio (klasik) | Microsoft Docs
description: Bu hata kodları, bir Azure Machine Learning web hizmeti üzerinde bir işlem tarafından döndürülebilir.
keywords: ''
services: machine-learning
author: xiaoharper
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: 0923074b-3728-439d-a1b8-8a7245e39be4
ms.service: machine-learning
ms.subservice: studio
ms.topic: reference
ms.date: 11/16/2016
ms.openlocfilehash: 9e25f2fbc10eb07cc71f2a7bd34247c0191f61b9
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79217768"
---
# <a name="azure-machine-learning-studio-classic-rest-api-error-codes"></a>Azure Machine Learning Studio (klasik) REST API hata kodları

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]
 
Aşağıdaki hata kodları Azure Machine Learning Studio (klasik) Web hizmetindeki bir işlem tarafından döndürülebilir.
 
## <a name="badargument-http-status-code-400"></a>BadArgument (HTTP durum kodu 400)
 
Sağlanan geçersiz bağımsız değişken.
 
Bu sınıf hataların yere sağlanan bağımsız değişken geçersiz anlamına gelir. Bu kimlik bilgileri veya bir web hizmetine geçirilen Azure depolama konumunu olabilir. Hangi belirli bağımsız değişkenin geçersiz olduğunu tanılamak için lütfen "Ayrıntılar" bölümündeki hata "Code" alanına bakın.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| BadParameterValue | Sağlanan parametre değeri parametre parametresi kural karşılamaz |
| BadSubscriptionId | Abonelik puanlamak için kullanılan kimliği kaynakta mevcut değil |
| BadVersionCall | API çağrısı sırasında geçersiz sürüm parametresi geçirildi: {0}. Doğru sürüm geçirmek için API Yardım sayfası denetleyin ve yeniden deneyin. |
| BatchJobInputsNotSpecified | Şu gerekli giriş (ler) istekle birlikte belirtilmedi: {0}. Lütfen olun tüm giriş verilerini belirtilir ve yeniden deneyin. |
| BatchJobInputsTooManySpecified | İstek, hizmette tanımlanan çok daha fazla giriş belirtilen. Kabul edilen giriş (lar) listesi: {0}. Lütfen olun tüm giriş verilerini doğru belirtildiğinden ve yeniden deneyin. |
| BlobNameTooLong | Tanılama çıktısı için belirtilen Azure Blob depolama yolu çok uzun: {0}. Yolunu kısaltın ve yeniden deneyin. |
| BlobNotFound | Belirtilen Azure Blob 'a erişilemiyor-{0}.  Azure hata iletisi: {1}. |
| ContainerIsEmpty | Hiçbir Azure depolama kapsayıcısı adı sağlanmadı. Geçerli kapsayıcı adı sağlayın ve tekrar deneyin. |
| ContainerSegmentInvalid | Kapsayıcı adı geçersiz. Geçerli kapsayıcı adı sağlayın ve tekrar deneyin. |
| ContainerValidationFailed | Blob kapsayıcısı doğrulaması şu hatayla başarısız oldu: {0}. |
| DataTypeNotSupported | Sağlanan desteklenmeyen veri türü. Geçerli veri türleri sağlayın ve tekrar deneyin. |
| DuplicateInputInBatchCall | Toplu istek geçerli değil. Tek ve çoklu giriş, aynı anda belirtilemez. Bu öğelerden birini istekten kaldırın ve yeniden deneyin. |
| ExpiryTimeInThePast | Belirtilen sona erme saati geçmişte: {0}. Gelecekteki bitiş zamanı, UTC sağlayın ve tekrar deneyin. Süresiz olarak süre sonu değeri NULL olarak ayarlayın. |
| IncompleteSettings | Tanılama ayarları eksik. |
| InputBlobRelativeLocationInvalid | Azure depolama blob adı sağlandı. Geçerli bir blob adı sağlayın ve tekrar deneyin. |
| InvalidBlob | Blob için geçersiz blob belirtimi: {0}. Bu bağlantı dizesini doğrulayın / göreli yol veya SAS belirteci belirtimi doğru olduğundan ve yeniden deneyin. |
| InvalidBlobConnectionString | Giriş/çıkış Bloblarından biri için belirtilen bağlantı dizesi geçersiz: {0}. Lütfen bunu düzeltin ve yeniden deneyin. |
| InvalidBlobExtension | Blob başvurusu: {0} geçersiz veya eksik bir dosya uzantısına sahip. Bu çıkış türü için desteklenen dosya uzantıları şunlardır: "{1}". |
| InvalidInputNames | İstekte geçersiz hizmet girdisi adları belirtildi: {0}. Lütfen doğru hizmet girişleri için giriş verilerini eşleştirin ve yeniden deneyin. |
| InvalidOutputOverrideName | Geçersiz çıkış geçersiz kılma adı: {0}. Hizmet, bu ada sahip bir çıkış düğümü yok. Lütfen geçersiz kılmak için bir doğru çıkış düğümü adı olarak oldfashionedgoat (büyük/küçük harfe duyarlılık geçerlidir). |
| InvalidQueryParameter | '{0}' sorgu parametresi geçersiz. {1} |
| MissingInputBlobInformation | Azure depolama blob bilgisi eksik. Geçerli bağlantı dizesi ve göreli yol veya URI sağlayın ve tekrar deneyin. |
| MissingJobId | İş Kimliği sağlanmadı. Bir işi ilk kez gönderildiğinde bir iş kimliği döndürülür. ' % S'proje kimliği doğru olduğunu doğrulayın ve yeniden deneyin. |
| MissingKeys | Sağlanan anahtar yok veya bir birincil veya ikincil anahtarı sağlanmadı. |
| MissingModelPackage | Model paket kimliği veya sağlanan model paketi yok. Geçerli model paket kimliği belirtin veya paket model ve yeniden deneyin. |
| MissingOutputOverrideSpecification | İsteğin çıkış geçersiz kılma {0}için blob belirtimi eksik. Lütfen geçerli bir blob konumuna istekle belirtin veya konumu geçersiz kılma isteniyorsa çıkış belirtimini kaldırın. |
| MissingRequestInput | Web hizmeti giriş bekliyor, ancak herhangi bir giriş sağlanmadı. Geçerli giriş modelinde yayımlanan giriş noktalarına dayalı olduğundan emin olun ve yeniden deneyin. |
| MissingRequiredGlobalParameters | Tüm web hizmeti parametreleri sağlanan gereklidir. Modüller için beklenen parametreyle doğru olduğundan emin olun ve yeniden deneyin. |
| MissingRequiredOutputOverrides | Çıkış geçirin zorunludur bir şifrelenmiş hizmet uç noktası çağrısı yapıldığında hizmetin tüm çıkışlar için geçersiz kılar. Şu anda bu çıktılar için geçersiz kılmalar eksik: {0} |
| MissingWebServiceGroupId | Hiçbir web hizmeti grubu kimliği sağlanmadı. Geçerli web hizmet grubu kimliği sağlayın ve tekrar deneyin. |
| MissingWebServiceId | Hiçbir web hizmeti Kimliği sağlanmadı. Geçerli web hizmeti kimliği sağlayın ve tekrar deneyin. |
| MissingWebServicePackage | Web hizmeti paketi sağlanan. Geçerli web hizmeti paketi sağlayın ve tekrar deneyin. |
| MissingWorkspaceId | Çalışma alanı Kimliği sağlanmadı. Geçerli çalışma alanı kimliği sağlayın ve tekrar deneyin. |
| ModelConfigurationInvalid | Model paketi yapılandırmasında geçersiz model. Çıkış uç noktası tanımı, standart hata uç noktası, model yapılandırmasını içerir ve std uç noktası ve yeniden deneyin emin olun. |
| ModelPackageIdInvalid | Geçersiz model paket kimliği. model paket kimliğinin doğru olduğundan emin olun ve yeniden deneyin. |
| RequestBodyInvalid | Hiçbir istek gövdesinde sağlanan veya istek gövdesi seri durumdan çıkarılırken hata oluştu. |
| RequestIsEmpty | Sağlanan istek. Geçerli bir isteği sağlayın ve tekrar deneyin. |
| UnexpectedParameter | Beklenmeyen parametresi belirtilmedi. Tüm parametre adları doğru yazıldığından, yalnızca beklenen parametrelerin geçirilir ve yeniden deneyin doğrulayın. |
| Başvuruları | Bilinmeyen hata. |
| UserParameterInvalid | {0} |
| WebServiceConcurrentRequestRequirementInvalid | {0} Web hizmeti için eşzamanlı istek gereksinimleri değiştirilemiyor. |
| WebServiceIdInvalid | Sağlanan geçersiz web hizmeti kimliği. Web hizmeti kimliği geçerli bir GUID olmalıdır. |
| WebServiceTooManyConcurrentRequestRequirement | Eşzamanlı istek gereksinimi {0}fazlasına ayarlanamaz. |
| WebServiceTypeInvalid | Sağlanan geçersiz web hizmeti türü. Geçerli web hizmeti türü doğru olduğunu doğrulayın ve yeniden deneyin. Geçerli Web hizmeti türleri: {0}. |
 
## <a name="baduserargument-http-status-code-400"></a>BadUserArgument (HTTP durum kodu 400)
 
Sağlanan geçersiz kullanıcı bağımsız değişkeni.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| InputMismatchError | Giriş verilerini giriş bağlantı noktasını şemayla eşleşmiyor. |
| InputParseError | Giriş vektörü başarısız ayrıştırma.  Sütunları ve türleri doğru sayıda giriş vektör olduğunu doğrulayın.  Ek ayrıntılar: {0}. |
| MissingRequiredGlobalParameters | Web hizmeti tarafından beklenen parametre eksik. Web hizmeti tarafından beklenen tüm gerekli parametreleri doğru olduğundan emin olun ve yeniden deneyin. |
| UnexpectedParameter | Doğrulayın. yalnızca web hizmeti tarafından beklenen gerekli parametreleri geçirilir ve yeniden deneyin. |
| UserParameterInvalid | {0} |
 
## <a name="invalidoperation-http-status-code-400"></a>Invalidoperation (HTTP durum kodu 400)
 
İstek, geçerli bağlamda geçersiz.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| CannotStartJob | İş {0} durumunda olduğundan başlatılamıyor. |
| IncompatibleModel | Model isteği sürümü ile uyumlu değil. İstek sürümü, yalnızca tek bir datatable çıkış modelleri destekler. |
| MultipleInputsNotAllowed | Model, birden çok giriş izin vermez. |
 
## <a name="libraryexecutionerror-http-status-code-400"></a>LibraryExecutionError (HTTP durum kodu 400)
 
Modül yürütme iç kitaplığı hatayla karşılaştı.
 
 
## <a name="moduleexecutionerror-http-status-code-400"></a>ModuleExecutionError (HTTP durum kodu 400)
 
Modül yürütme bir hatayla karşılaştı.
 
 
## <a name="webservicepackageerror-http-status-code-400"></a>WebServicePackageError (HTTP durum kodu 400)
 
Geçersiz web hizmeti paketi. Sağlanan web hizmeti paketi doğru olduğunu doğrulayın ve yeniden deneyin.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| FormatError | Web hizmeti paketi hatalı biçimlendirilmiş. Ayrıntılar: {0} |
| RuntimesError | Web hizmeti paketi graf geçersiz. Ayrıntılar: {0} |
| ValidationError | Web hizmeti paketi graf geçersiz. Ayrıntılar: {0} |
 
## <a name="unauthorized-http-status-code-401"></a>Yetkisiz (HTTP durum kodu 401)
 
Yetkisiz erişim kaynağa isteğidir.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| AdminRequestUnauthorized | Yetkisiz |
| ManagementRequestUnauthorized | Yetkisiz |
| ScoreRequestUnauthorized | Geçersiz kimlik bilgileri sağlandı. |
 
## <a name="notfound-http-status-code-404"></a>Bulunamadı (HTTP durum kodu 404)
 
Kaynak bulunamadı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| ModelPackageNotFound | Model paketi bulunamadı. Model paket kimliği doğru olduğunu doğrulayın ve yeniden deneyin. |
| WebServiceIdNotFoundInWorkspace | Web hizmeti altında bu çalışma alanı bulunamadı. WebServiceId ve çalışma alanı kimliği arasında bir uyumsuzluk var. Sağlanan web hizmeti çalışma alanında bir parçası olduğunu doğrulayın ve yeniden deneyin. |
| WebServiceNotFound | Web hizmeti bulunamadı. Web hizmeti kimliği doğru olduğunu doğrulayın ve yeniden deneyin. |
| WorkspaceNotFound | Çalışma alanı bulunamadı. Çalışma alanı kimliği doğru olduğunu doğrulayın ve yeniden deneyin. |
 
## <a name="requesttimeout-http-status-code-408"></a>RequestTimeout (HTTP durum kodu 408)
 
İşlem izin verilen sürede tamamlanamadı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| RequestCanceled | İstek, istemci tarafından iptal edildi. |
| ScoreRequestTimeout | Yürütme isteği zaman aşımına uğradı. |
 
## <a name="conflict-http-status-code-409"></a>Çakışma (HTTP durum kodu 409)
 
Kaynak zaten var.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| ModelOutputMetadataMismatch | Geçersiz çıkış parametresi adı. Sütunları yeniden adlandırma ve yeniden denemek için meta verileri Düzenleyicisi modülü kullanmayı deneyin. |
 
## <a name="memoryquotaviolation-http-status-code-413"></a>MemoryQuotaViolation (HTTP durum kodu 413)
 
Model, kendisine atanan bellek kotasını aşmıştı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| OutOfMemoryLimit | Model için tahsis bellekten daha fazlasını tüketilen. Model için izin verilen en yüksek bellek {0} MB 'tır. Modelinizi sorunları gözden geçirin. |
 
## <a name="internalerror-http-status-code-500"></a>Internalerror (HTTP durum kodunu 500)
 
Yürütme bir iç hatayla karşılaştı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| AdminAuthenticationFailed |  |
| BackendArgumentError |  |
| BackendBadRequest |  |
| ClusterConfigBlobMisconfigured |  |
| ContainerProcessTerminatedWithSystemError | Sistem hatası ile kapsayıcı işlemi kilitlendi |
| ContainerProcessTerminatedWithUnknownError | Bilinmeyen bir hata ile kapsayıcı işlemi kilitlendi |
| ContainerValidationFailed | Blob kapsayıcısı doğrulaması şu hatayla başarısız oldu: {0}. |
| DeleteWebServiceResourceFailed |  |
| ExceptionDeserializationError |  |
| FailedGettingApiDocument |  |
| FailedStoringWebService |  |
| InvalidMemoryConfiguration | Invalidmemoryconfiguration, ConfigValue: {0} |
| InvalidResourceCacheConfiguration |  |
| InvalidResourceDownloadConfiguration |  |
| InvalidWebServiceResources |  |
| MissingTaskInstance | Hiç bağımsız değişken sağlanmadı. Doğrulayın. geçerli bağımsız değişkenler geçirilir ve yeniden deneyin. |
| ModelPackageInvalid |  |
| ModuleExecutionFailed |  |
| ModuleLoadFailed |  |
| ModuleObjectCloneFailed |  |
| OutputConversionFailed |  |
| PortDataTypeNotSupported | Bağlantı noktası kimliği ={0} desteklenmeyen bir veri türüne sahip: {1}. |
| ResourceDownload |  |
| ResourceLoadFailed |  |
| ServiceUrisNotFound |  |
| SwaggerGeneration | Swagger oluşturma başarısız oldu, Ayrıntılar: {0} |
| UnexpectedScoreStatus |  |
| UnknownBackendErrorResponse |  |
| Başvuruları |  |
| UnknownJobStatusCode | Bilinmeyen iş durum kodu {0}. |
| UnknownModuleError |  |
| UpdateWebServiceResourceFailed |  |
| WebServiceGroupNotFound |  |
| WebServicePackageInvalid | Invalidwebservicepackage, Ayrıntılar: {0} |
| WorkerAuthorizationFailed |  |
| WorkerUnreachable |  |
 
## <a name="internalerrorsystemlowonmemory-http-status-code-500"></a>InternalErrorSystemLowOnMemory (HTTP durum kodunu 500)
 
Yürütme bir iç hatayla karşılaştı. Sistem belleği yetersiz. Lütfen yeniden deneyin.
 
 
## <a name="modelpackageformaterror-http-status-code-500"></a>ModelPackageFormatError (HTTP durum kodunu 500)
 
Geçersiz model paketi. Sağlanan model paketi doğru olduğunu doğrulayın ve yeniden deneyin.
 
 
## <a name="webservicepackageinternalerror-http-status-code-500"></a>WebServicePackageInternalError (HTTP durum kodunu 500)
 
Geçersiz web hizmeti paketi. Sağlanan web paketi doğru olduğunu doğrulayın ve yeniden deneyin.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| ModuleError | Web hizmeti paketi graf geçersiz. Ayrıntılar: {0} |
 
## <a name="initializingcontainers-http-status-code-503"></a>InitializingContainers (HTTP durum kodu 503)
 
Kapsayıcıları başlatıldı olarak istek yürütülemiyor.
 
 
## <a name="serviceunavailable-http-status-code-503"></a>ServiceUnavailable (HTTP durum kodu 503)
 
Hizmet geçici olarak kullanılamıyor.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| NoMoreResources | İstek için kullanılabilir kaynaklar yoktur. |
| RequestThrottled | {0} uç noktası için istek kısıtlandı. Uç nokta için en fazla eşzamanlılık {1}. |
| TooManyConcurrentRequests | Çok fazla eş zamanlı istek gönderildi. |
| TooManyHostsBeingInitialized | Çok fazla konak aynı anda başlatılmış. Azaltma / yeniden deneniyor göz önünde bulundurun. |
| TooManyHostsBeingInitializedPerModel | Çok fazla konak aynı anda başlatılmış. Azaltma / yeniden deneniyor göz önünde bulundurun. |
 
## <a name="gatewaytimeout-http-status-code-504"></a>GatewayTimeout (HTTP durum kodu 504)
 
İşlem izin verilen sürede tamamlanamadı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| BackendInitializationTimeout | Web hizmeti başlatma, izin verilen sürede tamamlanamadı. |
| BackendScoreTimeout | Web hizmeti isteği yürütülmesine izin verilen sürede tamamlanamadı. |
 
