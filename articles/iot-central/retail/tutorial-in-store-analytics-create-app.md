---
title: Öğretici - Azure IoT Central'da mağaza içi analiz uygulaması oluşturun
description: Bu öğretici, IoT Central'da mağaza içi bir analitik perakende uygulamasının nasıl oluşturulacağımı gösterir. Bunu oluşturacak, özelleştireceğiniz ve sensör aygıtları ekleyeceğiz.
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
ms.openlocfilehash: 50dd6038a8642f13cea7840fff723a5cf12ce2dd
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "81000255"
---
# <a name="tutorial-create-an-in-store-analytics-application-in-azure-iot-central"></a>Öğretici: Azure IoT Central'da mağaza içi analiz uygulaması oluşturun



Öğretici, çözüm oluşturucularına Azure IoT Merkezi mağaza içi analiz uygulamasını nasıl oluşturacaklarını gösterir. Örnek uygulama bir perakende mağaza içindir. Bu, doluluk ve çevre koşullarını izlemek ve uyum sağlamak için ortak iş ihtiyacına bir çözümdür.

Oluşturduğunuz örnek uygulama üç gerçek cihaz içerir: bir Rigado Cascade 500 ağ geçidi ve iki RuuviTag sensörleri. Öğretici ayrıca, uygulama şablonuna dahil edilen simüle edilmiş doluluk sensörünün test amacıyla nasıl kullanılacağını da gösterir. Rigado C500 ağ geçidi, uygulamanızda iletişim merkezi olarak hizmet vermektedir. Mağazanızdaki sensörlerle iletişim kurar ve bulutla olan bağlantılarını yönetir. RuuviTag sıcaklık, nem ve basınç dahil olmak üzere telemetri sağlayan bir çevre sensörüdür. Simüle doluluk sensörü, bir mağazanın ödeme alanlarındaki hareketi ve varlığı izlemek için bir yol sağlar. 

Bu öğretici, Rigado ve RuuviTag aygıtlarını uygulamanıza bağlamak için yol tariflerini içerir. Başka bir ağ geçidiniz ve sensörleriniz varsa, uygulamanızı oluşturmak için adımları izlemeye devam edebilirsiniz. Öğretici aynı zamanda simüle RuuviTag sensörleri oluşturmak için nasıl gösterir. Simüle edilmiş sensörler, gerçek cihazlarınız yoksa uygulamayı oluşturmanıza olanak tanır. 

Ödeme ve durum izleme çözümünü üç bölümden oluşur:

* Koşulları izlemek için uygulamayı oluşturun ve cihazları bağlayın
* Operatörlerin cihazları izlemesini ve yönetmesini sağlamak için panoyu özelleştirin
* Mağaza yöneticilerinin analitiği çalıştırmasını ve öngörüleri görselleştirmesini sağlamak için veri dışa aktarmayı yapılandırın

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Bir perakende mağazası uygulaması oluşturmak için Azure IoT Merkezi **Mağaza İçi analizini - ödeme** şablonundan yararlanın
> * Uygulama ayarlarını özelleştirme
> * IoT aygıt şablonları oluşturma ve özelleştirme
> * Cihazları uygulamanıza bağlama
> * Koşulları izlemek için kurallar ve eylemler ekleme

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici serisi tamamlamak için, ihtiyacınız var:
* Azure aboneliği önerilir. İsteğe bağlı olarak 7 günlük ücretsiz deneme sürümü kullanabilirsiniz. Azure aboneliğiniz yoksa, [Azure kaydolma sayfasında](https://aka.ms/createazuresubscription)bir abonelik oluşturabilirsiniz.
* Bir ağ geçidi aygıtına ve iki çevresel sensöre erişim (eğitimde açıklandığı gibi isteğe bağlı olarak simüle edilmiş cihazları kullanabilirsiniz)
* Kullandığınız aygıtlar için aygıt şablonları (şablonlar öğreticide kullanılan tüm aygıtlar için sağlanır)

## <a name="create-an-application"></a>Uygulama oluşturma
Bu bölümde, şablondan yeni bir Azure IoT Merkezi uygulaması oluşturursunuz. Tam bir çözüm oluşturmak için öğretici serisi boyunca bu uygulamayı kullanacağız.

Yeni bir Azure IoT Central uygulaması oluşturmak için:

1. [Azure IoT Central uygulama yöneticisi](https://aka.ms/iotcentral) web sitesine gidin.

1. Azure aboneliğiniz varsa, erişmek için kullandığınız kimlik bilgileriyle oturum açın, aksi takdirde bir Microsoft hesabı kullanarak oturum açın:

    ![Kuruluş hesabınızı girin](./media/tutorial-in-store-analytics-create-app/sign-in.png)

1. Yeni bir Azure IoT Central uygulaması oluşturmaya başlamak için **Yeni Uygulama**'yı seçin.

1. **Perakende'yi**seçin.  Perakende sayfası birkaç perakende uygulama şablonu görüntüler.

Yeni bir mağaza içi analitik ödeme uygulaması oluşturmak için:  

1. Mağaza **içi analiz - ödeme** uygulama şablonu'nu seçin. Bu şablon, RuuviTag sensörleri dışında öğreticide kullanılan tüm aygıtlar için aygıt şablonlarını içerir. Şablon ayrıca, ödeme ve çevre koşullarını ve cihaz durumunu izlemek için bir operatör panosu da sağlar. 

1. İsteğe bağlı olarak, dostça bir **Uygulama adı**seçin. Bu uygulama Contoso adlı kurgusal bir perakende mağazası dayanmaktadır. Öğretici Uygulama **adı** *Contoso ödeme*kullanır. Uygulama şablonu kurgusal şirket Northwind dayanmaktadır. Bu eğitimde, uygulamayı nasıl özelleştirdiğinizi öğrenmek için Contoso'yu kullanırsınız.

    > [!NOTE]
    > Dostane bir **Uygulama adı**kullanıyorsanız, yine de uygulama **URL'si**için benzersiz bir değer kullanmanız gerekir.

1. Azure aboneliğiniz varsa, *Dizin, Azure aboneliğive Bölgegirin.* Aboneliğiniz yoksa, 7 günlük ücretsiz **deneme sürümünü** etkinleştirebilir ve gerekli iletişim bilgilerini tamamlayabilirsiniz.  

    Dizinler ve abonelikler hakkında daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](../core/quick-deploy-iot-central.md).

1. **Oluştur'u**seçin.

    ![Azure IoT Central Uygulama Oluştur sayfası](./media/tutorial-in-store-analytics-create-app/preview-application-template.png)

    ![Azure IoT Merkezi Uygulama Oluşturma fatura bilgileri](./media/tutorial-in-store-analytics-create-app/preview-application-template-billinginfo.png)

## <a name="customize-application-settings"></a>Uygulama ayarlarını özelleştirme

Oluşturucu olarak, uygulamanızdaki kullanıcı deneyimini özelleştirmek için çeşitli ayarları değiştirebilirsiniz. Bu bölümde, önceden tanımlanmış bir uygulama teması seçeceksiniz. İsteğe bağlı olarak, özel bir tema oluşturmayı ve uygulama görüntüsünü nasıl güncelleştireceğinizi öğreneceksiniz. Özel bir tema, uygulama tarayıcı renklerini, tarayıcı simgesini ve masthead'da görünen uygulama logosunu ayarlamanızı sağlar.

Önceden tanımlanmış bir uygulama teması seçmek için:

1. Maske başlığındaki **Ayarlar'ı** seçin.

    ![Azure IoT Merkezi uygulama ayarları](./media/tutorial-in-store-analytics-create-app/settings-icon.png)

2. Yeni bir **Tema**seçin.

3. **Kaydet'i**seçin.

Önceden tanımlanmış bir tema kullanmak yerine, özel bir tema oluşturabilirsiniz. Uygulamayı özelleştirmek ve öğreticiyi tamamlamak için bir dizi örnek resim kullanmak istiyorsanız, [Contoso örnek resimlerini](https://github.com/Azure-Samples/iot-central-docs-samples/tree/master/retail)indirin.

Özel bir tema oluşturmak için:

1. Zaten genişletilmediyse, sol bölmeyi genişletin.

    ![Azure IoT Merkezi sol bölme](./media/tutorial-in-store-analytics-create-app/dashboard-expand.png)

1. **Uygulamanızı Özelleştir> İdaresi'ni**seçin.

1. **Uygulama logosu**olarak yüklemek için bir resim seçmek için **Değiştir** düğmesini kullanın. İsteğe bağlı olarak, **Logo alt metin**için bir değer belirtin. 

1. Tarayıcı sekmelerinde görünecek bir **Tarayıcı simgesi** resmi seçmek için **Değiştir** düğmesini kullanın.

1. İsteğe bağlı olarak, HTML hexadecimal renk kodları ekleyerek varsayılan **Tarayıcı renklerini** değiştirin. **Üstbilgi**için, *#008575*ekleyin.  **Aksan**için, *#A1F3EA*ekleyin. 

1. **Kaydet'i**seçin. 

    ![Azure IoT Central özelleştirilmiş logo](./media/tutorial-in-store-analytics-create-app/select-application-logo.png)

    Siz kurtardıktan sonra, uygulama tarayıcı renklerini, maskedeki logoyu ve tarayıcı simgesini güncelleştirir. 

    ![Azure IoT Merkezi güncelleştirilmiş uygulama ayarları](./media/tutorial-in-store-analytics-create-app/saved-application-settings.png)

Uygulama görüntüsünü güncelleştirmek için:

1. **Yönetim > Uygulama ayarlarını**seçin.

1. Uygulama görüntüsü olarak yüklemek için bir resim seçmek için **Resmi Seç** düğmesini kullanın. Bu resim, IoT Central uygulama yöneticisinin **Uygulamalarım** sayfasındaki uygulama döşemesinde görünür.

1. **Kaydet'i**seçin.

1. İsteğe bağlı olarak, [Azure IoT Central uygulama yöneticisi](https://aka.ms/iotcentral) web sitesindeki **Uygulamalarım** görünümüne gidin. Uygulama döşemesi güncelleştirilmiş uygulama görüntüsünü görüntüler.

    ![Azure IoT Merkezi uygulama görüntüsünü özelleştirme](./media/tutorial-in-store-analytics-create-app/customize-application-image.png)

## <a name="create-device-templates"></a>Aygıt şablonları oluşturma
Oluşturucu olarak, sizin ve uygulama operatörlerinin aygıtları yapılandırmasını ve yönetmesini sağlayan aygıt şablonları oluşturabilirsiniz. Özel bir şablon oluşturarak, varolan bir şablon dosyasını içe aktararak veya Azure IoT aygıt kataloğundan şablon alarak şablon oluşturursunuz. Bir aygıt şablonu oluşturup özelleştirdikten sonra, gerçek aygıtları uygulamanıza bağlamak için bu şablonu kullanın. İsteğe bağlı olarak, sınama için simüle edilmiş aygıtlar oluşturmak için bir aygıt şablonu kullanın.

**Mağaza içi analiz - ödeme** uygulama şablonu, çeşitli aygıtlar için aygıt şablonlarına sahiptir.  Uygulamada kullandığınız üç aygıttan ikisi için aygıt şablonları vardır. RuuviTag aygıt şablonu mağaza içi **analiz - ödeme** uygulama şablonuna dahil değildir. Bu bölümde, uygulamanıza RuuviTag sensörleri için bir aygıt şablonu eklersiniz.

Uygulamanıza bir RuuviTag aygıt şablonu eklemek için:

1. Sol bölmede **Aygıt Şablonları'nı** seçin.

1. Yeni bir aygıt şablonu oluşturmak için **+ Yeni'yi** seçin.

1. Azure IoT aygıt kataloğundaki **RuuviTag** sensör aygıtı şablonuna bulun ve seçin. 

1. **Sonrakini Seçin: Özelleştirin.**

    ![Azure IoT Merkezi RuuviTag sensör cihaz şablonu](./media/tutorial-in-store-analytics-create-app/ruuvitag-device-template.png)

1. **Oluştur'u**seçin. Uygulama RuuviTag aygıt şablonu ekler.

1. Sol bölmede **Aygıt şablonlarını** seçin. Sayfa, uygulama şablonuna dahil olan tüm aygıt şablonlarını ve az önce eklediğiniz RuuviTag aygıt şablonlarını görüntüler.

    ![Azure IoT Merkezi RuuviTag sensör cihaz şablonu](./media/tutorial-in-store-analytics-create-app/device-templates-list.png)

## <a name="customize-device-templates"></a>Aygıt şablonlarını özelleştirme
Uygulamanızdaki aygıt şablonlarını üç şekilde özelleştirebilirsiniz. İlk olarak, aygıt özelliklerini değiştirerek aygıtlarınızdaki yerleşik arabirimleri özelleştirebilirsiniz. Örneğin, bir sıcaklık sensörüyle, sıcaklık arabiriminin görüntü adı, veri türü, ölçüm birimleri ve minimum ve maksimum çalışma aralıkları gibi ayrıntıları değiştirebilirsiniz. 

İkinci olarak, bulut özellikleri ekleyerek aygıt şablonlarınızı özelleştirin. Bulut özellikleri yerleşik aygıt özelliklerinin bir parçası değildir. Bulut özellikleri, Azure IoT Central uygulamanızın aygıtlarınızla oluşturduğu, depoladığını ve ilişkilendirdiyi özel verilerdir. Bulut özelliğinin bir örneği, hesaplanan bir değer veya bir aygıt kümesiyle ilişkilendirmek istediğiniz konum gibi meta veriler olabilir. 

Üçüncü olarak, özel görünümler oluşturarak aygıt şablonlarını özelleştirin. Görünümler, operatörlerin aygıtlarınız için aygıtlarınız için aygıtölçümleri ve sistem durumu gibi telemetri ve meta verileri görselleştirmeleri için bir yol sağlar.

Burada, RuuviTag sensörlerinizin cihaz şablonunu özelleştirmek için ilk iki yöntemi kullanırsınız. Sensörleriniz için görünüm oluşturma hakkında daha fazla bilgi [için, IoT Central uygulamanıza benzetilen bir aygıtı](../core/quick-create-simulated-device.md) hızlı bir şekilde başlatın.

RuuviTag aygıt şablonunun yerleşik arabirimlerini özelleştirmek için:

1. Sol bölmede **Aygıt Şablonları'nı** seçin. 

1. RuuviTag sensörleri için şablonu seçin. 

1. Sol bölmeyi sakla. Şablonun özet görünümü aygıt özelliklerini görüntüler.

    ![Azure IoT Central RuuviTag cihaz şablonözeti görünümü](./media/tutorial-in-store-analytics-create-app/ruuvitag-device-summary-view.png)

1. RuuviTag aygıt şablonu menüsünde **Özelleştir'i** seçin. 

1. Yetenekler listesinde ilerleyin ve `humidity` telemetri türünü bulun. Bu *nem*değiştirilebilir **Ekran adı** değeri ile satır öğesi.

Aşağıdaki adımlarda, RuuviTag sensörleri için `humidity` telemetri türünü özelleştirebilirsiniz. İsteğe bağlı olarak, diğer telemetri türlerinden bazılarını özelleştirin.

`humidity` Telemetri türü için aşağıdaki değişiklikleri yapın:

1. Satırın şema ayrıntılarını genişletmek için **Genişlet** denetimini seçin.

1. Görüntü **Adı** değerini *nemden* *Bağıl nem*gibi özel bir değere güncelleştirin.

1. **Anlamsal Tip** seçeneğini *Yok'tan* *Neme*değiştirin.  İsteğe bağlı olarak, genişletilmiş şema görünümünde nem telemetri türü için şema değerleri ayarlayın. Şema ayarları, sensörlerinizin izlediği veriler için ayrıntılı doğrulama gereksinimleri oluşturmanıza olanak tanır. Örneğin, belirli bir arabirim için minimum ve maksimum çalışma aralığı değerleri ayarlayabilirsiniz.

1. Değişikliklerinizi kaydetmek için **Kaydet** seçeneğini belirleyin.

    ![Azure IoT Central RuuviTag cihaz şablonu özelleştirme](./media/tutorial-in-store-analytics-create-app/ruuvitag-device-template-customize.png)

Uygulamanızdaki aygıt şablonuna bulut özelliği eklemek için:

1. RuuviTag aygıt şablonu menüsünde **Bulut Özellikleri'ni** seçin.

1. **Bulut Özelliği Ekle'yi**seçin. 

Her aygıtın konumunu depolamak için özel bir özellik oluşturmak için aşağıdaki değerleri belirtin:

1. **Görüntü Adı**için *Konum* değerini girin. Bu değer, özellik için uygun bir ad olan **Ad** alanına otomatik olarak kopyalanır. Kopyalanan değeri kullanabilir veya değiştirebilirsiniz.

1. **Şema** açılır düşüşünde *String'i* seçin. Dize türü, bir konum adı dizesini şablona dayalı herhangi bir aygıtla ilişkilendirmenizi sağlar. Örneğin, bir mağazadaki bir alanı her aygıtla ilişkilendirebilirsiniz. İsteğe bağlı olarak, mülkünüzdeki **Anlamsal Türü** *Konum*olarak ayarlayabilirsiniz ve bu da **Şemayı** otomatik olarak *Geopoint'e*ayarlar. GPS koordinatlarını bir cihazla ilişkilendirmenizi sağlar. 

1. **Minimum Uzunluğu** *2*olarak ayarlayın. 

1. **Trim Whitespace'i** **A'ya**ayarlayın.

1. Özel bulut özelliğinizi kaydetmek için **Kaydet'i** seçin.

    ![Azure IoT Central RuuviTag cihaz şablonu özelleştirme](./media/tutorial-in-store-analytics-create-app/ruuvitag-device-template-cloud-property.png)

1. **Yayımla**’yı seçin. 

    Aygıt şablonu yayımlamak, aygıt şablonu uygulama işleçleri tarafından görünür hale getirir. Bir şablon yayınladıktan sonra, sınama için simüle edilmiş aygıtlar oluşturmak veya uygulamanıza gerçek aygıtlar bağlamak için şablonu kullanın. Uygulamanıza zaten bağlı aygıtlarınız varsa, özelleştirilmiş bir şablonyayımlamak değişiklikleri aygıtlara iter.

## <a name="add-devices"></a>Cihazları ekleme
Aygıt şablonları oluşturduktan ve özelleştirdikten sonra, aygıt ekleme nin zamanı gelmiştir. 

Bu öğretici için, uygulamayı oluşturmak için aşağıdaki gerçek ve simüle edilmiş aygıtkümesini kullanırsınız:
- Gerçek bir Rigado C500 ağ geçidi
- İki gerçek RuuviTag sensörü
- Simüle edilmiş **doluluk** sensörü. Benzetimli sensör uygulama şablonuna dahildir, bu nedenle oluşturmanız gerekmez. 

> [!NOTE]
> Gerçek cihazlarınız yoksa, simüle edilmiş RuuviTag sensörleri oluşturarak bu eğitimi yine de tamamlayabilirsiniz. Aşağıdaki yönler, simüle edilmiş bir RuuviTag oluşturmak için adımları içerir. Benzetimli bir ağ geçidi oluşturmanız gerekmez.

Gerçek bir Rigado ağ geçidi ve RuuviTag sensörleri bağlamak için aşağıdaki iki makalede adımları tamamlayın. İşin izniniz bittikten sonra, bu öğreticiye geri dönün. Bu öğreticide aygıt şablonları oluşturduğunuziçin, bunları aşağıdaki yol tarifi kümesinde yeniden oluşturmanız gerekmez.

- Bir Rigado ağ geçidini bağlamak için Azure [IoT Merkezi uygulamanıza Rigado Cascade 500 bağlayın'a](../core/howto-connect-rigado-cascade-500.md?toc=/azure/iot-central/retail/toc.json&bc=/azure/iot-central/retail/breadcrumb/toc.json)bakın.
- RuuviTag sensörlerini bağlamak için Bkz. [Bir RuuviTag sensörünü Azure IoT Merkezi uygulamanıza bağlayın.](../core/howto-connect-ruuvi.md?toc=/azure/iot-central/retail/toc.json&bc=/azure/iot-central/retail/breadcrumb/toc.json) Gerekirse iki simüle sensör oluşturmak için de bu yönergeleri kullanabilirsiniz.

## <a name="add-rules-and-actions"></a>Kurallar ve eylemler ekleme
Koşulları izlemek için Azure IoT Central uygulamanızdaki sensörleri kullanmanın bir parçası olarak, belirli koşullar yerine getirildiğinde eylemleri çalıştırmak için kurallar oluşturabilirsiniz. Kural, aygıt şablonu ve bir veya daha fazla aygıtla ilişkilidir ve aygıt telemetrisi veya olaylara göre karşılanması gereken koşulları içerir. Bir kuralın da bir veya daha fazla ilişkili eylemi vardır. Eylemler arasında e-posta bildirimleri gönderilmesi veya diğer hizmetlere veri göndermek için bir webhook eyleminin tetikleilmesi yer alabilir. Mağaza içi analiz - ödeme uygulaması şablonu, uygulamadaki **aygıtlar** için önceden tanımlanmış bazı kurallar içerir.

Bu bölümde, RuuviTag sensör telemetrisini temel alan maksimum bağıl nem düzeyini kontrol eden yeni bir kural oluşturursunuz. Nem maksimumu aşarsa uygulamanın e-posta göndermesi için kurala bir eylem eklersiniz. 

Bir kural oluşturmak için: 

1. Sol bölmeyi genişletin.

1. **Kurallar'ı**seçin.

1. Seçin **+ Yeni**.

1. Kuralın adı olarak *Nem düzeyini* girin. 

1. Kapsamlar'daki RuuviTag aygıt **şablonunu**seçin. Tanımladığınız kural, bu şablona dayalı tüm sensörler için geçerli olacaktır. İsteğe bağlı olarak, kuralı yalnızca sensörlerin tanımlı bir alt kümesine uygulayacak bir filtre oluşturabilirsiniz. 

1. `Relative humidity` **Telemetri**olarak seçin. Önceki adımda özelleştirdiğiniz aygıt özelliğidir.

1. Operatör `Is greater than` olarak **Operator**seçin. 

1. **Değer**olarak ortamınız için tipik bir üst aralık iç nem düzeyi girin. Örneğin, *65*girin. Herhangi bir RuuviTag gerçek veya simüle sensör bağıl nem bu değeri aştığında oluşur kural için bir koşul belirledik. Ortamınızdaki normal nem aralığına bağlı olarak değeri yukarı veya aşağı ayarlamanız gerekebilir.  

   ![Azure IoT Merkezi ekleme kuralı koşulları](./media/tutorial-in-store-analytics-create-app/rules-add-conditions.png)

Kurala eylem eklemek için:

1. Seçin **+ E-posta**. 

1. Eylem için uygun Ekran **adı** olarak *Yüksek nem bildirimi* girin. 

1. **Hesabınızla**ilişkili e-posta adresini To 'ya girin. Farklı bir e-posta kullanıyorsanız, kullandığınız adres uygulamaya eklenen bir kullanıcı için olmalıdır. Kullanıcının da en az bir kez oturum açma ve çıkma yapması gerekir.

1. İsteğe bağlı olarak, e-postametnine eklemek için bir not girin.

1. Eylemi tamamlamak için **Bitti'yi** seçin.

   ![Azure IoT Central kurallara eylem ekleme](./media/tutorial-in-store-analytics-create-app/rules-add-action.png)

1. Kaydetmek ve yeni kuralı etkinleştirmek için **Kaydet'i** seçin. 

    Birkaç dakika içinde, belirtilen e-posta hesabı e-posta almaya başlamalıdır. Uygulama, her sensör nem seviyesinin durumunuzdaki değeri aştığını gösterdiğinde e-posta gönderir.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Bir perakende mağazası uygulaması oluşturmak için Azure IoT Merkezi **Mağaza İçi analizini - ödeme** şablonundan yararlanın
* Uygulama ayarlarını özelleştirme
* IoT aygıt şablonları oluşturma ve özelleştirme
* Cihazları uygulamanıza bağlama
* Koşulları izlemek için kurallar ve eylemler ekleme

Artık bir Azure IoT Merkezi koşul izleme uygulaması oluşturduğunuza göre, önerilen bir sonraki adım aşağıda veda edebilirsiniz:

> [!div class="nextstepaction"]
> [Operatör panosunu özelleştirme](./tutorial-in-store-analytics-customize-dashboard.md)
