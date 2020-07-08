---
title: CAF Geçişi giriş bölgesi şema örneğine genel bakış
description: Azure Geçişi giriş bölgesi şema örneği için Bulut Benimseme Çerçevesi’ne (CAF) genel bakış ve mimari.
ms.date: 04/15/2020
ms.topic: sample
ms.openlocfilehash: 45479fd6fa3a49d2639e587d3dba2aaaf567d0b1
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86044830"
---
# <a name="overview-of-the-microsoft-cloud-adoption-framework-for-azure-migration-landing-zone-blueprint-sample"></a>Azure Geçişi giriş bölgesi şema örneği için Microsoft Bulutu Benimseme Çerçevesine genel bakış

Azure Geçişi giriş bölgesi şeması için Microsoft Bulutu Benimseme Çerçevesi (CAF), ilk iş yükünüzü geçirmek üzere ayarlamanıza ve bulut varlığınızı CAF ile uyumlu şekilde yönetmenize yardımcı olan bir altyapı kümesidir.

[CAF Foundation](../caf-foundation/index.md) şema örneği, bu örneği genişletir.

## <a name="architecture"></a>Mimari

CAF Geçişi giriş bölgesi şema örneği, kuruluşlar tarafından aboneliklerini sanal makinelerin geçişine hazırlamak amacıyla kullanılabilen Azure’daki temel altyapı kaynaklarını dağıtır. Ayrıca, bulut varlıklarını yönetmek için gerekli idari denetimleri belirlemelerine de yardımcı olur. Bu örnek, bir kuruluşun Azure’ı güvenle kullanmaya başlamasını sağlayacak kaynakları, ilkeleri ve şablonları dağıtıp uygular.

:::image type="content" source="../../media/caf-blueprints/caf-migration-landing-zone-architecture.png" alt-text="CAF Geçişi giriş bölgesi görüntüsü, ilk giriş bölgesi için CAF rehberi kapsamında neyin yüklendiğini tanımlar" border="false":::

Bu ortam, güvenli, tam olarak izlenen, kurumsal kullanıma hazır yönetim sağlamak amacıyla kullanılan çeşitli Azure hizmetlerinden oluşur. Bu ortam şunlardan oluşur:

- Paylaşılan hizmetler ortamına dağıtılmış Sertifikalar, Anahtarlar ve Gizli Dizilere yönelik gizli dizileri barındırmak üzere kullanılan [Azure Key Vault](../../../../key-vault/general/overview.md) örneği
- Geçiş işleminize başladığınız andan itibaren tüm eylemlerin ve hizmetlerin merkezi bir konumda günlüğe kaydedildiğinden emin olmak amacıyla dağıtılan [Log Analytics](../../../../azure-monitor/overview.md) dağıtımı
- [Azure Güvenlik Merkezi](../../../../security-center/security-center-intro.md) (standart sürüm) dağıtımını yaparak, geçirilen iş yüklerinize yönelik tehdit koruması sağlayın.
- [Azure Sanal Ağı](../../../../virtual-network/virtual-networks-overview.md) dağıtımını yaparak, sanal makineniz için yalıtılmış ağ ve alt ağlar sağlayın.
- Keşif ve değerlendirme için [Azure Geçişi Projesi](../../../../migrate/migrate-services-overview.md) dağıtımını yapın. Sunucu değerlendirmesine, Sunucu geçişine, Veritabanı değerlendirmesine ve Veritabanı geçişine yönelik araçları ekliyoruz.  


Tam bu öğeler [Azure Mimari Merkezi - Referans Mimarileri](/azure/architecture/reference-architectures/)'nde yayımlanan kanıtlanmış uygulamalara dayanır.

> [!NOTE]
> CAF Geçişi şeması, iş yüklerinize yönelik bir giriş bölgesi hazırlar. Yine de bu temel mimarinin en üstünde bulunan Sanal Makinelerinizin/Veritabanlarınızın değerlendirme ve geçiş işlemini yapmanız gerekir.

Daha fazla bilgi için bkz. [Azure için Microsoft Bulutu Benimseme Çerçevesi - Geçiş](/azure/architecture/cloud-adoption/migrate/).

## <a name="next-steps"></a>Sonraki adımlar

CAF Geçişi giriş bölgesi şema örneğinin genel bakış bilgilerini ve mimarisini gözden geçirdiniz.

> [!div class="nextstepaction"]
> [CAF Geçişi giriş bölgesi şeması - Dağıtım adımları](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.
