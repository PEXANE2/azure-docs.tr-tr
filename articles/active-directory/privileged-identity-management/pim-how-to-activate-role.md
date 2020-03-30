---
title: AZURE AD rollerimi PIM'de etkinleştirin - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure AD rollerini nasıl etkinleştirdiğinizi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/28/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f95a1a08189668e5b6f88941069566b00a73bce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77499162"
---
# <a name="activate-my-azure-ad-roles-in-pim"></a>PIM'de Azure AD rollerimi etkinleştirme

Azure Etkin Dizin (Azure AD) Ayrıcalıklı Kimlik Yönetimi (PIM), işletmelerin Azure AD'deki kaynaklara ve Office 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerine ayrıcalıklı erişimi nasıl yönetebildiğini basitleştirir.  

İdari bir rol için uygun hale getirilmişseniz, ayrıcalıklı eylemler gerçekleştirmeniz gerektiğinde bu rolü etkinleştirebilirsiniz. Örneğin, zaman zaman Office 365 özelliklerini yönetiyorsanız, kuruluşunuzun ayrıcalıklı rol yöneticileri sizi kalıcı bir Genel Yönetici yapmayabilir, çünkü bu rol diğer hizmetleri de etkiler. Bunun yerine, Exchange Online Administrator gibi Azure REKLAM rolleri için uygun hale getirmez. Bu rolü ayrıcalıklara gereksinim duyduğunuzda etkinleştirme isteğinde bulunabilirsiniz ve ardından önceden belirlenmiş bir süre için yönetici denetimine sahip olabilirsiniz.

Bu makale, Ayrıcalıklı Kimlik Yönetimi'ndeki Azure AD rollerini etkinleştirmeleri gereken yöneticiler içindir.

## <a name="determine-your-version-of-pim"></a>PIM sürümünüzü belirleyin

Kasım 2019'dan itibaren, Ayrıcalıklı Kimlik Yönetimi'nin Azure AD rolleri bölümü, Azure kaynak rollerinin deneyimleriyle eşleşen yeni bir sürüme güncelleniyor. Bu, [varolan API'de değişikliklerin](azure-ad-roles-features.md#api-changes)yanı sıra ek özellikler de oluşturur. Yeni sürüm kullanıma alınırken, bu makalede hangi yordamları izleyeceğiniz, şu anda sahip olduğunuz Ayrıcalıklı Kimlik Yönetimi sürümüne bağlıdır. Ayrıcalıklı Kimlik Yönetimi'nin hangi sürümüne sahip olduğunuzu belirlemek için bu bölümdeki adımları izleyin. Ayrıcalıklı Kimlik Yönetimi sürümünüzü aldıktan sonra, bu makalede bu sürümle eşleşen yordamları seçebilirsiniz.

1. [Ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolüyle Azure [portalında](https://portal.azure.com/) oturum açın.
1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın. Genel bakış sayfasının üst kısmında bir başlık varsa, bu makalenin **Yeni sürüm** sekmesindeki yönergeleri izleyin. Aksi takdirde, Önceki **sürüm** sekmesindeki yönergeleri izleyin.

    [![](media/pim-how-to-add-role-to-user/pim-new-version.png "Select Azure AD > Privileged Identity Management")](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox)

# <a name="new-version"></a>[Yeni sürüm](#tab/new)

## <a name="activate-a-role"></a>Bir rolü etkinleştirme

Bir Azure AD rolünü üstlenmeniz gerektiğinde, Ayrıcalıklı Kimlik Yönetimi'nde **Rollerim** gezintiseçeneğini kullanarak etkinleştirme isteğinde bulunabilirsiniz.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın. Panonuza Ayrıcalıklı Kimlik Yönetimi döşemesinin nasıl eklenöğretilen hakkında bilgi [için](pim-getting-started.md)bkz.

1. **Rollerim'i**seçin ve ardından uygun Azure REKLAM rollerinizin listesini görmek için **Azure REKLAM rollerini** seçin.

    ![Etkinleştirebileceğiniz rolleri gösteren roller sayfam](./media/pim-how-to-activate-role/my-roles.png)

1. Azure **AD rolleri** listesinde etkinleştirmek istediğiniz rolü bulun.

    ![Azure AD rolleri - Uygun roller listem](./media/pim-how-to-activate-role/activate-link.png)

1. Etkinleştir bölmesini açmak için **Etkinleştir'i** seçin.

    ![Azure AD rolleri - etkinleştirme sayfası süre ve kapsam içerir](./media/pim-how-to-activate-role/activate-page.png)

1. Rolünüz çok faktörlü kimlik doğrulama gerektiriyorsa, **devam etmeden önce kimliğinizi doğrulayın'ı**seçin. Oturum başına yalnızca bir kez kimlik doğrulamanız gerekir.

    ![Rol etkinleştirmeden önce kimliğimi MFA ile doğrulayın](./media/pim-resource-roles-activate-your-roles/resources-my-roles-mfa.png)

1. **Kimliğimi doğrula'yı** seçin ve ek güvenlik doğrulaması sağlamak için yönergeleri izleyin.

    ![PIN kodu gibi güvenlik doğrulaması sağlamak için ekran](./media/pim-resource-roles-activate-your-roles/resources-mfa-enter-code.png)

1. Azaltılmış bir kapsam belirtmek istiyorsanız, filtre bölmesini açmak için **Kapsam'ı** seçin. Filtre bölmesine erişmeniz gereken Azure AD kaynaklarını belirtebilirsiniz. Yalnızca ihtiyacınız olan kaynaklara erişim istemek en iyi yöntemdir.

1. Gerekirse, özel bir etkinleştirme başlangıç saati belirtin. Azure AD rolü, seçili süreden sonra etkinleştirilir.

1. **Reason** kutusuna etkinleştirme isteğinin nedenini girin.

1. **Etkinleştir'i**seçin.

    Rol onay gerektirmiyorsa, etkinleştirilir ve etkin roller listesine eklenir. Rolü kullanmak istiyorsanız, sonraki bölümdeki adımları izleyin.

    ![Tamamlanmış Kapsam, başlangıç saati, süre ve neden ile bölmeyi etkinleştirme](./media/pim-how-to-activate-role/azure-ad-activation-status.png)

    Rol etkinleştirmek için [onay gerektiriyorsa,](pim-resource-roles-approval-workflow.md) tarayıcınızın sağ üst köşesinde isteğin onay beklemede olduğunu bildiren bir bildirim görüntülenir.

    ![Etkinleştirme isteği onay bildirimi bekliyor](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-notification.png)

## <a name="use-a-role-immediately-after-activation"></a>Etkinleştirmeden hemen sonra bir rol kullanma

Etkinleştirmeden sonra herhangi bir gecikme durumunda, Azure REKLAM rollerinizi hemen kullanmak için etkinleştirdikten sonra aşağıdaki adımları izleyin.

1. Azure AD Ayrıcalıklı Kimlik Yönetimi'ni açın.

1. Uygun Azure REKLAM rollerinizin ve Azure kaynak rollerinizin listesini görmek için **rollerimi** seçin.

1. **Azure AD rollerini**seçin.

1. Etkin **roller** sekmesini seçin.

1. Rol etkin olduğunda, portaldan çıkış ve tekrar oturum açın.

    Rolün artık kullanılmaması gerekir.

## <a name="view-the-status-of-your-requests"></a>İsteklerinizin durumunu görüntüleme

Etkinleştirmek için bekleyen isteklerinizin durumunu görüntüleyebilirsiniz.

1. Azure AD Ayrıcalıklı Kimlik Yönetimi'ni açın.

1. Azure REKLAM rolünüzün ve Azure kaynak rol isteklerinizin listesini görmek için **isteklerimi** seçin.

    ![İsteklerim - Bekleyen isteklerinizi gösteren Azure REKLAM sayfası](./media/pim-how-to-activate-role/my-requests-page.png)

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

    Bir Azure REKLAM rolünü etkinleştirdiğinizde, etkinleştirme aşamanızın aşamalarını görürsünüz. Tüm aşamalar tamamlandıktan sonra bir **Oturumu kapat** bağlantısı görürsünüz. Oturum sonlarını kullanmak için bu bağlantıyı kullanabilirsiniz. Bu etkinleştirme gecikmesi için çoğu durumda çözecektir.

1. Ayrıcalıklı Kimlik Yönetimi'nde, rolün üyesi olarak listelendinizgini doğrulayın.

# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

## <a name="activate-a-role"></a>Bir rolü etkinleştirme

Azure AD rolünü üstlenmeniz gerektiğinde, Ayrıcalıklı Kimlik Yönetimi'nde **Rollerim** gezinti seçeneğini kullanarak etkinleştirme isteğinde bulunabilirsiniz.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın. Panonuza Ayrıcalıklı Kimlik Yönetimi döşemesinin nasıl eklenöğretilen hakkında bilgi [için](pim-getting-started.md)bkz.

1. **Azure AD rollerini**tıklatın.

1. Uygun Azure REKLAM rollerinizin listesini görmek için **rollerim'i** tıklatın.

    ![Azure AD rolleri - Uygun veya etkin roller listesini gösteren rollerim](./media/pim-how-to-activate-role/directory-roles-my-roles.png)

1. Etkinleştirmek istediğiniz bir rol bulun.

    ![Azure AD rolleri - Etkinleştir bağlantısını gösteren uygun roller listem](./media/pim-how-to-activate-role/directory-roles-my-roles-activate.png)

1. Rol etkinleştirme ayrıntıları bölmesini açmak için **Etkinleştir'i** tıklatın.

1. Rolünüz çok faktörlü kimlik doğrulama (MFA) gerektiriyorsa, **devam etmeden önce kimliğinizi doğrulayın'ı**tıklatın. Oturum başına yalnızca bir kez kimlik doğrulamanız gerekir.

    ![Rol etkinleştirmeden önce kimlik bölmemi MFA ile doğrulama](./media/pim-how-to-activate-role/directory-roles-my-roles-mfa.png)

1. **Kimliğimi doğrula'yı** tıklatın ve ek güvenlik doğrulaması sağlamak için yönergeleri izleyin.

    ![Sizinle nasıl iletişim kurabileceğinizi soran ek güvenlik doğrulama sayfası](./media/pim-how-to-activate-role/additional-security-verification.png)

1. Etkinleştirme bölmesini açmak için **Etkinleştir'i** tıklatın.

    ![Başlangıç saatini, süresini, biletini ve nedenini belirtmek için etkinleştirme bölmesi](./media/pim-how-to-activate-role/directory-roles-activate.png)

1. Gerekirse, özel bir etkinleştirme başlangıç saati belirtin.

1. Etkinleştirme süresini belirtin.

1. **Etkinleştirme nedeni** kutusuna etkinleştirme isteğinin nedenini girin. Bazı roller, sorunlu bilet numarasını sağlamanızı gerektirir.

    ![Özel bir başlangıç zamanı, süresi, bilet ve nedeni ile tamamlanmış Etkinleştirme bölmesi](./media/pim-how-to-activate-role/directory-roles-activation-pane.png)

1. **Etkinleştir'i**tıklatın.

    Rol onay gerektirmiyorsa, etkinleştirme durumunu görüntüleyen bir **Etkinleştirme durum** bölmesi görüntülenir.

    ![Etkinleştirme nin üç aşamasını gösteren etkinleştirme durumu sayfası](./media/pim-how-to-activate-role/activation-status.png)

    Tüm aşamalar tamamlandıktan sonra, Azure portalından çıkış yapmak için **Oturum Dışı** bağlantısını tıklatın. Portala geri oturum açdiğinizde, artık rolü kullanabilirsiniz.

    Rol etkinleştirmek için [onay gerektiriyorsa,](./azure-ad-pim-approval-workflow.md) tarayıcınızın sağ üst köşesinde bir Azure bildirimi görünür ve isteğin onay beklemede olduğunu bildirir.

## <a name="view-the-status-of-your-requests"></a>İsteklerinizin durumunu görüntüleme

Etkinleştirmek için bekleyen isteklerinizin durumunu görüntüleyebilirsiniz.

1. Azure AD Ayrıcalıklı Kimlik Yönetimi'ni açın.

1. **Azure AD rollerini**tıklatın.

1. İsteklerinizin listesini görmek için **isteklerimi** tıklatın.

    ![Azure REKLAM rolleri - İstek listem](./media/pim-how-to-activate-role/directory-roles-my-requests.png)

## <a name="deactivate-a-role"></a>Rolü devre dışı bırakma

Bir rol etkinleştirildikten sonra, zaman sınırına (uygun süre) ulaşıldığında otomatik olarak devre dışı bırakır.

Yönetici görevlerinizi erken tamamlarsanız, Azure AD Ayrıcalıklı Kimlik Yönetimi'nde bir rolü el ile devre dışı bırakabilirsiniz.

1. Azure AD Ayrıcalıklı Kimlik Yönetimi'ni açın.

1. **Azure AD rollerini**tıklatın.

1. **Rollerimi**tıklatın.

1. Etkin roller listenizi görmek için **Etkin roller'i** tıklatın.

1. Kullandığınız rolü bulun ve ardından **Devre dışı bırak'ı**tıklatın.

## <a name="cancel-a-pending-request"></a>Bekleyen isteği iptal etme

Onay gerektiren bir rolün etkinleştirilmesine gerek yoksa, bekleyen bir isteği istediğiniz zaman iptal edebilirsiniz.

1. Azure AD Ayrıcalıklı Kimlik Yönetimi'ni açın.

1. **Azure AD rollerini**tıklatın.

1. **İsteklerimi**tıklatın.

1. İptal etmek istediğiniz rol için **İptal** düğmesini tıklatın.

    İptal'i tıklattığınızda, istek iptal edilir. Rolü yeniden etkinleştirmek için etkinleştirme için yeni bir istek göndermeniz gerekir.

   ![İptal düğmesi vurgulanan istek listem](./media/pim-how-to-activate-role/directory-role-cancel.png)

## <a name="troubleshoot"></a>Sorun giderme

### <a name="permissions-are-not-granted-after-activating-a-role"></a>Rol etkinleştirildikten sonra izinler verilmiyor

Ayrıcalıklı Kimlik Yönetimi'nde bir rolü etkinleştirdiğinizde, etkinleştirme ayrıcalıklı rol gerektiren tüm portallara anında yayılmayabilir. Bazı durumlarda değişiklik yayılsa bile portalda web önbelleği değişikliğin anında geçerlilik kazanmamasına yol açabilir. Etkinleştirme niz gecikirse, yapmanız gerekenler şunlardır.

1. Azure portalında oturumunuzu kapatın ve sonra yeniden oturum açın.

    Bir Azure REKLAM rolünü etkinleştirdiğinizde, etkinleştirme aşamanızın aşamalarını görürsünüz. Tüm aşamalar tamamlandıktan sonra bir **Oturumu kapat** bağlantısı görürsünüz. Oturum sonlarını kullanmak için bu bağlantıyı kullanabilirsiniz. Bu etkinleştirme gecikmesi için çoğu durumda çözecektir.

1. Ayrıcalıklı Kimlik Yönetimi'nde, rolün üyesi olarak listelendinizgini doğrulayın.

 ---

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'ndeki Azure REKLAM rollerimi etkinleştirme](pim-how-to-activate-role.md)
