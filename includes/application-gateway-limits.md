---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 6/5/2019
ms.author: victorh
ms.openlocfilehash: 0ba21fe3789fba03cd4814d19fb103c3a2559c13
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75662941"
---
| Kaynak | Varsayılan/en yüksek sınır | Not |
| --- | --- | --- |
| Azure Application Gateway |Abonelik başına 1.000 | |
| Ön uç IP yapılandırması |2 |1 ortak ve 1 özel |
| Ön uç bağlantı noktaları |100<sup>1</sup> | |
| Arka uç adres havuzları |100<sup>1</sup> | |
| Havuz başına arka uç sunucuları |1,200 | |
| HTTP dinleyicileri |100<sup>1</sup> | |
| HTTP Yük Dengeleme kuralları |100<sup>1</sup> | |
| Arka uç HTTP ayarları |100<sup>1</sup> | |
| Ağ geçidi başına örnek |32 | |
| SSL sertifikaları |100<sup>1</sup> |HTTP dinleyicilerine göre 1 |
| En fazla SSL sertifika boyutu |V1 SKU 'SU-10 KB<br>V2 SKU-16 KB| |
| Kimlik doğrulama sertifikaları |100 | |
| Güvenilen kök sertifikalar |100 | |
| İstek zaman aşımı en az |1 saniye | |
| İstek zaman aşımı üst sınırı |24 saat | |
| Site sayısı |100<sup>1</sup> |HTTP dinleyicilerine göre 1 |
| Dinleyici başına URL haritaları |1 | |
| URL eşlemesi başına en fazla yol tabanlı kural|100||
| Yeniden yönlendirme yapılandırması |100<sup>1</sup>| |
| Eşzamanlı WebSocket bağlantıları |Orta ağ geçitleri 20.000<br> Büyük ağ geçitleri 50.000| |
| En fazla URL uzunluğu|AŞAMAZ| |
| HTTP/2 için üst bilgi boyutu üst sınırı |4 KB 'DIR| |
| Dosya karşıya yükleme boyutu üst sınırı, standart |2 GB | |
| En fazla dosya yükleme boyutu WAF |V1 orta WAF ağ geçitleri, 100 MB<br>V1 büyük WAF ağ geçitleri, 500 MB<br>v2 WAF, 750 MB| |
| WAF gövde boyutu sınırı, dosyalar olmadan|128 KB||
| En fazla WAF özel kuralları|100||
| Maksimum WAF dışlamaları|100||

<sup>1</sup> WAF özellikli SKU 'lar söz konusu olduğunda, en iyi performans için kaynak sayısını 40 olarak sınırlamanızı öneririz.
