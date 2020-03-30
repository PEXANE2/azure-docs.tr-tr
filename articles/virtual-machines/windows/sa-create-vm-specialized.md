---
title: Azure'da özel bir diskten VM oluşturma
description: Kaynak Yöneticisi dağıtım modeline özel yönetilmeyen bir disk ekleyerek yeni bir VM oluşturun.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: d887ef2ef74bb433d6e8ae7f53cd0b77f5948303
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74073343"
---
# <a name="create-a-vm-from-a-specialized-vhd-in-a-storage-account"></a>Depolama hesabında ki özel bir VHD'den VM oluşturma

Powershell kullanarak işletim sistemi diski olarak özel bir yönetilmeyen disk ekleyerek yeni bir VM oluşturun. Özelleştirilmiş disk, orijinal VM'nizdeki kullanıcı hesaplarını, uygulamaları ve diğer durum verilerini koruyan varolan bir VM'den VHD'nin kopyasıdır. 

İki seçeneğiniz vardır:
* [VHD’yi karşıya yükleme](sa-create-vm-specialized.md#option-1-upload-a-specialized-vhd)
* [Varolan bir Azure VM'nin VHD'sini kopyalama](sa-create-vm-specialized.md#option-2-copy-the-vhd-from-an-existing-azure-vm)

 


## <a name="option-1-upload-a-specialized-vhd"></a>Seçenek 1: Özel bir VHD yükleyin

VHD'yi Hyper-V gibi şirket içi sanallaştırma aracıyla oluşturulan özel bir VM'den veya başka bir buluttan dışa aktarılan bir VM'den yükleyebilirsiniz.

### <a name="prepare-the-vm"></a>VM’yi hazırlama
Şirket içi Bir VM veya başka bir buluttan dışa aktarılan bir VHD kullanılarak oluşturulan özel bir VHD yükleyebilirsiniz. Özel leştirilmiş bir VHD, kullanıcı hesaplarını, uygulamaları ve diğer durum verilerini orijinal VM'inizden korur. Yeni bir VM oluşturmak için Olduğu gibi VHD'yi kullanmayı planlıyorsanız, aşağıdaki adımların tamamlandığından emin olun. 
  
  * [Azure'a yüklemek için bir Windows VHD hazırlayın.](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Sysprep kullanarak VM genelleme **yapmayın.**
  * VM'de yüklü olan konuk sanallaştırma araçlarını ve aracılarını kaldırın (örneğin VMware araçları).
  * VM'nin IP adresini ve DNS ayarlarını DHCP üzerinden çekecek şekilde yapılandırıldığından emin olun. Bu, sunucunun başlatıldığında VNet içinde bir IP adresi almasını sağlar. 


### <a name="get-the-storage-account"></a>Depolama hesabını alma
Yüklenen VM görüntüsünü depolamak için Azure'da bir depolama hesabına ihtiyacınız vardır. Varolan bir depolama hesabı kullanabilir veya yeni bir depo oluşturabilirsiniz. 

Kullanılabilir depolama hesaplarını göstermek için şunları yazın:

```powershell
Get-AzStorageAccount
```

Varolan bir depolama hesabı kullanmak istiyorsanız, VM resim yükleme bölümüne gidin.

Bir depolama hesabı oluşturmanız gerekiyorsa, aşağıdaki adımları izleyin:

1. Depolama hesabının oluşturulması gereken kaynak grubunun adına ihtiyacınız var. Aboneliğinizde bulunan tüm kaynak gruplarını bulmak için şunları yazın:
   
    ```powershell
    Get-AzResourceGroup
    ```

    **Batı ABD** bölgesinde **myResourceGroup** adında bir kaynak grubu oluşturmak için şunları yazın:

    ```powershell
    New-AzResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. [Yeni Depolama Hesabı](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount) cmdlet'ini kullanarak bu kaynak grubunda **mystorageaccount** adında bir depolama hesabı oluşturun:
   
    ```powershell
    New-AzStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
### <a name="upload-the-vhd-to-your-storage-account"></a>VHD’yi depolama hesabınıza yükleme
Görüntüyü depolama hesabınızdaki bir kapsayıcıya yüklemek için [Add-AzVhd](https://docs.microsoft.com/powershell/module/az.compute/add-azvhd) cmdlet'i kullanın. Bu örnek, **myVHD.vhd** dosyasını `"C:\Users\Public\Documents\Virtual hard disks\"` **myResourceGroup** kaynak grubunda **mystorageaccount** adlı bir depolama hesabına yükler. Dosya **mycontainer** adlı konteyner içine yerleştirilir ve yeni dosya adı **myUploadedVHD.vhd**olacaktır.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Başarılı olursa, buna benzer bir yanıt alırsınız:

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Ağ bağlantınıza ve VHD dosyanızın boyutuna bağlı olarak, bu komutun tamamlanması biraz zaman alabilir.


## <a name="option-2-copy-the-vhd-from-an-existing-azure-vm"></a>Seçenek 2: VHD'yi varolan bir Azure VM'sinden kopyalama

Yeni, yinelenen bir VM oluştururken kullanmak üzere başka bir depolama hesabına Bir VHD kopyalayabilirsiniz.

### <a name="before-you-begin"></a>Başlamadan önce
Şunları yaptığınızdan emin olun:

* Kaynak ve **hedef depolama hesapları**hakkında bilgi sahibi. Kaynak VM için depolama hesabı ve kapsayıcı adlarına sahip olmanız gerekir. Genellikle, konteyner adı **vhds**olacaktır. Ayrıca bir hedef depolama hesabınız olması gerekir. Zaten bir tane yoksa, portalı **(Tüm Hizmetler** > Depolama hesapları > Ekle) veya [Yeni Depolama Hesabı](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount) cmdlet'ini kullanarak bir tane oluşturabilirsiniz. 
* [AzCopy aracını](../../storage/common/storage-use-azcopy.md)indirip yükledim. 

### <a name="deallocate-the-vm"></a>Deallocate the VM
VHD'nin kopyalanmasını sağlayan VM'yi bulun. 

* **Portal**: **Sanal makineler** > **myVM** > Stop'a tıklayın
* **Powershell**: **MyResourceGroup**kaynak grubunda **myVM** adlı VM'yi durdurmak (anlaşma yapmak) için [Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm) kullanın.

```powershell
Stop-AzVM -ResourceGroupName myResourceGroup -Name myVM
```

Azure portalındaki **VM'nin Durumu** **Durduruldu'dan** **Durduruldu'ya (ayrılan)** değişir.

### <a name="get-the-storage-account-urls"></a>Depolama hesabı URL'lerini alın
Kaynak ve hedef depolama hesaplarının URL'lerine ihtiyacınız vardır. URL'ler şöyle görünür: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Depolama hesabı ve kapsayıcı adını zaten biliyorsanız, URL'nizi oluşturmak için parantezler arasındaki bilgileri değiştirebilirsiniz. 

URL'yi almak için Azure portalını veya Azure Powershell'i kullanabilirsiniz:

* **Portal**: **>** **Tüm hizmetler** > **depolama hesapları** > *depolama hesabı* > **Blobs** için tıklayın ve kaynak VHD dosyanız muhtemelen **vhds** konteyner bulunmaktadır. Kapsayıcı için **Özellikler'i** tıklatın ve **URL**etiketli metni kopyalayın. Hem kaynak hem de hedef kapsayıcılarının URL'lerine ihtiyacınız vardır. 
* **Powershell**: **MyResourceGroup**kaynak grubunda **myVM** adlı VM için bilgi almak için [Get-AzVM'yi](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) kullanın. Sonuçlarda, **Vhd Uri**için **Depolama profili** bölümüne bakın. Uri'nin ilk bölümü konteynerin URL'sidir ve son bölümü VM'nin OS VHD adıdır.

```powershell
Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
``` 

## <a name="get-the-storage-access-keys"></a>Depolama erişim anahtarlarını alın
Kaynak ve hedef depolama hesaplarının erişim anahtarlarını bulun. Erişim anahtarları hakkında daha fazla bilgi için [Azure depolama hesapları hakkında](../../storage/common/storage-create-storage-account.md)bkz.

* **Portal**: **Tüm hizmetler** > **Depolama hesapları** > *depolama hesabı* > **Erişim anahtarlarını**tıklatın. Anahtar olarak etiketlenmiş anahtarı **kopyalayın1.**
* **Powershell**: Kaynak grubu **myResourceGroup'taki**depolama hesabı **mystorage hesabının** depolama anahtarını almak için [Get-AzStorageAccountKey'i](https://docs.microsoft.com/powershell/module/az.storage/get-azstorageaccountkey) kullanın. Etiketli **anahtarı kopyalayın1.**

```powershell
Get-AzStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup
```

### <a name="copy-the-vhd"></a>VHD'yi kopyalayın
AzCopy'yi kullanarak dosyaları depolama hesapları arasında kopyalayabilirsiniz. Hedef kapsayıcı için, belirtilen kapsayıcı yoksa, sizin için oluşturulur. 

AzCopy'yi kullanmak için yerel makinenizde bir komut istemi açın ve AzCopy'nin yüklü olduğu klasöre gidin. *C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy'e*benzer. 

Tüm dosyaları bir kapsayıcının içinde kopyalamak için **/S** anahtarını kullanırsınız. Bu, OS VHD'yi ve tüm veri disklerini aynı kapsayıcıda ysa kopyalamak için kullanılabilir. Bu örnek, depolama hesabı **mysourcestorage hesabındaki** konteyner **mysourcecontainer'taki** tüm dosyaların **mydestinationstorage account'daki mydestinationcontainer'a** nasıl kopyalanılabildiğini gösterir. **mydestinationcontainer** Depolama hesaplarının ve kapsayıcıların adlarını kendi hesaplarNızla değiştirin. `<sourceStorageAccountKey1>` Değiştirin `<destinationStorageAccountKey1>` ve kendi anahtarlarınızla.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
    /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
    /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Yalnızca birden çok dosyaiçeren bir kapsayıcıda belirli bir VHD kopyalamak istiyorsanız, /Desen anahtarını kullanarak dosya adını da belirtebilirsiniz. Bu örnekte, yalnızca **myFileName.vhd** adlı dosya kopyalanır.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
  /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
  /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
  /Pattern:myFileName.vhd
```


Bittiğinde, şuna benzer bir ileti alırsınız:

```
Finished 2 of total 2 file(s).
[2016/10/07 17:37:41] Transfer summary:
-----------------
Total files transferred: 2
Transfer successfully:   2
Transfer skipped:        0
Transfer failed:         0
Elapsed time:            00.00:13:07
```

### <a name="troubleshooting"></a>Sorun giderme
* AZCopy'i kullandığınızda, "Sunucu isteği doğrulamadı" hatasını görürseniz, Yetkilendirme üstbilgisinin değerinin imza da dahil olmak üzere doğru şekilde oluşturulduğundan emin olun. Key 2 veya ikincil depolama anahtarını kullanıyorsanız, birincil veya birinci depolama anahtarını kullanmayı deneyin.

## <a name="create-the-new-vm"></a>Yeni VM'yi oluşturun 

Yeni VM tarafından kullanılmak üzere ağ ve diğer VM kaynakları oluşturmanız gerekir.

### <a name="create-the-subnet-and-vnet"></a>alt Net ve vNet oluşturma

[Sanal ağın](../../virtual-network/virtual-networks-overview.md)vNet ve alt Net'ini oluşturun.

1. Alt Net'i oluşturun. Bu örnek, kaynak grubu **myResourceGroup'ta** **mySubNet**adında bir alt ağ oluşturur ve alt net adresi önekini **10.0.0.0/24**olarak ayarlar.
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. vNet'i oluşturun. Bu örnek, sanal ağ adını **myVnetName,** **Batı ABD'ye**konum ve sanal ağ için adres öneki **10.0.0.0/16**olarak ayarlar. 
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
   ### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Ağ güvenlik grubu ve RDP kuralı oluşturma
   RDP kullanarak VM'nizde oturum açabilmek için, 3389 bağlantı noktasında RDP erişimine izin veren bir güvenlik kuralına sahip olmanız gerekir. Yeni VM için VHD varolan özel bir VM oluşturulduğu için, VM oluşturulduktan sonra RDP kullanarak oturum açma izni olan kaynak sanal makineden varolan bir hesabı kullanabilirsiniz.
   Bu, ilişkili olacağı ağ arabirimini oluşturmadan önce tamamlanması gerekir.  
   Bu örnek, **MyNsg'ye** NSG adını ve rdp kural adını **myRdpRule**olarak ayarlar.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

Uç noktalar ve NSG kuralları hakkında daha fazla bilgi için [PowerShell'i kullanarak Azure'da bir VM bağlantı noktasını açma'ya](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.

### <a name="create-a-public-ip-address-and-nic"></a>Ortak bir IP adresi oluşturma ve NIC
Sanal makinenin sanal ağda iletişimini etkinleştirmeniz için, [genel IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve ağ arabirimi gereklidir.

1. Genel IP oluşturun. Bu örnekte, genel IP adresi adı **myIP**olarak ayarlanır.
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. NIC'yi oluşturun. Bu örnekte, NIC adı **myNicName**olarak ayarlanır. Bu adım, daha önce oluşturulan Ağ Güvenlik Grubu'nu da bu NIC ile ilişkilendirer.
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
    ```

### <a name="set-the-vm-name-and-size"></a>VM adını ve boyutunu ayarlama

Bu örnekte VM adı "myVM" ve VM boyutu "Standard_A2" olarak ayarlar.
```powershell
$vmName = "myVM"
$vmConfig = New-AzVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a>NIC ekle
    
```powershell
$vm = Add-AzVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    
    
### <a name="configure-the-os-disk"></a>İşletim sistemi diskini yapılandırma

1. Yüklediğiniz veya kopyaladığınız VHD için URI'yi ayarlayın. Bu örnekte, **myOsDisk.vhd** adlı VHD dosyası **myContainer**adlı bir kapsayıcıda **myStorageAccount** adlı bir depolama hesabında tutulur.

    ```powershell
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    ```
2. İşletim sistemi diskini ekleyin. Bu örnekte, işletim sistemi diski oluşturulduğunda, işletim sistemi disk adını oluşturmak için VM adına "osDisk" terimi eklenir. Bu örnekte ayrıca, bu Windows tabanlı VHD'nin Işletim Sistemi diski olarak VM'ye eklenmesi gerektiği de belirtilmektedir.
    
    ```powershell
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
    ```

İsteğe bağlı: VM'ye iliştirilmesi gereken veri diskinesahipseniz, veri VHD'lerinin URL'lerini ve uygun Mantıksal Birim Numarasını (Lun) kullanarak veri disklerini ekleyin.

```powershell
$dataDiskName = $vmName + "dataDisk"
$vm = Add-AzVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 1 -CreateOption attach
```

Bir depolama hesabı kullanırken, veri ve işletim sistemi disk URL'leri şuna benzer: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. Bunu portalda hedef depolama kapsayıcısına göz atarak, kopyalanan işletim sistemini veya vhd verilerini tıklayarak ve url'nin içeriğini kopyalayarak bulabilirsiniz.


### <a name="complete-the-vm"></a>VM'yi tamamlayın 

Az önce oluşturduğumuz yapılandırmaları kullanarak VM'yi oluşturun.

```powershell
#Create the new VM
New-AzVM -ResourceGroupName $rgName -Location $location -VM $vm
```

Bu komut başarılı olduysa, şu şekilde çıktı görürsünüz:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a>VM'nin oluşturulduğunu doğrulama
Yeni oluşturulan VM'yi Azure [portalında](https://portal.azure.com), **Tüm hizmetler** > **Sanal makineler**altında veya aşağıdaki PowerShell komutlarını kullanarak görmeniz gerekir:

```powershell
$vmList = Get-AzVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Yeni sanal makinenizde oturum açın. Daha fazla bilgi için [windows çalıştıran bir Azure sanal makinesine nasıl bağlanıp oturum açarken](connect-logon.md)göreceğiz.

