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
ms.date: 03/31/2020
ms.openlocfilehash: 2760033cd66e99a7a7f6d331e03c6f98c486d286
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82231977"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning"></a>Bilinen sorunlar ve sorun giderme Azure Machine Learning

Bu makale, Azure Machine Learning kullanırken karşılaşabileceğiniz hataları veya hataları bulmanıza ve düzeltmenize yardımcı olur.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

Bazen yardım isterken tanılama bilgilerini sağlayabilmeniz faydalı olabilir. Bazı günlükleri görmek için: 
1. [Azure Machine Learning Studio 'yu](https://ml.azure.com)ziyaret edin. 
1. Sol tarafta **denemeler** ' i seçin. 
1. Bir deneme seçin.
1. Bir çalıştırma seçin.
1. Üstteki **çıktılar + Günlükler**' i seçin.

> [!NOTE]
> Azure Machine Learning, eğitim sırasında (örneğin, oto ml) veya eğitim işini çalıştıran Docker kapsayıcısı gibi çeşitli kaynaklardan günlük bilgileri günlüğe kaydeder. Bu günlüklerin birçoğu açıklanmamıştır. Sorunlarla karşılaşırsanız ve Microsoft Destek ile iletişime geçerek, sorun giderme sırasında bu günlükleri kullanabiliyor olabilirler.


## <a name="resource-quotas"></a>Kaynak kotaları

Azure Machine Learning çalışırken karşılaşabileceğiniz [kaynak kotaları](how-to-manage-quotas.md) hakkında bilgi edinin.

## <a name="installation-and-import"></a>Yükleme ve içeri aktarma

* **PIP yüklemesi: bağımlılıkların tek satırlık yüklemeyle tutarlı olması garanti edilmez**: 

   Tek bir satır olarak yüklediğinizde çalışan bir bağımlılık Çözümleyicisi olmadığından, bu, PIP 'nin bilinen bir sınırlamasıdır. İlk benzersiz bağımlılık, tek bir baktığı tek. 

   Aşağıdaki kodda `azure-ml-datadrift` bulunur ve `azureml-train-automl` tek satırlık bir PIP yüklemesi kullanılarak yüklenir. 
     ```
       pip install azure-ml-datadrift, azureml-train-automl
     ```
   Bu örnekte, diyelim ki `azure-ml-datadrift` 1,0 sürüm > ve `azureml-train-automl` < 1,2 sürümünü gerektiriyor. En son sürümü 1,3 `azure-ml-datadrift` ise, daha eski bir sürümün `azureml-train-automl` paket gereksiniminden bağımsız olarak her iki paket de 1,3 ' e yükseltilir. 

   Paketleriniz için uygun sürümlerin yüklendiğinden emin olmak için aşağıdaki kodda gibi birden çok satır kullanarak yükleme yapın. Burada sıra bir sorun değildir, çünkü PIP bir sonraki satır çağrısının parçası olarak açıkça indirgendiğinden. Bu nedenle, uygun sürüm bağımlılıkları uygulanır.
    
     ```
        pip install azure-ml-datadrift
        pip install azureml-train-automl 
     ```

* **Panda hataları: genellikle, normal ml denemesi sırasında görüldü:**
   
   Environmnet kullanarak environmnet 'i el ile kurarken, desteklenmeyen paket sürümlerinin yüklenmesi nedeniyle öznitelik hataları (özellikle Pandas 'tan) fark edeceksiniz. Bu tür hataları engellemek için [lütfen automl_setup. cmd ' yi kullanarak oto ml SDK 'sını yüklemelisiniz](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/README.md):
   
    1. Bir Anaconda istemi açın ve bir örnek Not defteri kümesi için GitHub deposunu kopyalayın.

    ```bash
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```
    
    2. Örnek Not defterlerinin ayıklandığı nasıl yapılır kullanımı-azureml/otomatik makine öğrenimi klasörü ve ardından şunu çalıştırın:
    
    ```bash
    automl_setup
    ```
  
* **Hata iletisi: ' PyYAML ' kaldırılamıyor**

    Python için Azure Machine Learning SDK: PyYAML yüklü bir `distutils` projem. Bu nedenle, kısmi bir kaldırma işlemi varsa, hangi dosyaların kendisine ait olduğunu doğru bir şekilde belirleyemedik. Bu hatayı yoksayarak SDK 'Yı yüklemeye devam etmek için şunu kullanın:
    
    ```Python
    pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
    ```

* **Paketler yüklenirken databricks hatası**

    Azure Machine Learning SDK yüklemesi, daha fazla paket yüklendiğinde Azure Databricks başarısız olur. Gibi bazı paketler `psutil`çakışmalara neden olabilir. Yükleme hatalarını önlemek için, kitaplık sürümünü dondurarak paketleri yükleme. Bu sorun, Azure Machine Learning SDK 'Sı değil Databricks ile ilgilidir. Bu sorunla diğer kitaplıklarla de karşılaşabilirsiniz. Örnek:
    
    ```python
    psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
    ```

    Alternatif olarak, Python kitaplıklarıyla ilgili sorunları gidermek için Init betiklerini kullanabilirsiniz. Bu yaklaşım resmi olarak desteklenmez. Daha fazla bilgi için bkz. [küme kapsamlı init betikleri](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

* **Databricks içeri aktarma hatası: ' Pandas. _libs. tslibs ' öğesinden ' timedelta ' adı içeri**aktarılamıyor: otomatik makine öğrenimi kullandığınızda bu hatayı görürseniz, Not defterinizde aşağıdaki iki satırı çalıştırın:
    ```
    %sh rm -rf /databricks/python/lib/python3.7/site-packages/pandas-0.23.4.dist-info /databricks/python/lib/python3.7/site-packages/pandas
    %sh /databricks/python/bin/pip install pandas==0.23.4
    ```

* **Databricks içeri aktarma hatası: ' Pandas. Core. Indexes ' adlı bir modül yok**: otomatik makine öğrenimi kullandığınızda bu hatayı görürseniz:

    1. Azure Databricks kümenize iki paket yüklemek için şu komutu çalıştırın:
    
       ```bash
       scikit-learn==0.19.1
       pandas==0.22.0
       ```
    
    1. Kümeyi ayırın ve Not defterinize yeniden bağlayın.
    
    Bu adımlar sorunu çözmezse, kümeyi yeniden başlatmayı deneyin.

* **Databricks Failtosendtıı**: Azure Databricks kümesindeki verileri `FailToSendFeather` okurken bir hata görürseniz, aşağıdaki çözümlere başvurun:
    
    * Paketi `azureml-sdk[automl]` en son sürüme yükseltin.
    * Sürüm `azureml-dataprep` 1.1.8 veya üstünü ekleyin.
    * Sürüm `pyarrow` 0,11 veya üstünü ekleyin.
    
## <a name="create-and-manage-workspaces"></a>Çalışma alanları oluşturma ve yönetme

> [!WARNING]
> Azure Machine Learning çalışma alanınızı farklı bir aboneliğe taşımak veya sahip olunan aboneliğin yeni bir kiracıya taşınması desteklenmez. Bunun yapılması hatalara neden olabilir.

* **Azure Portal**: çalışma alanınızı SDK veya portaldan bir Share bağlantısından görüntülemeye doğrudan giderseniz, uzantı içindeki abonelik bilgileriyle normal **genel bakış** sayfasını görüntüleyemeyeceksiniz. Ayrıca, başka bir çalışma alanına geçiş yapamazsınız. Başka bir çalışma alanını görüntülemeniz gerekiyorsa, doğrudan [Azure Machine Learning Studio](https://ml.azure.com) 'ya gidin ve çalışma alanı adını arayın.

## <a name="set-up-your-environment"></a>Ortamınızı ayarlama

* **Amlcompute oluşturma sorunu**: Azure Machine Learning çalışma ALANıNı, GA sürümünden önce Azure Portal oluşturan bazı kullanıcıların, bu çalışma alanında AmlCompute oluşturamayacak nadir bir şansınız vardır. Hizmette bir destek isteği oluşturabilir veya portal veya SDK aracılığıyla hemen engelini kaldırmak için yeni bir çalışma alanı oluşturabilirsiniz.

## <a name="work-with-data"></a>Verilerle çalışma

### <a name="overloaded-azurefile-storage"></a>Aşırı yüklenmiş AzureFile depolama

Bir hata `Unable to upload project files to working directory in AzureFile because the storage is overloaded`alırsanız, aşağıdaki geçici çözümleri uygulayın.

Veri aktarımı gibi diğer iş yükleri için dosya paylaşma 'yı kullanıyorsanız, dosya paylaşımının çalıştırmaları için kullanılabilmesi için blob 'ları kullanmak, bu nedenle söz konusu çalışma. İş yükünü iki farklı çalışma alanı arasında da bölebilirsiniz.

### <a name="passing-data-as-input"></a>Verileri giriş olarak geçirme

*  **TypeError: Fılenotfound: böyle bir dosya veya dizin yok**: sağladığınız dosya yolu dosyanın bulunduğu yere değilse bu hata oluşur. Dosyaya başvurduğunuzdan emin olmanız gerekir. bu şekilde, veri kümenizi işlem Hedefinizdeki bağladığınız konum ile tutarlıdır. Belirleyici bir durum sağlamak için, bir veri kümesini bir işlem hedefine bağlamak için soyut yolun kullanılmasını öneririz. Örneğin, aşağıdaki kodda, veri kümesini işlem hedefinin FileSystem kökünün altına bağlamamız gerekir `/tmp`. 
    
    ```python
    # Note the leading / in '/tmp/dataset'
    script_params = {
        '--data-folder': dset.as_named_input('dogscats_train').as_mount('/tmp/dataset'),
    } 
    ```

    Önde gelen eğik çizgi '/' dahil değilseniz, veri kümesinin bağlanmasını istediğiniz yeri belirtmek için işlem hedefi üzerinde çalışma dizinini ( `/mnt/batch/.../tmp/dataset` .) ön eki uygulamanız gerekir.

### <a name="data-labeling-projects"></a>Veri etiketleme projeleri

|Sorun  |Çözüm  |
|---------|---------|
|Yalnızca blob veri depolarında oluşturulan veri kümeleri kullanılabilir     |  Bu, geçerli sürümün bilinen bir sınırlamasıdır.       |
|Oluşturulduktan sonra proje, uzun süredir "başlatılıyor" olarak gösterilir     | Sayfayı el ile yenileyin. Başlatma, saniyede yaklaşık 20 veri noktasında devam etmelidir. Bu, bilinen bir sorundur.         |
|Görüntüleri gözden geçirirken yeni etiketlenmiş görüntüler gösterilmez     |   Etiketlenmiş tüm görüntüleri yüklemek için **ilk** düğmeyi seçin. **İlk** düğme, listenin önüne geri götürür, ancak etiketlenmiş tüm verileri yükler.      |
|Nesne algılama için etiketleme sırasında Esc tuşuna basmak, sol üst köşede Sıfır boyutlu bir etiket oluşturur. Etiketlerin bu durumda gönderilmesi başarısız oluyor.     |   Yanındaki çapraz işaretine tıklayarak etiketi silin.  |

## <a name="azure-machine-learning-designer"></a>Azure Machine Learning Tasarımcısı

Bilinen sorunlar:

* **Uzun süreli işlem hazırlama süresi**: bir işlem hedefine ilk kez bağlanırken veya oluşturduğunuzda birkaç dakika veya daha uzun bir süre olabilir. 

## <a name="train-models"></a>Modelleri eğitme

* **Moduleerrors (modül adı yok)**: Azure ML 'de denemeleri gönderirken moduleerrors içinde çalıştırıyorsanız, eğitim betiğinin bir paketin yüklenmesini beklediği ancak eklenmediği anlamına gelir. Paket adını belirledikten sonra, Azure ML paketi eğitim çalıştırınızdan kullanılan ortama yükler. 

    Denemeleri göndermek için [estimators](concept-azure-machine-learning-architecture.md#estimators) kullanıyorsanız, paketi yüklemek istediğiniz kaynağı temel alarak tahmin aracı 'da veya `pip_packages` `conda_packages` parametresi aracılığıyla bir paket adı belirtebilirsiniz. Ayrıca, tüm bağımlılıklarınızı kullanarak `conda_dependencies_file`bir de bir de belirtebilirsiniz veya parametresini kullanarak `pip_requirements_file` bir txt dosyasındaki tüm PIP gereksinimlerinizi listeleyin. Tahmin aracı tarafından kullanılan varsayılan görüntüyü geçersiz kılmak istediğiniz bir Azure ML ortamı nesneniz varsa, bu ortamı tahmin aracı oluşturucusunun `environment` parametresi aracılığıyla belirtebilirsiniz.

    Azure ML, TensorFlow, PyTorch, Chainer ve Sköğren için çerçeveye özgü tahminler de sağlar. Bu tahmini kullanımı, çekirdek Framework bağımlılıklarının eğitim için kullanılan ortamda sizin adınıza yüklü olduğundan emin olur. Yukarıda açıklandığı gibi ek bağımlılıklar belirtme seçeneğiniz vardır. 
 
    Azure ML tarafından sağlanan Docker görüntüleri ve içerikleri, [AzureML kapsayıcılarında](https://github.com/Azure/AzureML-Containers)görülebilir.
    Çerçeveye özgü bağımlılıklar ilgili Framework belgelerinde listelenmiştir- [Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py#remarks), [pytorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py#remarks), [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py#remarks), [sköğren](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py#remarks).

    > [!Note]
    > Belirli bir paketin Azure ML tarafından korunan görüntülere ve ortamlara eklenmek için yeterince yaygın olduğunu düşünüyorsanız, lütfen [AzureML kapsayıcılarında](https://github.com/Azure/AzureML-Containers)GitHub sorununu yükseltin. 
 
* **NameError (ad tanımlı değil), AttributeError (nesne bir özniteliğe sahip değil)**: Bu özel durum eğitim betiklerinden gelmelidir. Tanımlı bir ad veya öznitelik hatası hakkında daha fazla bilgi edinmek için, Azure portal günlük dosyalarına bakabilirsiniz. SDK 'dan, hata iletisine bakmak için `run.get_details()` ' i kullanabilirsiniz. Bu, çalıştırma için oluşturulan tüm günlük dosyalarını da listeler. Lütfen eğitim betiğe göz atın ve çalıştırmayı yeniden göndermeden önce hatayı düzeltemedi. 

* **Horovod kapatıldı**: "AbortedError: Horovod" ile karşılaşırsanız çoğu durumda, bu özel durum Horovod 'nin kapatılmasına neden olan işlemlerden birinde temeldeki özel durum olduğu anlamına gelir. MPı işindeki her bir derecelendirme, Azure ML 'de özel bir günlük dosyası alır. Bu Günlükler adlandırılır `70_driver_logs`. Dağıtılmış eğitim söz konusu olduğunda, günlükleri ayırt etmek daha kolay hale getirmek `_rank` için günlük adlarının ' de sonlanmasını sağlayın. Horovod 'nin kapatılmasına neden olan hatayı tam olarak bulmak için, tüm günlük dosyalarını `Traceback` inceleyin ve driver_log dosyalarının sonundaki bölümüne bakın. Bu dosyalardan biri size gerçek temel özel durumu verecektir. 

* **Çalıştırma veya deneme silme**: denemeleri, [deneme. Arşiv](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#archive--) yöntemi kullanılarak veya Azure Machine Learning Studio istemcisindeki deneme sekmesi görünümünden "Arşiv denemesi" düğmesi aracılığıyla arşivlenebilir. Bu eylem, sorgu ve görünümleri listeleme denemesini gizler, ancak silmez.

    Tek tek denemeleri veya çalıştırmaları kalıcı olarak silme işlemi şu anda desteklenmiyor. Çalışma alanı varlıklarını silme hakkında daha fazla bilgi için bkz. [Machine Learning hizmeti çalışma alanı verilerinizi dışarı veya silme](how-to-export-delete-data.md).

* **Ölçüm belgesi çok büyük**: Azure Machine Learning bir eğitim çalıştırmasında bir kez günlüğe kaydedilebilir ölçüm nesnelerinin boyutunda iç sınırlara sahiptir. Liste değerli bir ölçümü günlüğe kaydederken "ölçüm belgesi çok büyük" hatası ile karşılaşırsanız, listeyi daha küçük parçalara bölmeyi deneyin, örneğin:

    ```python
    run.log_list("my metric name", my_metric[:N])
    run.log_list("my metric name", my_metric[N:])
    ```

    Azure ML, dahili olarak aynı ölçüm adına sahip blokları bitişik bir liste olarak birleştirir.

## <a name="automated-machine-learning"></a>Otomatik makine öğrenimi

* **Tensor akışı**: otomatik makine öğrenimi Şu anda Tensor Flow sürüm 1,13 ' i desteklememektedir. Bu sürümün yüklenmesi paket bağımlılıklarının çalışmayı durdurmasına neden olur. Bu sorunu gelecekte yayımlanacak bir sürümde gidermeye çalışıyoruz.

* **Deneme grafikleri**: otomatik ml denemesi yinelemeleriyle gösterilen ikili sınıflandırma grafikleri (duyarlık-HATıRLA, Roc, kazanç eğrisi vb.), 4/12 ' den beri Kullanıcı arabiriminde doğru işlenmemektedir. Grafik çizimleri Şu anda daha iyi şekilde uygulanan modellerin daha düşük sonuçlarla gösterildiği ters sonuçları gösteriyor. Bir çözüm, araştırma aşamasındadır.

* **Databricks otomatik makine öğrenimi çalıştırmayı iptal et**: Azure Databricks ' de otomatik makine öğrenimi özellikleri kullandığınızda, çalıştırmayı iptal etmek ve yeni bir deneme çalıştırması başlatmak için Azure Databricks kümenizi yeniden başlatın.

* **Databricks otomatik makine öğrenimi için 10 yineleme >**: otomatik makine öğrenimi ayarları 'nda 10 ' dan fazla yineleme sahipseniz, `show_output` çalıştırmayı `False` gönderdiğinizde olarak ayarlayın.

* **Azure MACHINE LEARNING SDK ve otomatik makine öğrenimi Için Databricks pencere öğesi**: Not defterleri HTML pencere öğelerini ayrıştıramadığından, bir databricks not DEFTERINDE Azure Machine Learning SDK pencere öğesi desteklenmez. Azure Databricks Not defteri hücresinizdeki bu python kodunu kullanarak portalda pencere öğesini görüntüleyebilirsiniz:

    ```
    displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
    ```

## <a name="deploy--serve-models"></a>Modelleri dağıtma ve sunma

Aşağıdaki hatalar için bu eylemleri gerçekleştirin:

|Hata  | Çözüm  |
|---------|---------|
|Web hizmeti dağıtımında görüntü oluşturma hatası     |  Görüntü yapılandırması için Conda dosyasına bir zar bağımlılığı olarak "pynacl = = 1.2.1" ekleyin       |
|`['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>`     |   Dağıtımınızda kullanılan VM 'Ler için SKU 'YU daha fazla belleğe sahip olan bir şekilde değiştirin. |
|FPGA hatası     |  FPGA kotası istenene ve onaylanana kadar, Fpg' de modeller dağıtacaksınız. Erişim istemek için kota isteği formunu doldurun:https://aka.ms/aml-real-time-ai       |

### <a name="updating-azure-machine-learning-components-in-aks-cluster"></a>AKS kümesindeki Azure Machine Learning bileşenleri güncelleştiriliyor

Azure Kubernetes hizmet kümesinde yüklü Azure Machine Learning bileşenlere yapılan güncelleştirmeler el ile uygulanmalıdır. 

Kümeyi Azure Machine Learning çalışma alanından ayırarak ve sonra kümeyi çalışma alanına yeniden açarak bu güncelleştirmeleri uygulayabilirsiniz. Kümede TLS etkinse, kümeyi yeniden eklerken TLS/SSL sertifikasını ve özel anahtarı sağlamanız gerekir. 

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

Artık TLS/SSL sertifikasına ve özel anahtara sahip değilseniz veya Azure Machine Learning tarafından oluşturulan bir sertifika kullanıyorsanız, küme kullanımdan çıkarmadan önce, gizli `kubectl` `azuremlfessl`anahtarı kullanarak kümeye bağlanarak dosyaları alabilirsiniz.

```bash
kubectl get secret/azuremlfessl -o yaml
```

>[!Note]
>Kubernetes gizli dizileri temel-64 kodlu biçimde depolar. Ana 64 'nin `cert.pem` ve `key.pem` içindeki parolaların bileşenlerinin kodunu vermeden önce kodu çözmelisiniz. `attach_config.enable_ssl` 

### <a name="webservices-in-azure-kubernetes-service-failures"></a>Azure Kubernetes hizmeti hatalarında WebServices

Azure Kubernetes hizmetindeki birçok Web hizmeti hatası, kullanılarak `kubectl`kümeye bağlanarak hata ayıklanabilir. Bir Azure Kubernetes hizmet kümesi `kubeconfig.json` için şunu çalıştırarak edinebilirsiniz

```azurecli-interactive
az aks get-credentials -g <rg> -n <aks cluster name>
```

## <a name="authentication-errors"></a>Kimlik Doğrulama hataları

Uzak bir işten bir işlem hedefinde yönetim işlemi gerçekleştirirseniz, aşağıdaki hatalardan birini alırsınız:

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

Örneğin, uzaktan yürütme için gönderilen bir ML ardışık düzeninde bir işlem hedefi oluşturmaya veya eklemeye çalıştığınızda bir hata alırsınız.
