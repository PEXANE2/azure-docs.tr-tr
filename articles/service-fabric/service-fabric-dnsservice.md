---
title: Azure Servis Kumaş DNS hizmeti
description: Kümenin içinden mikro hizmetleri keşfetmek için Service Fabric'in dns hizmetini kullanın.
ms.topic: conceptual
ms.date: 7/20/2018
ms.openlocfilehash: 317aa81238ec7a0dc24b69b1d00568901b9bc34f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75458037"
---
# <a name="dns-service-in-azure-service-fabric"></a>Azure Service Fabric'te DNS Hizmeti
DNS Hizmeti, kümenizde DNS protokolünü kullanarak diğer hizmetleri keşfetmesini etkinleştirebileceğiniz isteğe bağlı bir sistem hizmetidir. 

Birçok hizmet, özellikle kapsayıcı hizmetler, önceden varolan bir URL üzerinden ele alınabiliyor. Hizmet Kumaş Adlandırma Hizmeti protokolü yerine standart DNS protokolünü kullanarak bu hizmetleri çözebilmek arzu edilir. DNS hizmeti, DNS adlarını bir hizmet adıyla eşlenebilmenizi ve dolayısıyla uç nokta IP adreslerini çözmenizi sağlar. Bu tür işlevler, kapsayıcılaştırılmış hizmetlerin farklı platformlarda taşınabilirliğini korur ve Adlandırma Hizmetinden yararlanmak için kodu yeniden yazmak yerine varolan hizmet URL'lerini kullanmanıza izin vererek "kaldırma ve kaydırma" senaryolarını kolaylaştırabilir. 

DNS hizmeti, DNS adlarını hizmet bitiş noktasını döndürmek için Adlandırma Hizmeti tarafından çözülen hizmet adlarıyla eşler. Hizmetin DNS adı oluşturuldurma sırasında sağlanır. Aşağıdaki diyagram, DNS hizmetinin devletsiz hizmetler için nasıl çalıştığını gösterir.

![hizmet bitiş noktaları](./media/service-fabric-dnsservice/stateless-dns.png)

Service Fabric sürüm 6.3 ile başlayarak, Service Fabric DNS protokolü, bölümlenmiş durum hizmetlerini ele almak için bir düzen içerecek şekilde genişletildi. Bu uzantılar, durum lu hizmet DNS adı ve bölüm adının birleşimini kullanarak belirli bölüm IP adreslerini çözümlenebilmektir. Her üç bölümleme düzenleri desteklenir:

- Adlandırılmış bölümleme
- Aralıklı bölümleme
- Singleton bölümleme

Aşağıdaki diyagram, DNS hizmetinin bölümlenmiş durum lu hizmetler için nasıl çalıştığını gösterir.

![devlet hizmeti bitiş noktaları](./media/service-fabric-dnsservice/stateful-dns.png)

Dinamik bağlantı noktaları DNS hizmeti tarafından desteklenmez. Dinamik bağlantı noktalarında açığa çıkan hizmetleri çözmek için [ters proxy hizmetini](./service-fabric-reverseproxy.md)kullanın.

## <a name="enabling-the-dns-service"></a>DNS hizmetini etkinleştirme
> [!NOTE]
> Service Fabric hizmetleri için DNS hizmeti henüz Linux'ta desteklenmedi.

Portalı kullanarak bir küme oluşturduğunuzda, DNS hizmeti varsayılan olarak **Cluster yapılandırma** menüsündeki **DNS hizmet** ekle onay kutusunda etkinleştirilir:

![Portal üzerinden DNS hizmetini etkinleştirme](./media/service-fabric-dnsservice/enable-dns-service.png)

Portalı kümenizi oluşturmak için kullanmıyorsanız veya varolan bir kümeyi güncelliyorsanız, DNS hizmetini bir şablonda etkinleştirmeniz gerekir:

- Yeni bir küme dağıtmak için [örnek şablonları](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) kullanabilir veya kendi Kaynak Yöneticisi şablonunuzu oluşturabilirsiniz. 
- Varolan bir kümeyi güncelleştirmek için, portaldaki kümenin kaynak grubuna gidebilir ve kümenin geçerli durumunu ve gruptaki diğer kaynakları yansıtan bir şablonla çalışmak için **Otomasyon Komut Dosyası'nı** tıklatabilirsiniz. Daha fazla bilgi için kaynak [grubundan şablonu dışa aktar'a](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template)bakın.

Bir şablona sahip olduktan sonra, DNS hizmetini aşağıdaki adımlarla etkinleştirebilirsiniz:

1. Kaynak için `apiversion` veya `2017-07-01-preview` daha sonra ayarlanıp ayarlanmadığını denetleyin ve değilse, aşağıdaki örnekte gösterildiği gibi güncelleştirin: `Microsoft.ServiceFabric/clusters`

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Şimdi DNS hizmetini aşağıdaki yollardan biriyle etkinleştirin:

   - DNS hizmetini varsayılan ayarlarla etkinleştirmek için, aşağıdaki `properties` örnekte gösterildiği gibi bölümün içindeki `addonFeatures` bölüme ekleyin:

        ```json
          "properties": {
            ...
            "addonFeatures": [
              "DnsService"
              ],
            ...
          }
        ```

   - Varsayılan ayarlar dışında hizmet etkinleştirmek `DnsService` `fabricSettings` `properties` için, bölümün içindeki bölüme bir bölüm ekleyin. Bu durumda, DnsService eklemek `addonFeatures`gerekmez. DNS Hizmeti için ayarlanabilen özellikler hakkında daha fazla bilgi edinmek için [DNS Hizmeti ayarlarına](./service-fabric-cluster-fabric-settings.md#dnsservice)bakın.

       ```json
           "properties": {
             ...  
             "fabricSettings": [
               ...
               {
                 "name": "DnsService",
                 "parameters": [
                   {
                     "name": "IsEnabled",
                     "value": "true"
                   },
                   {
                     "name": "PartitionSuffix",
                     "value": "--"
                   },
                   {
                     "name": "PartitionPrefix",
                     "value": "--"
                   }
                 ]
               },
               ...
              ]
            }
       ```
3. Küme şablonunu değişikliklerinizle güncelleştirdikten sonra uygulayın ve yükseltmenin tamamlanmasına izin verin. Yükseltme tamamlandığında, DNS sistem hizmeti kümenizde çalışmaya başlar. Hizmet adı `fabric:/System/DnsService`, ve Hizmet Kumaş explorer **Sistem** hizmet bölümü altında bulabilirsiniz. 

> [!NOTE]
> DNS'yi devre dışı bırakılmış tan etkine yükseltirken, Service Fabric Explorer yeni durumu yansıtmayabilir. Çözmek için, Azure Kaynak Yöneticisi şablonunuzda Yükseltme Politikası'nı değiştirerek düğümleri yeniden başlatın. Daha fazla iş için [Hizmet Kumaşı ŞablonU Başvurusu'na](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2019-03-01/clusters/applications) bakın.

> [!NOTE]
> Yerel bir makinede geliştirirken DNS hizmetini etkinleştirmek bazı DNS ayarlarını geçersiz kılar. Internet'e bağlanma sorunları yla karşılaşırsanız, DNS ayarlarınızı kontrol edin.

## <a name="setting-the-dns-name-for-your-service"></a>Hizmetiniz için DNS adını ayarlama
Hizmetleriniz için ApplicationManifest.xml dosyasındaki varsayılan hizmetler için veya PowerShell komutları aracılığıyla bir DNS adı ayarlayabilirsiniz.

Hizmetinizin DNS adı küme boyunca çözülebilir, bu nedenle küme deki DNS adının benzersizliğini sağlamak önemlidir. 

Bir adlandırma şeması kullanmanız şiddetle tavsiye `<ServiceDnsName>.<AppInstanceName>`edilir; örneğin, `service1.application1`. Docker oluşturma kullanılarak bir uygulama dağıtılırsa, hizmetlere bu adlandırma düzenini kullanarak otomatik olarak DNS adları atanır.

### <a name="setting-the-dns-name-for-a-default-service-in-the-applicationmanifestxml"></a>ApplicationManifest.xml'de varsayılan bir hizmet için DNS adını ayarlama
Projenizi Visual Studio'da veya en sevdiğiniz düzenleyicide açın ve ApplicationManifest.xml dosyasını açın. Varsayılan hizmetler bölümüne gidin ve her `ServiceDnsName` hizmet için özniteliği ekleyin. Aşağıdaki örnek, hizmetin DNS adının nasıl`service1.application1`

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
      <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
```
Uygulama dağıtıldıktan sonra, Hizmet Kumaşı gezginindeki hizmet örneği, aşağıdaki şekilde gösterildiği gibi, bu örneğin DNS adını gösterir: 

![hizmet bitiş noktaları](./media/service-fabric-dnsservice/service-fabric-explorer-dns.png)

Aşağıdaki örnekte, devlet hizmeti için DNS `statefulsvc.app`adı . Hizmet, adlandırılmış bir bölümleme düzeni kullanır. Bölüm adlarının küçük harf olduğuna dikkat edin. Bu, DNS sorgularında hedeflenecek bölümler için bir gerekliliktir; daha fazla bilgi için, [bkz.](https://docs.microsoft.com/azure/service-fabric/service-fabric-dnsservice#preview-making-dns-queries-on-a-stateful-service-partition)

```xml
    <Service Name="Stateful1" ServiceDnsName="statefulsvc.app" />
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="2" MinReplicaSetSize="2">
        <NamedPartition>
         <Partition Name="partition1" />
         <Partition Name="partition2" />
        </NamedPartition>
      </StatefulService>
    </Service>
```

### <a name="setting-the-dns-name-for-a-service-using-powershell"></a>Powershell kullanarak bir hizmet için DNS adını ayarlama
`New-ServiceFabricService` Powershell komutunu kullanarak bir hizmet için DNS adını ayarlayabilirsiniz. Aşağıdaki örnek, DNS adı ile yeni bir stateless hizmet oluşturur`service1.application1`

```powershell
    New-ServiceFabricService `
    -Stateless `
    -PartitionSchemeSingleton `
    -ApplicationName `fabric:/application1 `
    -ServiceName fabric:/application1/service1 `
    -ServiceTypeName Service1Type `
    -InstanceCount 1 `
    -ServiceDnsName service1.application1
```

## <a name="preview-making-dns-queries-on-a-stateful-service-partition"></a>[Önizleme] Devlet hizmeti bölmede DNS sorguları yapma
Service Fabric sürüm 6.3'ten başlayarak, Service Fabric DNS hizmeti, hizmet bölümleri için sorguları destekler.

DNS sorgularında kullanılacak bölümler için aşağıdaki adlandırma kısıtlamaları uygulanır:

   - Bölüm adları DNS uyumlu olmalıdır.
   - Çok etiketli bölüm adları (addaki nokta, '.', dahil) kullanılmamalıdır.
   - Bölüm adları küçük harfli olmalıdır.

Bir bölümü hedefleyen DNS sorguları aşağıdaki gibi biçimlendirilir:

```
    <First-Label-Of-Partitioned-Service-DNSName><PartitionPrefix><Target-Partition-Name>< PartitionSuffix>.<Remaining- Partitioned-Service-DNSName>
```
Konumlar:

- *İlk-Etiket-Of-Bölümlenmiş-Hizmet-DNSName* hizmet DNS adının ilk parçasıdır.
- *PartitionPrefix* küme bildiriminin DnsService bölümünde veya kümenin Kaynak Yöneticisi şablonu aracılığıyla ayarlanabilen bir değerdir. Varsayılan değer "--"' olur. Daha fazla bilgi için [DNS Hizmeti ayarlarına](./service-fabric-cluster-fabric-settings.md#dnsservice)bakın.
- *Hedef-Bölüm-Adı,* bölümün adıdır. 
- *PartitionSuffix* küme bildiriminin DnsService bölümünde veya kümenin Kaynak Yöneticisi şablonu aracılığıyla ayarlanabilen bir değerdir. Varsayılan değer boş dizedir. Daha fazla bilgi için [DNS Hizmeti ayarlarına](./service-fabric-cluster-fabric-settings.md#dnsservice)bakın.
- *Kalan-Bölümlenmiş-Hizmet-DNSName* hizmet DNS adınızın kalan parçasıdır.

Aşağıdaki örnekler, varsayılan ayarları olan bir küme üzerinde çalışan bölümlenmiş `PartitionPrefix` hizmetler `PartitionSuffix`için DNS sorgularını gösterir ve: 

- Aralıklı bölümleme düzeni kullanan DNS `backendrangedschemesvc.application` adında bir hizmetin "0" `backendrangedschemesvc-0.application`bölümüne çözüm vermek için.
- Adlandırılmış bölümleme düzeni kullanan DNS `backendnamedschemesvc.application` adındabir hizmetin "ilk" `backendnamedschemesvc-first.application`bölümüne çözüm bulmak için.

DNS hizmeti, bölümün birincil yinelemesinin IP adresini döndürür. Bölüm belirtilmemişse, hizmet rasgele seçilmiş bir bölümün birincil yinelemesinin IP adresini döndürür.

## <a name="using-dns-in-your-services"></a>Hizmetlerinizde DNS kullanma
Birden fazla hizmet dağıtabiliyorsanız, bir DNS adı kullanarak iletişim kurmak için diğer hizmetlerin uç noktalarını bulabilirsiniz. DNS hizmeti, devletsiz hizmetler için ve Service Fabric sürüm 6.3 ve sonraki sürümlerde, devlet hizmetleri için çalışır. 6.3'ten önce Service Fabric sürümlerinde çalışan devlethizmetleri için, belirli bir hizmet bölümü çağırmak için http çağrıları için yerleşik [ters proxy hizmetini](./service-fabric-reverseproxy.md) kullanabilirsiniz. 

Dinamik bağlantı noktaları DNS hizmeti tarafından desteklenmez. Dinamik bağlantı noktaları kullanan hizmetleri çözmek için ters proxy hizmetini kullanabilirsiniz.

Aşağıdaki kod, DNS üzerinden devletsiz bir hizmetin nasıl çağrıldığını gösterir. Bu sadece DNS adını, bağlantı noktasını ve URL'nin bir parçası olarak herhangi bir isteğe bağlı yol sağladığınız düzenli bir http aramadır.

```csharp
public class ValuesController : Controller
{
    // GET api
    [HttpGet]
    public async Task<string> Get()
    {
        string result = "";
        try
        {
            Uri uri = new Uri("http://service1.application1:8080/api/values");
            HttpClient client = new HttpClient();
            var response = await client.GetAsync(uri);
            result = await response.Content.ReadAsStringAsync();
            
        }
        catch (Exception e)
        {
            Console.Write(e.Message);
        }

        return result;
    }
}
```

Aşağıdaki kod, devlet hizmetinin belirli bir bölümüne yapılan çağrıyı gösterir. Bu durumda, DNS adı bölüm adı (partition1) içerir. Çağrı için `PartitionPrefix` varsayılan değerleri olan bir `PartitionSuffix`küme varsayar ve .

```csharp
public class ValuesController : Controller
{
    // GET api
    [HttpGet]
    public async Task<string> Get()
    {
        string result = "";
        try
        {
            Uri uri = new Uri("http://service2-partition1.application1:8080/api/values");
            HttpClient client = new HttpClient();
            var response = await client.GetAsync(uri);
            result = await response.Content.ReadAsStringAsync();
            
        }
        catch (Exception e)
        {
            Console.Write(e.Message);
        }

        return result;
    }
}
```

## <a name="known-issues"></a>Bilinen Sorunlar
* Service Fabric sürümleri 6.3 ve üzeri için, DNS adında tire içeren hizmet adları için DNS aramalarında bir sorun vardır. Bu konuda daha fazla bilgi için lütfen aşağıdaki [GitHub Sorununu](https://github.com/Azure/service-fabric-issues/issues/1197)izleyin. Bunun için bir düzeltme sonraki 6.3 güncelleme geliyor. 

* Service Fabric hizmetleri için DNS hizmeti henüz Linux'ta desteklenmedi. DNS hizmeti Linux'taki konteynerler için desteklenir. Kumaş İstemci/ServicePartitionResolver kullanarak manuel çözünürlük kullanılabilir bir alternatiftir.

## <a name="next-steps"></a>Sonraki adımlar
[Bağlantı ve hizmetlerle iletişim](service-fabric-connect-and-communicate-with-services.md) kurma ile küme deki hizmet iletişimi hakkında daha fazla bilgi edinin

