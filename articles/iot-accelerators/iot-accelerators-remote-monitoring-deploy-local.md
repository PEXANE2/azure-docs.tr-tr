---
title: Uzaktan izleme çözümünü yerel olarak dağıtma - VS IDE - Azure | Microsoft Dokümanlar
description: Bu nasıl yapılılır kılavuzu, test ve geliştirme için Visual Studio'yu kullanarak uzaktan izleme çözüm hızlandırıcısını yerel makinenize nasıl dağıtabileceğinizi gösterir.
author: avneet723
manager: hegate
ms.author: avneets
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/17/2019
ms.topic: conceptual
ms.openlocfilehash: a1eba1fceb959bd475d205176c2c53f6409fdc77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73890882"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---visual-studio"></a>Uzaktan İzleme çözüm hızlandırıcısını yerel olarak dağıtın - Visual Studio

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

Bu makalede, uzaktan izleme çözüm hızlandırıcısını test ve geliştirme için yerel makinenize nasıl dağıtabileceğinizgösterilmektedir. Visual Studio'da mikro servisleri nasıl çalıştırabileceğinizi öğreniyorsunuz. Yerel bir mikro hizmetler dağıtımı aşağıdaki bulut hizmetlerini kullanır: Buluttaki IoT Hub, Cosmos DB, Azure Akış Analizi ve Azure Zaman Serisi Öngörüleri hizmetleri.

Docker'daki Uzaktan İzleme çözüm hızlandırıcısını yerel makinenizde çalıştırmak istiyorsanız, [uzaktan izleme çözüm hızlandırıcısını yerel olarak dağıtın - Docker](iot-accelerators-remote-monitoring-deploy-local-docker.md)' a bakın.

## <a name="prerequisites"></a>Ön koşullar

Uzaktan İzleme çözüm hızlandırıcısı tarafından kullanılan Azure hizmetlerini dağıtmak için etkin bir Azure aboneliğine ihtiyacınız var.

Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).

### <a name="machine-setup"></a>Makine kurulumu

Yerel dağıtımı tamamlamak için, yerel geliştirme makinenize aşağıdaki araçların yüklenmesi gerekir:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Visual Studio](https://visualstudio.microsoft.com/)
* [Nginx](https://nginx.org/en/download.html)
* [Node.js v8](https://nodejs.org/) - Bu yazılım, komut dosyalarının Azure kaynakları oluşturmak için kullandığı PCS CLI için bir ön koşuldur. Node.js v10 kullanmayın.

> [!NOTE]
> Visual Studio, Windows ve Mac için kullanılabilir.

[!INCLUDE [iot-accelerators-local-setup](../../includes/iot-accelerators-local-setup.md)]

## <a name="run-the-microservices"></a>Mikro hizmetleri çalıştırın

Bu bölümde, Uzaktan İzleme mikro hizmetlerini çalıştırın. Web UI'yi yerel olarak, Docker'daki Aygıt Simülasyonu hizmetini ve Visual Studio'daki mikro hizmetleri çalıştırın.

### <a name="run-the-device-simulation-service"></a>Aygıt simülasyon hizmetini çalıştırın

Önceki bölümde **start.cmd** komut dosyası tarafından ayarlanan ortam değişkenlerine erişebildiğinizden emin olmak için yeni bir komut istemi penceresi açın.

Cihaz simülasyon hizmeti için Docker konteynerini başlatmak için aşağıdaki komutu çalıştırın. Hizmet, uzaktan izleme çözümü için aygıtları simüle eder.

```cmd
<path_to_cloned_repository>\services\device-simulation\scripts\docker\run.cmd
```

### <a name="deploy-all-other-microservices-on-local-machine"></a>Diğer tüm mikro hizmetleri yerel makineye dağıtın

Aşağıdaki adımlar, Visual Studio'da Uzaktan İzleme mikro hizmetlerini nasıl çalıştırabileceğinizi gösterir:

1. Visual Studio’yu başlatın.
1. Deponun yerel kopyasındaki **hizmetler** klasöründeki **uzaktan izleme.sln** çözümünü açın.
1. **Çözüm Gezgini'nde,** çözüme ve **Özellikler'e**sağ tıklayın.
1. **Başlangıç Projesi> Ortak Özellikleri'ni**seçin.
1. **Birden Çok başlangıç projesi** seçin ve aşağıdaki projeler için **Eylem'i** **Başlat'a** ayarlayın:
    * WebService (asa-manager\WebService)
    * WebService (auth\WebService)
    * WebService (config\WebService)
    * WebService (aygıt-telemetri\WebService)
    * WebService (iothub-manager\WebService)
    * WebService (depolama-bağdaştırıcısı\WebService)
1. Seçimlerinizi kaydetmek için **Tamam'ı** tıklatın.
1. Web hizmetlerini yerel makinede oluşturmak ve çalıştırmak için **Hata Ayıklama > Hata Ayıklama'yı** tıklatın.

Her web hizmeti bir komut istemi ve web tarayıcısı penceresi açar. Komut isteminde, çalışan hizmetten çıktı görürsünüz ve tarayıcı penceresi durumu izlemenizi sağlar. Komut istemlerini veya web sayfalarını kapatmayın, bu eylem web hizmetini durdurur.

### <a name="start-the-stream-analytics-job"></a>Akış Analizi işini başlatın

Akış Analizi işini başlatmak için aşağıdaki adımları izleyin:

1. [Azure portalına](https://portal.azure.com) gidin.
1. Çözümünüz için oluşturulan **Kaynak grubuna** gidin. Kaynak grubunun **adı, start.cmd** komut dosyasını çalıştırdığınızda çözümünüz için seçtiğiniz addır.
1. Kaynaklar listesinde **Akış Analizi işini** tıklatın.
1. Akış Analizi iş **genel bakış** sayfasında **Başlat** düğmesini tıklatın. Ardından işe başlamak için **Başlat'ı** tıklatın.

### <a name="run-the-web-ui"></a>Web UI'yi çalıştırın

Bu adımda, web ui başlatın. **Start.cmd** komut dosyası tarafından ayarlanan ortam değişkenlerine erişebildiğinizden emin olmak için yeni bir komut istemi penceresi açın. Deponun yerel kopyasındaki **webui** klasörüne gidin ve aşağıdaki komutları çalıştırın:

```cmd
npm install
npm start
```

Başlangıç tamamlandığında tarayıcınız **\/http: /localhost:3000/dashboard**sayfasını görüntüler. Bu sayfadaki hatalar bekleniyor. Uygulamayı hatasız görüntülemek için aşağıdaki adımı tamamlayın.

### <a name="configure-and-run-nginx"></a>NGINX'i yapılandırma ve çalıştırma

Yerel makinenizde çalışan web uygulamasını ve mikro hizmetleri bağlamak için ters proxy sunucusu ayarlayın:

* Nginx\conf yükleme dizinine deponun yerel kopyasında **webui\scripts\localhost** klasöründen **nginx.conf** dosyasını kopyalayın. **nginx\conf**
* **Nginx**çalıştırın.

**Nginx**çalıştırma hakkında daha fazla bilgi için, [Windows için nginx](https://nginx.org/en/docs/windows.html)bakın.

### <a name="connect-to-the-dashboard"></a>Panoya bağlanma

Uzaktan İzleme çözüm panosuna erişmek için\/tarayıcınızda http: /localhost:9000 adresine gidin.

## <a name="clean-up"></a>Temizleme

Gereksiz ücretleri önlemek için, testinizi tamamladığınızda bulut hizmetlerini Azure aboneliğinizden kaldırın. Hizmetleri kaldırmak için Azure [portalına](https://ms.portal.azure.com) gidin ve **start.cmd** komut dosyasının oluşturduğu kaynak grubunu silin.

Ayrıca, Kaynak Kodu GitHub'dan klonladığınızda oluşturulan Uzaktan İzleme deposunun yerel kopyasını da silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Uzaktan İzleme çözümünü dağıttığınız için bir sonraki adım [çözüm panosunun yeteneklerini keşfetmektir.](quickstart-remote-monitoring-deploy.md)
