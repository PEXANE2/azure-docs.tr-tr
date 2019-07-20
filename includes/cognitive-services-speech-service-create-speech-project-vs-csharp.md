---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: 308ee2ef121648cb45152948926c5fd7fb934744
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68362642"
---
1. Visual Studio 2019 ' i açın.

1. Başlangıç penceresinde **Yeni proje oluştur**' u seçin. 

1. **Konsol uygulaması (.NET Framework)** öğesini seçin ve ardından **İleri**' yi seçin.

1. **Proje adı**alanına girin `helloworld`ve ardından **Oluştur**' u seçin.

1. Visual Studio 'daki menü çubuğundan **Araçlar** > **Araçlar ve Özellikler al**' ı seçin ve **.net masaüstü geliştirme** iş yükünün kullanılabilir olup olmadığını denetleyin. İş yükü yüklenmemişse, onay kutusunu işaretleyin ve ardından yüklemeyi başlatmak için **Değiştir** ' i seçin. İndirmesi ve yüklenmesi birkaç dakika sürebilir.

   **.Net masaüstü geliştirme** 'nın yanındaki onay kutusu işaretliyse, iletişim kutusunu şimdi kapatabilirsiniz.

   ![.NET masaüstü geliştirmesini etkinleştirme](../articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

Bir sonraki adım, [konuşma SDK 'Sı NuGet paketini](https://aka.ms/csspeech/nuget)yüklerken kodda başvurabilirsiniz.

1. Çözüm Gezgini, ' a sağ tıklayın ve `helloworld`ardından NuGet Paket Yöneticisi ' ni göstermek için **NuGet Paketlerini Yönet** ' i seçin.

   ![NuGet Paket Yöneticisi](../articles/cognitive-services/speech-service/media/sdk/vs-nuget-package-manager.png)

1. Sağ üst köşede, **paket kaynağı** açılan kutusunu bulun ve **NuGet.org** ' nin seçili olduğundan emin olun.

1. Sol üst köşede, **Araştır**' ı seçin.

1. Arama kutusuna Package yazın `Microsoft.CognitiveServices.Speech` ve ENTER tuşuna basın.

1. En `Microsoft.CognitiveServices.Speech`son kararlı sürümü yüklemek için, öğesini seçin ve ardından **Install** ' u seçin.

   ![Microsoft. Biliveservices. Speech NuGet paketini yükler](../articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png)

1. Yüklemeyi başlatmak için tüm anlaşmaları ve lisansları kabul edin.

   Paket yüklendikten sonra, **Paket Yöneticisi konsol** penceresinde bir onay görüntülenir.

Şimdi, konsol uygulamasını derlemek ve çalıştırmak için bilgisayarınızın mimarisiyle eşleşen bir platform yapılandırması oluşturun.

1. Menü çubuğundan **derleme** > **Configuration Manager**' yi seçin. **Configuration Manager** iletişim kutusu görüntülenir.

   ![Configuration Manager iletişim kutusu](../articles/cognitive-services/speech-service/media/sdk/vs-configuration-manager-dialog-box.png)

1. **Etkin çözüm platformu** açılan kutusunda **Yeni**' yi seçin. **Yeni çözüm platformu** iletişim kutusu görüntülenir.

1. **Yazın veya yeni platform** açılan kutusunu seçin:
   - 64 bit Windows çalıştırıyorsanız **x64**' ü seçin.
   - 32 bit Windows çalıştırıyorsanız, **x86**' yı seçin.

1. **Tamam** ' ı ve ardından **Kapat**' ı seçin.
