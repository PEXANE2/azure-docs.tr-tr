---
title: Yerel bir Jupyter not defterini Azure Dizüstü Bilgisayarlar Önizlemesine geçirin
description: Bir Jupyter not defterini yerel bilgisayarınızdan veya web URL'nizden Azure Not Defteri'ne hızla aktarın ve işbirliği için paylaşın.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: baf05d7adb1340d712ff0fc87436d5bbac51bc8f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77064334"
---
# <a name="quickstart-migrate-a-local-jupyter-notebook-in-azure-notebooks-preview"></a>Hızlı başlangıç: Azure Dizüstü Bilgisayarlar Önizleme'de yerel bir Jupyter dizüstü bilgisayarı geçirme

Bu hızlı başlangıçta, bir Jupyter not defterini yerel bilgisayarınızdan veya erişilebilir başka bir dosya URL'sini Azure Not Defterlerine geçirebilirsiniz. 

Kendi bilgisayarınızdaki jupyter dizüstü bilgisayarlara yalnızca sizin erişebilirsiniz. Dosyalarınızı paylaşabilirsiniz, ancak alıcılar daha sonra not defterinin kendi yerel kopyalarına sahiptir ve bu dosyaların değiştirilmesi ni birleştirmek zordur. Dizüstü bilgisayarları GitHub gibi paylaşılan bir çevrimiçi depoda saklamış olsanız bile, her ortak çalışanın sizinki gibi yapılandırılmış yerel bir Jupyter yüklemesi olmalıdır.

Yerel veya depo tabanlı not defterlerinizi Azure Not Defterlerine geçirerek, bunları dizüstü bilgisayarlarınızı görüntülemek ve çalıştırmak için yalnızca bir tarayıcıya ihtiyaç duyan ortak çalışanlarınızla anında paylaşabilirsiniz. Azure Notebook'larda oturum açsalar, değişiklik de yapabilir.

## <a name="prerequisites"></a>Ön koşullar

- Yerel bilgisayarınızda veya erişilebilir başka bir dosya URL'sinde [bir Jupyter dizüstü bilgisayar.](https://jupyter-notebook.readthedocs.io) 

## <a name="create-a-project-on-azure-notebooks"></a>Azure Dizüstü Bilgisayarlarda proje oluşturma

Bu hızlı başlatma, yerel bilgisayarınızdan veya erişilebilir başka bir dosya URL'sinden bir not defterini geçirilmeye neden olduğunu gösterir. Bir GitHub deposundan not defterlerini geçirmek için Bkz. [Quickstart: Bir dizüstü bilgisayarı klonlama.](quickstart-clone-jupyter-notebook.md)

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

1. Azure [Not Defterleri'ne](https://notebooks.azure.com) gidin ve oturum açın. (Ayrıntılar için [Bkz. Hızlı Başlangıç - Azure Not Defterlerinde Oturum Aç).](quickstart-sign-in-azure-notebooks.md)

1. Herkese açık profil sayfanızdan, sayfanın üst kısmında **Projelerim'i** seçin:

    ![Tarayıcı penceresinin üst kısmındaki Projelerim bağlantısı](media/quickstarts/my-projects-link.png)

1. **Projelerim** sayfasında Yeni **Proje** 'yi (klavye kısayolu: n) seçin. Düğme yalnızca tarayıcı **+** penceresi darsa gibi görünebilir:

    ![Projelerim sayfasında yeni Proje komutu](media/quickstarts/new-project-command.png)

1. Görünen **Yeni Proje Oluştur** açılır penceresinde, **Proje adı** ve **Proje Kimliği** alanlarında geçirdiğiniz not defteri için uygun değerleri girin, Ortak **proje** seçeneklerini temizleyin ve **README.md oluşturun,** ardından **Oluştur'u**seçin.

## <a name="upload-the-local-notebook"></a>Yerel not defterini yükleme

1. Proje sayfasında **Yükle'yi** seçin (yalnızca tarayıcı pencereniz küçükse yukarı ok olarak görünebilir), ardından 1'i seçin. Görünen açılır pencerede, not defteriniz yerel dosya sisteminizde bulunuyorsa **bilgisayardan** veya not defteriniz çevrimiçiyse **URL'den'i** seçin:

    ![Bir URL'den veya yerel bilgisayardan not defteri yükleme komutu](media/quickstarts/upload-from-computer-url-command.png)

   Yine, dizüstü bilgisayarınız Bir GitHub deposundaysa, Quickstart'taki adımları izleyin: Bunun yerine [bir dizüstü bilgisayarı klonlayın.](quickstart-clone-jupyter-notebook.md)

   - **Bilgisayardan** *kullanıyorsanız,.ipynb* dosyalarınızı sürükleyip açılır pencereye bırakın veya **Dosyaları Seç'i**seçin, ardından içe aktarıp almak istediğiniz dosyalara göz atın ve seçin. Ardından **Karşıya Yükle**’yi seçin. Yüklenen dosyalara yerel dosyalarla aynı ad verilir. *.ipynb_checkpoints* klasörlerinin içeriğini yüklemeniz gerekmez.

     ![Bilgisayar açılır penceresinden yükleme](media/quickstarts/upload-from-computer-popup.png)

   - **URL'den**kullanıyorsanız, **Dosya Adı** alanında projenizdeki not defterine atamak için **Dosya URL** alanına kaynak adresi ve dosya adını girin. Ardından **Karşıya Yükle**’yi seçin. Ayrı URL'lere sahip birden çok dosyanız varsa, girdiğiniz ilk URL'yi denetlemek için **Dosya Ekle** komutunu kullanın ve ardından açılır pencere başka bir dosya için yeni alanlar sağlar.

     ![URL açılır penceresinden yükleme](media/quickstarts/upload-from-url-popup.png)

1. İçeriğini ve çalışmasını doğrulamak için yeni yüklenen not defterinizi açın ve çalıştırın. İşi bittiğinde **Dosya** > **Durdur'u** seçin ve not defterini kapatmak için kapatın.

1. Yüklenen not defterinize bir bağlantı paylaşmak için projedeki dosyayı sağ tıklatın ve **Bağlantı Kopyala'yı** (klavye kısayolu: y) seçin ve ardından bu bağlantıyı uygun iletiye yapıştırın. Alternatif olarak, proje sayfasındaki **Paylaş** denetimini kullanarak projeyi bir bütün olarak paylaşabilirsiniz.

1. Not defteri dışındaki dosyaları yeniden deletmek için projedeki dosyayı sağ tıklatın ve **Dosyayı Edit'i** seçin (klavye kısayolu: i). Varsayılan eylem, **Çalıştır** (klavye kısayolu: r), yalnızca dosya içeriğini gösterir ve düzenlemeye izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Doğrusal regresyon yapmak için bir Jupyter dizüstü bilgisayar oluşturun ve çalıştırın](tutorial-create-run-jupyter-notebook.md)
