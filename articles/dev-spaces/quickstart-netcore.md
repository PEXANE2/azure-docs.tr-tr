---
title: "Kubernetes'te hata ayıklama ve yinele: Visual Studio Code & .NET Core"
services: azure-dev-spaces
ms.date: 07/08/2019
ms.topic: quickstart
description: Bu hızlı başlangıç, Azure Kubernetes Hizmeti'ndeki bir .NET Core uygulamasını hata ayıklamak ve hızla doğrulamak için Azure Dev Spaces ve Visual Studio Code'u nasıl kullanacağınızı gösterir
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s
manager: gwallace
ms.openlocfilehash: b38562879fa67d7ee82e3251ea2fcaa57a2075d6
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80240219"
---
# <a name="quickstart-debug-and-iterate-on-kubernetes-visual-studio-code-and-net-core---azure-dev-spaces"></a>Quickstart: Kubernetes'te hata ayıklama ve yinele: Visual Studio Kodu ve .NET Core - Azure Dev Spaces

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure'da yönetilen bir Kubernetes ile Azure Dev Spaces'ı ayarlayın.
- Visual Studio Code kullanarak kapsayıcılarda yinelemeli bir şekilde kod geliştirin.
- Görsel Stüdyo Kodu'ndan geliştirme alanınızdaki kodu hata ayıklayın.

Azure Dev Alanları, şu ları kullanarak hata ayıklamanızı ve yinelemenizi de sağlar:
- [Java ve Görsel Stüdyo Kodu](quickstart-java.md)
- [Düğüm.js ve Görsel Stüdyo Kodu](quickstart-nodejs.md)
- [.NET Çekirdek ve Görsel Stüdyo](quickstart-netcore-visualstudio.md)

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Hesabınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
- [Visual Studio Kodu yüklendi.](https://code.visualstudio.com/download)
- Visual Studio Code için [Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) ve [C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) uzantıları yüklendi.
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

Uygulamayı GitHub'dan klonla.

```cmd
git clone https://github.com/Azure/dev-spaces
```

## <a name="prepare-the-sample-application-in-visual-studio-code"></a>Örnek uygulamayı Visual Studio Code'da hazırlama

Visual Studio Code'u açın, *Dosya'yı* tıklatın sonra *Aç...* *dev-spaces/samples/dotnetcore/getting-started/webfrontend* *Open*

Şimdi Visual Studio Code açık *webfrontend* proje var. Uygulamayı geliştirme alanınızda çalıştırmak için, Komut Paleti'ndeki Azure Geliştirme Alanları uzantısını kullanarak Docker ve Miğfer grafik varlıklarını oluşturun.

Visual Studio Code'da Komut Paleti'ni açmak için *Görünüm* ve *Komut Paleti'ni*tıklatın. Yazmaya `Azure Dev Spaces` başlayın ve `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`tıklayın.

![Azure Geliştirme Alanları için yapılandırma dosyaları hazırlama](./media/common/command-palette.png)

Visual Studio Code ayrıca ortak bitiş noktanızı yapılandırmanızı `Yes` istediğinde, ortak bir bitiş noktasını etkinleştirmeyi seçin.

![Ortak bitiş noktasını seçin](media/common/select-public-endpoint.png)

Bu komut, dockerfile ve Miğfer grafiği oluşturarak projenizi Azure Dev Spaces'te çalıştırmaya hazırlar. Ayrıca, projenizin kökünde hata ayıklama yapılandırması içeren bir *.vscode* dizini oluşturur.

> [!TIP]
> Projenizin [Dockerfile ve Helm grafiği,](how-dev-spaces-works-prep.md#prepare-your-code) kodunuzu oluşturmak ve çalıştırmak için Azure Dev Spaces tarafından kullanılır, ancak projenin oluşturulma ve çalışma şeklini değiştirmek isterseniz bu dosyaları değiştirebilirsiniz.

## <a name="build-and-run-code-in-kubernetes-from-visual-studio-code"></a>Visual Studio Code'tan Kubernetes'te kod oluşturma ve çalıştırma

Soldaki *Hata Ayıklama* simgesine tıklayın ve üstteki *.NET Core Launch 'a (AZDS)* tıklayın.

![](media/get-started-netcore/debug-configuration.png)

Bu komut, hata ayıklama modunda Azure Dev Spaces'te hizmetinizi oluşturur ve çalıştırır. Alttaki *Terminal* penceresi, Azure Geliştirme Spaces'te çalışan hizmetinizin yapı çıktısını ve URL'lerini gösterir. *Hata Ayıklama Konsolu* günlük çıktısını gösterir.

> [!Note]
> *Komut*Paleti'nde azure Dev Spaces komutları görmüyorsanız, [Azure Dev Spaces için Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds)yüklediğinizden emin olun. Ayrıca Visual Studio Code'da *dev-spaces/samples/dotnetcore/getting-started/webfrontend* dizinini açtığınızı da doğrulayın.

Genel URL'yi açarak hizmetin çalıştığını görebilirsiniz.

> [!Note]
> Başlangıçta, genel URL bir *Hatalı Ağ Geçidi* hatası gösterebilir. Web sayfasını yenilemeden önce birkaç saniye bekleyin ve hizmetinizin çalıştığını görmeniz gerekir.

Hata Ayıklama'yı durdurmak için *Hata* *Ayıklama'yı durdur'u* tıklatın.

## <a name="update-code"></a>Kodu güncelleştirme

Hizmetinizin güncelleştirilmiş bir sürümünü dağıtmak için, projenizdeki herhangi bir dosyayı güncelleyebilir ve *.NET Core Launch (AZDS)* yeniden çalıştırabilirsiniz. Örnek:

1. Uygulamanız hala çalışıyorsa, *Hata Ayıklama'yı* tıklatın ve durdurmak için Hata *Ayıklama'yı* Durdurun'u tıklatın.
1. [Güncelleme satırı 22 için: `Controllers/HomeController.cs` ](https://github.com/Azure/dev-spaces/blob/master/samples/dotnetcore/getting-started/webfrontend/Controllers/HomeController.cs#L22)
    
    ```csharp
    ViewData["Message"] = "Your application description page in Azure.";
    ```

1. Yaptığınız değişiklikleri kaydedin.
1. Rerun *.NET Çekirdek Başlatma (AZDS)*.
1. Çalışan hizmetinize gidin ve *Hakkında'yı*tıklatın.
1. Değişikliklerinizi gözlemleyin.
1. Hata *Debug* Ayıklama'yı tıklatın ve uygulamanızı durdurmak için *Hata Ayıklama'yı durdurun.*

## <a name="setting-and-using-breakpoints-for-debugging"></a>Hata ayıklama için kesme noktalarını ayarlama ve kullanma

*.NET Core Launch (AZDS)* kullanarak hata ayıklama modunda hizmetinizi başlatın.

*Görünüm* sonra *Explorer'ı* tıklatarak *Explorer*Explorer görünümüne geri gidin. Açılıp `Controllers/HomeController.cs` imlecinizi oraya koymak için 22. Bir kesme noktası *f9* vurmak ayarlamak için veya *Hata Ayıklama* sonra *Breakpoint toggle*tıklatın.

Hizmetinizi bir tarayıcıda açın ve hiçbir iletinin görüntülenmeyişolduğunu fark edin. Visual Studio Code'a dönün ve 20. Ayarladığınız kesme noktası, 20. Hizmeti devam ettirmek için *F5* tuşuna basveya *Hata Ayıklama'yı* tıklatın ve *devam edin.* Tarayıcınıza dönün ve iletinin şimdi görüntülendiğini fark edin.

Hizmetinizi Kubernetes'te bir hata ayıklama ekli yken, arama yığını, yerel değişkenler ve özel durum bilgileri gibi hata ayıklama bilgilerine tam erişime sahipsiniz.

İmlecinizi 22. `Controllers/HomeController.cs` *F9*

## <a name="update-code-from-visual-studio-code"></a>Visual Studio Kodu'ndan kodu güncelleştir

Hizmet hata ayıklama modunda çalışırken, satır 22'yi ' de güncelleştirin. `Controllers/HomeController.cs` Örnek:

```csharp
ViewData["Message"] = "Your application description page in Azure while debugging!";
```

Dosyayı kaydedin. *Hata Ayıklama'yı* tıklatın sonra Hata *Ayıklama'yı yeniden başlat'ı* veya Hata *Ayıklama araç çubuğunda*Hata *Ayıklama'yı yeniden başlat düğmesini* tıklatın.

![](media/common/debug-action-refresh.png)

Hizmetinizi bir tarayıcıda açın ve güncelleştirilmiş iletinizin görüntülendiğini fark edin.

Azure Dev Spaces, kod edinimi her yapıldığında yeni bir kapsayıcı görüntüsünü yeniden oluşturma ve yeniden dağıtmak yerine, daha hızlı bir edit/hata ayıklama döngüsü sağlamak için kodu varolan kapsayıcının içinde aşamalı olarak yeniden derler.

## <a name="clean-up-your-azure-resources"></a>Azure kaynaklarınızı temizleme

```azurecli
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Geliştirme Alanları'nın birden çok kapsayıcıda daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda farklı sürümlerle veya kod dallarıyla çalışarak ortak geliştirmeyi nasıl basitleştirebileceğinizi öğrenin. 

> [!div class="nextstepaction"]
> [Birden çok kapsayıcı ve takım geliştirme ile çalışma](multi-service-netcore.md)


[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
