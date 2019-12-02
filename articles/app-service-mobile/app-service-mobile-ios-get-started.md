---
title: iOS uygulaması oluşturun
description: Amaç-C veya Swift 'ta iOS geliştirmesi için Azure mobil uygulaması arka uçları kullanmaya başlamak için bu öğreticiyi izleyin.
ms.assetid: 6461a899-9340-42dd-b118-ffc5ba00e846
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: conceptual
ms.date: 06/25/2019
ms.openlocfilehash: d933319d22fe7622f0409e8931c41a801fcd18ca
ms.sourcegitcommit: 3d4917ed58603ab59d1902c5d8388b954147fe50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74668800"
---
# <a name="create-an-ios-app"></a>iOS uygulaması oluşturun

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

> [!NOTE]
> Visual Studio App Center mobil uygulama dağıtımında merkezi konumdaki uçtan uca ve tümleşik hizmetleri destekler. Geliştiriciler Sürekli Tümleştirme ve Teslim işlem hattını ayarlamak için **Oluşturma**, **Test** ve **Dağıtım** hizmetlerini kullanabilir. Uygulama dağıtıldıktan sonra, geliştiriciler **Analiz** ve **Tanılama** hizmetlerini kullanarak uygulamanın durumunu ve kullanımını izleyebilir, **Gönderme** hizmetini kullanarak kullanıcılarla etkileşim kurabilir. Geliştiriciler ayrıca kullanıcıların kimliğini doğrulamak için **Kimlik Doğrulaması**'ndan ve uygulama verilerini bulutta kalıcı hale getirmek ve eşitlemek için **Veri** hizmetinden yararlanabilir.
>
> Bulut hizmetlerini mobil uygulamanızla tümleştirmek istiyorsanız [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc)'a hemen kaydolun.

## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir iOS uygulamasına bulut tabanlı arka uç hizmeti olan [Azure App Service Mobile Apps](app-service-mobile-value-prop.md)’i nasıl ekleyeceğiniz gösterilmiştir. Birinci adım, Azure'da yeni bir mobil arka uç oluşturmaktır. Ardından Azure'da veri depolayan basit bir *Todo list* iOS örnek uygulaması indirin.

Bu öğreticiyi tamamlamak için bir [Mac bilgisayar](https://azure.microsoft.com/pricing/free-trial/) ve Azure hesabınızın olması gerekir

## <a name="create-a-new-azure-mobile-app-backend"></a>Yeni bir Azure mobil uygulama arka ucu oluşturma

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Bir veritabanı bağlantısı oluşturma ve istemciyi ve sunucu projesini yapılandırma
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-ios-app"></a>İOS uygulamasını çalıştırma

[!INCLUDE [app-service-mobile-ios-run-app](../../includes/app-service-mobile-ios-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
