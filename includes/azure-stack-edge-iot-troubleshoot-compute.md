---
author: v-dalc
ms.service: databox
ms.author: alkohli
ms.topic: include
ms.date: 03/02/2021
ms.openlocfilehash: 57415ec76a3e8d9fc3c160b47668d3419ff6ea5c
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103622099"
---
İşlem ile ilgili hataları gidermek için IoT Edge Aracısı çalışma zamanı yanıtlarını kullanın. Olası yanıtların bir listesi aşağıda verilmiştir:

* 200-TAMAM
* 400-dağıtım yapılandırması hatalı biçimlendirilmiş veya geçersiz.
* 417-cihazda bir dağıtım yapılandırma kümesi yok.
* 412-dağıtım yapılandırmasındaki şema sürümü geçersiz.
* 406-IoT Edge cihaz çevrimdışı veya durum raporları göndermiyor.
* 500-IoT Edge çalışma zamanında bir hata oluştu.

Daha fazla bilgi için bkz. [IoT Edge Aracısı](../articles/iot-edge/iot-edge-runtime.md?preserve-view=true&view=iotedge-2018-06#iot-edge-agent).

Aşağıdaki hata, Azure Stack Edge Pro cihazınızdaki IoT Edge hizmetiyle ilgilidir.

### <a name="compute-modules-have-unknown-status-and-cant-be-used"></a>İşlem modüllerinin durumu bilinmiyor ve kullanılamaz

#### <a name="error-description"></a>Hata açıklaması

Cihazdaki tüm modüller bilinmeyen bir durum gösterir ve kullanılamaz. Bilinmeyen durum yeniden başlatma yoluyla devam ettirir.<!--Original Support ticket relates to trying to deploy a container app on a Hub. Based on the work item, I assume the error description should not be that specific, and that the error applies to Azure Stack Edge Devices, which is the focus of this troubleshooting.-->

#### <a name="suggested-solution"></a>Önerilen çözüm

IoT Edge hizmetini silip modülleri yeniden dağıtın. Daha fazla bilgi için bkz. [IoT Edge hizmeti 'Ni kaldırma](../articles/databox-online/azure-stack-edge-j-series-manage-compute.md#remove-iot-edge-service).


### <a name="modules-show-as-running-but-are-not-working"></a>Modüller çalışıyor olarak gösterilir ancak çalışmıyor

#### <a name="error-description"></a>Hata açıklaması

Modülün çalışma zamanı durumu çalışıyor olarak gösteriliyor, ancak beklenen sonuçlar görülmedi. 

Bu durum, çalışmayan ya da `edgehub` beklenen şekilde ileti yönlendirmediği modül yol yapılandırmasındaki bir sorundan dolayı olabilir. Günlükleri kontrol edebilirsiniz `edgehub` . IoT Hub hizmetine bağlanamamakta olan gibi hatalar olduğunu görürseniz, en yaygın nedenler bağlantı sorunlarıdır. Bağlantı sorunları, iletişim için IoT Hub hizmeti tarafından varsayılan bağlantı noktası olarak kullanılan AMPQ bağlantı noktasının engellenmesi veya Web ara sunucusu bu iletileri engellediği için olabilir.

#### <a name="suggested-solution"></a>Önerilen çözüm

Aşağıdaki adımları izleyin:
1. Hatayı gidermek için cihazınızın IoT Hub kaynağına gidin ve ardından Edge cihazınızı seçin. 
1. **Modül ayarla > çalışma zamanı ayarları**' na gidin. 
1. `Upstream protocol`Ortam değişkenini ekleyin ve değeri atayın `AMQPWS` . Bu durumda yapılandırılan iletiler 443 numaralı bağlantı noktası üzerinden WebSockets üzerinden gönderilir.

### <a name="modules-show-as-running-but-do-not-have-an-ip-assigned"></a>Modüller çalışıyor olarak gösterilir ancak atanmış bir IP 'si yoktur

#### <a name="error-description"></a>Hata açıklaması

Modülün çalışma zamanı durumu çalışıyor olarak gösterilir ancak Kapsayıcılı uygulamanın atanmış bir IP 'si yoktur. 

Bu durum, Kubernetes dış hizmet IP 'Leri için sağladınız IP 'lerin aralığının yeterli olmadığı durumdur. Dağıttığınız her kapsayıcının veya VM 'nin kapsamakta olduğundan emin olmak için bu aralığı genişletmeniz gerekir.

#### <a name="suggested-solution"></a>Önerilen çözüm

Cihazınızın yerel Web Kullanıcı arabiriminde aşağıdaki adımları uygulayın:
1. **İşlem** sayfasına gidin. İşlem ağını etkinleştirdiğiniz bağlantı noktasını seçin. 
1. **Kubernetes dış hizmet IP 'leri** için statik ve bitişik bir IP aralığı girin. Hizmet için 1 IP gerekir `edgehub` . Ayrıca, her bir IoT Edge modülü için ve dağıtabileceğiniz her VM için bir IP 'ye ihtiyacınız vardır. 
1. **Uygula**’yı seçin. Değiştirilen IP aralığı hemen geçerli olacaktır.

Daha fazla bilgi için bkz. [kapsayıcılar için dış hizmet IP 'Lerini değiştirme](../articles/databox-online/azure-stack-edge-j-series-manage-compute.md#change-external-service-ips-for-containers).