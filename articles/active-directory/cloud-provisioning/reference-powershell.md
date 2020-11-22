---
title: Azure AD Connect bulut eşitlemesi için AADCloudSyncTools PowerShell modülü
description: Bu makalede Azure AD Connect bulut sağlama aracısının nasıl yükleneceği açıklanır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 11/16/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: d3ce5a7a313ab0f0680558aa60b34e3ebb9b51c9
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2020
ms.locfileid: "95255160"
---
# <a name="aadcloudsynctools-powershell-module-for-azure-ad-connect-cloud-sync"></a>Azure AD Connect bulut eşitlemesi için AADCloudSyncTools PowerShell modülü

Genel Önizleme yenileme 2 sürümü sayesinde, Microsoft AADCloudSyncTools PowerShell modülünü sunmuştur.  Bu modül, Azure AD Connect bulut eşitleme dağıtımlarınızın yönetilmesine yardımcı olmak için kullanabileceğiniz bir dizi kullanışlı araç sağlar.

## <a name="pre-requisites"></a>Ön koşullar
Aşağıdaki önkoşulların olması gerekir:
- Bu modül, MSAL kimlik doğrulamasını kullanır, bu nedenle MSAL.PS modülünün yüklü olmasını gerektirir. Artık Azure AD 'ye veya Azure AD önizlemesine bağlı değildir.   Doğrulamak için, bir yönetici PowerShell penceresinde yürütün `Get-module MSAL.PS` . Modül doğru şekilde yüklenirse bir yanıt alırsınız.  `Install-AADCloudSyncToolsPrerequisites`En son msal.PS sürümünü yüklemek için kullanabilirsiniz.
- AzureAD PowerShell modülü.  Bazı cmdlet 'ler, görevlerini gerçekleştirmek için AzureAD PowerShell modülünün parçalarını kullanır.  Doğrulamak için, bir yönetici PowerShell penceresinde yürütün `Get-module AzureAD` . Yanıt almanız gerekir.  `Install-AADCloudSyncToolsPrerequisites`AzureAD PowerShell modülünün en son sürümünü yüklemek için ' i kullanabilirsiniz.
- PowerShell 'den modül yükleme, TLS 1,2 kullanarak zorlayabilir.  Modül yükleyebilmeniz için aşağıdakileri ayarlayın: \
`[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 `

## <a name="install-the-aadcloudsynctools-powershell-module"></a>AADCloudSyncTools PowerShell modülünü yükler
AADCloudSyncTools modülünü yüklemek ve kullanmak için aşağıdaki adımları kullanın:

1.  Windows PowerShell 'i yönetici ayrıcalıklarıyla açın
2.  Yazın `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` ve ENTER tuşuna basın.
3.  Aşağıdakileri yazın veya kopyalayıp yapıştırın: 
    ``` powershell
    Import-module -Name "C:\Program Files\Microsoft Azure AD Connect Provisioning Agent\Utility\AADCloudSyncTools"
    ```
3.  ENTER tuşuna basın.
4.  Modülün yüklendiğini doğrulamak için aşağıdakileri girin veya kopyalayıp yapıştırın "
    ```powershell
    Get-module AADCloudSyncTools
    ```
5.  Artık modülle ilgili bilgileri görmeniz gerekir.
6.  Sonraki çalıştırma
    ``` powershell
    Install-AADCloudSyncToolsPrerequisites
    ```
7.  Bu, PowerShell Get modüllerini yükler.  PowerShell penceresini kapatın.
8.  Windows PowerShell 'i yönetici ayrıcalıklarıyla açın
9.  Adım 3 ' ü kullanarak modülü yeniden içeri aktarın.
10. `Install-AADCloudSyncToolsPrerequisites`Msal ve AzureAD modüllerini yüklemek için çalıştırın
11. Tüm ön yeniden QS yükleme modülünü başarıyla yüklemiş olmalıdır ![](media/reference-powershell/install-1.png)

## <a name="aadcloudsynctools--cmdlets"></a>AADCloudSyncTools cmdlet 'Leri
### <a name="connect-aadcloudsynctools"></a>Connect-AADCloudSyncTools
Microsoft Graph belirteç istemek için Azure AD 'ye ve MSAL.PS modülüne bağlanmak için AzureAD modülünü kullanır


### <a name="export-aadcloudsynctoolslogs"></a>Export-AADCloudSyncToolsLogs
Sıkıştırılmış bir dosyadaki tüm sorun giderme verilerini aşağıdaki şekilde dışa aktarır ve paketler:
 1. Start-AADCloudSyncToolsVerboseLogs ile ayrıntılı bir izleme başlatır.  Bu izleme günlüklerini C:\ProgramData\Microsoft\Azure AD Connect Provisioning Agent\Trace klasöründe bulabilirsiniz.
 2. 3 dakika boyunca bir izleme günlüğü toplar.
   -TracingDurationMins ile farklı bir saat belirtebilir veya-SkipVerboseTrace ile ayrıntılı izlemeyi atlayabilirsiniz
 3. Stop-AADCloudSyncToolsVerboseLogs ile ayrıntılı izlemeyi durduruyor
 4. Son 24 saat için Olay Görüntüleyicisi günlüklerini toplar
 5. Tüm aracı günlüklerini, ayrıntılı günlükleri ve Olay Görüntüleyici günlüklerini kullanıcının Belgeler klasörü altındaki sıkıştırılmış bir ZIP dosyasında sıkıştırır. 
 </br>-OutputPath ile farklı bir çıkış klasörü belirtebilirsiniz \<folder path\>

### <a name="get-aadcloudsynctoolsinfo"></a>Get-AADCloudSyncToolsInfo
Azure AD kiracı ayrıntılarını ve iç değişkenlerin durumunu gösterir

### <a name="get-aadcloudsynctoolsjob"></a>Get-AADCloudSyncToolsJob
, AD2AAD hizmet sorumlularını almak ve eşitleme Işi bilgilerini geri döndürmek için Graf kullanır.
Bir parametre olarak belirli eşitleme Işi KIMLIĞI kullanılarak da çağrılabilir.

### <a name="get-aadcloudsynctoolsjobschedule"></a>Get-AADCloudSyncToolsJobSchedule
, AD2AAD hizmet sorumlularını almak için Graf kullanır ve eşitleme Işinin zamanlamasını döndürür.
Bir parametre olarak belirli eşitleme Işi KIMLIĞI kullanılarak da çağrılabilir.

### <a name="get-aadcloudsynctoolsjobschema"></a>Get-AADCloudSyncToolsJobSchema
, AD2AAD hizmet sorumlularını almak için Graf kullanır ve eşitleme Işinin şemasını döndürür.

### <a name="get-aadcloudsynctoolsjobscope"></a>Get-AADCloudSyncToolsJobScope
, Belirtilen eşitleme Işi KIMLIĞI için eşitleme Işinin şemasını almak ve tüm filtre grubu kapsamlarını çıktılar için Graf kullanır.

### <a name="get-aadcloudsynctoolsjobsettings"></a>Get-AADCloudSyncToolsJobSettings
, AD2AAD hizmet sorumlularını almak için Graf kullanır ve eşitleme Işinin ayarlarını döndürür.
Bir parametre olarak belirli eşitleme Işi KIMLIĞI kullanılarak da çağrılabilir.

### <a name="get-aadcloudsynctoolsjobstatus"></a>Get-AADCloudSyncToolsJobStatus
, AD2AAD hizmet sorumlularını almak için Graf kullanır ve eşitleme Işinin durumunu döndürür.
Bir parametre olarak belirli eşitleme Işi KIMLIĞI kullanılarak da çağrılabilir.

### <a name="get-aadcloudsynctoolsserviceprincipal"></a>Get-AADCloudSyncToolsServicePrincipal
AD2AAD ve/veya SyncFabric için hizmet sorumlusunu almak üzere Graf kullanır.
Parametresiz, yalnızca AD2AAD hizmet sorumlusu döndürülür.

### <a name="install-aadcloudsynctoolsprerequisites"></a>Install-AADCloudSyncToolsPrerequisites
PowerShellGet v 2.2.4.1 veya üzeri ve Azure AD ve MSAL.PS modüllerinin varlığını denetler ve eksik ise bunları kurar.

### <a name="invoke-aadcloudsynctoolsgraphquery"></a>Invoke-AADCloudSyncToolsGraphQuery
Parametre olarak belirtilen URI, yöntem ve gövde için bir Web isteği çağırır

### <a name="repair-aadcloudsynctoolsaccount"></a>Repair-AADCloudSyncToolsAccount
Geçerli hesabı (varsa) silmek için Azure AD PowerShell 'i kullanır ve eşitleme hesabı kimlik doğrulamasını Azure AD 'de yeni bir eşitleme hesabıyla sıfırlar.

### <a name="restart-aadcloudsynctoolsjob"></a>Restart-AADCloudSyncToolsJob
Tam eşitlemeyi yeniden başlatır.

### <a name="resume-aadcloudsynctoolsjob"></a>Resume-AADCloudSyncToolsJob
Önceki filigrandan eşitlemeye devam eder.

### <a name="start-aadcloudsynctoolsverboselogs"></a>Start-AADCloudSyncToolsVerboseLogs
Ayrıntılı izlemeyi etkinleştirmek ve AADConnectProvisioningAgent hizmetini yeniden başlatmak için ' AADConnectProvisioningAgent.exe.config ' değerini değiştirerek hizmetin yeniden başlatılmasını engellemek için-SkipServiceRestart kullanabilirsiniz, ancak herhangi bir yapılandırma değişikliği etkili olmayacaktır.  Bu izleme günlüklerini C:\ProgramData\Microsoft\Azure AD Connect Provisioning Agent\Trace klasöründe bulabilirsiniz.

### <a name="stop-aadcloudsynctoolsverboselogs"></a>Stop-AADCloudSyncToolsVerboseLogs
Ayrıntılı izlemeyi devre dışı bırakmak için ' AADConnectProvisioningAgent.exe.config ' öğesini değiştirir ve AADConnectProvisioningAgent hizmetini yeniden başlatır. Hizmetin yeniden başlatılmasını engellemek için-SkipServiceRestart kullanabilirsiniz, ancak herhangi bir yapılandırma değişikliği etkili olmayacaktır.

### <a name="suspend-aadcloudsynctoolsjob"></a>Suspend-AADCloudSyncToolsJob
Eşitlemeyi duraklatır.

## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)

