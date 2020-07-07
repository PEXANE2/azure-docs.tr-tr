---
title: Yerel bir Jupyter Not defterini Azure Notebooks önizlemeye geçirme
description: Bir Jupyter Not defterini hızlı bir şekilde yerel bilgisayarınızdan veya bir Web URL 'sinden Azure Notebooks önizlemeye aktarın ve işbirliği için paylaşabilirsiniz.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: 9c961ad67260cee2809e0bba1e79f2c709183dea
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85832126"
---
# <a name="quickstart-migrate-a-local-jupyter-notebook-in-azure-notebooks-preview"></a>Hızlı başlangıç: Azure Notebooks önizlemede yerel bir Jupyter Not defteri geçirme

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Bu hızlı başlangıçta, bir Jupyter Not defterini yerel bilgisayarınızdan veya Azure Notebooks başka bir erişilebilir dosya URL 'sinden geçirolursunuz. 

Kendi bilgisayarınızdaki jupi not defterlerine yalnızca sizin erişebiliyordu. Dosyalarınızı paylaşabilir, ancak alıcıların not defterinizin kendi yerel kopyaları vardır ve değişiklikleri eklemek zordur. Not defterlerini GitHub gibi paylaşılan bir çevrimiçi depoda depoluseniz bile, her ortak çalışan, kendi gibi yapılandırılmış bir yerel jupi yüklemesine sahip olmalıdır.

Yerel veya depo tabanlı not defterlerinizi Azure Notebooks geçirerek, yalnızca not defterlerinizi görüntüleyip çalıştırmak için bir tarayıcı gerektiren ortak çalışanlarla anında paylaşabilirsiniz. Azure Notebooks oturum açtıklarında de değişiklik yapabilirler.

## <a name="prerequisites"></a>Önkoşullar

- Yerel bilgisayarınızdaki veya başka bir erişilebilir dosya URL 'sindeki [Jupyter Not defteri](https://jupyter-notebook.readthedocs.io) . 

## <a name="create-a-project-on-azure-notebooks"></a>Azure Notebooks proje oluşturma

Bu hızlı başlangıçta, yerel bilgisayarınızdaki veya başka bir erişilebilir dosya URL 'sindeki bir not defteri geçişi gösterilmektedir. Not defterlerini bir GitHub deposundan geçirmek için bkz. [hızlı başlangıç: Not defterini kopyalama](quickstart-clone-jupyter-notebook.md).

1. [Azure Notebooks](https://notebooks.azure.com) gidin ve oturum açın. (Ayrıntılar için bkz. [hızlı başlangıç-Azure Notebooks Için oturum açma](quickstart-sign-in-azure-notebooks.md)).

1. Ortak profil sayfanızda sayfanın en üstündeki **Projelerim** ' nı seçin:

    ![Projelerim tarayıcı penceresinin üst kısmında bağlantı](media/quickstarts/my-projects-link.png)

1. **Projelerim** sayfasında **Yeni proje** ' yi seçin (klavye kısayolu: n). Düğme yalnızca **+** tarayıcı penceresi dar gibi görünebilir:

    ![Projelerim sayfasında yeni proje komutu](media/quickstarts/new-project-command.png)

1. Görüntülenen **Yeni proje oluştur** açılır penceresinde, **Proje adı** ve **proje kimliği** alanlarına geçirdiğiniz Not defteri için uygun değerleri girin, **ortak proje** seçeneklerini temizleyin ve **bir Readme.MD oluşturun**ve ardından **Oluştur**' u seçin.

## <a name="upload-the-local-notebook"></a>Yerel Not defterini karşıya yükle

1. Proje sayfasında **karşıya yükle** ' yi seçin (yalnızca tarayıcı pencereniz küçük olduğunda yukarı ok olarak görünebilir), sonra 1 ' i seçin. Görüntülenen açılan pencerede, Not defteriniz yerel dosya **sisteminizde veya Not defteriniz çevrimiçi olduğunda** **bilgisayardan** ' i seçin:

    ![Bir URL 'den veya yerel bilgisayardan Not defteri yükleme komutu](media/quickstarts/upload-from-computer-url-command.png)

   Not defteriniz bir GitHub deponuz olduğunda, [hızlı başlangıç: bir not defteri kopyalama](quickstart-clone-jupyter-notebook.md) bölümündeki adımları izleyin.

   - **Bilgisayardan**kullanıyorsanız, *. ipynb* dosyalarınızı sürükleyip açılan pencereye bırakın veya **dosyaları seç**' i seçin ve ardından içeri aktarmak istediğiniz dosyaları seçin. Ardından **Karşıya Yükle**’yi seçin. Karşıya yüklenen dosyalara yerel dosyalarla aynı ad verilir. Herhangi bir *. ipynb_checkpoints* klasörünün içeriğini karşıya yüklemeniz gerekmez.

     ![Açılan bilgisayardan karşıya yükle](media/quickstarts/upload-from-computer-popup.png)

   - **URL 'den**kullanıyorsanız, dosya **URL 'si** alanına kaynak adresini ve **dosya adı** alanına projenizdeki not defterine atamak için dosya adını girin. Ardından **Karşıya Yükle**’yi seçin. Ayrı URL 'Ler içeren birden çok dosya varsa, açılan pencerenin, başka bir dosya için yeni alanlar sağladığı ilk URL 'YI denetlemek için **Dosya Ekle** komutunu kullanın.

     ![URL açılan penceresinden karşıya yükle](media/quickstarts/upload-from-url-popup.png)

1. İçeriğini ve işlemini doğrulamak için karşıya yüklenen yeni not defterinizi açın ve çalıştırın. İşiniz bittiğinde, **File**  >  Not defterini kapatmak için dosya**Durdur ve Kapat** ' ı seçin.

1. Karşıya yüklenen Not defterinize bir bağlantı paylaşmak için, projedeki dosyaya sağ tıklayın ve **Bağlantıyı Kopyala** ' yı (klavye kısayolu: y) seçin ve ardından bu bağlantıyı uygun iletiye yapıştırın. Alternatif olarak, proje sayfasında **paylaşma** denetimini kullanarak projeyi bir bütün olarak paylaşabilirsiniz.

1. Not defterleri dışındaki dosyaları düzenlemek için, projedeki dosyaya sağ tıklayın ve **dosyayı Düzenle** ' yi seçin (klavye kısayolu: ı). Varsayılan eylem, **Çalıştır** (klavye kısayolu: r) yalnızca dosya içeriğini gösterir ve düzenlenmesine izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: doğrusal regresyon yapmak için bir Jupyter Not defteri oluşturma ve çalıştırma](tutorial-create-run-jupyter-notebook.md)
