---
title: ML ortamları nelerdir
titleSuffix: Azure Machine Learning
description: Bu makalede, farklı işlem hedefleri genelinde tekrarlanabilir, denetlenebilir ve taşınabilir makine öğrenimi bağımlılık tanımlarını etkinleştiren makine öğrenimi ortamlarının avantajlarını öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: trbye
author: trevorbye
ms.date: 01/06/2020
ms.openlocfilehash: ad520c7e6503f28de0bd5538662c223575a078fa
ms.sourcegitcommit: 2f8ff235b1456ccfd527e07d55149e0c0f0647cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75692738"
---
# <a name="what-are-azure-machine-learning-environments"></a>Azure Machine Learning ortamları nelerdir?
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Ortamlar, eğitim ve Puanlama betikleriniz ve çalışma zamanları (Python, Spark veya Docker) içinde Python paketlerini, ortam değişkenlerini ve yazılım ayarlarını belirtir. Bunlar, farklı işlem hedefleri genelinde tekrarlanabilir, denetlenebilir ve taşınabilir makine öğrenimi iş akışlarını etkinleştiren Azure Machine Learning çalışma alanınızda yönetilen ve sürümlü varlıklardır.

Eğitim betiğinizi geliştirmek için yerel işlem ortamınızda bir ortam nesnesi kullanabilir, aynı ortamı ölçeklendirerek model eğitimi için Azure Machine Learning Işlem sırasında yeniden kullanabilir ve hatta modelinizi aynı ortamla dağıtabilirsiniz.

Aşağıda, eğitim için çalışma yapılandırmanızda ve Web hizmeti dağıtımları için çıkarım ve dağıtım yapılandırmanızda aynı ortam nesnesinin kullanılabileceği gösterilmektedir.

![Makine öğrenimi iş akışında ortam diyagramı](./media/concept-environments/ml-environment.png)

## <a name="types-of-environments"></a>Çeşitli ortamlarda

Ortamlar, yaygın olarak üç kategoriye ayrılabilir: **seçkin**, **Kullanıcı tarafından yönetilen** ve **sistem tarafından yönetilen**.

Seçkin ortamlar Azure Machine Learning tarafından sağlanır ve varsayılan olarak çalışma alanınızda kullanılabilir. Farklı makine öğrenimi çerçeveleri kullanmaya başlamanıza yardımcı olmak için Python paketleri ve ayarları koleksiyonlarını içerir. 

Kullanıcı tarafından yönetilen bir ortam için ortamınızı ayarlamaktan ve eğitim betiğinizin ihtiyaç duyacağı her paketi işlem hedefinde yüklemeye sorumlusunuz. Conda, ortamınızı denetetmez veya sizin için herhangi bir şey yüklemez. Kendi ortamınızı tanımlıyorsanız, sürüm `>= 1.0.45` `azureml-defaults` bir PIP bağımlılığı olarak listelerinizin olduğunu lütfen unutmayın. Bu paket, modeli bir Web hizmeti olarak barındırmak için gereken işlevleri içerir.

Sistem tarafından yönetilen ortamlar, Python ortamını ve komut dosyası bağımlılıklarını yönetmek için [Conda](https://conda.io/docs/) 'yi kullanmak istediğinizde kullanılır. Hizmet, el ile yapılandırılamayan uzak işlem hedefleri üzerinde yararlarından dolayı bu tür ortamları varsayılan olarak varsayar.

## <a name="creating-and-managing-environments"></a>Ortamları oluşturma ve yönetme

Ortamları şu şekilde oluşturulabilir:

* Yeni `Environment` nesneleri tanımlama, seçkin bir ortamı kullanarak veya kendi bağımlılıklarınızı tanımlayarak
* Mevcut `Environment` nesneleri çalışma alanınızdan kullanma. Bu, bağımlılıklarınız ile tutarlılık ve reproducibility sağlar
* Mevcut bir Anaconda ortam tanımından içeri aktarılıyor.

Belirli kod örnekleri için bkz. [nasıl yapılır](how-to-use-environments.md#create-an-environment) . Ortamlar, çalışma alanınız aracılığıyla da kolayca yönetilir ve aşağıdaki işlevleri içerir:

* Bir deneme gönderdiğinizde ortamlar çalışma alanınıza otomatik olarak kaydedilir. Ayrıca, el ile de kaydedilebilir
* Çalışma alanınızdan ortamları getirin ve bu ortamları eğitim, dağıtım ya da ortam tanımında düzenleme yapmak için kullanın
* Sürüm oluşturma, zaman içinde ortamlarınızdaki değişiklikleri görmenizi sağlar ve reproducibility sağlar
* Ortamlarınızdaki Docker görüntülerini otomatik olarak oluşturun

Bkz. nasıl yapılır kodu örnekleri için [ortamları yönetme](how-to-use-environments.md#manage-environments) bölümü.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Machine Learning [ortamda ortam oluşturma ve kullanma](how-to-use-environments.md) hakkında bilgi edinin
* [Ortam sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py)IÇIN Python SDK başvuru belgelerine bakın.
* [Ortamlar](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-environments)IÇIN R SDK başvuru belgelerine bakın.