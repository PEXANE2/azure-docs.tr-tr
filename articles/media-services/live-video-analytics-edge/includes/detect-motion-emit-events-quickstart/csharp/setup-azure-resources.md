---
ms.openlocfilehash: 729839c8e881f507e103c4644c012d8dffc5c8c6
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98956385"
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
    
    Betiği başarıyla tamamladıktan sonra aboneliğinizdeki tüm gerekli kaynakları görmeniz gerekir. Komut dosyası tarafından toplam 12 kaynak oluşturulur:
    1. **Akış uç noktası** -bu, kayıtlı AMS varlığını yürütmeye yardımcı olur.
    1. **Sanal makine** -bu, Edge cihazınız olarak görev yapacak bir sanal makinedir.
    1. **Disk** -bu, medya ve yapıtları depolamak için sanal makineye bağlı bir depolama disktir.
    1. **Ağ güvenlik grubu** -bu, bir Azure sanal ağındaki Azure kaynaklarından gelen ve giden ağ trafiğini filtrelemek için kullanılır.
    1. **Ağ arabirimi** -bu, bir Azure sanal makinesinin Internet, Azure ve diğer kaynaklarla iletişim kurmasını sağlar.
    1. Savunma **bağlantısı** -bu, tarayıcınızı ve Azure Portal kullanarak sanal makinenize bağlanmanızı sağlar.
    1. **Genel IP adresi** -bu, Azure kaynaklarının Internet ve genel kullanıma yönelik Azure hizmetleriyle iletişim kurmasını sağlar
    1. **Sanal ağ** -bu, sanal makineniz gibi birçok Azure Kaynak türünün, internet ve şirket içi ağlarla güvenli bir şekilde iletişim kurmasını sağlar. [Sanal ağlar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) hakkında daha fazla bilgi
    1. **IoT Hub** -bu, IoT uygulamanız, IoT Edge modülleriniz ve yönettiği cihazlar arasında çift yönlü iletişim için bir merkezi ileti hub 'ı görevi görür.
    1. **Medya hizmeti hesabı** -bu, Azure 'da medya içeriğini yönetmeye ve akışa yardımcı olur.
    1. **Depolama hesabı** -bir birincil depolama hesabınız olmalıdır ve Media Services hesabınızla Ilişkili birkaç ikincil depolama hesabı olabilir. Daha fazla bilgi için bkz. [Azure Media Services hesapları Ile Azure depolama hesapları](https://docs.microsoft.com/azure/media-services/latest/storage-account-concept).
    1. **Kapsayıcı kayıt defteri** -bu, özel Docker kapsayıcı görüntülerinizi ve ilgili yapıtlarınızı depolamaya ve yönetmeye yardımcı olur.

1. Betik tamamlandıktan sonra, klasör yapısını göstermek için süslü ayraçları seçin. *~/CloudDrive/LVA-Sample* dizininde birkaç dosya görürsünüz. Bu hızlı başlangıçta ilgilendiğiniz:

     * ***~/CloudDrive/LVA-Sample/Edge-Deployment/.exe**-bu dosya Visual Studio Code bir uç cihaza modül dağıtmak için kullandığı özellikleri içerir.
     _ ***~/CloudDrive/LVA-Sample/appsetting.json** _-Visual Studio Code, örnek kodu çalıştırmak için bu dosyayı kullanır.
     
    Bir sonraki bölümde Visual Studio Code ' de geliştirme ortamınızı ayarlarken bu dosyalar gerekir. Bunları şimdilik yerel bir dosyaya kopyalamak isteyebilirsiniz.
    
    ![Uygulama ayarları](../../../media/quickstarts/clouddrive.png)

> [!TIP]
> Oluşturulan Azure kaynaklarıyla ilgili sorunlar yaşıyorsanız, bazı sık karşılaşılan sorunları çözmek için lütfen _ *[sorun giderme kılavuzumuzu](../../../troubleshoot-how-to.md#common-error-resolutions)* görüntüleyin.