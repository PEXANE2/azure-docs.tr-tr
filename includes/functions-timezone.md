---
ms.openlocfilehash: b4fbeb6baa4516ca4cf3ca6194195fae2c688b07
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86165676"
---
CRON ifadeleriyle kullanılan varsayılan saat dilimi Eşgüdümlü Evrensel Saat (UTC) ' dir. CRON ifadenizi başka bir saat dilimine göre oluşturmak için adlı işlev uygulamanız için bir uygulama ayarı oluşturun `WEBSITE_TIME_ZONE` . 

Bu ayarın değeri, işlev uygulamanızın çalıştırıldığı işletim sistemine ve plana bağlıdır.

|İşletim sistemi |Plan |Değer |
|-|-|-|
| **Windows** |Tümü | Değeri, istenen saat diliminin adı [Microsoft saat dilimi dizini] ( https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749073(v=ws.10) . |
| **Linux** |Premium<br/>Ayrılmış |Değeri, [TZ veritabanında](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)gösterildiği gibi istenen saat diliminin adı olarak ayarlayın. |

> [!NOTE]
> `WEBSITE_TIME_ZONE`, Linux tüketim planında Şu anda desteklenmiyor.

Örneğin, *Doğu Standart Saati* (Windows) veya *Amerika/New_York* (Linux) UTC-05:00 ' dir. Zamanlayıcı tetikleyicinizin her gün 10:00 ' de tetiklenmesi için, UTC saat dilimi hesaplarının aşağıdaki NCRONTAB ifadesini kullanın:

```
"0 0 15 * * *"
``` 

Ya da işlev uygulamanız için adlı bir uygulama ayarı oluşturun `WEBSITE_TIME_ZONE` , değeri `Eastern Standard Time` (Windows) veya `America/New_York` (Linux) olarak ayarlayın ve ardından aşağıdaki ncrontab ifadesini kullanın: 

```
"0 0 10 * * *"
``` 

' I kullandığınızda, saat, belirli bir saat dilimlerinde `WEBSITE_TIME_ZONE` yaz tasarrufu süresi gibi zaman değişikliği için ayarlanır. 
