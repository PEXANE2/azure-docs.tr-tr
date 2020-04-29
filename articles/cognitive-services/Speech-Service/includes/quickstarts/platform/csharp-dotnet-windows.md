---
title: "Hızlı başlangıç: .NET Framework (Windows) Platform Kurulumu için konuşma SDK 'Sı-konuşma hizmeti"
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti SDK 'Sı ile Windows için .NET Framework altında C# platformunu ayarlamak için bu kılavuzu kullanın.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/10/2019
ms.author: erhopf
ms.openlocfilehash: a09b969ee3e11aeb04f338cf035b21b5da9bd952
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78924969"
---
Bu kılavuzda, .NET Framework (Windows) için [konuşma SDK 'sının](~/articles/cognitive-services/speech-service/speech-sdk.md) nasıl yükleneceği gösterilmektedir. Yalnızca paket adının kendi kendinize başlatılmasını istiyorsanız NuGet konsolunda öğesini çalıştırın `Install-Package Microsoft.CognitiveServices.Speech` .

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç şunları gerektirir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)

## <a name="create-a-visual-studio-project-and-install-the-speech-sdk"></a>Visual Studio projesi oluşturma ve konuşma SDK 'sını yüklemeye

Kodunuzda başvurabilmeniz için [konuşma SDK 'Sı NuGet paketini](https://aka.ms/csspeech/nuget) yüklemeniz gerekir. Bunu yapmak için önce bir **HelloWorld** projesi oluşturmanız gerekebilir. **.Net masaüstü geliştirme** iş yüküne sahip bir projeniz zaten varsa, bu projeyi kullanabilir ve [konuşma SDK 'Sını yüklemek Için NuGet Paket Yöneticisi 'ni kullanmak](#use-nuget-package-manager-to-install-the-speech-sdk)üzere atlayabilirsiniz.

### <a name="create-helloworld-project"></a>HelloWorld projesi oluştur

1. Visual Studio 2019 ' i açın.

1. Başlangıç penceresinde **Yeni proje oluştur**' u seçin. 

1. **Yeni proje oluştur** penceresinde **konsol uygulaması (.NET Framework)** öğesini seçin ve ardından **İleri**' yi seçin.

1. **Yeni projenizi yapılandırın** penceresinde, **Proje adı**alanına *HelloWorld* girin, **konum**bölümünde dizin yolunu seçin veya oluşturun ve ardından **Oluştur**' u seçin.

1. Visual Studio menü çubuğunda **Araçlar** > ' ı seçin ve Visual Studio yükleyicisi açan araçlar**ve Özellikler**' i seçin ve **değiştirme** iletişim kutusunu görüntüler.

1. **.Net masaüstü geliştirme** iş yükünün kullanılabilir olup olmadığını denetleyin. İş yükü yüklenmemişse, yanındaki onay kutusunu işaretleyin ve ardından yüklemeyi başlatmak için **Değiştir** ' i seçin. İndirmesi ve yüklenmesi birkaç dakika sürebilir.

   **.Net masaüstü geliştirme** 'nın yanındaki onay kutusu zaten seçiliyse, iletişim kutusundan çıkmak için **Kapat** ' ı seçin.

   ![.NET masaüstü geliştirmesini etkinleştirme](~/articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

1. Visual Studio Yükleyicisi kapatın.

### <a name="use-nuget-package-manager-to-install-the-speech-sdk"></a>Konuşma SDK 'sını yüklemek için NuGet Paket Yöneticisi 'Ni kullanma

1. Çözüm Gezgini, **HelloWorld** projesine sağ tıklayın ve ardından NuGet Paket Yöneticisi ' ni görüntülemek Için **NuGet Paketlerini Yönet** ' i seçin.

   ![NuGet Paket Yöneticisi](~/articles/cognitive-services/speech-service/media/sdk/vs-nuget-package-manager.png)

1. Sağ üst köşede, **paket kaynağı** açılan kutusunu bulun ve ' **`nuget.org`** nin seçili olduğundan emin olun.

1. Sol üst köşede, **Araştır**' ı seçin.

1. Arama kutusuna *Microsoft. Biliveservices. Speech* yazın ve **ENTER**' u seçin.

1. Arama sonuçlarından, **Microsoft. Biliveservices. Speech** paketini seçin ve ardından en son kararlı sürümü yüklemek için **yüklenir** ' i seçin.

   ![Microsoft. Biliveservices. Speech NuGet paketini yükler](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png)

1. Yüklemeyi başlatmak için tüm anlaşmaları ve lisansları kabul edin.

   Paket yüklendikten sonra, **Paket Yöneticisi konsol** penceresinde bir onay görüntülenir.

### <a name="choose-target-architecture"></a>Hedef mimari seçin

Konsol uygulamasını derlemek ve çalıştırmak için bilgisayarınızın mimarisiyle eşleşen bir platform yapılandırması oluşturun.

1. Menü çubuğundan **derleme** > **Configuration Manager**' yi seçin. **Configuration Manager** iletişim kutusu görüntülenir.

   ![Configuration Manager iletişim kutusu](~/articles/cognitive-services/speech-service/media/sdk/vs-configuration-manager-dialog-box.png)

1. **Etkin çözüm platformu** açılan kutusunda **Yeni**' yi seçin. **Yeni çözüm platformu** iletişim kutusu görüntülenir.

1. **Yazın veya yeni platform** açılan kutusunu seçin:
   - 64 bit Windows çalıştırıyorsanız **x64**' ü seçin.
   - 32 bit Windows çalıştırıyorsanız, **x86**' yı seçin.

1. **Tamam** ' ı ve ardından **Kapat**' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [windows](../quickstart-list.md)]
