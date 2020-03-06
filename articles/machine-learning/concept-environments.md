---
title: Azure Machine Learning ortamları hakkında
titleSuffix: Azure Machine Learning
description: Bu makalede, çeşitli bilgi işlem hedefleri genelinde tekrarlanabilir, denetlenebilir ve taşınabilir makine öğrenimi bağımlılık tanımlarını etkinleştiren makine öğrenimi ortamlarının avantajlarını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: trbye
author: trevorbye
ms.date: 01/06/2020
ms.openlocfilehash: 036efa27fb8d22c32f2f6bce1efe9dea300a3972
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78302788"
---
# <a name="what-are-azure-machine-learning-environments"></a>Azure Machine Learning ortamları nelerdir?
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning ortamlar, eğitim ve Puanlama betikleriniz etrafında Python paketlerini, ortam değişkenlerini ve yazılım ayarlarını belirtir. Bunlar ayrıca çalışma zamanlarını belirtir (Python, Spark veya Docker). Ortamlar, Machine Learning çalışma alanınızda, çeşitli bilgi işlem hedefleri genelinde tekrarlanabilir, denetlenebilir ve taşınabilir makine öğrenimi iş akışlarını etkinleştiren yönetilen ve sürümlü varlıklardır.

Yerel işlem sırasında `Environment` nesnesini kullanarak şunları yapabilirsiniz:
* Eğitim betiğinizi geliştirin.
* Model eğitimi için Azure Machine Learning Işlem üzerindeki aynı ortamı ölçeklendirerek yeniden kullanın.
* Modelinizi aynı ortamla dağıtın.

Aşağıdaki diyagramda, Web hizmeti dağıtımları için çalışma yapılandırmanızda, eğitim ve çıkarım ve dağıtım yapılandırmanızda tek bir `Environment` nesnesini nasıl kullanabileceğiniz gösterilmektedir.

![Machine Learning iş akışındaki bir ortamın diyagramı](./media/concept-environments/ml-environment.png)

## <a name="types-of-environments"></a>Çeşitli ortamlarda

Ortamlar, yaygın olarak üç kategoriye ayrılabilir: *seçkin*, *Kullanıcı tarafından yönetilen*ve *sistem tarafından yönetilen*.

Seçkin ortamlar Azure Machine Learning tarafından sağlanır ve varsayılan olarak çalışma alanınızda kullanılabilir. Çeşitli makine öğrenimi çerçeveleri ile çalışmaya başlamanıza yardımcı olması için Python paketleri ve ayarları koleksiyonlarını içerir. 

Kullanıcı tarafından yönetilen ortamlarda, ortamınızı ayarlamaktan ve eğitim betiğinizin işlem hedefinde ihtiyaç duyacağı her paketi yüklemenize siz sorumlusunuz. Conda, ortamınızı denetlemez veya sizin için herhangi bir şey yüklemez. Kendi ortamınızı tanımlıyorsanız, sürüm `>= 1.0.45` `azureml-defaults` bir PIP bağımlılığı olarak listeetmeniz gerekir. Bu paket, modeli bir Web hizmeti olarak barındırmak için gereken işlevselliği içerir.

Python ortamını ve komut dosyası bağımlılıklarını [yönetmek istediğinizde sistem](https://conda.io/docs/) tarafından yönetilen ortamları kullanırsınız. Hizmet, el ile yapılandırılamayan uzak işlem hedefleri üzerinde yararlarından dolayı bu tür ortamları varsayılan olarak varsayar.

## <a name="create-and-manage-environments"></a>Ortamları oluşturma ve yönetme

Ortamları şu şekilde oluşturabilirsiniz:

* Yeni `Environment` nesnelerini, seçkin bir ortamı kullanarak veya kendi bağımlılıklarınızı tanımlayarak tanımlama.
* Mevcut `Environment` nesneleri çalışma alanınızdan kullanma. Bu yaklaşım, bağımlılıklarınız ile tutarlılık ve reproducibility sağlar.
* Mevcut bir Anaconda ortam tanımından içeri aktarılıyor.
* Azure Machine Learning CLı 'yi kullanma

Belirli kod örnekleri için, [eğitim ve dağıtım için ortamları yeniden kullanma](how-to-use-environments.md#create-an-environment)' nın "ortam oluşturma" bölümüne bakın. Ortamlar, çalışma alanınız aracılığıyla da kolayca yönetilir. Bunlar aşağıdaki işlevleri içerir:

* Bir deneme gönderdiğinizde ortamlar çalışma alanınıza otomatik olarak kaydedilir. Ayrıca, el ile de kaydedilebilir.
* Eğitim veya dağıtım için kullanmak üzere çalışma alanınızdan ortamları veya ortam tanımında düzenleme yapmayı sağlayabilirsiniz.
* Sürüm oluşturma ile, ortamınızda zaman içinde yaptığınız değişiklikleri görebilirsiniz ve bu da reproducibility sağlar.
* Ortamlarınızdaki Docker görüntülerini otomatik olarak oluşturabilirsiniz.

Kod örnekleri için, [eğitim ve dağıtım için ortamları yeniden kullanma](how-to-use-environments.md#manage-environments)' nın "ortamları yönetme" bölümüne bakın.

## <a name="environment-building-caching-and-reuse"></a>Ortam oluşturma, önbelleğe alma ve yeniden kullanma

Azure Machine Learning hizmeti, Docker görüntüleri ve Conda ortamları için ortam tanımları oluşturur. Ayrıca ortamları, sonraki eğitim çalıştırmaları ve hizmet uç noktası dağıtımlarında yeniden kullanılabilmesi için önbelleğe alır.

### <a name="building-environments-as-docker-images"></a>Docker görüntüleri olarak ortam oluşturma

Genellikle, bir ortamı kullanarak bir çalıştırma gönderdiğinizde, Azure Machine Learning hizmeti, çalışma alanıyla ilişkili Azure Container Registry (ACR) üzerinde bir [ACR derleme görevi](https://docs.microsoft.com/azure/container-registry/container-registry-tasks-overview) çağırır. Oluşturulan Docker görüntüsü daha sonra çalışma alanı ACR üzerinde önbelleğe alınır. Çalıştırma yürütmesinin başlangıcında görüntü, işlem hedefi tarafından alınır.

Görüntü derlemesi iki adımdan oluşur:

 1. Temel görüntü indirme ve herhangi bir Docker adımını yürütme
 2. Ortam tanımında belirtilen Conda bağımlılıklarına göre Conda ortamı oluşturma.

[Kullanıcı tarafından yönetilen bağımlılıklar](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.pythonsection?view=azure-ml-py)belirtirseniz ikinci adım atlanır. Bu durumda, tüm Python paketlerini temel görüntlerinize dahil ederek veya ilk adımda özel Docker adımları belirterek siz sorumlusunuz. Ayrıca, Python yürütülebilir dosyası için doğru konumu belirtmekten de sorumlusunuz.

### <a name="image-caching-and-reuse"></a>Görüntü önbelleğe alma ve yeniden kullanma

Başka bir çalıştırma için aynı ortam tanımını kullanıyorsanız, Azure Machine Learning hizmeti, önbelleğe alınmış görüntüyü çalışma alanından ACR 'den yeniden kullanır. 

Önbelleğe alınmış bir görüntünün ayrıntılarını görüntülemek için [Environment. get_image_details](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#get-image-details-workspace-) yöntemi kullanın.

Önbelleğe alınmış bir görüntünün yeniden kullanılıp kullanılmayacağını veya yeni bir tane derlemenizi öğrenmek için, hizmet ortam tanımından [bir karma değer](https://en.wikipedia.org/wiki/Hash_table) hesaplar ve bunu mevcut ortamların karmalarıyla karşılaştırır. Karma şunları temel alır:
 
 * Taban görüntü özelliği değeri
 * Özel Docker adımları Özellik değeri
 * Conda Definition içindeki Python paketlerinin listesi
 * Spark tanımındaki paketlerin listesi 

Karma ortam adına veya sürümüne bağlı değildir. Bir Python paketi ekleme veya kaldırma ya da paket sürümünü değiştirme gibi ortam tanımı değişiklikleri, karma değerin görüntü yeniden oluşturmayı değiştirmesine ve tetiklemesini sağlar. Ancak, ortamınızı yeniden adlandırmanız veya var olan bir paketin tam özelliklerine ve paketlerine sahip yeni bir ortam oluşturursanız, karma değeri aynı kalır ve önbelleğe alınan görüntü kullanılır.

Üç ortam tanımını gösteren aşağıdaki diyagrama bakın. İkisinin iki farklı adı ve sürümü, ancak aynı temel görüntü ve Python paketleri vardır. Aynı karma değerine sahiptir ve bu nedenle aynı önbelleğe alınmış görüntüye karşılık gelir. Üçüncü ortamda farklı Python paketleri ve sürümleri bulunur ve bu nedenle, farklı bir önbelleğe alınmış görüntüye karşılık gelir.

![Docker görüntüleri olarak ortam önbelleğe alma diyagramı](./media/concept-environments/environment-caching.png)

Ayrılmış paket bağımlılığı olan bir ortam oluşturursanız, örneğin ```numpy```, bu ortam, ortam oluşturma sırasında yüklenen paket sürümünü kullanmaya devam edecektir. Ayrıca, eşleşen tanımı olan gelecekteki tüm ortamlar eski sürümü kullanmaya devam eder. Paketi güncelleştirmek için, görüntü yeniden oluşturmayı zorlamak için bir sürüm numarası belirtin, örneğin ```numpy==1.18.1```. Daha önce çalışan bir senaryoyu bozabilecek iç içe geçmiş olanlar dahil yeni bağımlılıkların yükleneceğini unutmayın

> [!WARNING]
>  [Environment. Build](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#build-workspace-) yöntemi, önbelleğe alınmış görüntüyü, bu önbelleğe alınmış görüntüye karşılık gelen tüm ortam tanımları için ayrılmış paketleri güncelleştirmenin ve son reproducibility güncelleştirme olasılığı ile yeniden oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Machine Learning [ortamında ortam oluşturma ve kullanma](how-to-use-environments.md) hakkında bilgi edinin.
* [Ortam sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py)IÇIN Python SDK başvuru belgelerine bakın.
* [Ortamlar](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-environments)IÇIN R SDK başvuru belgelerine bakın.
