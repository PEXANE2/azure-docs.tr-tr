---
title: Azure Machine Learning ortamları hakkında
titleSuffix: Azure Machine Learning
description: Bu makalede, çeşitli bilgi işlem hedefleri genelinde tekrarlanabilir, denetlenebilir ve taşınabilir makine öğrenimi bağımlılık tanımlarını etkinleştiren makine öğrenimi ortamlarının avantajlarını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: BlackMist
ms.date: 07/08/2020
ms.openlocfilehash: 773a05b911bc8e785957b5cf93700a361dc621ef
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89651145"
---
# <a name="what-are-azure-machine-learning-environments"></a>Azure Machine Learning ortamları nelerdir?
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning ortamlar, Machine Learning eğitiminin gerçekleştiği ortamın kapsüllenmesi. Bunlar, eğitim ve Puanlama betikleriniz etrafında Python paketlerini, ortam değişkenlerini ve yazılım ayarlarını belirler. Bunlar ayrıca çalışma zamanlarını belirtir (Python, Spark veya Docker). Ortamlar, Machine Learning çalışma alanınızda, çeşitli bilgi işlem hedefleri genelinde tekrarlanabilir, denetlenebilir ve taşınabilir makine öğrenimi iş akışlarını etkinleştiren yönetilen ve sürümlü varlıklardır.

`Environment`Şunları yapmak için yerel işlem üzerinde bir nesne kullanabilirsiniz:
* Eğitim betiğinizi geliştirin.
* Model eğitimi için Azure Machine Learning Işlem üzerindeki aynı ortamı ölçeklendirerek yeniden kullanın.
* Modelinizi aynı ortamla dağıtın.
* Varolan bir modelin eğitilen ortamı yeniden ziyaret edin.

Aşağıdaki diyagramda, `Environment` çalışma yapılandırmanızda (eğitim için) ve çıkarım ve dağıtım yapılandırmanızda (Web hizmeti dağıtımları için) tek bir nesneyi nasıl kullanabileceğiniz gösterilmektedir.

![Machine Learning iş akışındaki bir ortamın diyagramı](./media/concept-environments/ml-environment.png)

Ortam, işlem hedefi ve eğitim betiği çalıştırma yapılandırması: bir eğitim çalıştırmasının tam belirtimi.

## <a name="types-of-environments"></a>Ortam türleri

Ortamlar, yaygın olarak üç kategoriye ayrılabilir: *seçkin*, *Kullanıcı tarafından yönetilen*ve *sistem tarafından yönetilen*.

Seçkin ortamlar Azure Machine Learning tarafından sağlanır ve varsayılan olarak çalışma alanınızda kullanılabilir. Olduğu gibi kullanılması amaçlanan, çeşitli makine öğrenimi çerçeveleri ile çalışmaya başlamanıza yardımcı olacak Python paketleri ve ayarları koleksiyonlarını içerir. Bu önceden oluşturulmuş ortamlar, daha hızlı dağıtım süresine de olanak tanır. Tam liste için bkz. [seçkin ortamlar makalesi](resource-curated-environments.md).

Kullanıcı tarafından yönetilen ortamlarda, ortamınızı ayarlamaktan ve eğitim betiğinizin işlem hedefinde ihtiyaç duyacağı her paketi yüklemenize siz sorumlusunuz. Conda, ortamınızı denetlemez veya sizin için herhangi bir şey yüklemez. Kendi ortamınızı tanımlıyorsanız, `azureml-defaults` sürümü `>= 1.0.45` bir PIP bağımlılığı olarak listeetmeniz gerekir. Bu paket, modeli bir Web hizmeti olarak barındırmak için gereken işlevselliği içerir.

Python ortamını ve komut dosyası bağımlılıklarını [yönetmek istediğinizde sistem](https://conda.io/docs/) tarafından yönetilen ortamları kullanırsınız. Yeni bir Conda ortamı, Conda Dependencies nesnesi temel alınarak oluşturulur. Azure Machine Learning hizmeti, el ile yapılandırılamayan uzak işlem hedefleri üzerinde kullanışlılığı nedeniyle varsayılan olarak bu tür ortamları varsayar.

## <a name="create-and-manage-environments"></a>Ortamları oluşturma ve yönetme

Ortamları şu şekilde oluşturabilirsiniz:

* Yeni `Environment` nesneleri, seçkin bir ortam kullanarak ya da kendi bağımlılıklarınızı tanımlayarak tanımlama.
* `Environment`Çalışma alanınızdan mevcut nesneleri kullanma. Bu yaklaşım, bağımlılıklarınız ile tutarlılık ve reproducibility sağlar.
* Mevcut bir Anaconda ortam tanımından içeri aktarılıyor.
* Azure Machine Learning CLı 'yi kullanma
* [VS Code uzantısını kullanma](how-to-manage-resources-vscode.md#create-environment)

Belirli kod örnekleri için, [ortamları kullanma](how-to-use-environments.md#create-an-environment)konusunun "ortam oluşturma" bölümüne bakın. Ortamlar, çalışma alanınız aracılığıyla da kolayca yönetilir. Bunlar aşağıdaki işlevleri içerir:

* Bir deneme gönderdiğinizde ortamlar çalışma alanınıza otomatik olarak kaydedilir. Ayrıca, el ile de kaydedilebilir.
* Eğitim veya dağıtım için kullanmak üzere çalışma alanınızdan ortamları veya ortam tanımında düzenleme yapmayı sağlayabilirsiniz.
* Sürüm oluşturma ile, ortamınızda zaman içinde yaptığınız değişiklikleri görebilirsiniz ve bu da reproducibility sağlar.
* Ortamlarınızdaki Docker görüntülerini otomatik olarak oluşturabilirsiniz.

Kod örnekleri için, [ortamları kullanma](how-to-use-environments.md#manage-environments)konusunun "ortamları yönetme" bölümüne bakın.

## <a name="environment-building-caching-and-reuse"></a>Ortam oluşturma, önbelleğe alma ve yeniden kullanma

Azure Machine Learning hizmeti, Docker görüntüleri ve Conda ortamları için ortam tanımları oluşturur. Ayrıca ortamları, sonraki eğitim çalıştırmaları ve hizmet uç noktası dağıtımlarında yeniden kullanılabilmesi için önbelleğe alır. Bir eğitim betiğini uzaktan çalıştırmak için bir Docker görüntüsü oluşturulması gerekir, ancak yerel bir çalıştırma doğrudan Conda ortamını kullanabilir. 

### <a name="submitting-a-run-using-an-environment"></a>Ortam kullanarak çalıştırma gönderme

Bir ortamı kullanarak bir uzak çalıştırmayı ilk gönderdiğinizde Azure Machine Learning hizmeti, çalışma alanıyla ilişkili Azure Container Registry (ACR) üzerinde bir [ACR derleme görevi](https://docs.microsoft.com/azure/container-registry/container-registry-tasks-overview) çağırır. Oluşturulan Docker görüntüsü daha sonra çalışma alanı ACR üzerinde önbelleğe alınır. Seçkin ortamlar, genel ACR 'de önbelleğe alınan Docker görüntüleri tarafından desteklenir. Çalıştırma yürütmesinin başlangıcında, görüntü, ilgili ACR 'den işlem hedefi tarafından alınır.

Yerel çalıştırmalar için, ortam tanımına bağlı olarak bir Docker veya Conda ortamı oluşturulur. Betikler daha sonra hedef işlem-yerel çalışma zamanı ortamı veya yerel Docker altyapısı üzerinde yürütülür.

### <a name="building-environments-as-docker-images"></a>Docker görüntüleri olarak ortam oluşturma

Ortam tanımı çalışma alanı ACR 'de zaten mevcut değilse yeni bir görüntü oluşturulur. Görüntü derlemesi iki adımdan oluşur:

 1. Temel görüntü indirme ve herhangi bir Docker adımını yürütme
 2. Ortam tanımında belirtilen Conda bağımlılıklarına göre Conda ortamı oluşturma.

[Kullanıcı tarafından yönetilen bağımlılıklar](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.pythonsection?view=azure-ml-py&preserve-view=true)belirtirseniz ikinci adım atlanır. Bu durumda, tüm Python paketlerini temel görüntlerinize dahil ederek veya ilk adımda özel Docker adımları belirterek siz sorumlusunuz. Ayrıca, Python yürütülebilir dosyası için doğru konumu belirtmekten de sorumlusunuz. [Özel bir Docker temel görüntüsü](how-to-deploy-custom-docker-image.md)kullanmak da mümkündür.

### <a name="image-caching-and-reuse"></a>Görüntü önbelleğe alma ve yeniden kullanma

Başka bir çalıştırma için aynı ortam tanımını kullanıyorsanız, Azure Machine Learning hizmeti, önbelleğe alınmış görüntüyü çalışma alanından ACR 'den yeniden kullanır. 

Önbelleğe alınmış bir görüntünün ayrıntılarını görüntülemek için [Environment. get_image_details](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#&preserve-view=trueget-image-details-workspace-) yöntemi kullanın.

Önbelleğe alınmış bir görüntünün yeniden kullanılıp kullanılmayacağını veya yeni bir tane derlemenizi öğrenmek için, hizmet ortam tanımından [bir karma değer](https://en.wikipedia.org/wiki/Hash_table) hesaplar ve bunu mevcut ortamların karmalarıyla karşılaştırır. Karma şunları temel alır:
 
 * Taban görüntü özelliği değeri
 * Özel Docker adımları Özellik değeri
 * Conda Definition içindeki Python paketlerinin listesi
 * Spark tanımındaki paketlerin listesi 

Karma ortam adına veya sürümüne bağlı değildir. ortamınızı yeniden adlandırırsanız veya var olan bir sürümün özelliklerine ve paketlerine sahip yeni bir ortam oluşturursanız, karma değeri aynı kalır. Ancak, bir Python paketi ekleme veya kaldırma ya da paket sürümünü değiştirme gibi ortam tanımı değişiklikleri, karma değerin değişmesine neden olur. Bir ortamdaki bağımlılıkların veya kanalların sırasını değiştirmek yeni bir ortama neden olur ve bu nedenle yeni bir görüntü derlemesi gerektirir. Bir seçkin ortamda yapılan herhangi bir değişikliğin karmayı geçersiz kılacağına ve yeni bir "seçkin" ortamda sonuçlandığına dikkat edin.

Hesaplanan karma değeri, çalışma alanındaki ve genel ACR (ya da yerel çalıştırmalar için işlem hedefi) ile karşılaştırılır. Bir eşleşme varsa, önbelleğe alınmış görüntü çekilir, aksi takdirde bir görüntü derlemesi tetiklenir. Önbelleğe alınmış bir görüntüyü çekme süresi, indirme süresini içerir, ancak yeni oluşturulan bir görüntüyü çekme süresi hem derleme süresini hem de indirme süresini içerir. 

Aşağıdaki diyagramda üç ortam tanımı gösterilmektedir. Bunlardan ikisi farklı adlara ve sürümlere sahiptir, ancak aynı temel görüntü ve Python paketlerindekilerle aynıdır. Ancak aynı karma değerine sahiptir ve bu nedenle aynı önbelleğe alınmış görüntüye karşılık gelir. Üçüncü ortamda farklı Python paketleri ve sürümleri bulunur ve bu nedenle, farklı bir önbelleğe alınmış görüntüye karşılık gelir.

![Docker görüntüleri olarak ortam önbelleğe alma diyagramı](./media/concept-environments/environment-caching.png)

>[!IMPORTANT]
> Örneğin, ayrılmış paket bağımlılığı olan bir ortam oluşturursanız, ```numpy``` Bu ortam, _ortam oluşturma sırasında_yüklenen paket sürümünü kullanmaya devam edecektir. Ayrıca, eşleşen tanımı olan gelecekteki tüm ortamlar eski sürümü kullanmaya devam eder. 

Paketi güncelleştirmek için, örneğin, görüntü yeniden oluşturmayı zorlamak için bir sürüm numarası belirtin ```numpy==1.18.1``` . İç içe geçmiş olanlar dahil yeni bağımlılıklar yüklenir, daha önce çalışan bir senaryoyu bozabilecek. 

> [!WARNING]
>  [Environment. Build](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#&preserve-view=truebuild-workspace--image-build-compute-none-) yöntemi, önbelleğe alınmış görüntüyü, bu önbelleğe alınmış görüntüye karşılık gelen tüm ortam tanımları için ayrılmış paketleri güncelleştirmenin ve son reproducibility güncelleştirme olasılığı ile yeniden oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Machine Learning [ortamında ortam oluşturma ve kullanma](how-to-use-environments.md) hakkında bilgi edinin.
* [Ortam sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py&preserve-view=true)IÇIN Python SDK başvuru belgelerine bakın.
* [Ortamlar](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-environments)IÇIN R SDK başvuru belgelerine bakın.
