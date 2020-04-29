---
title: 'Hızlı başlangıç: bir mikrofondan konuşmayı tanıma, C# (Unity)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/02/2020
ms.author: erhopf
ms.openlocfilehash: 951ae2c48bcdd92f640a37ddbb6430ca62a3b816
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81274838"
---
> [!NOTE]
> Unity için konuşma SDK 'Sı, Windows Masaüstü (x86 ve x64) veya Evrensel Windows Platformu (x86, x64, ARM/ARM64), Android (x86, ARM32/64) ve iOS (x64 simülatör, ARM32 ve ARM64) destekler

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=unity&pivots=programming-language-csharp)
> * Ses yakalama için bir mikrofona erişiminizin olduğundan emin olun

Bunu zaten yaptıysanız harika. Şimdi devam edelim.

## <a name="create-a-unity-project"></a>Unity projesi oluşturma

1. Unity 'yi açın. Unity 'yi ilk kez kullanıyorsanız **Unity hub** *<version number>* penceresi görüntülenir. (Ayrıca, bu pencereye ulaşmak için Unity hub 'ını doğrudan açabilirsiniz.)

   [![Unity hub penceresi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-hub.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-hub.png#lightbox)
1. **Yeni**' yi seçin. *<version number>* **Unity ile yeni proje oluştur** penceresi görüntülenir.

   [![Unity hub 'da yeni proje oluşturma](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-create-a-new-project.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-create-a-new-project.png#lightbox)
1. **Proje adı**' nda, **CSharp-Unity**girin.
1. **Şablonlar**' da, **3B** zaten seçili değilse, bunu seçin.
1. **Konum**' da, projenin kaydedileceği klasörü seçin veya oluşturun.
1. **Oluştur**’u seçin.

Bir süre sonra Unity Düzenleyicisi penceresi görüntülenir.



## <a name="add-ui"></a>UI Ekle

Şimdi sahemizi en az bir kullanıcı arabirimi ekleyelim. Bu Kullanıcı arabirimi, konuşma tanımayı tetikleme ve sonucu görüntüleyen bir metin alanı içeren bir düğmeden oluşur. [ **Hiyerarşi** penceresinde](https://docs.unity3d.com/Manual/Hierarchy.html), Unity 'nin yeni projeyle oluşturulan örnek bir sahne gösterilmektedir.

1. **Hiyerarşi** penceresinin üst kısmında,**UI** >  **Oluştur** > **düğmesini**seçin.

   Bu eylem **hiyerarşi** penceresinde görebileceğiniz üç oyun nesnesi oluşturur: **düğme** nesnesi, düğmeyi içeren bir **tuval** nesnesi ve bir **EventSystem** nesnesi.

   [![Unity düzenleyici ortamı](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-editor-window.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-editor-window.png#lightbox)

1. [**Scene** ](https://docs.unity3d.com/Manual/SceneViewNavigation.html) Sahnenin ve [ **sahne** görünümündeki](https://docs.unity3d.com/Manual/UsingTheSceneView.html)düğmenin iyi bir görünümünü görmek için sahne görünümünde gezinin.

1. [ **Inspector** penceresinde](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan olarak, sağ tarafta), **POS X** ve **POS Y** özelliklerini **0**olarak ayarlayın, bu nedenle düğme tuvalin ortasında ortalanır.

1. **Hiyerarşi** penceresinde, bir **metin** nesnesi oluşturmak için**UI** > **metni** **Oluştur** > ' u seçin.

1. **Inspector** penceresinde, **POS X** ve **pos Y** özelliklerini **0** ve **120**olarak ayarlayıp **Genişlik** ve **Yükseklik** özelliklerini **240** ve **120**olarak ayarlayın. Bu değerler metin alanının ve düğmenin çakışmadığından emin olmanızı sağlamaktır.

İşiniz bittiğinde **sahne** görünümü şu ekran görüntüsüne benzer görünmelidir:

[![Unity düzenleyicisinde sahne görünümü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-02-ui-inline.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-02-ui-inline.png#lightbox)

## <a name="add-the-sample-code"></a>Örnek kod ekleme

Unity projesi için örnek betik kodu eklemek için aşağıdaki adımları izleyin:

1. [Proje penceresinde](https://docs.unity3d.com/Manual/ProjectView.html), yeni bir c# betiği eklemek için**c# betiği** **Oluştur** > ' u seçin.

   [![Unity düzenleyicisinde Proje penceresi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-project-window.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-project-window.png#lightbox)
1. Betiği `HelloWorld`adlandırın.

1. Yeni oluşturulan betiği `HelloWorld` düzenlemek için çift tıklayın.

   > [!NOTE]
   > Kod düzenleyicisini düzenleme için Unity tarafından kullanılacak şekilde yapılandırmak için,**tercihleri** **Düzenle** > ' yi seçin ve ardından **dış araçlar** tercihlerine gidin. Daha fazla bilgi için bkz. [Unity Kullanıcı el kitabı](https://docs.unity3d.com/Manual/Preferences.html).

1. Mevcut betiği şu kodla değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/unity/from-microphone/Assets/Scripts/HelloWorld.cs#code)]

1. Dizeyi `YourSubscriptionKey` bulun ve konuşma hizmeti abonelik anahtarınızla değiştirin.

1. Dizeyi `YourServiceRegion` bulun ve aboneliğinizle Ilişkili [bölgeden](https://aka.ms/speech/sdkregion) **bölge tanımlayıcısı** ile değiştirin. Örneğin, ücretsiz denemeyi kullanıyorsanız bölge `westus` olur.

1. Değişiklikleri betikte kaydedin.

Şimdi Unity düzenleyicisine dönün ve betiği oyun nesnelerinizin birine bileşen olarak ekleyin:

1. **Hiyerarşi** penceresinde **tuval** nesnesini seçin.

1. **Inspector** penceresinde, **Bileşen Ekle** düğmesini seçin.

   [![Unity düzenleyicisinde Inspector penceresi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-inspector-window.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-inspector-window.png#lightbox)

1. Aşağı açılan listede, yukarıda oluşturduğumuz `HelloWorld` betiği arayın ve ekleyin. **Inspector** penceresinde, iki başlatılmamış özellik, **çıkış metni** ve **Start ku düğmesi**listelenerek bir **Merhaba Dünya (betik)** bölümü görüntülenir. Bu Unity bileşen özellikleri, `HelloWorld` sınıfının ortak özellikleriyle eşleşir.

1. **Start ku Button** özelliğinin nesne seçicisini (özelliğin sağ tarafındaki küçük daire simgesini) seçin ve daha önce oluşturduğunuz **düğme** nesnesini seçin.

1. **Çıkış metni** özelliğinin nesne seçicisini seçin ve daha önce oluşturduğunuz **metin** nesnesini seçin.

   > [!NOTE]
   > Düğme Ayrıca iç içe metin nesnesine sahiptir. Metni yanlışlıkla metin çıktısı için seçtiğinizden emin olun (veya bir karışıklığı önlemek için **Inspector** penceresindeki **ad** alanını kullanarak metin nesnelerinden birini yeniden adlandırın).

## <a name="run-the-application-in-the-unity-editor"></a>Uygulamayı Unity düzenleyicisinde çalıştırma

Artık, uygulamayı Unity Düzenleyicisi içinde çalıştırmaya hazırsınız.

1. Unity Düzenleyici araç çubuğunda (menü çubuğunun altında) **oynat** düğmesini (sağ taraftaki bir üçgen) seçin.

1. [ **Oyun** görünümü](https://docs.unity3d.com/Manual/GameView.html)' ne gidin ve **metin** nesnesinin **konuşmayı tanımak için düğme ' yi**görüntülemesini bekleyin. (Uygulamanın başlatıldığı veya yanıt vermeye hazırlanmadığı durumlarda **Yeni metin** görüntüler.)

1. Düğmesini seçin ve bilgisayarınızın mikrofonuna bir Ingilizce tümcecik veya cümle konuşun. Konuşma, konuşma hizmetine iletilir ve **oyun** görünümünde görüntülenen metne gönderilir.

   [![Unity düzenleyicisinde oyun görünümü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-03-output-inline.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-03-output-inline.png#lightbox)

1. Hata ayıklama iletileri için [ **konsol** penceresini](https://docs.unity3d.com/Manual/Console.html) denetleyin. **Konsol** penceresi görünmüyorsa, menü çubuğuna gidin ve **pencereyi** > göstermek için**genel** > **konsol** ' ı seçin.

1. Konuşmayı tanımayı tamamladığınızda, uygulamayı durdurmak için Unity Düzenleyici araç çubuğundaki **oynat** düğmesini seçin.

## <a name="additional-options-to-run-this-application"></a>Bu uygulamayı çalıştırmak için ek seçenekler

Bu uygulama, bir Android uygulaması, Windows tek başına uygulaması veya UWP uygulaması olarak da dağıtılabilir.
Daha fazla bilgi için bkz. [örnek depomız](https://aka.ms/csspeech/samples). `quickstart/csharp-unity` Klasör bu ek hedeflerin yapılandırmasını açıklar.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]

