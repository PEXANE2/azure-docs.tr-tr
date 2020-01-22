---
title: Çalışma alanı nedir?
titleSuffix: Azure Machine Learning
description: Çalışma alanı Azure Machine Learning için en üst düzey kaynaktır. Günlükler, ölçümler, çıktılar ve betiklerinizin bir anlık görüntüsü dahil olmak üzere tüm eğitim çalıştırmaları geçmişini tutar. Hangi eğitim çalışmasının en iyi modeli ürettiğini öğrenmek için bu bilgileri kullanırsınız
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 12/27/2019
ms.openlocfilehash: 4891ec0ae87f0d7b0cd7e5d5d550f955a8fde1db
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76312388"
---
# <a name="what-is-an-azure-machine-learning-workspace"></a>Azure Machine Learning çalışma alanı nedir?

Çalışma alanı, Azure Machine Learning için en üst düzey kaynaktır ve Azure Machine Learning kullandığınızda oluşturduğunuz tüm yapıtlarla çalışmak üzere merkezi bir yer sağlar.  Çalışma alanı; Günlükler, ölçümler, çıktılar ve betiklerinizin bir anlık görüntüsü dahil olmak üzere tüm eğitim çalıştırmaları geçmişini tutar. Hangi eğitim çalıştırmasının en iyi modeli ürettiğini öğrenmek için bu bilgileri kullanın.  

İstediğiniz bir modele sahip olduktan sonra, çalışma alanına kayıt yaptırmış olursunuz. Daha sonra Azure Container Instances, Azure Kubernetes hizmetine dağıtmak için kayıtlı model ve Puanlama betiklerini ve REST tabanlı HTTP uç noktası olarak bir alan ile programlanabilir kapı dizisine (FPGA) dağıtım yapmak için kullanılır. Modeli bir Azure IoT Edge cihazına modül olarak da dağıtabilirsiniz.

Fiyatlandırma ve özellikler, çalışma alanı için [temel veya Enterprise sürümünün](overview-what-is-azure-ml.md#sku) seçili olmasına bağlıdır. [Çalışma alanını oluştururken](#create-workspace)sürümü seçersiniz.  Ayrıca temel sürümünden Enterprise sürümüne [yükseltebilirsiniz](#upgrade) .

## <a name="taxonomy"></a>Sınıflandır 

Çalışma alanının bir sınıflandırması aşağıdaki diyagramda gösterilmiştir:

[![Çalışma alanı sınıflandırma](./media/concept-workspace/azure-machine-learning-taxonomy.png)](./media/concept-workspace/azure-machine-learning-taxonomy.png#lightbox)

Diyagramda bir çalışma alanının aşağıdaki bileşenleri gösterilmektedir:

+ Bir çalışma alanı, Azure Machine Learning çalıştırmak için gereken Python ortamıyla yapılandırılmış [Azure Machine Learning işlem örnekleri](concept-compute-instance.md), bulut kaynakları içerebilir.

+ [Kullanıcı rolleri](how-to-assign-roles.md) , çalışma alanınızı diğer kullanıcılar, takımlar veya projelerle paylaşmanızı sağlar.
+ [İşlem hedefleri](concept-azure-machine-learning-architecture.md#compute-targets) , denemeleri hesabınızı çalıştırmak için kullanılır.
+ Çalışma alanını oluşturduğunuzda [ilişkili kaynaklar](#resources) da sizin için oluşturulur.
+ [Denemeleri](concept-azure-machine-learning-architecture.md#experiments) , modellerinizi oluşturmak için kullandığınız eğitim çalışmalardır.  
+ İşlem [hatları](concept-azure-machine-learning-architecture.md#ml-pipelines) , modelinize eğitim ve yeniden eğitim için yeniden kullanılabilir iş akışlarıdır.
+ Veri [kümeleri](concept-azure-machine-learning-architecture.md#datasets-and-datastores) , model eğitimi ve işlem hattı oluşturma için kullandığınız verilerin yönetimine yardımcı olur.
+ Dağıtmak istediğiniz bir model olduktan sonra, kayıtlı bir model oluşturursunuz.
+ [Dağıtım uç noktası](concept-azure-machine-learning-architecture.md#endpoints)oluşturmak için kayıtlı modeli ve bir Puanlama betiğini kullanın.

## <a name="tools-for-workspace-interaction"></a>Çalışma alanı etkileşimi için Araçlar

Aşağıdaki yollarla çalışma alanım ile etkileşim kurabilirsiniz:

+ Web 'de:
    + [Azure Machine Learning Studio](https://ml.azure.com) 
    + [Azure Machine Learning Designer](concept-designer.md) -yalnızca [Enterprise Edition](overview-what-is-azure-ml.md#sku) çalışma alanlarında kullanılabilir.
+ Python [için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)olan herhangi bir Python ortamında.
+ [R için Azure Machine Learning SDK 'sı](https://azure.github.io/azureml-sdk-for-r/reference/index.html)olan herhangi bir r ortamında.
+ Azure Machine Learning [CLI uzantısını](https://docs.microsoft.com/azure/machine-learning/reference-azure-machine-learning-cli) kullanarak komut satırında

## <a name="machine-learning-with-a-workspace"></a>Çalışma alanıyla makine öğrenimi

Machine Learning görevleri çalışma alanınıza yapıları okur ve/veya yazar.

+ Model yazma denemesi çalışma alanı için bir deneme çalıştırın.
+ Model yazma eğitim sonuçlarını çalışma alanına eğitme için otomatik ML kullanın.
+ Çalışma alanına bir model kaydettirin.
+ Model dağıtma-bir dağıtım oluşturmak için kayıtlı modeli kullanır.
+ Yeniden kullanılabilir iş akışları oluşturun ve çalıştırın.
+ Denemeleri, işlem hatları, modeller, dağıtımlar gibi makine öğrenimi yapılarını görüntüleyin.
+ Modelleri izleyin ve izleyin.

## <a name="workspace-management"></a>Çalışma alanı yönetimi

Ayrıca, aşağıdaki çalışma alanı yönetim görevlerini gerçekleştirebilirsiniz:

| Çalışma alanı yönetim görevi   | Portal              | Studio | Python SDK/R SDK       | CLI        |
|---------------------------|---------|---------|------------|------------|
| Çalışma alanı oluşturma        | **&check;**     | | **&check;** | **&check;** |
| Çalışma alanı erişimini yönetme    | **&check;**   || |  **&check;**    |
| Enterprise Edition 'a yükseltme    | **&check;** | **&check;**  | |     |
| İşlem kaynaklarını oluşturma ve yönetme    | **&check;**   | **&check;** | **&check;** |  **&check;**   |
| Not defteri VM 'si oluşturma |   | **&check;** | |     |

> [!NOTE]
> İşlem örnekleri yalnızca **Orta Kuzey ABD** veya **UK Güney**bölgesi olan çalışma alanları için kullanılabilir.
>Çalışma alanınız başka bir bölgedeyse, bunun yerine bir [Not DEFTERI VM](concept-compute-instance.md#notebookvm) 'si oluşturmaya ve kullanmaya devam edebilirsiniz.

## <a name='create-workspace'></a>Çalışma alanı oluşturma

Bir çalışma alanı oluşturduğunuzda, [temel veya Enterprise sürümü](overview-what-is-azure-ml.md#sku)ile oluşturulup oluşturulmayacağını siz karar verirsiniz. Sürüm, çalışma alanında kullanılabilen özellikleri belirler. Enterprise Edition, diğer özelliklerin yanı sıra [Azure Machine Learning tasarımcı](concept-designer.md) ve [otomatik makine öğrenimi denemeleri](tutorial-first-experiment-automated-ml.md)oluşturma Studio sürümüne erişmenizi sağlar.  Daha ayrıntılı bilgi ve fiyatlandırma bilgileri için bkz. [Azure Machine Learning fiyatlandırması](https://azure.microsoft.com/pricing/details/machine-learning/).

Çalışma alanı oluşturmanın birden çok yolu vardır:  

* Her adımda size yol göstermek için bir nokta ve tıklama arabirimi için [Azure Portal](how-to-manage-workspace.md) kullanın.
* Python betikleri veya Jupiter not defterlerinden anında çalışma alanı oluşturmak için [Python için Azure Machine Learning SDK 'sını](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py#workspace) kullanın
* Şirket güvenlik standartları ile oluşturma işlemini otomatik hale getirmeniz veya özelleştirmeniz gerektiğinde bir [Azure Resource Manager şablonu](how-to-create-workspace-template.md) veya [Azure Machine Learning CLI](reference-azure-machine-learning-cli.md) kullanın.
* Visual Studio Code ' de çalışıyorsanız [vs Code uzantısını](tutorial-setup-vscode-extension.md)kullanın.

## <a name="upgrade"></a>Enterprise Edition 'a yükseltme

Azure portal kullanarak [çalışma alanınızı temel kurumsal sürüme yükseltebilirsiniz](how-to-manage-workspace.md#upgrade) . Enterprise Edition çalışma alanını temel bir sürüm çalışma alanına indirgeyemezsiniz. 

## <a name="resources"></a>İlişkili kaynaklar

Yeni bir çalışma alanı oluşturduğunuzda, çalışma alanı tarafından kullanılan bazı Azure kaynakları otomatik olarak oluşturur:

+ [Azure Container Registry](https://azure.microsoft.com/services/container-registry/): eğitim sırasında ve bir modeli dağıtırken kullandığınız Docker kapsayıcılarını kaydeder. Maliyetleri en aza indirmek için, dağıtım görüntüleri oluşturuluncaya kadar ACR **yavaş yüklenir** .
+ [Azure depolama hesabı](https://azure.microsoft.com/services/storage/): çalışma alanı için varsayılan veri deposu olarak kullanılır.  Azure Machine Learning işlem örneklerinizde kullanılan jupi Not defterleri burada da depolanır.
+ [Azure Application Insights](https://azure.microsoft.com/services/application-insights/): modelleriniz hakkında izleme bilgilerini depolar.
+ [Azure Key Vault](https://azure.microsoft.com/services/key-vault/): işlem hedefleri ve çalışma alanı için gereken diğer hassas bilgiler tarafından kullanılan gizli dizileri depolar.

> [!NOTE]
> Yeni sürümler oluşturmaya ek olarak, var olan Azure hizmetlerini de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning kullanmaya başlamak için bkz.:

+ [Azure Machine Learning genel bakış](overview-what-is-azure-ml.md)
+ [Çalışma Alanı oluşturma](how-to-manage-workspace.md)
+ [Çalışma Alanını Yönetme](how-to-manage-workspace.md)
+ [Öğretici: Python SDK ile ilk ML denemenizi oluşturmaya başlama](tutorial-1st-experiment-sdk-setup.md)
+ [Öğretici: R SDK ile Azure Machine Learning kullanmaya başlama](tutorial-1st-r-experiment.md)
+ [Öğretici: otomatik makine öğrenimi ile ilk sınıflandırma modelinizi oluşturma](tutorial-first-experiment-automated-ml.md) (yalnızca [Enterprise Edition](overview-what-is-azure-ml.md#sku) çalışma alanlarında kullanılabilir)
+ [Öğretici: tasarımcı ile otomobil fiyatını tahmin](tutorial-designer-automobile-price-train-score.md) etme (yalnızca [Enterprise Edition](overview-what-is-azure-ml.md#sku) çalışma alanlarında kullanılabilir)
