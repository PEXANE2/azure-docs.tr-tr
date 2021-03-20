---
title: Azure Farmtempts 'de kullanıcıları yönetme
description: Bu makalede, Azure Farmtempts 'de kullanıcıların nasıl yönetileceği açıklanır.
author: uhabiba04
ms.topic: article
ms.date: 12/02/2019
ms.author: v-ummehabiba
ms.openlocfilehash: de9a11cac9856417422bd72579cbca44233ab422
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102179910"
---
# <a name="manage-users"></a>Kullanıcıları yönetme

Azure Farmtts, Azure Active Directory (Azure AD) örneğinin parçası olan kişilerin Kullanıcı yönetimini içerir. API 'Lere erişmek, oluşturulan haritaları görüntülemek ve grup algılayıcısı telemetrisine erişmek için Azure Farmtts örneğinize kullanıcı ekleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Azure Farmtts yüklemesi gerekiyor. Daha fazla bilgi için bkz. [Azure Farmtts 'Yi yüklemeyin](install-azure-farmbeats.md).
- Azure Farmtts örneğinden eklemek veya kaldırmak istediğiniz kullanıcıların e-posta kimlikleri.

## <a name="manage-azure-farmbeats-users"></a>Azure Farmtempts kullanıcılarını yönetme

Azure Farm, kimlik doğrulaması, erişim denetimi ve roller için Azure AD kullanır. Azure AD kiracısındaki kullanıcıları Azure Farmtempts ' deki kullanıcılar olarak ekleyebilirsiniz.

> [!NOTE]
> Bir Kullanıcı Azure AD kiracı kullanıcısı değilse, kurulumu gerçekleştirmek için **Azure AD kullanıcıları Ekle** bölümündeki yönergeleri izleyin.

Azure Farmtts, iki tür Kullanıcı rolünü destekler:

 - **Yönetici**: Azure Farmrets veri hub 'ı API 'lerine erişin. Bu roldeki kullanıcılar tüm Azure Farmtts veri hub nesnelerini sorgulayabilir ve tüm işlemleri Farmtts hızlandırıcılarından gerçekleştirebilir.
 - **Salt okunurdur**: Farmtts veri hub 'ı API 'lerine salt okuma erişimi. Kullanıcılar veri hub 'ı API 'Leri, Hızlandırıcı panoları ve haritaları görüntüleyebilir. Salt okuma erişimi olan kullanıcılar haritalar oluşturma, cihazları ilişkilendirme veya gruplar oluşturma gibi işlemler gerçekleştiremez.

## <a name="add-users-to-azure-farmbeats"></a>Azure Farmtts 'ye Kullanıcı ekleme

Azure Farmtts 'ye Kullanıcı eklemek için:

1. Hızlandırıcıda oturum açın ve ardından **Ayarlar** simgesini seçin.
2. **Access Control** seçin.

    ![Gruplar ayarlar bölmesi](./media/create-farms-in-azure-farmbeats/settings-users-1.png)

3. Erişim vermek istediğiniz kullanıcının e-posta KIMLIĞINI girin.
4. İstenen rolü, **yönetici** veya **salt okunurdur** seçin.
5. **Rol Ekle**' yi seçin.

Eklenen Kullanıcı artık Azure Farmtts 'ye erişebilir (hem Datahub hem de Hızlandırıcı).

## <a name="delete-users-from-azure-farmbeats"></a>Azure Farmtts 'den kullanıcıları silme

Kullanıcıları Azure Farmtts sisteminden kaldırmak için:

1. Hızlandırıcıda oturum açın ve ardından **Ayarlar** simgesini seçin.
2. **Access Control** seçin.
3. **Sil**’i seçin.

   Kullanıcı sistemden silinir. Aşağıdaki onay iletisini alırsınız:

   ![Azure Farmtts onay iletisi](./media/create-farms-in-azure-farmbeats/manage-users-2.png)

## <a name="add-azure-ad-users"></a>Azure AD kullanıcıları ekleme

> [!NOTE]
> Azure Farm, kullanıcıların uygulamalara ve rollere atamadan önce Azure AD kiracısında mevcut olmaları gerekir. Bir Kullanıcı Azure AD kiracısında yoksa, bu bölümdeki yönergeleri izleyin. Azure AD kiracısında bir kullanıcı zaten varsa yönergeleri atlayın.

Azure AD 'ye Kullanıcı eklemek için şu adımları izleyin:

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. Sağ üst köşedeki hesabınızı seçin ve ardından Farmtts ile ilişkili Azure AD kiracıya geçin.
3. **Azure Active Directory**  >  **kullanıcıları**' nı seçin.

    Azure AD kullanıcıları listesi görüntülenir.

4. Dizine bir kullanıcı eklemek için **Yeni Kullanıcı**' yı seçin. Dış Kullanıcı eklemek için **Yeni Konuk Kullanıcı**' yı seçin.

    !["Tüm kullanıcılar" bölmesi](./media/create-farms-in-azure-farmbeats/manage-users-3.png)

5. Yeni kullanıcının adını seçin ve ardından bu kullanıcı için gerekli alanları doldurun.
6. **Oluştur**’u seçin.

Azure AD kullanıcılarını yönetme hakkında daha fazla bilgi için bkz. [Azure AD 'de Kullanıcı ekleme veya silme](../../active-directory/fundamentals/add-users-azure-active-directory.md).

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcıları Azure Farmtts örneğinize başarıyla eklediniz. Şimdi [grupları oluşturma ve yönetme](manage-farms-in-azure-farmbeats.md#create-farms)hakkında bilgi edinin.