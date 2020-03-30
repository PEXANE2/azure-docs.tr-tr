---
title: Eşzamanlılık Kontrolü | Azure Marketi
description: Bulut İş Ortağı Portalı yayımlama API'leri için eşzamanlılık kontrol stratejileri.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 6d0f035d77e74f157b793b9edf3ab5d3494096d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288657"
---
# <a name="concurrency-control"></a>Eşzamanlılık Kontrolü

Bulut İş Ortağı Portalı yayımlama API'lerine yapılan her çağrıda hangi eşzamanlılık denetim stratejisinin kullanılacağı açıkça belirtilmelidir. **If-Match** üstbilgisinin sağlanamaması, HTTP 400 hata yanıtına neden olur. Eşzamanlılık kontrolü için iki strateji sunuyoruz.

-   **İyimser** - Güncelleştirmeyi gerçekleştiren istemci, verileri en son okuduğundan beri verilerin değişip değişmediğini doğrular.
-   **Sonuncusu kazanır** - İstemci, başka bir uygulamanın son okunduk zamana göre değiştirip değiştirmediğine bakılmaksızın verileri doğrudan güncelleştirir.

<a name="optimistic-concurrency-workflow"></a>İyimser eşzamanlılık iş akışı
-------------------------------

Kaynaklarınızda beklenmeyen bir işlem yapılmadığını garanti etmek için aşağıdaki iş akışıyla iyimser eşzamanlılık stratejisini kullanmanızı öneririz.

1.  API'leri kullanarak bir varlık alın. Yanıt, varlığın şu anda depolanan sürümünü tanımlayan bir ETag değeri içerir (yanıt sırasında).
2.  Güncelleştirme sırasında, zorunlu **If-Match** istek üstbilgisinde aynı ETag değerini ekleyin.
3.  API, istekte alınan ETag değerini atomik bir işlemdeki varlığın geçerli ETag değeriyle karşılaştırır.
    *   ETag değerleri farklıysa, API bir `412 Precondition Failed` HTTP yanıtı döndürür. Bu hata, istemci en son alındığı günden beri başka bir işlemin varlığı güncelleştirdiğini veya istekte belirtilen ETag değerinin yanlış olduğunu gösterir.
    *  ETag değerleri aynıysa veya **If-Match** üstbilgisi joker yıldız karakterini`*`içeriyorsa ( ), API istenen işlemi gerçekleştirir. API işlemi, varlığın depolanan ETag değerini de güncelleştirir.


> [!NOTE]
> **If-Match** üstbilgisinde joker karakter (*) belirtilmesi, Son bir-kazanç eşzamanlılık stratejisini kullanarak API ile sonuçlanır. Bu durumda, ETag karşılaştırması gerçekleşmez ve kaynak herhangi bir denetim olmadan güncelleştirilir. 
