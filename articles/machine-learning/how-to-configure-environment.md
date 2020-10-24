---
title: Geliştirme ortamını ayarlama | Python
titleSuffix: Azure Machine Learning
description: Azure Machine Learning için bir Python geliştirme ortamı ayarlamayı öğrenin. Conda ortamlarını kullanın, yapılandırma dosyaları oluşturun ve bulut tabanlı Not defteri sunucunuzu, Jupyter not defterlerini, Azure Databricks, IDEs, kod düzenleyicilerini ve Veri Bilimi Sanal Makinesi yapılandırın.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.date: 09/30/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python, contperfq1
ms.openlocfilehash: 66008012dd9b3eb6000e96f251d18cf14a9c8dc1
ms.sourcegitcommit: d6a739ff99b2ba9f7705993cf23d4c668235719f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92495241"
---
# <a name="set-up-a-development-environment-for-azure-machine-learning"></a>Azure Machine Learning için bir geliştirme ortamı ayarlama

Azure Machine Learning için bir Python geliştirme ortamı yapılandırmayı öğrenin.

Aşağıdaki tabloda, bu makalede ele alınan her geliştirme ortamı, profesyonelleri ve dezavantajlarla birlikte gösterilmektedir.

| Ortam | Avantajlar | Dezavantajlar |
| --- | --- | --- |
| [Yerel ortam](#local) | Geliştirme ortamınızın ve bağımlılıklarınızın tam denetimi. İstediğiniz herhangi bir yapı aracı, ortam veya IDE ile çalıştırın. | Kullanmaya başlamak için daha uzun sürer. Gerekli SDK paketlerinin yüklü olması ve henüz yoksa bir ortamın de yüklü olması gerekir. |
| [Azure Machine Learning işlem örneği](#compute-instance) | Başlamak için en kolay yol. Tüm SDK, çalışma alanı sanal makinenizde zaten yüklüdür ve Not defteri öğreticileri önceden klonlanır ve çalıştırılmaya hazırlanmıştır. | Geliştirme ortamınız ve bağımlılıklarınız üzerinde denetim olmaması. Linux VM için ek maliyet (sanal makine, ücretlerden kaçınmak için kullanımda olmadığında durdurulabilir). [Fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)bakın. |
| [Azure Databricks](#aml-databricks) | Ölçeklenebilir Apache Spark platformunda büyük ölçekli yoğun makine öğrenimi iş akışlarını çalıştırmak için idealdir. | Deneysel makine öğrenimi veya daha küçük ölçekli denemeleri ve iş akışları için fazla sonlandırılmalıdır. Azure Databricks için ek ücret tahakkuk ettir. [Fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/databricks/)bakın. |
| [Veri Bilimi Sanal Makinesi (DSVM)](#dsvm) | Bulut tabanlı işlem örneğine benzer şekilde (Python ve SDK önceden yüklenmiş olarak), ancak daha popüler veri bilimi ve Machine Learning araçları önceden yüklenmiş olarak. Kolayca ölçeklendirilmesi ve diğer özel araçlar ve iş akışlarıyla birleştirmek. | Bulut tabanlı işlem örneğiyle karşılaştırıldığında daha yavaş bir başlangıç deneyimi. |

Bu makalede ayrıca aşağıdaki araçlar için ek kullanım ipuçları sunulmaktadır:

* Jupyter Not defterleri: zaten Jupyter not defterlerini kullanıyorsanız SDK 'nın yüklenmesi gereken bazı ek özellikleri vardır.

* Visual Studio Code: Visual Studio Code kullanıyorsanız, [Azure Machine Learning uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai) Python için kapsamlı dil desteği ve Azure Machine Learning daha kolay ve üretken bir şekilde çalışmayı sağlayacak özellikler içerir.

## <a name="prerequisites"></a>Önkoşullar

* Azure Machine Learning çalışma alanı. Bir tane yoksa, [Azure Portal](how-to-manage-workspace.md), [Azure CLI](how-to-manage-workspace-cli.md#create-a-workspace)ve [Azure Resource Manager şablonları](how-to-create-workspace-template.md)aracılığıyla bir Azure Machine Learning çalışma alanı oluşturabilirsiniz.

### <a name="local-and-dsvm-only-create-a-workspace-configuration-file"></a><a id="workspace"></a> (Yalnızca yerel ve DSVM) Çalışma alanı yapılandırma dosyası oluşturma

Çalışma alanı yapılandırma dosyası, SDK 'nın Azure Machine Learning çalışma alanıyla nasıl iletişim kuracağını söyleyen bir JSON dosyasıdır. Dosya *config.js*olarak adlandırılır ve aşağıdaki biçime sahiptir:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Bu JSON dosyası, Python betikleri veya Jupyıter not defterlerini içeren dizin yapısında olmalıdır. Aynı dizinde, *. azureml*adlı bir alt dizin veya bir üst dizin içinde olabilir.

Bu dosyayı kodunuzda kullanmak için [`Workspace.from_config`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#from-config-path-none--auth-none---logger-none---file-name-none-&preserve-view=true) yöntemini kullanın. Bu kod, dosyadaki bilgileri yükler ve çalışma alanınıza bağlanır.

Aşağıdaki yöntemlerden birinde bir çalışma alanı yapılandırma dosyası oluşturun:

* Azure portalı

    **Dosyayı indirin**: [Azure Portal](https://ms.portal.azure.com), çalışma alanınızın **genel bakış** bölümünde **config.jsindir** ' i seçin.

    ![Azure portalı](./media/how-to-configure-environment/configure.png)

* Python SDK Azure Machine Learning

    Azure Machine Learning çalışma alanınıza bağlanmak için bir betik oluşturun ve [`write_config`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#write-config-path-none--file-name-none-&preserve-view=true) dosyanızı oluşturmak ve *. azureml/config.js*olarak kaydetmek için yöntemini kullanın. `subscription_id`,, Ve ' yi `resource_group` `workspace_name` kendi ile değiştirdiğinizden emin olun.

    ```python
    from azureml.core import Workspace

    subscription_id = '<subscription-id>'
    resource_group  = '<resource-group>'
    workspace_name  = '<workspace-name>'

    try:
        ws = Workspace(subscription_id = subscription_id, resource_group = resource_group, workspace_name = workspace_name)
        ws.write_config()
        print('Library configuration succeeded')
    except:
        print('Workspace not found')
    ```

## <a name="local-computer"></a><a id="local"></a>Yerel bilgisayar

Yerel bir geliştirme ortamı yapılandırmak için (bir Azure Machine Learning işlem örneği veya DSVM gibi uzak bir sanal makine de olabilir):

1. Python sanal ortamı (virtualenv, Conda) oluşturun.

    > [!NOTE]
    > Gerekli olmasa da, Python sanal ortamlarını yönetmek ve paketleri yüklemek için [Anaconda](https://www.anaconda.com/download/) veya [miniconda](https://www.anaconda.com/download/) kullanmanız önerilir.

    > [!IMPORTANT]
    > Linux veya macOS 'ta çalışıyorsanız ve Bash dışında bir Shell kullanıyorsanız (örneğin, ZSH), bazı komutları çalıştırdığınızda hatalar alabilirsiniz. Bu sorunu geçici olarak çözmek için, `bash` komutunu kullanarak yeni bir bash kabuğu başlatın ve komutları burada çalıştırın.

1. Yeni oluşturduğunuz Python sanal ortamınızı etkinleştirin.
1. [Azure Machine Learning Python SDK 'sını](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py&preserve-view=true)yükler.
1. Yerel ortamınızı Azure Machine Learning çalışma alanınızı kullanacak şekilde yapılandırmak için, [bir çalışma alanı yapılandırma dosyası oluşturun](#workspace) veya var olan bir dosyayı kullanın.

Yerel ortamınız ayarlamış olduğunuza göre artık Azure Machine Learning çalışmaya başlamaya hazırsınız. Başlamak için [Python başlangıç kılavuzu Azure Machine Learning](tutorial-1st-experiment-sdk-setup-local.md) bakın.

### <a name="jupyter-notebooks"></a><a id="jupyter"></a>Jupyter Notebooks

Yerel bir Jupyter Notebook sunucusu çalıştırırken, Python sanal ortamınız için bir IPython çekirdeği oluşturmanız önerilir. Bu, beklenen çekirdek ve paket içeri aktarma davranışının sağlanmasına yardımcı olur.

1. Ortama özel IPython çekirdekler 'yi etkinleştir

    ```bash
    conda install notebook ipykernel
    ```

1. Python sanal ortamınız için bir çekirdek oluşturun. Öğesini `<myenv>` Python sanal ortamınızın adıyla değiştirdiğinizden emin olun.

    ```bash
    ipython kernel install --user --name <myenv> --display-name "Python (myenv)"
    ```

1. Jupyter Notebook sunucusunu Başlat

Azure Machine Learning ve Jupyıter not defterlerini kullanmaya başlamak için [Azure Machine Learning Not defteri deposuna](https://github.com/Azure/MachineLearningNotebooks) bakın.

### <a name="visual-studio-code"></a><a id="vscode"></a>Visual Studio Code

Geliştirme için Visual Studio Code kullanmak için:

1. [Visual Studio Code](https://code.visualstudio.com/Download)'i yükler.
1. [Azure Machine Learning Visual Studio Code uzantısını](tutorial-setup-vscode-extension.md) (Önizleme) yükler.

Visual Studio Code uzantısı yüklendikten sonra, [Azure Machine Learning kaynaklarınızı](how-to-manage-resources-vscode.md)yönetebilir, [denemeleri çalıştırıp hata ayıklayın](how-to-debug-visual-studio-code.md)ve [eğitilen modeller dağıtabilirsiniz](tutorial-train-deploy-image-classification-model-vscode.md).

## <a name="azure-machine-learning-compute-instance"></a><a id="compute-instance"></a>Azure Machine Learning işlem örneği

Azure Machine Learning [işlem örneği](concept-compute-instance.md) , veri bilimcilerinin bir Jupyter Notebook sunucusu, jupi ve tam olarak yönetilen bir makine öğrenimi ortamı sağlayan, güvenli, bulut tabanlı bir Azure iş istasyonudur.

Bir işlem örneği için yüklenecek veya yapılandırılacak bir şey yok.  

Azure Machine Learning çalışma alanınızın içinden dilediğiniz zaman oluşturun. Yalnızca bir ad girin ve Azure VM türünü belirtin. Şu öğreticiyle şimdi deneyin [: Kurulum ortamı ve çalışma alanı](tutorial-1st-experiment-sdk-setup.md).

Paketlerin nasıl yükleneceğine ilişkin işlem örnekleri hakkında daha fazla bilgi edinmek için, bkz. [Azure Machine Learning işlem örneği oluşturma ve yönetme](how-to-create-manage-compute-instance.md).

> [!TIP]
> Kullanılmayan bir işlem örneği için ücretleri engellemek üzere [işlem örneğini durdurun](how-to-create-manage-compute-instance.md#manage).

Jupyter Notebook Server ve JupyterLab ' a ek olarak, [Azure Machine Learning Studio 'nun içindeki tümleşik Not defteri özelliğindeki](how-to-run-jupyter-notebooks.md)işlem örneklerini kullanabilirsiniz.

Ayrıca, [bir Azure Machine Learning işlem örneğini uzak Jupyter Notebook sunucusu olarak yapılandırmak](how-to-set-up-vs-code-remote.md#configure-compute-instance-as-remote-notebook-server)için Azure Machine Learning Visual Studio Code uzantısını da kullanabilirsiniz.

## <a name="data-science-virtual-machine"></a><a id="dsvm"></a>Veri Bilimi Sanal Makinesi

DSVM, özelleştirilmiş bir sanal makine (VM) görüntüsüdür. Şu şekilde önceden yapılandırılmış araçlar ve yazılımlar için veri bilimi çalışması için tasarlanmıştır:

  - TensorFlow, PyTorch, Scikit-öğren, XGBoost ve Azure Machine Learning SDK gibi paketler
  - Spark tek başına ve detaya gitme gibi popüler veri bilimi araçları
  - Azure CLı, AzCopy ve Depolama Gezgini gibi Azure Araçları
  - Visual Studio Code ve Pydüğme gibi tümleşik geliştirme ortamları (IDEs)
  - Jupyter Notebook sunucusu

Araçların daha kapsamlı bir listesi için [Dsvm dahil Araçlar Kılavuzu](data-science-virtual-machine/tools-included.md)' na bakın.

> [!IMPORTANT]
> DSVM 'yi eğitim veya ınişsiz işleriniz için bir [işlem hedefi](concept-compute-target.md) olarak kullanmayı planlıyorsanız yalnızca Ubuntu desteklenir.

DSVM 'yi bir geliştirme ortamı olarak kullanmak için

1. Aşağıdaki yöntemlerden birini kullanarak DSVM oluşturun:

    * [Ubuntu](data-science-virtual-machine/dsvm-ubuntu-intro.md) veya [Windows](data-science-virtual-machine/provision-vm.md) dsvm oluşturmak için Azure Portal kullanın.
    * [ARM şablonları kullanarak BIR DSVM oluşturun](data-science-virtual-machine/dsvm-tutorial-resource-manager.md).
    * Azure CLI'yi kullanma

        Ubuntu DSVM oluşturmak için aşağıdaki komutu kullanın:

        ```azurecli-interactive
        # create a Ubuntu DSVM in your resource group
        # note you need to be at least a contributor to the resource group in order to execute this command successfully
        # If you need to create a new resource group use: "az group create --name YOUR-RESOURCE-GROUP-NAME --location YOUR-REGION (For example: westus2)"
        az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:linux-data-science-vm-ubuntu:linuxdsvmubuntu:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --generate-ssh-keys --authentication-type password
        ```

        Bir Windows DSVM oluşturmak için aşağıdaki komutu kullanın:

        ```azurecli-interactive
        # create a Windows Server 2016 DSVM in your resource group
        # note you need to be at least a contributor to the resource group in order to execute this command successfully
        az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:dsvm-windows:server-2016:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --authentication-type password
        ```

1. Azure Machine Learning SDK 'sını içeren Conda ortamını etkinleştirin.

    * Ubuntu DSVM için:

        ```bash
        conda activate py36
        ```

    * Windows DSVM için:

        ```bash
        conda activate AzureML
        ```

1. DSVM 'yi Azure Machine Learning çalışma alanınızı kullanacak şekilde yapılandırmak için, [bir çalışma alanı yapılandırma dosyası oluşturun](#workspace) veya var olan bir dosyayı kullanın.

Yerel ortamlara benzer şekilde, Azure Machine Learning etkileşim kurmak için Visual Studio Code ve [Azure Machine Learning Visual Studio Code uzantısını](#vscode) kullanabilirsiniz.

Daha fazla bilgi için bkz. [veri bilimi sanal makineleri](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a name="azure-databricks"></a><a name="aml-databricks"></a> Azure Databricks

Azure Databricks, Azure bulutu 'nda Apache Spark tabanlı bir ortamdır. CPU veya GPU tabanlı işlem kümesi içeren, birlikte çalışan bir not defteri tabanlı ortam sağlar.

Azure Databricks Azure Machine Learning ile nasıl kullanılır:

+ Spark MLlib kullanarak bir modeli eğitebilirsiniz ve modeli Azure Databricks içinden ACG/AKS 'e dağıtabilirsiniz.
+ Ayrıca, Azure Databricks ile özel bir Azure ML SDK 'sında [otomatik makine öğrenimi](concept-automated-ml.md) özellikleri de kullanabilirsiniz.
+ Azure Databricks, bir [Azure Machine Learning](concept-ml-pipelines.md)işlem hattından işlem hedefi olarak kullanabilirsiniz.

### <a name="set-up-your-databricks-cluster"></a>Databricks kümenizi ayarlama

[Databricks kümesi](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal)oluşturun. Bazı ayarlar yalnızca Databricks 'te otomatik makine öğrenimi için SDK 'Yı yüklerseniz geçerlidir.
**Kümenin oluşturulması birkaç dakika sürer.**

Şu ayarları kullanın:

| Ayar |Şunlara uygulanır| Değer |
|----|---|---|
| Küme adı |Her| yourclustername |
| Databricks Çalışma Zamanı |Her|ML olmayan çalışma zamanı 7,1 (Scala 2,21, Spark 3.0.0) |
| Python sürümü |Her| 3 |
| Çalışanlarınız |Her| 2 veya üzeri |
| Çalışan düğümü VM türleri <br>(en fazla eşzamanlı yineleme sayısını belirler) |Otomatikleştirilmiş ML<br>yalnızca| Bellek için iyileştirilmiş VM tercih edilen |
| Otomatik ölçeklendirmeyi etkinleştir |Otomatikleştirilmiş ML<br>yalnızca| Kutunun |

Devam etmeden önce küme çalışmaya kadar bekleyin.

### <a name="install-the-correct-sdk-into-a-databricks-library"></a>Databricks kitaplığına doğru SDK 'Yı yükler

Küme çalışmaya başladıktan sonra uygun Azure Machine Learning SDK paketini kümenize eklemek için [bir kitaplık oluşturun](https://docs.databricks.com/user-guide/libraries.html#create-a-library) . Otomatik ML için [otomatik makine öğrenimi Ile Databricks SDK 'sına](#sdk-for-databricks-with-automated-machine-learning)atlayın.

1. Kitaplığı depolamak istediğiniz geçerli çalışma alanı klasörüne sağ tıklayın. Kitaplık **Oluştur**' u seçin  >  **Library**.

1. Aşağıdaki seçeneği seçin (diğer SDK yüklemesi desteklenmez)

   |SDK &nbsp; paketi &nbsp; ek özellikleri|Kaynak|Pypı &nbsp; adı&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
   |----|---|---|
   |Databricks için| Python Yumurg veya Pypı yükleme | azureml-SDK [databricks]|

   > [!Warning]
   > Başka SDK ek özellikleri yüklenemez. Yalnızca [ `databricks` ] seçeneğini belirleyin.

   * **Tüm kümelere otomatik olarak ekle**' yi seçmeyin.
   * Küme adınızın yanındaki  **Ekle** ' yi seçin.

1. Durum **iliştirilene**kadar, bu işlem birkaç dakika sürebilir.  Bu adım başarısız olursa:

   Kümenizi şu şekilde yeniden başlatmayı deneyin:
   1. Sol bölmede **kümeler**' ı seçin.
   1. Tabloda, küme adınızı seçin.
   1. **Kitaplıklar** sekmesinde **Yeniden Başlat**' ı seçin.

   Ayrıca şunları göz önünde bulundurun:
   + Azure Databricks kullanırken, oto ml yapılandırmasında aşağıdaki parametreleri ekleyin:
       1. ```max_concurrent_iterations``` , kümenizdeki çalışan düğümlerinin sayısına bağlıdır.
        2. ```spark_context=sc``` , varsayılan Spark bağlamını temel alır.
   + Ya da eski bir SDK sürümünüz varsa, kümenin yüklü olan kitaplıkların seçimini kaldırın ve çöp kutusuna geçiş yapın. Yeni SDK sürümünü yükleyip kümeyi yeniden başlatın. Yeniden başlatmadan sonra bir sorun varsa, kümenizi ayırın ve yeniden bağlayın.

Yüklemesi başarılı olduysa, içeri aktarılan kitaplık aşağıdakilerden biri gibi görünmelidir:

#### <a name="sdk-for-databricks"></a>Databricks için SDK
![Databricks için SDK Azure Machine Learning](./media/how-to-configure-environment/amlsdk-withoutautoml.jpg)

#### <a name="sdk-for-databricks-with-automated-machine-learning"></a>Otomatik makine öğrenimi ile Databricks için SDK
Küme, ML çalışma zamanı 7,1 veya üzeri olmayan Databricks ile oluşturulduysa, AML SDK 'sını yüklemek için not defterinizin ilk hücresinde aşağıdaki komutu çalıştırın.

```
%pip install --upgrade --force-reinstall -r https://aka.ms/automl_linux_requirements.txt
```
ML Runtime 7,0 ve daha düşük olmayan Databricks için, [Init betiğini](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks/automl/README.md)kullanarak AML SDK 'sını yüklersiniz.


### <a name="start-exploring"></a>Keşfetmeye başlayın

Deneyin:
+ Birçok örnek Not defteri kullanılabilir, **ancak [Bu örnek Not defterleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) yalnızca Azure Databricks çalışır.**

+ Bu örnekleri doğrudan çalışma alanınızdan içeri aktarın. Aşağıya bakın: içeri aktarma ![ ](./media/how-to-configure-environment/azure-db-screenshot.png)
 ![ bölmesini seçin](./media/how-to-configure-environment/azure-db-import.png)

+ [Eğitim işlemi olarak Databricks ile bir işlem hattı oluşturmayı](how-to-create-your-first-pipeline.md)öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Machine Learning model veri kümesiyle [bir modeli eğitme](tutorial-train-models-with-aml.md)
- Python başvurusu [için Azure Machine Learning SDK 'sını](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true) görüntüleme
