---
title: Özel Konuşma Tanıma genel bakış-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Özel Konuşma Tanıma, uygulamalarınız, araçlarınız ve ürünleriniz için Microsoft konuşmadan metne doğruluğu değerlendirmenize ve iyileştirmenize olanak tanıyan çevrimiçi araçlar kümesidir.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: trbye
ms.custom: contperf-fy21q2; references_regions
ms.openlocfilehash: f2dca513b4413f1761ea1b21bf71f90bb27d3053
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100388660"
---
# <a name="what-is-custom-speech"></a>Özel Konuşma Tanıma nedir?

[Özel konuşma tanıma](https://aka.ms/customspeech) , uygulamalarınız ve ürünleriniz için Microsoft konuşmadan metne doğruluğu değerlendirmenize ve iyileştirmenize olanak tanıyan bir kullanıcı arabirimi tabanlı araç kümesidir. Kullanmaya başlamak her şey için en çok bir test ses dosyası. Özel bir konuşmaya metin deneyimi oluşturmaya başlamak için bu makaledeki bağlantıları izleyin.

## <a name="whats-in-custom-speech"></a>Özel Konuşma Tanıma neler?

Özel Konuşma Tanıma herhangi bir şey yapabilmeniz için önce bir Azure hesabı ve bir konuşma hizmeti aboneliğine sahip olmanız gerekir. Bir hesabınız olduktan sonra verilerinizi dağıtabilir, modellerinizi eğitebilir ve test edebilir, tanınma kalitesini inceleyebilir, doğruluğu değerlendirebilir ve son olarak özel konuşmayı metin modelini kullanabilirsiniz.

Bu diyagram, [konuşma Studio 'nun özel konuşma tanıma alanını](https://aka.ms/customspeech)oluşturan parçaları vurgular. Her adım hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın.

![Konuşma Studio 'nun Özel Konuşma Tanıma alanını oluşturan bileşenleri vurgulayan diyagram.](./media/custom-speech/custom-speech-overview.png)

1. [Abone olun ve proje oluşturun](#set-up-your-azure-account). Bir Azure hesabı oluşturun ve konuşma hizmetine abone olun. Bu Birleşik abonelik, konuşmadan metne, metinden konuşmaya, konuşma çevirisine ve [konuşma Studio](https://speech.microsoft.com/customspeech)'ya erişmenizi sağlar. Ardından, ilk Özel Konuşma Tanıma projenizi oluşturmak için konuşma hizmeti aboneliğinizi kullanın.

1. [Test verilerini karşıya yükleyin](./how-to-custom-speech-test-and-train.md). Uygulamalarınız, araçlarınız ve ürünleriniz için Microsoft konuşmadan metne teklifi değerlendirmek için test verilerini (ses dosyaları) karşıya yükleyin.

1. [Tanıma kalitesini inceleyin](how-to-custom-speech-inspect-data.md). Karşıya yüklenen ses çalmak ve test verilerinizin konuşma tanıma kalitesini incelemek için [konuşma Studio 'yu](https://speech.microsoft.com/customspeech) kullanın. Nicel ölçümleri için bkz. [verileri İnceleme](how-to-custom-speech-inspect-data.md).

1. [Değerlendirin ve doğruluğu geliştirir](how-to-custom-speech-evaluate-data.md). Konuşma-metin modelinin doğruluğunu değerlendirin ve geliştirir. [Konuşma Studio](https://speech.microsoft.com/customspeech) , ek eğitim gerekip gerekmediğini tespit etmek için kullanabileceğiniz bir *sözcük hata oranı* sağlayacaktır. Doğruluğun memnun kaldıysanız, konuşma hizmeti API 'Lerini doğrudan kullanabilirsiniz. %5 ' e kadar %5 oranında %20 ' ye kadar doğru bir ortalama değeri artırmak istiyorsanız, insan etiketli döküm ve ilgili metin gibi ek eğitim verilerini karşıya yüklemek için portaldaki **eğitim** sekmesini kullanın.

1. [Bir modeli eğitme ve dağıtma](how-to-custom-speech-train-model.md). Ses test verilerinize birlikte yazılı yazılı betikler (10 ila 1.000 saat) ve ilgili metni (<200 MB) sağlayarak konuşmadan metne ait doğruluğun doğruluğunu geliştirir. Bu veriler, konuşmadan metne modeli eğmenize yardımcı olur. Eğitim sonrası, yeniden test etme. Sonuçtan memnun kaldıysanız, modelinizi özel bir uç noktaya dağıtabilirsiniz.

## <a name="set-up-your-azure-account"></a>Azure hesabınızı ayarlama

Özel bir model oluşturmak için [konuşma Studio 'yu](https://speech.microsoft.com/customspeech) kullanabilmeniz için önce bir Azure hesabı ve konuşma hizmeti aboneliğine sahip olmanız gerekir. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](overview.md#try-the-speech-service-for-free).

> [!NOTE]
> **Ses verileri** içeren özel bir modeli eğitemeniz planlandıysanız, uygun donanıma sahip olan, eğitim için kullanılabilir olan aşağıdaki bölgelerden birini seçin. Bu, bir modeli eğitmek için geçen süreyi azaltır.

* Doğu Avustralya
* Orta Kanada
* Orta Hindistan
* Doğu ABD
* Doğu ABD 2
* Orta Kuzey ABD
* Kuzey Avrupa
* Orta Güney ABD
* Güneydoğu Asya
* Güney Birleşik Krallık
* US Gov Arizona
* US Gov Virginia
* West Europe
* Batı ABD 2

> [!NOTE]
> Lütfen standart (S0) bir abonelik oluşturmayı unutmayın. Ücretsiz (F0) abonelikler desteklenmez.

Bir Azure hesabı ve bir konuşma hizmeti aboneliği oluşturduktan sonra, [konuşma Studio](https://speech.microsoft.com/customspeech) 'da oturum açmanız ve aboneliğinizi bağlamanız gerekir.

1. [Konuşma Studio](https://aka.ms/custom-speech)'da oturum açın.
1. Üzerinde çalışmanız gereken aboneliği seçin ve bir konuşma projesi oluşturun.
1. Aboneliğinizi değiştirmek isterseniz, üst menüdeki dişli düğmesini seçin.

## <a name="how-to-create-a-project"></a>Proje oluşturma

Veriler, modeller, testler ve uç noktalar gibi içerikler, [konuşma Studio](https://speech.microsoft.com/customspeech)'daki *Projeler* halinde düzenlenmiştir. Her proje bir etki alanı ve ülke/dil için özeldir. Örneğin, Birleşik Devletler Ingilizce kullanan çağrı merkezleri için bir proje oluşturabilirsiniz.

İlk projenizi oluşturmak için **konuşmayı metne/özel konuşma**' yı seçin ve ardından **Yeni proje**' yi seçin. Projenizi oluşturmak için sihirbaz tarafından sunulan yönergeleri izleyin. Bir proje oluşturduktan sonra, dört sekme görmeniz gerekir: **veri**, **Test**, **eğitim** ve **dağıtım**. Her bir sekmeyi nasıl kullanacağınızı öğrenmek için [sonraki adımlarda](#next-steps) sunulan bağlantıları kullanın.

> [!IMPORTANT]
> Daha önce "Özel Konuşma Tanıma Portal" olarak bilinen [konuşma Studio](https://aka.ms/custom-speech) yakın zamanda güncelleştirildi! CRIS.ai portalında veya API 'lerle önceki veriler, modeller, testler ve yayımlanmış uç noktalar oluşturduysanız, bu eski varlıklara bağlanmak için yeni portalda yeni bir proje oluşturmanız gerekir.

## <a name="model-lifecycle"></a>Model yaşam döngüsü

Özel Konuşma Tanıma hem *temel modelleri* hem de *özel modelleri* kullanır. Her dilin bir veya daha fazla temel modeli vardır. Genellikle, normal konuşma hizmetine yeni bir konuşma modeli sunulduğunda, yeni bir temel model olarak Özel Konuşma Tanıma hizmetine de aktarılır. Her 3 ayda bir güncelleştirilir. En yeni model genellikle daha fazla doğruluk sağladığından, eski modeller genellikle zaman içinde daha az yararlı hale gelir.

Buna karşılık, özel modeller seçilen bir temel modeli belirli bir müşteri senaryosuna uyarlayarak oluşturulur. Gereksinimlerinizi karşıladıktan sonra, belirli bir özel modeli uzun bir süre kullanmaya devam edebilirsiniz. Ancak, en son temel modele düzenli olarak güncelleştirmeniz ve ek verilerle zaman içinde yeniden eğitmeniz önerilir. 

Model yaşam döngüsüyle ilgili diğer önemli terimler şunlardır:

* **Uyarlama**: bir temel model alma ve metin verileri ve/veya ses verilerini kullanarak etki alanı/senaryonuz ile özelleştirme.
* **Kod çözme**: model kullanma ve konuşma tanımayı gerçekleştirme (sesin metin olarak kodunu çözme).
* **Uç noktası**: bir temel modelin veya *yalnızca* belirli bir kullanıcıya erişilebilen özel bir modelin kullanıcıya özgü bir dağıtımı.

### <a name="expiration-timeline"></a>Süre sonu zaman çizelgesi

Yeni modeller ve yeni işlevler kullanılabilir hale gelir ve daha eski, daha az doğru modeller kullanımdan kalkmışsa model ve uç nokta süre sonu için aşağıdaki zaman çizelgelerine bakın:

**Taban modeller** 

* Uyarlama: bir yıl boyunca kullanılabilir. Model alındıktan sonra, özel modeller oluşturmak için bir yıl kullanılabilir. Bir yıldan sonra, yeni özel modellerin daha yeni bir temel model sürümünden oluşturulması gerekir.  
* Kod çözme: içeri aktarma işleminden sonraki iki yıl boyunca kullanılabilir. Bu nedenle, bir uç nokta oluşturabilir ve bu modelle iki yıl boyunca toplu iş dökümü kullanabilirsiniz. 
* Uç noktalar: kod çözme ile aynı zaman çizelgesinde kullanılabilir.

**Özel modeller**

* Kod çözme: model oluşturulduktan iki yıl sonra kullanılabilir. Bu nedenle, özel modeli oluşturulduktan sonra iki yıl (Batch/gerçek zamanlı/test) kullanabilirsiniz. İki yıl sonra, temel model genellikle uyarlama için kullanım dışı bırakıldığı için *modelinize yeniden eğmelisiniz* .  
* Uç noktalar: kod çözme ile aynı zaman çizelgesinde kullanılabilir.

Bir temel model veya özel modelin süresi dolmuşsa, her zaman *en yeni temel model sürümüne* geri döner. Bu nedenle, uygulamanız hiçbir zaman kesintiye uğramayacaktır, ancak özel modellerin süresi dolduktan sonra *belirli verileriniz* için daha az doğru olabilir. Bir modelin kullanım süresini, konuşma Studio 'nun Özel Konuşma Tanıma alanında aşağıdaki konumlarda görebilirsiniz:

* Model Eğitimi Özeti
* Model eğitimi ayrıntısı
* Dağıtım özeti
* Dağıtım ayrıntısı

Ayrıca, [`GetModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetModel) [`GetBaseModel`](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetBaseModel) JSON yanıtında özelliği altındaki ve özel konuşma API 'leri aracılığıyla sona erme tarihlerini de denetleyebilirsiniz `deprecationDates` .

Bir özel konuşma uç noktasındaki modeli, konuşma Studio 'nun dağıtım bölümünde uç nokta tarafından kullanılan modeli değiştirerek veya özel konuşma API 'SI aracılığıyla yükseltebileceğinizi unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Verilerinizi hazırlayın ve test edin](./how-to-custom-speech-test-and-train.md)
* [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)
* [Model doğruluğunu değerlendirin ve geliştirebilirsiniz](how-to-custom-speech-evaluate-data.md)
* [Modeli eğitme ve dağıtma](how-to-custom-speech-train-model.md)
