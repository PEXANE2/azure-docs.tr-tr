---
title: Öğretici-Azure 'da Windows sanal makinelerini Izleme | Microsoft Docs
description: Bu öğreticide, Windows sanal makinelerinizde çalışan performansı ve keşfedilen uygulama bileşenlerini izlemeyi öğreneceksiniz.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: mgoedtel
manager: carmonm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/27/2018
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: a2f4083841c801db3edf1b2838b8d3271b700731
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71680067"
---
# <a name="tutorial-monitor-a-windows-virtual-machine-in-azure"></a>Öğretici: Azure 'da bir Windows sanal makinesini Izleme

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

Cloud Shell açmak için, yalnızca bir kod bloğunun sağ üst köşesinden **dene** ' yi seçin. Cloud Shell, [https://shell.azure.com/powershell](https://shell.azure.com/powershell)' e giderek ayrı bir tarayıcı sekmesinde da başlatabilirsiniz. Kod bloklarını kopyalamak için **Kopyala** ' yı seçin, Cloud Shell yapıştırın ve ENTER tuşuna basarak çalıştırın.

## <a name="create-virtual-machine"></a>Sanal makine oluştur

Bu öğreticide Azure izleme ve güncelleştirme yönetimini yapılandırmak için Azure 'da bir Windows sanal makinesi gereklidir. İlk olarak, VM için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential)ile bir Yönetici Kullanıcı adı ve parola ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

Şimdi [New-azvm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm)ile VM 'yi oluşturun. Aşağıdaki örnek *EastUS* konumunda *myvm* adlı bir VM oluşturur. Henüz yoksa, *Myresourcegroupmonitormonitor* ve destekleyici ağ kaynaklarını kaynak grubu oluşturulur:

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroupMonitor" `
    -Name "myVM" `
    -Location "East US" `
    -Credential $cred
```

Kaynakların ve sanal makinenin oluşturulması birkaç dakika sürer.

## <a name="view-boot-diagnostics"></a>Önyükleme tanılamayı görüntüle

Windows sanal makineleri önyüklendikten sonra, önyükleme tanılama Aracısı, sorun giderme amacıyla kullanılabilecek ekran çıktısını yakalar. Bu özellik varsayılan olarak etkindir. Yakalanan ekran görüntüleri, varsayılan olarak da oluşturulan bir Azure depolama hesabında depolanır.

[Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/az.compute/get-azvmbootdiagnosticsdata) komutuyla önyükleme tanılama verilerini alabilirsiniz. Aşağıdaki örnekte, önyükleme tanılaması * c: \* sürücüsünün köküne indirilir.

```powershell
Get-AzVMBootDiagnosticsData -ResourceGroupName "myResourceGroupMonitor" -Name "myVM" -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>Konak ölçümlerini görüntüleme

Bir Windows VM 'nin, Azure 'da etkileşimde bulunduğu adanmış bir konak VM 'si vardır. Ölçümler, ana bilgisayar için otomatik olarak toplanır ve Azure portal görüntülenebilir.

1. Azure portal, **kaynak grupları**' na tıklayın, **Myresourcegroupmonitor**' ı seçin ve ardından kaynak listesinden **myvm** ' yi seçin.
2. VM dikey penceresinde **ölçümler** ' e tıklayın ve ardından konak VM 'nin nasıl çalıştığını görmek için **kullanılabilir ölçümler** altında konak ölçümlerinden birini seçin.

    ![Konak ölçümlerini görüntüleme](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

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
