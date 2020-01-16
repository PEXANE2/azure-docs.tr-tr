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
ms.openlocfilehash: 8906299cc9e2c000dab2ac9d2a345d9aaf238260
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76045851"
---
# <a name="what-are-azure-machine-learning-environments"></a>Azure Machine Learning ortamları nelerdir?
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning ortamlar, eğitim ve Puanlama betikleriniz etrafında Python paketlerini, ortam değişkenlerini ve yazılım ayarlarını belirtir. Bunlar ayrıca çalışma zamanlarını belirtir (Python, Spark veya Docker). Bunlar, farklı işlem hedefleri genelinde tekrarlanabilir, denetlenebilir ve taşınabilir makine öğrenimi iş akışlarını etkinleştiren Machine Learning çalışma alanınızda yönetilen ve sürümlü varlıklardır.

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

## <a name="next-steps"></a>Sonraki adımlar

* Azure Machine Learning [ortamında ortam oluşturma ve kullanma](how-to-use-environments.md) hakkında bilgi edinin.
* [Ortam sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py)IÇIN Python SDK başvuru belgelerine bakın.
* [Ortamlar](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-environments)IÇIN R SDK başvuru belgelerine bakın.
