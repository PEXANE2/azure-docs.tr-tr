---
title: Docker Kapsayıcıları-LUSıS
titleSuffix: Azure Cognitive Services
description: LUSıS kapsayıcısı, eğitilen veya yayınlanan uygulamanızı bir Docker kapsayıcısına yükler ve kapsayıcının API uç noktalarından sorgu tahminlerine erişim sağlar.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: dapine
ms.openlocfilehash: 2b87f9bcbaa0fd9d8a23d774e0765e1eb5b56633
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563894"
---
# <a name="install-and-run-luis-docker-containers"></a>LUSıS Docker kapsayıcılarını yükleyip çalıştırın
 
Language Understanding (LUSıS) kapsayıcısı, bir [Luo uygulaması](https://www.luis.ai)olarak da bir Docker kapsayıcısına, Ayrıca, kapsayıcının API uç noktalarından alınan sorgu tahminlerine erişim sağlayan eğitilen veya yayınlanan Language Understanding modelinizi yükler. Sorgudan sorgu günlüklerini toplayabilir ve uygulamanın tahmin doğruluğunu artırmak için bunları Language Understanding uygulamasına geri yükleyebilirsiniz.

Aşağıdaki videoda Bu kapsayıcının kullanımı gösterilmektedir.

[![Bilişsel hizmetler için kapsayıcı Tanıtımı](./media/luis-container-how-to/luis-containers-demo-video-still.png)](https://aka.ms/luis-container-demo)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

LUSıS kapsayıcısını çalıştırmak için aşağıdakilere sahip olmanız gerekir: 

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Bir [ana bilgisayarda](#the-host-computer)Docker altyapısının yüklü olması gerekir. Docker, [MacOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/)ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms)'ta Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows 'da**Docker 'ın de Linux kapsayıcılarını destekleyecek şekilde yapılandırılması gerekir.<br><br>|
|Docker ile benzerlik | Kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri gibi Docker kavramlarının yanı sıra temel `docker` komutlar hakkında bilgi sahibi olmanız gerekir.| 
|Azure `Cognitive Services` Resource ve lusıs [paketlenmiş uygulama](luis-how-to-start-new-app.md#export-app-for-containers) dosyası |Kapsayıcısını kullanabilmeniz için şunları yapmanız gerekir:<br><br>* Bilişsel _Hizmetler_ Azure kaynağı ve ilgili faturalandırma anahtarı faturalandırma uç noktası URI 'si. Her iki değer de kaynak için genel bakış ve anahtarlar sayfalarında bulunur ve kapsayıcıyı başlatmak için gereklidir. Aşağıdaki BILLING_ENDPOINT_URI örneğinde gösterildiği gibi `luis/v2.0` , yönlendirmeyi uç nokta URI 'sine eklemeniz gerekir. <br>* İlişkili uygulama KIMLIĞIYLE kapsayıcıya bağlı giriş olarak paketlenmiş eğitilen veya yayımlanmış bir uygulama. Paketlenmiş dosyayı LUıS portalından veya yazma API 'Lerinden alabilirsiniz. [Yazma API 'lerinden](#authoring-apis-for-package-file)LUIS paketlenmiş uygulama alıyorsanız, _yazma anahtarınıza_de ihtiyacınız olacaktır.<br><br>Bu gereksinimler komut satırı bağımsız değişkenlerini aşağıdaki değişkenlere iletmek için kullanılır:<br><br>**{AUTHORING_KEY}** : Bu anahtar, buluttaki LUSıS hizmetinden paketlenmiş uygulamayı almak ve sorgu günlüklerini buluta geri yüklemek için kullanılır. Biçim `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.<br><br>**{APPLICATION_ID}** : Bu KIMLIK, uygulamayı seçmek için kullanılır. Biçim `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`.<br><br>**{API_KEY}** : Bu anahtar, kapsayıcıyı başlatmak için kullanılır. Uç nokta anahtarını iki yerde bulabilirsiniz. Birincisi, bilişsel _Hizmetler_ kaynağının anahtar listesi içindeki Azure Portal. Uç nokta anahtarı, anahtarlar ve uç nokta ayarları sayfasındaki LUO portalında da kullanılabilir. Başlangıç anahtarını kullanmayın.<br><br>**{ENDPOINT_URI}** : Genel Bakış sayfasında belirtilen bitiş noktası.<br><br>[Yazma anahtarı ve uç nokta anahtarının](luis-boundaries.md#key-limits) farklı amaçları vardır. Bunları birbirlerinin yerine kullanmayın. |

### <a name="authoring-apis-for-package-file"></a>Paket dosyası için yazma API 'Leri

Paketlenmiş uygulamalar için yazma API 'Leri:

* [Yayımlanmış paket API 'SI](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-packagepublishedapplicationasgzip)
* [Yayımlanmamış, yalnızca eğitilen paket API 'SI](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-packagetrainedapplicationasgzip)

### <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Bu kapsayıcı, ayarlar için en düşük ve önerilen değerleri destekler:

|Kapsayıcı| Minimum | Önerilen | TPS<br>(En düşük, en yüksek)|
|-----------|---------|-------------|--|
|LUIS|1 çekirdek, 2 GB bellek|1 çekirdek, 4 GB bellek|20, 40|

* Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.
* TPS-saniye başına işlem

Çekirdek ve bellek, `--cpus` `docker run` komutunun bir parçası `--memory` olarak kullanılan ve ayarlarına karşılık gelir.

## <a name="get-the-container-image-with-docker-pull"></a>Kapsayıcı görüntüsünü al`docker pull`

`mcr.microsoft.com/azure-cognitive-services/luis` [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) Depodan bir kapsayıcı görüntüsü indirmek için komutunu kullanın:

```
docker pull mcr.microsoft.com/azure-cognitive-services/luis:latest
```

Bir kapsayıcı görüntüsünü indirmek için [komutunukullanın.`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/)

Yukarıdaki komutta `latest` kullanılan gibi kullanılabilir etiketlerin tam bir açıklaması için bkz. Docker Hub üzerinde [Luis](https://go.microsoft.com/fwlink/?linkid=2043204) .

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]


## <a name="how-to-use-the-container"></a>Kapsayıcıyı kullanma

Kapsayıcı [ana bilgisayardan](#the-host-computer)olduktan sonra, kapsayıcında çalışmak için aşağıdaki işlemi kullanın.

![Language Understanding (LUSıS) kapsayıcısını kullanma işlemi](./media/luis-container-how-to/luis-flow-with-containers-diagram.jpg)

1. LUıS portalından veya LUıS API 'Lerinden kapsayıcı için [paket dışarı aktarma](#export-packaged-app-from-luis) .
1. Paket dosyasını, [ana bilgisayarda](#the-host-computer)gerekli **giriş** dizinine taşıyın. Yeniden adlandırmayın, değiştirme, üzerine yazma veya sıkıştırmayı açma, HALSıS paket dosyası.
1. [Kapsayıcıyı](##run-the-container-with-docker-run), gerekli _giriş bağlama_ ve faturalandırma ayarlarıyla çalıştırın. `docker run` Komuta daha fazla [örnek](luis-container-configuration.md#example-docker-run-commands) kullanılabilir. 
1. [Kapsayıcının tahmin uç noktası sorgulanıyor](#query-the-containers-prediction-endpoint). 
1. Kapsayıcı ile işiniz bittiğinde, Halu portalındaki çıkış bağlamasından [uç nokta günlüklerini içeri aktarın](#import-the-endpoint-logs-for-active-learning) ve kapsayıcıyı [durdurun](#stop-the-container) .
1. Uygulamayı geliştirmek için, **Gözden geçirme uç noktası sıralayıcısı** sayfasında Luo portalının [etkin öğrenimini](luis-how-to-review-endpoint-utterances.md) kullanın.

Kapsayıcıda çalışan uygulama değiştirilemez. Kapsayıcıda uygulama değişikliği sırasında, LUIS [portalını kullanarak](https://www.luis.ai) LUIS hizmetinde uygulamayı DEĞIŞTIRMENIZ veya LUIS [yazma API 'lerini](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f)kullanmanız gerekir. Daha sonra eğitme ve/veya Yayımla, sonra yeni bir paket indirip kapsayıcıyı yeniden çalıştırın.

Kapsayıcının içindeki LUO uygulaması, LUSıS hizmetine geri verilemez. Yalnızca sorgu günlükleri karşıya yüklenebilir. 

## <a name="export-packaged-app-from-luis"></a>Paketlenmiş uygulamayı LUSıS 'den dışarı aktarma

LUSıS kapsayıcısı, Kullanıcı sorusunun tahmin sorgularını yanıtlamak için eğitilen veya yayınlanmış bir LUO uygulaması gerektirir. LUıN uygulamasını almak için, eğitilen veya yayımlanmış paket API 'sini kullanın. 

Varsayılan konum, `docker run` komutu çalıştırdığınız `input` konuma göre alt dizindir.  

Paket dosyasını bir dizine yerleştirin ve Docker kapsayıcısını çalıştırdığınızda giriş bağlama olarak bu dizine başvurun. 

### <a name="package-types"></a>Paket türleri

Giriş bağlama dizini, uygulamanın **Üretim**, **hazırlama**ve **eğitimli** sürümlerini eşzamanlı olarak içerebilir. Tüm paketler bağlanır. 

|Paket türü|Sorgu uç noktası API 'SI|Sorgu kullanılabilirliği|Paket dosya adı biçimi|
|--|--|--|--|
|Eğitildi|Al, postala|Yalnızca kapsayıcı|`{APPLICATION_ID}_v{APPLICATION_VERSION}.gz`|
|Hazırlanıyor|Al, postala|Azure ve kapsayıcı|`{APPLICATION_ID}_STAGING.gz`|
|Üretim|Al, postala|Azure ve kapsayıcı|`{APPLICATION_ID}_PRODUCTION.gz`|

> [!IMPORTANT]
> LUSıS paket dosyalarını yeniden adlandırmayın, değiştirmeyin, üzerine yazmaz veya sıkıştırmasını açın.

### <a name="packaging-prerequisites"></a>Paketleme önkoşulları

Bir LUSıS uygulamasını paketlemeden önce aşağıdakilere sahip olmanız gerekir:

|Paketleme gereksinimleri|Ayrıntılar|
|--|--|
|Azure bilişsel _Hizmetler_ kaynak örneği|Desteklenen bölgeler şunlardır<br><br>Batı ABD (```westus```)<br>Batı Avrupa (```westeurope```)<br>Avustralya Doğu (```australiaeast```)|
|Eğitilen veya yayınlanan LUO uygulaması|, [Desteklenmeyen bağımlılıklar](#unsupported-dependencies)yok. |
|[Ana bilgisayarın](#the-host-computer)dosya sistemine erişim |Ana bilgisayar bir [giriş bağlamaya](luis-container-configuration.md#mount-settings)izin vermelidir.|
  
### <a name="export-app-package-from-luis-portal"></a>Uygulama paketini LUSıS portalından dışarı aktarma

Lua [portalı](https://www.luis.ai) , eğitilen veya yayınlanan uygulamanın paketini dışarı aktarma olanağını sağlar. 

### <a name="export-published-apps-package-from-luis-portal"></a>Yayınlanan uygulamanın paketini LUSıS portalından dışarı aktar

Yayınlanan uygulamanın paketine, **uygulamalar** listesi sayfasından ulaşılabilir. 

1. LUI [portalında](https://www.luis.ai)oturum açın.
1. Listedeki uygulama adının solundaki onay kutusunu seçin. 
1. Listenin üzerindeki bağlam araç çubuğundan **dışarı aktarma** öğesini seçin.
1. **Kapsayıcı (GZIP) Için dışarı aktar**' ı seçin.
1. **Üretim yuvası** veya **hazırlama yuvası**ortamını seçin.
1. Paket tarayıcıdan indirilir.

![Uygulama sayfasının dışarı aktarma menüsünden, kapsayıcı için yayımlanmış paketi dışarı aktarma](./media/luis-container-how-to/export-published-package-for-container.png)

### <a name="export-trained-apps-package-from-luis-portal"></a>Eğitilen uygulamasının paketini LUSıS portalından dışa aktarma

Eğitilen uygulamanın paketine **sürümler** listesi sayfasından ulaşılabilir. 

1. LUI [portalında](https://www.luis.ai)oturum açın.
1. Listeden uygulamayı seçin. 
1. Uygulamanın gezinti çubuğunda **Yönet** ' i seçin.
1. Sol gezinti çubuğundaki **sürümler** ' i seçin.
1. Listedeki sürüm adının solundaki onay kutusunu seçin.
1. Listenin üzerindeki bağlam araç çubuğundan **dışarı aktarma** öğesini seçin.
1. **Kapsayıcı (GZIP) Için dışarı aktar**' ı seçin.
1. Paket tarayıcıdan indirilir.

![Sürümler sayfasının dışa aktarma menüsünde, kapsayıcı için eğitilen paketi dışarı aktarma](./media/luis-container-how-to/export-trained-package-for-container.png)


### <a name="export-published-apps-package-from-api"></a>Yayınlanan uygulamanın paketini API 'den dışarı aktar

Zaten [yayımlamış](luis-how-to-publish-app.md)olduğunuz bir Luo uygulamasını paketlemek için aşağıdaki REST API yöntemini kullanın. HTTP belirtiminin altındaki tabloyu kullanarak API çağrısındaki yer tutucular için kendi uygun değerlerinizi değiştirme.

```http
GET /luis/api/v2.0/package/{APPLICATION_ID}/slot/{APPLICATION_ENVIRONMENT}/gzip HTTP/1.1
Host: {AZURE_REGION}.api.cognitive.microsoft.com
Ocp-Apim-Subscription-Key: {AUTHORING_KEY}
```

| Yer tutucu | Value |
|-------------|-------|
|{APPLICATION_ID} | Yayınlanan LUSıS uygulamasının uygulama KIMLIĞI. |
|{APPLICATION_ENVIRONMENT} | Yayınlanan LUSıS uygulamasının ortamı. Aşağıdaki değerlerden birini kullanın:<br/>```PRODUCTION```<br/>```STAGING``` |
|{AUTHORING_KEY} | Yayımlanan LUO uygulaması için Luo hesabının yazma anahtarı.<br/>Yazma anahtarınızı, Lua portalındaki **Kullanıcı ayarları** sayfasından edinebilirsiniz. |
|{AZURE_REGION} | Uygun Azure bölgesi:<br/><br/>```westus```-Batı ABD<br/>```westeurope```-Batı Avrupa<br/>```australiaeast```-Avustralya Doğu |

Yayınlanan paketi indirmek için lütfen [buradaki API belgelerine][download-published-package]başvurun. Başarılı bir şekilde indirildiyse, yanıt bir LUSıS paket dosyasıdır. Dosyayı kapsayıcının giriş bağlaması için belirtilen depolama konumuna kaydedin. 

### <a name="export-trained-apps-package-from-api"></a>Eğitilen uygulamanın paketini API 'den dışarı aktar

Zaten [eğitilen](luis-how-to-train.md)bir Luo uygulamasını paketlemek için aşağıdaki REST API yöntemini kullanın. HTTP belirtiminin altındaki tabloyu kullanarak API çağrısındaki yer tutucular için kendi uygun değerlerinizi değiştirme.

```http
GET /luis/api/v2.0/package/{APPLICATION_ID}/versions/{APPLICATION_VERSION}/gzip HTTP/1.1
Host: {AZURE_REGION}.api.cognitive.microsoft.com
Ocp-Apim-Subscription-Key: {AUTHORING_KEY}
```

| Yer tutucu | Value |
|-------------|-------|
|{APPLICATION_ID} | Eğitilen LUO uygulamasının uygulama KIMLIĞI. |
|{APPLICATION_VERSION} | Eğitilen LUO uygulamasının uygulama sürümü. |
|{AUTHORING_KEY} | Yayımlanan LUO uygulaması için Luo hesabının yazma anahtarı.<br/>Yazma anahtarınızı, Lua portalındaki **Kullanıcı ayarları** sayfasından edinebilirsiniz.  |
|{AZURE_REGION} | Uygun Azure bölgesi:<br/><br/>```westus```-Batı ABD<br/>```westeurope```-Batı Avrupa<br/>```australiaeast```-Avustralya Doğu |

Eğitilen paketi indirmek için lütfen [buradaki API belgelerine][download-trained-package]başvurun. Başarılı bir şekilde indirildiyse, yanıt bir LUSıS paket dosyasıdır. Dosyayı kapsayıcının giriş bağlaması için belirtilen depolama konumuna kaydedin. 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı ile çalıştırma`docker run`

Kapsayıcıyı çalıştırmak için [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. Komut aşağıdaki parametreleri kullanır:

| Yer tutucu | Değer |
|-------------|-------|
|{API_KEY} | Bu anahtar, kapsayıcıyı başlatmak için kullanılır. Başlangıç anahtarını kullanmayın. |
|{ENDPOINT_URI} | Uç nokta değeri Azure Portal `Cognitive Services` Genel Bakış sayfasında kullanılabilir. |

Aşağıdaki örnek `docker run` komutta bu parametreleri kendi değerlerinizle değiştirin. Windows konsolunda komutunu çalıştırın.

```console
docker run --rm -it -p 5000:5000 ^
--memory 4g ^
--cpus 2 ^
--mount type=bind,src=c:\input,target=/input ^
--mount type=bind,src=c:\output\,target=/output ^
mcr.microsoft.com/azure-cognitive-services/luis ^
Eula=accept ^
Billing={ENDPOINT_URI} ^
ApiKey={API_KEY}
```

* Bu örnek, Windows 'da herhangi bir `C:` izin çakışmasını önlemek için dizini sürücüden kullanır. Giriş dizini belirli bir dizini kullanmak istiyorsanız, docker vermeniz gerekebilir hizmet izni. 
* Docker Kapsayıcıları hakkında bilginiz yoksa bağımsız değişkenlerin sırasını değiştirmeyin.
* Farklı bir işletim sistemi kullanıyorsanız, sistem için doğru konsol/Terminal, bağlama için klasör söz dizimi ve satır devamlılık karakteri kullanın. Bu örneklerde bir Windows konsolunun satır devamlılık karakteriyle `^`birlikte varsayılmaktadır. Kapsayıcı bir Linux işletim sistemi olduğundan, hedef bağlama bir Linux stili klasör söz dizimini kullanır.

Bu komut:

* LUSıS kapsayıcı görüntüsünden bir kapsayıcı çalıştırır
* Kapsayıcı konağında bulunan c:\ınput konumundaki giriş bağlamasından LUO uygulamasını yükler
* İki CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Kapsayıcı ana bilgisayarında bulunan c:\Output konumundaki çıkış bağlamasındaki kapsayıcıyı ve Lua günlüklerini kaydeder
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir. 

`docker run` Komuta daha fazla [örnek](luis-container-configuration.md#example-docker-run-commands) kullanılabilir. 

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](#billing).
> Apikey değeri, Lua  portalındaki anahtarlar ve uç noktalar sayfasından ve Azure `Cognitive Services` kaynak anahtarları sayfasında da kullanılabilir.  

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

## <a name="endpoint-apis-supported-by-the-container"></a>Kapsayıcı tarafından desteklenen uç nokta API 'Leri

Kapsayıcı ile birlikte API 'nin v2 ve [v3 (Önizleme)](luis-migration-api-v3.md) sürümleri mevcuttur. 

## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin uç noktasını sorgulama

Kapsayıcı, REST tabanlı sorgu tahmin uç noktası API 'Leri sağlar. Yayımlanan (hazırlama veya üretim) uygulamalarının uç noktaları, eğitilen uygulamalar için uç noktalardan _farklı_ bir rotaya sahiptir. 

Kapsayıcı API 'leri için `https://localhost:5000`Konağı kullanın. 

|Paket türü|Yöntem|Rota|Sorgu parametreleri|
|--|--|--|--|
|Yayımlanmış|[Al](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78), [Postala](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)|/Luis/v2.0/Apps/{AppID}?|q = {q}<br>& hazırlama<br>[& Timezonekayması]<br>[& verbose]<br>[& günlüğü]<br>|
|Eğitildi|Al, postala|/Luis/v2.0/Apps/{AppID}/Versions/{VersionId}?|q = {q}<br>[& Timezonekayması]<br>[& verbose]<br>[& günlüğü]|

Sorgu parametreleri, sorgu yanıtında nasıl ve neyin döndürüleceğini yapılandırır:

|Sorgu parametresi|Type|Amaç|
|--|--|--|
|`q`|dize|Kullanıcının utterliği.|
|`timezoneOffset`|numarası|Timezonekayması, önceden oluşturulmuş varlık datetimeV2 tarafından kullanılan [saat dilimini değiştirmenize](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) izin verir.|
|`verbose`|boolean|Doğru olarak ayarlandığında tüm hedefleri ve puanlarını döndürür. Varsayılan değer, yalnızca en üst amacı döndüren false ' dır.|
|`staging`|boolean|True olarak ayarlanırsa, hazırlama ortamı sonuçlarından sorgu döndürür. |
|`log`|boolean|Daha sonra [etkin öğrenme](luis-how-to-review-endpoint-utterances.md)için kullanılabilen sorguları günlüğe kaydeder. Varsayılan değer true 'dur.|

### <a name="query-published-app"></a>Yayımlanan uygulamayı sorgula

Yayımlanmış bir uygulama için kapsayıcıyı sorgulamak üzere örnek bir KıVRıMLı komutu:

```bash
curl -X GET \
"http://localhost:5000/luis/v2.0/apps/{APPLICATION_ID}?q=turn%20on%20the%20lights&staging=false&timezoneOffset=0&verbose=false&log=true" \
-H "accept: application/json"
```
**Hazırlama** ortamına sorgu yapmak için, **hazırlama** sorgu dizesi parametre değerini doğru olarak değiştirin: 

`staging=true`

### <a name="query-trained-app"></a>Eğitilen uygulamayı sorgula

Eğitilen bir uygulama için kapsayıcıyı sorgulamak üzere örnek bir KıVRıMLı komut: 

```bash
curl -X GET \
"http://localhost:5000/luis/v2.0/apps/{APPLICATION_ID}/versions/{APPLICATION_VERSION}?q=turn%20on%20the%20lights&timezoneOffset=0&verbose=false&log=true" \
-H "accept: application/json"
```
Sürüm adı en fazla 10 karakterden oluşur ve yalnızca bir URL 'de izin verilen karakterleri içerir. 

## <a name="import-the-endpoint-logs-for-active-learning"></a>Etkin öğrenme için uç nokta günlüklerini içeri aktarma

LUSıS kapsayıcısı için bir çıkış bağlaması belirtilmişse, uygulama sorgusu günlük dosyaları çıkış dizinine kaydedilir; burada {INSTANCE_ID} kapsayıcı KIMLIĞIDIR. Uygulama sorgu günlüğü, LUO kapsayıcısına gönderilen her bir tahmin sorgusunun sorgusunu, yanıtını ve zaman damgalarını içerir. 

Aşağıdaki konum kapsayıcının günlük dosyaları için iç içe dizin yapısını gösterir.
```
/output/luis/{INSTANCE_ID}/
```
 
LUU portalından, uygulamanızı seçin ve ardından bu günlükleri karşıya yüklemek için **uç nokta günlüklerini Içeri aktar** ' ı seçin. 

![Etkin öğrenme için kapsayıcının günlük dosyalarını içeri aktar](./media/luis-container-how-to/upload-endpoint-log-files.png)

Günlük karşıya yüklendikten sonra, LUı portalındaki uç nokta utslerini [gözden geçirin](https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-review-endpoint-utterances) .

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcıyı durdur

Kapsayıcıyı kapatmak için kapsayıcının çalıştığı komut satırı ortamında **CTRL + C**tuşlarına basın.

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı bir çıkış [bağlaması](luis-container-configuration.md#mount-settings) ve günlüğü etkin olarak çalıştırırsanız kapsayıcı, kapsayıcıyı başlatırken veya çalıştırırken oluşan sorunları gidermek için yararlı olan günlük dosyaları oluşturur. 

## <a name="billing"></a>Faturalandırma

LUSıS kapsayıcısı, Azure hesabınızdaki bilişsel _Hizmetler_ kaynağını kullanarak faturalandırma bilgilerini Azure 'a gönderir. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](luis-container-configuration.md).

## <a name="supported-dependencies-for-latest-container"></a>Kapsayıcı için `latest` desteklenen bağımlılıklar

2019 gt derlemesinde yayınlanan en son kapsayıcı şunları destekleyecektir:

* Bing yazım denetimi: sorgu dizesi parametreleriyle sorgu tahmin uç noktasına `&spellCheck=true&bing-spell-check-subscription-key={bingKey}` istekler. Daha fazla bilgi için [Bing yazım denetimi v7 öğreticisini](luis-tutorial-bing-spellcheck.md) kullanın. Bu özellik kullanılırsa kapsayıcı, Bing yazım denetimi v7 kaynağına söylenişi gönderir.
* [Yeni önceden oluşturulmuş etki alanları](luis-reference-prebuilt-domains.md): Bu kurumsal odaklı etki alanları varlıklar, örnek söylerin ve desenleri içerir. Bu etki alanlarını kendi kullanım için genişletin. 

<a name="unsupported-dependencies"></a>

## <a name="unsupported-dependencies-for-latest-container"></a>Kapsayıcı için `latest` desteklenmeyen bağımlılıklar

LUSıS uygulamanızda desteklenmeyen bağımlılıklar varsa, desteklenmeyen özellikleri kaldırana kadar [kapsayıcı için dışa](#export-packaged-app-from-luis) aktarabilemeyeceksiniz. Kapsayıcıyı dışarı aktarmaya çalıştığınızda, Lua portalı, kaldırmanız gereken desteklenmeyen özellikleri raporlar.

Aşağıdaki bağımlılıklardan herhangi birini **içermiyorsa** bir Luo uygulaması kullanabilirsiniz:

Desteklenmeyen uygulama yapılandırması|Ayrıntılar|
|--|--|
|Desteklenmeyen kapsayıcı kültürleri| Felemenkçe (nl-NL)<br>Japonca (ja-JP)<br>Almanca yalnızca [1.0.2 belirteç ayırıcı](luis-language-support.md#custom-tokenizer-versions)ile desteklenir.|
|Tüm kültürler için desteklenmeyen varlıklar|Tüm kültürler için [KeyPhrase](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-keyphrase) önceden oluşturulmuş varlık|
|Ingilizce (en-US) kültürüne yönelik desteklenmeyen varlıklar|[GeographyV2](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-geographyv2) önceden oluşturulmuş varlıklar|
|Konuşma primi|Kapsayıcıda dış bağımlılıklar desteklenmiyor.|
|Yaklaşım analizi|Kapsayıcıda dış bağımlılıklar desteklenmiyor.|

<!--blogs/samples/video courses -->
[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, Language Understanding (LUSıS) kapsayıcıları indirmek, yüklemek ve çalıştırmak için kavramları ve iş akışını öğrendiniz. Özet:

* Language Understanding (LUSıS), Docker için uç nokta sorgu tahminleri sağlayan bir Linux kapsayıcısı sağlar.
* Kapsayıcı görüntüleri Microsoft Container Registry (MCR) ile indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Kapsayıcının ana bilgisayar URI 'sini belirterek kapsayıcı uç noktalarını sorgulamak için REST API kullanabilirsiniz.
* Bir kapsayıcı örneği oluşturulurken, fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcıları, müşteri verilerini (örneğin, çözümlenmekte olan resim veya metin) Microsoft 'a göndermez.

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [kapsayıcıları yapılandırma](luis-container-configuration.md) yapılandırma ayarları
* LUSıS işlevleriyle ilgili sorunları çözmek için [sorun giderme](troubleshooting.md) bölümüne bakın.
* Daha fazla bilişsel [Hizmetler kapsayıcısı](../cognitive-services-container-support.md) kullanın

<!-- Links - external -->
[download-published-package]: https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-packagepublishedapplicationasgzip
[download-trained-package]: https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/apps-packagetrainedapplicationasgzip