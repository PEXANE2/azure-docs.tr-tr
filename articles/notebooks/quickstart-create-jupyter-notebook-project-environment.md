---
title: Özel bir ortamla Azure Notebooks önizleme projesi oluşturma
description: Azure Notebooks önizlemede, belirli bir yüklü paketler ve başlatma betikleri kümesiyle yapılandırılmış yeni bir proje oluşturun.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: 999133dd7d9d792956f9a2c93ec218e458c921e8
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75647076"
---
# <a name="quickstart-create-a-project-with-a-custom-environment-in-azure-notebooks-preview"></a>Hızlı başlangıç: Azure Notebooks önizlemede özel ortamla bir proje oluşturma

Azure not defterlerinde bir proje, not defterleri, veri dosyaları, belgeler, resimler ve özgü Kurulum komutları ile yapılandırılmış bir ortam ile birlikte vb. gibi dosyaları koleksiyonudur. Proje ortamıyla tanımlayarak, proje kendi Azure not defterleri hesaba klonlar herkes gerekli ortam yeniden oluşturmak ihtiyaç duydukları tüm bilgilere sahiptir.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="create-a-project"></a>Proje oluşturma

1. Git [Azure not defterleri](https://notebooks.azure.com) ve oturum açın. (Ayrıntılar için bkz [hızlı başlangıç - Azure not defterleri için oturum açma](quickstart-sign-in-azure-notebooks.md)).

1. Genel profil sayfanızdan seçin **Projelerim** sayfanın üstündeki:

    ![Projeleri bağlantımı tarayıcı penceresinin üst kısmındaki](media/quickstarts/my-projects-link.png)

1. Üzerinde **Projelerim** sayfasında **+ yeni proje** (klavye kısayolu: n); yalnızca olarak görüntülenebilir **+** tarayıcı penceresini darsa:

    ![Projelerim sayfasında yeni proje komutu](media/quickstarts/new-project-command.png)

1. İçinde **yeni proje oluştur** görünen açılan girin veya aşağıdaki bilgileri ayarlayın ve ardından **Oluştur**:

    - **Proje adı**: Proje özel bir ortam ile
    - **Proje kimliği**: Proje özel ortam
    - **Genel proje**: (Seçili)
    - **Bir Benioku.MD oluşturma**: (Seçili)

1. Birkaç dakika sonra Azure not defterleri yeni projeye gider. Bir not defteri seçerek projeye ekleyin. **+ yeni** açılan (yalnızca görünen **+** ), sonra seçerek **not defteri**.

1. Not Defteri gibi bir ad verin *özel environment.ipynb*seçin **Python 3.6** dili ve seçin için **yeni**.

## <a name="add-a-custom-setup-step"></a>Özel Kurulum adımı ekleyin

1. Proje sayfasında **proje ayarları**.

    ![Proje ayarları komutu](media/quickstarts/project-settings-command.png)

1. İçinde **proje ayarları** açılan menüsü, select **ortam** sekmesinde altında **ortamı Kurulumu adımları**seçin **+ Ekle**:

    ![Yeni bir ortam Kurulum adımı eklemek için komutu](media/quickstarts/environment-add-command.png)

1. **+ Ekle** komut bir işlem ve projenize dosyalar seçtiğiniz hedef dosyası tarafından tanımlanan bir adım oluşturur. Aşağıdaki işlemler desteklenmektedir:

    | İşlem | Açıklama |
    | --- | --- |
    | Requirements.txt | Python projeleri requirements.txt dosyasında bağımlılıkları tanımlayın. Bu seçenek, projenin dosya listesinden uygun dosyayı seçin ve ayrıca ek görünen açılan Python sürümünü seçin. Gerekirse seçin **iptal** projeye döndürmek için karşıya yükleme veya dosya oluşturun, ardından dönmek **proje ayarları** > **ortam** sekmesini ve oluşturma bir Yeni adım. Bu adım bir yerde bir not defteri projesinde otomatik olarak çalıştırarak çalışır `pip install -r <file>` |
    | Kabuk betiği | Bir bash Kabuk betiği belirtmek için kullanın (genellikle bir dosyayla *.sh* uzantısı) ortamı başlatmak için çalıştırmak istediğiniz herhangi bir komut içerir. |
    | Environment.yml | Ortam yönetme kullanan conda kullanan bir Python proje bir *environments.yml* bağımlılıklarını tanımlamak için dosya. Bu seçenek belirtilmişse, projenin dosya listeden uygun dosyayı seçin. |

1. Herhangi bir kurulum adımı kaldırmak için işaretleyin **X** adım sağındaki.

1. Tüm kurulum adımlarını Tamam olduğunda seçin **Kaydet**. (Seçmek **iptal** değişiklikleri atmak için).

1. Ortamınızı test etmek için oluşturun ve yeni bir not defteri çalıştırma sonra bir Python kullanarak ortamında bir paket bağımlı deyimleri ile bir kod hücresi oluşturmak `import` deyimi. Deyim başarılı olursa, gerekli paket ortamda başarıyla yüklendi.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure not defterlerinde projeleri yapılandırma ve yönetme](configure-manage-azure-notebooks-projects.md)

> [!div class="nextstepaction"]
> [Öğretici: bir çalışma doğrusal regresyon yapmak için Jupyter not defteri oluşturma](tutorial-create-run-jupyter-notebook.md)
