---
title: Azure Percept gelişmiş geliştirmeyi yerel olarak kullanmaya başlama
description: AzureML VS Code + jupi not defterlerini kullanarak yerel makine öğrenimi geliştirmeye başlama
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: tutorial
ms.date: 02/10/2021
ms.custom: template-tutorial
ms.openlocfilehash: 88e71ac21177a13d30176212e97442c63272eca6
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101665111"
---
# <a name="getting-started-with-machine-learning-development-using-vs-code--jupyter-notebooks-on-azureml"></a>AzureML VS Code + jupi not defterlerini kullanarak makine öğrenimi geliştirmeye Başlarken

Bu makalede, bir Azure Machine Learning çalışma alanı ayarlama, işlem örneği oluşturma, yerel makinenizde bir Visual Studio Code geliştirme ortamı ayarlama ve VS Code içinde önceden yapılandırılmış örnek bir Jupyter Not defteri hücrelerini çalıştırma işleminde size kılavuzluk eder.

[Not defteri](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/Sample-Scripts-and-Notebooks/Official/Machine%20Learning%20Notebooks/Transferlearningusing_SSDLiteV2%20Model.ipynb) , Bowls 'yi algılamaya yönelik özel bir veri kümesiyle Python 'da AzureML 'da önceden eğitilen bir TensorFlow modeli (MobileNetSSDV2Lite) kullanarak Aktarım öğrenimini gerçekleştirir.

Not defteri, [Coco veri kümesinden](https://cocodataset.org/#home)nasıl başlayabileceğiniz, bunu yalnızca ilgi sınıfına (Bowls) filtreleyecek şekilde filtreleyip uygun biçime dönüştürmeye yönelik olduğunu gösterir. Alternatif olarak, PASCAL VOC biçiminde sınırlayıcı kutular oluşturup etiketlemek için açık kaynaklı [Vott 2](https://github.com/microsoft/VoTT) etiketleme aracını kullanabilirsiniz.

Model, özel veri kümesinde yeniden öğreticduktan sonra modül ikizi Update yöntemi kullanılarak Azure Percept DK 'nize dağıtılabilir. Daha sonra, Azure Percept Vision SoM 'dan canlı RTSP akışını görüntüleyerek model alt sınırlaması denetimini kontrol edebilirsiniz. Hem model yeniden eğitim hem de dağıtım, uzaktan işlem örneğiniz aracılığıyla Not defteri içinde gerçekleştirilir.

## <a name="prerequisites"></a>Önkoşullar

- [Azure Machine Learning aboneliği](https://azure.microsoft.com/free/services/machine-learning/)
- [Azure Percept Vision Soconnected ile Azure Percept DK](./overview-azure-percept-dk.md)
- [Azure PERCEPT dk taslak oluşturma deneyimi](./quickstart-percept-dk-set-up.md) tamamlandı

## <a name="download-azure-percept-github-repository"></a>Azure Percept GitHub deposunu indirin

1. [Azure PERCEPT dk GitHub deposuna](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview)gidin.
1. Depoyu kopyalayın veya ZIP dosyasını indirin. Ekranın üst kısmındaki **kod**  ->  **indirme ZIP** kodu ' na tıklayın.

    :::image type="content" source="./media/advanced-development-local/download-zip.png" alt-text="GitHub indirme ekranı.":::

## <a name="create-an-azure-machine-learning-workspace-and-remote-compute-instance"></a>Azure Machine Learning çalışma alanı ve uzak işlem örneği oluşturma

1. [Azure Machine Learning portalına](https://ml.azure.com)gidin.
1. Açılır menülerden dizininizi, Azure aboneliğinizi ve Machine Learning çalışma alanını seçin ve **kullanmaya** başlayın ' a tıklayın.

    :::image type="content" source="./media/advanced-development-local/machine-learning-portal-get-started.png" alt-text="Azure ML Başlarken ekranı.":::

    Henüz bir Azure Machine Learning çalışma alanınız yoksa **Yeni çalışma alanı oluştur ' a** tıklayın. Yeni tarayıcı sekmesinde aşağıdakileri yapın:

    1. Azure aboneliğinizi seçin.
    1. Kaynak grubunuzu seçin.
    1. Çalışma alanınız için bir ad girin.
    1. Bölgenizi seçin.
    1. Çalışma alanı sürümünüzü seçin.
    1. **Gözden geçir ve oluştur**’a tıklayın.
    1. Sonraki sayfada, seçimlerinizi gözden geçirin ve **Oluştur**' a tıklayın.

        :::image type="content" source="./media/advanced-development-local/workspace-review-and-create.png" alt-text="Azure ML 'de çalışma alanı oluşturma ekranı.":::

    Lütfen çalışma alanı oluşturma için birkaç dakika bekleyin. Çalışma alanı oluşturma işlemi tamamlandıktan sonra kaynaklarınızın yanında yeşil onay işaretleri görürsünüz ve dağıtımınız Machine Learning Services Genel Bakış sayfasının en üstünde **tamamlanır** .

    :::image type="content" source="./media/advanced-development-local/workspace-creation-complete-inline.png" alt-text="Çalışma alanı oluşturma onayı." lightbox= "./media/advanced-development-local/workspace-creation-complete.png":::

    Çalışma alanı oluşturma işlemi tamamlandıktan sonra Machine Learning portalı sekmesine dönün ve **kullanmaya** başlayın ' a tıklayın.

1. Machine Learning çalışma alanı giriş sayfasında, sol taraftaki bölmede **işlem** ' e tıklayın.

1. Mevcut bir işlem örneği yoksa, **Yeni**' ye tıklayarak yenı bir CPU veya GPU işlem oluşturun. **Yeni işlem örneği** penceresinde bir **işlem adı** girin, bir **sanal makine türü** seçin ve bir **sanal makine boyutu** seçin. **Oluştur**’a tıklayın.

    :::image type="content" source="./media/advanced-development-local/new-compute-instance.png" alt-text="Yeni işlem örneği oluşturma ekranı.":::

    **Oluştur**'a tıkladığınızda, işlem örneğinin oluşturulması birkaç dakika sürer. İşlem **durumunuz,** işlem oluşturma tamamlandıktan sonra yeşil bir daire ve **\<your compute name> çalışır** durumda görüntülenir. Bu işlem örneği Jupyıter sunucusunu çalıştırır ve bu öğretici için yararlanılabilir olacaktır.

## <a name="visual-studio-code-development-environment-setup"></a>Visual Studio Code geliştirme ortamı kurulumu

1. Gerekli araçları yükler.

    1. 1. Seçenek:

        Aşağıdaki paketleri yüklemek için [geliştirme araçları paketi yükleyicisini](./dev-tools-installer.md) kullanın:

        - Visual Studio Code
        - Python 3,5, 3,6 veya 3,7
        - Miniconda3
        - Intel OpenVino araç seti 2020,2

        Note: Intel OpenVino araç seti, geliştirme araçları paketi yükleyicisinde isteğe bağlı bir paket olarak listelenir, ancak Azure Percept DK geliştirme seti 'nin Azure Percept Vision SoM som 'u ile birlikte çalışmak için gereklidir.

    1. 2. Seçenek:

        Geliştirme araçları paketi yükleyicisini kullanmayı tercih ediyorsanız aşağıdaki bağlantılara tıklayarak ve belirtilen indirme ve yükleme yönergelerini izleyerek aşağıdaki paketleri el ile yükleyin:

        - [Visual Studio Code](https://code.visualstudio.com/)
        - [Python 3,5, 3,6 veya 3,7](https://www.python.org/)
        - [Miniconda3](https://docs.conda.io/en/latest/miniconda.html)
        - [Intel OpenVino araç seti 2020,2](https://docs.openvinotoolkit.org/)

    Note: tam Anaconda dağıtımı zaten yüklüyse, Miniconda yüklemeniz gerekmez. Alternatif olarak, Anaconda veya Miniconda kullanmayı tercih ediyorsanız, bir Python sanal ortamı oluşturabilir ve PIP 'yi kullanarak yapay zeka modeli geliştirmeyi çalıştırmak için gereken paketleri yükleyebilirsiniz.

1. Visual Studio Code'u başlatın.
1. Veri bilimi ortamını ayarlama:

    - Seçenek 1-zaten seçkin ML paketlerine sahip olan mevcut veya yeni bir Machine Learning uzak işlem örneğine bağlanın. Bu **öğreticide kullanacağınız seçenektir**.

    - Seçenek 2-yerel makinede bir Conda ortamı ayarlayın.
        1. Bir Anaconda veya Miniconda komut istemi açın ve belirtilen paketlerle **MyENV** adlı bir ortam oluşturmak için aşağıdaki komutu çalıştırın:

            `conda create -n myenv python=3.7 pandas jupyter seaborn scikit-learn keras tensorflow=1.15`

            Anaconda ortamlarını oluşturma ve yönetme hakkında daha fazla bilgi için bkz. [Anaconda belgeleri](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).

1. VS Code çalışma alanı oluşturun:

    1. Visual Studio Code çalışma alanınız olarak kullanılmak üzere uygun bir konumda bir klasör oluşturun. **MyWorkspace** adını adlandırın.
    1. **Dosya** klasörü seç klasörü ' ne tıklayarak **MyWorkspace** 'i Visual Studio Code açın  >    >  .
    1. Dosya Gezgini 'nde, ' a gidin ve Azure Percept DK GitHub deposunun yerel kopyasından [Transferlearningusing_SSDLiteV2 model. ipynbb dosyasını](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/Sample-Scripts-and-Notebooks/Official/Machine%20Learning%20Notebooks/Transferlearningusing_SSDLiteV2%20Model.ipynb) seçin. Bu not defteri dosyasını MyWorkspace klasörüne kopyalayın.

## <a name="azure-integration-with-visual-studio-code"></a>Visual Studio Code ile Azure tümleştirmesi

1. Henüz yapmadıysanız, [Azure Machine Learning ücretsiz veya ücretli sürümüne](https://aka.ms/AMLFree)kaydolun.

1. VS Code için aşağıdaki Azure uzantılarını yükler:
    - [Azure Machine Learning uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).
    - Python Insiders uzantısı. VS Code, **görüntüleme**  ->  **komut paleti**' ne gidin. Komut paletinde **Python: Insiders günlük kanalına geç**' i seçin.
    - [Azure hesabı uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account). İstendiğinde VS Code pencerenizi yeniden yükleyin.
    - [Azure IoT Hub araç seti uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit).
    - [Azure IoT Edge uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).

1. Kaynakları sağlamak ve Azure 'da iş yüklerini çalıştırmak için Visual Studio Code içindeki Azure hesabınızda oturum açın.
    1. Menü çubuğundan komut paletini **göster**' i seçerek Visual Studio Code komut paleti ' ni açın  >   .
    1. Azure girin: oturum açma işlemini başlatmak için komut paletinde **oturum açın** .

        :::image type="content" source="./media/advanced-development-local/transfer-learning-azure-sign-in-inline.png" alt-text="Azure oturum açma ekranı." lightbox= "./media/advanced-development-local/transfer-learning-azure-sign-in.png":::

    1. VS Code sol taraftaki Azure simgesine tıklayarak Python uzantısını ve Azure hesabını etkinleştirin.

        :::image type="content" source="./media/advanced-development-local/azure-icon.png" alt-text="VS Code 'de Azure simgesi.":::

    1. **MyWorkspace** klasöründen Transferlearningusing_SSDLiteV2 Model_VSCode. ipynb Not defterini açın.
    1. Komut paletini açın. Python yazın ve seçin **: bağlantılar için yerel veya uzak Jupyter sunucusu belirtin**.
    1. Aralarından seçim yapabileceğiniz bağlantı seçeneklerinin bir listesini görmeniz gerekir. **Azure ML işlem örnekleri**' ni seçin.

        :::image type="content" source="./media/advanced-development-local/azure-machine-learning-compute-instances.png" alt-text="VS Code 'de Azure ML işlem örneği seçenekleri.":::

    1. Bir abonelik, çalışma alanı ve uzak işlem örneği seçmek için, Kılavuzlu istemleri izleyin. Bu öğreticide daha önce oluşturulan çalışma alanını ve Uzaktan işlem örneğini seçin. Ayrıca yeni bir işlem örneği oluşturma seçeneğiniz de vardır.
    1. Bir işlem örneği seçtikten sonra VS Code pencerenizi yeniden yüklemeniz istenir. Yeniden yükledikten sonra **Python 3,6-AzureML** çekirdeğini seçin. Artık Not defterinizdeki hücrelerden herhangi birini çalıştırabilirsiniz. Not defteri hücresini çalıştırmak, Not defteriniz ve işlem örneğiniz arasındaki bağlantıyı başlatır. Not defteri araç çubuğu, üzerinde çalışmakta olduğunuz işlem örneğini yansıtacak şekilde güncelleştirilir.

        :::image type="content" source="./media/advanced-development-local/kernel-inline.png" alt-text="VS Code çekirdek seçimi." lightbox= "./media/advanced-development-local/kernel.png":::

## <a name="working-with-the-notebook"></a>Not defteriyle çalışma

Artık VS Code içindeki özel maetinizi eğit ve devkit 'e dağıtmak için Not defterini çalıştırmaya hazırsınız. Betiği yürütmeden önce bazı hücrelerden biri giriş parametresi gerektirdiğinden Not defterinin her bir hücresini ayrı ayrı çalıştırdığınızdan emin olun. Giriş parametreleri gerektiren hücreler doğrudan not defterinde düzenlenebilir. Bir hücreyi çalıştırmak için, hücrenin sol tarafındaki Oynat simgesine tıklayın:

:::image type="content" source="./media/advanced-development-local/run-cell-1.png" alt-text="Not defteri vurgulama hücresi simgesi.":::

## <a name="next-steps"></a>Sonraki adımlar

Ek Azure Machine Learning hizmet örneği Not defterleri için lütfen bu [depoyu](https://github.com/Azure/MachineLearningNotebooks/tree/2aa7c53b0ce84e67565d77e484987714fdaed36e/how-to-use-azureml)ziyaret edin.
