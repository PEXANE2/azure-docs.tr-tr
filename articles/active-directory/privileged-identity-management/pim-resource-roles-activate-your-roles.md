---
title: PIM'de Azure kaynak rollerini etkinleştirme - Azure AD | Microsoft Dokümanlar
description: Azure Özel Kimlik Yönetimi'nde (PIM) Azure kaynak rollerinizi nasıl etkinleştirdiğinizi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: d35c81f7bb478d91bd207327ea37c80aa1778142
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74023144"
---
# <a name="activate-my-azure-resource-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rollerimi etkinleştirme

Azure kaynakları için uygun rol üyelerinin gelecekteki bir tarih ve saat için etkinleştirme zamanlamasına olanak sağlamak için Ayrıcalıklı Kimlik Yönetimi'ni (PIM) kullanın. Ayrıca, maksimum (yöneticiler tarafından yapılandırılan) içinde belirli bir etkinleştirme süresi seçebilirsiniz.

Bu makale, Azure kaynak rollerini Ayrıcalıklı Kimlik Yönetimi'nde etkinleştirmeleri gereken üyeler içindir.

## <a name="activate-a-role"></a>Bir rolü etkinleştirme

Azure kaynak rolünü üstlenmeniz gerektiğinde, Ayrıcalıklı Kimlik Yönetimi'nde **Rollerim** gezinti seçeneğini kullanarak etkinleştirme isteğinde bulunabilirsiniz.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın. Panonuza Ayrıcalıklı Kimlik Yönetimi döşemesinin nasıl eklenöğretilen hakkında bilgi [için](pim-getting-started.md)bkz.

1. **Rollerimi**seçin.

    ![Etkinleştirebileceğiniz rolleri gösteren roller sayfam](./media/pim-resource-roles-activate-your-roles/resources-my-roles.png)

1. Uygun Azure kaynak rollerinizin listesini görmek için **Azure kaynak rollerini** seçin.

   ![Rollerim - Azure kaynak rolleri sayfası](./media/pim-resource-roles-activate-your-roles/resources-my-roles-azure-resources.png) 

1. Azure **kaynak rolleri** listesinde etkinleştirmek istediğiniz rolü bulun.

    ![Azure kaynak rolleri - Uygun roller listem](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate.png)

1. Etkinleştir bölmesini açmak için **Etkinleştir'i** seçin.

1. Rolünüz çok faktörlü kimlik doğrulama gerektiriyorsa, **devam etmeden önce kimliğinizi doğrulayın'ı**seçin. Oturum başına yalnızca bir kez kimlik doğrulamanız gerekir.

    ![Rol etkinleştirmeden önce kimliğimi MFA ile doğrulayın](./media/pim-resource-roles-activate-your-roles/resources-my-roles-mfa.png)

1. **Kimliğimi doğrula'yı** seçin ve ek güvenlik doğrulaması sağlamak için yönergeleri izleyin.

    ![PIN kodu gibi güvenlik doğrulaması sağlamak için ekran](./media/pim-resource-roles-activate-your-roles/resources-mfa-enter-code.png)

1. Azaltılmış bir kapsam belirtmek istiyorsanız, Kaynak filtresi bölmesini açmak için **Kapsam'ı** seçin.

    Yalnızca ihtiyacınız olan kaynaklara erişim istemek en iyi yöntemdir. Kaynak filtresi bölmesine, erişmeniz gereken kaynak gruplarını veya kaynakları belirtebilirsiniz.

    ![Etkinleştir - Kapsamı belirtmek için kaynak filtre bölmesi](./media/pim-resource-roles-activate-your-roles/resources-my-roles-resource-filter.png)

1. Gerekirse, özel bir etkinleştirme başlangıç saati belirtin. Üye, seçili süreden sonra etkinleştirilir.

1. **Reason** kutusuna etkinleştirme isteğinin nedenini girin.

    ![Tamamlanmış Kapsam, başlangıç saati, süre ve neden ile bölmeyi etkinleştirme](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-done.png)

1. **Etkinleştir'i**seçin.

    Rol onay gerektirmiyorsa, etkinleştirilir ve etkin roller listesine eklenir. Rolü kullanmak istiyorsanız, sonraki bölümdeki adımları izleyin.

    Rol etkinleştirmek için [onay gerektiriyorsa,](pim-resource-roles-approval-workflow.md) tarayıcınızın sağ üst köşesinde isteğin onay beklemede olduğunu bildiren bir bildirim görüntülenir.

    ![Etkinleştirme isteği onay bildirimi bekliyor](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-notification.png)

## <a name="use-a-role-immediately-after-activation"></a>Etkinleştirmeden hemen sonra bir rol kullanma

Etkinleştirmeden sonra herhangi bir gecikme durumunda, Azure kaynak rollerinizi hemen kullanmak için etkinleştirdikten sonra aşağıdaki adımları izleyin.

1. Azure AD Ayrıcalıklı Kimlik Yönetimi'ni açın.

1. Uygun Azure REKLAM rollerinizin ve Azure kaynak rollerinizin listesini görmek için **rollerimi** seçin.

1. **Azure kaynak rollerini**seçin.

1. Etkin **roller** sekmesini seçin.

1. Rol etkin olduğunda, portaldan çıkış ve tekrar oturum açın.

    Rolün artık kullanılmaması gerekir.

## <a name="view-the-status-of-your-requests"></a>İsteklerinizin durumunu görüntüleme

Etkinleştirmek için bekleyen isteklerinizin durumunu görüntüleyebilirsiniz.

1. Azure AD Ayrıcalıklı Kimlik Yönetimi'ni açın.

1. Azure REKLAM rolünüzün ve Azure kaynak rol isteklerinizin listesini görmek için **isteklerimi** seçin.

    ![İsteklerim - Bekleyen isteklerinizi gösteren Azure kaynak sayfası](./media/pim-resource-roles-activate-your-roles/resources-my-requests.png)

1. **İstek Durumu** sütununa görüntülemek için sağa kaydırın.

## <a name="cancel-a-pending-request"></a>Bekleyen isteği iptal etme

Onay gerektiren bir rolün etkinleştirilmesine gerek yoksa, bekleyen bir isteği istediğiniz zaman iptal edebilirsiniz.

1. Azure AD Ayrıcalıklı Kimlik Yönetimi'ni açın.

1. **İsteklerimi**seçin.

1. İptal etmek istediğiniz rol için **İptal** bağlantısını seçin.

    İptal'i seçtiğinizde, istek iptal edilir. Rolü yeniden etkinleştirmek için etkinleştirme için yeni bir istek göndermeniz gerekir.

   ![İptal eylemi vurgulanan istek listem](./media/pim-resource-roles-activate-your-roles/resources-my-requests-cancel.png)

## <a name="troubleshoot"></a>Sorun giderme

### <a name="permissions-are-not-granted-after-activating-a-role"></a>Rol etkinleştirildikten sonra izinler verilmiyor

Ayrıcalıklı Kimlik Yönetimi'nde bir rolü etkinleştirdiğinizde, etkinleştirme ayrıcalıklı rol gerektiren tüm portallara anında yayılmayabilir. Bazı durumlarda değişiklik yayılsa bile portalda web önbelleği değişikliğin anında geçerlilik kazanmamasına yol açabilir. Etkinleştirme niz gecikirse, yapmanız gerekenler şunlardır.

1. Azure portalında oturumunuzu kapatın ve sonra yeniden oturum açın.

    Bir Azure kaynak rolünü etkinleştirdiğinizde, etkinleştirme aşamanızı görürsünüz. Tüm aşamalar tamamlandıktan sonra bir **Oturumu kapat** bağlantısı görürsünüz. Oturum sonlarını kullanmak için bu bağlantıyı kullanabilirsiniz. Bu etkinleştirme gecikmesi için çoğu durumda çözecektir.

1. Ayrıcalıklı Kimlik Yönetimi'nde, rolün üyesi olarak listelendinizgini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rollerini genişletme veya yenileme](pim-resource-roles-renew-extend.md)
- [Ayrıcalıklı Kimlik Yönetimi'ndeki Azure REKLAM rollerimi etkinleştirme](pim-how-to-activate-role.md)
