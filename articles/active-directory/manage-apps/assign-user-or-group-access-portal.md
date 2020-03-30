---
title: Azure AD'deki bir kurumsal uygulamaya kullanıcı veya grup atama
description: Azure Etkin Dizini'nde bir kullanıcı veya grubu atamak için kurumsal bir uygulamayı seçme
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 02/21/2020
ms.author: mimart
ms.reviewer: luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 186e36e4625a60362c54972b16b53f0f3e6753fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79409201"
---
# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory"></a>Azure Etkin Dizini'ndeki bir kurumsal uygulamaya kullanıcı veya grup atama

Bu makalede, Azure Portalı'ndan veya PowerShell'i kullanarak Azure Active Directory'deki (Azure AD) kurumsal uygulamalara kullanıcıları veya grupları nasıl atayabileceğiniz gösterilmektedir. Bir uygulamaya bir kullanıcı atadığınızda, uygulama kolay erişim için kullanıcının [Uygulamalarım erişim panelinde](https://myapps.microsoft.com/) görünür. Uygulama rolleri ortaya çıkarırsa, kullanıcıya belirli bir rol de atayabilirsiniz.

Daha fazla denetim için, belirli türdeki kurumsal uygulamalar [kullanıcı ataması gerektirecek](#configure-an-application-to-require-user-assignment)şekilde yapılandırılabilir. 

[Bir kullanıcıyı veya grubu bir kuruluş uygulamasına atamak](#assign-users-or-groups-to-an-app-via-the-azure-portal)için, genel yönetici, uygulama yöneticisi, bulut uygulama yöneticisi veya kuruluş uygulamasının atanmış sahibi olarak oturum açmanız gerekir.

> [!NOTE]
> Grup tabanlı atama, Azure Active Directory Premium P1 veya P2 sürümü gerektirir. Grup tabanlı atama yalnızca Güvenlik grupları için desteklenir. İç içe grup üyelikleri ve Office 365 grupları şu anda desteklenmiyor. Bu makalede tartışılan özellikler için daha fazla lisans [gereksinimi için Azure Etkin Dizin fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/active-directory)bakın. 

## <a name="configure-an-application-to-require-user-assignment"></a>Kullanıcı ataması gerektirecek şekilde bir uygulama yapılandırma

Aşağıdaki uygulama türlerinde, kullanıcıların uygulamaya erişebilmeleri için kullanıcıya atanmasını zorunlu kılabilir:

- SAML tabanlı kimlik doğrulama ile federe tek oturum açma (SSO) için yapılandırılan uygulamalar
- Azure Active Directory Pre-Authentication kullanan Uygulama Proxy uygulamaları
- Bir kullanıcı veya yönetici bu uygulamayı kabul ettikten sonra OAuth 2.0 / OpenID Connect Authentication kullanan Azure AD uygulama platformunda oluşturulmuş uygulamalar.

Kullanıcı ataması gerektiğinde, yalnızca uygulamaya açıkça atadığınız kullanıcılar oturum açabılabilir. Uygulamanın My Apps sayfasından veya doğrudan bir bağlantı kullanarak uygulamaya erişebilirler. 

Atama *gerekmediğinde*(bu seçeneği **Hayır** olarak ayarladığınızdan veya uygulama başka bir SSO modu kullandığından) herhangi bir kullanıcı, uygulamanın **Özellikleri** sayfasında uygulamaya veya Kullanıcı **Erişimi URL'sine** doğrudan bir bağlantısı varsa uygulamaya erişebilir. 

Bu ayar, Uygulamalarım erişim panelinde bir uygulamanın görünüp görünmediğini etkilemez. Uygulamalar, bir kullanıcıyı veya grubu uygulamaya atadıktan sonra kullanıcıların Uygulamalarım erişim panellerinde görünür. Arka plan için bkz: [Uygulamalara erişimi yönetme.](what-is-access-management.md)


Bir uygulama için kullanıcı ataması gerektirmek için:

1. [Azure portalında](https://portal.azure.com) yönetici hesabıyla veya uygulamanın sahibi olarak oturum açın.

2. **Azure Etkin Dizini'ni**seçin. Soldaki gezinti menüsünde **Kurumsal uygulamaları**seçin.

3. Listeden uygulamayı seçin. Uygulamayı görmüyorsanız, adını arama kutusuna yazmaya başlayın. Veya uygulama türünü, durumunu veya görünürlüğünü seçmek için filtre denetimlerini kullanın ve sonra **Uygula'yı**seçin.

4. Soldaki gezinti menüsünde **Özellikler'i**seçin.

5. **Kullanıcı atamasının gerekli** olduğundan emin olun? **Yes**

   > [!NOTE]
   > Kullanıcı **ataması gerekliyse?** geçiş kullanılamıyorsa, powershell'i kullanarak hizmet ilkesindeki RoleAssignmentRequired özelliğini ayarlayabilirsiniz.

6. Ekranın üst kısmındaki **Kaydet** düğmesini seçin.

## <a name="assign-users-or-groups-to-an-app-via-the-azure-portal"></a>Azure portalı üzerinden bir uygulamaya kullanıcı veya grup atama

1. [Azure portalında](https://portal.azure.com) global bir yönetici, uygulama yöneticisi veya bulut uygulama yöneticisi hesabıyla veya kurumsal uygulamanın atanmış sahibi olarak oturum açın.
2. **Azure Etkin Dizini'ni**seçin. Soldaki gezinti menüsünde **Kurumsal uygulamaları**seçin.
3. Listeden uygulamayı seçin. Uygulamayı görmüyorsanız, adını arama kutusuna yazmaya başlayın. Veya uygulama türünü, durumunu veya görünürlüğünü seçmek için filtre denetimlerini kullanın ve sonra **Uygula'yı**seçin.
4. Soldaki gezinti **menüsünde, Kullanıcıları ve grupları**seçin.
   > [!NOTE]
   > Kullanıcıları Office 365 uygulamaları gibi Microsoft Uygulamalarına atamak istiyorsanız, bu uygulamalardan bazıları PowerShell'i kullanır. 
5. Kullanıcı **Ekle** düğmesini seçin.
6. Atama **Ekle** bölmesine, **Kullanıcılar ve gruplar**seçin.
7. Uygulamaya atamak istediğiniz kullanıcıyı veya grubu seçin veya arama kutusuna kullanıcının veya grubun adını yazmaya başlayın. Birden çok kullanıcı ve grup seçebilirsiniz ve seçimleriniz **Seçili öğelerin**altında görünür.
8. Bittiğinde, **Seç'i**tıklatın.

   ![Uygulamaya bir kullanıcı veya grup atama](./media/assign-user-or-group-access-portal/assign-users.png)

9. Kullanıcılar **ve gruplar** bölmelerinde, listeden bir veya daha fazla kullanıcı veya grup seçin ve ardından bölmenin altındaki **Seç** düğmesini seçin.
10. Uygulama destekliyorsa, kullanıcıya veya gruba bir rol atayabilirsiniz. Atama **Ekle** bölmesine, **Rolü Seç'i**seçin. Ardından, **Rolü Seç** bölmesinde, seçili kullanıcılara veya gruplara uygulanacak bir rol seçin ve ardından bölmenin altındaki **Tamam'ı** seçin. 

    > [!NOTE]
    > Uygulama rol seçimini desteklemiyorsa, varsayılan erişim rolü atanır. Bu durumda, uygulama kullanıcıların sahip olduğu erişim düzeyini yönetir.

2. Atama **Ekle** bölmesine, bölmenin altındaki **Atama** düğmesini seçin.

## <a name="assign-users-or-groups-to-an-app-via-powershell"></a>PowerShell aracılığıyla bir uygulamaya kullanıcı veya grup atama

1. Yükseltilmiş bir Windows PowerShell komut istemini açın.

   > [!NOTE]
   > AzureAD modüllerini yüklemeniz gerekir (komutu `Install-Module -Name AzureAD`kullanın). Bir NuGet modülü veya yeni Azure Active Directory V2 PowerShell modülü yüklemeniz istenirse, Y yazın ve ENTER tuşuna basın.

1. Global `Connect-AzureAD` Admin kullanıcı hesabıyla çalıştırın ve oturum açın.
1. Bir uygulamaya bir kullanıcı ve rol atamak için aşağıdaki komut dosyasını kullanın:

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

Bir kullanıcıyı bir uygulama rolüne nasıl atayacağım hakkında daha fazla bilgi için [Yeni AzureADUserAppRoleAssignment](https://docs.microsoft.com/powershell/module/azuread/new-azureaduserapproleassignment?view=azureadps-2.0) belgelerini ziyaret edin

Bir grubu kurumsal bir uygulamaya atamak için `Get-AzureADUser` `Get-AzureADGroup`, ' ile değiştirmeniz gerekir.

### <a name="example"></a>Örnek

Bu örnek, kullanıcı Britta Simon'ı PowerShell kullanarak [Microsoft İşyeri Analizi](https://products.office.com/business/workplace-analytics) uygulamasına atar.

1. PowerShell'de, karşılık gelen değerleri $username, $app_name ve $app_role_name değişkenlerine atayın.

    ```powershell
    # Assign the values to the variables
    $username = "britta.simon@contoso.com"
    $app_name = "Workplace Analytics"
    ```

1. Bu örnekte, Britta Simon'a atamak istediğimiz uygulama rolünün tam adını bilmiyoruz. Kullanıcı UPN ve hizmet ana ekran adlarını kullanarak kullanıcıyı ($user) ve hizmet sorumlusunun ($sp) almak için aşağıdaki komutları çalıştırın.

    ```powershell
    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    ```

1. İşyeri Analizi `$sp.AppRoles` uygulaması için kullanılabilir rolleri görüntülemek için komutu çalıştırın. Bu örnekte, Britta Simon Analist (Sınırlı erişim) Rolü atamak istiyorum.

   ![İşyeri Analizi Rolünü kullanarak kullanıcının kullanabileceği rolleri gösterir](./media/assign-user-or-group-access-portal/workplace-analytics-role.png)

1. Rol adını değişkene `$app_role_name` atayın.

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
- [Azure AD erişim paneli dağıtımı planlama](access-panel-deployment-plan.md)
- [Uygulamalara erişimi yönetme](what-is-access-management.md)
 
## <a name="next-steps"></a>Sonraki adımlar

- [Tüm gruplarımı görün](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Bir kurumsal uygulamadan kullanıcı veya grup ataması kaldırma](remove-user-or-group-access-portal.md)
- [Kurumsal bir uygulama için kullanıcı oturum açma larını devre dışı](disable-user-sign-in-portal.md)
- [Kurumsal bir uygulamanın adını veya logosunu değiştirme](change-name-or-logo-portal.md)
