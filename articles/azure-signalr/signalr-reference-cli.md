---
title: Azure CLI Örnekleri - Azure SignalR Hizmeti
description: Azure SignalR Hizmeti için Azure CLI'yi öğrenmek için gerçek örnekleri izleyin. Daha fazla Azure hizmetiyle SignalR Hizmeti'ni nasıl oluşturacağınızı öğreneceksiniz.
author: sffamily
ms.service: signalr
ms.topic: reference
ms.date: 11/13/2019
ms.author: zhshang
ms.openlocfilehash: 3c28781362e60d22b0d9a4e3754daaa24e5b61ee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74158135"
---
# <a name="azure-cli-reference"></a>Azure CLI başvurusu

Aşağıdaki tablo, Azure CLI kullanılarak Azure SignalR Hizmeti için bash betiklerine yönelik bağlantılar içerir.

| | |
|-|-|
|**Oluştur**||
| [Yeni bir SignalR Hizmeti ve kaynak grubu oluşturma](scripts/signalr-cli-create-service.md) | Rasgele bir ad ile yeni bir kaynak grubunda yeni bir Azure SignalR Hizmeti kaynağı oluşturur.  |
|**Tümleştirme**||
| [SignalR kullanmak üzere yapılandırılmış yeni bir SignalR Hizmeti ve Web Uygulaması oluşturma](scripts/signalr-cli-create-with-app-service.md) | Rasgele bir ad ile yeni bir kaynak grubunda yeni bir Azure SignalR Hizmeti kaynağı oluşturur. Ayrıca SignalR Hizmetini kullanan ASP.NET Core Web Uygulaması'nı barındırmak için yeni bir Web Uygulaması ve Uygulama Hizmeti planı ekler. Web uygulaması, yeni SignalR hizmet kaynağına bağlanmak için bir Uygulama Ayarı ile yapılandırılır. |
| [SignalR ve GitHub OAuth kullanmak üzere yapılandırılmış yeni bir SignalR Hizmeti ve Web Uygulaması oluşturma](scripts/signalr-cli-create-with-app-service-github-oauth.md) | Rasgele bir ad ile yeni bir kaynak grubunda yeni bir Azure SignalR Hizmeti kaynağı oluşturur. Ayrıca, SignalR Hizmetini kullanan ASP.NET Core Web Uygulaması'nı barındırmak için yeni bir Azure Web Uygulaması ve barındırma planı ekler. Web uygulaması, [kimlik doğrulaması öğreticisinde](signalr-concept-authenticate-oauth.md) gösterildiği gibi, [GitHub kimlik doğrulamasını](https://developer.github.com/v3/guides/basics-of-authentication/) desteklemek amacıyla yeni SignalR hizmeti kaynağının bağlantı dizesi ve istemci gizli dizisi için uygulama ayarlarıyla yapılandırılır. Web uygulaması ayrıca bir yerel git deposu dağıtım kaynağını kullanacak şekilde de yapılandırılır. |
| | |
