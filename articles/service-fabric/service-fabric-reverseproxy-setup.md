---
title: Azure Hizmet Kumaşı ters proxy'yi ayarlama
description: Azure Service Fabric uygulaması için ters proxy hizmetini nasıl ayarlayacağınizi ve yapılandırın.
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: pepogors
ms.openlocfilehash: 6e3edb0fe238dcaddb7d99cc68660591f081581c
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80476679"
---
# <a name="set-up-and-configure-reverse-proxy-in-azure-service-fabric"></a>Azure Hizmet Kumaşı'nda ters proxy ayarlama ve yapılandırma
Ters proxy, Hizmet Kumaşı kümesinde çalışan mikro hizmetlerin http uç noktaları olan diğer hizmetleri keşfetmeye ve onlarla iletişim kurmasına yardımcı olan isteğe bağlı bir Azure Hizmet Kumaşhizmeti hizmetidir. Daha fazla bilgi için [Azure Hizmet Kumaşı'nda Ters proxy'ye](service-fabric-reverseproxy.md)bakın. Bu makalede, kümenizde ters proxy'yi nasıl ayarlayıp yapılandırabileceğiniz gösterilmektedir. 

## <a name="enable-reverse-proxy-using-azure-portal"></a>Azure portalını kullanarak ters proxy'yi etkinleştirme

Azure portalı, yeni bir Hizmet Kumaşı kümesi oluşturduğunuzda ters proxy'yi etkinleştirme seçeneği sunar. Portal üzerinden ters proxy kullanmak için varolan bir kümeyi yükseltemezsiniz. 

[Azure portalını kullanarak bir küme oluştururken](./service-fabric-cluster-creation-via-portal.md)ters proxy'yi yapılandırmak için aşağıdakileri yaptığınızdan emin olun:

1. **Adım 2: Küme Yapılandırması**, **Düğüm türü yapılandırma**altında , ters proxy **etkinleştir'i**seçin.

   ![Portalda ters proxy'yi etkinleştirme](./media/service-fabric-reverseproxy-setup/enable-rp-portal.png)
2. (İsteğe bağlı) Güvenli ters proxy yapılandırmak için bir TLS/SSL sertifikası yapılandırmanız gerekir. **Adım 3: Güvenlik**, Yapılandırma **türü**altında küme **güvenlik ayarlarını yapılandırma,** **Özel**seçin . Ardından, **Ters Proxy SSL sertifikası**altında, **ters proxy için bir SSL sertifikası ekle'yi** seçin ve sertifika bilgilerinizi girin.

   ![Portalda güvenli ters proxy yapılandırma](./media/service-fabric-reverseproxy-setup/configure-rp-certificate-portal.png)

   Küme oluştururken ters proxy'yi bir sertifikayla yapılandırmamayı seçerseniz, bunu daha sonra kümenin kaynak grubu için Kaynak Yöneticisi şablonu aracılığıyla yapabilirsiniz. Daha fazla bilgi için bkz: [Azure Kaynak Yöneticisi şablonları aracılığıyla ters proxy'yi etkinleştir.](#enable-reverse-proxy-via-azure-resource-manager-templates)

## <a name="enable-reverse-proxy-via-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonları aracılığıyla ters proxy'yi etkinleştirme

Azure'daki kümeler için, Hizmet Kumaşı'nda ters proxy'yi etkinleştirmek için Azure Kaynak Yöneticisi şablonunu kullanabilirsiniz. Kümeyi oluştururken ters proxy'yi etkinleştirebilir veya kümeyi daha sonra güncelleştirerek etkinleştirebilirsiniz. 

Yeni bir küme için [özel bir Kaynak Yöneticisi şablonu oluşturabilir](service-fabric-cluster-creation-via-arm.md) veya örnek bir şablon kullanabilirsiniz. 

GitHub'daki [Güvenli Ters Proxy Örnek Şablonları'nda](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/Reverse-Proxy-Sample) Bir Azure kümesi için güvenli ters proxy yapılandırmanıza yardımcı olabilecek örnek Kaynak Yöneticisi şablonları bulabilirsiniz. Https Ters Proxy'yi, sertifikayla güvenli ters proxy'yi yapılandırmak ve sertifika devrini işlemek için kullanım için README [dosyasındaki güvenli bir kümede yapılandırmaya](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/Reverse-Proxy-Sample/README.md#configure-https-reverse-proxy-in-a-secure-cluster) bakın.

Varolan bir küme için, [Azure portalı](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template) [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-powershell)veya [Azure CLI'yi](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-cli)kullanarak kümenin kaynak grubu için Kaynak Yöneticisi şablonu dışa aktarabilirsiniz.

Kaynak Yöneticisi şablonunuz olduktan sonra, ters proxy'yi aşağıdaki adımlarla etkinleştirebilirsiniz:

1. Şablonun [Parametreler bölümünde](../azure-resource-manager/templates/template-syntax.md) ters proxy için bir bağlantı noktası tanımlayın.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19081,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. [**Microsoft.ServiceFabric/clusters**](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/clusters) [Kaynak türü bölümündeki](../azure-resource-manager/templates/template-syntax.md)düğüm tipi nesnelerinher biri için bağlantı noktasını belirtin.

    Bağlantı noktası, tersProxyEndpointPort parametresi adı ile tanımlanır.

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```
3. Ters proxy için bağlantı noktasındaTLS/SSL sertifikalarını yapılandırmak için, sertifikayı **Microsoft.ServiceFabric/clusters** [Kaynak türü bölümündeki](../resource-group-authoring-templates.md)ters ***ProxySertifikası*** özelliğine ekleyin.

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

### <a name="supporting-a-reverse-proxy-certificate-thats-different-from-the-cluster-certificate"></a>Küme sertifikasından farklı bir ters proxy sertifikasını destekleme
 Ters proxy sertifikası kümeyi güvenli sertifikadan farklıysa, daha önce belirtilen sertifika sanal makineye yüklenmeli ve Hizmet Kumaşı'nın erişebilmeleri için erişim denetim listesine (ACL) eklenmelidir. Bu [**Microsoft.Compute /virtualMachineScaleSets**](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachinescalesets) [Kaynak türü bölümünde](../resource-group-authoring-templates.md)yapılabilir. Yükleme için sertifikayı osProfile'a ekleyin. Şablonun uzantı bölümü ACL'deki sertifikayı güncelleştirebilir.

  ```json
  {
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    ....
      "osProfile": {
          "adminPassword": "[parameters('adminPassword')]",
          "adminUsername": "[parameters('adminUsername')]",
          "computernamePrefix": "[parameters('vmNodeType0Name')]",
          "secrets": [
            {
              "sourceVault": {
                "id": "[parameters('sfReverseProxySourceVaultValue')]"
              },
              "vaultCertificates": [
                {
                  "certificateStore": "[parameters('sfReverseProxyCertificateStoreValue')]",
                  "certificateUrl": "[parameters('sfReverseProxyCertificateUrlValue')]"
                }
              ]
            }
          ]
        }
   ....
   "extensions": [
          {
              "name": "[concat(parameters('vmNodeType0Name'),'_ServiceFabricNode')]",
              "properties": {
                      "type": "ServiceFabricNode",
                      "autoUpgradeMinorVersion": false,
                      ...
                      "publisher": "Microsoft.Azure.ServiceFabric",
                      "settings": {
                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                        "dataPath": "D:\\\\SvcFab",
                        "durabilityLevel": "Bronze",
                        "testExtension": true,
                        "reverseProxyCertificate": {
                          "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                          "x509StoreName": "[parameters('sfReverseProxyCertificateStoreValue')]"
                        },
                  },
                  "typeHandlerVersion": "1.0"
              }
          },
      ]
    }
  ```
> [!NOTE]
> Varolan bir kümede ters proxy'yi etkinleştirmek için küme sertifikasından farklı sertifikalar kullandığınızda, ters proxy sertifikasını yükleyin ve ters proxy'yi etkinleştirmeden önce kümedeki ACL'yi güncelleştirin. 1-3 adımlarında ters proxy'yi etkinleştirmek için dağıtımı başlatmadan önce daha önce bahsedilen ayarları kullanarak [Azure Kaynak Yöneticisi şablonu](service-fabric-cluster-creation-via-arm.md) dağıtımını tamamlayın.

## <a name="enable-reverse-proxy-on-standalone-clusters"></a>Bağımsız kümelerde ters proxy'yi etkinleştirme

Bağımsız kümeler için ClusterConfig.json dosyasında ters proxy'yi etkinleştirin. Küme oluşturmada veya varolan bir küme için yapılandırmayı yükselterek ters proxy'yi etkinleştirebilirsiniz. ClusterConfig.json dosyalarında bulunan ayarlar hakkında daha fazla bilgi edinmek için [Bağımsız küme ayarlarına](./service-fabric-cluster-manifest.md)bakın.

Aşağıdaki adımlar, ters proxy'yi etkinleştirmek ve isteğe bağlı olarak x.509 sertifikasıyla ters proxy'yi güvence altına almak için kullanılacak ayarları gösterir. 

1. Ters proxy'yi etkinleştirmek için, küme config'indeki **özelliklerin** altındaki düğüm türü için **ters ProxyEndpointPort** değerini ayarlayın. Aşağıdaki JSON, "NodeType0" türüne sahip düğümler için ters proxy uç noktası bağlantı noktasını 19081 olarak ayarı gösterir:

   ```json
       "properties": {
          ... 
           "nodeTypes": [
               {
                   "name": "NodeType0",
                   ...
                   "reverseProxyEndpointPort": "19081",
                   ...
               }
           ],
          ...
       }
   ```
2. (İsteğe bağlı) Güvenli bir ters proxy için, **özellikler**altında **güvenlik** bölümünde bir sertifika yapılandırmak. 
   - Geliştirme veya test ortamı için **Ters Proxy Sertifikası** ayarını kullanabilirsiniz:

      ```json
          "properties": {
              ...
              "security": {
                  ...
                  "CertificateInformation": {
                      ...
                      "ReverseProxyCertificate": {
                          "Thumbprint": "[Thumbprint]",
                          "ThumbprintSecondary": "[Thumbprint]",
                          "X509StoreName": "My"
                      },
                      ...
                  }
              },
              ...
          }
      ```
   - Bir üretim ortamı için **Ters ProxySertifikasıOrtak Adlar** ayarı önerilir:

      ```json
          "properties": {
              ...
              "security": {
                  ...
                  "CertificateInformation": {
                      ...
                      "ReverseProxyCertificateCommonNames": {
                        "CommonNames": [
                            {
                              "CertificateCommonName": "[CertificateCommonName]"
                            }
                          ],
                          "X509StoreName": "My"
                      },
                      ...
                  }
              },
              ...
          }
      ```

   Bağımsız bir küme için sertifikaları yapılandırma ve yönetme hakkında daha fazla bilgi ve ters proxy sağlamak için kullanılan sertifikaları yapılandırma hakkında daha fazla ayrıntı için [Bkz. X509 sertifika tabanlı güvenlik.](./service-fabric-windows-cluster-x509-security.md)

ClusterConfig.json dosyanızı ters proxy'yi etkinleştirmek için değiştirdikten sonra, kümenizdeki değişiklikleri zorlamak için [küme yapılandırmasını yükselt'teki](service-fabric-cluster-config-upgrade-windows-server.md) yönergeleri izleyin.


## <a name="expose-reverse-proxy-on-a-public-port-through-azure-load-balancer"></a>Azure Yük Bakiyesi aracılığıyla ortak bir bağlantı noktasında ters proxy ortaya çıkarma

Bir Azure kümesinin dışından ters proxy'yi gidermek için, ters proxy bağlantı noktası için Azure Yük Dengeleyici kuralları ve Azure Sistem Durumu Sondası ayarlayın. Bu adımlar, kümeyi oluşturduktan sonra istediğiniz zaman Azure portalı veya Kaynak Yöneticisi şablonu kullanılarak gerçekleştirilebilir. 

> [!WARNING]
> Ters proxy'nin Yük Dengeleyicisi'ndeki bağlantı noktasını yapılandırırken, kümedeki bir HTTP bitiş noktasını ortaya çıkaran tüm mikro hizmetler kümenin dışından adreslenebilir. Bu, dahili olması amaçedilen mikro hizmetlerin kararlı bir kötü niyetli kullanıcı tarafından keşfedilebilir olduğu anlamına gelir. Bu, yararlanılabilen ciddi güvenlik açıkları sunar; örneğin:
>
> * Kötü niyetli bir kullanıcı, yeterince sertleştirilmiş bir saldırı yüzeyine sahip olmayan bir dahili hizmeti defalarca arayarak hizmet reddi saldırısı başlatabilir.
> * Kötü amaçlı bir kullanıcı, istenmeyen davranışlarla sonuçlanan bozuk paketleri dahili bir hizmete sunabilir.
> * Dahili olması gereken bir hizmet, küme dışındaki hizmetlere maruz kalması amaçlanmayan özel veya hassas bilgileri döndürerek bu hassas bilgileri kötü amaçlı bir kullanıcıya sunabilir. 
>
> Ters proxy bağlantı noktasını herkese açık hale getirmeden önce kümenizin ve üzerinde çalışan uygulamaların olası güvenlik sonuçlarını tam olarak anladığınızdan ve azaltladığınızdan emin olun. 
>

Bağımsız bir küme için ters proxy'yi genel olarak ortaya çıkarmak istiyorsanız, bunu yapma biçiminiz kümeyi barındıran sisteme bağlıdır ve bu makalenin kapsamı dışındadır. Ancak ters proxy'yi genel olarak ifşa etme konusunda önceki uyarı yine de geçerlidir.

### <a name="expose-the-reverse-proxy-using-azure-portal"></a>Azure portalını kullanarak ters proxy'yi ortaya çıkarma 

1. Azure portalında, kümenizin kaynak grubunu tıklatın ve ardından kümenizin yük bakiyesini tıklatın.
2. Ters proxy bağlantı noktası için bir sistem durumu Sondası eklemek için, yük dengeleyici penceresinin sol bölmesinde, **AYARLAR**altında , **Sağlık sondalarını**tıklatın. Ardından Sistem Durumu sondaları penceresinin üst kısmında **Ekle'yi** tıklatın ve ters proxy bağlantı noktası için ayrıntıları girin, ardından **Tamam'ı**tıklatın. Varsayılan olarak, kümeyi oluşturduğunuzda değiştirmediğiniz sürece ters proxy bağlantı noktası 19081'dir.

   ![Ters proxy sistem sondası yapılandırma](./media/service-fabric-reverseproxy-setup/lb-rp-probe.png)
3. Ters proxy bağlantı noktasını ortaya çıkarmak için yük dengeleyici kuralı eklemek için, yük dengeleyici penceresinin sol bölmesinde, **AYARLAR**altında, **Yük dengeleme kurallarını**tıklatın. Ardından Yük dengeleme kuralları penceresinin üst kısmında **Ekle'yi** tıklatın ve ters proxy bağlantı noktası için ayrıntıları girin. **Ters** proxy'nin açıkta kalmasını istediğiniz bağlantı noktası değerini, ters proxy'yi etkinleştirdiğinizde ayarladığınız bağlantı **noktasına Arka uç bağlantı noktası** değerini ve önceki adımda yapılandırdığınız sistem durumu **sondası** değerini ayarladığınızdan emin olun. Diğer alanları uygun şekilde ayarlayın ve **Tamam'ı**tıklatın.

   ![Ters proxy için yük dengeleyici kuralını yapılandırma](./media/service-fabric-reverseproxy-setup/lb-rp-rule.png)

### <a name="expose-the-reverse-proxy-via-resource-manager-templates"></a>Kaynak Yöneticisi şablonları aracılığıyla ters proxy'yi ortaya çıkarma

Aşağıdaki JSON, Azure Kaynak Yöneticisi [şablonları aracılığıyla ters proxy'yi etkinleştir'de](#enable-reverse-proxy-via-azure-resource-manager-templates)kullanılan şablona başvurur. Kaynak Yöneticisi şablonu oluşturma veya varolan bir küme için şablon verme hakkında bilgi için belgenin bu bölümüne bakın.  Değişiklikler [**Microsoft.Network/loadBalancers**](https://docs.microsoft.com/azure/templates/microsoft.network/loadbalancers) [Kaynak türü bölümünde](../resource-group-authoring-templates.md)yapılır.

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```


## <a name="customize-reverse-proxy-behavior-using-fabric-settings"></a>Kumaş ayarlarını kullanarak ters proxy davranışını özelleştirme

Azure'da barındırılan kümeler için Kaynak Yöneticisi şablonundaki kumaş ayarları aracılığıyla veya bağımsız kümeler için ClusterConfig.json dosyasında ters proxy davranışını özelleştirebilirsiniz. Ters proxy davranışını denetleyen ayarlar, cluster **özellikleri** bölümünün altındaki **fabricAyarlar** bölümündeki [**ApplicationGateway/Http**](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) bölümünde yer alır. 

Örneğin, **Varsayılan HttpRequestTimeout** değerini, aşağıdaki JSON'da olduğu gibi ters proxy'deki istekler için zaman aşmasını 180 saniyeolarak ayarlayabilirsiniz:

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                 {
                   "name": "DefaultHttpRequestTimeout",
                   "value": "180"
                 }
               ]
             }
           ],
           ...
   }
   ``` 

Azure kümeleri için kumaş ayarlarını güncelleştirme hakkında daha fazla bilgi için [Kaynak Yöneticisi şablonlarını kullanarak küme ayarlarını özelleştir'e](service-fabric-cluster-config-upgrade-azure.md)bakın. Bağımsız kümeler [için, bağımsız kümeler için küme ayarlarını özelleştir'e](service-fabric-cluster-config-upgrade-windows-server.md)bakın. 

Ters proxy ve hizmetler arasında güvenli iletişim kurulmasına yardımcı olmak için çeşitli kumaş ayarları kullanılır. Bu ayarlar hakkında ayrıntılı bilgi için ters [proxy ile güvenli bir hizmete bağlan'a](service-fabric-reverseproxy-configure-secure-communication.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
* [TERS proxy ile HTTP hizmetini güvenli hale getirmek için yönlendirmeyi ayarlama](service-fabric-reverseproxy-configure-secure-communication.md)
* Ters proxy yapılandırma seçenekleri için, [Service Fabric küme ayarlarını Özelleştir'deki ApplicationGateway/Http bölümüne](service-fabric-cluster-fabric-settings.md#applicationgatewayhttp)bakın.
