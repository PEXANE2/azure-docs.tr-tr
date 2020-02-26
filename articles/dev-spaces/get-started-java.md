---
title: 'Kubernetes geliştirme alanı oluşturma: Visual Studio Code & Java'
services: azure-dev-spaces
author: stepro
ms.author: stephpr
ms.date: 09/26/2018
ms.topic: tutorial
description: Bu öğreticide, Azure Kubernetes hizmetinde bir Java uygulamasını hata ayıklama ve hızla yinelemek için Azure Dev Spaces ve Visual Studio Code nasıl kullanılacağı gösterilmektedir
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s
manager: gwallace
ms.openlocfilehash: 2c086c952528d5521ace44d56eaf11c14ac1cabb
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77602835"
---
# <a name="create-a-kubernetes-dev-space-visual-studio-code-and-java-with-azure-dev-spaces"></a>Bir Kubernetes geliştirme alanı oluşturun: Azure Dev Spaces ile Visual Studio Code ve Java

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure’da geliştirme için iyileştirilmiş Kubernetes tabanlı bir ortam (bir _dev space_) oluşturun.
- VS Code'u ve komut satırını kullanarak kapsayıcılarda yinelemeli kod geliştirin.
- Kodunuzu bir ekip ortamında verimli bir şekilde geliştirip test edin.

> [!Note]
> Herhangi bir zamanda **takıldıysanız** , [sorun giderme](troubleshooting.md) bölümüne bakın.

## <a name="install-the-azure-cli"></a>Azure CLI'yı yükleme
Azure Dev Spaces, çok az yerel makine kurulumu gerektirir. Geliştirme ortamı yapılandırmanızın büyük bölümü bulutta depolanır ve diğer kullanıcılarla paylaşılabilir. İlk olarak [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) indirip yükleyin.

### <a name="sign-in-to-azure-cli"></a>Azure CLI'da oturum açma
Azure'da oturum açın. Bir terminal penceresine aşağıdaki komutu yazın:

```cmd
az login
```

> [!Note]
> Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.

#### <a name="if-you-have-multiple-azure-subscriptions"></a>Birden çok Azure aboneliğiniz varsa...
Şunu çalıştırarak aboneliklerinizi görüntüleyebilirsiniz: 

```cmd
az account list --output table
```

*IsDefault*Için *true değerine* sahip aboneliği bulun.
Kullanmak istediğiniz abonelik bu değilse, varsayılan aboneliği değiştirebilirsiniz:

```cmd
az account set --subscription <subscription ID>
```

## <a name="create-a-kubernetes-cluster-enabled-for-azure-dev-spaces"></a>Azure Dev Spaces için bir Kubernetes kümesi oluşturma

Komut isteminde, [Azure dev Spaces destekleyen bir bölgede][supported-regions]kaynak grubunu oluşturun.

```cmd
az group create --name MyResourceGroup --location <region>
```

Şu komutu kullanarak bir Kubernetes kümesi oluşturun:

```cmd
az aks create -g MyResourceGroup -n MyAKS --location <region> --generate-ssh-keys
```

Kümenin oluşturulması birkaç dakika sürer.

### <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>AKS kümenizi Azure Dev Spaces kullanacak şekilde yapılandırma

AKS kümenizi içeren kaynak grubuyla AKS kümesi adınızı kullanarak aşağıdaki Azure CLI komutunu girin. Komut, kümenizi Azure Dev Spaces desteğiyle yapılandırır.

   ```cmd
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```

> [!IMPORTANT]
> Azure Dev Spaces yapılandırma işlemi, varsa kümedeki `azds` ad alanını kaldırır.

## <a name="get-kubernetes-debugging-for-vs-code"></a>VS Code için Kubernetes hata ayıklaması edinin
Kubernetes hata ayıklaması gibi zengin özellikler VS Code kullanarak .NET Core ve Node.js geliştiricileri için kullanılabilir.

1. Yüklü değilse, [VS Code](https://code.visualstudio.com/Download)’u yükleyin.
1. [VS Azure Dev Spaces uzantısını](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) indirip yükleyin. Uzantının Market sayfasında ve yeniden VS Code’da Yükle’ye bir kez tıklayın. 

Azure Dev Spaces ile Java uygulamalarında hata ayıklamak istiyorsanız VS Code [Azure Dev Spaces için Java Hata Ayıklayıcısı](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debugger-azds) uzantısını indirin. Uzantının Market sayfasında ve yeniden VS Code’da Yükle’ye bir kez tıklayın.

## <a name="create-a-web-app-running-in-a-container"></a>Kapsayıcıda çalışan bir web uygulaması oluşturma

Bu bölümde bir Java web uygulaması oluşturacak ve Kubernetes’teki bir kapsayıcı içinde çalıştıracaksınız.

### <a name="create-a-java-web-app"></a>Java web uygulaması oluşturma
[https://github.com/Azure/dev-spaces](https://github.com/Azure/dev-spaces) giderek GitHub 'dan kod Indirin ve GitHub deposunu yerel ortamınıza Indirmek için **Kopyala veya indir** ' i seçin. Bu kılavuzun kodu `samples/java/getting-started/webfrontend` içindedir.

## <a name="preparing-code-for-docker-and-kubernetes-development"></a>Docker ve Kubernetes geliştirme için kod hazırlama
Şimdiye kadar, yerel olarak çalıştırılabilen temel bir web uygulamanız vardı. Şimdi uygulamanın kapsayıcısını tanımlayan varlıklar oluşturup Kubernetes’de nasıl dağıtılacağını belirleyerek uygulamayı kapsayıcılı hale getireceksiniz. Bu görev Azure Dev Spaces ile kolayca gerçekleştirilebilir: 

1. VS Code’u başlatın ve `webfrontend` klasörünü açın. (Hata ayıklama varlıkları eklemek veya projeyi geri yüklemek için tüm varsayılan istemleri yoksayabilirsiniz.)
1. VS Code’da Tümleşik Terminal’i açın (**Görünüm > Tümleşik Terminal** menüsünü kullanarak).
1. Şu komutu çalıştırın (geçerli klasörünüzün **webfrontend** olduğundan emin olun):

    ```cmd
    azds prep --enable-ingress
    ```

Azure CLI’nin `azds prep` komutu varsayılan ayarlarla Docker ve Kubernetes varlıklarını oluşturur:
* `./Dockerfile`, uygulamanın kapsayıcı görüntüsünü açıklar, kaynak kodunun nasıl derlendiğini ve kapsayıcının içinde çalıştırıldığını belirtir.
* [ altındaki ](https://docs.helm.sh)Helm grafiği`./charts/webfrontend`, kapsayıcının Kubernetes'de nasıl dağıtıldığını açıklar.

> [!TIP]
> Projeniz için [Dockerfile ve HELI grafiği](how-dev-spaces-works.md#prepare-your-code) , kodunuzu derlemek ve çalıştırmak için Azure dev Spaces tarafından kullanılır, ancak projenin oluşturulup çalıştırıldığını değiştirmek istiyorsanız bu dosyaları değiştirebilirsiniz.

Şimdilik bu dosyaların tüm içeriğini anlamanız gerekli değildir. Bununla birlikte, **geliştirme aşamasından üretim aşamasına kadar aynı Kubernetes ve Docker kod yapılandırmalı varlıklarının kullanılabildiğini, bu şekilde farklı ortamlarda daha tutarlı sonuçlar sağlanabildiğini** belirtmek gerekir.
 
`./azds.yaml` komutuyla `prep` adlı bir dosya da oluşturulur ve bu dosya Azure Dev Spaces’in yapılandırma dosyasıdır. Azure’da yinelemeli bir geliştirme deneyimi sağlamak için Docker ve Kubernetes yapıtlarını ek yapılandırmayla tamamlar.

## <a name="build-and-run-code-in-kubernetes"></a>Kubernetes'de kodu oluşturma ve çalıştırma
Şimdi kodumuzu çalıştıralım! Terminal penceresinde, bu komutu webfrontend **kök kod klasöründen** çalıştırın:

```cmd
azds up
```

Komutun çıkışını gözden geçirin; ilerledikçe bazı şeyler göreceksiniz:
- Kaynak kod Azure Dev Space ile eşitlenir.
- Kod klasörünüzdeki Docker varlıkları tarafından belirtildiği gibi Azure'da bir kapsayıcı görüntüsü oluşturulur.
- Kod klasörünüzdeki Helm grafiği tarafından belirtildiği gibi kapsayıcı görüntüsünü kullanan Kubernetes nesneleri oluşturulur.
- Kapsayıcının uç noktaları hakkındaki bilgiler görüntülenir. Bizim örneğimizde, genel bir HTTP URL'si görmeyi bekleriz.
- Yukarıdaki aşamaların başarıyla tamamlandığını varsayarsak, kapsayıcı başlatılırken `stdout` (ve `stderr`) çıkışını görmeye başlamalısınız.

> [!Note]
> Bu adımlar, `up` komutu ilk kez çalıştırıldığında biraz uzun sürer ama izleyen çalıştırmalar daha hızlı olacaktır.

### <a name="test-the-web-app"></a>Web uygulamasını test etme
`up` komutu tarafından oluşturulan genel URL hakkındaki bilgiler için konsol çıkışını tarayın. Şu biçimde olacaktır: 

```
(pending registration) Service 'webfrontend' port 'http' will be available at <url>
Service 'webfrontend' port 'http' is available at http://webfrontend.1234567890abcdef1234.eus.azds.io/
Service 'webfrontend' port 80 (TCP) is available at 'http://localhost:<port>'
```

`up` komutundan çıkışta hizmetin genel URL 'sini belirler. `.azds.io`biter. Yukarıdaki örnekte, genel URL `http://webfrontend.1234567890abcdef1234.eus.azds.io/`.

Web uygulamanızı görmek için genel URL 'YI bir tarayıcıda açın. Ayrıca, `stdout` ve `stderr` çıkışı, Web uygulamanızla etkileşime geçerek *azds Trace* Terminal penceresine akışla kaydedilir. Ayrıca, sistemde ilerlediklerinde HTTP istekleri için izleme bilgilerini görürsünüz. Bu, geliştirme sırasında karmaşık çok hizmet çağrılarını izlemenizi kolaylaştırır. Dev Spaces tarafından eklenen araçlar bu istek izlemeyi sağlar.

> [!Note]
> Genel URL 'nin yanı sıra, konsol çıkışında görüntülenen alternatif `http://localhost:<portnumber>` URL 'sini de kullanabilirsiniz. Localhost URL'sini kullanırsanız kapsayıcı yerel olarak çalışıyor gibi görünebilir, ancak gerçekte AKS'de çalışıyordur. Azure Dev Spaces, Kubernetes *bağlantı noktası-iletme* işlevini kullanarak localhost bağlantı noktasını aks 'de çalışan kapsayıcıya eşler. Bu, yerel makinenizden hizmetle etkileşimde bulunmayı kolaylaştırır.

### <a name="update-a-content-file"></a>İçerik dosyası güncelleştirme
Azure Dev Spaces yalnızca kodu Kubernetes’te çalıştırmaya yönelik değildir; aynı zamanda kod değişikliklerinizin buluttaki bir Kubernetes ortamında uygulandığını hızlıca ve yinelenerek görmenizi sağlar.

1. Terminal penceresinde `Ctrl+C` düğmesine basın (`azds up` hizmetini durdurmak için).
1. `src/main/java/com/ms/sample/webfrontend/Application.java`açın ve [19. satırdaki](https://github.com/Azure/dev-spaces/blob/master/samples/java/getting-started/webfrontend/src/main/java/com/ms/sample/webfrontend/Application.java#L19)tebrik iletisini düzenleyin:

    ```java
    return "Hello from webfrontend in Azure!";
    ```

1. Dosyayı kaydedin.
1. Terminal penceresinde `azds up` komutunu çalıştırın.

Bu komut, kapsayıcı görüntüsünü yeniden derler ve Helm grafiğini yeniden dağıtır. Kod değişikliklerinizin çalışan uygulamada etkili olup olmadığını görmek için tarayıcıyı yenileyin.

Bununla birlikte, kod geliştirmek için sonraki bölümde öğreneceğiniz daha da *hızlı bir yöntem* mevcuttur. 

## <a name="debug-a-container-in-kubernetes"></a>Kubernetes’te bir kapsayıcının hatalarını ayıklama

Bu bölümde, Azure'da çalıştırılan kapsayıcımızda doğrudan hata ayıklamak için VS Code kullanacaksınız. Ayrıca daha hızlı bir düzenleme-çalıştırma-test döngüsü elde etmeyi öğreneceksiniz.

![](media/common/edit-refresh-see.png)

> [!Note]
> Herhangi bir zamanda **kilitlenirseniz**[Sorun giderme](troubleshooting.md) bölümüne başvurun veya bu sayfada bir yorum paylaşın.

### <a name="initialize-debug-assets-with-the-vs-code-extension"></a>Hata ayıklama varlıklarını VS Code uzantısıyla başlatma
VS Code'un Azure Dev Space'imizle iletişim kurabilmesi için önce kod projenizi yapılandırmalısınız. Azure Dev Spaces için VS Code uzantısı hata ayıklama yapılandırmasını ayarlamak için yardımcı komutu sağlar. 

**Komut Paleti**'ni açın (**Görünüm | Komut Paleti** menüsünü kullanarak) ve otomatik tamamlama özelliğini kullanarak komutu yazın ve seçin: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. 

Bu, `.vscode` klasörünün altında Azure Dev Spaces için hata ayıklama yapılandırması ekler. Bu komut, projeyi dağıtım için yapılandıran `azds prep` komutuyla karıştırılmamalıdır.

![](media/common/command-palette.png)

### <a name="select-the-azds-debug-configuration"></a>AZDS hata ayıklama yapılandırmasını seçme
1. Hata Ayıklama görünümünü açmak için VS Code’un yan tarafındaki **Etkinlik Çubuğu** içinde Hata Ayıklama simgesine tıklayın.
1. Etkin hata ayıklama yapılandırması olarak **Java Programını Başlat (AZDS)** öğesini seçin.

![](media/get-started-java/debug-configuration.png)

> [!Note]
> Komut Paletinde herhangi bir Azure Dev Spaces komutu görmüyorsanız, Azure Dev Spaces için VS Code uzantısını yüklediğinizden emin olun. VS Code ' de açtığınız çalışma alanının `azds.yaml`içeren klasör olduğundan emin olun.

### <a name="debug-the-container-in-kubernetes"></a>Kubernetes’te kapsayıcının hatalarını ayıklama
Kubernetes’te kodunuzun hatalarını ayıklamak için **F5**’e basın.

`up` komutunda olduğu gibi kod, geliştirme ortamıyla eşitlenir ve bir kapsayıcı derlenip Kubernetes’e dağıtılır. Elbette bu kez, hata ayıklayıcı uzak kapsayıcıya eklenir.

> [!Tip]
> VS Code durum çubuğu, hata ayıklayıcının ekli olduğunu belirten turuncu kullanacaktır. Ayrıca, uygulamanızı açmak için kullanabileceğiniz tıklatılabilir bir URL görüntüler.

![](media/common/vscode-status-bar-url.png)

Sunucu tarafı kod dosyasında (örneğin `greeting()` kaynak dosyasındaki `src/main/java/com/ms/sample/webfrontend/Application.java` işlevi içinde) bir kesme noktası belirleyin. Tarayıcı sayfasının yenilenmesi, kesme noktasına ulaşılmasına neden olur.

Kodun yerel olarak yürütülmesi durumunda olduğu gibi, çağrı yığını, yerel değişkenler, özel durum bilgileri vb. hata ayıklama bilgilerine tam erişiminiz vardır.

### <a name="edit-code-and-refresh"></a>Kod düzenleme ve yenileme
Hata ayıklayıcı etkinken bir kod düzenlemesi yapın. Örneğin `src/main/java/com/ms/sample/webfrontend/Application.java` karşılamasını değiştirin. 

```java
public String greeting()
{
    return "I'm debugging Java code in Azure!";
}
```

Dosyayı kaydedin ve **Hata Ayıkla eylemleri bölmesinde** **Yeniden Başlat** düğmesine tıklayın.

![](media/common/debug-action-refresh.png)

Azure Dev Spaces, her kod düzenlemesi yapıldığında yeni bir kapsayıcı görüntüsünü yeniden derleme ve yeniden dağıtmayı içeren uzun süreli işlem yerine, mevcut kapsayıcı içindeki kodu artımlı bir şekilde yeniden derleyerek daha hızlı bir düzenleme/hata ayıklama döngüsü sağlar.

Tarayıcıda web uygulamasını yenileyin. Özel iletinizin kullanıcı arabiriminde görüntülendiğini görürsünüz.

**Artık kod üzerinde hızlıca yineleme ve doğrudan Kubernetes’te hata ayıklamaya yönelik bir yönteminiz var!** Ardından, ikinci bir kapsayıcıyı nasıl oluşturabileceğinizi ve çağırabileceğinizi göreceksiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Çoklu hizmet geliştirme hakkında bilgi edinin](multi-service-java.md)


[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
