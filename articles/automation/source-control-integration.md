---
title: Azure Otomasyonu’nda kaynak denetimi tümleştirmesi
description: Bu makalede, Azure Otomasyonu 'nda GitHub ile kaynak denetimi tümleştirmesi açıklanmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 12/10/2019
ms.topic: conceptual
ms.openlocfilehash: eef67ca8111983adb4d9994894ba215240daee6f
ms.sourcegitcommit: d4a4f22f41ec4b3003a22826f0530df29cf01073
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78253740"
---
# <a name="source-control-integration-in-azure-automation"></a>Azure Otomasyonu’nda kaynak denetimi tümleştirmesi

 Azure Otomasyonu 'nda kaynak denetimi tümleştirmesi, kaynak denetimi deponuzdan tek yönlü eşitlemeyi destekler. Kaynak denetimi, GitHub veya Azure Repos kaynak denetimi deponuzdaki betiklerle Otomasyon hesabınızda runbook 'larınızı güncel tutmanızı sağlar. Bu özellik, geliştirme ortamınızda test edilmiş kodu Üretim otomasyon hesabınıza yükseltmeyi kolaylaştırır.
 
 Kaynak denetimi tümleştirmesini kullanarak ekibinizle kolayca işbirliği yapabilir, değişiklikleri izleyebilir ve Runbook 'larınızın önceki sürümlerine geri dönebilirsiniz. Örneğin, kaynak denetimi, kaynak denetimindeki farklı dalları geliştirme, test ve üretim Otomasyon hesaplarınızla eşitlemenize olanak tanır. 

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="source-control-types"></a>Kaynak Denetim türleri

Azure Otomasyonu üç tür kaynak denetimi destekler:

* GitHub
* Azure Repos (git)
* Azure Repos (TFVC)

## <a name="prerequisites"></a>Önkoşullar

* Kaynak denetimi deposu (GitHub veya Azure Repos)
* [Farklı Çalıştır hesabı](manage-runas-account.md)
* Otomasyon hesabınızda **az. Accounts** modülünü de içeren [en son Azure modülleri](automation-update-azure-modules.md) (Azurerm. Profile ' in az Module eşdeğeri)

> [!NOTE]
> Kaynak denetimi eşitleme işleri, kullanıcının Otomasyon hesabı altında çalışır ve diğer otomasyon işleriyle aynı hızda faturalandırılır.

## <a name="configuring-source-control"></a>Kaynak denetimini yapılandırma

Bu bölümde Otomasyon hesabınız için kaynak denetiminin nasıl yapılandırılacağı açıklanır. Azure portal veya PowerShell kullanabilirsiniz.

### <a name="configure-source-control----azure-portal"></a>Kaynak denetimini Yapılandırma--Azure portal

Azure portal kullanarak kaynak denetimini yapılandırmak için bu yordamı kullanın.

1. Otomasyon hesabınızda, **kaynak denetimi** ' ni seçin ve **+ Ekle**' ye tıklayın.

    ![Kaynak denetimi seçin](./media/source-control-integration/select-source-control.png)

2. **Kaynak denetim türünü**seçin ve ardından **kimlik doğrula**' ya tıklayın. 

3. Bir tarayıcı penceresi açılır ve oturum açmanızı ister. Kimlik doğrulamasını tamamlamaya yönelik istemleri izleyin.

4. **Kaynak denetimi Özeti** sayfasında, alanları, aşağıda tanımlanan kaynak denetimi özelliklerini dolduracak şekilde kullanın. İşiniz bittiğinde **Kaydet** ' e tıklayın. 

    |Özellik  |Açıklama  |
    |---------|---------|
    |Kaynak Denetim adı     | Kaynak denetimi için kolay bir ad. Bu ad yalnızca harf ve rakam içermelidir.        |
    |Kaynak Denetim türü     | Kaynak denetimi mekanizmasının türü. Kullanılabilen seçenekler:</br> GitHub</br>Azure Repos (git)</br> Azure Repos (TFVC)        |
    |Depo     | Deponun veya projenin adı. İlk 200 depo alınır. Bir depoyu aramak için alana adı yazın ve **GitHub 'Da ara**' yı tıklatın.|
    |Öğede     | Kaynak dosyaları çekilecek dal. TFVC kaynak denetimi türü için dal hedefleme kullanılamıyor.          |
    |Klasör yolu     | Eşitlenmesi gereken runbook 'ları içeren klasör, örneğin,/runbooks. Yalnızca belirtilen klasördeki runbook 'lar eşitlenir. Özyineleme desteklenmiyor.        |
    |Otomatik eşitleme<sup>1</sup>     | Kaynak denetim deposunda bir kayıt yapıldığında otomatik eşitlemeyi açan veya kapatan ayar.        |
    |Runbook 'U Yayımla     | Runbook 'lar kaynak denetiminden eşitlemeden sonra otomatik olarak yayımlanıyorsa ve aksi takdirde, ' nin ayarlanması.           |
    |Açıklama     | Kaynak denetimi ile ilgili ek ayrıntıları belirten metin.        |

    <sup>1</sup> Azure Repos ile kaynak denetimi tümleştirmesini yapılandırırken otomatik eşitlemeyi etkinleştirmek Için bir proje yöneticisi olmanız gerekir.

   ![Kaynak denetimi Özeti](./media/source-control-integration/source-control-summary.png)

> [!NOTE]
> Kaynak denetimi deponuz için oturum açma bilgileriniz Azure portal için oturum açınızdan farklı olabilir. Kaynak denetimini yapılandırırken kaynak denetimi deponuzun doğru hesabıyla oturum açtığınızdan emin olun. Şüpheli bir sorun varsa, tarayıcınızda yeni bir sekme açın, visualstudio.com veya github.com adresinden oturumunuzu açıp kaynak denetimine yeniden bağlanmayı deneyin.

### <a name="configure-source-control----powershell"></a>Kaynak denetimini yapılandırma--PowerShell

Azure Otomasyonu 'nda kaynak denetimi yapılandırmak için PowerShell de kullanabilirsiniz. Bu işlem için PowerShell cmdlet 'lerini kullanmak için bir kişisel erişim belirteci (PAT) gereklidir. Kaynak denetimi bağlantısını oluşturmak için [New-AzAutomationSourceControl](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationsourcecontrol?view=azps-3.5.0
) cmdlet 'ini kullanın. Bu cmdlet, PAT için güvenli bir dize gerektirir. Güvenli dize oluşturma hakkında bilgi edinmek için bkz. [ConvertTo-SecureString](/powershell/module/microsoft.powershell.security/convertto-securestring?view=powershell-6).

Aşağıdaki alt bölümlerde, GitHub, Azure Repos (git) ve Azure Repos (TFVC) için kaynak denetimi bağlantısının PowerShell oluşturması gösterilmektedir.

#### <a name="create-source-control-connection-for-github"></a>GitHub için kaynak denetimi bağlantısı oluşturma

```powershell-interactive
New-AzAutomationSourceControl -Name SCGitHub -RepoUrl https://github.com/<accountname>/<reponame>.git -SourceType GitHub -FolderPath "/MyRunbooks" -Branch master -AccessToken <secureStringofPAT> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

#### <a name="create-source-control-connection-for-azure-repos-git"></a>Azure Repos için kaynak denetimi bağlantısı oluşturma (git)

```powershell-interactive
New-AzAutomationSourceControl -Name SCReposGit -RepoUrl https://<accountname>.visualstudio.com/<projectname>/_git/<repositoryname> -SourceType VsoGit -AccessToken <secureStringofPAT> -Branch master -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName> -FolderPath "/Runbooks"
```

#### <a name="create-source-control-connection-for-azure-repos-tfvc"></a>Azure Repos için kaynak denetimi bağlantısı oluşturma (TFVC)

```powershell-interactive
New-AzAutomationSourceControl -Name SCReposTFVC -RepoUrl https://<accountname>.visualstudio.com/<projectname>/_versionControl -SourceType VsoTfvc -AccessToken <secureStringofPAT> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName> -FolderPath "/Runbooks"
```

#### <a name="personal-access-token-pat-permissions"></a>Kişisel erişim belirteci (PAT) izinleri

Kaynak denetimi, PATs için bazı minimum izinleri gerektirir. Aşağıdaki alt bölümler, GitHub ve Azure Repos için gereken en düşük izinleri içerir.

##### <a name="minimum-pat-permissions-for-github"></a>GitHub için en düşük PAT izinleri

Aşağıdaki tabloda, GitHub için gereken en düşük PAT izinleri tanımlanmaktadır. GitHub 'da bir PAT oluşturma hakkında daha fazla bilgi için, bkz. [komut satırı için kişisel erişim belirteci oluşturma](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).

|Kapsam  |Açıklama  |
|---------|---------|
|**depo**     |         |
|Depo: durum     | Erişim yapma durumu         |
|repo_deployment      | Erişim dağıtım durumu         |
|public_repo     | Genel depolara erişin         |
|**Yönetici: repo_hook**     |         |
|yazma: repo_hook     | Depo kancalarını Yaz         |
|okuma: repo_hook|Depo kancalarını okuma|

##### <a name="minimum-pat-permissions-for-azure-repos"></a>Azure Repos için en düşük PAT izinleri

Aşağıdaki liste Azure Repos için gereken en düşük PAT izinlerini tanımlar. Azure Repos bir PAT oluşturma hakkında daha fazla bilgi için bkz. [kişisel erişim belirteçleriyle erişim kimlik doğrulaması](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate).

| Kapsam  |  Erişim türü  |
|---------| ----------|
| Kod      | Okuma  |
| Proje ve takım | Okuma |
| Kimlik | Okuma     |
| Kullanıcı profili | Okuma     |
| İş öğeleri | Okuma    |
| Hizmet bağlantıları | Oku, sorgula, Yönet<sup>1</sup>    |

<sup>1</sup> hizmet bağlantıları izni yalnızca, oto eşitlemesini etkinleştirdiyseniz gereklidir.

## <a name="synchronizing"></a>Iz

Kaynak denetimiyle eşitlenmek için aşağıdakileri yapın. 

1. **Kaynak denetimi** sayfasındaki tablodan kaynağı seçin. 

2. Eşitleme işlemini başlatmak için **Eşitlemeyi Başlat** ' a tıklayın. 

3. **Eşitleme işleri** sekmesine tıklayarak geçerli eşitleme işinin durumunu veya önceki olanları görüntüleyin. 

4. **Kaynak denetimi** açılan menüsünde, bir kaynak denetim mekanizması seçin.

    ![Eşitleme durumu](./media/source-control-integration/sync-status.png)

5. Bir işi tıklatmak, iş çıktısını görüntülemenize izin verir. Aşağıdaki örnek, bir kaynak denetimi eşitleme işinin çıktıdır.

    ```output
    ============================================================================

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

     =========================================================================

    ```

6. **Kaynak denetimi eşitleme Işi Özeti** sayfasında **Tüm Günlükler** seçilerek ek günlüğe kaydetme kullanılabilir. Bu ek günlük girişleri, kaynak denetimi kullanılırken oluşabilecek sorunları gidermenize yardımcı olabilir.

## <a name="disconnecting-source-control"></a>Kaynak denetiminin bağlantısı kesiliyor

Kaynak denetim deposundan bağlantıyı kesmek için:

1. Otomasyon hesabınızda **Hesap ayarları** altında **kaynak denetimi** ' ni açın.

2. Kaldırılacak kaynak denetim mekanizmasını seçin. 

3. **Kaynak denetimi Özeti** sayfasında **Sil**' e tıklayın.

## <a name="handling-encoding-issues"></a>Kodlama sorunlarını işleme

Farklı düzenleyiciler kullanarak kaynak denetim deponuzdaki runbook 'ları birden çok kişi düzenliyorsanız, kodlama sorunları oluşabilir. Bu durumla ilgili daha fazla bilgi edinmek için bkz. [kodlama sorunlarının yaygın nedenleri](/powershell/scripting/components/vscode/understanding-file-encoding#common-causes-of-encoding-issues)

## <a name="updating-the-pat"></a>PAT güncelleştiriliyor

Şu anda, kaynak denetimindeki PAT 'yi güncelleştirmek için Azure portal kullanmanın bir yolu yoktur. PAT süreniz dolduktan veya iptal edildikten sonra, kaynak denetimini şu yollarla yeni bir erişim belirteciyle güncelleştirebilirsiniz:

* [REST API](https://docs.microsoft.com/rest/api/automation/sourcecontrol/update)kullanın.
* [Update-AzAutomationSourceControl](/powershell/module/az.automation/update-azautomationsourcecontrol) cmdlet 'ini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Runbook türleri ve bunların avantajları ve sınırlamaları hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu runbook türleri](automation-runbook-types.md).
