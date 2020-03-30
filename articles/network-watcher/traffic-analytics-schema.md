---
title: Azure trafik analizi şeması | Microsoft Dokümanlar
description: Azure ağ güvenliği grubu akış günlüklerini analiz etmek için Trafik Analitiği şemasını anlayın.
services: network-watcher
documentationcenter: na
author: vinynigam
manager: agummadi
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/26/2019
ms.author: vinigam
ms.openlocfilehash: ccfbb92c27e4508595f19c2ea6900730cde609b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74666384"
---
# <a name="schema-and-data-aggregation-in-traffic-analytics"></a>Trafik Analitiğinde Şema ve veri toplama

Traffic Analytics, bulut ağlarında kullanıcı ve uygulama etkinliğine görünürlük sağlayan bulut tabanlı bir çözümdür. Trafik Analizi, Azure bulutunuzdaki trafik akışı hakkında öngörüler sağlamak için Ağ İzleyiciağ güvenlik grubunu (NSG) akış günlüklerini analiz eder. Trafik analitiği ile şunları yapabilirsiniz:

- Azure aboneliklerinizde ağ etkinliğini görselleştirin ve etkin noktaları belirleyin.
- Açık bağlantı noktaları, internet erişimine çalışan uygulamalar ve sahte ağlara bağlanan sanal makineler (VM) gibi bilgilerle güvenlik tehditlerini belirleyin ve ağınızı güvence altına alayın.
- Ağ dağıtımınızı performans ve kapasite için optimize etmek için Azure bölgeleri ve Internet'teki trafik akışı desenlerini anlayın.
- Ağdaki başarısız bağlantılara yol açan ağ yanlış yapılandırmalarını sapta.
- Ağ kullanımını bayt, paket veya akışlarda bilin.

### <a name="data-aggregation"></a>Veri toplama

1. "FlowIntervalStartTime_t" ve "FlowIntervalEndTime_t" arasındaki NSG'deki tüm akış günlükleri, Traffic Analytics tarafından işlenmeden önce depolama hesabında bir dakikalık aralıklarla blob olarak yakalanır.
2. Trafik Analitiği varsayılan işlem aralığı 60 dakikadır. Bu, her 60 dakika Trafik Analitiği toplama için depolama dan lekeler alır anlamına gelir. Seçilen işlem aralığı 10 dakika ise, Trafik Analitiği her 10 dakika sonra depolama hesabından lekeler seçer.
3. Aynı Kaynak IP, Hedef IP, Hedef bağlantı noktası, NSG adı, NSG kuralı, Akış Yönü ve Aktarım katmanı protokolü (Not: Kaynak bağlantı noktası toplama için hariç) olan akışlar Trafik Analitiği tarafından tek bir akışa dönüştürülür
4. Bu tek kayıt dekore edilmiştir (aşağıdaki bölümdeki ayrıntılar) ve Trafik Analytics tarafından Log Analytics'te yutulan.Bu işlem en fazla 1 saat sürebilir.
5. FlowStartTime_t alanı, "FlowIntervalStartTime_t" ve "FlowIntervalEndTime_t" arasındaki akış günlüğü işleme aralığında böyle bir toplu akış (aynı dört tuple) ilk oluşumunu gösterir.
6. TA'daki herhangi bir kaynak için, Kullanıcı Arabirimi'nde belirtilen akışlar NSG tarafından görülen toplam akışlardır, ancak Log Analytics'te kullanıcı yalnızca tek ve azaltılmış kaydı görür. Tüm akışları görmek için Depolama'dan başvurulan blob_id alanını kullanın. Bu kaydın toplam akış sayısı, blob'da görülen tek tek akışla eşleşir.

Aşağıdaki sorgu, son 30 gün içinde şirket içi tüm akış günlüklerine bakmanıza yardımcı olur.
```
AzureNetworkAnalytics_CL
| where SubType_s == "FlowLog" and FlowStartTime_t >= ago(30d) and FlowType_s == "ExternalPublic"
| project Subnet_s  
```
Yukarıda belirtilen sorgudaki akışların blob yolunu görüntülemek için aşağıdaki sorguyu kullanın:

```
let TableWithBlobId =
(AzureNetworkAnalytics_CL
   | where SubType_s == "Topology" and ResourceType == "NetworkSecurityGroup" and DiscoveryRegion_s == Region_s and IsFlowEnabled_b
   | extend binTime = bin(TimeProcessed_t, 6h),
            nsgId = strcat(Subscription_g, "/", Name_s),
            saNameSplit = split(FlowLogStorageAccount_s, "/")
   | extend saName = iif(arraylength(saNameSplit) == 3, saNameSplit[2], '')
   | distinct nsgId, saName, binTime)
| join kind = rightouter (
   AzureNetworkAnalytics_CL
   | where SubType_s == "FlowLog"  
   | extend binTime = bin(FlowEndTime_t, 6h)
) on binTime, $left.nsgId == $right.NSGList_s  
| extend blobTime = format_datetime(todatetime(FlowIntervalStartTime_t), "yyyy MM dd hh")
| extend nsgComponents = split(toupper(NSGList_s), "/"), dateTimeComponents = split(blobTime, " ")
| extend BlobPath = strcat("https://", saName,
                        "@insights-logs-networksecuritygroupflowevent/resoureId=/SUBSCRIPTIONS/", nsgComponents[0],
                        "/RESOURCEGROUPS/", nsgComponents[1],
                        "/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/", nsgComponents[2],
                        "/y=", dateTimeComponents[0], "/m=", dateTimeComponents[1], "/d=", dateTimeComponents[2], "/h=", dateTimeComponents[3],
                        "/m=00/macAddress=", replace(@"-", "", MACAddress_s),
                        "/PT1H.json")
| project-away nsgId, saName, binTime, blobTime, nsgComponents, dateTimeComponents;

TableWithBlobId
| where SubType_s == "FlowLog" and FlowStartTime_t >= ago(30d) and FlowType_s == "ExternalPublic"
| project Subnet_s , BlobPath
```

Yukarıdaki sorgu, blob'a doğrudan erişmek için bir URL oluşturuyor. Yer sahiplerinin url'si aşağıdadır:

```
https://{saName}@insights-logs-networksecuritygroupflowevent/resoureId=/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroup}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json

```

### <a name="fields-used-in-traffic-analytics-schema"></a>Trafik Analitiği şemasında kullanılan alanlar
  > [!IMPORTANT]
  > Trafik Analitiği Şeması 22 Ağustos 2019 tarihinde güncellenmiştir. Yeni şema, akış yön alanını ayrışdırarak sorguları daha basit hale getirme gereksinimini ayrı ayrı ortadan kaldırarak kaynak ve hedef IP'leri ayrı ayrı sağlar. </br>
  > FASchemaVersion_s 1'den 2'ye kadar güncellendi. </br>
  > Amortismana FlowCount_d alanları: VMIP_s, Subscription_s, Region_s, NSGRules_s, Subnet_s, VM_s, NIC_s, PublicIPs_s, FlowCount_d </br>
  > Yeni alanlar: SrcPublicIPs_s, DestPublicIPs_s, NSGRule_s </br>
  > Amortismana uyacağız alanlar 22 Kasım 2019 tarihine kadar geçerli olacaktır.

Trafik Analitiği, Log Analytics'in üzerine kuruludur, böylece Trafik Analitiği tarafından dekore edilmiş veriler üzerinde özel sorgular çalıştırabilir ve uyarıları aynı şekilde ayarlayabilirsiniz.

Aşağıda şema alanları ve ne anlama olduğunu listelenmiştir

| Alan | Biçimlendir | Yorumlar |
|:---   |:---    |:---  |
| TableName | AzureNetworkAnalytics_CL | Trafik Analitiği verileri için tablo
| SubType_s | FlowLog | Akış günlükleri için alt yazı. Yalnızca "FlowLog" kullanın, SubType_s diğer değerleri ürünün dahili işleyişi içindir |
| FASchemaVersion_s |   2   | Şema versiyonu. NSG Akış Günlüğü sürümünü yansıtmaz |
| TimeProcessed_t   | UTC'de Tarih ve Saat  | Trafik Analitiği'nin depolama hesabından ham akış günlüklerini işlediği saat |
| FlowIntervalStartTime_t | UTC'de Tarih ve Saat |  Akış günlüğü işleme aralığının başlangıç zamanı. Bu, akış aralığının ölçüldüğü zaman |
| FlowIntervalEndTime_t | UTC'de Tarih ve Saat | Akış günlüğü işlem aralığının bitiş süresi |
| FlowStartTime_t | UTC'de Tarih ve Saat |  "FlowIntervalStartTime_t" ve "FlowIntervalEndTime_t" arasındaki akış günlüğü işleme aralığında akışın ilk oluşumu (toplu alacak). Bu akış toplama mantığına göre toplanır |
| FlowEndTime_t | UTC'de Tarih ve Saat | "FlowIntervalStartTime_t" ve "FlowIntervalEndTime_t" arasındaki akış günlüğü işleme aralığında ki akışın (toplu olarak toplanır) son oluşumu. Akış günlüğü v2 açısından, bu alan aynı dört-tuple ile son akış (ham akış kaydında "B" olarak işaretlenmiş) başladığı zamanı içerir |
| FlowType_s |  * IntraVNet <br> * Intervnet <br> * S2S <br> * P2S <br> * AzureGenel <br> * Dış Kamu <br> * Kötü Niyetli Akış <br> * Bilinmeyen Özel <br> * Bilinmiyor | Tablonun altındaki notlarda tanım |
| SrcIP_s | Kaynak IP adresi | AzurePublic ve ExternalPublic akışları durumunda boş olacak |
| DestIP_s | Hedef IP adresi | AzurePublic ve ExternalPublic akışları durumunda boş olacak |
| VMIP_s | IP of the VM | AzurePublic ve ExternalPublic akışları için kullanılır |
| PublicIP_s | Genel IP adresleri | AzurePublic ve ExternalPublic akışları için kullanılır |
| DestPort_d | Hedef Bağlantı Noktası | Trafiğin geldiği bağlantı noktası |
| L4Protocol_s  | * T <br> * U  | Ulaşım Protokolü. T = TCP <br> U = UDP |
| L7Protocol_s  | Protokol Adı | Hedef bağlantı noktasından türetilmiş |
| FlowDirection_s | * I = Gelen<br> * O = Giden | Akış günlüğüne göre NSG'nin giriş/çıkış akışının yönü |
| FlowStatus_s  | * A = NSG Kuralı tarafından izin <br> * D = NSG Kuralı ile Reddedildi  | Akış günlüğüne göre NSG tarafından izin verilen/engellenen akış durumu |
| NSGList_s | \<NSG_NAME NSG_NAME \/>'><RESOURCEGROUP_NAME>><\/ RESOURCEGROUP_NAME | Akışla ilişkili Ağ Güvenlik Grubu (NSG) |
| NSGRules_s | \<Dizin değeri \| \<0)>NSG_RULENAME>\| \<Akış Yönü>\| \<Akış Durumu>\| \<FlowCount> |  Bu akışa izin veren veya reddeden NSG kuralı |
| NSGRule_s | NSG_RULENAME |  Bu akışa izin veren veya reddeden NSG kuralı |
| NSGRuleType_s | * Kullanıcı Tanımlı * Varsayılan |   Akış tarafından kullanılan NSG Kuralı türü |
| MACAddress_s | MAC Adresi | Akışın yakalandığı NIC'nin MAC adresi |
| Subscription_s | Azure sanal ağ/ ağ arabirimi/ sanal makine aboneliği bu alanda doldurulur | Yalnızca FlowType = S2S, P2S, AzurePublic, ExternalPublic, MaliciousFlow ve UnknownPrivate akış türleri için geçerlidir (yalnızca bir tarafın azure olduğu akış türleri) |
| Subscription1_s | Abonelik Kimliği | Akıştaki kaynak IP'nin ait olduğu sanal ağ/ ağ arabirimi/ sanal makinenin abonelik kimliği |
| Subscription2_s | Abonelik Kimliği | Akıştaki hedef IP'nin ait olduğu sanal ağ/ağ arabirimi/ sanal makinenin abonelik kimliği |
| Region_s | Akıştaki IP'nin ait olduğu sanal ağ/ağ arabirimi/ sanal makinenin Azure bölgesi | Yalnızca FlowType = S2S, P2S, AzurePublic, ExternalPublic, MaliciousFlow ve UnknownPrivate akış türleri için geçerlidir (yalnızca bir tarafın azure olduğu akış türleri) |
| Region1_s | Azure Bölgesi | Akıştaki kaynak IP'nin ait olduğu sanal ağ/ağ arabirimi/ sanal makinenin Azure bölgesi |
| Region2_s | Azure Bölgesi | Akıştaki hedef IP'nin ait olduğu sanal ağın Azure bölgesi |
| NIC_s | \<resourcegroup_Name \/ \<>NetworkInterfaceName> |  VM'nin trafiği göndermesi veya almasıyla ilişkili NIC |
| NIC1_s | <resourcegroup_Name\<>/ NetworkInterfaceNameName> | Akıştaki kaynak IP ile ilişkili NIC |
| NIC2_s | <resourcegroup_Name\<>/ NetworkInterfaceNameName> | Akıştaki hedef IP ile ilişkili NIC |
| VM_s | <resourcegroup_Name \/ \<>NetworkInterfaceName> | Ağ arabirimi ile ilişkili Sanal Makine NIC_s |
| VM1_s | <resourcegroup_Name\<>/ VirtualMachineName> | Akıştaki kaynak IP ile ilişkili Sanal Makine |
| VM2_s | <resourcegroup_Name\<>/ VirtualMachineName> | Akıştaki hedef IP ile ilişkili Sanal Makine |
| Subnet_s | <ResourceGroup_Name>/<\<VNET_Name>/ SubnetName> | NIC_s ile ilişkili alt ağ |
| Subnet1_s | <ResourceGroup_Name>/<\<VNET_Name>/ SubnetName> | Akıştaki Kaynak IP ile ilişkili alt ağ |
| Subnet2_s | <ResourceGroup_Name>/<\<VNET_Name>/ SubnetName>    | Akıştaki Hedef IP ile ilişkili alt ağ |
| ApplicationGateway1_s | \<SubscriptionID>/\<ResourceGroupName\<>/ ApplicationGatewayName> | Akıştaki Kaynak IP ile ilişkili uygulama ağ geçidi |
| ApplicationGateway2_s | \<SubscriptionID>/\<ResourceGroupName\<>/ ApplicationGatewayName> | Akıştaki Hedef IP ile ilişkili uygulama ağ geçidi |
| LoadBalancer1_s | \<SubscriptionID>/\<ResourceGroupName\<>/ LoadBalancerName> | Akıştaki Kaynak IP ile ilişkili yük dengeleyicisi |
| LoadBalancer2_s | \<SubscriptionID>/\<ResourceGroupName\<>/ LoadBalancerName> | Akıştaki Hedef IP ile ilişkili yük dengeleyicisi |
| LocalNetworkGateway1_s | \<SubscriptionID>/\<ResourceGroupName\<>/ LocalNetworkGatewayName> | Akıştaki Kaynak IP ile ilişkili yerel ağ ağ geçidi |
| LocalNetworkGateway2_s | \<SubscriptionID>/\<ResourceGroupName\<>/ LocalNetworkGatewayName> | Akışta Hedef IP ile ilişkili yerel ağ ağ geçidi |
| ConnectionType_s | Olası değerler VNetPeering, VpnGateway ve ExpressRoute'dur |    Bağlantı Türü |
| ConnectionName_s | \<SubscriptionID>/\<ResourceGroupName\<>/ ConnectionName> | Bağlantı Adı. Akış türü P2S için, bu <gateway name>_ olarak biçimlendirilir<VPN Client IP> |
| ConnectingVNets_s | Sanal ağ adlarının ayrılmış alan listesi | Hub ve kollu topoloji durumunda, hub sanal ağlar burada doldurulur |
| Country_s | İki harfli ülke kodu (ISO 3166-1 alfa-2) | Akış türü Harici Genel için doldurulur. PublicIPs_s alanındaki tüm IP adresleri aynı ülke kodunu paylaşır |
| AzureRegion_s | Azure bölge konumları | Akış türü AzurePublic için doldurulur. PublicIPs_s alanındaki tüm IP adresleri Azure bölgesini paylaşır |
| AllowedInFlows_d | | İzin verilen gelen akış sayısı. Bu, akışın yakalandığı ağ arabirimine gelen aynı dört tuple'ı paylaşan akış sayısını temsil eder |
| DeniedInFlows_d |  | Reddedilen gelen akış sayısı. (Akışın yakalandığı ağ arabirimine gelen) |
| AllowedOutFlows_d | | İzin verilen giden akışların sayısı (Akışın yakalandığı ağ arabirimine giden) |
| DeniedOutFlows_d  | | Reddedilen giden akış sayısı (Akışın yakalandığı ağ arabirimine giden) |
| FlowCount_d | Kullanım dışı. Aynı dört-tuple eşleşen toplam akışları. Akış türleri Harici Genel ve AzurePublic durumunda, sayım çeşitli PublicIP adreslerinden gelen akışları da içerir.
| InboundPackets_d | NSG kuralının uygulandığı ağ arabiriminde yakalanan paketler | Bu sadece NSG akış günlüğü şeması Sürüm 2 için doldurulur |
| OutboundPackets_d  | NSG kuralının uygulandığı ağ arabiriminde yakalanan paketler | Bu sadece NSG akış günlüğü şeması Sürüm 2 için doldurulur |
| InboundBytes_d |  NSG kuralının uygulandığı ağ arabiriminde yakalanan baytlar | Bu sadece NSG akış günlüğü şeması Sürüm 2 için doldurulur |
| OutboundBytes_d | NSG kuralının uygulandığı ağ arabiriminde yakalanan baytlar olarak gönderilir | Bu sadece NSG akış günlüğü şeması Sürüm 2 için doldurulur |
| CompletedFlows_d  |  | Bu, yalnızca NSG akış günlüğü şemasının Sürüm 2'si için sıfır olmayan değerle doldurulur |
| PublicIPs_s | <> \| \< \<INBOUND_BYTES \| \< \<OUTBOUND_BYTES \| \< \|>\| \< \|OUTBOUND_PACKETS>>FLOW_STARTED_COUNT>>>>>>INBOUND_PACKETS INBOUND_PACKETS INBOUND_PACKETS INBOUND_PACKETS FLOW_ENDED_COUNT FLOW_ENDED_COUNT FLOW_ENDED_COUNT FLOW_ENDED_COUNT PUBLIC_IP | Çubuklarla ayrılmış girişler |
| SrcPublicIPs_s | >>\| \< \| \< \| \< \| \<OUTBOUND_BYTES>\|OUTBOUND_PACKETS>FLOW_STARTED_COUNT SOURCE_PUBLIC_IP <INBOUND_BYTES INBOUND_BYTES INBOUND_BYTES INBOUND_BYTES INBOUND_BYTES INBOUND_BYTES INBOUND_BYTES INBOUND_BYTES>'INBOUND_BYTES'INBOUND_BYTES>>>>>>>INBOUND_PACKETS INBOUND_PACKETS INBOUND_PACKETS FLOW_ENDED_COUNT \< \| \< | Çubuklarla ayrılmış girişler |
| DestPublicIPs_s | <DESTINATION_PUBLIC_IP \| \<INBOUND_PACKETS \| \<>\| \< \| \<OUTBOUND_PACKETS>\| \<FLOW_ENDED_COUNT \| \<>FLOW_STARTED_COUNT>>>>>>>INBOUND_BYTES>>>>>>>>>>>INBOUND_BYTES> INBOUND_BYTES INBOUND_BYTES INBOUND_BYTES>>>>>>>>>> INBOUND_BYTES INBOUND_BYTES INBOUND_BYTES INBOUND_BYTES>OUTBOUND_BYTES>>>>>>>>> | Çubuklarla ayrılmış girişler |

### <a name="notes"></a>Notlar

1. AzurePublic ve ExternalPublic akışları söz konusu olduğunda, müşteriye ait Azure VM IP VMIP_s alanında doldurulurken, Ortak IP adresleri PublicIPs_s alanında doldurulur. Bu iki akış türü için, SrcIP_s ve DestIP_s alanları yerine VMIP_s ve PublicIPs_s kullanmalıyız. AzurePublic ve ExternalPublicIP adresleri için, müşteri günlüğü analizi çalışma alanına alınan kayıt sayısının en az olması için daha fazla bilgi toyar. (Bu alan yakında amortismana sunulacak ve azure VM'nin kaynak mı yoksa akıştaki hedef mi olduğuna bağlı olarak SrcIP_ ve DestIP_s kullanmalıyız)
1. Akış türleri için ayrıntılar: Akışla ilgili IP adreslerine bağlı olarak, akışları aşağıdaki akış türlerine göre kategorilere ayırıyoruz:
1. IntraVNet – Akıştaki her iki IP adresi de aynı Azure Sanal Ağı'nda yer eder.
1. InterVNet - Akıştaki IP adresleri iki farklı Azure Sanal Asında yer eder.
1. S2S – (Siteden Siteye) IP adreslerinden biri Azure Sanal Ağı'na, diğer IP adresi ise VPN ağ geçidi veya Ekspres Rota üzerinden Azure Sanal Ağına bağlı müşteri ağına (Site) aittir.
1. P2S - (Noktaya Siteye) IP adreslerinden biri Azure Sanal Ağı'na, diğer IP adresi ise VPN ağ geçidi aracılığıyla Azure Sanal Ağı'na bağlı müşteri ağına (Site) aittir.
1. AzurePublic - IP adreslerinden biri Azure Sanal Ağı'na, diğer IP adresi ise Microsoft'a ait Azure Dahili Genel IP adreslerine aittir. Müşteriye ait Genel IP adresleri bu akış türünün bir parçası olmayacaktır. Örneğin, bir Azure Hizmetine (Depolama bitiş noktası) trafik gönderen herhangi bir müşteri vm sahip olunan herhangi bir akış türü altında kategorize edilir.
1. ExternalPublic - IP adreslerinden biri Azure Sanal Ağı'na aitken, diğer IP adresi Azure'da olmayan genel bir IP'dir, Trafik Analitiği arasındaki işlem aralığı için tükettiği ASC akışlarında kötü amaçlı olarak bildirilmemiştir. FlowIntervalStartTime_t" ve "FlowIntervalEndTime_t".
1. Kötü Amaçlı Akış - IP adreslerinden biri azure sanal ağa aitken, diğer IP adresi Azure'da olmayan ve Trafik Analitiği'nin " arasındaki işlem aralığı için tükettiği ASC akışlarında kötü amaçlı olarak bildirilen genel bir IP' dir. FlowIntervalStartTime_t" ve "FlowIntervalEndTime_t".
1. UnknownPrivate - IP adreslerinden biri Azure Sanal Ağı'na aitken, diğer IP adresi RFC 1918'de tanımlandığı şekilde özel IP aralığına aittir ve Traffic Analytics tarafından müşteriye ait bir siteye veya Azure Sanal Ağı'na eşlenemedi.
1. Bilinmiyor – Azure'daki müşteri topolojisi ve şirket içi (site) ile akışlarda IP adreslerinden herhangi birinin haritasını koyamıyor.
1. Bazı alan adları s \_veya \_d ile eklenir. Bunlar kaynak ve hedef anlamına gelmez, ancak sırasıyla veri türleri dize ve ondalık gösterir.

### <a name="next-steps"></a>Sonraki Adımlar
Sık sorulan soruların yanıtlarını almak için, işlevsellikle ilgili ayrıntıları görmek için [Trafik analizi SSS](traffic-analytics-faq.md) bölümüne bakın, [Trafik analizi belgelerine](traffic-analytics.md) bakın
