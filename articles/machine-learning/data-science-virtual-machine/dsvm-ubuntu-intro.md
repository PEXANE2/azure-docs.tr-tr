---
title: Linux Ubuntu hızlı başlangıç
titleSuffix: Azure Data Science Virtual Machine
description: Yapılandırın ve analiz ve makine öğrenimi için Azure'da bir veri bilimi sanal makinesi için Linux (Ubuntu) oluşturun.
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: quickstart
ms.date: 08/13/2019
ms.openlocfilehash: 9b323970e735a3e2a9d2cf418b787ed046c5574d
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70192092"
---
# <a name="quickstart-set-up-the-data-science-virtual-machine-for-linux-ubuntu"></a>Hızlı Başlangıç: Linux için Veri Bilimi Sanal Makinesi ayarlama (Ubuntu)

Linux için Veri Bilimi Sanal Makinesi (DSVM), Azure üzerinde derin öğrenme de dahil olmak üzere Machine Learning 'i kullanmaya başlamanıza olanak sağlayan bir Ubuntu tabanlı sanal makine görüntüsüdür. Derin öğrenme araçları içerir:

* [Caffe](https://caffe.berkeleyvision.org/): Hız, ifade ve modülerlik için tasarlanan derin bir öğrenme çerçevesi.
* [Caffe2](https://github.com/caffe2/caffe2): Caffe 'nin platformlar arası bir sürümü.
* [Microsoft Cognitive Toolkit](https://github.com/Microsoft/CNTK): Microsoft Research 'tan derin bir öğrenme yazılım araç seti.
* [H2O](https://www.h2o.ai/): Açık kaynaklı büyük veri platformu ve grafik kullanıcı arabirimi.
* [Keras](https://keras.io/): Python 'da TensorFlow, Microsoft Cognitive Toolkit ve teano için üst düzey bir sinir ağ API 'SI.
* [Mxnet](https://mxnet.io/): Birçok dil bağlaması olan esnek, verimli bir ayrıntılı öğrenme kitaplığı.
* [NVIDIA RAKAMLARI](https://developer.nvidia.com/digits): Yaygın derin öğrenme görevlerini kolaylaştıran bir grafik sistem.
* [Pytorch](https://pytorch.org/): Dinamik ağ desteği olan üst düzey bir Python kitaplığı.
* [TensorFlow](https://www.tensorflow.org/): Google 'dan Machine Intelligence için açık kaynak kitaplığı.
* [Theano](http://deeplearning.net/software/theano/): Çok boyutlu dizileri içeren matematik ifadelerini tanımlamak, iyileştirmek ve etkili bir şekilde değerlendirmek için bir Python kitaplığı.
* [Torch](http://torch.ch/): Makine öğrenimi algoritmaları için geniş destek içeren bilimsel bir bilgi işlem altyapısı.
* CUDA, cuDNN ve NVıDıA sürücüsü.
* Birçok örnek jupi Not defteri.

Tüm kitaplıkları GPU sürümleri olsa da bunlar ayrıca CPU üzerinde çalıştırın.

Linux için veri bilimi sanal makinesi de dahil olmak üzere veri bilimi ve geliştirme etkinlikleri için popüler araçların içerir:

* Microsoft R açık ile Microsoft Machine Learning Server.
* Popüler veri analizi kitaplıkları da dahil olmak üzere Anaconda Python dağıtımı (sürüm 2,7 ve 3,5).
* JuliaPro, Julia dilinin popüler bilimsel ve veri analizi kitaplıklarıyla dağıtımı.
* Tek başına Spark örneği ve tek düğümlü Hadoop (, Yarn).
* JupyterHub, R, Python, PySpark ve Julia kernels 'yi destekleyen çok kullanıcılı bir Jupyter Not defteri sunucusudur.
* Azure Depolama Gezgini.
* Azure kaynaklarını yönetmek için Azure CLı.
* Machine Learning araçları:
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): Çevrimiçi, karma, allazaltma, indirimleri, learning2search, etkin ve etkileşimli öğrenme gibi teknikleri destekleyen hızlı bir makine öğrenimi sistemi.
  * [Xgboost](https://xgboost.readthedocs.org/en/latest/): Hızlı ve doğru artırmalı ağaç uygulaması sağlayan bir araç.
  * [Rattle](https://togaware.com/rattle/): R 'de veri analizi ve makine öğrenimini kullanmaya başlamanızı sağlayan grafik bir araç.
  * [Lightgbm](https://github.com/Microsoft/LightGBM): Hızlı, dağıtılmış ve yüksek performanslı bir gradyan yükseltme çerçevesi.
* Java, Python, Node. js, Ruby ve PHP 'de Azure SDK.
* Kitaplıklarında, R ve Python için Azure Machine Learning ve diğer Azure hizmetlerini kullanın.
* Geliştirme araçları ve düzenleyiciler (RStudio, Pylt, IntelliJ, Emacs, VI).

Veri bilimi görevleri bir dizi üzerinde yineleme içerir:

1. Bulma, yükleme ve verilerin önceden işlenmesi
1. Oluşturma ve modelleri test etme
1. Akıllı uygulamalarda kullanılmak modelleri dağıtma

Veri bilimcileri, bu görevleri tamamlamak için çeşitli araçlar kullanın. Yazılımın uygun sürümlerini bulmak ve ardından bu sürümleri indirmek, derlemek ve yüklemek için zaman alıcı olabilir.

Linux için veri bilimi sanal makinesi, bu yükü önemli ölçüde kolaylaştırabilir. Analiz projenizi hızlı giriş yapmak için kullanın. R, Python, SQL, Java ve C++ gibi çeşitli dillerde görevler üzerinde çalışmanıza olanak tanır. Azure SDK'ın sanal Makineye dahil çeşitli hizmetlere Linux üzerinde Microsoft bulut platformunu kullanarak uygulamalarınızı oluşturmanıza olanak tanır. Ayrıca, önceden yüklenmiş olan Ruby, Perl, PHP ve Node. js gibi diğer dillere erişebilirsiniz.

Bu DSVM görüntüsü için yazılım ücreti yok. Yalnızca, sağlamanız sanal makinenin boyutuna göre değerlendirilen Azure donanım kullanım ücretleri ödeme yaparsınız. İşlem ücretleri hakkında daha fazla bilgi için bkz. [Azure Marketi 'Nde VM listeleme sayfası](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).

## <a name="other-versions-of-the-data-science-virtual-machine"></a>Diğer sürümler, veri bilimi sanal makinesi

A [CentOS](linux-dsvm-intro.md) görüntüsüdür Ayrıca aynı araçları Ubuntu görüntüsünü olarak birçoğu ile kullanılabilir. Bir [Windows](provision-vm.md) görüntüsü de mevcuttur.

## <a name="prerequisites"></a>Önkoşullar

Linux için veri bilimi sanal makinesi oluşturmadan önce bir Azure aboneliğinizin olması gerekir. [Azure Ücretsiz deneme sürümünü](https://azure.microsoft.com/free/)edinebilirsiniz.

## <a name="create-your-data-science-virtual-machine-for-linux"></a>Linux için veri bilimi sanal makinenizi oluşturma

Linux için veri bilimi sanal makinesi örneğini oluşturmak için adımlar şunlardır:

1. [Azure Portal](https://portal.azure.com/#create/microsoft-dsvm.linux-data-science-vm-ubuntulinuxdsvmubuntu)sanal makine listesine gidin. Henüz oturum açmadıysanız Azure hesabınızda oturum açmanız istenebilir. 
1. Sihirbazı açmak için **Oluştur** ' u seçin.
    ![Sanal makine oluşturma Sihirbazı](./media/dsvm-ubuntu-intro/configure-data-science-virtual-machine.png)
1. Sihirbazın her adımını yapılandırmak için aşağıdaki bilgileri girin:

    1. **Temel**:
    
       * **Abonelik**: Birden fazla aboneliğiniz varsa makinenin oluşturulacağı ve faturalandırılabileceği bir tane seçin. Bu abonelikte kaynak oluşturma ayrıcalıklarına sahip olmanız gerekir.
       * **Kaynak grubu**: Yeni bir tane oluşturabilir veya varolan bir grubu kullanın.
       * **Sanal makine adı**: Oluşturmakta olduğunuz veri bilimi sunucusunun adını girin.
       * **Bölge**: En uygun veri merkezini seçin. Genellikle verilerinizin büyük veya en hızlı ağ erişimi için fiziksel konumunuza en yakın olan veri merkezdir.
       * **Kullanılabilirlik seçenekleri**: Bu VM 'yi kullanılabilirlik kümelerinde veya bölgelerde kullanmak istiyorsanız bunu ayarlayın. Aksi takdirde, varsayılan olarak bırakın.
       * **Görüntü**: Varsayılan değeri değiştirmeyin.
       * **Boyutu**: İşlevsel gereksinimlerinizi ve maliyet kısıtlamalarını karşılayan sunucu türünü seçin. GPU tabanlı VM örnekleri için bir NC veya ND serisi VM seçin. 
       * **Kullanıcı adı**: Yönetici Kullanıcı adını girin.
       * **SSH ortak anahtarı**: RSA ortak anahtarını tek satırlı biçimde girin. (Bir SSH anahtarı yerine bir parola kullanabilirsiniz.)
    
    1. **Diskler**:
    
       * **Işletim sistemi diski türü**: Bir katı hal sürücüsü (SSD) tercih ediyorsanız **Premium SSD** seçin. Aksi takdirde seçin **standart HDD**.
    
    1. Ayarların geri kalanı için varsayılan değerleri kullanabilirsiniz. Varsayılan olmayan değerleri göz önünde bulundurmanız için yardım için bilgilendirici bağlantının üzerine gelin. İşiniz bittiğinde, **gözden geçir + oluştur**' u seçin.
    
    1. VM doğrulamayı geçtikten sonra girdiğiniz tüm bilgilerin doğru olduğundan emin olun. Bir bağlantı sizi kullanım koşullarına yönlendirir. VM 'de, **Boyut** girişinde seçtiğiniz sunucu boyutu için işlem dışında ek ücret yoktur. Sağlamayı başlatmak için **Oluştur**' u seçin.
    
    Sağlama, yaklaşık 5 dakika sürer. Durum Azure portal görüntülenir.

## <a name="how-to-access-the-data-science-virtual-machine-for-linux"></a>Linux için veri bilimi sanal makinesi erişme

Ubuntu DSVM 'ye üç yöntem kullanarak erişebilirsiniz:

- Terminal oturumlar için SSH
- Grafik oturumlarını X2Go
- JupyterHub ve Jupyter not defterleri için JupyterLab

Ayrıca, sanal makine üzerinde jupi not defterlerini çalıştırmak ve ücretsiz hizmet katmanının sınırlamalarını atlamak için Azure Notebooks Veri Bilimi Sanal Makinesi ekleyebilirsiniz. Daha fazla bilgi için bkz. [Azure Notebooks projelerini yönetme ve yapılandırma](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier).

### <a name="ssh"></a>SSH

VM oluşturulduktan sonra ona SSH kullanarak oturum açabilirsiniz. Oluşturduğunuz hesabı kimlik bilgilerini kullan **Temelleri** bölümünde metin kabuk arabirimi için adım 3. Windows üzerinde [Putty](https://www.putty.org)gıbı bir SSH istemci aracını indirebilirsiniz. Grafik Masaüstü (X pencere sistemi) tercih ediyorsanız, PuTTY 'te X11 iletmeyi kullanabilir veya X2Go istemcisini yükleyebilirsiniz.

> [!NOTE]
> X2Go istemci testinde iletme X11 daha iyi gerçekleştirdi. X2Go istemci masaüstü bir grafik arabirim için kullanmanızı öneririz.

### <a name="x2go"></a>X2Go

Linux VM, X2Go Server ile zaten sağlanmış ve istemci bağlantılarını kabul etmeye hazır. Linux VM grafik masaüstüne bağlanmak için istemcinizi aşağıdaki yordamı tamamlayın:

1. İstemci platformunuza yönelik X2Go istemcisini indirme ve yükleme [X2Go](https://wiki.x2go.org/doku.php/doc:installation:x2goclient).
1. X2Go istemci çalıştırıp seçeneğini **yeni oturumu**. Bu, birden fazla sekme ile bir yapılandırma penceresi açılır. Aşağıdaki yapılandırma parametrelerini girin:
   * **Oturum sekmesini**:
     * **Ana bilgisayar**: Linux Veri Bilimi Sanal Makinesi ana bilgisayar adını veya IP adresini girin.
     * **Oturum aç**: Linux VM 'de Kullanıcı adı girin.
     * **SSH bağlantı noktası**: Varsayılan değeri 22 ' de bırakın.
     * **Oturum türü**: Değeri **Xfce**olarak değiştirin. Şu anda, Linux VM yalnızca XFCE masaüstünü destekler.
   * **Medya sekmesi**: Ses desteğini devre dışı bırakabilirsiniz ve bunları kullanmanız gerekmiyorsa istemci yazdırma özelliğini kapatabilirsiniz.
   * **Paylaşılan klasörler**: Linux VM 'ye bağlı istemci makinelerinizden dizinler istiyorsanız, bu sekmedeki VM ile paylaşmak istediğiniz istemci makine dizinlerini ekleyin.

VM 'de X2Go istemcisi aracılığıyla SSH istemcisini veya XFCE grafik masaüstünü kullanarak oturum açtıktan sonra, sanal makinede yüklü ve yapılandırılmış araçları kullanmaya başlamaya hazırsınız demektir. XFCE 'de, araçların birçoğu için uygulama menüsü kısayollarını ve Masaüstü simgelerini görebilirsiniz.

### <a name="jupyterhub-and-jupyterlab"></a>JupyterHub ve JupyterLab

Ubuntu DSVM, çok kullanıcılı bir Jupyıter sunucusu olan [Juponterhub](https://github.com/jupyterhub/jupyterhub)'ı çalıştırır. Bağlanmak için, dizüstü bilgisayarınızda veya masaüstünüzde\/https:/Your-VM-IP: 8000 adresine gidin. VM 'yi oluşturmak için kullandığınız kullanıcı adını ve parolayı girin ve oturum açın. Birçok örnek not defterleri göz atın ve denemek için kullanılabilir.

JupyterLab, Jupyter Not defterlerinden ve JupyterHub, yeni nesil de kullanılabilir. Erişmek için jupyterhub 'da oturum açın ve https:\//Your-VM-IP: 8000/User/ınızı-username/Lab URL 'sine gidin. Aşağıdaki satırı ekleyerek JupyterLab 'ı `/etc/jupyterhub/jupyterhub_config.py`varsayılan not defteri sunucusu olarak ayarlayabilirsiniz:

```python
c.Spawner.default_url = '/lab'
```

## <a name="tools-installed-on-the-data-science-virtual-machine-for-linux"></a>Linux için veri bilimi sanal makinesi üzerinde yüklü araçları

### <a name="deep-learning-libraries"></a>Derin öğrenme kitaplıkları

#### <a name="cntk"></a>CNTK

Microsoft Cognitive Toolkit, açık kaynaklı bir ayrıntılı öğrenme araç setidir. Python bağlamaları, kök ve py35 Conda ortamları kullanılabilir. Ayrıca, yolunda zaten olan bir komut satırı aracı (CNTK) vardır.

Örnek Python not defterleri JupyterHub içinde kullanılabilir. Komut satırında temel bir örnek çalıştırmak için, kabukta aşağıdaki komutları çalıştırın:

```bash
cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
cntk configFile=lr_bs.cntk makeMode=false command=Train
```

Daha fazla bilgi için [GitHub](https://github.com/Microsoft/CNTK) ve [cntk wiki](https://github.com/Microsoft/CNTK/wiki)'nin cntk bölümüne bakın.

#### <a name="caffe"></a>Caffe

Caffe eğitim merkezi Berkeley görme ve derin öğrenme çerçevedir. /Seçenek/Caffe. içinde kullanılabilir. Örnekleri/opt/Caffe/Examples. içinde bulabilirsiniz

#### <a name="caffe2"></a>Caffe2

Caffe2 Caffe üzerinde oluşturulmuş facebook'taki derin öğrenme çerçevedir. Conda root ortamında Python 2,7 ' de mevcuttur. Etkinleştirmek için, kabuktan aşağıdaki komutu çalıştırın:

```bash
source /anaconda/bin/activate root
```

Bazı örnek not defterleri JupyterHub içinde kullanılabilir.

#### <a name="h2o"></a>H2O

H2O bir hızlı, bellek içi, dağıtılmış bir makine öğrenimi ve Tahmine dayalı analiz platformudur. Bir Python paketi kök ve py35 Anaconda ortamlarda yüklenir. Bir R paketi de yüklenir. 

Komut satırından H2O 'yi açmak için komutunu çalıştırın `java -jar /dsvm/tools/h2o/current/h2o.jar`. Yapılandırmak isteyebileceğiniz çeşitli [komut satırı seçenekleri](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/starting-h2o.html#from-the-command-line) vardır. Başlamak için ' e http://localhost:54321 giderek Flow Web Kullanıcı arabirimine erişebilirsiniz. Örnek Not defterleri de JupyterHub de mevcuttur.

#### <a name="keras"></a>Keras

Keras, Python 'da üst düzey bir sinir ağ API 'sidir. Bu, TensorFlow, Microsoft Cognitive Toolkit veya teano üzerinde çalışabilir. Kök ve py35 Python ortamlarında kullanılabilir.

#### <a name="mxnet"></a>MXNet

MXNet hem verimlilik hem de esneklik için tasarlanmış bir derin öğrenme çerçevedir. DSVM'nin dahil, R ve Python bağlamaları var. Örnek Not Defterleri JupyterHub dahildir ve örnek kod, /dsvm/samples/mxnet içinde kullanılabilir.

#### <a name="nvidia-digits"></a>NVIDIA BASAMAK

RAKAM olarak bilinen NVıDıA derin öğrenme GPU eğitim sistemi, yaygın derin öğrenme görevlerini basitleştirecek bir sistemdir. Bu görevler, verileri yönetmeyi, GPU sistemlerinde sinir ağlarını tasarlamayı ve eğitimini ve gelişmiş görselleştirmede performansı gerçek zamanlı olarak izlemeyi içerir.

Basamaklar, *basamaklar*adlı bir hizmet olarak kullanılabilir. Hizmeti başlatıp göz atın http://localhost:5000 kullanmaya başlamak için.

BASAMAK de yüklüdür Conda kök ortamında bir Python modülü olarak.

#### <a name="tensorflow"></a>TensorFlow

TensorFlow, Google'nın derin öğrenme kitaplığıdır. Veri akışı grafiklerini kullanan sayısal hesaplama için açık kaynaklı bir yazılım kitaplığıdır. TensorFlow py35 Python ortamında kullanılabilir ve bazı örnek not defterleri JupyterHub dahildir.

#### <a name="theano"></a>Theano

Theano verimli sayısal hesaplama için bir Python kitaplıktır. Kök ve py35 Python ortamlarında kullanılabilir. 

#### <a name="torch"></a>Torch

Torch bir makine öğrenimi algoritması için geniş destek ile bilimsel hesaplama çerçevedir. Bu,/dsvm/Tools/Torch sürümünde bulunur ve etkileşimli oturum ve LuaRocks Paket Yöneticisi komut satırından kullanılabilir. Örnekler /dsvm/samples/torch içinde kullanılabilir.

PyTorch ayrıca kök Anaconda ortamında kullanılabilir. İçinde /dsvm/samples/pytorch verilebilir.

### <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Sunucusu

R veri analizi ve makine öğrenimi için en popüler diller biridir. Analiz etmeniz için R 'yi kullanmak istiyorsanız, VM 'nin Microsoft R Open ve Math Kernel kitaplığı ile Microsoft Machine Learning Server vardır. Matematik Çekirdek Kitaplığı, analitik algoritmalarda ortak olan matematik işlemlerini iyileştirir. Microsoft R Open, CRAN R ile yüzde 100 uyumludur ve CRAN 'de yayınlanan R kitaplıklarının herhangi biri Microsoft R Open 'a yüklenebilir. 

Machine Learning Server, R modellerinin Web Hizmetleri ile ölçeklendirilmesi ve kullanımını sağlar. RStudio, olduğu gibi vi veya Emacs gibi varsayılan düzenleyicilerden biriyle R programlarınızın düzenleyebilirsiniz. Emacs düzenleyiciyi kullanmak isterseniz, önceden yüklenmiş olmuştur. Emacs ESS (Emacs Hoparlörks Istatistikleri) paketi, Emacs Düzenleyicisi içindeki R dosyalarıyla çalışmayı basitleştirir.

R konsolunu açmak için, kabuğa **r** yazın. Bu komut sizi etkileşimli bir ortama götürür. R programınızı geliştirmek için, genellikle Emacs veya vi gibi bir düzenleyici kullanın ve ardından betikleri R içinde çalıştırın. RStudio ile R programınızı geliştirmek için tam bir grafik IDE 'niz vardır.

Ayrıca, isterseniz [en iyi 20 r paketlerini](https://www.kdnuggets.com/2015/06/top-20-r-packages.html) yükleyebileceğiniz bir R betiği de vardır. Bu betiği, R etkileşimli arabiriminizdeki olduktan sonra çalıştırabilirsiniz. Daha önce belirtildiği gibi, bu arabirimi kabuğa **R** girerek açabilirsiniz.  

### <a name="python"></a>Python

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

### <a name="jupyter-notebook"></a>Jupyter notebook

Anaconda dağıtım bir Jupyter not defteri ile kod ve analiz paylaşmak için bir ortam da gelir. Jupyter not defteri JupyterHub erişilir. Yerel Linux Kullanıcı adınızı ve parolanızı kullanarak oturum açın.

Jupyter notebook sunucusu Python 2, Python 3 ve R çekirdekler ile önceden yapılandırıldı. Tarayıcıyı açmak ve Not defteri sunucusuna erişmek için **Jupyter Notebook** masaüstü simgesini kullanın. Sanal makine ile SSH veya X2Go istemcisi aracılığıyla çalışıyorsanız, Jupyter Not defteri sunucusuna [https://localhost:8000/](https://localhost:8000/)da erişebilirsiniz.

> [!NOTE]
> Hiçbir sertifika uyarısı alırsanız devam edin.

Jupyter notebook sunucusu herhangi bir ana bilgisayardan erişebilirsiniz. **\>Https://\<VM DNS adını veya IP adresini girin: 8000/** .

> [!NOTE]
> VM hazırlandığında 8000 numaralı bağlantı noktasını Güvenlik Duvarı'nda varsayılan olarak açılır. 

Örnek Not defterlerimizi (Python 'da, diğeri R 'de) paketliyoruz. Yerel Linux Kullanıcı adınızı ve parolanızı kullanarak Jupyter not defterine kimlik doğrulaması yaptıktan sonra Not defteri giriş sayfasındaki örneklerin bağlantısını görebilirsiniz. Yeni ' yi seçerek ve ardından uygun dilçekirdeğini seçerek yeni bir not defteri oluşturabilirsiniz. **Yeni** düğmesini görmüyorsanız, sol üstteki **Jupyter** simgesini seçerek not defteri sunucusunun ana sayfasına gidin.

### <a name="apache-spark-standalone"></a>Tek başına Apache Spark

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

### <a name="ides-and-editors"></a>IDE'ler ve düzenleyicilerden

VI/VIM, Emacs, Pylt, RStudio ve IntelliJ dahil olmak üzere çeşitli kod düzenleyicilerinden birini tercih edersiniz. 

Pydüğme, RStudio ve IntelliJ grafik düzenleyicilerdir. Bunları kullanmak için bir grafik masaüstünde oturum açmanız gerekir. Bunları masaüstü ve uygulama menüsü kısayollarını kullanarak açarsınız.

VIM ve Emacs metin tabanlı düzenleyicilerlerdir. Emacs 'de, ESS eklenti paketi Emacs Düzenleyicisi içinde R ile çalışmayı kolaylaştırır. [ESS Web sitesi](https://ess.r-project.org/)hakkında daha fazla bilgi edinebilirsiniz.

LaTex, TeXLive paketiyle birlikte [Auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html)adlı bir Emacs eklenti paketi ile yüklenir. Bu paket, Emacs içindeki LaTex belgelerinizi yazmayı basitleştirir.  

### <a name="databases"></a>Veritabanları

#### <a name="graphical-sql-client"></a>Grafik SQL istemcisi

Grafik SQL istemcisi olan SQuirrel SQL, çeşitli veritabanlarına (Microsoft SQL Server ve MySQL gibi) bağlanabilir ve SQL sorguları çalıştırabilir. Bir masaüstü simgesi kullanarak, bir grafik Masaüstü oturumundan (örneğin, X2Go istemcisi aracılığıyla) SQUIRREL SQL 'i çalıştırabilirsiniz. Ya da kabuğu içinde aşağıdaki komutu kullanarak istemcisini çalıştırabilirsiniz:

```bash
/usr/local/squirrel-sql-3.7/squirrel-sql.sh
```

İlk kullanılmadan önce sürücüleri ve veritabanı diğer adlar ayarlayın. JDBC sürücüleri/usr/share/Java/jdbcdrivers. adresinde bulunur

Daha fazla bilgi için [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Microsoft SQL Server'a erişmek için komut satırı araçları

SQL Server için ODBC sürücü paketi ayrıca iki komut satırı araçları ile birlikte gelir:

- **bcp**: BCP Aracı, bir Microsoft SQL Server örneği ve Kullanıcı tarafından belirtilen biçimdeki bir veri dosyası arasında verileri toplu olarak kopyalar. BCP aracını kullanarak çok sayıda yeni satırı SQL Server tablolarına içeri aktarabilir veya tablolardaki verileri veri dosyalarına aktarabilirsiniz. Verileri bir tabloya aktarmak için, bu tablo için oluşturulmuş bir biçim dosyası kullanmanız gerekir. Ya da tablonun yapısını ve sütunlarının geçerli olan veri türlerini anlamanız gerekir.

  Daha fazla bilgi için [bcp ile bağlanma](https://msdn.microsoft.com/library/hh568446.aspx).

- **sqlcmd**: Sqlcmd aracını kullanarak Transact-SQL deyimlerini girebilirsiniz. Ayrıca, komut istemine sistem yordamları ve betik dosyaları da girebilirsiniz. Bu araç Transact-SQL toplu işlerini çalıştırmak için ODBC kullanır.

  Daha fazla bilgi için [sqlcmd ile bağlanma](https://msdn.microsoft.com/library/hh568447.aspx).

  > [!NOTE]
  > Bu araçta Linux ve Windows platformları arasında bazı farklılıklar vardır. Ayrıntılar için belgelere bakın.

#### <a name="database-access-libraries"></a>Veritabanı erişimi kitaplıkları

Kitaplıklar, veritabanı erişimi için R ve Python 'da kullanılabilir:

* R 'de, RODBC paketini veya dplyr paketini kullanarak veritabanı sunucusundaki SQL deyimlerini sorgulayabilir veya çalıştırabilirsiniz.
* Python 'da pyodbc kitaplığı, temel alınan katman olarak ODBC ile veritabanı erişimi sağlar.  

### <a name="azure-tools"></a>Azure Araçları

Aşağıdaki Azure Araçları VM'de yüklü:

* **Azure CLI**: Azure 'da kabuk komutları aracılığıyla Azure kaynakları oluşturmak ve yönetmek için komut satırı arabirimini kullanabilirsiniz. Azure araçlarını açmak için **Azure yardımı**'nı girin. Daha fazla bilgi için [Azure CLI belgeleri sayfasını](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* **Azure Depolama Gezgini**: Azure Depolama Gezgini, Azure depolama hesabınızda depoladığınız nesneler üzerinde gezinmek ve Azure bloblarına ve verileri yüklemek ve buradan veri indirmek için kullanabileceğiniz bir grafik aracıdır. Depolama Gezgini masaüstü kısayolu simgesinden erişebilirsiniz. Ayrıca, bunu bir kabuk isteminden **Storageexplorer**girerek açabilirsiniz. Bir X2Go istemcisinden oturum açmanız veya X11 iletme ayarlamış olmanız gerekir.
* **Azure kitaplıkları**: Önceden yüklenmiş kitaplıkların bazıları aşağıda verilmiştir.
  
  * **Python**: Python 'daki Azure ile ilgili kitaplıklar *Azure*, *azureml*, *pydocumentdb*ve *pyodbc*' dir. İlk üç kitaplıkları ile Azure depolama hizmetleri, Azure Machine Learning ve Azure Cosmos DB (Azure üzerinde bir NoSQL veritabanı) erişebilir. Dördüncü kitaplığı pyodbc (yanı sıra Microsoft ODBC sürücüsü için SQL Server) erişimi etkinleştirir SQL Server, Azure SQL veritabanı ve Azure SQL veri ambarı python'dan ODBC arabirimini kullanarak. Girin **pip listesi** listelenen tüm kitaplıkları görmek için. Hem Python 2.7 hem de 3,5 ortamlarında bu komutu çalıştırmak emin olun.
  * **R**: R 'deki Azure ile ilgili kitaplıklar AzureML ve RODBC.
  * **Java**: Azure Java kitaplıklarının listesi VM 'de/dsvm/sdk/AzureSDKJava dizininde bulunabilir. Anahtar kitaplıkları, SQL Server için Azure depolama ve Yönetimi API'leri, Azure Cosmos DB ve JDBC sürücüleri vardır.  

Erişebildiğiniz [Azure portalında](https://portal.azure.com) önceden yüklenmiş Firefox tarayıcısı. Azure portalında, oluşturmak, yönetmek ve Azure kaynaklarınızı izleyin.

### <a name="azure-machine-learning"></a>Azure Machine Learning

Azure Machine Learning oluşturmanızı, dağıtmanızı ve Tahmine dayalı analiz çözümlerini sağlayan tam olarak yönetilen bir bulut hizmetidir. Azure Machine Learning Studio'dan denemeleri ve modeller oluşturun. [Microsoft Azure Machine Learning](https://studio.azureml.net)ziyaret ederek veri bilimi sanal makinesi bir Web tarayıcısından erişebilirsiniz.

Azure Machine Learning Studio oturum açtıktan sonra, makine öğrenimi algoritmaları için bir mantıksal akış oluşturmak üzere bir deneme tuvali kullanabilirsiniz. Ayrıca, Azure Machine Learning barındırılan bir Jupyter not defterine erişiminiz vardır ve Machine Learning Studio ' de denemeleri ile sorunsuz bir şekilde çalışabilir. 

Machine learning web hizmeti arabiriminde sarmalama tarafından oluşturulmuş modelleri kullanıma hazır hale getirin. Makine öğrenimi modelleri, herhangi bir dilde yazılmış istemcilerin bu modellerdeki tahminleri çağırmasına olanak sağlar. Daha fazla bilgi için [Machine Learning belgeleri](https://azure.microsoft.com/documentation/services/machine-learning/).

Ayrıca, modellerinizi VM 'de R veya Python 'da oluşturabilir ve ardından bunları Azure Machine Learning üretim ortamında dağıtabilirsiniz. Biz R kitaplıkları yüklü (**AzureML**) ve Python (**azureml**) bu işlevselliği etkinleştirmek için.

R ve Python 'da modellerin Azure Machine Learning içine nasıl dağıtılacağı hakkında bilgi için, bkz. [veri bilimi sanal makinesi üzerinde yapabileceğiniz on şey](vm-do-ten-things.md).

> [!NOTE]
> Bu yönergeler Veri Bilimi Sanal Makinesi Windows sürümü için yazılmıştır. Ancak bilgileri var. için Azure Machine Learning modelleri dağıtma hakkında Linux VM'ye uygun sağlanır.

### <a name="machine-learning-tools"></a>Machine learning araçları

VM, önceden derlenmiş ve yerel olarak önceden yüklenmiş makine öğrenimi araçları ve algoritmalarıyla birlikte gelir. Bunlar:

* **Vowpal Wabbit**: Hızlı bir çevrimiçi öğrenme algoritması.
* **xgboost**: İyileştirilmiş, artırılmış ağaç algoritmaları sağlayan bir araç.
* **Rattle**: Kolay veri araştırması ve modelleme için R tabanlı bir grafik araç.
* **Python**: Anaconda Python, Scikit-öğren gibi kitaplıklar ile makine öğrenimi algoritmalarıyla birlikte sunulur. Diğer kitaplıkları kullanarak yükleyebileceğiniz `pip install` komutu.
* **Lightgbm**: Karar ağacı algoritmalarını temel alan hızlı, dağıtılmış ve yüksek performanslı bir gradyan arttırma çerçevesi.
* **R**: R için zengin bir makine öğrenimi işlevleri kitaplığı vardır. önceden yüklenmiş kitaplıklar lm, GLM, Rasgeleforest ve rpart ' ı içerir. Şu komutu çalıştırarak diğer kitaplıkları yükleyebilirsiniz:
  
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

Bu dizinde büyük, diğer tanıtımlar vardır. Vowpal Wabbit hakkında daha fazla bilgi için GitHub ve [Vowpal Wabbit wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki) ['nin bu bölümüne](https://github.com/JohnLangford/vowpal_wabbit) bakın.

#### <a name="xgboost"></a>xgboost

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

#### <a name="rattle"></a>Çıngırağı

Rattle ( **R** **A**nalitik **T**aracı **T**o **L**kazanın **E** GUI tabanlı bir veri keşfi ve modelleme asily) kullanır. Veriler, kolayca modellenebilir, denetimli hem de Denetimsiz modellerinden veri yapıları, grafik performansını modelleri sunar dönüşümler veriler istatistiksel ve görsel özetlerini sunar ve puanları yeni veri kümeleri. Ayrıca R kodunu doğrudan R çalıştırmak veya daha fazla analiz için başlangıç noktası olarak kullanılan işlemleri kullanıcı arabiriminde çoğaltma oluşturur.

Çıngırağı çalıştırmak için bir grafik Masaüstü Oturum açma oturumunda olması gerekir. Terminal üzerinde r ' yi girerek r ortamını açın. R isteminde aşağıdaki komutları girin:

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

İşte öğrenme ve araştırma nasıl devam edebilirsiniz:

* [Linux için veri bilimi sanal makinesi veri bilimi](linux-dsvm-walkthrough.md) izlenecek yol, burada sağlanan Linux dsvm ile birkaç ortak veri bilimi görevinin nasıl yapılacağını gösterir. 
* Bu makalede açıklanan araçları deneyerek DSVM 'deki çeşitli veri bilimi araçlarını inceleyin. Ayrıca, VM 'de `dsvm-more-info` yüklü araçlar hakkında daha fazla bilgi için temel bir giriş ve işaretçiler için sanal makine içindeki kabukta de çalıştırabilirsiniz.  
* Sistematik olarak kullanarak uçtan uca analitik çözümler oluşturmayı öğrenin [Team Data Science Process](https://aka.ms/tdsp).
* Ziyaret [Azure AI Gallery](https://gallery.azure.ai/) Azure yapay ZEKA hizmetlerini kullanma makine öğrenimi ve veri analizi örnekleri için.
