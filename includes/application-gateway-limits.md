---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 03/04/2020
ms.author: victorh
ms.openlocfilehash: a3fb3dbf026a696b9d472efcba139c371ff1e587
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334928"
---
| Kaynak | Sınır | Not |
| --- | --- | --- |
| Azure Application Gateway |Abonelik başına 1.000 | |
| Ön uç IP yapılandırmaları |2 |1 ortak ve 1 özel |
| Ön uç bağlantı noktaları |100<sup>1</sup> | |
| Arka uç adres havuzları |100<sup>1</sup> | |
| Havuz başına arka uç sunucuları |1.200 | |
| HTTP dinleyici |200<sup>100 1</sup> |Trafiği yönlendirme yapan 100 etkin dinleyiciyle sınırlıdır. Aktif dinleyici = toplam dinleyici sayısı - dinleyici etkin değil.<br>Yönlendirme kuralı içindeki varsayılan yapılandırma trafiği yönlendirmek için ayarlanmışsa (örneğin, bir dinleyici, bir arka uç havuzu ve HTTP ayarları vardır) bu da dinleyici olarak sayılır.|
| HTTP yük dengeleme kuralları |100<sup>1</sup> | |
| Arka uç HTTP ayarları |100<sup>1</sup> | |
| Ağ geçidi başına örnek |V1 SKU - 32<br>V2 SKU - 125 | |
| SSL sertifikaları |100<sup>1</sup> |HTTP dinleyicibaşına 1 |
| Maksimum SSL sertifika boyutu |V1 SKU - 10 KB<br>V2 SKU - 16 KB| |
| Kimlik doğrulama sertifikaları |100 | |
| Güvenilen kök sertifikalar |100 | |
| Zaman alakart en az istekte bulunun |1 saniye | |
| Zaman alakart en fazla istekte bulunun |24 saat | |
| Site sayısı |100<sup>1</sup> |HTTP dinleyicibaşına 1 |
| Dinleyici başına URL haritaları |1 | |
| URL haritası başına en yüksek yol tabanlı kurallar|100||
| Yapılandırmaları yeniden yönlendirme |100<sup>1</sup>| |
| Eşzamanlı WebSocket bağlantıları |Orta ağ geçitleri 20k<br> Büyük ağ geçitleri 50k| |
| Maksimum URL uzunluğu|32 KB| |
| HTTP/2 için maksimum üstbilgi boyutu |4KB| |
| Maksimum dosya yükleme boyutu, Standart |2 GB | |
| Maksimum dosya yükleme boyutu WAF |V1 Orta WAF ağ geçitleri, 100 MB<br>V1 Büyük WAF ağ geçitleri, 500 MB<br>V2 WAF, 750 MB| |
| WAF gövde boyutu sınırı, dosya olmadan|128 KB||
| Maksimum WAF özel kuralları|100||
| Maksimum WAF hariç tutma|100||

<sup>1</sup> WAF özellikli SNU'larda, en iyi performans için kaynak sayısını 40 ile sınırlamanızı öneririz.
