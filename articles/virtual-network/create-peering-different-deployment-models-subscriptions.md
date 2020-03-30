---
title: Azure sanal ağ eşlemesi - farklı dağıtım modelleri -farklı abonelikler
titlesuffix: Azure Virtual Network
description: Farklı Azure aboneliklerinde bulunan farklı Azure dağıtım modelleri aracılığıyla oluşturulan sanal ağlar arasında sanal ağ oluşturmayı öğrenin.
services: virtual-network
documentationcenter: ''
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: kumud
ms.reviewer: anavin
ms.openlocfilehash: 6823514e284f75f0580578dcabaa1b1bdcbe2f59
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80239831"
---
# <a name="create-a-virtual-network-peering---different-deployment-models-and-subscriptions"></a>Sanal ağ eşlemeoluşturma - farklı dağıtım modelleri ve abonelikleri

Bu eğitimde, farklı dağıtım modelleri aracılığıyla oluşturulan sanal ağlar arasında sanal ağ eşleme oluşturmayı öğrenirsiniz. Sanal ağlar farklı aboneliklerde bulunur. İki sanal ağa bakmak, farklı sanal ağlardaki kaynakların, kaynaklar aynı sanal ağdaymış gibi aynı bant genişliği ve gecikme siyle birbirleriyle iletişim kurmasını sağlar. [Sanal ağ eşleme](virtual-network-peering-overview.md)hakkında daha fazla bilgi edinin.

Sanal ağ eşlemi oluşturma adımları, sanal ağların aynı veya farklı aboneliklerde olup olmadığına ve sanal ağların hangi [Azure dağıtım modeli](../azure-resource-manager/management/deployment-models.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nde oluşturulduğuna bağlı olarak farklıdır. Aşağıdaki tablodan senaryoyu tıklatarak diğer senaryolarda sanal ağ eşleme oluşturmayı öğrenin:

|Azure dağıtım modeli  | Azure aboneliği  |
|--------- |---------|
|[Her ikisi de Resource Manager](tutorial-connect-virtual-networks-portal.md) |Aynı|
|[Her ikisi de Resource Manager](create-peering-different-subscriptions.md) |Farklı|
|[Biri Resource Manager, diğeri klasik](create-peering-different-deployment-models.md) |Aynı|

Klasik dağıtım modeli aracılığıyla dağıtılan iki sanal ağ arasında sanal ağ eşlemi oluşturulamaz. Bu öğretici, aynı bölgede bulunan sanal ağları kullanır. Bu öğretici, aynı bölgedeki sanal ağları eşler. Ayrıca, desteklenen farklı [bölgelerdeki](virtual-network-manage-peering.md#cross-region)sanal ağları da eşleyebilirsiniz. Sanal ağlara bakmadan [önce, izleme gereksinimleri ve kısıtlamaları](virtual-network-manage-peering.md#requirements-and-constraints) hakkında bilgi edinmeniz önerilir.

Farklı aboneliklerde bulunan sanal ağlar arasında sanal ağ eşleme oluştururken, aboneliklerin her ikisi nin de aynı Azure Etkin Dizin kiracıyla ilişkilendirilmesi gerekir. Azure Etkin Dizin kiracınız yoksa, hızlı bir şekilde [bir tane oluşturabilirsiniz.](../active-directory/develop/quickstart-create-new-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-new-azure-ad-tenant) Azure [VPN Ağ Geçidi'ni](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)kullanarak farklı aboneliklerde sanal ağları ve farklı Azure Active Directory kiracılarını bağlayabilirsiniz.

Sanal ağ eşlemeoluşturmak için [Azure portalını,](#portal)Azure [komut satırı arabirimini](#cli) (CLI) veya Azure [PowerShell'i](#powershell) kullanabilirsiniz. Seçtiğiniz aracı kullanarak sanal ağ eşleme oluşturmak için doğrudan adımlara gitmek için önceki araç bağlantılarından herhangi birini tıklatın.

## <a name="create-peering---azure-portal"></a><a name="portal"></a>Akran oluşturma - Azure portalı

Bu öğretici, her abonelik için farklı hesaplar kullanır. Her iki abonelik için izinleri olan bir hesap kullanıyorsanız, tüm adımlar için aynı hesabı kullanabilir, portaldan çıkış için adımları atlayabilir ve sanal ağlara başka bir kullanıcı izni atama adımlarını atlayabilirsiniz.

1. [Azure portalında](https://portal.azure.com) UserA olarak oturum açın. Oturum açtığın hesabın sanal ağ eşlemesi oluşturmak için gerekli izinlere sahip olması gerekir. İzinlerin listesi için [Sanal ağ izleme izinlerine](virtual-network-manage-peering.md#permissions)bakın.
2. + **Yeni' yi**tıklatın, **Ağ'ı**tıklatın, ardından **Sanal ağ'ı**tıklatın.
3. Sanal **ağ** oluştur bıçağında, aşağıdaki ayarlar için değerleri girin veya seçin, ardından **Oluştur'u**tıklatın:
    - **Ürün :** *myVnetA*
    - **Adres alanı**: *10.0.0.0/16*
    - **Subnet adı**: *varsayılan*
    - **Alt net adres aralığı**: *10.0.0.0/24*
    - **Abonelik**: Abonelik A'yı seçin.
    - **Kaynak grubu**: **Yeni Oluştur'u** seçin ve *myResourceGroupA'ya* girin
    - **Yer**: *Doğu ABD*
4. Portalın üst kısmındaki **Arama kaynakları** kutusuna *myVnetA*yazın. Arama sonuçlarında göründüğünde **myVnetA'yı** tıklatın. **MyVnetA** sanal ağı için bir bıçak görünür.
5. Görünen **myVnetA** bıçağında, bıçağın sol tarafındaki dikey seçenekler listesinden **Access denetimine (IAM)** tıklayın.
6. **myVnetA - Görünen erişim denetimi (IAM)** bıçak, tıklayın **+ Rol atama ekle**.
7. Görünen **Rol Atama Ekle** bıçağında, **Rol** kutusunda Ağ **katkıda bulunanı** seçin.
8. **Select** kutusunda UserB'yi seçin veya aramak için UserB'nin e-posta adresini yazın. Gösterilen kullanıcıların listesi, eşlemi ayarladığınız sanal ağla aynı Azure Etkin Dizin kiracısından dır. Listede göründüğünde UserB'yi tıklatın.
9. **Kaydet**'e tıklayın.
10. Portaldan UserA olarak giriş yapın, ardından UserB olarak oturum açın.
11. **Pazar Yeri Arama** kutusuna + **Yeni,** *Sanal ağ* yazın, ardından arama sonuçlarında **Sanal ağı** tıklatın.
12. Görünen **Sanal Ağ** bıçak kutusunda, dağıtım **modeli seç** kutusunda **Klasik'i** seçin ve ardından **Oluştur'u**tıklatın.
13. Görünen sanal ağ oluştur (klasik) kutusuna aşağıdaki değerleri girin:

    - **Adı**: *myVnetB*
    - **Adres alanı**: *10.1.0.0/16*
    - **Subnet adı**: *varsayılan*
    - **Alt net adres aralığı**: *10.1.0.0/24*
    - **Abonelik**: Abonelik B'yi seçin.
    - **Kaynak grubu**: **Yeni Oluştur'u** seçin ve *myResourceGroupB'a* girin
    - **Yer**: *Doğu ABD*

14. Portalın üst kısmındaki **Arama kaynakları** kutusuna *myVnetB*yazın. Arama sonuçlarında göründüğünde **myVnetB'yi** tıklatın. **MyVnetB** sanal ağı için bir bıçak görünür.
15. Görünen **myVnetB** bıçağında, bıçağın sol tarafındaki dikey seçenekler listesinden **Özellikler'i** tıklatın. Daha sonraki bir adımda kullanılan **KAYNAK Kimliğini**kopyalayın. Kaynak kimliği aşağıdaki örneğe benzer:`/subscriptions/<Subscription ID>/resourceGroups/myResourceGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB`
16. MyVnetB için 5-9 adımlarını tamamlayın, **8.**
17. Portaldan UserB olarak giriş yapın ve UserA olarak giriş yapın.
18. Portalın üst kısmındaki **Arama kaynakları** kutusuna *myVnetA*yazın. Arama sonuçlarında göründüğünde **myVnetA'yı** tıklatın. **MyVnet** sanal ağı için bir bıçak görünür.
19. **myVnetA'yı**tıklatın.
20. Görünen **myVnetA** bıçağında, bıçağın sol tarafındaki dikey seçenekler listesinden **Peerings'i** tıklatın.
21. **myVnetA - Görünen Peerings** bıçak, tıklayın **+ Ekle**
22. Görünen, giren veya aşağıdaki seçenekleri seçen **Eşleme** Ekle bıçağında **Tamam'ı**tıklatın:
     - **Adı**: *myVnetAToMyVnetB*
     - **Sanal ağ dağıtım modeli**: **Select Classic**.
     - **Kaynak kimliğimi biliyorum:** Bu kutuyu işaretleyin.
     - **Kaynak Kimliği**: myVnetB'nin kaynak kimliğini adım 15'ten girin.
     - **Sanal ağ erişimine izin verin:** **Etkin'in** seçildiğinden emin olun.
    Bu öğreticide başka hiçbir ayar kullanılmaz. Tüm eşleme ayarları hakkında bilgi edinmek için [sanal ağ eşlemelerini yönet'i](virtual-network-manage-peering.md#create-a-peering)okuyun.
23. Önceki adımda **Tamam'ı** tıklattıktan **sonra, Bakış** ekle bıçağı kapanır ve **myVnetA - Peerings** bıçağını yeniden görürsünüz. Birkaç saniye sonra, oluşturduğunuz bakış bıçakta görünür. **Bağlı** **myVnetAToMyVnetB** oluşturduğunuz peering için **PEERING DURUM** sütununda listelenir. Akranşimdi kurulmuştur. Sanal ağa (klasik) sanal ağa (Kaynak Yöneticisi) eşmeye gerek yoktur.

    Sanal ağda oluşturduğunuz tüm Azure kaynakları artık IP adresleri aracılığıyla birbirleriyle iletişim kurabilir. Sanal ağlar için varsayılan Azure ad çözünürlüğünü kullanıyorsanız, sanal ağlardaki kaynaklar sanal ağlardaki adları çözümlemez. Bir eşlemede sanal ağlardaki adları çözümlemek istiyorsanız, kendi DNS sunucunuzu oluşturmanız gerekir. [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğünü](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)nasıl ayarlayınızı öğrenin.

24. **İsteğe Bağlı**: Sanal makineler oluşturma bu öğreticide yer almasa da, bağlantının doğrulanması için her sanal ağda sanal bir makine oluşturabilir ve bir sanal makineden diğerine bağlanabilirsiniz.
25. **İsteğe Bağlı**: Bu eğitimde oluşturduğunuz kaynakları silmek için, bu makalenin [Kaynakları Sil](#delete-portal) bölümündeki adımları tamamlayın.

## <a name="create-peering---azure-cli"></a><a name="cli"></a>Akran oluşturma - Azure CLI

Bu öğretici, her abonelik için farklı hesaplar kullanır. Her iki abonelik için izinleri olan bir hesap kullanıyorsanız, tüm adımlar için aynı hesabı kullanabilir, Azure'dan çıkış için adımları atlayabilir ve kullanıcı rol atamaları oluşturan komut dosyası satırlarını kaldırabilirsiniz. KullanıcıA UserB@azure.com ve UserB için kullandığınız kullanıcı adlarını değiştirin UserA@azure.com ve aşağıdaki komut dosyalarının tümlerinde değiştirin. Azure klasik CLI ve Azure CLI'yi kullanarak aşağıdaki adımları tamamlayın. Aşağıdaki adımlardan herhangi birinde **Try it** düğmesini seçerek veya klasik CLI ve [CLI'yi](/cli/azure/install-classic-cli) yükleyerek ve komutları yerel bilgisayarınızda çalıştırarak Azure Bulut Su Şurup'undaki adımları tamamlayabilirsiniz. [CLI](/cli/azure/install-azure-cli)

1. Bulut Kabuğu'nu kullanıyorsanız, Bulut Kabuğu sizi otomatik olarak Azure'da imzaladığından adım 2'ye geçin. Komutu kullanarak bir komut oturumu `azure login` açın ve Azure'da oturum açın.
2. Komutu girerek Hizmet Yönetimi modunda klasik `azure config mode asm` CLI çalıştırın.
3. Sanal ağ (klasik) oluşturmak için aşağıdaki klasik CLI komutunu girin:

    ```console
    azure network vnet create --vnet myVnetB --address-space 10.1.0.0 --cidr 16 --location "East US"
    ```

4. Kalan adımlar Azure CLI (klasik CLI değil) ile bir bash kabuk kullanılarak tamamlanmalıdır.
5. Aşağıdaki komut dosyasını bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Abonelik `<SubscriptionB-Id>` kimliğinizle değiştirin. Abonelik Kimliğinizi bilmiyorsanız, komutu `az account show` girin. Çıktıdaki **id** değeri abonelik Kimliğinizdir. Değiştirilmiş komut dosyasını kopyalayın, CLI oturumunuza yapıştırın ve sonra basın. `Enter`

    ```azurecli-interactive
    az role assignment create \
      --assignee UserA@azure.com \
      --role "Classic Network Contributor" \
      --scope /subscriptions/<SubscriptionB-Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

    Adım 4'te sanal ağı (klasik) oluşturduğunuzda, Azure *Varsayılan Ağ* kaynak grubunda sanal ağı oluşturdu.
6. UserB'yi Azure'dan kaydedin ve CLI'de UserA olarak oturum açın.
7. Kaynak grubu ve sanal ağ (Kaynak Yöneticisi) oluşturun. Aşağıdaki komut dosyasını kopyalayın, CLI oturumunuza `Enter`yapıştırın ve sonra .

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout the script.
    rgName="myResourceGroupA"
    location="eastus"

    # Create a resource group.
    az group create \
      --name $rgName \
      --location $location

    # Create virtual network A (Resource Manager).
    az network vnet create \
      --name myVnetA \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.0.0.0/16

    # Get the id for myVnetA.
    vNetAId=$(az network vnet show \
      --resource-group $rgName \
      --name myVnetA \
      --query id --out tsv)

    # Assign UserB permissions to myVnetA.
    az role assignment create \
      --assignee UserB@azure.com \
      --role "Network Contributor" \
      --scope $vNetAId
    ```

8. Farklı dağıtım modelleri aracılığıyla oluşturulan iki sanal ağ arasında sanal ağ eşleme oluşturun. Aşağıdaki komut dosyasını bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Abonelik `<SubscriptionB-id>` Kimliğinizle değiştirin. Abonelik Kimliğinizi bilmiyorsanız, komutu `az account show` girin. Çıktıdaki **id** değeri abonelik Id'nizdir. Azure, *Varsayılan Ağ*adlı bir kaynak grubunda adım 4'te oluşturduğunuz sanal ağı (klasik) oluşturdu. CLI oturumunuza değiştirilmiş komut dosyasını `Enter`yapıştırın ve sonra .'a basın.

    ```azurecli-interactive
    # Peer VNet1 to VNet2.
    az network vnet peering create \
      --name myVnetAToMyVnetB \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --remote-vnet-id  /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB \
      --allow-vnet-access
    ```

9. Komut dosyası yürütülmeden sonra, sanal ağ (Kaynak Yöneticisi) için eşleme gözden geçirin. Aşağıdaki komut dosyasını kopyalayın ve CLI oturumunuza yapıştırın:

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --output table
    ```

    Çıktı, **PeeringState** sütununda **Bağlı'yı** gösterir.

    Sanal ağda oluşturduğunuz tüm Azure kaynakları artık IP adresleri aracılığıyla birbirleriyle iletişim kurabilir. Sanal ağlar için varsayılan Azure ad çözünürlüğünü kullanıyorsanız, sanal ağlardaki kaynaklar sanal ağlardaki adları çözümlemez. Bir eşlemede sanal ağlardaki adları çözümlemek istiyorsanız, kendi DNS sunucunuzu oluşturmanız gerekir. [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğünü](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)nasıl ayarlayınızı öğrenin.

10. **İsteğe Bağlı**: Sanal makineler oluşturma bu öğreticide yer almasa da, bağlantının doğrulanması için her sanal ağda sanal bir makine oluşturabilir ve bir sanal makineden diğerine bağlanabilirsiniz.
11. **İsteğe Bağlı**: Bu eğitimde oluşturduğunuz kaynakları silmek için, bu makaledeki [kaynakları sil](#delete-cli) adımlarını tamamlayın.

## <a name="create-peering---powershell"></a><a name="powershell"></a>Eşleme oluşturma - PowerShell

Bu öğretici, her abonelik için farklı hesaplar kullanır. Her iki abonelik için izinleri olan bir hesap kullanıyorsanız, tüm adımlar için aynı hesabı kullanabilir, Azure'dan çıkış için adımları atlayabilir ve kullanıcı rol atamaları oluşturan komut dosyası satırlarını kaldırabilirsiniz. KullanıcıA UserB@azure.com ve UserB için kullandığınız kullanıcı adlarını değiştirin UserA@azure.com ve aşağıdaki komut dosyalarının tümlerinde değiştirin. 

1. PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) ve [Az](https://www.powershellgallery.com/packages/Az) modüllerinin en son sürümünü yükleyin. Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin.
2. PowerShell oturumu başlatın.
3. PowerShell'de, komutu girerek UserB aboneliğine `Add-AzureAccount` UserB olarak giriş yapın. Oturum açtığın hesabın sanal ağ eşlemesi oluşturmak için gerekli izinlere sahip olması gerekir. İzinlerin listesi için [Sanal ağ izleme izinlerine](virtual-network-manage-peering.md#permissions)bakın.
4. PowerShell ile sanal ağ (klasik) oluşturmak için, yeni bir ağ yapılandırma dosyası oluşturmanız veya değiştirmeniz gerekir. Ağ yapılandırma dosyalarını nasıl [dışa aktarıp aktartığın, güncelleştirecek ve içe aktarılamayı](virtual-networks-using-network-configuration-file.md)öğrenin. Dosya, bu öğreticide kullanılan sanal ağ için aşağıdaki **VirtualNetworkSite** öğesini içermelidir:

    ```xml
    <VirtualNetworkSite name="myVnetB" Location="East US">
      <AddressSpace>
        <AddressPrefix>10.1.0.0/16</AddressPrefix>
      </AddressSpace>
      <Subnets>
        <Subnet name="default">
          <AddressPrefix>10.1.0.0/24</AddressPrefix>
        </Subnet>
      </Subnets>
    </VirtualNetworkSite>
    ```

    > [!WARNING]
    > Değiştirilen bir ağ yapılandırma dosyasını almak, aboneliğinizdeki varolan sanal ağlarda (klasik) değişikliklere neden olabilir. Yalnızca önceki sanal ağı eklediğinizden ve aboneliğinizden varolan sanal ağları değiştirmediğinizden veya kaldırmadığınızdan emin olun. 

5. Komutu girerek Kaynak Yöneticisi komutlarını kullanmak için UserB olarak `Connect-AzAccount` UserB aboneliğinde oturum açın.
6. Sanal ağa UserA izinleri atama B. Aşağıdaki komut dosyasını bilgisayarınızdaki `<SubscriptionB-id>` bir metin düzenleyicisine kopyalayın ve B abonelik kimliğiyle değiştirin. Abonelik Kimliğini bilmiyorsanız, görüntülemek için `Get-AzSubscription` komutu girin. İade edilen çıktıdaki **Id** değeri abonelik kimliğinizdir. Azure, *Varsayılan Ağ*adlı bir kaynak grubunda adım 4'te oluşturduğunuz sanal ağı (klasik) oluşturdu. Komut dosyasını çalıştırmak için değiştirilen komut dosyasını kopyalayın, PowerShell'e yapıştırın ve sonra 'ya basın. `Enter`

    ```powershell
    New-AzRoleAssignment `
      -SignInName UserA@azure.com `
      -RoleDefinitionName "Classic Network Contributor" `
      -Scope /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

7. Azure'dan UserB olarak giriş yapın ve komutu girerek UserA `Connect-AzAccount` aboneliğine UserA olarak giriş yapın. Oturum açtığın hesabın sanal ağ eşlemesi oluşturmak için gerekli izinlere sahip olması gerekir. İzinlerin listesi için [Sanal ağ izleme izinlerine](virtual-network-manage-peering.md#permissions)bakın.
8. Aşağıdaki komut dosyasını kopyalayarak, PowerShell'e yapıştırarak ve sonra şu tuşa basarak `Enter`sanal ağı (Kaynak Yöneticisi) oluşturun:

    ```powershell
    # Variables for common values
      $rgName='MyResourceGroupA'
      $location='eastus'

    # Create a resource group.
    New-AzResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network A.
    $vnetA = New-AzVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnetA' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location
    ```

9. UserB izinlerini myVnetA'ya atayın. Aşağıdaki komut dosyasını bilgisayarınızdaki bir metin `<SubscriptionA-Id>` düzenleyicisine kopyalayın ve Abonelik A kimliğiyle değiştirin. Abonelik Kimliğini bilmiyorsanız, görüntülemek için `Get-AzSubscription` komutu girin. İade edilen çıktıdaki **Id** değeri abonelik kimliğinizdir. Komut dosyasının değiştirilmiş sürümünü PowerShell'e yapıştırın ve yürütmek için basın. `Enter`

    ```powershell
    New-AzRoleAssignment `
      -SignInName UserB@azure.com `
      -RoleDefinitionName "Network Contributor" `
      -Scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

10. Aşağıdaki komut dosyasını bilgisayarınızdaki bir metin `<SubscriptionB-id>` düzenleyicisine kopyalayın ve B abonelik kimliğiyle değiştirin. myVnetA'yı myVNetB'ye göre görmek için değiştirilmiş komut dosyasını kopyalayın, PowerShell'e yapıştırın ve ardından basın. `Enter`

    ```powershell
    Add-AzVirtualNetworkPeering `
      -Name 'myVnetAToMyVnetB' `
      -VirtualNetwork $vnetA `
      -RemoteVirtualNetworkId /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

11. Aşağıdaki komut dosyasını kopyalayarak, PowerShell'e yapıştırarak ve 'ye basarak `Enter`myVnetA'nın görüntüdurumunu görüntüleyin.

    ```powershell
    Get-AzVirtualNetworkPeering `
      -ResourceGroupName $rgName `
      -VirtualNetworkName myVnetA `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Durum **Bağlı.** MyVnetB'den myVnetA'ya bakışı ayarladığınızda **Connected** olarak değişir.

    Sanal ağda oluşturduğunuz tüm Azure kaynakları artık IP adresleri aracılığıyla birbirleriyle iletişim kurabilir. Sanal ağlar için varsayılan Azure ad çözünürlüğünü kullanıyorsanız, sanal ağlardaki kaynaklar sanal ağlardaki adları çözümlemez. Bir eşlemede sanal ağlardaki adları çözümlemek istiyorsanız, kendi DNS sunucunuzu oluşturmanız gerekir. [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğünü](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)nasıl ayarlayınızı öğrenin.

12. **İsteğe Bağlı**: Sanal makineler oluşturma bu öğreticide yer almasa da, bağlantının doğrulanması için her sanal ağda sanal bir makine oluşturabilir ve bir sanal makineden diğerine bağlanabilirsiniz.
13. **İsteğe Bağlı**: Bu eğitimde oluşturduğunuz kaynakları silmek için, bu makaledeki [kaynakları sil](#delete-powershell) adımlarını tamamlayın.

## <a name="delete-resources"></a><a name="delete"></a>Kaynakları silme
Bu öğreticiyi tamamladığınızda, kullanım ücreti ödememek için öğreticide oluşturduğunuz kaynakları silmek isteyebilirsiniz. Kaynak grubunu silmek, kaynak grubundaki tüm kaynakları da siler.

### <a name="azure-portal"></a><a name="delete-portal"></a>Azure portalında

1. Portal arama kutusuna **myResourceGroupA'yı**girin. Arama sonuçlarında **myResourceGroupA'yı**tıklatın.
2. **myResourceGroupA** bıçağında **Sil** simgesini tıklatın.
3. Silme işlemini onaylamak için, **KAYNAK GRUBU Ad** türünün içinde **myResourceGroupA'yı**girin ve sonra **Sil'i**tıklatın.
4. Portalın üst kısmındaki **Arama kaynakları** kutusuna *myVnetB*yazın. Arama sonuçlarında göründüğünde **myVnetB'yi** tıklatın. **MyVnetB** sanal ağı için bir bıçak görünür.
5. **myVnetB** bıçağında **Sil'i**tıklatın.
6. Silme işlemini onaylamak için Sanal **Ağ Sil** kutusunda **Evet'i** tıklatın.

### <a name="azure-cli"></a><a name="delete-cli"></a>Azure CLI

1. Aşağıdaki komutla sanal ağı (Kaynak Yöneticisi) silmek için CLI'yi kullanarak Azure'da oturum açın:

   ```azurecli-interactive
   az group delete --name myResourceGroupA --yes
   ```

2. Aşağıdaki komutlarla sanal ağı (klasik) silmek için klasik CLI'yi kullanarak Azure'da oturum açın:

   ```console
   azure config mode asm

   azure network vnet delete --vnet myVnetB --quiet
   ```

### <a name="powershell"></a><a name="delete-powershell"></a>Powershell

1. PowerShell komut isteminde, sanal ağı (Kaynak Yöneticisi) silmek için aşağıdaki komutu girin:

   ```powershell
   Remove-AzResourceGroup -Name myResourceGroupA -Force
   ```

2. PowerShell ile sanal ağı (klasik) silmek için varolan bir ağ yapılandırma dosyasını değiştirmeniz gerekir. Ağ yapılandırma dosyalarını nasıl [dışa aktarıp aktartığın, güncelleştirecek ve içe aktarılamayı](virtual-networks-using-network-configuration-file.md)öğrenin. Bu öğreticide kullanılan sanal ağ için aşağıdaki VirtualNetworkSite öğesini kaldırın:

   ```xml
   <VirtualNetworkSite name="myVnetB" Location="East US">
     <AddressSpace>
       <AddressPrefix>10.1.0.0/16</AddressPrefix>
     </AddressSpace>
     <Subnets>
       <Subnet name="default">
         <AddressPrefix>10.1.0.0/24</AddressPrefix>
       </Subnet>
     </Subnets>
   </VirtualNetworkSite>
   ```

   > [!WARNING]
   > Değiştirilen bir ağ yapılandırma dosyasını almak, aboneliğinizdeki varolan sanal ağlarda (klasik) değişikliklere neden olabilir. Yalnızca önceki sanal ağı kaldırdığınızdan ve aboneliğinizden varolan diğer sanal ağları değiştirmediğinizden veya kaldırmadığınızdan emin olun. 

## <a name="next-steps"></a>Sonraki adımlar

- Üretim kullanımı için sanal ağ eşleme oluşturmadan önce önemli [sanal ağ izleme kısıtlamaları ve davranışları](virtual-network-manage-peering.md#requirements-and-constraints) hakkında kendinizi iyice tanıtın.
- Tüm [sanal ağ eşleme ayarları](virtual-network-manage-peering.md#create-a-peering)hakkında bilgi edinin.
- Sanal ağ eşlemeli [bir hub ve kollu ağ topolojisi oluşturmayı](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke#virtual-network-peering) öğrenin.