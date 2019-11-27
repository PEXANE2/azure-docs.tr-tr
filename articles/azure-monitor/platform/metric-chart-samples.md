---
title: Azure Izleyici ölçüm grafiği örneği
description: Azure Izleyici verilerinizi görselleştirmeyi öğrenin.
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: 8cc653b8e5d2c239243bf6a09955b10011c7408b
ms.sourcegitcommit: 36eb583994af0f25a04df29573ee44fbe13bd06e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74538976"
---
# <a name="metric-chart-examples"></a>Ölçüm grafiği örnekleri 

Azure platformu, çoğu boyut içeren, binlerce [ölçüm üzerinde](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported)sunulur. [Boyut filtrelerini](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts)kullanarak, [bölme](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts), grafik türünü denetleme ve grafik ayarlarını ayarlama yoluyla, altyapınızın ve uygulamalarınızın sistem durumuna ilişkin Öngörüler sağlayan güçlü tanılama görünümleri ve panolar oluşturabilirsiniz. Bu makalede, [Ölçüm Gezgini](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts) kullanarak oluşturabileceğiniz grafiklerin bazı örnekleri gösterilmektedir ve bu grafiklerin her birini yapılandırmak için gereken adımları açıklar.

Harika grafik örneklerinizi dünya ile paylaşmak ister misiniz? GitHub 'da bu sayfaya katkıda bulunun ve kendi grafik örneklerinizi burada paylaşabilirsiniz!

## <a name="website-cpu-utilization-by-server-instances"></a>Sunucu örneklerine göre Web sitesi CPU kullanımı

Bu grafik, bir App Service için CPU 'nun kabul edilebilir aralık dahilinde olup olmadığını gösterir ve yükün doğru bir şekilde dağıtılıp dağıtılmadığını anlamak için örneğe göre böler. Uygulamanın 6 ' dan önce tek bir sunucu örneğinde çalıştığını ve sonra başka bir örnek ekleyerek ölçeği artırılacağını görebilirsiniz.

![Sunucu örneğine göre ortalama CPU yüzdesi satır grafiği](./media/metric-chart-samples/cpu-by-instance.png)

### <a name="how-to-configure-this-chart"></a>Bu grafik nasıl yapılandırılır?

App Service kaynağını seçin ve **CPU yüzdesi** ölçüsünü bulun. Ardından **bölme Uygula** ' ya tıklayın ve **örnek** boyutunu seçin.

## <a name="application-availability-by-region"></a>Bölgeye göre uygulama kullanılabilirliği

Hangi coğrafi konumların sorun yaşadığını belirlemek için uygulamanızın bölgeye göre kullanılabilirliğini görüntüleyin. Bu grafik Application Insights kullanılabilirlik ölçümünü gösterir. İzlenen uygulamanın Doğu ABD veri merkezinde kullanılabilirliğiyle ilgili bir sorun olmadığını görebilirsiniz, ancak Batı ABD ve Doğu Asya kısmi bir kullanılabilirlik sorunu yaşar.

![Konumlara göre ortalama kullanılabilirlik grafiği](./media/metric-chart-samples/availability-run-location.png)

### <a name="how-to-configure-this-chart"></a>Bu grafik nasıl yapılandırılır?

Önce Web siteniz için [Application Insights kullanılabilirlik](https://docs.microsoft.com/azure/azure-monitor/app/monitor-web-app-availability) izlemeyi açmanız gerekir. Bundan sonra Application Insights kaynağını seçin ve kullanılabilirlik ölçümünü seçin. **Çalışma konumu** boyutunda bölme uygulayın.

## <a name="volume-of-storage-account-transactions-by-api-name"></a>API adına göre depolama hesabı işlemleri hacmi

Depolama hesabı kaynağınız fazla miktarda işlem hacimde yaşıyor. Fazla yükün hangi API 'nin sorumlu olduğunu belirlemek için işlem ölçümünü kullanabilirsiniz. Aşağıdaki grafiğin filtrelemede aynı boyut (API adı) ile yapılandırıldığını ve görünümü yalnızca ilgilendiğiniz API çağrılarına daraltmak için böltiğine dikkat edin:

![API işlemlerinin çubuk grafiği](./media/metric-chart-samples/transactions-by-api.png)

### <a name="how-to-configure-this-chart"></a>Bu grafik nasıl yapılandırılır?

Ölçüm seçicide depolama hesabınızı ve **işlem** ölçümünü seçin. Grafik türünü **çubuk grafik**olarak değiştirin. **Bölmeyi Uygula** ' ya tıklayın ve boyut **API 'si adını**seçin. Sonra **Filtre Ekle** ' ye tıklayın ve **API adı** boyutunu bir kez daha seçin. Filtre iletişim kutusunda, grafiğe çizmek istediğiniz API 'Leri seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Izleyici [çalışma kitapları](../../azure-monitor/app/usage-workbooks.md) hakkında bilgi edinin
* [Ölçüm Gezgini](metrics-charts.md) hakkında daha fazla bilgi edinin
