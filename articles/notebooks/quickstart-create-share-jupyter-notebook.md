---
title: Azure Dizüstü Bilgisayarlar Önizleme'de bir Jupyter dizüstü bilgisayar oluşturun ve paylaşın
description: Azure Notebook Önizleme'de bir Jupyter dizüstü bilgisayar oluşturun ve çalıştırın ve ardından bu not defterini başkalarıyla paylaşın.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: d3310444fa28240b8fb1344199514a9601a2c615
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77064453"
---
# <a name="quickstart-create-and-share-a-notebook-in-azure-notebooks-preview"></a>Hızlı başlangıç: Azure Not Defterleri Önizleme'de bir not defteri oluşturun ve paylaşın

Bu hızlı başlangıçta, Azure Not Defterlerinde bir Jupyter dizüstü bilgisayar oluşturup çalıştırırsınız, ardından bu not defterini başkalarıyla paylaşırsınız. Jupyter, Markdown metnini, çalıştırılabilir kodu, kalıcı verileri, grafikleri ve görselleştirmeleri tek bir sharable tuvalde, not defterinde kolayca birleştirmenizi sağlar. Azure Notebooks, yükleme yapmadan Jupyter not defterlerini geliştirmeye ve çalıştırmaya yönelik, ücretsiz ve barındırılan bir hizmettir.

## <a name="prerequisites"></a>Ön koşullar
Yok.

## <a name="create-a-new-project-and-notebook"></a>Yeni bir proje ve not defteri oluşturma

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

1. [Azure Not Defterleri sitesinehttps://notebooks.azure.com) ](https://notebooks.azure.com) gidin (ve oturum açın). Ayrıntılar için [Quickstart - Azure Not Defterlerinde oturum açın.](quickstart-sign-in-azure-notebooks.md)

1. Herkese açık profil sayfanızdan, sayfanın üst kısmında **Projelerim'i** seçin:

    ![Tarayıcı penceresinin üst kısmındaki Projelerim bağlantısı](media/quickstarts/my-projects-link.png)

1. **Projelerim** sayfasında + **Yeni Proje** (klavye kısayolu: n) seçeneğini belirleyin. Düğme yalnızca tarayıcı **+** penceresi darsa gibi görünebilir:

    ![Projelerim sayfasında yeni Proje komutu](media/quickstarts/new-project-command.png)

1. Aşağıdaki ayrıntıları görünen, giren veya ayarlayan **Yeni Proje Oluştur** açılır penceresinde Oluştur seçeneğini **belirleyin:**

   - **Proje Adı**: Python'da Merhaba Dünya
   - **Proje Kimliği**: hello-world-python
   - **Kamu projesi**: (temizlendi)
   - **bir README.md oluşturun**: (temizlendi)

     ![Doldurulan ayrıntıları içeren yeni Proje açılır](media/quickstarts/new-project-popup.png)

1. Birkaç dakika sonra Azure Dizüstü Bilgisayarlar sizi yeni projeye yönlendirer. **+ Yeni** açılır (yalnızca olarak görünebilir) seçeneğini seçerek projeye **+** bir not defteri ekleyin (yalnızca olarak görünebilir), ardından **Not Defteri'ni**seçin:

    [![](media/quickstarts/empty-project-new-notebook-button.png "A new, empty project and add notebook command")](media/quickstarts/empty-project-new-notebook-button.png#lightbox)

1. Görünen **Yeni Not Defteri Oluştur** açılır penceresinde, not defterinizin *helloworldinpython.ipynb* (*.ipynb* ironpython (Jupyter) Notebook anlamına gelir) gibi bir dosya adı girin ve dil için **Python 3.6'yı** *(çekirdek*olarak da adlandırılır) seçin:

    ![Yeni Not Defteri Oluşturma açılır](media/quickstarts/new-notebook-popup.png)

1. Projenizin dosya listesinde görünen not defterini oluşturmak için **Yeni'yi** seçin:

    ![Projenin dosya listesinde görünen yeni not defteri](media/quickstarts/new-notebook-created.png)

## <a name="run-the-notebook"></a>Not defterini çalıştırma

1. Editörde çalıştırmak için yeni not defterini seçin; seçtiğiniz çekirdek (Bu örnekte Python 3.6) bu not defteri için otomatik olarak etkinleştirilir:

    ![Azure Dizüstü Bilgisayarlarda yeni bir not defteri görünümü](media/quickstarts/create-notebook-first-open.png)

1. Varsayılan olarak, not defterinde bir boş kod hücresi vardır. Hücre türünü **Markdown**olarak değiştirmek için, **Markdown'u**seçmek için hücre türünü açılır kullanın:

    ![Yeni bir not defterinde hücre türünü değiştirme](media/quickstarts/create-notebook-cell-type.png)

1. Hücreye aşağıdaki başlık metnini girin veya yapıştırın:

    ```markdown
    # Hello World in Python
    ```

1. Markdown'u düzenlediğiniz için metin "#" ile bir üstbilgi olarak görünür. Markdown'u HTML'ye dönüştürmek için **Çalıştır** düğmesini seçin. Azure Not Defterleri daha sonra otomatik olarak yeni bir kod hücresi oluşturur:

    ![Bir hücre için çalıştırma düğmesi ve Markdown render](media/quickstarts/run-cell-markdown-render.png)

1. Kod hücresine aşağıdaki Python kodunu girin:

    ```python
    from datetime import datetime

    now = datetime.now()
    msg = "Hello, Azure Notebooks! Today is %s"  % now.strftime("%A, %d %B, %Y")
    print(msg)
    ```

1. Kodu çalıştırmak için **Çalıştır** 'ı (klavye kısayolu: Shift+Enter) seçeneğini belirleyin. Hücrenin altında aşağıdaki metne benzer başarılı çıktı görmelisiniz:

    ```output
    Hello, Azure Notebooks! Today is Thursday, 15 November, 2018
    ```

1. Çalışmanızı kaydetmek için kaydet simgesini seçin:

    ![Jupyter dizüstü araç çubuğundaki simgeyi kaydet](media/quickstarts/hello-results-save-icon.png)

1. Sunucuyu durdurmak ve tarayıcı penceresini kapatmak için **Dosya** > **Kapat ve Durdur** menüsü komutunu seçin.

## <a name="share-the-notebook"></a>Not defterini paylaşma

Not defterinizi paylaşmak için gerekirse proje sayfasına geri dön, not defteri dosyasına sağ tıklayın, **Bağlantı Yı** kolun (klavye kısayolu: y) seçin ve bu bağlantıyı uygun bir iletiye (e-posta, Anlık Ileti, vb.) yapıştırın.

Proje sayfasında, bağlantı edinmek, bağlantıyla birlikte bir e-posta iletisi oluşturmak veya HTML ve Markdown gömme kodu almak için **Paylaş** menüsünü de kullanabilirsiniz:

![Proje payı komutu](media/quickstarts/share-project-command.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Doğrusal regresyon yapmak için bir Jupyter dizüstü bilgisayar oluşturun ve çalıştırın](tutorial-create-run-jupyter-notebook.md)
