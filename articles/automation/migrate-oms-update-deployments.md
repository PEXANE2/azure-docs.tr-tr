---
title: OMS güncelleştirme Dağıtımlarınızı Azure'a geçirin
description: Bu makalede, varolan OMS Update dağıtımlarınızı Azure'a nasıl geçirebilirsiniz
services: automation
ms.subservice: update-management
ms.date: 07/16/2018
ms.topic: conceptual
ms.openlocfilehash: 910f284eedbf50be5b58b6c18f02e50adda35e9a
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679998"
---
# <a name="migrate-your-oms-update-deployments-to-azure"></a>OMS güncelleştirme dağıtımlarınızı Azure'a geçirin

Operasyon Yönetimi Paketi (OMS) portalı [amortismana sokulmaktadır.](../azure-monitor/platform/oms-portal-transition.md) Güncelleme Yönetimi için OMS portalında bulunan tüm işlevler Azure Monitor günlükleri aracılığıyla Azure portalında kullanılabilir. Bu makalede, Azure portalına geçirmek için gereken bilgiler sağlanmaktadır.

## <a name="key-information"></a>Önemli bilgiler

* Varolan dağıtımlar çalışmaya devam eder. Azure'da dağıtımı yeniden oluşturduktan sonra, eski dağıtımınızı OMS'den silebilirsiniz.
* OMS'da sahip olduğunuz tüm mevcut özellikler, Güncelleştirme Yönetimi hakkında daha fazla bilgi edinmek için Azure'da kullanılabilir, [bkz.](automation-update-management.md)

## <a name="access-the-azure-portal"></a>Azure Portalına erişme

OMS çalışma alanınızdan **Azure'da Aç'ı**tıklatın. Bu seçim, OMS'nin kullandığı Günlük Analizi çalışma alanına yönlendirin.

![Azure'da açık - OMS portalı](media/migrate-oms-update-deployments/link-to-azure-portal.png)

Azure portalında **Otomasyon Hesabı'nı** tıklatın

![Azure İzleyici günlükleri](media/migrate-oms-update-deployments/log-analytics.png)

Otomasyon hesabınızda, **Yönetim Güncelle'yi**tıklatın.

![Güncelleştirme Yönetimi](media/migrate-oms-update-deployments/azure-automation.png)

Azure portalında, Tüm hizmetler altında **Otomasyon Hesapları'nı** seçin. **All services** 

**Yönetim Araçları**altında, uygun Otomasyon hesabını seçin ve **Yönetim Güncelleştir'i**tıklatın.

## <a name="recreate-existing-deployments"></a>Varolan dağıtımları yeniden oluşturma

OMS portalında oluşturulan tüm güncelleştirme dağıtımlarında, var olan güncelleştirme dağıtımıyla aynı ada sahip bilgisayar grubu olarak da bilinen kayıtlı bir [arama](../azure-monitor/platform/computer-groups.md) bulunur. Kaydedilen arama, güncelleştirme dağıtımında zamanlanan makinelerin listesini içerir.

![Güncelleştirme Yönetimi](media/migrate-oms-update-deployments/oms-deployment.png)

Bu varolan kaydedilmiş aramayı kullanmak için aşağıdaki adımları izleyin:

Yeni bir güncelleştirme dağıtımı oluşturmak için Azure portalına gidin, kullanılan Otomasyon Hesabı'nı seçin ve **Yönetim'i Güncelleştir'i**tıklatın. **Dağıtımı Zamanla güncelleştir'i**tıklatın.

![Güncelleştirmeyi zamanlama dağıtımı](media/migrate-oms-update-deployments/schedule-update-deployment.png)

Yeni Güncelleştirme Dağıtım bölmesi açılır. Aşağıdaki tabloda açıklanan özellikler için değerleri girin ve sonra **Oluştur'u**tıklatın:

**Makinelerin güncelleştirilmesi için,** varolan OMS dağıtımı tarafından kullanılan kayıtlı aramayı seçin.

| Özellik | Açıklama |
| --- | --- |
|Adı |Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad. |
|İşletim Sistemi| **Linux** veya **Windows'u**seçin.|
|Güncelleştirilen makineler |Kaydedilen arama, Alınan grup veya açılan makineden Makine'yi seçin ve tek tek makineleri seçin. **Makineler**'i seçerseniz makinenin hazır olma durumu **GÜNCELLEŞTİRME ARACISI HAZIRLIĞI** sütununda gösterilir.</br> Azure Monitor günlüklerinde bilgisayar grupları oluşturmanın farklı yöntemleri hakkında bilgi edinmek için [Azure Monitor günlüklerinde Bilgisayar gruplarına](../azure-monitor/platform/computer-groups.md) bakın |
|Update classifications|İhtiyacınız olan tüm güncelleştirme sınıflandırmalarını seçin. CentOS bunu kutunun dışında desteklemez.|
|Hariç tutmak için güncelleştirmeler|Hariç tutmak için güncelleştirmeleri girin. Windows için KB **öneki** olmadan KB makalesini girin. Linux için paket adını girin veya joker karakter kullanın.  |
|Zamanlama ayarları|Başlangıç saatini seçin ve ardından yineleme için **Bir kez** veya **Yinelenen'i** seçin. | 
| Bakım penceresi |Güncelleştirmeler için ayarlanan dakika sayısı. Değer 30 dakikadan az veya 6 saatten fazla olamaz. |
| Yeniden başlatma denetimi| Yeniden başlatmaların nasıl işleneceğini belirler.</br>Kullanılabilen seçenekler:</br>Gerekirse yeniden başlat (Varsayılan)</br>Her zaman yeniden başlat</br>Hiçbir zaman yeniden başlatma</br>Yalnızca yeniden başlatma - güncelleştirmeleri yüklemez|

Yeni oluşturulan güncelleştirme dağıtımının durumunu görüntülemek için **Zamanlanmış güncelleştirme dağıtımlarını** tıklatın.

![yeni güncelleştirme dağıtımı](media/migrate-oms-update-deployments/new-update-deployment.png)

Daha önce de belirtildiği gibi, yeni dağıtımlarınız Azure portalı üzerinden yapılandırıldıktan sonra, varolan dağıtımlar OMS portalından kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure'da Güncelleştirme Yönetimi hakkında daha fazla bilgi edinmek için Bkz. [Güncelleştirme Yönetimi.](automation-update-management.md)
