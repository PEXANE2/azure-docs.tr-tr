---
title: 'Hızlı Başlangıç: Sentezleştirme, Unity konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Unity ve Unity için konuşma SDK 'Sı (Beta) ile bir metin okuma uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde, konuşmanızı gerçek zamanlı olarak cihazınızın hoparlörünü senkize ekleyebilirsiniz.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 6/26/2019
ms.author: yinhew
ms.openlocfilehash: 507ab9ef9bb3e482e5a33d2406424dfb9116de54
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68553623"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-unity-beta"></a>Hızlı Başlangıç: Unity için konuşma SDK 'Sı (Beta) ile konuşmayı sentezleştirme

Hızlı başlangıç, [konuşma tanıma](quickstart-csharp-unity.md)için de kullanılabilir.

[Unity](https://unity3d.com/) 'Yi ve Unity için konuşma SDK 'Sını (Beta) kullanarak bir metin okuma uygulaması oluşturmak için bu kılavuzu kullanın.
İşiniz bittiğinde, konuşmanızı gerçek zamanlı olarak cihazınızın hoparlörünü senkize ekleyebilirsiniz.
Unity 'yi bilmiyorsanız, uygulama geliştirmeye başlamadan önce [Unity Kullanıcı el ile](https://docs.unity3d.com/Manual/UnityManual.html) çalışmanız önerilir.

> [!NOTE]
> Unity için konuşma SDK 'Sı Şu anda beta aşamasındadır.
> Windows Masaüstü (x86 ve x64) veya Evrensel Windows Platformu (x86, x64, ARM/ARM64) ve Android (x86, ARM32/64) destekler.

## <a name="prerequisites"></a>Önkoşullar

Bu projeyi tamamlamak için şunlar gerekir:

* Unity [2018,3 veya üzeri](https://store.unity.com/) UNITY [2019,1 UWP ARM64 için destek ekleme](https://blogs.unity3d.com/2019/04/16/introducing-unity-2019-1/#universal)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
     * ARM64 desteği için, [ARM64 için isteğe bağlı derleme araçlarını ve ARM64 Için Windows 10 SDK 'sını](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/) yükler 
* Konuşma hizmeti için bir abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).

## <a name="create-a-unity-project"></a>Unity projesi oluşturma

* Unity 'yi başlatın ve **Projeler** sekmesinde **Yeni**' yi seçin.
* **Proje adını** **CSharp-Unity**, **şablon** olarak **belirtin ve bir** konum seçin.
  Ardından **proje oluştur**' u seçin.
* Biraz zaman sonra Unity Düzenleyicisi penceresi açılır.

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

* Unity için konuşma SDK 'Sı (Beta), Unity varlık paketi (. unitypackage) olarak paketlenmiştir.
  İndirdiği [burada](https://aka.ms/csspeech/unitypackage).
* **Varlık** > **içeri aktarma paket** > **özel paketini**seçerek konuşma SDK 'sını içeri aktarın.
  Ayrıntılar için [Unity belgelerine](https://docs.unity3d.com/Manual/AssetPackages.html) göz atın.
* Dosya seçicisinde, yukarıda indirdiğiniz konuşma SDK. unitypackage dosyasını seçin.
* Tüm dosyaların seçili olduğundan emin olun ve **Içeri aktar**' a tıklayın:

  ![Konuşma SDK Unity varlık paketi içeri aktarılırken Unity düzenleyicisinin ekran görüntüsü](media/sdk/qs-csharp-unity-01-import.png)

## <a name="add-ui"></a>UI Ekle

Sentiğimize en az bir kullanıcı arabirimi ekledik, sencıya yönelik metni girmek için bir giriş alanından, konuşma sentiğini tetiklemeye yönelik bir düğme ve sonucu görüntüleyen bir metin alanı.

* [Hiyerarşi penceresinde](https://docs.unity3d.com/Manual/Hierarchy.html) (varsayılan olarak sol tarafta), Unity 'nin yeni projeyle oluşturulduğu örnek bir sahne gösterilmektedir.
* Hiyerarşi penceresinin en üstündeki **Oluştur** düğmesine tıklayın ve **UI** > **giriş alanı**' nı seçin.
* Bu, hiyerarşi penceresinde görebileceğiniz üç oyun nesnesini oluşturur: bir **tuval** nesnesi içinde iç içe geçmiş bir **giriş alanı** nesnesi ve bir **EventSystem** nesnesi.
* Sahnenin ve [sahne görünümündeki](https://docs.unity3d.com/Manual/UsingTheSceneView.html)giriş alanının iyi bir görünümünü görmek Için [sahne görünümünde gezinin](https://docs.unity3d.com/Manual/SceneViewNavigation.html) .
* Ayarlarını [Inspector penceresinde](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan olarak, sağ tarafta) göstermek Için hiyerarşi penceresindeki **giriş alanı** nesnesine tıklayın.
* **POS X** ve **POS Y** özelliklerini **0**olarak ayarlayın, bu nedenle giriş alanı tuvalin ortasında ortalanır.
* Hiyerarşi penceresinin en üstündeki **Oluştur** düğmesine yeniden tıklayın ve bir düğme oluşturmak için **UI** > **düğmesini** seçin.
* Ayarlarını [Inspector penceresinde](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan olarak, sağ tarafta) göstermek Için hiyerarşi penceresindeki **düğme** nesnesine tıklayın.
* **POS X** ve **POS Y** özelliklerini **0** ve **-48**olarak ayarlayın ve düğmenin ve girdi alanının çakışmadığından emin olmak için **Width** ve **Height** özelliklerini **160** ve **30** olarak ayarlayın.
* Hiyerarşi penceresinin en üstündeki **Oluştur** düğmesine yeniden tıklayın ve bir metin alanı oluşturmak için **UI** > **metni** ' ni seçin.
* Ayarlarını [Inspector penceresinde](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan olarak, sağ tarafta) göstermek Için hiyerarşi penceresinde **metin** nesnesine tıklayın.
* **POS X** ve **POS Y** özelliklerini **0** ve **80**olarak ayarlayın ve metin alanının ve giriş alanının çakışmadığından emin olmak için **Width** ve **Height** özelliklerini **320** ve **80** olarak ayarlayın.
* Hiyerarşi penceresinin en üstündeki **Oluştur** düğmesine yeniden tıklayın ve ses kaynağı oluşturmak için ses**ses kaynağı** ' **nı seçin.**  > 

İşiniz bittiğinde, Kullanıcı arabirimi şu ekran görüntüsüne benzer şekilde görünmelidir:

[![Unity düzenleyicisinde hızlı başlangıç Kullanıcı arabiriminin ekran görüntüsü](media/sdk/qs-tts-csharp-unity-ui-inline.png)](media/sdk/qs-tts-csharp-unity-ui-expanded.png#lightbox)

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. [Proje penceresinde](https://docs.unity3d.com/Manual/ProjectView.html) (varsayılan olarak sol alt tarafta) **Oluştur** düğmesine tıklayın ve ardından  **C# komut dosyası**' nı seçin. Betiği `HelloWorld`adlandırın.

1. Betiği çift tıklayarak düzenleyin.

   > [!NOTE]
   > Hangi kod düzenleyicisinin, **düzenleme** > **tercihleri**altında başlatılacak şekilde yapılandırılacağını, bkz. [Unity Kullanıcı el kitabı](https://docs.unity3d.com/Manual/Preferences.html).

1. Tüm kodu aşağıdaki kodla değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-unity/Assets/Scripts/HelloWorld.cs#code)]

1. Dizeyi `YourSubscriptionKey` bulun ve konuşma Hizmetleri abonelik anahtarınızla değiştirin.

1. `YourServiceRegion` dizesini bulun ve aboneliğinizle ilişkili [bölge](regions.md) ile değiştirin. Örneğin, ücretsiz denemeyi kullanıyorsanız bölge `westus` olur.

1. Değişiklikleri betikte kaydedin.

1. Unity düzenleyicisine geri döndüğünüzde, betiğin oyun nesnelerinizin birine bileşen olarak eklenmesi gerekir.

   * Hiyerarşi penceresinde **tuval** nesnesine tıklayın. Bu, ayarı [Inspector penceresinde](https://docs.unity3d.com/Manual/UsingTheInspector.html) açar (varsayılan olarak, sağ tarafta).
   * Inspector penceresinde **Bileşen Ekle** düğmesine tıklayın, ardından yukarıda oluşturduğumuz HelloWorld betiğini arayın ve ekleyin.
   * `HelloWorld` Merhaba Dünya bileşeni, sınıfının ortak özellikleriyle eşleşen dört başlatılmamış özellik, **çıkış metni**, **giriş alanı**, **konuşma düğmesi** ve **ses kaynağı**olduğunu unutmayın.
     Bunları bağlamak için nesne seçicisine (özelliğin sağ tarafındaki küçük daire simgesine) tıklayın ve daha önce oluşturduğunuz metin ve düğme nesnelerini seçin.

     > [!NOTE]
     > Giriş alanı ve düğme de iç içe metin nesnesine sahiptir. Metni yanlışlıkla metin çıktısı için seçtiğinizden emin olun (veya bu karışıklığın önüne geçmek için Inspector penceresindeki ad alanını kullanarak metin nesnelerini yeniden adlandırın).

## <a name="run-the-application-in-the-unity-editor"></a>Uygulamayı Unity düzenleyicisinde çalıştırma

* Unity Düzenleyici araç çubuğunda **oynat** düğmesine basın (menü çubuğunun altında).

* Uygulama başlatıldıktan sonra, giriş alanına bir metin girin ve düğmeye tıklayın. Metniniz konuşma hizmetlerine iletilir ve konuşmacının oynadığı konuşmayı sentezleştirilmiştir.

  [![Unity oyun penceresinde çalışan hızlı başlangıç ekranının ekran görüntüsü](media/sdk/qs-tts-csharp-unity-output-inline.png)](media/sdk/qs-tts-csharp-unity-output-expanded.png#lightbox)

* Hata ayıklama iletileri için [konsol penceresini](https://docs.unity3d.com/Manual/Console.html) denetleyin.

* Konuşmayı senizle tamamladıktan sonra, uygulamayı durdurmak için Unity Düzenleyici araç çubuğundaki **oynat** düğmesine tıklayın.

## <a name="additional-options-to-run-this-application"></a>Bu uygulamayı çalıştırmak için ek seçenekler

Bu uygulama, Windows tek başına uygulama veya UWP uygulaması olarak Android 'e de dağıtılabilir.
Bu ek hedeflerin yapılandırmasını açıklayan hızlı başlangıç/CSharp-Unity klasöründeki [örnek](https://aka.ms/csspeech/samples) havuzumuza bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub C# 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Ses yazı tiplerini özelleştirme](how-to-customize-voice-font.md)
- [Ses örneklerini Kaydet](record-custom-voice-samples.md)
