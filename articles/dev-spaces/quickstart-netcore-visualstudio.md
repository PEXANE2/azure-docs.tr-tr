---
title: "Kubernetes'te hata ayıklama ve yinele: Visual Studio & .NET Core"
services: azure-dev-spaces
ms.date: 11/13/2019
ms.topic: quickstart
description: Bu hızlı başlangıç, Azure Kubernetes Hizmeti'ndeki bir .NET Core uygulamasını hata ayıklamak ve hızla doğrulamak için Azure Dev Spaces ve Visual Studio'yu nasıl kullanacağınızı gösterir
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s
manager: gwallace
ms.custom: vs-azure
ms.workload: azure-vs
ms.openlocfilehash: 5d0d1541600f4c10b021d5d7f7f435f1aa5ae589
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80239715"
---
# <a name="quickstart-debug-and-iterate-on-kubernetes-visual-studio--net-core---azure-dev-spaces"></a>Quickstart: Kubernetes'te hata ayıklama ve yinele: Visual Studio & .NET Core - Azure Dev Spaces

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure'da yönetilen bir Kubernetes ile Azure Dev Spaces'ı ayarlayın.
- Visual Studio kullanarak kapsayıcılarda yinelemeli kod geliştirin.
- Visual Studio'u kullanarak kümenizde çalışan hata ayıklama kodu.

Azure Dev Spaces ayrıca şu ları kullanarak hata ayıklamanızı ve yinelemenizi de sağlar:
- [Java ve Görsel Stüdyo Kodu](quickstart-java.md)
- [Düğüm.js ve Görsel Stüdyo Kodu](quickstart-nodejs.md)
- [.NET Çekirdek ve Görsel Stüdyo Kodu](quickstart-netcore.md)

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Hesabınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
- Visual Studio 2019 Windows'da Azure Geliştirme iş yükü yüklü. Web Geliştirme iş yükü ve [Kubernetes için Visual Studio Araçları](https://aka.ms/get-vsk8stools) yüklü windows'da Visual Studio 2017'yi de kullanabilirsiniz. Visual Studio yüklü yoksa, [buradan](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs)indirin.

## <a name="create-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes Hizmet kümesi oluşturma

Desteklenen bir [bölgede][supported-regions]bir AKS kümesi oluşturmanız gerekir. Küme oluşturmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın
1. Seçin *+ Kubernetes Hizmeti > kaynak oluşturun.* 
1. _Abonelik,_ _Kaynak Grubu,_ _Kubernetes küme adı,_ _Bölge_, _Kubernetes sürümü_ve _DNS adı öneki_girin.

    ![Azure portalında AKS oluşturma](media/get-started-netcore-visualstudio/create-aks-portal.png)

1. *Gözden geçir ve oluştur*’a tıklayın.
1. *Oluştur'u*tıklatın.

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>AKS kümenizde Azure Dev Alanları'nı etkinleştirme

Azure portalında AKS kümenize gidin ve *Dev Spaces'i*tıklatın. Dev Alanları *Evet* olarak *değiştirin* ve *Kaydet'i*tıklatın.

![Azure portalında Dev Boşlukları etkinleştirme](media/get-started-netcore-visualstudio/enable-dev-spaces-portal.png)

## <a name="create-a-new-aspnet-web-app"></a>Yeni bir ASP.NET web uygulaması oluşturma

1. Visual Studio'yu açın.
1. Yeni bir proje oluşturma.
1. *Core Web Uygulaması ASP.NET* seçin ve *İleri'yi*tıklatın.
1. Projenizi *webfrontend'e* adlandırın ve *Oluştur'u*tıklatın.
1. İstendiğinde, şablon için *Web Uygulaması 'nı (Model-View-Controller)* seçin.
1. En üstte *.NET Core* ve *ASP.NET Core 2.1'i* seçin.
1. *Oluştur'u*tıklatın.

## <a name="connect-your-project-to-your-dev-space"></a>Projenizi geliştirme alanınıza bağlayın

Projenizde, aşağıda gösterildiği gibi başlatma ayarlarından **Azure Dev Spaces'i** seçin.

![](media/get-started-netcore-visualstudio/LaunchSettings.png)

Azure Dev Spaces iletişim *kutusunda, Abonelik* ve *Azure Kubernetes Cluster'ınızı*seçin. *Alan* kümesini *varsayılan* olarak bırakın ve Genel *Olarak Erişilebilir* onay kutusunu etkinleştirin. *Tamam*'a tıklayın.

![](media/get-started-netcore-visualstudio/Azure-Dev-Spaces-Dialog.png)

Bu işlem, hizmetinizi genel olarak erişilebilen bir URL ile *varsayılan* dev alanına dağıtır. Azure Dev Spaces ile çalışacak şekilde yapılandırılmamış bir küme seçerseniz, yapılandırmak isteyip istemediğinizi soran bir ileti görürsünüz. *Tamam*'a tıklayın.

![](media/get-started-netcore-visualstudio/Add-Azure-Dev-Spaces-Resource.png)

*Varsayılan* geliştirme alanında çalışan hizmetin genel *URL'si Çıktı* penceresinde görüntülenir:

```cmd
Starting warmup for project 'webfrontend'.
Waiting for namespace to be provisioned.
Using dev space 'default' with target 'MyAKS'
...
Successfully built 1234567890ab
Successfully tagged webfrontend:devspaces-11122233344455566
Built container image in 39s
Waiting for container...
36s

Service 'webfrontend' port 'http' is available at http://default.webfrontend.1234567890abcdef1234.eus.azds.io/
Service 'webfrontend' port 80 (http) is available at http://localhost:62266
Completed warmup for project 'webfrontend' in 125 seconds.
```

Yukarıdaki örnekte, genel URL' dir. http://default.webfrontend.1234567890abcdef1234.eus.azds.io/ 

**Hata** Ayıklama'yı seçin ve **hata ayıklamayı başlatın.** Birkaç saniye sonra hizmetiniz başlayacak ve Visual Studio hizmetin genel URL'sini içeren bir tarayıcı açacaktır. Bir tarayıcı otomatik olarak açılmıyorsa, bir tarayıcıda hizmetinizin herkese açık URL'sine gidin ve geliştirme alanınızda çalışan hizmetle etkileşimkurun.

Bu işlem, hizmetinize genel erişimi devre dışı bıraktı rilebilir. Genel erişimi etkinleştirmek [ *için, values.yaml'deki*giriş değerini][ingress-update]güncelleştirebilirsiniz.

## <a name="update-code"></a>Kodu güncelleştirme

Visual Studio hala dev alanınıza bağlıysa, durdur düğmesini tıklatın. Satır 20'yi şu şekilde değiştirin: `Controllers/HomeController.cs`
    
```csharp
ViewData["Message"] = "Your application description page in Azure.";
```

Değişikliklerinizi kaydedin ve **Hata Ayıklama'yı** seçin ve hata **ayıklamaya başlayın'ı**seçin. Birkaç saniye sonra hizmetiniz başlayacak ve Visual Studio hizmetin genel URL'sini içeren bir tarayıcı açacaktır. Bir tarayıcı otomatik olarak açılmıyorsa, bir tarayıcıda hizmetinizin genel URL'sinde gezin ve *Hakkında'yı*tıklatın. Güncelleştirilmiş iletinizin görüntülediğini gözlemleyin.

Azure Dev Spaces, kod edinimi her yapıldığında yeni bir kapsayıcı görüntüsünü yeniden oluşturma ve yeniden dağıtmak yerine, daha hızlı bir edit/hata ayıklama döngüsü sağlamak için kodu varolan kapsayıcının içinde aşamalı olarak yeniden derler.

## <a name="setting-and-using-breakpoints-for-debugging"></a>Hata ayıklama için kesme noktalarını ayarlama ve kullanma

Visual Studio hala dev alanınıza bağlıysa, durdur düğmesini tıklatın. Açılıp `Controllers/HomeController.cs` imlecinizi oraya koymak için 20. Bir kesme noktası *f9* vurmak ayarlamak için veya *Hata Ayıklama* sonra *Breakpoint toggle*tıklatın. Geliştirme alanınızda hata ayıklama modunda hizmetinizi başlatmak için *F5* tuşuna basın veya *Hata* *Ayıklama'yı başlatın'* ı tıklatın.

Hizmetinizi bir tarayıcıda açın ve hiçbir iletinin görüntülenmeyişolduğunu fark edin. Visual Studio'ya dönün ve 20. Ayarladığınız kesme noktası, 20. Hizmeti devam ettirmek için *F5* tuşuna basveya *Hata Ayıklama'yı* tıklatın ve *devam edin.* Tarayıcınıza dönün ve iletinin şimdi görüntülendiğini fark edin.

Hizmetinizi Kubernetes'te bir hata ayıklama ekli yken, arama yığını, yerel değişkenler ve özel durum bilgileri gibi hata ayıklama bilgilerine tam erişime sahipsiniz.

İmlecinizi 20.satıra `Controllers/HomeController.cs` koyup *F9*tuşuna basarak kesme noktasını kaldırın.

## <a name="clean-up-your-azure-resources"></a>Azure kaynaklarınızı temizleme

Azure portalındaki kaynak grubunuza gidin ve *kaynak grubunu sil'i*tıklatın. Alternatif olarak, az [aks silme](/cli/azure/aks#az-aks-delete) komutunu kullanabilirsiniz:

```azurecli
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Birden çok kapsayıcı ve takım geliştirme ile çalışma](multi-service-netcore-visualstudio.md)

[ingress-update]: how-dev-spaces-works-up.md#how-running-your-code-is-configured
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
