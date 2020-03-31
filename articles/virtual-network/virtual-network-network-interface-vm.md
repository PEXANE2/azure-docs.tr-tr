---
title: Azure VM'lerine ağ arabirimleri ekleme veya Azure VM'lerden kaldırma
description: Sanal makinelerden ağ arabirimlerine ağ arabirimleri eklemeyi veya kaldırmayı öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/13/2020
ms.author: kumud
ms.openlocfilehash: 4169bfb5da5b1ad13bab0eb01397f7c1fb20b11b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060316"
---
# <a name="add-network-interfaces-to-or-remove-network-interfaces-from-virtual-machines"></a>Sanal makinelerde ağ arabirimi ekleme veya kaldırma işlemlerini gerçekleştirme

Bir Azure sanal makinesi (VM) oluştururken varolan bir ağ arabirimi eklemeyi öğrenin. Ayrıca, durdurulan (ayrılan) durumda varolan bir VM'den ağ arabirimleri eklemeyi veya kaldırmayı da öğrenin. Ağ arabirimi, Azure VM'nin Internet, Azure ve şirket içi kaynaklarla iletişim kurmasını sağlar. VM'de bir veya daha fazla ağ arabirimi vardır. 

Ağ arabirimi için IP adresleri eklemeniz, değiştirmeniz veya kaldırmanız gerekiyorsa, [bkz.](virtual-network-network-interface-addresses.md) Ağ arabirimlerini oluşturmak, değiştirmek veya silmek için [bkz.](virtual-network-network-interface.md)

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Hesabınız yoksa, etkin bir abonelikle bir Azure hesabı ayarlayın. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Bu makalenin geri kalanını başlatmadan önce bu görevlerden birini tamamlayın:

- **Portal kullanıcıları**: Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

- **PowerShell kullanıcıları**: Azure Bulut [Shell'deki](https://shell.azure.com/powershell)komutları çalıştırın veya bilgisayarınızdan PowerShell çalıştırın. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Azure Bulut BulutU tarayıcı sekmesinde, çevre açılır **listesini seçin** ve daha önce seçilmemişse **PowerShell'i** seçin.

    PowerShell'i yerel olarak çalıştırıyorsanız, Azure PowerShell modül sürüm 1.0.0 veya sonrası kullanın. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az.Network` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). Azure ile bağlantı oluşturmak için `Connect-AzAccount` komutunu çalıştırın.

- **Azure Komut satırı arabirimi (CLI) kullanıcıları**: [Azure Bulut BulutU'ndaki](https://shell.azure.com/bash)komutları çalıştırın veya CLI'yi bilgisayarınızdan çalıştırın. Azure CLI sürümünü yerel olarak çalıştırıyorsanız Azure CLI sürüm 2.0.26 veya daha sonra kullanın. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). Azure ile bağlantı oluşturmak için `az login` komutunu çalıştırın.

## <a name="add-existing-network-interfaces-to-a-new-vm"></a>Varolan ağ arabirimlerini yeni bir VM'ye ekleme

Portal üzerinden sanal bir makine oluşturduğunuzda, portal varsayılan ayarları içeren bir ağ arabirimi oluşturur ve ağ arabirimini sizin için VM'ye bağlar. Portalı, yeni bir VM'ye varolan ağ arabirimleri eklemek veya birden çok ağ arabirimine sahip bir VM oluşturmak için kullanamazsınız. CLI veya PowerShell'i kullanarak her ikisini de yapabilirsiniz. [Kısıtlamalar](#constraints)hakkında bilgi edindiğinden emin olun. Birden çok ağ arabirimine sahip bir VM oluşturursanız, VM'yi oluşturduktan sonra işletim sistemini bunları düzgün kullanacak şekilde yapılandırmanız gerekir. Birden çok ağ arabirimi için [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) veya [Windows'u](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) nasıl yapılandırıştırın öğrenin.

### <a name="commands"></a>Komutlar

VM'yi oluşturmadan önce [bir ağ arabirimi oluşturun.](virtual-network-network-interface.md#create-a-network-interface)

|Araç|Komut|
|---|---|
|CLI|[az network nic create](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#az-network-nic-create)|
|PowerShell|[Yeni-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="add-a-network-interface-to-an-existing-vm"></a>Varolan bir VM'ye ağ arabirimi ekleme

Sanal makinenize ağ arabirimi eklemek için:

1. Varolan bir sanal makineyi bulmak için [Azure portalına](https://portal.azure.com) gidin. **Sanal makineleri**arayın ve seçin.

2. VM'nizin adını seçin. VM, eklemek istediğiniz ağ arabirimi sayısını desteklemelidir. Her VM boyutunun kaç ağ arabirimini desteklediğini öğrenmek için [Linux VM'leri](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows VM'leri](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)için Azure'daki boyutlara bakın.

3. VM komut **çubuğunda, onay**iletişim kutusunda Durdur ve ardından **Tamam'ı** seçin. Ardından **VM'nin Durumu** Durduruldu **(ayrılan)** olana kadar bekleyin.

4. VM menü çubuğundan **Ağ** > **Ekle ağ arabirimini**seçin. Daha sonra **Ekle varolan ağ arabiriminde,** eklemek istediğiniz ağ arabirimini seçin ve **Tamam'ı**seçin.

    >[!NOTE]
    >Seçtiğiniz ağ arabirimi hızlandırılmış ağ özelliği etkin olamaz, ona atanmış bir IPv6 adresi olamaz ve vm'ye bağlı ağ arabirimi yle aynı sanal ağda bulunmalıdır.

    Varolan bir ağ arabirimi niz yoksa, önce bir ağ arabirimi oluşturmanız gerekir. Bunu yapmak için **ağ arabirimi oluştur'u**seçin. Ağ arabirimi oluşturma hakkında daha fazla bilgi edinmek için [bkz.](virtual-network-network-interface.md#create-a-network-interface) Sanal makinelere ağ arabirimleri eklerken ek kısıtlamalar hakkında daha fazla bilgi edinmek için [Kısıtlamalar'a](#constraints)bakın.

5. VM menü çubuğundan, sanal makineyi yeniden başlatmak için **Genel Bakış** > **Başlat'ı** seçin.

Artık VM işletim sistemini birden çok ağ arabirimini düzgün kullanacak şekilde yapılandırabilirsiniz. Birden çok ağ arabirimi için [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) veya [Windows'u](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) nasıl yapılandırıştırın öğrenin.

### <a name="commands"></a>Komutlar

|Araç|Komut|
|---|---|
|CLI|[az vm nic eklemek](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#az-vm-nic-add) (referans); [ayrıntılı adımlar](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Add-AzVMNetworkInterface](/powershell/module/az.compute/add-azvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (başvuru); [ayrıntılı adımlar](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="view-network-interfaces-for-a-vm"></a>Bir VM için ağ arabirimlerini görüntüleme

Her ağ arabiriminin yapılandırması hakkında bilgi edinmek için şu anda bir VM'e bağlı olan ağ arabirimlerini ve her ağ arabirimine atanan IP adreslerini görüntüleyebilirsiniz. 

1. Varolan bir sanal makineyi bulmak için [Azure portalına](https://portal.azure.com) gidin. **Sanal makineleri**arayın ve seçin.

    >[!NOTE]
    >Aboneliğiniz için Sahibi, Katılımcısı veya Ağ Katılımcısı rolüne atanmış bir hesabı kullanarak oturum açın. Rollerin hesaplara nasıl atayabildiğini hakkında daha fazla bilgi edinmek için Azure [rol tabanlı erişim denetimi için Yerleşik rollere](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)bakın.

2. Ekli ağ arabirimlerini görüntülemek istediğiniz VM'nin adını seçin.

3. VM menü çubuğunda **Ağ'ı**seçin.

Ağ arabirimi ayarları ve bunları nasıl değiştireceğinizi öğrenmek için [bkz.](virtual-network-network-interface.md) Ağ arabirimine atanan IP adreslerinin nasıl ekleneceği, değiştirilen veya kaldırılacakları hakkında bilgi edinmek için [bkz.](virtual-network-network-interface-addresses.md)

### <a name="commands"></a>Komutlar

|Araç|Komut|
|---|---|
|CLI|[az vm nic listesi](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#az-vm-nic-list)|
|PowerShell|[Al-AzVM](/powershell/module/az.compute/get-azvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="remove-a-network-interface-from-a-vm"></a>VM'den ağ arabirimini kaldırma

1. Varolan bir sanal makineyi bulmak için [Azure portalına](https://portal.azure.com) gidin. **Sanal makineleri**arayın ve seçin.

2. Ekli ağ arabirimlerini görüntülemek istediğiniz VM'nin adını seçin.

3. VM araç çubuğunda **Stop'u**seçin.

4. **VM'nin Durumu** **Durduruldu (ayrılan)** olana kadar bekleyin.

5. VM menü çubuğundan **Ağ** > **Bağlantısı ağ arabirimini**seçin.

6. Ağ **arabirimi** iletişim kutusunda, ayırmak istediğiniz ağ arabirimini seçin. Sonra **Tamam**’ı seçin.

    >[!NOTE]
    >Yalnızca bir ağ arabirimi listelenirse, sanal bir makinenin her zaman en az bir ağ arabirimi olması gerektiğinden, onu ayıramazsınız.

### <a name="commands"></a>Komutlar

|Araç|Komut|
|---|---|
|CLI|[az vm nic kaldırmak](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#az-vm-nic-remove) (referans); [ayrıntılı adımlar](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Remove-AzVMNetworkInterface](/powershell/module/az.compute/remove-azvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (başvuru); [ayrıntılı adımlar](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="constraints"></a>Kısıtlamalar

- Bir VM'de en az bir ağ arabirimi olmalıdır.

- Bir VM'de yalnızca VM boyutu desteklediği kadar ağ arabirimi eklenebilir. Her VM boyutunun kaç ağ arabirimini desteklediği hakkında daha fazla bilgi edinmek için [Linux VM'leri](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows VM'leri](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json)için Azure'daki boyutlara bakın. Tüm boyutlar en az iki ağ arabirimini destekler.

- VM'ye eklediğiniz ağ arabirimleri şu anda başka bir VM'ye eklenemez. Ağ arabirimleri oluşturma hakkında daha fazla bilgi edinmek için [bkz.](virtual-network-network-interface.md#create-a-network-interface)

- Geçmişte, ağ arabirimlerini yalnızca birden çok ağ arabirimini destekleyen ve en az iki ağ arabirimiyle oluşturulan VM'lere ekleyebilirsiniz. VM boyutu birden fazla ağ arabirimini desteklese bile, tek bir ağ arabirimiyle oluşturulan VM'ye ağ arabirimi ekleyemezsiniz. Tam tersine, en az iki ağ arabirimi yle oluşturulan VM'lerin her zaman en az iki ağ arabirimi olması gerekdığından, ağ arabirimlerini yalnızca en az üç ağ arabirimine sahip bir VM'den kaldırabilirsiniz. Bu kısıtlamalar artık geçerli değildir. Artık herhangi bir sayıda ağ arabirimi (VM boyutu tarafından desteklenen sayıya kadar) bir VM oluşturabilirsiniz.

- Varsayılan olarak, VM'ye bağlı ilk ağ arabirimi *birincil* ağ arabirimidir. VM'deki diğer tüm ağ arabirimleri *ikincil* ağ arabirimleridir.

- Giden trafiği hangi ağ arabirimine gönderdiğinizi kontrol edebilirsiniz. Ancak, bir VM varsayılan olarak tüm giden trafiği birincil ağ arabiriminin birincil IP yapılandırmasına atanan IP adresine gönderir.

- Geçmişte, aynı kullanılabilirlik kümesindeki tüm VM'lerin tek veya birden çok ağ arabirimine sahip olması gerekiyordu. Herhangi bir sayıda ağ arabirimine sahip VM'ler artık aynı kullanılabilirlik kurulumunda, VM boyutu tarafından desteklenen sayıya kadar bulunabilir. Bir VM'yi yalnızca oluşturulduğunda kullanılabilirlik kümesine ekleyebilirsiniz. Kullanılabilirlik kümeleri hakkında daha fazla bilgi edinmek için [azure'da VM'lerin kullanılabilirliğini yönet'e](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy)bakın.

- Aynı VM'deki ağ arabirimlerini sanal ağdaki farklı alt ağlara bağlayabilirsiniz. Ancak, ağ arabirimlerinin tümü aynı sanal ağa bağlı olmalıdır.

- Herhangi bir birincil veya ikincil ağ arabiriminin herhangi bir IP yapılandırması için herhangi bir IP adresini bir Azure Yük Dengeleyici arka uç havuzuna ekleyebilirsiniz. Geçmişte, yalnızca birincil ağ arabirimi için birincil IP adresi arka uç havuzuna eklenebilir. IP adresleri ve yapılandırmaları hakkında daha fazla bilgi edinmek için IP [adreslerini ekle, değiştir veya kaldır'](virtual-network-network-interface-addresses.md)a bakın.

- VM'yi silmek, bağlı olan ağ arabirimlerini silmez. Bir VM sildiğinizde, ağ arabirimleri VM'den ayrılır. Bu ağ arabirimlerini farklı VM'lere ekleyebilir veya silebilirsiniz.

- IPv6'da olduğu gibi, oluşturduktan sonra VM'ye hızlandırılmış ağ etkinleştirilmiş bir ağ arabirimi takamazsınız. Ayrıca, hızlandırılmış ağdan yararlanmak için VM işletim sistemi içindeki adımları da tamamlamanız gerekir. [Windows](create-vm-accelerated-networking-powershell.md) veya [Linux](create-vm-accelerated-networking-cli.md) sanal makineleri için hızlandırılmış ağ kullanımı ve diğer kısıtlamalar hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Birden çok ağ arabirimine veya IP adresine sahip bir VM oluşturmak için bkz:

|Görev|Araç|
|---|---|
|Birden çok NIC ile VM oluşturma|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Birden çok IPv4 adresi olan tek bir NIC VM oluşturma|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Özel IPv6 adresi olan tek bir NIC VM oluşturun (Azure Yük Bakiyeleyicisi'nin arkasında)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Kaynak Yöneticisi şablonu](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
