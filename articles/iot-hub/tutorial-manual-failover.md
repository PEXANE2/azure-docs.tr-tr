---
title: Azure IoT hub'ında elle yük devretme | Microsoft Docs
description: IoT hub'ınızın başka bir bölgeye manuel olarak nasıl başarısız olacağınızi öğrenin ve çalıştığını onaylayın ve ardından özgün bölgeye döndürün ve yeniden denetleyin.
author: robinsh
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 07/24/2019
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: bf37f7b27e3f8923a229cc0617365d912d47aec2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77110669"
---
# <a name="tutorial-perform-manual-failover-for-an-iot-hub"></a>Öğretici: Bir IoT hub'ı için manuel hata yapma

Elle yük devretme, müşterilerin hub işlemleri için birincil bölgeden coğrafi olarak eşleştirilen ilgili Azure coğrafi eşleme bölgesine [yük devretme](https://en.wikipedia.org/wiki/Failover) gerçekleştirmesini sağlayan bir IoT Hub hizmeti özelliğidir. Elle yük devretme, bölgesel olağanüstü durum veya uzun süreli hizmet kesintisi durumunda gerçekleştirilebilir. Ayrıca sisteminizin olağanüstü durum kurtarma özelliklerini test etmek için de planlı bir yük devretme gerçekleştirebilirsiniz. Bunun için üretim ortamı yerine test amaçlı bir IoT hub kullanmanız önerilir. Elle yük devretme özelliği, müşterilere ek maliyet olmadan sunulur.

Bu öğreticide, aşağıdaki görevleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Azure portalı kullanarak bir IoT hub'ı oluşturun. 
> * Yük devretme gerçekleştirin. 
> * Hub'ın ikincil konumda çalıştığını görün.
> * IoT hub'ın işlemlerini birincil konuma geri almak için yeniden çalışma gerçekleştirin. 
> * Hub'ın doğru konumda düzgün biçimde çalıştığını onaylayın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu öğreticideki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

1. [Azure portalına](https://portal.azure.com)giriş yapın. 

2. **+ Kaynak oluştur**'a tıklayıp **Nesnelerin İnterneti**'ni ve ardından **IoT Hub** öğesini seçin.

   ![IoT hub'ı oluşturmayı gösteren ekran görüntüsü](./media/tutorial-manual-failover/create-hub-01.png)

3. Temel **Bilgiler** sekmesini seçin. Aşağıdaki alanları doldurun.

    **Abonelik**: Kullanmak istediğiniz Azure aboneliğini seçin.

    **Kaynak Grubu**: **Yeni oluştur**'a tıklayın ve kaynak grubu adı olarak **ManlFailRG** girin.

    **Bölge**: size yakın bir bölge seçin. Bu öğreticide `West US 2` kullanılır. Yük devretme yalnızca coğrafi olarak eşleştirilmiş Azure bölgeleri arasında gerçekleştirilebilir. Batı ABD 2 ile coğrafi eşleştirilmiş bölge WestCentralUS olduğunu.
    
   **IoT Hub'ı Adı**: IoT hub'ınıza bir ad verin. Hub adının genel olarak benzersiz olması gerekir. 

   ![IoT hub'ı oluşturmak için Temel Bilgiler bölmesini gösteren ekran görüntüsü](./media/tutorial-manual-failover/create-hub-02-basics.png)

   **Gözden geçir ve oluştur**’a tıklayın. (Boyut ve ölçek için varsayılan değerler kullanılır.) 

4. Bilgileri gözden geçirin ve **Oluştur**'a tıklayarak IoT hub'ını oluşturun. 

   ![IoT hub'ı oluşturmak için son adımı gösteren ekran görüntüsü](./media/tutorial-manual-failover/create-hub-03-create.png)

## <a name="perform-a-manual-failover"></a>Elle yük devretme gerçekleştirme

Bir IoT hub'ı için günlük iki yük devretme ve iki yeniden çalışma sınırı vardır.

1. **Kaynak grupları**'na tıklayın ve **ManlFailRG** adlı kaynak grubunu seçin. Kaynak listesinde hub'ınıza tıklayın. 

1. IoT Hub bölmesindeki **Ayarlar'ın** altında **Failover'ı**tıklatın.

   ![IoT Hub'ı özellikler bölmesini gösteren ekran görüntüsü](./media/tutorial-manual-failover/trigger-failover-01.png)

1. El Ile failover bölmesinde **Geçerli konumu** ve **Failover konumunu**görürsünüz. Geçerli konum her zaman hub'ın şu anda etkin olduğu konumu gösterir. Başarısız konumu, geçerli konumla eşleştirilmiş standart [Azure coğrafi eşleştirilmiş bölgedir.](../best-practices-availability-paired-regions.md) Konum değerlerini değiştiremezsiniz. Bu öğretici için geçerli `West US 2` konum ve başarısız `West Central US`konumu .

   ![Elle Yük Devretme bölmesini gösteren ekran görüntüsü](./media/tutorial-manual-failover/trigger-failover-02.png)

1. El Ile failover bölmesinin üst kısmında, **başarısızlığı başlat'ı**tıklatın. 

1. Onay bölmesinde, başarısız olmak istediğiniz hub'ın olduğunu doğrulamak için IoT hub'ınızın adını doldurun. Ardından, failover'ı başlatmak için **Failover'ı**tıklatın.

   Elle yük devretme gerçekleştirmek için gereken süre, hub'ınıza kayıtlı olan cihaz sayısına bağlıdır. Örneğin 100.000 cihazınız varsa 15 dakika, beş milyon cihazınız varsa bir saat veya daha uzun sürebilir.

   ![Elle Yük Devretme bölmesini gösteren ekran görüntüsü](./media/tutorial-manual-failover/trigger-failover-03-confirm.png)

   El ile başarısız verme işlemi çalışırken, bir başlık size bir el ile başarısız lık devam ettiğini söylemek için görünür. 

   ![Devam eden Elle Yük Devretme işlemini gösteren ekran görüntüsü](./media/tutorial-manual-failover/trigger-failover-04-in-progress.png)

   IoT Hub bölmesini kapatıp Kaynak Grubu bölmesine tıklayarak yeniden açarsanız, hub'ın el ile başarısız bir bölmenin ortasında olduğunu belirten bir başlık görürsünüz. 

   ![IoT Hub'ın başarısız olduğunu gösteren ekran görüntüsü devam ediyor](./media/tutorial-manual-failover/trigger-failover-05-hub-inactive.png)

   Bittikten sonra, Manuel Failover sayfasındaki geçerli ve başarısız bölgeler çevrilir ve hub yeniden etkin hale getirir. Bu örnekte, geçerli konum `WestCentralUS` şimdi ve failover `West US 2`konumu şimdi. 

   ![Yük devretme işleminin tamamlandığını gösteren ekran görüntüsü](./media/tutorial-manual-failover/trigger-failover-06-finished.png)

   Genel bakış sayfasında ayrıca, failover'ın tamamladığını ve IoT `West Central US`Hub'ın çalıştığını belirten bir başlık da bulunur.

   ![Genel bakış sayfasında başarısız olan ekran görüntüsü tamamlandı](./media/tutorial-manual-failover/trigger-failover-06-finished-overview.png)


## <a name="perform-a-failback"></a>Yeniden çalışma gerçekleştirme 

Elle yük devretme gerçekleştirdikten sonra hub işlemlerini tekrar birincil bölgeye geçirebilirsiniz. Bu işlem yeniden çalışma olarak adlandırılır. Yük devretme işlemini kısa bir süre önce gerçekleştirdiyseniz yeniden çalışma isteğinde bulunmak için yaklaşık bir saat beklemeniz gerekir. Daha kısa bir süre içinde yeniden çalışma isteğinde bulunursanız bir hata iletisi görüntülenir.

Yeniden çalışma işlemi, elle yük devretme işlemiyle aynı şekilde gerçekleştirilir. Bu adımlar şunlardır: 

1. Yeniden çalışma gerçekleştirmek için IoT hub'ınızın IoT Hub'ı bölmesine dönün.

2. IoT Hub bölmesindeki **Ayarlar'ın** altında **Failover'ı**tıklatın. 

3. El Ile failover bölmesinin üst kısmında, **başarısızlığı başlat'ı**tıklatın. 

4. Onay bölmesinde, başarısız olmak istediğiniz hub'ın olduğunu doğrulamak için IoT hub'ınızın adını doldurun. Ardından yeniden çalışmayı başlatmak için Tamam'a tıklayın. 

   ![Elle yeniden çalışma isteğinin ekran görüntüsü](./media/tutorial-manual-failover/trigger-failover-03-confirm.png)

   Görüntülenen başlıklar yük devretme gerçekleştirme bölümünde anlatılanlarla aynıdır. Failback tamamlandıktan sonra, yeniden `West US 2` geçerli konum `West Central US` olarak ve başarısız konumu olarak, başlangıçta ayarlanan olarak gösterir.

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bu öğretici için oluşturduğunuz kaynakları kaldırmak için kaynak grubunu silin. Bu eylem grubun içerdiği tüm kaynakları siler. Bu durumda, IoT hub'ı ve kaynak grubu kaldırılır. 

1. **Kaynak Grupları**'na tıklayın. 

2. **ManlFailRG** adlı kaynak grubunu bulun ve seçin. Açmak için tıklayın. 

3. **Kaynak grubunu sil**'e tıklayın. İstendiğinde kaynak grubunun adını girin ve **Sil**'e tıklayarak onaylayın. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki görevleri gerçekleştirerek elle yük devretme yapılandırma ve gerçekleştirmenin yanı sıra yeniden çalışma isteğinde bulunmayı öğrendiniz:

> [!div class="checklist"]
> * Azure portalı kullanarak bir IoT hub'ı oluşturun. 
> * Yük devretme gerçekleştirin. 
> * Hub'ın ikincil konumda çalıştığını görün.
> * IoT hub'ın işlemlerini birincil konuma geri almak için yeniden çalışma gerçekleştirin. 
> * Hub'ın doğru konumda düzgün biçimde çalıştığını onaylayın.

IoT cihazı durumunun nasıl yönetileceğini öğrenmek için sonraki öğreticiye geçin. 

> [!div class="nextstepaction"]
> [IoT cihazı durumunu yönetme](tutorial-device-twins.md)
