---
title: Azure Monitör metrik grafik örneği
description: Azure Monitor verilerinizi görselleştirme hakkında bilgi edinin.
author: vgorbenko
services: azure-monitor
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: 1aaeb853a67b36a21a09db57e015029d10cd0c36
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77660879"
---
# <a name="metric-chart-examples"></a>Metrik grafik örnekleri 

Azure platformu, çoğu boyutları olan [binden fazla ölçüm](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported)sunar. Boyut [filtrelerini](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts)kullanarak, [bölme](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts)uygulayarak, grafik türünü kontrol ederek ve grafik ayarlarını ayarlayarak, altyapınızın ve uygulamalarınızın durumu hakkında bilgi sağlayan güçlü tanılama görünümleri ve panolar oluşturabilirsiniz. Bu makalede, [Ölçümler Gezgini](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts) kullanarak oluşturabileceğiniz grafiklerin bazı örneklerini gösterir ve bu grafiklerin her birini yapılandırmak için gerekli adımları açıklar.

Harika grafik örneklerinizi dünyayla paylaşmak ister misiniz? GitHub'daki bu sayfaya katkıda bulunun ve kendi grafik örneklerinizi burada paylaşın!

## <a name="website-cpu-utilization-by-server-instances"></a>Sunucu örneklerine göre web sitesi CPU kullanımı

Bu grafik, bir Uygulama Hizmeti için CPU'nun kabul edilebilir aralıkta olup olmadığını gösterir ve yükün düzgün dağıtılıp dağıtılmadığını belirlemek için onu kolayca ayırır. Grafikten uygulamanın 06:00'dan önce tek bir sunucu örneğinde çalıştığını ve daha sonra başka bir örnek ekleyerek ölçeklendirildiğini görebilirsiniz.

![Sunucu örneğine göre ortalama işlemci yüzdesinin çizgi grafiği](./media/metric-chart-samples/cpu-by-instance.png)

### <a name="how-to-configure-this-chart"></a>Bu grafik nasıl yapılandırılabilen?

Uygulama Hizmeti kaynağınızı seçin ve **CPU Yüzdesi** ölçümünü bulun. Ardından **Uygula'ya** tıklayın ve **Örnek** boyutunu seçin.

## <a name="application-availability-by-region"></a>Bölgeye göre uygulama kullanılabilirliği

Hangi coğrafi konumlarda sorun yaşadığını belirlemek için uygulamanızın bölgeye göre kullanılabilirliğini görüntüleyin. Bu grafik, Application Insights kullanılabilirlik ölçümlerini gösterir. İzlenen uygulamanın Doğu ABD veri merkezinden kullanılabilirlik ile ilgili bir sorun olmadığını görebilirsiniz, ancak Batı ABD ve Doğu Asya'dan kısmi bir kullanılabilirlik sorunu yaşıyor.

![Konumlara göre ortalama kullanılabilirlik grafiği](./media/metric-chart-samples/availability-run-location.png)

### <a name="how-to-configure-this-chart"></a>Bu grafik nasıl yapılandırılabilen?

Öncelikle web siteniz için [Application Insights kullanılabilirlik](https://docs.microsoft.com/azure/azure-monitor/app/monitor-web-app-availability) izleme açmanız gerekir. Bundan sonra, Application Insights kaynağınızı seçin ve Kullanılabilirlik metrikünü seçin. **Run konumu** boyutuna bölme uygulayın.

## <a name="volume-of-storage-account-transactions-by-api-name"></a>API adına göre depolama hesabı hareketlerinin hacmi

Depolama hesabı kaynağınız fazla işlem hacmiyle karşınızdadır. Fazla yükten hangi API'nin sorumlu olduğunu belirlemek için hareket metriklerini kullanabilirsiniz. Aşağıdaki grafiğin filtreleme ve bölmede aynı boyutla yapılandırıldığına dikkat edin, görünümü yalnızca ilgi çekici API çağrılarına daraltmak için:

![API hareketlerinin çubuk grafiği](./media/metric-chart-samples/transactions-by-api.png)

### <a name="how-to-configure-this-chart"></a>Bu grafik nasıl yapılandırılabilen?

Metrik seçicide, depolama hesabınızı ve **Hareketler** metrik'ini seçin. Grafik türünü **Çubuk grafiğine**değiştirin. **Bölme uygula'yı** tıklatın ve boyut **API adını**seçin. Ardından Ekle **filtresine** tıklayın ve **API ad** boyutunu bir kez daha seçin. Filtre iletişim kutusunda, grafikte çizmek istediğiniz API'leri seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Monitör [çalışma kitapları](../../azure-monitor/app/usage-workbooks.md) hakkında bilgi edinin
* [Metrik Explorer](metrics-charts.md) hakkında daha fazla bilgi edinin
