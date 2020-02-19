---
title: Xamarin. iOS uygulamalarını kullanmaya başlama
description: Xamarin.iOS geliştirme için Mobile Apps kullanmaya başlamak için bu öğreticiyi izleyin.
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/25/2019
ms.openlocfilehash: 6c35189e7c841fa2724f1cfe84afc689d5510676
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77459013"
---
# <a name="create-a-xamarinios-app"></a>Yeni bir Xamarin.iOS uygulaması oluşturma
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, bir Xamarin.iOS mobil uygulamasına Azure mobil uygulaması arka ucunu kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir.  Hem yeni bir mobil arka uç hem de Azure’da uygulama verilerini depolayan basit bir *Yapılacaklar listesi* oluşturursunuz.

Bu öğreticiyi tamamlamak, Azure App Service’de Mobile Apps özelliğini kullanmayla ilgili diğer tüm Xamarin.iOS öğreticileri için ön koşuldur.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* Etkin bir Azure hesabı. Hesabınız yoksa Azure deneme sürümü için kaydolun ve deneme süreniz bittikten sonra bile kullanmaya devam edebileceğiniz 10 ücretsiz mobil uygulama edinin. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Mac için Visual Studio. [Mac için Visual Studio için bkz. Kurulum ve yükleme](https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019)
* Xcode 9,0 veya üzeri bir Mac.
  
## <a name="create-an-azure-mobile-app-backend"></a>Azure Mobil Uygulama arka ucu oluşturma
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Bir veritabanı bağlantısı oluşturma ve istemciyi ve sunucu projesini yapılandırma
[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinios-app"></a>Xamarin. iOS uygulamasını çalıştırma
1. Xamarin. iOS projesini açın.

2. [Azure Portal](https://portal.azure.com/) gidin ve oluşturduğunuz mobil uygulamaya gidin. `Overview` dikey penceresinde, mobil uygulamanız için genel uç nokta olan URL 'YI arayın. Örnek-uygulamamın adı "test123" olan SiteName değeri https://test123.azurewebsites.netolacaktır.

3. Bu klasördeki dosya `QSTodoService.cs` açın-Xamarin. iOS/ZUMOAPPNAME. Uygulama adı `ZUMOAPPNAME`.

4. `QSTodoService` sınıfında, `ZUMOAPPURL` değişkenini Yukarıdaki genel uç nokta ile değiştirin.

    `const string applicationURL = @"ZUMOAPPURL";`

    geldiğinde
    
    `const string applicationURL = @"https://test123.azurewebsites.net";`
    
5. Uygulamayı bir iPhone öykünücüsünde dağıtmak ve çalıştırmak için F5 tuşuna basın.

6. Uygulamada, *öğreticiyi doldurun* gibi anlamlı bir metin yazın ve ardından + düğmesine tıklayın.

    ![][10]

    İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil uygulama arka ucu tarafından döndürülür ve veriler listede görüntülenir.

   > [!NOTE]
   > Sorgulamak ve ToDoActivity.cs C# dosyasında bulunan verileri eklemek için, mobil uygulamanızın arka ucuna erişen kodu gözden geçirebilirsiniz.
   
<!-- Images. -->
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png
