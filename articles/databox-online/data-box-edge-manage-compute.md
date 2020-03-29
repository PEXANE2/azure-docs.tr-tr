---
title: Azure Veri Kutusu Kenarı bilgi işlem yönetimi | Microsoft Dokümanlar
description: Tetikleyici, modüller, görünüm bilgi işlem yapılandırması gibi Edge bilgi işlem ayarlarını nasıl yönetiştireceklerini, Azure Veri Kutusu Kenarı'nızdaki Azure portalı üzerinden yapılandırmayı nasıl kaldırabilirsiniz' açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 05/20/2019
ms.author: alkohli
ms.openlocfilehash: a9daf1d59b03d283be999aaab559c6d60f6405dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65953129"
---
# <a name="manage-compute-on-your-azure-data-box-edge"></a>Azure Veri Kutusu Kenarınızda bilgi işlem yönetme

Bu makalede, Azure Veri Kutusu Kenarınızdaki bilgi işlem nasıl yönetilmeye açıklanmaktadır. İşlemi Azure portalı üzerinden veya yerel web web ui üzerinden yönetebilirsiniz. Modülleri, tetikleyicileri ve bilgi işlem yapılandırmasını yönetmek için Azure portalını ve bilgi işlem ayarlarını yönetmek için yerel web ui'sini kullanın.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Tetikleyicileri yönetme
> * İşlem yapılandırması yönetme


## <a name="manage-triggers"></a>Tetikleyicileri yönetme

Olaylar, bulut ortamınızda veya cihazınızda eylemde bulunmanızı isteyebileceğin şeylerdir. Örneğin, bir dosya bir paylaşımda oluşturulduğunda, bu bir olaydır. Tetikleyiciler olayları yükseltir. Veri Kutusu Kenarınız için tetikleyiciler dosya olaylarına veya zamanlamaya yanıt olarak olabilir.

- **Dosya**: Bu tetikleyiciler, dosya oluşturma, dosya değişikliği gibi dosya olaylarına yanıt olarak verilir.
- **Zamanlanan**: Bu tetikleyiciler, başlangıç tarihi, başlangıç saati ve yineleme aralığıyla tanımlayabileceğiniz bir zamanlamaya yanıt olarak verilir.


### <a name="add-a-trigger"></a>Tetikleyici ekleme

Tetikleyici oluşturmak için Azure portalında aşağıdaki adımları izleyin.

1. Azure portalında, Veri Kutusu Kenarı kaynağınıza gidin ve ardından **Edge bilgi işlem > Tetikleyici'ye**gidin. Komut çubuğuna **+ tetikleyici ekle'yi** seçin.

    ![Tetikleyici ekle'yi seçin](media/data-box-edge-manage-compute/add-trigger-1.png)

2. **Tetik bıçağı ekle'de,** tetikleyiciniz için benzersiz bir ad sağlayın.
    
    <!--Trigger names can only contain numbers, lowercase letters, and hyphens. The share name must be between 3 and 63 characters long and begin with a letter or a number. Each hyphen must be preceded and followed by a non-hyphen character.-->

3. Tetikleyici için bir **Tür** seçin. Tetikleyici bir dosya olayına yanıt verirken **Dosya'yı** seçin. Tetikleyicinin tanımlı bir zamanda başlamasını ve belirli bir yineleme aralığında çalışmasını istediğinizde **Zamanlanmış'ı** seçin. Seçiminize bağlı olarak, farklı bir seçenek kümesi sunulur.

    - **Dosya tetikleyicisi** - Monte edilmiş bir paylaşım açılır listesinden seçin. Bu paylaşımda bir dosya olayı başlatıldığında, tetikleyici bir Azure İşi çağırır.

        ![SMB paylaşımı ekleme](media/data-box-edge-manage-compute/add-file-trigger.png)

    - **Zamanlanmış tetikleyici** - Başlangıç tarihini/saatini ve saat, dakika veya saniye cinsinden yineleme aralığını belirtin. Ayrıca, bir konunun adını girin. Bir konu, tetikleyiciyi aygıtta dağıtılan bir modüle yönlendirme esnekliği sağlar.

        Örnek bir rota `"route3": "FROM /* WHERE topic = 'topicname' INTO BrokeredEndpoint("modules/modulename/inputs/input1")"`dizesi: .

        ![NFS paylaşımı ekleme](media/data-box-edge-manage-compute/add-scheduled-trigger.png)

4. Tetikleyiciyi oluşturmak için **Ekle'yi** seçin. Bildirim, tetikleyici oluşturmanın devam ettiğini gösterir. Tetikleyici oluşturulduktan sonra, bıçak yeni tetikleyiciyi yansıtacak şekilde güncellenir.
 
    ![Güncelleştirilmiş tetikleyici listesi](media/data-box-edge-manage-compute/add-trigger-2.png)

### <a name="delete-a-trigger"></a>Tetikleyiciyi silme

Tetikleyiciyi silmek için Azure portalında aşağıdaki adımları izleyin.

1. Tetikleyiciler listesinden, silmek istediğiniz tetikleyiciyi seçin.

    ![Tetikleyiciyi seçin](media/data-box-edge-manage-compute/add-trigger-1.png)

2. Sağ tıklatın ve sonra **Sil'i**seçin.

    ![Sil'i seçin](media/data-box-edge-manage-compute/add-trigger-1.png)

3. Onayınız istendiğinde **Evet**’e tıklayın.

    ![Silmeyi onayla](media/data-box-edge-manage-compute/add-trigger-1.png)

Silmeyi yansıtacak tetikleyici güncelleştirmeleri listesi.

## <a name="manage-compute-configuration"></a>İşlem yapılandırması yönetme

Bilgi işlem yapılandırmasını görüntülemek, varolan bir bilgi işlem yapılandırmasını kaldırmak veya Veri Kutusu Edge cihazınız Için IoT aygıtı ve IoT Edge aygıtının erişim anahtarlarını eşitlemek için bilgi işlem yapılandırmasını yenilemek için Azure portalını kullanın.

### <a name="view-compute-configuration"></a>İşlem yapılandırması görüntüleme

Cihazınızın işlem yapılandırmasını görüntülemek için Azure portalında aşağıdaki adımları izleyin.

1. Azure portalında, Veri Kutusu Kenarı kaynağınıza gidin ve ardından **Edge bilgi işlem > Modüllerine**gidin. Komut çubuğunda **Görünüm İşlemi'ni** seçin.

    ![İşlemi Görüntüle'yi seçin](media/data-box-edge-manage-compute/view-compute-1.png)

2. Cihazınızdaki işlem yapılandırmasını not edin. Hesaplamayı yapılandırdığınızda, bir IoT Hub kaynağı oluşturdunuz. Bu IoT Hub kaynağının altında, bir IoT aygıtı ve bir IoT Edge aygıtı yapılandırılır. IoT Edge aygıtında çalışmak üzere yalnızca Linux modülleri desteklenir.

    ![Yapılandırmayı görüntüleme](media/data-box-edge-manage-compute/view-compute-2.png)


### <a name="remove-compute-configuration"></a>İşlem yapılandırması kaldırma

Cihazınız için varolan Edge işlem yapılandırmasını kaldırmak için Azure portalında aşağıdaki adımları izleyin.

1. Azure portalında, Veri Kutusu Kenarı kaynağınıza gidin ve ardından **Edge bilgi işlem > başlayın.** Komut çubuğundaki **İşlemi Kaldır'ı** seçin.

    ![İşlemi Kaldır'ı seçin](media/data-box-edge-manage-compute/remove-compute-1.png)

2. Bilgi işlem yapılandırmasını kaldırırsanız, yeniden bilgi işlem kullanmanız gerektiğinde cihazınızı yeniden yapılandırmanız gerekir. Onay istendiğinde **Evet'i**seçin.

    ![İşlemi Kaldır'ı seçin](media/data-box-edge-manage-compute/remove-compute-2.png)

### <a name="sync-up-iot-device-and-iot-edge-device-access-keys"></a>IoT cihazını ve IoT Edge cihaz erişim anahtarlarını senkronize etme

Veri Kutusu Kenarı'nızda bilgi işlem yapılandırdığınızda, bir IoT aygıtı ve bir IoT Edge aygıtı oluşturulur. Bu aygıtlara otomatik olarak simetrik erişim anahtarları atanır. Güvenlik en iyi uygulama olarak, bu anahtarlar IoT Hub hizmeti üzerinden düzenli olarak döndürülür.

Bu anahtarları döndürmek için oluşturduğunuz IoT Hub hizmetine gidebilir ve IoT aygıtını veya IoT Edge aygıtını seçebilirsiniz. Her aygıtın birincil erişim anahtarı ve ikincil erişim anahtarları vardır. Birincil erişim anahtarını ikincil erişim anahtarına atayın ve ardından birincil erişim anahtarını yeniden oluşturun.

IoT aygıtınız ve IoT Edge aygıt tuşlarınız döndürüldüyse, en son erişim anahtarlarını almak için Veri Kutusu Kenarı'ndaki yapılandırmayı yenilemeniz gerekir. Eşitleme, aygıtın IoT aygıtınız ve IoT Edge aygıtınız için en son anahtarları elde edilmesine yardımcı olur. Veri Kutusu Kenarı yalnızca birincil erişim anahtarlarını kullanır.

Cihazınızın erişim anahtarlarını eşitlemek için Azure portalında aşağıdaki adımları izleyin.

1. Azure portalında, Veri Kutusu Kenarı kaynağınıza gidin ve ardından **Edge bilgi işlem > başlayın.** Komut çubuğundaki **yapılandırmayı yenile'yi** seçin.

    ![Yapılandırmayı Yenile'yi seçin](media/data-box-edge-manage-compute/refresh-configuration-1.png)

2. Onay istendiğinde **Evet'i** seçin.

     ![İstendiğinde Evet'i seçin](media/data-box-edge-manage-compute/refresh-configuration-2.png)

3. Eşitleme tamamlandıktan sonra iletişim kutusunu kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalı üzerinden Edge bilgi işlem ağını](data-box-edge-extend-compute-access-modules.md)nasıl yöneteceklerini öğrenin.
