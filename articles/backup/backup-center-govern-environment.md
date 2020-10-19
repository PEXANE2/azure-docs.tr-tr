---
title: Yedekleme Merkezi'ni kullanarak tüm yedeklemelerinizi idare etme
description: Tüm kaynaklarınızın yedekleme merkezi ile yedekleme açısından uyumlu olduğundan emin olmak için Azure ortamınızı nasıl yöneteceğinizi öğrenin.
ms.topic: conceptual
ms.date: 09/01/2020
ms.openlocfilehash: ebb4d72c3a3377072e185251bd642762f6876c03
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92173797"
---
# <a name="govern-your-backup-estate-using-backup-center"></a>Yedekleme Merkezi'ni kullanarak tüm yedeklemelerinizi idare etme

Yedekleme merkezi, tüm kaynaklarınızın yedekleme açısından uyumlu olduğundan emin olmak için Azure ortamınızı yönetmenize yardımcı olur. Aşağıda, yedekleme merkezi 'nin bazı idare özellikleri verilmiştir:

* Yedekleme için Azure Ilkelerini görüntüleyin ve atayın

* Yedekleme için tüm yerleşik Azure Ilkelerinde kaynaklarınızın uyumluluğunu görüntüleyin.

* Yedekleme için yapılandırılmamış tüm veri kaynaklarını görüntüleyin.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

* Desteklenen ve desteklenmeyen senaryoların ayrıntılı bir listesi için [destek matrisine](backup-center-support-matrix.md) bakın.

## <a name="azure-policies-for-backup"></a>Yedekleme için Azure Ilkeleri

Yedekleme için kullanılabilen tüm [Azure ilkelerini](../governance/policy/overview.md) görüntülemek için yedekleme menü öğesi **için Azure ilkeleri** ' ni seçin. Bu, abonelikleriniz ve kaynak gruplarınız için kullanılabilir olan [yedekleme için yerleşik ve özel Azure ilke tanımlarının](policy-reference.md) tümünü görüntüler.

Tanımlardan herhangi birini seçmek, ilkeyi bir kapsama [atamanıza](../governance/policy/tutorials/create-and-manage.md#assign-a-policy) izin verir.

![Azure Ilke tanımlarını seçin](./media/backup-center-govern-environment/azure-policy-definitions.png)

## <a name="backup-compliance"></a>Yedekleme uyumluluğu

Yedekleme uyumluluğu menü öğesine tıkladığınızda kaynaklarınızın [uyumluluğunu](../governance/policy/how-to/get-compliance-data.md) , Azure ortamınıza atadığınız çeşitli yerleşik ilkelere göre görüntülemenize yardımcı olur. Tüm ilkelerle uyumlu olan kaynakların yüzdesini ve bir veya daha fazla uyumlu olmayan kaynağa sahip ilkeleri görüntüleyebilirsiniz.

![Yedekleme uyumluluğunu görüntüleme](./media/backup-center-govern-environment/azure-policy-compliance.png)

## <a name="protectable-datasources"></a>Korunabilir veri kaynakları

**Korunabilir veri kaynakları** menü öğesinin seçilmesi, yedekleme için yapılandırılmamış tüm veri kaynaklarınızı görüntülemenize izin verir. Listeye veri kaynağı aboneliği, kaynak grubu, konum, tür ve etiketlere göre filtre uygulayabilirsiniz. Yedeklenmesi gereken bir veri kaynağını tanımladıktan sonra, ilgili kılavuz öğesine sağ tıklayıp kaynak için yedeklemeyi yapılandırmak üzere **Yedekle** ' yi seçebilirsiniz.

![Korunabilir veri kaynakları menüsü](./media/backup-center-govern-environment/protectable-datasources.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Yedeklemeleri izleme ve çalıştırma](backup-center-monitor-operate.md)
* [Yedekleme merkezi 'ni kullanarak eylem gerçekleştirme](backup-center-actions.md)
* [Yedeklemeleriniz hakkında Öngörüler edinin](backup-center-obtain-insights.md)