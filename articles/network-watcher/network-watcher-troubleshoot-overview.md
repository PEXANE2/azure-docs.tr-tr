---
title: Kaynak sorun giderme ye giriş
titleSuffix: Azure Network Watcher
description: Bu sayfa, Ağ İzleyicikaynak sorun giderme özelliklerine genel bir bakış sağlar
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: damendo
ms.openlocfilehash: 199b4fc762919c2e3988f477c14d09fc23b0136b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840698"
---
# <a name="introduction-to-resource-troubleshooting-in-azure-network-watcher"></a>Azure Ağ İzleyicisi'nde kaynak sorun giderme ye giriş

Sanal Ağ Ağ Ağ Geçitleri, şirket içi kaynaklar la Azure'daki diğer sanal ağlar arasında bağlantı sağlar. Ağ geçitlerinin ve bağlantılarının izlenmesi, iletişimin kesilmemesini güvence altına alma açısından kritik önem taşır. Ağ İzleyicisi ağ geçitleri ve bağlantıları sorun giderme olanağı sağlar. Bu özellik portal, PowerShell, Azure CLI veya REST API üzerinden çağrılabilir. Çağrıldığında, Ağ İzleyicisi ağ geçidinin veya bağlantının sistem durumunu tanılar ve uygun sonuçları döndürür. İstek uzun süren bir işlemdir. Tanı tamamlandıktan sonra sonuçlar döndürülür.

![portal][2]

## <a name="results"></a>Sonuçlar

Döndürülen ön sonuçlar, kaynağın sağlık durumunun genel bir resmini verir. Aşağıdaki bölümde gösterildiği gibi kaynaklar için daha derin bilgiler sağlanabilir:

Aşağıdaki liste, sorun giderme API ile döndürülen değerlerdir:

* **startTime** - Bu değer, sorun giderme API çağrısının başladığı zamandır.
* **endTime** - Bu değer, sorun giderme süresinin sona erdirilen zamandır.
* **kod** - Tek bir tanı hatası varsa bu değer Sağlıksız'dır.
* **sonuçlar** - Sonuçlar, Bağlantı veya sanal ağ ağ geçidinde döndürülen sonuçlar topluluğudur.
    * **id** - Bu değer hata türüdür.
    * **özet** - Bu değer hatanın bir özetidir.
    * **ayrıntılı** - Bu değer hatanın ayrıntılı bir açıklamasını sağlar.
    * **recommendedActions** - Bu özellik, gerçekleşmesi önerilen eylemlerin bir koleksiyondur.
      * **actionText** - Bu değer, hangi eylemin gerçekleşmesi gerektiğini açıklayan metni içerir.
      * **actionUri** - Bu değer URI nasıl hareket etmek için belgelere sağlar.
      * **actionUriText** - Bu değer eylem metninin kısa bir açıklamasıdır.

Aşağıdaki tablolar, kullanılabilir farklı hata türlerini (önceki listedeki sonuçların altında id) ve hata günlükleri oluşturursa gösterir.

### <a name="gateway"></a>Ağ geçidi

| Hata Türü | Neden | Günlük|
|---|---|---|
| NoFault | Hata algılanmadığınızda |Evet|
| GatewayNotFound | Ağ geçidi veya ağ geçidi bulunamıyor sağlanmaz |Hayır|
| PlannedMaintenance |  Ağ geçidi örneği bakım altında  |Hayır|
| UserDrivenUpdate | Bu hata bir kullanıcı güncelleştirmesi devam ederken oluşur. Güncelleştirme bir yeniden boyutlandırma işlemi olabilir. | Hayır |
| VipUnResponsive | Bu hata, ağ geçidinin birincil örneğine bir sistem durumu yoklama hatası nedeniyle ulaşılamadığında oluşur. | Hayır |
| PlatformInActive | Platform ile ilgili bir sorun var. | Hayır|
| ServiceNotRunning | Temel hizmet çalışmıyor. | Hayır|
| NoConnectionsFoundForGateway | Ağ geçidinde bağlantı yok. Bu hata sadece bir uyarıdır.| Hayır|
| BağlantılarNotConnected | Bağlantılar bağlı değil. Bu hata sadece bir uyarıdır.| Evet|
| Ağ GeçidiCPUUsageExceeded | Geçerli ağ geçidi CPU kullanımı %95'>. | Evet |

### <a name="connection"></a>Bağlantı

| Hata Türü | Neden | Günlük|
|---|---|---|
| NoFault | Hata algılanmadığınızda |Evet|
| GatewayNotFound | Ağ geçidi veya ağ geçidi bulunamıyor sağlanmaz |Hayır|
| PlannedMaintenance | Ağ geçidi örneği bakım altında  |Hayır|
| UserDrivenUpdate | Bu hata bir kullanıcı güncelleştirmesi devam ederken oluşur. Güncelleştirme bir yeniden boyutlandırma işlemi olabilir.  | Hayır |
| VipUnResponsive | Bu hata, ağ geçidinin birincil örneğine bir sistem durumu yoklama hatası nedeniyle ulaşılamadığında oluşur. | Hayır |
| BağlantıEntityNotFound | Bağlantı yapılandırması eksik | Hayır |
| Bağlantıİşaretli Kesildi | Bağlantı "bağlantısı kesildi" olarak işaretlenir |Hayır|
| BağlantıNotConfiguredOnGateway | Temel hizmet, bağlantı yapılandırıldı. | Evet |
| BağlantıİşaretliStandby | Temel hizmet bekleme olarak işaretlenir.| Evet|
| Kimlik doğrulaması | Önceden paylaşılan anahtar uyuşmazlığı | Evet|
| PeerReachability | Eş ağ geçidine erişilemez. | Evet|
| IkePolicyMismatch | Eş ağ geçidinde Azure tarafından desteklenmeyen IKE ilkeleri vardır. | Evet|
| WfpParse Hatası | WFP günlüğünü ayrışdıran bir hata oluştu. |Evet|

## <a name="supported-gateway-types"></a>Desteklenen Ağ Geçidi türleri

Ağ İzleyicisorun giderme ile ağ geçitleri ve bağlantılarıdesteklenen aşağıdaki tablo listeler:

|  |  |
|---------|---------|
|**Ağ geçidi türleri**   |         |
|VPN      | Destekleniyor        |
|ExpressRoute | Desteklenmiyor |
|**VPN türleri** | |
|Rota Tabanlı | Destekleniyor|
|İlke Tabanlı | Desteklenmiyor|
|**Bağlantı türleri**||
|IPSec| Destekleniyor|
|VNet2Vnet| Destekleniyor|
|ExpressRoute| Desteklenmiyor|
|VPNClient| Desteklenmiyor|

## <a name="log-files"></a>Günlük dosyaları

Kaynak sorun giderme günlüğü dosyaları, kaynak sorun giderme tamamlandıktan sonra bir depolama hesabında depolanır. Aşağıdaki resimde, bir hatayla sonuçlanan bir çağrının örnek içeriği gösterilmektedir.

![zip dosyası][1]

> [!NOTE]
> Bazı durumlarda, günlük dosyalarının yalnızca bir alt kümesi depolama alanına yazılır.

Azure depolama hesaplarından dosya indirme yle ilgili talimatlar için [,.NET'i kullanarak Azure Blob depolama alanına başlayın'a](../storage/blobs/storage-dotnet-how-to-use-blobs.md)bakın. Kullanılabilecek bir diğer araç da Depolama Gezgini'dir. Depolama Gezgini hakkında daha fazla bilgiyi aşağıdaki bağlantıda bulabilirsiniz: [Depolama Gezgini](https://storageexplorer.com/)

### <a name="connectionstatstxt"></a>BağlantıStats.txt

**ConnectionStats.txt** dosyası, giriş ve çıkış baytları, Bağlantı durumu ve Bağlantının kurulduğu saat dahil olmak üzere Bağlantının genel istatistiklerini içerir.

> [!NOTE]
> Sorun giderme API'sine yapılan çağrı sağlıklı döndürürse, zip dosyasında döndürülen tek şey **ConnectionStats.txt** dosyasıdır.

Bu dosyanın içeriği aşağıdaki örneğe benzer:

```
Connectivity State : Connected
Remote Tunnel Endpoint :
Ingress Bytes (since last connected) : 288 B
Egress Bytes (Since last connected) : 288 B
Connected Since : 2/1/2017 8:22:06 PM
```

### <a name="cpustatstxt"></a>CPUStats.txt

**CPUStats.txt** dosyası, sınama sırasında kullanılabilir CPU kullanımı ve bellek içerir.  Bu dosyanın içeriği aşağıdaki örneğe benzer:

```
Current CPU Usage : 0 % Current Memory Available : 641 MBs
```

### <a name="ikeerrorstxt"></a>IKEErrors.txt

**IKEErrors.txt** dosyası izleme sırasında bulunan tüm IKE hatalarını içerir.

Aşağıdaki örnekte, bir IKEErrors.txt dosyasının içeriği gösterilmektedir. Sorununa bağlı olarak hatalarınız farklı olabilir.

```
Error: Authentication failed. Check shared key. Check crypto. Check lifetimes. 
     based on log : Peer failed with Windows error 13801(ERROR_IPSEC_IKE_AUTH_FAIL)
Error: On-prem device sent invalid payload. 
     based on log : IkeFindPayloadInPacket failed with Windows error 13843(ERROR_IPSEC_IKE_INVALID_PAYLOAD)
```

### <a name="scrubbed-wfpdiagtxt"></a>Ovma-wfpdiag.txt

**Scrubbed-wfpdiag.txt** günlük dosyası wfp günlüğü içerir. Bu günlük, paket bırakma ve IKE/AuthIP hatalarının günlüğe kaydedilmesini içerir.

Aşağıdaki örnek, Scrubbed-wfpdiag.txt dosyasının içeriğini gösterir. Bu örnekte, bir Bağlantının paylaşılan anahtarı alttan üçüncü satırda görülebileceği gibi doğru değildi. Aşağıdaki örnek, günlük soruna bağlı olarak uzun olabilir gibi, tüm günlük sadece bir bölümüdür.

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

### <a name="wfpdiagtxtsum"></a>wfpdiag.txt.sum

**wfpdiag.txt.sum** dosyası, arabellekleri ve işlenen olayları gösteren bir günlük.

Aşağıdaki örnek bir wfpdiag.txt.sum dosyasının içeriğidir.
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

Ağ geçidi veya ağ geçidi bağlantısıyla ilgili bir sorunu nasıl tanılayabileceğini öğrenmek için [bkz.](diagnose-communication-problem-between-networks.md)
<!--Image references-->

[1]: ./media/network-watcher-troubleshoot-overview/GatewayTenantWorkerLogs.png
[2]: ./media/network-watcher-troubleshoot-overview/portal.png
