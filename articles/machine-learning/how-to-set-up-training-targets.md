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
ms.topic: how-to
ms.date: 06/11/2020
ms.custom: seodec18, tracking-python
ms.openlocfilehash: aa11f7e964f66d0a345e25f307127d75838f872f
ms.sourcegitcommit: a8928136b49362448e992a297db1072ee322b7fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84718725"
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
> Azure Machine Learning Işlem, kalıcı bir kaynak olarak oluşturulabilir veya bir çalıştırma istediğinizde dinamik olarak oluşturulabilir. Çalışma tabanlı oluşturma, eğitim çalıştırması tamamlandıktan sonra işlem hedefini kaldırır, bu nedenle bu şekilde oluşturulan işlem hedeflerini yeniden kullanamazsınız.

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
* [Azure Machine Learning İşlemi](#amlcompute)
* [Uzak sanal makineler](#vm)
* [Azure HDInsight](#hdinsight)


### <a name="local-computer"></a><a id="local"></a>Yerel bilgisayar

1. **Oluşturma ve iliştirme**: eğitim ortamı olarak yerel bilgisayarınızı kullanmak için bir işlem hedefi oluşturmanız veya eklemeniz gerekmez.  

1. **Yapılandırma**: yerel bilgisayarınızı bir işlem hedefi olarak kullandığınızda, eğitim kodu [geliştirme ortamınızda](how-to-configure-environment.md)çalıştırılır.  Bu ortamda zaten ihtiyacınız olan Python paketleri varsa, Kullanıcı tarafından yönetilen ortamı kullanın.

 [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=run_local)]

Bu işlemi yaptıktan sonra, çalışmanızı yapılandırdığınıza göre, sonraki adım [eğitim çalıştırmasını göndermektedir](#submit).

### <a name="azure-machine-learning-compute"></a><a id="amlcompute"></a>Azure Machine Learning İşlemi

Azure Machine Learning Işlem, kullanıcının kolayca tek veya çok düğümlü bir işlem oluşturmasına olanak tanıyan bir yönetilen işlem altyapısıdır. İşlem, çalışma alanınızdaki diğer kullanıcılarla paylaşılabilecek bir kaynak olarak çalışma alanı bölgeniz içinde oluşturulur. İşlem, bir iş gönderildiğinde otomatik olarak ölçeklendirilir ve bir Azure sanal ağına yerleştirilebilir. İşlem kapsayıcılı bir ortamda yürütülür ve model bağımlılıklarınızı bir [Docker kapsayıcısında](https://www.docker.com/why-docker)paketleyebilir.

Eğitim sürecini buluttaki bir CPU veya GPU işlem düğümleri kümesi arasında dağıtmak için Azure Machine Learning Işlem kullanabilirsiniz. GPU 'ları içeren VM boyutları hakkında daha fazla bilgi için bkz. [GPU ile iyileştirilmiş sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu). 

Azure Machine Learning Işlem, ayrılabilen çekirdek sayısı gibi varsayılan sınırlara sahiptir. Daha fazla bilgi için bkz. [Azure kaynakları için kotaları yönetme ve isteme](https://docs.microsoft.com/azure/machine-learning/how-to-manage-quotas).

İş yüklerinizin bazılarını veya tümünü çalıştırmak için düşük öncelikli VM 'Ler kullanmayı da tercih edebilirsiniz. Bu VM 'Lerin garantili kullanılabilirliği yoktur ve kullanımda olabilir. Geçersiz hale başlatılan iş yeniden başlatıldı, sürdürülmez.  Düşük öncelikli VM 'Ler, normal VM 'Lerle karşılaştırılan indirimli tarifeler için bkz. [maliyetleri planlayın ve yönetin](https://docs.microsoft.com/azure/machine-learning/concept-plan-manage-cost).

> [!TIP]
> Gereken çekirdek sayısı için yeterli kotanın olması koşuluyla, kümeler genellikle 100 düğüme kadar ölçeklendirebilir. Varsayılan olarak kümeler, MPı işlerini desteklemek üzere küme düğümleri arasında etkinleştirilen düğümler arası iletişim ile ayarlanır. Ancak, [bir destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)oluşturarak ve abonelik veya çalışma alanınızı ya da düğümler arası iletişimi devre dışı bırakmaya yönelik belirli bir kümeyi beyaz listeye almak istediğinizde, kümelerinizi 1000 düğüm olarak ölçeklendirebilirsiniz. 

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


### <a name="remote-virtual-machines"></a><a id="vm"></a>Uzak sanal makineler

Azure Machine Learning Ayrıca kendi işlem kaynağınızı getirme ve çalışma alanınıza ekleme desteği de sağlar. Bu tür bir kaynak türü, Azure Machine Learning 'ten erişilebilen sürece rastgele bir uzak VM 'dir. Kaynak bir Azure VM 'si, kuruluşunuzdaki uzak sunucu veya şirket içi olabilir. Özellikle, IP adresi ve kimlik bilgileri (Kullanıcı adı ve parola veya SSH anahtarı) verildiğinde, uzak çalıştırmalar için erişilebilir VM 'leri kullanabilirsiniz.

Sistemde oluşturulmuş bir Conda ortamı, zaten varolan bir Python ortamı veya Docker kapsayıcısı kullanabilirsiniz. Bir Docker kapsayıcısında yürütmek için, VM 'de çalışan bir Docker altyapısına sahip olmanız gerekir. Bu işlevsellik, yerel makinenize kıyasla daha esnek, bulut tabanlı bir geliştirme/deneme ortamı istediğinizde özellikle yararlıdır.

Bu senaryo için tercih edilen Azure sanal makinesi olarak Azure Veri Bilimi Sanal Makinesi (DSVM) kullanın. Bu VM, Azure 'da önceden yapılandırılmış bir veri bilimi ve AI geliştirme ortamıdır. VM, tam yaşam döngüsü makine öğrenimi geliştirmesi için seçkin bir araç ve çerçeve seçeneği sunar. DSVM 'nin Azure Machine Learning ile nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [bir geliştirme ortamı yapılandırma](https://docs.microsoft.com/azure/machine-learning/how-to-configure-environment#dsvm).

1. **Oluştur**: modelinize eğitebilmeniz için kullanmadan önce dsvm oluşturun. Bu kaynağı oluşturmak için bkz. [Linux için veri bilimi sanal makinesi sağlama (Ubuntu)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro).

    > [!WARNING]
    > Azure Machine Learning yalnızca **Ubuntu**çalıştıran sanal makineleri destekler. Bir VM oluşturduğunuzda veya var olan bir VM 'yi seçtiğinizde Ubuntu kullanan bir VM seçmeniz gerekir.
    > 
    > Azure Machine Learning Ayrıca, sanal makinenin __Genel BIR IP adresine__sahip olmasını gerektirir.

1. **Ekle**: var olan bir sanal makineyi bir işlem hedefi olarak eklemek için, sanal makine IÇIN kaynak kimliği, Kullanıcı adı ve parola sağlamanız gerekir. VM 'nin kaynak KIMLIĞI, aşağıdaki dize biçimi kullanılarak abonelik KIMLIĞI, kaynak grubu adı ve VM adı kullanılarak oluşturulabilir:`/subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Compute/virtualMachines/<vm_name>`

 
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

1. **İliştirme**: bir HDInsight kümesini işlem hedefi olarak eklemek için HDInsight kümesi IÇIN kaynak kimliği, Kullanıcı adı ve parola sağlamalısınız. HDInsight kümesinin kaynak KIMLIĞI, aşağıdaki dize biçimi kullanılarak abonelik KIMLIĞI, kaynak grubu adı ve HDInsight kümesi adı kullanılarak oluşturulabilir:`/subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.HDInsight/clusters/<cluster_name>`

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

1. __Oluştur__'u seçin.


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

Azure Machine Learning için [vs Code uzantısını](tutorial-train-deploy-image-classification-model-vscode.md#configure-compute-targets) kullanarak çalışma alanınız ile ilişkili işlem hedeflerini erişebilir, oluşturabilir ve yönetebilirsiniz.

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
