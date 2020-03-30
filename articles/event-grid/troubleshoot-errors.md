---
title: Azure Olay Kılavuzu - Sorun Giderme kılavuzu
description: Bu makalede, hata kodları, hata iletileri, açıklamalar ve önerilen eylemlerin bir listesini sağlar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: spelluru
ms.openlocfilehash: 1ab9aeac0bde21e229fdb57b7ad02d5d48471551
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645081"
---
# <a name="troubleshoot-azure-event-grid-errors"></a>Azure Olay Kılavuz hatalarını giderme
Bu sorun giderme kılavuzu, Azure Olay Kılavuzu hata kodlarının, hata iletilerinin, açıklamalarının ve bu hataları aldığınızda almanız gereken önerilen eylemlerin bir listesini sağlar. 

## <a name="error-code-400"></a>Hata kodu: 400
| Hata kodu | Hata iletisi | Açıklama | Öneri |
| ---------- | ------------- | ----------- | -------------- | 
| httpstatuscode.badrequest<br/>400 | Konu adı uzunluğu 3 ile 50 karakter arasında olmalıdır. | Özel konu adı uzunluğu uzunluğu 3 ile 50 karakter arasında olmalıdır. Konu adına yalnızca alfasayısal harflere, basamaklara ve '-' karakterine izin verilir. Ayrıca, ad aşağıdaki ayrılmış sözcüklerle başlamamalıdır: <ul><li>Microsoft</li><li>EventGrid</li><li>Sistem</li></ul> | Konu adı gereksinimlerine uyan farklı bir konu adı seçin. |
| httpstatuscode.badrequest<br/>400 | Etki alanı adı uzunluğu 3 ile 50 karakter arasında olmalıdır. | Etki alanı adı uzunluğu 3 ile 50 karakter arasında olmalıdır. Konu adına yalnızca alfasayısal harflere, basamaklara ve '-' karakterine izin verilir. Ayrıca, ad aşağıdaki ayrılmış sözcüklerle başlamamalıdır:<ul><li>Microsoft</li><li>EventGrid</li><li>Sistem</li> | Etki alanı adı gereksinimlerine uyan farklı bir etki alanı adı seçin. |
| httpstatuscode.badrequest<br/>400 | Geçersiz son kullanma süresi. | Olay aboneliğinin sona erme süresi, etkinlik aboneliğinin ne zaman emekli olacağını belirler. Bu değer gelecekte geçerli bir DateTime değeri olmalıdır.| Olay Aboneliği'nin geçerli bir DateTime biçiminde sona erme süresinin sona erdiğinden ve gelecekte de ayarlandığından emin olun. |

## <a name="error-code-409"></a>Hata kodu: 409
| Hata kodu | Hata iletisi | Açıklama | Önerilen eylem |
| ---------- | ------------- | ----------- | -------------- | 
| httpStatusCode.Conflict <br/>409 | Belirtilen adı taşıyan konu zaten var. Farklı bir konu adı seçin.   | Doğru bir yayımlama işlemi sağlamak için özel konu adı tek bir Azure bölgesinde benzersiz olmalıdır. Aynı ad farklı Azure bölgelerinde kullanılabilir. | Konu için farklı bir ad seçin. |
| httpStatusCode.Conflict <br/> 409 | Belirtilen etki alanı zaten var. Farklı bir etki alanı adı seçin. | Doğru bir yayımlama işlemi sağlamak için etki alanı adı tek bir Azure bölgesinde benzersiz olmalıdır. Aynı ad farklı Azure bölgelerinde kullanılabilir. | Etki alanı için farklı bir ad seçin. |
| httpStatusCode.Conflict<br/>409 | Kota sınırına ulaşıldı. Bu sınırlar hakkında daha fazla bilgi için [Azure Olay Kılavuz sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md#event-grid-limits)bakın.  | Her Azure aboneliğinin kullanabileceği Azure Olay Ağı kaynaklarının sayısında bir sınırı vardır. Bu kotanın bir kısmı veya tamamı aşıldı ve başka kaynak oluşturulamadı. |    Geçerli kaynak kullanımınızı kontrol edin ve gerekli olmayan ları silin. Kotanızı hala artırmanız gerekiyorsa, gereken [aeg@microsoft.com](mailto:aeg@microsoft.com) tam kaynak sayısını içeren bir e-posta gönderin. |


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma ihtiyacınız varsa, sorununuzu [Stack Overflow forumunda](https://stackoverflow.com/questions/tagged/azure-eventgrid) yayınla veya bir [destek bileti](https://azure.microsoft.com/support/options/)açın. 
