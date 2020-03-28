---
title: Azure AD Etki Alanı Hizmetleri için parola karma eşitlemesini etkinleştirme | Microsoft Dokümanlar
description: Bu eğitimde, Azure AD Connect'i kullanarak Azure Active Directory Etki Alanı Hizmetleri yönetilen etki alanına parola karma senkronizasyonunu nasıl etkinleştirin öğrenin.
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: tutorial
ms.date: 02/10/2020
ms.author: iainfou
ms.openlocfilehash: 93e5ee9b46fb3387b70dd5092f72efcaa8a2bc19
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239171"
---
# <a name="tutorial-enable-password-synchronization-in-azure-active-directory-domain-services-for-hybrid-environments"></a>Öğretici: Karma ortamlar için Azure Active Directory Etki Alanı Hizmetlerinde parola eşitlemesini etkinleştirme

Karma ortamlar için, bir Azure Etkin Dizin (Azure AD) kiracısı, Azure AD Connect'i kullanarak şirket içi Active Directory Domain Services (AD DS) ortamıyla eşitlenecek şekilde yapılandırılabilir. Varsayılan olarak, Azure AD Connect, Azure Etkin Dizin Etki Alanı Hizmetleri (Azure AD DS) için gereken eski NT LAN Yöneticisi (NTLM) ve Kerberos parola çözümlerini eşitlemez.

Azure AD DS'yi şirket içi AD DS ortamından senkronize edilmiş hesaplarla kullanmak için, NTLM ve Kerberos kimlik doğrulaması için gereken parola çözümlerini eşitlemek için Azure AD Connect'i yapılandırmanız gerekir. Azure AD Connect yapılandırıldıktan sonra, şirket içi hesap oluşturma veya parola değiştirme olayı da eski parola çözümlerini Azure AD ile eşitler.

Şirket içinde AD DS ortamı olmayan yalnızca bulut hesapları kullanıyorsanız, bu adımları gerçekleştirmeniz gerekmez.

Bu öğreticide şunları öğreniyorsunuz:

> [!div class="checklist"]
> * Neden eski NTLM ve Kerberos şifre haşiyegereklidir
> * Azure AD Connect için eski parola karma senkronizasyonu nasıl yapılandırılabilen

Azure aboneliğiniz yoksa, başlamadan önce [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara ihtiyacınız var:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili ve Azure AD Connect'i kullanarak şirket içi bir diziniyle eşitlenmiş bir Azure Etkin Dizin kiracısı.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
    * Gerekirse, [parola karma eşitleme için Azure AD Connect'i etkinleştirin.][enable-azure-ad-connect]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, [bir Azure Etkin Dizin Etki Alanı Hizmetleri örneği oluşturun ve yapılandırın.][create-azure-ad-ds-instance]

## <a name="password-hash-synchronization-using-azure-ad-connect"></a>Azure AD Connect'i kullanarak parola karma senkronizasyonu

Azure AD Connect, şirket içi AD DS ortamından kullanıcı hesapları ve gruplar gibi nesneleri Azure AD kiracısına eşitlemek için kullanılır. İşlemin bir parçası olarak, parola karma eşitleme hesaplarının ön-prem AD DS ortamında ve Azure AD'de aynı parolayı kullanmasını sağlar.

Azure AD DS, yönetilen etki alanında kullanıcıların kimliğini doğrulamak için NTLM ve Kerberos kimlik doğrulaması için uygun bir biçimde parola hashes'e ihtiyaç duyar. Azure AD, parola hatalarını, kiracınız için Azure AD DS'sini etkinleştirene kadar NTLM veya Kerberos kimlik doğrulaması için gerekli biçimde depolamaz. Azure AD, güvenlik nedeniyle parola kimlik bilgilerini açık metin biçiminde de depolamaz. Bu nedenle, Azure AD bu NTLM veya Kerberos parola hatalarını kullanıcıların varolan kimlik bilgilerini temel alarak otomatik olarak oluşturamaz.

Azure AD Connect, Azure AD DS için gerekli NTLM veya Kerberos parola hatalarını eşitlemek üzere yapılandırılabilir. [Parola karma eşitleme için Azure AD Connect'i etkinleştirme][enable-azure-ad-connect]adımlarını tamamladığınızdan emin olun. Varolan bir Azure AD Connect örneğiniz varsa, NTLM ve Kerberos için eski parola hatalarını eşitleyebildiğinizden emin olmak için [en son sürüme indirin ve güncelleyin.][azure-ad-connect-download] Bu işlevsellik, Azure AD Connect'in ilk sürümlerinde veya eski DirSync aracında kullanılamaz. Azure AD Connect sürümü *1.1.614.0* veya sonraki sürüm gereklidir.

> [!IMPORTANT]
> Azure AD Connect yalnızca şirket içi AD DS ortamlarıyla senkronizasyon için yüklenmeli ve yapılandırılmalıdır. Azure AD Connect'i Azure AD DS yönetilen bir etki alanına yüklemek için desteklenmez.

## <a name="enable-synchronization-of-password-hashes"></a>Parola varlıklarının eşitlemesini etkinleştirme

Azure AD Connect yüklü ve Azure AD ile senkronize olacak şekilde yapılandırıldı, şimdi NTLM ve Kerberos için eski parola karma eşitlemesini yapılandırın. PowerShell komut dosyası, gerekli ayarları yapılandırmak ve ardından Azure AD için tam parola eşitlemesi başlatmak için kullanılır. Bu Azure AD Connect parola karma eşitleme işlemi tamamlandığında, kullanıcılar eski NTLM veya Kerberos parola karmalarını kullanan Azure AD DS aracılığıyla uygulamalarda oturum açabilir.

1. Azure AD Connect yüklü bilgisayarda Başlat menüsünden **Azure AD Connect > Senkronizasyon Hizmeti'ni**açın.
1. **Bağlayıcılar** sekmesini seçin. Şirket içi AD DS ortamı ile Azure AD ortamı arasındaki eşitlemayı oluşturmak için kullanılan bağlantı bilgileri listelenir.

    **Tür,** Azure AD bağlayıcısı için *Windows Azure Etkin Dizin (Microsoft)* veya şirket içi AD DS bağlayıcısı için *Active Directory Etki Alanı Hizmetlerini* gösterir. Bir sonraki adımda PowerShell komut dosyasında kullanılacak bağlayıcı adlarını not alın.

    ![Bağlayıcı adlarını Eşitleme Servis Yöneticisi'nde listele](media/tutorial-configure-password-hash-sync/service-sync-manager.png)

    Bu örnek ekran görüntüsünde aşağıdaki bağlayıcılar kullanılır:

    * Azure AD bağlayıcısı aaddscontoso.onmicrosoft.com olarak adlandırılır *- AAD*
    * Şirket içi AD DS konektörü *onprem.contoso.com*

1. Aşağıdaki PowerShell komut dosyasını Azure AD Connect yüklü bilgisayara kopyalayıp yapıştırın. Komut dosyası, eski parola işbadetlerini içeren tam bir parola eşitlemeyi tetikler. Önceki `$azureadConnector` adımdaki bağlayıcı adlarıyla ve `$adConnector` değişkenleri güncelleştirin.

    Şirket içi hesap NTLM ve Kerberos parola hashes'lerini Azure AD ile senkronize etmek için bu komut dosyasını her AD ormanında çalıştırın.

    ```powershell
    # Define the Azure AD Connect connector names and import the required PowerShell module
    $azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"
    $adConnector = "<CASE SENSITIVE AD DS CONNECTOR NAME>"
    
    Import-Module "C:\Program Files\Microsoft Azure AD Sync\Bin\ADSync\ADSync.psd1"
    Import-Module "C:\Program Files\Microsoft Azure Active Directory Connect\AdSyncConfig\AdSyncConfig.psm1"

    # Create a new ForceFullPasswordSync configuration parameter object then
    # update the existing connector with this new configuration
    $c = Get-ADSyncConnector -Name $adConnector
    $p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
    $p.Value = 1
    $c.GlobalParameters.Remove($p.Name)
    $c.GlobalParameters.Add($p)
    $c = Add-ADSyncConnector -Connector $c

    # Disable and re-enable Azure AD Connect to force a full password synchronization
    Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false
    Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true
    ```

    Hesap ve grup sayısı açısından dizininizin boyutuna bağlı olarak, eski parola karfetlerinin Azure AD ile eşitlenmesi biraz zaman alabilir. Parolalar, Azure AD ile eşitlendikten sonra Azure AD DS yönetilen etki alanına eşitlenir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> * Neden eski NTLM ve Kerberos şifre haşiyegereklidir
> * Azure AD Connect için eski parola karma senkronizasyonu nasıl yapılandırılabilen

> [!div class="nextstepaction"]
> [Azure AD Etki Alanı Hizmetleri yönetilen etki alanında eşitlemenin nasıl çalıştığını öğrenin](synchronization.md)

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[enable-azure-ad-connect]: ../active-directory/hybrid/how-to-connect-install-express.md

<!-- EXTERNAL LINKS -->
[azure-ad-connect-download]: https://www.microsoft.com/download/details.aspx?id=47594
