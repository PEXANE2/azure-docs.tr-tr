---
title: Kubernetes 'te uygulama geliştirme
services: azure-dev-spaces
ms.date: 02/20/2020
ms.topic: quickstart
description: Bu hızlı başlangıçta, Azure Kubernetes hizmetinde bir uygulama geliştirmek için Azure Dev Spaces ve komut satırının nasıl kullanılacağı gösterilir.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s
manager: gwallace
ms.custom: devx-track-javascript
ms.openlocfilehash: 82e7e73a91b07770881353465af063497deae085
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87414811"
---
# <a name="quickstart-develop-an-application-on-kubernetes---azure-dev-spaces"></a>Hızlı başlangıç: Kubernetes 'te uygulama geliştirme-Azure Dev Spaces
Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure'da yönetilen bir Kubernetes ile Azure Dev Spaces'ı ayarlayın.
- Komut satırını kullanarak kapsayıcılarda kod geliştirin ve çalıştırın.

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

`use-dev-spaces`AKS kümenizde dev alanlarını etkinleştirmek ve istemleri izlemek için komutunu kullanın. Aşağıdaki komut *Myresourcegroup* grubundaki *myaks* kümesinde dev alanlarını etkinleştiriyor ve *varsayılan* bir dev alanı oluşturuyor.

> [!NOTE]
> `use-dev-spaces`Bu komut, zaten yüklenmemişse Azure dev Spaces CLI 'yi de yükleyecek. Azure Dev Spaces CLı 'yi Azure Cloud Shell yükleyemezsiniz.

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

## <a name="get-sample-application-code"></a>Örnek uygulama kodu al

Bu makalede, Azure Dev Spaces kullanmayı göstermek için [Azure dev Spaces örnek uygulamayı](https://github.com/Azure/dev-spaces) kullanırsınız.

Uygulamayı GitHub 'dan kopyalayın ve *dev-Spaces/Samples/NodeJS/Başlarken/webön uç* dizinine gidin:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/nodejs/getting-started/webfrontend
```

## <a name="prepare-the-application"></a>Uygulamayı hazırlama

Uygulamanızı Azure Dev Spaces çalıştırmak için bir Dockerfile ve HELI grafiğinin olması gerekir. [Java][java-quickstart], [.net Core][netcore-quickstart]ve [Node.js][nodejs-quickstart]gibi bazı diller için Azure dev Spaces istemci araçları, ihtiyacınız olan tüm varlıkları oluşturabilir. Go, PHP ve Python gibi diğer birçok dil için, istemci araçları, geçerli bir Dockerfile sağlayabilmeniz koşuluyla Held grafiğini oluşturabilir.

Şu komutu kullanarak Kubernetes 'te uygulamayı çalıştırmak için Docker ve helk grafik varlıkları oluşturun `azds prep` :

```cmd
azds prep --enable-ingress
```

`prep`Docker ve Held grafik varlıklarını doğru şekilde oluşturmak için *geliştirme-Spaces/Samples/NodeJS/alma-başlatma/webön uç* dizininden komutunu çalıştırmanız gerekir.

> [!TIP]
> `prep`Komut, projeniz için [bir Dockerfile ve hela grafiği](how-dev-spaces-works-prep.md#prepare-your-code) oluşturmaya çalışır. Azure Dev Spaces, kodunuzu derlemek ve çalıştırmak için bu dosyaları kullanır, ancak projenin oluşturulup çalıştırıldığını değiştirmek istiyorsanız bu dosyaları değiştirebilirsiniz.

## <a name="build-and-run-code-in-kubernetes"></a>Kubernetes'de kodu oluşturma ve çalıştırma

Şu komutu kullanarak kodlarınızı AKS 'de derleyin ve çalıştırın `azds up` :

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
Service 'webfrontend' port 'http' is available at `http://webfrontend.1234567890abcdef1234.eus.azds.io/`
Service 'webfrontend' port 80 (http) is available at http://localhost:54256
...
```

Hizmetin çıkış bölümünde gösterildiği genel URL 'YI açarak çalışır durumda olduğunu görebilirsiniz `azds up` . Bu örnekte, genel URL olur *`http://webfrontend.1234567890abcdef1234.eus.azds.io/`* .

> [!NOTE]
> Çalışırken hizmetinize gittiğinizde `azds up` , http isteği izlemeleri komutun çıktısında de görüntülenir `azds up` . Bu izlemeler, hizmetinizde sorun gidermenize ve hata ayıklamanıza yardımcı olabilir. Çalıştırırken kullanarak bu izlemeleri devre dışı bırakabilirsiniz `--disable-http-traces` `azds up` .

`azds up` *Ctrl + c*kullanarak komutu durdurursanız, hizmet aks 'de çalışmaya devam eder ve genel URL kullanılabilir olmaya devam eder.

## <a name="update-code"></a>Kodu güncelleştirme

Hizmetinizin güncelleştirilmiş bir sürümünü dağıtmak için, projenizdeki herhangi bir dosyayı güncelleştirebilir ve komutunu yeniden çalıştırabilirsiniz `azds up` . Örneğin:

1. `azds up`Hala çalışıyorsa, *Ctrl + c*tuşlarına basın.
1. [Satır 13 ' `server.js` ](https://github.com/Azure/dev-spaces/blob/master/samples/nodejs/getting-started/webfrontend/server.js#L13) i şu şekilde güncelleştir:
    
    ```javascript
        res.send('Hello from webfrontend in Azure');
    ```

1. Yaptığınız değişiklikleri kaydedin.
1. Komutu yeniden çalıştırın `azds up` :

    ```cmd
    $ azds up
    Using dev space 'default' with target 'MyAKS'
    Synchronizing files...1s
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