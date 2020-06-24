---
title: Ölçek oturumu Azure Otomasyonu 'Nu barındırır-Azure
description: Windows sanal masaüstü oturumu konaklarının Azure Otomasyonu ile otomatik olarak ölçeklendirilmesi.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: f3a82665f197301fe81c448dd18181f0602bdbef
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85209802"
---
# <a name="scale-session-hosts-using-azure-automation"></a>Azure Otomasyonu 'Nu kullanarak oturum ana bilgisayarlarını ölçeklendirme

>[!IMPORTANT]
>Bu içerik, Windows sanal masaüstü nesneleri Azure Resource Manager desteklemeyen sonbahar 2019 sürümü için geçerlidir.

Sanal makinelerinizi (VM 'Ler) ölçeklendirerek toplam Windows sanal masaüstü dağıtım maliyetinizi azaltabilirsiniz. Bu, yoğun olmayan kullanım saatlerinde oturum ana bilgisayar VM 'lerinin kapatılması ve serbest bir şekilde çıkarılması, daha sonra yeniden açılması ve yoğun saatlerde yeniden bulunması anlamına gelir.

Bu makalede, Azure Otomasyonu ile derlenen ve Windows sanal masaüstü ortamınızdaki oturum ana bilgisayarı sanal makinelerini otomatik olarak ölçeklendirecek Azure Logic Apps ölçeklendirme aracı hakkında bilgi edineceksiniz. Ölçeklendirme aracının nasıl kullanılacağını öğrenmek için [önkoşulların](#prerequisites)önüne atlayın.

## <a name="report-issues"></a>Sorun bildirme

Ölçeklendirme aracı için sorun raporları Şu anda Microsoft Desteği yerine GitHub 'da işlenmekte. Ölçeklendirme aracıyla ilgili herhangi bir sorunla karşılaşırsanız, [RDS GitHub sayfasında](https://github.com/Azure/RDS-Templates/issues?q=is%3Aissue+is%3Aopen+label%3A4a-WVD-scaling-logicapps)"4A-WVD-ölçeklendirmesini-logicapps" olarak etiketlenmiş bir GitHub sorunu açmak için bu dosyaları bildirebilirsiniz.

## <a name="how-the-scaling-tool-works"></a>Ölçeklendirme aracının çalışması

Ölçeklendirme Aracı, oturum ana makinesi maliyetlerini iyileştirmek isteyen müşteriler için düşük maliyetli bir Otomasyon seçeneği sağlar.

Ölçeklendirme aracını kullanarak şunları yapabilirsiniz:

- VM 'Leri en yüksek ve en yoğun iş saatlerine göre başlatılacak ve durdurulacak şekilde zamanlayın.
- CPU çekirdeği başına oturum sayısına göre VM 'Leri ölçeklendirin.
- Yoğun olmayan saatlerde sanal makinelerin ölçeğini, çalışan en az sayıda oturum ana makinesi VM 'sini bırakarak ölçeklendirin.

Ölçeklendirme Aracı, Azure Otomasyonu PowerShell runbook 'ları, Web kancaları ve Azure Logic Apps işlev birleşimini kullanır. Araç çalıştırıldığında, Azure Otomasyonu runbook 'u başlatmak için bir Web kancası çağırır Azure Logic Apps. Runbook daha sonra bir iş oluşturur.

En yüksek kullanım süresi boyunca iş, her konak havuzu için geçerli çalışan oturum ana bilgisayarının geçerli oturum sayısını ve VM kapasitesini denetler. Çalışan oturum ana bilgisayar VM 'lerinin, **createazurelogicapp.ps1** dosyası Için tanımlanan *Sessionthresholdpercpu* parametresine göre mevcut oturumları destekleyebiliyor olup olmadığını hesaplamak için bu bilgileri kullanır. Oturum Ana bilgisayar VM 'Leri mevcut oturumları destekleyemiyorum, iş konak havuzundaki ek oturum ana bilgisayar VM 'lerini başlatır.

>[!NOTE]
>*Sessionthresholdpercpu* , sanal makine üzerindeki oturum sayısını kısıtlamaz. Bu parametre yalnızca yeni VM 'Lerin, bağlantıların yük dengelenmesi için ne zaman başlatılması gerektiğini belirler. Oturum sayısını kısıtlamak için, *Maxsessionlimit* parametresini uygun şekilde yapılandırmak için [set-RdsHostPool](/powershell/module/windowsvirtualdesktop/set-rdshostpool/) yönergelerini izlemeniz gerekir.

En yoğun kullanım süresi boyunca, iş, hangi oturum ana bilgisayar VM 'lerinin *Minimumnumberofrdsh* parametresine bağlı olarak kapanması gerektiğini belirler. İş, konaklara bağlanan yeni oturumları engellemek için oturum ana bilgisayarları 'nı boşalt moduna ayarlar. *Limitsecondstoforcelogoffuser* parametresini sıfır olmayan pozitif bir değere ayarlarsanız, iş, oturum açmış olan tüm kullanıcılara, çalışmalarını kaydetmesi, yapılandırılan süreyi beklemesi ve ardından kullanıcıları oturumu kapatmaya zorlayacaktır. Oturum Ana bilgisayar VM 'si üzerindeki tüm Kullanıcı oturumları oturumu kapatıldıktan sonra, iş sanal makineyi kapatır.

*Limitsecondstoforcelogoffuser* parametresini sıfır olarak ayarlarsanız, iş, belirtilen grup ilkelerindeki oturum yapılandırma ayarının kullanıcı oturumlarını kapatmayı işlemesine izin verir. Bu grup ilkelerini görmek için **Computer Configuration**  >  **Policies**  >  **Administrative Templates**  >  **Windows bileşenleri**  >  **Terminal Hizmetleri**  >  **Terminal sunucusu**  >  **oturumu süre sınırları**Yönetim Şablonları bilgisayar yapılandırma ilkeleri ' ne gidin. Bir oturum ana bilgisayar VM 'sinde etkin bir oturum varsa, iş, oturum ana bilgisayar VM 'sini çalışır durumda bırakır. Etkin oturum yoksa, iş, oturum ana bilgisayarı sanal makinesini kapatır.

İş düzenli aralıklarla ayarlanan yineleme aralığına göre çalışır. Windows sanal masaüstü ortamınızın boyutuna bağlı olarak bu aralığı değiştirebilirsiniz, ancak sanal makinelerin başlatılması ve kapatılması biraz zaman alabilir, bu nedenle gecikmeyi hesaba erteleyebilirsiniz. Yinelenme aralığını her 15 dakikada bir ayarlamanız önerilir.

Ancak, araç aşağıdaki sınırlamalara de sahiptir:

- Bu çözüm yalnızca havuza alınmış oturum ana bilgisayar VM 'Leri için geçerlidir.
- Bu çözüm, herhangi bir bölgedeki VM 'Leri yönetir, ancak yalnızca Azure Otomasyonu hesabınızla aynı abonelikte ve Azure Logic Apps kullanılabilir.

>[!NOTE]
>Ölçeklendirme Aracı, ölçeklendirildiği konak havuzunun yük dengeleme modunu denetler. Bu, hem yoğun hem de yoğun olmayan saatlerde, ilk yük dengeleyiciyi belirler.

## <a name="prerequisites"></a>Ön koşullar

Ölçeklendirme aracı 'nı ayarlamaya başlamadan önce, aşağıdaki şeyleri hazırlamış olduğunuzdan emin olun:

- Bir [Windows sanal masaüstü kiracısı ve konak havuzu](create-host-pools-arm-template.md)
- Windows sanal masaüstü hizmeti ile yapılandırılan ve kaydettirilen oturum ana bilgisayar havuzu VM 'Leri
- Azure aboneliğinde [katkıda bulunan erişimi](../../role-based-access-control/role-assignments-portal.md) olan bir Kullanıcı

Aracı dağıtmak için kullandığınız makine şunları içermelidir:

- Windows PowerShell 5,1 veya üzeri
- Microsoft az PowerShell modülü

Her şeyi hazırlayın ve kullanmaya başlayın.

## <a name="create-an-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma

İlk olarak, PowerShell runbook 'unu çalıştırmak için bir Azure Otomasyonu hesabınızın olması gerekir. Hesabınızı nasıl ayarlayaöğreneceksiniz:

1. Windows PowerShell'i yönetici olarak açın.
2. Azure hesabınızda oturum açmak için aşağıdaki cmdlet 'i çalıştırın.

     ```powershell
     Login-AzAccount
     ```

     >[!NOTE]
     >Hesabınız, ölçeklendirme aracını dağıtmak istediğiniz Azure aboneliğinde katılımcı haklarına sahip olmalıdır.

3. Azure Otomasyonu hesabını oluşturmak için betiği indirmek üzere aşağıdaki cmdlet 'i çalıştırın:

     ```powershell
     Set-Location -Path "c:\temp"
     $uri = "https://raw.githubusercontent.com/Azure/RDS-Templates/master/wvd-templates/wvd-scaling-script/createazureautomationaccount.ps1"
     Invoke-WebRequest -Uri $uri -OutFile ".\createazureautomationaccount.ps1"
     ```

4. Betiği yürütmek ve Azure Otomasyonu hesabını oluşturmak için aşağıdaki cmdlet 'i çalıştırın:

     ```powershell
     .\createazureautomationaccount.ps1 -SubscriptionID <azuresubscriptionid> -ResourceGroupName <resourcegroupname> -AutomationAccountName <name of automation account> -Location "Azure region for deployment"
     ```

5. Cmdlet 'in çıktısı, bir Web kancası URI 'sini içerir. Azure Logic Apps için yürütme zamanlamasını ayarlarken bu parametreyi bir parametre olarak kullanacağınız için URI kaydını kaydettiğinizden emin olun.

6. Azure Otomasyonu hesabınızı ayarladıktan sonra Azure aboneliğinizde oturum açın ve aşağıdaki görüntüde gösterildiği gibi Azure Otomasyonu hesabınızın ve ilgili runbook 'un belirtilen kaynak grubunda göründüğünden emin olun:

![Yeni oluşturulan Otomasyon hesabını ve Runbook 'u gösteren Azure genel bakış sayfasının bir görüntüsü.](../media/automation-account.png)

  Web kancası 'nizin nerede olması gerektiğini denetlemek için Runbook 'un adını seçin. Sonra, runbook 'un kaynaklar bölümüne gidin ve **Web kancaları**' nı seçin.

## <a name="create-an-azure-automation-run-as-account"></a>Azure Otomasyonu farklı çalıştır hesabı oluşturma

Artık bir Azure Otomasyonu hesabınız olduğuna göre, Azure kaynaklarınıza erişmek için bir Azure Otomasyonu farklı çalıştır hesabı da oluşturmanız gerekir.

Azure [Otomasyonu farklı çalıştır hesabı](../../automation/manage-runas-account.md) , Azure 'da Azure cmdlet 'leriyle kaynakları yönetmeye yönelik kimlik doğrulaması sağlar. Farklı Çalıştır hesabı oluşturduğunuzda, Azure Active Directory yeni bir hizmet sorumlusu kullanıcısı oluşturur ve katılımcı rolünü abonelik düzeyinde hizmet sorumlusu kullanıcısına atar; Azure farklı çalıştır hesabı, bir kimlik bilgisi nesnesinde Kullanıcı adı ve parola depolamaya gerek kalmadan sertifikalar ve hizmet sorumlusu adıyla güvenli kimlik doğrulaması yapmanın harika bir yoludur. Farklı çalıştır kimlik doğrulaması hakkında daha fazla bilgi edinmek için bkz. [Farklı Çalıştır hesabı Izinlerini sınırlandırma](../../automation/manage-runas-account.md#limit-run-as-account-permissions).

Abonelik yöneticileri rolü ve aboneliğin ortak Yöneticisi üyesi olan herhangi bir Kullanıcı, sonraki bölümde yer alan yönergeleri izleyerek bir farklı çalıştır hesabı oluşturabilir.

Azure hesabınızda bir farklı çalıştır hesabı oluşturmak için:

1. Azure portal, **tüm hizmetler**' i seçin. Kaynak listesinde **Otomasyon hesapları**girin ve seçin.

2. **Otomasyon hesapları** sayfasında, Otomasyon hesabınızın adını seçin.

3. Pencerenin sol tarafındaki bölmede hesap ayarları bölümünde **Farklı Çalıştır hesapları** ' nı seçin.

4. **Azure farklı çalıştır hesabı**' nı seçin. **Azure farklı çalıştır hesabı ekle** bölmesi göründüğünde, genel bakış bilgilerini gözden geçirin ve ardından **Oluştur** ' u seçerek hesap oluşturma işlemini başlatın.

5. Azure 'un farklı çalıştır hesabını oluşturması için birkaç dakika bekleyin. Oluşturma ilerleme durumunu menüdeki Bildirimler bölümünde izleyebilirsiniz.

6. İşlem tamamlandığında, belirtilen Otomasyon hesabında AzureRunAsConnection adlı bir varlık oluşturur. Bağlantı varlığı uygulama KIMLIĞI, kiracı KIMLIĞI, abonelik KIMLIĞI ve sertifika parmak izini barındırır. Daha sonra kullanacağınız için uygulama KIMLIĞINI unutmayın.

### <a name="create-a-role-assignment-in-windows-virtual-desktop"></a>Windows sanal masaüstünde rol ataması oluşturma

Daha sonra, AzureRunAsConnection 'un Windows sanal masaüstü ile etkileşime girebilmesi için bir rol ataması oluşturmanız gerekir. Rol atamaları oluşturma izinlerine sahip bir hesapla oturum açmak için PowerShell 'i kullandığınızdan emin olun.

İlk olarak, henüz yapmadıysanız PowerShell oturumunuzda kullanmak üzere [Windows sanal masaüstü PowerShell modülünü](/powershell/windows-virtual-desktop/overview/) indirip içeri aktarın. Windows sanal masaüstüne bağlanmak ve Kiracılarınızı göstermek için aşağıdaki PowerShell cmdlet 'lerini çalıştırın.

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"

Get-RdsTenant
```

Ölçeklendirmek istediğiniz konak havuzlarıyla kiracıyı bulduğunuzda, [Azure Otomasyonu hesabı oluşturma](#create-an-azure-automation-account) ' daki yönergeleri izleyin ve rol atamasını oluşturmak için aşağıdaki cmdlet 'teki önceki cmdlet 'ten aldığınız kiracı adını kullanın:

```powershell
New-RdsRoleAssignment -RoleDefinitionName "RDS Contributor" -ApplicationId <applicationid> -TenantName <tenantname>
```

## <a name="create-the-azure-logic-app-and-execution-schedule"></a>Azure Logic App ve yürütme zamanlaması oluşturma

Son olarak, Azure Logic App 'i oluşturmanız ve yeni ölçeklendirme aracınız için bir yürütme zamanlaması ayarlamanız gerekir.

1.  Windows PowerShell 'i yönetici olarak açın

2.  Azure hesabınızda oturum açmak için aşağıdaki cmdlet 'i çalıştırın.

     ```powershell
     Login-AzAccount
     ```

3. createazurelogicapp.ps1 betik dosyasını yerel makinenize indirmek için aşağıdaki cmdlet 'i çalıştırın.

     ```powershell
     Set-Location -Path "c:\temp"
     $uri = "https://raw.githubusercontent.com/Azure/RDS-Templates/master/wvd-templates/wvd-scaling-script/createazurelogicapp.ps1"
     Invoke-WebRequest -Uri $uri -OutFile ".\createazurelogicapp.ps1"
     ```

4. RDS sahibi veya RDS katkıda bulunan izinleri olan bir hesapla Windows sanal masaüstü 'nde oturum açmak için aşağıdaki cmdlet 'i çalıştırın.

     ```powershell
     Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
     ```

5. Azure Logic App ve yürütme zamanlamasını oluşturmak için aşağıdaki PowerShell betiğini çalıştırın.

     ```powershell
     $aadTenantId = (Get-AzContext).Tenant.Id

     $azureSubscription = Get-AzSubscription | Out-GridView -PassThru -Title "Select your Azure Subscription"
     Select-AzSubscription -Subscription $azureSubscription.Id
     $subscriptionId = $azureSubscription.Id

     $resourceGroup = Get-AzResourceGroup | Out-GridView -PassThru -Title "Select the resource group for the new Azure Logic App"
     $resourceGroupName = $resourceGroup.ResourceGroupName
     $location = $resourceGroup.Location

     $wvdTenant = Get-RdsTenant | Out-GridView -PassThru -Title "Select your WVD tenant"
     $tenantName = $wvdTenant.TenantName

     $wvdHostpool = Get-RdsHostPool -TenantName $wvdTenant.TenantName | Out-GridView -PassThru -Title "Select the host pool you'd like to scale"
     $hostPoolName = $wvdHostpool.HostPoolName

     $recurrenceInterval = Read-Host -Prompt "Enter how often you'd like the job to run in minutes, e.g. '15'"
     $beginPeakTime = Read-Host -Prompt "Enter the start time for peak hours in local time, e.g. 9:00"
     $endPeakTime = Read-Host -Prompt "Enter the end time for peak hours in local time, e.g. 18:00"
     $timeDifference = Read-Host -Prompt "Enter the time difference between local time and UTC in hours, e.g. +5:30"
     $sessionThresholdPerCPU = Read-Host -Prompt "Enter the maximum number of sessions per CPU that will be used as a threshold to determine when new session host VMs need to be started during peak hours"
     $minimumNumberOfRdsh = Read-Host -Prompt "Enter the minimum number of session host VMs to keep running during off-peak hours"
     $limitSecondsToForceLogOffUser = Read-Host -Prompt "Enter the number of seconds to wait before automatically signing out users. If set to 0, users will be signed out immediately"
     $logOffMessageTitle = Read-Host -Prompt "Enter the title of the message sent to the user before they are forced to sign out"
     $logOffMessageBody = Read-Host -Prompt "Enter the body of the message sent to the user before they are forced to sign out"

     $automationAccount = Get-AzAutomationAccount -ResourceGroupName $resourceGroup.ResourceGroupName | Out-GridView -PassThru
     $automationAccountName = $automationAccount.AutomationAccountName
     $automationAccountConnection = Get-AzAutomationConnection -ResourceGroupName $resourceGroup.ResourceGroupName -AutomationAccountName $automationAccount.AutomationAccountName | Out-GridView -PassThru -Title "Select the Azure RunAs connection asset"
     $connectionAssetName = $automationAccountConnection.Name

     $webHookURI = Read-Host -Prompt "Enter the URI of the WebHook returned by when you created the Azure Automation Account"
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

     Betiği çalıştırdıktan sonra, aşağıdaki görüntüde gösterildiği gibi mantıksal uygulamanın bir kaynak grubunda görünmesi gerekir.

     ![Örnek bir Azure mantıksal uygulaması için genel bakış sayfasının bir görüntüsü.](../media/logic-app.png)

Yineleme zaman dilimini veya saat dilimini değiştirme gibi yürütme zamanlamasında değişiklik yapmak için otomatik ölçeklendirme Scheduler ' a gidin ve Logic Apps tasarımcısına gitmek için **Düzenle** ' yi seçin.

![Logic Apps tasarımcısının bir görüntüsü. Kullanıcının yineleme sürelerini ve Web kancası dosyasını düzenlemesini sağlayan yineleme ve Web kancası menüleri.](../media/logic-apps-designer.png)

## <a name="manage-your-scaling-tool"></a>Ölçeklendirme aracınızı yönetme

Ölçeklendirme aracınızı oluşturdığınıza göre, çıktısına erişebilirsiniz. Bu bölümde yararlı bulabileceğiniz bazı özellikler açıklanmaktadır.

### <a name="view-job-status"></a>İş durumunu görüntüleme

Tüm runbook işlerinin özetlenen durumunu görüntüleyebilir veya Azure portal belirli bir runbook işinin daha ayrıntılı bir durumunu görüntüleyebilirsiniz.

Seçtiğiniz Otomasyon hesabınızın sağ tarafındaki "Iş Istatistikleri" bölümünde, tüm runbook işlerinin özetlerinin bir listesini görüntüleyebilirsiniz. Pencerenin sol tarafındaki **işler** sayfasını açmak geçerli iş durumlarını, başlangıç zamanlarını ve tamamlanma zamanlarını gösterir.

![İş durumu sayfasının ekran görüntüsü.](../media/jobs-status.png)

### <a name="view-logs-and-scaling-tool-output"></a>Günlükleri görüntüleme ve ölçeklendirme aracı çıktısı

Runbook 'unuzu açıp işinizin adını seçerek genişleme ve ölçek işlemleri işlemlerini izleyebilirsiniz.

Azure Otomasyonu hesabını barındıran kaynak grubunuzda runbook 'a gidin (varsayılan ad WVDAutoScaleRunbook) ve **genel bakış**' ı seçin. Genel Bakış sayfasında, aşağıdaki görüntüde gösterildiği gibi, ölçek aracı çıktısını görüntülemek için son Işler altında bir iş seçin.

![Ölçeklendirme aracı için çıkış penceresinin bir görüntüsü.](../media/tool-output.png)

