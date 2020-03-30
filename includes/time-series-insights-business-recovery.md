---
ms.topic: include
ms.service: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.date: 02/03/2020
ms.openlocfilehash: 6a3837d01815306e469a684404ab76506f547f43
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77013801"
---
## <a name="business-disaster-recovery"></a>İş felaketkurtarma

Bu bölümde, bir felaket meydana gelse bile *(iş felaketinin kurtarılması*olarak bilinir) uygulamaların ve hizmetlerin çalışmasını sağlayan Azure Zaman Serisi Öngörüleri'nin özellikleri açıklanmaktadır.

### <a name="high-availability"></a>Yüksek kullanılabilirlik

Bir Azure hizmeti olarak Time Series Insights, Azure bölge düzeyinde fazlalıklar kullanarak belirli *yüksek kullanılabilirlik* özellikleri sağlar. Örneğin, Azure, Azure'un *bölgeler arası kullanılabilirlik* özelliği aracılığıyla olağanüstü durum kurtarma özelliklerini destekler.

Azure aracılığıyla sağlanan ek yüksek kullanılabilirlik özellikleri (ve ayrıca herhangi bir Zaman Serisi Öngörüleri örneğinde kullanılabilir) şunlardır:

- **Failover**: [Azure, coğrafi çoğaltma ve yük dengeleme sağlar.](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region)
- **Veri geri yükleme** ve **depolama kurtarma**: [Azure, verileri korumak ve kurtarmak için çeşitli seçenekler](https://docs.microsoft.com/azure/architecture/resiliency/recovery-data-corruption)sunar.
- **Azure Site Kurtarma**: [Azure, Azure Site Kurtarma](https://docs.microsoft.com/azure/site-recovery/)yoluyla site kurtarma özellikleri sağlar.
- **Azure Yedekleme**: [Azure Yedekleme,](https://docs.microsoft.com/azure/backup/backup-architecture) Azure VM'lerinin hem şirket içi hem de bulut içi yedeklemesini destekler.

Aygıtlarınız ve kullanıcılarınız için genel, bölgeler arası yüksek kullanılabilirlik sağlamak için ilgili Azure özelliklerinin etkinleştirdiğinden emin olun.

> [!NOTE]
> Azure, bölgeler arası kullanılabilirliği etkinleştirecek şekilde yapılandırıldıysa, Azure Zaman Serisi Öngörüleri'nde ek bir bölge kullanılabilirliği yapılandırması gerekmez.

### <a name="iot-and-event-hubs"></a>IoT ve etkinlik merkezleri

Bazı Azure IoT hizmetleri, yerleşik iş olağanüstü durum kurtarma özelliklerini de içerir:

- [Azure IoT Hub,bölge](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr)içi artıklığı içeren yüksek kullanılabilirlik olağanüstü durum kurtarma
- [Azure Etkinlik Hub'ları ilkeleri](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr)
- [Azure Depolama artıklığı](https://docs.microsoft.com/azure/storage/common/storage-redundancy)

Zaman Serisi Öngörülerini diğer hizmetlerle tümleştirmek ek olağanüstü durum kurtarma fırsatları sağlar. Örneğin, etkinlik merkezinize gönderilen telemetri, yedek bir Azure Blob depolama veritabanında kalıcı olabilir.

### <a name="time-series-insights"></a>Time Series Insights

Zaman Serisi Öngörüleri verilerinizi, uygulamalarınızı ve hizmetlerinizi kesintiye uğratsalar bile çalışır durumda tutmanın birkaç yolu vardır. 

Ancak, aşağıdaki amaçlar için Azure Zaman Serisi ortamınızın tam bir yedek kopyasının da gerekli olduğunu belirleyebilirsiniz:

- Özellikle Time Series Insights için veri ve trafiği yeniden yönlendirmek için bir *başarısız lık örneği* olarak
- Verileri ve denetim bilgilerini korumak için

Genel olarak, Bir Zaman Serisi Öngörüleri ortamını çoğaltmanın en iyi yolu, yedek bir Azure bölgesinde ikinci bir Time Series Öngörüleri ortamı oluşturmaktır. Olaylar da birincil olay kaynağınızdan bu ikincil ortama gönderilir. İkinci bir özel tüketici grubu kullandığınızdan emin olun. Daha önce açıklandığı gibi, bu kaynağın iş felaketkurtarma yönergelerine uyun.

Yinelenen bir ortam oluşturmak için:

1. İkinci bir bölgede bir ortam oluşturun. Daha fazla bilgi için [Azure portalında yeni bir Zaman Serisi Öngörüleri Oluştur ortamını](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started)okuyun.
1. Etkinlik kaynağınız için ikinci bir özel tüketici grubu oluşturun.
1. Olay kaynağını yeni ortama bağlayın. İkinci özel tüketici grubunu belirlediğinizden emin olun.
1. Zaman Serisi Öngörüleri [IoT Hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) ve [Olay Hub'ları](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access) belgelerini inceleyin.

Bir olay oluşursa:

1. Birincil bölgeniz bir felaket olayı sırasında etkilenirse, işlemleri yedek Zaman Serisi Öngörüleri ortamına yeniden yönlendirin.
1. Tüm Time Series Insights telemetrive sorgu verilerini yedeklemek ve kurtarmak için ikinci bölgenizi kullanın.

> [!IMPORTANT]
> Bir hata oluşursa:
> 
> * Bir gecikme de oluşabilir.
> * İşlemler yeniden yönlendirildikçe ileti işlemede anlık bir artış oluşabilir.
> 
> Daha fazla bilgi için [Time Series Insights'ta Azlayık gecikmesi](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency)okuyun.

