---
title: Azure'da özel bir metrik kullanarak otomatik ölçeklendirme
description: Azure'da özel metrikle kaynağınızı nasıl ölçeklendireceklerini öğrenin.
ms.topic: conceptual
ms.date: 05/07/2017
ms.subservice: autoscale
ms.openlocfilehash: 7758c440c75af5819099110dcbdaf5a86a1d2a04
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77425128"
---
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a>Azure'da özel ölçüme göre otomatik ölçekle başlayın
Bu makalede, Azure portalında kaynağınızı özel bir ölçüye göre nasıl ölçeklendirilir.

Azure Monitör otomatik ölçeklendirmesi yalnızca [Sanal Makine Ölçeği Kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services/), Uygulama Hizmeti - [Web Uygulamaları](https://azure.microsoft.com/services/app-service/web/), Azure [Veri Gezgini Kümesi](https://azure.microsoft.com/services/data-explorer/) ,   
Entegrasyon Hizmet Ortamı ve [API Yönetimi hizmetleri.](https://docs.microsoft.com/azure/api-management/api-management-key-concepts)

## <a name="lets-get-started"></a>Başlayabilesin
Bu makalede, uygulama istatistikleri yapılandırılan bir web uygulaması var varsayar. Zaten bir web siteniz yoksa, [ASP.NET web siteniz için Uygulama Öngörüleri ayarlayabilirsiniz][1]

- [Azure portalı][2] aç
- Sol gezinti bölmesindeki Azure Monitör simgesine tıklayın.
  ![Azure Monitörü Başlat][3]
- Geçerli otomatik ölçek durumuyla ![birlikte azure monitöründe otomatik ölçekle birlikte otomatik ölçek uygulanabilir olan tüm kaynakları görüntülemek için Otomatik Ölçek ayarını tıklatın][4]
- Azure Monitor'da 'Otomatik Ölçeklendirme' bıçağını açın ve ölçeklendirmek istediğiniz bir kaynak seçin
  > Not: Aşağıdaki adımlar, uygulama öngörüleri yapılandırılan bir web uygulamasıyla ilişkili bir uygulama hizmet planı kullanır.
- Kaynak için ölçek ayar bıçağında, geçerli örnek sayısının 1 olduğuna dikkat edin. 'Otomatik ölçeklendir'i etkinleştir'e tıklayın.
  ![Yeni web uygulaması için ölçek ayarı][5]
- Ölçek ayarı için bir ad ve "Kural ekle"yi tıklatın. Sağ tarafta bağlam bölmesi olarak açılan ölçek kuralı seçeneklerine dikkat edin. Varsayılan olarak, kaynağın CPU yüzdesi %70'i aşarsa örnek sayınızı 1 olarak ölçeklendirme seçeneğini ayarlar. Üstteki metrik kaynağı "Uygulama Öngörüleri" olarak değiştirin, 'Kaynak' açılır durumundaki uygulama öngörüleri kaynağını seçin ve ardından ölçeklendirmek istediğiniz özel metrik'i seçin.
  ![Özel metrikgöre ölçeklendirin][6]
- Yukarıdaki adıma benzer şekilde, özel metrik bir eşiğin altındaysa ölçek sayısını 1'e ölçeklendirecek ve azaltacak bir ölçek kuralı ekleyin.
  ![Cpu'ya dayalı ölçek][7]
- Örnek sınırlarını ayarlayın. Örneğin, özel metrik dalgalanmalara bağlı olarak 2-5 örnek arasında ölçeklendirmek istiyorsanız, 'en az'ı '2', 'maksimum'u '5' ve 'varsayılan' ile '2' olarak ayarlayın
  > Not: Kaynak ölçümlerini okurken bir sorun olması ve geçerli kapasitenin varsayılan kapasitenin altında olması durumunda, kaynağın kullanılabilirliğini sağlamak için Otomatik Ölçek varsayılan değere ölçeklenir. Geçerli kapasite zaten varsayılan kapasiteden daha yüksekse, Otomatik Ölçek ölçeklendirilmeyecektir.
- 'Kaydet' üzerine tıklayın

Tebrikler. Artık web uygulamanızı özel bir ölçüye göre otomatik ölçeklendirmek için ölçek ayarınızı başarıyla oluşturdunuz.

> Not: VMSS veya bulut hizmeti rolüyle başlamak için aynı adımlar geçerlidir.

<!--Reference-->
[1]: https://docs.microsoft.com/azure/application-insights/app-insights-asp-net
[2]: https://portal.azure.com
[3]: ./media/autoscale-custom-metric/azure-monitor-launch.png
[4]: ./media/autoscale-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/autoscale-custom-metric/scale-setting-new-web-app.png
[6]: ./media/autoscale-custom-metric/scale-by-custom-metric.png
[7]: ./media/autoscale-custom-metric/autoscale-setting-custom-metrics-ai.png

