---
title: Python geliştirme ortamı nı ayarlama
titleSuffix: Azure Machine Learning
description: Azure Machine Learning için geliştirme ortamınızı yapılandırmayı öğrenin. Conda ortamlarını kullanın, yapılandırma dosyaları oluşturun ve kendi bulut tabanlı dizüstü bilgisayar sunucunuzu, Jupyter Notebook'larınızı, Azure Veri Tuğlalarını, IDE'leri, kod düzenleyicilerini ve Veri Bilimi Sanal Makinesini yapılandırın.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.topic: conceptual
ms.date: 12/27/2019
ms.custom: seodec18
ms.openlocfilehash: 10ea0002b2e99c4675f56e48a638f3c1cb87e6c0
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399001"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Azure Machine Learning için bir geliştirme ortamı nı yapılandırma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, azure machine learning ile çalışacak bir geliştirme ortamını nasıl yapılandırabileceğinizi öğreneceksiniz. Azure Machine Learning platform agnostiktir. Geliştirme ortamınız için tek zor gereksinim Python 3'dür. Anaconda veya Virtualenv gibi izole bir ortam da önerilir.

Aşağıdaki tablo, bu makalede kapsanan her geliştirme ortamını, artıları ve eksileri ile birlikte gösterir.

| Ortam | Artıları | Simgeler |
| --- | --- | --- |
| [Bulut tabanlı Azure Machine Learning bilgi işlem örneği (önizleme)](#compute-instance) | Başlamak için en kolay yol. Tüm SDK zaten çalışma alanı VM yüklü ve dizüstü öğreticiler önceden klonlanmış ve çalışmaya hazır. | Geliştirme ortamınız ve bağımlılıklarınız üzerinde kontrol eksikliği. Linux VM için tahakkuk eden ek maliyet (ücretlendirmeyi önlemek için kullanılmadığında VM durdurulabilir). [Fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)bakın. |
| [Yerel çevre](#local) | Geliştirme ortamınızın ve bağımlılıklarınızın tam kontrolü. İstediğiniz herhangi bir yapı aracı, ortam veya IDE ile çalıştırın. | Başlamak daha uzun sürer. Gerekli SDK paketleri yüklenmeli ve zaten bir paketiniz yoksa bir ortam da yüklenmelidir. |
| [Azure Databricks](#aml-databricks) | Ölçeklenebilir Apache Spark platformunda büyük ölçekli yoğun makine öğrenimi iş akışlarını çalıştırmak için idealdir. | Deneysel makine öğrenimi veya daha küçük ölçekli deneyler ve iş akışları için overkill. Azure Databricks için ek maliyet tahakkuk etti. [Fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/databricks/)bakın. |
| [Veri Bilimi Sanal Makine (DSVM)](#dsvm) | Bulut tabanlı bilgi işlem örneğine benzer (Python ve SDK önceden yüklenmiştir), ancak ek popüler veri bilimi ve makine öğrenimi araçları önceden yüklenmiştir. Ölçeklendirmesi ve diğer özel araçlar ve iş akışlarıyla birleştirilmesi kolaydır. | Bulut tabanlı işlem örneğine kıyasla daha yavaş bir başlangıç deneyimi. |

Bu makalede, aşağıdaki araçlar için ek kullanım ipuçları da sağlar:

* [Jupyter Notebooks](#jupyter): Jupyter Notebook'u zaten kullanıyorsanız, SDK'da yüklemeniz gereken bazı ekstralar vardır.

* [Visual Studio Code](#vscode): Visual Studio Code kullanıyorsanız, [Azure Machine Learning uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai) Python için kapsamlı dil desteğinin yanı sıra Azure Machine Learning ile çalışmayı çok daha kullanışlı ve üretken hale getirmek için özellikler içerir.

## <a name="prerequisites"></a>Ön koşullar

Azure Machine Learning çalışma alanı. Çalışma alanı oluşturmak için [bkz.](how-to-manage-workspace.md) Kendi [bulut tabanlı dizüstü bilgisayar sunucunuz](#compute-instance), [DSVM](#dsvm)veya [Azure Databrick'leriniz](#aml-databricks)ile başlamak için ihtiyacınız olan tek şey çalışma alanıdır.

[Yerel bilgisayarınız,](#local) [Jupyter Notebook sunucusu](#jupyter) veya Visual [Studio Kodu](#vscode) için SDK ortamını yüklemek için aşağıdakilere de ihtiyacınız vardır:

- [Anaconda](https://www.anaconda.com/download/) ya da [Miniconda](https://conda.io/miniconda.html) paket yöneticisi.

- Linux veya macOS'ta, bash kabuğuna ihtiyacınız var.

    > [!TIP]
    > Linux veya macOS kullanıyorsanız ve bash dışında bir kabuk kullanıyorsanız (örneğin, zsh) bazı komutları çalıştırdığınızda hata lar alabilirsiniz. Bu sorunu aşmak için, `bash` yeni bir bash kabuğu başlatmak ve komutları çalıştırmak için komutu kullanın.

- Windows'da komut istemi veya Anaconda istemi (Anaconda ve Miniconda tarafından yüklenmiş) gerekir.

## <a name="your-own-cloud-based-compute-instance"></a><a id="compute-instance"></a>Kendi bulut tabanlı işlem örneğiniz

Azure Machine Learning [bilgi işlem örneği (önizleme),](concept-compute-instance.md) veri bilimcilere Jupyter dizüstü bilgisayar sunucusu, JupyterLab ve tamamen hazırlanmış bir ML ortamı sağlayan güvenli, bulut tabanlı bir Azure iş istasyonudur.

Bir bilgi işlem örneği için yükleyecek veya yapılandırılacak hiçbir şey yok.  Azure Machine Learning çalışma alanınızdan her zaman bir tane oluşturun. Sadece bir ad sağlayın ve bir Azure VM türü belirtin. Şimdi bu Öğretici ile [deneyin: Kurulum ortamı ve çalışma alanı.](tutorial-1st-experiment-sdk-setup.md)

[Bilgi işlem örnekleri](concept-compute-instance.md)hakkında daha fazla bilgi edinin.

İşlem ücretlerine neden olmayı durdurmak [için, işlem örneğini durdurun.](tutorial-1st-experiment-sdk-train.md#clean-up-resources)

## <a name="data-science-virtual-machine"></a><a id="dsvm"></a>Veri Bilimi Sanal Makinesi

DSVM özelleştirilmiş bir sanal makine (VM) görüntüdür. Önceden yapılandırılan veri bilimi çalışmaları için tasarlanmıştır:

  - TensorFlow, PyTorch, Scikit-learn, XGBoost ve Azure Machine Learning SDK gibi paketler
  - Spark Standalone ve Drill gibi popüler veri bilimi araçları
  - Azure CLI, AzCopy ve Depolama Gezgini gibi Azure araçları
  - Visual Studio Code ve PyCharm gibi entegre geliştirme ortamları (IME'ler)
  - Jupyter Notebook Sunucusu

Azure Machine Learning SDK, DSVM'nin Ubuntu veya Windows sürümünde çalışır. Ancak DSVM'yi de bir hesaplama hedefi olarak kullanmayı planlıyorsanız, yalnızca Ubuntu desteklenir.

DSVM'yi geliştirme ortamı olarak kullanmak için:

1. Aşağıdaki ortamlardan herhangi birinde bir DSVM oluşturun:

    * Azure portalı:

        * [Ubuntu Veri Bilimi Sanal Makinesi oluşturma](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)

        * [Windows Veri Bilimi Sanal Makinesi oluşturma](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm)

    * The Azure CLI:

        > [!IMPORTANT]
        > * Azure CLI'yi kullandığınızda, önce `az login` komutu kullanarak Azure aboneliğinizde oturum açmanız gerekir.
        >
        > * Bu adımda komutları kullandığınızda, bir kaynak grubu adı, VM için bir ad, bir kullanıcı adı ve bir parola sağlamanız gerekir.

        * Ubuntu Veri Bilimi Sanal Makinesi oluşturmak için aşağıdaki komutu kullanın:

            ```azurecli-interactive
            # create a Ubuntu DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            # If you need to create a new resource group use: "az group create --name YOUR-RESOURCE-GROUP-NAME --location YOUR-REGION (For example: westus2)"
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:linux-data-science-vm-ubuntu:linuxdsvmubuntu:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --generate-ssh-keys --authentication-type password
            ```

        * Bir Windows Veri Bilimi Sanal Makinesi oluşturmak için aşağıdaki komutu kullanın:

            ```azurecli-interactive
            # create a Windows Server 2016 DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:dsvm-windows:server-2016:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --authentication-type password
            ```

2. Azure Machine Learning SDK zaten DSVM'ye yüklendi. SDK içeren Conda ortamını kullanmak için aşağıdaki komutlardan birini kullanın:

    * Ubuntu DSVM için:

        ```bash
        conda activate py36
        ```

    * Windows DSVM için:

        ```bash
        conda activate AzureML
        ```

1. SDK'ya erişip sürümü kontrol edebileceğinizi doğrulamak için aşağıdaki Python kodunu kullanın:

    ```python
    import azureml.core
    print(azureml.core.VERSION)
    ```

1. DSVM'yi Azure Machine Learning çalışma alanınızı kullanacak şekilde yapılandırmak için [bir çalışma alanı yapılandırma dosyası oluştur](#workspace) bölümüne bakın.

Daha fazla bilgi için [Bkz. Veri Bilimi Sanal Makineler.](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)

## <a name="local-computer"></a><a id="local"></a>Yerel bilgisayar

Yerel bir bilgisayar kullanıyorsanız (uzak bir sanal makine de olabilir), bir Anaconda ortamı oluşturun ve SDK'yı yükleyin. Bir örneği aşağıda verilmiştir:

1. [Anaconda'yı](https://www.anaconda.com/distribution/#download-section) (Python 3.7 sürümü) indirin ve yükleyin.

1. Bir Anaconda istemi açın ve aşağıdaki komutları içeren bir ortam oluşturun:

    Ortamı oluşturmak için aşağıdaki komutu çalıştırın.

    ```bash
    conda create -n myenv python=3.6.5
    ```

    Sonra ortamı etkinleştirin.

    ```bash
    conda activate myenv
    ```

    Bu örnek, python 3.6.5 kullanarak bir ortam oluşturur, ancak belirli alt versiyonları seçilebilir. SDK uyumluluğu belirli ana sürümlerle garanti edilemeyebilir (3,5+ önerilir) ve hatalarla karşılaştığınızda Anaconda ortamınızda farklı bir sürüm/alt sürüm denemeniz önerilir. Bileşenler ve paketler karşıdan yüklenirken ortamı oluşturmak birkaç dakika sürer.

1. Ortama özgü IPython çekirdeklerini etkinleştirmek için yeni ortamınızda aşağıdaki komutları çalıştırın. Bu, Anaconda ortamlarında Jupyter Notebook'larla çalışırken beklenen çekirdek ve paket alma davranışı sağlayacaktır:

    ```bash
    conda install notebook ipykernel
    ```

    Ardından çekirdeği oluşturmak için aşağıdaki komutu çalıştırın:

    ```bash
    ipython kernel install --user --name myenv --display-name "Python (myenv)"
    ```

1. Paketleri yüklemek için aşağıdaki komutları kullanın:

    Bu komut, azure machine learning SDK `automl` tabanını dizüstü bilgisayar ve ekstralarla birlikte yükler. Ekstra `automl` büyük bir yüklemedir ve otomatik makine öğrenme denemeleri çalıştırmak niyetinde değilseniz parantez kaldırılabilir. Ekstra `automl` da varsayılan olarak bir bağımlılık olarak Azure Machine Learning Data Prep SDK içerir.

    ```bash
    pip install azureml-sdk[notebooks,automl]
    ```

   > [!NOTE]
   > * PyYAML'nin kaldırılamayacağına dair bir ileti alırsanız, bunun yerine aşağıdaki komutu kullanın:
   >
   >   `pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML`
   >
   > * macOS Catalina ile başlayarak, zsh (Z kabuk) varsayılan giriş kabuğu ve etkileşimli kabuk. zsh'de , " (\\backslash) ile parantezlerden kaçan aşağıdaki komutu kullanın:
   >
   >   `pip install --upgrade azureml-sdk\[notebooks,automl\]`

   SDK'nın yüklenmesi birkaç dakika sürer. Yükleme seçenekleri hakkında daha fazla bilgi için [yükleme kılavuzuna](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)bakın.

1. Makine öğrenimi denemeniz için başka paketler yükleyin.

    Aşağıdaki komutlardan birini kullanın ve * \<yeni paket>* yüklemek istediğiniz paketle değiştirin. Paketleri n `conda install` için yüklemek, paketin geçerli kanalların bir parçası olmasını gerektirir (Anaconda Cloud'a yeni kanallar eklenebilir).

    ```bash
    conda install <new package>
    ```

    Alternatif olarak, paketleri `pip`.

    ```bash
    pip install <new package>
    ```

### <a name="jupyter-notebooks"></a><a id="jupyter"></a>Jupyter Notebooks

Jupyter Notebook'lar [Jupyter Projesi'nin](https://jupyter.org/)bir parçasıdır. Bunlar, canlı kodu anlatı metni ve grafiklerle harmanlayan belgeler oluşturduğunuz etkileşimli bir kodlama deneyimi sağlar. Jupyter Notebook'lar da sonuçlarınızı başkalarıyla paylaşmak için harika bir yoldur, çünkü belgedeki kod bölümlerinizin çıktısını kaydedebilirsiniz. Jupyter Notebook'ları çeşitli platformlarda yükleyebilirsiniz.

[Yerel bilgisayar](#local) bölümündeki yordam, Jupyter Notebook'ları Anaconda ortamında çalıştırmak için gerekli bileşenleri yükler.

Jupyter Notebook ortamınızda bu bileşenleri etkinleştirmek için:

1. Bir Anaconda istemi açın ve ortamınızı etkinleştirin.

    ```bash
    conda activate myenv
    ```

1. Örnek dizüstü bilgisayarlar kümesi için [GitHub deposunu](https://aka.ms/aml-notebooks) klonla.

    ```bash
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Jupyter Notebook sunucusunu aşağıdaki komutla başlatın:

    ```bash
    jupyter notebook
    ```

1. Jupyter Notebook'un SDK'yı kullanabileceğini doğrulamak için **Yeni** bir not defteri oluşturun, çekirdeğiniz olarak **Python 3'ü** seçin ve ardından bir not defteri hücresinde aşağıdaki komutu çalıştırın:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Modülleri içe aktarma sorunlarıyla karşılaşırsanız ve bir `ModuleNotFoundError`, Jupyter çekirdeğinizin bir Not Defteri hücresinde aşağıdaki kodu çalıştırarak ortamınız için doğru yola bağlı olduğundan emin olun.

    ```python
    import sys
    sys.path
    ```

1. Jupyter Notebook'u Azure Machine Learning çalışma alanınızı kullanacak şekilde yapılandırmak için [bir çalışma alanı yapılandırma dosyası oluştur](#workspace) bölümüne gidin.

### <a name="visual-studio-code"></a><a id="vscode"></a>Visual Studio Code

Visual Studio Code, Visual Studio pazarda bulunan uzantılar aracılığıyla geniş bir programlama dili ve aracı kümesini destekleyen çok popüler bir çapraz platform kodu [düzenleyicisidir.](https://marketplace.visualstudio.com/vscode) [Azure Machine Learning uzantısı,](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai) python [eklentisini](https://marketplace.visualstudio.com/items?itemName=ms-python.python) tüm Python ortamlarında (sanal, Anaconda, vb.) kodlamak için yükler. Ayrıca, Azure Machine Learning kaynaklarıyla çalışmak ve Visual Studio Code'dan ayrılmadan Azure Machine Learning denemelerini çalıştırmak için kolaylık özellikleri sağlar.

Geliştirme için Visual Studio Kodunu kullanmak için:

1. Visual Studio Kodu için Azure Machine [Learning](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai)uzantısını yükleyin, bkz.

    Daha fazla bilgi için [bkz.](tutorial-setup-vscode-extension.md)

1. Python geliştirme herhangi bir tür için Visual Studio Kodu nasıl kullanılacağını öğrenin, [VSCode Python ile başlayın](https://code.visualstudio.com/docs/python/python-tutorial)bakın.

    - SDK içeren SDK Python ortamını seçmek için VS Kodu'nu açın ve ardından Ctrl+Shift+P (Linux ve Windows) veya Command+Shift+P (Mac) seçeneğini belirleyin.
        - __Komut Paleti__ açılır.

    - __Python girin: Yorumlayıcı seçin__ve sonra uygun ortamı seçin

1. SDK'yı kullanabileceğinizi doğrulamak için, aşağıdaki kodu içeren yeni bir Python dosyası (.py) oluşturun:

    ```python
    #%%
    import azureml.core
    azureml.core.VERSION
    ```
    "Çalıştır hücre" CodeLens'i tıklatarak bu kodu çalıştırın veya shift-enter tuşuna basın.
<a name="aml-databricks"></a>

## <a name="azure-databricks"></a>Azure Databricks
Azure Databricks, Azure bulutundaki Apache Spark tabanlı bir ortamdır. CPU veya GPU tabanlı bilgi işlem kümesi ile ortak bir Not Defteri tabanlı ortam sağlar.

Azure Databricks, Azure Machine Learning ile nasıl çalışır:
+ Spark MLlib kullanarak bir model eğitebilir ve modeli Azure Databricks'in içinden ACI/AKS'ye dağıtabilirsiniz.
+ Azure Databricks ile özel bir Azure ML SDK'da [otomatik makine öğrenimi](concept-automated-ml.md) özelliklerini de kullanabilirsiniz.
+ Azure Veri Tuğlaları'nı bir Azure Machine [Learning ardışık ardışık kaynaktan](concept-ml-pipelines.md)bilgi işlem hedefi olarak kullanabilirsiniz.

### <a name="set-up-your-databricks-cluster"></a>Databricks kümenizi ayarlama

[Databricks kümesi](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal)oluşturun. Bazı ayarlar yalnızca Databricks'e otomatik makine öğrenimi için SDK'yı yüklerseniz geçerlidir.
**Kümeoluşturmak birkaç dakika sürer.**

Şu ayarları kullanın:

| Ayar |Uygulandığı öğe:| Değer |
|----|---|---|
| Küme adı |Her zaman| sizin clustername |
| Databricks Çalışma Zamanı |Her zaman|Non-ML Çalışma Süresi 6.5 (scala 2.11, kıvılcım 2.4.3) |
| Python sürümü |Her zaman| 3 |
| Işçi |Her zaman| 2 veya daha yüksek |
| İşçi düğümü VM türleri <br>(eşzamanlı yinelemelerin maksimum # #'ını belirler) |Otomatikleştirilmiş ML<br>Sadece| Bellek optimize VM tercih |
| Otomatik ölçekletme etkinleştirme |Otomatikleştirilmiş ML<br>Sadece| Işaret -ini kaldırın |

Daha fazla ilerlemeden önce küme nin çalışmasını bekleyin.

### <a name="install-the-correct-sdk-into-a-databricks-library"></a>Doğru SDK'yı Databricks kitaplığına yükleme
Küme çalışmaya başladıktan sonra, kümenize uygun Azure Machine Learning SDK paketini eklemek için [bir kitaplık oluşturun.](https://docs.databricks.com/user-guide/libraries.html#create-a-library)

1. Kitaplığı depolamak istediğiniz geçerli Çalışma Alanı klasörünü sağ tıklatın. **Kitaplık** **Oluştur'u** > seçin.

1. **Yalnızca bir** seçenek seçin (başka SDK yüklemesi desteklenmez)

   |SDK&nbsp;&nbsp;paket ekstralar|Kaynak|PyPi&nbsp;Adı&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
   |----|---|---|
   |Databricks için| Python Yumurta veya PyPI yükle | azureml-sdk[databricks]|
   |Databricks için -ile-<br> otomatik ML yetenekleri| Python Yumurta veya PyPI yükle | azureml-sdk[automl]|

   > [!Warning]
   > Başka SDK ekstrası yüklenemez. Önceki seçeneklerden yalnızca birini [databricks] veya [automl] seçin.

   * **Tüm kümelere otomatik olarak ekle'yi**seçmeyin.
   * Küme adınızın yanına **Ekle'yi** seçin.

1. Birkaç dakika sürebilir **Ekli**durum değişiklikleri kadar hataları için izleyin.  Bu adım başarısız olursa:

   Kümenizi şu şekilde yeniden başlatmayı deneyin:
   1. Sol bölmede **Kümeler'i**seçin.
   1. Tabloda küme adınızı seçin.
   1. **Kitaplıklar** sekmesinde **Yeniden Başlat'ı**seçin.

   Ayrıca göz önünde bulundurun:
   + AutoML config'inde, Azure Databricks kullanırken aşağıdaki parametreleri ekleyin:
       1. ```max_concurrent_iterations```kümenizdeki işçi düğümlerinin sayısını temel alıntır.
        2. ```spark_context=sc```varsayılan kıvılcım bağlamını temel alır.
   + Veya eski bir SDK sürümünüz varsa, kümenin yüklü libs'inden seçin ve çöp kutusuna geçin. Yeni SDK sürümünü yükleyin ve kümeyi yeniden başlatın. Yeniden başlatmadan sonra bir sorun varsa, kümenizi ayırın ve yeniden bağlayınız.

Yükleme başarılı olduysa, alınan kitaplık aşağıdakilerden biri gibi görünmelidir:

Otomatik makine öğrenme ![ **_olmadan_** Databricks için SDK Veri tuğlaları için Azure Machine Learning SDK](./media/how-to-configure-environment/amlsdk-withoutautoml.jpg)

SDK Databricks **WITH** yüklü otomatik ![makine öğrenme sdk ile otomatik makine öğrenme SDK ile Databricks için](./media/how-to-configure-environment/automlonadb.png)

### <a name="start-exploring"></a>Keşfetmeye başlayın

Deneyin:
+ Birçok örnek not defteri olsa da, **yalnızca bu örnek not [defterleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) Azure Databricks ile çalışır.**

+ Bu örnekleri doğrudan çalışma alanınızdan aktarın. Aşağıya bakın: ![](./media/how-to-configure-environment/azure-db-screenshot.png)
![İthalat Panelini seçin](./media/how-to-configure-environment/azure-db-import.png)

+ [Eğitim bilgini olarak Databricks ile nasıl bir boru hattı oluşturup oluşturabilirsiniz](how-to-create-your-first-pipeline.md)öğrenin.

## <a name="create-a-workspace-configuration-file"></a><a id="workspace"></a>Çalışma alanı yapılandırma dosyası oluşturma

Çalışma alanı yapılandırma dosyası, SDK'ya Azure Machine Learning çalışma alanınızla nasıl iletişim kurarak iletişim kurmasını söyleyen bir JSON dosyasıdır. Dosya *config.json*olarak adlandırılır ve aşağıdaki biçimi vardır:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Bu JSON dosyası Python komut dosyalarınızı veya Jupyter Not Defterlerinizi içeren dizin yapısında olmalıdır. Aynı dizinde, *.azureml*adlı bir alt dizinde veya bir üst dizinde olabilir.

Bu dosyayı kodunuzdakullanmak `ws=Workspace.from_config()`için . Bu kod, dosyadaki bilgileri yükler ve çalışma alanınıza bağlanır.

Yapılandırma dosyasını üç şekilde oluşturabilirsiniz:

* ** [ws.write_config](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)kullanın**: *config.json* dosyası yazmak için. Dosya, çalışma alanınız için yapılandırma bilgilerini içerir. *Config.json'u* diğer geliştirme ortamlarına indirebilir veya kopyalayabilirsiniz.

* **Dosyayı indirin**: [Azure portalında](https://ms.portal.azure.com)çalışma alanınızın **Genel Bakış** bölümünden **config.json indir'i** seçin.

     ![Azure portal](./media/how-to-configure-environment/configure.png)

* **Dosyayı programlı olarak oluşturun**: Aşağıdaki kod snippet'inde, abonelik kimliği, kaynak grubu ve çalışma alanı adını sağlayarak bir çalışma alanına bağlanırsınız. Daha sonra çalışma alanı yapılandırmasını dosyaya kaydeder:

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

    Bu kod yapılandırma dosyasını *.azureml/config.json* dosyasına yazar.

## <a name="next-steps"></a>Sonraki adımlar

- MNIST veri seti ile Azure Machine Learning hakkında [bir model eğitin](tutorial-train-models-with-aml.md)
- Python başvurusu [için Azure Machine Learning SDK'yı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) görüntüleyin
