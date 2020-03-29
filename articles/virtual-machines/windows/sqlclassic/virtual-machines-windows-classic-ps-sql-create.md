---
title: Klasik bir SQL Server VM (PowerShell) oluşturma
description: SQL Server sanal makine galerisi görüntüleriyle azure VM oluşturmak için adımlar ve PowerShell komut dosyaları sağlar. Bu konu klasik dağıtım modunu kullanır.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-service-management
ms.assetid: b73be387-9323-4e08-be53-6e5928e3786e
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 5bfdcfab37091a5f581ce147c0a6af5ccd8147a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77914798"
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Azure PowerShell’i kullanarak bir SQL Server sanal makinesi sağlama (Klasik)

[!INCLUDE [classic-vm-deprecation](../../../../includes/classic-vm-deprecation.md)]

Bu makalede, PowerShell cmdlets kullanarak Azure'da bir SQL Server sanal makine oluşturmak için nasıl adımlar sağlar.

> [!NOTE] 
> Azure'un kaynakları oluşturmak ve onlarla çalışmak için iki farklı dağıtım modeli vardır: [Kaynak Yöneticisi ve Klasik.](../../../azure-resource-manager/management/deployment-models.md) Bu makalede, Klasik dağıtım modeli kullanılarak kapsar. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Bu konunun Kaynak Yöneticisi sürümü için Azure [PowerShell Kaynak Yöneticisi'ni kullanarak sql server sanal makine sağlama](../sql/virtual-machines-windows-ps-sql-create.md)bölümüne bakın.

### <a name="install-and-configure-powershell"></a>PowerShell'i yükleyin ve yapılandırın:
1. Bir Azure hesabınız yoksa, [Azure ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/)yi ziyaret edin.
2. [En son Azure PowerShell komutlarını indirin ve yükleyin.](/powershell/azure/overview)
3. Windows PowerShell'i başlatın ve Azure Aboneliğinize **Ekle-Azure Hesabı** komutuyla bağlayın.

   ```powershell
   Add-AzureAccount
   ```

## <a name="determine-your-target-azure-region"></a>Hedef Azure bölgenizi belirleme

SQL Server Virtual Machine'iniz belirli bir Azure bölgesinde bulunan bir bulut hizmetinde barındırılacaktır. Aşağıdaki adımlar, öğreticinin geri kalanı için kullanılacak bölgenizi, depolama hesabınızı ve bulut hizmetinizi belirlemenize yardımcı olur.

1. SQL Server VM'nizi barındırmak için kullanmak istediğiniz veri merkezini belirleyin. Aşağıdaki PowerShell komutu kullanılabilir bölge adlarının listesini görüntüler.

   ```powershell
   (Get-AzureLocation).Name
   ```

2. Tercih ettiğiniz konumu belirledikten sonra, o bölgeye **$dcLocation** adlı bir değişken ayarlayın. Örneğin, aşağıdaki komut bölgeyi "Doğu ABD" olarak ayarlar:

   ```powershell
   $dcLocation = "East US"
   ```

## <a name="set-your-subscription-and-storage-account"></a>Abonelik ve depolama hesabınızı ayarlama

1. Yeni sanal makine için kullanacağınız Azure aboneliğini belirleyin.

   ```powershell
   (Get-AzureSubscription).SubscriptionName
   ```

2. Hedef Azure aboneliğinizi **$subscr** değişkenine atayın. Ardından bunu geçerli Azure aboneliğiniz olarak ayarlayın.

   ```powershell
   $subscr="<subscription name>"
   Select-AzureSubscription -SubscriptionName $subscr –Current
   ```

3. Ardından varolan depolama hesaplarını denetleyin. Aşağıdaki komut dosyası, seçtiğiniz bölgede bulunan tüm depolama hesaplarını görüntüler:

   ```powershell
   (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName
   ```

   > [!NOTE]
   > Yeni bir depolama hesabı nız gerekiyorsa, önce aşağıdaki örnekte olduğu gibi New-AzureStorageAccount komutuyla birlikte çok küçük bir depolama hesabı adı oluşturun:`New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation`

4. Hedef depolama hesabı adını **$staccount**atayın. Ardından, abonelik ve geçerli depolama hesabını ayarlamak için **AzureAboneliği'ni** kullanın.

   ```powershell
   $staccount="<storage account name>"
   Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount
   ```

## <a name="select-a-sql-server-virtual-machine-image"></a>SQL Server sanal makine görüntüsü seçin

1. Mevcut SQL Server sanal makine resimlerinin listesini galeriden öğrenin. Bu görüntülerin tümü "SQL" ile başlayan bir **ImageFamily** özelliğine sahiptir. Aşağıdaki sorgu, SQL Server önceden yüklenmiş olan görüntü ailesini görüntüler.

   ```powershell
   Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily
   ```

2. Sanal makine görüntüsü ailesini bulduğunuzda, bu ailede birden çok yayınlanmış resim olabilir. Seçtiğiniz resim aileniz için en son yayınlanan sanal makine resim adını bulmak için aşağıdaki komut dosyasını kullanın **(Windows Server 2012 R2'deki SQL Server 2016 RTM Enterprise**gibi):

   ```powershell
   $family="<ImageFamily value>"
   $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

   echo "Selected SQL Server image name:"
   echo "   $image"
   ```

## <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

Son olarak, PowerShell ile sanal makine oluşturun:

1. Yeni VM'yi barındırmak için bir bulut hizmeti oluşturun. Bunun yerine varolan bir bulut hizmetini kullanmanın da mümkün olduğunu unutmayın. Bulut hizmetinin kısa adı ile yeni bir değişken **$svcname** oluşturun.

   ```powershell
   $svcname = "<cloud service name>"
   New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation
   ```

2. Sanal makine adını ve boyutunu belirtin. Sanal makine boyutları hakkında daha fazla bilgi için [Azure için Sanal Makine Boyutları'na](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.

   ```powershell
   $vmname="<machine name>"
   $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see the link to the other VM sizes>"
   $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image
   ```

3. Yerel yönetici hesabını ve parolasını belirtin.

   ```powershell
   $cred=Get-Credential -Message "Type the name and password of the local administrator account."
   $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password
   ```

4. Sanal makineyi oluşturmak için aşağıdaki komut dosyasını çalıştırın.

   ```powershell
   New-AzureVM –ServiceName $svcname -VMs $vm1
   ```

> [!NOTE]
> Ek açıklama ve yapılandırma seçenekleri için, Windows [tabanlı Sanal Makineler oluşturmak ve önceden yapılandırmak için Azure PowerShell'i kullan'daki](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) **komut setinizi oluştur** bölümüne bakın.

## <a name="example-powershell-script"></a>Örnek PowerShell betiği

Aşağıdaki komut dosyası, **Windows Server 2012 R2 sanal makinede BIR SQL Server 2016 RTM Enterprise** oluşturan tam bir komut dosyası örneği sağlar. Bu komut dosyasını kullanıyorsanız, bu konudaönceki adımları temel alan ilk değişkenleri özelleştirmeniz gerekir.

```powershell
# Customize these variables based on your settings and requirements:
$dcLocation = "East US"
$subscr="mysubscription"
$staccount="mystorageaccount"
$family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
$svcname = "mycloudservice"
$vmname="myvirtualmachine"
$vmsize="A5"

# Set the current subscription and storage account
# Comment out the New-AzureStorageAccount line if the account already exists
Select-AzureSubscription -SubscriptionName $subscr –Current
New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

# Select the most recent VM image in this image family:
$image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

# Create the new cloud service; comment out this line if cloud service exists already:
New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

# Create the VM config:
$vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

# Set administrator credentials:
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

# Create the SQL Server VM:
New-AzureVM –ServiceName $svcname -VMs $vm1
```

## <a name="connect-with-remote-desktop"></a>Uzak masaüstüyle bağlanma

1. Kurulumu tamamlamak için bu sanal makineleri başlatmak için geçerli kullanıcının belge klasöründe RDP dosyalarını oluşturun:

   ```powershell
   $documentspath = [environment]::getfolderpath("mydocuments")
   Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"
   ```

2. Belgeler dizininde RDP dosyasını başlatın. Daha önce sağlanan yönetici kullanıcı adı ve parolayla bağlantı kurun (örneğin, kullanıcı adınız VMAdmin ise, kullanıcı olarak "\VMAdmin" belirtin ve parolayı girin).

   ```powershell
   cd $documentspath
   .\vm1.rdp
   ```

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a>Uzaktan erişim için SQL Server Machine yapılandırmasını tamamlama

Makinede uzak masaüstü ile oturum açtıktan sonra, SQL Server bağlantısını [Bir Azure VM'de yapılandırmak için Adımlar'daki](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)yönergelere göre SQL Server'ı yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

[Sanal makine belgelerinde](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)PowerShell ile sanal makineler sağlama için ek talimatlar bulabilirsiniz.

Çoğu durumda, bir sonraki adım veritabanlarınızı bu yeni SQL Server VM'ye geçirmektir. Veritabanı geçişi kılavuzu için bkz: [Azure VM'de Veritabanını SQL Server'a Geçirme](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).

SQL Sanal Makineler oluşturmak için Azure portalını kullanmak istiyorsanız, [Azure'da SQL Server Sanal Makine Sağlama](../sql/virtual-machines-windows-portal-sql-server-provision.md)konusuna bakın. Portalda size yol gösteren öğreticinin, bu PowerShell konusunda kullanılan klasik model yerine önerilen Kaynak Yöneticisi modelini kullanarak VM'ler oluşturduğunu unutmayın.

Bu kaynaklara ek olarak, [Azure Sanal Makinelerde SQL Server'ı çalıştırmakla ilgili diğer konuları](../sql/virtual-machines-windows-sql-server-iaas-overview.md)gözden geçirmenizi öneririz.
