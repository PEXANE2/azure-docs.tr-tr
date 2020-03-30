---
title: Gruplara duyarlılık etiketleri atama - Azure AD | Microsoft Dokümanlar
description: Grupları otomatik olarak doldurmak için üyelik kuralları ve kural başvurusu oluşturma.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 02/24/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 667fb39aabfec14cff01221b82a45ba8ad1d68d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78329741"
---
# <a name="assign-sensitivity-labels-to-office-365-groups-in-azure-active-directory-preview"></a>Azure Etkin Dizini'nde Office 365 gruplarına duyarlılık etiketleri atama (önizleme)

Azure Etkin Dizin (Azure AD), [Microsoft 365 uyumluluk merkezi](https://sip.protection.office.com/homepage) tarafından yayınlanan duyarlılık etiketlerinin Office 365 gruplarına uygulanmasını destekler. Outlook, Microsoft Teams ve SharePoint gibi hizmetler arasında gruplandırmaiçin duyarlılık etiketleri uygulanır. Bu özellik şu anda genel önizlemede dir. Office 365 uygulamaları desteği hakkında daha fazla bilgi [için, duyarlılık etiketleri için Office 365 desteğine](https://docs.microsoft.com/microsoft-365/compliance/sensitivity-labels-teams-groups-sites#support-for-the-sensitivity-labels)bakın.

> [!IMPORTANT]
> Bu özelliği yapılandırmak için Azure REKLAM kuruluşunuzda en az bir etkin Azure Active Directory Premium P1 lisansı olmalıdır.

## <a name="enable-sensitivity-label-support-in-powershell"></a>PowerShell'de duyarlılık etiketi desteğini etkinleştirme

Yayımlanmış etiketleri gruplara uygulamak için önce özelliği etkinleştirmeniz gerekir. Bu adımlar Azure AD'deki özelliği etkinleştirin.

1. Bilgisayarınızda bir Windows PowerShell penceresi açın. Yüksek ayrıcalıklar olmadan açabilirsiniz.
1. Ortamı cmdlet'leri çalıştırmaya hazır hale getirmek için aşağıdaki komutları çalıştırın.

    ```PowerShell
    Import-Module AzureADPreview
    Connect-AzureAD
    ```

    Hesap **sayfanızda Oturum** Aç'ta, yönetici hesabınızı ve şifrenizi girerek sizi hizmetinize bağlayın ve **Oturum Aç'ı**seçin.
1. Azure AD kuruluşunun geçerli grup ayarlarını getirin.

    ```PowerShell
    $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
    ```

    > [!NOTE]
    > Bu Azure REKLAM kuruluşu için grup ayarları oluşturulmadıysa, öncelikle ayarları oluşturmanız gerekir. Bu Azure REKLAM kuruluşu için grup ayarlarını oluşturmak [üzere grup ayarlarını yapılandırmak için Azure Active Directory cmdlets'teki](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-settings-cmdlets) adımları izleyin.

1. Ardından, geçerli grup ayarlarını görüntüleyin.

    ```PowerShell
    $Setting.Values
    ```

1. Ardından özelliği etkinleştirin:

    ```PowerShell
    $Setting["EnableMIPLabels"] = "True"
    ```

1. Ardından değişiklikleri kaydedin ve ayarları uygulayın:

    ```PowerShell
    Set-AzureADDirectorySetting -Id $Setting.Id -DirectorySetting $Setting
    ```

İşte bu kadar. Özelliği etkinleştirdin ve yayımlanmış etiketleri gruplara uygulayabilirsiniz.

## <a name="assign-a-label-to-a-new-group-in-azure-portal"></a>Azure portalında yeni bir gruba etiket atama

1. Azure AD [yönetici merkezinde](https://aad.portal.azure.com)oturum açın.
1. **Gruplar'ı**seçin ve ardından **Yeni grubu**seçin.
1. Yeni **Grup** sayfasında **Office 365'i**seçin ve ardından yeni grup için gerekli bilgileri doldurun ve listeden bir duyarlılık etiketi seçin.

   ![Yeni gruplar sayfasında bir duyarlılık etiketi atama](./media/groups-assign-sensitivity-labels/new-group-page.png)

1. Değişikliklerinizi kaydedin ve **Oluştur'u**seçin.

Grubunuz oluşturulur ve seçili etiketle ilişkili site ve grup ayarları otomatik olarak zorlanır.

## <a name="assign-a-label-to-an-existing-group-in-azure-portal"></a>Azure portalında varolan bir gruba etiket atama

1. Azure AD [yönetici merkezinde](https://aad.portal.azure.com) Gruplar yöneticisi hesabıyla veya grup sahibi olarak oturum açın.
1. **Grupları**seçin.
1. Tüm **gruplar** sayfasından etiketlemek istediğiniz grubu seçin.
1. Seçili grubun sayfasında **Özellikler'i** seçin ve listeden bir duyarlılık etiketi seçin.

   ![Bir grup için genel bakış sayfasında bir duyarlılık etiketi atama](./media/groups-assign-sensitivity-labels/assign-to-existing.png)

1. Değişikliklerinizi kaydetmek için **Kaydet** seçeneğini belirleyin.

## <a name="remove-a-label-from-an-existing-group-in-azure-portal"></a>Azure portalında varolan bir gruptan etiket kaldırma

1. Azure AD [yönetici merkezinde](https://aad.portal.azure.com) Global yönetici veya Gruplar yöneticisi hesabıyla veya grup sahibi olarak oturum açın.
1. **Grupları**seçin.
1. Tüm **gruplar** sayfasından, etiketi kaldırmak istediğiniz grubu seçin.
1. **Grup** sayfasında **Özellikler'i**seçin.
1. **Kaldır**’ı seçin.
1. Değişikliklerinizi uygulamak için **Kaydet**’i seçin.

## <a name="using-classic-azure-ad-classifications"></a>Klasik Azure REKLAM sınıflandırmalarını kullanma

Bu özelliği etkinleştirdikten sonra, gruplar için "klasik" sınıflandırmalar yalnızca varolan gruplar ve siteler görünür ve bunları yalnızca duyarlılık etiketlerini desteklemeyen uygulamalarda gruplar oluştururken yeni gruplar için kullanmanız gerekir. Yöneticiniz gerekirse bunları daha sonra duyarlılık etiketlerine dönüştürebilir. Klasik sınıflandırmalar, Azure AD PowerShell'deki `ClassificationList` ayar için değerleri tanımlayarak ayarladığınız eski sınıflandırmalardır. Bu özellik etkinleştirildiğinde, bu sınıflandırmalar gruplara uygulanmaz.

## <a name="troubleshooting-issues"></a>Sorun giderme

### <a name="sensitivity-labels-are-not-available-for-assignment-on-a-group"></a>Duyarlılık etiketleri bir grupta atama için kullanılamaz

Duyarlılık etiketi seçeneği yalnızca aşağıdaki tüm koşullar yerine getirildiğinde gruplar için görüntülenir:

1. Etiketler, bu kiracı için Microsoft 365 Uyumluluk Merkezi'nde yayınlanır.
1. Özellik etkin, EnableMIPLabels PowerShell True olarak ayarlanır.
1. Grup bir Office 365 grubudur.
1. Kiracının etkin bir Azure Active Directory Premium P1 lisansı vardır.
1. Geçerli oturum açmış kullanıcı, etiketleri atamak için yeterli ayrıcalıklara sahiptir. Kullanıcı, Genel Yönetici, Grup Yöneticisi veya grup sahibi olmalıdır.

Lütfen bir gruba etiket atamak için tüm koşulların karşılandıklıolduğundan emin olun.

### <a name="the-label-i-want-to-assign-is-not-in-the-list"></a>Atamak istediğim etiket listede yok

Aradığınız etiket listede yoksa, aşağıdaki nedenlerden biri için durum böyle olabilir:

- Etiket Microsoft 365 Uyumluluk Merkezi'nde yayımlanmayabilir. Bu, artık yayınlanmamış etiketler için de geçerli olabilir. Daha fazla bilgi için lütfen yöneticinize danışın.
- Etiket yayımlanabilir, ancak oturum açan kullanıcı tarafından kullanılamaz. Etikete nasıl erişilenhakkında daha fazla bilgi için lütfen yöneticinize danışın.

### <a name="how-to-change-the-label-on-a-group"></a>Bir gruptaki etiket değiştirme

Etiketler, varolan bir gruba etiket atamayla aynı adımları kullanarak herhangi bir zamanda aşağıdaki gibi değiştirilebilir:

1. Azure AD [yönetici merkezinde,](https://aad.portal.azure.com) Global veya Grup yöneticisi hesabıyla veya grup sahibi olarak oturum açın.
1. **Grupları**seçin.
1. Tüm **gruplar** sayfasından etiketlemek istediğiniz grubu seçin.
1. Seçili grubun sayfasında **Özellikler'i** seçin ve listeden yeni bir duyarlılık etiketi seçin.
1. **Kaydet'i**seçin.

### <a name="group-setting-changes-to-published-labels-are-not-updated-on-the-groups"></a>Yayımlanmış etiketlerdeki grup ayarı değişiklikleri gruplarda güncelleştirilmeyecek

En iyi uygulama olarak, etiket gruplara uygulandıktan sonra etiketiçin grup ayarlarını değiştirmenizi önermiyoruz. [Microsoft 365 uyumluluk merkezinde](https://sip.protection.office.com/homepage)yayımlanmış etiketlerle ilişkili grup ayarlarında değişiklik yaptığınızda, bu ilke değişiklikleri etkilenen gruplarüzerinde otomatik olarak uygulanmaz.

Bir değişiklik yapmak zorundaysanız, etkilenen gruplara güncelleştirmeleri el ile uygulamak için bir [Azure AD PowerShell komut dosyası](https://github.com/microsoftgraph/powershell-aad-samples/blob/master/ReassignSensitivityLabelToO365Groups.ps1) kullanın. Bu yöntem, varolan tüm grupların yeni ayarı uyguladığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft Teams, Office 365 grupları ve SharePoint siteleri ile duyarlılık etiketleri kullanma](https://docs.microsoft.com/microsoft-365/compliance/sensitivity-labels-teams-groups-sites)
- [Azure AD PowerShell komut dosyasıyla etiket ilkesi değişikliğinden sonra grupları el ile güncelleştirme](https://github.com/microsoftgraph/powershell-aad-samples/blob/master/ReassignSensitivityLabelToO365Groups.ps1)
- [Grup ayarlarınızı düzenleme](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-settings-azure-portal)
- [PowerShell komutlarını kullanarak grupları yönetme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-settings-v2-cmdlets)
