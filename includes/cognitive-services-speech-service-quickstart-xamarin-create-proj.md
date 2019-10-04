---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 09/13/2019
ms.author: erhopf
ms.openlocfilehash: 0e4e67710c98b80dce2b0d55a86869625f3942d2
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71837381"
---
Xamarin ile platformlar arası mobil uygulama .NET geliştirmesi için Visual Studio projesi oluşturmak üzere Visual Studio geliştirme seçeneklerini ayarlamanız, projeyi oluşturmanız, hedef mimariyi seçmeniz ve konuşma SDK 'sını yüklemeniz gerekir.

### <a name="set-up-visual-studio-development-options"></a>Visual Studio geliştirme seçeneklerini ayarlama

Başlamak için, .NET ile platformlar arası mobil geliştirme için Visual Studio 'da doğru şekilde ayarlandığından emin olun:

1. Visual Studio 2019 ' i açın.

1. Visual Studio menü çubuğunda **araçlar** >  ' i seçerek Visual Studio yükleyicisi**açın ve** **değiştirme** iletişim kutusunu görüntüleyin.

   ![İş yükleri sekmesi, değiştirme iletişim kutusu, Visual Studio Yükleyicisi](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-workload.png)

1. **Iş yükleri** sekmesinde, **Windows**altında .net Iş yüküne **sahip mobil geliştirmeyi** bulun. Bu iş yükünün yanındaki onay kutusu zaten seçiliyse, **değiştirme** iletişim kutusunu kapatın ve 5. adıma gidin.

1. **.Net Ile mobil geliştirme** onay kutusunu seçin, **Değiştir**' i seçin ve ardından **Başlarken** iletişim kutusunda, mobil geliştirmeyi .NET iş yüküne yüklemek için **devam** ' ı seçin. Yeni özelliğin yüklenmesi biraz zaman alabilir.

1. Visual Studio Yükleyicisi kapatın.

### <a name="create-the-project"></a>Proje oluşturma

1. Visual Studio menü çubuğunda **dosya** > **Yeni** > **Proje** ' yi seçerek **Yeni proje oluştur** penceresini görüntüleyin.

   ![Yeni proje oluşturma-Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-create-new-project.png)

1. **Mobil uygulama (Xamarin Forms)** bulun ve seçin.

1. **Yeni projeyi Yapılandır** ekranınızı göstermek için **İleri ' yi** seçin. 

   ![Yeni projenizi yapılandırma-Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-configure-your-new-project.png)

1. **Proje adı**' nda `helloworld` girin.

1. **Konum**' a gidin ve projenizi kaydetmek için klasörü seçin veya oluşturun.

1. **Yeni mobil uygulama Xamarin formları proje** penceresine gitmek için **Oluştur** ' u seçin.

   ![Yeni Evrensel Windows Platformu Projesi iletişim kutusu-Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-xamarin-new-xamarin-project.png)

1. **Boş** şablon seçin

1. **Platform**' da **Android**, **iOS** ve **Windows (UWP)** için onay kutularını işaretleyin.

1. **Tamam**’ı seçin. Visual Studio IDE 'ye geri döndürüyorsunuz, yeni proje oluşturulup **Çözüm Gezgini** bölmesinde görünür.

   ![HelloWorld projesi-Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-helloworld.png)

Şimdi hedef platform mimarinizi ve başlangıç projenizi seçin. Visual Studio araç çubuğunda, **çözüm platformları** açılan kutusunu bulun. (Bunu görmüyorsanız, **çözüm platformlarını**içeren araç çubuğunu görüntülemek için  > **araç çubuklarını** **görüntüle** > **standardı** ' nı seçin.) 64 bit Windows çalıştırıyorsanız, açılan kutuda **x64** ' ü seçin. 64 bit Windows ayrıca 32 bit uygulamalar çalıştırabilir, bu sayede isterseniz **x86** seçeneğini belirleyebilirsiniz. **Başlangıç projeleri** açılan kutusu için HelloWorld ' i ayarlayın. UWP (Evrensel Windows).

### <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

[Konuşma SDK 'Sı NuGet paketini](https://aka.ms/csspeech/nuget)yükledikten sonra projenizdeki konuşma SDK 'sına başvurun:

1. **Çözüm Gezgini**, çözümünüze sağ tıklayın ve **çözüm Için NuGet Paketlerini Yönet** ' i seçerek **NuGet-çözüm** penceresine gidin.

1. **Gözat**’ı seçin.

   ![Çözüm için Paketleri Yönet iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-nuget-solution-browse.png)

1. **Paket kaynağı**' nda **NuGet.org**' yi seçin.

1. **Arama** kutusuna `Microsoft.CognitiveServices.Speech` girin ve sonra arama sonuçlarında göründükten sonra bu paketi seçin.

   ![Çözüm için Paketleri Yönet iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-xamarin-nuget-install.png)
   > Not: Microsoft. Biliveservices. Speech NuGet içindeki iOS kitaplığı bitcode 'u 'u etkinleştirmiyor. Uygulamanız için bitcode 'u özellikli kitaplığa ihtiyacınız varsa, özellikle iOS projesi için Microsoft. Biliveservices. Speech. Xamarin. iOS NuGet ' i kullanın.

1. Arama sonuçlarının yanındaki paket durumu bölmesinde tüm projeler ' i seçin. **HelloWorld**, **HelloWorld. Android**, **HelloWorld. iOS** ve **HelloWorld. UWP**.

1. **Yükle**’yi seçin.

1. **Değişiklikleri Önizle** Iletişim kutusunda **Tamam**' ı seçin.

1. **Lisans kabulü** iletişim kutusunda, lisansı görüntüleyin ve ardından tüm projelere konuşma SDK 'Sı başvurusunu **kabul ediyorum** ve yüklensin ' i seçin. Yükleme başarıyla tamamlandıktan sonra HelloWorld. iOS için aşağıdaki uyarıyı görebilirsiniz. Bu bilinen bir sorundur ve uygulama işlevselliklerinizi etkilememelidir.

> "C:\Users\Default @ no__t-0nuget\packages\microsoft.cognitiveservices.speech\1.7.0\build\Xamarin.iOS\libMicrosoft.CognitiveServices.Speech.core.a" başvurusu çözümlenemedi. Kodunuz için bu başvuru gerekliyse, derleme hataları alabilirsiniz.
