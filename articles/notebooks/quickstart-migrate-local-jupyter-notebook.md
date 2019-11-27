---
title: Yerel Jupyter not defterini Azure not defterleri için geçirme
description: Hızlı bir şekilde Azure not defterleri için yerel bilgisayarınıza veya bir web URL'si bir Jupyter not defteri aktarılması, sonra işbirliği için paylaşın.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: 3b7d5aab05923fb4356e0d45aaf21a77076a870a
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277476"
---
# <a name="quickstart-migrate-a-local-jupyter-notebook"></a>Hızlı Başlangıç: yerel Jupyter not defterini geçirme

Kendi bilgisayarınızda yerel olarak oluşturduğunuz Jupyter not defterleri yalnızca sizin erişilebilir. Dosyalarınızı anlamına gelir, çeşitli paylaşabilirsiniz, ancak ardından alıcılar not defteri kendi yerel kopyasına sahip ve neden olabilecek tüm değişiklikleri birleştirmek için zordur. GitHub gibi paylaşılan çevrimiçi depoda not defterlerini depolayabilir, ancak bunun yapılması yine de her bir ortak çalışanı kendi yerel Jupyter yükleme sizinkiyle aynı yapılandırmaya sahip olmasını gerektirir.

Azure not defterleri için yerel ya da depo tabanlı not defterlerinizi geçiş yaparak, bunları içinden anında, ortak çalışanlarla paylaşmanız bulutta depolar. Bu ortak çalışanlarla, Not defterinizi görüntülemek ve çalıştırmak için yalnızca bir tarayıcı gerekir ve bu kişiler Azure Notebooks [oturum](quickstart-sign-in-azure-notebooks.md) açtıklarında de değişiklik yapabilirler.

Bu hızlı başlangıçta bir not defteri yerel bilgisayarınıza veya başka bir ulaşılabilir dosya URL'si geçirme işlemi gösterilmektedir. Not defterlerini bir GitHub deposundan geçirmek için bkz. [hızlı başlangıç: Not defterini kopyalama](quickstart-clone-jupyter-notebook.md).

## <a name="create-a-project-on-azure-notebooks"></a>Azure not defterleri ile ilgili bir proje oluşturun

1. [Azure Notebooks](https://notebooks.azure.com) gidin ve oturum açın. (Ayrıntılar için bkz. [hızlı başlangıç-Azure Notebooks Için oturum açma](quickstart-sign-in-azure-notebooks.md)).

1. Ortak profil sayfanızda sayfanın en üstündeki **Projelerim** ' nı seçin:

    ![Projeleri bağlantımı tarayıcı penceresinin üst kısmındaki](media/quickstarts/my-projects-link.png)

1. **Projelerim** sayfasında **+ Yeni proje** ' yi seçin (klavye kısayolu: n); düğme yalnızca tarayıcı penceresi dar ise **+** olarak görünebilir:

    ![Projelerim sayfasında yeni proje komutu](media/quickstarts/new-project-command.png)

1. Görüntülenen **Yeni proje oluştur** açılır penceresinde, **Proje adı** ve **proje kimliği** alanlarına geçirdiğiniz Not defteri için uygun değerleri girin, **ortak proje** seçeneklerini temizleyin ve **bir Readme.MD oluşturun**ve ardından **Oluştur**' u seçin.

## <a name="upload-the-local-notebook"></a>Yerel not defterini karşıya yükleme

1. Proje sayfasında **karşıya yükle** ' yi seçin (yalnızca tarayıcı pencereniz küçük olduğunda yukarı ok olarak görünebilir), sonra 1 ' i seçin. Görüntülenen açılan pencerede, Not defteriniz yerel dosya **sisteminizde veya Not defteriniz çevrimiçi olduğunda** **bilgisayardan** ' i seçin:

    ![Bir URL veya yerel bilgisayar bir not defterini karşıya yüklemek için komutu](media/quickstarts/upload-from-computer-url-command.png)

   (Not defteriniz bir GitHub deponuz olduğunda, [hızlı başlangıç: bir not defteri kopyalama](quickstart-clone-jupyter-notebook.md) bölümündeki adımları izleyin.)

   - **Bilgisayardan**kullanıyorsanız, *. ipynb* dosyalarınızı sürükleyip açılan pencereye bırakın veya **dosyaları seç**' i seçin ve ardından içeri aktarmak istediğiniz dosyaları seçin. Ardından **karşıya yükle**' yi seçin. Karşıya yüklenen dosyalar olan yerel dosyalarla aynı adı verilir. (Herhangi bir *. ipynb_checkpoints* klasörünün içeriğini karşıya yüklemeniz gerekmez.)

     ![Bilgisayar açılır penceresinden karşıya yükleme](media/quickstarts/upload-from-computer-popup.png)

   - **URL 'den**kullanıyorsanız, dosya **URL 'si** alanına kaynak adresini ve **dosya adı** alanına projenizdeki not defterine atamak için dosya adını girin. Ardından **karşıya yükle**' yi seçin. Ayrı URL 'Ler içeren birden çok dosyanız varsa, açılan pencerenin, daha sonra başka bir dosya için yeni alanlar sağladığı ilk URL 'YI denetlemek için **+ Dosya Ekle** komutunu kullanın.

     ![URL açılır penceresinden karşıya yükleme](media/quickstarts/upload-from-url-popup.png)

1. Açın ve içeriğini ve işlem doğrulamak için yeni yüklenen defterinizin çalıştırın. İşiniz bittiğinde, Not defterini kapatmak için **dosya** > **Durdur ve Kapat** ' ı seçin.

1. Karşıya yüklenen Not defterinize bir bağlantı paylaşmak için, projedeki dosyaya sağ tıklayın ve **Bağlantıyı Kopyala** ' yı (klavye kısayolu: y) seçin ve ardından bu bağlantıyı uygun iletiye yapıştırın. Alternatif olarak, proje sayfasında **paylaşma** denetimini kullanarak projeyi bir bütün olarak paylaşabilirsiniz.

1. Not defterleri dışındaki dosyaları düzenlemek için, projedeki dosyaya sağ tıklayın ve **dosyayı Düzenle** ' yi seçin (klavye kısayolu: ı). Varsayılan eylem, **Çalıştır** (klavye kısayolu: r) yalnızca dosya içeriğini gösterir ve düzenlenmesine izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: doğrusal regresyon yapmak için bir Jupyter Not defteri çalıştırma](tutorial-create-run-jupyter-notebook.md)
