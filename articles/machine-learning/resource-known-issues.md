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
ms.date: 03/31/2020
ms.openlocfilehash: 2db7a25f3f463e9210544354395c9d33a75f633c
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619371"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning"></a>Bilinen sorunlar ve Azure Machine Learning sorun giderme

Bu makale, Azure Machine Learning kullanırken karşılaşabileceğiniz hataları veya hataları bulmanıza ve düzeltmenize yardımcı olur.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

Bazen yardım isterken tanılama bilgileri sağlayabilirseniz yararlı olabilir. Bazı günlükleri görmek için: 
1. [Azure Machine Learning stüdyosuna](https://ml.azure.com)katılın. 
1. Sol tarafta, **Deney'i** seçin 
1. Bir deneme seçin.
1. Bir çalıştırma seçin.
1. Üstte Çıktı **+ günlükleri**seçin.

> [!NOTE]
> Azure Machine Learning, eğitim sırasında automl veya eğitim işini çalıştıran Docker konteyneri gibi çeşitli kaynaklardan gelen bilgileri günlüğe kaydeder. Bu günlüklerin çoğu belgelenmez. Sorunlarla karşılaşır ve Microsoft desteğine başvurursanız, sorun giderme sırasında bu günlükleri kullanabilirler.


## <a name="resource-quotas"></a>Kaynak kotaları

Azure Machine Learning ile çalışırken karşılaşabileceğiniz [kaynak kotaları](how-to-manage-quotas.md) hakkında bilgi edinin.

## <a name="installation-and-import"></a>Kurulum ve alma

* **Hata iletisi: 'PyYAML' kaldıramıyor**

    Python için Azure Machine Learning SDK: `distutils` PyYAML yüklü bir projedir. Bu nedenle, kısmi bir kaldırma varsa hangi dosyaların ona ait olduğunu doğru bir şekilde belirleyemiyoruz. Bu hatayı yoksayarak SDK'yı yüklemeye devam etmek için şunları kullanın:
    
    ```Python
    pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
    ```

* **Paketleri yüklerken veri tuğlaları hatası**

    Azure Machine Learning SDK yüklemesi, daha fazla paket yüklendiğinde Azure Databricks'te başarısız olur. Bazı paketler, `psutil`örneğin, çakışmalara neden olabilir. Yükleme hatalarını önlemek için kitaplık sürümünü dondurarak paketleri yükleyin. Bu sorun Azure Machine Learning SDK ile değil, Databricks ile ilgilidir. Bu sorunu diğer kitaplıklarla da yaşayabilirsiniz. Örnek:
    
    ```python
    psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
    ```

    Alternatif olarak, Python kitaplıklarıyla ilgili yükleme sorunlarıyla karşı karşıya kalırsanız init komut dosyalarını kullanabilirsiniz. Bu yaklaşım resmi olarak desteklenmez. Daha fazla bilgi için, [Küme kapsamına giren init komut dosyalarına](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts)bakın.

* **Databricks alma hatası: 'pandas._libs.tslibs' adını 'Timedelta' olarak içe aktaramıyor:** Otomatik makine öğrenimi kullandığınızda bu hatayı görürseniz, not defterinizde aşağıdaki iki satırı çalıştırın:
    ```
    %sh rm -rf /databricks/python/lib/python3.7/site-packages/pandas-0.23.4.dist-info /databricks/python/lib/python3.7/site-packages/pandas
    %sh /databricks/python/bin/pip install pandas==0.23.4
    ```

* **Databricks alma hatası: 'pandas.core.indexes' adlı modül yok:** Otomatik makine öğrenimini kullandığınızda bu hatayı görürseniz:

    1. Azure Veri Tuğlaları kümenize iki paket yüklemek için bu komutu çalıştırın:
    
       ```bash
       scikit-learn==0.19.1
       pandas==0.22.0
       ```
    
    1. Kümeyi ayırın ve not defterinize yeniden takın.
    
    Bu adımlar sorunu çözmezse, kümeyi yeniden başlatmayı deneyin.

* **Databricks FailToSendFeather**: Azure `FailToSendFeather` Databricks kümesindeki verileri okurken bir hata görürseniz, aşağıdaki çözümlere bakın:
    
    * Paketi `azureml-sdk[automl]` en son sürüme yükseltin.
    * Sürüm `azureml-dataprep` 1.1.8 veya üzeri ekleyin.
    * Sürüm `pyarrow` 0.11 veya üzeri ekleyin.

## <a name="create-and-manage-workspaces"></a>Çalışma alanları oluşturma ve yönetme

> [!WARNING]
> Azure Machine Learning çalışma alanınızı farklı bir aboneye taşımak veya sahip olan aboneliğiyeni bir kiracıya taşımak desteklenmez. Bunu yapmak hatalara neden olabilir.

* **Azure portalı**: Çalışma alanınızı Doğrudan SDK'dan veya portaldan gelen bir paylaşım bağlantısından görüntülemek için giderseniz, uzantıdaki abonelik bilgilerini içeren normal **Genel Bakış** sayfasını görüntüleyemezsiniz. Ayrıca başka bir çalışma alanına geçiş yapamayacaktır. Başka bir çalışma alanı görüntülemeniz gerekiyorsa, doğrudan [Azure Machine Learning stüdyosuna](https://ml.azure.com) gidin ve çalışma alanı adını arayın.

## <a name="set-up-your-environment"></a>Ortamınızı ayarlama

* **AmlCompute oluşturmada sorun**: Azure Machine Learning çalışma alanını Azure portalından oluşturan bazı kullanıcıların, GA sürümünden önce bu çalışma alanında AmlCompute oluşturamayabilme olasılığı nadirdir. Hizmete karşı bir destek isteği oluşturabilir veya portal veya SDK aracılığıyla kendinizi hemen kaldırmak için yeni bir çalışma alanı oluşturabilirsiniz.

## <a name="work-with-data"></a>Verilerle çalışma

### <a name="overloaded-azurefile-storage"></a>Aşırı yüklenmiş AzureFile depolama

Bir hata `Unable to upload project files to working directory in AzureFile because the storage is overloaded`alırsanız, aşağıdaki geçici çözümlerini uygulayın.

Veri aktarımı gibi diğer iş yükleri için dosya paylaşımı kullanıyorsanız, sorun göndermek için dosya paylaşımının ücretsiz olması için öneri blobs kullanmaktır. İş yükünü iki farklı çalışma alanı arasında da bölebilirsiniz.

### <a name="passing-data-as-input"></a>Verileri girdi olarak aktarma

*  **TypeError: FileNotFound: Böyle bir dosya veya dizin**yok : Sağladığınız dosya yolu dosyanın bulunduğu yerde değilse bu hata oluşur. Dosyaya başvurma şeklinizin, veri kümenizi bilgi işlem hedefinize monte ettiğiniz yerle tutarlı olduğundan emin olmanız gerekir. Deterministik bir durum sağlamak için, bir veri kümesini bir bilgi işlem hedefine monte ederken soyut yolu kullanmanızı öneririz. Örneğin, aşağıdaki kodda veri kümesini bilgi işlem hedefinin dosya sisteminin kökü `/tmp`ne söktürür, . 
    
    ```python
    # Note the leading / in '/tmp/dataset'
    script_params = {
        '--data-folder': dset.as_named_input('dogscats_train').as_mount('/tmp/dataset'),
    } 
    ```

    Önde gelen ileri eğik çizgiyi '/' eklemezseniz, veri kümesinin nereye monte `/mnt/batch/.../tmp/dataset` edilip atılmasını istediğinizi belirtmek için bilgi işlem hedefinde örneğin çalışma dizinini öne eklemeniz gerekir.

### <a name="data-labeling-projects"></a>Veri etiketleme projeleri

|Sorun  |Çözüm  |
|---------|---------|
|Yalnızca blob veri depolarında oluşturulan veri kümeleri kullanılabilir     |  bu, geçerli sürümün bilinen bir sınırlamasıdır.       |
|Oluşturulduktan sonra, proje uzun bir süre için "Başlatma" gösterir     | Sayfayı el ile yenileyin. Başlatma saniyede yaklaşık 20 veri noktasında devam etmelidir. Otomatik yenileme eksikliği bilinen bir konudur.         |
|Görüntüleri gözden geçirirken, yeni etiketlenen görüntüler gösterilmez     |   Etiketli tüm görüntüleri yüklemek için **İlk** düğmesini seçin. **İlk** düğme sizi listenin önüne geri götürür, ancak etiketli tüm verileri yükler.      |
|Nesne algılama için etiketleme yaparken Esc tuşuna basıldığında sol üst köşede sıfır boyutu etiketi oluşturulur. Bu durumda etiket gönderme başarısız olur.     |   Yanındaki çapraz işareti tıklatarak etiketi silin.  |

## <a name="azure-machine-learning-designer"></a>Azure Machine Learning tasarımcısı

Bilinen sorunlar:

* **Uzun işlem hazırlama süresi**: Bir işlem hedefine ilk bağlandığınızda veya oluşturduğunuzda birkaç dakika veya daha uzun olabilir. 

## <a name="train-models"></a>Modelleri eğitme

* **Modül Hataları (No module named)**: Azure ML'de deneme gönderirken Modül Hataları ile karşınıza çalışıyorsanız, bu, eğitim komut dosyasının bir paketin yüklenmesini beklediği ancak eklenmediğini anlamına gelir. Paket adını sağladıktan sonra, Azure ML paketi eğitim çalışmanız için kullanılan ortama yükler. 

    Denemeler göndermek için [Tahminciler](concept-azure-machine-learning-architecture.md#estimators) kullanıyorsanız, paketi yüklemek istediğiniz `pip_packages` `conda_packages` kaynaktan bağlı olarak tahmincide bir paket adı veya parametre belirtebilirsiniz. Ayrıca, tüm bağımlılıklarınızı kullanarak `conda_dependencies_file`bir yml dosyası belirtebilir veya parametre kullanarak `pip_requirements_file` bir txt dosyasındaki tüm pip gereksinimlerinizi listeleyebilirsiniz. Tahminci tarafından kullanılan varsayılan görüntüyü geçersiz kılmak istediğiniz kendi Azure ML Ortamı nesneniz varsa, `environment` bu ortamı tahminci oluşturucunun parametresi aracılığıyla belirtebilirsiniz.

    Azure ML ayrıca Tensorflow, PyTorch, Chainer ve SKLearn için çerçeveye özgü tahminciler de sağlar. Bu tahmincilerin kullanılması, temel çerçeve bağımlılıklarının eğitim için kullanılan ortamda sizin adınıza yüklendiğinden emin olacaktır. Yukarıda açıklandığı gibi ek bağımlılıkları belirtme seçeneğiniz var. 
 
    Azure ML muhafaza docker görüntüleri ve içeriği [AzureML Kapsayıcılar](https://github.com/Azure/AzureML-Containers)görülebilir.
    Çerçeveye özgü bağımlılıklar ilgili çerçeve belgelerinde listelenir - [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py#remarks), [PyTorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py#remarks), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py#remarks), [SKLearn](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py#remarks).

    > [!Note]
    > Belirli bir paketin Azure ML'ye eklenecek kadar yaygın olduğunu düşünüyorsanız, lütfen [AzureML Kapsayıcılarında](https://github.com/Azure/AzureML-Containers)bir GitHub sorunu gündeme getirin. 
 
* **NameError (Ad tanımlanmamış), Öznitelik Hatası (Nesnenin özniteliği yoktur)**: Bu özel durum eğitim komut dosyalarınızdan gelmelidir. Tanımlanmayan belirli ad veya öznitelik hatası hakkında daha fazla bilgi almak için Azure portalındaki günlük dosyalarına bakabilirsiniz. SDK'dan hata iletisine bakmak için kullanabilirsiniz. `run.get_details()` Bu, çalıştırmanız için oluşturulan tüm günlük dosyalarını da listeler. Lütfen eğitim komut dosyanıza bir göz attıktan ve çalışmanızı yeniden göndermeden önce hatayı düzeltin. 

* **Horovod kapatıldı**: Çoğu durumda "İptal Hatası: Horovod kapatıldı" ile karşılaşırsanız bu istisna, Horovod'un kapanmasına neden olan işlemlerden birinde temel bir istisna olduğu anlamına gelir. MPI işindeki her sıralama, Azure ML'de kendi özel günlük dosyasını alır. Bu günlükler `70_driver_logs`adlandırılmış. Dağıtılmış eğitim durumunda, günlük adlarının daha `_rank` kolay günlükleri ayırt etmek için suffixed vardır. Horovod'un kapanmasına neden olan tam hatayı bulmak için `Traceback` tüm günlük dosyalarını inceleyin ve driver_log dosyalarının sonuna bakın. Bu dosyalardan biri size gerçek temel özel durum verecektir. 

* **Çalıştır veya deneme silme**: Denemeler [Deneme.arşiv](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#archive--) yöntemi kullanılarak veya Azure Machine Learning stüdyosu istemcisindeki Deneme sekmesi görünümünden "Arşiv lemesi" düğmesi aracılığıyla arşivlenebilir. Bu eylem denemeyi liste sorgularından ve görünümlerinden gizler, ancak silmez.

    Tek tek denemelerin veya çalıştırmaların kalıcı olarak silinmesi şu anda desteklenmez. Çalışma alanı varlıklarını silme hakkında daha fazla bilgi için, [Machine Learning hizmeti çalışma alanı verilerinizi dışa aktarın veya silin.](how-to-export-delete-data.md)

* **Metrik Belge çok büyüktür**: Azure Machine Learning, bir eğitim çalıştıran bir anda günlüğe kaydedilebilen metrik nesnelerin boyutuyla ilgili dahili sınırlamalara sahiptir. Liste değeri olan bir metrik günlüğe kaydolurken "Metrik Belge çok büyüktür" hatasıyla karşılaşırsanız, listeyi örneğin daha küçük parçalara bölmeyi deneyin:

    ```python
    run.log_list("my metric name", my_metric[:N])
    run.log_list("my metric name", my_metric[N:])
    ```

    Azure ML, dahili olarak, aynı metrik ada sahip blokları bitişik bir listeye dönüştürür.

## <a name="automated-machine-learning"></a>Otomatik makine öğrenimi

* **Tensor Akışı**: Otomatik makine öğrenimi şu anda tensor akış sürüm 1.13'u desteklememektedir. Bu sürümün yüklenmesi paket bağımlılıklarının çalışmayı durdurmasına neden olur. Bu sorunu ileride piyasaya sürülecek şekilde gidermek için çalışıyoruz.

* **Deney Grafikleri**: Otomatik ML deneme yinelemelerinde gösterilen ikili sınıflandırma grafikleri (hassas hatırlama, ROC, kazanç eğrisi vb.) 4/12'den beri kullanıcı arabiriminde doğru şekilde işlenmemektedir. Grafik çizimleri şu anda daha iyi performans gösteren modellerin daha düşük sonuçlarla gösterildiği ters sonuçlar gösteriyor. Bir karar soruşturma altında.

* **Veri tuğlaları otomatik bir makine öğrenimi çalışmasını iptal eder:** Azure Databricks'te otomatik makine öğrenimi özelliklerini kullandığınızda, bir çalıştırmayı iptal eder ve yeni bir deneme çalışması başlatın, Azure Databricks kümenizi yeniden başlatın.

* **Veri tuğlaları otomatik makine öğrenimi için 10 yineleme >**: Otomatik makine öğrenimi ayarlarında, `False` çalıştırmayı gönderdiğiniz zaman ayarlanan `show_output` 10'dan fazla yinelemeniz varsa.

* **Azure Machine Learning SDK ve otomatik makine öğrenimi için Databricks widget'ı**: Azure Machine Learning SDK widget'ı, dizüstü bilgisayarlar HTML widget'larını ayrıştıramadığı için Databricks dizüstü bilgisayarında desteklenmez. Azure Databricks not defteri hücrenizde bu Python kodunu kullanarak portaldaki widget'ı görüntüleyebilirsiniz:

    ```
    displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
    ```

## <a name="deploy--serve-models"></a>Modelleri dağıtma ve sunma

Aşağıdaki hatalar için aşağıdaki eylemleri yapın:

|Hata  | Çözüm  |
|---------|---------|
|Web hizmetini dağıtırken görüntü oluşturma hatası     |  Görüntü yapılandırması için Conda dosyasına pip bağımlılığı olarak "pynacl==1.2.1" ekleme       |
|`['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>`     |   Dağıtımınızda kullanılan VM'ler için SKU'yu daha fazla belleğe sahip biriyle değiştirin. |
|FPGA hatası     |  FPGA kotası için talepte bulunup onaylanana kadar FPGA'larda modeller dağıtamazsınız. Erişim isteğinde bulunmak için kota istek formunu doldurun:https://aka.ms/aml-real-time-ai       |

### <a name="updating-azure-machine-learning-components-in-aks-cluster"></a>AKS kümesinde Azure Machine Learning bileşenlerini güncelleme

Azure Kubernetes Hizmet kümesinde yüklü olan Azure Machine Learning bileşenlerine yapılan güncelleştirmeler el ile uygulanmalıdır. 

Bu güncelleştirmeleri, kümeyi Azure Machine Learning çalışma alanından ayırıp ardından kümeyi çalışma alanına yeniden ekleyerek uygulayabilirsiniz. Kümede TLS etkinse, kümeyi yeniden bağlarken TLS/SSL sertifikasını ve özel anahtarı sağlamanız gerekir. 

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

TLS/SSL sertifikasına ve özel anahtarına artık sahip değilseniz veya Azure Machine Learning tarafından oluşturulan bir sertifika kullanıyorsanız, kümeyi ayırmadan önce gizliyi `kubectl` `azuremlfessl`kullanarak kümeye bağlanıp alarak dosyaları alabilirsiniz.

```bash
kubectl get secret/azuremlfessl -o yaml
```

>[!Note]
>Kubernetes sırları base-64 kodlanmış formatta saklar. Temel-64'e vermeden önce `cert.pem` sırların ve `key.pem` bileşenlerinin şifresini `attach_config.enable_ssl`çözmeniz gerekir. 

### <a name="webservices-in-azure-kubernetes-service-failures"></a>Azure Kubernetes Hizmet hatalarında Web hizmetleri

Azure Kubernetes Hizmeti'ndeki birçok web hizmeti hatası, `kubectl`kümeye bağlayarak debubaşabilir. Bir Azure `kubeconfig.json` Kubernetes Hizmet Kümesi'ni çalıştırarak alabilirsiniz

```azurecli-interactive
az aks get-credentials -g <rg> -n <aks cluster name>
```

## <a name="authentication-errors"></a>Kimlik Doğrulama hataları

Uzak bir iş bir işlem hedefi üzerinde bir yönetim işlemi gerçekleştirirseniz, aşağıdaki hatalardan birini alırsınız:

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

Örneğin, uzaktan yürütme için gönderilen bir ML Ardışık Hatonu'ndan bir işlem hedefi oluşturmaya veya eklemeye çalışırsanız bir hata alırsınız.
