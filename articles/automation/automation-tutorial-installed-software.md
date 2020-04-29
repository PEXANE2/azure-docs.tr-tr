---
title: Azure Otomasyonu ile makinelerinizde yüklü olan yazılımları keşfetme | Microsoft Docs
description: Ortamınızdaki makinelerde yüklü olan yazılımları keşfetmek için Stok özelliğinden faydalanın.
services: automation
keywords: stok, otomasyon, değişiklik, izleme
ms.date: 04/11/2018
ms.topic: tutorial
ms.subservice: change-inventory-management
ms.custom: mvc
ms.openlocfilehash: b93035fc7e315f8117516771236186f9d942a0aa
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81604662"
---
# <a name="discover-what-software-is-installed-on-your-azure-and-non-azure-machines"></a>Azure ve Azure harici makinelerinizde yüklü olan yazılımları keşfetme

Bu öğreticide ortamınızda yüklü olan yazılımları keşfetmeyi öğreneceksiniz. Yazılımlar, dosyalar, Linux daemon'ları, Windows hizmetleri ve Windows kayıt defteri anahtarlarıyla ilgili stok durumunu sorgulayabilir ve görüntüleyebilirsiniz. Makinelerinizin yapılandırmasını izlemek ortamınızdaki işletimsel sorunları bulmanıza ve makinelerinizin durumunu daha iyi anlamanıza yardımcı olabilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Çözümü etkinleştirme
> * Bir Azure VM ekleme
> * Azure olmayan bir VM ekleme
> * Yüklü olan yazılımları görüntüleme
> * Yüklü olan yazılımların stok günlüklerinde arama yapma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)için kaydolabilirsiniz.
* İzleyici ve eylem runbook 'larını ve Izleyici görevini tutacak bir [Otomasyon hesabı](automation-offering-get-started.md) .
* Sisteme eklenecek bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="enable-change-tracking-and-inventory"></a>Değişiklik İzleme ve Stok özelliklerini etkinleştirme

Bu öğreticide ilk yapmanız gereken Değişiklik İzleme ve Stok özelliklerini etkinleştirmektir. Daha önce **Değişiklik İzleme** çözümünü etkinleştirdiyseniz bu adımı atlayabilirsiniz.

Otomasyon hesabınıza gidin ve **yapılandırma yönetimi**altında **Stok** ' ı seçin.

Log Analytics çalışma alanını ve otomasyon hesabını seçin ve çözümü etkinleştirmek için **Etkinleştir** ' e tıklayın. Çözümün etkinleştirilmesi 15 dakika sürer.

![Stok devreye alma yapılandırma başlığı](./media/automation-tutorial-installed-software/enableinventory.png)

Çözümü etkinleştirmek için, kullanılacak konumu, Log Analytics çalışma alanını ve otomasyon hesabını yapılandırın ve **Etkinleştir**' e tıklayın. Bu alanların gri renkte olması VM için etkinleştirilmiş başka bir otomasyon çözümü olduğunu gösterir ve bu durumda aynı çalışma alanı ile Otomasyon hesabının kullanılması gerekir.

[Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json) çalışma alanı, Stok gibi özellikler ve hizmetler tarafından oluşturulan verileri toplamak için kullanılır.
Çalışma alanı, birden fazla kaynaktan alınan verilerin incelenip analiz edilebileceği ortak bir konum sağlar.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Çözümün etkinleştirilmesi 15 dakika sürebilir. Bu süre boyunca tarayıcı penceresini kapatmamanız gerekir.
Çözüm etkinleştirildikten sonra VM 'deki yüklü yazılım ve değişiklikler hakkında bilgiler Azure Izleyici günlüklerine akar.
Verilerin çözümlemeye hazır hale gelmesi 30 dakika ile 6 saat arasında sürebilir.

## <a name="onboard-a-vm"></a>VM ekleme

Otomasyon hesabınızda, **yapılandırma yönetimi**altında **Stok** ' a gidin.

**+ Azure VM ekle**’yi seçin, bu Sanal makineler sayfasını açarak listeden mevcut bir VM seçmenize olanak sağlar. Eklemek istediğiniz sanal makineyi seçin. Açılan sayfada, VM’de çözümü etkinleştirmek için **Etkinleştir**’e tıklayın. Microsoft Yönetim Aracısı VM’ye dağıtılır ve aracıyı çözümü etkinleştirirken yapılandırdığınız Log Analytics çalışma alanıyla iletişim kurması için yapılandırır. Ekleme işleminin tamamlanması birkaç dakika sürebilir. Bu noktada, listeden yeni bir VM seçebilir ve başka bir VM ekleyebilirsiniz.

## <a name="onboard-a-non-azure-machine"></a>Azure olmayan bir makine ekleme

Azure olmayan makineler eklemek için işletim sisteminize bağlı olarak, [Windows için Log Analytics aracısını](../azure-monitor/platform/agent-windows.md) veya [Linux için Log Analytics aracısını](automation-linux-hrw-install.md)yüklersiniz. Aracı yüklendikten sonra, Otomasyon hesabınıza gidin ve **yapılandırma yönetimi**altında **Stok** ' a gidin. **Makineleri Yönet**’e tıkladığınızda, Log Analytics çalışma alanınıza raporlayan çözüm etkinleştirilmemiş makinelerin bir listesini görürsünüz. Ortamınız için uygun seçeneği belirleyin.

* **Kullanılabilir tüm makinelerde etkinleştir** - Bu seçenek, çözümü şu anda Log Analytics çalışma alanınıza raporlayan tüm makinelerde etkinleştirir.
* **tüm kullanılabilir makinelerde ve gelecekteki makinelerde etkinleştir** - Bu seçenek, çözümü Log Analytics çalışma alanınıza raporlayan tüm makinelerde ve daha sonra çalışma alanına eklenen tüm gelecek makinelerde etkinleştirir.
* **Seçili makinelerde etkinleştir** - Bu seçenek çözümü yalnızca seçtiğiniz makinelerde etkinleştirir.

![Makineleri Yönetme](./media/automation-tutorial-installed-software/manage-machines.png)

## <a name="view-installed-software"></a>Yüklü olan yazılımları görüntüleme

Değişiklik İzleme ve envanter çözümü etkinleştirildikten sonra, sonuçları envanter sayfasında görüntüleyebilirsiniz.

Otomasyon hesabınızda, **yapılandırma yönetimi**altında **Stok** ' u seçin.

Stok sayfasında **Yazılım** sekmesine tıklayın.

**Yazılım** sekmesinde bulunan yazılımların listelendiği bir tablo yer alır. Yazılımlar ada ve sürüme göre gruplara ayrılmıştır.

Tabloda her yazılım kaydı hakkındaki üst düzey ayrıntılar görüntülenebilir. Bu ayrıntılar arasında yazılım adı, sürümü, yayımcı, son yenileme zamanı (grup içindeki bir makine tarafından bildirilen en son yenileme zamanı) ve makine sayısı (bu yazılıma sahip makinelerin sayısı) yer alır.

![Yazılım envanteri](./media/automation-tutorial-installed-software/inventory-software.png)

Yazılım kaydının özelliklerini ve bu yazılımın yüklü olduğu makinelerin adlarını görüntülemek için satırlardan birine tıklayın.

Belirli bir yazılımı veya yazılım grubunu aramak için yazılım listesinin üstünde yer alan metin kutusundan arama yapabilirsiniz.
Filtre yazılım adı, sürümü veya yayımcı ile arama yapmanızı sağlar.

Örneğin, **contoso** araması, **contoso**içeren bir ad, yayımcı veya sürüm ile tüm yazılımları döndürür.

## <a name="search-inventory-logs-for-installed-software"></a>Yüklü olan yazılımların stok günlüklerinde arama yapma

Envanter, Azure Izleyici günlüklerine gönderilen günlük verileri oluşturur. Sorguları çalıştırarak günlüklerde arama yapmak için, envanter sayfasının en üstünde **Log Analytics** ' yi seçin.

Envanter verileri, türü `ConfigurationData`altında depolanır.
Aşağıdaki örnek Log Analytics sorgusu, yayımcının **Microsoft Corporation**'a eşit olduğu envanter sonuçlarını döndürür.

```loganalytics
ConfigurationData
| where ConfigDataType == "Software"
| where Publisher == "Microsoft Corporation"
| summarize arg_max(TimeGenerated, *) by SoftwareName, Computer
```

Azure Izleyici günlüklerinde günlük dosyalarını çalıştırma ve arama hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici günlükleri](../azure-monitor/log-query/log-query-overview.md).

### <a name="single-machine-inventory"></a>Tek makine stoku

Tek bir makinenin yazılım envanterini görmek için, Azure VM kaynak sayfasından envantere erişebilir veya Azure Izleyici günlüklerini kullanarak ilgili makineye filtre uygulayabilirsiniz.
Aşağıdaki örnek Log Analytics sorgu, **Contosovm**adlı bir makine için yazılım listesini döndürür.

```loganalytics
ConfigurationData
| where ConfigDataType == "Software"
| summarize arg_max(TimeGenerated, *) by SoftwareName, CurrentVersion
| where Computer =="ContosoVM"
| render table
| summarize by Publisher, SoftwareName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakiler de dahil olmak üzere yazılım stok bilgisini görüntülemeyi öğrendiniz:

> [!div class="checklist"]
> * Çözümü etkinleştirme
> * Bir Azure VM ekleme
> * Azure olmayan bir VM ekleme
> * Yüklü olan yazılımları görüntüleme
> * Yüklü olan yazılımların stok günlüklerinde arama yapma

Daha fazla bilgi edinmek için Değişiklik izleme ve Stok çözümü özetine göz atın.

> [!div class="nextstepaction"]
> [Değişiklik yönetimi ve Stok çözümü](automation-change-tracking.md)
