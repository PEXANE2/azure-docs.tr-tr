---
title: ML Boru Hatları Nedir
titleSuffix: Azure Machine Learning
description: Bu makalede, Python için Azure Machine Learning SDK ile oluşturabileceğiniz makine öğrenimi (ML) boru hatlarının avantajlarını öğrenin. Makine öğrenimi boru hatları, veri bilimciler tarafından makine öğrenimi iş akışlarını oluşturmak, optimize etmek ve yönetmek için kullanılır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: laobri
author: lobrien
ms.date: 04/01/2020
ms.openlocfilehash: 0cefa78b6f52cc67df8817f68a9b793ab86b2a7f
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878587"
---
# <a name="what-are-azure-machine-learning-pipelines"></a>Azure Machine Learning boru hatları nedir?

Azure Machine Learning ardışık hatları, makine öğrenimi projelerinizde iş akışları oluşturmanıza olanak tanır. Bu iş akışlarının bir dizi avantajı vardır: 

+ Sade -lik
+ Hız
+ Tekrarlanabilirlik
+ Esneklik
+ Sürüm ve izleme
+ Modüler 
+ Kalite güvencesi
+ Maliyet kontrolü

Bu avantajlar, makine öğrenimi projeniz saf keşif ve yinelemenin ötesine geçer geçmez önem edir. Hatta basit tek adımlı boru hatları değerli olabilir. Makine öğrenimi projeleri genellikle karmaşık bir durumdadır ve tek bir iş akışının kesin başarısını önemsiz bir süreç haline getirmek rahatlatıcı olabilir.

[İlk ardışık hattınızı](how-to-create-your-first-pipeline.md)nasıl oluşturabilirsiniz öğrenin.

![Azure Machine Learning'de makine öğrenimi boru hatları](./media/concept-ml-pipelines/pipeline-flow.png)

<a name="compare"></a>
### <a name="which-azure-pipeline-technology-should-i-use"></a>Hangi Azure boru hattı teknolojisini kullanmalıyım?

Azure bulutu, her biri farklı bir amaca sahip birkaç başka ardışık hat lar sağlar. Aşağıdaki tabloda farklı ardışık lıklar ve ne için kullanıldığı listelenir:

| Senaryo | Birincil persona | Azure teklifi | OSS teklifi | Kanonik boru | Güçlü | 
| -------- | --------------- | -------------- | ------------ | -------------- | --------- | 
| Model orkestrasyonu (Makine öğrenimi) | Veri bilimcisi | Azure Makine Öğrenme Boru Hatları | Kubeflow Boru Hatları | Veri -> Modeli | Dağıtım, önbelleğe alma, kod-ilk, yeniden | 
| Veri düzenleme (Veri hazırlama) | Veri mühendisi | [Azure Data Factory işlem hatları](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) | Apache Hava Akımı | Veri -> Verileri | Güçlü bir şekilde yazılan hareket. Veri merkezli etkinlikler. |
| Kod & uygulama düzenleme (CI/CD) | Uygulama Geliştirici / Ops | [Azure DevOps Boru Hatları](https://azure.microsoft.com/services/devops/pipelines/) | Jenkins | Kod + Model -> Uygulama/Servis | En açık ve esnek etkinlik desteği, onay kuyrukları, gating ile aşamaları | 


## <a name="what-can-azure-ml-pipelines-do"></a>Azure ML boru hatları ne yapabilir?

Azure Machine Learning ardışık alanı, tam bir makine öğrenimi görevinin bağımsız olarak yürütülebilir bir iş akışıdır. Alt görevler işlem hattı içinde bir dizi adım olarak kapsüllenir. Azure Machine Learning ardışık bir ardışık nokta Python komut dosyası çağıran bir boru hattı kadar basit olabilir, _bu_ nedenle hemen hemen her şeyi yapabilir. Boru hatları gibi makine öğrenimi görevlerine _odaklanmalıdır:_

+ İçeri aktarma, doğrulama ve temizleme, ham verileri kullanışlı hale getirme ve dönüştürme, normalleştirme ve hazırlamayı içeren veri hazırlığı
+ Bağımsız değişkenleri, dosya yollarını ve günlük / raporlama yapılandırmalarını parametreleştirmeyi içeren eğitim yapılandırması
+ Verimli ve sürekli eğitim ve doğrulama. Verimlilik, belirli veri alt kümeleri, farklı donanım bilgi işlem kaynakları, dağıtılmış işleme ve ilerleme izleme belirtilmesinden kaynaklanabilir
+ Sürüm oluşturma, ölçeklendirme, sağlama ve erişim denetimini içeren dağıtım 

Bağımsız adımlar, birden fazla veri bilim adamının aynı boru hattı üzerinde aynı anda, bilgi işlem kaynaklarını aşırı vergilendirmeden çalışmasına olanak sağlar. Ayrı adımlar, her adım için farklı işlem türlerinin/boyutlarının kullanılmasını da kolaylaştırır.

Boru hattı tasarlandıktan sonra, genellikle boru hattının eğitim döngüsü etrafında daha ince ayar vardır. Bir ardışık hattı yeniden çalıştırdığınızda, çalıştır, güncelleştirilmiş bir eğitim komut dosyası gibi yeniden çalıştırılması gereken adımlara atlar. Yeniden çalıştırılması gerekmeyen adımlar atlanır. Aynı çözümleme, adımın başarıya uyması için kullanılan değişmemiş komut dosyaları için de geçerlidir. Bu yeniden kullanım işlevi, temel veriler değişmemişse veri alımı ve dönüştürme gibi maliyetli ve zaman yoğun adımlardan kaçınmaya yardımcı olur.

Azure Machine Learning ile, boru hattınızdaki her adım için PyTorch veya TensorFlow gibi çeşitli araç kitlerini ve çerçeveleri kullanabilirsiniz. Azure, ara verilerinizin akış aşağı bilgi işlem hedefleriyle paylaşılabilmeleri için kullandığınız çeşitli [bilgi işlem hedeflerini](concept-azure-machine-learning-architecture.md) koordine eder.

Boru [hattı denemelerinizin ölçümlerini](https://docs.microsoft.com/azure/machine-learning/how-to-track-experiments) doğrudan Azure portalında veya [çalışma alanı açılış sayfanızda (önizleme)](https://ml.azure.com)izleyebilirsiniz. Bir ardışık hatlar yayımlandıktan sonra, ardışık hattı herhangi bir platformdan veya yığından yeniden çalıştırmanızı sağlayan bir REST bitiş noktası yapılandırabilirsiniz.

Kısacası, makine öğrenimi yaşam döngüsünün tüm karmaşık görevleri boru hatları ile yardımcı olabilir. Diğer Azure boru hattı teknolojilerinin kendi güçlü yanları vardır. [Azure Veri Fabrikası ardışık hatları](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) verilerle çalışmada öne çıkmaktadır ve [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) sürekli tümleştirme ve dağıtım için doğru araçtır. Ancak odak noktanız makine öğrenimiyse, Azure Machine Learning boru hatları iş akışı gereksinimleriniz için en iyi seçim olabilir. 

## <a name="what-are-azure-ml-pipelines"></a>Azure ML boru hatları nedir?

Azure ML ardışık bir ardışık düzen, sıralı bir adım dizisiyle tam bir mantıksal iş akışı gerçekleştirir. Her adım ayrı bir işleme eylemidir. Ardışık hatlar, Azure Machine Learning [Experiment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py)bağlamında çalışır.

BIR ML projesinin ilk aşamalarında, Azure çalışma alanı ve kaynak yapılandırması, veri hazırlama, yapılandırma çalıştırma, eğitim ve doğrulama nın tüm işlerini yapan tek bir Jupyter dizüstü bilgisayarına veya Python komut dosyasına sahip olmak iyidir. Ancak işlevler ve sınıflar tek bir zorunlu kod bloğuna hızla tercih edilir hale geldikçe, ML iş akışları da hızla yekpare bir not defterine veya komut dosyasına tercih edilir hale gelir. 

ML görevlerini modüle ederek, boru hatları bilgisayar bilimi zorunluluğunu destekler, bir bileşenin "tek bir şeyi iyi yapması" gerektiğini yapar. Modülerlik açıkça takım halinde programlama başarı proje için hayati önem taşımaktadır, ama tek başına çalışırken bile, hatta küçük bir ML proje ayrı görevler içerir, karmaşıklık iyi bir miktar her. Görevler şunlardır: çalışma alanı yapılandırması ve veri erişimi, veri hazırlama, model tanımı ve yapılandırma ve dağıtım. Bir veya daha fazla görevin çıktıları girdileri diğerine oluştururken, herhangi bir görevin tam uygulama ayrıntıları, en iyi ihtimalle, sonraki alakasız dikkat dağıtıcı şeylerdir. En kötü ihtimalle, bir görevin hesaplama durumu başka bir hataya neden olabilir. 

### <a name="analyzing-dependencies"></a>Bağımlılıkları analiz etme

Birçok programlama ekosisteminde kaynak, kitaplık veya derleme bağımlılıklarını düzenleyen araçlar bulunur. Genellikle, bu araçlar bağımlılıkları hesaplamak için dosya zaman damgalarını kullanır. Bir dosya değiştirildiğinde, yalnızca o ve bağımlıları güncelleştirilir (karşıdan yüklenir, yeniden derlenir veya paketlenir). Azure ML boru hatları bu kavramı önemli ölçüde genişletir. Geleneksel yapı araçları gibi, ardışık hatlar da adımlar arasındaki bağımlılıkları hesaplar ve yalnızca gerekli yeniden hesaplamaları gerçekleştirir. 

Azure ML ardışık hatlarındaki bağımlılık çözümlemesi basit zaman damgalarından daha karmaşıktır. Her adım farklı bir donanım ve yazılım ortamında çalışabilir. Veri hazırlama zaman alan bir işlem olabilir, ancak güçlü GPU'lara sahip donanımda çalışması gerekmez, belirli adımlar işletim sistemi özel yazılım gerektirebilir, dağıtılmış eğitim kullanmak isteyebilirsiniz, vb. Kaynakları optimize etmek için maliyet tasarrufu önemli olsa da, donanım ve yazılım kaynaklarındaki tüm farklı varyasyonları el ile yapmak çok zor olabilir. Adımlar arasında aktardığınız verilerde hata yapmadan tüm bunları yapmak daha da zordur. 

Boru hatları bu sorunu çözer. Azure Machine Learning, boru hattı adımları arasındaki tüm bağımlılıkları otomatik olarak yönetir. Bu orkestrasyon, Docker görüntülerini yukarı ve aşağı döndürmeyi, bilgi işlem kaynaklarını eklemeyi ve ayırmayı ve verileri adımlar arasında tutarlı ve otomatik bir şekilde taşımayı içerebilir.

### <a name="reusing-results"></a>Sonuçları yeniden kullanma

Ayrıca, bir adımın çıktısı, isterseniz, yeniden kullanılabilir. Yeniden kullanımı bir olasılık olarak belirtirseniz ve yeniden hesaplamayı tetikleyen yukarı akım bağımlılıkları yoksa, ardışık düzen hizmeti adımın sonuçlarının önbelleğe alınmış bir sürümünü kullanır. Bu tür yeniden kullanım önemli ölçüde geliştirme süresini azaltabilir. Karmaşık bir veri hazırlama göreviniz varsa, büyük olasılıkla yalnızca gerekli olandan daha sık yeniden çalıştırAbilirsiniz. Boru hatları sizi bu endişeden kurtarıyor: gerekirse, adım çalışmayacak, değilse, çalışmayacaktır.

Tüm bu bağımlılık analizi, orkestrasyon ve etkinleştirme, bir [Pipeline](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline(class)?view=azure-ml-py) nesnesini anında aldığınızda, `Experiment`bir `submit()`'ye aktardığınızda ve . 

### <a name="coordinating-the-steps-involved"></a>İlgili adımların koordinasyonu

Bir `Pipeline` nesne oluşturup çalıştırdığınızda, aşağıdaki üst düzey adımlar oluşur:

+ Her adım için hizmet aşağıdaki gereksinimleri hesaplar:
    + Donanım hesaplama kaynakları
    + İşletim sistemi kaynakları (Docker image(s))
    + Yazılım kaynakları (Conda / virtualenv bağımlılıkları)
    + Veri girişleri 
+ Hizmet, adımlar arasındaki bağımlılıkları belirler ve dinamik bir yürütme grafiği oluşturur
+ Yürütme grafiğindeki her düğüm çalıştığında:
    + Hizmet gerekli donanım ve yazılım ortamını yapılandırır (belki de varolan kaynakları yeniden kullanır)
    + Adım çalışır, içeren `Experiment` nesneye günlük ve izleme bilgileri sağlar
    + Adım tamamlandığında, çıktıları bir sonraki adıma giriş olarak hazırlanır ve/veya depolamaalanına yazılır
    + Artık ihtiyaç duyulmaan kaynaklar tamamlanır ve ayrılır

![Boru hattı adımları](./media/concept-ml-pipelines/run_an_experiment_as_a_pipeline.png)

## <a name="building-pipelines-with-the-python-sdk"></a>Python SDK ile boru hatları inşa etme

Azure [Machine Learning Python SDK'da,](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)bir ardışık `azureml.pipeline.core` koşul, modülde tanımlanan bir Python nesnesidir. [Bir Pipeline nesnesi,](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline%28class%29?view=azure-ml-py) bir veya daha fazla [PipelineStep](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.builder.pipelinestep?view=azure-ml-py) nesnesinin sıralı sıralı sırasını içerir. Sınıf `PipelineStep` soyuttur ve gerçek adımlar [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimatorstep?view=azure-ml-py), [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.pythonscriptstep?view=azure-ml-py)veya [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep?view=azure-ml-py)gibi alt sınıflardan olacaktır. [ModuleStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.modulestep?view=azure-ml-py) sınıfı, ardışık hatlar arasında paylaşılabilen yeniden kullanılabilir bir adım dizisi tutar. A `Pipeline` bir `Experiment`parçası olarak çalışır .

Azure ML ardışık bir ardışık işlem alanı, Azure Machine Learning çalışma alanıyla ilişkilidir ve bir ardışık işlem adımı da bu çalışma alanında bulunan bir bilgi işlem hedefiyle ilişkilidir. Daha fazla bilgi için azure [portalında Azure Machine Learning çalışma alanlarını oluştur ve yönetme](https://docs.microsoft.com/azure/machine-learning/how-to-manage-workspace) veya Azure Machine [Learning'deki bilgi işlem hedefleri nelerdir?](https://docs.microsoft.com/azure/machine-learning/concept-compute-target)

Azure Machine Learning'de, bir bilgi işlem hedefi ML aşamasının oluştuğu ortamdır. Yazılım ortamı uzak bir VM, Azure Machine Learning Compute, Azure Databricks, Azure Toplu İş ve benzeri olabilir. Donanım ortamı, GPU desteğine, belleğe, depolamaya vb. bağlı olarak büyük ölçüde değişebilir. Her adım için bilgi işlem hedefini belirtebilirsiniz, bu da size maliyetler üzerinde ince taneli denetim sağlar. Projenizin belirli eylem, veri hacmi ve performans gereksinimleri için daha fazla veya daha az güçlü kaynak kullanabilirsiniz. 

## <a name="building-pipelines-with-the-designer"></a>Tasarımcı ile boru hatları oluşturma

Görsel tasarım yüzeyini tercih eden geliştiriciler, ardışık hatlar oluşturmak için Azure Machine Learning tasarımcısını kullanabilir. Bu araca çalışma alanınızın ana sayfasındaki **Tasarımcı** seçiminden erişebilirsiniz.  Tasarımcı, adımları tasarım yüzeyine sürüklemenizi ve bırakmanızı sağlar. Hızlı gelişme için, ML görev spektrumu boyunca mevcut modülleri kullanabilirsiniz; varolan modüller veri dönüşümünden algoritma seçimine, eğitimden dağıtıma kadar her şeyi kapsar. Veya Python komut dosyalarında tanımlanan kendi adımlarınızı birleştirerek tamamen özel bir ardışık kaynak oluşturabilirsiniz.

Ardışık hatları görsel olarak tasarlarken, bir adımın giriş ve çıkışları gözle görülür şekilde görüntülenir. Veri bağlantılarını sürükleyebilir ve bırakarak ardınızdaki veri akışını hızlı bir şekilde anlamanızı ve değiştirmenizi sağlayabilirsiniz.
 
![Azure Machine Learning tasarımcı örneği](./media/concept-designer/designer-drag-and-drop.gif)

### <a name="understanding-the-execution-graph"></a>Yürütme grafiğini anlama

Bir ardışık ardışık alandaki adımlar diğer adımlara bağımlı olabilir. Azure ML boru hattı hizmeti, bu bağımlılıkları çözümleme ve düzenleme işini yapar. Ortaya çıkan "yürütme grafiği" düğümleri işleme adımları vardır. Her adım, belirli bir donanım ve yazılım birleşimi oluşturmayı veya yeniden kullanmayı, önbelleğe alınmış sonuçları yeniden oluşturmayı ve benzerlerini içerebilir. Hizmetin bu yürütme grafiğinin düzenlenmesi ve optimizasyonu ml aşamasını önemli ölçüde hızlandırabilir ve maliyetleri düşürebilir. 

Adımlar bağımsız olarak çalıştırıldığı için, adımlar arasında akan giriş ve çıktı verilerini tutacak nesnelerin dışarıdan tanımlanması gerekir. Bu [DataSet](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py)ve [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py), nesnelerin rolüdür. Bu veri nesneleri, depolama yapılandırmalarını kapsülleyen bir [Datastore](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore%28class%29?view=azure-ml-py) nesnesi ile ilişkilidir. `PipelineStep` Taban sınıf her zaman `name` bir dize, `inputs`bir listesi `outputs`ve bir listesi ile oluşturulur. Genellikle, aynı zamanda bir `arguments` listesi vardır ve genellikle `resource_inputs`bir listesi olacak . Alt sınıfların da genellikle ek bağımsız değişkenleri olacaktır (örneğin, `PythonScriptStep` dosya adı ve komut dosyasının çalışması için yol gerektirir). 

Yürütme grafiği asikliktir, ancak ardışık düzenler yinelenen bir zamanlamada çalıştırılabilir ve dosya sistemine durum bilgilerini yazabilen Python komut dosyalarını çalıştırarak karmaşık profiller oluşturmayı mümkün kılar. Belirli adımların paralel veya eş zamanlı olarak çalışabilmesi için ardışık yapınızı tasarlarsanız, Azure Machine Learning, fan-out ve fan-in bağımlılık çözümlemesi ve koordinasyonunu şeffaf bir şekilde işler. Genellikle yürütme grafiğinin ayrıntıları ile kendinizi endişe etmek zorunda değilsiniz, ama [Pipeline.graph](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline.pipeline?view=azure-ml-py#attributes) özniteliği üzerinden kullanılabilir. 


### <a name="a-simple-python-pipeline"></a>Basit bir Python Boru Hattı

Bu parçacık, temel `Pipeline`bir oluşturmak ve çalıştırmak için gereken nesneleri ve çağrıları gösterir:

```python
ws = Workspace.from_config() 
blob_store = Datastore(ws, "workspaceblobstore")
compute_target = ws.compute_targets["STANDARD_NC6"]
experiment = Experiment(ws, 'MyExperiment') 

input_data = Dataset.File.from_files(
    DataPath(datastore, '20newsgroups/20news.pkl'))

output_data = PipelineData("output_data", datastore=blob_store)

input_named = input_data.as_named_input('input')

steps = [ PythonScriptStep(
    script_name="train.py",
    arguments=["--input", input_named.as_download(), "--output", output_data],
    inputs=[input_data],
    outputs=[output_data],
    compute_target=compute_target,
    source_directory="myfolder"
) ]

pipeline = Pipeline(workspace=ws, steps=steps)

pipeline_run = experiment.submit(pipeline)
pipeline_run.wait_for_completion()
```

Parçacık ortak Azure Machine Learning nesneleri ile başlar, `Workspace` `Datastore`a , a , bir `Experiment` [ComputeTarget](https://docs.microsoft.com/python/api/azureml-core/azureml.core.computetarget?view=azure-ml-py), ve bir . Daha sonra, kod tutmak `input_data` için nesneleri `output_data`oluşturur ve. Dizi `steps` tek bir öğe, `PythonScriptStep` veri nesneleri kullanacak ve üzerinde `compute_target`çalışacak bir tutar. Daha sonra, kod nesnenin `Pipeline` kendisini anında, çalışma alanı ve adım dizisinde niçin geçer. Azure ML `experiment.submit(pipeline)` ardışık hatlar hattını çalıştırma çağrısı. Boru hattı `wait_for_completion()` bitene kadar engelleme çağrısı. 

Ardışık hattınızı verilerinize bağlama hakkında daha fazla bilgi edinmek için Azure Machine Learning ve Moving [data access'teki makalelere](concept-data.md) bakın VERI erişimleri [ML ardışık hat lar (Python) içine ve arasına girer.](how-to-move-data-in-out-of-pipelines.md) 

## <a name="best-practices-when-using-pipelines"></a>Boru hatlarını kullanırken en iyi uygulamalar

Gördüğünüz gibi, Bir Azure ML ardışık oluşturmak bir komut dosyası başlatmaktan biraz daha karmaşıktır. Ardışık hatlar, birkaç Python nesnesi yapılandırmasını ve oluşturulmasını gerektirir. 

Bir ardışık hat lar kullanarak düşündüren bazı durumlar:

* Ekip ortamında: GELIŞTIRICILERin programlarını bağımsız olarak çalışabilmesi ve geliştirebilmeleri için ML görevlerini birden çok bağımsız adıma bölün. 

* Dağıtıma yakın olduğunda veya dağıtıma yakınken: yapılandırmayı çivilayın ve verileri değiştirme konusunda en üstte kalmak için zamanlanmış ve olay odaklı işlemleri kullanın.

* Bir ML projesinin erken aşamalarında veya tek başına çalışmak: yapıyı otomatikleştirmek için boru hatlarını kullanın. Yeni bir fikir uygulamadan önce yapılandırmayı ve hesaplama durumunu yeniden oluşturma konusunda endişelenmeye başladıysanız, bu iş akışını otomatikleştirmek için bir ardışık nokta kullanmayı düşünabileceğiniz bir sinyaldir. 

Önbelleğe alınmış sonuçları yeniden elde etme, bilgi işlem maliyetleri üzerinde ince taneli kontrol ve proses yalıtımı konusunda hevesli olmak kolaydır, ancak boru hatlarının maliyetleri vardır. Bazı anti-desenler şunlardır:

* Boru hatlarını endişeleri ayırmak için tek araç olarak kullanmak. Python'un yerleşik işlevleri, nesneleri ve modülleri kafa karıştırıcı programlı durumu önlemek için uzun bir yol kat eder! Bir boru hattı adımı işlev çağrısından çok daha pahalıdır.

* Boru hattı adımları arasında ağır bağlantı. Bağımlı bir adımı sık sık yeniden düzenleme yapmak önceki bir adımın çıktılarını değiştirmeyi gerektiriyorsa, ayrı adımlar şu anda bir yarardan daha fazla maliyet olabilir. Adımların aşırı birleştiğine dair bir diğer ipucu da, veri değil, işlemeyi denetlemek için bayraklar olan bir adımın bağımsız değişkenleridir. 

* İşlem kaynaklarını erken optimize etme. Örneğin, veri hazırlama için genellikle çeşitli aşamaları vardır ve bir sık sık görebilirsiniz "Oh, burada paralel programlama için bir `MpiStep` kullanabilirsiniz `PythonScriptStep` bir yer ama burada daha az güçlü bir bilgi işlem hedefi ile kullanabilirsiniz bir yer," ve benzeri. Ve belki de, uzun vadede, bunun gibi ince taneli adımlar oluşturmak değerli olabilir, özellikle de önbelleğe alınmış sonuçları her zaman yeniden hesaplamak yerine kullanma olasılığı varsa. Ancak boru hatları Python'un yerel `multiprocessing` modülün yerini almak için tasarlanmamıştır. 

Bir proje büyük olana veya dağıtıma yaklaşınceye kadar, ardışık hatlar oldukça ince taneli olmalıdır. ML projenizi _aşamaları_ ve bir boru hattını içeren bir boru hattı olarak düşünürseniz, sizi belirli bir aşamadan geçirmek için tam bir iş akışı sağlarsanız, doğru yoldasınız. 

## <a name="key-advantages"></a>Önemli avantajlar

Makine öğrenimi iş akışlarınız için boru hatları kullanmanın temel avantajları şunlardır:

|Önemli avantaj|Açıklama|
|:-------:|-----------|
|**Katılımsız&nbsp;çalıştırmalar**|Adımları güvenilir ve katılımsız bir şekilde paralel veya sırayla çalışacak şekilde zamanlayın. Veri hazırlama ve modelleme günler veya haftalar sürebilir ve ardışık işler, işlem çalışırken diğer görevlere odaklanmanıza olanak tanır. |
|**Heterojen hesaplama**|Heterojen ve ölçeklenebilir işlem kaynakları ve depolama konumları arasında güvenilir bir şekilde koordine edilmiş birden çok ardışık işlem aygıtı kullanın. HDInsight, GPU Data Science VM'ler ve Databricks gibi farklı bilgi işlem hedeflerinde tek tek ardışık işlem adımları çalıştırarak kullanılabilir bilgi işlem kaynaklarını verimli bir şekilde kullanın.|
|**Yeniden kullanılabilirlik**|Yeniden eğitim ve toplu puanlama gibi belirli senaryolar için ardışık işlem şablonları oluşturun. Basit REST çağrıları aracılığıyla harici sistemlerden yayınlanan boru hatlarını tetikler.|
|**İzleme ve sürüm**|Verileri ve sonuç yollarını, siz yinelerken el ile izlemek yerine, veri kaynaklarınızı, girişlerinizi ve çıktılarınızı açıkça adlandırmak ve sürüm olarak SDK ardışık hatlar hattını kullanın. Daha fazla üretkenlik için komut dosyalarını ve verileri ayrı ayrı yönetebilirsiniz.|
| **Modüler** | Endişe alanlarını ayırmak ve değişiklikleri yalıtmak, yazılımın daha yüksek kaliteyle daha hızlı gelişmesini sağlar. | 
|**İş Birliği**|Boru hatları, veri bilimcilerin makine öğrenimi tasarım sürecinin tüm alanlarında işbirliği yapmasına olanak sağlarken, aynı anda boru hattı adımları üzerinde çalışabilme lerini sağlar.|

### <a name="choosing-the-proper-pipelinestep-subclass"></a>Uygun PipelineStep alt sınıfını seçme

Soyut `PythonScriptStep` en esnek alt `PipelineStep`sınıftır. Alt sınıflar gibi `EstimatorStep` diğer alt `DataTransferStep` sınıflar ve daha az kodla belirli görevleri gerçekleştirebilirsiniz. Örneğin, bir `EstimatorStep` adım, bir `Estimator`ve bir işlem hedefi için bir ad geçerek oluşturulabilir. Veya giriş ve çıkışları, boru hattı parametrelerini ve bağımsız değişkenleri geçersiz kılabilir. Daha fazla bilgi için, [tahminci kullanarak Azure Machine Learning'e sahip Train modellerine](how-to-train-ml-models.md)bakın. 

Veri `DataTransferStep` kaynakları ve lavabolar arasında veri taşımayı kolaylaştırır. Bu aktarımı el ile yapmak için kod basit ama yinelenen. Bunun yerine, yalnızca `DataTransferStep` bir ad, bir veri kaynağı ve veri lavabo ve bir bilgi işlem hedefi referansları ile oluşturabilirsiniz. [DataTransferStep ile](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-data-transfer.ipynb) birlikte dizüstü Azure Machine Learning Pipeline bu esnekliği gösterir.

## <a name="modules"></a>Modüller

Ardışık hatlar adımları önceki çalıştırmanın sonuçlarının yeniden kullanılmasına izin verirken, çoğu durumda adımın yapısı, gerekli komut dosyalarının ve bağımlı dosyaların yerel olarak kullanılabilir olması gerektiğini varsayar. Bir veri bilimci varolan kodun üzerine oluşturmak istiyorsa, komut dosyaları ve bağımlılıklar genellikle ayrı bir depodan klonlanmalıdır.

Modüller, kullanımda ardışık basamakadımlarına benzer, ancak çalışma alanı aracılığıyla kolaylaştırılan sürümler sağlar ve bu da ölçekte işbirliği ve yeniden kullanılabilirlik sağlar. Modüller birden çok ardışık alanda yeniden kullanılmak üzere tasarlanmıştır ve belirli bir hesaplamayı farklı kullanım durumlarına uyarlamak için gelişebilir. Kullanıcılar dış depoları kullanmadan çalışma alanı üzerinden aşağıdaki görevleri yapabilir:

* Yeni modüller oluşturun ve mevcut modüllerin yeni sürümlerini yayımlayın
* Varolan sürümleri amortismana ayırma
* Tüketicilerin bu sürümü kullanmasını engellemek için devre dışı bırakılan sürümleri işaretle
* Varsayılan sürümleri belirleme
* Ekiplerin aynı kodu kullandığından emin olmak için modülleri çalışma alanından sürüme göre alın

Azure Machine Learning ardışık hatlarında modüllerin nasıl oluşturulup bağlanılacak ve kullanılacağıyla ilgili kod örnekleri için [not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-how-to-use-modulestep.ipynb) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure ML boru hatları, erken geliştirme aşamalarında değer sunmaya başlayan güçlü bir tesistir. Takım ve proje büyüdükçe değer artar. Bu makalede, Azure Machine Learning Python SDK ile boru hatlarının nasıl belirtildiği ve Azure'da nasıl düzenlendiği açıklanmıştır. Bazı temel kaynak kodu gördünüz ve kullanılabilen `PipelineStep` birkaç sınıfla tanıştınız. Azure ML ardışık hatlarını ne zaman kullanacağınız ve Azure'un bunları nasıl çalıştırdığınızı anlamanız gerekir. 


+ [İlk ardışık hattınızı](how-to-create-your-first-pipeline.md)nasıl oluşturabilirsiniz öğrenin.

+ [Büyük verilerde toplu iş tahminlerini nasıl çalıştırılacıolarak](tutorial-pipeline-batch-scoring-classification.md )öğrenin.

+ Boru hattı çekirdeği ve [boru hattı](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) adımları için SDK başvuru [dokümanlarına](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py)bakın.

+ [Azure Machine Learning boru hatlarını](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines)gösteren örnek Jupyter dizüstü bilgisayarları deneyin. Bu hizmeti keşfetmek için not defterlerini nasıl [çalıştırılacıolarak çalıştırılamayı](samples-notebooks.md)öğrenin.
