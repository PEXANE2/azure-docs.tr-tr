---
title: Power BI kaydetmek ve taramak için PowerShell 'i kullanma (Önizleme)
description: Azure purview 'da Power BI kiracıyı kaydetmek ve taramak için PowerShell 'in nasıl kullanılacağını öğrenin.
author: darrenparker
ms.author: hophan
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 10/12/2020
ms.openlocfilehash: f0b541baf49307006c18f07d92bd409763a29e3a
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96553503"
---
# <a name="use-powershell-to-register-and-scan-power-bi-in-azure-purview-preview"></a>Azure purview 'da Power BI kaydetmek ve taramak için PowerShell 'i kullanma (Önizleme) 

Bu makalede, Azure purview kataloğunda Power BI kiracının taramasını ayarlamak için PowerShell 'in nasıl kullanılacağı gösterilmektedir.

## <a name="power-bi-authentication-background"></a>Power BI kimlik doğrulaması arka planı

Purview kataloğu, bir Power BI kiracısındaki yapıtları taramak için Power BI yönetim API 'sine bağlanmalıdır. Power BI yönetici API 'SI Şu anda iki tür kimlik doğrulamasını desteklemektedir:

- Yönetilen kimlik (MSI).
- Temsilci Kullanıcı kimlik doğrulaması.

> [!Note]
> Temsilci Kullanıcı kimlik doğrulaması gerekmiyorsa MSI önerilir.

## <a name="create-a-security-group"></a>Güvenlik grubu oluşturma

Her purview kataloğunda, taramayı etkinleştirmek için Power BI kiracıya erişim verilmesi gereken kendi sistem tarafından atanan yönetilen kimliği vardır. Katalog adı, Azure portal kimliğini bulmak için kullanılabilir.

1. [Azure Portal](https://portal.azure.com), Azure Active Directory için arama yapın.
1. Azure Active Directory yeni bir güvenlik grubu oluşturun, aşağıdaki [temel bir grup oluşturun ve Azure Active Directory kullanarak Üyeler ekleyin](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal).

    > [!Tip]
    > Kullanılacak bir güvenlik grubunuz varsa, bu adımı atlayabilirsiniz.

1. **Grup türü** olarak güvenlik ' i seçtiğinizden emin olun.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/security-group.png" alt-text="Güvenlik grubu türü":::

1. Kataloglarınızın yönetilen kimliğini bu güvenlik grubuna ekleyerek Üyeler ' i ve ardından **Üyeler Ekle**' yi seçin.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/add-group-member.png" alt-text="Kataloğun yönetilen örneğini gruba ekle":::

1. Kataloğunuza yönelik arama yapın ve seçin.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/add-catalog-to-group-by-search.png" alt-text="Arayarak katalog ekleme":::

1. Eklendiğini gösteren bir başarı bildirimi görmeniz gerekir.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/success-add-catalog-msi.png" alt-text="Katalog MSI başarısı ekleme":::

## <a name="associate-the-security-group-with-power-bi"></a>Güvenlik grubunu Power BI ile ilişkilendir

1. [Power BI yönetici portalında](https://app.powerbi.com/admin-portal/tenantSettings?allowServicePrincipalsUseReadAdminAPIsUI=1)oturum açın. Bu özellik bayrağını ekleyin:  `allowServicePrincipalsUseReadAdminAPIsUI=1` . Bu bayrak güvenlik grubunuzu ilişkilendirebilmenizi sağlayan özelliği sağlar. Örneğin,

    ```http
    https://app.powerbi.com/admin-portal/tenantSettings?allowServicePrincipalsUseReadAdminAPIsUI=1`
    ```

    > [!Important]
    > Kiracı ayarları sayfasını görmek için bir Power BI yöneticisi olmanız gerekir.

1. **Geliştirici ayarlarını**,  >  **hizmet sorumluların salt okunurdur Power BI API 'leri (Önizleme) kullanmasına izin ver**' i seçin.
1. **Belirli güvenlik gruplarını** seçin.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/allow-service-principals-power-bi-admin.png" alt-text="Hizmet sorumluların salt okunurdur Power BI yönetici API izinleri alma ile nasıl izin verileceğini gösteren resim ":::

    > [!Caution]
    > Oluşturduğunuz güvenlik grubuna (bir üye olarak veri kataloğu yönetilen Kimliğiniz) izin vermek için, salt okunurdur Power BI yönetici API 'Lerini kullanmak için, bu Kiracıdaki tüm Power BI yapıtlarınız için meta verilere (ör. Pano ve rapor adları, sahipler, açıklamalar vb.) erişmesine izin verin. Meta veriler Azure purview 'a çekildikten sonra, Power BI izinleri değil, bu meta verileri kimlerin görebileceğini saptayın.

    > [!Note]
    > Güvenlik grubunu geliştirici ayarlarından kaldırabilirsiniz, ancak daha önce ayıklanan meta veriler, purview hesabından kaldırılmaz. Ayrıca, bunu ayrı olarak silebilirsiniz.

## <a name="register-power-bi-and-set-up-a-scan"></a>Power BI kaydetme ve tarama ayarlama

Artık Power BI kiracınızın yönetici API 'sine bağlanmak için katalog izinleri vermiş olduğunuza göre, bu durumda, değişikliklerinizi katalogda ayarlamanız gerekir. Bunu yapmak için bir PowerShell betiğini yapılandırıp çalıştırırsınız.

1. ADC PowerShell cmdlet 'lerini indirip ayıklayın.
1. Betiğin en üstündeki atamaların değerlerini sağlayarak betiğinizi yapılandırın.

    ```PowerShell
    #Purview Account Info
    $azuretenantId = '<Tenant Id>'
    $azuresubscriptionId = '<Tenant Id>'
    $azureResourceGroupName = '<Resource Group Name>'
    $azurePurviewAccountName = '<Catalog Name>'
    $createCatalog = $false #Change to true if you need a new catalog to be created
    $azureCatalogLocation = 'East Us' #The region of your account
    $dataSourceName = 'pbi-msi-datasource01' #provide a unique data source name
    $dataScanName = 'scan01' #provide a unique scan name

    #PowerShell Command Module Path
    $ModulePath = '<Full path to where you extracted the PS zip files>\Microsoft.DataCatalog.Management.Commands.dll'
    Import-Module $ModulePath

    If ($createCatalog -eq $true)
    {
      Connect-AzAccount -Tenant $azuretenantId -SubscriptionId $azuresubscriptionId
      Set-AzDataCatalog -ResourceGroupName $azureResourceGroupName -Name $azurePurviewAccountName -Location $azureCatalogLocation
    }

    Set-AzDataCatalogSessionSettings -DataCatalogSession -UserAuthentication -TenantId $azuretenantId  -DataCatalogAccountName $azurePurviewAccountName

    Set-AzDataCatalogDataSource -Name $dataSourceName -AccountType PowerBI  -Tenant $azuretenantId

    Set-AzDataCatalogScan -DataSourceName $dataSourceName -ScanName $dataScanName -PowerBIManagedInstanceMsi
    ```

    > [!Note]
    > İçinde komutları çalıştırdığınız abonelikte bir katkıda bulunan veya sahip olmanız gerekir.

1. Aşağıdaki betiği çalıştırarak taramanızı başlatın:

    ```PowerShell
    Start-AzDataCatalogScan -DataSourceName $dataSourceName -Name $dataScanName -AsJob
    ```

## <a name="register-and-scan-power-bi"></a>Kaydet ve Tara Power BI

Önerilen kimlik doğrulama yöntemi MSI 'dir. Ancak, kataloğunuzun farklı bir Azure kiracısında bulunan bir Power BI kiracıyı taramak için, Temsilcili kimlik doğrulaması kullanırsınız.

Temsilci olarak kimlik doğrulaması yapmak için yönetici kullanıcı kimlik bilgilerinin yanı sıra yönetici kimlik bilgilerini Power BI olmanız gerekir. Ayrıca, bir Azure uygulaması oluşturmanız ve buna kiracı Power BI vermeniz gerekir. ReadAll izinleri.

1. [Azure Portal](https://portal.azure.com) gidin ve **uygulama kayıtlarını** arayın.

1. **Uygulama kayıtları**, **+ Yeni kayıt**' ı seçin.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/new-app-registration.png" alt-text="Yeni bir Azure uygulaması kaydının nasıl oluşturulacağını gösteren resim":::

1. Uygulamanız için bir ad girin.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/register-new-app.png" alt-text="Yeni uygulama Kaydet":::

1. Uygulamanız oluşturulduktan sonra, **API izinleri**' ni seçin ve ardından **izin Ekle**' ye tıklayın.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/app-permissions.png" alt-text="Uygulamaya nasıl izin ekleneceğini gösteren resim":::

1. **API Izinleri isteğiyle** **Power BI hizmeti** seçin.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/select-power-bi-service.png" alt-text="PBı hizmetinin nasıl seçileceğini gösteren resim":::

1. **Temsilci izinleri** ve **kiracı. Read. All** öğesini seçin. Ardından **Izin Ekle**' yi seçin.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/request-api-permissions.png" alt-text="API izinlerinin nasıl isteneceğini gösteren resim":::

1. **Yönetici Izni ver** ' i seçin

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/grant-admin-perms.png" alt-text="Yönetici onayı verme şeklini gösteren resim":::

1. **Uygulama (istemci) kimliğini** ve **Dizin (kiracı) kimliği** değerlerini kopyalayın.  Bu değerleri, taramanızı ayarlarken kullanacaksınız.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/copy-client-and-tenantid.png" alt-text="İstemci ve kiracı kimliklerinin kopyalanmasını gösteren görüntü":::

1. Taramayı PowerShell 'de yapılandırın. Betik kimlik bilgilerini ister. Kullanmayı düşündüğünüz Azure aboneliğinde en az katkıda bulunan rolüne ve takip ettiğiniz veri kaynağı Yöneticisi rolüne sahip olmanız gerekir.

    ```PowerShell
    # Purview Account Info
    $azuretenantId = '<Tenant Id>'
    $azuresubscriptionId = '<Tenant Id>'
    $azureResourceGroupName = '<Resource Group Name>'
    $azurePurviewAccountName = '<Catalog Name>'
    $createCatalog = $false
    $azureCatalogLocation = 'East Us' #The region of your account
    $dataSourceName = 'pbi-delegated-datasource01' #provide a unique data source name
    $dataScanName = 'scan01' #provide a unique scan name

    # Power BI Tenant Info
    $powerBITenantIdToScan = '<Power BI Tenant ID copied from above steps>'
    $ServicePrincipalId = '<Client ID copied from above steps>'
    $UserName = '<Power BI Admin emil ex: admin@firsttomarket.onmicrosoft.com>'
    $Password = '<Power BI Admin Password>'

    #PowerShell Command Module Path
    $ModulePath = '<Full path to where you extracted the PS zip files>\Microsoft.DataCatalog.Management.Commands.dll'
    Import-Module $ModulePath

    #Commands To Create catalog, Create DataSource, Create Datascan, Start Scan
    If($createCatalog -eq $true)
    {
      Connect-AzAccount -Tenant $azuretenantId -SubscriptionId $azuresubscriptionId
      Set-AzDataCatalog -ResourceGroupName $azureResourceGroupName -Name $azurePurviewAccountName -Location $azureCatalogLocation
    }

    Set-AzDataCatalogSessionSettings -DataCatalogSession -UserAuthentication -TenantId $azuretenantId   -DataCatalogAccountName $azurePurviewAccountName

    Set-AzDataCatalogDataSource -Name $dataSourceName -AccountType PowerBI -Tenant $powerBITenantIdToScan

    Set-AzDataCatalogScan -DataSourceName $dataSourceName -ScanName $dataScanName -PowerBIDelegated -ServicePrincipalId $ServicePrincipalId -UserName $UserName -Password $Password
    ```

1. Taramanızı çalıştırın.

      ```PowerShell
      Start-AzDataCatalogScan -DataSourceName $dataSourceName -Name $dataScanName -AsJob
      ```

## <a name="next-steps"></a>Sonraki adımlar

Azure purview kullanmaya başlamak için bkz. [hızlı başlangıç: Azure purview hesabı oluşturma](create-catalog-portal.md).
