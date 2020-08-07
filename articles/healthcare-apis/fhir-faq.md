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
ms.openlocfilehash: 252abcac6e9e39930593c1b110bf6d55ffdfc33f
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87843530"
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

## <a name="azure-iot-connector-for-fhir-preview"></a>FHıR için Azure IoT Bağlayıcısı (Önizleme)

### <a name="what-is-iomt"></a>IoMT nedir?
IoMT, tıbbi nesnelerin Interneti olduğunu ve ağ üzerinden BT sistemleri ile sistem durumu ve iş verileri yakalayan ve alıp veren bir IoT cihazları kategorisidir. Bazı ıomt cihazlarına örnek olarak, uygunluk ve klinik wearables, izleme algılayıcıları, etkinlik izleyicileri, bakım noktaları ve hatta akıllı bir Pill sayılabilir.

### <a name="how-many-azure-iot-connector-for-fhir-preview-do-i-need"></a>FHıR (Önizleme) için kaç Azure IoT bağlayıcısına ihtiyacım var?
FHIR * için tek bir Azure IoT Bağlayıcısı, çok sayıda farklı cihaz türünden veri almak için kullanılabilir. Yine de aşağıdaki nedenlerle farklı bağlayıcılar kullanmaya karar verebilirsiniz:
- **Ölçek**: genel önizleme için, fhır kaynak kapasitesi Için Azure IoT Bağlayıcısı sabittir ve saniyede 200 ileti hakkında bir verimlilik sağlanması beklenmektedir. Daha yüksek aktarım hızı gerekiyorsa FHıR için daha fazla Azure IoT Bağlayıcısı ekleyebilirsiniz.
- **Cihaz türü**: cihaz yönetimi nedenleriniz için sahip olduğunuz her ıomt cihazı türü için fhır için ayrı bir Azure IoT Bağlayıcısı ayarlayabilirsiniz.

### <a name="is-there-a-limit-on-number-of-azure-iot-connector-for-fhir-preview-during-public-preview"></a>Genel Önizleme sırasında FHIR (Önizleme) için Azure IoT Bağlayıcısı sayısı sınırı var mı?
Evet, özellik genel önizlemedeyken abonelik başına FHıR için yalnızca iki Azure IoT Bağlayıcısı oluşturabilirsiniz. Bu sınır, Önizleme süresince Özellik ücretsiz olarak kullanılabilir olduğu için beklenmeyen giderleri engellemek için mevcuttur. İstek üzerine bu sınır, FHıR için en fazla beş Azure IoT Bağlayıcısı üzerinden yükseltilebilir.

### <a name="what-azure-regions-azure-iot-connector-for-fhir-preview-feature-is-available-during-public-preview"></a>FHıR için Azure IoT Bağlayıcısı (Önizleme) özelliği, genel önizleme sırasında kullanılabilir mi?
FHıR için Azure IoT Bağlayıcısı, FHıR için Azure API 'sinin kullanılabildiği tüm Azure bölgelerinde kullanılabilir.

### <a name="can-i-configure-scaling-capacity-for-azure-iot-connector-for-fhir-preview"></a>FHIR (Önizleme) için Azure IoT Bağlayıcısı için ölçek kapasitesini yapılandırabilir miyim?
FHıR için Azure IoT Bağlayıcısı genel önizleme sırasında ücretsiz olduğundan, ölçeklendirme kapasitesi sabittir ve sınırlı olur. FHıR yapılandırması için Azure IoT Bağlayıcısı 'nın, saniye başına 200 ileti hakkında bir verimlilik sağlaması beklenir. Bazı kaynak kapasitesi yapılandırması, genel kullanıma açık (GA) olarak kullanılabilir hale getirilir.

### <a name="what-fhir-version-does-azure-iot-connector-for-fhir-preview-support"></a>FHıR (Önizleme) için Azure IoT Bağlayıcısı 'nın hangi FHıR sürümü desteği vardır?
FHıR için Azure IoT Bağlayıcısı Şu anda yalnızca FHıR sürüm R4 desteklemektedir. Bu nedenle, bu özellik yalnızca FHıR için Azure API 'sinin R4 örneklerinde görünür ve Microsoft şu anda sürüm STU3 desteklemeyi planlıyor.

### <a name="why-cant-i-install-azure-iot-connector-for-fhir-preview-when-private-link-is-enabled-on-azure-api-for-fhir"></a>FHıR için Azure API üzerinde özel bağlantı etkinleştirildiğinde neden FHIR için Azure IoT bağlayıcısını (Önizleme) yükleyemiyorum?
FHıR için Azure IoT Bağlayıcısı Şu anda özel bağlantı özelliğini desteklemiyor. Bu nedenle, FHıR için Azure API 'sinde özel bağlantınız etkinse, FHıR için Azure IoT bağlayıcısını yükleyemez ve tam tersi de geçerlidir. Bu sınırlamanın, FHıR için Azure IoT Bağlayıcısı genel kullanıma (GA) uygun olduğunda dışarıda olması beklenir.

### <a name="whats-the-difference-between-the-open-source-iomt-fhir-connector-for-azure-and-azure-iot-connector-for-fhir-preview-feature-of-azure-api-for-fhir-service"></a>Azure için açık kaynaklı ıomt FHıR Bağlayıcısı ve FHıR hizmeti için Azure API 'sinin FHIR (Önizleme) özelliği için Azure IoT Bağlayıcısı arasındaki fark nedir?
FHıR için Azure IoT Bağlayıcısı, Azure için açık kaynaklı ıomt FHıR bağlayıcısının barındırılan ve yönetilen bir sürümüdür. Yönetilen hizmette, Microsoft tüm bakım ve güncelleştirmeleri sağlar.

Azure için IoMT FHıR bağlayıcısını çalıştırırken, temel alınan kaynaklara doğrudan erişiminiz vardır. Ancak, FI verilerini depoluyorsanız, sunucunun ve gerekli tüm uyumluluk işinin saklanması ve güncelleştirilmesinden de sorumlu olursunuz.

Geliştirme açısından, her özellik Azure için açık kaynaklı ıomt FHıR bağlayıcısına dağıtılır. Açık kaynakta doğrulandıktan sonra, FHıR hizmeti için Azure API 'sinin FHıR özelliği için PaaS Azure IoT Bağlayıcısı ' nda kullanıma sunulacaktır. Açık kaynaklı ve PaaS 'deki yayın arasındaki süre, özelliğin karmaşıklığına ve diğer yol eşleme önceliklere bağlıdır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, FHıR için Azure API 'SI hakkında sık sorulan sorulardan bazılarını okuyun. Azure için FHıR sunucusundaki desteklenen özellikler hakkında bilgi edinin:
 
>[!div class="nextstepaction"]
>[Desteklenen FHıR özellikleri](fhir-features-supported.md)

* Azure portal, FHıR için Azure IoT Bağlayıcısı, IoT Bağlayıcısı (Önizleme) olarak adlandırılır.

FHIR, HL7’nin kayıtlı ticari markasıdır ve HL7’nin izniyle kullanılır.