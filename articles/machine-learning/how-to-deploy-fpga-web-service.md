---
title: FPGA nedir - nasıl dağıtılır
titleSuffix: Azure Machine Learning
description: Ultra düşük gecikme gecikmesi için Azure Machine Learning ile FPGA üzerinde çalışan bir modelle bir web hizmetini nasıl dağıtılayarak dağıtılamayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: jordane
author: jpe316
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: 870f7b0ab0f1d7b247435cdbb74e21801b3b052a
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81257190"
---
# <a name="what-are-field-programmable-gate-arrays-fpga-and-how-to-deploy"></a>Alan programlanabilir geçit dizileri (FPGA) nedir ve nasıl dağıtılır
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makale, alan programlanabilir kapı dizilerine (FPGA) giriş sağlar ve Azure Machine Learning'i kullanarak modellerinizi Azure FPGA'ya nasıl dağıtabileceğinizi gösterir.

FPGA’lar programlanabilen bir mantık blokları dizisi ve yeniden yapılandırılabilen bir bağlantı hiyerarşisi içerir. Ara bağlantılar, bu blokların üretimden sonra çeşitli şekillerde yapılandırılmasına olanak sağlar. Diğer yongaları ile karşılaştırıldığında, FPGA'lar programlanabilirlik ve performansın bir birleşimini sağlar.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGA'lar vs. CPU, GPU ve ASIC

Aşağıdaki diyagram ve tablo, FPGA'ların diğer işlemcilerle nasıl karşılaştırıştını gösterir.

![Azure Machine Learning FPGA karşılaştırması diyagramı](./media/how-to-deploy-fpga-web-service/azure-machine-learning-fpga-comparison.png)

|İşlemci||Açıklama|
|---|:-------:|------|
|Uygulamaya özel entegre devreler|Asıcs|Google'ın TensorFlow İşlemci Birimleri (TPU) gibi özel devreler en yüksek verimi sağlar. İhtiyaçlarınız değiştikçe yeniden yapılandırılamazlar.|
|Alan programlanabilir kapı dizileri|FPGA'lar|Azure'da bulunanlar gibi FPGA'lar, ASI'lere yakın performans sağlar. Onlar da esnek ve zaman içinde yeniden yapılandırılabilir, yeni bir mantık uygulamak için.|
|Grafik işleme birimleri|GPU’lar|AI hesaplamaları için popüler bir seçim. GPU'lar paralel işleme yetenekleri sunar ve bu da görüntü oluşturmada CPU'lardan daha hızlı olmasını sağlar.|
|Merkezi işleme birimleri|CPU Sayısı|Genel amaçlı işlemciler, hangi performans grafik ve video işleme için ideal değildir.|

Azure'daki FPGA'lar, veri bilimcileri ve geliştiricilerin gerçek zamanlı AI hesaplamalarını hızlandırmak için kullandıkları Intel'in FPGA aygıtlarına dayanır. FPGA özellikli bu mimari performans, esneklik ve ölçek sunar ve Azure'da kullanılabilir.

FPGA'lar, gerçek zamanlı çıkarım (veya model puanlama) istekleri için düşük gecikme süresi elde etmeyi mümkün kılar. Eşkron istekler (toplu iş) gerekmez. Daha fazla verinin işlenmesi gerektiğinden, toplu işlem gecikmeye neden olabilir. Nöral işleme birimlerinin uygulamaları toplu iş gerektirmez; bu nedenle gecikme, CPU ve GPU işlemcilere kıyasla birçok kez daha düşük olabilir.

### <a name="reconfigurable-power"></a>Yeniden yapılandırılabilir güç
Farklı makine öğrenimi modelleri için FPGA'ları yeniden yapılandırabilirsiniz. Bu esneklik, kullanılan en uygun sayısal hassasiyet ve bellek modeline dayalı olarak uygulamaları hızlandırmayı kolaylaştırır. FPGA'lar yeniden yapılandırılabilir olduğundan, hızla değişen AI algoritmalarının gereksinimleriyle güncel kalabilirsiniz.

## <a name="whats-supported-on-azure"></a>Azure'da desteklenenler
Microsoft Azure, FPGA'larda dünyanın en büyük bulut yatırımıdır. Bu FPGA özellikli donanım mimarisini kullanarak, eğitimli sinir ağları hızlı ve daha düşük gecikme ile çalışır. Azure, hizmetinizi ölçeklendirmek için önceden eğitilmiş derin sinir ağlarını (DNN) FPGA'lar arasında paralelleştirebilir. DNN'ler, transfer öğrenimi için derin bir featurizer olarak veya güncelleştirilmiş ağırlıklarla ince ayarlı olarak önceden eğitilebilir.

Azure'da FPGA'lar destekler:

+ Görüntü sınıflandırma ve tanıma senaryoları
+ TensorFlow dağıtımı (Tensorflow 1.x gerektirir)
+ Intel FPGA donanım

Bu DNN modelleri şu anda kullanılabilir:
  - ResNet 50
  - ResNet 152
  - DenseNet-121
  - VGG-16
  - SSD-VGG

FPGA'lar şu Azure bölgelerinde kullanılabilir:
  - Doğu ABD
  - Güneydoğu Asya
  - Batı Avrupa
  - Batı ABD 2

> [!IMPORTANT]
> Gecikme ve iş akışını optimize etmek için, müşterinizin FPGA modeline veri göndermesi yukarıdaki bölgelerden birinde olmalıdır (modeli dağıttığınız)

**Azure VM'lerin PBS Ailesi** Intel Arria 10 FPGA içerir. Azure kota tahsisinizi kontrol ettiğinizde "Standart PBS Family vCPUs" olarak gösterecektir. PB6 VM'de altı vCPUs ve bir FPGA vardır ve bir modeli FPGA'ya dağıtmanın bir parçası olarak azure ML tarafından otomatik olarak sağlanacaktır. Yalnızca Azure ML ile kullanılır ve rasgele bitakışları çalıştıramaz. Örneğin, şifreleme, kodlama, vb yapmak için bit akışları ile FPGA yanıp sönmek mümkün olmayacaktır.

### <a name="scenarios-and-applications"></a>Senaryolar ve uygulamalar

Azure FPGA'lar Azure Machine Learning ile entegre edilmiştir. Microsoft, gecikme süresini azaltmak için DNN değerlendirmesi, Bing arama sıralaması ve yazılım tanımlı ağ (SDN) ivmesi için FPGA'lar kullanırken, CPU'ları diğer görevler için serbest bırakır.

Aşağıdaki senaryolarda FPGA'lar kullanılır:
+ [Otomatik optik denetim sistemi](https://blogs.microsoft.com/ai/build-2018-project-brainwave/)

+ [Arazi örtüsü haritalama](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/)

## <a name="example-deploy-models-on-fpgas"></a>Örnek: FPGA'larda modelleri dağıtın

Azure Machine Learning Donanım Hızlandırılmış Modeller ile FPGA'larda web hizmeti olarak bir modeli dağıtabilirsiniz. FPGA'ların kullanılması, tek bir parti boyutuyla bile ultra düşük gecikme gecikmesi sağlar. Çıkarım veya model puanlama, dağıtılan modelin tahmin için kullanıldığı aşamadır, en yaygın olarak üretim verilerinde.

### <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği.  Aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

- FPGA kotası. Kotanız olup olmadığını kontrol etmek için Azure CLI'yi kullanın:

    ```azurecli-interactive
    az vm list-usage --location "eastus" -o table --query "[?localName=='Standard PBS Family vCPUs']"
    ```

    > [!TIP]
    > Diğer olası konumlar ``southeastasia`` ``westeurope``, ``westus2``, ve .

    Komut aşağıdakine benzer metni döndürür:

    ```text
    CurrentValue    Limit    LocalName
    --------------  -------  -------------------------
    0               6        Standard PBS Family vCPUs
    ```

    __CurrentValue__altında en az 6 vCPUs olduğundan emin olun.

    Kotanız yoksa, 'de [https://aka.ms/accelerateAI](https://aka.ms/accelerateAI)bir istek gönderin.

- Bir Azure Machine Learning çalışma alanı ve Python için Azure Machine Learning SDK yüklendi. Daha fazla bilgi için [bkz.](how-to-manage-workspace.md)
 
- Donanım hızlandırılmış modeller için Python SDK:

    ```bash
    pip install --upgrade azureml-accel-models[cpu]
    ```

## <a name="1-create-and-containerize-models"></a>1. Modeller oluşturma ve konteynerleme

Bu belge, giriş görüntüsünü önceden işlemek için bir TensorFlow grafiğinin nasıl oluşturulacağını, Bir FPGA'da ResNet 50'yi kullanarak bir başarıya neden olacağını ve ardından özellikleri ImageNet veri kümesinde eğitilmiş bir sınıflandırıcı aracılığıyla çalıştırmayı açıklar.

Aşağıdaki talimatları uygulayın:

* TensorFlow modelini tanımlayın
* Modeli dönüştürme
* Modeli dağıtma
* Dağıtılan modeli tüketin
* Dağıtılan hizmetleri silme

Hizmet tanımı oluşturmak [için Python için Azure Machine Learning SDK'yı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) kullanın. Hizmet tanımı, TensorFlow'a dayalı bir grafik (giriş, featurizer ve sınıflandırıcı) ardışıklığı açıklayan bir dosyadır. Dağıtım komutu, tanımı ve grafikleri bir ZIP dosyasına otomatik olarak sıkıştırır ve ZIP'i Azure Blob depolama alanına yükler. DNN zaten FPGA üzerinde çalıştırmak için konuşlandırıldı.

### <a name="load-azure-machine-learning-workspace"></a>Azure Makine Öğrenimi çalışma alanını yükleyin

Azure Makine Öğrenimi çalışma alanınızı yükleyin.

```python
import os
import tensorflow as tf

from azureml.core import Workspace

ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep='\n')
```

### <a name="preprocess-image"></a>Ön işlem görüntüsü

Web hizmetine giriş bir JPEG görüntüsüdür.  İlk adım JPEG görüntü nün şifresini çözmek ve ön işlemdir.  JPEG görüntüleri dizeleri olarak kabul edilir ve sonuç ResNet 50 modeline giriş olacak tensors vardır.

```python
# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.accel.models.utils as utils
tf.reset_default_graph()

in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="load-featurizer"></a>Yük featurizer

Modeli başlatma ve resNet50'nin sayısallaştırılmış sürümünün bir TensorFlow denetim noktasını bir featurizer olarak kullanılmak üzere indirin.  Aşağıdaki kod snippet'inde "QuantizedResnet50"yi diğer derin sinir ağlarını içe aktararak değiştirebilirsiniz:

- QuantizedResnet152
- SayısalLaştırılmışVgg16
- Densenet121

```python
from azureml.accel.models import QuantizedResnet50
save_path = os.path.expanduser('~/models')
model_graph = QuantizedResnet50(save_path, is_frozen=True)
feature_tensor = model_graph.import_graph_def(image_tensors)
print(model_graph.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Sınıflandırıcı ekle

Bu sınıflandırıcı ImageNet veri kümesi üzerinde eğitilmiştir.  Özelleştirilmiş ağırlıklarınızı aktarma ve eğitim e-örnekleri [örnek not defterleri](https://aka.ms/aml-notebooks)kümesinde mevcuttur.

```python
classifier_output = model_graph.get_default_classifier(feature_tensor)
print(classifier_output)
```

### <a name="save-the-model"></a>Modeli kaydetme

Önişlemci, ResNet 50 featurizer ve sınıflandırıcı yüklendiğine göre, grafiği ve ilişkili değişkenleri bir model olarak kaydedin.

```python
model_name = "resnet50"
model_save_path = os.path.join(save_path, model_name)
print("Saving model in {}".format(model_save_path))

with tf.Session() as sess:
    model_graph.restore_weights(sess)
    tf.saved_model.simple_save(sess, model_save_path,
                               inputs={'images': in_images},
                               outputs={'output_alias': classifier_output})
```

### <a name="save-input-and-output-tensors"></a>Giriş ve çıkış tensörleri kaydedin
Model dönüştürme ve çıkarım için ön işleme ve sınıflandırıcı adımları sırasında oluşturulan giriş ve çıkış tensörleri gerekir.

```python
input_tensors = in_images.name
output_tensors = classifier_output.name

print(input_tensors)
print(output_tensors)
```

> [!IMPORTANT]
> Giriş ve çıkış tensörleri kaydedin, çünkü model dönüştürme ve çıkarım istekleri için bunlara ihtiyacınız olacak.

Kullanılabilir modeller ve buna karşılık gelen varsayılan sınıflandırıcı çıkış tensörleri aşağıdadır ve varsayılan sınıflandırıcıyı kullanırsanız çıkarım için kullanacağınız şey budur.

+ Resnet50, QuantizedResnet50
  ```python
  output_tensors = "classifier_1/resnet_v1_50/predictions/Softmax:0"
  ```
+ Resnet152, QuantizedResnet152
  ```python
  output_tensors = "classifier/resnet_v1_152/predictions/Softmax:0"
  ```
+ Densenet121, QuantizedDensenet121
  ```python
  output_tensors = "classifier/densenet121/predictions/Softmax:0"
  ```
+ Vgg16, QuantizedVgg16
  ```python
  output_tensors = "classifier/vgg_16/fc8/squeezed:0"
  ```
+ SsdVgg, QuantizedSsdVgg
  ```python
  output_tensors = ['ssd_300_vgg/block4_box/Reshape_1:0', 'ssd_300_vgg/block7_box/Reshape_1:0', 'ssd_300_vgg/block8_box/Reshape_1:0', 'ssd_300_vgg/block9_box/Reshape_1:0', 'ssd_300_vgg/block10_box/Reshape_1:0', 'ssd_300_vgg/block11_box/Reshape_1:0', 'ssd_300_vgg/block4_box/Reshape:0', 'ssd_300_vgg/block7_box/Reshape:0', 'ssd_300_vgg/block8_box/Reshape:0', 'ssd_300_vgg/block9_box/Reshape:0', 'ssd_300_vgg/block10_box/Reshape:0', 'ssd_300_vgg/block11_box/Reshape:0']
  ```

### <a name="register-model"></a>Modeli kaydetme

SDK'yı Azure Blob depolama alanında ZIP dosyasıyla kullanarak modeli [kaydedin.](concept-model-management-and-deployment.md) Model le ilgili etiketler ve diğer meta veriler eklemek, eğitimli modellerinizi izlemenize yardımcı olur.

```python
from azureml.core.model import Model

registered_model = Model.register(workspace=ws,
                                  model_path=model_save_path,
                                  model_name=model_name)

print("Successfully registered: ", registered_model.name,
      registered_model.description, registered_model.version, sep='\t')
```

Bir modeli zaten kaydettiyseniz ve yüklemek istiyorsanız, modeli geri alabilirsiniz.

```python
from azureml.core.model import Model
model_name = "resnet50"
# By default, the latest version is retrieved. You can specify the version, i.e. version=1
registered_model = Model(ws, name="resnet50")
print(registered_model.name, registered_model.description,
      registered_model.version, sep='\t')
```

### <a name="convert-model"></a>Modeli dönüştürme

TensorFlow grafiğini Açık Nöral Ağ Değişimi biçimine[(ONNX)](https://onnx.ai/)dönüştürün.  Giriş ve çıktı tensörlerinin adlarını sağlamanız gerekir ve bu adlar web hizmetini kullandığınızda istemciniz tarafından kullanılacaktır.

```python
from azureml.accel import AccelOnnxConverter

convert_request = AccelOnnxConverter.convert_tf_model(
    ws, registered_model, input_tensors, output_tensors)

# If it fails, you can run wait_for_completion again with show_output=True.
convert_request.wait_for_completion(show_output=False)

# If the above call succeeded, get the converted model
converted_model = convert_request.result
print("\nSuccessfully converted: ", converted_model.name, converted_model.url, converted_model.version,
      converted_model.id, converted_model.created_time, '\n')
```

### <a name="create-docker-image"></a>Docker görüntüsünü oluşturma

Dönüştürülen model ve tüm bağımlılıklar Docker görüntüsüne eklenir.  Bu Docker görüntüsü daha sonra dağıtılabilir ve anında kullanılabilir.  Desteklenen dağıtım hedefleri buluttaki AKS'yi veya [Azure Veri Kutusu Kenarı](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview)gibi bir kenar aygıtını içerir.  Kayıtlı Docker resminiz için etiketler ve açıklamalar da ekleyebilirsiniz.

```python
from azureml.core.image import Image
from azureml.accel import AccelContainerImage

image_config = AccelContainerImage.image_configuration()
# Image name must be lowercase
image_name = "{}-image".format(model_name)

image = Image.create(name=image_name,
                     models=[converted_model],
                     image_config=image_config,
                     workspace=ws)
image.wait_for_creation(show_output=False)
```

Resimleri etikete göre listele ve hata ayıklama için ayrıntılı günlükleri alın.

```python
for i in Image.list(workspace=ws):
    print('{}(v.{} [{}]) stored at {} with build log {}'.format(
        i.name, i.version, i.creation_state, i.image_location, i.image_build_log_uri))
```

## <a name="2-deploy-to-cloud-or-edge"></a>2. Bulut veya kenara dağıtma

### <a name="deploy-to-the-cloud"></a>Buluta dağıt

Modelinizi yüksek ölçekli bir üretim web hizmeti olarak dağıtmak için Azure Kubernetes Hizmeti'ni (AKS) kullanın. Azure Machine Learning SDK, CLI veya [Azure Machine Learning stüdyosunu](https://ml.azure.com)kullanarak yeni bir tane oluşturabilirsiniz.

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Specify the Standard_PB6s Azure VM and location. Values for location may be "eastus", "southeastasia", "westeurope", or "westus2". If no value is specified, the default is "eastus".
prov_config = AksCompute.provisioning_configuration(vm_size = "Standard_PB6s",
                                                    agent_count = 1,
                                                    location = "eastus")

aks_name = 'my-aks-cluster'
# Create the cluster
aks_target = ComputeTarget.create(workspace=ws,
                                  name=aks_name,
                                  provisioning_configuration=prov_config)
```

AKS konuşlandırması yaklaşık 15 dakika sürebilir.  Dağıtımın başarılı olup olmadığını denetleyin.

```python
aks_target.wait_for_completion(show_output=True)
print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```

Kapsayıcıyı AKS kümesine dağıtın.
```python
from azureml.core.webservice import Webservice, AksWebservice

# For this deployment, set the web service configuration without enabling auto-scaling or authentication for testing
aks_config = AksWebservice.deploy_configuration(autoscale_enabled=False,
                                                num_replicas=1,
                                                auth_enabled=False)

aks_service_name = 'my-aks-service'

aks_service = Webservice.deploy_from_image(workspace=ws,
                                           name=aks_service_name,
                                           image=image,
                                           deployment_config=aks_config,
                                           deployment_target=aks_target)
aks_service.wait_for_deployment(show_output=True)
```

#### <a name="test-the-cloud-service"></a>Bulut hizmetini test edin
Docker görüntü gRPC ve TensorFlow Sunan "tahmin" API destekler.  Modelden öngörüler almak için Docker resmine çağırmak için örnek istemciyi kullanın.  Örnek istemci kodu kullanılabilir:
- [Python](https://github.com/Azure/aml-real-time-ai/blob/master/pythonlib/amlrealtimeai/client.py)
- [C #](https://github.com/Azure/aml-real-time-ai/blob/master/sample-clients/csharp)

TensorFlow Porsiyon kullanmak istiyorsanız, [örnek bir istemci indirebilirsiniz.](https://www.tensorflow.org/serving/setup)

```python
# Using the grpc client in Azure ML Accelerated Models SDK package
from azureml.accel import PredictionClient

address = aks_service.scoring_uri
ssl_enabled = address.startswith("https")
address = address[address.find('/')+2:].strip('/')
port = 443 if ssl_enabled else 80

# Initialize Azure ML Accelerated Models client
client = PredictionClient(address=address,
                          port=port,
                          use_ssl=ssl_enabled,
                          service_name=aks_service.name)
```

Bu sınıflandırıcı [ImageNet](http://www.image-net.org/) veri kümesi nde eğitildiği için, sınıfları insan tarafından okunabilir etiketlerle eşleyin.

```python
import requests
classes_entries = requests.get(
    "https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()

# Score image with input and output tensor names
results = client.score_file(path="./snowleopardgaze.jpg",
                            input_name=input_tensors,
                            outputs=output_tensors)

# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
```

### <a name="clean-up-the-service"></a>Hizmeti temizleme
Web hizmetinizi, resminizi ve modelinizi silin (bağımlılıklar olduğundan bu sırada yapılmalıdır).

```python
aks_service.delete()
aks_target.delete()
image.delete()
registered_model.delete()
converted_model.delete()
```

### <a name="deploy-to-a-local-edge-server"></a>Yerel kenar sunucusuna dağıtma

Tüm [Azure Veri Kutusu Kenarı aygıtları](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview
) modeli çalıştırmak için bir FPGA içerir.  FPGA'da aynı anda yalnızca bir model çalışıyor olabilir.  Farklı bir model çalıştırmak için yeni bir kapsayıcı dağıtmaniz gerekir. Talimatlar ve örnek kod [bu Azure Örneği'nde](https://github.com/Azure-Samples/aml-hardware-accelerated-models)bulunabilir.

## <a name="secure-fpga-web-services"></a>Güvenli FPGA web hizmetleri

FPGA web servislerinizi güvence altına almak için [Secure web hizmetleri](how-to-secure-web-service.md) belgesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu not defterlerine, videolara ve bloglara göz atın:

+ Birkaç [örnek not defteri](https://aka.ms/aml-accel-models-notebooks)

+ [Hiper ölçekli donanım: Azure + FPGA'nın üstündeki ölçekte ML: Build 2018 (video)](https://channel9.msdn.com/events/Build/2018/BRK3202)

+ [Microsoft FPGA tabanlı yapılandırılabilir bulutun içinde (video)](https://channel9.msdn.com/Events/Build/2017/B8063)

+ [Gerçek zamanlı AI için Project Brainwave: proje ana sayfası](https://www.microsoft.com/research/project/project-brainwave/)
