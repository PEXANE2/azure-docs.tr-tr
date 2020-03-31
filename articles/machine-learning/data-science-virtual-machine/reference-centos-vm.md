---
title: 'Referans: CentOS Veri Bilimi Sanal Makine'
titleSuffix: Azure Data Science Virtual Machine
description: CentOS Data Science Virtual Machine'de yer alan araçlarla ilgili ayrıntılar
author: gvashishtha
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: gopalv
ms.date: 09/11/2019
ms.topic: reference
ms.openlocfilehash: db49a9f5e0e6675d93cb58d6af9c92fac21e8b74
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77525847"
---
# <a name="reference-centos-linux-data-science-virtual-machine"></a>Referans: CentOS (Linux) Veri Bilimi Sanal Makine

Linux Data Science Virtual Machine (DSVM), CentOS tabanlı bir Azure sanal makinesidir. Linux DSVM, veri analitiği ve makine öğrenimi için kullanabileceğiniz önceden yüklenmiş araçlar koleksiyonuyla birlikte gelir. 

Linux DSVM'de yer alan temel yazılım bileşenleri şunlardır:

* Linux CentOS dağıtım işletim sistemi.
* Microsoft Machine Learning Server.
* Anaconda Python dağıtımı (sürüm 3.5 ve 2.7), popüler veri analizi kitaplıkları da dahil olmak üzere.
* JuliaPro, Julia dili ve popüler bilimsel ve veri analitiği kütüphanelerinin küratörlüğünde bir dağıtım.
* Kıvılcım Bağımsız örnek ve tek düğüm Hadoop (HDFS, YARN).
* JupyterHub, R, Python, PySpark ve Julia çekirdeklerini destekleyen çok kullanıcılı bir Jupyter dizüstü bilgisayar sunucusu.
* Azure Depolama Gezgini.
* Azure CLI, Azure kaynaklarını yönetmek için Azure komut satırı arabirimi.
* PostgresSQL veritabanı.
* Makine öğrenimi araçları:
  * [Microsoft Cognitive Toolkit](https://github.com/Microsoft/CNTK) (CNTK), Microsoft Research'ün derin öğrenme yazılım araç seti.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit), online gibi teknikleri destekleyen hızlı bir makine öğrenme sistemi, karma, allreduce, azaltma, learning2search, aktif, ve interaktif öğrenme.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/), hızlı ve doğru artırılmış ağaç uygulaması sağlayan bir araç.
  * R'de veri analitiği ve makine öğrenimine başlamayı kolaylaştıran bir araç olan [Rattle.](https://togaware.com/rattle/) Rattle, otomatik R kodu oluşturma yı kullanarak hem GUI tabanlı veri arama ve modelleme sunar.
* Java, Python, Node.js, Ruby ve PHP'de Azure SDK.
* Azure Machine Learning ve diğer Azure hizmetlerinde kullanılacak R ve Python kitaplıkları.
* Geliştirme araçları ve editörler (RStudio, PyCharm, IntelliJ, Emacs, gedit, vi).

Veri bilimi, bir dizi görevi yineler:

1. Verileri bulun, yükleyin ve işlem öncesi verin.
1. Modeller oluşturun ve test edin.
1. Akıllı uygulamalarda tüketim için modelleri dağıtın.

Veri bilim adamları bu görevleri tamamlamak için çeşitli araçlar kullanın. Yazılımın doğru sürümlerini bulmak ve yazılımı indirmek, derlemek ve yüklemek zaman alabilir.

Linux DSVM bu yükü önemli ölçüde hafifletebilir. Analitik projenizi başlatmak için Linux DSVM'yi kullanın. Linux DSVM, R, Python, SQL, Java ve C++ gibi çeşitli dillerdeki görevler üzerinde çalışmanıza yardımcı olur. Eclipse, kodunuzu geliştirmek ve test etmek için kullanımı kolay bir IDE sağlar. DSVM'ye dahil olan Azure SDK, Microsoft bulut platformu için Linux'ta çeşitli hizmetler kullanarak uygulamalarınızı oluşturmanıza yardımcı olur. Ruby, Perl, PHP ve Node.js gibi diğer diller önceden yüklenmiştir.

DSVM görüntüsü için yazılım ücreti alınmaz. Yalnızca DSVM görüntüsüyle birlikte sağladığınız sanal makinenin boyutuna göre değerlendirilen Azure donanım kullanım ücretlerini ödlersiniz. Bilgi işlem ücretleri hakkında daha fazla bilgi için Azure Marketi'ndeki [Linux için Veri Bilimi Sanal Makine (CentOS) listesine](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/) bakın.


## <a name="machine-learning-server"></a>Makine Öğrenme Sunucusu

R, veri analizi ve makine öğrenimi için en popüler dillerden biridir. Analitiğiniz için R'yi kullanmak istiyorsanız, DSVM'de Microsoft R Open ve Math Kernel Library'ye sahip Machine Learning Server bulunur. Math Kernel Library analitik algoritmalarda yaygın matematik işlemlerini optimize eder. R Open, CRAN R ile tamamen uyumludur. CRAN'da yayınlanan R kütüphanelerinden herhangi biri R Open'a yüklenebilir. 

R modellerini web hizmetlerine ölçeklendirmek ve işlevsel hale getirmek için Machine Learning Server'ı kullanabilirsiniz. R programlarınızı RStudio, vi veya Emacs gibi varsayılan düzenleyicilerden birinde edinebilirsiniz. Emacs düzenleyicisi DSVM'ye önceden yüklenmiş. Emacs ESS (Emacs Speaks Statistics) paketi, Emacs düzenleyicisindeki R dosyalarıyla çalışmayı kolaylaştırır.

R konsolu açmak için, kabuk içinde, **R**girin . Bu komut sizi etkileşimli bir ortama götürür. R programınızı geliştirmek için genellikle Emacs veya vi gibi bir düzenleyici kullanırsınız ve r. RStudio'daki komut dosyalarını çalıştırırsınız ve R programınızı geliştirmek için tam bir grafik IDE sunar.

[En iyi 20 R paketlerini](https://www.kdnuggets.com/2015/06/top-20-r-packages.html) yüklemek için kullanabileceğiniz bir R komut dosyası DSVM'ye dahildir. R etkileşimli arabirimindeyken bu komut dosyasını çalıştırabilirsiniz. Daha önce de belirtildiği gibi, bu arayüzü açmak için, kabuk, **R**girin .  

## <a name="python"></a>Python

Anaconda Python Python 3.5 ve 2.7 ortamları ile yüklenir. 2.7 _ortama kök,_ 3.5 ortamına _py35_denir. Bu dağılım, en popüler matematik, mühendislik ve veri analizi paketlerinden yaklaşık 300'ünün yanı sıra temel Python'u da içerir.

Py35 ortamı varsayılandır. Kök (2.7) ortamını etkinleştirmek için şu komutu kullanın:

```bash
source activate root
```

Py35 ortamını yeniden etkinleştirmek için şu komutu kullanın:

```bash
source activate py35
```

Python etkileşimli oturumçağırmak için, kabuk içinde, **python**girin. 

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

## <a name="jupyter-notebook"></a>Jupyter Notebook

Anaconda dağıtım da jupyter Notebook, kod ve analiz paylaşmak için bir ortam ile birlikte gelir. Jupyter Notebook'a JupyterHub üzerinden erişin. Yerel Linux kullanıcı adınızı ve şifrenizi kullanarak oturum açın.

Jupyter Notebook sunucusu Python 2, Python 3 ve R çekirdekleri ile önceden yapılandırılmıştır. Tarayıcıyı açmak ve Jupyter Notebook sunucusuna erişmek için **Jupyter Notebook** masaüstü simgesini kullanın. DSVM'ye SSH veya X2Go istemcisi üzerinden erişirseniz, https:\//localhost:8000/ adresindeki Jupyter Notebook sunucusuna da erişebilirsiniz.

> [!NOTE]
> Sertifika uyarıları alırsanız devam edin.

Jupyter dizüstü bilgisayar sunucusuna herhangi bir ana bilgisayardan erişebilirsiniz. **https\//\<girin: DSVM DNS\>adı veya IP adresi :8000/**.

> [!NOTE]
> DSVM sağlandığında bağlantı noktası 8000 varsayılan olarak güvenlik duvarında açılır. 

Microsoft, biri Python'da, diğeri R'de olmak üzere örnek not defterlerini paketlemiştir. Yerel Linux kullanıcı adınızı ve şifrenizi kullanarak Jupyter Notebook'a kimlik doğrulaması yaptıktan sonra Jupyter Notebook ana sayfasındaörneklerin bağlantısını görebilirsiniz. Yeni bir not defteri oluşturmak için **Yeni'yi**seçin ve ardından kullanmak istediğiniz dil çekirdeğini seçin. **Yeni** düğmesini görmüyorsanız, not defteri sunucusunun ana sayfasına gitmek için sol üstteki **Jupyter** simgesini seçin.

## <a name="spark-standalone"></a>Kıvılcım Tek Başına 

Spark Bağımsız modunun bir örneği, spark uygulamalarını büyük kümeler üzerinde test etmeden ve dağıtmadan önce yerel olarak geliştirmenize yardımcı olmak için Linux DSVM'de önceden yüklenir. 

PySpark programlarını Jupyter çekirdeğinden çalıştırabilirsiniz. Jupyter'ı açtığınızda, **Yeni** düğmesini seçin ve kullanılabilir çekirdeklerin listesini görmeniz gerekir. **Spark - Python,** Python dilini kullanarak Spark uygulamalarını oluşturmanıza olanak tanıyan PySpark çekirdeğidir. Ayrıca Spark programı oluşturmak için PyCharm veya Spyder gibi bir Python IDE kullanabilirsiniz. 

Bu bağımsız durumda, Spark yığını çağıran istemci programında çalışır. Bu özellik, bir Kıvılcım kümesinde geliştirmeye kıyasla sorunları daha hızlı ve daha kolay gidermeyi sağlar.

Jupyter örnek bir PySpark dizüstü bilgisayar sağlar. Jupyter ev dizini altında SparkML dizini bulabilirsiniz ($HOME / dizüstü / SparkML / pySpark). 

R for Spark'ta programlıyorsanız, Machine Learning Server, SparkR veya Sparklyr'ı kullanabilirsiniz. 

Machine Learning Server'da Bir Kıvılcım bağlamında çalıştırmadan önce, yerel tek düğümlü Hadoop HDFS ve İPN örneğini etkinleştirmek için tek seferlik bir kurulum adımı yapmanız gerekir. Varsayılan olarak, Hadoop hizmetleri yüklenir, ancak DSVM'de devre dışı bırakılır. Hadoop hizmetlerini etkinleştirmek için, aşağıdaki komutları ilk kez kök olarak çalıştırın:

```bash
echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
chmod 0600 ~hadoop/.ssh/authorized_keys
chown hadoop:hadoop ~hadoop/.ssh/id_rsa
chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
systemctl start hadoop-namenode hadoop-datanode hadoop-yarn
```

Hadoop ile ilgili hizmetleri çalıştırarak `systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn`bunlara ihtiyacınız olmadığında durdurabilirsiniz.

/dsvm/samples/MRS dizini, Machine Learning Server'ın uzak bir Kıvılcım bağlamında nasıl geliştirileceği ve test edilebildiğini gösteren bir örnek sağlar (DSVM'deki bağımsız Kıvılcım örneği).

## <a name="ides-and-editors"></a>IID'ler ve editörler

Vi/VIM, Emacs, gedit, PyCharm, RStudio, Eclipse, LaTeX ve IntelliJ gibi çeşitli kod editörleri arasından seçim yapabilirsiniz. 

* gedit, Eclipse, IntelliJ, R Studio ve PyCharm grafik editörleridir. Bunları kullanmak için grafik bir masaüstünde oturum açmış olmalısınız. Masaüstü ve uygulama menüsü kısayollarını kullanarak açarsınız.

* Vim ve Emacs metin tabanlı editörlerdir. Emacs'ta ESS eklenti paketi, Emacs düzenleyicisinde R ile çalışmayı kolaylaştırır. Ess [web sitesinde](https://ess.r-project.org/)daha fazla bilgi bulabilirsiniz.

* Eclipse, birden çok dili destekleyen açık kaynak kodlu, genişletilebilir bir IDE'dir. Java Geliştiriciler için Eclipse IDE sürümü DSVM yüklü. Ortamı genişletmek için birkaç popüler dil için eklentiler yükleyebilirsiniz. 

  Eclipse eklentisi için Azure Araç Seti, Eclipse ile DSVM'de yüklüdür. Azure Araç Kiti'ni, Java gibi dilleri destekleyen Eclipse geliştirme ortamını kullanarak Azure uygulamaları oluşturmak, geliştirmek, sınamak ve dağıtmak için Eclipse için Azure Araç Kiti'ni kullanabilirsiniz.

  Java için Azure SDK, DSVM'deki Eclipse için Azure Araç Seti ile de yüklenir. Java için Azure SDK, Java ortamından farklı Azure hizmetlerine erişmenizi sağlar. 
  
  Daha fazla bilgi için bkz. [Azure Toolkit for Eclipse](/java/azure/eclipse/azure-toolkit-for-eclipse).

* LaTeX texlive paketi üzerinden, [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html)denilen bir Emacs eklenti paketi ile birlikte yüklenir. Bu paket, LaTeX belgelerinizi Emacs'ta yazmanızı kolaylaştırır. 

## <a name="databases"></a>Veritabanları

Linux DSVM çeşitli veritabanı ve komut satırı araçlarına erişim sağlar.

### <a name="postgressql"></a>PostgresSQL

Açık kaynak veritabanı PostgresSQL, DSVM'de kullanılabilir ve hizmetler çalışır ve initdb tamamlanır. Veritabanları ve kullanıcılar oluşturmanız gerekir. Daha fazla bilgi için [PostgresSQL belgelerine](https://www.postgresql.org/docs/)bakın.  

### <a name="squirrel-sql"></a>SQuirreL SQL

SQuirreL SQL, çeşitli veritabanlarına (SQL Server, PostgresSQL ve MySQL dahil) bağlanabilen ve SQL sorgularını çalıştırabilen grafiksel bir SQL istemcisidir. Bir masaüstü simgesi kullanarak squirreL SQL'i grafik sel masaüstü oturumundan (örneğin X2Go istemcisi aracılığıyla) çalıştırabilirsiniz. Veya kabuktaki aşağıdaki komutu kullanarak istemciyi çalıştırabilirsiniz:

```bash
/usr/local/squirrel-sql-3.7/squirrel-sql.sh /usr/local/squirrel-sql-3.7/squirrel-sql.sh
```

İlk kullanımdan önce, sürücülerinizi ve veritabanı takma adlarınızı ayarlayın. JDBC sürücüleri /usr/share/java/jdbcdrivers adresinde bulunmaktadır.

Daha fazla bilgi için [Bkz. SQuirreL SQL.](http://squirrel-sql.sourceforge.net/index.php?page=screenshots)

### <a name="command-line-tools-for-accessing-sql-server"></a>SQL Server'a erişmek için komut satırı araçları

SQL Server için ODBC sürücü paketi de iki komut satırı araçları ile birlikte gelir:

* **bcp**: BCP aracı, SQL Server örneği ile kullanıcı tarafından belirtilen biçimde bir veri dosyası arasındaki verileri toplu kopyalar. BCP aracını, çok sayıda yeni satırı SQL Server tablolarına aktarmak veya tablolardaki verileri veri dosyalarına aktarmak için kullanabilirsiniz. Verileri tabloya almak için, bu tablo için oluşturulan biçim dosyası kullanmanız gerekir. Veya, tablonun yapısını ve sütunları için geçerli olan veri türlerini anlamanız gerekir.

  Daha fazla bilgi için [bcp ile bağlan'a](https://msdn.microsoft.com/library/hh568446.aspx)bakın.

* **sqlcmd**: Transact-SQL deyimlerini, sistem yordamlarını ve komut dosyalarını komut istemine girmek için sqlcmd yardımcı programını kullanabilirsiniz. Sqlcmd yardımcı programı, Transact-SQL toplu işlerini yürütmek için ODBC kullanır.

  Daha fazla bilgi için [sqlcmd ile Bağlan'a](https://msdn.microsoft.com/library/hh568447.aspx)bakın.

  > [!NOTE]
  > Linux ve Windows platformları arasında bu araçta bazı farklılıklar vardır. Ayrıntılar için belgelere bakın.

### <a name="database-access-libraries"></a>Veritabanı erişim kitaplıkları

Veritabanı erişimi için kitaplıklar R ve Python'da kullanılabilir:

* R'de, veritabanı sunucusunda SQL deyimlerini sorgulamak veya çalıştırmak için RODBC paketini veya dplyr paketini kullanabilirsiniz.
* Python'da pyodbc kitaplığı, altta yatan katman olarak ODBC ile veritabanı erişimi sağlar.

## <a name="azure-tools"></a>Azure araçları

Aşağıdaki Azure araçları DSVM'ye yüklenir:

* **Azure CLI**: Azure'daki komut satırı arabirimini kullanarak Azure kaynaklarını kabuk komutları aracılığıyla oluşturabilir ve yönetebilirsiniz. Azure araçlarını açmak için **azure yardımı**girin. Daha fazla bilgi için [Azure CLI dokümantasyon sayfasına](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)bakın.
* **Azure Depolama Gezgini**: Azure Depolama Gezgini, Azure depolama hesabınızda depoladığınız nesnelere göz atmak ve Azure blob'larına veri yüklemek ve indirmek için kullanabileceğiniz bir grafik aracıdır. Depolama Gezgini'ne masaüstü kısayol simgesinden erişebilirsiniz. Ayrıca **StorageExplorer**girerek bir kabuk istemi açabilirsiniz. Bir X2Go istemcisinden oturum açmış olmalısınız veya X11 iletme kurulumu nuz gerekir.
* **Azure kitaplıkları**: Aşağıdaki kitaplıklar DSVM'ye önceden yüklenir:
  
  * **Python**: Python'daki Azure ile ilgili kitaplıklar *azure*, *azureml*, *pydocumentdb*ve *pyodbc'dir.* İlk üç kitaplıkla Azure depolama hizmetlerine, Azure Machine Learning'e ve Azure Cosmos DB'ye (Azure'da NoSQL veritabanı) erişebilirsiniz. Dördüncü kitaplık pyodbc (SQL Server için Microsoft ODBC sürücüsü ile birlikte), ODBC arabirimi kullanarak Python'dan SQL Server, Azure SQL Veritabanı ve Azure SQL Veri Ambarı'na erişim sağlar. Listelenen tüm kitaplıkları görmek için **pip listesini** girin. Bu komutu hem Python 2.7 hem de 3.5 ortamlarında çalıştırdığından emin olun.
  * **R**: R'deki Azure ile ilgili kitaplıklar AzureML ve RODBC'dir.
  * **Java**: Azure Java kitaplıklarının listesi DSVM'deki /dsvm/sdk/AzureSDKJava dizininde bulunabilir. Önemli kitaplıklar, SQL Server için Azure depolama ve yönetim API'leri, Azure Cosmos DB ve JDBC sürücüleridir.  

[Azure portalına](https://portal.azure.com) önceden yüklenmiş Firefox tarayıcısından erişebilirsiniz. Azure portalında Azure kaynaklarını oluşturabilir, yönetebilir ve izleyebilirsiniz.

## <a name="azure-machine-learning"></a>Azure Machine Learning

Azure Machine Learning, tahmine dayalı analitik çözümleri oluşturmak, dağıtmak ve paylaşmak için kullanabileceğiniz tam olarak yönetilen bir bulut hizmetidir. Denemelerinizi ve modellerinizi Azure Machine Learning Studio'dan (klasik) oluşturursunuz. Azure Machine Learning'e DSVM'deki bir web tarayıcısından erişmek için [Microsoft Azure Machine Learning'e](https://studio.azureml.net)gidin.

Azure Machine Learning Studio'da (klasik) oturum imzaladıktan sonra, makine öğrenimi algoritmaları için mantıksal bir akış oluşturmak için bir deneme tuvali kullanabilirsiniz. Azure Machine Learning'de barındırılan bir Jupyter Notebook'a da erişebilirsiniz. Not defteri, Azure Machine Learning Studio'daki (klasik) denemelerle sorunsuz bir şekilde çalışabilir. 

Oluşturduğunuz makine öğrenimi modellerini bir web hizmeti arabirimine sararak operasyonel hale getirin. Makine öğrenimi modellerinin operasyonelleştirilmesi, herhangi bir dilde yazılmış istemcilerin bu modellerden öngörüler çağırmasına olanak tanır. Daha fazla bilgi için [Machine Learning belgelerine](https://azure.microsoft.com/documentation/services/machine-learning/)bakın.

Ayrıca modellerinizi DSVM'de R veya Python'da oluşturabilir ve bunları Azure Machine Learning'de üretime dağıtabilirsiniz. Microsoft, bu işlevselliği desteklemek için R (**AzureML**) ve Python **(azureml)** kitaplıklarını yüklemiştir.

R ve Python'daki modelleri Azure Machine Learning'e nasıl dağıtabileceğiniz hakkında bilgi için [Data Science Virtual Machine'de yapabileceğiniz on şeye](vm-do-ten-things.md)bakın.

> [!NOTE]
> [Veri Bilimi Sanal Makine](vm-do-ten-things.md) yapabileceğiniz On şey talimatları DSVM Windows sürümü için yazılmıştır. Ancak, modelleri Azure Machine Learning'e dağıtma hakkındaki bilgiler Linux DSVM için de geçerlidir.

## <a name="machine-learning-tools"></a>Makine öğrenimi araçları

DSVM, önceden derlenmiş ve yerel olarak önceden yüklenmiş birkaç makine öğrenimi aracı ve algoritması ile birlikte gelir. Bunlar:

* **Microsoft Cognitive Toolkit**: Derin öğrenme araç seti.
* **Vowpal Wabbit**: Hızlı bir online öğrenme algoritması.
* **XGBoost**: Optimize edilmiş, artırılmış ağaç algoritmaları sağlayan bir araçtır.
* **Python**: Anaconda Python, Scikit-learn gibi kütüphanelerle birlikte makine öğrenimi algoritmalarıyla birlikte gelir. Komutu kullanarak diğer kitaplıkları `pip install` yükleyebilirsiniz.
* **R**: R. Önceden yüklenmiş kütüphaneler için zengin bir makine öğrenimi işlevleri kütüphanesi mevcuttur lm, glm, randomForest ve rpart. Çalıştırarak diğer kitaplıkları `install.packages(<lib name>)`yükleyebilirsiniz.

Microsoft Bilişsel Araç Seti, Vowpal Wabbit ve XGBoost sonraki bölümlerde daha ayrıntılı olarak ele alınmıştır.

### <a name="microsoft-cognitive-toolkit"></a>Microsoft Bilişsel Araç Seti

Microsoft Cognitive Toolkit açık kaynak kodlu bir derin öğrenme araç setidir. Bu bir komut satırı aracı (CNTK) ve zaten PATH'de.

Temel bir örneği çalıştırmak için kabuktaki aşağıdaki komutları çalıştırın:

```bash
cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
cntk configFile=lr_bs.cntk makeMode=false command=Train
```

Daha fazla bilgi için [GitHub CNTK deposuna](https://github.com/Microsoft/CNTK) ve [CNTK wiki'ye](https://github.com/Microsoft/CNTK/wiki)bakın.

### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit online, karma, allreduce, azaltma, learning2search, aktif ve interaktif öğrenme gibi teknikleri kullanan bir makine öğrenme sistemidir.

Aracı temel bir örnekte çalıştırmak için aşağıdaki komutları çalıştırın:

```bash
cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
cd vwdemo
vw house_dataset
```

Vowpal Wabbit demo dizini diğer, daha büyük demolar içerir. Vowpal Wabbit hakkında daha fazla bilgi için [GitHub Vowpal Wabbit deposu](https://github.com/JohnLangford/vowpal_wabbit) ve [Vowpal Wabbit wiki sayfasına](https://github.com/JohnLangford/vowpal_wabbit/wiki)bakın.

### <a name="xgboost"></a>XGBoost

XGBoost kitaplığı artırılmış (ağaç) algoritmaları için tasarlanmış ve optimize edilmiştir. XGBoost kütüphanenin amacı ölçeklenebilir, taşınabilir ve doğru büyük ölçekli ağaç artırma sağlamak için gerekli aşırı makinelerin hesaplama sınırlarını itmektir.

XGBoost bir komut satırı ve R kitaplığı olarak sağlanır.

R'deki XGBoost kitaplığını kullanmak için etkileşimli bir R oturumu başlatın (kabukta, **R**girin) ve ardından kitaplığı yükleyin.

Aşağıda, R komut isteminde çalıştırabileceğiniz basit bir örnek verilmiştir:

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

XGBoost komut satırını çalıştırmak için, bu komutları kabukta çalıştırın:

```bash
cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
cd xgboostdemo
xgboost mushroom.conf
```

Bir .model dosyası belirtilen dizine yazılır. GitHub'daki bu demo örneği hakkında bilgi için [Ikili Sınıflandırma'ya](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification)bakın.

XGBoost hakkında daha fazla bilgi için, [XGBoost belgeleri](https://xgboost.readthedocs.org/en/latest/) ve [XGBoost GitHub deposuna](https://github.com/dmlc/xgboost)bakın.

### <a name="rattle"></a>Rattle

Çıngırak *(R* *A*nalytical *T*ool *T*o *L*asily *E*kazanmak) GUI tabanlı veri arama ve modelleme kullanır. Çıngırak:
- Verilerin istatistiksel ve görsel özetlerini sunar.
- Kolayca modellenebilen verileri dönüştürür.
- Verilerden hem denetimsiz hem de denetlenen modeller oluşturur.
- Modellerin performansını grafiksel olarak sunar.
- Yeni veri kümeleri puanlar.
- R kodu oluşturur.
- UI'de doğrudan R'de çalıştırılabilen veya daha fazla çözümleme için başlangıç noktası olarak kullanılabilen işlemleri çoğaltıyor.

Rattle'ı çalıştırmak için grafiksel bir masaüstü oturumunda oturum açmış olmalısınız. Bir terminalde, R ortamını açmak için **R** girin. R isteminde aşağıdaki komutları girin:

```R
library(rattle)
rattle()
```

Bir sekme kümesi olan grafik arabirimi açılır. Örnek bir hava durumu veri kümesini kullanmak ve bir model oluşturmak için Rattle'da aşağıdaki hızlı başlatma adımlarını kullanın. Bazı adımlarda, sistemde zaten olmayan bazı gerekli R paketlerini otomatik olarak yüklemeniz ve yüklemeniz istenir.

> [!NOTE]
> Paketi sistem dizinine (varsayılan) yükleme izniniz yoksa, r konsol pencerenizde paketleri kişisel kitaplığınıza yüklemek için bir istem görebilirsiniz. Bu istemleri görürseniz, **y**.

1. **Yürüt**’ü seçin.
1. Bir iletişim kutusu örnek hava durumu veri kümesini yüklemenizi ister. Örneği yüklemek için **Evet'i** seçin.
1. **Model** sekmesini seçin.
1. Karar **Execute** ağacı oluşturmak için Yürüt'''ün'u seçin.
1. Karar ağacını görüntülemek için **Beraberlik'i** seçin.
1. **Orman** seçeneğini seçin ve ardından rasgele bir orman oluşturmak için Yürüt'''ün'u seçin. **Execute**
1. **Değerlendir** sekmesini seçin.
1. **Risk** seçeneğini seçin ve ardından iki **Risk (Kümülatif)** performans çizimi görüntülemek için **Yürüt'ü** seçin.
1. Önceki işlemler için oluşturulan R kodunu göstermek için **Günlük** sekmesini seçin. (Rattle'ın geçerli sürümündeki bir hata nedeniyle, **#** bu günlüğü günlüğün metnine **dışa** aktar'ın önüne bir karakter eklemeniz gerekir.)
1. weather_script adlı R komut dosyası dosyasını kaydetmek için **Dışa** Aktarma düğmesini *seçin. R* ev klasörüne.

Rattle ve R'den çıkabilirsin. Artık oluşturulan R komut dosyasını değiştirebilirsiniz. Veya, komut dosyasını olduğu gibi kullanın ve Rattle UI'de yapılan her şeyi tekrarlamak için her zaman çalıştırın. Özellikle R yeni başlayanlar için, bu hızlı bir şekilde basit bir grafik arayüzü nde analiz ve makine öğrenme yapmak için bir yoldur, otomatik olarak r kod oluştururken değiştirmek veya öğrenmek için.

## <a name="next-steps"></a>Sonraki adımlar

Başka sorularınız mı var? Destek [bileti](https://azure.microsoft.com/support/create-ticket/)oluşturmayı düşünün.