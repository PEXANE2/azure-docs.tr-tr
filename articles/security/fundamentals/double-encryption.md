---
title: Microsoft Azure 'de Çift şifreleme
description: Bu makalede Microsoft Azure, bekleyen veriler ve yoldaki veriler için nasıl Çift şifreleme sağladığını açıklamaktadır.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
ms.assetid: 9dcb190e-e534-4787-bf82-8ce73bf47dba
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/13/2020
ms.author: terrylan
ms.openlocfilehash: 020e8249f57ccb1a14da798a717a00dcc3962389
ms.sourcegitcommit: 152c522bb5ad64e5c020b466b239cdac040b9377
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88227510"
---
# <a name="double-encryption"></a>Çift şifreleme
İki veya daha fazla bağımsız şifreleme katmanı, herhangi bir şifreleme katmanının güvencesine karşı korunmak için iki veya daha fazla bağımsız bir şifreleme katmanıdır. İki şifreleme katmanını kullanmak, verileri şifreleme ile gelen tehditleri azaltır. Örnek:

- Veri şifrelemesindeki yapılandırma hataları
- Şifreleme algoritmasındaki uygulama hataları
- Tek bir şifreleme anahtarının güvenliğinin düşmesi

Azure, bekleyen veriler ve yoldaki veriler için çift şifreleme sağlar.

## <a name="data-at-rest"></a>Bekleme durumundaki veriler
Microsoft 'un, bekleyen veriler için iki katmanlı şifrelemeyi etkinleştirme yaklaşımı şunlardır:

- **Müşteri tarafından yönetilen anahtarları kullanarak disk şifrelemesi**. Disk şifrelemesi için kendi anahtarınızı sağlarsınız. İstediğiniz kaynakları şifrelemek için kendi anahtarlarınızı Key Vault (BYOK – Kendi Anahtarını Getir) veya Azure Key Vault yeni anahtarlar oluşturabilirsiniz.
- **Platform tarafından yönetilen anahtarlar kullanılarak altyapı şifreleme**.  Varsayılan olarak, diskler platform tarafından yönetilen şifreleme anahtarları kullanılarak otomatik olarak şifrelenir.

## <a name="data-in-transit"></a>Aktarım durumundaki veriler
Microsoft 'un yoldaki veriler için iki katmanlı şifrelemeyi etkinleştirme yaklaşımı şunlardır:

- **Aktarım Katmanı Güvenliği (TLS) 1,2 kullanarak, bulut hizmetleri ve sizin arasında hareket ederken verileri korumak Için geçiş şifrelemesi**. Trafik hedefi aynı bölgedeki başka bir etki alanı denetleyicisi olsa bile, veri merkezinde yer alan tüm trafik aktarım sırasında şifrelenir. TLS 1,2, kullanılan varsayılan güvenlik protokolüdür. TLS, güçlü kimlik doğrulaması, ileti gizliliği ve bütünlük (ileti izinsiz değişikliği, yakalama ve forgery), birlikte çalışabilirlik, algoritma esnekliği ve dağıtım ve kullanım kolaylığı sağlar.
- **Altyapı katmanında sunulan ek şifreleme katmanı**. IEEE 802.1 AE MAC güvenlik standartlarını (MACsec olarak da bilinir) kullanan bir veri bağlantısı katmanı şifreleme yöntemi, temeldeki Ağ donanımında noktadan noktaya uygulanır. Azure müşteri trafiği, Microsoft tarafından denetlenmeyen (veya Microsoft adına), fiziksel sınırların dışındaki veri merkezleri arasında her taşınamadığında, paketler gönderilmeden önce cihazlarda şifrelenir ve çözülür ve sonra fiziksel "açık adam" veya gözetleme/kablolu dokunma saldırıları engellenir. Bu teknoloji ağ donanımının kendisi üzerinde tümleştirildiği için, ölçülebilir bağlantı gecikme süresi artışına sahip Ağ donanımında satır hızı şifrelemesi sağlar. Bu MACsec şifrelemesi, bir bölgede veya bölgeler arasında seyahat eden tüm Azure trafiği için varsayılan olarak açık olur ve müşterilerin etkinleştirilmesi için hiçbir işlem yapılması gerekmez.

## <a name="next-steps"></a>Sonraki adımlar
[Azure 'da şifrelemenin nasıl kullanıldığını](encryption-overview.md)öğrenin.
