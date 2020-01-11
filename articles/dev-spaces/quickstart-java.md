---
title: "Kubernetes 'te hata ayıklama ve yineleme: Visual Studio Code & Java"
services: azure-dev-spaces
ms.date: 07/08/2019
ms.topic: quickstart
description: Bu hızlı başlangıçta, Azure Kubernetes hizmetinde bir Java uygulamasını hata ayıklama ve hızla yinelemek için Azure Dev Spaces ve Visual Studio Code nasıl kullanılacağı gösterilmektedir
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Java, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s
manager: gwallace
ms.openlocfilehash: 9237137f86bf7b6b30eb908e0acbe3b8b0e624d9
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75867464"
---
# <a name="quickstart-debug-and-iterate-on-kubernetes-with-visual-studio-code-and-java---azure-dev-spaces"></a>Hızlı başlangıç: Visual Studio Code ve Java-Azure Dev Spaces ile Kubernetes 'te hata ayıklama ve yineleme

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure’da yönetilen bir Kubernetes ile Azure Dev Spaces’ı ayarlayın.
- Visual Studio Code kullanarak kapsayıcılardaki kodu tekrarlayarak geliştirin.
- Visual Studio Code geliştirici alanınızdaki kodda hata ayıklayın.

Azure Dev Spaces ayrıca şunları kullanarak hata ayıklamanıza ve yinelemenize olanak tanır:
- [Node. js ve Visual Studio Code](quickstart-nodejs.md)
- [.NET Core ve Visual Studio Code](quickstart-netcore.md)
- [.NET Core ve Visual Studio](quickstart-netcore-visualstudio.md)

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Hesabınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
- [Visual Studio Code yüklendi](https://code.visualstudio.com/download).
- Visual Studio Code için Azure Dev Spaces uzantıları için [Azure dev SPACES](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) ve [Java hata ayıklayıcı](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debugger-azds) .
- [Yüklü Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).
- [Maven yüklendi ve yapılandırıldı](https://maven.apache.org).

## <a name="create-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes hizmet kümesi oluşturma

[Desteklenen bir bölgede][supported-regions]aks kümesi oluşturmanız gerekir. Aşağıdaki komutlar *Myresourcegroup* adlı bir kaynak grubu ve *myaks*adlı bir aks kümesi oluşturur.

```cmd
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --disable-rbac --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>AKS kümenizde Azure Dev Spaces etkinleştirme

AKS kümenizde dev alanlarını etkinleştirmek ve istemleri izlemek için `use-dev-spaces` komutunu kullanın. Aşağıdaki komut *Myresourcegroup* grubundaki *myaks* kümesinde dev alanlarını etkinleştiriyor ve *varsayılan* bir dev alanı oluşturuyor.

> [!NOTE]
> `use-dev-spaces` komutu, zaten yüklenmemişse Azure Dev Spaces CLı 'yi de yükler. Azure Dev Spaces CLı 'yi Azure Cloud Shell yükleyemezsiniz.

```cmd
$ az aks use-dev-spaces -g MyResourceGroup -n MyAKS

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

Uygulamayı GitHub 'dan kopyalayın.

```cmd
git clone https://github.com/Azure/dev-spaces
```

## <a name="prepare-the-sample-application-in-visual-studio-code"></a>Örnek uygulamayı Visual Studio Code hazırlama

Visual Studio Code açın, *Dosya* ' yı açın *..* . ' a tıklayın, *dev-Spaces/Samples/Java/alma işlemi/webön uç* dizinine gidin ve *Aç*' a tıklayın.

Artık Visual Studio Code *Web ön uç* projesi açılır. Uygulamayı geliştirme alanınızda çalıştırmak için, komut paletindeki Azure Dev Spaces uzantısını kullanarak Docker ve hele grafik varlıklarını oluşturun.

Visual Studio Code içinde komut paletini açmak için, *görüntüle* ve ardından *komut paleti*' ne tıklayın. `Azure Dev Spaces` yazmaya başlayın ve `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`' a tıklayın.

![Yapılandırma dosyalarını Azure Dev Spaces hazırlama](./media/common/command-palette.png)

Visual Studio Code Ayrıca temel görüntülerinizi yapılandırmanıza, bağlantı noktası ve genel uç noktasına, temel görüntü için `Azul Zulu OpenJDK for Azure (Free LTS)`, gösterilen bağlantı noktası için `8080` ve genel bir uç noktayı etkinleştirmek için `Yes` ' ı seçmenizi ister.

![Temel görüntü seçin](media/get-started-java/select-base-image.png)

![Açık bağlantı noktası seçin](media/get-started-java/select-exposed-port.png)

![Ortak uç noktayı seçin](media/get-started-java/select-public-endpoint.png)

Bu komut, bir Dockerfile ve Held grafiği oluşturarak projenizi Azure Dev Spaces çalıştırmayı hazırlar. Ayrıca, projenizin kökünde hata ayıklama yapılandırmasıyla bir *. vscode* dizini oluşturur.

> [!TIP]
> Projeniz için [Dockerfile ve HELI grafiği](how-dev-spaces-works.md#prepare-your-code) , kodunuzu derlemek ve çalıştırmak için Azure dev Spaces tarafından kullanılır, ancak projenin oluşturulup çalıştırıldığını değiştirmek istiyorsanız bu dosyaları değiştirebilirsiniz.

## <a name="build-and-run-code-in-kubernetes-from-visual-studio"></a>Visual Studio 'dan Kubernetes 'te kod oluşturma ve çalıştırma

Sol taraftaki *hata ayıklama* simgesine tıklayın ve en üstteki *Java programını (AZD) Başlat* seçeneğine tıklayın.

![Java programını Başlat](media/get-started-java/debug-configuration.png)

Bu komut Azure Dev Spaces ' de hizmetinizi oluşturur ve çalıştırır. En alttaki *Terminal* penceresi, Azure dev Spaces çalıştıran hizmetinizin derleme çıkışını ve URL 'lerini gösterir. *Hata ayıklama konsolu* günlük çıktısını gösterir.

> [!Note]
> *Komut paletinde*herhangi bir Azure dev Spaces komutu görmüyorsanız [Azure dev Spaces için Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds)yüklediğinizden emin olun. Ayrıca, Visual Studio Code ' de *dev-Spaces/Samples/Java/alma-başlatma/webön uç* dizinini açtığınızdan emin olun.

Ortak URL 'YI açarak hizmetin çalıştığını görebilirsiniz.

Hata ayıklayıcıyı durdurmak için hata *Ayıkla* ve hata *ayıklamayı Durdur* ' a tıklayın.

## <a name="update-code"></a>Kodu güncelleştirme

Hizmetinizin güncelleştirilmiş bir sürümünü dağıtmak için, projenizdeki herhangi bir dosyayı güncelleştirebilir ve *Java programını Başlat ' ı (AZD)* yeniden çalıştırabilirsiniz. Örneğin:

1. Uygulamanız hala çalışıyorsa, *Hata Ayıkla* ' ya tıklayın, ardından *hata ayıklamayı* durdurun.
1. [`src/main/java/com/ms/sample/webfrontend/Application.java`satır 19 ' da](https://github.com/Azure/dev-spaces/blob/master/samples/java/getting-started/webfrontend/src/main/java/com/ms/sample/webfrontend/Application.java#L19) şu şekilde güncelleştirin:
    
    ```java
    return "Hello from webfrontend in Azure!";
    ```

1. Yaptığınız değişiklikleri kaydedin.
1. *Başlat Java programı 'nı (AZD)* yeniden çalıştırın.
1. Çalışan hizmetinize gidin ve değişikliklerinizi gözlemleyin.
1. Uygulamanızı durdurmak için *Hata Ayıkla* ve *hata ayıklamayı Durdur* ' a tıklayın.

## <a name="setting-and-using-breakpoints-for-debugging"></a>Hata ayıklama için kesme noktaları ayarlama ve kullanma

Başlat *Java programı 'nı (AZD)* kullanarak hizmetinizi başlatın. Bu ayrıca hizmetinizi hata ayıklama modunda çalıştırır.

*Görünüm* *' e*tıklayarak *Gezgin görünümüne geri* gidin. `src/main/java/com/ms/sample/webfrontend/Application.java` açın ve imlecinizi buraya yerleştirmek için 19. satırda herhangi bir yere tıklayın. Kesme noktası isabet *F9* ayarlamak Için, *Hata Ayıkla* ' ya tıklayın ve ardından *kesme noktasını değiştirin*

Hizmetinizi bir tarayıcıda açın ve hiçbir ileti görüntülenmediğini unutmayın. Visual Studio Code dön ve 19 satırı gözlemle vurgulanır. Ayarladığınız kesme noktası 19. satırdaki hizmeti duraklattı. Hizmeti sürdürmek için *F5* 'e basın veya *Hata Ayıkla* 'Ya tıkladıktan sonra *devam edin*. Tarayıcınıza geri dönün ve iletinin şimdi görüntülendiğini unutmayın.

Bir hata ayıklayıcı eklenmiş olarak Kubernetes 'de hizmetinizi çalıştırırken, çağrı yığını, yerel değişkenler ve özel durum bilgileri gibi bilgileri hata ayıklamaya yönelik tam erişime sahip olursunuz.

İmleci `src/main/java/com/ms/sample/webfrontend/Application.java` 19. satıra koyarak ve *F9*tuşuna basarak kesme noktasını kaldırın.

## <a name="update-code-from-visual-studio-code"></a>Visual Studio Code kodu güncelleştirme

Hizmet hata ayıklama modunda çalışırken, `src/main/java/com/ms/sample/webfrontend/Application.java`satır 19 ' u güncelleştirin. Örneğin:
```java
return "Hello from webfrontend in Azure while debugging!";
```

Dosyayı kaydedin. Hata *Ayıkla* ' ya tıklayın ve *hata ayıklamayı yeniden başlatın* veya *Hata Ayıkla araç çubuğunda* *hata ayıklamayı yeniden Başlat* düğmesine tıklayın

![Hata ayıklamayı Yenile](media/common/debug-action-refresh.png)

Hizmetinizi bir tarayıcıda açın ve güncelleştirilmiş iletinizin görüntülendiğini unutmayın.

Kod düzenlemeleri her yapıldığında yeni bir kapsayıcı görüntüsünü yeniden oluşturmak ve yeniden dağıtmak yerine, daha hızlı bir düzenleme/hata ayıklama döngüsü sağlamak üzere mevcut kapsayıcı içindeki kodu artımlı olarak yeniden derler Azure Dev Spaces.

## <a name="clean-up-your-azure-resources"></a>Azure kaynaklarınızı Temizleme

```cmd
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Dev Spaces birden çok kapsayıcı genelinde daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda kodunuzun farklı sürümleriyle veya dallarıyla çalışarak işbirliğine dayalı geliştirmeyi nasıl kolaylaştırabileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Birden çok kapsayıcı ve takım geliştirme ile çalışma](multi-service-java.md)


[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
