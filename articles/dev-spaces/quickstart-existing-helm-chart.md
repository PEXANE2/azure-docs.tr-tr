---
title: Kubernetes 'te mevcut bir helk grafiği ile uygulama geliştirme
services: azure-dev-spaces
ms.date: 04/21/2020
ms.topic: quickstart
description: Bu hızlı başlangıçta, Azure Kubernetes hizmetinde mevcut bir Held grafiğine sahip bir uygulama geliştirmek için Azure Dev Spaces ve komut satırının nasıl kullanılacağı gösterilir.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s
manager: gwallace
ms.custom: devx-track-javascript
ms.openlocfilehash: 7d1d782203401607d3909d79d1f0f3a499712c32
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87414318"
---
# <a name="quickstart-develop-an-application-with-an-existing-helm-chart-on-kubernetes---azure-dev-spaces"></a>Hızlı başlangıç: Kubernetes 'de mevcut bir helk grafiği ile uygulama geliştirme-Azure Dev Spaces
Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure'da yönetilen bir Kubernetes ile Azure Dev Spaces'ı ayarlayın.
- Komut satırında Azure Dev Spaces kullanarak AKS 'de mevcut bir Held grafiğiyle bir uygulama çalıştırın.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
- [Yüklü Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes Service kümesi oluşturma

[Desteklenen bir bölgede][supported-regions]aks kümesi oluşturmanız gerekir. Aşağıdaki komutlar *Myresourcegroup* adlı bir kaynak grubu ve *myaks*adlı bir aks kümesi oluşturur.

```azurecli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>AKS kümenizde Azure Dev Spaces etkinleştirme

`use-dev-spaces`AKS kümenizde dev alanlarını etkinleştirmek ve istemleri izlemek için komutunu kullanın. Aşağıdaki komut *Myresourcegroup* grubundaki *myaks* kümesinde dev alanları sunar ve *dev*adlı bir dev alanı oluşturur.

> [!NOTE]
> `use-dev-spaces`Bu komut, zaten yüklenmemişse Azure dev Spaces CLI 'yi de yükleyecek. Azure Dev Spaces CLı 'yi Azure Cloud Shell yükleyemezsiniz.

```azurecli
az aks use-dev-spaces -g MyResourceGroup -n MyAKS --space dev --yes
```

## <a name="get-sample-application-code"></a>Örnek uygulama kodu al

Bu makalede, Azure Dev Spaces kullanmayı göstermek için [Azure dev Spaces örnek uygulamayı](https://github.com/Azure/dev-spaces) kullanırsınız.

Uygulamayı GitHub 'dan kopyalayın ve *dev-Spaces/Samples/Python/alma-başlatma/webön uç* dizinine gidin:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/python/getting-started/webfrontend
```

## <a name="prepare-the-application"></a>Uygulamayı hazırlama

Uygulamanızı Azure Dev Spaces çalıştırmak için bir Dockerfile ve HELI grafiğinin olması gerekir. [Java][java-quickstart], [.net Core][netcore-quickstart]ve [Node.js][nodejs-quickstart]gibi bazı diller için Azure dev Spaces istemci araçları, ihtiyacınız olan tüm varlıkları oluşturabilir. Go, PHP ve Python gibi diğer birçok dil için, istemci araçları, geçerli bir Dockerfile sağlayabilmeniz koşuluyla Held grafiğini oluşturabilir. Bu durumda, örnek uygulamanın var olan bir Dockerfile ve HELI grafiği vardır

Şu komutu kullanarak, mevcut Held grafiği ve Dockerfile ile Azure Dev Spaces uygulamayı çalıştırmaya yönelik yapılandırmayı oluşturun `azds prep` :

```cmd
azds prep --enable-ingress --chart webfrontend/
```

`prep`Komutunu *dev-Spaces/Samples/Python/Başlarken/webön uç* dizininden çalıştırmanız ve kullanarak Held grafiğinin konumunu belirtmeniz gerekir `--chart` .

> [!NOTE]
> Uyarı: *Uyarı: Dockerfile desteklenmeyen bir dil nedeniyle oluşturulamadı.* çalışırken `azds prep` . `azds prep`Komut, projeniz için [bir Dockerfile ve HELI grafiği](how-dev-spaces-works-prep.md#prepare-your-code) oluşturmaya çalışır, ancak var olan dockerfiles veya HELI grafiklerinin üzerine yazmaz.

## <a name="build-and-run-code-in-kubernetes"></a>Kubernetes'de kodu oluşturma ve çalıştırma

Şu komutu kullanarak kodlarınızı AKS 'de derleyin ve çalıştırın `azds up` :

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

Hizmetin çıkış bölümünde gösterildiği genel URL 'YI açarak çalışır durumda olduğunu görebilirsiniz `azds up` . Bu örnekte, genel URL olur `http://dev.service.1234567890abcdef1234.eus.azds.io/` .

> [!NOTE]
> Çalışırken hizmetinize gittiğinizde `azds up` , http isteği izlemeleri komutun çıktısında de görüntülenir `azds up` . Bu izlemeler, hizmetinizde sorun gidermenize ve hata ayıklamanıza yardımcı olabilir. Çalıştırırken kullanarak bu izlemeleri devre dışı bırakabilirsiniz `--disable-http-traces` `azds up` .

`azds up` *Ctrl + c*kullanarak komutu durdurursanız, hizmet aks 'de çalışmaya devam eder ve genel URL kullanılabilir olmaya devam eder.

## <a name="update-code"></a>Kodu güncelleştirme

Hizmetinizin güncelleştirilmiş bir sürümünü dağıtmak için, projenizdeki herhangi bir dosyayı güncelleştirebilir ve komutunu yeniden çalıştırabilirsiniz `azds up` . Örneğin:

1. `azds up`Hala çalışıyorsa, *Ctrl + c*tuşlarına basın.
1. [Satır 13 ' `webfrontend.py` ](https://github.com/Azure/dev-spaces/blob/master/samples/python/getting-started/webfrontend/webfrontend.py#L13) i şu şekilde güncelleştir:
    
    ```javascript
        res.send('Hello from webfrontend in Azure');
    ```

1. Yaptığınız değişiklikleri kaydedin.
1. Komutu yeniden çalıştırın `azds up` :

    ```cmd
    $ azds up
    Using dev space 'dev' with target 'MyAKS'
    Synchronizing files...11s
    Installing Helm chart...3s
    Waiting for container image build...
    ...    
    ```

1. Çalışan hizmetinize gidin ve değişikliklerinizi gözlemleyin.
1. Komutu durdurmak için *Ctrl + c* tuşlarına basın `azds up` .

## <a name="clean-up-your-azure-resources"></a>Azure kaynaklarınızı Temizleme

```azurecli
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Dev Spaces birden çok kapsayıcı genelinde daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda kodunuzun farklı sürümleriyle veya dallarıyla çalışarak işbirliğine dayalı geliştirmeyi nasıl kolaylaştırabileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Azure Dev Spaces 'de takım geliştirme][team-quickstart]

[java-quickstart]: quickstart-java.md
[nodejs-quickstart]: quickstart-nodejs.md
[netcore-quickstart]: quickstart-netcore.md
[team-quickstart]: quickstart-team-development.md
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service