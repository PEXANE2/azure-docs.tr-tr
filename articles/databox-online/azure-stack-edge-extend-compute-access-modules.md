---
title: Modül erişimi için Azure Stack Edge 'de işlem ağını yönetme | Microsoft Docs
description: Bir dış IP aracılığıyla modüllere erişmek için Azure Stack kenarınızdan işlem ağının nasıl genişletileceğini açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 05/17/2019
ms.author: alkohli
ms.openlocfilehash: 01b0eae7d2e5d1078c0761838081a2379a724a97
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84342995"
---
# <a name="enable-compute-network-on-your-azure-stack-edge"></a>Azure Stack Kenarunuzda işlem ağını etkinleştirin

Bu makalede, Azure Stack Edge üzerinde çalışan modüllerin cihazda etkin olan işlem ağına nasıl erişebileceği açıklanmaktadır.

Ağı yapılandırmak için aşağıdaki adımları uygulayın:

- İşlem için Azure Stack Edge cihazınızda bir ağ arabirimini etkinleştirme
- Azure Stack Edge 'de işlem ağına erişmek için bir modül ekleyin
- Modülün etkin ağ arabirimine erişebileceğini doğrulayın

Bu öğreticide, senaryoyu göstermek için bir Web sunucusu uygulama modülünü kullanacaksınız.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yapmanız gerekir:

- Cihaz kurulumuna sahip bir Azure Stack Edge cihazı tamamlandı.
- Öğreticiye göre **işlem adımını yapılandırmayı** tamamladınız: cihazınızdaki [Azure Stack Edge ile veri dönüştürme](azure-stack-edge-deploy-configure-compute-advanced.md#configure-compute) . Cihazınızın ilişkili bir IoT Hub kaynağına, IoT cihazına ve bir IoT Edge cihazına sahip olması gerekir.

## <a name="enable-network-interface-for-compute"></a>İşlem için ağ arabirimini etkinleştir

Cihazınızda çalışan modüllerle bir dış ağ aracılığıyla erişmek için, cihazınızdaki bir ağ arabirimine bir IP adresi atamanız gerekir. Bu işlem ayarlarını, yerel Web kullanıcı arabiriminden yönetebilirsiniz.

İşlem ayarlarını yapılandırmak için yerel Web Kullanıcı arabiriminde aşağıdaki adımları uygulayın.

1. Yerel Web Kullanıcı arabiriminde **yapılandırma > işlem ayarları**' na gidin.  

2. Cihazda çalıştıracağınız bir işlem modülüne bağlanmak için kullanmak istediğiniz ağ arabirimini **etkinleştirin** .

    - Statik IP adresleri kullanılıyorsa, ağ arabirimi için bir IP adresi girin.
    - DHCP kullanılıyorsa, IP adresleri otomatik olarak atanır. Bu örnek DHCP kullanır.

    ![İşlem ayarlarını etkinleştir 1](media/azure-stack-edge-extend-compute-access-modules/enable-compute-setting-1.png)

3. Ayarları uygulamak için **Uygula** ' yı seçin. DHCP kullanılıyorsa ağ arabirimine atanan IP adresini bir yere unutmayın.

    ![İşlem ayarlarını etkinleştir](media/azure-stack-edge-extend-compute-access-modules/enable-compute-setting-2.png)

## <a name="add-webserver-app-module"></a>Web sunucusu uygulama modülünü Ekle

Azure Stack Edge cihazınıza bir Web sunucusu uygulama modülü eklemek için aşağıdaki adımları uygulayın.

1. Azure Stack Edge aygıtınızla ilişkili IoT Hub kaynağına gidin ve **IoT Edge cihaz**' ı seçin.
2. Azure Stack Edge cihazlarınızın ilişkilendirildiği IoT Edge cihazını seçin. **Cihaz ayrıntıları**' nın **Modül ayarla**' yı seçin. **Modül Ekle**' de **+ Ekle** ' yi seçin ve ardından **IoT Edge modülü**' nü seçin.
3. **IoT Edge özel modüller** dikey penceresinde:

    1. Web sunucusu uygulaması modülünüzün dağıtmak istediğiniz bir **ad** belirtin.
    2. Modül görüntünüz için bir **görüntü URI 'si** sağlayın. Belirtilen ad ve etiketlerle eşleşen bir modül alındı. Bu durumda, `nginx:stable` genel [Docker deposundan](https://hub.docker.com/_/nginx/)kararlı bir NGINX görüntüsü (kalıcı olarak etiketlenir) çeker.
    3. **Kapsayıcı oluşturma seçeneklerinde**aşağıdaki örnek kodu yapıştırın:  

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

        Bu yapılandırma, TCP bağlantı noktası 8080 ' de (varsayılan Web sunucusu bağlantı noktası 80 olan) *http* üzerinden Işlem ağı IP 'sini kullanarak modüle erişmenizi sağlar.

        ![IoT Edge özel modül dikey penceresinde bağlantı noktası bilgilerini belirtin](media/azure-stack-edge-extend-compute-access-modules/module-information.png)

    4. **Kaydet**'i seçin.

## <a name="verify-module-access"></a>Modül erişimini doğrula

1. Modülün başarıyla dağıtıldığını ve çalıştığını doğrulayın. **Cihaz ayrıntıları** sayfasındaki **modüller** sekmesinde, modülün çalışma zamanı durumu **çalışıyor**olmalıdır.  
2. Web sunucusu uygulama modülüne bağlanın. Bir tarayıcı penceresi açın ve şunu yazın:

    `http://<compute-network-IP-address>:8080`

    Web sunucusu uygulamasının çalıştığını görmeniz gerekir.

    ![Belirtilen bağlantı noktası üzerinden modülle bağlantıyı doğrula](media/azure-stack-edge-extend-compute-access-modules/verify-connect-module-1.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal aracılığıyla kullanıcıları yönetme](azure-stack-edge-manage-users.md) hakkında bilgi edinin.
