---
title: Yedekleme merkezi 'ni kullanarak yedeklemenizi yönetin
description: Tüm kaynaklarınızın yedekleme merkezi ile yedekleme açısından uyumlu olduğundan emin olmak için Azure ortamınızı nasıl yöneteceğinizi öğrenin.
ms.topic: conceptual
ms.date: 09/01/2020
ms.openlocfilehash: 8a16769be827bf34e5be82409d5b8eb014cdcf1a
ms.sourcegitcommit: 06ba80dae4f4be9fdf86eb02b7bc71927d5671d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91614354"
---
# <a name="govern-your-backup-estate-using-backup-center"></a>Yedekleme merkezi 'ni kullanarak yedeklemenizi yönetin

Yedekleme merkezi, tüm kaynaklarınızın yedekleme açısından uyumlu olduğundan emin olmak için Azure ortamınızı yönetmenize yardımcı olur. Aşağıda, yedekleme merkezi 'nin bazı idare özellikleri verilmiştir:

* Yedekleme için Azure Ilkelerini görüntüleyin ve atayın

* Yedekleme için tüm yerleşik Azure Ilkelerinde kaynaklarınızın uyumluluğunu görüntüleyin.

* Yedekleme için yapılandırılmamış tüm veri kaynaklarını görüntüleyin.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

* Desteklenen ve desteklenmeyen senaryoların ayrıntılı bir listesi için [destek matrisine](backup-center-support-matrix.md) bakın.

## <a name="azure-policies-for-backup"></a>Yedekleme için Azure Ilkeleri

Yedekleme için kullanılabilen tüm [Azure ilkelerini](https://docs.microsoft.com/azure/governance/policy/overview) görüntülemek için yedekleme menü öğesi **için Azure ilkeleri** ' ni seçin. Bu, abonelikleriniz ve kaynak gruplarınız için kullanılabilir olan [yedekleme için yerleşik ve özel Azure ilke tanımlarının](policy-reference.md) tümünü görüntüler.

Tanımlardan herhangi birini seçmek, ilkeyi bir kapsama [atamanıza](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage#assign-a-policy) izin verir.

![Azure Ilke tanımlarını seçin](./media/backup-center-govern-environment/azure-policy-definitions.png)

## <a name="backup-compliance"></a>Yedekleme uyumluluğu

Yedekleme uyumluluğu menü öğesine tıkladığınızda kaynaklarınızın [uyumluluğunu](https://docs.microsoft.com/azure/governance/policy/how-to/get-compliance-data) , Azure ortamınıza atadığınız çeşitli yerleşik ilkelere göre görüntülemenize yardımcı olur. Tüm ilkelerle uyumlu olan kaynakların yüzdesini ve bir veya daha fazla uyumlu olmayan kaynağa sahip ilkeleri görüntüleyebilirsiniz.

![Yedekleme uyumluluğunu görüntüleme](./media/backup-center-govern-environment/azure-policy-compliance.png)

## <a name="protectable-datasources"></a>Korunabilir veri kaynakları

**Korunabilir veri kaynakları** menü öğesinin seçilmesi, yedekleme için yapılandırılmamış tüm veri kaynaklarınızı görüntülemenize izin verir. Listeye veri kaynağı aboneliği, kaynak grubu, konum, tür ve etiketlere göre filtre uygulayabilirsiniz. Yedeklenmesi gereken bir veri kaynağını tanımladıktan sonra, ilgili kılavuz öğesine sağ tıklayıp kaynak için yedeklemeyi yapılandırmak üzere **Yedekle** ' yi seçebilirsiniz.

![Korunabilir veri kaynakları menüsü](./media/backup-center-govern-environment/protectable-datasources.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Yedeklemeleri izleme ve çalıştırma](backup-center-monitor-operate.md)
* [Yedekleme merkezi 'ni kullanarak eylem gerçekleştirme](backup-center-actions.md)
* [Yedeklemeleriniz hakkında Öngörüler edinin](backup-center-obtain-insights.md)
