---
title: Özel sesli konuşma hizmeti ile birleştirmenin geliştirilmesi
titleSuffix: Azure Cognitive Services
description: Özel ses, markanız için tanınabilir, tek bir tür ses oluşturmanıza olanak sağlayan bir dizi çevrimiçi araç. Kullanmaya başlamak her şey için çok sayıda ses dosyası ve ilişkili dökümlerdir. Özel bir konuşmaya metin deneyimi oluşturmaya başlamak için aşağıdaki bağlantıları izleyin.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: trbye
ms.openlocfilehash: 2aacf8881a57f0677177cd341a0ddc63224b26b6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81402048"
---
# <a name="get-started-with-custom-voice"></a>Özel Ses’i kullanmaya başlama

[Özel ses](https://aka.ms/customvoice) , markanız için tanınabilir, tek bir tür ses oluşturmanıza olanak sağlayan bir dizi çevrimiçi araç. Kullanmaya başlamak her şey için çok sayıda ses dosyası ve ilişkili dökümlerdir. Özel bir metin okuma deneyimi oluşturmaya başlamak için aşağıdaki bağlantıları izleyin.

## <a name="whats-in-custom-voice"></a>Özel ses nedir?

Özel sesle başlamadan önce bir Azure hesabı ve bir konuşma hizmeti aboneliğine sahip olmanız gerekir. Bir hesap oluşturduktan sonra verilerinizi hazırlayabilir, modellerinizi eğitmenizi, test edebilir, ses kalitesini değerlendirebilir ve sonunda özel ses modelinizi dağıtabilirsiniz.

Aşağıdaki diyagramda [özel ses portalını](https://aka.ms/customvoice)kullanarak özel bir ses modeli oluşturma adımları vurgulanmaktadır. Daha fazla bilgi edinmek için bağlantıları kullanın.

![Özel ses mimarisi diyagramı](media/custom-voice/custom-voice-diagram.png)

1. [Abone olun ve proje oluşturun](#set-up-your-azure-account) -Azure hesabı oluşturun ve bir konuşma hizmeti aboneliği oluşturun. Bu Birleşik abonelik, konuşmadan metne, metinden konuşmaya, konuşma çevirisine ve özel ses portalına erişmenizi sağlar. Ardından, konuşma hizmeti aboneliğinizi kullanarak ilk özel ses projenizi oluşturun.

2. [Karşıya veri yükleme](how-to-custom-voice-create-voice.md#upload-your-datasets) -özel ses portalı veya özel ses API 'sini kullanarak verileri (ses ve metin) karşıya yükleyin. Portalda, Söyleniş puanlarını ve sinyal-gürültü oranlarını araştırabilir ve değerlendirebilirsiniz. Daha fazla bilgi için bkz. [özel ses için verileri hazırlama](how-to-custom-voice-prepare-data.md).

3. [Modelinize eğitme](how-to-custom-voice-create-voice.md#build-your-custom-voice-model) : verilerinizi kullanarak özel bir metinden konuşmaya ses modeli oluşturun. Modeli farklı dillerde eğitebilirsiniz. Eğitim sonrasında modelinizi test edin ve sonuçtan memnun kaldıysanız modeli dağıtabilirsiniz.

4. [Modelinizi dağıtın](how-to-custom-voice-create-voice.md#create-and-use-a-custom-voice-endpoint) -metinden konuşmaya ses modeliniz için özel bir uç nokta oluşturun ve bunu, ürün, araç ve uygulamalarınızda konuşma sensimi için kullanın.

## <a name="custom-neural-voices"></a>Özel sinir sesleri

Sinir ses özelleştirme özelliği şu anda seçili müşterilerle sınırlı olan genel önizlemededir. Başlamak için bu [uygulama formunu](https://go.microsoft.com/fwlink/?linkid=2108737) doldurun.

> [!NOTE]
> Microsoft 'un sorumlu AI tasarlama taahhüdünün bir parçası olarak, bireyler ve topluluğu 'in haklarını korumaktır. Bu nedenle, özel sinir Voice tüm müşterilerin genel kullanımına açık değildir. Teknolojiden yalnızca uygulamalarınız incelendikten ve bunu etlenmiş ICS ilkelerimize göre hizalı olarak kullanmayı taahhüt ettikten sonra erişebilirsiniz. [Uygulama geçişi sürecimiz](https://aka.ms/custom-neural-gating-overview)hakkında daha fazla bilgi edinin.

## <a name="set-up-your-azure-account"></a>Azure hesabınızı ayarlama

Özel bir model oluşturmak için Özel Konuşma Tanıma portalını kullanabilmeniz için bir konuşma hizmeti aboneliği gerekir. Azure 'da bir konuşma hizmeti aboneliği oluşturmak için bu yönergeleri izleyin. Bir Azure hesabınız yoksa, yeni bir hesap için kaydolabilirsiniz.  

Bir Azure hesabı ve bir konuşma hizmeti aboneliği oluşturduktan sonra, özel sesli portalda oturum açmanız ve aboneliğinizi bağlamanız gerekir.

1. Azure portal konuşma hizmeti abonelik anahtarınızı alın.
2. [Özel ses portalında](https://aka.ms/custom-voice)oturum açın.
3. Aboneliğinizi seçin ve bir konuşma projesi oluşturun.
4. Başka bir konuşma aboneliğine geçiş yapmak isterseniz, üstteki gezinmede bulunan dişli simgesini kullanın.

> [!NOTE]
> Özel ses hizmeti 30 günlük ücretsiz deneme anahtarını desteklemez. Hizmeti kullanabilmeniz için önce Azure 'da oluşturulmuş bir F0 veya S0 anahtarınız olmalıdır.

## <a name="how-to-create-a-project"></a>Proje oluşturma

Veriler, modeller, testler ve uç noktalar gibi içerikler, özel ses portalındaki **Projeler** halinde düzenlenmiştir. Her proje, oluşturmak istediğiniz sesin ülke/dil ve cinsiyetine özgüdür. Örneğin, Birleşik Devletler (en-US) içinde Ingilizce kullanan çağrı merkezinizin sohbet botları için kadın sesi için bir proje oluşturabilirsiniz.

İlk projenizi oluşturmak için, **metinden konuşmaya/özel ses** sekmesini seçin ve ardından **Yeni proje**' ye tıklayın. Projenizi oluşturmak için sihirbaz tarafından sunulan yönergeleri izleyin. Projeyi oluşturduktan sonra dört sekme görürsünüz: **veri**, **eğitim**, **Test**ve **dağıtım**. Her bir sekmeyi nasıl kullanacağınızı öğrenmek için [sonraki adımlarda](#next-steps) sunulan bağlantıları kullanın.

> [!IMPORTANT]
> [Özel ses portalı](https://aka.ms/custom-voice) yakın zamanda güncelleştirildi! CRIS.ai portalında veya API 'lerle önceki veriler, modeller, testler ve yayımlanmış uç noktalar oluşturduysanız, bu eski varlıklara bağlanmak için yeni portalda yeni bir proje oluşturmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel ses verileri hazırlama](how-to-custom-voice-prepare-data.md)
- [Özel bir ses oluşturun](how-to-custom-voice-create-voice.md)
- [Kılavuz: ses örneklerinizi kaydetme](record-custom-voice-samples.md)
