---
title: 'Quickstart: .NET Framework (Windows) platformu kurulumu için Konuşma SDK - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti SDK ile .NET Framework for Windows altında C# platformunuzu kurmak için bu kılavuzu kullanın.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/10/2019
ms.author: erhopf
ms.openlocfilehash: a09b969ee3e11aeb04f338cf035b21b5da9bd952
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78924969"
---
Bu kılavuz, .NET Framework (Windows) için [Konuşma SDK'sının](~/articles/cognitive-services/speech-service/speech-sdk.md) nasıl yüklenir olduğunu gösterir. Paket adının kendi başınızın başlamasını istiyorsanız, `Install-Package Microsoft.CognitiveServices.Speech` NuGet konsolunda çalıştırın.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç şunları gerektirir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)

## <a name="create-a-visual-studio-project-and-install-the-speech-sdk"></a>Görsel Stüdyo projesi oluşturun ve Konuşma SDK'yı kurun

Kodunuzda başvuruda bulunabilmeniz için [Konuşma SDK NuGet paketini](https://aka.ms/csspeech/nuget) yüklemeniz gerekir. Bunu yapmak için öncelikle bir **helloworld** projesi oluşturmanız gerekebilir. **Zaten .NET masaüstü geliştirme** iş yükü mevcut bir proje varsa, bu projeyi kullanabilirsiniz ve Konuşma SDK yüklemek için [NuGet Paket Yöneticisi'ni kullanmak](#use-nuget-package-manager-to-install-the-speech-sdk)için atlayabilirsiniz.

### <a name="create-helloworld-project"></a>helloworld projesi oluşturun

1. Görsel Stüdyo 2019'u açın.

1. Başlat penceresinde yeni **bir proje oluştur'u**seçin. 

1. Yeni **proje oluştur** penceresinde **Konsol Uygulaması'nı (.NET Framework)** seçin ve ardından **İleri'yi**seçin.

1. Yeni **proje pencerenizi Yapılandır'** da, **Project adına** *helloworld* girin, **Konum'da**dizin yolunu seçin veya oluşturun ve ardından **Oluştur'u**seçin.

1. Visual Studio menü çubuğundan, Visual Studio Installer'ı açan ve **Değiştirme** iletişim kutusunu görüntüleyen **Araçlar** > **Araçlar ve Özellikler Al'ı**seçin.

1. **.NET masaüstü geliştirme** iş yükünün kullanılabilir olup olmadığını kontrol edin. İş yükü yüklenmediyse, yanındaki onay kutusunu seçin ve yüklemeyi başlatmak için **Değiştir'i** seçin. İndirmek ve yüklemek birkaç dakika sürebilir.

   **.NET masaüstü geliştirmesinin** yanındaki onay kutusu zaten seçiliyse, iletişim kutusundan çıkmak için **Kapat'ı** seçin.

   ![.NET masaüstü geliştirmesini etkinleştirme](~/articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

1. Visual Studio Yükleyici'yi kapatın.

### <a name="use-nuget-package-manager-to-install-the-speech-sdk"></a>Konuşma SDK'yı yüklemek için NuGet Paket Yöneticisi'ni kullanın

1. Çözüm Gezgini'nde **helloworld** projesine sağ tıklayın ve ardından **NuGet** Paket Yöneticisi'ni göstermek için Paketleri Yönet'i seçin.

   ![NuGet Paket Yöneticisi](~/articles/cognitive-services/speech-service/media/sdk/vs-nuget-package-manager.png)

1. Sağ üst köşede, Paket **Kaynağı** açılır kutusunu bulun ve seçildiğinden **`nuget.org`** emin olun.

1. Sol üst köşede **Gözat'ı**seçin.

1. Arama kutusuna *Microsoft.CognitiveServices.Speech* yazın ve **Enter'u**seçin.

1. Arama sonuçlarından **Microsoft.CognitiveServices.Speech** paketini seçin ve ardından en son kararlı sürümü yüklemek için **Yükle'yi** seçin.

   ![Microsoft.CognitiveServices.Speech NuGet paketini yükleyin](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png)

1. Yüklemeyi başlatmak için tüm anlaşmaları ve lisansları kabul edin.

   Paket yüklendikten sonra **Paket Yöneticisi Konsol** penceresinde bir onay görüntülenir.

### <a name="choose-target-architecture"></a>Hedef mimarisini seçin

Konsol uygulamasını oluşturmak ve çalıştırmak için bilgisayarınızın mimarisiyle eşleşen bir platform yapılandırması oluşturun.

1. Menü çubuğundan**Yapı Yapılandırma Yöneticisi'ni** **seçin.** >  **Configuration Manager** iletişim kutusu görüntülenir.

   ![Configuration Manager iletişim kutusu](~/articles/cognitive-services/speech-service/media/sdk/vs-configuration-manager-dialog-box.png)

1. Etkin **çözüm platformu** açılır kutusunda **Yeni'yi**seçin. **Yeni Çözüm Platformu** iletişim kutusu görüntülenir.

1. Türü'nde veya yeni platform açılır kutusunu **seçin:**
   - 64 bit Windows çalıştırıyorsanız, **x64'i**seçin.
   - 32 bit Windows çalıştırıyorsanız, **x86'yı**seçin.

1. **Tamam'ı** seçin ve ardından **Kapat'ı**seçin.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list.md)]
