---
title: Azure Event Grid-sorun giderme kılavuzu
description: Bu makale, hata kodlarının, hata iletilerinin, açıklamaların ve önerilen eylemlerin bir listesini sağlar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: spelluru
ms.openlocfilehash: 1ab9aeac0bde21e229fdb57b7ad02d5d48471551
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75645081"
---
# <a name="troubleshoot-azure-event-grid-errors"></a>Azure Event Grid hatalarında sorun giderme
Bu sorun giderme kılavuzu, Azure Event Grid hata kodlarının, hata iletilerinin, açıklamalarının ve bu hataları aldığınızda gerçekleştirmeniz gereken önerilen eylemlerin bir listesini sağlar. 

## <a name="error-code-400"></a>Hata kodu: 400
| Hata kodu | Hata iletisi | Açıklama | Öneri |
| ---------- | ------------- | ----------- | -------------- | 
| HttpStatusCode. BadRequest<br/>400 | Konu adı 3 ila 50 karakter uzunluğunda olmalıdır. | Özel konu adı uzunluğu 3 ila 50 karakter uzunluğunda olmalıdır. Konu adında yalnızca alfasayısal harfler, rakamlar ve '-' karakterine izin verilir. Ayrıca, ad aşağıdaki ayrılmış sözcüklerle başlamamalıdır: <ul><li>Microsoft</li><li>EventGrid</li><li>Sistem</li></ul> | Konu adı gereksinimlerine uygun olan farklı bir konu adı seçin. |
| HttpStatusCode. BadRequest<br/>400 | Etki alanı adı 3 ila 50 karakter uzunluğunda olmalıdır. | Etki alanı adının uzunluğu 3 ila 50 karakter uzunluğunda olmalıdır. Konu adında yalnızca alfasayısal harfler, rakamlar ve '-' karakterine izin verilir. Ayrıca, ad aşağıdaki ayrılmış sözcüklerle başlamamalıdır:<ul><li>Microsoft</li><li>EventGrid</li><li>Sistem</li> | Etki alanı adı gereksinimlerine uygun farklı bir etki alanı adı seçin. |
| HttpStatusCode. BadRequest<br/>400 | Geçersiz süre sonu zamanı. | Olay aboneliğinin sona erme saati, olay aboneliğinin ne zaman devre dışı olacağını belirler. Bu değer, gelecekte geçerli bir DateTime değeri olmalıdır.| Olay aboneliği zaman aşımı süresinin geçerli bir tarih saat biçiminde ve gelecekte olacak şekilde ayarlandığından emin olun. |

## <a name="error-code-409"></a>Hata kodu: 409
| Hata kodu | Hata iletisi | Açıklama | Önerilen eylem |
| ---------- | ------------- | ----------- | -------------- | 
| HttpStatusCode. Conflict <br/>409 | Belirtilen ada sahip bir konu zaten var. Farklı bir konu adı seçin.   | Özel konu adı, doğru bir yayımlama işleminin sağlamak için tek bir Azure bölgesinde benzersiz olmalıdır. Aynı ad, farklı Azure bölgelerinde kullanılabilir. | Konu için farklı bir ad seçin. |
| HttpStatusCode. Conflict <br/> 409 | Belirtilen etki alanı zaten var. Farklı bir etki alanı adı seçin. | Doğru bir yayımlama işlemi sağlamak için etki alanı adının tek bir Azure bölgesinde benzersiz olması gerekir. Aynı ad, farklı Azure bölgelerinde kullanılabilir. | Etki alanı için farklı bir ad seçin. |
| HttpStatusCode. Conflict<br/>409 | Kota sınırına ulaşıldı. Bu limitlerin hakkında daha fazla bilgi için bkz. [Azure Event Grid sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md#event-grid-limits).  | Her Azure aboneliğinin kullanabileceği Azure Event Grid kaynak sayısı sınırlıdır. Bu kotanın bazıları veya tümü aşılmıştı ve daha fazla kaynak oluşturulamadı. |    Geçerli kaynak kullanımınızı denetleyin ve gerekmeyen kaynakları silin. Kotayı artırmanız gerekiyorsa, tam olarak [aeg@microsoft.com](mailto:aeg@microsoft.com) gereken kaynak sayısıyla bir e-posta gönderin. |


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma ihtiyacınız varsa sorununuzu [Stack Overflow forumuna](https://stackoverflow.com/questions/tagged/azure-eventgrid) gönderin veya bir [destek bileti](https://azure.microsoft.com/support/options/)açın. 
