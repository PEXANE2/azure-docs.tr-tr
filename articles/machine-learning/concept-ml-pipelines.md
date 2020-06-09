---
title: Azure Machine Learning işlem hatları nedir?
description: Machine Learning (ML) ardışık düzenleri makine öğrenimi iş akışlarını oluşturmanıza, iyileştirmenize ve yönetmenize nasıl yardımcı olduğunu öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: laobri
author: lobrien
ms.date: 04/01/2020
ms.custom: tracking-python
ms.openlocfilehash: 7d34b54349a681061f80798a19ce65d2266f589d
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84559480"
---
# <a name="what-are-azure-machine-learning-pipelines"></a>Azure Machine Learning işlem hatları nelerdir?

Bu makalede, Azure Machine Learning işlem hatları makine öğrenimi iş akışlarını oluşturmanıza, iyileştirmenize ve yönetmenize nasıl yardımcı olabileceğini öğrenebilirsiniz. Bu iş akışlarının çeşitli avantajları vardır: 

+ Olması
+ Hız
+ Yinelenebilirlik
+ Esneklik
+ Sürüm oluşturma ve izleme
+ Modülerlik 
+ Kalite denetimi
+ Maliyet denetimi

Bu avantajlar, Machine Learning projeniz saf araştırmadan ve yinelemeye doğru ilerlemediğinde önemli hale gelir. Basit tek adımlı işlem hatları da değerli olabilir. Makine öğrenimi projeleri genellikle karmaşık bir durumlardır ve tek bir iş akışının kesin bir işlem olmasını sağlamak zor olabilir.

[İlk işlem hattınızı oluşturmayı](how-to-create-your-first-pipeline.md)öğrenin.

![Azure Machine Learning 'de makine öğrenimi ardışık düzenleri](./media/concept-ml-pipelines/pipeline-flow.png)

<a name="compare"></a>
### <a name="which-azure-pipeline-technology-should-i-use"></a>Hangi Azure ardışık düzen teknolojisini kullanmalıyım?

Azure bulutu, her biri farklı bir amaca sahip birkaç farklı işlem hattı sağlar. Aşağıdaki tabloda, farklı işlem hatları ve bunların kullanıldıkları özellikler listelenmiştir:

| Senaryo | Birincil kişi | Azure teklifi | OSS sunumu | Kurallı Kanal | Yönleri | 
| -------- | --------------- | -------------- | ------------ | -------------- | --------- | 
| Model düzenleme (Machine Learning) | Veri bilimcisi | Azure Machine Learning işlem hatları | Kubeflow işlem hatları | Veri > modeli | Dağıtım, önbelleğe alma, kod ilk, yeniden kullanım | 
| Veri düzenleme (veri hazırlığı) | Veri mühendisi | [Azure Data Factory işlem hatları](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) | Apache Airflow | Veri > verileri | Kesin tür belirtilmiş hareket. Veri merkezli etkinlikler. |
| Kod & uygulama düzenlemesi (CI/CD) | Uygulama geliştiricisi/Ops | [Azure DevOps işlem hatları](https://azure.microsoft.com/services/devops/pipelines/) | Jenkins | Kod + model-> App/Service | En açık ve esnek etkinlik desteği, onay kuyrukları, geçişi olan aşamalar | 

## <a name="what-can-azure-ml-pipelines-do"></a>Azure ML işlem hatları ne yapabilir?

Azure Machine Learning işlem hattı, bir makine öğrenimi görevinin bağımsız bir yürütülebilir iş akışıdır. Alt görevler işlem hattı içinde bir dizi adım olarak kapsüllenir. Bir Azure Machine Learning işlem hattı, Python betiği çağıran bir basit olabilir, bu nedenle yalnızca herhangi bir şey _olabilir_ . İşlem hatları, şu gibi makine öğrenimi görevlerine _odaklanmalıdır_ :

+ İçeri aktarma, doğrulama ve temizleme, ham verileri kullanışlı hale getirme ve dönüştürme, normalleştirme ve hazırlamayı içeren veri hazırlığı
+ Bağımsız değişkenleri, dosya yollarını ve günlük / raporlama yapılandırmalarını parametreleştirmeyi içeren eğitim yapılandırması
+ Verimli ve sürekli olarak eğitim ve doğrulama. Verimlilik, belirli veri alt kümeleri, farklı donanım işlem kaynakları, dağıtılmış işlem ve ilerleme izlemeyi belirtmekten gelebilir
+ Sürüm oluşturma, ölçeklendirme, sağlama ve erişim denetimini içeren dağıtım 

Bağımsız adımlar, birden fazla veri bilimcilerinin aynı işlem hattı üzerinde, yoğun işlem kaynakları olmadan aynı anda çalışmasına izin verir. Ayrı adımlar, her adım için farklı işlem türleri/boyutları kullanmayı da kolaylaştırır.

İşlem hattı tasarlandıktan sonra, işlem hattının eğitim döngüsü etrafında genellikle daha fazla ince ayar yapılır. Bir işlem hattını yeniden çalıştırdığınızda, çalıştırma, güncelleştirilmiş bir eğitim betiği gibi yeniden çalıştırılması gereken adımlara atlar. Yeniden çalıştırılması gerekmeyen adımlar atlanır. Aynı analiz, adımın yerine kullanılan değiştirilmemiş betikler için geçerlidir. Bu yeniden kullanım işlevselliği, temel alınan veriler değişmemişse veri alımı ve dönüştürme gibi maliyetli ve zaman uyumlu adımların çalıştırılmasının önlenmesine yardımcı olur.

Azure Machine Learning, işlem hattınızdaki her adım için PyTorch veya TensorFlow gibi çeşitli araç setleri ve çerçeveler kullanabilirsiniz. Azure, kullandığınız çeşitli [işlem hedeflerini](concept-azure-machine-learning-architecture.md) koordine eder, bu sayede ara verileriniz aşağı akış işlem hedefleri ile paylaşılabilir.

İşlem [hattı denemeleri için ölçümleri](https://docs.microsoft.com/azure/machine-learning/how-to-track-experiments) doğrudan Azure Portal veya [çalışma alanı giriş sayfanız (Önizleme)](https://ml.azure.com)içinde izleyebilirsiniz. Bir işlem hattı yayımlandıktan sonra, herhangi bir platform veya yığından işlem hattını yeniden çalıştırmanıza olanak sağlayan bir REST uç noktası yapılandırabilirsiniz.

Kısacası, makine öğrenimi yaşam döngüsünün tüm karmaşık görevlerine işlem hatları ile yardımcı olabilir. Diğer Azure ardışık düzen teknolojilerinin kendi güçlü yönleri vardır. [Azure Data Factory işlem hatları](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) , verilerle çalışma ve [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) sürekli tümleştirme ve dağıtım için doğru araçtır. Ancak odaklanmanız makine öğrenimi ise, Azure Machine Learning işlem hatları, iş akışı gereksinimleriniz için en iyi seçenektir. 

## <a name="what-are-azure-ml-pipelines"></a>Azure ML işlem hatları nelerdir?

Azure ML işlem hattı, sıralı bir adım dizisi ile tamamlanmış bir mantıksal iş akışı gerçekleştirir. Her adım ayrı bir işleme eylemi olur. İşlem hatları Azure Machine Learning [deneme](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py)bağlamında çalışır.

Bir ML projesinin erken aşamalarında, Azure çalışma alanı ve kaynak yapılandırması, veri hazırlama, çalıştırma yapılandırma, eğitim ve doğrulama çalışmalarını yapan tek bir Jupyter Not defteri veya Python betiğinin olması çok güzel. Ancak, işlevlerin ve sınıfların tek bir kesinlik bloğu için hızlı bir şekilde tercih edileceği şekilde, ML iş akışlarının tek parçalı bir not defteri veya betiğe hızla tercih edileceği. 

ML görevleri modüle göre, işlem hatları, bir bileşenin "do (yalnızca) bir tane daha iyi olması gerektiğini, bilgisayar bilimi 'ni destekler. Modülerlik, ekiplerde programlarken proje başarısı için çok önemlidir, ancak tek başına çalışırken bile, küçük bir ML projesi her biri ayrı bir karmaşıklık miktarı olan ayrı görevler içerir. Görevler şunlardır: çalışma alanı yapılandırma ve veri erişimi, veri hazırlama, model tanımı ve yapılandırma ve dağıtım. Bir ya da daha fazla görevin çıkışları, girdileri başka bir şekilde oluştururken, her bir görevin tam uygulama ayrıntıları, en iyi ve bir sonraki konuda ilgisiz değildir. En kötü durumda, bir görevin hesaplama durumu başka bir hataya neden olabilir. 

### <a name="analyzing-dependencies"></a>Bağımlılıklar çözümleniyor

Birçok programlama ekosisteminde kaynak, kitaplık veya derleme bağımlılıklarını düzenleyen araçlar vardır. Genellikle, bu araçlar bağımlılıkları hesaplamak için dosya zaman damgalarını kullanır. Bir dosya değiştirildiğinde yalnızca bu ve bağımlıları güncelleştirilir (indirilir, yeniden derlenir veya paketlenmiş). Azure ML işlem hatları bu kavramı önemli ölçüde genişletir. Geleneksel derleme araçları gibi işlem hatları, adımlar arasındaki bağımlılıkları hesaplar ve yalnızca gerekli yeniden hesaplamaları gerçekleştirir. 

Azure ML işlem hatları 'ndaki bağımlılık analizi, basit zaman damgalarından daha karmaşıktır. Her adım farklı bir donanım ve yazılım ortamında çalıştırılabilir. Veri hazırlama zaman alan bir işlem olabilir ancak güçlü GPU 'Lar ile donanımda çalıştırılması gerekmez, bazı adımlar işletim sistemine özgü yazılım gerektirebilir, dağıtılmış eğitim kullanmak isteyebilirsiniz ve bu şekilde devam edebilir. Kaynakları iyileştirmek için maliyet tasarrufları önemli olabilir, ancak donanım ve yazılım kaynaklarındaki farklı çeşitliliğe el ile karşı el ile karşı bir işlem yapmak oldukça zor olabilir. Adımlar arasında aktardığınız verilerde bir hata yapmadan tüm bunları yapmak daha da zordur. 

İşlem hatları bu sorunu çözüyor. Azure Machine Learning, işlem hattı adımları arasındaki tüm bağımlılıkları otomatik olarak düzenler. Bu düzenleme, Docker görüntülerini alıp, işlem kaynaklarını ekleyerek ve ayırmayı ve adımlar arasında tutarlı ve otomatik bir şekilde veri taşımayı içerebilir.

### <a name="reusing-results"></a>Sonuçları yeniden kullanma

Ayrıca, bir adımın çıktısı, öğesini seçerseniz, yeniden kullanılabilir. Bir olasılık olarak yeniden kullan seçeneğini belirtirseniz ve yeniden hesaplamayı tetikleyen yukarı akış bağımlılığı yoksa, işlem hattı hizmeti adım sonuçlarının önbelleğe alınmış bir sürümünü kullanır. Bu yeniden kullanım, geliştirme süresini önemli ölçüde azaltabilir. Karmaşık bir veri hazırlama göreviniz varsa, büyük olasılıkla kesinlikle gerekli olandan daha sık yeniden çalıştırmanız gerekir. İşlem hatları sizi merak eder: gerekirse, adım çalışır, yoksa, bu işlem çalışmaz.

Bu bağımlılık analizi, düzenleme ve etkinleştirmenin hepsi, bir işlem [hattı](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline(class)?view=azure-ml-py) nesnesini örneklediğinizde, bir öğesine geçirdiğinizde ve çağırdığınızda Azure Machine Learning tarafından işlenir `Experiment` `submit()` . 

### <a name="coordinating-the-steps-involved"></a>İlgili adımları koordine etme

Bir nesne oluşturup çalıştırdığınızda `Pipeline` , aşağıdaki üst düzey adımlar oluşur:

+ Her adım için, hizmet gereksinimleri hesaplar:
    + Donanım işlem kaynakları
    + İşletim sistemi kaynakları (Docker görüntüsü)
    + Yazılım kaynakları (Conda/virtualenv bağımlılıkları)
    + Veri girişleri 
+ Hizmet, adımlar arasındaki bağımlılıkları belirler ve bu, dinamik bir yürütme grafiğine yol açar
+ Yürütme grafiğindeki her düğüm çalıştığında:
    + Hizmet gereken donanım ve yazılım ortamını yapılandırır (Belki de mevcut kaynakları yeniden kullanılıyor)
    + Bu adım çalışma, kapsayan nesnesine günlük kaydı ve izleme bilgilerini sağlar `Experiment`
    + Adım tamamlandığında, çıkışları bir sonraki adıma giriş olarak hazırlanır ve/veya depolama alanına yazılır
    + Artık gerekli olmayan kaynaklar sonlandırılır ve ayrılır

![Ardışık düzen adımları](./media/concept-ml-pipelines/run_an_experiment_as_a_pipeline.png)

## <a name="building-pipelines-with-the-python-sdk"></a>Python SDK ile işlem hatları oluşturma

[Azure Machine Learning Python SDK 'sında](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), işlem hattı modülünde tanımlanan bir Python nesnesidir `azureml.pipeline.core` . İşlem [hattı](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline%28class%29?view=azure-ml-py) nesnesi bir veya daha fazla [ardışık düzen inestep](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.builder.pipelinestep?view=azure-ml-py) nesnesinin sıralı dizisini içerir. `PipelineStep`Sınıf soyuttur ve gerçek adımlar, [Estimatorstep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimatorstep?view=azure-ml-py), [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.pythonscriptstep?view=azure-ml-py)veya [datatransferstep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep?view=azure-ml-py)gibi alt sınıflar olacaktır. [Modulestep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.modulestep?view=azure-ml-py) sınıfı, işlem hatları arasında paylaşılabilen, yeniden kullanılabilir bir adım dizisi içerir. Bir `Pipeline` parçası olarak çalışır `Experiment` .

Bir Azure ML işlem hattı, bir Azure Machine Learning çalışma alanıyla ilişkilendirilir ve bir işlem hattı adımı söz konusu çalışma alanı içinde kullanılabilir bir işlem hedefi ile ilişkilendirilir. Daha fazla bilgi için bkz. [Azure portal Azure Machine Learning çalışma alanları oluşturma ve yönetme](https://docs.microsoft.com/azure/machine-learning/how-to-manage-workspace) veya [Azure Machine Learning 'Nda işlem hedefleri nelerdir?](https://docs.microsoft.com/azure/machine-learning/concept-compute-target).

Azure Machine Learning, bir işlem hedefi, bir ML aşamasının gerçekleştiği ortamdır. Yazılım ortamı, uzak bir VM, Azure Machine Learning Işlem, Azure Databricks, Azure Batch vb. olabilir. Ayrıca, donanım ortamı GPU desteğine, belleğe, depolamaya ve benzeri önemli ölçüde farklılık gösterebilir. Her adım için işlem hedefini belirtebilir, bu, maliyetleri üzerinde ayrıntılı denetim sağlar. Projenizin belirli bir eylemi, veri hacmi ve performans ihtiyaçları için daha fazla veya daha az güçlü kaynak kullanabilirsiniz. 

## <a name="building-pipelines-with-the-designer"></a>Tasarımcı ile işlem hatları oluşturma

Görsel tasarım yüzeyini tercih eden geliştiriciler, işlem hatları oluşturmak için Azure Machine Learning tasarımcısını kullanabilir. Bu araca, çalışma alanınızın giriş sayfasındaki **Tasarımcı** seçiminden erişebilirsiniz.  Tasarımcı, tasarım yüzeyine olan adımları sürükleyip bırakmayı sağlar. Hızlı geliştirme için, ML görevlerinin spektrumu genelinde mevcut modülleri kullanabilirsiniz; mevcut modüller, veri dönüşümden algoritma seçimine, dağıtıma eğitimlere kadar her şeyi kapsar. Ya da Python betiklerinizde tanımlanan kendi adımlarınızı birleştirerek tamamen özel bir işlem hattı oluşturabilirsiniz.

İşlem hatlarını görsel olarak tasarladığınızda, bir adımın giriş ve çıkışları görünmez bir şekilde görüntülenir. Veri akışını sürükleyip bırakabilir, böylece işlem hattınızdaki veri akışını hızla anlamanıza ve değiştirebilirsiniz.
 
![Azure Machine Learning tasarımcı örneği](./media/concept-designer/designer-drag-and-drop.gif)

### <a name="understanding-the-execution-graph"></a>Yürütme grafiğini anlama

Bir işlem hattının içindeki adımlarda diğer adımlara bağımlılıklar olabilir. Azure ML işlem hattı hizmeti bu bağımlılıkları çözümleme ve düzenleme işini yapar. Sonuçta elde edilen "yürütme grafiği" düğümleri işleme adımları. Her adım, belirli bir donanım ve yazılım birleşimini oluşturma veya yeniden kullanma, önbelleğe alınmış sonuçları yeniden kullanma ve benzeri bir işlem içerebilir. Hizmetin bu yürütme grafiğinin düzenlemesi ve iyileştirmesi, bir ML aşamasını önemli ölçüde hızlandırır ve maliyetleri azaltabilir. 

Adımlar bağımsız olarak çalıştırıldığı için, adımlar arasında akan giriş ve çıkış verilerini tutacak nesneler dışarıdan tanımlanmalıdır. Bu, [veri kümesi](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference?view=azure-ml-py)ve [pipelinedata](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py), nesneleri rolüdür. Bu veri nesneleri, depolama yapılandırmalarını kapsülleyen bir [veri deposu](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore%28class%29?view=azure-ml-py) nesnesi ile ilişkilendirilir. `PipelineStep`Temel sınıf her zaman bir `name` dize, liste listesi `inputs` ve bir listesi ile oluşturulur `outputs` . Genellikle, bir listesini de içerir `arguments` ve genellikle bir listesi olur `resource_inputs` . Alt sınıflarda genellikle ek bağımsız değişkenler de olur (örneğin, `PythonScriptStep` çalıştırılacak betiğin dosya adını ve yolunu gerektirir). 

Yürütme grafiği Çevrimsiz olur, ancak işlem hatları yinelenen bir zamanlamaya göre çalıştırılabilir ve dosya sistemine durum bilgilerini yazabilmesi için karmaşık profiller oluşturmak mümkün hale getiren Python betikleri çalıştırılabilir. İşlem hattınızı, belirli adımların paralel veya zaman uyumsuz olarak çalıştırılabilmesini sağlayacak şekilde tasarlarsanız, Azure Machine Learning, hem fan hem de fan için bağımlılık analizini ve koordinasyonunu, saydam bir şekilde işler. Genellikle yürütme grafiğinin ayrıntıları ile ilgilenmeniz gerekmez, ancak işlem [hattı. Graph](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline.pipeline?view=azure-ml-py#attributes) özniteliği aracılığıyla bulunabilir. 


### <a name="a-simple-python-pipeline"></a>Basit bir Python işlem hattı

Bu kod parçacığı, temel bir oluşturmak ve çalıştırmak için gereken nesneleri ve çağrıları gösterir `Pipeline` :

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

Kod parçacığı, ortak Azure Machine Learning nesneleri, a `Workspace` , a `Datastore` , [ComputeTarget](https://docs.microsoft.com/python/api/azureml-core/azureml.core.computetarget?view=azure-ml-py)ve ile başlar `Experiment` . Daha sonra kod, ve tutulacak nesneleri oluşturur `input_data` `output_data` . Dizi, `steps` `PythonScriptStep` veri nesnelerini kullanacak ve üzerinde çalışacak olan tek bir öğesi barındırır `compute_target` . Daha sonra, kod `Pipeline` nesnenin kendisini başlatır, çalışma alanı ve Steps dizisine geçer. `experiment.submit(pipeline)`Azure ML işlem hattı çalıştırmasını Başlatan çağrı. `wait_for_completion()`İşlem hattı bitene kadar blokların çağrısı. 

İşlem hattınızı verilerinize bağlama hakkında daha fazla bilgi edinmek için [Azure Machine Learning makaleleri veri erişimi](concept-data.md) ' ne bakın ve [verileri ml ardışık düzen adımları (Python) arasında ve arasında hareket ettirin](how-to-move-data-in-out-of-pipelines.md). 

## <a name="best-practices-when-using-pipelines"></a>İşlem hatları kullanırken en iyi uygulamalar

Gördüğünüz gibi, bir Azure ML işlem hattı oluşturmak, bir betiği başlatmaktan biraz daha karmaşıktır. İşlem hatları birkaç Python nesnesinin yapılandırılmasını ve oluşturulmasını gerektirir. 

İşlem hattı kullanmayı öneren bazı durumlar:

* Bir ekip ortamında: geliştiricilerin programlarını bağımsız olarak çalışabilmesi ve gelişmesi için ML görevlerini birden çok bağımsız adımlara bölün. 

* Ya da neredeyse dağıtım sırasında: yapılandırmayı değiştirip, değişen verilerin üzerine kalmak için zamanlanmış ve olay odaklı işlemleri kullanın.

* Bir ML projesinin erken aşamalarında veya tek başına çalışırken: derlemeyi otomatikleştirmek için işlem hatlarını kullanın. Yeni bir fikir uygulamadan önce yapılandırma ve hesaplama durumunu yeniden oluşturma konusunda endişelenmeye başladıysanız, bu, iş akışını otomatikleştirmek için bir işlem hattı kullanmayı düşünebileceğiniz bir sinyaldir. 

Önbelleğe alınmış sonuçları yeniden kullanma, işlem maliyetleri üzerinde ayrıntılı denetim ve işlem yalıtımının yanı sıra işlem hatlarında maliyetleri vardır. Bazı anti-desenler şunları içerir:

* Sorunları ayırmak için tek bir anlamı olan işlem hatlarını kullanma. Python 'un yerleşik işlevleri, nesneleri ve modülleri, programlı bir durum oluşmasını önlemek için uzun bir yoldur! İşlem hattı adımı, bir işlev çağrısından çok daha pahalıdır.

* Ardışık düzen adımları arasında ağır bir eşlenme. Bir bağımlı adımı sık sık yeniden düzenleme, önceki bir adımın çıktılarının değiştirilmesini gerektiriyorsa, bu durum büyük bir avantajdan daha fazla maliyetle zaten daha fazla maliyetlidir. Bu adımların çok fazla bağlanmış olduğunu gösteren bir adım, veri olmayan bir adımın bağımsız değişkenlerdir ancak işlemeyi denetlemek için bayraklar. 

* İşlem kaynaklarını erken iyileştirme. Örneğin, veri hazırlığı için genellikle birkaç aşama vardır ve biri genellikle "Oh, `MpiStep` hem de paralel programlama için kullanabilirim, ancak `PythonScriptStep` daha az güçlü bir işlem hedefi olan bir yer olan bir yerde" vb. Belki de uzun çalıştırmada, özellikle de her zaman yeniden hesaplama yerine önbelleğe alınmış sonuçları kullanma olanağı varsa, bu gibi hassas adımlar oluşturma. Ancak işlem hatları, Python 'un yerel modülünün yerini alacak şekilde tasarlanmamıştır `multiprocessing` . 

Bir proje büyük veya daha iyi bir dağıtım yapana kadar, işlem hatlarınızın hassas olması yerine kaba olması gerekir. ML projenizi _aşamalar_ ve bir işlem hattı dahil olmak üzere düşünüyorsanız, sizi belirli bir aşamada hareket ettirmek üzere bir iş akışı sağlamak için doğru yolda olursunuz. 

## <a name="key-advantages"></a>Önemli avantajlar

Machine Learning iş akışlarınız için işlem hatlarını kullanmanın temel avantajları şunlardır:

|Önemli avantaj|Description|
|:-------:|-----------|
|**Katılımsız &nbsp; çalıştırmalar**|Adımları güvenilir ve katılımsız bir şekilde paralel veya sırayla çalışacak şekilde zamanlayın. Veri hazırlama ve modelleme son günler veya haftadır ve işlem hatları, işlem çalışırken diğer görevlere odaklanabilmenize olanak tanır. |
|**Heterojen işlem**|Heterojen ve ölçeklenebilir işlem kaynakları ve depolama konumları genelinde güvenilir bir şekilde koordine edilen birden çok işlem hattı kullanın. HDInsight, GPU veri bilimi VM 'Leri ve Databricks gibi farklı işlem hedeflerinde bireysel ardışık düzen adımlarını çalıştırarak kullanılabilir işlem kaynaklarının verimli bir şekilde kullanılmasını sağlayın.|
|**Ölçeklendirilebileceği**|Yeniden eğitme ve Batch Puanlama gibi belirli senaryolar için ardışık düzen şablonları oluşturun. Dış sistemlerden yayınlanan işlem hatlarını basit REST çağrıları aracılığıyla tetikleyin.|
|**İzleme ve sürüm oluşturma**|Yineleme sırasında verileri ve sonuç yollarını el ile izlemek yerine, veri kaynaklarınızı, girdilerlerinizi ve çıkışları açıkça adlandırmak ve sürümüne eklemek için işlem hatları SDK 'sını kullanın. Ayrıca, daha fazla üretkenlik için betikleri ve verileri ayrı olarak yönetebilirsiniz.|
| **Modülerlik** | Kaygıları ve değişiklikleri yalıtma alanlarının ayrılması, yazılımın daha yüksek kalitede daha hızlı gelişmeye olanak tanır. | 
|**İşbirliği**|İşlem hatları, veri bilimcilerinin makine öğrenimi tasarım sürecinin tüm alanlarında işbirliği yapmasına olanak sağlarken işlem hattı adımlarında eşzamanlı olarak çalışabiliyor.|

### <a name="choosing-the-proper-pipelinestep-subclass"></a>Doğru ardışık düzen Inestep alt sınıfını seçme

, `PythonScriptStep` Özetin en esnek alt sınıfıdır `PipelineStep` . Alt sınıflar gibi diğer alt sınıflar `EstimatorStep` ve `DataTransferStep` daha az kodlu belirli görevleri gerçekleştirebilir. Örneğin, bir `EstimatorStep` yalnızca adım, bir `Estimator` ve bir işlem hedefi için bir ad geçirerek oluşturulabilir. Ya da girişleri ve çıkışları, işlem hattı parametrelerini ve bağımsız değişkenleri geçersiz kılabilirsiniz. Daha fazla bilgi için bkz. [tahmin aracı kullanarak Azure Machine Learning modelleri eğitme](how-to-train-ml-models.md). 

Verileri `DataTransferStep` veri kaynakları ve havuzlar arasında taşımayı kolaylaştırır. Bu aktarmayı el ile yapmak için kod basit ancak tekrarlı. Bunun yerine, yalnızca bir `DataTransferStep` ad, bir veri kaynağı ve veri havuzu ve bir işlem hedefi ile bir oluşturma oluşturabilirsiniz. [DataTransferStep ile](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-data-transfer.ipynb) not defteri Azure Machine Learning işlem hattı bu esnekliği gösterir.

## <a name="modules"></a>Modül

Ardışık düzen adımları önceki çalıştırmanın sonuçlarının yeniden kullanılmasına izin verirken, çoğu durumda adımın oluşturulması, betiklerin ve bağımlı dosyaların gerekli olduğu varsayılır. Bir veri bilimconu mevcut kodların üzerine derlemek istiyorsa, betikler ve bağımlılıkların genellikle ayrı bir depodan klonlanmış olması gerekir.

Modüller, ardışık düzen adımları için kullanımlarda benzerdir, ancak çalışma alanı aracılığıyla kolaylaştırırken, birlikte çalışma ve ölçek açısından yeniden kullanılabilirlik sağlar. Modüller birden çok işlem hattına yeniden kullanılmak üzere tasarlanmıştır ve belirli bir hesaplamayı farklı kullanım durumlarına uyarlamak için gelişebilirler. Kullanıcılar, dış depoları kullanmadan, çalışma alanı aracılığıyla aşağıdaki görevleri gerçekleştirebilir:

* Yeni modüller oluşturma ve var olan modüllerin yeni sürümlerini yayımlama
* Mevcut sürümleri kullanımdan kaldırma
* Tüketicilerin bu sürümü kullanmasını engellemek için sürümleri devre dışı olarak işaretleyin
* Varsayılan sürümleri belirleme
* Takımların aynı kodu kullandığından emin olmak için, çalışma alanından modülleri sürüme göre al

Azure Machine Learning işlem hatları 'nda modül oluşturma, bağlanma ve kullanma hakkında kod örnekleri için [Not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-how-to-use-modulestep.ipynb) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure ML işlem hatları, erken geliştirme aşamalarında değer sunmaya başlayan güçlü bir olanaklardır. Takım ve proje büyüdükçe değer artar. Bu makalede, işlem hatlarının Azure Machine Learning Python SDK ile nasıl belirtildiği ve Azure 'da düzenlendiği açıklanmaktadır. Bazı temel kaynak kodlarını gördünüz ve `PipelineStep` kullanılabilir sınıfların bazılarına sunulmuştur. Azure ML işlem hatlarını ne zaman kullanacağınızı ve Azure 'un nasıl çalıştığını anlamalısınız. 


+ [İlk işlem hattınızı oluşturmayı](how-to-create-your-first-pipeline.md)öğrenin.

+ [Büyük verilerde toplu tahmine dayalı tahminleri nasıl çalıştıracağınızı](tutorial-pipeline-batch-scoring-classification.md )öğrenin.

+ İşlem [hattı çekirdeği](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) ve [ardışık düzen adımları](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py)için SDK başvuru belgeleri bölümüne bakın.

+ İşlem [hatları Azure Machine Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines)gösteren örnek bir Jupyter Not defteri deneyin. [Bu hizmeti araştırmak için not defterlerini çalıştırmayı](samples-notebooks.md)öğrenin.
