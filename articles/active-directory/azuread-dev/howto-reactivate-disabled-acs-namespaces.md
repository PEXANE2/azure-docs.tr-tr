---
title: Devre dışı bırakılmış Azure Erişim Denetim Hizmeti (ACS) ad alanlarını yeniden etkinleştirme
description: Azure Erişim Denetim Hizmeti (ACS) ad boşluklarınızı bulun ve etkinleştirin ve 4 Şubat 2019'a kadar bunları etkin tutmak için bir uzantı isteyin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: conceptual
ms.date: 01/21/2019
ms.author: ryanwi
ms.reviewer: jlu
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: ff0ce05b13fea8409475e3415c5d810d7c79769a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154874"
---
# <a name="how-to-reactivate-disabled-access-control-service-namespaces"></a>Nasıl yapılır: Devre dışı bırakılmış Erişim Denetimi Hizmeti ad alanlarını yeniden etkinleştirme

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Kasım 2017'de, Azure Active Directory (Azure AD) hizmeti olan Microsoft Azure Erişim Kontrol Hizmeti'nin (ACS) 7 Kasım 2018'de kullanımdan kaldırılmakta olduğunu duyurduk.

O zamandan beri ACS aboneliklerinin yönetici e-postasına ACS'nin 7 Kasım 2018'deki emeklilik tarihinden 12 ay, 9 ay, 6 ay, 3 ay, 1 ay, 2 hafta, 1 hafta ve 1 gün önce e-posta gönderdik.

3 Ekim 2018'de, göçlerini 7 Kasım 2018'den önce tamamlayamayan müşterilere (e-posta ve [blog gönderisi](https://azure.microsoft.com/blog/one-month-retirement-notice-access-control-service/)yoluyla) bir uzantı teklifi duyurduk. Duyuruda uzatma talebinde bulunulan talimatlar da vardı.

## <a name="why-your-namespace-is-disabled"></a>Ad alanınız neden devre dışı bırakılır?

Uzatmayı seçmediyseniz, 7 Kasım 2018'den itibaren ACS ad boşluklarını devre dışı bırakmaya başlarız. Uzatmayı 4 Şubat 2019'a kadar talep etmiş olmalısınız; aksi takdirde, PowerShell üzerinden ad boşluklarını etkinleştiremeniz gerekir.

> [!NOTE]
> PowerShell komutlarını çalıştırmak ve bir uzantı istemek için aboneliğin bir hizmet yöneticisi veya eş yöneticisi olmalısınız.

## <a name="find-and-enable-your-acs-namespaces"></a>ACS ad alanlarınızı bulun ve etkinleştirin

TÜM ACS ad boşluklarınızı listelemek ve devre dışı bırakılmış olanları yeniden etkinleştirmek için ACS PowerShell'i kullanabilirsiniz.

1. ACS PowerShell'i indirin ve kurun:
    1. PowerShell Galerisi'ne gidin ve [Acs.Namespaces](https://www.powershellgallery.com/packages/Acs.Namespaces/1.0.2)indirin.
    1. Modülü yükleyin:

        ```powershell
        Install-Module -Name Acs.Namespaces
        ```

    1. Olası tüm komutların listesini alın:

        ```powershell
        Get-Command -Module Acs.Namespaces
        ```

        Belirli bir komutla ilgili yardım almak için çalıştırın:

        ```powershell
        Get-Help [Command-Name] -Full
        ```
    
        ACS komutunun adı nerededir. `[Command-Name]`
1. **Connect-AcsAccount** cmdlet'i kullanarak ACS'ye bağlanın. 

    Komutu çalıştırmadan önce **Set-ExecutionPolicy'yi** çalıştırarak yürütme ilkenizi değiştirmeniz gerekebilir.
1. **Get-AcsSubscription** cmdlet'i kullanarak kullanılabilir Azure aboneliklerinizi listele.
1. **Get-AcsNamespace** cmdlet'i kullanarak ACS ad boşluklarınızı listele.
1. Ad boşluklarının `State` `Disabled`devre dışı bırakıldığını onaylayın.

    [![Ad boşluklarının devre dışı bırakıldığını doğrulayın](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png)](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png#lightbox)

    Etki alanının `nslookup {your-namespace}.accesscontrol.windows.net` hala etkin olup olmadığını doğrulamak için de kullanabilirsiniz.

1. **Enable-AcsNamespace** cmdlet'i kullanarak ACS ad alanınızı etkinleştirin.

    Ad alanınızı etkinleştirdikten sonra, ad alanının 4 Şubat 2019'dan önce tekrar devre dışı bırakılmaması için bir uzantı isteyebilirsiniz. Bu tarihten sonra, ACS'ye gelen tüm istekler başarısız olur.

## <a name="request-an-extension"></a>Uzantı isteme

21 Ocak 2019 tarihinden itibaren yeni uzatma talepleri alıyoruz.

4 Şubat 2019'a kadar uzantı isteyen müşteriler için ad alanlarını devre dışı bırakacağız. PowerShell üzerinden ad alanlarını yeniden etkinleştirebilirsiniz, ancak ad boşlukları 48 saat sonra yeniden devre dışı bırakılır.

4 Mart 2019'dan sonra müşteriler PowerShell üzerinden herhangi bir ad alanlarını yeniden etkinleştiremeyecek.

Diğer uzantılar artık otomatik olarak onaylanmayacaktır. Geçiş yapmak için ek zamana ihtiyacınız varsa, ayrıntılı bir geçiş zaman çizelgesi sağlamak için [Azure desteğine](https://portal.azure.com/#create/Microsoft.Support) başvurun.

### <a name="to-request-an-extension"></a>Uzantı istemek için

1. Azure portalında oturum açın ve yeni bir [destek isteği](https://portal.azure.com/#create/Microsoft.Support)oluşturun.
1. Aşağıdaki örnekte gösterildiği gibi yeni destek isteği formunu doldurun.

    | Destek isteği alanı | Değer |
    |-----------------------|--------------------|
    | **Sorun türü** | `Technical` |
    | **Abonelik** | Aboneliğinize ayarlayın |
    | **Hizmet** | `All services` |
    | **Kaynak** | `General question/Resource not available` |
    | **Sorun türü** | `ACS to SAS Migration` |
    | **Konu** | Sorunu açıklayın |

   ![Yeni bir teknik destek isteği örneği gösterir](./media/howto-reactivate-disabled-acs-namespaces/new-technical-support-request.png)

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

- Bu nasıl yapılacağını takip ettikten sonra herhangi bir sorunla karşılaştıysanız, [Azure desteğine](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)başvurun.
- ACS'nin emekliliği ile ilgili sorularınız acsfeedback@microsoft.comveya geri bildiriminiz varsa, bizimle iletişime geçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Erişim Denetim Hizmetinden Geçiş:](active-directory-acs-migration.md)AcS'nin emekliliği yle ilgili bilgileri gözden geçirin.
