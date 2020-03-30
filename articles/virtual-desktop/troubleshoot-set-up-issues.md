---
title: Windows Sanal Masaüstü kiracı ana bilgisayar havuzu oluşturma - Azure
description: Windows Sanal Masaüstü kiracı ortamının kurulumu sırasında kiracı ve ana bilgisayar havuzu sorunlarını giderme ve giderme sorunu nasıl giderilir?
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 01/08/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 36b15b41279edc60d337a7ba70abe2ca64d4bc7f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371605"
---
# <a name="tenant-and-host-pool-creation"></a>Kiracı ve ana bilgisayar havuzu oluşturma

Bu makalede, Windows Sanal Masaüstü kiracı ve ilgili oturum ana bilgisayar havuzu altyapısının ilk kurulumu sırasında sorunları kapsar.

## <a name="provide-feedback"></a>Geri bildirimde bulunma

Windows Sanal Masaüstü Hizmeti'ni ürün ekibi ve etkin topluluk üyeleriyle tartışmak için [Windows Sanal Masaüstü Teknik Topluluğu'nu](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) ziyaret edin.

## <a name="acquiring-the-windows-10-enterprise-multi-session-image"></a>Windows 10 Enterprise çok oturumlu görüntüyü edinme

Windows 10 Enterprise çok oturumlu görüntüyü kullanmak için Azure Marketi'ne gidin, Başlangıç**Microsoft Windows 10** > ve [Sanal Masaüstü bilgisayarlar için Windows 10 Enterprise, Sürüm 1809'u](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsdesktop.windows-10?tab=PlansAndPrice) **seçin.** > 

![Sanal Masaüstü Bilgisayarlar için Windows 10 Enterprise,Sürüm 1809'u seçmenin ekran görüntüsü.](media/AzureMarketPlace.png)

## <a name="creating-windows-virtual-desktop-tenant"></a>Windows Sanal Masaüstü kiracı oluşturma

Bu bölümde, Windows Sanal Masaüstü kiracı oluştururken olası sorunları kapsar.

### <a name="error-the-user-isnt-authorized-to-query-the-management-service"></a>Hata: Kullanıcının yönetim hizmetini sorgulama yetkisi yok

![Bir kullanıcının yönetim hizmetini sorgulama yetkisinin olmadığı PowerShell penceresinin ekran görüntüsü.](media/UserNotAuthorizedNewTenant.png)

Ham hata örneği:

```Error
   New-RdsTenant : User isn't authorized to query the management service.
   ActivityId: ad604c3a-85c6-4b41-9b81-5138162e5559
   Powershell commands to diagnose the failure:
   Get-RdsDiagnosticActivities -ActivityId ad604c3a-85c6-4b41-9b81-5138162e5559
   At line:1 char:1
   + New-RdsTenant -Name "testDesktopTenant" -AadTenantId "01234567-89ab-c ...
   + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
       + CategoryInfo          : FromStdErr: (Microsoft.RDInf...nt.NewRdsTenant:NewRdsTenant) [New-RdsTenant], RdsPowerSh
      ellException
       + FullyQualifiedErrorId : UnauthorizedAccess,Microsoft.RDInfra.RDPowershell.Tenant.NewRdsTenant
```

**Sebep:** Oturum açmış olan kullanıcı, Azure Etkin Dizininde Kiracı Oluşturucu rolüne atanmadı.

**Düzeltme:** Azure Etkin [Dizin iden bir kullanıcıya TenantCreator uygulama rolünü atama](tenant-setup-azure-active-directory.md#assign-the-tenantcreator-application-role)yönergelerini izleyin. Yönergeleri takip ettikten sonra TenantCreator rolüne atanmış bir kullanıcınız olur.

![TenantCreator rolünün ekran görüntüsü atandı.](media/TenantCreatorRoleAssigned.png)

## <a name="creating-windows-virtual-desktop-session-host-vms"></a>Windows Sanal Masaüstü oturum ana bilgisayar VM oluşturma

Oturum ana bilgisayar VM'leri çeşitli şekillerde oluşturulabilir, ancak Windows Sanal Masaüstü ekibi yalnızca [Azure Marketi](https://azuremarketplace.microsoft.com/) teklifiyle ilgili VM sağlama sorunlarını destekler. Daha fazla bilgi için Bkz. [Windows Sanal Masaüstü'nü kullanan Sorunlar - Ana bilgisayar havuzu Azure Market teklifini sağlama](#issues-using-windows-virtual-desktop--provision-a-host-pool-azure-marketplace-offering).

## <a name="issues-using-windows-virtual-desktop--provision-a-host-pool-azure-marketplace-offering"></a>Windows Sanal Masaüstü'nü kullanan sorunlar – Ana bilgisayar havuzu Azure Marketi teklifini sağlama

Windows Sanal Masaüstü – Kullanılabilir ana bilgisayar havuzu şablonu Azure Marketi'nden kullanılabilir.

### <a name="error-when-using-the-link-from-github-the-message-create-a-free-account-appears"></a>Hata: GitHub'daki bağlantıyı kullanırken "Ücretsiz hesap oluştur" iletisi görüntülenir

![Ücretsiz bir hesap oluşturmak için ekran görüntüsü.](media/be615904ace9832754f0669de28abd94.png)

**Neden 1:** Azure'da oturum açmak için kullanılan hesapta etkin abonelik ler yoktur veya kullanılan hesabın abonelikleri görüntüleme izni yoktur.

**Düzeltme 1:** Oturum ana bilgisayar VM'lerinin dağıtılanacağı aboneye katılımcı erişimi olan bir hesapla (en azından) oturum açın.

**Neden 2:** Kullanılan abonelik, Microsoft Bulut Hizmeti Sağlayıcısı (CSP) kiracısının bir parçasıdır.

**Düzeltme 2:** **Yeni Windows Sanal Masaüstü ana bilgisayar havuzu oluşturmak ve sağlama** için GitHub konumuna gidin ve aşağıdaki yönergeleri izleyin:

1. Azure'a **Dağıt'a** sağ tıklayın ve **Bağlantı Adresini Kopyala'yı**seçin.
2. **Not Defteri'ni** açın ve bağlantıyı yapıştırın.
3. # karakterinden önce CSP son müşteri kiracı adını ekleyin.
4. Yeni bağlantıyı bir tarayıcıda açın ve Azure portalı şablonu yükler.

    ```Example
    Example: https://portal.azure.com/<CSP end customer tenant name>
    #create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%
    2FRDS-Templates%2Fmaster%2Fwvd-templates%2FCreate%20and%20provision%20WVD%20host%20pool%2FmainTemplate.json
    ```

### <a name="error-you-receive-template-deployment-is-not-valid-error"></a>Hata: "Şablon dağıtımı geçerli değil" hatası alırsınız

![Ekran görüntüsü "şablon dağıtım ... geçerli değildir" hatası](media/troubleshooting-marketplace-validation-error-generic.png)

Belirli bir eylemde bulunmadan önce, başarısız dağıtım doğrulaması için ayrıntılı hatayı görmek için etkinlik günlüğünü denetlemeniz gerekir.

Etkinlik günlüğündeki hatayı görüntülemek için:

1. Geçerli Azure Marketi dağıtım teklifinden çıkın.
2. Üst arama çubuğunda, Aktivite **Günlüğü'nü**arayın ve seçin.
3. **Başarısız** durumu olan **Dağıtımı Doğrula** adlı bir etkinlik bulun ve etkinliği seçin.
   ![**Başarısız** durumuyla tek tek **Dağıtımı Doğrula** etkinliğinin ekran görüntüsü](media/troubleshooting-marketplace-validation-error-activity-summary.png)

4. JSON'u seçin ve ardından "statusMessage" alanını görene kadar ekranın altına doğru ilerleyin.
   ![JSON metninin statusMessage özelliğinin etrafında kırmızı bir kutu yla başarısız olan etkinliğin ekran görüntüsü.](media/troubleshooting-marketplace-validation-error-json-boxed.png)

İşlem şablonunuzun kota sınırını aşması durumunda, bunu düzeltmek için aşağıdaki şeylerden birini yapabilirsiniz:

 - Azure Marketi'ni ilk kez kullandığınız parametrelerle çalıştırın, ancak bu sefer daha az VM ve VM çekirdeği kullanın.
 - Belirtilen VM SKU için Azure aboneliğinizin kotasını artırmak için bir istek göndermek için bir tarayıcıdaki **statusMessage** alanında gördüğünüz bağlantıyı açın.

## <a name="azure-resource-manager-template-and-powershell-desired-state-configuration-dsc-errors"></a>Azure Kaynak Yöneticisi şablonu ve PowerShell İstenilen Durum Yapılandırması (DSC) hataları

Azure Kaynak Yöneticisi şablonlarının ve PowerShell DSC'nin başarısız dağıtımlarını gidermek için bu yönergeleri izleyin.

1. Azure Kaynak Yöneticisi [ile dağıtım işlemlerini görüntüle'yi](../azure-resource-manager/resource-manager-deployment-operations.md)kullanarak dağıtımdaki hataları gözden geçirin.
2. Dağıtımda hata yoksa, [kaynaklardaki eylemleri denetlemek için etkinlik günlüklerini görüntülemek'i](../azure-resource-manager/resource-group-audit.md)kullanarak etkinlik günlüğündeki hataları gözden geçirin.
3. Hata tanımlandıktan sonra, sorunu gidermek için Hata iletisini ve [Azure Kaynak Yöneticisi ile sık karşılaşılan Azure dağıtım hatalarını giderme](../azure-resource-manager/resource-manager-common-deployment-errors.md) deki kaynakları kullanın.
4. Önceki dağıtım sırasında oluşturulan kaynakları silin ve şablonu yeniden dağıtmayı deneyin.

### <a name="error-your-deployment-failedhostnamejoindomain"></a>Hata: Dağıtımınız başarısız\<oldu.... ana bilgisayar adı>/joindomain

![Dağıtım Başarısız ekran görüntüsünüz.](media/e72df4d5c05d390620e07f0d7328d50f.png)

Ham hata örneği:

```Error
 {"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details.
 Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\": \"Failed\",\r\n \"error\":
 {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n \"message\": \"The resource operation completed with terminal provisioning state 'Failed'.
 \",\r\n \"details\": [\r\n {\r\n \"code\": \"VMExtensionProvisioningError\",\r\n \"message\": \"VM has reported a failure when processing
 extension 'joindomain'. Error message: \\\"Exception(s) occurred while joining Domain 'diamondsg.onmicrosoft.com'\\\".\"\r\n }\r\n ]\r\n }\r\n}"}]}
```

**Neden 1:** VM'leri etki alanına katılmak için sağlanan kimlik bilgileri yanlış.

**Düzeltme 1:** VM'ler için "Yanlış kimlik bilgileri" hatasına bakın [Oturum ana bilgisayar VM yapılandırmasında](troubleshoot-vm-configuration.md)etki alanına katılmaz.

**Neden 2:** Alan adı çözülmüyor.

**Düzeltme 2:** Bkz. Hata: Etki alanı adı [Oturum ana bilgisayar VM yapılandırmasında](troubleshoot-vm-configuration.md) [çözülmüyor.](troubleshoot-vm-configuration.md#error-domain-name-doesnt-resolve)

**Neden 3:** Sanal ağınız (VNET) DNS yapılandırmanız **Varsayılan**olarak ayarlanır.

Bunu düzeltmek için aşağıdaki leri yapın:

1. Azure Portalı'nı açın ve **Sanal ağlar** sekmesine gidin.
2. VNET'inizi bulun, ardından **DNS sunucularını**seçin.
3. DNS sunucuları menüsü ekranınızın sağ tarafında görünmelidir. Bu menüde **Özel'i**seçin.
4. Özel altında listelenen DNS sunucularının etki alanı denetleyiciniz veya Active Directory etki alanınızla eşleştidiğinden emin olun. DNS sunucunuzu görmüyorsanız, değerini **DNS sunucu ekle** alanına girerek ekleyebilirsiniz.

### <a name="error-your-deployment-failedunauthorized"></a>Hata: Dağıtımınız başarısız oldu...\Yetkisiz

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"Unauthorized","message":"{\r\n \"Code\": \"Unauthorized\",\r\n \"Message\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\",\r\n \"Target\": null,\r\n \"Details\": [\r\n {\r\n \"Message\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\"\r\n },\r\n {\r\n \"Code\": \"Unauthorized\"\r\n },\r\n {\r\n \"ErrorEntity\": {\r\n \"ExtendedCode\": \"52020\",\r\n \"MessageTemplate\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\",\r\n \"Parameters\": [\r\n \"default\"\r\n ],\r\n \"Code\": \"Unauthorized\",\r\n \"Message\": \"The scale operation is not allowed for this subscription in this region. Try selecting different region or scale option.\"\r\n }\r\n }\r\n ],\r\n \"Innererror\": null\r\n}"}]}
```

**Sebep:** Kullandığınız abonelik, müşterinin dağıtmaya çalıştığı bölgede gerekli özelliklere erişemeyen bir türdür. Örneğin, MSDN, Free veya Education abonelikleri bu hatayı gösterebilir.

**Düzeltme:** Abonelik türünüzü veya bölgenizi gerekli özelliklere erişebilecek biriyle değiştirin.

### <a name="error-vmextensionprovisioningerror"></a>Hata: VMExtensionProvisioningError

![Dağıtımınızın ekran görüntüsü, terminal sağlama durumu başarısız oldu.](media/7aaf15615309c18a984673be73ac969a.png)

**Neden 1:** Windows Sanal Masaüstü ortamında geçici hata.

**Neden 2:** Bağlantı ile geçici hata.

**Düzeltme:** PowerShell kullanarak oturum aayarak Windows Sanal Masaüstü ortamının sağlıklı olduğunu onaylayın. [PowerShell ile ana bilgisayar havuzu oluştur'da](create-host-pools-powershell.md)VM kaydını el ile bitirin.

### <a name="error-the-admin-username-specified-isnt-allowed"></a>Hata: Belirtilen Yönetici Kullanıcı Adı'na izin verilmiyor

![Dağıtımınızın ekran görüntüsü, belirtilen bir yöneticinin izin verilmeyen başarısız oldu.](media/f2b3d3700e9517463ef88fa41875bac9.png)

Ham hata örneği:

```Error
 { "id": "/subscriptions/EXAMPLE/resourceGroups/demoHostDesktop/providers/Microsoft.
  Resources/deployments/vmCreation-linkedTemplate/operations/EXAMPLE", "operationId": "EXAMPLE", "properties": { "provisioningOperation":
 "Create", "provisioningState": "Failed", "timestamp": "2019-01-29T20:53:18.904917Z", "duration": "PT3.0574505S", "trackingId":
 "1f460af8-34dd-4c03-9359-9ab249a1a005", "statusCode": "BadRequest", "statusMessage": { "error": { "code": "InvalidParameter", "message":
 "The Admin Username specified is not allowed.", "target": "adminUsername" } }, "targetResource": { "id": "/subscriptions/EXAMPLE
 /resourceGroups/demoHostDesktop/providers/Microsoft.Compute/virtualMachines/demo", "resourceType": "Microsoft.Compute/virtualMachines", "resourceName": "demo" } }}
```

**Sebep:** Sağlanan parola yasak alt dizeleri (yönetici, yönetici, kök) içerir.

**Düzeltme:** Kullanıcı adını güncelleştirin veya farklı kullanıcılar kullanın.

### <a name="error-vm-has-reported-a-failure-when-processing-extension"></a>Hata: VM uzantısı işleme bir hata bildirdi

![Dağıtım Başarısız Lığınızda terminal sağlama durumuyla tamamlanan kaynak işleminin ekran görüntüsü.](media/49c4a1836a55d91cd65125cf227f411f.png)

Ham hata örneği:

```Error
{ "id": "/subscriptions/EXAMPLE/resourceGroups/demoHostD/providers/Microsoft.Resources/deployments/
 rds.wvd-provision-host-pool-20190129132410/operations/5A0757AC9E7205D2", "operationId": "5A0757AC9E7205D2", "properties":
 { "provisioningOperation": "Create", "provisioningState": "Failed", "timestamp": "2019-01-29T21:43:05.1416423Z",
 "duration": "PT7M56.8150879S", "trackingId": "43c4f71f-557c-4abd-80c3-01f545375455", "statusCode": "Conflict",
 "statusMessage": { "status": "Failed", "error": { "code": "ResourceDeploymentFailure", "message":
 "The resource operation completed with terminal provisioning state 'Failed'.", "details": [ { "code":
 "VMExtensionProvisioningError", "message": "VM has reported a failure when processing extension 'dscextension'.
 Error message: \"DSC Configuration 'SessionHost' completed with error(s). Following are the first few:
 PowerShell DSC resource MSFT_ScriptResource failed to execute Set-TargetResource functionality with error message:
 One or more errors occurred. The SendConfigurationApply function did not succeed.\"." } ] } }, "targetResource":
 { "id": "/subscriptions/EXAMPLE/resourceGroups/demoHostD/providers/Microsoft.
 Compute/virtualMachines/desktop-1/extensions/dscextension",
 "resourceType": "Microsoft.Compute/virtualMachines/extensions", "resourceName": "desktop-1/dscextension" } }}
```

**Sebep:** PowerShell DSC uzantısı VM üzerinde yönetici erişimi elde etmek mümkün değildi.

**Düzeltme:** Kullanıcı adı ve parolayı onaylayın, sanal makinede yönetim erişimine sahip olun ve Azure Kaynak Yöneticisi şablonuna yeniden çalıştırın.

### <a name="error-deploymentfailed--powershell-dsc-configuration-firstsessionhost-completed-with-errors"></a>Hata: DeploymentFailed – PowerShell DSC Yapılandırması 'FirstSessionHost' Hata(lar) ile tamamlandı

![PowerShell DSC Yapılandırması 'FirstSessionHost' hatası ile tamamlanan dağıtım ekran görüntüsü hatasıdır.)](media/64870370bcbe1286906f34cf0a8646ab.png)

Ham hata örneği:

```Error
{
    "code": "DeploymentFailed",
   "message": "At least one resource deployment operation failed. Please list
 deployment operations for details. 4 Please see https://aka.ms/arm-debug for usage details.",
 "details": [
         { "code": "Conflict",  
         "message": "{\r\n \"status\": \"Failed\",\r\n \"error\": {\r\n \"code\":
         \"ResourceDeploymentFailure\",\r\n \"message\": \"The resource
         operation completed with terminal provisioning state 'Failed'.\",\r\n
         \"details\": [\r\n {\r\n \"code\":
        \"VMExtensionProvisioningError\",\r\n \"message\": \"VM has
              reported a failure when processing extension 'dscextension'.
              Error message: \\\"DSC Configuration 'FirstSessionHost'
              completed with error(s). Following are the first few:
              PowerShell DSC resource MSFT ScriptResource failed to
              execute Set-TargetResource functionality with error message:
              One or more errors occurred. The SendConfigurationApply
              function did not succeed.\\\".\"\r\n }\r\n ]\r\n }\r\n}"  }

```

**Sebep:** PowerShell DSC uzantısı VM üzerinde yönetici erişimi elde etmek mümkün değildi.

**Düzeltme:** Sağlanan kullanıcı adını ve parolayı onaylayın, sanal makinede yönetim erişimine sahip olun ve Azure Kaynak Yöneticisi şablonuna yeniden çalıştırın.

### <a name="error-deploymentfailed--invalidresourcereference"></a>Hata: DeploymentFailed – GeçersizKaynak Başvurusu

Ham hata örneği:

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation
failed. Please list deployment operations for details. Please see https://aka.ms/arm-
debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\":
\"Failed\",\r\n \"error\": {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n
\"message\": \"The resource operation completed with terminal provisioning state
'Failed'.\",\r\n \"details\": [\r\n {\r\n \"code\": \"DeploymentFailed\",\r\n
\"message\": \"At least one resource deployment operation failed. Please list
deployment operations for details. Please see https://aka.ms/arm-debug for usage
details.\",\r\n \"details\": [\r\n {\r\n \"code\": \"BadRequest\",\r\n \"message\":
\"{\\r\\n \\\"error\\\": {\\r\\n \\\"code\\\": \\\"InvalidResourceReference\\\",\\r\\n
\\\"message\\\": \\\"Resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/virtualNetworks/wvd-vnet/subnets/default
referenced by resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/networkInterfaces/erd. Please make sure that
the referenced resource exists, and that both resources are in the same
region.\\\",\\r\\n\\\"details\\\": []\\r\\n }\\r\\n}\"\r\n }\r\n ]\r\n }\r\n ]\r\n }\r\n}"}]}
```

**Sebep:** Kaynak grubu adının bir bölümü şablon tarafından oluşturulan belirli kaynaklar için kullanılır. Varolan kaynakları eşleştiren ad nedeniyle, şablon farklı bir gruptan varolan bir kaynak seçebilir.

**Düzeltme:** Oturum ana bilgisayar VM'lerini dağıtmak için Azure Kaynak Yöneticisi şablonunu çalıştırırken, abonelik kaynak grup adınız için ilk iki karakteri benzersiz hale getirin.

### <a name="error-deploymentfailed--invalidresourcereference"></a>Hata: DeploymentFailed – GeçersizKaynak Başvurusu

Ham hata örneği:

```Error
{"code":"DeploymentFailed","message":"At least one resource deployment operation
failed. Please list deployment operations for details. Please see https://aka.ms/arm-
debug for usage details.","details":[{"code":"Conflict","message":"{\r\n \"status\":
\"Failed\",\r\n \"error\": {\r\n \"code\": \"ResourceDeploymentFailure\",\r\n
\"message\": \"The resource operation completed with terminal provisioning state
'Failed'.\",\r\n \"details\": [\r\n {\r\n \"code\": \"DeploymentFailed\",\r\n
\"message\": \"At least one resource deployment operation failed. Please list
deployment operations for details. Please see https://aka.ms/arm-debug for usage
details.\",\r\n \"details\": [\r\n {\r\n \"code\": \"BadRequest\",\r\n \"message\":
\"{\\r\\n \\\"error\\\": {\\r\\n \\\"code\\\": \\\"InvalidResourceReference\\\",\\r\\n
\\\"message\\\": \\\"Resource /subscriptions/EXAMPLE/resourceGroups/ernani-wvd-
demo/providers/Microsoft.Network/virtualNetworks/wvd-vnet/subnets/default
referenced by resource /subscriptions/EXAMPLE/resourceGroups/DEMO/providers/Microsoft.Network/networkInterfaces
/EXAMPLE was not found. Please make sure that the referenced resource exists, and that both
resources are in the same region.\\\",\\r\\n \\\"details\\\": []\\r\\n }\\r\\n}\"\r\n
}\r\n ]\r\n }\r\n ]\r\n }\r\n\
```

**Sebep:** Bu hata, Azure Kaynak Yöneticisi şablonuyla oluşturulan NIC'nin VNET'teki başka bir NIC ile aynı ada sahip olmasıdır.

**Düzeltme:** Farklı bir ana bilgisayar öneki kullanın.

### <a name="error-deploymentfailed--error-downloading"></a>Hata: DeploymentFailed – Hata indirme

Ham hata örneği:

```Error
\\\"The DSC Extension failed to execute: Error downloading
https://catalogartifact.azureedge.net/publicartifacts/rds.wvd-provision-host-pool-
2dec7a4d-006c-4cc0-965a-02bbe438d6ff-prod
/Artifacts/DSC/Configuration.zip after 29 attempts: The remote name could not be
resolved: 'catalogartifact.azureedge.net'.\\nMore information about the failure can
be found in the logs located under
'C:\\\\WindowsAzure\\\\Logs\\\\Plugins\\\\Microsoft.Powershell.DSC\\\\2.77.0.0' on
the VM.\\\"
```

**Sebep:** Bu hata, statik bir rota, güvenlik duvarı kuralı veya NSG'nin Azure Kaynak Yöneticisi şablonuna bağlı zip dosyasının karşıdan yüklenmesinin engellenmesinden kaynaklanır.

**Düzeltme:** Engelleme statik rotasını, güvenlik duvarı kuralını veya NSG'yi kaldırın. İsteğe bağlı olarak, metin düzenleyicisinde Azure Kaynak Yöneticisi şablonu json dosyasını açın, zip dosyasına bağlantıyı alın ve kaynağı izin verilen bir konuma indirin.

### <a name="error-the-user-isnt-authorized-to-query-the-management-service"></a>Hata: Kullanıcının yönetim hizmetini sorgulama yetkisi yok

Ham hata örneği:

```Error
"response": { "content": { "startTime": "2019-04-01T17:45:33.3454563+00:00", "endTime": "2019-04-01T17:48:52.4392099+00:00",
"status": "Failed", "error": { "code": "VMExtensionProvisioningError", "message": "VM has reported a failure when processing
extension 'dscextension'. Error message: \"DSC Configuration 'FirstSessionHost' completed with error(s).
Following are the first few: PowerShell DSC resource MSFT_ScriptResource failed to execute Set-TargetResource
 functionality with error message: User is not authorized to query the management service.
\nActivityId: 1b4f2b37-59e9-411e-9d95-4f7ccd481233\nPowershell commands to diagnose the failure:
\nGet-RdsDiagnosticActivities -ActivityId 1b4f2b37-59e9-411e-9d95-4f7ccd481233\n
The SendConfigurationApply function did not succeed.\"." }, "name": "2c3272ec-d25b-47e5-8d70-a7493e9dc473" } } }}
```

**Sebep:** Belirtilen Windows Sanal Masaüstü kiracı yöneticisinin geçerli bir rol ataması yoktur.

**Düzeltme:** Windows Sanal Masaüstü kiracısını oluşturan kullanıcının Windows Virtual Desktop PowerShell'de oturum açması ve denenen kullanıcıya bir rol ataması ataması gerekir. GitHub Azure Kaynak Yöneticisi şablon parametrelerini çalıştırıyorsanız, PowerShell komutlarını kullanarak aşağıdaki yönergeleri izleyin:

```PowerShell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
New-RdsRoleAssignment -TenantName <Windows Virtual Desktop tenant name> -RoleDefinitionName "RDS Contributor" -SignInName <UPN>
```

### <a name="error-user-requires-azure-multi-factor-authentication-mfa"></a>Hata: Kullanıcı Azure Çok Faktörlü Kimlik Doğrulaması (MFA) gerektirir

![Çok Faktörlü Kimlik Doğrulama (MFA) eksikliği nedeniyle dağıtımınızın ekran görüntüsü başarısız oldu](media/MFARequiredError.png)

Ham hata örneği:

```Error
"message": "{\r\n  \"status\": \"Failed\",\r\n  \"error\": {\r\n    \"code\": \"ResourceDeploymentFailure\",\r\n    \"message\": \"The resource operation completed with terminal provisioning state 'Failed'.\",\r\n    \"details\": [\r\n      {\r\n        \"code\": \"VMExtensionProvisioningError\",\r\n        \"message\": \"VM has reported a failure when processing extension 'dscextension'. Error message: \\\"DSC Configuration 'FirstSessionHost' completed with error(s). Following are the first few: PowerShell DSC resource MSFT_ScriptResource  failed to execute Set-TargetResource functionality with error message: One or more errors occurred.  The SendConfigurationApply function did not succeed.\\\".\"\r\n      }\r\n    ]\r\n  }\r\n}"
```

**Sebep:** Belirtilen Windows Sanal Masaüstü kiracı yöneticisi, Oturum açmanız için Azure Çok Faktörlü Kimlik Doğrulama (MFA) gerektirir.

**Düzeltme:** Bir hizmet sorumlusu oluşturun ve Öğretici' deki adımları izleyerek Windows Virtual Desktop kiracınız için bir rol [atayın: PowerShell ile hizmet ilkeleri ve rol atamaları oluşturun.](create-service-principal-role-powershell.md) Windows Sanal Masaüstü'nde hizmet sorumlusuyla oturum açabileceğinizi doğruladıktan sonra, kullandığınız yönteme bağlı olarak Azure Marketi teklifini veya GitHub Azure Kaynak Yöneticisi şablonuna yeniden göz atın. Yönteminizle ilgili doğru parametreleri girmek için aşağıdaki yönergeleri izleyin.

Azure Marketi teklifini çalıştırıyorsanız, Windows Sanal Masaüstü'nde doğru kimlik doğrulaması yapmak için aşağıdaki parametrelerin değerlerini sağlayın:

- Windows Sanal Masaüstü kiracı RDS Sahibi: Hizmet sorumlusu
- Başvuru Kimliği: Oluşturduğunuz yeni hizmet müdürünün başvuru kimliği
- Parola/Onayla Şifresi: Hizmet sorumlusu için oluşturduğunuz parola sırrı
- Azure AD Kiracı Kimliği: Oluşturduğunuz hizmet sorumlusunun Azure AD Kiracı Kimliği

GitHub Azure Kaynak Yöneticisi şablonu çalıştırıyorsanız, Windows Sanal Masaüstünde doğru kimlik doğrulaması yapmak için aşağıdaki parametrelerin değerlerini sağlayın:

- Kiracı Yönetici kullanıcı adı (UPN) veya Uygulama Kimliği: Oluşturduğunuz yeni hizmet yöneticisinin uygulama tanımlaması
- Kiracı Yönetici Şifresi: Hizmet sorumlusu için oluşturduğunuz parola sırrı
- IsServicePrincipal: **doğru**
- AadTenantId: Oluşturduğunuz hizmet sorumlusunun Azure REKLAM Kiracı Kimliği

## <a name="next-steps"></a>Sonraki adımlar

- Windows Sanal Masaüstü sorun giderme ve yükseltme parçalarına genel bakış için [Sorun Giderme genel bakışı, geri bildirim ve desteğe](troubleshoot-set-up-overview.md)bakın.
- Windows Sanal Masaüstü'nde sanal makine (VM) yapılandırırken sorunları gidermek için [Oturum ana bilgisayar sanal makine yapılandırmasına](troubleshoot-vm-configuration.md)bakın.
- Windows Sanal Masaüstü istemci bağlantılarıyla ilgili sorunları gidermek için [Windows Sanal Masaüstü hizmet bağlantılarına](troubleshoot-service-connection.md)bakın.
- Uzak Masaüstü istemcileriyle ilgili sorunları gidermek için Bkz. [Uzak Masaüstü istemcisi sorun giderme](troubleshoot-client.md)
- Windows Virtual Desktop ile PowerShell kullanırken sorunları gidermek için [Windows Virtual Desktop PowerShell'e](troubleshoot-powershell.md)bakın.
- Hizmet hakkında daha fazla bilgi edinmek için [Windows Sanal Masaüstü ortamına](environment-setup.md)bakın.
- Bir sorun giderme öğreticisine geçmek için [Bkz. Öğretici: Kaynak Yöneticisi şablonu dağıtımları.](../azure-resource-manager/templates/template-tutorial-troubleshoot.md)
- Denetim eylemleri hakkında bilgi edinmek için [Kaynak Yöneticisi ile Denetim işlemlerine](../azure-resource-manager/management/view-activity-logs.md)bakın.
- Dağıtım sırasında hataları belirlemek için eylemler hakkında bilgi edinmek için [bkz.](../azure-resource-manager/templates/deployment-history.md)
