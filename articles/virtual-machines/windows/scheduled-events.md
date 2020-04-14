---
title: Azure'da Windows VM'leri için Zamanlanan Etkinlikler
description: Windows sanal makinelerinizde Azure Meta veri hizmetini kullanarak zamanlanan olaylar.
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: ''
author: mimckitt
manager: gwallace
editor: ''
tags: ''
ms.assetid: 28d8e1f2-8e61-4fbe-bfe8-80a68443baba
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: mimckitt
ms.openlocfilehash: c1e9ef8de65912c4f33e17ee2bb2175c76e7ea07
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81258699"
---
# <a name="azure-metadata-service-scheduled-events-for-windows-vms"></a>Azure Meta veri Hizmeti: Windows SANAL'ları için Zamanlanmış Etkinlikler

Zamanlanmış Etkinlikler, uygulamanıza sanal makine bakımına hazırlanmanız için zaman veren bir Azure Meta Veri Hizmetidir. Uygulamanızın bunlara hazırlanabilmesi ve kesintiyi sınırlayabilmeleri için yaklaşan bakım olayları (örn. yeniden başlatma) hakkında bilgi sağlar. Hem Windows'da hem de Linux'ta PaaS ve IaaS dahil olmak üzere tüm Azure Sanal Makine türleri için kullanılabilir. 

Linux'ta Zamanlanan Etkinlikler hakkında daha fazla bilgi için [Linux VM'leri için Zamanlanmış Etkinlikler'e](../linux/scheduled-events.md)bakın.

> [!Note] 
> Zamanlanmış Etkinlikler genellikle tüm Azure Bölgelerinde kullanılabilir. En son sürüm bilgileri için [Sürüm ve Bölge Kullanılabilirliği'ne](#version-and-region-availability) bakın.

## <a name="why-scheduled-events"></a>Neden Planlanmış Olaylar?

Birçok uygulama sanal makine bakımı için hazırlamak için zaman yararlanabilir. Bu süre, aşağıdakiler de dahil olmak üzere kullanılabilirliği, güvenilirliği ve hizmet edebilmeyi artıran uygulamaya özgü görevleri gerçekleştirmek için kullanılabilir: 

- Kontrol noktası ve geri yükleme
- Bağlantı boşaltma
- Birincil yineleme failover 
- Yük dengeleyici havuzundan kaldırma
- Olay günlüğü
- Zarif kapatma 

Zamanlanmış Olaylar'ı kullanarak uygulamanız bakımın ne zaman gerçekleşeceğini keşfedebilir ve etkisini sınırlamak için görevleri tetikleyebilir. Zamanlanan olayları etkinleştirmek, sanal makinenize bakım etkinliği gerçekleştirilmeden önce en az süre verir. Ayrıntılar için aşağıdaki Olay Zamanlama bölümüne bakın.

Zamanlanmış Olaylar aşağıdaki kullanım durumlarında olaylar sağlar:
- [Platform tarafından başlatılan bakım](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates) (örneğin, VM yeniden başlatma, canlı geçiş veya ana bilgisayar için güncelleştirmeleri koruma bellek)
- Sanal makine yakında başarısız olduğu tahmin edilen [bozulmuş ana bilgisayar donanımı](https://azure.microsoft.com/blog/find-out-when-your-virtual-machine-hardware-is-degraded-with-scheduled-events) üzerinde çalışıyor
- Kullanıcı tarafından başlatılan bakım (örn. kullanıcı bir VM'yi yeniden başlatır veya yeniden dağıtır)
- [Spot VM](spot-vms.md) ve [Spot ölçek seti](../../virtual-machine-scale-sets/use-spot.md) örnek tahliyeleri

## <a name="the-basics"></a>Temeller  

Azure Meta veri hizmeti, Sanal Makineler'in VM içinden erişilebilen bir REST Endpoint kullanarak sanal makineleri çalıştırmayla ilgili bilgileri ortaya çıkarır. Bilgiler, VM dışında açığa çıkmaması için routable olmayan bir IP üzerinden kullanılabilir.

### <a name="endpoint-discovery"></a>Bitiş Noktası Bulma
VNET özellikli VM'ler için meta veri hizmeti, durağan `169.254.169.254`olmayan bir IP'den kullanılabilir. Zamanlanan Etkinlikler'in en son sürümü için tam bitiş noktası: 

 > `http://169.254.169.254/metadata/scheduledevents?api-version=2019-01-01`

Sanal Makine sanal ağ da oluşturulmazsa, bulut hizmetleri ve klasik Sanal M'ler için varsayılan durumlar, kullanılacak IP adresini bulmak için ek mantık gerekir. [Ana bilgisayar bitiş noktasını](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm)nasıl keşfederiz öğrenmek için bu örneğe bakın.

### <a name="version-and-region-availability"></a>Sürüm ve Bölge Kullanılabilirliği
Zamanlanan Olaylar Hizmeti sürülür. Sürümler zorunludur ve `2019-01-01`geçerli sürüm .

| Sürüm | Yayın Türü | Bölgeler | Sürüm Notları | 
| - | - | - | - |
| 2019-01-01 | Genel Kullanılabilirlik | Tümü | <li> Sanal makine ölçeği için eklenen destek, EventType 'Sonlandırma' kümesini ayarlar |
| 2017-11-01 | Genel Kullanılabilirlik | Tümü | <li> Spot VM tahliye EventType 'Preempt' için destek eklendi<br> | 
| 2017-08-01 | Genel Kullanılabilirlik | Tümü | <li> IaaS VM'ler için kaynak adlarından hazır alt alt alt<br><li>Tüm istekler için zorunlu meta veri üstbilgisi gereksinimi | 
| 2017-03-01 | Önizleme | Tümü |<li>İlk yayın |

> [!NOTE] 
> Api sürümü olarak {en son} desteklenen zamanlanmış olayların önceki önizleme sürümleri. Bu biçim artık desteklenmez ve gelecekte amortismana hazır olacaktır.

### <a name="enabling-and-disabling-scheduled-events"></a>Zamanlanan Olayları Etkinleştirme ve Devre Dışı Bırakma
Etkinlikler için ilk kez istekte bulunduğunuzda, hizmetiniz için Zamanlanmış Etkinlikler etkinleştirilir. İki dakikaya kadar olan ilk aramanızda gecikmeli bir yanıt beklemelisiniz. Yaklaşan bakım olaylarını ve gerçekleştirilen bakım etkinliklerinin durumunu algılamak için bitiş noktasını düzenli aralıklarla sorgulamanız gerekir.

24 saat boyunca istekte bulunmazsa, Zamanlanmış Etkinlikler hizmetiniz için devre dışı bırakılır.

### <a name="user-initiated-maintenance"></a>Kullanıcı Başlatılan Bakım
Kullanıcı, Azure portalı, API, CLI veya PowerShell üzerinden sanal makine bakımını zamanlanmış bir olayla sonuçlandı. Bu, uygulamanızdaki bakım hazırlama mantığını test etmenizi ve uygulamanızın kullanıcı tarafından başlatılan bakıma hazırlanmasına olanak tanır.

Sanal makineyi yeniden başlatmak türü `Reboot`olan bir olayı zamanlar. Sanal makinenin yeniden dağıtılması, türü `Redeploy`olan bir olayı zamanlamayı.

## <a name="using-the-api"></a>API'yi kullanma

### <a name="headers"></a>Üst bilgiler
Meta veri hizmetini sorgularken, isteğin istemeden yeniden yönlendirilen olmadığından emin olmak için üstbilgi `Metadata:true` sağlamanız gerekir. Üstbilgi `Metadata:true` tüm zamanlanan olaylar istekleri için gereklidir. Üstbilginin isteğe eklenmemesi, Meta veri hizmetinden gelen Kötü İstek yanıtıyla sonuçlanır.

### <a name="query-for-events"></a>Olaylar için sorgu
Zamanlanan Olaylar için yalnızca aşağıdaki aramayı yaparak sorgulayabilirsiniz:

#### <a name="powershell"></a>PowerShell
```
curl http://169.254.169.254/metadata/scheduledevents?api-version=2019-01-01 -H @{"Metadata"="true"}
```

Yanıt, zamanlanmış olaylar dizisi içerir. Boş bir dizi, şu anda zamanlanan olay olmadığı anlamına gelir.
Zamanlanan olayların olduğu durumlarda, yanıt bir dizi olay içerir: 
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Reboot" | "Redeploy" | "Freeze" | "Preempt" | "Terminate",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled" | "Started",
            "NotBefore": {timeInUTC},
        }
    ]
}
```
DocumentIncarnation bir ETag ve Olaylar yük son sorgudan bu yana değişti olup olmadığını incelemek için kolay bir yol sağlar.

### <a name="event-properties"></a>Etkinlik Özellikleri
|Özellik  |  Açıklama |
| - | - |
| Eventıd | Bu etkinlik için genel olarak benzersiz tanımlayıcı. <br><br> Örnek: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| Olay türü | Etkisi bu olay neden olur. <br><br> Değerler: <br><ul><li> `Freeze`: Sanal Makine birkaç saniye duraklatmak için zamanlanır. CPU ve ağ bağlantısı askıya alınabilir, ancak bellek veya açık dosyalar üzerinde hiçbir etkisi yoktur. <li>`Reboot`: Sanal Makine yeniden başlatma için zamanlanır (kalıcı olmayan bellek kaybolur). <li>`Redeploy`: Sanal Makine başka bir düğüme geçmek için zamanlanır (kısa ömürlü diskler kaybolur). <li>`Preempt`: Spot Sanal Makine silinmektedir (kısa ömürlü diskler kaybolur). <li> `Terminate`: Sanal Makine silinecek şekilde zamanlanır. |
| ResourceType | Kaynak türü bu olay etkiler. <br><br> Değerler: <ul><li>`VirtualMachine`|
| Kaynaklar| Bu olayın etkilediği kaynakların listesi. Bu, en fazla bir [Güncelleştirme Etki Alanı'ndan](manage-availability.md)makineler içerdiği garanti edilir, ancak UD'deki tüm makineleri içermeyebilir. <br><br> Örnek: <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| Olay Durumu | Bu olayın durumu. <br><br> Değerler: <ul><li>`Scheduled`: Bu `NotBefore` olay, özellikte belirtilen süreden sonra başlayacak şekilde zamanlanır.<li>`Started`: Bu olay başladı.</ul> Hiçbir `Completed` veya benzer durum hiç sağlanmaz; olay tamamlandığında olay artık döndürülmeyecektir.
| NotBefore| Bu olayın başladığı zaman. <br><br> Örnek: <br><ul><li> Pzt, 19 Eyl 2016 18:29:47 GMT  |

### <a name="event-scheduling"></a>Olay Zamanlama
Her olay, olay türüne bağlı olarak gelecekte en az bir süre zamanlanır. Bu süre bir etkinliğin `NotBefore` özelliğine yansıtılır. 

|Olay türü  | Minimum Bildirim |
| - | - |
| Dondurmak| 15 dakika |
| Yeniden başlatma | 15 dakika |
| Yeniden dağıtım | 10 dakika |
| Preempt | 30 saniye |
| Terminate | [Kullanıcı Yapılandırılabilir](../../virtual-machine-scale-sets/virtual-machine-scale-sets-terminate-notification.md#enable-terminate-notifications): 5 ila 15 dakika |

> [!NOTE] 
> Bazı durumlarda, Azure bozulmuş donanım nedeniyle ana bilgisayar hatasını tahmin edebilir ve bir geçiş zamanlayarak hizmetinizdeki kesintiyi azaltmaya çalışır. Etkilenen sanal makineler genellikle gelecekte birkaç `NotBefore` gün olan bir zamanlanan olay alırsınız. Gerçek süre, öngörülen başarısızlık risk değerlendirmesine bağlı olarak değişir. Azure mümkün olduğunda 7 gün önceden bildirimde bulunmayı dener, ancak gerçek süre değişir ve tahmin donanımın yakında başarısız olma olasılığı yüksekse daha küçük olabilir. Donanımın sistem geçiş başlatmadan önce arızalamaması durumunda hizmetinizdeki riski en aza indirmek için, sanal makinenizi mümkün olan en kısa sürede yeniden dağıtmanız önerilir.

### <a name="event-scope"></a>Etkinlik Kapsamı     
Zamanlanan olaylar şu şekilde teslim edilir:
 - Bağımsız Sanal Makineler
 - Bulut Hizmetindeki Tüm Sanal Makineler      
 - Kullanılabilirlik Kümesindeki Tüm Sanal Makineler      
 - Bir Ölçek Seti Yerleştirme Grubundaki Tüm Sanal Makineler.         

Sonuç olarak, hangi VM'lerin etkileneceğini belirlemek için olaydaki `Resources` alanı kontrol etmelisiniz. 

### <a name="starting-an-event"></a>Bir etkinliği başlatma 

Yaklaşan bir olayı öğrendikten ve zarif kapatma mantığınız tamamlandıktan sonra, meta `POST` veri hizmetine bir çağrı `EventId`yaparak olağanüstü olayı onaylayabilirsiniz. Bu, Azure'a minimum bildirim süresini kısaltabileceğini gösterir (mümkün olduğunda). 

Aşağıdaki json `POST` istek gövdesinde bekleniyor. İstek bir listesini `StartRequests`içermelidir. Her `StartRequest` biri `EventId` hızlandırmak istediğiniz olay için şunları içerir:
```
{
    "StartRequests" : [
        {
            "EventId": {EventId}
        }
    ]
}
```

#### <a name="powershell"></a>PowerShell
```
curl -H @{"Metadata"="true"} -Method POST -Body '{"StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' -Uri http://169.254.169.254/metadata/scheduledevents?api-version=2019-01-01
```

> [!NOTE] 
> Bir olayı kabul etmek, etkinliğin `Resources` yalnızca olayı kabul eden sanal makineiçin değil, etkinlikte ki herkes için devam etmesine olanak tanır. Bu nedenle, `Resources` bu alandaki ilk makine kadar basit olabilecek kabulü koordine etmek için bir lider seçmeyi seçebilirsiniz.


## <a name="powershell-sample"></a>PowerShell örneği 

Aşağıdaki örnek, zamanlanan olaylar için meta veri hizmetini sorgular ve her bekleyen olayı onaylar.

```powershell
# How to get scheduled events 
function Get-ScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How to approve a scheduled event
function Approve-ScheduledEvent($eventId, $uri)
{
    # Create the Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests} 
    
    # Convert to JSON string
    $approvalString = ConvertTo-Json $scheduledEventsApproval

    Write-Host "Approving with the following: `n" $approvalString

    # Post approval string to scheduled events endpoint
    Invoke-RestMethod -Uri $uri -Headers @{"Metadata"="true"} -Method POST -Body $approvalString
}

function Handle-ScheduledEvents($scheduledEvents)
{
    # Add logic for handling events here
}

######### Sample Scheduled Events Interaction #########

# Set up the scheduled events URI for a VNET-enabled VM
$localHostIP = "169.254.169.254"
$scheduledEventURI = 'http://{0}/metadata/scheduledevents?api-version=2019-01-01' -f $localHostIP 

# Get events
$scheduledEvents = Get-ScheduledEvents $scheduledEventURI

# Handle events however is best for your service
Handle-ScheduledEvents $scheduledEvents

# Approve events when ready (optional)
foreach($event in $scheduledEvents.Events)
{
    Write-Host "Current Event: `n" $event
    $entry = Read-Host "`nApprove event? Y/N"
    if($entry -eq "Y" -or $entry -eq "y")
    {
        Approve-ScheduledEvent $event.EventId $scheduledEventURI 
    }
}
``` 

## <a name="next-steps"></a>Sonraki adımlar 

- Azure Cuma günü [Zamanlanmış Etkinlikler](https://channel9.msdn.com/Shows/Azure-Friday/Using-Azure-Scheduled-Events-to-Prepare-for-VM-Maintenance) Demosu'na göz atın. 
- Azure Örneği Meta veri [Zamanlanmış Olaylar GitHub Deposu'ndaki](https://github.com/Azure-Samples/virtual-machines-scheduled-events-discover-endpoint-for-non-vnet-vm) Zamanlanmış Olaylar kod örneklerini gözden geçirin
- [Örnek Meta veri hizmetinde](instance-metadata-service.md)bulunan API'ler hakkında daha fazla bilgi edinin.
- [Azure'daki Windows sanal makineleri için planlanan bakım](planned-maintenance.md)hakkında bilgi edinin.
