---
title: 'MLOps: ML model yönetimi'
titleSuffix: Azure Machine Learning
description: 'Azure Machine Learning (MLOps) ile model yönetimi hakkında bilgi edinin. Sürekli olarak geliştirmek için modellerinizi dağıtın, yönetin ve izleyin. '
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: jpe316
ms.author: jordane
ms.date: 03/17/2020
ms.custom: seodec18
ms.openlocfilehash: 7857d11c625911cd1b49dfcf0e0d612fc6a3871e
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81314304"
---
# <a name="mlops-model-management-deployment-and-monitoring-with-azure-machine-learning"></a>MLOps: Azure Machine Learning ile model yönetimi, dağıtım ve izleme

Bu makalede, modellerinizin yaşam döngüsünü yönetmek için Azure Machine Learning'i nasıl kullanacağınız hakkında bilgi edinin. Azure Machine Learning, Machine Learning Operations (MLOps) yaklaşımını kullanır. MLOps, makine öğrenimi çözümlerinizin kalitesini ve tutarlılığını artırır. 

## <a name="what-is-mlops"></a>MLOps nedir?

Machine Learning Operations (MLOps), iş akışlarının verimliliğini artıran [DevOps](https://azure.microsoft.com/overview/what-is-devops/) ilke ve uygulamalarına dayanmaktadır. Örneğin, sürekli tümleştirme, teslim ve dağıtım. MLOps aşağıdaki amacı ile, makine öğrenme sürecine bu ilkeleri uygular:

* Modellerin daha hızlı deneme ve geliştirilmesi
* Modellerin üretime daha hızlı dağıtılması
* Kalite güvencesi

Azure Machine Learning aşağıdaki MLOps özelliklerini sağlar:

- **Tekrarlanabilir ML boru hatları oluşturun.** Machine Learning ardışık hatları, veri hazırlama, eğitim ve puanlama süreçleriniz için tekrarlanabilir ve yeniden kullanılabilir adımlar tanımlamanıza olanak sağlar.
- Modelleri eğitmek ve dağıtmak için **yeniden kullanılabilir yazılım ortamları oluşturun.**
- **Modelleri her yerden kaydedin, paketlendirin ve dağıtın.** Modeli kullanmak için gereken ilişkili meta verileri de izleyebilirsiniz.
- **Uçten uca ML yaşam döngüsü için yönetim verilerini yakalayın.** Günlüğe kaydedilen bilgiler, modelleri kimin yayımlandığını, değişikliklerin neden yapıldığını ve modellerin ne zaman dağıtıldığını veya üretimde kullanıldığını içerebilir.
- **ML yaşam döngüsündeki olayları bildirin ve uyarın.** Örneğin, deneme tamamlama, model kaydı, model dağıtımı ve veri kayması algılama.
- **Operasyonel ve ML ile ilgili sorunlar için ML uygulamalarını izleyin.** Eğitim ve çıkarım arasındaki model girişlerini karşılaştırın, modele özel ölçümleri keşfedin ve ML altyapınızda izleme ve uyarılar sağlayın.
- **Azure Machine Learning ve Azure Pipelines ile uçlardan uca ML yaşam döngüsünü otomatikleştirin.** Boru hatları nı kullanmak, modelleri sık sık güncelleştirmenize, yeni modelleri test etmenize ve diğer uygulama ve hizmetlerinizin yanı sıra sürekli olarak yeni ML modellerini piyasaya çıkarmanıza olanak tanır.

## <a name="create-reproducible-ml-pipelines"></a>Tekrarlanabilir ML boru hatları oluşturma

Azure Machine Learning'in ML boru hatlarını kullanarak model eğitim sürecinizde yer alan tüm adımları bir araya getirin.

ML ardışık bir yapı, veri hazırlamadan özellik çıkarmadan hiperparametre atonuna ve model değerlendirmesine kadar adımlar içerebilir. Daha fazla bilgi için [ML boru hatlarına](concept-ml-pipelines.md)bakın.

Ml ardışık hatlarınızı oluşturmak için [Tasarımcı'yı](concept-designer.md) kullanırsanız, istediğiniz zaman Designer sayfasının sağ üst kısmındaki **"..."** seçeneğini tıklayabilir ve ardından **Klon'u**seçebilirsiniz. Boru hattınızı klonlamak, eski sürümlerinizi kaybetmeden boru hattı tasarımınızı yinelemenize olanak tanır.  

## <a name="create-reusable-software-environments"></a>Yeniden kullanılabilir yazılım ortamları oluşturma

Azure Machine Learning ortamları, projelerinizin yazılım bağımlılıklarını geliştikçe izlemenize ve yeniden oluşturmanıza olanak tanır. Ortamlar, el ile yazılım yapılandırmaları olmadan yapıların tekrarlanabilir olmasını sağlamanıza olanak tanır.

Ortamlar projeleriniz için pip ve Conda bağımlılıklarını açıklar ve modellerin hem eğitimi hem de dağıtımı için kullanılabilir. Daha fazla bilgi için Azure [Machine Learning ortamları nelerdir'e](concept-environments.md)bakın.

## <a name="register-package-and-deploy-models-from-anywhere"></a>Modelleri her yerden kaydedin, paketle ve dağıtın

### <a name="register-and-track-ml-models"></a>ML modellerini kaydedin ve izleyin

Model kaydı, modellerinizi Azure bulutu içinde, çalışma alanınızda depolamanıza ve sürüm olarak saklamanıza olanak tanır. Model kayıt defteri, eğitimli modellerinizi düzenlemeyi ve izlemenizi kolaylaştırır.

> [!TIP]
> Kayıtlı model, modelinizi oluşturan bir veya daha fazla dosya için mantıksal bir kapsayıcıdır. Örneğin, birden çok dosyada depolanan bir modeliniz varsa, bunları Azure Machine Learning çalışma alanınızda tek bir model olarak kaydedebilirsiniz. Kayıt olduktan sonra, kayıtlı modeli karşıdan yükleyebilir veya dağıtabilir ve kayıtlı tüm dosyaları alabilirsiniz.

Kayıtlı modeller ad ve sürümle tanımlanır. Modeli var olan bir adla her kaydettiğinizde, kayıt defteri sürümü bir artırır. Kayıt sırasında ek meta veri etiketleri sağlanabilir. Bu etiketler daha sonra bir model ararken kullanılır. Azure Machine Learning, Python 3.5.2 veya üzeri kullanılarak yüklenebilecek tüm modelleri destekler.

> [!TIP]
> Azure Machine Learning dışında eğitilmiş modelleri de kaydedebilirsiniz.

Etkin bir dağıtımda kullanılan kayıtlı bir modeli silemezsiniz.
Daha fazla bilgi [için, Dağıt modelleri](how-to-deploy-and-where.md#registermodel)kayıt modeli bölümüne bakın.

### <a name="profile-models"></a>Profil modelleri

Azure Machine Learning, modelinizi dağıttığınızda oluşturulacak hizmetin CPU ve bellek gereksinimlerini anlamanıza yardımcı olabilir. Profil oluşturma, modelinizi çalıştıran hizmeti sınar ve CPU kullanımı, bellek kullanımı ve yanıt gecikmesi gibi bilgileri döndürür. Ayrıca, kaynak kullanımına dayalı bir CPU ve bellek önerisi de sağlar.
Daha fazla bilgi [için, Dağıt modelleri](how-to-deploy-and-where.md#profilemodel)profil bölümüne bakın.

### <a name="package-and-debug-models"></a>Paket ve hata ayıklama modelleri

Bir modeli üretime dağıtmadan önce Docker görüntüsüne paketlenir. Çoğu durumda, görüntü oluşturma dağıtım sırasında arka planda otomatik olarak gerçekleşir. Görüntüyü el ile belirtebilirsiniz.

Dağıtımla ilgili sorunlarla karşılaştıysanız, sorun giderme ve hata ayıklama için yerel geliştirme ortamınızda dağıtabilirsiniz.

Daha fazla bilgi için [Troubleshooting deployments](how-to-troubleshoot-deployment.md) [bkz.](how-to-deploy-and-where.md#registermodel)

### <a name="convert-and-optimize-models"></a>Modelleri dönüştürme ve optimize edin

Modelinizi Açık [Nöral Ağ Değişimi'ne](https://onnx.ai) (ONNX) dönüştürmek performansı artırabilir. Ortalama olarak, ONNX'e dönüştürme 2 kat performans artışı sağlayabilir.

Azure Machine Learning ile ONNX hakkında daha fazla bilgi için [ML modelleri oluştur ve hızlandırma](concept-onnx.md) makalesine bakın.

### <a name="use-models"></a>Modelleri kullanma

Eğitimli makine öğrenimi modelleri bulutta veya yerel olarak web hizmetleri olarak dağıtılır. Modelleri Azure IoT Edge aygıtlarına da dağıtabilirsiniz. Dağıtımlar, çıkarım için CPU, GPU veya alan tarafından programlanabilir geçit dizileri (FPGA) kullanır. Power BI modellerini de kullanabilirsiniz.

Bir modeli web hizmeti veya IoT Edge aygıtı olarak kullanırken aşağıdaki öğeleri sağlarsınız:

* Hizmete/cihaza gönderilen verileri puanlamak için kullanılan model(ler).
* Giriş betiği. Bu komut dosyası istekleri kabul eder, verileri puanlamak için modeli kullanır ve bir yanıt döndürer.
* Model(ler) ve giriş komut dosyasının gerektirdiği pip ve Conda bağımlılıklarını açıklayan bir Azure Machine Learning ortamı.
* Metin, veri, vb. model(ler) ve giriş komut dosyası tarafından gerekli olan ek varlıklar.

Ayrıca hedef dağıtım platformunun yapılandırmasını da sağlarsınız. Örneğin, Azure Kubernetes Hizmeti'ne dağıtılırken VM aile türü, kullanılabilir bellek ve çekirdek sayısı.

Görüntü oluşturulduğunda, Azure Machine Learning tarafından gerekli olan bileşenler de eklenir. Örneğin, web hizmetini çalıştırmak ve IoT Edge ile etkileşimde kalmak için gereken varlıklar.

#### <a name="batch-scoring"></a>Batch puanlama
Toplu puanlama ML boru hatları ile desteklenir. Daha fazla bilgi [için, büyük verilerle ilgili Toplu tahminlere](how-to-use-parallel-run-step.md)bakın.

#### <a name="real-time-web-services"></a>Gerçek zamanlı web hizmetleri

Web **hizmetlerinde** modellerinizi aşağıdaki hesaplama hedefleri yle kullanabilirsiniz:

* Azure Container Örneği
* Azure Kubernetes Service
* Yerel kalkınma ortamı

Modeli bir web hizmeti olarak dağıtmak için aşağıdaki öğeleri sağlamanız gerekir:

* Model veya model topluluğu.
* Modeli kullanmak için gereken bağımlılıklar. Örneğin, istekleri kabul eden ve modeli, conda bağımlılıklarını vb. çağıran bir komut dosyası.
* Modelin nasıl ve nerede dağıtılanacağını açıklayan dağıtım yapılandırması.

Daha fazla bilgi için [bkz.](how-to-deploy-and-where.md)

#### <a name="controlled-rollout"></a>Kontrollü kullanıma

Azure Kubernetes Hizmetine dağıtılırken, aşağıdaki senaryoları etkinleştirmek için denetimli kullanıma hazır layabilirsiniz:

* Dağıtım için bitiş noktasının birden çok sürümü oluşturma
* Trafiği bitiş noktasının farklı sürümlerine yönlendirerek A/B testi gerçekleştirin.
* Bitiş noktası yapılandırmasında trafik yüzdesini güncelleştirerek bitiş noktası sürümleri arasında geçiş yapabilirsiniz.

Daha fazla bilgi için [ML modellerinin kontrollü ürün lansmanına](how-to-deploy-azure-kubernetes-service.md#deploy-models-to-aks-using-controlled-rollout-preview)bakın.

#### <a name="iot-edge-devices"></a>IoT Edge cihazları

**Azure IoT Edge modülleri**aracılığıyla IoT aygıtlı modelleri kullanabilirsiniz. IoT Edge modülleri, aygıtta çıkarım veya model puanlaması sağlayan bir donanım aygıtına dağıtılır.

Daha fazla bilgi için [bkz.](how-to-deploy-and-where.md)

### <a name="analytics"></a>Analiz

Microsoft Power BI, veri analitiği için makine öğrenimi modellerini kullanmayı destekler. Daha fazla bilgi için [Power BI (önizleme) 'deki Azure Machine Learning tümleştirmesi 'ne](https://docs.microsoft.com/power-bi/service-machine-learning-integration)bakın.

## <a name="capture-the-governance-data-required-for-capturing-the-end-to-end-ml-lifecycle"></a>Uçten uca ML yaşam döngüsünü yakalamak için gereken yönetim verilerini yakalama

Azure ML, meta verileri kullanarak tüm ML varlıklarınızın uçtan uca denetim izini izleme olanağı sağlar.

- Azure ML, kodunuzu hangi depo / şube / commit gelen bilgileri izlemek için [Git ile tümleştirir.](how-to-set-up-training-targets.md#gitintegration)
- [Azure ML Datasets,](how-to-create-register-datasets.md) verileri izlemenize, profilini ve sürüm verilerini izlemenize yardımcı olur.
- [Yorumlanabilirlik,](how-to-machine-learning-interpretability.md) modellerinizi açıklamanızı, mevzuata uygunluğu karşılamanızı ve modellerin belirli bir girdi için bir sonuca nasıl vardığını anlamanızı sağlar.
- Azure ML Run geçmişi, bir modeli eğitmek için kullanılan kodun, verilerin ve hesaplamaların anlık görüntüsünü depolar.
- Azure ML Model Kayıt Defteri, modelinizle ilişkili tüm meta verileri yakalar (hangi denemeyle onu eğittir, dağıtımları sağlıklıysa nerede dağıtılıyorsa onu eğittir).
- [Azure Olay Grid ile tümleştirme,](concept-event-grid-integration.md) ML yaşam döngüsündeki olaylara göre hareket etmenizi sağlar. Örneğin, model kaydı, dağıtım, veri kayması ve eğitim (çalıştır) olayları.

> [!TIP]
> Modeller ve veri kümeleri hakkındaki bazı bilgiler otomatik olarak yakalanırken, __etiketleri__kullanarak ek bilgiler ekleyebilirsiniz. Çalışma alanınızda kayıtlı modelleri ve veri kümelerini ararken, etiketleri filtre olarak kullanabilirsiniz.
>
> Bir veri kümesini kayıtlı bir modelle ilişkilendirme isteğe bağlı bir adımdır. Bir modeli kaydederken bir veri kümesine başvurma hakkında bilgi için [Model](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model(class)?view=azure-ml-py) sınıfı başvurusuna bakın.


## <a name="notify-automate-and-alert-on-events-in-the-ml-lifecycle"></a>ML yaşam döngüsündeki olayları bildirin, otomatikleştirin ve uyarı verin
Azure ML, ML yaşam döngüsündeki olayları bildirmek ve otomatikleştirmek için kullanılabilecek önemli olayları Azure EventGrid'e yayımlar. Daha fazla bilgi için lütfen [bu belgeye](how-to-use-event-grid.md)bakın.


## <a name="monitor-for-operational--ml-issues"></a>Operasyonel & ML sorunları için monitör

İzleme, modelinize hangi verilerin gönderildiğini ve döndürdünüzü anlamanızı sağlar.

Bu bilgiler, modelinizin nasıl kullanıldığını anlamanıza yardımcı olur. Toplanan giriş verileri, modelin gelecekteki sürümlerini eğitimde de yararlı olabilir.

Daha fazla bilgi için [model veri toplamayı nasıl etkinleştirin.](how-to-enable-data-collection.md)

## <a name="retrain-your-model-on-new-data"></a>Modelinizi yeni veriler üzerinde yeniden eğitin

Genellikle, yeni bilgiler aldığınızda modelinizi doğrulamak, güncellemek veya hatta sıfırdan yeniden eğitmek isteyebilirsiniz. Bazen, yeni veri almak etki alanının beklenen bir parçasıdır. Diğer zamanlarda, veri [kümelerinde veri kaydırma (önizleme)](how-to-monitor-datasets.md)algılatırken, model performansı belirli bir sensörde yapılan değişiklikler, mevsimsel etkiler gibi doğal veri değişiklikleri veya diğer özelliklerle olan ilişkileri değişen özellikler gibi şeyler karşısında bozulabilir. 

"Yeniden eğitilip eğitileyim gerektiğini nasıl bileceğim?" diye evrensel bir cevap yok. ancak Azure ML etkinlik ve izleme araçları daha önce tartışılan otomasyon için iyi bir başlangıç noktalarıdır. Yeniden eğitmeye karar verdikten sonra şunları yapmalısın: 

- Tekrarlanabilir, otomatikleştirilmiş bir işlem kullanarak verilerinizi önceden işleme
- Yeni modelinizi eğitin
- Yeni modelinizin çıktılarını eski modelinizle karşılaştırın
- Eski modelinizi değiştirip değiştirmeyeceğinizi seçmek için önceden tanımlanmış ölçütleri kullanın 

Yukarıdaki adımların bir teması, yeniden eğitim otomatik değil, geçici olması gerektiğidir. [Azure Machine Learning ardışık hatları,](concept-ml-pipelines.md) veri hazırlama, eğitim, doğrulama ve dağıtımla ilgili iş akışları oluşturmak için iyi bir yanıttır. Boru hatlarının ve Azure Machine Learning tasarımcısının yeniden eğitim senaryosuna nasıl uyduğunu görmek için Azure Machine Learning tasarımcısıyla yeniden [eğitme modellerini okuyun (önizleme).](how-to-retrain-designer.md) 

## <a name="automate-the-ml-lifecycle"></a>ML yaşam döngüsünü otomatikleştirin 

Bir modeli eğiten sürekli bir tümleştirme işlemi oluşturmak için GitHub ve Azure Ardışık Hatları'nı kullanabilirsiniz. Tipik bir senaryoda, bir Veri Bilim cisi projesi için Git repo'sundaki değişikliği denetlediğinde, Azure Ardışık Ardışık Bir eğitim çalışması başlatır. Çalıştırmanın sonuçları daha sonra eğitilmiş modelin performans özelliklerini görmek için incelenebilir. Ayrıca, modeli bir web hizmeti olarak dağıtan bir ardışık hatlar hattı da oluşturabilirsiniz.

[Azure Machine Learning uzantısı,](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml) Azure Ardışık Hatları ile çalışmayı kolaylaştırır. Azure Ardışık Hatları için aşağıdaki geliştirmeleri sağlar:

* Bir hizmet bağlantısı tanımlanırken çalışma alanı seçimini etkinleştirirken.
* Sürüm ardışık hatlarının bir eğitim boru hattında oluşturulan eğitimli modeller tarafından tetiklemesini sağlar.

Azure Makine Öğrenimi ile Azure Ardışık Hatlar'ı kullanma hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

* [ML modellerinin Azure Boru Hatları ile sürekli entegrasyonu ve dağıtımı](/azure/devops/pipelines/targets/azure-machine-learning) 
* [Azure Machine Learning MLOps](https://aka.ms/mlops) deposu.
* [Azure Machine Learning MLOpsPython](https://github.com/Microsoft/MLOpspython) deposu.

Verileri eğitimle kullanıma hazırlayan bir veri alma ardışık hattı oluşturmak için Azure Veri Fabrikası'nı da kullanabilirsiniz. Daha fazla bilgi için [bkz.](how-to-cicd-data-ingestion.md)

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki kaynakları okuyarak ve keşfederek daha fazla bilgi edinin:

+ Azure Machine Learning ile [modelleri nereye dağıtacağı &?](how-to-deploy-and-where.md)

+ [Öğretici: ACI bir görüntü sınıflandırma modeli dağıtın.](tutorial-deploy-models-with-aml.md)

+ [Uçuça MLOps örnekleri repo](https://github.com/microsoft/MLOps)

+ [Azure Boru Hatları ile ML modellerinIN CI/CD'si](/azure/devops/pipelines/targets/azure-machine-learning)

+ [Dağıtılmış bir modeli tüketen istemciler](how-to-consume-web-service.md) oluşturma

+ [Uygun ölçekte makine öğrenimi](/azure/architecture/data-guide/big-data/machine-learning-at-scale)

+ [Azure AI başvuru mimarileri & en iyi uygulamalar temsilcisi](https://github.com/microsoft/AI)
