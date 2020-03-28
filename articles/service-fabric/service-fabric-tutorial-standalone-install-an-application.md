---
title: Tek başına kümeye uygulama yükleme
description: Bu öğreticide, bağımsız Service Fabric kümenize nasıl bir uygulama yükleyin ilerlersiniz.
author: dkkapur
ms.topic: tutorial
ms.date: 07/22/2019
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: 124fc6035c1d0ad504a34b7db227f0f4675f24f7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75613967"
---
# <a name="tutorial-deploy-an-application-on-your-service-fabric-standalone-cluster"></a>Öğretici: Service Fabric tek başına kümenize uygulama dağıtma

Service Fabric tek başına kümeleri, kendi ortamınızı seçme ve Service Fabric’in benimsediği "her işletim sistemi, her bulut" yaklaşımının bir parçası olarak bir küme oluşturma seçeneği sunar. Bu öğretici seride, AWS'de barındırılan bağımsız bir küme oluşturur ve içine bir uygulama dağıtırsınız.

Bu öğretici, bir serinin üçüncü bölümüdür.  Service Fabric bağımsız kümeleri, Service Fabric ile "herhangi bir işletim sistemi, herhangi bir bulut" yaklaşımımızın bir parçası olarak size kendi ortamınızı seçme ve bir küme oluşturma seçeneği sunar. Bu öğreticide, bu tek başına kümeyi barındırmak için gereken AWS altyapısını oluşturma işlemi gösterilmektedir.

Serinin üçüncü bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Örnek uygulamayı indirme
> * Kümeye dağıtma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* [Visual Studio 2019'u yükleyin](https://www.visualstudio.com/) ve **Azure geliştirme** ve ASP.NET ve web geliştirme iş yüklerini **yükleyin.**
* [Servis Kumaşı SDK'yı yükleyin](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a>Voting örnek uygulamasını indirme

Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

### <a name="deploy-the-app-to-the-service-fabric-cluster"></a>Uygulamayı Service Fabric kümesine dağıtma

Uygulama indirildikten sonra, doğrudan Visual Studio'dan bir kümeye dağıtabilirsiniz.

1. Visual Studio’yu açın

2. **Dosya** > **Aç'ı** Seçin

3. Git deposunu kopyaladığınız klasöre gidin ve Voting.sln dosyasını seçin

4. Çözüm Gezgini'ndeki `Voting` uygulama projesine sağ tıklayın ve **Yayımla'yı** seçin

5. **Bağlantı Uç Noktası** açılır listesini seçin ve kümenizdeki düğümlerden birinin genel DNS Adını girin.  Örneğin, `ec2-34-215-183-77.us-west-2.compute.amazonaws.com:19000`. Azure'da tam nitelikli bir alan adı (FQDN) otomatik olarak verilmez, ancak VM Genel Bakış sayfasında kolayca [ayarlanabilirsiniz.](https://docs.microsoft.com/azure/virtual-machines/linux/portal-create-fqdn)

6. Tercih ettiğiniz tarayıcıyı açın ve küme adresini girin (bu uygulamanın 8080 numaralı bağlantı noktasında dağıttığı bağlantı uç noktası - örneğin, ec2-34-215-183-77.us-west-2.compute.amazonaws.com:8080).

    ![Kümeden API Yanıtı](./media/service-fabric-tutorial-standalone-cluster/deployed-app.png)

## <a name="next-steps"></a>Sonraki adımlar

Serinin üçüncü bölümünde, kümenize bir uygulamanın nasıl dağıtılacağı öğrendiniz:

> [!div class="checklist"]
> * Örnek uygulamayı indirme
> * Kümeye dağıtma

Kümenizi temizlemek için serinin dördüncü bölümüne ilerleyin.

> [!div class="nextstepaction"]
> [Kaynaklarınızı temizleme](service-fabric-tutorial-standalone-clean-up.md)