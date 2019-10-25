---
title: 'Hızlı başlangıç: Sentezleştirme konuşma, Unity-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Unity ile bir metin okuma uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde, konuşmanızı gerçek zamanlı olarak cihazınızın hoparlörünü senkize ekleyebilirsiniz.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 9/19/2019
ms.author: yinhew
ms.openlocfilehash: b4d329d9d3c2a259fb90b0278c54ba8590590995
ms.sourcegitcommit: 9a4296c56beca63430fcc8f92e453b2ab068cc62
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2019
ms.locfileid: "72675494"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-unity"></a>Hızlı başlangıç: Unity için konuşma SDK 'Sı ile konuşmayı Sentezleştirme

Hızlı başlangıç, [konuşma tanıma](quickstart-csharp-unity.md)için de kullanılabilir.

[Unity](https://unity3d.com/) 'Yi ve Unity için Azure bilişsel hizmetler konuşma SDK 'sını kullanarak bir metin okuma uygulaması oluşturmak için bu kılavuzu kullanın.
İşiniz bittiğinde, konuşmanızı gerçek zamanlı olarak cihazınızın hoparlörünü senkize ekleyebilirsiniz.
Unity 'yi tanımıyorsanız, uygulama geliştirmeye başlamadan önce [Unity Kullanıcı el ile](https://docs.unity3d.com/Manual/UnityManual.html) çalışın.

> [!NOTE]
> Unity, Windows Masaüstü (x86 ve x64) veya Evrensel Windows Platformu (x86, x64, ARM/ARM64), Android (x86, ARM32/64) ve iOS (x64 simülatör, ARM32 ve ARM64) destekler.

## <a name="prerequisites"></a>Önkoşullar

Bu projeyi tamamlamak için şunlar gerekir:

* Unity [2018,3 veya üzeri](https://store.unity.com/) Unity [2019,1, UWP ARM64 için destek ekleme](https://blogs.unity3d.com/2019/04/16/introducing-unity-2019-1/#universal).
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/). Visual Studio 2017 sürüm 15,9 veya üzeri sürümleri de kabul edilebilir.
* Windows ARM64 desteği için, [ARM64 için isteğe bağlı derleme araçlarını ve ARM64 Için Windows 10 SDK 'yı](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/)yüklemelisiniz.
* Konuşma hizmeti için bir abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).

## <a name="create-a-unity-project"></a>Unity projesi oluşturma

* Unity 'yi başlatın ve **Projeler** sekmesinde **Yeni**' yi seçin.
* **Proje adını** **CSharp-Unity** ve **şablon** **olarak belirtin**ve bir konum seçin.
  Ardından **proje oluştur**' u seçin.
* Biraz zaman sonra Unity Düzenleyicisi penceresi açılır.

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

[!INCLUDE [License notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

* Unity için konuşma SDK 'Sı (Beta), Unity varlık paketi (. unitypackage) olarak paketlenmiştir. [Bu Web sitesinden](https://aka.ms/csspeech/unitypackage)indirin.
* **Varlık** > **paketi Içeri aktar** > **özel paket**' i seçerek konuşma SDK 'sını içeri aktarın. Daha fazla bilgi için [Unity belgelerine](https://docs.unity3d.com/Manual/AssetPackages.html)bakın.
* Dosya seçicisinden indirdiğiniz konuşma SDK. unitypackage dosyasını seçin.
* Tüm dosyaların seçili olduğundan emin olun ve **Içeri aktar**' ı seçin.

  ![Konuşma SDK Unity varlık paketi içeri aktarılırken Unity düzenleyicisinin ekran görüntüsü](media/sdk/qs-csharp-unity-01-import.png)

## <a name="add-a-ui"></a>Kullanıcı arabirimi ekleme

Sentiğimize metin girmek için bir giriş alanından oluşan, konuşma sentiğinin tetiklenmesi için bir düğme ve sonucu görüntüleyen bir metin alanı olan sahneye en az bir kullanıcı arabirimi ekleyeceğiz.

* [Hiyerarşi penceresinde](https://docs.unity3d.com/Manual/Hierarchy.html) (varsayılan olarak sol tarafta), Unity 'nin yeni projeyle oluşturulduğu örnek bir sahne gösterilmektedir.
* **Hiyerarşi** penceresinin en üstündeki **Oluştur** düğmesini seçin ve **Kullanıcı arabirimi** > **giriş alanı**' nı seçin.
* Bu seçenek, **hiyerarşi** penceresinde görebileceğiniz üç oyun nesnesini oluşturur: bir **tuval** nesnesi içinde Iç Içe geçmiş bir **giriş alanı** nesnesi ve bir **EventSystem** nesnesi.
* Sahnenin ve [sahne görünümündeki](https://docs.unity3d.com/Manual/UsingTheSceneView.html)giriş alanının iyi bir görünümünü görmek [Için sahne görünümünde gezinin](https://docs.unity3d.com/Manual/SceneViewNavigation.html) .
* Ayarlarını [Inspector penceresinde](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan olarak, sağ tarafta) göstermek için **hiyerarşi** penceresinde **giriş alanı** nesnesini seçin.
* Giriş alanının tuvalin ortasında ortalanmasını sağlamak için **POS X** ve **POS Y** özelliklerini **0** olarak ayarlayın.
* **Hiyerarşi** penceresinin en üstündeki **Oluştur** düğmesini yeniden seçin. Düğme oluşturmak için **uı** > **düğmesini** seçin.
* Ayarlarını [Inspector penceresinde](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan olarak, sağ tarafta) göstermek için **hiyerarşi** penceresinde **düğme** nesnesini seçin.
* **POS X** ve **POS Y** özelliklerini **0** ve **-48**olarak ayarlayın. Düğmenin ve giriş alanının çakışmadığından emin olmak için **Genişlik** ve **Yükseklik** özelliklerini **160** ve **30** olarak ayarlayın.
* **Hiyerarşi** penceresinin en üstündeki **Oluştur** düğmesini yeniden seçin. Bir metin alanı oluşturmak için **Kullanıcı arabirimi** > **metin** ' i seçin.
* Ayarlarını [Inspector penceresinde](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan olarak, sağ tarafta) göstermek için **hiyerarşi** penceresinde **metin** nesnesini seçin.
* **POS X** ve **POS Y** özelliklerini **0** ve **80**olarak ayarlayın. Metin alanının ve giriş alanının çakışmadığından emin olmak için **Genişlik** ve **Yükseklik** özelliklerini **320** ve **80** olarak ayarlayın.
* **Hiyerarşi** penceresinin en üstündeki **Oluştur** düğmesini yeniden seçin. Ses **kaynağı** oluşturmak için ses > **ses kaynağı** ' nı seçin.

İşiniz bittiğinde, Kullanıcı arabirimi şu ekran görüntüsüne benzer şekilde görünmelidir:

[Unity düzenleyicisinde hızlı başlangıç Kullanıcı arabiriminin ekran görüntüsünü![](media/sdk/qs-tts-csharp-unity-ui-inline.png)](media/sdk/qs-tts-csharp-unity-ui-expanded.png#lightbox)

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. [Proje penceresinde](https://docs.unity3d.com/Manual/ProjectView.html) (varsayılan olarak sol alt tarafta) **Oluştur** düğmesini seçin ve ardından  **C# komut dosyası**' nı seçin. Betiği `HelloWorld`olarak adlandırın.

1. Betiği çift tıklayarak düzenleyin.

   > [!NOTE]
   >  > **tercihlerini** **Düzenle** seçeneğini belirleyerek hangi kod düzenleyicisinin başlatılmış olduğunu yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Unity Kullanıcı el kitabı](https://docs.unity3d.com/Manual/Preferences.html).

1. Tüm kodu aşağıdaki kodla değiştirin:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-unity/Assets/Scripts/HelloWorld.cs#code)]

1. `YourSubscriptionKey` dize, konuşma Hizmetleri abonelik anahtarınızla bulun ve değiştirin.

1. `YourServiceRegion` dizesini bulun ve aboneliğinizle ilişkili [bölge](regions.md) ile değiştirin. Örneğin, ücretsiz deneme sürümünü kullanıyorsanız bölge `westus`.

1. Değişiklikleri betikte kaydedin.

1. Unity düzenleyicisine geri döndüğünüzde, betiği oyun nesnelerinizin birine bileşen olarak ekleyin.

   * Ayarla penceresinde ayarı açmak için **hiyerarşi** penceresinde **tuval** nesnesini [seçin (varsayılan](https://docs.unity3d.com/Manual/UsingTheInspector.html) olarak, sağ tarafta).
   * **Inspector** penceresindeki **Bileşen Ekle** düğmesini seçin. Daha önce oluşturduğumuz `HelloWorld` betiği arayın ve ekleyin.
   * HelloWorld bileşeni, `HelloWorld` sınıfının ortak özellikleriyle eşleşen dört başlatılmamış özellik, **çıkış metni**, **giriş alanı**, **konuşma düğmesi** ve **Ses kaynağıdır**.
     Bunları bağlamak için nesne seçiciyi (özelliğin sağ tarafındaki küçük daire simgesini) seçin. Daha önce oluşturduğunuz metin ve düğme nesnelerini seçin.

     > [!NOTE]
     > Giriş alanı ve düğme de iç içe metin nesnesine sahiptir. Yanlışlıkla metin çıktısı için seçim yaptığınızdan emin olun. Ya da bu karışıklığın önüne geçmek için **Inspector** penceresindeki **ad** alanını kullanan metin nesnelerini yeniden adlandırabilirsiniz.

## <a name="run-the-application-in-the-unity-editor"></a>Uygulamayı Unity düzenleyicisinde çalıştırma

* Menü çubuğunun altındaki Unity Düzenleyici araç çubuğunda **oynat** düğmesini seçin.
* Uygulama başlatıldıktan sonra, giriş alanına bir metin girin ve düğmeyi seçin. Metniniz, konuşmacının oynadığı konuşma hizmetine iletilir ve konuşmaya sentezleştirilmiştir.

  [Unity oyun penceresinde çalışan hızlı başlangıç ekranının ekran görüntüsünü![](media/sdk/qs-tts-csharp-unity-output-inline.png)](media/sdk/qs-tts-csharp-unity-output-expanded.png#lightbox)

* Hata ayıklama iletileri için [konsol penceresini](https://docs.unity3d.com/Manual/Console.html) denetleyin.
* Konuşmayı senizle tamamladıktan sonra, uygulamayı durdurmak için Unity Düzenleyici araç çubuğundaki **oynat** düğmesini seçin.

## <a name="additional-options-to-run-this-application"></a>Bu uygulamayı çalıştırmak için ek seçenekler

Bu uygulama, Windows tek başına uygulama veya UWP uygulaması olarak Android 'e de dağıtılabilir.
Bu ek hedeflerin yapılandırmasını açıklayan hızlı başlangıç/CSharp-Unity klasöründeki [örnek depoya](https://aka.ms/csspeech/samples) bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub C# 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Ses yazı tiplerini özelleştirme](how-to-customize-voice-font.md)
- [Ses örneklerini Kaydet](record-custom-voice-samples.md)
