---
title: Azure Hizmet Kumaşınızı tek başına yapılandırın
description: Bağımsız veya şirket içi Azure Hizmet Kumaşı kümenizi nasıl yapılandırılayarıştırmayı öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: dekapur
ms.openlocfilehash: 0f9b625dfbe9c39bea7771dcc5fd58805ce19811
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75458366"
---
# <a name="configuration-settings-for-a-standalone-windows-cluster"></a>Bağımsız bir Windows kümesi için yapılandırma ayarları
Bu makalede, *ClusterConfig.json* dosyasında ayarlanabilen bağımsız bir Azure Hizmet Dokusu kümesinin yapılandırma ayarları açıklanmaktadır. Bu dosyayı kümenin düğümleri, güvenlik yapılandırmaları ve ağ topolojisi hakkında hata ve yükseltme etki alanları açısından bilgi belirtmek için kullanırsınız.  Yapılandırma ayarlarını değiştirdikten veya ekledikten [sonra, bağımsız bir küme oluşturabilir](service-fabric-cluster-creation-for-windows-server.md) veya bağımsız bir [kümenin yapılandırmasını yükseltebilirsiniz.](service-fabric-cluster-config-upgrade-windows-server.md)

Bağımsız [Service Fabric paketini indirdiğinizde](service-fabric-cluster-creation-for-windows-server.md#downloadpackage)ClusterConfig.json örnekleri de dahildir. Adlarında "DevCluster" bulunan örnekler, mantıksal düğümler kullanarak aynı makinede üç düğümü niçeren bir küme oluşturur. Bu düğümlerden en az birinin birincil düğüm olarak işaretlemesi gerekir. Bu tür küme geliştirme veya test ortamları için yararlıdır. Üretim kümesi olarak desteklenmez. Adlarında "MultiMachine" olan örnekler, her düğüm ayrı bir makinede bulunan üretim sınıfı kümeleri oluşturmaya yardımcı olur. Bu kümeler için birincil düğüm sayısı kümenin güvenilirlik [düzeyini](#reliability)temel adatır. API Sürüm 05-2017 sürümünde güvenilirlik düzeyindeki özelliği kaldırdık. Bunun yerine, kodumuz kümeniz için en iyi duruma getirilmiş güvenilirlik düzeyini hesaplar. 5.7 sürümlerinde bu özellik için bir değer ayarlamaya çalışmayın.

* ClusterConfig.Unsecure.DevCluster.json ve ClusterConfig.Unsecure.MultiMachine.json sırasıyla güvenli olmayan bir test veya üretim kümesi oluşturmak için nasıl gösterin.

* ClusterConfig.Windows.DevCluster.json ve ClusterConfig.Windows.MultiMachine.json, [Windows security](service-fabric-windows-cluster-windows-security.md)kullanılarak güvenli test veya üretim kümelerinin nasıl oluşturulacaklarını gösterir.

* ClusterConfig.X509.DevCluster.json ve ClusterConfig.X509.MultiMachine.json, [X509 sertifika tabanlı güvenlik](service-fabric-windows-cluster-x509-security.md)kullanılarak güvenli test veya üretim kümelerinin nasıl oluşturulacak larını gösterir.

Şimdi ClusterConfig.json dosyasının çeşitli bölümlerini inceleyelim.

## <a name="general-cluster-configurations"></a>Genel küme yapılandırmaları
Genel küme yapılandırmaları, aşağıdaki JSON parçacığında gösterildiği gibi geniş kümeye özgü yapılandırmaları kapsar:

```json
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "01-2017",
```

Hizmet Kumaşı kümenize herhangi bir dost adı, ad değişkenine atayarak verebilirsiniz. ClusterConfigurationVersion kümenizin sürüm numarasıdır. Service Fabric kümenizi her yükselttiğınızda artırın. apiVersion kümesini varsayılan değere bırakın.

## <a name="nodes-on-the-cluster"></a>Kümedeki düğümler
Aşağıdaki parçacıkta görüldüğü gibi, Hizmet Kumaşı kümenizdeki düğümleri düğümler bölümünü kullanarak yapılandırabilirsiniz:
```json
"nodes": [{
    "nodeName": "vm0",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD0"
}, {
    "nodeName": "vm1",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType1",
    "faultDomain": "fd:/dc1/r1",
    "upgradeDomain": "UD1"
}, {
    "nodeName": "vm2",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType2",
    "faultDomain": "fd:/dc1/r2",
    "upgradeDomain": "UD2"
}],
```

Hizmet Kumaşı kümesi en az üç düğüm içermelidir. Kurulumunuza göre bu bölüme daha fazla düğüm ekleyebilirsiniz. Aşağıdaki tabloda her düğüm için yapılandırma ayarları açıklanmaktadır:

| **Düğüm yapılandırması** | **Açıklama** |
| --- | --- |
| düğümName |Düğüme herhangi bir dost adı verebilirsiniz. |
| ıpaddress |Komut penceresini açıp yazarak `ipconfig`düğümünizin IP adresini bulun. IPV4 adresini not alın ve iPAddress değişkenine atayın. |
| düğümTypeRef |Her düğüme farklı bir düğüm türü atanabilir. [Düğüm türleri](#node-types) aşağıdaki bölümde tanımlanır. |
| fayDomain |Hata etki alanları küme yöneticilerinin paylaşılan fiziksel bağımlılıklar nedeniyle aynı anda başarısız olabilecek fiziksel düğümleri tanımlamasını sağlar. |
| yükseltmeEtki Alanı |Yükseltme etki alanları, Service Fabric yükseltmeleri için yaklaşık aynı anda kapatılan düğüm kümelerini açıklar. Fiziksel gereksinimlerle sınırlı olmadığından, hangi yükseltme etki alanlarını atayabileceğinizi seçebilirsiniz. |

## <a name="cluster-properties"></a>Küme özellikleri
ClusterConfig.json'daki özellikler bölümü, kümeyi gösterildiği gibi yapılandırmak için kullanılır:

### <a name="reliability"></a>Güvenilirlik
Güvenilirlik kavramıDüzey, kümenin birincil düğümlerinde çalıştırılabilen Service Fabric sistem hizmetlerinin yineleme veya örnek sayısını tanımlar. Bu hizmetlerin ve dolayısıyla kümenin güvenilirliğini belirler. Değer küme oluşturma ve yükseltme zamanında sistem tarafından hesaplanır.

### <a name="diagnostics"></a>Tanılama
DiagnosticsStore bölümünde, aşağıdaki parçacıkta gösterildiği gibi, tanılama ve sorun giderme düğümü veya küme hatalarını etkinleştirecek parametreleri yapılandırabilirsiniz: 

```json
"diagnosticsStore": {
    "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
    "dataDeletionAgeInDays": "7",
    "storeType": "FileShare",
    "IsEncrypted": "false",
    "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
}
```

Meta veriler küme tanılamanızın bir açıklamasıdır ve kurulumunuza göre ayarlanabilir. Bu değişkenler, Performans sayaçlarının yanı sıra ETW izleme günlükleri ve kilitlenme dökümlerinin yanı sıra toplamaya yardımcı olur. ETW izleme günlükleri hakkında daha fazla bilgi için [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) ve [ETW izleme](https://msdn.microsoft.com/library/ms751538.aspx)bakın. [Kilitlenme dökümleri](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) ve performans [sayaçları](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx)da dahil olmak üzere tüm günlükler, makinenizdeki connectionString klasörüne yönlendirilebilir. Tanılamayı depolamak için AzureStorage'ı da kullanabilirsiniz. Aşağıdaki örnek parçacığına bakın:

```json
"diagnosticsStore": {
    "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
    "dataDeletionAgeInDays": "7",
    "storeType": "AzureStorage",
    "IsEncrypted": "false",
    "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
}
```

### <a name="security"></a>Güvenlik
Güvenlik bölümü, güvenli bağımsız hizmet kumaş ı kümesi için gereklidir. Aşağıdaki parçacık bu bölümün bir bölümünü gösterir:

```json
"security": {
    "metadata": "This cluster is secured using X509 certificates.",
    "ClusterCredentialType": "X509",
    "ServerCredentialType": "X509",
    . . .
}
```

Meta veriler güvenli kümenizin bir açıklamasıdır ve kurulumunuza göre ayarlanabilir. ClusterCredentialType ve ServerCredentialType küme ve düğümlerin uyguladığı güvenlik türünü belirler. Sertifika tabanlı güvenlik için *X509* veya Etkin Dizin tabanlı güvenlik için *Windows* olarak ayarlanabilirler. Güvenlik bölümünün geri kalanı güvenlik türüne bağlıdır. Güvenlik bölümünün geri kalanının nasıl doldurulur hakkında bilgi için, [bağımsız kümedeki Sertifikatabanlı güvenlik](service-fabric-windows-cluster-x509-security.md) veya [bağımsız kümedeki Windows güvenliği](service-fabric-windows-cluster-windows-security.md)bölümüne bakın.

### <a name="node-types"></a>Düğüm türleri
DüğümTürleri bölümünde kümenizin sahip olduğu düğüm türü açıklanır. Bir küme için aşağıdaki parçacıkta gösterildiği gibi en az bir düğüm türü belirtilmelidir: 

```json
"nodeTypes": [{
    "name": "NodeType0",
    "clientConnectionEndpointPort": "19000",
    "clusterConnectionEndpointPort": "19001",
    "leaseDriverEndpointPort": "19002"
    "serviceConnectionEndpointPort": "19003",
    "httpGatewayEndpointPort": "19080",
    "reverseProxyEndpointPort": "19081",
    "applicationPorts": {
        "startPort": "20575",
        "endPort": "20605"
    },
    "ephemeralPorts": {
        "startPort": "20606",
        "endPort": "20861"
    },
    "isPrimary": true
}]
```

Ad, bu özel düğüm türü için uygun addır. Bu düğüm türünün bir düğüm oluşturmak için, [daha önce belirtildiği](#nodes-on-the-cluster)gibi, bu düğüm için düğümTypeRef değişkenine dostu adını atayın. Her düğüm türü için kullanılan bağlantı uç noktalarını tanımlayın. Bu kümedeki diğer uç noktalarla çakışmadıkları sürece, bu bağlantı uç noktaları için herhangi bir bağlantı noktası numarası seçebilirsiniz. Bir çok düğüm kümesinde, [güvenirliğe](#reliability)bağlı olarak bir veya daha fazla birincil düğüm vardır (yani, isPrimary *doğru*olarak ayarlanır ), Düzey . Birincil ve birincil olmayan düğüm türleri hakkında daha fazla bilgi edinmek için, düğüm Türleri ve güvenilirlik Düzeyi hakkında bilgi için [Service Fabric küme kapasite planlama hususlarına](service-fabric-cluster-capacity.md) bakın. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Düğüm türlerini yapılandırmak için kullanılan uç noktalar
* clientConnectionEndpointPort, istemci API'leri kullanıldığında kümeye bağlanmak için istemci tarafından kullanılan bağlantı noktasıdır. 
* clusterConnectionEndpointPort düğümlerin birbiriyle iletişim kurduğu bağlantı noktasıdır.
* leaseDriverEndpointPort düğümleri hala etkin olup olmadığını öğrenmek için küme kiralama sürücüsü tarafından kullanılan bağlantı noktasıdır. 
* serviceConnectionEndpointPort, o düğümdeki Servis Kumaşı istemcisiyle iletişim kurmak için bir düğüm üzerinde dağıtılan uygulamalar ve hizmetler tarafından kullanılan bağlantı noktasıdır.
* httpGatewayEndpointPort, Service Fabric Explorer tarafından kümeye bağlanmak için kullanılan bağlantı noktasıdır.
* geçici Ports [işletim sistemi tarafından kullanılan dinamik bağlantı noktalarını](https://support.microsoft.com/kb/929851)geçersiz kılar. Service Fabric bu bağlantı noktalarının bir kısmını uygulama bağlantı noktası olarak kullanır ve geri kalanı işletim sistemi için kullanılabilir. Ayrıca, bu aralığı işletim sistemi içinde mevcut olan aralıkla eşler, böylece tüm amaçlar için örnek JSON dosyalarında verilen aralıkları kullanabilirsiniz. Başlangıç ve bitiş bağlantı noktaları arasındaki farkın en az 255 olduğundan emin olun. Bu aralık işletim sistemi ile paylaşıldığı için, bu fark çok düşükse çakışmalara rastlaabilirsiniz. Yapılandırılan dinamik bağlantı noktası aralığını görmek için çalıştırın. `netsh int ipv4 show dynamicport tcp`
* applicationPorts, Service Fabric uygulamaları tarafından kullanılan bağlantı noktalarıdır. Uygulama bağlantı noktası aralığı, uygulamalarınızın bitiş noktası gereksinimini karşılayacak kadar büyük olmalıdır. Bu aralık makinedeki dinamik bağlantı noktası aralığından, yani yapılandırmada ayarlanan geçici Ports aralığından münhasır olmalıdır. Service Fabric, yeni bağlantı noktaları gerektiğinde bu bağlantı noktalarını kullanır ve bu bağlantı noktaları için güvenlik duvarını açmanın icabına bakar. 
* reverseProxyEndpointPort isteğe bağlı ters proxy bitiş noktasıdır. Daha fazla bilgi için [Service Fabric ters proxy'ye](service-fabric-reverseproxy.md)bakın. 

### <a name="log-settings"></a>Günlük ayarları
kumaş Ayarlar bölümünde, Hizmet Kumaşı verilerinin ve günlüklerinin kök dizinlerini ayarlayabilirsiniz. Bu dizinleri yalnızca ilk küme oluşturma sırasında özelleştirebilirsiniz. Bu bölümün aşağıdaki örnek parçacığına bakın:

```json
"fabricSettings": [{
    "name": "Setup",
    "parameters": [{
        "name": "FabricDataRoot",
        "value": "C:\\ProgramData\\SF"
    }, {
        "name": "FabricLogRoot",
        "value": "C:\\ProgramData\\SF\\Log"
}]
```

FabricDataRoot ve FabricLogRoot olarak işletim sistemi olmayan bir sürücü kullanmanızı öneririz. İşletim sistemi yanıt vermeyi bıraktığında durumlardan kaçınmada daha fazla güvenilirlik sağlar. Yalnızca veri kökünü özelleştirseniz, günlük kökü veri kökünü bir düzey altına yerleştirilir.

### <a name="stateful-reliable-services-settings"></a>Stateful Reliable Services ayarları
KtlLogger bölümünde, Güvenilir Hizmetler için genel yapılandırma ayarlarını ayarlayabilirsiniz. Bu ayarlar hakkında daha fazla bilgi için, [Bkz.](service-fabric-reliable-services-configuration.md) Aşağıdaki örnek, devlet hizmetleri için güvenilir koleksiyonları destekleyecek şekilde oluşturulan paylaşılan işlem günlüğünün nasıl değiştirilebildiğini gösterir:

```json
"fabricSettings": [{
    "name": "KtlLogger",
    "parameters": [{
        "name": "SharedLogSizeInMB",
        "value": "4096"
    }]
}]
```

### <a name="add-on-features"></a>Eklenti özellikleri
Eklenti özelliklerini yapılandırmak için apiVersion'u 04-2017 veya daha yüksek olarak yapılandırın ve addonFeatures'i burada gösterildiği gibi yapılandırın:

```json
"apiVersion": "04-2017",
"properties": {
    "addOnFeatures": [
        "DnsService",
        "RepairManager"
    ]
}
```
Mevcut tüm eklenti özellikleri [Service Fabric REST API Referans](https://docs.microsoft.com/rest/api/servicefabric/sfrp-model-addonfeatures)görülebilir.

### <a name="container-support"></a>Kapsayıcı desteği
Bağımsız kümeler için hem Windows Server kapsayıcıları hem de Hyper-V kapsayıcıları için kapsayıcı desteğini etkinleştirmek için DnsService eklenti özelliğinin etkinleştirilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Bağımsız küme kurulumunuza göre yapılandırılan tam bir *ClusterConfig.json* dosyanız olduktan sonra kümenizi dağıtabilirsiniz. Bağımsız Hizmet [Kumaşı kümesi Oluştur'daki](service-fabric-cluster-creation-for-windows-server.md)adımları izleyin. 

Tek başına bir küme dağıtılmış sa, [bağımsız bir kümenin yapılandırmasını](service-fabric-cluster-config-upgrade-windows-server.md)da yükseltebilirsiniz. 

Service Fabric Explorer ile kümenizi nasıl [görselleştirebilirsiniz](service-fabric-visualizing-your-cluster.md)öğrenin.

