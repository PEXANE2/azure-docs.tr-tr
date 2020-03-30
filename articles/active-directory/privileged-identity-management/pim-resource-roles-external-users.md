---
title: PIM'deki konuklara Azure kaynak rolleri atama - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) harici konuk kullanıcıları nasıl davet edinve Azure kaynak rollerini nasıl atayacaksınız öğrenin.
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
ms.openlocfilehash: 2efcf77d65fa2f9e203ed805cd7d78b9802ee3aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74021933"
---
# <a name="invite-guest-users-and-assign-azure-resource-roles-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde konuk kullanıcıları davet edin ve Azure kaynak rollerini atayın

Azure Etkin Dizin (Azure AD) konuk kullanıcıları, Azure AD'de harici konuk kullanıcıları ve satıcıları konuk olarak yönetebilmeniz için Azure AD'deki işletmeden işletmeye (B2B) işbirliği özelliklerinin bir parçasıdır. B2B işbirliğini Azure AD Ayrıcalıklı Kimlik Yönetimi (PIM) ile birleştirdiğinizde, uyumluluk ve yönetim gereksinimlerinizi konuklara genişletebilirsiniz. Örneğin, azure kimlik görevleri için bu Ayrıcalıklı Kimlik Yönetimi özelliklerini konuklarla birlikte kullanabilirsiniz:

- Belirli Azure kaynaklarına erişim atama
- Tam zamanında erişimi etkinleştirme
- Atama süresini ve bitiş tarihini belirtin
- Etkin atama veya etkinleştirme de çok faktörlü kimlik doğrulama gerektirir
- Erişim incelemelerini gerçekleştirin
- Uyarıları ve denetim günlüklerini kullanma

Bu makalede, bir konuğu kuruluşunuza nasıl davet edeceğinizi ve Ayrıcalıklı Kimlik Yönetimi'ni kullanarak Azure kaynaklarına erişimlerini nasıl yönetebilirsiniz.

## <a name="when-would-you-invite-guests"></a>Konukları ne zaman davet ederdin?

Konukları kuruluşunuza ne zaman davet edebileceğinize birkaç örnek aşağıda verilmiştir:

- Yalnızca bir e-posta hesabı olan harici bir serbest çalışan satıcının proje için Azure kaynaklarınıza erişmesine izin verin.
- Harcama başvurunuza erişmek için şirket içi Active Directory Federation Services kullanan büyük bir kuruluşta harici bir iş ortağına izin verin.
- Sorunları gidermek için kuruluşunuzdaki (Microsoft desteği gibi) destek mühendislerinin Azure kaynağınıza geçici olarak erişmesine izin verin.

## <a name="how-does-collaboration-using-b2b-guests-work"></a>B2B konuklarını kullanarak işbirliği nasıl çalışır?

B2B işbirliğini kullandığınızda, harici bir kullanıcıyı kuruluşunuza konuk olarak davet edebilirsiniz. Konuk kuruluşunuzda bir kullanıcı olarak yönetilebilir, ancak bir konuğun Azure REKLAM kuruluşunuzda değil, kendi kuruluşunda kimlik doğrulaması gerekir. Bu, konuğun artık kendi ev kuruluşuna erişimi yoksa, kuruluşunuzun erişimini de kaybettiği anlamına gelir. Örneğin, konuk kuruluşunu terk ederse, azure AD'de paylaştığınız tüm kaynaklara, siz hiçbir şey yapmanıza gerek kalmadan otomatik olarak erişimini kaybeder. B2B işbirliği hakkında daha fazla bilgi için Azure [Active Directory B2B'de konuk kullanıcı erişimi nedir?](../b2b/what-is-b2b.md)

![Konuk kullanıcının ev dizininde nasıl kimlik doğrulamasını gösteren diyagram](./media/pim-resource-roles-external-users/b2b-external-user.png)

## <a name="check-guest-collaboration-settings"></a>Konuk işbirliği ayarlarını kontrol edin

Konukları kuruluşunuza davet edebileceğinizden emin olmak için, konuk işbirliği ayarlarınızı kontrol etmelisiniz.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. **Azure Etkin Dizin** > **Kullanıcı ayarlarını**seçin.

1. **Dış işbirliği ayarlarını yönet'i**seçin.

    ![İzin, davet ve işbirliği kısıtlama ayarlarını gösteren dış işbirliği ayarları sayfası](./media/pim-resource-roles-external-users/external-collaboration-settings.png)

1. **Yöneticive konuk davetli rolündeki kullanıcıların geçişi davet edebilmesini** sağlamak **Evet**olarak ayarlanır.

## <a name="invite-a-guest-and-assign-a-role"></a>Bir konuk davet edin ve bir rol atayın

Ayrıcalıklı Kimlik Yönetimi'ni kullanarak, bir konuk davet edebilir ve azure kaynak rolü için uygun hale getirebilirsiniz.

1. [Ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) veya Kullanıcı [Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#user-administrator) rolünün üyesi bir kullanıcıyla [Azure portalında](https://portal.azure.com/) oturum açın.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure kaynaklarını**seçin.

1. Yönetilen kaynaklar listesini filtrelemek için **Kaynak filtresini** kullanın.

1. Kaynak, kaynak grubu, abonelik veya yönetim grubu gibi yönetmek istediğiniz kaynağı seçin.

    Kapsamı yalnızca konuğun ihtiyacı olan şeye ayarlamalısınız.

1. Yönet'in altında, Azure kaynaklarının rol listesini görmek için **Roller'i** seçin.

    ![Etkin ve uygun kullanıcı sayısını gösteren Azure kaynakları rolleri listesi](./media/pim-resource-roles-external-users/resources-roles.png)

1. Kullanıcının gereksinim edeceği minimum rolü seçin.

    ![Bu rolün geçerli üyelerini listeleyen seçili rol sayfası](./media/pim-resource-roles-external-users/selected-role.png)

1. Rol sayfasında, Yeni atama bölmesini açmak için **üye ekle'yi** seçin.

1. **Üye veya grup seçin'i**tıklatın.

    ![Yeni atama - Davet seçeneğiyle birlikte kullanıcıları ve grupları listeleyen bir üye veya grup bölmesi seçin](./media/pim-resource-roles-external-users/select-member-group.png)

1. Bir konuğu davet etmek için **Davet Et'i**tıklatın.

    ![Bir e-posta adresi girmek ve kişisel bir mesaj belirtmek için kutulu bir konuk sayfasını davet etme](./media/pim-resource-roles-external-users/invite-guest.png)

1. Bir konuk seçtikten sonra **Davet Et'i**tıklatın.

    Konuk seçilen bir üye olarak eklenmelidir.

1. Üye **veya grup** bölmesi seç'te **Seç'i**tıklatın.

1. Üyelik **ayarları** bölmesinde atama türünü ve süresini seçin.

    ![Yeni atama - Atama türünü, başlangıç tarihini ve bitiş tarihini belirtme seçenekleri olan üyelik ayarları sayfası](./media/pim-resource-roles-external-users/membership-settings.png)

1. Atamayı tamamlamak için **Bitti'yi** seçin ve sonra ekle' yi **seçin.**

    Konuk rol ataması rol listenizde görünür.

    ![Konuk ları uygun olarak listeleyen rol sayfası](./media/pim-resource-roles-external-users/role-assignment.png)

## <a name="activate-role-as-a-guest"></a>Konuk olarak rolü etkinleştirme

Harici bir kullanıcıysanız, Azure REKLAM kuruluşuna konuk olma davetini kabul etmeli ve büyük olasılıkla rol atamanızı etkinleştirmelisiniz.

1. Davetinizle birlikte e-postayı açın. E-posta aşağıdakine benzer olacaktır.

    ![Dizin adı, kişisel mesaj ve Başlangıç düğmesi yle e-posta daveti](./media/pim-resource-roles-external-users/email-invite.png)

1. E-postadaki **Başlat bağlantısını** seçin.

1. İzinleri inceledikten sonra **Kabul Et'i**tıklatın.

    ![Kuruluşun gözden geçirmenizi istediği izinlerin listesini içeren bir tarayıcıdaki izinler sayfasını gözden geçirme](./media/pim-resource-roles-external-users/invite-accept.png)

1. Bir kullanım koşullarını kabul etmeniz ve oturum içinde kalmak isteyip istemediğiniz belirtilmeniz istenebilir. Azure portalında, bir rol için *uygunsanız,* kaynaklara henüz erişiminiz olmaz.

1. Rol atamanızı etkinleştirmek için, etkinle rol bağlantınızla e-postayı açın. E-posta aşağıdakine benzer olacaktır.

    ![Etkinleştir'in rol bağlantısı yla bir rol için uygun olduğunuzu belirten e-posta](./media/pim-resource-roles-external-users/email-role-assignment.png)

1. Ayrıcalıklı Kimlik Yönetimi'ndeki uygun rollerinizi açmak için **Etkinleştir rolünü** seçin.

    ![Uygun rollerinizi listeleyen Ayrıcalıklı Kimlik Yönetimi'ndeki rollerim](./media/pim-resource-roles-external-users/my-roles-eligible.png)

1. Eylem altında, **Etkinleştir** bağlantısını seçin.

    Rol ayarlarına bağlı olarak, rolü etkinleştirmek için bazı bilgiler belirtmeniz gerekir.

1. Rolün ayarlarını belirttikten sonra, rolü etkinleştirmek için **Etkinleştir'i** tıklatın.

    ![Başlangıç saatini, süresini ve nedenini belirtmek için sayfa listeleme kapsamını ve seçeneklerini etkinleştirme](./media/pim-resource-roles-external-users/activate-role.png)

    Yöneticinin isteğinizi onaylaması gerekmedikçe, belirtilen kaynaklara erişiminiz olmalıdır.

## <a name="view-activity-for-a-guest"></a>Bir konuk için etkinliği görüntüleme

Konukların ne yaptığını takip etmek için denetim günlüklerini görüntüleyebilirsiniz.

1. Yönetici olarak, Ayrıcalıklı Kimlik Yönetimi'ni açın ve paylaşılan kaynağı seçin.

1. Bu kaynağın etkinliğini görüntülemek için **Kaynak denetimi'ni** seçin. Aşağıdaki bir kaynak grubu için etkinlik bir örnek gösterir.

    ![Azure kaynakları - Zaman, istek ve eylemi listeleyen kaynak denetim sayfası](./media/pim-resource-roles-external-users/audit-resource.png)

1. Konuğun etkinliğini görüntülemek için **Azure Etkin Dizin** > **Kullanıcıları** > *konuk adını*seçin.

1. Kuruluşun denetim günlüklerini görmek için **Denetim günlüklerini** seçin. Gerekirse, filtreler belirtebilirsiniz.

    ![Dizin denetim günlükleri listeleme tarihi, hedef, tarafından başlatılan ve etkinlik](./media/pim-resource-roles-external-users/audit-directory.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure AD yönetici rollerini atama](pim-how-to-add-role-to-user.md)
- [Azure AD B2B işbirliğinde konuk kullanıcı erişimi nedir?](../b2b/what-is-b2b.md)
