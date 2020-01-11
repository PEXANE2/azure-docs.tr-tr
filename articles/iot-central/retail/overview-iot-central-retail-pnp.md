---
title: Azure IoT Central perakende çözümleri oluşturma | Microsoft Docs
description: Bağlı lojistik, dijital dağıtım merkezi, mağaza içi analiz, koşul izleme, kullanıma alma, akıllı envanter yönetimi ve perakende çözümleri oluşturmak için Azure IoT Central uygulama şablonlarını kullanma hakkında bilgi edinin.
author: avneet723
ms.author: avneets
ms.date: 01/10/2020
ms.topic: overview
ms.service: iot-central
ms.subservice: iot-central-retail
services: iot-central
ms.openlocfilehash: 8b1861b96f7ed53388d49e226bf3880bfcf657cb
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75895848"
---
# <a name="building-retail-solutions-with-azure-iot-central"></a>Azure IoT Central ile perakende çözümleri oluşturma

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Azure IoT Central, kurumsal düzeyde IoT çözümlerini geliştirme, yönetme ve sürdürme ile ilişkili yükü ve maliyeti azaltan bir IoT uygulama platformudur. Azure IoT Central ile oluşturmayı seçtiğinizde, yalnızca karmaşık ve sürekli gelişen bir IoT altyapısını korumak ve güncelleştirmek yerine, işletmenizin IoT verileriyle dönüştürülmesi için zaman, para ve enerji tasarrufu sağlayabilirsiniz.

Bu makalede, perakende satış için belirli IoT Central uygulama şablonları açıklanmaktadır. Bir çözüm Oluşturucusu olarak, arz zincirlerini optimize eden IoT çözümleri oluşturmak, müşteriler için mağaza içi deneyimleri iyileştirmek ve envanteri daha verimli bir şekilde izlemek için bu şablonları kullanabilirsiniz.

> [!div class="mx-imgBorder"]
> ![Azure IoT perakende](./media/overview-iot-central-retail/retail-app-templates.png) genel bakış

Aşağıdaki bölümler, bu uygulama şablonlarının yeteneklerini anlatmaktadır:

## <a name="connected-logistics"></a>Bağlı lojistik

Küresel lojistik harcamanın 2020 içinde $10.6 TN 'e ulaşması beklenmektedir. Bu harcama ve nakliye sağlayıcılarının çoğunluğu için mal hesaplarının taşınması, yoğun rekabet baskısı ve kısıtlamalarıdır.

Sıcaklık, nem, eğim, darbe, ışık ve sevkıyatın bulunduğu konum gibi çevresel koşulları toplamak ve izlemek için IoT sensörlerini kullanabilirsiniz. IoT sensörlerinden ve cihazlarından toplanan Telemetriyi, bulut tabanlı iş zekası sistemlerinde hava durumu ve trafik bilgileri gibi diğer veri kaynaklarıyla birleştirebilirsiniz.

Bağlı bir lojistik çözümünün avantajları şunlardır:

* Gerçek zamanlı izleme ve izleme ile sevkıyat izleme. 
* Gerçek zamanlı çevresel koşul izlemeyle, sevkıyat bütünlüğü.
* Hırsızlık, kayıp veya sevkiyatların zarar verdiği güvenlik.
* Coğrafi sınırlama, rota iyileştirmesi, filo yönetimi ve araç analizi.
* Öngörülebilir ve sevkiyatların gelişini tahmin etme.

Aşağıdaki ekran görüntüleri, uygulama şablonundaki kullanıma hazır panoyu gösterir. Pano, özel çözüm gereksinimlerinizi karşılayacak şekilde tamamen özelleştirilebilir:

> [!div class="mx-imgBorder"]
> ![bağlı lojistik panosu](./media/overview-iot-central-retail/connected-logistics-dashboard1.png)

> [!div class="mx-imgBorder"]
> ![bağlı lojistik panosu](./media/overview-iot-central-retail/connected-logistics-dashboard2.png)

Daha fazla bilgi edinmek için bkz. [dağıtım ve bağlı bir lojistik uygulama şablonu](./tutorial-iot-central-connected-logistics-pnp.md) öğreticisini gözden geçir.

## <a name="digital-distribution-center"></a>Dijital dağıtım merkezi

Üreticiler ve perakendeciler dünya çapında bir şekilde kullanıma suntıkça, arz zincirlerinin dalı de daha karmaşık hale gelir. Müşteriler artık büyük miktarda ürünlerin kullanılabilir olmasını ve bu malların satın alma için bir veya iki gün içinde ulaşmasını bekliyor. Dağıtım merkezleri, var olan verimsizlikleri üzerine gelirken bu eğilimleri uyarmalıdır. 

Günümüzde, el ile işçiye yönelik bir rahatlık, dağıtım merkezi maliyetlerinin% 55-65 ' ı için çekme ve ambalaj hesapları anlamına gelir. El ile çekme ve paketleme işlemleri genellikle otomatik sistemlerden daha yavaştır ve ekip oluşturma ihtiyaçlarını hızlı bir şekilde dalgalanmak, nakliye hacimlerini de zorlaştırır. Bu mevsimler, yüksek personel cirosu elde eden sonuçlara neden olur ve maliyetli hata olasılığını artırır.

IoT özellikli kameraları temel alan çözümler, dijital bir geri bildirim döngüsünü etkinleştirerek dönüştürülebilir avantajlar sunabilir. Dağıtım Merkezi genelindeki veriler, daha iyi veri ile sonuçlanarak eyleme dönüştürülebilir içgörülere yol açar.

Dijital bir dağıtım merkezinin avantajları şunlardır:

* Kameralar, geldikçe malları izler ve taşıyıcı sistem üzerinden taşınır.
* Hatalı malların otomatik tanımlanması.
* Etkili sıra izleme.
* Düşük maliyetler, geliştirilmiş üretkenlik ve iyileştirilmiş kullanım.

Aşağıdaki ekran görüntüsünde, uygulama şablonundaki kullanıma hazır pano görüntülenir. Pano, özel çözüm gereksinimlerinizi karşılayacak şekilde tamamen özelleştirilebilir: 

> [!div class="mx-imgBorder"]
> ![Digital Distribution Center panosu](./media/overview-iot-central-retail/digital-distribution-center-dashboard.png)

Daha fazla bilgi edinmek için bkz. [bir Digital Distribution Center uygulama şablonu öğreticisine dağıtım ve İzlenecek yol](./tutorial-iot-central-digital-distribution-center-pnp.md) .

## <a name="in-store-analytics---condition-monitoring"></a>Mağaza içi analiz-koşul izleme

Birçok perakende için, mağazaların içindeki çevresel koşullar rakiplerinden önemli bir farklımalardır. Perakendeciler, müşterilerinin avantajlarından yararlanmak için mağazalarındaki parçalar dahilinde bakım koşullarını sürdürmek ister.  

Bir çözüm Oluşturucusu olarak, uçtan uca bir çözüm oluşturmak için mağaza içi Analytics Condition izleme uygulaması şablonunu IoT Central kullanabilirsiniz. Uygulama şablonu, çeşitli algılayıcı cihazları kullanarak bir perakende mağaza ortamına dijital olarak bağlanmanızı ve bunları izlemenizi sağlar. Bu algılayıcı cihazları, iş maliyetlerini azaltmaya ve müşterileri için harika bir deneyim oluşturmanıza yardımcı olan iş öngörülerine dönüştürebileceğiniz telemetri oluşturur.

Uygulama şablonunu kullanarak şunları yapın:

* Birçok IoT sensöri IoT Central uygulama örneğine bağlayın.
* , Algılayıcı ağının sistem durumunu ve ortamdaki tüm ağ geçidi cihazlarını izleyin ve yönetin.
* Mağaza yöneticileri için uyarıları tetiklemek üzere bir depodaki ortam koşullarının etrafında özel kurallar oluşturun.
* Mağazalarınızın içindeki çevresel koşulları, Retail Store ekibinin müşteri deneyimini geliştirmek için kullanabileceği öngörülere dönüştürün.
* Toplu öngörüleri mevcut veya yeni iş uygulamalarına dışarı aktarıp perakende personeli için yararlı ve zamanında bilgiler sağlayın.

Uygulama şablonu bir cihaz şablonları kümesiyle birlikte gelir ve panoyu doldurmak için bir sanal cihaz kümesi kullanır. 

Aşağıdaki ekran görüntüsünde, uygulama şablonundaki kullanıma hazır pano görüntülenir. Pano, özel çözüm gereksinimlerinizi karşılayacak şekilde tamamen özelleştirilebilir: 

> [!div class="mx-imgBorder"]
> Mağaza Içi analiz koşulu Izleme](./media/overview-iot-central-retail/in-store-analytics-condition-dashboard.png) ![

Daha fazla bilgi edinmek için bkz. [Azure 'da bir mağaza 'da analiz uygulaması oluşturma IoT Central](./tutorial-in-store-analytics-create-app-pnp.md) öğreticisi.

## <a name="in-store-analytics---checkout"></a>Mağaza içi analiz-kullanıma alma

Bazı perakendeciler için mağazaların içindeki kullanıma alma deneyimi rakiplerinden önemli bir farklıdır. Perakendeciler, müşterilerin döndürmesini teşvik etmek için mağazalarında sorunsuz bir kullanıma alma deneyimi sunmak ister.  

Bir çözüm Oluşturucusu olarak, bir mağazanın teslim alma bölgesinin perakende personeline ait kullanıma sunduğu içgörüler sunan bir çözüm oluşturmak için IoT Central mağaza Analizi kullanıma alma uygulaması şablonunu kullanabilirsiniz. Örneğin, sensörler, her bir kullanıma alma şeridi için sıra uzunlukları ve ortalama bekleme süreleri hakkında bilgi verebilir.

Uygulama şablonunu kullanarak şunları yapın:

* Birçok IoT sensöri IoT Central uygulama örneğine bağlayın.
* , Algılayıcı ağının sistem durumunu ve ortamdaki tüm ağ geçidi cihazlarını izleyin ve yönetin.
* Retail personele yönelik uyarıları tetiklemek için bir mağaza içindeki kullanıma alma koşulunun etrafında özel kurallar oluşturun.
* Mağaza içindeki kullanıma alma koşullarını, Retail Store ekibinin müşteri deneyimini geliştirmek için kullanabileceği öngörülere dönüştürün.
* Toplu öngörüleri mevcut veya yeni iş uygulamalarına dışarı aktarıp perakende personeli için yararlı ve zamanında bilgiler sağlayın.

Uygulama şablonu bir cihaz şablonları kümesiyle birlikte gelir ve panoyu Lane doluluk verileriyle doldurmak için bir dizi sanal cihaz kullanır. 

Aşağıdaki ekran görüntüsünde, uygulama şablonundaki kullanıma hazır pano görüntülenir. Pano, özel çözüm gereksinimlerinizi karşılayacak şekilde tamamen özelleştirilebilir: 

> [!div class="mx-imgBorder"]
> Mağaza Analizi kullanıma alma](./media/overview-iot-central-retail/In-Store-Analytics-Checkout-Dashboard.png) ![

Daha fazla bilgi edinmek için bkz. [Azure 'da bir mağaza 'da analiz uygulaması oluşturma IoT Central](./tutorial-in-store-analytics-create-app-pnp.md) öğreticisi.

## <a name="smart-inventory-management"></a>Akıllı envanter yönetimi

Stok, bir perakendecinin elinde tutulduğu malların envanterini alır. Envanter yönetimi, doğru ürünün doğru zamanda doğru yerde olduğundan emin olmak için önemlidir. Satıcı, talebi karşılamak için stokta yeterli miktarda öğe bulunmayan maliyetlere göre çok fazla envanteri depolamanın maliyetlerini dengelemenize gerek vermelidir.

Radyo frekansı tanımlama (RFıD) etiketlerinden, işaretleriyle ve kameralardan oluşturulan IoT verileri, envanter yönetimi süreçlerini geliştirmek için fırsat sağlar. IoT sensörlerinden ve cihazlarından toplanan Telemetriyi, bulut tabanlı iş zekası sistemlerinde hava durumu ve trafik bilgileri gibi diğer veri kaynaklarıyla birleştirebilirsiniz.

Akıllı envanter yönetimi 'nin avantajları şunlardır:

* Maddelerin stokunda risk azaltma ve istenen müşteri hizmeti düzeyini sağlama. 
* Neredeyse gerçek zamanlı olarak stok doğruluğu hakkında ayrıntılı analiz ve Öngörüler.
* Müşteri emirlerini karşılamak üzere tutulacak doğru envanter miktarına karar vermenize yardımcı olacak araçlar.

Bu uygulama şablonu cihaz bağlantısına odaklanır ve RFıD ve Bluetooth düşük enerji (BLE) okuyucu cihazlarının yapılandırma ve yönetimine odaklanır.

Aşağıdaki ekran görüntüsünde, uygulama şablonundaki kullanıma hazır pano görüntülenir. Pano, özel çözüm gereksinimlerinizi karşılayacak şekilde tamamen özelleştirilebilir:

> [!div class="mx-imgBorder"]
> Akıllı envanter yönetimi panosu ![](./media/overview-iot-central-retail/smart-inventory-management-dashboard.png)

Daha fazla bilgi edinmek için bkz. [dağıtım ve bir akıllı envanter yönetimi uygulama şablonu](./tutorial-iot-central-smart-inventory-management-pnp.md) öğreticisini gözden geçir.

## <a name="micro-fulfillment-center"></a>Mikro Karşılama Merkezi

Daha fazla rekabet eden perakende yatay, perakendeciler, talep ve yerine getirme arasındaki boşluğu kapatmak için sürekli olarak bir basınç. Büyüyen tüketici talebini ele almak için ortaya çıktı olan yeni bir eğilim, son müşterilere ve ziyaret ettikleri depolara yaklaşmaktadır.

IoT Central mikro Karşılama Merkezi uygulama şablonu, çözüm oluşturucularının tamamen otomatikleştirilmiş karşılama merkezlerinin tüm yönlerini izlemesine ve yönetmesine olanak sağlar. Şablon, çözüm geliştirme sürecini hızlandırmak için bir dizi sanal durum izleme algılayıcı ve robot taşıyıcılar içerir. Bu algılayıcı cihazları, perakendecilerin kendi işletim maliyetlerini azaltmasına ve müşterilerine yönelik deneyimler oluşturmalarına olanak tanıyan anlamlı sinyalleri yakalar.

Uygulama şablonu şunları yapmanızı sağlar: 

- IoT Central bir uygulama örneğine robots veya Condition izleme algılayıcıları gibi çeşitli IoT sensörlerinden sorunsuz bir şekilde bağlanın.
- , Algılayıcı ağının sistem durumunu ve ortamdaki tüm ağ geçidi cihazlarını izleyin ve yönetin.
- Uygun uyarıları tetiklemek için bir karşılama Merkezi içindeki çevresel koşulların etrafında özel kurallar oluşturun.
- Karşılama merkezinizdeki çevresel koşulları, perakende ambar ekibi tarafından yararlanılabilir olabilecek öngörülere dönüştürün.
- Toplu öngörüleri, perakende personeli üyelerinin avantajı için mevcut veya yeni iş uygulamalarına dışarı aktarın.

Aşağıdaki ekran görüntüsünde, uygulama şablonundaki kullanıma hazır pano görüntülenir. Pano, özel çözüm gereksinimlerinizi karşılayacak şekilde tamamen özelleştirilebilir:

> [!div class="mx-imgBorder"]
> ![mikro Karşılama Merkezi](./media/overview-iot-central-retail/MFC-Dashboard.png)

Daha fazla bilgi edinmek için bkz. [dağıtım ve Micro-Karşılama Merkezi uygulama şablonu](./tutorial-iot-central-smart-inventory-management-pnp.md) öğreticisini inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bir perakende çözümü oluşturmaya başlamak için:

* Azure IoT Central, mağaza içi analitik uygulama şablonlarından biriyle bir çözüm oluşturma konusunda size yol gösteren [Azure 'da bir yerleşik analiz uygulaması oluşturma](./tutorial-in-store-analytics-create-app-pnp.md) öğreticisini kullanmaya başlayın.
* [Bağlı bir lojistik uygulama şablonunu dağıtın ve gözden geçir](./tutorial-iot-central-connected-logistics-pnp.md).
* [Bir dijital dağıtım merkezi uygulama şablonunu dağıtın ve gözden geçir](./tutorial-iot-central-digital-distribution-center-pnp.md).
* [Bir akıllı envanter yönetimi uygulama şablonunu dağıtın ve gözden geçir](./tutorial-iot-central-smart-inventory-management-pnp.md).
* [Mikro Karşılama Merkezi uygulama şablonunu dağıtın ve adım adım](./tutorial-iot-central-smart-inventory-management-pnp.md)inceleyin.
* [IoT Central genel bakışta](../preview/overview-iot-central.md)IoT Central hakkında daha fazla bilgi edinin.
