---
title: Çalışma alanınızda Jupyter Notebooks’u çalıştırma
titleSuffix: Azure Machine Learning
description: Azure Machine Learning Studio 'da çalışma alanınızdan çıkmadan Jupyter Notebook nasıl çalıştıracağınızı öğrenin.
services: machine-learning
author: abeomor
ms.author: osomorog
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.date: 04/21/2020
ms.openlocfilehash: b8869eee4e44001f5d4aeafbbdb32f93f0a7e0c8
ms.sourcegitcommit: b55d1d1e336c1bcd1c1a71695b2fd0ca62f9d625
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84433342"
---
# <a name="how-to-run-jupyter-notebooks-in-your-workspace-preview"></a>Çalışma alanınızda Jupyıter not defterlerini çalıştırma (Önizleme)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Jupyıter not defterlerinizi Azure Machine Learning Studio 'daki çalışma alanınızda doğrudan çalıştırmayı öğrenin. [Jupi](https://jupyter.org/) veya [Jupyıterlab](https://jupyterlab.readthedocs.io)'ı başlatabilmeniz sırasında, çalışma alanından çıkmadan not defterlerinizi düzenleyebilir ve çalıştırabilirsiniz.

Nasıl kullanabileceğinizi öğrenin:

* Çalışma alanınızda Jupyıter Not defterleri oluşturma
* Bir not defterinden deneme çalıştırma
* Not defteri ortamını değiştirme
* Not defterlerinizi çalıştırmak için kullanılan işlem örneklerinin ayrıntılarını bulun

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://aka.ms/AMLFree) oluşturun.
* Machine Learning çalışma alanı. Bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).

## <a name="create-notebooks"></a><a name="create"></a>Not defterleri oluştur

Azure Machine Learning çalışma alanınızda yeni bir Jupyter Not defteri oluşturun ve çalışmaya başlayın. Yeni oluşturulan Not defteri, varsayılan çalışma alanı depolama alanında depolanır. Bu not defteri, çalışma alanına erişimi olan herkesle paylaşılabilir. 

Yeni bir not defteri oluşturmak için: 

1. Çalışma alanınızı [Azure Machine Learning Studio](https://ml.azure.com)'da açın.
1. Sol tarafta, **Not defterleri**' ni seçin. 
1. **My Files** bölümündeki **User Files** bölümünde **yeni dosya oluştur** simgesini seçin.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/create-new-file.png" alt-text="Yeni dosya oluştur":::

1. Dosyayı adlandırın. 
1. Jupyter Notebook dosyaları için dosya türü olarak **Python Not defteri** ' ni seçin.
1. Bir dosya dizini seçin.
1. **Oluştur**'u seçin.

> [!TIP]
> Metin dosyaları da oluşturabilirsiniz.  Dosya türü olarak **metin** ' i seçin ve uzantıyı ada ekleyin (örneğin, MyFile.py veya dosyam. txt)  

Ayrıca, Not defterleri sayfasının en üstünde bulunan araçlarla Not defterleri de dahil olmak üzere klasörleri ve dosyaları karşıya yükleyebilirsiniz.  Not defterleri ve çoğu metin dosya türleri Önizleme bölümünde görüntülenir.  Diğer dosya türleri için Önizleme yoktur.

### <a name="clone-samples"></a>Örnek kopyalama

Çalışma alanınız, SDK 'Yı keşfetmenize ve kendi makine öğrenimi projeleriniz için örnek olarak size yardımcı olmak üzere tasarlanan Not defterleri içeren bir **Samples** klasörü içerir.  Bu not defterlerini çalışma alanı depolama kabınızda kendi klasörünüze kopyalayabilirsiniz.  

Bir örnek için bkz. [öğretici: Ilk ml denemenize oluşturma](tutorial-1st-experiment-sdk-setup.md#azure).

### <a name="use-files-from-git-and-version-my-files"></a><a name="terminal"></a>Git ve sürüm dosyalarımı dosyaları kullan

Tüm git işlemlerine, bir Terminal penceresi kullanarak erişebilirsiniz. Tüm git dosyaları ve klasörleri, çalışma alanı dosya sisteminizde depolanacak.

> [!NOTE]
> Dosya ve klasörlerinizi **~/CloudFiles/Code/Users** klasörünün altında herhangi bir yere ekleyerek tüm jupi ortamlarınızda görünür olmaları gerekir.

Terminale erişmek için:

1. Çalışma alanınızı [Azure Machine Learning Studio](https://ml.azure.com)'da açın.
1. Sol tarafta, **Not defterleri**' ni seçin.
1. Sol taraftaki **Kullanıcı dosyaları** bölümünde yer alan herhangi bir not defteri seçin.  Not defteriniz yoksa, önce [bir not defteri oluşturun](#create)
1. Bir **işlem** hedefi seçin veya yeni bir tane oluşturun ve çalışır hale gelene kadar bekleyin.
1. **Açık Terminal** simgesini seçin.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/open-terminal.png" alt-text="Açık Terminal":::

1. Simgeyi görmüyorsanız, işlem hedefinin sağındaki **...** ' ı seçin ve ardından **Açık Terminal** ' yı seçin.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/alt-open-terminal.png" alt-text="Terminalden aç...":::


[Git depoları çalışma alanı dosya sisteminize kopyalama](concept-train-model-git-integration.md#clone-git-repositories-into-your-workspace-file-system)hakkında daha fazla bilgi edinin.


### <a name="share-notebooks-and-other-files"></a>Not defterlerini ve diğer dosyaları paylaşma

Bir not defteri veya dosyayı paylaşmak için URL 'YI kopyalayıp yapıştırın.  Yalnızca çalışma alanının diğer kullanıcıları bu URL 'ye erişebilecektir.  [Çalışma alanınıza erişim verme](how-to-assign-roles.md)hakkında daha fazla bilgi edinin.

## <a name="edit-a-notebook"></a>Not defterini düzenleme

Bir not defteri düzenlemek için, çalışma alanınızın **Kullanıcı dosyaları** bölümünde yer alan herhangi bir not defterini açın. Düzenlemek istediğiniz hücreye tıklayın. 

Çalıştıran bir işlem örneği çalışırken, herhangi bir Python not defterinde [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense)tarafından desteklenen kod tamamlamayı da kullanabilirsiniz.

Ayrıca, Not defteri araç çubuğundan Jupyter veya JupyterLab ' i de başlatabilirsiniz.  Azure Machine Learning, Microsoft Desteği sınırının dışında açık kaynak ürünleri olduklarından jupi veya jupi 'nın hata ve hatalarını düzeltir.

### <a name="useful-keyboard-shortcuts"></a>Faydalı klavye kısayolları

|Klavye  |Eylem  |
|---------|---------|
|Shift+Enter     |  Bir hücre Çalıştır       |
|CTRL + a (Windows)     |  Not defterinde sekme yakalamayı etkinleştirin/devre dışı bırakın.       |
|CTRL + SHIFT + d (Mac & Linux)     |    Not defterinde sekme yakalamayı etkinleştirin/devre dışı bırakın.     |
|Sekme (sekme tuzağı etkin olduğunda) | ' \T ' karakteri ekle (Girintile)
|Sekme (Tab tuzağı devre dışı bırakıldığında) | Odağı sonraki odaksız öğeye Dönüştür (hücreyi Sil düğmesi, Çalıştır düğmesi vb.)

## <a name="delete-a-notebook"></a>Not defterini silme

**Örnek** Not *defterlerini silemezsiniz.*  Bu not defterleri, Studio 'nun bir parçasıdır ve her yeni SDK yayımlandığında güncellenir.  

**Kullanıcı dosyaları** not defterlerini şu *yollarla silebilirsiniz:*

* Studio 'da, bir klasörün veya dosyanın sonundaki **...** öğesini seçin.  Desteklenen bir tarayıcı (Microsoft Edge, Chrome veya Firefox) kullandığınızdan emin olun.
* Herhangi bir not defteri araç çubuğundan, işlem örneği için Terminal penceresine erişmek üzere [**terminali aç**](#terminal) ' ı seçin.
* Ya da bu araçların bulunduğu jupi veya Jupyıterlab.

## <a name="run-an-experiment"></a>Deneme çalıştırma

Bir not defterinden deneme çalıştırmak için önce çalışan bir [işlem örneğine](concept-compute-instance.md)bağlanırsınız. İşlem örneğiniz yoksa, oluşturmak için aşağıdaki adımları kullanın: 

1. **+** Not defteri araç çubuğundan öğesini seçin. 
2. Işlem adını adlandırın ve bir **sanal makine boyutu**seçin. 
3. **Oluştur**'u seçin.
4. İşlem örneği not defterine otomatik olarak bağlanır ve artık hücrelerden çalıştırabilirsiniz.

Yalnızca sizin oluşturduğunuz işlem örneklerini görebilir ve kullanabilirsiniz.  **Kullanıcı DOSYALARıNıZ** VM 'den ayrı olarak depolanır ve çalışma alanındaki tüm işlem örnekleri arasında paylaşılır.

### <a name="view-logs-and-output"></a>Günlükleri ve çıktıyı görüntüleme

Çalışma ve günlüklerin ilerlemesini görüntülemek için [Not defteri pencere](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) öğelerini kullanın. Pencere öğesi zaman uyumsuzdur ve eğitim bitene kadar güncelleştirmeler sağlar. Azure Machine Learning pencere öğeleri Jupyıter ve JupterLab içinde de desteklenir.

## <a name="change-the-notebook-environment"></a>Not defteri ortamını değiştirme

Not defteri araç çubuğu, not defterinizin çalıştırıldığı ortamı değiştirmenize izin verir.  

Bu eylemler Not defteri durumunu veya not defterindeki değişkenlerin değerlerini değiştirmez:

|Eylem  |Sonuç  |
|---------|---------| --------|
|Çekirdeği durdur     |  Çalışan tüm hücreleri sonlandırır. Bir hücreyi çalıştırmak, çekirdeği otomatik olarak yeniden başlatır. |
|Başka bir çalışma alanı bölümüne git     |     Çalışan hücreler durdurulur. |

Bu eylemler, Not defteri durumunu sıfırlayacaktır ve not defterindeki tüm değişkenleri sıfırlar.

|Eylem  |Sonuç  |
|---------|---------| --------|
| Çekirdeği değiştirme | Not defteri yeni çekirdek kullanır |
| İşlem değiştirme    |     Not Defteri otomatik olarak yeni işlem kullanır. |
| İşlem sıfırlama | Bir hücreyi çalıştırmaya çalıştığınızda yeniden başlar |
| İşlem durdur     |    Hiçbir hücre çalıştırılmayacak  |
| Jupyter veya JupyterLab 'da Not defteri 'ni açın     |    Yeni bir sekmede açılan not defteri.  |

### <a name="add-new-kernels"></a>Yeni çekirdekler Ekle

Not defteri, bağlı işlem örneğinde yüklü olan tüm Jupyter çekirdekler 'leri otomatik olarak bulur.  İşlem örneğine bir çekirdek eklemek için:

1. Not defteri araç çubuğunda [**Terminal aç**](#terminal) ' ı seçin.
1. Yeni bir ortam oluşturmak için Terminal penceresini kullanın.
1. Ortamı etkinleştirin.  Örneğin, oluşturduktan sonra `newenv` :

    ```shell
    source activate newenv
    python -m ipykernel install --user --name newenv --display-name "Python (newenv)"
    ```

[Kullanılabilir Jupyter kernels](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels) 'leri yüklenebilir.

### <a name="status-indicators"></a>Durum göstergeleri

**İşlem** açılan listesinin yanındaki gösterge durumunu gösterir.  Durum, açılan listede de gösterilir.  

|Renk |İşlem durumu |
|---------|---------| 
| Yeşil | İşlem çalışıyor |
| Red |İşlem başarısız oldu | 
| Siyah | İşlem durduruldu |
|  Açık mavi |İşlem oluşturma, başlatma, yeniden başlatma, kurulum |
|  Tonlamalı |İşlem silme, durdurma |

**Çekirdek** açılan listesinin yanındaki gösterge durumunu gösterir.

|Renk |Çekirdek durumu |
|---------|---------|
|  Yeşil |Çekirdek bağlı, boşta, meşgul|
|  Tonlamalı |Çekirdek bağlı değil |

## <a name="find-compute-details"></a>İşlem ayrıntılarını bul 

[Studio](https://ml.azure.com)'daki **işlem** sayfasında işlem örneklerinizin ayrıntılarını bulun.

## <a name="next-steps"></a>Sonraki adımlar

* [İlk denemenizi çalıştırın](tutorial-1st-experiment-sdk-train.md)
* [Dosya depolama alanınızı anlık görüntülerle yedekleyin](../storage/files/storage-snapshots-files.md)
