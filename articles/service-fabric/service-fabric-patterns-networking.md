---
title: Azure Hizmet Kumaşı için ağ desenleri
description: Hizmet Kumaşı için ortak ağ desenleri ve Azure ağ özelliklerini kullanarak küme oluşturma yöntemini açıklar.
ms.topic: conceptual
ms.date: 01/19/2018
ms.openlocfilehash: 065c311fffe409b20e02a3fddf1e9e7e6a82a2a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75466281"
---
# <a name="service-fabric-networking-patterns"></a>Servis Kumaş ağ desenleri
Azure Hizmet Kumaşı kümenizi diğer Azure ağ özellikleriyle tümleştirebilirsiniz. Bu makalede, aşağıdaki özellikleri kullanan kümelerin nasıl oluşturulabileceğinizi göstereceğiz:

- [Varolan sanal ağ veya alt ağ](#existingvnet)
- [Statik genel IP adresi](#staticpublicip)
- [Yalnızca dahili yük dengeleyicisi](#internallb)
- [İç ve dış yük dengeleyicisi](#internalexternallb)

Service Fabric standart sanal makine ölçeği kümesinde çalışır. Sanal makine ölçek kümesinde kullanabileceğiniz herhangi bir işlevselliği, Service Fabric kümesi ile kullanabilirsiniz. Sanal makine ölçek kümeleri ve Hizmet Kumaşı için Azure Kaynak Yöneticisi şablonlarının ağ bölümleri aynıdır. Varolan bir sanal ağa dağıtım yaptıktan sonra Azure ExpressRoute, Azure VPN Ağ Geçidi, ağ güvenlik grubu ve sanal ağ eşleme gibi diğer ağ özelliklerini kolayca dahil edebilirsiniz.

### <a name="allowing-the-service-fabric-resource-provider-to-query-your-cluster"></a>Service Fabric kaynak sağlayıcısının kümenizi sorgulamasına izin verme

Service Fabric bir yönüyle diğer ağ özelliklerinden benzersizdir. [Azure portalı,](https://portal.azure.com) düğümler ve uygulamalar hakkında bilgi almak için kümeye çağrı yapmak için Service Fabric kaynak sağlayıcısını dahili olarak kullanır. Service Fabric kaynak sağlayıcısı, yönetim bitiş noktasındaki HTTP ağ geçidi bağlantı noktasına (varsayılan olarak 19080 bağlantı noktası) genel olarak erişilebilir giriş erişimi gerektirir. [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) kümenizi yönetmek için yönetim bitiş noktasını kullanır. Service Fabric kaynak sağlayıcısı, bu bağlantı noktasını kümenle ilgili bilgileri sorgulamak ve Azure portalında görüntülemek için de kullanır. 

Bağlantı noktası 19080'e Service Fabric kaynak sağlayıcısından erişilemiyorsa, *portalda Bulunamadı Düğümler* gibi bir ileti görüntülenir ve düğümünüz ve uygulama listeniz boş görünür. Kümenizi Azure portalında görmek istiyorsanız, yük bakiyecinizin herkese açık bir IP adresi ortaya çıkarması ve ağ güvenlik grubunuzun gelen bağlantı noktası 19080 trafiğine izin vermesi gerekir. Kurulumunuz bu gereksinimleri karşılamazsa, Azure portalı kümenizin durumunu görüntülemez.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="templates"></a>Şablonlar

Tüm Hizmet Kumaşı [GitHub](https://github.com/Azure/service-fabric-scripts-and-templates/tree/master/templates/networking)şablonları GitHub'dadır. Aşağıdaki PowerShell komutlarını kullanarak şablonları olduğu gibi dağıtabilmelisiniz. Varolan Azure Sanal Ağ şablonuna veya statik genel IP şablonuna dağıtıyorsanız, önce bu makalenin [İlk kurulum](#initialsetup) bölümünü okuyun.

<a id="initialsetup"></a>
## <a name="initial-setup"></a>İlk kurulum

### <a name="existing-virtual-network"></a>Mevcut sanal ağ

Aşağıdaki örnekte, **ExistingRG** kaynak grubunda ExistingRG-vnet adlı mevcut bir sanal ağla başlarız. Alt ağ varsayılan olarak adlandırılır. Bu varsayılan kaynaklar, standart bir sanal makine (VM) oluşturmak için Azure portalını kullandığınızda oluşturulur. VM oluşturmadan sanal ağ ve alt ağ oluşturabilirsiniz, ancak varolan bir sanal ağa küme eklemenin temel amacı diğer VM'lere ağ bağlantısı sağlamaktır. VM'yi oluşturmak, varolan bir sanal ağın genellikle nasıl kullanıldığına iyi bir örnek verir. Service Fabric kümenizde genel BIR IP adresi olmadan yalnızca bir dahili yük dengeleyici sayılsa, VM ve genel IP'sini güvenli bir *atlama kutusu*olarak kullanabilirsiniz.

### <a name="static-public-ip-address"></a>Statik genel IP adresi

Statik genel IP adresi genellikle atandığı VM veya VM'lerden ayrı olarak yönetilen özel bir kaynaktır. Özel bir ağ kaynak grubunda (Service Fabric küme kaynak grubunun kendisinden farklı olarak) karşınızda dır. Azure portalında veya PowerShell'i kullanarak aynı ExistingRG kaynak grubunda staticIP1 adlı statik bir genel IP adresi oluşturun:

```powershell
PS C:\Users\user> New-AzPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG -Location westus -AllocationMethod Static -DomainNameLabel sfnetworking

Name                     : staticIP1
ResourceGroupName        : ExistingRG
Location                 : westus
Id                       : /subscriptions/1237f4d2-3dce-1236-ad95-123f764e7123/resourceGroups/ExistingRG/providers/Microsoft.Network/publicIPAddresses/staticIP1
Etag                     : W/"fc8b0c77-1f84-455d-9930-0404ebba1b64"
ResourceGuid             : 77c26c06-c0ae-496c-9231-b1a114e08824
ProvisioningState        : Succeeded
Tags                     :
PublicIpAllocationMethod : Static
IpAddress                : 40.83.182.110
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : null
DnsSettings              : {
                             "DomainNameLabel": "sfnetworking",
                             "Fqdn": "sfnetworking.westus.cloudapp.azure.com"
                           }
```

### <a name="service-fabric-template"></a>Servis Kumaşı şablonu

Bu makaledeki örneklerde Service Fabric template.json'u kullanıyoruz. Bir küme oluşturmadan önce şablonu portaldan indirmek için standart portal sihirbazını kullanabilirsiniz. Ayrıca, [güvenli beş düğümlü Service Fabric kümesi](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure)gibi örnek [şablonlardan](https://github.com/Azure-Samples/service-fabric-cluster-templates)birini de kullanabilirsiniz.

<a id="existingvnet"></a>
## <a name="existing-virtual-network-or-subnet"></a>Varolan sanal ağ veya alt ağ

1. Alt ağ parametresini varolan alt ağ adıyla değiştirin ve varolan sanal ağa başvurmak için iki yeni parametre ekleyin:

    ```json
        "subnet0Name": {
                "type": "string",
                "defaultValue": "default"
            },
            "existingVNetRGName": {
                "type": "string",
                "defaultValue": "ExistingRG"
            },

            "existingVNetName": {
                "type": "string",
                "defaultValue": "ExistingRG-vnet"
            },
            /*
            "subnet0Name": {
                "type": "string",
                "defaultValue": "Subnet-0"
            },
            "subnet0Prefix": {
                "type": "string",
                "defaultValue": "10.0.0.0/24"
            },*/
    ```

2. Varolan `nicPrefixOverride` alt `Microsoft.Compute/virtualMachineScaleSets`ağı kullandığınız ve bu değişkeni adım 1'de devre dışı bıraktığınızdan dolayı açıklama dışında açıklama yapın.

    ```json
            /*"nicPrefixOverride": "[parameters('subnet0Prefix')]",*/
    ```

3. Değişkeni `vnetID` varolan sanal ağa işaret etmek için değiştirin:

    ```json
            /*old "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",*/
            "vnetID": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingVNetRGName'), '/providers/Microsoft.Network/virtualNetworks/', parameters('existingVNetName'))]",
    ```

4. Azure'un yeni bir sanal ağ oluşturmaması için kaynaklarınızdan kaldırın: `Microsoft.Network/virtualNetworks`

    ```json
    /*{
    "apiVersion": "[variables('vNetApiVersion')]",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('virtualNetworkName')]",
    "location": "[parameters('computeLocation')]",
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "subnets": [
            {
                "name": "[parameters('subnet0Name')]",
                "properties": {
                    "addressPrefix": "[parameters('subnet0Prefix')]"
                }
            }
        ]
    },
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    }
    },*/
    ```

5. Yeni bir sanal ağ `dependsOn` oluşturmaya `Microsoft.Compute/virtualMachineScaleSets`bağlı kalmamanız için sanal ağı özdeyiş özelliğinden yorum yapın:

    ```json
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Computer/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType0Name')]",
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        /*"[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]",
        */
        "[Concat('Microsoft.Storage/storageAccounts/', variables('uniqueStringArray0')[0])]",

    ```

6. Şablonu dağıtın:

    ```powershell
    New-AzResourceGroup -Name sfnetworkingexistingvnet -Location westus
    New-AzResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingexistingvnet -TemplateFile C:\SFSamples\Final\template\_existingvnet.json
    ```

    Dağıtımdan sonra, sanal ağınız yeni ölçek kümesi VM'leri içermelidir. Sanal makine ölçeği kümesi düğüm türü varolan sanal ağ ve alt ağı göstermelidir. Sanal ağda bulunan VM'ye erişmek ve yeni ölçek kümesi VM'leri pinglemek için Uzak Masaüstü Protokolü'nü (RDP) de kullanabilirsiniz:

    ```
    C:>\Users\users>ping 10.0.0.5 -n 1
    C:>\Users\users>ping NOde1000000 -n 1
    ```

Başka bir örnek için, [Hizmet Kumaşözel olmayan bir](https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet)bakın.


<a id="staticpublicip"></a>
## <a name="static-public-ip-address"></a>Statik genel IP adresi

1. Varolan statik IP kaynak grubunun adı, adı ve tam nitelikli etki alanı adı (FQDN) adı için parametreler ekleyin:

    ```json
    "existingStaticIPResourceGroup": {
                "type": "string"
            },
            "existingStaticIPName": {
                "type": "string"
            },
            "existingStaticIPDnsFQDN": {
                "type": "string"
    }
    ```

2. Parametreyi `dnsName` kaldırın. (Statik IP adresi zaten bir tane vardır.)

    ```json
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

3. Varolan statik IP adresine başvurmak için bir değişken ekleyin:

    ```json
    "existingStaticIP": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingStaticIPResourceGroup'), '/providers/Microsoft.Network/publicIPAddresses/', parameters('existingStaticIPName'))]",
    ```

4. Azure'un yeni bir IP adresi oluşturmaması için kaynaklarınızdan kaldırın: `Microsoft.Network/publicIPAddresses`

    ```json
    /*
    {
        "apiVersion": "[variables('publicIPApiVersion')]",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[concat(parameters('lbIPName'),)'-', '0')]",
        "location": "[parameters('computeLocation')]",
        "properties": {
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsName')]"
            },
            "publicIPAllocationMethod": "Dynamic"        
        },
        "tags": {
            "resourceType": "Service Fabric",
            "clusterName": "[parameters('clusterName')]"
        }
    }, */
    ```

5. Ip adresini öznitelikten `dependsOn` `Microsoft.Network/loadBalancers`yorumyapın, böylece yeni bir IP adresi oluşturmaya bağlı değilsiniz:

    ```json
    "apiVersion": "[variables('lbIPApiVersion')]",
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB', '-', parameters('clusterName'), '-', parameters('vmNodeType0Name'))]",
    "location": "[parameters('computeLocation')]",
    /*
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', concat(parameters('lbIPName'), '-', '0'))]"
    ], */
    "properties": {
    ```

6. Kaynakta, yeni `publicIPAddress` oluşturulan `frontendIPConfigurations` bir tane yerine varolan statik IP adresine başvuru için öğeyi değiştirin: `Microsoft.Network/loadBalancers`

    ```json
                "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                "publicIPAddress": {
                                    /*"id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"*/
                                    "id": "[variables('existingStaticIP')]"
                                }
                            }
                        }
                    ],
    ```

7. Kaynakta, `Microsoft.ServiceFabric/clusters` statik `managementEndpoint` IP adresinin DNS FQDN'sine değiştirin. Güvenli bir küme kullanıyorsanız, *http://* *https://'* e değiştirdiğinden emin olun. (Bu adımın yalnızca Service Fabric kümeleri için geçerli olduğunu unutmayın. Sanal makine ölçek kümesi kullanıyorsanız, bu adımı atlayın.)

    ```json
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',parameters('existingStaticIPDnsFQDN'),':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

8. Şablonu dağıtın:

    ```powershell
    New-AzResourceGroup -Name sfnetworkingstaticip -Location westus

    $staticip = Get-AzPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG

    $staticip

    New-AzResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingstaticip -TemplateFile C:\SFSamples\Final\template\_staticip.json -existingStaticIPResourceGroup $staticip.ResourceGroupName -existingStaticIPName $staticip.Name -existingStaticIPDnsFQDN $staticip.DnsSettings.Fqdn
    ```

Dağıtımdan sonra, yük bakiyecinizin diğer kaynak grubundan ortak statik IP adresine bağlı olduğunu görebilirsiniz. Hizmet Kumaşı bağlantı bitiş noktası ve [Hizmet Kumaş ıstabiri](service-fabric-visualizing-your-cluster.md) statik IP adresinin DNS FQDN'sine işaret eder.

<a id="internallb"></a>
## <a name="internal-only-load-balancer"></a>Yalnızca dahili yük dengeleyicisi

Bu senaryo, varsayılan Hizmet Kumaşı şablonundaki harici yük dengeleyicisinin yerine yalnızca dahili yük dengeleyicisi ile değiştirilir. Azure portalı ve Service Fabric kaynak sağlayıcısı için etkileri için [makalenin daha önceki](#allowing-the-service-fabric-resource-provider-to-query-your-cluster) yazılarına bakın.

1. Parametreyi `dnsName` kaldırın. (Buna gerek yok.)

    ```json
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

2. İsteğe bağlı olarak, statik ayırma yöntemi kullanıyorsanız, statik BIR IP adresi parametresi ekleyebilirsiniz. Dinamik bir ayırma yöntemi kullanıyorsanız, bu adımı yapmanız gerekmez.

    ```json
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

3. Azure'un yeni bir IP adresi oluşturmaması için kaynaklarınızdan kaldırın: `Microsoft.Network/publicIPAddresses`

    ```json
    /*
    {
        "apiVersion": "[variables('publicIPApiVersion')]",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[concat(parameters('lbIPName'),)'-', '0')]",
        "location": "[parameters('computeLocation')]",
        "properties": {
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsName')]"
            },
            "publicIPAllocationMethod": "Dynamic"        
        },
        "tags": {
            "resourceType": "Service Fabric",
            "clusterName": "[parameters('clusterName')]"
        }
    }, */
    ```

4. Yeni bir `dependsOn` IP adresi `Microsoft.Network/loadBalancers`oluşturmaya bağlı olmayacak şekilde IP adresi özniteliğini kaldırın. Yük dengeleyici `dependsOn` artık sanal ağdan alt net bağlıdır, çünkü sanal ağ özniteliği ekleyin:

    ```json
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'))]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /*"[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"*/
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
    ```

5. Yük dengeleyicisinin ayarını `frontendIPConfigurations` bir `publicIPAddress`alt ağ kullanarak değiştirme `privateIPAddress`ve . `privateIPAddress`önceden tanımlanmış statik bir iç IP adresi kullanır. Dinamik bir IP adresi kullanmak `privateIPAddress` için öğeyi `privateIPAllocationMethod` kaldırın ve ardından **Dinamik**olarak değiştirin.

    ```json
                "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                /*
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"
                                } */
                                "subnet" :{
                                    "id": "[variables('subnet0Ref')]"
                                },
                                "privateIPAddress": "[parameters('internalLBAddress')]",
                                "privateIPAllocationMethod": "Static"
                            }
                        }
                    ],
    ```

6. Kaynakta, `Microsoft.ServiceFabric/clusters` iç `managementEndpoint` yük dengeleyici adresine işaret etmek için değiştirin. Güvenli bir küme kullanıyorsanız, *https:// http://* değiştirdiğinden emin *olun.* (Bu adımın yalnızca Service Fabric kümeleri için geçerli olduğunu unutmayın. Sanal makine ölçek kümesi kullanıyorsanız, bu adımı atlayın.)

    ```json
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',reference(variables('lbID0')).frontEndIPConfigurations[0].properties.privateIPAddress,':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

7. Şablonu dağıtın:

    ```powershell
    New-AzResourceGroup -Name sfnetworkinginternallb -Location westus

    New-AzResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternallb -TemplateFile C:\SFSamples\Final\template\_internalonlyLB.json
    ```

Dağıtımdan sonra yük bakiyeciniz özel statik 10.0.0.250 IP adresini kullanır. Aynı sanal ağda başka bir makineniz varsa, dahili [Servis Kumaş Explorer](service-fabric-visualizing-your-cluster.md) bitiş noktasına gidebilirsiniz. Yük dengeleyicisinin arkasındaki düğümlerden birine bağlandığını unutmayın.

<a id="internalexternallb"></a>
## <a name="internal-and-external-load-balancer"></a>İç ve dış yük dengeleyicisi

Bu senaryoda, varolan tek düğüm türü dış yük dengeleyicisiyle başlar ve aynı düğüm türü için bir iç yük dengeleyicisi eklersiniz. Arka uç adres havuzuna bağlı bir arka uç bağlantı noktası yalnızca tek bir yük dengeleyicisine atanabilir. Uygulama bağlantı noktalarınızın hangi yük dengeleyicisi olacağını ve hangi yük dengeleyicinin yönetim bitiş noktalarına sahip olacağını (19000 ve 19080 bağlantı noktaları) seçin. Yönetim uç noktalarını iç yük dengeleyiciye koyarsanız, makalede daha önce tartışılan Service Fabric kaynak sağlayıcı kısıtlamalarını aklınızda [bulundurun.](#allowing-the-service-fabric-resource-provider-to-query-your-cluster) Kullandığımız örnekte, yönetim uç noktaları dış yük dengeleyicisinde kalır. Ayrıca bir bağlantı noktası 80 uygulama bağlantı noktası ekleyin ve iç yük dengeleyici üzerine yerleştirin.

İki düğüm tipi kümede, bir düğüm türü dış yük dengeleyicisi üzerindedir. Diğer düğüm türü iç yük dengeleyicisi üzerindedir. Portal tarafından oluşturulan iki düğüm tipi şablonda (iki yük dengeleyicisiyle birlikte gelen) iki düğüm tipi küme kullanmak için ikinci yük dengeleyicisini bir iç yük dengeleyicisine geçin. Daha fazla bilgi için [yalnızca Dahili yük dengeleyicisi](#internallb) bölümüne bakın.

1. Statik iç yük dengeleyici IP adresi parametresini ekleyin. (Dinamik bir IP adresi kullanmayla ilgili notlar için bu makalenin önceki bölümlerine bakın.)

    ```json
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

2. Bir uygulama bağlantı noktası 80 parametresi ekleyin.

3. Varolan ağ değişkenlerinin dahili sürümlerini eklemek için bunları kopyalayıp yapıştırın ve ada "-Int" ekleyin:

    ```json
    /* Add internal load balancer networking variables */
            "lbID0-Int": "[resourceId('Microsoft.Network/loadBalancers', concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal'))]",
            "lbIPConfig0-Int": "[concat(variables('lbID0-Int'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
            "lbPoolID0-Int": "[concat(variables('lbID0-Int'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
            "lbProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricGatewayProbe')]",
            "lbHttpProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricHttpGatewayProbe')]",
            "lbNatPoolID0-Int": "[concat(variables('lbID0-Int'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]",
            /* Internal load balancer networking variables end */
    ```

4. Uygulama bağlantı noktası 80'i kullanan portal oluşturulan şablonla başlarsanız, varsayılan portal şablonu harici yük dengeleyiciye AppPort1 (bağlantı noktası 80) ekler. Bu durumda, AppPort1'i harici yük `loadBalancingRules` dengeleyicisinden ve problarından çıkarın, böylece dahili yük dengeleyicisine ekleyebilirsiniz:

    ```json
    "loadBalancingRules": [
        {
            "name": "LBHttpRule",
            "properties":{
                "backendAddressPool": {
                    "id": "[variables('lbPoolID0')]"
                },
                "backendPort": "[parameters('nt0fabricHttpGatewayPort')]",
                "enableFloatingIP": "false",
                "frontendIPConfiguration": {
                    "id": "[variables('lbIPConfig0')]"            
                },
                "frontendPort": "[parameters('nt0fabricHttpGatewayPort')]",
                "idleTimeoutInMinutes": "5",
                "probe": {
                    "id": "[variables('lbHttpProbeID0')]"
                },
                "protocol": "tcp"
            }
        } /* Remove AppPort1 from the external load balancer.
        {
            "name": "AppPortLBRule1",
            "properties": {
                "backendAddressPool": {
                    "id": "[variables('lbPoolID0')]"
                },
                "backendPort": "[parameters('loadBalancedAppPort1')]",
                "enableFloatingIP": "false",
                "frontendIPConfiguration": {
                    "id": "[variables('lbIPConfig0')]"            
                },
                "frontendPort": "[parameters('loadBalancedAppPort1')]",
                "idleTimeoutInMinutes": "5",
                "probe": {
                    "id": "[concate(variables('lbID0'), '/probes/AppPortProbe1')]"
                },
                "protocol": "tcp"
            }
        }*/

    ],
    "probes": [
        {
            "name": "FabricGatewayProbe",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('nt0fabricTcpGatewayPort')]",
                "protocol": "tcp"
            }
        },
        {
            "name": "FabricHttpGatewayProbe",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('nt0fabricHttpGatewayPort')]",
                "protocol": "tcp"
            }
        } /* Remove AppPort1 from the external load balancer.
        {
            "name": "AppPortProbe1",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('loadBalancedAppPort1')]",
                "protocol": "tcp"
            }
        } */

    ],
    "inboundNatPools": [
    ```

5. İkinci `Microsoft.Network/loadBalancers` bir kaynak ekleyin. [Yalnızca Dahili yük dengeleyicisi](#internallb) bölümünde oluşturulan dahili yük dengeleyicisine benzer, ancak "-Int" yük dengeleyici değişkenlerini kullanır ve yalnızca uygulama bağlantı noktası 80'i uygular. Bu da `inboundNatPools`kaldırır , kamu yük dengeleyici üzerinde RDP uç noktaları tutmak için. RdP'yi dahili yük dengeleyicisi üzerinde istiyorsanız, dış yük dengeleyicisinden bu dahili yük dengeleyicisine geçin: `inboundNatPools`

    ```json
            /* Add a second load balancer, configured with a static privateIPAddress and the "-Int" load balancer variables. */
            {
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                /* Add "-Internal" to the name. */
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal')]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /* Remove public IP dependsOn, add vnet dependsOn
                    "[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"
                    */
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
                "properties": {
                    "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                /* Switch from Public to Private IP address
                                */
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"
                                }
                                */
                                "subnet" :{
                                    "id": "[variables('subnet0Ref')]"
                                },
                                "privateIPAddress": "[parameters('internalLBAddress')]",
                                "privateIPAllocationMethod": "Static"
                            }
                        }
                    ],
                    "backendAddressPools": [
                        {
                            "name": "LoadBalancerBEAddressPool",
                            "properties": {}
                        }
                    ],
                    "loadBalancingRules": [
                        /* Add the AppPort rule. Be sure to reference the "-Int" versions of backendAddressPool, frontendIPConfiguration, and the probe variables. */
                        {
                            "name": "AppPortLBRule1",
                            "properties": {
                                "backendAddressPool": {
                                    "id": "[variables('lbPoolID0-Int')]"
                                },
                                "backendPort": "[parameters('loadBalancedAppPort1')]",
                                "enableFloatingIP": "false",
                                "frontendIPConfiguration": {
                                    "id": "[variables('lbIPConfig0-Int')]"
                                },
                                "frontendPort": "[parameters('loadBalancedAppPort1')]",
                                "idleTimeoutInMinutes": "5",
                                "probe": {
                                    "id": "[concat(variables('lbID0-Int'),'/probes/AppPortProbe1')]"
                                },
                                "protocol": "tcp"
                            }
                        }
                    ],
                    "probes": [
                    /* Add the probe for the app port. */
                    {
                            "name": "AppPortProbe1",
                            "properties": {
                                "intervalInSeconds": 5,
                                "numberOfProbes": 2,
                                "port": "[parameters('loadBalancedAppPort1')]",
                                "protocol": "tcp"
                            }
                        }
                    ],
                    "inboundNatPools": [
                    ]
                },
                "tags": {
                    "resourceType": "Service Fabric",
                    "clusterName": "[parameters('clusterName')]"
                }
            },
    ```

6. Kaynak `networkProfile` için `Microsoft.Compute/virtualMachineScaleSets` iç arka uç adresi havuzunu ekleyin:

    ```json
    "loadBalancerBackendAddressPools": [
                                                        {
                                                            "id": "[variables('lbPoolID0')]"
                                                        },
                                                        {
                                                            /* Add internal BE pool */
                                                            "id": "[variables('lbPoolID0-Int')]"
                                                        }
    ],
    ```

7. Şablonu dağıtın:

    ```powershell
    New-AzResourceGroup -Name sfnetworkinginternalexternallb -Location westus

    New-AzResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternalexternallb -TemplateFile C:\SFSamples\Final\template\_internalexternalLB.json
    ```

Dağıtımdan sonra kaynak grubunda iki yük dengeleyicisi görebilirsiniz. Yük dengeleyicilerine göz atarsanız, ortak IP adresine atanan genel IP adresini ve yönetim bitiş noktalarını (19000 ve 19080 bağlantı noktaları) görebilirsiniz. Ayrıca, dahili yük dengeleyicisine atanan statik iç IP adresini ve uygulama bitiş noktasını (bağlantı noktası 80) da görebilirsiniz. Her iki yük dengeleyicisi de arka uç havuzunu ayarlayan aynı sanal makine ölçeğini kullanır.

## <a name="notes-for-production-workloads"></a>Üretim iş yükleri için notlar

Yukarıdaki GitHub şablonları, Temel SKU olan Azure Standart Yük Dengeleyicisi (SLB) için varsayılan SKU ile çalışacak şekilde tasarlanmıştır. Bu SLB'nin SLA'sı yoktur, bu nedenle üretim iş yükleri için Standart SKU kullanılmalıdır. Bu konuda daha fazla bilgi için [Azure Standart Yük Dengeleyicisi'ne genel bakış](/azure/load-balancer/load-balancer-standard-overview)konusuna bakın. SLB için Standart SKU'yu kullanan herhangi bir Hizmet Kumaş ı kümesi, her düğüm türünün 443 numaralı bağlantı noktasında giden trafiğe izin veren bir kurala sahip olduğundan emin olmak zorundadır. Bu küme kurulumu tamamlamak için gereklidir ve böyle bir kural olmadan herhangi bir dağıtım başarısız olur. Yukarıdaki "yalnızca iç" yük dengeleyicisi örneğinde, 443 nolu bağlantı noktası için giden trafiğe izin veren bir kuralla şablona ek bir harici yük dengeleyici eklenmelidir.

## <a name="next-steps"></a>Sonraki adımlar
[Küme oluşturma](service-fabric-cluster-creation-via-arm.md)

Dağıtımdan sonra kaynak grubunda iki yük dengeleyicisi görebilirsiniz. Yük dengeleyicilerine göz atarsanız, ortak IP adresine atanan genel IP adresini ve yönetim bitiş noktalarını (19000 ve 19080 bağlantı noktaları) görebilirsiniz. Ayrıca, dahili yük dengeleyicisine atanan statik iç IP adresini ve uygulama bitiş noktasını (bağlantı noktası 80) da görebilirsiniz. Her iki yük dengeleyicisi de arka uç havuzunu ayarlayan aynı sanal makine ölçeğini kullanır.

