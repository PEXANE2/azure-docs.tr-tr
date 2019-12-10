---
title: Kullanıcıları yönetme
description: Bu makalede, kullanıcıların nasıl yönetileceği açıklanır.
author: uhabiba04
ms.topic: article
ms.date: 12/02/2019
ms.author: v-umha
ms.openlocfilehash: f58db00e598fd08972adb60cbd8aae0d3bf3391d
ms.sourcegitcommit: b5ff5abd7a82eaf3a1df883c4247e11cdfe38c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74942347"
---
# <a name="manage-users"></a>Kullanıcıları yönetme

Azure Farmfor, Azure Active Directory (Azure AD) bir parçası olan kişilerin Kullanıcı yönetimini içerir. API 'Lere erişmek, oluşturulan haritaları görüntülemek ve gruptan algılayıcı Telemetriyi görmek için Azure Farmtts örneğinize Kullanıcı ekleyebileceksiniz.

## <a name="prerequisites"></a>Önkoşullar

- Azure Farmtts dağıtımı gereklidir. Bkz. Azure Farmtts 'yi ayarlama hakkında daha fazla bilgi için bkz. [Farmtempts 'ı dağıtma](prepare-for-deployment.md) .
- Azure Farmtts örneğinden eklemek veya kaldırmak istediğiniz kullanıcıların e-posta KIMLIĞI.

## <a name="manage-azure-farmbeats-users"></a>Azure Farmtempts kullanıcılarını yönetme

Azure Farm, kimlik doğrulaması, erişim denetimi ve roller için Azure AD kullanır. Azure AD kiracısındaki kullanıcılar, Azure Farmtları 'nda Kullanıcı olarak eklenebilir.

> [!NOTE]
> Eklemeye çalıştığınız kullanıcı Azure AD kiracısında yoksa, kurulumu gerçekleştirmek için Azure **ad kullanıcıları Ekle** bölümündeki yönergeleri izleyin ve bunları Azure Farmınts kullanıcıları olarak ayarlamaya devam edin.

**Roller**

Bugün Azure Farmtts 'de desteklenen iki tür Kullanıcı rolü vardır:

 - **Yönetici** — Azure 'un tüm erişimi veri merkezi API 'leri. Bu roldeki kullanıcılar tüm Azure farmm veri merkezi nesnelerini sorgulayabilir, tüm işlemleri Farmtts hızlandırıcılarından gerçekleştirebilir.
 - **Salt okunurdur** — Farmtts veri hub 'ı API 'lerine salt okuma erişimi. Kullanıcılar veri merkezi API 'Lerini, Hızlandırıcı panoları ve haritaları görüntüleyebilir. "Salt okunurdur" rolüne sahip bir Kullanıcı, haritalar oluşturma, cihazları ilişkilendirme veya grupları oluşturma gibi işlemler gerçekleştiremez.


## <a name="add-user-to-azure-farmbeats"></a>Azure Farmtts 'ye Kullanıcı ekleme

Azure Farmtts 'ye Kullanıcı eklemek için şu adımları izleyin:

1.  Hızlandırıcıda oturum açın ve ardından **Ayarlar** simgesini seçin.
2.  **Access Control**seçin.

    ![Proje grubu ları](./media/create-farms/settings-users-1.png)

3.  Erişim sağlamak istediğiniz kullanıcının e-posta KIMLIĞINI girin.
4.  İstenen rol – yönetici veya salt okunurdur ' ı seçin.
5.  **Rol Ekle**' yi seçin.

Eklenen kullanıcılar artık Azure Farmtts 'ye (veri hub 'ı ve hızlandırıcısına) erişebilecektir.

## <a name="delete-user-from-azure-farmbeats"></a>Azure Farmtts 'den Kullanıcı silme

Azure Farmmests sisteminden bir kullanıcıyı kaldırmak için aşağıdaki adımları izleyin:

1.  Hızlandırıcıda oturum açın ve ardından **Ayarlar** simgesini seçin.
2.  **Access Control**seçin.
3.  Bir kullanıcıyı silmek için **Sil** ' i seçin.

Kullanıcı sistemden silinir. Başarılı işlemi onaylamak için aşağıdaki iletiyi alacaksınız.

![Proje grubu ları](./media/create-farms/manage-users-2.png)

## <a name="add-azure-ad-users"></a>Azure AD kullanıcıları ekleme

> [!NOTE]
> Kullanıcı Azure AD kiracısında yoksa Azure Farmtts 'ye Kullanıcı erişimi sağlamak için aşağıdaki adımları izleyin. Kullanıcı Azure AD kiracısında varsa aşağıdaki adımları atlayabilirsiniz.
>

Uygulama ve rollere atamadan önce bu kullanıcıların Azure AD kiracısında mevcut olmaları gerekir. Azure AD 'ye Kullanıcı eklemek için aşağıdaki adımları kullanın:

1.  [Azure Portal](https://portal.azure.com/)’ında oturum açın.
2.  Sağ üst köşede hesabınızı seçin ve Farmtts ile ilişkili Azure AD kiracısına geçiş yapın.
3.  **Azure Active Directory > kullanıcıları**' nı seçin.
    Dizininizde bulunan kullanıcıların listesini görürsünüz.
4.  Dizine kullanıcı eklemek için **Yeni Kullanıcı**' yı seçin. Dış kullanıcılar için **Yeni Konuk Kullanıcı**' yı seçin.

    ![Proje grubu ları](./media/create-farms/manage-users-3.png)

5.  Yeni Kullanıcı için gerekli alanları doldurun. **Oluştur**'u seçin.

Azure AD 'de kullanıcıları yönetme hakkında daha fazla bilgi için [Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/add-users-azure-active-directory/) belgelerini ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcıları Azure Farmtts örneğine başarıyla eklediniz. Şimdi [gruplar oluşturmayı](manage-farms.md#create-farms)öğrenin.
