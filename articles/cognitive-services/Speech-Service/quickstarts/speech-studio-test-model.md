---
title: 'Hızlı başlangıç: ses dosyalarını kullanarak bir modeli test etme-konuşma Studio'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Ses dosyasında konuşmayı tanımayı test etmek için konuşma Studio 'Yu kullanacaksınız.
services: cognitive-services
author: v-demjoh
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 02/10/2020
ms.author: v-demjoh
ms.openlocfilehash: d696c0f6d1cdab8175af77c265435c48824f3e15
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84346117"
---
# <a name="quickstart-test-a-model-using-an-audio-file-in-speech-studio"></a>Hızlı başlangıç: konuşma Studio 'da bir ses dosyası kullanarak bir modeli test etme

Bu hızlı başlangıçta, konuşmayı bir ses dosyasından metne dönüştürmek için konuşma Studio 'Yu kullanırsınız. Konuşma Studio, ilgili metin, insan etiketli yazılı betikler ile ses ve sağladığınız Söyleniş kılavuzunuzu kullanarak konuşma tanıma modellerini test etmenize, karşılaştırmanıza, iyileştirmenize ve dağıtmanıza olanak tanır.

## <a name="prerequisites"></a>Önkoşullar

Konuşma portalını kullanmadan önce, [bir Azure hesabı oluşturmak ve konuşma hizmetine abone olmak için bu yönergeleri izleyin](../how-to-custom-speech.md#set-up-your-azure-account). Bu Birleşik abonelik, konuşmadan metne, metinden konuşmaya, konuşma çevirisine ve Özel Konuşma Tanıma portalına erişmenizi sağlar.

## <a name="download-an-audio-file"></a>Ses dosyası indirme

Konuşma içeren bir ses dosyasını indirmek ve bir ZIP dosyasına paketlemek için bu adımları izleyin.

1. Bağlantıyı sağ tıklayıp **Bağlantıyı farklı kaydet**' i seçerek **[Bu bağlantıdan örnek wav dosyasını](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-speech-sdk/f9807b1079f3a85f07cbb6d762c6b5449d536027/samples/cpp/windows/console/samples/whatstheweatherlike.wav)** indirin. Dosyayı indirmek için **Kaydet** ' e tıklayın `whatstheweatherlike.wav` .
2. Bir dosya Gezgini veya bir zip aracıyla Terminal penceresi kullanarak, `whatstheweatherlike.zip` indirdiğiniz dosyayı içeren adlı bir zip dosyası oluşturun `whatstheweatherlike.wav` . Windows Gezgini ' nde, Windows Gezgini 'ni açabilir, klasöre gidebilir, `Downloads` `whatstheweatherliike.wav` **Gönder ' e**tıklayın, **Sıkıştırılmış (daraltılmış) klasöre**ve ardından ENTER tuşuna basarak varsayılan dosya adını kabul edebilirsiniz.

## <a name="create-a-project-in-the-custom-speech-portal"></a>Özel Konuşma Tanıma portalında proje oluşturma

Bir ses dosyasının zip alanınızı içeren bir proje oluşturmak için bu adımları izleyin.

1. [Konuşma Studio 'yu](https://speech.microsoft.com/)açın ve **Yeni proje**' ye tıklayın. Bu proje için bir ad yazın ve **Oluştur**' a tıklayın. Projeniz Özel Konuşma Tanıma listesinde görüntülenir.
2. Projenizin adına tıklayın. Veri sekmesinde **verileri karşıya yükle**' ye tıklayın.
3. Konuşma veri türü varsayılan olarak **yalnızca ses olarak kullanılabilir**, bu nedenle **İleri**' ye tıklayın.
4. Yeni konuşma veri kümenizi adlandırın `MyZipOfAudio` ve **İleri**' ye tıklayın.
5. **Dosyalara gözatamıyorum...**' a tıklayın, `whatstheweatherlike.zip` dosyanıza gidin ve **Aç**' a tıklayın.
6. **Karşıya Yükle** düğmesine tıklayın. Tarayıcı, ZIP dosyanızı konuşma Studio 'ya yükler ve konuşma Studio içeriği işler.

## <a name="test-a-model"></a>Modeli test etme

Konuşma Studio, ZIP dosyanızın içeriğini tamamladıktan sonra, hataları veya omissions aramak için dökümü incelerken kaynak sesi oynatabilirsiniz. Tarayıcıda Döküm kalitesini incelemek için bu adımları izleyin.

1. **Test** sekmesine tıklayın ve **Test Ekle**' ye tıklayın.
2. Bu testte yalnızca ses verileri kalitesini inceliyor ve bu test türünü kabul etmek için **İleri** ' ye tıklayın.
3. Bu testi adlandırın `MyModelTest` ve **İleri**' ye tıklayın.
4. Sol tarafta bulunan radyo düğmesine tıklayın `MyZipOfAudio` ve **İleri**' ye tıklayın.
5. **Model 1** açılan menüsü varsayılan olarak en son tanıma modelini alır, bu nedenle **Oluştur**' a tıklayın. Ses veri kümenizin içeriğini işledikten sonra, test durumu **başarılı**olarak değişir.
6. **Mymodeltest**öğesine tıklayın. Konuşma tanımanın sonuçları görüntülenir. Sesi dinlemek için dairenin içindeki sağ taraftaki üçgeni tıklatın ve metin ile dairenin ne kadar duydudığını karşılaştırın.

## <a name="download-detailed-results"></a>Ayrıntılı sonuçları indirin

Daha ayrıntılı bilgi için, bu ayrıntıları tanımlayan dosyaları indirebilirsiniz. Dosyalar, ses dosyalarınıza ait sözcük temelli konuşma ve her sözcük hakkında konum, süre ve döküm güvenilirliği bilgilerini içeren JSON dosyalarını içerir. Bu dosyaları görmek için aşağıdaki adımları izleyin.

1. **İndir**'e tıklayın.
2. Indir iletişim kutusunda **Ses**seçimini kaldırın ve **İndir**' e tıklayın.
3. İndirilen ZIP dosyasını sıkıştırmayı açın ve ayıklanan dosyaları inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Özel bir modeli eğitmek](../how-to-custom-speech-test-and-train.md)için konuşma tanımanın doğruluğunu artırma hakkında bilgi edinin.