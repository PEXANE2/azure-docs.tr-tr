---
title: 'MLOps: ML model yönetimi'
titleSuffix: Azure Machine Learning
description: 'Azure Machine Learning (MLOps) ile model yönetimi hakkında bilgi edinin. Modellerini sürekli olarak geliştirmek için modellerinizi dağıtın, yönetin ve izleyin. '
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: jpe316
ms.author: jordane
ms.date: 11/22/2019
ms.custom: seodec18
ms.openlocfilehash: 9cde171f1c031b4b02a71fe0dac1a689e3ec0722
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75772593"
---
# <a name="mlops-model-management-deployment-and-monitoring-with-azure-machine-learning"></a>MLOps: model yönetimi, dağıtım ve Azure Machine Learning ile izleme

Bu makalede, modellerinizin yaşam döngüsünü yönetmek için Azure Machine Learning kullanma hakkında bilgi edinin. Azure Machine Learning, Machine Learning Işlemler (MLOps) yaklaşımı kullanır. MLOps, Machine Learning çözümlerinizin kalitesini ve tutarlılığını geliştirir. 

Azure Machine Learning aşağıdaki MLOps yeteneklerini sağlar:

- **Tekrarlanabilir ml işlem hatları oluşturun**. İşlem hatları, veri hazırlama, eğitim ve Puanlama işlemleriniz için yinelenebilir ve yeniden kullanılabilir adımlar tanımlamanızı sağlar.
- **Modelleri her yerden kaydedin, paketleyin ve dağıtın** ve modeli kullanmak için gereken ilişkili meta verileri izleyin.
- Modellerden kimin yayımladığına, değişikliklerin neden yapılmakta olduğuna ve modellerin üretimde ne zaman dağıtıldığını ve ne zaman kullanıldığını de kapsayan, **uçtan uca ml yaşam döngüsünü yakalamak için gereken idare verilerini yakalayın**.
- Deneme tamamlama, model kaydı, model dağıtımı ve veri DRI algılama gibi **ml yaşam döngüsünün olaylarını uyarın ve uyarır** .
- **Çalışır ve ml ile ilgili sorunlar IÇIN ml uygulamalarını izleyin**. Eğitim ve çıkarım arasındaki model girişlerini karşılaştırın, modele özgü ölçümleri bulun ve ML altyapınızda izleme ve Uyarılar sağlayın.
- Modelleri sıklıkla güncelleştirmek, yeni modelleri test etmek ve diğer uygulama ve hizmetlerinizle birlikte yeni ML modellerini sürekli olarak almak için **Azure Machine Learning ve Azure DevOps ile uçtan uca ml yaşam döngüsünü otomatikleştirin** .

## <a name="create-reproducible-ml-pipelines"></a>Tekrarlanabilir ML işlem hatları oluşturma

Model eğitim sürecinizdeki tüm adımları birlikte birleştirmek için Azure Machine Learning adresinden ML işlem hatlarını kullanın.

Bir ML işlem hattı, veri hazırlığından, model değerlendirmesi için hiper parametre ayarlamayı ayıklama adımları içerebilir. Daha fazla bilgi için bkz. [ml işlem hatları](concept-ml-pipelines.md).

ML işlem hatlarınızı oluşturmak için [tasarımcıyı](concept-designer.md) kullanırsanız, tasarımcı sayfasının sağ üst köşesindeki **"..."** düğmesine tıklayın ve ardından **Kopyala**' yı seçin. İşlem hattınızı kopyalamak, eski sürümlerinizi kaybetmeden işlem hattı tasarımınızı yinelemenize olanak tanır.  

## <a name="register-package-and-deploy-models-from-anywhere"></a>Her yerden model kaydetme, paketleme ve dağıtma

### <a name="register-and-track-ml-models"></a>ML modellerini kaydetme ve izleme

Model kaydı, modellerinizi Azure bulutu 'nda çalışma alanınızda depolamanıza ve yapılandırmanıza olanak tanır. Model kayıt defteri, eğitilen modellerinizi düzenlemenizi ve izlemenizi kolaylaştırır.

> [!TIP]
> Kayıtlı bir model, modelinizi oluşturan bir veya daha fazla dosya için mantıksal bir kapsayıcıdır. Örneğin, birden çok dosyada depolanan bir modeliniz varsa, bunları Azure Machine Learning çalışma alanınızda tek bir model olarak kaydedebilirsiniz. Kayıttan sonra, kayıtlı modeli indirebilir veya dağıtabilir ve kayıtlı tüm dosyaları alabilirsiniz.

Kayıtlı modeller ad ve sürüm ile tanımlanır. Mevcut bir aynı ada sahip bir model her kaydettirdiğinizde, kayıt defteri sürüm artırır. Kayıt sırasında ek meta veri etiketleri sağlayabilirsiniz. Bu Etiketler daha sonra bir model ararken kullanılır. Azure Machine Learning, Python 3.5.2 veya üzeri kullanılarak yüklenebilecek tüm modelleri destekler.

> [!TIP]
> Azure Machine Learning dışında eğitilen modelleri de kaydedebilirsiniz.

Etkin bir dağıtımda kullanılmakta olan kayıtlı bir modeli silemezsiniz.
Daha fazla bilgi için, [modelleri dağıtma](how-to-deploy-and-where.md#registermodel)' nın model kaydetme bölümüne bakın.

### <a name="package-and-debug-models"></a>Paket ve hata ayıklama modelleri

Bir modeli üretime dağıtmadan önce, bir Docker görüntüsüne paketlenir. Çoğu durumda, dağıtım sırasında görüntü oluşturma arka planda otomatik olarak gerçekleşir. Görüntüyü el ile belirtebilirsiniz.

Dağıtım ile ilgili sorunlar yaşıyorsanız, sorun giderme ve hata ayıklama için yerel geliştirme ortamınızda dağıtım yapabilirsiniz.

Daha fazla bilgi için bkz. [modelleri dağıtma](how-to-deploy-and-where.md#registermodel) ve [dağıtım sorunlarını giderme](how-to-troubleshoot-deployment.md).

### <a name="validate-and-profile-models"></a>Model ve profil modellerini doğrulama

Azure Machine Learning, modelinizi dağıttığınızda kullanılacak ideal CPU ve bellek ayarlarını tespit etmek için profil oluşturmayı kullanabilir. Model doğrulama, profil oluşturma işlemi için sağladığınız verileri kullanarak bu işlemin bir parçası olarak gerçekleşir.

### <a name="convert-and-optimize-models"></a>Modelleri dönüştürme ve iyileştirme

Modelinizi [Open sinir Network Exchange](https://onnx.ai) (onnx) olarak dönüştürmek, performansı iyileştirebilir. ONNX 'e dönüştürmek, ortalama bir 2x performans artışı sağlayabilir.

Azure Machine Learning ile ONNX hakkında daha fazla bilgi için bkz. [ml modellerini oluşturma ve hızlandırma](concept-onnx.md) makalesi.

### <a name="use-models"></a>Modelleri kullanma

Eğitilen makine öğrenimi modelleri, bulutta veya yerel olarak Web Hizmetleri olarak dağıtılır. Modelleri, Azure IoT Edge cihazlara de dağıtabilirsiniz. Dağıtımlar, Inor sınırlaması için CPU, GPU veya alan programlanabilir geçit dizileri (FPGA) kullanır. Ayrıca, Power BI modelleri de kullanabilirsiniz.

Bir modeli Web hizmeti veya IoT Edge cihaz olarak kullanırken, aşağıdaki öğeleri sağlarsınız:

* Hizmete/cihaza gönderilen verileri almak için kullanılan model (ler).
* Bir giriş betiği. Bu betik istekleri kabul eder, verileri öğrenmek için modeli kullanır ve bir yanıt döndürür.
* Model (ler) ve giriş betiği için gereken bağımlılıkları açıklayan bir Conda ortam dosyası.
* Model ve giriş betiği için gereken metin, veri vb. gibi diğer varlıklar.

Ayrıca, hedef dağıtım platformunun yapılandırmasını da sağlarsınız. Örneğin, sanal makine aile türü, kullanılabilir bellek ve Azure Kubernetes hizmetine dağıtım yaparken çekirdek sayısı.

Görüntü oluşturulduğunda, Azure Machine Learning için gereken bileşenler de eklenir. Örneğin, Web hizmetini çalıştırmak ve IoT Edge etkileşimde bulunmak için gereken varlıklar.

#### <a name="batch-scoring"></a>Toplu İşlem puanlama
Batch Puanlama, ML işlem hatları aracılığıyla desteklenir. Daha fazla bilgi için bkz. [büyük veriler üzerinde Batch tahminleri](how-to-run-batch-predictions.md).

#### <a name="real-time-web-services"></a>Gerçek zamanlı Web Hizmetleri

Aşağıdaki işlem hedefleri ile **Web hizmetlerindeki** modellerinizi kullanabilirsiniz:

* Azure Container Örneği
* Azure Kubernetes Service
* Yerel geliştirme ortamı

Modeli bir Web hizmeti olarak dağıtmak için aşağıdaki öğeleri sağlamanız gerekir:

* Modellerin modeli veya modelini işleme.
* Modeli kullanmak için gereken bağımlılıklar. Örneğin, istekleri kabul eden ve modeli çağıran bir betik, Conda bağımlılıkları, vb.
* Modelin nasıl ve nereye dağıtılacağını açıklayan dağıtım yapılandırması.

Daha fazla bilgi için bkz. [modelleri dağıtma](how-to-deploy-and-where.md).

#### <a name="iot-edge-devices"></a>IoT Edge cihazlar

IoT cihazlarıyla modelleri **Azure IoT Edge modüller**aracılığıyla kullanabilirsiniz. IoT Edge modüller, cihazda çıkarım veya model Puanlama sağlayan bir donanım cihazına dağıtılır.

Daha fazla bilgi için bkz. [modelleri dağıtma](how-to-deploy-and-where.md).

### <a name="analytics"></a>Analiz

Microsoft Power BI, veri analizi için makine öğrenimi modellerinin kullanılmasını destekler. Daha fazla bilgi için bkz. [Power BI Azure Machine Learning Tümleştirme (Önizleme)](https://docs.microsoft.com/power-bi/service-machine-learning-integration).

## <a name="capture-the-governance-data-required-for-capturing-the-end-to-end-ml-lifecycle"></a>Uçtan uca ML yaşam döngüsünü yakalamak için gereken idare verilerini yakalayın

Azure ML, tüm ML varlıklarınızın uçtan uca denetim izlerini izleme olanağı sunar. Bu avantajlar şunlardır:

- Azure ML, kodunuzun hangi depoya/dala/yürütmeye geldiğini izlemek için [Git ile tümleşir](how-to-set-up-training-targets.md#gitintegration) .
- [Azure ML veri kümeleri](how-to-create-register-datasets.md) , verileri izlemenize, profillerinize ve sürümetmenize yardımcı olur. 
- Azure ML çalışma geçmişi, bir modeli eğitmek için kullanılan kodun, verilerin ve işlemin bir anlık görüntüsünü depolar.
- Azure ML modeli kayıt defteri, modelinizle ilişkili tüm meta verileri yakalar (Bu, dağıtımları, dağıtımı sağlıklı ise, dağıtıldığı yerde).

## <a name="notify-automate-and-alert-on-events-in-the-ml-lifecycle"></a>ML yaşam döngüsünde olayları bildirme, otomatikleştirme ve uyarma
Azure ML, ML yaşam döngüsünün olaylarını bilgilendirmek ve otomatikleştirmek için kullanılabilecek Azure EventGrid 'e anahtar olayları yayımlar. Daha fazla bilgi için lütfen [Bu belgeye](how-to-use-event-grid.md)bakın.


## <a name="monitor-for-operational--ml-issues"></a>İşlemsel & ML sorunlarını izleme

İzleme, modelinize hangi verilerin gönderildiğini ve döndürdüğü tahminleri anlamanıza olanak sağlar.

Bu bilgiler, modelinizin nasıl kullanıldığını anlamanıza yardımcı olur. Toplanan giriş verileri, modelin gelecek sürümlerini eğitmek için de yararlı olabilir.

Daha fazla bilgi için bkz. [model veri toplamayı etkinleştirme](how-to-enable-data-collection.md).

## <a name="automate-the-ml-lifecycle"></a>ML yaşam döngüsünü otomatikleştirin 

Bir modeli gösteren bir sürekli tümleştirme işlemi oluşturmak için GitHub ve Azure Pipelines kullanabilirsiniz. Tipik bir senaryoda, bir veri Bilimconu bir proje için git deposunda değişiklik denetlediğinde, Azure işlem hattı bir eğitim çalıştırması başlatır. Daha sonra, eğitilen modelin performans özelliklerini görmek için çalıştırmanın sonuçları incelenebilir. Ayrıca, modeli bir Web hizmeti olarak dağıtan bir işlem hattı da oluşturabilirsiniz.

[Azure Machine Learning uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml) , Azure Pipelines çalışmayı kolaylaştırır. Azure Pipelines için aşağıdaki geliştirmeleri sağlar:

* Hizmet bağlantısı tanımlarken çalışma alanı seçimini izin vermez.
* Eğitim ardışık düzeninde oluşturulan eğitilen modeller tarafından tetiklenecek yayın işlem hatlarını sağlar.

Azure Machine Learning ile Azure Pipelines kullanma hakkında daha fazla bilgi için, bkz. Azure Pipelines article ve [Azure Machine Learning MLOps](https://aka.ms/mlops) deposu [Ile ml modellerinin sürekli tümleştirilmesi ve dağıtımı](/azure/devops/pipelines/targets/azure-machine-learning) .

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki kaynakları okuyarak ve inceleyerek daha fazla bilgi edinin:

+ Azure Machine Learning ile [modellerin dağıtılacağı konum &](how-to-deploy-and-where.md)

+ [Öğretici: ACI 'de görüntü sınıflandırma modeli dağıtın](tutorial-deploy-models-with-aml.md).

+ [Uçtan uca MLOps örnekleri deposu](https://github.com/microsoft/MLOps)

+ [Azure Pipelines ile ML modellerinin CI/CD 'si](/azure/devops/pipelines/targets/azure-machine-learning)

+ [Dağıtılan bir modeli](how-to-consume-web-service.md) kullanan istemciler oluşturma

+ [Uygun ölçekte makine öğrenimi](/azure/architecture/data-guide/big-data/machine-learning-at-scale)

+ [Azure AI başvuru mimarileri & en iyi yöntemler temsilcisi](https://github.com/microsoft/AI)
