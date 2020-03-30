---
title: Azure FarmBeats'teki kullanıcıları yönetme
description: Bu makalede, Azure FarmBeats'teki kullanıcıların nasıl yönetilenbir şekilde yönetilenleri açıklanmaktadır.
author: uhabiba04
ms.topic: article
ms.date: 12/02/2019
ms.author: v-umha
ms.openlocfilehash: 6ccc29422c6abf1120d13c05d10125efd0871ca6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502057"
---
# <a name="manage-users"></a>Kullanıcıları yönetme

Azure FarmBeats, Azure Etkin Dizin (Azure AD) örneğinizin bir parçası olan kişiler için kullanıcı yönetimini içerir. API'lere erişmek, oluşturulan haritaları görüntülemek ve çiftlikten sensör telemetrisine erişmek için Azure FarmBeats örneğinize kullanıcı ekleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure FarmBeats yüklemesi gereklidir. Daha fazla bilgi için Azure [FarmBeats yükle'ye](install-azure-farmbeats.md)bakın.
- Azure FarmBeats örneğinize eklemek veya kaldırmak istediğiniz kullanıcıların e-posta iyelikleri.

## <a name="manage-azure-farmbeats-users"></a>Azure FarmBeats kullanıcılarını yönetin

Azure FarmBeats, kimlik doğrulama, erişim denetimi ve roller için Azure AD'yi kullanır. Azure AD kiracısındaki kullanıcıları Azure FarmBeats'teki kullanıcılar olarak ekleyebilirsiniz.

> [!NOTE]
> Bir kullanıcı Azure AD kiracısı değilse, kurulumu tamamlamak için **Azure AD kullanıcıları ekle** bölümündeki yönergeleri izleyin.

Azure FarmBeats iki tür kullanıcı rollerini destekler:

 - **Yönetici**: Azure FarmBeats Datahub API'lerine tam erişim. Bu roldeki kullanıcılar tüm Azure FarmBeats Datahub nesnelerini sorgulayabilir ve FarmBeats Hızlandırıcısı'ndaki tüm işlemleri gerçekleştirebilir.
 - **Salt Oku**: FarmBeats Datahub API'lerine salt okunur erişim. Kullanıcılar Datahub API'lerini, Hızlandırıcı Panolarını ve haritaları görüntüleyebilir. Salt okunur erişimi olan kullanıcılar eşlem oluşturma, aygıtları ilişkilendirme veya çiftlik oluşturma gibi işlemleri gerçekleştiremez.

## <a name="add-users-to-azure-farmbeats"></a>Azure FarmBeats'e kullanıcı ekleme

Azure FarmBeats'e kullanıcı eklemek için:

1. Hızlandırıcı'da oturum açın ve ardından **Ayarlar** simgesini seçin.
2. **Erişim Denetimi'ni**seçin.

    ![Çiftlikler Ayarları bölmesi](./media/create-farms-in-azure-farmbeats/settings-users-1.png)

3. Erişim izni vermek istediğiniz kullanıcının e-posta kimliğini girin.
4. İstenilen rolü, **Yöneticiyi** veya **Salt Okunur'u**seçin.
5. **Rol Ekle'yi**seçin.

Eklenen kullanıcı artık Azure FarmBeats'e (hem Datahub hem de Hızlandırıcı) erişebilir.

## <a name="delete-users-from-azure-farmbeats"></a>Kullanıcıları Azure FarmBeats'ten silme

Kullanıcıları Azure FarmBeats sisteminden kaldırmak için:

1. Hızlandırıcı'da oturum açın ve ardından **Ayarlar** simgesini seçin.
2. **Erişim Denetimi'ni**seçin.
3. **Sil**’i seçin.

   Kullanıcı sistemden silinir. Aşağıdaki onay iletisini alırsınız:

   ![Azure FarmBeats onay mesajı](./media/create-farms-in-azure-farmbeats/manage-users-2.png)

## <a name="add-azure-ad-users"></a>Azure AD kullanıcıları ekleme

> [!NOTE]
> Azure FarmBeats kullanıcılarının, bunları uygulamalara ve rollere atamadan önce Azure AD kiracısında bulunması gerekir. Azure AD kiracısında bir kullanıcı yoksa, bu bölümdeki yönergeleri izleyin. Bir kullanıcı Azure AD kiracısında zaten varsa yönergeleri atlayın.

Kullanıcıları Azure AD'ye eklemek için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Sağ üstte hesabınızı seçin ve ardından FarmBeats ile ilişkili Azure AD kiracısına geçin.
3. **Azure Etkin Dizin** > **Kullanıcıları'nı**seçin.

    Azure AD kullanıcılarının listesi görüntülenir.

4. Dizine kullanıcı eklemek için **Yeni kullanıcıyı**seçin. Harici bir kullanıcı eklemek için **Yeni konuk kullanıcıyı**seçin.

    !["Tüm kullanıcılar" bölmesi](./media/create-farms-in-azure-farmbeats/manage-users-3.png)

5. Yeni kullanıcının adını seçin ve ardından bu kullanıcı için gerekli alanları tamamlayın.
6. **Oluştur'u**seçin.

Azure AD kullanıcılarını yönetme hakkında bilgi için Azure [AD'deki kullanıcıları ekle veya silme 'ye](https://docs.microsoft.com/azure/active-directory/fundamentals/add-users-azure-active-directory/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure FarmBeats örneğinize başarıyla kullanıcı eklediniz. Şimdi, çiftlikleri [oluşturma yı ve yönetmeyi](manage-farms-in-azure-farmbeats.md#create-farms)öğrenin.
