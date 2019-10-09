---
title: Xamarin iOS uygulamaları için Azure Uygulama Hizmeti Mobile Apps’i Kullanmaya Başlama | Microsoft Belgeleri
description: Xamarin.iOS geliştirme için Mobile Apps kullanmaya başlamak için bu öğreticiyi izleyin.
services: app-service\mobile
documentationcenter: xamarin
author: elamalani
manager: crdun
editor: ''
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: 26095757f8079f43e620e5f067f461f67c7042f9
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72024845"
---
# <a name="create-a-xamarinios-app"></a>Xamarin.iOS Uygulaması oluşturma
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

> [!NOTE]
> Visual Studio App Center, mobil uygulama geliştirmeye kadar uçtan uca ve tümleşik hizmetler merkezi 'ni destekler. Geliştiriciler, sürekli tümleştirme ve teslim işlem hattı ayarlamak için **oluşturma**, **Test** etme ve **dağıtma** hizmetlerini kullanabilir. Uygulama dağıtıldıktan sonra, geliştiriciler **analiz** ve **Tanılama** hizmetlerini kullanarak uygulamasının durumunu ve kullanımını izleyebilir ve **Push** hizmetini kullanarak kullanıcılarla etkileşime geçebilir. Geliştiriciler, uygulama verilerini bulutta kalıcı hale getirmek ve eşitlemek için kullanıcıların ve **veri** hizmetinin kimliklerini doğrulamak üzere **kimlik** doğrulamasından faydalanabilir.
> Mobil uygulamanızda bulut hizmetlerini tümleştirmek istiyorsanız bugün App Center [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc) kaydolun.

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

2. [Azure Portal](https://portal.azure.com/) gidin ve oluşturduğunuz mobil uygulamaya gidin. @No__t-0 dikey penceresinde, mobil uygulamanız için genel uç nokta olan URL 'YI arayın. Örnek-uygulamamın adı "test123" olan SiteName değeri https://test123.azurewebsites.net olacaktır.

3. Bu klasördeki `QSTodoService.cs` dosyasını açın-Xamarin. iOS/ZUMOAPPNAME. Uygulama adı `ZUMOAPPNAME` ' dır.

4. @No__t-0 sınıfında, `ZUMOAPPURL` değişkenini Yukarıdaki genel uç nokta ile değiştirin.

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