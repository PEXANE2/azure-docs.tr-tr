---
title: Özel Konuşma Tanıma genel bakış-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Özel Konuşma Tanıma, uygulamalarınız, araçlarınız ve ürünleriniz için konuşmadan metne doğruluğu değerlendirmenize ve iyileştirmenize olanak tanıyan çevrimiçi araçlar kümesidir. Kullanmaya başlamak her şey en çok bir test ses dosyası. Özel bir konuşmaya metin deneyimi oluşturmaya başlamak için aşağıdaki bağlantıları izleyin.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/11/2020
ms.author: trbye
ms.openlocfilehash: 6dbe97d615753f0a90c8ba80aa7afa6dafa15eb2
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94658477"
---
# <a name="what-is-custom-speech"></a>Özel Konuşma Tanıma nedir?

[Özel konuşma tanıma](https://aka.ms/customspeech) , Microsoft 'un Uygulamalarınız ve ürünleriniz için konuşmayı metne doğruluğu değerlendirmenize ve iyileştirmenize olanak tanıyan bir UI tabanlı araç kümesidir. Kullanmaya başlamak her şey en çok bir test ses dosyası. Özel bir konuşmaya metin deneyimi oluşturmaya başlamak için aşağıdaki bağlantıları izleyin.

## <a name="whats-in-custom-speech"></a>Özel Konuşma Tanıma neler?

Özel Konuşma Tanıma herhangi bir şey yapabilmeniz için önce bir Azure hesabı ve bir konuşma hizmeti aboneliğine sahip olmanız gerekir. Bir hesap aldıktan sonra verilerinizi dağıtabilir, modellerinizi eğitebilir ve test edebilir, tanınma kalitesini inceleyebilir, doğruluğu değerlendirebilir ve son olarak özel konuşmayı metin modelini kullanabilirsiniz.

Bu diyagramda [özel konuşma tanıma portalını](https://aka.ms/customspeech)oluşturan parçalar vurgulanmıştır. Her adım hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın.

![Özel Konuşma Tanıma portalını oluşturan farklı bileşenleri vurgular.](./media/custom-speech/custom-speech-overview.png)

1. [Abone olun ve proje oluşturun](#set-up-your-azure-account) -bir Azure hesabı oluşturun ve konuşma hizmetine abone olun. Bu Birleşik abonelik, konuşmadan metne, metinden konuşmaya, konuşma çevirisine ve [özel konuşma tanıma portalına](https://speech.microsoft.com/customspeech)erişmenizi sağlar. Ardından, konuşma hizmeti aboneliğinizi kullanarak ilk Özel Konuşma Tanıma projenizi oluşturun.

1. [Test verilerini karşıya yükleme](how-to-custom-speech-test-data.md) -Microsoft 'un uygulamalarınız, araçları ve ürünleriniz için konuşma-metin tekliflerini değerlendirmek için test verilerini (ses dosyaları) karşıya yükleyin.

1. [Tanıma kalitesini inceleme](how-to-custom-speech-inspect-data.md) -karşıya yüklenen ses çalmak ve test verilerinizin konuşma tanıma kalitesini incelemek için [özel konuşma tanıma portalını](https://speech.microsoft.com/customspeech) kullanın. Nicel ölçümleri için bkz. [verileri İnceleme](how-to-custom-speech-inspect-data.md).

1. [Doğruluğu değerlendirin ve iyileştirin](how-to-custom-speech-evaluate-data.md) , konuşma-metin modelinin doğruluğunu değerlendirin ve iyileştirin. [Özel konuşma tanıma Portal](https://speech.microsoft.com/customspeech) , ek eğitim gerekip gerekmediğini belirlemede kullanılabilecek bir *sözcük hata oranı* sağlar. Doğruluğun memnun kaldıysanız, konuşma hizmeti API 'Lerini doğrudan kullanabilirsiniz. %5 oranında %20 oranında doğru bir ortalama değeri artırmak isterseniz, insan etiketli döküm ve ilgili metin gibi ek eğitim verilerini karşıya yüklemek için portaldaki **eğitim** sekmesini kullanın.

1. [Model eğitme ve dağıtma](how-to-custom-speech-train-model.md) -ses test verilerinize birlikte yazılı yazılı betikler (10-1000 saat) ve ilgili metni (<200 MB) sağlayarak konuşmadan metne ait doğruluğun doğruluğunu geliştirir. Bu veriler, konuşmadan metne modeli eğmenize yardımcı olur. Eğitim, yeniden test ve sonuçtan memnun kaldığınızda, modelinizi özel bir uç noktaya dağıtabilirsiniz.

## <a name="set-up-your-azure-account"></a>Azure hesabınızı ayarlama

Özel bir model oluşturmak için [özel konuşma tanıma portalını](https://speech.microsoft.com/customspeech) kullanabilmeniz Için bir Azure hesabı ve konuşma hizmeti aboneliği gerekir. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](overview.md#try-the-speech-service-for-free).

> [!NOTE]
> Lütfen standart (S0) abonelikler oluşturmayı unutmayın, ücretsiz (F0) abonelikler desteklenmez.

Bir Azure hesabı ve bir konuşma hizmeti aboneliği oluşturduktan sonra, [özel konuşma tanıma Portal](https://speech.microsoft.com/customspeech) 'da oturum açmanız ve aboneliğinizi bağlamanız gerekir.

1. [Özel konuşma tanıma portalında](https://aka.ms/custom-speech)oturum açın.
1. Üzerinde çalışmanız gereken aboneliği seçin ve bir konuşma projesi oluşturun.
1. Aboneliğinizi değiştirmek isterseniz, üstteki gezinmede bulunan **dişli** simgesini kullanın.

## <a name="how-to-create-a-project"></a>Proje oluşturma

Veriler, modeller, testler ve uç noktalar gibi içerikler [özel konuşma tanıma portalındaki](https://speech.microsoft.com/customspeech) **Projeler** halinde düzenlenir. Her proje bir etki alanı ve ülke/dil için özeldir. Örneğin, Birleşik Devletler Ingilizce kullanan çağrı merkezleri için bir proje oluşturabilirsiniz.

İlk projenizi oluşturmak için **konuşmayı metne/özel konuşmayı** seçin ve ardından **Yeni proje**' ye tıklayın. Projenizi oluşturmak için sihirbaz tarafından sunulan yönergeleri izleyin. Bir projeyi oluşturduktan sonra, dört sekme görmeniz gerekir: **veri**, **Test**, **eğitim** ve **dağıtım**. Her bir sekmeyi nasıl kullanacağınızı öğrenmek için [sonraki adımlarda](#next-steps) sunulan bağlantıları kullanın.

> [!IMPORTANT]
> [Özel konuşma tanıma portalı](https://aka.ms/custom-speech) yakın zamanda güncelleştirildi! CRIS.ai portalında veya API 'lerle önceki veriler, modeller, testler ve yayımlanmış uç noktalar oluşturduysanız, bu eski varlıklara bağlanmak için yeni portalda yeni bir proje oluşturmanız gerekir.

## <a name="model-lifecycle"></a>Model yaşam döngüsü

Özel konuşma hem **temel modelleri** hem de **özel modelleri** kullanır. Her dilin bir veya daha fazla **temel modeli** vardır. Genellikle, normal konuşma hizmetine yeni bir konuşma modeli sunulduğunda, yeni bir **temel model** olarak özel konuşma tanıma hizmetine de aktarılır. Genellikle her 3-6 ayda bir güncelleştirilir ve en yeni model genellikle daha fazla doğruluk sahibi olduğu için eski modeller zaman içinde daha az yararlı hale gelir.

Buna karşılık, **özel modeller** seçilen bir temel modeli belirli bir müşteri senaryosuna uyarlayarak oluşturulur. Gereksinimlerinize uygun bir süre sonra belirli bir özel modeli kullanmaya devam edebilirsiniz, ancak en son temel modele düzenli olarak güncelleştirmeniz ve ek verilerle yeniden eğmeniz önerilir.

Model yaşam döngüsüyle ilgili diğer önemli terimler şunlardır:

* **Uyarlama**: temel model alma ve metin verileri ve/veya ses verilerini kullanarak etki alanı/senaryonuz ile özelleştirme
* **Kod çözme**: model kullanma ve konuşma tanımayı gerçekleştirme (sesin metin olarak kodunu çözme)
* **Uç noktası**: bir temel modelin veya *yalnızca* belirli bir kullanıcı tarafından erişilebilen özel bir modelin kullanıcıya özgü bir dağıtımı.

### <a name="expiration-timeline"></a>Süre sonu zaman çizelgesi

Yeni modeller ve yeni işlevler kullanılabilir hale gelir ve daha eski, daha az doğru modeller kullanımdan kalkmışsa model ve uç nokta süre sonu için aşağıdaki zaman çizelgelerine bakın:

**Taban modeller** 

* Uyarlama: 1 yıl boyunca kullanılabilir. Model içeri aktarıldıktan sonra, özel modeller oluşturmak için 1 yıl kullanılabilir. 1 yıldan sonra yeni özel modellerin daha yeni bir temel model sürümünden oluşturulması gerekir.  
* Kod çözme: içeri aktarmadan sonra 2 yıl boyunca kullanılabilir. Bu, bir uç nokta oluşturabileceğiniz ve bu modelle 2 yıl boyunca toplu iş dökümü kullanabileceğiniz anlamına gelir. 
* Uç noktalar: kod çözmede aynı zaman çizelgesinde kullanılabilir

**Özel modeller**

* Kod çözme: model oluşturulduktan sonra 2 yıl boyunca kullanılabilir. Bu, özel modeli 2 yıl (Batch/gerçek zamanlı/test) oluşturulduktan sonra kullanabileceğiniz anlamına gelir. 2 yıldan sonra **modelinizi yeniden eğitmeniz gerekir**, çünkü çoğu genellikle temel model uyarlama için kullanım dışıdır.  
* Uç noktalar: kod çözmede aynı zaman çizelgesinde kullanılabilir

Bir temel model veya özel modelin süresi dolmuşsa, her zaman **en yeni temel model sürümüne** geri döner. Bu nedenle, uygulamanız hiçbir zaman kesintiye uğramayacaktır, ancak özel modellerin süre sonu ile iletişime *geçebilmesi* durumunda bu veriler daha az doğru olabilir. Bir modelin kullanım süresini Özel Konuşma Tanıma portalında aşağıdaki konumlarda görebilirsiniz:

* Model Eğitimi Özeti
* Model eğitimi ayrıntısı
* Dağıtım özeti
* Dağıtım ayrıntısı

Ayrıca, [`GetModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetModel) [`GetBaseModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetBaseModel) JSON yanıtında özelliği altındaki ve özel konuşma API 'leri aracılığıyla sona erme tarihlerini de denetleyebilirsiniz `deprecationDates` .

Özel konuşma portalının dağıtım bölümünde uç nokta tarafından kullanılan modeli değiştirerek veya özel konuşma API 'SI aracılığıyla, özel bir konuşma uç noktasındaki modeli, kapalı kalma süresi olmadan yükseltebileceğinizi unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Verilerinizi hazırlayın ve test edin](how-to-custom-speech-test-data.md)
* [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)
* [Model doğruluğunu değerlendirin ve geliştirebilirsiniz](how-to-custom-speech-evaluate-data.md)
* [Modeli eğitme ve dağıtma](how-to-custom-speech-train-model.md)
