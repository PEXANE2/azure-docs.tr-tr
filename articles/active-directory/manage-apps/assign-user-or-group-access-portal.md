---
title: Azure AD 'de bir kurumsal uygulamaya Kullanıcı veya Grup atama
description: Bir kurumsal uygulamayı seçerek bu gruba bir kullanıcı veya Grup atama Azure Active Directory
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 02/21/2020
ms.author: kenwith
ms.reviewer: luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2eeab9cbb68567b6af3b1381b8ec5e67e82461ec
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84763746"
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a>Azure Active Directory'de kurumsal uygulamaya kullanıcı veya grup atama

Bu makalede, Azure portal içinden veya PowerShell kullanarak Azure Active Directory (Azure AD) içindeki kurumsal uygulamalara Kullanıcı veya Grup atama işlemlerinin nasıl yapılacağı gösterilir. Bir uygulamayı bir kullanıcıya atadığınızda, uygulama kolay erişim için kullanıcının [uygulamalar erişim panelinde](https://myapps.microsoft.com/) görüntülenir. Uygulama rolleri kullanıma sunarsa, kullanıcıya belirli bir rol de atayabilirsiniz.

Daha fazla denetim için, bazı kurumsal uygulama türleri [Kullanıcı Ataması gerektirecek](#configure-an-application-to-require-user-assignment)şekilde yapılandırılabilir. 

[Bir kullanıcı veya grubu kurumsal bir uygulamaya atamak](#assign-users-or-groups-to-an-app-via-the-azure-portal)için, bir genel yönetici, uygulama Yöneticisi, bulut uygulaması Yöneticisi veya kurumsal uygulamanın atanmış sahibi olarak oturum açmanız gerekir.

> [!NOTE]
> Grup tabanlı atama Azure Active Directory Premium P1 veya P2 Edition gerektirir. Grup tabanlı atama yalnızca güvenlik grupları için desteklenir. İç içe geçmiş grup üyelikleri ve Office 365 grupları Şu anda desteklenmemektedir. Bu makalede ele alınan özelliklerin daha fazla lisanslama gereksinimi için [Azure Active Directory fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/active-directory)bakın. 

## <a name="configure-an-application-to-require-user-assignment"></a>Bir uygulamayı kullanıcı ataması gerektirecek şekilde yapılandırma

Aşağıdaki uygulama türleri ile, kullanıcıların uygulamaya erişebilmesi için kullanıcılara atanmasını isteme seçeneğiniz vardır:

- SAML tabanlı kimlik doğrulaması ile federe çoklu oturum açma (SSO) için yapılandırılmış uygulamalar
- Azure Active Directory ön kimlik doğrulaması kullanan uygulama proxy uygulamaları
- Bir kullanıcı veya yönetici bu uygulamaya alındıktan sonra OAuth 2,0/OpenID Connect kimlik doğrulaması kullanan Azure AD uygulama platformunda oluşturulan uygulamalar.

Kullanıcı Ataması gerekli olduğunda, yalnızca uygulamaya açıkça atadığınız kullanıcılar (doğrudan kullanıcı ataması veya grup üyeliğine göre) oturum açabiliyor. Uygulamalar uygulamalarım sayfasında veya doğrudan bağlantı kullanarak uygulamalara erişebilirler. 

Atama *gerekli*olmadığında, bu seçeneği **Hayır** olarak ayarlamış olmanız ya da uygulamanın başka bir SSO modu kullanması nedeniyle, uygulamanın doğrudan bir bağlantısı veya uygulamanın **Özellikler** sayfasında **Kullanıcı erişimi URL 'si** varsa, herhangi bir Kullanıcı uygulamaya erişebilecektir. 

Bu ayar, uygulama erişimi panelinde bir uygulamanın görünüp başlatılmayacağını etkilemez. Uygulamaya bir kullanıcı veya grup atadıktan sonra, uygulamalar kullanıcıların uygulama erişim panellerinde görüntülenir. Arka plan için bkz. [uygulamalara erişimi yönetme](what-is-access-management.md).


Bir uygulama için Kullanıcı Ataması gerektirmek için:

1. [Azure Portal](https://portal.azure.com) bir yönetici hesabıyla veya uygulamanın sahibi olarak oturum açın.

2. **Azure Active Directory**seçin. Sol gezinti menüsünde **Kurumsal uygulamalar**' ı seçin.

3. Listeden uygulamayı seçin. Uygulamayı görmüyorsanız, arama kutusuna adını yazmaya başlayın. Ya da uygulama türünü, durumunu veya görünürlüğünü seçmek için filtre denetimlerini kullanın ve ardından **Uygula**' yı seçin.

4. Sol gezinti menüsünde **Özellikler**' i seçin.

5. **Kullanıcı atamasının gerekli** olduğundan emin olun. iki durumlu düğme **Evet**olarak ayarlanmıştır.

   > [!NOTE]
   > **Kullanıcı Ataması gerekli değil mi?** geçiş kullanılamıyor, hizmet sorumlusu üzerinde Approleatamamentrequired özelliğini ayarlamak için PowerShell kullanabilirsiniz.

6. Ekranın üst kısmındaki **Kaydet** düğmesini seçin.

## <a name="assign-users-or-groups-to-an-app-via-the-azure-portal"></a>Azure portal aracılığıyla bir uygulamaya Kullanıcı veya Grup atama

1. [Azure Portal](https://portal.azure.com) bir genel yönetici, uygulama Yöneticisi veya bulut uygulaması yönetici hesabıyla ya da kurumsal uygulamanın atanmış sahibi olarak oturum açın.
2. **Azure Active Directory**seçin. Sol gezinti menüsünde **Kurumsal uygulamalar**' ı seçin.
3. Listeden uygulamayı seçin. Uygulamayı görmüyorsanız, arama kutusuna adını yazmaya başlayın. Ya da uygulama türünü, durumunu veya görünürlüğünü seçmek için filtre denetimlerini kullanın ve ardından **Uygula**' yı seçin.
4. Sol gezinti menüsünde **Kullanıcılar ve gruplar**' ı seçin.
   > [!NOTE]
   > Kullanıcıları Office 365 uygulamaları gibi Microsoft uygulamalarına atamak istiyorsanız, bu uygulamalardan bazıları PowerShell kullanır. 
5. **Kullanıcı Ekle** düğmesini seçin.
6. **Atama Ekle** bölmesinde **Kullanıcılar ve gruplar**' ı seçin.
7. Uygulamaya atamak istediğiniz kullanıcı veya grubu seçin veya arama kutusuna kullanıcı veya grup adını yazmaya başlayın. Birden çok kullanıcı ve grup seçebilirsiniz ve seçimleriniz **Seçili öğeler**altında görünür.
8. İşiniz bittiğinde **Seç**' e tıklayın.

   ![Uygulamaya Kullanıcı veya Grup atama](./media/assign-user-or-group-access-portal/assign-users.png)

9. **Kullanıcılar ve gruplar** bölmesinde, listeden bir veya daha fazla Kullanıcı veya grup seçin ve ardından bölmenin altındaki **Seç** düğmesini seçin.
10. Uygulama destekliyorsa, kullanıcıya veya gruba bir rol atayabilirsiniz. **Atama Ekle** bölmesinde **Rol Seç**' i seçin. Ardından, **Rol Seç** bölmesinde, seçili kullanıcılara veya gruplara uygulanacak bir rol seçin ve ardından bölmenin altındaki **Tamam** ' ı seçin. 

    > [!NOTE]
    > Uygulama, rol seçimini desteklemiyorsa, varsayılan erişim rolü atanır. Bu durumda, uygulama kullanıcıların sahip olduğu erişim düzeyini yönetir.

2. **Atama Ekle** bölmesinde, bölmenin altındaki **ata** düğmesini seçin.

## <a name="assign-users-or-groups-to-an-app-via-powershell"></a>PowerShell aracılığıyla bir uygulamaya Kullanıcı veya Grup atama

1. Yükseltilmiş bir Windows PowerShell komut istemi açın.

   > [!NOTE]
   > AzureAD modülünü yüklemeniz gerekir (komutunu kullanın `Install-Module -Name AzureAD` ). NuGet modülünü veya yeni Azure Active Directory v2 PowerShell modülünü yüklemek isteyip istemediğiniz sorulursa, Y yazın ve ENTER tuşuna basın.

1. `Connect-AzureAD`' İ çalıştırın ve bir genel yönetici kullanıcı hesabıyla oturum açın.
1. Bir uygulamaya Kullanıcı ve rol atamak için aşağıdaki betiği kullanın:

    ```powershell
    # Assign the values to the variables
    $username = "<You user's UPN>"
    $app_name = "<Your App's display name>"
    $app_role_name = "<App role display name>"

    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }

    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```

Bir kullanıcıyı bir uygulama rolüne atama hakkında daha fazla bilgi için, [New-Azureaduserapprotaatama](https://docs.microsoft.com/powershell/module/azuread/new-azureaduserapproleassignment?view=azureadps-2.0)belgelerine bakın.

Bir kurumsal uygulamaya bir grup atamak için `Get-AzureADUser` ile değiştirmeniz ve ile değiştirmeniz gerekir `Get-AzureADGroup` `New-AzureADUserAppRoleAssignment` `New-AzureADGroupAppRoleAssignment` .

Bir uygulama rolüne Grup atama hakkında daha fazla bilgi için, [New-AzureADGroupAppRoleAssignment](https://docs.microsoft.com/powershell/module/azuread/new-azureadgroupapproleassignment?view=azureadps-2.0)belgelerine bakın.

### <a name="example"></a>Örnek

Bu örnek, PowerShell kullanarak [Microsoft çalışma alanı analizi](https://products.office.com/business/workplace-analytics) uygulamasına Britta Simon kullanıcısını atar.

1. PowerShell 'de, karşılık gelen değerleri $username değişkenine atayın $app _name ve $app _role_name.

    ```powershell
    # Assign the values to the variables
    $username = "britta.simon@contoso.com"
    $app_name = "Workplace Analytics"
    ```

1. Bu örnekte, Britta Simon 'a atamak istediğimiz uygulama rolünün tam adı ne olduğunu bilmedik. Kullanıcı UPN 'sini ve hizmet sorumlusu görünen adlarını kullanarak kullanıcıyı ($user) ve hizmet sorumlusunu ($sp) almak için aşağıdaki komutları çalıştırın.

    ```powershell
    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    ```

1. `$sp.AppRoles`Çalışma alanı analizi uygulaması için kullanılabilen rolleri göstermek için komutunu çalıştırın. Bu örnekte, çözümleyici (sınırlı erişim) rolüne Britta Simı atamak istiyoruz.

   ![Çalışma alanı analizi rolü kullanan bir kullanıcının kullanabileceği rolleri gösterir](./media/assign-user-or-group-access-portal/workplace-analytics-role.png)

1. Rol adını `$app_role_name` değişkenine atayın.

    ```powershell
    # Assign the values to the variables
    $app_role_name = "Analyst (Limited access)"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }
    ```

1. Kullanıcıyı uygulama rolüne atamak için aşağıdaki komutu çalıştırın:

    ```powershell
    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```

## <a name="related-articles"></a>İlgili makaleler:

- [Uygulamalara son kullanıcı erişimi hakkında daha fazla bilgi edinin](end-user-experiences.md)
- [Azure AD erişim paneli dağıtımını planlayın](access-panel-deployment-plan.md)
- [Uygulamalara erişimi yönetme](what-is-access-management.md)
 
## <a name="next-steps"></a>Sonraki adımlar

- [Tüm gruplarımı gör](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Bir kurumsal uygulamadan Kullanıcı veya grup atamasını kaldırma](remove-user-or-group-access-portal.md)
- [Kurumsal uygulama için Kullanıcı oturum açma işlemlerini devre dışı bırakma](disable-user-sign-in-portal.md)
- [Kurumsal uygulamanın adını veya logosunu değiştirme](change-name-or-logo-portal.md)
