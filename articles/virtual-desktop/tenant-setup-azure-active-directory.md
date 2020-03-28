---
title: Windows Sanal Masaüstünde kiracı oluşturma - Azure
description: Azure Active Directory'de Windows Sanal Masaüstü kiracılarının nasıl ayarlanır olduğunu açıklar.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 12/17/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 9f9f7f709d31967e892900ccb25657a5963c0379
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79370228"
---
# <a name="tutorial-create-a-tenant-in-windows-virtual-desktop"></a>Öğretici: Windows Sanal Masaüstünde kiracı oluşturma

Windows Sanal Masaüstü'nde kiracı oluşturmak, masaüstü sanallaştırma çözümünüzü oluşturmanın ilk adımıdır. Kiracı, bir veya daha fazla ana bilgisayar havuzundan oluşan bir gruptur. Her ana bilgisayar havuzu, Azure'da sanal makine olarak çalışan ve Windows Sanal Masaüstü hizmetine kayıtlı birden çok oturum ana bilgisayardan oluşur. Her ana bilgisayar havuzu, kullanıcılara uzak masaüstü ve uzak uygulama kaynaklarını yayımlamak için kullanılan bir veya daha fazla uygulama grubundan da oluşur. Kiracı ile ana bilgisayar havuzları oluşturabilir, uygulama grupları oluşturabilir, kullanıcıları atayabilir ve hizmet aracılığıyla bağlantı kurabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure Active Directory izinlerini Windows Sanal Masaüstü hizmetine ver.
> * Azure Etkin Dizin kiracınızdaki bir kullanıcıya TenantCreator uygulama rolünü atayın.
> * Windows Sanal Masaüstü kiracıoluşturun.

## <a name="what-you-need-to-set-up-a-tenant"></a>Kiracı kurmak için gerekenler

Windows Sanal Masaüstü kiracınızı ayarlamaya başlamadan önce şunları yaptığınızdan emin olun:

* Windows Sanal Masaüstü kullanıcıları için [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) kiracı kimliği.
* Azure Etkin Dizin kiracıiçinde genel bir yönetici hesabı.
   * Bu, müşterileri için Windows Sanal Masaüstü kiracısı oluşturan Bulut Çözüm Sağlayıcısı (CSP) kuruluşları için de geçerlidir. Bir CSP kuruluşundaysanız, müşterinin Azure Etkin Dizin örneğinin genel yöneticisi olarak oturum açabilmelisiniz.
   * Yönetici hesabı, Windows Sanal Masaüstü kiracısını oluşturmaya çalıştığınız Azure Etkin Dizin kiracısından kaynaklanmalıdır. Bu işlem Azure Etkin Dizin B2B (konuk) hesaplarını desteklemez.
   * Yönetici hesabı bir çalışma veya okul hesabı olmalıdır.
* Azure aboneliği.

Bu öğreticide açıklanan işlemin düzgün çalışabilmesi için kiracı kimliğini, genel yönetici hesabını ve Azure aboneliğini hazır bulundurmalısınız.

## <a name="grant-permissions-to-windows-virtual-desktop"></a>Windows Sanal Masaüstü'ne izin verme

Bu Azure Etkin Dizin örneği için Windows Sanal Masaüstüne izin verdiyseniz, bu bölümü atlayın.

Windows Sanal Masaüstü hizmetine izin vermek, yönetim ve son kullanıcı görevleri için Azure Etkin Dizini sorgulamasına olanak tanır.

Hizmet izinlerini vermek için:

1. Bir tarayıcı açın ve [Windows Sanal Masaüstü sunucu uygulamasına](https://login.microsoftonline.com/common/adminconsent?client_id=5a0aa725-4958-4b0c-80a9-34562e23f3b7&redirect_uri=https%3A%2F%2Frdweb.wvd.microsoft.com%2FRDWeb%2FConsentCallback)yönetici onayı akışını başlatın.
   > [!NOTE]
   > Bir müşteriyi yönetiyorsanız ve müşterinin dizini için yönetici onayı almanız gerekiyorsa, tarayıcıya aşağıdaki URL'yi girin ve {kiracı} yerine müşterinin Azure AD etki alanı adı girin. Örneğin, müşterinin kuruluşu contoso.onmicrosoft.com'nin Azure AD etki alanı adını kaydettiyse, {kiracı} yerine contoso.onmicrosoft.com.
   >```
   >https://login.microsoftonline.com/{tenant}/adminconsent?client_id=5a0aa725-4958-4b0c-80a9-34562e23f3b7&redirect_uri=https%3A%2F%2Frdweb.wvd.microsoft.com%2FRDWeb%2FConsentCallback
   >```

2. Genel yönetici hesabıyla Windows Sanal Masaüstü onay sayfasında oturum açın. Örneğin, Contoso kuruluşundaysanız, hesabınız admin@contoso.com . admin@contoso.onmicrosoft.com
3. **Kabul Et**’i seçin.
4. Azure AD'nin onay belgesini kaydedebilmeleri için bir dakika bekleyin.
5. Bir tarayıcı açın ve [Windows Sanal Masaüstü istemci uygulamasına](https://login.microsoftonline.com/common/adminconsent?client_id=fa4345a4-a730-4230-84a8-7d9651b86739&redirect_uri=https%3A%2F%2Frdweb.wvd.microsoft.com%2FRDWeb%2FConsentCallback)yönetici onayı akışını başlatın.
   >[!NOTE]
   > Bir müşteriyi yönetiyorsanız ve müşterinin dizini için yönetici onayı almanız gerekiyorsa, tarayıcıya aşağıdaki URL'yi girin ve {kiracı} yerine müşterinin Azure AD etki alanı adı girin. Örneğin, müşterinin kuruluşu contoso.onmicrosoft.com'nin Azure AD etki alanı adını kaydettiyse, {kiracı} yerine contoso.onmicrosoft.com.
   >```
   > https://login.microsoftonline.com/{tenant}/adminconsent?client_id=fa4345a4-a730-4230-84a8-7d9651b86739&redirect_uri=https%3A%2F%2Frdweb.wvd.microsoft.com%2FRDWeb%2FConsentCallback
   >```

6. Adım 2'de yaptığınız gibi, Windows Sanal Masaüstü onay sayfasında küresel yönetici olarak oturum açın.
7. **Kabul Et**’i seçin.

## <a name="assign-the-tenantcreator-application-role"></a>TenantCreator uygulama rolünü atama

Bir Azure Active Directory kullanıcısını TenantCreator uygulama rolüolarak atamak, kullanıcının Azure Etkin Dizin örneğiyle ilişkili bir Windows Sanal Masaüstü kiracısı oluşturmasına olanak tanır. TenantCreator rolünü atamak için genel yönetici hesabınızı kullanmanız gerekir.

TenantCreator uygulama rolünü atamak için:

1. TenantCreator uygulama rolünü yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Kurumsal uygulamaları**arayın ve seçin. Birden çok Azure Etkin Dizin kiracısıyla çalışıyorsanız, özel bir tarayıcı oturumu açıp URL'leri adres çubuğuna kopyalayıp yapıştırmak en iyi yöntemdir.

   ![Azure portalında Kurumsal uygulamaları arama ekran görüntüsü](media/azure-portal-enterprise-applications.png)
2. **Kurumsal uygulamalar**içinde, **Windows Sanal Masaüstü**için arama. Önceki bölümde onay sağladığınız iki uygulamayı görürsünüz. Bu iki uygulamadan, **Windows Sanal Masaüstü'nü**seçin.
   
   !["Kurumsal uygulamalarda" "Windows Sanal Masaüstü" aranırken arama sonuçlarının ekran görüntüsü. "Windows Sanal Masaüstü" adlı uygulama vurgulanır.](media/tenant-enterprise-app.png)
3. **Kullanıcıları ve grupları**seçin. Uygulamaya onay veren yöneticinin **varsayılan erişim** rolü atanmış olarak zaten listelenmiş olduğunu görebilirsiniz. Bu, Windows Sanal Masaüstü kiracıoluşturmak için yeterli değildir. Bir kullanıcıya **TenantCreator** rolünü eklemek için bu yönergeleri takip etmeye devam edin.
   
   !["Windows Sanal Masaüstü" kurumsal uygulamasını yönetmek için atanan kullanıcıların ve grupların ekran görüntüsü. Ekran görüntüsü, "Varsayılan Erişim" için yalnızca bir atama yı gösterir.](media/tenant-default-access.png)
4. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** sekmesinde Kullanıcılar ve **gruplar** seçin.
5. Windows Sanal Masaüstü kiracınızı oluşturacak bir kullanıcı hesabı arayın. Basitlik için, bu genel yönetici hesabı olabilir.
   - Windows Sanal Masaüstü'nde contosoadmin@live.com contosoadmin@outlook.comoturum açamayabilirsiniz. Bunun admin@contoso.com admin@contoso.onmicrosoft.com yerine etki alanına özgü bir hesap kullanmanızı öneririz.

   !["TenantCreator" olarak eklenecek bir kullanıcının seçilme ekran görüntüsü.](media/tenant-assign-user.png)
   > [!NOTE]
   > Bu Azure Etkin Dizin örneğinden gelen bir kullanıcıyı (veya kullanıcı içeren bir grubu) seçmeniz gerekir. Konuk (B2B) kullanıcı veya hizmet sorumlusu seçemezsiniz.

6. Kullanıcı hesabını seçin, **Seç** düğmesini seçin ve sonra **Atla'yı**seçin.
7. Windows **Sanal Masaüstü** - Kullanıcılar ve gruplar sayfasında, Windows Sanal Masaüstü kiracısını oluşturacak kullanıcıya atanan **TenantCreator** rolüyle yeni bir giriş gördüğünüzden doğrulayın.

   !["Windows Sanal Masaüstü" kurumsal uygulamasını yönetmek için atanan kullanıcıların ve grupların ekran görüntüsü. Ekran görüntüsü artık "TenantCreator" rolüne atanan bir kullanıcının ikinci girişini içerir.](media/tenant-tenant-creator-added.png)

Windows Sanal Masaüstü kiracınızı oluşturmaya devam etmeden önce iki bilgi almanız gerekir:

   - Azure Etkin Dizin kiracı kimliğiniz (veya **Dizin Kimliğiniz)**
   - Azure abonelik kimliğiniz

Azure Etkin Dizin kiracı kimliğinizi (veya **Dizin Kimliğinizi)** bulmak için:
1. Aynı [Azure portal](https://portal.azure.com) oturumunda Azure **Etkin Dizini'ni**arayın ve seçin.

   ![Azure portalındaki "Azure Etkin Dizini" arama sonuçlarının ekran görüntüsü. "Hizmetler" altında arama sonucu vurgulanır.](media/tenant-search-azure-active-directory.png)
2. **Özellikleri**bulana kadar aşağı kaydırın ve sonra seçin.
3. **Dizin Kimliği'ni**arayın ve ardından pano simgesini seçin. Daha sonra **AadTenantId** değeri olarak kullanabilirsiniz böylece kullanışlı bir konuma yapıştırın.

   ![Azure Active Directory özelliklerinin ekran görüntüsü. Fare, kopyalamak ve yapıştırmak için "Dizin Kimliği" pano simgesinin üzerinde geziniyor.](media/tenant-directory-id.png)

Azure abonelik kimliğinizi bulmak için:
1. Aynı [Azure portal](https://portal.azure.com) oturumunda **Abonelikleri**arayın ve seçin.
   
   ![Azure portalındaki "Azure Etkin Dizini" arama sonuçlarının ekran görüntüsü. "Hizmetler" altında arama sonucu vurgulanır.](media/tenant-search-subscription.png)
2. Windows Sanal Masaüstü hizmet bildirimlerini almak için kullanmak istediğiniz Azure aboneliğini seçin.
3. Abonelik **Kimliği'ni**arayın ve pano simgesi görünene kadar değerin üzerinde gezinin. Pano simgesini seçin ve daha sonra **AzureSubscriptionId** değeri olarak kullanabilmeniz için kullanışlı bir konuma yapıştırın.
   
   ![Azure abonelik özelliklerinin ekran görüntüsü. Fare, kopyalamak ve yapıştırmak için "Abonelik Kimliği" pano simgesinin üzerinde geziniyor.](media/tenant-subscription-id.png)

## <a name="create-a-windows-virtual-desktop-tenant"></a>Windows Sanal Masaüstü kiracı oluşturma

Azure Etkin Dizini sorgulamak için Windows Sanal Masaüstü hizmeti izni verdiğiniz ve TenantCreator rolünü bir kullanıcı hesabına atadığınıza göre, bir Windows Sanal Masaüstü kiracısı oluşturabilirsiniz.

İlk olarak, PowerShell oturumunuzda kullanmak üzere [Windows Sanal Masaüstü modülünü indirin ve içe aktarın.](/powershell/windows-virtual-desktop/overview/)

Bu cmdlet ile TenantCreator kullanıcı hesabını kullanarak Windows Sanal Masaüstüoturum da oturum açın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Bundan sonra, Azure Etkin Dizin kiracısıyla ilişkili yeni bir Windows Sanal Masaüstü kiracısı oluşturun:

```powershell
New-RdsTenant -Name <TenantName> -AadTenantId <DirectoryID> -AzureSubscriptionId <SubscriptionID>
```

Parantez içinde bulunan değerleri kuruluşunuz ve kiracınızla ilgili değerlerle değiştirin. Yeni Windows Sanal Masaüstü kiracınız için seçtiğiniz ad genel olarak benzersiz olmalıdır. Örneğin, Contoso kuruluşunun Windows Sanal Masaüstü KiracıSı olduğunuzu varsayalım. Çalıştırdığınız cmdlet şöyle görünür:

```powershell
New-RdsTenant -Name Contoso -AadTenantId 00000000-1111-2222-3333-444444444444 -AzureSubscriptionId 55555555-6666-7777-8888-999999999999
```

Hesabınızın dışında kalmanız veya tatile çıkıp yokluğunuzda kiracı yönetici olarak hareket edecek birine ihtiyaç duymanız durumunda ikinci bir kullanıcıya yönetim erişimi atamak iyi bir fikirdir. İkinci bir kullanıcıya yönetici erişimi atamak için, `<TenantName>` aşağıdaki `<Upn>` cmdlet'i çalıştırın ve kiracı adınız ve ikinci kullanıcının UPN'i ile değiştirin.

```powershell
New-RdsRoleAssignment -TenantName <TenantName> -SignInName <Upn> -RoleDefinitionName "RDS Owner"
```

## <a name="next-steps"></a>Sonraki adımlar

Kiracınızı oluşturduktan sonra, Azure Etkin Dizini'nde bir hizmet yöneticisi oluşturmanız ve windows sanal masaüstünde bir rol atamanız gerekir. Hizmet sorumlusu, ana bilgisayar havuzu oluşturmak için Windows Sanal Masaüstü Azure Marketi teklifini başarıyla dağıtmanıza olanak tanır. Ana bilgisayar havuzları hakkında daha fazla bilgi edinmek için, Windows Sanal Masaüstü'nde ana bilgisayar havuzu oluşturma eğitimine devam edin.

> [!div class="nextstepaction"]
> [PowerShell ile hizmet sorumluları ve rol atamaları oluşturma](./create-service-principal-role-powershell.md)
