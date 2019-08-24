---
title: 'MLOps: ML modellerini yönetin, dağıtın & izleyin'
titleSuffix: Azure Machine Learning service
description: 'MLOps için Azure Machine Learning hizmetini nasıl kullanacağınızı öğrenin: modellerinizi dağıtmak, yönetmek ve izlemek için modellerinizi sürekli olarak geliştirmek üzere kullanın. Yerel makinenizde veya diğer kaynaklardan Azure Machine Learning hizmeti ile eğitilmiş modeller dağıtabilirsiniz.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: jpe316
ms.author: jordane
ms.date: 06/24/2019
ms.custom: seodec18
ms.openlocfilehash: 270ebb6247e812d42ac84698d0073b41c639cb9a
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70011582"
---
# <a name="mlops-manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>MLOps: Yönetin, dağıtın ve modeller Azure Machine Learning hizmeti ile izleme

Bu makalede, modellerinizin yaşam döngüsünü yönetmek için Azure Machine Learning hizmetini kullanma hakkında bilgi edinin. Azure Machine Learning, Machine Learning çözümlerinizin kalitesini ve tutarlılığını artıran bir Machine Learning Işlemleri (MLOps) yaklaşımı kullanır. 

Azure Machine Learning hizmeti aşağıdaki MLOps yeteneklerini sağlar:

- **ML projelerini her yerden dağıtın**
- **Çalışma ve ml ile ilgili sorunlar IÇIN ml uygulamalarını izleyin** -eğitim ve çıkarım arasındaki model girişlerini karşılaştırın, modele özgü ölçümleri bulun ve ml altyapınızda izleme ve Uyarılar sağlayın.
- Modellerden kimin yayımladığına, neden değişikliklerin yapılmakta olduğuna ve modellerin üretimde ne zaman dağıtıldığını veya üretimde kullanıldığına dahil olmak üzere **ml yaşam döngüsünün uçtan uca denetim izi oluşturmak için gereken verileri yakalayın**.
- **Azure Machine Learning ve Azure DevOps ile uçtan uca ml yaşam döngüsünü otomatikleştirerek** , sıklıkla güncelleştirme, yeni modelleri test etme ve diğer uygulama ve hizmetlerinizle bırlıkte yeni ml modellerini sürekli olarak alma.

MLOps 'nin arkasındaki kavramlar ve bunların Azure Machine Learning hizmetine nasıl uygulandığı hakkında daha fazla bilgi almak için aşağıdaki videoyu izleyin.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GX]

## <a name="deploy-ml-projects-from-anywhere"></a>ML projelerini her yerden dağıtın

### <a name="turn-your-training-process-into-a-reproducible-pipeline"></a>Eğitim işleminizi tekrarlanabilir bir işlem hattına dönüştürün
Model eğitimi sürecinizdeki tüm adımları bir araya toplamak için Azure Machine Learning ' den ML işlem hatlarını kullanın, veri hazırlama ve özellik ayıklama 'den hiper parametre ayarlama ' ya kadar model değerlendirmesi yapın.

Daha fazla bilgi için bkz. [ml işlem hatları](concept-ml-pipelines.md).

### <a name="register-and-track-ml-models"></a>ML modellerini kaydetme ve izleme

Model kaydı, modellerinizi Azure bulutu 'nda çalışma alanınızda depolamanıza ve yapılandırmanıza olanak tanır. Model kayıt defteri, eğitilen modellerinizi düzenlemenizi ve izlemenizi kolaylaştırır.

> [!TIP]
> Kayıtlı bir model, modelinizi oluşturan bir veya daha fazla dosya için mantıksal bir kapsayıcıdır. Örneğin, birden çok dosyada depolanan bir modeliniz varsa, bunları Azure Machine Learning çalışma alanınızda tek bir model olarak kaydedebilirsiniz. Kayıttan sonra, kayıtlı modeli indirebilir veya dağıtabilir ve kayıtlı tüm dosyaları alabilirsiniz.
 
Kayıtlı modeller ad ve sürüm ile tanımlanır. Mevcut bir aynı ada sahip bir model her kaydettirdiğinizde, kayıt defteri sürüm artırır. Ek meta veri etiketleri aramak modellerinde kullanılabilir kayıt sırasında de sağlayabilirsiniz. Azure Machine Learning hizmeti, Python 3.5.2 veya üzeri kullanılarak yüklenebilen tüm modelleri destekler.

> [!TIP]
> Ayrıca, Azure Machine Learning hizmeti dışında eğitilen modelleri de kaydedebilirsiniz.

Etkin bir dağıtımda kullanılmakta olan kayıtlı bir modeli silemezsiniz.
Daha fazla bilgi için, [modelleri dağıtma](how-to-deploy-and-where.md#registermodel)' nın model kaydetme bölümüne bakın.

### <a name="package-and-debug-models"></a>Paket ve hata ayıklama modelleri

Bir modeli üretime dağıtmadan önce, bir Docker görüntüsüne paketlenir. Çoğu durumda, dağıtım sırasında görüntü oluşturma arka planda otomatik olarak gerçekleşir. Gelişmiş senaryolar için görüntüyü el ile belirtebilirsiniz.

Dağıtım ile ilgili sorunlar yaşıyorsanız, sorun giderme ve hata ayıklama için yerel geliştirme ortamınızda dağıtım yapabilirsiniz.

Daha fazla bilgi için bkz. [modelleri dağıtma](how-to-deploy-and-where.md#registermodel) ve [dağıtım sorunlarını giderme](how-to-troubleshoot-deployment.md).

### <a name="validate-and-profile-models"></a>Model ve profil modellerini doğrulama

Azure Machine Learning hizmet, modelinizi dağıttığınızda kullanılacak ideal CPU ve bellek ayarlarını tespit etmek için profil oluşturmayı kullanabilir. Model doğrulama, profil oluşturma işlemi için sağladığınız verileri kullanarak bu işlemin bir parçası olarak gerçekleşir.

### <a name="convert-and-optimize-models"></a>Modelleri dönüştürme ve iyileştirme

Modelinizi [Open sinir Network Exchange](https://onnx.ai) (onnx) olarak dönüştürmek, performansı iyileştirebilir. ONNX 'e dönüştürmek, ortalama bir 2x performans artışı sağlayabilir.

Azure Machine Learning hizmeti ile ONNX hakkında daha fazla bilgi için bkz. [ml modellerini oluşturma ve hızlandırma](concept-onnx.md) makalesi.

### <a name="use-models"></a>Modelleri kullanma

Eğitilen makine öğrenimi modelleri, bulutta Web Hizmetleri olarak veya geliştirme ortamınızda yerel olarak dağıtılabilir. Modelleri, Azure IoT Edge cihazlara de dağıtabilirsiniz. Dağıtımlar, Inor sınırlaması için CPU, GPU veya alan programlanabilir geçit dizileri (FPGA) kullanabilir. Ayrıca, Power BI modelleri de kullanabilirsiniz.

Bir modeli Web hizmeti veya IoT Edge cihaz olarak kullanırken, aşağıdaki öğeleri sağlarsınız:

* Hizmete/cihaza gönderilen verileri almak için kullanılan model (ler).
* Bir giriş betiği. Bu betik istekleri kabul eder, verileri öğrenmek için modeli kullanır ve bir yanıt döndürür.
* Model (ler) ve giriş betiği için gereken bağımlılıkları açıklayan bir Conda ortam dosyası.
* Model ve giriş betiği için gereken metin, veri vb. gibi diğer varlıklar.

Bu varlıklar bir Docker görüntüsüne paketlenir ve bir Web hizmeti veya IoT Edge modülü olarak dağıtılır.

İsteğe bağlı olarak, dağıtımı daha fazla ayarlamak için aşağıdaki parametreleri kullanabilirsiniz:

* GPU 'YU etkinleştir: Docker görüntüsünde GPU desteğini etkinleştirmek için kullanılır. Görüntü, Azure Container Instances, Azure Kubernetes hizmeti, Azure Machine Learning Işlem veya Azure sanal makineleri gibi Microsoft Azure hizmetlerinde kullanılmalıdır.
* Ek Docker dosya adımları: Docker görüntüsünü oluştururken çalıştırmak için ek Docker adımları içeren bir dosya.
* Temel görüntü: Temel görüntü olarak kullanılacak özel bir görüntü. Özel bir görüntü kullanmıyorsanız, temel görüntü Azure Machine Learning hizmeti tarafından sağlanır.

Ayrıca, hedef dağıtım platformunun yapılandırmasını da sağlarsınız. Örneğin, sanal makine aile türü, kullanılabilir bellek ve Azure Kubernetes hizmetine dağıtım yaparken çekirdek sayısı.

Görüntü oluşturulduğunda, Azure Machine Learning hizmeti için gereken bileşenler de eklenir. Örneğin, Web hizmetini çalıştırmak ve IoT Edge etkileşimde bulunmak için gereken varlıklar.

> [!NOTE]
> Docker görüntüsünde kullanılan Web sunucusunu veya IoT Edge bileşenlerini değiştiremez veya değiştiremezsiniz. Azure Machine Learning hizmeti, Microsoft tarafından sınanmış ve desteklenen bir Web sunucusu yapılandırması ve IoT Edge bileşenleri kullanır.

#### <a name="web-service"></a>Web hizmeti

Aşağıdaki işlem hedefleri ile **Web hizmetlerindeki** modellerinizi kullanabilirsiniz:

* Azure Container Örneği
* Azure Kubernetes Service
* Yerel geliştirme ortamı

Modeli bir Web hizmeti olarak dağıtmak için aşağıdaki öğeleri sağlamanız gerekir:

* Modellerin modeli veya modelini işleme.
* Modeli kullanmak için gereken bağımlılıklar. Örneğin, istekleri kabul eden ve modeli çağıran bir betik, Conda bağımlılıkları, vb.
* Modelin nasıl ve nereye dağıtılacağını açıklayan dağıtım yapılandırması.

Daha fazla bilgi için bkz. [modelleri dağıtma](how-to-deploy-and-where.md).

#### <a name="iot-edge-devices"></a>IoT Edge cihazları

IoT cihazlarıyla modelleri **Azure IoT Edge modüller**aracılığıyla kullanabilirsiniz. IoT Edge modüller, cihazda çıkarım veya model Puanlama sağlayan bir donanım cihazına dağıtılır.

Daha fazla bilgi için bkz. [modelleri dağıtma](how-to-deploy-and-where.md).

### <a name="analytics"></a>Analiz

Microsoft Power BI, veri analizi için makine öğrenimi modellerinin kullanılmasını destekler. Daha fazla bilgi için bkz. [Power BI Azure Machine Learning Tümleştirme (Önizleme)](https://docs.microsoft.com/power-bi/service-machine-learning-integration).


## <a name="monitor-ml-applications-for-operational-and-ml-related-issues"></a>İşletimsel ve ML ile ilgili sorunlar için ML uygulamalarını izleyin

İzleme, modelinize hangi verilerin gönderildiğini ve döndürdüğü tahminleri anlamanıza olanak sağlar.

Bu bilgiler, modelinizin nasıl kullanıldığını anlamanıza yardımcı olur. Toplanan giriş verileri, modelin gelecek sürümlerini eğitmek için de yararlı olabilir.

Daha fazla bilgi için bkz. [model veri toplamayı etkinleştirme](how-to-enable-data-collection.md).


## <a name="capture-an-end-to-end-audit-trail-of-the-ml-lifecycle"></a>ML yaşam döngüsünün uçtan uca denetim izini yakalayın

Azure ML, tüm ML varlıklarınızın uçtan uca denetim izini izleme olanağı sunar. Bu avantajlar şunlardır:

- Azure ML, kodunuzun geldiği depoyu/dalı/yürütmeyi belirten bilgileri izlemek için [Git ile tümleşir](how-to-set-up-training-targets.md#gitintegration) .
- [Azure ML veri kümeleri](how-to-create-register-datasets.md) , verileri izlemenize ve sürümetmenize yardımcı olur.
- Azure ML çalışma geçmişi, bir modeli eğitmek için kullanılan kodun, verilerin ve işlemin bir anlık görüntüsünü depolar.
- Azure ML modeli kayıt defteri, modelinizle ilişkili tüm meta verileri yakalar (Bu, dağıtımları, dağıtımı sağlıklı ise, dağıtıldığı yerde).

## <a name="automate-the-end-to-end-ml-lifecycle"></a>Uçtan uca ML yaşam döngüsünü otomatikleştirin 

Bir modeli gösteren bir sürekli tümleştirme işlemi oluşturmak için GitHub ve Azure Pipelines kullanabilirsiniz. Tipik bir senaryoda, bir veri Bilimconu bir proje için git deposunda değişiklik denetlediğinde, Azure işlem hattı bir eğitim çalıştırması başlatır. Daha sonra, eğitilen modelin performans özelliklerini görmek için çalıştırmanın sonuçları incelenebilir. Ayrıca, modeli bir Web hizmeti olarak dağıtan bir işlem hattı da oluşturabilirsiniz.

[Azure Machine Learning uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml) , Azure Pipelines çalışmayı kolaylaştırır. Azure Pipelines için aşağıdaki geliştirmeleri sağlar:

* Hizmet bağlantısı tanımlarken çalışma alanı seçimini izin vermez.
* Eğitim ardışık düzeninde oluşturulan eğitilen modeller tarafından tetiklenecek yayın işlem hatlarını sağlar.

Azure Machine Learning ile Azure Pipelines kullanma hakkında daha fazla bilgi için, bkz. Azure Pipelines article ve [Azure Machine Learning Service MLOps](https://aka.ms/mlops) deposu [Ile ml modellerinin sürekli tümleştirilmesi ve dağıtımı](/azure/devops/pipelines/targets/azure-machine-learning) .

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [nasıl ve nerede modelleri dağıtma](how-to-deploy-and-where.md) Azure Machine Learning hizmeti ile. Dağıtım örneği için bkz [. Öğretici: Azure Container Instances](tutorial-deploy-models-with-aml.md)bir görüntü sınıflandırma modeli dağıtın.

[Azure Pipelines Ile ml modellerinin sürekli tümleştirme ve dağıtımı](/azure/devops/pipelines/targets/azure-machine-learning)oluşturmayı öğrenin. 

[Web hizmeti olarak dağıtılan bir modeli](how-to-consume-web-service.md)kullanan istemci uygulamaları ve Hizmetleri oluşturmayı öğrenin.
