---
title: Bulutta Azure Percept gelişmiş geliştirme ile çalışmaya başlama
description: Jupi Not defterleri aracılığıyla bulutta gelişmiş geliştirme ile çalışmaya başlayın ve Azure Machine Learning
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: tutorial
ms.date: 02/10/2021
ms.custom: template-tutorial
ms.openlocfilehash: 408040ea68273a29ed9be87b60bd0cc6da736a05
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101665056"
---
# <a name="getting-started-with-advanced-development-in-the-cloud-via-jupyter-notebooks-and-azure-machine-learning"></a>Jupi Not defterleri ile bulutta gelişmiş geliştirme ile çalışmaya başlama ve Azure Machine Learning

Bu makalede, bir Azure Machine Learning çalışma alanı ayarlama, önceden yapılandırılmış bir örnek Jupyter Notebook çalışma alanına yükleme, bir işlem örneği oluşturma ve çalışma alanı içinde Not defterinin hücrelerini çalıştırma işleminde size kılavuzluk eder.

[Not defteri](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/Sample-Scripts-and-Notebooks/Official/Machine%20Learning%20Notebooks/Transferlearningusing_SSDLiteV2%20Model.ipynb) , Bowls 'yi algılamaya yönelik özel bir veri kümesiyle Python 'da AzureML 'da önceden eğitilen bir TensorFlow modeli (MobileNetSSDV2Lite) kullanarak Aktarım öğrenimini gerçekleştirir.

Not defteri, [Coco](https://cocodataset.org/#home)'dan nasıl başlayabileceğiniz, bunu yalnızca ilgi sınıfına (Bowls) filtreleyecek şekilde filtreleyip uygun biçime dönüştürmeye yönelik olduğunu gösterir. Alternatif olarak, PASCAL VOC biçiminde sınırlayıcı kutular oluşturup etiketlemek için açık kaynaklı [Vott 2](https://github.com/microsoft/VoTT) etiketleme aracını kullanabilirsiniz.

Model, özel veri kümesinde yeniden öğreticduktan sonra, model ikizi Update yöntemi kullanılarak Azure Percept DK 'nize dağıtılabilir.

Daha sonra, Azure Percept Vision SoM 'dan canlı RTSP akışını görüntüleyerek model alt sınırlaması denetimini kontrol edebilirsiniz. Hem model yeniden eğitim hem de dağıtım, bulutta Not defteri içinde gerçekleştirilir.

## <a name="prerequisites"></a>Önkoşullar

- [Azure Machine Learning aboneliği](https://azure.microsoft.com/free/services/machine-learning/)
- [Azure Percept Vision Soconnected ile Azure Percept DK](./overview-azure-percept-dk.md)
- [Azure PERCEPT dk taslak oluşturma deneyimi](./quickstart-percept-dk-set-up.md) tamamlandı

## <a name="download-azure-percept-github-repository"></a>Azure Percept GitHub deposunu indirin

1. [Azure Percept GitHub deposuna](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview)gidin.

1. Depoyu kopyalayın veya ZIP dosyasını indirin. Ekranın üst kısmındaki **kod**  ->  **indirme ZIP** kodu ' na tıklayın.

    :::image type="content" source="./media/advanced-development-cloud/download-zip.png" alt-text="GitHub indirme ekranı.":::

## <a name="set-up-azure-machine-learning-portal-and-notebook"></a>Azure Machine Learning Portal ve Not defteri ayarlama

1. [Azure Machine Learning portalına](https://ml.azure.com)gidin.

1. Açılır menülerden dizininizi, Azure aboneliğinizi ve Machine Learning çalışma alanını seçin ve **kullanmaya** başlayın ' a tıklayın.

    :::image type="content" source="./media/advanced-development-cloud/machine-learning-portal-get-started.png" alt-text="Azure ML Başlarken ekranı.":::

    Henüz bir Azure Machine Learning çalışma alanınız yoksa **Yeni çalışma alanı oluştur ' a** tıklayın. Yeni tarayıcı sekmesinde aşağıdakileri yapın:

    1. Azure aboneliğinizi seçin.
    1. Kaynak grubunuzu seçin.
    1. Çalışma alanınız için bir ad girin.
    1. Bölgenizi seçin.
    1. Çalışma alanı sürümünüzü seçin.
    1. **Gözden geçir ve oluştur**’a tıklayın.
    1. Sonraki sayfada, seçimlerinizi gözden geçirin ve **Oluştur**' a tıklayın.

        :::image type="content" source="./media/advanced-development-cloud/workspace-review-and-create.png" alt-text="Azure ML 'de çalışma alanı oluşturma ekranı.":::

    Lütfen çalışma alanı oluşturma için birkaç dakika bekleyin. Çalışma alanı oluşturma işlemi tamamlandıktan sonra kaynaklarınızın yanında yeşil onay işaretleri görürsünüz ve dağıtımınız Machine Learning Services Genel Bakış sayfasının en üstünde **tamamlanır** .

    :::image type="content" source="./media/advanced-development-cloud/workspace-creation-complete-inline.png" alt-text="Çalışma alanı oluşturma onayı." lightbox= "./media/advanced-development-cloud/workspace-creation-complete.png":::

    Çalışma alanı oluşturma işlemi tamamlandıktan sonra Machine Learning portalı sekmesine dönün ve **kullanmaya** başlayın ' a tıklayın.

1. Machine Learning çalışma alanı giriş sayfasında sol bölmedeki **Not defterleri** ' ne tıklayın.

    :::image type="content" source="./media/advanced-development-cloud/notebook.png" alt-text="Azure ML giriş sayfası.":::

1. **My Files** sekmesinde Dikey oka tıklayarak. ipynb dosyanızı karşıya yükleyin.

    :::image type="content" source="./media/advanced-development-cloud/upload-files.png" alt-text="Karşıya dosya Yükle simgesini vurgulayan giriş sayfası.":::

1. ' A gidin ve Azure Percept GitHub deposunun yerel kopyasından [Transferlearningusing_SSDLiteV2 model. ipynb dosyasını](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/Sample-Scripts-and-Notebooks/Official/Machine%20Learning%20Notebooks/Transferlearningusing_SSDLiteV2%20Model.ipynb) seçin. **Aç**’a tıklayın. **Dosyaları karşıya yükle** penceresinde, **Bu dosyadaki içeriğe güvenim** ' ın yanındaki kutuyu işaretleyin ve **karşıya yükle**' ye tıklayın.

    :::image type="content" source="./media/advanced-development-cloud/select-file.png" alt-text="Dosya seçim ekranı.":::

1. Sağ üst menü çubuğunda, **işlem** durumunuzu denetleyin. Hiçbir hesaplama bulunamazsa, **+** Yeni bir işlem oluşturmak için simgeye tıklayın.

    :::image type="content" source="./media/advanced-development-cloud/no-computes-found-inline.png" alt-text="+ Simgesi vurgulanmış şekilde işlem durumu." lightbox= "./media/advanced-development-cloud/no-computes-found.png":::

1. **Yeni işlem örneği** penceresinde bir **işlem adı** girin, bir **sanal makine türü** seçin ve bir **sanal makine boyutu** seçin. **Oluştur**’a tıklayın.

    :::image type="content" source="./media/advanced-development-cloud/new-compute-instance.png" alt-text="Yeni işlem örneği oluşturma ekranı.":::

    **Oluştur**'a tıkladığınızda, **işlem** durumunuz mavi bir daire ve **\<your compute name> oluşturma**

    :::image type="content" source="./media/advanced-development-cloud/compute-creating.png" alt-text="İşlem oluşturma işlemi devam ederken işlem durumu.":::

    İşlem **durumunuz,** işlem oluşturma tamamlandıktan sonra yeşil bir daire ve **\<your compute name> çalışır** durumda görüntülenir.

    :::image type="content" source="./media/advanced-development-cloud/compute-running.png" alt-text="İşlem oluşturma işleminin tamamlanma durumu gösteriliyor.":::

1. İşlem çalışırken, simgenin yanındaki açılan menüden **Python 3,6-AzureML** çekirdeğini seçin **+** .

## <a name="working-with-the-notebook"></a>Not defteriyle çalışma

Artık Özel Alım algılayıcısı 'nı eğit ve devkit 'e dağıtmak için Not defterini çalıştırmaya hazırsınız. Betiği yürütmeden önce bazı hücrelerden biri giriş parametresi gerektirdiğinden Not defterinin her bir hücresini ayrı ayrı çalıştırdığınızdan emin olun. Giriş parametreleri gerektiren hücreler doğrudan not defterinde düzenlenebilir. Bir hücreyi çalıştırmak için, hücrenin sol tarafındaki Oynat simgesine tıklayın:

:::image type="content" source="./media/advanced-development-cloud/run-cell-inline.png" alt-text="Not defteri vurgulama hücresi simgesi." lightbox= "./media/advanced-development-cloud/run-cell.png":::

## <a name="next-steps"></a>Sonraki adımlar

Ek Azure Machine Learning hizmet örneği Not defterleri için lütfen bu [depoyu](https://github.com/Azure/MachineLearningNotebooks/tree/2aa7c53b0ce84e67565d77e484987714fdaed36e/how-to-use-azureml) ziyaret edin
