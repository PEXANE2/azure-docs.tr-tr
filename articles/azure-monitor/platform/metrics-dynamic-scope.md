---
title: Ölçüm Gezgini birden çok kaynağı görüntüleme
description: Azure Izleyici 'de birden çok kaynağı görselleştirmeyi öğrenin Ölçüm Gezgini
author: ritaroloff
services: azure-monitor
ms.topic: conceptual
ms.date: 12/14/2020
ms.author: riroloff
ms.subservice: metrics
ms.openlocfilehash: 724809dbce3ca1b5a36f4da0ba5c03d0f78897f5
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577759"
---
# <a name="viewing-multiple-resources-in-metrics-explorer"></a>Ölçüm Gezgini birden çok kaynağı görüntüleme

Kaynak kapsamı seçici, aynı abonelik ve bölge içindeki birden fazla kaynak arasında ölçümleri görüntülemenize olanak sağlar. Aşağıda, Azure Izleyici Ölçüm Gezgini birden fazla kaynağın nasıl görüntüleneceği hakkında yönergeler verilmiştir. 

## <a name="selecting-a-resource"></a>Kaynak seçme 

**Azure izleyici** menüsünde veya bir kaynağın menüsünün **izleme** bölümünden **ölçümler** ' i seçin. Kaynak kapsamı seçicisini açmak için "kapsam seçin" düğmesine tıklayın. Bu işlem, ölçümlerini görmek istediğiniz kaynakları seçmenizi sağlar. Bu, ölçüm Gezginini bir kaynağın menüsünden açtıysanız zaten doldurulmalıdır. 

![Kırmızı renkle vurgulanmış kaynak kapsamı seçicisinin ekran görüntüsü](./media/metrics-charts/019.png)

## <a name="selecting-multiple-resources"></a>Birden çok kaynak seçme 

Bazı kaynak türleri, aynı abonelik ve konum dahilinde oldukları sürece birden fazla kaynak üzerinde ölçümleri sorgulama özelliğini etkinleştirdi. Bu kaynak türleri "kaynak türleri" açılan listesinin en üstünde bulunabilir. 

![Çoklu kaynak uyumlu kaynakların açılan listesini gösteren ekran görüntüsü ](./media/metrics-charts/020.png)

> [!WARNING] 
> Birden çok kaynakta, kaynak grubunda veya abonelikte ölçümleri görselleştirmek için abonelik düzeyinde Izleme okuyucusu izninizin olması gerekir. Bunu yapmak için lütfen [Bu belgedeki](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)yönergeleri izleyin.

Birden çok kaynak üzerinde ölçümleri görselleştirmek için kaynak kapsamı seçicisinden birden çok kaynak seçerek başlayın. 

![Birden çok kaynağın nasıl seçdiğinin gösterildiği ekran görüntüsü](./media/metrics-charts/021.png)

> [!NOTE]
> Aynı kaynak türü, konum ve abonelik içinde yalnızca birden fazla kaynak seçebilirsiniz. Bu ölçüt dışındaki kaynaklar seçilemeyecektir. 

Seçmeyi tamamladığınızda, seçiminizi kaydetmek için "Uygula" düğmesine tıklayın. 

## <a name="selecting-a-resource-group-or-subscription"></a>Kaynak grubu veya abonelik seçme 

> [!WARNING]
> Birden çok kaynakta, kaynak grubunda veya abonelikte ölçümleri görselleştirmek için abonelik düzeyinde Izleme okuyucusu izninizin olması gerekir. 

Çoklu kaynak uyumlu türler için bir abonelik veya birden çok kaynak grubu genelinde ölçümleri de sorgulayabilirsiniz. Bir abonelik veya bir veya daha fazla kaynak grubu seçerek başlayın: 

![Birden çok kaynak grubu genelinde sorgu oluşturmayı gösteren ekran görüntüsü ](./media/metrics-charts/022.png)

Daha sonra yeni kapsamınızı uygulamaya devam edebilmeniz için önce bir kaynak türü ve konum seçmeniz gerekir. 

![Seçili kaynak gruplarını gösteren ekran görüntüsü ](./media/metrics-charts/023.png)

Ayrıca, bu, hangi kaynakların uygulanacağını doğrulamak için seçilen kapsamları genişletebilirsiniz.

![Gruplar içindeki seçili kaynakları gösteren ekran görüntüsü ](./media/metrics-charts/024.png)

Kapsamlarınızı seçmeyi tamamladıktan sonra Seçimlerinizi kaydetmek için "Uygula" düğmesine tıklayın. 

## <a name="splitting-and-filtering-by-resource-group-or-resources"></a>Kaynak grubu veya kaynaklara göre bölme ve filtreleme

Kaynaklarınızı doldurduktan sonra, verilerinize daha fazla öngörü kazanmak için bölme ve filtreleme aracını kullanabilirsiniz. 

Bölme, ölçümün farklı segmentlerinin birbirleriyle nasıl karşılaştırılacağını görselleştirmenize olanak tanır. Örneğin, birden çok kaynak için bir ölçüm çizdirme yaparken, kaynak kimliğine veya kaynak grubuna göre bölmek için "bölmeyi Uygula" aracını kullanabilirsiniz. Bu, birden çok kaynak veya kaynak grubu genelinde tek bir ölçümü kolayca karşılaştırmanıza imkan tanır.  

Örneğin, aşağıda, 9Vm 'lerde CPU yüzdesi olarak bir grafik verilmiştir. Kaynak kimliğine göre bölerek sanal makine başına yüzde CPU 'nun nasıl farklı olduğunu kolayca görebilirsiniz. 

![VM başına yüzde CPU değerini görmek için bölmeyi nasıl kullanabileceğinizi gösteren ekran görüntüsü](./media/metrics-charts/026.png)

Bölmenin yanı sıra, yalnızca görmek istediğiniz kaynak gruplarını görüntülemek için filtreleme özelliğini de kullanabilirsiniz.  Örneğin, belirli bir kaynak grubu için VM 'Lerin CPU yüzdesini görüntülemek istiyorsanız, kaynak grubuna göre filtrelemek için "Filtre Ekle" aracını kullanabilirsiniz. Bu örnekte, Uyarspintoysdemo içindeki kaynaklarla ilişkili ölçümleri kaldıran, bir,,, bir,,,, bir,,, bir 

![Kaynak grubuna göre nasıl filtreleyekullanabileceğinizi gösteren ekran görüntüsü](./media/metrics-charts/027.png)

## <a name="pinning-your-multi-resource-charts"></a>Çoklu kaynak grafiklerinizi sabitleme 

> [!WARNING] 
> Birden çok kaynakta, kaynak grubunda veya abonelikte ölçümleri görselleştirmek için abonelik düzeyinde Izleme okuyucusu izninizin olması gerekir. Bunu yapmak için lütfen [Bu belgedeki](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)yönergeleri izleyin. 

Çoklu kaynak grafiğinizi sabitlemek için lütfen [buradaki](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts#create-alert-rules)yönergeleri izleyin. 

## <a name="next-steps"></a>Sonraki adımlar

* [Ölçüm Gezgini sorunlarını giderme](metrics-troubleshoot.md)
* [Azure hizmetleri için kullanılabilir ölçümlerin listesine bakın](metrics-supported.md)
* [Yapılandırılmış grafik örneklerine bakın](metric-chart-samples.md)

