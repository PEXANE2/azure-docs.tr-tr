---
title: UWP uygulaması oluşturma
description: C#, Visual Basic ya da JavaScript’te Evrensel Windows Platformu (UWP) uygulaması geliştirme için Azure mobil uygulama arka uçlarını kullanmaya başlamak üzere bu öğreticiyi izleyin.
ms.assetid: 47124296-2908-4d92-85e0-05c4aa6db916
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/25/2019
ms.openlocfilehash: 9188db19adab9bd46d65fc97f1c62b39141cee90
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77461394"
---
# <a name="create-a-windows-app-with-an-azure-backend"></a>Azure arka uçlu bir Windows uygulaması oluşturma

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir evrensel Windows Platformu’na (UWP) bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir. Daha fazla bilgi için bkz. [Mobile Apps nedir?](app-service-mobile-value-prop.md). Aşağıdaki ekran görüntüleri tamamlanan şu uygulamalara aittir:

![Tamamlanmış masaüstü uygulaması](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)

Bu öğreticiyi tamamlamak UWP uygulamalarına ilişkin tüm Mobil Uygulama öğreticileri için ön koşuldur.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Bir hesabınız yoksa, Azure deneme sürümünü kaydolabilir ve deneme süresi bittikten sonra dahi kullanmaya devam edebileceğiniz 10 ücretsiz mobil uygulama edinebilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Windows 10.
* Visual Studio Community 2017.
* UWP uygulama geliştirme ile aşinalık. UWP uygulamaları oluşturmak için nasıl [ayarlın](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) öğrenince [UWP belgelerini](https://docs.microsoft.com/windows/uwp/) ziyaret edin.

## <a name="create-a-new-azure-mobile-app-backend"></a>Yeni bir Azure Mobil Uygulama arka ucu oluşturma

Yeni Mobil Uygulama arka ucu oluşturmak için bu adımları izleyin.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Veritabanı bağlantısı oluşturun ve istemci ve sunucu projesini yapılandırma
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-client-project"></a>İstemci projesini çalıştırma

1. UWP projesini açın.

2. [Azure portalına](https://portal.azure.com/) gidin ve oluşturduğunuz mobil uygulamaya gidin. Bıçakta, `Overview` mobil uygulamanızın genel bitiş noktası olan URL'ye bakın. Örnek - uygulama adım "test123" için https://test123.azurewebsites.netsite adı olacaktır.

3. Bu klasördeki dosyayı `App.xaml.cs` açın - windows-uwp-cs/ZUMOAPPNAME/. Uygulama `ZUMOAPPNAME`adı.

4. Sınıfta, `App` parametreyi yukarıdaki ortak bitiş noktasıyla değiştirin. `ZUMOAPPURL`

    `public static MobileServiceClient MobileService = new MobileServiceClient("ZUMOAPPURL");`

    Olur
    
    `public static MobileServiceClient MobileService = new MobileServiceClient("https://test123.azurewebsites.net");`

5. Uygulamayı dağıtmak ve çalıştırmak için F5 tuşuna basın.

6. Uygulamada **TodoItem Ekle** metin kutusuna *Öğreticiyi tamamla* gibi anlamlı bir metin yazın ve ardından**Kaydet**’e tıklayın.

    ![Windows hızlı başlangıç tamamlanmış masaüstü](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Bu, Azure üzerinde barındırılan yeni mobil uygulama arka ucuna bir POST isteği gönderir.
