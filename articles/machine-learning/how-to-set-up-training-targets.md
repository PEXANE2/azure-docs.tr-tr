---
title: Model eğitimi için işlem hedeflerini kullanma
titleSuffix: Azure Machine Learning
description: Makine öğrenimi modeli eğitimi için eğitim ortamlarını (bilgi işlem hedefleri) yapılandırın. Eğitim ortamları arasında kolayca geçiş yapabilirsiniz. Yerel eğitime başlayın. Ölçeklendirmeniz gerekiyorsa, bulut tabanlı bir işlem hedefine geçin.
services: machine-learning
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 03/13/2020
ms.custom: seodec18
ms.openlocfilehash: 7fcfac923da1c0daee58b10d92cbc6a6ad5e7910
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383416"
---
# <a name="set-up-and-use-compute-targets-for-model-training"></a>Model eğitimi için işlem hedeflerini ayarlama ve kullanma 
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning ile, modelinizi toplu olarak [__bilgi işlem hedefleri__](concept-azure-machine-learning-architecture.md#compute-targets)olarak adlandırılan çeşitli kaynaklar veya ortamlar üzerinde eğitebilirsiniz. İşlem hedefi yerel makine, Azure Machine Learning İşlem veya Azure HDInsight gibi bir bulut kaynağı veya uzaktaki bir sanal makine olabilir.  Ayrıca, ["Modellerinizi nerede ve nasıl dağıtacağınız"](how-to-deploy-and-where.md)adlı model dağıtımı için işlem hedefleri de oluşturabilirsiniz.

Azure Machine Learning SDK, Azure Machine Learning studio, Azure CLI veya Azure Machine Learning VS Code uzantısını kullanarak bir bilgi işlem hedefi oluşturabilir ve yönetebilirsiniz. Başka bir hizmet (örneğin, bir HDInsight kümesi) aracılığıyla oluşturulan bilgi işlem hedefleriniz varsa, bunları Azure Machine Learning çalışma alanınıza ekleyerek kullanabilirsiniz.
 
Bu makalede, model eğitimi için çeşitli bilgi işlem hedeflerini nasıl kullanacağınızı öğreneceksiniz.  Tüm bilgi işlem hedefleri için adımlar aynı iş akışını izler:
1. Zaten bir tane yoksa bir işlem hedefi __oluşturun.__
2. İşlem hedefini çalışma alanınıza __takın.__
3. İşlem hedefini Python ortamını ve komut dosyanızın gerektirdiği paket bağımlılıklarını içererek __yapılandırın.__


>[!NOTE]
> Bu makaledeki kod Azure Machine Learning SDK sürüm 1.0.74 ile test edilmiştir.

## <a name="compute-targets-for-training"></a>Eğitim için hesaplama hedefleri

Azure Machine Learning, farklı bilgi işlem hedefleri arasında farklı desteklere sahiptir. Tipik bir model geliştirme yaşam döngüsü, az miktarda veri üzerinde dev/deneme ile başlar. Bu aşamada, yerel bir ortam kullanmanızı öneririz. Örneğin, yerel bilgisayarınız veya bulut tabanlı bir VM. Eğitiminizi daha büyük veri kümelerinde ölçeklendirirken veya dağıtılmış eğitim ler yaptığınızda, her çalıştırmayı otomatik olarak ölçeklendiren tek veya çok düğümlü bir küme oluşturmak için Azure Machine Learning Compute'u kullanmanızı öneririz. Çeşitli senaryolar için destek aşağıda ayrıntılı olarak değişebilir rağmen, ayrıca kendi bilgi işlem kaynağı ekleyebilirsiniz:

[!INCLUDE [aml-compute-target-train](../../includes/aml-compute-target-train.md)]


> [!NOTE]
> Azure Machine Learning Compute kalıcı bir kaynak olarak oluşturulabilir veya bir çalıştırma isteğinde bulunduğunuzda dinamik olarak oluşturulabilir. Çalıştırma tabanlı oluşturma, eğitim çalışması tamamlandıktan sonra işlem hedefini kaldırır, bu nedenle bu şekilde oluşturulan işlem hedeflerini yeniden kullanamazsınız.

## <a name="whats-a-run-configuration"></a>Çalışma yapılandırması nedir?

Eğitim alırken, yerel bilgisayarınızda başlatmak ve daha sonra bu eğitim komut dosyasını farklı bir bilgi işlem hedefinde çalıştırmak yaygındır. Azure Machine Learning ile komut dosyanızı değiştirmenize gerek kalmadan çeşitli bilgi işlem hedeflerinde çalıştırabilirsiniz.

Tek yapmanız **gereken, bir çalıştırma yapılandırması**içindeki her işlem hedefi için ortamı tanımlamaktır.  Daha sonra, eğitim denemenizi farklı bir bilgi işlem hedefiüzerinde çalıştırmak istediğinizde, bu işlem için çalıştır yapılandırmasını belirtin. Bir ortamı belirtme ve yapılandırmayı çalıştırmak için bağlama ayrıntıları [için](how-to-use-environments.md)bkz.

Bu makalenin sonunda [deneme gönderme](#submit) hakkında daha fazla bilgi edinin.

## <a name="whats-an-estimator"></a>Tahminci nedir?

Azure Machine Learning Python SDK, popüler çerçeveleri kullanarak model eğitimini kolaylaştırmak için, tahmin sınıfı olan alternatif bir üst düzey soyutlama sağlar.  Bu sınıf, çalıştırma yapılandırmalarını kolayca oluşturmanıza olanak tanır. Seçtiğiniz herhangi bir öğrenme çerçevesini (scikit-learn gibi) kullanan eğitim komut dosyalarını göndermek için genel bir [Tahminci](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) oluşturabilir ve kullanabilirsiniz. Sizin için bir ortam veya RunConfiguration nesneleri gibi katıştak nesneleri otomatik olarak oluşturan bir tahminci kullanmanızı öneririz. Bu nesnelerin nasıl oluşturulduğu üzerinde daha fazla denetime sahip olmak ve deneme çalıştırmanız için hangi paketleri yükleyeceklerini belirtmek istiyorsanız, bir Azure Machine Learning Compute'da bir RunConfiguration nesnesi kullanarak eğitim denemelerinizi göndermek için [aşağıdaki adımları](#amlcompute) izleyin.

PyTorch, TensorFlow ve Chainer görevleri için Azure Machine Learning, bu çerçeveleri kullanarak basitleştirmek için ilgili [PyTorch,](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py) [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py)ve [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) tahmincileri de sağlar.

Daha fazla bilgi için [tahminörlü Train ML Modelleri'ne](how-to-train-ml-models.md)bakın.

## <a name="whats-an-ml-pipeline"></a>ML Boru Hattı nedir?

ML boru hatları ile iş akışınızı basitlik, hız, taşınabilirlik ve yeniden kullanımla optimize edebilirsiniz. Azure Machine Learning ile boru hatları inşa ederken, altyapı ve otomasyon yerine uzmanlığınıza, makine öğreniminize odaklanabilirsiniz.

ML boru hatları, boru hattında farklı hesaplama birimleri olan birden çok **adımdan**inşa edilir. Her adım bağımsız olarak çalıştırılabilir ve yalıtılmış işlem kaynaklarını kullanabilir. Bu, birden çok veri bilim adamının aynı anda, aşırı vergilendirici bilgi işlem kaynakları olmadan aynı anda çalışmasına olanak tanır ve ayrıca her adım için farklı işlem türleri/boyutlarının kullanılmasını kolaylaştırır.

> [!TIP]
> ML Pipelines, modelleri çalıştırırken çalıştır yapılandırması veya tahmincileri kullanabilir.

ML boru hatları modelleri eğitebilirken, eğitimden önce veri hazırlayabilir ve eğitimden sonra modelleri dağıtabilir. Ardışık hatlar için birincil kullanım durumlardan biri toplu puanlamadır. Daha fazla bilgi için [bkz: Boru Hatları: Makine öğrenimi iş akışlarını optimize edin.](concept-ml-pipelines.md)

## <a name="set-up-in-python"></a>Python'da ayarlama

Bu bilgi işlem hedeflerini yapılandırmak için aşağıdaki bölümleri kullanın:

* [Yerel bilgisayar](#local)
* [Azure Machine Learning İşlemi](#amlcompute)
* [Uzak sanal makineler](#vm)
* [Azure HDInsight](#hdinsight)


### <a name="local-computer"></a><a id="local"></a>Yerel bilgisayar

1. **Oluştur ve ekle**: Yerel bilgisayarınızı eğitim ortamı olarak kullanmak için bir bilgi işlem hedefi oluşturmaya veya eklemeye gerek yoktur.  

1. **Yapılandırma**: Yerel bilgisayarınızı bilgi işlem hedefi olarak kullandığınızda, eğitim kodu [geliştirme ortamınızda](how-to-configure-environment.md)çalıştırılır.  Bu ortamda zaten ihtiyacınız olan Python paketleri varsa, kullanıcı tarafından yönetilen ortamı kullanın.

 [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=run_local)]

Artık bilgi işleminizi iliştirdiğinize ve çalışmanızı yapılandırdığınıza göre, bir sonraki adım [eğitim çalışmasını göndermektir.](#submit)

### <a name="azure-machine-learning-compute"></a><a id="amlcompute"></a>Azure Machine Learning İşlemi

Azure Machine Learning Compute, kullanıcının tek veya çok düğümlü bir işlem oluşturmasını sağlayan yönetilen bir bilgi işlem altyapısıdır. Bilgi işlem, çalışma alanınızdaki diğer kullanıcılarla paylaşılabilen bir kaynak olarak çalışma alanı bölgenizde oluşturulur. Bir iş gönderildiğinde işlem otomatik olarak ölçeklenir ve bir Azure Sanal Ağına konulabilir. Bilgi işlem kapsayıcı bir ortamda yürütür ve model bağımlılıklarınızı [Docker kapsayıcısında](https://www.docker.com/why-docker)paketler.

Eğitim işlemini buluttaki CPU veya GPU bilgi işlem düğümleri kümesine dağıtmak için Azure Machine Learning Compute'u kullanabilirsiniz. GPU'lar içeren VM boyutları hakkında daha fazla bilgi için [GPU için optimize edilmiş sanal makine boyutlarına](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu)bakın.

Azure Machine Learning Compute, ayrılabilen çekirdek sayısı gibi varsayılan sınırlara sahiptir. Daha fazla bilgi için Azure [kaynakları için yönet ve kota isteğine](https://docs.microsoft.com/azure/machine-learning/how-to-manage-quotas)bakın.


Bir çalışma zamanlarken veya kalıcı bir kaynak olarak isteğe bağlı olarak bir Azure Machine Learning bilgi işlem ortamı oluşturabilirsiniz.

#### <a name="run-based-creation"></a>Çalıştırma tabanlı oluşturma

Çalışma zamanında bir bilgi işlem hedefi olarak Azure Machine Learning Compute'u oluşturabilirsiniz. İşlem, çalışmanız için otomatik olarak oluşturulur. İşlem tamamlandıktan sonra otomatik olarak silinir. 

> [!IMPORTANT]
> Azure Machine Learning bilgiişleminin çalıştırım tabanlı oluşturulması şu anda Önizleme'de. Otomatik hiperparametre ayarı veya otomatik makine öğrenimi kullanıyorsanız, çalıştırma tabanlı oluşturma yı kullanmayın. Hiperparametre atonlama veya otomatik makine öğrenimi kullanmak için, bunun yerine kalıcı bir [bilgi işlem](#persistent) hedefi oluşturun.

1.  **Oluşturma, iliştirme ve yapılandırma**: Çalıştırma tabanlı oluşturma, çalışma yapılandırması ile işlem hedefini oluşturmak, eklemek ve yapılandırmak için gerekli tüm adımları gerçekleştirir.  

  [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute.py?name=run_temp_compute)]


Artık bilgi işleminizi iliştirdiğinize ve çalışmanızı yapılandırdığınıza göre, bir sonraki adım [eğitim çalışmasını göndermektir.](#submit)

#### <a name="persistent-compute"></a><a id="persistent"></a>Kalıcı hesaplama

Kalıcı bir Azure Machine Learning Compute işler arasında yeniden kullanılabilir. İşlem, çalışma alanındaki diğer kullanıcılarla paylaşılabilir ve işler arasında tutulur.

1. **Oluşturma ve ekleme**: Python'da kalıcı bir Azure Machine Learning Compute kaynağı oluşturmak için **vm_size** ve **max_nodes** özelliklerini belirtin. Azure Machine Learning daha sonra diğer özellikler için akıllı varsayılanlar kullanır. İşlem otomatik olarak kullanılmadığında sıfır düğüme küçülür.   Gerektiğinde işinizi çalıştırmak için özel VM'ler oluşturulur.
    
    * **vm_size**: Azure Machine Learning Compute tarafından oluşturulan düğümlerin VM ailesi.
    * **max_nodes**: Azure Machine Learning Compute'da bir işi çalıştırdığınızda otomatik olarak ölçeklendirilebilen maksimum düğüm sayısı.
    
   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=cpu_cluster)]

   Azure Machine Learning Compute'u oluştururken birkaç gelişmiş özelliği de yapılandırabilirsiniz. Özellikler, kalıcı bir sabit boyutta küme oluşturmanıza veya aboneliğinizde varolan bir Azure Sanal Ağı içinde oluşturmanıza olanak tanır.  Ayrıntılar için [AmlCompute sınıfına](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py
    ) bakın.
    
   Veya [Azure Machine Learning stüdyosunda](#portal-create)kalıcı bir Azure Machine Learning Compute kaynağı oluşturabilir ve ekleyebilirsiniz.

1. **Configure**: Kalıcı bilgi işlem hedefi için bir çalıştırma yapılandırması oluşturun.

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=run_amlcompute)]

Artık bilgi işleminizi iliştirdiğinize ve çalışmanızı yapılandırdığınıza göre, bir sonraki adım [eğitim çalışmasını göndermektir.](#submit)


### <a name="remote-virtual-machines"></a><a id="vm"></a>Uzak sanal makineler

Azure Machine Learning, kendi bilgi işlem kaynağınızı getirmeyi ve çalışma alanınıza eklemeyi de destekler. Azure Machine Learning'den erişilebildiği sürece, bu tür bir kaynak türü rasgele uzak VM'dir. Kaynak bir Azure VM, kuruluşunuzdaki uzak bir sunucu veya şirket içi olabilir. Özellikle, IP adresi ve kimlik bilgileri (kullanıcı adı ve parola veya SSH anahtarı) göz önüne alındığında, uzaktan çalıştırmalar için erişilebilir herhangi bir VM kullanabilirsiniz.

Sistem tarafından oluşturulmuş bir conda ortamı, zaten varolan bir Python ortamı veya Docker kapsayıcısı kullanabilirsiniz. Docker konteynerini çalıştırmak için VM üzerinde çalışan bir Docker Engine'inolması gerekir. Bu işlevsellik, yerel makinenizden daha esnek, bulut tabanlı bir geliştirme/deneme ortamı istediğinizde özellikle kullanışlıdır.

Bu senaryo için Azure Veri Bilimi Sanal Makinesi'ni (DSVM) Azure VM olarak kullanın. Bu VM, Azure'da önceden yapılandırılmış bir veri bilimi ve AI geliştirme ortamıdır. VM, tam yaşam döngüsü makine öğrenimi gelişimi için çeşitli araçlar ve çerçeveler sunar. Azure Machine Learning ile DSVM'nin nasıl kullanılacağı hakkında daha fazla bilgi [için](https://docs.microsoft.com/azure/machine-learning/how-to-configure-environment#dsvm)bkz.

1. **Create**: Modelinizi eğitmek için kullanmadan önce bir DSVM oluşturun. Bu kaynağı oluşturmak [için, Linux için Veri Bilimi Sanal Makine (Ubuntu) hükmüne](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)bakın.

    > [!WARNING]
    > Azure Machine Learning yalnızca Ubuntu çalıştıran sanal makineleri destekler. Bir VM oluşturduğunuzda veya varolan bir VM seçtiğinizde, Ubuntu kullanan bir VM seçmeniz gerekir.

1. **Ekle**: Varolan bir sanal makineyi bilgi işlem hedefi olarak eklemek için, sanal makine için kaynak kimliği, kullanıcı adı ve parola sağlamanız gerekir. VM'nin kaynak kimliği, aşağıdaki dize biçimi kullanılarak abonelik kimliği, kaynak grubu adı ve VM adı kullanılarak oluşturulabilir:`/subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Compute/virtualMachines/<vm_name>`

 
   ```python
   from azureml.core.compute import RemoteCompute, ComputeTarget

   # Create the compute config 
   compute_target_name = "attach-dsvm"
   
   attach_config = RemoteCompute.attach_configuration(resource_id='<resource_id>',
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

   Veya [Azure Machine Learning stüdyosunu kullanarak](#portal-reuse)DSVM'yi çalışma alanınıza ekleyebilirsiniz.

1. **Configure**: DSVM bilgi işlem hedefi için bir çalıştırma yapılandırması oluşturun. Docker ve conda, DSVM'deki eğitim ortamını oluşturmak ve yapılandırmak için kullanılır.

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/dsvm.py?name=run_dsvm)]


Artık bilgi işleminizi iliştirdiğinize ve çalışmanızı yapılandırdığınıza göre, bir sonraki adım [eğitim çalışmasını göndermektir.](#submit)

### <a name="azure-hdinsight"></a><a id="hdinsight"></a>Azure HDInsight 

Azure HDInsight, büyük veri analitiği için popüler bir platformdur. Platform, modelinizi eğitmek için kullanılabilecek Apache Spark sağlar.

1. **Oluştur**: Modelinizi eğitmek için kullanmadan önce HDInsight kümesini oluşturun. HDInsight kümesinde Bir Kıvılcım oluşturmak için [hdinsight'ta Kıvılcım Kümesi Oluştur'a](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql)bakın. 

    Küme oluştururken, bir SSH kullanıcı adı ve parola belirtmeniz gerekir. HDInsight'ı bilgi işlem hedefi olarak kullanmaları gerektiğinden, bu değerlere dikkat edin.
    
    Küme oluşturulduktan sonra, küme adı \<> küme için sağladığınız \<ad olan ana bilgisayar adı küme adı>-ssh.azurehdinsight.net ile bağlanın. 

1. **Ekle**: Bir HDInsight kümesini bilgi işlem hedefi olarak eklemek için, HDInsight kümesi için kaynak kimliği, kullanıcı adı ve parola sağlamanız gerekir. HDInsight kümesinin kaynak kimliği, aşağıdaki dize biçimi kullanılarak abonelik kimliği, kaynak grup adı ve HDInsight küme adı kullanılarak oluşturulabilir:`/subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.HDInsight/clusters/<cluster_name>`

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

   Veya [Azure Machine Learning stüdyosunu kullanarak](#portal-reuse)HDInsight kümesini çalışma alanınıza ekleyebilirsiniz.

1. **Configure**: HDI bilgi işlem hedefi için bir çalıştırma yapılandırması oluşturun. 

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/hdi.py?name=run_hdi)]


Artık bilgi işleminizi iliştirdiğinize ve çalışmanızı yapılandırdığınıza göre, bir sonraki adım [eğitim çalışmasını göndermektir.](#submit)


### <a name="azure-batch"></a><a id="azbatch"></a>Azure Batch 

Azure Toplu İşlem, büyük ölçekli paralel ve yüksek performanslı bilgi işlem (HPC) uygulamalarını bulutta verimli bir şekilde çalıştırmak için kullanılır. AzureBatchStep, işleri Bir Azure Toplu İşlem makinesi havuzuna göndermek için Bir Azure Machine Learning Pipeline'da kullanılabilir.

Azure Toplu İşlem'i bir bilgi işlem hedefine eklemek için Azure Machine Learning SDK'yı kullanmanız ve aşağıdaki bilgileri sağlamanız gerekir:

-    **Azure Toplu İşlem adı**: Çalışma alanı içindeki işlem için kullanılacak kolay bir ad
-    **Azure Toplu Hesap Adı**: Azure Toplu İş hesabının adı
-    **Kaynak Grubu**: Azure Toplu İş hesabını içeren kaynak grubudur.

Aşağıdaki kod, Azure Toplu İşlem'in bir işlem hedefi olarak nasıl eklenebildiğini gösterir:

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

## <a name="set-up-in-azure-machine-learning-studio"></a>Azure Machine Learning stüdyosunda ayarlama

Azure Machine Learning stüdyosunda çalışma alanınızla ilişkili bilgi işlem hedeflerine erişebilirsiniz.  Stüdyoyu şu şekilde kullanabilirsiniz:

* Çalışma alanınıza bağlı [işlem hedeflerini görüntüleme](#portal-view)
* Çalışma alanınızda [bir işlem hedefi oluşturma](#portal-create)
* Çalışma alanının dışında oluşturulan [bir işlem hedefi ekleme](#portal-reuse)


Bir hedef oluşturulduktan ve çalışma alanınıza iliştirildikten sonra, `ComputeTarget` bunu çalışma yapılandırmanızda bir nesneyle kullanırsınız: 

```python
from azureml.core.compute import ComputeTarget
myvm = ComputeTarget(workspace=ws, name='my-vm-name')
```

### <a name="view-compute-targets"></a><a id="portal-view"></a>İşlem hedeflerini görüntüleme


Çalışma alanınız için hesaplama hedeflerini görmek için aşağıdaki adımları kullanın:

1. Azure [Machine Learning stüdyosuna](https://ml.azure.com)gidin.
 
1. __Uygulamalar__altında, __Bilgi İşlem'i__seçin.

    [![İşlem sekmesini görüntüleme](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace-expanded.png)

### <a name="create-a-compute-target"></a><a id="portal-create"></a>İşlem hedefi oluşturma

İşlem hedefleri listesini görüntülemek için önceki adımları izleyin. Ardından, bir işlem hedefi oluşturmak için şu adımları kullanın: 

1. Bir hesaplama hedefi eklemek için artı işaretini (+) seçin.

    ![İşlem hedefi ekleme](./media/how-to-set-up-training-targets/add-compute-target.png) 

1. İşlem hedefi için bir ad girin. 

1. Eğitim için kullanılacak bilgi işlem türü olarak **Makine Öğrenimi İşlemi'ni** seçin. __Training__ 

    >[!NOTE]
    >Azure Machine Learning Compute, Azure Machine Learning stüdyosunda oluşturabileceğiniz tek yönetilen işlem kaynağıdır.  Diğer tüm hesaplama kaynakları oluşturulduktan sonra eklenebilir.

1. Formu doldurun. Gerekli özellikleri, özellikle **VM Ailesi**ve bilgi işlem kadar döndürmek için kullanılacak **maksimum düğümdeğerleri** sağlayın.  

1. __Oluştur'u__seçin.


1. Listeden işlem hedefini seçerek oluşturma işleminin durumunu görüntüleyin:

    ![Oluşturma işlemi durumunu görüntülemek için bir işlem hedefi seçin](./media/how-to-set-up-training-targets/View_list.png)

1. Daha sonra bilgi işlem hedefiiçin ayrıntıları görürsünüz: 

    ![Bilgisayar hedef ayrıntılarını görüntüleme](./media/how-to-set-up-training-targets/compute-target-details.png) 

### <a name="attach-compute-targets"></a><a id="portal-reuse"></a>İşlem hedeflerini ekleme

Azure Machine Learning çalışma alanı dışında oluşturulan bilgi işlem hedeflerini kullanmak için bunları eklemeniz gerekir. Bir işlem hedefi eklemek, onu çalışma alanınız için kullanılabilir hale getirir.

İşlem hedefleri listesini görüntülemek için daha önce açıklanan adımları izleyin. Ardından, bir işlem hedefi eklemek için aşağıdaki adımları kullanın: 

1. Bir hesaplama hedefi eklemek için artı işaretini (+) seçin. 
1. İşlem hedefi için bir ad girin. 
1. __Eğitim__için eklenecek işlem türünü seçin:

    > [!IMPORTANT]
    > Azure Machine Learning stüdyosundan tüm bilgi işlem türleri eklenebilir. Şu anda eğitim için eklenebilir bilgi işlem türleri şunlardır:
    >
    > * Uzak bir VM
    > * Azure Databricks (makine öğrenimi boru hatlarında kullanım için)
    > * Azure Veri Gölü Analizi (makine öğrenimi boru hatlarında kullanım için)
    > * Azure HDInsight

1. Formu doldurun ve gerekli özellikler için değerler sağlayın.

    > [!NOTE]
    > Microsoft, parolalardan daha güvenli olan SSH anahtarlarını kullanmanızı önerir. Parolalar kaba kuvvet saldırılarına karşı savunmasızdır. SSH anahtarları şifreleme imzalarına dayanır. Azure Sanal Makineleri ile kullanılmak üzere SSH anahtarlarının nasıl oluşturulabildiğini öğrenmek için aşağıdaki belgelere bakın:
    >
    > * [Linux veya macOS'ta SSH tuşları oluşturma ve kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Windows'da SSH tuşları oluşturma ve kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. __Ekle'yi__seçin. 
1. Listeden işlem hedefini seçerek ekleme işleminin durumunu görüntüleyin.

## <a name="set-up-with-cli"></a>CLI ile ayarlama

Azure Machine Learning için [CLI uzantısını](reference-azure-machine-learning-cli.md) kullanarak çalışma alanınızla ilişkili bilgi işlem hedeflerine erişebilirsiniz.  CLI'yi şu şekilde kullanabilirsiniz:

* Yönetilen bir işlem hedefi oluşturma
* Yönetilen bir bilgi işlem hedefini güncelleştirme
* Yönetilmeyen bir işlem hedefi ekleme

Daha fazla bilgi için [Kaynak yönetimine](reference-azure-machine-learning-cli.md#resource-management)bakın.

## <a name="set-up-with-vs-code"></a>VS Kodu ile ayarlama

Azure Machine Learning için [VS Kodu uzantısını](tutorial-train-deploy-image-classification-model-vscode.md#configure-compute-targets) kullanarak çalışma alanınızla ilişkili bilgi işlem hedeflerine erişebilir, bunları oluşturabilir ve yönetebilirsiniz.

## <a name="submit-training-run-using-azure-machine-learning-sdk"></a><a id="submit"></a>Azure Machine Learning SDK kullanarak eğitim çalıştırma gönder

Çalıştır yapılandırması oluşturduktan sonra, denemenizi çalıştırmak için kullanırsınız.  Bir eğitim çalışması göndermek için kod deseni tüm bilgi işlem hedefleri türleri için aynıdır:

1. Çalıştırmak için bir deneme oluşturma
1. Çalıştırmayı gönderin.
1. Koşunun tamamlanmasını bekleyin.

> [!IMPORTANT]
> Eğitim çalışmasını gönderdiğinizde, eğitim komut dosyalarınızı içeren dizinin anlık görüntüsü oluşturulur ve bilgi işlem hedefine gönderilir. Ayrıca, çalışma alanınızda denemenin bir parçası olarak da depolanır. Dosyaları değiştirir ve çalıştırmayı yeniden gönderirseniz, yalnızca değiştirilen dosyalar yüklenir.
>
> Dosyaların anlık görüntüye eklenmesini önlemek için, dizinde `.amlignore` bir [.gitignore](https://git-scm.com/docs/gitignore) veya dosya oluşturun ve dosyaları bu dosyaya ekleyin. Dosya `.amlignore` [,gitignore](https://git-scm.com/docs/gitignore) dosyasıyla aynı sözdizimini ve desenleri kullanır. Her iki dosya `.amlignore` varsa, dosya önceliklidir.
> 
> Daha fazla bilgi için [Anlık Görüntüler'e](concept-azure-machine-learning-architecture.md#snapshots)bakın.

### <a name="create-an-experiment"></a>Deneme oluşturma

İlk olarak, çalışma alanınızda bir deneme oluşturun.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=experiment)]

### <a name="submit-the-experiment"></a>Denemeyi gönderme

Denemeyi bir `ScriptRunConfig` nesneyle gönderin.  Bu nesne şunları içerir:

* **source_directory**: Eğitim komut dosyanızı içeren kaynak dizini
* **komut dosyası**: Eğitim komut dosyasını tanımlayın
* **run_config**: Eğitimin nerede gerçekleşeceğini tanımlayan çalıştırma yapılandırması.

Örneğin, yerel [hedef yapılandırmasını](#local) kullanmak için:

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=local_submit)]

[Amlcompute hedefi](#amlcompute)gibi farklı bir çalıştırma yapılandırması kullanarak farklı bir işlem hedefinde çalıştırmak için aynı denemeyi geçiş:

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=amlcompute_submit)]

> [!TIP]
> Bu örnek, yalnızca eğitim için işlem hedefinin tek bir düğüm kullanarak varsayılan. Birden fazla düğüm kullanmak için, `node_count` çalıştır yapılandırmasını istenilen düğüm sayısına ayarlayın. Örneğin, aşağıdaki kod, eğitim için kullanılan düğüm sayısını dörde ayarlar:
>
> ```python
> src.run_config.node_count = 4
> ```

Veya şunları yapabilirsiniz:

* `Estimator` [Tahmincileri olan Train ML modellerinde](how-to-train-ml-models.md)gösterildiği gibi denemeyi bir nesneyle gönderin.
* [Hiperparametre atoması](how-to-tune-hyperparameters.md)için bir HyperDrive çalıştırın.
* [VS Kodu uzantısı](tutorial-train-deploy-image-classification-model-vscode.md#train-the-model)üzerinden deneme gönderin.

Daha fazla bilgi için [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) ve [RunConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py) belgelerine bakın.

## <a name="create-run-configuration-and-submit-run-using-azure-machine-learning-cli"></a>Azure Machine Learning CLI'yi kullanarak çalıştır yapılandırması oluşturma ve çalıştırma gönderme

Azure [CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ve [Machine Learning CLI uzantısını](reference-azure-machine-learning-cli.md) kullanarak çalıştırma yapılandırmaları oluşturabilir ve farklı bilgi işlem hedeflerinde çalıştırmalar gönderebilirsiniz. Aşağıdaki örnekler, varolan bir Azure Machine Learning Çalışma Alanı'na sahip `az login` olduğunuzu ve CLI komutunu kullanarak Azure'da oturum açtığınızı varsayar. 

[!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)]

### <a name="create-run-configuration"></a>Çalıştır yapılandırması oluşturma

Çalıştır yapılandırmasını oluşturmanın en basit yolu, makinenizin Python komut dosyalarını öğrenen klasöründe gezinmek ve CLI komutunu kullanmaktır

```azurecli
az ml folder attach
```

Bu komut, farklı `.azureml` işlem hedefleri için şablon çalıştırış yapılandırma dosyaları içeren bir alt klasör oluşturur. Bu dosyaları, örneğin Python paketleri eklemek veya Docker ayarlarını değiştirmek gibi yapılandırmanızı özelleştirmek için kopyalayabilir ve düzenlemeyapabilirsiniz.  

### <a name="structure-of-run-configuration-file"></a>Çalıştır yapılandırma dosyasının yapısı

Çalıştır yapılandırma dosyası YAML biçimlendirilmiştir ve aşağıdaki bölümler
 * Çalışacak komut dosyası ve bağımsız değişkenleri
 * Çalışma alanı altında "yerel" veya bir işlemin adını hesapla hedef adı.
 * Çalıştırmayı yürütmek için parametreler: çerçeve, dağıtılmış çalıştırmalar için iletişim, maksimum süre ve işlem düğümlerinin sayısı.
 * Çevre bölümü. Bu bölümdeki alanların ayrıntıları [için eğitim ve dağıtım ortamları oluşturma ve yönetme](how-to-use-environments.md) bölümüne bakın.
   * Çalıştırmak için yüklenmesi için Python paketlerini belirtmek için [conda ortamı dosyası](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#create-env-file-manually)oluşturun ve __condaDependenciesFile__ alanını ayarlayın.
 * Günlük dosya klasörünü belirtmek ve çıktı toplamayı etkinleştirmek veya devre dışı kakmak ve geçmiş anlık görüntülerini çalıştırmak için geçmiş ayrıntılarını çalıştırın.
 * Seçilen çerçeveye özgü yapılandırma ayrıntıları.
 * Veri başvurusu ve veri deposu ayrıntıları.
 * Yeni bir küme oluşturmak için Machine Learning Compute'a özgü yapılandırma ayrıntıları.

Tam bir runconfig şeması için örnek [JSON dosyasına](https://github.com/microsoft/MLOps/blob/b4bdcf8c369d188e83f40be8b748b49821f71cf2/infra-as-code/runconfigschema.json) bakın.

### <a name="create-an-experiment"></a>Deneme oluşturma

İlk olarak, çalıştırmalarınız için bir deneme oluşturun

```azurecli
az ml experiment create -n <experiment>
```

### <a name="script-run"></a>Komut dosyası çalıştırma

Komut dosyası çalıştırma göndermek için bir komut uygulama

```azurecli
az ml run submit-script -e <experiment> -c <runconfig> my_train.py
```

### <a name="hyperdrive-run"></a>HyperDrive çalıştır

Parametre aetme çalıştırmaları gerçekleştirmek için Azure CLI ile HyperDrive'ı kullanabilirsiniz. İlk olarak, aşağıdaki biçimde bir HyperDrive yapılandırma dosyası oluşturun. Hiperparametre ayarlama parametreleri hakkında ayrıntılar için [model makaleniz için Tune hiperparametrelerine](how-to-tune-hyperparameters.md) bakın.

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

Çalıştırılanın yapılandırma dosyalarının yanına bu dosyayı ekleyin. Ardından şunları kullanarak bir HyperDrive çalışması gönderin:
```azurecli
az ml run submit-hyperdrive -e <experiment> -c <runconfig> --hyperdrive-configuration-name <hdconfig> my_train.py
```

HyperDrive config'deki runconfig ve *parametre alanındaki* *bağımsız değişkenler* bölümüne dikkat edin. Bunlar, eğitim komut dosyasına geçirilecek komut satırı bağımsız değişkenlerini içerir. Runconfig'deki değer her yineleme için aynı kalırken, HyperDrive config'deki aralık üzerinde yinelenir. Her iki dosyada da aynı bağımsız değişkeni belirtmeyin.

Bu ```az ml``` CLI komutları ve tam bağımsız değişken ler kümesi hakkında daha fazla bilgi için [başvuru belgelerine](reference-azure-machine-learning-cli.md)bakın.

<a id="gitintegration"></a>

## <a name="git-tracking-and-integration"></a>Git izleme ve entegrasyon

Kaynak dizininin yerel bir Git deposu olduğu bir eğitim çalışması başlattığınızda, depo hakkındaki bilgiler çalışma geçmişinde depolanır. Daha fazla bilgi [için Azure Machine Learning için Git tümleştirmesi'ne](concept-train-model-git-integration.md)bakın.

## <a name="notebook-examples"></a>Not defteri örnekleri

Çeşitli hesaplama hedefleri ne kadar çok eğitim veren eğitim örnekleri için bu not defterlerine bakın:
* [nasıl kullanılır-azureml/eğitim](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [öğreticiler/img-sınıflandırma-part1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/image-classification-mnist-data/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Bir modeli eğitin,](tutorial-train-models-with-aml.md) bir modeli eğitmek için yönetilen bir bilgi işlem hedefi kullanır.
* Daha iyi modeller oluşturmak için [hiperparametreleri verimli bir şekilde](how-to-tune-hyperparameters.md) nasıl ayarlayabilirsiniz öğrenin.
* Eğitimli bir modele sahip olduktan sonra, [modelleri nasıl ve nerede dağıtacağınızı](how-to-deploy-and-where.md)öğrenin.
* [RunConfiguration sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) SDK başvurularını görüntüleyin.
* [Azure Sanal Ağlarla Azure Makine Öğrenimini Kullanma](how-to-enable-virtual-network.md)
