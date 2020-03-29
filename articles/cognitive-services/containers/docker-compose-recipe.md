---
title: Docker Compose kullanarak birden çok kapsayıcı dağıtma
titleSuffix: Azure Cognitive Services
description: Birden çok Bilişsel Hizmet kapsayıcısını nasıl dağıtabileceğinizi öğrenin. Bu makalede, Docker Compose kullanarak birden fazla Docker konteyner görüntüleri düzenlemek için nasıl gösterir.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: dapine
ms.openlocfilehash: bfbaa03469ee04ff900a215aadd8c814efcba761
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79037530"
---
# <a name="use-docker-compose-to-deploy-multiple-containers"></a>Docker Compose kullanarak birden çok kapsayıcı dağıtma

Bu makalede, birden çok Azure Bilişsel Hizmet kapsayıcısı nasıl dağıtılacağınızda gösterilmektedir. Özellikle, birden fazla Docker konteyner görüntüsünü düzenlemek için Docker Compose'u nasıl kullanacağınızı öğreneceksiniz.

> [Docker Compose,](https://docs.docker.com/compose/) çok konteynerli Docker uygulamalarını tanımlamak ve çalıştırmak için bir araçtır. Compose'de, uygulamanızın hizmetlerini yapılandırmak için bir YAML dosyası kullanırsınız. Ardından, tek bir komut çalıştırarak yapılandırmanızdaki tüm hizmetleri oluşturur ve başlatırsınız.

Tek bir ana bilgisayarda birden çok kapsayıcı görüntüsünü düzenlemek yararlı olabilir. Bu makalede, Okuma ve Form Tanıyıcı kapsayıcılarını bir araya getireceğiz.

## <a name="prerequisites"></a>Ön koşullar

Bu yordam, yüklenmesi ve yerel olarak çalıştırılması gereken çeşitli araçlar gerektirir:

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* [Docker Motoru](https://www.docker.com/products/docker-engine). Docker CLI'nin konsol penceresinde çalıştığını doğrulayın.
* Doğru fiyatlandırma katmanına sahip bir Azure kaynağı. Yalnızca aşağıdaki fiyatlandırma katmanları bu kapsayıcıyla çalışır:
  * **Yalnızca** F0 veya Standart fiyatlandırma katmanına sahip Bilgisayar Lı Vizyon kaynağı.
  * Yalnızca F0 veya Standart fiyatlandırma katmanına sahip **Form Tanıyıcı** kaynağı.
  * S0 fiyatlandırma katmanı ile **Bilişsel Hizmetler** kaynak.

## <a name="request-access-to-the-container-registry"></a>Konteyner kayıt defterine erişim isteği

Bilişsel Hizmetler [Konuşma Kapları İstek formunu](https://aka.ms/speechcontainerspreview/)doldurun ve gönderin. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="docker-compose-file"></a>Docker Oluştur dosyası

YAML dosyası dağıtılacak tüm hizmetleri tanımlar. Bu hizmetler, varolan bir `DockerFile` kapsayıcı görüntüsüne dayanır. Bu durumda, iki önizleme görüntüsü kullanırız. Aşağıdaki YAML dosyasını kopyalayıp yapıştırın ve *docker-compose.yaml*olarak kaydedin. Dosyada uygun **apikey,** **faturalama**ve **EndpointUri** değerlerini sağlayın.

```yaml
version: '3.7'
services:
  forms:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer"
    environment:
       eula: accept
       billing: # < Your form recognizer billing URL >
       apikey: # < Your form recognizer API key >
       FormRecognizer__ComputerVisionApiKey: # < Your form recognizer API key >
       FormRecognizer__ComputerVisionEndpointUri: # < Your form recognizer URI >
    volumes:
       - type: bind
         source: E:\publicpreview\output
         target: /output
       - type: bind
         source: E:\publicpreview\input
         target: /input
    ports:
      - "5010:5000"

  ocr:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-read"
    environment:
      eula: accept
      apikey: # < Your computer vision API key >
      billing: # < Your computer vision billing URL >
    ports:
      - "5021:5000"
```

> [!IMPORTANT]
> **Birim** düğümüaltında belirtilen ana bilgisayar makinesinde dizinler oluşturun. Ses bağlamaları kullanarak bir görüntü bağlamaya çalışmadan önce dizinlerin bulunması gerektiğinden, bu yaklaşım gereklidir.

## <a name="start-the-configured-docker-compose-services"></a>Yapılandırılmış Docker Compose hizmetlerini başlatın

Docker Compose dosyası, tanımlanmış bir hizmetin yaşam döngüsündeki tüm aşamaların yönetilmesini sağlar: hizmetlerin başlatılması, durdurulması ve yeniden oluşturulması; hizmet durumunu görüntüleme; ve günlük akışı. Proje dizininden (docker-compose.yaml dosyasının bulunduğu) bir komut satırı arabirimi açın.

> [!NOTE]
> Hataları önlemek için, ana makinenin sürücüleri Docker Engine ile doğru şekilde paylaştığından emin olun. Örneğin, *Docker-compose.yaml* dosyasında *E:\publicpreview* dizin olarak kullanılıyorsa, **E** sürücüsünu Docker ile paylaşın.

Komut satırı arabiriminden *docker-compose.yaml* dosyasında tanımlanan tüm hizmetleri başlatmak (başlatmak veya yeniden başlatmak) için aşağıdaki komutu uygulayın:

```console
docker-compose up
```

Docker bu yapılandırmayı kullanarak **docker-compose** komutunu ilk kez çalıştırır, **hizmet** düğümü altında yapılandırılan görüntüleri çeker ve sonra indirir ve monte eder:

```console
Pulling forms (containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer:)...
latest: Pulling from microsoft/cognitive-services-form-recognizer
743f2d6c1f65: Pull complete
72befba99561: Pull complete
2a40b9192d02: Pull complete
c7715c9d5c33: Pull complete
f0b33959f1c4: Pull complete
b8ab86c6ab26: Pull complete
41940c21ed3c: Pull complete
e3d37dd258d4: Pull complete
cdb5eb761109: Pull complete
fd93b5f95865: Pull complete
ef41dcbc5857: Pull complete
4d05c86a4178: Pull complete
34e811d37201: Pull complete
Pulling ocr (containerpreview.azurecr.io/microsoft/cognitive-services-read:)...
latest: Pulling from microsoft/cognitive-services-read
f476d66f5408: Already exists
8882c27f669e: Already exists
d9af21273955: Already exists
f5029279ec12: Already exists
1a578849dcd1: Pull complete
45064b1ab0bf: Download complete
4bb846705268: Downloading [=========================================>         ]  187.1MB/222.8MB
c56511552241: Waiting
e91d2aa0f1ad: Downloading [==============================================>    ]  162.2MB/176.1MB
```

Görüntüler indirildikten sonra görüntü hizmetleri başlatılır:

```console
Starting docker_ocr_1   ... done
Starting docker_forms_1 ... doneAttaching to docker_ocr_1, docker_forms_1forms_1  | forms_1  | forms_1  | Notice: This Preview is made available to you on the condition that you agree to the Supplemental Terms of Use for Microsoft Azure Previews [https://go.microsoft.com/fwlink/?linkid=2018815], which supplement your agreement [https://go.microsoft.com/fwlink/?linkid=2018657] governing your use of Azure. If you do not have an existing agreement governing your use of Azure, you agree that your agreement governing use of Azure is the Microsoft Online Subscription Agreement [https://go.microsoft.com/fwlink/?linkid=2018755] (which incorporates the Online Services Terms [https://go.microsoft.com/fwlink/?linkid=2018760]). By using the Preview you agree to these terms.
forms_1  | 
forms_1  | 
forms_1  | Using '/input' for reading models and other read-only data.
forms_1  | Using '/output/forms/812d811d1bcc' for writing logs and other output data.
forms_1  | Logging to console.
forms_1  | Submitting metering to 'https://westus2.api.cognitive.microsoft.com/'.
forms_1  | WARNING: No access control enabled!
forms_1  | warn: Microsoft.AspNetCore.Server.Kestrel[0]
forms_1  |       Overriding address(es) 'http://+:80'. Binding to endpoints defined in UseKestrel() instead.
forms_1  | Hosting environment: Production
forms_1  | Content root path: /app/forms
forms_1  | Now listening on: http://0.0.0.0:5000
forms_1  | Application started. Press Ctrl+C to shut down.
ocr_1    | 
ocr_1    | 
ocr_1    | Notice: This Preview is made available to you on the condition that you agree to the Supplemental Terms of Use for Microsoft Azure Previews [https://go.microsoft.com/fwlink/?linkid=2018815], which supplement your agreement [https://go.microsoft.com/fwlink/?linkid=2018657] governing your use of Azure. If you do not have an existing agreement governing your use of Azure, you agree that your agreement governing use of Azure is the Microsoft Online Subscription Agreement [https://go.microsoft.com/fwlink/?linkid=2018755] (which incorporates the Online Services Terms [https://go.microsoft.com/fwlink/?linkid=2018760]). By using the Preview you agree to these terms.
ocr_1    |
ocr_1    | 
ocr_1    | Logging to console.
ocr_1    | Submitting metering to 'https://westcentralus.api.cognitive.microsoft.com/'.
ocr_1    | WARNING: No access control enabled!
ocr_1    | Hosting environment: Production
ocr_1    | Content root path: /
ocr_1    | Now listening on: http://0.0.0.0:5000
ocr_1    | Application started. Press Ctrl+C to shut down.
```

## <a name="verify-the-service-availability"></a>Hizmet kullanılabilirliğini doğrulama

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

Aşağıda bazı örnek çıktı verilmiştir:

```
IMAGE ID            REPOSITORY                                                                 TAG
2ce533f88e80        containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer   latest
4be104c126c5        containerpreview.azurecr.io/microsoft/cognitive-services-read              latest
```

### <a name="test-containers"></a>Test kapları

Ana bilgisayar makinesinde bir tarayıcı açın ve *docker-compose.yaml* dosyasından belirtilen bağlantı http://localhost:5021/swagger/index.htmlnoktasını kullanarak **localhost'a** gidin. Örneğin, Form Tanıyıcı bitiş noktasını sınamak için API'deki **Try It** özelliğini kullanabilirsiniz. Her iki kapsayıcı swagger sayfaları kullanılabilir ve test edilebilir olmalıdır.

![Form Tanıyıcı Konteyner](media/form-recognizer-swagger-page.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilişsel Hizmetler konteynerler](../cognitive-services-container-support.md)
