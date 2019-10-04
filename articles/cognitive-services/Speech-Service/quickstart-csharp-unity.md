---
title: 'Hızlı başlangıç: konuşmayı tanıma, Unity-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Unity ile bir konuşmadan metin uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 09/17/2019
ms.author: jhakulin
ms.openlocfilehash: a7ac6831fd21fcc0dc425b57f5d73d8c328f9350
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71803487"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-unity"></a>Hızlı başlangıç: Unity için konuşma SDK 'Sı ile konuşmayı tanıma

Hızlı başlangıç, [metinden konuşmaya](quickstart-text-to-speech-csharp-unity.md)için de kullanılabilir.

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

[Unity 'yi ve Unity](https://unity3d.com/) için konuşma SDK 'sını kullanarak konuşmadan metne bir uygulama oluşturmak için bu kılavuzu kullanın.
İşiniz bittiğinde, konuşmayı gerçek zamanlı olarak metne dönüştürmek için cihazunuzla konuşabilirsiniz.
Unity için yeni başladıysanız, uygulamanızı geliştirmeye başlamadan önce [Unity Kullanıcı el ile](https://docs.unity3d.com/Manual/UnityManual.html) araştırma yapmanız önerilir.

> [!NOTE]
> Unity için konuşma SDK 'Sı, Windows Masaüstü (x86 ve x64) veya Evrensel Windows Platformu (x86, x64, ARM/ARM64), Android (x86, ARM32/64) ve iOS (x64 simülatör, ARM32 ve ARM64) destekler

## <a name="prerequisites"></a>Önkoşullar

Bu projeyi tamamlamak için şunlar gerekir:

- Unity [2018,3 veya üzeri](https://store.unity.com/) Unity [2019,1, UWP ARM64 için destek ekleme](https://blogs.unity3d.com/2019/04/16/introducing-unity-2019-1/#universal).
- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/). Visual Studio 2017 sürüm 15,9 veya üzeri sürümleri de kabul edilebilir.
- Windows ARM64 desteği için, [ARM64 için isteğe bağlı derleme araçlarını ve ARM64 Için Windows 10 SDK](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/)'yı yüklemelisiniz.
- Konuşma hizmeti için bir abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).
- Bilgisayarınızın mikrofonuna erişin.

## <a name="create-a-unity-project"></a>Unity projesi oluşturma

1. Unity 'yi açın. Unity 'yi ilk kez kullanıyorsanız, **Unity Hub** *<version number>* penceresi görüntülenir. (Ayrıca, bu pencereye ulaşmak için Unity hub 'ını doğrudan açabilirsiniz.)

   [![Unity hub penceresi](media/sdk/qs-csharp-unity-hub.png)](media/sdk/qs-csharp-unity-hub.png#lightbox)
1. **Yeni**'yi seçin. Unity *<version number>* **içeren yeni bir proje oluştur** penceresi görüntülenir.

   [![ Unity hub 'da yeni proje oluşturma](media/sdk/qs-csharp-unity-create-a-new-project.png)](media/sdk/qs-csharp-unity-create-a-new-project.png#lightbox)
1. **Proje adı**' nda, **CSharp-Unity**girin.
1. **Şablonlar**' da, **3B** zaten seçili değilse, bunu seçin.
1. **Konum**' da, projenin kaydedileceği klasörü seçin veya oluşturun.
1. **Oluştur**'u seçin.

Bir süre sonra Unity Düzenleyicisi penceresi görüntülenir.

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

Unity için konuşma SDK 'sını yüklemek için şu adımları izleyin:

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

1. Unity varlık paketi (. unitypackage) olarak paketlenmiş [Unity Için konuşma SDK 'sını](https://aka.ms/csspeech/unitypackage)indirip açın. Varlık paketi açıldığında, **Unity paketini Içeri aktar** iletişim kutusu görünür.

   [![Unity düzenleyicisinde Unity paketini Içeri Aktar iletişim kutusu](media/sdk/qs-csharp-unity-01-import.png)](media/sdk/qs-csharp-unity-01-import.png#lightbox)
1. Tüm dosyaların seçili olduğundan emin olun ve **Içeri aktar**' ı seçin. Birkaç dakika sonra Unity varlık paketi projenize aktarılır.

Varlık paketlerini Unity 'ye aktarma hakkında daha fazla bilgi için [Unity belgelerine](https://docs.unity3d.com/Manual/AssetPackages.html)bakın.

## <a name="add-ui"></a>UI Ekle

Şimdi sahemizi en az bir kullanıcı arabirimi ekleyelim. Bu Kullanıcı arabirimi, konuşma tanımayı tetikleme ve sonucu görüntüleyen bir metin alanı içeren bir düğmeden oluşur. [ **Hiyerarşi** penceresinde](https://docs.unity3d.com/Manual/Hierarchy.html), Unity 'nin yeni projeyle oluşturulan örnek bir sahne gösterilmektedir.

1. **Hiyerarşi** penceresinin üst kısmında, @no__t **Oluştur**-2**Kullanıcı arabirimi** > **düğmesini**seçin.

   Bu eylem **hiyerarşi** penceresinde görebileceğiniz üç oyun nesnesi oluşturur: **düğme** nesnesi, düğmeyi içeren bir **tuval** nesnesi ve bir **EventSystem** nesnesi.

   [![Unity düzenleyici ortamı](media/sdk/qs-csharp-unity-editor-window.png)](media/sdk/qs-csharp-unity-editor-window.png#lightbox)

1. [ **](https://docs.unity3d.com/Manual/SceneViewNavigation.html) Sahnenin ve [ **sahne** görünümündeki](https://docs.unity3d.com/Manual/UsingTheSceneView.html)düğmenin iyi bir görünümünü görmek için sahne görünümünde gezinin.

1. [ **Inspector** penceresinde](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan olarak, sağ tarafta), **POS X** ve **POS Y** özelliklerini **0**olarak ayarlayın, bu nedenle düğme tuvalin ortasında ortalanır.

1. **Hiyerarşi** penceresinde, **metin** nesnesi oluşturmak için @no__t **oluştur**-2**Kullanıcı arabirimi** > **metin** ' i seçin.

1. **Inspector** penceresinde, **POS X** ve **pos Y** özelliklerini **0** ve **120**olarak ayarlayıp **Genişlik** ve **Yükseklik** özelliklerini **240** ve **120**olarak ayarlayın. Bu değerler metin alanının ve düğmenin çakışmadığından emin olmanızı sağlamaktır.

İşiniz bittiğinde **sahne** görünümü şu ekran görüntüsüne benzer görünmelidir:

[Unity düzenleyicisinde ![Sahne görünümü](media/sdk/qs-csharp-unity-02-ui-inline.png)](media/sdk/qs-csharp-unity-02-ui-inline.png#lightbox)

## <a name="add-the-sample-code"></a>Örnek kod ekleme

Unity projesi için örnek betik kodu eklemek için aşağıdaki adımları izleyin:

1. [Proje penceresinde](https://docs.unity3d.com/Manual/ProjectView.html), C# yeni bir betik eklemek için  >  **C# betiği** **Oluştur**' u seçin.

   [Unity düzenleyicisinde ![Proje penceresi](media/sdk/qs-csharp-unity-project-window.png)](media/sdk/qs-csharp-unity-project-window.png#lightbox)
1. Betiği `HelloWorld` olarak adlandırın.

1. Yeni oluşturulan betiği düzenlemek için `HelloWorld` ' a çift tıklayın.

   > [!NOTE]
   > Kod düzenleyicisini düzenleme için Unity tarafından kullanılmak üzere yapılandırmak için  >  tercihlerini **Düzenle**' yi seçinve ardından **dış araçlar** tercihlerine gidin. Daha fazla bilgi için bkz. [Unity Kullanıcı el kitabı](https://docs.unity3d.com/Manual/Preferences.html).

1. Mevcut betiği şu kodla değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-unity/Assets/Scripts/HelloWorld.cs#code)]

1. @No__t-0 dizesini bulun ve, konuşma Hizmetleri abonelik anahtarınızla değiştirin.

1. @No__t-0 dizgisini, aboneliğinizle ilişkili [bölgeyle](regions.md) bulun ve değiştirin. Örneğin, ücretsiz denemeyi kullanıyorsanız bölge `westus` olur.

1. Değişiklikleri betikte kaydedin.

Şimdi Unity düzenleyicisine dönün ve betiği oyun nesnelerinizin birine bileşen olarak ekleyin:

1. **Hiyerarşi** penceresinde **tuval** nesnesini seçin.

1. **Inspector** penceresinde, **Bileşen Ekle** düğmesini seçin.

   [Unity düzenleyicisinde ![ınspector penceresi](media/sdk/qs-csharp-unity-inspector-window.png)](media/sdk/qs-csharp-unity-inspector-window.png#lightbox)

1. Aşağı açılan listede, yukarıda oluşturduğumuz @no__t 0 betiği arayın ve ekleyin. **Inspector** penceresinde, iki başlatılmamış özellik, **çıkış metni** ve **Start ku düğmesi**listelenerek bir **Merhaba Dünya (betik)** bölümü görüntülenir. Bu Unity bileşen özellikleri `HelloWorld` sınıfının ortak özellikleriyle eşleşir.

1. **Start ku Button** özelliğinin nesne seçicisini (özelliğin sağ tarafındaki küçük daire simgesini) seçin ve daha önce oluşturduğunuz **düğme** nesnesini seçin.

1. **Çıkış metni** özelliğinin nesne seçicisini seçin ve daha önce oluşturduğunuz **metin** nesnesini seçin.

   > [!NOTE]
   > Düğme Ayrıca iç içe metin nesnesine sahiptir. Metni yanlışlıkla metin çıktısı için seçtiğinizden emin olun (veya bir karışıklığı önlemek için **Inspector** penceresindeki **ad** alanını kullanarak metin nesnelerinden birini yeniden adlandırın).

## <a name="run-the-application-in-the-unity-editor"></a>Uygulamayı Unity düzenleyicisinde çalıştırma

Artık, uygulamayı Unity Düzenleyicisi içinde çalıştırmaya hazırsınız.

1. Unity Düzenleyici araç çubuğunda (menü çubuğunun altında) **oynat** düğmesini (sağ taraftaki bir üçgen) seçin.

1. [ **Oyun** görünümü](https://docs.unity3d.com/Manual/GameView.html)' ne gidin ve **metin** nesnesinin **konuşmayı tanımak için düğme ' yi**görüntülemesini bekleyin. (Uygulamanın başlatıldığı veya yanıt vermeye hazırlanmadığı durumlarda **Yeni metin** görüntüler.)

1. Düğmesini seçin ve bilgisayarınızın mikrofonuna bir Ingilizce tümcecik veya cümle konuşun. Konuşma, konuşma hizmetlerine iletilir ve **oyun** görünümünde görüntülenen metne gönderilir.

   [Unity düzenleyicisinde ![ oyun görünümü](media/sdk/qs-csharp-unity-03-output-inline.png)](media/sdk/qs-csharp-unity-03-output-inline.png#lightbox)

1. Hata ayıklama iletileri için [ **konsol** penceresini](https://docs.unity3d.com/Manual/Console.html) denetleyin. **Konsol** penceresi görünmüyorsa, menü çubuğuna gidin ve **pencereyi**göstermek Için  > **genel** > **konsolunu** seçin.

1. Konuşmayı tanımayı tamamladığınızda, uygulamayı durdurmak için Unity Düzenleyici araç çubuğundaki **oynat** düğmesini seçin.

## <a name="additional-options-to-run-this-application"></a>Bu uygulamayı çalıştırmak için ek seçenekler

Bu uygulama, bir Android uygulaması, Windows tek başına uygulaması veya UWP uygulaması olarak da dağıtılabilir.
Daha fazla bilgi için bkz. [örnek depomız](https://aka.ms/csspeech/samples). @No__t-0 klasörü, bu ek hedeflerin yapılandırmasını açıklar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub C# 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Özel Konuşma Tanıma için model eğitme](how-to-custom-speech-train-model.md)
