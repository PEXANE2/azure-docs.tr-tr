---
title: Bir Azure not defterleri projesi ile özel bir ortam oluşturun
description: Yüklü paketleri başlatma komut dosyaları ve belirli bir kümesi ile yapılandırılmış bir Azure not defterlerinde yeni bir proje oluşturun.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: f167d1bf6064cde535d17fa4a336ee13424baba7
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277491"
---
# <a name="quickstart-create-a-project-with-a-custom-environment"></a>Hızlı Başlangıç: özel bir ortamla bir proje oluşturma

Azure not defterlerinde bir proje, not defterleri, veri dosyaları, belgeler, resimler ve özgü Kurulum komutları ile yapılandırılmış bir ortam ile birlikte vb. gibi dosyaları koleksiyonudur. Proje ortamıyla tanımlayarak, proje kendi Azure not defterleri hesaba klonlar herkes gerekli ortam yeniden oluşturmak ihtiyaç duydukları tüm bilgilere sahiptir.

## <a name="create-a-project"></a>Proje oluşturma

1. [Azure Notebooks](https://notebooks.azure.com) gidin ve oturum açın. (Ayrıntılar için bkz. [hızlı başlangıç-Azure Notebooks Için oturum açma](quickstart-sign-in-azure-notebooks.md)).

1. Ortak profil sayfanızda sayfanın en üstündeki **Projelerim** ' nı seçin:

    ![Projeleri bağlantımı tarayıcı penceresinin üst kısmındaki](media/quickstarts/my-projects-link.png)

1. **Projelerim** sayfasında **+ Yeni proje** ' yi seçin (klavye kısayolu: n); düğme yalnızca tarayıcı penceresi dar ise **+** olarak görünebilir:

    ![Projelerim sayfasında yeni proje komutu](media/quickstarts/new-project-command.png)

1. Görüntülenen **Yeni proje oluştur** açılır penceresinde aşağıdaki ayrıntıları girin veya ayarlayın, sonra **Oluştur**' u seçin.

    - **Proje adı**: özel bir ortama sahip proje
    - **Proje kimliği**: proje-özel-ortam
    - **Ortak proje**: (işaretsiz)
    - **README.MD oluşturma**: (işaretsiz)

1. Birkaç dakika sonra Azure not defterleri yeni projeye gider. **+ Yeni** açılan listesini (yalnızca **+** olarak görünebilir) seçip **Not defteri**' ni seçerek projeye bir not defteri ekleyin.

1. Not defterine özel ortam gibi bir ad verin *. ipynb*, dil için **Python 3,6** ' i seçin ve **Yeni**' yi seçin.

## <a name="add-a-custom-setup-step"></a>Özel Kurulum adımı ekleyin

1. Proje sayfasında **proje ayarları**' nı seçin.

    ![Proje ayarları komutu](media/quickstarts/project-settings-command.png)

1. **Proje ayarları** açılır penceresinde **ortam** sekmesini seçin ve ardından **ortam kurulum adımları**altında **+ Ekle**' yi seçin:

    ![Yeni bir ortam Kurulum adımı eklemek için komutu](media/quickstarts/environment-add-command.png)

1. **+ Add** komutu, bir işlem tarafından tanımlanan bir adım ve projenizdeki dosyalardan seçilen bir hedef dosya oluşturur. Aşağıdaki işlemler desteklenmektedir:

    | İşlem | Açıklama |
    | --- | --- |
    | Requirements.txt | Python projeleri requirements.txt dosyasında bağımlılıkları tanımlayın. Bu seçenek, projenin dosya listesinden uygun dosyayı seçin ve ayrıca ek görünen açılan Python sürümünü seçin. Gerekirse, projeye geri dönmek için **iptal** ' i seçin, dosyayı karşıya yükleyin veya oluşturun, ardından **proje ayarları** > **ortam** sekmesine dönüp yeni bir adım oluşturun. Bu adımla birlikte, projede bir not defterini çalıştırmak otomatik olarak çalıştırılır `pip install -r <file>` |
    | Kabuk betiği | Ortamı başlatmak için çalıştırmak istediğiniz komutları içeren bir bash kabuğu betiğini (genellikle *. sh* uzantılı bir dosya) göstermek için kullanın. |
    | Environment.yml | Bir ortamı yönetmek için Conda kullanan bir Python projesi, bağımlılıkları tanımlayacak *ortamlar. yıml* dosyasını kullanır. Bu seçenek belirtilmişse, projenin dosya listeden uygun dosyayı seçin. |

1. Herhangi bir kurulum adımını kaldırmak için adımın sağındaki **X** seçeneğini belirleyin.

1. Tüm kurulum adımları yerinde olduğunda, **Kaydet**' i seçin. (Değişiklikleri atmak için **iptal** ' i seçin).

1. Ortamınızı test etmek için yeni bir not defteri oluşturup çalıştırın, sonra, bir Python `import` deyimi kullanma gibi, ortamdaki bir pakete bağlı olan deyimlerle bir kod hücresi oluşturun. Deyim başarılı olursa, gerekli paket ortamda başarıyla yüklendi.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Notebooks projeleri yönetme ve yapılandırma](configure-manage-azure-notebooks-projects.md)

> [!div class="nextstepaction"]
> [Öğretici: doğrusal regresyon yapmak için bir Jupyter Not defteri çalıştırma](tutorial-create-run-jupyter-notebook.md)
