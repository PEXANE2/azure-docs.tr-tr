---
title: Azure AD'deki yerel yöneticilerin birleştiği cihazlarda nasıl yönetilir?
description: Azure rollerini bir Windows aygıtının yerel yöneticiler grubuna nasıl atayacağız öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: ravenn
ms.collection: M365-identity-device-management
ms.openlocfilehash: dc1812d955590ec0c7372e1311c9d69f93b9957c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128890"
---
# <a name="how-to-manage-the-local-administrators-group-on-azure-ad-joined-devices"></a>Azure AD'deki yerel yöneticiler grubunu yönetme

Bir Windows aygıtını yönetmek için yerel yöneticiler grubunun bir üyesi olmanız gerekir. Azure Etkin Dizin (Azure AD) birleştirme işleminin bir parçası olarak, Azure AD bu grubun üyeliğini bir aygıtta güncelleştirir. İş gereksinimlerinizi karşılamak için üyelik güncelleştirmesini özelleştirebilirsiniz. Örneğin, yardım masası personelinizin bir aygıtta yönetici hakları gerektiren görevleri yapmasını sağlamak istiyorsanız, üyelik güncelleştirmesi yararlıdır.

Bu makalede, üyelik güncelleştirmesinin nasıl çalıştığı ve Azure REKLAM Join sırasında nasıl özelleştirebileceğiniz açıklanmaktadır. Bu makalenin içeriği **karma** Azure AD join için geçerli değildir.

## <a name="how-it-works"></a>Nasıl çalışır?

Azure AD birleştirme sini kullanarak bir Windows aygıtını Azure AD'ye bağladığınızda, Azure AD aygıtı aygıttaki yerel yönetici grubuna aşağıdaki güvenlik ilkelerini ekler:

- Azure AD genel yönetici rolü
- Azure AD aygıt yöneticisi rolü 
- Azure AD birleştirme gerçekleştiren kullanıcı   

Yerel yöneticiler grubuna Azure AD rolleri ekleyerek, aygıtta hiçbir değişiklik yapmadan Azure AD'da istediğiniz zaman bir aygıtı yönetebilen kullanıcıları güncelleştirebilirsiniz. Şu anda, bir yönetici rolüne gruplar atayamazsınız.
Azure AD ayrıca, en az ayrıcalık ilkesini (PoLP) desteklemek için azure AD aygıt yöneticisi rolünü yerel yöneticiler grubuna da ekler. Genel yöneticilere ek olarak, *yalnızca* aygıt yöneticisi rolüne atanmış kullanıcıların bir aygıtı yönetmesini de sağlayabilirsiniz. 

## <a name="manage-the-global-administrators-role"></a>Genel yönetici rolünü yönetme

Genel yönetici rolünün üyeliğini görüntülemek ve güncelleştirmek için bkz:

- [Azure Etkin Dizin'de yönetici rolünün tüm üyelerini görüntüleme](../users-groups-roles/directory-manage-roles-portal.md)
- [Azure Active Directory’de kullanıcılara yönetici rolü atama](../fundamentals/active-directory-users-assign-role-azure-portal.md)


## <a name="manage-the-device-administrator-role"></a>Aygıt yöneticisi rolünü yönetme 

Azure portalında, **Aygıtlar** sayfasındaaygıt yöneticisi rolünü yönetebilirsiniz. **Cihazlar** sayfasını açmak için:

1. [Azure portalınızda](https://portal.azure.com) genel yönetici olarak oturum açın.
1. *Azure Active Directory*'yi bulun ve seçin.
1. **Yönet** **bölümünde, Cihazlar'ı**tıklatın.
1. **Cihazlar** sayfasında Aygıt **ayarlarını**tıklatın.

Aygıt yöneticisi rolünü değiştirmek için Azure **AD'deki Ek yerel yöneticilerin birleştiği aygıtları**yapılandırın.  

![Ek yerel yöneticiler](./media/assign-local-admin/10.png)

>[!NOTE]
> Bu seçenek, bir Azure AD Premium kiracı gerektirir. 

Aygıt yöneticileri, Azure AD'ye katılan tüm aygıtlara atanır. Aygıt yöneticilerini belirli bir aygıt kümesine kapsam süremezsiniz. Aygıt yöneticisi rolünün güncelleştirilmesinin etkilenen kullanıcılar üzerinde hemen bir etkisi olması gerekmez. Bir kullanıcının zaten oturum açmış olduğu aygıtlarda, ayrıcalık güncelleştirmesi aşağıdaki her *iki* eylem de gerçekleştiğinde gerçekleşir:

- Azure AD'nin uygun ayrıcalıklarla yeni bir Birincil Yenileme Belirteci yayınlaması için 4 saat geçti. 
- Kullanıcı, profilini yenilemek için kilitleme/kilidini açmamayı ve işaretlerini işaretler.

## <a name="manage-regular-users"></a>Normal kullanıcıları yönetme

Varsayılan olarak, Azure AD, Azure AD join'i gerçekleştiren kullanıcıyı aygıttaki yönetici grubuna ekler. Normal kullanıcıların yerel yönetici olmasını engellemek istiyorsanız, aşağıdaki seçeneklere sahipsiniz:

- [Windows Autopilot](/windows/deployment/windows-autopilot/windows-10-autopilot) - Windows Autopilot, birincil kullanıcının birleştirme gerçekleştirmesini engelleyen bir seçenek sunar. Bir [Otomatik Pilot profili oluşturarak](/intune/enrollment-autopilot#create-an-autopilot-deployment-profile)bunu başarabilirsiniz.
- [Toplu kayıt](/intune/windows-bulk-enroll) - Toplu kayıt bağlamında gerçekleştirilen bir Azure AD join, otomatik olarak oluşturulan bir kullanıcı bağlamında gerçekleşir. Aygıt birleştikten sonra oturum açmak için oturum açmak için yöneticiler grubuna eklenmez.   

## <a name="manually-elevate-a-user-on-a-device"></a>Bir aygıttaki kullanıcıyı el ile yükseltme 

Azure AD join işlemini kullanmanın yanı sıra, normal bir kullanıcıyı belirli bir aygıtta yerel yönetici olması için el ile yükseltebilirsiniz. Bu adım, zaten yerel yöneticiler grubunun bir üyesi olmanızı gerektirir. 

**Windows 10 1709** sürümünden başlayarak, bu görevi **Ayarlar -> Hesaplar> diğer kullanıcılardan**gerçekleştirebilirsiniz. **Bir iş veya okul kullanıcısı ekle'yi**seçin, Kullanıcı **hesabı** nın altında kullanıcının UPN'ini girin ve Hesap türü altında **Yönetici'yi** seçin *Administrator*  
 
Ayrıca, komut istemini kullanarak kullanıcıları da ekleyebilirsiniz:

- Kiracı kullanıcılarınız şirket içi Active Directory'den senkronize edilmişse, 'yi kullanın. `net localgroup administrators /add "Contoso\username"`
- Kiracı kullanıcılarınız Azure AD'de oluşturulursa,`net localgroup administrators /add "AzureAD\UserUpn"`

## <a name="considerations"></a>Dikkat edilmesi gerekenler 

Aygıt yöneticisi rolüne grup atamazsınız, yalnızca tek tek kullanıcılara izin verilir.

Aygıt yöneticileri, tüm Azure AD Joined aygıtlarına atanır. Belirli bir aygıt kümesine kapsam alamayacaklarını.

Kullanıcıları aygıt yöneticisi rolünden kaldırdığınızda, aygıtta oturum açılmışoldukları sürece bir cihazda yerel yönetici ayrıcalığına sahip olmaya devam ederler. Ayrıcalık, bir sonraki oturum açma sırasında veya yeni bir birincil yenileme belirteci verildiğinde 4 saat sonra iptal edilir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure portal'da cihaz yönetimine ilişkin genel bir bakış edinmek için bkz. [Azure portal'ı kullanarak cihazları yönetme](device-management-azure-portal.md)
- Aygıt tabanlı Koşullu Erişim hakkında daha fazla bilgi edinmek için [Azure Active Directory aygıt tabanlı Koşullu Erişim ilkelerini yapılandırma](../conditional-access/require-managed-devices.md)ya da yapılandırma ya da bkz.
