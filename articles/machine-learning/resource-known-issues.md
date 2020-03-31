---
title: Sorun giderme & bilinen sorunlar
titleSuffix: Azure Machine Learning
description: Azure Machine Learning için bilinen sorunların, geçici geçici işlerin ve sorun giderme lerin bir listesini alın.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: d5525c02edb30eff0ee8971a382f2acb8f2e57ee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79455732"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning"></a>Bilinen sorunlar ve Azure Machine Learning sorun giderme

Bu makale, Azure Machine Learning kullanırken karşılaşılan hataları veya hataları bulmanıza ve düzeltmenize yardımcı olur.

## <a name="sdk-installation-issues"></a>SDK kurulum sorunları

**Hata iletisi: 'PyYAML' kaldıramıyor**

Python için Azure Machine Learning SDK: PyYAML yüklü bir projedir. Bu nedenle, kısmi bir kaldırma varsa hangi dosyaların ona ait olduğunu doğru bir şekilde belirleyemiyoruz. Bu hatayı yoksayarak SDK'yı yüklemeye devam etmek için şunları kullanın:

```Python
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

**Hata iletisi:`ERROR: No matching distribution found for azureml-dataprep-native`**

Anaconda'nın Python 3.7.4 dağıtımı azureml-sdk yüklemesini bozan bir hataya sahiptir. Bu sorun bu [GitHub Sorunu'nda](https://github.com/ContinuumIO/anaconda-issues/issues/11195) ele alınmıştır Bu komutu kullanarak yeni bir Conda Ortamı oluşturarak üzerinde çalışılabilir:
```bash
conda create -n <env-name> python=3.7.3
```
Python 3.7.3 kullanarak bir Conda Ortamı oluşturur, 3.7.4'te yükleme sorunu yoktur.

## <a name="training-and-experimentation-issues"></a>Eğitim ve deney konuları

### <a name="metric-document-is-too-large"></a>Metrik Belge çok büyük
Azure Machine Learning, bir eğitim çalışmasından aynı anda günlüğe kaydedilebilen metrik nesnelerin boyutuyla ilgili dahili sınırlamalara sahiptir. Liste değeri olan bir metrik günlüğe kaydolurken "Metrik Belge çok büyüktür" hatasıyla karşılaşırsanız, listeyi örneğin daha küçük parçalara bölmeyi deneyin:

```python
run.log_list("my metric name", my_metric[:N])
run.log_list("my metric name", my_metric[N:])
```

Azure ML, dahili olarak, aynı metrik ada sahip blokları bitişik bir listeye dönüştürür.

### <a name="moduleerrors-no-module-named"></a>Modül Hataları (İsimli modül yok)
Azure ML'de deneme gönderirken Modül Hataları ile karşınıza çalışıyorsanız, bu, eğitim komut dosyasının bir paketin yüklenmesini beklediği ancak eklenmediğini anlamına gelir. Paket adını sağladıktan sonra, Azure ML paketi eğitim çalışmanız için kullanılan ortama yükler. 

Denemeler göndermek için [Tahminciler](concept-azure-machine-learning-architecture.md#estimators) kullanıyorsanız, paketi yüklemek istediğiniz `pip_packages` `conda_packages` kaynaktan bağlı olarak tahmincide bir paket adı veya parametre belirtebilirsiniz. Ayrıca, tüm bağımlılıklarınızı kullanarak `conda_dependencies_file`bir yml dosyası belirtebilir veya parametre kullanarak `pip_requirements_file` bir txt dosyasındaki tüm pip gereksinimlerinizi listeleyebilirsiniz. Tahminci tarafından kullanılan varsayılan görüntüyü geçersiz kılmak istediğiniz kendi Azure ML Ortamı nesneniz varsa, `environment` bu ortamı tahminci oluşturucunun parametresi aracılığıyla belirtebilirsiniz.

Azure ML ayrıca Tensorflow, PyTorch, Chainer ve SKLearn için çerçeveye özgü tahminciler de sağlar. Bu tahmincilerin kullanılması, temel çerçeve bağımlılıklarının eğitim için kullanılan ortamda sizin adınıza yüklendiğinden emin olacaktır. Yukarıda açıklandığı gibi ek bağımlılıkları belirtme seçeneğiniz var. 
 
Azure ML muhafaza docker görüntüleri ve içeriği [AzureML Kapsayıcılar](https://github.com/Azure/AzureML-Containers)görülebilir.
Çerçeveye özgü bağımlılıklar ilgili çerçeve belgelerinde listelenir - [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py#remarks), [PyTorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py#remarks), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py#remarks), [SKLearn](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py#remarks).

> [!Note]
> Belirli bir paketin Azure ML'ye eklenecek kadar yaygın olduğunu düşünüyorsanız, lütfen [AzureML Kapsayıcılarında](https://github.com/Azure/AzureML-Containers)bir GitHub sorunu gündeme getirin. 
 
### <a name="nameerror-name-not-defined-attributeerror-object-has-no-attribute"></a>NameError (Ad tanımlanmadı), Öznitelik Hatası (Nesnenin özniteliği yoktur)
Bu özel durum, eğitim komut dosyalarınızdan gelmelidir. Tanımlanmayan belirli ad veya öznitelik hatası hakkında daha fazla bilgi almak için Azure portalındaki günlük dosyalarına bakabilirsiniz. SDK'dan hata iletisine bakmak için kullanabilirsiniz. `run.get_details()` Bu, çalıştırmanız için oluşturulan tüm günlük dosyalarını da listeler. Lütfen eğitim komut dosyanıza bir göz attıktan ve çalışmanızı yeniden göndermeden önce hatayı düzeltin. 

### <a name="horovod-has-been-shut-down"></a>Horovod kapatıldı.
Çoğu durumda "İptal Hatası: Horovod kapatıldı" ile karşılaşırsanız, bu özel durum Horovod'un kapanmasına neden olan işlemlerden birinde temel bir özel durum olduğu anlamına gelir. MPI işindeki her sıralama, Azure ML'de kendi özel günlük dosyasını alır. Bu günlükler `70_driver_logs`adlandırılmış. Dağıtılmış eğitim durumunda, günlük adlarının daha `_rank` kolay günlükleri ayırt etmek için suffixed vardır. Horovod'un kapanmasına neden olan tam hatayı bulmak için `Traceback` tüm günlük dosyalarını inceleyin ve driver_log dosyalarının sonuna bakın. Bu dosyalardan biri size gerçek temel özel durum verecektir. 

### <a name="sr-iov-availability-on-ncv3-machines-in-amlcompute-for-distributed-training"></a>Dağıtılmış eğitim için AmlCompute'daki NCv3 makinelerinde SR-IOV kullanılabilirliği
Azure Compute, müşterilerin Azure ML'nin yönetilen bilgi işlem teklifiyle (AmlCompute) yararlanabileceği NCv3 makinelerinin [SR-IOV yükseltmesini](https://azure.microsoft.com/updates/sriov-availability-on-ncv3-virtual-machines-sku/) kullanıma sunuyor. Güncelleştirmeler, özellikle derin öğrenme için geliştirilmiş çok düğümlü dağıtılmış eğitim performansı için tüm MPI yığınının ve Infiniband RDMA ağının kullanımını sağlayacaktır.

Bölgeniz için desteğin ne zaman dağıtılacağını görmek için [güncelleştirme zamanlamasını](https://azure.microsoft.com/updates/sr-iov-availability-schedule-on-ncv3-virtual-machines-sku/) görüntüleyin.

### <a name="run-or-experiment-deletion"></a>Çalıştırma veya deneme silme
Denemeler, [Deneme.arşiv](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#archive--) yöntemi kullanılarak veya Azure Machine Learning stüdyosu istemcisindeki Deneme sekmesi görünümünden "Arşivle deneme" düğmesi aracılığıyla arşivlenebilir. Bu eylem denemeyi liste sorgularından ve görünümlerinden gizler, ancak silmez.

Tek tek denemelerin veya çalıştırmaların kalıcı olarak silinmesi şu anda desteklenmez. Çalışma alanı varlıklarını silme hakkında daha fazla bilgi için, [Machine Learning hizmeti çalışma alanı verilerinizi dışa aktarın veya silin.](how-to-export-delete-data.md)

## <a name="azure-machine-learning-compute-issues"></a>Azure Machine Learning İşlem sorunları
Azure Machine Learning Compute (AmlCompute) kullanımıyla ilgili bilinen sorunlar.

### <a name="trouble-creating-amlcompute"></a>AmlCompute oluşturma da sorun

Azure Machine Learning çalışma alanını Azure portalından oluşturan bazı kullanıcıların, GA sürümünden önce bu çalışma alanında AmlCompute oluşturamama olasılığı nadirdir. Hizmete karşı bir destek isteği oluşturabilir veya portal veya SDK aracılığıyla kendinizi hemen kaldırmak için yeni bir çalışma alanı oluşturabilirsiniz.

### <a name="outage-sr-iov-upgrade-to-ncv3-machines-in-amlcompute"></a>Kesinti: AmlCompute NCv3 makineleri SR-IOV yükseltme

Azure Compute, Kasım 2019 başlarından itibaren tüm MPI uygulamalarını ve sürümlerini ve InfiniBand donanımlı sanal makineler için RDMA fiillerini desteklemek üzere NCv3 SUS'ları güncelleyecek. Bu kısa bir kesinti gerektirir - [SR-IOV yükseltme hakkında daha fazla bilgi edinin.](https://azure.microsoft.com/updates/sriov-availability-on-ncv3-virtual-machines-sku)

Azure Machine Learning'in yönetilen bilgi işlem teklifinin (AmlCompute) bir müşterisi olarak, şu anda herhangi bir değişiklik yapmanız gerekmez. [Güncelleştirme zamanlamasına](https://azure.microsoft.com/updates/sr-iov-availability-schedule-on-ncv3-virtual-machines-sku) bağlı olarak, eğitiminizde kısa bir mola planlamanız gerekir. Hizmet, küme düğümlerinizdeki VM görüntülerini güncelleme ve kümenizi otomatik olarak ölçeklendirme sorumluluğunu üstlenir. Yükseltme tamamlandıktan sonra daha yüksek InfiniBand bant genişliği, daha düşük gecikme süreleri ve daha iyi dağıtılmış uygulama performansı almanın yanı sıra diğer tüm MPI dağıtımlarını (Pytorch'lu OpenMPI gibi) kullanabilirsiniz.

## <a name="azure-machine-learning-designer-issues"></a>Azure Machine Learning tasarımcı sorunları

Tasarımcı ile bilinen sorunlar.

### <a name="long-compute-preparation-time"></a>Uzun işlem hazırlama süresi

Yeni bir işlem oluşturun veya ayrılan hesaplama yı çağrıştırın, birkaç dakika hatta daha uzun olabilir. Ekip optimizasyon için çalışıyor.


### <a name="cannot-run-an-experiment-only-contains-a-dataset"></a>Deneme çalıştırılamıyor yalnızca bir veri kümesi içeriyor 

Bir denemeyi çalıştırmak isteyebilirsiniz, yalnızca veri kümesini görselleştirmek için veri kümesi içerir. Ancak, denemeçalıştırmak için izin verilmez yalnızca bugün veri kümesi içerir. Bu sorunu aktif olarak çözeriz.
 
Düzeltmeden önce, veri kümesini herhangi bir veri dönüştürme modülüne bağlayabilir (Veri Kümesi'ndeki Sütunları Seçin, Meta Verileri Edin, Verileri Böl vb.) ve denemeyi çalıştırabilirsiniz. Ardından veri kümesini görselleştirebilirsiniz. 

Aşağıdaki resim nasıl ![gösterir: visulize-veri](./media/resource-known-issues/aml-visualize-data.png)

## <a name="image-building-failure"></a>Görüntü oluşturma hatası

Web hizmetini dağıtırken görüntü oluşturma hatası. Geçici çözüm, görüntü yapılandırması için Conda dosyasına pip bağımlılığı olarak "pynacl==1.2.1" eklemektir.

## <a name="deployment-failure"></a>Dağıtım hatası

Gözlemlerseniz, `['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>`dağıtımınızda kullanılan VM'ler için SKU'yu daha fazla belleğe sahip bir sku ile değiştirin.

## <a name="fpgas"></a>FPGA'lar

FPGA kotası için talepte bulunup onaylanana kadar FPGA'larda modeller dağıtamazsınız. Erişim isteğinde bulunmak için kota istek formunu doldurun:https://aka.ms/aml-real-time-ai

## <a name="automated-machine-learning"></a>Otomatik makine öğrenimi

Tensor Flow Automated machine learning şu anda tensor akış sürüm 1.13'u desteklememektedir. Bu sürümün yüklenmesi paket bağımlılıklarının çalışmayı durdurmasına neden olur. Bu sorunu ileride piyasaya sürülecek şekilde gidermek için çalışıyoruz.

### <a name="experiment-charts"></a>Deney Grafikleri

Otomatik ML deneme yinelemelerinde gösterilen ikili sınıflandırma grafikleri (hassas geri çağırma, ROC, kazanç eğrisi vb.) 4/12'den beri kullanıcı arabiriminde doğru şekilde işlenmemektedir. Grafik çizimleri şu anda daha iyi performans gösteren modellerin daha düşük sonuçlarla gösterildiği ters sonuçlar gösteriyor. Bir karar soruşturma altında.

## <a name="datasets-and-data-preparation"></a>Veri Kümeleri ve Veri Hazırlama

Bunlar Azure Machine Learning Datasets için bilinen sorunlardır.

### <a name="typeerror-filenotfound-no-such-file-or-directory"></a>TypeError: FileNotFound: Böyle bir dosya veya dizin yok

Sağladığınız dosya yolu dosyanın bulunduğu yerde değilse, bu hata oluşur. Dosyaya başvurma şeklinizin, veri kümenizi bilgi işlem hedefinize monte ettiğiniz yerle tutarlı olduğundan emin olmanız gerekir. Deterministik bir durum sağlamak için, bir veri kümesini bir bilgi işlem hedefine monte ederken soyut yolu kullanmanızı öneririz. Örneğin, aşağıdaki kodda veri kümesini bilgi işlem hedefinin dosya sisteminin kökü `/tmp`ne söktürür, . 

```python
# Note the leading / in '/tmp/dataset'
script_params = {
    '--data-folder': dset.as_named_input('dogscats_train').as_mount('/tmp/dataset'),
} 
```

Önde gelen ileri eğik çizgiyi '/' eklemezseniz, veri kümesinin nereye monte `/mnt/batch/.../tmp/dataset` edilip atılmasını istediğinizi belirtmek için bilgi işlem hedefinde örneğin çalışma dizinini öne eklemeniz gerekir.

### <a name="fail-to-read-parquet-file-from-http-or-adls-gen-2"></a>HTTP veya ADLS Gen 2'den Parke dosyasını okumayı başaramaz

AzureML DataPrep SDK sürüm 1.1.25'te, HTTP veya ADLS Gen 2'den Parke dosyalarını okuyarak veri kümesi oluştururken hataya neden olan bilinen bir sorun vardır. Bu ile `Cannot seek once reading started.`başarısız olur . Bu sorunu gidermek için `azureml-dataprep` lütfen 1.1.26'dan yüksek bir sürüme yükseltin veya 1.1.24'ten daha düşük bir sürüme düşürün.

```python
pip install --upgrade azureml-dataprep
```

### <a name="typeerror-mount-got-an-unexpected-keyword-argument-invocation_id"></a>TypeError: mount() beklenmeyen bir anahtar kelime bağımsız değişkeni 'invocation_id' var

Bu hata, arasında `azureml-core` uyumsuz bir sürümü `azureml-dataprep`varsa oluşur . Bu hatayı görürseniz, `azureml-dataprep` paketi daha yeni bir sürüme yükseltin (1.1.29'dan büyük veya eşit).

```python
pip install --upgrade azureml-dataprep
```

## <a name="databricks"></a>Databricks

Databricks ve Azure Machine Learning sorunları.

### <a name="failure-when-installing-packages"></a>Paketleri yüklerken hata

Azure Machine Learning SDK yüklemesi, daha fazla paket yüklendiğinde Azure Databricks'te başarısız olur. Bazı paketler, `psutil`örneğin, çakışmalara neden olabilir. Yükleme hatalarını önlemek için kitaplık sürümünü dondurarak paketleri yükleyin. Bu sorun Azure Machine Learning SDK ile değil, Databricks ile ilgilidir. Bu sorunu diğer kitaplıklarla da yaşayabilirsiniz. Örnek:

```python
psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
```

Alternatif olarak, Python kitaplıklarıyla ilgili yükleme sorunlarıyla karşı karşıya kalırsanız init komut dosyalarını kullanabilirsiniz. Bu yaklaşım resmi olarak desteklenmez. Daha fazla bilgi için, [Küme kapsamına giren init komut dosyalarına](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts)bakın.

### <a name="cancel-an-automated-machine-learning-run"></a>Otomatik makine öğrenimi çalışmasını iptal etme

Azure Databricks'te otomatik makine öğrenimi özelliklerini kullandığınızda, bir çalışmayı iptal edip yeni bir deneme çalışması başlattığınızda Azure Databricks kümenizi yeniden başlatın.

### <a name="10-iterations-for-automated-machine-learning"></a>otomatik makine öğrenimi için >10 yineleme

Otomatik makine öğrenimi ayarlarında, çalıştırmayı gönderdiğiniz `show_output` `False` zamana ayarlanmış 10'dan fazla yinelemeniz varsa.

### <a name="widget-for-the-azure-machine-learning-sdk-and-automated-machine-learning"></a>Azure Machine Learning SDK ve otomatik makine öğrenimi için widget

Azure Machine Learning SDK widget'ı, dizüstü bilgisayarlar HTML widget'larını ayrıştıramadığı için Databricks dizüstü bilgisayarında desteklenmez. Azure Databricks not defteri hücrenizde bu Python kodunu kullanarak portaldaki widget'ı görüntüleyebilirsiniz:

```
displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
```

### <a name="import-error-cannot-import-name-timedelta-from-pandas_libstslibs"></a>Alma hatası: 'pandas._libs.tslibs' adını 'Timedelta' olarak içe aktaramıyor

Otomatik makine öğrenimini kullandığınızda bu hatayı görürseniz, not defterinizde aşağıdaki iki satırı çalıştırın:
```
%sh rm -rf /databricks/python/lib/python3.7/site-packages/pandas-0.23.4.dist-info /databricks/python/lib/python3.7/site-packages/pandas
%sh /databricks/python/bin/pip install pandas==0.23.4
```

### <a name="import-error-no-module-named-pandascoreindexes"></a>Alma hatası: 'pandas.core.indexes' adlı modül yok

Otomatik makine öğrenimi kullanırken bu hatayı görürseniz:

1. Azure Veri Tuğlaları kümenize iki paket yüklemek için bu komutu çalıştırın:

   ```bash
   scikit-learn==0.19.1
   pandas==0.22.0
   ```

1. Kümeyi ayırın ve not defterinize yeniden takın.

Bu adımlar sorunu çözmezse, kümeyi yeniden başlatmayı deneyin.

### <a name="failtosendfeather"></a>FailtoSendFeather

Azure Veri `FailToSendFeather` Tuğlaları kümesinde veri okurken bir hata görürseniz, aşağıdaki çözümlere bakın:

* Paketi `azureml-sdk[automl]` en son sürüme yükseltin.
* Sürüm `azureml-dataprep` 1.1.8 veya üzeri ekleyin.
* Sürüm `pyarrow` 0.11 veya üzeri ekleyin.

## <a name="azure-portal"></a>Azure portalında

Çalışma alanınızı Doğrudan SDK'dan veya portaldan gelen bir paylaşım bağlantısından görüntülemek için giderseniz, uzantıdaki abonelik bilgilerini içeren normal Genel Bakış sayfasını görüntüleyemezsiniz. Ayrıca başka bir çalışma alanına geçiş yapamayacaktır. Başka bir çalışma alanı görüntülemeniz gerekiyorsa, geçici çözüm doğrudan [Azure Machine Learning stüdyosuna](https://ml.azure.com) gidip çalışma alanı adını aramaktır.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

Bazen yardım isterken tanılama bilgileri sağlayabilirseniz yararlı olabilir. Bazı günlükleri görmek için [Azure Machine Learning stüdyonuzu](https://ml.azure.com) ziyaret edin ve çalışma alanınıza gidin ve **Çalışma Alanı > Deneme > > Günlükleri Çalıştır'ı**seçin.  

> [!NOTE]
> Azure Machine Learning, eğitim sırasında automl veya eğitim işini çalıştıran Docker konteyneri gibi çeşitli kaynaklardan gelen bilgileri günlüğe kaydeder. Bu günlüklerin çoğu belgelenmez. Sorunlarla karşılaşır ve Microsoft desteğine başvurursanız, sorun giderme sırasında bu günlükleri kullanabilirler.

## <a name="activity-logs"></a>Etkinlik günlükleri

Azure Machine Learning çalışma alanındaki bazı eylemler, Bilgileri __Etkinlik günlüğüne__günlüğe kaydetmez. Örneğin, bir eğitim çalışması başlatma veya bir modeli kaydetme.

Bu eylemlerden bazıları çalışma alanınızın __Etkinlikler__ alanında görünür, ancak etkinliği kimin başlattığını göstermez.

## <a name="resource-quotas"></a>Kaynak kotaları

Azure Machine Learning ile çalışırken karşılaşabileceğiniz [kaynak kotaları](how-to-manage-quotas.md) hakkında bilgi edinin.

## <a name="authentication-errors"></a>Kimlik Doğrulama hataları

Uzak bir iş bir işlem hedefi üzerinde bir yönetim işlemi gerçekleştirirseniz, aşağıdaki hatalardan birini alırsınız:

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

Örneğin, uzaktan yürütme için gönderilen bir ML Ardışık Hatonu'ndan bir işlem hedefi oluşturmaya veya eklemeye çalışırsanız bir hata alırsınız.

## <a name="overloaded-azurefile-storage"></a>Aşırı yüklenmiş AzureFile depolama

Bir hata `Unable to upload project files to working directory in AzureFile because the storage is overloaded`alırsanız, aşağıdaki geçici çözümlerini uygulayın.

Veri aktarımı gibi diğer iş yükleri için dosya paylaşımı kullanıyorsanız, sorun göndermek için dosya paylaşımının ücretsiz olması için öneri blobs kullanmaktır. İş yükünü iki farklı çalışma alanı arasında da bölebilirsiniz.

## <a name="webservices-in-azure-kubernetes-service-failures"></a>Azure Kubernetes Hizmet hatalarında Web hizmetleri

Azure Kubernetes Hizmeti'ndeki birçok web hizmeti hatası, `kubectl`kümeye bağlayarak debubaşabilir. Bir Azure `kubeconfig.json` Kubernetes Hizmet Kümesi'ni çalıştırarak alabilirsiniz

```azurecli-interactive
az aks get-credentials -g <rg> -n <aks cluster name>
```

## <a name="updating-azure-machine-learning-components-in-aks-cluster"></a>AKS kümesinde Azure Machine Learning bileşenlerini güncelleme

Azure Kubernetes Hizmet kümesinde yüklü olan Azure Machine Learning bileşenlerine yapılan güncelleştirmeler el ile uygulanmalıdır. 

Bu güncelleştirmeleri, kümeyi Azure Machine Learning çalışma alanından ayırıp ardından kümeyi çalışma alanına yeniden ekleyerek uygulayabilirsiniz. Kümede SSL etkinse, kümeyi yeniden bağlarken SSL sertifikasını ve özel anahtarı sağlamanız gerekir. 

```python
compute_target = ComputeTarget(workspace=ws, name=clusterWorkspaceName)
compute_target.detach()
compute_target.wait_for_completion(show_output=True)

attach_config = AksCompute.attach_configuration(resource_group=resourceGroup, cluster_name=kubernetesClusterName)

## If SSL is enabled.
attach_config.enable_ssl(
    ssl_cert_pem_file="cert.pem",
    ssl_key_pem_file="key.pem",
    ssl_cname=sslCname)

attach_config.validate_configuration()

compute_target = ComputeTarget.attach(workspace=ws, name=args.clusterWorkspaceName, attach_configuration=attach_config)
compute_target.wait_for_completion(show_output=True)
```

Artık SSL sertifikanız ve özel anahtarınız yoksa veya Azure Machine Learning tarafından oluşturulan bir sertifika kullanıyorsanız, kümeyi `kubectl` `azuremlfessl`ayırmadan önce gizliyi kullanarak kümeye bağlanıp alarak dosyaları alabilirsiniz.

```bash
kubectl get secret/azuremlfessl -o yaml
```

>[!Note]
>Kubernetes sırları base-64 kodlanmış formatta saklar. Temel-64'e vermeden önce `cert.pem` sırların ve `key.pem` bileşenlerinin şifresini `attach_config.enable_ssl`çözmeniz gerekir. 

## <a name="labeling-projects-issues"></a>Proje sorunlarını etiketleme

Proje etiketleme ile bilinen sorunlar.

### <a name="only-datasets-created-on-blob-datastores-can-be-used"></a>Yalnızca blob veri depolarında oluşturulan veri kümeleri kullanılabilir

Bu, geçerli sürümün bilinen bir sınırlamasıdır.

### <a name="after-creation-the-project-shows-initializing-for-a-long-time"></a>Oluşturulduktan sonra, proje uzun bir süre için "Başlatma" gösterir

Sayfayı el ile yenileyin. Başlatma saniyede yaklaşık 20 veri noktasında devam etmelidir. Otomatik yenileme eksikliği bilinen bir konudur. 

### <a name="when-reviewing-images-newly-labeled-images-are-not-shown"></a>Görüntüleri gözden geçirirken, yeni etiketlenen görüntüler gösterilmez

Etiketli tüm görüntüleri yüklemek için **İlk** düğmesini seçin. **İlk** düğme sizi listenin önüne geri götürür, ancak etiketli tüm verileri yükler.

### <a name="pressing-esc-key-while-labeling-for-object-detection-creates-a-zero-size-label-on-the-top-left-corner-submitting-labels-in-this-state-fails"></a>Nesne algılama için etiketleme yaparken Esc tuşuna basıldığında sol üst köşede sıfır boyutu etiketi oluşturulur. Bu durumda etiket gönderme başarısız olur.

Yanındaki çapraz işareti tıklatarak etiketi silin.

## <a name="moving-the-workspace"></a>Çalışma alanını taşıma

> [!WARNING]
> Azure Machine Learning çalışma alanınızı farklı bir aboneye taşımak veya sahip olan aboneliğiyeni bir kiracıya taşımak desteklenmez. Bunu yapmak hatalara neden olabilir.
