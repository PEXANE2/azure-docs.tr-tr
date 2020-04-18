---
title: Öğretici - Azure PowerShell ile özel VM görüntüleri oluşturun
description: Bu öğreticide, Azure PowerShell kullanarak Azure’da özel sanal makine görüntüsü oluşturmayı öğrenirsiniz
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/30/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 87347cfea0e45d3498c48f07578523a20d5a13e2
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641092"
---
# <a name="tutorial-create-a-custom-image-of-an-azure-vm-with-azure-powershell"></a>Öğretici: Azure PowerShell ile bir Azure VM'nin özel görüntüsünü oluşturma

Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, dağıtımları önyükleme ve birden çok VM arasında tutarlılık sağlamak için kullanılabilir. Bu eğitimde, PowerShell'i kullanarak bir Azure sanal makinenin kendi özel görüntüsünü oluşturursunuz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Sysprep ve VM’leri genelleştirme
> * Özel görüntü oluşturma
> * Özel görüntüden VM oluşturma
> * Aboneliğinizdeki tüm görüntüleri listeleme
> * Görüntü silme

Genel önizlemede Azure [VM Image Builder](https://docs.microsoft.com/azure/virtual-machines/windows/image-builder-overview) hizmetine sahibiz. Özelleştirmelerinizi bir şablonda açıklayın ve bu makalede görüntü oluşturma adımlarını işleyecek. [Azure Image Builder 'ı deneyin (önizleme)](https://docs.microsoft.com/azure/virtual-machines/windows/image-builder).

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki adımlar, mevcut bir VM’yi alıp, yeni VM örnekleri oluşturmak için kullanabileceğiniz yeniden kullanılabilir bir özel görüntüye dönüştürmeyi ayrıntılı olarak açıklar.

Bu öğreticideki örneği tamamlamak için, mevcut bir sanal makinenizin olması gerekir. Gerekirse, bu [komut dosyası örneği](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) sizin için bir tane oluşturabilir. Bu öğreticide çalışırken, gerektiğinde kaynak grubu ve VM adlarını değiştirin.

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. Ayrıca bulut shell'i ayrı bir tarayıcı [https://shell.azure.com/powershell](https://shell.azure.com/powershell)sekmesinde başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

## <a name="prepare-vm"></a>VM'yi hazırlama

Sanal bir makinenin görüntüsünü oluşturmak için, kaynak VM'yi genelleştirerek, ayırarak ve ardından Azure ile genelleştirilmiş olarak işaretleyerek hazırlamanız gerekir.

### <a name="generalize-the-windows-vm-using-sysprep"></a>Sysprep kullanarak Windows VM'sini genelleştirme

Sysprep diğer öğelerin yanı sıra tüm kişisel hesap bilgilerinizi kaldırır ve makineyi bir görüntü olarak kullanılacak şekilde hazırlar. Sysprep hakkındaki ayrıntılar için bkz. [Sysprep İşlemini Kullanma: Giriş](https://technet.microsoft.com/library/bb457073.aspx).


1. Sanal makineye bağlanın.
2. Yönetici olarak Komut İstemi penceresini açın. Dizin *değiştirin %windir%\system32\sysprep*, `sysprep.exe`ve sonra çalıştırın .
3. **Sistem Hazırlama Aracı** iletişim kutusunda ** Sistem İlk Çalıştırma Deneyimi (OOBE) Moduna Gir**'i seçin ve **Genelleştir** onay kutusunun seçili olduğundan emin olun.
4. **Kapatma Seçenekleri**'nde **Kapat**'ı seçin ve **Tamam**'a tıklayın.
5. Sysprep tamamlandığında, sanal makineyi kapatır. **VM'yi yeniden başlatmayın**.

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>VM’yi serbest bırakma ve genelleştirilmiş olarak işaretleme

Görüntü oluşturmak için, VM'nin serbest bırakılması ve Azure'da genelleştirilmiş olarak işaretlenmesi gerekir.

[Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm)kullanarak VM'yi bulun.

```azurepowershell-interactive
Stop-AzVM `
   -ResourceGroupName myResourceGroup `
   -Name myVM -Force
```

Sanal makinenin durumunu `-Generalized` [Set-AzVm'i](https://docs.microsoft.com/powershell/module/az.compute/set-azvm)kullanarak ayarlayın. 
   
```azurepowershell-interactive
Set-AzVM `
   -ResourceGroupName myResourceGroup `
   -Name myVM -Generalized
```


## <a name="create-the-image"></a>Görüntü oluşturma

Şimdi [Yeni-AzImageConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azimageconfig) ve [New-AzImage](https://docs.microsoft.com/powershell/module/az.compute/new-azimage)kullanarak VM bir görüntü oluşturabilirsiniz. Aşağıdaki örnek, *myVM* adlı bir VM’den *myImage* adlı bir görüntü oluşturur.

Sanal makineyi alın. 

```azurepowershell-interactive
$vm = Get-AzVM `
   -Name myVM `
   -ResourceGroupName myResourceGroup
```

Görüntü yapılandırması oluşturun.

```azurepowershell-interactive
$image = New-AzImageConfig `
   -Location EastUS `
   -SourceVirtualMachineId $vm.ID 
```

Görüntü oluşturun.

```azurepowershell-interactive
New-AzImage `
   -Image $image `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
``` 

 
## <a name="create-vms-from-the-image"></a>Görüntüden VM oluşturma

Artık bir görüntünüz olduğuna göre, görüntüden bir veya daha fazla yeni VM oluşturabilirsiniz. Özel görüntüden VM oluşturma işlemi, Market görüntüsü kullanarak VM oluşturmaya benzer. Market görüntüsünü kullandığınızda, görüntü, görüntü sağlayıcısı, teklif, SKU ve sürüm hakkındaki bilgileri sağlamanız gerekir. [Yeni-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) cmdlet için basitleştirilmiş parametre kümesini kullanarak, aynı kaynak grubunda olduğu sürece özel görüntünün adını sağlamanız gerekir. Farklı bir kaynak grubunda VM oluşturmayı planlıyorsanız, -ImageName parametresi için görüntünün Kaynak Kimliğini sağlayın.

Bu örnek, *myImage* görüntümden *myVMfromImage* adlı bir VM oluşturur, *myResourceGroup.*


```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```

Eşzamanlı dağıtım sayısını tek bir görüntüden 20 VM ile sınırlamanızı öneririz. Aynı özel görüntüden 20'den fazla VM'nin büyük ölçekli, eşzamanlı dağıtımlarını planlıyorsanız, birden çok resim yinelemesi içeren [paylaşılan resim galerisi](shared-image-galleries.md) kullanmanız gerekir. 


## <a name="image-management"></a>Görüntü yönetimi 

Burada, yaygın görüntü yönetimi görevlerini ve PowerShell kullanarak bunların nasıl tamamlanacağını gösteren bazı örnekler verilmiştir.

Tüm görüntüleri ada göre listeleyin.

```azurepowershell-interactive
$images = Get-AzResource -ResourceType Microsoft.Compute/images 
$images.name
```

Görüntüyü silin. Bu örnek *myImage* adlı görüntüyü *myResourceGroup'tan*siler.

```azurepowershell-interactive
Remove-AzImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, özel bir VM görüntüsü oluşturdunuz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Sysprep ve VM’leri genelleştirme
> * Özel görüntü oluşturma
> * Özel görüntüden VM oluşturma
> * Aboneliğinizdeki tüm görüntüleri listeleme
> * Görüntü silme

Yüksek kullanılabilir sanal makinelerin nasıl oluşturulması hakkında bilgi edinmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Yüksek oranda kullanılabilir VM’ler oluşturma](tutorial-availability-sets.md)



