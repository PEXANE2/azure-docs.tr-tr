---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 02/10/2020
ms.author: erhopf
ms.openlocfilehash: 5db99a9d500b05fa6886dce2f29087920f8a1790
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80658310"
---
Evrensel Windows Platformu (UWP) geliştirme için bir Visual Studio projesi oluşturmak için Visual Studio geliştirme seçeneklerini ayarlamanız, projeyi oluşturmanız, hedef mimarisini seçmeniz, ses yakalamayı ayarlamanız ve Speech SDK'yı yüklemeniz gerekir.

### <a name="set-up-visual-studio-development-options"></a>Visual Studio geliştirme seçeneklerini ayarlama

Başlamak için, UWP geliştirme için Visual Studio'da doğru şekilde ayarlandığınızdan emin olun:

1. **Başlangıç** penceresini görüntülemek için Visual Studio 2019'u açın.

   ![Başlangıç penceresi - Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-start-window.png)

1. Visual Studio IDE'ye gitmek için **kodsuz Devam'ı** seçin.

1. Visual Studio menü çubuğundan Visual Studio Installer'ı açmak ve **Değiştirme** iletişim kutusunu görüntülemek için **Araçlar** > **Al Araçları ve Özellikleri'ni** seçin.

   ![İş Yükleri sekmesi, Iletişim kutusunu değiştirme, Visual Studio Yükleyici](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-workload.png)

1. Windows'un **altında,** **İş Yükleri** sekmesinde **Evrensel Windows Platformu geliştirme** iş yükünü bulun. Bu iş yükünün yanındaki onay kutusu zaten seçiliyse, **Değiştir** iletişim kutusunu kapatın ve 6.

1. Evrensel **Windows Platformu geliştirme** onay kutusunu seçin, **Değiştir'i**seçin ve ardından iletişim kutusunu **başlatmadan önce** UWP geliştirme iş yükünü yüklemeye **devam** et'i seçin. Yeni özelliğin yüklenmesi biraz zaman alabilir.

1. Visual Studio Yükleyici'yi kapatın.

### <a name="create-the-project-and-select-the-target-architecture"></a>Projeyi oluşturun ve hedef mimariyi seçin

Ardından, projenizi oluşturun:

1. Visual Studio menü çubuğunda, yeni proje **penceresi oluşturma** penceresini görüntülemek için**Yeni** >  **Dosya** > **Projesi'ni** seçin.

   ![Yeni bir proje oluşturun - Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-create-new-project.png)

1. **Boş Uygulama (Evrensel Windows)** bulun ve seçin. Bu proje türünün C# sürümünü seçtiğinizden emin olun (Visual Basic'in aksine).

1. **Yeni proje ekranınızı Yapılandırmak** için **İleri'yi** seçin.

   ![Yeni projenizi yapılandırın - Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-configure-your-new-project.png)

1. **Proje adına**, `helloworld`girin.

1. **Konum'da,** projenizi kaydetmek için klasöre gidin ve seçin veya oluşturun.

1. Yeni Evrensel Windows **Platformu Projesi** penceresine gitmek için **Oluştur'u** seçin.

   ![Yeni Evrensel Windows Platformu Projesi iletişim kutusu - Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-02-new-uwp-project.png)

1. **Minimum sürümde** (ikinci açılır kutu) **Windows 10 Fall Creators Update'i seçin (10.0; Yapı 16299)**, Konuşma SDK için minimum gerekliliktir.

1. **Hedef sürümünde** (ilk açılır kutu), **Minimum sürümdeki**değerle aynı veya daha sonraki bir değer seçin.

1. **Tamam'ı**seçin. **Solution Explorer** bölmesinde oluşturulan ve görünen yeni projeyle Visual Studio IDE'ye döndürülürsiniz.

   ![helloworld projesi - Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-helloworld.png)

Şimdi hedef platform mimarinizi seçin. Visual Studio araç çubuğunda **Çözüm Platformları** açılır kutusunu bulun. (Görmüyorsanız, **Çözüm Platformları**içeren araç çubuğunu görüntülemek için Araç**Çubuklarını** > **Görüntüle Standard'ı** **seçin.)** >  64 bit Windows çalıştırıyorsanız, açılır kutuda **x64'u** seçin. 64 bit Windows da 32 bit uygulamaları çalıştırabilirsiniz, böylece isterseniz **x86** seçebilirsiniz.

> [!NOTE]
> Speech SDK yalnızca Intel uyumlu işlemcileri destekler. ARM işlemciler şu anda desteklenmez.

### <a name="set-up-audio-capture"></a>Ses yakalamayı ayarlama

Ardından projenin ses girişi yakalamasına izin verin:

1. **Solution**Explorer'da, paket uygulama bildirimini açmak için **Package.appxmanifest'e** çift tıklayın.

1. **Özellikler** sekmesini seçin.

   ![Yetenekler sekmesi, Paket uygulama bildirimi - Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-07-capabilities.png)

1. **Mikrofon** özelliği için kutuyu seçin.

1. Menü çubuğundan, değişikliklerinizi kaydetmek için **Dosya** > **Kaydet Package.appxmanifest'i** seçin.

### <a name="install-the-speech-sdk"></a>Konuşma SDK'yı yükleyin

Son olarak, [Konuşma SDK NuGet paketini](https://aka.ms/csspeech/nuget)yükleyin ve projenizde Konuşma SDK'ya başvurun:

1. **Solution**Explorer'da, çözümünüzü sağ tıklatın ve **NuGet - Solution** penceresine gitmek **için Çözüm için Paketleri Yönet'i** seçin.

1. **Gözat**'ı seçin.

   ![Çözüm için Paketleri Yönet iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-nuget-solution-browse.png)

1. **Paket kaynağında,** **nuget.org**seçin.

1. **Arama** kutusuna, `Microsoft.CognitiveServices.Speech`arama sonuçlarında göründükten sonra bu paketi girin ve sonra seçin.

   ![Çözüm için Paketleri Yönet iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-05-nuget-install-1.0.0.png)

1. Arama sonuçlarının yanındaki paket durumu bölmesinde **helloworld** projenizi seçin.

1. **Yükle**’yi seçin.

1. Değişiklikleri **Önizleme** iletişim kutusunda **Tamam'ı**seçin.

1. Lisans **Kabul** iletişim kutusunda, lisansı görüntüleyin ve sonra **Kabul Et'i**seçin. Paket yüklemesi başlar ve yükleme tamamlandığında, **Çıktı** bölmesi aşağıdaki metne `Successfully installed 'Microsoft.CognitiveServices.Speech 1.11.0' to helloworld`benzer bir ileti görüntüler: .
