---
title: Azure Uygulama Hizmeti Mobile Apps’de iOS uygulaması oluşturma | Microsoft Belgeleri
description: Objective-C ya da Swift’te iOS geliştirme için Azure mobil uygulaması arka uçlarını kullanmaya başlamak için bu öğreticiden yararlanın.
services: app-service\mobile
documentationcenter: ios
author: elamalani
manager: crdun
editor: ''
ms.assetid: 6461a899-9340-42dd-b118-ffc5ba00e846
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: cdfe395bca7ac73f00506da0180146310bd71d3d
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72027238"
---
# <a name="create-an-ios-app"></a>iOS uygulaması oluşturun

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

> [!NOTE]
> Visual Studio App Center, mobil uygulama geliştirmeye kadar uçtan uca ve tümleşik hizmetler merkezi 'ni destekler. Geliştiriciler, sürekli tümleştirme ve teslim işlem hattı ayarlamak için **oluşturma**, **Test** etme ve **dağıtma** hizmetlerini kullanabilir. Uygulama dağıtıldıktan sonra, geliştiriciler **analiz** ve **Tanılama** hizmetlerini kullanarak uygulamasının durumunu ve kullanımını izleyebilir ve **Push** hizmetini kullanarak kullanıcılarla etkileşime geçebilir. Geliştiriciler, uygulama verilerini bulutta kalıcı hale getirmek ve eşitlemek için kullanıcıların ve **veri** hizmetinin kimliklerini doğrulamak üzere **kimlik** doğrulamasından faydalanabilir.
> Mobil uygulamanızda bulut hizmetlerini tümleştirmek istiyorsanız bugün App Center [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc) kaydolun.

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
