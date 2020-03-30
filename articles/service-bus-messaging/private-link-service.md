---
title: Azure Hizmet Veri Toplarını Azure Özel Bağlantı Hizmetiyle Tümleştir
description: Azure Hizmet Veri Yolu'nun Azure Özel Bağlantı Hizmeti ile nasıl entegre edilebildiğini öğrenin
services: service-bus-messaging
author: spelluru
ms.author: spelluru
ms.date: 03/13/2020
ms.service: service-bus-messaging
ms.topic: article
ms.openlocfilehash: b8c4248b7275ac96acce96f890f6ff0148116f48
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79478012"
---
# <a name="integrate-azure-service-bus-with-azure-private-link-preview"></a>Azure Hizmet Veri Toplarını Azure Özel Bağlantısıyla Tümleştir (Önizleme)

Azure Özel Bağlantı Hizmeti, Azure hizmetlerine (örneğin, Azure Servis Veri Mes'i, Azure Cosmos Depolama ve Azure Cosmos DB) ve Azure barındırılan müşteri/iş ortağı hizmetlerine sanal **ağınızdaki özel** bir bitiş noktası üzerinden erişmenizi sağlar.

Özel bitiş noktası, sizi Azure Özel Bağlantısı ile çalışan bir hizmete özel ve güvenli bir şekilde bağlayan bir ağ arabirimidir. Özel bitiş noktası, VNet'inizden gelen özel bir IP adresini kullanır ve hizmeti VNet'inize etkin bir şekilde getirir. Hizmete giden tüm trafik özel bitiş noktasından yönlendirilebilir, bu nedenle ağ geçidi, NAT aygıtları, ExpressRoute veya VPN bağlantıları veya genel IP adresleri gerekmez. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Bir Azure kaynağıörneğine bağlanarak erişim denetiminde en yüksek parçalılık düzeyini sağlayabilirsiniz.

Daha fazla bilgi için Azure [Özel Bağlantı nedir?](../private-link/private-link-overview.md)

> [!NOTE]
> Bu özellik, Azure Hizmet Veri Servisi'nin **birinci sınıf** katmanıyla desteklenir. Premium katman hakkında daha fazla bilgi için [Service Bus Premium ve Standart mesajlaşma katmanları](service-bus-premium-messaging.md) makalesine bakın.
>
> Bu özellik şu anda **önizlemede.** 


## <a name="add-a-private-endpoint-using-azure-portal"></a>Azure portalını kullanarak özel bir bitiş noktası ekleme

### <a name="prerequisites"></a>Ön koşullar

Bir Hizmet Veri Aracı ad alanını Azure Özel Bağlantısıyla tümleştirmek için aşağıdaki varlıklara veya izinlere ihtiyacınız vardır:

- Servis Veri Günü ad alanı.
- Azure sanal ağı.
- Sanal ağdaki bir alt ağ.
- Hem Hizmet Veri Günü ad alanı hem de sanal ağ için sahip veya katılımcı izinleri.

Özel bitiş noktanız ve sanal ağınız aynı bölgede olmalıdır. Portalı kullanarak özel bitiş noktası için bir bölge seçtiğinizde, otomatik olarak yalnızca o bölgedeki sanal ağları filtreler. Servis Veri Günü ad alanınız farklı bir bölgede olabilir. Ayrıca, özel bitiş noktanız sanal ağınızda özel bir IP adresi kullanır.

### <a name="steps"></a>adımlar

Zaten varolan bir ad alanınız varsa, aşağıdaki adımları izleyerek özel bir bitiş noktası oluşturabilirsiniz:

1. [Azure portalında](https://portal.azure.com)oturum açın. 
2. Arama çubuğunda Servis **Veri Servisi**yazın.
3. Özel bir bitiş noktası eklemek istediğiniz listeden **ad alanını** seçin.
4. **Ayarlar'ın**altındaki **Ağ** sekmesini seçin.
5. Sayfanın üst kısmındaki **Özel bitiş noktası bağlantıları (önizleme)** sekmesini seçin
6. Sayfanın üst kısmındaki **+ Özel Bitiş Noktası** düğmesini seçin.

    ![Özel bitiş noktası düğmesi ekleme](./media/private-link-service/private-link-service-3.png)
7. Temel **Bilgiler** sayfasında aşağıdaki adımları izleyin: 
    1. Özel bitiş noktasını oluşturmak istediğiniz **Azure aboneliğini** seçin. 
    2. Özel bitiş noktası kaynağı için **kaynak grubunu** seçin.
    3. Özel bitiş noktası için bir **ad** girin. 
    5. Özel bitiş noktası için bir **bölge** seçin. Özel bitiş noktanız sanal ağınızla aynı bölgede olmalıdır, ancak bağlandığınız özel bağlantı kaynağının farklı bir bölgesinde olabilir. 
    6. **Sonraki'ni seçin:** Sayfanın altındaki kaynak >düğmesi.

        ![Özel Bitiş Noktası Oluşturma - Temel Bilgiler sayfası](./media/private-link-service/create-private-endpoint-basics-page.png)
8. **Kaynak** sayfasında aşağıdaki adımları izleyin:
    1. Bağlantı yöntemi için, **dizinimdeki bir Azure kaynağına Bağlan'ı**seçerseniz aşağıdaki adımları izleyin:   
        1. **Hizmet Veri Aracı ad alanınızın** bulunduğu Azure **aboneliğini** seçin. 
        2. **Kaynak türü**için, Kaynak türü için **Microsoft.ServiceBus/namespaces'i** seçin. **Resource type**
        3. **Kaynak**için açılan listeden bir Hizmet Veri Günü ad alanı seçin. 
        4. **Hedef alt kaynağının** ad **alanı**olarak ayarlı olduğunu doğrulayın.
        5. **İleri'yi seçin:** Sayfanın altındaki Configuration >düğmesi. 
        
            ![Özel Bitiş Noktası Oluşturma - Kaynak sayfası](./media/private-link-service/create-private-endpoint-resource-page.png)
    2. **Kaynak kimliği veya diğer adıyla bir Azure kaynağına bağlan'ı**seçerseniz aşağıdaki adımları izleyin:
        1. Kaynak **kimliği** veya **takma ad**girin. Bazılarının sizinle paylaştığı kaynak kimliği veya takma ad olabilir.
        2. **Hedef alt kaynak**için ad **alanını**girin. Özel bitiş noktanızın erişebileceği alt kaynak türüdür. 
        3. (isteğe bağlı) İstek **iletisi**girin. Kaynak sahibi, özel uç nokta bağlantısını yönetirken bu iletiyi görür. 
        4. Ardından, **Sayfanın** altındaki >Yapılandırma düğmesini seçin. 

            ![Özel Bitiş Noktası Oluşturma - kaynak kimliğini kullanarak bağlan](./media/private-link-service/connect-resource-id.png)
9. **Yapılandırma** sayfasında, sanal ağdaki alt ağı özel bitiş noktasını dağıtmak istediğiniz yere seçersiniz. 
    1. Sanal **ağ**seçin. Yalnızca seçili abonelik ve konumdaki sanal ağlar açılır listede listelenir. 
    2. Seçtiğiniz sanal ağda bir **alt ağ** seçin. 
    3. **Sonrakini Seçin: Etiketler sayfanın** altındaki >düğmesini etiketler. 

        ![Özel Bitiş Noktası Oluşturma - Yapılandırma sayfası](./media/private-link-service/create-private-endpoint-configuration-page.png)
10. **Etiketler** sayfasında, özel bitiş noktası kaynağıyla ilişkilendirmek istediğiniz etiketleri (adlar ve değerler) oluşturun. Ardından, sayfanın altındaki **Gözden Geçir + oluştur** düğmesini seçin. 
11. Gözden **Geçir + oluştur,** tüm ayarları gözden geçir ve özel bitiş noktasını oluşturmak için **Oluştur'u** seçin.
    
    ![Özel Bitiş Noktası Oluşturma - Sayfayı Gözden Geçir in ve Oluştur](./media/private-link-service/create-private-endpoint-review-create-page.png)
12. Özel bitiş noktasının oluşturulduğunu doğrulayın. Kaynağın sahibiyseniz ve **Bağlantı yöntemi** **için dizin seçeneğimde bir Azure kaynağına bağlan'ı** seçmişseniz, bitiş noktası bağlantısı otomatik olarak **onaylanmalıdır.** **Bekleme** durumundaysa, Azure portalı bölümünü [kullanarak özel uç noktalarını yönet](#manage-private-endpoints-using-azure-portal) bölümüne bakın.

    ![Özel bitiş noktası oluşturuldu](./media/private-link-service/private-endpoint-created.png)

## <a name="add-a-private-endpoint-using-powershell"></a>PowerShell'i kullanarak özel bir bitiş noktası ekleme
Aşağıdaki örnek, Hizmet Veri Servisi ad alanına özel bir uç nokta bağlantısı oluşturmak için Azure PowerShell'i nasıl kullanacağınızı gösterir.

Özel bitiş noktanız ve sanal ağınız aynı bölgede olmalıdır. Servis Veri Günü ad alanınız farklı bir bölgede olabilir. Ayrıca, özel bitiş noktanız sanal ağınızda özel bir IP adresi kullanır.

```azurepowershell-interactive

$rgName = "<RESOURCE GROUP NAME>"
$vnetlocation = "<VNET LOCATION>"
$vnetName = "<VIRTUAL NETWORK NAME>"
$subnetName = "<SUBNET NAME>"
$namespaceLocation = "<NAMESPACE LOCATION>"
$namespaceName = "<NAMESPACE NAME>"
$peConnectionName = "<PRIVATE ENDPOINT CONNECTION NAME>"

# create resource group
az group create -l $vnetLocation -n $rgName

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

# create premium service bus namespace
$namespaceResource = New-AzResource -Location $namespaceLocation -ResourceName $namespaceName -ResourceGroupName $rgName -Sku @{name = "Premium"; capacity = 1} -Properties @{} -ResourceType "Microsoft.ServiceBus/namespaces" -

# create a private link service connection
$privateEndpointConnection = New-AzPrivateLinkServiceConnection `
                                -Name $peConnectionName `
                                -PrivateLinkServiceId $namespaceResource.ResourceId `
                                -GroupId "namespace"

# get subnet object that you will use in the next step                                
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroupName  $rgName -Name $vnetName
$subnet = $virtualNetwork | Select -ExpandProperty subnets `
                                | Where-Object  {$_.Name -eq $subnetName}  
   
# now, create private endpoint   
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
1. Arama çubuğunda Servis **Veri Servisi**yazın.
1. Yönetmek istediğiniz **ad alanını** seçin.
1. **Ağ** sekmesini seçin.
5. İstediğiniz işlemi temel alan aşağıdaki uygun bölüme gidin: onaylamak, reddetmek veya kaldırmak. 

### <a name="approve-a-private-endpoint-connection"></a>Özel bir bitiş noktası bağlantısını onaylama

1. Bekleyen herhangi bir bağlantı varsa, sağlama durumunda **Bekleyen** ile listelenen bir bağlantı görürsünüz. 
2. Onaylamak istediğiniz **özel bitiş noktasını** seçin
3. **Onayla** düğmesini seçin.

    ![Özel bitiş noktasını onaylama](./media/private-link-service/private-endpoint-approve.png)
4. Bağlantı **bağlantısını onayla** sayfasında isteğe bağlı bir **yorum**girin ve **Evet'i**seçin. **Hayır'ı**seçerseniz hiçbir şey olmaz. 

    ![Bağlantı sayfasını onaylama](./media/private-link-service/approve-connection-page.png)
5. **Onaylanan**olarak değiştirilen listedeki bağlantıdurumunu görmeniz gerekir. 

    ![Bağlantı durumu - onaylı](./media/private-link-service/connection-status-approved.png)

### <a name="reject-a-private-endpoint-connection"></a>Özel uç nokta bağlantısını reddetme

1. Reddetmek istediğiniz özel bitiş noktası bağlantıları varsa, bekleyen bir istek veya daha önce onaylanmış varolan bağlantı olsun, bitiş noktası bağlantısını seçin ve **Reddet** düğmesini tıklatın.

    ![Reddet düğmesi](./media/private-link-service/private-endpoint-reject.png)
2. Bağlantıyı **Reddet** sayfasında isteğe bağlı bir açıklama girin ve **Evet'i**seçin. **Hayır'ı**seçerseniz hiçbir şey olmaz. 

    ![Bağlantı sayfasını reddet](./media/private-link-service/reject-connection-page.png)
3. **Reddedilen**ler listesindeki bağlantıdurumunu görmeniz gerekir. 

    ![Bitiş noktası reddedildi](./media/private-link-service/endpoint-rejected.png)


### <a name="remove-a-private-endpoint-connection"></a>Özel uç nokta bağlantısını kaldırma

1. Özel bir bitiş noktası bağlantısını kaldırmak için, listede onu seçin ve araç çubuğunda **Kaldır'ı** seçin. 

    ![Düğmeyi kaldır](./media/private-link-service/remove-endpoint.png)
2. Sil **bağlantı** sayfasında, özel bitiş noktasının silinmesini onaylamak için **Evet'i** seçin. **Hayır'ı**seçerseniz hiçbir şey olmaz. 

    ![Bağlantı sayfasını silme](./media/private-link-service/delete-connection-page.png)
3. Durum Bağlantısız olarak **değiştirilmeniz**gerekir. Ardından, bitiş noktasının listeden kaybolduğunu görürsünüz. 

## <a name="validate-that-the-private-link-connection-works"></a>Özel bağlantı bağlantısının çalıştığını doğrulayın

Özel bitiş noktası kaynağının aynı alt netindeki kaynakların özel bir IP adresi üzerinden Hizmet Veri Gönderi ad alanınıza bağladığını ve doğru özel DNS bölge tümleştirmesine sahip olduğunu doğrulamanız gerekir.

İlk olarak, [Azure portalında Windows sanal makine oluştur'daki](../virtual-machines/windows/quick-create-portal.md) adımları izleyerek sanal bir makine oluşturun

**Ağ** sekmesinde:

1. **Sanal ağ** ve **Subnet**belirtin. Yeni bir sanal ağ oluşturabilir veya varolan bir ağ seçebilirsiniz. Varolan bir tane seçiyorsanız, bölgenin eşleştiğinden emin olun.
1. Ortak **bir IP** kaynağı belirtin.
1. **NIC ağ güvenlik grubu için** **Yok'u**seçin.
1. **Yük dengelemesi**için **Hayır'ı**seçin.

Komut satırını açın ve aşağıdaki komutu çalıştırın:

```console
nslookup <your-service-bus-namespace-name>.servicebus.windows.net
```

Bir Hizmet Veri Gönderi ad alanının IP adresini ortak bir bitiş noktası üzerinden çözmek için ns arama komutunu çalıştırıyorsanız, aşağıdaki gibi görünen bir sonuç görürsünüz:

```console
c:\ >nslookup <your-service-bus-namespace-name>.servicebus.windows.net

Non-authoritative answer:
Name:    
Address:  (public IP address)
Aliases:  <your-service-bus-namespace-name>.servicebus.windows.net
```

Özel bir bitiş noktası üzerinden bir Hizmet Veri Gönderi ad alanının IP adresini çözmek için ns arama komutunu çalıştırArsanız, aşağıdaki gibi görünen bir sonuç görürsünüz:

```console
c:\ >nslookup your_service-bus-namespace-name.servicebus.windows.net

Non-authoritative answer:
Name:    
Address:  10.1.0.5 (private IP address)
Aliases:  <your-service-bus-namespace-name>.servicebus.windows.net
```

## <a name="limitations-and-design-considerations"></a>Sınırlamalar ve Tasarım Konuları

**Fiyatlandırma**: Fiyatlandırma bilgileri için [Azure Özel Bağlantı fiyatlandırması'na](https://azure.microsoft.com/pricing/details/private-link/)bakın.

**Sınırlamalar**: Azure Hizmet Veri Servisi için Özel Bitiş Noktası genel önizlemededir. Bu özellik tüm Azure ortak bölgelerinde kullanılabilir.

**Hizmet Veri Günü ad alanı başına maksimum özel uç noktası sayısı**: 120.

Daha fazla kullanım için Azure [Özel Bağlantı hizmeti: Sınırlamalar](../private-link/private-link-service-overview.md#limitations)

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure Özel Bağlantı](../private-link/private-link-service-overview.md) hakkında daha fazla bilgi edinin
- [Azure Servis Veri Yolu](service-bus-messaging-overview.md) hakkında daha fazla bilgi edinin
