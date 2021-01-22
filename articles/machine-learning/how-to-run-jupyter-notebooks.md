---
title: Çalışma alanınızda Jupyıter not defterlerini çalıştırma
titleSuffix: Azure Machine Learning
description: Jupyter Not defterini Azure Machine Learning Studio 'da çalışma alanınızdan çıkmadan nasıl çalıştıracağınızı öğrenin.
services: machine-learning
author: abeomor
ms.author: osomorog
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.date: 01/19/2021
ms.openlocfilehash: d6832238b0c76059079e2a1330d31eed3212b242
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98685587"
---
# <a name="how-to-run-jupyter-notebooks-in-your-workspace"></a>Çalışma alanınızda Jupyter Notebooks’u çalıştırma

Jupyıter not defterlerinizi Azure Machine Learning Studio 'daki çalışma alanınızda doğrudan çalıştırmayı öğrenin. [Jupi](https://jupyter.org/) veya [Jupyıterlab](https://jupyterlab.readthedocs.io)'ı başlatabilmeniz sırasında, çalışma alanından çıkmadan not defterlerinizi düzenleyebilir ve çalıştırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://aka.ms/AMLFree) oluşturun.
* Machine Learning çalışma alanı. Bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).

## <a name="create-notebooks"></a><a name="create"></a> Not defterleri oluştur

Azure Machine Learning çalışma alanınızda yeni bir Jupyter Not defteri oluşturun ve çalışmaya başlayın. Yeni oluşturulan Not defteri, varsayılan çalışma alanı depolama alanında depolanır. Bu not defteri, çalışma alanına erişimi olan herkesle paylaşılabilir. 

Yeni bir not defteri oluşturmak için: 

1. Çalışma alanınızı [Azure Machine Learning Studio](https://ml.azure.com)'da açın.
1. Sol tarafta, **Not defterleri**' ni seçin. 
1. **My Files** bölümündeki **User Files** bölümünde **yeni dosya oluştur** simgesini seçin.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/create-new-file.png" alt-text="Yeni dosya oluştur":::

1. Dosyayı adlandırın. 
1. Jupyter Not defteri dosyaları için dosya türü olarak **Not defteri** ' ni seçin.
1. Bir dosya dizini seçin.
1. **Oluştur**’u seçin.

Metin dosyaları da oluşturabilirsiniz.  Dosya türü olarak **metin** ' i seçin ve uzantıyı ada ekleyin (örneğin, myfile.py veya myfile.txt)  

Ayrıca, Not defterleri sayfasının en üstünde bulunan araçlarla Not defterleri de dahil olmak üzere klasörleri ve dosyaları karşıya yükleyebilirsiniz.  Not defterleri ve çoğu metin dosya türleri Önizleme bölümünde görüntülenir.  Diğer dosya türleri için Önizleme yoktur.

> [!IMPORTANT]
> Not defterlerinde ve betiklerde bulunan içerikler, oturumlarınızdan verileri okuyabilir ve Azure 'da kuruluşunuz olmadan verilere erişebilir.  Yalnızca güvenilir kaynaklardan gelen dosyaları yükleyin. Daha fazla bilgi için bkz. [güvenli kod en iyi uygulamaları](concept-secure-code-best-practice.md#azure-ml-studio-notebooks).

### <a name="clone-samples"></a>Örnek kopyalama

Çalışma alanınız, SDK 'Yı keşfetmenize ve kendi makine öğrenimi projeleriniz için örnek olarak size yardımcı olmak üzere tasarlanan Not defterleri içeren bir **Samples** klasörü içerir.  Bu not defterlerini çalışma alanı depolama kabınızda kendi klasörünüze kopyalayabilirsiniz.  

Bir örnek için bkz. [öğretici: Ilk ml denemenize oluşturma](tutorial-1st-experiment-sdk-setup.md#azure).

### <a name="use-files-from-git-and-version-my-files"></a><a name="terminal"></a> Git ve sürüm dosyalarımı dosyaları kullan

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

1. Simgeyi görmüyorsanız, işlem hedefinin sağındaki **...** ' ı seçin ve ardından **Açık Terminal**' yı seçin.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/alt-open-terminal.png" alt-text="Terminalden aç...":::


[Git depoları çalışma alanı dosya sisteminize kopyalama](concept-train-model-git-integration.md#clone-git-repositories-into-your-workspace-file-system)hakkında daha fazla bilgi edinin.

### <a name="copy-and-paste-in-terminal"></a>Terminalde Kopyala ve Yapıştır

> * Windows: `Ctrl-Insert` kopyalayıp `Ctrl-Shift-v` yapıştırmak için veya kullanın `Shift-Insert` .
> * Mac OS: `Cmd-c` kopyalamak ve `Cmd-v` yapıştırmak için.
> * FireFox/IE Pano izinlerini düzgün şekilde desteklemiyor olabilir.

### <a name="share-notebooks-and-other-files"></a>Not defterlerini ve diğer dosyaları paylaşma

Bir not defteri veya dosyayı paylaşmak için URL 'YI kopyalayıp yapıştırın.  Bu URL 'ye yalnızca çalışma alanının diğer kullanıcıları erişebilir.  [Çalışma alanınıza erişim verme](how-to-assign-roles.md)hakkında daha fazla bilgi edinin.

## <a name="edit-a-notebook"></a>Not defterini düzenleme

Bir not defteri düzenlemek için, çalışma alanınızın **Kullanıcı dosyaları** bölümünde yer alan herhangi bir not defterini açın. Düzenlemek istediğiniz hücreye tıklayın. 

Bir işlem örneğine bağlanmadan Not defterini düzenleyebilirsiniz.  Not defterindeki hücreleri çalıştırmak istediğinizde, bir işlem örneği seçin veya oluşturun.  Durdurulmuş bir işlem örneği seçerseniz, ilk hücreyi çalıştırdığınızda otomatik olarak başlatılır.

Bir işlem örneği çalışırken, herhangi bir Python not defterinde [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense)tarafından desteklenen kod tamamlamayı da kullanabilirsiniz.

Ayrıca, Not defteri araç çubuğundan Jupyter veya JupyterLab ' i de başlatabilirsiniz.  Azure Machine Learning, Microsoft Desteği sınırının dışında açık kaynak ürünleri olduklarından jupi veya jupi 'nın hata ve hatalarını düzeltir.

### <a name="focus-mode"></a>Odak modu

Etkin sekmelerinize odaklanabilmeniz için, geçerli görünümünüzü genişletmek üzere odak modunu kullanın. Odak modu, Not defterleri dosya gezginini gizler.

1. Odak modunu açmak için Terminal penceresi araç çubuğunda **odak modu** ' nu seçin. Pencere genişenize bağlı olarak, bu, araç çubuğunuza **...** menü öğesinin altında bulunabilir.
1. Odak modundayken **Standart Görünüm '** ü seçerek standart görünüme geri dönün.

    :::image type="content" source="media/how-to-run-jupyter-notebooks/focusmode.gif" alt-text="Odak modunu/standart görünümü değiştirme":::


### <a name="use-intellisense"></a>IntelliSense kullanma

[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) , bir dizi özelliği içeren bir kod tamamlama yardımıdır: liste üyeleri, parametre bilgileri, hızlı bilgi ve tam sözcük. Bu özellikler, kullanmakta olduğunuz kod hakkında daha fazla bilgi edinmenize, yazmakta olduğunuz parametreleri izlemenize ve yalnızca birkaç tuş vuruşu ile özelliklere ve yöntemlere çağrılar eklemenize yardımcı olur.  

Kod yazarken, IntelliSense 'i tetiklemek için Ctrl + Space tuşlarını kullanın.

### <a name="clean-your-notebook-preview"></a>Not defterinizi Temizleme (Önizleme)

> [!IMPORTANT]
> Toplama özelliği şu anda genel önizlemededir.
> Önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bir not defteri oluşturma sırasında, genellikle veri araştırma veya hata ayıklama için kullandığınız hücrelerle birlikte sona erdir demektir. *Toplama* özelliği, bu yabancı hücreler olmadan temiz bir not defteri oluşturmanıza yardımcı olur.

1. Tüm Not defteri hücrelerinizi çalıştırın.
1. Yeni Not defterinin çalıştırmasını istediğiniz kodu içeren hücreyi seçin. Örneğin, bir deneme gönderen kod veya belki de bir modeli kaydeden kod.
1. Hücre araç çubuğunda görüntülenen **topla** simgesini seçin.
    :::image type="content" source="media/how-to-run-jupyter-notebooks/gather.png" alt-text="Ekran görüntüsü: toplama simgesini seçin":::
1. Yeni "toplanan" Not defteriniz için ad girin.  

Yeni Not defteri, toplama için seçtiğiniz hücreyle aynı sonuçları üretmek için gereken tüm hücrelere sahip yalnızca kod hücrelerini içerir.

### <a name="save-and-checkpoint-a-notebook"></a>Not defteri kaydetme ve kontrol noktası

Azure Machine Learning, bir *ıpynb* dosyası oluşturduğunuzda bir denetim noktası dosyası oluşturur.

Not defteri araç çubuğunda, menüyü seçin ve ardından not defterini el ile kaydetmek için **&gt; Kaydet ve kontrol noktası dosya** ve not defteriyle ilişkili bir kontrol noktası dosyası eklenir.

:::image type="content" source="media/how-to-run-jupyter-notebooks/file-save.png" alt-text="Not defteri araç çubuğunda kaydetme aracının ekran görüntüsü":::

Her bir not defteri, her 30 saniyede bir otomatik kaydedilir. Otomatik kaydetme, denetim noktası dosyasını değil yalnızca ilk *ipynb* dosyasını güncelleştirir.
 
Adlandırılmış bir kontrol noktası oluşturmak ve Not defterini kaydedilmiş bir denetim noktasına dönüştürmek için Not defteri menüsünde **kontrol noktaları** ' nı seçin.

## <a name="delete-a-notebook"></a>Not defterini silme

**Örnek** Not *defterlerini silemezsiniz.*  Bu not defterleri, Studio 'nun bir parçasıdır ve her yeni SDK yayımlandığında güncellenir.  

**Kullanıcı dosyaları** not defterlerini şu *yollarla silebilirsiniz:*

* Studio 'da, bir klasörün veya dosyanın sonundaki **...** öğesini seçin.  Desteklenen bir tarayıcı (Microsoft Edge, Chrome veya Firefox) kullandığınızdan emin olun.
* Herhangi bir not defteri araç çubuğundan, işlem örneği için Terminal penceresine erişmek üzere [**terminali aç**](#terminal)  ' ı seçin.
* Ya da bu araçların bulunduğu jupi veya Jupyıterlab.

## <a name="run-a-notebook-or-python-script"></a>Bir not defteri veya Python betiği çalıştırma

Bir not defteri veya Python betiği çalıştırmak için önce çalışan bir [işlem örneğine](concept-compute-instance.md)bağlanırsınız. İşlem örneğiniz yoksa, oluşturmak için aşağıdaki adımları kullanın: 

1. **+** Not defteri veya betik araç çubuğunda öğesini seçin. 
2. Işlem adını adlandırın ve bir **sanal makine boyutu** seçin. 
3. **Oluştur**’u seçin.
4. İşlem örneği dosyaya otomatik olarak bağlanır.  Artık işlem örneğinin solundaki aracı kullanarak not defteri hücrelerini veya Python betiğini çalıştırabilirsiniz

Yalnızca sizin oluşturduğunuz işlem örneklerini görebilir ve kullanabilirsiniz.  **Kullanıcı DOSYALARıNıZ** VM 'den ayrı olarak depolanır ve çalışma alanındaki tüm işlem örnekleri arasında paylaşılır.

### <a name="view-logs-and-output"></a>Günlükleri ve çıktıyı görüntüleme

Çalışma ve günlüklerin ilerlemesini görüntülemek için [Not defteri pencere](/python/api/azureml-widgets/azureml.widgets?preserve-view=true&view=azure-ml-py) öğelerini kullanın. Pencere öğesi zaman uyumsuzdur ve eğitim bitene kadar güncelleştirmeler sağlar. Azure Machine Learning pencere öğeleri Jupyıter ve JupterLab içinde de desteklenir.

:::image type="content" source="media/how-to-run-jupyter-notebooks/jupyter-widget.png" alt-text="Ekran görüntüsü: Jupyter Not defteri pencere öğesi ":::

## <a name="explore-variables-in-the-notebook"></a>Not defterindeki değişkenleri keşfet

Not defteri araç çubuğunda, Not defterinizde oluşturulmuş tüm değişkenlerin adını, türünü, uzunluğunu ve örnek değerlerini göstermek için **değişken gezgin** aracını kullanın.

:::image type="content" source="media/how-to-run-jupyter-notebooks/variable-explorer.png" alt-text="Ekran görüntüsü: değişken gezgin aracı":::

Değişken Gezgini penceresini göstermek için aracı seçin.

:::image type="content" source="media/how-to-run-jupyter-notebooks/variable-explorer-window.png" alt-text="Ekran görüntüsü: değişken Gezgin penceresi":::

## <a name="navigate-with-a-toc"></a>TOC ile gezinme

Not defteri araç çubuğunda, içindekiler tablosunu göstermek veya gizlemek için  **Içindekiler tablosu** aracını kullanın.  İçindekiler tablosuna eklemek için başlık ile bir markaşağı hücresi başlatın. Not defterindeki bir hücreye kaydırmak için tablodaki bir girdiye tıklayın.  

:::image type="content" source="media/how-to-run-jupyter-notebooks/table-of-contents.png" alt-text="Ekran görüntüsü: not defterindeki içindekiler tablosu":::

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
1. Yeni bir ortam oluşturmak için Terminal penceresini kullanın.  Örneğin, aşağıdaki kod oluşturulur `newenv` :
    ```shell
    conda create -y --name newenv
    ```
1. Ortamı etkinleştirin.  Örneğin, oluşturduktan sonra `newenv` :

    ```shell
    conda activate newenv
    ```
1. Yeni ortama privand ipykernel paketini yükleyip bu Conda env için bir çekirdek oluşturun

    ```shell
    conda install -y pip
    conda install -y ipykernel
    python -m ipykernel install --user --name newenv --display-name "Python (newenv)"
    ```
1. Çekirdeği yükledikten sonra lütfen sayfayı yenileyin ve bir not defteri açın. Artık çekirdek listesinde yeni çekirdeği görürsünüz.

> [!NOTE]
> Bir not defteri içinde paket yönetimi için **% PIP** veya **% Conda** Magic işlevlerini kullanarak paketleri, tüm paketlere (Şu anda çalışan çekirdeklerdeki paketler dahil), tüm paketlere başvuran **! PIP** veya **! Conda** yerine **çalışmakta olan çekirdeğe** otomatik olarak yükler.

[Kullanılabilir Jupyter kernels](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels) 'leri yüklenebilir.

### <a name="status-indicators"></a>Durum göstergeleri

**İşlem** açılan listesinin yanındaki gösterge durumunu gösterir.  Durum, açılan listede de gösterilir.  

|Renk |İşlem durumu |
|---------|---------| 
| Yeşil | İşlem çalışıyor |
| Kırmızı |İşlem başarısız oldu | 
| Siyahi | İşlem durduruldu |
|  Açık mavi |İşlem oluşturma, başlatma, yeniden başlatma, kurulum |
|  Tonlamalı |İşlem silme, durdurma |

**Çekirdek** açılan listesinin yanındaki gösterge durumunu gösterir.

|Renk |Çekirdek durumu |
|---------|---------|
|  Yeşil |Çekirdek bağlı, boşta, meşgul|
|  Tonlamalı |Çekirdek bağlı değil |

## <a name="shortcut-keys"></a>Kısayol tuşları
Jupi not defterlerine benzer şekilde, Azure Machine Learning Studio Not defterleri de kalıcı bir kullanıcı arabirimine sahiptir. Klavye, Not defteri hücresinin bulunduğu moda bağlı olarak farklı şeyler yapar. Azure Machine Learning Studio Not defterleri, belirli bir kod hücresi için aşağıdaki iki modu destekler: komut modu ve düzenleme modu.

### <a name="command-mode-shortcuts"></a>Komut modu kısayolları

Bir hücre, yazmanızı isteyen bir metin imleci olmadığında komut modundadır. Bir hücre komut modundayken, Not defterini tek tek hücrelere değil, bir bütün olarak düzenleyebilirsiniz. `ESC`Bir hücrenin düzenleyici alanının dışında seçim yapmak için fare tuşuna basarak veya fareyi kullanarak komut moduna girin.  Etkin hücrenin sol kenarlığı mavi ve Solid, **Çalıştır** düğmesi ise mavi.

   :::image type="content" source="media/how-to-run-jupyter-notebooks/command-mode.png" alt-text="Komut modundaki Not defteri hücresi ":::

| Kısayol                      | Description                          |
| ----------------------------- | ------------------------------------|
| Enter                         | Düzenleme moduna girme             |        
| Shift + Enter                 | Hücreyi Çalıştır, aşağıdan seçin         |     
| Denetim/komut + ENTER       | Çalışma hücresi                            |
| Alt + Enter                   | Hücreyi Çalıştır, aşağıya kod hücresi Ekle    |
| Denetim/komut + alt + ENTER | Hücreyi Çalıştır, aşağı doğru markbelow hücresi Ekle|
| Alt + R                       | Tümünü Çalıştır      |                       
| Y                             | Hücreyi koda Dönüştür    |                         
| M                             | Hücreyi markaşağı Dönüştür  |                       
| Yukarı/K                          | Üstteki hücreyi seçin    |               
| Aşağı/J                        | Aşağıdan hücre seçin    |               
| A                             | Yukarıdaki kod hücresini Ekle  |            
| B                             | Aşağıya kod hücresi Ekle   |           
| Denetim/komut + Shift + A   | Yukarıya markaşağı hücresi Ekle    |      
| Denetim/komut + SHIFT + B   | Aşağı markbelow hücresi Ekle   |       
| X                             | Seçili hücreyi kes    |               
| C                             | Seçili hücreyi Kopyala   |               
| SHIFT + V                     | Seçili hücreyi yukarıya Yapıştır           |
| V                             | Seçili hücreyi aşağı Yapıştır    |       
| D D                           | Seçili hücreyi Sil|                
| O                             | Çıkışı geç         |              
| SHIFT + O                     | Çıkış kaydırmayı aç   |          
| Ben                           | Kesme çekirdeği |                   
| 0 0                           | Çekirdeği yeniden Başlat |                     
| SHIFT + boşluk                 | Yukarı kaydır  |                         
| Alan                         | Aşağı kaydır|
| Tab                           | Odağı sonraki odaksız öğeye Değiştir (sekme tuzağı devre dışı bırakıldığında)|
| Denetim/komut + S           | Not defterini Kaydet |                      
| 1                             | H1 olarak değiştir|                       
| 2                             | H2 olarak değiştir|                        
| 3                             | H3 olarak değiştir|                        
| 4                             | H4 olarak değiştir |                       
| 5                             | H5 olarak değiştir |                       
| 6                             | H6 olarak değiştir |                       

### <a name="edit-mode-shortcuts"></a>Düzenleme modu kısayolları

Düzenleme modu, düzenleyici alanına yazmanızı isteyen bir metin imlece belirtilir. Bir hücre düzenleme modundayken, hücreye yazabilirsiniz. `Enter`Fareyle sürükleyerek veya bir hücrenin düzenleyici alanında seçim yaparak düzenleme modunu girin. Etkin hücrenin sol kenarlığı yeşil ve taranmış ve **Çalıştır** düğmesi yeşil olur. Ayrıca, imleç isteminin hücrede düzenleme modunda görürsünüz.

   :::image type="content" source="media/how-to-run-jupyter-notebooks/edit-mode.png" alt-text="Düzenleme modundaki Not defteri hücresi":::

Aşağıdaki tuş vuruşu kısayollarını kullanarak, düzenleme modundayken Azure Machine Learning Not defterlerindeki kodu daha kolay bir şekilde gezinebilir ve çalıştırabilirsiniz.

| Kısayol                      | Description|                                     
| ----------------------------- | ----------------------------------------------- |
| Esc                        | Komut moduna gir|  
| Denetim/komut + boşluk       | IntelliSense 'i etkinleştir |
| Shift + Enter                 | Hücreyi Çalıştır, aşağıdan seçin |                         
| Denetim/komut + ENTER       | Çalışma hücresi  |                                      
| Alt + Enter                   | Hücreyi Çalıştır, aşağıya kod hücresi Ekle  |              
| Denetim/komut + alt + ENTER | Hücreyi Çalıştır, aşağı doğru markbelow hücresi Ekle  |          
| Alt + R                       | Tüm hücreleri Çalıştır     |                              
| Yukarı                            | İmleci yukarı veya önceki hücreyi taşıyın    |             
| Aşağı                          | İmleci aşağı veya sonraki hücreyi taşı |                  
| Denetim/komut + S           | Not defterini Kaydet   |                                
| Denetim/komut + yukarı          | Hücre başlangıcına git   |                             
| Denetim/komut + aşağı        | Hücre sonuna git |                                 
| Tab                           | Kod tamamlama veya girintileme (sekme tuzağı etkinse) |
| Denetim/komut + d           | Sekme yakalamayı etkinleştir/devre dışı bırak  |                       
| Denetim/komut +]           | Leyebilirsiniz |                                         
| Denetim/komut + [           | Girintiyi geri al  |                                        
| Control/Command + A           | Tümünü seç|                                      
| Denetim/komut + Z           | Geri Al |                                           
| Denetim/komut + SHIFT + Z   | Yinele |                                           
| Denetim/komut + Y           | Yinele |                                           
| Denetim/komut + giriş sayfası        | Hücre başlangıcına git|                                
| Denetim/komut + End         | Hücre sonuna git   |                               
| Denetim/komut + sol        | Bir kelimeyi sola git |                               
| Denetim/komut + sağ       | Bir sözcüğe sağ git |                              
| Denetim/komut + geri al   | Önceki kelimeyi Sil |                             
| Denetim/komut + Sil      | Sonra sözcüğü Sil |                              
| Denetim/komut +/           | Cu 'daki yorumu değiştirme

## <a name="find-compute-details"></a>İşlem ayrıntılarını bul

[Studio](https://ml.azure.com)'daki **işlem** sayfasında işlem örneklerinizin ayrıntılarını bulun.

## <a name="troubleshooting"></a>Sorun giderme

* Bir not defterine bağlanamıyorsanız, Web yuva iletişiminin devre dışı **bırakılmadığından** emin olun. İşlem örneği jupi işlevinin çalışması için Web yuva iletişiminin etkinleştirilmesi gerekir. Lütfen ağınızın *. instances.azureml.net ve *. instances.azureml.ms öğesine WebSocket bağlantılarına izin verdiğinden emin olun. 

* İşlem örneği bir özel bağlantı çalışma alanında dağıtıldığında, yalnızca sanal ağ içinden erişilebilir. Özel DNS veya hosts dosyası kullanıyorsanız lütfen <örnek adı> için bir giriş <region> ekleyin. çalışma alanı özel uç noktasının özel IP adresi ile instances.azureml.ms. Daha fazla bilgi için bkz. [özel DNS](https://docs.microsoft.com/azure/machine-learning/how-to-custom-dns?tabs=azure-cli) makalesi.
    
## <a name="next-steps"></a>Sonraki adımlar

* [İlk denemenizi çalıştırın](tutorial-1st-experiment-sdk-train.md)
* [Dosya depolama alanınızı anlık görüntülerle yedekleyin](../storage/files/storage-snapshots-files.md)
* [Güvenli ortamlarda çalışma](https://docs.microsoft.com/azure/machine-learning/how-to-secure-training-vnet#compute-instance)
