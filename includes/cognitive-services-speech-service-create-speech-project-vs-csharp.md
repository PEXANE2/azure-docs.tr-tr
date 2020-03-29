---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 08/30/2019
ms.author: wolfma
ms.openlocfilehash: c7332177795a45331749225e0f3dc3856e28a6a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "71327039"
---
Windows geliştirme için bir Visual Studio projesi oluşturmak için projeyi oluşturmanız, .NET masaüstü geliştirme için Visual Studio'yu kurmanız, Speech SDK'yı yüklemeniz ve hedef mimarisini seçmeniz gerekir.

### <a name="create-the-project-and-add-the-workload"></a>Projeyi oluşturun ve iş yükünü ekleyin

Başlatmak için Visual Studio'da projeyi oluşturun ve Visual Studio'nun .NET masaüstü geliştirme için ayarlandığınızdan emin olun:

1. Görsel Stüdyo 2019'u açın.

1. Başlat penceresinde yeni **bir proje oluştur'u**seçin. 

1. Yeni **proje oluştur** penceresinde **Konsol Uygulaması'nı (.NET Framework)** seçin ve ardından **İleri'yi**seçin.

1. Yeni **proje pencerenizi Yapılandır'** da, **Project adına** *helloworld* girin, **Konum'da**dizin yolunu seçin veya oluşturun ve ardından **Oluştur'u**seçin.

1. Visual Studio menü çubuğundan, Visual Studio Installer'ı açan ve **Değiştirme** iletişim kutusunu görüntüleyen **Araçlar** > **Araçlar ve Özellikler Al'ı**seçin.

1. **.NET masaüstü geliştirme** iş yükünün kullanılabilir olup olmadığını kontrol edin. İş yükü yüklenmediyse, yanındaki onay kutusunu seçin ve yüklemeyi başlatmak için **Değiştir'i** seçin. İndirmek ve yüklemek birkaç dakika sürebilir.

   **.NET masaüstü geliştirmesinin** yanındaki onay kutusu zaten seçiliyse, iletişim kutusundan çıkmak için **Kapat'ı** seçin.

   ![.NET masaüstü geliştirmesini etkinleştirme](../articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

1. Visual Studio Yükleyici'yi kapatın.

### <a name="install-the-speech-sdk"></a>Konuşma SDK'yı yükleyin

Bir sonraki adım [Konuşma SDK NuGet paketini](https://aka.ms/csspeech/nuget)yüklemektir, böylece koda başvuruyapabilirsiniz.

1. Çözüm Gezgini'nde **helloworld** projesine sağ tıklayın ve ardından **NuGet** Paket Yöneticisi'ni göstermek için Paketleri Yönet'i seçin.

   ![NuGet Paket Yöneticisi](../articles/cognitive-services/speech-service/media/sdk/vs-nuget-package-manager.png)

1. Sağ üst köşede, Paket **Kaynağı** açılır kutusunu bulun ve **nuget.org** seçildiğinden emin olun.

1. Sol üst köşede **Gözat'ı**seçin.

1. Arama kutusuna *Microsoft.CognitiveServices.Speech* yazın ve **Enter'u**seçin.

1. Arama sonuçlarından **Microsoft.CognitiveServices.Speech** paketini seçin ve ardından en son kararlı sürümü yüklemek için **Yükle'yi** seçin.

   ![Microsoft.CognitiveServices.Speech NuGet paketini yükleyin](../articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png)

1. Yüklemeyi başlatmak için tüm anlaşmaları ve lisansları kabul edin.

   Paket yüklendikten sonra **Paket Yöneticisi Konsol** penceresinde bir onay görüntülenir.

### <a name="choose-the-target-architecture"></a>Hedef mimariyi seçin

Şimdi, konsol uygulamasını oluşturmak ve çalıştırmak için bilgisayarınızın mimarisiyle eşleşen bir platform yapılandırması oluşturun.

1. Menü çubuğundan**Yapı Yapılandırma Yöneticisi'ni** **seçin.** >  **Configuration Manager** iletişim kutusu görüntülenir.

   ![Configuration Manager iletişim kutusu](../articles/cognitive-services/speech-service/media/sdk/vs-configuration-manager-dialog-box.png)

1. Etkin **çözüm platformu** açılır kutusunda **Yeni'yi**seçin. **Yeni Çözüm Platformu** iletişim kutusu görüntülenir.

1. Türü'nde veya yeni platform açılır kutusunu **seçin:**
   - 64 bit Windows çalıştırıyorsanız, **x64'i**seçin.
   - 32 bit Windows çalıştırıyorsanız, **x86'yı**seçin.

1. **Tamam'ı** seçin ve ardından **Kapat'ı**seçin.
