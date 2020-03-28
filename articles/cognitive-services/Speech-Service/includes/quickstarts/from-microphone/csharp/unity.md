---
title: 'Quickstart: Bir mikrofondan konuşmayı tanıma, C# (Birlik)- Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 12/17/2019
ms.author: erhopf
ms.openlocfilehash: be8497cb07bd3c761c59bfc55e773b5dcc937c50
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78924877"
---
> [!NOTE]
> Unity için Konuşma SDK Windows Masaüstü (x86 ve x64) veya Evrensel Windows Platformu (x86, x64, ARM/ARM64), Android (x86, ARM32/64) ve iOS (x64 simülatörü, ARM32 ve ARM64) destekler

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

> [!div class="checklist"]
> * [Azure Konuşma Kaynağı Oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=unity)
> * Ses çekimi için mikrofona erişebildiğinizden emin olun

Bunu zaten yaptıysan, harika. Devam edelim.

## <a name="create-a-unity-project"></a>Birlik projesi oluşturma

1. Birlik'i aç. Birlik'i ilk kez kullanıyorsanız, **Unity Hub** *<version number>* penceresi görüntülenir. (Bu pencereye ulaşmak için doğrudan Unity Hub'ı da açabilirsiniz.)

   [![Unity Hub penceresi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-hub.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-hub.png#lightbox)
1. **Yeni'yi**seçin. **Unity** *<version number>* penceresi ile yeni bir proje oluştur görüntülenir.

   [![Unity Hub'da yeni bir proje oluşturma](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-create-a-new-project.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-create-a-new-project.png#lightbox)
1. **Proje Adı'na** **csharp-unity**girin.
1. **Şablonlar'da** **, 3B** zaten seçilmemişse, seçin.
1. **Konum'da,** projeyi kaydetmek için bir klasör seçin veya oluşturun.
1. **Oluştur'u**seçin.

Bir süre sonra, Unity Editor penceresi görüntülenir.



## <a name="add-ui"></a>UI Ekle

Şimdi sahnemize minimum bir uI ekleyelim. Bu Kullanıcı GEkibi, konuşma tanımayı tetiklemek için bir düğme ve sonucu görüntülemek için bir metin alanından oluşur. [ **Hiyerarşi** penceresinde,](https://docs.unity3d.com/Manual/Hierarchy.html)yeni projeyle oluşturulan Birleşim' in bir örnek sahnesi gÜ

1. **Hiyerarşi** penceresinin üst**kısmında, UI** >  **Oluştur** > **düğmesini**seçin.

   Bu **eylem, Hiyerarşi** penceresinde görebileceğiniz üç oyun nesnesi oluşturur: **Düğme** nesnesi, düğmeyi içeren bir **Tuval** nesnesi ve **Bir EventSystem** nesnesi.

   [![Unity Editor ortamı](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-editor-window.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-editor-window.png#lightbox)

1. [Tuvalin **Scene** ](https://docs.unity3d.com/Manual/SceneViewNavigation.html) ve [ **Sahne** görünümündeki](https://docs.unity3d.com/Manual/UsingTheSceneView.html)düğmenin iyi bir görünümünü görmek için Sahne görünümünde gezinin.

1. [ **Denetçi** penceresinde](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan olarak sağda), **Pos X** ve **Pos Y** özelliklerini **0**olarak ayarlayın, böylece düğme tuvalin ortasında ortalanır.

1. **Hiyerarşi** penceresinde, **Metin** **nesnesi** > oluşturmak için Kullanıcı Arası Oluştur**Metni'ni** **seçin.** > 

1. **Denetçi** penceresinde, Pos **X** ve **Pos Y** özelliklerini **0** ve **120**olarak ayarlayın ve **Genişlik** ve **Yükseklik** özelliklerini **240** ve **120**olarak ayarlayın. Bu değerler, metin alanı nın ve düğmenin çakışmamasını sağlar.

İşi bittiğinde, **Sahne** görünümü bu ekran görüntüsüne benzer olmalıdır:

[![Unity Editor'da sahne görünümü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-02-ui-inline.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-02-ui-inline.png#lightbox)

## <a name="add-the-sample-code"></a>Örnek kod ekleme

Unity projesi için örnek komut dosyası kodunu eklemek için aşağıdaki adımları izleyin:

1. Proje [penceresinde,](https://docs.unity3d.com/Manual/ProjectView.html)yeni bir C# komut dosyası eklemek için**C# komut dosyası** **oluştur'u** > seçin.

   [![Unity Editor'da proje penceresi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-project-window.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-project-window.png#lightbox)
1. Komut dosyasına `HelloWorld`isim .

1. Yeni oluşturulan `HelloWorld` komut dosyasını yeniden yapmak için çift tıklatın.

   > [!NOTE]
   > Düzenleme için Unity tarafından kullanılacak kod düzenleyicisini yapılandırmak için**Tercihleri** **Düzenle'yi** > seçin ve ardından **Dış Araçlar** tercihlerine gidin. Daha fazla bilgi için [Birlik Kullanım Kılavuzu'na](https://docs.unity3d.com/Manual/Preferences.html)bakın.

1. Varolan komut dosyasını aşağıdaki kodla değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/unity/from-microphone/Assets/Scripts/HelloWorld.cs#code)]

1. Dizeyi `YourSubscriptionKey` bulun ve Konuşma hizmeti abonelik anahtarınızla değiştirin.

1. Dizeyi, `YourServiceRegion` aboneliğinizle ilişkili [bölgeden](https://aka.ms/speech/sdkregion) **Bölge tanımlayıcısıyla** bulun ve değiştirin. Örneğin, ücretsiz denemeyi kullanıyorsanız bölge `westus` olur.

1. Değişiklikleri komut dosyasına kaydedin.

Şimdi Unity Editor'a dönün ve komut dosyasını oyun nesnelerinizden birine bileşen olarak ekleyin:

1. **Hiyerarşi** **penceresinde, Kanvas** nesnesini seçin.

1. **Denetleç** penceresinde Bileşen **Ekle** düğmesini seçin.

   [![Unity Editor'da Müfettiş penceresi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-inspector-window.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-inspector-window.png#lightbox)

1. Açılan listede, yukarıda oluşturduğumuz `HelloWorld` komut dosyasını arayın ve ekleyin. Bir **Hello World (Script)** bölümü **Müfettiş** penceresinde, iki başlatılmamış özellikleri, **Çıkış Metni** ve Başlangıç **Reco Düğmesi**listeleyen görünür. Bu Birlik bileşeni özellikleri sınıfın `HelloWorld` ortak özellikleriyle eşleşir.

1. Başlat **Reco Düğmesi** özelliğinin nesne seçicisini (özelliğin sağındaki küçük daire simgesi) seçin ve daha önce oluşturduğunuz **Düğme** nesnesini seçin.

1. Çıktı **Metni** özelliğinin nesne seçicisini seçin ve daha önce oluşturduğunuz **Metin** nesnesini seçin.

   > [!NOTE]
   > Düğmede iç içe bir metin nesnesi de vardır. Metin çıktısı için yanlışlıkla seçmediğinizden (veya karışıklığı önlemek için **Inspector** penceresindeki **Ad** alanını kullanan metin nesnelerinden birini yeniden adlandırmadığınızdan) emin olun.

## <a name="run-the-application-in-the-unity-editor"></a>Birliğin Düzenleyicisi'nde uygulamayı çalıştırın

Artık uygulamayı Unity Editor içinde çalıştırmaya hazırsınız.

1. Unity Editor araç çubuğunda (menü çubuğunun altında) **Oynat** düğmesini (sağişaret üçgeni) seçin.

1. [ **Oyun** görünümüne](https://docs.unity3d.com/Manual/GameView.html)gidin ve **metni** tanımak için Metin nesnesinin **görüntülemesini bekleyin.** (Uygulama başlatılmadığında veya yanıt vermeye hazır olmadığında **Yeni Metin** görüntüler.)

1. Düğmeyi seçin ve bilgisayarınızın mikrofonuna İngilizce bir tümcecik veya cümle konuşun. Konuşmanız Konuşma hizmetine aktarılır ve **Oyun** görünümünde görünen metne aktarılır.

   [![Unity Editor'da oyun görünümü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-03-output-inline.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-03-output-inline.png#lightbox)

1. Hata ayıklama iletileri için [ **Konsol** penceresinden](https://docs.unity3d.com/Manual/Console.html) denetleyin. **Konsol** penceresi görünmüyorsa, menü çubuğuna gidin ve görüntülemek için **Pencere** > **Genel** > **Konsolu'nu** seçin.

1. Konuşmayı tanımayı bitirdiğinizde, uygulamayı durdurmak için Unity Editor araç çubuğundaki **Oynat** düğmesini seçin.

## <a name="additional-options-to-run-this-application"></a>Bu uygulamayı çalıştırmak için ek seçenekler

Bu uygulama aynı zamanda bir Android uygulaması, tek başına bir Windows uygulaması veya bir UWP uygulaması olarak da dağıtılabilir.
Daha fazla bilgi için [örnek depomuza](https://aka.ms/csspeech/samples)bakın. Klasör, `quickstart/csharp-unity` bu ek hedeflerin yapılandırmasını açıklar.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
