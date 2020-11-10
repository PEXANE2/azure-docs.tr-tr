---
title: Azure 'da FHıR hizmetleri hakkında SSS-FHıR için Azure API
description: Fhir API 'Leri ve sürüm desteğinin arkasındaki verilerin depolama konumu gibi FHıR için Azure API 'SI hakkında sık sorulan soruların yanıtlarını alın.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 08/03/2020
ms.author: matjazl
ms.openlocfilehash: 8f4d16931f09f94af81dd4e0f178ce6e0f990881
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94426213"
---
# <a name="frequently-asked-questions-about-the-azure-api-for-fhir"></a>FHıR için Azure API 'SI hakkında sık sorulan sorular

## <a name="azure-api-for-fhir-the-basics"></a>FHıR için Azure API: temel bilgiler

### <a name="what-is-fhir"></a>FHAR nedir?
Hızlı sağlık birlikte çalışabilirlik kaynakları (FHıR-, "yangın"), farklı sistem durumu sistemleri arasında sağlık verileri değişimini etkinleştirmeye yönelik bir birlikte çalışabilirlik standardıdır. Bu standart, HL7 organizasyonu tarafından geliştirilmiştir ve dünyanın dört bir yanındaki sağlık kurumları tarafından benimsenmekte. FHıR 'nin en güncel sürümü, R4 (sürüm 4). FHıR için Azure API 'SI R4 destekler ve ayrıca önceki sürüm STU3 ' i (deneme sürümü 3 için standart) destekler. FHAR hakkında daha fazla bilgi için [HL7.org](http://hl7.org/fhir/summary.html)adresini ziyaret edin.

### <a name="is-the-data-behind-the-fhir-apis-stored-in-azure"></a>Azure 'da depolanan FHAR API 'Lerinin arkasındaki veriler midir?

Evet, veriler Azure 'da yönetilen veritabanlarında depolanır. FHıR için Azure API 'SI, temel alınan veri deposuna doğrudan erişim sağlamaz.

### <a name="what-identity-provider-do-you-support"></a>Hangi kimlik sağlayıcısını destekliyoruz?

Şu anda kimlik sağlayıcısı olarak Microsoft Azure Active Directory destekliyoruz.

### <a name="what-is-the-recovery-point-objective-rpo-for-the-azure-api-for-fhir"></a>FHıR için Azure API 'SI için kurtarma noktası hedefi (RPO) nedir?
FHıR için Azure API, kalıcılık sağlayıcımız olarak Cosmos DB tarafından desteklenir. Bu nedenle, hizmet için RPO [Cosmos DB (tek bölge)](https://docs.microsoft.com/azure/cosmos-db/consistency-levels) eşittir ve 240 dakika <.

### <a name="what-fhir-version-do-you-support"></a>Hangi FHıR sürümünü destekliyoruz?

4.0.0 ve 3.0.1 sürümlerini hem FHıR için Azure API 'SI (PaaS) hem de Azure için FHıR sunucusu (açık kaynak) üzerinde destekliyoruz.

Ayrıntılar için bkz. [desteklenen özellikler](fhir-features-supported.md). [HL7 fhir sürüm geçmişinde](https://hl7.org/fhir/R4/history.html)fhır sürümleri (örn. STU3 ile R4) arasında nelerin değiştiğini okuyun.

FHIR için Azure IoT Bağlayıcısı (Önizleme) Şu anda yalnızca FHıR sürüm R4 desteklemektedir ve yalnızca FHıR için Azure API 'sinin R4 örneklerinde görülebilir.

### <a name="whats-the-difference-between-microsoft-fhir-server-for-azure-and-the-azure-api-for-fhir"></a>' Azure için Microsoft FHıR Server ' ve ' FHıR için Azure API ' arasındaki fark nedir?

FHıR için Azure API 'SI, Azure için açık kaynaklı Microsoft FHıR sunucusunun barındırılan ve yönetilen bir sürümüdür. Yönetilen hizmette, Microsoft tüm bakım ve güncelleştirmeleri sağlar. 

Azure için FHıR sunucusunu çalıştırdığınızda, temeldeki hizmetlere doğrudan erişim sahibi olursunuz, ancak FI verilerini depoluyorsanız sunucunun ve gerekli tüm uyumluluk işinin saklanması ve güncellenmesi sorumludur.

Geliştirme açısından, yalnızca yönetilen hizmet için uygulanan her özellik, ilk olarak Azure için açık kaynaklı Microsoft FHıR sunucusuna dağıtılır. Açık kaynakta doğrulandıktan sonra, FHıR çözümü için PaaS Azure API 'sinde kullanıma sunulacaktır. Açık kaynaklı ve PaaS 'deki yayın arasındaki süre, özelliğin karmaşıklığına ve diğer yol haritası önceliklere bağlıdır. Bu, FHıR için Azure IoT Bağlayıcısı (Önizleme) gibi hizmetlerimiz için de aynı işlemdir.

### <a name="where-can-i-see-what-is-releasing-into-the-azure-api-for-fhir"></a>FHIR için Azure API 'ye ne kadar serbest bırakıldıklarınızı nereden görebilirim?

FHıR için Azure API 'sine nelerin [yayınlanabileceği](https://github.com/microsoft/fhir-server/releases) hakkında daha fazla bilgi için lütfen açık kaynaklı Fhır sunucusunun sürümüne bakın. 2020 Kasım 'Dan itibaren, açık kaynak öğe yönetilen hizmete yayınlanacaktır, öğeleri Azure-API-for-FHIR ile etiketliyoruz. Bu özellikler genellikle açık kaynaklı sürüm sayfasında olduktan sonra iki hafta kullanılabilir. Ayrıca, derlemeyi test etme [buraya] ( https://github.com/microsoft/fhir-server/blob/master/docs/Testing-Releases.md) kendi ortamınızda test etmek istiyorsanız) ile ilgili yönergeler de sunuyoruz. Ek yönetilen hizmet güncelleştirmelerinin en iyi şekilde nasıl paylaşılacağını değerlendiriyoruz.

### <a name="in-which-regions-is-azure-api-for-fhir-available"></a>FHıR için Azure API hangi bölgelerde kullanılabilir?

Şu anda [birden fazla coğrafi bölgede](https://azure.microsoft.com/global-infrastructure/services/?products=azure-api-for-fhir&regions=non-regional,us-east,us-east-2,us-central,us-north-central,us-south-central,us-west-central,us-west,us-west-2,canada-east,canada-central,usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-texas,usgov-virginia)hem genel hem de kamu için genel kullanıma sunulduk. Microsoft 'ta kamu bulut hizmetleri hakkında daha fazla bilgi için, [Fedrampa Ile Azure hizmetleri](../azure-government/compliance/azure-services-in-fedramp-auditscope.md)'ne göz atın.

### <a name="where-can-i-see-what-is-releasing-into-the-azure-api-for-fhir"></a>FHIR için Azure API 'ye ne kadar serbest bırakıldıklarınızı nereden görebilirim?

FHıR için Azure API 'sine nelerin [yayınlanabileceği](https://github.com/microsoft/fhir-server/releases) hakkında daha fazla bilgi için lütfen açık kaynaklı Fhır sunucusunun sürümüne bakın. Yönetilen hizmete serbest bırakmaları ve genellikle açık kaynaklı sürüm sayfasında olduktan sonra iki hafta kullanılabilir olmaları durumunda, öğeleri Azure-API-for-FHıR ile etiketledik. Ayrıca, kendi ortamınızda test etmek istiyorsanız, [burada](https://github.com/microsoft/fhir-server/blob/master/docs/Testing-Releases.md) derlemeyi test etme yönergeleri de eklenmiştir. Ek yönetilen hizmet güncelleştirmelerinin en iyi şekilde nasıl paylaşılacağını değerlendiriyoruz.

### <a name="what-is-smart-on-fhir"></a>FHAR üzerinde akıllı nedir?

FHıR üzerinde akıllı (Substitutable tıbbi uygulamalar ve yeniden kullanılabilir teknoloji), iş ortağı uygulamalarını FHıR sunucularıyla ve elektronik sistem durumu kayıtları ve sistem durumu bilgileri değişimleri gibi diğer sistem durumu BT sistemleriyle tümleştirmeye yönelik bir dizi açık belirtimdir. FHıR uygulaması ile akıllı bir uygulama oluşturarak, uygulamanızın farklı sistemlerdeki bir plethora erişilebilir ve yararlanılabilir sağlayabilirsiniz.
FHIR için kimlik doğrulaması ve Azure API. AKıLLı hakkında daha fazla bilgi edinmek için bkz. [Smart Health](https://smarthealthit.org/).

## <a name="fhir-implementations-and-specifications"></a>FHıR uygulamaları ve belirtimleri

### <a name="can-i-create-a-custom-fhir-resource"></a>Özel bir FHıR kaynağı oluşturabilir miyim?

Özel FHıR kaynaklarına izin vermedik. Özel bir FHıR kaynağına ihtiyacınız varsa, uzantılara sahip [temel kaynağın](http://www.hl7.org/fhir/basic.html) üstünde özel bir kaynak oluşturabilirsiniz. 

### <a name="are-extensions-supported-on-azure-api-for-fhir"></a>[Uzantılar](https://www.hl7.org/fhir/extensibility.html) , fhır IÇIN Azure API 'de destekleniyor mu?

Geçerli bir FHıR JSON verilerini sunucuya yükleyebiliriz. Uzantıyı tanımlayan yapı tanımını depolamak istiyorsanız bunu bir yapı tanımı kaynağı olarak kaydedebilirsiniz. Şu anda, uzantılar üzerinde arama yapılamaz.

### <a name="what-is-the-limit-on-_count"></a>_Count sınırı nedir?

_Count geçerli sınırı 100 ' dir. _Count 100 ' den fazlasına ayarlarsanız, pakette yalnızca 100 kaydın gösterildiğini belirten bir uyarı alırsınız.

### <a name="are-there-any-limitations-on-the-group-export-functionality"></a>Grup dışarı aktarma işlevinde herhangi bir sınırlama var mı?

Grup dışarı aktarma için, Grup [kaynağının](https://www.hl7.org/fhir/group.html)tüm özelliklerini değil yalnızca dahil edilen başvuruları gruptan dışarı aktardık.

### <a name="can-i-post-a-bundle-to-the-azure-api-for-fhir"></a>FHıR için Azure API 'sine bir paket gönderebilir miyim?

Şu anda [Batch](https://www.hl7.org/fhir/valueset-bundle-type.html) paketlerinin nakledilmesini destekliyoruz ancak fhır IÇIN Azure API 'sinde işlem paketlerinin nakledilmesini desteklemez. İşlem paketleri göndermek için SQL tarafından desteklenen açık kaynaklı FHıR sunucusunu kullanabilirsiniz.

### <a name="how-can-i-get-all-resources-for-a-single-patient-in-the-azure-api-for-fhir"></a>FHıR için Azure API 'sindeki tek bir hasta için tüm kaynakları nasıl alabilirim?

FHıR için Azure API 'sinde [bölme aramasını](https://www.hl7.org/fhir/compartmentdefinition.html) destekliyoruz. Bu, belirli bir hasta ile ilgili tüm kaynakları almanızı sağlar. Şimdi, hasta ile ilgili tüm kaynakları (hasta değil), sonuçlarınızda hasta kaynağına ihtiyacınız varsa hasta elde etmeniz için de arama yapmanız gerektiğini unutmayın.

Bunun bazı örnekleri aşağıda verilmiştir:

* Hasta al/<id>/*
* Hasta/ <id> /gözlem al
* Hasta/ <id> /gözlem al? kod = 8302-2

### <a name="what-is-the-default-sort-when-searching-for-resources-in-azure-api-for-fhir"></a>FHıR için Azure API 'sindeki kaynakları ararken varsayılan sıralama nedir?

Son güncelleştirilme tarihine göre sıralamayı destekliyoruz: _sort = _lastUpdated. Desteklenen diğer arama parametreleri hakkında daha fazla bilgi için, [desteklenen özellikler sayfamıza](./fhir-features-supported.md#search)göz atın.

### <a name="how-does-export-work"></a>$Export nasıl çalışır?

$export FHıR belirtiminin bir parçasıdır: https://hl7.org/fhir/uv/bulkdata/export/index.html . FHıR hizmeti yönetilen bir kimlikle ve bir depolama hesabıyla yapılandırıldıysa ve yönetilen kimliğin bu depolama hesabına erişimi varsa, FHıR API 'sindeki $export çağırabilirsiniz ve tüm FHıR kaynakları depolama hesabına aktarılabilir. Daha fazla bilgi için [$Export makalemize](./export-data.md)göz atın.

## <a name="using-azure-api-for-fhir"></a>FHıR için Azure API 'YI kullanma

### <a name="how-do-i-enable-log-analytics-for-azure-api-for-fhir"></a>FHıR için Azure API Log Analytics 'i etkinleştirmek Nasıl yaparım? mı?

Tanılama günlük kaydını etkinleştirir ve bu günlüklere yönelik örnek sorguların gözden geçirilmesini sağlar. Denetim günlüklerini ve örnek sorguları etkinleştirme hakkında daha fazla bilgi için [Bu bölüme](./enable-diagnostic-logging.md)göz atın. Günlüklere ek bilgi eklemek istiyorsanız, [özel http üst bilgilerini kullanarak](./use-custom-headers.md)göz atın.

### <a name="where-can-i-see-some-examples-of-using-the-azure-api-for-fhir-within-a-workflow"></a>Bir iş akışı içinde FHıR için Azure API 'SI kullanmayla ilgili bazı örnekleri nereden görebilirim?

[Durum mimarisi GitHub sayfasında](https://github.com/microsoft/health-architectures)bulunan bir başvuru mimarileri koleksiyonudur.

### <a name="where-can-i-see-an-example-of-connecting-a-web-application-to-azure-api-for-fhir"></a>Bir Web uygulamasını FHIR için Azure API 'sine bağlama örneğini nereden görebilirim?

Örnek uygulamalar ve senaryolar içeren bir [sistem durumu mimarisi GitHub sayfasıdır](https://github.com/microsoft/health-architectures) . Bir Web uygulamasını FHIR için Azure API 'sine bağlamayı gösterir.  

## <a name="azure-api-for-fhir-features-and-services"></a>FHıR özellikleri ve hizmetleri için Azure API 

### <a name="is-there-a-way-to-encrypt-my-data-using-my-personal-key-not-a-default-key"></a>Kişisel anahtarımı varsayılan anahtar değil kullanarak şifrelemem için bir yol var mı?

Evet, FHıR için Azure API, Cosmos DB destek aracılığıyla müşteri tarafından yönetilen anahtarların yapılandırılmasını sağlar. Kişisel anahtarla verilerinizi şifreleme hakkında daha fazla bilgi için [Bu bölüme](./customer-managed-key.md)göz atın.

## <a name="azure-api-for-fhir-preview-features"></a>FHıR için Azure API: Önizleme özellikleri

### <a name="can-i-configure-scaling-capacity-for-azure-iot-connector-for-fhir-preview"></a>FHIR (Önizleme) için Azure IoT Bağlayıcısı için ölçek kapasitesini yapılandırabilir miyim?

FHıR için Azure IoT Bağlayıcısı genel önizleme sırasında ücretsiz olduğundan, ölçeklendirme kapasitesi sabittir ve sınırlı olur. FHıR yapılandırması için Azure IoT Bağlayıcısı 'nın, saniye başına 200 ileti hakkında bir verimlilik sağlaması beklenir. Bazı kaynak kapasitesi yapılandırması, genel kullanıma açık (GA) olarak kullanılabilir hale getirilir.

### <a name="why-cant-i-install-azure-iot-connector-for-fhir-preview-when-private-link-is-enabled-on-azure-api-for-fhir"></a>FHıR için Azure API üzerinde özel bağlantı etkinleştirildiğinde neden FHIR için Azure IoT bağlayıcısını (Önizleme) yükleyemiyorum?

FHıR için Azure IoT Bağlayıcısı Şu anda özel bağlantı özelliğini desteklemiyor. Bu nedenle, FHıR için Azure API 'sinde özel bağlantınız etkinse, FHıR için Azure IoT bağlayıcısını yükleyemez ve tam tersi de geçerlidir. Bu sınırlamanın, FHıR için Azure IoT Bağlayıcısı genel kullanıma (GA) uygun olduğunda dışarıda olması beklenir.