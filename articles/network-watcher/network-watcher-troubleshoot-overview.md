---
title: Kaynak sorun gidermeye giriş
titleSuffix: Azure Network Watcher
description: Bu sayfada ağ Izleyicisi kaynağı sorun giderme özelliklerine genel bir bakış sunulmaktadır
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: c1145cd6-d1cf-4770-b1cc-eaf0464cc315
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: kumud
ms.openlocfilehash: 736bbd16456dd0abda3292b9b9e73ea5b941e7ed
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277784"
---
# <a name="introduction-to-resource-troubleshooting-in-azure-network-watcher"></a>Azure ağ Izleyicisi 'nde kaynak sorunlarını gidermeye giriş

Sanal ağ geçitleri, şirket içi kaynaklar ve Azure 'daki diğer sanal ağlar arasında bağlantı sağlar. Ağ geçitlerinin ve bağlantılarının izlenmesi, iletişimin kesilmemesini güvence altına alma açısından kritik önem taşır. Ağ Izleyicisi ağ geçitlerinin ve bağlantıların sorunlarını giderme yeteneği sağlar. Bu özellik Portal, PowerShell, Azure CLı veya REST API aracılığıyla çağrılabilir. Çağrıldığında, ağ Izleyicisi ağ geçidinin veya bağlantının sistem durumunu tanılar ve uygun sonuçları döndürür. İstek, uzun süre çalışan bir işlemdir. Tanı tamamlandıktan sonra sonuçlar döndürülür.

![portal][2]

## <a name="results"></a>Sonuçlar

Döndürülen sonuç, kaynağın sistem durumunun genel bir resmini verir. Aşağıdaki bölümde gösterildiği gibi kaynaklar için daha derin bilgiler de sağlanmaktadır:

Aşağıdaki liste, sorun giderme API 'SI ile döndürülen değerlerdir:

* **StartTime** -bu değer, sorun giderme API 'si çağrısının başlatıldığı süredir.
* **BitişZamanı** -bu değer, sorun gidermenin sona erdiği süredir.
* **Code** -tek bir tanılama hatası varsa bu değer sağlıksız olur.
* **sonuçlar** -sonuçlar, bağlantıda veya sanal ağ geçidinde döndürülen bir sonuç koleksiyonudur.
    * **ID** -bu değer hata türüdür.
    * **Özet** -bu değer hatanın bir özetidir.
    * **ayrıntılı** -bu değer hatanın ayrıntılı bir açıklamasını sağlar.
    * **recommendedActions** -bu özellik, gerçekleştirilecek önerilen eylemlerin bir koleksiyonudur.
      * **ActionText** -bu değer, hangi eylemin yapılacağını açıklayan metni içerir.
      * **Actionuri** -bu değer, nasıl davranması ile ılgılı belgelere URI sağlar.
      * **actionUriText** -bu değer eylem metninin kısa bir açıklamasıdır.

Aşağıdaki tablolarda, kullanılabilir olan farklı hata türleri (önceki listenin sonuçları altında kimlik) ve hata günlükleri oluşturduğunda gösterilmektedir.

### <a name="gateway"></a>Ağ Geçidi

| Hata türü | Neden | Günlük|
|---|---|---|
| Nofatoult | Hiçbir hata algılanmadığında |Evet|
| GatewayNotFound | Ağ Geçidi bulunamıyor veya ağ geçidi sağlanmadı |Hayır|
| PlannedMaintenance |  Ağ Geçidi örneği bakımda  |Hayır|
| UserDrivenUpdate | Bu hata, bir Kullanıcı güncelleştirmesi devam ederken oluşur. Güncelleştirme bir yeniden boyutlandırma işlemi olabilir. | Hayır |
| Canlı yanıt verme | Bu hata, ağ geçidinin birincil örneğine bir sistem durumu araştırma hatası nedeniyle ulaşılamadığından oluşur. | Hayır |
| PlatformInActive | Platformda bir sorun var. | Hayır|
| ServiceNotRunning | Temel alınan hizmet çalışmıyor. | Hayır|
| NoConnectionsFoundForGateway | Ağ geçidinde bağlantı yok. Bu hata yalnızca bir uyarıdır.| Hayır|
| ConnectionsNotConnected | Bağlantılar bağlı değil. Bu hata yalnızca bir uyarıdır.| Evet|
| Gatewaycpuusageaşıldı | Geçerli ağ geçidi CPU kullanımı %95 >. | Evet |

### <a name="connection"></a>Bağlantı

| Hata türü | Neden | Günlük|
|---|---|---|
| Nofatoult | Hiçbir hata algılanmadığında |Evet|
| GatewayNotFound | Ağ Geçidi bulunamıyor veya ağ geçidi sağlanmadı |Hayır|
| PlannedMaintenance | Ağ Geçidi örneği bakımda  |Hayır|
| UserDrivenUpdate | Bu hata, bir Kullanıcı güncelleştirmesi devam ederken oluşur. Güncelleştirme bir yeniden boyutlandırma işlemi olabilir.  | Hayır |
| Canlı yanıt verme | Bu hata, ağ geçidinin birincil örneğine bir sistem durumu araştırma hatası nedeniyle ulaşılamadığından oluşur. | Hayır |
| ConnectionEntityNotFound | Bağlantı yapılandırması eksik | Hayır |
| Connectionımarkedconnected | Bağlantı "bağlantısı kesildi" olarak işaretlendi |Hayır|
| ConnectionNotConfiguredOnGateway | Temeldeki hizmette bağlantı yapılandırılmamış. | Evet |
| ConnectionMarkedStandby | Temel alınan hizmet bekleme olarak işaretlendi.| Evet|
| Kimlik Doğrulaması | Önceden paylaşılmış anahtar uyumsuzluğu | Evet|
| Peerulaşılabilirlik | Eş ağ geçidine erişilemiyor. | Evet|
| Ikepolicyuyuşmazlığıdır | Eş ağ geçidinde Azure tarafından desteklenmeyen ıKE ilkeleri vardır. | Evet|
| WfpParse Error | WFP günlüğü ayrıştırılırken bir hata oluştu. |Evet|

## <a name="supported-gateway-types"></a>Desteklenen ağ geçidi türleri

Aşağıdaki tabloda ağ Izleyicisi sorunlarını gidermek için hangi ağ geçitlerinin ve bağlantıların desteklendiği listelenmektedir:

|  |  |
|---------|---------|
|**Ağ geçidi türleri**   |         |
|VPN      | Desteklenen        |
|ExpressRoute | Desteklenmiyor |
|**VPN türleri** | |
|Rota tabanlı | Desteklenen|
|İlke tabanlı | Desteklenmiyor|
|**Bağlantı türleri**||
|Bkz| Desteklenen|
|VNet2Vnet| Desteklenen|
|ExpressRoute| Desteklenmiyor|
|VPNClient| Desteklenmiyor|

## <a name="log-files"></a>Günlük dosyaları

Kaynak sorun giderme günlük dosyaları, kaynak sorunlarını giderme işlemi tamamlandıktan sonra bir depolama hesabında depolanır. Aşağıdaki görüntüde, bir hata ile sonuçlanan bir çağrının örnek içerikleri gösterilmektedir.

![zip dosyası][1]

> [!NOTE]
> Bazı durumlarda, günlük dosyalarının yalnızca bir alt kümesi depolamaya yazılır.

Azure depolama hesaplarından dosya indirme yönergeleri için bkz. [.NET kullanarak Azure Blob depolamayı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Kullanılabilecek başka bir araç Depolama Gezgini. Depolama Gezgini hakkında daha fazla bilgi aşağıdaki bağlantıda bulunabilir: [Depolama Gezgini](https://storageexplorer.com/)

### <a name="connectionstatstxt"></a>ConnectionStats. txt

**Connectionstats. txt** dosyası, giriş ve çıkış baytları, bağlantı durumu ve bağlantının kurulduğu zaman dahil olmak üzere bağlantının genel istatistiklerini içerir.

> [!NOTE]
> Sorun giderme API 'SI çağrısı sağlıklı döndürüyorsa, ZIP dosyasında döndürülen tek şey bir **Connectionstats. txt** dosyasıdır.

Bu dosyanın içeriği aşağıdaki örneğe benzer:

```
Connectivity State : Connected
Remote Tunnel Endpoint :
Ingress Bytes (since last connected) : 288 B
Egress Bytes (Since last connected) : 288 B
Connected Since : 2/1/2017 8:22:06 PM
```

### <a name="cpustatstxt"></a>CPUStats. txt

**Cpustats. txt** dosyası, test SıRASıNDA kullanılabilir CPU kullanımı ve bellek içerir.  Bu dosyanın içeriği aşağıdaki örneğe benzer:

```
Current CPU Usage : 0 % Current Memory Available : 641 MBs
```

### <a name="ikeerrorstxt"></a>Ikeerrors. txt

**Ikeerrors. txt** dosyası, izleme sırasında bulunan tüm IKE hatalarını içerir.

Aşağıdaki örnek, bir ıkeerrors. txt dosyasının içeriğini gösterir. Sorununuz soruna bağlı olarak farklı olabilir.

```
Error: Authentication failed. Check shared key. Check crypto. Check lifetimes. 
     based on log : Peer failed with Windows error 13801(ERROR_IPSEC_IKE_AUTH_FAIL)
Error: On-prem device sent invalid payload. 
     based on log : IkeFindPayloadInPacket failed with Windows error 13843(ERROR_IPSEC_IKE_INVALID_PAYLOAD)
```

### <a name="scrubbed-wfpdiagtxt"></a>Scrubbed-wfpdiag. txt

**Scrubbed-wfpdiag. txt** günlük dosyası WFP günlüğünü içerir. Bu günlük, paket bırakma ve ıKE/AuthIP hatalarının günlüğe kaydedilmesini içerir.

Aşağıdaki örnek, Scrubbed-wfpdiag. txt dosyasının içeriğini gösterir. Bu örnekte, bir bağlantının paylaşılan anahtarı doğru değildi, en alttan üçüncü satırdan görünebilirler. Aşağıdaki örnek, bir günlüğün, soruna bağlı olarak uzun sürebilecek bir bütün bir kod parçacıkdır.

```
...
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Deleted ICookie from the high priority thread pool list
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|IKE diagnostic event:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Event Header:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Timestamp: 1601-01-01T00:00:00.000Z
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Flags: 0x00000106
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Local address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Remote address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IP version field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP version: IPv4
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP protocol: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local address: 13.78.238.92
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote address: 52.161.24.36
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Application ID:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  User SID: <invalid>
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Failure type: IKE/Authip Main Mode Failure
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Type specific info:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure error code:0x000035e9
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IKE authentication credentials are unacceptable
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure point: Remote
...
```

### <a name="wfpdiagtxtsum"></a>wfpdiag. txt. Sum

**Wfpdiag. txt. Sum** dosyası, işlenen arabellekleri ve olayları gösteren bir günlüğe kaydedilir.

Aşağıdaki örnek bir wfpdiag. txt. Sum dosyasının içeriğidir.
```
Files Processed:
    C:\Resources\directory\924336c47dd045d5a246c349b8ae57f2.GatewayTenantWorker.DiagnosticsStorage\2017-02-02T17-34-23\wfpdiag.etl
Total Buffers Processed 8
Total Events  Processed 2169
Total Events  Lost      0
Total Format  Errors    0
Total Formats Unknown   486
Elapsed Time            330 sec
+-----------------------------------------------------------------------------------+
|EventCount    EventName            EventType   TMF                                 |
+-----------------------------------------------------------------------------------+
|        36    ikeext               ike_addr_utils_c844  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        12    ikeext               ike_addr_utils_c857  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        96    ikeext               ike_addr_utils_c832  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|         6    ikeext               ike_bfe_callbacks_c133  1dc2d67f-8381-6303-e314-6c1452eeb529|
|         6    ikeext               ike_bfe_callbacks_c61  1dc2d67f-8381-6303-e314-6c1452eeb529|
|        12    ikeext               ike_sa_management_c5698  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c8447  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c494  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c642  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c3162  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c3307  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
```

## <a name="next-steps"></a>Sonraki adımlar

Ağ geçidi veya ağ geçidi bağlantısıyla ilgili bir sorunu tanılamayı öğrenmek için bkz. [ağlar arasındaki iletişim sorunlarını tanılama](diagnose-communication-problem-between-networks.md).
<!--Image references-->

[1]: ./media/network-watcher-troubleshoot-overview/GatewayTenantWorkerLogs.png
[2]: ./media/network-watcher-troubleshoot-overview/portal.png
