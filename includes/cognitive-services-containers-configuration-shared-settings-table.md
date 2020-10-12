---
author: IEvangelist
ms.author: dapine
ms.date: 10/02/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 7ccbc6c06419d22add7c52829069bb858cb35cf7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "73484097"
---
Kapsayıcı aşağıdaki yapılandırma ayarlarına sahiptir:

|Gerekli|Ayar|Amaç|
|--|--|--|
|Evet|[ApiKey](#apikey-configuration-setting)|Fatura bilgilerini izler.|
|Hayır|[ApplicationInsights](#applicationinsights-setting)|, Kapsayıcınıza [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) telemetri desteğinin eklenmesini sağlar.|
|Evet|[Faturalandırma](#billing-configuration-setting)|Azure üzerindeki hizmet kaynağının uç nokta URI 'sini belirtir.|
|Evet|[Sözleşmesi](#eula-setting)| Kapsayıcının lisansını kabul ettiğinizi gösterir.|
|Hayır|[Fluentd](#fluentd-settings)|Günlük ve isteğe bağlı olarak ölçüm verilerini Floentd sunucusuna yazar.|
|Hayır|HTTP proxy 'Si|Giden istekler oluşturmak için bir HTTP proxy yapılandırır.|
|Hayır|[Günlüğe kaydetme](#logging-settings)|, Kapsayıcınız için ASP.NET Core günlük desteği sağlar. |
|Hayır|[Kullan](#mount-settings)|Ana bilgisayardan kapsayıcıya ve kapsayıcıdan ana bilgisayara geri veri okur ve yazar.|
