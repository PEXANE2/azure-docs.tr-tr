---
title: Azure portalından Chef istemcisini yükleme
description: Azure portalından Şef istemcinizi nasıl dağıtıp yapılandırılamayı öğrenin
keywords: azure, şef, devops, istemci, yükleme, portal
ms.date: 02/22/2020
ms.topic: article
ms.openlocfilehash: 6e46133c598c44b314077f2d020852416d3d2745
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77586368"
---
# <a name="install-the-chef-client-from-the-azure-portal"></a>Azure portalından Chef istemcisini yükleme
Chef istemci uzantısını doğrudan Azure portalından bir Linux veya Windows makinesine ekleyebilirsiniz. Bu makale, yeni bir Linux sanal makine kullanarak süreç boyunca size yol.

## <a name="prerequisites"></a>Ön koşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **Şef**: Aktif bir Chef hesabınız [yoksa, Hosted Chef'in ücretsiz deneme sürümüne](https://manage.chef.io/signup)kaydolun. Bu makaledeki talimatları takip etmek için Şef hesabınızdan aşağıdaki değerlere ihtiyacınız olacak:
  - organization_validation anahtarı
  - Rb
  - run_list

## <a name="install-the-chef-extension-on-a-new-linux-virtual-machine"></a>Chef uzantısını yeni bir Linux sanal makinesine yükleyin
Bu bölümde, önce bir Linux makinesi oluşturmak için Azure portalını kullanırsınız. İşlem sırasında, Şef uzantısını yeni sanal makineye nasıl yükleyeceğinizi de görürsünüz.

1. [Azure portalına](https://portal.azure.com)göz atın.

1. Soldaki menüden **Sanal makineler** seçeneğini seçin. Sanal **makineler** seçeneği yoksa, Tüm **hizmetleri** seçin ve ardından **Sanal makineleri**seçin.

1. Sanal **makineler** sekmesinde **Ekle'yi**seçin.

    ![Azure portalına yeni bir sanal makine ekleme](./media/chef-extension-portal/add-vm.png)

1. **İşlem** sekmesinde, istediğiniz işletim sistemini seçin. Bu demo için **Ubuntu Server** seçilir.

1. **Ubuntu Server** **sekmesinde, Ubuntu Server 16.04 LTS'yi**seçin.

    ![Bir Ubuntu sanal makinesi oluştururken, ihtiyacınız olan sürümü belirtin](./media/chef-extension-portal/ubuntu-server-version.png)

1. **Ubuntu Server 16.04 LTS** sekmesinde **Oluştur'u**seçin.

    ![Ubuntu kendi ürün hakkında ek bilgi sağlar](./media/chef-extension-portal/create-vm.png)

1. Sanal **makine oluştur** sekmesinde **Temeller'i**seçin.

1. Temel **bilgiler** sekmesinde aşağıdaki değerleri belirtin ve ardından **Tamam'ı**seçin.

   - **Ad** - Yeni sanal makine için bir ad girin.
   - **VM disk türü** - Depolama diski türü için **SSD** veya **HDD** belirtin. Azure'daki sanal makine disk türleri hakkında [Select a disk type](../virtual-machines/windows/disks-types.md)daha fazla bilgi için bkz.
   - **Kullanıcı adı** - Sanal makinede yönetici ayrıcalıkları verilen bir kullanıcı adı girin.
   - **Kimlik doğrulama türü** - **Parola'yı**seçin. Ayrıca **SSH ortak anahtarını**seçebilir ve bir SSH ortak anahtar değeri sağlayabilirsiniz. Bu demo (ve ekran görüntüleri) **amacıyla, Parola** seçilir.
   - **Parola** ve **Onayla -** Kullanıcı için bir parola girin.
   - **Azure Active Directory ile giriş yapın** - **Devre Dışı'yı**seçin.
   - **Abonelik** - Birden fazla aboneliğiniz varsa istediğiniz Azure aboneliğini seçin.
   - **Kaynak grubu** - Kaynak grubunuz için bir ad girin.
   - **Konum** - **Doğu ABD'yi**seçin.

     ![Sanal makine oluşturmak için temeller sekmesi](./media/chef-extension-portal/add-vm-basics.png)

1. Boyut **sekmesini** seç'te, sanal makinenin boyutunu seçin ve sonra **Seç'i**seçin.

1. **Ayarlar** sekmesinde, değerlerin çoğu önceki sekmelerde seçtiğiniz değerlere göre sizin için doldurulur. **Uzantılar**'ı seçin.

     ![Uzantılar Ayarlar sekmesi aracılığıyla sanal makinelere eklenir](./media/chef-extension-portal/add-vm-select-extensions.png)

1. **Uzantılar** **sekmesinde, uzantı ekle'yi**seçin.

     ![Sanal makineye uzantı eklemek için uzantı ekle'yi seçin](./media/chef-extension-portal/add-vm-add-extension.png)

1. Yeni **kaynak** sekmesinde **Linux Chef Extension(1.2.3)** seçeneğini belirleyin.

     ![Chef Linux ve Windows sanal makineler için uzantıları vardır](./media/chef-extension-portal/select-linux-chef-extension.png)

1. Linux **Chef Extension** sekmesinde **Oluştur'u**seçin.

1. **Uzantıyı Yükle** sekmesinde, aşağıdaki değerleri belirtin ve sonra **Tamam'ı**seçin.

    - **Chef Server URL** ' i - Kuruluş adını içeren *https://api.chef.io/organization/mycompany*Chef Server URL'sini girin, örneğin.
    - **Chef Node adı** - Chef Node adını girin.
    - **Çalıştırma listesi** - Makineye eklenen Chef çalıştırıl listesine girin. Bu değer boş bırakılabilir.
    - **Doğrulama istemci adı** - Chef Doğrulama İstemci Adını girin. örneğin, `tarcher-validator`.
    - **Doğrulama Anahtarı** - Makinelerinizi önyükleme yaparken kullanılan doğrulama anahtarını içeren bir dosya seçin.
    - **İstemci yapılandırma dosyası** - Şef-istemci için bir yapılandırma dosyası seçin. Bu değer boş bırakılabilir.
    - **Chef Client sürümü** - Yüklemek için şef istemci sürümü girin. Bu değer, en son sürümü yükler boş bırakılabilir.
    - **SSL Doğrulama Modu** - **Yok** veya **Eş'i**seçin. *Demo* için hiçbiri seçilmedi.
    - **Şef Çevre** - Şef çevre girin bu düğüm üyesi olmalıdır. Bu değer boş bırakılabilir.
    - **Şifreli veri torbası gizli** - Bu makinenin erişmesi gereken şifreli veri torbası nın sırrını içeren bir dosya seçin. Bu değer boş bırakılabilir.
    - **Chef Server SSL sertifikası** - Chef Server'ınıza atanan SSL sertifikasını seçin. Bu değer boş bırakılabilir.

      ![Chef Server'ı Linux sanal makinesine yükleme](./media/chef-extension-portal/install-extension.png)

1. **Uzantılar** sekmesi görüntülendiğinde **Tamam'ı**seçin.

1. **Ayarlar** sekmesi görüntülendiğinde **Tamam'ı**seçin.

1. **Oluştur** sekmesi görüntülendiğinde, seçtiğiniz ve girdiğiniz seçeneklerin bir özetini görürsünüz. Bilgileri ve **kullanım koşullarını**doğrulayın ve **Oluştur'u**seçin.

Chef Extension ile sanal makineoluşturma ve dağıtma işlemi tamamlandığında, bir bildirim işlemin başarısını veya başarısızlığını gösterir. Ayrıca, yeni sanal makinenin kaynak sayfası oluşturulduktan sonra Azure portalında otomatik olarak açılır.

![Chef Server'ı Linux sanal makinesine yükleme](./media/chef-extension-portal/resource-created.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Chef'i kullanarak Azure'da Windows sanal makinesi oluşturma](chef-automation.md)