---
title: Öğretici - Azure'da Windows VM'leri için yüksek kullanılabilirlik
description: Bu öğreticide, Kullanılabilirlik Kümelerinde yüksek oranda kullanılabilir sanal makineler dağıtmak için Azure PowerShell kullanmayı öğreneceksiniz
documentationcenter: ''
services: virtual-machines-windows
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: tutorial
ms.date: 11/30/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 59bf06d2b279bad792bdc42a7c3b6acc2bc304b8
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985720"
---
# <a name="tutorial-create-and-deploy-highly-available-virtual-machines-with-azure-powershell"></a>Öğretici: Azure PowerShell ile yüksek oranda kullanılabilir sanal makineler oluşturma ve dağıtma

Bu eğitimde, Kullanılabilirlik Kümelerini kullanarak Sanal Makinelerinizin (VM'ler) kullanılabilirliğini ve güvenilirliğini nasıl artırabileceğinizi öğreneceksiniz. Kullanılabilirlik Kümeleri, Azure'da dağıttığınız VM'lerin bir kümede birden çok, yalıtılmış donanım düğümüne dağıtıldıklarından emin olun. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Kullanılabilirlik kümesi oluşturma
> * Kullanılabilirlik kümesinde sanal makine oluşturma
> * Kullanılabilir sanal makine boyutlarını denetleme
> * Azure Danışmanı’nı denetleme


## <a name="availability-set-overview"></a>Kullanılabilirlik kümesine genel bakış

Kullanılabilirlik Kümesi, VM kaynaklarını dağıtıldığında birbirinden yalıtma için mantıksal bir gruplandırma yeteneğidir. Azure, bir Kullanılabilirlik Kümesi'ne yerleştirdiğiniz VM'lerin birden çok fiziksel sunucu, bilgisayar rafları, depolama birimleri ve ağ anahtarları arasında çalışmasını sağlar. Bir donanım veya yazılım hatası olursa, yalnızca VM'lerinizin bir alt kümesi etkilenir ve genel çözümünüz çalışır durumda kalır. Kullanılabilirlik Setleri, güvenilir bulut çözümleri oluşturmak için gereklidir.

Dört adet ön uç web sunucusuna ve iki adet arka uç sanal makineye sahip olabileceğiniz tipik bir sanal makine tabanlı çözümü düşünelim. Azure ile, VM'lerinizi dağıtmadan önce iki kullanılabilirlik kümesi tanımlamak istersiniz: biri web katmanı için, diğeri de arka katman için. Yeni bir VM oluşturduğunuzda, kullanılabilirlik kümesini parametre olarak belirtirsiniz. Azure, VM'lerin birden çok fiziksel donanım kaynağı arasında yalıtılmış olmasını sağlar. Sunucularınızdan birinin çalıştığı fiziksel donanımda bir sorun varsa, sunucularınızın diğer örneklerinin farklı donanımlarda oldukları için çalışmaya devam edeceğini bilirsiniz.

Azure’da güvenilir sanal makine tabanlı çözümleri dağıtmak istediğinizde Kullanılabilirlik Kümelerini kullanın.

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. Ayrıca bulut shell'i ayrı bir tarayıcı [https://shell.azure.com/powershell](https://shell.azure.com/powershell)sekmesinde başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

## <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma

Konumdaki donanım, birden çok etki alanına ve hata etki alanına bölünmüştür. **Güncelleştirme etki alanı**, aynı anda yeniden başlatılabilen bir VM grubu ve temel alınan fiziksel donanımdır. Aynı **hata etki alanında** bulunan VM’ler, ortak güç kaynağı ve ağ anahtarıyla birlikte ortak depolama alanını paylaşır.  

[Yeni-AzAvailabilitySet'i](https://docs.microsoft.com/powershell/module/az.compute/new-azavailabilityset)kullanarak bir kullanılabilirlik kümesi oluşturabilirsiniz. Bu örnekte, hem güncelleştirme hem de hata etki alanlarının sayısı *2'dir* ve kullanılabilirlik kümesi *myAvailabilitySet*olarak adlandırılır.

Bir kaynak grubu oluşturun.

```azurepowershell-interactive
New-AzResourceGroup `
   -Name myResourceGroupAvailability `
   -Location EastUS
```

Parametreli [New-AzAvailabilitySet'i](https://docs.microsoft.com/powershell/module/az.compute/new-azavailabilityset) kullanarak `-sku aligned` yönetilen kullanılabilirlik kümesi oluşturun.

```azurepowershell-interactive
New-AzAvailabilitySet `
   -Location "EastUS" `
   -Name "myAvailabilitySet" `
   -ResourceGroupName "myResourceGroupAvailability" `
   -Sku aligned `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>Kullanılabilirlik kümesi içinde sanal makineler oluşturma
VM'ler, donanımarasında doğru şekilde dağıtıldıklarına emin olmak için kullanılabilirlik kümesi içinde oluşturulmalıdır. Oluşturulduktan sonra kullanılabilirlik kümesine varolan bir VM ekemezsiniz. 


[New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm)içeren bir VM oluşturduğunuzda, kullanılabilirlik kümesinin adını belirtmek için `-AvailabilitySetName` parametreyi kullanırsınız.

İlk olarak, VM için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

Şimdi kullanılabilirlik kümesinde [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) ile iki VM oluşturun.

```azurepowershell-interactive
for ($i=1; $i -le 2; $i++)
{
    New-AzVm `
        -ResourceGroupName "myResourceGroupAvailability" `
        -Name "myVM$i" `
        -Location "East US" `
        -VirtualNetworkName "myVnet" `
        -SubnetName "mySubnet" `
        -SecurityGroupName "myNetworkSecurityGroup" `
        -PublicIpAddressName "myPublicIpAddress$i" `
        -AvailabilitySetName "myAvailabilitySet" `
        -Credential $cred
}
```

İki VM’yi de oluşturup yapılandırmak birkaç dakika sürer. Tamamlandığında, temel alınan donanım arasında dağıtılmış iki sanal makineye sahip olursunuz. 

**Kaynak Grupları** > **myResourceGroupAvailability** > **myAvailabilitySet'e**giderek portalda ayarlanan kullanılabilirlik durumuna bakarsanız, VM'lerin iki hata arasında nasıl dağıtıldığını görmeniz ve etki alanlarını güncelleştirin.

![Portaldaki kullanılabilirlik kümesi](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Kullanılabilir sanal makine boyutlarını denetleme 

Kullanılabilirlik kümesi içinde bir VM oluşturduğunuzda, donanımda hangi VM boyutlarının kullanılabillerini bilmeniz gerekir. Kullanılabilirlik kümesinde dağıtabileceğiniz sanal makineler için kullanılabilir tüm boyutları elde etmek için [Get-AzVMSize](https://docs.microsoft.com/powershell/module/az.compute/get-azvmsize) komutunu kullanın.

```azurepowershell-interactive
Get-AzVMSize `
   -ResourceGroupName "myResourceGroupAvailability" `
   -AvailabilitySetName "myAvailabilitySet"
```

## <a name="check-azure-advisor"></a>Azure Danışmanı’nı denetleme 

Azure Danışmanı'nı, VM'lerinizin kullanılabilirliğini nasıl iyileştireceğiniz hakkında daha fazla bilgi almak için de kullanabilirsiniz. Azure Danışmanı yapılandırmanızı ve kullanım telemetrinizi analiz eder ve azure kaynaklarınızın maliyet etkinliğini, performansını, kullanılabilirliğini ve güvenliğini artırmanıza yardımcı olacak çözümler önerir.

[Azure portal](https://portal.azure.com)’ında oturum açın, **Tüm hizmetler**’i seçin ve **Danışman** yazın. Danışman panosu, seçili abonelik için kişiselleştirilmiş öneriler gösterir. Daha fazla bilgi için bkz. [Azure Danışmanı’nı kullanmaya başlayın](../../advisor/advisor-get-started.md).


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Kullanılabilirlik kümesi oluşturma
> * Kullanılabilirlik kümesinde sanal makine oluşturma
> * Kullanılabilir sanal makine boyutlarını denetleme
> * Azure Danışmanı’nı denetleme

Sanal makine ölçek kümeleri hakkında daha fazla bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [VM ölçek kümesi oluşturma](tutorial-create-vmss.md)


