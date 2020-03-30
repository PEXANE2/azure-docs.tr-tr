---
title: 'Referans: Ubuntu Veri Bilimi Sanal Makine'
titleSuffix: Azure Data Science Virtual Machine
description: Ubuntu Data Science Virtual Machine'de yer alan araçlarla ilgili ayrıntılar
author: gvashishtha
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: gopalv
ms.date: 09/11/2019
ms.topic: reference
ms.openlocfilehash: 5c184e7f1dc828c3f9ff8d449d29ab3aaa4d1cf6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77525830"
---
# <a name="reference-ubuntu-linux-data-science-virtual-machine"></a>Referans: Ubuntu (Linux) Veri Bilimi Sanal Makine

Ubuntu Veri Bilimi Sanal Makinenizde mevcut araçların listesi için aşağıya bakın. 

## <a name="deep-learning-libraries"></a>Derin öğrenme kütüphaneleri

### <a name="cntk"></a>CNTK

Microsoft Cognitive Toolkit açık kaynak kodlu bir derin öğrenme araç setidir. Python bağlamaları kök ve py35 Conda ortamlarında mevcuttur. Ayrıca, zaten yolda olan bir komut satırı aracı (CNTK) vardır.

Örnek Python dizüstü bilgisayarlar JupyterHub mevcuttur. Komut satırında temel bir örneği çalıştırmak için kabuktaki aşağıdaki komutları çalıştırın:

```bash
cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
cntk configFile=lr_bs.cntk makeMode=false command=Train
```

Daha fazla bilgi için [GitHub'ın](https://github.com/Microsoft/CNTK) CNTK bölümüne ve [CNTK wiki'ye](https://github.com/Microsoft/CNTK/wiki)bakın.

### <a name="caffe"></a>Caffe

Caffe, Berkeley Vision and Learning Center'ın derin öğrenme çerçevesidir. /opt/caffe'de mevcuttur. /opt/caffe/örneklerde örnekler bulabilirsiniz.

### <a name="caffe2"></a>Caffe2

Caffe2, Facebook'tan Caffe üzerine kurulu derin bir öğrenme çerçevesidir. Conda kök ortamında Python 2.7'de mevcuttur. Etkinleştirmek için, kabuktan aşağıdaki komutu çalıştırın:

```bash
source /anaconda/bin/activate root
```

Bazı örnek dizüstü bilgisayarlar JupyterHub mevcuttur.

### <a name="h2o"></a>H2O

H2O hızlı, bellekte, dağıtılmış makine öğrenimi ve tahmine dayalı bir analiz platformudur. Hem kök hem de py35 Anaconda ortamlarında bir Python paketi yüklenir. R paketi de yüklenir. 

Komut satırından H2O'yu `java -jar /dsvm/tools/h2o/current/h2o.jar`açmak için çalıştırın. Yapılandırmak isteyebileceğin çeşitli [komut satırı seçenekleri](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/starting-h2o.html#from-the-command-line) vardır. Başlamak için göz atarak Flow web http://localhost:54321 Web Web UI'ye erişebilirsiniz. Örnek dizüstü bilgisayarlar JupyterHub'da da mevcuttur.

### <a name="keras"></a>Keras

Keras Python'da üst düzey bir sinir ağı API'si. TensorFlow, Microsoft Cognitive Toolkit veya Theano'nun üzerinde çalıştırılabilir. Kök ve py35 Python ortamlarında mevcuttur.

### <a name="mxnet"></a>MXNet

MXNet, hem verimlilik hem de esneklik için tasarlanmış derin bir öğrenme çerçevesidir. DSVM'de R ve Python bağlamaları vardır. Örnek not defterleri JupyterHub'a dahildir ve örnek kod /dsvm/samples/mxnet adresinde mevcuttur.

### <a name="nvidia-digits"></a>NVIDIA BASAMAKLARI

DIGITS olarak bilinen NVIDIA Derin Öğrenme GPU Eğitim Sistemi, ortak derin öğrenme görevlerini basitleştiren bir sistemdir. Bu görevler arasında gpu sistemlerinde veri yönetimi, sinir ağları tasarlama ve eğitim ve gelişmiş görselleştirme ile performansı gerçek zamanlı olarak izlemek yer almaktadır.

DIGITS, *basamak*adı verilen bir hizmet olarak kullanılabilir. Hizmeti başlatın ve http://localhost:5000 başlamak için göz atın.

DIGITS, Conda kök ortamında Python modülü olarak da yüklenir.

### <a name="tensorflow"></a>TensorFlow

TensorFlow, Google'ın derin öğrenme kütüphanesidir. Veri akışı grafiklerini kullanarak sayısal hesaplama için açık kaynak kodlu bir yazılım kitaplığıdır. TensorFlow py35 Python ortamında mevcuttur ve bazı örnek not defterleri JupyterHub'da bulunmaktadır.

### <a name="theano"></a>Theano

Theano verimli sayısal hesaplama için bir Python kitaplığıdır. Kök ve py35 Python ortamlarında mevcuttur. 

### <a name="torch"></a>Torch

Torch makine öğrenme algoritmaları için geniş destek ile bilimsel bir bilgisayar çerçevesidir. /dsvm/tools/torch adresinde mevcuttur ve **th** interaktif oturum ve LuaRocks paket yöneticisi komut satırında mevcuttur. Örnekler /dsvm/samples/torch adresinde mevcuttur.

PyTorch kök Anaconda ortamında da mevcuttur. Örnekler /dsvm/samples/pytorch'dadır.

## <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Sunucusu

R, veri analizi ve makine öğrenimi için en popüler dillerden biridir. Analitik için R'yi kullanmak istiyorsanız, VM'de Microsoft R Open ve Math Kernel Library'ye sahip Microsoft Machine Learning Server bulunur. Math Kernel Library analitik algoritmalarda yaygın olarak kullanılan matematik işlemlerini optimize eder. Microsoft R Open, CRAN R ile yüzde 100 uyumludur ve CRAN'da yayınlanan R kitaplıklarından herhangi biri Microsoft R Open'a yüklenebilir. 

Machine Learning Server, R modellerinin web hizmetlerine ölçekleme ve operasyonelhale getirilmesini sağlar. R programlarınızı RStudio, vi veya Emacs gibi varsayılan düzenleyicilerden birinde edinebilirsiniz. Emacs düzenleyicisini kullanmayı tercih ederseniz, önceden yüklenmiş. Emacs ESS (Emacs Speaks Statistics) paketi, Emacs düzenleyicisi içindeki R dosyalarıyla çalışmayı kolaylaştırır.

R konsolu açmak için kabukta **R** girersiniz. Bu komut sizi etkileşimli bir ortama götürür. R programınızı geliştirmek için genellikle Emacs veya vi gibi bir düzenleyici kullanırsınız ve ardından komut dosyalarını R içinde çalıştırırsınız. RStudio ile, R programınızı geliştirmek için tam bir grafik IDE'ye sahipsiniz.

İsterseniz [En İyi 20 R paketlerini](https://www.kdnuggets.com/2015/06/top-20-r-packages.html) yüklemeniz için bir R komut dosyası da vardır. Bu komut dosyasını R etkileşimli arabiriminde olduktan sonra çalıştırabilirsiniz. Daha önce de belirtildiği gibi, kabuk içinde **R** girerek bu arabirimi açabilirsiniz.  

## <a name="python"></a>Python

Anaconda Python Python 2.7 ve 3.5 ortamları ile yüklenir. 2.7 _ortama kök,_ 3.5 ortamına _ise py35_denir. Bu dağılım, en popüler matematik, mühendislik ve veri analizi paketlerinden yaklaşık 300'ünün yanı sıra temel Python'u da içerir.

Py35 ortamı varsayılandır. Kök (2.7) ortamını etkinleştirmek için şu komutu kullanın:

```bash
source activate root
```

Py35 ortamını yeniden etkinleştirmek için şu komutu kullanın:

```bash
source activate py35
```

Python etkileşimli oturum çağırmak için kabukta **python** girin. 

Conda veya pip kullanarak ek Python kitaplıkları yükleyin. Pip için, varsayılanı istemiyorsanız önce doğru ortamı etkinleştirin:

```bash
source activate root
pip install <package>
```

Veya pip için tam yol belirtin:

```bash
/anaconda/bin/pip install <package>
```

Conda için her zaman çevre adını (py35 veya root) belirtmelisiniz:

```bash
conda install <package> -n py35
```

Grafik bir arabirimdeyseniz veya X11 yönlendirmeniz ayarlanmışsa, PyCharm Python IDE'yi açmak için **pycharm'a** girebilirsiniz. Varsayılan metin düzenleyicilerini kullanabilirsiniz. Buna ek olarak, Anaconda Python dağıtımları ile birlikte bir Python IDE Spyder kullanabilirsiniz. Spyder bir grafik masaüstü veya X11 yönlendirme gerekir. Grafik masaüstü Spyder için bir kısayol vardır.

## <a name="jupyter-notebook"></a>Jupyter dizüstü bilgisayar

Anaconda dağıtım da bir Jupyter dizüstü bilgisayar, kod ve analiz paylaşmak için bir ortam ile birlikte gelir. Jupyter dizüstü bilgisayara JupyterHub üzerinden erişilir. Yerel Linux kullanıcı adınızı ve şifrenizi kullanarak oturum açın.

Jupyter dizüstü bilgisayar sunucusu Python 2, Python 3 ve R çekirdekleri ile önceden yapılandırılmıştır. Tarayıcıyı açmak ve dizüstü bilgisayar sunucusuna erişmek için **Jupyter Notebook** masaüstü simgesini kullanın. SSH veya X2Go istemcisi üzerinden VM'deyseniz, [https://localhost:8000/](https://localhost:8000/)Jupyter dizüstü bilgisayar sunucusuna da erişebilirsiniz.

> [!NOTE]
> Sertifika uyarıları alırsanız devam edin.

Jupyter dizüstü bilgisayar sunucusuna herhangi bir ana bilgisayardan erişebilirsiniz. **Https://\<VM DNS adı\>veya IP adresi girin :8000/**.

> [!NOTE]
> VM sağlandığında bağlantı noktası 8000 varsayılan olarak güvenlik duvarında açılır. 

Biri Python'da, diğeri R'de olmak üzere örnek defterleri paketledik. Yerel Linux kullanıcı adınızı ve parolanızı kullanarak Jupyter not defterine kimlik doğrulaması yaptıktan sonra not defteri ana sayfasında örneklerin bağlantısını görebilirsiniz. **Yeni**'yi seçip uygun dil çekirdeğini seçerek yeni bir not defteri oluşturabilirsiniz. **Yeni** düğmesini görmüyorsanız, not defteri sunucusunun ana sayfasına gitmek için sol üstteki **Jupyter** simgesini seçin.

## <a name="apache-spark-standalone"></a>Apache Kıvılcım tek başına

Apache Spark'ın tek başına bir örneği, spark uygulamalarını büyük kümeler üzerinde test etmeden ve dağıtmadan önce yerel olarak geliştirmenize yardımcı olmak için Linux DSVM'de önceden yüklenir. 

PySpark programlarını Jupyter çekirdeğinden çalıştırabilirsiniz. Jupyter'ı açtığınızda, **Yeni** düğmesini seçin ve kullanılabilir çekirdeklerin listesini görmeniz gerekir. **Spark - Python,** Python dilini kullanarak Spark uygulamalarını oluşturmanıza olanak tanıyan PySpark çekirdeğidir. Ayrıca Spark programı oluşturmak için PyCharm veya Spyder gibi bir Python IDE kullanabilirsiniz. 

Bu bağımsız durumda, Spark yığını çağıran istemci programı içinde çalışır. Bu özellik, bir Kıvılcım kümesinde geliştirmeye kıyasla sorunları daha hızlı ve daha kolay gidermeyi sağlar.

Jupyter örnek bir PySpark dizüstü bilgisayar sağlar. Jupyter ev dizini altında SparkML dizini bulabilirsiniz ($HOME / dizüstü / SparkML / pySpark). 

R for Spark'ta programlıyorsanız, Microsoft Machine Learning Server, SparkR veya sparklyr'ı kullanabilirsiniz. 

Microsoft Machine Learning Server'da Kıvılcım bağlamında çalıştırmadan önce, yerel tek düğümlü Hadoop HDFS ve İplik örneğini etkinleştirmek için tek seferlik bir kurulum adımı yapmanız gerekir. Varsayılan olarak, Hadoop hizmetleri yüklenir, ancak DSVM'de devre dışı bırakılır. Etkinleştirmek için, aşağıdaki komutları ilk kez kök olarak çalıştırmanız gerekir:

```bash
echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
chmod 0600 ~hadoop/.ssh/authorized_keys
chown hadoop:hadoop ~hadoop/.ssh/id_rsa
chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
systemctl start hadoop-namenode hadoop-datanode hadoop-yarn
```

Hadoop ile ilgili hizmetleri çalıştırarak ```systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```bunlara ihtiyacınız olmadığında durdurabilirsiniz.

/dsvm/samples/MRS dizini, Microsoft Machine Learning Server'ın uzak bir Kıvılcım bağlamında nasıl geliştirileceği ve test edilebildiğini gösteren bir örnek sağlar (DSVM'deki bağımsız Kıvılcım örneği).

## <a name="ides-and-editors"></a>IID'ler ve editörler

Vi/Vim, Emacs, PyCharm, RStudio ve IntelliJ dahil olmak üzere çeşitli kod editörleri seçeneğiniz vardır. 

PyCharm, RStudio ve IntelliJ grafik editörleridir. Bunları kullanmak için grafik bir masaüstünde oturum açmış olmanız gerekir. Masaüstü ve uygulama menüsü kısayollarını kullanarak açarsınız.

Vim ve Emacs metin tabanlı editörlerdir. Emacs'ta ESS eklenti paketi, Emacs düzenleyicisi içinde R ile çalışmayı kolaylaştırır. Ess [web sitesinde](https://ess.r-project.org/)daha fazla bilgi bulabilirsiniz.

LaTex texlive paketi üzerinden, [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html)denilen bir Emacs eklenti paketi ile birlikte yüklenir. Bu paket, LaTex belgelerinizi Emacs içinde yazmanızı kolaylaştırır.  

## <a name="databases"></a>Veritabanları

### <a name="graphical-sql-client"></a>Grafiksel SQL istemcisi

Bir grafik SQL istemcisi olan SQuirrel SQL, çeşitli veritabanlarına (Microsoft SQL Server ve MySQL gibi) bağlanabilir ve SQL sorguları çalıştırabilir. SQuirrel SQL'i grafik sel masaüstü oturumundan (örneğin X2Go istemcisi aracılığıyla) bir masaüstü simgesi kullanarak çalıştırabilirsiniz. Veya kabuktaki aşağıdaki komutu kullanarak istemciyi çalıştırabilirsiniz:

```bash
/usr/local/squirrel-sql-3.7/squirrel-sql.sh
```

İlk kullanımdan önce, sürücülerinizi ve veritabanı takma adlarınızı ayarlayın. JDBC sürücüleri /usr/share/java/jdbcdrivers adresinde bulunmaktadır.

Daha fazla bilgi için [Bkz. SQuirrel SQL.](http://squirrel-sql.sourceforge.net/index.php?page=screenshots)

### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Microsoft SQL Server'a erişmek için komut satırı araçları

SQL Server için ODBC sürücü paketi de iki komut satırı araçları ile birlikte gelir:

- **bcp**: BCP aracı, Microsoft SQL Server örneği ile kullanıcı tarafından belirtilen biçimde bir veri dosyası arasındaki verileri toplu kopyalar. BCP aracını, çok sayıda yeni satırı SQL Server tablolarına aktarmak veya tablolardaki verileri veri dosyalarına aktarmak için kullanabilirsiniz. Verileri tabloya almak için, bu tablo için oluşturulan biçim dosyası kullanmanız gerekir. Veya, tablonun yapısını ve sütunları için geçerli olan veri türlerini anlamanız gerekir.

  Daha fazla bilgi için [bcp ile bağlanma'ya](https://msdn.microsoft.com/library/hh568446.aspx)bakın.

- **sqlcmd**: Sqlcmd aracını kullanarak Transact-SQL deyimlerini girebilirsiniz. Ayrıca komut istemi sistem yordamları ve komut dosyası dosyaları girebilirsiniz. Bu araç, Transact-SQL toplu işlerini çalıştırmak için ODBC kullanır.

  Daha fazla bilgi için [sqlcmd ile Bağlanma'ya](https://msdn.microsoft.com/library/hh568447.aspx)bakın.

  > [!NOTE]
  > Linux ve Windows platformları arasında bu araçta bazı farklılıklar vardır. Ayrıntılar için belgelere bakın.

### <a name="database-access-libraries"></a>Veritabanı erişim kitaplıkları

Kitaplıklar veritabanı erişimi için R ve Python'da kullanılabilir:

* R'de, veritabanı sunucusunda SQL deyimlerini sorgulamak veya çalıştırmak için RODBC paketini veya dplyr paketini kullanabilirsiniz.
* Python'da pyodbc kitaplığı, altta yatan katman olarak ODBC ile veritabanı erişimi sağlar.  

## <a name="azure-tools"></a>Azure araçları

VM'de aşağıdaki Azure araçları yüklenir:

* **Azure CLI**: Azure'daki komut satırı arabirimini kullanarak Azure kaynaklarını kabuk komutları aracılığıyla oluşturabilir ve yönetebilirsiniz. Azure araçlarını açmak için **azure yardımı**girin. Daha fazla bilgi için [Azure CLI dokümantasyon sayfasına](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)bakın.
* **Azure Depolama Gezgini**: Azure Depolama Gezgini, Azure depolama hesabınızda depoladığınız nesnelere göz atmak ve Azure blob'larına veri yüklemek ve indirmek için kullanabileceğiniz bir grafik aracıdır. Depolama Gezgini'ne masaüstü kısayol simgesinden erişebilirsiniz. Ayrıca **StorageExplorer**girerek bir kabuk istemi açabilirsiniz. Bir X2Go istemcisinden oturum açmış olmalısınız veya X11 yönlendirmesi ayarlanmalıdır.
* **Azure kitaplıkları**: Aşağıda önceden yüklenmiş kitaplıklardan bazıları veremilmiştir.
  
  * **Python**: Python'daki Azure ile ilgili kitaplıklar *azure*, *azureml*, *pydocumentdb*ve *pyodbc'dir.* İlk üç kitaplıkla Azure depolama hizmetlerine, Azure Machine Learning'e ve Azure Cosmos DB'ye (Azure'da NoSQL veritabanı) erişebilirsiniz. Dördüncü kitaplık pyodbc (SQL Server için Microsoft ODBC sürücüsü ile birlikte), ODBC arabirimi kullanarak Python'dan SQL Server, Azure SQL Veritabanı ve Azure SQL Veri Ambarı'na erişim sağlar. Listelenen tüm kitaplıkları görmek için **pip listesini** girin. Bu komutu hem Python 2.7 hem de 3.5 ortamlarında çalıştırdığından emin olun.
  * **R**: R'deki Azure ile ilgili kitaplıklar AzureML ve RODBC'dir.
  * **Java**: Azure Java kitaplıklarının listesi VM'deki /dsvm/sdk/AzureSDKJava dizininde bulunabilir. Önemli kitaplıklar, SQL Server için Azure depolama ve yönetim API'leri, Azure Cosmos DB ve JDBC sürücüleridir.  

[Azure portalına](https://portal.azure.com) önceden yüklenmiş Firefox tarayıcısından erişebilirsiniz. Azure portalında Azure kaynaklarını oluşturabilir, yönetebilir ve izleyebilirsiniz.

## <a name="azure-machine-learning"></a>Azure Machine Learning

Azure Machine Learning, tahmine dayalı analitik çözümleri oluşturmanıza, dağıtmanıza ve paylaşmanıza olanak tanıyan tam olarak yönetilen bir bulut hizmetidir. Denemelerinizi ve modellerinizi Azure Machine Learning Studio'dan (klasik) oluşturabilirsiniz. [Microsoft Azure Machine Learning'i](https://studio.azureml.net)ziyaret ederek Veri Bilimi Sanal Makine'deki bir web tarayıcısından erişebilirsiniz.

Azure Machine Learning Studio'da (klasik) oturum imzaladıktan sonra, makine öğrenimi algoritmaları için mantıksal bir akış oluşturmak için bir deneme tuvali kullanabilirsiniz. Ayrıca Azure Machine Learning'de barındırılan ve Azure Machine Learning Studio'daki (klasik) deneylerle sorunsuz bir şekilde çalışabilen bir Jupyter dizüstü bilgisayara da erişebilirsiniz. 

Bir web hizmeti arabirimine sararak oluşturduğunuz makine öğrenimi modellerini operasyonel hale getirin. Makine öğrenimi modellerinin operasyonelleştirilmesi, herhangi bir dilde yazılmış istemcilerin bu modellerden öngörüler çağırmasına olanak tanır. Daha fazla bilgi için [Machine Learning belgelerine](https://azure.microsoft.com/documentation/services/machine-learning/)bakın.

Ayrıca modellerinizi VM'de R veya Python'da oluşturabilir ve bunları Azure Machine Learning'de üretime dağıtabilirsiniz. Bu işlevselliği etkinleştirmek için R (**AzureML**) ve Python **(azureml)** kitaplıklarını yükledik.

R ve Python'daki modelleri Azure Machine Learning'e nasıl dağıtabileceğiniz hakkında bilgi için [Data Science Virtual Machine'de yapabileceğiniz on şeye](vm-do-ten-things.md)bakın.

> [!NOTE]
> Bu talimatlar Veri Bilimi Sanal Makine Windows sürümü için yazılmıştır. Ancak, azure machine learning'e modelleri dağıtma konusunda orada sağlanan bilgiler Linux VM için geçerlidir.

## <a name="machine-learning-tools"></a>Makine öğrenimi araçları

VM, önceden derlenmiş ve yerel olarak önceden yüklenmiş makine öğrenimi araçları ve algoritmalarıyla birlikte gelir. Bunlar:

* **Vowpal Wabbit**: Hızlı bir online öğrenme algoritması.
* **xgboost**: Optimize edilmiş, artırılmış ağaç algoritmaları sağlayan bir araçtır.
* **Çıngırak**: Kolay veri arama ve modelleme için R tabanlı bir grafik aracı.
* **Python**: Anaconda Python, Scikit-learn gibi kütüphanelerle birlikte makine öğrenimi algoritmalarıyla birlikte gelir. Komutu kullanarak diğer kitaplıkları `pip install` yükleyebilirsiniz.
* **LightGBM**: Karar ağacı algoritmalarına dayalı hızlı, dağıtılmış, yüksek performanslı degrade artırıcı çerçeve.
* **R**: R. Önceden yüklenmiş kütüphaneler için lm, glm, randomForest ve rpart gibi zengin bir makine öğrenimi işlevleri kütüphanesi mevcuttur. Bu komutu çalıştırarak diğer kitaplıkları yükleyebilirsiniz:
  
        install.packages(<lib name>)

Burada listedeki ilk üç makine öğrenme araçları hakkında bazı ek bilgiler.

### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit online, karma, allreduce, azaltma, learning2search, aktif ve interaktif öğrenme gibi teknikleri kullanan bir makine öğrenme sistemidir.

Aracı temel bir örnekte çalıştırmak için aşağıdaki komutları kullanın:

```bash
cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
cd vwdemo
vw house_dataset
```

Bu dizinde başka büyük demolar da var. Vowpal Wabbit hakkında daha fazla bilgi için GitHub ve [Vowpal Wabbit wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki) [bu bölüme](https://github.com/JohnLangford/vowpal_wabbit) bakın.

### <a name="xgboost"></a>xgboost

Xgboost kitaplığı artırılmış (ağaç) algoritmaları için tasarlanmış ve optimize edilmiştir. Bu kitaplığın amacı, ölçeklenebilir, taşınabilir ve doğru büyük ölçekli ağaç artırma sağlamak için gerekli uç noktalara makinelerin hesaplama sınırlarını itmektir.

Komut satırı ve R kitaplığı olarak sağlanır. Bu kitaplığı R'de kullanmak için etkileşimli bir R oturumu başlatabilir (kabukta **R** girerek) ve kitaplığı yükleyebilirsiniz.

Aşağıda, R isteminde çalıştırabileceğiniz basit bir örnek verilmiştir:

```R
library(xgboost)

data(agaricus.train, package='xgboost')
data(agaricus.test, package='xgboost')
train <- agaricus.train
test <- agaricus.test
bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
pred <- predict(bst, test$data)
```

xgboost komut satırını çalıştırmak için, kabukta çalıştırılabilmek için komutlar şunlardır:

```bash
cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
cd xgboostdemo
xgboost mushroom.conf
```

Bir .model dosyası belirtilen dizine yazılır. Bu demo örneği hakkında bilgileri [GitHub'da](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification)bulabilirsiniz.

xgboost hakkında daha fazla bilgi [için, xgboost dokümantasyon sayfasına](https://xgboost.readthedocs.org/en/latest/) ve [GitHub deposuna](https://github.com/dmlc/xgboost)bakın.

### <a name="rattle"></a>Rattle

Çıngırak **(R** **A**nalytical **T**ool **T**o **L**asily **E**kazanmak) GUI tabanlı veri arama ve modelleme kullanır. Verilerin istatistiksel ve görsel özetlerini sunar, kolayca modellenebilen verileri dönüştürür, verilerden hem denetimsiz hem de denetlenen modeller oluşturur, modellerin performansını grafikolarak sunar ve yeni veri kümeleri puanlar. Ayrıca, doğrudan R çalıştırılabilir veya daha fazla analiz için bir başlangıç noktası olarak kullanılan UI işlemleri çoğaltarak, R kodu oluşturur.

Rattle'ı çalıştırmak için grafiksel masaüstü oturumda olmanız gerekir. Terminalde R ortamını açmak için **R** girin. R isteminde aşağıdaki komutları girin:

```R
library(rattle)
rattle()
```

Şimdi bir grafik arabirimi sekmeler kümesi ile açılır. Örnek bir hava durumu veri kümesini kullanmak ve bir model oluşturmak için Rattle'da aşağıdaki hızlı başlatma adımlarını kullanın. Bazı adımlarda, sistemde zaten olmayan bazı gerekli R paketlerini otomatik olarak yüklemeniz ve yüklemeniz istenir.

> [!NOTE]
> Paketi sistem dizinine (varsayılan) yükleme erişiminiz yoksa, r konsol pencerenizde paketleri kişisel kitaplığınıza yüklemek için bir istem görebilirsiniz. Bu istemleri görürseniz **y** yanıtını verin.

1. **Yürüt**’ü seçin.
1. Örnek hava durumu veri kümesini kullanmak isteyip istemediğinizi soran bir iletişim kutusu görüntülenir. Örneği yüklemek için **Evet'i** seçin.
1. **Model** sekmesini seçin.
1. Karar **Execute** ağacı oluşturmak için Yürüt'''ün'u seçin.
1. Karar ağacını görüntülemek için **Beraberlik'i** seçin.
1. **Orman** seçeneğini seçin ve rasgele bir orman oluşturmak için Yürüt'''ün'u seçin. **Execute**
1. **Değerlendir** sekmesini seçin.
1. **Risk** seçeneğini seçin ve iki **Risk (Kümülatif)** performans çizimi görüntülemek için **Yürüt'ü** seçin.
1. Önceki işlemler için oluşturulan R kodunu göstermek için **Günlük** sekmesini seçin.
   (Rattle'ın geçerli sürümündeki bir hata nedeniyle, bu **#** günlüğü günlüğün metnine **dışa** aktar'ın önüne bir karakter eklemeniz gerekir.)
1. weather_script adlı R komut dosyası dosyasını kaydetmek için **Dışa** Aktarma düğmesini *seçin. R* ev klasörüne.

Rattle ve R'den çıkabilirsin. Artık oluşturulan R komut dosyasını değiştirebilirsiniz. Veya, komut dosyasını olduğu gibi kullanın ve Rattle UI içinde yapılan her şeyi tekrarlamak için her zaman çalıştırın. Özellikle R yeni başlayanlar için, bu hızlı bir şekilde basit bir grafik arayüzünde analiz ve makine öğrenme yapmak için bir yoldur, otomatik olarak r kod oluştururken değiştirmek veya öğrenmek.

## <a name="next-steps"></a>Sonraki adımlar

Başka sorularınız mı var? Destek [bileti](https://azure.microsoft.com/support/create-ticket/)oluşturmayı düşünün.