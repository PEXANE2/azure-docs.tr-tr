---
title: Dinamik grup üyeliği ekleme kullanıcıları Ekle-öğretici-Azure Active Directory
description: Bu öğreticide kullanıcıları otomatik olarak eklemek veya kaldırmak için grupları ve kullanıcı üyeliği kurallarını kullanacaksınız
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: tutorial
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1429841ca1376d67c7372f36bd35694afd4cd7ce
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71102639"
---
# <a name="tutorial-add-or-remove-group-members-automatically"></a>Öğretici: Grup üyelerini otomatik olarak ekle veya Kaldır

Azure Active Directory'de (Azure AD) sürekli el ile müdahale etmenize gerek kalmaması için güvenlik gruplarında veya Office 365 gruplarında kullanıcı ekleme veya kaldırma işlemlerini otomatik olarak gerçekleştirebilirsiniz. Bir kullanıcının veya cihazın özelliklerinden biri değiştiğinde Azure AD kiracınızdaki tüm dinamik grup kurallarını değerlendirerek gerekli üye ekleme veya kaldırma işlemlerini gerçekleştirir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Bir iş ortağı şirketten otomatik olarak doldurulmuş bir konuk kullanıcıları grubu oluşturma
> * Gruba konuk kullanıcıların erişmesi için iş ortağına özgü özelliklere yönelik lisans atama
> * Bonus: Üye kullanıcılarınızın yalnızca içeriden erişilebilen sitelere erişimini yönetme gibi işlemler gerçekleştirmek için konuk kullanıcıları kaldırarak **Tüm kullanıcılar** grubunun güvenliğini sağlama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Bu özellik, kiracının genel yöneticisi olarak sizin için bir Azure AD Premium lisansı gerektirir. Lisansınız yoksa Azure AD'de **Lisanslar** > **Ürünler** > **Dene/Satın Al**'ı seçin.

Dinamik gruplara üye olarak ekleyeceğiniz kullanıcılara lisans atamanıza gerek yoktur. Yalnızca kiracıda bu kullanıcıları kapsamaya yeterli sayıda Azure AD Premium P1 lisansına sahip olmanız gerekir. 

## <a name="create-a-group-of-guest-users"></a>Konuk kullanıcı grubu oluşturma

Öncelikle tamamı tek bir iş ortağı şirketinize ait olan konuk kullanıcılar için bir grup oluşturmanız gerekir. Bu kullanıcılar özel lisansa ihtiyaç duyduğundan bu amaca yönelik bir grup oluşturmak daha etkili olacaktır.

1. Azure Portal (https://portal.azure.com) kiracınız için genel yönetici olan bir hesapla) oturum açın.
2. **Azure Active Directory** > **Gruplar** > **Yeni grup**'u seçin.
   ![Yeni bir grup başlatmak için komutu seçin](./media/groups-dynamic-tutorial/new-group.png)
3. **Grup** dikey penceresinde:
  
   * Grup türü olarak **güvenlik** ' i seçin.
   * Grubun `Guest users Contoso` adını ve açıklamasını girin.
   * **Üyelik türünü** **Dinamik Kullanıcı**olarak değiştirin.
   
4. **Sahipler** ' i seçin ve istediğiniz sahipleri bulmak Için **sahip Ekle** dikey penceresinde arama yapın. Seçime eklemek için istediğiniz sahibe tıklayın.
5. **Sahipler Ekle** dikey penceresini kapatmak için **Seç** ' e tıklayın.  
6. **Dinamik Kullanıcı üyeleri** kutusunda **dinamik sorguyu Düzenle** ' yi seçin.
7. **Dinamik üyelik kuralları** dikey penceresinde:

   * **Özellik** alanında, var olan değere tıklayın ve **UserType**' ı seçin. 
   * **İşleç** alanında **eşittir** seçili olduğunu doğrulayın.  
   * **Değer** alanını seçin ve **Konuk**yazın. 
   * Başka bir satır eklemek için **Ifade Ekle** Köprüsü ' ne tıklayın.
   * **Ve/veya** alanında, **ve**' ı seçin.
   * **Özellik** alanında **CompanyName**' i seçin.
   * **İşleç** alanında **eşittir** seçili olduğunu doğrulayın.
   * **Değer** alanına **contoso**girin.
   * **Dinamik üyelik kuralları** dikey penceresini kapatmak için **Kaydet** ' e tıklayın.
   
8. **Grup** dikey penceresinde **Oluştur**'u seçerek grubu oluşturun.

## <a name="assign-licenses"></a>Lisans ata

Yeni grubunuzu oluşturduğunuza göre bu iş ortağı kullanıcılarına ihtiyacı olan lisansları atayabilirsiniz.

1. Azure AD'de **Lisanslar**'ı seçin, bir veya daha fazla lisans seçin ve **Ata**'yı belirleyin.
2. **Kullanıcılar ve gruplar**'ı ve **Guest users Contoso** grubunu seçip değişikliklerinizi kaydedin.
3. **Atama seçenekleri**, seçtiğiniz lisanslarda bulunan hizmet planlarını açıp kapatmanızı sağlar. Değişiklik yaptığınızda **Tamam**'a tıklayarak kaydetmeyi unutmayın.
4. Atamayı tamamlamak için **Lisans ata** bölmesinin en altında bulunan **Ata**'ya tıklayın.

## <a name="remove-guests-from-all-users-group"></a>Tüm kullanıcılar grubundan konukları kaldırma

Yönetim planınız, tüm konuk kullanıcılarınızı şirketlerine göre gruplara ayırmak olabilir. Artık **Tüm kullanıcılar** grubunu değiştirerek yalnızca kiracınızdaki üye kullanıcılara özel hale getirebilirsiniz. Ardından bu grubu kullanarak kuruluşunuza özgü uygulama ve lisans atamaları gerçekleştirebilirsiniz.

   ![Tüm kullanıcılar grubunu yalnızca üyeleri kapsayacak şekilde değiştirme](./media/groups-dynamic-tutorial/all-users-edit.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

**Konuk kullanıcılar grubunu kaldırmak için**

1. Kiracınızın Genel Yöneticisi olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açın.
2. **Azure Active Directory** > **Gruplar**'ı seçin. **Guest users Contoso** grubunu, üç noktayı (...) ve ardından **Sil**'i seçin. Grubu sildiğinizde atanmış olan lisanslar kaldırılır.

**Tüm Kullanıcılar grubunu geri yüklemek için**
1. **Azure Active Directory** > **Gruplar**'ı seçin. Grubu açmak için **Tüm kullanıcılar** grubunun adını seçin.
1. **Dinamik üyelik kuralları**'nı seçin, kural metninin tamamını silin ve **Kaydet**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Konuk kullanıcı grubu oluşturma
> * Yeni grubunuza lisans atama
> * Tüm kullanıcılar grubunu yalnızca üyeleri kapsayacak şekilde değiştirme

Grup tabanlı lisanslama ile ilgili temel bilgiler için bir sonraki makaleye geçin
> [!div class="nextstepaction"]
> [Grup tabanlı lisanslama ile ilgili temel bilgiler](../fundamentals/active-directory-licensing-whatis-azure-portal.md)



