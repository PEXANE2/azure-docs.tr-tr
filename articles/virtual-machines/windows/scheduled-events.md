---
title: Azure 'da Windows VM 'Leri için Zamanlanan Olaylar
description: Windows sanal makinelerinizdeki için Azure meta veri hizmetini kullanan Zamanlanmış olaylar.
author: mimckitt
ms.service: virtual-machines-windows
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: mimckitt
ms.openlocfilehash: 105279940546c8e5b40d1d8378b35f85af1ea98b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82099555"
---
# <a name="azure-metadata-service-scheduled-events-for-windows-vms"></a>Azure Metadata Service: Windows VM 'Leri için Zamanlanan Olaylar

Zamanlanan Olaylar, sanal makine bakımına hazırlanmak için uygulamanızın süresini sağlayan bir Azure Metadata Service. Bu, yaklaşan bakım olayları (örneğin, yeniden başlatma) hakkında bilgi sağlar ve böylece uygulamanızın onlara hazırlanabilmesi ve kesintiye uğramasını sınırlayabilirsiniz. Hem Windows hem de Linux 'ta PaaS ve IaaS dahil tüm Azure sanal makine türlerinde kullanılabilir. 

Linux üzerinde Zamanlanan Olaylar hakkında daha fazla bilgi için bkz. [Linux VM 'leri için zamanlanan olaylar](../linux/scheduled-events.md).

> [!Note] 
> Zamanlanan Olaylar, tüm Azure bölgelerinde genel kullanıma sunulmuştur. En son sürüm bilgileri için [Sürüm ve bölge kullanılabilirliğine](#version-and-region-availability) bakın.

## <a name="why-scheduled-events"></a>Neden Zamanlanan Olaylar?

Birçok uygulama, sanal makine bakımı için hazırlanma zamanından faydalanabilir. Bu süre, kullanılabilirliği, güvenilirliği ve bakım için aşağıdakiler dahil olmak üzere uygulama özel görevleri gerçekleştirmek için kullanılabilir: 

- Denetim noktası ve geri yükleme
- Bağlantı boşaltma
- Birincil çoğaltma yük devretmesi 
- Yük dengeleyici havuzundan kaldırma
- Olay günlüğü
- Düzgün kapanma 

Uygulamanızın Zamanlanan Olaylar kullanımı, bakımın ne zaman gerçekleşeceğini bulabilir ve etkilerini sınırlamak için görevleri tetikler. Zamanlanan olayları etkinleştirmek, bakım etkinliği gerçekleştirilmeden önce sanal makinenize en az bir süre sağlar. Ayrıntılar için aşağıdaki olay zamanlaması bölümüne bakın.

Zamanlanan Olaylar aşağıdaki kullanım durumlarında Olaylar sağlar:
- [Platform tarafından başlatılan bakım](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates) (ÖRNEĞIN, VM yeniden başlatma, dinamik geçiş veya konak için güncelleştirmeleri koruma)
- Sanal makine, yakında başarısız olarak tahmin edilen [düşürülmüş konak donanımında](https://azure.microsoft.com/blog/find-out-when-your-virtual-machine-hardware-is-degraded-with-scheduled-events) çalışıyor
- Kullanıcı tarafından başlatılan bakım (ör. Kullanıcı bir VM 'yi yeniden başlatır veya yeniden dağıtır)
- [Spot VM](spot-vms.md) ve [spot ölçek kümesi](../../virtual-machine-scale-sets/use-spot.md) örnek çıkarmaları

## <a name="the-basics"></a>Temeller  

Azure meta veri hizmeti, sanal makineleri VM içinden erişilebilen bir REST uç noktası kullanarak çalıştırmaya yönelik bilgiler sunar. Bilgiler, sanal makinenin dışında kullanıma sunulmaması için yönlendirilemeyen bir IP aracılığıyla kullanılabilir.

### <a name="endpoint-discovery"></a>Uç nokta bulma
SANAL ağ etkin VM 'Ler için, meta veri hizmeti statik bir yönlendirilemeyen IP 'den kullanılabilir `169.254.169.254`. Zamanlanan Olaylar en son sürümü için tam uç nokta şunlardır: 

 > `http://169.254.169.254/metadata/scheduledevents?api-version=2019-01-01`

Sanal makine bir sanal ağ içinde oluşturulmadıysa, bulut hizmetleri ve klasik VM 'Ler için varsayılan durumlar olarak, kullanılacak IP adresini bulması için ek mantık gerekir. [Konak uç noktasını bulmayı](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm)öğrenmek için bu örneğe bakın.

### <a name="version-and-region-availability"></a>Sürüm ve bölge kullanılabilirliği
Zamanlanan Olaylar hizmeti sürümlenmiş. Sürümler zorunludur ve geçerli sürümdür `2019-01-01`.

| Sürüm | Yayın türü | Bölgeler | Sürüm Notları | 
| - | - | - | - |
| 2019-01-01 | Genel kullanılabilirlik | Tümü | <li> Sanal Makine Ölçek Kümeleri için destek eklendi EventType ' Terminate ' |
| 2017-11-01 | Genel kullanılabilirlik | Tümü | <li> Nokta VM çıkarma olay türü ' preempt ' için destek eklendi<br> | 
| 2017-08-01 | Genel kullanılabilirlik | Tümü | <li> IaaS VM 'lerinin kaynak adlarından eklenmiş alt çizgi kaldırıldı<br><li>Tüm istekler için meta veri üst bilgisi gereksinimi zorlandı | 
| 2017-03-01 | Önizleme | Tümü |<li>İlk yayın |

> [!NOTE] 
> Zamanlanan olayların önceki önizleme sürümleri, api sürümü olarak {latest} desteklenir. Bu biçim artık desteklenmemektedir ve gelecekte kullanım dışı bırakılacak.

### <a name="enabling-and-disabling-scheduled-events"></a>Zamanlanan Olayları Etkinleştirme ve Devre Dışı Bırakma
Zamanlanan Olaylar, ilk kez olay isteği yaptığınızda hizmetiniz için etkinleştirilir. En fazla iki dakikalık ilk çağrındaki gecikmeli bir yanıt beklemelisiniz. Yaklaşan bakım olaylarını ve gerçekleştirilen bakım etkinliklerinin durumunu tespit etmek için uç noktayı düzenli aralıklarla sorgulayın.

24 saat boyunca bir istek yapmadığından hizmetiniz için Zamanlanan Olaylar devre dışı bırakılır.

### <a name="user-initiated-maintenance"></a>Kullanıcı tarafından başlatılan bakım
Azure portal, API, CLı veya PowerShell aracılığıyla Kullanıcı tarafından başlatılan sanal makine bakımı zamanlanmış bir olayda sonuçlanıyor. Bu, uygulamanızda bakım hazırlama mantığını test etmenizi ve uygulamanızın kullanıcı tarafından başlatılan bakım için hazırlanmasına olanak tanır.

Bir sanal makineyi yeniden başlatmak, türünde `Reboot`bir olay zamanlar. Bir sanal makineyi yeniden dağıtmak, türünde `Redeploy`bir olay zamanlar.

## <a name="using-the-api"></a>API 'YI kullanma

### <a name="headers"></a>Üst bilgiler
Metadata Service sorgulayıp, isteğin istem dışı olarak yeniden yönlendirilmediğinden `Metadata:true` emin olmak için üstbilgiyi sağlamanız gerekir. `Metadata:true` Üst bilgi tüm zamanlanmış olaylar istekleri için gereklidir. Üst bilgiyi istek içine dahil etme hatası, Metadata Service hatalı Istek yanıtı oluşmasına neden olur.

### <a name="query-for-events"></a>Olayları sorgula
Aşağıdaki çağrıyı yaparak Zamanlanan Olaylar için sorgulama yapabilirsiniz:

#### <a name="powershell"></a>PowerShell
```
curl http://169.254.169.254/metadata/scheduledevents?api-version=2019-01-01 -H @{"Metadata"="true"}
```

Bir yanıt, Zamanlanmış olaylar dizisi içerir. Boş bir dizi, şu anda zamanlanan hiçbir olay olmadığı anlamına gelir.
Zamanlanan olayların bulunduğu durumda, yanıt bir olay dizisi içerir: 
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
Documentınnation bir ETag ' dir ve olayların yükünün son sorgudan bu yana değişip değişmediğini denetlemek için kolay bir yol sağlar.

### <a name="event-properties"></a>Olay özellikleri
|Özellik  |  Açıklama |
| - | - |
| Even | Bu olay için genel benzersiz tanımlayıcı. <br><br> Örnek: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| Olay türü | Bu olay nedenlerini etkiler. <br><br> Değerler: <br><ul><li> `Freeze`: Sanal makine birkaç saniye duraklamak üzere zamanlandı. CPU ve ağ bağlantısı askıya alınabilir, ancak bellekte veya açık dosyalarda bir etkisi yoktur. <li>`Reboot`: Sanal makine yeniden başlatma için zamanlandı (kalıcı olmayan bellek kaybolur). <li>`Redeploy`: Sanal makine başka bir düğüme ilerlemek üzere zamanlandı (kısa ömürlü diskler kaybolur). <li>`Preempt`: Spot sanal makine siliniyor (kısa ömürlü diskler kaybolur). <li> `Terminate`: Sanal makine silinmek üzere zamanlandı. |
| ResourceType | Bu olayın etkilediği kaynak türü. <br><br> Değerler: <ul><li>`VirtualMachine`|
| Kaynaklar| Bu olayın etkilediği kaynakların listesi. Bu, en çok bir [güncelleştirme etki alanındaki](manage-availability.md)makineler bulundur, ancak ud 'deki tüm makineleri içermeyebilir. <br><br> Örnek: <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| Olay durumu | Bu olayın durumu. <br><br> Değerler: <ul><li>`Scheduled`: Bu olay, `NotBefore` özellikte belirtilen süreden sonra başlayacak şekilde zamanlandı.<li>`Started`: Bu olay başlatıldı.</ul> Hiç `Completed` veya benzer bir durum sağlanmaz; olay tamamlandığında olay artık döndürülmeyecektir.
| NotBefore| Bu olayın başlayabileceği zaman. <br><br> Örnek: <br><ul><li> Mon, 19 Eyl 2016 18:29:47 GMT  |

### <a name="event-scheduling"></a>Olay zamanlaması
Her olay, gelecekte olay türüne göre en az bir süre zamanlanır. Bu zaman, bir olayın `NotBefore` özelliğinde yansıtılır. 

|Olay türü  | En düşük bildirim |
| - | - |
| Amazsınız| 15 dakika |
| Yeniden başlatma | 15 dakika |
| Yeniden dağıtım | 10 dakika |
| Preempt | 30 saniye |
| Terminate | [Kullanıcı yapılandırılabilir](../../virtual-machine-scale-sets/virtual-machine-scale-sets-terminate-notification.md#enable-terminate-notifications): 5 ila 15 dakika |

> [!NOTE] 
> Azure, bazı durumlarda ana bilgisayar başarısızlığını düşürülmüş bir donanım nedeniyle tahmin edebilir ve bir geçiş zamanlayarak hizmetinize olan kesintiyi azaltmaya çalışacaktır. Etkilenen sanal makineler, genellikle gelecekte birkaç gün olan zamanlanmış `NotBefore` bir olay alır. Gerçek süre, tahmin edilen hata riski değerlendirmesine bağlı olarak farklılık gösterir. Azure, mümkün olduğunda 7 gün daha kısa bir bildirimde bulunmaya çalışır, ancak tahmin, donanım arızalanmaya yönelik yüksek bir şansınız olması durumunda gerçek zaman değişir ve daha küçük olabilir. Sistemin geçişten önce başarısız olması durumunda hizmetinize yönelik riski en aza indirmek için, sanal makinenizi mümkün olan en kısa sürede kendi kendine yeniden dağıtmanız önerilir.

### <a name="event-scope"></a>Olay kapsamı     
Zamanlanan olaylar şu şekilde dağıtılır:
 - Tek başına sanal makineler
 - Bir bulut hizmetindeki tüm sanal makineler      
 - Bir kullanılabilirlik kümesindeki tüm sanal makineler      
 - Ölçek kümesi yerleştirme grubundaki tüm sanal makineler.         

Sonuç olarak, hangi VM 'Lerin etkilenecek olduğunu `Resources` belirlemek için olaydaki alanı denetlemeniz gerekir. 

### <a name="starting-an-event"></a>Olay başlatma 

Yaklaşan bir olayı öğrendikten ve düzgün kapanma için mantığınızı tamamladıktan sonra, ile meta veri hizmetine bir `POST` çağrı yaparak bekleyen olayı onaylayabilirsiniz. `EventId` Bu, Azure 'a, en düşük bildirim süresini (mümkün olduğunda) kısaltıp kısaltamayacağını gösterir. 

`POST` İstek gövdesinde beklenen JSON aşağıda verilmiştir. İstek bir listesi içermelidir `StartRequests`. Her `StartRequest` biri, `EventId` hızlandırmak istediğiniz olay için şunu içerir:
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
> Bir olayı ele almak, olayın yalnızca olayı ele alan sanal `Resources` makineyi değil, tüm olayda devam etmesine izin verir. Bu nedenle, onay koordine etmek için bir öncü seçerek `Resources` alandaki ilk makine kadar kolay olabilir.


## <a name="powershell-sample"></a>PowerShell örneği 

Aşağıdaki örnek, Zamanlanmış olaylar için meta veri hizmetini sorgular ve her bir bekleyen olayı onaylar.

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

- Azure Cuma 'da [zamanlanan olaylar tanıtım](https://channel9.msdn.com/Shows/Azure-Friday/Using-Azure-Scheduled-Events-to-Prepare-for-VM-Maintenance) izleyin. 
- [GitHub deposunda zamanlanan olaylar Azure örnek meta verilerinde](https://github.com/Azure-Samples/virtual-machines-scheduled-events-discover-endpoint-for-non-vnet-vm) zamanlanan olaylar kod örneklerini gözden geçirin
- [Örnek meta veri hizmetinde](instance-metadata-service.md)bulunan API 'ler hakkında daha fazla bilgi edinin.
- [Azure 'Da Windows sanal makineleri için planlı bakım](planned-maintenance.md)hakkında bilgi edinin.
