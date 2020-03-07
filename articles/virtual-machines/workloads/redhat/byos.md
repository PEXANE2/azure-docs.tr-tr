---
title: Kendi aboneliklerinizi getir Azure görüntülerini Red Hat Enterprise Linux | Microsoft Docs
description: Azure 'da Red Hat Enterprise Linux için kendi abonelik görüntülerini getir hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
editor: ''
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/10/2020
ms.author: alsin
ms.openlocfilehash: 712626345e10ab0e4290ac91b0f121ff6960303e
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78396825"
---
# <a name="red-hat-enterprise-linux-bring-your-own-subscription-gold-images-in-azure"></a>Azure 'da kendi aboneliğinizi getir Gold görüntülerini Red Hat Enterprise Linux

Red Hat Enterprise Linux (RHEL) görüntüleri Azure 'da Kullandıkça Öde (PAYG) veya kendi aboneliğinizi getir (Red Hat Gold görüntü) modeli aracılığıyla kullanılabilir. Bu belgede, Azure 'daki Red Hat Gold görüntülerine genel bakış sunulmaktadır.

>[!NOTE]
> RHEL BYOS Gold görüntüleri Azure genel (ticari) ve Azure Kamu bulutlarında kullanılabilir. Azure Çin veya Azure Kara Orman bulutlarında mevcut değildir.

## <a name="important-points-to-consider"></a>Dikkate alınması gereken önemli nokta

- Bu programda sunulan Red Hat Gold görüntüleri, Azure galerisindeki/Market 'teki RHEL PAYG görüntülerine benzer üretime yönelik olarak kullanılabilir RHEL görüntüleridir.

- Görüntüler, [Azure 'da Red Hat Enterprise Linux görüntülerde](./redhat-images.md) açıklanan geçerli ilkelerinizi izler

- Bu görüntülerden oluşturulan VM 'lere standart destek ilkeleri uygulanır

- Red Hat Gold görüntülerinden sağlanan VM 'Ler RHEL PAYG görüntüleriyle ilişkili RHEL ücretlerini taşımaz

- Görüntülerin yetkileri yoktur, bu nedenle, Red Hat 'ten güncelleştirmeleri doğrudan almak için VM 'Leri kaydetmek ve abone olmak için abonelik Yöneticisi 'ni kullanmanız gerekir

- Linux görüntüleri için KCG ve PAYG faturalandırma modelleri arasında dinamik olarak geçiş yapmak mümkün değildir. Faturalandırma modelini değiştirmek için VM 'nin ilgili görüntüden yeniden dağıtılması gerekir

>[!NOTE]
> 2\. nesil RHEL BYOS görüntüleri Market teklifiyle Şu anda kullanılamıyor. Gen 2 RHEL BYOS görüntüsüne ihtiyacınız varsa, Red Hat abonelik yönetiminde coud erişim panosunu ziyaret edin. Daha fazla ayrıntı, [Red Hat belgelerinde](https://access.redhat.com/articles/4847681)bulunabilir.

## <a name="requirements-and-conditions-to-access-the-red-hat-gold-images"></a>Red Hat Gold görüntülerine erişmek için gereksinimler ve koşullar

1. Red hat [bulut erişim programı](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) hükümlerini öğrenin ve Red hat [Abonelik Yöneticisi](https://access.redhat.com/management/cloud)'Nde bulut erişimi için Red Hat aboneliklerinizi etkinleştirin. Bulut erişimi için kaydedilecek Azure aboneliklerinizi de uygulamanız gerekir.

1. Bulut erişimi için uygun uygunluk gereksinimlerini karşılayan Red Hat aboneliklerini etkinleştirdiyseniz, altın görüntü erişimi için Azure abonelikleriniz otomatik olarak etkinleştirilir.

### <a name="expected-time-for-image-access"></a>Görüntü erişimi için beklenen süre

Bulut erişimi etkinleştirme adımlarını tamamladıktan sonra Red hat, Red Hat altın görüntülerine uygunluk görüntülerini doğrular. Doğrulama başarılı olursa, üç saat içinde altın görüntülere erişim elde edersiniz.

## <a name="use-the-red-hat-gold-images-from-the-azure-portal"></a>Azure portal kırmızı hat altın görüntülerini kullanın

1. Azure aboneliğiniz Red Hat Gold görüntülerine erişim aldıktan sonra, **kaynak oluştur ' a** giderek ve ardından **Tümünü göster**' i inceleyerek [Azure Portal](https://portal.azure.com) bulabilirsiniz.

1. Sayfanın üst kısmında özel tekliflerle ilgili olduğunu görürsünüz.

    ![Market özel teklifleri](./media/rhel-byos-privateoffers.png)

1. Özel tekliflerinizi görmek için mor bağlantıya tıklayabilir veya sayfanın en altına gidin.

1. Kullanıcı arabirimindeki sağlamanın geri kalanı, var olan herhangi bir Red Hat görüntüsüne göre farklı olmayacaktır. RHEL sürümünüzü seçin ve VM 'nizi sağlamak için istemleri izleyin. Bu işlem, son adımda resmin şartlarını kabul etmenize de olanak tanır.

>[!NOTE]
>Bu adımlar, şu ana kadar Red Hat Gold görüntü görüntünüzü programsal dağıtım için etkinleştirmeyecektir: aşağıdaki "ek bilgiler" bölümünde açıklandığı gibi ek bir adım gerekecektir.

Bu belgenin geri kalanı, görüntü üzerinde hüküm sağlamak ve kabul etmek için CLı yöntemine odaklanır. Kullanıcı arabirimi ve CLı, son sonuç (sağlanan RHEL Gold Image VM) ile oldukça tamamen değiştirilebilir.

## <a name="use-the-red-hat-gold-images-from-the-azure-cli"></a>Azure CLı 'deki Red Hat Gold görüntülerini kullanma
Aşağıdaki yönergeler kümesi, Azure CLı kullanarak bir RHEL VM için ilk dağıtım sürecinde size yol gösterecektir. Bu yönergelerde, [Azure CLI 'nin yüklü](https://docs.microsoft.com/cli/azure/install-azure-cli)olduğu varsayılır.

>[!IMPORTANT]
>Aşağıdaki komutlar için yayımcı, teklif, plan ve görüntü başvurularında tüm küçük harfleri kullandığınızdan emin olun

1. İstediğiniz abonelikte olup olmadığınızı kontrol edin:
    ```azurecli
    az account show -o=json
    ```

1. Red Hat Gold Image VM 'niz için bir kaynak grubu oluşturun:
    ```azurecli
    az group create --name <name> --location <location>
    ```

1. Görüntü koşullarını kabul edin:
    ```azurecli
    az vm image terms accept --publisher redhat --offer rhel-byos --plan <SKU value here> -o=jsonc

    # Example:
    az vm image terms accept --publisher redhat --offer rhel-byos --plan rhel-lvm75 -o=jsonc

    OR

    az vm image terms accept --urn RedHat:rhel-byos:rhel-lvm8:8.0.20190620
    ```
    >[!NOTE]
    >Bu koşulların *her bir Azure aboneliği için, görüntü SKU 'su başına bir kez*kabul edilmesi gerekir.

1. Seçim Aşağıdaki komutla VM dağıtımınızı doğrulayın:
    ```azurecli
    az vm create -n <VM name> -g <resource group name> --image <image urn> --validate

    # Example:
    az vm create -n rhel-byos-vm -g rhel-byos-group --image RedHat:rhel-byos:rhel-lvm75:7.5.20190620
    ```

1. `--validate` bağımsız değişkeni olmadan yukarıdaki gibi aynı komutu çalıştırarak VM 'nizi sağlayın:
    ```azurecli
    az vm create -n <VM name> -g <resource group name> --image <image urn> --validate
    ```

1. Sanal makinenize SSH ekleyin ve ilgili olmayan bir görüntünüz olduğunu doğrulayın. Bunu yapmak için `sudo yum repolist` çalıştırın (RHEL 8 için `sudo dnf repolist`kullanın). Bu çıktı, VM 'yi Red Hat ile kaydetmek için abonelik Yöneticisi 'ni kullanmanızı ister.

>[!NOTE]
>RHEL 8 ' de `dnf` ve `yum`, bu, [RHEL 8 yönetici kılavuzunda](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_basic_system_settings/installing-software-with-yum_configuring-basic-system-settings)bunun hakkında daha fazla değiştirilebilir.


## <a name="use-the-red-hat-gold-images-from-powershell"></a>PowerShell 'den Red Hat Gold görüntülerini kullanma
Aşağıda örnek bir komut dosyası verilmiştir. Kaynak grubu, konum, VM adı, oturum açma bilgileri ve diğer değişkenleri tercih ettiğiniz yapılandırmayla değiştirmelisiniz. Yayımcı ve plan bilgileri küçük harfle yazılmalıdır.

```powershell-interactive
    # Variables for common values
    $resourceGroup = "testbyos"
    $location = "canadaeast"
    $vmName = "test01"

    # Define user name and blank password
    $securePassword = ConvertTo-SecureString 'TestPassword1!' -AsPlainText -Force
    $cred = New-Object System.Management.Automation.PSCredential("azureuser",$securePassword)
    Get-AzureRmMarketplaceTerms -Publisher RedHat -Product rhel-byos -Name rhel-lvm75 | SetAzureRmMarketplaceTerms -Accept

    # Create a resource group
    New-AzureRmResourceGroup -Name $resourceGroup -Location $location

    # Create a subnet configuration
    $subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

    # Create a virtual network
    $vnet = New-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Location
    $location `-Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

    # Create a public IP address and specify a DNS name
    $pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location
    $location `-Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

    # Create an inbound network security group rule for port 22
    $nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name
    myNetworkSecurityGroupRuleSSH -Protocol Tcp `
    -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -
    DestinationAddressPrefix * `-DestinationPortRange 22 -Access Allow

    # Create a network security group
    $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location
    $location `-Name myNetworkSecurityGroup -SecurityRules $nsgRuleSSH

    # Create a virtual network card and associate with public IP address and NSG
    $nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -
    Location $location `-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

    # Create a virtual machine configuration
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize Standard_D3_v2 |
    Set-AzureRmVMOperatingSystem -Linux -ComputerName $vmName -Credential $cred |
    Set-AzureRmVMSourceImage -PublisherName redhat -Offer rhel-byos -Skus rhel-lvm75 -Version latest | Add-     AzureRmVMNetworkInterface -Id $nic.Id
    Set-AzureRmVMPlan -VM $vmConfig -Publisher redhat -Product rhel-byos -Name "rhel-lvm75"

    # Configure SSH Keys
    $sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
    Add-AzureRmVMSshPublicKey -VM $vmconfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

    # Create a virtual machine
    New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig
```

## <a name="encrypt-red-hat-enterprise-linux-bring-your-own-subscription-gold-images"></a>Kendi aboneliğinizi getir altın görüntülerini şifreleyin Red Hat Enterprise Linux

Red Hat Enterprise Linux kendi aboneliğinizi getir Gold görüntülerinin [Azure disk şifrelemesi](../../linux/disk-encryption-overview.md)kullanılarak güvenliği sağlanmış olabilir. Ancak, şifreleme etkinleştirilmeden önce aboneliğin kayıtlı olması **gerekir** .  Red Hat sitesinde bir RHEL BYOS Gold görüntüsünü kaydetme ayrıntılarına ulaşılabilir. [Red Hat abonelik-Manager kullanarak bir sistemi kaydetme ve Red Hat müşteri portalı 'na abone olma](https://access.redhat.com/solutions/253273)konusuna bakın. etkin bir Red Hat aboneliğiniz varsa, [Red Hat müşteri portalı etkinleştirme anahtarları oluşturmayı](https://access.redhat.com/articles/1378093)da okuyabilirsiniz.

Azure disk şifrelemesi, [Red hat özel görüntülerinde](../../linux/redhat-create-upload-vhd.md)desteklenmez. Ek ADE gereksinimleri ve önkoşulları, [Linux sanal makineleri Için Azure disk şifrelemesi](../../linux/disk-encryption-overview.md#additional-vm-requirements)bölümünde belgelenmiştir.

Azure disk şifrelemesi uygulama adımları, Linux VM 'lerinde ve ilgili makalelerde [Azure disk şifrelemesi senaryolarında](../../linux/disk-encryption-linux.md) bulunabilir.

## <a name="additional-information"></a>Ek bilgiler

- Bu teklif için etkinleştirilmemiş bir abonelikte VM sağlamaya çalışırsanız aşağıdaki hatayı alırsınız:

    ```
    "Offer with PublisherId: redhat, OfferId: rhel-byos, PlanId: rhel-lvm75 is private and can not be purchased by subscriptionId: GUID"
    ```

    Bu durumda, aboneliğinizi etkinleştirmek için Microsoft veya Red Hat ile iletişim kurun.

- Bir RHEL BYOS görüntüsünden bir anlık görüntüyü değiştirir ve bu özel görüntüyü [paylaşılan görüntü galerisine](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries)yayımlamayı denerseniz, anlık görüntünün özgün kaynağıyla eşleşen plan bilgilerini sağlamanız gerekir. Örneğin, komut şöyle görünebilir:

    ```azurecli
    az vm create –image \
    "/subscriptions/GUID/resourceGroups/GroupName/providers/Microsoft.Compute/galleries/GalleryName/images/ImageName/versions/1.0.0" \
    -g AnotherGroupName --location EastUS2 -n VMName \
    --plan-publisher redhat --plan-product rhel-byos --plan-name rhel-lvm75
    ```
    Yukarıdaki son satırdaki plan parametrelerine göz önünde edin.

    [Azure disk şifrelemesi](#encrypt-red-hat-enterprise-linux-bring-your-own-subscription-gold-images) özel görüntülerde desteklenmez.

- RHEL BYOS görüntülerinden sanal makineler sağlamak için Otomasyon kullanıyorsanız, yukarıda gösterildiğine benzer plan parametreleri sağlamanız gerekir. Örneğin, Terrayform kullanıyorsanız, plan bilgilerini bir [plan bloğunda](https://www.terraform.io/docs/providers/azurerm/r/virtual_machine.html#plan)sağlarsınız.

## <a name="next-steps"></a>Sonraki adımlar
- Bulut erişimi için adım adım kılavuzlar ve program ayrıntıları, [Red Hat bulut erişimi belgelerinde bulunabilir.](https://access.redhat.com/documentation/en-us/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/index)
- [Azure Red Hat güncelleştirme altyapısı](./redhat-rhui.md)hakkında daha fazla bilgi edinin.
- Azure 'daki tüm Red Hat görüntüleri hakkında daha fazla bilgi edinmek için [Belgeler sayfasına](./redhat-images.md)gidin.
- Tüm RHEL sürümleri için Red Hat destek ilkeleriyle ilgili bilgiler [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfasında bulunabilir.
- [Red Hat belgelerinde](https://access.redhat.com/documentation/en-us/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/using_red_hat_gold_images#con-gold-image-azure)RHEL Gold görüntülerine ek belgeler bulabilirsiniz.
