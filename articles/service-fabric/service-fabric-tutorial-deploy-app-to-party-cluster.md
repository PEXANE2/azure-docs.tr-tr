---
title: Azure'da bir kümeye Hizmet Kumaşı uygulaması dağıtma
description: Visual Studio'dan yeni oluşturulan Azure Hizmet Kumaşı kümesine varolan bir uygulamayı nasıl dağıtılayabilirsiniz öğrenin.
author: athinanthny
ms.topic: tutorial
ms.date: 07/22/2019
ms.author: mikhegn
ms.custom: mvc
ms.openlocfilehash: 9951610732cbb1c5884a7b7e830033f427db0ab1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75646016"
---
# <a name="tutorial-deploy-a-service-fabric-application-to-a-cluster-in-azure"></a>Öğretici: Azure’da bir Service Fabric uygulamasını kümeye dağıtma

Bu öğretici, bir dizinin ikinci bölümüdür. Azure Service Fabric uygulamasının Azure’da yeni kümeye nasıl dağıtılacağı gösterilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Bir küme oluşturun.
> * Visual Studio kullanarak uygulamayı uzak bir kümeye dağıtma.

Bu öğretici serisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Bir .NET Service Fabric uygulaması oluşturun.](service-fabric-tutorial-create-dotnet-app.md)
> * Uygulamayı uzak kümeye dağıtma.
> * [Bir ASP.NET Core ön uç hizmetine HTTPS uç noktası ekleme](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md).
> * [Azure Pipelines kullanarak CI/CD yapılandırın](service-fabric-tutorial-deploy-app-with-cicd-vsts.md).
> * [Uygulama için izleme ve tanılama ayarlayın.](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)oluşturun.
* [Visual Studio 2019'u yükleyin](https://www.visualstudio.com/)ve **Azure geliştirme** ve **ASP.NET ve web geliştirme** iş yüklerini yükleyin.
* [Service Fabric SDK'yı yükleyin](service-fabric-get-started.md).

> [!NOTE]
> Ücretsiz bir hesap, sanal bir makine oluşturmak için gereksinimleri karşılamayabilir. Bu, öğreticinin tamamlanmasını engeller. Ayrıca, iş dışı veya okul dışı bir hesap, kümeyle ilişkili anahtar kasasında sertifika oluştururken izin sorunlarıyla karşılaşabilir. Sertifika oluşturmayla ilgili bir hatayla karşılaşırsanız, bunun yerine kümeoluşturmak için Portal'ı kullanın. 

## <a name="download-the-voting-sample-application"></a>Voting örnek uygulamasını indirme

[Bu öğretici serisinin birinci kısmında](service-fabric-tutorial-create-dotnet-app.md) Voting örnek uygulamasını oluşturmadıysanız, indirebilirsiniz. Komut penceresinde, örnek uygulama deposunu yerel makinenize klonlamak için aşağıdaki kodu çalıştırın.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart 
```

Uygulamayı Visual Studio'da açın, yönetici olarak çalıştırın ve uygulamayı oluşturun.

## <a name="create-a-cluster"></a>Küme oluşturma

Uygulama hazır olduğuna göre, bir Hizmet Kumaşı kümesi oluşturun ve uygulamayı kümeye dağıtırsınız. [Service Fabric kümesi,](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-anywhere) mikro hizmetlerinizin dağıtıldığı ve yönetildiği ağa bağlı sanal veya fiziksel makineler kümesidir.

Bu öğreticide, Visual Studio IDE'de yeni bir üç düğüm test kümesi oluşturur ve uygulamayı bu kümeye yayımlarsınız. Üretim kümesi oluşturma hakkında bilgi için [bir küme öğreticisi oluştur ve yönet'](service-fabric-tutorial-create-vnet-and-windows-cluster.md) e bakın. Uygulamayı, [PowerShell](./scripts/service-fabric-powershell-create-secure-cluster-cert.md) veya [Azure CLI](./scripts/cli-create-cluster.md) komut dosyalarını kullanarak veya [Azure Kaynak Yöneticisi şablonundan](service-fabric-tutorial-create-vnet-and-windows-cluster.md)Azure [portalı](https://portal.azure.com)üzerinden daha önce oluşturduğunuz varolan bir kümeye de dağıtabilirsiniz.

> [!NOTE]
> Oylama uygulaması ve diğer birçok uygulama, hizmetler arasında iletişim kurmak için Service Fabric ters proxy'yi kullanır. Visual Studio'dan oluşturulan kümeler varsayılan olarak ters proxy'ye sahiptir. Varolan bir kümeye dağıtım yapıyorsunuzsa, Oylama uygulamasının çalışması için [kümedeki ters proxy'yi etkinleştirmeniz](service-fabric-reverseproxy-setup.md) gerekir.


### <a name="find-the-votingweb-service-endpoint"></a>VotingWeb hizmet uç noktasını bulun

Oylama uygulamasının ön uç web hizmeti belirli bir bağlantı noktasında (8080 [eğer bu öğretici serisinin bölüm bir](service-fabric-tutorial-create-dotnet-app.md)adımları takip dinliyor . Uygulama Azure'daki bir kümeye dağıtıldığında hem küme hem de uygulama bir Azure yük dengeleyicinin arkasında çalışır. Uygulama bağlantı noktası, bir kural kullanılarak Azure yük dengeleyicisinde açılmalıdır. Kural, gelen trafiği yük bakiyesi aracılığıyla web hizmetine gönderir. Bağlantı noktası **VotingWeb/PackageRoot/ServiceManifest.xml** dosyasının **Endpoint** öğesinde bulunur. 

```xml
<Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
```

Daha sonraki bir adımda gereken hizmet bitiş noktasını not alın.  Varolan bir kümeye dağıtım yapıyorsunuz, bu bağlantı noktasını bir yük dengeleme kuralı oluşturarak ve Azure yük bakiyesi üzerinde [PowerShell komut dosyası](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) kullanarak veya [Azure portalındaki](https://portal.azure.com)bu kümenin yük dengeleyicisi aracılığıyla sonda kullanarak açın.

### <a name="create-a-test-cluster-in-azure"></a>Azure'da test kümesi oluşturma
Çözüm Gezgini’nde **Oylama**’ya sağ tıklayın ve **Yayımla**’yı seçin.

**Bağlantı Bitiş Noktası'nda**Yeni **Küme Oluştur'u**seçin.  Varolan bir kümeye dağıtım yapıyorsunuz, küme bitiş noktasını listeden seçin.  Hizmet Oluştur Kumaş Kümesi iletişim kutusu açılır.

**Küme** sekmesinde, **Küme adını** girin (örneğin, "mytestcluster"), aboneliğinizi seçin, küme için bir bölge seçin (Güney Orta ABD gibi), küme düğümleri sayısını girin (bir test kümesi için üç düğüm öneririz) ve bir kaynak grubu girin (örneğin"mytestclustergroup"). **İleri**'ye tıklayın.

![Küme oluşturma](./media/service-fabric-tutorial-deploy-app-to-party-cluster/create-cluster.png)

**Sertifika** sekmesine, küme sertifikasının parola ve çıktı yolunu girin. PFX dosyası olarak kendi imzalı bir sertifika oluşturulur ve belirtilen çıktı yoluna kaydedilir.  Sertifika, hem düğümden düğüme hem de istemciden düğüme güvenlik için kullanılır.  Üretim kümeleri için kendi imzalı bir sertifika kullanmayın.  Bu sertifika, Visual Studio tarafından kümeyle kimlik doğrulamak ve uygulamaları dağıtmak için kullanılır. PFX'i bilgisayarınızın Geçerli Kullanıcı\My sertifika deposuna yüklemek için **Içe Aktar sertifikasını** seçin.  **İleri**'ye tıklayın.

![Küme oluşturma](./media/service-fabric-tutorial-deploy-app-to-party-cluster/certificate.png)

**VM Detay** sekmesine, küme yöneticisi hesabının **Kullanıcı adı** ve **Parolasını** girin.  Küme düğümleri için **Sanal makine görüntüsünü** ve her küme düğümü için Sanal makine **boyutunu** seçin.  **Gelişmiş** sekmesini tıklatın.

![Küme oluşturma](./media/service-fabric-tutorial-deploy-app-to-party-cluster/vm-detail.png)

**Bağlantı noktalarında,** önceki adımdan (örneğin, 8080) VotingWeb hizmeti bitiş noktasını girin.  Küme oluşturulduğunda, trafiği kümeye iletmek için bu uygulama bağlantı noktaları Azure yük dengeleyicisinde açılır.  Birkaç dakika süren küme oluşturmak için **Oluştur'u** tıklatın.

![Küme oluşturma](./media/service-fabric-tutorial-deploy-app-to-party-cluster/advanced.png)

## <a name="publish-the-application-to-the-cluster"></a>Uygulamayı kümeye yayımlama

Yeni küme hazır olduğunda, Oylama uygulamasını doğrudan Visual Studio'dan dağıtabilirsiniz.

Çözüm Gezgini’nde **Oylama**’ya sağ tıklayın ve **Yayımla**’yı seçin. **Yayımla** iletişim kutusu görüntülenir.

**Bağlantı Bitiş Noktası'nda,** önceki adımda oluşturduğunuz kümenin bitiş noktasını seçin.  Örneğin, "mytestcluster.southcentral.cloudapp.azure.com:19000". **Gelişmiş Bağlantı Parametreleri'ni**seçerseniz, sertifika bilgilerinin otomatik olarak doldurulması gerekir.  
![Service Fabric uygulamasını yayımlama](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)

**Yayımla**’yı seçin.

Uygulama dağıtıldıktan sonra bir tarayıcı açın ve ardından **:8080'in**ardından küme adresini girin. Veya başka bir bağlantı noktası yapılandırdıysanız onu girin. `http://mytestcluster.southcentral.cloudapp.azure.com:8080` bunun bir örneğidir. Artık Azure'da kümede çalıştırılan uygulamayı görüyor olmalısınız. Voting web sayfasında, oylama seçeneklerini ve bu seçeneklerden en az biri için oylama ekleyip silmeyi deneyin.

![Service Fabric Voting örneği](./media/service-fabric-tutorial-deploy-app-to-party-cluster/application-screenshot-new-azure.png)


## <a name="next-steps"></a>Sonraki adımlar
Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir küme oluşturun.
> * Visual Studio kullanarak uygulamayı uzak bir kümeye dağıtma.

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [HTTPS'yi etkinleştirme](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
