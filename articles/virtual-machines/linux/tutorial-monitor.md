---
title: Öğretici-Azure 'da Linux sanal makinelerini Izleme | Microsoft Docs
description: Bu öğreticide, Linux sanal makinelerinizde çalışan performansı ve keşfedilen uygulama bileşenlerini izlemeyi öğreneceksiniz.
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
ms.date: 09/30/2019
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 9c6458eea2b1352e7d13ea6691eac4498182ecd3
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71680081"
---
# <a name="tutorial-monitor-a-linux-virtual-machine-in-azure"></a>Öğretici: Azure 'da Linux sanal makinesini Izleme

Azure izleme, Azure VM 'lerinden önyükleme ve performans verilerini toplamak, bu verileri Azure depolama 'da depolamak ve Portal, Azure PowerShell modülü ve Azure CLı aracılığıyla erişilebilir hale getirmek için aracıları kullanır. Gelişmiş izleme, performans ölçümleri toplayarak, VM 'de yüklü olan uygulama bileşenlerini keşfeterek ve performans grafiklerini ve bağımlılık eşlemesini içerdiğinden VM'ler için Azure İzleyici ile birlikte sunulur.

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * VM 'de önyükleme tanılamayı etkinleştirme
> * Önyükleme tanılamayı görüntüle
> * VM konak ölçümlerini görüntüleme
> * VM'ler için Azure İzleyici etkinleştir
> * VM performans ölçümlerini görüntüleme
> * Uyarı oluşturma

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell Başlat

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Önceden yüklenmiş ve hesabınızla kullanılmak üzere yapılandırılmış ortak Azure araçları vardır. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğreticide, Azure CLı sürüm 2.0.30 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` ' yı çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="create-vm"></a>VM oluşturma

Tanılama ve ölçümleri eylem bölümünde görmek için bir VM 'ye ihtiyacınız vardır. İlk olarak, [az Group Create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create)ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *Myresourcegroupmonitor* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroupMonitor --location eastus
```

Şimdi [az VM Create](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create)Ile bir VM oluşturun. Aşağıdaki örnek, *myvm* ADLı bir VM oluşturur ve şu anda *~/.SSH/* dizininde yoksa SSH anahtarları oluşturur:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a>Önyükleme tanılamayı etkinleştir

Linux VM 'Leri önyüklemesinde, önyükleme tanısı uzantısı önyükleme çıkışını yakalar ve Azure Storage 'da depolar. Bu veriler, VM önyükleme sorunlarını gidermek için kullanılabilir. Azure CLı kullanarak bir Linux VM oluşturduğunuzda, önyükleme tanılaması otomatik olarak etkinleştirilmez.

Önyükleme tanılamayı etkinleştirmeden önce, önyükleme günlüklerini depolamak için bir depolama hesabının oluşturulması gerekir. Depolama hesapları, genel olarak benzersiz bir ada sahip olmalıdır, 3 ila 24 karakter arasında olmalıdır ve yalnızca rakamlar ve küçük harfler içermelidir. [Az Storage Account Create](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create) komutuyla bir depolama hesabı oluşturun. Bu örnekte, benzersiz bir depolama hesabı adı oluşturmak için rastgele bir dize kullanılır.

```azurecli-interactive
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

Önyükleme tanılamayı etkinleştirirken, BLOB depolama kapsayıcısının URI 'SI gereklidir. Aşağıdaki komut, bu URI 'yi döndürecek depolama hesabını sorgular. URI değeri, bir sonraki adımda kullanılan *bloburi*adında bir ad olarak depolanır.

```azurecli-interactive
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Şimdi [az VM Boot-Diagnostics Enable](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az-vm-boot-diagnostics-enable)ile önyükleme tanılamayı etkinleştirin. @No__t-0 değeri, önceki adımda toplanan blob URI 'sidir.

```azurecli-interactive
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```

## <a name="view-boot-diagnostics"></a>Önyükleme tanılamayı görüntüle

Önyükleme tanılaması etkinleştirildiğinde, VM 'yi durdurup başlattığınızda, önyükleme işlemiyle ilgili bilgiler bir günlük dosyasına yazılır. Bu örnekte, önce [az VM ayırmayı kaldır](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-deallocate) komutuyla VM 'yi şu şekilde serbest bırakın:

```azurecli-interactive
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Şimdi [az VM start](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-start) komutuyla VM 'yi şu şekilde başlatın:

```azurecli-interactive
az vm start --resource-group myResourceGroupMonitor --name myVM
```

*Myvm* için önyükleme tanılama verilerini [az VM Boot-Diagnostics Get-boot-log](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az-vm-boot-diagnostics-get-boot-log) komutunu aşağıdaki gibi alabilirsiniz:

```azurecli-interactive
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```

## <a name="view-host-metrics"></a>Konak ölçümlerini görüntüleme

Bir Linux VM, Azure 'da etkileşimde bulunduğu adanmış bir konağa sahiptir. Ölçümler, ana bilgisayar için otomatik olarak toplanır ve Azure portal aşağıdaki gibi görüntülenebilir:

1. Azure portal, **kaynak grupları**' nı seçin, **Myresourcegroupmonitor**' ı seçin ve ardından kaynak listesinden **myvm** ' yi seçin.
1. Konak VM 'nin nasıl çalıştığını görmek için VM penceresinde **ölçümler** ' i seçin ve ardından **kullanılabilir ölçümler**altında *[ana bilgisayar]* ölçümlerinden birini seçin.

    ![Konak ölçümlerini görüntüleme](./media/tutorial-monitoring/monitor-host-metrics.png)

## <a name="enable-advanced-monitoring"></a>Gelişmiş izlemeyi etkinleştir

Azure VM 'nizi VM'ler için Azure İzleyici izlemeyi etkinleştirmek için:

1. Azure portal, **kaynak grupları**' na tıklayın, **Myresourcegroupmonitor**' ı seçin ve ardından kaynak listesinden **myvm** ' yi seçin.

2. VM sayfasında, **izleme** bölümünde Öngörüler ' i **(Önizleme)** seçin.

3. **Öngörüler (Önizleme)** sayfasında **Şimdi dene**' yi seçin.

    ![VM için VM'ler için Azure İzleyici etkinleştirme](../../azure-monitor/insights/media/vminsights-enable-single-vm/enable-vminsights-vm-portal-01.png)

4. **Azure Izleyici öngörüleri ekleme** sayfasında, aynı abonelikte mevcut bir Log Analytics çalışma alanınız varsa, açılır listeden seçin.  

    Liste, varsayılan çalışma alanını ve VM 'nin abonelikte dağıtıldığı konumu önceden seçer. 

    >[!NOTE]
    >İzleme verilerini VM 'den depolamak üzere yeni bir Log Analytics çalışma alanı oluşturmak için, bkz. [Log Analytics çalışma alanı oluşturma](../../azure-monitor/learn/quick-create-workspace.md). Log Analytics çalışma alanınız [desteklenen bölgelerden](../../azure-monitor/insights/vminsights-enable-overview.md#log-analytics)birine ait olmalıdır.

İzlemeyi etkinleştirdikten sonra, VM 'nin performans ölçümlerini görüntüleyebilmeniz için birkaç dakika beklemeniz gerekebilir.

![Dağıtım işlemeyi VM'ler için Azure İzleyici izlemeyi etkinleştir](../../azure-monitor/insights/media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)

## <a name="view-vm-performance-metrics"></a>VM performans ölçümlerini görüntüleme

VM'ler için Azure İzleyici, bir sanal makinenin ne kadar iyi performans gösterdiğini belirlemenize yardımcı olmak üzere birkaç ana performans göstergelerini (KPI) hedefleyen bir performans grafikleri kümesi içerir. Sanal makinenize erişmek için aşağıdaki adımları uygulayın.

1. Azure portal, **kaynak grupları**' na tıklayın, **Myresourcegroupmonitor**' ı seçin ve ardından kaynak listesinden **myvm** ' yi seçin.

2. VM sayfasında, **izleme** bölümünde Öngörüler ' i **(Önizleme)** seçin.

3. **Performans** sekmesini seçin.

Bu sayfa yalnızca performans kullanım grafiklerini içermez, aynı zamanda bulunan her bir mantıksal disk için bir tablo, kapasitesi, kullanımı ve her bir ölçünün toplam ortalaması dahil değildir.

## <a name="create-alerts"></a>Uyarı oluşturma

Belirli performans ölçümlerine göre uyarılar oluşturabilirsiniz. Bu uyarılar, ortalama CPU kullanımı belirli bir eşiği aştığında veya kullanılabilir boş disk alanı belirli bir miktarın altına düştüğünde size bildirimde bulunabilir. Uyarılar Azure portal görüntülenir veya e-posta yoluyla gönderilebilir. Ayrıca, oluşturulan uyarılara yanıt olarak Azure Otomasyonu runbook 'larını veya Azure Logic Apps tetikleyebilirsiniz.

Aşağıdaki örnek ortalama CPU kullanımı için bir uyarı oluşturur.

1. Azure portal, **kaynak grupları**' na tıklayın, **Myresourcegroupmonitor**' ı seçin ve ardından kaynak listesinden **myvm** ' yi seçin.

2. VM dikey penceresinde **Uyarı kuralları** ' na tıklayın ve ardından uyarılar dikey penceresinin üst kısmında **ölçüm uyarısı Ekle** ' ye tıklayın.

3. Uyarınız için *Myalertrule* gibi bir **ad** sağlayın

4. CPU yüzdesi beş dakika boyunca 1,0 aştığında bir uyarı tetiklemek için, diğer tüm varsayılanları seçili bırakın.

5. İsteğe bağlı olarak, e-posta bildirimi göndermek için *e-posta sahipleri, katkıda bulunanlar ve okuyucular* kutusunu işaretleyin. Varsayılan eylem, portalda bir bildirim sunmanız.

6. **Tamam** düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide VM 'nizin performansını yapılandırmış ve görüntülediyseniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Kaynak grubu ve VM oluşturma
> * VM 'de önyükleme tanılamayı etkinleştirme
> * Önyükleme tanılamayı görüntüle
> * Konak ölçümlerini görüntüleme
> * VM'ler için Azure İzleyici etkinleştir
> * VM ölçümlerini görüntüleme
> * Uyarı oluşturma

Azure Güvenlik Merkezi hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [VM güvenliğini yönetme](../../security/fundamentals/overview.md)
