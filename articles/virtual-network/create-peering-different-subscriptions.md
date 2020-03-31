---
title: VNet eşleme oluşturma - farklı abonelikler
titlesuffix: Azure Virtual Network
description: Farklı Azure aboneliklerinde bulunan Kaynak Yöneticisi aracılığıyla oluşturulan sanal ağlar arasında sanal ağ oluşturmayı öğrenin.
services: virtual-network
documentationcenter: ''
author: anavinahar
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/09/2019
ms.author: anavin
ms.openlocfilehash: d085279167b498b13cfb79b97703cfdff7d6dd8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79245128"
---
# <a name="create-a-virtual-network-peering---resource-manager-different-subscriptions"></a>Sanal ağ eşleme oluşturma - Kaynak Yöneticisi, farklı abonelikler

Bu öğreticide, Kaynak Yöneticisi aracılığıyla oluşturulan sanal ağlar arasında sanal ağ eşleme oluşturmayı öğrenirsiniz. Sanal ağlar farklı aboneliklerde bulunur. İki sanal ağa bakmak, farklı sanal ağlardaki kaynakların, kaynaklar aynı sanal ağdaymış gibi aynı bant genişliği ve gecikme siyle birbirleriyle iletişim kurmasını sağlar. [Sanal ağ eşleme](virtual-network-peering-overview.md)hakkında daha fazla bilgi edinin.

Sanal ağ eşlemi oluşturma adımları, sanal ağların aynı veya farklı aboneliklerde olup olmadığına ve sanal ağların hangi [Azure dağıtım modeli](../azure-resource-manager/management/deployment-models.md?toc=%2fazure%2fvirtual-network%2ftoc.json) nde oluşturulduğuna bağlı olarak farklıdır. Aşağıdaki tablodan senaryoyu seçerek diğer senaryolarda sanal ağ eşleme oluşturmayı öğrenin:

|Azure dağıtım modeli  | Azure aboneliği  |
|--------- |---------|
|[Her ikisi de Resource Manager](tutorial-connect-virtual-networks-portal.md) |Aynı|
|[Biri Resource Manager, diğeri klasik](create-peering-different-deployment-models.md) |Aynı|
|[Biri Resource Manager, diğeri klasik](create-peering-different-deployment-models-subscriptions.md) |Farklı|

Klasik dağıtım modeli aracılığıyla dağıtılan iki sanal ağ arasında sanal ağ eşlemi oluşturulamaz. Her ikisi de klasik dağıtım modeli aracılığıyla oluşturulan sanal ağları bağlamanız gerekiyorsa, sanal ağları bağlamak için bir Azure [VPN Ağ Geçidi](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) kullanabilirsiniz.

Bu öğretici, aynı bölgedeki sanal ağları eşler. Ayrıca, desteklenen farklı [bölgelerdeki](virtual-network-manage-peering.md#cross-region)sanal ağları da eşleyebilirsiniz. Sanal ağlara bakmadan [önce, izleme gereksinimleri ve kısıtlamaları](virtual-network-manage-peering.md#requirements-and-constraints) hakkında bilgi edinmeniz önerilir.

Sanal ağ eşlemeoluşturmak için [Azure portalını, Azure](#portal)komut satırı [arabirimini](#cli) (CLI), Azure [PowerShell'i](#powershell)veya [Azure Kaynak Yöneticisi şablonunu](#template) kullanabilirsiniz. Seçtiğiniz aracı kullanarak sanal ağ eşleme oluşturmak için doğrudan adımlara gitmek için önceki araç bağlantılarından birini seçin.

Sanal ağlar farklı aboneliklerdeyse ve abonelikler farklı Azure Etkin Dizin kiracılarıyla ilişkilendiriliyorsa, devam etmeden önce aşağıdaki adımları tamamlayın:
1. Her Active Directory kiracısının kullanıcısını karşıdaki Azure Active Directory kiracısında [konuk kullanıcı](../active-directory/b2b/add-users-administrator.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-guest-users-to-the-directory) olarak ekleyin.
1. Her kullanıcı, karşıdaki Azure Active Directory kiracısından gelen konuk kullanıcı davetini kabul etmelidir.

## <a name="create-peering---azure-portal"></a><a name="portal"></a>Akran oluşturma - Azure portalı

Aşağıdaki adımlar, her abonelik için farklı hesaplar kullanır. Her iki abonelik için izinleri olan bir hesap kullanıyorsanız, tüm adımlar için aynı hesabı kullanabilir, portaldan çıkış için adımları atlayabilir ve sanal ağlara başka bir kullanıcı izni atama adımlarını atlayabilirsiniz.

1. [Azure portalına](https://portal.azure.com) *UserA*olarak giriş yapın. Oturum açtığın hesabın sanal ağ eşlemesi oluşturmak için gerekli izinlere sahip olması gerekir. İzinlerin listesi için [Sanal ağ izleme izinlerine](virtual-network-manage-peering.md#permissions)bakın.
2. + **Kaynak Oluştur'u**seçin, **Ağ Oluşturma'yı**seçin ve ardından Sanal **Ağ'ı**seçin.
3. Aşağıdaki ayarlar için aşağıdaki örnek değerleri seçin veya girin, ardından **Oluştur'u**seçin:
    - **Ürün :** *myVnetA*
    - **Adres alanı**: *10.0.0.0/16*
    - **Subnet adı**: *varsayılan*
    - **Alt net adres aralığı**: *10.0.0.0/24*
    - **Abonelik**: Abonelik A'yı seçin.
    - **Kaynak grubu**: **Yeni Oluştur'u** seçin ve *myResourceGroupA'ya* girin
    - **Yer**: *Doğu ABD*
4. Portalın üst kısmındaki **Arama kaynakları** kutusuna *myVnetA*yazın. Arama sonuçlarında göründüğünde **myVnetA'yı** seçin. 
5. Sol taraftaki dikey seçenekler listesinden **Access denetimi (IAM)** seçeneğini belirleyin.
6. **myVnetA altında - Erişim denetimi (IAM)**, seçin **+ Rol ataması ekleyin.**
7. **Rol** kutusunda **Ağ katkıda bulunanı** seçin.
8. **Select** kutusunda *UserB'yi*seçin veya aramak için UserB'nin e-posta adresini yazın.
9. **Kaydet'i**seçin.
10. **myVnetA - Access denetimi (IAM)** altında, sol taraftaki dikey seçenekler listesinden **Özellikler'i** seçin. Daha sonraki bir adımda kullanılan **KAYNAK Kimliğini**kopyalayın. Kaynak kimliği aşağıdaki örneğe benzer: `/subscriptions/<Subscription Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/virtualNetworks/myVnetA`.
11. Portaldan UserA olarak giriş yapın, ardından UserB olarak oturum açın.
12. Adım 2-3'te aşağıdaki değerleri girerek veya seçerek 2-3'e kadar tamamlayın:

    - **Adı**: *myVnetB*
    - **Adres alanı**: *10.1.0.0/16*
    - **Subnet adı**: *varsayılan*
    - **Alt net adres aralığı**: *10.1.0.0/24*
    - **Abonelik**: Abonelik B'yi seçin.
    - **Kaynak grubu**: **Yeni Oluştur'u** seçin ve *myResourceGroupB'a* girin
    - **Yer**: *Doğu ABD*

13. Portalın üst kısmındaki **Arama kaynakları** kutusuna *myVnetB*yazın. Arama sonuçlarında göründüğünde **myVnetB'yi** seçin.
14. **myVnetB**altında, sol taraftaki seçeneklerin dikey listesinden **Özellikler'i** seçin. Daha sonraki bir adımda kullanılan **KAYNAK Kimliğini**kopyalayın. Kaynak kimliği aşağıdaki örneğe benzer: `/subscriptions/<Subscription ID>/resourceGroups/myResourceGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB`.
15. **myVnetB**altında **Access denetimini (IAM)** seçin ve ardından adım 8'de **UserA'yı** girerek myVnetB için 5-10 adımlarını tamamlayın.
16. Portaldan UserB olarak giriş yapın ve UserA olarak giriş yapın.
17. Portalın üst kısmındaki **Arama kaynakları** kutusuna *myVnetA*yazın. Arama sonuçlarında göründüğünde **myVnetA'yı** seçin.
18. **myVnetA'yı**seçin.
19. **AYARLAR** **altında, Eşler'i**seçin.
20. **myVnetA altında - Peerings**, seçin **+ Ekle**
21. **Eşleme Ekle**altında, aşağıdaki seçenekleri girin veya seçin, ardından **Tamam'ı**seçin:
     - **Adı**: *myVnetAToMyVnetB*
     - **Sanal ağ dağıtım modeli**: **Kaynak Yöneticisi'ni**seçin.
     - **Kaynak kimliğimi biliyorum:** Bu kutuyu işaretleyin.
     - **Kaynak Kimliği**: 14. adımdan kaynak kimliğini girin.
     - **Sanal ağ erişimine izin verin:** **Etkin'in** seçildiğinden emin olun.
    Bu öğreticide başka hiçbir ayar kullanılmaz. Tüm eşleme ayarları hakkında bilgi edinmek için [sanal ağ eşlemelerini yönet'i](virtual-network-manage-peering.md#create-a-peering)okuyun.
22. Oluşturduğunuz eşleme, önceki adımda **Tamam'ı** seçtikten sonra kısa bir bekleme süresi görünür. **Başlatılan** **myVnetAToMyVnetB** oluşturduğunuz peering için **PEERING DURUM** sütununda listelenir. MyVnetA'ya myVnetB'ye baktınız, ama şimdi myVnetB'yi myVnetA'ya bakmalısınız. Sanal ağlardaki kaynakların birbirleriyle iletişim kurmasını sağlamak için her iki yönde de eşleme oluşturulmalıdır.
23. Portaldan UserA olarak giriş yapın ve UserB olarak giriş yapın.
24. MyVnetB için adımları tekrar 17-21 tamamlayın. Adım 21, bakan *myVnetBToMyVnetA*adı , **Sanal ağ**için *myVnetA* seçin , ve **Kaynak Kimliği** kutusuna adım 10 dan kimlik girin.
25. MyVnetB için eşleme oluşturmak için **Tamam'ı** seçtikten birkaç saniye sonra, oluşturduğunuz **myVnetBToMyVnetA,** **EŞLEME DURUM** sütununda **Bağlı** olarak listelenir.
26. Portaldan UserB olarak giriş yapın ve UserA olarak giriş yapın.
27. Adımları tekrar 17-19 tamamlayın. **myVnetAToVNetB** peering için **PEERING DURUMU** şimdi de **Bağlı**. Eşlemedeki her iki sanal ağ için **PEERING STATUS** sütununda **Bağlı'yı** gördünkten sonra eşleme başarıyla kurulur. Sanal ağda oluşturduğunuz tüm Azure kaynakları artık IP adresleri aracılığıyla birbirleriyle iletişim kurabilir. Sanal ağlar için varsayılan Azure ad çözünürlüğünü kullanıyorsanız, sanal ağlardaki kaynaklar sanal ağlardaki adları çözümlemez. Bir eşlemede sanal ağlardaki adları çözümlemek istiyorsanız, kendi DNS sunucunuzu oluşturmanız gerekir. [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğünü](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)nasıl ayarlayınızı öğrenin.
28. **İsteğe Bağlı**: Sanal makineler oluşturma bu öğreticide yer almasa da, bağlantının doğrulanması için her sanal ağda sanal bir makine oluşturabilir ve bir sanal makineden diğerine bağlanabilirsiniz.
29. **İsteğe Bağlı**: Bu eğitimde oluşturduğunuz kaynakları silmek için, bu makalenin [Kaynakları Sil](#delete-portal) bölümündeki adımları tamamlayın.

## <a name="create-peering---azure-cli"></a><a name="cli"></a>Akran oluşturma - Azure CLI

Bu öğretici, her abonelik için farklı hesaplar kullanır. Her iki abonelik için izinleri olan bir hesap kullanıyorsanız, tüm adımlar için aynı hesabı kullanabilir, Azure'dan çıkış için adımları atlayabilir ve kullanıcı rol atamaları oluşturan komut dosyası satırlarını kaldırabilirsiniz. KullanıcıA UserB@azure.com ve UserB için kullandığınız kullanıcı adlarını değiştirin UserA@azure.com ve aşağıdaki komut dosyalarının tümlerinde değiştirin. 

Aşağıdaki komut dosyaları:

- Azure CLI sürümü 2.0.4 veya sonrası gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.
- Bash kabuğunda çalışıyor. Windows istemcisi üzerinde Azure CLI betiklerini çalıştırma seçenekleri için bkz. [Windows’da Azure CLI'yi yükleme](/cli/azure/install-azure-cli-windows).

CLI'yi ve bağımlılıklarını yüklemek yerine Azure Bulut Kabuğu'nu kullanabilirsiniz. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Azure hesabınızda oturum açabileceğiniz bir Bulut Bulutu çağıran komut dosyasında **Try it** düğmesini seçin.

1. Bir CLI oturumu açın ve komutu kullanarak `azure login` UserA olarak Azure'da oturum açın. Oturum açtığın hesabın sanal ağ eşlemesi oluşturmak için gerekli izinlere sahip olması gerekir. İzinlerin listesi için [Sanal ağ izleme izinlerine](virtual-network-manage-peering.md#permissions)bakın.
2. Aşağıdaki komut dosyasını bilgisayarınızdaki bir metin `<SubscriptionA-Id>` düzenleyicisine kopyalayın, SubscriptionA kimliğiyle değiştirin, ardından değiştirilmiş komut `Enter`dosyasını kopyalayın, CLI oturumunuza yapıştırın ve basın. Abonelik Kimliğinizi bilmiyorsanız, komutu `az account show` girin. Çıktıdaki **kimlik** değeri abonelik kimliğinizdir.

    ```azurecli-interactive
    # Create a resource group.
    az group create \
      --name myResourceGroupA \
      --location eastus

    # Create virtual network A.
    az network vnet create \
      --name myVnetA \
      --resource-group myResourceGroupA \
      --location eastus \
      --address-prefix 10.0.0.0/16

    # Assign UserB permissions to virtual network A.
    az role assignment create \
      --assignee UserB@azure.com \
      --role "Network Contributor" \
      --scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

3. `az logout` Komutu kullanarak Azure'dan UserA olarak oturum açın ve ardından UserB olarak Azure'da oturum açın. Oturum açtığın hesabın sanal ağ eşlemesi oluşturmak için gerekli izinlere sahip olması gerekir. İzinlerin listesi için [Sanal ağ izleme izinlerine](virtual-network-manage-peering.md#permissions)bakın.
4. myVnetB oluşturun. Komut dosyası içeriğini 2. SubscriptionB kimliğiile değiştirin. `<SubscriptionA-Id>` 10.0.0.0/16'yı 10.1.0.0/16 olarak değiştirin, tüm As As'ı B'ye ve tüm B'leri A'ya `Enter`değiştirin, değiştirilmiş komut dosyasını kopyalayın, CLI oturumunuza yapıştırın ve basın.
5. Azure'dan UserB olarak giriş yapın ve Azure'da UserA olarak oturum açın.
6. myVnetA'dan myVnetB'ye bakan bir sanal ağ oluşturun. Aşağıdaki komut dosyası içeriğini bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. SubscriptionB kimliğiile değiştirin. `<SubscriptionB-Id>` Komut dosyasını çalıştırmak için değiştirilen komut dosyasını kopyalayın, CLI oturumunuza yapıştırın ve Enter tuşuna basın.

    ```azurecli-interactive
        # Get the id for myVnetA.
        vnetAId=$(az network vnet show \
          --resource-group myResourceGroupA \
          --name myVnetA \
          --query id --out tsv)

        # Peer myVNetA to myVNetB.
        az network vnet peering create \
          --name myVnetAToMyVnetB \
          --resource-group myResourceGroupA \
          --vnet-name myVnetA \
          --remote-vnet-id /subscriptions/<SubscriptionB-Id>/resourceGroups/myResourceGroupB/providers/Microsoft.Network/VirtualNetworks/myVnetB \
          --allow-vnet-access
    ```

7. myVnetA'nın görüntü durumunu görüntüleyin.

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroupA \
      --vnet-name myVnetA \
      --output table
    ```

    Durum **Başlatılır.** MyVnetB'den myVnetA'ya bakış oluşturmanız sonucu **Connected** olarak değişir.

8. Azure'dan UserA'yı oturumu açın ve Azure'da UserB olarak oturum açın.
9. myVnetB'den myVnetA'ya bakış oluşturun. Komut dosyası içeriğini 6. SubscriptionA için kimliği değiştirin `<SubscriptionB-Id>` ve tüm As As'ı B ve tüm B'leri A olarak değiştirin. Değişiklikleri yaptıktan sonra, değiştirilen komut dosyasını kopyalayın, CLI oturumunuza `Enter`yapıştırın ve basın.
10. myVnetB'nin görüntü durumunu görüntüleyin. 7. adımdaki komut dosyası içeriğini bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. Kaynak grubu ve sanal ağ adları için A'yı B'ye değiştirin, komut dosyasını kopyalayın, değiştirilen komut dosyasını CLI oturumunuza yapıştırın ve sonra 'ya basın. `Enter` Eşleme durumu **Bağlı.** myVnetA'nın bakış durumunu myVnetB'den myVnetA'ya oluşturduktan sonra **Connected'e** dönüşür. MyVnetA'nın eşleme durumunu doğrulamak için UserA'yı Azure'a yeniden kaydedebilir ve 7. 

    > [!NOTE]
    > Her iki sanal ağ için de eşleme durumu **Bağlanına** kadar eşleme kurulmadı.

11. **İsteğe Bağlı**: Sanal makineler oluşturma bu öğreticide yer almasa da, bağlantının doğrulanması için her sanal ağda sanal bir makine oluşturabilir ve bir sanal makineden diğerine bağlanabilirsiniz.
12. **İsteğe Bağlı**: Bu eğitimde oluşturduğunuz kaynakları silmek için, bu makaledeki [kaynakları sil](#delete-cli) adımlarını tamamlayın.

Sanal ağda oluşturduğunuz tüm Azure kaynakları artık IP adresleri aracılığıyla birbirleriyle iletişim kurabilir. Sanal ağlar için varsayılan Azure ad çözünürlüğünü kullanıyorsanız, sanal ağlardaki kaynaklar sanal ağlardaki adları çözümlemez. Bir eşlemede sanal ağlardaki adları çözümlemek istiyorsanız, kendi DNS sunucunuzu oluşturmanız gerekir. [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğünü](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)nasıl ayarlayınızı öğrenin.

## <a name="create-peering---powershell"></a><a name="powershell"></a>Eşleme oluşturma - PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğretici, her abonelik için farklı hesaplar kullanır. Her iki abonelik için izinleri olan bir hesap kullanıyorsanız, tüm adımlar için aynı hesabı kullanabilir, Azure'dan çıkış için adımları atlayabilir ve kullanıcı rol atamaları oluşturan komut dosyası satırlarını kaldırabilirsiniz. KullanıcıA UserB@azure.com ve UserB için kullandığınız kullanıcı adlarını değiştirin UserA@azure.com ve aşağıdaki komut dosyalarının tümlerinde değiştirin.

1. Azure PowerShell sürüm 1.0.0 veya daha yüksek olduğunu doğrulayın. `Get-Module -Name Az` Bunu PowerShell [Az modülünün](/powershell/azure/install-az-ps)en son sürümünü yüklemenizi öneririz çalıştırarak yapabilirsiniz. Azure PowerShell'i kullanmaya yeni başladıysanız [Azure PowerShell'e genel bakış](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) sayfasını inceleyin. 
2. PowerShell oturumu başlatın.
3. PowerShell'de, `Connect-AzAccount` komutu girerek UserA olarak Azure'da oturum açın. Oturum açtığın hesabın sanal ağ eşlemesi oluşturmak için gerekli izinlere sahip olması gerekir. İzinlerin listesi için [Sanal ağ izleme izinlerine](virtual-network-manage-peering.md#permissions)bakın.
4. Kaynak grubu ve sanal ağ oluşturun A. Aşağıdaki komut dosyasını bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. SubscriptionA kimliğiyle değiştirin. `<SubscriptionA-Id>` Abonelik Kimliğinizi bilmiyorsanız, görüntülemek için `Get-AzSubscription` komutu girin. İade edilen çıktıdaki **Id** değeri abonelik kimliğinizdir. Komut dosyasını çalıştırmak için değiştirilen komut dosyasını kopyalayın, PowerShell'e yapıştırın ve sonra 'ya basın. `Enter`

    ```powershell
    # Create a resource group.
    New-AzResourceGroup `
      -Name MyResourceGroupA `
      -Location eastus

    # Create virtual network A.
    $vNetA = New-AzVirtualNetwork `
      -ResourceGroupName MyResourceGroupA `
      -Name 'myVnetA' `
      -AddressPrefix '10.0.0.0/16' `
      -Location eastus

    # Assign UserB permissions to myVnetA.
    New-AzRoleAssignment `
      -SignInName UserB@azure.com `
      -RoleDefinitionName "Network Contributor" `
      -Scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

5. Azure'dan UserA'yı oturumu açın ve UserB'de oturum açın. Oturum açtığın hesabın sanal ağ eşlemesi oluşturmak için gerekli izinlere sahip olması gerekir. İzinlerin listesi için [Sanal ağ izleme izinlerine](virtual-network-manage-peering.md#permissions)bakın.
6. Komut dosyası içeriğini 4. Abonelik `<SubscriptionA-Id>` için kimliği değiştirin B. 10.0.0.0/16 ile 10.1.0.0/16 değiştirin. Tüm Olarak B'yi ve tüm B'leri A olarak değiştirin. Komut dosyasını çalıştırmak için değiştirilmiş komut dosyasını kopyalayın, `Enter`PowerShell'e yapıştırın ve sonra 'ya basın.
7. Azure'dan UserB'yi oturumu açın ve UserA'ya giriş yapın.
8. myVnetA'dan myVnetB'ye bakış oluşturun. Aşağıdaki komut dosyasını bilgisayarınızdaki bir metin düzenleyicisine kopyalayın. B `<SubscriptionB-Id>` abonelik kimliğiile değiştirin. Komut dosyasını çalıştırmak için değiştirilmiş komut dosyasını kopyalayın, `Enter`PowerShell'e yapıştırın ve sonra 'ya basın.

   ```powershell
   # Peer myVnetA to myVnetB.
   $vNetA=Get-AzVirtualNetwork -Name myVnetA -ResourceGroupName myResourceGroupA
   Add-AzVirtualNetworkPeering `
     -Name 'myVnetAToMyVnetB' `
     -VirtualNetwork $vNetA `
     -RemoteVirtualNetworkId "/subscriptions/<SubscriptionB-Id>/resourceGroups/myResourceGroupB/providers/Microsoft.Network/virtualNetworks/myVnetB"
   ```

9. myVnetA'nın görüntü durumunu görüntüleyin.

    ```powershell
    Get-AzVirtualNetworkPeering `
      -ResourceGroupName myResourceGroupA `
      -VirtualNetworkName myVnetA `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    Durum **Başlatılır.** MyVnetB'den myVnetA'ya bakışı ayarladığınızda **Connected** olarak değişir.

10. Azure'dan UserA'yı oturumu açın ve UserB'de oturum açın.
11. myVnetB'den myVnetA'ya bakış oluşturun. Komut dosyası içeriğini 8. Abonelik `<SubscriptionB-Id>` A kimliğiyle değiştirin ve tüm As As'ı B ve tüm B'leri A olarak değiştirin. Komut dosyasını çalıştırmak için değiştirilen komut dosyasını kopyalayın, PowerShell'e yapıştırın ve sonra 'ya basın. `Enter`
12. myVnetB'nin görüntü durumunu görüntüleyin. Komut dosyası içeriğini 9. Kaynak grubu ve sanal ağ adları için A'yı B olarak değiştirin. Komut dosyasını çalıştırmak için değiştirilen komut dosyasını PowerShell'e yapıştırın ve ardından 'ye basın. `Enter` Durum **Bağlı.** myVnetA'nın bakış durumunu **myVnetB'den** **myVnetA'ya**oluşturduktan sonra **Connected'e** dönüşür. **myVnetB** MyVnetA'nın eşleme durumunu doğrulamak için UserA'yı Azure'a yeniden kaydedebilir ve 9.

    > [!NOTE]
    > Her iki sanal ağ için de eşleme durumu **Bağlanına** kadar eşleme kurulmadı.

    Sanal ağda oluşturduğunuz tüm Azure kaynakları artık IP adresleri aracılığıyla birbirleriyle iletişim kurabilir. Sanal ağlar için varsayılan Azure ad çözünürlüğünü kullanıyorsanız, sanal ağlardaki kaynaklar sanal ağlardaki adları çözümlemez. Bir eşlemede sanal ağlardaki adları çözümlemek istiyorsanız, kendi DNS sunucunuzu oluşturmanız gerekir. [Kendi DNS sunucunuzu kullanarak Ad çözünürlüğünü](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)nasıl ayarlayınızı öğrenin.

13. **İsteğe Bağlı**: Sanal makineler oluşturma bu öğreticide yer almasa da, bağlantının doğrulanması için her sanal ağda sanal bir makine oluşturabilir ve bir sanal makineden diğerine bağlanabilirsiniz.
14. **İsteğe Bağlı**: Bu eğitimde oluşturduğunuz kaynakları silmek için, bu makaledeki [kaynakları sil](#delete-powershell) adımlarını tamamlayın.

## <a name="create-peering---resource-manager-template"></a><a name="template"></a>Eşleme oluşturma - Kaynak Yöneticisi şablonu

1. Sanal ağ oluşturmak ve uygun [izinleri](virtual-network-manage-peering.md#permissions)atamak için, bu makalenin [Portal](#portal), [Azure CLI](#cli)veya [PowerShell](#powershell) bölümlerindeki adımları tamamlayın.
2. Takip eden metni yerel bilgisayarınızdaki bir dosyaya kaydedin. UserA'nın abonelik kimliğiyle değiştirin. `<subscription ID>` Örneğin, dosyayı vnetpeeringA.json olarak kaydedebilirsiniz.

   ```json
   {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
    "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "myVnetA/myVnetAToMyVnetB",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<subscription ID>/resourceGroups/PeeringTest/providers/Microsoft.Network/virtualNetworks/myVnetB"
                }
            }
            }
        ]
   }
   ```

3. UserA olarak Azure'da oturum açın ve şablonu [PowerShell](../azure-resource-manager/templates/deploy-powershell.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-local-template) [portalını](../azure-resource-manager/templates/deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-resources-from-custom-template)veya [Azure CLI'yi](../azure-resource-manager/templates/deploy-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-local-template)kullanarak dağıtın. Adım 2'de örnek json metnini kaydettiğiniz dosya adını belirtin.
4. Örnek json'u 2.
   - **adı**: *myVnetA/myVnetAToMyVnetB'i* *myVnetB/myVnetBToMyVnetA*olarak değiştirin.
   - **id**: `<subscription ID>` UserB'nin abonelik kimliğini değiştirin ve *myVnetB'yi* *myVnetA*olarak değiştirin.
5. Yine 3. adımı tamamlayın, Azure'da UserB olarak oturum açtı.
6. **İsteğe Bağlı**: Sanal makineler oluşturma bu öğreticide yer almasa da, bağlantının doğrulanması için her sanal ağda sanal bir makine oluşturabilir ve bir sanal makineden diğerine bağlanabilirsiniz.
7. **İsteğe Bağlı**: Bu eğitimde oluşturduğunuz kaynakları silmek için, Azure portalı, PowerShell veya Azure CLI'yi kullanarak bu makalenin [kaynakları sil](#delete) bölümündeki adımları tamamlayın.

## <a name="delete-resources"></a><a name="delete"></a>Kaynakları silme
Bu öğreticiyi tamamladığınızda, kullanım ücreti ödememek için öğreticide oluşturduğunuz kaynakları silmek isteyebilirsiniz. Kaynak grubunu silmek, kaynak grubundaki tüm kaynakları da siler.

### <a name="azure-portal"></a><a name="delete-portal"></a>Azure portalında

1. Azure portalında UserA olarak oturum açın.
2. Portal arama kutusuna **myResourceGroupA'yı**girin. Arama sonuçlarında **myResourceGroupA'yı**seçin.
3. **Sil**’i seçin.
4. Silme işlemini onaylamak için, **KAYNAK GRUBU Ad** türünün içinde **myResourceGroupA'yı**girin ve sonra **Sil'i**seçin.
5. Portaldan UserA olarak giriş yapın ve UserB olarak giriş yapın.
6. MyResourceGroupB için 2-4 adımlarını tamamlayın.

### <a name="azure-cli"></a><a name="delete-cli"></a>Azure CLI

1. Azure'da UserA olarak oturum açın ve aşağıdaki komutu uygulayın:

   ```azurecli-interactive
   az group delete --name myResourceGroupA --yes
   ```

2. Azure'dan UserA olarak çıkış yapın ve UserB olarak oturum açın.
3. Aşağıdaki komutu yürütün:

   ```azurecli-interactive
   az group delete --name myResourceGroupB --yes
   ```

### <a name="powershell"></a><a name="delete-powershell"></a>Powershell

1. Azure'da UserA olarak oturum açın ve aşağıdaki komutu uygulayın:

   ```powershell
   Remove-AzResourceGroup -Name myResourceGroupA -force
   ```

2. Azure'dan UserA olarak çıkış yapın ve UserB olarak oturum açın.
3. Aşağıdaki komutu yürütün:

   ```powershell
   Remove-AzResourceGroup -Name myResourceGroupB -force
   ```

## <a name="next-steps"></a>Sonraki adımlar

- Üretim kullanımı için sanal ağ eşleme oluşturmadan önce önemli [sanal ağ izleme kısıtlamaları ve davranışları](virtual-network-manage-peering.md#requirements-and-constraints) hakkında kendinizi iyice tanıtın.
- Tüm [sanal ağ eşleme ayarları](virtual-network-manage-peering.md#create-a-peering)hakkında bilgi edinin.
- Sanal ağ eşlemeli [bir hub ve kollu ağ topolojisi oluşturmayı](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke#virtual-network-peering) öğrenin.
