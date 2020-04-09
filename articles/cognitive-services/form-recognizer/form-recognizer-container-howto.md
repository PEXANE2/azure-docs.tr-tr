---
title: Form Tanııcı için kapsayıcı nasıl yüklenir ve çalıştırUlur?
titleSuffix: Azure Cognitive Services
description: Bu makalede, form ve tablo verilerini ayrıştırmak için Azure Bilişsel Hizmetler Formu Tanıyılayıcı kapsayıcısının nasıl kullanılacağı açıklanmıştır.
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 8cfa9114c5a5e57882cb84b604c1cf71be9acc52
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878349"
---
# <a name="install-and-run-form-recognizer-containers-preview"></a>Form Tanıyıcı kapsayıcıları yükleme ve çalıştırma (Önizleme)

Azure Form Tanıyıcısı, formlardan anahtar değer çiftleri ve tabloları tanımlamak ve ayıklamak için makine öğrenimi teknolojisini uygular. Değerleri ve tablo girişlerini anahtar değer çiftleri ile ilişkilendirir ve özgün dosyadaki ilişkileri içeren yapılandırılmış verileri çıktıalır. 

Karmaşıklığı azaltmak ve özel bir Form Tanıyıcı modelini iş akışı otomasyon işleminize veya başka bir uygulamaya kolayca entegre etmek için, basit bir REST API kullanarak modeli arayabilirsiniz. Yalnızca beş form belgesi (veya bir boş form ve iki doldurulmuş form) gereklidir, böylece sonuçları hızlı, doğru bir şekilde alabilir ve özel içeriğinize göre uyarlayabilirsiniz. Hiçbir ağır manuel müdahale veya kapsamlı veri bilimi uzmanlık gereklidir. Ayrıca veri etiketleme veya veri ek açıklama gerektirmez.

> [!IMPORTANT]
> Form Tanıyıcı kapsayıcıları şu anda Form Tanıyıcı API sürümü 1.0 kullanın. Bunun yerine yönetilen hizmeti kullanarak API'nin en son sürümüne erişebilirsiniz.

| İşlev | Özellikler |
|----------|----------|
| Form Tanıma | <li>PDF, PNG ve JPG dosyalarını işler<li>Aynı düzenin en az beş formu ile özel modeller trenler <li>Anahtar değeri çiftleri ve tablo bilgilerini ayıklar <li>Formların içindeki resimlerden yazdırılan metni algılamak ve ayıklamak için Azure Bilişsel Hizmetler Bilgisayar Vizyonu API Tanıma özelliğini kullanır<li>Ek açıklama veya etiketleme gerektirmez |

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Form Recognizer kapsayıcılarını kullanmadan önce aşağıdaki ön koşulları karşılamanız gerekir:

| Gerekli | Amaç |
|----------|---------|
| Docker altyapısı | Docker [Engine'in ana bilgisayara](#the-host-computer)yüklenmesi gerekir. Docker, [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms) üzerinde Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/) bölümüne bakın.<br><br> Docker, kapsayıcıların Azure'a bağlanmasına ve fatura verilerini göndermesine izin verecek şekilde yapılandırılmalıdır. <br><br> Windows'da Docker, Linux kapsayıcılarını destekleyecek şekilde de yapılandırılmalıdır.<br><br> |
| Docker ile aşinalık | Kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri gibi Docker kavramları hakkında temel bir `docker` anlayışa ve temel komutlar hakkında bilgi sahibi olmalısınız. |
| Azure CLI | Azure [CLI'yi](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ana bilgisayarınıza yükleyin. |
| Bilgisayarlı Vision API kaynağı | Taranmış belgeleri ve görüntüleri işlemek için bir Bilgisayar Vizyonu kaynağına ihtiyacınız vardır. Metni Tanı özelliğine Azure kaynağı (REST API veya SDK) veya *bilişsel hizmetlerle tanıyan metin* [kapsayıcısı](../Computer-vision/computer-vision-how-to-install-containers.md#get-the-container-image-with-docker-pull)olarak erişebilirsiniz. Her zamanki fatura ücretleri geçerlidir. <br><br>Computer Vision kaynağınız (Azure bulutu veya Bilişsel Hizmetler kapsayıcısı) için hem API anahtarını hem de uç noktalarını geçirin. Bu API anahtarını ve bitiş noktasını **{COMPUTER_VISION_API_KEY}** ve **{COMPUTER_VISION_ENDPOINT_URI}** olarak kullanın.<br><br> *Bilişsel-hizmetleri tanıyan metin* kapsayıcısını kullanıyorsanız, aşağıdakilerden emin olun:<br><br>Form Recognizer kapsayıcısı için Bilgisayar Görüşü anahtarınız, `docker run` *bilişsel hizmetler-tanıma-metin* kapsayıcısı için Computer Vision komutunda belirtilen anahtardır.<br>Faturalandırma bitiş noktanız kapsayıcının bitiş noktasıdır `http://localhost:5000`(örneğin,). Hem Computer Vision kapsayıcısını hem de Form Recognizer kapsayıcısını aynı ana bilgisayarda birlikte kullanırsanız, her ikisi de *varsayılan 5000*bağlantı noktasıyla başlatılamaz. |
| Form Tanıyıcı kaynağı | Bu kapsayıcıları kullanmak için şunları yapmalısınız:<br><br>İlişkili API anahtarını ve bitiş noktası URI'yi almak için bir Azure **Form Tanıyıcı** kaynağı. Her iki değer de Azure portalı **Form Recognizer** Overview ve Keys sayfalarında mevcuttur ve kapsayıcıyı başlatmak için her iki değer de gereklidir.<br><br>**{FORM_RECOGNIZER_API_KEY}**: Keys sayfasındaki mevcut iki kaynak anahtarından biri<br><br>**{FORM_RECOGNIZER_ENDPOINT_URI}**: Genel Bakış sayfasında sağlanan bitiş noktası |

> [!NOTE]
> Computer Vision kaynak adı, tire `-` veya diğer özel karakterler olmadan tek bir sözcük olmalıdır. Bu kısıtlama, Form Tanıyır ve Metin kapsayıcıuyumluluğunu tanımak için yerindedir.

## <a name="gathering-required-parameters"></a>Gerekli parametrelerin toplanması

Gerekli olan tüm Bilişsel Hizmetler 'kapları için üç birincil parametre vardır. Son kullanıcı lisans sözleşmesi (EULA) değeri ile `accept`birlikte olmalıdır. Ayrıca, hem Bir Endpoint URL'si hem de API Anahtarı gereklidir.

### <a name="endpoint-uri-computer_vision_endpoint_uri-and-form_recognizer_endpoint_uri"></a>Endpoint `{COMPUTER_VISION_ENDPOINT_URI}` URI ve`{FORM_RECOGNIZER_ENDPOINT_URI}`

**Endpoint** URI değeri, ilgili Bilişsel Hizmet kaynağının Azure portalı *Genel Bakış* sayfasında kullanılabilir. *Genel Bakış* sayfasına gidin, Bitiş Noktası'nın `Copy to clipboard` <span class="docon docon-edit-copy x-hidden-focus"></span> üzerine gidin ve bir simge görüntülenir. Gerektiğinde kopyalayın ve kullanın.

![Daha sonra kullanmak için uç nokta uri toplamak](../containers/media/overview-endpoint-uri.png)

### <a name="keys-computer_vision_api_key-and-form_recognizer_api_key"></a>Anahtarlar `{COMPUTER_VISION_API_KEY}` ve`{FORM_RECOGNIZER_API_KEY}`

Bu anahtar kapsayıcıyı başlatmak için kullanılır ve ilgili Bilişsel Hizmet kaynağının Azure portalının Keys sayfasında kullanılabilir. *Keys* sayfasına gidin ve simgeye `Copy to clipboard` <span class="docon docon-edit-copy x-hidden-focus"></span> tıklayın.

![Daha sonra kullanmak için iki anahtardan birini alın](../containers/media/keys-copy-api-key.png)

> [!IMPORTANT]
> Bu abonelik anahtarları Bilişsel Hizmet API'nıza erişmek için kullanılır. Anahtarlarınızı paylaşmayın. Örneğin Azure Key Vault'u kullanarak güvenli bir şekilde saklayın. Ayrıca bu anahtarları düzenli olarak yenilemenizi öneririz. API araması yapmak için yalnızca bir anahtar gereklidir. İlk anahtarı yenilerken, hizmete sürekli erişim için ikinci anahtarı kullanabilirsiniz.

## <a name="request-access-to-the-container-registry"></a>Konteyner kayıt defterine erişim isteği

Öncelikle kapsayıcıya erişim isteği nde bulunmak için [Bilişsel Hizmetler Formu TanıYıcı Konteynerleri erişim istek formunu](https://aka.ms/FormRecognizerContainerRequestAccess) doldurmanız ve göndermeniz gerekir. Bunu yapmak da Computer Vision için kaydolur. Bilgisayar Lı Vizyon istek formuna ayrıca kaydolmanız gerekmez. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Konteyner gereksinimleri ve öneriler

Her Form Tanıyıcı kapsayıcısı için ayrılacak minimum ve önerilen CPU çekirdekleri ve bellek aşağıdaki tabloda açıklanmıştır:

| Kapsayıcı | Minimum | Önerilen |
|-----------|---------|-------------|
| Form Tanıma | 2 çekirdekli, 4 GB bellek | 4 çekirdekli, 8 GB bellek |
| Metni Tanı | 1 çekirdekli, 8 GB bellek | 2 çekirdek, 8 GB bellek |

* Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.
* Çekirdek ve bellek, `--cpus` `--memory` komutun bir parçası olarak `docker run` kullanılan ve ayarlara karşılık gelir.

> [!Note]
> Minimum ve önerilen değerler ana makine kaynaklarına *değil* Docker sınırlarına dayanır.

## <a name="get-the-container-images-with-the-docker-pull-command"></a>Docker çekme komutu ile konteyner görüntülerini alın

**Hem Form Tanıyıcı** hem de Metin sunusunu **Tanıyın** için kapsayıcı görüntüleri aşağıdaki konteyner kayıt defterinde mevcuttur:

| Kapsayıcı | Tam nitelikli görüntü adı |
|-----------|------------|
| Form Tanıma | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer:latest` |
| Metni Tanı | `containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest` |

Her iki kapsayıcıya da ihtiyacınız olacak, lütfen **Recognizer Text** kapsayıcısının [bu makalenin dışında ayrıntılı olarak açıkolduğunu unutmayın.](../Computer-vision/computer-vision-how-to-install-containers.md#get-the-container-image-with-docker-pull)

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-form-recognizer-container"></a>Form Recognizer konteyneri için Docker çekme

#### <a name="form-recognizer"></a>Form Tanıma

Form Tanıyıcı kapsayıcısını almak için aşağıdaki komutu kullanın:

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer:latest
```
### <a name="docker-pull-for-the-recognize-text-container"></a>Metin Tanı konteyneri için Docker çekme

#### <a name="recognize-text"></a>Metni Tanı

Metin tanı kapsayıcısını almak için aşağıdaki komutu kullanın:

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest
```

## <a name="how-to-use-the-container"></a>Kapsayıcı nasıl kullanılır?

Kapsayıcı [ana bilgisayarda](#the-host-computer)sonra, kapsayıcı ile çalışmak için aşağıdaki işlemi kullanın.

1. Gerekli fatura ayarlarıyla [kapsayıcıyı çalıştırın.](#run-the-container-by-using-the-docker-run-command) Komutun `docker run` daha fazla [örneği](form-recognizer-container-configuration.md#example-docker-run-commands) mevcuttur.
1. [Kapsayıcının tahmin bitiş noktasını sorgula.](#query-the-containers-prediction-endpoint)

## <a name="run-the-container-by-using-the-docker-run-command"></a>Docker run komutunu kullanarak kapsayıcıyı çalıştırın

Kapsayıcıyı çalıştırmak için [docker run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. ,, `{COMPUTER_VISION_API_KEY}` `{FORM_RECOGNIZER_ENDPOINT_URI}` ve `{FORM_RECOGNIZER_API_KEY}` değerleri nasıl alacağınız `{COMPUTER_VISION_ENDPOINT_URI}`hakkında ayrıntılar için gerekli [parametreleri toplamaya](#gathering-required-parameters) bakın.

Komut [örnekleri](form-recognizer-container-configuration.md#example-docker-run-commands) `docker run` mevcuttur.

### <a name="form-recognizer"></a>Form Tanıma

```bash
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
Billing={FORM_RECOGNIZER_ENDPOINT_URI} \
ApiKey={FORM_RECOGNIZER_API_KEY} \
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
```

Şu komut:

* Kapsayıcı görüntüsünden form tanıma kapsayıcısı çalıştırın.
* 2 CPU çekirdeği ve 8 gigabayt (GB) bellek ayırır.
* TCP bağlantı noktası 5000'i açığa çıkarır ve kapsayıcı için sözde TTY ayırır.
* Çıktıktan sonra kapsayıcıotomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.
* Konteynere bir /giriş ve /çıkış hacmi bağlar.

[!INCLUDE [Running multiple containers on the same host H2](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

### <a name="run-separate-containers-as-separate-docker-run-commands"></a>Ayrı docker çalıştır komutları olarak ayrı kapsayıcılar çalıştırın

Aynı ana bilgisayarda yerel olarak barındırılan Form Tanıyıcı ve Metin Tanıyıcı birleşimi için aşağıdaki iki örnek Docker CLI komutunu kullanın:

5000 portundaki ilk konteynırı çalıştırın. 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
Billing={FORM_RECOGNIZER_ENDPOINT_URI} \
ApiKey={FORM_RECOGNIZER_API_KEY}
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
```

5001 portundaki ikinci konteynırı çalıştırın.

```bash 
docker run --rm -it -p 5001:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
Eula=accept \
Billing={COMPUTER_VISION_ENDPOINT_URI} \
ApiKey={COMPUTER_VISION_API_KEY}
```
Sonraki her kapsayıcı farklı bir bağlantı noktasında olmalıdır. 

### <a name="run-separate-containers-with-docker-compose"></a>Docker Compose ile ayrı kaplar çalıştırın

Aynı ana bilgisayarda yerel olarak barındırılan Form Tanıyıcı ve Metin Tanıyıcı birleşimi için aşağıdaki örnek Docker Compose YAML dosyasına bakın. Metin Tanıyıcısı `{COMPUTER_VISION_API_KEY}` hem kapsayıcılar `formrecognizer` hem `ocr` de kapsayıcılar için aynı olmalıdır. `formrecognizer` Kapsayıcı `{COMPUTER_VISION_ENDPOINT_URI}` adı ve `ocr` bağlantı noktasını kullandığından, `ocr` yalnızca kapsayıcıda kullanılır. 

```docker
version: '3.3'
services:   
  ocr:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text"
    deploy:
      resources:
        limits:
          cpus: '2'
          memory: 8g
        reservations:
          cpus: '1'
          memory: 4g
    environment:
      eula: accept
      billing: "{COMPUTER_VISION_ENDPOINT_URI}"
      apikey: "{COMPUTER_VISION_API_KEY}"

  formrecognizer:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer"
    deploy:
      resources:
        limits:
          cpus: '2'
          memory: 8g
        reservations:
          cpus: '1'
          memory: 4g
    environment:
      eula: accept
      billing: "{FORM_RECOGNIZER_ENDPOINT_URI}"
      apikey: "{FORM_RECOGNIZER_API_KEY}"
      FormRecognizer__ComputerVisionApiKey: {COMPUTER_VISION_API_KEY}
      FormRecognizer__ComputerVisionEndpointUri: "http://ocr:5000"
      FormRecognizer__SyncProcessTaskCancelLimitInSecs: 75
    links:
      - ocr
    volumes:
      - type: bind
        source: c:\output
        target: /output
      - type: bind
        source: c:\input
        target: /input
    ports:
      - "5000:5000"
```

> [!IMPORTANT]
> `Eula`Kapsayıcıyı `Billing` `FormRecognizer:ComputerVisionApiKey` çalıştırmak `ApiKey`için , ve `FormRecognizer:ComputerVisionEndpointUri` , ve , yanı sıra ve seçenekleri, belirtilmelidir; aksi takdirde, kapsayıcı başlamaz. Daha fazla bilgi için [Faturalandırma'ya](#billing)bakın.

## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin bitiş noktasını sorgula

|Kapsayıcı|Uç Nokta|
|--|--|
|form-tanıyan|http://localhost:5000

### <a name="form-recognizer"></a>Form Tanıma

Kapsayıcı, [Form Recognizer hizmetleri SDK belgeleri](https://docs.microsoft.com/azure/cognitive-services/form-recognizer/)aracılığıyla erişebildiğiniz websocket tabanlı sorgu uç noktası API'leri sağlar.

Varsayılan olarak, Form Tanıyan SDK çevrimiçi hizmetleri kullanır. Kapsayıcıyı kullanmak için başlatma yöntemini değiştirmeniz gerekir. Aşağıdaki örneklere bakın.

#### <a name="for-c"></a>C için #

Bu Azure bulutu başlatma çağrısını kullanarak değiştirin:

```csharp
var config =
    FormRecognizerConfig.FromSubscription(
        "YourSubscriptionKey",
        "YourServiceRegion");
```
kapsayıcı bitiş noktasını kullanan bu çağrıya:

```csharp
var config =
    FormRecognizerConfig.FromEndpoint(
        "ws://localhost:5000/formrecognizer/v1.0-preview/custom",
        "YourSubscriptionKey");
```

#### <a name="for-python"></a>Python için

Bu Azure bulutu başlatma çağrısını kullanarak değiştirin:

```python
formrecognizer_config =
    formrecognizersdk.FormRecognizerConfig(
        subscription=formrecognizer_key, region=service_region)
```

kapsayıcı bitiş noktasını kullanan bu çağrıya:

```python
formrecognizer_config = 
    formrecognizersdk.FormRecognizerConfig(
        subscription=formrecognizer_key,
        endpoint="ws://localhost:5000/formrecognizer/v1.0-preview/custom"
```

### <a name="form-recognizer"></a>Form Tanıma

Kapsayıcı, [Form Recognizer API](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/AnalyzeWithCustomModel) sayfasında bulabileceğiniz REST uç nokta API'leri sağlar.


[!INCLUDE [Validate container is running - Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]


## <a name="stop-the-container"></a>Kapsayıcıyı durdurma

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı çıktı [yuvası](form-recognizer-container-configuration.md#mount-settings) ve günlüğe kaydetme etkinken çalıştırıyorsanız, kapsayıcı kapsayıcıyı çalıştırırken veya çalıştırırken meydana gelen sorunları gidermeye yardımcı olan günlük dosyaları oluşturur.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Faturalandırma

Form Recognizer kapsayıcıları, Azure hesabınızdaki _form tanıyıcı_ kaynağını kullanarak fatura bilgilerini Azure'a gönderir.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için [yapı kaplarını yapıya kullanabilirsiniz.](form-recognizer-container-configuration.md)

<!--blogs/samples/video courses -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, Form Tanıyıcı kapsayıcılarını indirmek, yüklemek ve çalıştırmak için kavramları ve iş akışını öğrendiniz. Özet:

* Form Recognizer Docker için bir Linux konteyner sağlar.
* Kapsayıcı görüntüleri Azure'daki özel konteyner kayıt defterinden indirilir.
* Konteyner görüntüleri Docker'da çalışır.
* Kabın ana bilgisayarı URI'yi belirterek Form Recognizer kapsayıcısındaki işlemleri aramak için REST API veya REST SDK'yı kullanabilirsiniz.
* Bir kapsayıcıyı anında yaptığınızda fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
>  Bilişsel Hizmetler kapsayıcıları ölçüm için Azure'a bağlı olmadan çalışma lisansı na sahip değildir. Müşterilerin, konteynerlerin fatura bilgilerini her zaman ölçüm hizmetiyle iletmesini sağlaması gerekir. Bilişsel Hizmetler kapsayıcıları müşteri verilerini (örneğin, analiz edilen resim veya metin) Microsoft'a göndermez.

## <a name="next-steps"></a>Sonraki adımlar

* Yapılandırma ayarları için [kapsayıcıları yapılandırın.](form-recognizer-container-configuration.md)
* Daha fazla [Bilişsel Hizmet Kapsayıcıları](../cognitive-services-container-support.md)kullanın.
