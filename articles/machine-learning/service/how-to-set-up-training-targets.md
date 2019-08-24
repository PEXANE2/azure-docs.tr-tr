---
title: Oluşturma ve model yönetimi için işlem hedeflerini kullan
titleSuffix: Azure Machine Learning service
description: Machine Learning model eğitimi için eğitim ortamlarını (işlem hedefleri) yapılandırın. Eğitim ortamları arasında kolayca geçiş yapabilirsiniz. Eğitimi yerel olarak başlatın. Ölçeği ölçeklendirmeniz gerekiyorsa, bulut tabanlı bir işlem hedefine geçiş yapın.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 06/12/2019
ms.custom: seodec18
ms.openlocfilehash: b1ee18abfab2cf286ee010bd6d25dfbc5a38cebb
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70011574"
---
# <a name="set-up-compute-targets-for-model-training"></a>İşlem hedeflerine yönelik model eğitiminin ayarlama 

Azure Machine Learning hizmeti sayesinde modelinizi, toplu olarak [__işlem hedefleri__](concept-azure-machine-learning-architecture.md#compute-targets)olarak adlandırılan çeşitli kaynaklar veya ortamlar üzerinde eğitebilirsiniz. İşlem hedefi bir yerel makine veya Azure Machine Learning Işlem, Azure HDInsight veya uzak bir sanal makine gibi bir bulut kaynağı olabilir.  Model dağıtımı için, ["modellerinizi dağıtma"](how-to-deploy-and-where.md)bölümünde açıklandığı gibi işlem hedefleri de oluşturabilirsiniz.

Azure Machine Learning SDK, Azure portal, Azure CLı veya Azure Machine Learning VS Code uzantısını kullanarak bir işlem hedefi oluşturabilir ve yönetebilirsiniz. Başka bir hizmete (örneğin, bir HDInsight kümesi) oluşturulan işlem hedefleri varsa, Azure Machine Learning hizmeti çalışma alanınıza ekleyerek kullanabilirsiniz.
 
Bu makalede, model eğitimi için çeşitli işlem hedeflerini nasıl kullanacağınızı öğreneceksiniz.  Tüm işlem hedeflerinin adımları aynı iş akışını izler:
1. Henüz yoksa bir işlem hedefi __oluşturun__ .
2. İşlem hedefini çalışma alanınıza __ekleyin__ .
3. İşlem hedefini, betik için gereken Python ortamını ve paket bağımlılıklarını içerecek şekilde __yapılandırın__ .


>[!NOTE]
> Bu makaledeki kod, Azure Machine Learning SDK sürümü 1.0.39 ile test edilmiştir.

## <a name="compute-targets-for-training"></a>Eğitim için işlem hedefleri

Azure Machine Learning hizmeti farklı işlem hedefleri arasında değişen desteğe sahiptir. Az miktarda veriniz üzerinde dev/deneme ile tipik model geliştirme yaşam döngüsü başlatır. Bu aşamada, yerel bir ortamı kullanmanızı öneririz. Örneğin, yerel bilgisayarınıza veya bulut tabanlı bir VM. Büyük veri kümeleri üzerinde eğitim ölçeğini veya dağıtılmış eğitimi yapmak gibi bir Farklı Çalıştır gönderdiğiniz her zaman bu daralttığında tek veya çok node küme oluşturmak için Azure Machine Learning işlem kullanmanızı öneririz. Çeşitli senaryolarda olarak değişiklik gösterebilir destek aşağıda ayrıntılarıyla olsa da, kendi işlem kaynağı ekleyebilirsiniz:

[!INCLUDE [aml-compute-target-train](../../../includes/aml-compute-target-train.md)]


> [!NOTE]
> Azure Machine Learning Işlem, kalıcı bir kaynak olarak oluşturulabilir veya bir çalıştırma istediğinizde dinamik olarak oluşturulabilir. Çalışma tabanlı oluşturma, eğitim çalıştırması tamamlandıktan sonra işlem hedefini kaldırır, bu nedenle bu şekilde oluşturulan işlem hedeflerini yeniden kullanamazsınız.

## <a name="whats-a-run-configuration"></a>Çalıştırma Yapılandırması nedir?

Eğitim sırasında yerel bilgisayarınızda başlamak yaygındır ve daha sonra bu eğitim betiği farklı bir işlem hedefinde çalıştırılır. Azure Machine Learning hizmeti sayesinde, betiğinizi değiştirmek zorunda kalmadan komut dosyanızı çeşitli işlem hedeflerinde çalıştırabilirsiniz. 

Tüm yapmanız gereken, **çalıştırma yapılandırması**ile her bir işlem hedefi için ortamı tanımlamaktır.  Daha sonra eğitim denemenizi farklı bir işlem hedefinde çalıştırmak istediğinizde, bu işlem için çalıştırma yapılandırmasını belirtin.

Bu makalenin sonunda [denemeleri gönderme](#submit) hakkında daha fazla bilgi edinin.

### <a name="manage-environment-and-dependencies"></a>Ortamı ve bağımlılıkları yönetme

Bir çalıştırma yapılandırması oluşturduğunuzda, ortam ve bağımlılıkların işlem hedefi üzerinde nasıl yönetileceğine karar vermeniz gerekir. 

#### <a name="system-managed-environment"></a>Sistem tarafından yönetilen ortamı

Python ortamını ve komut dosyası bağımlılıklarını yönetmek istiyorsanız, [](https://conda.io/docs/) sistem tarafından yönetilen bir ortam kullanın. Sistem tarafından yönetilen bir ortam varsayılan olarak ve en sık kullanılan seçim olarak kabul edilir. Özellikle bu hedefi yapılandıradığınızda, uzak işlem hedefleri için faydalıdır. 

Tüm yapmanız gereken, [Condaddependency sınıfını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) kullanarak her bir paket bağımlılığını belirt, sonra da çalışma alanınızdaki **aml_config** dizininde paket bağımlılıkları listeniz olan **conda_dependencies. yıml** adlı bir dosya oluşturur ve Eğitim denemenizi gönderdiğinizde Python ortamınızı ayarlar. 

Yeni bir ortamın ilk kurulumu, gerekli bağımlılıkların boyutuna bağlı olarak birkaç dakika sürebilir. Paketlerin listesi değişmeden kaldığı sürece, kurulum zamanı yalnızca bir kez gerçekleşir.
  
Aşağıdaki kod, sistem tarafından yönetilen ve scikit gerektiren bir ortam için bir örnek gösterir-bilgi edinin:
    
[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/runconfig.py?name=run_system_managed)]

#### <a name="user-managed-environment"></a>Kullanıcı tarafından yönetilen ortamı

Kullanıcı tarafından yönetilen bir ortam için ortamınızı ayarlamaktan ve eğitim betiğinizin ihtiyaç duyacağı her paketi işlem hedefinde yüklemeye sorumlusunuz. Eğitim ortamınız zaten yapılandırıldıysa (örneğin, yerel makinenizde), kurulum adımını true olarak ayarlayarak `user_managed_dependencies` atlayabilirsiniz. Conda, ortamınızı denetetmez veya sizin için herhangi bir şey yüklemez.

Aşağıdaki kod, Kullanıcı tarafından yönetilen bir ortam için eğitim çalıştırmalarını yapılandırmaya ilişkin bir örnek gösterir:

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/runconfig.py?name=run_user_managed)]

## <a name="whats-an-estimator"></a>Estimator nedir?

Popüler çerçeveleri kullanarak model eğitimi kolaylaştırmak için Azure Machine Learning Python SDK, daha yüksek düzeyde bir soyutlama ve tahmin aracı sınıfı sağlar. Bu sınıf, çalışma yapılandırmasını kolayca oluşturmanıza olanak tanır. Seçtiğiniz herhangi bir öğrenme çerçevesini kullanan eğitim betikleri göndermek için genel bir [Estimator](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) oluşturabilir ve kullanabilirsiniz (örneğin, scikit-öğren).

Pytorch, TensorFlow ve Chainer görevleri için Azure Machine Learning Ayrıca bu çerçeveleri kullanmayı basitleştirmek için, ilgili [pytorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py)ve [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) tahmini 'ı da sağlar.

Daha fazla bilgi için bkz. [tahmini ile ml modellerini eğitme](how-to-train-ml-models.md).

## <a name="whats-an-ml-pipeline"></a>ML ardışık düzeni nedir?

ML işlem hatları sayesinde, basitlik, hız, taşınabilirlik ve yeniden kullanım sayesinde iş akışınızı iyileştirebilmenizi sağlayabilirsiniz. Azure Machine Learning ile işlem hatları oluştururken, altyapı ve otomasyon yerine uzmanınıza ve makine öğrenmenize odaklanırsınız.

ML işlem hatları, işlem hattının farklı hesaplama birimleri olan birden çok **adımdan**oluşturulur. Her adım bağımsız olarak çalıştırılabilir ve yalıtılmış işlem kaynakları kullanabilir. Bu, birden çok veri bilimcilerinin aynı işlem hattı üzerinde, yoğun bilgi işlem kaynakları olmadan aynı anda çalışmasına olanak tanır ve ayrıca her adım için farklı işlem türleri/boyutları kullanmayı kolaylaştırır.

> [!TIP]
> ML işlem hatları, eğitim modelleri sırasında yapılandırmayı veya tahminleri Çalıştır 'ı kullanabilir.

ML işlem hatları modelleri eğitebilir, ancak eğitim vermeden önce verileri hazırlayabilir ve eğitiminden sonra modeller dağıtabilirler. İşlem hatları için birincil kullanım çalışmalarından biri Batch Puanlama. Daha fazla bilgi için bkz [. işlem hatları: Makine öğrenimi iş](concept-ml-pipelines.md)akışlarını iyileştirin.

## <a name="set-up-in-python"></a>Python 'da ayarlama

Bu işlem hedeflerini yapılandırmak için aşağıdaki bölümleri kullanın:

* [Yerel bilgisayar](#local)
* [Azure Machine Learning işlem](#amlcompute)
* [Uzak sanal makineler](#vm)
* [Azure HDInsight](#hdinsight)


### <a id="local"></a>Yerel bilgisayar

1. **Oluşturma ve iliştirme**: Yerel bilgisayarınızı eğitim ortamı olarak kullanmak için bir işlem hedefi oluşturmanız veya eklemeniz gerekmez.  

1. **Yapılandırma**:  Yerel bilgisayarınızı bir işlem hedefi olarak kullandığınızda eğitim kodu [geliştirme ortamınızda](how-to-configure-environment.md)çalıştırılır.  Bu ortamda zaten ihtiyacınız olan Python paketleri varsa, Kullanıcı tarafından yönetilen ortamı kullanın.

 [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=run_local)]

Bu işlemi yaptıktan sonra, çalışmanızı yapılandırdığınıza göre, sonraki adım [eğitim çalıştırmasını göndermektedir](#submit).

### <a id="amlcompute"></a>Azure Machine Learning işlem

Azure Machine Learning Işlem, kullanıcının kolayca tek veya çok düğümlü bir işlem oluşturmasına olanak tanıyan bir yönetilen işlem altyapısıdır. İşlem, çalışma alanınızdaki diğer kullanıcılarla paylaşılabilecek bir kaynak olarak çalışma alanı bölgeniz içinde oluşturulur. İşlem, bir iş gönderildiğinde otomatik olarak ölçeklendirilir ve bir Azure sanal ağına yerleştirilebilir. İşlem kapsayıcılı bir ortamda yürütülür ve model bağımlılıklarınızı bir [Docker kapsayıcısında](https://www.docker.com/why-docker)paketleyebilir.

Azure Machine Learning işlem eğitim işlemin CPU veya GPU işlem düğümü bulutta bir küme dağıtmak için kullanabilirsiniz. GPU 'ları içeren VM boyutları hakkında daha fazla bilgi için bkz. [GPU ile iyileştirilmiş sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu).

Azure Machine Learning Işlem, ayrılabilen çekirdek sayısı gibi varsayılan sınırlara sahiptir. Daha fazla bilgi için bkz. [Azure kaynakları için kotaları yönetme ve isteme](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas).


Bir çalıştırmayı zamanladığınızda veya kalıcı bir kaynak olarak, isteğe bağlı olarak Azure Machine Learning işlem ortamı oluşturabilirsiniz.

#### <a name="run-based-creation"></a>Çalıştırma tabanlı oluşturma

Çalışma zamanında işlem hedefi olarak Azure Machine Learning Işlem oluşturabilirsiniz. İşlem, çalıştırmanız için otomatik olarak oluşturulur. İşlem, çalıştırma tamamlandıktan sonra otomatik olarak silinir. 

> [!NOTE]
> Kullanılacak en fazla düğüm sayısını belirtmek için normalde düğüm sayısına ayarlanır `node_count` . Şu anda (04/04/2019) bunun çalışmasını engelleyen bir hata var. Geçici bir çözüm olarak, çalıştırma `amlcompute._cluster_max_node_count` Yapılandırması özelliğini kullanın. Örneğin: `run_config.amlcompute._cluster_max_node_count = 5`.

> [!IMPORTANT]
> Azure Machine Learning işlem tabanlı oluşturma işlemi şu anda önizleme aşamasındadır. Otomatik hiper parametre ayarlama veya otomatik makine öğrenimi kullanıyorsanız, çalıştırma tabanlı oluşturma kullanmayın. Hiper parametre ayarlamayı veya otomatik makine öğrenimini kullanmak için, bunun yerine [kalıcı bir işlem](#persistent) hedefi oluşturun.

1.  **Oluşturun, iliştirin ve yapılandırın**: Çalıştırma tabanlı oluşturma, işlem hedefini çalıştırma yapılandırması ile oluşturmak, eklemek ve yapılandırmak için gerekli tüm adımları gerçekleştirir.  

  [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute.py?name=run_temp_compute)]


Bu işlemi yaptıktan sonra, çalışmanızı yapılandırdığınıza göre, sonraki adım [eğitim çalıştırmasını göndermektedir](#submit).

#### <a id="persistent"></a>Kalıcı işlem

Kalıcı bir Azure Machine Learning Işlem, işler arasında yeniden kullanılabilir. İşlem, çalışma alanındaki diğer kullanıcılarla paylaşılabilir ve işler arasında tutulur.

1. **Oluşturma ve iliştirme**: Python 'da kalıcı bir Azure Machine Learning Işlem kaynağı oluşturmak için **vm_size** ve **max_nodes** özelliklerini belirtin. Azure Machine Learning daha sonra diğer özellikler için akıllı Varsayılanları kullanır. İşlem, kullanılmazsa otomatik olarak sıfır düğümlere ölçeklenir.   İşlerinizi gerektiğinde çalıştırmak için adanmış VM 'Ler oluşturulur.
    
    * **vm_size**: Azure Machine Learning Işlem tarafından oluşturulan düğümlerin VM ailesi.
    * **max_nodes**: Azure Machine Learning Işlem sırasında bir işi çalıştırdığınızda otomatik olarak en fazla düğüm sayısı.
    
   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=cpu_cluster)]

   Ayrıca, Azure Machine Learning Işlem oluştururken birçok gelişmiş özelliği de yapılandırabilirsiniz. Özellikler, sabit boyutlu kalıcı bir küme oluşturmanıza veya mevcut bir Azure Sanal Ağa gelen aboneliğinizden bu.  ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py
    ) Ayrıntılar için [amlcompute sınıfına bakın.
    
   Ya da [Azure Portal](#portal-create)kalıcı bir Azure Machine Learning işlem kaynağı oluşturup ekleyebilirsiniz.

1. **Yapılandırma**: Kalıcı işlem hedefi için bir çalıştırma yapılandırması oluşturun.

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=run_amlcompute)]

Bu işlemi yaptıktan sonra, çalışmanızı yapılandırdığınıza göre, sonraki adım [eğitim çalıştırmasını göndermektedir](#submit).


### <a id="vm"></a>Uzak sanal makineler

Azure Machine Learning, ayrıca kendi işlem kaynağı getiren ve çalışma alanınıza eklenmesini destekler. Bu tür bir kaynak türü, Azure Machine Learning hizmetinden erişilebilen sürece rastgele bir uzak VM 'dir. Kaynak bir Azure VM 'si, kuruluşunuzdaki uzak sunucu veya şirket içi olabilir. Özellikle, IP adresi ve kimlik bilgileri (Kullanıcı adı ve parola veya SSH anahtarı) verildiğinde, uzak çalıştırmalar için erişilebilir VM 'leri kullanabilirsiniz.

Bir Docker kapsayıcısı, zaten var olan bir Python ortamını veya sistem tarafından oluşturulan conda ortamda kullanabilirsiniz. Bir Docker kapsayıcısında yürütmek için, VM 'de çalışan bir Docker altyapısına sahip olmanız gerekir. Yerel makinenize daha esnek, bulut tabanlı geliştirme/deneme ortamı istediğinizde, bu işlev özellikle yararlıdır.

Bu senaryo için tercih edilen Azure sanal makinesi olarak Azure Veri Bilimi Sanal Makinesi (DSVM) kullanın. Bu VM, Azure 'da önceden yapılandırılmış bir veri bilimi ve AI geliştirme ortamıdır. VM, tam yaşam döngüsü makine öğrenimi geliştirmesi için seçkin bir araç ve çerçeve seçeneği sunar. DSVM 'nin Azure Machine Learning ile nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [bir geliştirme ortamı yapılandırma](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-environment#dsvm).

1. **Oluşturma**: Modelinize eğitebilmeniz için kullanmadan önce bir DSVM oluşturun. Bu kaynağı oluşturmak için bkz. [Linux için veri bilimi sanal makinesi sağlama (Ubuntu)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro).

    > [!WARNING]
    > Azure Machine Learning yalnızca Ubuntu çalıştıran sanal makineleri destekler. Bir VM oluşturduğunuzda veya var olan bir VM 'yi seçtiğinizde Ubuntu kullanan bir VM seçmeniz gerekir.

1. **Ekle**: Var olan bir sanal makineyi bir işlem hedefi olarak eklemek için, sanal makine için tam etki alanı adı (FQDN), Kullanıcı adı ve parola sağlamanız gerekir. Örnekte, FQDN > değerini \<VM 'nin Genel FQDN 'si veya genel IP adresi ile değiştirin. Username \<> ve \<Password > sanal makinenin SSH Kullanıcı adı ve parolasıyla değiştirin.

   ```python
   from azureml.core.compute import RemoteCompute, ComputeTarget

   # Create the compute config 
   compute_target_name = "attach-dsvm"
   attach_config = RemoteCompute.attach_configuration(address = "<fqdn>",
                                                    ssh_port=22,
                                                    username='<username>',
                                                    password="<password>")

   # If you authenticate with SSH keys instead, use this code:
   #                                                  ssh_port=22,
   #                                                  username='<username>',
   #                                                  password=None,
   #                                                  private_key_file="<path-to-file>",
   #                                                  private_key_passphrase="<passphrase>")

   # Attach the compute
   compute = ComputeTarget.attach(ws, compute_target_name, attach_config)

   compute.wait_for_completion(show_output=True)
   ```

   Ya da [Azure Portal kullanarak](#portal-reuse)dsvm 'yi çalışma alanınıza ekleyebilirsiniz.

1. **Yapılandırma**: DSVM işlem hedefi için bir çalıştırma yapılandırması oluşturun. Docker ve Conda, DSVM 'de Eğitim ortamı oluşturmak ve yapılandırmak için kullanılır.

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/dsvm.py?name=run_dsvm)]


Bu işlemi yaptıktan sonra, çalışmanızı yapılandırdığınıza göre, sonraki adım [eğitim çalıştırmasını göndermektedir](#submit).

### <a id="hdinsight"></a>Azure HDInsight 

Azure HDInsight, büyük veri analizi için popüler bir platformdur. Platform, modelinizi eğitebilmeniz için kullanılabilecek Apache Spark sağlar.

1. **Oluşturma**:  Modelinize eğitebilmeniz için önce HDInsight kümesini oluşturun. HDInsight kümesinde Spark oluşturmak için bkz. [HDInsight 'Ta Spark kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql). 

    Kümeyi oluşturduğunuzda bir SSH Kullanıcı adı ve parolası belirtmeniz gerekir. HDInsight 'ı bir işlem hedefi olarak kullanmak için ihtiyacınız olduğu için bu değerleri de göz önünde ayırın.
    
    Küme oluşturulduktan sonra, ana bilgisayar \<adı clustername >-SSH.azurehdinsight.NET ile bağlanın, burada \<clustername >, küme için verdiğiniz addır. 

1. **Ekle**: Bir HDInsight kümesini işlem hedefi olarak eklemek için, HDInsight kümesi için konak adı, Kullanıcı adı ve parola sağlamalısınız. Aşağıdaki örnek, bir küme çalışma alanınıza eklemek için SDK'sını kullanır. Örnekte, clustername > değerini \<kümenizin adıyla değiştirin. Username \<> ve \<Password >, küme için SSH Kullanıcı adı ve parolasıyla değiştirin.

   ```python
   from azureml.core.compute import ComputeTarget, HDInsightCompute
   from azureml.exceptions import ComputeTargetException

   try:
    # if you want to connect using SSH key instead of username/password you can provide parameters private_key_file and private_key_passphrase
    attach_config = HDInsightCompute.attach_configuration(address='<clustername>-ssh.azureinsight.net', 
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

   Ya da [Azure Portal kullanarak](#portal-reuse)HDInsight kümesini çalışma alanınıza ekleyebilirsiniz.

1. **Yapılandırma**: HDI işlem hedefi için bir çalıştırma yapılandırması oluşturun. 

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/hdi.py?name=run_hdi)]


Bu işlemi yaptıktan sonra, çalışmanızı yapılandırdığınıza göre, sonraki adım [eğitim çalıştırmasını göndermektedir](#submit).


### <a id="azbatch"></a>Azure Batch 

Azure Batch, büyük ölçekli paralel ve yüksek performanslı bilgi işlem (HPC) uygulamalarını bulutta verimli bir şekilde çalıştırmak için kullanılır. AzureBatchStep bir makine havuzuna Azure Batch iş göndermek için bir Azure Machine Learning işlem hattında kullanılabilir.

İşlem hedefi olarak Azure Batch iliştirmek için Azure Machine Learning SDK 'sını kullanmanız ve aşağıdaki bilgileri sağlamanız gerekir:

-   **Azure Batch işlem adı**: Çalışma alanı içinde işlem için kullanılacak kolay bir ad
-   **Azure Batch hesap adı**: Azure Batch hesabının adı
-   **Kaynak grubu**: Azure Batch hesabını içeren kaynak grubu.

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

## <a name="set-up-in-azure-portal"></a>Azure portal ayarla

Azure portal çalışma alanınız ile ilişkili işlem hedeflerine erişebilirsiniz.  Portalı kullanarak şunları yapabilirsiniz:

* Çalışma alanınıza bağlı [işlem hedeflerini görüntüleme](#portal-view)
* Çalışma alanınızda [bir işlem hedefi oluşturma](#portal-create)
* Çalışma alanının dışında oluşturulmuş [bir işlem hedefi iliştirme](#portal-reuse)

Bir hedef oluşturulduktan ve çalışma alanınıza eklendikten sonra, çalışma yapılandırmanızda bunu bir `ComputeTarget` nesneyle kullanacaksınız: 

```python
from azureml.core.compute import ComputeTarget
myvm = ComputeTarget(workspace=ws, name='my-vm-name')
```

### <a id="portal-view"></a>İşlem hedeflerini görüntüleme


Çalışma alanınızın işlem hedeflerini görmek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com) gidin ve çalışma alanınızı açın. 
1. __Uygulamalar__' ın altında, __işlem__' ı seçin.

    [![İşlem sekmesini görüntüle](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace-expanded.png)

### <a id="portal-create"></a>İşlem hedefi oluştur

İşlem hedeflerinin listesini görüntülemek için önceki adımları izleyin. Daha sonra bir işlem hedefi oluşturmak için bu adımları kullanın: 

1. Bir işlem hedefi eklemek için artı işaretini (+) seçin.

    ![İşlem hedefi ekleme](./media/how-to-set-up-training-targets/add-compute-target.png) 

1. İşlem hedefi için bir ad girin. 

1. __Eğitim__için kullanılacak işlem türü olarak **Machine Learning işlem** seçin. 

    >[!NOTE]
    >Azure Machine Learning Işlem, Azure portal oluşturabileceğiniz tek yönetilen işlem kaynağıdır.  Diğer tüm işlem kaynakları, oluşturulduktan sonra eklenebilir.

1. Formu doldurun. Gerekli özellikler, özellikle **VM ailesi**ve işlem için kullanılacak **en fazla düğüm** değerlerini sağlayın.  

1. __Oluştur__’u seçin.


1. Listeden işlem hedefini seçerek oluşturma işleminin durumunu görüntüleyin:

    ![Oluşturma işlemi durumunu görüntülemek için bir işlem hedefi seçin](./media/how-to-set-up-training-targets/View_list.png)

1. Daha sonra işlem hedefinin ayrıntılarını görürsünüz: 

    ![Bilgisayar hedefi ayrıntılarını görüntüleme](./media/how-to-set-up-training-targets/compute-target-details.png) 

### <a id="portal-reuse"></a>İşlem hedeflerini iliştirme

Azure Machine Learning hizmet çalışma alanı dışında oluşturulan işlem hedeflerini kullanmak için, onları iliştirmeli. İşlem hedefini iliştirmek, çalışma alanınız için kullanılabilir hale getirir.

Daha önce açıklanan adımları izleyerek işlem hedeflerinin listesini görüntüleyin. Daha sonra bir işlem hedefi eklemek için aşağıdaki adımları kullanın: 

1. Bir işlem hedefi eklemek için artı işaretini (+) seçin. 
1. İşlem hedefi için bir ad girin. 
1. __Eğitim__için iliştirilecek işlem türünü seçin:

    > [!IMPORTANT]
    > Tüm işlem türleri Azure portal iliştirilemez. Şu anda eğitim için iliştirilebilecek işlem türleri şunlardır:
    >
    > * Uzak VM
    > * Azure Databricks (makine öğrenimi ardışık düzenleri kullanımı için)
    > * Azure Data Lake Analytics (makine öğrenimi ardışık düzenleri kullanımı için)
    > * Azure HDInsight

1. Formu doldurun ve gerekli özellikler için değerler sağlayın.

    > [!NOTE]
    > Microsoft, parolalardan daha güvenli olan SSH anahtarlarını kullanmanızı önerir. Parolalar, deneme yanılma saldırılarına karşı savunmasızdır. SSH anahtarları şifreleme imzaları kullanır. Azure sanal makineler ile kullanmak üzere SSH anahtarları oluşturma hakkında bilgi için aşağıdaki belgelere bakın:
    >
    > * [Oluşturma ve Linux veya Macos'ta SSH anahtarlarını kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Oluşturma ve Windows üzerinde SSH anahtarlarını kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. Seçin __ekleme__. 
1. Listeden işlem hedefini seçerek iliştirme işleminin durumunu görüntüleyin.

## <a name="set-up-with-cli"></a>CLı ile ayarlama

Azure Machine Learning hizmeti için [CLI uzantısını](reference-azure-machine-learning-cli.md) kullanarak çalışma alanınız ile ilişkili işlem hedeflerine erişebilirsiniz.  CLı 'yi kullanarak şunları yapabilirsiniz:

* Yönetilen işlem hedefi oluşturma
* Yönetilen bir işlem hedefini güncelleştirme
* Yönetilmeyen bir işlem hedefi iliştirme

Daha fazla bilgi için bkz. [kaynak yönetimi](reference-azure-machine-learning-cli.md#resource-management).

## <a name="set-up-with-vs-code"></a>VS Code ayarla

Azure Machine Learning hizmeti için [vs Code uzantısını](how-to-vscode-tools.md#create-and-manage-compute-targets) kullanarak çalışma alanınız ile ilişkili işlem hedeflerini erişebilir, oluşturabilir ve yönetebilirsiniz.

## <a id="submit"></a>Eğitim çalıştırmasını gönder

Bir çalıştırma yapılandırması oluşturduktan sonra, deneme hesabınızı çalıştırmak için onu kullanırsınız.  Eğitim çalışması göndermek için kod deseninin her türlü bilgi işlem hedefi vardır:

1. Çalıştırmak için bir deneme oluşturun
1. Çalıştırma gönderin.
1. Çalıştırmak için bekleyin.

> [!IMPORTANT]
> Eğitim çalıştırmasını gönderdiğinizde, eğitim betiklerinizi içeren dizinin bir anlık görüntüsü oluşturulur ve işlem hedefine gönderilir. Ayrıca çalışma alanınızdaki denemenin bir parçası olarak da depolanır. Dosyaları değiştirir ve çalışmayı yeniden gönderirseniz yalnızca değiştirilen dosyalar karşıya yüklenir.
>
> Dosyaların anlık görüntüye eklenmesini engellemek için, dizinde bir [. gitignore](https://git-scm.com/docs/gitignore) veya `.amlignore` dosya oluşturun ve dosyaları bu dosyaya ekleyin. Dosya,. gitignore dosyası ile aynı söz dizimini ve desenleri kullanır. [](https://git-scm.com/docs/gitignore) `.amlignore` Her iki dosya de varsa, `.amlignore` dosya önceliklidir.
> 
> Daha fazla bilgi için bkz. [anlık görüntüler](concept-azure-machine-learning-architecture.md#snapshots).

### <a name="create-an-experiment"></a>Deneme oluşturma

İlk olarak, çalışma alanınızda bir deneme oluşturun.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=experiment)]

### <a name="submit-the-experiment"></a>Denemeyi gönderme

Denemeyi bir `ScriptRunConfig` nesneyle birlikte gönder.  Bu nesne şunları içerir:

* **source_directory**: Eğitim betiğinizi içeren kaynak dizin
* **betik**: Eğitim betiğini tanımla
* **run_config**: Çalıştırılan çalıştırma yapılandırması, eğitimin nerede olacağını tanımlar.

Örneğin, [yerel hedef](#local) yapılandırmasını kullanmak için:

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=local_submit)]

Aynı denemeyi, [amlcompute hedefi](#amlcompute)gibi farklı bir çalıştırma yapılandırması kullanarak farklı bir işlem hedefinde çalışacak şekilde değiştirin:

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=amlcompute_submit)]

İsterseniz şunları yapabilirsiniz:

* Denemesi, `Estimator` [ml modellerini tahmini ile eğitme](how-to-train-ml-models.md)bölümünde gösterildiği gibi bir nesneyle birlikte gönder.
* [CLI uzantısını kullanarak](reference-azure-machine-learning-cli.md#experiments)bir deneme gönderir.
* [Vs Code uzantısı](how-to-vscode-tools.md#train-and-tune-models)aracılığıyla bir deneme gönderir.

<a id="gitintegration"></a>

## <a name="git-tracking-and-integration"></a>Git izleme ve Tümleştirme

Kaynak dizinin yerel bir git deposu olduğu bir eğitim çalıştırması başlattığınızda, depo hakkındaki bilgiler çalıştırma geçmişinde depolanır. Örneğin, deponun geçerli kayıt KIMLIĞI, geçmişin bir parçası olarak günlüğe kaydedilir.

## <a name="notebook-examples"></a>Not defteri örnekleri

Çeşitli işlem hedeflerine yönelik eğitim örnekleri için bu not defterlerine bakın:
* [Yardım-How-to-kullanın-azureml/eğitimi](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [öğreticiler/img-sınıflandırma-bölüm 1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Bir modeli](tutorial-train-models-with-aml.md) eğitme bir modeli eğmek için yönetilen bir işlem hedefi kullanır.
* Daha iyi modeller oluşturmak için [hiper parametreleri verimli](how-to-tune-hyperparameters.md) bir şekilde ayarlamayı öğrenin.
* Eğitilen bir modelden sonra [modellerin nasıl ve nereye dağıtılacağını](how-to-deploy-and-where.md)öğrenin.
* [RunConfiguration sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) SDK başvurusunu görüntüleyin.
* [Azure sanal ağları ile Azure Machine Learning hizmetini kullanma](how-to-enable-virtual-network.md)
