---
title: Site Kurtarma kurtarma planlarına Azure Otomasyonrunkitaplarını ekleme
description: Azure Site Kurtarma'yı kullanarak olağanüstü durum kurtarma için Azure Otomasyonu ile kurtarma planlarını nasıl genişleteceklerini öğrenin.
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: conceptual
ms.date: 09/18/2019
ms.author: rajanaki
ms.openlocfilehash: ecfe993a137ca63c84438870ec54ac1e6d6707da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257491"
---
# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Kurtarma planlarına Azure Otomasyonu runbook’ları ekleme

Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md) kurtarma planlarını genişletmek için Azure Otomasyon runbook'ları nasıl tümleştirilen açıklanmaktadır. Aksi takdirde el ile müdahale gerektiren temel görevleri nasıl otomatikleştireceğinizve çok aşamalı bir kurtarmayı tek tıklatma eylemine nasıl dönüştüreceğinizi gösteririz.

## <a name="recovery-plans"></a>Kurtarma planları 

Şirket içi makinelerde veya Azure VM'lerinde başarısız olduğunuzda kurtarma planlarını kullanabilirsiniz. Kurtarma planları, makinelerin nasıl başarısız olduğunu ve başarısız olduktan sonra nasıl başlayıp kurtarıldığını tanımlayan sistematik bir kurtarma işlemi tanımlamanıza yardımcı olur. 

Büyük uygulamaların kurtarılması karmaşık olabilir. Kurtarma planları, kurtarmanın tutarlı bir şekilde doğru, yinelenebilir ve otomatik olması için siparişin dayatılmasına yardımcı olur. Komut dosyalarının yanı sıra Azure Otomasyon runbook'larını kullanarak kurtarma planı içindeki görevleri otomatikleştirebilirsiniz. Tipik örnekler, başarısız olduktan sonra Azure VM'deki ayarları yapılandırmak veya VM'de çalışan bir uygulamayı yeniden yapılandırmak olabilir.

- Kurtarma planları hakkında [daha fazla bilgi edinin.](recovery-plan-overview.md)
- Azure Otomasyon runbook'ları hakkında [daha fazla bilgi edinin.](../automation/automation-runbook-types.md)



## <a name="runbooks-in-recovery-plans"></a>Kurtarma planlarında runbook'lar

Kurtarma planına bir Azure Otomasyon hesabı ve runbook'lar eklersiniz. Kurtarma planı çalıştığında runbook çağrılır.

- Otomasyon hesabı herhangi bir Azure bölgesinde olabilir ve Site Kurtarma kasası ile aynı abonelikte olmalıdır. 
- Runbook, birincil konumdan ikincil konuma geçeme sırasında veya ikincil konumdan birincil konuma geri dönüş sırasında bir kurtarma planında çalıştırılabilir.
- Kurtarma planındaki runbook'lar, ayarlanan sırada birbiri ardına seri olarak çalıştırılır.
- Kurtarma planındaki runbook'lar VM'leri farklı gruplarda başlayacak şekilde yapılandırıyorsa, kurtarma planı yalnızca Azure tüm VM'leri çalışıyor olarak bildirdiğinde devam edecektir.
- Komut dosyası başarısız olsa bile kurtarma planları çalışmaya devam eder.

### <a name="recovery-plan-context"></a>Kurtarma planı bağlamı

Bir komut dosyası çalıştığında, runbook'a bir kurtarma planı bağlamı enjekte eder. Bağlam, tabloda özetlenen değişkenleri içerir.

| **Değişken adı** | **Açıklama** |
| --- | --- |
| KurtarmaPlanName |Kurtarma planı adı. Ada dayalı eylemlerde kullanılır. |
| FailoverType |Bunun bir test mi yoksa üretim başarısız mı olduğunu belirtir. 
| FailoverDirection | Kurtarmanın birincil veya ikincil bir konuma olup olmadığını belirtir. |
| Groupıd |Plan çalışırken kurtarma planındaki grup numarasını tanımlar. |
| VmMap |Gruptaki tüm VM'lerden oluşan bir dizi. |
| VMMap tuşu |Her VM için benzersiz bir anahtar (GUID). |
| SubscriptionId |VM'nin oluşturulduğu Azure abonelik kimliği. |
| ResourceGroupName | VM'nin bulunduğu kaynak grubunun adı.
| CloudServiceName |VM'nin oluşturulduğu Azure bulut hizmeti adı. |
| RoleName |Azure VM'nin adı. |
| RecoveryPointId|VM kurtarma için zaman damgası. |

Aşağıdaki örnekte bir bağlam değişkeni gösterilmektedir:

```
{"RecoveryPlanName":"hrweb-recovery",
"FailoverType":"Test",
"FailoverDirection":"PrimaryToSecondary",
"GroupId":"1",
"VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":
    { "SubscriptionId":"7a1111111-c1d6-49c5-8c5d-111ce8dd183",
    "ResourceGroupName":"ContosoRG",
    "CloudServiceName":"pod02hrweb-Chicago-test",
    "RoleName":"Fabrikam-Hrweb-frontend-test",
    "RecoveryPointId":"TimeStamp"}
    }
}
```

VMMap'teki tüm VMM'lere bir döngü içinde erişmek istiyorsanız, aşağıdaki kodu kullanabilirsiniz:

```
$VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
$vmMap = $RecoveryPlanContext.VmMap
    foreach($VMID in $VMinfo)
    {
        $VM = $vmMap.$VMID                
            if( !(($VM -eq $Null) -Or ($VM.ResourceGroupName -eq $Null) -Or ($VM.RoleName -eq $Null))) {
            #this check is to ensure that we skip when some data is not available else it will fail
    Write-output "Resource group name ", $VM.ResourceGroupName
    Write-output "Rolename " = $VM.RoleName
            }
        }
```


[Hasat Bulutlar](http://harvestingclouds.com) de Aman Sharma günlüğü üzerinde bir [kurtarma planı bağlam komut yararlı](http://harvestingclouds.com/post/script-sample-azure-automation-runbook-for-asr-recovery-plan/)bir örnek vardır.



## <a name="before-you-start"></a>Başlamadan önce

- Azure Otomasyonu'nda yeniyseniz, [kaydolabilir](https://azure.microsoft.com/services/automation/) ve [örnek komut dosyalarını indirebilirsiniz.](https://azure.microsoft.com/documentation/scripts/)
- Otomasyon hesabının aşağıdaki modüllere sahip olduğundan emin olun:
    - AzureRM.profile
    - AzureRM.Resources
    - AzureRM.Automation
    - AzureRM.Network
    - AzureRM.Compute

    Tüm modüller uyumlu versiyonlardan olmalıdır. En basit yolu her zaman tüm modüllerin en son sürümlerini kullanmaktır.



## <a name="customize-the-recovery-plan"></a>Kurtarma planını özelleştirme

1. Kasada Kurtarma **Planları 'nu (Site Kurtarma)** seçin
2. Kurtarma planı oluşturmak için **+Kurtarma Planı'nı**tıklatın. [Daha fazla bilgi edinin](site-recovery-create-recovery-plans.md). Zaten bir kurtarma planınız varsa, açmayı seçin.
3. Kurtarma planı **sayfasında, Özelleştir'i**tıklatın.

    ![Özelleştir düğmesini tıklatın](media/site-recovery-runbook-automation-new/custom-rp.png)

2. Grup 1'in yanındaki elipsleri (...) **tıklatın:** > **Gönder eylemini Başlat'** ı başlatın.
3. **InSert**action'da, **Komut Dosyası'nın** seçildiğini doğrulayın ve komut dosyası için bir ad belirtin (**Hello World).**
4. Bir otomasyon hesabı belirtin ve bir runbook seçin. Komut dosyasını kaydetmek için **Tamam'ı**tıklatın. Komut dosyası **Grup 1'e eklenir: Sonraki adımlar.**


## <a name="reuse-a-runbook-script"></a>Runbook komut dosyalarını yeniden kullanma

Dış değişkenleri kullanarak birden çok kurtarma planında tek bir runbook komut dosyası kullanabilirsiniz. 

- Kurtarma planı çalıştırmak için parametreleri depolamak için [Azure Otomasyon değişkenlerini](../automation/automation-variables.md) kullanırsınız.
- Kurtarma planı adını değişkene önek olarak ekleyerek, her kurtarma planı için ayrı değişkenler oluşturabilirsiniz. Ardından, değişkenleri parametre olarak kullanın.
- Komut dosyasını değiştirmeden bir parametreyi değiştirebilir, ancak yine de komut dosyasının çalışma şeklini değiştirebilirsiniz.

### <a name="use-a-simple-string-variable-in-a-runbook-script"></a>Runbook komut dosyasında basit bir dize değişkeni kullanma

Bu örnekte, bir komut dosyası bir Ağ Güvenlik Grubu (NSG) girdisini alır ve kurtarma planındaki VM'lere uygular. 

1. Komut dosyasının hangi kurtarma planının çalıştığını algılayabilmesi için bu kurtarma planı bağlamını kullanın:

    ```
    workflow AddPublicIPAndNSG {
        param (
              [parameter(Mandatory=$false)]
              [Object]$RecoveryPlanContext
        )

        $RPName = $RecoveryPlanContext.RecoveryPlanName
    ```

2. NSG adını ve kaynak grubunu not edin. Bu değişkenleri kurtarma planı komut dosyaları için giriş olarak kullanırsınız. 
1. Otomasyon hesabı varlıklarında. NSG adını depolamak için bir değişken oluşturun. Kurtarma planının adını içeren değişken adına bir önek ekleyin.

    ![NSG ad değişkeni oluşturma](media/site-recovery-runbook-automation-new/var1.png)

2. NSG kaynağıiçin kaynak grubu adını depolamak için bir değişken oluşturun. Kurtarma planının adını içeren değişken adına bir önek ekleyin.

    ![NSG kaynak grubu adı oluşturma](media/site-recovery-runbook-automation-new/var2.png)


3.  Komut dosyasında, değişken değerlerini almak için bu başvuru kodunu kullanın:

    ```
    $NSGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSG"
    $NSGRGValue = $RecoveryPlanContext.RecoveryPlanName + "-NSGRG"

    $NSGnameVar = Get-AutomationVariable -Name $NSGValue
    $RGnameVar = Get-AutomationVariable -Name $NSGRGValue
    ```

4.  NSG'yi başarısız vm'nin ağ arabirimine uygulamak için runbook'taki değişkenleri kullanın:

    ```
    InlineScript {
    if (($Using:NSGname -ne $Null) -And ($Using:NSGRGname -ne $Null)) {
            $NSG = Get-AzureRmNetworkSecurityGroup -Name $Using:NSGname -ResourceGroupName $Using:NSGRGname
            Write-output $NSG.Id
            #Apply the NSG to a network interface
            #$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
            #Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
            #  -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $NSG
        }
    }
    ```


Her kurtarma planı için, komut dosyasını yeniden kullanabilmeniz için bağımsız değişkenler oluşturun. Kurtarma planı adını kullanarak bir önek ekleyin. 

Bu senaryo için eksiksiz, uçuca bir komut dosyası için [bu komut dosyasını](https://gallery.technet.microsoft.com/Add-Public-IP-and-NSG-to-a6bb8fee)gözden geçirin.


### <a name="use-a-complex-variable-to-store-more-information"></a>Daha fazla bilgi depolamak için karmaşık bir değişken kullanma

Bazı senaryolarda, her kurtarma planı için ayrı değişkenler oluşturamayabilirsiniz. Belirli VM'lerde ortak bir IP adresi atamak için tek bir komut dosyası istediğiniz bir senaryo düşünün. Başka bir senaryoda, farklı VM'lere farklı NSG'ler uygulamak isteyebilirsiniz (tüm VM'lerde değil). Şunlara dikkat edin:

- Herhangi bir kurtarma planı için yeniden kullanılabilir bir komut dosyası yapabilirsiniz.
- Her kurtarma planı değişken sayıda VM'ye sahip olabilir.
- Örneğin, SharePoint kurtarma iki ön uçları vardır. Temel bir iş hattı (LOB) uygulamasının yalnızca bir ön ucu vardır.
- Bu senaryoda, her kurtarma planı için ayrı değişkenler oluşturamazsınız.

Aşağıdaki örnekte, Azure Otomasyonu hesabında karmaşık bir [değişken](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azureautomationvariable) oluştururuz.

Bunu Azure PowerShell kullanarak birden çok değer belirterek yapıyoruz.

1. PowerShell'de Azure aboneliğinizde oturum açın:

    ```
    Connect-AzureRmAccount
    $sub = Get-AzureRmSubscription -Name <SubscriptionName>
    $sub | Select-AzureRmSubscription
    ```

2. Parametreleri depolamak için, kurtarma planının adını kullanarak karmaşık değişkeni oluşturun:

    ```
    $VMDetails = @{"VMGUID"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"};"VMGUID2"=@{"ResourceGroupName"="RGNameOfNSG";"NSGName"="NameOfNSG"}}
        New-AzureRmAutomationVariable -ResourceGroupName <RG of Automation Account> -AutomationAccountName <AA Name> -Name <RecoveryPlanName> -Value $VMDetails -Encrypted $false
    ```

3. Bu karmaşık **değişkende, VMDetails** korumalı VM için VM KIMLIĞIdir. Azure portalında VM Kimliğini almak için VM özelliklerini görüntüleyin. Aşağıdaki ekran görüntüsü, iki VM'nin ayrıntılarını depolayan bir değişkeni gösterir:

    ![VM Kimliğini GUID olarak kullanma](media/site-recovery-runbook-automation-new/vmguid.png)

4. Runbook'unuzda bu değişkeni kullanın. Belirtilen VM GUID kurtarma planı bağlamında bulunursa, VM'ye NSG uygulayın:

    ```
    $VMDetailsObj = (Get-AutomationVariable -Name $RecoveryPlanContext.RecoveryPlanName).ToObject([hashtable])
    ```

4. Runbook'unuzda, kurtarma planı bağlamının VM'leri arasında geçiş yapın. VM'nin $VMDetailsObj'da var olup olmadığını kontrol **edin.** Varsa, NSG uygulamak için değişkenin özelliklerine erişin:

    ```
        $VMinfo = $RecoveryPlanContext.VmMap | Get-Member | Where-Object MemberType -EQ NoteProperty | select -ExpandProperty Name
        $vmMap = $RecoveryPlanContext.VmMap

        foreach($VMID in $VMinfo) {
            $VMDetails = $VMDetailsObj[$VMID].ToObject([hashtable]);
            Write-output $VMDetails
            if ($VMDetails -ne $Null) { #If the VM exists in the context, this will not be Null
                $VM = $vmMap.$VMID
                # Access the properties of the variable
                $NSGname = $VMDetails.NSGName
                $NSGRGname = $VMDetails.NSGResourceGroupName

                # Add code to apply the NSG properties to the VM
            }
        }
    ```

Farklı kurtarma planları için aynı komut dosyasını kullanabilirsiniz. Bir kurtarma planına karşılık gelen değeri farklı değişkenlerde depolayarak farklı parametreler girin.

## <a name="sample-scripts"></a>Örnek komut dosyaları

Örnek komut dosyalarını Otomasyon hesabınıza dağıtmak için **Azure'a Dağıt** düğmesini tıklatın.

[![Azure'a Dağıt](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

Bu video başka bir örnek sağlar. İki katmanlı bir WordPress uygulamasını Azure'a nasıl kurtarılabildiğini gösterir:


> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]


## <a name="next-steps"></a>Sonraki adımlar

- Azure [Otomasyon Hesabı Olarak Çalıştır](../automation/automation-create-runas-account.md) hakkında bilgi edinin
- [Azure Otomasyon örnek komut dosyalarını](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=User&f%5B0%5D.Value=SC%20Automation%20Product%20Team&f%5B0%5D.Text=SC%20Automation%20Product%20Team)inceleyin.
- Failovers çalıştırma hakkında [daha fazla bilgi edinin.](site-recovery-failover.md)



