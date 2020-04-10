---
title: IoT Bağlantılı lojistik Eğitimi | Microsoft Dokümanlar
description: IoT Central için Bağlı lojistik uygulama şablonu eğitimi
author: KishorIoT
ms.author: nandab
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: overview
ms.date: 10/20/2019
ms.openlocfilehash: eac43ae68b10436b3e45452c6b1d03bec3ae4c9c
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "81000563"
---
# <a name="tutorial-deploy-and-walk-through-a-connected-logistics-application-template"></a>Öğretici: Bağlı bir lojistik uygulama şablonu dağıtma ve yürüme



Bu öğretici, bir IoT Merkezi **bağlı lojistik** uygulama şablonu dağıtarak nasıl başlatılabilirsiniz gösterir. Şablonu nasıl dağıtacağınızı, kutunun dışında nelerin eklenmiş olduğunu ve bundan sonra ne yapmak isteyebileceğini öğreneceksiniz.

Bu eğitimde, nasıl öğreneceksiniz,

* bağlı lojistik uygulaması oluşturmak
* uygulama ile yürümek 

## <a name="prerequisites"></a>Ön koşullar

* Bu uygulamayı dağıtmak için belirli bir ön koşul gerekmez
* Azure aboneliği önerilir, ancak onsuz bile deneyebilirsiniz

## <a name="create-connected-logistics-application-template"></a>Bağlı lojistik uygulama şablonu oluşturma

Aşağıdaki adımları kullanarak uygulama oluşturabilirsiniz

1. Azure IoT Central uygulama yöneticisi web sitesine gidin. Sol daki gezinme çubuğundan **Oluştur'u** seçin ve ardından **Perakende** sekmesini tıklatın.

    > [!div class="mx-imgBorder"]
    > ![Bağlı Lojistik Panosu](./media/tutorial-iot-central-connected-logistics/iotc-retail-homepage.png)

2. Connected Logistics Application altında **Uygulama** **Oluştur'u** seçin

3. **Create uygulaması** Yeni başvuru formunu açacak ve istenen ayrıntıları aşağıda göstereceği gibi dolduracaktır.
   * **Uygulama adı**: varsayılan önerilen adı kullanabilir veya ortak uygulama adınızı girebilirsiniz.
   * **URL**: Önerilen varsayılan URL'yi kullanabilir veya benzersiz unutulmaz URL'nizi girebilirsiniz. Ardından, azure aboneliğiniz zaten varsa varsayılan ayar önerilir. 7 günlük ücretsiz deneme fiyatlandırma planıyla başlayabilir ve ücretsiz yolun süresi dolmadan önce istediğiniz zaman standart bir fiyatlandırma planına dönüştürmeyi seçebilirsiniz.
   * **Fatura Bilgileri**: Kaynakları sağlamak için Dizin, Azure Aboneliği ve Bölge ayrıntıları gereklidir.
   * **Oluştur**: Uygulamanızı dağıtmak için sayfanın altındaki oluştur'u seçin.

    > [!div class="mx-imgBorder"]
    > ![Bağlı Lojistik Panosu](./media/tutorial-iot-central-connected-logistics/connected-logistics-app-create.png)

    > [!div class="mx-imgBorder"]
    > ![Bağlı Lojistik fatura bilgileri](./media/tutorial-iot-central-connected-logistics/connected-logistics-app-create-billinginfo.png)

## <a name="walk-through-the-application"></a>Uygulama da yürüyün 

## <a name="dashboard"></a>Pano

Uygulama şablonunu başarıyla dağıttıktan sonra, varsayılan panonuz bağlı bir lojistik operatörü odaklı portaldır. Northwind Trader okyanusta ve karada kargo filosu nu yöneten hayali bir lojistik sağlayıcısıdır. Bu panoda, ilgili komutlar, işler ve yapabileceğiniz eylemlerle birlikte gönderiler hakkında telemetri sağlayan iki farklı ağ geçidi görürsünüz. Bu pano, kritik lojistik cihaz operasyon faaliyetlerini sergilemek için önceden yapılandırılmıştır.
Pano mantıksal olarak iki farklı ağ geçidi aygıtı yönetimi işlemi arasında bölünmüştür, 
   * Kamyon sevkiyatı için lojistik rota ve okyanus sevkiyatının konum detayları tüm çok modal taşımacılık için vazgeçilmez bir unsurdur.
   * Ağ geçidi durumunu & ilgili bilgileri görüntüleme 

> [!div class="mx-imgBorder"]
> ![Bağlı Lojistik Panosu](./media/tutorial-iot-central-connected-logistics/connected-logistics-dashboard1.png)

   * Etkin ve bilinmeyen etiketlerin toplam sayısını kolayca izleyebilirsiniz.
   * Yazılımları güncelleme, sensörü devre dışı bırakma, sensörü etkinleştirme, sensör eşiğini güncelleme, telemetri aralıklarını güncelleme, cihaz hizmet sözleşmelerini & gibi cihaz yönetimi işlemleri yapabilirsiniz.
   * Cihazın pil tüketimini görüntüleme

> [!div class="mx-imgBorder"]
> ![Bağlı Lojistik Panosu](./media/tutorial-iot-central-connected-logistics/connected-logistics-dashboard2.png)

## <a name="device-template"></a>Aygıt Şablonu

Aygıt şablonları sekmesine tıkladığınızda ağ geçidi özelliği modelini görürsünüz. Bir yetenek modeli iki farklı arabirim **ağ geçidi Telemetri & Özellik** ve Ağ Geçidi **Komutları** etrafında yapılandırılmıştır

**Gateway Telemetri & Özelliği** - Bu arayüz sensörler, konum ve cihaz bilgileri ile ilgili tüm telemetri yanı sıra sensör eşikleri & güncelleme aralıkları gibi cihaz ikiz özelliği yeteneği temsil eder.

> [!div class="mx-imgBorder"]
> ![Bağlı Lojistik Panosu](./media/tutorial-iot-central-connected-logistics/connected-logistics-devicetemplate1.png)

**Ağ Geçidi Komutları** - Bu arabirim tüm ağ geçidi komutu özelliklerini düzenler

> [!div class="mx-imgBorder"]
> ![Bağlı Lojistik Panosu](./media/tutorial-iot-central-connected-logistics/connected-logistics-devicetemplate2.png)

## <a name="rules"></a>Kurallar
Bu uygulama şablonunda bulunan iki farklı kuralı görmek için kurallar sekmesini seçin. Bu kurallar, daha fazla araştırma için operatörlere gönderilen e-posta bildirimlerine göre yapılandırılır.
 
**Ağ geçidi hırsızlığı uyarısı**: Bu kural, yolculuk sırasında sensörler tarafından beklenmeyen Bir ışık algılamaolduğunda tetiklenir. Operatörler potansiyel hırsızlık araştırmak için En KıSA sürede haberdar edilmesi gerekir.
 
**Yanıt vermeyen Ağ Geçidi**: Ağ geçidi buluta uzun bir süre rapor vermezse bu kural tetiklenir. Ağ geçidi, düşük pil modu, bağlantı kaybı, aygıt durumu nedeniyle yanıt vermiyor olabilir.

> [!div class="mx-imgBorder"]
> ![Bağlı Lojistik Panosu](./media/tutorial-iot-central-connected-logistics/connected-logistics-rules.png)

## <a name="jobs"></a>İşler
Bu uygulama şablonunun bir parçası olarak var olan beş farklı işi görmek için işler sekmesini seçin:

> [!div class="mx-imgBorder"]
> ![Bağlı Lojistik Panosu](./media/tutorial-iot-central-connected-logistics/connected-logistics-jobs.png)

Çözüm genelinde işlemler yapmak için iş özelliğini kullanabilirsiniz. Burada işler, tüm ağ geçidindeki belirli sensörleri devre dışı bırakmak veya sevkiyat moduna ve rotaya bağlı olarak sensör eşiğini değiştirmek gibi görevleri yapmak için aygıt komutlarını ve ikiz yeteneğini kullanıyor. 
   * Soğuk zincir taşımacılığı sırasında piltasarrufu veya sıcaklık eşiğini azaltmak için okyanus sevkiyatı sırasında şok sensörlerini devre dışı bırakmastandart bir işlemdir. 
 
   * İşler, ağ geçitlerinde firmware'i güncelleştirme veya bakım faaliyetlerinde güncel kalmak için hizmet sözleşmesini güncelleştirme gibi sistem çapında işlemler yapmanızı sağlar.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu uygulamayı kullanmaya devam etmeyecekseniz, **İdare** > **Uygulaması ayarlarını** ziyaret ederek uygulama şablonunu silin ve **Sil'i**tıklatın.

> [!div class="mx-imgBorder"]
> ![Bağlı Lojistik Panosu](./media/tutorial-iot-central-connected-logistics/connected-logistics-cleanup.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Bağlantılı lojistik konsepti](./architecture-connected-logistics.md) hakkında daha fazla bilgi edinin
* Diğer [IoT Central perakende şablonları](./overview-iot-central-retail.md) hakkında daha fazla bilgi edinin
* [IoT Central'a genel bakış](../core/overview-iot-central.md) hakkında daha fazla bilgi edinin
