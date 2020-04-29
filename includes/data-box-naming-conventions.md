---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: d965b89659a9e3dc01035221a729f094c336eb5b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "66244570"
---
| Varlık                                       | Kurallar   |
|----------------------------------------------|-----------------------------------------------------|
| Blok Blobu ve Sayfa Blobu için kapsayıcı adları | 3 ile 63 karakter uzunluğunda geçerli bir DNS adı olmalıdır. <br>  Bir harf veya sayı ile başlamalıdır. <br> Yalnızca küçük harf, sayı ve kısa çizgi (-) içerebilir. <br> Kısa çizgiden (-) hemen önce ve sonra bir harf veya rakam gelmelidir. <br> Adlarda ardışık çizgilerden izin verilmez. |
| Azure dosyaları için ad paylaşma                  | Yukarıdakiyle aynı                                          |
| Azure dosyaları için dizin ve dosya adları     |<li> Büyük küçük harf koruma, büyük/küçük harfe duyarsız ve 255 karakter uzunluğunda olmalıdır. </li><li> Eğik çizgi (/) ile bitemez. </li><li>Sağlanmışsa, otomatik olarak kaldırılır. </li><li> Şu karakterlere izin verilmez:<code>" \\ / : \| < > * ?</code></li><li> Ayrılmış URL karakterleri doğru şekilde atlanmalıdır. </li><li> Geçersiz URL yolu karakterlerine izin verilmiyor. UE000 gibi \\kod noktaları geçerli Unicode karakterler değildir. Denetim karakterleri (0x00-0x1F, \\u0081, vb.) gıbı bazı ASCII veya Unicode karakterlere de izin verilmez. HTTP/1.1 'de Unicode dizelerini yöneten kurallar için bkz. RFC 2616, Bölüm 2,2: temel kurallar ve RFC 3987. </li><li> Şu dosya adlarına izin verilmiyor: LPT1, LPT2, LPT3, LPT4, LPT5, LPT6, LPT7, LPT8, LPT9, COM1, COM2, COM3, COM4, COM5, COM6, COM7, COM8, COM9, PRN, AUX, NUL, CON, CLOCK $, Dot karakteri (.) ve iki nokta karakteri (..).</li>|
| Blok blobu ve sayfa blobu için blob adları      | </li><li>Blob adları büyük/küçük harfe duyarlıdır ve karakterler herhangi bir düzende sıralanabilir. </li><li>Blob adı 1 ila 1024 karakter uzunluğunda olmalıdır. </li><li>Ayrılmış URL karakterleri doğru şekilde atlanmalıdır. </li><li>Blob adını oluşturan yolun bölümleri 254 karakterden uzun olamaz. Yol bölümü, arka arkaya gelen sınırlayıcı karakterlerinin (örneğin eğik çizgi "/") arasında yer alan ve bir sanal dizinin adına karşılık gelen dizedir.</li> |
