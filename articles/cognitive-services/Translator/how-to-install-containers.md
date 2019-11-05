---
title: Kapsayıcıları yükleyip çalıştırma-Translator Metin Çevirisi
titleSuffix: Azure Cognitive Services
description: Bu adım adım öğreticide Translator Metin Çevirisi Analytics için kapsayıcıları indirme, yükleme ve çalıştırma.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: dapine
ms.openlocfilehash: 7b2b07f71d00e6da66062d1826f8c5c88bb6be7a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73507885"
---
# <a name="install-and-run-translator-text-containers"></a>Translator Metin Çevirisi kapsayıcıları yükleyip çalıştırma

<!--
    ACOM forward link:
    https://docs.microsoft.com/azure/cognitive-services/translator/howto-install-containers
-->

Kapsayıcılar, kendi ortamınızda Translator Metin Çevirisi API 'Leri çalıştırmanızı sağlar ve belirli güvenlik ve veri idare gereksinimleri için harika bir seçenektir.

## <a name="prerequisites"></a>Önkoşullar

Translator Metin Çevirisi kapsayıcıları kullanmadan önce aşağıdaki önkoşulları karşılamanız gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Bir [ana bilgisayarda](#the-host-computer)Docker altyapısının yüklü olması gerekir. Docker, [MacOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/)ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms)'ta Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temelleri hakkında bilgi için bkz. [Docker genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> **Windows 'da**Docker 'ın de Linux kapsayıcılarını destekleyecek şekilde yapılandırılması gerekir.<br><br>|
|Docker ile benzerlik | Kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri gibi Docker kavramlarından ve temel `docker` komutlarının bilgisine sahip olmanız gerekir.|

## <a name="request-access-to-the-container-registry"></a>Kapsayıcı kayıt defterine erişim isteme

Kapsayıcıya erişim istemek için önce bilişsel [Translator metin çevirisi kapsayıcıları istek formunu](https://aka.ms/translatorcontainerform) doldurmanız ve göndermeniz gerekir.

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

### <a name="the-host-computer"></a>Ana bilgisayar

Ana bilgisayar, Docker kapsayıcısını çalıştıran bir Linux işletim sistemi olan x64 tabanlı bir bilgisayardır. Şirket içinde veya Azure 'da bir Docker barındırma hizmeti olan bir bilgisayar olabilir; Örneğin:

* Azure Kubernetes hizmeti.
* Azure Container Instances.
* Azure Stack için dağıtılan bir [Kubernetes](https://kubernetes.io/) kümesi.

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda, her bir Translator Metin Çevirisi kapsayıcısı için ayırmak üzere en az 2,6 gigahertz (GHz) veya daha hızlı ve belleğin gigabayt (GB) cinsinden minimum ve önerilen CPU çekirdekleri açıklanmaktadır.

| Kapsayıcı | Minimum | Dil çifti |
|-----------|---------|---------------|
| Translator Metin Çevirisi | 4 çekirdek, 4 GB bellek | 4 |

Her dil çifti için, 1 GB bellek olması önerilir. Varsayılan olarak, Translator Metin Çevirisi kapsayıcısının çalıştırdığınız `<image-tag>` bağlı olarak 3 veya 4 dil çifti vardır. Ayrıntılar için bkz. [desteklenen diller ve çeviri](#supported-languages-and-translation) . Çekirdek ve bellek, `docker run` komutunun bir parçası olarak kullanılan `--cpus` ve `--memory` ayarlarına karşılık gelir.

## <a name="get-the-container-image-with-docker-pull"></a>`docker pull` kapsayıcı görüntüsünü al

Translator Metin Çevirisi için kapsayıcı görüntüleri aşağıdaki kapsayıcı deposunda kullanılabilir. Tablo aynı zamanda kapsayıcı görüntüsü etiketlerini ve bunlara karşılık gelen desteklenen dilleri de eşler.

| Kapsayıcı | Resim etiketi | Desteklenen diller |
|-----------|-----------|---------------------|
| `containerpreview.azurecr.io/microsoft/cognitive-services-translator-text` | `ar_de_en_ru_zh_1.0.0` | `ar-SA`, `zh-CN`, `de-DE`ve `ru-RU` |
| `containerpreview.azurecr.io/microsoft/cognitive-services-translator-text` | `de_en_es_fr_1.0.0` | `de-DE`, `fr-FR`ve `es-ES` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-translator-text-container"></a>Translator Metin Çevirisi kapsayıcısı için Docker Pull

[`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) komutunu gerçekleştirmek için önce kapsayıcı kayıt defterine erişmeniz gerekir. Azure CLı 'dan [`az acr login`](https://docs.microsoft.com/cli/azure/acr?view=azure-cli-latest#az-acr-login) komutunu kullanarak Azure Container Registry oturum açabilirsiniz.

```azureinteractive
az acr login --name containerpreview.azurecr.io
```

Bu komut, geçerli kullanıcı için karşılık gelen Azure Container Registry kimliğini doğrular. Şimdi `docker pull` komutunu yürütmek için ücretsiz olursunuz.

> [!NOTE]
> İhtiyaç duyduğunuz dil desteğine bağlı olarak, karşılık gelen `<image-tag>`sağlayın.

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-translator-text:<image-tag>
```

## <a name="how-to-use-the-container"></a>Kapsayıcıyı kullanma

Kapsayıcı [ana bilgisayardan](#the-host-computer)olduktan sonra, kapsayıcında çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı çalıştırın](#run-the-container-with-docker-run). `docker run` komutuna daha fazla [örnek](translator-text-container-config.md#example-docker-run-commands) kullanılabilir.
1. [Kapsayıcının çeviri uç noktasını sorgulayın](#query-the-containers-translate-endpoint).

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı `docker run` ile çalıştırma

Üç kapsayıcının herhangi birini çalıştırmak için [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. `docker run` komut [örnekleri](translator-text-container-config.md#example-docker-run-commands) mevcuttur.

### <a name="run-container-example-of-docker-run-command"></a>Docker Run komutunun kapsayıcı örneğini Çalıştır

```Docker
docker run --rm -it -p 5000:80 --memory 4g --cpus 4 \
containerpreview.azurecr.io/microsoft/cognitive-services-translator-text:ar_de_en_ru_zh_1.0.0 \
Eula=accept
```

Bu komut:

* Kapsayıcı görüntüsünden bir Translator Metin Çevirisi kapsayıcısı çalıştırır
* 4 CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır
* TCP bağlantı noktası 5000 ' i kullanıma sunar ve kapsayıcı için bir sözde TTY ayırır
* Son Kullanıcı sözleşmesini (EULA) kabul eder
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü, ana bilgisayarda hala kullanılabilir

### <a name="how-to-collect-docker-logs"></a>Docker günlüklerini toplama

Sorun giderme amacıyla, kapsayıcının yürütmesindeki Docker günlüklerini görüntülemek faydalı olabilir. İlk olarak, tüm kapsayıcılar için görüntülenecek ayrıntıları sınırlamak üzere biçimlendirme bayrağıyla [Docker PS](https://docs.docker.com/engine/reference/commandline/ps/) komutunu yürütün.

```Docker
docker ps -a --format "table {{.ID}}\t{{.Image}}\t{{.Status}}"

CONTAINER ID    IMAGE                                                                       STATUS
ed6f115697f3    containerpreview.azurecr.io/microsoft/cognitive-services-translator-text    Up 4 minutes
```

Ardından [Docker logs](https://docs.docker.com/engine/reference/commandline/logs/) komutunu, söz konusu kapsayıcının günlüklerini görüntülemek için ilgili kapsayıcı için `<Container ID>` çalıştırın.

```Docker
docker logs <Container ID> --timestamps --since=4h | grep Error
```

Yukarıdaki Docker logs komutu, son dört saat için "hata" günlüklerini toplar.

## <a name="supported-languages-and-translation"></a>Desteklenen diller ve çeviri

`POST /translate` yöntemi, *İngilizce* 'den hedef dile veya tam tersi yönde hareket eden aşağıdaki dil dönüştürmelerini destekler. Listelenen dillerden biriyle Ingilizce 'ye gidebileceğinizi *ve İngilizce dışındaki bir dilden* başka bir *İngilizce olmayan* *dile gidebileceğinizi* lütfen unutmayın.

> [!NOTE]
> En iyi kalite için, tüketiciler istek başına yalnızca bir tümce göndermelidir.

| Dil dönüştürme | Dil ISO dönüşümü | Resim etiketleri |
|--|--|:--|
| İngilizce: left_right_arrow: Çince | `en-US`: left_right_arrow: `zh-CN` | `ar_de_en_ru_zh_1.0.0` |
| İngilizce: left_right_arrow: Rusça | `en-US`: left_right_arrow: `ru-RU` | `ar_de_en_ru_zh_1.0.0` |
| İngilizce: left_right_arrow: Arapça | `en-US`: left_right_arrow: `ar-SA` | `ar_de_en_ru_zh_1.0.0` |
| İngilizce: left_right_arrow: Almanca | `en-US`: left_right_arrow: `de-DE` | `ar_de_en_ru_zh_1.0.0` ve `de_en_es_fr_1.0.0` |
| İngilizce: left_right_arrow: Ispanyolca | `en-US`: left_right_arrow: `es-ES` | `de_en_es_fr_1.0.0` |
| İngilizce: left_right_arrow: Fransızca | `en-US`: left_right_arrow: `fr-FR` | `de_en_es_fr_1.0.0` |

> [!IMPORTANT]
> Kapsayıcıyı çalıştırmak için `Eula` belirtilmelidir; Aksi takdirde, kapsayıcı başlatılmaz.

## <a name="query-the-containers-translate-endpoint"></a>Kapsayıcının çeviri uç noktasını sorgulama

Kapsayıcı, REST tabanlı çeviri uç noktası API 'SI sağlar. Bu uç noktanın birkaç örnek kullanımı şunlardır:

# <a name="curltabcurl"></a>[cURL](#tab/curl)


İstediğiniz CLı 'dan aşağıdaki kıvrımlı komutunu yürütün.

```curl
curl -X POST "http://localhost:5000/translate?api-version=3.0&from=en-US&to=de-DE"
    -H "Content-Type: application/json" -d "[{'Text':'hello, how are you'}]"
```

> [!TIP]
> Kapsayıcı hazırlanmadan önce bu kıvrımlı GÖNDERIYI denerseniz, "hizmet geçici olarak kullanılamıyor" yanıtını alacaksınız. Kapsayıcının hazırlanana kadar beklemeniz yeterlidir, sonra yeniden deneyin.

Aşağıdaki çıktı konsola yazdırılır.

```console
"translations": [
    {
        "text": "hallo, wie geht es dir",
        "to": "de-DE"
    }
]
```

# <a name="swaggertabswagger"></a>[Swagger](#tab/Swagger)

Swagger sayfasına gidin http://localhost:5000/swagger/index.html

1. **/Translate gönderi** seçin
1. **Deneyin** seçeneğini belirleyin
1. **From** parametresini `en-US` olarak girin
1. **To** parametresini `de-DE` olarak girin
1. **Api sürümü** parametresini `3.0` olarak girin
1. **Metin**altında `string` aşağıdaki JSON ile değiştirin
    ```json
    [
        {
            "text": "hello, how are you"
        }
    ]
    ```
1. **Yürüt**' ü seçtiğinizde, elde edilen çeviriler **yanıt gövdesinde**çıkış olur. Aşağıdakine benzer bir şey beklemelisiniz:
    ```json
    "translations": [
      {
          "text": "hallo, wie geht es dir",
          "to": "de-DE"
      }
    ]
    ```

# <a name="net-coretabnetcore"></a>[.NET Core](#tab/netcore)

1. Visual Studio 'yu başlatın ve yeni bir konsol uygulaması oluşturun.
1. `<LangVersion>7.1</LangVersion>` düğümü eklemek için `*.csproj` dosyasını düzenleyin-bu 7,1 belirtir C# .
1. [Newtonsoft. JSON](https://www.nuget.org/packages/Newtonsoft.Json/) NuGet paketini, sürüm 11.0.2 ekleyin.
1. `Program.cs`, tüm mevcut kodu aşağıdaki kodla değiştirin:
    ```csharp
    using Newtonsoft.Json;
    using System;
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;

    namespace TranslateContainer
    {
        class Program
        {
            const string ApiHostEndpoint = "http://localhost:5000";
            const string TranslateApi = "/translate?api-version=3.0&from=en-US&to=de-DE";

            static async Task Main(string[] args)
            {
                var textToTranslate = "hello, how are you";
                var result = await TranslateTextAsync(textToTranslate);

                Console.WriteLine(result);
                Console.ReadLine();
            }

            static async Task<string> TranslateTextAsync(string textToTranslate)
            {
                var body = new object[] { new { Text = textToTranslate } };
                var requestBody = JsonConvert.SerializeObject(body);

                var client = new HttpClient();
                using (var request =
                    new HttpRequestMessage
                    {
                        Method = HttpMethod.Post,
                        RequestUri = new Uri($"{ApiHostEndpoint}{TranslateApi}"),
                        Content = new StringContent(requestBody, Encoding.UTF8, "application/json")
                    })
                {
                    // Send the request and await a response.
                    var response = await client.SendAsync(request);

                    return await response.Content.ReadAsStringAsync();
                }
            }
        }
    }
    ```
1. Programı çalıştırmak için **F5** tuşuna basın.
1. Aşağıdaki çıktı konsola yazdırılır.
    ```console
    "translations": [
      {
          "text": "hallo, wie geht es dir",
          "to": "de-DE"
      }
    ]
    ```

***

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcıyı durdur

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı bir çıkış [bağlaması](translator-text-container-config.md#mount-settings) ve günlüğü etkin olarak çalıştırırsanız kapsayıcı, kapsayıcıyı başlatırken veya çalıştırırken oluşan sorunları gidermek için yararlı olan günlük dosyaları oluşturur.

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, Translator Metin Çevirisi kapsayıcıları indirmek, yüklemek ve çalıştırmak için kavramları ve iş akışını öğrendiniz. Özet:

* Translator Metin Çevirisi, çeşitli dil çiftlerini kapsülleyerek Docker için birden çok Linux kapsayıcısı sağlar.
* Kapsayıcı görüntüleri "kapsayıcı önizlemesi" kayıt defterinden indirilir.
* Kapsayıcı görüntüleri Docker 'da çalışır.
* Kapsayıcının ana bilgisayar URI 'sini belirterek Translator Metin Çevirisi kapsayıcılarındaki işlemleri çağırmak için REST API veya SDK kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Yapılandırma ayarları için [kapsayıcıları](translator-text-container-config.md) yapılandırmayı gözden geçir
* İşlevlerle ilgili sorunları çözmek için [kapsayıcı hakkında sık sorulan sorular (SSS)](../containers/container-faq.md) bölümüne bakın.
