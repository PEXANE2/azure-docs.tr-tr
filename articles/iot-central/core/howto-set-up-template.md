---
title: Azure IoT Central'da yeni bir IoT aygıt türü tanımlayın | Microsoft Dokümanlar
description: Bu makalede, bir oluşturucu olarak Azure IoT Merkezi uygulamanızda yeni bir Azure IoT aygıt şablonu nasıl oluşturulabileceğiniz gösterilmektedir. Türünüz için telemetriyi, durumu, özellikleri ve komutları tanımlarsınız.
author: dominicbetts
ms.author: dobett
ms.date: 12/06/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 6f93d74653aab78e48e613ddf9252a0876548b95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80157679"
---
# <a name="define-a-new-iot-device-type-in-your-azure-iot-central-application"></a>Azure IoT Merkezi uygulamanızda yeni bir IoT aygıt türü tanımlama

Aygıt şablonu, Azure IoT Merkezi uygulamasına bağlanan bir aygıt türünün özelliklerini ve davranışlarını tanımlayan bir plandır.

Örneğin, bir oluşturucu, bağlı bir fan için aşağıdaki özelliklere sahip bir aygıt şablonu oluşturabilir:

- Sıcaklık telemetrisi gönderir
- Konum özelliği gönderir
- Fan motoru hata olayları gönderir
- Fan işletim durumu gönderir
- Yazılabilir fan hızı özelliği sağlar
- Aygıtı yeniden başlatmak için bir komut sağlar
- Pano aracılığıyla aygıtın genel görünümünü sağlar

Bu aygıt şablonundan, bir operatör gerçek fan aygıtları oluşturabilir ve bağlayabilir. Tüm bu fanlar, operatörlerin bunları izlemek ve yönetmek için kullandığı ölçümlere, özelliklere ve komutlara sahiptir. Operatörler fan aygıtları ile etkileşim kurmak için cihaz panoları ve formları kullanın.

> [!NOTE]
> Aygıt şablonlarını yalnızca oluşturucular ve yöneticiler oluşturabilir, düzenleyebilir ve silebilir. Herhangi bir kullanıcı, aygıtlar **sayfasında** varolan aygıt şablonlarından aygıtlar oluşturabilir.

[IoT Tak ve Çalıştır (önizleme), IoT](../../iot-pnp/overview-iot-plug-and-play.md) Central'ın herhangi bir gömülü aygıt kodu yazmadan cihazları tümleştirmesini sağlar. IoT Tak ve Çalıştır'ın (önizleme) özünde, aygıt özelliklerini açıklayan bir aygıt yetenek modeli şeması yer alan bir model şeması yer alan bir modeldir. Bir IoT Merkezi uygulamasında, aygıt şablonları bu IoT Tak ve Çalıştır (önizleme) aygıt özelliği modellerini kullanır.

Oluşturucu olarak, aygıt şablonları oluşturmak için çeşitli seçenekleriniz vardır:

- Aygıt şablonunu IoT Central'da tasarlayın ve aygıt kodunda aygıt yetenek modelini uygulayın.
- [IoT aygıt kataloğuiçin Azure Sertifikalı'ndan](https://aka.ms/iotdevcat)bir aygıt özelliği modeli alma. Ardından, IoT Merkezi uygulama gereksinimleriniz için bulut özellikleri, özelleştirmeler ve panolar ekleyin.
- Visual Studio Code'u kullanarak bir aygıt yeteneği modeli oluşturun. Cihaz kodunuzu modelden uygulayın. Aygıt yetenek modelini IoT Merkezi uygulamanıza el ile aktarın ve ardından IoT Merkezi uygulama gereksinimlerinize bulut özellikleri, özelleştirmeler ve panolar ekleyin.
- Visual Studio Code'u kullanarak bir aygıt yeteneği modeli oluşturun. Cihaz kodunuzu modelden uygulayın ve aygıtın ilk bağlantısını kullanarak gerçek cihazınızı IoT Central uygulamanıza bağlayın. IoT Central, cihaz yetenek modelini sizin için genel depodan bulur ve içeri niçin içeriye ilerler. Daha sonra ioT Merkezi uygulamanızın ihtiyaç duyduğu bulut özelliklerini, özelleştirmeleri ve panoları aygıt şablonuna ekleyebilirsiniz.

## <a name="create-a-device-template-from-the-device-catalog"></a>Aygıt kataloğundan aygıt şablonu oluşturma

Bir oluşturucu olarak, IoT Tak ve Çalıştır (önizleme) sertifikalı bir cihaz kullanarak çözümünüzü hızla oluşturmaya başlayabilirsiniz. [Azure IoT Aygıt Kataloğu'ndaki](https://catalog.azureiotsolutions.com/alldevices)listeye bakın. IoT Central, aygıt kataloğuyla entegre olur, böylece bu IoT Tak ve Çalıştır (önizleme) sertifikalı aygıtlardan herhangi bir aygıt yetenek modelini içe aktarabilirsiniz. IoT Central'daki bu aygıtlardan birinden aygıt şablonu oluşturmak için:

1. IoT Central uygulamanızdaki **Aygıt Şablonları** sayfasına gidin.
1. **+ Yeni'yi**seçin ve ardından katalogdan IoT Tak ve Çalıştır (önizleme) sertifikalı cihazlardan herhangi birini seçin. IoT Central, bu aygıt yetenek modelini temel alan bir aygıt şablonu oluşturur.
1. Aygıt şablonunuza bulut özellikleri, özelleştirmeler veya görünümler ekleyin.
1. Şablonu operatörlerin aygıtları görüntülemesi ve bağlaması için kullanılabilir hale getirmek için **Yayımla'yı** seçin.

## <a name="create-a-device-template-from-scratch"></a>Sıfırdan bir aygıt şablonu oluşturma

Aygıt şablonu şunları içerir:

- Aygıtın uyguladığı telemetriyi, özellikleri ve komutları belirten bir _aygıt yeteneği modeli._ Bu özellikler bir veya daha fazla arabirim halinde düzenlenir.
- IoT Central uygulamanızın aygıtlarınız hakkında depoladığınız bilgileri tanımlayan _bulut özellikleri._ Örneğin, bir bulut özelliği bir aygıtın en son hizmet verildiği tarihi kaydedebilir. Bu bilgiler aygıtla hiçbir zaman paylaşılmaz.
- _Özelleştirmeler_ oluşturucu nun aygıt özelliği modelindeki bazı tanımları geçersiz kılmasına izin vermez. Örneğin, oluşturucu bir aygıt özelliğinin adını geçersiz kılabilir. Özellik adları IoT Merkezi panolarında ve formlarında görünür.
- _Panolar ve formlar,_ oluşturucunun operatörlerin uygulamanıza bağlı aygıtları izlemesine ve yönetmesine olanak tanıyan bir Kullanıcı Durumu oluşturmasına olanak tanır.

IoT Central'da bir aygıt şablonu oluşturmak için:

1. IoT Central uygulamanızdaki **Aygıt Şablonları** sayfasına gidin.
1. Seçin **+ Yeni** > **Özel**.
1. **Şablonunuzun çevre sensörü**gibi bir ad girin.
1. **Enter**'a basın. IoT Central boş bir aygıt şablonu oluşturur.

## <a name="manage-a-device-template"></a>Aygıt şablonu yönetme

Şablonun ana sayfasından şablonu yeniden adlandırabilir veya silebilirsiniz.

Şablonunuza bir aygıt yeteneği modeli ekledikten sonra yayımlayabilirsiniz. Şablonu yayımlayana kadar, operatörlerinizin **Cihazlar** sayfasında görmesi için bu şablona dayalı bir aygıt bağlayamazsınız.

## <a name="create-a-capability-model"></a>Yetenek modeli oluşturma

Aygıt yeteneği modeli oluşturmak için şunları yapabilirsiniz:

- Sıfırdan özel bir model oluşturmak için IoT Central'ı kullanın.
- JSON dosyasından bir model alma. Bir aygıt oluşturucu, uygulamanız için bir aygıt yetenek modeli yazar görsel stüdyo kodu kullanmış olabilir.
- Aygıt Kataloğu'ndan aygıtlardan birini seçin. Bu seçenek, üreticinin bu aygıt için yayımettiği aygıt yetenek modelini içeri içeri yedirer. Bu gibi içe aktarılan bir aygıt yeteneği modeli otomatik olarak yayımlanır.

## <a name="manage-a-capability-model"></a>Yetenek modelini yönetme

Bir aygıt yeteneği modeli oluşturduktan sonra şunları yapabilirsiniz:

- Modele arabirimler ekleyin. Bir modelin en az bir arabirimi olmalıdır.
- Kimliği, ad alanı ve adı gibi model meta verilerini düzenleme.
- Modeli silin.

## <a name="create-an-interface"></a>Arayüz oluşturma

Aygıt özelliğinin en az bir arabirimi olmalıdır. Arayüz, yeniden kullanılabilir bir yetenek koleksiyonudur.

Arabirim oluşturmak için:

1. Aygıt yeteneği modelinize gidin ve **+ Arabirim Ekle'yi**seçin.

1. Arayüz **Seç** sayfasında şunları yapabilirsiniz:

    - Sıfırdan özel bir arayüz oluşturun.
    - Bir dosyadan varolan bir arabirim alma. Bir aygıt oluşturucu, aygıtınız için bir arabirim oluşturmak için Visual Studio Code'u kullanmış olabilir.
    - **Aygıt Bilgileri** arabirimi gibi standart arabirimlerden birini seçin. Standart arabirimler birçok aygıtta ortak olan yetenekleri belirtir. Bu standart arabirimler Azure IoT tarafından yayımlanır ve sürümlendirilebilir veya düzenlenemez.

1. Arabirim oluşturduktan sonra, arabirimin görüntü adını değiştirmek için **Kimliği Edin'i** seçin.

1. Sıfırdan özel bir arabirim oluşturmayı seçerseniz, cihazınızın özelliklerini ekleyebilirsiniz. Aygıt yetenekleri telemetri, özellikler ve komutlardır.

### <a name="telemetry"></a>Telemetri

Telemetri, genellikle bir sensörden cihazdan gönderilen değerler akışıdır. Örneğin, bir sensör ortam sıcaklığını bildirebilir.

Aşağıdaki tablo, bir telemetri özelliği için yapılandırma ayarlarını gösterir:

| Alan | Açıklama |
| ----- | ----------- |
| Görünen Ad | Panolarda ve formlarda kullanılan telemetri değerinin görüntü adı. |
| Adı | Telemetri iletisindeki alanın adı. IoT Central bu alan için görüntü adından bir değer oluşturur, ancak gerekirse kendi değerinizi seçebilirsiniz. |
| Yetenek Türü | Telemetri. |
| Anlamsal Tip | Sıcaklık, durum veya olay gibi telemetrinin anlamsal türü. Anlamsal tür seçimi, aşağıdaki alanlardan hangisinin kullanılabilir olduğunu belirler. |
| Şema | Çift, dize veya vektör gibi telemetri veri türü. Kullanılabilir seçenekler anlamsal türe göre belirlenir. Şema olay ve durum semantik türleri için kullanılamaz. |
| Severity | Yalnızca olay semantik türü için kullanılabilir. Önemleri **Hata**, **Bilgi**veya **Uyarıdır.** |
| Durum Değerleri | Yalnızca durum semantik türü için kullanılabilir. Her biri görüntü adı, ad, numaralandırma türü ve değeri olan olası durum değerlerini tanımlayın. |
| Birim | **Mph**, **%** veya ** &deg;C**gibi telemetri değeri için bir birim. |
| Görüntü Ünitesi | Panolarda ve formlarda kullanılmak üzere bir görüntü ünitesi. |
| Açıklama | Telemetri yeteneği hakkında herhangi bir yorum. |
| Açıklama | Telemetri yeteneğinin açıklaması. |

### <a name="properties"></a>Özellikler

Özellikler zaman içinde nokta değerlerini temsil ediyor. Örneğin, bir aygıt ulaşmaya çalıştığı hedef sıcaklığı bildirmek için bir özellik kullanabilir. IoT Central'dan yazılabilir özellikleri ayarlayabilirsiniz.

Aşağıdaki tablo, bir özellik özelliğiiçin yapılandırma ayarlarını gösterir:

| Alan | Açıklama |
| ----- | ----------- |
| Görünen Ad | Panolarda ve formlarda kullanılan özellik değerinin görüntü adı. |
| Adı | Özelliğin adı. IoT Central bu alan için görüntü adından bir değer oluşturur, ancak gerekirse kendi değerinizi seçebilirsiniz. |
| Yetenek Türü | Özellik. |
| Anlamsal Tip | Sıcaklık, durum veya olay gibi özelliğin anlamsal türü. Anlamsal tür seçimi, aşağıdaki alanlardan hangisinin kullanılabilir olduğunu belirler. |
| Şema | Çift, dize veya vektör gibi özellik veri türü. Kullanılabilir seçenekler anlamsal türe göre belirlenir. Şema olay ve durum semantik türleri için kullanılamaz. |
| Yazılabilir | Özellik yazılamazsa, aygıt özellik değerlerini IoT Central'a bildirebilir. Özellik yazılabilirse, aygıt özellik değerlerini IoT Central'a bildirebilir ve IoT Central aygıta özellik güncelleştirmeleri gönderebilir.
| Severity | Yalnızca olay semantik türü için kullanılabilir. Önemleri **Hata**, **Bilgi**veya **Uyarıdır.** |
| Durum Değerleri | Yalnızca durum semantik türü için kullanılabilir. Her biri görüntü adı, ad, numaralandırma türü ve değeri olan olası durum değerlerini tanımlayın. |
| Birim | **Mph**, **%** veya ** &deg;C**gibi özellik değeri için bir birim. |
| Görüntü Ünitesi | Panolarda ve formlarda kullanılmak üzere bir görüntü ünitesi. |
| Açıklama | Özellik yeteneği hakkında herhangi bir yorum. |
| Açıklama | Özellik kapasitesinin açıklaması. |

### <a name="commands"></a>Komutlar

IoT Central'dan aygıt komutlarını arayabilirsiniz. Komutlar isteğe bağlı olarak parametreleri aygıta geçirir ve aygıttan yanıt alır. Örneğin, bir aygıtı 10 saniye içinde yeniden başlatmak için bir komut çağırabilirsiniz.

Aşağıdaki tablo, komut özelliğinin yapılandırma ayarlarını gösterir:

| Alan | Açıklama |
| ----- | ----------- |
| Görünen Ad | Panolarda ve formlarda kullanılan komutun görüntü adı. |
| Adı | Komutun adı. IoT Central bu alan için görüntü adından bir değer oluşturur, ancak gerekirse kendi değerinizi seçebilirsiniz. |
| Yetenek Türü | Komut. |
| Komut | `SynchronousExecutionType`. |
| Açıklama | Komut yeteneği hakkında herhangi bir yorum. |
| Açıklama | Komut yeteneğinin açıklaması. |
| İstek | Etkinleştirilmişse, istek parametresi tanımı, dahil: ad, görüntü adı, şema, birim ve görüntü birimi. |
| Yanıt | Etkinse, ad, görüntü adı, şema, birim ve görüntü birimi de dahil olmak üzere komut yanıtı tanımı. |

## <a name="manage-an-interface"></a>Arabirimi yönetme

Arabirimi yayımlamamışsanız, arabirim tarafından tanımlanan yetenekleri yeniden düzenleme yapabilirsiniz. Arabirimi yayımladıktan sonra, herhangi bir değişiklik yapmak istiyorsanız, aygıt şablonunun yeni bir sürümünü oluşturmanız ve arabirimi sürüm oluşturmanız gerekir. Görüntü adları veya birimler gibi sürüm gerektirmeyen değişiklikleri **Özelleştir** bölümünde yapabilirsiniz.

Başka bir yetenek modelinde yeniden kullanmak istiyorsanız, arabirimi JSON dosyası olarak da dışa aktarabilirsiniz.

## <a name="add-cloud-properties"></a>Bulut özellikleri ekleme

IoT Central'da aygıtlar hakkında bilgi depolamak için bulut özelliklerini kullanın. Bulut özellikleri hiçbir zaman bir aygıta gönderilmez. Örneğin, aygıtı yükleyen müşterinin adını veya aygıtın son hizmet tarihini depolamak için bulut özelliklerini kullanabilirsiniz.

Aşağıdaki tablo, bir bulut özelliğinin yapılandırma ayarlarını gösterir:

| Alan | Açıklama |
| ----- | ----------- |
| Görünen Ad | Panolarda ve formlarda kullanılan bulut özellik değerinin görüntü adı. |
| Adı | Bulut özelliğinin adı. IoT Central bu alan için görüntü adından bir değer oluşturur, ancak gerekirse kendi değerinizi seçebilirsiniz. |
| Anlamsal Tip | Sıcaklık, durum veya olay gibi özelliğin anlamsal türü. Anlamsal tür seçimi, aşağıdaki alanlardan hangisinin kullanılabilir olduğunu belirler. |
| Şema | Çift, dize veya vektör gibi bulut özelliği veri türü. Kullanılabilir seçenekler anlamsal türe göre belirlenir. |

## <a name="add-customizations"></a>Özelleştirme ekleme

İçe aktarılan arabirimi değiştirmeniz veya bir kapasiteye IoT Merkezi'ne özgü özellikler eklemeniz gerektiğinde özelleştirmeleri kullanın. Yalnızca arabirim uyumluluğunu bozmayan alanları özelleştirebilirsiniz. Örneğin, şunları yapabilirsiniz:

- Bir özelliğin görüntü adını ve birimlerini özelleştirin.
- Değer grafikte göründüğünde kullanmak üzere varsayılan renk ekleyin.
- Bir özellik için ilk, minimum ve maksimum değerleri belirtin.

Yetenek adını veya yetenek türünü özelleştiremezsiniz. **Özelleştir** bölümünde yapamadığınız değişiklikler varsa, yeteneği değiştirmek için aygıt şablonunuzu ve arabiriminizi sürüm oluşturmanız gerekir.

### <a name="generate-default-views"></a>Varsayılan görünümler oluşturma

Varsayılan görünümler oluşturmak, önemli aygıt bilgilerinizi görselleştirmenin hızlı bir yoludur. Aygıt şablonunuzun oluşturulması için en fazla üç varsayılan görünüm oluşturdunuz:

- **Komutlar** aygıt komutlarıyla bir görünüm sağlar ve operatörünüzün bunları cihazınıza göndermesine olanak tanır.
- **Genel bakış,** çizelgeleri ve ölçümleri görüntüleyen aygıt telemetrisi ile bir görünüm sağlar.
- **Hakkında** aygıt bilgileri ile bir görünüm sağlar, aygıt özelliklerini görüntüleme.

**Varsayılan görünümleri oluştur'u**seçtikten sonra, bunların aygıt şablonunuzun **Görünümler** bölümüne otomatik olarak eklendiğini görürsünüz.

## <a name="add-dashboards"></a>Pano ekleme

Operatörlerin grafikleri ve ölçümleri kullanarak aygıtı görselleştirmesini sağlamak için aygıt şablonuna panolar ekleyin. Aygıt şablonu için birden çok panonuz olabilir.

Aygıt şablonuna pano eklemek için:

1. Aygıt şablonunuza gidin ve **Görünümler'i**seçin.
1. **Aygıtı Görselleştirme'yi**seçin.
1. **Pano Adı'na**panonuz için bir ad girin.
1. Statik, özellik, bulut özelliği, telemetri ve komut kutucukları listesinden panonuza kutucukekleyin. Panonuza eklemek istediğiniz kutucukları sürükleyin ve bırakın.
1. Tek bir grafik döşemesi üzerinde birden çok telemetri değeri çizmek için, telemetri değerlerini seçin ve sonra **Birleştir'i**seçin.
1. Eklediğiniz her döşemeyi, verileri görüntüleme şeklini özelleştirmek için yapılandırın. Bunu dişli simgesini seçerek veya grafik döşemenizde **Değiştir yapılandırmasını** seçerek yapabilirsiniz.
1. Panonuzdaki kutucukları düzenleyin ve yeniden boyutlandırın.
1. Değişiklikleri kaydedin.

### <a name="configure-preview-device-to-view-dashboard"></a>Önizleme aygıtını panogörüntülemek için yapılandırın

Panonuzu görüntülemek ve test etmek için **önizleme aygıtını Yapılanla'yı**seçin. Bu, panoyayımlandıktan sonra operatörünüzün gördüğü şekilde görmenizi sağlar. Görünümlerinizin doğru verileri gösterdiğini doğrulamak için bu seçeneği kullanın. Aşağıdakilerden birini seçebilirsiniz:

- Önizleme cihazı yok.
- Aygıt şablonunuzun yapılandırması için yapılandırdığınız gerçek test aygıtı.
- Cihazınızı kullanarak uygulamanızda varolan bir aygıt.

## <a name="add-forms"></a>Form ekleme

Operatörlerin özellikleri görüntüleyerek ve ayarlayarak aygıtı yönetmesini sağlamak için aygıt şablonuna form ekleyin. Operatörler yalnızca bulut özelliklerini ve yazılabilir aygıt özelliklerini edinebilir. Aygıt şablonu için birden çok form alabilirsiniz.

Aygıt şablonuna form eklemek için:

1. Aygıt şablonunuza gidin ve **Görünümler'i**seçin.
1. **Düzenleme Aygıtı ve Bulut verilerini**seçin.
1. **Form Adını'na**formunuzun adını girin.
1. Formunuzu düzenlemek için kullanılacak sütun sayısını seçin.
1. Formunuzdaki varolan bir bölüme özellikler ekleyin veya özellikleri seçin ve **Bölüm Ekle'yi**seçin. Özellikleri formunuzda gruplandırmak için bölümleri kullanın. Bir bölüme başlık ekleyebilirsiniz.
1. Formdaki her özelliği davranışını özelleştirmek için yapılandırın.
1. Formunuzdaözellikleri düzenleyin.
1. Değişiklikleri kaydedin.

## <a name="publish-a-device-template"></a>Cihaz şablonunu yayımlama

Aygıt yeteneği modelinizi uygulayan bir aygıtı bağlamadan önce aygıt şablonunuzu yayımlamanız gerekir.

Aygıt şablonu yayımladıktan sonra, aygıt yeteneği modelinde yalnızca sınırlı değişiklikler yapabilirsiniz. Bir arabirimi değiştirmek için [yeni bir sürüm oluşturmanız ve yayımlamanız](./howto-version-device-template.md)gerekir.

Aygıt şablonu yayımlamak için aygıt şablonunuzu size gidin ve **Yayımla'yı**seçin.

Aygıt şablonunu yayımladıktan sonra, bir operatör **Aygıtlar** sayfasına gidebilir ve aygıt şablonunuzu kullanan gerçek veya simüle edilmiş aygıtlar ekleyebilir. Değişiklik yaparken cihaz şablonunuzu değiştirmeye ve kaydetmeye devam edebilirsiniz. Bu değişiklikleri **Aygıtlar** sayfasının altında görüntülemek üzere işletime itmek istediğinizde, her seferinde **Yayımla'yı** seçmeniz gerekir.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Yeni bir IoT aygıt şablonu oluşturun.
* Bulut özellikleri oluşturun.
* Özelleştirmeler oluşturun.
* Aygıt telemetrisi için bir görselleştirme tanımlayın.
* Aygıt şablonunuzu yayımlayın.

Sonra, şunları yapabilirsiniz:

> [!div class="nextstepaction"]
> [Cihaz bağlama](howto-connect-devkit.md)
