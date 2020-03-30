---
title: 'Kubernetes üzerinde hata ayıklama ve yinele: Visual Studio Kodu & Java'
services: azure-dev-spaces
ms.date: 07/08/2019
ms.topic: quickstart
description: Bu hızlı başlangıç, Azure Kubernetes Hizmeti'nde bir Java uygulamasını hata ayıklamak ve hızla yeniden doğrulamak için Azure Geliştirme Alanları ve Görsel Stüdyo Kodunu nasıl kullanacağınızı gösterir
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Java, Miğfer, servis örgü, servis örgü yönlendirme, kubectl, k8s
manager: gwallace
ms.openlocfilehash: ac7a1b37b565f3589b7c049a3c1ed2a84972ded0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80239742"
---
# <a name="quickstart-debug-and-iterate-on-kubernetes-with-visual-studio-code-and-java---azure-dev-spaces"></a>Quickstart: Visual Studio Code ve Java ile Kubernetes'te hata ayıklama ve yinele - Azure Dev Spaces

Bu hızlı başlangıçta, Azure Dev Spaces'i yönetilen bir Kubernetes kümesiyle ayarlar sınız ve visual studio code'da bir Java uygulamasını kullanarak kapsayıcılarda yinelemeli olarak kod geliştirip hata ayıklama nız sağlar. Azure Geliştirme Alanları, en az geliştirme makinesi kurulumuyla Azure Kubernetes Hizmeti'nde (AKS) uygulamanızın tüm bileşenlerini ayıklamanızı ve test etmenizi sağlar. 

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) 
- [Java Geliştirme Kiti (JDK) 1.8.0+](https://aka.ms/azure-jdks).
- [Maven 3.5.0+](https://maven.apache.org/download.cgi).
- [Görsel Stüdyo Kodu](https://code.visualstudio.com/download).
- Visual Studio Code için [Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) uzantıları için Azure Dev Alanları ve Java Hata [Ayıklayıcı.](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debugger-azds)
- [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).
- [Git.](https://www.git-scm.com/downloads)

## <a name="create-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes Hizmet kümesi oluşturma

Desteklenen bir [bölgede][supported-regions]bir AKS kümesi oluşturmanız gerekir. Aşağıdaki komutlar *MyResourceGroup* adında bir kaynak grubu ve *MyAKS*adlı bir AKS kümesi oluşturur.

```azurecli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>AKS kümenizde Azure Dev Alanları'nı etkinleştirme

AKS `use-dev-spaces` kümenizde Dev Spaces'i etkinleştirmek ve istemleri izlemek için komutu kullanın. Aşağıdaki komut, *MyResourceGroup* grubunda *MyAKS* kümesinde Dev Spaces'i etkinleştirir ve *varsayılan* bir dev alanı oluşturur.

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

Visual Studio Code'u açın, **Dosya'yı** seçin sonra **Aç,** *boşluk/örnekler/java/getting-started/webfrontend* dizinine gidin ve **Aç'ı**seçin.

Şimdi Visual Studio Code açık *webfrontend* proje var. Uygulamayı geliştirme alanınızda çalıştırmak için, Komut Paleti'ndeki Azure Geliştirme Alanları uzantısını kullanarak Docker ve Miğfer grafik varlıklarını oluşturun.

Visual Studio Code'da Komut Paleti'ni açmak için **Görünüm** ve **Komut Paleti'ni**seçin. Yazmaya `Azure Dev Spaces` başlayın ve **Azure Dev Alanları'nı seçin: Azure Dev Alanları için yapılandırma dosyaları hazırlayın.**

![Azure Geliştirme Alanları için yapılandırma dosyaları hazırlama](./media/common/command-palette.png)

Visual Studio Code ayrıca temel görüntülerinizi, açıkta kalan bağlantı noktanızı `Azul Zulu OpenJDK for Azure (Free LTS)` ve genel `8080` bitiş noktanızı yapılandırmanızı istediğinde, temel görüntü için, açıkbağlantı noktası için seçin ve `Yes` ortak bir bitiş noktası etkinleştirin.

![Temel resmi seçin](media/get-started-java/select-base-image.png)

![Açıkta kalan bağlantı noktasını seçin](media/get-started-java/select-exposed-port.png)

![Ortak bitiş noktasını seçin](media/get-started-java/select-public-endpoint.png)

Bu komut, dockerfile ve Miğfer grafiği oluşturarak projenizi Azure Dev Spaces'te çalıştırmaya hazırlar. Ayrıca, projenizin kökünde hata ayıklama yapılandırması içeren bir *.vscode* dizini oluşturur.

> [!TIP]
> Projenizin [Dockerfile ve Mişl-işme grafiği,](how-dev-spaces-works-prep.md#prepare-your-code) kodunuzu oluşturmak ve çalıştırmak için Azure Dev Spaces tarafından kullanılır, ancak projenin oluşturulma ve çalışma şeklini değiştirmek isterseniz bu dosyaları değiştirebilirsiniz.

## <a name="build-and-run-code-in-kubernetes-from-visual-studio-code"></a>Visual Studio Code'tan Kubernetes'te kod oluşturma ve çalıştırma

Soldaki **Hata Ayıklama** simgesini seçin ve üstteki **Java Programını Başlat'ı (AZDS)** seçin.

![Java Programı Başlat](media/get-started-java/debug-configuration.png)

Bu komut, Azure Dev Spaces'te hizmetinizi oluşturur ve çalıştırUr. Alttaki **Terminal** penceresi, Azure Dev Alanları çalıştıran hizmetiniz için yapı çıktısını ve URL'lerini gösterir. **Hata Ayıklama Konsolu** günlük çıktısını gösterir.

> [!Note]
> **Komut**Paleti'nde azure Dev Spaces komutları görmüyorsanız, [Azure Dev Spaces için Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds)yüklediğinizden emin olun. Ayrıca Visual Studio Code'da *dev-spaces/samples/java/getting-started/webfrontend* dizinini açtığınızı da doğrulayın.

Genel URL'yi açarak hizmetin çalıştığını görebilirsiniz.

Hata **ayıklamayı** durdurmak için Hata **Ayıklama'yı seçin.**

## <a name="update-code"></a>Kodu güncelleştirme

Hizmetinizin güncelleştirilmiş bir sürümünü dağıtmak için, projenizdeki herhangi bir dosyayı güncelleyebilir ve **Java Programını Başlat'ı (AZDS)** yeniden çalıştırabilirsiniz. Örnek:

1. Uygulamanız hala çalışıyorsa, Hata Ayıklama'yı seçin ve durdurmak için **Hata Ayıklama'yı** Durdurun'u seçin. **Debug**
1. [Güncelleme satırı 19 için: `src/main/java/com/ms/sample/webfrontend/Application.java` ](https://github.com/Azure/dev-spaces/blob/master/samples/java/getting-started/webfrontend/src/main/java/com/ms/sample/webfrontend/Application.java#L19)
    
    ```java
    return "Hello from webfrontend in Azure!";
    ```

1. Yaptığınız değişiklikleri kaydedin.
1. Yeniden **Başlat Java Programı (AZDS)**.
1. Çalışan servisinize gidin ve değişikliklerinizi gözlemleyin.
1. Hata **Debug** Ayıklama'yı seçin ve uygulamanızı durdurmak için **Hata Ayıklama'yı durdurun'** u seçin.

## <a name="setting-and-using-breakpoints-for-debugging"></a>Hata ayıklama için kesme noktalarını ayarlama ve kullanma

Launch Java **Programı 'nı (AZDS)** kullanarak hizmetinizi başlatın. Bu da hata ayıklama modunda hizmet çalışır.

**Görünüm** sonra **Explorer'ı** seçerek **Explorer**Explorer görünümüne geri gidin. *Src/main/java/com/ms/sample/webfrontend/Application.java* adresini açın ve imlecinizi oraya koymak için satır 19'da bir yere tıklayın. Bir kesme noktası ayarlamak için **F9** tuşuna basın veya **Hata Ayıklama'yı** seçin ve ardından Kesme **Noktası'nı Toggle'** ı seçin.

Hizmetinizi bir tarayıcıda açın ve hiçbir iletinin görüntülenmeyişolduğunu fark edin. Visual Studio Code'a dönün ve 19. Ayarladığınız kesme noktası, 19. Hizmeti devam ettirmek için **F5** tuşuna basın veya **Hata Ayıklama'yı** seçin ve **devam edin.** Tarayıcınıza dönün ve iletinin şimdi görüntülendiğini fark edin.

Hizmetinizi Kubernetes'te bir hata ayıklama ekli yken, arama yığını, yerel değişkenler ve özel durum bilgileri gibi hata ayıklama bilgilerine tam erişime sahipsiniz.

İmlecinizi *src/main/java/com/ms/sample/webfrontend/Application.java* adresindeki **19.**

## <a name="update-code-from-visual-studio-code"></a>Visual Studio Kodu'ndan kodu güncelleştir

Hizmet hata ayıklama modunda çalışırken, *src/main/java/com/ms/sample/webfrontend/Application.java'daki*19. Örnek:
```java
return "Hello from webfrontend in Azure while debugging!";
```

Dosyayı kaydedin. **Hata Ayıklama'yı** seçin ve Hata **Ayıklama'yı yeniden başlatın** veya Hata **Ayıklama araç çubuğunda**Hata Ayıklama düğmesini yeniden **başlat'ı** seçin.

![Hata Ayıklama'yı Yenile](media/common/debug-action-refresh.png)

Hizmetinizi bir tarayıcıda açın ve güncelleştirilmiş iletinizin görüntülendiğini fark edin.

Azure Dev Spaces, kod edinimi her yapıldığında yeni bir kapsayıcı görüntüsünü yeniden oluşturma ve yeniden dağıtmak yerine, daha hızlı bir edit/hata ayıklama döngüsü sağlamak için kodu varolan kapsayıcının içinde aşamalı olarak yeniden derler.

## <a name="clean-up-your-azure-resources"></a>Azure kaynaklarınızı temizleme

```azurecli
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Geliştirme Alanları'nın birden çok kapsayıcıda daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda farklı sürümlerle veya kod dallarıyla çalışarak ortak geliştirmeyi nasıl basitleştirebileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Birden çok kapsayıcı ve takım geliştirme ile çalışma](multi-service-java.md)


[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
