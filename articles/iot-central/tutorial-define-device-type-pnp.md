---
title: Azure IoT Central'da yeni bir cihaz türü tanımlama | Microsoft Docs
description: Bu öğreticide, Azure IoT Central uygulamanızda yeni bir cihaz türü oluşturma, Oluşturucu olarak gösterilir. Türü için telemetri, durum, özellik ve komutları tanımlarsınız.
author: dominicbetts
ms.author: dobett
ms.date: 08/06/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 877769041d1587c6c1b0f4dbaff51d5a1a7924b5
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69997705"
---
# <a name="tutorial-define-a-new-device-type-in-your-azure-iot-central-application-preview-features"></a>Öğretici: Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Bu öğreticide, bir oluşturucu olarak Microsoft Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlamak için cihaz şablonunu nasıl tanımlayacağınız gösterilir. Cihaz şablonu, cihazınızın yeteneklerini tanımlar. Özellikler cihaz gönderme, cihaz özellikleri ve bir cihazın yanıt verdiği komutları içerir.

Bu öğreticide, bir **ortam algılayıcısı** cihaz şablonu oluşturacaksınız. Bir ortam algılayıcı cihazı:

* Sıcaklık gibi telemetri gönderir.
* , Parlaklık düzeyi gibi cihaza özgü özellikleri raporlar.
* Açma ve kapatma gibi komutlara yanıt verir.
* Bellenim sürümü ve seri numarası gibi genel cihaz özelliklerini raporlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yeni bir cihaz şablonu oluşturun.
> * Bir cihaz yetenek modeli içeri aktarın.
> * Bulut özellikleri oluşturun.
> * Cihaz telemetrisi için görselleştirme tanımlayın.
> * Cihaz şablonunuzu yayımlayın.
> * Cihaz şablonunuz için bir sanal cihaz oluşturun.
> * Sanal cihazınızı görüntüleyin.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için bir Azure IoT Central uygulamanızın olması gerekir. [Azure IoT Central uygulamasını oluşturma](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) hızlı başlangıcını tamamladıysanız, hızla başlangıçta oluşturduğunuz uygulamayı yeniden kullanabilirsiniz. Aksi takdirde, aşağıdaki adımları tamamlayarak boş bir Azure IoT Central uygulaması oluşturun:

1. Azure IoT Central [Uygulama Yöneticisi](https://aka.ms/iotcentral) sayfasına gidin.

1. Azure aboneliğiniz varsa, ona erişmek için kullandığınız kimlik bilgileriyle oturum açın, aksi takdirde Microsoft hesabı kullanarak imzalayın:

    ![Kuruluş hesabınızı girin](./media/tutorial-define-device-type-pnp/sign-in.png)

1. Yeni bir Azure IoT Central uygulaması oluşturmaya başlamak için **Yeni Uygulama**'yı seçin.

IoT Tak ve Kullan de dahil olmak üzere önizleme özelliklerini kullanan yeni bir Azure IoT Central uygulaması oluşturmak için:

1. **Deneme**’yi seçin. Deneme uygulaması oluşturmak için bir Azure aboneliğine ihtiyacınız yoktur.

    Dizinler ve abonelikler hakkında daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

1. **Önizleme uygulaması**' nı seçin.

1. İsteğe bağlı olarak **Contoso Klimaları** gibi kolay bir uygulama adı seçebilirsiniz. Azure IoT Central sizin için benzersiz bir URL ön eki oluşturur. Bu URL ön ekini daha akılda kalır bir şeyle değiştirebilirsiniz.

1. Deneme uygulaması oluşturuyorsanız, iletişim ayrıntılarını sağlamanız gerekir.

1. **Oluştur**’u seçin.

    ![Azure IoT Central Uygulama Oluştur sayfası](./media/tutorial-define-device-type-pnp/iotcentralcreate.png)

    Daha fazla bilgi için bkz. [Uygulama oluşturma hızlı başlangıcı](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

[Iot Tak ve kullan](../iot-pnp/overview-iot-plug-and-play.md) cihaz yetenek modelini Içeren **Environmentalsensorline. capabilitymodel. JSON** dosyasının yerel bir kopyasına de ihtiyacınız vardır. [Buradan](https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/EnvironmentalSensorInline.capabilitymodel.json)indirebilirsiniz. Sayfaya sağ tıklayın ve **farklı kaydet**' i seçin.

Dosyayı indirdikten sonra bir metin düzenleyicisinde açın ve iki örneğini `<YOUR_COMPANY_NAME_HERE>` adıyla değiştirin. Yalnızca a-z, A-Z, 0-9 ve alt çizgi karakterlerini kullanın.

## <a name="create-a-template"></a>Şablon oluşturma

Bir Oluşturucu olarak, uygulamanızda cihaz şablonları oluşturabilir ve düzenleyebilirsiniz. Bir cihaz şablonu yayımladıktan sonra, sanal cihaz oluşturabilir veya cihaz şablonunu uygulayan gerçek cihazları bağlayabilirsiniz. Sanal cihazlar, gerçek bir cihazı bağlanmadan önce uygulamanızın davranışını test etmenize olanak sağlar.

Uygulamanıza yeni bir cihaz şablonu eklemek için **cihaz şablonları** sayfasına gidin. Bunu yapmak için sol gezinti menüsünde **cihaz şablonları** sekmesini seçin.

![Cihaz şablonları sayfası](./media/tutorial-define-device-type-pnp/devicedefinitions.png)

### <a name="add-a-device-capability-model"></a>Cihaz yetenek modeli ekleme

IoT Central bir cihaz yetenek modeli yazmak için çeşitli seçenekler vardır. Sıfırdan özel bir model oluşturmayı, bir dosyadan içeri aktarmayı, cihaz kataloğundan seçim yapmak ya da cihaz yetenek modelinin ortak depoda yayımlandığı cihaz-ilk bağlantı aracılığıyla bir IoT Tak ve Kullan cihazını bağlamayı seçebilirsiniz. Bu öğreticide bir cihaz yetenek modelini bir dosyadan içeri aktarırsınız.

Aşağıdaki adımlarda, bir **ortam algılayıcı** cihazı için yetenek modelinin nasıl içeri aktarılacağı gösterilmektedir. Bu cihazlar, uygulamanıza sıcaklık gibi telemetri gönderir:

1. Yeni bir cihaz şablonu eklemek için **cihaz şablonları** sayfasında **+ Yeni** ' yi seçin.

1. Şablonlar listesinden **özel** ' i seçin.

1. Cihaz şablonunuzun adı olarak **çevresel algılayıcı** girin.

1. Bir JSON dosyasından yeni bir cihaz yetenek modeli oluşturmak için **yetenek modeli Içeri aktar** ' ı seçin. Yerel makinenizde **Environmentalsensorline. capabilitymodel. JSON** dosyasını kaydettiğiniz klasöre gidin. **Environmentalsensorline. capabilitymodel. JSON** dosyasını seçin ve **Aç**' ı seçin. Çevresel algılayıcı yetenek modeli, **ortam algılayıcısı** ve **cihaz bilgileri** arabirimlerini içerir:

    ![Çevresel algılayıcı cihaz yeteneği modeli](./media/tutorial-define-device-type-pnp/newdevicecapabilitymodel.png)

    Bu arabirimler, bir **ortam algılayıcısı** cihazının yeteneklerini tanımlar. Özellikler, bir cihazın gönderdiği Telemetriyi, bir cihaz rapor özelliklerini ve bir cihazın yanıt verdiği komutları içerir.

### <a name="add-cloud-properties"></a>Bulut özellikleri ekle

Bir cihaz şablonu, bulut özellikleri içerebilir. Bulut özellikleri yalnızca IoT Central uygulamasında bulunur ve bir cihaza hiçbir şekilde gönderilmez veya buradan alınmaz.

1. **Bulut özellikleri** ' ni seçin ve **+ bulut özelliği Ekle**' ye tıklayın. Cihaz şablonunuza bir bulut özelliği eklemek için aşağıdaki tablodaki bilgileri kullanın.

    | Görünen Ad      | Anlam Türü | Şema |
    | ----------------- | ------------- | ------ |
    | Son Hizmet Tarihi | Yok.          | Date   |
    | Müşteri adı     | Yok.          | Dize |

1. Değişikliklerinizi kaydetmek için **Kaydet** ' i seçin:

    ![Bulut özellikleri](media/tutorial-define-device-type-pnp/cloudproperties.png)

### <a name="add-customizations"></a>Özelleştirmeler ekleme

Bir arabirimi değiştirmeniz veya cihaz yetenek modelinizi sürümünüzü gerektirmeyen bir işleve IoT Central özgü özellikler eklemeniz gerektiğinde özelleştirmeleri kullanın. Yetenek modeli taslak veya Yayımlanma durumundaysa alanları özelleştirebilirsiniz. Yalnızca arabirim uyumluluğunu kesen alanları özelleştirebilirsiniz. Örneğin, şunları yapabilirsiniz:

- Bir özelliğin görünen adını ve birimlerini özelleştirin.
- Değer bir grafikte göründüğünde kullanılacak varsayılan rengi ekleyin.
- Bir özellik için başlangıçtaki, minimum ve maksimum değerleri belirtin.

Yetenek adını veya yetenek türünü özelleştiremezsiniz.


## <a name="create-views"></a>Görünüm oluşturma

Bir Oluşturucu olarak, uygulamayı bir işlecine çevresel algılayıcı cihazı hakkındaki ilgili bilgileri görüntüleyecek şekilde özelleştirebilirsiniz. Özelleştirmeleriniz, uygulamaya bağlı çevresel algılayıcı cihazlarını yönetmek için işlecini etkinleştirir. Cihazlarla etkileşim kurmak için kullanılacak bir operatör için iki tür görünüm oluşturabilirsiniz:

* Cihaz ve bulut özelliklerini görüntüleme ve düzenleme için formlar.
* Cihazları görselleştirmek için panolar.

### <a name="generate-default-views"></a>Varsayılan görünümleri oluştur

Varsayılan görünümleri oluşturmak, önemli cihaz bilgilerinizi görselleştirmeyi kullanmaya başlamak için hızlı bir yoldur. Cihaz şablonunuz için en fazla üç varsayılan görünüm oluşturmuş olabilirsiniz:

* **Komutlar** görünümü, operatörünüzün komutlarınıza erişmenizi sağlar.
* **Genel bakış** görünümü cihaz telemetrisini görüntülemek için grafikleri ve ölçümleri kullanır.
* **Hakkında** görünümü cihaz özelliklerini görüntüler.

**Varsayılan görünümleri oluştur**' u seçtikten sonra bunlar otomatik olarak cihaz şablonunuzun **Görünümler** bölümünün altına eklenir.

### <a name="configure-a-view-to-visualize-devices"></a>Cihazları görselleştirmek için bir görünüm yapılandırma

Bir cihaz panosu, bir işlecin grafikleri ve ölçümleri kullanarak bir cihazı görselleştirmesine olanak sağlar. Oluşturucu olarak, bir cihaz panosunda hangi bilgilerin gösterileceğini tanımlayabilirsiniz. Cihazlar için birden çok Pano tanımlayabilirsiniz. Ortam algılayıcısı telemetrisini görselleştirmek üzere bir pano oluşturmak için, **Görünümler** ' i seçin ve ardından **cihazı görselleştirmeyi**seçin:

1. Tüm cihaz özellikleri, bulut özellikleri, telemetri ve statik Seçenekler **Özellikler**altında listelenmiştir. Bu öğelerin herhangi birini görünüme sürükleyip bırakabilirsiniz. **Parlaklık düzeyi** özelliğini görünüme sürükleyin. Kutucuğu, dişli simgesini kullanarak yapılandırabilirsiniz.

1. Telemetriyi çiztireeden bir grafik eklemek için **nem** ve **sıcaklık**' yı ve ardından **Birleştir**' i seçin. Bu grafiği, pasta grafiği veya çubuk grafik gibi farklı bir biçimde görüntülemek için kutucuğun en üstündeki **görselleştirmeyi Değiştir** düğmesini seçin.

1. Görünümünüzü kaydetmek için **Kaydet** ' i seçin:

Diğer özellikleri veya telemetri değerlerini gösteren daha fazla kutucuk ekleyebilirsiniz. Statik metin, bağlantılar ve görüntüler de ekleyebilirsiniz. Panodaki bir kutucuğu taşımak veya yeniden boyutlandırmak için fare işaretçisini döşemenin üzerine taşıyın ve kutucuğu yeni bir konuma sürükleyin veya yeniden boyutlandırın.

### <a name="add-a-device-form"></a>Bir cihaz formu Ekle

Bir cihaz formu, bir işlecin yazılabilir cihaz özelliklerini ve bulut özelliklerini düzenlemesini sağlar. Bir Oluşturucu olarak, birden çok form tanımlayabilir ve her formda hangi cihaz ve bulut özelliklerinin gösterileceğini seçebilirsiniz. Ayrıca, salt okunurdur cihaz özelliklerini bir formda görüntüleyebilirsiniz.

Ortam algılayıcısı özelliklerini görüntülemek ve düzenlemek için bir form oluşturmak için:

1. **Ortam algılayıcı** şablonundaki **görünümlere** gidin. Yeni bir görünüm eklemek için **cihazı ve bulut verilerini Düzenle** kutucuğunu seçin.

1. **Çevresel algılayıcı özellikleri**form adını girin.

1. **Müşteri adı** ve **son hizmet tarihi** bulutu özelliklerini formdaki mevcut bölüme sürükleyin.

1. **Parlaklık düzeyini** ve **cihaz durumu** cihaz özelliklerini seçin. Ardından **Bölüm Ekle**' yi seçin. Bölümün başlığını **algılayıcı özellikleri**olacak şekilde düzenleyin. **Uygula**’yı seçin.

1. **Cihaz modelini**, **yazılım sürümünü**, **üreticiyi**ve **işlemci üreticisi** cihaz özelliklerini seçin. Ardından **Bölüm Ekle**' yi seçin. Bölüm başlığını **cihaz özellikleri**olacak şekilde düzenleyin. **Uygula**’yı seçin.

1. Görünümünüzü kaydetmek için **Kaydet** ' i seçin.

## <a name="publish-device-template"></a>Cihaz şablonunu Yayımla

Sanal bir ortam algılayıcısı oluşturmadan veya gerçek bir ortam sensöre bağlanmadan önce, cihaz şablonunuzu yayımlamanız gerekir.

Bir cihaz şablonunu yayımlamak için:

1. Cihaz **şablonları** sayfasından cihaz şablonunuza gidin.

1. **Yayımla**’yı seçin.

1. **Cihaz şablonu Yayımla** Iletişim kutusunda **Yayımla**' yı seçin.

    ![Yayımlanan model](media/tutorial-define-device-type-pnp/publishedmodel.png)

Bir cihaz şablonu yayımlandıktan sonra **cihazlar** sayfasında ve işlecine görünür. Yayımlanmış bir cihaz şablonunda, yeni bir sürüm oluşturmadan bir cihaz yetenek modeli düzenleyemezsiniz. Ancak, bulut özellikleri, özelleştirmeler ve görünümlerde, sürüm oluşturmadan yayımlanmış bir cihaz şablonunda güncelleştirmeler yapabilirsiniz. Herhangi bir değişiklik yaptıktan sonra, bu değişiklikleri işletmenizin dışına göndermek için **Yayımla** ' yı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Yeni cihaz şablonu oluşturma
* Bir cihaz yetenek modeli içeri aktarın.
* Bulut özellikleri oluşturun.
* Özelleştirmeler oluşturun.
* Cihaz telemetrisi için görselleştirme tanımlayın.
* Cihaz şablonunuzu yayımlayın.

Artık Azure IoT Central uygulamanızda bir cihaz şablonu oluşturduğunuza göre, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Cihaz Ekle](tutorial-add-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
