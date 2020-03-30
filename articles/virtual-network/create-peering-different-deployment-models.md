---
title: Bir Azure sanal ağ eşlemeoluşturma - farklı dağıtım modelleri - aynı abonelik | Microsoft Dokümanlar
description: Aynı Azure aboneliğinde bulunan farklı Azure dağıtım modelleri aracılığıyla oluşturulan sanal ağlar arasında sanal ağ oluşturmayı öğrenin.
services: virtual-network
documentationcenter: ''
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/15/2018
ms.author: kumud
ms.reviewer: anavin
ms.openlocfilehash: 61df13e78dc7115d4f4d45ab18b9ffdae107dc96
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77023268"
---
# <a name="create-a-virtual-network-peering---different-deployment-models-same-subscription"></a>Sanal ağ eşleme oluşturma - farklı dağıtım modelleri, aynı abonelik

Bu eğitimde, farklı dağıtım modelleri aracılığıyla oluşturulan sanal ağlar arasında sanal ağ eşleme oluşturmayı öğrenirsiniz. Her iki sanal ağ da aynı abonelikte bulunur. İki sanal ağa bakmak, farklı sanal ağlardaki kaynakların, kaynaklar aynı sanal ağdaymış gibi aynı bant genişliği ve gecikme siyle birbirleriyle iletişim kurmasını sağlar. [Sanal ağ eşleme](virtual-network-peering-overview.md)hakkında daha fazla bilgi edinin.

Sanal ağ eşlemi oluşturma adımları, sanal ağların aynı veya farklı aboneliklerde olup olmadığına ve sanal ağların hangi [Azure dağıtım modeli](../azure-resource-manager/management/deployment-models.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nde oluşturulduğuna bağlı olarak farklıdır. Aşağıdaki tablodan senaryoyu tıklatarak diğer senaryolarda sanal ağ eşleme oluşturmayı öğrenin:

|Azure dağıtım modeli  | Azure aboneliği  |
|--------- |---------|
|[Her ikisi de Resource Manager](tutorial-connect-virtual-networks-portal.md) |Aynı|
|[Her ikisi de Resource Manager](create-peering-different-subscriptions.md) |Farklı|
|[Biri Resource Manager, diğeri klasik](create-peering-different-deployment-models-subscriptions.md) |Farklı|

Klasik dağıtım modeli aracılığıyla dağıtılan iki sanal ağ arasında sanal ağ eşleme oluşturulamaz. Her ikisi de klasik dağıtım modeli aracılığıyla oluşturulan sanal ağları bağlamanız gerekiyorsa, sanal ağları bağlamak için bir Azure [VPN Ağ Geçidi](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) kullanabilirsiniz.

Bu öğretici, aynı bölgedeki sanal ağları eşler. Ayrıca, desteklenen farklı [bölgelerdeki](virtual-network-manage-peering.md#cross-region)sanal ağları da eşleyebilirsiniz. Sanal ağlara bakmadan [önce, izleme gereksinimleri ve kısıtlamaları](virtual-network-manage-peering.md#requirements-and-constraints) hakkında bilgi edinmeniz önerilir.

Sanal ağ eşlemeoluşturmak için Azure portalını, Azure [komut satırı arabirimini](#cli) (CLI), Azure [PowerShell'i](#powershell)veya Azure Kaynak Yöneticisi şablonunu kullanabilirsiniz. Seçtiğiniz aracı kullanarak sanal ağ eşleme oluşturmak için doğrudan adımlara gitmek için önceki araç bağlantılarından herhangi birini tıklatın.

## <a name="create-peering---azure-portal"></a>Akran oluşturma - Azure portalı

1. [Azure portalında](https://portal.azure.com)oturum açın. Oturum açtırdığınız hesap, sanal ağ eşlemesi oluşturmak için gerekli izinlere sahip olmalıdır. İzinlerin listesi için [Sanal ağ izleme izinlerine](virtual-network-manage-peering.md#requirements-and-constraints)bakın.
2. + **Yeni' yi**tıklatın, **Ağ'ı**tıklatın, ardından **Sanal ağ'ı**tıklatın.
3. Sanal **ağ** oluştur bıçağında, aşağıdaki ayarlar için değerleri girin veya seçin, ardından **Oluştur'u**tıklatın:
    - **Ürün :** *myVnet1*
    - **Adres alanı**: *10.0.0.0/16*
    - **Subnet adı**: *varsayılan*
    - **Alt net adres aralığı**: *10.0.0.0/24*
    - **Abonelik**: Aboneliğinizi seçin
    - **Kaynak grubu**: **Yeni Oluştur'u** seçin ve *myResourceGroup'a* girin
    - **Yer**: *Doğu ABD*
4. **+ Yeni** öğesine tıklayın. Pazar **Ara kutusuna** *Sanal ağ*yazın. Arama sonuçlarında göründüğünde **Sanal ağı** tıklatın.
5. Sanal **ağ** bıçak, **bir dağıtım modeli seç** kutusunda **Klasik'i** seçin ve ardından **Oluştur'u**tıklatın.
6. Sanal **ağ** oluştur bıçağında, aşağıdaki ayarlar için değerleri girin veya seçin, ardından **Oluştur'u**tıklatın:
    - **Ürün :** *myVnet2*
    - **Adres alanı**: *10.1.0.0/16*
    - **Subnet adı**: *varsayılan*
    - **Alt net adres aralığı**: *10.1.0.0/24*
    - **Abonelik**: Aboneliğinizi seçin
    - **Kaynak grubu**: **Varolan Kullan'ı** seçin ve *myResourceGroup'u* seçin
    - **Yer**: *Doğu ABD*
7. Portalın üst kısmındaki **Arama kaynakları** kutusuna *myResourceGroup*yazın. Arama sonuçlarında göründüğünde **myResourceGroup'u** tıklatın. **Kaynak grubum** kaynak grubu için bir bıçak görüntülenir. Kaynak grubu, önceki adımlarda oluşturulan iki sanal ağı tutar.
8. **myVNet1'e**tıklayın.
9. Görünen **myVnet1** bıçağında, bıçağın sol tarafındaki dikey seçenekler listesinden **Peerings'i** tıklatın.
10. **myVnet1 - Görünen Peerings** bıçak, tıklayın **+ Ekle**
11. Görünen, giren veya aşağıdaki seçenekleri seçen **Eşleme** Ekle bıçağında **Tamam'ı**tıklatın:
     - **Adı**: *myVnet1ToMyVnet2*
     - **Sanal ağ dağıtım modeli**: **Select Classic**.
     - **Abonelik**: Aboneliğinizi seçin
     - **Sanal ağ**: **Sanal ağ seçin'** i tıklatın, ardından **myVnet2'yi**tıklatın.
     - **Sanal ağ erişimine izin verin:** **Etkin'in** seçildiğinden emin olun.
    Bu öğreticide başka hiçbir ayar kullanılmaz. Tüm eşleme ayarları hakkında bilgi edinmek için [sanal ağ eşlemelerini yönet'i](virtual-network-manage-peering.md#create-a-peering)okuyun.
12. Önceki adımda **Tamam'ı** tıklattıktan **sonra, Bakış** ekle bıçağı kapanır ve **myVnet1 - Peerings** bıçağını yeniden görürsünüz. Birkaç saniye sonra, oluşturduğunuz bakış bıçakta görünür. **Bağlı** **myVnet1ToMyVnet2** oluşturduğunuz peering için **PEERING DURUM** sütununda listelenir.

    Akranşimdi kurulmuştur. Sanal ağda oluşturduğunuz tüm Azure kaynakları artık IP adresleri aracılığıyla birbirleriyle iletişim kurabilir. Sanal ağlar için varsayılan Azure ad çözünürlüğünü kullanıyorsanız, sanal ağlardaki kaynaklar sanal ağlardaki adları çözümleemez. Bir eşlemede sanal ağlardaki adları çözümlemek istiyorsanız, kendi DNS sunucunuzu oluşturmanız gerekir. [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğünü](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)nasıl ayarlayınızı öğrenin.
13. **İsteğe Bağlı**: Sanal makineler oluşturmak bu öğreticide yer almasa da, bağlantının doğrulanması için her sanal ağda sanal bir makine oluşturabilir ve bir sanal makineden diğerine bağlanabilirsiniz.
14. **İsteğe Bağlı**: Bu eğitimde oluşturduğunuz kaynakları silmek için, bu makalenin [Kaynakları Sil](#delete-portal) bölümündeki adımları tamamlayın.

## <a name="create-peering---azure-cli"></a><a name="cli"></a>Akran oluşturma - Azure CLI

Azure klasik CLI ve Azure CLI'yi kullanarak aşağıdaki adımları tamamlayın. Aşağıdaki adımlardan herhangi birinde **Try it** düğmesini seçerek veya klasik CLI ve [CLI'yi](/cli/azure/install-cli-version-1.0?toc=%2fazure%2fvirtual-network%2ftoc.json) yükleyerek ve komutları yerel bilgisayarınızda çalıştırarak Azure Bulut Su Şurup'undaki adımları tamamlayabilirsiniz. [CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json)

1. Bulut Kabuğu'nu kullanıyorsanız, Bulut Kabuğu sizi otomatik olarak Azure'da imzaladığından adım 2'ye geçin. Komutu kullanarak bir komut oturumu `azure login` açın ve Azure'da oturum açın.
2. Komutu girerek CLI'yi Hizmet `azure config mode asm` Yönetimi modunda çalıştırın.
3. Sanal ağ (klasik) oluşturmak için aşağıdaki komutu girin:

   ```azurecli-interactive
   azure network vnet create --vnet myVnet2 --address-space 10.1.0.0 --cidr 16 --location "East US"
   ```

4. Aşağıdaki bash CLI komut dosyasını klasik CLI'yi değil, CLI'yi kullanarak uygulayın. Windows bilgisayarda bash CLI komut dosyalarını çalıştırma seçenekleri [için](/cli/azure/install-azure-cli-windows)bkz.

   ```azurecli-interactive
   #!/bin/bash

   # Create a resource group.
   az group create \
     --name myResourceGroup \
     --location eastus

   # Create the virtual network (Resource Manager).
   az network vnet create \
     --name myVnet1 \
     --resource-group myResourceGroup \
     --location eastus \
     --address-prefix 10.0.0.0/16
   ```

5. CLI'yi kullanarak farklı dağıtım modelleri aracılığıyla oluşturulan iki sanal ağ arasında sanal ağ oluşturma. Aşağıdaki komut dosyasını bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Abonelik `<subscription id>` kimliğinizle değiştirin. Abonelik kimliğinizi bilmiyorsanız, komutu `az account show` girin. Çıktıdaki **kimlik** değeri abonelik kimliğinizdir. Değiştirilen komut dosyasını CLI oturumunuza yapıştırın ve sonra 'ya basın. `Enter`

   ```azurecli-interactive
   # Get the ID for VNet1.
   vnet1Id=$(az network vnet show \
     --resource-group myResourceGroup \
     --name myVnet1 \
     --query id --out tsv)

   # Peer VNet1 to VNet2.
   az network vnet peering create \
     --name myVnet1ToMyVnet2 \
     --resource-group myResourceGroup \
     --vnet-name myVnet1 \
     --remote-vnet-id /subscriptions/<subscription id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnet2 \
     --allow-vnet-access
   ```

6. Komut dosyası yürütülmeden sonra, sanal ağ (Kaynak Yöneticisi) için eşleme gözden geçirin. Aşağıdaki komutu kopyalayın, CLI oturumunuza yapıştırın ve ardından basın: `Enter`

   ```azurecli-interactive
   az network vnet peering list \
     --resource-group myResourceGroup \
     --vnet-name myVnet1 \
     --output table
   ```

   Çıktı, **PeeringState** sütununda **Bağlı'yı** gösterir.

   Sanal ağda oluşturduğunuz tüm Azure kaynakları artık IP adresleri aracılığıyla birbirleriyle iletişim kurabilir. Sanal ağlar için varsayılan Azure ad çözünürlüğünü kullanıyorsanız, sanal ağlardaki kaynaklar sanal ağlardaki adları çözümleemez. Bir eşlemede sanal ağlardaki adları çözümlemek istiyorsanız, kendi DNS sunucunuzu oluşturmanız gerekir. [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğünü](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)nasıl ayarlayınızı öğrenin.
7. **İsteğe Bağlı**: Sanal makineler oluşturmak bu öğreticide yer almasa da, bağlantının doğrulanması için her sanal ağda sanal bir makine oluşturabilir ve bir sanal makineden diğerine bağlanabilirsiniz.
8. **İsteğe Bağlı**: Bu eğitimde oluşturduğunuz kaynakları silmek için, bu makaledeki [kaynakları sil](#delete-cli) adımlarını tamamlayın.

## <a name="create-peering---powershell"></a><a name="powershell"></a>Eşleme oluşturma - PowerShell

1. PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) ve [Az](https://www.powershellgallery.com/packages/Az/) modüllerinin en son sürümünü yükleyin. Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin.
2. PowerShell oturumu başlatın.
3. `Add-AzureAccount` PowerShell'de, komutu girerek Azure'da oturum açın. Oturum açtırdığınız hesap, sanal ağ eşlemesi oluşturmak için gerekli izinlere sahip olmalıdır. İzinlerin listesi için [Sanal ağ izleme izinlerine](virtual-network-manage-peering.md#requirements-and-constraints)bakın.
4. PowerShell ile sanal ağ (klasik) oluşturmak için, yeni bir ağ yapılandırma dosyası oluşturmanız veya değiştirmeniz gerekir. Ağ yapılandırma dosyalarını nasıl [dışa aktarıp aktartığın, güncelleştirecek ve içe aktarılamayı](virtual-networks-using-network-configuration-file.md)öğrenin. Dosya, bu öğreticide kullanılan sanal ağ için aşağıdaki **VirtualNetworkSite** öğesini içermelidir:

    ```xml
    <VirtualNetworkSite name="myVnet2" Location="East US">
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
5. `Connect-AzAccount` Komutu girerek sanal ağı (Kaynak Yöneticisi) oluşturmak için Azure'da oturum açın. Oturum açtırdığınız hesap, sanal ağ eşlemesi oluşturmak için gerekli izinlere sahip olmalıdır. İzinlerin listesi için [Sanal ağ izleme izinlerine](virtual-network-manage-peering.md#requirements-and-constraints)bakın.
6. Kaynak grubu ve sanal ağ (Kaynak Yöneticisi) oluşturun. Komut dosyasını kopyalayın, PowerShell'e `Enter`yapıştırın ve sonra basın.

    ```powershell
    # Create a resource group.
      New-AzResourceGroup -Name myResourceGroup -Location eastus

    # Create the virtual network (Resource Manager).
      $vnet1 = New-AzVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location eastus
    ```

7. Farklı dağıtım modelleri aracılığıyla oluşturulan iki sanal ağ arasında sanal ağ eşleme oluşturun. Aşağıdaki komut dosyasını bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Abonelik `<subscription id>` kimliğinizle değiştirin. Abonelik kimliğinizi bilmiyorsanız, görüntülemek için `Get-AzSubscription` komutu girin. İade edilen çıktıdaki **Id** değeri abonelik kimliğinizdir. Komut dosyasını yürütmek için metin düzenleyicinizden değiştirilmiş komut dosyasını kopyalayın, ardından `Enter`PowerShell oturumunuza sağ tıklayın ve sonra .

    ```powershell
    # Peer VNet1 to VNet2.
    Add-AzVirtualNetworkPeering `
      -Name myVnet1ToMyVnet2 `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId /subscriptions/<subscription Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnet2
    ```

8. Komut dosyası yürütülmeden sonra, sanal ağ (Kaynak Yöneticisi) için eşleme gözden geçirin. Aşağıdaki komutu kopyalayın, PowerShell oturumunuza yapıştırın ve ardından şuna basın: `Enter`

    ```powershell
    Get-AzVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Çıktı, **PeeringState** sütununda **Bağlı'yı** gösterir.

    Sanal ağda oluşturduğunuz tüm Azure kaynakları artık IP adresleri aracılığıyla birbirleriyle iletişim kurabilir. Sanal ağlar için varsayılan Azure ad çözünürlüğünü kullanıyorsanız, sanal ağlardaki kaynaklar sanal ağlardaki adları çözümleemez. Bir eşlemede sanal ağlardaki adları çözümlemek istiyorsanız, kendi DNS sunucunuzu oluşturmanız gerekir. [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğünü](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)nasıl ayarlayınızı öğrenin.

9. **İsteğe Bağlı**: Sanal makineler oluşturmak bu öğreticide yer almasa da, bağlantının doğrulanması için her sanal ağda sanal bir makine oluşturabilir ve bir sanal makineden diğerine bağlanabilirsiniz.
10. **İsteğe Bağlı**: Bu eğitimde oluşturduğunuz kaynakları silmek için, bu makaledeki [kaynakları sil](#delete-powershell) adımlarını tamamlayın.

## <a name="delete-resources"></a><a name="delete"></a>Kaynakları silme

Bu öğreticiyi tamamladığınızda, kullanım ücreti ödememek için öğreticide oluşturduğunuz kaynakları silmek isteyebilirsiniz. Kaynak grubunu silmek, kaynak grubundaki tüm kaynakları da siler.

### <a name="azure-portal"></a><a name="delete-portal"></a>Azure portalında

1. Portal arama kutusuna **myResourceGroup'u**girin. Arama sonuçlarında **myResourceGroup'u**tıklatın.
2. **myResourceGroup** bıyığınızda **Sil** simgesini tıklatın.
3. Silme işlemini onaylamak için, **KAYNAK GRUBU Ad** türünün içinde, **myResourceGroup'u**girin ve sonra **Sil'i**tıklatın.

### <a name="azure-cli"></a><a name="delete-cli"></a>Azure CLI

1. Aşağıdaki komutla sanal ağı (Kaynak Yöneticisi) silmek için Azure CLI'yi kullanın:

    ```azurecli-interactive
    az group delete --name myResourceGroup --yes
    ```

2. Aşağıdaki komutlarla sanal ağı (klasik) silmek için klasik CLI'yi kullanın:

    ```azurecli-interactive
    azure config mode asm

    azure network vnet delete --vnet myVnet2 --quiet
    ```

### <a name="powershell"></a><a name="delete-powershell"></a>Powershell

1. Sanal ağı (Kaynak Yöneticisi) silmek için aşağıdaki komutu girin:

    ```powershell
    Remove-AzResourceGroup -Name myResourceGroup -Force
    ```

2. PowerShell ile sanal ağı (klasik) silmek için varolan bir ağ yapılandırma dosyasını değiştirmeniz gerekir. Ağ yapılandırma dosyalarını nasıl [dışa aktarıp aktartığın, güncelleştirecek ve içe aktarılamayı](virtual-networks-using-network-configuration-file.md)öğrenin. Bu öğreticide kullanılan sanal ağ için aşağıdaki VirtualNetworkSite öğesini kaldırın:

    ```xml
    <VirtualNetworkSite name="myVnet2" Location="East US">
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
