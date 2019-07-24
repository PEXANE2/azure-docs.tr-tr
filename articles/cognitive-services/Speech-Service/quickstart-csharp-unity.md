---
title: 'Hızlı Başlangıç: Konuşmayı, Unity-konuşma hizmetlerini tanıma'
titleSuffix: Azure Cognitive Services
description: Unity ile bir konuşma-metin uygulaması oluşturmak için bu kılavuzu kullanın (örneğin, Unity için konuşma SDK 'Sı). İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/23/2019
ms.author: jhakulin
ms.openlocfilehash: 425cacb22865e64a996c653477120a5f7f410c9a
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68405945"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-unity-beta"></a>Hızlı Başlangıç: Unity için konuşma SDK 'Sı (Beta) ile konuşmayı tanıma

Hızlı başlangıç, [metinden konuşmaya](quickstart-text-to-speech-csharp-unity.md)için de kullanılabilir.

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

[Unity](https://unity3d.com/) 'Yi ve Unity için konuşma SDK 'Sını (Beta) kullanarak konuşmaya metin uygulaması oluşturmak için bu kılavuzu kullanın.
İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.
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
* Bilgisayarınızın mikrofonuna erişin.

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

Sahnemize en az bir kullanıcı arabirimi ekleyeceğiz. Bu, konuşma tanımayı tetikleyen bir düğmeden ve sonucu görüntüleyen bir metin alanı ekledik.

* [Hiyerarşi penceresinde](https://docs.unity3d.com/Manual/Hierarchy.html) (varsayılan olarak sol tarafta), Unity 'nin yeni projeyle oluşturulduğu örnek bir sahne gösterilmektedir.
* Hiyerarşi penceresinin en üstündeki **Oluştur** düğmesine tıklayın ve **UI** > **düğmesini**seçin.
* Bu, hiyerarşi penceresinde görebileceğiniz üç oyun nesnesini oluşturur: bir **tuval** nesnesi içinde iç Içe geçmiş **düğme** nesnesi ve bir **EventSystem** nesnesi.
* Sahnenin ve [sahne görünümündeki](https://docs.unity3d.com/Manual/UsingTheSceneView.html)düğmenin iyi bir görünümünü görmek Için [sahne görünümünde gezinin](https://docs.unity3d.com/Manual/SceneViewNavigation.html) .
* Ayarlarını [Inspector penceresinde](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan olarak, sağ tarafta) göstermek Için hiyerarşi penceresindeki **düğme** nesnesine tıklayın.
* **POS X** ve **POS Y** özelliklerini **0**olarak ayarlayın, bu nedenle düğme tuvalin ortasında ortalanır.
* Hiyerarşi penceresinin en üstündeki **Oluştur** düğmesine yeniden tıklayın ve bir metin alanı oluşturmak için **UI** > **metni** ' ni seçin.
* Ayarlarını [Inspector penceresinde](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan olarak, sağ tarafta) göstermek Için hiyerarşi penceresinde **metin** nesnesine tıklayın.
* **POS X** ve **POS Y** özelliklerini **0** ve **120**olarak ayarlayın ve metin alanının ve düğmenin çakışmadığından emin olmak için **Width** ve **Height** özelliklerini **240** ve **120** olarak ayarlayın.

İşiniz bittiğinde, Kullanıcı arabirimi şu ekran görüntüsüne benzer şekilde görünmelidir:

[![Unity düzenleyicisinde hızlı başlangıç Kullanıcı arabiriminin ekran görüntüsü](media/sdk/qs-csharp-unity-02-ui-inline.png)](media/sdk/qs-csharp-unity-02-ui-expanded.png#lightbox)

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. [Proje penceresinde](https://docs.unity3d.com/Manual/ProjectView.html) (varsayılan olarak sol alt tarafta) **Oluştur** düğmesine tıklayın ve ardından  **C# komut dosyası**' nı seçin. Betiği `HelloWorld`adlandırın.

1. Betiği çift tıklayarak düzenleyin.

   > [!NOTE]
   > Hangi kod düzenleyicisinin, **düzenleme** > **tercihleri**altında başlatılacak şekilde yapılandırılacağını, bkz. [Unity Kullanıcı el kitabı](https://docs.unity3d.com/Manual/Preferences.html).

1. Tüm kodu aşağıdaki kodla değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-unity/Assets/Scripts/HelloWorld.cs#code)]

1. Dizeyi `YourSubscriptionKey` bulun ve konuşma Hizmetleri abonelik anahtarınızla değiştirin.

1. `YourServiceRegion` dizesini bulun ve aboneliğinizle ilişkili [bölge](regions.md) ile değiştirin. Örneğin, ücretsiz denemeyi kullanıyorsanız bölge `westus` olur.

1. Değişiklikleri betikte kaydedin.

1. Unity düzenleyicisine geri döndüğünüzde, betiğin oyun nesnelerinizin birine bileşen olarak eklenmesi gerekir.

   * Hiyerarşi penceresinde **tuval** nesnesine tıklayın. Bu, ayarı [Inspector penceresinde](https://docs.unity3d.com/Manual/UsingTheInspector.html) açar (varsayılan olarak, sağ tarafta).
   * Inspector penceresinde **Bileşen Ekle** düğmesine tıklayın, ardından yukarıda oluşturduğumuz HelloWorld betiğini arayın ve ekleyin.
   * Merhaba Dünya bileşeni, `HelloWorld` sınıfının ortak özellikleriyle eşleşen iki başlatılmamış özelliğe, **Çıkış metnine** ve **Start ku düğmesine**sahip olduğunu unutmayın.
     Bunları bağlamak için nesne seçicisine (özelliğin sağ tarafındaki küçük daire simgesine) tıklayın ve daha önce oluşturduğunuz metin ve düğme nesnelerini seçin.

     > [!NOTE]
     > Düğme Ayrıca iç içe metin nesnesine sahiptir. Metni yanlışlıkla metin çıktısı için seçtiğinizden emin olun (veya bu karışıklığın önüne geçmek için Inspector penceresindeki ad alanını kullanarak metin nesnelerinden birini yeniden adlandırın).

## <a name="run-the-application-in-the-unity-editor"></a>Uygulamayı Unity düzenleyicisinde çalıştırma

* Unity Düzenleyici araç çubuğunda **oynat** düğmesine basın (menü çubuğunun altında).

* Uygulama başlatıldıktan sonra, düğmesine tıklayın ve bilgisayarınızın mikrofonuna bir Ingilizce tümcecik veya cümle konuşun. Konuşma, konuşma hizmetlerine iletilir ve pencerede görüntülenen metne gönderilir.

  [![Unity oyun penceresinde çalışan hızlı başlangıç ekranının ekran görüntüsü](media/sdk/qs-csharp-unity-03-output-inline.png)](media/sdk/qs-csharp-unity-03-output-expanded.png#lightbox)

* Hata ayıklama iletileri için [konsol penceresini](https://docs.unity3d.com/Manual/Console.html) denetleyin.

* Konuşmayı tanımayı tamamladığınızda, uygulamayı durdurmak için Unity Düzenleyici araç çubuğundaki **oynat** düğmesine tıklayın.

## <a name="additional-options-to-run-this-application"></a>Bu uygulamayı çalıştırmak için ek seçenekler

Bu uygulama, Windows tek başına uygulama veya UWP uygulaması olarak Android 'e de dağıtılabilir.
Bu ek hedeflerin yapılandırmasını açıklayan hızlı başlangıç/CSharp-Unity klasöründeki [örnek](https://aka.ms/csspeech/samples) havuzumuza bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub C# 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
