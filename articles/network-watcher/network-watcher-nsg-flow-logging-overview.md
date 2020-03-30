---
title: NSG'ler için akış günlüğüne giriş
titleSuffix: Azure Network Watcher
description: Bu makalede, Azure Ağ İzleyicisi'nin NSG akış günlükleri özelliğinin nasıl kullanılacağı açıklanmaktadır.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: fb4a55b9757748581e26f3d6594f9be2139658cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78228265"
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a>Ağ güvenlik grupları için akış günlüğe giriş

Ağ güvenlik grubu (NSG) akış günlükleri NSG üzerinden giren ve çıkan IP trafiği hakkındaki bilgileri görüntülemenize olanak tanıyan bir Ağ İzleyicisi özelliğidir. Akış günlükleri JSON biçiminde yazılır ve kural tabanında giden ve gelen akışları, akışın uygulandığı ağ arabirimini (NIC), akış hakkındaki 5 tanımlama grubu (kaynak/hedef IP, kaynak/hedef bağlantı noktası ve protokol) bilgilerini ve trafiği izin verildiği veya trafiğin engellendiği bilgisini, ayrıca 2. Sürüm'de işleme hızı bilgilerini (Bayt ve Paket) gösterir.


![akış günlüklerine genel bakış](./media/network-watcher-nsg-flow-logging-overview/figure1.png)

Akış günlükleri NSG'leri hedef alırken, bunlar diğer günlüklerle aynı görüntülenmez. Akış günlükleri yalnızca bir depolama hesabında depolanır ve aşağıdaki örnekte gösterilen günlük yolunu izleyin:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```
Akış günlüklerini analiz edebilir ve [trafik analitiğini](traffic-analytics.md)kullanarak ağ trafiğiniz hakkında bilgi edinebilirsiniz.

Diğer günlükler için görülen aynı bekletme ilkeleri akış günlükleri için geçerlidir. Günlük bekletme ilkesini 1 günden 365 güne ayarlayabilirsiniz. Bekletme ilkesi ayarlanmazsa, günlükler sonsuza kadar korunur.

## <a name="log-file"></a>Günlük dosyası

Akış günlükleri aşağıdaki özellikleri içerir:

* **zaman** - Olayın günlüğe kaydedildiği saat
* **systemId** - Ağ Güvenlik Grubu kaynak Kimliği.
* **kategori** - etkinliğin kategorisi. Kategori her zaman **NetworkSecurityGroupFlowEvent** olduğunu
* **resourceid** - NSG kaynak Kimliği
* **operationName** - Her Zaman NetworkSecurityGroupFlowEvents
* **özellikleri** - Akış özellikleri nin bir koleksiyon
    * **Sürüm** - Akış Günlüğü olay şemasının sürüm numarası
    * **akar** - akışları bir koleksiyon. Bu özellik, farklı kurallar için birden çok girişi vardır
        * **kural** - Akışların listelendiği kural
            * **akar** - akışları bir koleksiyon
                * **mac** - Akış toplandı VM için NIC MAC adresi
                * **flowTuples** - Virgülle ayrılmış biçimde akış tuple için birden çok özellik içeren bir dize
                    * **Zaman Damgası** - Bu değer, akışın UNIX dönem biçiminde oluştuğu zaman dilimidir
                    * **Kaynak IP** - Kaynak IP
                    * **Hedef IP** - Hedef IP
                    * **Kaynak Bağlantı Noktası** - Kaynak bağlantı noktası
                    * **Hedef Bağlantı Noktası** - Hedef Bağlantı Noktası
                    * **Protokol** - akış protokolü. Geçerli değerler TCP için **T,** UDP için **U**
                    * **Trafik Akışı** - Trafik akışının yönü. Geçerli değerler gelen **için I** ve giden için **O'dur.**
                    * **Trafik Kararı** - Trafiğe izin verilip verilmediveya reddedildi. Geçerli değerler izin verilen **a** ve reddedilen için **D'dir.**
                    * **Akış Durumu - Yalnızca Sürüm 2** - Akış durumunu yakalar. Olası durumlar **B**: Bir akış oluşturulduğunda begin. İstatistikler sağlanmaz. **C**: Devam eden bir akış için devam eden. İstatistikler 5 dakikalık aralıklarla sağlanır. **E**: Bir akış sonlandığında sona erer. İstatistikler sağlanır.
                    * **Paketler - Kaynaktan hedefe - Yalnızca Sürüm 2** Son güncelleştirmeden bu yana kaynaktan hedefe gönderilen toplam TCP veya UDP paketi sayısı.
                    * **Gönderilen baytlar - Kaynaktan hedefe - Sürüm 2 Yalnızca** Son güncelleştirmeden bu yana kaynaktan hedefe gönderilen toplam TCP veya UDP paket bayt sayısı. Paket baytları paket üstbilgisini ve yükü içerir.
                    * **Paketler - Kaynak hedef - Sürüm 2 Yalnızca** Son güncelleştirmeden bu yana hedeften kaynağa gönderilen toplam TCP veya UDP paketi sayısı.
                    * **Gönderilen baytlar - Hedefe kaynak - Sürüm 2 Yalnızca** Son güncelleştirmeden bu yana hedeften kaynağa gönderilen toplam TCP ve UDP paket bayt sayısı. Paket baytlar paket üstbilgi ve yük içerir.

## <a name="nsg-flow-logs-version-2"></a>NSG akış günlükleri sürüm 2

Günlüklerin Sürüm 2 akış durumunu tanıtır. Aldığınız akış günlüklerinin hangi sürümünü yapılandırabilirsiniz. Akış günlüklerini etkinleştirmeyi öğrenmek için [bkz.](network-watcher-nsg-flow-logging-portal.md)

Akış durumu *B,* bir akış başlatıldığında kaydedilir. Akış durumu *C* ve akış durumu *E,* sırasıyla bir akış ve akış sonlandırmasının devamını işaretleyen durumlardır. Hem *C* hem de *E* durumları trafik bant genişliği bilgilerini içerir.

**Örnek**: 185.170.185.105:35370 ve 10.2.0.4:23 arasındaki Bir TCP konuşmasından akış tuples:

"1493763938,185.170.185.105.105,10.2.0.4,35370,23,T,I,A,B,,,," "1493695838,185.170.185.105,10.2.0.4.35370,23,T,T A,C,1021,588096,8005,4610880" "1493696138,185.170.185.105,10.2.0.4,35370,23,T,I,A,E,52,29952,47,27072"

Devamı *C* ve bitiş *E* akış durumları için, bayt ve paket sayıları önceki akış tuple kaydının zamanından toplam sayımlardır. Önceki örnek konuşmaya atıfta bulunularak, aktarılan toplam paket sayısı 1021+52+8005+47 = 9125'tir. Aktarılan bayt ların toplam sayısı 588096+29952+4610880+27072 = 5256000'dür.

Aşağıdaki metin bir akış günlüğü örneğidir. Gördüğünüz gibi, önceki bölümde açıklanan özellik listesini izleyen birden çok kayıt vardır.

## <a name="nsg-flow-logging-considerations"></a>NSG akış günlüğü hususlar

**Depolama hesabı hususlar:** 

- Konum: Kullanılan depolama hesabı NSG ile aynı bölgede olmalıdır.
- Kendi kendini yöneten anahtar döndürme: Erişim anahtarlarını depolama hesabınıza değiştirir/döndürürseniz, NSG Akış Günlükleri çalışmayı durdurur. Bu sorunu gidermek için, NSG Akış Günlüklerini devre dışı bırakıp yeniden etkinleştirmeniz gerekir.

Bir **kaynağa bağlı tüm NSG'lerde NSG Akış Günlüğe**kaydetmeyi etkinleştirin : Azure'da akış günlüğü NSG kaynağında yapılandırılır. Akış yalnızca bir NSG Kuralı ile ilişkilendirilecektir. Birden çok NSG'nin kullanıldığı senaryolarda, tüm NSG'lerde tüm NSG'lerin tüm trafiğin kaydedilmesini sağlamak için bir kaynağın alt ağı veya ağ arabiriminde etkin olmasını öneririz. Daha fazla bilgi için Ağ Güvenlik Grupları'nda [trafiğin nasıl değerlendirildiğini](../virtual-network/security-overview.md#how-traffic-is-evaluated) öğrenin.

**Akış Günlüğü Maliyetleri**: NSG akış günlüğü, üretilen günlüklerin hacmine faturalandırılır. Yüksek trafik hacmi büyük akış günlüğü hacmi ve ilişkili maliyetlerneden olabilir. NSG Akış günlüğü fiyatlandırması, depolamanın temel maliyetlerini içermez. NSG Akış Günlüğü ile saklama ilkesi özelliğinin kullanılması, uzun süreler için ayrı depolama maliyetlerine neden olmak anlamına gelir. Bekletme ilkesi özelliğini gerektirmezseniz, bu değeri 0 olarak ayarlamanızı öneririz. Daha fazla bilgi için, ek ayrıntılar için [Ağ İzleyici fiyatlandırması](https://azure.microsoft.com/pricing/details/network-watcher/) ve [Azure Depolama Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/storage/) bakın.

**Çevrimiçi IP'lerden VM'lere genel IP'ler olmadan günlüğe kaydedilen gelen akışlar**: NIC ile ilişkili genel bir IP adresi aracılığıyla örnek düzeyinde genel IP olarak atanan veya temel yük dengeleyici arka uç havuzunun bir parçası olan, [varsayılan SNAT'ı](../load-balancer/load-balancer-outbound-connections.md#defaultsnat) kullanan ve giden bağlantıyı kolaylaştırmak için Azure tarafından atanan bir IP adresine sahip olan vm'ler. Sonuç olarak, akış SNAT için atanan bağlantı noktaları aralığındaki bir bağlantı noktasına mukadder ise, internet IP adreslerinden gelen akışlar için akış günlüğü girişleri görebilirsiniz. Azure bu akışlarıN VM'ye akmasına izin vermese de, deneme günlüğe kaydedilir ve ağ izleyicisinin NSG akış günlüğünde tasarım gereği görünür. İstenmeyen gelen internet trafiğinin NSG ile açıkça engellenmesini öneririz.

**Durumsuz akışlar için yanlış bayt ve paket sayıları**: [Ağ Güvenlik Grupları (NSG'ler)](https://docs.microsoft.com/azure/virtual-network/security-overview) [Devlet eki](https://en.wikipedia.org/wiki/Stateful_firewall?oldformat=true)güvenlik duvarı olarak uygulanır. Ancak trafik akışını kontrol eden birçok varsayılan/iç kural durumsuz bir şekilde uygulanır. Platform sınırlamaları nedeniyle, bayt ve paket sayıları durumsuz akışlar için kaydedilmez (diğer bir süre, trafik akışları devletsiz kurallardan geçer), bunlar yalnızca durumlu akışlar için kaydedilir. Sonuç olarak, NSG Akış Günlükleri'nde (ve Trafik Analizi'nde) bildirilen bayt ve paket sayısı gerçek akışlardan farklı olabilir. Bu sınırlamanın Haziran 2020'ye kadar düzeltilmesi planlanmaktadır.

## <a name="sample-log-records"></a>Örnek günlük kayıtları

Aşağıdaki metin bir akış günlüğü örneğidir. Gördüğünüz gibi, önceki bölümde açıklanan özellik listesini izleyen birden çok kayıt vardır.


> [!NOTE]
> **flowTuples* özelliğindeki değerler virgülle ayrılmış bir listedir.
 
### <a name="version-1-nsg-flow-log-format-sample"></a>Sürüm 1 NSG akış günlüğü biçimi örneği
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
### <a name="version-2-nsg-flow-log-format-sample"></a>Sürüm 2 NSG akış günlüğü biçimi örneği
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

- Akış günlüklerini etkinleştirmeyi öğrenmek için [bkz.](network-watcher-nsg-flow-logging-portal.md)
- Akış günlüklerini okumayı öğrenmek için [NSG akış günlüklerini okuyun'a](network-watcher-read-nsg-flow-logs.md)bakın.
- NSG günlüğü hakkında daha fazla bilgi edinmek [için ağ güvenlik grupları (NSG'ler) için Azure Monitor günlüklerine](../virtual-network/virtual-network-nsg-manage-log.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)bakın.
- Bir VM'de trafiğe izin verilip verilmediğini veya vm'den izin verilip verilmediğini belirlemek için [vm ağ trafiği filtresi sorunu tanıla'ya](diagnose-vm-network-traffic-filtering-problem.md) bakın
