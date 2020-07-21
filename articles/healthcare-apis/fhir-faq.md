---
title: Azure 'da FHıR hizmetleri hakkında SSS-FHıR için Azure API
description: Fhir API 'Leri ve sürüm desteğinin arkasındaki verilerin depolama konumu gibi FHıR için Azure API 'SI hakkında sık sorulan soruların yanıtlarını alın.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: af891935fd474e6f1f83ff1c2ce56ef71cd065c6
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86536734"
---
# <a name="frequently-asked-questions-about-the-azure-api-for-fhir"></a>FHıR için Azure API 'SI hakkında sık sorulan sorular

## <a name="azure-api-for-fhir"></a>FHIR için Azure API'si

### <a name="what-is-fhir"></a>FHAR nedir?
Hızlı sağlık birlikte çalışabilirlik kaynakları (FHıR-, "yangın"), farklı sistem durumu sistemleri arasında sağlık verileri değişimini etkinleştirmeye yönelik bir birlikte çalışabilirlik standardıdır. Bu standart, HL7 organizasyonu tarafından geliştirilmiştir ve dünyanın dört bir yanındaki sağlık kurumları tarafından benimsenmekte. FHıR 'nin en güncel sürümü, R4 (sürüm 4). FHıR için Azure API 'SI R4 destekler ve ayrıca önceki sürüm STU3 ' i (deneme sürümü 3 için standart) destekler. FHAR hakkında daha fazla bilgi için [HL7.org](http://hl7.org/fhir/summary.html)adresini ziyaret edin.

### <a name="is-the-data-behind-the-fhir-apis-stored-in-azure"></a>Azure 'da depolanan FHAR API 'Lerinin arkasındaki veriler midir?

Evet, veriler Azure 'da yönetilen veritabanlarında depolanır. FHıR için Azure API 'SI, temel alınan veri deposuna doğrudan erişim sağlamaz.

### <a name="what-identity-provider-do-you-support"></a>Hangi kimlik sağlayıcısını destekliyoruz?

Şu anda kimlik sağlayıcısı olarak Microsoft Azure Active Directory destekliyoruz.

### <a name="what-fhir-version-do-you-support"></a>Hangi FHıR sürümünü destekliyoruz?

4.0.0 ve 3.0.1 sürümlerini hem FHıR için Azure API 'SI (PaaS) hem de Azure için FHıR sunucusu (açık kaynak) üzerinde destekliyoruz.

Ayrıntılar için bkz. [desteklenen özellikler](fhir-features-supported.md). [HL7 FHIR sürüm geçmişindeki](https://hl7.org/fhir/R4/history.html)sürümler arasında nelerin değiştiğini okuyun.

### <a name="whats-the-difference-between-the-open-source-microsoft-fhir-server-for-azure-and-the-azure-api-for-fhir"></a>Azure için açık kaynaklı Microsoft FHıR sunucusu ve FHıR için Azure API arasındaki fark nedir?

FHıR için Azure API 'SI, Azure için açık kaynaklı Microsoft FHıR sunucusunun barındırılan ve yönetilen bir sürümüdür. Yönetilen hizmette, Microsoft tüm bakım ve güncelleştirmeleri sağlar. 

Azure için FHıR sunucusu çalıştırırken, temeldeki hizmetlere doğrudan erişim sahibi olursunuz. Ancak, FI verilerini depoluyorsanız, sunucunun ve gerekli tüm uyumluluk işinin saklanması ve güncelleştirilmesinden de sorumlu olursunuz.

Geliştirme açısından, her özellik ilk olarak Azure için açık kaynaklı Microsoft FHıR sunucusuna dağıtılır. Açık kaynakta doğrulandıktan sonra, FHıR çözümü için PaaS Azure API 'sinde kullanıma sunulacaktır. Açık kaynaklı ve PaaS 'deki yayın arasındaki süre, özelliğin karmaşıklığına ve diğer yol haritası önceliklere bağlıdır. 

### <a name="what-is-smart-on-fhir"></a>FHAR üzerinde akıllı nedir?

FHıR üzerinde akıllı (Substitutable tıbbi uygulamalar ve yeniden kullanılabilir teknoloji), iş ortağı uygulamalarını FHıR sunucularıyla ve elektronik sistem durumu kayıtları ve sistem durumu bilgileri değişimleri gibi diğer sistem durumu BT sistemleriyle tümleştirmeye yönelik bir dizi açık belirtimdir. FHıR uygulaması ile akıllı bir uygulama oluşturarak, uygulamanızın farklı sistemlerdeki bir plethora erişilebilir ve yararlanılabilir sağlayabilirsiniz.
FHIR için kimlik doğrulaması ve Azure API. AKıLLı hakkında daha fazla bilgi edinmek için bkz. [Smart Health](https://smarthealthit.org/).


## <a name="iot-connector-preview"></a>IoT Bağlayıcısı (Önizleme)

### <a name="what-is-iomt"></a>IoMT nedir?
IoMT, tıbbi nesnelerin Interneti olduğunu ve ağ üzerinden BT sistemleri ile sistem durumu ve iş verileri yakalayan ve alıp veren bir IoT cihazları kategorisidir. Bazı ıomt cihazlarına örnek olarak, uygunluk ve klinik wearables, izleme algılayıcıları, etkinlik izleyicileri, bakım noktaları ve hatta akıllı bir Pill sayılabilir.

### <a name="how-many-iot-connectors-do-i-need"></a>Kaç IoT bağlayıcısıyla ihtiyacım var?
Tek bir IoT Bağlayıcısı, çok sayıda farklı cihaz türünden veri almak için kullanılabilir. Yine de aşağıdaki nedenlerle farklı bağlayıcılar kullanmaya karar verebilirsiniz:
- **Ölçek**: genel önizleme Için, IoT Bağlayıcısı kaynak kapasitesi sabittir ve saniyede 200 ileti aktarımı sağlaması beklenir. Daha yüksek aktarım hızı gerekliyse daha fazla IoT Bağlayıcısı ekleyebilirsiniz.
- **Cihaz türü**: cihaz yönetimi nedenleriniz için sahip olduğunuz her ıomt cihazı türü için ayrı bir IoT Bağlayıcısı ayarlayabilirsiniz.

### <a name="is-there-a-limit-on-number-of-iot-connectors-during-public-preview"></a>Genel Önizleme sırasında IoT bağlayıcılarının sayısıyla ilgili bir sınır var mı?
Evet, özellik genel önizlemede olduğunda, abonelik başına yalnızca iki IoT Bağlayıcısı oluşturabilirsiniz. Bu sınır, Önizleme süresince Özellik ücretsiz olarak kullanılabilir olduğu için beklenmeyen giderleri engellemek için mevcuttur. İstek üzerine bu sınır en fazla beş IoT bağlayıcısından yükseltilebilir.

### <a name="what-azure-regions-iot-connector-feature-is-available-during-public-preview"></a>Genel Önizleme sırasında hangi Azure bölgeleri IoT Bağlayıcısı özelliği kullanılabilir?
IoT Bağlayıcısı, FHıR için Azure API 'sinin kullanılabildiği tüm Azure bölgelerinde kullanılabilir.

### <a name="can-i-configure-scaling-capacity-for-iot-connector"></a>IoT Bağlayıcısı için ölçek kapasitesini yapılandırabilir miyim?
IoT Bağlayıcısı genel önizleme sırasında ücretsiz olduğundan, ölçeklendirme kapasitesi sabittir ve sınırlı olur. IoT Bağlayıcısı yapılandırmasının, saniyede 200 ileti aktarımı sağlaması beklenmektedir. Bazı kaynak kapasitesi yapılandırması, genel kullanıma açık (GA) olarak kullanılabilir hale getirilir.

### <a name="what-fhir-version-does-iot-connector-support"></a>IoT Bağlayıcısı hangi FHıR sürümünde desteklenir?
IoT Bağlayıcısı Şu anda yalnızca FHıR sürüm R4 desteklemektedir. Bu nedenle, bu özellik yalnızca FHıR için Azure API 'sinin R4 örneklerinde görünür ve Microsoft şu anda sürüm STU3 desteklemeyi planlıyor.

### <a name="why-cant-i-install-iot-connector-when-private-link-is-enabled-on-azure-api-for-fhir"></a>FHıR için Azure API 'sinde özel bağlantı etkinleştirildiğinde neden IoT bağlayıcısını yükleyemiyorum?
IoT Bağlayıcısı Şu anda özel bağlantı özelliğini desteklemiyor. Bu nedenle, FHıR için Azure API 'sinde özel bağlantınız etkinse IoT bağlayıcısını yükleyemez ve tam tersi de geçerlidir. Bu sınırlamanın, IoT Bağlayıcısı genel kullanıma (GA) uygun olduğunda dışarıda olması beklenir.

### <a name="whats-the-difference-between-the-open-source-iomt-fhir-connector-for-azure-and-iot-connector-feature-of-azure-api-for-fhir-service"></a>FHıR hizmeti için Azure API 'nin Azure API 'si ve IoT Bağlayıcısı özelliği için açık kaynaklı ıomt FHıR Bağlayıcısı arasındaki fark nedir?
IoT Bağlayıcısı, Azure için açık kaynaklı ıomt FHıR bağlayıcısının barındırılan ve yönetilen bir sürümüdür. Yönetilen hizmette, Microsoft tüm bakım ve güncelleştirmeleri sağlar.

Azure için IoMT FHıR bağlayıcısını çalıştırırken, temel alınan kaynaklara doğrudan erişiminiz vardır. Ancak, FI verilerini depoluyorsanız, sunucunun ve gerekli tüm uyumluluk işinin saklanması ve güncelleştirilmesinden de sorumlu olursunuz.

Geliştirme açısından, her özellik Azure için açık kaynaklı ıomt FHıR bağlayıcısına dağıtılır. Açık kaynakta doğrulandıktan sonra, FHıR hizmeti için Azure API 'sinin PaaS IoT Bağlayıcısı özelliğinde serbest bırakılır. Açık kaynaklı ve PaaS 'deki yayın arasındaki süre, özelliğin karmaşıklığına ve diğer yol eşleme önceliklere bağlıdır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, FHıR için Azure API 'SI hakkında sık sorulan sorulardan bazılarını okuyun. Azure için FHıR sunucusundaki desteklenen özellikler hakkında bilgi edinin:
 
>[!div class="nextstepaction"]
>[Desteklenen FHıR özellikleri](fhir-features-supported.md)