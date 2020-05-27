---
title: Azure Otomasyonu güncelleştirme değerlendirmelerini görüntüleme
description: Bu makalede Güncelleştirme Yönetimi dağıtımlarının güncelleştirme değerlendirmelerinin nasıl görüntüleneceği açıklanır.
services: automation
ms.subservice: update-management
ms.date: 01/21/2020
ms.topic: conceptual
ms.openlocfilehash: 321146364897d46a403bdfd6789fcb219179d88c
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83830608"
---
# <a name="view-update-assessments"></a>Güncelleştirme değerlendirmelerini görüntüleme

Azure Otomasyonu hesabınızda, makinelerinizin durumunu görüntülemek için **güncelleştirme yönetimi** ' yi seçin.

Bu görünüm, makineleriniz, eksik güncelleştirmeler, güncelleştirme dağıtımları ve zamanlanmış güncelleştirme dağıtımları hakkında bilgi sağlar. **Uyumluluk** sütununda, makinenin en son değerlendirildiğini görebilirsiniz. **GÜNCELLEŞTIRME ARACıSı hazırlığı** sütununda, güncelleştirme aracısının sistem durumunu görebilirsiniz. Bir sorun varsa, sorunu düzeltmenize yardımcı olabilecek sorun giderme belgelerine gitmek için bağlantıyı seçin.

Makine, güncelleştirme veya dağıtım hakkında bilgi döndüren bir günlük araması çalıştırmak için, listeden karşılık gelen öğeyi seçin. Günlük araması bölmesi, seçilen öğe için bir sorgu ile açılır.

![Güncelleştirme Yönetimi varsayılan görünüm](media/automation-update-management/update-management-view.png)

## <a name="view-missing-updates"></a>Eksik güncelleştirmeleri görüntüle

Makinelerinizde bulunmayan güncelleştirmelerin listesini görüntülemek için **eksik güncelleştirmeler** ' i seçin. Her güncelleştirme listelenir ve seçilebilir. Güncelleştirme, işletim sistemi ayrıntıları ve daha fazla bilgi için bir bağlantının gerekli olduğu makine sayısı hakkında bilgi gösterilir. Günlük araması bölmesinde güncelleştirmeler hakkında daha fazla ayrıntı de gösterilmektedir.

![Eksik güncelleştirmeler](./media/automation-view-update-assessments/automation-view-update-assessments-missing-updates.png)

## <a name="work-with-update-classifications"></a>Güncelleştirme sınıflandırmalarıyla çalışma

Aşağıdaki tablolarda, Güncelleştirme Yönetimi içinde desteklenen güncelleştirme sınıflandırmaları, her sınıflandırma için bir tanım ile listelenmektedir.

### <a name="windows"></a>Windows

|Sınıflandırma  |Açıklama  |
|---------|---------|
|Kritik güncelleştirmeler     | Kritik, güvenlikle ilgili olmayan hataları ele alan belirli sorunlara yönelik güncelleştirmeler.        |
|Güvenlik güncelleştirmeleri     | Ürüne özgü, güvenlikle ilgili sorunlar için güncelleştirmeler.        |
|Güncelleştirme paketleri     | Kolay dağıtım için bir arada paketlenmiş düzeltme kümeleri.        |
|Özellik paketleri     | Ürün sürümü dışında dağıtılan yeni ürün özellikleri.        |
|Hizmet paketleri     | Bir uygulamaya uygulanan düzeltme kümeleri.        |
|Tanım güncelleştirmeleri     | Virüs veya diğer tanım dosyalarına yönelik güncelleştirmeler.        |
|Araçlar     | Bir veya daha fazla görevi tamamlamaya yardımcı olan yardımcı programlar veya Özellikler.        |
|Güncelleştirmeler     | Şu anda yüklü olan uygulama veya dosyalar için güncelleştirmeler.        |

### <a name="linux"></a><a name="linux-2"></a>Linux

|Sınıflandırma  |Açıklama  |
|---------|---------|
|Kritik güncelleştirmeler ve güvenlik güncelleştirmeleri     | Belirli bir sorun veya ürüne özgü, güvenlikle ilgili bir sorun için güncelleştirmeler.         |
|Diğer güncelleştirmeler     | Doğası gereği önemli olmayan veya güvenlik güncelleştirmeleri olmayan diğer tüm güncelleştirmeler.        |

Linux için Güncelleştirme Yönetimi, değerlendirme verilerini görüntülerken buluttaki kritik güncelleştirmeleri ve güvenlik güncelleştirmelerini ayırt edebilir. (Bulutta veri zenginleştirme nedeniyle bu ayrıntı düzeyi mümkündür.) Düzeltme eki uygulama Güncelleştirme Yönetimi makinede bulunan sınıflandırma verilerine bağımlıdır. Diğer dağıtımlardan farklı olarak, CentOS ürünün RTM sürümlerinde bu bilgiler bulunmamaktadır. Aşağıdaki komut için güvenlik verilerini döndürecek şekilde yapılandırılmış CentOS makineleriniz varsa, sınıflandırmalara göre Güncelleştirme Yönetimi düzeltme eki uygulanabilir:

```bash
sudo yum -q --security check-update
```

Şu anda yerel sınıflandırmanın etkinleştirilmesi için desteklenen bir yöntem yok-CentOS üzerinde veri kullanılabilirliği. Şu anda, bu işlevselliği kendi kendilerine etkinleştirmiş olan müşterilere yalnızca en iyi çaba desteği sağlanır.

Red Hat Enterprise sürüm 6 ' da güncelleştirmeleri sınıflandırmak için, yıum-güvenlik eklentisini yüklemeniz gerekir. Red Hat Enterprise Linux 7 ' de, eklenti zaten bir yalnızca bir parçası olduğundan, herhangi bir şey yüklemeye gerek yoktur. Daha fazla bilgi için aşağıdaki Red hat [Bilgi Bankası makalesine](https://access.redhat.com/solutions/10021)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Genel bilgiler için bkz. [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md).
