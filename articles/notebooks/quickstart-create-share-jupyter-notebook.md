---
title: Azure Notebooks önizlemede bir Jupyter Not defteri oluşturma ve paylaşma
description: Azure Notebooks önizlemede hızlı bir şekilde bir Jupyter Not defteri oluşturup çalıştırın ve ardından bu not defterini başkalarıyla paylaşabilirsiniz.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: d3310444fa28240b8fb1344199514a9601a2c615
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77064453"
---
# <a name="quickstart-create-and-share-a-notebook-in-azure-notebooks-preview"></a>Hızlı başlangıç: Azure Notebooks önizlemede bir not defteri oluşturma ve paylaşma

Bu hızlı başlangıçta Azure Notebooks bir Jupyter Not defteri oluşturup çalıştırın ve ardından bu not defterini başkalarıyla paylaşabilirsiniz. Jupyter, bir paylaşılabilir tuvalde Not defteri üzerinde markı metnini, çalıştırılabilir kodu, kalıcı verileri, grafikleri ve görselleştirmeleri kolayca birleştirebilmenizi sağlar. Azure Notebooks, yükleme yapmadan Jupyter not defterlerini geliştirmeye ve çalıştırmaya yönelik, ücretsiz ve barındırılan bir hizmettir.

## <a name="prerequisites"></a>Ön koşullar
Yok.

## <a name="create-a-new-project-and-notebook"></a>Yeni bir proje ve Not defteri oluşturun

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

1. [Azure Notebooks sitesine gidin (https://notebooks.azure.com) ](https://notebooks.azure.com) ve oturum açın. Ayrıntılar için bkz. [hızlı başlangıç-Azure Notebooks Için oturum açma](quickstart-sign-in-azure-notebooks.md).

1. Ortak profil sayfanızda sayfanın en üstündeki **Projelerim** ' nı seçin:

    ![Projelerim tarayıcı penceresinin üst kısmında bağlantı](media/quickstarts/my-projects-link.png)

1. **Projelerim** sayfasında **+ Yeni proje** ' yi seçin (klavye kısayolu: n). Düğme yalnızca tarayıcı penceresi dar gibi **+** görünebilir:

    ![Projelerim sayfasında yeni proje komutu](media/quickstarts/new-project-command.png)

1. Görüntülenen **Yeni proje oluştur** açılır penceresinde aşağıdaki ayrıntıları girin veya ayarlayın, sonra **Oluştur**' u seçin.

   - **Proje adı**: Python 'da Merhaba Dünya
   - **Proje kimliği**: Merhaba-Dünya-Python
   - **Ortak proje**: (işaretsiz)
   - **README.MD oluşturma**: (işaretsiz)

     ![Doldurulan ayrıntılarla yeni proje açılan penceresi](media/quickstarts/new-project-popup.png)

1. Birkaç dakika sonra Azure Notebooks yeni projeye götürür. **+ Yeni** açılan listesini seçerek (yalnızca **+** olarak görünebilen) ve ardından **Not defteri**' ni seçerek projeye bir not defteri ekleyin:

    [![](media/quickstarts/empty-project-new-notebook-button.png "A new, empty project and add notebook command")](media/quickstarts/empty-project-new-notebook-button.png#lightbox)

1. Görüntülenen **Yeni Not Defteri Oluştur** açılır penceresinde, Not defteriniz Için *Helloworldinpython. ipynb* (*. ipynb* ) gibi bir dosya adı girin ve dil için **Python 3,6** ' i ( *çekirdek*olarak da anılır) seçin:

    ![Yeni Not Defteri Oluştur açılan menüsü](media/quickstarts/new-notebook-popup.png)

1. Not defterini oluşturmak için **Yeni** ' yi seçin ve daha sonra projenizin dosya listesinde görünür:

    ![Projenin dosya listesinde görünen yeni not defteri](media/quickstarts/new-notebook-created.png)

## <a name="run-the-notebook"></a>Not defterini çalıştırma

1. Düzenleyicide çalıştırmak için yeni not defterini seçin; seçtiğiniz çekirdek (Bu örnekteki Python 3,6) Bu not defteri için otomatik olarak etkinleştirilir:

    ![Azure Notebooks yeni bir not defteri görüntüleme](media/quickstarts/create-notebook-first-open.png)

1. Varsayılan olarak, not defterinde bir boş kod hücresi bulunur. Hücre türünü **Markaşağı**olarak değiştirmek Için, **markaçýlan**' i seçmek için hücre türü açılır öğesini kullanın:

    ![Yeni bir not defterinde hücre türünü değiştirme](media/quickstarts/create-notebook-cell-type.png)

1. Aşağıdaki başlık metnini hücreye girin veya yapıştırın:

    ```markdown
    # Hello World in Python
    ```

1. Markaşağı düzenlemekte olduğunuz için metin, "#" ile bir başlık olarak görünür. Markı 'yi HTML olarak işlemek için **Çalıştır** düğmesini seçin. Azure Notebooks bundan sonra otomatik olarak yeni bir kod hücresi oluşturur:

    ![Bir hücre ve işlenmiş Markaşağı için Çalıştır düğmesi](media/quickstarts/run-cell-markdown-render.png)

1. Kod hücresine aşağıdaki python kodunu girin:

    ```python
    from datetime import datetime

    now = datetime.now()
    msg = "Hello, Azure Notebooks! Today is %s"  % now.strftime("%A, %d %B, %Y")
    print(msg)
    ```

1. Kodu çalıştırmak için **Çalıştır** ' ı (klavye kısayolu: Shift + Enter) seçin. Hücrenin altında şu metne benzer başarılı bir çıktı görmeniz gerekir:

    ```output
    Hello, Azure Notebooks! Today is Thursday, 15 November, 2018
    ```

1. Çalışmanızı kaydetmek için Kaydet simgesini seçin:

    ![Jupyter Not defteri araç çubuğunda Kaydet simgesi](media/quickstarts/hello-results-save-icon.png)

1. Sunucuyu durdurmak ve tarayıcı **penceresini kapatmak için****Kapat ve Durdur** menü komutunu seçin. > 

## <a name="share-the-notebook"></a>Not defterini paylaşma

Not defterinizi paylaşmak için, gerekirse proje sayfasına dönün, Not Defteri dosyasına sağ tıklayın, **Bağlantıyı Kopyala** (klavye kısayolu: y) seçeneğini belirleyin ve bu bağlantıyı uygun bir iletiye yapıştırın (e-posta, anlık ileti, vb.).

Proje sayfasında, bir bağlantı almak, bağlantıyla birlikte bir e-posta iletisi oluşturmak veya HTML ve Markaşağı ekleme kodunu almak için **paylaşma** menüsünü de kullanabilirsiniz:

![Proje paylaşma komutu](media/quickstarts/share-project-command.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: doğrusal regresyon yapmak için bir Jupyter Not defteri oluşturma ve çalıştırma](tutorial-create-run-jupyter-notebook.md)
