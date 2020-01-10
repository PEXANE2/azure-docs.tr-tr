---
title: Azure Farmtempts 'de kullanıcıları yönetme
description: Bu makalede, Azure Farmtempts 'de kullanıcıların nasıl yönetileceği açıklanır.
author: uhabiba04
ms.topic: article
ms.date: 12/02/2019
ms.author: v-umha
ms.openlocfilehash: 208a302a0702a5c4de4d194c9e72f562aaf758c4
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75482302"
---
# <a name="manage-users"></a>Kullanıcıları yönetme

Azure Farmtts, Azure Active Directory (Azure AD) örneğinin parçası olan kişilerin Kullanıcı yönetimini içerir. API 'Lere erişmek, oluşturulan haritaları görüntülemek ve grup algılayıcısı telemetrisine erişmek için Azure Farmtts örneğinize kullanıcı ekleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure Farmtts yüklemesi gerekiyor. Daha fazla bilgi için bkz. [Azure Farmtts 'Yi yüklemeyin](install-azure-farmbeats.md).
- Azure Farmtts örneğinden eklemek veya kaldırmak istediğiniz kullanıcıların e-posta kimlikleri.

## <a name="manage-azure-farmbeats-users"></a>Azure Farmtempts kullanıcılarını yönetme

Azure Farm, kimlik doğrulaması, erişim denetimi ve roller için Azure AD kullanır. Azure AD kiracısındaki kullanıcıları Azure Farmtempts ' deki kullanıcılar olarak ekleyebilirsiniz.

> [!NOTE]
> Azure AD kiracısında bir Azure Farmınts kullanıcısı olarak eklemeye çalıştığınız bir kullanıcı yoksa, "Azure AD kullanıcıları ekleme" bölümündeki yönergeleri izleyerek Kurulumu doldurun.

Azure Farmtts, iki tür Kullanıcı rolünü destekler:

 - **Yönetici**: Azure Farmtts veri hub 'ı API 'lerine tam erişim. Bu roldeki kullanıcılar tüm Azure Farmtts veri hub nesnelerini sorgulayabilir ve tüm işlemleri Farmtts hızlandırıcılarından gerçekleştirebilir.
 - **Salt okunurdur**: Farmtts veri hub 'ı API 'lerine salt okuma erişimi. Kullanıcılar veri hub 'ı API 'Leri, Hızlandırıcı panoları ve haritaları görüntüleyebilir. Salt okuma erişimi olan kullanıcılar haritalar oluşturma, cihazları ilişkilendirme veya gruplar oluşturma gibi işlemler gerçekleştiremez.

## <a name="add-users-to-azure-farmbeats"></a>Azure Farmtts 'ye Kullanıcı ekleme

Azure Farmtts 'ye Kullanıcı eklemek için:

1. Hızlandırıcıda oturum açın ve ardından **Ayarlar** simgesini seçin.
2. **Access Control**seçin.

    ![Gruplar ayarlar bölmesi](./media/create-farms-in-azure-farmbeats/settings-users-1.png)

3. Erişim vermek istediğiniz kullanıcının e-posta KIMLIĞINI girin.
4. İstenen rolü, **yönetici** veya **salt okunurdur**seçin.
5. **Rol Ekle**' yi seçin.

Eklenen Kullanıcı artık Azure Farmtts 'ye erişebilir (hem Datahub hem de Hızlandırıcı).

## <a name="delete-users-from-azure-farmbeats"></a>Azure Farmtts 'den kullanıcıları silme

Kullanıcıları Azure Farmtts sisteminden kaldırmak için:

1. Hızlandırıcıda oturum açın ve ardından **Ayarlar** simgesini seçin.
2. **Access Control**seçin.
3. **Sil**’i seçin.

   Kullanıcı sistemden silinir. Aşağıdaki onay iletisini alırsınız:

   ![Azure Farmtts onay iletisi](./media/create-farms-in-azure-farmbeats/manage-users-2.png)

## <a name="add-azure-ad-users"></a>Azure AD kullanıcıları ekleme

> [!NOTE]
> Azure Farm, kullanıcıların uygulama ve rollere atamadan önce Azure AD kiracısında mevcut olmaları gerekir. Azure Farmınts 'ye eklemek istediğiniz bir Kullanıcı Azure AD kiracısında zaten mevcut değilse, bu bölümdeki yönergeleri izleyin. Kullanıcı Azure AD kiracısında varsa, bu yönergeleri atlayabilirsiniz.

Azure AD 'ye Kullanıcı eklemek için aşağıdakileri yapın:

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.
2. Sağ üst köşedeki hesabınızı seçin ve ardından Farmtts ile ilişkili Azure AD kiracıya geçin.
3. **Azure Active Directory** > **kullanıcıları**' nı seçin.

    Azure AD kullanıcıları listesi görüntülenir.

4. Dizine bir kullanıcı eklemek için **Yeni Kullanıcı**' yı seçin. Dış Kullanıcı eklemek için **Yeni Konuk Kullanıcı**' yı seçin.

    !["Tüm kullanıcılar" bölmesi](./media/create-farms-in-azure-farmbeats/manage-users-3.png)

5. Yeni kullanıcının adını seçin ve ardından bu kullanıcı için gerekli alanları doldurun.
6. **Oluştur**’u seçin.

Azure AD kullanıcılarını yönetme hakkında daha fazla bilgi için bkz. [Azure AD 'de Kullanıcı ekleme veya silme](https://docs.microsoft.com/azure/active-directory/fundamentals/add-users-azure-active-directory/).

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcıları Azure Farmtts örneğinize başarıyla eklediniz. Şimdi [grupları oluşturma ve yönetme](manage-farms-in-azure-farmbeats.md#create-farms)hakkında bilgi edinin.
