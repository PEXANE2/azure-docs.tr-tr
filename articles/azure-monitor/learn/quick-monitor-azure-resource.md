---
title: Azure Monitörü ile Azure kaynağını izleme
description: Azure Monitörü'nde bir Azure kaynağı için nasıl veri toplayıp analiz edeceğiz öğrenin.
ms. subservice: logs
ms.topic: quickstart
author: bwren
ms.author: bwren
ms.date: 12/15/2019
ms.openlocfilehash: c4b80e62d3800392b847a411dfc66c3278e72bba
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77661873"
---
# <a name="quickstart-monitor-an-azure-resource-with-azure-monitor"></a>Hızlı başlatma: Azure Monitörü ile Azure kaynağını izleme
[Azure Monitor,](../overview.md) oluşturuldukları anda Azure kaynaklarından veri toplamaya başlar. Bu hızlı başlatma, bir kaynak için otomatik olarak toplanan verilerin kısa bir şekilde gözden geçirinir ve belirli bir kaynak için Azure portalında nasıl görüntülenir. Daha sonra, ek veri toplamak için yapılandırma ekleyebilir ve aboneliğinizdeki tüm kaynaklar için toplanan verilere erişmek için aynı araçları kullanmak üzere Azure Monitor menüsüne gidebilirsiniz.

Azure kaynaklarından toplanan verilerin izlenmesiyle ilgili daha ayrıntılı açıklamalar için Azure [kaynaklarını Azure Monitor ile izleme](../insights/monitor-azure-resource.md)bölümüne bakın.


## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç. 


## <a name="overview-page"></a>Genel bakış sayfası
Birçok hizmet, işlemlerine hızlı bir bakış olarak **Genel Bakış** sayfalarındaki verileri izlemeyi içerir. Bu genellikle Azure Monitör Ölçümleri'nde depolanan platform ölçümlerinin bir alt kümesini temel alacaktır.

1. Aboneliğinizde bir Azure kaynağı bulun.
2. **Genel Bakış** sayfasına gidin ve görüntülenecek performans verileri varsa not edin. Bu veriler Azure Monitor tarafından sağlanacaktır. Aşağıdaki örnek, bir Azure depolama hesabının **Genel Bakış** sayfasıdır ve birden çok ölçümün görüntülendiğini görebilirsiniz.

    ![Genel bakış sayfası](media/quick-monitor-azure-resource/overview.png)

3. Aşağıda açıklanan ölçümler explorer verileri açmak için grafiklerden herhangi birini tıklayabilirsiniz.

## <a name="view-the-activity-log"></a>Etkinlik günlüğünü görüntüleme
Etkinlik günlüğü, abonelikteki her Azure kaynağındaki işlemlerhakkında bilgi sağlar. Bu, kaynağın ne zaman oluşturulduğu veya değiştirilme, bir işin ne zaman başlatıldığü veya belirli bir işlemin ne zaman gerçekleştiği gibi bilgileri içerir.

1. Kaynağınız için menünün üst kısmında **Etkinlik günlüğü'nü**seçin.
2. Geçerli filtre kaynağınızla ilgili olaylara ayarlanır. Herhangi bir olay görmüyorsanız, zaman kapsamını artırmak için **Zaman Bölmesini** değiştirmeyi deneyin.

    ![Etkinlik günlüğü](media/quick-monitor-azure-resource/activity-log-resource.png)

4. Aboneliğinizdeki diğer kaynaklardaki olayları görmek istiyorsanız, filtredeki ölçütleri değiştirin ve filtre özelliklerini kaldırın.

    ![Etkinlik günlüğü](media/quick-monitor-azure-resource/activity-log-all.png)



## <a name="view-metrics"></a>Ölçümleri görüntüle
Ölçümler, kaynağınızın belirli bir zamanda bazı yönünü açıklayan sayısal değerlerdir. Azure Monitor, platform ölçümlerini tüm Azure kaynaklarından bir dakikalık aralıklarla otomatik olarak toplar. Bu ölçümleri metrikgezgini kullanarak görüntüleyebilirsiniz.

1. Kaynak menünün **İzleme** bölümünde **Ölçümler'i**seçin. Bu, kaynağınıza ayarlanan kapsamla ölçümler gezginini açar.
2. Grafiğe metrik eklemek için **metrik ekle'yi** tıklatın.
   
   ![Ölçümler gezgini](media/quick-monitor-azure-resource/metrics-explorer-01.png)
   
4. Açılan listeden bir **Metrik** ve ardından **bir Toplama**seçin. Bu, toplanan değerlerin her zaman aralığında nasıl örnekleneceğini tanımlar.

    ![Ölçümler gezgini](media/quick-monitor-azure-resource/metrics-explorer-02.png)

5. Grafiğe ek metrik ve toplama birleşimleri eklemek için **metrik** ekle'yi tıklatın.

    ![Ölçümler gezgini](media/quick-monitor-azure-resource/metrics-explorer-03.png)



## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, Azure Monitor tarafından otomatik olarak toplanan bir Azure kaynağının Etkinlik günlüğünü ve ölçümlerini görüntülediniz. Kaynak günlükleri, kaynağın ayrıntılı çalışması hakkında bilgi sağlar, ancak toplanacak şekilde yapılandırılmalıdır. Günlük sorguları kullanılarak analiz edilebilen bir Log Analytics çalışma alanında kaynak günlüklerini toplamak için öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Azure Monitor ile kaynak günlüklerini toplama ve analiz](tutorial-resource-logs.md)
