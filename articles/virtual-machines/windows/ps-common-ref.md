---
title: Azure Sanal Makineler için ortak PowerShell komutları
description: Windows VM'lerinizi Azure'da oluşturmaya ve yönetmeye başlamanızı sağlamak için Ortak PowerShell komutları.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
tags: azure-resource-manager
ms.assetid: ba3839a2-f3d5-4e19-a5de-95bfb1c0e61e
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/01/2018
ms.author: cynthn
ms.openlocfilehash: 3d08693b6b07b5a2f1fb40d0c4758ab43729bd76
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81456371"
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Azure Sanal Makineler’i oluşturmak ve yönetmek için genel PowerShell komutları

Bu makale, Azure aboneliğinizde sanal makineler oluşturmak ve yönetmek için kullanabileceğiniz bazı Azure PowerShell komutlarını kapsar.  Belirli komut satırı anahtarları ve seçenekleri ile ilgili daha ayrıntılı yardım için **Yardım Al** *komutunu*kullanabilirsiniz.

 

Bu makalede birden fazla komut çalışıyorsanız bu değişkenler sizin için yararlı olabilir:

- $location - Sanal makinenin konumu. Sizin için çalışan bir coğrafi [bölge](https://azure.microsoft.com/regions/) bulmak için [Get-AzLocation'ı](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation) kullanabilirsiniz.
- $myResourceGroup - Sanal makineyi içeren kaynak grubunun adı.
- $myVM - Sanal makinenin adı.

## <a name="create-a-vm---simplified"></a>VM oluşturma - basitleştirilmiş

| Görev | Komut |
| ---- | ------- |
| Basit bir VM oluşturma | [Yeni-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) -İsim $myVM <BR></BR><BR></BR> New-AzVM'de tek bir ad gereken *basitleştirilmiş* parametreler vardır. -Adı değeri, yeni bir VM oluşturmak için gereken tüm kaynaklar için ad olarak kullanılır. Daha fazlasını belirtebilirsiniz, ancak gereken tek şey budur.|
| Özel görüntüden VM oluşturma | New-AzVm -ResourceGroupName $myResourceGroup -Name $myVM ImageName "myImage" -Konum $location  <BR></BR><BR></BR>Zaten kendi [yönetilen görüntü](capture-image-resource.md)oluşturmuş olmanız gerekir. Birden çok, aynı VM yapmak için bir görüntü kullanabilirsiniz. |



## <a name="create-a-vm-configuration"></a>VM yapılandırması oluşturma

| Görev | Komut |
| ---- | ------- |
| VM yapılandırması oluşturma |$vm = [Yeni-AzVMConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azvmconfig) -VMName $myVM -VMSize "Standard_D1_v1"<BR></BR><BR></BR>VM yapılandırması, VM ayarlarını tanımlamak veya güncelleştirmek için kullanılır. Yapılandırma VM adı ve [boyutu](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)ile baş harfe getirilir. |
| Yapılandırma ayarları ekleme |$vm = [Set-AzVMOperatingSystem](https://docs.microsoft.com/powershell/module/az.compute/set-azvmoperatingsystem) -VM $vm -Windows -ComputerName $myVM -Kimlik $cred -ProvisionVMAgent -EnableAutoUpdate<BR></BR><BR></BR>[Kimlik bilgilerini](https://technet.microsoft.com/library/hh849815.aspx) içeren işletim sistemi ayarları, Daha önce New-AzVMConfig kullanılarak oluşturduğunuz yapılandırma nesnesine eklenir. |
| Ağ arabirimi ekleme |$vm = [Add-AzVMNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/Add-AzVMNetworkInterface) -VM $vm -Id $nic. Kimliği<BR></BR><BR></BR>VM'nin sanal ağda iletişim kurmak için bir [ağ arabirimine](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) sahip olması gerekir. Varolan bir ağ arabirimi nesnesini almak için [Get-AzNetworkInterface'i](https://docs.microsoft.com/powershell/module/az.compute/add-azvmnetworkinterface) de kullanabilirsiniz. |
| Platform görüntüsü belirtin |$vm = [Set-AzVMSourceImage](https://docs.microsoft.com/powershell/module/az.compute/set-azvmsourceimage) -VM $vm -PublisherName "publisher_name" -Teklif "publisher_offer" -Skus "product_sku" -Sürüm "son"<BR></BR><BR></BR>[Görüntü bilgileri,](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Daha önce New-AzVMConfig kullanılarak oluşturduğunuz yapılandırma nesnesine eklenir. Bu komuttan döndürülen nesne yalnızca işletim sistemi diskini platform görüntüsü kullanacak şekilde ayarladığınızda kullanılır. |
| VM oluşturma |[New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) -ResourceGroupName $myResourceGroup -Konum $location -VM $vm<BR></BR><BR></BR>Tüm kaynaklar bir [kaynak grubunda](../../azure-resource-manager/management/manage-resource-groups-powershell.md)oluşturulur. Bu komutu çalıştırmadan önce New-AzVMConfig, Set-AzVMOperatingSystem, Set-AzVMSourceImage, Add-AzVMNetworkInterface ve Set-AzVMOSDisk'i çalıştırın. |
| VM'yi güncelleştir |[Güncelleme-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) -ResourceGroupName $myResourceGroup -VM $vm<BR></BR><BR></BR>Get-AzVM'yi kullanarak geçerli VM yapılandırmasını alın, VM nesnesindeki yapılandırma ayarlarını değiştirin ve bu komutu çalıştırın. |

## <a name="get-information-about-vms"></a>VM'ler hakkında bilgi alın

| Görev | Komut |
| ---- | ------- |
| Abonelikteki VM'leri listele |[Al-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) |
| Kaynak grubundaki VM'leri listele |Get-AzVM -ResourceGroupName $myResourceGroup<BR></BR><BR></BR>Aboneliğinizdeki kaynak gruplarının listesini almak için [Get-AzResourceGroup'u](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup)kullanın. |
| VM hakkında bilgi alma |Get-AzVM -ResourceGroupName $myResourceGroup -Ad $myVM |

## <a name="manage-vms"></a>VM’leri yönetme
| Görev | Komut |
| --- | --- |
| VM başlatma |[Start-AzVM](https://docs.microsoft.com/powershell/module/az.compute/start-azvm) -ResourceGroupName $myResourceGroup -Ad $myVM |
| VM durdurma |[Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm) -ResourceGroupName $myResourceGroup -İsim $myVM |
| Çalışan vm'yi yeniden başlatma |[Yeniden Başlat-AzVM](https://docs.microsoft.com/powershell/module/az.compute/restart-azvm) -ResourceGroupName $myResourceGroup -İsim $myVM |
| VM silme |[Remove-AzVM](https://docs.microsoft.com/powershell/module/az.compute/remove-azvm) -ResourceGroupName $myResourceGroup -Ad $myVM |


## <a name="next-steps"></a>Sonraki adımlar
* [Kaynak Yöneticisi ve PowerShell'i kullanarak Windows VM oluştur'da](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)sanal makine oluşturmanın temel adımlarını görün.

