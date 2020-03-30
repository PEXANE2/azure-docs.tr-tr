---
title: Azure PowerShell ile SQL Server VM'ler için sağlama kılavuzu | Microsoft Dokümanlar
description: SQL Server sanal makine galerisi görüntüleriyle azure VM oluşturmak için adımlar ve PowerShell komutları sağlar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/21/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: b1578547fbca4caaecb209021569f0fbb2f1ae24
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74790639"
---
# <a name="how-to-provision-sql-server-virtual-machines-with-azure-powershell"></a>Azure PowerShell ile SQL Server sanal makineleri nasıl sağlar?

Bu kılavuzda, Azure PowerShell ile Windows SQL Server VM'leri oluşturma seçeneklerinizi açıklanmaktadır. Daha fazla varsayılan değere sahip basitleştirilmiş bir Azure PowerShell örneği için [SQL VM Azure PowerShell hızlı başlat'A](quickstart-sql-vm-create-powershell.md)bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

## <a name="configure-your-subscription"></a>Aboneliğinizi yapılandırma

1. PowerShell'i açın ve **Connect-AzAccount** komutunu çalıştırarak Azure hesabınıza erişimi kurun.

   ```powershell
   Connect-AzAccount
   ```

1. Kimlik bilgilerinizi girmek için bir ekran görmeniz gerekir. Azure portala giriş yapmak için aynı e-posta adresini ve parolayı kullanın.

## <a name="define-image-variables"></a>Görüntü değişkenlerini tanımlama
Değerleri yeniden kullanmak ve komut dosyası oluşturmayı basitleştirmek için, bir dizi değişken tanımlayarak başlayın. Parametre değerlerini istediğiniz gibi değiştirin, ancak sağlanan değerleri değiştirirken ad uzunlukları ve özel karakterlerle ilgili adlandırma kısıtlamalarına dikkat edin.

### <a name="location-and-resource-group"></a>Konum ve kaynak grubu
Veri bölgesini ve diğer VM kaynaklarını oluşturduğunuz kaynak grubunu tanımlayın.

İstediğinizi değiştirin ve bu değişkenleri başlatmak için bu cmdlets çalıştırın.

```powershell
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"
```

### <a name="storage-properties"></a>Depolama özellikleri
Sanal makine tarafından kullanılacak depolama hesabını ve depolama türünü tanımlayın.

İstediğiniz gibi değiştirin ve bu değişkenleri başlatmanız için aşağıdaki cmdlet'i çalıştırın. Üretim iş yükleri için [premium SSD](../disks-types.md#premium-ssd) kullanmanızı öneririz.

```powershell
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"
```

### <a name="network-properties"></a>Ağ özellikleri
Sanal makinede ağ tarafından kullanılacak özellikleri tanımlayın. 

- Ağ arabirimi
- TCP/IP ayırma yöntemi
- Sanal ağın adı
- Sanal alt ağ adı
- Sanal ağ için IP adresleri aralığı
- Alt ağ için IP adresleri aralığı
- Genel etki alanı adı etiketi

İstediğinizi değiştirin ve bu değişkenleri başlatmak için bu cmdlet'i çalıştırın.

```powershell
$InterfaceName = $ResourceGroupName + "ServerInterface"
$NsgName = $ResourceGroupName + "nsg"
$TCPIPAllocationMethod = "Dynamic"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$DomainName = $ResourceGroupName
```

### <a name="virtual-machine-properties"></a>Sanal makine özellikleri
Sanal makine nin sanal makine adını, bilgisayar adını, sanal makine boyutunu ve işletim sistemi disk adını tanımlayın.

İstediğinizi değiştirin ve bu değişkenleri başlatmak için bu cmdlet'i çalıştırın.

```powershell
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"
```

### <a name="choose-a-sql-server-image"></a>BIR SQL Server görüntüsü seçin

Sanal makine için kullanmak üzere SQL Server görüntüsünü tanımlamak için aşağıdaki değişkenleri kullanın. 

1. İlk olarak, `Get-AzVMImageOffer` komutu ile tüm SQL Server görüntü teklifleri listeleyin. Bu komut, Azure Portalı'nda bulunan güncel görüntüleri ve yalnızca PowerShell ile yüklenebilen eski görüntüleri listeler:

   ```powershell
   Get-AzVMImageOffer -Location $Location -Publisher 'MicrosoftSQLServer'
   ```

1. Bu öğretici için, Windows Server 2016'da SQL Server 2017'yi belirtmek için aşağıdaki değişkenleri kullanın.

   ```powershell
   $OfferName = "SQL2017-WS2016"
   $PublisherName = "MicrosoftSQLServer"
   $Version = "latest"
   ```

1. Ardından, teklifiniz için kullanılabilir sürümleri listele.

   ```powershell
   Get-AzVMImageSku -Location $Location -Publisher 'MicrosoftSQLServer' -Offer $OfferName | Select Skus
   ```

1. Bu eğitim için SQL Server 2017 Developer sürümünü **(SQLDEV)** kullanın. Geliştirici sürümü test ve geliştirme için serbestçe lisanslanır ve yalnızca VM'yi çalıştırma nın maliyetini ödlersiniz.

   ```powershell
   $Sku = "SQLDEV"
   ```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Kaynak Yöneticisi dağıtım modelinde, oluşturduğunuz ilk nesne kaynak grubudur. Azure kaynak grubu ve kaynaklarını oluşturmak için [Yeni Kaynak Grubu](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) cmdlet'ini kullanın. Kaynak grubu adı ve konumu için daha önce aranızdaki değişkenleri belirtin.

Yeni kaynak grubunuzu oluşturmak için bu cmdlet'i çalıştırın.

```powershell
New-AzResourceGroup -Name $ResourceGroupName -Location $Location
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
Sanal makine işletim sistemi diski ve SQL Server veri ve günlük dosyaları için depolama kaynakları gerektirir. Basitlik için, her ikisi için de tek bir disk oluşturursunuz. SQL Server verilerinizi ve günlük dosyalarınızı özel disklere yerleştirmek için [Azure Disk Ekle](https://docs.microsoft.com/powershell/module/servicemanagement/azure/add-azuredisk) cmdlet'i kullanarak daha sonra ek diskler ekleyebilirsiniz. Yeni kaynak grubunuzda standart bir depolama hesabı oluşturmak için [Yeni Depolama Hesabı](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount) cmdlet'ini kullanın. Depolama hesabı adı, depolama Sku adı ve konumu için daha önce aranızdaki değişkenleri belirtin.

Yeni depolama hesabınızı oluşturmak için bu cmdlet'i çalıştırın.

```powershell
$StorageAccount = New-AzStorageAccount -ResourceGroupName $ResourceGroupName `
   -Name $StorageName -SkuName $StorageSku `
   -Kind "Storage" -Location $Location
```

> [!TIP]
> Depolama hesabının oluşturulması birkaç dakika sürebilir.

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma
Sanal makine ağ bağlantısı için bir dizi ağ kaynağı gerektirir.

* Her sanal makine bir sanal ağ gerektirir.
* Sanal ağ en az bir alt ağa tanımlanmış olmalıdır.
* Bir ağ arabirimi ortak veya özel bir IP adresi ile tanımlanmalıdır.

### <a name="create-a-virtual-network-subnet-configuration"></a>Sanal ağ alt ağ yapılandırması oluşturma
Sanal ağınız için bir alt ağ yapılandırması oluşturarak başlayın. Bu öğretici için, [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig) cmdlet kullanarak varsayılan bir alt ağ oluşturun. Alt ağ adı ve adres öneki için daha önce başharfe başlattığınız değişkenleri belirtin.

> [!NOTE]
> Bu cmdlet kullanarak sanal ağ alt net yapılandırmaek özelliklerini tanımlayabilirsiniz, ancak bu öğretici kapsamı dışındadır.

Sanal alt ağ yapılandırmanızı oluşturmak için bu cmdlet'i çalıştırın.

```powershell
$SubnetConfig = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
```

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
Ardından, [Yeni-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork) cmdlet'ini kullanarak yeni kaynak grubunuzda sanal ağınızı oluşturun. Ad, konum ve adres öneki için daha önce başharfe başladını verdiğiniz değişkenleri belirtin. Önceki adımda tanımladığınız alt ağ yapılandırmasını kullanın.

Sanal ağınızı oluşturmak için bu cmdlet'i çalıştırın.

```powershell
$VNet = New-AzVirtualNetwork -Name $VNetName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
```

### <a name="create-the-public-ip-address"></a>Genel IP adresini oluşturma
Sanal ağınız tanımlandığına göre, sanal makineye bağlantı için bir IP adresi yapılandırmanız gerekir. Bu öğretici için, Internet bağlantısını desteklemek için dinamik IP adreslerini kullanarak herkese açık bir IP adresi oluşturun. Yeni kaynak grubunuzdaki genel IP adresini oluşturmak için [Yeni-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) cmdlet'i kullanın. Ad, konum, ayırma yöntemi ve DNS etki alanı adı etiketi için daha önce aranızdaki değişkenleri belirtin.

> [!NOTE]
> Bu cmdlet kullanarak ortak IP adresinin ek özelliklerini tanımlayabilirsiniz, ancak bu ilk öğreticinin kapsamı dışındadır. Ayrıca, özel bir adres veya statik bir adrese sahip bir adres de oluşturabilirsiniz, ancak bu da bu öğreticinin kapsamı dışındadır.

Genel IP adresinizi oluşturmak için bu cmdlet'i çalıştırın.

```powershell
$PublicIp = New-AzPublicIpAddress -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
```

### <a name="create-the-network-security-group"></a>Ağ güvenlik grubunu oluşturma
VM ve SQL Server trafiğini güvence altına almak için bir ağ güvenlik grubu oluşturun.

1. İlk olarak, RDP'nin uzak masaüstü bağlantılarına izin vermesi için bir ağ güvenliği grubu kuralı oluşturun.

   ```powershell
   $NsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp `
      -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
   ```
1. TCP bağlantı noktası 1433'te trafiğe izin veren bir ağ güvenlik grubu kuralını yapılandırın. Bunu yapmak, internet üzerinden SQL Server'a bağlantı sağlar.

   ```powershell
   $NsgRuleSQL = New-AzNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp `
      -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
   ```

1. Ağ güvenlik grubunu oluşturun.

   ```powershell
   $Nsg = New-AzNetworkSecurityGroup -ResourceGroupName $ResourceGroupName `
      -Location $Location -Name $NsgName `
      -SecurityRules $NsgRuleRDP,$NsgRuleSQL
   ```

### <a name="create-the-network-interface"></a>Ağ arabirimini oluşturma
Artık sanal makineniz için ağ arabirimi oluşturmaya hazırsınız. Yeni kaynak grubunuzda ağ arabirimioluşturmak için [Yeni-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) cmdlet kullanın. Daha önce tanımlanan adı, konumu, alt ağı ve genel IP adresini belirtin.

Ağ arabirimi oluşturmak için bu cmdlet çalıştırın.

```powershell
$Interface = New-AzNetworkInterface -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id `
   -NetworkSecurityGroupId $Nsg.Id
```

## <a name="configure-a-vm-object"></a>VM nesnesi yapılandırma
Depolama ve ağ kaynakları tanımlandığına göre, sanal makine için bilgi işlem kaynaklarını tanımlamaya hazırsınız.

- Sanal makine boyutunu ve çeşitli işletim sistemi özelliklerini belirtin.
- Daha önce oluşturduğunuz ağ arabirimini belirtin.
- Blob depolama tanımlayın.
- İşletim sistemi diskini belirtin.

### <a name="create-the-vm-object"></a>VM nesnesini oluşturma
Sanal makine boyutunu belirterek başlayın. Bu öğretici için bir DS13 belirtin. Yapılandırılabilir bir sanal makine nesnesi oluşturmak için [Yeni-AzVMConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azvmconfig) cmdlet kullanın. Ad ve boyut için daha önce başharfe başladını aldığınız değişkenleri belirtin.

Sanal makine nesnesi oluşturmak için bu cmdlet çalıştırın.

```powershell
$VirtualMachine = New-AzVMConfig -VMName $VMName -VMSize $VMSize
```

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Yerel yönetici kimlik bilgilerinin adını ve parolasını tutmak için bir kimlik bilgisi nesnesi oluşturma
Sanal makine için işletim sistemi özelliklerini ayarlamadan önce, yerel yönetici hesabının kimlik bilgilerini güvenli bir dize olarak sağlamanız gerekir. Bunu başarmak için [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet'i kullanın.

Aşağıdaki cmdlet'i çalıştırın ve PowerShell kimlik bilgisi istek penceresinde, sanal makinedeki yerel yönetici hesabı için kullanılacak ad ve parolayı yazın.

```powershell
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
```

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Sanal makine için işletim sistemi özelliklerini ayarlama
Artık Sanal makinenin işletim sistemi özelliklerini [Set-AzVMOperatingSystem](https://docs.microsoft.com/powershell/module/az.compute/set-azvmoperatingsystem) cmdlet ile ayarlamaya hazırsınız.

- İşletim sisteminin türünü Windows olarak ayarlayın.
- Sanal [makine aracısının](../../extensions/agent-windows.md) yüklenmesini gerektirir.
- Cmdlet'in otomatik güncelleştirmeyi etkinleştirdiğini belirtin.
- Sanal makine adı, bilgisayar adı ve kimlik bilgisi için daha önce başharfe başladını aldığınız değişkenleri belirtin.

Sanal makinenizin işletim sistemi özelliklerini ayarlamak için bu cmdlet'i çalıştırın.

```powershell
$VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine `
   -Windows -ComputerName $ComputerName -Credential $Credential `
   -ProvisionVMAgent -EnableAutoUpdate
```

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Ağ arabirimini sanal makineye ekleme
Ardından, daha önce tanımladığınız değişkeni kullanarak ağ arabirimini eklemek için [Add-AzVMNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/add-azvmnetworkinterface) cmdlet'i kullanın.

Sanal makinenizin ağ arabirimini ayarlamak için bu cmdlet'i çalıştırın.

```powershell
$VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
```

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Sanal makine tarafından kullanılacak disk için blob depolama konumunu ayarlama
Ardından, daha önce tanımladığınız değişkenleri kullanarak VM diskinin blob depolama konumunu ayarlayın.

Blob depolama konumunu ayarlamak için bu cmdlet çalıştırın.

```powershell
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
```

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Sanal makine için işletim sistemi disk özelliklerini ayarlama
Ardından, [Set-AzVMOSDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk) cmdlet'i kullanarak sanal makinenin işletim sistemi disk özelliklerini ayarlayın. 

- Sanal makinenin işletim sisteminin bir görüntüden geleceğini belirtin.
- Yalnızca okunacak şekilde önbelleğe alma ayarlayın (SQL Server aynı diske yüklü olduğundan).
- VM adı ve işletim sistemi diski için daha önce aranızgelen değişkenleri belirtin.

Sanal makinenizin işletim sistemi disk özelliklerini ayarlamak için bu cmdlet'i çalıştırın.

```powershell
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name `
   $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage
```

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Sanal makine için platform görüntüsünü belirtin
Son yapılandırma adımı, sanal makineniz için platform görüntüsünü belirtmektir. Bu eğitim için en son SQL Server 2016 CTP görüntüsünü kullanın. Bu görüntüyü daha önce tanımladığınız değişkenlerle kullanmak için [Set-AzVMSourceImage](https://docs.microsoft.com/powershell/module/az.compute/set-azvmsourceimage) cmdlet'i kullanın.

Sanal makinenizin platform görüntüsünü belirtmek için bu cmdlet'i çalıştırın.

```powershell
$VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine `
   -PublisherName $PublisherName -Offer $OfferName `
   -Skus $Sku -Version $Version
```

## <a name="create-the-sql-vm"></a>SQL VM'sini oluşturma
Yapılandırma adımlarını tamamladığınızda, sanal makineyi oluşturmaya hazırsınız. Tanımladığınız değişkenleri kullanarak sanal makineyi oluşturmak için [Yeni-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) cmdlet'ini kullanın.

> [!TIP]
> VM oluşturma birkaç dakika sürebilir.

Sanal makine oluşturmak için bu cmdlet çalıştırın.

```powershell
New-AzVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

Sanal makine oluşturulur.

> [!NOTE]
> Önyükleme tanılama yla ilgili bir hata alırsanız, bunu yok sayabilirsiniz. Sanal makinenin diski için belirtilen depolama hesabı premium depolama hesabı olduğundan önyükleme tanılama için standart bir depolama hesabı oluşturulur.

## <a name="install-the-sql-iaas-agent"></a>SQL Iaas Aracısı'nı yükleme
SQL Server sanal makineleri [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md)ile otomatik yönetim özelliklerini destekler. Aracıyı yeni VM'ye yüklemek ve kaynak sağlayıcısına kaydetmek için sanal makine oluşturulduktan sonra [Yeni-AzSqlVM](/powershell/module/az.sqlvirtualmachine/new-azsqlvm) komutunu çalıştırın. SQL Server VM'nizin lisans türünü belirterek, [Azure Karma Avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/)üzerinden kullandıkça öde veya kendi lisansınızı getir arasında seçim yapmayı belirleyin. Lisanslama hakkında daha fazla bilgi için lisans [lama modeline](virtual-machines-windows-sql-ahb.md)bakın. 


   ```powershell
   New-AzSqlVM -ResourceGroupName $ResourceGroupName -Name $VMName -Location $Location -LicenseType <PAYG/AHUB> 
   ```


## <a name="stop-or-remove-a-vm"></a>Bir VM'yi durdurma veya kaldırma

VM'nin sürekli çalışmasına gerek yoksa, kullanılmadığı zaman durdurarak gereksiz ücretleri önleyebilirsiniz. Aşağıdaki komut VM'yi durdurur ancak özelliği daha sonra kullanılmak üzere muhafaza eder.

```powershell
Stop-AzVM -Name $VMName -ResourceGroupName $ResourceGroupName
```

Ayrıca, **Remove-AzResourceGroup** komutuyla sanal makineyle ilişkili tüm kaynakları kalıcı olarak silebilirsiniz. Bunu yapmak da sanal makineyi kalıcı olarak siler, bu nedenle bu komutu dikkatli kullanın.

## <a name="example-script"></a>Örnek betik
Aşağıdaki komut dosyası bu öğretici için tam PowerShell komut içerir. Azure aboneliğini **Connect-AzAccount** ve **Select-AzSubscription** komutlarıyla kullanmak üzere zaten ayarladığınızı varsayar.

```powershell
# Variables

## Global
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"

## Storage
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"

## Network
$InterfaceName = $ResourceGroupName + "ServerInterface"
$NsgName = $ResourceGroupName + "nsg"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$TCPIPAllocationMethod = "Dynamic"
$DomainName = $ResourceGroupName

##Compute
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"

##Image
$PublisherName = "MicrosoftSQLServer"
$OfferName = "SQL2017-WS2016"
$Sku = "SQLDEV"
$Version = "latest"

# Resource Group
New-AzResourceGroup -Name $ResourceGroupName -Location $Location

# Storage
$StorageAccount = New-AzStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

# Network
$SubnetConfig = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
$VNet = New-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
$PublicIp = New-AzPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
$NsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
$NsgRuleSQL = New-AzNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
$Nsg = New-AzNetworkSecurityGroup -ResourceGroupName $ResourceGroupName -Location $Location -Name $NsgName -SecurityRules $NsgRuleRDP,$NsgRuleSQL
$Interface = New-AzNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id -NetworkSecurityGroupId $Nsg.Id

# Compute
$VirtualMachine = New-AzVMConfig -VMName $VMName -VMSize $VMSize
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
$VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
$VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

# Image
$VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

# Create the VM in Azure
New-AzVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

# Add the SQL IaaS Extension, and choose the license type
New-AzSqlVM -ResourceGroupName $ResourceGroupName -Name $VMName -Location $Location -LicenseType <PAYG/AHUB> 
```

## <a name="next-steps"></a>Sonraki adımlar
Sanal makine oluşturulduktan sonra şunları yapabilirsiniz:

- RDP kullanarak sanal makineye bağlanma
- VM'niz için portaldaki SQL Server ayarlarını yapılandırın:
   - [Depolama ayarları](virtual-machines-windows-sql-server-storage-configuration.md) 
   - [Otomatik yönetim görevleri](virtual-machines-windows-sql-server-agent-extension.md)
- [Bağlantıyı yapılandırma](virtual-machines-windows-sql-connect.md)
- İstemcileri ve uygulamaları yeni SQL Server örneğine bağlayın

