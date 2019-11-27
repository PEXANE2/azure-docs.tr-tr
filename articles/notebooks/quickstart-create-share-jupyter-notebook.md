---
title: Oluşturma ve Azure üzerinde Jupyter notebook paylaşma
description: Hızlı bir şekilde oluşturun ve Azure not defterleri ile ilgili bir Jupyter not defteri çalıştırın, sonra bu not defterini başkalarıyla paylaşın.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: 71220fa5aa0367d1cb1694582b4f96459a3016e7
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277513"
---
# <a name="quickstart-create-and-share-a-notebook"></a>Hızlı Başlangıç: Oluşturma ve bir not defteri paylaşın

1. [Azure Notebooks](https://notebooks.azure.com) gidin ve oturum açın. (Ayrıntılar için bkz. [hızlı başlangıç-Azure Notebooks Için oturum açma](quickstart-sign-in-azure-notebooks.md)).

1. Ortak profil sayfanızda sayfanın en üstündeki **Projelerim** ' nı seçin:

    ![Projeleri bağlantımı tarayıcı penceresinin üst kısmındaki](media/quickstarts/my-projects-link.png)

1. **Projelerim** sayfasında **+ Yeni proje** ' yi seçin (klavye kısayolu: n); düğme yalnızca tarayıcı penceresi dar ise **+** olarak görünebilir:

    ![Projelerim sayfasında yeni proje komutu](media/quickstarts/new-project-command.png)

1. Görüntülenen **Yeni proje oluştur** açılır penceresinde aşağıdaki ayrıntıları girin veya ayarlayın, sonra **Oluştur**' u seçin.

   - **Proje adı**: Python 'da Merhaba Dünya
   - **Proje kimliği**: Merhaba-Dünya-Python
   - **Ortak proje**: (işaretsiz)
   - **README.MD oluşturma**: (işaretsiz)

     ![Yeni Proje açılır penceresi ile doldurulmuş ayrıntıları](media/quickstarts/new-project-popup.png)

1. Birkaç dakika sonra Azure not defterleri yeni projeye gider. **+ Yeni** açılan listesini (yalnızca **+** olarak görünebilir) seçip **Not defteri**' ni seçerek projeye bir not defteri ekleyin:

    [![](media/quickstarts/empty-project-new-notebook-button.png "A new, empty project and add notebook command")](media/quickstarts/empty-project-new-notebook-button.png#lightbox)

1. Görüntülenen **Yeni Not Defteri Oluştur** açılır penceresinde, Not defteriniz Için *Helloworldinpython. ipynb* ( *. ipynb* ) gibi bir dosya adı girin ve dil için **Python 3,6** ' i ( *çekirdek*olarak da anılır) seçin:

    ![Yeni Not Defteri Oluştur açılan menüsü](media/quickstarts/new-notebook-popup.png)

1. Not defterini oluşturmak için **Yeni** ' yi seçin ve daha sonra projenizin dosya listesinde görünür:

    ![Projenin dosya listesinde görünen yeni not defteri](media/quickstarts/new-notebook-created.png)

## <a name="run-the-notebook"></a>Not defterini çalıştırma

1. Yeni Not Defteri Düzenleyicisi'nde çalıştırmak için seçin. (Bu örnekte Python 3.6) seçtiğiniz çekirdek bu not defteri için otomatik olarak etkinleştirilir:

    ![Azure not defterleri yeni bir not defteri görünümü](media/quickstarts/create-notebook-first-open.png)

1. Varsayılan olarak, Not defterini bir boş kodu hücreyi vardır. Hücre türünü **Markaşağı**olarak değiştirmek Için, **markaçýlan**' i seçmek için hücre türü açılır öğesini kullanın:

    ![Yeni bir not defterinde hücresi türü değiştirme](media/quickstarts/create-notebook-cell-type.png)

1. Hücreye aşağıdaki başlık metnini yapıştırın veya girin:

    ```markdown
    # Hello World in Python
    ```

1. Markdown düzenlediğiniz çünkü metin "#" ile bir üstbilgi görünür. Markı 'yi HTML olarak işlemek için **Çalıştır** düğmesini seçin. Azure not defterleri sonra otomatik olarak oluşturur Yeni bir kod hücresi daha sonra:

    ![Bir hücreyi ve işlenmiş Markdown Çalıştır düğmesi](media/quickstarts/run-cell-markdown-render.png)

1. Kod hücresine aşağıdaki Python kodu girin:

    ```python
    from datetime import datetime

    now = datetime.now()
    msg = "Hello, Azure Notebooks! Today is %s"  % now.strftime("%A, %d %B, %Y")
    print(msg)
    ```

1. Kodu çalıştırmak için **Çalıştır** ' ı (klavye kısayolu: Shift + Enter) seçin. Bir hücrede aşağıdaki metne benzer başarılı bir çıktı görmeniz gerekir:

    ```output
    Hello, Azure Notebooks! Today is Thursday, 15 November, 2018
    ```

1. Ve kayıt simgesini çalışmanızı kaydetmek için:

    ![Jupyter not defteri araç Kaydet simgesine](media/quickstarts/hello-results-save-icon.png)

1. Sunucuyu durdurmak ve tarayıcı penceresini kapatmak için **dosya** > **Kapat ve Durdur** menü komutunu seçin.

## <a name="share-the-notebook"></a>Not defterini paylaşın

Not defterinizi paylaşmak için, gerekirse proje sayfasına dönün, Not Defteri dosyasına sağ tıklayın, **Bağlantıyı Kopyala** (klavye kısayolu: y) seçeneğini belirleyin ve bu bağlantıyı uygun bir iletiye yapıştırın (e-posta, anlık ileti, vb.).

Proje sayfasında, bir bağlantı almak, bağlantıyla birlikte bir e-posta iletisi oluşturmak veya HTML ve Markaşağı ekleme kodunu almak için **paylaşma** menüsünü de kullanabilirsiniz:

![Proje paylaşımı komutu](media/quickstarts/share-project-command.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: doğrusal regresyon yapmak için bir Jupyter Not defteri çalıştırma](tutorial-create-run-jupyter-notebook.md)
