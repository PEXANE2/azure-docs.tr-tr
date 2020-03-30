---
title: REST API hata kodları - Azure Machine Learning Studio (klasik) | Microsoft Dokümanlar
description: Bu hata kodları, Azure Machine Learning web hizmetindeki bir işlemle döndürülebilir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79217768"
---
# <a name="azure-machine-learning-studio-classic-rest-api-error-codes"></a>Azure Machine Learning Studio (klasik) REST API Hata Kodları

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]
 
Aşağıdaki hata kodları, Azure Machine Learning Studio (klasik) web hizmetindeki bir işlemle döndürülebilir.
 
## <a name="badargument-http-status-code-400"></a>BadArgument (HTTP durum kodu 400)
 
Geçersiz bağımsız değişken sağlandı.
 
Bu hata sınıfı, bir yerde sağlanan bir bağımsız değişkenin geçersiz olduğu anlamına gelir. Bu, Azure depolama alanının web hizmetine aktarılan bir şeye ait bir kimlik bilgisi veya konumu olabilir. Lütfen hangi özel bağımsız değişkenin geçersiz olduğunu tanılamak için "ayrıntılar" bölümündeki "kod" bölümüne bakın.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| BadParameterValue | Sağlanan parametre değeri parametre üzerindeki parametre kuralını karşılamaz |
| BadSubscriptionId | Puan lamak için kullanılan abonelik kimliği kaynaktaki mevcut kimlik |
| BadVersionCall | Geçersiz sürüm parametresi API çağrısı {0}sırasında geçirildi: . Doğru sürümü geçmek için API yardım sayfasını kontrol edin ve yeniden deneyin. |
| BatchJobInputsNotSpecified | Aşağıdaki gerekli giriş(ler) istek ile belirtilmedi: {0}. Lütfen tüm giriş verilerinin belirtildiğinden emin olun ve yeniden deneyin. |
| BatchJobInputsTooManySpecified | İstek, hizmette tanımlanandan daha fazla giriş belirtti. Kabul edilen giriş(ler) {0}listesi: . Lütfen tüm giriş verilerinin doğru şekilde belirtildiğinden emin olun ve yeniden deneyin. |
| BlobNameTooLong | Tanılama çıktısı için sağlanan azure blob depolama yolu çok uzundur: {0}. Yolu kısaltın ve yeniden deneyin. |
| BlobNotFound | Sağlanan Azure blob erişemiyor {0}- .  Azure hata {1}iletisi: . |
| Konteyner Boş | Azure depolama kapsayıcı adı sağlanmadı. Geçerli bir kapsayıcı adı sağlayın ve yeniden deneyin. |
| KonteynerSegmentGeçersiz | Geçersiz kapsayıcı adı. Geçerli bir kapsayıcı adı sağlayın ve yeniden deneyin. |
| Konteyner DoğrulamaBaşarısız | Blob kapsayıcı doğrulama bu hata {0}ile başarısız oldu: . |
| DataTypeNotDesteklenen | Desteklenmeyen veri türü sağlandı. Geçerli veri türü(ler) sağlayın ve yeniden deneyin. |
| DuplicateInputInBatchCall | Toplu iş isteği geçersizdir. Aynı anda hem tek hem de birden çok giriş belirtemez. Bu öğelerden birini istekten çıkarın ve yeniden deneyin. |
| ExpiryTimeInThePast | Verilen son kullanma süresi geçmişte {0}kaldı: . UTC'de ileride bir son kullanma süresi sağlayın ve yeniden deneyin. Süresinin dolmaması için son kullanma tarihini NULL olarak ayarlayın. |
| Eksik Ayarlar | Tanılama ayarları tamamlanmaz. |
| GirişBlobRelativeLocationGeçersiz | Azure depolama blob adı sağlanmadı. Geçerli bir blob adı sağlayın ve yeniden deneyin. |
| GeçersizBlob | Blob için geçersiz blob {0}belirtimi: . Bağlantı dizesi / göreli yol veya SAS belirteç belirtimidoğru olduğunu doğrulayın ve yeniden deneyin. |
| GeçersizBlobConnectionString | Geçersiz giriş/çıkış lekelerinden biri için belirtilen bağlantı dizesi: {0}. Lütfen bunu düzeltin ve tekrar deneyin. |
| GeçersizBlobExtension | Blob başvurusu: {0} geçersiz veya eksik dosya uzantısı vardır. Bu çıktı türü için desteklenen dosya{1}uzantıları şunlardır: " ". |
| InvalidInputNames | İstekte belirtilen geçersiz hizmet giriş adı(lar) : {0}. Lütfen giriş verilerini doğru hizmet girişleriyle eşleyin ve yeniden deneyin. |
| GeçersizOutputOverrideName | Geçersiz çıktı geçersiz kılma {0}adı: . Hizmetin bu ada sahip bir çıkış düğümü yok. Geçersiz kılmak için lütfen doğru çıktı düğümü adını geçirin (büyük/küçük harf duyarlılığı uygulanır). |
| GeçersizQueryParametresi | Geçersiz sorgu parametresi '{0}'. {1} |
| EksikInputBlobBilgi | Eksik Azure depolama blob bilgileri. Geçerli bir bağlantı dizesi ve göreli yol veya URI sağlayın ve yeniden deneyin. |
| EksikJobid | İş kimliği verilmedi. Bir iş kimliği, bir iş ilk kez gönderildiğinde döndürülür. İş Kimliğinin doğru olduğunu doğrulayın ve yeniden deneyin. |
| Eksik Anahtarlar | Anahtar sağlanmadı veya Birincil veya İkincil Anahtardan biri sağlanmadı. |
| Eksik Model Paketi | Model paketi Kimliği veya model paketi sağlanmadı. Geçerli bir model paketi Kimliği veya model paketi sağlayın ve yeniden deneyin. |
| Eksik OutputOverrideSpecification | İstek çıkış geçersiz kılma {0}için blob belirtimi eksik. Lütfen istekle birlikte geçerli bir blob konumu belirtin veya konum geçersiz kılma İstenmiyorsa çıktı belirtimini kaldırın. |
| Eksikİstek Girişi | Web hizmeti bir giriş bekler, ancak giriş sağlanmaz. Modeldeki yayımlanmış giriş bağlantı noktalarına göre geçerli girdilerin sağlandığından emin olun ve yeniden deneyin. |
| Eksik GerekliKüresel Parametreler | Gerekli tüm web hizmeti parametresi(ler) sağlanmadı. Modül(ler) için beklenen parametrenin doğru olduğunu doğrulayın ve yeniden deneyin. |
| Eksik Gerekli OutputOverrides | Şifreli bir hizmet bitiş noktasını ararken, tüm hizmet çıktıları için çıktı geçersiz kılmaları geçmek zorunludur. Bu çıktılar için şu anda eksik geçersiz kılar:{0} |
| Eksik WebServiceGroupId | Web servis grubu Kimliği sağlanmadı. Geçerli bir web hizmet grubu Kimliği sağlayın ve yeniden deneyin. |
| Eksik WebServiceid | Web hizmeti Kimliği sağlanmadı. Geçerli bir web hizmeti Kimliği sağlayın ve yeniden deneyin. |
| Eksik WebServicePackage | Web Hizmeti paketi sağlanmadı. Geçerli bir web servis paketi sağlayın ve yeniden deneyin. |
| EksikÇalışma AlanıId | Çalışma alanı kimliği sağlanmadı. Geçerli bir çalışma alanı kimliği sağlayın ve yeniden deneyin. |
| ModelYapılandırmaGeçersiz | Model paketinde geçersiz model yapılandırması. Model yapılandırmasının çıktı uç noktası(lar) tanımı, std hatası bitiş noktası ve std out endpoint ve yeniden deneyin içerir emin olun. |
| ModelPackageIdGeçersiz | Geçersiz model paketi Id. Model paketi Kimliğinin doğru olduğunu doğrulayın ve yeniden deneyin. |
| İstekBodyGeçersiz | İstek gövdesi nin serisini silmede hiçbir istek gövdesi veya hata sağlanmadı. |
| İstekBoş | İstek sağlanmadı. Geçerli bir istek sağlayın ve yeniden deneyin. |
| Beklenmeyen Parametre | Beklenmeyen parametreler sağlandı. Tüm parametre adlarının doğru yazıldığından, yalnızca beklenen parametrelerin geçirildiğini doğrulayın ve yeniden deneyin. |
| UnknownError | Bilinmeyen hata. |
| UserParameterGeçersiz | {0} |
| WebServiceConcurrentRequestRequirementGeçersiz | Web hizmeti için {0} eşzamanlı istek gereksinimlerini değiştiremez. |
| WebServiceIdGeçersiz | Geçersiz web hizmeti kimliği sağlandı. Web hizmeti kimliği geçerli bir kılavuz olmalıdır. |
| WebServiceTooManyConcurrentRequestRequirement | Eşzamanlı istek gereksinimini ' {0}den fazlasına ayarlayamaz. |
| WebServiceTypeGeçersiz | Geçersiz web hizmeti türü sağlandı. Geçerli web hizmet türünün doğru olduğunu doğrulayın ve yeniden deneyin. Geçerli web hizmet {0}türleri: . |
 
## <a name="baduserargument-http-status-code-400"></a>BadUserArgument (HTTP durum kodu 400)
 
Geçersiz kullanıcı bağımsız değişkeni sağlandı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| InputMismatchError | Giriş verileri giriş noktası şemasıyla eşleşmiyor. |
| GirişParseError | Giriş vektörünün ayrışması başarısız oldu.  Giriş vektörünün doğru sayıda sütun ve veri türüne sahip olduğunu doğrulayın.  Ek ayrıntılar: {0}. |
| Eksik GerekliKüresel Parametreler | Web hizmeti tarafından beklenen parametre(ler) eksik. Web hizmeti tarafından beklenen tüm gerekli parametrelerin doğru olduğunu doğrulayın ve yeniden deneyin. |
| Beklenmeyen Parametre | Yalnızca web hizmeti tarafından beklenen gerekli parametrelerin geçirildiğini doğrulayın ve yeniden deneyin. |
| UserParameterGeçersiz | {0} |
 
## <a name="invalidoperation-http-status-code-400"></a>Geçersiz Çalışma (HTTP durum kodu 400)
 
İstek geçerli bağlamda geçersizdir.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| İşe Başlayamaması | {0} İş, eyalette olduğu için başlatılamaz. |
| UyumsuzModel | Model istek sürümüile uyumsuzdur. İstek sürümü yalnızca tek bir veri tablosu çıktı modellerini destekler. |
| MultipleInputsNotAllowed | Model birden çok girişe izin vermez. |
 
## <a name="libraryexecutionerror-http-status-code-400"></a>LibraryExecutionError (HTTP durum kodu 400)
 
Modül yürütme bir iç kitaplık hatası yla karşılaştı.
 
 
## <a name="moduleexecutionerror-http-status-code-400"></a>ModuleExecutionError (HTTP durum kodu 400)
 
Modül yürütme bir hata yla karşılaştı.
 
 
## <a name="webservicepackageerror-http-status-code-400"></a>WebServicePackageError (HTTP durum kodu 400)
 
Geçersiz web servis paketi. Sağlanan web hizmet paketinin doğru olduğunu doğrulayın ve yeniden deneyin.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| Biçim Hatası | Web servis paketi bozuk. Şey:{0} |
| RuntimesError | Web hizmeti paket grafiği geçersizdir. Şey:{0} |
| Validationerror | Web hizmeti paket grafiği geçersizdir. Şey:{0} |
 
## <a name="unauthorized-http-status-code-401"></a>Yetkisiz (HTTP durum kodu 401)
 
Kaynağa erişmek için istek yetkisizdir.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| AdminRequestYetkisiz | Yetkisiz |
| YönetimİsteğIYetkisiz | Yetkisiz |
| ScoreRequestYetkisiz | Geçersiz kimlik bilgileri sağlandı. |
 
## <a name="notfound-http-status-code-404"></a>NotFound (HTTP durum kodu 404)
 
Kaynak bulunamadı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| ModelPackageNotFound | Model paketi bulunamadı. Model paketi Kimliğinin doğru olduğunu doğrulayın ve yeniden deneyin. |
| WebServiceIdNotFoundInWorkspace | Bu çalışma alanı altında Web hizmeti bulunamadı. webServiceId ve workspaceId arasında bir uyumsuzluk vardır. Sağlanan web hizmetinin çalışma alanının bir parçası olduğunu doğrulayın ve yeniden deneyin. |
| WebServiceNotFound | Web hizmeti bulunamadı. Web hizmeti Kimliği'nin doğru olduğunu doğrulayın ve yeniden deneyin. |
| Çalışma AlanıNotFound | Çalışma alanı bulunamadı. Çalışma alanı kimliğinin doğru olduğunu doğrulayın ve yeniden deneyin. |
 
## <a name="requesttimeout-http-status-code-408"></a>İstekZaman (HTTP durum kodu 408)
 
İşlem izin verilen süre içinde tamamlanamadı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| İstekİptal edildi | İstek istemci tarafından iptal edildi. |
| ScoreRequestTimeout | Yürütme isteği zaman doldu. |
 
## <a name="conflict-http-status-code-409"></a>Çakışma (HTTP durum kodu 409)
 
Kaynak zaten var.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| ModelOutputMetadataMismatch | Geçersiz çıktı parametresi adı. Sütunları yeniden adlandırmak ve yeniden denemek için meta veri düzenleyicisi modüllerini kullanmayı deneyin. |
 
## <a name="memoryquotaviolation-http-status-code-413"></a>MemoryQuotaViolation (HTTP durum kodu 413)
 
Model, kendisine atanan bellek kotasını aşmıştı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| OutofmemoryLimit | Model, ona tahsis edilenden daha fazla bellek tüketti. Model için izin verilen {0} maksimum bellek MB'dir. Sorunlar için lütfen modelinizi kontrol edin. |
 
## <a name="internalerror-http-status-code-500"></a>InternalError (HTTP durum kodu 500)
 
Yürütme bir iç hata yla karşılaştı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| AdminAuthenticationFailed |  |
| BackendArgumentHatası |  |
| BackendBadRequest |  |
| ClusterConfigBlobMisconfigured |  |
| KonteynerİşlemiSonSistemiHatası | Konteyner işlemi sistem hatası ile çöktü |
| ContainerProcessTerminatedUnknownError hatası | Konteyner işlemi bilinmeyen bir hatayla çöktü |
| Konteyner DoğrulamaBaşarısız | Blob kapsayıcı doğrulama bu hata {0}ile başarısız oldu: . |
| DeleteWebServiceResourceFailed |  |
| Özel DurumDeserializationError |  |
| FailedGettingApiDocument |  |
| FailedStoringWebService |  |
| GeçersizMemoryConfiguration | GeçersizMemoryConfiguration, ConfigValue:{0} |
| GeçersizKaynak ÖnbellekYapılandırma |  |
| GeçersizKaynakDownloadConfiguration |  |
| GeçersizWebServiceResources |  |
| Eksik Görev Örneği | Bağımsız değişken sağlanmadı. Geçerli bağımsız değişkenlerin geçirildiğini doğrulayın ve yeniden deneyin. |
| ModelPackageGeçersiz |  |
| Modül Yürütme Başarısız |  |
| ModülYükü Başarısız Oldu |  |
| ModülObjectCloneFailed |  |
| OutputConversionFailed |  |
| PortDataTypeNotDesteklenen | Port id={0} desteklenmeyen bir {1}veri türü vardır: . |
| Kaynak İndir |  |
| Kaynak YüküBaşarısız |  |
| ServiceUrisNotFound |  |
| SwaggerNesil | Swagger nesil başarısız oldu, Ayrıntılar:{0} |
| Beklenmeyen Puan Durumu |  |
| UnknownBackendErrorResponse |  |
| UnknownError |  |
| UnknownJobStatusCode | Bilinmeyen iş {0}durum kodu. |
| BilinmeyenModül Hatası |  |
| UpdateWebServiceResourceFailed |  |
| WebServiceGroupNotFound |  |
| WebServicePackageGeçersiz | GeçersizWebServicePackage, Ayrıntılar:{0} |
| İşçi Yetkilendirmesi Başarısız Oldu |  |
| İşçi Erişilemez |  |
 
## <a name="internalerrorsystemlowonmemory-http-status-code-500"></a>InternalErrorSystemLowOnMemory (HTTP durum kodu 500)
 
Yürütme bir iç hata yla karşılaştı. Sistem hafızası düşük. Lütfen tekrar deneyin.
 
 
## <a name="modelpackageformaterror-http-status-code-500"></a>ModelPackageFormatError (HTTP durum kodu 500)
 
Geçersiz model paketi. Sağlanan model paketinin doğru olduğunu doğrulayın ve yeniden deneyin.
 
 
## <a name="webservicepackageinternalerror-http-status-code-500"></a>WebServicePackageInternalError (HTTP durum kodu 500)
 
Geçersiz web servis paketi. Sağlanan web paketinin doğru olduğunu doğrulayın ve yeniden deneyin.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| Modül Hatası | Web hizmeti paket grafiği geçersizdir. Şey:{0} |
 
## <a name="initializingcontainers-http-status-code-503"></a>Başlangıç Kapları (HTTP durum kodu 503)
 
Kapsayıcılar başharfe atılıyor gibi istek yürütülemez.
 
 
## <a name="serviceunavailable-http-status-code-503"></a>ServiceKullanılamaz (HTTP durum kodu 503)
 
Hizmet geçici olarak kullanılamıyor.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| NomoreResources | İstek için kullanılabilir kaynak yok. |
| İstekThrottled | İstek bitiş noktası için {0} azaltıldı. Bitiş noktası için maksimum eşzamanlılık. {1} |
| TooManyConcurrentRequests | Çok fazla eşzamanlı istek gönderildi. |
| TooManyHostsBeingInitialized | Aynı anda çok fazla ana bilgisayar başharfe bünyede başharfe bilgili. Azaltmayı / yeniden denemeyi düşünün. |
| TooManyHostsBeingInitializedPerModel | Aynı anda çok fazla ana bilgisayar başharfe bünyede başharfe bilgili. Azaltmayı / yeniden denemeyi düşünün. |
 
## <a name="gatewaytimeout-http-status-code-504"></a>Ağ Geçidi Timeout (HTTP durum kodu 504)
 
İşlem izin verilen süre içinde tamamlanamadı.
 
| Hata kodu | Kullanıcı iletisi |
| ---------- |--------------|
| BackendInitializationTimeout | Web hizmeti başlatma izin verilen süre içinde tamamlanamadı. |
| BackendScoreTimeout | Web hizmeti isteği yürütme izin verilen süre içinde tamamlanamadı. |
 
