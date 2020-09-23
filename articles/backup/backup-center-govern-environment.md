---
title: Yedekleme merkezi 'ni kullanarak yedeklemenizi yönetin
description: Tüm kaynaklarınızın yedekleme merkezi ile yedekleme açısından uyumlu olduğundan emin olmak için Azure ortamınızı nasıl yöneteceğinizi öğrenin.
ms.topic: conceptual
ms.date: 09/01/2020
ms.openlocfilehash: 009ee461b0372a3fb73ffb3b0ee5151b77bd8ef8
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90998202"
---
# <a name="govern-your-backup-estate-using-backup-center"></a>Yedekleme merkezi 'ni kullanarak yedeklemenizi yönetin

Yedekleme merkezi, tüm kaynaklarınızın yedekleme açısından uyumlu olduğundan emin olmak için Azure ortamınızı yönetmenize yardımcı olur. Aşağıda, yedekleme merkezi 'nin bazı idare özellikleri verilmiştir:

* Yedekleme için Azure Ilkelerini görüntüleyin ve atayın

* Yedekleme için yapılandırılmamış tüm veri kaynaklarını görüntüleyin.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

* Desteklenen ve desteklenmeyen senaryoların ayrıntılı bir listesi için [destek matrisine](backup-center-support-matrix.md) bakın.

## <a name="azure-policies-for-backup"></a>Yedekleme için Azure Ilkeleri

Yedekleme için kullanılabilen tüm [Azure ilkelerini](https://docs.microsoft.com/azure/governance/policy/overview) görüntülemek için yedekleme menü öğesi **için Azure ilkeleri** ' ni seçin. Bu, abonelikleriniz ve kaynak gruplarınız için kullanılabilir olan [yedekleme için yerleşik ve özel Azure ilke tanımlarının](policy-reference.md) tümünü görüntüler.

Tanımlardan herhangi birini seçmek, ilkeyi bir kapsama [atamanıza](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage#assign-a-policy) izin verir.

![Azure Ilke tanımlarını seçin](./media/backup-center-govern-environment/azure-policy-definitions.png)

## <a name="protectable-datasources"></a>Korunabilir veri kaynakları

**Korunabilir veri kaynakları** menü öğesinin seçilmesi, yedekleme için yapılandırılmamış tüm veri kaynaklarınızı görüntülemenize izin verir. Listeye veri kaynağı aboneliği, kaynak grubu, konum, tür ve etiketlere göre filtre uygulayabilirsiniz. Yedeklenmesi gereken bir veri kaynağını tanımladıktan sonra, ilgili kılavuz öğesine sağ tıklayıp kaynak için yedeklemeyi yapılandırmak üzere **Yedekle** ' yi seçebilirsiniz.

![Korunabilir veri kaynakları menüsü](./media/backup-center-govern-environment/protectable-datasources.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Yedeklemeleri izleme ve çalıştırma](backup-center-monitor-operate.md)
* [Yedekleme merkezi 'ni kullanarak eylem gerçekleştirme](backup-center-actions.md)
* [Yedeklemeleriniz hakkında Öngörüler edinin](backup-center-obtain-insights.md)
