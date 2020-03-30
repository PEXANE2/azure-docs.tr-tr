---
title: Hızlandırılmış Ağ ile Azure VM Oluşturma - Azure PowerShell
description: Hızlandırılmış Ağ ile nasıl bir Linux sanal makine oluşturabilirsiniz öğrenin.
services: virtual-network
documentationcenter: ''
author: gsilva5
manager: gedegrac
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/04/2018
ms.author: gsilva
ms.openlocfilehash: 16837782af2f08e27363091dc21587a100194cd8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79245063"
---
# <a name="create-a-windows-virtual-machine-with-accelerated-networking-using-azure-powershell"></a>Azure PowerShell'i kullanarak Hızlandırılmış Ağ ile bir Windows sanal makinesi oluşturun

Bu eğitimde, Hızlandırılmış Ağ ile bir Windows sanal makine (VM) oluşturmayı öğrenirsiniz. Hızlandırılmış Ağ ile bir Linux VM oluşturmak için [bkz.](create-vm-accelerated-networking-cli.md) Hızlandırılmış ağ, tek kök G/Ç sanallaştırmasını (SR-IOV) VM'ye sağlayarak ağ performansını büyük ölçüde artırır. Bu yüksek performanslı yol, desteklenen VM türlerinde en zorlu ağ iş yüklerinde kullanılmak üzere gecikme süresini, gerginliği ve CPU kullanımını azaltarak ana bilgisayarı veri yolundan atlar. Aşağıdaki resim, hızlandırılmış ağ ile ve olmadan iki VMs arasındaki iletişimi gösterir:

![Karşılaştırma](./media/create-vm-accelerated-networking/accelerated-networking.png)

Hızlandırılmış ağ olmadan, VM'ye girip çıkan tüm ağ trafiği ana bilgisayardan ve sanal anahtardan geçmelidir. Sanal anahtar, ağ güvenlik grupları, erişim denetim listeleri, yalıtım ve diğer ağ sanallaştırılmış hizmetler gibi tüm ilke zorlama sağlar ağ trafiğine. Sanal anahtarlar hakkında daha fazla bilgi edinmek için [Hyper-V ağ sanallaştırma ve sanal anahtara](https://technet.microsoft.com/library/jj945275.aspx)bakın.

Hızlandırılmış ağ ile ağ trafiği VM'nin ağ arabirimine (NIC) gelir ve daha sonra VM'ye iletilir. Sanal anahtarın uyguladığı tüm ağ ilkeleri artık boşaltılır ve donanımda uygulanır. Donanımda ilke uygulamak, NIC'nin ağ trafiğini doğrudan VM'ye iletmesini sağlar, ana bilgisayarda uyguladığı tüm ilkeyi korurken ana bilgisayarı ve sanal anahtarı atlar.

Hızlandırılmış ağ avantajları yalnızca etkinleştirilen VM için geçerlidir. En iyi sonuçlar için, bu özelliği aynı Azure Sanal Ağına (VNet) bağlı en az iki VM'de etkinleştirmek idealdir. VNets üzerinden iletişim kurarken veya şirket içinde bağlantı kurarken, bu özelliğin genel gecikme ye en az etkisi vardır.

## <a name="benefits"></a>Avantajlar
* **Düşük Gecikme / Saniye başına daha yüksek paketler (pps):** Sanal anahtarı veri yolundan kaldırmak, paketlerinin ilke işleme için ana bilgisayarda harcadığı zamanı kaldırır ve VM içinde işlenebilecek paket sayısını artırır.
* **Azaltılmış jitter:** Sanal anahtar işleme, uygulanması gereken ilke miktarına ve işlemeyi yapan CPU'nun iş yüküne bağlıdır. İlke zorlamanın donanıma indirilmesi, paketleri doğrudan VM'ye teslim ederek, ana bilgisayarı VM iletişimine ve tüm yazılım kesintilerine ve bağlam anahtarlarına kaldırarak bu değişkenliği ortadan kaldırır.
* **Azalmış CPU kullanımı:** Ana bilgisayardaki sanal anahtarı atlayarak, ağ trafiğini işlemek için daha az CPU kullanımına yol açar.

## <a name="limitations-and-constraints"></a>Sınırlamalar ve Kısıtlamalar

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Aşağıdaki dağıtımlar Azure Galerisi'nden kutunun dışında desteklenir:
* **Windows Server 2016 Datacenter** 
* **Windows Server 2012 R2 Datacenter**
* **Windows Server 2019 Datacenter**

### <a name="supported-vm-instances"></a>Desteklenen VM örnekleri
Hızlandırılmış Ağ, en genel amaç ve 2 veya daha fazla vCPUs ile bilgi işlem için optimize edilmiş örnek boyutlarında desteklenir.  Bu desteklenen seriler şunlardır: D/DSv2 ve F/Fs

Hiper iş parçacığı destekleyen durumlarda, Hızlandırılmış Ağ 4 veya daha fazla vCPUs ile VM örneklerinde desteklenir. Desteklenen seriler şunlardır: D/Dsv3, E/Esv3, Fsv2, Lsv2, Ms/Mms ve Ms/Mmsv2.

VM örnekleri hakkında daha fazla bilgi için [Windows VM boyutlarına](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.

### <a name="regions"></a>Bölgeler
Tüm genel Azure bölgelerinde ve Azure Kamu Bulutu'nda kullanılabilir.

### <a name="enabling-accelerated-networking-on-a-running-vm"></a>Çalışan bir VM'de Hızlandırılmış Ağ Etkinleştirme
Hızlandırılmış ağ etkin olmadan desteklenen bir VM boyutu, özelliği yalnızca durdurulduğunda ve devre dendiğinde etkinleştirilebilir.

### <a name="deployment-through-azure-resource-manager"></a>Azure Kaynak Yöneticisi aracılığıyla dağıtım
Hızlandırılmış Ağ ile sanal makineler (klasik) dağıtılamaz.

## <a name="create-a-windows-vm-with-azure-accelerated-networking"></a>Azure Hızlandırılmış Ağ ile Windows VM Oluşturma
## <a name="portal-creation"></a>Portal oluşturma
Bu makalede, Azure Powershell kullanarak hızlandırılmış ağ içeren bir sanal makine oluşturmak için adımlar sağlasa da, [Azure portalını kullanarak hızlandırılmış ağ içeren bir sanal makine de oluşturabilirsiniz.](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Portalda sanal bir makine oluştururken, sanal makine bıçağı **oluştur'da** **Ağ sekmesini** seçin.  Bu sekmede, **Hızlandırılmış ağ**için bir seçenek vardır.  Desteklenen bir [işletim sistemi](#supported-operating-systems) ve [VM boyutu](#supported-vm-instances)seçtiyseniz, bu seçenek otomatik olarak "A"ya doldurulur.  Değilse, Hızlandırılmış Ağ için "Kapalı" seçeneğini dolduracak ve kullanıcıya etkinleştirilmeme nedeni verir.   
* *Not:* Portal üzerinden yalnızca desteklenen işletim sistemleri etkinleştirilebilir.  Özel bir görüntü kullanıyorsanız ve görüntünüz Hızlandırılmış Ağ'ı destekliyorsa, lütfen CLI veya Powershell kullanarak VM'nizi oluşturun. 

Sanal makine oluşturulduktan sonra, Hızlandırılmış Ağ'ın etkinleştirildiğindeonay'daki yönergeleri izleyerek Hızlandırılmış Ağ oluşturmanın etkinleştirildiğinden onaylayabilirsiniz.

## <a name="powershell-creation"></a>Powershell oluşturma
## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Azure PowerShell](/powershell/azure/install-az-ps) sürüm 1.0.0 veya sonraki sürüm'u yükleyin. Şu anda yüklü sürümünüzü `Get-Module -ListAvailable Az`bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [PowerShell Galerisi'nden](https://www.powershellgallery.com/packages/Az)Az modülünün en son sürümünü yükleyin. PowerShell oturumunda, [Connect-AzAccount'ı](/powershell/module/az.accounts/connect-azaccount)kullanarak bir Azure hesabına giriş yapın.

Aşağıdaki örneklerde, örnek parametre adlarını kendi değerlerinizle değiştirin. Örnek parametre adları *myResourceGroup,* *myNic*ve *myVM'i*içeriyordu.

[New-AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup)ile bir kaynak grubu oluşturun. Aşağıdaki örnek, *merkezi* konumda *myResourceGroup* adında bir kaynak grubu oluşturur:

```powershell
New-AzResourceGroup -Name "myResourceGroup" -Location "centralus"
```

İlk olarak, [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.Network/New-azVirtualNetworkSubnetConfig)ile bir alt net yapılandırması oluşturun. Aşağıdaki örnek *mySubnet*adlı bir alt ağ oluşturur:

```powershell
$subnet = New-AzVirtualNetworkSubnetConfig `
    -Name "mySubnet" `
    -AddressPrefix "192.168.1.0/24"
```

*MySubnet* alt ağı ile [New-AzVirtualNetwork](/powershell/module/az.Network/New-azVirtualNetwork)ile sanal ağ oluşturun.

```powershell
$vnet = New-AzVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "centralus" `
    -Name "myVnet" `
    -AddressPrefix "192.168.0.0/16" `
    -Subnet $Subnet
```

## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

İlk olarak, [New-AzNetworkSecurityRuleConfig](/powershell/module/az.Network/New-azNetworkSecurityRuleConfig)ile bir ağ güvenlik grubu kuralı oluşturun.

```powershell
$rdp = New-AzNetworkSecurityRuleConfig `
    -Name 'Allow-RDP-All' `
    -Description 'Allow RDP' `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 100 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389
```

[New-AzNetworkSecurityGroup](/powershell/module/az.Network/New-azNetworkSecurityGroup) ile bir ağ güvenlik grubu oluşturun ve buna *İzin-RDP-Tüm güvenlik* kuralını atayın. *Tümlere İzin Ver* kuralına ek olarak, ağ güvenlik grubu birkaç varsayılan kural içerir. Varsayılan bir kural Internet'ten gelen tüm erişimi devre dışı kılabilir, bu nedenle oluşturulan makine oluşturulduktan sonra sanal makineye uzaktan bağlanabilmeniz için Ağ güvenlik grubuna *İzin Ver* kuralı atanır.

```powershell
$nsg = New-AzNetworkSecurityGroup `
    -ResourceGroupName myResourceGroup `
    -Location centralus `
    -Name "myNsg" `
    -SecurityRules $rdp
```

Ağ güvenlik grubunu *mySubnet* alt ağıyla [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.Network/Set-azVirtualNetworkSubnetConfig)ile ilişkilendirin. Ağ güvenlik grubundaki kural, alt ağda dağıtılan tüm kaynaklar için etkilidir.

```powershell
Set-AzVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name 'mySubnet' `
    -AddressPrefix "192.168.1.0/24" `
    -NetworkSecurityGroup $nsg
```

## <a name="create-a-network-interface-with-accelerated-networking"></a>Hızlandırılmış ağ ile bir ağ arabirimi oluşturma
[New-AzPublicIpAddress](/powershell/module/az.Network/New-azPublicIpAddress)ile ortak bir IP adresi oluşturun. Sanal makineye Internet'ten erişmeye düşünmüyorsanız, herkese açık bir IP adresi gerekmez, ancak bu makaledeki adımları tamamlamak için gereklidir.

```powershell
$publicIp = New-AzPublicIpAddress `
    -ResourceGroupName myResourceGroup `
    -Name 'myPublicIp' `
    -location centralus `
    -AllocationMethod Dynamic
```

Hızlandırılmış ağ etkinleştirilmiş [New-AzNetworkInterface](/powershell/module/az.Network/New-azNetworkInterface) ile bir ağ arabirimi oluşturun ve ortak IP adresini ağ arabirimine atayın. Aşağıdaki örnek, *myVnet* sanal ağının *mySubnet* alt ağında *myNic* adında bir ağ arabirimi oluşturur ve *myPublicIp* genel IP adresini ona atar:

```powershell
$nic = New-AzNetworkInterface `
    -ResourceGroupName "myResourceGroup" `
    -Name "myNic" `
    -Location "centralus" `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIp.Id `
    -EnableAcceleratedNetworking
```

## <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

`$cred` [Get-Credentials](/powershell/module/microsoft.powershell.security/get-credential)kullanarak VM kimlik bilgilerinizi değişkene ayarlama:

```powershell
$cred = Get-Credential
```

İlk olarak, [Yeni-AzVMConfig](/powershell/module/az.compute/new-azvmconfig)ile VM tanımlayın. Aşağıdaki örnekte, Hızlandırılmış Ağ *(Standard_DS4_v2)* destekleyen VM boyutuna sahip *myVM* adlı bir VM tanımlanır:

```powershell
$vmConfig = New-AzVMConfig -VMName "myVm" -VMSize "Standard_DS4_v2"
```

Tüm VM boyutlarının ve özelliklerinin bir listesi için [Windows VM boyutlarına](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.

[Set-AzVMOperatingSystem](/powershell/module/az.compute/set-azvmoperatingsystem) ve [Set-AzVMSourceImage](/powershell/module/az.compute/set-azvmsourceimage)ile VM yapılandırmanızın geri kalanını oluşturun. Aşağıdaki örnekte bir Windows Server 2016 VM oluşturulur:

```powershell
$vmConfig = Set-AzVMOperatingSystem -VM $vmConfig `
    -Windows `
    -ComputerName "myVM" `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$vmConfig = Set-AzVMSourceImage -VM $vmConfig `
    -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" `
    -Skus "2016-Datacenter" `
    -Version "latest"
```

[Add-AzVMNetworkInterface](/powershell/module/az.compute/add-azvmnetworkinterface)ile daha önce oluşturduğunuz ağ arabirimini ekleyin:

```powershell
$vmConfig = Add-AzVMNetworkInterface -VM $vmConfig -Id $nic.Id
```

Son olarak, [Yeni-AzVM](/powershell/module/az.compute/new-azvm)ile VM oluşturun:

```powershell
New-AzVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "centralus"
```

## <a name="confirm-the-driver-is-installed-in-the-operating-system"></a>Sürücünün işletim sistemine yüklü olduğunu doğrulayın

Azure'da VM'yi oluşturduktan sonra VM'ye bağlanın ve sürücünün Windows'a yüklenmiş olduğunu onaylayın.

1. Bir Internet tarayıcısından Azure [portalını](https://portal.azure.com) açın ve Azure hesabınızla oturum açın.
2. Azure portalının üst kısmındaki metin *Arama kaynaklarını* içeren kutuya *myVm*yazın. **myVm** arama sonuçlarında göründüğünde, tıklatın. **Oluşturma,** **Bağlan** düğmesinin altında görünüyorsa, Azure VM'yi oluşturmayı henüz bitirmemiştir. Genel bakışın sol üst köşesinde **bağlan'ı** tıklatın, ancak **Artık Bağlan** düğmesinin altında **Oluşturma'yı** görmeden sonra.
3. Girilen kullanıcı adı ve [şifreyi girin sanal makine oluştur.](#create-the-virtual-machine) Azure'da hiç Windows VM'ye bağlanmadıysanız, [bkz.](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-virtual-machine)
4. Windows Başlat düğmesine sağ tıklayın ve **Aygıt Yöneticisi'ni**tıklatın. Ağ **bağdaştırıcıları** düğümlerini genişletin. Aşağıdaki resimde gösterildiği gibi **Mellanox ConnectX-3 Sanal Fonksiyon Ethernet Adaptörü'nün** göründüğünü doğrulayın:

    ![Cihaz Yöneticisi](./media/create-vm-accelerated-networking/device-manager.png)

VM'iniz için hızlandırılmış Ağ artık etkinleştirildi.

## <a name="enable-accelerated-networking-on-existing-vms"></a>Mevcut VM'lerde Hızlandırılmış Ağ Etkinleştirme
Hızlandırılmış Ağ olmadan bir VM oluşturduysanız, bu özelliği varolan bir VM'de etkinleştirmek mümkündür.  VM, yukarıda da özetlenen aşağıdaki ön koşulları karşılayarak Hızlandırılmış Ağ'ı desteklemelidir:

* VM Hızlandırılmış Ağ için desteklenen bir boyut olmalıdır
* VM desteklenen bir Azure Galerisi resmi olmalıdır (ve Linux için çekirdek sürümü)
* Bir kullanılabilirlik kümesindeki veya VMSS'deki tüm VM'ler, herhangi bir NIC'te Hızlandırılmış Ağ'ı etkinleştirmeden önce durdurulmalı/devre denilmelidir

### <a name="individual-vms--vms-in-an-availability-set"></a>Bir kullanılabilirlik kümesinde tek tek VM'ler & VM'ler
VM'yi veya Kullanılabilirlik Kümesi'ni ilk durdurma/anlaşma, Kümedeki tüm VM'leri belirleyin:

```azurepowershell
Stop-AzVM -ResourceGroup "myResourceGroup" `
    -Name "myVM"
```

Önemli, lütfen unutmayın, VM'niz bir kullanılabilirlik kümesi olmadan tek tek oluşturulduysa, Hızlandırılmış Ağ'ı etkinleştirmek için tek tek VM'yi durdurmanız/anlaşma yapmanız gerekir.  VM'niz bir kullanılabilirlik kümesiyle oluşturulduysa, kullanılabilirlik kümesinde bulunan tüm VM'lerin NIC'lerden herhangi birinde Hızlandırılmış Ağ oluşturmayı etkinleştirmeden önce durdurulması/devre denmesi gerekir. 

Durdurulduktan sonra, VM'nizin NIC'inde Hızlandırılmış Ağ'ı etkinleştirin:

```azurepowershell
$nic = Get-AzNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic"

$nic.EnableAcceleratedNetworking = $true

$nic | Set-AzNetworkInterface
```

VM'nizi yeniden başlatın veya bir kullanılabilirlik kümesinde yken, kümedeki tüm VM'leri yeniden başlatın ve Hızlandırılmış Ağ'ın etkinleştirildiğinden onaylayın:

```azurepowershell
Start-AzVM -ResourceGroup "myResourceGroup" `
    -Name "myVM"
```

### <a name="vmss"></a>VMSS
VMSS biraz farklıdır, ancak aynı iş akışını izler.  İlk olarak, VM'leri durdurun:

```azurepowershell
Stop-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet"
```

VM'ler durdurulduktan sonra, ağ arabirimi altında Hızlandırılmış Ağ özelliğini güncelleştirin:

```azurepowershell
$vmss = Get-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet"

$vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].EnableAcceleratedNetworking = $true

Update-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Bir VMSS'de otomatik, haddeleme ve manuel olmak üzere üç farklı ayar kullanarak güncelleştirmeleri uygulayan VM yükseltmeleri olduğunu lütfen unutmayın.  Bu yönergelerde, VMSS'nin yeniden başladıktan hemen sonra değişiklikleri alması için ilke otomatik olarak ayarlanır.  Değişikliklerin hemen alınması için otomatik olarak ayarlamak için:

```azurepowershell
$vmss.UpgradePolicy.AutomaticOSUpgrade = $true

Update-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Son olarak, VMSS'yi yeniden başlatın:

```azurepowershell
Start-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet"
```

Yeniden başlattıktan sonra yükseltmelerin bitmesini bekleyin, ancak tamamlandıktan sonra VF VM'nin içinde görünür.  (Lütfen desteklenen bir işletim sistemi ve VM boyutu kullandığınızdan emin olun)

### <a name="resizing-existing-vms-with-accelerated-networking"></a>Hızlandırılmış Ağ ile varolan VM'leri yeniden boyutlandırma

Hızlandırılmış Ağ etkin leştirilmiş VM'ler yalnızca Hızlandırılmış Ağ'ı destekleyen VM'lere yeniden boyutlandırılabilir.  

Hızlandırılmış Ağ etkinleştirilmiş bir VM, yeniden boyutlandırma işlemini kullanarak Hızlandırılmış Ağ'ı desteklemeyen bir VM örneğine yeniden boyutlandırılamaz.  Bunun yerine, bu VM'lerden birini yeniden boyutlandırmak için:

* VM'yi durdur/Deallocate veya bir kullanılabilirlik kümesi/VMSS'de, set/VMSS'deki tüm VM'leri durdurun/anlaşmalı olarak bulun.
* Hızlandırılmış Ağ VM NIC devre dışı bırakılmalıdır veya bir kullanılabilirlik kümesi / VMSS, set / VMSS tüm VMsS.
* Hızlandırılmış Ağ devre dışı bırakıldıktan sonra, VM/availability set/VMSS Hızlandırılmış Ağ'ı desteklemeyen yeni bir boyuta taşınabilir ve yeniden başlatılabilir.
