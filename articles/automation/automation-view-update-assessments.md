---
title: Azure Güncelleştirme Yönetimi güncelleştirme değerlendirmelerini görüntüleyin
description: Bu makalede, güncelleştirme dağıtımları için güncelleştirme değerlendirmelerinin nasıl görüntülenilen açıklanmaktadır.
services: automation
ms.subservice: update-management
ms.date: 01/21/2020
ms.topic: conceptual
ms.openlocfilehash: 58d3cf6261456c09195ad6dafaeb781b55d9e5ee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278421"
---
# <a name="view-azure-update-management-update-assessments"></a>Azure Güncelleştirme Yönetimi güncelleştirme değerlendirmelerini görüntüleyin

Azure Otomasyon hesabınızda, makinelerinizin durumunu görüntülemek için **Yönetim'i Güncelleştir'i** seçin.

Bu görünüm, makineleriniz, eksik güncelleştirmeleriniz, güncelleştirme dağıtımlarınız ve zamanlanmış güncelleştirme dağıtımları hakkında bilgi sağlar. **UYUMLULUK** sütununda, makinenin en son ne zaman değerlendirildiğini görebilirsiniz. UPDATE **AGENT READINESS** sütununda, güncelleştirme aracısının durumunu görebilirsiniz. Bir sorun varsa, sorunu düzeltmenize yardımcı olabilecek sorun giderme belgelerine gitmek için bağlantıyı seçin.

Makine, güncelleştirme veya dağıtım la ilgili bilgileri döndüren bir günlük araması çalıştırmak için listedeki ilgili öğeyi seçin. **Günlük Arama** bölmesi seçilen öğe için bir sorgu ile açılır:

![Yönetim varsayılan görünümünü güncelleştir](media/automation-update-management/update-management-view.png)

## <a name="view-missing-updates"></a>Eksik güncelleştirmeleri görüntüleme

Makinelerinizde eksik olan güncelleştirmelerin listesini görüntülemek için **Eksik güncelleştirmeleri** seçin. Her güncelleştirme listelenir ve seçilebilir. Güncelleştirme gerektiren makine sayısı, işletim sistemi ayrıntıları ve daha fazla bilgi için bir bağlantı hakkında bilgiler gösterilir. **Günlük arama** bölmesi de güncelleştirmeler hakkında daha fazla ayrıntı gösterir.

![Eksik Güncellemeler](./media/automation-view-update-assessments/automation-view-update-assessments-missing-updates.png)

## <a name="update-classifications"></a>Update classifications

Aşağıdaki tablolar, Güncelleştirme Yönetimi'nde desteklenen güncelleştirme sınıflandırmalarını ve her sınıflandırma için bir tanımları listelenebilmektedir.

### <a name="windows"></a>Windows

|Sınıflandırma  |Açıklama  |
|---------|---------|
|Kritik güncelleştirmeler     | Güvenlikle ilgili kritik bir hatayı gideren belirli bir sorun için güncelleştirme.        |
|Güvenlik güncelleştirmeleri     | Ürüne özgü, güvenlikle ilgili bir sorun için güncelleştirme.        |
|Güncelleştirme paketleri     | Kolay dağıtım için birlikte paketlenmiş bir birikimli düzeltme kümesi.        |
|Özellik paketleri     | Ürün sürümü dışında dağıtılan yeni ürün özellikleri.        |
|Hizmet paketleri     | Bir uygulamaya uygulanan birikmeli düzeltme kümesi.        |
|Tanım güncelleştirmeleri     | Virüs veya diğer tanım dosyaları için bir güncelleştirme.        |
|Araçlar     | Bir veya daha fazla görevi tamamlamaya yardımcı olan bir yardımcı program veya özellik.        |
|Güncelleştirmeler     | Şu anda yüklü olan bir uygulama veya dosyaiçin güncelleştirme.        |

### <a name="linux"></a><a name="linux-2"></a>Linux

|Sınıflandırma  |Açıklama  |
|---------|---------|
|Kritik güncelleştirmeler ve güvenlik güncelleştirmeleri     | Belirli bir sorun veya ürüne özgü, güvenlikle ilgili bir sorun için güncelleştirmeler.         |
|Diğer güncelleştirmeler     | Doğada kritik olmayan veya güvenlik güncelleştirmeleri olmayan diğer tüm güncelleştirmeler.        |

Linux için, Update Management değerlendirme verilerini görüntülerken buluttaki kritik güncelleştirmeleri ve güvenlik güncelleştirmelerini ayırt edebilir. (Bu parçalılık, buluttaki veri zenginleştirme nedeniyle mümkündür.) Düzeltme için Güncelleştirme Yönetimi, makinede bulunan sınıflandırma verilerine dayanır. Diğer dağıtımların aksine, CentOS ürünün RTM sürümlerinde bu bilgilere sahip değildir. Aşağıdaki komut için güvenlik verilerini döndürecek şekilde yapılandırılan CentOS makineleriniz varsa, Güncelleştirme Yönetimi sınıflandırmalara göre yama yapabilir:

```bash
sudo yum -q --security check-update
```

Şu anda CentOS'ta yerel sınıflandırma verilerinin kullanılabilirliğini etkinleştirmek için desteklenen bir yöntem yok. Şu anda, bu işlevselliği kendi başlarına etkinleştiren müşterilere yalnızca en iyi çaba desteği sağlanır.

Red Hat Enterprise sürüm 6'daki güncellemeleri sınıflandırmak için yum-security eklentisini yüklemeniz gerekir. Red Hat Enterprise Linux 7, eklenti zaten yum kendisi bir parçasıdır, bir şey yüklemek için gerek yoktur. Daha fazla bilgi için aşağıdaki Red Hat [bilgi makalesine](https://access.redhat.com/solutions/10021)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Herhangi bir güncelleştirme değerlendirmesini görüntüledikten sonra, [Azure VM'leriniz için güncelleştirmeleri ve yamaları yönet](automation-tutorial-update-management.md)adımlarını izleyerek bir güncelleştirme dağıtımı zamanlayabilirsiniz.
