---
title: Özel Konuşma ile başlayın - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Özel Konuşma, uygulamalarınız, araçlarınız ve ürünleriniz için metin den metin konuşma doğruluğunu değerlendirmenize ve geliştirmenize olanak tanıyan çevrimiçi araçlar kümesidir. Başlamak için gereken tek şey bir avuç test ses dosyası. Özel bir metin den metine deneyim oluşturmaya başlamak için aşağıdaki bağlantıları izleyin.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: trbye
ms.openlocfilehash: a52735ee62f564a5dc536fd2b7d3539406388341
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81402076"
---
# <a name="what-is-custom-speech"></a>Özel Konuşma Nedir?

[Özel Konuşma,](https://aka.ms/customspeech) Microsoft'un uygulamalarınız, araçlarınız ve ürünleriniz için metin konuşma doğruluğunu değerlendirmenize ve geliştirmenize olanak tanıyan çevrimiçi araçlar kümesidir. Başlamak için gereken tek şey bir avuç test ses dosyası. Özel bir metin den metine deneyim oluşturmaya başlamak için aşağıdaki bağlantıları izleyin.

## <a name="whats-in-custom-speech"></a>Özel Konuşma'da ne var?

Özel Konuşma ile herhangi bir şey yapmadan önce bir Azure hesabına ve Konuşma hizmeti aboneliğine ihtiyacınız vardır. Bir hesabınız olduktan sonra, verilerinizi hazırlayabilir, modellerinizi eğitebilir ve test edebilir, tanıma kalitesini inceleyebilir, doğruluğu değerlendirebilir ve sonuçta özel konuşmadan metne modelini dağıtabilir ve kullanabilirsiniz.

Bu diyagram, Özel Konuşma [portalını](https://aka.ms/customspeech)oluşturan parçaları vurgular. Her adım hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın.

![Özel Konuşma portalını oluşturan farklı bileşenleri vurgular.](./media/custom-speech/custom-speech-overview.png)

1. [Abone olun ve bir proje oluşturun](#set-up-your-azure-account) - Bir Azure hesabı oluşturun ve Konuşma hizmetine abone olun. Bu birleştirilmiş abonelik, konuşmadan metne, metinden konuşmaya, konuşma çevirisine ve [Özel Konuşma portalına](https://speech.microsoft.com/customspeech)erişim sağlar. Ardından, Konuşma hizmeti aboneliğinizi kullanarak ilk Özel Konuşma projenizi oluşturun.

2. [Test verilerini yükleyin](how-to-custom-speech-test-data.md) - Microsoft'un uygulamalarınız, araçlarınız ve ürünleriniz için metin den söze sunduğu konuşma teklifini değerlendirmek için test verilerini (ses dosyaları) yükleyin.

3. [Tanıma kalitesini inceleyin](how-to-custom-speech-inspect-data.md) - Yüklenen sesi oynatmak ve test verilerinizin konuşma tanıma kalitesini incelemek için [Özel Konuşma portalını](https://speech.microsoft.com/customspeech) kullanın. Nicel ölçümler için [bkz.](how-to-custom-speech-inspect-data.md)

4. [Doğruluğu değerlendirin](how-to-custom-speech-evaluate-data.md) - Konuşmadan metne modelin doğruluğunu değerlendirin. [Özel Konuşma portalı,](https://speech.microsoft.com/customspeech) ek eğitim gerekip gerekip gerekmeyeceğini belirlemek için kullanılabilecek bir *Sözcük Hata Oranı*sağlar. Doğruluktan memnunsanız, Konuşma hizmeti API'lerini doğrudan kullanabilirsiniz. Doğruluğu göreceli olarak %5 - %20 oranında artırmak istiyorsanız, insan etiketli transkriptler ve ilgili metin gibi ek eğitim verilerini yüklemek için portaldaki **Eğitim** sekmesini kullanın.

5. [Modeli eğitin](how-to-custom-speech-train-model.md) - Ses testi verilerinizle birlikte yazılı transkript (10-1.000 saat) ve ilgili metin (<200 MB) sağlayarak konuşma-metin modelinin doğruluğunu artırın. Bu veriler, konuşma-metin modeli nin eğitilen yardımcı olur. Eğitimden sonra, yeniden test edin ve sonuçtan memnunsanız, modelinizi dağıtabilirsiniz.

6. [Modeli dağıtın](how-to-custom-speech-deploy-model.md) - Konuşmadan metne modeliniz için özel bir bitiş noktası oluşturun ve uygulamalarınızda, araçlarınızda veya ürünlerinizde kullanın.

## <a name="set-up-your-azure-account"></a>Azure hesabınızı ayarlama

Özel Bir model oluşturmak için [Özel Konuşma portalını](https://speech.microsoft.com/customspeech) kullanabilmeniz için konuşma hizmeti aboneliği gerekir. Standart bir Konuşma hizmeti aboneliği oluşturmak için aşağıdaki yönergeleri izleyin: [Konuşma Aboneliği oluşturun.](get-started.md#new-resource)

> [!NOTE]
> Lütfen standart (S0) abonelikler oluşturduğunuzdan emin olun, ücretsiz deneme (F0) abonelikleri desteklenmez.

Bir Azure hesabı ve Konuşma hizmeti aboneliği oluşturduktan sonra, Özel [Konuşma portalında](https://speech.microsoft.com/customspeech) oturum açmanız ve aboneliğinizi bağlamanız gerekir.

1. Azure portalından Konuşma hizmeti abonelik anahtarınızı alın.
2. [Özel Konuşma portalında](https://aka.ms/custom-speech)oturum açma.
3. Üzerinde çalışmanız gereken aboneliği seçin ve bir konuşma projesi oluşturun.
4. Aboneliğinizi değiştirmek istiyorsanız, üst gezintide bulunan **dişli** simgesini kullanın.

## <a name="how-to-create-a-project"></a>Proje oluşturma

Veriler, modeller, testler ve uç noktalar gibi içerik, [Özel Konuşma portalındaki](https://speech.microsoft.com/customspeech) **Projeler** olarak düzenlenir. Her proje bir etki alanına ve ülkeye/dile özgüdür. Örneğin, ABD'de İngilizce kullanan çağrı merkezleri için bir proje oluşturabilirsiniz.

İlk projenizi oluşturmak **için, Metinden Metne/Özel Konuşma'yı**seçin ve ardından **Yeni Proje'yi**tıklatın. Projenizi oluşturmak için sihirbaz tarafından sağlanan yönergeleri izleyin. Bir proje oluşturduktan sonra dört sekme görmeniz gerekir: **Veri,** **Sınama,** **Eğitim**ve **Dağıtım.** Her sekmeyi nasıl kullanacağınızı öğrenmek için [Sonraki adımlarda](#next-steps) sağlanan bağlantıları kullanın.

> [!IMPORTANT]
> [Özel Konuşma portalı](https://aka.ms/custom-speech) yakın zamanda güncellendi! önceki verileri, modelleri, testleri ve yayımlanmış bitiş noktalarını CRIS.ai portalında veya API'lerle oluşturduysanız, bu eski varlıklara bağlanmak için yeni portalda yeni bir proje oluşturmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Verilerinizi hazırlayın ve test edin](how-to-custom-speech-test-data.md)
* [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)
* [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
* [Modelinizi eğitin](how-to-custom-speech-train-model.md)
* [Modelinizi dağıtın](how-to-custom-speech-deploy-model.md)
