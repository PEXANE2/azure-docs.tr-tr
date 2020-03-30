---
title: Azure CLI kullanarak Hızlandırılmış Ağ ile Bir Azure VM oluşturma
description: Hızlandırılmış Ağ etkinleştirilmiş bir Linux sanal makine oluşturmak için nasıl öğrenin.
services: virtual-network
documentationcenter: na
author: gsilva5
manager: gedegrac
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/10/2019
ms.author: gsilva
ms.custom: ''
ms.openlocfilehash: 05f8430efa31b39d49025fb8456108da229d3d71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80239820"
---
# <a name="create-a-linux-virtual-machine-with-accelerated-networking-using-azure-cli"></a>Azure CLI kullanarak Hızlandırılmış Ağ ile bir Linux sanal makinesi oluşturun

Bu eğitimde, Hızlandırılmış Ağ ile bir Linux sanal makine (VM) oluşturmak için nasıl öğrenirler. Hızlandırılmış Ağ ile bir Windows VM oluşturmak için [bkz.](create-vm-accelerated-networking-powershell.md) Hızlandırılmış ağ, tek kök G/Ç sanallaştırmasını (SR-IOV) VM'ye sağlayarak ağ performansını büyük ölçüde artırır. Bu yüksek performanslı yol, desteklenen VM türlerinde en zorlu ağ iş yüklerinde kullanılmak üzere gecikme süresini, gerginliği ve CPU kullanımını azaltarak ana bilgisayarı veri yolundan atlar. Aşağıdaki resim, hızlandırılmış ağ ile ve olmadan iki VMs arasındaki iletişimi gösterir:

![Karşılaştırma](./media/create-vm-accelerated-networking/accelerated-networking.png)

Hızlandırılmış ağ olmadan, VM'ye girip çıkan tüm ağ trafiği ana bilgisayardan ve sanal anahtardan geçmelidir. Sanal anahtar, ağ güvenlik grupları, erişim denetim listeleri, yalıtım ve diğer ağ sanallaştırılmış hizmetler gibi tüm ilke zorlama sağlar ağ trafiğine. Sanal anahtarlar hakkında daha fazla bilgi edinmek için [Hyper-V ağ sanallaştırma ve sanal anahtar](https://technet.microsoft.com/library/jj945275.aspx) makalesini okuyun.

Hızlandırılmış ağ ile ağ trafiği sanal makinenin ağ arabirimine (NIC) gelir ve daha sonra VM'ye iletilir. Sanal anahtarın uyguladığı tüm ağ ilkeleri artık boşaltılır ve donanımda uygulanır. Donanımda ilke uygulamak, NIC'nin ağ trafiğini doğrudan VM'ye iletmesini sağlar, ana bilgisayarda uyguladığı tüm ilkeyi korurken ana bilgisayarı ve sanal anahtarı atlar.

Hızlandırılmış ağ avantajları yalnızca etkinleştirilen VM için geçerlidir. En iyi sonuçlar için, bu özelliği aynı Azure sanal ağına (VNet) bağlı en az iki VM'de etkinleştirmek idealdir. VNets üzerinden iletişim kurarken veya şirket içinde bağlantı kurarken, bu özelliğin genel gecikme ye en az etkisi vardır.

## <a name="benefits"></a>Avantajlar
* **Düşük Gecikme / Saniye başına daha yüksek paketler (pps):** Sanal anahtarı veri yolundan kaldırmak, paketlerinin ilke işleme için ana bilgisayarda harcadığı zamanı kaldırır ve VM içinde işlenebilecek paket sayısını artırır.
* **Azaltılmış jitter:** Sanal anahtar işleme, uygulanması gereken ilke miktarına ve işlemeyi yapan CPU'nun iş yüküne bağlıdır. İlke zorlamanın donanıma indirilmesi, paketleri doğrudan VM'ye teslim ederek, ana bilgisayarı VM iletişimine ve tüm yazılım kesintilerine ve bağlam anahtarlarına kaldırarak bu değişkenliği ortadan kaldırır.
* **Azalmış CPU kullanımı:** Ana bilgisayardaki sanal anahtarı atlayarak, ağ trafiğini işlemek için daha az CPU kullanımına yol açar.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Aşağıdaki dağıtımlar Azure Galerisi'nden kutunun dışında desteklenir: 
* **Linux-azure çekirdekli Ubuntu 14.04**
* **Ubuntu 16.04 veya sonrası** 
* **SLES12 SP3 veya sonrası** 
* **RHEL 7.4 veya sonrası**
* **CentOS 7.4 veya sonrası**
* **CoreOS Linux**
* **Debian "Stretch" backports çekirdek ile**
* **Oracle Linux 7.4 ve daha sonra Red Hat Uyumlu Çekirdek (RHCK) ile**
* **Oracle Linux 7.5 ve daha sonra UEK sürümü 5 ile**
* **FreeBSD 10.4, 11.1 & 12.0**

## <a name="limitations-and-constraints"></a>Sınırlamalar ve Kısıtlamalar

### <a name="supported-vm-instances"></a>Desteklenen VM örnekleri
Hızlandırılmış Ağ, en genel amaç ve 2 veya daha fazla vCPUs ile bilgi işlem için optimize edilmiş örnek boyutlarında desteklenir.  Bu desteklenen seriler şunlardır: D/DSv2 ve F/Fs

Hiper iş parçacığı destekleyen durumlarda, Hızlandırılmış Ağ 4 veya daha fazla vCPUs ile VM örneklerinde desteklenir. Desteklenen seriler şunlardır: D/Dsv3, E/Esv3, Fsv2, Lsv2, Ms/Mms ve Ms/Mmsv2.

VM örnekleri hakkında daha fazla bilgi için [Linux VM boyutlarına](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.

### <a name="custom-images"></a>Özel Görüntüler
Özel bir görüntü kullanıyorsanız ve resminiz Hızlandırılmış Ağ'ı destekliyorsa, lütfen Gerekli sürücülerin Azure'da Mellanox ConnectX-3 ve ConnectX-4 Lx NIC'leriyle çalışması gerektiğinden emin olun.

### <a name="regions"></a>Bölgeler
Azure Genel Bulutlar'ın yanı sıra tüm genel Azure bölgelerinde kullanılabilir.

<!-- ### Network interface creation 
Accelerated networking can only be enabled for a new NIC. It cannot be enabled for an existing NIC.
removed per issue https://github.com/MicrosoftDocs/azure-docs/issues/9772 -->
### <a name="enabling-accelerated-networking-on-a-running-vm"></a>Çalışan bir VM'de Hızlandırılmış Ağ Etkinleştirme
Hızlandırılmış ağ etkin olmadan desteklenen bir VM boyutu, özelliği yalnızca durdurulduğunda ve devre dendiğinde etkinleştirilebilir.  
### <a name="deployment-through-azure-resource-manager"></a>Azure Kaynak Yöneticisi aracılığıyla dağıtım
Hızlandırılmış Ağ ile sanal makineler (klasik) dağıtılamaz.

## <a name="create-a-linux-vm-with-azure-accelerated-networking"></a>Azure Hızlandırılmış Ağ ile Bir Linux VM Oluşturma
## <a name="portal-creation"></a>Portal oluşturma
Bu makalede, Azure CLI'yi kullanarak hızlandırılmış ağ içeren bir sanal makine oluşturmak için adımlar sağlasa da, [Azure portalını kullanarak hızlandırılmış ağ içeren bir sanal makine de oluşturabilirsiniz.](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Portalda sanal bir makine oluştururken, sanal makine bıçağı **oluştur'da** **Ağ sekmesini** seçin.  Bu sekmede, **Hızlandırılmış ağ**için bir seçenek vardır.  Desteklenen bir [işletim sistemi](#supported-operating-systems) ve [VM boyutu](#supported-vm-instances)seçtiyseniz, bu seçenek otomatik olarak "A"ya doldurulur.  Değilse, Hızlandırılmış Ağ için "Kapalı" seçeneğini dolduracak ve kullanıcıya etkinleştirilmeme nedeni verir.   

* *Not:* Portal üzerinden yalnızca desteklenen işletim sistemleri etkinleştirilebilir.  Özel bir görüntü kullanıyorsanız ve resminiz Hızlandırılmış Ağ'ı destekliyorsa, lütfen CLI veya PowerShell kullanarak VM'nizi oluşturun. 

Sanal makine oluşturulduktan sonra, Hızlandırılmış Ağ'ın [etkinleştirildiğindeonay'daki](#confirm-that-accelerated-networking-is-enabled)yönergeleri izleyerek Hızlandırılmış Ağ oluşturmanın etkinleştirildiğinden onaylayabilirsiniz.

## <a name="cli-creation"></a>CLI oluşturma
### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

En son [Azure CLI'sini](/cli/azure/install-azure-cli) yükleyin ve az giriş 'i kullanarak bir Azure [hesabına](/cli/azure/reference-index)giriş yapın. Aşağıdaki örneklerde, örnek parametre adlarını kendi değerlerinizle değiştirin. Örnek parametre adları *myResourceGroup,* *myNic*ve *myVm'i*içeriyordu.

[az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Aşağıdaki örnek, *merkezi* konumda *myResourceGroup* adında bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location centralus
```

[Linux hızlandırılmış ağ](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview)da listelenen desteklenen bir Linux bölgesi seçin.

[az network vnet create](/cli/azure/network/vnet) komutu ile bir sanal ağ oluşturun. Aşağıdaki örnek, *myVnet* adında bir alt ağ içeren bir sanal ağ oluşturur:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma
[az ağ nsg oluşturmak](/cli/azure/network/nsg)ile bir ağ güvenlik grubu oluşturun. Aşağıdaki örnek *myNetworkSecurityGroup* adında bir ağ güvenlik grubu oluşturur:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

Ağ güvenlik grubu, biri Internet'ten gelen tüm erişimi devre dışı bırakan birkaç varsayılan kural içerir. [Az ağ nsg kuralı oluşturarak](/cli/azure/network/nsg/rule)sanal makineye SSH erişimi sağlamak için bir bağlantı noktası açın:

```azurecli
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup \
  --name Allow-SSH-Internet \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix Internet \
  --source-port-range "*" \
  --destination-address-prefix "*" \
  --destination-port-range 22
```

### <a name="create-a-network-interface-with-accelerated-networking"></a>Hızlandırılmış ağ ile bir ağ arabirimi oluşturma

[az network public-ip create](/cli/azure/network/public-ip) komutu ile bir genel IP adresi oluşturun. Sanal makineye Internet'ten erişmeye düşünmüyorsanız, herkese açık bir IP adresi gerekmez, ancak bu makaledeki adımları tamamlamak için gereklidir.

```azurecli
az network public-ip create \
    --name myPublicIp \
    --resource-group myResourceGroup
```

Az [network nic](/cli/azure/network/nic) ile hızlandırılmış ağ etkin bir ağ oluşturmak ile bir ağ arabirimi oluşturun. Aşağıdaki örnek, *myVnet* sanal ağının *mySubnet* alt ağında *myNic* adında bir ağ arabirimi oluşturur ve *myNetworkSecurityGroup* ağ güvenlik grubunu ağ arabirimiyle ilişkilendirer:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --accelerated-networking true \
    --public-ip-address myPublicIp \
    --network-security-group myNetworkSecurityGroup
```

### <a name="create-a-vm-and-attach-the-nic"></a>Bir VM oluşturun ve NIC'yi takın
VM'yi oluşturduğunuzda, oluşturduğunuz `--nics`NIC'yi belirtin. [Linux hızlandırılmış ağ](https://azure.microsoft.com/updates/accelerated-networking-in-expanded-preview)da listelenen bir boyut ve dağıtım seçin. 

[az vm create](/cli/azure/vm) ile bir VM oluşturun. Aşağıdaki örnek, UbuntuLTS görüntü ve Hızlandırılmış Ağ *(Standard_DS4_v2)* destekleyen bir boyut ile *myVM* adlı bir VM oluşturur:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS4_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

Tüm VM boyutları nın ve özelliklerinin bir listesi için [Linux VM boyutlarına](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.

VM oluşturulduktan sonra, aşağıdaki örnek çıktıya benzer çıktı döndürülür. **publicIpAddress** değerini not alın. Bu adres sonraki adımlarda VM'ye erişmek için kullanılır.

```output
{
  "fqdns": "",
  "id": "/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "centralus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "192.168.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

### <a name="confirm-that-accelerated-networking-is-enabled"></a>Hızlandırılmış ağ özelliğinin etkin olduğunu doğrulayın

Sanal makine ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. Oluşturduğunuz sanal makineye atanan genel IP adresiyle değiştirin `<your-public-ip-address>` ve VM'yi `--admin-username` oluşturduğunuzda farklı bir değer kullandıysanız *azureuser'ı* değiştirin.

```bash
ssh azureuser@<your-public-ip-address>
```

Bash kabuğundan çekirdek `uname -r` sürümünün aşağıdaki sürümlerden biri veya daha büyük olduğunu girin ve onaylayın:

* **Ubuntu 16.04**: 4.11.0-1013
* **SLES SP3**: 4.4.92-6.18
* **RHEL**: 7.4.2017120423
* **CentOS**: 7.4.20171206


Mellanox VF cihazının komutla VM'ye maruz kaldığını doğrulayın. `lspci` Döndürülen çıktı aşağıdaki çıktıya benzer:

```output
0000:00:00.0 Host bridge: Intel Corporation 440BX/ZX/DX - 82443BX/ZX/DX Host bridge (AGP disabled) (rev 03)
0000:00:07.0 ISA bridge: Intel Corporation 82371AB/EB/MB PIIX4 ISA (rev 01)
0000:00:07.1 IDE interface: Intel Corporation 82371AB/EB/MB PIIX4 IDE (rev 01)
0000:00:07.3 Bridge: Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 02)
0000:00:08.0 VGA compatible controller: Microsoft Corporation Hyper-V virtual VGA
0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
```

`ethtool -S eth0 | grep vf_` Komutu ile VF (sanal işlev) üzerinde etkinlik olup yok. Aşağıdaki örnek çıktıya benzer çıktı alırsanız, hızlandırılmış ağ etkinleştirilir ve çalışır.

```output
vf_rx_packets: 992956
vf_rx_bytes: 2749784180
vf_tx_packets: 2656684
vf_tx_bytes: 1099443970
vf_tx_dropped: 0
```
VM'iniz için hızlandırılmış Ağ artık etkinleştirildi.

## <a name="handle-dynamic-binding-and-revocation-of-virtual-function"></a>Dinamik bağlama ve sanal işlevin iptalini işleme 
Uygulamalar VM maruz sentetik NIC üzerinde çalıştırmak gerekir. Uygulama doğrudan VF NIC üzerinden çalışırsa, bazı paketler sentetik arabirim üzerinde görününcünüz, VM'ye gidecek **tüm** paketleri almaz.
Sentetik NIC üzerinden bir uygulama çalıştırıyorsanız, uygulamanın ona yönelik **tüm** paketleri almasını garanti eder. Ayrıca, ana bilgisayara hizmet verilirken VF iptal edilebilse bile uygulamanın çalışmaya devam ettiğinden de emin olur. Sentetik NIC'ye bağlanan **uygulamalar, Hızlandırılmış Ağ'dan**yararlanan tüm uygulamalar için **zorunlu** bir gerekliliktir.

## <a name="enable-accelerated-networking-on-existing-vms"></a>Mevcut VM'lerde Hızlandırılmış Ağ Etkinleştirme
Hızlandırılmış Ağ olmadan bir VM oluşturduysanız, bu özelliği varolan bir VM'de etkinleştirmek mümkündür.  VM, yukarıda da özetlenen aşağıdaki ön koşulları karşılayarak Hızlandırılmış Ağ'ı desteklemelidir:

* VM Hızlandırılmış Ağ için desteklenen bir boyut olmalıdır
* VM desteklenen bir Azure Galerisi resmi olmalıdır (ve Linux için çekirdek sürümü)
* Bir kullanılabilirlik kümesindeki veya VMSS'deki tüm VM'ler, herhangi bir NIC'te Hızlandırılmış Ağ'ı etkinleştirmeden önce durdurulmalı/devre denilmelidir

### <a name="individual-vms--vms-in-an-availability-set"></a>Bir kullanılabilirlik kümesinde tek tek VM'ler & VM'ler
VM'yi veya Kullanılabilirlik Kümesi'ni ilk durdurma/anlaşma, Kümedeki tüm VM'leri belirleyin:

```azurecli
az vm deallocate \
    --resource-group myResourceGroup \
    --name myVM
```

Önemli, lütfen unutmayın, VM'niz bir kullanılabilirlik kümesi olmadan tek tek oluşturulduysa, Hızlandırılmış Ağ'ı etkinleştirmek için tek tek VM'yi durdurmanız/anlaşma yapmanız gerekir.  VM'niz bir kullanılabilirlik kümesiyle oluşturulduysa, kullanılabilirlik kümesinde bulunan tüm VM'lerin NIC'lerden herhangi birinde Hızlandırılmış Ağ oluşturmayı etkinleştirmeden önce durdurulması/devre denmesi gerekir. 

Durdurulduktan sonra, VM'nizin NIC'inde Hızlandırılmış Ağ'ı etkinleştirin:

```azurecli
az network nic update \
    --name myNic \
    --resource-group myResourceGroup \
    --accelerated-networking true
```

VM'nizi yeniden başlatın veya Kullanılabilirlik Kümesi'ndeyken, Setteki tüm VM'ler etkinleştirin ve Hızlandırılmış Ağ'ın etkinleştirildiğinden onaylayın: 

```azurecli
az vm start --resource-group myResourceGroup \
    --name myVM
```

### <a name="vmss"></a>VMSS
VMSS biraz farklıdır, ancak aynı iş akışını izler.  İlk olarak, VM'leri durdurun:

```azurecli
az vmss deallocate \
    --name myvmss \
    --resource-group myrg
```

VM'ler durdurulduktan sonra, ağ arabirimi altında Hızlandırılmış Ağ özelliğini güncelleştirin:

```azurecli
az vmss update --name myvmss \
    --resource-group myrg \
    --set virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].enableAcceleratedNetworking=true
```

Bir VMSS'de otomatik, haddeleme ve manuel olmak üzere üç farklı ayar kullanarak güncelleştirmeleri uygulayan VM yükseltmeleri olduğunu lütfen unutmayın.  Bu yönergelerde, VMSS'nin yeniden başladıktan hemen sonra değişiklikleri alması için ilke otomatik olarak ayarlanır.  Değişikliklerin hemen alınması için otomatik olarak ayarlamak için: 

```azurecli
az vmss update \
    --name myvmss \
    --resource-group myrg \
    --set upgradePolicy.mode="automatic"
```

Son olarak, VMSS'yi yeniden başlatın:

```azurecli
az vmss start \
    --name myvmss \
    --resource-group myrg
```

Yeniden başlattıktan sonra yükseltmelerin bitmesini bekleyin, ancak tamamlandıktan sonra VF VM'nin içinde görünür.  (Lütfen desteklenen bir işletim sistemi ve VM boyutu kullandığınızdan emin olun.)

### <a name="resizing-existing-vms-with-accelerated-networking"></a>Hızlandırılmış Ağ ile varolan VM'leri yeniden boyutlandırma

Hızlandırılmış Ağ etkin leştirilmiş VM'ler yalnızca Hızlandırılmış Ağ'ı destekleyen VM'lere yeniden boyutlandırılabilir.  

Hızlandırılmış Ağ etkinleştirilmiş bir VM, yeniden boyutlandırma işlemini kullanarak Hızlandırılmış Ağ'ı desteklemeyen bir VM örneğine yeniden boyutlandırılamaz.  Bunun yerine, bu VM'lerden birini yeniden boyutlandırmak için: 

* VM'yi durdur/Deallocate veya bir kullanılabilirlik kümesi/VMSS'de, set/VMSS'deki tüm VM'leri durdurun/anlaşmalı olarak bulun.
* Hızlandırılmış Ağ VM NIC devre dışı bırakılmalıdır veya bir kullanılabilirlik kümesi / VMSS, set / VMSS tüm VMsS.
* Hızlandırılmış Ağ devre dışı bırakıldıktan sonra, VM/availability set/VMSS Hızlandırılmış Ağ'ı desteklemeyen yeni bir boyuta taşınabilir ve yeniden başlatılabilir.  

