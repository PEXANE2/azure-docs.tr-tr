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
ms.openlocfilehash: e48dfd224a9656c7d8327dd77f1b55e9a744f3af
ms.sourcegitcommit: 65cef6e5d7c2827cf1194451c8f26a3458bc310a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2021
ms.locfileid: "98573097"
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

Özel ses Şu anda hem standart hem de sinir katmanlarını desteklemektedir. Özel sinir ses, kullanıcıların daha az veri gerektirmesi sırasında daha yüksek kaliteli ses modelleri oluşturmasını ve yapay zeka dağıtmanıza yardımcı olan ölçüler sağladığını sağlar. Daha doğal konuşma arabirimleri için daha gerçekçi sesler geliştirmek ve müşterilerinizin ve son kullanıcılarınızın, en son metin okuma teknolojisinden sorumlu bir şekilde faydalanmasını sağlamak için özel sinir Voice kullanmanız önerilir. [Özel sinir sesi hakkında daha fazla bilgi edinin](https://aka.ms/CNV-Transparency-Note). 

> [!NOTE]
> Microsoft 'un sorumlu AI tasarlama taahhüdünün bir parçası olarak özel sinir Voice kullanımını sınırlandırdık. Teknolojiye yalnızca uygulamalarınız incelendikten ve sorumlu AI ilkelerimize göre hizalama olarak kullanmayı taahhüt ettikten sonra erişebilirsiniz. [Erişim sınırı](https://aka.ms/gating-overview) hakkında daha fazla bilgi edinin ve [burada uygulayın](https://aka.ms/customneural). Özel sesin standart ve sinir sürümü için desteklenen [Diller](language-support.md#customization) ve [bölgeler](regions.md#custom-voices) farklıdır. Başlamadan önce ayrıntıları denetleyin.  

## <a name="set-up-your-azure-account"></a>Azure hesabınızı ayarlama

Özel bir model oluşturmak için Özel Konuşma Tanıma portalını kullanabilmeniz için bir konuşma hizmeti aboneliği gerekir. Azure 'da bir konuşma hizmeti aboneliği oluşturmak için bu yönergeleri izleyin. Bir Azure hesabınız yoksa, yeni bir hesap için kaydolabilirsiniz.  

Bir Azure hesabı ve bir konuşma hizmeti aboneliği oluşturduktan sonra, özel sesli portalda oturum açmanız ve aboneliğinizi bağlamanız gerekir.

1. Azure portal konuşma hizmeti abonelik anahtarınızı alın.
2. [Özel ses portalında](https://aka.ms/custom-voice)oturum açın.
3. Aboneliğinizi seçin ve bir konuşma projesi oluşturun.
4. Başka bir konuşma aboneliğine geçiş yapmak isterseniz, üstteki gezinmede bulunan dişli simgesini kullanın.

> [!NOTE]
> Hizmeti kullanabilmeniz için önce Azure 'da oluşturulmuş bir F0 veya S0 konuşma hizmeti anahtarınız olmalıdır. Özel sinir Voice yalnızca S0 katmanını destekler. 

## <a name="how-to-create-a-project"></a>Proje oluşturma

Veriler, modeller, testler ve uç noktalar gibi içerikler, özel ses portalındaki **Projeler** halinde düzenlenmiştir. Her proje, oluşturmak istediğiniz sesin ülke/dil ve cinsiyetine özgüdür. Örneğin, Birleşik Devletler (en-US) içinde Ingilizce kullanan çağrı merkezinizin sohbet botları için kadın sesi için bir proje oluşturabilirsiniz.

İlk projenizi oluşturmak için, **metinden konuşmaya/özel ses** sekmesini seçin ve ardından **Yeni proje**' ye tıklayın. Projenizi oluşturmak için sihirbaz tarafından sunulan yönergeleri izleyin. Projeyi oluşturduktan sonra dört sekme görürsünüz: **veri**, **eğitim**, **Test** ve **dağıtım**. Her bir sekmeyi nasıl kullanacağınızı öğrenmek için [sonraki adımlarda](#next-steps) sunulan bağlantıları kullanın.

> [!IMPORTANT]
> [Özel ses portalı](https://aka.ms/custom-voice) yakın zamanda güncelleştirildi! CRIS.ai portalında veya API 'lerle önceki veriler, modeller, testler ve yayımlanmış uç noktalar oluşturduysanız, bu eski varlıklara bağlanmak için yeni portalda yeni bir proje oluşturmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel ses verileri hazırlama](how-to-custom-voice-prepare-data.md)
- [Özel bir ses oluşturun](how-to-custom-voice-create-voice.md)
- [Kılavuz: ses örneklerinizi kaydetme](record-custom-voice-samples.md)
