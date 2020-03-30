---
title: Uzaktan İzleme çözümünü yerel olarak dağıtın - Visual Studio Code - Azure | Microsoft Dokümanlar
description: Bu nasıl yapılılır kılavuzu, test ve geliştirme için Visual Studio Code'u kullanarak uzaktan izleme çözüm hızlandırıcısını yerel makinenize nasıl dağıtabileceğinizi gösterir.
author: avneet723
manager: hegate
ms.author: avneets
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/17/2019
ms.topic: conceptual
ms.openlocfilehash: 8f1d20e9a6a78d99a23fe4b98aeb4f3eb8359da7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73890950"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---visual-studio-code"></a>Uzaktan İzleme çözüm hızlandırıcısını yerel olarak dağıtın - Visual Studio Code

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

Bu makalede, uzaktan izleme çözüm hızlandırıcısını test ve geliştirme için yerel makinenize nasıl dağıtabileceğinizgösterilmektedir. Visual Studio Code'da mikro servisleri nasıl çalıştırabileceğinizi öğrenirsiniz. Yerel bir mikro hizmetler dağıtımı aşağıdaki bulut hizmetlerini kullanır: IoT Hub, Cosmos DB, Azure Akış Analitiği ve Azure Zaman Serisi Öngörüleri.

## <a name="prerequisites"></a>Ön koşullar

Uzaktan İzleme çözüm hızlandırıcısı tarafından kullanılan Azure hizmetlerini dağıtmak için etkin bir Azure aboneliğine ihtiyacınız var.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).

### <a name="machine-setup"></a>Makine kurulumu

Yerel dağıtımı tamamlamak için, yerel geliştirme makinenize aşağıdaki araçların yüklenmesi gerekir:

* [Git](https://git-scm.com/)
* [.NET Core](https://dotnet.microsoft.com/download)
* [Docker](https://www.docker.com)
* [Nginx](https://nginx.org/en/download.html)
* [Visual Studio Code](https://code.visualstudio.com/)
* [VS Code'un C# uzantısı](https://code.visualstudio.com/docs/languages/csharp)
* [Node.js v8](https://nodejs.org/) - Bu yazılım, komut dosyalarının Azure kaynakları oluşturmak için kullandığı PCS CLI için bir ön koşuldur. Node.js v10 kullanmayın

> [!NOTE]
> Windows, Mac ve Ubuntu için Visual Studio Code kullanılabilir.

[!INCLUDE [iot-accelerators-local-setup](../../includes/iot-accelerators-local-setup.md)]

## <a name="run-the-microservices"></a>Mikro hizmetleri çalıştırın

Bu bölümde, Uzaktan İzleme mikro hizmetlerini çalıştırın. Web UI'yi yerel olarak, Docker'daki Aygıt Simülasyonu hizmetini ve Visual Studio Code'daki mikro hizmetleri çalıştırın.

### <a name="build-the-code"></a>Kodu oluşturma

Komut isteminde azure-iot-pcs-remote-monitoring-dotnet\services adresine gidin ve kodu oluşturmak için aşağıdaki komutları çalıştırın.

```cmd
dotnet restore
dotnet build -c Release
```

### <a name="deploy-all-other-microservices-on-local-machine"></a>Diğer tüm mikro hizmetleri yerel makineye dağıtın

Aşağıdaki adımlar, Visual Studio Code'ta Uzaktan İzleme mikro hizmetlerini nasıl çalıştırabileceğinizi gösterir:

1. Visual Studio Code'u başlatın.
1. VS Code'da **azure-iot-pcs-remote-monitoring-dotnet** klasörünü açın.
1. **Azure-iot-pcs-remote-monitoring-dotnet** klasöründe **.vscode** adında yeni bir klasör oluşturun.
1. Files **launch.json** ve **tasks.json** from services\scripts\local\launch\idesettings\vscode'u yeni oluşturduğunuz **.vscode** klasörüne kopyalayın.
1. Hata **Ayıklama panelini** VS Code'da açın ve **Tüm mikro hizmetleri çalıştırın** yapılandırmasını çalıştırın. Bu yapılandırma, Docker'daki aygıt simülasyonu microservice'i çalıştırAr ve hata ayıklamadaki diğer mikro hizmetleri çalıştırAr.

Hata Ayıklama Konsolundaki **Run All microsoervices** çalıştıran çıktı aşağıdaki gibi görünür:

[![Deploy-Local-Microservices](./media/deploy-locally-vscode/auth-debug-results-inline.png)](./media/deploy-locally-vscode/auth-debug-results-expanded.png#lightbox)

### <a name="run-the-web-ui"></a>Web UI'yi çalıştırın

Bu adımda, web ui başlatın. Yerel kopyanızda **azure-iot-pcs-remote-monitoring-dotnet\webui** klasörüne gidin ve aşağıdaki komutları çalıştırın:

```cmd
npm install
npm start
```

Başlangıç tamamlandığında tarayıcınız **\/http: /localhost:3000/dashboard**sayfasını görüntüler. Bu sayfadaki hatalar bekleniyor. Uygulamayı hatasız görüntülemek için aşağıdaki adımı tamamlayın.

### <a name="configure-and-run-nginx"></a>NGINX'i yapılandırma ve çalıştırma

Yerel makinenizde çalışan web uygulamasını ve mikro hizmetleri bağlamak için ters proxy sunucusu ayarlayın:

* **Nginx.conf** dosyasını **webui\scripts\localhost** klasöründen **nginx\conf** yükleme dizinine kopyalayın.
* **Nginx**çalıştırın.

**Nginx**çalıştırma hakkında daha fazla bilgi için, [Windows için nginx](https://nginx.org/en/docs/windows.html)bakın.

### <a name="connect-to-the-dashboard"></a>Panoya bağlanma

Uzaktan İzleme çözüm panosuna erişmek için\/tarayıcınızda http: /localhost:9000 adresine gidin.

## <a name="clean-up"></a>Temizleme

Gereksiz ücretleri önlemek için, testinizi tamamladığınızda bulut hizmetlerini Azure aboneliğinizden kaldırın. Hizmetleri kaldırmak için Azure [portalına](https://ms.portal.azure.com) gidin ve **start.cmd** komut dosyasının oluşturduğu kaynak grubunu silin.

Ayrıca, Kaynak Kodu GitHub'dan klonladığınızda oluşturulan Uzaktan İzleme deposunun yerel kopyasını da silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Uzaktan İzleme çözümünü dağıttığınız için bir sonraki adım [çözüm panosunun yeteneklerini keşfetmektir.](quickstart-remote-monitoring-deploy.md)
