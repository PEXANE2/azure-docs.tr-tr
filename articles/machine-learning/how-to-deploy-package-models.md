---
title: Paket modelleri
titleSuffix: Azure Machine Learning
description: Bir modeli Dockerfile olarak paketleme
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: gopalv
author: gvashishtha
ms.date: 07/31/2020
ms.topic: conceptual
ms.openlocfilehash: d5fb2539d79c31de5a5e0196a7a4814c02a84602
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87544654"
---
# <a name="how-to-package-a-registered-model-with-docker"></a>Docker ile kayıtlı bir modeli paketleme

Bu makalede, kayıtlı bir Azure Machine Learning modelinin Docker ile nasıl paketlenecek gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, Machine Learning çalışma alanınızda zaten bir modeli eğitidiğinizi ve kaydettiğiniz varsayılmaktadır. Bir scikit-öğrenme modelini eğitme ve kaydetme hakkında bilgi edinmek için [Bu öğreticiyi izleyin](how-to-train-scikit-learn.md).


## <a name="package-models"></a>Paket modelleri

Bazı durumlarda, modeli dağıtmadan bir Docker görüntüsü oluşturmak isteyebilirsiniz (örneğin, [Azure App Service dağıtmayı](how-to-deploy-app-service.md)planlıyorsanız). Ya da görüntüyü indirmek ve yerel bir Docker yüklemesinde çalıştırmak isteyebilirsiniz. Görüntüyü oluşturmak, incelemek, değiştirmek ve görüntüyü el ile oluşturmak için kullanılan dosyaları da indirmek isteyebilirsiniz.

Model paketleme, bu işlemleri yapmanızı sağlar. Bir modeli Web hizmeti olarak barındırmak için gereken tüm varlıkları paketler ve tamamen oluşturulmuş bir Docker görüntüsünü ya da bir tane oluşturmak için gereken dosyaları indirmelerini sağlar. Model paketlemeyi kullanmanın iki yolu vardır:

**Paketlenmiş bir model indirin:** Modeli ve bir Web hizmeti olarak barındırmak için gereken diğer dosyaları içeren bir Docker görüntüsü indirin.

**Dockerfile oluştur:** Docker görüntüsü oluşturmak için gereken Dockerfile, model, giriş betiği ve diğer varlıkları indirin. Görüntüyü yerel olarak oluşturmadan önce dosyaları inceleyebilir veya değişiklik yapabilirsiniz.

Her iki paket de yerel bir Docker görüntüsü almak için kullanılabilir.

> [!TIP]
> Bir paket oluşturmak, model dağıtmaya benzer. Kayıtlı bir model ve çıkarım yapılandırmasını kullanırsınız.

> [!IMPORTANT]
> Tamamen oluşturulmuş bir görüntüyü indirmek veya yerel olarak bir görüntü oluşturmak için, geliştirme ortamınızda [Docker](https://www.docker.com) 'ın yüklü olması gerekir.

### <a name="download-a-packaged-model"></a>Paketlenmiş model indirme

Aşağıdaki örnek, çalışma alanınız için Azure Container Registry 'de kayıtlı olan bir görüntü oluşturur:

```python
package = Model.package(ws, [model], inference_config)
package.wait_for_creation(show_output=True)
```

Bir paket oluşturduktan sonra, `package.pull()` görüntüyü yerel Docker ortamınıza çekmek için ' i kullanabilirsiniz. Bu komutun çıktısı görüntünün adını görüntüler. Örneğin: 

`Status: Downloaded newer image for myworkspacef78fd10.azurecr.io/package:20190822181338`. 

Modeli indirdikten sonra, `docker images` yerel görüntüleri listelemek için komutunu kullanın:

```text
REPOSITORY                               TAG                 IMAGE ID            CREATED             SIZE
myworkspacef78fd10.azurecr.io/package    20190822181338      7ff48015d5bd        4 minutes ago       1.43 GB
```

Bu görüntüye göre yerel bir kapsayıcı başlatmak için, Shell veya komut satırından adlandırılmış bir kapsayıcı başlatmak üzere aşağıdaki komutu kullanın. Değeri, `<imageid>` komutu tarafından döndürülen görüntü kimliğiyle değiştirin `docker images` .

```bash
docker run -p 6789:5001 --name mycontainer <imageid>
```

Bu komut, adlı görüntünün en son sürümünü başlatır `myimage` . Yerel bağlantı noktası 6789 ' i Web hizmetinin dinlediği kapsayıcıda bağlantı noktasına eşler (5001). Ayrıca `mycontainer` , kapsayıcıyı daha kolay durdurmayı sağlayan kapsayıcıya de atar. Kapsayıcı başlatıldıktan sonra, istek gönderebilirsiniz `http://localhost:6789/score` .

### <a name="generate-a-dockerfile-and-dependencies"></a>Dockerfile ve bağımlılıklar oluşturma

Aşağıdaki örnek, bir görüntüyü yerel olarak oluşturmak için gereken Dockerfile, model ve diğer varlıkların nasıl indirileceği gösterilmektedir. `generate_dockerfile=True`Parametresi, tam olarak oluşturulmuş bir görüntü değil, dosyaları istediğinizi belirtir.

```python
package = Model.package(ws, [model], inference_config, generate_dockerfile=True)
package.wait_for_creation(show_output=True)
# Download the package.
package.save("./imagefiles")
# Get the Azure container registry that the model/Dockerfile uses.
acr=package.get_container_registry()
print("Address:", acr.address)
print("Username:", acr.username)
print("Password:", acr.password)
```

Bu kod, görüntüyü dizine oluşturmak için gereken dosyaları indirir `imagefiles` . Kayıtlı dosyalara eklenen Dockerfile, bir Azure Container Registry 'de depolanan bir temel görüntüye başvurur. Yerel Docker yüklemenizde görüntüyü oluştururken, kayıt defterinde kimlik doğrulamak için adresi, Kullanıcı adını ve parolayı kullanmanız gerekir. Yerel bir Docker yüklemesi kullanarak görüntüyü derlemek için aşağıdaki adımları kullanın:

1. Bir kabuk veya komut satırı oturumundan, Azure Container Registry ile Docker kimlik doğrulaması yapmak için aşağıdaki komutu kullanın. `<address>`, `<username>` Ve `<password>` değerlerini tarafından alınan değerlerle değiştirin `package.get_container_registry()` .

    ```bash
    docker login <address> -u <username> -p <password>
    ```

2. Görüntüyü oluşturmak için aşağıdaki komutu kullanın. `<imagefiles>`Dosyalarını kaydettiğiniz dizinin yoluyla değiştirin `package.save()` .

    ```bash
    docker build --tag myimage <imagefiles>
    ```

    Bu komut, görüntü adını olarak ayarlar `myimage` .

Görüntünün derlendiğini doğrulamak için `docker images` komutunu kullanın. Listede görüntü görmeniz gerekir `myimage` :

```text
REPOSITORY      TAG                 IMAGE ID            CREATED             SIZE
<none>          <none>              2d5ee0bf3b3b        49 seconds ago      1.43 GB
myimage         latest              739f22498d64        3 minutes ago       1.43 GB
```

Bu görüntüye göre yeni bir kapsayıcı başlatmak için aşağıdaki komutu kullanın:

```bash
docker run -p 6789:5001 --name mycontainer myimage:latest
```

Bu komut, adlı görüntünün en son sürümünü başlatır `myimage` . Yerel bağlantı noktası 6789 ' i Web hizmetinin dinlediği kapsayıcıda bağlantı noktasına eşler (5001). Ayrıca `mycontainer` , kapsayıcıyı daha kolay durdurmayı sağlayan kapsayıcıya de atar. Kapsayıcı başlatıldıktan sonra, istek gönderebilirsiniz `http://localhost:6789/score` .

### <a name="example-client-to-test-the-local-container"></a>Yerel kapsayıcıyı test eden örnek istemci

Aşağıdaki kod, kapsayıcında kullanılabilecek bir Python istemcisi örneğidir:

```python
import requests
import json

# URL for the web service.
scoring_uri = 'http://localhost:6789/score'

# Two sets of data to score, so we get two results back.
data = {"data":
        [
            [ 1,2,3,4,5,6,7,8,9,10 ],
            [ 10,9,8,7,6,5,4,3,2,1 ]
        ]
        }
# Convert to JSON string.
input_data = json.dumps(data)

# Set the content type.
headers = {'Content-Type': 'application/json'}

# Make the request and display the response.
resp = requests.post(scoring_uri, input_data, headers=headers)
print(resp.text)
```

Örneğin, diğer programlama dillerinde istemciler için bkz. [Web Hizmetleri olarak dağıtılan modelleri](how-to-consume-web-service.md)kullanma.

### <a name="stop-the-docker-container"></a>Docker kapsayıcısını durdur

Kapsayıcıyı durdurmak için, farklı bir kabuktan veya komut satırından aşağıdaki komutu kullanın:

```bash
docker kill mycontainer
```

## <a name="next-steps"></a>Sonraki adımlar

* [Başarısız bir dağıtımda sorun giderme](how-to-troubleshoot-deployment.md)
* [Azure Kubernetes Service’e dağıtma](how-to-deploy-azure-kubernetes-service.md)
* [Web hizmetlerini kullanmak için istemci uygulamaları oluşturma](how-to-consume-web-service.md)
* [Web hizmetini Güncelleştir](how-to-deploy-update-web-service.md)
* [Özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md)
* [Azure Machine Learning aracılığıyla bir Web hizmetinin güvenliğini sağlamak için TLS kullanma](how-to-secure-web-service.md)
* [Application Insights Azure Machine Learning modellerinizi izleyin](how-to-enable-app-insights.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)
* [Model dağıtımları için olay uyarıları ve Tetikleyicileri oluşturma](how-to-use-event-grid.md)
