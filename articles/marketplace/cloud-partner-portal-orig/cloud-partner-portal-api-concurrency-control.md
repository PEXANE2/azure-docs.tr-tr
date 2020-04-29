---
title: Eşzamanlılık denetimi | Azure Marketi
description: Bulut İş Ortağı Portalı yayımlama API 'Leri için eşzamanlılık denetimi stratejileri.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: 302ba8d550f5e91efe12c620d766550958d3bf68
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81256408"
---
# <a name="concurrency-control"></a>Eşzamanlılık denetimi

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri iş ortağı merkezi ile tümleşiktir ve teklifleriniz iş ortağı merkezi 'ne geçirildikten sonra çalışmaya devam edecektir. Tümleştirme küçük değişiklikler sunar. İş Ortağı Merkezi 'ne geçişten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) listelenen değişiklikleri gözden geçirin.

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
    *  ETag değerleri aynıysa veya **IF-Match** üst bilgisi (`*`) joker karakteri içeriyorsa, API istenen işlemi gerçekleştirir. API işlemi ayrıca varlığın depolanan ETag değerini güncelleştirir.


> [!NOTE]
> **IF-Match** üst bilgisinde joker karakteri (*) BELIRTMEK, API 'nin son bir WINS eşzamanlılık stratejisini kullanarak sonuçlanır. Bu durumda, ETag karşılaştırması gerçekleşmez ve kaynak herhangi bir denetim olmadan güncelleştirilir. 
