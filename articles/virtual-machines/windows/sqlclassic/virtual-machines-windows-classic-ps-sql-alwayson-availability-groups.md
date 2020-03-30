---
title: PowerShell'i kullanarak Azure VM'deki Her Zaman Kullanılabilirlik grubunu yapılandırın | Microsoft Dokümanlar
description: Bu öğretici, klasik dağıtım modeliyle oluşturulan kaynakları kullanır. Azure'da Her Zaman Açık kullanılabilirlik grubu oluşturmak için PowerShell'i kullanırsınız.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a4e2f175-fe56-4218-86c7-a43fb916cc64
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: ba6f1300353247ef2de99b2bd903bc82665d9a52
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75978152"
---
# <a name="configure-the-always-on-availability-group-on-an-azure-vm-with-powershell"></a>PowerShell ile Bir Azure VM'de Her Zaman Kullanılabilirlik grubunu yapılandırın
> [!div class="op_single_selector"]
> * [Klasik: UI](../classic/portal-sql-alwayson-availability-groups.md)
> * [Klasik: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
<br/>

Başlamadan önce, azure kaynak yöneticisi modelinde artık bu görevi tamamlayabileceğinizi düşünün. Yeni dağıtımlar için Azure kaynak yöneticisi modelini öneririz. Sql [Server Her Zaman Azure sanal makinelerinde kullanılabilirlik gruplarına](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md)bakın.

> [!IMPORTANT]
> Yeni dağıtımların çoğunun Kaynak Yöneticisi modelini kullanmasını öneririz. Azure, kaynak oluşturmak ve bu kaynaklarla çalışmak için iki dağıtım modeli kullanır: [Resource Manager ve klasik](../../../azure-resource-manager/management/deployment-models.md). Bu makale klasik dağıtım modelini incelemektedir.

Azure sanal makineleri (VM'ler), veritabanı yöneticilerinin yüksek kullanılabilirlikte olan bir SQL Server sisteminin maliyetini düşürmesine yardımcı olabilir. Bu öğretici, bir Azure ortamında SQL Server Always Her Zaman Uçuçta kullanarak bir kullanılabilirlik grubunu nasıl uygulayacağınızı gösterir. Öğreticinin sonunda, Azure'daki SQL Server Always On çözümünüz aşağıdaki öğelerden oluşur:

* Ön uç ve arka uç alt ağı da dahil olmak üzere birden çok alt ağ içeren sanal ağ.
* Etkin Dizin etki alanına sahip bir etki alanı denetleyicisi.
* Arka uç alt ağına dağıtılan ve Active Directory etki alanına katılan iki SQL Server VM..
* Düğüm Çoğunluk çoğunluk modeliile üç düğümlü Windows failover kümesi.
* Kullanılabilirlik veritabanının iki eşzamanlı yinelemesini içeren kullanılabilirlik grubu.

Bu senaryo, azure'daki basitliği için iyi bir seçimdir, maliyet etkinliği veya diğer etkenler için değil. Örneğin, iki düğümlü bir failover kümesinde yeterlik dosya paylaşım tanığı olarak etki alanı denetleyicisini kullanarak Azure'da işlem saatlerinde kaydetmek için iki yinelemeli kullanılabilirlik grubunun VM sayısını en aza indirebilirsiniz. Bu yöntem, vm sayısını yukarıdaki yapılandırmadan bir oranında azaltır.

Bu öğretici, her adımın ayrıntılarını ayrıntıya girmeden, yukarıda açıklanan çözümü ayarlamak için gereken adımları size göstermek için tasarlanmıştır. Bu nedenle, GUI yapılandırma adımlarını sağlamak yerine, her adımda hızlı bir şekilde sizi götürmek için PowerShell komut dosyası kullanır. Bu öğretici aşağıdakileri varsayar:

* Sanal makine aboneliğine sahip bir Azure hesabınız zaten var.
* [Azure PowerShell cmdlets'i](/powershell/azure/overview)yükledin.
* Şirket içi çözümler için Her Zaman Kullanılabilirlik Grupları hakkında zaten sağlam bir anlayışa sahipsiniz. Daha fazla bilgi için her [zaman kullanılabilirlik gruplarına (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx)bakın.

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a>Azure aboneliğinize bağlanın ve sanal ağı oluşturun
1. Yerel bilgisayarınızdaki Bir PowerShell penceresinde, Azure modülünü içe aktarın, yayımlama ayarları dosyasını makinenize indirin ve indirilen yayımlama ayarlarını içe aktararak PowerShell oturumunuzu Azure aboneliğinize bağlayın.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    **Azure'u AlPublishSettingsFile** komutu otomatik olarak Azure ile bir yönetim sertifikası oluşturur ve makinenize indirir. Bir tarayıcı otomatik olarak açılır ve Azure aboneliğiniz için Microsoft hesap kimlik bilgilerini girmeniz istenir. İndirilen **.publishsettings** dosyası, Azure aboneliğinizi yönetmek için gereken tüm bilgileri içerir. Bu dosyayı yerel bir dizine kaydettikten **sonra, Alma-AzurePublishSettingsFile** komutunu kullanarak dosyayı içe aktarın.

   > [!NOTE]
   > .publishsettings dosyası, Azure aboneliklerinizi ve hizmetlerinizi yönetmek için kullanılan kimlik bilgilerinizi (kodlanmamış) içerir. Bu dosya için en iyi güvenlik uygulaması, dosyayı kaynak dizinlerinizin dışında geçici olarak depolamaktır (örneğin, Kitaplıklar\Belgeler klasöründe) ve alma işlemi tamamlandıktan sonra silmektir. .publishsettings dosyasına erişim elde eden kötü niyetli bir kullanıcı Azure hizmetlerinizi düzenleyebilir, oluşturabilir ve silebilir.

2. Bulut BT altyapınızı oluşturmak için kullanacağınız bir dizi değişken tanımlayın.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Komutlarınızın daha sonra başarılı olduğundan emin olmak için aşağıdakilere dikkat edin:

   * $storageAccountName **ve** **$dcServiceName** değişkenler benzersiz olmalıdır, çünkü bunlar internette sırasıyla bulut depolama hesabınızı ve bulut sunucunuzu tanımlamak için kullanılır.
   * **değişkenler $affinityGroupName** ve **$virtualNetworkName** için belirttiğiniz adlar, daha sonra kullanacağınız sanal ağ yapılandırma belgesinde yapılandırılır.
   * **$sqlImageName,** SQL Server 2012 Service Pack 1 Enterprise Edition içeren VM görüntüsünün güncelleştirilmiş adını belirtir.
   * Basitlik için, **Contoso!000** tüm öğretici boyunca kullanılan aynı şifredir.

3. Bir yakınlık grubu oluşturun.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

4. Yapılandırma dosyalarını içe aktararak sanal ağ oluşturun.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    Yapılandırma dosyası aşağıdaki XML belgesini içerir. Kısacası, **contosoAG**adlı yakınlık grubunda **ContosoNET** adlı bir sanal ağ belirtir. Adres alanı **10.10.0.0/16** ve iki alt netleri vardır, **10.10.1.0/24** ve **10.10.2.0/24**, sırasıyla ön alt ve arka alt ağ vardır. Ön alt ağ, Microsoft SharePoint gibi istemci uygulamalarını yerleştirebileceğiniz yerdir. Arka alt ağ, SQL Server VM'leri yerleştireceğiniz yerdir. **değişkenleri daha** önce $affinityGroupName ve **$virtualNetworkName** değiştirirseniz, aşağıdaki karşılık gelen adları da değiştirmeniz gerekir.

        <NetworkConfiguration xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

5. Oluşturduğunuz yakınlık grubuyla ilişkili bir depolama hesabı oluşturun ve aboneliğinizdeki geçerli depolama hesabı olarak ayarlayın.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

6. Yeni bulut hizmeti ve kullanılabilirlik kümesinde etki alanı denetleyici sabunu oluşturun.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Bu borulu komutlar aşağıdaki şeyleri yapar:

   * **Yeni AzureVMConfig** bir VM yapılandırması oluşturur.
   * **Add-AzureProvisioningConfig,** bağımsız bir Windows sunucusunun yapılandırma parametrelerini verir.
   * **Add-AzureDataDisk,** Active Directory verilerini depolamak için kullanacağınız veri diskini önbelleğe alma seçeneği yle Birlikte Yok olarak ayarlar.
   * **Yeni AzureVM** yeni bir bulut hizmeti oluşturur ve yeni bulut hizmetinde yeni Azure VM'yi oluşturur.

7. Yeni VM'nin tam olarak sağlanmasını bekleyin ve uzak masaüstü dosyasını çalışma dizininize indirin. Yeni Azure VM'nin sağlanması uzun zaman `while` aldığından, döngü kullanıma hazır olana kadar yeni VM'yi yoklamaya devam eder.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

Etki alanı denetleyicisi sunucusu artık başarıyla sağlanmış. Ardından, bu etki alanı denetleyicisi sunucusunda Active Directory etki alanını yapılandıracaksınız. PowerShell penceresini yerel bilgisayarınızda açık bırakın. İki SQL Server VM'yi oluşturmak için daha sonra tekrar kullanırsınız.

## <a name="configure-the-domain-controller"></a>Etki alanı denetleyicisini yapılandırma
1. Uzak masaüstü dosyasını başlatarak etki alanı denetleyici sunucusuna bağlanın. Makine yöneticisinin yeni VM'yi oluşturduğunuzda belirttiğiniz Kullanıcı Adı AzureAdmin'i ve **Parola Contoso!000'i**kullanın.
2. Yönetici modunda bir PowerShell penceresi açın.
3. Aşağıdaki **DCPROMO'yu çalıştırın. EXE** **komutu,** sürücü M'deki veri dizinleri ile corp.contoso.com etki alanını kurmak için kullanılır.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Komut bittikten sonra VM otomatik olarak yeniden başlatılır.

4. Uzak masaüstü dosyasını başlatarak etki alanı denetleyici sunucusuna yeniden bağlanın. Bu kez, **CORP\Administrator**olarak oturum açın.
5. Yönetici modunda bir PowerShell penceresi açın ve aşağıdaki komutu kullanarak Active Directory PowerShell modüllerini içe aktarın:

        Import-Module ActiveDirectory

6. Etki alanına üç kullanıcı eklemek için aşağıdaki komutları çalıştırın.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    **CORP\Install,** SQL Server hizmet örnekleri, başarısız küme ve kullanılabilirlik grubuyla ilgili her şeyi yapılandırmak için kullanılır. **CORP\SQLSvc1** ve **CORP\SQLSvc2** iki SQL Server VM'nin SQL Server hizmet hesapları olarak kullanılır.
7. Ardından, **CORP\Install** izinlerini etki alanında bilgisayar nesneleri oluşturmak için vermek için aşağıdaki komutları çalıştırın.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    Yukarıda belirtilen GUID, bilgisayar nesnesi türü için GUID'dir. **CORP\Install** hesabı, failover kümesi için Active Direct nesnelerini oluşturmak için **Tüm Özellikleri Oku** ve Bilgisayar Nesneleri **Oluşturma** iznine ihtiyaç duyar. **Tüm Özellikleri Oku** izni zaten varsayılan olarak CORP\Install'a verilir, bu nedenle açıkça vermeniz gerekmez. Failover kümesini oluşturmak için gereken izinler hakkında daha fazla bilgi için [bkz.](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx)

    Active Directory ve kullanıcı nesnelerini yapılandırmayı tamamladığınızda, iki SQL Server VM oluşturup bu etki alanına katılırsınız.

## <a name="create-the-sql-server-vms"></a>SQL Server VM'leri oluşturma
1. Yerel bilgisayarınızda açık olan PowerShell penceresini kullanmaya devam edin. Aşağıdaki ek değişkenleri tanımlayın:

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    IP adresi **10.10.0.4** genellikle Azure sanal ağınızın **10.10.0.0/16** alt ağında oluşturduğunuz ilk VM'ye atanır. **IpCONFIG**çalıştırarak bunun etki alanı denetleyici sunucunuzun adresi olduğunu doğrulamanız gerekir.
2. **ContosoQuorum**adlı failover kümesindeki ilk VM'yi oluşturmak için aşağıdaki borulu komutları çalıştırın:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Yukarıdaki komutla ilgili olarak aşağıdakileri not ediniz:

   * **Yeni AzureVMConfig,** istenilen kullanılabilirlik kümesi adı ile bir VM yapılandırması oluşturur. Sonraki VM'ler aynı kullanılabilirlik kümesine katılabilmeleri için aynı kullanılabilirlik kümesi adı ile oluşturulur.
   * **Add-AzureProvisioningConfig,** VM'yi oluşturduğunuz Etkin Dizin etki alanına katılır.
   * **Set-AzureSubnet** VM'yi arka alt ağa yerleştirir.
   * **Yeni AzureVM** yeni bir bulut hizmeti oluşturur ve yeni bulut hizmetinde yeni Azure VM'yi oluşturur. **DnsSettings** parametresi, yeni bulut hizmetindeki sunucular için DNS sunucusunun IP **adresi10.10.0.4'e**sahip olduğunu belirtir. Bu etki alanı denetleyicisi sunucusunun IP adresidir. Bu parametre, bulut hizmetindeki yeni VM'lerin Active Directory etki alanına başarıyla katılmasını sağlamak için gereklidir. Bu parametre olmadan, VM'nin sağlanmasından sonra etki alanı denetleyici sabunu birincil DNS sunucusu olarak kullanmak için VM'nizdeki IPv4 ayarlarını el ile ayarlamanız ve ardından VM'yi Active Directory etki alanına katılmanız gerekir.
3. **ContosoSQL1** ve **ContosoSQL2**adlı SQL Server VM'leri oluşturmak için aşağıdaki borulu komutları çalıştırın.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Yukarıdaki komutlarla ilgili olarak aşağıdakilere dikkat edin:

   * **New-AzureVMConfig,** etki alanı denetleyici sağlığıyla aynı kullanılabilirlik kümesi adını kullanır ve sanal makine galerisindeki SQL Server 2012 Service Pack 1 Enterprise Edition resmini kullanır. Ayrıca, işletim sistemi diskini yalnızca okuma önbelleğe alma (yazma önbelleğe alma yok) olarak ayarlar. Veritabanı dosyalarını VM'ye iliştirdiğiniz ayrı bir veri diskine geçirmenizi ve önbelleğe okuma veya yazma olmadan yapılandırmanızı öneririz. Ancak, bir sonraki en iyi şey, işletim sistemi diskinde okuma önbelleğe kaldıramadığınız için işletim sistemi diskinde yazma önbelleğe almaktır.
   * **Add-AzureProvisioningConfig,** VM'yi oluşturduğunuz Etkin Dizin etki alanına katılır.
   * **Set-AzureSubnet** VM'yi arka alt ağa yerleştirir.
   * **Add-AzureEndpoint,** istemci uygulamalarının bu SQL Server hizmetleri örneklerine Internet'te erişebilmeleri için erişim uç noktaları ekler. Farklı bağlantı noktaları ContosoSQL1 ve ContosoSQL2 verilir.
   * **New-AzureVM,** ContosoQuorum ile aynı bulut hizmetinde yeni SQL Server VM oluşturur. Aynı kullanılabilirlik kümesinde olmalarını istiyorsanız, VM'leri aynı bulut hizmetine yerleştirmeniz gerekir.
4. Her VM'nin tam olarak sağlanmasını ve her VM'nin uzak masaüstü dosyasını çalışma dizininize indirmesini bekleyin. Döngü `for` üç yeni VM'den geçer ve her biri için üst düzey kıvırcık parantez içindeki komutları çalıştırır.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until the VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    SQL Server VM'ler artık kullanılabilir ve çalışıyor, ancak varsayılan seçenekleri olan SQL Server ile yüklüler.

## <a name="initialize-the-failover-cluster-vms"></a>Başarısız küme VM'leri başlatma
Bu bölümde, failover kümesinde ve SQL Server yüklemesinde kullanacağınız üç sunucuyu değiştirmeniz gerekir. Daha ayrıntılı şekilde belirtmek gerekirse:

* Tüm sunucular: **Failover Clustering** özelliğini yüklemeniz gerekir.
* Tüm sunucular: Makine **yöneticisi**olarak **CORP\Install** eklemeniz gerekir.
* Yalnızca ContosoSQL1 ve ContosoSQL2: Varsayılan veritabanında **sysadmin** rolü olarak **CORP\Install** eklemeniz gerekir.
* Yalnızca ContosoSQL1 ve ContosoSQL2: **NT AUTHORITY\System'i** aşağıdaki izinlerle oturum açma olarak eklemeniz gerekir:

  * Kullanılabilirlik grubunu değiştirme
  * SQL'e Bağla
  * Sunucu durumunu görüntüleme
* Yalnızca ContosoSQL1 ve ContosoSQL2: **TCP** protokolü SQL Server VM'de zaten etkindir. Ancak, SQL Server'a uzaktan erişim için güvenlik duvarını açmanız gerekir.

Şimdi, başlamaya hazırsın. **ContosoQuorum**ile başlayarak aşağıdaki adımları izleyin:

1. Uzak masaüstü dosyalarını başlatarak **ContosoQuorum'a** bağlanın. Makine yöneticisinin Kullanıcı Adı **AzureAdmin'i** ve VM'leri oluşturduğunuzda belirttiğiniz **Contoso!000**parolasını kullanın.
2. Bilgisayarların **corp.contoso.com'a**başarıyla katıldığını doğrulayın.
3. Devam etmeden önce otomatik başlatma görevlerini çalıştırmayı bitirmek için SQL Server yüklemesini bekleyin.
4. Yönetici modunda bir PowerShell penceresi açın.
5. Windows Failover Kümeleme özelliğini yükleyin.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. Yerel yönetici olarak **CORP\Install'ı** ekleyin.

        net localgroup administrators "CORP\Install" /Add
7. ContosoQuorum'dan çıkış. Bu sunucuyla artık bitirdin.

        logoff.exe

Ardından, **ContosoSQL1** ve **ContosoSQL2'yi**başharfe alı Aşağıdaki adımları izleyin, her iki SQL Server VM için aynıdır.

1. Uzak masaüstü dosyalarını başlatarak iki SQL Server VM'ye bağlanın. Makine yöneticisinin Kullanıcı Adı **AzureAdmin'i** ve VM'leri oluşturduğunuzda belirttiğiniz **Contoso!000**parolasını kullanın.
2. Bilgisayarların **corp.contoso.com'a**başarıyla katıldığını doğrulayın.
3. Devam etmeden önce otomatik başlatma görevlerini çalıştırmayı bitirmek için SQL Server yüklemesini bekleyin.
4. Yönetici modunda bir PowerShell penceresi açın.
5. Windows Failover Kümeleme özelliğini yükleyin.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. Yerel yönetici olarak **CORP\Install'ı** ekleyin.

        net localgroup administrators "CORP\Install" /Add
7. SQL Server PowerShell Sağlayıcısı'nı içeri aktarın.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking
8. Varsayılan SQL Server örneği için sysadmin rolü olarak **CORP\Install** ekleyin.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."
9. NT **AUTHORITY\System'i** yukarıda açıklanan üç izinle oturum açma olarak ekleyin.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
10. SQL Server'a uzaktan erişmek için güvenlik duvarını açın.

         netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP
11. Her iki VM'den de çıkış kaydedin.

         logoff.exe

Son olarak, kullanılabilirlik grubunu yapılandırmaya hazırsınız. **ContosoSQL1**üzerindeki tüm çalışmaları gerçekleştirmek için SQL Server PowerShell Sağlayıcısı'nı kullanacaksınız.

## <a name="configure-the-availability-group"></a>Kullanılabilirlik grubunu yapılandırma
1. Uzak masaüstü dosyalarını başlatarak **ContosoSQL1'e** tekrar bağlanın. Makine hesabını kullanarak oturum imzalamak **yerine, CORP\Install**kullanarak oturum açın.
2. Yönetici modunda bir PowerShell penceresi açın.
3. Aşağıdaki değişkenleri tanımlayın:

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"
4. SQL Server PowerShell Sağlayıcısı'nı içeri aktarın.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking
5. ContosoSQL1 için SQL Server hizmet hesabını CORP\SQLSvc1 olarak değiştirin.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
6. ContosoSQL2 için SQL Server hizmet hesabını CORP\SQLSvc2 olarak değiştirin.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
7. [Azure VM'deki Her Zaman Kullanılabilirlik Grupları için Failover Cluster Oluştur'dan](https://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) **CreateAzureFailoverCluster.ps1'i** yerel çalışma dizinine indirin. Bu komut dosyasını işlevsel bir başarısız küme oluşturmanıza yardımcı olmak için kullanırsınız. Windows Failover Clustering'in Azure ağıyla nasıl etkileşimde bulunduğu hakkında önemli bilgiler için Azure [Sanal Makinelerde SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json)bilgisine bakın.
8. Çalışma dizininizi değiştirin ve indirilen komut dosyasıyla birlikte başarısız kümeyi oluşturun.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"
9. **ContosoSQL1** ve **ContosoSQL2'deki**varsayılan SQL Server örnekleri için Her Zaman Kullanılabilirlik Gruplarını Etkinleştirin.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
10. SQL Server hizmet hesapları için bir yedekleme dizini oluşturun ve izinler tanıyın. Bu dizini, ikincil yinelemedeki kullanılabilirlik veritabanını hazırlamak için kullanırsınız.

         $backup = "C:\backup"
         New-Item $backup -ItemType directory
         net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
         icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")
11. **ContosoSQL1** **MyDB1**adlı bir veritabanı oluşturun, hem tam bir yedekleme hem de bir günlük yedeklemesi alın ve **NORECOVERY** seçeneği ile **ContosoSQL2'de** geri yükleyin.

         Invoke-SqlCmd -Query "CREATE database $db"
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery
12. SQL Server VM'lerde kullanılabilirlik grubu uç noktalarını oluşturun ve uç noktalarda uygun izinleri ayarlayın.

         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server1\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"
         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server2\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"

         Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct2]" -ServerInstance $server1
         Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct1]" -ServerInstance $server2
13. Kullanılabilirlik yinelemelerini oluşturun.

         $primaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server1 `
             -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
         $secondaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server2 `
             -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
14. Son olarak, kullanılabilirlik grubunu oluşturun ve kullanılabilirlik grubuna ikincil yinelemekatılın.

         New-SqlAvailabilityGroup `
             -Name $ag `
             -Path "SQLSERVER:\SQL\$server1\Default" `
             -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
             -Database $db
         Join-SqlAvailabilityGroup `
             -Path "SQLSERVER:\SQL\$server2\Default" `
             -Name $ag
         Add-SqlAvailabilityDatabase `
             -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
             -Database $db

## <a name="next-steps"></a>Sonraki adımlar
Azure'da bir kullanılabilirlik grubu oluşturarak SQL Server Always Always'i başarıyla uyguladınız. Bu kullanılabilirlik grubu için bir dinleyiciyapılandırmak [için, Azure'daki her zaman kullanılabilirlik grupları için bir ILB dinleyicisi yapılandırın'](../classic/ps-sql-int-listener.md)a bakın.

Azure'da SQL Server'ı kullanma hakkında diğer bilgiler için [Azure sanal makinelerde SQL Server'a](../sql/virtual-machines-windows-sql-server-iaas-overview.md)bakın.
