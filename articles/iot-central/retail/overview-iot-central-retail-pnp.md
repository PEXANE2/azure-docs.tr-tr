---
title: Azure IoT Central ile perakende çözümleri oluşturma | Microsoft Dokümanlar
description: Bağlı lojistik, dijital dağıtım merkezi, mağaza içi analiz, durum izleme, ödeme, akıllı envanter yönetimi ve perakende çözümleri oluşturmak için Azure IoT Merkezi uygulama şablonlarını kullanma hakkında bilgi edinin.
author: avneet723
ms.author: avneets
ms.date: 01/10/2020
ms.topic: overview
ms.service: iot-central
ms.subservice: iot-central-retail
services: iot-central
ms.openlocfilehash: 808a17fcf303c6eb72f11f5379fa172302dc782a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77059870"
---
# <a name="building-retail-solutions-with-azure-iot-central"></a>Azure IoT Central ile perakende çözümleri oluşturma



Azure IoT Central, kurumsal kalitede IoT çözümleri geliştirme, yönetme ve sürdürmeyle ilişkili yükü ve maliyeti azaltan bir IoT uygulama platformudur. Azure IoT Central ile geliştirmeyi seçmek, karmaşık ve sürekli gelişen bir IoT altyapısını korumak ve güncellemek yerine zamanınızı, paranızı ve enerjinizi ioT verileriyle işletmenizi dönüştürmeye odaklama fırsatı verir.

Bu makalede, birkaç perakendeye özel IoT Merkezi uygulama şablonları açıklanmaktadır. Çözüm oluşturucu olarak, bu şablonları tedarik zincirlerini optimize eden, müşteriler için mağaza içi deneyimleri geliştiren ve envanteri daha verimli bir şekilde izleyen IoT çözümleri oluşturmak için kullanabilirsiniz.

> [!div class="mx-imgBorder"]
> ![Azure IoT Perakende Genel Bakış](./media/overview-iot-central-retail/retail-app-templates.png)

Aşağıdaki bölümlerde bu uygulama şablonlarının yetenekleri açıklayınız:

## <a name="connected-logistics"></a>Bağlı lojistik

Küresel lojistik harcamalarının 2020 yılında 10,6 trilyon dolara ulaşması bekleniyor. Bu harcamaların ve nakliye sağlayıcılarının büyük bir kısmı için mal ların taşınması yoğun rekabet baskısı ve kısıtlamaları altındadır.

Sıcaklık, nem, eğim, şok, ışık ve gönderinin konumu gibi ortam koşullarını toplamak ve izlemek için IoT sensörlerini kullanabilirsiniz. IoT sensörlerinden ve aygıtlarından toplanan telemetrileri bulut tabanlı iş zekası sistemlerinde hava durumu ve trafik bilgileri gibi diğer veri kaynaklarıyla birleştirebilirsiniz.

Bağlantılı bir lojistik çözümün yararları şunlardır:

* Gerçek zamanlı izleme ve izleme ile sevkiyat izleme. 
* Gerçek zamanlı ortam durumu izleme ile sevkiyat bütünlüğü.
* Sevkiyatların çalınması, kaybolması veya hasar görmesi ne kadar güvenlik.
* Coğrafi eskrim, rota optimizasyonu, filo yönetimi ve araç analitiği.
* Sevkiyatların öngörülebilir kalkış ve varış tahmini.

Aşağıdaki ekran görüntüleri, uygulama şablonundaki kutunun dışında panosunu gösterir. Pano, özel çözüm gereksinimlerinizi karşılamak için tamamen özelleştirilebilir:

> [!div class="mx-imgBorder"]
> ![Bağlı Lojistik Panosu](./media/overview-iot-central-retail/connected-logistics-dashboard1.png)

> [!div class="mx-imgBorder"]
> ![Bağlı Lojistik Panosu](./media/overview-iot-central-retail/connected-logistics-dashboard2.png)

Daha fazla bilgi edinmek için Dağıtım'a bakın ve bağlı bir lojistik uygulama şablonu öğreticisi [aracılığıyla yürüyün.](./tutorial-iot-central-connected-logistics-pnp.md)

## <a name="digital-distribution-center"></a>Dijital dağıtım merkezi

Üreticiler ve perakendeciler dünya çapında varlıklar oluşturdukça, tedarik zincirleri şubeleşir ve daha karmaşık hale gelir. Tüketiciler artık büyük ürün seçeneklerinin mevcut olmasını ve bu malların satın alma işleminden sonraki bir veya iki gün içinde gelmesini bekliyorlar. Dağıtım merkezleri, mevcut verimsizliklerin üstesinden gelirken bu eğilimlere uyum sağlamalıdır. 

Bugün, el emeğine duyulan güven, toplama ve paketlemenin dağıtım merkezi maliyetlerinin %55-65'ini oluşturduğu anlamına gelir. Manuel toplama ve paketleme de genellikle otomatik sistemlere göre daha yavaştır ve personel ihtiyacının hızla dalgalanması nakliye hacimlerini karşılamayı daha da zorlaştırır. Bu mevsimsel dalgalanma yüksek personel cirosu ile sonuçlanır ve pahalı hatalar olasılığını artırır.

IoT özellikli kameralara dayalı çözümler, dijital geri bildirim döngüsü sağlayarak dönüşümsel faydalar sağlayabilir. Dağıtım merkezinin dört bir yanından gelen veriler, daha iyi verilerle sonuçlanan işlem edilebilir öngörülere yol açar.

Dijital dağıtım merkezinin yararları şunlardır:

* Kameralar malları gelirken izler ve konveyör sisteminden geçerler.
* Hatalı malların otomatik olarak tanımlanması.
* Verimli sipariş takibi.
* Daha düşük maliyetler, geliştirilmiş üretkenlik ve optimize edilmiş kullanım.

Aşağıdaki ekran görüntüsü, uygulama şablonundaki kutunun dışında panoyu gösterir. Pano, özel çözüm gereksinimlerinizi karşılamak için tamamen özelleştirilebilir: 

> [!div class="mx-imgBorder"]
> ![Dijital Dağıtım Merkezi Panosu](./media/overview-iot-central-retail/digital-distribution-center-dashboard.png)

Daha fazla bilgi edinmek için Dağıtım'a bakın ve dijital dağıtım merkezi uygulama şablonu öğreticisini [inceleyin.](./tutorial-iot-central-digital-distribution-center-pnp.md)

## <a name="in-store-analytics---condition-monitoring"></a>Mağaza içi analiz - durum izleme

Birçok perakendeci için, kendi mağazalarında çevre koşulları rakiplerinden önemli bir fark vardır. Perakendeciler müşterilerinin yararına kendi mağazalarında hoş koşullar korumak istiyorum.  

Çözüm oluşturucu olarak, uçtan uca bir çözüm oluşturmak için IoT Merkezi mağaza içi analitik durum izleme uygulama şablonunu kullanabilirsiniz. Uygulama şablonu, farklı sensör aygıtlarını kullanarak bir perakende mağaza ortamına dijital olarak bağlanmanızı ve izlemenizi sağlar. Bu sensörlü cihazlar, perakendecinin işletme maliyetlerini azaltmasına ve müşterileri için harika bir deneyim yaratmasına yardımcı olarak iş öngörülerine dönüştürebileceğiniz telemetri oluşturur.

Uygulama şablonundan yararlanın:

* Çeşitli IoT sensörlerini IoT Merkezi uygulama örneğine bağlayın.
* Sensör ağının ve çevredeki tüm ağ geçidi aygıtlarının sağlığını izleyin ve yönetin.
* Mağaza yöneticileri için uyarıları tetiklemek için mağazadaki çevre koşulları yla ilgili özel kurallar oluşturun.
* Mağazanızdaki çevre koşullarını, perakende mağaza ekibinin müşteri deneyimini geliştirmek için kullanabileceği içgörülere dönüştürün.
* Perakende personeline yararlı ve zamanında bilgi sağlamak için, toplu öngörüleri mevcut veya yeni iş uygulamalarına aktarın.

Uygulama şablonu bir aygıt şablonları kümesiyle birlikte gelir ve panoyu doldurmak için bir dizi benzetimli aygıt kullanır. 

Aşağıdaki ekran görüntüsü, uygulama şablonundaki kutunun dışında panoyu gösterir. Pano, özel çözüm gereksinimlerinizi karşılamak için tamamen özelleştirilebilir: 

> [!div class="mx-imgBorder"]
> ![Mağaza İçi Analitik Durum İzleme](./media/overview-iot-central-retail/in-store-analytics-condition-dashboard.png)

Daha fazla bilgi edinmek için [Azure IoT Central öğreticisinde mağaza içi analiz uygulaması oluşturun'a](./tutorial-in-store-analytics-create-app-pnp.md) bakın.

## <a name="in-store-analytics---checkout"></a>Mağaza içi analitik - ödeme

Bazı perakendeciler için, mağazalarında ödeme deneyimi rakiplerinden önemli bir fark vardır. Perakendeciler, müşterileringeri dönmelerini teşvik etmek için mağazalarında sorunsuz bir ödeme deneyimi sunmak isterler.  

Çözüm oluşturucu olarak, bir mağazanın ödeme bölgesinin çevresinden perakende personeline kadar öngörüler sunan bir çözüm oluşturmak için IoT Merkezi mağaza içi analiz ödeme uygulama şablonu'nu kullanabilirsiniz. Örneğin, sensörler her ödeme şeridi için sıra uzunlukları ve ortalama bekleme süreleri hakkında bilgi sağlayabilir.

Uygulama şablonundan yararlanın:

* Çeşitli IoT sensörlerini IoT Merkezi uygulama örneğine bağlayın.
* Sensör ağının ve çevredeki tüm ağ geçidi aygıtlarının sağlığını izleyin ve yönetin.
* Perakende çalışanları için uyarıları tetiklemek için mağazadaki ödeme durumu etrafında özel kurallar oluşturun.
* Mağazadaki ödeme koşullarını, perakende mağaza ekibinin müşteri deneyimini geliştirmek için kullanabileceği öngörülere dönüştürün.
* Perakende personeline yararlı ve zamanında bilgi sağlamak için, toplu öngörüleri mevcut veya yeni iş uygulamalarına aktarın.

Uygulama şablonu bir dizi aygıt şablonuyla birlikte gelir ve gösterge panelini şerit doluluk verileriyle doldurmak için bir dizi simüle edilmiş aygıt kullanır. 

Aşağıdaki ekran görüntüsü, uygulama şablonundaki kutunun dışında panoyu gösterir. Pano, özel çözüm gereksinimlerinizi karşılamak için tamamen özelleştirilebilir: 

> [!div class="mx-imgBorder"]
> ![Mağaza İçi Analitik Ödeme](./media/overview-iot-central-retail/In-Store-Analytics-Checkout-Dashboard.png)

Daha fazla bilgi edinmek için [Azure IoT Central öğreticisinde mağaza içi analiz uygulaması oluşturun'a](./tutorial-in-store-analytics-create-app-pnp.md) bakın.

## <a name="smart-inventory-management"></a>Akıllı envanter yönetimi

Envanter, bir perakendecinin elinde tuttuğu mal stokudur. Doğru ürünün doğru zamanda doğru yerde olduğundan emin olmak için envanter yönetimi çok önemlidir. Bir perakendeci, talebi karşılamak için stokta yeterli madde olmamasının maliyetlerine göre çok fazla stok depolama maliyetlerini dengelemelidir.

Radyo frekansı tanımlama (RFID) etiketleri, işaretçileri ve kameralardan oluşturulan IoT verileri, envanter yönetimi süreçlerini iyileştirmek için fırsatlar sağlar. IoT sensörlerinden ve aygıtlarından toplanan telemetrileri bulut tabanlı iş zekası sistemlerinde hava durumu ve trafik bilgileri gibi diğer veri kaynaklarıyla birleştirebilirsiniz.

Akıllı envanter yönetiminin yararları şunlardır:

* Maddelerin stokta kalma riskini azaltmak ve istenilen müşteri hizmetleri düzeyini sağlamak. 
* Neredeyse gerçek zamanlı olarak envanter doğruluğuna ilişkin derinlemesine analiz ve öngörüler.
* Müşteri siparişlerini karşılamak için tutmak için doğru stok miktarına karar vermenize yardımcı olacak araçlar.

Bu uygulama şablonu aygıt bağlantısına ve RFID ve Bluetooth düşük enerjili (BLE) okuyucu aygıtlarının yapılandırması ve yönetimine odaklanır.

Aşağıdaki ekran görüntüsü, uygulama şablonundaki kutunun dışında panoyu gösterir. Pano, özel çözüm gereksinimlerinizi karşılamak için tamamen özelleştirilebilir:

> [!div class="mx-imgBorder"]
> ![Akıllı Envanter Yönetimi Panosu](./media/overview-iot-central-retail/smart-inventory-management-dashboard.png)

Daha fazla bilgi edinmek için Dağıtım'a bakın [ve akıllı envanter yönetimi uygulama şablonu öğreticisini inceleyin.](./tutorial-iot-central-smart-inventory-management-pnp.md)

## <a name="micro-fulfillment-center"></a>Mikro karşılama merkezi

Giderek daha rekabetçi hale gelen perakende ortamında, perakendeciler talep ve karşılama arasındaki boşluğu kapatmak için sürekli olarak baskı yla karşı laşıyorlar. Artan tüketici talebini karşılamak için ortaya çıkan yeni bir eğilim, son müşterilere ve ziyaret ettikleri mağazalara yakın envanter barındırmaktır.

IoT Central mikro-dolum merkezi uygulama şablonu, çözüm oluşturucuların tam otomatik karşılama merkezlerinin tüm yönlerini izlemelerine ve yönetmelerine olanak tanır. Şablon, çözüm geliştirme sürecini hızlandırmak için bir dizi simüle durum izleme sensörlerini ve robot taşıyıcıları içerir. Bu sensörlü cihazlar, perakendecilerin işletme maliyetlerini azaltmalarına ve müşterileri için deneyimler oluşturmalarına olanak tanıyan iş öngörülerine dönüştürülebilen anlamlı sinyaller yakalar.

Uygulama şablonu şunları yapmanızı sağlar: 

- Robotlar veya durum izleme sensörleri gibi farklı IoT sensörlerini IoT Merkezi uygulama örneğine sorunsuz bir şekilde bağlayın.
- Sensör ağının ve çevredeki tüm ağ geçidi aygıtlarının sistem durumunu izleyin ve yönetin.
- Uygun uyarıları tetiklemek için bir karşılama merkezi içindeki çevre koşulları etrafında özel kurallar oluşturun.
- Karşılama merkezinizdeki çevre koşullarını perakende depo ekibi tarafından yararlanılabilen öngörülere dönüştürün.
- Perakende personelinin yararına mevcut veya yeni iş uygulamalarına toplu öngörüler aktarın.

Aşağıdaki ekran görüntüsü, uygulama şablonundaki kutunun dışında panoyu gösterir. Pano, özel çözüm gereksinimlerinizi karşılamak için tamamen özelleştirilebilir:

> [!div class="mx-imgBorder"]
> ![Mikro-karşılama Merkezi](./media/overview-iot-central-retail/MFC-Dashboard.png)

Daha fazla bilgi edinmek için Dağıtım'ı görün [ve mikro yerine getirme merkezi uygulama şablonu öğreticisini inceleyin.](./tutorial-micro-fulfillment-center-pnp.md)

## <a name="next-steps"></a>Sonraki adımlar

Perakende çözümü oluşturmaya başlamak için:

* [Azure IoT Central öğreticisinde](./tutorial-in-store-analytics-create-app-pnp.md) mağaza içi analitik uygulama oluşturma uygulamasına başlayın ve mağaza içi analitik uygulama şablonlarından biriyle nasıl bir çözüm oluşturabileceğinizi öğrenin.
* [Bağlı bir lojistik uygulama şablonu dağıtın ve yürüyün.](./tutorial-iot-central-connected-logistics-pnp.md)
* [Bir dijital dağıtım merkezi uygulama şablonu dağıtın ve yürüyün.](./tutorial-iot-central-digital-distribution-center-pnp.md)
* [Akıllı envanter yönetimi uygulama şablonu dağıtın ve yürüyün.](./tutorial-iot-central-smart-inventory-management-pnp.md)
* [Dağıtma ve mikro-yerine getirme merkezi uygulama şablonu ile yürümek.](./tutorial-micro-fulfillment-center-pnp.md)
* [IoT Central'a genel bakış](../preview/overview-iot-central.md)ta IoT Central hakkında daha fazla bilgi edinin.
