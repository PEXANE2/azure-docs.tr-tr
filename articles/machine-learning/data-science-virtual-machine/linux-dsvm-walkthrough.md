---
title: Linux'u keşfedin
titleSuffix: Azure Data Science Virtual Machine
description: Linux Veri Bilimi Sanal Makine'yi kullanarak çeşitli yaygın veri bilimi görevlerini nasıl tamamlayısılamayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 04/02/2020
ms.openlocfilehash: 38088503fee016651a8c1c9a1f57ad4bbe102456
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81257156"
---
# <a name="data-science-with-a-linux-data-science-virtual-machine-in-azure"></a>Azure'da Linux Veri Bilimi Sanal Makine ile veri bilimi

Bu walkthrough, Linux Veri Bilimi Sanal Makinesini (DSVM) kullanarak birkaç yaygın veri bilimi görevini nasıl tamamlayabileceğinizi gösterir. Linux DSVM, Azure'da bulunan ve veri analitiği ve makine öğrenimi için yaygın olarak kullanılan araçlar koleksiyonuyla önceden yüklenmiş sanal bir makine görüntüsüdür. Temel yazılım bileşenleri [Hükmü Linux Veri Bilimi Sanal Makine](linux-dsvm-intro.md)ayrıntılı vardır. DSVM görüntüsü, araçların her birini ayrı ayrı yüklemek ve yapılandırmak zorunda kalmadan dakikalar içinde veri bilimi yapmaya başlamayı kolaylaştırır. Gerekirse DSVM'yi kolayca ölçeklendirebilir ve kullanılmadığı zaman durdurabilirsiniz. DSVM kaynağı hem elastik hem de uygun maliyetlidir.

Bu iznin gösterdiği veri bilimi [görevleri, Ekip Veri Bilimi Süreci nedir?](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) Ekip Veri Bilimi Süreci, veri bilimciekiplerinin akıllı uygulamalar oluşturma yaşam döngüsü üzerinde etkin bir şekilde işbirliği yapmasına yardımcı olan sistematik bir veri bilimi yaklaşımıdır. Veri bilimi süreci aynı zamanda bir birey tarafından takip edilebilen veri bilimi için yinelemeli bir çerçeve sağlar.

Bu izbarada, [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) veri kümesini analiz ederiz. Spambase, spam veya jambon (spam değil) olarak işaretlenmiş bir e-posta kümesidir. Spambase ayrıca e-postaların içeriği hakkında bazı istatistikler içerir. Daha sonra iznin içinde istatistikler hakkında konuşacağız.

## <a name="prerequisites"></a>Ön koşullar

Linux DSVM kullanamadan önce aşağıdaki ön koşullara sahip olmalısınız:

* **Azure aboneliği.** Azure aboneliği almak için [bkz.](https://azure.microsoft.com/free/)
* [**Linux Veri Bilimi Sanal Makine**](https://azure.microsoft.com/marketplace/apps/microsoft-dsvm.ubuntu-1804). Sanal makine sağlama hakkında bilgi için, [Linux Veri Bilimi Sanal Makine Hükmü'ne](linux-dsvm-intro.md)bakın.
* [**X2Go**](https://wiki.x2go.org/doku.php) bilgisayarınızda açık bir XFCE oturumu ile yüklü. Daha fazla bilgi için [X2Go istemcisini yükleyin ve yapılandırın.](linux-dsvm-intro.md#x2go)
* Daha yumuşak bir kaydırma deneyimi için, DSVM Firefox web `gfx.xrender.enabled` tarayıcısında, `about:config`bayrağı . [Daha fazla bilgi edinin](https://www.reddit.com/r/firefox/comments/4nfmvp/ff_47_unbearable_slow_over_remote_x11/). Ayrıca ayar `mousewheel.enable_pixel_scrolling` `False`düşünün. [Daha fazla bilgi edinin](https://support.mozilla.org/questions/981140).
* **Azure Machine Learning hesabı**. Zaten hesabınız yoksa, [Azure Machine Learning ana sayfasında](https://azure.microsoft.com/free/services/machine-learning//)yeni bir hesap için kaydolun.

## <a name="download-the-spambase-dataset"></a>Spambase veri kümesini indirin

[Spambase](https://archive.ics.uci.edu/ml/datasets/spambase) veri kümesi, 4.601 örnek içeren nispeten küçük bir veri kümesidir. Veri kümesi, kaynak gereksinimlerini mütevazı tuttuğundan DSVM'nin bazı temel özelliklerini göstermek için kullanışlı bir boyuttır.

> [!NOTE]
> Bu gözden geçirme, D2 v2 boyutunda bir Linux DSVM (Ubuntu 18.04 Edition) kullanılarak oluşturulmuştur. Bu izbiste gösterilen yordamları tamamlamak için bu boyutta bir DSVM kullanabilirsiniz.

Daha fazla depolama alanına ihtiyacınız varsa, ek diskler oluşturabilir ve bunları DSVM'nize ekleyebilirsiniz. Diskler kalıcı Azure depolama sını kullanır, bu nedenle sunucu yeniden boyutlandırma nedeniyle yeniden sağlanmış veya kapatılmış olsa bile verileri korunur. Bir disk eklemek ve DSVM'nize eklemek [için, Linux VM'ye disk ekle'deki](../../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)adımları tamamlayın. Disk ekleme adımları, DSVM'de zaten yüklü olan Azure CLI'yi kullanır. Adımları DSVM'nin kendisinden tamamen tamamlayabilirsiniz. Depolamayı artırmak için başka bir seçenek de [Azure Dosyaları'nı](../../storage/files/storage-how-to-use-files-linux.md)kullanmaktır.

Verileri indirmek için bir terminal penceresi açın ve ardından şu komutu çalıştırın:

```bash
wget --no-check-certificate https://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data
```

İndirilen dosyada üstbilgi satırı yok. Üstbilgisi olan başka bir dosya oluşturalım. Uygun üstbilgi içeren bir dosya oluşturmak için bu komutu çalıştırın:

```bash
echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers
```

Ardından, iki dosyayı birleştirin:

```bash
cat spambase.data >> headers
mv headers spambaseHeaders.data
```

Veri kümesinde her e-posta için çeşitli istatistik türleri vardır:

* **Freq\_\_WORD sözcüğü** gibi sütunlar, WORD ile *eşleşen*e-postadaki sözcüklerin yüzdesini gösterir. Örneğin, **kelime\_freq\_yapmak** **1**ise, o zaman e-postatüm kelimelerin% 1 *yapıldı.*
* **Char\_freq\_CHAR** gibi sütunlar *CHAR*olan e-postadaki tüm karakterlerin yüzdesini gösterir.
* **büyük\_\_harf\_** dizisinin en uzun uzunluğudur.
* **büyük\_\_harf\_uzunluğu ortalaması,** büyük harflerin tüm dizilerinin ortalama uzunluğudur.
* **büyük\_\_harf\_tüm** dizilerinin toplam uzunluğutoplamıdır.
* **spam,** e-postanın spam olarak kabul edilip edilmediğini gösterir (1 = spam, 0 = spam değil).

## <a name="explore-the-dataset-by-using-r-open"></a>R Open'ı kullanarak veri kümesini keşfedin

Verileri inceleyelim ve R kullanarak bazı temel makine öğrenimi yapalım. DSVM, [Microsoft R Open](https://mran.revolutionanalytics.com/open/) önceden yüklenmiş olarak gelir. R'nin önceden yüklenmiş sürümündeki çok iş parçacığı lı matematik kitaplıkları, tek iş parçacığı sürümlerinden daha iyi performans sunar. R Open, CRAN paket deposunun anlık görüntüsü yle de tekrarlanabilirlik sağlar.

Bu izbarada kullanılan kod örneklerinin kopyalarını almak için, Azure-Machine-Learning-Data-Science deposunu klonlamak için git'i kullanın. Git, DSVM'ye önceden yüklenmiş. Git komut satırında çalıştırın:

```bash
git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git
```

Terminal penceresini açın ve R etkileşimli konsolunda yeni bir R oturumu başlatın. Ayrıca, DSVM'ye önceden yüklenmiş olan RStudio'yu da kullanabilirsiniz.

Verileri almak ve ortamı ayarlamak için:

```R
data <- read.csv("spambaseHeaders.data")
set.seed(123)
```

Her sütunla ilgili özet istatistikleri görmek için:

```R
summary(data)
```

Verilerin farklı bir görünümü için:

```R
str(data)
```

Bu görünüm, her değişkenin türünü ve veri kümesindeki ilk birkaç değeri gösterir.

**Spam** sütunu tamsayı olarak okundu, ancak aslında kategorik bir değişken (veya faktör) bulunuyor. Türünü ayarlamak için:

```R
data$spam <- as.factor(data$spam)
```

Bazı keşif analizi yapmak için, DSVM'ye önceden yüklenmiş R için popüler bir grafik kitaplığı olan [ggplot2](https://ggplot2.tidyverse.org/) paketini kullanın. Daha önce görüntülenen özet verilere dayanarak, ünlem işareti karakterinin sıklığına ilişkin özet istatistiklerimiz var. Aşağıdaki komutları çalıştırarak bu frekansları burada çizelim:

```R
library(ggplot2)
ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)
```

Sıfır çubuğu çizimi çarpıttığı için, onu ortadan kaldıralım:

```R
email_with_exclamation = data[data$char_freq_exclamation > 0, ]
ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)
```

İlginç görünen 1'in üzerinde önemsiz bir yoğunluk vardır. Sadece bu verilere bakalım:

```R
ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)
```

Sonra, spam karşı jambon tarafından bölün:

```R
ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
geom_density(lty=3) +
geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
xlab("spam") +
ggtitle("Distribution of spam \nby frequency of !") +
labs(fill="spam", y="Density")
```

Bu örnekler, benzer çizimler yapmak ve diğer sütunlarda verileri keşfetmek yardımcı olmalıdır.

## <a name="train-and-test-a-machine-learning-model"></a>Bir makine öğrenme modelini eğitin ve test edin

Veri setindeki e-postaları spam veya jambon içeren olarak sınıflandırmak için birkaç makine öğrenimi modeli eğitelim. Bu bölümde, bir karar ağacı modeli ve rasgele bir orman modeli eğitiyoruz. Daha sonra, tahminlerin doğruluğunu test ediyoruz.

> [!NOTE]
> Aşağıdaki kodda kullanılan *rpart* (Recursive Partitioning and Regresyon Ağaçları) paketi Zaten DSVM yüklü.

İlk olarak, veri kümesini eğitim kümelerine ve test kümelerine bölelim:

```R
rnd <- runif(dim(data)[1])
trainSet = subset(data, rnd <= 0.7)
testSet = subset(data, rnd > 0.7)
```

Ardından, e-postaları sınıflandırmak için bir karar ağacı oluşturun:

```R
require(rpart)
model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
plot(model.rpart)
text(model.rpart)
```

İşte sonuç:

![Oluşturulan karar ağacının diyagramı](./media/linux-dsvm-walkthrough/decision-tree.png)

Eğitim kümesinde ne kadar iyi performans gösterdiğini belirlemek için aşağıdaki kodu kullanın:

```R
trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
accuracy <- sum(diag(t))/sum(t)
accuracy
```

Test kümesinde ne kadar iyi performans gösterdiğini belirlemek için:

```R
testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
accuracy <- sum(diag(t))/sum(t)
accuracy
```

Rastgele bir orman modeli de deneyelim. Rastgele ormanlar karar ağaçları çok sayıda tren ve tüm bireysel karar ağaçlarından sınıflandırmalar modu bir sınıf çıktı. Daha güçlü bir makine öğrenme yaklaşımı sağlarlar, çünkü bir karar ağacı modelinin eğitim veri kümesini aşırı yakışma eğilimini düzeltirler.

```R
require(randomForest)
trainVars <- setdiff(colnames(data), 'spam')
model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
accuracy <- sum(diag(t))/sum(t)
accuracy
```

<a name="deep-learning"></a>

## <a name="deep-learning-tutorials-and-walkthroughs"></a>Derin öğrenme eğitimleri ve walkthroughs

Çerçeve tabanlı örneklere ek olarak, kapsamlı bir dizi gözden geçirme de sağlanır. Bu izbolar, görüntü ve metin/dil anlama gibi alanlarda derin öğrenme uygulamaları geliştirmenizi hızlandırarak başlatmanıza yardımcı olur.

- [Farklı çerçevelerde sinir ağları nın çalıştırılması](https://github.com/ilkarman/DeepLearningFrameworks): Kodu bir çerçeveden diğerine nasıl geçirilenleri gösteren kapsamlı bir gözden geçirme. Ayrıca, model ve çalışma zamanı performansının çerçeveler arasında nasıl karşılaştırılabildiğini de gösterir. 

- [Görüntülerdeki ürünleri algılamak için uçtan uca bir çözüm oluşturma kılavuzu](https://github.com/Azure/cortana-intelligence-product-detection-from-images): Görüntü algılama, görüntülerdeki nesneleri bulup sınıflandırabilen bir tekniktir. Teknoloji birçok gerçek iş alanlarında büyük ödüller getirmek için potansiyele sahiptir. Örneğin, perakendeciler bu tekniği, müşterinin raftan hangi ürünü aldığını belirlemek için kullanabilir. Bu bilgiler sırayla, mağazaların ürün envanterini yönetmesine yardımcı olur. 

- [Ses için derin öğrenme](https://blogs.technet.microsoft.com/machinelearning/2018/01/30/hearing-ai-getting-started-with-deep-learning-for-audio-on-azure/): Bu öğretici [nasıl kentsel sesler dataset](https://urbansounddataset.weebly.com/)ses olay algılama için derin öğrenme modeli tren gösterir. Öğretici, ses verileriyle nasıl çalışılabilenlere genel bir bakış sağlar.

- [Metin belgelerinin sınıflandırılması](https://github.com/anargyri/lstm_han): Bu izbis, iki farklı sinir ağı mimarisinin nasıl inşa edilip eğitilenolduğunu gösterir: Hiyerarşik Dikkat Ağı ve Uzun Kısa Süreli Bellek (LSTM). Bu sinir ağları metin belgelerini sınıflandırmak için derin öğrenme için Keras API'yi kullanır. Keras en popüler derin öğrenme çerçeveleri üç bir ön uç: Microsoft Cognitive Toolkit, TensorFlow ve Theano.

## <a name="other-tools"></a>Diğer araçlar

Geri kalan bölümler, Linux DSVM'de yüklenen bazı araçların nasıl kullanılacağını gösterir. Biz bu araçları tartışmak:

* XGBoost
* Python
* JupyterHub
* Rattle
* PostgreSQL ve SQuirreL SQL
* SQL Server Veri Ambarı

### <a name="xgboost"></a>XGBoost

[XGBoost](https://xgboost.readthedocs.org/en/latest/) hızlı ve doğru artırılmış ağaç uygulaması sağlar.

```R
require(xgboost)
data <- read.csv("spambaseHeaders.data")
set.seed(123)

rnd <- runif(dim(data)[1])
trainSet = subset(data, rnd <= 0.7)
testSet = subset(data, rnd > 0.7)

bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

pred <- predict(bst, data.matrix(testSet[, 0:57]))
accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
print(paste("test accuracy = ", accuracy))
```

XGBoost ayrıca Python'dan veya bir komut satırından da arayabilir.

### <a name="python"></a>Python

Python gelişimi için Anaconda Python dağılımları 3.5 ve 2.7 DSVM'ye yüklenir.

> [!NOTE]
> Anaconda dağılımı [Conda](https://conda.pydata.org/docs/index.html)içerir. Conda'yı, farklı sürümler veya paketler yüklü özel Python ortamları oluşturmak için kullanabilirsiniz.

Bazı spambase veri kümesini okuyalım ve Scikit-learn'teki destek vektör makineleriyle e-postaları sınıflandıralım:

```Python
import pandas
from sklearn import svm
data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
X = data.ix[:, 0:57]
y = data.ix[:, 57]
clf = svm.SVC()
clf.fit(X, y)
```

Tahminlerde bulunmak için:

```Python
clf.predict(X.ix[0:20, :])
```

Azure Machine Learning bitiş noktasının nasıl yayımlandırış gösteriş yapılacağını göstermek için daha temel bir model yapalım. R modelini daha önce yayınladığımızda kullandığımız üç değişkeni kullanacağız:

```Python
X = data[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
y = data.ix[:, 57]
clf = svm.SVC()
clf.fit(X, y)
```

Modeli Azure Machine Learning olarak yayımlamak için:

```Python
# Publish the model.
workspace_id = "<workspace-id>"
workspace_token = "<workspace-token>"
from azureml import services
@services.publish(workspace_id, workspace_token)
@services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
@services.returns(int) # 0 or 1
def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
    inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
    return clf.predict(inputArray)

# Get some info about the resulting model.
predictSpam.service.url
predictSpam.service.api_key

# Call the model
predictSpam.service(1, 1, 1)
```

> [!NOTE]
> Bu seçenek yalnızca Python 2.7 için kullanılabilir. Python 3.5'te henüz desteklenmedi. Çalıştırmak için **/anaconda/bin/python2.7**kullanın.

### <a name="jupyterhub"></a>JupyterHub

DSVM'deki Anaconda dağılımı, Python, R veya Julia kod ve çözümlemesi paylaşımı için bir çapraz platform ortamı olan Jupyter Notebook ile birlikte gelir. Jupyter Notebook'a JupyterHub üzerinden erişilir. Yerel Linux kullanıcı adınızı ve şifrenizi https://\<DSVM DNS adı\>veya IP adresi :8000/de kullanarak oturum açın. JupyterHub için tüm yapılandırma dosyaları / vb / jupyterhub bulunur.

> [!NOTE]
> Geçerli çekirdekteki bir Jupyter Notebook'tan Python Paket Yöneticisi'ni `pip` (komut yoluyla) kullanmak için kod hücresinde şu komutu kullanın:
>
>   ```Python
>    import sys
>    ! {sys.executable} -m pip install numpy -y
>   ```
> 
> Geçerli çekirdekteki bir Jupyter `conda` Notebook'tan Gelen Conda yükleyicisini (komut yoluyla) kullanmak için bu komutu bir kod hücresinde kullanın:
>
>   ```Python
>    import sys
>    ! {sys.prefix}/bin/conda install --yes --prefix {sys.prefix} numpy
>   ```

DSVM'de zaten birkaç örnek dizüstü bilgisayar yüklenmiştir:

* Örnek Python dizüstü bilgisayarlar:
  * [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb)
  * [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb)
* Örnek R dizüstü bilgisayar:
  * [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) 

> [!NOTE]
> Julia dili, Linux DSVM'deki komut satırından da kullanılabilir.

### <a name="rattle"></a>Rattle

[Çıngırak](https://cran.r-project.org/web/packages/rattle/index.html) *(R* *A*nalytical *T*ool *T*o *L*asily *E*kazanmak) veri madenciliği için grafiksel bir R aracıdır. Rattle, verileri yüklemeyi, keşfetmeyi ve dönüştürmeyi ve modelleri oluşturmayı ve değerlendirmeyi kolaylaştıran sezgisel bir arayüze sahiptir. [Çıngırak: R için Veri Madenciliği GUI](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) Rattle özelliklerini gösteren bir walkthrough sağlar.

Bu komutları çalıştırarak Rattle'ı yükleyin ve başlatın:

```R
if(!require("rattle")) install.packages("rattle")
require(rattle)
rattle()
```

> [!NOTE]
> Rattle'ı DSVM'ye yüklemenize gerek yok. Ancak, Rattle açıldığında ek paketler yüklemeniz istenebilir.

Rattle sekme tabanlı bir arayüz kullanır. Sekmelerin çoğu, veri yükleme veya verileri keşfetme gibi [Ekip Veri Bilimi Süreci'ndeki](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)adımlara karşılık gelir. Veri bilimi süreci sekmeler aracılığıyla soldan sağa akar. Son sekme, Rattle tarafından çalıştırılabilen R komutlarının bir günlüğünü içerir.

Veri kümesini yüklemek ve yapılandırmak için:

1. Dosyayı yüklemek için **Veri** sekmesini seçin.
1. **Filename'nin**yanındaki seçiciyi seçin ve ardından **spambaseHeaders.data'yı**seçin.
1. Dosyayı yüklemek için. Yürüt'ün'u seçin. **Execute** Tanımlanan veri türü de dahil olmak üzere her sütunun bir özetini görmeniz gerekir; bir giriş, bir hedef veya başka bir değişken türü olup olmadığı; ve benzersiz değerlerin sayısı.
1. Rattle **spam** sütunu doğru bir şekilde hedef olarak tanımlamıştır. **İstenmeyen posta** sütununa gidin ve ardından **Hedef Veri Türünü** **Kategorik**olarak ayarlayın.

Verileri keşfetmek için:

1. **Keşfet** sekmesini seçin.
1. Değişken türleri ve bazı özet istatistikleri hakkındaki bilgileri görmek için **Özet** > Yürüt'ün'u seçin.**Execute**
1. Her değişkenle ilgili diğer istatistik türlerini görüntülemek **için, Açıkla** veya **Temel Bilgiler**gibi diğer seçenekleri seçin.

Anlayışlı çizimler oluşturmak için **Keşfet** sekmesini de kullanabilirsiniz. Verilerin histogramını çizmek için:

1. **Dağıtımları**seçin.
1. word_freq_remove **word_freq_remove** ve **word_freq_you**için **Histogram'ı**seçin.
1. **Yürüt**’ü seçin. Her iki yoğunluk çizimini de tek bir grafik penceresinde görmeniz gerekir, burada _göründüğünüz_ sözcüğün e-postalarda _kaldırmaktan_çok daha sık göründüğü açıktır.

**Korelasyon** çizimleri de ilginçtir. Bir çizim oluşturmak için:

1. **Türü**için , **Korelasyon**seçin.
1. **Yürüt**’ü seçin.
1. Rattle en fazla 40 değişken önerdiği konusunda sizi uyarır. Çizimi görüntülemek için **Evet'i** seçin.

Ortaya çıkan bazı ilginç korelasyonlar vardır: _teknoloji_ güçlü _HP_ ve _laboratuvarları_ile ilişkilidir, örneğin. Ayrıca _650_ ile de güçlü bir şekilde ilişkilidir çünkü veri seti bağışçılarının alan kodu 650'dir.

Sözcükler arasındaki bağıntılar için sayısal değerler **Keşfet** penceresinde kullanılabilir. Bu, örneğin, _bu teknoloji_ olumsuz _sizin_ ve _para_ile ilişkili olduğunu unutmayın ilginç.

Çıngırak, bazı yaygın sorunları işlemek için veri kümesini dönüştürebilir. Örneğin, özellikleri yeniden ölçeklendirebilir, eksik değerleri impute edebilir, aykırılıkları işleyebilir ve eksik veri içeren değişkenleri veya gözlemleri kaldırabilir. Çıngırak da gözlemler ve değişkenler arasındaki ilişki kurallarını belirleyebilirsiniz. Bu sekmeler bu giriş gözden geçirme de kapsama değildir.

Çıngırak da küme analizi çalıştırabilirsiniz. Çıktının okunmasını kolaylaştırmak için bazı özellikleri hariç tutalım. **Veri** sekmesinde, bu 10 öğe dışında değişkenlerin her birinin yanında **Yoksay'ı** seçin:

* word_freq_hp
* word_freq_technology
* word_freq_george
* word_freq_remove
* word_freq_your
* word_freq_dollar
* word_freq_money
* capital_run_length_longest
* word_freq_business
* spam

**Küme** sekmesine dön. **KMeans'i**seçin ve ardından **küme sayısını** **4**olarak ayarlayın. **Yürüt**’ü seçin. Sonuçlar çıktı penceresinde görüntülenir. Bir küme _george_ ve _hp_yüksek frekansa sahiptir , ve muhtemelen meşru bir iş e-posta.

Temel bir karar ağacı makine öğrenme modeli oluşturmak için:

1. **Model** sekmesini seçin,
1. **Türü** **için, Ağaç'ı**seçin.
1. Çıktı **Execute** penceresinde ağacı metin biçiminde görüntülemek için Yürüt'''ün'u seçin.
1. Grafik bir sürümü görüntülemek için **Çiz** düğmesini seçin. Karar ağacı daha önce rpart kullanarak elde edilen ağaca benzer.

Rattle yararlı bir özelliği çeşitli makine öğrenme yöntemleri çalıştırmak ve hızlı bir şekilde bunları değerlendirmek için yeteneğidir. İşte adımlar şunlardır:

1. **Türü**için , **Tümünü**seçin.
1. **Yürüt**’ü seçin.
1. Çıngırak çalışırken bittiğinde, **SVM**gibi herhangi bir **Tür** değerini seçebilir ve sonuçları görüntüleyebilirsiniz.
1. **Ayrıca, Değerlendirme** sekmesini kullanarak doğrulama kümesindeki modellerin performansını karşılaştırabilirsiniz. Örneğin, **Hata Matrisi** seçimi, doğrulama kümesindeki her model için karışıklık matrisini, genel hatayı ve ortalama sınıf hatasını gösterir. Ayrıca ROC eğrilerini çizebilir, duyarlılık çözümlemesi yapabilir ve diğer model değerlendirme türlerini yapabilirsiniz.

Modelleri oluşturmayı bitirdiğinizde, oturumunuz sırasında Rattle tarafından çalıştırılan R kodunu görüntülemek için **Günlük** sekmesini seçin. Kaydetmek için **Dışa** Aktar düğmesini seçebilirsiniz.

> [!NOTE]
> Rattle'ın geçerli sürümünde bir hata var. Komut dosyasını değiştirmek veya adımlarınızı daha sonra yinelemek **#** için kullanmak için, *bu günlük* önüne ... günlüğün metnine bir karakter eklemeniz gerekir.

### <a name="postgresql-and-squirrel-sql"></a>PostgreSQL ve SQuirreL SQL

DSVM, PostgreSQL yüklü olarak gelir. PostgreSQL karmaşık, açık kaynak ilişkisel bir veritabanıdır. Bu bölümde, spambase veri kümesini PostgreSQL'e nasıl yükleyip sorgulayabileceğiniz i

Verileri yükleyemeden önce, yerel ana bilgisayardan parola kimlik doğrulaması için izin vermelisiniz. Bir komut isteminde şunu çalıştırın:

```Bash
sudo gedit /var/lib/pgsql/data/pg_hba.conf
```

Config dosyasının alt kısmında izin verilen bağlantıları ayrıntılı birkaç satır vardır:

```
# "local" is only for Unix domain socket connections:
local   all             all                                     trust
# IPv4 local connections:
host    all             all             127.0.0.1/32            ident
# IPv6 local connections:
host    all             all             ::1/128                 ident
```

**IPv4 yerel bağlantı** hattını **ident**yerine **md5** kullanmak için değiştirin, böylece bir kullanıcı adı ve parola kullanarak oturum açabiliriz:

```
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
```

Ardından, PostgreSQL hizmetini yeniden başlatın:

```Bash
sudo systemctl restart postgresql
```

Yerleşik postgres kullanıcısı olarak *psql* (PostgreSQL için etkileşimli bir terminal) başlatmak için şu komutu çalıştırın:

```Bash
sudo -u postgres psql
```

Oturum açmak için kullandığınız Linux hesabının kullanıcı adını kullanarak yeni bir kullanıcı hesabı oluşturun. Parola oluşturma:

```Bash
CREATE USER <username> WITH CREATEDB;
CREATE DATABASE <username>;
ALTER USER <username> password '<password>';
\quit
```

psql'e giriş yapın:

```Bash
psql
```

Verileri yeni bir veritabanına aktarın:

```SQL
CREATE DATABASE spam;
\c spam
CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
\copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
\quit
```

Şimdi, verileri inceleyelim ve bir JDBC sürücüsü aracılığıyla veritabanlarıyla etkileşimde kalmak için kullanabileceğiniz bir grafik aracı olan SQuirreL SQL'i kullanarak bazı sorguları çalıştıralım.

Başlamak için **Uygulamalar** menüsünde SQuirreL SQL'i açın. Sürücüyü ayarlamak için:

1. **Windows** > **View Sürücüleri'ni**seçin.
1. **PostgreSQL'e** sağ tıklayın ve **Sürücüyü Değiştir'i**seçin.
1. **Ekstra Sınıf Yol** > **Ekle'yi**seçin.
1. **Dosya Adı**için , girin **/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar**.
1. **Aç**'ı seçin.
1. **Liste Sürücüleri'ni**seçin. **Sınıf Adı**için **org.postgresql.Driver'ı**seçin ve ardından **Tamam'ı**seçin.

Yerel sunucuya bağlantı kurmak için:

1. **Windows** > **Görünüm Takma Adları'nı seçin.**
1. Yeni **+** bir takma ad oluşturmak için düğmeyi seçin. Yeni takma ad için **Spam veritabanını**girin. 
1. **Sürücü**için **PostgreSQL'i**seçin.
1. URL'yi **jdbc:postgresql://localhost/spam**olarak ayarlayın.
1. Kullanıcı adınızı ve parolanızı girin.
1. **Tamam'ı**seçin.
1. **Bağlantı** penceresini açmak için **Spam veritabanı** takma adını çift tıklatın.
1. **Bağlan**’ı seçin.

Bazı sorguları çalıştırmak için:

1. **SQL** sekmesini seçin.
1. **SQL** sekmesinin üst kısmındaki sorgu kutusuna, '' `SELECT * from data;`gibi temel bir sorgu girin.
1. Sorguyu çalıştırmak için Ctrl+Enter tuşuna basın. Varsayılan olarak, SQuirreL SQL sorgunuzdaki ilk 100 satırı döndürür.

Bu verileri keşfetmek için çalıştırabileceğiniz daha birçok sorgu vardır. Örneğin, sözcüğün sıklığı spam *make* ve jambon arasında nasıl farklılık gösterir?

```SQL
SELECT avg(word_freq_make), spam from data group by spam;
```

Veya, sık sık *3d*içeren e-posta özellikleri nelerdir?

```SQL
SELECT * from data order by word_freq_3d desc;
```

*3d* yüksek bir olay var çoğu e-postalar görünüşte spam vardır. Bu bilgiler, e-postaları sınıflandırmak için tahmine dayalı bir model oluşturmak için yararlı olabilir.

PostgreSQL veritabanında depolanan verileri kullanarak makine öğrenimi yapmak istiyorsanız, [MADlib'i](https://madlib.incubator.apache.org/)kullanmayı düşünün.

### <a name="sql-data-warehouse"></a>SQL Veri Ambarı

Azure SQL Veri Ambarı, hem ilişkisel hem de ilişkisel olmayan büyük hacimli verileri işleyebilir bulut tabanlı, ölçeklendirilebilir bir veritabanıdır. Daha fazla bilgi için Azure [SQL Veri Ambarı nedir?](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

Veri ambarına bağlanmak ve tabloyu oluşturmak için aşağıdaki komutu komut isteminden çalıştırın:

```Bash
sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I
```

sqlcmd komut u komutunu çalıştırın:

```SQL
CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
GO
```

BCP kullanarak verileri kopyalayın:

```bash
bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"
```

> [!NOTE]
> İndirilen dosya, Windows stili satır sonları içerir. bcp aracı Unix tarzı çizgi uçları bekliyor. bcp'yi söylemek için -r bayrağını kullanın.

Sonra, sqlcmd kullanarak sorgulayın:

```sql
select top 10 spam, char_freq_dollar from spam;
GO
```

Ayrıca SQuirreL SQL kullanarak sorgulayabilirsiniz. SQL Server JDBC sürücüsünü kullanarak PostgreSQL'e benzer adımları izleyin. JDBC sürücüsü /usr/share/java/jdbcdrivers/sqljdbc42.jar klasöründedir.

## <a name="next-steps"></a>Sonraki adımlar

Azure'daki veri bilimi işlemini oluşturan görevlerde size yol gösteren makalelere genel bakış [için, Takım Veri Bilimi Süreci'ne](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview)bakın.

Belirli senaryolar için Ekip Veri Bilimi Süreci'ndeki adımları gösteren uçtan uca gözden geçirmelerin açıklaması [için, Bkz.](../team-data-science-process/walkthroughs.md) İzler ayrıca bulut ve şirket içi araçları ve hizmetleri akıllı bir uygulama oluşturmak için iş akışı veya ardışık yol üzerinde nasıl birleştirilen gösteriş.
