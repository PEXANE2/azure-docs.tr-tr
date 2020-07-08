---
title: Azure Event Hubs Azure özel bağlantı hizmeti ile tümleştirme
description: Azure Event Hubs Azure özel bağlantı hizmeti ile tümleştirmeyi öğrenin
ms.date: 06/23/2020
ms.topic: article
ms.openlocfilehash: aa1eb4df425d83a37fbf4ac69e0e256c464dc5c9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85312832"
---
# <a name="integrate-azure-event-hubs-with-azure-private-link"></a>Azure Event Hubs Azure özel bağlantısı ile tümleştirme
Azure özel bağlantı hizmeti, Azure hizmetlerine (örneğin, Azure Event Hubs, Azure depolama ve Azure Cosmos DB) ve Azure 'da barındırılan müşteri/iş ortağı hizmetlerine sanal ağınızdaki **özel bir uç nokta** üzerinden erişmenizi sağlar.

Özel uç nokta, Azure özel bağlantısı tarafından desteklenen bir hizmete özel ve güvenli bir şekilde bağlanan bir ağ arabirimidir. Özel uç nokta, sanal ağınızdan bir özel IP adresi kullanarak hizmeti sanal ağınıza etkin bir şekilde getiriyor. Hizmete giden tüm trafik özel uç nokta aracılığıyla yönlendirilebilir, bu nedenle ağ geçitleri, NAT cihazları, ExpressRoute veya VPN bağlantıları ya da genel IP adresleri gerekmez. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Bir Azure kaynağı örneğine bağlanarak, erişim denetimi için en yüksek düzeyde ayrıntı düzeyi sağlayabilirsiniz.

Daha fazla bilgi için bkz. [Azure özel bağlantısı nedir?](../private-link/private-link-overview.md)

> [!IMPORTANT]
> Bu özellik hem **Standart** hem de **adanmış** katmanlar için desteklenir. 

>[!WARNING]
> Özel uç noktaların etkinleştirilmesi, diğer Azure hizmetlerinin Event Hubs etkileşimde olmasını engelleyebilir.
>
> Sanal ağlar kullanılırken güvenilen Microsoft Hizmetleri desteklenmez.
>
> Sanal ağlarla çalışmayan yaygın Azure senaryoları ( **listenin ayrıntılı olmadığına** unutmayın)-
> - Azure Izleyici (Tanılama ayarı)
> - Azure Stream Analytics
> - Azure Event Grid ile tümleştirme
> - Azure IoT Hub yolları
> - Azure IoT Device Explorer
>
> Aşağıdaki Microsoft hizmetlerinin bir sanal ağda olması gerekir
> - Azure Web Apps
> - Azure İşlevleri

## <a name="add-a-private-endpoint-using-azure-portal"></a>Azure portal kullanarak özel uç nokta ekleme

### <a name="prerequisites"></a>Ön koşullar

Bir Event Hubs ad alanını Azure özel bağlantısıyla bütünleştirmek için aşağıdaki varlıklara veya izinlere ihtiyacınız olacaktır:

- Bir Event Hubs ad alanı.
- Bir Azure sanal ağı.
- Sanal ağdaki bir alt ağ.
- Hem ad alanı hem de sanal ağ için sahip veya katkıda bulunan izinleri.

Özel uç noktanız ve sanal ağınız aynı bölgede olmalıdır. Portalı kullanarak özel uç nokta için bir bölge seçtiğinizde bu, yalnızca o bölgedeki sanal ağları otomatik olarak filtreleyecek. Ad alanınız farklı bir bölgede olabilir.

Özel uç noktanız sanal ağınızda özel bir IP adresi kullanıyor.

### <a name="steps"></a>Adımlar
Zaten bir Event Hubs ad alanınız varsa, aşağıdaki adımları izleyerek bir özel bağlantı bağlantısı oluşturabilirsiniz:

1. [Azure portalında](https://portal.azure.com) oturum açın. 
2. Arama çubuğuna **Olay Hub 'ları**yazın.
3. Listeden özel uç nokta eklemek istediğiniz **ad alanını** seçin.
4. **Ayarlar**altında **ağ** sekmesini seçin.
5. Sayfanın üst kısmındaki **Özel uç nokta bağlantıları** sekmesini seçin. 
6. Sayfanın üst kısmındaki **+ Özel uç nokta** düğmesini seçin.

    ![Görüntü](./media/private-link-service/private-link-service-3.png)
7. **Temel bilgiler** sayfasında, aşağıdaki adımları izleyin: 
    1. Özel uç noktasını oluşturmak istediğiniz **Azure aboneliğini** seçin. 
    2. Özel uç nokta kaynağı için **kaynak grubunu** seçin.
    3. Özel uç nokta için bir **ad** girin. 
    5. Özel uç nokta için bir **bölge** seçin. Özel uç noktanız, sanal ağınızla aynı bölgede olmalıdır, ancak bağlanmakta olduğunuz özel bağlantı kaynağından farklı bir bölgede olabilir. 
    6. Sayfanın alt kısmındaki **İleri: kaynak >** düğmesini seçin.

        ![Özel uç nokta oluşturma-temel bilgiler sayfası](./media/private-link-service/create-private-endpoint-basics-page.png)
8. **Kaynak** sayfasında, aşağıdaki adımları izleyin:
    1. Bağlantı yöntemi için **dizinimde bir Azure kaynağına bağlan**' ı seçerseniz şu adımları izleyin: 
        1. **Event Hubs ad alanının** bulunduğu **Azure aboneliğini** seçin. 
        2. **Kaynak türü**Için, **kaynak türü**için **Microsoft. EventHub/namespaces** ' i seçin.
        3. **Kaynak**için, açılan listeden bir Event Hubs ad alanı seçin. 
        4. **Hedef alt kaynağın** **ad alanı**olarak ayarlandığını onaylayın.
        5. Sayfanın alt kısmındaki **İleri: yapılandırma >** düğmesini seçin. 
        
            ![Özel uç nokta oluşturma-kaynak sayfası](./media/private-link-service/create-private-endpoint-resource-page.png)    
    2. **Kaynak kimliği veya diğer ad ile bir Azure kaynağına bağlan**' ı seçerseniz, aşağıdaki adımları izleyin:
        1. **Kaynak kimliğini** veya **diğer adı**girin. Bu, birinin sizinle paylaştığı kaynak KIMLIĞI veya diğer ad olabilir. Kaynak KIMLIĞINI almanın en kolay yolu, Azure portal Event Hubs ad alanına gitmeniz ve URI 'nin ' den başlayarak olan kısmını kopyalamadır `/subscriptions/` . Örnek için aşağıdaki resme bakın. 
        2. **Hedef alt kaynak**için **ad alanı**girin. Bu, Özel uç noktanızın erişebileceği alt kaynağın türüdür.
        3. seçim **İstek iletisi**girin. Kaynak sahibi, Özel uç nokta bağlantısını yönetirken bu iletiyi görür.
        4. Ardından sayfanın alt kısmındaki **İleri: yapılandırma >** düğmesini seçin.

            ![Özel uç nokta oluşturma-kaynak KIMLIĞI kullanarak bağlanma](./media/private-link-service/connect-resource-id.png)
9. **Yapılandırma** sayfasında, bir sanal ağda özel uç noktayı dağıtmak istediğiniz alt ağı seçersiniz. 
    1. Bir **sanal ağ**seçin. Yalnızca şu anda seçili olan abonelikte ve konumda bulunan sanal ağlar açılan listede listelenir. 
    2. Seçtiğiniz sanal ağda bir **alt ağ** seçin. 
    3. Sayfanın alt kısmındaki **İleri: etiketler >** düğmesini seçin. 

        ![Özel uç nokta oluşturma-yapılandırma sayfası](./media/private-link-service/create-private-endpoint-configuration-page.png)
10. **Etiketler** sayfasında, Özel uç nokta kaynağıyla ilişkilendirmek istediğiniz etiketleri (adlar ve değerler) oluşturun. Ardından sayfanın alt kısmındaki **gözden geçir + oluştur** düğmesini seçin. 
11. **Gözden geçir + oluştur**' da, tüm ayarları gözden geçirin ve **Oluştur** ' u seçerek özel uç noktasını oluşturun.
    
    ![Özel uç nokta oluşturma-sayfa Inceleme ve oluşturma](./media/private-link-service/create-private-endpoint-review-create-page.png)
12. Oluşturduğunuz özel uç nokta bağlantısının uç noktalar listesinde görüntülendiğini doğrulayın. Bu örnekte, dizininizde bir Azure kaynağına bağlandığınızdan ve yeterli izinlere sahip olduğunuzdan özel uç nokta otomatik olarak onaylanır. 

    ![Özel uç nokta oluşturuldu](./media/private-link-service/private-endpoint-created.png)

## <a name="add-a-private-endpoint-using-powershell"></a>PowerShell kullanarak özel uç nokta ekleme
Aşağıdaki örnek, Azure PowerShell özel bir uç nokta bağlantısı oluşturmak için nasıl kullanılacağını gösterir. Sizin için adanmış bir küme oluşturmaz. Adanmış bir Event Hubs kümesi oluşturmak için [Bu makaledeki](event-hubs-dedicated-cluster-create-portal.md) adımları izleyin. 

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

### <a name="configure-the-private-dns-zone"></a>Özel DNS bölgesini yapılandırma
Event Hubs etki alanı için özel bir DNS bölgesi oluşturun ve sanal ağla bir ilişki bağlantısı oluşturun:

```azurepowershell-interactive
$zone = New-AzPrivateDnsZone -ResourceGroupName $rgName `
                            -Name "privatelink.servicebus.windows.net" 
 
$link  = New-AzPrivateDnsVirtualNetworkLink -ResourceGroupName $rgName `
                                            -ZoneName "privatelink.servicebus.windows.net" `
                                            -Name "mylink" `
                                            -VirtualNetworkId $virtualNetwork.Id  
 
$networkInterface = Get-AzResource -ResourceId $privateEndpoint.NetworkInterfaces[0].Id -ApiVersion "2019-04-01" 
 
foreach ($ipconfig in $networkInterface.properties.ipConfigurations) { 
    foreach ($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) { 
        Write-Host "$($ipconfig.properties.privateIPAddress) $($fqdn)"  
        $recordName = $fqdn.split('.',2)[0] 
        $dnsZone = $fqdn.split('.',2)[1] 
        New-AzPrivateDnsRecordSet -Name $recordName -RecordType A -ZoneName "privatelink.servicebus.windows.net"  `
                                -ResourceGroupName $rgName -Ttl 600 `
                                -PrivateDnsRecords (New-AzPrivateDnsRecordConfig -IPv4Address $ipconfig.properties.privateIPAddress)  
    } 
}
```

## <a name="manage-private-endpoints-using-azure-portal"></a>Azure portal kullanarak özel uç noktaları yönetme

Özel bir uç nokta oluşturduğunuzda bağlantının onaylanması gerekir. Özel bir uç noktası oluşturmakta olduğunuz kaynak dizininizdeki ise, yeterli izinlere sahip olduğunuz belirtilen bağlantı isteğini onaylayabilirsiniz. Başka bir dizindeki bir Azure kaynağına bağlanıyorsanız, bu kaynağın sahibinin bağlantı isteğinizi onaylamasını beklemeniz gerekir.

Dört sağlama durumu vardır:

| Hizmet eylemi | Hizmet tüketicisi özel uç nokta durumu | Açıklama |
|--|--|--|
| Hiçbiri | Beklemede | Bağlantı el ile oluşturulur ve özel bağlantı kaynağı sahibinden onay bekliyor. |
| Onaylama | Onaylandı | Bağlantı otomatik olarak veya el ile onaylandı ve kullanılabilir hale gelmiştir. |
| Reddet | Reddedildi | Bağlantı, özel bağlantı kaynağı sahibi tarafından reddedildi. |
| Kaldır | Bağlantı kesildi | Bağlantı, özel bağlantı kaynağı sahibi tarafından kaldırıldı, Özel uç nokta bilgilendirici hale gelir ve temizlik için silinmelidir. |
 
###  <a name="approve-reject-or-remove-a-private-endpoint-connection"></a>Özel bir uç nokta bağlantısını onaylama, reddetme veya kaldırma

1. Azure Portal’da oturum açın.
2. Arama çubuğuna **Olay Hub 'ları**yazın.
3. Yönetmek istediğiniz **ad alanını** seçin.
4. **Ağ** sekmesini seçin.
5. Şunları yapmak istediğiniz işleme göre aşağıdaki ilgili bölüme gidin: onaylama, reddetme veya kaldırma.

### <a name="approve-a-private-endpoint-connection"></a>Özel bir uç nokta bağlantısını onaylama
1. Bekleyen herhangi bir bağlantı varsa, sağlama durumunda **bekliyor** ile listelenmiş bir bağlantı görürsünüz. 
2. Onaylamak istediğiniz **Özel uç noktayı** seçin
3. **Onayla** düğmesini seçin.

    ![Görüntü](./media/private-link-service/approve-private-endpoint.png)
4. **Bağlantıyı Onayla** sayfasında, bir açıklama ekleyin (isteğe bağlı) ve **Evet**' i seçin. **Hayır**' ı seçerseniz, hiçbir şey olmaz. 
5. Listede, **Onaylandı**olarak değiştirilen özel uç nokta bağlantısının durumunu görmeniz gerekir. 

### <a name="reject-a-private-endpoint-connection"></a>Özel bir uç nokta bağlantısını reddetme

1. Reddetmek istediğiniz özel uç nokta bağlantıları varsa, bekleyen bir istek ya da var olan bir bağlantı olup olmadığına bakılmaksızın bağlantıyı seçin ve **Reddet** düğmesine tıklayın.

    ![Görüntü](./media/private-link-service/private-endpoint-reject-button.png)
2. **Bağlantıyı Reddet** sayfasında, bir açıklama girin (isteğe bağlı) ve **Evet**' i seçin. **Hayır**' ı seçerseniz, hiçbir şey olmaz. 
3. Listede, **reddedildi**olarak değiştirilen özel uç nokta bağlantısının durumunu görmeniz gerekir. 

### <a name="remove-a-private-endpoint-connection"></a>Özel uç nokta bağlantısını kaldırma

1. Özel bir uç nokta bağlantısını kaldırmak için listeden seçin ve araç çubuğundan **Kaldır** ' ı seçin.
2. **Bağlantıyı Sil** sayfasında, Özel uç noktasının silinmesini onaylamak için **Evet** ' i seçin. **Hayır**' ı seçerseniz, hiçbir şey olmaz.
3. Durumu **bağlı değil**olarak değiştirildiğini görmeniz gerekir. Ardından, uç noktanın listeden kaybolduğunu görürsünüz.

## <a name="validate-that-the-private-link-connection-works"></a>Özel bağlantı bağlantısının çalışıp çalışmadığını doğrulama

Özel uç nokta kaynağının aynı alt ağı içindeki kaynakların, özel bir IP adresi üzerinden Event Hubs ad alanına bağlanıp doğru özel DNS bölge tümleştirmesine sahip olduğunu doğrulamanız gerekir.

İlk olarak, [Azure Portal Windows sanal makinesi oluşturma](../virtual-machines/windows/quick-create-portal.md) bölümündeki adımları izleyerek bir sanal makine oluşturun

**Ağ** sekmesinde: 

1. **Sanal ağ** ve **alt ağ**belirtin. Özel uç noktasını dağıttığınız sanal ağı seçmeniz gerekir.
2. Genel bir **IP** kaynağı belirtin.
3. **NIC ağ güvenlik grubu**için **hiçbiri**' ni seçin.
4. **Yük Dengeleme**için **Hayır**' ı seçin.

VM 'ye bağlanın, komut satırını açın ve şu komutu çalıştırın:

```console
nslookup <event-hubs-namespace-name>.servicebus.windows.net
```

Aşağıdakine benzer bir sonuç görmeniz gerekir. 

```console
Non-authoritative answer:
Name:    <event-hubs-namespace-name>.privatelink.servicebus.windows.net
Address:  10.0.0.4 (private IP address associated with the private endpoint)
Aliases:  <event-hubs-namespace-name>.servicebus.windows.net
```

## <a name="limitations-and-design-considerations"></a>Sınırlamalar ve tasarım konuları

**Fiyatlandırma**: fiyatlandırma bilgileri için bkz. [Azure özel bağlantı fiyatlandırması](https://azure.microsoft.com/pricing/details/private-link/).

**Sınırlamalar**: Bu özellik tüm Azure genel bölgelerinde kullanılabilir.

**Event Hubs ad alanı başına en fazla özel uç nokta sayısı**: 120.

Daha fazla bilgi için bkz [. Azure özel bağlantı hizmeti: sınırlamalar](../private-link/private-link-service-overview.md#limitations)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure özel bağlantısı](../private-link/private-link-service-overview.md) hakkında daha fazla bilgi edinin
- [Azure Event Hubs](event-hubs-about.md) hakkında daha fazla bilgi
