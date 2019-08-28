---
title: Öğretici - Azure’da Linux sanal makinelerini izleme ve güncelleştirme | Microsoft Docs
description: Bu öğreticide, bir Linux sanal makinesinde önyükleme tanılamaları ile performans ölçümlerinin nasıl izleneceğini ve paket güncelleştirmelerinin nasıl yönetileceğini öğreneceksiniz
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/26/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 64000f42798b321c937dc3d01bf22e594f2e6176
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70081699"
---
# <a name="tutorial-monitor-and-update-a-linux-virtual-machine-in-azure"></a>Öğretici: Azure 'da bir Linux sanal makinesini izleme ve güncelleştirme

Azure’daki sanal makinelerin (VM) düzgün bir şekilde çalıştığından emin olmak için önyükleme tanılamalarını ve performans ölçümlerini gözden geçirebilir ve paket güncelleştirmelerini yönetebilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * VM’de önyükleme tanılamalarını etkinleştirme
> * Önyükleme tanılamasını görüntüle
> * Konak ölçümlerini görüntüleme
> * VM’de tanılama uzantısını etkinleştirme
> * VM ölçümlerini görüntüleme
> * Tanılama ölçümlerine bağlı uyarılar oluşturma
> * Paket güncelleştirmelerini yönetme
> * Değişiklikleri ve sayımı izleme
> * Gelişmiş izlemeyi ayarlama

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.30 veya sonraki bir sürümünü çalıştırmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli).

## <a name="create-vm"></a>VM Oluştur

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

## <a name="enable-boot-diagnostics"></a>Önyükleme tanılamasını etkinleştirme

Linux VM’lerde önyükleme yapılırken önyükleme tanılama uzantısı, önyükleme çıktısını yakalar ve bunu Azure depolama alanında depolar. Bu veriler VM önyükleme sorunlarını gidermek için kullanılabilir. Azure CLI kullanarak bir Linux VM’si oluşturduğunuzda önyükleme tanılamaları otomatik olarak etkinleştirilmez.

Önyükleme tanılamalarını etkinleştirmeden önce, önyükleme günlüklerini depolamak için bir depolama hesabı oluşturulmalıdır. Depolama hesapları, genel olarak benzersiz bir ada sahip olmalıdır. Ad, 3 ile 24 karakter arasında olmalıdır ve yalnızca sayı ve küçük harf içermelidir. [az storage account create](/cli/azure/storage/account#az-storage-account-create) komutuyla bir depolama hesabı oluşturun. Bu örnekte benzersiz bir depolama hesabı oluşturmak için rastgele bir dize kullanılmaktadır.

```azurecli-interactive
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

Önyükleme tanılamasını etkinleştirirken blob depolama kapsayıcısının URI’sı gerekir. Aşağıdaki komut, bu URI’yı döndürmek için depolama hesabını sorgular. URI değeri, *bloburi* adlı bir değişkende depolanmaktadır ve bu değişken sonraki adımda kullanılabilir.

```azurecli-interactive
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Şimdi [az vm boot-diagnostics enable](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az-vm-boot-diagnostics-enable) komutuyla önyükleme tanılamasını etkinleştirin. `--storage` değeri, önceki adımda toplanan blob URI’sıdır.

```azurecli-interactive
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```

## <a name="view-boot-diagnostics"></a>Önyükleme tanılamasını görüntüle

Önyükleme tanılaması etkinleştirildiğinde, VM’yi durdurduğunuz ve başlattığınızda her seferinde önyükleme işlemiyle ilgili bilgiler bir günlük dosyasına yazılır. Bu örnekte öncelikle [az vm deallocate](/cli/azure/vm#az-vm-deallocate) komutuyla VM’yi şu şekilde serbest bırakın:

```azurecli-interactive
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Şimdi [az vm start]( /cli/azure/vm#az-vm-stop) komutuyla VM’yi şu şekilde başlatın:

```azurecli-interactive
az vm start --resource-group myResourceGroupMonitor --name myVM
```

*myVM* için önyükleme tanılama verilerini [az vm boot-diagnostics get-boot-log](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az-vm-boot-diagnostics-get-boot-log) komutuyla şu şekilde alabilirsiniz:

```azurecli-interactive
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```

## <a name="view-host-metrics"></a>Konak ölçümlerini görüntüleme

Linux VM’si, Azure’da etkileşimde bulunduğu ayrılmış bir konağa sahiptir. Konağa ait ölçümler otomatik olarak toplanır ve Azure portalında şu şekilde görüntülenebilir:

1. Azure portalında **Kaynak Grupları**’nı seçin, önce **myResourceGroupMonitor** seçeneğini belirtin ve ardından kaynak listesinden **myVM**’yi seçin.
1. Konak VM’nin performansını görüntülemek için VM penceresinde **Ölçümler**’i seçin ve ardından **Kullanılabilen ölçümler** bölümünden herhangi bir *[Konak]* ölçümünü seçin.

    ![Konak ölçümlerini görüntüleme](./media/tutorial-monitoring/monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>Tanılama uzantısını yükleme

Temel konak ölçümleri kullanılabilir, ancak daha ayrıntılı bilgileri ve VM’ye özel ölçümleri görüntülemek için Azure tanılama uzantısını VM’ye yüklemeniz gerekir. Azure tanılama uzantısı, VM’den ek izleme ve tanılama verilerinin alınmasına izin verir. Bu performans ölçümlerini görüntüleyebilir ve VM performansına bağlı uyarılar oluşturabilirsiniz. Tanılama uzantısı Azure portalından şu şekilde yüklenir:

1. Azure portalında **Kaynak Grupları**’nı seçin, önce **myResourceGroupMonitor** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.
1. **Tanılama ayarları**’nı seçin. *Bir depolama hesabı seçin* açılan menüsünde, önceki bölümde oluşturulan *mydiagdata[1234]* hesabını seçin (henüz seçili değilse).
1. **Konuk düzeyinde izlemeyi etkinleştir** düğmesini seçin.

    ![Tanılama ölçümlerini görüntüleme](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>VM ölçümlerini görüntüleme

VM ölçümlerini, konak VM ölçümlerini görüntülediğiniz gibi görüntüleyebilirsiniz:

1. Azure portalında **Kaynak Grupları**’nı seçin, önce **myResourceGroupMonitor** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.
1. VM’nin performansını görüntülemek için VM penceresinde **Ölçümler**’i seçin ve ardından *Kullanılabilen ölçümler* bölümünden herhangi bir **[Konuk]** tanılama ölçümünü seçin.

    ![VM ölçümlerini görüntüleme](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>Uyarı oluşturma

Belirli performans ölçümlerine bağlı uyarılar oluşturabilirsiniz. Uyarılar, ortalama CPU kullanımı belirli bir eşiği aştığında veya mevcut boş disk alanı belirli bir miktarın altına düştüğünde bildirim almak için kullanılabilir. Uyarılar Azure portalında görüntülenebilir veya e-posta ile gönderilebilir. Ayrıca oluşturulan uyarılara yanıt olarak Azure Otomasyonu runbook’larını veya Azure Logic Apps’i tetikleyebilirsiniz.

Aşağıdaki örnek, ortalama CPU kullanımı için bir uyarı oluşturur.

1. Azure portalında **Kaynak Grupları**’nı seçin, önce **myResourceGroupMonitor** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.
2. **Uyarılar (klasik)** öğesini seçin ve sonra da uyarılar penceresinin üst kısmındaki **Ölçüm uyarısı ekle (klasik)** öğesini seçin.
3. Uyarınız için *myAlertRule* gibi bir **Ad** girin
4. CPU yüzdesi beş dakika boyunca 1,0’ı aştığında bir uyarı tetiklemek için diğer varsayılan ayarların tümünü seçili bırakın.
5. İsteğe bağlı olarak e-posta bildirimi göndermek için *E-posta sahipleri, katkıda bulunanlar ve okuyucular* kutusunu işaretleyebilirsiniz. Varsayılan eylem olarak portalda bir bildirim sunulur.
6. **Tamam** düğmesini seçin.

## <a name="manage-software-updates"></a>Yazılım güncelleştirmelerini yönetme

Güncelleştirme yönetimi, Azure Linux sanal makineleriniz için güncelleştirme ve yamaları yönetmenize olanak sağlar.
VM’nizden doğrudan güncelleştirmelerin durumunu değerlendirebilir, gerekli güncelleştirmelerin yüklenmesini zamanlayabilir ve güncelleştirmelerin VM’ye başarılı bir şekilde uygulandığından emin olmak için dağıtım sonuçlarını gözden geçirebilirsiniz.

Fiyatlandırma bilgisi için bkz. [Güncelleştirme yönetimi için Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)

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

Doğrulama işlemi ayrıca VM 'nin Log Analytics Aracısı ve otomasyon karma Runbook Worker ile sağlanıp sağlanmadığını kontrol eder. Bu aracı, VM ile iletişim kurmak ve güncelleştirme durumu hakkında bilgi almak için kullanılır.

Log Analytics çalışma alanını ve otomasyon hesabını seçin ve çözümü etkinleştirmek için **Etkinleştir** ' i seçin. Çözümün etkinleştirilmesi 15 dakika sürer.

Ekleme sırasında aşağıdaki önkoşullardan birinin karşılanmadığı tespit edilirse ilgili önkoşul otomatik olarak eklenir:

* [Log Analytics](../../log-analytics/log-analytics-overview.md) çalışma alanı
* [Otomasyon hesabı](../../automation/automation-offering-get-started.md)
* VM üzerinde etkin bir [Karma runbook çalışanı](../../automation/automation-hybrid-runbook-worker.md)

**Güncelleştirme Yönetimi** ekranı açılır. Kullanılacak konumu, Log Analytics çalışma alanını ve otomasyon hesabını yapılandırın ve **Etkinleştir**' i seçin. Bu alanların gri renkte olması, VM için etkinleştirilmiş başka bir otomasyon çözümü olduğunu gösterir ve bu durumda aynı çalışma alanı ile Otomasyon hesabının kullanılması gerekir.

![Güncelleştirme yönetimi çözümünü etkinleştirme](./media/tutorial-monitoring/manage-updates-update-enable.png)

Çözümün etkinleştirilmesi 15 dakika sürebilir. Bu süre boyunca tarayıcı penceresini kapatmamanız gerekir. Çözüm etkinleştirildikten sonra VM 'deki eksik güncelleştirmeler hakkında bilgiler Azure Izleyici günlüklerine akar. Verilerin çözümlemeye hazır hale gelmesi 30 dakika ile 6 saat arasında sürebilir.

### <a name="view-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

**Güncelleştirme yönetimi** etkinleştirildikten sonra **Güncelleştirme yönetimi** ekranı görünür. Güncelleştirme değerlendirmesi tamamlandıktan sonra, **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin bir listesini görürsünüz.

 ![Güncelleştirme durumunu görüntüleme](./media/tutorial-monitoring/manage-updates-view-status-linux.png)

### <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın. Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

**Güncelleştirme yönetimi** ekranının üst kısmındaki **Güncelleştirme dağıtımı zamanla**’ya tıklayarak VM için yeni bir Güncelleştirme Dağıtımı zamanlayabilirsiniz. **Yeni güncelleştirme dağıtım** ekranında aşağıdaki bilgileri belirtin:

Yeni bir güncelleştirme dağıtımı oluşturmak için **güncelleştirme dağıtımı zamanla**' yı seçin. **Yeni güncelleştirme dağıtım** sayfası açılır. Aşağıdaki tabloda açıklanan özellikler için değerler girin ve ardından **Oluştur**' a tıklayın:

| Özellik | Description |
| --- | --- |
| Adı |Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad. |
|İşletim sistemi| Linux veya Windows|
| Güncelleştirilecek gruplar |Azure makinelerinde, dağıtımınıza dahil etmek için dinamik bir Azure VM grubu oluşturmaya yönelik bir abonelik, kaynak grubu, konum ve etiket birleşimine göre bir sorgu tanımlayın. </br></br>Azure dışı makineler için, dağıtıma dahil edilecek bir Azure dışı makine grubu seçmek üzere mevcut kayıtlı bir aramayı seçin. </br></br>Daha fazla bilgi için bkz. [Dinamik Gruplar](../../automation/automation-update-management.md#using-dynamic-groups)|
| Güncelleştirilecek makineler |Açılan listeden kaydedilmiş bir arama, Içeri aktarılan grup veya Select Machine seçin ve tek tek makineler ' i seçin. **Makineler**'i seçerseniz makinenin hazır olma durumu **GÜNCELLEŞTİRME ARACISI HAZIRLIĞI** sütununda gösterilir.</br> Azure Izleyici günlüklerinde bilgisayar grupları oluşturmaya yönelik farklı yöntemler hakkında bilgi edinmek için bkz. [Azure izleyici günlüklerinde bilgisayar grupları](../../azure-monitor/platform/computer-groups.md) |
|Güncelleştirme sınıflandırmaları|İhtiyaç duyduğunuz tüm güncelleştirme sınıflandırmalarını seçin|
|Güncelleştirmeleri dahil et/Dışla|Bu, **dahil etme/hariç tutma** sayfasını açar. Dahil edilecek veya dışlanacak güncelleştirmeler ayrı sekmelerdedir. İçerme hakkında daha fazla bilgi için bkz. [içerme davranışı](../../automation/automation-update-management.md#inclusion-behavior) |
|Zamanlama ayarları|Başlatılacak saati seçin ve yinelenme için bir kez veya yineleme seçin|
| Ön betikler + betikleri sonrası|Dağıtımdan önce ve sonra çalıştırılacak betikleri seçin|
| Bakım penceresi |Güncelleştirmeler için ayarlanan dakika sayısı. Değer 30 dakikadan kısa olamaz ve 6 saatten fazla olamaz |
| Yeniden başlatma denetimi| Yeniden başlatmaları nasıl ele alınacağını belirler. Kullanılabilen seçenekler:</br>Gerekirse yeniden başlat (Varsayılan)</br>Her zaman yeniden başlat</br>Hiçbir zaman yeniden başlatma</br>Yalnızca yeniden başlatma - güncelleştirmeleri yüklemez|

Güncelleştirme dağıtımları da programlı bir şekilde oluşturulabilir. REST API güncelleştirme dağıtımı oluşturmayı öğrenmek için bkz. [yazılım güncelleştirme yapılandırması-oluştur](/rest/api/automation/softwareupdateconfigurations/create). Ayrıca, haftalık güncelleştirme dağıtımı oluşturmak için kullanılabilecek bir örnek runbook vardır. Bu runbook hakkında daha fazla bilgi edinmek için bkz. [bir kaynak grubundaki bir veya daha fazla VM için haftalık güncelleştirme dağıtımı oluşturma](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1).

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

Bilgisayarlarınızda yazılım, dosya, Linux Daemon 'ları, Windows Hizmetleri ve Windows kayıt defteri anahtarlarının envanterini toplayabilir ve görüntüleyebilirsiniz. Makinelerinizin yapılandırmasını izlemek ortamınızdaki işletimsel sorunları bulmanıza ve makinelerinizin durumunu daha iyi anlamanıza yardımcı olabilir.

### <a name="enable-change-and-inventory-management"></a>Değişiklik ve Sayım yönetimini etkinleştirme

Sanal makineniz için Değişiklik ve Sayım yönetimini etkinleştirme:

1. Ekranın solundan **Sanal makineler**’i seçin.
2. Listeden bir VM seçin.
3. Sanal makine ekranında, **İşlemler** bölümünde **Sayım** veya **Değişiklik izleme**'yi seçin. **Değişiklik İzleme ve Sayımı Etkinleştir** ekranı açılır.

Kullanılacak konumu, Log Analytics çalışma alanını ve otomasyon hesabını yapılandırın ve **Etkinleştir**' i seçin. Bu alanların gri renkte olması, VM için etkinleştirilmiş başka bir otomasyon çözümü olduğunu gösterir ve bu durumda aynı çalışma alanı ile Otomasyon hesabının kullanılması gerekir. Çözümler menüde ayrı yerlerde olsa da aynıdır. Biri etkinleştirildiğinde, sanal makinenizde her ikisi de etkinleştirilir.

![Değişiklik ve Sayım izlemeyi etkinleştirme](./media/tutorial-monitoring/manage-inventory-enable.png)

Çözüm etkinleştirildikten sonra, veriler görüntülenmeden önce sanal makinede sayımın toplanması biraz zaman alabilir.

### <a name="track-changes"></a>Değişiklikleri izleme

Sanal makinenizde, **İŞLEMLER** bölümünden **Değişiklik İzleme**'yi seçin. **Ayarları Düzenle**’yi seçin; **Değişiklik İzleme** sayfası görüntülenir. İzlemek istediğiniz ayar türünü seçin ve **+ Ekle**’yi seçerek ayarları yapılandırın. Linux için kullanılabilir seçenek **Linux Dosyaları**’dır

Değişiklik İzleme hakkında ayrıntılı bilgi için bkz. [Sanal makinedeki değişiklik sorunlarını giderme](../../automation/automation-tutorial-troubleshoot-changes.md)

### <a name="view-inventory"></a>Sayımı görüntüleme

Sanal makinenizde **İŞLEMLER** bölümünden **Sayım**’ı seçin. **Yazılım** sekmesinde bulunan yazılımların listelendiği bir tablo yer alır. Tabloda her yazılım kaydı hakkındaki üst düzey ayrıntılar görüntülenebilir. Bu ayrıntılar arasında yazılım adı, sürüm, yayımcı, son yenilenme zamanı yer alır.

![Sayımı görüntüleme](./media/tutorial-monitoring/inventory-view-results.png)

### <a name="monitor-activity-logs-and-changes"></a>Etkinlik günlüklerini ve değişiklikleri izleme

VM'nizdeki **Değişik izleme** sayfasında **Etkinlik Günlüğü Bağlantısını Yönetme**'yi seçin. Bu görev **Azure Etkinlik günlüğü** sayfasını açar. Değişiklik izleme özelliğini VM'nizin Azure etkinlik günlüğü verilerine bağlamak için **Bağlan**'ı seçin.

Bu ayar etkin durumdayken VM'nizin **Özet** sayfasına gidip **Durdur**'u seçerek VM'nizi durdurun. Sorulduğunda **Evet**'i seçerek VM'yi durdurun. Serbest bırakıldıktan sonra **Başlat**'ı seçerek VM'nizi yeniden başlatın.

VM'yi durdurup başlattığınızda etkinlik günlüğüne bir olay kaydedilir. **Değişiklik izleme** sayfasına dönün. Sayfanın en altındaki **Olaylar** sekmesini seçin. Bir süre sonra olaylar grafik ve tablo şeklinde gösterilir. Her bir olay seçilerek olayla ilgili ayrıntılı bilgiler görüntülenebilir.

![Etkinlik günlüğünde değişiklikleri görüntüleme](./media/tutorial-monitoring/manage-activitylog-view-results.png)

Grafik, zaman içinde gerçekleştirilen değişiklikleri gösterir. Etkinlik Günlüğü bağlantısı eklediğinizde en üstteki çizgi grafik Azure Etkinlik Günlüğü olaylarını görüntüler. Çubuk grafiklerin her satırı farklı bir izlenebilir Değişiklik türünü temsil eder. Bu türler, Linux daemon, dosya ve yazılım türleridir. Değişiklik sekmesinde görselleştirmede gösterilen değişikliklerle ilgili ayrıntılar, değişikliğin oluşma zamanına göre azalan düzende (en son gerçekleşen en başta) gösterilir.

## <a name="advanced-monitoring"></a>Gelişmiş izleme

Azure sanal makinelerinizi (VM), farklı işlemleri de dahil olmak üzere Windows ve Linux sanal makinelerinizin performansını ve sistem durumunu analiz ederek ölçeklendirerek [VM'ler için Azure izleyici](../../azure-monitor/insights/vminsights-overview.md)gibi bir çözüm kullanarak VM 'nizin daha gelişmiş bir şekilde izlenmesini sağlayabilirsiniz. ve diğer kaynaklardaki ve dış süreçlerdeki birbirine bağlı bağımlılıklardır. Azure sanal makinelerinizin yapılandırma yönetimi, ortamınızdaki değişiklikleri kolayca belirlemek için [Azure otomasyonu](../../automation/automation-intro.md) değişiklik izleme ve envanter çözümü ile birlikte sunulur. Güncelleştirme uyumluluğunu yönetmek, Azure Otomasyonu Güncelleştirme Yönetimi çözümüyle birlikte sağlanır.   

VM 'nin bağlı olduğu Log Analytics çalışma alanından, toplanan verileri [zengin sorgu diliyle](../../azure-monitor/log-query/log-query-overview.md)de alabilir, birleştirebilir ve çözümleyebilirsiniz. 

![Log Analytics çalışma alanı](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir VM için güncelleştirmeleri yapılandırdınız, gözden geçirdiniz ve yönetimini gerçekleştirdiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * VM’de önyükleme tanılamalarını etkinleştirme
> * Önyükleme tanılamasını görüntüle
> * Konak ölçümlerini görüntüleme
> * VM’de tanılama uzantısını etkinleştirme
> * VM ölçümlerini görüntüleme
> * Tanılama ölçümlerine bağlı uyarılar oluşturma
> * Paket güncelleştirmelerini yönetme
> * Değişiklikleri ve sayımı izleme
> * Gelişmiş izlemeyi ayarlama

Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [VM güvenliğini yönetme](../../security/fundamentals/overview.md)
