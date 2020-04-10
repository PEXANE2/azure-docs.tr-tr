---
title: IoT Dijital Dağıtım Merkezi Eğitimi | Microsoft Dokümanlar
description: IoT Central için dijital dağıtım merkezi uygulama şablonu eğitimi
author: KishorIoT
ms.author: nandab
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: overview
ms.date: 10/20/2019
ms.openlocfilehash: cb40252460fe62deb94ce495bb41d38c326f10e2
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "81000591"
---
# <a name="tutorial-deploy-and-walk-through-a-digital-distribution-center-application-template"></a>Öğretici: Dağıtım ve dijital dağıtım merkezi uygulama şablonu ile yürümek



Bu öğretici, bir IoT Merkezi **dijital dağıtım merkezi** uygulama şablonu dağıtarak nasıl başlatılabilirsiniz gösterir. Şablonu nasıl dağıtacağınızı, kutunun dışında nelerin eklenmiş olduğunu ve bundan sonra ne yapmak isteyebileceğini öğreneceksiniz.

Bu öğreticide, nasıl, öğrenmek 
* Dijital dağıtım merkezi uygulaması oluşturma 
* Uygulama da yürüyün 

## <a name="prerequisites"></a>Ön koşullar
* Bu uygulamayı dağıtmak için belirli bir ön koşul gerekmez
* Azure aboneliği önerilir, ancak onsuz bile deneyebilirsiniz

## <a name="create-digital-distribution-center-application-template"></a>Dijital dağıtım merkezi uygulama şablonu oluşturma

Aşağıdaki adımları kullanarak uygulama oluşturabilirsiniz

1. Azure IoT Central uygulama yöneticisi web sitesine gidin. Sol daki gezinme çubuğundan **Oluştur'u** seçin ve ardından **Perakende** sekmesini tıklatın.

    > [!div class="mx-imgBorder"]
    > ![Dijital Dağıtım Merkezi](./media/tutorial-iot-central-ddc/iotc-retail-homepage.png)

2. Dijital dağıtım merkezi uygulaması altında **Perakende** sekmesini seçin ve **uygulamayı oluştur'u** seçin **digital distribution center application**

3. **Create uygulaması** Yeni başvuru formunu açacak ve istenen ayrıntıları aşağıda göstereceği gibi dolduracaktır.
   **Uygulama adı**: varsayılan önerilen adı kullanabilir veya ortak uygulama adınızı girebilirsiniz.
   **URL**: Önerilen varsayılan URL'yi kullanabilir veya benzersiz unutulmaz URL'nizi girebilirsiniz. Ardından, azure aboneliğiniz zaten varsa varsayılan ayar önerilir. 7 günlük ücretsiz deneme fiyatlandırma planıyla başlayabilir ve ücretsiz yolun süresi dolmadan önce istediğiniz zaman standart bir fiyatlandırma planına dönüştürmeyi seçebilirsiniz.
   **Fatura Bilgileri**: Kaynakları sağlamak için Dizin, Azure Aboneliği ve Bölge ayrıntıları gereklidir.
   **Oluştur**: Uygulamanızı dağıtmak için sayfanın altındaki oluştur'u seçin.

    > [!div class="mx-imgBorder"]
    > ![Dijital Dağıtım Merkezi](./media/tutorial-iot-central-ddc/ddc-create.png)

    > [!div class="mx-imgBorder"]
    > ![Dijital Dağıtım fatura bilgileri](./media/tutorial-iot-central-ddc/ddc-create-billinginfo.png)

## <a name="walk-through-the-application-dashboard"></a>Uygulama panosunda yürüyün 

Uygulama şablonunu başarıyla dağıttıktan sonra, varsayılan panonuz dağıtım merkezi operatörü odaklı bir portaldır. Northwind Trader, konveyör sistemlerini yöneten hayali bir dağıtım merkezi çözüm sağlayıcısıdır. 

Bu panoda, bir ağ geçidi ve bir kameranın IoT aygıtı olarak hareket ettiğini göreceksiniz. Ağ Geçidi, ilişkili aygıt ikiz özellikleriyle birlikte geçerli, geçersiz, tanımlanamayan ve boyut gibi paketler hakkında telemetri sağlar. Tüm akış aşağı komutları kamera gibi IoT aygıtlarında çalıştırılır. Bu pano, kritik dağıtım merkezi cihaz işlemleri etkinliğini sergilemek için önceden yapılandırılmıştır.

Pano, Azure IoT ağ geçidi ve IoT aygıtının aygıt yönetimi yeteneklerini göstermek için mantıksal olarak düzenlenir.  
   * Ağ geçidi komutunu & denetim görevlerini gerçekleştirebilirsiniz
   * Çözümün bir parçası olan tüm kameraları yönetin. 

> [!div class="mx-imgBorder"]
> ![Dijital Dağıtım Merkezi](./media/tutorial-iot-central-ddc/ddc-dashboard.png)

## <a name="device-template"></a>Aygıt Şablonu

Aygıt şablonları sekmesine tıkladığınızda ağ geçidi özelliği modelini görürsünüz. Bir yetenek modeli iki farklı arayüzleri **Kamera** ve **Dijital Dağıtım Ağ Geçidi** etrafında yapılandırılmıştır

> [!div class="mx-imgBorder"]
> ![Dijital Dağıtım Merkezi](./media/tutorial-iot-central-ddc/ddc-devicetemplate1.png)

**Kamera** - Bu arayüz tüm kameraya özgü komut özelliklerini düzenler 

> [!div class="mx-imgBorder"]
> ![Dijital Dağıtım Merkezi](./media/tutorial-iot-central-ddc/ddc-camera.png)

**Dijital Dağıtım Ağ Geçidi** - Bu arayüz, kameradan gelen tüm telemetrileri, bulut tanımlı aygıt ikiz özelliklerini ve ağ geçidi bilgilerini temsil eder.

> [!div class="mx-imgBorder"]
> ![Dijital Dağıtım Merkezi](./media/tutorial-iot-central-ddc/ddc-devicetemplate1.png)


## <a name="gateway-commands"></a>Ağ Geçidi Komutları
Bu arabirim tüm ağ geçidi komutu özelliklerini düzenler

> [!div class="mx-imgBorder"]
> ![Dijital Dağıtım Merkezi](./media/tutorial-iot-central-ddc/ddc-camera.png)

## <a name="rules"></a>Kurallar
Bu uygulama şablonunda bulunan iki farklı kuralı görmek için kurallar sekmesini seçin. Bu kurallar, daha fazla araştırma için operatörlere gönderilen e-posta bildirimlerine göre yapılandırılır.

 **Çok fazla geçersiz paket uyarısı** - Bu kural, kamera konveyör sisteminden akan çok sayıda geçersiz paket algıladığında tetiklenir.
 
**Büyük paket** - Bu kural, kamera kalitesi için denetlenemeyen büyük bir paket algılarsa tetikleyecektir. 

> [!div class="mx-imgBorder"]
> ![Dijital Dağıtım Merkezi](./media/tutorial-iot-central-ddc/ddc-rules.png)

## <a name="jobs"></a>İşler
Bu uygulama şablonunun bir parçası olarak var olan beş farklı işi görmek için işler sekmesini seçin: Çözüm genelinde işlemleri gerçekleştirmek için iş özelliğinden yararlanabilirsiniz. Burada dijital dağıtım merkezi işleri gibi görevleri gerçekleştirmek için & ikiz yeteneği cihaz komutları kullanıyor,
   * paket algılamasını başlatmadan önce kamerayı kalibre etme 
   * kamera firmware periyodik olarak güncellenmesi
   * veri yüklemeyönetmek için telemetri aralığını değiştirme

> [!div class="mx-imgBorder"]
> ![Dijital Dağıtım Merkezi](./media/tutorial-iot-central-ddc/ddc-jobs.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu uygulamayı kullanmaya devam etmeyecekseniz, **İdare** > **Uygulaması ayarlarını** ziyaret ederek uygulama şablonunu silin ve **Sil'i**tıklatın.

> [!div class="mx-imgBorder"]
> ![Dijital Dağıtım Merkezi](./media/tutorial-iot-central-ddc/ddc-cleanup.png)

## <a name="next-steps"></a>Sonraki adımlar
* Dijital dağıtım merkezi çözüm mimarisi [dijital dağıtım merkezi konsepti](./architecture-digital-distribution-center.md) hakkında daha fazla bilgi edinin
* Diğer [IoT Central perakende şablonları](./overview-iot-central-retail.md) hakkında daha fazla bilgi edinin
* IoT Central hakkında daha fazla bilgi için [IoT Central genel bakış](../core/overview-iot-central.md)
