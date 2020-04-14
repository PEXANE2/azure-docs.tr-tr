---
title: Red Hat Enterprise Linux kendi aboneliğinizi getirazure görüntüleri | Microsoft Dokümanlar
description: Azure'da Red Hat Enterprise Linux için kendi abonelik resimlerinizi getir ini öğrenin.
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
ms.openlocfilehash: 9ab578b4b688c02c9150dfb23fce53fbb82df405
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273180"
---
# <a name="red-hat-enterprise-linux-bring-your-own-subscription-gold-images-in-azure"></a>Red Hat Enterprise Linux Azure'da kendi aboneliğinizi getirin Gold Images

Red Hat Enterprise Linux (RHEL) görüntüleri Azure'da kendi aboneliğinizi (BYOS) (Red Hat Gold Image) modeliyle olabildiğince öde veya getir (Red Hat Gold Image) modeli yle kullanılabilir. Bu makalede, Azure'daki Red Hat Gold Images'a genel bir bakış sağlanmaktadır.

>[!NOTE]
> RHEL BYOS Gold Görüntüleri Azure Genel (ticari) ve Azure Genel bulutlarda kullanılabilir. Azure Çin veya Azure Kara Orman bulutlarında kullanılamaz.

## <a name="important-points-to-consider"></a>Göz önünde bulundurulması gereken önemli noktalar

- Bu programda sağlanan Red Hat Gold Images, Azure Marketi'ndeki RHEL'in istediğiniz kadar öde görüntülerine benzer üretime hazır RHEL görüntülerdir.
- Görüntüler, Azure'daki Red [Hat Enterprise Linux görüntülerinde](./redhat-images.md)açıklanan geçerli ilkeleri izler.
- Standart destek ilkeleri, bu görüntülerden oluşturulan VM'ler için geçerlidir.
- Red Hat Gold Images'dan sağlanan VM'ler, RHEL pay-as-you-go görüntüleriyle ilişkili RHEL ücretlerini taşımaz.
- Görüntüler inanılmamış. Doğrudan Red Hat'ten güncellemeler almak için VM'lere kaydolmak ve abone olmak için Red Hat Subscription-Manager'ı kullanmanız gerekir.
- Şu anda BYOS ile Linux görüntüleri için istediğiniz kadar öde faturalandırma modelleri arasında dinamik olarak geçiş yapmak mümkün değildir. Faturalandırma modelini değiştirmek için VM'yi ilgili görüntüden yeniden dağıtmanız gerekir.

>[!NOTE]
> Nesil 2 RHEL BYOS görüntüleri şu anda pazar teklifi aracılığıyla mevcut değildir. Generation 2 RHEL BYOS görüntüsüne ihtiyacınız varsa, Red Hat abonelik yönetiminde Ki Bulut Erişimi panosunu ziyaret edin. Daha fazla bilgi için [Red Hat belgelerine](https://access.redhat.com/articles/4847681)bakın.

## <a name="requirements-and-conditions-to-access-the-red-hat-gold-images"></a>Red Hat Gold Images'a erişmek için gerekli şartlar ve koşullar

1. Red Hat [Cloud Access program](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) koşullarını yakından bilin. [Red Hat Subscription-Manager](https://access.redhat.com/management/cloud)adresinde Bulut Erişimi için Red Hat aboneliklerinizi etkinleştirin. Bulut Erişimi için kaydedilecek Azure aboneliklerine sahip olmanız gerekir.

1. Bulut Erişimi için etkinleştirdiğiniz Red Hat abonelikleri uygunluk gereksinimlerini karşılıyorsa, Gold Image erişimi için Azure abonelikleriniz otomatik olarak etkinleştirilir.

### <a name="expected-time-for-image-access"></a>Görüntü erişimi için beklenen süre

Bulut Erişimi etkinleştirme adımlarını tamamladıktan sonra Red Hat, Red Hat Gold Images için uygunluğunuzu doğrular. Doğrulama başarılı olursa, üç saat içinde Altın Görüntüler'e erişebilirsiniz.

## <a name="use-the-red-hat-gold-images-from-the-azure-portal"></a>Azure portalındaki Red Hat Gold Görüntülerini kullanma

1. Azure aboneliğiniz Red Hat Gold Görüntüleri'ne erişinden sonra bunları [Azure portalında](https://portal.azure.com)bulabilirsiniz. **Kaynak** > Oluştur'a git**Tümlerini Gör.**

1. Sayfanın üst kısmında özel teklifleriniz olduğunu görürsünüz.

    ![Pazar yeri özel teklifleri](./media/rhel-byos-privateoffers.png)

1. Mor bağlantıyı seçin veya özel tekliflerinizi görmek için sayfanın altına doğru ilerleyin.

1. UI'deki hükmün geri kalanı, mevcut diğer Red Hat görüntülerinden farklı değildir. RHEL sürümünüzü seçin ve VM'inizi sağlamak için istemleri izleyin. Bu işlem, son adımda görüntünün koşullarını kabul etmenizi de sağlar.

>[!NOTE]
>Şimdiye kadar ki bu adımlar, programlı dağıtım için Red Hat Gold Image'ınızı etkinleştirmez. "Ek bilgiler" bölümünde açıklandığı gibi ek bir adım gereklidir.

Bu belgenin geri kalanı, görüntüdeki koşulları sağlamak ve kabul etmek için CLI yöntemine odaklanır. UI ve CLI, nihai sonuç (verilen bir RHEL Gold Image VM) söz konusu olduğunda tamamen değiştirilebilir.

## <a name="use-the-red-hat-gold-images-from-the-azure-cli"></a>Azure CLI'deki Red Hat Gold Görüntülerini Kullanma

Aşağıdaki yönergeler, Azure CLI'yi kullanarak bir RHEL VM için ilk dağıtım işleminde size yol sunar. Bu yönergeler, [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)yüklü olduğunuzu varsayar.

>[!IMPORTANT]
>Yayımcıdaki tüm küçük harfleri kullandığınızdan emin olun, aşağıdaki komutların tümü için teklif, plan ve resim başvuruları.

1. İstediğiniz abonelikte olup olmadığınızı kontrol edin.

    ```azurecli
    az account show -o=json
    ```

1. Red Hat Gold Image VM'iniz için bir kaynak grubu oluşturun.

    ```azurecli
    az group create --name <name> --location <location>
    ```

1. Resim terimlerini kabul edin.

    ```azurecli
    az vm image terms accept --publisher redhat --offer rhel-byos --plan <SKU value here> -o=jsonc

    # Example:
    az vm image terms accept --publisher redhat --offer rhel-byos --plan rhel-lvm75 -o=jsonc

    OR

    az vm image terms accept --urn RedHat:rhel-byos:rhel-lvm8:8.0.20190620
    ```

    >[!NOTE]
    >Bu koşulların Azure *aboneliği başına bir kez, görüntü Başına SKU'nun*kabul edilmesi gerekir.

1. (İsteğe bağlı) VM dağıtımınızı aşağıdaki komutla doğrulayın:

    ```azurecli
    az vm create -n <VM name> -g <resource group name> --image <image urn> --validate

    # Example:
    az vm create -n rhel-byos-vm -g rhel-byos-group --image RedHat:rhel-byos:rhel-lvm75:7.5.20190620
    ```

1. `--validate` VM'nizi, bağımsız değişken olmadan önceki örnekte gösterildiği komutu çalıştırarak sağlama.

    ```azurecli
    az vm create -n <VM name> -g <resource group name> --image <image urn> --validate
    ```

1. VM'nize SSH ve entitledolmayan bir görüntüye sahip olduğunuzu doğrulayın. Bu adımı yapmak `sudo yum repolist`için çalıştırın. RHEL 8 için. `sudo dnf repolist` Çıktı, VM'yi Red Hat'e kaydettirmek için Abonelik Yöneticisi'ni kullanmanızı ister.

>[!NOTE]
>RHEL `dnf` 8'de `yum` ve değiştirilebilir. Daha fazla bilgi için [RHEL 8 yönetici kılavuzuna](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/packaging_and_distributing_software/index)bakın.

## <a name="use-the-red-hat-gold-images-from-powershell"></a>PowerShell'den Red Hat Gold Görüntülerini Kullanın

Aşağıdaki komut dosyası bir örnektir. Kaynak grubunu, konumu, VM adını, giriş bilgilerini ve diğer değişkenleri seçtiğiniz yapılandırmayla değiştirin. Yayımcı ve plan bilgileri küçük olmalıdır.

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

## <a name="encrypt-red-hat-enterprise-linux-bring-your-own-subscription-gold-images"></a>Şifrele Red Hat Enterprise Linux getirmek-your-own-abonelik Gold Images

Red Hat Enterprise Linux BYOS Gold [Images, Azure Disk Şifrelemesi](../../linux/disk-encryption-overview.md)ile güvence altına alınabilir. Şifrelemeyi etkinleştirebilmeniz için aboneliğin kaydedilmesi *gerekir.* RHEL BYOS Gold Image'ın nasıl kaydedilen hakkında daha fazla bilgi için [Red Hat Subscription-Manager kullanarak red hat müşteri portalına nasıl kaydolunve abone olunur.](https://access.redhat.com/solutions/253273) Aktif bir Red Hat aboneliğiniz varsa, [Red Hat Müşteri Portalı Etkinleştirme Anahtarlarını Oluşturma'yı](https://access.redhat.com/articles/1378093)da okuyabilirsiniz.

Azure Disk Şifreleme, Red [Hat özel görüntülerde](../../linux/redhat-create-upload-vhd.md)desteklenmez. [Linux VM'leri için Azure Disk](../../linux/disk-encryption-overview.md#additional-vm-requirements)Şifrelemesi'nde ek Azure Disk Şifreleme gereksinimleri ve ön koşullar belgelenmiştir.

Azure Disk Şifrelemesi'ni uygulamak için adımlar için [Linux VM'lerinde](../../linux/disk-encryption-linux.md) ve ilgili makalelerde Azure Disk Şifreleme senaryolarına bakın.

## <a name="additional-information"></a>Ek bilgiler

- Bu teklif için etkinleştirilmeyen bir abonelikte VM sağlamayı denerseniz, aşağıdaki iletiyi alırsınız:

    ```
    "Offer with PublisherId: redhat, OfferId: rhel-byos, PlanId: rhel-lvm75 is private and can not be purchased by subscriptionId: GUID"
    ```

    Bu durumda, aboneliğinizi etkinleştirmek için Microsoft veya Red Hat'e başvurun.

- Bir anlık görüntüyü bir RHEL BYOS görüntüsünden değiştirir ve bu özel görüntüyü [Paylaşılan Görüntü Galerisi'nde](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries)yayımlamaya çalışırsanız, anlık görüntünün özgün kaynağıyla eşleşen plan bilgileri sağlamanız gerekir. Örneğin, komut şu şekilde görünebilir:

    ```azurecli
    az vm create –image \
    "/subscriptions/GUID/resourceGroups/GroupName/providers/Microsoft.Compute/galleries/GalleryName/images/ImageName/versions/1.0.0" \
    -g AnotherGroupName --location EastUS2 -n VMName \
    --plan-publisher redhat --plan-product rhel-byos --plan-name rhel-lvm75
    ```

    Son satırdaki plan parametrelerine dikkat edin.

    [Azure Disk Şifrelemesi](#encrypt-red-hat-enterprise-linux-bring-your-own-subscription-gold-images) özel görüntülerde desteklenmez.

- RHEL BYOS görüntülerinden VM'ler sağlamak için otomasyon kullanıyorsanız, örnek komutlarda gösterilene benzer plan parametreleri sağlamanız gerekir. Örneğin, Terraform kullanıyorsanız, plan bilgilerini bir [plan bloğunda](https://www.terraform.io/docs/providers/azurerm/r/virtual_machine.html#plan)sağlarsınız.

## <a name="next-steps"></a>Sonraki adımlar

- Red Hat Cloud Access hakkında daha fazla bilgi [Red Hat genel bulut belgeleri](https://access.redhat.com/public-cloud) mevcuttur
- Bulut Erişimi için adım adım kılavuzlar ve program ayrıntıları için [Red Hat Cloud Access belgelerine](https://access.redhat.com/documentation/en-us/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/index)bakın.
- Red Hat Update Altyapısı hakkında daha fazla bilgi edinmek için [Azure Red Hat Update Infrastructure](./redhat-rhui.md)'a bakın.
- Azure'daki tüm Red Hat resimleri hakkında daha fazla bilgi edinmek için [belgeler sayfasına](./redhat-images.md)bakın.
- RHEL'in tüm sürümleri için Red Hat destek politikaları hakkında daha fazla bilgi için [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfasına bakın.
- RHEL Gold Images ile ilgili ek belgeler için [Red Hat belgelerine](https://access.redhat.com/documentation/en-us/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/using_red_hat_gold_images#con-gold-image-azure)bakın.
