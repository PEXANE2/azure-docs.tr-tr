---
title: CAF Foundation şema örneğine genel bakış
description: Azure Foundation şema örneği için Bulut Benimseme Çerçevesi’ne (CAF) genel bakış ve mimari.
ms.date: 04/15/2020
ms.topic: sample
ms.openlocfilehash: a5616667ae909b63f406e399a23bdf568089bf52
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383584"
---
# <a name="overview-of-the-microsoft-cloud-adoption-framework-for-azure-foundation-blueprint-sample"></a>Azure Foundation şema örneği için Microsoft Bulutu Benimseme çerçevesine genel bakış

Azure Foundation şeması için Microsoft Bulutu Benimseme Çerçevesi (CAF), üretim düzeyindeki ilk Azure uygulamanız için gerekli çekirdek altyapı kaynakları ve ilke denetimleri kümesini dağıtır. Bu temel şema, CAF’de bulunan önerilen desene göre belirlenir.

## <a name="architecture"></a>Mimari

CAF Foundation şema örneği, kuruluşların bulut varlıklarını yönetmek için gerekli olan temel denetimleri belirlemek üzere kullanılabileceği, Azure’da önerilen altyapı kaynaklarını dağıtır. Bu örnek, bir kuruluşun Azure’ı güvenle kullanmaya başlamasını sağlayacak kaynakları, ilkeleri ve şablonları dağıtıp uygular.

:::image type="content" source="../../media/caf-blueprints/caf-foundation-architecture.png" alt-text="CAF Foundation görüntüsü, Azure’ı kullanmaya başlamak amacıyla temel oluşturmaya yönelik CAF rehberliğinin bir parçası olarak yüklenenleri tanımlar" border="false":::

Bu uygulama, güvenli, tam olarak izlenen, kurumsal kullanıma hazır bir temel sağlamak amacıyla kullanılan çeşitli Azure hizmetlerinden oluşur. Bu ortam şunlardan oluşur:

- Paylaşılan hizmetler ortamına dağıtılmış VM'lerin gizli dizilerini barındırmak için kullanılan [Azure Key Vault](../../../../key-vault/key-vault-overview.md) örneği
- [Depolama Hesapları](../../../../storage/common/storage-introduction.md)’na güvenli dağıtımınıza başladığınız andan itibaren tüm eylemlerin ve hizmetlerin tanılama günlüğüne kaydedilmesi için merkezi bir konumda günlüğe kaydedildiğinden emin olmak amacıyla dağıtılan [Log Analytics](../../../../azure-monitor/overview.md) dağıtımı
- Geçirilen iş yüklerinize yönelik tehdit koruması sağlayan [Azure Güvenlik Merkezi](../../../../security-center/security-center-intro.md) (standart sürüm) dağıtımı
- Şema ayrıca, şunlar için [Azure İlkeleri](../../../policy/overview.md)’ni tanımlar ve dağıtır: 
  - Kaynak gruplarına uygulanan etiketleme (CostCenter)
  - Kaynak grubuna kaynakları CostCenter Etiketiyle ekleme
  - Kaynaklar ve Kaynak Grupları için izin verilen Azure Bölgesi
  - İzin verilen Depolama Hesabı SKU’ları (dağıtırken seçin)
  - İzin verilen Azure VM SKU’ları (dağıtırken seçin)
  - Ağ İzleme’nin dağıtılmasını gerektirme 
  - Azure Depolama Hesabı Güvenli aktarım Şifrelemesi gerektirme
  - Kaynak türlerini reddetme (dağıtırken seçin)  
- Girişimler
  - Azure Güvenlik Merkezi’nde İzleme’yi etkinleştirme (89 İlke)

Tam bu öğeler [Azure Mimari Merkezi - Referans Mimarileri](/azure/architecture/reference-architectures/)'nde yayımlanan kanıtlanmış uygulamalara dayanır.

> [!NOTE]
> CAF Foundation, iş yükleri için temel mimariyi hazırlar.
> Bu işlevsel mimarinin arkasında yine iş yüklerini dağıtmanız gerekir.

Daha fazla bilgi için bkz. [Azure için Microsoft Bulut Benimseme Çerçevesi - Hazır](/azure/cloud-adoption-framework/ready/).

## <a name="next-steps"></a>Sonraki adımlar

CAF Foundation şema örneğinin genel bakış bilgilerini ve mimarisini gözden geçirdiniz.

> [!div class="nextstepaction"]
> [CAF Foundation şeması - Dağıtım adımları](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.
