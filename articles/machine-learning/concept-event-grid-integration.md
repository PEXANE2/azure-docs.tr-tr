---
title: Azure Machine Learning etkinliklerini tüketin
titleSuffix: Azure Machine Learning
description: Bu makalede, Azure Machine Learning tarafından oluşturulan etkinliklere abone olmak, tepki vermek ve aboneliğinizi iptal etmek için Azure Olay Izgarasını nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: andyxu
author: gogowings
ms.date: 11/04/2019
ms.openlocfilehash: d8a975487c68a21b2c8b6fa2f07d86c312243f12
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79139054"
---
# <a name="consume-azure-machine-learning-events-preview"></a>Azure Machine Learning etkinliklerini tüketin (Önizleme)

Azure Machine Learning, model eğitimi, model dağıtımı ve izleme de dahil olmak üzere makine öğrenimi sürecinin tüm yaşam döngüsünü yönetir. Azure Machine Learning etkinlikleri, uygulamaların, eğitim çalıştırmalarının tamamlanması, modellerin kaydedilmesi ve dağıtılması ve veri kaymasının algılanması gibi makine öğrenimi yaşam döngüsü sırasında ki olaylara modern sunucusuz kullanarak tepki göstermesine olanak tanır Mimarileri. 

Bu etkinlikler [Azure Olay Ağıtı](https://azure.microsoft.com/services/event-grid/)üzerinden yayınlanır. Müşteriler Azure portalı, Powershell veya Azure CLI'yi kullanarak [bir veya daha fazla olay türü belirterek ve filtreleme koşullarını](/azure/event-grid/event-filtering)belirterek olaylara kolayca abone olabilir. Müşterilerin Azure İşlevler, Azure Mantık Uygulamaları veya genel web hook'ları gibi çok çeşitli olay işleyicileri oluşturma seçenekleri de vardır. Azure Machine Learning, Azure Etkinlik Ağıtı ile birlikte, etkinlik odaklı uygulamalar oluşturmanız için kullanılabilir yüksek, güvenilir ve hataya dayanıklı bir etkinlik teslim platformu sağlar.

Etkinlik Ağıla Azure Machine Learning'i kullanma hakkında daha fazla bilgi [için](how-to-use-event-grid.md)bkz.

## <a name="the-event-model"></a>Olay modeli 

Azure Olay Ağı,Azure Machine Learning ve diğer Azure hizmetleri gibi kaynaklardan gelen etkinlikleri okur. Bu olaylar daha sonra Azure Etkinlik Hub'ları, Azure İşlevleri, Mantık Uygulamaları ve diğerleri gibi olay işleyicilerine gönderilir. Aşağıdaki diyagram, Olay Izgara'sının kaynakları ve işleyicileri nasıl bağdaştırdığını gösterir, ancak desteklenen tümleştirmelerin kapsamlı bir listesi değildir.

![Azure Olay Izgarası fonksiyonel modeli](./media/concept-event-grid-integration/azure-event-grid-functional-model.png)

Olay kaynakları ve olay işleyicileri hakkında daha fazla bilgi için olay [ızgarası nedir?](/azure/event-grid/overview)

## <a name="azure-machine-learning-event-types"></a>Azure Machine Learning etkinlik türleri

Azure Machine Learning, makine öğrenimi yaşam döngüsünün çeşitli noktalarında etkinlikler sağlar: 

| Olay türü | Açıklama |
| ---------- | ----------- |
| `Microsoft.MachineLearningServices.RunCompleted` | Makine öğrenme denemesi tamamlandığında yükseltildi |
| `Microsoft.MachineLearningServices.ModelRegistered` | Çalışma alanına bir makine öğrenme modeli kaydedildiğinde yükseltildi |
| `Microsoft.MachineLearningServices.ModelDeployed` | Bir veya daha fazla modelle çıkarım hizmetinin dağıtımı tamamlandığında yükseltildi |
| `Microsoft.MachineLearningServices.DatasetDriftDetected` | İki veri kümesi için veri kayması algılama işi tamamlandığında yükseltildi |
| `Microsoft.MachineLearningServices.RunStatusChanged` | Bir çalıştırma durumu değiştiğinde yükseltilmiş, şu anda yalnızca bir çalıştırma durumu 'başarısız' olduğunda yükseltilmiş |

## <a name="subscribe-to-machine-learning-events"></a>Machine Learning etkinliklerine abone olun

Azure Machine Learning etkinlikleri için abonelikler rol tabanlı erişim denetimi (RBAC) tarafından korunur. Yalnızca bir çalışma alanının [sahibi veya katılımcısı](how-to-assign-roles.md#default-roles) olay abonelikleri oluşturabilir, güncelleyebilir ve silebilir.

Olay abonelikleri çeşitli koşullara göre filtrelenebilir. Filtreler olay aboneliğinin [oluşturulması](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest) sırasında veya [daha sonraki bir zamanda](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest)olay aboneliklerine uygulanabilir. 

### <a name="filter-by-event-type"></a>Olay türüne göre filtreleme
Etkinlik aboneliği bir veya daha fazla Azure Machine Learning etkinlik türü belirtebilir.

### <a name="filter-by-event-subject"></a>Olay konusuna göre filtreleme
Azure Olay Idamı, eşleşen bir konuya sahip olayların aboneye teslim edilebilmeleri için, __eşlemelerle başlayan__ ve __biten__ ekişlere göre konu filtrelerini destekler. Farklı makine öğrenimi etkinlikleri farklı konu biçimine sahiptir.

| Olay türü | Konu biçimi | Örnek konu |
| ---------- | ----------- | ----------- |
| `Microsoft.MachineLearningServices.RunCompleted` | `experiments/{ExperimentId}/runs/{RunId}` | `experiments/b1d7966c-f73a-4c68-b846-992ace89551f/runs/my_exp1_1554835758_38dbaa94` |
| `Microsoft.MachineLearningServices.ModelRegistered` | `models/{modelName}:{modelVersion}` | `models/sklearn_regression_model:3` |
| `Microsoft.MachineLearningServices.ModelDeployed` | `endpoints/{serviceId}` | `endpoints/my_sklearn_aks` |
| `Microsoft.MachineLearningServices.DatasetDriftDetected` | `datadrift/{data.DataDriftId}/run/{data.RunId}` | `datadrift/4e694bf5-712e-4e40-b06a-d2a2755212d4/run/my_driftrun1_1550564444_fbbcdc0f` |
| `Microsoft.MachineLearningServices.RunStatusChanged` | `experiments/{ExperimentId}/runs/{RunId}` | `experiments/b1d7966c-f73a-4c68-b846-992ace89551f/runs/my_exp1_1554835758_38dbaa94` | 

### <a name="advanced-filtering"></a>Gelişmiş filtreleme

Azure Olay Ağış, yayımlanmış olay şemasına dayalı gelişmiş filtreleme yi de destekler. Azure Machine Learning etkinlik şeması ayrıntıları, [Azure Machine Learning için Azure Olay Ağı etkinlik şemasında](../event-grid/event-schema-machine-learning.md)bulunabilir.

Gerçekleştirebileceğiniz bazı örnek gelişmiş filtreler şunlardır:

* Olay `Microsoft.MachineLearningServices.ModelRegistered` için, modelin etiket değerini filtrelemek için:

    ```
    --advanced-filter data.ModelTags.key1 StringIn ('value1')
    ```

Filtrelerin nasıl uygulanacağı hakkında daha fazla bilgi edinmek [için Olay Izgarası için Filtre olayları'na](https://docs.microsoft.com/azure/event-grid/how-to-filter-events)bakın.

## <a name="consume-machine-learning-events"></a>Makine Öğrenimi etkinliklerini tüketin

Machine Learning etkinliklerini işleyen uygulamalar önerilen birkaç uygulamayı izlemelidir:

> [!div class="checklist"]
> * Birden çok abonelik olayları aynı olay işleyicisine yönlendirecek şekilde yapılandırılabildiği için, olayların belirli bir kaynaktan geldiğini varsaymak değil, beklediğiniz makine öğrenimi çalışma alanından geldiğinden emin olmak için iletinin konusunu denetlemek önemlidir.
> * Benzer şekilde, eventType'ın işlemeye hazır olup olmadığını denetleyin ve aldığınız tüm olayların beklediğiniz türler olacağını varsaymayın.
> * İletiler bozuk olduğundan ve biraz gecikmeden sonra, nesneler hakkındaki bilgilerinizin hala güncel olup olmadığını anlamak için etag alanlarını kullanın.  Ayrıca, belirli bir nesnedeki olayların sırasını anlamak için sıralayıcı alanlarını kullanın.
> * Anlamadığınız alanları yoksay. Bu uygulama, gelecekte eklenebilir yeni özelliklere karşı esnek tutmanıza yardımcı olacaktır.
> * Başarısız olan veya iptal edilen Azure Machine Learning işlemleri bir olayı tetiklemez. Örneğin, bir model dağıtımı başarısız olursa Microsoft.MachineLearningServices.ModelDeployed tetiklenmez. Uygulamalarınızı tasarlarken bu tür hata modunu göz önünde bulundurun. Bir işlemin durumunu kontrol etmek ve ayrıntılı hata nedenlerini anlamak için Azure Machine Learning SDK, CLI veya portalı her zaman kullanabilirsiniz.

Azure Olay İzlemi, müşterilerin Azure Machine Learning etkinlikleri tarafından tetiklenebilen çift siz ileti işleyicileri oluşturmasına olanak tanır. İleti işleyicilerinin bazı önemli örnekleri şunlardır:
* Azure İşlevleri
* Azure Logic Apps
* Azure Event Hubs
* Azure Veri Fabrikası Boru Hattı
* Azure platformunda veya başka bir yerde barındırılabilen genel webhook'lar

## <a name="next-steps"></a>Sonraki adımlar

Etkinlik Ağıt'ı hakkında daha fazla bilgi edinin ve Azure Machine Learning etkinliklerini deneyin:

- [Event Grid Hakkında](../event-grid/overview.md)
- [Azure Makine Öğrenimi için Azure Olay Izgara etkinliği şeması](../event-grid/event-schema-machine-learning.md)
- [Azure Machine Learning ile etkinlik odaklı iş akışları oluşturun](how-to-use-event-grid.md)
