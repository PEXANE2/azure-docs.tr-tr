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
ms.date: 03/17/2020
ms.custom: seodec18
ms.openlocfilehash: d1d14fa9730e3ddd47378a45ff7a1442bdee69ac
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87543393"
---
# <a name="mlops-model-management-deployment-and-monitoring-with-azure-machine-learning"></a>MLOps: model yönetimi, dağıtım ve Azure Machine Learning ile izleme

Bu makalede, modellerinizin yaşam döngüsünü yönetmek için Azure Machine Learning kullanma hakkında bilgi edinin. Azure Machine Learning, Machine Learning Işlemler (MLOps) yaklaşımı kullanır. MLOps, Machine Learning çözümlerinizin kalitesini ve tutarlılığını geliştirir. 

## <a name="what-is-mlops"></a>MLOps nedir?

Machine Learning Işlemler (MLOps), iş akışlarının verimliliğini artıran [DevOps](https://azure.microsoft.com/overview/what-is-devops/) ilkelerine ve uygulamalarına dayanır. Örneğin, sürekli tümleştirme, teslim ve dağıtım. MLOps, bu ilkeleri makine öğrenimi sürecine uygular, amacı:

* Modellerin daha hızlı deneme ve geliştirmesi
* Modellerin üretime daha hızlı dağıtılması
* Kalite denetimi

Azure Machine Learning aşağıdaki MLOps yeteneklerini sağlar:

- **Tekrarlanabilir ml işlem hatları oluşturun**. Machine Learning işlem hatları, veri hazırlama, eğitim ve Puanlama işlemleriniz için yinelenebilir ve yeniden kullanılabilir adımlar tanımlamanızı sağlar.
- Eğitim ve dağıtım modelleri için yeniden **kullanılabilir yazılım ortamları oluşturun** .
- **Modelleri her yerden kaydedin, paketleyin ve dağıtın**. Ayrıca, modeli kullanmak için gereken ilişkili meta verileri izleyebilirsiniz.
- **Uçtan uca ml yaşam döngüsü için idare verilerini yakalayın**. Günlüğe kaydedilen bilgiler, hangi modellerin yayımladığına, neden değişikliklerin yapıldığını ve modellerin üretimde ne zaman dağıtıldığını veya ne zaman kullanıldığını içerebilir.
- **Ml yaşam döngüsünün olaylarını uyarın ve uyarır**. Örneğin, deneme tamamlama, model kaydı, model dağıtımı ve veri dralgılama.
- **Çalışır ve ml ile ilgili sorunlar IÇIN ml uygulamalarını izleyin**. Eğitim ve çıkarım arasındaki model girişlerini karşılaştırın, modele özgü ölçümleri bulun ve ML altyapınızda izleme ve Uyarılar sağlayın.
- **Azure Machine Learning ve Azure Pipelines ile uçtan uca ml yaşam döngüsünü otomatikleştirin**. İşlem hatlarını kullanmak, sıklıkla modelleri güncelleştirmenizi, yeni modelleri test etmenizi ve diğer uygulama ve hizmetlerinizle birlikte yeni ML modellerini sürekli olarak almayı sağlar.

## <a name="create-reproducible-ml-pipelines"></a>Tekrarlanabilir ML işlem hatları oluşturma

Model eğitim sürecinizdeki tüm adımları birlikte birleştirmek için Azure Machine Learning adresinden ML işlem hatlarını kullanın.

Bir ML işlem hattı, veri hazırlığından, model değerlendirmesi için hiper parametre ayarlamayı ayıklama adımları içerebilir. Daha fazla bilgi için bkz. [ml işlem hatları](concept-ml-pipelines.md).

ML işlem hatlarınızı oluşturmak için [tasarımcıyı](concept-designer.md) kullanırsanız, tasarımcı sayfasının sağ üst köşesindeki **"..."** düğmesine tıklayın ve ardından **Kopyala**' yı seçin. İşlem hattınızı kopyalamak, eski sürümlerinizi kaybetmeden işlem hattı tasarımınızı yinelemenize olanak tanır.  

## <a name="create-reusable-software-environments"></a>Yeniden kullanılabilir yazılım ortamları oluşturma

Azure Machine Learning ortamlar, geliştirdikleri şekilde projenizin yazılım bağımlılıklarını izlemenize ve yeniden üretmeye olanak tanır. Ortamlar, derlemelerin el ile yazılım yapılandırmaları olmadan tekrarlanabilir olmasını sağlar.

Ortamlar, projelerinize yönelik PIP ve Conda bağımlılıklarını betimliyor ve modellerin eğitimi ve dağıtımı için kullanılabilir. Daha fazla bilgi için bkz. [Azure Machine Learning ortamları nelerdir](concept-environments.md).

## <a name="register-package-and-deploy-models-from-anywhere"></a>Her yerden model kaydetme, paketleme ve dağıtma

### <a name="register-and-track-ml-models"></a>ML modellerini kaydetme ve izleme

Model kaydı, modellerinizi Azure bulutu 'nda çalışma alanınızda depolamanıza ve yapılandırmanıza olanak tanır. Model kayıt defteri, eğitilen modellerinizi düzenlemenizi ve izlemenizi kolaylaştırır.

> [!TIP]
> Kayıtlı bir model, modelinizi oluşturan bir veya daha fazla dosya için mantıksal bir kapsayıcıdır. Örneğin, birden çok dosyada depolanan bir modeliniz varsa, bunları Azure Machine Learning çalışma alanınızda tek bir model olarak kaydedebilirsiniz. Kayıttan sonra, kayıtlı modeli indirebilir veya dağıtabilir ve kayıtlı tüm dosyaları alabilirsiniz.

Kayıtlı modeller ad ve sürümle tanımlanır. Modeli var olan bir adla her kaydettiğinizde, kayıt defteri sürümü bir artırır. Kayıt sırasında ek meta veri etiketleri sağlayabilirsiniz. Bu Etiketler daha sonra bir model ararken kullanılır. Azure Machine Learning, Python 3.5.2 veya üzeri kullanılarak yüklenebilecek tüm modelleri destekler.

> [!TIP]
> Azure Machine Learning dışında eğitilen modelleri de kaydedebilirsiniz.

Etkin bir dağıtımda kullanılmakta olan kayıtlı bir modeli silemezsiniz.
Daha fazla bilgi için, [modelleri dağıtma](how-to-deploy-and-where.md#registermodel)' nın model kaydetme bölümüne bakın.

### <a name="profile-models"></a>Profil modelleri

Azure Machine Learning, modelinizi dağıtırken oluşturulacak hizmetin CPU ve bellek gereksinimlerini anlamanıza yardımcı olabilir. Profil oluşturma, modelinizi çalıştıran hizmeti sınar ve CPU kullanımı, bellek kullanımı ve yanıt gecikme süresi gibi bilgileri döndürür. Ayrıca, kaynak kullanımına bağlı olarak bir CPU ve bellek önerisi sağlar.
Daha fazla bilgi için bkz. [dağıtım modellerinin](how-to-deploy-profile-model.md)profil oluşturma bölümü.

### <a name="package-and-debug-models"></a>Paket ve hata ayıklama modelleri

Bir modeli üretime dağıtmadan önce, bir Docker görüntüsüne paketlenir. Çoğu durumda, dağıtım sırasında görüntü oluşturma arka planda otomatik olarak gerçekleşir. Görüntüyü el ile belirtebilirsiniz.

Dağıtım ile ilgili sorunlar yaşıyorsanız, sorun giderme ve hata ayıklama için yerel geliştirme ortamınızda dağıtım yapabilirsiniz.

Daha fazla bilgi için bkz. [modelleri dağıtma](how-to-deploy-and-where.md#registermodel) ve [dağıtım sorunlarını giderme](how-to-troubleshoot-deployment.md).

### <a name="convert-and-optimize-models"></a>Modelleri dönüştürme ve iyileştirme

Modelinizi [Open sinir Network Exchange](https://onnx.ai) (onnx) olarak dönüştürmek, performansı iyileştirebilir. ONNX 'e dönüştürmek, ortalama bir 2x performans artışı sağlayabilir.

Azure Machine Learning ile ONNX hakkında daha fazla bilgi için bkz. [ml modellerini oluşturma ve hızlandırma](concept-onnx.md) makalesi.

### <a name="use-models"></a>Modelleri kullanma

Eğitilen makine öğrenimi modelleri, bulutta veya yerel olarak Web Hizmetleri olarak dağıtılır. Modelleri, Azure IoT Edge cihazlara de dağıtabilirsiniz. Dağıtımlar, Inor sınırlaması için CPU, GPU veya alan programlanabilir geçit dizileri (FPGA) kullanır. Ayrıca, Power BI modelleri de kullanabilirsiniz.

Modeli bir web hizmeti veya IoT Edge cihazı olarak kullanırken şu öğeleri sağlarsınız:

* Hizmete/cihaza gönderilen verileri almak için kullanılan model (ler).
* Giriş betiği. Bu betik istekleri kabul eder, verileri öğrenmek için modeli kullanır ve bir yanıt döndürür.
* Model ve giriş betiği için gereken PIP ve Conda bağımlılıklarını açıklayan bir Azure Machine Learning ortamı.
* Model ve giriş betiği için gereken metin, veri vb. gibi diğer varlıklar.

Ayrıca hedef dağıtım platformunun yapılandırmasını da sağlarsınız. Örneğin, sanal makine aile türü, kullanılabilir bellek ve Azure Kubernetes hizmetine dağıtım yaparken çekirdek sayısı.

Görüntü oluşturulduğunda Azure Machine Learning’e gereken bileşenler de eklenir. Örneğin, web hizmetinin çalıştırılması ve IoT Edge ile etkileşim kurması için gereken varlıklar.

#### <a name="batch-scoring"></a>Batch puanlama
Batch Puanlama, ML işlem hatları aracılığıyla desteklenir. Daha fazla bilgi için bkz. [büyük veriler üzerinde Batch tahminleri](how-to-use-parallel-run-step.md).

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

#### <a name="controlled-rollout"></a>Denetlenen dağıtım

Azure Kubernetes hizmetine dağıtım yaparken, aşağıdaki senaryoları etkinleştirmek için denetimli bir dağıtım kullanabilirsiniz:

* Dağıtım için bir uç noktanın birden çok sürümünü oluşturma
* Trafiği uç noktanın farklı sürümlerine yönlendirerek bir/B testi gerçekleştirin.
* Uç nokta yapılandırmasındaki trafik yüzdesini güncelleştirerek uç nokta sürümleri arasında geçiş yapın.

Daha fazla bilgi için bkz. [ml modellerinin denetimli dağıtımı](how-to-deploy-azure-kubernetes-service.md#deploy-models-to-aks-using-controlled-rollout-preview).

#### <a name="iot-edge-devices"></a>IoT Edge cihazlar

IoT cihazlarıyla modelleri **Azure IoT Edge modüller**aracılığıyla kullanabilirsiniz. IoT Edge modüller, cihazda çıkarım veya model Puanlama sağlayan bir donanım cihazına dağıtılır.

Daha fazla bilgi için bkz. [modelleri dağıtma](how-to-deploy-and-where.md).

### <a name="analytics"></a>Analiz

Microsoft Power BI, veri analizi için makine öğrenimi modellerinin kullanılmasını destekler. Daha fazla bilgi için bkz. [Power BI Azure Machine Learning Tümleştirme (Önizleme)](https://docs.microsoft.com/power-bi/service-machine-learning-integration).

## <a name="capture-the-governance-data-required-for-capturing-the-end-to-end-ml-lifecycle"></a>Uçtan uca ML yaşam döngüsünü yakalamak için gereken idare verilerini yakalayın

Azure ML, meta verileri kullanarak tüm ML varlıklarınızın uçtan uca denetim izlerini izleme özelliği sunar.

- Azure ML, kodunuzun hangi depoya/dala/yürütmeye geldiğini izlemek için [Git ile tümleşir](how-to-set-up-training-targets.md#gitintegration) .
- [Azure ML veri kümeleri](how-to-create-register-datasets.md) , verileri izlemenize, profillerinize ve sürümetmenize yardımcı olur.
- [Yorumlandırma](how-to-machine-learning-interpretability.md) , modellerinizi açıklamanıza, yasal uyumluluğu karşılamanıza ve belirli bir giriş için modellerin bir sonuca nasıl ulaştığını anlamanıza olanak tanır.
- Azure ML çalışma geçmişi, bir modeli eğitebilmeniz için kullanılan kodun, verilerin ve hesaplanan bir anlık görüntü depolar.
- Azure ML modeli kayıt defteri, modelinizle ilişkili tüm meta verileri yakalar (Bu, dağıtımları, dağıtımı sağlıklı ise, dağıtıldığı yerde).
- [Azure Ile tümleştirme](how-to-use-event-grid.md) , ml yaşam döngüsünün olayları üzerinde işlem yapmanıza olanak sağlar. Örneğin, model kaydı, dağıtım, veri kayması ve eğitim (çalıştırma) olayları.

> [!TIP]
> Modeller ve veri kümeleri hakkında bazı bilgiler otomatik olarak yakalanırken, __etiketleri__kullanarak ek bilgi ekleyebilirsiniz. Çalışma alanınızda kayıtlı modelleri ve veri kümelerini ararken Etiketleri filtre olarak kullanabilirsiniz.
>
> Bir veri kümesini kayıtlı bir modelle ilişkilendirme isteğe bağlı bir adımdır. Bir modeli kaydederken bir veri kümesine başvurma hakkında bilgi için bkz. [model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model(class)?view=azure-ml-py) sınıfı başvurusu.


## <a name="notify-automate-and-alert-on-events-in-the-ml-lifecycle"></a>ML yaşam döngüsünde olayları bildirme, otomatikleştirme ve uyarma
Azure ML, ML yaşam döngüsünün olaylarını bilgilendirmek ve otomatikleştirmek için kullanılabilecek Azure EventGrid 'e anahtar olayları yayımlar. Daha fazla bilgi için lütfen [Bu belgeye](how-to-use-event-grid.md)bakın.


## <a name="monitor-for-operational--ml-issues"></a>İşlemsel & ML sorunlarını izleme

İzleme, modelinize hangi verilerin gönderildiğini ve döndürdüğü tahminleri anlamanıza olanak sağlar.

Bu bilgiler, modelinizin nasıl kullanıldığını anlamanıza yardımcı olur. Toplanan giriş verileri, modelin gelecek sürümlerini eğitmek için de yararlı olabilir.

Daha fazla bilgi için bkz. [model veri toplamayı etkinleştirme](how-to-enable-data-collection.md).

## <a name="retrain-your-model-on-new-data"></a>Yeni verilerde modelinize yeniden eğitme

Genellikle, yeni bilgi aldığınızda modelinizi doğrulamak, güncelleştirmek ve hatta sıfırdan yeniden eğmeniz gerekecektir. Bazen, yeni veri alma, etki alanının beklenen bir parçasıdır. Diğer zamanlarda, [veri kümelerinde verileri algılama (Önizleme)](how-to-monitor-datasets.md)bölümünde açıklandığı gibi, model performansı, belirli bir sensörde değişiklik gibi doğal veri değişiklikleri, mevsimsel efektler gibi doğal veri değişiklikleri veya diğer özelliklerle ilişkili olarak değişen özellikler gibi, bu tür şeyler üzerinde azalabilir. 

"Nasıl yaparım? yeniden eğitmem gerekip gerekmediğini bilmeniz" için evrensel yanıt yok mu? " Ancak daha önce tartışılan Azure ML olayı ve izleme araçları, otomasyon için iyi başlangıç noktalarıdır. Yeniden eğitmeye karar verdikten sonra şunları yapmalısınız: 

- Yinelenebilir, otomatikleştirilmiş bir işlem kullanarak verilerinizi ön işleme
- Yeni modelinizi eğitme
- Yeni modelinizin çıkışlarını eski modelinizle karşılaştırın
- Eski modelinizi değiştirip değiştiremeyeceğini seçmek için önceden tanımlı ölçütleri kullanın 

Yukarıdaki adımların bir teması, yeniden eğitimin geçici bir şekilde değil otomatik hale gelmelidir. [Azure Machine Learning işlem hatları](concept-ml-pipelines.md) , veri hazırlama, eğitim, doğrulama ve dağıtımla ilgili iş akışları oluşturmak için iyi bir yanıttır. İşlem hatlarının ve Azure Machine Learning tasarımcısının bir yeniden eğitim senaryosuna nasıl uyduğunu görmek için [Azure Machine Learning Designer ile modelleri yeniden eğitme (Önizleme)](how-to-retrain-designer.md) makalesini okuyun. 

## <a name="automate-the-ml-lifecycle"></a>ML yaşam döngüsünü otomatikleştirin 

Bir modeli gösteren bir sürekli tümleştirme işlemi oluşturmak için GitHub ve Azure Pipelines kullanabilirsiniz. Tipik bir senaryoda, bir veri Bilimconu bir proje için git deposunda değişiklik denetlediğinde, Azure işlem hattı bir eğitim çalıştırması başlatır. Daha sonra, eğitilen modelin performans özelliklerini görmek için çalıştırmanın sonuçları incelenebilir. Ayrıca, modeli bir Web hizmeti olarak dağıtan bir işlem hattı da oluşturabilirsiniz.

[Azure Machine Learning uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml) , Azure Pipelines çalışmayı kolaylaştırır. Azure Pipelines için aşağıdaki geliştirmeleri sağlar:

* Hizmet bağlantısı tanımlarken çalışma alanı seçimini izin vermez.
* Eğitim ardışık düzeninde oluşturulan eğitilen modeller tarafından tetiklenecek yayın işlem hatlarını sağlar.

Azure Machine Learning ile Azure Pipelines kullanma hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

* [Azure Pipelines ile ML modellerini sürekli tümleştirme ve dağıtma](/azure/devops/pipelines/targets/azure-machine-learning) 
* [Azure Machine Learning MLOps](https://aka.ms/mlops) deposu.
* [MLOpsPython repository Azure Machine Learning](https://github.com/Microsoft/MLOpspython) .

Ayrıca, eğitim ile kullanım için verileri hazırlayan bir veri alma işlem hattı oluşturmak için Azure Data Factory de kullanabilirsiniz. Daha fazla bilgi için bkz. veri alma işlem [hattı](how-to-cicd-data-ingestion.md).

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki kaynakları okuyarak ve inceleyerek daha fazla bilgi edinin:

+ Azure Machine Learning ile [modellerin dağıtılacağı konum &](how-to-deploy-and-where.md)

+ [Öğretici: ACI 'de görüntü sınıflandırma modeli dağıtın](tutorial-deploy-models-with-aml.md).

+ [Uçtan uca MLOps örnekleri deposu](https://github.com/microsoft/MLOps)

+ [Azure Pipelines ile ML modellerinin CI/CD 'si](/azure/devops/pipelines/targets/azure-machine-learning)

+ [Dağıtılan bir modeli](how-to-consume-web-service.md) kullanan istemciler oluşturma

+ [Uygun ölçekte makine öğrenimi](/azure/architecture/data-guide/big-data/machine-learning-at-scale)

+ [Azure AI başvuru mimarileri & en iyi yöntemler temsilcisi](https://github.com/microsoft/AI)
