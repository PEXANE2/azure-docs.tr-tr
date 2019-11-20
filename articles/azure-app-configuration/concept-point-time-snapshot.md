---
title: Azure uygulama yapılandırma noktası anlık görüntüsü | Microsoft Docs
description: Azure Uygulama yapılandırmasında, noktadan noktaya görüntünün nasıl çalıştığına ilişkin genel bakış
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: fdbe4846700c690261dbc734063f4420478666a8
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74185209"
---
# <a name="point-in-time-snapshot"></a>Belirli bir noktanın anlık görüntüsü

Azure Uygulama yapılandırması, yeni bir anahtar-değer çifti oluşturulduğunda ve sonra değiştirildiğinde kesin saatlerin kayıtlarını tutar. Bu kayıtlar, anahtar-değer değişikliklerinde tam bir zaman çizelgesi oluşturur. Bir uygulama yapılandırma deposu herhangi bir anahtar değerinin geçmişini yeniden oluşturabilir ve mevcut herhangi bir anda onun yerine geçmiş değerini yeniden çalabilirsiniz. Bu özellikle, "zaman gezme" geri ve eski bir anahtar değeri alabilirsiniz. Örneğin, önceki bir yapılandırmayı kurtarmanız ve uygulamayı geri almanız için en son dağıtımdan hemen önce, dün 'nin yapılandırma ayarlarını edinebilirsiniz.

## <a name="key-value-retrieval"></a>Anahtar-değer alma

Geçmiş anahtar değerlerini almak için, bir REST API çağrısının HTTP üstbilgisinde anahtar değerlerinin anlık görüntü olduğu bir saat belirtin. Örneğin:

        GET /kv HTTP/1.1
        Accept-Datetime: Sat, 1 Jan 2019 02:10:00 GMT

Şu anda, uygulama yapılandırması yedi gün değişiklik geçmişini tutar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [ASP.NET Core Web uygulaması oluşturma](./quickstart-aspnet-core-app.md)  
