---
title: 'Referans: Ubuntu DSVM'
description: Ubuntu Veri Bilimi Sanal Makinesi bulunan araçlarla ilgili ayrıntılar
author: gvashishtha
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: gopalv
ms.date: 09/11/2019
ms.topic: reference
ms.openlocfilehash: 0c48b8ba28f67ac7f38d6f636728abe7c5a6bd4e
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71204026"
---
# <a name="reference-ubuntu-linux-data-science-virtual-machine"></a>Referans: Ubuntu (Linux) Veri Bilimi Sanal Makinesi

Ubuntu Veri Bilimi Sanal Makinesi kullanılabilen araçların listesi için aşağıya bakın. 

## <a name="deep-learning-libraries"></a>Derin öğrenme kitaplıkları

### <a name="cntk"></a>CNTK

Microsoft Cognitive Toolkit, açık kaynaklı bir ayrıntılı öğrenme araç setidir. Python bağlamaları, kök ve py35 Conda ortamları kullanılabilir. Ayrıca, yolunda zaten olan bir komut satırı aracı (CNTK) vardır.

Örnek Python not defterleri JupyterHub içinde kullanılabilir. Komut satırında temel bir örnek çalıştırmak için, kabukta aşağıdaki komutları çalıştırın:

```bash
cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
cntk configFile=lr_bs.cntk makeMode=false command=Train
```

Daha fazla bilgi için [GitHub](https://github.com/Microsoft/CNTK) ve [cntk wiki](https://github.com/Microsoft/CNTK/wiki)'nin cntk bölümüne bakın.

### <a name="caffe"></a>Caffe

Caffe eğitim merkezi Berkeley görme ve derin öğrenme çerçevedir. /Seçenek/Caffe. içinde kullanılabilir. Örnekleri/opt/Caffe/Examples. içinde bulabilirsiniz

### <a name="caffe2"></a>Caffe2

Caffe2 Caffe üzerinde oluşturulmuş facebook'taki derin öğrenme çerçevedir. Conda root ortamında Python 2,7 ' de mevcuttur. Etkinleştirmek için, kabuktan aşağıdaki komutu çalıştırın:

```bash
source /anaconda/bin/activate root
```

Bazı örnek not defterleri JupyterHub içinde kullanılabilir.

### <a name="h2o"></a>H2O

H2O bir hızlı, bellek içi, dağıtılmış bir makine öğrenimi ve Tahmine dayalı analiz platformudur. Bir Python paketi kök ve py35 Anaconda ortamlarda yüklenir. Bir R paketi de yüklenir. 

Komut satırından H2O 'yi açmak için komutunu çalıştırın `java -jar /dsvm/tools/h2o/current/h2o.jar`. Yapılandırmak isteyebileceğiniz çeşitli [komut satırı seçenekleri](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/starting-h2o.html#from-the-command-line) vardır. Başlamak için ' e http://localhost:54321 giderek Flow Web Kullanıcı arabirimine erişebilirsiniz. Örnek Not defterleri de JupyterHub de mevcuttur.

### <a name="keras"></a>Keras

Keras, Python 'da üst düzey bir sinir ağ API 'sidir. Bu, TensorFlow, Microsoft Cognitive Toolkit veya teano üzerinde çalışabilir. Kök ve py35 Python ortamlarında kullanılabilir.

### <a name="mxnet"></a>MXNet

MXNet hem verimlilik hem de esneklik için tasarlanmış bir derin öğrenme çerçevedir. DSVM'nin dahil, R ve Python bağlamaları var. Örnek Not Defterleri JupyterHub dahildir ve örnek kod, /dsvm/samples/mxnet içinde kullanılabilir.

### <a name="nvidia-digits"></a>NVIDIA BASAMAK

RAKAM olarak bilinen NVıDıA derin öğrenme GPU eğitim sistemi, yaygın derin öğrenme görevlerini basitleştirecek bir sistemdir. Bu görevler, verileri yönetmeyi, GPU sistemlerinde sinir ağlarını tasarlamayı ve eğitimini ve gelişmiş görselleştirmede performansı gerçek zamanlı olarak izlemeyi içerir.

Basamaklar, *basamaklar*adlı bir hizmet olarak kullanılabilir. Hizmeti başlatıp göz atın http://localhost:5000 kullanmaya başlamak için.

BASAMAK de yüklüdür Conda kök ortamında bir Python modülü olarak.

### <a name="tensorflow"></a>TensorFlow

TensorFlow, Google'nın derin öğrenme kitaplığıdır. Veri akışı grafiklerini kullanan sayısal hesaplama için açık kaynaklı bir yazılım kitaplığıdır. TensorFlow py35 Python ortamında kullanılabilir ve bazı örnek not defterleri JupyterHub dahildir.

### <a name="theano"></a>Theano

Theano verimli sayısal hesaplama için bir Python kitaplıktır. Kök ve py35 Python ortamlarında kullanılabilir. 

### <a name="torch"></a>Torch

Torch bir makine öğrenimi algoritması için geniş destek ile bilimsel hesaplama çerçevedir. Bu,/dsvm/Tools/Torch sürümünde bulunur **ve etkileşimli oturum** ve LuaRocks Paket Yöneticisi komut satırından kullanılabilir. Örnekler /dsvm/samples/torch içinde kullanılabilir.

PyTorch ayrıca kök Anaconda ortamında kullanılabilir. İçinde /dsvm/samples/pytorch verilebilir.

## <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Sunucusu

R veri analizi ve makine öğrenimi için en popüler diller biridir. Analiz etmeniz için R 'yi kullanmak istiyorsanız, VM 'nin Microsoft R Open ve Math Kernel kitaplığı ile Microsoft Machine Learning Server vardır. Matematik Çekirdek Kitaplığı, analitik algoritmalarda ortak olan matematik işlemlerini iyileştirir. Microsoft R Open, CRAN R ile yüzde 100 uyumludur ve CRAN 'de yayınlanan R kitaplıklarının herhangi biri Microsoft R Open 'a yüklenebilir. 

Machine Learning Server, R modellerinin Web Hizmetleri ile ölçeklendirilmesi ve kullanımını sağlar. RStudio, olduğu gibi vi veya Emacs gibi varsayılan düzenleyicilerden biriyle R programlarınızın düzenleyebilirsiniz. Emacs düzenleyiciyi kullanmak isterseniz, önceden yüklenmiş olmuştur. Emacs ESS (Emacs Hoparlörks Istatistikleri) paketi, Emacs Düzenleyicisi içindeki R dosyalarıyla çalışmayı basitleştirir.

R konsolunu açmak için, kabuğa **r** yazın. Bu komut sizi etkileşimli bir ortama götürür. R programınızı geliştirmek için, genellikle Emacs veya vi gibi bir düzenleyici kullanın ve ardından betikleri R içinde çalıştırın. RStudio ile R programınızı geliştirmek için tam bir grafik IDE 'niz vardır.

Ayrıca, isterseniz [en iyi 20 r paketlerini](https://www.kdnuggets.com/2015/06/top-20-r-packages.html) yükleyebileceğiniz bir R betiği de vardır. Bu betiği, R etkileşimli arabiriminizdeki olduktan sonra çalıştırabilirsiniz. Daha önce belirtildiği gibi, bu arabirimi kabuğa **R** girerek açabilirsiniz.  

## <a name="python"></a>Python

Anaconda Python 3.5 ortamları ve Python 2.7 ile yüklenir. 2\.7 ortam adında _kök_, ve 3.5 ortam adında _py35_. Bu dağıtım, temel bir Python yaklaşık 300 en popüler matematik, mühendislik ve veri analizi paketlerinin yanı sıra içerir.

Varsayılan py35 ortamıdır. Kök (2,7) ortamını etkinleştirmek için şu komutu kullanın:

```bash
source activate root
```

Py35 ortamını yeniden etkinleştirmek için şu komutu kullanın:

```bash
source activate py35
```

Python etkileşimli oturumunu çağırmak için, kabuğa **Python** girin. 

Conda veya PIP kullanarak ek Python kitaplıkları kurun. PIP için, varsayılan değer istemiyorsanız, önce doğru ortamı etkinleştirin:

```bash
source activate root
pip install <package>
```

Veya pip için tam yolu belirtin:

```bash
/anaconda/bin/pip install <package>
```

Conda 'nın her zaman ortam adını (py35 veya root) belirtmeniz gerekir:

```bash
conda install <package> -n py35
```

Grafik arabiriminiz varsa veya X11 iletmeyi ayarladıysanız, Pydüğme Python IDE 'yi açmak için **pydüğme** girebilirsiniz. Varsayılan metin düzenleyicisi kullanabilirsiniz. Ayrıca, Anaconda Python dağıtımları ile paketlenmiş bir Python IDE olan Spyder kullanabilirsiniz. Grafik bir masaüstü veya X11 Spyder gereken iletme. Grafik masaüstünde Spyder kısayolu bulunur.

## <a name="jupyter-notebook"></a>Jupyter notebook

Anaconda dağıtım bir Jupyter not defteri ile kod ve analiz paylaşmak için bir ortam da gelir. Jupyter not defteri JupyterHub erişilir. Yerel Linux Kullanıcı adınızı ve parolanızı kullanarak oturum açın.

Jupyter notebook sunucusu Python 2, Python 3 ve R çekirdekler ile önceden yapılandırıldı. Tarayıcıyı açmak ve Not defteri sunucusuna erişmek için **Jupyter Notebook** masaüstü simgesini kullanın. Sanal makine ile SSH veya X2Go istemcisi aracılığıyla çalışıyorsanız, Jupyter Not defteri sunucusuna [https://localhost:8000/](https://localhost:8000/)da erişebilirsiniz.

> [!NOTE]
> Hiçbir sertifika uyarısı alırsanız devam edin.

Jupyter notebook sunucusu herhangi bir ana bilgisayardan erişebilirsiniz. **\>Https://\<VM DNS adını veya IP adresini girin: 8000/** .

> [!NOTE]
> VM hazırlandığında 8000 numaralı bağlantı noktasını Güvenlik Duvarı'nda varsayılan olarak açılır. 

Örnek Not defterlerimizi (Python 'da, diğeri R 'de) paketliyoruz. Yerel Linux Kullanıcı adınızı ve parolanızı kullanarak Jupyter not defterine kimlik doğrulaması yaptıktan sonra Not defteri giriş sayfasındaki örneklerin bağlantısını görebilirsiniz. **Yeni ' yi**seçerek ve ardından uygun dil çekirdeğini seçerek yeni bir not defteri oluşturabilirsiniz. **Yeni** düğmesini görmüyorsanız, sol üstteki **Jupyter** simgesini seçerek not defteri sunucusunun ana sayfasına gidin.

## <a name="apache-spark-standalone"></a>Tek başına Apache Spark

Tek başına bir Apache Spark örneği,, test etmeden ve bunları büyük kümelerdeki dağıtmadan önce yerel olarak Spark uygulamaları geliştirmenize yardımcı olmak için Linux DSVM 'ye önceden yüklenir. 

PySpark programlar Jupyter çekirdek çalıştırabilirsiniz. Jupyter açtığınızda, **Yeni** düğmesini seçin ve kullanılabilir çekirdekler listesini görmeniz gerekir. **Spark-Python** , Python dilini kullanarak Spark uygulamaları oluşturmanıza olanak sağlayan pyspark çekirdeğidir. Spark programınızı oluşturmak için Pydüğme veya Spyder gibi bir Python IDE de kullanabilirsiniz. 

Bu tek başına örneğinde, Spark Stack çağıran istemci programı içinde çalışır. Bu özellik, Spark kümesinde geliştirmeye kıyasla sorunları gidermeye daha hızlı ve kolay bir şekilde olanak sağlar.

Jupyter bir örnek PySpark Not defteri sağlar. Bu dosyayı jupi 'nın giriş dizini ($HOME/Notebooks/mini can ml/pyspark) altında can ml dizininde bulabilirsiniz. 

Spark için R 'de Programlama yapıyorsanız Microsoft Machine Learning Server, parlak r veya parlak LYR ' yi kullanabilirsiniz. 

Microsoft Machine Learning Server bir Spark bağlamında çalıştırmadan önce, yerel bir tek düğümlü Hadoop ve Yarn örneğini etkinleştirmek için bir kerelik Kurulum adımı yapmanız gerekir. Varsayılan olarak, Hadoop Hizmetleri yüklendi ancak DSVM'nin devre dışı. Bunu etkinleştirmek için, ilk kez kök olarak aşağıdaki komutları çalıştırmanız gerekir:

```bash
echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
chmod 0600 ~hadoop/.ssh/authorized_keys
chown hadoop:hadoop ~hadoop/.ssh/id_rsa
chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
systemctl start hadoop-namenode hadoop-datanode hadoop-yarn
```

' İ çalıştırarak ```systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```Hadoop ile ilgili hizmetleri durdurabilirsiniz.

/Dsvm/samples/MRS dizini, uzak Spark bağlamında Microsoft Machine Learning Server geliştirme ve test etme (DSVM 'deki tek başına Spark örneği) hakkında bir örnek sağlar.

## <a name="ides-and-editors"></a>IDE'ler ve düzenleyicilerden

VI/VIM, Emacs, Pylt, RStudio ve IntelliJ dahil olmak üzere çeşitli kod düzenleyicilerinden birini tercih edersiniz. 

Pydüğme, RStudio ve IntelliJ grafik düzenleyicilerdir. Bunları kullanmak için bir grafik masaüstünde oturum açmanız gerekir. Bunları masaüstü ve uygulama menüsü kısayollarını kullanarak açarsınız.

VIM ve Emacs metin tabanlı düzenleyicilerlerdir. Emacs 'de, ESS eklenti paketi Emacs Düzenleyicisi içinde R ile çalışmayı kolaylaştırır. [ESS Web sitesi](https://ess.r-project.org/)hakkında daha fazla bilgi edinebilirsiniz.

LaTex, TeXLive paketiyle birlikte [Auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html)adlı bir Emacs eklenti paketi ile yüklenir. Bu paket, Emacs içindeki LaTex belgelerinizi yazmayı basitleştirir.  

## <a name="databases"></a>Veritabanları

### <a name="graphical-sql-client"></a>Grafik SQL istemcisi

Grafik SQL istemcisi olan SQuirrel SQL, çeşitli veritabanlarına (Microsoft SQL Server ve MySQL gibi) bağlanabilir ve SQL sorguları çalıştırabilir. Bir masaüstü simgesi kullanarak, bir grafik Masaüstü oturumundan (örneğin, X2Go istemcisi aracılığıyla) SQUIRREL SQL 'i çalıştırabilirsiniz. Ya da kabuğu içinde aşağıdaki komutu kullanarak istemcisini çalıştırabilirsiniz:

```bash
/usr/local/squirrel-sql-3.7/squirrel-sql.sh
```

İlk kullanılmadan önce sürücüleri ve veritabanı diğer adlar ayarlayın. JDBC sürücüleri/usr/share/Java/jdbcdrivers. adresinde bulunur

Daha fazla bilgi için [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Microsoft SQL Server'a erişmek için komut satırı araçları

SQL Server için ODBC sürücü paketi ayrıca iki komut satırı araçları ile birlikte gelir:

- **bcp**: BCP Aracı, bir Microsoft SQL Server örneği ve Kullanıcı tarafından belirtilen biçimdeki bir veri dosyası arasında verileri toplu olarak kopyalar. BCP aracını kullanarak çok sayıda yeni satırı SQL Server tablolarına içeri aktarabilir veya tablolardaki verileri veri dosyalarına aktarabilirsiniz. Verileri bir tabloya aktarmak için, bu tablo için oluşturulmuş bir biçim dosyası kullanmanız gerekir. Ya da tablonun yapısını ve sütunlarının geçerli olan veri türlerini anlamanız gerekir.

  Daha fazla bilgi için [bcp ile bağlanma](https://msdn.microsoft.com/library/hh568446.aspx).

- **sqlcmd**: Sqlcmd aracını kullanarak Transact-SQL deyimlerini girebilirsiniz. Ayrıca, komut istemine sistem yordamları ve betik dosyaları da girebilirsiniz. Bu araç Transact-SQL toplu işlerini çalıştırmak için ODBC kullanır.

  Daha fazla bilgi için [sqlcmd ile bağlanma](https://msdn.microsoft.com/library/hh568447.aspx).

  > [!NOTE]
  > Bu araçta Linux ve Windows platformları arasında bazı farklılıklar vardır. Ayrıntılar için belgelere bakın.

### <a name="database-access-libraries"></a>Veritabanı erişimi kitaplıkları

Kitaplıklar, veritabanı erişimi için R ve Python 'da kullanılabilir:

* R 'de, RODBC paketini veya dplyr paketini kullanarak veritabanı sunucusundaki SQL deyimlerini sorgulayabilir veya çalıştırabilirsiniz.
* Python 'da pyodbc kitaplığı, temel alınan katman olarak ODBC ile veritabanı erişimi sağlar.  

## <a name="azure-tools"></a>Azure Araçları

Aşağıdaki Azure Araçları VM'de yüklü:

* **Azure CLI**: Azure 'da kabuk komutları aracılığıyla Azure kaynakları oluşturmak ve yönetmek için komut satırı arabirimini kullanabilirsiniz. Azure araçlarını açmak için **Azure yardımı**'nı girin. Daha fazla bilgi için [Azure CLI belgeleri sayfasını](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* **Azure Depolama Gezgini**: Azure Depolama Gezgini, Azure depolama hesabınızda depoladığınız nesneler üzerinde gezinmek ve Azure bloblarına ve verileri yüklemek ve buradan veri indirmek için kullanabileceğiniz bir grafik aracıdır. Depolama Gezgini masaüstü kısayolu simgesinden erişebilirsiniz. Ayrıca, bunu bir kabuk isteminden **Storageexplorer**girerek açabilirsiniz. Bir X2Go istemcisinden oturum açmanız veya X11 iletme ayarlamış olmanız gerekir.
* **Azure kitaplıkları**: Önceden yüklenmiş kitaplıkların bazıları aşağıda verilmiştir.
  
  * **Python**: Python 'daki Azure ile ilgili kitaplıklar *Azure*, *azureml*, *pydocumentdb*ve *pyodbc*' dir. İlk üç kitaplıkları ile Azure depolama hizmetleri, Azure Machine Learning ve Azure Cosmos DB (Azure üzerinde bir NoSQL veritabanı) erişebilir. Dördüncü kitaplığı pyodbc (yanı sıra Microsoft ODBC sürücüsü için SQL Server) erişimi etkinleştirir SQL Server, Azure SQL veritabanı ve Azure SQL veri ambarı python'dan ODBC arabirimini kullanarak. Girin **pip listesi** listelenen tüm kitaplıkları görmek için. Hem Python 2.7 hem de 3,5 ortamlarında bu komutu çalıştırmak emin olun.
  * **R**: R 'deki Azure ile ilgili kitaplıklar AzureML ve RODBC.
  * **Java**: Azure Java kitaplıklarının listesi VM 'de/dsvm/sdk/AzureSDKJava dizininde bulunabilir. Anahtar kitaplıkları, SQL Server için Azure depolama ve Yönetimi API'leri, Azure Cosmos DB ve JDBC sürücüleri vardır.  

Erişebildiğiniz [Azure portalında](https://portal.azure.com) önceden yüklenmiş Firefox tarayıcısı. Azure portalında, oluşturmak, yönetmek ve Azure kaynaklarınızı izleyin.

## <a name="azure-machine-learning"></a>Azure Machine Learning

Azure Machine Learning oluşturmanızı, dağıtmanızı ve Tahmine dayalı analiz çözümlerini sağlayan tam olarak yönetilen bir bulut hizmetidir. Azure Machine Learning Studio'dan denemeleri ve modeller oluşturun. [Microsoft Azure Machine Learning](https://studio.azureml.net)ziyaret ederek veri bilimi sanal makinesi bir Web tarayıcısından erişebilirsiniz.

Azure Machine Learning Studio oturum açtıktan sonra, makine öğrenimi algoritmaları için bir mantıksal akış oluşturmak üzere bir deneme tuvali kullanabilirsiniz. Ayrıca, Azure Machine Learning barındırılan bir Jupyter not defterine erişiminiz vardır ve Machine Learning Studio ' de denemeleri ile sorunsuz bir şekilde çalışabilir. 

Machine learning web hizmeti arabiriminde sarmalama tarafından oluşturulmuş modelleri kullanıma hazır hale getirin. Makine öğrenimi modelleri, herhangi bir dilde yazılmış istemcilerin bu modellerdeki tahminleri çağırmasına olanak sağlar. Daha fazla bilgi için [Machine Learning belgeleri](https://azure.microsoft.com/documentation/services/machine-learning/).

Ayrıca, modellerinizi VM 'de R veya Python 'da oluşturabilir ve ardından bunları Azure Machine Learning üretim ortamında dağıtabilirsiniz. Biz R kitaplıkları yüklü (**AzureML**) ve Python (**azureml**) bu işlevselliği etkinleştirmek için.

R ve Python 'da modellerin Azure Machine Learning içine nasıl dağıtılacağı hakkında bilgi için, bkz. [veri bilimi sanal makinesi üzerinde yapabileceğiniz on şey](vm-do-ten-things.md).

> [!NOTE]
> Bu yönergeler Veri Bilimi Sanal Makinesi Windows sürümü için yazılmıştır. Ancak bilgileri var. için Azure Machine Learning modelleri dağıtma hakkında Linux VM'ye uygun sağlanır.

## <a name="machine-learning-tools"></a>Machine learning araçları

VM, önceden derlenmiş ve yerel olarak önceden yüklenmiş makine öğrenimi araçları ve algoritmalarıyla birlikte gelir. Bunlar:

* **Vowpal Wabbit**: Hızlı bir çevrimiçi öğrenme algoritması.
* **xgboost**: İyileştirilmiş, artırılmış ağaç algoritmaları sağlayan bir araç.
* **Rattle**: Kolay veri araştırması ve modelleme için R tabanlı bir grafik araç.
* **Python**: Anaconda Python, Scikit-öğren gibi kitaplıklar ile makine öğrenimi algoritmalarıyla birlikte sunulur. Diğer kitaplıkları kullanarak yükleyebileceğiniz `pip install` komutu.
* **Lightgbm**: Karar ağacı algoritmalarını temel alan hızlı, dağıtılmış ve yüksek performanslı bir gradyan arttırma çerçevesi.
* **R**: R için zengin bir makine öğrenimi işlevleri kitaplığı vardır. önceden yüklenmiş kitaplıklar lm, GLM, Rasgeleforest ve rpart ' ı içerir. Şu komutu çalıştırarak diğer kitaplıkları yükleyebilirsiniz:
  
        install.packages(<lib name>)

Listedeki ilk üç machine learning araçları hakkında bazı ek bilgiler aşağıda verilmiştir.

### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit olan bir makine öğrenimi çevrimiçi, karma, allreduce, indirimleri, learning2search, etkin, gibi teknikler kullanan sistemi ve etkileşimli öğrenme.

Aracı temel bir örnekte çalıştırmak için aşağıdaki komutları kullanın:

```bash
cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
cd vwdemo
vw house_dataset
```

Bu dizinde büyük, diğer tanıtımlar vardır. Vowpal Wabbit hakkında daha fazla bilgi için GitHub ve [Vowpal Wabbit wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki) ['nin bu bölümüne](https://github.com/JohnLangford/vowpal_wabbit) bakın.

### <a name="xgboost"></a>xgboost

Xgboost kitaplığı, artırılmış (ağaç) algoritmaları için tasarlanmış ve iyileştirilmiştir. Amacı, bu kitaplık, büyük ölçekli ağacı, yükseltme sağlamak için gerekli uç makineler hesaplama sınırlarını göndermek için ölçeklenebilir, taşınabilir ve doğru ' dir.

Bu, bir komut satırı ve R kitaplığı olarak sunulur. Bu kitaplığı R 'de kullanmak için etkileşimli bir R oturumu başlatabilir (bir **r** 'yi kabuğa girerek) ve kitaplığı yükleyebilirsiniz.

İşte, bir R isteminde çalıştırabileceğiniz basit bir örnek:

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

Xgboost komut satırını çalıştırmak için, kabukta çalıştırılacak komutlar aşağıda verilmiştir:

```bash
cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
cd xgboostdemo
xgboost mushroom.conf
```

Bir. model dosyası belirtilen dizine yazılır. Bu demo örneği hakkındaki bilgileri [GitHub '](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification)da bulabilirsiniz.

Xgboost hakkında daha fazla bilgi için, bkz. [xgboost belgeleri sayfası](https://xgboost.readthedocs.org/en/latest/) ve [GitHub deposu](https://github.com/dmlc/xgboost).

### <a name="rattle"></a>Çıngırağı

Rattle ( **R** **A**nalitik **T**aracı **T**o **L**kazanın **E** GUI tabanlı bir veri keşfi ve modelleme asily) kullanır. Veriler, kolayca modellenebilir, denetimli hem de Denetimsiz modellerinden veri yapıları, grafik performansını modelleri sunar dönüşümler veriler istatistiksel ve görsel özetlerini sunar ve puanları yeni veri kümeleri. Ayrıca R kodunu doğrudan R çalıştırmak veya daha fazla analiz için başlangıç noktası olarak kullanılan işlemleri kullanıcı arabiriminde çoğaltma oluşturur.

Çıngırağı çalıştırmak için bir grafik Masaüstü Oturum açma oturumunda olması gerekir. Terminal **üzerinde r ' yi girerek r** ortamını açın. R isteminde aşağıdaki komutları girin:

```R
library(rattle)
rattle()
```

Artık bir sekme kümesiyle birlikte bir grafik arabirimi açılır. Örnek bir hava durumu verisi kümesi kullanmak ve bir model oluşturmak için Rattle içindeki aşağıdaki hızlı başlangıç adımlarını kullanın. Bazı adımlarda, zaten sistemde olmayan bazı gerekli R paketlerini otomatik olarak yükleyip yüklemeniz istenir.

> [!NOTE]
> Paketi sistem dizinine (varsayılan) yüklemek için erişiminiz yoksa, kişisel kitaplığınıza paket yüklemek için R konsol pencerenizde bir istem görürsünüz. Yanıt **y** varsa, bu yönergeleri.

1. **Yürüt**’ü seçin.
1. Örnek Hava durumu veri kümesini kullanmak isteyip istemediğinizi soran bir iletişim kutusu görüntülenir. Örneği yüklemek için **Evet** ' i seçin.
1. **Model** sekmesini seçin.
1. Bir karar ağacı oluşturmak için **Yürüt** ' ü seçin.
1. Karar ağacını göstermek için **Çiz** ' i seçin.
1. **Orman** seçeneğini belirleyin ve rastgele bir orman oluşturmak için **Yürüt** ' ü seçin.
1. **Değerlendir** sekmesini seçin.
1. **Risk** seçeneğini belirleyin ve iki **risk (birikmeli)** performans grafiğini göstermek için **Yürüt** ' ü seçin.
1. Önceki işlemler için oluşturulan R kodunu göstermek üzere **günlük** sekmesini seçin.
   (Rattle 'in geçerli sürümündeki bir hata nedeniyle bu günlüğü, günlüğün metninde **dışarı aktar** önüne bir **#** karakter eklemeniz gerekir.)
1. Weather_script adlı R betiği dosyasını kaydetmek için **dışarı aktar** düğmesini seçin *.* Ana klasöre R.

Rattle ve R 'den çıkabilirsiniz. Artık oluşturulan R betiğini değiştirebilirsiniz. Ya da, komut dosyasını olduğu gibi kullanın ve Rattle Kullanıcı arabiriminde yapılan her şeyi yinelemek için dilediğiniz zaman çalıştırın. Özellikle R 'deki yeni başlayanlar için bu, bir basit grafik arabiriminde analiz ve makine öğrenimini hızlı bir şekilde yapmanın bir yoludur. böylece, değiştirmek veya öğrenmek için R 'de otomatik olarak kod oluşturma.

## <a name="next-steps"></a>Sonraki adımlar

Başka sorularınız mı var? Bir [destek bileti](https://azure.microsoft.com/support/create-ticket/)oluşturmayı düşünün.