---
title: Python geliştirme ortamı ayarlama
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmetiyle çalışırken geliştirme ortamının nasıl yapılandırılacağını öğrenin. Bu makalede, Conda ortamlarını kullanmayı, yapılandırma dosyalarını oluşturmayı ve bulut tabanlı Not defteri sunucunuzu, Jupyter not defterlerini, Azure Databricks, Azure Notebooks, IDEs, kod düzenleyicilerini ve Veri Bilimi Sanal Makinesi yapılandırmayı öğreneceksiniz.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.topic: conceptual
ms.date: 07/31/2019
ms.custom: seodec18
ms.openlocfilehash: 45b28b4d88c670a8b2ec34b93a342f06b80e02d7
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68668474"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Azure Machine Learning için bir geliştirme ortamı yapılandırma

Bu makalede, Azure Machine Learning hizmetiyle çalışmak için bir geliştirme ortamının nasıl yapılandırılacağını öğreneceksiniz. Azure Machine Learning hizmet platformu belirsiz. Geliştirme ortamınız için tek sabit gereksinim Python 3 ' dir. Anaconda veya virtualenv gibi yalıtılmış bir ortam de önerilir.

Aşağıdaki tabloda, bu makalede ele alınan her geliştirme ortamı, profesyonelleri ve dezavantajlarla birlikte gösterilmektedir.

| Ortam | Uzmanları | Simgeler |
| --- | --- | --- |
| [Bulut tabanlı Not defteri VM](#notebookvm) | Başlamak için en kolay yol. Tüm SDK, çalışma alanı sanal makinenizde zaten yüklüdür ve Not defteri öğreticileri önceden klonlanır ve çalıştırılmaya hazırlanmıştır. | Geliştirme ortamınız ve bağımlılıklarınız üzerinde denetim olmaması. Linux VM için ek maliyet (sanal makine, ücretlerden kaçınmak için kullanımda olmadığında durdurulabilir). [Fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)bakın. |
| [Yerel ortam](#local) | Geliştirme ortamınızın ve bağımlılıklarınızın tam denetimi. İstediğiniz herhangi bir yapı aracı, ortam veya IDE ile çalıştırın. | Kullanmaya başlamak için daha uzun sürer. Gerekli SDK paketlerinin yüklü olması ve henüz yoksa bir ortamın de yüklü olması gerekir. |
| [Azure Databricks](#aml-databricks) | Ölçeklenebilir Apache Spark platformunda büyük ölçekli yoğun makine öğrenimi iş akışlarını çalıştırmak için idealdir. | Deneysel makine öğrenimi veya daha küçük ölçekli denemeleri ve iş akışları için fazla sonlandırılmalıdır. Azure Databricks için ek ücret tahakkuk ettir. [Fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/databricks/)bakın. |
| [Veri Bilimi Sanal Makinesi (DSVM)](#dsvm) | Bulut tabanlı Not defteri VM 'sine benzer (Python ve SDK önceden yüklenmiş), ancak daha popüler veri bilimi ve Machine Learning araçları önceden yüklenmiş olarak. Kolayca ölçeklendirilmesi ve diğer özel araçlar ve iş akışlarıyla birleştirmek. | Bulut tabanlı Notebook VM ile karşılaştırıldığında daha yavaş bir başlangıç deneyimi. |
| [Azure Notebooks](#aznotebooks) | Python ve SDK önceden yüklenmiş olarak ücretsiz ve hafif ağırlığa Başlarken deneyimi. | Bulut tabanlı Notebook VM 'ye kıyasla daha az güçlü VM mevcuttur. Çalışma alanından ve diğer kaynaklardan yalıtılmıştır. |

Bu makalede ayrıca aşağıdaki araçlar için ek kullanım ipuçları sunulmaktadır:

* [Jupyter Not defterleri](#jupyter): Zaten Jupyter Notebook kullanıyorsanız SDK 'nın yüklenmesi gereken bazı ek özellikler vardır.

* [Visual Studio Code](#vscode): Visual Studio Code kullanıyorsanız, yükleyebileceğiniz bazı yararlı uzantılar vardır.

## <a name="prerequisites"></a>Önkoşullar

Bir Azure Machine Learning hizmeti çalışma alanı. Çalışma alanını oluşturmak için, bkz. [Azure Machine Learning hizmet çalışma alanı oluşturma](setup-create-workspace.md). [Bulut tabanlı bir not defteri sunucusu](#notebookvm), [dsvm](#dsvm), [Azure Databricks](#aml-databricks)veya [Azure Notebooks](#aznotebooks)kullanmaya başlamak için bir çalışma alanı yeterlidir.

[Yerel bilgisayarınız](#local)için SDK ortamını yüklemek üzere [Jupyter Notebook sunucu](#jupyter) veya [Visual Studio Code](#vscode) şunları yapmanız gerekir:

- [Anaconda](https://www.anaconda.com/download/) ya da [miniconda](https://conda.io/miniconda.html) Paket Yöneticisi.

- Linux veya macOS 'da bash kabuğu gerekir.

    > [!TIP]
    > Linux veya macOS 'ta çalışıyorsanız ve Bash dışında bir Shell kullanıyorsanız (örneğin, ZSH), bazı komutları çalıştırdığınızda hatalar alabilirsiniz. Bu sorunu geçici olarak çözmek için kullanın `bash` yeni bir bash Kabuğu'nu başlatın ve orada komutları çalıştırmak için komutu.

- Windows üzerinde bir komut istemi veya Anaconda istemi (Anaconda ve Miniconda ile yüklenen) gerekir.

## <a id="notebookvm"></a>Kendi bulut tabanlı Not defteriniz VM 'niz

Not defteri sanal makinesi (Önizleme), bir Jupyter Not defteri sunucusu, JupyterLab ve tamamen hazırlanmış ML ortamı ile veri bilimcileri sağlayan, güvenli, bulut tabanlı bir Azure iş istasyonudur.

Not defteri VM 'si:

+ **Güvenli**. VM ve Not defteri erişiminin HTTPS ile güvenli hale getirilmesinden ve Azure Active Directory varsayılan olarak, BT uzmanları Multi-Factor Authentication gibi çoklu oturum açma ve diğer güvenlik özelliklerini kolayca uygulayabilir.

+ **Önceden yapılandırılmış**. Bu tamamen hazırlanmış Python ML ortamı, Pedigree ' i popüler IaaS Veri Bilimi VM'si çizer ve şunları içerir:
  + Azure ML Python SDK (en son)
  + Çalışma alanınızda çalışmak için otomatik yapılandırma
  + Jupyter Not defteri sunucusu
  + JupyterLab Not defteri IDE
  + Önceden yapılandırılmış GPU sürücüleri
  + Derin öğrenme çerçevelerinin seçimi


  Kodunuz varsa, sanal makine Azure Machine Learning hizmetini keşfetmenize ve nasıl kullanacağınızı öğrenmenize yardımcı olacak öğreticiler ve örnekler içerir. Örnek Not defterleri, çalışma alanınızın Azure Blob depolama hesabında depolanır ve bunları VM 'lerde paylaşılabilir hale getirir. Çalıştırıldığında, çalışma alanınızın veri depolarına ve işlem kaynaklarına erişimi de vardır.

+ **Basit kurulum**: Azure Machine Learning çalışma alanınızın içinden dilediğiniz zaman oluşturun. Yalnızca bir ad girin ve Azure VM türünü belirtin. Şimdi bu [hızlı başlangıç ile deneyin: Azure Machine Learning](quickstart-run-cloud-notebook.md)kullanmaya başlamak için bulut tabanlı bir not defteri sunucusu kullanın.

+ **Özelleştirilebilir**. Yönetilen ve güvenli bir VM teklifi olsa da, donanım özelliklerine tam erişim sağlar ve bunu sizin için istediğiniz gibi özelleştirebilirsiniz. Örneğin, nolevel sinir ağ mimarisinin adım adım hata ayıklamasını gerçekleştirmek için en son NVIDIA V100 yönetimli VM 'yi hızlıca oluşturun.

Not defteri VM 'si ücretlerini durdurmak için, [Not defteri VM 'yi durdurun](quickstart-run-cloud-notebook.md#stop-the-notebook-vm).

## <a id="dsvm"></a>Veri bilimi sanal makinesi

DSVM, özelleştirilmiş bir sanal makine (VM) görüntüsüdür. Önceden yapılandırılmış olan veri bilimi işleri için tasarlanmıştır:

  - TensorFlow, PyTorch, Scikit-öğren, XGBoost ve Azure Machine Learning SDK gibi paketler
  - Spark tek başına ve detaya gitme gibi popüler veri bilimi araçları
  - Azure CLı, AzCopy ve Depolama Gezgini gibi Azure Araçları
  - Visual Studio Code ve Pydüğme gibi tümleşik geliştirme ortamları (IDEs)
  - Jupyter Notebook sunucusu

Azure Machine Learning SDK, DSVM 'nin Ubuntu veya Windows sürümünde kullanılabilir. Ancak DSVM 'yi bir işlem hedefi olarak kullanmayı planlıyorsanız yalnızca Ubuntu desteklenir.

DSVM 'yi bir geliştirme ortamı olarak kullanmak için aşağıdakileri yapın:

1. Aşağıdaki ortamların birinde bir DSVM oluşturun:

    * Azure portal:

        * [Ubuntu Veri Bilimi Sanal Makinesi oluşturma](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)

        * [Windows Veri Bilimi Sanal Makinesi oluşturma](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm)

    * Azure CLı:

        > [!IMPORTANT]
        > * Azure CLI 'yı kullandığınızda, önce `az login` komutunu kullanarak Azure aboneliğinizde oturum açmanız gerekir.
        >
        > * Bu adımdaki komutları kullandığınızda, bir kaynak grubu adı, VM için bir ad, Kullanıcı adı ve parola sağlamanız gerekir.

        * Ubuntu Veri Bilimi Sanal Makinesi oluşturmak için aşağıdaki komutu kullanın:

            ```azurecli
            # create a Ubuntu DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            # If you need to create a new resource group use: "az group create --name YOUR-RESOURCE-GROUP-NAME --location YOUR-REGION (For example: westus2)"
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:linux-data-science-vm-ubuntu:linuxdsvmubuntu:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --generate-ssh-keys --authentication-type password
            ```

        * Bir Windows Veri Bilimi Sanal Makinesi oluşturmak için aşağıdaki komutu kullanın:

            ```azurecli
            # create a Windows Server 2016 DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:dsvm-windows:server-2016:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --authentication-type password
            ```

2. Azure Machine Learning SDK DSVM 'de zaten yüklü. SDK'sını içeren Conda ortama kullanmak için aşağıdaki komutlardan birini kullanın:

    * Ubuntu DSVM için:

        ```shell
        conda activate py36
        ```

    * Windows DSVM için:

        ```shell
        conda activate AzureML
        ```

1. SDK'sı erişmek ve sürümü denetlemek doğrulamak için aşağıdaki Python kodu kullanın:

    ```python
    import azureml.core
    print(azureml.core.VERSION)
    ```

1. DSVM 'yi Azure Machine Learning hizmeti çalışma alanınızı kullanacak şekilde yapılandırmak için, [çalışma alanı yapılandırma dosyası oluşturma](#workspace) bölümüne bakın.

Daha fazla bilgi için bkz. [veri bilimi sanal makineleri](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a id="local"></a>Yerel bilgisayar

Yerel bir bilgisayar (Ayrıca, uzak bir sanal makine de olabilir) kullandığınızda, bir Anaconda ortamı oluşturun ve aşağıdakileri yaparak SDK 'Yı kurun:

1. Henüz yoksa [Anaconda](https://www.anaconda.com/distribution/#download-section) (Python 3,7 sürümü) sürümünü indirip yükleyin.

1. Bir Anaconda istemi açın ve aşağıdaki komutları kullanarak bir ortam oluşturun:

    Ortamı oluşturmak için aşağıdaki komutu çalıştırın.

    ```shell
    conda create -n myenv python=3.6.5
    ```

    Ardından ortamı etkinleştirin.

    ```shell
    conda activate myenv
    ```

    Bu örnek, Python 3.6.5 kullanarak bir ortam oluşturur, ancak herhangi bir belirli alt sürüm seçilebilir. SDK uyumluluğu belirli ana sürümlerle garanti edilemez (3,5 + önerilir) ve hatalar halinde çalıştırırsanız, Anaconda ortamınızda farklı bir sürüm/alt sürüm denemek önerilir. Bileşenler ve paketler indirilirken ortamın oluşturulması birkaç dakika sürer.

1. Ortama özel IPython kernels özelliğini etkinleştirmek için yeni ortamınızda aşağıdaki komutları çalıştırın. Bu, Anaconda ortamları içinde Jupyter Not defterleri ile çalışırken beklenen çekirdek ve paket içeri aktarma davranışının sağlanması gerekir:

    ```shell
    conda install notebook ipykernel
    ```

    Ardından, aşağıdaki komutu çalıştırarak çekirdeği oluşturun:

    ```shell
    ipython kernel install --user
    ```

1. Paketleri yüklemek için aşağıdaki komutları kullanın:

    Bu komut, temel Azure Machine Learning SDK 'sını Not defteri ve oto ek ekstralarla birlikte kurar. `automl` Ek büyük bir yüklemedir ve otomatik makine öğrenimi denemeleri çalıştırmayı düşünmüyorsanız ayraçlardan kaldırılabilir. Ek `automl` Ayrıca, bağımlılık olarak varsayılan olarak Azure Machine Learning veri hazırlama SDK 'sını içerir.

     ```shell
    pip install azureml-sdk[notebooks,automl]
    ```

   > [!NOTE]
   > PyYAML 'nin kaldırılamayacağı bir ileti alırsanız, bunun yerine aşağıdaki komutu kullanın:
   >
   > `pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML`

   SDK 'nın yüklenmesi birkaç dakika sürer. Yükleme seçenekleri hakkında daha fazla bilgi için bkz. [Yükleme Kılavuzu](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) .

1. Machine Learning deneme için diğer paketleri yükler.

    Aşağıdaki komutlardan birini kullanın ve  *\<yeni paketi >* yüklemek istediğiniz paket ile değiştirin. Aracılığıyla `conda install` paket yükleme, paketin geçerli kanalların bir parçası olmasını gerektirir (yeni kanallar Anaconda buluta eklenebilir).

    ```shell
    conda install <new package>
    ```

    Alternatif olarak, paketini aracılığıyla `pip`yükleyebilirsiniz.

    ```shell
    pip install <new package>
    ```

### <a id="jupyter"></a>Jupyter Not Defterleri

Jupyter not defterleri parçası olan [Jupyter proje](https://jupyter.org/). Bunlar, Canlı kod anlatım metin ve grafikleri ile karışık belgeleri oluşturmak burada etkileşimli bir kodlama deneyimi sunar. Jupi Not defterleri, kod bölümlerinin çıktısını belgeye kaydedebilmeniz için sonuçlarınızı başkalarıyla paylaşmanın harika bir yoludur. Jupyter not defterleri çeşitli platformlarda yükleyebilirsiniz.

[Yerel bilgisayar](#local) bölümündeki yordam, Jupyıter not defterlerini bir Anaconda ortamında çalıştırmak için gerekli bileşenleri yüklüyor. Jupyter Notebook ortamınızda bu bileşenleri etkinleştirmek için aşağıdakileri yapın:

1. Bir Anaconda istemi açın ve ortamınızı etkinleştirin.

    ```shell
    conda activate myenv
    ```
    
1. Örnek bir not defteri kümesi için [GitHub deposunu](https://aka.ms/aml-notebooks) kopyalayın.

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Aşağıdaki komutla Jupyter Notebook sunucusunu başlatın:

    ```shell
    jupyter notebook
    ```

1. Jupyter Notebook SDK 'Yı kullanıp kullanbildiğini doğrulamak için **Yeni** bir not defteri oluşturun, çekirdekte **Python 3** ' ü seçin ve ardından bir not defteri hücresinde aşağıdaki komutu çalıştırın:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Modülleri içeri aktarma ve alma `ModuleNotFoundError`sorunlarıyla karşılaşırsanız, bir not defteri hücresinde aşağıdaki kodu çalıştırarak Jupyter çekirdeğin ortamınız için doğru yola bağlı olduğundan emin olun.

    ```python
    import sys
    sys.path
    ```
    
1. Jupyter Notebook Azure Machine Learning hizmeti çalışma alanınızı kullanacak şekilde yapılandırmak için, [çalışma alanı yapılandırma dosyası oluşturma](#workspace) bölümüne gidin.


### <a id="vscode"></a>Visual Studio kodu

Visual Studio Code, platformlar arası kod düzenleyicisidir. Python desteği için yerel bir Python 3 ve Conda yükleme kullanır, ancak yapay ZEKA ile çalışmak için ek araçlar sağlar. Kod Düzenleyicisi içinde Conda ortamından seçmek için desteği de sağlar.

Geliştirme için Visual Studio Code kullanmak için şunları yapın:

1. Python geliştirmesi için Visual Studio Code kullanmayı öğrenmek için bkz. [VSCode 'Da Python ile çalışmaya başlama](https://code.visualstudio.com/docs/python/python-tutorial).

1. Conda ortamını seçmek için VS Code açın ve ardından CTRL + SHIFT + P (Linux ve Windows) veya komut + SHIFT + P (Mac) öğesini seçin.
    __Komut paleti__ açılır.

1. Python __girin: Yorumlayıcı__' yı seçin ve ardından Conda ortamını seçin.

1. SDK 'yı kullanacağınızı doğrulamak için, aşağıdaki kodu içeren yeni bir Python dosyası (. Kopyala) oluşturun ve çalıştırın:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Visual Studio Code için Azure Machine Learning uzantısını yüklemek için bkz. [AI araçları](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).

    Daha fazla bilgi için bkz. [Visual Studio Code için Azure Machine Learning kullanma](how-to-vscode-tools.md).

<a name="aml-databricks"></a>

## <a name="azure-databricks"></a>Azure Databricks
Azure Databricks, Azure bulutu 'nda Apache Spark tabanlı bir ortamdır. CPU veya GPU tabanlı işlem kümesi ile birlikte çalışan bir not defteri tabanlı ortam sağlar.

Azure Databricks Azure Machine Learning hizmeti ile nasıl kullanılır:
+ Spark MLlib kullanarak bir modeli eğitebilirsiniz ve modeli Azure Databricks içinden ACG/AKS 'e dağıtabilirsiniz.
+ Ayrıca, Azure Databricks ile özel bir Azure ML SDK 'sında [otomatik makine öğrenimi](concept-automated-ml.md) özellikleri de kullanabilirsiniz.
+ Azure Databricks, bir [Azure Machine Learning](concept-ml-pipelines.md)işlem hattından işlem hedefi olarak kullanabilirsiniz.

### <a name="set-up-your-databricks-cluster"></a>Databricks kümenizi ayarlama

[Databricks kümesi](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal)oluşturun. Bazı ayarlar yalnızca Databricks 'te otomatik makine öğrenimi için SDK 'Yı yüklerseniz geçerlidir.
**Kümenin oluşturulması birkaç dakika sürer.**

Şu ayarları kullanın:

| Ayar |Şunlara uygulanacaktır:| Value |
|----|---|---|
| Küme adı |her zaman| yourclustername |
| Databricks Çalışma Zamanı |her zaman| ML olmayan tüm çalışma zamanı (ML 4. x, 5. x) |
| Python sürümü |her zaman| 3 |
| Çalışanlar |her zaman| 2 veya üzeri |
| Çalışan düğümü VM türleri <br>(en fazla eşzamanlı yineleme sayısını belirler) |Otomatikleştirilmiş ML<br>yalnızca| Bellek için iyileştirilmiş VM tercih edilen |
| Otomatik Ölçeklendirmeyi Etkinleştirme |Otomatikleştirilmiş ML<br>yalnızca| Kutunun |

Devam etmeden önce küme çalışmaya kadar bekleyin.

### <a name="install-the-correct-sdk-into-a-databricks-library"></a>Databricks kitaplığına doğru SDK 'Yı yükler
Küme çalışmaya başladıktan sonra uygun Azure Machine Learning SDK paketini kümenize eklemek için [bir kitaplık oluşturun](https://docs.databricks.com/user-guide/libraries.html#create-a-library) .

1. **Yalnızca bir** seçenek belirleyin (diğer SDK yüklemesi desteklenmez)

   |SDK&nbsp;paketi&nbsp;ek özellikleri|Source|Pypı&nbsp;adı&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
   |----|---|---|
   |Databricks için| Python Yumurg veya Pypı yükleme | azureml-SDK [databricks]|
   |Databricks için-ile<br> Otomatik ML özellikleri| Python Yumurg veya Pypı yükleme | azureml-SDK [automl_databricks]|

   > [!Warning]
   > Başka SDK ek özellikleri yüklenemez. Yukarıdaki seçeneklerden yalnızca birini seçin [databricks] veya [automl_databricks].

   * **Tüm kümelere otomatik olarak ekle**' yi seçmeyin.
   * Küme adınızın yanındaki **Ekle** ' yi seçin.

1. Durum Iliştirilene kadar, bu işlembirkaç dakika sürebilir.  Bu adım başarısız olursa, aşağıdakileri denetleyin:

   Kümenizi şu şekilde yeniden başlatmayı deneyin:
   1. Sol bölmede **kümeler**' ı seçin.
   1. Tabloda, küme adınızı seçin.
   1. **Kitaplıklar** sekmesinde **Yeniden Başlat**' ı seçin.

   Ayrıca şunları göz önünde bulundurun:
   + Azure Databricks kullanırken, oto ml yapılandırmasında, lütfen aşağıdaki parametreleri ekleyin:
       1. ```max_concurrent_iterations```, kümenizdeki çalışan düğümlerinin sayısına bağlıdır.
        2. ```spark_context=sc```, varsayılan Spark bağlamını temel alır.
   + Ya da eski bir SDK sürümünüz varsa, kümenin yüklü olan kitaplıkların seçimini kaldırın ve çöp kutusuna geçiş yapın. Yeni SDK sürümünü yükleyip kümeyi yeniden başlatın. Bundan sonra bir sorun varsa, kümenizi ayırın ve yeniden bağlayın.

Yüklemesi başarılı olduysa, içeri aktarılan kitaplık aşağıdakilerden biri gibi görünmelidir:

Databricks için otomatik **** makine öğrenimi ![Azure Machine Learning SDK 'sı olmadan databricks için SDK](./media/how-to-configure-environment/amlsdk-withoutautoml.jpg)

Databricks 'te otomatik makine öğrenimi ile ![otomatik makine öğrenimi SDK 'sı ile databricks için SDK](./media/how-to-configure-environment/automlonadb.jpg)

### <a name="start-exploring"></a>Keşfetmeye başlayın

Deneyin:
+ Azure Databricks/Azure Machine Learning SDK için [Not defteri arşiv dosyasını](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks/Databricks_AMLSDK_1-4_6.dbc) indirin ve [Arşiv dosyasını](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-an-archive) databricks kümenize içeri aktarın.
  Birçok örnek Not defteri kullanılabilir, **ancak [Bu örnek Not defterleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) yalnızca Azure Databricks çalışır.**

+ [Eğitim işlemi olarak Databricks ile bir işlem hattı oluşturmayı](how-to-create-your-first-pipeline.md)öğrenin.

## <a id="aznotebooks"></a>Azure Not Defterleri

[Azure not defterleri](https://notebooks.azure.com) (Önizleme) olan Azure bulutundaki bir etkileşimli bir geliştirme ortamı. Azure Machine Learning geliştirmeyi kullanmaya başlamak için kolay bir yoldur.

* Azure Machine Learning SDK zaten yüklü.
* Azure portal bir Azure Machine Learning hizmet çalışma alanı oluşturduktan sonra, Azure Not defteri ortamınızı çalışma alanıyla çalışacak şekilde otomatik olarak yapılandırmak için bir düğmeye tıklayabilirsiniz.

Azure Notebooks kullanmaya başlamak için [Azure Portal](https://portal.azure.com) kullanın.  Çalışma alanınızı açın ve **genel bakış** bölümünden **Azure Notebooks kullanmaya başlayın**' ı seçin.

Azure Notebooks, varsayılan olarak, 4 GB bellek ve 1 GB veri ile sınırlı bir ücretsiz hizmet katmanını kullanır. Ancak, Azure Notebooks projesine bir Veri Bilimi Sanal Makinesi örneği ekleyerek bu limitleri kaldırabilirsiniz. Daha fazla bilgi için bkz. [Azure Notebooks projelerini yönetme ve yapılandırma-işlem katmanı](/azure/notebooks/configure-manage-azure-notebooks-projects#compute-tier).

## <a id="workspace"></a>Çalışma alanı yapılandırma dosyası oluşturma

Çalışma alanı yapılandırma dosyası, SDK 'nın Azure Machine Learning hizmet çalışma alanıyla nasıl iletişim kuracağını söyleyen bir JSON dosyasıdır. Dosya *config. JSON*olarak adlandırılır ve aşağıdaki biçimdedir:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Bu JSON dosyası, Python betikleri veya Jupyıter not defterlerini içeren dizin yapısında olmalıdır. Aynı dizinde, *. azureml*adlı bir alt dizin veya bir üst dizin içinde olabilir.

Bu dosyadan kodunuzu kullanmak için `ws=Workspace.from_config()`. Bu kod, bilgileri bir dosyadan yükler ve çalışma alanınıza bağlanır.

Yapılandırma dosyasını üç şekilde oluşturabilirsiniz:

* **[Azure Machine Learning hizmet çalışma alanı oluşturma](setup-create-workspace.md#sdk)bölümündeki adımları izleyin**: Azure Notebooks kitaplığınızda bir *config. JSON* dosyası oluşturulur. Dosya, çalışma alanınızın yapılandırma bilgilerini içerir. *Config. json dosyasını* diğer geliştirme ortamlarına indirebilir veya kopyalayabilirsiniz.

* **Dosyayı indirin**: [Azure Portal](https://ms.portal.azure.com), çalışma alanınızın **genel bakış** bölümünden **config. json dosyasını indir** ' i seçin.

     ![Azure portal](./media/how-to-configure-environment/configure.png)

* **Dosyayı program aracılığıyla oluşturun**: Aşağıdaki kod parçacığında, abonelik KIMLIĞI, kaynak grubu ve çalışma alanı adı sağlayarak bir çalışma alanına bağlanırsınız. Daha sonra çalışma alanı yapılandırmasını dosyaya kaydeder:

    ```python
    from azureml.core import Workspace

    subscription_id = '<subscription-id>'
    resource_group  = '<resource-group>'
    workspace_name  = '<workspace-name>'

    try:
        ws = Workspace(subscription_id = subscription_id, resource_group = resource_group, workspace_name = workspace_name)
        ws.write_config()
        print('Library configuration succeeded')
    except:
        print('Workspace not found')
    ```

    Bu kod, yapılandırma dosyasını *. azureml/config. JSON* dosyasına yazar.


## <a name="next-steps"></a>Sonraki adımlar

- Azure Machine Learning model veri kümesiyle [bir modeli eğitme](tutorial-train-models-with-aml.md)
- Python başvurusu [için Azure Machine Learning SDK 'sını](https://aka.ms/aml-sdk) görüntüleme
- [Azure Machine Learning için veri hazırlığı paketi](https://aka.ms/data-prep-sdk) hakkında bilgi edinin
