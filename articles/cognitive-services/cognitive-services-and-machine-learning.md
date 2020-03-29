---
title: Bilişsel Hizmetler ve Makine Öğrenimi
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler’in, makine öğrenmesine yönelik diğer Azure teklifleriyle birlikte nasıl çalıştığını öğrenin.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: diberry
ms.openlocfilehash: cde505e4c95de9b9693a0e9d260d7fa84f3e905b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75531488"
---
# <a name="cognitive-services-and-machine-learning"></a>Bilişsel Hizmetler ve makine öğrenmesi

Bilişsel Hizmetler, metni duygusal duygular için analiz etmek veya nesneleri veya yüzleri tanımak için görüntüleri analiz etmek gibi genel sorunları çözmek için makine öğrenimi yetenekleri sağlar. Bu hizmetleri kullanmak için özel makine öğrenimi veya veri bilimi bilgisine ihtiyacınız yoktur. 

[Bilişsel Hizmetler,](welcome.md) her biri farklı, genelleştirilmiş tahmin yeteneklerini destekleyen bir hizmet grubudur. Hizmetler, doğru hizmeti bulmanıza yardımcı olmak için farklı kategorilere ayrılmıştır. 

|Hizmet kategorisi|Amaç|
|--|--|
|[Karar](https://azure.microsoft.com/services/cognitive-services/directory/decision/)|Bilgiye dayalı ve verimli kararlar almak için öneriler sunan uygulamalar derleyin.|
|[Dil](https://azure.microsoft.com/services/cognitive-services/directory/lang/)|Önceden oluşturulmuş betiklerle uygulamalarınızın doğal dil işlemesine, duyguları değerlendirmesine ve kullanıcıların ne istediğini tanımayı öğrenmesine imkan sağlayın.|
|[Arama](https://azure.microsoft.com/services/cognitive-services/directory/search/)|Uygulamalarınıza Bing Arama API'leri ekleyin ve tek bir API çağrısıyla milyarlarca web sayfası, resim, video ve haberi tarama imkanından yararlanın.|
|[Konuşma](https://azure.microsoft.com/services/cognitive-services/directory/speech/)|Konuşmaları metne, metinleri kulağa doğal gelen konuşmalara dönüştürün. Bir dilden başka bir dile çeviri yapın, konuşmacı doğrulama ve tanıma özelliğini etkinleştirin.|
|[Görsel](https://azure.microsoft.com/services/cognitive-services/directory/vision/)|Resimlerinizi, videolarınızı ve dijital mürekkep içeriğinizi tanıyın, tanımlayın, altyazı ekleyin, dizine alın ve yönetin.|
||||

Şunları yaptığınızda Bilişsel Hizmetleri kullanın:

* Genelleştirilmiş bir çözüm kullanabilir.
* Bir programlama REST API veya SDK'dan erişim çözümüne. 

Şunları yaptığınızda başka bir makine öğrenme çözümü kullanın:

* Algoritmayı seçmeniz ve çok özel veriler üzerinde eğitim vermem gerekir.

## <a name="what-is-machine-learning"></a>Machine learning nedir?

Makine öğrenimi, belirli bir ihtiyacı çözmek için verileri ve algoritmayı bir araya getirdiğiniz bir kavramdır. Veri ve algoritma eğitildikten sonra, çıktı farklı verilerle yeniden kullanabileceğiniz bir modeldir. Eğitilen model, yeni verilere dayalı öngörüler sağlar. 

Bir makine öğrenme sistemi oluşturma süreci makine öğrenimi veya veri bilimi hakkında bilgi gerektirir.

Makine öğrenimi [Azure Machine Learning (AML) ürün ve hizmetleri](https://docs.microsoft.com/azure/architecture/data-guide/technology-choices/data-science-and-machine-learning?context=azure/machine-learning/studio/context/ml-context)kullanılarak sağlanır.

## <a name="what-is-a-cognitive-service"></a>Bilişsel Hizmet Nedir?

Bilişsel Hizmet, bir makine öğrenme çözümündeki bileşenlerin bir kısmını veya tümlerini sağlar: veri, algoritma ve eğitilmiş model. Bu hizmetler, makine öğrenimi veya veri bilimi ile ilgili deneyime ihtiyaç duymadan verileriniz hakkında genel bilgi gerektirmektedir. Bu hizmetler hem REST API(lar) hem de dil tabanlı SDK'lar sağlar. Sonuç olarak, hizmetleri kullanmak için programlama dili bilgisine sahip olmanız gerekir.

## <a name="how-are-cognitive-services-and-azure-machine-learning-aml-similar"></a>Bilişsel Hizmetler ve Azure Machine Learning (AML) nasıl benzer?

Her ikisi de iş operasyonlarını geliştirmek için yapay zeka (AI) uygulama nihai hedefi var, her ilgili teklifleri bu sağlar nasıl farklı olsa. 

Genel olarak, izleyiciler farklıdır:

* Bilişsel Hizmetler, makine öğrenme deneyimi olmayan geliştiriciler içindir.
* Azure Machine Learning veri bilimciler için özel olarak tasarlanmıştır. 

## <a name="how-is-a-cognitive-service-different-from-machine-learning"></a>Bilişsel Hizmetin makine öğreniminden farkı nedir?

Bilişsel Hizmet sizin için eğitimli bir model sağlar. Bu, bir REST API(lar) veya SDK'dan edinilebilen verileri ve algoritmayı bir araya getirir. Senaryonuza bağlı olarak bu hizmeti birkaç dakika içinde uygulayabilirsiniz.  Bilişsel Hizmet, metindeki anahtar tümcecikler veya resimlerdeki öğe tanımlama gibi genel sorunlara yanıt sağlar. 

Makine öğrenimi genellikle başarılı bir şekilde uygulanması için daha uzun bir süre gerektiren bir süreçtir. Bu süre, Bilişsel Hizmet tarafından sağlanan aynı işlevsellik düzeyine ulaşmak için veri toplama, temizleme, dönüştürme, algoritma seçimi, model eğitimi ve dağıtım için harcanır. Makine öğrenimi ile, son derece özel ve / veya özel sorunlara cevap sağlamak mümkündür. Makine öğrenimi problemleri, göz önünde bulundurulan sorunun belirli konusu ve verilerinin yanı sıra veri biliminde uzmanlık gerektirir.

## <a name="what-kind-of-data-do-you-have"></a>Ne tür verileriniz var?

Bilişsel Hizmetler, bir hizmet grubu olarak, eğitilmiş model için hiçbir, bazı veya tüm özel verileri gerektiremez. 

### <a name="no-additional-training-data-required"></a>Ek eğitim verisi gerekmez

Tam eğitimli bir model sağlayan hizmetler _bir kara kutu_olarak kabul edilebilir. Nasıl çalıştıklarını veya onları eğitmek için hangi verilerin kullanıldığını bilmeniz gerekmez. Bir tahmin almak için verilerinizi tam eğitimli bir modele getirirsiniz. 

### <a name="some-or-all-training-data-required"></a>Gerekli eğitim verilerinin bir kısmı veya tümü

Bazı hizmetler, kendi verilerinizi getirmenize ve ardından bir model eğitmenize olanak sağlar. Bu, hizmetin verilerini ve algoritmasını kullanarak modeli kendi verilerinizle genişletmenize olanak tanır. Çıktı ihtiyaçlarınıza uyuyor. Kendi verilerinizi getirdiğinizde, verileri hizmete özgü bir şekilde etiketlemeniz gerekebilir. Örneğin, çiçekleri tanımlamak için bir model eğitiyorumsa, modeli eğitmek için her görüntüdeki çiçeğin konumuyla birlikte çiçek görüntülerinin bir kataloğunu sağlayabilirsiniz. 

Bir hizmet, kendi verilerini geliştirmek için veri sağlamanıza _izin_ verebilir. Bir hizmet, veri sağlamanızı _gerektirebilir._ 

### <a name="real-time-or-near-real-time-data-required"></a>Gerçek zamanlı veya yakın gerçek zamanlı veri gerekli

Bir hizmetin etkili bir model oluşturmak için gerçek zamanlı veya neredeyse gerçek zamanlı verilere ihtiyacı olabilir. Bu hizmetler önemli miktarda model verisi işlenir. 

## <a name="service-requirements-for-the-data-model"></a>Veri modeli için hizmet gereksinimleri

Aşağıdaki veriler, her hizmeti hangi tür verilere izin verdiğine veya gerektirdiğine göre kategorilere ayırıyor.

|Bilişsel Hizmet|Eğitim verisi gerekmez|Eğitim verilerinin bir kısmını veya tamamını sağlarsınız|Gerçek zamanlı veya yakın gerçek zamanlı veri toplama|
|--|--|--|--|
|[Anomali Algılayıcısı](./Anomaly-Detector/overview.md)|x|x|x|
|Bing Arama |x|||
|[BilgisayarLı Vizyon](./Computer-vision/Home.md)|x|||
|[Content Moderator](./Content-Moderator/overview.md)|x||x|
|[Özel Görüntü](./Custom-Vision-Service/home.md)||x||
|[Yüz](./Face/Overview.md)|x|x||
|[Form Tanıma](./form-recognizer/overview.md)||x||
|[Tam Ekran Okuyucu](./immersive-reader/overview.md)|x|||
|[Mürekkep Tanıma](./Ink-recognizer/overview.md)|x|x||
|[Dil Anlama (LUIS)](./LUIS/what-is-luis.md)||x||
|[Kişiselleştirme](./personalizer/what-is-personalizer.md)|x*|x*|x|
|[QnA Üreticisi](./QnAMaker/Overview/overview.md)||x||
|[Hoparlör Tanıyıcı](./speaker-recognition/home.md)||x||
|[Konuşma Metni-Konuşma (TTS)](speech-service/text-to-speech.md)|x|x||
|[Konuşma Konuşma-metin (STT)](speech-service/speech-to-text.md)|x|x||
|[Konuşma Çevirisi](speech-service/speech-translation.md)|x|||
|[Metin Analizi](./text-analytics/overview.md)|x|||
|[Çevirmen Metni](./translator/translator-info-overview.md)|x|||
|[Çevirmen Metin - özel çevirmen](./translator/custom-translator/overview.md)||x||

*Personalizer'ın politikanızı ve verilerinizi değerlendirmek için yalnızca hizmet tarafından toplanan eğitim verilerine (gerçek zamanlı olarak çalıştığı gibi) ihtiyacı vardır. Personalizer up-front veya toplu eğitim için büyük tarihsel veri kümeleri gerekmez. 

## <a name="where-can-you-use-cognitive-services"></a>Bilişsel Hizmetleri nerede kullanabilirsiniz?
 
Hizmetler, REST API(lar) veya SDK aramaları yapabilen herhangi bir uygulamada kullanılır. Uygulamalara örnek olarak web siteleri, botlar, sanal veya karma gerçeklik, masaüstü ve mobil uygulamalar verilebilir. 

## <a name="how-is-azure-cognitive-search-related-to-cognitive-services"></a>Azure Bilişsel Arama'nın Bilişsel Hizmetlerle ilişkisi nedir?

[Azure Bilişsel Arama,](../search/search-what-is-azure-search.md) iş yüklerini dizine görüntü ve doğal dil işleme eklemek için isteğe bağlı olarak Bilişsel Hizmetler kullanan ayrı bir bulut arama hizmetidir. Bilişsel Hizmetler, Azure Bilişsel Arama'da tek tek [API'leri](../search/cognitive-search-predefined-skills.md) saran yerleşik beceriler aracılığıyla ortaya çıkarır. İzler için ücretsiz bir kaynak kullanabilirsiniz, ancak daha büyük birimler için [faturalandırılabilir](../search/cognitive-search-attach-cognitive-services.md) bir kaynak oluşturmayı ve eklemeyi planlayabilirsiniz.

## <a name="how-can-you-use-cognitive-services"></a>Bilişsel Hizmetleri nasıl kullanabilirsiniz?

Her hizmet verileriniz hakkında bilgi sağlar. Konuşmaları (sesi) metne dönüştürme, metni birçok dile çevirme, ardından bir bilgi tabanından yanıt almak için çevrilmiş dilleri kullanma gibi çözümleri zincirlemek için hizmetleri birleştirebilirsiniz. Bilişsel Hizmetler kendi akıllı çözümler oluşturmak için kullanılabilir iken, onlar da modelleri tamamlamak veya geliştirme sürecini hızlandırmak için geleneksel makine öğrenme projeleri ile kombine edilebilir. 

Diğer makine öğrenimi araçları için dışa aktarılan modeller sağlayan Bilişsel Hizmetler:

|Bilişsel Hizmet|Model bilgileri|
|--|--|
|[Özel Görüntü](./custom-vision-service/home.md)|Android için Tensorflow için [dışa aktarma,](./Custom-Vision-Service/export-model-python.md) iOS11 için CoreML, Windows ML için ONNX|

## <a name="learn-more"></a>Daha fazla bilgi edinin

* [Architecture Guide - Microsoft'ta makine öğrenimi ürünleri nelerdir?](https://docs.microsoft.com/azure/architecture/data-guide/technology-choices/data-science-and-machine-learning)
* [Makine öğrenimi - Derin öğrenmeye giriş vs. makine öğrenimi](../machine-learning/concept-deep-learning-vs-machine-learning.md)

## <a name="next-steps"></a>Sonraki adımlar

* Azure [portalında](cognitive-services-apis-create-account.md) veya [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli)ile Bilişsel Hizmet hesabınızı oluşturun.
* Bilişsel Hizmete nasıl [doğrulaşmayı](authentication.md) öğrenin.
* Sorun tanımlama ve hata ayıklama için [tanısal günlüğe kaydetmeyi](diagnostic-logging.md) kullanın. 
* Bir Docker [konteyner](cognitive-services-container-support.md)içinde bir Bilişsel Hizmet dağıtmak.
* [Hizmet güncellemelerini](https://azure.microsoft.com/updates/?product=cognitive-services)güncel tutun.
