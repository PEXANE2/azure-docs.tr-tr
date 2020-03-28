---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 09/13/2019
ms.author: erhopf
ms.openlocfilehash: 0bcdd315fe11b7472166a5a9ad4f7395e22d2126
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72675620"
---
Xamarin ile platformötesi mobil uygulama .NET geliştirme için bir Visual Studio projesi oluşturmak için şunları yapmanız gerekir:
- Visual Studio geliştirme seçeneklerini ayarlayın.
- Projeyi oluşturun ve hedef mimariyi seçin. 
- Konuşma SDK'sını yükleyin.

### <a name="set-up-visual-studio-development-options"></a>Visual Studio geliştirme seçeneklerini ayarlama

Başlamak için Visual Studio'da .NET ile platformlar arası mobil geliştirme için doğru şekilde ayarlandığınızdan emin olun:

1. Görsel Stüdyo 2019'u açın.

1. Visual Studio menü çubuğundan Visual Studio Installer'ı açmak ve **Değiştirme** iletişim kutusunu görüntülemek için **Araçlar** > **Al Araçları ve Özellikleri'ni** seçin.

   ![İş Yükleri sekmesi, Iletişim kutusunu değiştirme, Visual Studio Yükleyici](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-workload.png)

1. Windows'un **altında,** **İş Yükleri** sekmesinde **,.NET iş yüküyle Mobil geliştirmeyi** bulun. Bu iş yükünün yanındaki onay kutusu zaten seçiliyse, **Değiştir** iletişim kutusunu kapatın ve adım 5'e gidin.

1. .NET onay **kutusuyla Mobil geliştirmeyi** seçin ve **Değiştir'i**seçin. İletişim **kutusunu başlatmadan** önce,.NET iş yüküyle mobil geliştirmeyi yüklemeye **devam** et'i seçin. Yeni özelliğin yüklenmesi biraz zaman alabilir.

1. Visual Studio Yükleyici'yi kapatın.

### <a name="create-the-project"></a>Proje oluşturma

1. Visual Studio menü çubuğunda, yeni proje **penceresi oluşturma** penceresini görüntülemek için**Yeni** >  **Dosya** > **Projesi'ni** seçin.

   ![Yeni bir proje oluşturun - Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-create-new-project.png)

1. **Mobil Uygulamayı (Xamarin.Forms)** bulun ve seçin.

1. **Yeni proje ekranınızı Yapılandırmak** için **İleri'yi** seçin.

   ![Yeni projenizi yapılandırın - Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-configure-your-new-project.png)

1. **Proje adına**, *helloworld*girin .

1. **Konum'da,** projenizi kaydetmek için klasörü seçin ve seçin veya oluşturun.

1. Yeni Mobil Uygulama **Xamarin Forms Project** penceresine gitmek için **Oluştur'u** seçin.

   ![Yeni Evrensel Windows Platformu Projesi iletişim kutusu - Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-xamarin-new-xamarin-project.png)

1. **Boş** şablonu seçin.

1. **Platform'da,** **Android,** **iOS**ve **Windows (UWP)** için kutuları seçin.

1. **Tamam'ı**seçin. **Solution Explorer** bölmesinde oluşturulan ve görünen yeni projeyle Visual Studio IDE'ye döndürülürsiniz.

   ![Helloworld projesi - Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-helloworld.png)

Şimdi hedef platform mimarinizi ve başlangıç projenizi seçin. Visual Studio araç çubuğunda **Çözüm Platformları** açılır kutusunu bulun. (Görmüyorsanız, **Çözüm Platformları**içeren araç çubuğunu görüntülemek için Araç**Çubuklarını** > Görüntüle**Standard'ı** **seçin.)** >  64 bit Windows çalıştırıyorsanız, açılır kutuda **x64'u** seçin. 64 bit Windows da 32 bit uygulamaları çalıştırabildiği için isterseniz **x86'yı** seçebilirsiniz. **Start-up Projects** açılır kutusunda, **helloworld ayarlayın. UWP (Evrensel Windows)**.

### <a name="install-the-speech-sdk"></a>Konuşma SDK'yı yükleyin

Konuşma [SDK NuGet paketini](https://aka.ms/csspeech/nuget)yükleyin ve projenizde Konuşma SDK'ya başvurun.

1. **Solution**Explorer'da, çözümünüzü sağ tıklatın. **NuGet - Solution** penceresine gitmek **için Çözüm için NuGet Paketlerini Yönet'i** seçin.

1. **Gözat**'ı seçin.

   ![Çözüm için Paketleri Yönet iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-nuget-solution-browse.png)

1. **Paket kaynağında,** nuget.org seçin.

1. **Arama** kutusuna *Microsoft.CognitiveServices.Speech*girin. Ardından, arama sonuçlarında göründükten sonra bu paketi seçin.

   ![Çözüm için Paketleri Yönet iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-xamarin-nuget-install.png)

   > [!NOTE] 
   > NuGet'deki `Microsoft.CognitiveServices.Speech` iOS kitaplığı bit kodu etkin değil. Uygulamanız için etkin bitkodu kitaplığına `Microsoft.CognitiveServices.Speech.Xamarin.iOS` ihtiyacınız varsa, özellikle iOS projesi için NuGet'i kullanın.

1. Arama sonuçlarının yanındaki paket durum bölmesinde, tüm projeleri seçin: **helloworld**, **helloworld. Android**, **helloworld.iOS**, ve **helloworld. UWP**.

1. **Yükle**’yi seçin.

1. Değişiklikleri **Önizleme** iletişim kutusunda **Tamam'ı**seçin.

1. Lisans **Kabul** iletişim kutusunda, lisansı görüntüleyin ve sonra **Kabul Et'i**seçin. Tüm projelere Konuşma SDK paket başvuru yükleyin. Yükleme başarıyla tamamlandıktan sonra helloworld.iOS için aşağıdaki uyarıyı görebilirsiniz. Bu bilinen bir sorundur ve uygulama işlevselliğinizi etkilememelidir.

   > "C:\Users\Default\.nuget\packages\microsoft.cognitiveservices.speech\1.7.0\build\Xamarin.iOS\libMicrosoft.CognitiveServices.Speech.core.a" başvurularını çözemedi. Bu başvuru kodunuz tarafından gerekliyse, derleme hataları alabilirsiniz.
