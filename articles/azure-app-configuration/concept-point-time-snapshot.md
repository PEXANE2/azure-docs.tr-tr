---
title: Azure uygulama yapılandırma noktası anlık görüntüsü
description: Azure Uygulama yapılandırmasında, noktadan noktaya görüntünün nasıl çalıştığına ilişkin genel bakış
services: azure-app-configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.openlocfilehash: 4a352ba913b6ad4e3c8607677078e21070f294fd
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76899582"
---
# <a name="point-in-time-snapshot"></a>Belirli bir noktanın anlık görüntüsü

Azure Uygulama yapılandırması, yeni bir anahtar-değer çifti oluşturulduğunda ve sonra değiştirildiğinde kesin saatlerin kayıtlarını tutar. Bu kayıtlar, anahtar-değer değişikliklerinde tam bir zaman çizelgesi oluşturur. Bir uygulama yapılandırma deposu herhangi bir anahtar değerinin geçmişini yeniden oluşturabilir ve mevcut herhangi bir anda onun yerine geçmiş değerini yeniden çalabilirsiniz. Bu özellikle, "zaman gezme" geri ve eski bir anahtar değeri alabilirsiniz. Örneğin, önceki bir yapılandırmayı kurtarmanız ve uygulamayı geri almanız için en son dağıtımdan hemen önce, dün 'nin yapılandırma ayarlarını edinebilirsiniz.

## <a name="key-value-retrieval"></a>Anahtar-değer alma

Geçmiş anahtar değerlerini almak için, bir REST API çağrısının HTTP üstbilgisinde anahtar değerlerinin anlık görüntü olduğu bir saat belirtin. Örneğin:

```rest
GET /kv HTTP/1.1
Accept-Datetime: Sat, 1 Jan 2019 02:10:00 GMT
```

Şu anda, uygulama yapılandırması yedi gün değişiklik geçmişini tutar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [ASP.NET Core Web uygulaması oluşturma](./quickstart-aspnet-core-app.md)  
