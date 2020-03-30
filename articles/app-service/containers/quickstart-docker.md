---
title: 'Quickstart: Özel bir Linux kapsayıcısı çalıştırın'
description: Azure Konteyner Kayıt Defterleri'ni kullanarak ilk özel kapsayıcınızı dağıtarak Azure Uygulama Hizmeti'ndeki Linux kapsayıcılarıyla başlayın.
author: msangapu-msft
ms.author: msangapu
ms.date: 08/28/2019
ms.topic: quickstart
ms.openlocfilehash: 62e6b007e89fc6be726d3d971ca838770db9cb6e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75422143"
---
# <a name="deploy-a-custom-linux-container-to-azure-app-service"></a>Azure Uygulama Hizmetine özel bir Linux kapsayıcısı dağıtma

Linux'taki Uygulama Hizmeti, .NET, PHP, Node.js ve diğerleri gibi diller için destek sağlayan Linux'ta önceden tanımlanmış uygulama yığınları sağlar. Ayrıca web uygulamanızı Azure’da zaten tanımlı olmayan bir uygulama yığınında çalıştırmak için özel bir Docker görüntüsü de kullanabilirsiniz. Bu hızlı başlangıç, bir görüntüyü [Azure Kapsayıcı Kayıt Defteri'nden](/azure/container-registry) (ACR) Uygulama Hizmetine nasıl dağıtabileceğinizi gösterir.

## <a name="prerequisites"></a>Ön koşullar

* Azure [hesabı](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)
* [Docker](https://www.docker.com/community-edition)
* [Visual Studio Code](https://code.visualstudio.com/)
* [VS Kodu için Azure Uygulama Hizmeti uzantısı.](vscode:extension/ms-azuretools.vscode-azureappservice) Bu uzantıyı, Azure Platformu'nda (PaaS) Linux Web Apps'ı oluşturmak, yönetmek ve dağıtmak için kullanabilirsiniz.
* [VS Kodu için Docker uzantısı.](vscode:extension/ms-azuretools.vscode-docker) Bu uzantıyı, yerel Docker görüntülerinin ve komutlarının yönetimini basitleştirmek ve yerleşik uygulama görüntülerini Azure'a dağıtmak için kullanabilirsiniz.

## <a name="create-an-image"></a>Görüntü oluşturma

Bu hızlı başlangıcı tamamlamak için, bir [Azure Konteyner Kayıt Defteri'nde](/azure/container-registry)depolanan uygun bir web uygulaması görüntüsüne ihtiyacınız olacaktır. Quickstart'taki yönergeleri [izleyin: Azure portalını kullanarak özel bir kapsayıcı kayıt defteri oluşturun,](/azure/container-registry/container-registry-get-started-portal)ancak `mcr.microsoft.com/azuredocs/go` `hello-world` görüntü yerine resmi kullanın. Başvuru için, [örnek Dockerfile Azure Örnekleri repo bulunur.](https://github.com/Azure-Samples/go-docs-hello-world)

> [!IMPORTANT]
> Kapsayıcı kayıt defterini oluştururken **Yönetici Kullanıcı** seçeneğini **Etkinleştirmeye** ayarladığınızdan emin olun. Azure portalındaki kayıt defteri sayfanızın **Erişim tuşları** bölümünden de ayarlayabilirsiniz. Bu ayar, Uygulama Hizmeti erişimi için gereklidir.

## <a name="sign-in"></a>Oturum aç

Ardından, VS Kodu'nu başlatın ve Uygulama Hizmeti uzantısını kullanarak Azure hesabınızda oturum açın. Bunu yapmak için Etkinlik Çubuğu'ndaki Azure logosunu seçin, **APP SERVICE** explorer'a gidin, ardından **Azure'da Oturum Aç'ı** seçin ve yönergeleri izleyin.

![Azure'da oturum açma](./media/quickstart-docker/sign-in.png)

## <a name="check-prerequisites"></a>Önkoşulları denetleme

Artık tüm ön koşulların düzgün bir şekilde yüklenip yapılandırılmadığını kontrol edebilirsiniz.

VS Code'da, Durum Çubuğu'nda Azure e-posta adresinizi ve **APP SERVICE** explorer'da aboneliğinizi görmeniz gerekir.

Ardından, Docker'ın yüklü ve çalışır durumda olduğunu doğrulayın. Aşağıdaki komut, çalışıyorsa Docker sürümünü görüntüler.

```bash
docker --version
```

Son olarak, Azure Konteyner Kayıt Defterinizin bağlı olduğundan emin olun. Bunu yapmak için Etkinlik Çubuğu'ndaki Docker logosunu seçin ve ardından **REGISTRIES'e**gidin.

![Kayıt Defterleri](./media/quickstart-docker/registries.png)

## <a name="deploy-the-image-to-azure-app-service"></a>Görüntüyü Azure Uygulama Hizmetine dağıtma

Artık her şey yapılandırıldığına göre, resminizi doğrudan Docker uzantılı gezginden [Azure Uygulama Hizmeti'ne](https://azure.microsoft.com/services/app-service/) dağıtabilirsiniz.

**Docker** gezgininde **Kayıt Defteri** düğümünün altındaki resmi bulun ve etiketlerini göstermek için genişletin. Bir etiketi sağ tıklatın ve ardından **Azure Uygulama Hizmetine Resmi Dağıt'ı**seçin.

Buradan, bir abonelik, genel olarak benzersiz bir uygulama adı, Kaynak Grubu ve bir Uygulama Hizmet Planı seçmek için istemleri izleyin. Fiyatlandırma katmanı ve bölge için **B1 Basic'i** seçin.

Dağıtımdan sonra uygulamanız `http://<app name>.azurewebsites.net`' dan edinilebilir.

**Kaynak Grubu,** Uygulamanızın Azure'daki tüm kaynaklarının adlandırılmış bir koleksiyonudur. Örneğin, Bir Kaynak Grubu bir web sitesine, veritabanına ve Azure İşi'ne başvuruda bulunabilir.

**Uygulama Hizmet Planı,** web sitenizi barındırmak için kullanılacak fiziksel kaynakları tanımlar. Bu hızlı başlatma, **Linux** altyapısı nda **temel** barındırma planı kullanır, bu da sitenin diğer web sitelerinin yanı sıra bir Linux makinesinde barındırılan bir araç olacağı anlamına gelir. **Temel** planla başlarsanız, azure portalını kullanarak ölçeklendirebilirsiniz, böylece sizinki bir makinede çalışan tek sitedir.

## <a name="browse-the-website"></a>Web sitesine göz atın

**İşlemin** durumunu belirtmek için dağıtım sırasında Çıktı paneli açılır. İşlem tamamlandığında, **APP SERVICE** explorer'da oluşturduğunuz uygulamayı bulun, sağ tıklayın ve ardından tarayıcınızda siteyi açmak için Web **Sitesine Gözat'ı** seçin.

> [!div class="nextstepaction"]
> [Bir sorunla karşılaştım](https://www.research.net/r/PWZWZ52?tutorial=quickstart-docker&step=deploy-app)

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler, bu hızlı başlangıcı başarıyla tamamladınız!

Ardından, diğer Azure uzantılarına göz atın.

* [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
* [Azure İşlevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
* [Azure CLI Araçları](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
* [Azure Kaynak Yöneticisi Araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

Veya [Azure Araçları](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) uzantı paketini yükleyerek hepsini elde edin.
