---
title: Devre dışı bırakılan Azure Access Control Service (ACS) ad alanlarını yeniden etkinleştirme
description: Azure Access Control Service (ACS) ad alanlarınızı bulup etkinleştirin ve 4 Şubat 2019 ' e kadar etkin kalmasını sağlamak için bir uzantı isteyin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: how-to
ms.date: 01/21/2019
ms.author: ryanwi
ms.reviewer: jlu
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 7bb572e9e22519491290e54417f5ca350d6c0b5c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85383726"
---
# <a name="how-to-reactivate-disabled-access-control-service-namespaces"></a>Nasıl yapılır: devre dışı Access Control Service ad alanlarını yeniden etkinleştirme

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

2017 Kasım 'da, Microsoft Azure Access Control Service (ACS) Azure Active Directory (Azure AD) hizmeti, 7 Kasım 2018 tarihinde devre dışı bırakılmakta olduğunu duyurduk.

Bundan sonra, ACS aboneliklerini 3 ay, 9 ay, 6 ay, 3 ay, 1 ay, 2 hafta, 1 hafta ve 1 Kasım 2018 Kasım tarihine kadar bir gün önce ACS aboneliklerinin yönetici e-postasına e-postaları gönderdik.

3 Ekim 2018 ' de, geçiş işlemini 7 Kasım 2018 tarihinden önce tamamlayamıyorum müşterilere bir uzantı teklifi (e-posta ve [blog gönderisi](https://azure.microsoft.com/blog/one-month-retirement-notice-access-control-service/)aracılığıyla) duyurduk. Duyuruda, uzantıyı istemek için yönergeler vardı.

## <a name="why-your-namespace-is-disabled"></a>Ad alanınız neden devre dışı bırakıldı

Uzantıyı istemiyorsanız, 6 Kasım 2018 tarihinden itibaren ACS ad alanlarını devre dışı bırakacağız. Uzantıyı 4 Şubat 2019 ' de istemiş olmanız gerekir; Aksi takdirde, PowerShell aracılığıyla ad alanlarını etkinleştiremeyeceksiniz.

> [!NOTE]
> PowerShell komutlarını çalıştırmak ve bir uzantı istemek için bir hizmet yöneticisi veya aboneliğin ortak yöneticisi olmanız gerekir.

## <a name="find-and-enable-your-acs-namespaces"></a>ACS ad alanınızı bulun ve etkinleştirin

ACS PowerShell 'i, tüm ACS ad alanlarınızı listelemek ve devre dışı bırakılmış olanları yeniden etkinleştirmek için kullanabilirsiniz.

1. ACS PowerShell 'i indirme ve yükleme:
    1. PowerShell Galerisi gidin ve [ACS. Namespaces](https://www.powershellgallery.com/packages/Acs.Namespaces/1.0.2)' i indirin.
    1. Modülü yükler:

        ```powershell
        Install-Module -Name Acs.Namespaces
        ```

    1. Olası tüm komutların bir listesini alın:

        ```powershell
        Get-Command -Module Acs.Namespaces
        ```

        Belirli bir komut hakkında yardım almak için şunu çalıştırın:

        ```powershell
        Get-Help [Command-Name] -Full
        ```
    
        Burada `[Command-Name]` ACS komutunun adıdır.
1. **Connect-AcsAccount** cmdlet 'INI kullanarak ACS 'ye bağlanın. 

    Komutu çalıştırabilmeniz için, **set-executionpolicy** komutunu çalıştırarak yürütme ilkenizi değiştirmeniz gerekebilir.
1. **Get-AcsSubscription** cmdlet 'ini kullanarak kullanılabilir Azure aboneliklerinizi listeleyin.
1. **Get-AcsNamespace** cmdlet 'INI kullanarak ACS ad alanlarınızı listeleyin.
1. Olduğunu onayladıktan sonra ad alanlarının devre dışı bırakıldığını onaylayın `State` `Disabled` .

    [![Ad alanlarının devre dışı bırakıldığını onaylayın](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png)](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png#lightbox)

    Ayrıca, `nslookup {your-namespace}.accesscontrol.windows.net` etki alanının hala etkin olup olmadığını doğrulamak için ' i kullanabilirsiniz.

1. **Enable-AcsNamespace** cmdlet 'INI kullanarak ACS ad alanınızı etkinleştirin.

    Ad alanınızı etkinleştirdikten sonra, ad alanı (ler) 4 Şubat 2019 tarihinden önce yeniden devre dışı bırakılmayacak şekilde bir uzantı isteyebilirsiniz. Bu tarihten sonra, ACS 'ye gönderilen tüm istekler başarısız olur.

## <a name="request-an-extension"></a>Uzantı isteyin

21 Ocak 2019 tarihinden itibaren yeni uzantı istekleri sunuyoruz.

4 Şubat 2019 ' de uzantıları istemiş olan müşteriler için ad alanlarını devre dışı bırakacağız. Hala ad alanlarını PowerShell aracılığıyla yeniden etkinleştirebilirsiniz, ancak ad alanları 48 saat sonra yeniden devre dışı bırakılır.

4 Mart 2019 ' den sonra müşteriler artık PowerShell aracılığıyla ad alanlarını yeniden etkinleştiremeyecektir.

Daha fazla uzantı artık otomatik olarak onaylanmayacaktır. Geçiş için ek süre gerekiyorsa, ayrıntılı bir geçiş zaman çizelgesi sağlamak için [Azure desteğine](https://portal.azure.com/#create/Microsoft.Support) başvurun.

### <a name="to-request-an-extension"></a>Uzantı istemek için

1. Azure portal oturum açın ve [Yeni bir destek isteği](https://portal.azure.com/#create/Microsoft.Support)oluşturun.
1. Aşağıdaki örnekte gösterildiği gibi yeni destek isteği formunu girin.

    | Destek isteği alanı | Değer |
    |-----------------------|--------------------|
    | **Sorun türü** | `Technical` |
    | **Abonelik** | Aboneliğiniz için ayarlama |
    | **Hizmet** | `All services` |
    | **Kaynak** | `General question/Resource not available` |
    | **Sorun türü** | `ACS to SAS Migration` |
    | **Konu** | Sorunu açıkla |

   ![Yeni teknik destek isteğine bir örnek gösterir](./media/howto-reactivate-disabled-acs-namespaces/new-technical-support-request.png)

<!--

1. Navigate to your ACS namespace's management portal by going to `https://{your-namespace}.accesscontrol.windows.net`.
1. Select the **Read Terms** button to read the [updated Terms of Use](https://azure.microsoft.com/support/legal/access-control/), which will direct you to a page with the updated Terms of Use.

    [![Select the Read Terms button](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png#lightbox)

1. Select **Request Extension** on the banner at the top of the page. The button will only be enabled after you read the [updated Terms of Use](https://azure.microsoft.com/support/legal/access-control/).

    [![Select the Request Extension button](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png#lightbox)

1. After the extension request is registered, the page will refresh with a new banner at the top of the page.

    [![Updated page with refreshed banner](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png#lightbox)
-->

## <a name="help-and-support"></a>Yardım ve destek

- Bu nasıl yapılır uygulandıktan sonra herhangi bir sorunla karşılaşırsanız [Azure desteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)'ne başvurun.
- ACS kullanımdan kaldırma hakkında sorularınız veya geri bildiriminiz varsa, adresinden bizimle iletişim kurun acsfeedback@microsoft.com .

## <a name="next-steps"></a>Sonraki adımlar

- ACS kullanımdan kaldırma hakkında bilgileri gözden geçirin [: Azure Access Control Service geçiş](active-directory-acs-migration.md).
