---
title: Azure Stack Edge GPU üzerinde Kubernetes durum bilgisiz uygulamasını IoT Edge modül aracılığıyla dağıtma | Microsoft Docs
description: Bir Kubernetes durum bilgisiz uygulamasının, bir dış IP aracılığıyla erişilen bir IoT Edge modülünü kullanarak Azure Stack Edge GPU cihazınızda nasıl dağıtılacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/26/2020
ms.author: alkohli
ms.openlocfilehash: 15316cbdd44053bfaf7403815ba42d92e2264b7b
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89254177"
---
# <a name="use-iot-edge-module-to-run-a-kubernetes-stateless-application-on-your-azure-stack-edge-gpu-device"></a>Azure Stack Edge GPU cihazınızda bir Kubernetes durum bilgisiz uygulaması çalıştırmak için IoT Edge modülünü kullanın

Bu makalede, Azure Stack Edge cihazınızda durum bilgisiz bir uygulamayı dağıtmak için bir IoT Edge modülünü nasıl kullanabileceğiniz açıklanır.

Durum bilgisiz uygulamayı dağıtmak için aşağıdaki adımları uygulayın:

- IoT Edge modülünü dağıtmadan önce önkoşulların tamamlandığından emin olun.
- Azure Stack Kenarunuzdaki işlem ağına erişmek için bir IoT Edge modülü ekleyin.
- Modülün etkin ağ arabirimine erişip erişebildiğini doğrulayın.

Bu nasıl yapılır makalesinde, senaryoyu göstermek için bir Web sunucusu uygulama modülünü kullanacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yapmanız gerekir:

- Azure Stack Edge cihazı. Şunlardan emin olun:

    - İşlem ağ ayarları cihazda yapılandırılır.
    - [Öğretici: cihazınızı etkinleştirme](azure-stack-edge-gpu-deploy-activate.md)adımları uyarınca cihaz etkinleştirilir.
- Öğreticiye göre **işlem adımını yapılandırmayı** tamamladınız: cihazınızdaki [Azure Stack Edge cihazında işlem yapılandırma](azure-stack-edge-gpu-deploy-configure-compute.md) . Cihazınızın ilişkili bir IoT Hub kaynağına, IoT cihazına ve bir IoT Edge cihazına sahip olması gerekir.


## <a name="add-webserver-app-module"></a>Web sunucusu uygulama modülünü Ekle

Azure Stack Edge cihazınıza bir Web sunucusu uygulama modülü eklemek için aşağıdaki adımları uygulayın.

1. Aygıtınızla ilişkili IoT Hub kaynağında, **otomatik cihaz yönetimi > IoT Edge**' ne gidin.
1. Azure Stack Edge cihazlarınız ile ilişkili IoT Edge cihazı seçin ve tıklatın. 

    ![IoT Edge cihaz seçin](media/azure-stack-edge-gpu-deploy-stateless-application-iot-edge-module/select-iot-edge-device-1.png)  

1. **Modül ayarla**' yı seçin. **Cihazdaki modülleri ayarla**sayfasında **+ Ekle** ' yi seçin ve ardından **IoT Edge modülü**' nü seçin.

    ![IoT Edge modülünü seçin](media/azure-stack-edge-gpu-deploy-stateless-application-iot-edge-module/select-iot-edge-module-1.png)

1. **Add IoT Edge modülünde**:

    1. Web sunucusu uygulaması modülünüzün dağıtmak istediğiniz bir **ad** belirtin.
    2. **Modül ayarları** sekmesinde, modül görüntünüz Için bir **görüntü URI 'si** sağlayın. Belirtilen ad ve etiketlerle eşleşen bir modül alındı. Bu durumda, `nginx:stable` genel [Docker deposundan](https://hub.docker.com/_/nginx/)kararlı bir NGINX görüntüsü (kalıcı olarak etiketlenir) çeker.

        ![IoT Edge modülü Ekle](media/azure-stack-edge-gpu-deploy-stateless-application-iot-edge-module/set-module-settings-1.png)    

    3. **Kapsayıcı oluşturma seçenekleri** sekmesinde aşağıdaki örnek kodu yapıştırın:  

        ```
        {
            "HostConfig": {
                "PortBindings": {
                    "80/tcp": [
                        {
                            "HostPort": "8080"
                        }
                    ]
                }
            }
        }
        ```

        Bu yapılandırma, TCP bağlantı noktası 8080 ' de (varsayılan Web sunucusu bağlantı noktası 80 olan) *http* üzerinden Işlem ağı IP 'sini kullanarak modüle erişmenizi sağlar. **Add (Ekle)** seçeneğini belirleyin.

        ![IoT Edge özel modül dikey penceresinde bağlantı noktası bilgilerini belirtin](media/azure-stack-edge-gpu-deploy-stateless-application-iot-edge-module/verify-module-status-1.png)

    4. **Gözden geçir + oluştur**’u seçin. Modül ayrıntılarını gözden geçirin ve **Oluştur**' u seçin.

## <a name="verify-module-access"></a>Modül erişimini doğrula

1. Modülün başarıyla dağıtıldığını ve çalıştığını doğrulayın. **Modüller** sekmesinde, modülün çalışma zamanı durumu **çalışıyor**olmalıdır.  

    ![Modül durumunun çalıştığını doğrulama](media/azure-stack-edge-gpu-deploy-stateless-application-iot-edge-module/verify-module-status-1.png)

1. Web sunucusu uygulamasının dış uç noktasını almak için [Kubernetes panosuna erişin](azure-stack-edge-gpu-monitor-kubernetes-dashboard.md#access-dashboard). 
1. Panonun sol bölmesinde **iotedge** ad alanına göre filtreleyin. **Bulma ve yük dengeleme > Hizmetleri '** ne gidin. Listelenen hizmet listesi altında, Web sunucusu uygulaması modülünün dış uç noktasını bulun. 

    ![Dış uç noktada Web sunucusu uygulamasına bağlanma](media/azure-stack-edge-gpu-deploy-stateless-application-iot-edge-module/connect-external-endpoint-1.png)

1. Yeni bir tarayıcı penceresi açmak için dış uç noktayı seçin.

    Web sunucusu uygulamasının çalıştığını görmeniz gerekir.

    ![Belirtilen bağlantı noktası üzerinden modülle bağlantıyı doğrula](media/azure-stack-edge-gpu-deploy-stateless-application-iot-edge-module/verify-webserver-app-1.png)

## <a name="next-steps"></a>Sonraki adımlar

- IoT Edge bir modül aracılığıyla durum bilgisi olan uygulamayı kullanıma sunma hakkında bilgi edinin<!--insert link-->.
