---
title: 'Quickstart: Synthesize konuşma, C# (Birlik) - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Unity ve Unity için Speech SDK ile metin den konuşmaya bir uygulama oluşturmak için bu kılavuzu kullanın. Bittiğinde, metni gerçek zamanlı olarak cihazınızın hoparlörüne sentezleyebilirsiniz.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/04/2020
ms.author: yinhew
ms.openlocfilehash: 0934738c557ac6d26867546783797cf5d2b75056
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81274845"
---
> [!NOTE]
> Unity, Windows Desktop (x86 ve x64) veya Evrensel Windows Platformu (x86, x64, ARM/ARM64), Android (x86, ARM32/64) ve iOS 'u (x64 simülatörü, ARM32 ve ARM64) destekler.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları unutmayın:

> [!div class="checklist"]
> * [Azure Konuşma Kaynağı Oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=unity&pivots=programming-language-csharp)

## <a name="add-a-ui"></a>UI ekleme

Sahnemize sentez için metin girmek için bir giriş alanından, konuşma sentezini tetikleyen bir düğmeden ve sonucu görüntülemek için bir metin alanından oluşan en az kullanıcı arabirimi ekliyoruz.

* Hiyerarşi [penceresinde](https://docs.unity3d.com/Manual/Hierarchy.html) (varsayılan olarak solda), yeni projeyle oluşturulan Birleşim' in bir çeşitli sahnesi gÜ
* **Hiyerarşi** penceresinin üst kısmındaki **Oluştur** düğmesini seçin ve **UI** > **Giriş Alanı'nı**seçin.
* Bu **seçenek, Hiyerarşi** penceresinde görebileceğiniz üç oyun nesnesi oluşturur: **Bir Kanvas** nesnesi içinde iç içe bir **Giriş Alanı** nesnesi ve **Bir EventSystem** nesnesi.
* Tuvalin ve Giriş alanının [Sahne görünümünde](https://docs.unity3d.com/Manual/UsingTheSceneView.html)iyi bir görünümüne sahip olmak için [Sahne görünümünde gezinin.](https://docs.unity3d.com/Manual/SceneViewNavigation.html)
* Ayarlarını [Denetçi penceresinde](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan olarak sağtarafta) gününde günügünü almak için **Hiyerarşi** **penceresindeIn Input Field** nesnesini seçin.
* Pos **X** ve **Pos Y** özelliklerini **0** olarak ayarlayın, böylece giriş alanı tuvalin ortasında ortalanır.
* **Hiyerarşi** penceresinin üst kısmındaki **Oluştur** düğmesini yeniden seçin. Bir düğme oluşturmak için **UI** > **düğmesini** seçin.
* Ayarlarını [Denetçi penceresinde](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan olarak sağtarafta) gününde günüalmak için **Hiyerarşi** **penceresindeKi Düğme** nesnesini seçin.
* Pos **X** ve **Pos Y** özelliklerini **0** ve **-48**olarak ayarlayın. Düğme nin ve giriş alanının çakışmaması için **Genişlik** ve **Yükseklik** özelliklerini **160** ve **30** olarak ayarlayın.
* **Hiyerarşi** penceresinin üst kısmındaki **Oluştur** düğmesini yeniden seçin. Metin alanı oluşturmak için **Kullanıcı** > Arası**Arama Metni'ni** seçin.
* Ayarlarını [Denetçi penceresinde](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan olarak sağtarafta) gününde günüalmak için **Hiyerarşi** penceresindeKi **Metin** nesnesini seçin.
* Pos **X** ve **Pos Y** özelliklerini **0** ve **80**olarak ayarlayın. Metin alanı nın ve giriş alanının çakışmaması için **Genişlik** ve **Yükseklik** özelliklerini **320** ve **80** olarak ayarlayın.
* **Hiyerarşi** penceresinin üst kısmındaki **Oluştur** düğmesini yeniden seçin. Ses kaynağı oluşturmak için **Ses** > **Kaynağı'nı** seçin.

Bittiğinde, UI bu ekran görüntüsüne benzer görünmelidir:

[![Unity Editor'daki hızlı başlangıç kullanıcı arabiriminin ekran görüntüsü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-csharp-unity-ui-inline.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-csharp-unity-ui-expanded.png#lightbox)

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. Proje [penceresinde](https://docs.unity3d.com/Manual/ProjectView.html) (varsayılan olarak sol altta), **Oluştur** düğmesini seçin ve ardından **C# komut dosyasını**seçin. Komut dosyasına `HelloWorld`isim .

1. Komut dosyasını çift tıklatarak düzenleme.

   > [!NOTE]
   > **Tercihleri** **Düzenley'i** > seçerek hangi kod düzenleyicinin başlatıldığını yapılandırabilirsiniz. Daha fazla bilgi için [Birlik Kullanım Kılavuzu'na](https://docs.unity3d.com/Manual/Preferences.html)bakın.

1. Tüm kodu aşağıdakilerle değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/unity/text-to-speech/Assets/Scripts/HelloWorld.cs#code)]

1. Dizeyi `YourSubscriptionKey` bulun ve Konuşma hizmeti abonelik anahtarınızla değiştirin.

1. `YourServiceRegion` dizesini bulun ve aboneliğinizle ilişkili [bölge](~/articles/cognitive-services/Speech-Service/regions.md) ile değiştirin. Örneğin, ücretsiz deneme `westus` kullanırsanız bölgedir.

1. Değişiklikleri komut dosyasına kaydedin.

1. Unity Editor'a geri döndüğünüzde, komut dosyasını oyun nesnelerinizden birine bileşen olarak ekleyin.

   * [Denetmen penceresindeki](https://docs.unity3d.com/Manual/UsingTheInspector.html) ayarı açmak için **Hiyerarşi** penceresindeki **Tuval** nesnesini seçin (varsayılan olarak sağda).
   * **Denetçi** penceresinde **Bileşen Ekle** düğmesini seçin. Daha sonra `HelloWorld` daha önce oluşturduğumuz komut dosyasını arayın ve ekleyin.
   * HelloWorld bileşeni, `HelloWorld` sınıfın ortak özellikleriyle eşleşen dört adet çıkış **metni,** Giriş **Alanı,** Konuşma **Düğmesi** ve Ses **Kaynağı'na**sahiptir.
     Onları kablolamak için Nesne Seçici'yi (özelliğin sağındaki küçük daire simgesi) seçin. Daha önce oluşturduğunuz metin ve düğme nesnelerini seçin.

     > [!NOTE]
     > Giriş alanı ve düğmesi de iç içe bir metin nesnesi var. Metin çıktısı için yanlışlıkla seçmediğinden emin olun. Veya, bu karışıklığı önlemek için **Denetçi** penceresindeKi **Ad** alanını kullanan metin nesnelerini yeniden adlandırabilirsiniz.

## <a name="run-the-application-in-the-unity-editor"></a>Birliğin Düzenleyicisi'nde uygulamayı çalıştırın

* Menü çubuğunun altındaki Unity Editor araç çubuğunda **Oynat** düğmesini seçin.
* Uygulama kullanıma açıldıktan sonra giriş alanına bir metin girin ve düğmeyi seçin. Metniniz Konuşma hizmetine aktarılır ve hoparlörünüzün üzerinde çalan konuşmayla sentezlenir.

  [![Unity Game penceresinde çalışan hızlı başlatma ekran görüntüsü](~/articles/cognitive-services/speech-service/media/sdk/qs-tts-csharp-unity-output-inline.png)](~/articles/cognitive-services/speech-service/media/sdk/qs-tts-csharp-unity-output-expanded.png#lightbox)

* Hata ayıklama iletileri için [Konsol penceresinden](https://docs.unity3d.com/Manual/Console.html) denetleyin.

## <a name="additional-options-to-run-this-application"></a>Bu uygulamayı çalıştırmak için ek seçenekler

Bu uygulama, Windows tek başına bir uygulama veya UWP uygulaması olarak Android'e de dağıtılabilir.
Bu ek hedeflerin yapılandırmasını açıklayan quickstart/csharp-unity klasöründeki [örnek depoya](https://aka.ms/csspeech/samples) bakın.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Özel Ses Oluşturma](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Özel ses örneklerini kaydetme](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
