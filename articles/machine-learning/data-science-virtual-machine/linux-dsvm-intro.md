---
title: 'Hızlı Başlangıç: CentOS Linux oluşturma'
titleSuffix: Azure Data Science Virtual Machine
description: Analiz ve makine öğrenimi için Azure 'da bir Linux Veri Bilimi Sanal Makinesi oluşturun ve yapılandırın.
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: quickstart
ms.date: 03/16/2018
ms.openlocfilehash: 1ab3a527643bc5a567644a937fdd1eb9e31bf567
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195341"
---
# <a name="quickstart-set-up-a-linux-centos-data-science-virtual-machine-in-azure"></a>Hızlı Başlangıç: Azure 'da Linux CentOS Veri Bilimi Sanal Makinesi ayarlama

Linux Veri Bilimi Sanal Makinesi (DSVM), bir CentOS tabanlı Azure sanal makinedir. Linux DSVM, veri analizi ve makine öğrenimi için kullanabileceğiniz bir önceden yüklenmiş araçlar koleksiyonuyla birlikte gelir. 

Bir Linux DSVM 'ye dahil edilen anahtar yazılım bileşenleri şunlardır:

* Linux CentOS dağıtım işletim sistemi.
* Microsoft Machine Learning Server.
* Popüler veri analizi kitaplıkları da dahil olmak üzere Anaconda Python dağıtımı (sürüm 3,5 ve 2,7).
* JuliaPro, Julia dilinin ve popüler bilimsel ve veri analizi kitaplıklarının seçkin bir dağılımı.
* Spark tek başına örneği ve tek düğümlü Hadoop (, YARN).
* JupyterHub, R, Python, PySpark ve Julia kernels 'yi destekleyen çok kullanıcılı bir Jupyter Not defteri sunucusudur.
* Azure Depolama Gezgini.
* Azure CLı, Azure kaynaklarını yönetmeye yönelik Azure komut satırı arabirimi.
* PostgresSQL veritabanı.
* Machine Learning araçları:
  * [Microsoft Cognitive Toolkit](https://github.com/Microsoft/CNTK) (CNTK), Microsoft Research 'tan derin bir öğrenme yazılım araç seti.
  * Çevrimiçi, karma, allazaltma, indirimleri, learning2search, etkin ve etkileşimli öğrenme gibi teknikleri destekleyen hızlı bir makine öğrenimi sistemi olan [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit).
  * [Xgboost](https://xgboost.readthedocs.org/en/latest/), hızlı ve doğru şekilde artırılmış ağaç uygulamasını sağlayan bir araçtır.
  * [Rattle](https://togaware.com/rattle/), veri analizi ve makine öğrenimi ile R 'yi kolay bir şekilde kullanmaya başlamanıza olanak tanıyan bir araçtır. Rattle, otomatik R kod üretimi kullanarak hem GUI tabanlı veri araştırması hem de modelleme sağlar.
* Java, Python, Node. js, Ruby ve PHP 'de Azure SDK.
* Azure Machine Learning ve diğer Azure hizmetlerinde kullanılacak olan R ve Python kitaplıkları.
* Geliştirme araçları ve düzenleyiciler (RStudio, Pylt, IntelliJ, Emacs, gedit, vi).

Veri bilimi görevleri bir dizi üzerinde yineleme içerir:

1. Verileri bulun, yükleyin ve ön işleme yapın.
1. Yapı ve model test edin.
1. Akıllı uygulamalarda kullanılmak modelleri dağıtın.

Veri bilimcileri, bu görevleri tamamlamak için çeşitli araçlar kullanın. Yazılımın doğru sürümlerini bulmak ve ardından yazılımı indirmek, derlemek ve yüklemek zaman alabilir.

Linux DSVM bu yükü önemli ölçüde kolaylaştırabilir. Analiz projenize gitmek için Linux DSVM 'yi kullanın. Linux DSVM, R, Python, SQL, Java ve C++gibi çeşitli dillerdeki görevler üzerinde çalışmanıza yardımcı olur. Çakışan Küreler, kodunuzun geliştirilmesi ve test edilmesi için kullanımı kolay bir IDE sağlar. DSVM 'ye dahil edilen Azure SDK, Microsoft bulut platformu için Linux üzerinde çeşitli hizmetler kullanarak uygulamalarınızı oluşturmanıza yardımcı olur. Ruby, Perl, PHP ve Node. js dahil diğer diller önceden yüklenir.

DSVM görüntüsü için yazılım ücreti yok. Yalnızca DSVM görüntüsüyle sağladığınız sanal makinenin boyutuna bağlı olarak değerlendirilen Azure donanım kullanım ücretleri ödersiniz. İşlem ücretleri hakkında daha fazla bilgi için bkz. Azure Marketi 'nde [Linux için veri bilimi sanal makinesi (CentOS) listesi](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/) .

## <a name="prerequisites"></a>Önkoşullar

Bir Linux Veri Bilimi Sanal Makinesi oluşturabilmeniz için önce Şu önkoşullara sahip olmanız gerekir:

* **Azure aboneliği**: Azure aboneliği almak için bkz. [ücretsiz Azure hesabı oluşturma](https://azure.microsoft.com/free/).
* **Azure depolama hesabı**: Bir Azure depolama hesabı almak için bkz. [depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md). Mevcut bir Azure Depolama hesabını kullanmak istemiyorsanız, DSVM 'yi oluştururken bir depolama hesabı oluşturabilirsiniz.

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Diğer sürümler, veri bilimi sanal makinesi

Veri Bilimi Sanal Makinesi, bu sürümlerde de kullanılabilir:

* [Ubuntu](dsvm-ubuntu-intro.md): Ubuntu görüntüsü, derin öğrenme çerçeveleri dahil olmak üzere CentOS görüntüsüyle aynı araçların çoğuna sahiptir. 
* [Windows](provision-vm.md)

## <a name="create-a-linux-data-science-virtual-machine"></a>Linux Veri Bilimi Sanal Makinesi oluşturma

Linux DSVM 'nin bir örneğini oluşturmak için:

1. [Azure Portal](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm)sanal makine listesine gidin.
1. Sihirbazı açmak için **Oluştur** ' u seçin.

   ![Bir Veri Bilimi Sanal Makinesi yapılandıran sihirbaz](./media/linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
1. Sihirbazın her adımı için aşağıdaki bilgileri girin veya seçin:
   
   **1** **temel bilgiler**:

      * **Ad**: Oluşturmakta olduğunuz veri bilimi sunucusunun adı.
      * **Kullanıcı adı**: İlk hesap oturum açma KIMLIĞI.
      * **Parola**: İlk hesap parolası. (Parola yerine SSH ortak anahtarı kullanabilirsiniz.)
      * **Abonelik**: Birden fazla aboneliğiniz varsa, makinenin oluşturulacağı ve faturalanacağı aboneliği seçin. Abonelik için kaynak oluşturmak için izninizin olması gerekir.
      * **Kaynak grubu**: Yeni bir kaynak grubu oluşturabilir veya var olan bir grubu kullanabilirsiniz.
      * **Konum**: DSVM için kullanılacak bir veri merkezi seçin. Çoğu durumda, verilerinizin çoğunu tutan veya fiziksel konumunuza en yakın olan veri merkezini seçin (en hızlı ağ erişimi için).
   
   **2** **Boyut**: İşlevsel gereksinim ve maliyet kısıtlamalarınızı karşılayan bir sunucu türü seçin. Seçin **görünümü tüm** VM boyutları, daha fazla seçenek görmek için.
   
   
   **3** **ayar**:
      * **Disk türü**: Katı hal sürücüsü (SSD) tercih ediyorsanız, **Premium**' u seçin. Aksi takdirde **Standart**' ı seçin.
      * **Depolama hesabı**: Aboneliğinizde yeni bir Azure depolama hesabı oluşturabilir veya sihirbazın **temel kavramları** adımında seçtiğiniz konumda mevcut bir Azure hesabı kullanabilirsiniz.
      * **Diğer parametreler**: Çoğu durumda, diğer parametreleri yapılandırmak için varsayılan değerleri kullanın. Varsayılan olmayan değerleri görmek için, parametresinin bilgilendirici bağlantısının üzerine gelin.
   
   **4** **Özet**: Girdiğiniz bilgilerin doğru olduğundan emin olun.
   
   **5** **satın alma**: Sağlamayı başlatmak için **satın al**' ı seçin. İşlem koşullarına bir bağlantı sağlanır. DSVM 'nin **Boyut**olarak seçtiğiniz sunucu boyutu için işlem ötesinde ek ücret yoktur.

Sağlama 10-20 dakika sürer. Sağlama durumu Azure portal görüntülenir.

## <a name="how-to-access-the-linux-data-science-virtual-machine"></a>Linux veri bilimi sanal makinesi erişme

DSVM oluşturulduktan sonra, SSH kullanarak oturum açabilirsiniz. Metin kabuğu arabirimi için sihirbazın **temel bilgiler** bölümünde oluşturduğunuz hesap kimlik bilgilerini kullanın. Windows üzerinde [Putty](https://www.putty.org)gıbı bir SSH istemci aracını indirebilirsiniz. Grafik Masaüstü (X pencere sistemi) tercih ediyorsanız, PuTTY 'te X11 iletmeyi kullanabilir veya X2Go istemcisini yükleyebilirsiniz.

> [!NOTE]
> X2Go istemci testinde iletme X11 daha iyi gerçekleştirdi. X2Go istemci masaüstü bir grafik arabirim için kullanmanızı öneririz.

## <a name="install-and-configure-the-x2go-client"></a>X2Go istemcisini yükleyip yapılandırma

Linux DSVM, X2Go Server ile sağlanır ve istemci bağlantılarını kabul etmeye hazır hale gelir. Linux DSVM grafik masaüstüne bağlanmak için, istemciniz üzerinde aşağıdaki yordamı izleyin:

1. İstemci platformunuza yönelik X2Go istemcisini indirme ve yükleme [X2Go](https://wiki.x2go.org/doku.php/doc:installation:x2goclient).
1. X2Go istemcisini çalıştırın. **Yeni oturum**' ı seçin. Birden çok sekme içeren bir yapılandırma penceresi açılır. Aşağıdaki yapılandırma parametrelerini girin:
   * **Oturum sekmesini**:
     * **Ana bilgisayar**: Linux DSVM 'nizin ana bilgisayar adını veya IP adresini girin.
     * **Oturum aç**: Linux DSVM üzerinde Kullanıcı adı girin.
     * **SSH bağlantı noktası**: **22**. varsayılan değeri bırakın.
     * **Oturum türü**: Değeri **Xfce**olarak değiştirin. Şu anda Linux DSVM yalnızca XFCE masaüstünü destekler.
   * **Medya** sekmesi: Ses desteğini devre dışı bırakabilirsiniz ve bunları kullanmanız gerekmiyorsa istemci yazdırma özelliğini kapatabilirsiniz.
   * **Paylaşılan klasörler**: İstemci makinelerinizdeki dizinlerin Linux DSVM 'sine bağlanmasını istiyorsanız, DSVM ile paylaşmak istediğiniz istemci makine dizinlerini ekleyin.

X2Go istemcisi aracılığıyla SSH istemcisini ya da XFCE grafik masaüstünü kullanarak DSVM 'de oturum açtıktan sonra, DSVM 'de yüklü ve yapılandırılmış olan araçları kullanmaya başlamaya hazırsınız demektir. XFCE 'de, araçların birçoğu için uygulama menüsü kısayollarını ve Masaüstü simgelerini görebilirsiniz.

## <a name="tools-installed-on-the-linux-data-science-virtual-machine"></a>Linux veri bilimi sanal makinede yüklü araçları

### <a name="machine-learning-server"></a>Machine Learning Server

R veri analizi ve makine öğrenimi için en popüler diller biridir. Analiz etmeniz için R 'yi kullanmak istiyorsanız, DSVM 'nin Microsoft R Open ve Math Kernel kitaplığı ile Machine Learning Server vardır. Matematik Çekirdek Kitaplığı, analitik algoritmalarda ortak matematik işlemlerini iyileştirir. R Open, CRAN R ile tamamen uyumludur. CRAN içinde Yayınlanan R kitaplıklarının herhangi biri R Open 'a yüklenebilir. 

R modellerini Web Hizmetleri 'nde ölçeklendirmek ve kullanıma almak için Machine Learning Server kullanabilirsiniz. RStudio, olduğu gibi vi veya Emacs gibi varsayılan düzenleyicilerden biriyle R programlarınızın düzenleyebilirsiniz. Emacs Düzenleyicisi DSVM 'ye önceden yüklenir. Emacs ESS (Emacs Hoparlörks Istatistikleri) paketi Emacs düzenleyicisinde R dosyalarıyla çalışmayı basitleştirir.

R konsolunu açmak için, kabukta **r**yazın. Bu komut sizi etkileşimli bir ortama götürür. R programınızı geliştirmek için, genellikle Emacs veya VI gibi bir düzenleyici kullanır ve ardından komut dosyalarını R. RStudio 'da çalıştırmak, R programınızı geliştirmek için tam bir grafik IDE sunar.

[İlk 20 R paketini](https://www.kdnuggets.com/2015/06/top-20-r-packages.html) yüklemek için kullanabileceğiniz bir R BETIĞI dsvm 'ye dahildir. R etkileşimli arabiriminizdeki bu betiği çalıştırabilirsiniz. Daha önce belirtildiği gibi, bu arabirimi açmak için kabukta **R**yazın.  

### <a name="python"></a>Python

Anaconda Python, Python 3,5 ve 2,7 ortamları ile birlikte yüklenir. 2,7 ortamına _kök_ adı verilir ve 3,5 ortamı _py35_olarak adlandırılır. Bu dağıtım, temel bir Python yaklaşık 300 en popüler matematik, mühendislik ve veri analizi paketlerinin yanı sıra içerir.

Varsayılan py35 ortamıdır. Kök (2,7) ortamını etkinleştirmek için şu komutu kullanın:

```bash
source activate root
```

Py35 ortamını yeniden etkinleştirmek için şu komutu kullanın:

```bash
source activate py35
```

Python etkileşimli oturumunu çağırmak için, kabukta **Python**girin. 

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

### <a name="jupyter-notebook"></a>Jupyter Notebook

Anaconda dağıtımı, kod ve analiz paylaşmak için bir ortam Jupyter Notebook de birlikte gelir. Jupyıterhub üzerinden Jupyter Notebook erişin. Yerel Linux Kullanıcı adınızı ve parolanızı kullanarak oturum açın.

Jupyter Notebook sunucusu Python 2, Python 3 ve R kernels ile önceden yapılandırılmıştır. Tarayıcıyı açmak ve Jupyter Notebook sunucusuna erişmek için **Jupyter Notebook** masaüstü simgesini kullanın. Dsvm 'ye SSH veya X2Go istemcisi aracılığıyla eriştiğinizde, https:\//localhost: 8000/konumundaki Jupyter Notebook sunucusuna da erişebilirsiniz.

> [!NOTE]
> Hiçbir sertifika uyarısı alırsanız devam edin.

Jupyter notebook sunucusu herhangi bir ana bilgisayardan erişebilirsiniz. **Https:\//\>dsvm DNS adı veya IP adresi: 8000/yazın.\<**

> [!NOTE]
> DSVM sağlandığında, bağlantı noktası 8000 güvenlik duvarında varsayılan olarak açılır. 

Microsoft, bir tane Python ve diğeri R 'de örnek Not defterleri paketlenir. Yerel Linux Kullanıcı adınızı ve parolanızı kullanarak Jupyter Notebook kimlik doğrulamasından geçtikten sonra Jupyter Notebook giriş sayfasındaki örneklerin bağlantısını görebilirsiniz. Yeni bir not defteri oluşturmak için **Yeni**' yi seçin ve ardından kullanmak istediğiniz dil çekirdeğini seçin. **Yeni** düğmesini görmüyorsanız, sol üstteki **Jupyter** simgesini seçerek not defteri sunucusunun ana sayfasına gidin.

### <a name="spark-standalone"></a>Spark tek başına 

Spark bağımsız modunun bir örneği, bunları test etmeden ve bunları büyük kümelerle dağıtmadan önce yerel olarak Spark uygulamaları geliştirmenize yardımcı olmak için Linux DSVM 'ye önceden yüklenir. 

PySpark programlar Jupyter çekirdek çalıştırabilirsiniz. Jupyter açtığınızda, **Yeni** düğmesini seçin ve kullanılabilir çekirdekler listesini görmeniz gerekir. **Spark-Python** , Python dilini kullanarak Spark uygulamaları oluşturmanıza olanak sağlayan pyspark çekirdeğidir. Spark programınızı oluşturmak için Pydüğme veya Spyder gibi bir Python IDE de kullanabilirsiniz. 

Bu tek başına örneğinde, Spark Stack çağıran istemci programında çalışır. Bu özellik, Spark kümesinde geliştirme ile karşılaştırıldığında sorun gidermeyi daha hızlı ve kolay hale getirir.

Jupyter bir örnek PySpark Not defteri sağlar. Bu dosyayı jupi 'nın giriş dizini ($HOME/Notebooks/mini can ml/pyspark) altında can ml dizininde bulabilirsiniz. 

Spark için R 'de Programlama yapıyorsanız Machine Learning Server, parlak r veya parlak LYR ' yi kullanabilirsiniz. 

Machine Learning Server bir Spark bağlamında çalıştırmadan önce, yerel bir tek düğümlü Hadoop ve YARN örneğini etkinleştirmek için bir kerelik Kurulum adımı yapmanız gerekir. Varsayılan olarak, Hadoop Hizmetleri yüklendi ancak DSVM'nin devre dışı. Hadoop hizmetlerini etkinleştirmek için, ilk kez kök olarak aşağıdaki komutları çalıştırın:

```bash
echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
chmod 0600 ~hadoop/.ssh/authorized_keys
chown hadoop:hadoop ~hadoop/.ssh/id_rsa
chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
systemctl start hadoop-namenode hadoop-datanode hadoop-yarn
```

' İ çalıştırarak `systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn`Hadoop ile ilgili hizmetleri durdurabilirsiniz.

/Dsvm/samples/MRS dizini, uzak Spark bağlamında Machine Learning Server geliştirme ve test etme (DSVM 'deki tek başına Spark örneği) hakkında bir örnek sağlar.

### <a name="ides-and-editors"></a>IDE'ler ve düzenleyicilerden

VI/VıM, Emacs, gedit, Pylt, RStudio, tutulma, LaTeX ve IntelliJ gibi çeşitli kod düzenleyicilerinden seçim yapabilirsiniz. 

* gedit, tutulma, IntelliJ, R Studio ve Pyı düğmesi grafik düzenleyicilerdir. Bunları kullanmak için bir grafik masaüstünde oturum açmış olmanız gerekir. Bunları masaüstü ve uygulama menüsü kısayollarını kullanarak açarsınız.

* VIM ve Emacs metin tabanlı düzenleyicilerlerdir. Emacs 'de, ESS eklentisi paketi Emacs düzenleyicisinde R ile çalışmayı kolaylaştırır. [ESS Web sitesi](https://ess.r-project.org/)hakkında daha fazla bilgi edinebilirsiniz.

* Çakışan Küreler, birden çok dili destekleyen açık kaynaklı, genişletilebilir bir IDE 'dir. Java geliştiricileri için tutulma IDE, DSVM 'de yüklü olan sürümdür. Ortamı genişletmek için çeşitli popüler diller için eklentiler yükleyebilirsiniz. 

  Azure Toolkit for Eclipse eklentisi DSVM 'de tutulma ile birlikte yüklenir. Java gibi dilleri destekleyen tutulma geliştirme ortamını kullanarak Azure uygulamalarını oluşturmak, geliştirmek, test etmek ve dağıtmak için Azure Toolkit for Eclipse kullanabilirsiniz.

  Java için Azure SDK, DSVM 'deki Azure Toolkit for Eclipse de yüklenir. Java için Azure SDK, Java ortamının içinden farklı Azure hizmetlerine erişmenizi sağlar. 
  
  Daha fazla bilgi için bkz. [Azure Toolkit for Eclipse](/java/azure/eclipse/azure-toolkit-for-eclipse).

* LaTeX, TeXLive paketiyle birlikte [Auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html)adlı bir Emacs eklenti paketi ile yüklenir. Bu paket, Emacs 'de LaTeX belgelerinizi yazmayı basitleştirir. 

### <a name="databases"></a>Veritabanları

Linux DSVM, çeşitli veritabanı ve komut satırı araçlarına erişmenizi sağlar.

#### <a name="postgressql"></a>PostgresSQL

Açık kaynaklı veritabanı PostgresSQL, çalıştıran hizmetler ve ınitdb tamamlandığında DSVM 'de kullanılabilir. Veritabanları ve kullanıcılar oluşturmanız gerekir. Daha fazla bilgi için bkz. [Postgressql belgeleri](https://www.postgresql.org/docs/).  

#### <a name="squirrel-sql"></a>SQUIRREL SQL

SQUIRREL SQL, çeşitli veritabanlarına (SQL Server, PostgresSQL ve MySQL dahil) bağlanabilecek ve SQL sorguları çalıştırabilecek bir grafik SQL istemcidir. Bir masaüstü simgesi kullanarak, bir grafik Masaüstü oturumundan (örneğin, X2Go istemcisi aracılığıyla) SQUIRREL SQL 'i çalıştırabilirsiniz. Ya da kabuğu içinde aşağıdaki komutu kullanarak istemcisini çalıştırabilirsiniz:

```bash
/usr/local/squirrel-sql-3.7/squirrel-sql.sh /usr/local/squirrel-sql-3.7/squirrel-sql.sh
```

İlk kullanılmadan önce sürücüleri ve veritabanı diğer adlar ayarlayın. JDBC sürücüleri/usr/share/Java/jdbcdrivers. adresinde bulunur

Daha fazla bilgi için bkz. [SQUIRREL SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-sql-server"></a>SQL Server erişmek için komut satırı araçları

SQL Server için ODBC sürücü paketi ayrıca iki komut satırı araçları ile birlikte gelir:

* **bcp**: BCP Aracı, bir SQL Server örneği ve Kullanıcı tarafından belirtilen biçimdeki bir veri dosyası arasında verileri toplu olarak kopyalar. BCP aracını kullanarak çok sayıda yeni satırı SQL Server tablolarına aktarabilir veya tablolardaki verileri veri dosyalarına aktarabilirsiniz. Verileri bir tabloya aktarmak için, bu tablo için oluşturulmuş bir biçim dosyası kullanmanız gerekir. Ya da tablonun yapısını ve sütunlarının geçerli olan veri türlerini anlamanız gerekir.

  Daha fazla bilgi için bkz. [bcp Ile bağlanma](https://msdn.microsoft.com/library/hh568446.aspx).

* **sqlcmd**: Komut istemine Transact-SQL deyimlerini, sistem yordamlarını ve betik dosyalarını girmek için sqlcmd yardımcı programını kullanabilirsiniz. Sqlcmd yardımcı programı Transact-SQL toplu işlerini yürütmek için ODBC kullanır.

  Daha fazla bilgi için bkz. [sqlcmd Ile bağlanma](https://msdn.microsoft.com/library/hh568447.aspx).

  > [!NOTE]
  > Bu araçta Linux ve Windows platformları arasında bazı farklılıklar vardır. Ayrıntılar için belgelere bakın.

#### <a name="database-access-libraries"></a>Veritabanı erişimi kitaplıkları

Veritabanı erişimi kitaplıkları R ve Python 'da kullanılabilir:

* R 'de, RODBC paketini veya dplyr paketini kullanarak veritabanı sunucusundaki SQL deyimlerini sorgulayabilir veya çalıştırabilirsiniz.
* Python 'da pyodbc kitaplığı, temel alınan katman olarak ODBC ile veritabanı erişimi sağlar.

### <a name="azure-tools"></a>Azure Araçları

Aşağıdaki Azure Araçları DSVM 'ye yüklenir:

* **Azure CLI**: Azure 'da kabuk komutları aracılığıyla Azure kaynakları oluşturmak ve yönetmek için komut satırı arabirimini kullanabilirsiniz. Azure araçlarını açmak için **Azure yardımı**'nı girin. Daha fazla bilgi için [Azure CLI belgeleri sayfasını](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* **Azure Depolama Gezgini**: Azure Depolama Gezgini, Azure depolama hesabınızda depoladığınız nesneler üzerinde gezinmek ve Azure bloblarına ve verileri yüklemek ve buradan veri indirmek için kullanabileceğiniz bir grafik aracıdır. Depolama Gezgini masaüstü kısayolu simgesinden erişebilirsiniz. Ayrıca, bunu bir kabuk isteminden **Storageexplorer**girerek açabilirsiniz. Bir X2Go istemcisinden oturum açmanız veya X11 iletme ayarlamış olmanız gerekir.
* **Azure kitaplıkları**: Aşağıdaki kitaplıklar DSVM 'ye önceden yüklenir:
  
  * **Python**: Python 'daki Azure ile ilgili kitaplıklar *Azure*, *azureml*, *pydocumentdb*ve *pyodbc*' dir. İlk üç kitaplıkları ile Azure depolama hizmetleri, Azure Machine Learning ve Azure Cosmos DB (Azure üzerinde bir NoSQL veritabanı) erişebilir. Dördüncü kitaplığı pyodbc (yanı sıra Microsoft ODBC sürücüsü için SQL Server) erişimi etkinleştirir SQL Server, Azure SQL veritabanı ve Azure SQL veri ambarı python'dan ODBC arabirimini kullanarak. Girin **pip listesi** listelenen tüm kitaplıkları görmek için. Hem Python 2.7 hem de 3,5 ortamlarında bu komutu çalıştırmak emin olun.
  * **R**: R 'deki Azure ile ilgili kitaplıklar AzureML ve RODBC.
  * **Java**: Azure Java kitaplıklarının listesi DSVM 'de/dsvm/sdk/AzureSDKJava dizininde bulunabilir. Anahtar kitaplıkları, SQL Server için Azure depolama ve Yönetimi API'leri, Azure Cosmos DB ve JDBC sürücüleri vardır.  

[Azure Portal](https://portal.azure.com) önceden yüklenmiş Firefox tarayıcısından erişebilirsiniz. Azure portal Azure kaynaklarını oluşturabilir, yönetebilir ve izleyebilirsiniz.

### <a name="azure-machine-learning"></a>Azure Machine Learning

Azure Machine Learning, tahmine dayalı analiz çözümleri oluşturmak, dağıtmak ve paylaşmak için kullanabileceğiniz tam olarak yönetilen bir bulut hizmetidir. Azure Machine Learning Studio'dan denemeleri ve modeller oluşturun. DSVM 'deki bir Web tarayıcısından Azure Machine Learning erişmek için [Microsoft Azure Machine Learning](https://studio.azureml.net)' a gidin.

Azure Machine Learning Studio oturum açtıktan sonra, makine öğrenimi algoritmaları için bir mantıksal akış oluşturmak üzere bir deneme tuvali kullanabilirsiniz. Ayrıca, Azure Machine Learning barındırılan bir Jupyter Notebook erişiminiz vardır. Not defteri, Machine Learning Studio 'daki denemeleri ile sorunsuz bir şekilde çalışabilir. 

Oluşturduğunuz makine öğrenimi modellerini bir Web hizmeti arabirimine sarmalayarak oluşturun. Makine öğrenimi modelleri, herhangi bir dilde yazılmış istemcilerin bu modellerdeki tahminleri çağırmasına olanak sağlar. Daha fazla bilgi için [Machine Learning belgeleri](https://azure.microsoft.com/documentation/services/machine-learning/).

Ayrıca, modellerinizi DSVM 'de R veya Python 'da oluşturabilir ve ardından bunları Azure Machine Learning üretime dağıtabilirsiniz. Microsoft, bu işlevselliği desteklemek için R (**azureml**) ve Python (**azureml**) kitaplıklarını yükledi.

Azure Machine Learning için R ve Python modelleri dağıtma hakkında daha fazla bilgi için, bkz. [veri bilimi sanal makinesi üzerinde yapabileceğiniz on şey](vm-do-ten-things.md).

> [!NOTE]
> [Veri bilimi sanal makinesi üzerinde yapabileceğiniz on şey](vm-do-ten-things.md) , Dsvm 'nin Windows sürümü için yazılmıştır. Ancak Azure Machine Learning modelleri dağıtma hakkında bilgiler Linux DSVM için de geçerlidir.

### <a name="machine-learning-tools"></a>Machine learning araçları

DSVM, önceden derlenmiş ve yerel olarak önceden yüklenmiş birkaç makine öğrenimi aracı ve algoritmalarıyla birlikte gelir. Bunlar:

* **Microsoft Cognitive Toolkit**: Derin öğrenme araç seti.
* **Vowpal Wabbit**: Hızlı bir çevrimiçi öğrenme algoritması.
* **Xgboost**: İyileştirilmiş, artırılmış ağaç algoritmaları sağlayan bir araç.
* **Python**: Anaconda Python, Scikit-öğren gibi kitaplıklar ile makine öğrenimi algoritmalarıyla birlikte sunulur. Diğer kitaplıkları kullanarak yükleyebileceğiniz `pip install` komutu.
* **R**: R için zengin bir makine öğrenimi işlevleri kitaplığı mevcuttur. önceden yüklenmiş kitaplıklar lm, GLM, Rasgeleforest ve rpart ' ı içerir. ' İ çalıştırarak `install.packages(<lib name>)`diğer kitaplıkları yükleyebilirsiniz.

Microsoft Cognitive Toolkit, Vowpal Wabbit ve XGBoost, sonraki bölümlerde daha ayrıntılı bir şekilde ele alınmıştır.

#### <a name="microsoft-cognitive-toolkit"></a>Microsoft Bilişsel Araç Seti

Microsoft Cognitive Toolkit, açık kaynaklı bir ayrıntılı öğrenme araç seti. Bu bir komut satırı aracıdır (CNTK) ve zaten yolda.

Temel bir örnek çalıştırmak için, kabukta aşağıdaki komutları çalıştırın:

```bash
cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
cntk configFile=lr_bs.cntk makeMode=false command=Train
```

Daha fazla bilgi için bkz. [GITHUB cntk deposu](https://github.com/Microsoft/CNTK) ve [cntk wiki](https://github.com/Microsoft/CNTK/wiki).

#### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit, çevrimiçi, karma, allazaltma, indirimleri, learning2search, etkin ve etkileşimli öğrenimi gibi teknikler kullanan bir makine öğrenimi sistemidir.

Aracı temel bir örnekte çalıştırmak için aşağıdaki komutları çalıştırın:

```bash
cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
cd vwdemo
vw house_dataset
```

Vowpal Wabbit tanıtım dizini diğer, daha büyük gösterileri içerir. Vowpal Wabbit hakkında daha fazla bilgi için [GitHub Vowpal Wabbit deposuna](https://github.com/JohnLangford/vowpal_wabbit) ve [Vowpal Wabbit wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki)bölümüne bakın.

#### <a name="xgboost"></a>XGBoost

XGBoost kitaplığı, artırılmış (ağaç) algoritmaları için tasarlanmış ve iyileştirilmiştir. XGBoost kitaplığı amacı, ölçeklenebilir, taşınabilir ve doğru olan büyük ölçekli ağaç arttırma sağlamak için makinelerin hesaplama sınırlarını gereken aşırı uç 'e göndermektir.

XGBoost, bir komut satırı ve R kitaplığı olarak sağlanır.

R 'de XGBoost kitaplığını kullanmak için, etkileşimli bir R oturumu başlatın (kabukta **r**yazın) ve ardından kitaplığı yükleyin.

İşte, R isteminde çalıştırabileceğiniz basit bir örnek:

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

Bir. model dosyası belirtilen dizine yazılır. GitHub 'da bu tanıtım örneği hakkında daha fazla bilgi için bkz. [Ikili sınıflandırma](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

XGBoost hakkında daha fazla bilgi için, bkz. [xgboost belgeleri](https://xgboost.readthedocs.org/en/latest/) ve [Xgboost GitHub deposu](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Çıngırağı

Rattle (*R* *A*Nalitik *T*OOL *T*o *L*kazanın) GUI tabanlı veri araştırması ve modelleme kullanır. Rattle:
- Verilerin istatistiksel ve görsel özetlerini sunar.
- Kolay modellenebilir verileri dönüştürür.
- Verilerden hem denetimli hem de denetimli modeller oluşturur.
- Modellerin performansını grafik olarak gösterir.
- Yeni veri kümelerine puan koyar.
- R kodu oluşturur.
- Kullanıcı arabirimindeki, doğrudan R 'de çalıştırılabilen veya daha fazla analiz için bir başlangıç noktası olarak kullanılan işlemleri çoğaltır.

Rattle çalıştırmak için bir grafik Masaüstü oturumunda oturum açmış olmanız gerekir. Bir terminalde r ortamını açmak için r yazın. R isteminde aşağıdaki komutları girin:

```R
library(rattle)
rattle()
```

Bir sekme kümesi açılan grafik arabirimi açılır. Örnek bir hava durumu verisi kümesi kullanmak ve bir model oluşturmak için Rattle içindeki aşağıdaki hızlı başlangıç adımlarını kullanın. Bazı adımlarda, zaten sistemde olmayan bazı gerekli R paketlerini otomatik olarak yükleyip yüklemeniz istenir.

> [!NOTE]
> Paketi sistem dizinine (varsayılan) yüklemeye yönelik izinleriniz yoksa, kişisel kitaplığınıza paket yüklemek için R konsol pencerenizde bir istem görebilirsiniz. Bu istemler görürseniz **y**girin.

1. **Yürüt**’ü seçin.
1. Bir iletişim kutusu, örnek Hava durumu veri kümesini yüklemeniz için istemde bulunur. Örneği yüklemek için **Evet** ' i seçin.
1. **Model** sekmesini seçin.
1. Bir karar ağacı oluşturmak için **Yürüt** ' ü seçin.
1. Karar ağacını göstermek için **Çiz** ' i seçin.
1. **Orman** seçeneğini belirleyin ve sonra rastgele bir orman oluşturmak için **Yürüt** ' ü seçin.
1. **Değerlendir** sekmesini seçin.
1. **Risk** seçeneğini belirleyin ve ardından iki **risk (birikmeli)** performans grafiğini göstermek için **Yürüt** ' ü seçin.
1. Önceki işlemler için oluşturulan R kodunu göstermek üzere **günlük** sekmesini seçin. (Rattle 'in geçerli sürümündeki bir hata nedeniyle bu günlüğü, günlüğün metninde **dışarı aktar** önüne **#** bir karakter eklemeniz gerekir.)
1. Weather_script adlı R betiği dosyasını kaydetmek için **dışarı aktar** düğmesini seçin *.* Ana klasöre R.

Rattle ve R 'den çıkabilirsiniz. Artık oluşturulan R betiğini değiştirebilirsiniz. Ya da, komut dosyasını olduğu gibi kullanın ve Rattle Kullanıcı arabiriminde yapılan her şeyi yinelemek için dilediğiniz zaman çalıştırın. Özellikle R 'deki yeni başlayanlar için bu, basit bir grafik arabiriminde analiz ve makine öğrenimini hızlı bir şekilde yapmanın bir yoludur. bu sayede, R 'de değişiklik yapmak veya öğrenmede kod otomatik olarak oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

İşte öğrenme ve araştırma nasıl devam edebilirsiniz:

* [Linux için veri bilimi sanal makinesi izlenecek yol bilgisi bilimi](linux-dsvm-walkthrough.md) , burada sağlanan Linux dsvm ile birkaç ortak veri bilimi görevinin nasıl yapılacağını gösterir. 
* Bu makalede açıklanan araçları deneyerek DSVM 'deki çeşitli veri bilimi araçlarını inceleyin. Ayrıca, temel bir `dsvm-more-info` giriş için sanal makinedeki kabukta de çalıştırabilirsiniz ve dsvm 'de yüklü araçlar hakkında daha fazla bilgi için işaretçiler bulabilirsiniz.  
* Sistematik olarak kullanarak uçtan uca analitik çözümler oluşturmayı öğrenin [Team Data Science Process](https://aka.ms/tdsp).
* Ziyaret [Azure AI Gallery](https://gallery.azure.ai/) Azure yapay ZEKA hizmetlerini kullanma makine öğrenimi ve veri analizi örnekleri için.