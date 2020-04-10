---
title: Öğretici - Azure IoT Central'daki operatör panosunu özelleştirin
description: Bu öğretici, bir IoT Merkezi uygulamasında operatör panosunun nasıl özelleştirilebildiğini ve aygıtları nasıl yöneteceğimi gösterir.
services: iot-central
ms.service: iot-central
ms.subservice: iot-central-retail
ms.topic: tutorial
ms.custom:
- iot-storeAnalytics-checkout
- iot-p0-scenario
ms.author: timlt
author: timlt
ms.date: 11/12/2019
ms.openlocfilehash: 47edaec54a2470e9b657b2f214be923439e1e8a2
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "81000122"
---
# <a name="tutorial--customize-the-operator-dashboard-and-manage-devices-in-azure-iot-central"></a>Öğretici: Azure IoT Central'da operatör panosunu özelleştirin ve aygıtları yönetin


Bu eğitimde, bir oluşturucu olarak Azure IoT Central mağaza içi analiz uygulamanızdaki operatör panosunu nasıl özelleştireceğimiz öğrenin. Uygulama operatörleri, uygulamayı çalıştırmak ve bağlı aygıtları yönetmek için özelleştirilmiş panoyu kullanabilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Pano adını değiştirme
> * Panodaki resim kutucuklarını özelleştirme
> * Düzeni değiştirmek için kutucukları düzenleme
> * Koşulları görüntülemek için telemetri döşemeleri ekleme
> * Aygıt ayrıntılarını görüntülemek için özellik kutucukları ekleme
> * Komutları çalıştırmak için komut kutucukları ekleme

## <a name="prerequisites"></a>Ön koşullar

Oluşturucu, Azure IoT Merkezi mağaza içi analiz uygulamasını oluşturmak ve aygıt eklemek için öğreticiyi tamamlamalıdır:

* [Azure IoT Central'da mağaza içi bir analiz uygulaması oluşturun](./tutorial-in-store-analytics-create-app.md) (Gerekli)

## <a name="change-the-dashboard-name"></a>Pano adını değiştirme
Operatör panosunu özelleştirmek için, uygulamanızdaki varsayılan panoyu da sağlamanız gerekir. Ayrıca, ek yeni panolar oluşturabilirsiniz. Uygulamanızdaki panoyu özelleştirmenin ilk adımı, adı değiştirmektir.

1. [Azure IoT Central uygulama yöneticisi](https://aka.ms/iotcentral) web sitesine gidin.

1. Azure IoT Central öğreticisinde [mağaza içi analiz uygulaması oluşturun'da](./tutorial-in-store-analytics-create-app.md) oluşturduğunuz durum izleme uygulamasını açın.

1. Pano araç çubuğunda **Düzenleme'yi** seçin. Düzenle modunda, pano görünümünü, düzenini ve içeriğini özelleştirebilirsiniz.

    ![Azure IoT Merkezi edit panosu](./media/tutorial-in-store-analytics-customize-dashboard/dashboard-edit.png)

1. İsteğe bağlı olarak, sol bölmeyi gizleyin. Sol bölmeyi gizlemek, panoyu düzenlemek için daha büyük bir çalışma alanı sağlar.

1. **Pano adınıza** panonuz için dostça bir ad girin. Bu öğretici Contoso adlı kurgusal bir şirket kullanır ve örnek pano adı *Contoso pano.* 

1. **Kaydet'i**seçin. Değişiklikleriniz panoya kaydedilir ve değiştirme modu devre dışı bırakılır.

    ![Azure IoT Merkezi değişiklik pano adı](./media/tutorial-in-store-analytics-customize-dashboard/dashboard-change-name.png)

## <a name="customize-image-tiles-on-the-dashboard"></a>Panodaki resim kutucuklarını özelleştirme
Azure IoT Merkezi uygulama panosu bir veya daha fazla kutucamadan oluşur. Döşeme, panoda içerik görüntülemek için dikdörtgen bir kapsayıcıdır. Çeşitli içerik türlerini kutucuklarla ilişkilendirersiniz ve pano düzenini özelleştirmek için kutucukları sürükler, bırakır ve yeniden boyutlandırın. İçeriği görüntülemek için çeşitli kutucuk türleri vardır. Resim kutucukları görüntüler içerir ve kullanıcıların resmi tıklatmasını sağlayan bir URL ekleyebilirsiniz. Etiket döşemeleri düz metin görüntüler. İşaretleme kutucukları biçimlendirilmiş içerik içerir ve HTML olarak işlenen bir resim, url, başlık ve işaretleme kodu ayarlamanıza izin verir. Telemetri, özellik veya komut kutucukları aygıta özgü verileri görüntüler. 

Bu bölümde, panodaki resim kutucuklarını nasıl özelleştirdiğinizi öğrenirsiniz.

Panoda marka görüntüsü görüntüleyen görüntü döşemesini özelleştirmek için:

1. Pano araç çubuğunda **Düzenleme'yi** seçin. 

1. Northwind marka görüntüsünü görüntüleyen görüntü döşemesinde **Yapıla'yı** seçin. 

    ![Azure IoT Central marka imajını yeniden delendi](./media/tutorial-in-store-analytics-customize-dashboard/brand-image-edit.png)

1. **Başlığı**değiştirin. Başlık, bir kullanıcı resmin üzerinde gezinirken görüntülenir.

1. **Resim'i**seçin. Bir iletişim kutusu açılır ve özel bir resim yüklemenize olanak tanır. 

1. İsteğe bağlı olarak, resim için bir URL belirtin.

1. **Yapılandırmayı Güncelleştir'i**seçin. **Yapılandırmayı Güncelleştir** düğmesi panodaki değişiklikleri kaydeder ve düzenleme modunu etkin bırakır.

    ![Azure IoT Central marka imajını kaydet](./media/tutorial-in-store-analytics-customize-dashboard/brand-image-save.png)

1. İsteğe bağlı olarak, döşeme de **Yapıla** başlıklı **Belgeler'i**seçin ve destek içeriği için bir URL belirtin. 

Mağazadaki sensör bölgelerinin haritasını görüntüleyen görüntü döşemesini özelleştirmek için:

1. Varsayılan depo bölgesi eşlemi gösteren görüntü döşemesinde **Yapılandır'ı** seçin. 

1. **Resim'i**seçin ve bir mağaza bölgesi haritasının özel bir görüntüsünü yüklemek için iletişim kutusunu kullanın. 

1. **Yapılandırmayı Güncelleştir'i**seçin.

    ![Azure IoT Central tasarruf mağaza haritası](./media/tutorial-in-store-analytics-customize-dashboard/store-map-save.png)

    Örnek Contoso mağaza haritası dört bölge gösterir: iki ödeme bölgeleri, giyim ve kişisel bakım için bir bölge ve bakkaliye ve şarküteri için bir bölge. Bu eğitimde, telemetri sağlamak için sensörleri bu bölgelerle ilişkilendireceksiniz.

    ![Azure IoT Merkezi depolama bölgeleri](./media/tutorial-in-store-analytics-customize-dashboard/store-zones.png)

1. **Kaydet'i**seçin. 

## <a name="arrange-tiles-to-modify-the-layout"></a>Düzeni değiştirmek için kutucukları düzenleme
Panoözelleştirmede önemli bir adım, kullanışlı bir görünüm oluşturmak için kutucukları yeniden düzenlemektir. Uygulama operatörleri, aygıt telemetrisini görselleştirmek, aygıtları yönetmek ve mağazadaki koşulları izlemek için panoyu kullanır. Azure IoT Central, bir pano oluşturma uygulama oluşturucu görevini basitleştirir. Pano düzenle modu, kutucukeklemenizi, taşımanızı, yeniden boyutlandırmanızı ve silmenizi sağlar. **Mağaza içi analiz - ödeme** uygulama şablonu da bir pano oluşturma görevini basitleştirir. Bağlı sensörler ve kasalar, ödeme hattı sayımlarını ve çevre koşullarını görüntüleyen bir çalışma panosu düzeni sağlar.

Bu bölümde, özel bir düzen oluşturmak için mağaza içi **analiz - ödeme** uygulama şablonundaki panoyu yeniden düzenlersiniz.

Uygulamanızda kullanmayı planlamadığınız kutucukları kaldırmak için:

1. Pano araç çubuğunda **Düzenleme'yi** seçin. 

1. Aşağıdaki döşemeleri kaldırmak için **X Sil'i** seçin: **Tüm bölgelere dön**, mağaza **panosunu ziyaret et,** **bekleme süresi**ve Çıkış **3**ile ilişkili üç kutucuk. Contoso mağaza panosu bu kutucukları kullanmaz. 

    ![Azure IoT Merkezi silme kutucukları](./media/tutorial-in-store-analytics-customize-dashboard/delete-tiles.png)

1. Kalan pano kutucuklarını görüntülemek için kaydırın.

1. Aşağıdaki döşemeleri kaldırmak için **X Delete'i** seçin: **Isınma çıkış bölgesi,** **Soğuma çıkış bölgesi,** **Doluluk sensörü ayarları,** **Termostat sensörü ayarları**ve Çevre **koşulları.** 

   ![Azure IoT Central kalan kutucukları silme](./media/tutorial-in-store-analytics-customize-dashboard/delete-tiles-2.png)

1. **Kaydet'i**seçin. Kullanılmayan kutucukların kaldırılması, edit sayfasında yer boşaltılır ve işleçler için pano görünümünü basitleştirir.

1. Değişikliklerinizi panoda görüntüleyin.

   ![Kutucukları siledikten sonra Azure IoT Central](./media/tutorial-in-store-analytics-customize-dashboard/after-delete-tiles.png)

Kullanılmayan kutucukları kaldırdıktan sonra, düzenli bir düzen oluşturmak için kalan kutucukları yeniden düzenleyin. Yeni düzen, daha sonraki bir adımda eklediğiniz kutucuklar için alan içerir.

Kalan döşemeleri yeniden düzenlemek için:

1. **Edit'i**seçin.

1. **Doluluk firmware** döşemesini seçin ve **Doluluk** pil döşemesinin sağına sürükleyin.

1. **Termostat firmware** karosu seçin ve **Termostat** pil karosu sağına sürükleyin.

1. **Kaydet'i**seçin.

1. Düzen değişikliklerinizi görüntüleyin. 

    ![Azure IoT Merkezi firmware pil kutucukları](./media/tutorial-in-store-analytics-customize-dashboard/firmware-battery-tiles.png)

## <a name="add-telemetry-tiles-to-display-conditions"></a>Koşulları görüntülemek için telemetri döşemeleri ekleme
Pano düzenini özelleştirdikten sonra, telemetriyi göstermek için kutucuk eklemeye hazırsınız demektir. Bir telemetri döşemesi oluşturmak için bir aygıt şablonu ve aygıt örneği seçin ve ardından döşemede görüntülemek için aygıta özgü telemetriyi seçin. **Mağaza içi analiz - ödeme** uygulama şablonu, panoda birkaç telemetri döşemesi içerir. İki ödeme bölgesinde yer alan dört kutucuk, simüle edilmiş doluluk sensöründen telemetri görüntüler. **Kişiler trafik** döşemesi, iki ödeme bölgesinde sayımları gösterir. 

Bu bölümde, [Azure IoT Central](./tutorial-in-store-analytics-create-app.md) öğreticisinde mağaza içi analiz uygulaması oluştur'a eklediğiniz RuuviTag sensörlerinden çevresel telemetriyi göstermek için iki telemetri döşemesi daha eklersiniz. 

RuuviTag sensörlerinden çevresel verileri görüntülemek için kutucuk eklemek için:

1. **Edit'i**seçin.

1. Aygıt `RuuviTag` **şablon** listesinde seçin. 

1. İki RuuviTag sensöründen birinin **Aygıt örneğini** seçin. Contoso deposuörneğinde, `Zone 1 Ruuvi` Bölge 1 için bir telemetri döşemesi oluşturmak için seçin. 

1. Seçin `Relative humidity` `temperature` ve **Telemetri** listesinde. Bunlar, döşemedeki her bölge için görüntüleyen telemetri öğeleridir.

1. **Birleştir'i**seçin. 

    ![Azure IoT Merkezi RuuviTag döşemesi 1 eklemek](./media/tutorial-in-store-analytics-customize-dashboard/add-zone1-ruuvi.png)

    Seçilen sensör için kombine nem ve sıcaklık telemetrisini yeni bir döşeme görüntüleyebilir. 

1. RuuviTag sensörü için yeni döşemede **Yapıla'yı** seçin. 

1. **Başlığı** Bölge *1 ortamına*değiştirin. 

1. **Yapılandırmayı Güncelleştir'i**seçin.

1. İkinci sensör örneği için bir döşeme oluşturmak için önceki adımları yineleyin. **Başlığı** Bölge *2 ortamına* ayarlayın ve ardından **yapılandırmayı güncelleştir'i seçin.**

1. **Termostat firmware** karosu altında **Bölge 2 ortamı** başlıklı döşemesürükleyin. 

1. **Halk trafik** döşemesinin altındaki Bölge **1 ortamı** başlıklı döşemeyi sürükleyin. 

1. **Kaydet'i**seçin. Pano, iki yeni kutucukta bölge telemetrisini görüntüler.

    ![Azure IoT Merkezi tüm RuuviTag karoları](./media/tutorial-in-store-analytics-customize-dashboard/all-ruuvitag-tiles.png)

Yalnızca iki ödeme bölgesi için telemetri göstermek için **Kişiler trafik** döşemesini delemek için:

1. **Edit'i**seçin. 

1. **Kişiler trafik** döşemesi üzerinde **Yapılandır'ı** seçin.

1. **Telemetri'de** **1, count** **2**ve **count 3'u**seçin. 

1. **Yapılandırmayı Güncelleştir'i**seçin. Döşemedeki varolan yapılandırmayı temizler. 

1. **Kişiler trafik** döşemesi üzerinde yeniden **Yapıla'yı** seçin.

1. **Telemetri'de** **1 ve 2'yi** **seçin.** 

1. **Yapılandırmayı Güncelleştir'i**seçin. 

1. **Kaydet'i**seçin.  Güncelleştirilmiş pano, simüle edilmiş doluluk sensörüne dayanan yalnızca iki ödeme bölgeniz için geçerlidir.

    ![Azure IoT Merkezi insanlar iki şeritten trafik](./media/tutorial-in-store-analytics-customize-dashboard/people-traffic-two-lanes.png)

## <a name="add-property-tiles-to-display-device-details"></a>Aygıt ayrıntılarını görüntülemek için özellik kutucukları ekleme
Uygulama operatörleri, aygıtları yönetmek ve durumu izlemek için panoyu kullanır. Operatörlerin yazılım sürümünü görüntülemesini sağlamak için her RuuviTag için bir döşeme ekleyin. 

Her RuuviTag için bir özellik döşemesi eklemek için:

1. **Edit'i**seçin.

1. Aygıt `RuuviTag` **şablon** listesinde seçin. 

1. İki RuuviTag sensöründen birinin **Aygıt örneğini** seçin. Contoso deposuörneğinde, `Zone 1 Ruuvi` Bölge 1 için bir telemetri döşemesi oluşturmak için seçin. 

1. **Özellikler > Yazılım sürümünü**seçin.

1. **Birleştir'i**seçin. 

1. Yeni oluşturulan Yapılandırılan **Yazılım sürümünde** **Yapılandır'ı** seçin. 

1. **Başlığı** *Ruuvi 1 yazılım sürümü olarak değiştirin.*

1. **Yapılandırmayı Güncelleştir'i**seçin. 

1. Bölge 1 **ortam** döşemesinin altındaki **Ruuv 1 yazılım sürümünü** sürükleyin.

1. İkinci RuuviTag için bir yazılım sürümü özellik döşemesi oluşturmak için önceki adımları yineleyin. 

1. **Kaydet'i**seçin.  

    ![Azure IoT Merkezi RuuviTag özellik kutucukları](./media/tutorial-in-store-analytics-customize-dashboard/add-ruuvi-property-tiles.png)

## <a name="add-command-tiles-to-run-commands"></a>Komutları çalıştırmak için komut kutucukları ekleme
Uygulama operatörleri, komutları çalıştırarak aygıtları yönetmek için de panoyu kullanır. Panoya, aygıtta önceden tanımlanmış komutları çalıştıracak komut kutucukları ekleyebilirsiniz. Bu bölümde, işleçlerin Rigado ağ geçidini yeniden başlatmasını sağlamak için bir komut döşemesi eklersiniz. 

Ağ geçidini yeniden başlatmak için komut döşemesi eklemek için:

1. **Edit'i**seçin. 

1. Aygıt `C500` **şablon** listesinde seçin. Bu Rigado C500 ağ geçidi için şablondur. 

1. **Aygıt örneğinde**ağ geçidi örneğini seçin.

1. **Komut > Yeniden Başlat'ı** seçin ve mağaza haritasının yanındaki panoya sürükleyin. 

1. **Kaydet'i**seçin. 

1. Tamamlanmış Contoso panonuzu görüntüleyin. 

    ![Azure IoT Merkezi tam pano özelleştirme](./media/tutorial-in-store-analytics-customize-dashboard/completed-dashboard.png)

1. İsteğe bağlı olarak, ağ geçidinizdeki yeniden başlatma komutunu çalıştırmak için **yeniden başlat'ı** seçin.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Pano adını değiştirme
* Panodaki resim kutucuklarını özelleştirme
* Düzeni değiştirmek için kutucukları düzenleme
* Koşulları görüntülemek için telemetri döşemeleri ekleme
* Aygıt ayrıntılarını görüntülemek için özellik kutucukları ekleme
* Komutları çalıştırmak için komut kutucukları ekleme

Azure IoT Central mağaza içi analitik uygulamanızda panonu özelleştirdiğinize göre, önerilen bir sonraki adım şudur:

> [!div class="nextstepaction"]
> [Verileri dışa aktarın ve öngörüleri görselleştirin](./tutorial-in-store-analytics-export-data-visualize-insights.md)
