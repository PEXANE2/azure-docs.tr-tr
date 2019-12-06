---
title: Kullanıcıları yönetme
description: Kullanıcıların nasıl yönetileceğini açıklar
author: uhabiba04
ms.topic: article
ms.date: 12/02/2019
ms.author: v-umha
ms.openlocfilehash: 37dacf0adfc9e3dcbab963cef50e2ee5209df314
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74852360"
---
# <a name="manage-users"></a>Kullanıcıları yönetme

Azure Farmfor, Azure Active Directory (Azure AD) bir parçası olan kişilerin Kullanıcı yönetimini içerir. API 'Lere erişmek için Azure Farmtts örneğiniz için Kullanıcı ekleyebileceksiniz, oluşturulan haritalar ve algılayıcı telemetrisini gruptan görüntüleyebilirsiniz

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

Azure Farmtts 'ye Kullanıcı eklemek için 
1.  Hızlandırıcıda oturum açın ve ayarlar simgesine tıklayın
2.  Access Control tıklayın

    ![Proje grubu ları](./media/create-farms/settings-users-1.png)

3.  Erişim vermek istediğiniz kullanıcının e-posta KIMLIĞINI girin
4.  İstenen rolü-yönetici veya salt okunurdur seçin
5.  Rol Ekle ' ye tıklayın

Eklenen kullanıcılar artık Azure Farmtts 'ye (veri hub 'ı ve hızlandırıcısına) erişebilecektir.

## <a name="delete-user-from-azure-farmbeats"></a>Azure Farmtts 'den Kullanıcı silme

Bir kullanıcıyı Azure Farmtts sisteminden kaldırmak için şunları yapabilirsiniz
1.  Hızlandırıcıda oturum açın ve ayarlar simgesine tıklayın
2.  Access Control tıklayın
3.  Kaldırmak istediğiniz kullanıcının e-posta KIMLIĞI yakınında Sil simgesine tıklayın

Kullanıcı sistemden kaldırılır. Başarılı işlemi onaylamak için aşağıdaki iletiyi alacaksınız


![Proje grubu ları](./media/create-farms/manage-users-2.png)


## <a name="add-azure-ad-users"></a>Azure AD kullanıcıları ekleme

> [!NOTE]
> Azure AD kiracısında yalnızca Azure Farmma erişimi sağlamaya çalıştığınız kullanıcı yoksa, aşağıdaki adımları gerçekleştirmeniz gerekir. Kullanıcı zaten varsa, aşağıdaki adımların gerçekleştirilmesi gerekir

Uygulama ve rollere atamadan önce bu kullanıcıların Azure AD kiracısında mevcut olmaları gerekir. Azure AD 'ye Kullanıcı eklemek için aşağıdaki adımları kullanın:
1.  [Azure Portal](https://portal.azure.com/)’ında oturum açın.
2.  Sağ üst köşede hesabınızı seçin ve Farmtts ile ilişkili Azure AD kiracısına geçiş yapın
3.  **Azure Active Directory > kullanıcıları**' nı seçin. Dizininizde bulunan kullanıcıların listesini görürsünüz.
4.  Dizine kullanıcı eklemek için **Yeni Kullanıcı**' yı seçin. Dış kullanıcılar için **Yeni Konuk Kullanıcı**' yı seçin.

    ![Proje grubu ları](./media/create-farms/manage-users-3.png)

5.  Yeni Kullanıcı için gerekli alanları doldurun. **Oluştur**'u seçin.

Azure AD 'de kullanıcıları yönetme hakkında daha fazla bilgi için [Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/add-users-azure-active-directory/) belgelerini ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Farmtts 'yi dağıttıysanız. Şimdi [gruplar oluşturmayı](manage-farms.md#create-farms)öğrenin.

