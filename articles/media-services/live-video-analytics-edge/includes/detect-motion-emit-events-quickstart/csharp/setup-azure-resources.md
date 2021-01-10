---
ms.openlocfilehash: 40d2f957ce115b43a1dcc138b86e05ec9cc47384
ms.sourcegitcommit: 31cfd3782a448068c0ff1105abe06035ee7b672a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2021
ms.locfileid: "98060624"
---
Bu öğreticide aşağıdaki Azure kaynakları gereklidir:

* IoT Hub
* Depolama hesabı
* Azure Media Services hesabı
* Azure 'da [IoT Edge Runtime](../../../../../iot-edge/how-to-install-iot-edge.md) yüklüyken Linux sanal makinesi

Bu hızlı başlangıç için, Azure aboneliğinizde gerekli kaynakları dağıtmak üzere [canlı video analizi kaynakları kurulum betiğini](https://github.com/Azure/live-video-analytics/tree/master/edge/setup) kullanmanızı öneririz. Bunu yapmak için aşağıdaki adımları izleyin:

1. [Azure Cloud Shell](https://shell.azure.com)’i açın.
1. İlk kez Cloud Shell kullanıyorsanız, bir depolama hesabı ve bir Microsoft Azure dosya paylaşımının oluşturulması için bir abonelik seçmeniz istenir. Cloud Shell oturum bilgileriniz için bir depolama hesabı oluşturmak üzere **depolama oluştur** ' u seçin. Bu depolama hesabı, komut dosyasının Azure Media Services hesabınızla kullanılmak üzere oluşturulacağı hesaptan ayrıdır.
1. Cloud Shell penceresinin sol tarafındaki açılan menüde, ortamınız olarak **Bash** ' i seçin.

    ![Ortam Seçicisi](../../../media/quickstarts/env-selector.png)
1. Aşağıdaki komutu çalıştırın.

    ```
    bash -c "$(curl -sL https://aka.ms/lva-edge/setup-resources-for-samples)"
    ```
    
    Betiği başarıyla tamamladıktan sonra aboneliğinizdeki tüm gerekli kaynakları görmeniz gerekir.
1. Betik tamamlandıktan sonra, klasör yapısını göstermek için süslü ayraçları seçin. *~/CloudDrive/LVA-Sample* dizininde birkaç dosya görürsünüz. Bu hızlı başlangıçta ilgilendiğiniz:

     * ***~/CloudDrive/LVA-Sample/Edge-Deployment/.exe**-bu dosya Visual Studio Code bir uç cihaza modül dağıtmak için kullandığı özellikleri içerir.
     _ ***~/CloudDrive/LVA-Sample/appsetting.json** _-Visual Studio Code, örnek kodu çalıştırmak için bu dosyayı kullanır.
     
    Bir sonraki bölümde Visual Studio Code ' de geliştirme ortamınızı ayarlarken bu dosyalar gerekir. Bunları şimdilik yerel bir dosyaya kopyalamak isteyebilirsiniz.
    
    ![Uygulama ayarları](../../../media/quickstarts/clouddrive.png)

> [!TIP]
> Oluşturulan Azure kaynaklarıyla ilgili sorunlar yaşıyorsanız, bazı sık karşılaşılan sorunları çözmek için lütfen _ *[sorun giderme kılavuzumuzu](../../../troubleshoot-how-to.md#common-error-resolutions)* görüntüleyin.