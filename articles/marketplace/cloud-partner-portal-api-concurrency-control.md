---
title: Eşzamanlılık denetimi-Azure Marketi
description: Bulut İş Ortağı Portalı yayımlama API 'Leri için eşzamanlılık denetimi stratejileri.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: mingshen-ms
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 2ec2e701a1d9cb449939416e65cf645f8bc01887
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87287899"
---
# <a name="concurrency-control"></a>Eşzamanlılık denetimi

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri ile tümleşiktir ve Iş Ortağı Merkezi 'nde çalışmaya devam edecektir. Geçiş küçük değişiklikler sunar. Iş Ortağı Merkezi 'ne geçtikten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](./cloud-partner-portal-api-overview.md) listelenen değişiklikleri gözden geçirin. CPP API 'Leri yalnızca Iş Ortağı Merkezi 'ne geçişten önce tümleştirilmiş mevcut ürünler için kullanılmalıdır; Yeni ürünlerin Iş Ortağı Merkezi gönderme API 'Leri kullanması gerekir.

Bulut İş Ortağı Portalı yayımlama API 'Lerine yapılan her çağrının, hangi eşzamanlılık denetimi stratejisinin kullanılacağını açıkça belirtmesi gerekir. **IF-Match** üst bilgisinin SAĞLANMASı, HTTP 400 hata yanıtına neden olur. Eşzamanlılık denetimi için iki strateji sunuyoruz.

-   **İyimser** -güncelleştirmeyi gerçekleştiren istemci, verilerin son okumasından bu yana verilerin değişip değişmediğini doğrular.
-   **Son bir WINS** -istemci, son okuma zamanından bu yana başka bir uygulamanın onu değiştirmediğine bakılmaksızın verileri doğrudan güncelleştirir.

<a name="optimistic-concurrency-workflow"></a>İyimser eşzamanlılık iş akışı
-------------------------------

Kaynaklarınızda beklenmedik bir düzenleme yapılyapılmadığını garanti etmek için, aşağıdaki iş akışı ile iyimser eşzamanlılık stratejisini kullanmanızı öneririz.

1.  API 'Leri kullanarak bir varlığı alma. Yanıt, varlığın Şu anda depolanmış sürümünü tanımlayan bir ETag değeri içerir (Yanıt sırasında).
2.  Güncelleştirme sırasında, zorunlu **IF-Match** istek üst bilgisine aynı ETag değerini ekleyin.
3.  API, istekte alınan ETag değerini, bir atomik işlem içindeki varlığın geçerli ETag değeri ile karşılaştırır.
    *   ETag değerleri farklıysa, API bir `412 Precondition Failed` http yanıtı döndürür. Bu hata, istemcinin en son aldığı bu yana başka bir işlemin varlığı güncelleştirdiğini veya istekte belirtilen ETag değerinin yanlış olduğunu gösterir.
    *  ETag değerleri aynıysa veya **IF-Match** üst bilgisi () joker karakteri IÇERIYORSA `*` , API istenen işlemi gerçekleştirir. API işlemi ayrıca varlığın depolanan ETag değerini güncelleştirir.


> [!NOTE]
> **IF-Match** üst bilgisinde joker karakteri (*) BELIRTMEK, API 'nin son bir WINS eşzamanlılık stratejisini kullanarak sonuçlanır. Bu durumda, ETag karşılaştırması gerçekleşmez ve kaynak herhangi bir denetim olmadan güncelleştirilir. 
