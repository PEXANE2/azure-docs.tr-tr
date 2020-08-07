---
title: Azure Machine Learning Enterprise ve Basic sürümleri
description: Azure Machine Learning sürümleri arasındaki farklar hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: j-martens
ms.author: jmartens
ms.date: 06/11/2020
ms.openlocfilehash: aa754868677802b7d0000045f22090fbca62d9b6
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87927428"
---
# <a name="enterprise-and-basic-editions-of-azure-machine-learning"></a>Azure Machine Learning Enterprise ve Basic sürümleri 

Azure Machine Learning, Machine Learning gereksinimleriniz için tasarlanmış iki sürüm sunar. Bu sürümler, geliştiricilerin ve veri bilimcilerinin çalışma alanından hangi makine öğrenimi araçlarının kullanılabilir olduğunu tespit.

## <a name="choose-an-edition"></a>Sürüm seçin

Her çalışma alanı oluşturduğunuzda sürümü atarsınız. Müşteriler, bu süre boyunca işlem ve diğer Azure kaynakları üzerinden oluşan maliyetlerden sorumludur. [Azure Machine Learning maliyetlerini yönetmeyi](concept-plan-manage-cost.md)öğrenin.

[Temel bir çalışma alanını Enterprise Edition 'a yükseltmeyi](how-to-manage-workspace.md#upgrade)öğrenin. 

## <a name="whats-in-each-edition"></a>Her sürümde neler vardır

### <a name="data-for-machine-learning-capabilities"></a>Machine Learning özellikleri için veriler  

| Özellikler                     | Sürüm                 |
|------------------------------------------------------------------------------------|:-----------:|
| Etiketleme: Studio 'da [etiketleme projeleri oluşturma ve yönetme](tutorial-labeling.md) (Web)                                                | Tümü                     |
| Etiketleme: Studio 'daki etiketleyici (Web)                                    | Tümü                     |
| Etiketleme: özel iş gücünü kullanın                               | Tümü                     |
| Etiketleme: [ml yardımlı görüntü sınıflandırması ve nesne algılama](how-to-label-images.md)                  | Yalnızca Enterprise Edition |
| Veri kümeleri + veri depoları: Python 'da oluşturma ve yönetme                       | Tümü                     |
| Veri kümeleri + veri depoları: Studio 'da oluşturma ve yönetme (Web)                         | Tümü                     |
| Drift: Python 'da veri kümesi izleyicileri görüntüleme ve yönetme                           | Tümü                     |
| Drift: Studio 'da veri kümesi izleyicilerini görüntüleme ve yönetme (Web)                            | Yalnızca Enterprise Edition |


<br/>
<br/>

### <a name="automated-training-capabilities-automl"></a>Otomatikleştirilmiş eğitim özellikleri (Otomatikml)

| Özellikler    | Sürüm                 |
|------------------------------------------------------------------------------------|:-----------:|
| [Not defterlerinde oto ml denemeleri](how-to-configure-auto-train.md) oluşturma ve çalıştırma               | Tümü                     |
| [Studio 'da (Web), oto ml denemeleri](how-to-use-automated-ml-for-ml-models.md) oluşturma ve çalıştırma   | Yalnızca Enterprise Edition |
| Sektör lideri Oto, tahmin özellikleri             | Yalnızca Enterprise Edition |
| Derin öğrenme ve diğer gelişmiş öğrenenler için destek | Yalnızca Enterprise Edition |
| Büyük veri desteği sınıflandırması ve gerileme görevleri (100 GB 'a kadar)                     | Yalnızca Enterprise Edition |


<br/>
<br/>

### <a name="responsible-machine-learning"></a>Sorumlu Machine Learning

| Özellikler    | Sürüm                 |
|------------------------------------------------------------------------------------|:-----------:|
| [Model explainability](how-to-machine-learning-interpretability-automl.md)                                              | Tümü                     |
| [Değişiklik gizliliği](how-to-differential-privacy.md)                          | Tümü                     |
| Veri sayfalarını uygulamak için özel Etiketler    | Tümü                     |
| Eşitliği AzureML tümleştirmesi                                      | Tümü                     |

<br/>
<br/>


### <a name="build-and-train-capabilities"></a>Oluşturma ve eğitme özellikleri

| Özellikler    | Sürüm                 |
|------------------------------------------------------------------------------------|:-----------:|
| Visual Studio Code tümleştirme                                                     | Tümü                     |
| Pekiştirmeye dayalı öğrenme                                                             | Tümü                     |
| Deneme Kullanıcı arabirimi                                                                 | Tümü                     |
| Jupyıter, Jupyıterlab tümleştirmesi                                                    | Tümü                     |
| Python SDK desteği                                                                 | Tümü                     |
| R SDK desteği                                                                      | Tümü                     |
| ML işlem hatları: Python 'da oluşturma, çalıştırma ve yayımlama                           | Tümü                     |
| ML işlem hatları: Python 'da zamanlanmış işlem hattı çalıştırmalarını oluşturma, düzenleme ve silme| Tümü                     |
| ML işlem hatları: Python SDK 'da işlem hattı uç noktaları oluşturma                                   | Tümü                     |
| ML ardışık düzenleri: Studio 'da çalıştırma ayrıntılarını görüntüleme (Web)                                              | Tümü                     |
| ML işlem hatları: tasarımcıda oluşturma, çalıştırma, görselleştirme ve yayımlama                  | Yalnızca Enterprise Edition |
| ML işlem hatları: tasarımcıda işlem hattı uç noktaları oluşturma | Yalnızca Enterprise Edition |
| Tümleşik Not defterleri için yönetilen işlem örnekleri                                 | Tümü                     |


<br/>
<br/>

### <a name="deployment-and-model-management-capabilities"></a>Dağıtım ve model yönetimi özellikleri

| Özellikler                            | Sürüm                 |
|------------------------------------------------------------------------------------|:-----------:|
| Machine Learning ve Azure ML CLI için Azure DevOps uzantısı                 | Tümü                     |
| [Event Grid tümleştirmesi](how-to-use-event-grid.md)                                                             | Tümü                     |
| Azure Stream Analytics Azure Machine Learning ile tümleştirin                       | Tümü                     |
| SDK 'da ML işlem hatları oluşturma                                                         | Tümü                     |
| Toplu iş ınkrii                                                                  | Tümü                     |
| FPGA tabanlı Hızlandırılmış Donanım Modelleri                                             | Tümü                     |
| Model profili oluşturma                                                                    | Tümü                     |

<br/>
<br/>

### <a name="security-governance-and-control-capabilities"></a>Güvenlik, idare ve denetim özellikleri

| Özellikler     | Sürüm                 |
|------------------------------------------------------------------------------------|:-----------:|
| [Azure rol tabanlı erişim denetimi (Azure RBAC)](how-to-assign-roles.md) desteği                                           | Tümü                     |
| İşlem için [sanal ağ (VNet)](how-to-enable-virtual-network.md) desteği                                         | Tümü                     |
| Puanlama uç noktası kimlik doğrulaması                                                    | Tümü                     |
| [Çalışma alanı özel bağlantısı](how-to-configure-private-link.md)                                                            | Tümü                     |
| Çalışma alanları genelinde [Kota yönetimi](how-to-manage-quotas.md)                                                 | Yalnızca Enterprise Edition |

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning [sürümüne genel bakış ve fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/machine-learning/)neler mevcuttur hakkında daha fazla bilgi edinin. 

[Temel bir çalışma alanını Enterprise Edition 'a yükseltmeyi](how-to-manage-workspace.md#upgrade)öğrenin. 
