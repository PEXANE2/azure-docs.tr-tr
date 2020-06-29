---
title: 'Öğretici: bir Machine Learning modeli oluşturmak için R kullanın'
titleSuffix: Azure Machine Learning
description: Bu öğreticide, bir arabadaki bir Fatality olma olasılığını tahmin eden bir lojistik regresyon modeli oluşturmak için Azure Machine Learning R SDK 'sını kullanacaksınız.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.reviewer: sgilley
author: revodavid
ms.author: davidsmi
ms.date: 02/07/2020
ms.openlocfilehash: d95c0fc9baf1e53e6643660726c72660719908ba
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85483389"
---
# <a name="tutorial-use-r-to-create-a-machine-learning-model-preview"></a>Öğretici: bir Machine Learning modeli oluşturmak için R kullanma (Önizleme)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu öğreticide, bir arabadaki bir Fatality olma olasılığını tahmin eden bir lojistik regresyon modeli oluşturmak için Azure Machine Learning R SDK (Önizleme) kullanacaksınız. Azure Machine Learning Bulut kaynaklarının, eğitim ve bir modeli dağıtmaya yönelik ölçeklenebilir bir ortam sağlamak için R ile nasıl çalıştığını göreceksiniz.  

Bu öğreticide, aşağıdaki görevleri gerçekleştireceksiniz:
> [!div class="checklist"]
> * Azure Machine Learning çalışma alanı oluşturma
> * Bu öğreticiyi çalışma alanınıza çalıştırmak için gerekli dosyaları içeren bir not defteri klasörünü kopyalayın
> * Çalışma alanınızdan RStudio 'Yu açın
> * Veri yükleme ve eğitim için hazırlanma
> * Verileri bir veri deposuna yükleme uzak eğitim için kullanılabilir
> * Modeli uzaktan eğitme için bir işlem kaynağı oluşturma
> * `caret`Fatality olasılığını tahmin etmek için bir modeli eğitme
> * Tahmin uç noktası dağıtma
> * Modeli R 'den test etme

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.


## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Azure Machine Learning çalışma alanı, bulutta makine öğrenimi modellerini denemek, eğmek ve dağıtmak için kullandığınız temel bir kaynaktır. Azure aboneliğiniz ve kaynak grubunuz, hizmette kolayca tüketilen bir nesne ile aynı olur. 

Azure kaynaklarınızı yönetmek için Web tabanlı bir konsol olan Azure portal bir çalışma alanı oluşturursunuz. 

[!INCLUDE [aml-create-portal](../../includes/aml-create-in-portal.md)]

>[!IMPORTANT] 
> **Çalışma alanınızı** ve **aboneliğinizi**bir yere göz atın. Denemenizin doğru yerde oluşturulmasını sağlamak için bunlara ihtiyacınız olacaktır. 


## <a name="clone-a-notebook-folder"></a><a name="azure"></a>Not defteri klasörünü Kopyala

Bu örnek, bir yüklemeden ücretsiz ve önceden yapılandırılmış bir deneyim için çalışma alanınızdaki bulut Not defteri sunucusunu kullanır. Ortamınız, paketler ve bağımlılıklarınız üzerinde denetim sahibi olmayı tercih ediyorsanız [kendi ortamınızı](https://azure.github.io/azureml-sdk-for-r/articles/installation.html) kullanın.

Tüm beceri seviyeleri için veri bilimi senaryoları gerçekleştirmek üzere Machine Learning araçları 'nı içeren birleştirilmiş bir arabirim olan Azure Machine Learning Studio 'da aşağıdaki deneme sürümü kurulumunu tamamlayıp adımları gerçekleştirin.

1. [Azure Machine Learning Studio](https://ml.azure.com/)'da oturum açın.

1. Aboneliğinizi ve oluşturduğunuz çalışma alanını seçin.

1. Sol taraftaki **not defterlerini** seçin.

1. **Örnekler** klasörünü açın.

1. **R** klasörünü açın.

1. Klasörü üzerinde bir sürüm numarasıyla açın.  Bu sayı, R SDK için geçerli yayını temsil eder.

1. **Vignettes** klasörünün sağ tarafındaki **"..."** öğesini seçin ve ardından **Kopyala**' yı seçin.

    ![Klasörü kopyala](media/tutorial-1st-r-experiment/clone-folder.png)

1. Çalışma alanına erişen her kullanıcıyı gösteren bir klasör listesi görüntülenir.  **Vignettes** klasörünü kopyalamak için klasörünüzü seçin.

## <a name="open-rstudio"></a><a name="open"></a>RStudio 'Yu aç

Bu öğreticiyi çalıştırmak için bir işlem örneği veya Not defteri sanal makinesinde RStudio kullanın.  

1. Sol tarafta **işlem** ' ı seçin.

1. Zaten yoksa bir işlem kaynağı ekleyin.

1. İşlem çalışırken **rstudio 'yu** kullanarak rstudio 'yu açın.

1. RStudio 'da, *Vignettes* klasörünüz, alt sağ taraftaki **dosyalar** bölümünde yer alan *kullanıcılardan* birkaç düzeydir.  *Vignettes*altında, bu öğreticide gereken dosyaları bulmak için *eğitme ve dağıtma-aci* klasörünü seçin.

> [!Important]
> Bu makalenin geri kalanında, *eğitme ve dağıtma-aci ile aynı içeriğe sahip olursunuz. RMD* dosyası. Rmarkkiyle karşılaşırsanız, bu dosyadaki kodu kullanmayı ücretsiz olarak kullanabilirsiniz.  Ya da kod parçacıklarını buradan veya bu makaleden bir R betiğine veya komut satırına kopyalayabilir/yapıştırabilirsiniz.  


## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma
Bu öğreticide geliştirme çalışmalarınız için kurulum aşağıdaki eylemleri içerir:

* Gerekli paketleri yükleme
* İşlem örneğinizin uzak kaynaklarla iletişim kurabilmesi için bir çalışma alanına bağlanın
* Çalıştırmalarınızı izlemek için bir deneme oluşturun
* Eğitim için kullanmak üzere uzak bir işlem hedefi oluşturun

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

 * CRAN 'ın en son sürümünü yükler.

    ```R
    # install the latest version from CRAN
    install.packages("azuremlsdk")
    azuremlsdk::install_azureml(envname = 'r-reticulate')
    ```
    
* Veya geliştirme sürümünü GitHub 'dan yükleyemezsiniz.

    ```R
    # or install the development version from GitHub
    remotes::install_github('https://github.com/Azure/azureml-sdk-for-r')
    azuremlsdk::install_azureml(envname = 'r-reticulate')
    ```

Şimdi devam edin ve **azuremlsdk** paketini içeri aktarın.

```R
library(azuremlsdk)
```

Eğitim ve Puanlama betikleri ( `accidents.R` ve `accident_predict.R` ) bazı ek bağımlılıklara sahiptir. Bu betikleri yerel olarak çalıştırmayı planlıyorsanız, gerekli paketlerin de bulunduğundan emin olun.

### <a name="load-your-workspace"></a>Çalışma alanınızı yükleme
Mevcut çalışma alanınızdan bir çalışma alanı nesnesi örneği oluşturun. Aşağıdaki kod, çalışma alanı ayrıntılarını dosyadaki **config.js** yükleyecek. Ayrıca kullanarak bir çalışma alanı da alabilirsiniz [`get_workspace()`](https://azure.github.io/azureml-sdk-for-r/reference/get_workspace.html) .

```R
ws <- load_workspace_from_config()
```

### <a name="create-an-experiment"></a>Deneme oluşturma
Azure ML denemesi, genellikle aynı eğitim betiğiyle çalıştırılan bir gruplamayı izler. Kazalardan dolayı verilerinde giriş işareti modeline eğitim için çalıştırmaları izlemek üzere bir deneme oluşturun.

```R
experiment_name <- "accident-logreg"
exp <- experiment(ws, experiment_name)
```

### <a name="create-a-compute-target"></a>İşlem hedefi oluştur
Yönetilen bir hizmet olan Azure Machine Learning Işlem (AmlCompute) kullanarak, veri bilimcileri, Azure sanal makinelerinin kümelerinde makine öğrenimi modellerini eğitebilir. Örnek olarak GPU desteği olan VM 'Ler sayılabilir. Bu öğreticide, eğitim ortamınız olarak tek düğümlü bir AmlCompute kümesi oluşturacaksınız. Aşağıdaki kod, çalışma alanınızda zaten mevcut değilse işlem kümesini sizin için oluşturur.

Zaten mevcut değilse, işlem kümenizin sağlanması için birkaç dakika beklemeniz gerekebilir.

```R
cluster_name <- "rcluster"
compute_target <- get_compute(ws, cluster_name = cluster_name)
if (is.null(compute_target)) {
  vm_size <- "STANDARD_D2_V2" 
  compute_target <- create_aml_compute(workspace = ws,
                                       cluster_name = cluster_name,
                                       vm_size = vm_size,
                                       max_nodes = 1)
}

wait_for_provisioning_completion(compute_target)
```

## <a name="prepare-data-for-training"></a>Eğitim için verileri hazırlama
Bu öğretici ABD [Ulusal otoyolu trafik güvenliği yönetimi](https://cdan.nhtsa.gov/tsftables/tsfar.htm) ( [Mary C. Meyer ve Tremika Finney](https://www.stat.colostate.edu/~meyer/airbags.htm)sayesinde) verilerini kullanır.
Bu veri kümesi, Fatality olma olasılığını tahmin etmek için kullanabileceğiniz değişkenlerle birlikte ABD 'de 25.000 otomobil kilitlenmelerinden veri içerir. İlk olarak, verileri R 'ye aktarın ve analiz için yeni bir veri çerçevesine dönüştürün `accidents` ve dosyayı bir dosyaya dışarı aktarın `Rdata` .

```R
nassCDS <- read.csv("nassCDS.csv", 
                     colClasses=c("factor","numeric","factor",
                                  "factor","factor","numeric",
                                  "factor","numeric","numeric",
                                  "numeric","character","character",
                                  "numeric","numeric","character"))
accidents <- na.omit(nassCDS[,c("dead","dvcat","seatbelt","frontal","sex","ageOFocc","yearVeh","airbag","occRole")])
accidents$frontal <- factor(accidents$frontal, labels=c("notfrontal","frontal"))
accidents$occRole <- factor(accidents$occRole)
accidents$dvcat <- ordered(accidents$dvcat, 
                          levels=c("1-9km/h","10-24","25-39","40-54","55+"))

saveRDS(accidents, file="accidents.Rd")
```

### <a name="upload-data-to-the-datastore"></a>Veri deposuna veri yükleme
Uzaktan Eğitim ortamınıza erişebilmek için verileri buluta yükleyin. Her Azure Machine Learning çalışma alanı, çalışma alanına bağlı depolama hesabında sağlanan Azure Blob kapsayıcısına bağlantı bilgilerini depolayan varsayılan bir veri deposu ile gelir. Aşağıdaki kod, yukarıda oluşturduğunuz kazalardan dolayı verilerini bu veri deposuna karşıya yükler.

```R
ds <- get_default_datastore(ws)

target_path <- "accidentdata"
upload_files_to_datastore(ds,
                          list("./accidents.Rd"),
                          target_path = target_path,
                          overwrite = TRUE)
```


## <a name="train-a-model"></a>Modeli eğitme

Bu öğreticide, uzaktan işlem kümenizi kullanarak karşıya yüklenen verilerinize bir lojistik regresyon modeli uydurun. Bir iş göndermek için şunları yapmanız gerekir:

* Eğitim betiğini hazırlama
* Tahmin aracı oluşturma
* İşi gönderme

### <a name="prepare-the-training-script"></a>Eğitim betiğini hazırlama
`accidents.R`Bu öğreticiyle aynı dizinde sizin için çağrılan bir eğitim betiği sunulmaktadır. Eğitim betiğine yönelik olarak Azure Machine Learning faydalanmak için yapılan **eğitim betiği içinde** aşağıdaki ayrıntılara dikkat edin:

* Eğitim betiği, `-d` eğitim verilerini içeren dizini bulmak için bir bağımsız değişken alır. İşinizi daha sonra tanımlayıp gönderdiğinizde, bu bağımsız değişken için veri deposuna işaret edersiniz. Azure ML, eğitim işi için depolama klasörünü uzak kümeye bağlayacaktır.
* Eğitim betiği, kullanarak Azure ML 'deki çalıştırma kaydına bir ölçüm olarak son doğruluğu günlüğe kaydeder `log_metric_to_run()` . Azure ML SDK, eğitim çalıştırmaları sırasında çeşitli ölçümleri günlüğe kaydetmek için bir günlük API kümesi sağlar. Bu ölçümler, deneme çalıştırması kaydında kaydedilir ve kalıcı hale getirilir. Ölçümlere daha sonra istediğiniz zaman erişilebilir veya [Studio](https://ml.azure.com)'daki çalıştırma ayrıntıları sayfasında görüntüleyebilirsiniz. Günlük yöntemlerinin tamamına yönelik [başvuruya](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-training-experimentation) bakın `log_*()` .
* Eğitim betiği, modelinizi **çıktılar**adlı bir dizine kaydeder. `./outputs`Klasör, Azure ML tarafından özel bir işleme alır. Eğitim sırasında, üzerine yazılan dosyalar `./outputs` Azure ML tarafından çalıştırma kaydlarınıza otomatik olarak yüklenir ve yapıtlar olarak kalıcı hale getirilir. Eğitilen modeli uygulamasına kaydederek `./outputs` , çalışma bittikten sonra ve uzaktan eğitim ortamınıza artık erişiminiz olmadığında bile model dosyanıza erişebiliyor ve bu dosyayı alabilirsiniz.

### <a name="create-an-estimator"></a>Tahmin aracı oluşturma

Azure ML tahmin aracı, işlem hedefinde bir eğitim betiği yürütmek için gereken çalıştırma yapılandırma bilgilerini kapsüller. Azure ML çalıştırmaları, belirtilen işlem hedefi üzerinde Kapsayıcılı işler olarak çalıştırılır. Varsayılan olarak, eğitim işiniz için tasarlanan Docker görüntüsü R, Azure ML SDK ve yaygın olarak kullanılan bir R paketi kümesi içerir. Varsayılan paketlerin tam listesine buraya bakın.

Estimator oluşturmak için şunu tanımlayın:

* Eğitim () için gereken betiklerinizi içeren dizin `source_directory` . Bu dizindeki tüm dosyalar, yürütme için küme düğümüne yüklenir. Dizin, eğitim betiğinizi ve gereken ek betikleri içermelidir.
* Yürütülecek eğitim betiği ( `entry_script` ).
* `compute_target`Bu durumda, daha önce oluşturduğunuz AmlCompute kümesi, bu örnekte işlem hedefi ().
* Eğitim betiğiyle () gerekli parametreler `script_params` . Azure ML, eğitim betiğinizi ile bir komut satırı betiği olarak çalıştıracaktır `Rscript` . Bu öğreticide, ile erişebileceğiniz veri dizini bağlama noktası olan betiğe bir bağımsız değişken belirtirsiniz `ds$path(target_path)` .
* Eğitim için gereken tüm ortam bağımlılıkları. Eğitim için tasarlanan varsayılan Docker görüntüsü, `caret` `e1071` eğitim betiğine gereken üç paketi (,, ve) zaten içerir `optparse` .  Bu nedenle ek bilgi belirtmeniz gerekmez. Varsayılan olarak dahil olmayan R paketleri kullanıyorsanız, `cran_packages` ek CRAN paketleri eklemek için Estimator 'un parametresini kullanın. [`estimator()`](https://azure.github.io/azureml-sdk-for-r/reference/estimator.html)Yapılandırılabilir seçeneklerin tam kümesinin başvurusuna bakın.

```R
est <- estimator(source_directory = ".",
                 entry_script = "accidents.R",
                 script_params = list("--data_folder" = ds$path(target_path)),
                 compute_target = compute_target
                 )
```

### <a name="submit-the-job-on-the-remote-cluster"></a>İşi uzak kümede gönder

Son olarak, işi kümenizde çalışacak şekilde gönderebilirsiniz. `submit_experiment()`daha sonra çalıştırma ile arabirim için kullandığınız bir Run nesnesi döndürür. Toplam olarak, ilk çalıştırma **yaklaşık 10 dakika**sürer. Ancak daha sonra çalıştırıldığında, komut dosyası bağımlılıkları değişmedikçe aynı Docker görüntüsü yeniden kullanılır.  Bu durumda, görüntü önbelleğe alınır ve kapsayıcı başlangıç zamanı çok daha hızlıdır.

```R
run <- submit_experiment(exp, est)
```

Çalıştırmanın ayrıntılarını RStudio görüntüleyicisinde görüntüleyebilirsiniz. Belirtilen "Web görünümü" bağlantısına tıkladığınızda, Kullanıcı arabiriminde çalışmayı izleyebileceğiniz Azure Machine Learning Studio 'ya gidecektir.

```R
view_run_details(run)
```

Model eğitimi arka planda gerçekleşir. Daha fazla kod çalıştırmadan önce model eğitimi bitirene kadar bekleyin.

```R
wait_for_run_completion(run, show_output = TRUE)
```

Siz ve çalışma alanına erişimi olan iş arkadaşları--paralel olarak birden çok denemeleri gönderebilir ve Azure ML, işlem kümesindeki görevleri zamanlamaya göre planlamalarını sağlar. Kümeyi, birden fazla düğüme otomatik olarak ölçeklenebilen şekilde yapılandırabilir ve kuyrukta daha fazla işlem görevi olmadığında ölçeği geri ölçeklendirebilirsiniz. Bu yapılandırma ekiplerin işlem kaynaklarını paylaşması için ekonomik bir yoldur.

## <a name="retrieve-training-results"></a>Eğitim sonuçlarını alma
Modelinize yönelik eğitimi tamamladıktan sonra, kaydedilen tüm ölçümler ve son eğitilen model dahil olmak üzere, çalışma kaydında kalıcı olan iş yapıtlarına erişebilirsiniz.

### <a name="get-the-logged-metrics"></a>Günlüğe kaydedilen ölçümleri alın
Eğitim betiğiyle `accidents.R` , modelinizden bir ölçüm günlüğe kaydedilir: eğitim verilerinde tahminlerinin doğruluğu. Ölçümleri [Studio](https://ml.azure.com)'da görebilir veya bir R listesi olarak yerel oturuma aşağıdaki gibi ayıklayabilirsiniz:

```R
metrics <- get_run_metrics(run)
metrics
```

Birden çok denemeleri (örneğin, farklı değişkenler, algoritmalar veya hiper paramers kullanarak) çalıştırırsanız, üretimde kullanacağınız modeli karşılaştırmak ve seçmek için her çalıştıranların ölçümlerini kullanabilirsiniz.

### <a name="get-the-trained-model"></a>Eğitilen modeli al
Eğitilen modeli alabilir ve yerel R oturumunuzda sonuçlara bakabilirsiniz. Aşağıdaki kod, `./outputs` model dosyasını içeren dizinin içeriğini indirir.

```R
download_files_from_run(run, prefix="outputs/")
accident_model <- readRDS("outputs/model.rds")
summary(accident_model)
```

Tahmini ölüm olasılığının artışına katkıda bulunan bazı faktörler görürsünüz:

* daha yüksek etki hızı 
* Erkek sürücü
* daha eski işant
* yolcular

Şu ile ölümün daha düşük olasılıkların olduğunu göreceksiniz:

* Uçak varlığı
* varlık Seatbelts
* en fazla çakışma 

Üretim araç yılının önemli bir etkisi yoktur.

Bu modeli yeni tahminler yapmak için kullanabilirsiniz:

```R
newdata <- data.frame( # valid values shown below
 dvcat="10-24",        # "1-9km/h" "10-24"   "25-39"   "40-54"   "55+"  
 seatbelt="none",      # "none"   "belted"  
 frontal="frontal",    # "notfrontal" "frontal"
 sex="f",              # "f" "m"
 ageOFocc=16,          # age in years, 16-97
 yearVeh=2002,         # year of vehicle, 1955-2003
 airbag="none",        # "none"   "airbag"   
 occRole="pass"        # "driver" "pass"
 )

## predicted probability of death for these variables, as a percentage
as.numeric(predict(accident_model,newdata, type="response")*100)
```

## <a name="deploy-as-a-web-service"></a>Web hizmeti olarak dağıtma

Modelinize göre, çakışmadan ölüm tehlikini tahmin edebilirsiniz. Azure ML 'yi kullanarak modelinizi bir tahmin hizmeti olarak dağıtın. Bu öğreticide, Web hizmetini [Azure Container Instances](https://docs.microsoft.com/azure/container-instances/) (aci) ' de dağıtacaksınız.

### <a name="register-the-model"></a>Modeli kaydedin

İlk olarak, çalışma alanınıza indirdiğiniz modeli ile kaydedin [`register_model()`](https://azure.github.io/azureml-sdk-for-r/reference/register_model.html) . Kayıtlı bir model herhangi bir dosya koleksiyonu olabilir, ancak bu durumda R model nesnesi yeterlidir. Azure ML, dağıtım için kayıtlı modeli kullanır.

```R
model <- register_model(ws, 
                        model_path = "outputs/model.rds", 
                        model_name = "accidents_model",
                        description = "Predict probablity of auto accident")
```

### <a name="define-the-inference-dependencies"></a>Çıkarım bağımlılıklarını tanımlama
Modelinize yönelik bir Web hizmeti oluşturmak için önce bir Puanlama betiği () oluşturmanız gerekir ( `entry_script` JSON biçiminde) ve modelinizden bir tahmin çıktısı alacak bir R betiği. Bu öğretici için, sunulan Puanlama dosyasını kullanın `accident_predict.R` . Puanlama betiği, `init()` modelinizi yükleyen ve giriş verilerine dayalı bir tahmin yapmak için modeli kullanan bir işlev döndüren bir yöntem içermelidir. Daha fazla bilgi için [belgelere](https://azure.github.io/azureml-sdk-for-r/reference/inference_config.html#details) bakın.

Sonra, betiğinizin paket bağımlılıkları için bir Azure ML **ortamı** tanımlayın. Bir ortamla, betiğinizin çalışması için gereken R paketlerini (CRAN veya başka bir yerde) belirtirsiniz. Ayrıca, betiğinizin davranışını değiştirmek için başvurmasına olanak sağlayan ortam değişkenlerinin değerlerini de sağlayabilirsiniz. Varsayılan olarak, Azure ml, eğitim için tahmin aracı ile kullanılan varsayılan Docker görüntüsünü oluşturur. Öğreticinin özel gereksinimleri olmadığından, özel öznitelikleri olmayan bir ortam oluşturun.

```R
r_env <- r_environment(name = "basic_env")
```

Bunun yerine dağıtım için kendi Docker görüntünüzü kullanmak istiyorsanız, `custom_docker_image` parametresini belirtin. [`r_environment()`](https://azure.github.io/azureml-sdk-for-r/reference/r_environment.html)Ortam tanımlama için yapılandırılabilir seçeneklerin tam kümesinin başvurusuna bakın.

Artık Puanlama betiğinizi ve ortam bağımlılıklarınızı kapsüllemek için bir **çıkarım yapılandırması** oluşturmak için ihtiyacınız olan her şeye sahipsiniz.

```R
inference_config <- inference_config(
  entry_script = "accident_predict.R",
  environment = r_env)
```

### <a name="deploy-to-aci"></a>ACI'ye dağıtma
Bu öğreticide, hizmetinizi ACI 'ye dağıtacaksınız. Bu kod, test ve hafif yüklemeler için uygun olan gelen isteklere yanıt vermek için tek bir kapsayıcı sağlar. [`aci_webservice_deployment_config()`](https://azure.github.io/azureml-sdk-for-r/reference/aci_webservice_deployment_config.html)Ek yapılandırılabilir seçenekler için bkz.. (Üretim ölçeğinde dağıtımlar için [Azure Kubernetes hizmeti](https://azure.github.io/azureml-sdk-for-r/articles/deploy-to-aks.html)' ne de dağıtım yapabilirsiniz.)

``` R
aci_config <- aci_webservice_deployment_config(cpu_cores = 1, memory_gb = 0.5)
```

Şimdi modelinizi bir Web hizmeti olarak dağıtırsınız. Dağıtım **birkaç dakika sürebilir**. 

```R
aci_service <- deploy_model(ws, 
                            'accident-pred', 
                            list(model), 
                            inference_config, 
                            aci_config)

wait_for_deployment(aci_service, show_output = TRUE)
```

## <a name="test-the-deployed-service"></a>Dağıtılan hizmeti test etme

Artık modelinize bir hizmet olarak dağıtıldığına göre, hizmetini kullanarak R 'den test edebilirsiniz [`invoke_webservice()`](https://azure.github.io/azureml-sdk-for-r/reference/invoke_webservice.html) .  Tahmin edilecek yeni bir veri kümesi sağlayın, bunu JSON 'a dönüştürün ve hizmete gönderin.

```R
library(jsonlite)

newdata <- data.frame( # valid values shown below
 dvcat="10-24",        # "1-9km/h" "10-24"   "25-39"   "40-54"   "55+"  
 seatbelt="none",      # "none"   "belted"  
 frontal="frontal",    # "notfrontal" "frontal"
 sex="f",              # "f" "m"
 ageOFocc=22,          # age in years, 16-97
 yearVeh=2002,         # year of vehicle, 1955-2003
 airbag="none",        # "none"   "airbag"   
 occRole="pass"        # "driver" "pass"
 )

prob <- invoke_webservice(aci_service, toJSON(newdata))
prob
```

Ayrıca, REST istemci çağrılarını kabul eden Web hizmetinin HTTP uç noktasını da alabilirsiniz. Bu uç noktayı, Web hizmetini test etmek veya bir uygulamayla bütünleştirmek isteyen herkesle paylaşabilirsiniz.

```R
aci_service$scoring_uri
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç kalmadığında kaynakları silin. Hala kullanmayı planladığınız herhangi bir kaynağı silmeyin. 

Web hizmetini silin:
```R
delete_webservice(aci_service)
```

Kayıtlı modeli Sil:
```R
delete_model(model)
```

Hesaplama kümesini silme:
```R
delete_compute(compute)
```

### <a name="delete-everything"></a>Her şeyi sil

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

Ayrıca, kaynak grubunu koruyabilir ancak tek bir çalışma alanını silebilirsiniz. Çalışma alanı özelliklerini görüntüleyin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

* R 'de ilk Azure Machine Learning denemenizi tamamladığınıza göre, [r için Azure MACHINE LEARNING SDK](https://azure.github.io/azureml-sdk-for-r/index.html)hakkında daha fazla bilgi edinin.

* Diğer *Vignettes* klasörlerindeki örneklerden R ile Azure Machine Learning hakkında daha fazla bilgi edinin.
