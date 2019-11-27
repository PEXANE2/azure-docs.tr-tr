---
title: Gruplara duyarlılık etiketleri atama-Azure AD | Microsoft Docs
description: Grupları otomatik olarak doldurmak için üyelik kuralları ve bir kural başvurusu oluşturma.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/19/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 07859299805c5f7be869350adbdbfa675775888c
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74404800"
---
# <a name="assign-sensitivity-labels-to-office-365-groups-in-azure-active-directory-preview"></a>Azure Active Directory (Önizleme) içinde Office 365 gruplarına duyarlılık etiketleri atama

Azure Active Directory (Azure AD), [Microsoft 365 Uyumluluk Merkezi](https://sip.protection.office.com/homepage) tarafından Office 365 gruplarına yayınlanan duyarlılık etiketlerinin uygulanmasını destekler. Duyarlılık etiketleri Outlook, Microsoft ekipleri ve SharePoint gibi hizmetler arasında grup için geçerlidir. Bu özellik şu anda genel önizleme aşamasındadır.

> [!IMPORTANT]
> Office 365 grupları için Azure AD duyarlılık etiketlerinin kullanılması Azure Active Directory Premium P1 lisansı gerektirir.

## <a name="group-settings-controlled-by-labels"></a>Etiketlere göre denetlenen grup ayarları

Bir etiketle ilişkilendirilebilen iki ayar vardır:

- **Gizlilik**: Yöneticiler, bir grubun genel mi yoksa özel mi olduğunu denetlemek için bir gizlilik ayarını etiketle ilişkilendirebilirler.
- **Konuk erişimi**: Yöneticiler, atanmış etiketi olan tüm gruplar için konuk ilkesini zorunlu kılabilir. Bu ilke, konukların üye olarak eklenip eklenemeyeceğini belirtir. Konuk ilkesi bir etiket için yapılandırılmışsa, etiketi atadığınız tüm gruplar, AllowToAddGuests ayarının değiştirilmesine izin vermez.

## <a name="enable-sensitivity-label-support-in-powershell"></a>PowerShell 'de duyarlılık etiketi desteğini etkinleştir

Yayınlanmış etiketleri gruplara uygulamak için, önce özelliği etkinleştirmeniz gerekir. Bu adımlar, Azure AD 'deki özelliği etkinleştirir.

1. Bilgisayarınızda bir Windows PowerShell penceresi açın. Bunu yükseltilmiş ayrıcalıklar olmadan açabilirsiniz.
1. Ortamı cmdlet'leri çalıştırmaya hazır hale getirmek için aşağıdaki komutları çalıştırın.

    ```PowerShell
    Import-Module AzureADPreview
    Connect-AzureAD
    ```

    **Hesabınızda oturum açın** sayfasında, hizmetinize bağlanmak için yönetici hesabınızı ve parolanızı girin ve **oturum aç**' ı seçin.
1. Azure AD kuruluşu için geçerli grup ayarlarını getirin.

    ```PowerShell
    $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
    ```

    > [!NOTE]
    > Bu Azure AD kuruluşu için Grup ayarları oluşturulmadıysa, önce ayarları oluşturmanız gerekir. Bu Azure AD kuruluşunun grup ayarlarını oluşturmak için [Grup ayarlarını yapılandırmak üzere Azure Active Directory cmdlet 'lerinde](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-settings-cmdlets) adımları izleyin.

1. Sonra, geçerli grup ayarlarını görüntüleyin.

    ```PowerShell
    $Setting.Values
    ```

1. Sonra özelliği etkinleştirin:

    ```PowerShell
    $Setting["EnableMIPLabels"] = "True"
    ```

1. Ardından, değişiklikleri kaydedin ve ayarları uygulayın:

    ```PowerShell
    Set-AzureADDirectorySetting -Id $Setting.Id -DirectorySetting $Setting
    ```

Bu kadar. Özelliği etkinleştirdiniz ve gruplar için yayımlanmış Etiketler uygulayabilirsiniz.

## <a name="assign-a-label-to-a-new-group-in-azure-portal"></a>Azure portal yeni bir gruba etiket atama

1. [Azure AD Yönetim merkezinde](https://aad.portal.azure.com)oturum açın.
1. **Gruplar**' ı seçin ve ardından **Yeni Grup**' u seçin.
1. **Yeni Grup** sayfasında **Office 365**' i seçin ve ardından yeni grup için gerekli bilgileri doldurun ve listeden bir duyarlılık etiketi seçin.

   ![Yeni Gruplar sayfasında duyarlılık etiketi atama](./media/groups-assign-sensitivity-labels/new-group-page.png)

1. Değişikliklerinizi kaydedin ve **Oluştur**' u seçin.

Grubunuz oluşturulur ve seçili etiketle ilişkili ilkeler otomatik olarak zorlanır.

## <a name="assign-a-label-to-an-existing-group-in-azure-portal"></a>Azure portal var olan bir gruba etiket atama

1. [Azure AD Yönetim merkezinde](https://aad.portal.azure.com) genel yönetici veya gruplar yönetici hesabıyla veya grup sahibi olarak oturum açın.
1. **Grupları**seçin.
1. **Tüm gruplar** sayfasında, etiketlemek istediğiniz grubu seçin.
1. Seçili grubun sayfasında, **Özellikler** ' i seçin ve listeden bir duyarlılık etiketi seçin.

   ![Bir grup için genel bakış sayfasında duyarlılık etiketi atama](./media/groups-assign-sensitivity-labels/assign-to-existing.png)

1. Değişikliklerinizi kaydetmek için **Kaydet** ' i seçin.

## <a name="remove-a-label-from-an-existing-group-in-azure-portal"></a>Azure portal var olan bir gruptan bir etiketi kaldır

1. [Azure AD Yönetim merkezinde](https://aad.portal.azure.com) genel yönetici veya gruplar yönetici hesabıyla veya grup sahibi olarak oturum açın.
1. **Grupları**seçin.
1. **Tüm gruplar** sayfasında, etiketi kaldırmak istediğiniz grubu seçin.
1. **Grup** sayfasında, **Özellikler**' i seçin.
1. **Kaldır**’ı seçin.
1. Değişikliklerinizi uygulamak için **Kaydet** ' i seçin.

## <a name="office-365-app-support-for-sensitivity-labels"></a>Duyarlılık etiketleri için Office 365 uygulama desteği

Aşağıdaki Office 365 uygulamaları ve Hizmetleri, bu Önizlemedeki duyarlılık etiketlerini destekler:

- Azure AD Yönetim Merkezi
- Uyumluluk Merkezi Microsoft 365
- SharePoint
- Web 'de Outlook
- Ekipler
- SharePoint Yönetim Merkezi

Office 365 uygulamaları desteği hakkında daha fazla bilgi için bkz. [duyarlılık etiketleri Için office 365 desteği](https://docs.microsoft.com/microsoft-365/compliance/sensitivity-labels-teams-groups-sites#support-for-the-new-sensitivity-labels).

## <a name="using-classic-azure-ad-classifications"></a>Klasik Azure AD sınıflandırmalarını kullanma

Bu özelliği etkinleştirdikten sonra, Office 365 yeni gruplar için "klasik" sınıflandırmaları artık desteklememektedir. Klasik sınıflandırmalar, Azure AD PowerShell 'de `ClassificationList` ayarı için değerleri tanımlayarak ayarladığınız eski sınıflandırmalardır. Bu özellik etkinleştirildiğinde, bu sınıflandırmalar gruplara uygulanmaz.

## <a name="troubleshooting-issues"></a>Sorun giderme sorunları

### <a name="sensitivity-labels-are-not-available-for-assignment-on-a-group"></a>Duyarlılık etiketleri bir grupta atama için kullanılamaz

Duyarlılık etiketi seçeneği yalnızca aşağıdaki koşulların tümü karşılandığında gruplar için görüntülenir:

1. Etiketler, bu kiracı için Microsoft 365 Uyumluluk Merkezi 'nde yayımlanır.
1. Özellik etkin, PowerShell 'de Enablemıplabels true olarak ayarlandı.
1. Grup bir Office 365 grubudur.
1. Kiracının etkin bir Azure Active Directory Premium P1 lisansı vardır.
1. Geçerli oturum açmış kullanıcının yayımlanmış etiketlere erişimi var.
1. Geçerli oturum açan kullanıcının etiket atamak için yeterli ayrıcalıkları vardır. Kullanıcının bir genel yönetici, Grup Yöneticisi veya grup sahibi olması gerekir.
1. Şu anda oturum açmış olan kullanıcıya bir Office 365 lisansı atanmış. Lisans gereksinimleri hakkında daha fazla bilgi için bkz. [Office uygulamalarında duyarlılık etiketleri](https://docs.microsoft.com/microsoft-365/compliance/sensitivity-labels-office-apps).

Bir gruba etiket atamak için lütfen tüm koşulların karşılandığından emin olun.

### <a name="the-label-i-want-to-assign-is-not-in-the-list"></a>Atamak istediğim etiket listede değil

Aradığınız etiket listede yoksa, bu durum aşağıdaki nedenlerden biri olabilir:

- Etiket Microsoft 365 Uyumluluk Merkezi 'nde yayımlanmayabilir. Bu, artık yayımlanmayacak Etiketler için de uygulanabilir. Daha fazla bilgi için lütfen yöneticinize başvurun.
- Etiket yayımlanmayabilir, ancak oturum açmış olan kullanıcı tarafından kullanılamaz. Etikete erişim alma hakkında daha fazla bilgi için lütfen yöneticinize başvurun.

### <a name="how-can-i-change-the-label-on-a-group"></a>Bir gruptaki etiketi nasıl değiştirebilirim?

Etiketler, mevcut bir gruba etiket atama ile aynı adımları kullanarak herhangi bir zamanda değiştirilebilir:

1. Küresel veya grup yöneticisi hesabı veya grup sahibi olarak [Azure AD Yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
1. **Grupları**seçin.
1. **Tüm gruplar** sayfasında, etiketlemek istediğiniz grubu seçin.
1. Seçili grubun sayfasında, **Özellikler** ' i seçin ve listeden yeni bir duyarlılık etiketi seçin.
1. **Kaydet**’i seçin.

### <a name="group-setting-changes-to-published-labels-are-not-updated-on-the-groups"></a>Yayınlanmış etiketlere yapılan grup ayarı değişiklikleri gruplar üzerinde güncelleştirilmiyor

En iyi uygulama olarak, etiket gruplara uygulandıktan sonra bir etiketin grup ayarlarını değiştirmenizi önermiyoruz. [Microsoft 365 Uyumluluk Merkezi](https://sip.protection.office.com/homepage)'nde yayımlanan etiketlerle ilişkili Grup ayarlarında değişiklik yaptığınızda, bu ilke değişiklikleri etkilenen gruplara otomatik olarak uygulanmaz.

Değişiklik yapmanız gerekiyorsa, etkilenen gruplara el ile güncelleştirmeleri uygulamak için bir [Azure AD PowerShell betiği](https://github.com/microsoftgraph/powershell-aad-samples/blob/master/ReassignSensitivityLabelToO365Groups.ps1) kullanın. Bu yöntem, var olan tüm grupların yeni ayarı zorlayacağı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft ekipleri, Office 365 grupları ve SharePoint siteleri ile duyarlılık etiketleri kullanma](https://docs.microsoft.com/microsoft-365/compliance/sensitivity-labels-teams-groups-sites)
- [Azure AD PowerShell betiği ile etiket ilkesi el ile değiştirildikten sonra grupları güncelleştirme](https://github.com/microsoftgraph/powershell-aad-samples/blob/master/ReassignSensitivityLabelToO365Groups.ps1)
- [Grup ayarlarınızı düzenleme](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-settings-azure-portal)
- [PowerShell komutlarını kullanarak grupları yönetme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-settings-v2-cmdlets)
