---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 12/12/2018
ms.author: erhopf
ms.openlocfilehash: 777b2608cf5f326556dfaea307f4f3e9346213f8
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66482376"
---
1. Visual Studio 2019'u başlatın.

1. **.NET platformlar arası geliştirme** iş yükünün bulunduğundan emin olun. Visual Studio yükleyicisini açmak için Visual Studio menü çubuğundan **Araçlar** > **Alın Araçları ve Özellikleri** seçin. Bu iş yükü zaten etkinse iletişim kutusunu kapatın.

   ![Visual Studio yükleyicisinin İş yükleri sekmesi vurgulanmış olarak ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-net-core-workload.png)

   Etkin değilse **.NET Core platformlar arası geliştirme**'nin yanındaki kutuyu işaretleyin ve iletişim kutusunun sağ alt köşesindeki **Değiştir**'i seçin. Yeni özelliğin yüklenme birkaç saniye sürer.

1. Yeni bir Visual C# .NET Core Konsol Uygulaması oluşturun. **Yeni Proje** iletişim kutusunda, sol panelden **Yüklü** > **Visual C#** > **.NET Core**'u genişletin. Ardından **Console App (.NET Core)** seçeneğine tıklayın. Proje adı olarak *helloworld* girin.

   ![Yeni Proje iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-01-new-console-app.png "Görsel C# Konsol Uygulaması Oluştur (.NET Core)")

1. [Konuşma SDK NuGet paketi](https://aka.ms/csspeech/nuget)'ni yükleyip başvurulara ekleyin. Çözüm Gezgini'nde çözüme sağ tıklayın ve ardından **Çözüm için NuGet Paketlerini Yönet**'i seçin.

   ![Çözüm Için NuGet Paketlerini Yönet vurgulanmış olan Solution Explorer ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-02-manage-nuget-packages.png "Çözüm için NuGet Paketlerini Yönetin")

1. Sağ üst köşede, Paket **Kaynak** **alanında, nuget.org**seçin. `Microsoft.CognitiveServices.Speech` Paketi arayın ve **helloworld** projesine yükleyin.

   ![Çözüm için Paketleri Yönet iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-03-nuget-install-1.0.0.png "NuGet paketini yükleyin")

1. NuGet paketinin yükleme işlemini başlatmak için görüntülenen lisansı kabul edin.

   ![Lisans Kabulü iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-04-nuget-license.png "Lisansı kabul et")

Paket yüklendikten sonra Paket Yöneticisi konsolunda bir onay görünür.
