---
title: Desteklenen diller
titleSuffix: Azure Data Science Virtual Machine
description: Desteklenen program dilleri ve ilgili araçlar Veri Bilimi Sanal Makine'ye önceden yüklenmiş.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: lobrien
ms.author: laobri
ms.topic: conceptual
ms.date: 12/12/2019
ms.openlocfilehash: e7b32579712e89c0d5595303ee7e03d8b2462607
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283660"
---
# <a name="languages-supported-on-the-data-science-virtual-machine"></a>Veri Bilimi Sanal Makine'de desteklenen diller 

Veri Bilimi Sanal Makine (DSVM) yapay zeka (AI) uygulamaları oluşturmak için birkaç önceden inşa edilmiş dil ve geliştirme araçları ile birlikte gelir. İşte bazı önemli olanlar.

## <a name="python-windows-server-2016-edition"></a>Python (Windows Server 2016 sürümü)

|    |           |
| ------------- | ------------- |
| Desteklenen dil sürümleri | Python 2.7 ve 3.7 |
| Desteklenen DSVM sürümleri      | Windows Server 2016     |
| Nasıl yapılandırılır / DSVM yüklü?  | İki `conda` genel ortam oluşturulur: <br /> * `root` Python 3.7'de `/anaconda/` bulunan ortamdır. <br/> * `python2` Python 2.7'de `/anaconda/envs/python2` bulunan ortamdır.       |
| Örneklere bağlantılar      | Python için örnek Jupyter dizüstü bilgisayarlar dahildir.     |
| DSVM ile ilgili araçlar      | PySpark, R, Julia.      |

> [!NOTE]
> Mart 2018'den önce oluşturulan Windows Server 2016'nın yapılarında Python 3.5 ve Python 2.7 bulunur. Python 2.7 conda **kök** ortamı, **py37** ise Python 3.7 ortamıdır.

### <a name="how-to-use-and-run-it"></a>Nasıl kullanılır ve çalıştırın    

* Komut isteminde çalıştırın:

  Çalıştırmak istediğiniz Python sürümüne bağlı olarak bir komut istemi açın ve aşağıdaki yöntemlerden birini kullanın:

    ```
    # To run Python 2.7
    activate python2
    python --version
    
    # To run Python 3.7
    activate 
    python --version 
    ```
    
* Bir IDE'de kullanın:

  Visual Studio Community sürümünde yüklü olan Visual Studio (PTVS) için Python Araçlarını kullanın. Varsayılan olarak, PTVS'de otomatik olarak ayarlanan tek ortam Python 3.6'dır. 

    > [!NOTE]
    > PTVS'yi Python 2.7'ye işaret etmek için PTVS'de özel bir ortam oluşturmanız gerekir. Visual Studio Community Edition'da bu ortam yolunu ayarlamak için **Araçlar** -> **Python Araçları** -> **Python Ortamları'na** gidin ve **+ Özel'i**seçin. Ardından, konumu **c:\anaconda\envs\python2** olarak ayarlayın ve **Otomatik Algılama'yı**seçin.

* Jupyter'da kullanım:

  Jupyter'ı açın ve yeni bir not defteri oluşturmak için **Yeni'yi** seçin. Çekirdek türünü Python 3.7 için _Python [Conda Root]_ ve Python 2.7 için _Python [Conda env:python2]_ olarak ayarlayabilirsiniz.

* Python paketlerini yükleyin:

  DSVM'deki varsayılan Python ortamları, tüm kullanıcılar tarafından okunabilen genel ortamlardır. Ancak yalnızca yöneticiler genel paketler yazabilir ve yükleyebilir. Paketleri genel ortama yüklemek için komutu yönetici olarak kullanarak `activate` root veya python2 ortamına etkinleştirin. Ardından, paketleri beğenmek `conda` veya `pip` yüklemek veya güncelleştirmek için bir paket yöneticisi kullanabilirsiniz.

## <a name="python-linux-edition"></a>Python (Linux sürümü)

|    |           |
| ------------- | ------------- |
| Desteklenen dil sürümleri | Python 2.7 ve 3.5 |
| Desteklenen DSVM sürümleri      | Linux   |
| Nasıl yapılandırılır / DSVM yüklü?  | İki `conda` genel ortam oluşturulur: <br /> * `root`ortamda bulunan `/anaconda/` Python 2.7 olduğunu. <br/> * `py35`ortamda bulunan `/anaconda/envs/py35`Python 3.5 olduğunu.       |
| Örneklere bağlantılar      | Python için örnek Jupyter dizüstü bilgisayarlar dahildir.     |
| DSVM ile ilgili araçlar      | PySpark, R, Julia      |
### <a name="how-to-use-and-run-it"></a>Nasıl kullanılır ve çalıştırın    

* Bir terminalde çalıştırın:

  Çalıştırmak istediğiniz Python sürümüne bağlı olarak terminali açın ve aşağıdakilerden birini yapın:

    ```
    # To run Python 2.7
    source activate 
    python --version
    
    # To run Python 3.5
    source activate py35
    python --version
    
    ```
* Bir IDE'de kullanın:

  Visual Studio Community sürümünde yüklü olan PyCharm'ı kullanın. 

* Jupyter'da kullanım:

  Jupyter'ı açın ve yeni bir not defteri oluşturmak için **Yeni'yi** seçin. Çekirdeği Python 2.7 için **Python [Conda Root]** ve Python 3.5 ortamı için **Python [Conda env:py35]** olarak ayarlayabilirsiniz. 

* Python paketlerini yükleyin:

  DSVM'deki varsayılan Python ortamları tüm kullanıcılar tarafından okunabilen genel ortamlardır. Ancak yalnızca yöneticiler genel paketler yazabilir ve yükleyebilir. Paketleri genel ortama yüklemek için, `source activate` komutu yönetici olarak veya sudo izinli bir kullanıcı olarak kullanarak root veya py35 ortamına etkinleştirin. Ardından, paketleri beğenmek `conda` veya `pip` yüklemek veya güncelleştirmek için bir paket yöneticisi kullanabilirsiniz.


## <a name="r"></a>R

|    |           |
| ------------- | ------------- |
| Desteklenen dil sürümleri | Microsoft R Open 3.x (%100 CRAN-R ile uyumlu)<br /> Microsoft R Server 9.x Developer sürümü (ölçeklenebilir kurumsal alete hazır R platformu)|
| Desteklenen DSVM sürümleri      | Linux, Windows     |
| Nasıl yapılandırılır / DSVM yüklü?  | Windows:`C:\Program Files\Microsoft\ML Server\R_SERVER` <br />Linux:`/usr/lib64/microsoft-r/3.3/lib64/R`    |
| Örneklere bağlantılar      | R için örnek Jupyter dizüstü bilgisayarlar dahildir.     |
| DSVM ile ilgili araçlar      | SparkR, Python, Julia      |
### <a name="how-to-use-and-run-it"></a>Nasıl kullanılır ve çalıştırın    

**Windows**:

* Komut isteminde çalıştırın:

  Bir komut istemi açın ve `R` yazın.

* Bir IDE'de kullanın:

  Visual Studio Topluluk sürümünde veya RStudio'da yüklü olan Visual Studio (RTVS) için RTools'u kullanın. Bunlar Başlat menüsünde veya masaüstü simgesi olarak kullanılabilir. 

* Jupyter'da Kullanımı

  Jupyter'ı açın ve yeni bir not defteri oluşturmak için **Yeni'yi** seçin. Jupyter R çekirdeğini (IRKernel) kullanmak için çekirdek türünü **R** olarak ayarlayabilirsiniz.

* R paketlerini yükleyin:

  R, DSVM'ye tüm kullanıcılar tarafından okunabilen küresel bir ortamda yüklenir. Ancak yalnızca yöneticiler genel paketler yazabilir ve yükleyebilir. Paketleri genel ortama yüklemek için, önceki yöntemlerden birini kullanarak R çalıştırın. Ardından, paketleri yüklemek veya `install.packages()` güncelleştirmek için R paket yöneticisini çalıştırabilirsiniz.

**Linux**:

* Terminalde çalıştırın:

  Bir terminal açın `R`ve çalıştırın.  

* Bir IDE'de kullanın:

  Linux DSVM yüklü RStudio'yu kullanın.  

* Jupyter'da kullanım:

  Jupyter'ı açın ve yeni bir not defteri oluşturmak için **Yeni'yi** seçin. Jupyter R çekirdeğini (IRKernel) kullanmak için çekirdek türünü **R** olarak ayarlayabilirsiniz. 

* R paketlerini yükleyin:

  R, DSVM'ye tüm kullanıcılar tarafından okunabilen küresel bir ortamda yüklenir. Ancak yalnızca yöneticiler genel paketler yazabilir ve yükleyebilir. Paketleri genel ortama yüklemek için, önceki yöntemlerden birini kullanarak R çalıştırın. Ardından, paketleri yüklemek veya `install.packages()` güncelleştirmek için R paket yöneticisini çalıştırabilirsiniz.


## <a name="julia"></a>Julia

|    |           |
| ------------- | ------------- |
| Desteklenen dil sürümleri | 0.6 |
| Desteklenen DSVM sürümleri      | Linux, Windows     |
| Nasıl yapılandırılır / DSVM yüklü?  | Windows: Yüklü`C:\JuliaPro-VERSION`<br /> Linux: Yüklü`/opt/JuliaPro-VERSION`    |
| Örneklere bağlantılar      | Julia için örnek Jupyter dizüstü bilgisayarlar dahildir.     |
| DSVM ile ilgili araçlar      | Python, R      |
### <a name="how-to-use-and-run-it"></a>Nasıl kullanılır ve çalıştırın    

**Windows**:

* Komut isteminde çalıştırın

  Komut istemini açın `julia`ve çalıştırın.
* Bir IDE'de kullanın:

  DSVM yüklü ve masaüstü kısayolu olarak kullanılabilir Julia IDE ile kullanın. `Juno`

* Jupyter'da kullanım:

  Jupyter'ı açın ve yeni bir not defteri oluşturmak için **Yeni'yi** seçin. Çekirdek türünü **Julia VERSION**olarak ayarlayabilirsiniz.

* Julia paketlerini yükleyin:

  Varsayılan Julia konumu, tüm kullanıcılar tarafından okunabilen genel bir ortamdır. Ancak yalnızca yöneticiler genel paketler yazabilir ve yükleyebilir. Paketleri genel ortama yüklemek için, önceki yöntemlerden birini kullanarak Julia'yı çalıştırın. Ardından, paketleri yüklemeveya güncelleştirme gibi `Pkg.add()` Julia paket yöneticisi komutlarını çalıştırabilirsiniz.


**Linux**:
* Bir terminalde çalıştırın:

  Bir terminal açın `julia`ve çalıştırın.
* Bir IDE'de kullanın:

  `Juno`DSVM'de Julia IDE yüklü ve **Uygulama** menüsü kısayolu olarak kullanılabilen bir kullanım.

* Jupyter'da kullanım:

  Jupyter'ı açın ve yeni bir not defteri oluşturmak için **Yeni'yi** seçin. Çekirdek türünü **Julia VERSION**olarak ayarlayabilirsiniz.

* Julia paketlerini yükleyin:

  Varsayılan Julia konumu, tüm kullanıcılar tarafından okunabilen genel bir ortamdır. Ancak yalnızca yöneticiler genel paketler yazabilir ve yükleyebilir. Paketleri genel ortama yüklemek için, önceki yöntemlerden birini kullanarak Julia'yı çalıştırın. Ardından, paketleri yüklemeveya güncelleştirme gibi `Pkg.add()` Julia paket yöneticisi komutlarını çalıştırabilirsiniz.

## <a name="other-languages"></a>Diğer diller

**C#**: Windows'da kullanılabilir ve Visual Studio `Developer Command Prompt for Visual Studio`Community sürümünden veya `csc` komutu çalıştırabileceğiniz ,

**Java**: OpenJDK, DSVM'nin hem Linux hem de Windows sürümlerinde mevcuttur ve yolda ayarlanır. Java'yı kullanmak `javac` için, Windows'da komut istemine veya Linux'taki bash kabuğuna veya `java` komut istemine yazın.

**Node.js**: Node.js, DSVM'nin hem Linux hem de Windows sürümlerinde kullanılabilir ve yolda ayarlanır. Düğüm.js'ye erişmek için, Windows'ta komut istemine `node` veya Linux'taki bash kabuğuna komut `npm` uy. Windows'da, Node.js araçlarının Visual Studio uzantısı, Node.js uygulamanızı geliştirmek için grafik seli sağlamak üzere yüklenir.

**F#**: Windows'da kullanılabilir ve Visual Studio `Developer Command Prompt for Visual Studio`Community sürümünden veya `fsc` komutu çalıştırabileceğiniz bir yerden erişilebilir.
