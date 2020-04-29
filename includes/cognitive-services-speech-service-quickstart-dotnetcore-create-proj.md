---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 12/12/2018
ms.author: erhopf
ms.openlocfilehash: 777b2608cf5f326556dfaea307f4f3e9346213f8
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "66482376"
---
1. Visual Studio 2019 ' i başlatın.

1. **.NET platformlar arası geliştirme** iş yükünün bulunduğundan emin olun.  > Visual Studio yükleyicisi 'ni açmak için Visual Studio menü çubuğunda Araçlar**ve Özellikler Al ' ı** seçin. **Tools** Bu iş yükü zaten etkinse iletişim kutusunu kapatın.

   ![Visual Studio yükleyicisinin İş yükleri sekmesi vurgulanmış olarak ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-net-core-workload.png)

   Etkin değilse **.NET Core platformlar arası geliştirme**'nin yanındaki kutuyu işaretleyin ve iletişim kutusunun sağ alt köşesindeki **Değiştir**'i seçin. Yeni özelliğin yüklenme birkaç saniye sürer.

1. Yeni bir Visual C# .NET Core Konsol Uygulaması oluşturun. **Yeni Proje** iletişim kutusunda, sol panelden **Yüklü** > **Visual C#** > **.NET Core**'u genişletin. Ardından **Console App (.NET Core)** seçeneğine tıklayın. Proje adı olarak *helloworld* girin.

   ![Yeni Proje iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-01-new-console-app.png "Visual C# konsol uygulaması oluşturma (.NET Core)")

1. [Konuşma SDK NuGet paketi](https://aka.ms/csspeech/nuget)'ni yükleyip başvurulara ekleyin. Çözüm Gezgini'nde çözüme sağ tıklayın ve ardından **Çözüm için NuGet Paketlerini Yönet**'i seçin.

   ![Çözüm Gezgini ekran görüntüsü, çözüm için NuGet Paketlerini Yönet vurgulanmış](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-02-manage-nuget-packages.png "Çözüm için NuGet Paketlerini Yönet")

1. Sağ üst köşede, **paket kaynağı** alanında **NuGet.org**' yi seçin. `Microsoft.CognitiveServices.Speech` Paketi arayın ve **HelloWorld** projesine yükler.

   ![Çözüm için Paketleri Yönet iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-03-nuget-install-1.0.0.png "NuGet paketini yükler")

1. NuGet paketinin yükleme işlemini başlatmak için görüntülenen lisansı kabul edin.

   ![Lisans Kabulü iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-04-nuget-license.png "Lisansı kabul et")

Paket yüklendikten sonra Paket Yöneticisi konsolunda bir onay görünür.
