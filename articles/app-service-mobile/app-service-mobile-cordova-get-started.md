---
title: Cordova uygulaması oluşturma
description: Apache Cordova geliştirme için Azure mobil uygulaması arka uçlarını kullanmaya başlamak için bu öğreticiden yararlanın.
keywords: cordova,javascript,mobil,istemci
ms.assetid: 0b08fc12-0a80-42d3-9cc1-9b3f8d3e3a3f
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: conceptual
ms.date: 06/25/2019
ms.openlocfilehash: 99fb4a4b07ecbd4a85abbc62ec52a0f5960654c5
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77461581"
---
# <a name="create-an-apache-cordova-app"></a>Apache Cordova uygulaması oluşturma
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, bir Apache Cordova mobil uygulamasına Azure mobil uygulaması arka ucunu kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir.  Yeni bir mobil uygulama arka ucu ve uygulama verilerini Azure'da depolayan basit bir *Yapılacaklar listesi* Apache Cordova uygulaması oluşturacaksınız.

Bu öğreticiyi tamamlamak, Azure App Service’de Mobile Apps özelliğini kullanmayla ilgili diğer tüm Apache Cordova öğreticileri için ön koşuldur.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* [Visual Studio Community 2017] ya da daha yeni sürümünü içeren bir bilgisayar.
* [Apache Cordova için Visual Studio Araçları].
* [Etkin bir Azure hesabı](https://azure.microsoft.com/pricing/free-trial/).

Ayrıca Visual Studio’yu atlayabilir ve doğrudan Apache Cordova komut satırını kullanabilirsiniz.  Komut satırını kullanmak, öğreticiyi bir Mac bilgisayarda tamamladığınızda kullanışlıdır.  Komut satırını kullanarak Apache Cordova istemci uygulamalarını derleme bu öğretici kapsamında değildir.

## <a name="create-an-azure-mobile-app-backend"></a>Azure mobil uygulama arka ucu oluşturma
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Bir veritabanı bağlantısı oluşturma ve istemciyi ve sunucu projesini yapılandırma
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>Apache Cordova uygulamasını indirme ve çalıştırma
[!INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/

[Visual Studio Community 2017]: https://www.visualstudio.com/
[Apache Cordova için Visual Studio Araçları]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
