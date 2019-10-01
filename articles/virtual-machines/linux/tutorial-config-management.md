---
title: Öğretici-Azure 'da Linux sanal makine yapılandırmasını yönetme | Microsoft Docs
description: Bu öğreticide, bir Linux sanal makinesinde değişiklikleri belirlemeyi ve paket güncelleştirmelerini yönetmeyi öğreneceksiniz
services: virtual-machines-linux
documentationcenter: virtual-machines
author: mgoedtel
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/27/2019
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 78c924ebe15186025f4f2a79f87be6fb4fbf5db9
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71680053"
---
# <a name="tutorial-monitor-changes-and-update-a-linux-virtual-machine-in-azure"></a>Öğretici: Azure 'da değişiklikleri Izleme ve Linux sanal makinesini güncelleştirme

Azure [değişiklik izleme](../../automation/change-tracking.md) , değişiklikleri kolayca tanımlamanızı ve [güncelleştirme yönetimi](../../automation/automation-update-management.md) Azure Linux sanal makinelerinize yönelik işletim sistemi güncelleştirmelerini yönetmenizi sağlar.

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Windows güncelleştirmelerini yönetme
> * Değişiklikleri ve envanteri izleme

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell Başlat

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Önceden yüklenmiş ve hesabınızla kullanılmak üzere yapılandırılmış ortak Azure araçları vardır. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğreticide, Azure CLı sürüm 2.0.30 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` ' yı çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek]( /cli/azure/install-azure-cli).

## <a name="create-vm"></a>VM oluşturma

Tanılama ve ölçümleri eylem bölümünde görmek için bir VM 'ye ihtiyacınız vardır. İlk olarak, [az Group Create](/cli/azure/group#az-group-create)ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *Myresourcegroupmonitor* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroupMonitor --location eastus
```

Şimdi [az VM Create](/cli/azure/vm#az-vm-create)Ile bir VM oluşturun. Aşağıdaki örnek, *myvm* ADLı bir VM oluşturur ve şu anda *~/.SSH/* dizininde yoksa SSH anahtarları oluşturur:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="manage-software-updates"></a>Yazılım güncelleştirmelerini yönetme

Güncelleştirme yönetimi, Azure Linux sanal makinelerinize yönelik güncelleştirmeleri ve düzeltme eklerini yönetmenizi sağlar.
Doğrudan VM 'nizden, kullanılabilir güncelleştirmelerin durumunu hızlı bir şekilde değerlendirebilir, gerekli güncelleştirmelerin yüklemesini zamanlayabilir ve güncelleştirmelerin VM 'ye başarıyla uygulandığını doğrulamak için dağıtım sonuçlarını gözden geçirebilirsiniz.

Fiyatlandırma bilgileri için bkz. [güncelleştirme yönetimi Için Otomasyon Fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)

### <a name="enable-update-management"></a>Güncelleştirme yönetimini etkinleştir

VM 'niz için güncelleştirme yönetimini etkinleştirme:

1. Ekranın sol tarafında, **sanal makineler**' i seçin.
2. Listeden bir VM seçin.
3. VM ekranında, **işlemler** bölümünde **güncelleştirme yönetimi**' ni seçin. **Etkinleştir güncelleştirme yönetimi** ekranı açılır.

Bu VM için güncelleştirme yönetiminin etkinleştirilip etkinleştirilmediğini belirleme doğrulaması gerçekleştirilir.
Doğrulama Log Analytics çalışma alanı ve bağlantılı Otomasyon hesabı için denetim içerir ve çözüm çalışma alanında ise.

[Log Analytics](../../log-analytics/log-analytics-overview.md) çalışma alanı, güncelleştirme yönetimi gibi özellikler ve hizmetler tarafından oluşturulan verileri toplamak için kullanılır.
Çalışma alanı, birden fazla kaynaktaki verileri gözden geçirmek ve analiz etmek için tek bir konum sağlar.
Güncelleştirme gerektiren VM 'lerde ek eylemler gerçekleştirmek için Azure Otomasyonu, güncelleştirmeleri indirme ve uygulama gibi VM 'Lere karşı çalıştırmanızı sağlar.

Doğrulama işlemi ayrıca VM 'nin Log Analytics Aracısı ve otomasyon karma Runbook Worker ile sağlanıp sağlanmadığını kontrol eder. Bu aracı, VM ile iletişim kurmak ve güncelleştirme durumu hakkında bilgi almak için kullanılır.

Log Analytics çalışma alanını ve otomasyon hesabını seçin ve çözümü etkinleştirmek için **Etkinleştir** ' i seçin. Çözümün etkinleştirilmesi 15 dakika sürer.

Ekleme sırasında aşağıdaki önkoşulların herhangi biri eksik olarak bulunursa, otomatik olarak eklenir:

* [Log Analytics](../../log-analytics/log-analytics-overview.md) çalışma alanı
* [Otomasyon hesabı](../../automation/automation-offering-get-started.md)
* VM 'de [karma Runbook Worker](../../automation/automation-hybrid-runbook-worker.md) etkin

**Güncelleştirme yönetimi** ekranı açılır. Kullanılacak konumu, Log Analytics çalışma alanını ve otomasyon hesabını yapılandırın ve **Etkinleştir**' i seçin. Alanlar gri ise, VM için başka bir otomasyon çözümünün etkinleştirildiği ve aynı çalışma alanı ile otomasyon hesabının kullanılması gerektiği anlamına gelir.

![Güncelleştirme yönetimi çözümünü etkinleştir](./media/tutorial-monitoring/manage-updates-update-enable.png)

Çözümün etkinleştirilmesi 15 dakika kadar sürebilir. Bu süre boyunca tarayıcı penceresini kapatmamanız gerekir. Çözüm etkinleştirildikten sonra VM 'deki eksik güncelleştirmeler hakkında bilgiler Azure Izleyici günlüklerine akar. Verilerin analiz için kullanılabilmesi 30 dakika ile 6 saat arasında sürebilir.

### <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüle

**Güncelleştirme yönetimi** etkinleştirildikten sonra **güncelleştirme yönetimi** ekranı görüntülenir. Güncelleştirmelerin değerlendirmesi tamamlandıktan sonra **eksik** Güncelleştirmeler sekmesinde eksik güncelleştirmelerin bir listesini görürsünüz.

 ![Güncelleştirme durumunu görüntüle](./media/tutorial-monitoring/manage-updates-view-status-linux.png)

### <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın. Dağıtıma dahil edilecek güncelleştirme türlerini seçebilirsiniz. Örneğin, kritik güncelleştirmeler veya güvenlik güncelleştirmeleri ekleyebilir ve güncelleştirme paketlerini dışlayabilirsiniz.

**Güncelleştirme yönetimi** ekranının üst kısmındaki **güncelleştirme dağıtımı zamanla** ' ya tıklayarak VM Için yeni bir güncelleştirme dağıtımı zamanlayın. **Yeni güncelleştirme dağıtım** ekranında, aşağıdaki bilgileri belirtin:

Yeni bir güncelleştirme dağıtımı oluşturmak için **güncelleştirme dağıtımı zamanla**' yı seçin. **Yeni güncelleştirme dağıtım** sayfası açılır. Aşağıdaki tabloda açıklanan özellikler için değerler girin ve ardından **Oluştur**' a tıklayın:

| Özellik | Açıklama |
| --- | --- |
| Name |Güncelleştirme dağıtımını tanımlamak için benzersiz ad. |
|İşletim sistemi| Linux veya Windows|
| Güncelleştirilecek gruplar |Azure makinelerinde, dağıtımınıza dahil etmek için dinamik bir Azure VM grubu oluşturmaya yönelik bir abonelik, kaynak grubu, konum ve etiket birleşimine göre bir sorgu tanımlayın. </br></br>Azure dışı makineler için, dağıtıma dahil edilecek bir Azure dışı makine grubu seçmek üzere mevcut kayıtlı bir aramayı seçin. </br></br>Daha fazla bilgi için bkz. [Dinamik Gruplar](../../automation/automation-update-management.md#using-dynamic-groups)|
| Güncelleştirilecek makineler |Açılan listeden kaydedilmiş bir arama, Içeri aktarılan grup veya Select Machine seçin ve tek tek makineler ' i seçin. **Makineler**' i seçerseniz makinenin hazırlığı, **Güncelleştirme Aracısı hazırlığı** sütununda gösterilir.</br> Azure Izleyici günlüklerinde bilgisayar grupları oluşturmaya yönelik farklı yöntemler hakkında bilgi edinmek için bkz. [Azure izleyici günlüklerinde bilgisayar grupları](../../azure-monitor/platform/computer-groups.md) |
|Güncelleştirme sınıflandırmaları|İhtiyaç duyduğunuz tüm güncelleştirme sınıflandırmalarını seçin|
|Güncelleştirmeleri dahil et/Dışla|Bu, **dahil etme/hariç tutma** sayfasını açar. Dahil edilecek veya hariç tutulacak güncelleştirmeler ayrı sekmelerde bulunur. İçerme hakkında daha fazla bilgi için bkz. [içerme davranışı](../../automation/automation-update-management.md#inclusion-behavior) |
|Zamanlama ayarları|Başlatılacak saati seçin ve yinelenme için bir kez veya yineleme seçin|
| Ön betikler + betikleri sonrası|Dağıtımdan önce ve sonra çalıştırılacak betikleri seçin|
| Bakım penceresi |Güncelleştirmeler için ayarlanan dakika sayısı. Değer 30 dakikadan kısa olamaz ve 6 saatten fazla olamaz |
| Yeniden başlatma denetimi| Yeniden başlatmaları nasıl ele alınacağını belirler. Kullanılabilir seçenekler şunlardır:</br>Gerekirse yeniden Başlat (varsayılan)</br>Her zaman yeniden Başlat</br>Hiçbir şekilde yeniden başlatma</br>Yalnızca yeniden başlatma-güncelleştirmeleri yüklemez|

Güncelleştirme dağıtımları da programlı bir şekilde oluşturulabilir. REST API güncelleştirme dağıtımı oluşturmayı öğrenmek için bkz. [yazılım güncelleştirme yapılandırması-oluştur](/rest/api/automation/softwareupdateconfigurations/create). Ayrıca, haftalık güncelleştirme dağıtımı oluşturmak için kullanılabilecek bir örnek runbook vardır. Bu runbook hakkında daha fazla bilgi edinmek için bkz. [bir kaynak grubundaki bir veya daha fazla VM için haftalık güncelleştirme dağıtımı oluşturma](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1).

Zamanlamayı yapılandırmayı tamamladıktan sonra **Oluştur** düğmesine tıklayın ve durum panosuna geri dönün.
**Zamanlanan** tablonun oluşturduğunuz dağıtım zamanlamasını gösterdiğini unutmayın.

### <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanan dağıtım başladıktan sonra, bu dağıtımın durumunu **güncelleştirme yönetimi** ekranındaki **güncelleştirme dağıtımları** sekmesinde görebilirsiniz.
Şu anda çalışıyorsa, durumu **devam ediyor**olarak gösterilir. İşlem tamamlandıktan sonra **başarılı olarak değişir.**
Dağıtımdaki bir veya daha fazla güncelleştirme ile ilgili bir hata varsa durum **kısmen başarısız**olur.
Bu güncelleştirme dağıtımının panosunu görmek için tamamlanan güncelleştirme dağıtımını seçin.

![Belirli bir dağıtım için dağıtım durumu panosunu Güncelleştir](./media/tutorial-monitoring/manage-updates-view-results.png)

**Güncelleştirme sonuçları** kutucuğunda, sanal makine üzerindeki toplam güncelleştirme sayısı ve dağıtım sonuçlarının bir özeti yer alır.
Sağdaki tabloda, her bir güncelleştirmenin ayrıntılı bir dökümü ve yükleme sonuçları aşağıdaki değerlerden biri olabilir:

* **Denenmedi** -tanımlanan bakım penceresi süresine göre yeterli süre olmadığından güncelleştirme yüklenmedi.
* **Başarılı** -güncelleştirme başarılı oldu
* **Başarısız** -güncelleştirme başarısız oldu

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm Günlükler** ' i seçin.

Hedef sanal makinede güncelleştirme dağıtımını yönetmekten sorumlu runbook 'un iş akışını görmek için **Çıkış** kutucuğunu seçin.

Dağıtımdan oluşan herhangi bir hata hakkında ayrıntılı bilgileri görmek için **hatalar** ' ı seçin.

## <a name="monitor-changes-and-inventory"></a>Değişiklikleri ve envanteri izleme

Bilgisayarlarınızda yazılım, dosya, Linux Daemon 'ları, Windows Hizmetleri ve Windows kayıt defteri anahtarlarının envanterini toplayabilir ve görüntüleyebilirsiniz. Makinelerinizin yapılandırmalarının izlenmesi, ortamınızda işletimsel sorunları belirlemenize ve makinelerinizin durumunu daha iyi anlamanıza yardımcı olabilir.

### <a name="enable-change-and-inventory-management"></a>Değişiklik ve stok yönetimini etkinleştir

VM 'niz için değişiklik ve stok yönetimini etkinleştirin:

1. Ekranın sol tarafında, **sanal makineler**' i seçin.
2. Listeden bir VM seçin.
3. VM ekranında, **işlemler** bölümünde, **Envanter** veya **değişiklik izleme**' yi seçin. **Değişiklik izleme etkinleştirme ve envanter** ekranı açılır.

Kullanılacak konumu, Log Analytics çalışma alanını ve otomasyon hesabını yapılandırın ve **Etkinleştir**' i seçin. Alanlar gri ise, VM için başka bir otomasyon çözümünün etkinleştirildiği ve aynı çalışma alanı ile otomasyon hesabının kullanılması gerektiği anlamına gelir. Çözümler menüde ayrı olsa da, aynı çözümdür. Bunun etkinleştirilmesi VM 'niz için her ikisini de etkinleştirir.

![Değişiklik ve stok izlemeyi etkinleştir](./media/tutorial-monitoring/manage-inventory-enable.png)

Çözüm etkinleştirildikten sonra, veriler görüntülenmeden önce VM 'de sayım toplanırken zaman alabilir.

### <a name="track-changes"></a>Değişiklikleri izle

SANAL makinenizde, **işlemler**altında **değişiklik izleme** ' yi seçin. **Ayarları Düzenle**' yi seçtiğinizde **değişiklik izleme** sayfası görüntülenir. İzlemek istediğiniz ayar türünü seçin ve ardından ayarları yapılandırmak için **+ Ekle** ' yi seçin. Kullanılabilir Linux Linux **dosyaları**

Değişiklik İzleme hakkında ayrıntılı bilgi için bkz. [VM 'deki değişikliklerle Ilgili sorunları giderme](../../automation/automation-tutorial-troubleshoot-changes.md)

### <a name="view-inventory"></a>Stoku görüntüleme

SANAL makinenizde, **işlemler**bölümünde **Stok** ' ı seçin. **Yazılım** sekmesinde, bulunan yazılımın listesini içeren bir tablo vardır. Her yazılım kaydı için üst düzey Ayrıntılar tabloda görüntülenebilir. Bu ayrıntılar yazılım adı, sürüm, Yayımcı, son yenilenme zamanı içerir.

![Stoku görüntüleme](./media/tutorial-monitoring/inventory-view-results.png)

### <a name="monitor-activity-logs-and-changes"></a>Etkinlik günlüklerini ve değişiklikleri izleme

SANAL makinenizin **değişiklik izleme** sayfasında, **etkinlik günlüğü bağlantısını Yönet**' i seçin. Bu görev, **Azure etkinlik günlüğü** sayfasını açar. Değişiklik izlemeyi VM 'niz için Azure etkinlik günlüğü ' ne bağlamak için **Bağlan** ' ı seçin.

Bu ayar etkinken, sanal makinenizin **genel bakış** sayfasına gıdın ve VM 'nizi durdurmak için **Durdur** ' u seçin. İstendiğinde, VM 'yi durdurmak için **Evet** ' i seçin. Serbest bırakıldığında, sanal makineyi yeniden başlatmak için **Başlat** ' ı seçin.

VM 'yi durdurma ve başlatma etkinlik günlüğünde bir olay günlüğe kaydeder. **Değişiklik izleme** sayfasına geri gidin. Sayfanın alt kısmındaki **Olaylar** sekmesini seçin. Bir süre sonra, grafikte ve tablosunda gösterilen olaylar. Olay hakkında ayrıntılı bilgi görüntülemek için her bir olay seçilebilir.

![Etkinlik günlüğündeki değişiklikleri görüntüle](./media/tutorial-monitoring/manage-activitylog-view-results.png)

Grafik zaman içinde oluşan değişiklikleri gösterir. Bir etkinlik günlüğü bağlantısı ekledikten sonra üstteki çizgi grafiğinde Azure etkinlik günlüğü olayları görüntülenir. Çubuk grafiklerin her bir satırı, farklı bir rafa yerleştirilebilir değişiklik türünü temsil eder. Bu türler Linux Daemon 'ları, dosyalar ve yazılımlardır. Değişiklik sekmesi, görselleştirmede gösterilen değişikliklere ilişkin ayrıntıları, değişikliğin gerçekleştiği zamana göre (en son ilk sırada) gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, VM 'niz için Değişiklik İzleme ve Güncelleştirme Yönetimi yapılandırdınız ve gözden geçirdiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Kaynak grubu ve VM oluşturma
> * Linux güncelleştirmelerini yönetme
> * Değişiklikleri ve envanteri izleme

VM 'nizi izleme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Sanal makineleri izleme](tutorial-monitor.md)