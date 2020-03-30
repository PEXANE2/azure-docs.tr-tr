---
title: Azure Olay Izgara SDK'ları
description: Azure Olay Idamı için SDK'ları açıklar. Bu SDK'lar yönetim, yayıncılık ve tüketim sağlar.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 01/19/2019
ms.author: spelluru
ms.openlocfilehash: 7f05665f4bcc5449c1a81fa24582b333b0a944e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60822839"
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>Yönetim ve yayıncılık için Olay Izgara SDK'ları

Olay Izgara, kaynaklarınızı programlı bir şekilde yönetmenize ve olayları yayınlamanıza olanak tanıyan SDK'lar sağlar.

## <a name="management-sdks"></a>Yönetim SDK'ları

Yönetim SDK'ları, olay ızgarası konularını ve aboneliklerini oluşturmanıza, güncelleştirmenize ve silmenize olanak tanır. Şu anda, aşağıdaki SDK'lar kullanılabilir:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Git](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://search.maven.org/#search%7Cga%7C1%7Cazure-mgmt-eventgrid)
* [Node](https://www.npmjs.com/package/azure-arm-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-mgmt-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_mgmt_event_grid)

## <a name="data-plane-sdks"></a>Veri düzlemi SDK'ları

Veri düzlemi SDK'ları, kimlik doğrulaması, olayı oluşturma ve belirtilen bitiş noktasına eş senkronize bir şekilde deftere naklederek olayları konulara göndermenize olanak tanır. Ayrıca ilk parti etkinliklerini tüketmenizi de sağlar. Şu anda, aşağıdaki SDK'lar kullanılabilir:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.EventGrid)
* [Git](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventgrid)
* [Node](https://www.npmjs.com/package/azure-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_event_grid)

## <a name="next-steps"></a>Sonraki adımlar

* Örneğin uygulamalar, [bkz. Olay Izgara kodu örnekleri.](https://azure.microsoft.com/resources/samples/?sort=0&service=event-grid)
* Olay Izgarasına giriş için [olay ızgarası nedir'e bakın?](overview.md)
* Azure CLI'deki Olay Izgara komutları için Azure [CLI'ye](/cli/azure/eventgrid)bakın.
* PowerShell'deki Olay Izgara komutları için [PowerShell'e](/powershell/module/az.eventgrid)bakın.
