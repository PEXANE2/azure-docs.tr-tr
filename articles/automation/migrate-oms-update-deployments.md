---
title: OMS güncelleştirme dağıtımlarınızı Azure 'a geçirme
description: Bu makalede, mevcut OMS güncelleştirme dağıtımlarınızın Azure 'a nasıl geçirileceği açıklanır.
services: automation
ms.subservice: update-management
ms.date: 07/16/2018
ms.topic: conceptual
ms.openlocfilehash: 910f284eedbf50be5b58b6c18f02e50adda35e9a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81679998"
---
# <a name="migrate-your-oms-update-deployments-to-azure"></a>OMS güncelleştirme dağıtımlarınızı Azure 'a geçirme

Operations Management Suite (OMS) portalı [kullanım dışı](../azure-monitor/platform/oms-portal-transition.md)bırakılıyor. Güncelleştirme Yönetimi için OMS portalında sunulan tüm işlevler, Azure Izleyici günlükleri aracılığıyla Azure portal sunulmaktadır. Bu makale, Azure portal geçirmek için gereken bilgileri sağlar.

## <a name="key-information"></a>Önemli bilgiler

* Mevcut dağıtımlar çalışmaya devam edecektir. Dağıtımı Azure 'da yeniden oluşturduktan sonra, OMS 'den eski dağıtımınızı silebilirsiniz.
* OMS 'de bulunan tüm mevcut özellikler Azure 'da bulunur, Güncelleştirme Yönetimi hakkında daha fazla bilgi edinmek için bkz. [güncelleştirme yönetimi genel bakış](automation-update-management.md).

## <a name="access-the-azure-portal"></a>Azure Portalına erişme

OMS çalışma alanınızda **Azure 'Da aç**' a tıklayın. Bu seçim, OMS 'nin kullandığı Log Analytics çalışma alanına gider.

![Azure-OMS portalında aç](media/migrate-oms-update-deployments/link-to-azure-portal.png)

Azure portal **Otomasyon hesabı** ' na tıklayın.

![Azure İzleyici günlükleri](media/migrate-oms-update-deployments/log-analytics.png)

Otomasyon hesabınızda **güncelleştirme yönetimi**' ye tıklayın.

![Güncelleştirme Yönetimi](media/migrate-oms-update-deployments/azure-automation.png)

Azure portal, **tüm hizmetler**altında **Otomasyon hesapları** ' nı seçin. 

**Yönetim Araçları**altında uygun Otomasyon hesabını seçin ve **güncelleştirme yönetimi**' ye tıklayın.

## <a name="recreate-existing-deployments"></a>Mevcut dağıtımları yeniden oluştur

OMS portalında oluşturulan tüm güncelleştirme dağıtımları, var olan güncelleştirme dağıtımıyla aynı ada sahip bir bilgisayar grubu olarak da bilinen [kayıtlı bir aramaya](../azure-monitor/platform/computer-groups.md) sahiptir. Kayıtlı arama, güncelleştirme dağıtımında zamanlanan makinelerin listesini içerir.

![Güncelleştirme Yönetimi](media/migrate-oms-update-deployments/oms-deployment.png)

Kayıtlı olan bu aramayı kullanmak için şu adımları izleyin:

Yeni bir güncelleştirme dağıtımı oluşturmak için Azure portal gidin, kullanılan Otomasyon hesabını seçin ve **güncelleştirme yönetimi**' a tıklayın. **Güncelleştirme dağıtımı zamanla**' ya tıklayın.

![Güncelleştirme dağıtımını zamanla](media/migrate-oms-update-deployments/schedule-update-deployment.png)

Yeni güncelleştirme dağıtım bölmesi açılır. Aşağıdaki tabloda açıklanan özellikler için değerler girin ve ardından **Oluştur**' a tıklayın:

**Makinelerin güncelleştirilmesi**için, mevcut OMS dağıtımı tarafından kullanılan kayıtlı aramayı seçin.

| Özellik | Açıklama |
| --- | --- |
|Adı |Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad. |
|İşletim Sistemi| **Linux** veya **Windows**'u seçin.|
|Güncelleştirilecek makineler |Açılan listeden kaydedilmiş bir arama, Içeri aktarılan grup veya Select Machine seçin ve tek tek makineler ' i seçin. **Makineler**'i seçerseniz makinenin hazır olma durumu **GÜNCELLEŞTİRME ARACISI HAZIRLIĞI** sütununda gösterilir.</br> Azure Izleyici günlüklerinde bilgisayar grupları oluşturmaya yönelik farklı yöntemler hakkında bilgi edinmek için bkz. [Azure izleyici günlüklerinde bilgisayar grupları](../azure-monitor/platform/computer-groups.md) |
|Update classifications|İhtiyaç duyduğunuz tüm güncelleştirme sınıflandırmalarını seçin. CentOS bu kutuyu desteklemiyor.|
|Hariç tutulacak güncelleştirmeler|Dışlanacak güncelleştirmeleri girin. Windows için **KB öneki olmadan KB makalesini** girin. Linux için, paket adını girin veya bir joker karakter kullanın.  |
|Zamanlama ayarları|Başlatılacak saati seçin ve sonra yinelenme için **bir kez** veya tekrardan **Recurring** birini seçin. | 
| Bakım penceresi |Güncelleştirmeler için ayarlanan dakika sayısı. Değer 30 dakikadan az veya 6 saatten uzun olamaz. |
| Yeniden başlatma denetimi| Yeniden başlatmaları nasıl ele alınacağını belirler.</br>Kullanılabilen seçenekler:</br>Gerekirse yeniden başlat (Varsayılan)</br>Her zaman yeniden başlat</br>Hiçbir zaman yeniden başlatma</br>Yalnızca yeniden başlatma - güncelleştirmeleri yüklemez|

Yeni oluşturulan güncelleştirme dağıtımının durumunu görüntülemek için **Zamanlanmış güncelleştirme dağıtımları** ' na tıklayın.

![yeni güncelleştirme dağıtımı](media/migrate-oms-update-deployments/new-update-deployment.png)

Daha önce belirtildiği gibi, yeni dağıtımlarınız Azure portal aracılığıyla yapılandırıldıktan sonra, mevcut dağıtımlar OMS portalından kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure 'da Güncelleştirme Yönetimi hakkında daha fazla bilgi için bkz. [güncelleştirme yönetimi](automation-update-management.md).
