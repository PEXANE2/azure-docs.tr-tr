---
title: Azure günlük tümleştirmesi hakkında SSS | Microsoft Docs
description: Bu makalede Azure günlük tümleştirmesi hakkında sorular yanıtlanmaktadır.
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: d06d1ac5-5c3b-49de-800e-4d54b3064c64
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload8: na
ms.date: 05/28/2019
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 7ed63364a9dc4b96470a23af3a4bff832d42621b
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727618"
---
# <a name="azure-log-integration-faq"></a>Azure günlük tümleştirmesi hakkında SSS

Bu makalede, Azure günlük tümleştirmesi hakkında sık sorulan sorular (SSS) yanıtlanmaktadır.

>[!IMPORTANT]
> Azure günlük tümleştirme özelliği 06/15/2019 tarafından kullanım dışı bırakılacak. AzLog İndirmeleri 27 Haz 2018 ' de devre dışı bırakıldı. İleriye doğru iletme hakkında yönergeler için gönderiyi [SıEM araçlarıyla tümleştirme Için Azure Izleyicisini kullanma](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

Azure günlük tümleştirmesi, Azure kaynaklarınızdan ham günlükleri şirket içi güvenlik bilgileri ve olay yönetimi (SıEM) sistemlerinizle tümleştirmek için kullanabileceğiniz bir Windows işletim sistemi hizmetidir. Bu tümleştirme, tüm varlıklarınızda, şirket içinde veya bulutta birleştirilmiş bir pano sağlar. Daha sonra, uygulamalarınızla ilişkili güvenlik olaylarını toplayabilir, ilişkilendirebilir, çözümleyebilir ve uyarabilir.

Azure günlüklerini tümleştirmek için tercih edilen yöntem, SıEM satıcınızın Azure Izleyici bağlayıcısını kullanarak ve bu [yönergeleri](/azure/azure-monitor/platform/stream-monitoring-data-event-hubs)izleyerek kullanmaktır. Ancak, SıEM satıcınız Azure Izleyici 'ye bağlayıcı sağlamıyorsa, bu tür bir bağlayıcı kullanılabilir olana kadar Azure günlük tümleştirmesini geçici bir çözüm (Azure günlük tümleştirmesi tarafından destekleniyorsa) kullanabilirsiniz.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="is-the-azure-log-integration-software-free"></a>Azure günlük tümleştirme yazılımı ücretsizdir mi?

Evet. Azure günlük tümleştirme yazılımı için ücret alınmaz.

## <a name="where-is-azure-log-integration-available"></a>Azure günlük tümleştirmesi nerede kullanılabilir?

Azure ticari ve Azure Kamu 'da Şu anda kullanılabilir ve Çin 'de veya Almanya 'da kullanılamaz.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs"></a>Azure günlük tümleştirmesinin Azure VM günlüklerini çekmesini sağlayan depolama hesaplarını nasıl görebilirim?

**Azlog kaynak listesi**komutunu çalıştırın.

## <a name="how-can-i-tell-which-subscription-the-azure-log-integration-logs-are-from"></a>Azure günlük tümleştirme günlüklerinin hangi abonelikte olduğunu nasıl anlayabilirim?

**AzureResourcemanagerJson** dizinlerinde yer alan denetim günlükleri söz konusu olduğunda, abonelik kimliği günlük dosyası adıdır. Bu, **AzureSecurityCenterJson** klasöründeki Günlükler için de geçerlidir. Örneğin:

20170407T070805_2768037.0000000023.**1111e5ee-1111-111b-a11e-1e111e1111dc**.json

Azure Active Directory Denetim günlükleri, adın parçası olarak kiracı KIMLIĞINI içerir.

Bir olay hub 'ından okunan tanılama günlükleri, abonelik KIMLIĞINI adının bir parçası olarak içermez. Bunun yerine, Olay Hub 'ı kaynağı oluşturmanın bir parçası olarak belirtilen kolay adı içerirler. 

## <a name="how-can-i-update-the-proxy-configuration"></a>Ara sunucu yapılandırmasını nasıl güncelleştirebilirim?

Ara sunucu ayarınız doğrudan Azure Storage erişimine izin vermediğinden Azlog ' i açın **. EXE.**  **C:\Program Files\Microsoft Azure günlük TÜMLEŞTIRMESINDE**yapılandırma dosyası. Dosyayı, kuruluşunuzun proxy adresine sahip **defaultProxy** bölümünü içerecek şekilde güncelleştirin. Güncelleştirme yapıldıktan sonra, **net stop azlog** ve **net start azlog**komutlarını kullanarak hizmeti durdurup başlatın.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress="http://127.0.0.1:8888"
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Windows olaylarında abonelik bilgilerini nasıl görebilirim?

Kaynağı eklerken kolay ada abonelik KIMLIĞI ekleyin:

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  
Olay XML 'SI, abonelik KIMLIĞI de dahil olmak üzere aşağıdaki meta verilere sahiptir:

![Event XML](media/azure-log-integration-faq/event-xml.png)

## <a name="error-messages"></a>Hata iletileri
### <a name="when-i-run-the-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Komutu ```AzLog createazureid```çalıştırdığımda, neden şu hatayı alıyorum?

Hata:

  *AAD uygulaması oluşturulamadı-kiracı 72f988bf-86f1-41af-91ab-2d7cd011db37-Reason = ' yasak '-Message = ' işlemi tamamlamaya yönelik ayrıcalıklar yetersiz. '*

**Azlog createazureid** komutu, Azure oturum açmanın erişebileceği abonelikler Için tüm Azure AD kiracılarında bir hizmet sorumlusu oluşturmaya çalışır. Azure oturum açma, bu Azure AD kiracısında yalnızca bir Konuk Kullanıcı ise, komut "işlemi tamamlamaya yönelik ayrıcalıklar yetersiz" ile başarısız olur. Kiracı yöneticisinden hesabınızı kiracıya bir kullanıcı olarak eklemesini isteyin.

### <a name="when-i-run-the-command-azlog-authorize-why-do-i-get-the-following-error"></a>**Azlog Yetkilendir**komutunu çalıştırdığımda, neden şu hatayı alıyorum?

Hata:

  *Rol ataması oluşturma hatası-AuthorizationFailed: ' Fe9e03e4-4dad-4328-910f-fd24a9660bd2 ' nesne kimliğine\@sahip istemci epedo Microsoft.com ', '/Subscriptions/' kapsamı üzerinde ' Microsoft. Authorization/roleatamalar/Write ' eylemini gerçekleştirme yetkisine sahip değil 70d95299-d689-4c97-b971-0d8ff0000000 '.*

**Azlog Yetkilendir** komutu, okuyucunun rolünü Azure AD hizmet sorumlusu 'na ( **azlog createazureid**ile oluşturulur) belirtilen aboneliklerle atar. Azure oturumu bir ortak yönetici veya aboneliğin sahibi değilse, "yetkilendirme başarısız oldu" hata iletisiyle başarısız olur. Bu eylemi gerçekleştirmek için ortak yönetici veya sahip Azure rol tabanlı Access Control (RBAC) gereklidir.

## <a name="where-can-i-find-the-definition-of-the-properties-in-the-audit-log"></a>Denetim günlüğündeki özelliklerin tanımını nerede bulabilirim?

Bkz.

* [Azure Resource Manager ile denetim işlemleri](/azure/azure-resource-manager/resource-group-audit)
* [Yönetim olaylarını Azure Izleyici 'deki bir abonelikte listeleyin REST API](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Azure Güvenlik Merkezi uyarıları hakkındaki ayrıntıları nereden bulabilirim?

Bkz. [Azure Güvenlik Merkezi 'nde güvenlik uyarılarını yönetme ve yanıtlama](/azure/security-center/security-center-managing-and-responding-alerts).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>VM Tanılama ile toplandığını nasıl değiştirebilirim?

Azure Tanılama yapılandırmasını alma, değiştirme ve ayarlama hakkında daha fazla bilgi için bkz. [Windows çalıştıran bir sanal makinede Azure tanılama etkinleştirmek Için PowerShell kullanma](/azure/virtual-machines/windows/ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Aşağıdaki örnek Azure Tanılama yapılandırmasını alır:

    Get-AzVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

Aşağıdaki örnek Azure Tanılama yapılandırmasını değiştirir. Bu yapılandırmada, güvenlik olay günlüğünden yalnızca olay KIMLIĞI 4624 ve olay KIMLIĞI 4625 toplanır. Azure olayları için Microsoft kötü amaçlı yazılımdan koruma, sistem olay günlüğünden toplanır. XPath ifadelerinin kullanımı hakkında ayrıntılı bilgi için bkz. [olayları](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85))kullanma.

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

Aşağıdaki örnek Azure Tanılama yapılandırmasını ayarlar:

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Değişiklik yaptıktan sonra, doğru olayların toplandığından emin olmak için depolama hesabını kontrol edin.

Yükleme ve yapılandırma sırasında herhangi bir sorun yaşıyorsanız lütfen bir [destek isteği](/azure/azure-supportability/how-to-create-azure-support-request)açın. Destek istediğiniz hizmet olarak **günlük tümleştirmesini** seçin.

## <a name="can-i-use-azure-log-integration-to-integrate-network-watcher-logs-into-my-siem"></a>Ağ Izleyicisi günlüklerini SıEM 'imde tümleştirmek için Azure günlük tümleştirmesini kullanabilir miyim?

Azure ağ Izleyicisi, büyük miktarlarda günlük bilgileri üretir. Bu günlüklerin bir SıEM 'ye gönderilmesi amaçlanır. Ağ Izleyicisi günlükleri için desteklenen tek hedef, bir depolama hesabıdır. Azure günlük tümleştirmesi, bu günlükleri okumayı ve bunları SıEM tarafından kullanılabilir hale getirmeyi desteklemez.


