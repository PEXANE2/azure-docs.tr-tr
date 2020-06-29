---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 02/10/2020
ms.author: erhopf
ms.openlocfilehash: a24c0a3129a851eb94c43158424f786b1df5a532
ms.sourcegitcommit: fdaad48994bdb9e35cdd445c31b4bac0dd006294
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85414376"
---
Evrensel Windows Platformu (UWP) geliştirme için Visual Studio projesi oluşturmak üzere Visual Studio geliştirme seçeneklerini ayarlamanız, projeyi oluşturmanız, hedef mimariyi seçmeniz, ses yakalamayı ayarlamanız ve konuşma SDK 'sını yüklemeniz gerekir.

### <a name="set-up-visual-studio-development-options"></a>Visual Studio geliştirme seçeneklerini ayarlama

Başlamak için, UWP geliştirmesi için Visual Studio 'da doğru şekilde ayarlandığından emin olun:

1. **Başlangıç** penceresini göstermek Için Visual Studio 2019 ' i açın.

   ![Başlangıç penceresi-Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-start-window.png)

1. Visual Studio IDE 'ye gitmek için **kod olmadan devam et** ' i seçin.

1. Visual Studio menü çubuğunda **Araçlar**' ı  >  açmak için Araçlar**ve Özellikler al** ' ı seçin Visual Studio yükleyicisi açın ve **değiştirme** iletişim kutusunu görüntüleyin.

   ![İş yükleri sekmesi, değiştirme iletişim kutusu, Visual Studio Yükleyicisi](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-workload.png)

1. **Iş yükleri** sekmesinde, **Windows**altında **Evrensel Windows platformu geliştirme** iş yükünü bulun. Bu iş yükünün yanındaki onay kutusu zaten seçiliyse, **değiştirme** iletişim kutusunu kapatın ve 6. adıma gidin.

1. **Evrensel Windows platformu geliştirme** onay kutusunu seçin, **Değiştir**' i seçin ve ardından **Başlarken** iletişim kutusunda, UWP geliştirme iş yükünü yüklemek için **devam** ' ı seçin. Yeni özelliğin yüklenmesi biraz zaman alabilir.

1. Visual Studio Yükleyicisi kapatın.

### <a name="create-the-project-and-select-the-target-architecture"></a>Projeyi oluşturma ve hedef mimarisini seçme

Sonra, projenizi oluşturun:

1. **File**  >  **New**  >  **Yeni proje oluştur** penceresini göstermek için Visual Studio menü çubuğunda dosya yeni**Proje** ' yi seçin.

   ![Yeni proje oluşturma-Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-create-new-project.png)

1. Boş uygulama bulun ve seçin **(Evrensel Windows)**. Bu proje türünün C# sürümünü seçtiğinizden emin olun (Visual Basic aksine).

1. **Yeni projeyi Yapılandır** ekranınızı göstermek için **İleri ' yi** seçin.

   ![Yeni projenizi yapılandırma-Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-configure-your-new-project.png)

1. **Proje adı**alanına girin `helloworld` .

1. **Konum**' a gidin ve projenizi kaydetmek için klasörü seçin veya oluşturun.

1. **Yeni Evrensel Windows platformu projesi** penceresine gitmek için **Oluştur** ' u seçin.

   ![Yeni Evrensel Windows Platformu Projesi iletişim kutusu-Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-02-new-uwp-project.png)

1. **En düşük sürüm** (ikinci açılan kutu) altında, **Windows 10 Fall Creators Update (10,0) öğesini seçin. Yapı 16299)**, konuşma SDK 'sı için en düşük gereksinimdir.

1. **Hedef sürümde** (ilk açılan kutu), **En düşük sürümdeki**değerden veya daha sonraki bir değere sahip bir değer seçin.

1. **Tamam**’ı seçin. Visual Studio IDE 'ye geri döndürüyorsunuz, yeni proje oluşturulup **Çözüm Gezgini** bölmesinde görünür.

   ![HelloWorld projesi-Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-helloworld.png)

Şimdi hedef platform mimarinizi seçin. Visual Studio araç çubuğunda, **çözüm platformları** açılan kutusunu bulun. (Görmüyorsanız, **Görünüm**  >  ' ü seçin. **Araç çubukları**  >  **Çözüm platformlarını**içeren araç çubuğunu göstermek için **Standart** .) 64 bit Windows çalıştırıyorsanız, açılan kutuda **x64** ' ü seçin. 64 bit Windows ayrıca 32 bit uygulamalar çalıştırabilir, bu sayede isterseniz **x86** seçeneğini belirleyebilirsiniz.

> [!NOTE]
> Konuşma SDK 'Sı yalnızca Intel uyumlu işlemcileri destekler. ARM İşlemcileri Şu anda desteklenmiyor.

### <a name="set-up-audio-capture"></a>Ses yakalamayı ayarlama

Projenin ses girişi yakalamaya izin ver:

1. **Çözüm Gezgini**, paket uygulama bildirimini açmak için **Package. appxmanifest** öğesine çift tıklayın.

1. **Özellikler** sekmesini seçin.

   ![Yetenekler sekmesi, paket uygulama bildirimi-Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-07-capabilities.png)

1. **Mikrofon** yeteneğinin kutusunu seçin.

1. Değişikliklerinizi kaydetmek için menü çubuğundan **Dosya**  >  **Kaydet. appxmanifest** ' i seçin.

### <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

Son olarak, [konuşma SDK 'Sı NuGet paketini](https://aka.ms/csspeech/nuget)yükleyip projenizde konuşma SDK 'sına başvurun:

1. **Çözüm Gezgini**, çözümünüze sağ tıklayın ve **çözüm Için NuGet Paketlerini Yönet** ' i seçerek **NuGet-çözüm** penceresine gidin.

1. **Gözat**'ı seçin.

   ![Çözüm için Paketleri Yönet iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-nuget-solution-browse.png)

1. **Paket kaynağı**' nda **NuGet.org**' yi seçin.

1. **Arama** kutusuna girin `Microsoft.CognitiveServices.Speech` ve ardından arama sonuçlarında göründükten sonra bu paketi seçin.

   ![Çözüm için Paketleri Yönet iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-05-nuget-install-1.0.0.png)

1. Arama sonuçlarının yanındaki paket durumu bölmesinde **HelloWorld** projenizi seçin.

1. **Yükle**'yi seçin.

1. **Değişiklikleri Önizle** Iletişim kutusunda **Tamam**' ı seçin.

1. **Lisans kabulü** iletişim kutusunda, lisansı görüntüleyin ve **kabul ediyorum**' u seçin. Paket yüklemesi başlar ve yükleme tamamlandığında **Çıkış** bölmesinde şu metne benzer bir ileti görüntülenir: `Successfully installed 'Microsoft.CognitiveServices.Speech 1.12.1' to helloworld` .
