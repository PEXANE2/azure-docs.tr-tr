---
title: Azure 'da bir Service Fabric uygulamasını kümeye dağıtma
description: Mevcut bir uygulamayı Visual Studio 'dan yeni oluşturulan bir Azure Service Fabric kümesine dağıtmayı öğrenin.
author: athinanthny
ms.topic: tutorial
ms.date: 07/22/2019
ms.author: mikhegn
ms.custom: mvc
ms.openlocfilehash: 9951610732cbb1c5884a7b7e830033f427db0ab1
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75646016"
---
# <a name="tutorial-deploy-a-service-fabric-application-to-a-cluster-in-azure"></a>Öğretici: Azure’da bir Service Fabric uygulamasını kümeye dağıtma

Bu öğretici, bir dizinin ikinci bölümüdür. Azure Service Fabric uygulamasının Azure’da yeni kümeye nasıl dağıtılacağı gösterilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Bir küme oluşturun.
> * Visual Studio kullanarak uygulamayı uzak bir kümeye dağıtma.

Bu öğretici serisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [.NET Service Fabric uygulaması oluşturma](service-fabric-tutorial-create-dotnet-app.md).
> * Uygulamayı uzak kümeye dağıtma.
> * [Bir ASP.NET Core ön uç hizmetine HTTPS uç noktası ekleme](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md).
> * [Azure Pipelines kullanarak CI/CD yapılandırın](service-fabric-tutorial-deploy-app-with-cicd-vsts.md).
> * [Uygulama için izleme ve tanılamayı ayarlama](service-fabric-tutorial-monitoring-aspnet.md).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* [Visual Studio 2019](https://www.visualstudio.com/)' i yükledikten sonra **Azure geliştirme** ve **ASP.net ve Web geliştirme** iş yüklerini yüklersiniz.
* [Service Fabric SDK'yı yükleyin](service-fabric-get-started.md).

> [!NOTE]
> Ücretsiz bir hesap, sanal makine oluşturma gereksinimlerini karşılamayabilir. Bu, öğreticinin tamamlanmasına engel olur. Ayrıca, iş dışı veya okul olmayan bir hesap, kümeyle ilişkili anahtar kasasında sertifika oluştururken izin sorunlarıyla karşılaşabilir. Sertifika oluşturma ile ilgili bir hatayla karşılaşırsanız, kümeyi oluşturmak için portalı kullanın. 

## <a name="download-the-voting-sample-application"></a>Voting örnek uygulamasını indirme

[Bu öğretici serisinin birinci kısmında](service-fabric-tutorial-create-dotnet-app.md) Voting örnek uygulamasını oluşturmadıysanız, indirebilirsiniz. Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki kodu çalıştırın.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart 
```

Uygulamayı Visual Studio 'da açın, yönetici olarak çalıştırın ve uygulamayı oluşturun.

## <a name="create-a-cluster"></a>Küme oluşturma

Artık uygulama hazır olduğuna göre, bir Service Fabric kümesi oluşturup uygulamayı kümeye dağıtırsınız. [Service Fabric kümesi](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-anywhere), mikro hizmetlerin dağıtılıp yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir.

Bu öğreticide, Visual Studio IDE 'de yeni üç düğümlü test kümesi oluşturur ve uygulamayı bu kümeye yayımlayabilirsiniz. Üretim kümesi oluşturma hakkında bilgi için bkz. [küme oluşturma ve yönetme öğreticisi](service-fabric-tutorial-create-vnet-and-windows-cluster.md) . Uygulamayı, [PowerShell](./scripts/service-fabric-powershell-create-secure-cluster-cert.md) veya [Azure CLI](./scripts/cli-create-cluster.md) betikleri kullanarak ya da bir [Azure Resource Manager şablonundan](service-fabric-tutorial-create-vnet-and-windows-cluster.md)daha önce [Azure Portal](https://portal.azure.com)aracılığıyla oluşturduğunuz var olan bir kümeye dağıtabilirsiniz.

> [!NOTE]
> Oylama uygulaması ve diğer birçok uygulama, hizmetler arasında iletişim kurmak için Service Fabric ters proxy 'yi kullanır. Visual Studio 'dan oluşturulan kümeler için ters proxy varsayılan olarak etkindir. Var olan bir kümeye dağıtım yapıyorsanız, oylama uygulamasının çalışması için [kümedeki ters proxy 'yi etkinleştirmeniz](service-fabric-reverseproxy-setup.md) gerekir.


### <a name="find-the-votingweb-service-endpoint"></a>VotingWeb hizmet uç noktasını bulun

Oylama uygulamasının ön uç Web hizmeti belirli bir bağlantı noktasını dinler (8080, [Bu öğretici serisinin birinci](service-fabric-tutorial-create-dotnet-app.md)kısmında yer alan adımları izlemektedir. Uygulama Azure'daki bir kümeye dağıtıldığında hem küme hem de uygulama bir Azure yük dengeleyicinin arkasında çalışır. Uygulama bağlantı noktası, bir kural kullanılarak Azure Yük dengeleyicisinde açılmalıdır. Kural, Web hizmetine yük dengeleyici aracılığıyla gelen trafiği gönderir. Bağlantı noktası **VotingWeb/PackageRoot/ServiceManifest.xml** dosyasının **Endpoint** öğesinde bulunur. 

```xml
<Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
```

Daha sonraki bir adımda gereken hizmet uç noktası ' nı bir yere göz atın.  Var olan bir kümeye dağıtım yapıyorsanız, bir [PowerShell betiği](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) veya [Azure Portal](https://portal.azure.com)bu kümenin yük dengeleyici aracılığıyla Azure Yük dengeleyicisinde bir yük dengeleme kuralı ve araştırması oluşturarak bu bağlantı noktasını açın.

### <a name="create-a-test-cluster-in-azure"></a>Azure 'da bir test kümesi oluşturma
Çözüm Gezgini’nde **Oylama**’ya sağ tıklayın ve **Yayımla**’yı seçin.

**Bağlantı uç noktası**' nda **Yeni küme oluştur**' u seçin.  Var olan bir kümeye dağıtım yapıyorsanız, listeden küme uç noktasını seçin.  Service Fabric kümesi oluştur iletişim kutusu açılır.

**Küme** sekmesinde, **küme adını** girin (örneğin, "mytestcluster"), aboneliğinizi seçin, küme için bir bölge seçin (örneğin, Orta Güney ABD), küme düğümlerinin sayısını girin (bir test kümesi için üç düğüm önerilir) ve bir kaynak grubu (örneğin, "mytestclustergroup") girin. **İleri**’ye tıklayın.

![Küme oluşturma](./media/service-fabric-tutorial-deploy-app-to-party-cluster/create-cluster.png)

**Sertifika** sekmesinde, küme sertifikası için parola ve çıkış yolunu girin. Otomatik olarak imzalanan bir sertifika PFX dosyası olarak oluşturulur ve belirtilen çıkış yoluna kaydedilir.  Sertifika, düğümden düğüme ve istemciden düğüme güvenlik için kullanılır.  Üretim kümeleri için kendinden imzalı bir sertifika kullanmayın.  Bu sertifika, Visual Studio tarafından, küme ve uygulama dağıtımı ile kimlik doğrulamak için kullanılır. PFX 'yi bilgisayarınızın Currentuser\certificate deposuna yüklemek için **sertifikayı Içeri aktar** ' ı seçin.  **İleri**’ye tıklayın.

![Küme oluşturma](./media/service-fabric-tutorial-deploy-app-to-party-cluster/certificate.png)

**VM ayrıntısı** sekmesinde, Küme Yöneticisi hesabı için **Kullanıcı adını** ve **parolayı** girin.  Küme düğümleri için **sanal makine görüntüsünü** ve her küme düğümünün **sanal makine boyutunu** seçin.  Tıklayın **Gelişmiş** sekmesi.

![Küme oluşturma](./media/service-fabric-tutorial-deploy-app-to-party-cluster/vm-detail.png)

**Bağlantı noktaları**' nda, önceki adımdan VotingWeb hizmeti uç noktasını girin (örneğin, 8080).  Küme oluşturulduğunda, trafiği kümeye iletmek için bu uygulama bağlantı noktaları Azure Yük dengeleyicide açılır.  Birkaç dakika süren kümeyi oluşturmak için **Oluştur** ' a tıklayın.

![Küme oluşturma](./media/service-fabric-tutorial-deploy-app-to-party-cluster/advanced.png)

## <a name="publish-the-application-to-the-cluster"></a>Uygulamayı kümede yayımlayın

Yeni küme hazırsanız, oylama uygulamasını doğrudan Visual Studio 'dan dağıtabilirsiniz.

Çözüm Gezgini’nde **Oylama**’ya sağ tıklayın ve **Yayımla**’yı seçin. **Yayımla** iletişim kutusu görüntülenir.

**Bağlantı uç noktası**' nda, önceki adımda oluşturduğunuz kümenin uç noktasını seçin.  Örneğin, "mytestcluster.southcentral.cloudapp.azure.com:19000". **Gelişmiş bağlantı parametreleri**' ni seçerseniz, sertifika bilgileri otomatik olarak doldurulmalıdır.  
![Service Fabric uygulaması yayımlama](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)

**Yayımla**’yı seçin.

Uygulama dağıtıldıktan sonra bir tarayıcı açın ve ardından küme adresini girin **: 8080**. Veya başka bir bağlantı noktası yapılandırdıysanız onu girin. `http://mytestcluster.southcentral.cloudapp.azure.com:8080` bunun bir örneğidir. Artık Azure'da kümede çalıştırılan uygulamayı görüyor olmalısınız. Voting web sayfasında, oylama seçeneklerini ve bu seçeneklerden en az biri için oylama ekleyip silmeyi deneyin.

![Service Fabric Voting örneği](./media/service-fabric-tutorial-deploy-app-to-party-cluster/application-screenshot-new-azure.png)


## <a name="next-steps"></a>Sonraki adımlar
Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir küme oluşturun.
> * Visual Studio kullanarak uygulamayı uzak bir kümeye dağıtma.

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [HTTPS'yi etkinleştirme](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
