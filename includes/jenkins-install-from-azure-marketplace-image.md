---
description: include dosyası
author: tomarcher
manager: rloutlaw
ms.service: multiple
ms.workload: web
ms.devlang: na
ms.topic: include
ms.date: 03/12/2018
ms.author: tarcher
ms.custom: Jenkins
ms.openlocfilehash: 5439de30b02b0ce05853c8112f9e29239743ef98
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "67188991"
---
1. Tarayıcınızda [Jenkins için Azure Marketi görüntüsünü](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview)açın.

1. **ŞİmDİ AL'ı**seçin.

    ![Jenkins Marketplace görüntüsü için yükleme işlemini başlatmak için ŞİmDİ GIT'i seçin.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-get-it-now.png)

1. Fiyatlandırma ayrıntıları ve terimler bilgilerini inceledikten sonra **Devam et'i**seçin.

    ![Jenkins Marketplace görüntü fiyatlandırma ve terimler bilgi.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-pricing-and-terms.png)

1. Azure portalındaki Jenkins sunucusunu yapılandırmak için **Oluştur'u** seçin. 

    ![Jenkins Marketplace görüntüsünü yükleyin.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-create.png)

1. Temel **bilgiler** sekmesinde aşağıdaki değerleri belirtin:

   - **Ad** - `Jenkins`Girin .
   - **Kullanıcı adı** - Jenkins'in üzerinde çalıştığını sanal makinede oturum alırken kullanmak üzere kullanıcı adını girin. Kullanıcı adı [belirli gereksinimleri](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm) karşılamalıdır.
   - **Kimlik doğrulama türü** - **SSH ortak anahtarını**seçin.
   - **SSH ortak anahtarı** - RsA ortak anahtarını tek satır biçiminde `ssh-rsa`(başlangıç) veya çok satırlı PEM biçiminde kopyalayın ve yapıştırın. Linux ve macOS'ta ssh-keygen veya Windows'da PuTTYGen kullanarak SSH tuşları oluşturabilirsiniz. SSH tuşları ve Azure hakkında daha fazla bilgi için, [Azure'da Windows ile SSH tuşları nasıl kullanılır](/azure/virtual-machines/linux/ssh-from-windows)makalesine bakın.
   - **Abonelik** - Jenkins'i yüklemek istediğiniz Azure aboneliğini seçin.
   - **Kaynak grubu** - **Yeni Oluştur'u**seçin ve Jenkins yüklemenizi oluşturan kaynakların toplanması için mantıksal bir kapsayıcı olarak hizmet veren kaynak grubu için bir ad girin.
   - **Konum** - **Doğu ABD'yi**seçin.

     ![Temel sekmesine Jenkins için kimlik doğrulama ve kaynak grubu bilgilerini girin.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-basic.png)

1. **Ek Ayarlar** sekmesine geçmek için **Tamam'ı** seçin. 

1. Ek **Ayarlar** sekmesinde aşağıdaki değerleri belirtin:

   - **Boyut** - Jenkins sanal makineniz için uygun boyutlandırma seçeneğini seçin.
   - **VM disk türü** - Jenkins sanal makinesi için hangi depolama disk türüne izin verildiğini belirtmek için HDD (sabit disk sürücüsü) veya SSD (katı hal sürücüsü) belirtin.
   - **Sanal ağ** - (İsteğe bağlı) Varsayılan ayarları değiştirmek için **Sanal ağı** seçin.
   - **Alt ağlar** - **Alt ağları**seçin, bilgileri doğrulayın ve **Tamam'ı**seçin.
   - **Genel IP adresi** - IP adresi, önceki sayfada belirttiğiniz Jenkins adı için varsayılan olarak -IP eki ile birlikte verilir. Bu varsayılanı değiştirme seçeneğini seçebilirsiniz.
   - **Alan adı etiketi** - Jenkins sanal makinesinin tam nitelikli URL değerini belirtin.
   - **Jenkins sürüm türü** - Seçeneklerden istenilen `LTS`sürüm `Weekly build`türünü `Azure Verified`seçin: , , veya . Ve `LTS` `Weekly build` seçenekleri makalede açıklanmıştır, [Jenkins LTS Yayın Hattı](https://jenkins.io/download/lts/). Bu `Azure Verified` seçenek, Azure'da çalıştırılan bir [Jenkins LTS sürümüne](https://jenkins.io/download/lts/) başvurur. 
   - **JDK Tipi** - JDK kurulacak. Varsayılan Zulu test edilir, OpenJDK sertifikalı yapılar.

     ![Ayarlar sekmesine Jenkins için sanal makine ayarlarını girin.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-settings.png)

1. **Tümleştirme Ayarları** sekmesine geçmek için **Tamam'ı** seçin.

1. **Tümleştirme Ayarları** sekmesinde aşağıdaki değerleri belirtin:

    - **Hizmet Sorumlusu** - Hizmet sorumlusu, Azure ile kimlik doğrulama için bir kimlik bilgisi olarak Jenkins'e eklenir. `Auto`müdürün MSI (Yönetilen Hizmet Kimliği) tarafından oluşturulacağı anlamına gelir. `Manual`müdürün sizin tarafından yaratılması gerektiği anlamına gelir. 
        - **Uygulama Kimliği** ve **Gizli** - `Manual` Servis **Müdürü** seçeneğini seçerseniz, servis müdürünüz `Secret` ve hizmet müdürünüzü belirtmeniz `Application ID` gerekir. [Bir hizmet ilkesi oluştururken,](/cli/azure/create-an-azure-service-principal-azure-cli)varsayılan rolün Azure kaynaklarıyla çalışmak için yeterli olan **Katılımcı**olduğunu unutmayın.
    - **Bulut Aracılarını Etkinleştir** - Azure Kapsayıcı `ACI` Örneği'ne başvuran `VM` ve sanal makinelere atıfta bulunan aracılar için varsayılan bulut şablonuna belirtinin. Ayrıca, bir `No` bulut aracısını etkinleştirmek istemiyorsanız da belirtebilirsiniz.

1. **Özet** sekmesine geçmek için **Tamam'ı** seçin.

1. **Özet** sekmesi görüntülendiğinde, girilen bilgiler doğrulanır. **Doğrulama geçti** iletisini (sekmenin üst kısmında) gördüğünüzde **Tamam'ı**seçin. 

     ![Özet sekmesi seçtiğiniz seçenekleri görüntüler ve doğrular.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-summary.png)

1. **Oluştur** sekmesi görüntülendiğinde, Jenkins sanal makinesini oluşturmak için **Oluştur'u** seçin. Sunucunuz hazır olduğunda, Azure portalında bir bildirim görüntülenir.

     ![Jenkins bildirime hazır.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-notification.png)
