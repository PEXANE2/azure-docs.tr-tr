---
title: Azure Pipelines ile özel ilkeler dağıtma
titleSuffix: Azure AD B2C
description: Azure DevOps Hizmetlerinde Azure Ardışık Hatlar hattını kullanarak Bir CI/CD ardışık alanda Azure AD B2C özel ilkelerini nasıl dağıtabileceğinizi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: b23b60ae49a4973fa04e6fa5f795f99536e32e7f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188758"
---
# <a name="deploy-custom-policies-with-azure-pipelines"></a>Azure Pipelines ile özel ilkeler dağıtma

[Azure Pipelines'da][devops-pipelines]ayarladığınız sürekli bir tümleştirme ve teslim (CI/CD) ardışık nokta sını kullanarak, Azure AD B2C özel ilkelerinizi yazılım teslimi ve kod denetimi otomasyonunuza ekleyebilirsiniz. Dev, test ve üretim gibi farklı Azure AD B2C ortamlarına dağıtırken, el ile işlemleri kaldırmanızı ve Azure Ardışık Hatlar hattını kullanarak otomatik sınama gerçekleştirmenizi öneririz.

Azure Ardışık Hatlarının Azure AD B2C içinde özel ilkeleri yönetmesini etkinleştirmek için gereken üç temel adım vardır:

1. Azure AD B2C kiracınızda bir web uygulama kaydı oluşturma
1. Azure Repo'su yapılandırma
1. Azure Ardışık Hatlar Yapılandırılması

> [!IMPORTANT]
> Azure AD B2C özel ilkelerini bir **preview** Azure Ardışık Alanı yla `/beta` yönetme, şu anda Microsoft Graph API bitiş noktasında bulunan önizleme işlemlerini kullanır. Bu API'lerin üretim uygulamalarında kullanımı desteklenmez. Daha fazla bilgi için [Microsoft Graph REST API beta uç nokta başvurusuna](https://docs.microsoft.com/graph/api/overview?toc=./ref/toc.json&view=graph-rest-beta)bakın.

## <a name="prerequisites"></a>Ön koşullar

* [Azure AD B2C kiracı](tutorial-create-tenant.md)ve [B2C IEF İlke Yöneticisi](../active-directory/users-groups-roles/directory-assign-admin-roles.md#b2c-ief-policy-administrator) rolü ile dizindeki bir kullanıcı için kimlik bilgileri
* Kiracınıza yüklenen [özel ilkeler](custom-policy-get-started.md)
* Microsoft Graph API izni *Policy.ReadWrite.TrustFramework* ile kiracınızda kayıtlı [yönetim uygulaması](microsoft-graph-get-started.md)
* [Azure Ardışık Katmanlar](https://azure.microsoft.com/services/devops/pipelines/)ve Bir [Azure DevOps Hizmetleri projesine][devops-create-project] erişim

## <a name="client-credentials-grant-flow"></a>İstemci kimlik bilgileri hibe akışı

Burada açıklanan senaryo, OAuth 2.0 [istemci kimlik bilgileri hibe akışını](../active-directory/develop/v1-oauth2-client-creds-grant-flow.md)kullanarak Azure Ardışık Hatları ve Azure AD B2C arasındaki hizmet-servis çağrılarından yararlanır. Bu hibe akışı, Azure Altyapı İşlemleri (gizli istemci) gibi bir web hizmetinin, başka bir web hizmetini (bu durumda Microsoft Graph API) ararken kimlik doğrulaması için bir kullanıcının kimliğine bürünmek yerine kendi kimlik bilgilerini kullanmasına izin verir. Azure Pipelines etkileşimli olmayan bir belirteç alır ve microsoft graph API'ye istekte bulunmaktadır.

## <a name="register-an-application-for-management-tasks"></a>Yönetim görevleri için bir uygulama kaydetme

[Önkoşullar'da](#prerequisites)belirtildiği gibi, Azure Pipelines tarafından yürütülen PowerShell komut dosyalarınızın kiracınızdaki kaynaklara erişmek için kullanabileceği bir uygulama kaydına ihtiyacınız vardır.

Otomasyon görevleri için kullandığınız bir uygulama kaydınız varsa, uygulama kaydının **API İzinleri** dahilinde **Microsoft Graph** > **Policy.ReadWrite.TrustFramework** izninin verildiğinden emin olun.**Policy** > 

Bir yönetim uygulamasının kaydedilmesiyle ilgili talimatlar [için](microsoft-graph-get-started.md)bkz.

## <a name="configure-an-azure-repo"></a>Azure Repo'su yapılandırma

Kayıtlı bir yönetim uygulamasıyla, ilke dosyalarınız için bir depo yapılandırmaya hazırsınız.

1. Azure DevOps Hizmetleri kuruluşunuzda oturum açın.
1. [Yeni bir proje oluşturun][devops-create-project] veya varolan bir projeyi seçin.
1. **Projenizde, Repos'a** gidin ve **Dosyalar** sayfasını seçin. Varolan bir depo seçin veya bu alıştırma için bir depo oluşturun.
1. *B2CAssets*adlı bir klasör oluşturun. Gerekli yer tutucu dosyayı *README.md* ve dosyayı **işleme.** İsterseniz bu dosyayı daha sonra kaldırabilirsiniz.
1. Azure AD B2C ilke dosyalarınızı *B2CAssets* klasörüne ekleyin. Buna *TrustFrameworkBase.xml,* *TrustFrameWorkExtensions.xml*, *SignUpOrSignin.xml*, *ProfileEdit.xml*, *PasswordReset.xml*ve oluşturduğunuz diğer ilkeler dahildir. Her Azure AD B2C ilke dosyasının dosya adını daha sonraki bir adımda kullanmak üzere kaydedin (PowerShell komut dosyası bağımsız değişkenleri olarak kullanılırlar).
1. Deponun kök dizininde *Komut Dosyaları* adlı bir klasör oluşturun, yer tutucu dosyasını *DağıtmaToB2c.ps1'i*adlandırın. Bu noktada dosyayı işlemeyin, bunu daha sonraki bir adımda yaparsınız.
1. Aşağıdaki PowerShell komut dosyasını *DeployToB2c.ps1'e*yapıştırın, ardından dosyayı **işlayın.** Komut dosyası, Azure AD'den bir belirteç edinir ve *B2CAssets* klasöründeki ilkeleri Azure AD B2C kiracınıza yüklemesi için Microsoft Graph API'yi çağırır.

    ```PowerShell
    [Cmdletbinding()]
    Param(
        [Parameter(Mandatory = $true)][string]$ClientID,
        [Parameter(Mandatory = $true)][string]$ClientSecret,
        [Parameter(Mandatory = $true)][string]$TenantId,
        [Parameter(Mandatory = $true)][string]$PolicyId,
        [Parameter(Mandatory = $true)][string]$PathToFile
    )

    try {
        $body = @{grant_type = "client_credentials"; scope = "https://graph.microsoft.com/.default"; client_id = $ClientID; client_secret = $ClientSecret }

        $response = Invoke-RestMethod -Uri https://login.microsoftonline.com/$TenantId/oauth2/v2.0/token -Method Post -Body $body
        $token = $response.access_token

        $headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
        $headers.Add("Content-Type", 'application/xml')
        $headers.Add("Authorization", 'Bearer ' + $token)

        $graphuri = 'https://graph.microsoft.com/beta/trustframework/policies/' + $PolicyId + '/$value'
        $policycontent = Get-Content $PathToFile
        $response = Invoke-RestMethod -Uri $graphuri -Method Put -Body $policycontent -Headers $headers

        Write-Host "Policy" $PolicyId "uploaded successfully."
    }
    catch {
        Write-Host "StatusCode:" $_.Exception.Response.StatusCode.value__

        $_

        $streamReader = [System.IO.StreamReader]::new($_.Exception.Response.GetResponseStream())
        $streamReader.BaseStream.Position = 0
        $streamReader.DiscardBufferedData()
        $errResp = $streamReader.ReadToEnd()
        $streamReader.Close()

        $ErrResp

        exit 1
    }

    exit 0
    ```

## <a name="configure-your-azure-pipeline"></a>Azure ardışık sisteminizi yapılandırma

Deponuz başlatılan ve özel ilke dosyalarınızla doldurulan sürüm ardışık hattını ayarlamaya hazırsınız.

### <a name="create-pipeline"></a>İşlem hattı oluşturma

1. Azure DevOps Hizmetleri kuruluşunuzla oturum açın ve projenize gidin.
1. Projenizde, **Pipelines** > **Releases** > **New pipeline'ı**seçin.
1. **Şablon seç'in**altında Boş **iş'i**seçin.
1. Bir **Sahne adı**girin , örneğin *Özel İlkeleri Dağıt,* ardından bölmeyi kapatın.
1. **Yapı ekle'yi**seçin ve **Kaynak türü**altında **Azure Deposu'nu**seçin.
    1. PowerShell komut dosyasıyla dolduran *Komut Dosyaları* klasörünü içeren kaynak deposunu seçin.
    1. Varsayılan **dal**seçin. Önceki bölümde yeni bir depo oluşturduysanız, varsayılan dal *ana*.
    1. *Varsayılan daldan En Son*Varsayılan **sürüm** ayarını bırakın.
    1. Depo için kaynak **takma adı** girin. Örneğin, *policyRepo*. Takma ada boşluk eklemeyin.
1. **Ekle**’yi seçin
1. Boru hattını amacını yansıtacak şekilde yeniden adlandırın. Örneğin, *Özel İlke Ardışık Alanı dağıtın.*
1. Boru hattı yapılandırmasını kaydetmek için **Kaydet'i** seçin.

### <a name="configure-pipeline-variables"></a>Boru hattı değişkenlerini yapılandırma

1. **Değişkenler** sekmesini seçin.
1. **Pipeline değişkenlerinin** altına aşağıdaki değişkenleri ekleyin ve değerlerini belirtildiği gibi ayarlayın:

    | Adı | Değer |
    | ---- | ----- |
    | `clientId` | Daha önce kaydettiğiniz uygulamanın **başvuru (istemci) kimliği.** |
    | `clientSecret` | Daha önce oluşturduğunuz **istemci sırrının** değeri. <br /> Değişken türünü **gizli** olarak değiştirin (kilit simgesini seçin). |
    | `tenantId` | `your-b2c-tenant.onmicrosoft.com`, Azure AD *B2C kiracınızın* adı dır. |

1. Değişkenleri kaydetmek için **Kaydet'i** seçin.

### <a name="add-pipeline-tasks"></a>Boru hattı görevleri ekleme

Ardından, bir ilke dosyası dağıtmak için bir görev ekleyin.

1. **Görevler** sekmesini seçin.
1. **Aracı işi**seçin ve sonra Aracı**+** lı işe bir görev eklemek için artı işaretini ( ) seçin.
1. **PowerShell'i**arayın ve seçin. "Azure PowerShell", "Hedef makinelerde PowerShell" veya başka bir PowerShell girişi seçmeyin.
1. Yeni eklenen **PowerShell Script** görevini seçin.
1. PowerShell Script görevi için aşağıdaki değerleri girin:
    * **Görev sürümü**: 2.*
    * **Görüntü adı**: Bu görevin yüklemesi gereken ilkenin adı. Örneğin, *B2C_1A_TrustFrameworkBase.*
    * **Tür**: Dosya Yolu
    * **Komut Dosyası Yolu**: Elipsis ***(...***), *Komut Dosyaları* klasörüne gidin ve ardından *DeployToB2C.ps1* dosyasını seçin.
    * **Bağımsız değişken:**

        **Bağımsız değişkenler**için aşağıdaki değerleri girin. Önceki `{alias-name}` bölümde belirttiğiniz takma adla değiştirin.

        ```PowerShell
        # Before
        -ClientID $(clientId) -ClientSecret $(clientSecret) -TenantId $(tenantId) -PolicyId B2C_1A_TrustFrameworkBase -PathToFile $(System.DefaultWorkingDirectory)/{alias-name}/B2CAssets/TrustFrameworkBase.xml
        ```

        Örneğin, belirttiğiniz diğer ad *policyRepo*ise, bağımsız değişken satırı olmalıdır:

        ```PowerShell
        # After
        -ClientID $(clientId) -ClientSecret $(clientSecret) -TenantId $(tenantId) -PolicyId B2C_1A_TrustFrameworkBase -PathToFile $(System.DefaultWorkingDirectory)/policyRepo/B2CAssets/TrustFrameworkBase.xml
        ```

1. Aracı işini kaydetmek için **Kaydet'i** seçin.

Az önce eklediğiniz görev Azure AD B2C'ye *bir* ilke dosyası yükler. Devam etmeden önce, ek görevler oluşturmadan önce başarılı bir şekilde tamamlandığından emin olmak için işi **(Sürüm Oluştur)** el ile tetikler.

Görev başarıyla tamamlanırsa, özel ilke dosyalarının her biri için önceki adımları gerçekleştirerek dağıtım görevleri ekleyin. Her `-PolicyId` ilke için bağımsız `-PathToFile` değişken değerlerini değiştirin.

Bu `PolicyId` değer, TrustFrameworkPolicy düğümünde bir XML ilke dosyasının başında bulunan bir değerdir. Örneğin, `PolicyId` aşağıdaki ilke XML *B2C_1A_TrustFrameworkBase:*

```XML
<TrustFrameworkPolicy
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:xsd="http://www.w3.org/2001/XMLSchema"
xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
PolicySchemaVersion="0.3.0.0"
TenantId="contoso.onmicrosoft.com"
PolicyId= "B2C_1A_TrustFrameworkBase"
PublicPolicyUri="http://contoso.onmicrosoft.com/B2C_1A_TrustFrameworkBase">
```

Aracıları çalıştırırken ve ilke dosyalarını yüklerken, bu dosyaların aşağıdaki sırayla yüklendiğinden emin olun:

1. *TrustFrameworkBase.xml*
1. *TrustFrameworkExtensions.xml*
1. *SignUpOrSignin.xml*
1. *ProfileEdit.xml*
1. *PasswordReset.xml*

Dosya yapısı hiyerarşik bir zincir üzerine inşa edildikçe Kimlik Deneyimi Çerçevesi bu düzeni zorlar.

## <a name="test-your-pipeline"></a>İşlem hattınızı test etme

Sürüm ardışık sisteminizi test etmek için:

1. **Ardışık Hatlar'ı** seçin ve ardından **Sürümler.**
1. Daha önce oluşturduğunuz ardışık alanı seçin, örneğin *Özel İlkeleri Dağıt.*
1. **Sürümü Oluştur'u**seçin, ardından sürümü sıraya almak için **Oluştur'u** seçin.

Bir sürümün sıraya alındığını belirten bir bildirim başlığı görmeniz gerekir. Durumunu görüntülemek için bildirim başlığındaki bağlantıyı seçin veya **Sürümler** sekmesindeki listede seçin.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:

* [İstemci kimlik bilgilerini kullanarak servise hizmet çağrıları](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow)
* [Azure DevOps Services](https://docs.microsoft.com/azure/devops/user-guide/?view=azure-devops)

<!-- LINKS - External -->
[devops]: https://docs.microsoft.com/azure/devops/?view=azure-devops
[devops-create-project]:  https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops
[devops-pipelines]: https://docs.microsoft.com/azure/devops/pipelines
