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
ms.openlocfilehash: cdec5ade4fc72fba6fcfba131b69ca9eb373874c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84871781"
---
# <a name="frequently-asked-questions-about-the-azure-api-for-fhir"></a>FHıR için Azure API 'SI hakkında sık sorulan sorular

## <a name="what-is-fhir"></a>FHAR nedir?
Hızlı sağlık birlikte çalışabilirlik kaynakları (FHıR-, "yangın"), farklı sistem durumu sistemleri arasında sağlık verileri değişimini etkinleştirmeye yönelik bir birlikte çalışabilirlik standardıdır. Bu standart, HL7 organizasyonu tarafından geliştirilmiştir ve dünyanın dört bir yanındaki sağlık kurumları tarafından benimsenmekte. FHıR 'nin en güncel sürümü, R4 (sürüm 4). FHıR için Azure API 'SI R4 destekler ve ayrıca önceki sürüm STU3 ' i (deneme sürümü 3 için standart) destekler. FHAR hakkında daha fazla bilgi için [HL7.org](http://hl7.org/fhir/summary.html)adresini ziyaret edin.

## <a name="is-the-data-behind-the-fhir-apis-stored-in-azure"></a>Azure 'da depolanan FHAR API 'Lerinin arkasındaki veriler midir?

Evet, veriler Azure 'da yönetilen veritabanlarında depolanır. FHıR için Azure API 'SI, temel alınan veri deposuna doğrudan erişim sağlamaz.

## <a name="what-identity-provider-do-you-support"></a>Hangi kimlik sağlayıcısını destekliyoruz?

Şu anda kimlik sağlayıcısı olarak Microsoft Azure Active Directory destekliyoruz.

## <a name="what-fhir-version-do-you-support"></a>Hangi FHıR sürümünü destekliyoruz?

4.0.0 ve 3.0.1 sürümlerini hem FHıR için Azure API 'SI (PaaS) hem de Azure için FHıR sunucusu (açık kaynak) üzerinde destekliyoruz.

Ayrıntılar için bkz. [desteklenen özellikler](fhir-features-supported.md). [HL7 FHIR sürüm geçmişindeki](https://hl7.org/fhir/R4/history.html)sürümler arasında nelerin değiştiğini okuyun.

## <a name="whats-the-difference-between-the-open-source-microsoft-fhir-server-for-azure-and-the-azure-api-for-fhir"></a>Azure için açık kaynaklı Microsoft FHıR sunucusu ve FHıR için Azure API arasındaki fark nedir?

FHıR için Azure API 'SI, Azure için açık kaynaklı Microsoft FHıR sunucusunun barındırılan ve yönetilen bir sürümüdür. Yönetilen hizmette, Microsoft tüm bakım ve güncelleştirmeleri sağlar. 

Azure için FHıR sunucusu çalıştırırken, temeldeki hizmetlere doğrudan erişim sahibi olursunuz. Ancak, FI verilerini depoluyorsanız, sunucunun ve gerekli tüm uyumluluk işinin saklanması ve güncelleştirilmesinden de sorumlu olursunuz.

Geliştirme açısından, her özellik ilk olarak Azure için açık kaynaklı Microsoft FHıR sunucusuna dağıtılır. Açık kaynakta doğrulandıktan sonra, FHıR çözümü için PaaS Azure API 'sinde kullanıma sunulacaktır. Açık kaynaklı ve PaaS 'deki yayın arasındaki süre, özelliğin karmaşıklığına ve diğer yol haritası önceliklere bağlıdır. 

## <a name="what-is-smart-on-fhir"></a>FHAR üzerinde akıllı nedir?

FHıR üzerinde akıllı (Substitutable tıbbi uygulamalar ve yeniden kullanılabilir teknoloji), iş ortağı uygulamalarını FHıR sunucularıyla ve elektronik sistem durumu kayıtları ve sistem durumu bilgileri değişimleri gibi diğer sistem durumu BT sistemleriyle tümleştirmeye yönelik bir dizi açık belirtimdir. FHıR uygulaması ile akıllı bir uygulama oluşturarak, uygulamanızın farklı sistemlerdeki bir plethora erişilebilir ve yararlanılabilir sağlayabilirsiniz.
FHIR için kimlik doğrulaması ve Azure API. AKıLLı hakkında daha fazla bilgi edinmek için bkz. [Smart Health](https://smarthealthit.org/).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, FHıR için Azure API 'SI hakkında sık sorulan sorulardan bazılarını okuyun. Azure için FHıR sunucusundaki desteklenen özellikler hakkında bilgi edinin:
 
>[!div class="nextstepaction"]
>[Desteklenen FHıR özellikleri](fhir-features-supported.md)