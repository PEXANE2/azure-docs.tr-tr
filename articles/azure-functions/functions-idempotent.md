---
title: Aynı giriş için Azure Işlevleri Tasarlama
description: Idempotent olmak için Azure Işlevleri Oluşturma
author: craigshoemaker
ms.author: cshoe
ms.date: 9/12/2019
ms.topic: article
ms.openlocfilehash: 15af60ac5a862e6fb20e65ba6fbb92482420b7c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74226869"
---
# <a name="designing-azure-functions-for-identical-input"></a>Aynı giriş için Azure Işlevleri Tasarlama

Olay odaklı ve ileti tabanlı mimarinin gerçekliği, veri bütünlüğünü ve sistem kararlılığını korurken aynı istekleri kabul etme gereksinimini belirler.

Göstermek için, bir asansör arama düğmesi düşünün. Düğmeye bastığınızda yanar ve yere bir asansör gönderilir. Birkaç dakika sonra lobide başka biri size katılıyor. Bu kişi size gülümsüyor ve ışıklı düğmeye ikinci kez basıyor. Asansör çağırma emrinin iktidara geldiğini hatırladığın için gülümseyip kendine kıkırdarsın.

Asansör arama düğmesine ikinci, üçüncü veya dördüncü kez basıldığında nihai sonuç üzerinde hiçbir etkisi yoktur. Düğmeye bastığınızda, kaç kez olursa olsun, asansör katınıza gönderilir. Asansör gibi idempotent sistemler, aynı komutlar kaç kez aynı komutlar verilirse verilsin aynı sonucu doğurur.

Uygulama oluşturma söz konusu olduğunda, aşağıdaki senaryoları göz önünde bulundurun:

- Stok kontrol uygulamanız aynı ürünü birden fazla kez silmeye çalışırsa ne olur?
- Aynı kişi için çalışan kaydı oluşturmak için birden fazla istek varsa, insan kaynakları uygulamanız nasıl çalışır?
- Bankacılık uygulamanız aynı para çekme işlemini yapmak için 100 istek alırsa para nereye gider?

Bir işleve gelen isteklerin aynı komutları alabileceği birçok bağlam vardır. Bazı durumlar şunlardır:

- Aynı isteği birçok kez gönderen ilkeleri yeniden deneyin
- Önbelleğe alınmış komutlar uygulamaya yeniden oynattı
- Birden çok aynı istek gönderen uygulama hataları

Veri bütünlüğünü ve sistem durumunu korumak için, bir idempotent uygulama aşağıdaki davranışları içeren mantık içerir:

- Silme işlemini gerçekleştirmeye çalışmadan önce verilerin varlığını doğrulama
- Oluşturma eylemi yürütmeye çalışmadan önce verilerin zaten var olup olmadığını denetleme
- Verilerde nihai tutarlılık oluşturan mutabakat mantığı
- Eşzamanlılık denetimleri
- Çoğaltma algılama
- Veri tazeliği doğrulaması
- Giriş verilerini doğrulamak için tutma mantığı

Sonuçta idempotency belirli bir eylem mümkün olduğundan emin olmak ve sadece bir kez yürütülür elde edilir.
