---
title: FHıR için Azure IoT Bağlayıcısı (Önizleme)-sorun giderme kılavuzu ve nasıl yapılır
description: Genel Azure IoT Bağlayıcısı 'nda FHıR (Önizleme) hata iletileri ve koşulları için sorun giderme ve eşleme dosyalarını kopyalama
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: troubleshooting
ms.date: 11/13/2020
ms.author: jasteppe
ms.openlocfilehash: 3eef7354f7197f60e8abd1b5522393bf00a6203f
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2021
ms.locfileid: "103020588"
---
# <a name="azure-iot-connector-for-fhir-preview-troubleshooting-guide"></a>FHıR için Azure IoT Bağlayıcısı (Önizleme) sorun giderme kılavuzu

Bu makalede, hızlı sağlık birlikte çalışabilirlik kaynakları için genel Azure IoT Bağlayıcısı sorunlarını giderme adımları sağlanır (FHıR&#174;) * hata iletileri ve koşulları.  

Ayrıca, FHıR dönüştürme eşlemeleri JSON (örneğin: cihaz ve FHıR) için Azure IoT bağlayıcısının kopyalarını oluşturmayı öğreneceksiniz.  

Dönüştürme ve arşivleme için JSON kopyalarını Azure portal dışında kullanarak değiştirebilirsiniz.  

> [!TIP]
> FHıR için Azure IoT Bağlayıcısı için bir [Azure teknik destek](https://azure.microsoft.com/support/create-ticket/) bileti açarsanız, sorun giderme işlemine yardımcı olmak için dönüştürme eşleme JSON 'larınızın kopyalarını eklediğinizden emin olun.

## <a name="device-and-fhir-conversion-mapping-json-template-validations-for-azure-iot-connector-for-fhir-preview"></a>FHıR için Azure IoT Bağlayıcısı için cihaz ve FHıR dönüştürme eşleme JSON şablonu doğrulamaları (Önizleme)
Bu bölümde, FHıR için Azure IoT bağlayıcısının, kullanım için kaydedilmesine izin vermeden önce cihaz ve FHıR dönüştürme eşleme JSON şablonlarını doğrulamak üzere gerçekleştirdiği doğrulama süreci hakkında bilgi edineceksiniz.  Bu öğeler cihazda gereklidir ve FHıR dönüştürme eşlemesi JSON.

**Cihaz eşleme**

|Öğe|Gerekli|
|:-------|:------|
|TypeName|Doğru|
|TypeMatchExpression|Doğru|
|DeviceIdExpression|Doğru|
|TimestampExpression|Doğru|
|Values []. ValueName|Doğru|
|Values []. ValueExpression|Doğru|

> [!NOTE]
> Values []. ValueName ve Values []. ValueExpression
>
> Bu öğeler yalnızca dizide bir değer girişi varsa gereklidir; hiçbir değer eşlenmediğinden geçerli değildir. Bu, gönderilen telemetri bir olay olduğunda kullanılır. Örneğin: takılabilir IoMT cihazı ne zaman konur veya kaldırılır. Bu öğe (ler), FHıR ile eşleşen ve yayan Azure IoT bağlayıcısının adı dışında herhangi bir değere sahip değildir. FHıR dönüştürmesinde, FHıR için Azure IoT Bağlayıcısı, anlam türünü temel alan kod uyumlu bir kavram ile eşler, hiçbir gerçek değer doldurulur.

**FHıR eşleme**

|Öğe|Gerekli|
|:------|:-------|
|TypeName|Doğru|

> [!NOTE]
> Bu, şu anda doğrulanan tek gereken FHıR eşleme öğesidir.

## <a name="error-messages-and-fixes-for-azure-iot-connector-for-fhir-preview"></a>FHıR için Azure IoT Bağlayıcısı hata iletileri ve düzeltmeleri (Önizleme)

|İleti|Görüntülenemeyecek|Koşul|Düzeltme| 
|-------|---------|---------|---|
|Geçersiz eşleme adı, eşleme adı cihaz veya FHıR olmalıdır.|API|Sağlanan eşleme türü cihaz veya FHıR değil.|Desteklenen iki eşleme türünden birini kullanın (örneğin: cihaz veya FHıR).|
|Doğrulama başarısız oldu. Gerekli bilgiler eksik veya geçersiz.|API ve Azure portal|Gerekli bilgi veya öğe için bir dönüştürme eşlemesinin kaydedilmeye çalışılıyor.|Eksik dönüştürme eşleme bilgilerini veya öğesini ekleyin ve dönüştürme eşlemesini tekrar kaydetmeyi deneyin.|
|Anahtar parametrelerini yeniden oluştur tanımlı değil.|API|Anahtar isteğini yeniden üret.|Yeniden oluşturma anahtar isteğine parametreleri ekleyin.|
|Bu abonelikte sağlanabilen IoT Bağlayıcısı örneklerinin maksimum sayısına ulaşıldı.|API ve Azure portal|FHıR abonelik kotasına ulaşılan Azure IoT Bağlayıcısı (abonelik başına varsayılan değer (2)).|FHıR için Azure IoT bağlayıcısının mevcut örneklerinden birini silin.  Abonelik kotasına ulaşmamış farklı bir abonelik kullanın.  Abonelik kotası artışı iste.|
|IoT Bağlayıcısı için Azure API 'SI için kaynak taşıma özelliği, FHıR kaynağı için desteklenmiyor.|API ve Azure portal|FHIR için Azure IoT bağlayıcısının bir veya daha fazla örneği olan FHıR kaynağı için bir Azure API 'SI üzerinde taşıma işlemi yapılmaya çalışılıyor.|Taşıma işlemini yapmak için FHAR Azure IoT bağlayıcısının mevcut örneklerini silin.|
|IoT Bağlayıcısı sağlanmadı.|API|Üst öğe (FHıR için Azure IoT Bağlayıcısı) sağlanmadığından alt hizmetler (bağlantılar & eşlemeler) kullanılmaya çalışılıyor.|FHıR için bir Azure IoT Bağlayıcısı sağlayın.|
|İstek desteklenmiyor.|API|Belirli API isteği desteklenmiyor.|Doğru API isteğini kullanın.|
|Hesap yok.|API|FHAR için Azure IoT Bağlayıcısı ekleme girişimi ve FHıR için Azure API kaynağı yok.|FHıR kaynağı için Azure API 'SI oluşturun ve ardından işlemi yeniden deneyin.|
|FHıR kaynağı için Azure API 'SI IoT Bağlayıcısı için desteklenmez.|API|FHıR için Azure API 'sinin uyumsuz bir sürümüyle fhır için bir Azure IoT Bağlayıcısı kullanılmaya çalışılıyor.|FHıR kaynağı (sürüm R4) için yeni bir Azure API 'SI oluşturun veya FHıR kaynağı için mevcut bir Azure API 'SI (sürüm R4) kullanın.

## <a name="why-is-my-azure-iot-connector-for-fhir-preview-data-not-showing-up-in-azure-api-for-fhir"></a>Azure IoT Bağlayıcım, fhır için Azure API 'de görünmüyor.

|Olası sorunlar|Düzeltmeler|
|----------------|-----|
|Verilerin işlenmesi devam ediyor.|Veriler, toplu iş (YAKLAŞıK 15 dakikada bir) için Azure API 'sine yöneliktir.  Verilerin hala işlenmesine ve verilerin FHıR için Azure API 'sinde kalıcı olması için ek süre gerekebilir.|
|Cihaz dönüştürme eşleme JSON 'ı yapılandırılmadı.|Uyumsuz cihaz dönüştürme eşlemesi JSON 'ı yapılandırın ve kaydedin.|
|FHıR dönüştürme eşlemesi JSON yapılandırılmadı.|Uygun FHıR dönüştürme eşlemesi JSON 'ı yapılandırın ve kaydedin.|
|Cihaz iletisinde cihaz eşlemesinde tanımlanmış bir beklenen ifade yok.|Cihaz iletisinde tanımlanan, cihaz eşlemesinde eşleşen belirteçlerde tanımlanan JsonPath ifadelerin doğrulanması.|
|FHıR için Azure API 'sinde bir cihaz kaynağı oluşturulmadı (çözüm türü: yalnızca arama) *.|FHıR için Azure API 'sinde geçerli bir cihaz kaynağı oluşturun. Cihaz kaynağının gelen iletide belirtilen cihaz tanımlayıcısıyla eşleşen bir tanımlayıcı içerdiğinden emin olun.|
|FHıR için Azure API 'sinde bir hasta kaynağı oluşturulmadı (çözüm türü: yalnızca arama) *.|FHıR için Azure API 'sinde geçerli bir hasta kaynağı oluşturun.|
|Device. hasta başvurusu ayarlanmadı veya başvuru geçersiz (çözüm türü: yalnızca arama) *.|Cihaz kaynağının bir hasta kaynağına geçerli bir [başvuru](https://www.hl7.org/fhir/device-definitions.html#Device.patient) içerdiğinden emin olun.| 

* Başvuru hızlı başlangıç: FHıR çözüm türleri için Azure IoT bağlayıcısının işlevsel bir açıklaması için [Azure Portal kullanarak Azure IoT bağlayıcısını (Önizleme) dağıtma](iot-fhir-portal-quickstart.md#create-new-azure-iot-connector-for-fhir-preview) (örneğin: arama veya oluşturma).

## <a name="use-metrics-to-troubleshoot-issues-in-azure-iot-connector-for-fhir-preview"></a>Azure IoT Bağlayıcısı ile ilgili sorunları gidermek için ölçümleri kullanma (Önizleme)

FHıR için Azure IoT Bağlayıcısı, veri akışı işleminde Öngörüler sağlamak için birden çok ölçüm oluşturur. Desteklenen ölçülerden birine, FHıR için bir Azure IoT Bağlayıcısı örneği içinde oluşan tüm hataların sayısını sağlayan *Toplam hata* adı verilir.

Her hata, ilişkili birçok özellik ile günlüğe kaydedilir. Her özellik hata hakkında farklı bir boyut sağlar, bu da sorunları belirlemenize ve gidermenize yardımcı olabilir. Bu bölüm, *Toplam* hata ölçümünde her bir hata için yakalanan farklı özellikleri ve bu özellikler için olası değerleri listeler.

> [!NOTE]
> Fhir (Önizleme) için Azure IoT Bağlayıcısı (Önizleme) [ölçümleri sayfasında](iot-metrics-display.md)açıklandığı gibi fhır Için Azure IoT Bağlayıcısı 'nın bir örneği Için *Toplam hata* ölçümüne gidebilirsiniz.

*Toplam hatalar* grafiğine tıklayın ve ardından aşağıda bahsedilen özelliklerden herhangi birini kullanarak hata ölçümünü dilimleyip zaratmak Için *Filtre Ekle* düğmesine tıklayın.

### <a name="the-operation-performed-by-the-azure-iot-connector-for-fhir-preview"></a>FHıR için Azure IoT Bağlayıcısı tarafından gerçekleştirilen işlem (Önizleme)

Bu özellik, hata oluştuğunda IoT Bağlayıcısı tarafından gerçekleştirilen işlemi temsil eder. Bir işlem, bir cihaz iletisini işlerken genellikle veri akışı aşamasını temsil eder. Bu özellik için olası değerlerin listesi aşağıda verilmiştir.

> [!NOTE]
> Daha fazla bilgi için Azure IoT Bağlayıcısı 'nda bulunan farklı veri akışı aşamaları hakkında daha fazla bilgi edinebilirsiniz ( [Önizleme).](iot-data-flow.md)

|Veri akışı aşaması|Description|
|---------------|-----------|
|Kurulum|IoT Bağlayıcısı örneğinizi ayarlamaya özgü işlem|
|Normalleştirme|Cihaz verilerinin normalleştirilme aldığı veri akışı aşaması|
|Gruplandırma|Normalleştirilmiş verilerin gruplandırıldığı veri akışı aşaması|
|FHIRConversion|Gruplanmış normalleştirilmiş verilerin bir FHıR kaynağına dönüştürülebileceği veri akışı aşaması|
|Bilinmiyor|Hata oluştuğunda işlem türü bilinmiyor|

### <a name="the-severity-of-the-error"></a>Hatanın önem derecesi

Bu özellik, gerçekleşen hatanın önem derecesini temsil eder. Bu özellik için olası değerlerin listesi aşağıda verilmiştir.

|Önem Derecesi|Açıklama|
|---------------|-----------|
|Uyarı|Veri akışı işleminde küçük bir sorun var, ancak cihaz iletisi işleme durdurulmaz|
|Hata|Belirli bir cihaz iletisinin işlenmesi bir hatayla çalıştırıldı ve diğer iletiler beklendiği gibi yürütülmeye devam edebilir|
|Kritik|IoT bağlayıcısında bazı sistem düzeyi sorunları var ve işlemek beklenen bir ileti yok|

### <a name="the-type-of-the-error"></a>Hatanın türü

Bu özellik, benzer türdeki hatalara yönelik olarak temel olarak bir mantıksal gruplamayı temsil eden belirli bir hata için bir kategori belirtir. Bu özellik için olası değer listesi aşağıda verilmiştir.

|Hata türü|Description|
|----------|-----------|
|DeviceTemplateError|Cihaz eşleme şablonlarıyla ilgili hatalar|
|DeviceMessageError|Belirli bir cihaz iletisi işlenirken hatalar oluştu|
|FHIRTemplateError|FHıR eşleme şablonlarıyla ilgili hatalar|
|FHIRConversionError|Bir ileti FHıR kaynağına dönüştürülürken hatalar oluştu|
|FHIRResourceError|IoT Bağlayıcısı tarafından başvurulan FHıR sunucusundaki mevcut kaynaklarla ilgili hatalar|
|Fhirsunucuhatası|FHıR sunucusuyla iletişim kurulurken oluşan hatalar|
|GeneralError|Diğer tüm hata türleri|

### <a name="the-name-of-the-error"></a>Hatanın adı

Bu özellik belirli bir hata için ad sağlar. Açıklamaları ve ilişkili hata türleri, önem derecesi ve veri akışı aşamalarıyla birlikte tüm hata adlarının listesi aşağıda verilmiştir.

|Hata adı|Description|Hata türleri|Hata önem derecesi|Veri akışı aşaması|
|----------|-----------|-------------|--------------|------------------|
|MultipleResourceFoundException|FHıR sunucusunda, cihaz iletisinde bulunan ilgili tanımlayıcılar için birden çok hasta veya cihaz kaynağı bulunduğunda hata oluştu|FHIRResourceError|Hata|FHIRConversion|
|TemplateNotFoundException|Bir cihaz veya FHıR eşleme şablonu IoT Bağlayıcısı örneğiyle yapılandırılmadı|DeviceTemplateError, FHIRTemplateError|Kritik|Normalleştirme, FHIRConversion|
|CorrelationIdNotDefinedException|İlişki KIMLIĞI, cihaz eşleme şablonunda belirtilmedi. CorrelationIdNotDefinedException, yalnızca FHıR gözlemlerinin bir bağıntı KIMLIĞI kullanarak cihaz ölçümlerini gruplandırmalıdır ancak doğru şekilde yapılandırılmadığı durumlarda oluşabilecek bir koşullu hatadır|DeviceMessageError|Hata|Normalleştirme|
|Haentdevicemismatchexception|Bu hata, FHıR sunucusundaki cihaz kaynağı bir hasta kaynağına başvuru olduğunda meydana gelir ve bu durum iletide bulunan hasta tanımlayıcısı ile eşleşmez|FHIRResourceError|Hata|FHIRConversionError|
|Haentnotfoundexception|Cihaz iletisinde bulunan cihaz tanımlayıcısıyla ilişkili cihaz FHıR kaynağı tarafından hiçbir hasta FHıR kaynağına başvurulmadı. Bu hata, yalnızca IoT Bağlayıcısı örneği *arama* çözümleme türü ile yapılandırıldığında oluşur|FHIRConversionError|Hata|FHIRConversion|
|DeviceNotFoundException|Cihaz iletisinde bulunan cihaz tanımlayıcısıyla ilişkili FHıR sunucusunda cihaz kaynağı yok|DeviceMessageError|Hata|Normalleştirme|
|PatientIdentityNotDefinedException|Bu hata, cihaz iletisinde hasta tanımlayıcıyı ayrıştırmaya yönelik ifade cihaz eşleme şablonunda yapılandırılmamışsa veya cihaz iletisinde hasta tanımlayıcı yoksa oluşur. Bu hata yalnızca IoT bağlayıcısının çözüm türü *Oluştur* olarak ayarlandığında oluşur|DeviceTemplateError|Kritik|Normalleştirme|
|DeviceIdentityNotDefinedException|Bu hata, cihaz tanımlayıcısı cihaz eşleme şablonunda yapılandırılmadığı veya cihaz iletisinde cihaz tanımlayıcı olmadığı zaman oluşur|DeviceTemplateError|Kritik|Normalleştirme|
|NotSupportedException|Desteklenmeyen biçimdeki cihaz iletisi alındığında hata oluştu|DeviceMessageError|Hata|Normalleştirme|

## <a name="creating-copies-of-the-azure-iot-connector-for-fhir-preview-conversion-mapping-json"></a>Azure IoT bağlayıcısının, FHIR (Önizleme) dönüştürme eşlemesi JSON için kopyalarını oluşturma

FHıR eşleme dosyaları için Azure IoT bağlayıcısının kopyalanması, Azure portal web sitesinin dışında düzenlenebilir ve arşivlenmek üzere yararlı olabilir.

Sorun gidermeye yardımcı olmak üzere bir destek bileti açılırken, eşleme dosya kopyaları Azure teknik desteği 'ne sağlanmalıdır.

> [!NOTE]
> JSON, şu anda cihaz ve FHıR eşleme dosyaları için desteklenen tek biçimdir.

> [!TIP]
> FHıR cihazı için Azure IoT Bağlayıcısı [ve fhır dönüştürme eşleme JSON](iot-mapping-templates.md) hakkında daha fazla bilgi edinin

1. **"Eklentiler"** bölümünde FHIR kaynak panosu IÇIN Azure API 'sinin sol alt tarafındaki **"IoT Bağlayıcısı (Önizleme)"** öğesini seçin.

   :::image type="content" source="media/iot-troubleshoot/map-files-main-with-box.png" alt-text="IoT Connector1" lightbox="media/iot-troubleshoot/map-files-main-with-box.png":::

2. JSON dönüştürme eşlemesini içinden kopyalayacaksınız **"bağlayıcısını"** seçin.

   :::image type="content" source="media/iot-troubleshoot/map-files-select-connector-with-box.png" alt-text="IoT Connector2" lightbox="media/iot-troubleshoot/map-files-select-connector-with-box.png":::

> [!NOTE]
> Bu işlem, **"FHıR eşlemesini yapılandırma"** JSON içeriğini kopyalamak ve kaydetmek için de kullanılabilir.

3. **"Cihaz eşlemeyi Yapılandır"** ı seçin.

    :::image type="content" source="media/iot-troubleshoot/map-files-select-device-with-box.png" alt-text="IoT Connector3" lightbox="media/iot-troubleshoot/map-files-select-device-with-box.png":::

4. JSON içeriğini seçin ve bir kopyalama işlemi yapın (örneğin: Ctrl + c ' yi seçin). 

   :::image type="content" source="media/iot-troubleshoot/map-files-select-device-json-with-box.png" alt-text="IoT Connector4" lightbox="media/iot-troubleshoot/map-files-select-device-json-with-box.png":::

5. Bir düzenleyici içindeki yeni bir dosyaya (örneğin: Visual Studio Code, Notepad) bir yapıştırma işlemi yapın ve dosyayı bir *. JSON uzantısıyla kaydedin.

> [!TIP]
> FHıR için Azure IoT Bağlayıcısı için bir [Azure teknik destek](https://azure.microsoft.com/support/create-ticket/) bileti açarsanız, sorun giderme işlemine yardımcı olmak için dönüştürme eşleme JSON 'larınızın kopyalarını eklediğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

FHıR için Azure IoT Bağlayıcısı hakkında sık sorulan sorulara göz atın.

>[!div class="nextstepaction"]
>[FHıR SSS için Azure IoT Bağlayıcısı](fhir-faq.md)

* Azure portal, FHıR için Azure IoT Bağlayıcısı, IoT Bağlayıcısı (Önizleme) olarak adlandırılır. FHıR, HL7 'in tescilli ticari markasıdır ve HL7 izniyle kullanılır.