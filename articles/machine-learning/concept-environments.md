---
title: Azure Machine Learning ortamları hakkında
titleSuffix: Azure Machine Learning
description: Bu makalede, çeşitli bilgi işlem hedefleri arasında tekrarlanabilir, denetlenebilir ve taşınabilir makine öğrenimi bağımlılık tanımlarını sağlayan makine öğrenimi ortamlarının avantajlarını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: trbye
author: trevorbye
ms.date: 03/18/2020
ms.openlocfilehash: 50ddbffd00e0cbbd0641089613aaa40d03658c9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064204"
---
# <a name="what-are-azure-machine-learning-environments"></a>Azure Makine Öğrenimi ortamları nelerdir?
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning ortamları, seminer ve puanlama komut dosyalarınızın etrafında Python paketlerini, ortam değişkenlerini ve yazılım ayarlarını belirtir. Ayrıca çalışma sürelerini (Python, Spark veya Docker) belirtirler. Ortamlar, Makine Öğrenimi çalışma alanınızda çeşitli bilgi işlem hedefleri arasında çoğaltılabilir, denetlenebilir ve taşınabilir makine öğrenimi iş akışları sağlayan yönetilen ve sürümlenmiş varlıklardır.

Yerel bilgi `Environment` işleminizdeki bir nesneyi şu şekilde kullanabilirsiniz:
* Eğitim senaryonuzu geliştirin.
* Azure Machine Learning Compute'da aynı ortamı ölçekte model eğitimi için yeniden kullanın.
* Modelinizi aynı ortamla dağıtın.

Aşağıdaki diyagram, web hizmeti dağıtımları `Environment` için hem çalıştırma yapılandırmanızda, hem eğitim hem de çıkarım ve dağıtım yapılandırmanızda tek bir nesneyi nasıl kullanabileceğinizi göstermektedir.

![Makine öğrenimi iş akışında ortamın diyagramı](./media/concept-environments/ml-environment.png)

## <a name="types-of-environments"></a>Ortam türleri

Ortamlar genel olarak üç kategoriye ayrılabilir: *küratörlü,* *kullanıcı tarafından yönetilen*ve sistem tarafından *yönetilen.*

Küratörlüğünü yaptığı ortamlar Azure Machine Learning tarafından sağlanır ve varsayılan olarak çalışma alanınızda kullanılabilir. Çeşitli makine öğrenimi çerçevelerine başlamanıza yardımcı olacak Python paketleri nin ve ayarlarının koleksiyonlarını içerirler. 

Kullanıcı tarafından yönetilen ortamlarda, ortamınızı ayarlamakve eğitim komut dosyanızın ihtiyaç duyduğu her paketi bilgi işlem hedefine yüklemekten siz sorumlusunuz. Conda çevrenizi kontrol etmez veya sizin için bir şey yüklemez. Kendi ortamınızı tanımlıyorsanız, sürümle birlikte `azureml-defaults` `>= 1.0.45` pip bağımlılığı nı listelemeniz gerekir. Bu paket, modeli bir web hizmeti olarak barındırmak için gereken işlevselliği içerir.

[Conda'nın](https://conda.io/docs/) Python ortamını ve komut dosyası bağımlılıklarını sizin için yönetmesini istediğinizde sistem tarafından yönetilen ortamları kullanırsınız. Hizmet, el ile yapılandırılamayan uzak bilgi işlem hedefleri üzerindeki kullanışlılığı nedeniyle varsayılan olarak bu tür bir ortam varsayar.

## <a name="create-and-manage-environments"></a>Ortamlar oluşturma ve yönetme

Şuna göre ortamlar oluşturabilirsiniz:

* Küratörlü `Environment` bir ortam kullanarak veya kendi bağımlılıklarınızı tanımlayarak yeni nesneler tanımlama.
* Çalışma `Environment` alanınızdaki varolan nesneleri kullanma. Bu yaklaşım, bağımlılıklarınızla tutarlılık ve tekrarlanabilirlik sağlar.
* Varolan bir Anaconda ortam tanımından alma.
* Azure Machine Learning CLI'yi kullanma

Belirli kod örnekleri [için, eğitim ve dağıtım için yeniden kullanma ortamlarının](how-to-use-environments.md#create-an-environment)"Ortam Oluşturma" bölümüne bakın. Ortamlar da çalışma alanınız aracılığıyla kolayca yönetilir. Bunlar aşağıdaki işlevselliği içerir:

* Bir deneme gönderdiğinizde ortamlar otomatik olarak çalışma alanınıza kaydedilir. Ayrıca el ile de kaydedilebilirler.
* Eğitim veya dağıtım için kullanılacak ortamları çalışma alanınızdan veya ortam tanımında yapılan ları getirebilirsiniz.
* Sürümleme ile, zaman içinde ortamlarınızda yapılan değişiklikleri görebilir ve bu da tekrarlanabilirlik sağlar.
* Ortamlarınızdan Docker görüntüleri otomatik olarak oluşturabilirsiniz.

Kod örnekleri [için, eğitim ve dağıtım için yeniden kullanma ortamlarının](how-to-use-environments.md#manage-environments)"Ortamları Yönet" bölümüne bakın.

## <a name="environment-building-caching-and-reuse"></a>Çevre oluşturma, önbelleğe alma ve yeniden kullanma

Azure Machine Learning hizmeti, Docker görüntüleri ve conda ortamlarında ortam tanımları oluşturur. Ayrıca, sonraki eğitim çalıştırmalarında ve hizmet bitiş noktası dağıtımlarında yeniden kullanılabilmelerini için ortamları önbelleğe alır.

### <a name="building-environments-as-docker-images"></a>Docker görüntüleri olarak ortamlar oluşturma

Genellikle, bir ortamı kullanarak ilk çalıştırmayı gönderdiğinizde, Azure Machine Learning hizmeti Çalışma Alanıile ilişkili Azure Kapsayıcı Kayıt Defteri'nde (ACR) bir [ACR Oluşturma Görevi](https://docs.microsoft.com/azure/container-registry/container-registry-tasks-overview) çağırır. Yerleşik Docker görüntüsü daha sonra Workspace ACR üzerinde önbelleğe çıkar. Çalıştırma yürütmenin başlangıcında, görüntü işlem hedefi tarafından alınır.

Görüntü oluşturma iki adımdan oluşur:

 1. Temel görüntü indirme ve docker adımlarını yürütme
 2. Çevre tanımında belirtilen konda bağımlılıklarına göre bir konda ortamı oluşturmak.

[Kullanıcı tarafından yönetilen bağımlılıkları](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.pythonsection?view=azure-ml-py)belirtirseniz ikinci adım atlanır. Bu durumda, temel resminize ekleyerek veya ilk adımda özel Docker adımlarını belirterek, herhangi bir Python paketini yüklemekten siz sorumlusunuz. Python çalıştırılabilir için doğru konumu belirtmekten de siz sorumlusunuz.

### <a name="image-caching-and-reuse"></a>Görüntü önbelleğe alma ve yeniden kullanma

Başka bir çalışma için aynı ortam tanımını kullanıyorsanız, Azure Machine Learning hizmeti Çalışma Alanı ACR'deki önbelleğe alınan görüntüyü yeniden kullanır. 

Önbelleğe alınmış bir görüntünün ayrıntılarını görüntülemek için [Çevre.get_image_details](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#get-image-details-workspace-) yöntemini kullanın.

Önbelleğe alınmış bir görüntüyü yeniden kullanıp kullanmayacağını veya yeni bir görüntü oluşturup oluşturmayacağını belirlemek için, hizmet ortam tanımından [karma bir değer](https://en.wikipedia.org/wiki/Hash_table) oluşturur ve bunu varolan ortamların karmalarıyla karşılaştırır. Karma dayanmaktadır:
 
 * Temel görüntü özellik değeri
 * Özel docker adımları özellik değeri
 * Conda tanımındaki Python paketleri listesi
 * Kıvılcım tanımındaki paketlerin listesi 

Karma, ortam adına veya sürümüne bağlı değildir. Python paketi ekleme veya kaldırma veya paket sürümünü değiştirme gibi ortam tanımı değişiklikleri karma değerin değişmesine neden olur ve görüntüyü yeniden oluşturmayı tetikler. Ancak, yalnızca ortamınızı yeniden adlandırır veya varolan ın tam özellikleri ve paketleriyle yeni bir ortam oluşturursanız, karma değer aynı kalır ve önbelleğe alınmış görüntü kullanılır.

Üç ortam tanımını gösteren aşağıdaki diyagrama bakın. Bunlardan ikisi farklı ad ve sürüm, ancak aynı temel görüntü ve Python paketleri var. Onlar aynı karma var ve bu nedenle aynı önbelleğe görüntükarşılık gelir. Üçüncü ortamda farklı Python paketleri ve sürümleri vardır ve bu nedenle farklı önbelleğe alınmış görüntüye karşılık gelir.

![Docker görüntüleri olarak ortam önbelleğe alma diyagramı](./media/concept-environments/environment-caching.png)

>[!IMPORTANT]
> Sabitlenmemiş bir paket bağımlılığı olan bir ortam oluşturursanız, örneğin, ```numpy```bu _ortam, ortam oluşturma sırasında_yüklü paket sürümünü kullanmaya devam edecektir. Ayrıca, eşleşen tanımı ile herhangi bir gelecekteki ortam eski sürümü kullanmaya devam edecektir. 

Paketi güncelleştirmek için, örneğin ```numpy==1.18.1```görüntüyü yeniden oluşturmaya zorlamak için bir sürüm numarası belirtin. İç içe geçenler de dahil olmak üzere, daha önce çalışan bir senaryoyu bozabilecek yeni bağımlılıkların yükleneceğini unutmayın.

> [!WARNING]
>  [Environment.build](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#build-workspace--image-build-compute-none-) yöntemi, önbelleğe alınmış görüntüyü yeniden oluşturacak, sabitlenmemiş paketleri güncelleştirme ve önbelleğe alınmış görüntüye karşılık gelen tüm ortam tanımları için tekrarlanabilirliği bozma nın olası yan etkisiyle.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Machine Learning'de ortamları nasıl [oluşturup kullanacağınızı](how-to-use-environments.md) öğrenin.
* [Ortam sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py)için Python SDK başvuru belgelerine bakın.
* [Ortamlar](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-environments)için R SDK başvuru belgelerine bakın.
