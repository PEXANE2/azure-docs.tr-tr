---
title: Çalışma alanı nedir
titleSuffix: Azure Machine Learning
description: Çalışma alanı, Azure Machine Learning için en üst düzey kaynaktır. Günlükler, ölçümler, çıktı ve komut dosyalarınızın anlık görüntüsü de dahil olmak üzere tüm eğitim çalıştırmalarının geçmişini tutar. Hangi eğitim çalışmasının en iyi modeli ürettiğini belirlemek için bu bilgileri kullanırsınız
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 12/27/2019
ms.openlocfilehash: 03bc49c24e3c2d32e97f3e5e03bd39da63428a6e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77505567"
---
# <a name="what-is-an-azure-machine-learning-workspace"></a>Azure Makine Öğrenimi çalışma alanı nedir?

Çalışma alanı, Azure Machine Learning'i kullandığınızda oluşturduğunuz tüm yapılarla çalışmak için merkezi bir yer sağlayan Azure Machine Learning için en üst düzey kaynaktır.  Çalışma alanı, günlükler, ölçümler, çıktılar ve komut dosyalarınızın anlık görüntüsü de dahil olmak üzere tüm eğitim çalıştırmalarının geçmişini tutar. Bu bilgileri, hangi eğitim çalışmasının en iyi modeli ürettiğini belirlemek için kullanırsınız.  

Beğendiğiniz bir modele sahip olduğunuzda, modelini çalışma alanına kaydedersiniz. Daha sonra, Azure Kapsayıcı Örnekleri, Azure Kubernetes Hizmeti veya alan programlanabilir bir geçit dizisine (FPGA) REST tabanlı HTTP bitiş noktası olarak dağıtmak için kayıtlı modeli ve puanlama komut dosyalarını kullanırsınız. Modeli modül olarak bir Azure IoT Edge aygıtına da dağıtabilirsiniz.

Fiyatlandırma ve kullanılabilir özellikler, çalışma alanı için [Temel veya Kurumsal sürümün](overview-what-is-azure-ml.md#sku) seçilip seçilmediğine bağlıdır. [Çalışma alanını oluştururken](#create-workspace)sürümü seçersiniz.  Ayrıca Basic'ten Enterprise sürümüne [de yükseltebilirsiniz.](#upgrade)

## <a name="taxonomy"></a>Taksonomi 

Çalışma alanının taksonomisi aşağıdaki diyagramda gösterilmiştir:

[![Çalışma alanı taksonomisi](./media/concept-workspace/azure-machine-learning-taxonomy.png)](./media/concept-workspace/azure-machine-learning-taxonomy.png#lightbox)

Diyagram, çalışma alanının aşağıdaki bileşenlerini gösterir:

+ Çalışma alanı, Azure Machine Learning'i çalıştırmak için gerekli python ortamıyla yapılandırılan [Azure Machine Learning bilgi işlem örneklerini,](concept-compute-instance.md)bulut kaynaklarını içerebilir.

+ [Kullanıcı rolleri,](how-to-assign-roles.md) çalışma alanınızı diğer kullanıcılarla, ekiplerle veya projelerle paylaşmanızı sağlar.
+ [Hesaplama hedefleri,](concept-azure-machine-learning-architecture.md#compute-targets) denemelerinizi çalıştırmak için kullanılır.
+ Çalışma alanını oluşturduğunuzda, [ilişkili kaynaklar](#resources) da sizin için oluşturulur.
+ [Denemeler,](concept-azure-machine-learning-architecture.md#experiments) modellerinizi oluşturmak için kullandığınız eğitim koşularıdır.  
+ [Boru hatları,](concept-azure-machine-learning-architecture.md#ml-pipelines) modelinizi eğitmek ve yeniden eğitmek için yeniden kullanılabilir iş akışlarıdır.
+ [Veri kümeleri,](concept-azure-machine-learning-architecture.md#datasets-and-datastores) model eğitimi ve boru hattı oluşturma için kullandığınız verilerin yönetimine yardımcı olabilir.
+ Dağıtmak istediğiniz bir modele sahip olduktan sonra, kayıtlı bir model oluşturursunuz.
+ [Dağıtım bitiş noktası](concept-azure-machine-learning-architecture.md#endpoints)oluşturmak için kayıtlı modeli ve puanlama komut dosyasını kullanın.

## <a name="tools-for-workspace-interaction"></a>Çalışma alanı etkileşimi için araçlar

Çalışma alanınızla aşağıdaki yollarla etkileşim kurabilirsiniz:

+ Web'de:
    + [Azure Machine Learning stüdyosu](https://ml.azure.com) 
    + [Azure Machine Learning tasarımcısı (önizleme)](concept-designer.md) - Yalnızca [Enterprise sürümü](overview-what-is-azure-ml.md#sku) çalışma alanlarında kullanılabilir.
+ Python için Azure [Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)ile herhangi bir Python ortamında.
+ R için Azure [Machine Learning SDK](https://azure.github.io/azureml-sdk-for-r/reference/index.html)ile herhangi bir R ortamında .
+ Azure Machine Learning [CLI uzantısını](https://docs.microsoft.com/azure/machine-learning/reference-azure-machine-learning-cli) kullanarak komut satırında

## <a name="machine-learning-with-a-workspace"></a>Çalışma alanı ile makine öğrenimi

Makine öğrenimi görevleri çalışma alanınıza eserleri okur ve/veya yazar.

+ Bir modeli eğitmek için bir deneme çalıştırın - çalışma alanına deneme çalıştırma sonuçları yazar.
+ Bir modeli eğitmek için otomatik ML kullanın - çalışma alanına eğitim sonuçları yazar.
+ Çalışma alanına bir model kaydedin.
+ Bir model dağıtın - bir dağıtım oluşturmak için kayıtlı modeli kullanır.
+ Yeniden kullanılabilir iş akışları oluşturun ve çalıştırın.
+ Denemeler, boru hatları, modeller, dağıtımlar gibi makine öğrenimi yapıtlarını görüntüleyin.
+ Modelleri takip edin ve izleyin.

## <a name="workspace-management"></a>Çalışma alanı yönetimi

Ayrıca aşağıdaki çalışma alanı yönetimi görevlerini gerçekleştirebilirsiniz:

| Çalışma alanı yönetimi görevi   | Portal              | Studio | Python SDK / R SDK       | CLI        |
|---------------------------|---------|---------|------------|------------|
| Çalışma alanı oluşturma        | **&check;**     | | **&check;** | **&check;** |
| Çalışma alanı erişimini yönetme    | **&check;**   || |  **&check;**    |
| Kurumsal sürümüne yükseltme    | **&check;** | **&check;**  | |     |
| İşlem kaynakları oluşturma ve yönetme    | **&check;**   | **&check;** | **&check;** |  **&check;**   |
| Not Defteri VM Oluşturma |   | **&check;** | |     |

> [!WARNING]
> Azure Machine Learning çalışma alanınızı farklı bir aboneye taşımak veya sahip olan aboneliğiyeni bir kiracıya taşımak desteklenmez. Bunu yapmak hatalara neden olabilir.

## <a name="create-a-workspace"></a><a name='create-workspace'></a>Çalışma alanı oluşturma

Bir çalışma alanı oluşturduğunuzda, [temel veya Enterprise sürümüyle](overview-what-is-azure-ml.md#sku)mi oluşturacağına karar verirsiniz. Sürüm, çalışma alanında bulunan özellikleri belirler. Diğer özelliklerin yanı sıra, Enterprise edition [Azure Machine Learning tasarımcısına](concept-designer.md) ve [otomatik makine öğrenimi denemeleri](tutorial-first-experiment-automated-ml.md)oluşturmanın stüdyo sürümüne erişmenizi sağlar.  Daha fazla bilgi ve fiyatlandırma bilgisi için [Azure Machine Learning fiyatlandırması](https://azure.microsoft.com/pricing/details/machine-learning/)'na bakın.

Çalışma alanı oluşturmanın birden çok yolu vardır:  

* Her adımda size yol vermek için bir nokta ve tıklatma arabirimi için [Azure portalını](how-to-manage-workspace.md) kullanın.
* Python komut dosyalarından veya Jüpiter not defterlerinden anında bir çalışma alanı oluşturmak [için Python için Azure Machine Learning SDK'yı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py#workspace) kullanın
* Oluşturmayı kurumsal güvenlik standartlarıyla otomatikleştirmeniz veya özelleştirmeniz gerektiğinde [Azure Kaynak Yöneticisi şablonu](how-to-create-workspace-template.md) veya [Azure Machine Learning CLI](reference-azure-machine-learning-cli.md) şablonu kullanın.
* Visual Studio Code'da çalışıyorsanız, [VS Kodu uzantısını](tutorial-setup-vscode-extension.md)kullanın.

> [!NOTE]
> Çalışma alanı adı büyük/küçük harf duyarsız.

## <a name="upgrade-to-enterprise-edition"></a><a name="upgrade"></a>Kurumsal sürümüne yükseltme

Çalışma [alanınızı Azure portalını kullanarak Temel'den Enterprise sürümüne yükseltebilirsiniz.](how-to-manage-workspace.md#upgrade) Kurumsal sürüm çalışma alanını Temel sürüm çalışma alanına indiremezsiniz. 

## <a name="associated-resources"></a><a name="resources"></a>İlişkili kaynaklar

Yeni bir çalışma alanı oluşturduğunuzda, çalışma alanı tarafından otomatik olarak kullanılan birkaç Azure kaynağı oluşturur:

+ [Azure Kapsayıcı Kayıt Defteri](https://azure.microsoft.com/services/container-registry/): Eğitim sırasında ve bir modeli dağıtırken kullandığınız docker kapsayıcılarını kaydeder. Maliyetleri en aza indirmek için, ACR dağıtım görüntüleri oluşturulana kadar **tembel yüklenir.**
+ [Azure Depolama hesabı](https://azure.microsoft.com/services/storage/): Çalışma alanı için varsayılan veri deposu olarak kullanılır.  Azure Machine Learning bilgi işlem örneklerinizde kullanılan jupyter dizüstü bilgisayarlar da burada depolanır.
+ [Azure Uygulama Öngörüleri](https://azure.microsoft.com/services/application-insights/): Modelleriniz hakkındaki izleme bilgilerini depolar.
+ [Azure Anahtar Kasası](https://azure.microsoft.com/services/key-vault/): Bilgi işlem hedefleri ve çalışma alanının ihtiyaç duyduğu diğer hassas bilgiler tarafından kullanılan sırları saklar.

> [!NOTE]
> Yeni sürümler oluşturmanın yanı sıra, varolan Azure hizmetlerini de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning'e başlamak için bkz:

+ [Azure Machine Learning'e genel bakış](overview-what-is-azure-ml.md)
+ [Çalışma alanı oluşturma](how-to-manage-workspace.md)
+ [Çalışma alanını yönetme](how-to-manage-workspace.md)
+ [Öğretici: Python SDK ile ilk ML denemeoluşturmaya başlayın](tutorial-1st-experiment-sdk-setup.md)
+ [Öğretici: R SDK ile Azure Machine Learning ile başlayın](tutorial-1st-r-experiment.md)
+ [Öğretici: Otomatik makine öğrenimi ile ilk sınıflandırma modelinizi oluşturun](tutorial-first-experiment-automated-ml.md) (yalnızca [Enterprise edition](overview-what-is-azure-ml.md#sku) çalışma alanlarında kullanılabilir)
+ [Öğretici: Tasarımcı ile otomobil fiyatını tahmin edin](tutorial-designer-automobile-price-train-score.md) (Yalnızca [Enterprise edition](overview-what-is-azure-ml.md#sku) çalışma alanlarında kullanılabilir)
