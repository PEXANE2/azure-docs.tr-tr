---
title: Özel Çevirmen nedir?
titleSuffix: Azure Cognitive Services
description: Custom Translator, Microsoft Translator Hub'ın İstatistiksel Makine Çevirisi (SMT) için, özellikle Nöral Makine Çevirisi (NMT) sistemleri için yaptığı na benzer özellikler sunar.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 12/09/2019
ms.author: swmachan
ms.topic: overview
ms.openlocfilehash: d84a0190e13d7bb7664e8792e0047338fe4bf5e2
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75982684"
---
# <a name="what-is-custom-translator"></a>Özel Çevirmen nedir?

[Custom Translator,](https://portal.customtranslator.azure.ai) Çevirmen kuruluşlarının, uygulama geliştiricilerinin ve dil hizmeti sağlayıcılarının özelleştirilmiş sinir seli çevirisi (NMT) sistemleri oluşturmasını sağlayan Microsoft Translator hizmetinin bir özelliğidir. Özelleştirilmiş çeviri sistemleri, varolan uygulamalara, iş akışlarına ve web sitelerine sorunsuz bir şekilde entegre olur. [Custom Translator,](https://portal.customtranslator.azure.ai/) [Microsoft Translator Hub'ın](https://hub.microsofttranslator.com/) İstatistiksel Makine Çevirisi (SMT) için, özellikle Nöral Makine Çevirisi (NMT) sistemleri için yaptığı na benzer özellikler sunar.

[Custom Translator](https://portal.customtranslator.azure.ai) ile inşa edilen çeviri sistemleri, her gün milyarlarca çeviriye güç veren aynı bulut tabanlı, [güvenli,](https://cognitive.uservoice.com/knowledgebase/articles/1147537-api-and-customization-confidentiality)yüksek performanslı, yüksek ölçeklenebilir Microsoft Translator [Text API V3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl)üzerinden kullanılabilir.

Özel Çevirmen üç düzineden fazla dili destekler ve haritaları doğrudan NMT için kullanılabilir dillere eşler. Tam liste için [Microsoft Çevirmen Dilleri'ne](https://docs.microsoft.com/azure/cognitive-services/translator/language-support#customization)bakın.

## <a name="features"></a>Özellikler

Custom Translator, özel çeviri sistemi oluşturmak ve daha sonra bu sisteme erişmek için farklı özellikler sağlar.

|Özellik  |Açıklama  |
|---------|---------|
|[Nöral makine çeviri teknolojisi](https://www.microsoft.com/translator/blog/2016/11/15/microsoft-translator-launching-neural-network-based-translations-for-all-its-speech-languages/)     |  Custom çevirmen tarafından sağlanan nöral makine çevirisi (NMT) yararlanarak çevirinizi geliştirin.       |
|[İşletmenizi sonlandırmayı bilen sistemler oluşturun](what-are-parallel-documents.md)     |  Kendi işinizde ve endüstrinizde kullanılan terminolojileri anlayan paralel belgeler kullanarak çeviri sistemlerini özelleştirin ve oluşturun.       |
|[Modellerinizi oluşturmak için sözlük kullanın](what-is-dictionary.md)     |   Eğitim veri setiniz yoksa, yalnızca sözlük verileri içeren bir model eğitebilirsiniz.       |
|[Başkalarıyla işbirliği yapma](how-to-manage-settings.md#share-your-workspace)     |   Çalışmanızı farklı kişilerle paylaşarak ekibinizle işbirliği yapın.     |
|[Özel çeviri modelinize erişin](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl)     |  Özel çeviri modelinize, microsoft Translator Text API V3 üzerinden mevcut uygulamalarınız/ programlarınız tarafından her zaman erişilebilir.       |

## <a name="get-better-translations"></a>Daha iyi çeviriler alın

Microsoft Translator, [Nöral Makine Çevirisi'ni (NMT)](https://www.microsoft.com/translator/blog/2016/11/15/microsoft-translator-launching-neural-network-based-translations-for-all-its-speech-languages/) 2016 yılında yayımladı. NMT, endüstri standardı [İstatistiksel Makine Çevirisi (SMT)](https://en.wikipedia.org/wiki/Statistical_machine_translation) teknolojisi üzerinde çeviri kalitesinde büyük ilerlemeler sağlamıştır. NMT, tam cümlelerin bağlamını çevirmeden önce daha iyi yakaladığından, daha yüksek kalite, daha fazla insan sesi ve daha akıcı çeviriler sağlar. [Custom Translator,](https://portal.customtranslator.azure.ai) özel modelleriniz için NMT sağlayarak daha iyi çeviri kalitesi sağlar.

Çeviri sistemi oluşturmak için daha önce çevrilmiş belgeleri kullanabilirsiniz. Bu belgeler, standart bir çeviri sisteminden daha iyi, etki alanına özgü terminoloji ve stil içerir. Kullanıcılar ALIGN, PDF, LCL, HTML, HTM, XLF, TMX, XLIFF, TXT, DOCX ve XLSX belgelerini yükleyebilir.

Custom Translator ayrıca veri toplama ve hazırlamayı daha etkili hale getirmek için belge düzeyinde paralel olan verileri de kabul eder. Kullanıcılar aynı içeriğin sürümlerine birden çok dilde ancak ayrı belgelerde erişebiliyorsa, Özel Çevirmen cümleleri belgeler arasında otomatik olarak eşleştirebilir.

Uygun eğitim veri türü ve miktarı sağlanıyorsa, Özel Çevirmen kullanarak [BLEU puanının](what-is-bleu-score.md) 5 ile 10 puan arasında kazandığını görmek sık rastlanan bir durumdur.

## <a name="be-productive-and-cost-effective"></a>Üretken ve uygun maliyetli olun

[Özel Çevirmen](https://portal.customtranslator.azure.ai)ile, özel bir sistemi eğitmek ve dağıtmak herhangi bir programlama becerisi gerektirmez.

Güvenli [Özel Çevirmen](https://portal.customtranslator.azure.ai) portalını kullanan kullanıcılar, sezgisel bir kullanıcı arabirimi aracılığıyla eğitim verilerini yükleyebilir, sistemleri eğitebilir, test sistemlerini test edebilir ve bunları üretim ortamına dağıtabilir. Sistem daha sonra birkaç saat içinde ölçekte kullanıma sunulacaktır (gerçek süre eğitim veri boyutuna bağlıdır).

[Özel Çevirmene](https://portal.customtranslator.azure.ai) özel bir [API](https://custom-api.cognitive.microsofttranslator.com/swagger/) (şu anda önizlemede) aracılığıyla programlanabilir. API, kullanıcıların kendi uygulama veya web hizmeti aracılığıyla düzenli olarak eğitim oluşturmayı veya güncellemeyi yönetmelerine olanak tanır.

İçeriği çevirmek için özel bir model kullanmanın maliyeti, kullanıcının Çevirmen Metin API fiyatlandırma katmanına dayanır. Katman ayrıntılarını fiyatlandırmak için Bilişsel Hizmetler [Çevirmeni Metin API fiyatlandırma web sayfasına](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/) bakın.

## <a name="securely-translate-anytime-anywhere-on-all-your-apps-and-services"></a>Tüm uygulamalarınızda ve hizmetlerinizde her zaman, her yerde güvenli bir şekilde çeviri yapın

Özel sistemlere, standart REST teknolojisi aracılığıyla Microsoft Translator Text API aracılığıyla herhangi bir ürün veya iş akışına ve herhangi bir cihaza sorunsuz bir şekilde erişilebilir ve entegre edilebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/) hakkında bilgi edinin.

- [Quickstart](quickstart-build-deploy-custom-model.md) ile Custom Translator'da bir çeviri modeli oluşturmayı öğrenin.
