---
title: Azure Etkinlik Hub'larını Azure Özel Bağlantı Hizmetiyle Tümleştir
description: Azure Etkinlik Hub'larını Azure Özel Bağlantı Hizmeti ile nasıl entegre edebilirsiniz öğrenin
services: event-hubs
author: spelluru
ms.author: spelluru
ms.date: 03/12/2020
ms.service: event-hubs
ms.topic: article
ms.openlocfilehash: cff1b3b79b34d3f0bed27a2ea50799185958a8ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477856"
---
# <a name="integrate-azure-event-hubs-with-azure-private-link-preview"></a>Azure Etkinlik Hub'larını Azure Özel Bağlantısıyla Tümleştir (Önizleme)
Azure Özel Bağlantı Hizmeti, Azure Hizmetleri'ne (örneğin, Azure Etkinlik Hub'ları, Azure Depolama ve Azure Cosmos DB) ve Azure barındırılan müşteri/iş ortağı hizmetlerine sanal **ağınızdaki özel** bir bitiş noktası üzerinden erişmenizi sağlar.

Özel bitiş noktası, sizi Azure Özel Bağlantısı ile çalışan bir hizmete özel ve güvenli bir şekilde bağlayan bir ağ arabirimidir. Özel bitiş noktası, VNet'inizden gelen özel bir IP adresini kullanır ve hizmeti VNet'inize etkin bir şekilde getirir. Hizmete giden tüm trafik özel bitiş noktasından yönlendirilebilir, bu nedenle ağ geçidi, NAT aygıtları, ExpressRoute veya VPN bağlantıları veya genel IP adresleri gerekmez. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Bir Azure kaynağıörneğine bağlanarak erişim denetiminde en yüksek parçalılık düzeyini sağlayabilirsiniz.

Daha fazla bilgi için Azure [Özel Bağlantı nedir?](../private-link/private-link-overview.md)

> [!NOTE]
> Bu özellik yalnızca **özel** katmanla desteklenir. İlgili katman hakkında daha fazla bilgi için, [Özel Etkinlik Hub'larına Genel Bakış'a](event-hubs-dedicated-overview.md)bakın. 
>
> Bu özellik şu anda **önizlemede.** 


## <a name="add-a-private-endpoint-using-azure-portal"></a>Azure portalını kullanarak özel bir bitiş noktası ekleme

### <a name="prerequisites"></a>Ön koşullar

Bir Etkinlik Hub'ı ad alanını Azure Özel Bağlantısıyla tümleştirmek için aşağıdaki varlıklara veya izinlere ihtiyacınız vardır:

- Olay Hub'ları ad alanı.
- Azure sanal ağı.
- Sanal ağdaki bir alt ağ.
- Hem ad alanı hem de sanal ağ için sahip veya katılımcı izinleri.

Özel bitiş noktanız ve sanal ağınız aynı bölgede olmalıdır. Portalı kullanarak özel bitiş noktası için bir bölge seçtiğinizde, otomatik olarak yalnızca o bölgedeki sanal ağları filtreler. Ad alanınız farklı bir bölgede olabilir.

Özel bitiş noktanız sanal abunuzda özel bir IP adresi kullanır.

### <a name="steps"></a>Adımlar
Zaten bir Olay Hub'ları ad alanınız varsa, aşağıdaki adımları izleyerek özel bir bağlantı bağlantısı oluşturabilirsiniz:

1. [Azure portalında](https://portal.azure.com)oturum açın. 
2. Arama **çubuğunda, olay hub'larını**yazın.
3. Özel bir bitiş noktası eklemek istediğiniz listeden **ad alanını** seçin.
4. **Ayarlar'ın**altındaki **Ağ** sekmesini seçin.
5. Sayfanın üst kısmındaki **Özel bitiş noktası bağlantıları (önizleme)** sekmesini seçin. Özel bir Olay Hub katmanı kullanmıyorsanız, bir ileti görürsünüz: **Olay Hub'larında özel uç nokta bağlantıları yalnızca özel bir küme altında oluşturulan ad alanları tarafından desteklenir.**
6. Sayfanın üst kısmındaki **+ Özel Bitiş Noktası** düğmesini seçin.

    ![Görüntü](./media/private-link-service/private-link-service-3.png)
7. Temel **Bilgiler** sayfasında aşağıdaki adımları izleyin: 
    1. Özel bitiş noktasını oluşturmak istediğiniz **Azure aboneliğini** seçin. 
    2. Özel bitiş noktası kaynağı için **kaynak grubunu** seçin.
    3. Özel bitiş noktası için bir **ad** girin. 
    5. Özel bitiş noktası için bir **bölge** seçin. Özel bitiş noktanız sanal ağınızla aynı bölgede olmalıdır, ancak bağlandığınız özel bağlantı kaynağının farklı bir bölgesinde olabilir. 
    6. **Sonraki'ni seçin:** Sayfanın altındaki kaynak >düğmesi.

        ![Özel Bitiş Noktası Oluşturma - Temel Bilgiler sayfası](./media/private-link-service/create-private-endpoint-basics-page.png)
8. **Kaynak** sayfasında aşağıdaki adımları izleyin:
    1. Bağlantı yöntemi için, **dizinimdeki bir Azure kaynağına Bağlan'ı**seçerseniz aşağıdaki adımları izleyin: 
        1. **Etkinlik Hub'larınızın ad alanının** bulunduğu **Azure aboneliğini** seçin. 
        2. **Kaynak türü**için, Kaynak türü için **Microsoft.EventHub/namespaces'i** seçin. **Resource type**
        3. **Kaynak**için açılan listeden olay hub'ları ad alanını seçin. 
        4. **Hedef alt kaynağının** ad **alanı**olarak ayarlı olduğunu doğrulayın.
        5. **İleri'yi seçin:** Sayfanın altındaki Configuration >düğmesi. 
        
            ![Özel Bitiş Noktası Oluşturma - Kaynak sayfası](./media/private-link-service/create-private-endpoint-resource-page.png)    
    2. **Kaynak kimliği veya diğer adıyla bir Azure kaynağına bağlan'ı**seçerseniz aşağıdaki adımları izleyin:
        1. Kaynak **kimliği** veya **takma ad**girin. Bazılarının sizinle paylaştığı kaynak kimliği veya takma ad olabilir.
        2. **Hedef alt kaynak**için ad **alanını**girin. Özel bitiş noktanızın erişebileceği alt kaynak türüdür.
        3. (isteğe bağlı) İstek **iletisi**girin. Kaynak sahibi, özel uç nokta bağlantısını yönetirken bu iletiyi görür.
        4. Ardından, **Sayfanın** altındaki >Yapılandırma düğmesini seçin.

            ![Özel Bitiş Noktası Oluşturma - Kaynak kimliğini kullanarak bağlan](./media/private-link-service/connect-resource-id.png)
9. **Yapılandırma** sayfasında, sanal ağdaki alt ağı özel bitiş noktasını dağıtmak istediğiniz yere seçersiniz. 
    1. Sanal **ağ**seçin. Yalnızca seçili abonelik ve konumdaki sanal ağlar açılır listede listelenir. 
    2. Seçtiğiniz sanal ağda bir **alt ağ** seçin. 
    3. **Sonrakini Seçin: Etiketler sayfanın** altındaki >düğmesini etiketler. 

        ![Özel Bitiş Noktası Oluşturma - Yapılandırma sayfası](./media/private-link-service/create-private-endpoint-configuration-page.png)
10. **Etiketler** sayfasında, özel bitiş noktası kaynağıyla ilişkilendirmek istediğiniz etiketleri (adlar ve değerler) oluşturun. Ardından, sayfanın altındaki **Gözden Geçir + oluştur** düğmesini seçin. 
11. Gözden **Geçir + oluştur,** tüm ayarları gözden geçir ve özel bitiş noktasını oluşturmak için **Oluştur'u** seçin.
    
    ![Özel Bitiş Noktası Oluşturma - Sayfayı Gözden Geçir in ve Oluştur](./media/private-link-service/create-private-endpoint-review-create-page.png)
12. Oluşturduğunuz özel uç nokta bağlantısını son noktalar listesinde gördüğünüzü doğrulayın. Bu örnekte, dizininizdeki bir Azure kaynağına bağlı olduğunuz ve yeterli izinleriniz olduğundan özel bitiş noktası otomatik olarak onaylanır. 

    ![Özel bitiş noktası oluşturuldu](./media/private-link-service/private-endpoint-created.png)

## <a name="add-a-private-endpoint-using-powershell"></a>PowerShell'i kullanarak özel bir bitiş noktası ekleme
Aşağıdaki örnekte, özel bir uç nokta bağlantısı oluşturmak için Azure PowerShell'in nasıl kullanılacağı gösterilmektedir. Sizin için özel bir küme oluşturmaz. Özel bir Olay Hub'ları kümesi oluşturmak için [bu makaledeki](event-hubs-dedicated-cluster-create-portal.md) adımları izleyin. 

```azurepowershell-interactive
# create resource group

$rgName = "<RESOURCE GROUP NAME>"
$vnetlocation = "<VIRTUAL NETWORK LOCATION>"
$vnetName = "<VIRTUAL NETWORK NAME>"
$subnetName = "<SUBNET NAME>"
$namespaceLocation = "<NAMESPACE LOCATION>"
$namespaceName = "<NAMESPACE NAME>"
$peConnectionName = "<PRIVATE ENDPOINT CONNECTION NAME>"

# create virtual network
$virtualNetwork = New-AzVirtualNetwork `
                    -ResourceGroupName $rgName `
                    -Location $vnetlocation `
                    -Name $vnetName `
                    -AddressPrefix 10.0.0.0/16

# create subnet with endpoint network policy disabled
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
                    -Name $subnetName `
                    -AddressPrefix 10.0.0.0/24 `
                    -PrivateEndpointNetworkPoliciesFlag "Disabled" `
                    -VirtualNetwork $virtualNetwork

# update virtual network
$virtualNetwork | Set-AzVirtualNetwork

# create an event hubs namespace in a dedicated cluster
$namespaceResource = New-AzResource -Location $namespaceLocation `
                                    -ResourceName $namespaceName `
                                    -ResourceGroupName $rgName `
                                    -Sku @{name = "Standard"; capacity = 1} `
                                    -Properties @{clusterArmId = "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/clusters/<EVENT HUBS CLUSTER NAME>"} `
                                    -ResourceType "Microsoft.EventHub/namespaces" -ApiVersion "2018-01-01-preview"

# create private endpoint connection
$privateEndpointConnection = New-AzPrivateLinkServiceConnection `
                                -Name $peConnectionName `
                                -PrivateLinkServiceId $namespaceResource.ResourceId `
                                -GroupId "namespace"

# get subnet object that you will use later
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroupName  $rgName -Name $vnetName
$subnet = $virtualNetwork | Select -ExpandProperty subnets `
                                | Where-Object  {$_.Name -eq $subnetName}  
   
# create a private endpoint   
$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName $rgName  `
                                -Name $vnetName   `
                                -Location $vnetlocation `
                                -Subnet  $subnet   `
                                -PrivateLinkServiceConnection $privateEndpointConnection

(Get-AzResource -ResourceId $namespaceResource.ResourceId -ExpandProperties).Properties


```

## <a name="manage-private-endpoints-using-azure-portal"></a>Azure portalLarını kullanarak özel uç noktaları yönetme

Özel bir bitiş noktası oluşturduğunuzda, bağlantının onaylanması gerekir. Özel bir bitiş noktası oluşturduğunuz kaynak dizininizdeyse, yeterli izine sahip olmak koşuluyla bağlantı isteğini onaylayabilirsiniz. Başka bir dizindeki bir Azure kaynağına bağlanıyorsanız, bağlantı isteğinizi bu kaynağın sahibinin onaylamasını beklemeniz gerekir.

Dört hüküm veren durum vardır:

| Hizmet eylemi | Hizmet tüketici özel bitiş noktası durumu | Açıklama |
|--|--|--|
| None | Beklemede | Bağlantı el ile oluşturulur ve Private Link kaynak sahibinden onay bekliyor. |
| Onaylama | Onaylandı | Bağlantı otomatik olarak veya el ile onaylandı ve kullanıma hazır. |
| Reddet | Reddedilen | Bağlantı, özel bağlantı kaynağı sahibi tarafından reddedildi. |
| Kaldır | Bağlantı kesildi | Bağlantı özel bağlantı kaynak sahibi tarafından kaldırıldı, özel bitiş noktası bilgilendirici hale gelir ve temizleme için silinmelidir. |
 
###  <a name="approve-reject-or-remove-a-private-endpoint-connection"></a>Özel bir bitiş noktası bağlantısını onaylama, reddetme veya kaldırma

1. Azure Portal’da oturum açın.
2. Arama **çubuğunda, olay hub'larını**yazın.
3. Yönetmek istediğiniz **ad alanını** seçin.
4. **Ağ** sekmesini seçin.
5. İstediğiniz işlemi temel alan aşağıdaki uygun bölüme gidin: onaylamak, reddetmek veya kaldırmak.

### <a name="approve-a-private-endpoint-connection"></a>Özel bir bitiş noktası bağlantısını onaylama
1. Bekleyen herhangi bir bağlantı varsa, sağlama durumunda **Bekleyen** ile listelenen bir bağlantı görürsünüz. 
2. Onaylamak istediğiniz **özel bitiş noktasını** seçin
3. **Onayla** düğmesini seçin.

    ![Görüntü](./media/private-link-service/approve-private-endpoint.png)
4. Bağlantı **yut'luk** sayfasında bir yorum ekleyin (isteğe bağlı) ve **Evet'i**seçin. **Hayır'ı**seçerseniz hiçbir şey olmaz. 
5. **Onaylandı**olarak değiştirilen listede ki özel bitiş noktası bağlantısının durumunu görmeniz gerekir. 

### <a name="reject-a-private-endpoint-connection"></a>Özel uç nokta bağlantısını reddetme

1. Reddetmek istediğiniz özel bitiş noktası bağlantıları varsa, bekleyen bir istek veya varolan bağlantı olsun, bağlantıyı seçin ve **Reddet** düğmesini tıklatın.

    ![Görüntü](./media/private-link-service/private-endpoint-reject-button.png)
2. Bağlantıyı **Reddet** sayfasında bir yorum girin (isteğe bağlı) girin ve **Evet'i**seçin. **Hayır'ı**seçerseniz hiçbir şey olmaz. 
3. **Reddedilen**olarak değiştirilen listedeki özel bitiş noktası bağlantısının durumunu görmeniz gerekir. 

### <a name="remove-a-private-endpoint-connection"></a>Özel uç nokta bağlantısını kaldırma

1. Özel bir bitiş noktası bağlantısını kaldırmak için, listede onu seçin ve araç çubuğunda **Kaldır'ı** seçin.
2. Sil **bağlantı** sayfasında, özel bitiş noktasının silinmesini onaylamak için **Evet'i** seçin. **Hayır'ı**seçerseniz hiçbir şey olmaz.
3. Durum Bağlantısız olarak **değiştirilmeniz**gerekir. Ardından, bitiş noktasının listeden kaybolduğunu görürsünüz.

## <a name="validate-that-the-private-link-connection-works"></a>Özel bağlantı bağlantısının çalıştığını doğrulayın

Özel bitiş noktası kaynağının aynı alt netindeki kaynakların özel bir IP adresi üzerinden Olay Hub'ları ad alanınıza bağladığını ve doğru özel DNS bölge tümleştirmesine sahip olduğunu doğrulamanız gerekir.

İlk olarak, [Azure portalında Windows sanal makine oluştur'daki](../virtual-machines/windows/quick-create-portal.md) adımları izleyerek sanal bir makine oluşturun

**Ağ** sekmesinde:

1. **Sanal ağ** ve **alt ağ**belirtin. Yeni bir sanal ağ oluşturabilir veya varolan bir ağ seçebilirsiniz. Varolan bir tane seçiyorsanız, bölgenin eşleştiğinden emin olun.
1. Ortak **bir IP** kaynağı belirtin.
1. NIC **ağ güvenlik grubunda** **Yok'u**seçin.
1. Yük **dengelemesinde**, **No'yu**seçin.

Komut satırını açın ve aşağıdaki komutu çalıştırın:

```console
nslookup <your-event-hubs-namespace-name>.servicebus.windows.net
```

Bir Olay Hub'ının IP adresini ortak bir bitiş noktası üzerinden çözmek için ns arama komutunu çalıştırıyorsanız, aşağıdaki gibi görünen bir sonuç görürsünüz:

```console
c:\ >nslookup <your-event-hubs-namespae-name>.servicebus.windows.net

Non-authoritative answer:
Name:    
Address:  (public IP address)
Aliases:  <your-event-hubs-namespace-name>.servicebus.windows.net
```

Özel bir bitiş noktası üzerinden olay hub'larının ad alanının IP adresini çözmek için ns arama komutunu çalıştırArsanız, aşağıdaki gibi görünen bir sonuç görürsünüz:

```console
c:\ >nslookup your_event-hubs-namespace-name.servicebus.windows.net

Non-authoritative answer:
Name:    
Address:  10.1.0.5 (private IP address)
Aliases:  <your-event-hub-name>.servicebus.windows.net
```

## <a name="limitations-and-design-considerations"></a>Sınırlamalar ve tasarım konuları

**Fiyatlandırma**: Fiyatlandırma bilgileri için [Azure Özel Bağlantı fiyatlandırması'na](https://azure.microsoft.com/pricing/details/private-link/)bakın.

**Sınırlamalar**: Azure Etkinlik Hub'ları için Özel Bitiş Noktası genel önizlemededir. Bu özellik tüm Azure ortak bölgelerinde kullanılabilir.

**Olay Hub'ları ad alanı başına maksimum özel uç nokta sayısı**: 120.

Daha fazla kullanım için Azure [Özel Bağlantı hizmeti: Sınırlamalar](../private-link/private-link-service-overview.md#limitations)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Özel Bağlantı](../private-link/private-link-service-overview.md) hakkında daha fazla bilgi edinin
- [Azure Etkinlik Hub'ları](event-hubs-about.md) hakkında daha fazla bilgi edinin
