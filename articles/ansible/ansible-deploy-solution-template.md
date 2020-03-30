---
title: Quickstart - Azure için Ansible çözüm şablonu yla CentOS'a dağıtın
description: Bu hızlı başlangıçta, Azure'da barındırılan CentOS sanal makinede Ansible çözüm şablonu ve Azure ile çalışmak üzere yapılandırılan araçlarnasıl dağıtılanın öğrenin.
keywords: ansible, azure, devops, çözüm şablonu, sanal makine, azure kaynakları için yönetilen kimlikler, centos, kırmızı şapka
ms.topic: quickstart
ms.date: 04/30/2019
ms.openlocfilehash: cc3a6c1f0ef36b51b62e6aa58f317aee13149589
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77193795"
---
# <a name="quickstart-deploy-the-ansible-solution-template-for-azure-to-centos"></a>Hızlı başlangıç: Azure için Ansible çözüm şablonu yla CentOS'a dağıtın

Azure için Ansible çözüm şablonu, Ansible ve Azure ile çalışmak üzere yapılandırılan bir araç paketiyle birlikte CentOS sanal makinede Ansible örneğini yapılandırmak üzere tasarlanmıştır. Araçlar şunları içerir:

- **Azure için ansible modülleri** - [Azure için Ansible modülleri,](./ansible-matrix.md) Azure'da altyapınızı oluşturmanıza ve yönetmenize olanak tanıyan bir modül paketidir. Bu modüllerin en son sürümü varsayılan olarak dağıtılır. Ancak, çözüm şablonu dağıtım işlemi sırasında, ortamınız için uygun bir sürüm numarası belirtebilirsiniz.
- **Azure Komut Satırı Arabirimi (CLI) 2.0** - [Azure CLI 2.0,](/cli/azure/?view=azure-cli-latest) Azure kaynaklarını yönetmek için bir çapraz platform komut satırı deneyimidir. 
- **Azure kaynakları için yönetilen kimlikler** - [Azure kaynakları için yönetilen kimlikler](/azure/active-directory/managed-identities-azure-resources/overview) özelliği, bulut uygulama kimlik bilgilerini güvende tutma sorununu gideriyor.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="deploy-the-ansible-solution-template"></a>Ansible çözüm şablonu dağıtma

1. [Azure Marketi'ndeki Ansible çözüm şablonuna](https://azuremarketplace.microsoft.com/en-%20%20us/marketplace/apps/azure-oss.ansible?tab=Overview)göz atın.

1. **ŞİmDİ AL'ı**seçin.

1. Kullanım Koşulları, Gizlilik İlkesi ve Azure MarketI Koşullarının Kullanımı'nı ayrıntılarıyla belirten bir pencere görüntülenir. **Devam**'ı seçin.

1. Azure portalı görünür ve çözüm şablonunu açıklayan Ansible sayfasını görüntüler. **Oluştur'u**seçin.

1. **Ansible Oluştur** sayfasında birkaç sekme görürsünüz. Temel **Bilgiler** sekmesinde, gerekli bilgileri girin:

   - **Ad** - Ansible örneğinizin adını belirtin. Demo amacıyla, ad `ansiblehost` kullanılır.
   - **Kullanıcı adı:** - Ansible örneğine erişebilecek kullanıcı adını belirtin. Demo amacıyla, ad `ansibleuser` kullanılır.
   - **Kimlik doğrulama türü:** - **Parola** veya **SSH ortak anahtarını**seçin. Demo **amacıyla, SSH ortak anahtarı** seçilir.
   - **Parola** ve **Onayla -** **Kimlik Doğrulama türü**için **Parola'yı** seçerseniz, bu değerler için parolanızı girin.
   - **SSH ortak anahtarı** - Kimlik Doğrulama **türü**için **SSH ortak anahtarını** seçerseniz, RSA `ssh-rsa`ortak anahtarınızı tek satır biçiminde girin - ile başlayarak .
   - **Abonelik** - Açılan listeden Azure aboneliğinizi seçin.
   - **Kaynak grubu** - Açılan listeden varolan bir kaynak grubu seçin veya **yeni oluştur'u** seçin ve yeni bir kaynak grubu için bir ad belirtin. Demo amacıyla, adlandırılmış `ansiblerg` yeni bir kaynak grubu kullanılır.
   - **Konum** - Senaryonuza uygun açılır listeden konumu seçin.

     ![Ansible temel ayarları için Azure portal sekmesi](./media/ansible-quick-deploy-solution-template/portal-ansible-setup-tab-1.png)

1. **Tamam'ı**seçin.

1. Ek **Ayarlar** sekmesine gerekli bilgileri girin:

   - **Boyut** - Azure portalı varsayılan olarak standart bir boyuta sahiptir. Belirli senaryonuzu barındıran farklı bir boyut belirtmek için, farklı boyutlardabir liste görüntülemek için oku seçin.
   - **VM disk türü** - **SSD** (Premium Solid-State Drive) veya **HDD** (Sabit Disk Sürücüsü) seçeneğini belirleyin. Demo amacıyla, **SSD** performans avantajları için seçilir. Bu disk depolama türleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
       - [VM’ler için yüksek performanslı Premium Depolama ve yönetilen diskler](/azure/virtual-machines/windows/premium-storage)
       - [Azure Sanal makine iş yükleri için Standart SSD Yönetilen Diskler](/azure/virtual-machines/windows/disks-standard-ssd)
   - **Genel IP Adresi** - Sanal makine nin dışından sanal makineyle iletişim kurmak istiyorsanız bu ayarı belirtin. Varsayılan ad ada sahip yeni bir `ansible-pip`genel IP adresidir. Farklı bir IP adresi belirtmek için, o IP adresinin adı, SKU ve Atama gibi öznitelikleri belirten oku seçin. 
   - **Alan adı etiketi** - Sanal makinenin genel kullanıma açık alan adını girin. Ad benzersiz olmalı ve adlandırma gereksinimlerini karşılamalıdır. Sanal makine için bir ad belirtme hakkında daha fazla bilgi için Azure [kaynakları için adlandırma kurallarına](/azure/architecture/best-practices/resource-naming)bakın.
   - **Ansible sürümü** - En son sürümü `latest` dağıtmak için bir sürüm numarası veya değeri belirtin. Kullanılabilir sürümler hakkında daha fazla bilgi görmek için **Ansible sürümünün** yanındaki bilgi simgesini seçin.

     ![Ansible ek ayarları için Azure portal sekmesi](./media/ansible-quick-deploy-solution-template/portal-ansible-setup-tab-2.png)

1. **Tamam'ı**seçin.

1. **Ansible Integration Settings** sekmesinde kimlik doğrulama türünü belirtin. Azure kaynaklarının güvenliğini sağlama hakkında daha fazla bilgi için azure [kaynakları için yönetilen kimlikler nedir?](/azure/active-directory/managed-identities-azure-resources/overview)

    ![Ansible tümleştirme ayarları için Azure portal sekmesi](./media/ansible-quick-deploy-solution-template/portal-ansible-setup-tab-3.png)

1. **Tamam'ı**seçin.

1. **Özet** sayfası doğrulama işlemini gösterir ve Ansible dağıtım için belirtilen ölçütleri listeler. Sekeğin altındaki bağlantı, desteklenen Azure dilleri ve platformlarıyla kullanılmak üzere **şablonu ve parametreleri indirmenize** olanak tanır. 

     ![Ansible Özet sekmesi için Azure portalı sekmesi](./media/ansible-quick-deploy-solution-template/portal-ansible-setup-tab-4.png)

1. **Tamam'ı**seçin.

1. **Oluştur** sekmesi göründüğünde, Ansible'ı dağıtmak için **Tamam'ı** seçin.

1. Ansible dağıtımını izlemek için portal sayfasının üst kısmındaki **Bildirimler** simgesini seçin. Dağıtım tamamlandıktan sonra **kaynak grubuna git'i**seçin. 

     ![Ansible Özet sekmesi için Azure portalı sekmesi](./media/ansible-quick-deploy-solution-template/portal-ansible-setup-complete.png)

1. Kaynak grubu sayfasında, Ansible ana bilgisayarınızın IP adresini alın ve Ansible'ı kullanarak Azure kaynaklarınızı yönetmek için oturum açın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Quickstart: Ansible kullanarak Azure'da bir Linux sanal makineyi yapılandırma](./ansible-create-vm.md)