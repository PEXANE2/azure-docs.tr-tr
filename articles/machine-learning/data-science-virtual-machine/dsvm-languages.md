---
title: Desteklenen diller
titleSuffix: Azure Data Science Virtual Machine
description: Desteklenen program dilleri ve ilgili Araçlar Veri Bilimi Sanal Makinesi önceden yüklenir.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 09/11/2017
ms.openlocfilehash: 0cedc9ede43d18d0b94b8a516170db53e3a27910
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70885639"
---
# <a name="languages-supported-on-the-data-science-virtual-machine"></a>Veri bilimi sanal makinesi üzerinde desteklenen diller 

Veri Bilimi Sanal Makinesi (DSVM), yapay zeka (AI) uygulamalarınızı oluşturmak için önceden oluşturulmuş birkaç dil ve geliştirme araçlarıyla birlikte gelir. Bazı önemli tabloları aşağıda verilmiştir.

## <a name="python-windows-server-2016-edition"></a>Python (Windows Server 2016 Edition)

|    |           |
| ------------- | ------------- |
| Desteklenen dil sürümleri | Python 2,7 ve 3,6 |
| Desteklenen DSVM sürümleri      | Windows Server 2016     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | İki genel `conda` ortam oluşturulur: <br /> * Konumunda`/anaconda/` bulunan ortam Python 3,6 ' dir. `root` <br/> * Konumunda`/anaconda/envs/python2` bulunan ortam Python 2,7 ' dir. `python2`       |
| Örneklere bağlantılar      | Python için örnek Jupyıter Not defterleri dahil edilmiştir.     |
| DSVM 'deki ilgili araçlar      | PySpark, R, Julia.      |

> [!NOTE]
> 2018 Mart ' den önce oluşturulan Windows Server 2016 ' in derlemeleri Python 3,5 ve Python 2,7 ' i içerir. Python 2,7, Conda **kök** ortamıdır ve **py35** , Python 3,5 ortamıdır.

### <a name="how-to-use-and-run-it"></a>Kullanma ve çalıştırma    

* Komut isteminden çalıştırın:

  Bir komut istemi açın ve çalıştırmak istediğiniz Python sürümüne bağlı olarak aşağıdaki yöntemlerden birini kullanın:

    ```
    # To run Python 2.7
    activate python2
    python --version
    
    # To run Python 3.6
    activate 
    python --version 
    ```
    
* IDE içinde kullanın:

  Visual Studio Community Edition 'da yüklü olan Visual Studio için Python Araçları (PTV) kullanın. Varsayılan olarak, PTV 'lerde otomatik olarak ayarlanan tek ortam Python 3,6 ' dir. 

    > [!NOTE]
    > Python 2,7 ' de PTV 'leri göstermek için PTV 'lerde özel bir ortam oluşturmanız gerekir. Visual Studio Community sürümünde bu ortam yolunu ayarlamak için **Araçlar** -> **Python araçları** -> **Python ortamları** ' na gidin ve **+ özel**' i seçin. Sonra, konumu **c:\anaconda\envs\python2** olarak ayarlayın ve **Otomatik Algıla**seçeneğini belirleyin.

* Jupyter içinde kullanın:

  Jupyter ' ı açın **ve yeni ' yi seçerek yeni** bir not defteri oluşturun. Python 3,6 için bir Python _[Conda root]_ ve Python 2,7 için _Python [Conda env: python2]_ olarak çekirdek türünü ayarlayabilirsiniz.

* Python paketlerini yükler:

  DSVM 'deki varsayılan Python ortamları, tüm kullanıcılar tarafından okunabilen küresel ortamlardır. Ancak, genel paketleri yalnızca Yöneticiler yazabilir ve yükleyebilir. Paketleri küresel ortama yüklemek için, `activate` komutunu yönetici olarak kullanarak kök veya python2 ortamına etkinleştirin. Ardından, paketleri yüklemek veya güncelleştirmek için veya `conda` `pip` gibi bir paket Yöneticisi kullanabilirsiniz.

## <a name="python-linux-and-windows-server-2012-edition"></a>Python (Linux ve Windows Server 2012 sürümü)

|    |           |
| ------------- | ------------- |
| Desteklenen dil sürümleri | Python 2,7 ve 3,5 |
| Desteklenen DSVM sürümleri      | Linux, Windows Server 2012    |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | İki genel `conda` ortam oluşturulur: <br /> * `root`konumunda `/anaconda/` bulunan ortam Python 2,7 ' dir. <br/> * `py35`konumunda `/anaconda/envs/py35`bulunan ortam Python 3,5 ' dir.       |
| Örneklere bağlantılar      | Python için örnek Jupyıter Not defterleri dahil edilmiştir.     |
| DSVM 'deki ilgili araçlar      | PySpark, R, Julia      |
### <a name="how-to-use-and-run-it"></a>Kullanma ve çalıştırma    

**Linux**
* Bir terminalde Çalıştır:

  Terminal ' i açın ve çalıştırmak istediğiniz Python sürümüne bağlı olarak aşağıdakilerden birini yapın:

    ```
    # To run Python 2.7
    source activate 
    python --version
    
    # To run Python 3.5
    source activate py35
    python --version
    
    ```
* IDE içinde kullanın:

  Visual Studio Community Edition 'da yüklü olan Pyı düğmesini kullanın. 

* Jupyter içinde kullanın:

  Jupyter ' ı açın **ve yeni ' yi seçerek yeni** bir not defteri oluşturun. Python 3,5 ortamı için Python 2,7 ve **Python [Conda env: py35]** için çekirdek türünü **Python [Conda root]** olarak ayarlayabilirsiniz. 

* Python paketlerini yükler:

  DSVM üzerinde varsayılan Python ortamları, genel ortamları okunabilir ve tüm kullanıcılar ' dir. Ancak, genel paketleri yalnızca Yöneticiler yazabilir ve yükleyebilir. Paketleri küresel ortama yüklemek için, `source activate` komutunu yönetici olarak veya sudo izinleri olan bir kullanıcı olarak kullanarak kök veya py35 ortamına etkinleştirin. Ardından, paketleri yüklemek veya güncelleştirmek için veya `conda` `pip` gibi bir paket Yöneticisi kullanabilirsiniz.

**Windows 2012**
* Komut isteminden çalıştırın:

  Çalıştırmak istediğiniz Python sürümüne bağlı olarak bir komut istemi açın ve aşağıdakilerden birini yapın:

     ```
    # To run Python 2.7
    activate 
    python --version
    
    # To run Python 3.5
    activate py35
    python --version
    
    ```
* IDE içinde kullanın:

  Python araçları için Visual Studio (Visual Studio Community edition yüklü PTVS) kullanın. PTV 'lerde otomatik olarak ayarlanan tek ortam Python 2,7 ' dir.
    > [!NOTE]
    > Python 3,5 ' de PTV 'leri işaret etmek için PTV 'lerde özel bir ortam oluşturmanız gerekir. Visual Studio Community sürümünde bu ortam yolunu ayarlamak için **Araçlar** -> **Python araçları** -> **Python ortamları** ' na gidin ve **+ özel**' i seçin. Sonra, konumunu olarak `c:\anaconda\envs\py35` ayarlayın ve _Otomatik Algıla_öğesini seçin.

* Jupyter içinde kullanın:

  Jupyter ' ı açın **ve yeni ' yi seçerek yeni** bir not defteri oluşturun. Python 2,7 için bir Python **[Conda root]** ve Python 3,5 için **Python [Conda env: py35]** olarak çekirdek türünü ayarlayabilirsiniz. 

* Python paketlerini yükler:

  DSVM 'deki varsayılan Python ortamları, tüm kullanıcılar tarafından okunabilen küresel ortamlardır. Ancak, genel paketleri yalnızca Yöneticiler yazabilir ve yükleyebilir. Paketleri küresel ortama yüklemek için, `activate` komutunu yönetici olarak kullanarak kök veya py35 ortamına etkinleştirin. Ardından, paketleri yüklemek veya güncelleştirmek için veya `conda` `pip` gibi bir paket Yöneticisi kullanabilirsiniz.

## <a name="r"></a>R

|    |           |
| ------------- | ------------- |
| Desteklenen dil sürümleri | Microsoft R Open 3. x (CRAN-R ile% 100 uyumlu)<br /> Microsoft R Server 9. x geliştirici sürümü (bir kurumsal özellikli ölçeklenebilir R platformu)|
| Desteklenen DSVM sürümleri      | Linux, Windows     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Windows: `C:\Program Files\Microsoft\ML Server\R_SERVER` <br />Linux: `/usr/lib64/microsoft-r/3.3/lib64/R`    |
| Örneklere bağlantılar      | R için örnek Jupyıter Not defterleri dahil edilmiştir.     |
| DSVM 'deki ilgili araçlar      | SparkR, Python, Julia      |
### <a name="how-to-use-and-run-it"></a>Kullanma ve çalıştırma    

**Windows**:

* Komut isteminden çalıştırın:

  Bir komut istemi açın ve yazın `R`.

* IDE içinde kullanın:

  Kullanım RTools için Visual Studio (Visual Studio Community sürümü veya RStudio yüklü RTVS). Bunlar, başlangıç menüsünde veya masaüstü simgesi olarak kullanılabilir. 

* Jupyıter 'da kullanma

  Jupyter ' ı açın **ve yeni ' yi seçerek yeni** bir not defteri oluşturun. Jupyıter R çekirdeğini (ırkernel) kullanmak için çekirdek türünü **R** olarak ayarlayabilirsiniz.

* R paketlerini yükler:

  R, tüm kullanıcılar tarafından okunabilen küresel bir ortamda DSVM 'ye yüklenir. Ancak, genel paketleri yalnızca Yöneticiler yazabilir ve yükleyebilir. Paketleri genel ortama yüklemek için önceki yöntemlerden birini kullanarak R 'yi çalıştırın. Ardından, paketleri yüklemek veya güncelleştirmek için R paket `install.packages()` yöneticisini çalıştırabilirsiniz.

**Linux**:

* Terminalde Çalıştır:

  Bir Terminal açın ve çalıştırın `R`.  

* IDE içinde kullanın:

  Linux DSVM 'de yüklü olan RStudio 'yu kullanın.  

* Jupyter içinde kullanın:

  Jupyter ' ı açın **ve yeni ' yi seçerek yeni** bir not defteri oluşturun. Jupyıter R çekirdeğini (ırkernel) kullanmak için çekirdek türünü **R** olarak ayarlayabilirsiniz. 

* R paketlerini yükler:

  R, tüm kullanıcılar tarafından okunabilen küresel bir ortamda DSVM 'ye yüklenir. Ancak, genel paketleri yalnızca Yöneticiler yazabilir ve yükleyebilir. Paketleri genel ortama yüklemek için önceki yöntemlerden birini kullanarak R 'yi çalıştırın. Ardından, paketleri yüklemek veya güncelleştirmek için R paket `install.packages()` yöneticisini çalıştırabilirsiniz.


## <a name="julia"></a>Julia

|    |           |
| ------------- | ------------- |
| Desteklenen dil sürümleri | 0,6 |
| Desteklenen DSVM sürümleri      | Linux, Windows     |
| Nasıl, yapılandırılmış / DSVM üzerinde yüklü?  | Windows: Yükleme saati:`C:\JuliaPro-VERSION`<br /> Linux: Yükleme saati:`/opt/JuliaPro-VERSION`    |
| Örneklere bağlantılar      | Julia için örnek Jupyıter Not defterleri dahil edilmiştir.     |
| DSVM 'deki ilgili araçlar      | Python, R      |
### <a name="how-to-use-and-run-it"></a>Kullanma ve çalıştırma    

**Windows**:

* Komut isteminden Çalıştır

  Bir komut istemi açın ve çalıştırın `julia`.
* IDE içinde kullanın:

  Dsvm 'de yüklü olan ve masaüstü kısayolu olarak kullanılabilen Julia IDE ile birlikte kullanın `Juno` .

* Jupyter içinde kullanın:

  Jupyter ' ı açın **ve yeni ' yi seçerek yeni** bir not defteri oluşturun. Çekirdek türünü **JULIA sürümü**olarak ayarlayabilirsiniz.

* Julia paketlerini yükler:

  Varsayılan Julia konumu, tüm kullanıcılar tarafından okunabilen küresel bir ortamdır. Ancak, genel paketleri yalnızca Yöneticiler yazabilir ve yükleyebilir. Paketleri küresel ortama yüklemek için yukarıdaki yöntemlerden birini kullanarak Julia 'yı çalıştırın. Ardından, paketleri yüklemek veya güncelleştirmek `Pkg.add()` için Julia Paket Yöneticisi komutlarını çalıştırabilirsiniz.


**Linux**:
* Bir terminalde Çalıştır:

  Bir Terminal açın ve çalıştırın `julia`.
* IDE içinde kullanın:

  Julia `Juno`IDE 'yi dsvm 'de yüklü ve **uygulama** menüsü kısayolu olarak kullanılabilir şekilde kullanın.

* Jupyter içinde kullanın:

  Jupyter ' ı açın **ve yeni ' yi seçerek yeni** bir not defteri oluşturun. Çekirdek türünü **JULIA sürümü**olarak ayarlayabilirsiniz.

* Julia paketlerini yükler:

  Varsayılan Julia konumu, tüm kullanıcılar tarafından okunabilen küresel bir ortamdır. Ancak, genel paketleri yalnızca Yöneticiler yazabilir ve yükleyebilir. Paketleri küresel ortama yüklemek için yukarıdaki yöntemlerden birini kullanarak Julia 'yı çalıştırın. Ardından, paketleri yüklemek veya güncelleştirmek `Pkg.add()` için Julia Paket Yöneticisi komutlarını çalıştırabilirsiniz.

## <a name="other-languages"></a>Diğer diller

**C#** : Windows 'da kullanılabilir ve Visual Studio Community Edition üzerinden veya `Developer Command Prompt for Visual Studio`üzerinde erişilebilir, burada `csc` komutu çalıştırabilirsiniz.

**Java**: OpenJDK, DSVM 'nin hem Linux hem de Windows sürümlerinde bulunur ve yolunda ayarlanır. Java kullanmak için Windows veya Linux `javac` 'taki `java` bash kabuğu 'ndaki bir komut isteminde veya komutunu yazın.

**Node.js**: Node. js, DSVM 'nin hem Linux hem de Windows sürümlerinde bulunur ve yolunda ayarlanır. Node. js ' ye erişmek için Windows `node` 'ta `npm` veya Linux 'taki bash kabuğu 'ndaki bir komut isteminde veya komutunu yazın. Windows 'da Node. js Araçları için Visual Studio uzantısı, Node. js uygulamanızı geliştirmek için bir grafik IDE sağlamak üzere yüklenmiştir.

**F#** : Windows 'da kullanılabilir ve Visual Studio Community Edition aracılığıyla veya `Developer Command Prompt for Visual Studio`' de erişilebilir, burada `fsc` komutu çalıştırabilirsiniz.
