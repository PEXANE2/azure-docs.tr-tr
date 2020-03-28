---
title: 'Öğretici: Azure IoT Central ile bir su tüketimi izleme uygulaması oluşturun'
description: 'Öğretici: Azure IoT Merkezi uygulama şablonlarını kullanarak bir su tüketimi izleme uygulaması oluşturmayı öğrenin.'
author: miriambrus
ms.author: miriamb
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: 7f00947504e5c6355379ce1e400fd2325016e05a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77122077"
---
# <a name="tutorial-create-a-water-consumption-monitoring-application-with-azure-iot-central"></a>Öğretici: Azure IoT Central ile su tüketimi izleme uygulaması oluşturun

Bu öğretici, Azure IoT Merkezi su tüketimi izleme uygulama şablonu kullanarak bir Azure IoT Merkezi su tüketimi izleme uygulaması oluşturmanızı gösterir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Su tüketimi izleme uygulamanızı oluşturmak için Azure IoT Merkezi su tüketimi izleme şablonunu kullanın.
> * Operatör panosunu keşfedin ve özelleştirin.
> * Aygıt şablonlarını keşfedin.
> * Simüle edilmiş aygıtları keşfedin.
> * Kuralları keşfedin ve yapılandırır.
> * İşleri yapılandırın.
> * Beyaz etiketleme kullanarak uygulama markanızı özelleştirin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- Azure aboneliği. Azure aboneliğiniz yoksa, [Azure kaydolma sayfasında](https://aka.ms/createazuresubscription)bir abonelik oluşturabilirsiniz.

## <a name="create-a-water-consumption-monitoring-app-with-azure-iot-central"></a>Azure IoT Central ile bir su tüketimi izleme uygulaması oluşturun

Bu bölümde, Azure IoT Central'da su tüketimi izleme uygulamanızı oluşturmak için Azure IoT Merkezi su tüketimi izleme şablonunu kullanırsınız.

Yeni bir Azure IoT Merkezi su tüketimi izleme uygulaması oluşturmak için:

1. [Azure IoT Merkezi ana sayfası](https://aka.ms/iotcentral) web sitesine gidin.

    Azure aboneliğiniz varsa, bu aboneye erişmek için kullandığınız kimlik bilgileriyle oturum açın. Aksi takdirde, bir Microsoft hesabı kullanarak oturum açın.

    ![Kuruluş hesabınızı girin](media/tutorial-waterconsumptionmonitoring/sign-in.png)

1. Sol bölmede **Yap'ı** seçin ve **Devlet** sekmesini seçin. **Hükümet** sayfası çeşitli devlet uygulama şablonları görüntüler.

   ![Devlet uygulama şablonları oluşturma](./media/tutorial-waterconsumptionmonitoring/iotcentral-government-tab-overview1.png)

1. Su **tüketim izleme** uygulama şablonu'nu seçin.
Bu şablon, örnek bir su tüketim aygıtı şablonu, benzetimli aygıt, operatör panosu ve önceden yapılandırılmış izleme kurallarını içerir.

1. **Yeni uygulama** oluşturma formunu aşağıdaki alanlarla açmak için **Oluştur uygulamasını** seçin:
    * **Uygulama adı**: Varsayılan olarak, uygulama *Su tüketimi izleme* sini ve ardından Azure IoT Central'ın oluşturduğu benzersiz bir kimlik dizesini kullanır. İsteğe bağlı olarak, kolay bir uygulama adı seçin. Uygulama adını daha sonra da değiştirebilirsiniz.
    * **URL**: Azure IoT Central, uygulama adını temel alan bir URL'yi otomatik olarak oluşturur. URL'yi istediğiniz gibi güncellemeyi seçebilirsiniz. URL'yi daha sonra da değiştirebilirsiniz.
    * Azure aboneliğiniz varsa, **Dizin,** **Azure aboneliği**ve **Konum** bilgilerinizi girin. Aboneliğiniz yoksa, **7 günlük ücretsiz deneme** seçeneğini seçebilir ve gerekli iletişim bilgilerini tamamlayabilirsiniz.

    Dizinler ve abonelikler hakkında daha fazla bilgi için [bkz.](../core/quick-deploy-iot-central.md)

1. Sayfanın altındaki **Oluştur'u** seçin.

    ![Azure IoT Merkezi Yeni uygulama sayfası](./media/tutorial-waterconsumptionmonitoring/new-application-waterconsumptionmonitoring.png)

    ![Azure IoT Merkezi Faturalandırma bilgi sayfası](./media/tutorial-waterconsumptionmonitoring/new-application-waterconsumptionmonitoring-billinginfo.png)

Azure IoT Merkezi su tüketimi izleme şablonu kullanarak bir su tüketimi izleme uygulaması oluşturdunuz.

Su tüketimi izleme uygulaması önceden yapılandırılmış ile birlikte gelir:

* Örnek operatör panoları.
* Örnek önceden tanımlanmış su akışı ve valf cihazı şablonları.
* Simüle su akışı ve akıllı valf cihazları.
* Kurallar ve işler.
* Beyaz etiketleme kullanarak markayı örneklendirin.

Bu sizin uygulamanız ve istediğiniz zaman değiştirebilirsiniz. Şimdi uygulamayı inceleyelim ve bazı özelleştirmeler yapalım.

## <a name="explore-and-customize-the-operator-dashboard"></a>Operatör panosunu keşfedin ve özelleştirin

Uygulamayı oluşturduktan sonra, örnek **Geniş Dünya su tüketim panosu** açılır.

   ![Su tüketimi izleme panosu](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-dashboardfull.png)

Bir oluşturucu olarak, operatörler için panoda görünümler oluşturabilir ve özelleştirebilirsiniz. Özelleştirmeye çalışmadan önce panonu inceleyelim.

> [!NOTE]
> Panoda görüntülenen tüm veriler, bir sonraki bölümde inceleyeceğimiz simüle edilmiş aygıt verilerine dayanmaktadır.
  
Pano farklı türde döşemelerden oluşur:

* **Geniş Dünya Su Utility görüntü karo**: Panodaki ilk karo hayali su programı Wide World Water bir görüntü karo olduğunu. Kendi resminizi ekleyerek veya kaldırarak döşemeyi özelleştirebilirsiniz.
* **Ortalama su akışı KPI karosu**: KPI karosu *son 30 dakikadaki ortalamayı*örnek olarak gösterecek şekilde yapılandırılır. KPI döşemesini özelleştirebilir ve farklı bir tür ve zaman aralığına ayarlayabilirsiniz.
* **Cihaz komut karoları**: Bu karolar **Yakın valf,** **Açık valf**ve **Set valf konum** karoları içerir. Komutları seçmek sizi simüle edilen aygıt komut uyruğu sayfasına götürür. Azure IoT Central'da *komut* *aygıt özelliği* türüdür. Bu kavramı daha sonra bu öğreticinin "Aygıt şablonu" bölümünde inceleyeceğiz.
* **Su dağıtım alanı haritası**: Harita, doğrudan Azure IoT Central'da yapılandırabileceğiniz Azure Haritaları'nı kullanır. Harita döşemesi aygıt konumunu görüntüler. Haritanın üzerine taşırın ve *yakınlaştırma,* *yakınlaştırma*veya *genişletme*gibi harita üzerindeki kontrolleri deneyin.

    ![Su tüketimi izleme pano haritası](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-dashboard-map.png)

* **Ortalama su akış çizgisi grafiği** ve Çevresel durum **çizgisi grafiği**: İstenilen zaman aralığında bir çizgi grafiği olarak çizilmiş bir veya birden çok cihaz telemetrisini görselleştirebilirsiniz.
* **Ortalama valf basınç ısı haritası grafiği**: Renk indeksi ile bir zaman aralığında dağıtıldığını görmek istediğiniz cihaz telemetri verilerinin ısı haritası görselleştirme türünü seçebilirsiniz.
* **Uyarı eşiklerini sıfırlama küreme içeriği kutucuğu**: Eylem çağrısı namına içerik kutucuklarını ekleyebilir ve bir eylem sayfasına bağlantı gömebilirsiniz. Bu durumda, sıfırlama uyarısı eşiği, güncelleştirmeleri aygıt özelliklerinde çalıştırabileceğiniz **Uygulama İşleri'ne**götürür. Bu seçeneği daha sonra bu öğreticinin "İşleri Yapılandır" bölümünde inceleyeceğiz.
* **Özellik kutucukları**: **Pano, Valf çalışma bilgilerini,** **Akış uyarı eşiklerini**ve **Bakım bilgi** kutucuklarını görüntüler.

### <a name="customize-the-dashboard"></a>Panoyu özelleştirme

Bir oluşturucu olarak, operatörler için panodaki görünümleri özelleştirebilirsiniz.

1. **Geniş Dünya su tüketim panosunu**özelleştirmek için **Edit'i** seçin. **Düzenleme** menüsünü seçerek panoyu özelleştirebilirsiniz. Pano **edit** modunda olduktan sonra, yeni kutucuklar ekleyebilir veya yapılandırabilirsiniz.

     ![Panoyu düzenle](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-edit-dashboard.png)

1. Yeni bir pano oluşturmak ve sıfırdan yapılandırmak için **+ Yeni'yi** seçin. Birden çok panonuz olabilir ve pano menüsündeki panolarınız arasında hareket edebilirsiniz.

## <a name="explore-the-device-template"></a>Aygıt şablonuna göz at

Azure IoT Central'daki aygıt şablonu, telemetri, özellik veya komut olabilecek bir aygıtın yeteneğini tanımlar. Oluşturucu olarak, Azure IoT Central'da bağlanacağınız aygıtların yeteneğini temsil eden bir veya daha fazla aygıt şablonu tanımlayabilirsiniz.

Su tüketimi izleme uygulaması, *akış ölçeri* ve *akıllı valf* cihazını temsil eden iki referans cihaz şablonuyla birlikte gelir.

Aygıt şablonu görüntülemek için:

1. Azure IoT Central'da uygulamanızın sol bölmesinde **Aygıt şablonlarını** seçin. Aygıt **şablonları** **listesinde, Akıllı Vana** ve **Akış ölçer**olmak üzere iki aygıt şablonu görürsünüz.

   ![Aygıt şablonu](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-devicetemplate.png)

1. Akış **ölçer** aygıt şablonu'nu seçin ve aygıt özelliklerini edin.

     ![Aygıt şablonu Akış ölçer](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-devicetemplate-flowmeter.png)

### <a name="customize-the-device-template"></a>Aygıt şablonu özelleştirme

Aygıt şablonu özelleştirmek için:

1. **Aygıt şablonları** menüsünde **Özelleştir'e** gidin.
1. `Temperature` Telemetri türünü bulun.
1. Ekran **Adını** `Temperature` `Reported temperature`güncelleştirin.
1. Ölçü birimini güncelleştirin veya **Min değerini** ve **Max değerini**ayarlayın.
1. Değişiklikleri kaydetmek için **Kaydet'i** seçin.

### <a name="add-a-cloud-property"></a>Bulut özelliği ekleme

1. **Aygıt şablonları** menüsünden **Bulut Özellikleri'ne** gidin.
1. **+ Bulut Özelliği Ekle'yi**seçerek yeni bir bulut özelliği ekleyin.
    Azure IoT Central'da aygıtla alakalı bir özellik ekleyebilirsiniz. Örnek olarak, bir bulut özelliği yükleme alanına, varlık bilgilerine veya diğer bakım bilgilerine özgü bir uyarı eşiği olabilir.
1. Değişiklikleri kaydetmek için **Kaydet'i** seçin.

### <a name="views"></a>Görünümler

Su tüketim monitörü cihaz şablonu önceden tanımlanmış görünümlerle birlikte gelir. Görünümleri keşfedin ve güncelleştirmeler yapabilirsiniz. Görünümler, işleçlerin aygıt verilerini nasıl gördüğünü ve aynı zamanda bulut özelliklerini nasıl girdirini tanımlar.

  ![Aygıt şablonu görünümleri](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-devicetemplate-views.png)

### <a name="publish"></a>Yayımlama

Herhangi bir değişiklik yaptıysanız, aygıt şablonuna **yayımladığınızdan** emin olun.

### <a name="create-a-new-device-template"></a>Yeni cihaz şablonu oluşturma

Yeni bir aygıt şablonu oluşturmak ve oluşturma işlemini izlemek için **+ Yeni'yi** seçin.
Sıfırdan özel bir aygıt şablonu oluşturabilir veya Azure Aygıt Kataloğu'ndan bir aygıt şablonu seçebilirsiniz.

## <a name="explore-simulated-devices"></a>Simüle edilmiş cihazları keşfedin

Azure IoT Central'da, aygıt şablonunuzu ve uygulamanızı sınamak için benzetimli aygıtlar oluşturabilirsiniz. Su tüketimi izleme uygulaması, **Akış ölçer** ve **Akıllı Vana** cihaz şablonlarına eşlenen iki simüle cihaza sahiptir.

### <a name="view-the-devices"></a>Aygıtları görüntüleme

1. **Aygıtlar'ı** > seçin Sol bölmedeki**tüm aygıtlar.**

   ![Tüm aygıtlar bölmesi](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-devices.png)

1. **Akıllı Vana 1'i**seçin.

    ![Akıllı Vana 1](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitor-device1.png)

1. **Komutlar** sekmesinde, **Akıllı Vana** cihaz şablonunda tanımlanan yetenekler olan üç cihaz komutunu **(Kapak valfi,** **Açık valf**ve **Set valf konumu)** görebilirsiniz.

1. Aygıt **Özellikleri** sekmesini ve **Aygıt Panosu** sekmesini keşfedin.

> [!NOTE]
> Tüm sekmelerin aygıt şablonu görünümlerinden yapılandırıldığına dikkat edin.

### <a name="add-new-devices"></a>Yeni aygıtlar ekleme

**Cihazlar** sekmesinde **+ Yeni'yi** seçerek yeni aygıtlar ekleyin.

## <a name="explore-and-configure-rules"></a>Kuralları keşfedin ve yapılandırır

Azure IoT Central'da, aygıt telemetrisini otomatik olarak izlemek ve bir veya daha fazla koşul yerine getirildiğinde eylemleri tetiklemek için kurallar oluşturabilirsiniz. Eylemler arasında e-posta bildirimleri göndermek veya diğer hizmetlere veri göndermek için bir Microsoft Power Otomatikleştir eylemini veya webhook eylemini tetiklemek de olabilir.

Oluşturduğunuz su tüketimi izleme uygulamasının önceden yapılandırılmış üç kuralı vardır.

### <a name="view-rules"></a>Kuralları görüntüleme

1. Sol bölmede **Kurallar'ı** seçin.

   ![Kurallar bölmesi](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-rules.png)

1. Uygulamada önceden yapılandırılmış kurallardan biri olan **Yüksek pH uyarısı'nı**seçin.

     ![Yüksek pH uyarısı](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-highflowalert.png)

    Kural `High flow alert` durumuna `Acidity (pH)` karşı kontrol etmek için `greater than` `Max flow threshold`yapılandırılmıştır . Maksimum akış eşiği, Smart **Valve** aygıt şablonunda tanımlanan bir bulut özelliğidir. `Max flow threshold` Değeri aygıt örneğine göre ayarlanır.

Şimdi bir e-posta eylemi oluşturalım.

Kurala eylem eklemek için:

1. Seçin **+ E-posta**.
1. Eylem için dost Ekran **adı** olarak **Yüksek pH uyarısı** girin.
1. Azure IoT Central hesabınızla ilişkili e-posta adresini **To'ya**girin.
1. İsteğe bağlı olarak, e-posta metnine eklemek için bir not girin.
1. Eylemi tamamlamak için **Bitti'yi** seçin.
1. Kaydetmek ve yeni kuralı etkinleştirmek için **Kaydet'i** seçin.

Birkaç dakika içinde, yapılandırılan koşul karşılandıktan sonra bir e-posta almanız gerekir.

> [!NOTE]
> Uygulama, bir koşul her karşılaştığında bir e-posta gönderir. Otomatik kuraldan e-posta almayı durdurmak için kuralı devre dışı bırakmak için **Devre Dışı** Bırak'ı seçin.
  
Yeni bir kural oluşturmak için:

* Sol bölmedeki **Kurallar** sekmesinde **+ Yeni'yi** seçin.

## <a name="configure-jobs"></a>İşleri yapılandırma

Azure IoT Central'da işler, birden çok cihazda aygıt veya bulut özelliği güncelleştirmelerini tetiklemenize olanak tanır. Özelliklere ek olarak, birden çok aygıtta aygıt komutlarını tetiklemek için işleri de kullanabilirsiniz. Azure IoT Central sizin için iş akışını otomatikleştirir.

1. Sol bölmede **İşler'i** seçin.
1. **+ Yeni'yi**seçin ve bir veya daha fazla işi yapılandırın.

## <a name="customize-your-application"></a>Uygulamanızı özelleştirin

Oluşturucu olarak, uygulamanızdaki kullanıcı deneyimini özelleştirmek için çeşitli ayarları değiştirebilirsiniz.

1. Uygulamanızı**Özelleştir'i** **Administration** > seçin.
1. **Uygulama logosu**olarak yüklemek için bir resim seçmek için **Değiştir** düğmesini seçin.
1. Tarayıcı sekmelerinde görünecek bir **Tarayıcı simgesi** resmi seçmek için **Değiştir** düğmesini seçin.
1. Ayrıca, HTML hexadecimal renk kodları ekleyerek varsayılan **Tarayıcı renklerini** değiştirebilirsiniz.

   ![Uygulama logosu, tarayıcı simgesi ve tarayıcı renkleri için seçimler](./media/tutorial-waterconsumptionmonitoring/waterconsumptionmonitoring-customize-your-application.png)

1. **Ayrıca Yönetim** > **Uygulama ayarlarını**seçerek uygulama görüntülerini değiştirebilirsiniz. Uygulama görüntüsü olarak yüklenebilmek için bir resim seçmek **için, resmi seç** düğmesini seçin.
1. Son olarak, uygulamanın sağ üst köşesindeki **Ayarlar** simgesini seçerek **Teonu** da değiştirebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz, silin.

1. Azure IoT Central uygulamanızın sol bölmesindeki **Yönetim'i** seçin.
1. **Uygulama ayarlarını**seçin ve ardından sayfanın altındaki **Sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Su tüketimi izleme kavramları](./concepts-waterconsumptionmonitoring-architecture.md)hakkında daha fazla bilgi edinin.
