---
title: Azure Otomasyonu farklı çalıştır hesabı oluşturma
description: Bu makalede, PowerShell ile veya Azure portal farklı çalıştır hesabının nasıl oluşturulacağı açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 01/06/2021
ms.topic: conceptual
ms.openlocfilehash: ef6afff30da48b79b42e5fb4b3c72c3500f22dd1
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102172312"
---
# <a name="how-to-create-an-azure-automation-run-as-account"></a>Azure Otomasyonu farklı çalıştır hesabı oluşturma

Azure Otomasyonu 'nda farklı çalıştır hesapları, Otomasyon Runbook 'larını ve diğer otomasyon özelliklerini kullanarak Azure Resource Manager veya Azure klasik dağıtım modelinde kaynakları yönetmeye yönelik kimlik doğrulaması sağlar. Bu makalede, Azure portal veya Azure PowerShell bir Farklı Çalıştır veya klasik farklı çalıştır hesabının nasıl oluşturulacağı açıklanır.

## <a name="create-account-in-azure-portal"></a>Azure portal hesap oluşturma

Azure portal Azure Otomasyonu hesabınızı güncelleştirmek için aşağıdaki adımları gerçekleştirin. Farklı Çalıştır ve klasik farklı çalıştır hesapları ayrı olarak oluşturulur. Klasik kaynak oluşturmanıza gerek yoksa yalnızca Azure Farklı Çalıştır hesabını oluşturabilirsiniz.

1. Azure portalında Abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açın.

2. **Otomasyon hesaplarını** arayın ve seçin.

3. **Otomasyon hesapları** sayfasında, listeden Otomasyon hesabınızı seçin.

4. Sol bölmede **Hesap ayarları** bölümünde **Farklı Çalıştır hesapları** ' nı seçin.

    :::image type="content" source="media/create-run-as-account/automation-account-properties-pane.png" alt-text="Farklı Çalıştır hesabı seçeneğini belirleyin.":::

5. İhtiyaç duyduğunuz hesaba bağlı olarak, **+ Azure farklı çalıştır hesabı** veya **+ Azure klasik farklı çalıştır hesabı** bölmesini kullanın. Genel bakış bilgilerini inceledikten sonra **Oluştur**' a tıklayın.

    :::image type="content" source="media/create-run-as-account/automation-account-create-run-as.png" alt-text="Farklı Çalıştır hesabı oluşturma seçeneğini belirleyin":::

6. Azure Farklı Çalıştır hesabını oluşturduğu sırada menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz. Hesabın oluşturulduğunu belirten bir başlık de görüntülenir. İşlemin tamamlanması birkaç dakika sürebilir.

## <a name="create-account-using-powershell"></a>PowerShell kullanarak hesap oluşturma

Aşağıdaki listede, sağlanan bir betiği kullanarak PowerShell 'de bir farklı çalıştır hesabı oluşturma gereksinimleri verilmiştir. Bu gereksinimler her iki farklı çalıştır hesabı türü için de geçerlidir.

* Windows 10 veya Windows Server 2016 Azure Resource Manager modüller 3.4.1 ve üzeri. PowerShell betiği Windows 'un önceki sürümlerini desteklemez.
* PowerShell 6.2.4 veya üstünü Azure PowerShell. Bilgi için bkz. [Azure PowerShell nasıl yüklenir ve yapılandırılır](/powershell/azure/install-az-ps).
* Ve parametrelerinin değeri olarak başvurulan bir Otomasyon hesabı `AutomationAccountName` `ApplicationDisplayName` .
* [Farklı Çalıştır hesaplarını yapılandırmak Için gerekli izinler](automation-security-overview.md#permissions)bölümünde listelenenlere eşdeğer izinler.

`AutomationAccountName`, Ve için, `SubscriptionId` `ResourceGroupName` PowerShell betiği için gerekli parametreler olan değerleri almak için aşağıdaki adımları izleyin.

1. Azure portalında oturum açın.

1. **Otomasyon hesaplarını** arayın ve seçin.

1. Otomasyon hesapları sayfasında, listeden Otomasyon hesabınızı seçin.

1. Sol bölmede **Özellikler**' i seçin.

1. **Özellikler** sayfasındaki **ad**, **abonelik kimliği** ve **kaynak grubu** değerlerini göz önünde edin.

   ![Otomasyon hesabı Özellikler sayfası](media/create-run-as-account/automation-account-properties.png)

### <a name="powershell-script-to-create-a-run-as-account"></a>Farklı Çalıştır hesabı oluşturmak için PowerShell betiği

PowerShell betiği birçok yapılandırma için destek içerir.

* Otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı oluşturun.
* Otomatik olarak imzalanan bir sertifika kullanarak farklı çalıştır hesabı ve/veya klasik farklı çalıştır hesabı oluşturun.
* Kuruluş sertifika yetkiliniz (CA) tarafından verilen bir sertifika kullanarak bir farklı çalıştır hesabı ve/veya klasik farklı çalıştır hesabı oluşturun.
* Azure Kamu bulutundaki kendinden imzalı bir sertifika kullanarak bir farklı çalıştır hesabı ve/veya klasik farklı çalıştır hesabı oluşturun.

1. Aşağıdaki komutu kullanarak betiği indirip yerel bir klasöre kaydedin.

    ```powershell
    wget https://raw.githubusercontent.com/azureautomation/runbooks/master/Utility/AzRunAs/Create-RunAsAccount.ps1 -outfile Create-RunAsAccount.ps1
    ```

2. PowerShell 'i yükseltilmiş kullanıcı haklarıyla başlatın ve betiği içeren klasöre gidin.

3. Gereksinimlerinize göre farklı çalıştır ve/veya klasik farklı çalıştır hesabı oluşturmak için aşağıdaki komutlardan birini çalıştırın.

    * Otomatik olarak imzalanan bir sertifika kullanarak bir farklı çalıştır hesabı oluşturun.

        ```powershell
        .\Create-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false
        ```

    * Otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturun.

        ```powershell
        .\Create-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true
        ```

    * Kurumsal sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturun.

        ```powershell
        .\Create-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>
        ```

        Kurumsal ortak sertifika (. cer dosyası) ile bir klasik farklı çalıştır hesabı oluşturduysanız, bu sertifikayı kullanın. Betik, `%USERPROFILE%\AppData\Local\Temp` PowerShell oturumunu yürütmek için kullandığınız kullanıcı profili altında, bunu Bilgisayarınızdaki geçici dosyalar klasörüne oluşturur ve kaydeder. Bkz. [Azure Portal bir YÖNETIM API sertifikası yükleme](../cloud-services/cloud-services-configure-ssl-certificate-portal.md).

    * Azure Kamu bulutunda otomatik olarak imzalanan sertifika kullanarak Farklı Çalıştır hesabı ve Klasik Farklı Çalıştır hesabı oluşturma

        ```powershell
        .\Create-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnvironmentName AzureUSGovernment
        ```

4. Betik yürütüldükten sonra Azure ile kimlik doğrulaması yapmanız istenir. Abonelik yöneticileri rolünün üyesi olan bir hesapla oturum açın. Klasik farklı çalıştır hesabı oluşturuyorsanız, hesabınız aboneliğin ortak Yöneticisi olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* Grafik yazma hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu 'nda grafik runbook 'Ları yazma](automation-graphical-authoring-intro.md).
* PowerShell runbook 'larını kullanmaya başlamak için bkz. [öğretici: PowerShell runbook 'U oluşturma](learn/automation-tutorial-runbook-textual-powershell.md).
* Python 3 runbook 'u kullanmaya başlamak için bkz. [öğretici: Python 3 runbook oluşturma](learn/automation-tutorial-runbook-textual-python-3.md).
