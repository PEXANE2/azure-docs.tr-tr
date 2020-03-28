---
title: Öğretici - Azure'da Linux sanal makine yapılandırması yönetin
description: Bu eğitimde, değişiklikleri nasıl tanımlayabileceğinizi ve Bir Linux sanal makinesinde paket güncellemelerini nasıl yönetersiniz
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
ms.openlocfilehash: 5dd35ed2f4533d19cc3a59788d6422416dce13f5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239297"
---
# <a name="tutorial-monitor-changes-and-update-a-linux-virtual-machine-in-azure"></a>Öğretici: Azure'da değişiklikleri izleyin ve bir Linux sanal makinesini güncelleyin

Azure [Değişiklik İzleme,](../../automation/change-tracking.md) değişiklikleri kolayca belirlemenize olanak tanır ve [Update Yönetimi,](../../automation/automation-update-management.md) Azure Linux VM'leriniz için işletim sistemi güncelleştirmelerini yönetmenize olanak tanır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Windows güncelleştirmelerini yönetme
> * Değişiklikleri ve sayımı izleme

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.30 veya sonraki bir sürümünü çalıştırmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="create-vm"></a>VM oluşturma

Tanılama ve ölçüm özelliklerinin nasıl çalıştığını görmek için bir VM gerekir. Öncelikle [az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroupMonitor* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroupMonitor --location eastus
```

Şimdi [az vm create](/cli/azure/vm#az-vm-create) ile bir VM oluşturun. Aşağıdaki örnekte *myVM* adlı bir VM oluşturulur ve *~/.ssh/* içinde henüz yoksa SSH anahtarları oluşturulur:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="manage-software-updates"></a>Yazılım güncelleştirmelerini yönetme

Güncelleştirme yönetimi, Azure Linux sanal makineleriniz için güncelleştirme ve yamaları yönetmenize olanak sağlar.
VM’nizden doğrudan güncelleştirmelerin durumunu değerlendirebilir, gerekli güncelleştirmelerin yüklenmesini zamanlayabilir ve güncelleştirmelerin VM’ye başarılı bir şekilde uygulandığından emin olmak için dağıtım sonuçlarını gözden geçirebilirsiniz.

Fiyatlandırma bilgileri [için, Güncelleme yönetimi için Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)

### <a name="enable-update-management"></a>Güncelleştirme yönetimini etkinleştirme

Sanal makineniz için Güncelleştirme yönetimini etkinleştirme:

1. Ekranın solundan **Sanal makineler**’i seçin.
2. Listeden bir VM seçin.
3. VM ekranında **İşlemler** bölümünden **Güncelleştirme yönetimi**'ni seçin. **Güncelleştirme Yönetimini Etkinleştirme** ekranı açılır.

Bu VM için Güncelleştirme yönetimi özelliğinin etkin olup olmadığını belirlemek için doğrulama gerçekleştirilir.
Bu doğrulama kapsamında Log Analytics çalışma alanı ve bağlantılı Otomasyon hesabının yanı sıra çözümün çalışma alanında olup olmadığı kontrol edilir.

[Log Analytics](../../log-analytics/log-analytics-overview.md) çalışma alanı, Güncelleştirme yönetimi gibi özellikler ve hizmetler tarafından oluşturulan verileri toplamak için kullanılır.
Çalışma alanı, birden fazla kaynaktan alınan verilerin incelenip analiz edilebileceği ortak bir konum sağlar.
Azure Otomasyonu, güncelleştirme yapılması gereken VM'lerde güncelleştirme indirme ve uygulama gibi ek işlemleri gerçekleştirmek için VM'ler üzerinde runbook'lar çalıştırmanızı sağlar.

Doğrulama işlemi, VM'nin Log Analytics aracısı ve Otomasyon karma runbook çalışanıyla sağlanıp sağlanmadÝðýný da denetler. Bu aracı, VM ile iletişim kurmak ve güncelleştirme durumu hakkında bilgi almak için kullanılır.

Log Analytics çalışma alanı ve otomasyon hesabını seçin ve çözümü etkinleştirmek için **Etkinleştir'i** seçin. Çözümün etkinleştirilmesi 15 dakika sürer.

Ekleme sırasında aşağıdaki önkoşullardan birinin karşılanmadığı tespit edilirse ilgili önkoşul otomatik olarak eklenir:

* [Günlük Analytics](../../log-analytics/log-analytics-overview.md) çalışma alanı
* [Otomasyon hesabı](../../automation/automation-offering-get-started.md)
* VM üzerinde etkin bir [Karma runbook çalışanı](../../automation/automation-hybrid-runbook-worker.md)

**Güncelleştirme Yönetimi** ekranı açılır. Konumu, Log Analytics çalışma alanını ve Otomasyon hesabını kullanmak ve Etkinleştir'i seçmek için **yapılandırın.** Bu alanların gri renkte olması VM için etkinleştirilmiş başka bir otomasyon çözümü olduğunu gösterir ve bu durumda aynı çalışma alanı ile Otomasyon hesabının kullanılması gerekir.

![Güncelleştirme yönetimi çözümünü etkinleştirme](./media/tutorial-monitoring/manage-updates-update-enable.png)

Çözümün etkinleştirilmesi 15 dakika sürebilir. Bu süre boyunca tarayıcı penceresini kapatmamanız gerekir. Çözüm etkinleştirildikten sonra, VM akışlarındaki eksik güncelleştirmelerle ilgili bilgiler Azure Monitor günlüklerine akar. Verilerin çözümlemeye hazır hale gelmesi 30 dakika ile 6 saat arasında sürebilir.

### <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

**Güncelleştirme yönetimi** etkinleştirildikten sonra **Güncelleştirme yönetimi** ekranı görünür. Güncelleştirme değerlendirmesi tamamlandıktan sonra, **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin bir listesini görürsünüz.

 ![Güncelleştirme durumunu görüntüleme](./media/tutorial-monitoring/manage-updates-view-status-linux.png)

### <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın. Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

**Güncelleştirme yönetimi** ekranının üst kısmındaki **Güncelleştirme dağıtımı zamanla**’ya tıklayarak VM için yeni bir Güncelleştirme Dağıtımı zamanlayabilirsiniz. **Yeni güncelleştirme dağıtım** ekranında aşağıdaki bilgileri belirtin:

Yeni bir güncelleştirme dağıtımı oluşturmak **için, Güncelleştir dağıtımını Zamanla'yı**seçin. **Yeni güncelleştirme dağıtım** sayfası açılır. Aşağıdaki tabloda açıklanan özellikler için değerleri girin ve sonra **Oluştur'u**tıklatın:

| Özellik | Açıklama |
| --- | --- |
| Adı |Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad. |
|İşletim Sistemi| Linux veya Windows|
| Güncelleştirilen gruplar |Azure makineleri için, dağıtımınıza dahil etmek üzere dinamik bir Azure VM grubu oluşturmak için abonelik, kaynak grupları, konumlar ve etiketlerin birleşimini temel alan bir sorgu tanımlayın. </br></br>Azure'a uygun olmayan makineler için, dağıtıma dahil etmek üzere bir grup Azure dışı makine seçmek için varolan kaydedilmiş bir aramayı seçin. </br></br>Daha fazla bilgi için [Dinamik Gruplar'a](../../automation/automation-update-management-groups.md) bakın|
| Güncelleştirilen makineler |Kaydedilen arama, Alınan grup veya açılan makineden Makine'yi seçin ve tek tek makineleri seçin. **Makineler**'i seçerseniz makinenin hazır olma durumu **GÜNCELLEŞTİRME ARACISI HAZIRLIĞI** sütununda gösterilir.</br> Azure Monitor günlüklerinde bilgisayar grupları oluşturmanın farklı yöntemleri hakkında bilgi edinmek için [Azure Monitor günlüklerinde Bilgisayar gruplarına](../../azure-monitor/platform/computer-groups.md) bakın |
|Update classifications|İhtiyacınız olan tüm güncelleştirme sınıflandırmalarını seçin|
|Güncelleştirmeleri ekleme/hariç tutma|Bu, **Ekle/Hariçle** sayfasını açar. Dahil edilecek veya dışlanacak güncelleştirmeler ayrı sekmelerdedir. İçermenin nasıl işlenirhakkında daha fazla bilgi için [bkz.](../../automation/automation-tutorial-update-management.md#schedule-an-update-deployment) |
|Zamanlama ayarları|Başlangıç saatini seçin ve yineleme için bir kez veya yinelenen birini seçin|
| Ön komut dosyaları + Post-scriptler|Dağıtımdan önce ve sonra çalışacak komut dosyalarını seçin|
| Bakım penceresi |Güncelleştirmeler için ayarlanan dakika sayısı. Değer 30 dakikadan az ve 6 saatten fazla olamaz |
| Yeniden başlatma denetimi| Yeniden başlatmaların nasıl işleneceğini belirler. Kullanılabilen seçenekler:</br>Gerekirse yeniden başlat (Varsayılan)</br>Her zaman yeniden başlat</br>Hiçbir zaman yeniden başlatma</br>Yalnızca yeniden başlatma - güncelleştirmeleri yüklemez|

Güncelleştirme Dağıtımları da programlı olarak oluşturulabilir. REST API ile Güncelleştirme Dağıtımı'nı nasıl oluşturabilirsiniz öğrenmek için Bkz. [Yazılım Güncelleştirme Yapılandırmaları - Oluştur](/rest/api/automation/softwareupdateconfigurations/create). Ayrıca, haftalık Güncelleştirme Dağıtımı oluşturmak için kullanılabilecek örnek bir runbook da vardır. Bu runbook hakkında daha fazla bilgi edinmek [için](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1)bkz.

Zamanlamayı yapılandırmayı tamamladıktan sonra **Oluştur** düğmesine tıklayın ve durum panosuna dönün.
**Zamanlanan** tablosunda oluşturduğunuz dağıtım zamanlaması görüntülenir.

### <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başladıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** ekranındaki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz.
O anda çalışıyorsa, durumu **Devam ediyor** olarak gösterilir. Tamamlandıktan sonra başarılı olursa durum **Başarılı** olarak değişir.
Dağıtımdaki bir veya daha fazla güncelleştirme ile ilgili hata varsa durum **Kısmen başarısız** şeklindedir.
Tamamlanan güncelleştirme dağıtımını seçerek bu güncelleştirme dağıtımının panosunu görebilirsiniz.

![Belirli bir dağıtım için Güncelleştirme Dağıtımı durum panosu](./media/tutorial-monitoring/manage-updates-view-results.png)

**Güncelleştirme sonuçları** kutucuğunda toplam güncelleştirme sayısının bir özeti ve VM’deki dağıtım sonuçları gösterilir.
Sağdaki tabloda her güncelleştirmenin ayrıntılı bir dökümü ile yükleme sonuçları gösterilir ve aşağıdaki değerlerden biri olabilir:

* **Denenmedi**: Tanımlanan bakım penceresi süresine göre yeterli süre olmadığından güncelleştirme yüklenmedi.
* **Başarılı**: Güncelleştirme başarılı oldu
* **Başarısız**: Güncelleştirme başarısız oldu

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükler**’i seçin.

Hedef VM'de güncelleştirme dağıtımını yönetmekten sorumlu runbook'un iş akışını görmek için **Çıktı** kutucuğunu seçin.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgiler için **Hatalar**’ı seçin.

## <a name="monitor-changes-and-inventory"></a>Değişiklikleri ve sayımı izleme

Bilgisayarlarınızda yazılım, dosya, Linux daemons, Windows Hizmetleri ve Windows kayıt defteri anahtarları için envanter toplayabilir ve görüntüleyebilirsiniz. Makinelerinizin yapılandırmasını izlemek ortamınızdaki işletimsel sorunları bulmanıza ve makinelerinizin durumunu daha iyi anlamanıza yardımcı olabilir.

### <a name="enable-change-and-inventory-management"></a>Değişiklik ve Sayım yönetimini etkinleştirme

Sanal makineniz için Değişiklik ve Sayım yönetimini etkinleştirme:

1. Ekranın solundan **Sanal makineler**’i seçin.
2. Listeden bir VM seçin.
3. Sanal makine ekranında, **İşlemler** bölümünde **Sayım** veya **Değişiklik izleme**'yi seçin. **Değişiklik İzleme ve Sayımı Etkinleştir** ekranı açılır.

Konumu, Log Analytics çalışma alanını ve Otomasyon hesabını kullanmak ve Etkinleştir'i seçmek için **yapılandırın.** Bu alanların gri renkte olması VM için etkinleştirilmiş başka bir otomasyon çözümü olduğunu gösterir ve bu durumda aynı çalışma alanı ile Otomasyon hesabının kullanılması gerekir. Çözümler menüde ayrı yerlerde olsa da aynıdır. Biri etkinleştirildiğinde, sanal makinenizde her ikisi de etkinleştirilir.

![Değişiklik ve Sayım izlemeyi etkinleştirme](./media/tutorial-monitoring/manage-inventory-enable.png)

Çözüm etkinleştirildikten sonra, veriler görüntülenmeden önce sanal makinede sayımın toplanması biraz zaman alabilir.

### <a name="track-changes"></a>Değişiklikleri izleme

VM'nizde, **OPERATIONS**altında **İzlemeyi Değiştir'i** seçin. **Ayarları Düzenle**’yi seçin; **Değişiklik İzleme** sayfası görüntülenir. İzlemek istediğiniz ayar türünü seçin ve **+ Ekle**’yi seçerek ayarları yapılandırın. Linux için kullanılabilir seçenek **Linux Dosyaları**’dır

Değişiklik İzleme hakkında ayrıntılı bilgi için bkz. [Sanal makinedeki değişiklik sorunlarını giderme](../../automation/automation-tutorial-troubleshoot-changes.md)

### <a name="view-inventory"></a>Sayımı görüntüleme

VM'nizde, **OPERATIONS**altında **Envanter'i** seçin. **Yazılım** sekmesinde bulunan yazılımların listelendiği bir tablo yer alır. Tabloda her yazılım kaydı hakkındaki üst düzey ayrıntılar görüntülenebilir. Bu ayrıntılar arasında yazılım adı, sürüm, yayımcı, son yenilenme zamanı yer alır.

![Sayımı görüntüleme](./media/tutorial-monitoring/inventory-view-results.png)

### <a name="monitor-activity-logs-and-changes"></a>Etkinlik günlüklerini ve değişiklikleri izleme

VM'nizdeki **Değişik izleme** sayfasında **Etkinlik Günlüğü Bağlantısını Yönetme**'yi seçin. Bu görev **Azure Etkinlik günlüğü** sayfasını açar. Değişiklik izleme özelliğini VM'nizin Azure etkinlik günlüğü verilerine bağlamak için **Bağlan**'ı seçin.

Bu ayar etkin durumdayken VM'nizin **Özet** sayfasına gidip **Durdur**'u seçerek VM'nizi durdurun. Sorulduğunda **Evet**'i seçerek VM'yi durdurun. Serbest bırakıldıktan sonra **Başlat**'ı seçerek VM'nizi yeniden başlatın.

VM'yi durdurup başlattığınızda etkinlik günlüğüne bir olay kaydedilir. **Değişiklik izleme** sayfasına dönün. Sayfanın en altındaki **Olaylar** sekmesini seçin. Bir süre sonra olaylar grafik ve tablo şeklinde gösterilir. Her bir olay seçilerek olayla ilgili ayrıntılı bilgiler görüntülenebilir.

![Etkinlik günlüğünde değişiklikleri görüntüleme](./media/tutorial-monitoring/manage-activitylog-view-results.png)

Grafik, zaman içinde gerçekleştirilen değişiklikleri gösterir. Etkinlik Günlüğü bağlantısı eklediğinizde en üstteki çizgi grafik Azure Etkinlik Günlüğü olaylarını görüntüler. Çubuk grafiklerin her satırı farklı bir izlenebilir Değişiklik türünü temsil eder. Bu türler, Linux daemon, dosya ve yazılım türleridir. Değişiklik sekmesinde görselleştirmede gösterilen değişikliklerle ilgili ayrıntılar, değişikliğin oluşma zamanına göre azalan düzende (en son gerçekleşen en başta) gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, VM'iniz için Değişiklik İzleme ve Güncelleştirme Yönetimi'ni yapılandırmış ve gözden geçirmişsiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Kaynak grubu ve sanal makine oluşturma
> * Linux güncellemelerini yönetme
> * Değişiklikleri ve sayımı izleme

VM izleme hakkında bilgi edinmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Sanal makineleri izleme](tutorial-monitor.md)
