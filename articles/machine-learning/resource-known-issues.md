---
title: Sorun giderme & bilinen sorunlar
titleSuffix: Azure Machine Learning
description: Azure Machine Learning için bilinen sorunların, geçici çözümlerin ve sorun gidermenin bir listesini alın.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 40749a80d99782a1ea84b27e68376ea2870e8eb7
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77138000"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning"></a>Bilinen sorunlar ve sorun giderme Azure Machine Learning

Bu makale, Azure Machine Learning kullanırken hataları veya hataları bulmanıza ve düzeltmenize yardımcı olur.

## <a name="outage-sr-iov-upgrade-to-ncv3-machines-in-amlcompute"></a>Kesinti: AmlCompute içindeki NCv3 makinelere SR-ıOV yükseltmesi

Azure Işlem, tüm MPı uygulamalarını ve sürümlerini desteklemek üzere NCv3 SKU 2019 'Larını ve InfiniBand ile donatılmış sanal makineler için RDMA fiillerini güncelleştirmek olacaktır. Bu, kısa bir kesinti süresi gerektirir [ve SR-IOV yükseltmesi hakkında daha fazla bilgi edinin](https://azure.microsoft.com/updates/sriov-availability-on-ncv3-virtual-machines-sku).

Azure Machine Learning yönetilen işlem sunumu (AmlCompute) müşterisi olarak, şu anda herhangi bir değişiklik yapmanız gerekmez. [Güncelleştirme zamanlaması](https://azure.microsoft.com/updates/sr-iov-availability-schedule-on-ncv3-virtual-machines-sku) temelinde, eğitiminde kısa bir kesme planlamanız gerekir. Hizmet, Küme düğümlerinizin VM görüntülerini güncelleştirmek ve kümenizi otomatik olarak ölçeklendirmek için sorumluluğu alacak. Yükseltme tamamlandıktan sonra, daha yüksek InfiniBand bant genişliği, daha düşük gecikme süreleri ve daha iyi dağıtılmış uygulama performansını elde etmek için diğer tüm MPı dağıtımlarını (Pytorch ile OpenMPI gibi) kullanabilirsiniz.

## <a name="azure-machine-learning-designer-issues"></a>Tasarımcı sorunlarını Azure Machine Learning

Tasarımcı ile ilgili bilinen sorunlar.

### <a name="long-compute-preparation-time"></a>Uzun süreli işlem hazırlık süresi

Yeni işlem oluşturun veya çağırabilir işlem zaman alır, birkaç dakika veya daha uzun olabilir. Takım en iyi duruma getirme için çalışıyor.


### <a name="cannot-run-an-experiment-only-contains-a-dataset"></a>Yalnızca bir veri kümesi içeren bir deneme çalıştırılamaz 

Bir denemeyi çalıştırmak, veri kümesini görselleştirmek için yalnızca veri kümesi içeriyor. Ancak, yalnızca bir denemeyi çalıştırmaya izin verilmez. Bu sorunu etkin bir şekilde düzelttik.
 
Bu yüklemeden önce veri kümesini herhangi bir veri dönüştürme modülüne bağlayabilirsiniz (veri kümesinde sütunları seçin, meta verileri düzenleyebilir, verileri bölebilir vb.) ve denemeyi çalıştırabilirsiniz. Daha sonra veri kümesini görselleştirebilirsiniz. 

Aşağıdaki görüntüde nasıl yapılacağı gösterilmektedir: ![visulize-Data](./media/resource-known-issues/aml-visualize-data.png)

## <a name="sdk-installation-issues"></a>SDK yükleme sorunları

**Hata iletisi: ' PyYAML ' kaldırılamıyor**

Python için Azure Machine Learning SDK: PyYAML olan yüklü distutils proje. Bu nedenle, kısmi bir kaldırma işlemi varsa, hangi dosyaların kendisine ait olduğunu doğru bir şekilde belirleyemedik. Bu hatayı yoksayma sırasında SDK'sı yüklemeye devam etmek için kullanın:

```Python
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

**Hata iletisi: `ERROR: No matching distribution found for azureml-dataprep-native`**

Anaconda 'nın Python 3.7.4 dağıtımında, azureml-SDK yüklemesini kesen bir hata vardır. Bu sorun, bu [GitHub sorunu](https://github.com/ContinuumIO/anaconda-issues/issues/11195) ile tartışılır ve bu komut kullanılarak yeni bir Conda ortamı oluşturulabilir:
```bash
conda create -n <env-name> python=3.7.3
```
Bu, 3.7.4 içinde bir Install sorunu bulunmayan Python 3.7.3 kullanarak bir Conda ortamı oluşturur.

## <a name="trouble-creating-azure-machine-learning-compute"></a>Azure Machine Learning işlem oluştururken sorun

GA sürümü önce Azure portalından, Azure Machine Learning çalışma alanı oluşturan bazı kullanıcıların bu çalışma alanında Azure Machine Learning işlem oluşturmak mümkün olmayabilir nadir bir fırsat yoktur. Bir destek isteği hizmetinde yükseltmek veya Portal veya SDK'yı kendiniz hemen engelini kaldırmak için yeni bir çalışma alanı oluşturun.

## <a name="image-building-failure"></a>Görüntü oluşturma hatası

Web hizmeti dağıtılırken hata oluşturma görüntüsü. Geçici çözüm olan eklemek için "pynacl 1.2.1 ==" Conda dosyasına görüntü yapılandırması için pip bağımlılık olarak.

## <a name="deployment-failure"></a>Dağıtım hatası

`['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>`gözlemlerseniz, dağıtımınızda kullanılan VM 'Ler için SKU 'YU daha fazla belleğe sahip olan bir şekilde değiştirin.

## <a name="fpgas"></a>FPGA'lar

İstenen ve FPGA kotası için onaylanmış kadar FPGA modellerde dağıtmayı mümkün olmayacaktır. Erişim istemek için kota isteği formunu doldurun: https://aka.ms/aml-real-time-ai

## <a name="automated-machine-learning"></a>Otomatik makine öğrenimi

Tensor Flow otomatik makine öğrenimi Şu anda Tensor Flow sürüm 1,13 ' i desteklememektedir. Bu sürümün yüklenmesi paket bağımlılıklarının çalışmayı durdurmasına neden olur. Bu sorunu gelecekte yayımlanacak bir sürümde gidermeye çalışıyoruz. 

### <a name="experiment-charts"></a>Deneme grafikleri

Otomatik ML denemesi yinelemeleriyle gösterilen ikili sınıflandırma grafikleri (duyarlık-hatırla, ROC, kazanç eğrisi vb.), 4/12 ' den beri Kullanıcı arabiriminde doğru işlenmemektedir. Grafik çizimleri Şu anda daha iyi şekilde uygulanan modellerin daha düşük sonuçlarla gösterildiği ters sonuçları gösteriyor. Bir çözüm, araştırma aşamasındadır.

## <a name="datasets-and-data-preparation"></a>Veri kümeleri ve veri hazırlama

Bunlar Azure Machine Learning veri kümeleri için bilinen sorunlardır.

### <a name="typeerror-filenotfound-no-such-file-or-directory"></a>TypeError: Fılenotfound: böyle bir dosya veya dizin yok

Sağladığınız dosya yolu dosyanın bulunduğu konumda değilse bu hata oluşur. Dosyaya başvurduğunuzdan emin olmanız gerekir. bu şekilde, veri kümenizi işlem Hedefinizdeki bağladığınız konum ile tutarlıdır. Belirleyici bir durum sağlamak için, bir veri kümesini bir işlem hedefine bağlamak için soyut yolun kullanılmasını öneririz. Örneğin, aşağıdaki kodda, veri kümesini işlem hedefinin FileSystem kökünün altına bağlamamız, `/tmp`. 

```python
# Note the leading / in '/tmp/dataset'
script_params = {
    '--data-folder': dset.as_named_input('dogscats_train').as_mount('/tmp/dataset'),
} 
```

Önde gelen eğik çizgi '/' dahil değilseniz, veri kümesinin bağlanmasını istediğiniz yeri belirtmek için işlem hedefinde `/mnt/batch/.../tmp/dataset` Örneğin, çalışma dizinine önek uygulamanız gerekir. 

### <a name="fail-to-read-parquet-file-from-http-or-adls-gen-2"></a>HTTP veya ADLS Gen 2 ' den Parquet dosyası okunamadı

AzureML, HTTP veya ADLS Gen 2 ' den Parquet dosyalarını okuyarak bir veri kümesi oluştururken hata oluşmasına neden olan AzureML veri hazırlama SDK sürümü 1.1.25 'da bilinen bir sorun vardır. `Cannot seek once reading started.`ile başarısız olur. Bu sorunu gidermeye yönelik `azureml-dataprep`, lütfen 1.1.26 'den daha yüksek bir sürüme yükseltin veya 1.1.24 'den daha düşük bir sürüme indirgemeniz gerekir.

```python
pip install --upgrade azureml-dataprep
```

### <a name="typeerror-mount-got-an-unexpected-keyword-argument-invocation_id"></a>TypeError: Mount () beklenmeyen bir anahtar sözcük bağımsız değişkeni ' invocation_id ' aldı

`azureml-core` ve `azureml-dataprep`arasında uyumsuz bir sürüme sahipseniz bu hata oluşur. Bu hatayı görürseniz, `azureml-dataprep` paketini daha yeni bir sürüme yükseltin (1.1.29 değerinden büyük veya buna eşittir).

```python
pip install --upgrade azureml-dataprep
```

## <a name="databricks"></a>Databricks

Databricks ve Azure Machine Learning sorunları.

### <a name="failure-when-installing-packages"></a>Paketler yüklenirken hata oluştu

Azure Machine Learning SDK yüklemesi, daha fazla paket yüklendiğinde Azure Databricks başarısız olur. `psutil`gibi bazı paketler çakışmalara neden olabilir. Yükleme hatalarını önlemek için, kitaplık sürümünü dondurarak paketleri yükleme. Bu sorun, Azure Machine Learning SDK 'Sı değil Databricks ile ilgilidir. Bu sorunla diğer kitaplıklarla de karşılaşabilirsiniz. Örnek:

```python
psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
```

Alternatif olarak, Python kitaplıklarıyla ilgili sorunları gidermek için Init betiklerini kullanabilirsiniz. Bu yaklaşım resmi olarak desteklenmez. Daha fazla bilgi için bkz. [küme kapsamlı init betikleri](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

### <a name="cancel-an-automated-machine-learning-run"></a>Otomatik makine öğrenimi çalıştırmasını iptal et

Azure Databricks üzerinde otomatik makine öğrenimi özellikleri kullandığınızda, bir çalıştırmayı iptal etmek ve yeni bir deneme çalıştırması başlatmak için Azure Databricks kümenizi yeniden başlatın.

### <a name="10-iterations-for-automated-machine-learning"></a>otomatik makine öğrenimi için 10 > yineleme

Otomatik makine öğrenimi ayarları ' nda, 10 ' dan fazla yinelediğiniz bir `show_output`, çalıştırmayı gönderdiğinizde `False` olarak ayarlayın.

### <a name="widget-for-the-azure-machine-learning-sdk-and-automated-machine-learning"></a>Azure Machine Learning SDK ve otomatik makine öğrenimi için pencere öğesi

Azure Machine Learning SDK pencere öğesi, bir Databricks not defterinde desteklenmez çünkü Not defterleri HTML pencere öğelerini ayrıştıramaz. Azure Databricks Not defteri hücresinizdeki bu python kodunu kullanarak portalda pencere öğesini görüntüleyebilirsiniz:

```
displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
```

### <a name="import-error-no-module-named-pandascoreindexes"></a>İçeri aktarma hatası: ' Pandas. Core. Indexes ' adlı modül yok

Otomatik makine öğrenimi kullandığınızda bu hatayı görürseniz:

1. Azure Databricks kümenize iki paket yüklemek için şu komutu çalıştırın: 

   ```
   scikit-learn==0.19.1
   pandas==0.22.0
   ```

1. Kümeyi ayırın ve Not defterinize yeniden bağlayın. 

Bu adımlar sorunu çözmezse, kümeyi yeniden başlatmayı deneyin.

### <a name="failtosendfeather"></a>Failtosendgeçiş yumuşatma

Azure Databricks kümesindeki verileri okurken `FailToSendFeather` hatası görürseniz, aşağıdaki çözümlere başvurun:

* `azureml-sdk[automl]` paketini en son sürüme yükseltin.
* `azureml-dataprep` Version 1.1.8 veya üstünü ekleyin.
* `pyarrow` sürüm 0,11 veya üstünü ekleyin.

## <a name="azure-portal"></a>Azure portalı

Doğrudan paylaşım bağlantısı SDK veya portalından çalışma alanınızda görüntülemeye giderseniz, uzantı normal genel bakış sayfası ile abonelik bilgilerini görüntülemek mümkün olmayacaktır. Siz de başka bir çalışma alanına geçmeniz mümkün olmayacaktır. Başka bir çalışma alanını görüntülemeniz gerekirse, geçici çözüm doğrudan [Azure Machine Learning Studio](https://ml.azure.com) 'ya gidip çalışma alanı adını arayacak.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

Bazen Yardım isteme, tanılama bilgilerini sağlarsanız, yararlı olabilir. Bazı günlükleri görmek için [Azure Machine Learning Studio 'yu](https://ml.azure.com) ziyaret edin ve çalışma alanınıza gidin ve **> günlüklerini çalıştırmak > deneyin > çalışma alanı**' nı seçin.  

> [!NOTE]
> Azure Machine Learning, eğitim sırasında (örneğin, oto ml) veya eğitim işini çalıştıran Docker kapsayıcısı gibi çeşitli kaynaklardan günlük bilgileri günlüğe kaydeder. Bu günlüklerin birçoğu açıklanmamıştır. Sorunlarla karşılaşırsanız ve Microsoft Destek ile iletişime geçerek, sorun giderme sırasında bu günlükleri kullanabiliyor olabilirler.

## <a name="activity-logs"></a>Etkinlik günlükleri

Azure Machine Learning çalışma alanındaki bazı eylemler __etkinlik günlüğüne__bilgi vermez. Örneğin, bir eğitimi başlatma veya bir modeli kaydetme.

Bu eylemlerden bazıları, çalışma alanınızın __Etkinlikler__ alanında görünür, ancak aktiviteyi kimin başlattığını göstermez.

## <a name="resource-quotas"></a>Kaynak kotaları

Azure Machine Learning çalışırken karşılaşabileceğiniz [kaynak kotaları](how-to-manage-quotas.md) hakkında bilgi edinin.

## <a name="authentication-errors"></a>Kimlik Doğrulama hataları

Uzak bir işten bir işlem hedefinde yönetim işlemi gerçekleştirirseniz, aşağıdaki hatalardan birini alırsınız:

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

Örneğin, uzaktan yürütme için gönderilen bir ML ardışık düzeninde bir işlem hedefi oluşturmaya veya eklemeye çalıştığınızda bir hata alırsınız.

## <a name="overloaded-azurefile-storage"></a>Aşırı yüklenmiş AzureFile depolama

`Unable to upload project files to working directory in AzureFile because the storage is overloaded`bir hata alırsanız, aşağıdaki geçici çözümleri uygulayın.

Veri aktarımı gibi diğer iş yükleri için dosya paylaşma 'yı kullanıyorsanız, dosya paylaşımının çalıştırmaları için kullanılabilmesi için blob 'ları kullanmak, bu nedenle söz konusu çalışma. İş yükünü iki farklı çalışma alanı arasında da bölebilirsiniz.

## <a name="webservices-in-azure-kubernetes-service-failures"></a>Azure Kubernetes hizmeti hatalarında WebServices 

Azure Kubernetes hizmetindeki birçok Web hizmeti hatası, `kubectl`kullanılarak kümeye bağlanarak hata ayıklanabilir. Çalıştıran bir Azure Kubernetes hizmet kümesi için `kubeconfig.json` alabilirsiniz

```bash
az aks get-credentials -g <rg> -n <aks cluster name>
```

## <a name="updating-azure-machine-learning-components-in-aks-cluster"></a>AKS kümesindeki Azure Machine Learning bileşenleri güncelleştiriliyor

Azure Kubernetes hizmet kümesinde yüklü Azure Machine Learning bileşenlere yapılan güncelleştirmeler el ile uygulanmalıdır. 

Kümeyi Azure Machine Learning çalışma alanından ayırarak ve sonra kümeyi çalışma alanına yeniden açarak bu güncelleştirmeleri uygulayabilirsiniz. Kümede SSL etkinse, kümeyi yeniden eklerken SSL sertifikasını ve özel anahtarı sağlamanız gerekir. 

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

Artık SSL sertifikasına ve özel anahtara sahip değilseniz veya Azure Machine Learning tarafından oluşturulan bir sertifika kullanıyorsanız, `kubectl` kullanarak kümeye bağlanarak ve gizli dizi `azuremlfessl`alarak, bu dosyaları kümeyi kullanımdan çıkarmadan önce alabilirsiniz.

```bash
kubectl get secret/azuremlfessl -o yaml
```

>[!Note]
>Kubernetes gizli dizileri temel-64 kodlu biçimde depolar. `attach_config.enable_ssl`'e vermeden önce gizliliklerin `cert.pem` ve `key.pem` bileşenlerinin kodunu çözmelisiniz. 64 

## <a name="recommendations-for-error-fix"></a>Hata düzeltilme önerileri
Genel gözlemye bağlı olarak, Azure ML 'deki bazı yaygın hataları gidermeye yönelik Azure ML önerileri aşağıda verilmiştir.

### <a name="metric-document-is-too-large"></a>Ölçüm belgesi çok büyük
Azure Machine Learning, bir eğitim çalıştırmasında bir kez günlüğe kaydedilebilir ölçüm nesnelerinin boyutunda iç sınırlara sahiptir. Liste değerli bir ölçümü günlüğe kaydederken "ölçüm belgesi çok büyük" hatası ile karşılaşırsanız, listeyi daha küçük parçalara bölmeyi deneyin, örneğin:

```python
run.log_list("my metric name", my_metric[:N])
run.log_list("my metric name", my_metric[N:])
```

 Dahili olarak, çalıştırma geçmişi hizmeti aynı ölçüm adına sahip blokları bitişik bir liste olarak birleştirir.

### <a name="moduleerrors-no-module-named"></a>ModuleErrors (modül adı yok)
Azure ML 'de denemeleri gönderirken ModuleErrors 'da çalıştırıyorsanız, eğitim betiğinin bir paketin yüklenmesini beklediği ancak eklenmediği anlamına gelir. Paket adı 'nı sağladığınızda, Azure ML, paketi eğitiminizi için kullanılan ortama yükler. 

Denemeleri göndermek için [estimators](concept-azure-machine-learning-architecture.md#estimators) kullanıyorsanız, paketi yüklemek istediğiniz kaynağı temel alan tahmin aracı 'da `pip_packages` veya `conda_packages` parametresi aracılığıyla bir paket adı belirtebilirsiniz. Ayrıca, `conda_dependencies_file`kullanarak tüm bağımlılıklarınızı içeren bir rivml dosyası belirtebilir veya `pip_requirements_file` parametresini kullanarak bir txt dosyasındaki tüm PIP gereksinimlerinizi listeleyebilirsiniz.

Azure ML, TensorFlow, PyTorch, Chainer ve Sköğren için çerçeveye özgü tahminler de sağlar. Bu tahmini kullanımı, Framework bağımlılıklarının eğitim için kullanılan ortamda sizin adınıza yüklü olduğundan emin olur. Yukarıda açıklandığı gibi ek bağımlılıklar belirtme seçeneğiniz vardır. 
 
Azure ML tarafından sağlanan Docker görüntüleri ve içerikleri, [AzureML kapsayıcılarında](https://github.com/Azure/AzureML-Containers)görülebilir.
Çerçeveye özgü bağımlılıklar ilgili Framework belgelerinde listelenmiştir- [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py#remarks), [pytorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py#remarks), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py#remarks), [sköğren](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py#remarks).

> [!Note]
> Belirli bir paketin Azure ML tarafından korunan görüntülere ve ortamlara eklenmek için yeterince yaygın olduğunu düşünüyorsanız, lütfen [AzureML kapsayıcılarında](https://github.com/Azure/AzureML-Containers)GitHub sorununu yükseltin. 
 
 ### <a name="nameerror-name-not-defined-attributeerror-object-has-no-attribute"></a>NameError (ad tanımlı değil), AttributeError (nesne bir özniteliğe sahip değil)
Bu özel durum, eğitim betiklerinden gelmelidir. Tanımlı bir ad veya öznitelik hatası hakkında daha fazla bilgi edinmek için, Azure portal günlük dosyalarına bakabilirsiniz. SDK 'dan, hata iletisine bakmak için `run.get_details()` kullanabilirsiniz. Bu, çalıştırma için oluşturulan tüm günlük dosyalarını da listeler. Lütfen eğitim betiğe göz atın, yeniden denemeden önce hatayı onarın. 

### <a name="horovod-is-shut-down"></a>Horovod kapatıldı
Çoğu durumda, bu özel durum horovod 'nin kapanmasının neden olduğu işlemlerden birinde temeldeki bir özel durum olduğu anlamına gelir. MPı işindeki her bir derecelendirme, Azure ML 'de özel bir günlük dosyası alır. Bu Günlükler `70_driver_logs`olarak adlandırılır. Dağıtılmış eğitim söz konusu olduğunda, günlüklerin ayırt edilmesini kolaylaştırmak için günlük adları `_rank` ile sonlardır. Horovod kapatmaya neden olan hatayı tam olarak bulmak için tüm günlük dosyalarını gözden geçirin ve driver_log dosyalarının sonundaki `Traceback` bulun. Bu dosyalardan biri size gerçek temel özel durumu verecektir. 

## <a name="labeling-projects-issues"></a>Proje sorunlarını etiketleme

Etiketleme projeleri ile ilgili bilinen sorunlar.

### <a name="only-datasets-created-on-blob-datastores-can-be-used"></a>Yalnızca blob veri depolarında oluşturulan veri kümeleri kullanılabilir

Bu, geçerli sürümün bilinen bir sınırlamasıdır. 

### <a name="after-creation-the-project-shows-initializing-for-a-long-time"></a>Oluşturulduktan sonra proje, uzun süredir "başlatılıyor" olarak gösterilir

Sayfayı el ile yenileyin. Başlatma, saniyede yaklaşık 20 veri noktasında devam etmelidir. Bu, bilinen bir sorundur. 

### <a name="when-reviewing-images-newly-labeled-images-are-not-shown"></a>Görüntüleri gözden geçirirken yeni etiketlenmiş görüntüler gösterilmez

Etiketlenmiş tüm görüntüleri yüklemek için **ilk** düğmeyi seçin. **İlk** düğme, listenin önüne geri götürür, ancak etiketlenmiş tüm verileri yükler.

### <a name="pressing-esc-key-while-labeling-for-object-detection-creates-a-zero-size-label-on-the-top-left-corner-submitting-labels-in-this-state-fails"></a>Nesne algılama için etiketleme sırasında Esc tuşuna basmak, sol üst köşede Sıfır boyutlu bir etiket oluşturur. Etiketlerin bu durumda gönderilmesi başarısız oluyor.

Yanındaki çapraz işaretine tıklayarak etiketi silin.

## <a name="run-or-experiment-deletion"></a>Çalıştırma veya deneme silme

Denemeleri, [deneme. Arşiv](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#archive--) yöntemi kullanılarak veya Azure Machine Learning Studio istemcisinde deneme sekmesi görünümünden arşivlenebilir. Bu eylem, sorgu ve görünümleri listeleme denemesini gizler, ancak silmez.

Tek tek denemeleri veya çalıştırmaları kalıcı olarak silme işlemi şu anda desteklenmiyor. Çalışma alanı varlıklarını silme hakkında daha fazla bilgi için bkz. [Machine Learning hizmeti çalışma alanı verilerinizi dışarı veya silme](how-to-export-delete-data.md).

## <a name="moving-the-workspace"></a>Çalışma alanı taşınıyor

> [!WARNING]
> Azure Machine Learning çalışma alanınızı farklı bir aboneliğe taşımak veya sahip olunan aboneliğin yeni bir kiracıya taşınması desteklenmez. Bunun yapılması hatalara neden olabilir.