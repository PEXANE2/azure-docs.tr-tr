---
title: Azure IoT Central yeni bir IoT cihaz türü tanımlayın | Microsoft Docs
description: Bu öğreticide, Azure IoT Central uygulamanızda yeni bir Azure IoT cihaz şablonu oluşturma, Oluşturucu olarak gösterilir. Türü için telemetri, durum, özellik ve komutları tanımlarsınız.
author: rangv
ms.author: rangv
ms.date: 10/22/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 177caaa5400c10ed8de80b04a3305dce7cae77d6
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74407017"
---
# <a name="tutorial-define-a-new-iot-device-type-in-your-azure-iot-central-application-preview-features"></a>Öğretici: Azure IoT Central uygulamanızda yeni bir IoT cihaz türü tanımlama (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Bir cihaz şablonu, bir Azure IoT Central uygulamasına bağlanan bir cihaz türünün özelliklerini ve davranışlarını tanımlayan bir şema.

Örneğin, bir Oluşturucu, bağlı bir fan için aşağıdaki özelliklere sahip bir cihaz şablonu oluşturabilir:

- Sıcaklık telemetrisini gönderir
- Location özelliğini gönderir
- Fan Motoru hata olaylarını gönderir
- Fan çalışma durumunu gönderir
- Yazılabilir fan hızı özelliği sağlar
- Cihazı yeniden başlatmak için bir komut sağlar
- Bir pano aracılığıyla cihazın genel görünümünü sağlar

Bu cihaz şablonundan bir operatör gerçek fan cihazları oluşturup bağlayabilirler. Tüm bu fanların ölçümleri, özellikleri ve operatörlerin bunları izlemek ve yönetmek için kullandığı komutları vardır. İşleçler, fan cihazlarıyla etkileşim kurmak için cihaz panoları ve formları kullanır.

> [!NOTE]
> Yalnızca oluşturucular ve yöneticiler cihaz şablonları oluşturabilir, düzenleyebilir ve silebilir. Herhangi bir Kullanıcı, mevcut cihaz şablonlarından **cihazlar** sayfasında cihaz oluşturabilir.

[Iot Tak ve kullan](../../iot-pnp/overview-iot-plug-and-play.md) , hiçbir katıştırılmış cihaz kodu yazmadan IoT Central cihazları tümleştirmesine olanak sağlar. IoT Tak ve Kullan 'nin çekirdeği, cihaz yeteneklerini açıklayan bir cihaz yetenek modeli şemadır. IoT Central önizleme uygulamasında, cihaz şablonları bu IoT Tak ve Kullan cihaz yeteneği modellerini kullanır.

Bir Oluşturucu olarak, cihaz şablonları oluşturmak için çeşitli seçenekleriniz vardır:

- IoT Central cihaz şablonunu tasarlayın ve cihaz kodunuzda cihaz yetenek modelini uygulayın.
- [IoT cihaz kataloğu Için Azure Sertifikalı](https://aka.ms/iotdevcat)bir cihaz yetenek modeli içeri aktarın. Ardından IoT Central uygulamanızın ihtiyaç duyacağı tüm bulut özelliklerini, özelleştirmeleri ve panoları ekleyin.
- Visual Studio Code kullanarak bir cihaz yetenek modeli oluşturun. Modelden cihaz kodunuzu uygulayın. Cihaz yetenek modelini IoT Central uygulamanıza el ile içeri aktarın ve ardından IoT Central uygulamanızın ihtiyaç duyacağı tüm bulut özelliklerini, özelleştirmeleri ve panoları ekleyin.
- Visual Studio Code kullanarak bir cihaz yetenek modeli oluşturun. Cihazdan cihaz kodunuzu uygulayın ve cihaz ilk bağlantısı kullanarak gerçek cihazınızı IoT Central uygulamanıza bağlayın. IoT Central, cihaz yetenek modelini sizin için ortak depodan bulur ve içeri aktarır. Daha sonra IoT Central uygulamanızın ihtiyaç duyacağı tüm bulut özelliklerini, özelleştirmeleri ve panoları cihaz şablonuna ekleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlayabilmeniz için [bir Azure IoT Central uygulaması oluşturmanız](quick-deploy-iot-central.md)gerekir.

## <a name="create-a-device-template-from-the-device-catalog"></a>Cihaz kataloğundan cihaz şablonu oluşturma

Bir Oluşturucu olarak, IoT Tak ve Kullan sertifikalı bir cihaz kullanarak çözümünüzü oluşturmaya hızlı bir başlangıç yapabilirsiniz. [Azure IoT cihaz kataloğunda](https://catalog.azureiotsolutions.com/alldevices)listeyi inceleyin. IoT Central, bu IoT Tak ve Kullan sertifikalı cihazlardan bir cihaz yetenek modeli alabilmeniz için cihaz kataloğu ile tümleşir. IoT Central içinde bu cihazlardan birini bir cihaz şablonu oluşturmak için:

1. IoT Central uygulamanızda **cihaz şablonları** sayfasına gidin.
1. **+ Yeni**' yi seçin ve ardından katalogdan IoT Tak ve kullan sertifikalı cihazlardan birini seçin. IoT Central, bu cihaz yetenek modelini temel alan bir cihaz şablonu oluşturur.
1. Cihaz şablonunuza tüm bulut özelliklerini, özelleştirmeleri veya görünümlerini ekleyin.
1. Şablonu operatörlerin cihazları görüntülemesi ve bağlanabilmesi için kullanılabilir hale getirmek için **Yayımla** ' yı seçin.

## <a name="create-a-device-template-from-scratch"></a>Sıfırdan bir cihaz şablonu oluşturma

Bir cihaz şablonu şunları içerir:

- Cihazın uyguladığı telemetri, özellik ve komutları belirten bir _cihaz yetenek modeli_ . Bu yetenekler bir veya daha fazla arabirim halinde düzenlenmiştir.
- IoT Central uygulamanızın cihazlarınızla ilgili depoladığını belirten bilgileri tanımlayan _bulut özellikleri_ . Örneğin, bir bulut özelliği bir cihazın en son hizmet verdiği tarihi kaydedebilir. Bu bilgiler hiçbir şekilde cihazla paylaşılmaz.
- _Özelleştirmeler_ , oluşturucunun cihaz yetenek modelindeki bazı tanımları geçersiz kılmasına izin verir. Örneğin, Oluşturucu bir cihaz özelliğinin adını geçersiz kılabilir. Özellik adları IoT Central panolar ve formlarda görüntülenir.
- _Panolar ve formlar_ , oluşturucunun uygulamanıza bağlı olan cihazları izleyip yönetmesine olanak tanıyan bir kullanıcı arabirimi oluşturmasını sağlar.

IoT Central bir cihaz şablonu oluşturmak için:

1. IoT Central uygulamanızda **cihaz şablonları** sayfasına gidin.
1. **+ Yeni** > **özel**' i seçin.
1. Şablonunuz için **ortam algılayıcısı**gibi bir ad girin.
1. **Enter**'a basın. IoT Central boş bir cihaz şablonu oluşturur.

## <a name="manage-a-device-template"></a>Bir cihaz şablonunu yönetme

Şablonun giriş sayfasından bir şablonu yeniden adlandırabilir veya silebilirsiniz.

Şablonunuza bir cihaz yetenek modeli ekledikten sonra, bunu yayımlayabilirsiniz. Şablonu yayımlaana kadar, işletmenler için bu şablona dayalı bir cihazı, **cihazlar** sayfasında görmek üzere bağlanamazsınız.

## <a name="create-a-capability-model"></a>Yetenek modeli oluşturma

Bir cihaz yetenek modeli oluşturmak için şunları yapabilirsiniz:

- Sıfırdan özel bir model oluşturmak için IoT Central kullanın.
- JSON dosyasından bir model içeri aktarın. Bir cihaz Oluşturucu, uygulamanız için bir cihaz yetenek modeli yazmak üzere Visual Studio Code kullanmış olabilir.
- Cihaz kataloğundan cihazlardan birini seçin. Bu seçenek, üreticinin Bu cihaz için yayımladığı cihaz yetenek modelini içe aktarır. Bu şekilde içeri aktarılan bir cihaz yetenek modeli otomatik olarak yayımlanır.

## <a name="manage-a-capability-model"></a>Yetenek modeli yönetme

Bir cihaz yetenek modeli oluşturduktan sonra şunları yapabilirsiniz:

- Modele arabirimler ekleyin. Bir modelde en az bir arabirim olmalıdır.
- KIMLIĞI, ad alanı ve adı gibi model meta verilerini düzenleyin.
- Modeli silin.

## <a name="create-an-interface"></a>Arabirim oluşturma

Cihaz yeteneğinin en az bir arabirimi olmalıdır. Arabirim, yeniden kullanılabilir bir özellik koleksiyonudur.

Bir arabirim oluşturmak için:

1. Cihaz yetenek modelinize gidin ve **+ arabirim Ekle**' yi seçin.

1. **Bir arabirim seçin** sayfasında şunları yapabilirsiniz:

    - Sıfırdan özel bir arabirim oluşturun.
    - Varolan bir arabirimi dosyadan içeri aktarın. Cihaz Oluşturucu, cihazınız için bir arabirim yazmak üzere Visual Studio Code kullanmış olabilir.
    - **Cihaz bilgileri** arabirimi gibi standart arabirimlerden birini seçin. Standart arabirimler, birçok cihaz için ortak olan özellikleri belirtir. Bu standart arabirimler Azure IoT tarafından yayımlanır ve sürümü oluşturulamaz veya düzenlenemez.

1. Arabirim oluşturduktan sonra, arabirimin görünen adını değiştirmek için **kimliği Düzenle** ' yi seçin.

1. Sıfırdan özel bir arabirim oluşturmayı seçerseniz, cihazınızın yeteneklerini ekleyebilirsiniz. Cihaz özellikleri telemetri, Özellikler ve komutlardır.

### <a name="telemetry"></a>Telemetri

Telemetri, genellikle bir sensörden cihazdan gönderilen değerlerin bir akışıdır. Örneğin, bir algılayıcı çevresel sıcaklığa rapor edebilir.

Aşağıdaki tabloda bir telemetri yeteneğinin yapılandırma ayarları gösterilmektedir:

| Alan | Açıklama |
| ----- | ----------- |
| Görünen Ad | Panolar ve formlarda kullanılan telemetri değeri için görünen ad. |
| Name | Telemetri iletisindeki alanın adı. IoT Central görünen adından Bu alan için bir değer oluşturur, ancak gerekirse kendi değerini seçebilirsiniz. |
| Yetenek türü | Telemetri. |
| Anlamsal tür | Telemetrinin sıcaklık, durum veya olay gibi anlam türü. Anlamsal tür seçimi aşağıdaki alanlardan hangisinin kullanılabildiğini belirler. |
| Şema | Çift, dize veya vektör gibi telemetri veri türü. Kullanılabilir seçimler anlamsal tür tarafından belirlenir. Şema, olay ve durum anlam türleri için kullanılamaz. |
| Severity | Yalnızca olay anlam türü için kullanılabilir. Önem derecesi **hata**, **bilgi**veya **uyarılardır**. |
| Durum değerleri | Yalnızca durum anlam türü için kullanılabilir. Her birinin görünen adı, adı, sabit listesi türü ve değeri olan olası durum değerlerini tanımlayın. |
| Birim | Telemetri değeri için **mph**, **%** veya **&deg;C**gibi bir birim. |
| Görüntüleme birimi | Panolar ve formlarda kullanılacak bir görüntüleme birimi. |
| Yorum | Telemetri yeteneği hakkında herhangi bir yorum. |
| Açıklama | Telemetri yeteneğinin açıklaması. |

### <a name="properties"></a>Özellikler

Özellikler, zaman içinde nokta değerlerini temsil eder. Örneğin, bir cihaz, ulaşmaya çalıştığı hedef sıcaklığın raporlanabilmesi için bir özelliği kullanabilir. IoT Central yazılabilir özellikleri ayarlayabilirsiniz.

Aşağıdaki tabloda bir özellik yeteneği için yapılandırma ayarları gösterilmektedir:

| Alan | Açıklama |
| ----- | ----------- |
| Görünen Ad | Panolar ve formlarda kullanılan özellik değeri için görünen ad. |
| Name | Özelliğin adı. IoT Central görünen adından Bu alan için bir değer oluşturur, ancak gerekirse kendi değerini seçebilirsiniz. |
| Yetenek türü | Özelliði. |
| Anlamsal tür | Özelliğin sıcaklık, durum veya olay gibi anlam türü. Anlamsal tür seçimi aşağıdaki alanlardan hangisinin kullanılabildiğini belirler. |
| Şema | Double, String veya Vector gibi özellik veri türü. Kullanılabilir seçimler anlamsal tür tarafından belirlenir. Şema, olay ve durum anlam türleri için kullanılamaz. |
| Yazılabilir | Özellik yazılabilir değilse, cihaz özellik değerlerini IoT Central rapor edebilir. Özellik yazılabilir ise, cihaz özellik değerlerini IoT Central rapor edebilir ve IoT Central Özellik güncelleştirmelerini cihaza gönderebilir.
| Severity | Yalnızca olay anlam türü için kullanılabilir. Önem derecesi **hata**, **bilgi**veya **uyarılardır**. |
| Durum değerleri | Yalnızca durum anlam türü için kullanılabilir. Her birinin görünen adı, adı, sabit listesi türü ve değeri olan olası durum değerlerini tanımlayın. |
| Birim | Özellik değeri için **mph**, **%** veya **&deg;C**gibi bir birim. |
| Görüntüleme birimi | Panolar ve formlarda kullanılacak bir görüntüleme birimi. |
| Yorum | Özellik yeteneği hakkında herhangi bir açıklama. |
| Açıklama | Özellik yeteneğinin açıklaması. |

### <a name="commands"></a>Komutlar

IoT Central 'tan cihaz komutlarını çağırabilirsiniz. Komutlar isteğe bağlı olarak parametreleri cihaza iletir ve cihazdan bir yanıt alır. Örneğin, bir cihazı 10 saniye içinde yeniden başlatmak için bir komut çağırabilirsiniz.

Aşağıdaki tabloda, bir komut özelliğine ait yapılandırma ayarları gösterilmektedir:

| Alan | Açıklama |
| ----- | ----------- |
| Görünen Ad | Panolar ve formlarda kullanılan komutun görünen adı. |
| Name | Komutun adı. IoT Central görünen adından Bu alan için bir değer oluşturur, ancak gerekirse kendi değerini seçebilirsiniz. |
| Yetenek türü | Komutundaki. |
| Komut | `SynchronousExecutionType`. |
| Yorum | Komut özelliğiyle ilgili herhangi bir yorum. |
| Açıklama | Komut yeteneğinin açıklaması. |
| İstek | Etkinleştirilirse, istek parametresinin tanımı: ad, görünen ad, şema, birim ve görüntü birimi. |
| Yanıt | Etkinleştirilirse, komut yanıtının tanımı: ad, görünen ad, şema, birim ve görüntü birimi. |

## <a name="manage-an-interface"></a>Arabirim yönetme

Arabirimi yayımlamadıysanız, arabirim tarafından tanımlanan özellikleri düzenleyebilirsiniz. Arabirimi yayımladıktan sonra, herhangi bir değişiklik yapmak istiyorsanız, cihaz şablonunun yeni bir sürümünü oluşturmanız ve arabirimi sürümüne uygulamanız gerekir. **Özelleştirme** bölümünde, sürüm oluşturma gerektirmeyen değişiklikler (görünen adlar veya birimler gibi) yapabilirsiniz.

Ayrıca, başka bir yetenek modelinde yeniden kullanmak istiyorsanız, bir JSON dosyası olarak arayüzü dışarı aktarabilirsiniz.

## <a name="add-cloud-properties"></a>Bulut özellikleri ekle

IoT Central içindeki cihazlarla ilgili bilgileri depolamak için bulut özelliklerini kullanın. Bulut özellikleri hiçbir şekilde cihaza gönderilmez. Örneğin, bulut özelliklerini, cihazı yüklemiş olan müşterinin adını veya cihazın son hizmet tarihini depolamak için kullanabilirsiniz.

Aşağıdaki tabloda bir bulut özelliğinin yapılandırma ayarları gösterilmektedir:

| Alan | Açıklama |
| ----- | ----------- |
| Görünen Ad | Panolar ve formlarda kullanılan bulut özelliği değeri için görünen ad. |
| Name | Bulut özelliğinin adı. IoT Central görünen adından Bu alan için bir değer oluşturur, ancak gerekirse kendi değerini seçebilirsiniz. |
| Anlamsal tür | Özelliğin sıcaklık, durum veya olay gibi anlam türü. Anlamsal tür seçimi aşağıdaki alanlardan hangisinin kullanılabildiğini belirler. |
| Şema | Çift, dize veya vektör gibi bulut özelliği veri türü. Kullanılabilir seçimler anlamsal tür tarafından belirlenir. |

## <a name="add-customizations"></a>Özelleştirmeler ekleme

İçeri aktarılan bir arabirimi değiştirmeniz veya bir özelliğe IoT Central özgü özellikler eklemeniz gerektiğinde özelleştirmeleri kullanın. Yalnızca arabirim uyumluluğunu kesen alanları özelleştirebilirsiniz. Örneğin, şunları yapabilirsiniz:

- Bir özelliğin görünen adını ve birimlerini özelleştirin.
- Değer bir grafikte göründüğünde kullanılacak varsayılan rengi ekleyin.
- Bir özellik için başlangıçtaki, minimum ve maksimum değerleri belirtin.

Yetenek adını veya yetenek türünü özelleştiremezsiniz. **Özelleştirme** bölümünde yapameyeceğiniz değişiklikler varsa, özelliği değiştirmek için cihaz şablonunuzu ve arabirimini sürüm yapmanız gerekir.

### <a name="generate-default-views"></a>Varsayılan görünümleri oluştur

Varsayılan görünümleri oluşturmak, önemli cihaz bilgilerinizin görselleştirilmesine yönelik hızlı bir yoldur. Cihaz şablonunuz için en fazla üç varsayılan görünümünüz oluşturulmuştur:

- **Komutlar** , cihaz komutlarıyla bir görünüm sağlar ve operatörünüzün bunları cihazınıza göndermelerine olanak sağlar.
- **Genel bakış** cihaz telemetrisi ile grafik ve ölçümleri görüntüleyen bir görünüm sağlar.
- **Hakkında** cihaz bilgileriyle cihaz bilgilerini görüntüleyen bir görünüm sağlar.

**Varsayılan görünümleri oluştur**' u seçtikten sonra, bunların cihaz şablonunuzun **Görünümler** bölümü altında otomatik olarak eklendiğini görürsünüz.

## <a name="add-dashboards"></a>Pano ekleme

İşleçlerin grafikleri ve ölçümleri kullanarak bir cihazı görselleştirmesini sağlamak için bir cihaz şablonuna panolar ekleyin. Bir cihaz şablonu için birden çok panonuz olabilir.

Bir cihaz şablonuna Pano eklemek için:

1. Cihaz şablonunuza gidin ve **Görünümler**' i seçin.
1. **Cihazı görselleştirmeyi**seçin.
1. Pano **adı**bölümünde panonuz için bir ad girin.
1. Statik, özellik, bulut özelliği, telemetri ve komut kutucukları listesinden panonuza kutucuk ekleyin. Panonuza eklemek istediğiniz kutucukları sürükleyip bırakın.
1. Tek bir grafik kutucuğunda birden çok telemetri değeri çizmek için telemetri değerlerini seçin ve ardından **Birleştir**' i seçin.
1. Eklediğiniz her kutucuğu, verilerin nasıl görüntüleneceğini özelleştirmek için yapılandırın. Bunu, dişli simgesini seçerek veya grafik kutucuğunda **yapılandırmayı Değiştir** ' i seçerek yapabilirsiniz.
1. Panonuzdaki kutucukları düzenleyin ve yeniden boyutlandırın.
1. Değişiklikleri kaydedin.

### <a name="configure-preview-device-to-view-dashboard"></a>Panoyu görüntülemek için Önizleme cihazını yapılandırma

Panonuzu görüntülemek ve test etmek için **Önizleme cihazını Yapılandır**' ı seçin. Bu, işletmeni yayımlandıktan sonra bir kez gördüğünde panoyu görmenizi sağlar. Görünümlerinizin doğru verileri göstermesini doğrulamak için bu seçeneği kullanın. Aşağıdakiler arasından seçim yapabilirsiniz:

- Önizleme cihazı yok.
- Cihaz şablonunuz için yapılandırdığınız gerçek test cihazı.
- Uygulamanızdaki mevcut bir cihaz, cihaz KIMLIĞINI kullanarak.

## <a name="add-forms"></a>Form Ekle

İşleçleri, özellikleri görüntüleyip ayarlayarak bir cihazı yönetmesine olanak tanımak için bir cihaz şablonuna form ekleyin. İşleçler yalnızca bulut özelliklerini ve yazılabilir cihaz özelliklerini düzenleyebilir. Bir cihaz şablonu için birden çok form kullanabilirsiniz.

Bir cihaz şablonuna form eklemek için:

1. Cihaz şablonunuza gidin ve **Görünümler**' i seçin.
1. **Cihaz ve bulut verilerini Düzenle '** yi seçin.
1. Form **adı**' na formunuz için bir ad girin.
1. Formunuzu düzenlemek için kullanılacak sütun sayısını seçin.
1. Formunuzdaki mevcut bir bölüme özellikler ekleyin veya Özellikler ' i seçin ve **Bölüm Ekle**' yi seçin. Formunuzdaki özellikleri gruplamak için bölümleri kullanın. Bir bölüme başlık ekleyebilirsiniz.
1. Davranışını özelleştirmek için formdaki her özelliği yapılandırın.
1. Formunuzdaki özellikleri düzenleyin.
1. Değişiklikleri kaydedin.

## <a name="publish-a-device-template"></a>Bir cihaz şablonu yayımlama

Cihaz yetenek modelinizi uygulayan bir cihazı bağlayabilmeniz için önce cihaz şablonunuzu yayımlamanız gerekir.

Bir cihaz şablonu yayımladıktan sonra, yalnızca cihaz yetenek modelinde sınırlı değişiklikler yapabilirsiniz. Bir arabirimi değiştirmek için [Yeni bir sürüm oluşturmanız ve yayımlamanız](./howto-version-device-template.md)gerekir.

Bir cihaz şablonu yayımlamak için, cihaz şablonunuza gidin ve **Yayımla**' yı seçin.

Bir cihaz şablonu yayımladıktan sonra, bir operatör **cihazlar** sayfasına gidebilir ve cihaz şablonunuzu kullanan gerçek ya da sanal cihazları ekleyebilir. Değişiklik yaparken cihaz şablonunuzu değiştirmeye ve kaydetmeye devam edebilirsiniz. Bu değişiklikleri, **cihazlar** sayfasında görüntülenecek olan işlece iletmek istediğinizde, her seferinde **Yayımla** ' yı seçmeniz gerekir.

## <a name="define-a-new-iot-gateway-device-type-preview-features"></a>Yeni bir IoT Ağ Geçidi cihaz türü tanımlama (Önizleme özellikleri)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Bu öğreticide, bir Oluşturucu olarak, IoT Central uygulamanızda yeni bir IoT cihazı türü tanımlamak üzere bir ağ geçidi cihaz şablonu nasıl kullanılacağı gösterilir. 

Bu bölümde, **akıllı bir bina** cihaz şablonu oluşturacaksınız. Akıllı bir bina ağ geçidi cihazı:

* Sıcaklık ve doluluk gibi telemetri gönderir.
* Bulutta güncelleştirildiği zaman, telemetri gönderme aralığı gibi yazılabilir özelliklere yanıt verir.
* Sıcaklık sıfırlama gibi komutlara yanıt verir.
* Diğer cihaz yetenek modelleriyle ilişkilerin yapılmasına izin verir.

### <a name="create-iot-device-templates"></a>IoT cihaz şablonları oluşturma

IoT cihaz şablonlarının oluşturulması aşağıda verilmiştir: 

1. Sol gezinti bölmesinde **cihaz şablonları**' nı seçin. Ardından **+ Yeni**' yi seçin ve **IoT cihaz** kutucuğu ve doluluk algılayıcısı kutucuğunu seçin. **İleri ' yi seçin: Özelleştir**.

   ![Cihaz şablonları sayfası ve seçeneklerinin ekran görüntüsü](./media/tutorial-define-iot-device-type/gateway-downstream-new.png)

1. **İnceleme** sayfasında **Oluştur**' u seçin. 

   ![Inceleme sayfasının ekran görüntüsü](./media/tutorial-define-iot-device-type/gateway-downstream-review.png)

1. Yeni bir cihaz şablonu oluşturulur. 

   ![Yeni cihaz şablonunun ekran görüntüsü](./media/tutorial-define-iot-device-type/occupancy-sensor.png)

S1 algılayıcısı için bir cihaz şablonu oluşturma:

1. Sol gezinti bölmesinde **cihaz şablonları**' nı seçin. Ardından **+ Yeni**' yi seçin ve **IoT cihaz** kutucuğunu seçin ve sahiplik algılayıcısı kutucuğunu seçin. **İleri ' yi seçin: Özelleştir**.

   ![Cihaz şablonları sayfası ve seçeneklerinin ekran görüntüsü](./media/tutorial-define-iot-device-type/s1-sensor.png)

1. **İnceleme** sayfasında **Oluştur**' u seçin. 

   ![Inceleme sayfasının ekran görüntüsü](./media/tutorial-define-iot-device-type/s1-review.png)

1. Yeni bir cihaz şablonu oluşturulur. 

   ![Yeni cihaz şablonunun ekran görüntüsü](./media/tutorial-define-iot-device-type/s1-template.png)

## <a name="create-an-iot-gateway-device-template"></a>IoT Ağ Geçidi cihaz şablonu oluşturma

IoT Ağ Geçidi cihaz şablonu oluşturmayı seçebilirsiniz. Ağ Geçidi cihazında, ağ geçidi cihazından IoT Central bağlanan aşağı akış cihazları olan ilişkiler vardır. 

### <a name="downstream-device-relationships-with-gateway-device"></a>Ağ Geçidi cihazındaki aşağı akış cihaz ilişkileri

IoT cihazları, IoT Ağ Geçidi cihazına bağlanabilir.

![Ağ geçidi cihazı ve aşağı akış cihazları arasındaki ilişkinin diyagramı](./media/tutorial-define-iot-device-type/gatewaypattern.png)

Bir Oluşturucu olarak, uygulamanızda IoT Gateway cihaz şablonları oluşturabilir ve düzenleyebilirsiniz. Bir cihaz şablonu yayımladıktan sonra, cihaz şablonunu uygulayan gerçek cihazlara bağlanabilirsiniz.

### <a name="select-a-device-template-type"></a>Bir cihaz şablonu türü seçin 

Uygulamanıza yeni bir cihaz şablonu eklemek için:

1. Sol bölmeden **cihaz şablonları** sekmesini seçin.

   ![Cihaz şablonları sayfasının ekran görüntüsü](./media/tutorial-define-iot-device-type/devicetemplate.png)

1. Yeni bir cihaz şablonu oluşturmaya başlamak için **+ Yeni** ' yi seçin.

   ![Yeni vurgulanmış şekilde cihaz şablonları sayfasının ekran görüntüsü](./media/tutorial-define-iot-device-type/devicetemplatenew.png)

   ![Cihaz özelleştirme sayfasının ekran görüntüsü](./media/tutorial-define-iot-device-type/gateway-review.png)

1. **Şablon türü seç** sayfasında **Azure IoT**' yi seçin ve ardından **İleri**' yi seçin.

   ![Şablon türü seç sayfasının ekran görüntüsü](./media/tutorial-define-iot-device-type/gateway-customize.png)

1. Ağ geçidi onay kutusunu seçin ve **Oluştur**' u seçin.

   ![Ağ Geçidi vurgulanmış şekilde cihaz özelleştirme sayfasının ekran görüntüsü](./media/tutorial-define-iot-device-type/gateway-review.png)

1. İnceleme sayfasında **Oluştur**' u seçin. 

1. Ağ Geçidi şablon adını, **akıllı bina ağ geçidi şablonunu**girin. **Özel** kutucuğu seçin.

1. Standart arabirim **cihaz bilgileri**ekleyin.

### <a name="add-relationships"></a>İlişki Ekle

Bir ağ geçidi cihazına bağlandığınız cihazlar için cihaz yetenek modellerine aşağı akış ilişkileri ekleyebilirsiniz.

Aşağı akış cihazı yetenek modelleriyle ilişkiler oluşturun. **Kaydet**’i seçin.

![Çeşitli seçenekler vurgulanmış şekilde akıllı bina ağ geçidi şablonunun ekran görüntüsü](./media/tutorial-define-iot-device-type/gateway-occupancy-s1-rel.png)

### <a name="add-cloud-properties"></a>Bulut özellikleri ekle

Bir cihaz şablonu, bulut özellikleri içerebilir. Bulut özellikleri yalnızca IoT Central uygulamasında bulunur ve bir cihazdan hiçbir şekilde gönderilmez veya buradan alınmaz.

1. **Bulut özelliklerini** seçin >  **+ bulut özelliği ekleyin**. Cihaz şablonunuza bir bulut özelliği eklemek için aşağıdaki tablodaki bilgileri kullanın.

    | Görünen ad      | Anlamsal tür | Şema |
    | ----------------- | ------------- | ------ |
    | Son Hizmet Tarihi | Yok.          | Date   |
    | Müşteri adı     | Yok.          | Dize |

2. **Kaydet**’i seçin.

### <a name="add-customizations"></a>Özelleştirmeler ekleme

Bir arabirimi değiştirmek ya da cihaz yetenek modelinizi sürümünüzü gerektirmeyen bir özelliğe IoT Central özgü özellikler eklemek için özelleştirmeleri kullanın. Yetenek modeli taslak veya Yayımlanma durumundaysa alanları özelleştirebilirsiniz. Yalnızca arabirim uyumluluğunu kesen alanları özelleştirebilirsiniz. Örneğin, şunları yapabilirsiniz:

- Bir özelliğin görünen adını ve birimlerini özelleştirin.
- Değer bir grafikte göründüğünde kullanılacak varsayılan rengi ekleyin.
- Bir özellik için başlangıçtaki, minimum ve maksimum değerleri belirtin.

Yetenek adını veya yetenek türünü özelleştiremezsiniz.

Özelleştirmeyi bitirdiğinizde **Kaydet**' i seçin.

### <a name="create-views"></a>Görünüm oluşturma

Bir Oluşturucu olarak, uygulamayı bir işlecine çevresel algılayıcı cihazı hakkındaki ilgili bilgileri görüntüleyecek şekilde özelleştirebilirsiniz. Özelleştirmeleriniz, uygulamaya bağlı çevresel algılayıcı cihazlarını yönetmek için işlecini etkinleştirir. Cihazlarla etkileşim kurmak için kullanılacak bir operatör için iki tür görünüm oluşturabilirsiniz:

* Cihaz ve bulut özelliklerini görüntüleme ve düzenleme için formlar.
* Cihazları görselleştirmek için panolar.

### <a name="generate-default-views"></a>Varsayılan görünümleri oluştur

**Varsayılan görünümleri oluştur**' u seçerseniz **genel bakış** ve panolar **hakkında** ' yı oluşturabilirsiniz. 

## <a name="publish-a-device-template"></a>Bir cihaz şablonu yayımlama

Sanal bir ortam algılayıcısı oluşturmadan veya gerçek bir ortam sensöre bağlanmadan önce, cihaz şablonunuzu yayımlamanız gerekir.

Bir cihaz şablonunu yayımlamak için:

1. Cihaz **şablonları** sayfasından cihaz şablonunuza gidin.

2. **Yayımla**’yı seçin.

3. **Cihaz şablonu Yayımla** Iletişim kutusunda **Yayımla**' yı seçin.

Bir cihaz şablonu yayımlandıktan sonra **cihazlar** sayfasında ve işlecine görünür. Yayımlanmış bir cihaz şablonunda, yeni bir sürüm oluşturmadan bir cihaz yetenek modeli düzenleyemezsiniz. Ancak, yayımlanmış bir cihaz şablonunda bulut özellikleri, özelleştirmeler ve görünümlerde güncelleştirmeler yapabilirsiniz. Bu güncelleştirmeler yeni bir sürümün oluşturulmasına neden olmaz. Herhangi bir değişiklik yaptıktan sonra, bu değişiklikleri işletmenizin dışına göndermek için **Yayımla** ' yı seçin.

## <a name="create-a-gateway-simulated-device"></a>Ağ Geçidi sanal cihazı oluşturma

Cihaz Gezgini ' nden, sanal bir akıllı bina Ağ Geçidi oluşturun. 

![Yeni cihaz oluştur iletişim kutusunun ekran görüntüsü](./media/tutorial-define-iot-device-type/smartbuildingdevice.png)

## <a name="create-downstream-simulated-devices"></a>Aşağı akış sanal cihazlar oluşturma

Cihaz Gezgini ' nden, sanal bir doluluk algılayıcısı oluşturun. 

![Yeni cihaz oluştur iletişim kutusunun ekran görüntüsü](./media/tutorial-define-iot-device-type/occupancydevice.png)

Cihaz Gezgini ' nden, sanal bir S1 algılayıcısı oluşturun. 

![Yeni cihaz oluştur iletişim kutusunun ekran görüntüsü](./media/tutorial-define-iot-device-type/s1device.png)

## <a name="add-downstream-devices-relationships-to-a-gateway-device"></a>Bir ağ geçidi cihazına aşağı akış cihazları ilişkileri ekleme

S1 algılayıcı ve sahiplik algılayıcısı ' nı seçin ve **ağ geçidine Bağlan**' ı seçin. 

![Ağ geçidine Bağlan vurgulanarak doluluk algılayıcısı ekran görüntüsü](./media/tutorial-define-iot-device-type/connecttogateway.png)

Bir ağ geçidi cihaz şablonu ve ağ geçidi cihaz örneği seçin ve **Birleştir**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Cihaz şablonu olarak yeni bir IoT Ağ Geçidi oluşturun.
* Bulut özellikleri oluşturun.
* Özelleştirmeler oluşturun.
* Cihaz telemetrisi için görselleştirme tanımlayın.
* İlişkiler ekleyin.
* Cihaz şablonunuzu yayımlayın.

Ardından şunları yapabilirsiniz:

> [!div class="nextstepaction"]
> [Cihaz bağlama](tutorial-connect-pnp-device.md)
