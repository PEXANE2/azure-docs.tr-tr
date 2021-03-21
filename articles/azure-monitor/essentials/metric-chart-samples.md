---
title: Azure Izleyici ölçüm grafiği örneği
description: Azure Izleyici verilerinizi görselleştirmeyi öğrenin.
author: vgorbenko
services: azure-monitor
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: vitalyg
ms.openlocfilehash: b7e848e4565e2b1badd9bc418d28ea984a0306b2
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102051115"
---
# <a name="metric-chart-examples"></a>Ölçüm grafiği örnekleri 

Azure platformu, çoğu boyut içeren, binlerce [ölçüm üzerinde](./metrics-supported.md)sunulur. [Boyut filtrelerini](./metrics-charts.md)kullanarak, [bölme](./metrics-charts.md), grafik türünü denetleme ve grafik ayarlarını ayarlama yoluyla, altyapınızın ve uygulamalarınızın sistem durumuna ilişkin Öngörüler sağlayan güçlü tanılama görünümleri ve panolar oluşturabilirsiniz. Bu makalede, [Ölçüm Gezgini](./metrics-charts.md) kullanarak oluşturabileceğiniz grafiklerin bazı örnekleri gösterilmektedir ve bu grafiklerin her birini yapılandırmak için gereken adımları açıklar.

Harika grafik örneklerinizi dünya ile paylaşmak ister misiniz? GitHub 'da bu sayfaya katkıda bulunun ve kendi grafik örneklerinizi burada paylaşabilirsiniz!

## <a name="website-cpu-utilization-by-server-instances"></a>Sunucu örneklerine göre Web sitesi CPU kullanımı

Bu grafik, bir App Service için CPU 'nun kabul edilebilir aralık dahilinde olup olmadığını gösterir ve yükün doğru bir şekilde dağıtılıp dağıtılmadığını anlamak için örneğe göre böler. Uygulamanın 6 ' dan önce tek bir sunucu örneğinde çalıştığını ve sonra başka bir örnek ekleyerek ölçeği artırılacağını görebilirsiniz.

![Sunucu örneğine göre ortalama CPU yüzdesi satır grafiği](./media/metrics-charts/cpu-by-instance.png)

### <a name="how-to-configure-this-chart"></a>Bu grafik nasıl yapılandırılır?

App Service kaynağını seçin ve **CPU yüzdesi** ölçüsünü bulun. Ardından **bölme Uygula** ' ya tıklayın ve **örnek** boyutunu seçin.

## <a name="application-availability-by-region"></a>Bölgeye göre uygulama kullanılabilirliği

Hangi coğrafi konumların sorun yaşadığını belirlemek için uygulamanızın bölgeye göre kullanılabilirliğini görüntüleyin. Bu grafik Application Insights kullanılabilirlik ölçümünü gösterir. İzlenen uygulamanın Doğu ABD veri merkezinde kullanılabilirliğiyle ilgili bir sorun olmadığını görebilirsiniz, ancak Batı ABD ve Doğu Asya kısmi bir kullanılabilirlik sorunu yaşar.

![Konumlara göre ortalama kullanılabilirlik grafiği](./media/metrics-charts/availability-by-location.png)

### <a name="how-to-configure-this-chart"></a>Bu grafik nasıl yapılandırılır?

Önce Web siteniz için [Application Insights kullanılabilirlik](../app/monitor-web-app-availability.md) izlemeyi açmanız gerekir. Bundan sonra Application Insights kaynağını seçin ve kullanılabilirlik ölçümünü seçin. **Çalışma konumu** boyutunda bölme uygulayın.

## <a name="volume-of-failed-storage-account-transactions-by-api-name"></a>API adına göre başarısız depolama hesabı işlemleri hacmi

Depolama hesabı kaynağınız, başarısız olan fazla işlem hacmi yaşıyor. Fazla hata nedeniyle hangi API 'nin sorumlu olduğunu belirlemek için işlem ölçümünü kullanabilirsiniz. Aşağıdaki grafiğin, başarısız yanıt türüne göre bölme ve filtreleme sırasında aynı boyut (API adı) ile yapılandırıldığına dikkat edin:

![API işlemlerinin çubuk grafiği](./media/metrics-charts/split-and-filter-example.png)

### <a name="how-to-configure-this-chart"></a>Bu grafik nasıl yapılandırılır?

Ölçüm seçicide depolama hesabınızı ve **işlem** ölçümünü seçin. Grafik türünü **çubuk grafik** olarak değiştirin. **Bölmeyi Uygula** ' ya tıklayın ve boyut **API 'si adını** seçin. Sonra **Filtre Ekle** ' ye tıklayın ve **API adı** boyutunu bir kez daha seçin. Filtre iletişim kutusunda, grafiğe çizmek istediğiniz API 'Leri seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Izleyici [çalışma kitapları](../visualize/workbooks-overview.md) hakkında bilgi edinin
* [Ölçüm Gezgini](metrics-charts.md) hakkında daha fazla bilgi edinin