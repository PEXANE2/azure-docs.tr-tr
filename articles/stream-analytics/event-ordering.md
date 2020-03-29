---
title: Azure Akış Analizi için olay sıralama ilkelerini yapılandırma
description: Bu makalede, Stream Analytics'te sıralama ayarlarını yapılandırma nın nasıl yapılacağını açıklar
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: c0a108565a6a0f62c6252113f984e8b10967c5db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75461201"
---
# <a name="configuring-event-ordering-policies-for-azure-stream-analytics"></a>Azure Akış Analizi için olay sıralama ilkelerini yapılandırma

Bu makalede, Azure Akış Analizi'nde geç varış ve sıra dışı olay ilkelerinin nasıl kurulup kullanılacağı açıklanmaktadır. Bu ilkeler yalnızca sorgunuzdaki [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics) yan tümcesini kullandığınızda uygulanır.

## <a name="event-time-and-arrival-time"></a>Etkinlik zamanı ve Varış Saati

Akış Analizi işin, olayları olay *saatine* veya *varış zamanına*göre işleyebilir. **Olay/uygulama süresi,** olay yükündeki (olay oluşturulduğunda) bulunan zaman damgasIdır. **Varış saati,** olayın giriş kaynağından (Olay Hub'ları/IoT Hub/Blob depolama alanı) alındığı zaman damgasIdır. 

Varsayılan olarak, Akış Analizi olayları *varış saatine*göre işler, ancak sorgunuzda [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics) yan tümcesini kullanarak olayları *etkinlik zamanına* göre işlemeyi seçebilirsiniz. Geç varış ve sipariş dışı politikalar yalnızca olayları etkinlik saatine göre işliyorsanız geçerlidir. Bu ayarları yapılandırırken senaryonuzun gecikme süresi ve doğruluk gereksinimlerini göz önünde bulundurun. 

## <a name="what-is-late-arrival-policy"></a>Geç varış ilkesi nedir?

Bazen olaylar çeşitli nedenlerle geç gelir. Örneğin, 40 saniye geç gelen bir olayın etkinlik süresi = 00:10:00 ve varış zamanı = 00:10:40 olacaktır. Geç varış politikasını 15 saniyeye ayarlarsanız, 15 saniyeden sonra gelen herhangi bir olay ya bırakılır (Stream Analytics tarafından işlenmez) ya da etkinlik zamanları ayarlanır. Yukarıdaki örnekte, olay 40 saniye geç geldiğinde (poliçe kümesinden daha fazla), etkinlik süresi 00:10:25 (varış saati - geç varış politikası değeri) maksimum geç varış politikasına göre ayarlanır. Varsayılan geç varış ilkesi 5 saniyedir.

## <a name="what-is-out-of-order-policy"></a>Sıra dışı politika nedir? 

Olay da bozuk gelebilir. Olay süresi geç varış politikasına göre ayarlandıktan sonra, sıra dışı olayları otomatik olarak bırakmayı veya ayarlamayı da seçebilirsiniz. Bu ilkeyi 8 saniyeolarak ayarlarsanız, sıra dışı ancak 8 saniyelik pencere içinde gelen olaylar olay saatine göre yeniden sıralanır. Daha sonra gelen olaylar bırakılır veya en yüksek sıra dışı ilke değerine ayarlanır. Varsayılan sıra dışı ilke 0 saniyedir. 

## <a name="adjust-or-drop-events"></a>Olayları ayarlama veya bırakma

Olaylar, yapılandırdığınız ilkelere bağlı olarak geç veya sıra dışı gelirse, bu tür olayları bırakabilir (Stream Analytics tarafından işlenmez) veya etkinlik sürelerini ayarlayabilirsiniz.

Bu politikaların bir örneğini iş başında görelim.
<br> **Geç varış politikası:** 15 saniye
<br> **Sipariş dışı ilke:** 8 saniye

| Olay No | Etkinlik Zamanı | Varış Saati | System.Timestamp | Açıklama |
| --- | --- | --- | --- | --- |
| **1** | 00:10:00  | 00:10:40  | 00:10:25  | Olay geç ve dış tolerans seviyesi geldi. Böylece olay süresi maksimum geç varış toleransına göre ayarlanır.  |
| **2** | 00:10:30 | 00:10:41  | 00:10:30  | Olay geç geldi ama tolerans düzeyi içinde. Yani olay süresi ayarlanmaz.  |
| **3** | 00:10:42 | 00:10:42 | 00:10:42 | Olay zamanında geldi. Ayarlama ya da ayarlama ya da ayarlama ya da ayarlama ya da ayarlamaya  |
| **4** | 00:10:38  | 00:10:43  | 00:10:38 | Olay sipariş dışı geldi ama 8 saniye tolerans içinde. Yani, olay süresi ayarlanmaz. Analitik amaçlar için, bu olay önceki olay sayısı 4 olarak kabul edilecektir.  |
| **5** | 00:10:35 | 00:10:45  | 00:10:37 | Olay 8 saniye lik sıra dışı ve dış toleransgeldi. Bu nedenle, olay süresi maksimum sıra dışı toleransa ayarlanır. |

## <a name="can-these-settings-delay-output-of-my-job"></a>Bu ayarlar işimin çıktısını geciktirebilir mi? 

Evet. Varsayılan olarak, sıra dışı ilke sıfır (00 dakika ve 00 saniye) olarak ayarlanır. Varsayılandeğeri değiştirirseniz, işinizin ilk çıktısı bu değer (veya daha büyük) tarafından geciktirilir. 

Girişlerinizin bölümlerinden biri olay almazsa, çıktınızın geç varış ilkesi değeri tarafından geciktirmesini beklemelisiniz. Nedenini öğrenmek için aşağıdaki Giriş Bölümü hata bölümünü okuyun. 

## <a name="i-see-lateinputevents-messages-in-my-activity-log"></a>Etkinlik günlüğümde LateInputEvents iletilerini görüyorum

Bu iletiler, olayların geç geldiğini ve konfigürasyonunuza göre bırakıldığını veya ayarladığını bildirmek için gösterilir. Geç varış politikasını uygun şekilde yapılandırmışsanız, bu iletileri yoksayabilirsiniz. 

Bu iletinin örneği: <br>
<code>
{"message Time":"2019-02-04 17:11:52Z","error":null,
"message":"First Occurred: 02/04/2019 17:11:48 | Resource Name: ASAjob | Message: Source 'ASAjob' had 24 data errors of kind 'LateInputEvent' between processing times '2019-02-04T17:10:49.7250696Z' and '2019-02-04T17:11:48.7563961Z'. Input event with application timestamp '2019-02-04T17:05:51.6050000' and arrival time '2019-02-04T17:10:44.3090000' was sent later than configured tolerance.","type":"DiagnosticMessage","correlation ID":"49efa148-4asd-4fe0-869d-a40ba4d7ef3b"} 
</code>

## <a name="i-see-inputpartitionnotprogressing-in-my-activity-log"></a>GirişBölümNotIng'i etkinlik günlüğümde görüyorum

Giriş kaynağınızın (Event Hub/IoT Hub) büyük olasılıkla birden çok bölümü vardır. Azure Akış Analizi, zaman damgası t1 için çıktı üretir, ancak birleştirilen tüm bölümler en az t1 zamanında elde edin. Örneğin, sorgunun iki bölümü olan bir olay hub'ı bölümünden okuduğunu varsayalım. Bölümlerden biri olan P1'de t1 zamanına kadar olaylar vardır. Diğer bölüm, P2, zaman t1 + x kadar olaylar vardır. Çıkış daha sonra zaman t1 kadar üretilir. Ancak PartitionId yan tümcesi tarafından açık bir bölüm varsa, her iki bölüm de bağımsız olarak ilerler. 

Aynı giriş akışından birden çok bölüm birleştirildiğinde, geç varış toleransı her bölümün yeni verileri beklediği maksimum süredir. Olay Hub'ınızda bir bölüm varsa veya IoT Hub girdi almazsa, bu bölümün zaman çizelgesi geç varış tolerans eşiğine ulaşana kadar ilerlemez. Bu, geç varış tolerans eşiğine kadar çıktınızı geciktirir. Bu gibi durumlarda, aşağıdaki iletiyi görebilirsiniz:
<br><code>
{"message Time":"2/3/2019 8:54:16 PM UTC","message":"Input Partition [2] does not have additional data for more than [5] minute(s). Partition will not progress until either events arrive or late arrival threshold is met.","type":"InputPartitionNotProgressing","correlation ID":"2328d411-52c7-4100-ba01-1e860c757fc2"} 
</code><br><br>
Girişinizdeki en az bir bölümün boş olduğunu ve çıktınızı geç varış eşiğine kadar geciktireceğini bildiren bu ileti. Bunun üstesinden gelmek için, ya da tavsiye edilir:  
1. Event Hub/IoT Hub'ınızın tüm bölümlerinin giriş aldığından emin olun. 
2. Sorgunuzda Partitionby Partition by Partition clause'yı kullanın. 

## <a name="next-steps"></a>Sonraki adımlar
* [Zaman işleme konusunda dikkat edilmesi gerekenler](stream-analytics-time-handling.md)
* [Akış Analizi'nde bulunan ölçümler](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-monitoring#metrics-available-for-stream-analytics)
