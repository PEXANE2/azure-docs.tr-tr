---
title: Şirket iş hesapları ve Ortak Merkezi
description: Şirketinizin Microsoft ile ayarlanmış bir iş hesabı olup olmadığını, yeni bir iş hesabı oluşturup oluşturmadığını veya İş Ortağı Merkezi ile kullanmak üzere birden çok iş hesabı oluşturup oluşturmadığını denetleme.
author: dsindona
ms.author: parthp
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: c4e7427d87c5f88d8c686b867ef88ceb05f28286
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80281434"
---
# <a name="company-work-accounts-and-partner-center"></a>Şirket iş hesapları ve Ortak Merkezi

İş Ortağı Merkezi, birden çok kullanıcının hesap erişimini yönetmek, izinleri denetlemek, grupları ve uygulamaları barındırmak ve profil verilerini korumak için Azure Etkin Dizin (AD) kiracıları olarak da bilinen şirket çalışma hesaplarını kullanır. Şirketinizin iş e-posta hesabı etki alanını İş Merkezi hesabınıza bağlayarak, şirketinizin çalışanları kendi iş hesabı kullanıcı adlarını ve parolalarını kullanarak pazar teklifleri yönetmek için Ortak Merkezi'nde oturum açabilir.

## <a name="check-whether-your-company-already-has-a-work-account"></a>Şirketinizin zaten bir iş hesabı olup olmadığını kontrol edin

Şirketiniz Azure, Microsoft Intune veya Office 365 gibi bir Microsoft bulut hizmetine abone olduysa, İş Merkezi ile kullanılabilecek bir iş e-posta hesabı etki alanınız (Azure Etkin Dizin kiracı olarak da adlandırılır).

Denetlemek için aşağıdaki adımları izleyin:
1. Azure yönetici portalında oturum https://portal.azure.comaç.
2. Sol gezinme menüsünden **Azure Etkin Dizini'ni** seçin ve ardından Özel alan **adlarını**seçin.
3. Zaten bir iş hesabınız varsa, alan adınız listelenir.

Şirketinizin zaten bir iş hesabı yoksa, İş Ortağı Merkezi kayıt işlemi sırasında sizin için bir hesap oluşturulur.

## <a name="set-up-multiple-work-accounts"></a>Birden çok iş hesabı ayarlama

Varolan bir iş hesabını kullanmaya karar vermeden önce, iş hesabında kaç kullanıcının İş Ortağı Merkezi'ne erişmeye ihtiyacı olacağını göz önünde bulundurun. İş hesabında İş Ortağı Merkezi'ne erişmeihtiyacı olmayan kullanıcılarınız varsa, yalnızca İş Ortağı Merkezi'ne erişmeihtiyacı olan kullanıcıların belirli bir hesapta temsil edilmesi için birden çok iş hesabı oluşturmayı düşünebilirsiniz.

## <a name="create-a-new-work-account"></a>Yeni bir iş hesabı oluşturma

Şirketiniz için yeni bir iş hesabı oluşturmak için aşağıdaki adımları izleyin. Şirketinizin Microsoft Azure hesabında yönetim izinleri olan lardan yardım istemeniz gerekebilir.

1. Microsoft Azure [portalında](https://portal.azure.com)oturum açın.
2. Soldaki gezinti menüsünden **Azure Etkin Dizin** -> **Kullanıcıları'nı**seçin.
3. **Yeni kullanıcıyı** seçin ve bir ad ve e-posta adresi girerek yeni bir Azure iş hesabı oluşturun. **Dizin rolünün** **Kullanıcı'ya** ayarlandığından emin olun ve otomatik olarak oluşturulan parolayı görüntülemek ve not almak için alttaki **Parolayı Göster** onay kutusunu seçin.
4. Yeni kullanıcıyı kaydetmek için **Oluştur'u** seçin.

Kullanıcı hesabının e-posta adresi dizininizde doğrulanmış bir etki alanı adı olmalıdır. Sol gezinme menüsünde **Azure Active Directory** -> **Custom etki alanı adlarını** seçerek dizininizdeki tüm doğrulanmış etki alanlarını listeleyebilirsiniz.

Azure Etkin Dizin'de özel etki alanları ekleme hakkında daha fazla bilgi edinmek için Azure [AD'de bir etki alanı ekle veya ilişkilendirme](https://docs.microsoft.com/azure/active-directory/active-directory-add-domain)bölümüne bakın.

## <a name="troubleshoot-work-email-sign-in"></a>Sorun giderme iş e-posta oturum açma

İş hesabınızda oturum açmada sorun yaşıyorsanız (Azure AD kiracınız olarak da bilinir), aşağıdaki diyagramda durumunuzla en iyi eşleşen senaryoyu bulun ve önerilen adımları izleyin.

![Sorun giderme iş hesabı oturum açma diyagramı](./media/onboarding-aad-flow.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Pazar Hesabınızı İş Ortağı Merkezi'nde yönetin](./manage-account.md) 
