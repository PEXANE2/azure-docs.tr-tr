---
title: Özel ölçüm kullanarak Azure 'da otomatik ölçeklendirme
description: Azure 'da kaynağı özel ölçüm ile ölçeklendirmenin nasıl yapılacağını öğrenin.
ms.topic: conceptual
ms.date: 05/07/2017
ms.subservice: autoscale
ms.openlocfilehash: 7758c440c75af5819099110dcbdaf5a86a1d2a04
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77425128"
---
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a>Azure 'da özel ölçüm ile otomatik ölçeklendirmeyi kullanmaya başlama
Bu makalede, Azure portal bir özel ölçüm tarafından kaynağınızın nasıl ölçekleneceğini açıklanmaktadır.

Azure Izleyici otomatik ölçeklendirme yalnızca [Sanal Makine Ölçek Kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloud Services](https://azure.microsoft.com/services/cloud-services/), [App Service-Web Apps](https://azure.microsoft.com/services/app-service/web/), [Azure Veri Gezgini kümesi](https://azure.microsoft.com/services/data-explorer/) için geçerlidir.   
Tümleştirme Hizmeti Ortamı ve [API Management Hizmetleri](https://docs.microsoft.com/azure/api-management/api-management-key-concepts).

## <a name="lets-get-started"></a>Başlamanıza izin verir
Bu makalede, Application Insights 'ın yapılandırıldığı bir Web uygulamasına sahip olduğunuz varsayılır. Henüz bir hesabınız yoksa, [ASP.NET Web siteniz için Application Insights ayarlayabilirsiniz][1]

- [Azure Portal][2] açın
- Sol gezinti bölmesinde Azure Izleyici simgesine tıklayın.
  ![Azure Izleyicisini Başlat][3]
- Otomatik ölçeklendirmenin geçerli olduğu tüm kaynakları görüntülemek için otomatik ölçeklendirme ayarını tıklatın, geçerli otomatik ölçeklendirme durumuyla ![birlikte Azure izleyici 'de otomatik ölçeklendirmeyi keşfet][4]
- Azure Izleyici 'de ' otomatik ölçeklendirme ' dikey penceresini açın ve ölçeklendirmek istediğiniz kaynağı seçin
  > Unutmayın: aşağıdaki adımlarda, Application Insights 'ın yapılandırıldığı bir Web uygulamasıyla ilişkili bir App Service planı kullanılır.
- Kaynak için ölçek ayarı dikey penceresinde, geçerli örnek sayısı ' nın 1 olduğuna dikkat edin. ' Otomatik ölçeklendirmeyi etkinleştir ' seçeneğine tıklayın.
  ![Yeni Web uygulaması için ölçek ayarı][5]
- Ölçek ayarı için bir ad girin ve "kural ekle" seçeneğine tıklayın. Sağ taraftaki bağlam bölmesi olarak açılan ölçek kuralı seçeneklerine dikkat edin. Varsayılan olarak, kaynağın CPU yüzdesi %70 ' ü aşarsa, örnek sayınız 1 ' i ölçeklendirme seçeneğini ayarlar. En üstteki ölçüm kaynağını "Application Insights" olarak değiştirin, ' kaynak ' açılan menüsünde App Insights kaynağını seçin ve ardından ölçeklendirmek istediğiniz özel ölçümü seçin.
  ![Özel ölçüme göre ölçeklendir][6]
- Yukarıdaki adıma benzer şekilde, özel ölçüm bir eşiğin altındaysa ölçek sayısını 1 ' e ölçeklendirecek bir ölçek kuralı ekleyin.
  ![CPU 'ya göre ölçeklendirin][7]
- Örnek sınırlarını ayarlayın. Örneğin, özel ölçüm dalgalanmalarına bağlı olarak 2-5 örnek arasında ölçeklendirme yapmak istiyorsanız, ' en az ' değerini ' 2 ', ' en yüksek ' değerini ' 5 ' ve ' default ' olarak ' 2 ' olarak ayarlayın
  > Note: kaynak ölçümlerini okurken bir sorun olması ve geçerli kapasitenin varsayılan kapasitenin altında olduğundan emin olmak için, otomatik ölçeklendirme, kaynak kullanılabilirliğini sağlamak üzere varsayılan değere ölçeklendirecektir. Geçerli kapasite varsayılan kapasiteden daha yüksekse, otomatik ölçeklendirme bu şekilde ölçeklenmez.
- ' Kaydet 'e tıklayın

Tebrikler. Artık ölçek ayarınızı, Web uygulamanızı özel bir ölçüme göre otomatik olarak ölçeklendirmek için başarıyla oluşturdunuz.

> Note: bir VMSS veya bulut hizmeti rolü ile çalışmaya başlamak için aynı adımlar geçerlidir.

<!--Reference-->
[1]: https://docs.microsoft.com/azure/application-insights/app-insights-asp-net
[2]: https://portal.azure.com
[3]: ./media/autoscale-custom-metric/azure-monitor-launch.png
[4]: ./media/autoscale-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/autoscale-custom-metric/scale-setting-new-web-app.png
[6]: ./media/autoscale-custom-metric/scale-by-custom-metric.png
[7]: ./media/autoscale-custom-metric/autoscale-setting-custom-metrics-ai.png

