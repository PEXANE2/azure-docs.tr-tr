---
title: Ölçek oturumu Azure Otomasyonu 'na ev sahipliği yapıyor - Azure
description: Azure Otomasyonu ile Windows Sanal Masaüstü oturum ana bilgisayarlarını otomatik olarak ölçeklendirme.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 3a853dc32f8716f3f2ba32896a7a4a239efcc5bd
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80349874"
---
# <a name="scale-session-hosts-using-azure-automation"></a>Azure Otomasyonu'ni kullanarak oturum ana bilgisayarlarını ölçeklendirin

Sanal makinelerinizi (VM) ölçekleyerek toplam Windows Sanal Masaüstü dağıtım maliyetinizi azaltabilirsiniz. Bu, oturum ana bilgisayar VM'lerini yoğun olmayan kullanım saatlerinde kapatıp devre dışı bırakarak, daha sonra bunları yeniden açıp yoğun saatlerde yeniden tahsis etmek anlamına gelir.

Bu makalede, Windows Sanal Masaüstü ortamınızda oturum barındırma sanal makinelerini otomatik olarak ölçeklendirecek Azure Otomasyonu ve Azure Mantık Uygulamaları ile oluşturulmuş ölçeklendirme aracı hakkında bilgi edineceksiniz. Ölçekleme aracını nasıl kullanacağınızı öğrenmek için [Önkoşullar'a](#prerequisites)geçin.

## <a name="how-the-scaling-tool-works"></a>Ölçekleme aracı nasıl çalışır?

Ölçekleme aracı, oturum ana bilgisayar VM maliyetlerini optimize etmek isteyen müşteriler için düşük maliyetli bir otomasyon seçeneği sağlar.

Ölçekleme aracını aşağıdakiler için kullanabilirsiniz:
 
- Peak ve Off-Peak çalışma saatlerine göre vm'leri başlatmak ve durdurmak için zamanlayın.
- CPU çekirdeği başına oturum sayısına göre VM'leri ölçeklendirin.
- Yoğun olmayan saatlerde VM'lerde ölçeklendirin ve minimum oturum ana bilgisayar VM sayısını çalışır durumda bırakın.

Ölçekleme aracı, çalışması için Azure Automation PowerShell runbook'lar, web hook'lar ve Azure Logic Apps'ın bir birleşimini kullanır. Araç çalıştığında, Azure Logic Apps Azure Otomasyon runbook'u başlatmak için bir webhook çağırır. Runbook sonra bir iş oluşturur.

En yoğun kullanım süresi boyunca, iş geçerli oturum sayısını ve her ana bilgisayar havuzu için geçerli çalışan oturum ana bilgisayarının VM kapasitesini denetler. Bu, çalışan oturum ana bilgisayarı VM'lerin **createazurelogicapp.ps1** dosyası için tanımlanan *SessionThresholdPerCPU* parametresini temel alan varolan oturumları destekletip destekleyemeyebileceğini hesaplamak için bu bilgileri kullanır. Oturum ana bilgisayar VM'leri varolan oturumları desteklenemezse, iş ana bilgisayar havuzunda ek oturum ana bilgisayar VM'leri başlatır.

>[!NOTE]
>*SessionThresholdPerCPU* VM'deki oturum sayısını kısıtlamaz. Bu parametre, bağlantıları yüklemek için yalnızca yeni VM'lerin ne zaman başlatılması gerektiğini belirler. Oturum sayısını kısıtlamak için, *MaxSessionLimit* parametresini buna göre yapılandırmak için [Set-RdsHostPool](/powershell/module/windowsvirtualdesktop/set-rdshostpool/) yönergelerini izlemeniz gerekir.

Yoğun olmayan kullanım süresi boyunca, iş hangi oturum ana bilgisayar VM'lerinin *MinimumNumberOfRDSH* parametreye göre kapatılması gerektiğini belirler. İş, yeni oturumların ana bilgisayarlara bağlanmasını önlemek için oturum ana bilgisayar VM'lerini boşalacak şekilde ayarlar. *LimitSecondsToForceLogOffUser* parametresini sıfır olmayan pozitif bir değere ayarlarsanız, iş, şu anda oturum açmış olan kullanıcıları işlerini kaydetmeleri, yapılandırılan süreyi beklemeleri ve ardından kullanıcıları oturumu kapatmaya zorlayacak. Oturum ana bilgisayar VM'deki tüm kullanıcı oturumları kapatıldıktan sonra, iş VM'yi kapatır.

*LimitSecondsToForceLogOffUser* parametresini sıfıra ayarlarsanız, iş kullanıcı oturumlarını imzalamayı işlemek için belirtilen grup ilkelerinde oturum yapılandırma ayarı sağlar. Bu grup ilkelerini görmek için **Bilgisayar Yapılandırma** > **İlkeleri** > Yönetim**Şablonları** > Windows**Bileşenleri** > **Terminal Hizmetleri** > **TerminalI Sunucu** > **Oturum Zaman Sınırları'na**gidin. Oturum ana bilgisayar VM'de etkin oturum varsa, iş oturum ana bilgisayar VM'yi çalışır durumda bırakır. Etkin oturum yoksa, iş oturum ana bilgisayar VM'yi kapatır.

İş, ayarlanmış yineleme aralığını temel alan düzenli aralıklarla çalışır. Bu aralığı Windows Sanal Masaüstü ortamınızın boyutuna göre değiştirebilirsiniz, ancak sanal makineleri başlatmanın ve kapatmanın biraz zaman alacağını unutmayın, bu nedenle gecikmeyi hesaba kattığınızı unutmayın. Yineleme aralığını her 15 dakikada bir ayarlamanızı öneririz.

Ancak, araç da aşağıdaki sınırlamalar vardır:

- Bu çözüm yalnızca havuza alan oturum ana bilgisayar VM'leri için geçerlidir.
- Bu çözüm, herhangi bir bölgedeki VM'leri yönetir, ancak yalnızca Azure Otomasyon hesabınız ve Azure Mantıksal Uygulamalarınızla aynı abonelikte kullanılabilir.

>[!NOTE]
>Ölçekleme aracı, ölçekletittiyi barındıran havuzun yük dengeleme modunu kontrol eder. Hem yoğun hem de yoğun olmayan saatler için ilk yük dengelemesine ayarlar.

## <a name="prerequisites"></a>Ön koşullar

Ölçekleme aracını ayarlamaya başlamadan önce, aşağıdaki şeylerin hazır olduğundan emin olun:

- [Windows Sanal Masaüstü kiracı ve ana bilgisayar havuzu](create-host-pools-arm-template.md)
- Oturum ana bilgisayar havuzu VM'ler Windows Sanal Masaüstü hizmetine yapılandırıldı ve kaydedildi
- Azure aboneliğinde [Katılımcı erişimi](../role-based-access-control/role-assignments-portal.md) olan bir kullanıcı

Aracı dağıtmak için kullandığınız makinede aşağıdakiler olmalıdır: 

- Windows PowerShell 5.1 veya sonrası
- Microsoft Az PowerShell modülü

Her şey hazırsa, başlayalım.

## <a name="create-an-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma

İlk olarak, PowerShell runbook'u çalıştırmak için bir Azure Otomasyon uyrması gerekir. Hesabınızı şu şekilde ayarlayabilirsiniz:

1. Windows PowerShell'i yönetici olarak açın.
2. Azure Hesabınızda oturum açabilmek için aşağıdaki cmdlet'i çalıştırın.

     ```powershell
     Login-AzAccount
     ```

     >[!NOTE]
     >Hesabınız, ölçekleme aracını dağıtmak istediğiniz Azure aboneliğinde katkıda bulunma haklarına sahip olmalıdır.

3. Azure Otomasyon uyrama komut dosyasını indirmek için aşağıdaki cmdlet'i çalıştırın:

     ```powershell
     Set-Location -Path "c:\temp"
     $uri = "https://raw.githubusercontent.com/Azure/RDS-Templates/master/wvd-templates/wvd-scaling-script/createazureautomationaccount.ps1"
     Invoke-WebRequest -Uri $uri -OutFile ".\createazureautomationaccount.ps1"
     ```

4. Komut dosyasını çalıştırmak ve Azure Otomasyon uyrama hesabını oluşturmak için aşağıdaki cmdlet'i çalıştırın:

     ```powershell
     .\createazureautomationaccount.ps1 -SubscriptionID <azuresubscriptionid> -ResourceGroupName <resourcegroupname> -AutomationAccountName <name of automation account> -Location "Azure region for deployment"
     ```

5. Cmdlet çıkış bir webhook URI içerecektir. Azure Logic uygulamalarının yürütme zamanlamasını ayarlarken parametre olarak kullanacağınız için URI'nin kaydını sakladığından emin olun.

6. Azure Otomasyon hesabınızı kurduktan sonra Azure aboneliğinizde oturum açın ve Azure Otomasyon hesabınızın ve ilgili runbook'unuzun aşağıdaki resimde gösterildiği gibi belirtilen kaynak grubunda göründüğünden emin olun:

   ![Yeni oluşturulan otomasyon hesabını ve runbook'u gösteren Azure genel bakış sayfasının görüntüsü.](media/automation-account.png)

  Webhook'unuzun olması gereken yerde olup olmadığını kontrol etmek için runbook'unuzun adını seçin. Ardından, runbook'unuzun Kaynaklar bölümüne gidin ve **Webhooks'u**seçin.

## <a name="create-an-azure-automation-run-as-account"></a>Hesap Olarak Azure Otomasyon Çalıştır Oluşturma

Artık bir Azure Otomasyon hesabınız olduğuna göre, Azure kaynaklarına erişmek için bir Azure Otomasyon Çalıştır hesabı da oluşturmanız gerekir.

[Azure Otomasyon Çalıştır Hesabı,](../automation/manage-runas-account.md) Azure cmdlets ile Azure'daki kaynakları yönetmek için kimlik doğrulaması sağlar. Bir Run As hesabı oluşturduğunuzda, Azure Etkin Dizini'nde yeni bir hizmet yöneticisi kullanıcı oluşturur ve Katılımcı rolünü abonelik düzeyinde hizmet yöneticisi kullanıcıya atar, Azure Run As Hesabı güvenli bir şekilde kimlik doğrulaması yapmanın harika bir yoludur bir kimlik bilgisi nesnesinde bir kullanıcı adı ve parola depolamak gerek kalmadan sertifikaları ve hizmet ana adı. Kimlik doğrulaması olarak çalıştır hakkında daha fazla bilgi edinmek için, [Hesap İzinleri Olarak Çalıştır'ı Sınırlama'ya](../automation/manage-runas-account.md#limiting-run-as-account-permissions)bakın.

Abonelik Yöneticileri rolünün üyesi ve aboneliğin yardımcı yöneticisi olan herhangi bir kullanıcı, sonraki bölümün yönergelerini izleyerek Bir Run As hesabı oluşturabilir.

Azure hesabınızda Bir Run As hesabı oluşturmak için:

1. Azure portalında **Tüm hizmetler'i**seçin. Kaynaklar listesinde Otomasyon **Hesapları'na**girin ve seçin.

2. Otomasyon **Hesapları** sayfasında, Otomasyon hesabınızın adını seçin.

3. Pencerenin sol tarafındaki bölmede, Hesap Ayarları bölümünde **Hesap Olarak Çalıştır'ı** seçin.

4. **Hesap Olarak Azure Çalıştır'ı**seçin. Azure **Hesabı Olarak Çalıştır** bölmesi göründüğünde, genel bakış bilgilerini gözden geçirin ve ardından hesap oluşturma işlemini başlatmak için **Oluştur'u** seçin.

5. Azure'un Çalıştır Hesabı'nı oluşturması için birkaç dakika bekleyin. Bildirimler altında menüde oluşturma ilerleme izleyebilirsiniz.

6. İşlem bittiğinde, belirtilen Otomasyon hesabında AzureRunAsConnection adında bir varlık oluşturur. Bağlantı varlığı uygulama kimliğini, kiracı kimliğini, abonelik kimliğini ve sertifika parmak izini tutar. Uygulama kimliğini hatırlayın, çünkü daha sonra kullanacaksınız.

### <a name="create-a-role-assignment-in-windows-virtual-desktop"></a>Windows Sanal Masaüstünde rol ataması oluşturma

Ardından, AzureRunAsConnection'ın Windows Sanal Masaüstü ile etkileşim kurabilmesi için bir rol ataması oluşturmanız gerekir. Rol atamaları oluşturma izni olan bir hesapla oturum açmak için PowerShell'i kullandığınızdan emin olun.

İlk olarak, PowerShell oturumunuzda kullanmak üzere [Windows Virtual Desktop PowerShell modülünü](/powershell/windows-virtual-desktop/overview/) indirin ve içe aktarın. Windows Sanal Masaüstü'ne bağlanmak ve kiracılarınızı görüntülemek için aşağıdaki PowerShell cmdletlerini çalıştırın.

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"

Get-RdsTenant
```

Ölçeklendirmek istediğiniz ana bilgisayar havuzlarıolan kiracıyı bulduğunuzda, [Azure Otomasyonu hesabı oluşturma'daki](#create-an-azure-automation-account) yönergeleri izleyin ve rol atamasını oluşturmak için aşağıdaki cmdlet'teki önceki cmdlet'ten aldığınız kiracı adını kullanın:

```powershell
New-RdsRoleAssignment -RoleDefinitionName "RDS Contributor" -ApplicationId <applicationid> -TenantName <tenantname>
```

## <a name="create-the-azure-logic-app-and-execution-schedule"></a>Azure Mantık Uygulaması ve yürütme takvimioluşturma

Son olarak, Azure Mantık Uygulaması'nı oluşturmanız ve yeni ölçekleme aracınız için bir yürütme çizelgesi ayarlamanız gerekir.

1.  Windows PowerShell'i Yönetici Olarak Açma

2.  Azure Hesabınızda oturum açabilmek için aşağıdaki cmdlet'i çalıştırın.

     ```powershell
     Login-AzAccount
     ```

3. Createazurelogicapp.ps1 komut dosyası dosyasını yerel makinenize indirmek için aşağıdaki cmdlet'i çalıştırın.

     ```powershell
     Invoke-WebRequest -Uri "https://raw.githubusercontent.com/Azure/RDS-Templates/master/wvd-templates/wvd-scaling-script/createazurelogicapp.ps1" -OutFile "your local machine path\ createazurelogicapp.ps1"
     ```

4. RDS Sahibi veya RDS Katılımcısı izinleri olan bir hesapla Windows Sanal Masaüstünde oturum açabilmek için aşağıdaki cmdlet'i çalıştırın.

     ```powershell
     Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
     ```

5. Azure Logic uygulamasını ve yürütme zamanlamasını oluşturmak için aşağıdaki PowerShell komut dosyasını çalıştırın.

     ```powershell
     $resourceGroupName = Read-Host -Prompt "Enter the name of the resource group for the new Azure Logic App"
     
     $aadTenantId = Read-Host -Prompt "Enter your Azure AD tenant ID"

     $subscriptionId = Read-Host -Prompt "Enter your Azure Subscription ID"

     $tenantName = Read-Host -Prompt "Enter the name of your WVD tenant"

     $hostPoolName = Read-Host -Prompt "Enter the name of the host pool you'd like to scale"

     $recurrenceInterval = Read-Host -Prompt "Enter how often you'd like the job to run in minutes, e.g. '15'"

     $beginPeakTime = Read-Host -Prompt "Enter the start time for peak hours in local time, e.g. 9:00"

     $endPeakTime = Read-Host -Prompt "Enter the end time for peak hours in local time, e.g. 18:00"

     $timeDifference = Read-Host -Prompt "Enter the time difference between local time and UTC in hours, e.g. +5:30"

     $sessionThresholdPerCPU = Read-Host -Prompt "Enter the maximum number of sessions per CPU that will be used as a threshold to determine when new session host VMs need to be started during peak hours"

     $minimumNumberOfRdsh = Read-Host -Prompt "Enter the minimum number of session host VMs to keep running during off-peak hours"

     $limitSecondsToForceLogOffUser = Read-Host -Prompt "Enter the number of seconds to wait before automatically signing out users. If set to 0, users will be signed out immediately"

     $logOffMessageTitle = Read-Host -Prompt "Enter the title of the message sent to the user before they are forced to sign out"

     $logOffMessageBody = Read-Host -Prompt "Enter the body of the message sent to the user before they are forced to sign out"

     $location = Read-Host -Prompt "Enter the name of the Azure region where you will be creating the logic app"

     $connectionAssetName = Read-Host -Prompt "Enter the name of the Azure RunAs connection asset"

     $webHookURI = Read-Host -Prompt "Enter the URI of the WebHook returned by when you created the Azure Automation Account"

     $automationAccountName = Read-Host -Prompt "Enter the name of the Azure Automation Account"

     $maintenanceTagName = Read-Host -Prompt "Enter the name of the Tag associated with VMs you don't want to be managed by this scaling tool"

     .\createazurelogicapp.ps1 -ResourceGroupName $resourceGroupName `
       -AADTenantID $aadTenantId `
       -SubscriptionID $subscriptionId `
       -TenantName $tenantName `
       -HostPoolName $hostPoolName `
       -RecurrenceInterval $recurrenceInterval `
       -BeginPeakTime $beginPeakTime `
       -EndPeakTime $endPeakTime `
       -TimeDifference $timeDifference `
       -SessionThresholdPerCPU $sessionThresholdPerCPU `
       -MinimumNumberOfRDSH $minimumNumberOfRdsh `
       -LimitSecondsToForceLogOffUser $limitSecondsToForceLogOffUser `
       -LogOffMessageTitle $logOffMessageTitle `
       -LogOffMessageBody $logOffMessageBody `
       -Location $location `
       -ConnectionAssetName $connectionAssetName `
       -WebHookURI $webHookURI `
       -AutomationAccountName $automationAccountName `
       -MaintenanceTagName $maintenanceTagName
     ```

     Komut dosyasını çalıştırdıktan sonra, Mantık Uygulaması aşağıdaki resimde gösterildiği gibi bir kaynak grubunda görünmelidir.

     ![Örnek bir Azure Mantık Uygulaması için genel bakış sayfasının görüntüsü.](media/logic-app.png)

Yineleme aralığını veya saat dilimini değiştirme gibi yürütme zamanlamasında değişiklik yapmak için Otomatik Ölçeklendirme zamanlayıcısına gidin ve Logic Apps Tasarımcısı'na gitmek için **Edit'i** seçin.

![Logic Apps Tasarımcısı'nın bir resmi. Kullanıcının yineleme sürelerini ve webhook dosyasını yeniden de leten menüleri açılır.](media/logic-apps-designer.png)

## <a name="manage-your-scaling-tool"></a>Ölçekleme aracınızı yönetme

Ölçekleme aracınızı oluşturduğunuza göre, çıktısına erişebilirsiniz. Bu bölümde yararlı bulabileceğiniz birkaç özellik açıklanmaktadır.

### <a name="view-job-status"></a>İş durumunu görüntüleme

Tüm runbook işlerinin özet durumunu görüntüleyebilir veya Azure portalındaki belirli bir runbook işinin daha ayrıntılı durumunu görüntüleyebilirsiniz.

Seçili Otomasyon hesabınızın sağında, "İş İstatistikleri" altında, tüm runbook işlerinin özetlistesini görüntüleyebilirsiniz. Pencerenin sol tarafındaki **İşler** sayfasını açmak, geçerli iş durumlarını, başlangıç saatlerini ve tamamlanma saatlerini gösterir.

![İş durumu sayfasının ekran görüntüsü.](media/jobs-status.png)

### <a name="view-logs-and-scaling-tool-output"></a>Günlükleri ve ölçekleme aracı çıktısını görüntüleme

Runbook'unuzu açarak ve işinizin adını seçerek ölçeklendirme ve ölçeklendirme işlemlerinin günlüklerini görüntüleyebilirsiniz.

Azure Otomasyon hesabını barındıran kaynak grubunuzdaki runbook'a (varsayılan ad WVDAutoScaleRunbook) gidin ve **Genel Bakış'ı**seçin. Genel bakış sayfasında, aşağıdaki resimde gösterildiği gibi ölçekleme aracı çıktısını görüntülemek için Son İşler altında bir iş seçin.

![Ölçekleme aracı için çıkış penceresinin görüntüsü.](media/tool-output.png)

## <a name="report-issues"></a>Sorun bildirme

Ölçekleme aracıyla ilgili herhangi bir sorunla karşılaşırsanız, bunları [RDS GitHub sayfasından](https://github.com/Azure/RDS-Templates/issues?q=is%3Aissue+is%3Aopen+label%3A4a-WVD-scaling-logicapps)bildirebilirsiniz.
