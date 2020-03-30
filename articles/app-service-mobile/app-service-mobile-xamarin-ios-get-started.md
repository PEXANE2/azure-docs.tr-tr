---
title: Xamarin.iOS uygulamaları yla başlayın
description: Xamarin.iOS geliştirme için Mobile Apps kullanmaya başlamak için bu öğreticiyi izleyin.
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/25/2019
ms.openlocfilehash: 6c35189e7c841fa2724f1cfe84afc689d5510676
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77459013"
---
# <a name="create-a-xamarinios-app"></a>Yeni bir Xamarin.iOS uygulaması oluşturma
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, bir Xamarin.iOS mobil uygulamasına Azure mobil uygulaması arka ucunu kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir.  Hem yeni bir mobil arka uç hem de Azure’da uygulama verilerini depolayan basit bir *Yapılacaklar listesi* oluşturursunuz.

Bu öğreticiyi tamamlamak, Azure App Service’de Mobile Apps özelliğini kullanmayla ilgili diğer tüm Xamarin.iOS öğreticileri için ön koşuldur.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* Etkin bir Azure hesabı. Hesabınız yoksa Azure deneme sürümü için kaydolun ve deneme süreniz bittikten sonra bile kullanmaya devam edebileceğiniz 10 ücretsiz mobil uygulama edinin. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Mac için Visual Studio. [Mac için Visual Studio için Kurulum ve yükleme](https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019) ye bakın
* Xcode 9.0 veya sonrası olan bir Mac.
  
## <a name="create-an-azure-mobile-app-backend"></a>Azure Mobil Uygulama arka ucu oluşturma
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Veritabanı bağlantısı oluşturun ve istemci ve sunucu projesini yapılandırma
[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinios-app"></a>Xamarin.iOS uygulamasını çalıştırın
1. Xamarin.iOS projesini açın.

2. [Azure portalına](https://portal.azure.com/) gidin ve oluşturduğunuz mobil uygulamaya gidin. Bıçakta, `Overview` mobil uygulamanızın genel bitiş noktası olan URL'ye bakın. Örnek - uygulama adım "test123" için https://test123.azurewebsites.netsite adı olacaktır.

3. Bu klasördeki dosyayı `QSTodoService.cs` açın - xamarin.iOS/ZUMOAPPNAME. Uygulama `ZUMOAPPNAME`adı.

4. Sınıfta, `QSTodoService` değişkeni yukarıdaki ortak bitiş noktasıyla değiştirin. `ZUMOAPPURL`

    `const string applicationURL = @"ZUMOAPPURL";`

    Olur
    
    `const string applicationURL = @"https://test123.azurewebsites.net";`
    
5. Uygulamayı bir iPhone emülatörde dağıtmak ve çalıştırmak için F5 tuşuna basın.

6. Uygulamada, *öğreticiyi tamamla* ve sonra + düğmesini tıklatın gibi anlamlı metin yazın.

    ![][10]

    İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil uygulama arka ucu tarafından döndürülür ve veriler listede görüntülenir.

   > [!NOTE]
   > Sorgulamak ve ToDoActivity.cs C# dosyasında bulunan verileri eklemek için, mobil uygulamanızın arka ucuna erişen kodu gözden geçirebilirsiniz.
   
<!-- Images. -->
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png
