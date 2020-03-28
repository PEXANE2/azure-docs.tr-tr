---
title: Öğretici - Azure'da Linux sanal makinelerini izleyin
description: Bu eğitimde, performansı izlemeyi ve Linux sanal makinelerinizde çalışan uygulama bileşenlerini nasıl keşfedersiniz.
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
ms.openlocfilehash: b06342d5034b820be4e6fd49436546a5aa7b7e02
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75749786"
---
# <a name="tutorial-monitor-a-linux-virtual-machine-in-azure"></a>Öğretici: Azure'da bir Linux sanal makinesini izleyin

Azure izleme, Azure Sanal Taşıtlarından önyükleme ve performans verileri toplamak, bu verileri Azure depolamaalanında depolamak ve portal, Azure PowerShell modülü ve Azure CLI üzerinden erişilebilir hale getirmek için aracıları kullanır. Gelişmiş izleme, performans ölçümleri toplayarak, VM'ye yüklenen uygulama bileşenlerini keşfederek ve performans grafikleri ve bağımlılık haritasını içeren VM'ler için Azure Monitor ile yapılır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * VM’de önyükleme tanılamalarını etkinleştirme
> * Önyükleme tanılamasını görüntüleme
> * VM konak ölçümlerini görüntüleme
> * VM'ler için Azure Monitörünü etkinleştirme
> * VM performans ölçümlerini görüntüleme
> * Uyarı oluşturma

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. Ayrıca bulut shell'i ayrı bir tarayıcı [https://shell.azure.com/powershell](https://shell.azure.com/powershell)sekmesinde başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.30 veya sonraki bir sürümünü çalıştırmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="create-vm"></a>VM oluşturma

Tanılama ve ölçüm özelliklerinin nasıl çalıştığını görmek için bir VM gerekir. Öncelikle [az group create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroupMonitor* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroupMonitor --location eastus
```

Şimdi [az vm create](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) ile bir VM oluşturun. Aşağıdaki örnekte *myVM* adlı bir VM oluşturulur ve *~/.ssh/* içinde henüz yoksa SSH anahtarları oluşturulur:

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

Önyükleme tanılamalarını etkinleştirmeden önce, önyükleme günlüklerini depolamak için bir depolama hesabı oluşturulmalıdır. Depolama hesapları, genel olarak benzersiz bir ada sahip olmalıdır. Ad, 3 ile 24 karakter arasında olmalıdır ve yalnızca sayı ve küçük harf içermelidir. [az storage account create](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create) komutuyla bir depolama hesabı oluşturun. Bu örnekte benzersiz bir depolama hesabı oluşturmak için rastgele bir dize kullanılmaktadır.

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

## <a name="view-boot-diagnostics"></a>Önyükleme tanılamasını görüntüleme

Önyükleme tanılaması etkinleştirildiğinde, VM’yi durdurduğunuz ve başlattığınızda her seferinde önyükleme işlemiyle ilgili bilgiler bir günlük dosyasına yazılır. Bu örnekte öncelikle [az vm deallocate](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-deallocate) komutuyla VM’yi şu şekilde serbest bırakın:

```azurecli-interactive
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Şimdi [az vm start](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-start) komutuyla VM’yi şu şekilde başlatın:

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

## <a name="enable-advanced-monitoring"></a>Gelişmiş izlemeyi etkinleştirme

VM'ler için Azure VM'nizin Azure Monitor ile izlenmesini etkinleştirmek için:

1. Azure portalında **Kaynak Grupları**’na tıklayın, önce **myResourceGroupMonitor** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.

2. VM sayfasında, **İzleme** **bölümünde, Insights (önizleme) seçeneğini belirleyin.**

3. **Öngörüler (önizleme)** sayfasında **şimdi Deneyin'i**seçin.

    ![VM için VM'ler için Azure Monitörünü etkinleştirme](../../azure-monitor/insights/media/vminsights-enable-single-vm/enable-vminsights-vm-portal.png)

4. Azure **Monitor Öngörüleri Onboarding** sayfasında, aynı abonelikte mevcut bir Log Analytics çalışma alanınız varsa, açılan listede seçin.  

    Liste, varsayılan çalışma alanını ve VM'nin abonelikte dağıtıldığı konumu önceden seçer. 

    >[!NOTE]
    >VM'deki izleme verilerini depolamak için yeni bir Log [Analytics çalışma](../../azure-monitor/learn/quick-create-workspace.md)alanı oluşturmak için bkz. Log Analytics çalışma alanınız desteklenen [bölgelerden](../../azure-monitor/insights/vminsights-enable-overview.md#log-analytics)birine ait olmalıdır.

İzlemeyi etkinleştirdikten sonra, VM'nin performans ölçümlerini görüntüleyebilmeniz için birkaç dakika beklemeniz gerekebilir.

![VM izleme dağıtım işlemleri için Azure Monitörünü etkinleştirme](../../azure-monitor/insights/media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)

## <a name="view-vm-performance-metrics"></a>VM performans ölçümlerini görüntüleme

Sanal Ayarlar için Azure Monitörü, sanal bir makinenin ne kadar iyi performans gösterdiğini belirlemenize yardımcı olmak için birkaç önemli performans göstergesini (KPI) hedefleyen bir performans grafiği kümesi içerir. VM'nizden erişmek için aşağıdaki adımları gerçekleştirin.

1. Azure portalında **Kaynak Grupları**’na tıklayın, önce **myResourceGroupMonitor** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.

2. VM sayfasında, **İzleme** **bölümünde, Insights (önizleme) seçeneğini belirleyin.**

3. **Performans** sekmesini seçin.

Bu sayfa yalnızca performans kullanım grafiklerini değil, aynı zamanda keşfedilen her mantıksal disk için, kapasitesini, kullanımını ve her ölçüye göre toplam ortalamayı gösteren bir tabloyu da içerir.

## <a name="create-alerts"></a>Uyarı oluşturma

Belirli performans ölçümlerine bağlı uyarılar oluşturabilirsiniz. Uyarılar, ortalama CPU kullanımı belirli bir eşiği aştığında veya mevcut boş disk alanı belirli bir miktarın altına düştüğünde bildirim almak için kullanılabilir. Uyarılar Azure portalında görüntülenebilir veya e-posta ile gönderilebilir. Ayrıca oluşturulan uyarılara yanıt olarak Azure Otomasyonu runbook’larını veya Azure Logic Apps’i tetikleyebilirsiniz.

Aşağıdaki örnek, ortalama CPU kullanımı için bir uyarı oluşturur.

1. Azure portalında **Kaynak Grupları**’na tıklayın, önce **myResourceGroupMonitor** seçeneğini belirleyin ve ardından kaynak listesinden **myVM**’yi seçin.

2. Önce VM dikey penceresinde **Uyarı kuralları**’na ve ardından uyarılar dikey penceresinin üstündeki **Ölçüm uyarısı ekle** seçeneğine tıklayın.

3. Uyarınız için *myAlertRule* gibi bir **Ad** girin

4. CPU yüzdesi beş dakika boyunca 1,0’ı aştığında bir uyarı tetiklemek için diğer varsayılan ayarların tümünü seçili bırakın.

5. İsteğe bağlı olarak e-posta bildirimi göndermek için *E-posta sahipleri, katkıda bulunanlar ve okuyucular* kutusunu işaretleyebilirsiniz. Varsayılan eylem olarak portalda bir bildirim sunulur.

6. **Tamam** düğmesini tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, VM'nizin performansını yapılandırmış ve görüntülediniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Kaynak grubu ve sanal makine oluşturma
> * VM’de önyükleme tanılamalarını etkinleştirme
> * Önyükleme tanılamasını görüntüleme
> * Konak ölçümlerini görüntüleme
> * VM'ler için Azure Monitörünü etkinleştirme
> * VM ölçümlerini görüntüleme
> * Uyarı oluşturma

Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [VM güvenliğini yönetme](../../security/fundamentals/overview.md)
