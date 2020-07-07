---
title: Özel Çevirmen nedir?
titleSuffix: Azure Cognitive Services
description: Özel çevirmen, Microsoft Translator hub 'ının Istatistiksel makine çevirisi (SMT) için yaptığı, ancak özel olarak sinir makine çevirisi (NMT) sistemleri için de benzer özellikler sunar.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 05/26/2020
ms.author: swmachan
ms.topic: overview
ms.openlocfilehash: d78767474150bc9571b25fe1f26135d6f41d1f20
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85961243"
---
# <a name="what-is-custom-translator"></a>Özel Çevirmen nedir?

[Özel çevirici](https://portal.customtranslator.azure.ai) , kuruluşların, uygulama geliştiricilerinin ve dil hizmet sağlayıcılarının özelleştirilmiş sinir makine çevirisi (NMT) sistemleri oluşturmalarına olanak tanıyan bir Translator hizmeti özelliğidir. Özelleştirilmiş çeviri sistemleri, mevcut uygulamalar, iş akışları ve Web siteleri ile sorunsuz bir şekilde tümleşir. 

[Özel çevirmenle](https://portal.customtranslator.azure.ai) oluşturulan çeviri sistemleri, her gün milyarlarca çeviri sağlayan, bulut tabanlı, [güvenli](https://cognitive.uservoice.com/knowledgebase/articles/1147537-api-and-customization-confidentiality), yüksek performanslı, yüksek düzeyde ölçeklenebilir Azure bilişsel [hizmet Çeviricisi v3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl)ile kullanılabilir.

Özel çevirmen üç düzine dili destekler ve doğrudan NMT için kullanılabilen dillerle eşlenir. Tüm liste için bkz. [Translator dilleri](https://docs.microsoft.com/azure/cognitive-services/translator/language-support#customization).

## <a name="features"></a>Özellikler

Özel çevirmen özel çeviri sistemi oluşturup daha sonra ona erişmek için farklı özellikler sağlar.

|Özellik  |Description  |
|---------|---------|
|[Sinir makine çevirisi teknolojisinden yararlanın](https://www.microsoft.com/translator/blog/2016/11/15/microsoft-translator-launching-neural-network-based-translations-for-all-its-speech-languages/)     |  Özel çevirici tarafından sunulan sinir makine çevirisi (NMT) özelliğinden yararlanarak çevirinizi geliştirebilirsiniz.       |
|[İş terminolojinizi bilen yapı sistemleri](what-are-parallel-documents.md)     |  Kendi işinizde ve sektöründe kullanılan terimlerin anlayan paralel belgeler kullanarak çeviri sistemlerini özelleştirin ve oluşturun.       |
|[Modellerinizi derlemek için sözlük kullanma](what-is-dictionary.md)     |   Eğitim verileri ayarlanmamışsa, bir modeli yalnızca sözlük verileriyle eğitebilirsiniz.       |
|[Diğer kişilerle birlikte çalışma](how-to-manage-settings.md#share-your-workspace)     |   İşinizi farklı kişilerle paylaşarak ekibinizle işbirliği yapın.     |
|[Özel çeviri modelinize erişin](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl)     |  Özel çeviri modelinize, Translator v3 aracılığıyla mevcut uygulamalarınızın/programlarınızın her zaman erişilebilir.       |

## <a name="get-better-translations"></a>Daha iyi çeviriler alın

Translator, 2016 içinde [sinir makine çevirisi (NMT)](https://www.microsoft.com/translator/blog/2016/11/15/microsoft-translator-launching-neural-network-based-translations-for-all-its-speech-languages/) yayımladı. NMT, sektör standardı [Istatistiksel makine çevirisi (SMT)](https://en.wikipedia.org/wiki/Statistical_machine_translation) teknolojisi üzerinden çeviri kalitesindeki önemli avanslar sağladı. NMT, tüm Tümcelerin kapsamını çevirmeden önce daha iyi yakaladığından, daha yüksek kaliteli, daha fazla insan-sounve daha akıcı Çeviriler sağlar. Özel [çevirmenleriniz](https://portal.customtranslator.azure.ai) için NMT, daha iyi çeviri kalitesi elde eden özel modelleriniz için.

Çeviri sistemi oluşturmak için, daha önce çevrilmiş belgeleri kullanabilirsiniz. Bu belgeler, standart bir çeviri sisteminden daha iyi etki alanına özgü terminoloji ve stil içerir. Kullanıcılar ALIGN, PDF, LCL, HTML, HTM, XLF, TMX, XLıFF, TXT, DOCX ve XLSX belgelerini karşıya yükleyebilir.

Özel çevirici Ayrıca, veri toplamayı ve hazırlanmasını daha etkili hale getirmek için belge düzeyinde paralel olan verileri de kabul eder. Kullanıcılar, birden çok dilde aynı içeriğin sürümlerine, ancak ayrı belgelere erişebiliyorsa, özel çevirmen, otomatik olarak belge genelindeki cümleleri eşleştirebilir.

Uygun tür ve eğitim verisi miktarı belirtilmişse, özel çevirici kullanarak 5 ila 10 punto arasındaki [Bleu puanı](what-is-bleu-score.md) kazançlarını görmek yaygın olmayan bir durumdur.

## <a name="be-productive-and-cost-effective"></a>Üretken ve uygun maliyetli olun

Özel [çevirmenle](https://portal.customtranslator.azure.ai), özel bir sistemin eğitimi ve dağıtımı, programlama becerileri gerektirmez.

Güvenli [özel çevirmen](https://portal.customtranslator.azure.ai) portalını kullanarak kullanıcılar eğitim verilerini karşıya yükleyebilir, sistemleri eğitebilir ve bunları bir üretim ortamına, sezgisel bir kullanıcı arabirimi aracılığıyla dağıtabilir. Daha sonra sistem birkaç saat içinde uygun ölçekte kullanılabilir olacaktır (gerçek zamanlı eğitim veri boyutuna bağlıdır).

[Özel çevirmende](https://portal.customtranslator.azure.ai) programlı bir şekilde [adanmış bir API](https://custom-api.cognitive.microsofttranslator.com/swagger/) aracılığıyla erişilebilir (Şu anda önizleme aşamasındadır). API, kullanıcıların kendi uygulamaları veya Web hizmeti aracılığıyla düzenli olarak eğitim oluşturmayı veya güncelleştirmeyi yönetmesini sağlar.

İçeriği çevirmek için özel bir model kullanmanın maliyeti, kullanıcının Translator fiyatlandırma katmanını temel alır. Fiyatlandırma Katmanı ayrıntıları için bilişsel Hizmetler [çevirmeni fiyatlandırma web sayfasına](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/) bakın.

## <a name="securely-translate-anytime-anywhere-on-all-your-apps-and-services"></a>Tüm uygulama ve hizmetlerinize dilediğiniz zaman güvenli bir şekilde çevirin

Özel sistemler sorunsuz bir şekilde erişilebilir ve herhangi bir ürün ya da iş akışına ve herhangi bir cihaza, standart REST teknolojisi aracılığıyla Translator aracılığıyla tümleştirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/) hakkında bilgi edinin.

- [Hızlı başlangıç](quickstart-build-deploy-custom-model.md) Ile özel çevirmende bir çeviri modeli oluşturmayı öğrenin.
