---
title: Xamarin.Android uygulamaları ile başlayın
description: Xamarin Android geliştirme için Azure Mobil Uygulamalarını kullanmaya başlamak için bu öğreticiyi izleyin.
ms.assetid: 81649dd3-544f-40ff-b9b7-60c66d683e60
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/25/2019
ms.openlocfilehash: b42205436c88f9075423bfcaf9e5a9fd931ee4f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77461377"
---
# <a name="create-a-xamarinandroid-app"></a>Xamarin.Android Uygulaması oluşturma
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, bir Xamarin.Android uygulamasına bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilmiştir. Daha fazla bilgi için bkz. [Mobile Apps nedir?](app-service-mobile-value-prop.md).

Aşağıda tamamlanmış uygulamadan bir ekran görüntüsünü görebilirsiniz:

![][0]

Bu öğreticiyi tamamlamak Xamarin Android uygulamalarına ilişkin tüm Mobile Apps öğreticileri için ön koşuldur.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* Etkin bir Azure hesabı. Hesabınız yoksa Azure deneme sürümü için kaydolun ve 10 ücretsiz Mobil Uygulama edinin. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Xamarin ile Visual Studio. Yönergeler için bkz. [Visual Studio ve Xamarin için Kurulum ve Yükleme](/visualstudio/cross-platform/setup-and-install).

## <a name="create-an-azure-mobile-app-backend"></a>Azure Mobil Uygulama arka ucu oluşturma
Mobil Uygulama arka ucu oluşturmak için bu adımları izleyin.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Artık, mobil istemci uygulamalarınız tarafından kullanılabilecek bir Azure Mobil Uygulama arka ucu sağladınız. Ardından, basit bir "Yapılacaklar listesi" arka ucu için sunucu projesi indirin ve Azure’a yayımlayın.

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Veritabanı bağlantısı oluşturun ve istemci ve sunucu projesini yapılandırma
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinandroid-app"></a>Xamarin.Android uygulamasını çalıştırın
1. Xamarin.Android projesini açın.

2. [Azure portalına](https://portal.azure.com/) gidin ve oluşturduğunuz mobil uygulamaya gidin. Bıçakta, `Overview` mobil uygulamanızın genel bitiş noktası olan URL'ye bakın. Örnek - uygulama adım "test123" için https://test123.azurewebsites.netsite adı olacaktır.

3. Bu klasördeki dosyayı `ToDoActivity.cs` açın - xamarin.android/ZUMOAPPNAME/ToDoActivity.cs. Uygulama `ZUMOAPPNAME`adı.

4. Sınıfta, `ToDoActivity` değişkeni yukarıdaki ortak bitiş noktasıyla değiştirin. `ZUMOAPPURL`

    `const string applicationURL = @"ZUMOAPPURL";`

    Olur
    
    `const string applicationURL = @"https://test123.azurewebsites.net";`
    
5. Uygulamayı dağıtmak ve çalıştırmak için F5 tuşuna basın.

6. Uygulamada, *Öğreticiyi tamamla* gibi anlamlı bir metin yazın ve ardından **Ekle** düğmesine basın.

    ![][10]

    İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil uygulama arka ucu tarafından döndürülür ve veriler listede görüntülenir.

   > [!NOTE]
   > Sorgulamak ve ToDoActivity.cs C# dosyasında bulunan verileri eklemek için, mobil uygulamanızın arka ucuna erişen kodu gözden geçirebilirsiniz.
   
## <a name="troubleshooting"></a>Sorun giderme
Çözümü oluşturma konusunda sorun yaşarsanız, NuGet paket yöneticisini çalıştırıp `Xamarin.Android` destek paketlerini güncelleştirin. Hızlı Başlangıç projeleri her zaman son sürümleri içermeyebilir.

Projenizde başvurulan tüm destek paketlerinin aynı sürüme sahip olması gerektiğini unutmayın. [Azure Mobile Apps NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/), Android platformu için `Xamarin.Android.Support.CustomTabs` bağımlılığına sahiptir, yani projeniz daha yeni destek paketleri kullanıyorsa, çakışmaları önlemek için doğrudan gerekli sürüme sahip bu paketi yüklemeniz gerekir.

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png
<!-- URLs. -->
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
