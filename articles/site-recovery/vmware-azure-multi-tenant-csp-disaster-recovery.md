---
title: Site Kurtarma ve Bulut Çözüm Sağlayıcısı (CSP) programını kullanarak Çok kiralı bir ortamda Azure'a VMware olağanüstü durum kurtarma yı ayarlama | Microsoft Dokümanlar
description: Azure Site Kurtarma ile çok kiracılı bir ortamda VMware olağanüstü durum kurtarmanın nasıl ayarlanır olduğunu açıklar.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: mayg
ms.openlocfilehash: 77b64f09b7fd1429eb23c4407c729dfc0aafdf2b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60461026"
---
# <a name="set-up-vmware-disaster-recovery-in-a-multi-tenancy-environment-with-the-cloud-solution-provider-csp-program"></a>Bulut Çözüm Sağlayıcısı (CSP) programıyla çok kiralı bir ortamda VMware olağanüstü durum kurtarmayı ayarlama

[CSP programı,](https://partner.microsoft.com/en-US/cloud-solution-provider) Office 365, Enterprise Mobility Suite ve Microsoft Azure gibi Microsoft bulut hizmetleri için daha iyi bir araya getirilmiştir. CSP ile, iş ortakları müşterilerle uç-uç ilişki sahibi ve birincil ilişki iletişim noktası haline gelir. İş ortakları müşteriler için Azure aboneliklerini dağıtabilir ve abonelikleri kendi katma değerli, özelleştirilmiş teklifleri ile birleştirebilir.

[Azure Site Kurtarma](site-recovery-overview.md)ile iş ortakları olarak müşteriler için olağanüstü durum kurtarmayı doğrudan CSP üzerinden yönetebilirsiniz. Alternatif olarak, Site Kurtarma ortamlarını ayarlamak ve müşterilerin kendi olağanüstü durum kurtarma gereksinimlerini self servis bir şekilde yönetmelerine izin vermek için CSP'yi kullanabilirsiniz. Her iki senaryoda da, ortaklar Site Kurtarma ve müşterileri arasındaki irtibat vardır. İş ortakları müşteri ilişkilerine hizmet verir ve Site Kurtarma kullanımı için müşterilere fatura kes.

Bu makalede, bir iş ortağı olarak çok kiracılı VMware çoğaltma senaryosu için CSP aracılığıyla kiracı abonelikleri nasıl oluşturabileceğinizve yönetebileceğiniz açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

VMware çoğaltmayı ayarlamak için aşağıdakileri yapmanız gerekir:

- [Hazırla](tutorial-prepare-azure.md) Azure aboneliği, Azure sanal ağı ve depolama hesabı gibi Azure kaynakları.
- Şirket içi VMware sunucuları ve VM'ler [hazırlayın.](vmware-azure-tutorial-prepare-on-premises.md)
- Her kiracı için, kiracı VM'ler ve vCenter sunucularınızla iletişim kurabilen ayrı bir yönetim sunucusu oluşturun. Yalnızca iş ortağı olarak bu yönetim sunucusuna erişim haklarına sahip olmalısınız. [Çok kiracılı ortamlar](vmware-azure-multi-tenant-overview.md)hakkında daha fazla bilgi edinin.

## <a name="create-a-tenant-account"></a>Kiracı hesabı oluşturma

1. [Microsoft İş Ortağı Merkezi](https://partnercenter.microsoft.com/)aracılığıyla CSP hesabınızda oturum açın.
2. **Pano** menüsünde **Müşteriler'i**seçin.
3. Açılan sayfada **Müşteri Ekle** düğmesini tıklatın.
4. **Yeni Müşteri** sayfasında, kiracının hesap bilgilerini doldurun.

    ![Hesap Bilgileri sayfası](./media/vmware-azure-multi-tenant-csp-disaster-recovery/customer-add-filled.png)

5. Sonra **İleri'yi tıklatın: Abonelikler.**
6. Abonelikler seçim sayfasında **Microsoft Azure** onay kutusunu seçin. Diğer abonelikleri şimdi veya başka bir zamanda ekleyebilirsiniz.
7. Gözden **Geçir** sayfasında, kiracı ayrıntılarını onaylayın ve sonra **Gönder'i**tıklatın.
8. Kiracı hesabını oluşturduktan sonra, varsayılan hesabın ayrıntılarını ve bu aboneliğin parolasını görüntüleyen bir onay sayfası görüntülenir. Azure portalı oturum açma sayfasından bilgileri kaydedin ve parolayı gerektiğinde değiştirin.

Bu bilgileri olduğu gibi kiracıyla paylaşabilir veya gerekirse ayrı bir hesap oluşturup paylaşabilirsiniz.

## <a name="access-the-tenant-account"></a>Kiracı hesabına erişin

Microsoft İş Ortağı Merkezi Panosu aracılığıyla kiracının aboneliğine erişebilirsiniz.

1. **Müşteriler** sayfasında, kiracı hesabının adını tıklatın.
2. Kiracı hesabının **Abonelikler** sayfasında, varolan hesap aboneliklerini izleyebilir ve gerektiğinde daha fazla abonelik ekleyebilirsiniz.
3. Kiracının olağanüstü durum kurtarma işlemlerini yönetmek için **Tüm kaynaklar (Azure portalı)** seçeneğini belirleyin. Bu, kiracının Azure aboneliklerine erişmenizi verir.

    ![Tüm Kaynaklar bağlantısı](./media/vmware-azure-multi-tenant-csp-disaster-recovery/all-resources-select.png)  

4. Azure portalının sağ üst ağındaki Azure Active Directory bağlantısını tıklayarak erişimi doğrulayabilirsiniz.

    ![Azure Active Directory bağlantısı](./media/vmware-azure-multi-tenant-csp-disaster-recovery/aad-admin-display.png)

Artık Azure portalında kiracı için tüm Site Kurtarma işlemlerini gerçekleştirebilir ve yönetebilirsiniz. Yönetilen olağanüstü durum kurtarma için CSP aracılığıyla kiracı aboneliğine erişmek için, daha önce açıklanan işlemi izleyin.

## <a name="assign-tenant-access-to-the-subscription"></a>Aboneye kiracı erişimi atama

1. Olağanüstü durum kurtarma altyapısının ayarlandığından emin olun. İş ortakları, olağanüstü durum kurtarma nın yönetilip yönetilmediğine veya self servis olup olmadığına bakılmaksızın, Kiracı aboneliklerine CSP portalı üzerinden erişir. Kasayı ayarlayın ve altyapıyı kiracı aboneliklerine kaydedin.
2. Kiracıya [oluşturduğunuz hesabı sağlayın.](#create-a-tenant-account)
3. Aşağıdaki gibi CSP portalı üzerinden kiracı aboneliğine yeni bir kullanıcı ekleyebilirsiniz:

    a) Kiracının CSP abonelik sayfasına gidin ve ardından **Kullanıcılar ve lisanslar** seçeneğini seçin.

      ![Kiracının CSP abonelik sayfası](./media/vmware-azure-multi-tenant-csp-disaster-recovery/users-and-licences.png)

    b) Artık ilgili ayrıntıları girerek ve izinleri seçerek veya csv dosyasındaki kullanıcı listesini yükleyerek yeni bir kullanıcı oluşturun.
    
    c) Yeni bir kullanıcı oluşturduktan sonra Azure portalına geri dön. **Abonelik** sayfasında, ilgili aboneliği seçin.

    d) **Access denetimi (IAM) seçeneğini**belirleyin ve ardından **Rol atamaları'nı**tıklatın.

    e) İlgili erişim düzeyine sahip bir kullanıcı eklemek için **rol atamaekle'yi** tıklatın. CSP portalı aracılığıyla oluşturulan kullanıcılar Rol atamaları sekmesinde görüntülenir.

      ![Kullanıcı ekleme](./media/vmware-azure-multi-tenant-csp-disaster-recovery/add-user-subscription.png)

- Çoğu yönetim işlemi için *Katılımcı* rolü yeterlidir. Bu erişim düzeyine sahip kullanıcılar, erişim düzeylerini değiştirme *(Sahibi*düzeyinde erişimin gerekli olduğu) dışında her şeyi abonelikte yapabilir.
- Site Kurtarma da [üç önceden tanımlanmış kullanıcı rolleri](site-recovery-role-based-linked-access-control.md)vardır , daha fazla gerektiği gibi erişim düzeylerini kısıtlamak için kullanılabilir.

## <a name="multi-tenant-environments"></a>Çok kiracılı ortamlar

Üç büyük çok kiracılı model vardır:

* **Paylaşılan Barındırma Hizmetleri Sağlayıcısı (HSP)**: İş ortağı fiziksel altyapının sahibidir ve aynı altyapıda birden fazla kiracı VM barındırmak için paylaşılan kaynakları (vCenter, veri merkezleri, fiziksel depolama vb.) kullanır. İş ortağı, yönetilen bir hizmet olarak olağanüstü durum kurtarma yönetimi sağlayabilir veya kiracı self servis çözümü olarak olağanüstü durum kurtarma sahibi olabilir.

* **Özel Barındırma Hizmetleri Sağlayıcısı**: İş ortağı fiziksel altyapıya sahiptir, ancak her kiracının VM'lerini ayrı bir altyapıda barındırmak için özel kaynakları (birden çok vCenters, fiziksel veri depoları vb.) kullanır. İş ortağı, yönetilen bir hizmet olarak olağanüstü durum kurtarma yönetimi sağlayabilir veya kiracı bunu self servis bir çözüm olarak sahiplenebilir.

* **Yönetilen Hizmet Sağlayıcısı (MSP)**: Müşteri, VM'leri barındıran fiziksel altyapıya sahip olur ve iş ortağı olağanüstü durum kurtarma etkinleştirme ve yönetimi sağlar.

Bu makalede açıklandığı gibi kiracı abonelikleri ayarlayarak, ilgili çok kiracılı modellerden herhangi birinde müşterileri etkinleştirmeye hızla başlayabilirsiniz. Farklı çok kiracılı modeller ve şirket içi erişim denetimlerini [etkinleştirme](vmware-azure-multi-tenant-overview.md)hakkında daha fazla bilgi edinebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Azure Site Kurtarma dağıtımlarını yönetmek için [rol tabanlı erişim denetimi](site-recovery-role-based-linked-access-control.md) hakkında daha fazla bilgi edinin.
- Azure [çoğaltma mimarisine](vmware-azure-architecture.md)VMware hakkında daha fazla bilgi edinin.
- VMware VM'leri Azure'a çoğaltma [küçlonu lama yı gözden geçirin.](vmware-azure-tutorial.md)
VMware VM'leri Azure'a çoğaltmak için [çok kiracılı ortamlar](vmware-azure-multi-tenant-overview.md) hakkında daha fazla bilgi edinin.
