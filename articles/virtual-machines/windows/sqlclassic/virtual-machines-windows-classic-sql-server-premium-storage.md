---
title: SQL Server ile Azure Premium Depolama'yı kullanma | Microsoft Dokümanlar
description: Bu makalede, klasik dağıtım modeliyle oluşturulan kaynaklar kullanır ve Azure Sanal Makinelerde çalışan SQL Server ile Azure Premium Depolama'yı kullanma konusunda kılavuz luk verir.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 7ccf99d7-7cce-4e3d-bbab-21b751ab0e88
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/01/2017
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 479f9abc667e20a136da5f6231e78a1e4052f087
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75965663"
---
# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Sanal Makineler’de SQL Server ile Azure Premium Depolama kullanma

## <a name="overview"></a>Genel Bakış

[Azure premium SSD'ler,](../disks-types.md) düşük gecikme gecikmesi ve yüksek iş ortası IO sağlayan yeni nesil depolama alanıdır. IaaS [Sanal Makinelerdeki](https://azure.microsoft.com/services/virtual-machines/)SQL Server gibi önemli IO yoğun iş yükleri için en iyi şekilde çalışır.

> [!IMPORTANT]
> Azure'un kaynakları oluşturmak ve onlarla çalışmak için iki farklı dağıtım modeli vardır: [Kaynak Yöneticisi ve Klasik.](../../../azure-resource-manager/management/deployment-models.md) Bu makalede, Klasik dağıtım modeli kullanılarak kapsar. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Bu makalede, Premium Depolama kullanmak için SQL Server çalıştıran bir Sanal Makine geçiş için planlama ve rehberlik sağlar. Buna Azure altyapısı (ağ, depolama) ve konuk Windows VM adımları dahildir. [Ekteki](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) örnek, PowerShell ile geliştirilmiş yerel SSD depolamadan yararlanmak için daha büyük VM'lerin nasıl taşınacaklarına kadar kapsamlı bir geçiş sonu gösterir.

IAAS VM'lerde SQL Server ile Azure Premium Depolama'yı kullanmanın uçtan uca işlemini anlamak önemlidir. Buna aşağıdakiler dahildir:

* Premium Depolama'yı kullanmak için ön koşulların belirlenmesi.
* Yeni dağıtımlar için IaaS'da SQL Server'ı Premium Depolama'ya dağıtma örnekleri.
* Sql Always On Availability Groups'u kullanarak hem tek başına sunucular hem de dağıtımlar gibi varolan dağıtımları geçirme örnekleri.
* Olası göç yaklaşımları.
* Varolan bir Always On uygulamasının geçişi için Azure, Windows ve SQL Server adımlarını gösteren tam uçtan uca örnek.

Azure Sanal Makineler'deki SQL Server hakkında daha fazla bilgi için [Azure Sanal Makineler'deki SQL Server'a](../sql/virtual-machines-windows-sql-server-iaas-overview.md)bakın.

**Yazar:** Daniel Sol **Teknik Hakemler:** Luis Carlos Vargas Herring, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz.

## <a name="prerequisites-for-premium-storage"></a>Premium Depolama için Ön Koşullar

Premium Depolama'yı kullanmak için çeşitli ön koşullar vardır.

### <a name="machine-size"></a>Makine boyutu

Premium Depolama'yı kullanmak için DS serisi Sanal Makineler 'i (VM) kullanmanız gerekir. Daha önce bulut hizmetinizde DS Serisi makinelerini kullanmadıysanız, varolan VM'yi silmeniz, ekli diskleri saklamanız ve VM'yi DS* rol boyutu olarak yeniden oluşturmadan önce yeni bir bulut hizmeti oluşturmanız gerekir. Sanal Makine boyutları hakkında daha fazla bilgi için [Azure için Sanal Makine ve Bulut Hizmeti Boyutları'na](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.

### <a name="cloud-services"></a>Bulut hizmetleri

DS* VM'leri yalnızca yeni bir bulut hizmetinde oluşturulduklarında Premium Depolama ile kullanabilirsiniz. Azure'da SQL Server Always On kullanıyorsanız, Her Zaman Dinleyici, bir bulut hizmetiyle ilişkili Azure İç veya Dış Yük Dengeleyicisi IP adresini ifade eder. Bu makalede, bu senaryoda kullanılabilirliğini korurken nasıl geçirilen üzerinde duruluyor.

> [!NOTE]
> DS* Serisi, yeni Bulut Hizmetine dağıtılan ilk VM olmalıdır.
>
>

### <a name="regional-vnets"></a>Bölgesel VNETS

DS* VM'ler için Sanal Ağı (VNET) barındıran Sanal Ağı bölgesel olarak yapılandırmanız gerekir. Bu "genişleten" VNET, daha büyük VM'lerin diğer kümelerde sağlanmasına ve aralarındaki iletişimin sağlanmasına izin vermektir. Aşağıdaki ekran görüntüsünde, vurgulanan Konum bölgesel VNET'leri gösterirken, ilk sonuç "dar" bir VNET gösterir.

![RegionalVNET][1]

Bölgesel bir VNET'e geçiş yapmak için Microsoft destek bileti kaldırabilirsiniz. Microsoft daha sonra bir değişiklik yapar. Bölgesel VNET'lere geçişi tamamlamak için, ağ yapılandırmasındaki Özellik AffinityGroup'u değiştirin. Önce PowerShell'de Ağ Yapılandırmasını dışa aktarın ve ardından **VirtualNetworkSite** öğesindeki **AffinityGroup** özelliğini **konum** özelliğiyle değiştirin. Azure `Location = XXXX` `XXXX` bölgesinin nerede olduğunu belirtin. Sonra yeni yapılandırmayı aktarın.

Örneğin, aşağıdaki VNET yapılandırmasını göz önünde bulundurarak:

```xml
<VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
<AddressSpace>
  <AddressPrefix>10.0.0.0/8</AddressPrefix>
  <AddressPrefix>172.16.0.0/12</AddressPrefix>
</AddressSpace>
<Subnets>
...
</VirtualNetworkSite>
```

Bunu Batı Avrupa'daki bölgesel bir VNET'e taşımak için yapılandırmayı aşağıdakilere göre değiştirin:
```xml
<VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
<AddressSpace>
  <AddressPrefix>10.0.0.0/8</AddressPrefix>
  <AddressPrefix>172.16.0.0/12</AddressPrefix>
</AddressSpace>
<Subnets>
...
</VirtualNetworkSite>
```

### <a name="storage-accounts"></a>Depolama hesapları

Premium Depolama için yapılandırılan yeni bir depolama hesabı oluşturmanız gerekir. Premium Depolama kullanımının tek tek VHD'lerde değil, depolama hesabında ayarlandığında, DS* Serisi VM kullanırken Premium ve Standart Depolama hesaplarından VHD'ler ekebileceğinizi unutmayın. OS VHD'yi Premium Depolama hesabına yerleştirmek istemiyorsanız bunu düşünebilirsiniz.

"Premium_LRS" **Türüne** sahip aşağıdaki **Yeni AzureStorageAccountPowerShell** komutu bir Premium Depolama Hesabı oluşturur:

```powershell
$newstorageaccountname = "danpremstor"
New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   
```

### <a name="vhds-cache-settings"></a>VHDs Önbellek Ayarları

Premium Depolama hesabının bir parçası olan diskler oluşturma arasındaki temel fark, disk önbelleği ayarıdır. SQL Server Data birim diskleri için '**Önbelleğe Alma**' kullanmanız önerilir. Hareket günlüğü birimleri için disk önbellek ayarı '**Yok**' olarak ayarlanmalıdır. Bu, Standart Depolama hesapları için önerilerden farklıdır.

VHD'ler bağlandıktan sonra önbellek ayarı değiştirilemez. VHD'yi güncelleştirilmiş bir önbellek ayarı ile ayırmanız ve yeniden takmanız gerekir.

### <a name="windows-storage-spaces"></a>Windows depolama alanları

Önceki Standart Depolama alanı ile yaptığınız gibi [Windows Depolama Alanları](https://technet.microsoft.com/library/hh831739.aspx) kullanabilirsiniz, bu zaten Depolama Alanları kullanan bir VM geçirmenize olanak sağlar. [Ek'teki](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) örnekte (adım 9 ve ileri), birden çok bağlı VHD'li bir VM ayıklamak ve almak için Powershell kodunu gösterir.

Depolama Havuzları, iş ortasını artırmak ve gecikme süresini azaltmak için Standart Azure depolama hesabıyla birlikte kullanılmıştır. Yeni dağıtımlar için Premium Depolama alanı yla Depolama Havuzlarını sınamada değer bulabilirsiniz, ancak depolama kurulumuyla ek karmaşıklık eklerler.

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a>Depolama havuzlarına hangi Azure Sanal Diskler haritasını bulma

Ekli VHD'ler için farklı önbellek belirleme önerileri olduğundan, VHD'leri Premium Depolama hesabına kopyalamaya karar verebilirsiniz. Ancak, bunları yeni DS serisi VM'ye yeniden iliştirdiğinizde önbellek ayarlarını değiştirmeniz gerekebilir. SQL Veri dosyaları ve günlük dosyaları için ayrı VHD'ler (her ikisini de içeren tek bir VHD yerine) varsa Premium Depolama önerilen önbellek ayarlarını uygulamak daha kolaydır.

> [!NOTE]
> Aynı birimde SQL Server verisi ve günlük dosyaları varsa, seçtiğiniz önbelleğe alma seçeneği veritabanı iş yükleri için IO erişim desenlerine bağlıdır. Yalnızca sınama bu senaryo için en iyi önbelleğe alma seçeneği gösterebilir.
>
>

Ancak, birden çok VHD'den oluşan Windows Depolama Alanları kullanıyorsanız, hangi bağlı VHD'lerin hangi havuzda olduğunu belirlemek için orijinal komut dosyalarınıza bakmanız gerekir, böylece önbellek ayarlarını her disk için buna göre ayarlayabilirsiniz.

Depolama havuzuna hangi VHD'lerin eşleştürülücüğünü günü günü günü nüksolarak günü yoksa, disk/ depolama havuzu eşleşimi belirlemek için aşağıdaki adımlarğ

Her disk için aşağıdaki adımları kullanın:

1. **AzureVM Al** komutuyla VM'ye bağlı disklerin listesini alın:

```powershell
Get-AzureVM -ServiceName <servicename> -Name <vmname> | Get-AzureDataDisk
```

1. DiskName ve LUN'u not edin.

    ![DisknameAndLUN][2]
1. VM içine uzak masaüstü. Sonra **Bilgisayar Yönetimi** | **Aygıt Yöneticisi** | **Disk Sürücüler**gidin. 'Microsoft Sanal Diskler' her birinin özelliklerine bakın

    ![VirtualDiskÖzellikleri][3]
1. Buradaki LUN numarası VHD'yi VM'ye takarken belirttiğiniz LUN numarasına bir başvurudur.
1. 'Microsoft Sanal Disk' için **Ayrıntılar** sekmesine gidin, ardından **Özellik** listesinde, **Sürücü Anahtarı'na**gidin. **Değer'** de, aşağıdaki ekran görüntüsünde 0002 olan **Ofset'e**dikkat edin. 0002, depolama havuzunun başvurulettiği PhysicalDisk2'yi gösterir.

    ![VirtualDiskÖzellik Detayları][4]
1. Her depolama havuzu için ilişkili diskleri boşaltın:

```powershell
Get-StoragePool -FriendlyName AMS1pooldata | Get-PhysicalDisk
```

![GetStoragePool][5]

Artık bu bilgileri, ekli VHD'leri Depolama Havuzları'ndaki Fiziksel Disklerle ilişkilendirmek için kullanabilirsiniz.

Depolama Havuzları'ndaki Fiziksel Diskler'e VHD eşledikten sonra bunları bir Premium Depolama hesabına bağlayıp kopyalayabilir ve doğru önbellek ayarına takabilirsiniz. [Ekteki](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage)örneğe bakın , adım 8 ile 12 arasında. Bu adımlar, VM'ye bağlı bir VHD disk yapılandırmasının Bir CSV dosyasına nasıl ayıklanacağını, VHD'leri nasıl kopyalayabildiğini, disk yapılandırma önbelleği ayarlarını nasıl değiştireceğini ve son olarak VM'yi tüm ekli disklerle birlikte DS serisi VM olarak yeniden nasıl dağıtacağını gösterir.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>VM depolama bant genişliği ve VHD depolama iş liği

Depolama performansı miktarı belirtilen DS* VM boyutuna ve VHD boyutlarına bağlıdır. VM'ler, eklenebilen VH'lerin sayısı ve desteklendikleri maksimum bant genişliği (MB/s) için farklı izinlere sahiptir. Belirli bant genişliği numaraları [için Azure için Sanal Makine ve Bulut Hizmeti Boyutları'na](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.

Artırılmış IOPS büyük disk boyutları ile elde edilir. Geçiş yolunuz hakkında düşünürken bunu göz önünde bulundurmalısınız. Ayrıntılar için [IOPS ve Disk Türleri tablosuna bakın.](../disks-types.md#premium-ssd)

Son olarak, VM'lerin bağlı tüm diskler için desteklendikleri farklı maksimum disk bant genişliklerine sahip olduğunu düşünün. Yüksek yük altında, bu VM rol boyutu için kullanılabilir maksimum disk bant genişliği doygunolabilir. Örneğin bir Standard_DS14 512MB/s'ye kadar destekler; bu nedenle, üç P30 diskile VM disk bant genişliği doygun olabilir. Ancak bu örnekte, iş ve işlem limiti okuma ve yazma iOs karışımına bağlı olarak aşılabilir.

## <a name="new-deployments"></a>Yeni dağıtımlar

Sonraki iki bölüm, SQL Server VM'leri Premium Depolama alanına nasıl dağıtabileceğinizi gösterir. Daha önce de belirtildiği gibi, işletim sistemi diskini Premium depolama alanına yerleştirmeniz gerekmez. OS VHD'ye yoğun IO iş yüklerini yerleştirmeyi planlıyorsanız bunu yapmayı tercih edebilirsiniz.

İlk örnek, varolan Azure Galeri Görsellerinin kullanımını göstermektedir. İkinci örnek, varolan bir Standart depolama hesabında bulunan özel bir VM görüntüsünün nasıl kullanılacağını gösterir.

> [!NOTE]
> Bu örnekler, zaten bir Bölgesel VNET oluşturduğunuzu varsayar.

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Galeri Görüntüsü ile Premium Depolama ile yeni bir VM oluşturun

Aşağıdaki örnekte, OS VHD'nin premium depolama alanına nasıl yerleştirilip Premium Depolama VHD'leri nasıl takıldaaçık gösterilmektedir. Ancak, işletim sistemi diskini standart depolama hesabına yerleyebilir ve ardından Premium Depolama hesabında bulunan VHD'leri ekleyebilirsiniz. Her iki senaryo da gösterilmiştir.

```powershell
$mysubscription = "DansSubscription"
$location = "West Europe"

#Set up subscription
Set-AzureSubscription -SubscriptionName $mysubscription
Select-AzureSubscription -SubscriptionName $mysubscription -Current  
```

#### <a name="step-1-create-a-premium-storage-account"></a>Adım 1: Premium Depolama Hesabı Oluşturma

```powershell
#Create Premium Storage account, note Type
$newxiostorageaccountname = "danspremsams"
New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  
```

#### <a name="step-2-create-a-new-cloud-service"></a>Adım 2: Yeni Bir Bulut Hizmeti Oluşturun

```powershell
$destcloudsvc = "danNewSvcAms"
New-AzureService $destcloudsvc -Location $location
```

#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>Adım 3: Bulut Hizmeti VIP rezerve (İsteğe bağlı)

```powershell
#check exisitng reserved VIP
Get-AzureReservedIP

$reservedVIPName = "sqlcloudVIP"
New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location
```

#### <a name="step-4-create-a-vm-container"></a>Adım 4: VM Kapsayıcı oluşturma

```powershell
#Generate storage keys for later
$xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

##Generate storage acc contexts
$xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

#Create container
$containerName = 'vhds'
New-AzureStorageContainer -Name $containerName -Context $xioContext
```

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>Adım 5: OS VHD'yi Standart veya Premium Depolamaya Yerleştirme

```powershell
#NOTE: Set up subscription and default storage account which is used to place the OS VHD in

#If you want to place the OS VHD Premium Storage Account
Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

#If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
$standardstorageaccountname = "danstdams"

Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname
```

#### <a name="step-6-create-vm"></a>Adım 6: VM oluştur

```powershell
#Get list of available SQL Server Images from the Azure Image Gallery.
$galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
$image = $galleryImage.ImageName

#Set up Machine Specific Information
$vmName = "dsDan1"
$vnet = "dansvnetwesteur"
$subnet = "SQL"
$ipaddr = "192.168.0.8"

#Remember to change to DS series VM
$newInstanceSize = "Standard_DS1"

#create new Availability Set
$availabilitySet = "cloudmigAVAMS"

#Machine User Credentials
$userName = "myadmin"
$pass = "mycomplexpwd4*"

#Create VM Config
$vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

#Add Data and Log Disks to VM Config
#Note the size specified ‘-DiskSizeInGB 1023’, this attaches 2 x P30 Premium Storage Disk Type
#Utilising the Premium Storage enabled Storage account

$vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
$vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

#Create VM
$vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

#Add RDP Endpoint
$EndpointNameRDPInt = "3389"
Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

#Check VHD storage account, these should be in $newxiostorageaccountname
Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk
```

### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a>Premium Depolama'yı özel bir görüntüyle kullanmak için yeni bir VM oluşturun

Bu senaryo, standart depolama hesabında bulunan özelleştirilmiş görüntülerin nerede olduğunu gösterir. Belirtildiği gibi, OS VHD'yi Premium Depolama alanına yerleştirmek istiyorsanız, Standart Depolama hesabında bulunan görüntüyü kopyalamanız ve kullanılmadan önce Premium Depolama alanına aktarmanız gerekir. Şirket içinde bir resminiz varsa, bu yöntemi doğrudan Premium Depolama hesabına kopyalamak için de kullanabilirsiniz.

#### <a name="step-1-create-storage-account"></a>Adım 1: Depolama Hesabı Oluşturma

```powershell
$mysubscription = "DansSubscription"
$location = "West Europe"

#Create Premium Storage account
$newxiostorageaccountname = "danspremsams"
New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

#Standard Storage account
$origstorageaccountname = "danstdams"
```

#### <a name="step-2-create-cloud-service"></a>Adım 2 Bulut Hizmeti Oluştur

```powershell
$destcloudsvc = "danNewSvcAms"
New-AzureService $destcloudsvc -Location $location
```

#### <a name="step-3-use-existing-image"></a>Adım 3: Varolan görüntüyü kullanma

Varolan bir görüntü kullanabilirsiniz. Veya, [varolan bir makinenin görüntüsünü alabilirsiniz.](../classic/capture-image-classic.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) Görüntülediğiniz makinenin DS* makinesi olması gerekmediğini unutmayın. Görüntüyü aldıktan sonra, aşağıdaki adımlar, **başlat-AzureStorageBlobCopy** PowerShell komut ile Premium Depolama hesabına nasıl kopyalanabileceğinizi gösterir.

```powershell
#Get storage account keys:
#Standard Storage account
$originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
#Premium Storage account
$xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

#Set up contexts for the storage accounts:
$origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
$destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  
```

#### <a name="step-4-copy-blob-between-storage-accounts"></a>Adım 4: Depolama Hesapları Arasında Blob Kopyalama

```powershell
#Get Image VHD
$myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
$containerName = 'vhds'

#Copy Blob between accounts
$blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
-DestContainer vhds -Destblob "prem-$myImageVHD" `
-Context $origContext -DestContext $destContext  
```

#### <a name="step-5-regularly-check-copy-status"></a>Adım 5: Kopya durumunu düzenli olarak kontrol edin:

```powershell
$blob | Get-AzureStorageBlobCopyState
```

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a>Adım 6: Abonelikte Azure disk Deposuna Resim diski ekleme

```powershell
$imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
$newimageName = "prem"+"dansoldonorsql2k14"

Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation
```

> [!NOTE]
> Durum başarılı olarak rapor lanmış olsa bile, yine de bir disk kiralama hatası alabilirsiniz. Bu durumda, yaklaşık 10 dakika bekleyin.

#### <a name="step-7--build-the-vm"></a>Adım 7: VM oluşturun

Burada görüntünüzden VM'yi oluşturuyor ve iki Premium Depolama VHD'si takmıyorsun:

```powershell
$newimageName = "prem"+"dansoldonorsql2k14"
#Set up Machine Specific Information
$vmName = "dansolchild"
$vnet = "westeur"
$subnet = "Clients"
$ipaddr = "192.168.0.41"

#This needs to be a new cloud service
$destcloudsvc = "danregsvcamsxio2"

#Use to DS Series VM
$newInstanceSize = "Standard_DS1"

#create new Availability Set
$availabilitySet = "cloudmigAVAMS3"

#Machine User Credentials
$userName = "myadmin"
$pass = "theM)stC0mplexP@ssw0rd!"


#Create VM Config
$vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

$vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
$vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



$vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet
```

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Her Zaman Kullanılabilirlik Grupları'nı kullanmayan varolan dağıtımlar

> [!NOTE]
> Varolan dağıtımlar için öncelikle bu makalenin [Önkoşullar](#prerequisites-for-premium-storage) bölümüne bakın.

Sql Server dağıtımları için Her Zaman Kullanılabilirlik Grupları'nı ve bunları kullanmayan farklı hususlar vardır. Always Açık'ı kullanmıyorsanız ve mevcut bağımsız bir SQL Server'A sahipseniz, yeni bir bulut hizmeti ve depolama hesabı kullanarak Premium Depolama'ya yükseltebilirsiniz. Aşağıdaki seçenekleri göz önünde bulundurun:

* **Yeni bir SQL Server VM oluşturun.** Yeni Dağıtımlarda belgelenmiş gibi Premium Depolama hesabı kullanan yeni bir SQL Server VM oluşturabilirsiniz. Ardından SQL Server yapılandırmanızı ve kullanıcı veritabanlarınızı yedekleyin ve geri yükleyin. Uygulama, dahili veya harici olarak erişiliyorsa, yeni SQL Server'a başvurmak için güncelleştirilmelidir. Yan yana (SxS) SQL Server geçişi yapıyormuş gibi tüm 'db' nesnelerini kopyalamanız gerekir. Buna oturum açma, sertifika ve bağlantılı sunucular gibi nesneler dahildir.
* **Varolan bir SQL Server VM'yi geçirin.** Bunun için SQL Server VM'yi çevrimdışı hale getirmek ve bağlı tüm VHD'lerini Premium Depolama hesabına kopyalamayı içeren yeni bir bulut hizmetine aktarmak için bu işlem gerektirir. VM çevrimiçi olduğunda, uygulama sunucu ana bilgisayar adına daha önce olduğu gibi başvurur. Varolan diskin boyutunun performans özelliklerini etkilediğini unutmayın. Örneğin, 400 GB'lık bir disk P20'ye yuvarlanır. Bu disk performansına gerek olmadığını biliyorsanız, VM'yi DS Serisi VM olarak yeniden oluşturabilir ve ihtiyacınız olan boyut/performans belirtiminin Premium Depolama VHD'lerini takabilirsiniz. Sonra SQL DB dosyalarını ayırıp yeniden takabilirsiniz.

> [!NOTE]
> VHD diskleri kopyalarken boyutu farkında olmalıdır, boyutuna bağlı olarak hangi Premium Depolama Disk türü içine düşmek anlamına gelir, Bu disk performans belirtimi belirler. Azure en yakın disk boyutuna yuvarlanır, böylece 400 GB diskinz varsa, bu bir P20'ye yuvarlanır. OS VHD'nin mevcut IO gereksinimlerinize bağlı olarak, bunu bir Premium Depolama hesabına geçirmeniz gerekmeyebilir.

SQL Server'ınıza dışarıdan erişilirse, bulut hizmeti VIP değişir. Ayrıca bitiş noktalarını, ALA'ları ve DNS ayarlarını da güncelleştirmeniz gerekir.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Her Zaman Kullanılabilirlik Grupları'nı kullanan varolan dağıtımlar

> [!NOTE]
> Varolan dağıtımlar için öncelikle bu makalenin [Önkoşullar](#prerequisites-for-premium-storage) bölümüne bakın.

Başlangıçta bu bölümde Her Zaman Açık'ın Azure Ağı ile nasıl etkileşimde olduğunu inceliyoruz. Daha sonra geçişleri iki senaryoya ayırıyoruz: bazı kapalı kalma sürelerinin tolere edilebildiği geçişler ve en az kapalı kalma süresini sağlamanız gereken geçişler.

Şirket içi SQL Server Always On Availability Groups, bir veya daha fazla SQL Sunucusu arasında paylaşılan bir IP adresiyle birlikte sanal bir DNS adını kaydeden bir Dinleyici şirket içinde kullanır. İstemciler bağlandığında dinleyici IP'si üzerinden Birincil SQL Server'a yönlendirilirler. Bu, o zaman Her Zaman IP kaynağının sahibi olan sunucudur.

![DeploymentsUseAlways A][6]

Microsoft Azure'da VM'de bir NIC'ye atanmış yalnızca bir IP adresi olabilir, bu nedenle Azure şirket içi olarak aynı soyutlama katmanına ulaşmak için Dahili/Harici Yük Dengeleyicileri'ne (ILB/ELB) atanan IP adresini kullanır. Sunucular arasında paylaşılan IP kaynağı, ILB/ELB ile aynı IP olarak ayarlanır. Bu, DNS'de yayımlanır ve istemci trafiği ILB/ELB'den Birincil SQL Server yinelemesine aktarılır. ILB/ELB, Her Zaman IP kaynağını araştırmak için problar kullandığından hangi SQL Server'ın birincil olduğunu bilir. Önceki örnekte, elb/ILB tarafından başvurulan bir bitiş noktası olan her düğümü inceler( hangisi Birincil SQL Server'dır) olur.

> [!NOTE]
> ILB ve ELB belirli bir Azure bulut hizmetine atanır, bu nedenle Azure'daki herhangi bir bulut geçişi büyük olasılıkla Yük Bakiyesi IP'sinin değiştiği anlamına gelir.
>
>

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Her Zaman Bazı kapalı kalma sürelerine izin verebilecek dağıtımlarda geçiş

Her Zaman Dağıtımlarda geçiş yapmak için bazı kapalı kalma sürelerine izin veren iki strateji vardır:

1. **Varolan her zaman kümeye daha fazla ikincil yineleme ekleme**
2. **Yeni Bir Kümeye Geçirin**

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a>1. Varolan Her Zaman Kümeye Daha Fazla İkincil Yineleme Ekleyin

Bir strateji Kullanılabilirlik Grubu'na daha fazla secondaries eklemektir. Bunları yeni bir bulut hizmetine eklemeniz ve dinleyiciyi yeni yük dengeleyicisi IP ile güncelleştirmeniz gerekir.

##### <a name="points-of-downtime"></a>Kesinti noktaları:

* Küme Doğrulama.
* Yeni İkinciller için Her Zaman Başarısızlık üzerine Test.

Daha yüksek IO iş için VM içindeki Windows Depolama Havuzları kullanıyorsanız, bunlar Tam Küme Doğrulama sırasında çevrimdışı duruma alınır. Kümeye düğüm eklediğinizde doğrulama sınaması gereklidir. Testi çalıştırmak için gereken süre değişebilir, bu nedenle bunun ne kadar süreceği hakkında yaklaşık bir süre elde etmek için bunu temsili test ortamınızda test etmeniz gerekir.

Beklendiği gibi Her Zaman Yüksek Kullanılabilirlik işlevlerini sağlamak için yeni eklenen düğümlerde manuel arıza ve kaos testi yapabileceğiniz bir süre sağlamalısınız.

![DeploymentUseAlways On2][7]

> [!NOTE]
> Doğrulama çalıştırmadan önce Depolama Havuzları'nın kullanıldığı tüm SQL Server örneklerini durdurmalısınız.
>
> ##### <a name="high-level-steps"></a>Üst düzey adımlar
>

1. Bağlı Premium Depolama ile yeni bulut hizmetinde iki yeni SQL Sunucusu oluşturun.
2. TAM yedeklemeleri kopyalayın ve NORECOVERY ile geri **yükleyin.**
3. Oturum açma lar vb. gibi 'kullanıcı DB' bağımlı nesnelerin üzerine kopyalayın.
4. Yeni bir Dahili Yük Dengeleyici (ILB) oluşturun veya harici yük dengeleyici (ELB) kullanın ve sonra her iki yeni düğümde de Yük Dengeli Uç Noktaları ayarlayın.

   > [!NOTE]
   > Devam etmeden önce tüm Düğümlerin doğru Bitiş Noktası yapılandırmasına sahip olup olmadığını kontrol edin
   >
   >
5. SQL Server'a Kullanıcı/Uygulama Erişimini Durdurun (Depolama Havuzları kullanıyorsanız).
6. Tüm Düğümlerde SQL Server Engine Services'ı durdurun (Depolama Havuzları kullanıyorsanız).
7. Kümelemek için yeni Düğümler ekleyin ve tam doğrulama çalıştırın.
8. Doğrulama başarılı olduktan sonra tüm SQL Server Hizmetlerini başlatın.
9. Yedekleme Hareketi günlükleri ve kullanıcı veritabanlarını geri yükleyin.
10. Her Zaman Kullanılabilirlik Grubuna yeni düğümler ekleyin ve **çoğaltmayı Synchronous'a**yerleştirin.
11. Yeni Bulut Hizmeti ILB/ELB'nin IP adresi kaynağını PowerShell üzerinden her zaman [Ek'teki](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage)Çok site örneğine göre ekleyin. Windows kümelemede, **IP Adresi** kaynağının **Olası sahiplerini** eski yeni düğümlere ayarlayın. [Ekin](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage)'Aynı Alt Net'te IP Adresi Kaynağı Ekleme' bölümüne bakın.
12. Yeni düğümlerden birine geçemez.
13. Yeni düğümleri Otomatik Failover Partners ve test failovers olun.
14. Kullanılabilirlik Grubundan orijinal düğümleri kaldırın.

##### <a name="advantages"></a>Yararları

* Yeni SQL Sunucuları Her Zaman Aasla'ya eklenmeden önce sınanabilir (SQL Server ve Application).
* VM boyutunu değiştirebilir ve depolama alanını tam gereksinimlerinize göre özelleştirebilirsiniz. Ancak, tüm SQL dosya yollarını aynı tutmak yararlı olacaktır.
* DB yedeklemelerinin İkincil Yinelemelere aktarılmasının ne zaman başlatılabileceğini denetleyebilirsiniz. Bu, VH'leri kopyalamak için Azure **Başlat-AzureStorageStorageBlobCopy** komut izini kullanmaktan farklıdır, çünkü bu bir eşzamanlı kopyadır.

##### <a name="disadvantages"></a>Dezavantajlar

* Windows Depolama Havuzları kullanırken, yeni ek düğümler için Tam Küme Doğrulama sırasında Küme kapalı kalma süresi vardır.
* SQL Server Sürümü'ne ve varolan ikincil yineleme sayısına bağlı olarak, varolan ikincil leri kaldırmadan daha fazla ikincil yineleme ekleyemeyebilirsiniz.
* İkincilleri kurarken uzun SQL veri aktarım süresi olabilir.
* Paralel çalışan yeni makineleriniz varken geçiş sırasında ek maliyet vardır.

#### <a name="2-migrate-to-a-new-always-on-cluster"></a>2. Yeni Bir Her Zaman Kümeye Geçirin

Başka bir strateji, yeni bulut hizmetinde yepyeni düğümlerle yepyeni bir Always On Cluster oluşturmak ve ardından istemcileri kullanmaya yönlendirmektir.

##### <a name="points-of-downtime"></a>Kapalı kalma süresi puanları

Uygulamaları ve kullanıcıları yeni Always On dinleyicisine aktardığınızda kapalı kalma süresi vardır. Kesinti süresi şuna bağlıdır:

* Son işlem günlüğü yedeklemelerini yeni sunucularda veritabanlarına geri yüklemek için gereken süre.
* Yeni Always On dinleyicisini kullanmak için istemci uygulamalarını güncelleştirmek için gereken süre.

##### <a name="advantages"></a>Yararları

* Gerçek üretim ortamını, SQL Server'ı ve işletim sistemi yapı değişikliklerini sınayabilirsiniz.
* Depolama alanını özelleştirme ve VM boyutunu küçültme seçeneğiniz olabilir. Bu maliyet azaltma neden olabilir.
* Bu işlem sırasında SQL Server yapınızı veya sürümünüzü güncelleştirebilirsiniz. İşletim Sistemini de yükseltebilirsiniz.
* Önceki Always On Cluster katı bir geri alma hedefi olarak hareket edebilir.

##### <a name="disadvantages"></a>Dezavantajlar

* Her ikisinin de Aynı anda çalışan kümelerde olmasını istiyorsanız dinleyicinin DNS adını değiştirmeniz gerekir. İstemci uygulama dizeleri yeni Dinleyici adını yansıtması gerektiğinden, bu durum geçiş sırasında yönetim ek yükü ekler.
* Geçişten önce son eşitleme gereksinimlerini en aza indirmek için iki ortam arasında mümkün olduğunca yakın tutmak için bir eşitleme mekanizması uygulamanız gerekir.
* Yeni ortam çalışırken geçiş sırasında ek maliyet vardır.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>En az kapalı kalma süresi için Her Zaman Dağıtımlarda Geçiş

En az kapalı kalma süresi için Her Zaman Dağıtımlarda geçiş için iki strateji vardır:

1. **Varolan Bir İkincil Kullanımı: Tek-Site**
2. **Mevcut İkincil Çoğaltma(lar) kullanımı: Çok Siteli**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. Mevcut bir ikincil kullanımı: Tek-Site

En az kapalı kalma süresi için bir strateji, varolan bir bulut ikincil almak ve geçerli bulut hizmetinden kaldırmaktır. Ardından VHD'leri yeni Premium Depolama hesabına kopyalayın ve yeni bulut hizmetinde VM'yi oluşturun. Daha sonra dinleyiciyi kümeleme ve başarısız olarak güncelleyin.

##### <a name="points-of-downtime"></a>Kapalı kalma süresi puanları

* Load Balanced bitiş noktasıyla son düğümü güncellediğinizde kapalı kalma süresi vardır.
* İstemcinizin yeniden bağlantısı, istemci/DNS yapılandırmanıza bağlı olarak gecikebilir.
* IP adreslerini değiştirmek için Her Zaman Açık Küme grubunu çevrimdışı almayı seçerseniz ek bir kapalı kalma süresi vardır. Eklenen IP Adresi kaynağı için BIR OR bağımlılığı ve Olası Sahipler kullanarak bunu önleyebilirsiniz. [Ekin](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage)'Aynı Alt Net'te IP Adresi Kaynağı Ekleme' bölümüne bakın.

> [!NOTE]
> Eklenen düğümün Her Zaman Failover İş Ortağı olarak yer almasını istediğinizde, Yük Dengeli Kümesi'ne atıfta bulunarak bir Azure Bitiş Noktası eklemeniz gerekir. Bunu yapmak için **AzureEndpoint Ekle** komutunu çalıştırdığınızda, geçerli bağlantılar açık kalır, ancak yük bakiyesi güncellenene kadar dinleyiciye yeni bağlantılar kurulamaz. Bu 90-120saniye son görüldü test ederken, bu test edilmelidir.

##### <a name="advantages"></a>Yararları

* Geçiş sırasında ek ücret alınmaz.
* Bire bir geçiş.
* Azaltılmış karmaşıklık.
* Premium Depolama SK'larından artan IOPS sağlar. Diskler VM'den ayrılıp yeni bulut hizmetine kopyalandığında, daha yüksek iş girişleri sağlayan VHD boyutunu artırmak için üçüncü taraf bir araç kullanılabilir. VHD boyutlarını artırmak için bu [forum tartışmasına](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows)bakın.

##### <a name="disadvantages"></a>Dezavantajlar

* Göç sırasında geçici ha ve DR kaybı vardır.
* Bu 1:1 geçiş olduğundan, VH'lerinizi destekleyen en az VM boyutu kullanmanız gerekir, böylece VM'lerinizi küçültemeyebilirsiniz.
* Bu senaryo, asynchronous olan Azure **Başlat-AzureStorageBlobCopy** komut unu kullanır. Kopyalama nın tamamlanmasında SLA yoktur. Kopyaların zamanı değişir, ancak bu sırada beklemeye bağlı olarak aktarılacağa bağlı dır. Aktarım başka bir bölgedeki Premium Depolama'yı destekleyen başka bir Azure veri merkezine gidiyorsa kopyalama süresi artar. Yalnızca 2 düğümünüz varsa, kopyanın test edilenden daha uzun sürmesi durumunda olası bir azaltma yı göz önünde bulundurun. Bu aşağıdaki fikirleri içerebilir.
  * Kararlaştırılan kapalı kalma süresiyle geçişten önce HA için geçici bir 3.
  * Geçişi Azure zamanlanmış bakımın dışında çalıştırın.
  * Küme çoğunluğunuzu doğru yapılandırdığınızdan emin olun.  

##### <a name="high-level-steps"></a>Üst düzey adımlar

Bu belge, sonuna kadar tam bir son örneği göstermez, ancak [Ek](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) bunu gerçekleştirmek için yararlanılabilen ayrıntıları sağlar.

![Minimal Kesinti Süresi][8]

* Disk yapılandırmasını toplayın ve düğümü kaldırın (ekli VHD'leri silmeyin).
* Premium Depolama hesabı oluşturun ve Standart Depolama hesabından VHD kopyalayın
* Yeni bulut hizmeti oluşturun ve SQL2 VM'yi bu bulut hizmetinde yeniden dağıtın. VM'yi, kopyalanan orijinal OS VHD'yi kullanarak ve kopyalanan VHD'leri takarak oluşturun.
* ILB / ELB'yi yapılandırın ve Uç Noktaları ekleyin.
* Ya tarafından Update Dinleyici:
  * Always On Group'u çevrimdışı nasihat etmek ve Yeni ILB / ELB IP adresiyle Always On Listener'ı güncellemek.
  * Veya PowerShell aracılığıyla yeni Cloud Service ILB/ELB IP adresi kaynağını Windows kümelemeye eklemek. Ardından IP Adresi kaynağının olası sahiplerini geçirilen düğüm, SQL2'ye ayarlayın ve bunu Ağ Adı'nda OR bağımlılığı olarak ayarlayın. [Ekin](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage)'Aynı Alt Net'te IP Adresi Kaynağı Ekleme' bölümüne bakın.
* Istemcilere DNS yapılandırmasını/yayılmasını kontrol edin.
* SQL1 VM'yi geçirin ve 2 -4 adımlarını geçirin.
* 5ii adımlarını kullanıyorsanız, eklenen IP Adresi Kaynağı için Olası Sahip olarak SQL1'i ekleyin
* Test başarısızları.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. Mevcut ikincil çoğaltma(lar): Çoklu Site

Birden fazla Azure veri merkezinde (DC) düğümler varsa veya karma bir ortamınız varsa, kapalı kalma süresini en aza indirmek için bu ortamda Her Zaman yapılandırmasını kullanabilirsiniz.

Yaklaşım, şirket içi veya ikincil Azure DC için Her Zaman Eşzamanlı eşitlemesi'ni Senkronize olarak değiştirmek ve ardından bu SQL Server'a geçebilmektir. Ardından VHD'leri Premium Depolama hesabına kopyalayın ve makineyi yeni bir bulut hizmetine yeniden dağıtın. Dinleyiciyi güncelleştirin ve sonra geri başarısız olur.

##### <a name="points-of-downtime"></a>Kapalı kalma süresi puanları

Kesinti süresi alternatif DC ve geri başarısız zaman oluşur. Ayrıca istemci /DNS yapılandırmabağlıdır ve istemci yeniden bağlantı gecikebilir.
Karma Her Zaman yapılandırması için aşağıdaki örneği göz önünde bulundurun:

![Çok Site1][9]

##### <a name="advantages"></a>Yararları

* Varolan altyapıyı kullanabilirsiniz.
* Önce DR Azure DC'deki Azure depolama alanını önceden yükseltme seçeneğiniz var.
* DR Azure DC depolama alanı yeniden yapılandırılabilir.
* Geçiş sırasında test başarısızlığı hariç olmak üzere en az iki başarısızlık vardır.
* SQL Server verilerini yedekleme ve geri yükleme ile taşımanız gerekmez.

##### <a name="disadvantages"></a>Dezavantajlar

* SQL Server istemci erişimine bağlı olarak, SQL Server uygulamaya alternatif bir DC'de çalışırken gecikme süresi artabilir.
* VHD'lerin Premium depolama alanına kopyalama süresi uzun olabilir. Bu, düğümü Kullanılabilirlik Grubunda tutup tutmamana ilişkin kararınızı etkileyebilir. Geçiş sırasında günlük yoğun iş yükleri çalışırken, Birincil düğüm kopyalanmamış hareketleri hareket günlüğünde tutmak zorunda olduğundan bunu göz önünde bulundurun. Bu nedenle bu önemli ölçüde büyüyebilir.
* Bu senaryo, asynchronous olan Azure **Başlat-AzureStorageBlobCopy** komut unu kullanır. Tamamlandığında SLA yok. Kopyaların zamanı değişir, ancak bu sırada beklemeye bağlıdır, ayrıca aktası veriş miktarına da bağlıdır. Bu nedenle, 2. Bu azaltma adımları aşağıdaki fikirleri içerir:
  * Kararlaştırılan kapalı kalma süresiyle geçişten önce HA için geçici bir 2.
  * Geçişi Azure zamanlanmış bakımın dışında çalıştırın.
  * Küme çoğunluğunuzu doğru yapılandırdığınızdan emin olun.

Bu senaryo, yüklemenizi belgelediğinizi ve en iyi disk önbelleği ayarları için değişiklik yapmak için depolama alanının nasıl eşlendiğini bildiğinizi varsayar.

##### <a name="high-level-steps"></a>Üst düzey adımlar

![Çok Site2][10]

* Şirket içi / alternatif Azure DC'yi SQL Server Primary yapın ve diğer Otomatik Failover İş Ortağı (AFP) yapın.
* DISK yapılandırma bilgilerini SQL2'den toplayın ve düğümü kaldırın (ekli VHD'leri silmeyin).
* Bir Premium Depolama hesabı oluşturun ve Standart Depolama hesabından VHD kopyalayın.
* Yeni bir bulut hizmeti oluşturun ve Premiums Depolama diskleri ekli SQL2 VM oluşturun.
* ILB / ELB'yi yapılandırın ve Uç Noktaları ekleyin.
* Yeni ILB / ELB IP adresi ve test failover ile Her Zaman Dinleyici Güncelleyin.
* DNS yapılandırmasını kontrol edin.
* AFP'yi SQL2 olarak değiştirin ve ardından SQL1'i geçirin ve 2 -5 adımlarını gözden geçirin.
* Test başarısızları.
* AFP'yi SQL1 ve SQL2'ye geri çevirin

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a>Ek: Bir Çok Siteyi Her Zaman Kümede Premium Depolamaya Geçirme

Bu makalenin geri kalanı, çok siteli Her Zaman Küme'yi Premium depolama alanına dönüştürmenin ayrıntılı bir örneğini sağlar. Ayrıca Dinleyici'yi harici yük dengeleyicisi (ELB) kullanmaktan dahili yük dengeleyicisine (ILB) dönüştürür.

### <a name="environment"></a>Ortam

* Windows 2k12 / SQL 2k12
* SP'de 1 DB Dosyası
* Düğüm başına 2 x Depolama Havuzları

![Ek1][11]

### <a name="vm"></a>Vm:

Bu örnekte, bir ELB'den ILB'ye geçmemizi göstereceğiz. ELB ILB'den önce mevcuttu, bu da geçiş sırasında ILB'ye nasıl geçilen gösterir.

![Ek2][12]

### <a name="pre-steps-connect-to-subscription"></a>Ön Adımlar: Aboneye Bağlanma

```powershell
Add-AzureAccount

#Set up subscription
Get-AzureSubscription
```

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>Adım 1: Yeni Depolama Hesabı ve Bulut Hizmeti Oluşturma

```powershell
$mysubscription = "DansSubscription"
$location = "West Europe"

#Storage accounts
#current storage account where the vm to migrate resides
$origstorageaccountname = "danstdams"

#Create Premium Storage account
$newxiostorageaccountname = "danspremsams"
New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

#Generate storage keys for later
$originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
$xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

#Generate storage acc contexts
$origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
$xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#Set up subscription and default storage account
Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
Select-AzureSubscription -SubscriptionName $mysubscription -Current

#CREATE NEW CLOUD SVC
$vnet = "dansvnetwesteur"

##Existing cloud service
$sourceSvc="dansolSrcAms"

##Create new cloud service
$destcloudsvc = "danNewSvcAms"
New-AzureService $destcloudsvc -Location $location
```

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a>Adım 2: Kaynaklarda izin verilen \<hataları artırın İsteğe Bağlı>

Her Zaman Kullanılabilirlik Grubu'na ait belirli kaynaklarda, küme hizmetinin kaynak grubunu yeniden başlatmaya çalıştığı bir dönemde kaç hata oluşabileceği konusunda sınırlar vardır. Bu yordamı uygularken bunu artırmanız önerilir, çünkü makineleri kapatarak manuel olarak başarısız olmaz sanız ve arızaları tetiklerseniz bu sınıra yaklaşabilirsiniz.

Failover Cluster Manager'da bunu yapmak için hata ödeneğini ikikatına çıkarmak, Her Zaman Açık kaynak grubunun özelliklerine gitmek akıllıca olacaktır:

![Ek3][13]

Maksimum Hataları 6 olarak değiştirin.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>Adım 3: Küme Grubu \<İsteğe Bağlı> için IP Adresi kaynağı ekleme

Küme Grubu için yalnızca bir IP adresiniz varsa ve bu bulut alt ağına hizalanmışsa, dikkat edin, bu ağdaki buluttaki tüm küme düğümlerini yanlışlıkla çevrimdışına alırsanız, Küme IP kaynağı ve Küme Ağı Adı çevrimiçi olamaz. Bu durumda, diğer küme kaynaklarına güncelleştirmeleri engeller.

#### <a name="step-4-dns-configuration"></a>Adım 4: DNS yapılandırması

Sorunsuz bir geçiş uygulamak, DNS'nin nasıl kullanıldığına ve güncelleştirildiğine bağlıdır.
Her Zaman Açık yüklendiğinde, bir Windows Küme Kaynak grubu oluşturur, Failover Cluster Manager'ı açarsanız, en az üç kaynağı olduğunu görürsünüz, belgenin atıfta bulunduğu iki kaynak şunlardır:

* Sanal Ağ Adı (VNN) – Istemcilerin Sql Sunucularına Her Zaman Aracılığıyla bağlanmak istediklerinde bağlandığı DNS adı.
* IP Adresi Kaynağı – VNN ile ilişkili IP adresi, birden fazla olabilir ve çok siteli yapılandırmada site/alt ağ başına bir IP adresiniz vardır.

SQL Server'a bağlanırken, SQL Server Client sürücüsü dinleyiciyle ilişkili DNS kayıtlarını alır ve her Bir İlişkili IP adresine bağlanmaya çalışır. Daha sonra, bunu etkileyebilecek bazı faktörleri tartışıyoruz.

Dinleyici adı ile ilişkili eşzamanlı DNS kayıtlarının sayısı yalnızca ilişkili IP adreslerinin sayısına değil, Her Zaman VNN kaynağı için Failover Kümeleme'deki 'RegisterAllIpProviders' ayarına bağlıdır.

Azure'da Her Zaman Açık'ı dağıttığınızda Dinleyici ve IP Adreslerini oluşturmak için farklı adımlar vardır, 'RegisterAllIpProviders'ı el ile 1 olarak yapılandırmanız gerekir, bu durum, zaten 1 olarak ayarlanmış olan her zaman dağıtımda olan şirket içi bir işlemden farklıdır.

'RegisterAllIpProviders' 0 ise, Dinleyici ile ilişkili DNS'de yalnızca bir DNS kaydı görürsünüz:

![Ek4][14]

'RegisterAllIpProviders' ise 1:

![Ek5][15]

Aşağıdaki kod VNN ayarlarını boşaltıyor ve sizin için ayarlar. Değişikliğin etkili olması için VNN'yi çevrimdışı duruma alıp çevrimiçi olarak geri döndürmeniz gerekir. Bu, Dinleyici'yi çevrimdışı na alır ve istemci bağlantısının bozulmasına neden olur.

```powershell
##Always On Listener Name
$ListenerName = "Mylistener"
##Get AlwaysOn Network Name Settings
Get-ClusterResource $ListenerName| Get-ClusterParameter
##Set RegisterAllProvidersIP
Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1
```

Daha sonraki bir geçiş adımında, her zaman dinleyiciyi yük dengeleyicisine başvuran güncelleştirilmiş bir IP adresiyle güncelleştirmeniz gerekir, bu bir IP Adresi kaynak kaldırma ve ekleme içerir. IP güncellemeden sonra, yeni IP adresinin DNS Bölgesi'nde güncelleştirildiğini ve istemcilerin yerel DNS önbelleğini güncelleştirdiğinden emin olmanız gerekir.

Müşterileriniz farklı bir ağ segmentinde yaşıyorsa ve farklı bir DNS sunucusuna başvuruyorsa, uygulama yeniden bağlanma süresi en azından yeni bir IP'nin Bölge Aktarım Süresi ile sınırlandırılmıştırğından, geçiş sırasında DNS Bölge Aktarımı hakkında ne olduğunu göz önünde bulundurmanız gerekir dinleyici için adresleri. Burada zaman kısıtlaması altındaysanız, Windows ekiplerinizle artımlı bölge aktarımını zorlamayı tartışmalı ve test etmeli ve Ayrıca DNS ana bilgisayar kaydını daha düşük bir Zaman Yaşamak (TTL) olarak koymalısınız, böylece istemciler güncelleştirilmelidir. Daha fazla bilgi için bkz: [Artımlı Bölge Aktarımları](https://technet.microsoft.com/library/cc958973.aspx) ve [Start-DnsServerZoneTransfer.](https://docs.microsoft.com/powershell/module/dnsserver/start-dnsserverzonetransfer)

Varsayılan olarak, Azure'da Her Zaman Üzerinde Dinleyici ile ilişkili DNS Kaydı için TTL 1200 saniyedir. Geçiş sırasında zaman kısıtlaması altındaysanız, istemcilerin DNS'lerini dinleyicinin güncelleştirilmiş IP adresiyle güncelleştirdiğinden emin olmak için bunu azaltmak isteyebilirsiniz. VNN yapılandırmasını boşaltarak yapılandırmayı görebilir ve değiştirebilirsiniz:

```powershell
$AGName = "myProductionAG"
$ListenerName = "Mylistener"
#Look at HostRecordTTL
Get-ClusterResource $ListenerName| Get-ClusterParameter

#Set HostRecordTTL Examples
Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120
```

> [!NOTE]
> 'HostRecordTTL' ne kadar düşükse, daha yüksek miktarda DNS trafiği oluşur.

##### <a name="client-application-settings"></a>İstemci uygulama ayarları

SQL istemci uygulamanız .NET 4.5 SQLClient'ı destekliyorsa, 'MULTISUBNETFAILOVER=TRUE' anahtar sözcük lerini kullanabilirsiniz. Bu anahtar kelime, başarısız lık sırasında SQL Always On Availability Group'a daha hızlı bağlantı sağlar. Always On dinleyicisi ile ilişkili tüm IP adreslerini paralel olarak oyuvarlar ve bir hata sırasında daha agresif bir TCP bağlantı yeniden deneme hızı gerçekleştirir.

Önceki ayarlar hakkında daha fazla bilgi [için, bkz.](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover) Ayrıca yüksek [kullanılabilirlik, Olağanüstü Durum Kurtarma için SqlClient Desteği'ne](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx)bakın.

#### <a name="step-5-cluster-quorum-settings"></a>Adım 5: Küme çoğunluk ayarları

Aynı anda en az bir SQL Server'ı devre dışı bırakacağınız için, dosya paylaşım tanığı (FSW) kullanıyorsanız, düğüm çoğunluğuna izin verecek ve dinamik oylamadan yararlanmak için yeterliliği ayarlamanız gerekir , tek bir düğümün ayakta kalmasını sağlar.

```powershell
Set-ClusterQuorum -NodeMajority  
```

Küme çoğunluğunun yönetimi ve yapılandırılması hakkında daha fazla bilgi için [bkz.](https://technet.microsoft.com/library/jj612870.aspx)

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>Adım 6: Varolan Uç Noktaları ve ALA'ları Ayıkla

```powershell
#GET Endpoint info
Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
#GET ACL Rules for Each EP, this example is for the Always On Endpoint
Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  
```

Bu metni bir dosyaya kaydedin.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>Adım 7: Failover Ortaklarını ve Çoğaltma Modlarını Değiştir

İkiden fazla SQL Sunucunuz varsa, başka bir DC'deki veya şirket içi başka bir ikincil sunucunun hatasını 'Synchronous' olarak değiştirmeli ve otomatik failover partneri (AFP) yapmalısınız, bu nedenle değişiklik yaparken HA'yı korumanız gerekir. Bunu TSQL üzerinden yapabilirsiniz ama SSMS değiştirin:

![Ek6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>Adım 8: İkincil VM'yi bulut hizmetinden kaldırma

Önce bir bulut ikincil düğümü geçirmek için planlama olmalıdır. Bu düğüm birincil ise, bir el ile başarısız başlatmanız gerekir.

```powershell
$vmNameToMigrate="dansqlams2"

#Check machine status
Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#Shutdown secondary VM
Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


#Extract disk configuration

##Building Existing Data Disk Configuration
$file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
$datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
Add-Content $file "lun, vhdname, hostcaching, disklabel, diskName"
foreach ($disk in $datadisks)
{
    $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
    $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
}

#Get OS Disk
$osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
$osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
$osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
$addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
$addosdisk | add-content -path $file

#Import disk config
$diskobjects  = Import-CSV $file

#Check disk config, make sure below returns the disks associated with the VM
$diskobjects

#Identify OS Disk
$osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
$osdiskforbuild = $osdiskimport.diskName

#Check machibe is off
Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

#Drop machine and rebuild to new cls
Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate
```

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>Adım 9: CSV dosyasında disk önbelleğe alma ayarlarını değiştirin ve kaydedin

Veri birimleri için bunlar READONLY olarak ayarlanmalıdır.

TLOG birimleri için bunlar NONE olarak ayarlanmalıdır.

![Ek7][17]

#### <a name="step-10-copy-vhds"></a>Adım 10: Kopya VHDS

```powershell
#Ensure you have created the container for these:
$containerName = 'vhds'

#Create container
New-AzureStorageContainer -Name $containerName -Context $xioContext

####DISK COPYING####
#Get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
ForEach ($disk in $diskobjects)
{
   $lun = $disk.Lun
   $vhdname = $disk.vhdname
   $cacheoption = $disk.HostCaching
   $disklabel = $disk.DiskLabel
   $diskName = $disk.DiskName
   Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

   #Start async copy
   Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
}
```


VHD'lerin kopya durumunu Premium Depolama hesabına kontrol edebilirsiniz:

```powershell
ForEach ($disk in $diskobjects)
{
   $lun = $disk.Lun
   $vhdname = $disk.vhdname
   $cacheoption = $disk.HostCaching
   $disklabel = $disk.DiskLabel
   $diskName = $disk.DiskName

   $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
   Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
}
```

![Ek 8][18]

Tüm bunlar başarı olarak kaydedilene kadar bekleyin.

Tek tek lekeler için bilgi için:

```powershell
Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext
```

#### <a name="step-11-register-os-disk"></a>Adım 11: Os diskini kaydet

```powershell
#Change storage account
Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
Select-AzureSubscription -SubscriptionName $mysubscription -Current

#Register OS disk
$osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
$osvhd = $osdiskimport.vhdname
$osdiskforbuild = $osdiskimport.diskName

#Registering OS disk, but as XIO disk
$xioDiskName = $osdiskforbuild + "xio"
Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"
```

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>Adım 12: İkincil leri yeni bulut hizmetine aktarma

Aşağıdaki kod da makine yi içe aktarabilir ve istinat edilebilir VIP kullanabilirsiniz burada eklenen seçeneği kullanır.

```powershell
#Build VM Config
$ipaddr = "192.168.0.5"
#Remember to change to XIO
$newInstanceSize = "Standard_DS13"
$subnet = "SQL"

#Create new Availability Set
$availabilitySet = "cloudmigAVAMS"

#build machine config into object
$vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

#Reload disk config
$diskobjects  = Import-CSV $file
$datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

ForEach ( $attachdatadisk in $datadiskimport)
{
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks to a VM during a deploy to a new cloud service and new storage account is different from just attaching VHDs to just a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

}

#Create VM
$vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)
```

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>Adım 13: Yeni Bulut Svc'de ILB Oluşturma, Yük Dengeli Uç Noktaları ve ALAK'lar Ekleme

```powershell
#Check for existing ILB
GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

$ilb="sqlIntIlbDest"
$subnet = "SQL"
$IP="192.168.0.25"
Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

#Endpoints
$epname="sqlIntEP"
$prot="tcp"
$locport=1433
$pubport=1433
Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

#SET Azure ACLs or Network Security Groups & Windows FWs

#https://msdn.microsoft.com/library/azure/dn495192.aspx

####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####
```

#### <a name="step-14-update-always-on"></a>Adım 14: Her Zaman Güncelleştir

```powershell
#Code to be executed on a Cluster Node
$ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
$newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

$AGName = "myProductionAG"
$ListenerName = "Mylistener"


Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

#set dependency and NETBIOS, then remove old IP address

#set NETBIOS, then remove old IP address
Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

#set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

#Make sure no static records in DNS
```

![Ek9][19]

Şimdi eski bulut hizmeti IP Adresini kaldırın.

![Ek 10][20]

#### <a name="step-15-dns-update-check"></a>Adım 15: DNS güncelleştirme denetimi

Şimdi SQL Server istemci ağlarınızdaki DNS Sunucularını kontrol etmeli ve kümelemenin eklenen IP adresi için ek ana bilgisayar kaydı nı eklemiştir. Bu DNS sunucuları güncelleştirilmediyse, bir DNS Bölgesi aktarımını zorlamayı düşünün ve alt ağdaki istemcilerin her ikisi nin de IP Adreslerinde Olmasını çözebilmesini sağlayın, bu nedenle otomatik DNS çoğaltmasını beklemenize gerek yoktur.

#### <a name="step-16-reconfigure-always-on"></a>Adım 16: Her Zaman Yeniden Yapılandır

Bu noktada, şirket içi düğümle tam olarak yeniden eşitlemek ve eşzamanlı çoğaltma düğümüne geçmek ve afp yapmak için geçirilen ikincil düğümü beklersiniz.  

#### <a name="step-17-migrate-second-node"></a>Adım 17: İkinci düğümü geçirin

```powershell
$vmNameToMigrate="dansqlams1"

Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#Get endpoint information
$endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

#Shutdown VM
Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

#Get disk config

#Building Existing Data Disk Configuration
$file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
$datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
Add-Content $file "lun, vhdname, hostcaching, disklabel, diskName"
foreach ($disk in $datadisks)
{
    $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
    $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
}

#Get OS Disk
$osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
$osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
$osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
$addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
$addosdisk | add-content -path $file

#Import disk config
$diskobjects  = Import-CSV $file

#Check disk configuration
$diskobjects

#Identify OS Disk
$osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
$osdiskforbuild = $osdiskimport.diskName

#Check machine is off
Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

#Drop machine and rebuild to new cls
Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate
```

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Adım 18: CSV dosyasında disk önbelleğe alma ayarlarını değiştirin ve kaydedin

Veri birimleri için önbellek ayarları READONLY olarak ayarlanmalıdır.

TLOG birimleri için önbellek ayarları NONE olarak ayarlanmalıdır.

![Ek 11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>Adım 19: İkincil Düğüm için Yeni Bağımsız Depolama Hesabı Oluşturma

```powershell
$newxiostorageaccountnamenode2 = "danspremsams2"
New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

#Reset the storage account src if node 1 in a different storage account
$origstorageaccountname2nd = "danstdams2"

#Generate storage keys for later
$xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

#Generate storage acc contexts
$xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

#Set up subscription and default storage account
Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
Select-AzureSubscription -SubscriptionName $mysubscription -Current
```

#### <a name="step-20-copy-vhds"></a>Adım 20: Kopya VHDS

```powershell
#Ensure you have created the container for these:
$containerName = 'vhds'

#Create container
New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

####DISK COPYING####
##get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
ForEach ($disk in $diskobjects)
{
   $lun = $disk.Lun
   $vhdname = $disk.vhdname
   $cacheoption = $disk.HostCaching
   $disklabel = $disk.DiskLabel
   $diskName = $disk.DiskName
   Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

   #Start async copy
   Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContextnode2
}

#Check for copy progress

#check individual blob status
Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext
```

Tüm VHD'ler için VHD kopya durumunu kontrol edebilirsiniz:

```powershell
ForEach ($disk in $diskobjects)
{
    $lun = $disk.Lun
    $vhdname = $disk.vhdname
    $cacheoption = $disk.HostCaching
    $disklabel = $disk.DiskLabel
    $diskName = $disk.DiskName

    $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
}
```

![Ek 12][22]

Tüm bunlar başarı olarak kaydedilene kadar bekleyin.

Tek tek lekeler için bilgi için:

```powershell
#Check individual blob status
Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContextnode2
```

#### <a name="step-21-register-os-disk"></a>Adım 21: Os diskini kaydet

```powershell
#change storage account to the new XIO storage account
Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
Select-AzureSubscription -SubscriptionName $mysubscription -Current

#Register OS disk
$osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
$osvhd = $osdiskimport.vhdname
$osdiskforbuild = $osdiskimport.diskName

#Registering OS disk, but as XIO disk
$xioDiskName = $osdiskforbuild + "xio"
Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#Build VM Config
$ipaddr = "192.168.0.4"
$newInstanceSize = "Standard_DS13"

#Join to existing Availability Set

#Build machine config into object
$vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

#Reload disk config
$diskobjects  = Import-CSV $file
$datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

ForEach ( $attachdatadisk in $datadiskimport)
{
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different to just a straight cloud service change
    #note if you do not have a disk label the command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

}

#Create VM
$vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose
```

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>Adım 22: Yük Dengeli Uç Noktaları ve ALA'lar Ekle

```powershell
#Endpoints
$epname="sqlIntEP"
$prot="tcp"
$locport=1433
$pubport=1433
Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


#STOP!!! CHECK in the Azure portal or Machine Endpoints through PowerShell that these Endpoints are created!

#SET ACLs or Azure Network Security Groups & Windows FWs

#https://msdn.microsoft.com/library/azure/dn495192.aspx
```

#### <a name="step-23-test-failover"></a>Adım 23: Test başarısız

Geçirilen düğümün her zaman düğümüzerinde şirket içi ile eşitlemesini bekleyin. Senkronize çoğaltma moduna yerleştirin ve senkronize olana kadar bekleyin. Sonra bina içinde ilk düğüm, AFP göç başarısız. Bu işe yaradıktan sonra, AFP'ye en son geçirilen düğümü değiştirin.

Tüm düğümler arasındaki başarısızlıkları test etmeli ve başarısızların beklendiği gibi ve zamanında bir malikanede çalışmasını sağlamak için kaos testleri ne de olsa çalıştırmalısınız.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>Adım 24: Küme çoğunluk ayarlarını geri getirin / DNS TTL / Failover Pntrs / Sync Ayarları

##### <a name="adding-ip-address-resource-on-same-subnet"></a>Aynı Alt Ağa IP Adresi Kaynağı Ekleme

Yalnızca iki SQL Sunucunuz varsa ve bunları yeni bir bulut hizmetine geçirmek istiyorsanız, ancak bunları aynı alt ağda tutmak istiyorsanız, orijinal Her Zaman IP Adresini silmek ve Yeni IP Adresini eklemek için dinleyiciyi çevrimdışı kullanmaktan kaçınabilirsiniz. VM'leri başka bir alt ağa geçirtiyorsanız, bu alt ağa başvuran ek bir küme ağı olduğundan bunu yapmanız gerekmez.

Geçirilen ikincil bilgileri gündeme getirdikten ve varolan Birincil'i aşmadan önce yeni bulut hizmeti için yeni IP Adresi kaynağına ekledikten sonra, Cluster Failover Manager içinde aşağıdaki adımları atmalısınız:

IP Adresi eklemek için Ek 14'e bakın.

1. Geçerli IP Adresi kaynağı için, olası sahibini 'dansqlams4' örneğinde 'Varolan Birincil SQL Server' olarak değiştirin:

    ![Ek 13][23]
2. Yeni IP Adresi kaynağı için, olası sahibi 'geçirilen ikincil SQL Server' olarak değiştirin, örnekte, 'dansqlams5':

    ![Ek 14][24]
3. Bu ayarlandıktan sonra başarısız olabilirsiniz ve son düğüm geçirildiğinde Olası Sahipler düzenlenmelidir, böylece düğüm Olası Sahibi olarak eklenir:

    ![Ek 15][25]

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Premium Depolama](../disks-types.md)
* [Virtual Machines](https://azure.microsoft.com/services/virtual-machines/)
* [Azure Sanal Makinelerde SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
