---
title: Modüllere erişmek için Azure Veri Kutusu Kenarı'ndaki bilgi işlem ağını yönetin| Microsoft Dokümanlar
description: Veri Kutusu Kenarınızdaki bilgi işlem ağını harici bir IP üzerinden modüllere erişmek için nasıl genişletiriz açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 05/17/2019
ms.author: alkohli
ms.openlocfilehash: 907647725dd6795b3b6482476de7442fbbf66114
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65917243"
---
# <a name="enable-compute-network-on-your-azure-data-box-edge"></a>Azure Veri Kutusu Kenarı'nda bilgi işlem ağını etkinleştirme

Bu makalede, Azure Veri Kutusu Kenarı'nda çalışan modüllerin aygıtta etkinleştirilmiş bilgi işlem ağına nasıl erişebileceği açıklanmaktadır.

Ağı yapılandırmak için aşağıdaki adımları atarsınız:

- İşlem yapmak için Veri Kutusu Kenarı cihazınızda bir ağ arabirimi etkinleştirme
- Veri Kutusu Kenarı'nda bilgi işlem ağına erişmek için bir modül ekleme
- Modülün etkin ağ arabirimine erişebileceğini doğrulayın

Bu eğitimde, senaryoyu göstermek için bir webserver uygulaması modülü kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yapmanız gerekir:

- Aygıt kurulumu tamamlanmış bir Veri Kutusu Kenarı aygıtı.
- Öğreticiye göre **yapılandırma işlem** adımını tamamladınız: Cihazınızda Azure Veri Kutusu Kenarı ile verileri [dönüştürün.](data-box-edge-deploy-configure-compute-advanced.md#configure-compute) Cihazınızda ilişkili bir IoT Hub kaynağı, bir IoT aygıtı ve bir IoT Edge aygıtı olmalıdır.

## <a name="enable-network-interface-for-compute"></a>İşlem için ağ arabirimini etkinleştirme

Harici bir ağ üzerinden cihazınızda çalışan modüllere erişmek için cihazınızdaki bir ağ arabirimine bir IP adresi atamanız gerekir. Bu bilgi işlem ayarlarını yerel web web arama biriminizden yönetebilirsiniz.

Bilgi işlem ayarlarını yapılandırmak için yerel web UI'nizde aşağıdaki adımları izleyin.

1. Yerel web ui'sinde Configuration **> Compute ayarlarına**gidin.  

2. Aygıtta çalıştıracağınız bir bilgi işlem modülüne bağlanmak için kullanmak istediğiniz ağ arabirimini **etkinleştirin.**

    - Statik IP adreslerini kullanıyorsanız, ağ arabirimi için bir IP adresi girin.
    - DHCP kullanıyorsanız, IP adresleri otomatik olarak atanır. Bu örnekte DHCP kullanır.

    ![İşlem ayarlarını etkinleştirme 1](media/data-box-edge-extend-compute-access-modules/enable-compute-setting-1.png)

3. Ayarları uygulamak için **Uygula'yı** seçin. DHCP kullanıyorsanız ağ arabirimine atanan IP adresini not alın.

    ![İşlem ayarlarını etkinleştirme](media/data-box-edge-extend-compute-access-modules/enable-compute-setting-2.png)

## <a name="add-webserver-app-module"></a>Webserver uygulama modülü ekleme

Veri Kutusu Edge cihazınıza bir websunucusu uygulaması modülü eklemek için aşağıdaki adımları izleyin.

1. Veri Kutusu Edge aygıtınızla ilişkili IoT Hub kaynağına gidin ve ardından **IoT Edge aygıtını**seçin.
2. Veri Kutusu Kenarı aygıtınızla ilişkili IoT Edge aygıtını seçin. Aygıt **ayrıntılarında,** **modülleri ayarla'yı**seçin. **Modül ekle'de**, **+ Ekle'yi** seçin ve ardından **IoT Edge Modülünü**seçin.
3. **IoT Edge özel modülleri** bıçak:

    1. Dağıtmak istediğiniz web sunucusu uygulama modülünüz için bir **Ad** belirtin.
    2. Modül görüntünüz için bir **Image URI** sağlayın. Sağlanan ad ve etiketlerle eşleşen bir modül alınır. Bu durumda, `nginx:stable` kamu [Docker deposundan](https://hub.docker.com/_/nginx/)istikrarlı bir nginx görüntü (kararlı olarak etiketlenmiş) çekecektir.
    3. Kapsayıcı **Oluşturma**Seçenekleri'nde, aşağıdaki örnek kodu yapıştırın:  

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

        Bu yapılandırma, TCP bağlantı noktası 8080'deki *http* üzerinden bilgi işlem ağı IP'sini kullanarak modüle erişmenizi sağlar (varsayılan web sunucusu bağlantı noktası 80'dir).

        ![IoT Edge özel modül bıçağında bağlantı noktası bilgilerini belirtin](media/data-box-edge-extend-compute-access-modules/module-information.png)

    4. **Kaydet'i**seçin.

## <a name="verify-module-access"></a>Modül erişimini doğrulama

1. Modülün başarıyla dağıtılmış ve çalıştığını doğrulayın. Aygıt **Ayrıntıları** sayfasında, **Modüller** sekmesinde, modülün çalışma süresi durumu **çalışıyor**olmalıdır.  
2. Web sunucusu uygulaması modülüne bağlanın. Tarayıcı penceresi açın ve yazın:

    `http://<compute-network-IP-address>:8080`

    Web sunucusu uygulamasının çalıştığını görmeniz gerekir.

    ![Modüle bağlantıyı belirtilen bağlantı noktası üzerinden doğrulama](media/data-box-edge-extend-compute-access-modules/verify-connect-module-1.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal aracılığıyla kullanıcıları yönetme](data-box-edge-manage-users.md) hakkında bilgi edinin.
