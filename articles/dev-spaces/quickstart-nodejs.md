---
title: 'Kubernetes üzerinde hata ayıklama ve yinele: Visual Studio Kodu & Düğüm.js'
services: azure-dev-spaces
ms.date: 07/08/2019
ms.topic: quickstart
description: Bu hızlı başlangıç, Azure Kubernetes Hizmeti'ndeki bir Düğüm uygulamasını hata ayıklamak ve hızla doğrulamak için Azure Geliştirme Alanları ve Görsel Stüdyo Kodunu nasıl kullanacağınızı gösterir
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s
manager: gwallace
ms.openlocfilehash: 18171a2f8d13bfcf3df76b1453c39c59cab89d7c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80240191"
---
# <a name="quickstart-debug-and-iterate-on-kubernetes-with-visual-studio-code-and-nodejs---azure-dev-spaces"></a>Quickstart: Visual Studio Code ve Node.js ile Kubernetes'te hata ayıklama ve yinele - Azure Dev Spaces

Bu hızlı başlangıçta, Azure Dev Spaces'i yönetilen bir Kubernetes kümesiyle ayarlar sınız ve visual studio code'da bir Node.js uygulamasını kullanarak kapsayıcılarda yinelemeli bir şekilde kod geliştirip hata ayıklama nız sağlar. Azure Geliştirme Alanları, en az geliştirme makinesi kurulumuyla Azure Kubernetes Hizmeti'nde (AKS) uygulamanızın tüm bileşenlerini ayıklamanızı ve test etmenizi sağlar. 

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) 
- [Node.js en son sürümü](https://nodejs.org/download/).
- [Görsel Stüdyo Kodu](https://code.visualstudio.com/download).
- Visual Studio Code için [Azure Geliştirme Alanları](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) uzantısı.
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

Visual Studio Code'u açın, **Dosya'yı** seçin ve **Aç'ı**seçin, *boşluk/örnekler/düğümler/başlangıç/webfrontend* dizinine gidin ve **Aç'ı**seçin.

Şimdi Visual Studio Code açık *webfrontend* proje var. Uygulamayı geliştirme alanınızda çalıştırmak için, Komut Paleti'ndeki Azure Geliştirme Alanları uzantısını kullanarak Docker ve Miğfer grafik varlıklarını oluşturun.

Visual Studio Code'da Komut Paleti'ni açmak için **Görünüm** ve **Komut Paleti'ni**seçin. Yazmaya `Azure Dev Spaces` başlayın ve **Azure Dev Alanları'nı seçin: Azure Dev Alanları için yapılandırma dosyaları hazırlayın.**

![Azure Geliştirme Alanları için yapılandırma dosyaları hazırlama](./media/common/command-palette.png)

Visual Studio Code ayrıca ortak bitiş noktanızı yapılandırmanızı `Yes` istediğinde, ortak bir bitiş noktasını etkinleştirmeyi seçin.

![Ortak bitiş noktasını seçin](media/common/select-public-endpoint.png)

Bu komut, dockerfile ve Miğfer grafiği oluşturarak projenizi Azure Dev Spaces'te çalıştırmaya hazırlar. Ayrıca, projenizin kökünde hata ayıklama yapılandırması içeren bir *.vscode* dizini oluşturur.

> [!TIP]
> Projenizin [Dockerfile ve Mişl-işme grafiği,](how-dev-spaces-works-prep.md#prepare-your-code) kodunuzu oluşturmak ve çalıştırmak için Azure Dev Spaces tarafından kullanılır, ancak projenin oluşturulma ve çalışma şeklini değiştirmek isterseniz bu dosyaları değiştirebilirsiniz.

## <a name="build-and-run-code-in-kubernetes-from-visual-studio-code"></a>Visual Studio Code'tan Kubernetes'te kod oluşturma ve çalıştırma

Soldaki **Hata Ayıklama** simgesini seçin ve üstteki **Başlat Sunucusu'nu (AZDS)** seçin.

![Sunucubaşlat](media/get-started-node/debug-configuration-nodejs.png)

Bu komut, Azure Dev Spaces'te hizmetinizi oluşturur ve çalıştırUr. Alttaki **Terminal** penceresi, Azure Dev Alanları çalıştıran hizmetiniz için yapı çıktısını ve URL'lerini gösterir. **Hata Ayıklama Konsolu** günlük çıktısını gösterir.

> [!Note]
> **Komut**Paleti'nde azure Dev Spaces komutları görmüyorsanız, [Azure Dev Spaces için Visual Studio Code uzantısını](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds)yüklediğinizden emin olun. Ayrıca Visual Studio Code'da *dev-spaces/samples/nodejs/getting-started/webfrontend* dizinini açtığınızı da doğrulayın.

Genel URL'yi açarak hizmetin çalıştığını görebilirsiniz.

Hata **ayıklamayı** durdurmak için Hata **Ayıklama'yı seçin.**

## <a name="update-code"></a>Kodu güncelleştirme

Hizmetinizin güncelleştirilmiş bir sürümünü dağıtmak için, projenizdeki herhangi bir dosyayı güncelleyebilir ve **Launch Server'ı**yeniden çalıştırabilirsiniz. Örnek:

1. Uygulamanız hala çalışıyorsa, Hata Ayıklama'yı seçin ve durdurmak için **Hata Ayıklama'yı** Durdurun'u seçin. **Debug**
1. [Güncelleme satırı 13 için: `server.js` ](https://github.com/Azure/dev-spaces/blob/master/samples/nodejs/getting-started/webfrontend/server.js#L13)
    
    ```javascript
        res.send('Hello from webfrontend in Azure');
    ```

1. Yaptığınız değişiklikleri kaydedin.
1. Yeniden Başlat **Fırlatma Sunucusu.**
1. Çalışan servisinize gidin ve değişikliklerinizi gözlemleyin.
1. Hata **Debug** Ayıklama'yı seçin ve uygulamanızı durdurmak için **Hata Ayıklama'yı durdurun'** u seçin.

## <a name="setting-and-using-breakpoints-for-debugging"></a>Hata ayıklama için kesme noktalarını ayarlama ve kullanma

Launch Server **(AZDS)** kullanarak hizmetinizi başlatın.

**Görünüm** sonra **Explorer'ı**seçerek Explorer görünümüne geri gidin. *Server.js'yi* açın ve imlecinizi oraya koymak için satır 13'te bir yere tıklayın. Bir kesme noktası ayarlamak için **F9** tuşuna basın veya **Hata Ayıklama'yı** seçin ve ardından Kesme **Noktası'nı Toggle'** ı seçin.

Hizmetinizi bir tarayıcıda açın ve hiçbir iletinin görüntülenmeyişolduğunu fark edin. Visual Studio Code'a dönün ve 13. Ayarladığınız kesme noktası, 13. Hizmeti devam ettirmek için **F5** tuşuna basın veya **Hata Ayıklama'yı** seçin ve **devam edin.** Tarayıcınıza dönün ve iletinin şimdi görüntülendiğini fark edin.

Hizmetinizi Kubernetes'te bir hata ayıklama ekli yken, arama yığını, yerel değişkenler ve özel durum bilgileri gibi hata ayıklama bilgilerine tam erişime sahipsiniz.

İmlecinizi *server.js'de* 13. satıra koyup **F9**tuşuna basarak kesme noktasını kaldırın.

Hata **ayıklamayı** durdurmak için Hata **Ayıklama'yı seçin.**

## <a name="update-code-from-visual-studio-code"></a>Visual Studio Kodu'ndan kodu güncelleştir

Hata ayıklama modunu **Sunucuya (AZDS) eklemek** için değiştirin ve hizmeti başlatın:

![](media/get-started-node/attach-nodejs.png)

Bu komut, Azure Dev Spaces'te hizmetinizi oluşturur ve çalıştırUr. Ayrıca, hizmetin kapsayıcısında bir [nodemon](https://nodemon.io) işlemi başlatır ve buna VS Kodu bağlar. Kaynak kodu değişiklikleri *yapıldığında, nodemon* işlemi otomatik yeniden başlatmasağlar, yerel makinenizde geliştirmeye benzer daha hızlı iç döngü geliştirme sağlar.

Hizmet başladıktan sonra tarayıcınızı kullanarak tarayıcıya gidin ve onunla etkileşime başlayın.

Hizmet çalışırken, VS Kodu'na dönün ve *server.js'deki*13. Örnek:
```javascript
    res.send('Hello from webfrontend in Azure while debugging!');
```

Dosyayı kaydedin ve bir tarayıcıda hizmetinize dönün. Hizmetle etkileşime alın ve güncelleştirilmiş iletinizin görüntülendiğini fark edin.

*Nodemon*çalışırken, Düğüm işlemi herhangi bir kod değişikliği algılanır algılanır algılanmadı otomatik olarak yeniden başlatılır. Bu otomatik yeniden başlatma işlemi, yerel makinenizde hizmetinizi düzenleme ve yeniden başlatma deneyimine benzer ve bir iç döngü geliştirme deneyimi sağlar.

## <a name="clean-up-your-azure-resources"></a>Azure kaynaklarınızı temizleme

```azurecli
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Geliştirme Alanları'nın birden çok kapsayıcıda daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda farklı sürümlerle veya kod dallarıyla çalışarak ortak geliştirmeyi nasıl basitleştirebileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Birden çok kapsayıcı ve takım geliştirme ile çalışma](multi-service-nodejs.md)


[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
