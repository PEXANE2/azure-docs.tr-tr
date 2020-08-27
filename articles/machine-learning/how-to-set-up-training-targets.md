---
title: Model eğitimi için işlem hedeflerini kullanma
titleSuffix: Azure Machine Learning
description: Machine Learning model eğitimi için eğitim ortamlarını (işlem hedefleri) yapılandırın. Eğitim ortamları arasında kolayca geçiş yapabilirsiniz. Eğitimi yerel olarak başlatın. Ölçeği ölçeklendirmeniz gerekiyorsa, bulut tabanlı bir işlem hedefine geçiş yapın.
services: machine-learning
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.date: 07/08/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: e83faee7d72026dafc50b21d0a0773e663e5a03a
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88933129"
---
# <a name="set-up-and-use-compute-targets-for-model-training"></a>Model eğitimi için işlem hedeflerini ayarlama ve kullanma 
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning, modelinizi toplu olarak [__işlem hedefleri__](concept-azure-machine-learning-architecture.md#compute-targets)olarak adlandırılan çeşitli kaynaklar veya ortamlar üzerinde eğitebilirsiniz. İşlem hedefi bir yerel makine veya Azure Machine Learning Işlem, Azure HDInsight veya uzak bir sanal makine gibi bir bulut kaynağı olabilir.  Model dağıtımı için, ["modellerinizi dağıtma"](how-to-deploy-and-where.md)bölümünde açıklandığı gibi işlem hedefleri de oluşturabilirsiniz.

Azure Machine Learning SDK, Azure Machine Learning Studio, Azure CLı veya Azure Machine Learning VS Code uzantısını kullanarak bir işlem hedefi oluşturabilir ve yönetebilirsiniz. Başka bir hizmet (örneğin, bir HDInsight kümesi) aracılığıyla oluşturulmuş işlem hedeflerinizin varsa, bunları Azure Machine Learning çalışma alanınıza ekleyerek kullanabilirsiniz.
 
Bu makalede, model eğitimi için çeşitli işlem hedeflerini nasıl kullanacağınızı öğreneceksiniz.  Tüm işlem hedeflerinin adımları aynı iş akışını izler:
1. Henüz yoksa bir işlem hedefi __oluşturun__ .
2. İşlem hedefini çalışma alanınıza __ekleyin__ .
3. İşlem hedefini, betik için gereken Python ortamını ve paket bağımlılıklarını içerecek şekilde __yapılandırın__ .


>[!NOTE]
> Bu makaledeki kod, Azure Machine Learning SDK sürümü 1.0.74 ile test edilmiştir.

## <a name="compute-targets-for-training"></a>Eğitim için işlem hedefleri

Azure Machine Learning, farklı işlem hedefleri arasında değişen desteğe sahiptir. Tipik bir model geliştirme yaşam döngüsü, düşük miktarda veri üzerinde dev/deneme ile başlar. Bu aşamada, yerel bir ortam kullanmanızı öneririz. Örneğin, Yerel bilgisayarınız veya bulut tabanlı bir VM. Daha büyük veri kümelerinde eğitime göre ölçeklendirirken veya dağıtılmış eğitim yaparken, her çalıştırma gönderdiğinizde otomatik olarak ölçeklenen tek veya çok düğümlü bir küme oluşturmak için Azure Machine Learning Işlem kullanmanızı öneririz. Ayrıca, çeşitli senaryolar için destek aşağıda ayrıntılı şekilde değişiklik gösterebilse de kendi işlem kaynağınızı ekleyebilirsiniz:

[!INCLUDE [aml-compute-target-train](../../includes/aml-compute-target-train.md)]


> [!NOTE]
> Azure Machine Learning Işlem kümeleri, kalıcı bir kaynak olarak oluşturulabilir veya bir çalıştırma istediğinizde dinamik olarak oluşturulabilir. Çalışma tabanlı oluşturma, eğitim çalıştırması tamamlandıktan sonra işlem hedefini kaldırır, bu nedenle bu şekilde oluşturulan işlem hedeflerini yeniden kullanamazsınız.

## <a name="whats-a-run-configuration"></a>Çalıştırma Yapılandırması nedir?

Eğitim sırasında yerel bilgisayarınızda başlamak yaygındır ve daha sonra bu eğitim betiği farklı bir işlem hedefinde çalıştırılır. Azure Machine Learning, komut dosyanızı değiştirmek zorunda kalmadan komut dosyanızı çeşitli işlem hedeflerinde çalıştırabilirsiniz.

Tüm yapmanız gereken, **çalışma yapılandırmasındaki**her bir işlem hedefi için ortamı tanımlamaktır.  Daha sonra eğitim denemenizi farklı bir işlem hedefinde çalıştırmak istediğinizde, bu işlem için çalıştırma yapılandırmasını belirtin. Bir ortamı belirtme ve yapılandırmayı çalıştırmaya bağlama hakkında ayrıntılı bilgi için bkz. [eğitim ve dağıtım için ortamları oluşturma ve yönetme](how-to-use-environments.md).

Bu makalenin sonunda [denemeleri gönderme](#submit) hakkında daha fazla bilgi edinin.

## <a name="whats-an-estimator"></a>Estimator nedir?

Popüler çerçeveleri kullanarak model eğitimi kolaylaştırmak için Azure Machine Learning Python SDK, daha yüksek düzeyde bir soyutlama ve tahmin aracı sınıfı sağlar.  Bu sınıf, çalışma yapılandırmasını kolayca oluşturmanıza olanak tanır. Seçtiğiniz herhangi bir öğrenme çerçevesini kullanan eğitim betikleri göndermek için genel bir [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) oluşturabilir ve kullanabilirsiniz (örneğin, scikit-öğren). Bir ortam veya RunConfiguration nesneleri gibi katıştırılmış nesneleri otomatik olarak oluştururken eğitim için bir tahmin aracı kullanmanızı öneririz. Bu nesnelerin nasıl oluşturulduğuna ilişkin daha fazla denetime sahip olmak ve denemenizin çalışması için hangi paketlerin yükleneceğini belirlemek isterseniz, Azure Machine Learning Işlem üzerinde bir RunConfiguration nesnesi kullanarak eğitim denemeleri göndermek için [aşağıdaki adımları](#amlcompute) izleyin.

Azure Machine Learning [Pytorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py), [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py)ve [Ray rllib](how-to-use-reinforcement-learning.md)için belirli tahminler sağlar.

Daha fazla bilgi için bkz. [tahmini ile ml modellerini eğitme](how-to-train-ml-models.md).

## <a name="whats-an-ml-pipeline"></a>ML ardışık düzeni nedir?

ML işlem hatları sayesinde, basitlik, hız, taşınabilirlik ve yeniden kullanım sayesinde iş akışınızı iyileştirebilmenizi sağlayabilirsiniz. Azure Machine Learning ile işlem hatları oluştururken, altyapı ve otomasyon yerine uzmanınıza ve makine öğrenmenize odaklanırsınız.

ML işlem hatları, işlem hattının farklı hesaplama birimleri olan birden çok **adımdan**oluşturulur. Her adım bağımsız olarak çalıştırılabilir ve yalıtılmış işlem kaynakları kullanabilir. Bu yaklaşım, birden fazla veri bilimcilerinin aynı işlem hattı üzerinde, çok yönlü bilgi işlem kaynakları olmadan aynı anda çalışmasına olanak tanır ve ayrıca her adım için farklı işlem türleri/boyutları kullanmayı kolaylaştırır.

> [!TIP]
> ML işlem hatları, eğitim modelleri sırasında yapılandırmayı veya tahminleri Çalıştır 'ı kullanabilir.

ML işlem hatları modelleri eğitebilir, ancak eğitim vermeden önce verileri hazırlayabilir ve eğitiminden sonra modeller dağıtabilirler. İşlem hatları için birincil kullanım çalışmalarından biri Batch Puanlama. Daha fazla bilgi için bkz. işlem [hatları: makine öğrenimi iş akışlarını iyileştirme](concept-ml-pipelines.md).

## <a name="set-up-in-python"></a>Python 'da ayarlama

Bu işlem hedeflerini yapılandırmak için aşağıdaki bölümleri kullanın:

* [Yerel bilgisayar](#local)
* [Azure Machine Learning işlem kümesi](#amlcompute)
* [Azure Machine Learning işlem örneği](#instance)
* [Uzak sanal makineler](#vm)
* [Azure HDInsight](#hdinsight)


### <a name="local-computer"></a><a id="local"></a>Yerel bilgisayar

1. **Oluşturma ve iliştirme**: eğitim ortamı olarak yerel bilgisayarınızı kullanmak için bir işlem hedefi oluşturmanız veya eklemeniz gerekmez.  

1. **Yapılandırma**: yerel bilgisayarınızı bir işlem hedefi olarak kullandığınızda, eğitim kodu [geliştirme ortamınızda](how-to-configure-environment.md)çalıştırılır.  Bu ortamda zaten ihtiyacınız olan Python paketleri varsa, Kullanıcı tarafından yönetilen ortamı kullanın.

 [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=run_local)]

Bu işlemi yaptıktan sonra, çalışmanızı yapılandırdığınıza göre, sonraki adım [eğitim çalıştırmasını göndermektedir](#submit).

### <a name="azure-machine-learning-compute-cluster"></a><a id="amlcompute"></a>Azure Machine Learning işlem kümesi

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

    Ya da [Azure Machine Learning Studio](#portal-create)'da kalıcı bir Azure Machine Learning işlem kaynağı oluşturup ekleyebilirsiniz.

   
1. **Yapılandır**: kalıcı işlem hedefi için bir çalıştırma yapılandırması oluşturun.

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=run_amlcompute)]

Bu işlemi yaptıktan sonra, çalışmanızı yapılandırdığınıza göre, sonraki adım [eğitim çalıştırmasını göndermektedir](#submit).

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

 Azure Machine Learning işlem kümeleri, kodunuzun kimlik bilgilerini eklemeden Azure kaynaklarına erişimin kimliğini doğrulamak için de [yönetilen kimlikleri](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) destekler. İki tür yönetilen kimlik vardır:

* **Sistem tarafından atanan yönetilen kimlik** Azure Machine Learning işlem kümesi üzerinde doğrudan etkinleştirilir. Sistem tarafından atanan kimliğin yaşam döngüsü doğrudan işlem kümesine bağlıdır. İşlem kümesi silinirse Azure, kimlik bilgilerini ve kimlik bilgilerini Azure AD 'de otomatik olarak temizler.
* **Kullanıcı tarafından atanan yönetilen kimlik** , Azure yönetilen kimlik hizmeti aracılığıyla sunulan tek başına bir Azure kaynağıdır. Kullanıcı tarafından atanan bir yönetilen kimliği birden fazla kaynağa atayabilir ve istediğiniz sürece devam edebilirsiniz.

Bir işlem kümesi için yönetilen bir kimlik belirtmek için şu yolların herhangi birini kullanın:
    
* Studio 'da, küme oluşturma işlemi sırasında veya işlem kümesi ayrıntılarını düzenlediğinizde, **yönetilen bir kimlik atayın** ve sistem tarafından atanan bir kimlik veya Kullanıcı tarafından atanan kimlik belirtin.
    
* Python SDK ile, `identity_type` sağlama yapılandırmanızda özniteliğini ayarlayın.  
    
    ```python
    # configure cluster with a system-assigned managed identity
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                            max_nodes=5,
                                                            identity_type="SystemAssigned",
                                                            )

    # configure cluster with a user-assigned managed identity
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                            max_nodes=5,
                                                            identity_type="UserAssigned",
                                                            identity_id=['/subscriptions/<subcription_id>/resourcegroups/<resource_group>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<user_assigned_identity>'])

    cpu_cluster_name = "cpu-cluster"
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)
    ```

* Python SDK ile, `identity_type` `identity_id` sağlama yapılandırmanızda ve (Kullanıcı tarafından atanan yönetilen kimlik oluşturuyorsanız) öznitelikleri ayarlayın.  
    
    ```python
    # add a system-assigned managed identity
    cpu_cluster.add_identity(identity_type="SystemAssigned")

    # add a user-assigned managed identity
    cpu_cluster.add_identity(identity_type="UserAssigned", 
                                identity_id=['/subscriptions/<subcription_id>/resourcegroups/<resource_group>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<user_assigned_identity>'])
    ```
    
* CLı kullanarak, `assign-identity` küme oluşturma sırasında özniteliği ayarlayın:
    
    ```azurecli
    # create a cluster with a user-assigned managed identity
    az ml computetarget create amlcompute --name cpu-cluster --vm-size Standard_NC6 --max-nodes 5 --assign-identity '/subscriptions/<subcription_id>/resourcegroups/<resource_group>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<user_assigned_identity>'

    # create a cluster with a system-managed identity
    az ml computetarget create amlcompute --name cpu-cluster --vm-size Standard_NC6 --max-nodes 5 --assign-identity '[system]'

* Using the CLI, execute the following commands to assign a managed identity on an existing cluster:
    
    ```azurecli
    # add a user-assigned managed identity
    az ml computetarget amlcompute identity assign --name cpu-cluster '/subscriptions/<subcription_id>/resourcegroups/<resource_group>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<user_assigned_identity>'

    # add a system-assigned managed identity
    az ml computetarget amlcompute identity assign --name cpu-cluster '[system]'

> [!NOTE]
> Azure Machine Learning compute clusters support only **one system-assigned identity** or **multiple user-assigned identities**, not both concurrently.
> 
> Additionally, you can assign only one managed identity from the studio.

#### Managed identity usage

AML defines the **default managed identity** as the system-assigned managed identity or the first user-assigned managed identity.

During a run there are two applications of an identity:
1. The system uses an identity to setup the user's storage mounts, container registry, and datastores.
    * In this case, the system will use the default managed identity.

1. The user applies an identity to access resources from within the code for a submitted run
    
    * In this case, the user must provide the *client_id* corresponding to the managed identity they want to use to retrieve a credential. 
    * Alternatively, AML exposes the user-assigned identity's client ID through the *DEFAULT_IDENTITY_CLIENT_ID* environment variable.
    
    For example, to retrieve a token for a datastore with the default managed identity:
    
    ```python
    client_id = os.environ.get('DEFAULT_IDENTITY_CLIENT_ID')
    credential = ManagedIdentityCredential(client_id=client_id)
    token = credential.get_token('https://storage.azure.com/')



### <a id="instance"></a>Azure Machine Learning compute instance

[Azure Machine Learning compute instance](concept-compute-instance.md) is a managed-compute infrastructure that allows you to easily create a single VM. The compute is created within your workspace region, but unlike a compute cluster, an instance cannot be shared with other users in your workspace. Also the instance does not automatically scale down.  You must stop the resource to prevent ongoing charges.

A compute instance can run multiple jobs in parallel and has a job queue. 

Compute instances can run jobs securely in a [virtual network environment](how-to-enable-virtual-network.md#compute-instance), without requiring enterprises to open up SSH ports. The job executes in a containerized environment and packages your model dependencies in a Docker container. 

1. **Create and attach**: 
    
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

1. **Yapılandırma**: çalıştırma yapılandırması oluşturun.
    
    ```python
    
    from azureml.core import ScriptRunConfig
    from azureml.core.runconfig import DEFAULT_CPU_IMAGE
    
    src = ScriptRunConfig(source_directory='', script='train.py')
    
    # Set compute target to the one created in previous step
    src.run_config.target = instance
    
    # Set environment
    src.run_config.environment = myenv
     
    run = experiment.submit(config=src)
    ```

Bu işlemi yaptıktan sonra, çalışmanızı yapılandırdığınıza göre, bir sonraki adım [eğitim çalıştırmasını gönderdiniz](#submit)


### <a name="remote-virtual-machines"></a><a id="vm"></a>Uzak sanal makineler

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

   Veya [Azure Machine Learning Studio 'yu kullanarak](#portal-reuse)dsvm 'yi çalışma alanınıza ekleyebilirsiniz.

1. **Yapılandır**: dsvm işlem hedefi için bir çalıştırma yapılandırması oluşturun. Docker ve Conda, DSVM 'de Eğitim ortamı oluşturmak ve yapılandırmak için kullanılır.

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/dsvm.py?name=run_dsvm)]


Bu işlemi yaptıktan sonra, çalışmanızı yapılandırdığınıza göre, sonraki adım [eğitim çalıştırmasını göndermektedir](#submit).

### <a name="azure-hdinsight"></a><a id="hdinsight"></a>Azure HDInsight 

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

   İsterseniz de [Azure Machine Learning Studio kullanarak](#portal-reuse)HDInsight kümesini çalışma alanınıza ekleyebilirsiniz.

1. **Yapılandır**: HDI işlem hedefi için bir çalıştırma yapılandırması oluşturun. 

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/hdi.py?name=run_hdi)]


Bu işlemi yaptıktan sonra, çalışmanızı yapılandırdığınıza göre, sonraki adım [eğitim çalıştırmasını göndermektedir](#submit).


### <a name="azure-batch"></a><a id="azbatch"></a>Azure Batch 

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

## <a name="set-up-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio 'da ayarlama

Azure Machine Learning Studio 'daki çalışma alanınız ile ilişkili işlem hedeflerine erişebilirsiniz.  Studio 'yu kullanarak şunları yapabilirsiniz:

* Çalışma alanınıza bağlı [işlem hedeflerini görüntüleme](#portal-view)
* Çalışma alanınızda [bir işlem hedefi oluşturma](#portal-create)
* Çalışma alanının dışında oluşturulmuş [bir işlem hedefi iliştirme](#portal-reuse)


Bir hedef oluşturulduktan ve çalışma alanınıza eklendikten sonra, çalışma yapılandırmanızda bunu bir `ComputeTarget` nesneyle kullanacaksınız: 

```python
from azureml.core.compute import ComputeTarget
myvm = ComputeTarget(workspace=ws, name='my-vm-name')
```

### <a name="view-compute-targets"></a><a id="portal-view"></a>İşlem hedeflerini görüntüleme


Çalışma alanınızın işlem hedeflerini görmek için aşağıdaki adımları kullanın:

1. [Azure Machine Learning Studio](https://ml.azure.com)'ya gidin.
 
1. __Uygulamalar__' ın altında, __işlem__' ı seçin.

    [![İşlem sekmesini görüntüle](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace-expanded.png)

### <a name="create-a-compute-target"></a><a id="portal-create"></a>İşlem hedefi oluştur

İşlem hedeflerinin listesini görüntülemek için önceki adımları izleyin. Daha sonra bir işlem hedefi oluşturmak için bu adımları kullanın: 

1. Bir işlem hedefi eklemek için artı işaretini (+) seçin.

    ![İşlem hedefi ekleme](./media/how-to-set-up-training-targets/add-compute-target.png) 

1. İşlem hedefi için bir ad girin. 

1. __Eğitim__için kullanılacak işlem türü olarak **Machine Learning işlem** seçin. 

    >[!NOTE]
    >Azure Machine Learning Işlem, Azure Machine Learning Studio 'da oluşturabileceğiniz tek yönetilen işlem kaynağıdır.  Diğer tüm işlem kaynakları, oluşturulduktan sonra eklenebilir.

1. Formu doldurun. Gerekli özellikler, özellikle **VM ailesi**ve işlem için kullanılacak **en fazla düğüm** değerlerini sağlayın.  

1. __Oluştur__’u seçin.


1. Listeden işlem hedefini seçerek oluşturma işleminin durumunu görüntüleyin:

    ![Oluşturma işlemi durumunu görüntülemek için bir işlem hedefi seçin](./media/how-to-set-up-training-targets/View_list.png)

1. Daha sonra işlem hedefinin ayrıntılarını görürsünüz: 

    ![Bilgisayar hedefi ayrıntılarını görüntüleme](./media/how-to-set-up-training-targets/compute-target-details.png) 

### <a name="attach-compute-targets"></a><a id="portal-reuse"></a>İşlem hedeflerini iliştirme

Azure Machine Learning çalışma alanı dışında oluşturulan işlem hedeflerini kullanmak için, onları iliştirmeli. İşlem hedefini iliştirmek, çalışma alanınız için kullanılabilir hale getirir.

Daha önce açıklanan adımları izleyerek işlem hedeflerinin listesini görüntüleyin. Daha sonra bir işlem hedefi eklemek için aşağıdaki adımları kullanın: 

1. Bir işlem hedefi eklemek için artı işaretini (+) seçin. 
1. İşlem hedefi için bir ad girin. 
1. __Eğitim__için iliştirilecek işlem türünü seçin:

    > [!IMPORTANT]
    > Tüm bilgi işlem türleri Azure Machine Learning Studio 'dan iliştirilemez. Şu anda eğitim için iliştirilebilecek işlem türleri şunlardır:
    >
    > * Uzak VM
    > * Azure Databricks (makine öğrenimi ardışık düzenleri kullanımı için)
    > * Azure Data Lake Analytics (makine öğrenimi ardışık düzenleri kullanımı için)
    > * Azure HDInsight

1. Formu doldurun ve gerekli özellikler için değerler sağlayın.

    > [!NOTE]
    > Microsoft, parolalardan daha güvenli olan SSH anahtarlarını kullanmanızı önerir. Parolalar, deneme yanılma saldırılarına karşı savunmasızdır. SSH anahtarları şifreleme imzaları kullanır. Azure sanal makineler ile kullanmak üzere SSH anahtarları oluşturma hakkında bilgi için aşağıdaki belgelere bakın:
    >
    > * [Linux veya macOS 'ta SSH anahtarları oluşturma ve kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Windows 'da SSH anahtarları oluşturma ve kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. __Ekle__' yi seçin. 
1. Listeden işlem hedefini seçerek iliştirme işleminin durumunu görüntüleyin.

## <a name="set-up-with-cli"></a>CLı ile ayarlama

Azure Machine Learning için [CLI uzantısını](reference-azure-machine-learning-cli.md) kullanarak çalışma alanınız ile ilişkili işlem hedeflerine erişebilirsiniz.  CLı 'yi kullanarak şunları yapabilirsiniz:

* Yönetilen işlem hedefi oluşturma
* Yönetilen bir işlem hedefini güncelleştirme
* Yönetilmeyen bir işlem hedefi iliştirme

Daha fazla bilgi için bkz. [kaynak yönetimi](reference-azure-machine-learning-cli.md#resource-management).

## <a name="set-up-with-vs-code"></a>VS Code ayarla

Azure Machine Learning için [vs Code uzantısını](how-to-manage-resources-vscode.md#compute-clusters) kullanarak çalışma alanınız ile ilişkili işlem hedeflerini erişebilir, oluşturabilir ve yönetebilirsiniz.

## <a name="submit-training-run-using-azure-machine-learning-sdk"></a><a id="submit"></a>Azure Machine Learning SDK kullanarak eğitim çalışması gönder

Bir çalıştırma yapılandırması oluşturduktan sonra, deneme hesabınızı çalıştırmak için onu kullanırsınız.  Eğitim çalışması göndermek için kod deseninin her türlü bilgi işlem hedefi vardır:

1. Çalıştırmak için bir deneme oluşturun
1. Çalıştırmayı gönder.
1. Çalıştırmanın tamamlanmasını bekleyin.

> [!IMPORTANT]
> Eğitim çalıştırmasını gönderdiğinizde, eğitim betiklerinizi içeren dizinin bir anlık görüntüsü oluşturulur ve işlem hedefine gönderilir. Ayrıca çalışma alanınızdaki denemenin bir parçası olarak da depolanır. Dosyaları değiştirir ve çalışmayı yeniden gönderirseniz yalnızca değiştirilen dosyalar karşıya yüklenir.
>
> [!INCLUDE [amlinclude-info](../../includes/machine-learning-amlignore-gitignore.md)]
> 
> Daha fazla bilgi için bkz. [anlık görüntüler](concept-azure-machine-learning-architecture.md#snapshots).

### <a name="create-an-experiment"></a>Deneme oluşturma

İlk olarak, çalışma alanınızda bir deneme oluşturun.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=experiment)]

### <a name="submit-the-experiment"></a>Denemeyi gönderme

Denemeyi bir nesneyle birlikte gönder `ScriptRunConfig` .  Bu nesne şunları içerir:

* **source_directory**: eğitim betiğinizi içeren kaynak dizin
* **betik**: eğitim betiğini tanımla
* **run_config**: çalıştırma yapılandırması, daha sonra eğitimin nerede olacağını tanımlar.

Örneğin, [yerel hedef](#local) yapılandırmasını kullanmak için:

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=local_submit)]

Aynı denemeyi, [amlcompute hedefi](#amlcompute)gibi farklı bir çalıştırma yapılandırması kullanarak farklı bir işlem hedefinde çalışacak şekilde değiştirin:

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=amlcompute_submit)]

> [!TIP]
> Bu örnek, eğitim için yalnızca bir işlem hedefi düğümü kullanmayı varsayılan olarak belirler. Birden fazla düğüm kullanmak için, `node_count` çalıştırma yapılandırmasını istenen düğüm sayısı olarak ayarlayın. Örneğin, aşağıdaki kod eğitim için kullanılan düğümlerin sayısını dört olarak ayarlar:
>
> ```python
> src.run_config.node_count = 4
> ```

İsterseniz şunları yapabilirsiniz:

* Denemesi, `Estimator` [ml modellerini tahmini ile eğitme](how-to-train-ml-models.md)bölümünde gösterildiği gibi bir nesneyle birlikte gönder.
* [Hiper parametre ayarlama](how-to-tune-hyperparameters.md)Için bir Hyperdrive çalıştırması gönderebilirsiniz.
* [Vs Code uzantısı](tutorial-train-deploy-image-classification-model-vscode.md#train-the-model)aracılığıyla bir deneme gönderir.

Daha fazla bilgi için [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) ve [RunConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py) belgelerine bakın.

## <a name="create-run-configuration-and-submit-run-using-azure-machine-learning-cli"></a>Azure Machine Learning CLı kullanarak çalıştırma yapılandırması oluşturma ve çalıştırma

Çalışan yapılandırma oluşturmak ve farklı işlem hedeflerinde çalıştırmaları göndermek için [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ve [Machine Learning CLI uzantısı](reference-azure-machine-learning-cli.md) kullanabilirsiniz. Aşağıdaki örneklerde, mevcut bir Azure Machine Learning Çalışma Alanı olduğunu ve CLI komutunu kullanarak Azure 'da oturum açtığınızı varsayalım `az login` . 

[!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)] 

### <a name="create-run-configuration"></a>Çalıştırma yapılandırması oluştur

Çalıştırma yapılandırması oluşturmanın en kolay yolu, Machine Learning Python betiklerini içeren klasörde gezinmeniz ve CLı komutunu kullanmaktır

```azurecli
az ml folder attach
```

Bu komut `.azureml` , farklı işlem hedeflerine yönelik yapılandırma dosyalarını çalıştıran şablon içeren bir alt klasör oluşturur. Yapılandırmanızı özelleştirmek için bu dosyaları kopyalayabilir ve düzenleyebilirsiniz (örneğin, Python paketleri eklemek veya Docker ayarlarını değiştirmek).  

### <a name="structure-of-run-configuration-file"></a>Çalışma yapılandırma dosyasının yapısı

Çalışma yapılandırma dosyası, aşağıdaki bölümlerle, YAML olarak biçimlendirilir
 * Çalıştırılacak betiği ve bağımsız değişkenlerini
 * İşlem hedefi adı, "yerel" veya çalışma alanı altında bir işlem adı.
 * Çalıştırmayı yürütmek için Parametreler: Framework, dağıtılmış çalıştırmalar için Communicator, maksimum süre ve işlem düğümleri sayısı.
 * Ortam bölümü. Bu bölümdeki alanların ayrıntıları için [eğitim ve dağıtım için ortamları oluşturma ve yönetme](how-to-use-environments.md) bölümüne bakın.
   * Çalıştırma için yüklenecek Python paketlerini belirtmek için [Conda ortam dosyası](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#create-env-file-manually)oluşturun ve __Condadbağımlıcıesfile__ alanını ayarlayın.
 * Günlük dosyası klasörünü belirtmek ve çıkış toplamayı etkinleştirmek veya devre dışı bırakmak ve geçmiş anlık görüntülerini çalıştırmak için geçmiş ayrıntılarını çalıştırın.
 * Seçili çerçeveye özgü yapılandırma ayrıntıları.
 * Veri başvurusu ve veri deposu ayrıntıları.
 * Yeni bir küme oluşturmak için Machine Learning İşlem özgü yapılandırma ayrıntıları.

Tam runconfig şeması için örnek [JSON dosyasına](https://github.com/microsoft/MLOps/blob/b4bdcf8c369d188e83f40be8b748b49821f71cf2/infra-as-code/runconfigschema.json) bakın.

### <a name="create-an-experiment"></a>Deneme oluşturma

İlk olarak, çalışmalarınız için bir deneme oluşturun

```azurecli
az ml experiment create -n <experiment>
```

### <a name="script-run"></a>Betik çalıştırma

Bir betik çalıştırması göndermek için bir komut yürütün

```azurecli
az ml run submit-script -e <experiment> -c <runconfig> my_train.py
```

### <a name="hyperdrive-run"></a>Hiper sürücü çalıştırma

Parametre ayarlama çalıştırmalarını gerçekleştirmek için HyperDrive ile Azure CLı kullanabilirsiniz. İlk olarak, aşağıdaki biçimde bir hiper sürücü yapılandırma dosyası oluşturun. Hyperparameter ayarlama parametreleriyle ilgili ayrıntılar için bkz. [modelinize yönelik hiper parametreleri ayarlama](how-to-tune-hyperparameters.md) makalesi.

```yml
# hdconfig.yml
sampling: 
    type: random # Supported options: Random, Grid, Bayesian
    parameter_space: # specify a name|expression|values tuple for each parameter.
    - name: --penalty # The name of a script parameter to generate values for.
      expression: choice # supported options: choice, randint, uniform, quniform, loguniform, qloguniform, normal, qnormal, lognormal, qlognormal
      values: [0.5, 1, 1.5] # The list of values, the number of values is dependent on the expression specified.
policy: 
    type: BanditPolicy # Supported options: BanditPolicy, MedianStoppingPolicy, TruncationSelectionPolicy, NoTerminationPolicy
    evaluation_interval: 1 # Policy properties are policy specific. See the above link for policy specific parameter details.
    slack_factor: 0.2
primary_metric_name: Accuracy # The metric used when evaluating the policy
primary_metric_goal: Maximize # Maximize|Minimize
max_total_runs: 8 # The maximum number of runs to generate
max_concurrent_runs: 2 # The number of runs that can run concurrently.
max_duration_minutes: 100 # The maximum length of time to run the experiment before cancelling.
```

Bu dosyayı çalıştırma yapılandırma dosyalarının yanına ekleyin. Ardından, kullanarak bir HyperDrive çalıştırması gönderebilirsiniz:
```azurecli
az ml run submit-hyperdrive -e <experiment> -c <runconfig> --hyperdrive-configuration-name <hdconfig> my_train.py
```

Hiperdrive yapılandırmasında runconfig ve *Parameter Space* içindeki *arguments* bölümüne göz önünde bırakın. Eğitim betiğine geçirilecek komut satırı bağımsız değişkenlerini içerirler. Runconfig içindeki değer her yineleme için aynı kalır, hiper sürücü yapılandırma aralığı üzerinden yinelenir. Her iki dosyada de aynı bağımsız değişkeni belirtmeyin.

Bu CLI komutları hakkında daha fazla bilgi için ```az ml``` [başvuru belgelerine](reference-azure-machine-learning-cli.md)bakın.

<a id="gitintegration"></a>

## <a name="git-tracking-and-integration"></a>Git izleme ve Tümleştirme

Kaynak dizinin yerel bir git deposu olduğu bir eğitim çalıştırması başlattığınızda, depo hakkındaki bilgiler çalıştırma geçmişinde depolanır. Daha fazla bilgi için bkz. [Azure Machine Learning Için git tümleştirmesi](concept-train-model-git-integration.md).

## <a name="notebook-examples"></a>Not defteri örnekleri

Çeşitli işlem hedeflerine yönelik eğitim örnekleri için bu not defterlerine bakın:
* [Nasıl yapılır kullanımı-azureml/eğitim](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [Öğreticiler/img-Classification-part1-Training. ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/image-classification-mnist-data/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: bir modeli eğitme](tutorial-train-models-with-aml.md) bir modeli eğmek için yönetilen bir işlem hedefi kullanır.
* Daha iyi modeller oluşturmak için [hiper parametreleri verimli](how-to-tune-hyperparameters.md) bir şekilde ayarlamayı öğrenin.
* Eğitilen bir modelden sonra [modellerin nasıl ve nereye dağıtılacağını](how-to-deploy-and-where.md)öğrenin.
* [RunConfiguration sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) SDK başvurusunu görüntüleyin.
* [Azure sanal ağları ile Azure Machine Learning kullanma](how-to-enable-virtual-network.md)
