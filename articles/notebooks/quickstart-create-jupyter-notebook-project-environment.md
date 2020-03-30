---
title: Özel bir ortamla Azure Not Defterleri Önizleme projesi oluşturun
description: Azure Not Defteri Önizleme'de belirli bir yüklü paket ler ve başlangıç komut dosyalarıyla yapılandırılan yeni bir proje oluşturun.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: 6388cb7997cac5bef25975043a13c4e080f288d4
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78196850"
---
# <a name="quickstart-create-a-project-with-a-custom-environment-in-azure-notebooks-preview"></a>Hızlı başlatma: Azure Not Defterleri Önizleme'de özel bir ortama sahip bir proje oluşturma

Azure Not Defterleri'ndeki bir proje, belirli kurulum komutlarıyla yapılandırılabilen bir ortamla birlikte not defterleri, veri dosyaları, belgeler, resimler gibi dosyaların bir koleksiyonudur. Projeyle ortamı tanımlayarak, projeyi kendi Azure Dizüstü Bilgisayar hesabına klonlayan herkes, gerekli ortamı yeniden oluşturmak için gereken tüm bilgilere sahiptir.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="create-a-project"></a>Proje oluşturma

1. Azure [Not Defterleri'ne](https://notebooks.azure.com) gidin ve oturum açın. (Ayrıntılar için [Bkz. Hızlı Başlangıç - Azure Not Defterlerinde Oturum Aç).](quickstart-sign-in-azure-notebooks.md)

1. Herkese açık profil sayfanızdan, sayfanın üst kısmında **Projelerim'i** seçin:

    ![Tarayıcı penceresinin üst kısmındaki Projelerim bağlantısı](media/quickstarts/my-projects-link.png)

1. **Projelerim** sayfasında + **Yeni Proje 'yi** (klavye kısayolu: n); düğmesi yalnızca tarayıcı **+** penceresi darsa gibi görünebilir:

    ![Projelerim sayfasında yeni Proje komutu](media/quickstarts/new-project-command.png)

1. Aşağıdaki ayrıntıları görünen, giren veya ayarlayan **Yeni Proje Oluştur** açılır penceresinde Oluştur seçeneğini **belirleyin:**

    - **Proje adı**: Özel bir ortama sahip proje
    - **Project ID**: proje-özel ortam
    - **Kamu projesi**: (temizlendi)
    - **bir README.md oluşturun**: (temizlendi)

1. Birkaç dakika sonra Azure Dizüstü Bilgisayarlar sizi yeni projeye yönlendirer. **+ Yeni** açılır (yalnızca **+** olarak görünebilir) seçeneğini seçerek projeye bir not defteri ekleyin ( sonra Not **Defteri'ni**seçin.

1. Not defterine *Özel environment.ipynb*gibi bir ad verin, dil için **Python 3.6'yı** seçin ve **Yeni'yi**seçin.

## <a name="add-a-custom-setup-step"></a>Özel kurulum adımı ekleme

1. Proje sayfasında Proje **Ayarları'nı**seçin.

    ![Proje ayarları komutu](media/quickstarts/project-settings-command.png)

1. Proje **Ayarları** açılır açılır, **Çevre** sekmesini seçin, sonra **Çevre Kurulum Adımları**altında , seçin + **Ekle**:

    ![Yeni bir ortam kurulum adımı eklemek için komut](media/quickstarts/environment-add-command.png)

1. **+ Ekle** komutu, bir işlem tarafından tanımlanan bir adım ve projenizdeki dosyalardan seçilen bir hedef dosyası oluşturur. Aşağıdaki işlemler desteklenir:

   | İşlem | Açıklama |
   | --- | --- |
   | Gereksinimler.txt | Python projeleri gereksinimlerini tanımlar.txt dosyasında. Bu seçenekle, projenin dosya listesinden uygun dosyayı seçin ve görünen ek açılır durumda Python sürümünü de seçin. Gerekirse, projeye dönmek, dosyayı yüklemek veya oluşturmak için **İptal'i** seçin, ardından **Proje Ayarları** > **Ortamı** sekmesine dönün ve yeni bir adım oluşturun. Bu adım ın devreye sokulduğunda, projede bir not defterinin çalıştırıl`pip install -r <file>` |
   | Kabuk komut dosyası | Ortamı niçin çalıştırmak istediğiniz komutları içeren bir bash kabuk komutunu (genellikle *.sh* uzantılı bir dosya) belirtmek için kullanın. |
   | Çevre.yml | Bir ortamı yönetmek için conda kullanan bir Python projesi bağımlılıkları açıklamak için bir *ortam.yml* dosyası kullanır. Bu seçenekle, projenin dosya listesinden uygun dosyayı seçin. |

   > [!WARNING]
   > Bu, geliştirilmekte olan bir önizleme hizmeti olduğundan, `Environment.yml` şu anda ayarın beklendiği gibi projenize uygulanmadığı bilinen bir sorun vardır. Proje ve içindeki Jupyter dizüstü bilgisayarlar şu anda belirtilen ortam dosyasını yüklemez.

1. Herhangi bir kurulum adımını kaldırmak için adımın sağındaki **X'i** seçin.

1. Tüm kurulum adımları yerindeyken **Kaydet'i**seçin. (Değişiklikleri atmak için **İptal'i** seçin).

1. Ortamınızı sınamak için yeni bir not defteri oluşturun ve çalıştırın, ardından Python `import` deyimi kullanmak gibi ortamdaki pakete bağlı ifadeler içeren bir kod hücresi oluşturun. İfade başarılı olursa, gerekli paket ortama başarıyla yüklenmiş olur.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Dizüstü Bilgisayarlar'da projeleri yönetme ve yapılandırma](configure-manage-azure-notebooks-projects.md)

> [!div class="nextstepaction"]
> [Öğretici: doğrusal regresyon yapmak için bir Jupyter dizüstü çalıştırmak oluşturmak](tutorial-create-run-jupyter-notebook.md)
