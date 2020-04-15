---
title: Azure Çok Faktörlü Kimlik Doğrulama - Azure Etkin Dizini için kullanıcı ayarlarını yönetme
description: Azure Çok Faktörlü Kimlik Doğrulama için Azure Active Directory kullanıcı ayarlarını nasıl yapılandırabileceğinizi öğrenin
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 04/13/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 048224a55c2bbcbc99281d070d88d34e2dc77168
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81309766"
---
# <a name="manage-user-settings-for-azure-multi-factor-authentication"></a>Azure Çok Faktörlü Kimlik Doğrulama için kullanıcı ayarlarını yönetme

Azure Çok Faktörlü Kimlik Doğrulama kullanıcılarını yönetmenize yardımcı olmak için, kullanıcıların parolalarını sıfırlamalarını, MFA'ya yeniden kaydolmalarını veya varolan MFA oturumlarını iptal etmesini sağlayabilirsiniz. Uygulama parolalarını tanımlayan kullanıcılar için, bu parolaları silmeyi de seçebilirsiniz ve bu da eski kimlik doğrulamasının bu uygulamalarda başarısız olmasını sağlayabilirsiniz. Bir kullanıcıya yardım sağlamanız gerekiyorsa veya güvenlik durumlarını sıfırlamak istiyorsanız, bu eylemler gerekli olabilir.

## <a name="manage-user-authentication-options"></a>Kullanıcı kimlik doğrulama seçeneklerini yönetme

*Kimlik Doğrulama Yöneticisi* rolü size atanırsa, kullanıcıların parolalarını sıfırlamalarını, MFA'ya yeniden kaydolmalarını veya kullanıcı nesnelerinden varolan MFA oturumlarını iptal etmelerine gerek kalabilirsiniz. Kullanıcı ayarlarını yönetmek için aşağıdaki adımları tamamlayın:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Solda, Azure **Etkin Dizin** > **Kullanıcıları** > **Tüm kullanıcıları**seçin.
1. Bir eylem gerçekleştirmek istediğiniz kullanıcıyı seçin ve **Kimlik Doğrulama yöntemlerini**seçin. Pencerenin üst kısmında, ardından kullanıcı için aşağıdaki seçeneklerden birini seçin:
   - **Parolayı Sıfırla** kullanıcının parolasını sıfırlar ve bir sonraki oturum açmada değiştirilmesi gereken geçici bir parola atar.
   - **MFA'yı yeniden kaydetmeyi** zorunlu kılması, kullanıcı bir dahaki sefere imzalandığında yeni bir MFA kimlik doğrulama yöntemi ayarlamaları istenir.
   - **MFA Oturumları'nın iptali,** kullanıcının hatırlanan MFA oturumlarını temizler ve cihazdaki ilke tarafından bir sonraki kez gerekli olduğunda MFA gerçekleştirmelerini gerektirir.

   ![Azure portalından kimlik doğrulama yöntemlerini yönetme](./media/howto-mfa-userdevicesettings/manage-authentication-methods-in-azure.png)

## <a name="delete-users-existing-app-passwords"></a>Kullanıcıların varolan uygulama parolalarını silme

Gerekirse, bir kullanıcının oluşturduğu tüm uygulama parolalarını silebilirsiniz. Bu uygulama parolalarıyla ilişkili tarayıcı dışı uygulamalar, yeni bir uygulama parolası oluşturulana kadar çalışmayı durdurur. *Bu* eylemi gerçekleştirmek için genel yönetici izinleri gereklidir.

Bir kullanıcının uygulama parolalarını silmek için aşağıdaki adımları tamamlayın:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol tarafta, **Azure Etkin Dizin** > **Kullanıcıları** > **Tüm kullanıcıları**seçin.
1. **Çok Faktörlü Kimlik Doğrulama'yı**seçin. Bu menü seçeneğini görmek için sağa kaydırmanız gerekebilir. Azure portal penceresinin tamamını ve menü konumunu görmek için aşağıdaki örnek ekran görüntüsünü seçin:[![](media/howto-mfa-userstates/selectmfa-cropped.png "Azure AD'deki Kullanıcılar penceresinden Çok Faktörlü Kimlik Doğrulaması'nı seçin")](media/howto-mfa-userstates/selectmfa.png#lightbox)
1. Yönetmek istediğiniz kullanıcı veya kullanıcıların yanındaki kutuyu işaretleyin. Hızlı adım seçeneklerilistesi sağda görünür.
1. **Kullanıcı ayarlarını yönet'i**seçin, ardından aşağıdaki örnekte gösterildiği **gibi, seçili kullanıcılar tarafından oluşturulan tüm varolan uygulama parolalarını sil**kutusunu işaretleyin: ![Varolan tüm uygulama parolalarını silin](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
1. **Kaydet'i**seçin, ardından **kapatın.**

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, tek tek kullanıcı ayarlarının yapılandırılmasına yardımcı oldu. Azure Çok Faktörlü Kimlik Doğrulama hizmeti ayarlarını yapılandırmak için [bkz.](howto-mfa-mfasettings.md)

Kullanıcılarınızın yardıma ihtiyacı varsa, [Azure Çok Faktörlü Kimlik Doğrulama için Kullanım kılavuzuna](../user-help/multi-factor-authentication-end-user.md)bakın.
