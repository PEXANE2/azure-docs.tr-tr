---
title: Azure IoT Central yeni bir IoT cihaz türü tanımlayın | Microsoft Docs
description: Bu makalede, Azure IoT Central uygulamanızda yeni bir Azure IoT cihaz şablonu oluşturma, Oluşturucu olarak gösterilir. Türü için telemetri, durum, özellik ve komutları tanımlarsınız.
author: dominicbetts
ms.author: dobett
ms.date: 12/06/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 3e4b44c8f784524b4cd363a2f4531c5bf0a70e0d
ms.sourcegitcommit: 1a0dfa54116aa036af86bd95dcf322307cfb3f83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88041604"
---
# <a name="define-a-new-iot-device-type-in-your-azure-iot-central-application"></a>Azure IoT Central uygulamanızda yeni bir cihaz türü tanımlama

*Bu makale, çözüm oluşturucular ve cihaz geliştiricileri için geçerlidir.*

Bir cihaz şablonu, bir [Azure IoT Central uygulamasına](concepts-app-templates.md)bağlanan bir cihaz türünün özelliklerini ve davranışlarını tanımlayan bir şema.

Örneğin, bir Oluşturucu, bağlı bir fan için aşağıdaki özelliklere sahip bir cihaz şablonu oluşturabilir:

- Sıcaklık telemetrisini gönderir
- Location özelliğini gönderir
- Fan Motoru hata olaylarını gönderir
- Fan çalışma durumunu gönderir
- Yazılabilir fan hızı özelliği sağlar
- Cihazı yeniden başlatmak için bir komut sağlar
- Bir pano aracılığıyla cihazın genel görünümünü sağlar

Bu cihaz şablonundan bir operatör gerçek fan cihazları oluşturup bağlayabilirler. Tüm bu fanların ölçümleri, özellikleri ve operatörlerin bunları izlemek ve yönetmek için kullandığı komutları vardır. İşleçler, fan cihazlarıyla etkileşim kurmak için [cihaz panoları](#add-dashboards) ve formları kullanır. Bir cihaz geliştiricisi, cihazın uygulamayla nasıl etkileşime gireceğini anlamak için şablonu kullanır. Daha fazla bilgi için bkz. [telemetri, özellik ve komut yükleri](concepts-telemetry-properties-commands.md).

> [!NOTE]
> Yalnızca oluşturucular ve yöneticiler cihaz şablonları oluşturabilir, düzenleyebilir ve silebilir. Herhangi bir Kullanıcı, mevcut cihaz şablonlarından **cihazlar** sayfasında cihaz oluşturabilir.

Bir IoT Central uygulamasında cihaz şablonu, bir cihazın yeteneklerini anlatmak için cihaz yetenek modeli kullanır. Bir Oluşturucu olarak, cihaz şablonları oluşturmak için çeşitli seçenekleriniz vardır:

- IoT Central cihaz şablonunu tasarlayın ve cihaz [kodunuzda cihaz yetenek modelini uygulayın](concepts-telemetry-properties-commands.md).
- [IoT cihaz kataloğu Için Azure Sertifikalı](https://aka.ms/iotdevcat)bir cihaz yetenek modeli içeri aktarın. Ardından IoT Central uygulamanızın ihtiyaç duyacağı tüm bulut özelliklerini, özelleştirmeleri ve panoları ekleyin.
- Visual Studio Code kullanarak bir cihaz yetenek modeli oluşturun. Modelden cihaz kodunuzu uygulayın. Cihaz yetenek modelini IoT Central uygulamanıza el ile içeri aktarın ve ardından IoT Central uygulamanızın ihtiyaç duyacağı tüm bulut özelliklerini, özelleştirmeleri ve panoları ekleyin.
- Visual Studio Code kullanarak bir cihaz yetenek modeli oluşturun. Cihazdan cihaz kodunuzu uygulayın ve cihaz ilk bağlantısı kullanarak gerçek cihazınızı IoT Central uygulamanıza bağlayın. IoT Central, cihaz yetenek modelini sizin için ortak depodan bulur ve içeri aktarır. Daha sonra IoT Central uygulamanızın ihtiyaç duyacağı tüm bulut özelliklerini, özelleştirmeleri ve panoları cihaz şablonuna ekleyebilirsiniz.

Ayrıca, [REST API](https://docs.microsoft.com/learn/modules/manage-iot-central-apps-with-rest-api/) veya [clı](howto-manage-iot-central-from-cli.md)kullanarak bir IoT Central uygulamasına cihaz şablonları ekleyebilirsiniz.

Bazı [uygulama şablonları](concepts-app-templates.md) , uygulama şablonunun desteklediği senaryoda faydalı olan cihaz şablonlarını zaten içeriyor. Örneğin, bkz. [Mağaza içi analiz mimarisi](../retail/store-analytics-architecture.md).

## <a name="create-a-device-template-from-the-device-catalog"></a>Cihaz kataloğundan cihaz şablonu oluşturma

Bir Oluşturucu olarak, IoT Tak ve Kullan (Önizleme) sertifikalı bir cihaz kullanarak çözümünüzü oluşturmaya hızlı bir başlangıç yapabilirsiniz. [Azure IoT cihaz kataloğunda](https://catalog.azureiotsolutions.com/alldevices)listeyi inceleyin. IoT Central, bu IoT Tak ve Kullan (Önizleme) sertifikalı cihazların herhangi birinden bir cihaz yetenek modeli alabilmeniz için cihaz kataloğu ile tümleşir. IoT Central içinde bu cihazlardan birini bir cihaz şablonu oluşturmak için:

1. IoT Central uygulamanızda **cihaz şablonları** sayfasına gidin.
1. **+ Yeni**' yi seçin ve ardından katalogdaki IoT Tak ve kullan (Önizleme) sertifikalı cihazları seçin. IoT Central, bu cihaz yetenek modelini temel alan bir cihaz şablonu oluşturur.
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
1. **+ Yeni**  >  **özel**' i seçin.
1. Şablonunuz için **ortam algılayıcısı**gibi bir ad girin.
1.  **Enter** tuşuna basın. IoT Central boş bir cihaz şablonu oluşturur.

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
| Name | Telemetri iletisindeki alanın adı. IoT Central görünen adından Bu alan için bir değer oluşturur, ancak gerekirse kendi değerini seçebilirsiniz. Bu alanın alfasayısal olması gerekir. |
| Yetenek Türü | Telemetri. |
| Anlamsal tür | Telemetrinin sıcaklık, durum veya olay gibi anlam türü. Anlamsal tür seçimi aşağıdaki alanlardan hangisinin kullanılabildiğini belirler. |
| Şema | Çift, dize veya vektör gibi telemetri veri türü. Kullanılabilir seçimler anlamsal tür tarafından belirlenir. Şema, olay ve durum anlam türleri için kullanılamaz. |
| Önem Derecesi | Yalnızca olay anlam türü için kullanılabilir. Önem derecesi **hata**, **bilgi**veya **uyarılardır**. |
| Durum değerleri | Yalnızca durum anlam türü için kullanılabilir. Her birinin görünen adı, adı, sabit listesi türü ve değeri olan olası durum değerlerini tanımlayın. |
| Birim | Bir telemetri değeri için **mph**, **%** veya ** &deg; C**gibi bir birim. |
| Görüntüleme birimi | Panolar ve formlarda kullanılacak bir görüntüleme birimi. |
| Yorum | Telemetri yeteneği hakkında herhangi bir yorum. |
| Description | Telemetri yeteneğinin açıklaması. |

### <a name="properties"></a>Özellikler

Özellikler, zaman içinde nokta değerlerini temsil eder. Örneğin, bir cihaz, ulaşmaya çalıştığı hedef sıcaklığın raporlanabilmesi için bir özelliği kullanabilir. IoT Central yazılabilir özellikleri ayarlayabilirsiniz.

Aşağıdaki tabloda bir özellik yeteneği için yapılandırma ayarları gösterilmektedir:

| Alan | Açıklama |
| ----- | ----------- |
| Görünen Ad | Panolar ve formlarda kullanılan özellik değeri için görünen ad. |
| Name | Özelliğin adı. IoT Central görünen adından Bu alan için bir değer oluşturur, ancak gerekirse kendi değerini seçebilirsiniz. Bu alanın alfasayısal olması gerekir. |
| Yetenek Türü | Özelliði. |
| Anlamsal tür | Özelliğin sıcaklık, durum veya olay gibi anlam türü. Anlamsal tür seçimi aşağıdaki alanlardan hangisinin kullanılabildiğini belirler. |
| Şema | Double, String veya Vector gibi özellik veri türü. Kullanılabilir seçimler anlamsal tür tarafından belirlenir. Şema, olay ve durum anlam türleri için kullanılamaz. |
| Yazılabilir | Özellik yazılabilir değilse, cihaz özellik değerlerini IoT Central rapor edebilir. Özellik yazılabilir ise, cihaz özellik değerlerini IoT Central rapor edebilir ve IoT Central Özellik güncelleştirmelerini cihaza gönderebilir.
| Önem Derecesi | Yalnızca olay anlam türü için kullanılabilir. Önem derecesi **hata**, **bilgi**veya **uyarılardır**. |
| Durum değerleri | Yalnızca durum anlam türü için kullanılabilir. Her birinin görünen adı, adı, sabit listesi türü ve değeri olan olası durum değerlerini tanımlayın. |
| Birim | **Mph**, **%** veya ** &deg; C**gibi özellik değeri için bir birim. |
| Görüntüleme birimi | Panolar ve formlarda kullanılacak bir görüntüleme birimi. |
| Yorum | Özellik yeteneği hakkında herhangi bir açıklama. |
| Description | Özellik yeteneğinin açıklaması. |

### <a name="commands"></a>Komutlar

IoT Central 'tan cihaz komutlarını çağırabilirsiniz. Komutlar isteğe bağlı olarak parametreleri cihaza iletir ve cihazdan bir yanıt alır. Örneğin, bir cihazı 10 saniye içinde yeniden başlatmak için bir komut çağırabilirsiniz.

Aşağıdaki tabloda, bir komut özelliğine ait yapılandırma ayarları gösterilmektedir:

| Alan | Açıklama |
| ----- | ----------- |
| Görünen Ad | Panolar ve formlarda kullanılan komutun görünen adı. |
| Name | Komutun adı. IoT Central görünen adından Bu alan için bir değer oluşturur, ancak gerekirse kendi değerini seçebilirsiniz. Bu alanın alfasayısal olması gerekir. |
| Yetenek Türü | Komutundaki. |
| Komut | `SynchronousExecutionType`. |
| Yorum | Komut özelliğiyle ilgili herhangi bir yorum. |
| Description | Komut yeteneğinin açıklaması. |
| İstek | Etkinleştirilirse, istek parametresinin tanımı: ad, görünen ad, şema, birim ve görüntü birimi. |
| Yanıt | Etkinleştirilirse, komut yanıtının tanımı: ad, görünen ad, şema, birim ve görüntü birimi. |

## <a name="manage-an-interface"></a>Arabirim yönetme

Arabirimi yayımlamadıysanız, arabirim tarafından tanımlanan özellikleri düzenleyebilirsiniz. Arabirimi yayımladıktan sonra, herhangi bir değişiklik yapmak istiyorsanız, cihaz şablonunun yeni bir sürümünü oluşturmanız ve arabirimi sürümüne uygulamanız gerekir. **Özelleştirme** bölümünde, sürüm oluşturma gerektirmeyen değişiklikler (görünen adlar veya birimler gibi) yapabilirsiniz.

Ayrıca, başka bir yetenek modelinde yeniden kullanmak istiyorsanız, bir JSON dosyası olarak arayüzü dışarı aktarabilirsiniz.

## <a name="add-cloud-properties"></a>Bulut özelliklerini ekleme

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

Panonuzu görüntülemek ve test etmek için **Önizleme cihazını Yapılandır**' ı seçin. Bu, işletmeni yayımlandıktan sonra bir kez gördüğünde panoyu görmenizi sağlar. Görünümlerinizin doğru verileri göstermesini doğrulamak için bu seçeneği kullanın. Aşağıdakilerden birini seçebilirsiniz:

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

## <a name="publish-a-device-template"></a>Cihaz şablonunu yayımlama

Cihaz yetenek modelinizi uygulayan bir cihazı bağlayabilmeniz için önce cihaz şablonunuzu yayımlamanız gerekir.

Bir cihaz şablonu yayımladıktan sonra, yalnızca cihaz yetenek modelinde sınırlı değişiklikler yapabilirsiniz. Bir arabirimi değiştirmek için [Yeni bir sürüm oluşturmanız ve yayımlamanız](./howto-version-device-template.md)gerekir.

Bir cihaz şablonu yayımlamak için, cihaz şablonunuza gidin ve **Yayımla**' yı seçin.

Bir cihaz şablonu yayımladıktan sonra, bir operatör **cihazlar** sayfasına gidebilir ve cihaz şablonunuzu kullanan gerçek ya da sanal cihazları ekleyebilir. Değişiklik yaparken cihaz şablonunuzu değiştirmeye ve kaydetmeye devam edebilirsiniz. Bu değişiklikleri, **cihazlar** sayfasında görüntülenecek olan işlece iletmek istediğinizde, her seferinde **Yayımla** ' yı seçmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bir cihaz geliştiricisiyseniz, önerilen bir sonraki adım [cihaz şablonu sürümü oluşturma](./howto-version-device-template.md)hakkında bilgi almak için kullanılır.
