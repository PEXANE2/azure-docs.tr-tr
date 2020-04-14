---
title: "Öğretici: Makine öğrenme modeli oluşturmak için R'yi kullanın"
titleSuffix: Azure Machine Learning
description: Bu eğitimde, bir araba kazasında ölüm olasılığını tahmin eden bir lojistik regresyon modeli oluşturmak için Azure Machine Learning R SDK'yı kullanacaksınız.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.reviewer: sgilley
author: revodavid
ms.author: davidsmi
ms.date: 02/07/2020
ms.openlocfilehash: 5b1c6561519bc25c2b7ac77f0a25eff89413a07a
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81256493"
---
# <a name="tutorial-use-r-to-create-a-machine-learning-model"></a>Öğretici: Makine öğrenme modeli oluşturmak için R'yi kullanın
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu eğitimde, bir araba kazasında ölüm olasılığını tahmin eden bir lojistik regresyon modeli oluşturmak için Azure Machine Learning R SDK'yı kullanacaksınız. Bir modeli eğitmek ve dağıtmak için ölçeklenebilir bir ortam sağlamak için Azure Machine Learning bulut kaynaklarının R ile nasıl çalıştığını görürsünüz.  

Bu öğreticide, aşağıdaki görevleri gerçekleştireceksiniz:
> [!div class="checklist"]
> * Azure Machine Learning çalışma alanı oluşturma
> * Bu öğreticiyi çalışma alanınıza çalıştırmak için gerekli dosyaları içeren bir not defteri klasörünü klonlama
> * Çalışma alanınızdan RStudio'u açın
> * Verileri yükleyin ve eğitime hazırlanın
> * Uzaktan eğitim için kullanılabilecek şekilde verileri bir veri deposuna yükleyin
> * Modeli uzaktan eğitmek için bir bilgi işlem kaynağı oluşturma
> * Ölüm `caret` olasılığını tahmin etmek için bir model eğitin
> * Tahmin bitiş noktası dağıtma
> * Modeli R'den test edin

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.


## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Azure Machine Learning çalışma alanı, makine öğrenimi modellerini denemek, eğitmek ve dağıtmak için bulutta kullandığınız temel bir kaynaktır. Azure aboneliğinizi ve kaynak grubunuzu hizmette kolayca tüketilen bir nesneye bağlar. 

Azure kaynaklarınızı yönetmek için web tabanlı bir konsol olan Azure portalı üzerinden bir çalışma alanı oluşturursunuz. 

[!INCLUDE [aml-create-portal](../../includes/aml-create-in-portal.md)]

>[!IMPORTANT] 
> **Çalışma alanınızı** ve **aboneliğinizi**not alın. Denemenizi doğru yerde oluşturduğunuzdan emin olmak için bunlara ihtiyacınız olacak. 


## <a name="clone-a-notebook-folder"></a><a name="azure"></a>Not defteri klasörünü klonlama

Bu örnek, yüklemegerektirmeyen ve önceden yapılandırılmış bir deneyim için çalışma alanınızdaki bulut not defteri sunucusunu kullanır. [Ortamınız, paketleriniz](https://azure.github.io/azureml-sdk-for-r/articles/installation.html) ve bağımlılıklarınız üzerinde denetim sahibi olmayı tercih ediyorsanız kendi ortamınızı kullanın.

Tüm beceri düzeylerindeki veri bilimi uygulayıcıları için veri bilimi senaryoları gerçekleştirmek için makine öğrenimi araçlarını içeren birleştirilmiş arabirim olan Azure Machine Learning stüdyosunda aşağıdaki deneme kurulum ve çalıştırma adımlarını tamamlarsınız.

1. [Azure Machine Learning stüdyosunda](https://ml.azure.com/)oturum açın.

1. Aboneliğinizi ve oluşturduğunuz çalışma alanını seçin.

1. Soldaki **Not Defterleri'ni** seçin.

1. **Örnekler** klasörünü açın.

1. **R** klasörünü açın.

1. Üzerinde sürüm numarası olan klasörü açın.  Bu sayı R SDK için geçerli sürümü temsil eder.

1. **Vinyetler** klasörünün sağındaki **"..."** seçeneğini belirleyin ve ardından **Klon'u**seçin.

    ![Klon klasörü](media/tutorial-1st-r-experiment/clone-folder.png)

1. Çalışma alanına erişen her kullanıcıyı gösteren klasörlerin listesi görüntülenir.  **Vinyetler** klasörünü klonlamak için klasörünüzü seçin.

## <a name="a-nameopenopen-rstudio"></a><a name="open">RStudio'u aç

Bu öğreticiyi çalıştırmak için bir işlem örneğinde veya Notebook VM'de RStudio'u kullanın.  

1. Soldaki **İşlem'i** seçin.

1. Zaten yoksa bir bilgi işlem kaynağı ekleyin.

1. İşlem çalışmaya başladığında, **RStudio'yu** açmak için RStudio bağlantısını kullanın.

1. RStudio'da *vinyet* klasörünüz sağ alttaki **Dosyalar** bölümündeki *Kullanıcılar'dan* birkaç seviye aşağıdadır.  *Vinyetler*altında, bu öğreticide gerekli dosyaları bulmak *için tren ve dağıtım-to-aci* klasörünü seçin.

> [!Important]
> Bu makalenin geri *kalanı, tren ve dağıtma-aci'de gördüğünüz içerikle aynı içeriği içerir. Rmd* dosyası. RMarkdown ile deneyimli iseniz, bu dosyadan kodu kullanmaktan çekinmeyin.  Ya da kod parçacıklarını oradan kopyalayabilir/yapıştırabilir veya bu makaleden bir R komut dosyasına veya komut satırına yapıştırabilirsiniz.  


## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma
Bu öğreticideki geliştirme çalışmanızın kurulumu aşağıdaki eylemleri içerir:

* Gerekli paketleri yükleme
* Bilgi işlem örneğinizin uzak kaynaklarla iletişim kurabilmesi için bir çalışma alanına bağlanın
* Çalışanlarınızı izlemek için bir deneme oluşturun
* Eğitim için kullanmak üzere uzak bir işlem hedefi oluşturma

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

 * CRAN'ın en son sürümünü yükleyin.

    ```R
    # install the latest version from CRAN
    install.packages("azuremlsdk")
    azuremlsdk::install_azureml(envname = 'r-reticulate')
    ```
    
* Veya GitHub'dan geliştirme sürümünü yükleyin.

    ```R
    # or install the development version from GitHub
    remotes::install_github('https://github.com/Azure/azureml-sdk-for-r')
    azuremlsdk::install_azureml(envname = 'r-reticulate')
    ```

Şimdi devam edin ve **azuremlsdk** paketini içe aktarın.

```R
library(azuremlsdk)
```

Eğitim ve puanlama komut`accidents.R` `accident_predict.R`dosyaları ( ve ) bazı ek bağımlılıkları vardır. Bu komut dosyalarını yerel olarak çalıştırmayı planlıyorsanız, gerekli paketlere de sahip olduğunuzdan emin olun.

### <a name="load-your-workspace"></a>Çalışma alanınızı yükleyin
Bir çalışma alanı nesnesi varolan çalışma alanınızdan anında elde edin. Aşağıdaki kod, çalışma alanı ayrıntılarını **config.json** dosyasından yükler. Ayrıca bir çalışma alanı [`get_workspace()`](https://azure.github.io/azureml-sdk-for-r/reference/get_workspace.html)kullanarak da alabilirsiniz.

```R
ws <- load_workspace_from_config()
```

### <a name="create-an-experiment"></a>Deneme oluşturma
Azure ML denemesi, genellikle aynı eğitim komut dosyasından bir dizi çalıştırmayı izler. Kaza verileri üzerinde caret modelini eğitmek için çalıştırmaları izlemek için bir deneme oluşturun.

```R
experiment_name <- "accident-logreg"
exp <- experiment(ws, experiment_name)
```

### <a name="create-a-compute-target"></a>İşlem hedefi oluşturma
Veri bilimciler, yönetilen bir hizmet olan Azure Machine Learning Compute 'u (AmlCompute) kullanarak, makine öğrenimi modellerini Azure sanal makine kümeleri üzerinde eğitebilir. GPU destekli VM'ler buna örnek olarak verilebilir. Bu öğreticide, eğitim ortamınız olarak tek düğümlü Bir AmlCompute kümesi oluşturursunuz. Aşağıdaki kod, çalışma alanınızda zaten yoksa sizin için işlem kümesioluşturur.

İşlem kümenizin zaten yoksa sağlanması için birkaç dakika beklemeniz gerekebilir.

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

## <a name="prepare-data-for-training"></a>Eğitim için veri hazırlama
Bu öğretici [(Mary C. Meyer ve Tremika Finney](https://www.stat.colostate.edu/~meyer/airbags.htm)sayesinde) ABD [Ulusal Karayolu Trafik Güvenliği İdaresi](https://cdan.nhtsa.gov/tsftables/tsfar.htm) verileri kullanır.
Bu veri kümesi, ABD'de 25.000'den fazla araba kazasından elde edilen verileri içerir ve bir ölüm olasılığını tahmin etmek için kullanabileceğiniz değişkenler vardır. İlk olarak, verileri R'ye aktarın ve `accidents` analiz için yeni bir `Rdata` veri çerçevesine dönüştürün ve bir dosyaya aktarın.

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
Uzaktan eğitim ortamınız tarafından erişilebilmek için buluta veri yükleyin. Her Azure Machine Learning çalışma alanı, bağlantı bilgilerini çalışma alanına bağlı depolama hesabında sağlanan Azure blob kapsayıcısına depolayan varsayılan bir veri deposuyla birlikte gelir. Aşağıdaki kod, yukarıda oluşturduğunuz kaza verilerini bu veri deposuna yükler.

```R
ds <- get_default_datastore(ws)

target_path <- "accidentdata"
upload_files_to_datastore(ds,
                          list("./accidents.Rd"),
                          target_path = target_path,
                          overwrite = TRUE)
```


## <a name="train-a-model"></a>Modeli eğitme

Bu öğretici için, uzaktan bilgi işlem kümenizi kullanarak yüklenen verilerinize bir lojistik regresyon modeli sığdırın. Bir iş göndermek için şunları yapmanız gerekir:

* Eğitim komut dosyasını hazırlayın
* Tahmin aracı oluşturma
* İşi gönderme

### <a name="prepare-the-training-script"></a>Eğitim komut dosyasını hazırlayın
Bu öğreticiyle `accidents.R` aynı dizinde sizin için bir eğitim komut dosyası sağlanmıştır. Eğitim için Azure Machine Learning'den yararlanmak için yapılan **eğitim komut dosyasında** aşağıdaki ayrıntılara dikkat edin:

* Eğitim komut dosyası, `-d` eğitim verilerini içeren dizini bulmak için bir bağımsız değişken alır. İşinizi daha sonra tanımlayıp gönderdiğinizde, bu bağımsız değişken için veri deposuna işaret esiniz. Azure ML, depolama klasörünü eğitim işi için uzak kümeye monte edecektir.
* Eğitim komut dosyası, Azure ML'deki çalışma kaydına metrik `log_metric_to_run()`olarak son doğruluğu kaydeder. Azure ML SDK, eğitim çalıştırmaları sırasında çeşitli ölçümleri günlüğe kaydetmek için bir dizi günlük API'si sağlar. Bu ölçümler kaydedilir ve deneme çalıştırma kaydında kalıcıdır. Ölçümlere daha sonra herhangi bir zamanda erişilebilir veya [stüdyoda](https://ml.azure.com)çalıştırılan ayrıntılar sayfasında görüntülenebilir. Tam günlük yöntemleri `log_*()`kümesi için [başvuruya](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-training-experimentation) bakın.
* Eğitim komut dosyası, modelinizi çıkış **adı**verilen bir dizine kaydeder. Klasör, `./outputs` Azure ML tarafından özel muamele alır. Eğitim sırasında, dosyaya `./outputs` yazılan dosyalar Azure ML tarafından otomatik olarak run kaydınıza yüklenir ve yapı teşekküller olarak devam eder. Eğitimli modeli `./outputs`kaydederek, çalışma bittikten sonra bile model dosyanıza erişebilir ve geri alabilirsiniz ve artık uzaktan eğitim ortamınıza erişememiş olursunuz.

### <a name="create-an-estimator"></a>Tahmin aracı oluşturma

Azure ML tahmincisi, bilgi işlem hedefinde bir eğitim komut dosyası yürütmek için gereken çalıştırılan yapılandırma bilgilerini kapsüller. Azure ML çalıştırmaları, belirtilen işlem hedefinde kapsayıcı işler olarak çalıştırılır. Varsayılan olarak, eğitim göreviniz için oluşturulmuş Docker görüntüsünde R, Azure ML SDK ve yaygın olarak kullanılan R paketleri yer alır. Burada yer alan varsayılan paketlerin tam listesine bakın.

Tahminci oluşturmak için tanımlayın:

* Eğitim için gerekli komut dosyalarınızı içeren`source_directory`dizin ( ). Bu dizindeki tüm dosyalar yürütme için küme düğümüne yüklenir. Dizin, eğitim komut dosyanızı ve gerekli ek komut dosyalarını içermelidir.
* Yürütülecek eğitim komut`entry_script`dosyası ( ).
* İşlem hedefi (`compute_target`), bu durumda daha önce oluşturduğunuz AmlCompute kümesi.
* Eğitim komut dosyasından gerekli`script_params`parametreler ( ). Azure ML, eğitim komut dosyanızı `Rscript`komut satırı komut dosyası olarak çalıştıracaktır. Bu öğreticide komut dosyasına bir bağımsız değişken belirtin, veri dizini montaj noktası, hangi ile `ds$path(target_path)`erişebilirsiniz .
* Eğitim için gerekli olan tüm ortam bağımlılıkları. Eğitim için oluşturulmuş varsayılan Docker görüntüsü,`caret`eğitim `e1071`komut `optparse`dosyasında gereken üç paketi (, ve) içerir.  Bu nedenle ek bilgi belirtmeniz gerekmez. Varsayılan olarak dahil olmayan R paketleri kullanıyorsanız, ek CRAN `cran_packages` paketleri eklemek için tahmincinin parametresini kullanın. Yapılandırılabilir seçeneklerin tam kümesi için başvuruya [`estimator()`](https://azure.github.io/azureml-sdk-for-r/reference/estimator.html) bakın.

```R
est <- estimator(source_directory = ".",
                 entry_script = "accidents.R",
                 script_params = list("--data_folder" = ds$path(target_path)),
                 compute_target = compute_target
                 )
```

### <a name="submit-the-job-on-the-remote-cluster"></a>Uzak kümedeki işi gönderme

Son olarak kümenizde çalışacak işi gönderin. `submit_experiment()`daha sonra çalıştır ile arabirim için kullandığınız bir Çalıştır nesnesi döndürür. Toplamda, ilk çalışma **yaklaşık 10 dakika**sürer. Ancak daha sonraki çalıştırmalar için, komut dosyası bağımlılıkları değişmediği sürece aynı Docker görüntüsü yeniden kullanılır.  Bu durumda, görüntü önbelleğe alınmış ve kapsayıcı başlatma süresi çok daha hızlıdır.

```R
run <- submit_experiment(exp, est)
```

Çalıştırmanın ayrıntılarını RStudio Viewer'da görüntüleyebilirsiniz. Sağlanan "Web Görünümü" bağlantısını tıklattığınızda, kullanıcı bir arada arama nızı izleyebileceğiniz Azure Machine Learning stüdyosuna ulaşabilirsiniz.

```R
view_run_details(run)
```

Model eğitimi arka planda olur. Daha fazla kod çalıştırmadan önce modelin eğitimini tamamlayana kadar bekleyin.

```R
wait_for_run_completion(run, show_output = TRUE)
```

Siz ve çalışma alanına erişimi olan iş arkadaşları paralel olarak birden çok deneme gönderebilirsiniz ve Azure ML işlem kümesindeki görevleri zamanlamayı alır. Kümeyi otomatik olarak birden çok düğüme ölçeklendirecek şekilde yapılandırabilir ve kuyrukta daha fazla bilgi işlem görevi olmadığında yeniden ölçeklendirebilirsiniz. Bu yapılandırma, ekiplerin bilgi işlem kaynaklarını paylaşması için uygun maliyetli bir yoldur.

## <a name="retrieve-training-results"></a>Eğitim sonuçlarını alma
Modeliniz eğitimini tamamladıktan sonra, günlüğe kaydedilmiş ölçümler ve son eğitilmiş model de dahil olmak üzere çalışma kaydında kalıcı olan işinizin yapıtlarına erişebilirsiniz.

### <a name="get-the-logged-metrics"></a>Günlüğe kaydedilmiş ölçümleri alın
Eğitim komut `accidents.R`dosyasında, modelinizden bir metrik günlüğe kaydetmişsinizdir: eğitim verilerindeki tahminlerin doğruluğu. Ölçümleri [stüdyoda](https://ml.azure.com)görebilir veya bunları aşağıdaki gibi yerel oturuma R listesi olarak ayıklayabilirsiniz:

```R
metrics <- get_run_metrics(run)
metrics
```

Birden çok deneme çalıştırıyorsanız (örneğin, farklı değişkenler, algoritmalar veya hiperparamers kullanarak), her çalıştırmadaki ölçümleri üretimde kullanacağınız modeli karşılaştırmak ve seçmek için kullanabilirsiniz.

### <a name="get-the-trained-model"></a>Eğitimli modeli alın
Eğitimli modeli alabilir ve yerel R oturumunuzdaki sonuçlara bakabilirsiniz. Aşağıdaki kod, model dosyasını `./outputs` içeren dizinin içeriğini karşıdan yükler.

```R
download_files_from_run(run, prefix="outputs/")
accident_model <- readRDS("outputs/model.rds")
summary(accident_model)
```

Tahmini ölüm olasılığında artışa katkıda bulunan bazı faktörler görüyorsunuz:

* daha yüksek darbe hızı 
* erkek sürücü
* yaşlı yolcu
* Yolcu

Daha düşük ölüm olasılıkları görüyorsunuz:

* hava yastıkları varlığı
* durum emniyet kemerleri
* frontal çarpışma 

Araç üretim yılı önemli bir etkiye sahip değildir.

Yeni öngörüler yapmak için bu modeli kullanabilirsiniz:

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

Modelinle, bir çarpışmadan ölüm tehlikesini tahmin edebilirsiniz. Modelinizi bir tahmin hizmeti olarak dağıtmak için Azure ML'yi kullanın. Bu eğitimde, web hizmetini [Azure Kapsayıcı Örnekleri'nde](https://docs.microsoft.com/azure/container-instances/) (ACI) dağıtmış olursunuz.

### <a name="register-the-model"></a>Modeli kaydedin

İlk olarak, indirdiğiniz modeli çalışma alanınıza ' ile kaydedin. [`register_model()`](https://azure.github.io/azureml-sdk-for-r/reference/register_model.html) Kayıtlı bir model herhangi bir dosya koleksiyonu olabilir, ancak bu durumda R modeli nesnesi yeterlidir. Azure ML dağıtım için kayıtlı modeli kullanır.

```R
model <- register_model(ws, 
                        model_path = "outputs/model.rds", 
                        model_name = "accidents_model",
                        description = "Predict probablity of auto accident")
```

### <a name="define-the-inference-dependencies"></a>Çıkarım bağımlılıklarını tanımlama
Modeliniz için bir web hizmeti oluşturmak için, öncelikle`entry_script`giriş değişken değerleri (JSON biçiminde) olarak alacak bir r komut dosyası (), bir puanlama komut dosyası oluşturmanız ve modelinizden bir tahmin çıktısı almanız gerekir. Bu öğretici için, sağlanan `accident_predict.R`puanlama dosyasını kullanın. Puanlama komut dosyası, `init()` modelinizi yükleyen ve giriş verilerini temel alan bir tahmin yapmak için modeli kullanan bir işlev döndüren bir yöntem içermelidir. Daha fazla ayrıntı için [belgelere](https://azure.github.io/azureml-sdk-for-r/reference/inference_config.html#details) bakın.

Ardından, komut dosyanızın paket bağımlılıkları için bir Azure ML **ortamı** tanımlayın. Bir ortamda, komut dosyanızın çalışması için gereken R paketlerini (CRAN'dan veya başka bir yerden) belirtirsiniz. Ayrıca, komut dosyanızın davranışını değiştirmek için başvuruda bulunabileceği ortam değişkenlerinin değerlerini de sağlayabilirsiniz. Varsayılan olarak, Azure ML eğitim için tahminciyle birlikte kullanılan varsayılan Docker görüntüsünü oluşturur. Öğreticinin özel gereksinimleri olmadığından, özel niteliklere sahip olmayan bir ortam oluşturun.

```R
r_env <- r_environment(name = "basic_env")
```

Bunun yerine dağıtım için kendi Docker resminizi `custom_docker_image` kullanmak istiyorsanız, parametreyi belirtin. Bir [`r_environment()`](https://azure.github.io/azureml-sdk-for-r/reference/r_environment.html) ortamı tanımlamak için yapılandırılabilir seçeneklerin tam kümesi için başvuruya bakın.

Artık puanlama komut dosyanızı ve ortam bağımlılıklarınızı kapsüllemek için bir **çıkarım config** oluşturmak için ihtiyacınız olan her şeye sahipsiniz.

```R
inference_config <- inference_config(
  entry_script = "accident_predict.R",
  environment = r_env)
```

### <a name="deploy-to-aci"></a>ACI'ye dağıtma
Bu eğitimde, hizmetinizi ACI'ye dağıtmış olursunuz. Bu kod, test ve hafif yükler için uygun olan gelen istekleri yanıtlamak için tek bir kapsayıcıyı karşılar. Ek [`aci_webservice_deployment_config()`](https://azure.github.io/azureml-sdk-for-r/reference/aci_webservice_deployment_config.html) yapılandırılabilir seçenekler için bkz. (Üretim ölçeğinde dağıtımlar için [Azure Kubernetes Hizmetine](https://azure.github.io/azureml-sdk-for-r/articles/deploy-to-aks/deploy-to-aks.html)de dağıtabilirsiniz.)

``` R
aci_config <- aci_webservice_deployment_config(cpu_cores = 1, memory_gb = 0.5)
```

Şimdi modelinizi bir web hizmeti olarak dağıtın. Dağıtım **birkaç dakika sürebilir.** 

```R
aci_service <- deploy_model(ws, 
                            'accident-pred', 
                            list(model), 
                            inference_config, 
                            aci_config)

wait_for_deployment(aci_service, show_output = TRUE)
```

## <a name="test-the-deployed-service"></a>Dağıtılan hizmeti test edin

Artık modeliniz bir hizmet olarak dağıtıldı, hizmeti R'den [`invoke_webservice()`](https://azure.github.io/azureml-sdk-for-r/reference/invoke_webservice.html)kullanarak test edebilirsiniz.  Tahmin etmek, JSON'a dönüştürmek ve hizmete göndermek için yeni bir veri kümesi sağlayın.

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

Ayrıca, WEB hizmetinin REST istemci çağrılarını kabul eden HTTP bitiş noktasını da alabilirsiniz. Bu bitiş noktasını web hizmetini sınamak veya bir uygulamaya entegre etmek isteyen herkesle paylaşabilirsiniz.

```R
aci_service$scoring_uri
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız olmadığında kaynakları silin. Hala kullanmayı planladığınız kaynakları silmeyin. 

Web hizmetini silin:
```R
delete_webservice(aci_service)
```

Kayıtlı modeli silin:
```R
delete_model(model)
```

İşlem kümesini silin:
```R
delete_compute(compute)
```

### <a name="delete-everything"></a>Her şeyi silme

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

Kaynak grubunu da tutabilir, ancak tek bir çalışma alanını silebilirsiniz. Çalışma alanı özelliklerini görüntüleyin ve **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

* R'deki ilk Azure Machine Learning denemenizi tamamladığınızda, [R için Azure Machine Learning SDK](https://azure.github.io/azureml-sdk-for-r/index.html)hakkında daha fazla bilgi edinin.

* Diğer *vinyet* klasörleri klasörlerdeki örneklerden R ile Azure Machine Learning hakkında daha fazla bilgi edinin.
