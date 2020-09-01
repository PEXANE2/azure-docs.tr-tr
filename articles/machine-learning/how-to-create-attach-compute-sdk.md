---
title: Python SDK ile işlem kaynakları oluşturma
titleSuffix: Azure Machine Learning
description: Makine öğrenimi için eğitim ve dağıtım işlem kaynakları (işlem hedefleri) oluşturmak için Python SDK Azure Machine Learning kullanın
services: machine-learning
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.date: 07/08/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python, contperfq1
ms.openlocfilehash: 96aa6839fe51bb8a8c26f411c1a1f9df6b8c5a7f
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89147627"
---
# <a name="create-compute-targets-for-model-training-and-deployment-with-python-sdk"></a>Python SDK ile model eğitimi ve dağıtımı için işlem hedefleri oluşturma

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, işlem hedeflerini oluşturmak ve yönetmek için Python SDK Azure Machine Learning kullanın. Ayrıca, ile işlem hedefleri oluşturup bunları yönetebilirsiniz:
* [Azure Machine Learning Studio](how-to-create-attach-compute-studio.md), 
* Azure Machine Learning için [CLI uzantısı](reference-azure-machine-learning-cli.md#resource-management)
* Azure Machine Learning için [vs Code uzantısı](how-to-manage-resources-vscode.md#compute-clusters) .


## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin
* [Python için Azure Machine Learning SDK 'sı](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
* [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md)

## <a name="whats-a-compute-target"></a>İşlem hedefi nedir?

Azure Machine Learning, modelinizi toplu olarak [__işlem hedefleri__](concept-azure-machine-learning-architecture.md#compute-targets)olarak adlandırılan çeşitli kaynaklar veya ortamlar üzerinde eğitebilirsiniz. İşlem hedefi bir yerel makine veya Azure Machine Learning Işlem, Azure HDInsight veya uzak bir sanal makine gibi bir bulut kaynağı olabilir.  Model dağıtımı için, ["modellerinizi dağıtma"](how-to-deploy-and-where.md)bölümünde açıklandığı gibi işlem hedefleri de oluşturabilirsiniz.

## <a name="compute-targets-for-training"></a>Eğitim için işlem hedefleri

Azure Machine Learning, farklı işlem hedefleri arasında değişen desteğe sahiptir. Tipik bir model geliştirme yaşam döngüsü, düşük miktarda veri üzerinde dev/deneme ile başlar. Bu aşamada, yerel bir ortam kullanmanızı öneririz. Örneğin, Yerel bilgisayarınız veya bulut tabanlı bir VM. Daha büyük veri kümelerinde eğitime göre ölçeklendirirken veya dağıtılmış eğitim yaparken, her çalıştırma gönderdiğinizde otomatik olarak ölçeklenen tek veya çok düğümlü bir küme oluşturmak için Azure Machine Learning Işlem kullanmanızı öneririz. Ayrıca, çeşitli senaryolar için destek aşağıda ayrıntılı şekilde değişiklik gösterebilse de kendi işlem kaynağınızı ekleyebilirsiniz:

[!INCLUDE [aml-compute-target-train](../../includes/aml-compute-target-train.md)]

> [!NOTE]
> Azure Machine Learning Işlem kümeleri, kalıcı bir kaynak olarak oluşturulabilir veya bir çalıştırma istediğinizde dinamik olarak oluşturulabilir. Çalışma tabanlı oluşturma, eğitim çalıştırması tamamlandıktan sonra işlem hedefini kaldırır, bu nedenle bu şekilde oluşturulan işlem hedeflerini yeniden kullanamazsınız.

Bu işlem hedeflerini yapılandırmak için aşağıdaki bölümleri kullanın:

* [Yerel bilgisayar](#local)
* [Azure Machine Learning işlem kümesi](#amlcompute)
* [Azure Machine Learning işlem örneği](#instance)
* [Uzak sanal makineler](#vm)
* [Azure HDInsight](#hdinsight)


## <a name="local-computer"></a><a id="local"></a>Yerel bilgisayar

Eğitim için yerel bilgisayarınızı kullandığınızda bir işlem hedefi oluşturmanız gerekmez.  Yalnızca yerel makinenizden [eğitim çalıştırmasını göndermeniz](how-to-set-up-training-targets.md) yeterlidir.

## <a name="azure-machine-learning-compute-cluster"></a><a id="amlcompute"></a>Azure Machine Learning işlem kümesi

Azure Machine Learning işlem kümesi, kolayca tek veya çok düğümlü bir işlem oluşturmanıza olanak sağlayan bir yönetilen işlem altyapısıdır. İşlem, çalışma alanınızdaki diğer kullanıcılarla paylaşılabilecek bir kaynak olarak çalışma alanı bölgeniz içinde oluşturulur. İşlem, bir iş gönderildiğinde otomatik olarak ölçeklendirilir ve bir Azure sanal ağına yerleştirilebilir. İşlem kapsayıcılı bir ortamda yürütülür ve model bağımlılıklarınızı bir [Docker kapsayıcısında](https://www.docker.com/why-docker)paketleyebilir.

Eğitim sürecini buluttaki bir CPU veya GPU işlem düğümleri kümesi arasında dağıtmak için Azure Machine Learning Işlem kullanabilirsiniz. GPU 'ları içeren VM boyutları hakkında daha fazla bilgi için bkz. [GPU ile iyileştirilmiş sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu). 

Azure Machine Learning Işlem, ayrılabilen çekirdek sayısı gibi varsayılan sınırlara sahiptir. Daha fazla bilgi için bkz. [Azure kaynakları için kotaları yönetme ve isteme](how-to-manage-quotas.md).

> [!TIP]
> Gereken çekirdek sayısı için yeterli kotanın olması koşuluyla, kümeler genellikle 100 düğüme kadar ölçeklendirebilir. Varsayılan olarak kümeler, MPı işlerini desteklemek üzere küme düğümleri arasında etkinleştirilen düğümler arası iletişim ile ayarlanır. Ancak, [bir destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)oluşturarak ve abonelik veya çalışma alanınızı ya da düğümler arası iletişimi devre dışı bırakmaya yönelik belirli bir kümeyi listelemek istiyorsanız kümelerinizi 1000 ' e ölçeklendirebilirsiniz. 

Azure Machine Learning Işlem, çalıştırmalar arasında yeniden kullanılabilir. İşlem, çalışma alanındaki diğer kullanıcılarla paylaşılabilir ve çalıştırmalar arasında korunur, gönderilen çalışma sayısına ve kümenizde ayarlanan max_nodes göre otomatik olarak düğümleri yukarı veya aşağı ölçeklendirin. Min_nodes ayarı kullanılabilir en düşük düğümleri denetler.

[!INCLUDE [min-nodes-note](../../includes/machine-learning-min-nodes.md)]

1. **Oluşturma ve iliştirme**: Python 'da kalıcı bir Azure Machine Learning işlem kaynağı oluşturmak için **vm_size** ve **max_nodes** özelliklerini belirtin. Azure Machine Learning daha sonra diğer özellikler için akıllı Varsayılanları kullanır. İşlem, kullanılmazsa otomatik olarak sıfır düğümlere ölçeklenir.   İşlerinizi gerektiğinde çalıştırmak için adanmış VM 'Ler oluşturulur.
    
    * **vm_size**: Azure Machine Learning işlem tarafından oluşturulan düğümlerin VM ailesi.
    * **max_nodes**: Azure Machine Learning işlem sırasında bir işi çalıştırdığınızda otomatik olarak en fazla düğüm sayısı.
    
   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=cpu_cluster)]

   Ayrıca, Azure Machine Learning Işlem oluştururken birçok gelişmiş özelliği de yapılandırabilirsiniz. Özellikler, sabit boyutlu kalıcı bir küme oluşturmanıza veya mevcut bir Azure Sanal Ağa gelen aboneliğinizden bu.  Ayrıntılar için [Amlcompute sınıfına](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py
    ) bakın.

    Ya da [Azure Machine Learning Studio](how-to-create-attach-compute-studio.md#portal-create)'da kalıcı bir Azure Machine Learning işlem kaynağı oluşturup ekleyebilirsiniz.

Bu işlemi yaptıktan sonra, bir sonraki adım [eğitim çalıştırmasını göndermektedir](how-to-set-up-training-targets.md).

 ### <a name="lower-your-compute-cluster-cost"></a><a id="low-pri-vm"></a> İşlem kümesi maliyetinizi düşürün

İş yüklerinizin bazılarını veya tümünü çalıştırmak için [düşük öncelikli VM 'ler](concept-plan-manage-cost.md#low-pri-vm) kullanmayı da tercih edebilirsiniz. Bu VM 'Lerin garantili kullanılabilirliği yoktur ve kullanımda olabilir. Geçersiz hale başlatılan iş yeniden başlatıldı, sürdürülmez. 

Düşük öncelikli bir VM belirtmek için şu yolların herhangi birini kullanın:
    
* Studio 'da, bir VM oluştururken **düşük öncelik** ' i seçin.
    
* Python SDK ile, `vm_priority` sağlama yapılandırmanızda özniteliğini ayarlayın.  
    
    ```python
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                                vm_priority='lowpriority',
                                                                max_nodes=4)
    ```
    
* CLı 'yi kullanarak şunları ayarlayın `vm-priority` :
    
    ```azurecli-interactive
    az ml computetarget create amlcompute --name lowpriocluster --vm-size Standard_NC6 --max-nodes 5 --vm-priority lowpriority
    ```

 ### <a name="set-up-managed-identity"></a><a id="managed-identity"></a> Yönetilen kimliği ayarlama

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-managed-identity-intro.md)]

Yönetilen kimliği, sağlama yapılandırmanızda yapılandırın:  
    
* Sistem tarafından atanan yönetilen kimlik:
    ```python
    # configure cluster with a system-assigned managed identity
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                            max_nodes=5,
                                                            identity_type="SystemAssigned",
                                                            )
    ```

* Kullanıcı tarafından atanan yönetilen kimlik: 

    ```python
    # configure cluster with a user-assigned managed identity
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                            max_nodes=5,
                                                            identity_type="UserAssigned",
                                                            identity_id=['/subscriptions/<subcription_id>/resourcegroups/<resource_group>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<user_assigned_identity>'])

    cpu_cluster_name = "cpu-cluster"
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)
    ```

Yönetilen kimliği mevcut bir işlem kümesine ekleyin:  
    
* Sistem tarafından atanan yönetilen kimlik:

    ```python
    # add a system-assigned managed identity
    cpu_cluster.add_identity(identity_type="SystemAssigned")
    ````

* Kullanıcı tarafından atanan yönetilen kimlik:

    ```python
    # add a user-assigned managed identity
    cpu_cluster.add_identity(identity_type="UserAssigned", 
                                identity_id=['/subscriptions/<subcription_id>/resourcegroups/<resource_group>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<user_assigned_identity>'])
    ```

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-managed-identity-note.md)]

#### <a name="managed-identity-usage"></a>Yönetilen kimlik kullanımı

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-managed-identity-default.md)]


## <a name="azure-machine-learning-compute-instance"></a><a id="instance"></a>Azure Machine Learning işlem örneği

[Azure Machine Learning işlem örneği](concept-compute-instance.md) , kolayca tek bir VM oluşturmanıza olanak sağlayan bir yönetilen işlem altyapısıdır. İşlem, çalışma alanı bölgeniz içinde oluşturulur, ancak bir işlem kümesinden farklı olarak, bir örnek çalışma alanınızdaki diğer kullanıcılarla paylaşılamaz. Ayrıca, örnek otomatik olarak ölçeklenmez.  Devam eden ücretleri engellemek için kaynağı durdurmanız gerekir.

Bir işlem örneği, paralel olarak birden çok iş çalıştırabilir ve bir iş kuyruğuna sahiptir. 

İşlem örnekleri, kuruluşların SSH bağlantı noktalarını açmasına gerek kalmadan, işleri bir [sanal ağ ortamında](how-to-enable-virtual-network.md#compute-instance)güvenli bir şekilde çalıştırabilir. İş kapsayıcılı bir ortamda yürütülür ve model bağımlılıklarınızı bir Docker kapsayıcısında paketleyebilir. 

1. **Oluşturma ve iliştirme**: 
    
    ```python
    import datetime
    import time
    
    from azureml.core.compute import ComputeTarget, ComputeInstance
    from azureml.core.compute_target import ComputeTargetException
    
    # Choose a name for your instance
    # Compute instance name should be unique across the azure region
    compute_name = "ci{}".format(ws._workspace_id)[:10]
    
    # Verify that instance does not exist already
    try:
        instance = ComputeInstance(workspace=ws, name=compute_name)
        print('Found existing instance, use it.')
    except ComputeTargetException:
        compute_config = ComputeInstance.provisioning_configuration(
            vm_size='STANDARD_D3_V2',
            ssh_public_access=False,
            # vnet_resourcegroup_name='<my-resource-group>',
            # vnet_name='<my-vnet-name>',
            # subnet_name='default',
            # admin_user_ssh_public_key='<my-sshkey>'
        )
        instance = ComputeInstance.create(ws, compute_name, compute_config)
        instance.wait_for_completion(show_output=True)
    ```

Bu işlemi yaptıktan sonra, çalışmanızı yapılandırdığınıza göre, bir sonraki adım [eğitim çalıştırmasını gönderdiniz](how-to-set-up-training-targets.md)


## <a name="remote-virtual-machines"></a><a id="vm"></a>Uzak sanal makineler

Azure Machine Learning Ayrıca kendi işlem kaynağınızı getirme ve çalışma alanınıza ekleme desteği de sağlar. Bu tür bir kaynak türü, Azure Machine Learning 'ten erişilebilen sürece rastgele bir uzak VM 'dir. Kaynak bir Azure VM 'si, kuruluşunuzdaki uzak sunucu veya şirket içi olabilir. Özellikle, IP adresi ve kimlik bilgileri (Kullanıcı adı ve parola veya SSH anahtarı) verildiğinde, uzak çalıştırmalar için erişilebilir VM 'leri kullanabilirsiniz.

Sistemde oluşturulmuş bir Conda ortamı, zaten varolan bir Python ortamı veya Docker kapsayıcısı kullanabilirsiniz. Bir Docker kapsayıcısında yürütmek için, VM 'de çalışan bir Docker altyapısına sahip olmanız gerekir. Bu işlevsellik, yerel makinenize kıyasla daha esnek, bulut tabanlı bir geliştirme/deneme ortamı istediğinizde özellikle yararlıdır.

Bu senaryo için tercih edilen Azure sanal makinesi olarak Azure Veri Bilimi Sanal Makinesi (DSVM) kullanın. Bu VM, Azure 'da önceden yapılandırılmış bir veri bilimi ve AI geliştirme ortamıdır. VM, tam yaşam döngüsü makine öğrenimi geliştirmesi için seçkin bir araç ve çerçeve seçeneği sunar. DSVM 'nin Azure Machine Learning ile nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [bir geliştirme ortamı yapılandırma](https://docs.microsoft.com/azure/machine-learning/how-to-configure-environment#dsvm).

1. **Oluştur**: modelinize eğitebilmeniz için kullanmadan önce dsvm oluşturun. Bu kaynağı oluşturmak için bkz. [Linux için veri bilimi sanal makinesi sağlama (Ubuntu)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro).

    > [!WARNING]
    > Azure Machine Learning yalnızca **Ubuntu**çalıştıran sanal makineleri destekler. Bir VM oluşturduğunuzda veya var olan bir VM 'yi seçtiğinizde Ubuntu kullanan bir VM seçmeniz gerekir.
    > 
    > Azure Machine Learning Ayrıca, sanal makinenin __Genel BIR IP adresine__sahip olmasını gerektirir.

1. **Ekle**: var olan bir sanal makineyi bir işlem hedefi olarak eklemek için, sanal makine IÇIN kaynak kimliği, Kullanıcı adı ve parola sağlamanız gerekir. VM 'nin kaynak KIMLIĞI, aşağıdaki dize biçimi kullanılarak abonelik KIMLIĞI, kaynak grubu adı ve VM adı kullanılarak oluşturulabilir: `/subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Compute/virtualMachines/<vm_name>`

 
   ```python
   from azureml.core.compute import RemoteCompute, ComputeTarget

   # Create the compute config 
   compute_target_name = "attach-dsvm"
   
   attach_config = RemoteCompute.attach_configuration(resource_id='<resource_id>',
                                                   ssh_port=22,
                                                   username='<username>',
                                                   password="<password>")

   # Attach the compute
   compute = ComputeTarget.attach(ws, compute_target_name, attach_config)

   compute.wait_for_completion(show_output=True)
   ```

   Veya [Azure Machine Learning Studio 'yu kullanarak](how-to-create-attach-compute-studio.md#attached-compute)dsvm 'yi çalışma alanınıza ekleyebilirsiniz.

1. **Yapılandır**: dsvm işlem hedefi için bir çalıştırma yapılandırması oluşturun. Docker ve Conda, DSVM 'de Eğitim ortamı oluşturmak ve yapılandırmak için kullanılır.

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/dsvm.py?name=run_dsvm)]


Bu işlemi yaptıktan sonra, çalışmanızı yapılandırdığınıza göre, sonraki adım [eğitim çalıştırmasını göndermektedir](how-to-set-up-training-targets.md).

## <a name="azure-hdinsight"></a><a id="hdinsight"></a>Azure HDInsight 

Azure HDInsight, büyük veri analizi için popüler bir platformdur. Platform, modelinizi eğitebilmeniz için kullanılabilecek Apache Spark sağlar.

1. **Oluştur**: modelinize eğitebilmeniz Için önce HDInsight kümesini oluşturun. HDInsight kümesinde Spark oluşturmak için bkz. [HDInsight 'Ta Spark kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql). 

    > [!WARNING]
    > Azure Machine Learning, HDInsight kümesinin __Genel BIR IP adresine__sahip olmasını gerektirir.

    Kümeyi oluşturduğunuzda bir SSH Kullanıcı adı ve parolası belirtmeniz gerekir. HDInsight 'ı bir işlem hedefi olarak kullanmak için ihtiyacınız olduğu için bu değerleri de göz önünde ayırın.
    
    Küme oluşturulduktan sonra, ana bilgisayar adı \<clustername> -SSH.azurehdinsight.NET ile bağlanın, burada \<clustername> küme için verdiğiniz addır. 

1. **İliştirme**: bir HDInsight kümesini işlem hedefi olarak eklemek için HDInsight kümesi IÇIN kaynak kimliği, Kullanıcı adı ve parola sağlamalısınız. HDInsight kümesinin kaynak KIMLIĞI, aşağıdaki dize biçimi kullanılarak abonelik KIMLIĞI, kaynak grubu adı ve HDInsight kümesi adı kullanılarak oluşturulabilir: `/subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.HDInsight/clusters/<cluster_name>`

    ```python
   from azureml.core.compute import ComputeTarget, HDInsightCompute
   from azureml.exceptions import ComputeTargetException

   try:
    # if you want to connect using SSH key instead of username/password you can provide parameters private_key_file and private_key_passphrase

    attach_config = HDInsightCompute.attach_configuration(resource_id='<resource_id>',
                                                          ssh_port=22, 
                                                          username='<ssh-username>', 
                                                          password='<ssh-pwd>')
    hdi_compute = ComputeTarget.attach(workspace=ws, 
                                       name='myhdi', 
                                       attach_configuration=attach_config)

   except ComputeTargetException as e:
    print("Caught = {}".format(e.message))

   hdi_compute.wait_for_completion(show_output=True)
   ```

   İsterseniz de [Azure Machine Learning Studio kullanarak](how-to-create-attach-compute-studio.md#attached-compute)HDInsight kümesini çalışma alanınıza ekleyebilirsiniz.

1. **Yapılandır**: HDI işlem hedefi için bir çalıştırma yapılandırması oluşturun. 

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/hdi.py?name=run_hdi)]


Bu işlemi yaptıktan sonra, çalışmanızı yapılandırdığınıza göre, sonraki adım [eğitim çalıştırmasını göndermektedir](how-to-set-up-training-targets.md).

## <a name="azure-batch"></a><a id="azbatch"></a>Azure Batch 

Azure Batch, büyük ölçekli paralel ve yüksek performanslı bilgi işlem (HPC) uygulamalarını bulutta verimli bir şekilde çalıştırmak için kullanılır. AzureBatchStep bir makine havuzuna Azure Batch iş göndermek için bir Azure Machine Learning işlem hattında kullanılabilir.

İşlem hedefi olarak Azure Batch iliştirmek için Azure Machine Learning SDK 'sını kullanmanız ve aşağıdaki bilgileri sağlamanız gerekir:

-    **Azure Batch işlem adı**: çalışma alanı içinde işlem için kullanılacak kolay bir ad
-    **Azure Batch hesap adı**: Azure Batch hesabının adı
-    **Kaynak grubu**: Azure Batch hesabını içeren kaynak grubu.

Aşağıdaki kod, Azure Batch işlem hedefi olarak nasıl ekleneceğini göstermektedir:

```python
from azureml.core.compute import ComputeTarget, BatchCompute
from azureml.exceptions import ComputeTargetException

# Name to associate with new compute in workspace
batch_compute_name = 'mybatchcompute'

# Batch account details needed to attach as compute to workspace
batch_account_name = "<batch_account_name>"  # Name of the Batch account
# Name of the resource group which contains this account
batch_resource_group = "<batch_resource_group>"

try:
    # check if the compute is already attached
    batch_compute = BatchCompute(ws, batch_compute_name)
except ComputeTargetException:
    print('Attaching Batch compute...')
    provisioning_config = BatchCompute.attach_configuration(
        resource_group=batch_resource_group, account_name=batch_account_name)
    batch_compute = ComputeTarget.attach(
        ws, batch_compute_name, provisioning_config)
    batch_compute.wait_for_completion()
    print("Provisioning state:{}".format(batch_compute.provisioning_state))
    print("Provisioning errors:{}".format(batch_compute.provisioning_errors))

print("Using Batch compute:{}".format(batch_compute.cluster_resource_id))
```

### <a name="azure-databricks"></a><a id="databricks"></a>Azure Databricks

Azure Databricks, Azure bulutu 'nda Apache Spark tabanlı bir ortamdır. Bir Azure Machine Learning işlem hattı ile işlem hedefi olarak kullanılabilir.

Kullanmadan önce bir Azure Databricks çalışma alanı oluşturun. Bir çalışma alanı kaynağı oluşturmak için Azure Databricks belge [üzerinde bir Spark Işi çalıştırma](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) ' ya bakın.

Azure Databricks bir işlem hedefi olarak eklemek için aşağıdaki bilgileri sağlayın:

* __Databricks işlem adı__: Bu işlem kaynağına atamak istediğiniz ad.
* __Databricks çalışma alanı adı__: Azure Databricks çalışma alanının adı.
* __Databricks erişim belirteci__: Azure Databricks için kimlik doğrulaması yapmak için kullanılan erişim belirteci. Erişim belirteci oluşturmak için, bkz. [kimlik doğrulama](https://docs.azuredatabricks.net/dev-tools/api/latest/authentication.html) belgesi.

Aşağıdaki kod, Azure Machine Learning SDK ile bir işlem hedefi olarak Azure Databricks nasıl ekleneceğini gösterir (__Databricks çalışma alanı, AML çalışma alanınızın aynı abonelikte bulunması gerekir__):

```python
import os
from azureml.core.compute import ComputeTarget, DatabricksCompute
from azureml.exceptions import ComputeTargetException

databricks_compute_name = os.environ.get(
    "AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_workspace_name = os.environ.get(
    "AML_DATABRICKS_WORKSPACE", "<databricks_workspace_name>")
databricks_resource_group = os.environ.get(
    "AML_DATABRICKS_RESOURCE_GROUP", "<databricks_resource_group>")
databricks_access_token = os.environ.get(
    "AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(
        workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_workspace_name {}'.format(databricks_workspace_name))
    print('databricks_access_token {}'.format(databricks_access_token))

    # Create attach config
    attach_config = DatabricksCompute.attach_configuration(resource_group=databricks_resource_group,
                                                           workspace_name=databricks_workspace_name,
                                                           access_token=databricks_access_token)
    databricks_compute = ComputeTarget.attach(
        ws,
        databricks_compute_name,
        attach_config
    )

    databricks_compute.wait_for_completion(True)
```

Daha ayrıntılı bir örnek için GitHub 'daki [örnek bir not defteri](https://aka.ms/pl-databricks) bölümüne bakın.

### <a name="azure-data-lake-analytics"></a><a id="adla"></a>Azure Data Lake Analytics

Azure Data Lake Analytics, Azure bulutundaki büyük bir veri analizi platformudur. Bir Azure Machine Learning işlem hattı ile işlem hedefi olarak kullanılabilir.

Kullanmadan önce bir Azure Data Lake Analytics hesabı oluşturun. Bu kaynağı oluşturmak için [Azure Data Lake Analytics kullanmaya başlama](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal) belgesine bakın.

İşlem hedefi olarak Data Lake Analytics iliştirmek için Azure Machine Learning SDK 'sını kullanmanız ve aşağıdaki bilgileri sağlamanız gerekir:

* __İşlem adı__: Bu işlem kaynağına atamak istediğiniz ad.
* __Kaynak grubu__: Data Lake Analytics hesabını içeren kaynak grubu.
* __Hesap adı__: Data Lake Analytics hesap adı.

Aşağıdaki kod, Data Lake Analytics işlem hedefi olarak nasıl ekleneceğini göstermektedir:

```python
import os
from azureml.core.compute import ComputeTarget, AdlaCompute
from azureml.exceptions import ComputeTargetException


adla_compute_name = os.environ.get(
    "AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_group = os.environ.get(
    "AML_ADLA_RESOURCE_GROUP", "<adla_resource_group>")
adla_account_name = os.environ.get(
    "AML_ADLA_ACCOUNT_NAME", "<adla_account_name>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_group))
    print('adla_account_name {}'.format(adla_account_name))
    # create attach config
    attach_config = AdlaCompute.attach_configuration(resource_group=adla_resource_group,
                                                     account_name=adla_account_name)
    # Attach ADLA
    adla_compute = ComputeTarget.attach(
        ws,
        adla_compute_name,
        attach_config
    )

    adla_compute.wait_for_completion(True)
```

Daha ayrıntılı bir örnek için GitHub 'daki [örnek bir not defteri](https://aka.ms/pl-adla) bölümüne bakın.

> [!TIP]
> Azure Machine Learning işlem hatları yalnızca Data Lake Analytics hesabının varsayılan veri deposunda depolanan verilerle çalışabilir. Üzerinde çalışmanız gereken veriler varsayılan olmayan bir depoda varsa, [`DataTransferStep`](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) verileri eğitimden önce kopyalamak için kullanabilirsiniz.

## <a name="notebook-examples"></a>Not defteri örnekleri

Çeşitli işlem hedeflerine yönelik eğitim örnekleri için bu not defterlerine bakın:
* [Nasıl yapılır kullanımı-azureml/eğitim](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [Öğreticiler/img-Classification-part1-Training. ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/image-classification-mnist-data/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Eğitim çalışması göndermek](how-to-set-up-training-targets.md)için işlem kaynağını kullanın.
* [Öğretici: bir modeli eğitme](tutorial-train-models-with-aml.md) bir modeli eğmek için yönetilen bir işlem hedefi kullanır.
* Daha iyi modeller oluşturmak için [hiper parametreleri verimli](how-to-tune-hyperparameters.md) bir şekilde ayarlamayı öğrenin.
* Eğitilen bir modelden sonra [modellerin nasıl ve nereye dağıtılacağını](how-to-deploy-and-where.md)öğrenin.
* [Azure sanal ağları ile Azure Machine Learning kullanma](how-to-enable-virtual-network.md)
