---
title: Azure Otomasyonu’nda kaynak denetimi tümleştirmesi
description: Bu makalede, Azure Otomasyonu'nda GitHub ile kaynak denetimi tümleştirmesi açıklanmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 12/10/2019
ms.topic: conceptual
ms.openlocfilehash: 166902978d1641458f18aeee6269c8d819e85233
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132920"
---
# <a name="source-control-integration-in-azure-automation"></a>Azure Otomasyonu’nda kaynak denetimi tümleştirmesi

 Azure Otomasyon'daki kaynak denetimi tümleştirmesi, kaynak denetim deponuzdan tek yönlü eşitleme sağlar. Kaynak denetimi, runbook'larınızı GitHub veya Azure Depoları kaynak denetim deposunuzdaki komut dosyalarıyla Otomasyon hesabınızda güncel tutmanıza olanak tanır. Bu özellik, geliştirme ortamınızda test edilmiş kodu üretim Otomasyonu hesabınıza tanıtmayı kolaylaştırır.
 
 Kaynak denetimi tümleştirmesi, ekibinizle kolayca işbirliği yapmanızı, değişiklikleri izlemenizi ve runbook'larınızın önceki sürümlerine geri dönmenizi sağlar. Örneğin, kaynak denetimi, kaynak denetimindeki farklı dalları geliştirme, test ve üretim Otomasyon hesaplarınızla eşitlemenize olanak tanır. 

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="source-control-types"></a>Kaynak kontrol türleri

Azure Otomasyonu üç tür kaynak denetimini destekler:

* GitHub
* Azure Deposu (Git)
* Azure Repos (TFVC)

## <a name="prerequisites"></a>Ön koşullar

* Kaynak denetim deposu (GitHub veya Azure Repos)
* Bir [Run As hesabı](manage-runas-account.md)
* Modül de dahil olmak üzere Otomasyon hesabınızdaki [en son Azure modülleri](automation-update-azure-modules.md) (Az modülü eşdeğeri) `AzureRM.Profile` `Az.Accounts`

> [!NOTE]
> Kaynak denetimi eşitleme işleri kullanıcının Otomasyon hesabı altında çalıştırılır ve diğer Otomasyon işleri ile aynı oranda faturalandırılır.

## <a name="configuring-source-control"></a>Kaynak denetimini yapılandırma

Bu bölümde, Otomasyon hesabınız için kaynak denetiminin nasıl yapılandırılabildiğini anlatınız. Azure portalını veya PowerShell'i kullanabilirsiniz.

### <a name="configure-source-control-in-azure-portal"></a>Azure portalında kaynak denetimini yapılandırma

Kaynak denetimini Azure portalını kullanarak yapılandırmak için bu yordamı kullanın.

1. Otomasyon hesabınızda **Kaynak Denetimi'ni** seçin ve **Ekle'yi**tıklatın.

    ![Kaynak denetimini seçin](./media/source-control-integration/select-source-control.png)

2. **Kaynak Denetimi türünü**seçin, ardından Kimlik **Doğrulaması'yı**tıklatın. 

3. Bir tarayıcı penceresi açılır ve oturum açmanızı ister. Kimlik doğrulamasını tamamlamak için istemleri izleyin.

4. Kaynak Denetimi Özeti sayfasında, aşağıda tanımlanan kaynak denetimi özelliklerini doldurmak için alanları kullanın. Bittiğinde **Kaydet'i** tıklatın. 

    |Özellik  |Açıklama  |
    |---------|---------|
    |Kaynak denetim adı     | Kaynak denetimi için dostça bir ad. Bu ad yalnızca harf ve sayı içermelidir.        |
    |Kaynak kontrol türü     | Kaynak kontrol mekanizması türü. Kullanılabilen seçenekler:</br> * GitHub</br>* Azure Deposu (Git)</br> * Azure Depoları (TFVC)        |
    |Depo     | Deponun veya projenin adı. İlk 200 depo alınır. Bir depo aramak için, alana adı yazın ve **GitHub'da Ara'yı**tıklatın.|
    |Dal     | Kaynak dosyaları çekmek için hangi şube. TFVC kaynak denetim türü için şube hedeflemesi kullanılamaz.          |
    |Klasör yolu     | Eşitlemek için runbook'ları içeren klasör, örneğin, **/Runbook'lar.** Yalnızca belirtilen klasördeki runbook'lar eşitlenir. Özyineleme desteklenmez.        |
    |Otomatik Eşitleme<sup>1</sup>     | Kaynak denetim deposunda işleme yapıldığında otomatik eşitlemenin açık veya kapalı olması.        |
    |Runbook'u Yayımla     | Kaynak denetiminden eşitleme den sonra runbook'ların otomatik olarak yayımlanıp yayınlansın ve aksi takdirde Kapalı'nın ayarı.           |
    |Açıklama     | Kaynak denetimi yle ilgili ek ayrıntıları belirten metin.        |

    <sup>1</sup> Kaynak denetimi tümleştirmesini Azure Repos ile yapılandırırken Otomatik Eşitleme'yi etkinleştirmek için bir Proje Yöneticisi olmalısınız.

   ![Kaynak denetim özeti](./media/source-control-integration/source-control-summary.png)

> [!NOTE]
> Kaynak denetim deponuzun girişi, Azure portalı için girişinizden farklı olabilir. Kaynak denetimini yapılandırırken kaynak denetim deponuzun doğru hesabıyla oturum açtığınızdan emin olun. Bir şüphe varsa, tarayıcınızda yeni bir sekme açın, **dev.azure.com,** **visualstudio.com**veya **github.com'dan**çıkış yapın ve kaynak denetimine yeniden bağlanmayı deneyin.

### <a name="configure-source-control-in-powershell"></a>PowerShell'de kaynak denetimini yapılandırma

PowerShell'i Azure Otomasyonu'nda kaynak denetimini yapılandırmak için de kullanabilirsiniz. Bu işlem için PowerShell cmdlets kullanmak için, kişisel erişim belirteci (PAT) gerekir. Kaynak kontrol bağlantısını oluşturmak için [New-AzAutomationSourceControl](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationsourcecontrol?view=azps-3.5.0
) cmdlet'i kullanın. Bu cmdlet PAT için güvenli bir dize gerektirir. Güvenli bir dize oluşturmayı öğrenmek için [ConvertTo-SecureString'e](/powershell/module/microsoft.powershell.security/convertto-securestring?view=powershell-6)bakın.

Aşağıdaki alt bölümlerde GitHub, Azure Repos (Git) ve Azure Repos (TFVC) için kaynak denetim bağlantısının PowerShell oluşturulması gösterilmiştir. 

#### <a name="create-source-control-connection-for-github"></a>GitHub için kaynak denetim bağlantısı oluşturma

```powershell-interactive
New-AzAutomationSourceControl -Name SCGitHub -RepoUrl https://github.com/<accountname>/<reponame>.git -SourceType GitHub -FolderPath "/MyRunbooks" -Branch master -AccessToken <secureStringofPAT> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

#### <a name="create-source-control-connection-for-azure-repos-git"></a>Azure Repos (Git) için kaynak denetimi bağlantısı oluşturma

> [!NOTE]
> Azure Repos (Git), önceki biçimlerde kullanılan **visualstudio.com**yerine **dev.azure.com** erişen bir URL kullanır. Eski URL `https://<accountname>.visualstudio.com/<projectname>/_git/<repositoryname>` biçimi amortismana alınır, ancak yine de desteklenir. Yeni biçim tercih edilir.


```powershell-interactive
New-AzAutomationSourceControl -Name SCReposGit -RepoUrl https://dev.azure.com/<accountname>/<adoprojectname>/_git/<repositoryname> -SourceType VsoGit -AccessToken <secureStringofPAT> -Branch master -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName> -FolderPath "/Runbooks"
```

#### <a name="create-source-control-connection-for-azure-repos-tfvc"></a>Azure Repos (TFVC) için kaynak denetim bağlantısı oluşturma

> [!NOTE]
> Azure Repos (TFVC), önceki biçimlerde kullanılan visualstudio.com yerine **dev.azure.com** erişen **bir**URL kullanır. Eski URL `https://<accountname>.visualstudio.com/<projectname>/_versionControl` biçimi amortismana alınır, ancak yine de desteklenir. Yeni biçim tercih edilir.

```powershell-interactive
New-AzAutomationSourceControl -Name SCReposTFVC -RepoUrl https://dev.azure.com/<accountname>/<adoprojectname>/_git/<repositoryname> -SourceType VsoTfvc -AccessToken <secureStringofPAT> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName> -FolderPath "/Runbooks"
```

#### <a name="personal-access-token-pat-permissions"></a>Kişisel erişim belirteci (PAT) izinleri

Kaynak denetimi, PAT'ler için bazı minimum izinler gerektirir. Aşağıdaki alt bölümler, GitHub ve Azure Repos için gereken minimum izinleri içerir.

##### <a name="minimum-pat-permissions-for-github"></a>GitHub için minimum PAT izinleri

Aşağıdaki tablo, GitHub için gereken minimum PAT izinlerini tanımlar. GitHub'da BIR PAT oluşturma hakkında daha fazla bilgi [için](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)bkz.

|Kapsam  |Açıklama  |
|---------|---------|
|**`repo`**     |         |
|`repo:status`     | İşişleme durumuna erişin         |
|`repo_deployment`      | Erişim dağıtım durumu         |
|`public_repo`     | Ortak depolara erişin         |
|**`admin:repo_hook`**     |         |
|`write:repo_hook`     | Depo kancaları yazma         |
|`read:repo_hook`|Depo kancalarını okuyun|

##### <a name="minimum-pat-permissions-for-azure-repos"></a>Azure Depoları için minimum PAT izinleri

Aşağıdaki liste, Azure Repos'ları için gereken minimum PAT izinlerini tanımlar. Azure Repos'ta BIR PAT oluşturma hakkında daha fazla bilgi için [bkz.](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate)

| Kapsam  |  Erişim Türü  |
|---------| ----------|
| `Code`      | Okuma  |
| `Project and team` | Okuma |
| `Identity` | Okuma     |
| `User profile` | Okuma     |
| `Work items` | Okuma    |
| `Service connections` | Okuma, sorgula, yönetme<sup>1</sup>    |

<sup>1</sup> `Service connections` İzin yalnızca otomatik senkronizasyonu etkinleştirdiyseniz gereklidir.

## <a name="synchronizing"></a>Eşitleme

Kaynak denetimiyle eşitlemek için aşağıdaki adımları izleyin. 

1. Kaynak denetim sayfasındaki tablodan kaynağı seçin. 

2. Eşitleme işlemini başlatmak için **Eşitlemeyi Başlat'ı** tıklatın. 

3. **Eşitleme işleri** sekmesini tıklatarak geçerli eşitleme işinin veya öncekilerin durumunu görüntüleyin. 

4. Kaynak **Denetimi** açılır menüsünde bir kaynak denetim mekanizması seçin.

    ![Eşitleme durumu](./media/source-control-integration/sync-status.png)

5. Bir işi tıklatmak, iş çıktısını görüntülemenizi sağlar. Aşağıdaki örnek, kaynak denetimi eşitleme işinden gelen çıktıdır.

    ```output
    ===================================================================

    Azure Automation Source Control.
    Supported runbooks to sync: PowerShell Workflow, PowerShell Scripts, DSC Configurations, Graphical, and Python 2.

    Setting AzureRmEnvironment.

    Getting AzureRunAsConnection.

    Logging in to Azure...

    Source control information for syncing:

    [Url = https://ContosoExample.visualstudio.com/ContosoFinanceTFVCExample/_versionControl] [FolderPath = /Runbooks]

    Verifying url: https://ContosoExample.visualstudio.com/ContosoFinanceTFVCExample/_versionControl

    Connecting to VSTS...

    Source Control Sync Summary:

    2 files synced:
     - ExampleRunbook1.ps1
     - ExampleRunbook2.ps1

    ==================================================================

    ```

6. Kaynak Denetimi Eşitleme İş Özeti sayfasındaki **Tüm Günlükler** seçilerek ek günlüğe kaydetme kullanılabilir. Bu ek günlük girişleri, kaynak denetimi kullanırken ortaya çıkabilecek sorunları gidermenize yardımcı olabilir.

## <a name="disconnecting-source-control"></a>Kaynak denetiminin kesilmesi

Kaynak denetim deposundan bağlantı kesmek için:

1. Otomasyon hesabınızda **Hesap Ayarları** altında Kaynak **denetimi** açın.

2. Kaldırmak için kaynak denetim mekanizmasını seçin. 

3. Kaynak Denetimi Özeti sayfasında **Sil'i**tıklatın.

## <a name="handling-encoding-issues"></a>Kodlama sorunlarını işleme

Kaynak denetim deponuzda birden çok kişi farklı düzenleyicilerkullanarak runbook'ları düzenliyorsa, kodlama sorunları oluşabilir. Bu durum hakkında daha fazla bilgi edinmek [için, kodlama sorunlarının Yaygın nedenleri bölümüne](/powershell/scripting/components/vscode/understanding-file-encoding#common-causes-of-encoding-issues)bakın.

## <a name="updating-the-pat"></a>PAT'in güncellenmesi

Şu anda, kaynak denetiminde PAT'i güncelleştirmek için Azure portalını kullanamazsınız. PAT'nizin süresi dolduğunda veya iptal edildiğinde, kaynak denetimini yeni bir erişim belirteciyle şu yollardan biriyle güncelleyebilirsiniz:

* REST [API'yi](https://docs.microsoft.com/rest/api/automation/sourcecontrol/update)kullanın.
* [Update-AzAutomationSourceControl](/powershell/module/az.automation/update-azautomationsourcecontrol) cmdlet kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Runbook türleri ve bunların avantajları ve sınırlamaları hakkında daha fazla bilgi edinmek için [Azure Otomasyon runbook türlerine](automation-runbook-types.md)bakın.
