---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: d965b89659a9e3dc01035221a729f094c336eb5b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "66244570"
---
| Varlık                                       | Kurallar   |
|----------------------------------------------|-----------------------------------------------------|
| Blok blob ve sayfa blob için konteyner adları | 3 ila 63 karakter uzunluğunda geçerli bir DNS adı olmalıdır. <br>  Bir harf veya sayı ile başlamalıdır. <br> Yalnızca küçük harfler, sayılar ve tire (-) içerebilir. <br> Kısa çizgiden (-) hemen önce ve sonra bir harf veya rakam gelmelidir. <br> Ardışık tirelerin adlarına izin verilmez. |
| Azure dosyaları için adları paylaşma                  | Yukarıdakiyle aynı                                          |
| Azure dosyaları için dizin ve dosya adları     |<li> Büyük/küçük harf koruma, büyük/küçük harf duyarsız ve uzunluğu 255 karakter geçmemelidir. </li><li> İleri eğik çizgi (/) ile sona eremez. </li><li>Sağlanırsa, otomatik olarak kaldırılır. </li><li> Aşağıdaki karakterlere izin verilmez:<code>" \\ / : \| < > * ?</code></li><li> Ayrılmış URL karakterleri doğru şekilde atlanmalıdır. </li><li> Yasa dışı URL yolu karakterlerine izin verilmez. uE000 gibi \\kod noktaları geçerli Unicode karakterleri değildir. Kontrol karakterleri (0x00 - 0x1F, \\u0081, vb.) gibi bazı ASCII veya Unicode karakterlerine de izin verilmez. HTTP/1.1'deki Unicode dizelerini yöneten kurallar için bkz: RFC 2616, Bölüm 2.2: Temel Kurallar ve RFC 3987. </li><li> Aşağıdaki dosya adlarına izin verilmez: LPT1, LPT2, LPT3, LPT4, LPT7, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM7, COM8, COM9, PRN, AUX, NUL, CON, CLOCK$, dot karakter (.) ve iki nokta karakteri (.).</li>|
| Blok blobu ve sayfa blobu için blob adları      | </li><li>Blob adları büyük/küçük harfe duyarlıdır ve karakterler herhangi bir düzende sıralanabilir. </li><li>Blob adı 1 ila 1024 karakter uzunluğunda olmalıdır. </li><li>Ayrılmış URL karakterleri doğru şekilde atlanmalıdır. </li><li>Blob adını oluşturan yolun bölümleri 254 karakterden uzun olamaz. Yol bölümü, arka arkaya gelen sınırlayıcı karakterlerinin (örneğin eğik çizgi "/") arasında yer alan ve bir sanal dizinin adına karşılık gelen dizedir.</li> |
