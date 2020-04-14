---
title: Eşzamanlılık Kontrolü | Azure Marketi
description: Bulut İş Ortağı Portalı yayımlama API'leri için eşzamanlılık kontrol stratejileri.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: 302ba8d550f5e91efe12c620d766550958d3bf68
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81256408"
---
# <a name="concurrency-control"></a>Eşzamanlılık Kontrolü

> [!NOTE]
> Bulut İş Ortağı Portalı API'leri İş Ortağı Merkezi ile entegre edilmiştir ve teklifleriniz İş Ortağı Merkezi'ne geçtikten sonra çalışmaya devam edecektir. Entegrasyon küçük değişiklikler sunar. Kodunuzu İş Ortağı Merkezi'ne geçişten sonra çalışmaya devam etmesini sağlamak için [Bulut İş Ortağı Portalı API Başvurusu'nda](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) listelenen değişiklikleri gözden geçirin.

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
