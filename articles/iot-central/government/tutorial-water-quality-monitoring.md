---
title: 'Öğretici: Azure IoT Central ile su kalitesi izleme uygulaması oluşturun'
description: 'Öğretici: Azure IoT Central uygulama şablonlarını kullanarak su kalitesi izleme uygulamasını nasıl oluşturabilirsiniz öğrenin.'
author: miriambrus
ms.author: miriamb
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: d738868e0e4ca7599f4aaf8d6e09d22f26a8db92
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77016383"
---
# <a name="tutorial-create-a-water-quality-monitoring-application-in-azure-iot-central"></a>Öğretici: Azure IoT Central'da su kalitesi izleme uygulaması oluşturun



Bu öğretici, Azure IoT Central'da bir su kalitesi izleme uygulaması oluşturmakonusunda size rehberlik eder. Uygulamayı Azure IoT Merkezi **Su kalite izleme** uygulama şablonundan oluşturursunuz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Su **kalitesi** izleme uygulaması oluşturmak için Su kalitesi izleme şablonunu kullanın.
> * Operatör panosunun keşfedin ve özelleştirin.
> * Su kalitesi izleme cihazı şablonu keşfedin.
> * Simüle edilmiş aygıtları keşfedin.
> * Kuralları keşfedin ve yapılandırır.
> * İşleri yapılandırın.
> * Beyaz etiketleme kullanarak uygulama markasını özelleştirin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için bir Azure aboneliğiniz olmasını öneririz. Azure aboneliğiniz yoksa, [Azure kaydolma sayfasında](https://aka.ms/createazuresubscription)bir abonelik oluşturabilirsiniz.

## <a name="create-a-water-quality-monitoring-application-in-azure-iot-central"></a>Azure IoT Central'da su kalitesi izleme uygulaması oluşturun

Bu bölümde, bir su kalitesi izleme uygulaması oluşturmak için Azure IoT Merkezi **Su kalite izleme** şablonunu kullanırsınız.

1. [Azure IoT Merkezi giriş sayfasına](https://aka.ms/iotcentral)gidin.

    Azure aboneliğiniz varsa, bu aboneye erişmek için kullandığınız kimlik bilgileriyle oturum açın. Aksi takdirde, bir Microsoft hesabıyla oturum açın:

    ![Kuruluş hesabınızda oturum açma](./media/tutorial-waterqualitymonitoring/sign-in.png)

1. Azure IoT Central'ın en sol bölmesine **Yap'ı** seçin ve **Devlet** sekmesini seçin. Hükümet bölmesi birkaç hükümet uygulama şablonu gösterir.

    ![Devlet uygulama şablonları](./media/tutorial-waterqualitymonitoring/iotcentral-government-tab-overview1.png)

1. Su **kalitesi izleme** uygulama şablonu'nu seçin. Bu uygulama şablonu su kalitesi aygıt şablonu, simüle edilmiş aygıtlar, operatör panosu ve önceden yapılandırılmış izleme kuralları içerir.

1. **Uygulama oluştur**'u seçin. **Yeni uygulama** bölmesi açılır ve aşağıdaki öğeleri gösterir:

    * **Uygulama adı**: Varsayılan olarak, uygulama adı Azure IoT Central'ın oluşturduğu benzersiz bir kimlik dizesi tarafından izlenen **Su kalitesi izlemedir.** İsterseniz, bir görüntü adı girebilir veya daha sonra uygulama adını değiştirebilirsiniz.
    * **URL**: İstediğiniz URL'yi girebilir veya URL değerini daha sonra değiştirebilirsiniz.
    * Azure aboneliğiniz varsa, **Dizin,** **Azure aboneliği**ve **Bölge**için değerleri girin. Aboneliğiniz yoksa, **7 günlük ücretsiz deneme sürümünü** açabilir ve gerekli iletişim bilgilerini tamamlayabilirsiniz.

    Dizinler ve abonelikler hakkında daha fazla bilgi için uygulama hızlı [başlat'ı oluştur'a](../core/quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) bakın.

1. Sayfanın sol alt kısmındaki **Oluştur** düğmesini seçin.

    ![Azure IoT Merkezi yeni uygulama sayfası](./media/tutorial-waterqualitymonitoring/new-application-waterqualitymonitoring1.png)

    ![Azure IoT Merkezi yeni uygulama fatura bilgileri](./media/tutorial-waterqualitymonitoring/new-application-waterqualitymonitoring1-billinginfo.png)

Azure IoT Merkezi **Su kalitesi izleme** şablonu kullanarak bir su kalitesi izleme uygulaması oluşturdunuz.

Yeni uygulamanız şu önceden yapılandırılmış bileşenlerle birlikte gelir:

* Operatör panoları
* Su kalitesi izleme cihazı şablonları
* Simüle su kalitesi izleme cihazları
* Kurallar ve işler
* Beyaz etiketleme kullanan markalama

Uygulamanızı istediğiniz zaman değiştirebilirsiniz.

Ardından, uygulamayı keşfedin ve bazı özelleştirmeler yapın.

## <a name="explore-and-customize-the-operator-dashboard"></a>Operatör panosunu keşfedin ve özelleştirin

Uygulamayı oluşturduktan sonra, **Geniş Dünya su kalitesi pano** bölmesi açılır.

   ![Su kalitesi izleme panosu](./media/tutorial-waterqualitymonitoring/waterqualitymonitoring-dashboard1.png)

Bir oluşturucu olarak, operatörler tarafından kullanılmak üzere panoda görünümler oluşturabilir ve özelleştirebilirsiniz. Ancak özelleştirmeye çalışmadan önce, önce panoyu keşfedin.

Panoda gösterilen tüm veriler, bir sonraki bölümde tartışılan simüle edilmiş aygıt verilerine dayanır.

Pano aşağıdaki türde karoları içerir:

* **Wide World su yardımcı görüntü döşemesi**: Panonun sol üst köşesindeki ilk döşeme, Wide World adlı hayali programı gösteren bir görüntüdür. Kendi resminizi kullanmak için döşemeyi özelleştirebilir veya döşemeyi kaldırabilirsiniz.

* **Ortalama pH KPI karoları**: **Son 30 dakikada ortalama pH** gibi KPI karoları pano bölmesinin en üstündedir. KPI kutucuklarını özelleştirebilir ve her birini farklı bir tür ve zaman aralığına ayarlayabilirsiniz.

* **Su izleme alanı haritası**: Azure IoT Central, cihazınızın konumunu göstermek için doğrudan uygulamanızda ayarlayabileceğiniz Azure Haritaları kullanır. Ayrıca, uygulamanızdan cihazınıza kadar konum bilgilerini eşleyebilir ve bilgileri haritada göstermek için Azure Haritalar'ı kullanabilirsiniz. Haritanın üzerine gel ve kontrolleri dene.

* **Ortalama pH dağılımı ısı haritası grafiği**: Cihazınızın telemetrisini uygulamanız için en uygun şekilde göstermek için farklı görselleştirme grafikleri seçebilirsiniz.

* **Kritik kalite göstergeleri çizgi grafiği**: Bir zaman aralığı nda çizgi grafiği olarak çizilen aygıt telemetrisini görselleştirebilirsiniz.  

* **Kimyasal maddeler çubuğu grafiğinin konsantrasyonu**: Cihaz telemetrisini çubuk grafikte görselleştirebilirsiniz.

* **Eylem düğmesi**: Pano, bir operatörün doğrudan izleme panosundan başlatabileceği eylemler için bir döşeme içerir. Aygıtın özelliklerini sıfırlamak, bu tür eylemlere bir örnektir.

* **Özellik listesi kutucukları**: Panoda eşik bilgilerini, aygıt sistem durumu bilgilerini ve bakım bilgilerini temsil eden birden çok özellik kutucuğu bulunur.

### <a name="customize-the-dashboard"></a>Panoyu özelleştirme

Oluşturucu olarak, operatörler tarafından kullanılmak üzere panodaki görünümleri özelleştirebilirsiniz.

1. **Geniş Dünya su kalitesi pano** bölmesini özelleştirmek için **Edit'i** seçin. **Düzenleme** menüsündeki komutları seçerek panoyu özelleştirebilirsiniz. Pano edit modunda olduktan sonra, yeni kutucuklar ekleyebilir veya varolan dosyaları yapılandırabilirsiniz.

    ![Panonuzu edin](./media/tutorial-waterqualitymonitoring/edit-dashboard.png)

1. Yapılandırabileceğiniz yeni bir pano oluşturmak için **+ Yeni'yi** seçin. Birden çok panoya sahip olabilir ve pano menüsünden bunlar arasında gezinebilirsiniz.

## <a name="explore-a-water-quality-monitoring-device-template"></a>Su kalitesi izleme cihazı şablonu keşfedin

Azure IoT Central'daki aygıt şablonu, bir aygıtın özelliklerini tanımlar. Kullanılabilir özellikler telemetri, özellikler ve komutlardır. Oluşturucu olarak, Azure IoT Central'da bağlı aygıtların yeteneklerini temsil eden aygıt şablonlarını tanımlayabilirsiniz. Ayrıca, aygıt şablonunuzu ve uygulamanızı sınamak için simüle edilmiş aygıtlar da oluşturabilirsiniz.

Oluşturduğunuz su kalitesi izleme uygulaması, su kalitesi izleme cihazı şablonuyla birlikte gelir.

Aygıt şablonu görüntülemek için:

1. Azure IoT Central'da uygulamanızın en sol bölmesinde **Aygıt şablonlarını** seçin.
1. Cihaz şablonları listesinden **Su Kalitesi Monitörü'nü**seçin. Bu aygıt şablonu açılır.

    ![Aygıt şablonu](./media/tutorial-waterqualitymonitoring/waterqualitymonitoring-devicetemplate.png)

### <a name="customize-the-device-template"></a>Aygıt şablonu özelleştirme

Aşağıdaki aygıt şablonu ayarlarını özelleştirme alıştırması yapın:

1. Aygıt şablonu menüsünden **Özelleştir'i**seçin.
1. **Sıcaklık** telemetri türüne gidin.
1. Görüntü **adı** değerini **Bildirilen sıcaklıkla değiştirin.**
1. Ölçü birimini değiştirin veya **Min değerini** ve **Max değerini**ayarlayın.
1. **Kaydet'i**seçin.

#### <a name="add-a-cloud-property"></a>Bulut özelliği ekleme

1. Aygıt şablonu menüsünden **Bulut özelliğini**seçin.
1. Yeni bir bulut özelliği eklemek için **+ Bulut Özelliği Ekle'yi**seçin. Azure IoT Central'da, aygıtla alakalı ancak aygıt tarafından gönderilmesi beklenmeyen bir özellik ekleyebilirsiniz. Böyle bir özelliğin bir örneği, yükleme alanına, varlık bilgilerine veya bakım bilgilerine özgü bir uyarı eşiğidir.
1. **Kaydet'i**seçin.

### <a name="explore-views"></a>Görünümleri keşfedin

Su kalitesi izleme cihazı şablonu önceden tanımlanmış görünümlerle birlikte gelir. Görünümler, işleçlerin aygıt verilerini nasıl gördüğünü ve bulut özelliklerini nasıl ayarladığını tanımlar. Görünümleri keşfedin ve değişiklik yapma alıştırması yapın.

  ![Aygıt şablonu görünümleri](./media/tutorial-waterqualitymonitoring/waterqualitymonitoring-devicetemplate-views.png)

### <a name="publish-the-device-template"></a>Aygıt şablonu yayımlama

Herhangi bir değişiklik yaparsanız, aygıt şablonunu yayımlamak için **Yayımla'yı** seçtiğinizden emin olun.

### <a name="create-a-new-device-template"></a>Yeni cihaz şablonu oluşturma

1. Yeni bir aygıt şablonu oluşturmak ve oluşturma işlemini izlemek için **+ Yeni'yi** seçin.
1. Azure IoT aygıt kataloğundan özel bir aygıt şablonu oluşturun veya aygıt şablonu seçin.

## <a name="explore-simulated-devices"></a>Simüle edilmiş cihazları keşfedin

Uygulama şablonundan oluşturduğunuz su kalitesi izleme uygulaması, iki simüle cihaza sahiptir. Bu aygıtlar, su kalitesi izleme cihazı şablonuyla eşlenir.

### <a name="view-the-devices"></a>Aygıtları görüntüleme

1. Uygulamanızın en sol bölmesindeki **Cihazları** seçin.

   ![Cihazlar](./media/tutorial-waterqualitymonitoring/waterqualitymonitoring-devices.png)

1. Bir simüle aygıt seçin.

    ![Aygıtı seçin 1](./media/tutorial-waterqualitymonitoring/waterqualitymonitor-device1.png)

1. Bulut **Özellikleri** sekmesinde, **Asitlik (pH) eşik** değerini **8'den** **9'a**değiştirin.
1. Aygıt **Özellikleri** sekmesini ve **Aygıt Panosu** sekmesini keşfedin.

> [!NOTE]
> Tüm sekmeler Aygıt **şablonu görünümlerinden**yapılandırıldı.

### <a name="add-new-devices"></a>Yeni aygıtlar ekleme

**Aygıtlar** sekmesinde, yeni bir aygıt eklemek için **+ Yeni'yi** seçin.

## <a name="explore-and-configure-rules"></a>Kuralları keşfedin ve yapılandırır

Azure IoT Central'da, aygıt telemetrisini otomatik olarak izleyen kurallar oluşturabilirsiniz. Bu kurallar, herhangi bir koşul yerine getirildiğinde bir eylemi tetikler. Olası bir eylem e-posta bildirimleri göndermektir. Diğer olasılıklar arasında, diğer hizmetlere veri göndermek için bir Microsoft Flow eylemi veya webhook eylemi yer almaktadır.

Oluşturduğunuz su kalitesi izleme uygulamasının önceden yapılandırılmış iki kuralı vardır.

### <a name="view-rules"></a>Kuralları görüntüleme

1. Uygulamanızın en sol bölmesindeki **Kurallar'ı** seçin.

   ![Kurallar](./media/tutorial-waterqualitymonitoring/waterqualitymonitoring-rules.png)

1. Uygulamada önceden yapılandırılmış kurallardan biri olan **Yüksek pH uyarısı'nı**seçin.

   ![Yüksek pH uyarı kuralı](./media/tutorial-waterqualitymonitoring/waterqualitymonitoring-highphalert.png)

   **Yüksek pH uyarı** kuralı, asitlik durumunu (pH) 8'den büyük olacak şekilde denetlemek üzere yapılandırılır.

Ardından, kurala bir e-posta eylemi ekleyin:

1. Seçin **+ E-posta**.
1. Ekran **adı** kutusuna **Yüksek pH uyarısı**girin.
1. To **box'a** Azure IoT Merkezi hesabınızla ilişkili e-posta adresini girin.
1. İsteğe bağlı olarak, e-posta metnine eklemek için bir not girin.
1. Eylemi tamamlamak için **Bitti'yi** seçin.
1. Kaydetmek ve yeni kuralı etkinleştirmek için **Kaydet'i** seçin.

Birkaç dakika içinde, yapılandırılan koşul yerine getirildiğinde e-posta almanız gerekir.

> [!NOTE]
> Uygulama, bir koşul her karşılaştığında e-posta gönderir. Bu kuraldan otomatik e-posta almayı durdurmak için bir kural için **Devre Dışı Bırak'ı** seçin.
  
Yeni bir kural oluşturmak için, uygulamanızın en sol bölmesindeki **Kurallar'ı** seçin ve ardından **+Yeni'yi**seçin.

## <a name="configure-jobs"></a>İşleri yapılandırma

Azure IoT Merkezi işleri ile, birden çok cihazda aygıt veya bulut özellikleri güncelleştirmelerini tetikleyebilirsiniz. Birden çok cihazda aygıt komutlarını tetiklemek için işleri de kullanabilirsiniz. Azure IoT Central sizin için iş akışını otomatikleştirir.

1. Uygulamanızın en sol bölmesindeki **İşler'i** seçin.
1. **+Yeni'yi** seçin ve bir veya daha fazla işi yapılandırın.

## <a name="customize-your-application"></a>Uygulamanızı özelleştirin

Oluşturucu olarak, uygulamanızdaki kullanıcı deneyimini özelleştirmek için çeşitli ayarları değiştirebilirsiniz.

1. Uygulamanızı**Özelleştir'i** **Administration** > seçin.
1. **Uygulama logosunun**altında, logo olarak yükleyecek resmi seçmek için **Değiştir'i** seçin.
1. **Tarayıcı simgesinin**altında, tarayıcı sekmelerinde görünen görüntüyü seçmek için **Değiştir'i** seçin.
1. **Tarayıcı renkleri**altında, varsayılan değerleri HTML hexadecimal renk kodları ile değiştirebilirsiniz.
1. **Tema'nın**değerini değiştirmek için **Ayarlar'ı** seçin.

   ![Uygulamanızı özelleştirin](./media/tutorial-waterqualitymonitoring/waterqualitymonitoring-customize-your-application1.png)

### <a name="update-the-application-image"></a>Uygulama görüntüsünü güncelleştirme

1. **Yönetim** > **Uygulama ayarlarını**seçin.

1. Uygulama görüntüsü olarak yüklemek için bir resim seçmek için **Resmi Seç** düğmesini kullanın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulamanızı kullanmaya devam etmeyecekseniz, aşağıdaki adımlarla uygulamayı silin:

1. Uygulamanızın en sol bölmesindeki **Yönetim** sekmesini açın.
1. **Uygulama ayarlarını** seçin ve **Sil** düğmesini seçin.

    ![Uygulamanızı silme](./media/tutorial-waterqualitymonitoring/waterqualitymonitoring-application-settings-delete-app1.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Su kalitesi izleme kavramları](./concepts-waterqualitymonitoring-architecture.md)hakkında daha fazla bilgi edinin.
