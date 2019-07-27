---
title: Özel ses nedir? -Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Özel ses, markanız için tanınabilir, tek bir tür ses oluşturmanıza olanak sağlayan bir dizi çevrimiçi araç. Kullanmaya başlamak her şey için çok sayıda ses dosyası ve ilişkili dökümlerdir. Özel bir konuşmaya metin deneyimi oluşturmaya başlamak için aşağıdaki bağlantıları izleyin.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 10d76bc1dd52f04cceb9f0952a755c55d90c6896
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562793"
---
# <a name="get-started-with-custom-voice"></a>Özel Ses’i kullanmaya başlama

Özel ses, markanız için tanınabilir, tek bir tür ses oluşturmanıza olanak sağlayan bir dizi çevrimiçi araç. Kullanmaya başlamak her şey için çok sayıda ses dosyası ve ilişkili dökümlerdir. Özel bir metin okuma deneyimi oluşturmaya başlamak için aşağıdaki bağlantıları izleyin.

## <a name="whats-in-custom-voice"></a>Özel ses nedir?

Özel sesle başlamadan önce bir Azure hesabı ve bir konuşma Hizmetleri aboneliğine sahip olmanız gerekir. Bir hesap oluşturduktan sonra verilerinizi hazırlayabilir, modellerinizi eğitmenizi, test edebilir, ses kalitesini değerlendirebilir ve sonunda özel ses modelinizi dağıtabilirsiniz.

Aşağıdaki diyagramda özel ses portalını kullanarak özel bir ses modeli oluşturma adımları vurgulanmaktadır. Daha fazla bilgi edinmek için bağlantıları kullanın.

![Özel ses mimarisi diyagramı](media/custom-voice/custom-voice-diagram.png)

1.  [Abone olun ve proje oluşturun](#set-up-your-azure-account) -Azure hesabı oluşturun ve bir konuşma Hizmetleri aboneliği oluşturun. Bu Birleşik abonelik, konuşmadan metne, metinden konuşmaya, konuşma çevirisine ve özel ses portalına erişmenizi sağlar. Ardından, konuşma Hizmetleri aboneliğinizi kullanarak ilk özel ses projenizi oluşturun.

2.  [Karşıya veri yükleme](how-to-custom-voice-create-voice.md#upload-your-datasets) -özel ses portalı veya özel ses API 'sini kullanarak verileri (ses ve metin) karşıya yükleyin. Portalda, Söyleniş puanlarını ve sinyal-gürültü oranlarını araştırabilir ve değerlendirebilirsiniz. Daha fazla bilgi için bkz. [özel ses için verileri hazırlama](how-to-custom-voice-prepare-data.md).

3.  [Modelinize eğitme](how-to-custom-voice-create-voice.md#build-your-custom-voice-model) : verilerinizi kullanarak özel bir metinden konuşmaya ses modeli oluşturun. Modeli farklı dillerde eğitebilirsiniz. Eğitim sonrasında modelinizi test edin ve sonuçtan memnun kaldıysanız modeli dağıtabilirsiniz.

4.  [Modelinizi dağıtın](how-to-custom-voice-create-voice.md#create-and-use-a-custom-voice-endpoint) -metinden konuşmaya ses modeliniz için özel bir uç nokta oluşturun ve bunu, ürün, araç ve uygulamalarınızda konuşma sensimi için kullanın.

## <a name="set-up-your-azure-account"></a>Azure hesabınızı ayarlama

Özel bir model oluşturmak için Özel Konuşma Tanıma portalını kullanabilmeniz için bir konuşma Hizmetleri aboneliği gerekir. Azure 'da bir konuşma Hizmetleri aboneliği oluşturmak için bu yönergeleri izleyin. Bir Azure hesabınız yoksa, yeni bir hesap için kaydolabilirsiniz.  

Bir Azure hesabı ve bir konuşma Hizmetleri aboneliği oluşturduktan sonra, özel sesli portalda oturum açmanız ve aboneliğinizi bağlamanız gerekir.

1. Azure portal konuşma Hizmetleri abonelik anahtarınızı alın.
2. [Özel ses portalında](https://aka.ms/custom-voice)oturum açın.
3. Aboneliğinizi seçin ve bir konuşma projesi oluşturun.
4. Başka bir konuşma aboneliğine geçiş yapmak isterseniz, üstteki gezinmede bulunan dişli simgesini kullanın.

> [!NOTE]
> Özel ses hizmeti 30 günlük ücretsiz deneme anahtarını desteklemez. Hizmeti kullanabilmeniz için önce Azure 'da oluşturulmuş bir F0 veya S0 anahtarınız olmalıdır.

## <a name="how-to-create-a-project"></a>Proje oluşturma

Veriler, modeller, testler ve uç noktalar gibi içerikler, özel ses portalındaki **Projeler** halinde düzenlenmiştir. Her proje, oluşturmak istediğiniz sesin ülke/dil ve cinsiyetine özgüdür. Örneğin, Birleşik Devletler (en-US) içinde Ingilizce kullanan çağrı merkezinizin sohbet botları için kadın sesi için bir proje oluşturabilirsiniz.

İlk projenizi oluşturmak için, **metinden konuşmaya/özel ses** sekmesini seçin ve ardından **Yeni proje**' ye tıklayın. Projenizi oluşturmak için sihirbaz tarafından sunulan yönergeleri izleyin. Projeyi oluşturduktan sonra dört sekme görürsünüz: **Veri**, **eğitim**, **Test**ve **dağıtım**. Her bir sekmeyi nasıl kullanacağınızı öğrenmek için [sonraki adımlarda](#next-steps) sunulan bağlantıları kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel ses verileri hazırlama](how-to-custom-voice.md)
- [Özel bir ses oluşturun](how-to-custom-voice-create-voice.md)
- [Rehberi Ses örneklerinizi kaydedin](record-custom-voice-samples.md)
