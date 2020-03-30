---
title: Kubernetes üzerinde bir uygulama geliştirme
services: azure-dev-spaces
ms.date: 02/20/2020
ms.topic: quickstart
description: Bu hızlı başlangıç, Azure Kubernetes Hizmeti'nde bir uygulama geliştirmek için Azure Dev Spaces'i ve komut satırını nasıl kullanacağınızı gösterir
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s
manager: gwallace
ms.openlocfilehash: 8ee5cba06d9a526640d9057ee88a681d46392f4f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80239703"
---
# <a name="quickstart-develop-an-application-on-kubernetes---azure-dev-spaces"></a>Quickstart: Kubernetes'te bir uygulama geliştirin - Azure Dev Spaces
Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure'da yönetilen bir Kubernetes ile Azure Dev Spaces'ı ayarlayın.
- Komut satırını kullanarak kapsayıcılarda kod geliştirin ve çalıştırın.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
- [Yüklü Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes Hizmet kümesi oluşturma

Desteklenen bir [bölgede][supported-regions]bir AKS kümesi oluşturmanız gerekir. Aşağıdaki komutlar *MyResourceGroup* adında bir kaynak grubu ve *MyAKS*adlı bir AKS kümesi oluşturur.

```azurecli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>AKS kümenizde Azure Dev Alanları'nı etkinleştirme

AKS `use-dev-spaces` kümenizde Dev Spaces'i etkinleştirmek ve istemleri izlemek için komutu kullanın. Aşağıdaki komut, *MyResourceGroup* grubunda *MyAKS* kümesinde Dev Spaces'i etkinleştirive *varsayılan* bir dev alanı oluşturur.

> [!NOTE]
> Komut, `use-dev-spaces` zaten yüklü değilse Azure Dev Spaces CLI'yi de yükler. Azure Geliştirme Alanları CLI'sini Azure Bulut Kabuğu'na yükleyemezsiniz.

```azurecli
az aks use-dev-spaces -g MyResourceGroup -n MyAKS
```

```output
'An Azure Dev Spaces Controller' will be created that targets resource 'MyAKS' in resource group 'MyResourceGroup'. Continue? (y/N): y

Creating and selecting Azure Dev Spaces Controller 'MyAKS' in resource group 'MyResourceGroup' that targets resource 'MyAKS' in resource group 'MyResourceGroup'...2m 24s

Select a dev space or Kubernetes namespace to use as a dev space.
 [1] default
Type a number or a new name: 1

Kubernetes namespace 'default' will be configured as a dev space. This will enable Azure Dev Spaces instrumentation for new workloads in the namespace. Continue? (Y/n): Y

Configuring and selecting dev space 'default'...3s

Managed Kubernetes cluster 'MyAKS' in resource group 'MyResourceGroup' is ready for development in dev space 'default'. Type `azds prep` to prepare a source directory for use with Azure Dev Spaces and `azds up` to run.
```

## <a name="get-sample-application-code"></a>Örnek uygulama kodu alın

Bu makalede, Azure [Dev Spaces](https://github.com/Azure/dev-spaces) kullanarak göstermek için Azure Dev Spaces örnek uygulamasını kullanırsınız.

Uygulamayı GitHub'dan klonla ve *dev-spaces/samples/nodejs/getting-started/webfrontend* dizinine gidin:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/nodejs/getting-started/webfrontend
```

## <a name="prepare-the-application"></a>Uygulamayı hazırlama

Uygulamanızı Azure Geliştirme Spaces'te çalıştırmak için bir Dockerfile ve Helm grafiğine ihtiyacınız vardır. [Java][java-quickstart], [.NET core][netcore-quickstart]ve [Node.js][nodejs-quickstart]gibi bazı dillerde Azure Dev Spaces istemcisi aracı, ihtiyacınız olan tüm varlıkları oluşturabilir. Go, PHP ve Python gibi diğer birçok dilde, istemci aracı, geçerli bir Dockerfile sağlayabildiğiniz sürece Miğfer grafiğini oluşturabilir.

Uygulamayı Kubernetes'te `azds prep` komutu kullanarak çalıştırmak için Docker ve Helm grafik varlıklarını oluşturun:

```cmd
azds prep --enable-ingress
```

Docker ve `prep` Helm grafik varlıklarını doğru bir şekilde oluşturmak için komutu *dev-spaces/samples/nodejs/getting-started/webfrontend* dizininden çalıştırmanız gerekir.

> [!TIP]
> Komut, `prep` projeniz için [bir Dockerfile ve Helm grafiği](how-dev-spaces-works-prep.md#prepare-your-code) oluşturmaya çalışır. Azure Dev Spaces, kodunuzu oluşturmak ve çalıştırmak için bu dosyaları kullanır, ancak projenin oluşturulma ve çalışma şeklini değiştirmek istiyorsanız bu dosyaları değiştirebilirsiniz.

## <a name="build-and-run-code-in-kubernetes"></a>Kubernetes'de kodu oluşturma ve çalıştırma

Komutu kullanarak KODUNUZU AKS'de oluşturun ve çalıştırın: `azds up`

```cmd
$ azds up
Using dev space 'default' with target 'MyAKS'
Synchronizing files...2s
Installing Helm chart...2s
Waiting for container image build...2m 25s
Building container image...
Step 1/8 : FROM node
Step 2/8 : ENV PORT 80
Step 3/8 : EXPOSE 80
Step 4/8 : WORKDIR /app
Step 5/8 : COPY package.json .
Step 6/8 : RUN npm install
Step 7/8 : COPY . .
Step 8/8 : CMD ["npm", "start"]
Built container image in 6m 17s
Waiting for container...13s
Service 'webfrontend' port 'http' is available at http://webfrontend.1234567890abcdef1234.eus.azds.io/
Service 'webfrontend' port 80 (http) is available at http://localhost:54256
...
```

Komuttan çıktıda görüntülenen genel URL'yi açarak hizmetin `azds up` çalıştığını görebilirsiniz. Bu örnekte, genel *http://webfrontend.1234567890abcdef1234.eus.azds.io/* URL.

> [!NOTE]
> Çalışırken `azds up`hizmetinize gidince, HTTP istek izleri de `azds up` komutun çıktısında görüntülenir. Bu izlemeler, hizmetinizi sorun gidermenize ve hata ayıklamanıza yardımcı olabilir. Çalışırken bu izleri kullanarak `--disable-http-traces` devre dışı `azds up`kullanabilirsiniz.

`azds up` *Ctrl+c*kullanarak komutu durdurursanız, hizmet AKS'de çalışmaya devam eder ve genel URL kullanılabilir kalır.

## <a name="update-code"></a>Kodu güncelleştirme

Hizmetinizin güncelleştirilmiş bir sürümünü dağıtmak için, projenizdeki herhangi `azds up` bir dosyayı güncelleyebilir ve komutu yeniden çalıştırabilirsiniz. Örnek:

1. Çalışıyorsa `azds up` *Ctrl+c*tuşuna basın.
1. [Güncelleme satırı 13 için: `server.js` ](https://github.com/Azure/dev-spaces/blob/master/samples/nodejs/getting-started/webfrontend/server.js#L13)
    
    ```javascript
        res.send('Hello from webfrontend in Azure');
    ```

1. Yaptığınız değişiklikleri kaydedin.
1. Komutu `azds up` yeniden çalıştırın:

    ```cmd
    $ azds up
    Using dev space 'default' with target 'MyAKS'
    Synchronizing files...1s
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