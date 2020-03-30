---
title: Azure'da Linux VM'leri için Zamanlanan Etkinlikler
description: Linux sanal makineleriniz için Azure Meta veri hizmetini kullanarak olayları zamanlayın.
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: ''
author: ericrad
manager: gwallace
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: ericrad
ms.openlocfilehash: dbea68f5699f26b866d2e22c960c0359bcb3479b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267202"
---
# <a name="azure-metadata-service-scheduled-events-for-linux-vms"></a>Azure Meta veri hizmeti: Linux VM'leri için Zamanlanmış Etkinlikler

Zamanlanmış Etkinlikler, uygulamanıza sanal makine (VM) bakımına hazırlanmanız için zaman veren bir Azure Meta Veri Hizmetidir. Uygulamanızın bunlara hazırlanması ve kesintiyi sınırlayabilmesi için yaklaşan bakım olayları (örneğin, yeniden başlatma) hakkında bilgi sağlar. Hem Windows'da hem de Linux'ta PaaS ve IaaS dahil olmak üzere tüm Azure Sanal Makineler türleri için kullanılabilir. 

Windows'da Zamanlanan Olaylar hakkında daha fazla bilgi için [Windows VM'ler için Zamanlanmış Etkinlikler'e](../windows/scheduled-events.md)bakın.

> [!Note] 
> Zamanlanmış Etkinlikler genellikle tüm Azure Bölgelerinde kullanılabilir. En son sürüm bilgileri için [Sürüm ve Bölge Kullanılabilirliği'ne](#version-and-region-availability) bakın.

## <a name="why-use-scheduled-events"></a>Neden Zamanlanmış Etkinlikler kullanıyor?

Birçok uygulama VM bakım için hazırlamak için zaman yararlanabilir. Bu süre, aşağıdakiler de dahil olmak üzere kullanılabilirliği, güvenilirliği ve hizmet edebilmeyi artıran uygulamaya özgü görevleri gerçekleştirmek için kullanılabilir: 

- Kontrol noktası ve geri yükleyin.
- Bağlantı boşaltıyor.
- Birincil çoğaltma başarısız oldu.
- Yük dengeleyici havuzundan çıkarma.
- Olay günlüğü.
- Zarif kapatma.

Zamanlanan Olaylar ile, uygulamanız bakımın ne zaman gerçekleşeceğini keşfedebilir ve etkisini sınırlamak için görevleri tetikleyebilir.  

Zamanlanmış Olaylar aşağıdaki kullanım durumlarında olaylar sağlar:

- [Platform tarafından başlatılan bakım](https://docs.microsoft.com/azure/virtual-machines/linux/maintenance-and-updates) (örneğin, VM yeniden başlatma, canlı geçiş veya ana bilgisayar için güncelleştirmeleri koruma bellek)
- Sanal makine yakında başarısız olduğu tahmin edilen [bozulmuş ana bilgisayar donanımı](https://azure.microsoft.com/blog/find-out-when-your-virtual-machine-hardware-is-degraded-with-scheduled-events) üzerinde çalışıyor
- Kullanıcı tarafından başlatılan bakım (örneğin, bir kullanıcı bir VM'yi yeniden başlatır veya yeniden dağıtır)
- [Spot VM](spot-vms.md) ve Spot ölçeği örnek tahliyeleri [ayarlayın.](../../virtual-machine-scale-sets/use-spot.md)

## <a name="the-basics"></a>Temeller  

  Meta veri hizmeti, VM'den erişilebilen bir REST bitiş noktası kullanarak VM'lerin çalıştırılabildiği hakkındaki bilgileri ortaya çıkarır. Bilgiler, VM dışında açıklanmaması için rahatsız edilemeyen bir IP üzerinden kullanılabilir.

### <a name="scope"></a>Kapsam
Zamanlanan olaylar şu şekilde teslim edilir:

- Bağımsız Sanal Makineler.
- Bulut hizmetindeki tüm VM'ler.
- Kullanılabilirlik kümesindeki tüm VM'ler.
- Bir ölçek kümesi yerleşim grubundaki tüm VM'ler. 

Sonuç olarak, hangi `Resources` VM'lerin etkilendiğini belirlemek için olaydaki alanı denetleyin.

### <a name="endpoint-discovery"></a>Bitiş Noktası Bulma
VNET özellikli VM'ler için Metaveri Hizmeti, statik `169.254.169.254`bir unroutable IP'den kullanılabilir. Zamanlanan Etkinlikler'in en son sürümü için tam bitiş noktası: 

 > `http://169.254.169.254/metadata/scheduledevents?api-version=2019-01-01`

VM sanal ağ da oluşturulmazsa, bulut hizmetleri ve klasik VM'ler için varsayılan durumlar, kullanılacak IP adresini bulmak için ek mantık gerekir. Ana bilgisayar [bitiş noktasını](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm)nasıl keşfederiz öğrenmek için bu örneğe bakın.

### <a name="version-and-region-availability"></a>Sürüm ve Bölge Kullanılabilirliği
Zamanlanan Olaylar hizmeti sürülür. Sürümler zorunludur; geçerli sürümü `2019-01-01`.

| Sürüm | Yayın Türü | Bölgeler | Sürüm Notları | 
| - | - | - | - | 
| 2019-01-01 | Genel Kullanılabilirlik | Tümü | <li> Sanal makine ölçeği için eklenen destek, EventType 'Sonlandırma' kümesini ayarlar |
| 2017-11-01 | Genel Kullanılabilirlik | Tümü | <li> Spot VM tahliye EventType 'Preempt' için destek eklendi<br> | 
| 2017-08-01 | Genel Kullanılabilirlik | Tümü | <li> IaaS VM'ler için kaynak adlarından hazır alt alt alt<br><li>Tüm istekler için zorunlu meta veri üstbilgi gereksinimi | 
| 2017-03-01 | Önizleme | Tümü | <li>İlk yayın |


> [!NOTE] 
> Zamanlanmış Olaylar önceki önizleme sürümleri desteklenen {en son} api sürümü olarak. Bu biçim artık desteklenmez ve gelecekte amortismana hazır olacaktır.

### <a name="enabling-and-disabling-scheduled-events"></a>Zamanlanan Olayları Etkinleştirme ve Devre Dışı Bırakma
Etkinlikler için ilk kez istekte bulunduğunuzda, hizmetiniz için Zamanlanmış Etkinlikler etkinleştirilir. İki dakikaya kadar olan ilk aramanızda gecikmeli bir yanıt beklemelisiniz.

24 saat boyunca istekte bulunmazsa, Zamanlanmış Etkinlikler hizmetiniz için devre dışı bırakılır.

### <a name="user-initiated-maintenance"></a>Kullanıcı tarafından başlatılan Bakım
Azure portalı, API, CLI veya PowerShell üzerinden kullanıcı tarafından başlatılan VM bakımı, zamanlanmış bir olayla sonuçlanır. Daha sonra uygulamanızdaki bakım hazırlama mantığını test edebilir ve uygulamanız kullanıcı tarafından başlatılan bakım için hazırlanabilir.

Bir VM'yi yeniden başlatırsanız, `Reboot` türü olan bir olay zamanlanır. Bir VM'yi yeniden dağıtırsanız, `Redeploy` türü olan bir olay zamanlanır.

## <a name="use-the-api"></a>API’yi kullanma

### <a name="headers"></a>Üst bilgiler
Meta veri hizmetini sorgularken, isteğin `Metadata:true` istemeden yeniden yönlendirilen olmadığından emin olmak için üstbilgi sağlamanız gerekir. Üstbilgi `Metadata:true` tüm zamanlanan olaylar istekleri için gereklidir. Üstbilginin isteke eklenememesi, Meta veri hizmetinden gelen "Hatalı İstek" yanıtıyla sonuçlanır.

### <a name="query-for-events"></a>Olaylar için sorgu
Aşağıdaki aramayı yaparak zamanlanan olayları sorgulayabilirsiniz:

#### <a name="bash"></a>Bash
```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2019-01-01
```

Yanıt, zamanlanmış olaylar dizisi içerir. Boş bir dizi, şu anda hiçbir olayın zamanlanmadığı anlamına gelir.
Zamanlanan olayların olduğu durumlarda, yanıt bir dizi olay içerir. 
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

### <a name="event-properties"></a>Etkinlik Özellikleri
|Özellik  |  Açıklama |
| - | - |
| Eventıd | Bu etkinlik için genel olarak benzersiz tanımlayıcı. <br><br> Örnek: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| Olay türü | Etkisi bu olay neden olur. <br><br> Değerler: <br><ul><li> `Freeze`: Sanal Makine birkaç saniye duraklatmak için zamanlanır. CPU ve ağ bağlantısı askıya alınabilir, ancak bellek veya açık dosyalar üzerinde hiçbir etkisi yoktur.<li>`Reboot`: Sanal Makine yeniden başlatma için zamanlanır (kalıcı olmayan bellek kaybolur). <li>`Redeploy`: Sanal Makine başka bir düğüme geçmek için zamanlanır (kısa ömürlü diskler kaybolur). <li>`Preempt`: Spot Sanal Makine silinmektedir (kısa ömürlü diskler kaybolur). <li> `Terminate`: Sanal makine silinecek şekilde zamanlanır. |
| ResourceType | Bu olayın etkilediği kaynak türü. <br><br> Değerler: <ul><li>`VirtualMachine`|
| Kaynaklar| Bu olayın etkilediği kaynakların listesi. Liste, en fazla bir güncelleştirme etki [alanından](manage-availability.md)makineleri içerecek şekilde garanti edilir, ancak UD'deki tüm makineleri içermeyebilir. <br><br> Örnek: <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| EventStatus | Bu olayın durumu. <br><br> Değerler: <ul><li>`Scheduled`: Bu `NotBefore` olay, özellikte belirtilen süreden sonra başlayacak şekilde zamanlanır.<li>`Started`: Bu olay başladı.</ul> Hiçbir `Completed` veya benzer durum hiç sağlanmaz. Olay tamamlandığında olay artık döndürülür.
| NotBefore| Bu olayın başlatılabildiği zaman. <br><br> Örnek: <br><ul><li> Pzt, 19 Eyl 2016 18:29:47 GMT  |

### <a name="event-scheduling"></a>Olay Zamanlama
Her olay, olay türüne bağlı olarak gelecekte en az bir süre zamanlanır. Bu süre bir etkinliğin `NotBefore` özelliğine yansıtılır. 

|Olay türü  | Minimum bildirim |
| - | - |
| Dondurmak| 15 dakika |
| Yeniden başlatma | 15 dakika |
| Yeniden dağıtım | 10 dakika |
| Preempt | 30 saniye |
| Terminate | [Kullanıcı Yapılandırılabilir](../../virtual-machine-scale-sets/virtual-machine-scale-sets-terminate-notification.md#enable-terminate-notifications): 5 ila 15 dakika |

> [!NOTE] 
> Bazı durumlarda, Azure bozulmuş donanım nedeniyle ana bilgisayar hatasını tahmin edebilir ve bir geçiş zamanlayarak hizmetinizdeki kesintiyi azaltmaya çalışır. Etkilenen sanal makineler genellikle gelecekte birkaç `NotBefore` gün olan bir zamanlanan olay alırsınız. Gerçek süre, öngörülen başarısızlık risk değerlendirmesine bağlı olarak değişir. Azure mümkün olduğunda 7 gün önceden bildirimde bulunmayı dener, ancak gerçek süre değişir ve tahmin donanımın yakında başarısız olma olasılığı yüksekse daha küçük olabilir. Donanımın sistem tarafından başlatılan geçişten önce arızalamaması durumunda hizmetinizdeki riski en aza indirmek için, sanal makinenizi mümkün olan en kısa sürede yeniden dağıtmanızı öneririz.

### <a name="start-an-event"></a>Etkinlik başlatma 

Yaklaşan bir olayı öğrendikten ve zarif kapatma mantığını bitirdikten sonra, Meta `POST` veri servisini '' ile `EventId`bir arama yaparak olağanüstü etkinliği onaylayabilirsiniz. Bu çağrı Azure'a minimum bildirim süresini kısaltabileceğini gösterir (mümkün olduğunda). 

Aşağıdaki JSON örneği istek `POST` gövdesinde bekleniyor. İstek bir listesini `StartRequests`içermelidir. Her `StartRequest` `EventId` biri hızlandırmak istediğiniz olay için şunları içerir:
```
{
    "StartRequests" : [
        {
            "EventId": {EventId}
        }
    ]
}
```

#### <a name="bash-sample"></a>Bash örneği
```
curl -H Metadata:true -X POST -d '{"StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2019-01-01
```

> [!NOTE] 
> Bir olayı kabul etmek, etkinliğin `Resources` yalnızca olayı kabul eden VM'yi değil, etkinlikteki herkes için devam etmesine olanak tanır. Bu nedenle, `Resources` bu alanda ilk makine kadar basit olabilir onay, koordine etmek için bir lider seçebilirsiniz.

## <a name="python-sample"></a>Python örneği 

Aşağıdaki örnek, zamanlanan olaylar için Meta Veri Hizmeti sorgular ve her bekleyen olay onaylar:

```python
#!/usr/bin/python

import json
import socket
import urllib2

metadata_url = "http://169.254.169.254/metadata/scheduledevents?api-version=2019-01-01"
this_host = socket.gethostname()


def get_scheduled_events():
    req = urllib2.Request(metadata_url)
    req.add_header('Metadata', 'true')
    resp = urllib2.urlopen(req)
    data = json.loads(resp.read())
    return data


def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid = evt['EventId']
        status = evt['EventStatus']
        resources = evt['Resources']
        eventtype = evt['EventType']
        resourcetype = evt['ResourceType']
        notbefore = evt['NotBefore'].replace(" ", "_")
        if this_host in resources:
            print("+ Scheduled Event. This host " + this_host +
                " is scheduled for " + eventtype + " not before " + notbefore)
            # Add logic for handling events here


def main():
    data = get_scheduled_events()
    handle_scheduled_events(data)


if __name__ == '__main__':
    main()
```

## <a name="next-steps"></a>Sonraki adımlar 
- Bir demo görmek için [Azure Cuma günü Zamanlanan Etkinlikleri](https://channel9.msdn.com/Shows/Azure-Friday/Using-Azure-Scheduled-Events-to-Prepare-for-VM-Maintenance) izleyin. 
- Azure Örneği Meta veri [Zamanlanmış Olaylar GitHub deposunda](https://github.com/Azure-Samples/virtual-machines-scheduled-events-discover-endpoint-for-non-vnet-vm)Zamanlanmış Olaylar kod örneklerini gözden geçirin.
- [Örnek Meta veri hizmetinde](instance-metadata-service.md)bulunan API'ler hakkında daha fazla bilgi edinin.
- [Azure'da Linux sanal makineleri için planlanan bakım](planned-maintenance.md)hakkında bilgi edinin.
