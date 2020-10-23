---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/04/2020
ms.author: glenga
ms.openlocfilehash: 72588ccfd9ad00305cbdeaf8132999731d86b9b1
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2020
ms.locfileid: "92164937"
---
Her günlüğe bir *günlük düzeyi* atanır. Değer göreli önemi gösteren bir tamsayıdır:

|LogLevel    |Kod| Description |
|------------|---|--------------|
|İzleme       | 0 |En ayrıntılı iletileri içeren Günlükler. Bu iletiler, hassas uygulama verileri içerebilir. Bu iletiler varsayılan olarak devre dışıdır ve bir üretim ortamında hiçbir şekilde etkinleştirilmemelidir.|
|Hata ayıklama       | 1 | Geliştirme sırasında etkileşimli araştırma için kullanılan Günlükler. Bu Günlükler öncelikle hata ayıklama için yararlı bilgiler içermeli ve uzun süreli bir değer içermez. |
|Bilgi | 2 | Uygulamanın genel akışını izleyen Günlükler. Bu günlüklerde uzun vadeli değer bulunmalıdır. |
|Uyarı     | 3 | Uygulama akışında olağan dışı veya beklenmeyen bir olayı vurgulayan Günlükler, ancak başka bir şekilde uygulama yürütmenin durmasına neden olmaz. |
|Hata       | 4 | Bir hata nedeniyle geçerli yürütme akışı durdurulduğunda vurgulanacak Günlükler. Bu hatalar, uygulama genelinde bir hata değil, geçerli etkinliğin bir başarısızlığını göstermelidir. |
|Kritik    | 5 | Kurtarılamaz bir uygulamayı veya sistem kilitlenmesini tanımlayan Günlükler veya hemen ilgilenilmesi gereken çok zararlı bir hata. |
|Hiçbiri        | 6 | Belirtilen kategori için günlüğü devre dışı bırakır. |

Dosya yapılandırmasındaki [ *host.js* ](../articles/azure-functions/functions-host-json.md) , bir işlevler uygulamasının Application Insights ne kadar günlüğe göndereceğini belirler.  
