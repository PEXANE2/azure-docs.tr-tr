---
title: Docker konteynerler için tarifler
titleSuffix: Azure Cognitive Services
description: Dağıtım ve yeniden kullanım için yapılandırma ayarlarınızın bazılarıveya tümüyle kapsayıcıları nasıl oluşturup, sınamayı ve depolayacağınızı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 7380ff58d033a68565de7e419ff318f7bdec121d
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80875087"
---
# <a name="create-containers-for-reuse"></a>Yeniden kullanım için kapsayıcı oluşturma

Yeniden kullanılabilen Bilişsel Hizmetler Kapsayıcıları oluşturmak için bu kapsayıcı tariflerini kullanın. Kapsayıcı başlatıldığında gerekli _olmayacak_ şekilde bazı veya tüm yapılandırma ayarları ile kaplanabilir.

Bu yeni kapsayıcı katmanına (ayarlarla) sahip olduktan ve yerel olarak test edildikten sonra, kapsayıcıyı bir kapsayıcı kayıt defterinde depolayabilirsiniz. Kapsayıcı başlatıldığında, yalnızca şu anda kapsayıcıda depolanmayan ayarları n gerekir. Özel kayıt defteri kapsayıcısı, bu ayarları geçirmeniz için yapılandırma alanı sağlar.

## <a name="docker-run-syntax"></a>Docker çalıştır sözdizimini çalıştırın

Bu `docker run` belgedeki örnekler, satır devam `^` karakteri olan bir Windows konsolu varsayar. Kendi kullanımınız için aşağıdakileri göz önünde bulundurun:

* Docker konteynerleri hakkında çok bilginiz olmadıkça bağımsız değişkenlerin sırasını değiştirmeyin.
* Windows dışında bir işletim sistemi veya Windows konsolu dışındaki bir konsol kullanıyorsanız, konsolunuz ve sisteminiz için doğru konsol/terminal, bağlar için klasör sözdizimini ve satır devam karakterini kullanın.  Bilişsel Hizmetler kapsayıcısı bir Linux işletim sistemi olduğundan, hedef montaj Linux tarzı bir klasör sözdizimi kullanır.
* `docker run`örnekler, Windows'ta `c:` herhangi bir izin çakışmasını önlemek için sürücüüzerindeki dizini kullanır. Giriş dizini olarak belirli bir dizini kullanmanız gerekiyorsa, docker hizmeti ne izin verebilirsiniz.

## <a name="store-no-configuration-settings-in-image"></a>Görüntüde yapılandırma ayarlarını depolama

Her `docker run` hizmet için örnek komutlar kapsayıcıda herhangi bir yapılandırma ayarı saklamaz. Kapsayıcıyı bir konsoldan veya kayıt defteri hizmetinden başlattığınızda, bu yapılandırma ayarlarının geçmesi gerekir. Özel kayıt defteri kapsayıcısı, bu ayarları geçirmeniz için yapılandırma alanı sağlar.

## <a name="reuse-recipe-store-all-configuration-settings-with-container"></a>Tarifi yeniden kullanın: tüm yapılandırma ayarlarını konteynerle birlikte saklayın

Tüm yapılandırma ayarlarını depolamak `Dockerfile` için, bu ayarlarla bir ayar oluşturun.

Bu yaklaşımla ilgili sorunlar:

* Yeni kapsayıcının orijinal kapsayıcıdan ayrı bir adı ve etiketi vardır.
* Bu ayarları değiştirmek için Dockerfile'ın değerlerini değiştirmeniz, görüntüyü yeniden oluşturmanız ve kayıt defterinize yeniden yayımlamanız gerekir.
* Birisi konteyner kayıt defterinize veya yerel ana bilgisayara erişirse, kapsayıcıyı çalıştırabilir ve Bilişsel Hizmetler bitiş noktalarını kullanabilir.
* Bilişsel Hizmetinizin giriş bağlantıları yoksa, `COPY` satırları Dockerfile'ınıza eklemeyin.

Kullanmak istediğiniz mevcut Bilişsel Hizmetler kapsayıcısından çekerek Dockerfile'ı oluşturun ve ardından konteynerin ihtiyaç duyduğu bilgileri ayarlamak veya çekmek için Dockerdosyasındaki docker komutlarını kullanın.

Bu örnek:

* Faturalandırma bitiş noktasını, `{BILLING_ENDPOINT}` ana bilgisayar ortamı anahtarını kullanarak `ENV`ayarlar.
* 'ENV' kullanarak ana `{ENDPOINT_KEY}` bilgisayar ortamı anahtarından faturaapi anahtarını ayarlar.

### <a name="reuse-recipe-store-billing-settings-with-container"></a>Tarifi yeniden kullanın: fatura ayarlarını konteynerle birlikte saklayın

Bu örnek, Metin Analizi'nin duyarlılık kapsayıcısının Dockerfile'dan nasıl oluşturulabildiğini gösterir.

```Dockerfile
FROM mcr.microsoft.com/azure-cognitive-services/sentiment:latest
ENV billing={BILLING_ENDPOINT}
ENV apikey={ENDPOINT_KEY}
ENV EULA=accept
```

Konteyneri yerel [olarak](#how-to-use-container-on-your-local-host) veya gerektiğinde [özel kayıt defteri kapsayıcınızdan](#how-to-add-container-to-private-registry) oluşturun ve çalıştırın.

### <a name="reuse-recipe-store-billing-and-mount-settings-with-container"></a>Tarifi yeniden kullanın: konteyner ile mağaza faturalandırma ve montaj ayarları

Bu örnek, Dockerfile'dan faturalandırma yı ve modelleri kaydederek Dil Anlayışının nasıl kullanılacağını gösterir.

* Dil Anlayışı (LUIS) model dosyasını ana bilgisayar dosya `COPY`sisteminden kopyalar.
* LUIS kapsayıcı birden fazla modeli destekler. Tüm modeller aynı klasörde depolanırsa, hepinizin `COPY` tek bir ifadeye ihtiyacı vardır.
* Model giriş dizininin göreli üst öğesinden docker dosyasını çalıştırın. Aşağıdaki örnekiçin, 'nin göreli üst öğesinden `docker build` ve `docker run` komutlarını çalıştırın `/input` Komuttaki `/input` `COPY` ilk ana bilgisayar dizinidir. İkincisi `/input` konteynerin dizini.

```Dockerfile
FROM <container-registry>/<cognitive-service-container-name>:<tag>
ENV billing={BILLING_ENDPOINT}
ENV apikey={ENDPOINT_KEY}
ENV EULA=accept
COPY /input /input
```

Konteyneri yerel [olarak](#how-to-use-container-on-your-local-host) veya gerektiğinde [özel kayıt defteri kapsayıcınızdan](#how-to-add-container-to-private-registry) oluşturun ve çalıştırın.

## <a name="how-to-use-container-on-your-local-host"></a>Yerel ana bilgisayarda kapsayıcı nasıl kullanılır?

Docker dosyasını oluşturmak `<your-image-name>` için resmin yeni adı ile değiştirin ve ardından şunları kullanın:

```console
docker build -t <your-image-name> .
```

Görüntüyü çalıştırmak ve kapsayıcı durduğunda kaldırmak`--rm`için ( ):

```console
docker run --rm <your-image-name>
```

## <a name="how-to-add-container-to-private-registry"></a>Özel kayıt defterine kapsayıcı ekleme

Dockerfile'ı kullanmak ve yeni resmi özel kapsayıcı kayıt defterinize yerleştirmek için aşağıdaki adımları izleyin.  

1. Yeniden `Dockerfile` kullanma tarifinden bir metin oluşturun. A'nın `Dockerfile` uzantısı yoktur.

1. Açı parantezindeki tüm değerleri kendi değerlerinizle değiştirin.

1. Aşağıdaki komutu kullanarak dosyayı komut satırında veya terminalde bir görüntüye dönüştürün. Açı braketleri değerleri değiştirin, `<>`kendi kapsayıcı adı ve etiketi ile.  

    Etiket seçeneği, `-t`kapsayıcı için ne değişti hakkında bilgi eklemek için bir yoldur. Örneğin, bir kapsayıcı `modified-LUIS` adı özgün kapsayıcı katmanlı olduğunu gösterir. Dil Bilgisi `with-billing-and-model` (LUIS) kapsayıcısının nasıl değiştirildiğini gösteren bir etiket adı.

    ```Bash
    docker build -t <your-new-container-name>:<your-new-tag-name> .
    ```

1. Konsoldan Azure CLI'de oturum açın. Bu komut bir tarayıcı açar ve kimlik doğrulaması gerektirir. Kimlik doğrulaması alındıktan sonra tarayıcıyı kapatıp konsolda çalışmaya devam edebilirsiniz.

    ```azurecli
    az login
    ```

1. Konsoldan Azure CLI ile özel kayıt defterinizde oturum açın.

    Açı parantezindeki `<my-registry>`değerleri kendi kayıt defteri adınız ile değiştirin.  

    ```azurecli
    az acr login --name <my-registry>
    ```

    Ayrıca, bir hizmet müdürü atanmışsa docker girişi ile de oturum açabilirsiniz.

    ```Bash
    docker login <my-registry>.azurecr.io
    ```

1. Kapsayıcıyı özel kayıt defteri konumuyla etiketleyin. Açı parantezindeki `<my-registry>`değerleri kendi kayıt defteri adınız ile değiştirin. 

    ```Bash
    docker tag <your-new-container-name>:<your-new-tag-name> <my-registry>.azurecr.io/<your-new-container-name-in-registry>:<your-new-tag-name>
    ```

    Etiket adı kullanmıyorsanız, `latest` ima edilir.

1. Yeni görüntüyü özel konteyner kayıt defterinize itin. Özel konteyner kayıt defterinizi görüntülediğinizde, aşağıdaki CLI komutunda kullanılan kapsayıcı adı deponun adı olacaktır.

    ```Bash
    docker push <my-registry>.azurecr.io/<your-new-container-name-in-registry>:<your-new-tag-name>
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Kapsayıcı Örneği oluşturma ve kullanma](azure-container-instance-recipe.md)

<!--
## Store input and output configuration settings

Bake in input params only

FROM containerpreview.azurecr.io/microsoft/cognitive-services-luis:<tag>
COPY luisModel1 /input/
COPY luisModel2 /input/

## Store all configuration settings

If you are a single manager of the container, you may want to store all settings in the container. The new, resulting container will not need any variables passed in to run. 

Issues with this approach:

* In order to change these settings, you will have to change the values of the Dockerfile and rebuild the file. 
* If someone gets access to your container registry or your local host, they can run the container and use the Cognitive Services endpoints. 

The following _partial_ Dockerfile shows how to statically set the values for billing and model. This example uses the 

```Dockerfile
FROM <container-registry>/<cognitive-service-container-name>:<tag>
ENV billing=<billing value>
ENV apikey=<apikey value>
COPY luisModel1 /input/
COPY luisModel2 /input/
```

->