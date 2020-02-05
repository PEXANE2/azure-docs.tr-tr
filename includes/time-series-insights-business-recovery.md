---
ms.topic: include
ms.service: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.date: 02/03/2020
ms.openlocfilehash: 6a3837d01815306e469a684404ab76506f547f43
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "77013801"
---
## <a name="business-disaster-recovery"></a>İş olağanüstü durum kurtarma

Bu bölümde, bir olağanüstü durum meydana gelirse ( *iş olağanüstü durum kurtarma*olarak bilinir) uygulamaların ve hizmetlerin çalışır durumda tutulması Azure Time Series Insights özellikleri açıklanmaktadır.

### <a name="high-availability"></a>Yüksek kullanılabilirlik

Azure hizmeti olarak Time Series Insights, Azure bölge düzeyinde artıklıkları kullanarak belirli *yüksek kullanılabilirlik* özellikleri sağlar. Örneğin, Azure, Azure 'un *çapraz bölge kullanılabilirliği* özelliği aracılığıyla olağanüstü durum kurtarma özelliklerini destekler.

Azure (ve ayrıca tüm Time Series Insights örnekleri için de kullanılabilir) ile sağlanan diğer yüksek kullanılabilirlik özellikleri şunlardır:

- **Yük devretme**: Azure, [coğrafi çoğaltma ve yük dengeleme](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region)sağlar.
- **Veri geri yükleme** ve **depolama kurtarma**: Azure, [verileri korumak ve kurtarmak için çeşitli seçenekler](https://docs.microsoft.com/azure/architecture/resiliency/recovery-data-corruption)sunar.
- **Azure Site Recovery**: Azure [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/)aracılığıyla Site Recovery özellikleri sağlar.
- **Azure Backup**: [Azure Backup](https://docs.microsoft.com/azure/backup/backup-architecture) hem şirket Içi hem de Azure VM 'lerinin bulut yedeklemesini destekler.

Cihazlarınız ve kullanıcılarınız için küresel ve çapraz bölge yüksek kullanılabilirlik sağlamak üzere ilgili Azure özelliklerini etkinleştirdiğinizden emin olun.

> [!NOTE]
> Azure, bölgeler arası kullanılabilirliği etkinleştirecek şekilde yapılandırıldıysa Azure Time Series Insights ek çapraz bölge kullanılabilirlik yapılandırması gerekmez.

### <a name="iot-and-event-hubs"></a>IoT ve Olay Hub 'ları

Bazı Azure IoT Hizmetleri, yerleşik iş olağanüstü durum kurtarma özelliklerini de içerir:

- Azure IoT Hub, bölge içi artıklık içeren [yüksek kullanılabilirliğe sahip olağanüstü durum kurtarma](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr)
- [Azure Event Hubs ilkeleri](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr)
- [Azure depolama artıklığı](https://docs.microsoft.com/azure/storage/common/storage-redundancy)

Diğer hizmetlerle Time Series Insights tümleştirme, ek olağanüstü durum kurtarma olanakları sağlar. Örneğin, Olay Hub 'ınıza gönderilen telemetri, yedek bir Azure Blob depolama veritabanına kalıcı olabilir.

### <a name="time-series-insights"></a>Zaman Serisi İçgörüleri

Time Series Insights verilerinizi, uygulamalarınızı ve hizmetlerinizi, kesintiye uğrasa bile çalışır durumda tutmanın birkaç yolu vardır. 

Ancak, aşağıdaki amaçlar için Azure zaman serisi ortamınızın tamamen bir yedek kopyasının de gerekli olduğunu belirleyebilirsiniz:

- Özel bir *Yük devretme örneği* olarak, verileri ve trafiği yeniden yönlendirme Time Series Insights
- Veri ve denetim bilgilerini korumak için

Genel olarak, bir Time Series Insights ortamını yinelemek için en iyi yol, yedekleme Azure bölgesinde ikinci bir Time Series Insights ortamı oluşturmaktır. Ayrıca, olaylar birincil olay kaynağınızdan bu ikincil ortama gönderilir. İkinci bir adanmış Tüketici grubu kullandığınızdan emin olun. Daha önce açıklandığı gibi kaynağın iş olağanüstü durum kurtarma yönergelerini izleyin.

Yinelenen bir ortam oluşturmak için:

1. İkinci bir bölgede bir ortam oluşturun. Daha fazla bilgi için [Azure Portal yeni Time Series Insights ortamı oluştur](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started)' u okuyun.
1. Olay kaynağınız için ikinci bir ayrılmış Tüketici grubu oluşturun.
1. Bu olay kaynağını yeni ortama bağlayın. İkinci adanmış tüketici grubunu seçtiğinizden emin olun.
1. Time Series Insights [IoT Hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) ve [Event Hubs](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access) belgelerini gözden geçirin.

Bir olay oluşursa:

1. Birincil bölgeniz bir olağanüstü durum olayı sırasında etkileniyorsa, işlemleri yedekleme Time Series Insights ortamına yeniden yönlendir.
1. Tüm Time Series Insights telemetri ve sorgu verilerini yedeklemek ve kurtarmak için ikinci bölgenizi kullanın.

> [!IMPORTANT]
> Yük devretme gerçekleşirse:
> 
> * Bir gecikme da oluşabilir.
> * İşlemler tekrar yönlendirilmesiyle ileti işleme içindeki bir kopan ani meydana gelebilir.
> 
> Daha fazla bilgi için [Time Series Insights gecikme süresini azaltır](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency).

