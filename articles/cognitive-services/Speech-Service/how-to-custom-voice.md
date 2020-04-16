---
title: Custom Voice ile sentezi geliştirin - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Özel Ses, markanız için tanınabilir, türünün tek bir sesini oluşturmanıza olanak tanıyan çevrimiçi araçlar kümesidir. Başlamak için gereken tek şey bir avuç ses dosyası ve ilgili transkripsiyonlar. Özel bir metin den metine deneyim oluşturmaya başlamak için aşağıdaki bağlantıları izleyin.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: trbye
ms.openlocfilehash: 2aacf8881a57f0677177cd341a0ddc63224b26b6
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81402048"
---
# <a name="get-started-with-custom-voice"></a>Özel Ses’i kullanmaya başlama

[Özel Ses,](https://aka.ms/customvoice) markanız için tanınabilir, türünün tek bir sesini oluşturmanıza olanak tanıyan çevrimiçi araçlar kümesidir. Başlamak için gereken tek şey bir avuç ses dosyası ve ilgili transkripsiyonlar. Özel bir metinden konuşmaya deneyim oluşturmaya başlamak için aşağıdaki bağlantıları izleyin.

## <a name="whats-in-custom-voice"></a>Özel Ses'te neler var?

Özel Ses ile başlamadan önce bir Azure hesabı ve Konuşma hizmeti aboneliği ne ihtiyacınız vardır. Bir hesap oluşturduktan sonra, verilerinizi hazırlayabilir, modellerinizi eğitebilir ve test edebilir, ses kalitesini değerlendirebilir ve sonuçta özel ses modelinizi dağıtabilirsiniz.

Aşağıdaki diyagram, [Özel Ses portalını](https://aka.ms/customvoice)kullanarak özel bir ses modeli oluşturma adımlarını vurgular. Daha fazla bilgi edinmek için bağlantıları kullanın.

![Özel Ses mimarisi diyagramı](media/custom-voice/custom-voice-diagram.png)

1. [Abone olun ve proje oluşturun](#set-up-your-azure-account) - Bir Azure hesabı oluşturun ve bir Konuşma hizmeti aboneliği oluşturun. Bu birleştirilmiş abonelik, konuşmadan metne, metinden konuşmaya, konuşma çevirisine ve Özel Ses portalına erişim sağlar. Ardından, Konuşma hizmeti aboneliğinizi kullanarak ilk Özel Ses projenizi oluşturun.

2. [Veri yükleme](how-to-custom-voice-create-voice.md#upload-your-datasets) - Özel Ses portalı veya Özel Ses API'sini kullanarak veri (ses ve metin) yükleyin. Portaldan telaffuz puanlarını ve sinyal-gürültü oranlarını araştırabilir ve değerlendirebilirsiniz. Daha fazla bilgi [için, Özel Ses için veri hazırlama ya da nasıl hazırlanacağını](how-to-custom-voice-prepare-data.md)görün.

3. [Modelinizi eğitin](how-to-custom-voice-create-voice.md#build-your-custom-voice-model) – Özel bir metinden konuşmaya ses modeli oluşturmak için verilerinizi kullanın. Farklı dillerde bir model eğitebilirsiniz. Eğitimden sonra, modelinizi test edin ve sonuçtan memnunsanız, modeli dağıtabilirsiniz.

4. [Modelinizi dağıtın](how-to-custom-voice-create-voice.md#create-and-use-a-custom-voice-endpoint) - Metinden konuşmaya ses modeliniz için özel bir bitiş noktası oluşturun ve ürünlerinizde, araçlarınızda ve uygulamalarınızda konuşma sentezi için kullanın.

## <a name="custom-neural-voices"></a>Özel Sinirsel sesler

Nöral ses özelleştirme özelliği şu anda genel önizlemede, seçili müşterilerle sınırlıdır. Başlamak için bu [başvuru formunu](https://go.microsoft.com/fwlink/?linkid=2108737) doldurun.

> [!NOTE]
> Microsoft'un sorumlu AI'yi tasarlama taahhüdünün bir parçası olarak, amacımız bireylerin ve toplumun haklarını korumak ve şeffaf insan-bilgisayar etkileşimlerini teşvik etmektir. Bu nedenle, Özel Nöral Ses genellikle tüm müşteriler için kullanılabilir değildir. Teknolojiye ancak uygulamalarınız incelendikten ve etik ilkelerimize uygun olarak kullanmayı taahhüt ettikten sonra erişebilirsiniz. [Uygulama gating sürecimiz](https://aka.ms/custom-neural-gating-overview)hakkında daha fazla bilgi edinin.

## <a name="set-up-your-azure-account"></a>Azure hesabınızı ayarlama

Özel Bir model oluşturmak için Özel Konuşma portalını kullanabilmeniz için konuşma hizmeti aboneliği gerekir. Azure'da konuşma hizmeti aboneliği oluşturmak için bu yönergeleri izleyin. Azure hesabınız yoksa, yeni bir hesabınız için kaydolabilirsiniz.  

Bir Azure hesabı ve Konuşma hizmeti aboneliği oluşturduktan sonra, Özel Ses portalında oturum açmanız ve aboneliğinizi bağlamanız gerekir.

1. Azure portalından Konuşma hizmeti abonelik anahtarınızı alın.
2. Özel Ses [portalında](https://aka.ms/custom-voice)oturum açın.
3. Aboneliğinizi seçin ve bir konuşma projesi oluşturun.
4. Başka bir Konuşma aboneliğine geçmek istiyorsanız, üst gezintide bulunan dişli simgesini kullanın.

> [!NOTE]
> Custom Voice hizmeti 30 günlük ücretsiz deneme anahtarını desteklemiyor. Hizmeti kullanabiliyor sanız, Azure'da f0 veya S0 anahtarı nın oluşturulması gerekir.

## <a name="how-to-create-a-project"></a>Proje oluşturma

Veri, model, test ve uç nokta gibi içerik, Özel Ses portalındaki **Projeler'de** düzenlenir. Her proje bir ülkeye/dile ve oluşturmak istediğiniz sesin cinsiyetine özgüdür. Örneğin, çağrı merkezinizin ABD'de İngilizce kullanan sohbet botları için bir kadın sesi için bir proje oluşturabilirsiniz (tr-US).

İlk projenizi oluşturmak için **Metinden Konuşmaya/Özel Ses** sekmesini seçin ve ardından **Yeni Proje'yi**tıklatın. Projenizi oluşturmak için sihirbaz tarafından sağlanan yönergeleri izleyin. Bir proje oluşturduktan sonra dört sekme görürsünüz: **Veri,** **Eğitim,** **Test**ve **Dağıtım.** Her sekmeyi nasıl kullanacağınızı öğrenmek için [Sonraki adımlarda](#next-steps) sağlanan bağlantıları kullanın.

> [!IMPORTANT]
> [Özel Ses portalı](https://aka.ms/custom-voice) yakın zamanda güncellendi! önceki verileri, modelleri, testleri ve yayımlanmış bitiş noktalarını CRIS.ai portalında veya API'lerle oluşturduysanız, bu eski varlıklara bağlanmak için yeni portalda yeni bir proje oluşturmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel Ses verileri hazırlama](how-to-custom-voice-prepare-data.md)
- [Özel Ses Oluşturma](how-to-custom-voice-create-voice.md)
- [Kılavuz: Ses örneklerinizi kaydetme](record-custom-voice-samples.md)
