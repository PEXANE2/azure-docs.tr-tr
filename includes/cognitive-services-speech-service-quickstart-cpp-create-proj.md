---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 02/10/2020
ms.author: erhopf
ms.openlocfilehash: 72e9334b4df58b1b90288cb7363a7d94bb531661
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80658365"
---
C++ masaüstü geliştirme için bir Visual Studio projesi oluşturmak için Visual Studio geliştirme seçeneklerini ayarlamanız, projeyi oluşturmanız, hedef mimariyi seçmeniz ve Konuşma SDK'sını yüklemeniz gerekir.

### <a name="set-up-visual-studio-development-options"></a>Visual Studio geliştirme seçeneklerini ayarlama

Başlamak için, C++ masaüstü geliştirme için Visual Studio'da doğru şekilde ayarlandığınızdan emin olun:

1. **Başlangıç** penceresini görüntülemek için Visual Studio 2019'u açın.

   ![Başlangıç penceresi - Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-start-window.png)

1. Visual Studio IDE'ye gitmek için **kodsuz Devam'ı** seçin.

1. Visual Studio menü çubuğundan Visual Studio Installer'ı açmak ve **Değiştirme** iletişim kutusunu görüntülemek için **Araçlar** > **Al Araçları ve Özellikleri'ni** seçin.

   ![İş Yükleri sekmesi, Iletişim kutusunu değiştirme, Visual Studio Yükleyici](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-cpp-workload.png)

1. Windows'un **altında,** **İş Yükleri** sekmesinde, C++ iş **yüküne sahip Masaüstü geliştirmesini** bulun. İş yükünün yanındaki onay kutusu zaten seçilmemişse, onu seçin.

1. Bireysel **bileşenler** sekmesinde, **Nuget paket yöneticisi** onay kutusunu bulun. Onay kutusu zaten seçilmediyse, onu seçin.

1. **Köşedeki** düğmeyi seçin ya Kapat veya **Değiştir**etiketli. (Düğme adı, yükleme için herhangi bir özellik seçip seçmediğinize bağlı olarak değişir.) **Değiştir'i**seçerseniz, yükleme başlar ve bu da biraz zaman alabilir.

1. Visual Studio Yükleyici'yi kapatın.

### <a name="create-the-project-and-select-the-target-architecture"></a>Projeyi oluşturun ve hedef mimariyi seçin

Ardından, projenizi oluşturun:

1. Visual Studio menü çubuğunda, yeni proje **penceresi oluşturma** penceresini görüntülemek için**Yeni** >  **Dosya** > **Projesi'ni** seçin.

   ![Yeni bir proje oluşturun, C++ - Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-01-new-console-app.png)

1. **Konsol Uygulamasını**bul ve seç. Bu proje türünün C++ sürümünü seçtiğinizden emin olun (C# veya Visual Basic'in aksine).

1. **Yeni proje ekranınızı Yapılandırmak** için **İleri'yi** seçin.

   ![Yeni projeniz C++ yapıla - Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-cpp-configure-your-new-project.png)

1. **Proje adına**, `helloworld`girin.

1. **Konum'da,** projenizi kaydetmek için klasöre gidin ve seçin veya oluşturun.

Şimdi hedef platform mimarinizi seçin. Visual Studio araç çubuğunda **Çözüm Platformları** açılır kutusunu bulun. (Görmüyorsanız, **Çözüm Platformları**içeren araç çubuğunu görüntülemek için Araç**Çubuklarını** > **Görüntüle Standard'ı** **seçin.)** >  64 bit Windows çalıştırıyorsanız, açılır kutuda **x64'u** seçin. 64 bit Windows da 32 bit uygulamaları çalıştırabilirsiniz, böylece isterseniz **x86** seçebilirsiniz.

### <a name="install-the-speech-sdk"></a>Konuşma SDK'yı yükleyin

Son olarak, [Konuşma SDK NuGet paketini](https://aka.ms/csspeech/nuget)yükleyin ve projenizde Konuşma SDK'ya başvurun:

1. **Solution**Explorer'da, çözümünüzü sağ tıklatın ve **Nuget - Solution** penceresine gitmek **için Çözüm için Paketleri Yönet'i** seçin.

1. **Gözat**'ı seçin.

   ![NuGet - Çözüm sekmesi, Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-03-manage-nuget-packages.png)

1. **Paket kaynağında,** **nuget.org**seçin.

1. **Arama** kutusuna, `Microsoft.CognitiveServices.Speech`arama sonuçlarında göründükten sonra bu paketi girin ve sonra seçin.

   ![Microsoft.CognitiveServices.Speech C++ paketi yükleme - Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-04-nuget-install-1.0.0.png)

1. Arama sonuçlarının yanındaki paket durumu bölmesinde **helloworld** projenizi seçin.

1. **Yükle**’yi seçin.

1. Değişiklikleri **Önizleme** iletişim kutusunda **Tamam'ı**seçin.

1. Lisans **Kabul** iletişim kutusunda, lisansı görüntüleyin ve sonra **Kabul Et'i**seçin. Paket yüklemesi başlar ve yükleme tamamlandığında, **Çıktı** bölmesi aşağıdaki metne `Successfully installed 'Microsoft.CognitiveServices.Speech 1.11.0' to helloworld`benzer bir ileti görüntüler: .
