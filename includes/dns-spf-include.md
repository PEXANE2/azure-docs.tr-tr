---
author: vhorne
ms.service: dns
ms.topic: include
ms.date: 11/25/2018
ms.author: victorh
ms.openlocfilehash: 9cc650cea17acb8d89933c819c4ca60e2c459d1c
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "67189002"
---
Gönderen ilkesi çerçevesi (SPF) kayıtları, hangi e-posta sunucularının bir etki alanı adı adına e-posta gönderebileceği belirlemek için kullanılır. SPF kayıtlarının doğru yapılandırması, alıcıların e-postanızı gereksiz olarak işaretlemesini engellemek için önemlidir.

DNS RFC 'Leri başlangıçta bu senaryoyu desteklemek için yeni bir SPF kayıt türü sunmuştur. Eski ad sunucularını desteklemek için, Ayrıca, SPF kayıtlarını belirtmek üzere TXT kayıt türünün kullanımına izin verilir. Bu belirsizlik, [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1)tarafından çözümlenen karışıklıklara yol açmıştır. SPF kayıtlarının TXT kayıt türü kullanılarak oluşturulması gerektiğini belirtir. Ayrıca, SPF kayıt türünün kullanım dışı olduğunu da belirtir.

**SPF kayıtları Azure DNS tarafından desteklenir ve TXT kayıt türü kullanılarak oluşturulması gerekir.** Kullanılmayan SPF kayıt türü desteklenmiyor. [BIR DNS bölge dosyasını içeri](../articles/dns/dns-import-export.md)AKTARDıĞıNıZDA, SPF kayıt türünü kullanan tüm SPF KAYıTLARı, txt kayıt türüne dönüştürülür.
