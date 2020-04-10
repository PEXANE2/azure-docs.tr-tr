---
title: IoT Smart envanter yönetimi eğitimi | Microsoft Dokümanlar
description: IoT Central için akıllı envanter yönetimi uygulama şablonu eğitimi
author: KishorIoT
ms.author: nandab
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: overview
ms.date: 10/20/2019
ms.openlocfilehash: 430f477422e040e0e7e28fd69a1cbc18bce7d656
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "81000549"
---
# <a name="tutorial-deploy-and-walk-through-a-smart-inventory-management-application-template"></a>Öğretici: Akıllı envanter yönetimi uygulama şablonu dağıtın ve yürüyün



Bu öğretici, bir IoT Merkezi **akıllı envanter yönetimi** uygulama şablonu dağıtarak nasıl başlatılabilirsiniz gösterir. Şablonu nasıl dağıtacağınızı, kutunun dışında nelerin eklenmiş olduğunu ve bundan sonra ne yapmak isteyebileceğini öğreneceksiniz.

Bu öğreticide, nasıl, öğrenmek 
* akıllı envanter yönetimi uygulaması oluşturmak 
* uygulama ile yürümek 

## <a name="prerequisites"></a>Ön koşullar

* Bu uygulamayı dağıtmak için belirli bir ön koşul gerekmez
* Azure aboneliği önerilir, ancak onsuz bile deneyebilirsiniz

## <a name="create-smart-inventory-management-application-template"></a>Akıllı envanter yönetimi uygulama şablonu oluşturma

Aşağıdaki adımları kullanarak uygulama oluşturabilirsiniz

1. Azure IoT Central uygulama yöneticisi web sitesine gidin. Sol daki gezinme çubuğundan **Oluştur'u** seçin ve ardından **Perakende** sekmesini tıklatın.

    > [!div class="mx-imgBorder"]
    > ![Akıllı Envanter Yönetimi Panosu](./media/tutorial-iot-central-smart-inventory-management/iotc_retail_homepage.png)

2. **Perakende** sekmesini seçin ve akıllı envanter yönetimi altında **uygulama oluştur'u** seçin **smart inventory management**

3. **Create uygulaması** Yeni başvuru formunu açacak ve istenen ayrıntıları aşağıda göstereceği gibi dolduracaktır.
   **Uygulama adı**: varsayılan önerilen adı kullanabilir veya ortak uygulama adınızı girebilirsiniz.
   **URL**: Önerilen varsayılan URL'yi kullanabilir veya benzersiz unutulmaz URL'nizi girebilirsiniz. Ardından, azure aboneliğiniz zaten varsa varsayılan ayar önerilir. 7 günlük ücretsiz deneme fiyatlandırma planıyla başlayabilir ve ücretsiz yolun süresi dolmadan önce istediğiniz zaman standart bir fiyatlandırma planına dönüştürmeyi seçebilirsiniz.
   **Fatura Bilgileri**: Kaynakları sağlamak için Dizin, Azure Aboneliği ve Bölge ayrıntıları gereklidir.
   **Oluştur**: Uygulamanızı dağıtmak için sayfanın altındaki oluştur'u seçin.

    > [!div class="mx-imgBorder"]
    > ![Akıllı Envanter Yönetimi Panosu](./media/tutorial-iot-central-smart-inventory-management/smart_inventory_management_app_create.png)

    > [!div class="mx-imgBorder"]
    > ![Akıllı Envanter Yönetimi fatura bilgileri](./media/tutorial-iot-central-smart-inventory-management/smart-inventory-management-app-create-billinginfo.png)

## <a name="walk-through-the-application"></a>Uygulama da yürüyün 

### <a name="dashboard"></a>Pano 

Uygulama şablonunu başarıyla dağıttıktan sonra, varsayılan panonuz akıllı stok yönetimi operatörü odaklı bir portaldır. Northwind Trader, Bluetooth düşük enerjili (BLE) ve Radyo frekansı tanımlaması (RFID) ile perakende mağazayı yöneten hayali bir akıllı envanter sağlayıcısıdır. Bu panoda, ilgili komutlar, işler ve gerçekleştirebileceğiniz eylemlerle birlikte envanter hakkında telemetri sağlayan iki farklı ağ geçidi görürsünüz. Bu pano, kritik akıllı envanter yönetimi aygıtı operasyon etkinliğini sergilemek için önceden yapılandırılmıştır.
Pano mantıksal olarak iki farklı ağ geçidi aygıtı yönetimi işlemi arasında bölünmüştür, 
   * Ambar, daha büyük bir tesiste envanteri izleme& izlemek için paletlerde sabit bir BLE ağ geçidi & BLE etiketleri ile dağıtılır
   * Perakende mağaza, bir mağaza çıkışındaki stoku izlemek ve izlemek için tek tek bir madde düzeyinde RFID etiketleri & sabit bir RFID ağ geçidi ile uygulanır
   * Ağ geçidi konumunu, durum & ilgili ayrıntıları görüntüleme 

> [!div class="mx-imgBorder"]
> ![Akıllı Envanter Yönetimi Panosu](./media/tutorial-iot-central-smart-inventory-management/smart_inventory_management_dashboard1.png)

   * Etkin ve bilinmeyen etiketlerin toplam sayısını kolayca izleyebilirsiniz.
   * Yazılımları güncelleme, sensörü devre dışı bırakma, sensörü etkinleştirme, sensör eşiğini güncelleme, telemetri aralıklarını güncelleme & cihaz hizmet sözleşmelerini güncelleme gibi cihaz yönetimi işlemlerini gerçekleştirebilirsiniz
   * Ağ geçidi aygıtları, tam veya artımlı bir taramayla isteğe bağlı envanter yönetimi gerçekleştirebilir.

> [!div class="mx-imgBorder"]
> ![Akıllı Envanter Yönetimi Panosu](./media/tutorial-iot-central-smart-inventory-management/smart_inventory_management_dashboard2.png)

## <a name="device-template"></a>Aygıt Şablonu
Aygıt şablonları sekmesine tıkladığınızda ağ geçidi özelliği modelini görürsünüz. Bir yetenek modeli iki farklı arabirim **ağ geçidi Telemetri & Özellik** ve Ağ Geçidi **Komutları** etrafında yapılandırılmıştır

**Ağ Geçidi Telemetri & Özelliği** - Bu arabirim, sensörler, konum, aygıt bilgileri ve ağ geçidi eşikleri ve güncelleştirme aralıkları gibi aygıt ikiz özelliği yle ilgili tüm telemetrileri temsil eder.

> [!div class="mx-imgBorder"]
> ![Akıllı Envanter Yönetimi Panosu](./media/tutorial-iot-central-smart-inventory-management/smart_inventory_management_devicetemplate1.png)


**Ağ Geçidi Komutları** - Bu arabirim tüm ağ geçidi komutu özelliklerini düzenler

> [!div class="mx-imgBorder"]
> ![Akıllı Envanter Yönetimi Panosu](./media/tutorial-iot-central-smart-inventory-management/smart_inventory_management_devicetemplate2.png)

## <a name="rules"></a>Kurallar
Bu uygulama şablonunda bulunan iki farklı kuralı görmek için kurallar sekmesini seçin. Bu kurallar, daha fazla araştırma için operatörlere gönderilen e-posta bildirimlerine göre yapılandırılır.

**Ağ geçidi çevrimdışı**: Ağ geçidi buluta uzun bir süre rapor vermezse bu kural tetiklenir. Ağ geçidi, düşük pil modu, bağlantı kaybı, aygıt durumu nedeniyle yanıt vermiyor olabilir.

**Bilinmeyen etiketler**: Bir varlıkla ilişkili her RFID & BLE etiketini izlemek çok önemlidir. Ağ geçidi çok fazla bilinmeyen etiket algılıyorsa, bu etiket kaynak uygulamalarıyla eşitleme zorluklarının bir göstergesidir.

> [!div class="mx-imgBorder"]
> ![Akıllı Envanter Yönetimi Panosu](./media/tutorial-iot-central-smart-inventory-management/smart_inventory_management_rules.png)

## <a name="jobs"></a>İşler
Bu uygulama şablonunun bir parçası olarak var olan beş farklı işi görmek için işler sekmesini seçin: Çözüm genelinde işlemleri gerçekleştirmek için iş özelliğini kullanabilirsiniz. Burada envanter yönetimi işleri gibi görevleri gerçekleştirmek için cihaz komutları ve ikiz yeteneği kullanıyorsanız,
   * tüm ağ geçidinde okuyucuları devre dışı bırakma
   * arasında telemetri eşiğini değiştirme 
   * tüm çözüm boyunca isteğe bağlı envanter taraması gerçekleştirin.

> [!div class="mx-imgBorder"]
> ![Akıllı Envanter Yönetimi Panosu](./media/tutorial-iot-central-smart-inventory-management/smart_inventory_management_jobs.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz, **İdare** > **Uygulaması ayarlarını** ziyaret ederek uygulama şablonunu silin ve **Sil'i**tıklatın.

> [!div class="mx-imgBorder"]
> ![Akıllı Envanter Yönetimi Panosu](./media/tutorial-iot-central-smart-inventory-management/smart_inventory_management_cleanup.png)

## <a name="next-steps"></a>Sonraki adımlar
* Akıllı envanter yönetimi [akıllı envanter yönetimi konsepti](./architecture-smart-inventory-management.md) hakkında daha fazla bilgi edinin
* Diğer [IoT Central perakende şablonları](./overview-iot-central-retail.md) hakkında daha fazla bilgi edinin
* IoT Central hakkında daha fazla bilgi için [IoT Central genel bakış](../core/overview-iot-central.md)
