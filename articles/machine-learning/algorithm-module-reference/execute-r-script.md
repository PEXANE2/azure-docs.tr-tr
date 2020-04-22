---
title: 'Execute R Script: Modül Referans'
titleSuffix: Azure Machine Learning
description: R kodunu çalıştırmak için Azure Machine Learning'de Execute R Script modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/10/2020
ms.openlocfilehash: eb778c8d24639320b60927438de76a29de724ac2
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81684708"
---
# <a name="execute-r-script"></a>R Betiği yürütme

Bu makalede, Azure Machine Learning tasarımcınızda (önizleme) R kodunu çalıştırmak için **Execute R Script** modülünün nasıl kullanılacağı açıklanmaktadır.

R ile, şu anda şu anda şu anda şu anda şu gibi varolan modüller tarafından desteklenmeyen görevleri gerçekleştirebilirsiniz: 
- Özel veri dönüşümleri oluşturma
- Tahminleri değerlendirmek için kendi ölçümlerinizi kullanın
- Tasarımcıda bağımsız modül olarak uygulanmayan algoritmalar kullanarak modeller oluşturun

## <a name="r-version-support"></a>R sürüm desteği

Azure Machine Learning tasarımcısı, R'nin CRAN (Kapsamlı R Arşiv Ağı) dağıtımını kullanır. Şu anda kullanılan sürüm CRAN 3.5.1'dir.

## <a name="supported-r-packages"></a>Desteklenen R paketleri

R ortamı 100'den fazla paketle önceden yüklenir. Tam liste [için, önceden yüklenmiş R paketleri](#pre-installed-r-packages)bölümüne bakın.

Ayrıca, herhangi bir Execute **R Script** modülüne aşağıdaki kodu ekleyebilir ve yüklü paketleri görebilirsiniz.

```R
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  dataframe1 <- data.frame(installed.packages())
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
```

## <a name="installing-r-packages"></a>R paketlerinin yüklenmesi
Ek R paketleri yüklemek `install.packages()` için yöntemi kullanın. Paketler her Execute **R Script** modülü için yüklenir ve diğer **Execute R Script** modülleri arasında paylaşılmaz.

> [!NOTE]
> Lütfen cran deposunu örneğin`install.packages("zoo",repos = "http://cran.us.r-project.org")`

Bu örnek, Zoo'nun nasıl yüklenir:
```R
# R version: 3.5.1
# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# Please note that functions dependant on X11 library
# such as "View" are not supported because X11 library
# is not pre-installed.

# The entry point function MUST have two input arguments.
# If the input port is not connected, the corresponding
# dataframe argument will be null.
#   Param<dataframe1>: a R DataFrame
#   Param<dataframe2>: a R DataFrame
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  
  if(!require(zoo)) install.packages("zoo",repos = "http://cran.us.r-project.org")
  library(zoo)
  # Return datasets as a Named List
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
```
 > [!NOTE]
  > Tekrar yüklemeyi önlemek için paketi yüklemeden önce paketin zaten var olup olmadığını kontrol edin. Yukarıdaki `  if(!require(zoo)) install.packages("zoo",repos = "http://cran.us.r-project.org")` örnek koddaki gibi. Yineleme yükleme web hizmeti isteği zaman ağını neden olabilir.     

## <a name="upload-files"></a>Dosyaları karşıya yükleme
**Execute R Script,** Azure Machine Learning R SDK kullanarak dosya yüklemeyi destekler.

Aşağıdaki örnek, **Execute R Script'te**bir resim dosyasının nasıl yüklenir olduğunu gösterir:
```R

# R version: 3.5.1
# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# Please note that functions dependant on X11 library
# such as "View" are not supported because X11 library
# is not pre-installed.

# The entry point function MUST have two input arguments.
# If the input port is not connected, the corresponding
# dataframe argument will be null.
#   Param<dataframe1>: a R DataFrame
#   Param<dataframe2>: a R DataFrame
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")

  # Generate a jpeg graph
  img_file_name <- "rect.jpg"
  jpeg(file=img_file_name)
  example(rect)
  dev.off()

  upload_files_to_run(names = list(file.path("graphic", img_file_name)), paths=list(img_file_name))


  # Return datasets as a Named List
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
```

Boru hattı çalışması tamamlandıktan sonra, görüntüyü modülün sağ panelinde önizleyebilirsiniz

> [!div class="mx-imgBorder"]
> ![Yüklenen resim](media/module/upload-image-in-r-script.png)

## <a name="how-to-configure-execute-r-script"></a>Execute R Script nasıl yapılandırılır?

**Yürütme R Script** modülü, başlangıç noktası olarak kullanabileceğiniz örnek kodu içerir. Execute R **Script** modülünün yapılandırılması için, yürütmek için bir dizi giriş ve kod sağlayın.

![R modülü](media/module/execute-r-script.png)

Tasarımcıda depolanan veri kümeleri, bu modülle yüklendiğinde otomatik olarak R veri çerçevesine dönüştürülür.

1.  Yürütme **R Script** modüllerini ardınıza ekleyin.

  

1. Komut dosyasının gerektirdiği tüm girişleri bağlayın. Girişler isteğe bağlıdır ve veri ve ek R kodu içerebilir.

    * **Dataset1**: İlk girişi `dataframe1`. Giriş veri kümesi csv, TSV, ARFF olarak biçimlendirilmelidir veya bir Azure Machine Learning veri kümesi bağlayabilirsiniz.

    * **Dataset2**: İkinci girişi `dataframe2`. Bu veri kümesi aynı zamanda CSV, TSV, ARFF dosyası veya Azure Machine Learning veri kümesi olarak biçimlendirilmelidir.

    * **Komut Dosyası Paketi**: Üçüncü giriş ZIP dosyalarını kabul eder. Sıkıştırılabilen dosya birden çok dosya ve birden çok dosya türü içerebilir.

1. R **komut dosyası** metin kutusunda geçerli R komut dosyası yazın veya yapıştırın.

    > [!NOTE]
    > Lütfen komut dosyanızı yazarken çok dikkatli olun ve bildirilmemiş bir değişken veya alınmamış bir modül veya işlev kullanmak gibi sözdizimi hatası olmadığından emin olun. Ayrıca, bu belgenin sonunda önceden yüklenmiş paket listesine ekstra dikkat edin. Listede yer almayan paketleri kullanmak için lütfen komut dosyanıza`install.packages("zoo",repos = "http://cran.us.r-project.org")`
    
    > [!NOTE]
    > X11 kitaplığı önceden yüklenmediği için "Görünüm" gibi X11 kitaplığına bağımlı işlevler desteklenmez.
    
    Başlamanıza yardımcı olmak **için, R Script** metin kutusu, değiştirebileceğiniz veya değiştirebileceğiniz örnek kodla önceden doldurulur.
    
    ```R
    # R version: 3.5.1
    # The script MUST contain a function named azureml_main
    # which is the entry point for this module.

    # Please note that functions dependant on X11 library
    # such as "View" are not supported because X11 library
    # is not pre-installed.
    
    # The entry point function MUST have two input arguments.
    # If the input port is not connected, the corresponding
    # dataframe argument will be null.
    #   Param<dataframe1>: a R DataFrame
    #   Param<dataframe2>: a R DataFrame
    azureml_main <- function(dataframe1, dataframe2){
    print("R script run.")

    # If a zip file is connected to the third input port, it is
    # unzipped under "./Script Bundle". This directory is added
    # to sys.path.

    # Return datasets as a Named List
    return(list(dataset1=dataframe1, dataset2=dataframe2))
    }
    ```

 * Komut dosyası, bu `azureml_main`modülün giriş noktası olan adlandırılmış bir işlev içermelidir.

 * Giriş noktası işlevi iki giriş bağımsız `Param<dataframe1>` `Param<dataframe2>`değişkeni olmalıdır: ve , bu iki bağımsız değişken işlevde kullanılmadığında bile.

   > [!NOTE]
    > **Execute R Script** modülüne aktarılan veriler, `dataframe1` `dataframe2`Azure Machine Learning tasarımcısından farklı olan (tasarımcı başvurusu , `dataset1`olarak) `dataset2`olarak başvurulmuştür. Lütfen giriş verilerinin komut dosyanızda doğru şekilde yönlendirildiğinden emin olun.  
 
    > [!NOTE]
    >  Varolan R kodunun tasarımcı ardışık alanında çalışması için küçük değişiklikler gerekebilir. Örneğin, CSV biçiminde sağladığınız giriş verilerinin, kodunuzda kullanabilmek için açıkça bir veri kümesine dönüştürülmesi gerekir. R dilinde kullanılan veri ve sütun türleri de tasarımcıda kullanılan veri ve sütun türlerinden bazı yönlerden farklılık gösterir.

1.  **Rastgele Tohum**: Rastgele tohum değeri olarak R ortamı içinde kullanılacak bir değer yazın. Bu parametre R `set.seed(value)` kodunda arama eşdeğerdir.  

1. Boru hattını gönderin.  

## <a name="results"></a>Sonuçlar

**Execute R Script** modülleri birden çok çıktı döndürebilir, ancak R veri çerçevesi olarak sağlanmalıdır. Veri çerçeveleri, diğer modüllerle uyumluluk için tasarımcıdaki veri kümelerine otomatik olarak dönüştürülür.

Standart iletiler ve R'den gelen hatalar modülün günlüğüne döndürülür.

Sonuçları R komut dosyasında yazdırmanız gerekiyorsa, yazdırılan sonuçları **70_driver_log** **çıktılar+günlükler** sekmesi altında modülün sağ panelinde bulabilirsiniz.

## <a name="sample-scripts"></a>Örnek komut dosyaları

Özel R komut dosyalarını kullanarak ardışık hattınızı genişletmenin birçok yolu vardır.  Bu bölümde, ortak görevler için örnek kod sağlar.


### <a name="add-r-script-as-an-input"></a>Giriş olarak R komut dosyası ekleme

**Execute R Script** modülü rasgele R komut dosyası dosyalarını giriş olarak destekler. Bunu yapmak için, zip dosyasının bir parçası olarak çalışma alanınıza yüklenmesi gerekir.

1. Çalışma alanınıza R kodu içeren bir ZIP dosyası yüklemek için **Datasets** varlık sayfasına gidin, **veri kümesi oluştur'u**tıklatın ve ardından Yerel dosyadan ve **Dosya** veri kümesi türü **seçeneğinden'i** seçin.  

1. Sıkıştırılan dosyanın sol modül ağacında **Veri kümeleri** kategorisi altında **Veri kümelerim** listesinde kullanılabildiğini doğrulayın.

1.  Veri kümesini **Komut Dosyası Paketi** giriş bağlantı noktasına bağlayın.

1. ZIP dosyasında bulunan tüm dosyalar, ardışık hatlar hattı çalışma süresi sırasında kullanılabilir. 

    Komut dosyası paketi dosyasında bir dizin yapısı varsa, yapı korunur. Ancak, dizin **./Script Paketini** yola hazırlamak için kodunuzu değiştirmeniz gerekir.

### <a name="process-data"></a>Veri işleme

Aşağıdaki örnek, giriş verilerinin nasıl ölçeklendirilip normalleştirilenini gösterir:

```R
# R version: 3.5.1
# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# Please note that functions dependant on X11 library
# such as "View" are not supported because X11 library
# is not pre-installed.

# The entry point function MUST have two input arguments.
# If the input port is not connected, the corresponding
# dataframe argument will be null.
#   Param<dataframe1>: a R DataFrame
#   Param<dataframe2>: a R DataFrame
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  # If a zip file is connected to the third input port, it is
  # unzipped under "./Script Bundle". This directory is added
  # to sys.path.
  series <- dataframe1$width
  # find the maximum and minimum values of width column in dataframe1
  max_v <- max(series)
  min_v <- min(series)
  # calculate the scale and bias
  scale <- max_v - min_v
  bias <- min_v / dis
  # apply min-max normalizing
  dataframe1$width <- dataframe1$width / scale - bias
  dataframe2$width <- dataframe2$width / scale - bias
  # Return datasets as a Named List
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
 ```

### <a name="read-a-zip-file-as-input"></a>Giriş olarak ZIP dosyasını okuma

Bu örnek, Zip dosyasındaki bir veri kümesinin Execute **R Script** modülüne giriş olarak nasıl kullanılacağını gösterir.

1. CSV biçiminde veri dosyası oluşturun ve adını "mydatafile.csv".
1. Bir ZIP dosyası oluşturun ve CSV dosyasını arşive ekleyin.
1. Sıkıştırılmış dosyayı Azure Machine Learning çalışma alanınıza yükleyin. 
1. Ortaya çıkan veri kümesini **Execute R Script** modülünüzün **ScriptBundle** girişine bağlayın.
1. Sıkıştırılmış dosyadaki CSV verilerini okumak için aşağıdaki kodu kullanma.

```R
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  mydataset<-read.csv("./Script Bundle/mydatafile.csv",encoding="UTF-8");  
  # Return datasets as a Named List
  return(list(dataset1=mydataset, dataset2=dataframe2))
}
```

### <a name="replicate-rows"></a>Satırları çoğaltma

Bu örnek, örneği dengelemek için bir veri kümesindeki pozitif kayıtların nasıl çoğaltılacağını gösterir:

```R
azureml_main <- function(dataframe1, dataframe2){
  data.set <- dataframe1[dataframe1[,1]==-1,]  
  # positions of the positive samples
  pos <- dataframe1[dataframe1[,1]==1,]
  # replicate the positive samples to balance the sample  
  for (i in 1:20) data.set <- rbind(data.set,pos)  
  row.names(data.set) <- NULL
  # Return datasets as a Named List
  return(list(dataset1=data.set, dataset2=dataframe2))
}
```

### <a name="pass-r-objects-between-execute-r-script-modules"></a>Execute R Script modülleri arasında R nesnelerini geçirin

İç serileştirme mekanizmasını kullanarak R nesnelerini **Execute R Script** modülü örnekleri arasında geçirebilirsiniz. Bu örnek, iki **Execute R Script** `A` modülü arasında adlandırılmış R nesnesini taşımak istediğinizi varsayar.

1. İlk **Yürütme R Script** modülunu ardışık noktanıza ekleyin ve modülün çıktı Veri Tablosunda sütun olarak serileştirilmiş bir nesne `A` oluşturmak için R **Script** metin kutusuna aşağıdaki kodu yazın:  
  
    ```R
    azureml_main <- function(dataframe1, dataframe2){
      print("R script run.")
      # some codes generated A
      
      serialized <- as.integer(serialize(A,NULL))  
      data.set <- data.frame(serialized,stringsAsFactors=FALSE)

      return(list(dataset1=data.set, dataset2=dataframe2))
    }
    ```

    Serileştirme işlevi, tasarımcı tarafından desteklenmeyen Verileri R `Raw` biçiminde çıktırdığından, yineleyici türüne açık dönüştürme yapılır.

1. **Execute R Script** modülünün ikinci bir örneğini ekleyin ve önceki modülün çıkış noktasına bağlayın.

1. Giriş Veri Tablosu'ndan nesne `A` ayıklamak için R **Script** metin kutusuna aşağıdaki kodu yazın. 

    ```R
    azureml_main <- function(dataframe1, dataframe2){
      print("R script run.")
      A <- unserialize(as.raw(dataframe1$serialized))  
      # Return datasets as a Named List
      return(list(dataset1=dataframe1, dataset2=dataframe2))
    }
    ```

## <a name="pre-installed-r-packages"></a>Önceden yüklenmiş R Paketleri

Kullanılabilir önceden yüklenmiş R Paketlerinin geçerli listesi:

|              |            | 
|--------------|------------| 
| Paket      | Sürüm    | 
| askpass      | 1.1        | 
| assertthat   | 0.2.1      | 
| backports    | 1.1.4      | 
| base         | 3.5.1      | 
| base64enc    | 0.1-3      | 
| BH           | 1.69.0-1   | 
| bindr        | 0.1.1      | 
| bindrcpp     | 0.2.2      | 
| bitops       | 1.0-6      | 
| başlatma         | 1.3-22     | 
| broom        | 0.5.2      | 
| callr        | 3.2.0      | 
| caret        | 6.0-84     | 
| caTools      | 1.17.1.2   | 
| cellranger   | 1.1.0      | 
| sınıf        | 7.3-15     | 
| cli          | 1.1.0      | 
| kliptör        | 0.6.0      | 
| cluster      | 2.0.7-1    | 
| codetools    | 0.2-16     | 
| colorspace   | 1.4-1      | 
|  derleyicisi     | 3.5.1      | 
| crayon       | 1.3.4      | 
| curl         | 3.3        | 
| data.table   | 1.12.2     | 
| veri kümeleri     | 3.5.1      | 
| DBI          | 1.0.0      | 
| dbplyr       | 1.4.1      | 
| digest       | 0.6.19     | 
| dplyr        | 0.7.6      | 
| e1071        | 1.7-2      | 
| değerlendir     | 0.14       | 
| fansi        | 0.4.0      | 
| forcats      | 0.3.0      | 
| foreach      | 1.4.4      | 
| foreign      | 0.8-71     | 
| Fs           | 1.3.1      | 
| gdata        | 2.18.0     | 
| Generics     | 0.0.2      | 
| ggplot2      | 3.2.0      | 
| glmnet       | 2.0-18     | 
| glue         | 1.3.1      | 
| gower        | 0.2.1      | 
| gplots       | 3.0.1.1    | 
| grafikler     | 3.5.1      | 
| grDevices    | 3.5.1      | 
| grid         | 3.5.1      | 
| gtable       | 0.3.0      | 
| gtools       | 3.8.1      | 
| haven        | 2.1.0      | 
| highr        | 0,8        | 
| hms          | 0.4.2      | 
| htmltools    | 0.3.6      | 
| httr         | 1.4.0      | 
| ipred        | 0.9-9      | 
| iterators    | 1.0.10     | 
| jsonlite     | 1.6        | 
| KernSmooth   | 2.23-15    | 
| knitr        | 1,23       | 
| labeling     | 0.3        | 
| lattice      | 0.20-38    | 
| lava         | 1.6.5      | 
| lazyeval     | 0.2.2      | 
| lubridate    | 1.7.4      | 
| magrittr     | 1,5        | 
| markdown     | 1          | 
| MASS         | 7.3-51.4   | 
| Matrix       | 1.2-17     | 
| methods      | 3.5.1      | 
| mgcv         | 1.8-28     | 
| mime         | 0.7        | 
| ModelMetrics | 1.2.2      | 
| modelr       | 0.1.4      | 
| munsell      | 0.5.0      | 
| nlme         | 3.1-140    | 
| nnet         | 7.3-12     | 
| numDeriv     | 2016.8-1.1 | 
| openssl      | 1.4        | 
| parallel     | 3.5.1      | 
| pillar       | 1.4.1      | 
| pkgconfig    | 2.0.2      | 
| plogr        | 0.2.0      | 
| plyr         | 1.8.4      | 
| prettyunits  | 1.0.2      | 
| processx     | 3.3.1      | 
| prodlim      | 2018.04.18 | 
| progress     | 1.2.2      | 
| Ps           | 1.3.0      | 
| purrr        | 0.3.2      | 
| quadprog     | 1.5-7      | 
| quantmod     | 0.4-15     | 
| R6           | 2.4.0      | 
| randomForest | 4.6-14     | 
| RColorBrewer | 1.1-2      | 
| Rcpp         | 1.0.1      | 
| RcppRoll     | 0.3.0      | 
| readr        | 1.3.1      | 
| readxl       | 1.3.1      | 
| recipes      | 0.1.5      | 
| rematch      | 1.0.1      | 
| reprex       | 0.3.0      | 
| reshape2     | 1.4.3      | 
| reticulate   | 1.12       | 
| rlang        | 0.4.0      | 
| rmarkdown    | 1.13       | 
| ROCR         | 1.0-7      | 
| rpart        | 4.1-15     | 
| rstudioapi   | 0.1        | 
| rvest        | 0.3.4      | 
| scales       | 1.0.0      | 
| selectr      | 0.4-1      | 
| spatial      | 7.3-11     | 
| splines      | 3.5.1      | 
| SQUAREM      | 2017.10-1  | 
| stats        | 3.5.1      | 
| stats4       | 3.5.1      | 
| stringi      | 1.4.3      | 
| stringr      | 1.3.1      | 
| survival     | 2.44-1.1   | 
| sys          | 3,2        | 
| tcltk        | 3.5.1      | 
| tibble       | 2.1.3      | 
| tidyr        | 0.8.3      | 
| tidyselect   | 0.2.5      | 
| tidyverse    | 1.2.1      | 
| timeDate     | 3043.102   | 
| tinytex      | 0.13       | 
| araçlar        | 3.5.1      | 
| tseries      | 0.10-47    | 
| TTR          | 0.23-4     | 
| utf8         | 1.1.4      | 
| utils        | 3.5.1      | 
| vctrs        | 0.1.0      | 
| viridisLite  | 0.3.0      | 
| whisker      | 0.3-2      | 
| withr        | 2.1.2      | 
| xfun         | 0,8        | 
| xml2         | 1.2.0      | 
| xts          | 0.11-2     | 
| yaml         | 2.2.0      | 
| zeallot      | 0.1.0      | 
| zoo          | 1.8-6      | 

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 
