---
title: Kuruluşunuzun gizlilik bilgilerini ekleyin - Azure Active Directory | Microsoft Dokümanlar
description: Kuruluşunuzun gizlilik bilgilerini Azure Etkin Dizin Özellikleri alanına nasıl ekleyeceğiniz le ilgili talimatlar.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: ajburnle
ms.reviewer: bpham
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 95f1877ae630919ba7da5a851b5f8291ade2d165
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76897590"
---
# <a name="add-your-organizations-privacy-info-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak kuruluşunuzun gizlilik bilgilerini ekleme
Bu makalede, kiracı yöneticinin Azure portalı aracılığıyla kuruluşun Azure Etkin Dizini (Azure AD) kiracısına gizlilikle ilgili bilgileri nasıl ekleyebileceği açıklanmaktadır.

Dahili çalışanlarınızın ve harici misafirlerinizin politikalarınızı gözden geçirebilmeleri için hem küresel gizlilik kişinizi hem de kuruluşunuzun gizlilik bildirimini eklemenizi şiddetle öneririz. Gizlilik bildirimleri benzersiz bir şekilde oluşturulduğu ve her işletme için uyarlandığı için, yardım için bir avukata başvurmanızı şiddetle öneririz.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="add-your-privacy-info-on-azure-ad"></a>Azure AD'de gizlilik bilgilerinizi ekleyin
Kuruluşunuzun gizlilik bilgilerini Azure AD'nin **Özellikler** alanına eklersiniz.

### <a name="to-access-the-properties-area-and-add-your-privacy-information"></a>Özellikler alanına erişmek ve gizlilik bilgilerinizi eklemek için

1.  Azure portalında kiracı yönetici olarak oturum açın.

2.  Soldaki gezinme çubuğunda **Azure Etkin Dizini'ni**seçin ve ardından **Özellikler'i**seçin.

    **Özellikler** alanı görüntülenir.

    ![Gizlilik bilgileri alanını vurgulayan Azure AD Özellikleri alanı](media/active-directory-properties-area/properties-area.png)

3.  Çalışanlarınız için gizlilik bilgilerinizi ekleyin:

    - **Teknik temas.** Kuruluşunuzdaki teknik destek için iletişim kuracak kişinin e-posta adresini yazın.
    
    - **Küresel gizlilik iletişimi.** Kişisel verilerin gizliliği yle ilgili sorularınız için iletişim kuracak kişinin e-posta adresini yazın. Bu kişi, bir veri ihlali olduğunda Microsoft'un bağlantı kiminle iletişim kuran kişisidir. Burada listelenen kişi yoksa, Microsoft genel yöneticilerinizle iletişim eler.

    - **Gizlilik bildirimi URL'si.** Kuruluşunuzun hem dahili hem de harici konuk gizliliğini nasıl işleyeceğini açıklayan kuruluşunuzun belgesine bağlantı yazın.

        >[!Important]
        >Kendi gizlilik bildiriminizi veya gizlilik kişinizi eklemezseniz, harici konuklarınız **İnceleme İzinleri** kutusunda, ** < _org adınız_> yorumlayasınız.** Örneğin, bir konuk kullanıcı b2B işbirliği yoluyla bir kuruluşa erişim daveti aldığında bu iletiyi görür.

        ![B2B İşbirliği İnceleme İzinleri kutusu ile ileti](media/active-directory-properties-area/active-directory-no-privacy-statement-or-contact.png)

4.  **Kaydet'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Active Directory B2B işbirliği daveti ödeme](https://aka.ms/b2bredemption)
- [Azure Etkin Dizini'ndeki bir kullanıcı için profil bilgileri ekleme veya değiştirme](active-directory-users-profile-azure-portal.md)
