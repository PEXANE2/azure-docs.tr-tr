---
title: Çalışma alanı oluşturma
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti çalışma alanınızı oluşturmak için Azure portal, SDK, bir şablon veya CLı kullanın. Bu çalışma alanı, Azure Machine Learning hizmeti kullandığınızda oluşturduğunuz tüm yapıtlarla çalışmak üzere merkezi bir yer sağlar.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.author: sgilley
author: sdgilley
ms.date: 05/21/2019
ms.openlocfilehash: 4d689b51a53a27a0e85a52724752d959c4c2506d
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2019
ms.locfileid: "68775058"
---
# <a name="create-an-azure-machine-learning-service-workspace"></a>Azure Machine Learning hizmet çalışma alanı oluşturma

Azure Machine Learning hizmetini kullanmak için bir [**Azure Machine Learning hizmeti çalışma alanına**](concept-workspace.md)ihtiyacınız vardır.  Bu çalışma alanı, hizmet için en üst düzey kaynaktır ve oluşturduğunuz tüm yapıtlarla çalışmak üzere merkezi bir yer sağlar. 

Bu makalede, aşağıdaki yöntemlerden herhangi birini kullanarak bir çalışma alanı oluşturmayı öğreneceksiniz: 
* [Azure Portal](#portal) arabirimi
* [Python için Azure Machine Learning SDK 'sı](#sdk)
* Azure Resource Manager şablonu
* [Azure MACHINE LEARNING CLI](#cli)

Burada bulunan adımları kullanarak oluşturduğunuz çalışma alanı, diğer öğreticiler ve nasıl yapılır makaleleri için bir ön koşul olarak kullanılabilir.

Yerel bir Python ortamında otomatik makine öğrenimi ayarlamak için bir komut dosyası kullanmak istiyorsanız lütfen yönergeler için [Azure/Machinelearningnot defterleri GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning) ' a bakın.  

Bir çalışma alanı oluşturduğunuzda, aşağıdaki Azure kaynakları otomatik olarak eklenir (bölgesel olarak kullanılabiliyorsa):
 
- [Azure Container Registry](https://azure.microsoft.com/services/container-registry/): Maliyetleri en aza indirmek için, dağıtım görüntüleri oluşturuluncaya kadar ACR **yavaş yüklenir** .
- [Azure Depolama](https://azure.microsoft.com/services/storage/)
- [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
- [Azure Anahtar Kasası.](https://azure.microsoft.com/services/key-vault/)

>[!Note]
>Diğer Azure hizmetlerinde olduğu gibi, belirli sınırlar ve Kotalar Machine Learning ilişkilendirilir. [Kotalar ve artış talebinde bulunma hakkında bilgi edinin.](how-to-manage-quotas.md)


## <a name="prerequisites"></a>Önkoşullar
Bir çalışma alanı oluşturmak için bir Azure aboneliğinizin olması gerekir. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

## <a name="portal"></a>Azure portal

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

Nasıl oluşturulduğuna bakılmaksızın, çalışma alanınızı [Azure Portal](https://portal.azure.com/)görüntüleyebilirsiniz.  Ayrıntılar için bkz. [çalışma alanını görüntüleme](how-to-manage-workspace.md#view) .

## <a name="sdk"></a>Python SDK 'Sı

Python SDK 'sını kullanarak çalışma alanınızı oluşturun. Öncelikle SDK 'Yı yüklemeniz gerekir.

> [!IMPORTANT]
> Azure Veri Bilimi Sanal Makinesi veya Azure Databricks kullanıyorsanız SDK yüklemesini atlayın.
> * 27 Eylül 2018 ' den sonra oluşturulan Azure veri bilimi sanal makineleri, Python SDK 'sının önceden yüklenmiş olarak gelir. Yüklemeyi atlayın ve [SDK ile çalışma alanı oluşturma](#sdk-create)ile başlayın.
> * Azure Databricks ortamında, bunun yerine [Databricks yükleme adımlarını](how-to-configure-environment.md#azure-databricks) kullanın.

>[!NOTE]
> SDK 'Yı yerel bilgisayarınızdan yüklemek ve kullanmak için bu yönergeleri kullanın. Jupi 'yi uzak bir sanal makinede kullanmak için, uzak masaüstü veya X terminal oturumu ayarlayın.

SDK'yı yüklemeden yalıtılmış bir Python ortamı oluşturmanızı öneririz. Bu makalede [Miniconda](https://docs.conda.io/en/latest/miniconda.html)kullanılıyorsa, tam [Anaconda](https://www.anaconda.com/) yüklü veya [Python virtualalenv](https://virtualenv.pypa.io/en/stable/)de kullanabilirsiniz.

Bu makaledeki yönergeler, hızlı başlangıç ve öğretici not defterlerini çalıştırmak için ihtiyacınız olan tüm paketleri yükler.  Diğer örnek Not defterleri, ek bileşenlerin yüklenmesini gerektirebilir.  Bu bileşenler hakkında daha fazla bilgi için bkz. [Python için Azure Machine Learning SDK 'Sını yüklemeyin](https://docs.microsoft.com/python/api/overview/azure/ml/install).

### <a name="install-miniconda"></a>Miniconda'yı yükleme

[Miniconda indirin ve yükleyin](https://docs.conda.io/en/latest/miniconda.html). Yüklenecek Python 3,7 sürümünü seçin. Python 2.x sürümünü seçmeyin.  

### <a name="create-an-isolated-python-environment"></a>Yalıtılmış Python ortamı oluşturma

1. Anaconda Prompt ' ı açın ve ardından *MyENV* adlı yeni bir Conda ortamı oluşturun ve Python 3.6.5 ' yi kurun. Azure Machine Learning SDK, Python 3.5.2 veya sonraki sürümleriyle çalışır, ancak otomatik makine öğrenimi bileşenleri Python 3,7 ' de tamamen işlevsel değildir.  Bileşenler ve paketler indirilirken ortamın oluşturulması birkaç dakika sürer. 

    ```shell
    conda create -n myenv python=3.6.5
    ```

1. Ortamı etkinleştirin.

    ```shell
    conda activate myenv
    ```

1. Ortama özel IPython kernels 'yi etkinleştir:

    ```shell
    conda install notebook ipykernel
    ```

    Ardından çekirdeği oluşturun:

    ```shell
    ipython kernel install --user
    ```

### <a name="install-the-sdk"></a>SDK yükle

1. Etkinleştirilen Conda ortamında, Jupyter Not defteri özellikleri ile Machine Learning SDK 'nın temel bileşenlerini yükler. Yükleme işleminin, makinenizin yapılandırmasına bağlı olarak birkaç dakika sürer.

    ```shell
    pip install --upgrade azureml-sdk[notebooks]
    ```

1. Bu ortamı Azure Machine Learning öğreticileri için kullanmak üzere bu paketleri yüklemek için.

    ```shell
    conda install -y cython matplotlib pandas
    ```

1. Azure Machine Learning öğreticileri için bu ortamı kullanmak üzere, otomatik makine öğrenimi bileşenlerini yükler.

    ```shell
    pip install --upgrade azureml-sdk[automl]
    ```

> [!IMPORTANT]
> Bazı komut satırı araçlarında, tırnak işaretlerini aşağıdaki şekilde eklemeniz gerekebilir:
> *  ' azureml-SDK [Not defterleri] '
> * ' azureml-SDK [Oto ml] '
>

### <a name='sdk-create'></a>SDK ile çalışma alanı oluşturma

Python SDK 'sını kullanarak bir Jupyter Notebook çalışma alanınızı oluşturun.

1. Hızlı başlangıç ve öğreticiler için kullanmak istediğiniz dizine ve/veya CD 'yi oluşturun.

1. Jupyter Notebook başlatmak için şu komutu girin:

    ```shell
    jupyter notebook
    ```

1. Tarayıcı penceresinde varsayılan `Python 3` çekirdeğini kullanarak yeni bir notebook oluşturun. 

1. SDK sürümünü göstermek için bir not defteri hücresinde aşağıdaki python kodunu girin ve ardından yürütün:

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=import)]

1. `<azure-subscription-id>` [Azure Portal abonelikler listesinde](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)parametre için bir değer bulun. Sahip veya katkıda bulunan rolüne sahip olduğunuz herhangi bir aboneliği kullanabilirsiniz. Roller hakkında daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanına erişimi yönetme](how-to-assign-roles.md) .

   ```python
   from azureml.core import Workspace
   ws = Workspace.create(name='myworkspace',
                         subscription_id='<azure-subscription-id>', 
                         resource_group='myresourcegroup',
                         create_resource_group=True,
                         location='eastus2' 
                        )
   ```

   Kodu yürüttüğünüzde, Azure hesabınızda oturum açmanız istenebilir. Oturum açtıktan sonra kimlik doğrulama belirteci yerel önbelleğe alınır.

1. İlişkili depolama, kapsayıcı kayıt defteri ve anahtar kasası gibi çalışma alanı ayrıntılarını görüntülemek için aşağıdaki kodu girin:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=getDetails)]


### <a name="write-a-configuration-file"></a>Yapılandırma dosyası yazma

Çalışma alanınızın ayrıntılarını bir yapılandırma dosyasına geçerli dizine kaydedin. Bu dosya *. azureml/config. JSON*olarak adlandırılır.  

Bu çalışma alanı yapılandırma dosyası, daha sonra aynı çalışma alanını yüklemeyi kolaylaştırır. Kodu `ws=Workspace.from_config()` kullanarak aynı dizinde veya bir alt dizinde bulunan diğer Not defterleri ve betiklerle yükleyebilirsiniz. 

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=writeConfig)]

Bu `write_config()` API çağrısı geçerli dizinde yapılandırma dosyası oluşturur. *. Azureml/config. JSON* dosyası şunları içerir:

```json
{
    "subscription_id": "<azure-subscription-id>",
    "resource_group": "myresourcegroup",
    "workspace_name": "myworkspace"
}
```

> [!TIP]
> Çalışma alanınızı, diğer dizinlerde bulunan Python betiklerine veya Jupyıter not defterlerine kullanmak için bu dosyayı bu dizine kopyalayın. Dosya aynı dizinde, *. azureml*adlı bir alt dizin veya bir üst dizin içinde olabilir.

## <a name="resource-manager-template"></a>Resource Manager şablonu

Şablon ile bir çalışma alanı oluşturmak için bkz. [şablon kullanarak Azure Machine Learning hizmet çalışma alanı oluşturma](how-to-create-workspace-template.md)

<a name="cli"></a>
## <a name="command-line-interface"></a>Komut satırı arabirimi

CLı ile bir çalışma alanı oluşturmak için bkz. [Azure Machine Learning hizmeti IÇIN CLI uzantısını kullanma](reference-azure-machine-learning-cli.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Nasıl oluşturulduğuna bakılmaksızın, çalışma alanınızı [Azure Portal](https://portal.azure.com/)görüntüleyebilirsiniz.  Ayrıntılar için bkz. [çalışma alanını görüntüleme](how-to-manage-workspace.md#view) .

* Bu öğreticilerle çalışma alanınızı deneyin.

    * İki bölümden oluşan öğretici: [Ortamı ve çalışma alanını kurun](tutorial-1st-experiment-sdk-setup.md) ve [İlk modelinize eğitme](tutorial-1st-experiment-sdk-train.md).
    * İki bölümden oluşan öğretici: Görüntü sınıflandırma modunu [eğitme](tutorial-train-models-with-aml.md) ve [dağıtma](tutorial-deploy-models-with-aml.md) .
    * İki bölümden oluşan öğretici: Bir gerileme modeli oluşturmak için [verileri hazırlayın](tutorial-data-prep.md) ve [otomatik makine öğrenimi kullanın](tutorial-auto-train-models.md) .

* [Geliştirme ortamının nasıl yapılandırılacağı](how-to-configure-environment.md)hakkında daha fazla bilgi edinin.

* [Python için Azure MACHINE LEARNING SDK](https://aka.ms/aml-sdk)hakkında daha fazla bilgi edinin.
