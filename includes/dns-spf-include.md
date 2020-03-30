---
author: vhorne
ms.service: dns
ms.topic: include
ms.date: 11/25/2018
ms.author: victorh
ms.openlocfilehash: 9cc650cea17acb8d89933c819c4ca60e2c459d1c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67189002"
---
Gönderen ilkesi çerçevesi (SPF) kayıtları, bir etki alanı adı adına hangi e-posta sunucularının e-posta gönderebileceğini belirtmek için kullanılır. SPF kayıtlarının doğru yapılandırması, alıcıların e-postanızı önemsiz olarak işaretlemesini önlemek için önemlidir.

DNS RFC'ler başlangıçta bu senaryoyu desteklemek için yeni bir SPF kayıt türü tanıttı. Eski ad sunucularını desteklemek için, SPF kayıtlarını belirtmek için TXT kayıt türünün kullanılmasına da izin verdiler. Bu [belirsizlik, RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1)tarafından çözüldü karışıklığa yol açtı. SPF kayıtlarının TXT kayıt türü kullanılarak oluşturulması gerektiğini belirtir. Ayrıca, SPF kayıt türünün amortismana erdiğini belirtir.

**SPF kayıtları Azure DNS tarafından desteklenir ve TXT kayıt türü kullanılarak oluşturulmalıdır.** Eski SPF kayıt türü desteklenmez. Bir [DNS bölge dosyasını içe](../articles/dns/dns-import-export.md)aktardığınızda, SPF kayıt türünü kullanan tüm SPF kayıtları TXT kayıt türüne dönüştürülür.
