---
author: IEvangelist
ms.author: dapine
ms.date: 10/02/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 7ccbc6c06419d22add7c52829069bb858cb35cf7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73484097"
---
Kapsayıcı aşağıdaki yapılandırma ayarlarına sahiptir:

|Gerekli|Ayar|Amaç|
|--|--|--|
|Evet|[ApiKey](#apikey-configuration-setting)|Fatura bilgilerini izler.|
|Hayır|[ApplicationInsights](#applicationinsights-setting)|Kapsayıcınıza [Azure Uygulama Öngörüleri](https://docs.microsoft.com/azure/application-insights) telemetri desteği eklemeyi sağlar.|
|Evet|[Faturalama](#billing-configuration-setting)|Azure'daki hizmet kaynağının bitiş noktası URI'yi belirtir.|
|Evet|[Eula](#eula-setting)| Kapsayıcının lisansını kabul ettiğinizi gösterir.|
|Hayır|[Akıcı](#fluentd-settings)|Akıcı bir sunucuya günlük ve isteğe bağlı olarak metrik veri yazar.|
|Hayır|HTTP Proxy|Giden isteklerde bulunmak için bir HTTP proxy'si yapılandırır.|
|Hayır|[Günlüğe Kaydetme](#logging-settings)|Kapsayıcınız için ASP.NET Çekirdek günlük desteği sağlar. |
|Hayır|[Bağlar](#mount-settings)|Ana bilgisayardaki verileri kapsayıcıya ve kapsayıcıdan ana bilgisayara geri okur ve yazar.|
