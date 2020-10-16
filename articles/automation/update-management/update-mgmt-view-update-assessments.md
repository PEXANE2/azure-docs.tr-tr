---
title: Azure Otomasyonu güncelleştirme değerlendirmelerini görüntüleme
description: Bu makalede Güncelleştirme Yönetimi dağıtımlarının güncelleştirme değerlendirmelerinin nasıl görüntüleneceği açıklanır.
services: automation
ms.subservice: update-management
ms.date: 09/17/2020
ms.topic: conceptual
ms.openlocfilehash: 2e32fc7c1872bf18b7f1c995f281a9b09ec45dc8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91264489"
---
# <a name="view-update-assessments-in-update-management"></a>Güncelleştirme Yönetimi güncelleştirme değerlendirmelerini görüntüleme

Güncelleştirme Yönetimi, makineleriniz, eksik güncelleştirmeler, güncelleştirme dağıtımları ve zamanlanmış güncelleştirme dağıtımları hakkında bilgi görüntüleyebilirsiniz. Seçilen Azure sanal makinesi kapsamındaki değerlendirme bilgilerini seçili yay etkin sunucusundan veya Otomasyon hesabından tüm yapılandırılmış makineler ve sunucular arasında görüntüleyebilirsiniz.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalda](https://portal.azure.com) oturum açma

## <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

Azure VM 'den güncelleştirme değerlendirmesini görüntülemek için, **sanal makineler** ' e gidin ve listeden sanal makinenizi seçin. Sol taraftaki menüden **Konuk + ana bilgisayar güncelleştirmeleri**' ni seçin ve ardından **Konuk + ana bilgisayar güncelleştirmeleri** sayfasında **güncelleştirme yönetimi git** ' i seçin.

Güncelleştirme Yönetimi, makineniz, eksik güncelleştirmeler, güncelleştirme dağıtımları ve zamanlanmış güncelleştirme dağıtımları hakkında bilgi görüntüleyebilirsiniz.

[![Azure VM için güncelleştirme yönetimi değerlendirme görünümü](./media/update-mgmt-view-update-assessments/update-assessment-azure-vm.png)](./media/update-mgmt-view-update-assessments/update-assessment-azure-vm-expanded.png#lightbox)

Bir yay etkin sunucusundan güncelleştirme değerlendirmesini görüntülemek için, **sunucular-Azure Arc** ' a gidin ve listeden sunucunuzu seçin. Sol menüden **Konuk ve konak güncelleştirmeleri**' ni seçin. **Konuk + ana bilgisayar güncelleştirmeleri** sayfasında **güncelleştirme yönetimi git**' i seçin.

Güncelleştirme Yönetimi, Arc etkin makineniz, eksik güncelleştirmeler, güncelleştirme dağıtımları ve zamanlanmış güncelleştirme dağıtımları hakkında bilgi görüntüleyebilirsiniz.

[![Yay özellikli sunucular için güncelleştirme yönetimi değerlendirmesi görünümü](./media/update-mgmt-view-update-assessments/update-assessment-arc-server.png)](./media/update-mgmt-view-update-assessments/update-assessment-arc-server-expanded.png#lightbox)

Otomasyon hesabınızdan yay özellikli sunucular da dahil olmak üzere tüm makinelerde güncelleştirme değerlendirmesini görüntülemek için **Otomasyon hesapları** ' na gidin ve listeden güncelleştirme yönetimi etkinleştirilmiş Otomasyon hesabınızı seçin. Otomasyon hesabınızda, sol menüden **güncelleştirme yönetimi** ' ni seçin.

Ortamınız için güncelleştirmeler **güncelleştirme yönetimi** sayfasında listelenir. Herhangi bir güncelleştirme eksik olarak tanımlanmışsa, bu liste **eksik güncelleştirmeler** sekmesinde gösterilir.

[![Güncelleştirme yönetimi varsayılan görünüm](./media/update-mgmt-overview/update-management-view.png)](./media/update-mgmt-overview/update-management-view-expanded.png#lightbox)

**Uyumluluk** sütununun altında, makinenin en son değerlendirildiğini görebilirsiniz. **GÜNCELLEŞTIRME ARACıSı hazırlığı** sütununda, güncelleştirme aracısının sistem durumunu görebilirsiniz. Bir sorun varsa, sorunu düzeltmenize yardımcı olabilecek sorun giderme belgelerine gitmek için bağlantıyı seçin.

**Bilgi bağlantısı**altında güncelleştirme bağlantısını seçerek güncelleştirme hakkında önemli bilgiler sunan destek makalesini açın.

[![Güncelleştirme durumunu görüntüle](./media/update-mgmt-view-update-assessments/missing-updates.png)](./media/update-mgmt-view-update-assessments/missing-updates-expanded.png#lightbox)

Günlük araması bölmesini açmak için güncelleştirmede başka herhangi bir yere tıklayın. Günlük araması sorgusu ilgili güncelleştirmeye göre önceden tanımlanmıştır. Ayrıntılı bilgileri görüntülemek için bu sorguyu değiştirebilir veya kendi sorgunuzu oluşturabilirsiniz.

[![Günlük sorgusu sonuçlarını görüntüle](./media/update-mgmt-view-update-assessments/logsearch-results.png)](./media/update-mgmt-view-update-assessments/logsearch-results-expanded.png#lightbox)

## <a name="view-missing-updates"></a>Eksik güncelleştirmeleri görüntüle

Makinelerinizde bulunmayan güncelleştirmelerin listesini görüntülemek için **eksik güncelleştirmeler** ' i seçin. Her güncelleştirme listelenir ve seçilebilir. Güncelleştirme, işletim sistemi ayrıntıları ve daha fazla bilgi için bir bağlantının gerekli olduğu makine sayısı hakkında bilgi gösterilir. Günlük araması bölmesinde güncelleştirmeler hakkında daha fazla ayrıntı de gösterilmektedir.

![Eksik güncelleştirmeler](./media/update-mgmt-view-update-assessments/automation-view-update-assessments-missing-updates.png)

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

### <a name="linux"></a>Linux

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

İşlemin sonraki aşaması, güncelleştirmeleri uyumlu olmayan makinelere [dağıtmaktır](update-mgmt-deploy-updates.md) ve dağıtım sonuçlarını gözden geçirmaktır.
