---
title: Kubernetes üzerinde mevcut bir Miğfer grafiği içeren bir uygulama geliştirin
services: azure-dev-spaces
ms.date: 04/21/2020
ms.topic: quickstart
description: Bu hızlı başlangıç, Azure Kubernetes Hizmeti'nde varolan bir Miğfer grafiğine sahip bir uygulama geliştirmek için Azure Dev Spaces'i ve komut satırını nasıl kullanacağınızı gösterir
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s
manager: gwallace
ms.openlocfilehash: e767b1ade2a80882ee33ff1fdd718c691dcefcf3
ms.sourcegitcommit: 75089113827229663afed75b8364ab5212d67323
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82033567"
---
# <a name="quickstart-develop-an-application-with-an-existing-helm-chart-on-kubernetes---azure-dev-spaces"></a>Quickstart: Kubernetes'te varolan bir Miğfer grafiğine sahip bir uygulama geliştirme - Azure Dev Spaces
Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure'da yönetilen bir Kubernetes ile Azure Dev Spaces'ı ayarlayın.
- Komut satırında Azure Dev Spaces kullanarak AKS'de varolan bir Miğfer grafiğine sahip bir uygulama çalıştırın.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
- [Yüklü Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes Hizmet kümesi oluşturma

Desteklenen bir [bölgede][supported-regions]bir AKS kümesi oluşturmanız gerekir. Aşağıdaki komutlar *MyResourceGroup* adında bir kaynak grubu ve *MyAKS*adlı bir AKS kümesi oluşturur.

```azurecli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>AKS kümenizde Azure Dev Alanları'nı etkinleştirme

AKS `use-dev-spaces` kümenizde Dev Spaces'i etkinleştirmek ve istemleri izlemek için komutu kullanın. Aşağıdaki *komut, MyResourceGroup* grubunda *MyAKS* kümesindeki Dev Spaces'i etkinleştiriyor ve dev adı verilen bir *dev*alanı oluşturuyor.

> [!NOTE]
> Komut, `use-dev-spaces` zaten yüklü değilse Azure Dev Spaces CLI'yi de yükler. Azure Geliştirme Alanları CLI'sini Azure Bulut Kabuğu'na yükleyemezsiniz.

```azurecli
az aks use-dev-spaces -g MyResourceGroup -n MyAKS --space dev --yes
```

## <a name="get-sample-application-code"></a>Örnek uygulama kodu alın

Bu makalede, Azure [Dev Spaces](https://github.com/Azure/dev-spaces) kullanarak göstermek için Azure Dev Spaces örnek uygulamasını kullanırsınız.

Uygulamayı GitHub'dan klonla ve *dev-spaces/samples/python/getting-started/webfrontend* dizinine gidin:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/python/getting-started/webfrontend
```

## <a name="prepare-the-application"></a>Uygulamayı hazırlama

Uygulamanızı Azure Geliştirme Spaces'te çalıştırmak için bir Dockerfile ve Helm grafiğine ihtiyacınız vardır. [Java][java-quickstart], [.NET core][netcore-quickstart]ve [Node.js][nodejs-quickstart]gibi bazı dillerde Azure Dev Spaces istemcisi aracı, ihtiyacınız olan tüm varlıkları oluşturabilir. Go, PHP ve Python gibi diğer birçok dilde, istemci aracı, geçerli bir Dockerfile sağlayabildiğiniz sürece Miğfer grafiğini oluşturabilir. Bu durumda, örnek uygulama mevcut bir Dockerfile ve Helm grafik vardır

Uygulamayı, varolan Miğfer grafiği ve Dockerfile komutunu kullanarak `azds prep` Azure Dev Spaces ile çalıştırmak için yapılandırmayı oluşturun:

```cmd
azds prep --enable-ingress --chart webfrontend/
```

Komutu `prep` *dev-spaces/samples/python/getting-started/webfrontend* dizininden çalıştırmalı ve Helm grafiğinin konumunu `--chart`kullanarak belirtmelisiniz.

> [!NOTE]
> Uyarıyı görebilirsiniz: *UYARI: Dockerfile desteklenmeyen dil nedeniyle oluşturulamadı.* çalıştırırken `azds prep`. Komut, `azds prep` projeniz için [bir Dockerfile ve Helm grafiği](how-dev-spaces-works-prep.md#prepare-your-code) oluşturmaya çalışır, ancak varolan Dockerfiles veya Helm grafiklerini üzerine yazmaz.

## <a name="build-and-run-code-in-kubernetes"></a>Kubernetes'de kodu oluşturma ve çalıştırma

Komutu kullanarak KODUNUZU AKS'de oluşturun ve çalıştırın: `azds up`

```cmd
$ azds up
Using dev space 'dev' with target 'MyAKS'
Synchronizing files...14s
Installing Helm chart...2s
Waiting for container image build...3s
Building container image...
Step 1/7 : FROM python:3
Step 2/7 : WORKDIR /python/webfrontend
Step 3/7 : RUN pip install flask
Step 4/7 : COPY webfrontend.py webfrontend.py
Step 5/7 : COPY public/ public/
Step 6/7 : EXPOSE 80
Step 7/7 : CMD ["python", "./webfrontend.py"]
Built container image in 45s
Waiting for container...25s
Service 'azds-543eae-dev-webfrontend' port 'http' is available at http://dev.service.1234567890abcdef1234.eus.azds.io/
Service 'azds-543eae-dev-webfrontend' port 80 (http) is available via port forwarding at http://localhost:52382
Press Ctrl+C to detach
...
```

Komuttan çıktıda görüntülenen genel URL'yi açarak hizmetin `azds up` çalıştığını görebilirsiniz. Bu örnekte, genel *http://dev.service.1234567890abcdef1234.eus.azds.io/* URL.

> [!NOTE]
> Çalışırken `azds up`hizmetinize gidince, HTTP istek izleri de `azds up` komutun çıktısında görüntülenir. Bu izlemeler, hizmetinizi sorun gidermenize ve hata ayıklamanıza yardımcı olabilir. Çalışırken bu izleri kullanarak `--disable-http-traces` devre dışı `azds up`kullanabilirsiniz.

`azds up` *Ctrl+c*kullanarak komutu durdurursanız, hizmet AKS'de çalışmaya devam eder ve genel URL kullanılabilir kalır.

## <a name="update-code"></a>Kodu güncelleştirme

Hizmetinizin güncelleştirilmiş bir sürümünü dağıtmak için, projenizdeki herhangi `azds up` bir dosyayı güncelleyebilir ve komutu yeniden çalıştırabilirsiniz. Örneğin:

1. Çalışıyorsa `azds up` *Ctrl+c*tuşuna basın.
1. [Güncelleme satırı 13 için: `webfrontend.py` ](https://github.com/Azure/dev-spaces/blob/master/samples/python/getting-started/webfrontend/webfrontend.py#L13)
    
    ```javascript
        res.send('Hello from webfrontend in Azure');
    ```

1. Yaptığınız değişiklikleri kaydedin.
1. Komutu `azds up` yeniden çalıştırın:

    ```cmd
    $ azds up
    Using dev space 'dev' with target 'MyAKS'
    Synchronizing files...11s
    Installing Helm chart...3s
    Waiting for container image build...
    ...    
    ```

1. Çalışan servisinize gidin ve değişikliklerinizi gözlemleyin.
1. Komutu durdurmak için *Ctrl+c* tuşuna `azds up` basın.

## <a name="clean-up-your-azure-resources"></a>Azure kaynaklarınızı temizleme

```azurecli
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Geliştirme Alanları'nın birden çok kapsayıcıda daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda farklı sürümlerle veya kod dallarıyla çalışarak ortak geliştirmeyi nasıl basitleştirebileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Azure Geliştirme Alanlarında ekip geliştirme][team-quickstart]

[java-quickstart]: quickstart-java.md
[nodejs-quickstart]: quickstart-nodejs.md
[netcore-quickstart]: quickstart-netcore.md
[team-quickstart]: quickstart-team-development.md
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service