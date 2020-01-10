---
title: NSG 'ler için akış günlüğüne giriş
titleSuffix: Azure Network Watcher
description: Bu makalede, Azure ağ Izleyicisi 'nin NSG akış günlükleri özelliğinin nasıl kullanılacağı açıklanmaktadır.
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: 47d91341-16f1-45ac-85a5-e5a640f5d59e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: kumud
ms.openlocfilehash: 509545443bc08e8613d5f7a9ba7f33d2a90684b8
ms.sourcegitcommit: 8b37091efe8c575467e56ece4d3f805ea2707a64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75830522"
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a>Ağ güvenlik grupları için akış günlüğüne giriş

Ağ güvenlik grubu (NSG) akış günlükleri NSG üzerinden giren ve çıkan IP trafiği hakkındaki bilgileri görüntülemenize olanak tanıyan bir Ağ İzleyicisi özelliğidir. Akış günlükleri JSON biçiminde yazılır ve kural tabanında giden ve gelen akışları, akışın uygulandığı ağ arabirimini (NIC), akış hakkındaki 5 tanımlama grubu (kaynak/hedef IP, kaynak/hedef bağlantı noktası ve protokol) bilgilerini ve trafiği izin verildiği veya trafiğin engellendiği bilgisini, ayrıca 2. Sürüm'de işleme hızı bilgilerini (Bayt ve Paket) gösterir.


![akış günlüklerine genel bakış](./media/network-watcher-nsg-flow-logging-overview/figure1.png)

Akış günlükleri NSG 'leri hedeflerse, diğer Günlükler ile aynı görüntülenmez. Akış günlükleri yalnızca bir depolama hesabı içinde depolanır ve aşağıdaki örnekte gösterilen günlük yolunu izler:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```
[Trafik analizlerini](traffic-analytics.md)kullanarak akış günlüklerini çözümleyebilir ve ağ trafiğiniz hakkında öngörüler elde edebilirsiniz.

Diğer Günlükler için görülen aynı bekletme ilkeleri, akış günlükleri için geçerlidir. Günlük tutma ilkesini, 1 günden 365 güne kadar ayarlayabilirsiniz. Bekletme ilkesi ayarlanmazsa günlükler süresiz olarak saklanır.

> [!NOTE] 
> NSG akış günlüğü ile bekletme ilkesi özelliğinin kullanılması, yüksek hacimde depolama işlemlerine ve ilişkili maliyetlere neden olabilir. Bekletme İlkesi özelliği gerektirmiyorsa, bu değeri 0 olarak ayarlamanızı öneririz.


## <a name="log-file"></a>Günlük dosyası

Akış günlükleri aşağıdaki özellikleri içerir:

* etkinliğin günlüğe kaydedildiği **saat**
* **SystemId** -ağ güvenlik grubu kaynak kimliği.
* **Kategori** -etkinliğin kategorisi. Kategori her zaman **Networksecuritygroupflowevent**
* **RESOURCEID** -NSG kaynak kimliği
* **OperationName** -Always NetworkSecurityGroupFlowEvents
* **Özellikler** -akışın özelliklerinin bir koleksiyonu
    * Akış günlüğü olay şemasının **Sürüm** numarası
    * **akışlar** -akış koleksiyonu. Bu özelliğin farklı kurallar için birden çok girişi vardır
        * akışların listelendiği **kural** kuralı
            * **akışlar** -akış koleksiyonu
                * **Mac** -AKıŞıN toplandığı VM için NIC 'in MAC adresi
                * **flowtuple** -akış kayıt düzeni için virgülle ayrılmış biçimde birden çok özellik içeren bir dize
                    * **Zaman damgası** -bu değer, akışın UNIX dönem biçiminde gerçekleştiği zaman damgasıdır
                    * **Kaynak IP** -kaynak IP 'si
                    * **Hedef IP** -hedef IP
                    * **Kaynak bağlantı noktası** -kaynak bağlantı noktası
                    * **Hedef bağlantı noktası** -hedef bağlantı noktası
                    * **Protokol** -akışın protokolü. Geçerli değerler TCP için **T** ve UDP için **U**
                    * **Trafik akışı** -trafik akışının yönü. Giden **ve çıkış için geçerli** değerler **ı** .
                    * **Trafik kararı** -trafiğe izin verilip verilmediğini belirtir. Geçerli **değerler, izin** verilen ve **D** için için geçerlidir.
                    * **Flow durumu-yalnızca sürüm 2** -akışın durumunu yakalar. Olası durumlar **B**: bir akış oluşturulduğunda başlar. İstatistikler sağlanmamış. **C**: devam eden bir akış için devam ediliyor. İstatistikler 5 dakikalık aralıklarla sağlanır. **E**: bir akış sonlandırıldığında sonlandırın. İstatistikler sağlanır.
                    * **Paketler-kaynak-yalnızca hedef sürüm 2** Son güncelleştirmeden bu yana kaynaktan hedefe gönderilen TCP veya UDP paketlerinin toplam sayısı.
                    * **Gönderilen bayt-kaynak-yalnızca hedef sürüm 2 ' ye** Son güncelleştirmeden bu yana kaynaktan hedefe gönderilen TCP veya UDP paket baytlarının toplam sayısıdır. Paket baytları paket üst bilgisini ve yükünü içerir.
                    * **Paketler-hedefe yalnızca kaynak-sürüm 2** Son güncelleştirmeden bu yana hedefin kaynağına Gönderilen TCP veya UDP paketlerinin toplam sayısı.
                    * **Gönderilen bayt-hedef-yalnızca kaynak-sürüm 2** Son güncelleştirmeden bu yana, hedefin kaynağına Gönderilen TCP ve UDP paket baytlarının toplam sayısıdır. Paket baytları paket üst bilgisini ve yükünü içerir.

## <a name="nsg-flow-logs-version-2"></a>NSG akış günlükleri sürüm 2

Günlüklerin 2. sürümü Flow durumunu tanıtır. Aldığınız akış günlüklerinin sürümünü yapılandırabilirsiniz. Akış günlüklerinin nasıl etkinleştirileceği hakkında bilgi edinmek için bkz. [NSG akış günlüğünü etkinleştirme](network-watcher-nsg-flow-logging-portal.md).

Akış durumu *B* , bir akış başlatıldığında kaydedilir. Flow durumu *C* ve Flow durumu *E* , sırasıyla akış ve akış sonlandırmasının devamlılığını işaretleyen durumlardır. Hem *C* hem de *E* durumları trafik bant genişliği bilgilerini içerir.

**Örnek**: 185.170.185.105:35370 ve 10.2.0.4:23 arasında bir TCP görüşmesinde akış tanımlama grupları:

"1493763938, 185.170.185.105, 10.2.0.4, 35370, 23, T, I, A, B,,,," "1493695838, 185.170.185.105, 10.2.0.4, 35370, 23, T, I, A, C, 1021, 588096, 8005, 4610880" "1493696138, 185.170.185.105, 10.2.0.4, 35370, 23, T, ı, A, E, 52, 29952, 47, 27072"

Devamlılık *C* ve bitiş *E* akışı durumları için bayt ve paket sayıları, önceki akış kayıt kümesi kaydı zamanından itibaren toplam sayılardır. Önceki örnek konuşmaya başvurarak, aktarılan toplam paket sayısı 1021 + 52 + 8005 + 47 = 9125 olur. Aktarılan toplam bayt sayısı 588096 + 29952 + 4610880 + 27072 = 5256000.

Aşağıdaki metin akış günlüğüne bir örnektir. Gördüğünüz gibi, önceki bölümde açıklanan özellik listesini izleyen birden çok kayıt vardır.

## <a name="nsg-flow-logging-considerations"></a>NSG akış günlüğü konuları

**Depolama hesabı konuları**: 

- Konum: kullanılan depolama hesabı NSG ile aynı bölgede olmalıdır.
- Kendi kendine Yönetim anahtar döndürme: erişim anahtarlarını depolama hesabınıza değiştirirseniz/döndürdüğünüzde NSG akış günlükleri çalışmayı durdurur. Bu sorunu onarmak için NSG akış günlüklerini devre dışı bırakıp yeniden etkinleştirmeniz gerekir.

**Bir kaynağa bağlı olan tüm NSG 'ler için NSG akış günlüğünü etkinleştirme**: NSG kaynağında Azure 'da akış günlüğü yapılandırılır. Akış yalnızca bir NSG kuralıyla ilişkilendirilecektir. Birden çok NSG 'nin kullanıldığı senaryolarda, tüm trafiğin kaydedildiğinden emin olmak için bir kaynağın alt ağını veya ağ arabirimini uygulayan NSG akış günlüğü 'nün etkinleştirilmiş olması önerilir. Daha fazla bilgi için trafiğin ağ güvenlik gruplarında [nasıl değerlendirildiğini](../virtual-network/security-overview.md#how-traffic-is-evaluated) öğrenin.

**Akış günlüğü maliyetleri**: NSG akış günlüğü, üretilen günlüklerin hacminde faturalandırılır. Yüksek trafik hacmi, büyük akış günlüğü hacmine ve ilişkili maliyetlere yol açabilir. NSG akış günlüğü fiyatlandırması, depolamanın temel maliyetlerini içermez. NSG akış günlüğü ile bekletme ilkesi özelliğinin kullanılması, yüksek hacimde depolama işlemlerine ve ilişkili maliyetlere neden olabilir. Bekletme İlkesi özelliği gerektirmiyorsa, bu değeri 0 olarak ayarlamanızı öneririz. Daha fazla bilgi için bkz. [ağ Izleyicisi fiyatlandırması](https://azure.microsoft.com/pricing/details/network-watcher/) ve [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) ek ayrıntılar için.

**Internet IP 'lerinden ortak IP Içermeyen VM 'lere kaydedilen gelen akışlar**: bir genel IP adresi, örnek DÜZEYI genel IP olarak NIC ile ilişkili bir genel IP adresi aracılığıyla atanmamış veya temel bir yük dengeleyici arka uç havuzunun parçası olan VM 'ler, [varsayılan SNAT](../load-balancer/load-balancer-outbound-connections.md#defaultsnat) 'yi kullanın ve giden bağlantıyı kolaylaştırmak için Azure tarafından atanmış bir IP adresine sahip olmalıdır. Sonuç olarak, akış, SNAT için atanan bağlantı noktası aralığındaki bir bağlantı noktasına gidiyor ise internet IP adreslerinden akışlar için akış günlüğü girişleri görebilirsiniz. Azure bu akışlara sanal makineye izin vermediğinden, deneme günlüğe kaydedilir ve tasarıma göre ağ Izleyicisi 'nin NSG akış günlüğünde görüntülenir. İstenmeyen gelen internet trafiğinin NSG ile açıkça engellenmesini öneririz.

**Durum bilgisi olmayan akışlar Için yanlış bayt ve paket sayısı**: [ağ güvenlik grupları (NSG 'Ler)](https://docs.microsoft.com/azure/virtual-network/security-overview) [durum bilgisi içeren bir güvenlik duvarı](https://en.wikipedia.org/wiki/Stateful_firewall?oldformat=true)olarak uygulanır. Ancak trafik akışını denetleyen birçok varsayılan/iç kural durum bilgisiz bir biçimde uygulanır. Platform sınırlamaları nedeniyle, bayt ve paketlerin sayısı durum bilgisiz akışlar için kaydedilmez (yani trafik akışı durum bilgisiz kurallarından geçer), yalnızca durum bilgisi olan akışlar için kaydedilir. Sonuç olarak, NSG akış günlüklerinde (ve Trafik Analizi) raporlanan bayt ve paketlerin sayısı gerçek akışlardan farklı olabilir. Bu sınırlama, Haziran 2020 ' den itibaren düzeltilmek üzere zamanlanır.

## <a name="sample-log-records"></a>Örnek günlük kayıtları

Aşağıdaki metin akış günlüğüne bir örnektir. Gördüğünüz gibi, önceki bölümde açıklanan özellik listesini izleyen birden çok kayıt vardır.


> [!NOTE]
> **Flowbaşlıkların* özelliğindeki değerler, virgülle ayrılmış bir liste.
 
### <a name="version-1-nsg-flow-log-format-sample"></a>Sürüm 1 NSG akış günlüğü biçim örneği
```json
{
    "records": [
        {
            "time": "2017-02-16T22:00:32.8950000Z",
            "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
            "category": "NetworkSecurityGroupFlowEvent",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
            "operationName": "NetworkSecurityGroupFlowEvents",
            "properties": {
                "Version": 1,
                "flows": [
                    {
                        "rule": "DefaultRule_DenyAllInBound",
                        "flows": [
                            {
                                "mac": "000D3AF8801A",
                                "flowTuples": [
                                    "1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"
                                ]
                            }
                        ]
                    },
                    {
                        "rule": "UserRule_default-allow-rdp",
                        "flows": [
                            {
                                "mac": "000D3AF8801A",
                                "flowTuples": [
                                    "1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A",
                                    "1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A",
                                    "1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A",
                                    "1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"
                                ]
                            }
                        ]
                    }
                ]
            }
        },
        {
            "time": "2017-02-16T22:01:32.8960000Z",
            "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
            "category": "NetworkSecurityGroupFlowEvent",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
            "operationName": "NetworkSecurityGroupFlowEvents",
            "properties": {
                "Version": 1,
                "flows": [
                    {
                        "rule": "DefaultRule_DenyAllInBound",
                        "flows": [
                            {
                                "mac": "000D3AF8801A",
                                "flowTuples": [
                                    "1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"
                                ]
                            }
                        ]
                    },
                    {
                        "rule": "UserRule_default-allow-rdp",
                        "flows": [
                            {
                                "mac": "000D3AF8801A",
                                "flowTuples": [
                                    "1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A",
                                    "1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A",
                                    "1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"
                                ]
                            }
                        ]
                    }
                ]
            }
        },
    "records":
    [
        
        {
             "time": "2017-02-16T22:00:32.8950000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A","1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A","1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A","1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:01:32.8960000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A","1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A","1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:02:32.9040000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282492,175.182.69.29,10.1.0.4,28918,5358,T,I,D","1487282505,71.6.216.55,10.1.0.4,8080,8080,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282512,91.224.160.154,10.1.0.4,59046,3389,T,I,A"]}]}]}
        }
        ,
        ...
```
### <a name="version-2-nsg-flow-log-format-sample"></a>Sürüm 2 NSG akış günlüğü biçim örneği
```json
 {
    "records": [
        {
            "time": "2018-11-13T12:00:35.3899262Z",
            "systemId": "a0fca5ce-022c-47b1-9735-89943b42f2fa",
            "category": "NetworkSecurityGroupFlowEvent",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
            "operationName": "NetworkSecurityGroupFlowEvents",
            "properties": {
                "Version": 2,
                "flows": [
                    {
                        "rule": "DefaultRule_DenyAllInBound",
                        "flows": [
                            {
                                "mac": "000D3AF87856",
                                "flowTuples": [
                                    "1542110402,94.102.49.190,10.5.16.4,28746,443,U,I,D,B,,,,",
                                    "1542110424,176.119.4.10,10.5.16.4,56509,59336,T,I,D,B,,,,",
                                    "1542110432,167.99.86.8,10.5.16.4,48495,8088,T,I,D,B,,,,"
                                ]
                            }
                        ]
                    },
                    {
                        "rule": "DefaultRule_AllowInternetOutBound",
                        "flows": [
                            {
                                "mac": "000D3AF87856",
                                "flowTuples": [
                                    "1542110377,10.5.16.4,13.67.143.118,59831,443,T,O,A,B,,,,",
                                    "1542110379,10.5.16.4,13.67.143.117,59932,443,T,O,A,E,1,66,1,66",
                                    "1542110379,10.5.16.4,13.67.143.115,44931,443,T,O,A,C,30,16978,24,14008",
                                    "1542110406,10.5.16.4,40.71.12.225,59929,443,T,O,A,E,15,8489,12,7054"
                                ]
                            }
                        ]
                    }
                ]
            }
        },
        {
            "time": "2018-11-13T12:01:35.3918317Z",
            "systemId": "a0fca5ce-022c-47b1-9735-89943b42f2fa",
            "category": "NetworkSecurityGroupFlowEvent",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
            "operationName": "NetworkSecurityGroupFlowEvents",
            "properties": {
                "Version": 2,
                "flows": [
                    {
                        "rule": "DefaultRule_DenyAllInBound",
                        "flows": [
                            {
                                "mac": "000D3AF87856",
                                "flowTuples": [
                                    "1542110437,125.64.94.197,10.5.16.4,59752,18264,T,I,D,B,,,,",
                                    "1542110475,80.211.72.221,10.5.16.4,37433,8088,T,I,D,B,,,,",
                                    "1542110487,46.101.199.124,10.5.16.4,60577,8088,T,I,D,B,,,,",
                                    "1542110490,176.119.4.30,10.5.16.4,57067,52801,T,I,D,B,,,,"
                                ]
                            }
                        ]
                    }
                ]
            }
        },
        ...
```

## <a name="next-steps"></a>Sonraki adımlar

- Akış günlüklerinin nasıl etkinleştirileceği hakkında bilgi edinmek için bkz. [NSG akış günlüğünü etkinleştirme](network-watcher-nsg-flow-logging-portal.md).
- Akış günlüklerini okumayı öğrenmek için bkz. [NSG akış günlüklerini okuma](network-watcher-read-nsg-flow-logs.md).
- NSG günlüğü hakkında daha fazla bilgi edinmek için bkz. [ağ güvenlik grupları (NSG 'ler) Için Azure izleyici günlükleri](../virtual-network/virtual-network-nsg-manage-log.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- Bir VM 'ye veya sanal makineye giden trafiğe izin verilip verilmediğini belirlemek için bkz. [BIR VM ağ trafiği tanılama sorunlarını Tanıla](diagnose-vm-network-traffic-filtering-problem.md)
