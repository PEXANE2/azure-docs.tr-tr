---
title: Azure Izleyici günlüklerini güncelleştirme dağıtımlarını Azure portal 'ye geçirme
description: Bu makalede, Azure Izleyici günlükleri güncelleştirme dağıtımlarını Azure portal 'ye nasıl geçirebileceğiniz açıklanır.
services: automation
ms.subservice: update-management
ms.date: 07/16/2018
ms.topic: conceptual
ms.openlocfilehash: 9c8238e6e0b52a625c76f79fa0dd5a91dd640fb8
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87447848"
---
# <a name="migrate-azure-monitor-logs-update-deployments-to-azure-portal"></a>Azure Izleyici günlüklerini güncelleştirme dağıtımlarını Azure portal 'ye geçirme

Operations Management Suite (OMS) portalı [kullanım dışı](../azure-monitor/platform/oms-portal-transition.md)bırakılıyor. Güncelleştirme Yönetimi için OMS portalında sunulan tüm işlevler, Azure Izleyici günlükleri aracılığıyla Azure portal sunulmaktadır. Bu makale, Azure portal geçirmek için gereken bilgileri sağlar.

## <a name="key-information"></a>Önemli bilgiler

* Mevcut dağıtımlar çalışmaya devam edecektir. Dağıtımı Azure 'da yeniden oluşturduktan sonra eski dağıtımınızı silebilirsiniz.
* OMS 'de bulunan tüm mevcut özellikler Azure 'da kullanılabilir. Güncelleştirme Yönetimi hakkında daha fazla bilgi edinmek için bkz. [güncelleştirme yönetimi genel bakış](update-management/update-mgmt-overview.md).

## <a name="access-the-azure-portal"></a>Azure Portalına erişme

1. Çalışma alanınızdan **Azure 'Da aç**' a tıklayın. 

    ![Azure 'da aç-Log Analytics](media/migrate-oms-update-deployments/link-to-azure-portal.png)

2. Azure portal **Otomasyon hesabı** ' na tıklayın.

    ![Azure İzleyici günlükleri](media/migrate-oms-update-deployments/log-analytics.png)

3. Otomasyon hesabınızda **güncelleştirme yönetimi**' ye tıklayın.

    ![Güncelleştirme Yönetimi](media/migrate-oms-update-deployments/azure-automation.png)

4. Azure portal, **tüm hizmetler**altında **Otomasyon hesapları** ' nı seçin. 

5. **Yönetim Araçları**altında uygun Otomasyon hesabını seçin ve **güncelleştirme yönetimi**' ye tıklayın.

## <a name="recreate-existing-deployments"></a>Mevcut dağıtımları yeniden oluştur

OMS portalında oluşturulan tüm güncelleştirme dağıtımları, var olan güncelleştirme dağıtımıyla aynı ada sahip bir bilgisayar grubu olarak da bilinen [kayıtlı bir aramaya](../azure-monitor/platform/computer-groups.md) sahiptir. Kayıtlı arama, güncelleştirme dağıtımında zamanlanan makinelerin listesini içerir.

![Güncelleştirme Yönetimi](media/migrate-oms-update-deployments/oms-deployment.png)

Kayıtlı olan bu aramayı kullanmak için şu adımları izleyin:

1. Yeni bir güncelleştirme dağıtımı oluşturmak için Azure portal gidin, kullanılan Otomasyon hesabını seçin ve **güncelleştirme yönetimi**' a tıklayın. **Güncelleştirme dağıtımı zamanla**' ya tıklayın.

    ![Güncelleştirme dağıtımını zamanla](media/migrate-oms-update-deployments/schedule-update-deployment.png)

2. Yeni güncelleştirme dağıtım bölmesi açılır. Aşağıdaki tabloda açıklanan özellikler için değerler girin ve ardından **Oluştur**' a tıklayın:

3. **Makinelerin güncelleştirilmesi**için OMS dağıtımı tarafından kullanılan kayıtlı aramayı seçin.

    | Özellik | Açıklama |
    | --- | --- |
    |Ad |Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad. |
    |İşletim Sistemi| **Linux** veya **Windows**'u seçin.|
    |Güncelleştirilecek makineler |Açılan listeden kaydedilmiş bir arama, Içeri aktarılan grup veya Select makinesi seçin ve tek tek makineler ' i seçin. **Makineler**'i seçerseniz makinenin hazır olma durumu **GÜNCELLEŞTİRME ARACISI HAZIRLIĞI** sütununda gösterilir.</br> Azure Izleyici günlüklerinde bilgisayar grupları oluşturmaya yönelik farklı yöntemler hakkında bilgi edinmek için bkz. [Azure izleyici günlüklerinde bilgisayar grupları](../azure-monitor/platform/computer-groups.md) |
    |Update classifications|İhtiyaç duyduğunuz tüm güncelleştirme sınıflandırmalarını seçin. CentOS bu kutuyu desteklemiyor.|
    |Hariç tutulacak güncelleştirmeler|Dışlanacak güncelleştirmeleri girin. Windows için **KB öneki olmadan KB makalesini** girin. Linux için, paket adını girin veya bir joker karakter kullanın.  |
    |Zamanlama ayarları|Başlatılacak saati seçin ve sonra yinelenme için **bir kez** veya tekrardan **Recurring** birini seçin. | 
    | Bakım penceresi |Güncelleştirmeler için ayarlanan dakika sayısı. Değer 30 dakikadan az veya 6 saatten uzun olamaz. |
    | Yeniden başlatma denetimi| Yeniden başlatmaları nasıl ele alınacağını belirler.</br>Kullanılabilen seçenekler:</br>Gerekirse yeniden başlat (Varsayılan)</br>Her zaman yeniden başlat</br>Hiçbir zaman yeniden başlatma</br>Yalnızca yeniden başlatma - güncelleştirmeleri yüklemez|

4. Yeni oluşturulan güncelleştirme dağıtımının durumunu görüntülemek için **Zamanlanmış güncelleştirme dağıtımları** ' na tıklayın.

    ![yeni güncelleştirme dağıtımı](media/migrate-oms-update-deployments/new-update-deployment.png)

5. Daha önce belirtildiği gibi, yeni dağıtımlarınız Azure portal aracılığıyla yapılandırıldıktan sonra, mevcut dağıtımları Azure portal kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Otomasyonu 'nda Güncelleştirme Yönetimi hakkında daha fazla bilgi için bkz. [güncelleştirme yönetimi genel bakış](update-management/update-mgmt-overview.md).
