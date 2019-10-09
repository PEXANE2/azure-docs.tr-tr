---
title: Azure Mobile Apps kullanan bir Evrensel Windows Platformu (UWP) oluşturun | Microsoft Docs
description: C#, Visual Basic ya da JavaScript’te Evrensel Windows Platformu (UWP) uygulaması geliştirme için Azure mobil uygulama arka uçlarını kullanmaya başlamak üzere bu öğreticiyi izleyin.
services: app-service\mobile
documentationcenter: windows
author: elamalani
manager: crdun
editor: ''
ms.assetid: 47124296-2908-4d92-85e0-05c4aa6db916
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: 6bb94828344b6f6e846b573a94e3049d03effe89
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72025198"
---
# <a name="create-a-windows-app-with-an-azure-backend"></a>Azure arka ucu ile Windows uygulaması oluşturma

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

> [!NOTE]
> Visual Studio App Center, mobil uygulama geliştirmeye kadar uçtan uca ve tümleşik hizmetler merkezi 'ni destekler. Geliştiriciler, sürekli tümleştirme ve teslim işlem hattı ayarlamak için **oluşturma**, **Test** etme ve **dağıtma** hizmetlerini kullanabilir. Uygulama dağıtıldıktan sonra, geliştiriciler **analiz** ve **Tanılama** hizmetlerini kullanarak uygulamasının durumunu ve kullanımını izleyebilir ve **Push** hizmetini kullanarak kullanıcılarla etkileşime geçebilir. Geliştiriciler, uygulama verilerini bulutta kalıcı hale getirmek ve eşitlemek için kullanıcıların ve **veri** hizmetinin kimliklerini doğrulamak üzere **kimlik** doğrulamasından faydalanabilir.
> Mobil uygulamanızda bulut hizmetlerini tümleştirmek istiyorsanız bugün App Center [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc) kaydolun.

## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir evrensel Windows Platformu’na (UWP) bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir. Daha fazla bilgi için bkz. [Mobile Apps nedir?](app-service-mobile-value-prop.md). Aşağıdaki ekran görüntüleri tamamlanan şu uygulamalara aittir:

![Tamamlanmış masaüstü uygulaması](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)

Bu öğreticiyi tamamlamak UWP uygulamalarına ilişkin tüm Mobil Uygulama öğreticileri için ön koşuldur.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Bir hesabınız yoksa, Azure deneme sürümünü kaydolabilir ve deneme süresi bittikten sonra dahi kullanmaya devam edebileceğiniz 10 ücretsiz mobil uygulama edinebilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Windows 10.
* Visual Studio Community 2017.
* UWP uygulama geliştirmeye aşina. UWP uygulamalarını oluşturmak için nasıl [ayarlanbileceğinizi](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) öğrenmek için [UWP belgelerini](https://docs.microsoft.com/windows/uwp/) ziyaret edin.

## <a name="create-a-new-azure-mobile-app-backend"></a>Yeni bir Azure Mobil Uygulama arka ucu oluşturma

Yeni Mobil Uygulama arka ucu oluşturmak için bu adımları izleyin.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Bir veritabanı bağlantısı oluşturma ve istemciyi ve sunucu projesini yapılandırma
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-client-project"></a>İstemci projesini çalıştırma

1. UWP projesini açın.

2. [Azure Portal](https://portal.azure.com/) gidin ve oluşturduğunuz mobil uygulamaya gidin. @No__t-0 dikey penceresinde, mobil uygulamanız için genel uç nokta olan URL 'YI arayın. Örnek-uygulamamın adı "test123" olan SiteName değeri https://test123.azurewebsites.net olacaktır.

3. Bu klasördeki `App.xaml.cs` dosyasını açın-Windows-UWP-CS/ZUMOAPPNAME/. Uygulama adı `ZUMOAPPNAME` ' dır.

4. @No__t-0 sınıfında, `ZUMOAPPURL` parametresini yukarıdaki genel uç nokta ile değiştirin.

    `public static MobileServiceClient MobileService = new MobileServiceClient("ZUMOAPPURL");`

    geldiğinde
    
    `public static MobileServiceClient MobileService = new MobileServiceClient("https://test123.azurewebsites.net");`

5. Uygulamayı dağıtmak ve çalıştırmak için F5 tuşuna basın.

6. Uygulamada **TodoItem Ekle** metin kutusuna *Öğreticiyi tamamla* gibi anlamlı bir metin yazın ve ardından**Kaydet**’e tıklayın.

    ![Windows hızlı başlangıç tamamlanmış masaüstü](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Bu, Azure üzerinde barındırılan yeni mobil uygulama arka ucuna bir POST isteği gönderir.
