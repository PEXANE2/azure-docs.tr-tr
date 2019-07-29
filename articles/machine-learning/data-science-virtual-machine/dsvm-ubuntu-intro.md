---
title: Bir Ubuntu Linux veri bilimi sanal makinesi oluşturma
titleSuffix: Azure
description: Yapılandırın ve analiz ve makine öğrenimi için Azure'da bir veri bilimi sanal makinesi için Linux (Ubuntu) oluşturun.
services: machine-learning
documentationcenter: ''
author: vijetajo
ms.author: vijetaj
manager: cgronlun
ms.custom: seodec18
ms.assetid: 3bab0ab9-3ea5-41a6-a62a-8c44fdbae43b
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/16/2018
ms.openlocfilehash: 8a19f414f31c307111edad876ed973ff4027d907
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68591922"
---
# <a name="provision-the-data-science-virtual-machine-for-linux-ubuntu"></a>Linux (Ubuntu) için veri bilimi sanal makinesi sağlama

Linux için veri bilimi sanal makinesi, Azure üzerinde derin öğrenme dahil olmak üzere machine learning ile kullanmaya başlamak kolay bir Ubuntu tabanlı sanal makine görüntüsüdür. Derin öğrenme araçları içerir:

* [Caffe](https://caffe.berkeleyvision.org/): Hız, ifade ve modülerlik için tasarlanan derin bir öğrenme çerçevesi
* [Caffe2](https://github.com/caffe2/caffe2): Caffe 'nin platformlar arası bir sürümü
* [Microsoft Cognitive Toolkit](https://github.com/Microsoft/CNTK): Microsoft Research 'tan derin bir öğrenme yazılım araç seti
* [H2O](https://www.h2o.ai/): Açık kaynaklı büyük veri platformu ve grafik kullanıcı arabirimi
* [Keras](https://keras.io/): Python 'da TensorFlow, Microsoft Cognitive Toolkit ve şano için üst düzey bir sinir ağ API 'SI
* [Mxnet](https://mxnet.io/): Birçok dil bağlaması olan esnek, verimli bir derinlemesine öğrenme kitaplığı
* [NVIDIA RAKAMLARI](https://developer.nvidia.com/digits): Yaygın derin öğrenme görevlerini kolaylaştıran bir grafik sistem
* [Pytorch](https://pytorch.org/): Dinamik ağ desteği olan üst düzey bir Python kitaplığı
* [TensorFlow](https://www.tensorflow.org/): Google 'dan makine zekası için açık kaynak kitaplığı
* [Theano](http://deeplearning.net/software/theano/): Çok boyutlu diziler içeren matematik ifadelerini tanımlamak, iyileştirmek ve etkili bir şekilde değerlendirmek için bir Python kitaplığı
* [Torch](http://torch.ch/): Makine öğrenimi algoritmaları için geniş destek içeren bilimsel bir bilgi işlem çerçevesi
* CUDA, cuDNN ve NVIDIA sürücüsü
* Birçok örnek Jupyter Not Defterleri

Tüm kitaplıkları GPU sürümleri olsa da bunlar ayrıca CPU üzerinde çalıştırın.

Linux için veri bilimi sanal makinesi de dahil olmak üzere veri bilimi ve geliştirme etkinlikleri için popüler araçların içerir:

* Microsoft R Open ile Microsoft R Server Geliştirici sürümü
* Anaconda Python dağıtım (sürüm 2.7 ve 3.5) gibi popüler veri analiz kitaplıkları
* JuliaPro - Julia diline popüler bilimsel ve verileri analiz kitaplıkları seçkin bir dağıtımı
* Tek başına Spark örneğinde ve tek düğümlü Hadoop (HDFS, Yarn)
* JupyterHub - R, Python, PySpark, Julia çekirdekler destekleyen çok kullanıcılı bir Jupyter notebook sunucusu
* Azure Depolama Gezgini
* Azure komut satırı Azure kaynaklarını yönetmek için arabirimi (CLI)
* Machine learning araçları
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): Çevrimiçi, karma, allazaltma, indirimleri, learning2search, etkin ve etkileşimli öğrenme gibi teknikleri destekleyen hızlı bir makine öğrenimi sistemi
  * [Xgboost](https://xgboost.readthedocs.org/en/latest/): Hızlı ve doğru şekilde artırılmış ağaç uygulama sağlayan bir araç
  * [Rattle](https://togaware.com/rattle/): R 'de veri analizi ve makine öğrenimini kullanmaya başlamanızı sağlayan grafik bir araç
  * [Lightgbm](https://github.com/Microsoft/LightGBM): Hızlı, dağıtılmış, yüksek performanslı bir gradyan arttırma çerçevesi
* Azure SDK'sı, Java, Python, node.js, Ruby, PHP
* Kitaplıklarında, R ve Python için Azure Machine Learning ve diğer Azure Hizmetleri kullanma
* Geliştirme araçları ve Düzenleyicileri (RStudio, PyCharm, Intellij, Emacs, vim)

Veri bilimi görevleri bir dizi üzerinde yineleme içerir:

1. Bulma, yükleme ve verilerin önceden işlenmesi
1. Oluşturma ve modelleri test etme
1. Akıllı uygulamalarda kullanılmak modelleri dağıtma

Veri bilimcileri, bu görevleri tamamlamak için çeşitli araçlar kullanın. Bu oldukça zaman yazılım uygun sürümleri bulun ve sonra yüklemek için derlemek için alıcı ve bu sürümleri yükleyin.

Linux için veri bilimi sanal makinesi, bu yükü önemli ölçüde kolaylaştırabilir. Analiz projenizi hızlı giriş yapmak için kullanın. R, Python, SQL, Java ve C++ gibi çeşitli dillerde görevler üzerinde çalışmanıza olanak tanır. Azure SDK'ın sanal Makineye dahil çeşitli hizmetlere Linux üzerinde Microsoft bulut platformunu kullanarak uygulamalarınızı oluşturmanıza olanak tanır. Ayrıca, ayrıca önceden yüklü olan diğer dillere Ruby, Perl, PHP ve node.js gibi erişebilirsiniz.

Bu veri bilimi VM görüntüsü için hiçbir yazılım ücreti yoktur. Yalnızca, sağlamanız sanal makinenin boyutuna göre değerlendirilen Azure donanım kullanım ücretleri ödeme yaparsınız. İşlem ücretleri hakkında daha fazla ayrıntı bulunabilir [VM listesi Azure Marketi sayfasında](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Veri bilimi sanal makinesi diğer sürümleri

A [CentOS](linux-dsvm-intro.md) görüntüsüdür Ayrıca aynı araçları Ubuntu görüntüsünü olarak birçoğu ile kullanılabilir. A [Windows](provision-vm.md) görüntü kullanılabilir de.

## <a name="prerequisites"></a>Önkoşullar

Linux için veri bilimi sanal makinesi oluşturmadan önce bir Azure aboneliğinizin olması gerekir. Abonelik sahibi için bkz: [alma Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/).

## <a name="create-your-data-science-virtual-machine-for-linux"></a>Linux için veri bilimi sanal makinenizi oluşturma

Linux için veri bilimi sanal makinesi örneğini oluşturmak için adımlar şunlardır:

1. Sanal makine üzerinde listeleme gidin [Azure portalında](https://portal.azure.com/#create/microsoft-dsvm.linux-data-science-vm-ubuntulinuxdsvmubuntu). Henüz oturum açmadıysanız Azure hesabınızda oturum açmanız istenebilir. 
1. Tıklayın **Oluştur** (altındaki) Kurma Sihirbazı getirilecek.![ Yapılandırma-data-bilimi-vm](./media/dsvm-ubuntu-intro/configure-data-science-virtual-machine.png)
1. Aşağıdaki bölümler, Microsoft Veri bilimi sanal makinesi oluşturmak için kullanılan girişleri her (önceki şekilde sağ tarafındaki numaralandırılan) sihirbazdaki adımları sağlar. Bu adımların her biri yapılandırmak için gerekli girişleri şunlardır:

   a. **Temel**:

   * **Ad**: Oluşturmakta olduğunuz veri bilimi sunucunuzun adı.
   * **VM disk türü**: Bir katı hal sürücüsü (SSD) tercih ediyorsanız **Premium SSD** seçin. Aksi takdirde seçin **standart HDD**. 
   * **Kullanıcı adı**: İlk hesap oturum açma KIMLIĞI.
   * **Parola**: İlk hesap parolası (parola yerine SSH ortak anahtarını kullanabilirsiniz).
   * **Abonelik**: Birden fazla aboneliğiniz varsa, bir makine oluşturulması ve fatura olduğu seçin. Bu abonelikte kaynak oluşturma ayrıcalıklarına sahip olmanız gerekir.
   * **Kaynak grubu**: Yeni bir tane oluşturabilir veya varolan bir grubu kullanın.
   * **Konum**: En uygun veri merkezini seçin. Genellikle, verilerinizden en iyi sahip veya bu fiziksel konumunuza en hızlı ağ erişimi için en yakın veri merkezi bulunur.

   b. **Boyutu**:

   * Maliyet kısıtlamaları ve işlevsel bir gereksinimi karşılayan sunucusu türlerinden birini seçin. GPU tabanlı VM örnekleri için bir NC veya ND sınıfı VM seçin. [Bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/) sayfasında gpu'larla bölgeler listelenir.

   c. **Ayarları**:

   * Çoğu durumda, yalnızca varsayılan değerleri kullanabilirsiniz. Varsayılan olmayan değerleri dikkate alınması gereken belirli alanlar hakkında Yardım için bilgi bağlantı üzerine gelin.

   d. **Özet**:

   * Girdiğiniz tüm bilgilerin doğru olduğunu doğrulayın. Kullanım koşulları için bir bağlantı sağlanır. VM, seçtiğiniz sunucu boyutu için işlem ötesinde herhangi bir ek ücreti yok **boyutu** adım. Sağlama başlatmak için tıklatın **Oluştur**. 

Sağlama, yaklaşık 5 dakika sürer. Sağlama durumunu Azure portalında görüntülenir.

## <a name="how-to-access-the-data-science-virtual-machine-for-linux"></a>Linux için veri bilimi sanal makinesi erişme

Ubuntu DSVM üç yöntemle erişebilirsiniz:

1. Terminal oturumlar için SSH
1. Grafik oturumlarını X2Go
1. JupyterHub ve Jupyter not defterleri için JupyterLab

Ayrıca, sanal makine üzerinde jupi not defterlerini çalıştırmak ve ücretsiz hizmet katmanının sınırlamalarını atlamak için Azure Notebooks Veri Bilimi VM'si ekleyebilirsiniz. Daha fazla bilgi için bkz. [Not defteri projelerini yönetme ve yapılandırma-işlem katmanı](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier).

### <a name="ssh"></a>SSH

VM oluşturulduktan sonra ona SSH kullanarak oturum açabilirsiniz. Oluşturduğunuz hesabı kimlik bilgilerini kullan **Temelleri** bölümünde metin kabuk arabirimi için adım 3. Windows'da [Putty](https://www.putty.org) gibi bir SSH istemcisi aracı indirebilirsiniz. Bir grafik desktop (X Windows sistemi) tercih ederseniz, Putty üzerinde iletme X11 kullanın veya X2Go istemciyi yükleyin.

> [!NOTE]
> X2Go istemci testinde iletme X11 daha iyi gerçekleştirdi. X2Go istemci masaüstü bir grafik arabirim için kullanmanızı öneririz.

### <a name="x2go"></a>X2Go

Linux sanal makinesi zaten X2Go sunucusu ile sağlanan ve istemci bağlantılarını kabul etmeye hazır. Linux VM grafik masaüstüne bağlanmak için istemcinizi aşağıdaki yordamı tamamlayın:

1. İstemci platformunuza yönelik X2Go istemcisini indirme ve yükleme [X2Go](https://wiki.x2go.org/doku.php/doc:installation:x2goclient).
1. X2Go istemci çalıştırıp seçeneğini **yeni oturumu**. Bu, birden fazla sekme ile bir yapılandırma penceresi açılır. Aşağıdaki yapılandırma parametrelerini girin:
   * **Oturum sekmesini**:
     * **Ana bilgisayar**: Linux Veri Bilimi VM'si ana bilgisayar adı veya IP adresi.
     * **Oturum aç**: Linux VM 'de Kullanıcı adı.
     * **SSH bağlantı noktası**: Varsayılan değeri 22 ' de bırakın.
     * **Oturum türü**: Değeri XFCE olarak değiştirin. Şu anda yalnızca Linux VM XFCE Masaüstü destekler.
   * **Medya sekmesi**: Ses desteğini devre dışı bırakabilirsiniz ve bunları kullanmanız gerekmiyorsa istemci yazdırma özelliğini kapatabilirsiniz.
   * **Paylaşılan klasörler**: Linux VM 'ye bağlı istemci makinelerinizden dizinler istiyorsanız, bu sekmedeki VM ile paylaşmak istediğiniz istemci makine dizinlerini ekleyin.

VM'ye SSH istemcisi veya XFCE grafik Masaüstü X2Go istemcisi aracılığıyla kullanarak oturum açtıktan sonra yüklenmiş ve yapılandırılmış VM'de araçları kullanmaya başlamak hazırsınız. XFCE üzerinde çok sayıda araçla menüsü kısayolları uygulamalar ve masaüstü simgelerini görebilirsiniz.

### <a name="jupyterhub-and-jupyterlab"></a>JupyterHub ve JupyterLab

Ubuntu DSVM çalıştıran [JupyterHub](https://github.com/jupyterhub/jupyterhub), çok kullanıcılı Jupyter sunucusu. Bağlanmak için, dizüstü bilgisayar veya masaüstünüzde\/https:/Your-VM-IP: 8000 ' e gidin, VM 'yi oluşturmak için kullandığınız kullanıcı adını ve parolayı girin ve oturum açın. Birçok örnek not defterleri göz atın ve denemek için kullanılabilir.

JupyterLab, Jupyter Not defterlerinden ve JupyterHub, yeni nesil de kullanılabilir. Erişmek için, jupyterhub 'da oturum açın ve https:\//Your-VM-IP: 8000/User/size-username/Lab URL 'sine gidin. Bu satırı */etc/jupyterhub/jupyterhub_config.exe*öğesine ekleyerek, JupyterLab 'ı varsayılan not defteri sunucusu olarak ayarlayabilirsiniz:

```python
c.Spawner.default_url = '/lab'
```

## <a name="tools-installed-on-the-data-science-virtual-machine-for-linux"></a>Linux için veri bilimi sanal makinesi üzerinde yüklü araçları

### <a name="deep-learning-libraries"></a>Derin öğrenme kitaplıkları

#### <a name="cntk"></a>CNTK

Microsoft Bilişsel araç seti, açık kaynaklı, derin öğrenme Araç Seti ' dir. Python bağlamaları, kök ve py35 Conda ortamları kullanılabilir. Ayrıca, yolda zaten olan bir komut satırı aracı (cntk) sahiptir.

Örnek Python not defterleri JupyterHub içinde kullanılabilir. Komut satırında temel bir örnek çalıştırmak için, kabukta aşağıdaki komutları yürütün:

```bash
cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
cntk configFile=lr_bs.cntk makeMode=false command=Train
```

Daha fazla bilgi için bkz: CNTK bölümünü [GitHub](https://github.com/Microsoft/CNTK)ve [CNTK wiki](https://github.com/Microsoft/CNTK/wiki).

#### <a name="caffe"></a>Caffe

Caffe eğitim merkezi Berkeley görme ve derin öğrenme çerçevedir. /Opt/caffe içinde kullanılabilir. Örnekler /opt/caffe/examples içinde bulunabilir.

#### <a name="caffe2"></a>Caffe2

Caffe2 Caffe üzerinde oluşturulmuş facebook'taki derin öğrenme çerçevedir. Python 2.7 Conda kök ortamında kullanılabilir. Etkinleştirmek için, kabuktan aşağıdaki komutu çalıştırın:

```bash
source /anaconda/bin/activate root
```

Bazı örnek not defterleri JupyterHub içinde kullanılabilir.

#### <a name="h2o"></a>H2O

H2O bir hızlı, bellek içi, dağıtılmış bir makine öğrenimi ve Tahmine dayalı analiz platformudur. Bir Python paketi kök ve py35 Anaconda ortamlarda yüklenir. Bir R paketi de yüklenir. H2O komut satırından başlatmak için Çalıştır `java -jar /dsvm/tools/h2o/current/h2o.jar`; vardır çeşitli [komut satırı seçenekleri](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/starting-h2o.html#from-the-command-line) yapılandırmak istediğiniz. Flow Web kullanıcı arabirimini göz atarak erişilebilir http://localhost:54321 kullanmaya başlamak için. Örnek Not defterleri de JupyterHub de mevcuttur.

#### <a name="keras"></a>Keras

Keras, Python 'da TensorFlow, Microsoft Cognitive Toolkit veya teano üzerinde çalışan üst düzey bir sinir ağ API 'sidir. Kök ve py35 Python ortamlarında kullanılabilir.

#### <a name="mxnet"></a>MXNet

MXNet hem verimlilik hem de esneklik için tasarlanmış bir derin öğrenme çerçevedir. DSVM'nin dahil, R ve Python bağlamaları var. Örnek Not Defterleri JupyterHub dahildir ve örnek kod, /dsvm/samples/mxnet içinde kullanılabilir.

#### <a name="nvidia-digits"></a>NVIDIA BASAMAK

NVIDIA derin öğrenme GPU eğitim BASAMAKLI olarak bilinen, veri, tasarlama ve GPU sistemlerinde eğitim sinir ağları yönetme ve gerçek zamanlı olarak Gelişmiş görselleştirme ile performans izleme gibi genel derin öğrenme görevleri basitleştirmek için bir sistem sistemidir.

BASAMAK basamak adlı bir hizmet olarak kullanılabilir. Hizmeti başlatıp göz atın http://localhost:5000 kullanmaya başlamak için.

BASAMAK de yüklüdür Conda kök ortamında bir Python modülü olarak.

#### <a name="tensorflow"></a>TensorFlow

TensorFlow, Google'nın derin öğrenme kitaplığıdır. Veri akışı grafiklerini kullanan sayısal hesaplama için açık kaynaklı bir yazılım kitaplığıdır. TensorFlow py35 Python ortamında kullanılabilir ve bazı örnek not defterleri JupyterHub dahildir.

#### <a name="theano"></a>Theano

Theano verimli sayısal hesaplama için bir Python kitaplıktır. Kök ve py35 Python ortamlarında kullanılabilir. 

#### <a name="torch"></a>Torch

Torch bir makine öğrenimi algoritması için geniş destek ile bilimsel hesaplama çerçevedir. /Dsvm/tools/torch içinde kullanılabilir ve luarocks Paket Yöneticisi ve th Etkileşimli oturumun komut satırında kullanılabilir. Örnekler /dsvm/samples/torch içinde kullanılabilir.

PyTorch ayrıca kök Anaconda ortamında kullanılabilir. İçinde /dsvm/samples/pytorch verilebilir.

### <a name="microsoft-r-server"></a>Microsoft R Server

R veri analizi ve makine öğrenimi için en popüler diller biridir. R analiz için kullanmak istiyorsanız, VM matematik çekirdek kitaplığı (MKL) ve Microsoft R Open (MRO) ile Microsoft R Server (MRS) sahiptir. Matematik işlemlerinden analitik algoritmaları ortak MKL iyileştirir. MRO yüzde 100'ün üzerinde CRAN R ile uyumlu olan ve herhangi bir CRAN'de yayımlanan R kitaplıkları MRO üzerinde yüklenebilir. MRS, ölçeklendirme ve kullanıma hazır hale getirme, R modellerinin web hizmetleri sağlar. RStudio, olduğu gibi vi veya Emacs gibi varsayılan düzenleyicilerden biriyle R programlarınızın düzenleyebilirsiniz. Emacs düzenleyiciyi kullanmak isterseniz, önceden yüklenmiş olmuştur. Emacs paket Sırala (Emacs konuşur istatistikleri) ve Emacs Düzenleyici içindeki R dosyalarıyla çalışma basitleştirir.

Başlatma için R konsolunda, yalnızca yazdığınız **R** Kabuğu'nda. Bu komut sizi etkileşimli bir ortama götürür. R programınızı geliştirmek için genellikle Emacs veya SAN gibi bir düzenleyici kullanın ve ardından içinde r betikleri çalıştırın RStudio ile R programınızı geliştirmek için tam grafik bir IDE ortamını sahip.

Yüklemeniz için bir R betiğini de mevcuttur [üst 20 R paketleri](https://www.kdnuggets.com/2015/06/top-20-r-packages.html) istiyorsanız. (Belirtildiği gibi) yazarak girilebilir R etkileşimli arabiriminde olduktan sonra bu betiği çalıştırın **R** Kabuğu'nda.  

### <a name="python"></a>Python

Anaconda Python 3.5 ortamları ve Python 2.7 ile yüklenir. 2\.7 ortam adında _kök_, ve 3.5 ortam adında _py35_. Bu dağıtım, temel bir Python yaklaşık 300 en popüler matematik, mühendislik ve veri analizi paketlerinin yanı sıra içerir.

Varsayılan py35 ortamıdır. Kök (2.7) ortamı etkinleştirmek için:

```bash
source activate root
```

Py35 ortamı yeniden etkinleştirmek için:

```bash
source activate py35
```

Bir Python etkileşimli oturumu çağırmak için yazmanız yeterlidir **python** Kabuğu'nda. 

Ek Python kitaplıkları kullanarak yükleme ```conda``` veya ```pip``` . Varsayılan istemiyorsanız için pip, doğru ortamı ilk etkinleştirin:

```bash
source activate root
pip install <package>
```

Veya pip için tam yolu belirtin:

```bash
/anaconda/bin/pip install <package>
```

Conda için her zaman ortam adı belirtmeniz gerekir (_py35_ veya _kök_):

```bash
conda install <package> -n py35
```

Bir grafik arabirimde olan veya yedekleme kümesi iletme X11 varsa yazabilirsiniz **pycharm** PyCharm Python IDE başlatmak için. Varsayılan metin düzenleyicisi kullanabilirsiniz. Ayrıca, Spyder, Anaconda Python dağıtımları ile birlikte bir Python IDE kullanabilirsiniz. Grafik bir masaüstü veya X11 Spyder gereken iletme. Grafik Desktop'ta Spyder kısayolu sağlanır.

### <a name="jupyter-notebook"></a>Jupyter notebook

Anaconda dağıtım bir Jupyter not defteri ile kod ve analiz paylaşmak için bir ortam da gelir. Jupyter not defteri JupyterHub erişilir. Yerel Linux kullanıcı adınızı ve parolanızı kullanarak oturum açın.

Jupyter notebook sunucusu Python 2, Python 3 ve R çekirdekler ile önceden yapılandırıldı. Not Defteri sunucuya erişmek için tarayıcıyı başlatmak için "Jupyter Notebook" adlı bir masaüstü simgesi vardır. VM X2Go ya da SSH istemcisi kullanıyorsanız, da ziyaret edebilirsiniz [ https://localhost:8000/ ](https://localhost:8000/) Jupyter notebook sunucusu erişmek için.

> [!NOTE]
> Hiçbir sertifika uyarısı alırsanız devam edin.

Jupyter notebook sunucusu herhangi bir ana bilgisayardan erişebilirsiniz. Yazmanız yeterlidir *https://\<VM DNS adı veya IP adresi\>: 8000 /*

> [!NOTE]
> VM hazırlandığında 8000 numaralı bağlantı noktasını Güvenlik Duvarı'nda varsayılan olarak açılır. 

Size örnek not defterleri--bir da Python ve R'dir birinde paketlediğinizden Jupyter not defterine yerel Linux kullanıcı adı ve parola kullanarak kimlik doğrulama işleminden sonra not defteri giriş sayfasında örnekleri bağlantısını görebilirsiniz. Seçerek yeni bir not defteri oluşturabilirsiniz **yeni**ve ardından uygun dil çekirdek. Görmüyorsanız **yeni** düğmesini tıklatın, **Jupyter** notebook sunucusu giriş sayfasına dönmek için sol üstteki simgesi.

### <a name="apache-spark-standalone"></a>Tek başına Apache Spark

Tek başına bir örneğini Apache Spark, Spark uygulamalarını yerel olarak test etme ve büyük kümelerde dağıtımı önce ilk geliştirmenize yardımcı olması için bu Linux DSVM'sini önceden yüklenir. PySpark programlar Jupyter çekirdek çalıştırabilirsiniz. Jupyter açtığınızda'ı **yeni** düğmesini kullanılabilir çekirdekler listesini görmelisiniz. Spark Python dil kullanan uygulamalar oluşturmanıza olanak tanıyan PySpark çekirdeği "Spark – Python" dir. Spark programınızı oluşturmak için Pydüğme veya Spyder gibi bir Python IDE de kullanabilirsiniz. Bu tek başına örneğinde, Spark Stack çağıran istemci programı içinde çalışarak, Spark kümesi üzerinde geliştirmeyle karşılaştırıldığında sorun gidermeyi daha hızlı ve kolay hale getirir.

Bir örnek PySpark Not Defteri, Jupyter ($ giriş/dizüstü/SparkML/pySpark) giriş dizininin altında "SparkML" dizininde bulabilirsiniz Jupyter üzerinde sağlanır. 

Spark için R 'de programlıyorsanız Microsoft R Server, parlak r veya parlak LYR ' yi kullanabilirsiniz. 

Microsoft R Server Spark bağlamında çalıştırmadan önce kurulum adımı yerel bir tek düğümlü Hadoop HDFS ve Yarn örneği etkinleştirmek için bir kere yapmanız gerekir. Varsayılan olarak, Hadoop Hizmetleri yüklendi ancak DSVM'nin devre dışı. Bunu etkinleştirmek için aşağıdaki komutları kök olarak ilk kez çalıştırma gerekir:

```bash
echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
chmod 0600 ~hadoop/.ssh/authorized_keys
chown hadoop:hadoop ~hadoop/.ssh/id_rsa
chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
systemctl start hadoop-namenode hadoop-datanode hadoop-yarn
```

' İ çalıştırarak Hadoop ile ilgili hizmetleri durdurmanız gerekir```systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```

Uzak Spark bağlamında (DSVM 'deki tek başına Spark örneği), */Dsvm/Samples/Mrs* dizininde sağlanan ve kullanılabilen Mrs 'nin nasıl geliştirileceği ve test alınacağını gösteren bir örnek.

### <a name="ides-and-editors"></a>IDE'ler ve düzenleyicilerden

VI/VıM, Emacs, Pylt, RStudio ve IntelliJ gibi çeşitli kod düzenleyicilerinde bir seçeneğiniz vardır. Intellij ve RStudio PyCharm grafik düzenleyicilerden olan ve bunları kullanmak için bir grafik masaüstü oturum açmanız gerekir. Bu düzenleyicilerin Masaüstü ve uygulama vardır. bunları başlatmak için kısayol menüsü.

**VIM** ve **Emacs** olan metin tabanlı düzenleyiciler. Emacs üzerinde biz Emacs konuşur istatistikleri (Emacs Düzenleyici içindeki R ile çalışmayı kolaylaştırır, EES) adlı bir eklenti paketi yüklediniz. Daha fazla bilgi şu adreste bulunabilir: [Sırala](https://ess.r-project.org/).

**LaTex** Emacs eklenti ile birlikte texlive paketi aracılığıyla yüklenen [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) paketini LaTex belgelerinizi Emacs içinde yazma basitleştirir.  

### <a name="databases"></a>Veritabanları

#### <a name="graphical-sql-client"></a>Grafik SQL istemcisi

**SQuirrel SQL**, (örneğin, Microsoft SQL Server ve MySQL) farklı veritabanlarına bağlanmak için ve SQL sorguları çalıştırmak için bir grafik SQL istemci'nin sağlamış. SQUIRREL SQL 'i bir grafik Masaüstü oturumundan (örneğin, X2Go istemcisini kullanarak) masaüstü simgesini kullanarak veya kabukta aşağıdaki komutu kullanarak çalıştırabilirsiniz:

```bash
/usr/local/squirrel-sql-3.7/squirrel-sql.sh
```

İlk kullanılmadan önce sürücüleri ve veritabanı diğer adlar ayarlayın. JDBC sürücüleri şu adreste bulunabilir:

*/usr/Share/Java/jdbcdrivers*

Daha fazla bilgi için [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Microsoft SQL Server'a erişmek için komut satırı araçları

SQL Server için ODBC sürücü paketi ayrıca iki komut satırı araçları ile birlikte gelir:

**bcp**: BCP yardımcı programı, verileri bir Microsoft SQL Server örneği ve Kullanıcı tarafından belirtilen biçimdeki bir veri dosyası arasında toplu olarak kopyalar. Çok sayıda yeni satırı SQL Server tablolarına aktarmak veya veri tablolar dışında veri dosyalarına veri aktarmak için bcp yardımcı programı kullanılabilir. Bir tabloya veri almak için bu tabloda oluşturulmuş bir biçim dosyası kullanmak, veya tabloyu ve sütunlarını için geçerli olan veri türleri yapısını anlayın.

Daha fazla bilgi için [bcp ile bağlanma](https://msdn.microsoft.com/library/hh568446.aspx).

**sqlcmd**: Sqlcmd yardımcı programıyla Transact-SQL deyimlerini, ayrıca sistem yordamlarını ve komut isteminde komut dosyalarını girebilirsiniz. Bu yardımcı programı, Transact-SQL toplu işlerini yürütmek için ODBC kullanır.

Daha fazla bilgi için [sqlcmd ile bağlanma](https://msdn.microsoft.com/library/hh568447.aspx).

> [!NOTE]
> Bu yardımcı programı, Linux ve Windows platformlarındaki arasında bazı farklılıklar vardır. Ayrıntılar için belgelere bakın.

#### <a name="database-access-libraries"></a>Veritabanı erişimi kitaplıkları

Access veritabanları için R ve Python kullanılabilen kitaplıkları vardır.

* R ile **RODBC** paket veya **dplyr** paket sorgulamak ya da veritabanı sunucusunda SQL deyimlerini yürütmek olanak tanır.
* Python **pyodbc** kitaplığı, temel katmanı olarak ODBC ile veritabanı erişimi sağlar.  

### <a name="azure-tools"></a>Azure Araçları

Aşağıdaki Azure Araçları VM'de yüklü:

* **Azure komut satırı arabirimi**: Azure CLı, kabuk komutları aracılığıyla Azure kaynakları oluşturmanıza ve yönetmenize olanak sağlar. Azure araçlarını çağırmak için yazmanız yeterlidir **azure Yardımı**. Daha fazla bilgi için [Azure CLI belgeleri sayfasını](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* **Microsoft Azure Depolama Gezgini**: Microsoft Azure Depolama Gezgini, Azure depolama hesabınızda depoladığınız nesneler üzerinde gezinmek ve Azure bloblarına ve verileri yüklemek ve buradan veri indirmek için kullanılan bir grafik aracıdır. Depolama Gezgini masaüstü kısayolu simgesinden erişebilirsiniz. Yazarak, bir kabuk isteminde çağırabilirsiniz **StorageExplorer**. Bir X2Go istemcisinden oturum açmanız veya X11 iletme ayarlamış olmanız gerekir.
* **Azure kitaplıkları**: Önceden yüklenmiş kitaplıkların bazıları aşağıda verilmiştir.
  
  * **Python**: Python 'daki Azure ile ilgili kitaplıklar **Azure**, **azureml**, **pydocumentdb**ve **pyodbc**' dir. İlk üç kitaplıkları ile Azure depolama hizmetleri, Azure Machine Learning ve Azure Cosmos DB (Azure üzerinde bir NoSQL veritabanı) erişebilir. Dördüncü kitaplığı pyodbc (yanı sıra Microsoft ODBC sürücüsü için SQL Server) erişimi etkinleştirir SQL Server, Azure SQL veritabanı ve Azure SQL veri ambarı python'dan ODBC arabirimini kullanarak. Girin **pip listesi** listelenen tüm kitaplıkları görmek için. Hem Python 2.7 hem de 3,5 ortamlarında bu komutu çalıştırmak emin olun.
  * **R**: R 'de yüklü olan Azure ile ilgili kitaplıklar **AzureML** ve **rodbc**.
  * **Java**: Azure Java kitaplıklarının listesi VM 'de **/Dsvm/SDK/azuresdkjava** dizininde bulunabilir. Anahtar kitaplıkları, SQL Server için Azure depolama ve Yönetimi API'leri, Azure Cosmos DB ve JDBC sürücüleri vardır.  

Erişebildiğiniz [Azure portalında](https://portal.azure.com) önceden yüklenmiş Firefox tarayıcısı. Azure portalında, oluşturmak, yönetmek ve Azure kaynaklarınızı izleyin.

### <a name="azure-machine-learning"></a>Azure Machine Learning

Azure Machine Learning oluşturmanızı, dağıtmanızı ve Tahmine dayalı analiz çözümlerini sağlayan tam olarak yönetilen bir bulut hizmetidir. Azure Machine Learning Studio'dan denemeleri ve modeller oluşturun. Ziyaret ederek veri bilimi sanal makinesi bir web tarayıcısından erişilebileceğini [Microsoft Azure Machine Learning](https://studio.azureml.net).

Azure Machine Learning Studio'da oturum açtıktan sonra deneme tuvaline Burada, makine öğrenimi algoritmaları için mantıksal bir akış oluşturabilirsiniz erişebilirsiniz. Ayrıca Azure Machine Learning üzerinde barındırılan bir Jupyter not defteri erişimi ve Machine Learning Studio'da denemeleri ile sorunsuz bir şekilde çalışabilir. Machine learning web hizmeti arabiriminde sarmalama tarafından oluşturulmuş modelleri kullanıma hazır hale getirin. Makine öğrenimi modelleri, herhangi bir dilde yazılmış istemcilerin bu modellerdeki tahminleri çağırmasına olanak sağlar. Daha fazla bilgi için [Machine Learning belgeleri](https://azure.microsoft.com/documentation/services/machine-learning/).

Ayrıca VM üzerinde Modellerinizi R veya Python'ı oluşturun ve Azure Machine Learning'i üretim ortamında dağıtın. Biz R kitaplıkları yüklü (**AzureML**) ve Python (**azureml**) bu işlevselliği etkinleştirmek için.

R ve Python modeller Azure Machine Learning içine dağıtma hakkında daha fazla bilgi için bkz. [veri bilimi sanal makinesi üzerinde yapabileceğiniz on işlem](vm-do-ten-things.md) (özellikle bölümü "R veya Python'ı kullanarak modelleri oluşturma ve bunları kullanıma hazır hale getirme kullanma Azure Machine Learning").

> [!NOTE]
> Bu yönergeler, veri bilimi sanal makinesi Windows sürümü için yazılmıştır. Ancak bilgileri var. için Azure Machine Learning modelleri dağıtma hakkında Linux VM'ye uygun sağlanır.

### <a name="machine-learning-tools"></a>Machine learning araçları

VM birkaç makine öğrenme araçları ve önceden derlenmiş ve yerel olarak yüklenmiş algoritmalar ile birlikte gelir. Bunlar:

* **Vowpal Wabbit**: Hızlı bir çevrimiçi öğrenme algoritması.
* **xgboost**: İyileştirilmiş, artırılmış ağaç algoritmaları sağlayan bir araç.
* **Rattle**: Kolay veri araştırması ve modelleme için R tabanlı bir grafik araç.
* **Python**: Anaconda Python, Scikit-öğren gibi kitaplıklar ile makine öğrenimi algoritmalarıyla birlikte sunulur. Diğer kitaplıkları kullanarak yükleyebileceğiniz `pip install` komutu.
* **Lightgbm**: Karar ağacı algoritmalarını temel alan hızlı, dağıtılmış ve yüksek performanslı bir gradyan arttırma çerçevesi.
* **R**: R için bir zengin makine öğrenimi işlevleri kitaplığı mevcuttur. Önceden yüklenen kitaplıkların bazıları lm, GLM, Rasgeleforest, rpart ' dır. Diğer kitaplıkları çalıştırarak yüklenebilir:
  
        install.packages(<lib name>)

Listedeki ilk üç machine learning araçları hakkında bazı ek bilgiler aşağıda verilmiştir.

#### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit olan bir makine öğrenimi çevrimiçi, karma, allreduce, indirimleri, learning2search, etkin, gibi teknikler kullanan sistemi ve etkileşimli öğrenme.

Aracı temel bir örnekte çalıştırmak için aşağıdaki komutları kullanın:

```bash
cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
cd vwdemo
vw house_dataset
```

Bu dizinde büyük, diğer tanıtımlar vardır. VW hakkında daha fazla bilgi için bkz. [GitHub'un bu bölümünde](https://github.com/JohnLangford/vowpal_wabbit)ve [Vowpal Wabbit wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost

Bu tasarlanmış ve artırmalı (ağaç) algoritmaları için en iyi duruma getirilmiş bir kitaplıktır. Amacı, bu kitaplık, büyük ölçekli ağacı, yükseltme sağlamak için gerekli uç makineler hesaplama sınırlarını göndermek için ölçeklenebilir, taşınabilir ve doğru ' dir.

Bir R kitaplığı yanı sıra bir komut satırı sağlanır.

Bu kitaplıkta bir R kullanmak için R etkileşimli bir oturum başlatabilirsiniz (yazarak yalnızca **R** Kabuğu'nda) ve Kitaplığı yükleyin.

İsteminde R çalıştırarak basit bir örnek aşağıda verilmiştir:

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

Xgboost komut satırını çalıştırmak için kabukta yürütülecek komutlar şunlardır:

```bash
cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
cd xgboostdemo
xgboost mushroom.conf
```

.Model dosya belirtilen dizine yazılır. Bu Tanıtım örnek hakkında daha fazla bilgi bulunabilir [github'da](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Xgboost hakkında daha fazla bilgi için bkz: [xgboost belgeleri sayfasını](https://xgboost.readthedocs.org/en/latest/)ve onun [GitHub deposu](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Çıngırağı

Rattle ( **R** **A**nalitik **T**aracı **T**o **L**kazanın **E** GUI tabanlı bir veri keşfi ve modelleme asily) kullanır. Veriler, kolayca modellenebilir, denetimli hem de Denetimsiz modellerinden veri yapıları, grafik performansını modelleri sunar dönüşümler veriler istatistiksel ve görsel özetlerini sunar ve puanları yeni veri kümeleri. Ayrıca R kodunu doğrudan R çalıştırmak veya daha fazla analiz için başlangıç noktası olarak kullanılan işlemleri kullanıcı arabiriminde çoğaltma oluşturur.

Çıngırağı çalıştırmak için bir grafik Masaüstü Oturum açma oturumunda olması gerekir. Terminalde yazın ```R``` R ortamı girmek için. R isteminde aşağıdaki komutları girin:

```R
library(rattle)
rattle()
```

Bir sekme kümesi ile artık bir grafik arabirim açılır. Aşağıda, örnek bir hava durumu veri kümesi kullanmak ve bir model oluşturmak için Rattle ' daki hızlı başlangıç adımları verilmiştir. Bazı adımları otomatik olarak yüklemesini ve sistemde olmayan bazı gerekli R paketlerini yük sorulur.

> [!NOTE]
> Sistem dizininde (varsayılan) paketini yüklemek için erişiminiz yoksa Kişisel kitaplığınıza paketleri yüklemek için R konsol penceresinde bir ileti görebilirsiniz. Yanıt *y* varsa, bu yönergeleri.

1. **Yürüt**'e tıklayın.
1. Hava durumu örnek veri kümesini kullanmak isteyip soran bir iletişim kutusu açılır. Tıklayın **Evet** örnek yüklenemedi.
1. Tıklayın **modeli** sekmesi.
1. Tıklayın **yürütme** karar ağacı oluşturmak için.
1. Tıklayın **çizmek** karar ağacı görüntülenecek.
1. Tıklayın **orman** radyo düğmesini tıklatıp tıklayın **yürütme** rastgele bir orman oluşturmak için.
1. Tıklayın **değerlendir** sekmesi.
1. Tıklayın **Risk** radyo düğmesini tıklatıp tıklayın **yürütme** iki Risk (toplu) performans çizimleri görüntülenecek.
1. Tıklayın **günlük** önceki işlemleri Oluştur R kodunu göstermek için sekmesinde.
   (Eklemek ihtiyacınız Çıngırağı geçerli sürümünde bir hata nedeniyle bir *#* önüne karakter *... Bu günlüğünü dışarı aktarma*  metin günlüğü.)
1. Tıklayın **dışarı** adlı R betiği kaydetmek için *weather_script. R* için giriş klasörü.

Çıngırağı ve R'dir Çık Şimdi oluşturulan R betiğini değiştirebilir veya çalıştırmak için her zaman içinde Rattle UI yapılmış her şeyi yinelemek için olduğu gibi kullanabilirsiniz. Özellikle yeni başlayanlar için r ile hızlı bir şekilde analizlerini ve otomatik olarak değiştirmek ve/veya öğrenmek için R kod oluşturma sırasında basit bir grafik arabirim learning'de makine için kolay bir yolu budur.

## <a name="next-steps"></a>Sonraki adımlar

İşte öğrenme ve araştırma nasıl devam edebilirsiniz:

* [Üzerinde veri bilimi sanal makinesi için Linux veri bilimi](linux-dsvm-walkthrough.md) izlenecek yol, Linux veri bilimi buraya sağlanan VM ile çeşitli genel veri bilimi görevlerini gerçekleştirmek nasıl gösterir. 
* Çeşitli veri bilimi araçlarını, veri bilimi sanal makinesi, bu makalede açıklanan araçları deneyerek keşfedin. Ayrıca çalıştırabileceğiniz *dsvm daha fazla bilgi* Kabuk temel bir giriş ve işaretçileri VM'de yüklü araçları hakkında daha fazla bilgi için sanal makine içinde.  
* Sistematik olarak kullanarak uçtan uca analitik çözümler oluşturmayı öğrenin [Team Data Science Process](https://aka.ms/tdsp).
* Ziyaret [Azure AI Gallery](https://gallery.azure.ai/) Azure yapay ZEKA hizmetlerini kullanma makine öğrenimi ve veri analizi örnekleri için.
