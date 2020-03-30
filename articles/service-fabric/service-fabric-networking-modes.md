---
title: Kapsayıcı hizmetleri için ağ modlarını yapılandırma
description: Azure Hizmet Kumaşı tarafından desteklenen farklı ağ modlarını nasıl ayarladığınızı öğrenin.
author: athinanthny
ms.topic: conceptual
ms.date: 2/23/2018
ms.author: atsenthi
ms.openlocfilehash: ba1fa92559d39a481008d1dd18036e4232be1bfa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75639811"
---
# <a name="service-fabric-container-networking-modes"></a>Servis Kumaş konteyner ağ modları

Kapsayıcı hizmetleri için bir Azure Hizmet Kumaş ı kümesi varsayılan olarak **nat** ağ modunu kullanır. Aynı bağlantı noktasında birden fazla kapsayıcı hizmeti dinliyorsa ve nat modu kullanılıyorsa, dağıtım hataları oluşabilir. Aynı bağlantı noktasında dinleyen birden çok kapsayıcı hizmetini desteklemek için Service **Fabric, Açık** ağ modu (sürüm 5.7 ve sonrası) sunar. Açık modda, her kapsayıcı hizmeti, aynı bağlantı noktasında ndinleyen birden çok hizmeti destekleyen dahili, dinamik olarak atanmış bir IP adresine sahiptir.  

Hizmet bildiriminizde statik bir bitiş noktası olan bir kapsayıcı hizmetiniz varsa, dağıtım hatası olmadan Açık modunu kullanarak yeni hizmetler oluşturabilir ve silebilirsiniz. Aynı docker-compose.yml dosyası, birden çok hizmet oluşturmak için statik bağlantı noktası eşlemeleriyle de kullanılabilir.

Bir kapsayıcı hizmeti kümedeki başka bir düğüme yeniden başlandığında veya başka bir düğüme geçtiğinde IP adresi değişir. Bu nedenle, konteyner hizmetlerini keşfetmek için dinamik olarak atanmış IP adresini kullanmanızı önermiyoruz. Hizmet bulma için yalnızca Hizmet Kumaş Adlandırma Hizmeti veya DNS Hizmeti kullanılmalıdır. 

>[!WARNING]
>Azure, sanal ağ başına toplam 65.356 IP sağlar. Düğüm sayısının ve kapsayıcı hizmet örneklerinin (Açık mod kullanan) toplamı sanal ağdaki 65.356 IP'yi geçemez. Yüksek yoğunluklu senaryolar için nat ağ modu öneririz. Buna ek olarak, yük dengeleyicisi gibi diğer bağımlılıkların göz önünde bulundurulması gereken başka [sınırlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits) da olacaktır. Şu anda düğüm başına 50'ye kadar IP test edilmiş ve kararlı olduğu kanıtlanmıştır. 
>

## <a name="set-up-open-networking-mode"></a>Ağ modunu aç'ı ayarlama

1. Azure Kaynak Yöneticisi şablonu ayarla. Küme kaynağının **kumaş Ayarları** bölümünde, DNS Hizmetini ve IP Sağlayıcısını etkinleştirin: 

    ```json
    "fabricSettings": [
                {
                    "name": "DnsService",
                    "parameters": [
                       {
                            "name": "IsEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name": "Hosting",
                    "parameters": [
                      { 
                            "name": "IPProviderEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name": "Setup",
                    "parameters": [
                    {
                            "name": "ContainerNetworkSetup",
                            "value": "true"
                    }
                    ]
                }
            ],
    ```
    
2. Sanal Makine Ölçeği Set kaynağının ağ profili bölümünü ayarlayın. Bu, kümenin her düğümünde birden çok IP adresinin yapılandırılmasına olanak tanır. Aşağıdaki örnekte, bir Windows/Linux Service Fabric kümesi için düğüm başına beş IP adresi ayarlanır. Her düğümde bağlantı noktasını dinleyerek beş hizmet örneği olabilir. Beş IP'ye Azure Yük Bakiyesi'nden erişilebilmek için, beş IP'yi aşağıda gösterildiği gibi Azure Yük Bakiyesi Arka Uç Adres Havuzuna kaydedin.  Değişkenler bölümündeki değişkenleri şablonunuzun en üstüne eklemeniz de gerekir.

    Değişkenler için bu bölümü ekleyin:

    ```json
    "variables": {
        "nicName": "NIC",
        "vmName": "vm",
        "virtualNetworkName": "VNet",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "vmNodeType0Name": "[toLower(concat('NT1', variables('vmName')))]",
        "subnet0Name": "Subnet-0",
        "subnet0Prefix": "10.0.0.0/24",
        "subnet0Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet0Name'))]",
        "lbID0": "[resourceId('Microsoft.Network/loadBalancers',concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType0Name')))]",
        "lbIPConfig0": "[concat(variables('lbID0'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
        "lbPoolID0": "[concat(variables('lbID0'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
        "lbProbeID0": "[concat(variables('lbID0'),'/probes/FabricGatewayProbe')]",
        "lbHttpProbeID0": "[concat(variables('lbID0'),'/probes/FabricHttpGatewayProbe')]",
        "lbNatPoolID0": "[concat(variables('lbID0'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]"
    }
    ```
    
    Bu bölümü Sanal Makine Ölçeği Kümesi kaynağına ekleyin:

    ```json   
    "networkProfile": {
                "networkInterfaceConfigurations": [
                  {
                    "name": "[concat(parameters('nicName'), '-0')]",
                    "properties": {
                      "ipConfigurations": [
                        {
                          "name": "[concat(parameters('nicName'),'-',0)]",
                          "properties": {
                            "primary": "true",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "loadBalancerInboundNatPools": [
                              {
                                "id": "[variables('lbNatPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 1)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 2)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 3)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 4)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 5)]",
                          "properties": {
                            "primary": "false",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        }
                      ],
                      "primary": true
                    }
                  }
                ]
              }
   ```
 
3. Yalnızca Windows kümeleri için, aşağıdaki değerlere sahip sanal ağ için UDP/53 bağlantı noktasını açan bir Azure Ağ Güvenlik Grubu (NSG) kuralı ayarlayın:

   |Ayar |Değer | |
   | --- | --- | --- |
   |Öncelik |2000 | |
   |Adı |Custom_Dns  | |
   |Kaynak |VirtualNetwork | |
   |Hedef | VirtualNetwork | |
   |Hizmet | DNS (UDP/53) | |
   |Eylem | İzin Ver  | |
   | | |

4. Her hizmet için uygulama bildiriminde ağ `<NetworkConfig NetworkType="Open">`modunu belirtin: . **Açık** ağ modu, hizmetin özel bir IP adresi almasıyla sonuçlanır. Bir mod belirtilmemişse, hizmet **varsayılan olarak nat** moduna geçer. Aşağıdaki bildirim örneğinde, `NodeContainerServicePackage1` `NodeContainerServicePackage2` ve hizmetlerin her biri aynı bağlantı noktasında `Endpoint1`dinlenebilir (her iki hizmet de dinliyor). Açık ağ modu belirtildiğinde, `PortBinding` yapılandırmalar belirtilemez.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ApplicationManifest ApplicationTypeName="NodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
      <Description>Calculator Application</Description>
      <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
        <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      </Parameters>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage1" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService1.Code" Isolation="hyperv">
           <NetworkConfig NetworkType="Open"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage2" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService2.Code" Isolation="default">
            <NetworkConfig NetworkType="Open"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
    </ApplicationManifest>
    ```

    Bir Windows kümesi için bir uygulama içindeki hizmetler arasında farklı ağ modlarını karıştırıp eşleştirebilirsiniz. Bazı hizmetler Açık modunu, diğerleri nat modunu kullanabilir. Bir hizmet nat modunu kullanacak şekilde yapılandırıldığında, hizmetin dinlediği bağlantı noktası benzersiz olmalıdır.

    >[!NOTE]
    >Linux kümelerinde, farklı hizmetler için ağ karıştırma modları desteklenmez. 
    >

5. **Açık** modu seçildiğinde, hizmet bildirimindeki **Bitiş Noktası** tanımı, hizmet paketinde yalnızca bir kod paketi olsa bile, bitiş noktasına karşılık gelen kod paketini açıkça göstermelidir. 
   
   ```xml
   <Resources>
     <Endpoints>
       <Endpoint Name="ServiceEndpoint" Protocol="http" Port="80" CodePackageRef="Code"/>
     </Endpoints>
   </Resources>
   ```
   
6. Windows için, VM yeniden başlatma açık ağ yeniden oluşturulmasına neden olur. Bu, ağ yığınındaki temel bir sorunu azaltmak için kullanılır. Varsayılan davranış, ağı yeniden oluşturmaktır. Bu davranışın kapatılması gerekiyorsa, aşağıdaki yapılandırma bir config yükseltme si tarafından kullanılabilir.

```json
"fabricSettings": [
                {
                    "name": "Setup",
                    "parameters": [
                    {
                            "name": "SkipContainerNetworkResetOnReboot",
                            "value": "true"
                    }
                    ]
                }
            ],          
 ``` 
 
## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric uygulama modelini anlama](service-fabric-application-model.md)
* [Service Fabric hizmet bildirim kaynakları hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-manifest-resources)
* [Windows Server 2016'da Service Fabric'e Windows kapsayıcısı dağıtma](service-fabric-get-started-containers.md)
* [Linux'ta Service Fabric'e Docker konteyneri dağıtma](service-fabric-get-started-containers-linux.md)
