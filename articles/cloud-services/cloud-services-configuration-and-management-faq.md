---
title: Yapılandırma ve yönetim sorunları SSS
titleSuffix: Azure Cloud Services
description: Bu makalede, Microsoft Azure Bulut Hizmetleri için yapılandırma ve yönetim le ilgili sık sorulan sorular listelanmaktadır.
services: cloud-services
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/23/2018
ms.author: genli
ms.openlocfilehash: 40abd048b047bbece79b7c05d36a1fb189a4f28d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77656934"
---
# <a name="configuration-and-management-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Azure Bulut Hizmetleri için yapılandırma ve yönetim sorunları: Sık sorulan sorular (SSS)

Bu makalede, [Microsoft Azure Bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services)için yapılandırma ve yönetim sorunları hakkında sık sorulan sorular yer almaktadır. Boyut bilgileri için [Bulut Hizmetleri VM Boyut sayfasına](cloud-services-sizes-specs.md) da başvurabilirsiniz.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

**Sertifikalar**

- [Bulut Hizmeti SSL sertifikamın sertifika zinciri neden eksik?](#why-is-the-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete)
- ["Uzantılar için Windows Azure Araçları Şifreleme Sertifikası"nın amacı nedir?](#what-is-the-purpose-of-the-windows-azure-tools-encryption-certificate-for-extensions)
- [Örnekte "RDP-ing" olmadan nasıl sertifika imzalama isteği (CSR) oluşturabilirim?](#how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-to-the-instance)
- [Bulut Hizmeti Yönetimi Sertifikamın süresi doluyor. Nasıl yenilenir?](#my-cloud-service-management-certificate-is-expiring-how-to-renew-it)
- [Ana SSL sertifikası(.pfx) ve ara sertifika(.p7b) kurulumu nasıl otomatikleştirilir?](#how-to-automate-the-installation-of-main-ssl-certificatepfx-and-intermediate-certificatep7b)
- ["MachineKey için Microsoft Azure Hizmet Yönetimi" sertifikasının amacı nedir?](#what-is-the-purpose-of-the-microsoft-azure-service-management-for-machinekey-certificate)

**İzleme ve günlüğe kaydetme**

- [Azure portalında uygulamaların yönetilmesine ve izlenmesine yardımcı olabilecek yaklaşan Bulut Hizmeti özellikleri nelerdir?](#what-are-the-upcoming-cloud-service-capabilities-in-the-azure-portal-which-can-help-manage-and-monitor-applications)
- [IIS neden günlük dizinine yazmayı durdurur?](#why-does-iis-stop-writing-to-the-log-directory)
- [Bulut Hizmetleri için WAD günlüğe kaydetmeyi nasıl etkinleştirebilirim?](#how-do-i-enable-wad-logging-for-cloud-services)

**Ağ yapılandırması**

- [Azure yük bakiyesi için boşta zaman ödemesini nasıl ayarlıyorum?](#how-do-i-set-the-idle-timeout-for-azure-load-balancer)
- [Statik bir IP adresini Bulut Hizmetimle nasıl ilişkilendirebilirim?](#how-do-i-associate-a-static-ip-address-to-my-cloud-service)
- [Azure temel IPS/IDS ve DDOS'un sağladığı özellikler ve özellikler nelerdir?](#what-are-the-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides)
- [Bulut Hizmetleri VM'de HTTP/2 nasıl etkinleştirilir?](#how-to-enable-http2-on-cloud-services-vm)

**Izin**

- [Microsoft dahili mühendisleri masaüstünü Bulut Hizmeti örneklerine izinsiz uzak layabilir mi?](#can-microsoft-internal-engineers-remote-desktop-to-cloud-service-instances-without-permission)
- [RDP dosyasını kullanarak masaüstünü Cloud Service VM'ye uzaklatamam. Aşağıdaki hatayı alıyorum: Bir kimlik doğrulama hatası oluştu (Kod: 0x80004005)](#i-cannot-remote-desktop-to-cloud-service-vm--by-using-the-rdp-file-i-get-following-error-an-authentication-error-has-occurred-code-0x80004005)

**Ölçekleme**

- [X örneklerinin ötesine ölçeklendiremiyorum](#i-cannot-scale-beyond-x-instances)
- [Bellek ölçümlerine göre Otomatik Ölçeklendirmeyi nasıl yapılandırabilirim?](#how-can-i-configure-auto-scale-based-on-memory-metrics)

**Genel**

- [Web siteme nasıl "nosniff" eklerim?](#how-do-i-add-nosniff-to-my-website)
- [Bir web rolü için IIS'yi nasıl özelleştirebilirim?](#how-do-i-customize-iis-for-a-web-role)
- [Bulut Hizmetim için kota sınırı nedir?](#what-is-the-quota-limit-for-my-cloud-service)
- [Bulut Hizmeti VM'imdeki sürücü neden çok az boş disk alanı gösteriyor?](#why-does-the-drive-on-my-cloud-service-vm-show-very-little-free-disk-space)
- [Bulut Hizmetlerim için nasıl otomatik bir şekilde kötü amaçlı yazılımdan koruma uzantısı ekleyebilirim?](#how-can-i-add-an-antimalware-extension-for-my-cloud-services-in-an-automated-way)
- [Bulut Hizmetleri için Sunucu Adı Göstergesi (SNI) nasıl etkinleştirilir?](#how-to-enable-server-name-indication-sni-for-cloud-services)
- [Azure Bulut Hizmetime nasıl etiket ekleyebilirim?](#how-can-i-add-tags-to-my-azure-cloud-service)
- [Azure portalı Bulut Hizmetimin SDK sürümünü görüntülemez. Bunu nasıl alabilirim?](#the-azure-portal-doesnt-display-the-sdk-version-of-my-cloud-service-how-can-i-get-that)
- [Bulut Servisini birkaç aylığına kapatmak istiyorum. IP adresini kaybetmeden Bulut Hizmetinin faturalandırma maliyetini nasıl düşürür?](#i-want-to-shut-down-the-cloud-service-for-several-months-how-to-reduce-the-billing-cost-of-cloud-service-without-losing-the-ip-address)


## <a name="certificates"></a>Sertifikalar

### <a name="why-is-the-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete"></a>Bulut Hizmeti SSL sertifikamın sertifika zinciri neden eksik?
    
Müşterilerin sadece yaprak sertifikası yerine tam sertifika zincirini (yaprak cert, ara cert ve kök cert) yüklemelerini öneririz. Yalnızca yaprak sertifikasını yüklediğinizde, CTL'yi yürüyerek sertifika zincirini oluşturmak için Windows'a güvenirsiniz. Windows sertifikayı doğrulamaya çalışırken Azure veya Windows Update'te aralıklı ağ veya DNS sorunları oluşursa, sertifika geçersiz kabul edilebilir. Tam sertifika zinciri yüklenerek, bu sorun önlenebilir. [Zincirli SSL sertifikası nasıl yüklenir](https://blogs.msdn.microsoft.com/azuredevsupport/2010/02/24/how-to-install-a-chained-ssl-certificate/) blogu bunu nasıl yapacağımı gösterir.

### <a name="what-is-the-purpose-of-the-windows-azure-tools-encryption-certificate-for-extensions"></a>"Uzantılar için Windows Azure Araçları Şifreleme Sertifikası"nın amacı nedir?

Bulut Hizmetine bir uzantı eklendiğinde bu sertifikalar otomatik olarak oluşturulur. En yaygın olarak, bu WAD uzantısı veya RDP uzantısı, ancak antimalware veya Log Collector uzantısı gibi diğerleri olabilir. Bu sertifikalar yalnızca uzantıniçin özel yapılandırmayı şifrelemek ve çözmek için kullanılır. Son kullanma tarihi hiçbir zaman denetlenmez, bu nedenle sertifikanın süresinin dolması önemli değildir. 

Bu sertifikaları yoksayabilirsiniz. Sertifikaları temizlemek istiyorsanız, hepsini silmeyi deneyebilirsiniz. Kullanılmakta olan bir sertifikayı silmeye çalışırsanız Azure hata gösterir.

### <a name="how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-to-the-instance"></a>Örnekte "RDP-ing" olmadan nasıl sertifika imzalama isteği (CSR) oluşturabilirim?

Aşağıdaki kılavuz belgeye bakın:

[Windows Azure Web Siteleri (WAWS) ile kullanılmak üzere sertifika alma](https://azure.microsoft.com/blog/obtaining-a-certificate-for-use-with-windows-azure-web-sites-waws/)

KSS sadece bir metin dosyasıdır. Sertifikanın nihai olarak kullanılacağı makineden oluşturulması gerekmez.Bu belge bir Uygulama Hizmeti için yazılmış olsa da, CSR oluşturma geneldir ve Bulut Hizmetleri için de geçerlidir.

### <a name="my-cloud-service-management-certificate-is-expiring-how-to-renew-it"></a>Bulut Hizmeti Yönetimi Sertifikamın süresi doluyor. Nasıl yenilenir?

Yönetim Sertifikalarınızı yenilemek için aşağıdaki PowerShell komutlarını kullanabilirsiniz:

    Add-AzureAccount
    Select-AzureSubscription -Current -SubscriptionName <your subscription name>
    Get-AzurePublishSettingsFile

**Azure'u AlPublishSettingsFile,** Azure portalında **Abonelik** > **Yönetimi Sertifikalarında** yeni bir yönetim sertifikası oluşturur. Yeni sertifikanın adı "YourSubscriptionNam]-[CurrentDate]-credentials" gibi görünür.

### <a name="how-to-automate-the-installation-of-main-ssl-certificatepfx-and-intermediate-certificatep7b"></a>Ana SSL sertifikası(.pfx) ve ara sertifika(.p7b) kurulumu nasıl otomatikleştirilir?

Başlangıç komut dosyası (toplu iş/cmd/PowerShell) kullanarak bu görevi otomatikleştirebilir ve bu başlangıç komut dosyasını hizmet tanım dosyasına kaydedebilirsiniz. Başlangıç komut dosyasının aynı dizininin proje klasörüne hem başlangıç komut dosyasını hem de sertifikayı (.p7b dosyası) ekleyin.

### <a name="what-is-the-purpose-of-the-microsoft-azure-service-management-for-machinekey-certificate"></a>"MachineKey için Microsoft Azure Hizmet Yönetimi" sertifikasının amacı nedir?

Bu sertifika, Azure Web Rolleri'ndeki makine anahtarlarını şifrelemek için kullanılır. Daha fazla bilgi için [bu danışma belgesine](https://docs.microsoft.com/security-updates/securityadvisories/2018/4092731)göz atın.

Daha fazla bilgi için aşağıdaki makalelere bakın:
- [Bulut Hizmeti için başlangıç görevlerini yapılandırma ve çalıştırma](https://docs.microsoft.com/azure/cloud-services/cloud-services-startup-tasks)
- [Ortak Bulut Hizmeti başlatma görevleri](https://docs.microsoft.com/azure/cloud-services/cloud-services-startup-tasks-common)

## <a name="monitoring-and-logging"></a>İzleme ve günlüğe kaydetme

### <a name="what-are-the-upcoming-cloud-service-capabilities-in-the-azure-portal-which-can-help-manage-and-monitor-applications"></a>Azure portalında uygulamaların yönetilmesine ve izlenmesine yardımcı olabilecek yaklaşan Bulut Hizmeti özellikleri nelerdir?

Uzak Masaüstü Protokolü (RDP) için yeni bir sertifika oluşturma yeteneği yakında geliyor. Alternatif olarak, bu komut dosyalarını çalıştırabilirsiniz:

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My" -KeyLength 20 48 -KeySpec "KeyExchange"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```
Yeteneği csdef ve cscfg yükleme konumu için blob veya yerel seçmek için yakında geliyor. [Yeni-AzureDeployment'ı](/powershell/module/servicemanagement/azure/new-azuredeployment?view=azuresmps-4.0.0)kullanarak her konum değerini ayarlayabilirsiniz.

Örnek düzeyinde ölçümleri izleme yeteneği. [Bulut Hizmetlerinin İzlenmesi](cloud-services-how-to-monitor.md)için ek izleme özellikleri mevcuttur.

### <a name="why-does-iis-stop-writing-to-the-log-directory"></a>IIS neden günlük dizinine yazmayı durdurur?
Günlük dizinine yazmak için yerel depolama kotasını tüketmişsiniz.Bunu düzeltmek için üç şeyden birini yapabilirsiniz:
* IIS için tanılamayı etkinleştirin ve tanılamanın periyodik olarak blob depolamaya taşınmasını sağlar.
* Günlük dosyalarını günlük dizininden el ile kaldırın.
* Yerel kaynaklar için kota sınırını artırın.

Daha fazla bilgi için, aşağıdaki belgelere bakın:
* [Tanılama verilerini Azure Depolama'da depolama ve görüntüleme](/azure/storage/common/storage-introduction)
* [IIS Günlükleri Bulut Hizmeti'nde yazmayı durdurur](https://blogs.msdn.microsoft.com/cie/2013/12/21/iis-logs-stops-writing-in-cloud-service/)

### <a name="how-do-i-enable-wad-logging-for-cloud-services"></a>Bulut Hizmetleri için WAD günlüğe kaydetmeyi nasıl etkinleştirebilirim?
Aşağıdaki seçenekler aracılığıyla Windows Azure Tanılama (WAD) günlüğe kaydetmeyi etkinleştirebilirsiniz:
1. [Visual Studio'dan etkinleştirme](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#turn-on-diagnostics-in-cloud-service-projects-before-you-deploy-them)
2. [.NET kodu ile etkinleştirme](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics)
3. [Powershell ile etkinleştirme](https://docs.microsoft.com/azure/cloud-services/cloud-services-diagnostics-powershell)

Bulut Hizmetinizin geçerli WAD ayarlarını almak için [Get-AzureServiceDiagnosticsExtensions](https://docs.microsoft.com/azure/cloud-services/cloud-services-diagnostics-powershell#get-current-diagnostics-extension-configuration) ps cmd'yi kullanabilir veya portalı "Bulut Hizmetleri --> Uzantıları" bladeinden görüntüleyebilirsiniz.


## <a name="network-configuration"></a>Ağ yapılandırması

### <a name="how-do-i-set-the-idle-timeout-for-azure-load-balancer"></a>Azure yük bakiyesi için boşta zaman ödemesini nasıl ayarlıyorum?
Hizmet tanımı (csdef) dosyanızda zaman arasını şu şekilde belirtebilirsiniz:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="mgVS2015Worker" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10100"   idleTimeoutInMinutes="30" />
    </Endpoints>
  </WorkerRole>
```
Daha fazla bilgi [için Azure Yük Dengeleyicisi için Yeni: Yapılandırılabilir Boşta Zaman Acısı.](https://azure.microsoft.com/blog/new-configurable-idle-timeout-for-azure-load-balancer/)

### <a name="how-do-i-associate-a-static-ip-address-to-my-cloud-service"></a>Statik bir IP adresini Bulut Hizmetimle nasıl ilişkilendirebilirim?
Statik bir IP adresi ayarlamak için ayrılmış bir IP oluşturmanız gerekir. Bu ayrılmış IP, yeni bir Bulut Hizmeti yle veya varolan bir dağıtımla ilişkilendirilebilir. Ayrıntılar için aşağıdaki belgelere bakın:
* [Ayrılmış bir IP adresi oluşturma](/previous-versions/azure/virtual-network/virtual-networks-reserved-public-ip#manage-reserved-vips)
* [Varolan bir Bulut Hizmetinin IP adresini rezerve edin](/previous-versions/azure/virtual-network/virtual-networks-reserved-public-ip#reserve-the-ip-address-of-an-existing-cloud-service)
* [Ayrılmış bir IP'yi yeni bir Bulut Hizmetiyle ilişkilendirme](/previous-versions/azure/virtual-network/virtual-networks-reserved-public-ip#associate-a-reserved-ip-to-a-new-cloud-service)
* [Ayrılmış bir IP'yi çalışan bir dağıtımla ilişkilendirme](/previous-versions/azure/virtual-network/virtual-networks-reserved-public-ip#associate-a-reserved-ip-to-a-running-deployment)
* [Bir hizmet yapılandırma dosyası kullanarak ayrılmış bir IP'yi Bulut Hizmetiyle ilişkilendirme](/previous-versions/azure/virtual-network/virtual-networks-reserved-public-ip#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file)

### <a name="what-are-the-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides"></a>Azure temel IPS/IDS ve DDOS'un sağladığı özellikler ve özellikler nelerdir?
Azure'da tehditlere karşı savunmak için veri merkezi fiziksel sunucularında IPS/IDS vardır. Ayrıca, müşteriler web uygulaması güvenlik duvarları, ağ güvenlik duvarları, kötü amaçlı yazılımdan koruma, izinsiz giriş algılama, önleme sistemleri (IDS/IPS) ve daha fazlası gibi üçüncü taraf güvenlik çözümlerini dağıtabilir. Daha fazla bilgi için bkz: [Verilerinizi ve varlıklarınızı koruyun ve küresel güvenlik standartlarına uyun.](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity)

Microsoft, tehditleri algılamak için sunucuları, ağları ve uygulamaları sürekli olarak izler. Azure'un çok yönlü tehdit yönetimi yaklaşımı, savunmasını sürekli güçlendirmek için izinsiz giriş algılama, dağıtılmış hizmet reddi (DDoS) saldırı önleme, nüfuz testi, davranış analizi, anormallik algılama ve makine öğrenimini kullanır ve riskleri azaltmak. Azure için Microsoft Kötü Amaçlı Yazılımdan Koruma, Azure Bulut Hizmetlerini ve sanal makineleri korur. Web uygulaması yangın duvarları, ağ güvenlik duvarları, kötü amaçlı yazılımdan koruma, izinsiz giriş algılama ve önleme sistemleri (IDS/IPS) ve daha fazlası gibi üçüncü taraf güvenlik çözümlerini dağıtma seçeneğiniz vardır.

### <a name="how-to-enable-http2-on-cloud-services-vm"></a>Bulut Hizmetleri VM'de HTTP/2 nasıl etkinleştirilir?

Windows 10 ve Windows Server 2016, hem istemci hem de sunucu tarafında HTTP/2 desteğiyle birlikte gelir. İstemciniz (tarayıcınız) TLS uzantıları üzerinden HTTP/2'yi müzakere eden TLS üzerinden IIS sunucusuna bağlanıyorsa, sunucu tarafında herhangi bir değişiklik yapmanız gerekmez. Bunun nedeni, TLS üzerinde, HTTP/2 kullanımını belirten h2-14 üstbilgisinin varsayılan olarak gönderilmesidir. Öte yandan, istemciniz HTTP/2'ye yükseltmek için bir Yükseltme üstbilgisi gönderiyorsa, Yükseltme'nin çalıştığından ve bir HTTP/2 bağlantısıyla sonunuzu bulmak için sunucu tarafında aşağıdaki değişikliği yapmanız gerekir. 

1. Regedit.exe çalıştırın.
2. Kayıt defteri anahtarına göz atın: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HTTP\Parameters.
3. **DuoEnabled**adlı yeni bir DWORD değeri oluşturun.
4. Değerini 1 olarak ayarlayın.
5. Sunucunuzu yeniden başlatın.
6. **Varsayılan Web Sitenize** gidin ve **Ciltler**altında, yeni oluşturulan kendi imzalı sertifikayla yeni bir TLS bağlama oluşturun. 

Daha fazla bilgi için bkz.

- [HTTP/2 Üzerinde IIS](https://blogs.iis.net/davidso/http2)
- [Video: WINDOWS 10'da HTTP/2: Tarayıcı, Uygulamalar ve Web Sunucusu](https://channel9.msdn.com/Events/Build/2015/3-88)
         

Bu adımlar bir başlangıç görevi aracılığıyla otomatikleştirilebilir, böylece yeni bir PaaS örneği oluşturulduğunda, sistem kayıt defterinde yukarıdaki değişiklikleri yapabilir. Daha fazla bilgi için, [Bulut Hizmeti için başlangıç görevlerini nasıl yapılandırıp çalıştırılabilirsiniz.](cloud-services-startup-tasks.md)

 
Bu yapıldıktan sonra, aşağıdaki yöntemlerden birini kullanarak HTTP/2'nin etkinleştirilip etkinleştirilemediğini doğrulayabilirsiniz:

- IIS günlüklerinde Protokol sürümünü etkinleştirin ve IIS günlüklerine bakın. Günlüklerde HTTP/2 gösterecektir. 
- Internet Explorer veya Microsoft Edge'de F12 Geliştirici Aracı'nı etkinleştirin ve protokolü doğrulamak için Ağ sekmesine geçin. 

Daha fazla bilgi için [IIS'deki HTTP/2'ye](https://blogs.iis.net/davidso/http2)bakın.

## <a name="permissions"></a>İzinler

### <a name="how-can-i-implement-role-based-access-for-cloud-services"></a>Bulut Hizmetleri için rol tabanlı erişimi nasıl uygulayabilirim?
Bulut Hizmetleri, Azure Kaynak Yöneticisi tabanlı bir hizmet olmadığı için rol tabanlı erişim denetimi (RBAC) modelini desteklemez.

Bkz. [Azure'daki farklı rolleri anlayın.](../role-based-access-control/rbac-and-directory-admin-roles.md)

## <a name="remote-desktop"></a>Uzak masaüstü

### <a name="can-microsoft-internal-engineers-remote-desktop-to-cloud-service-instances-without-permission"></a>Microsoft dahili mühendisleri masaüstünü Bulut Hizmeti örneklerine izinsiz uzak layabilir mi?
Microsoft, dahili mühendislerin, sahibinden veya atadığı kişiden yazılı izin (e-posta veya diğer yazılı iletişim) olmadan masaüstünü Bulut Hizmetinize uzaklaştırmalarına izin vermeyecek sıkı bir işlem izler.

### <a name="i-cannot-remote-desktop-to-cloud-service-vm--by-using-the-rdp-file-i-get-following-error-an-authentication-error-has-occurred-code-0x80004005"></a>RDP dosyasını kullanarak masaüstünü Cloud Service VM'ye uzaklatamam. Aşağıdaki hatayı alıyorum: Bir kimlik doğrulama hatası oluştu (Kod: 0x80004005)

Bu hata, Azure Etkin Dizini'ne birleştirilmiş bir makineden RDP dosyasını kullanırsanız oluşabilir. Bu sorunu çözmek için şu adımları izleyin:

1. İndirdiğiniz RDP dosyasına sağ tıklayın ve ardından **Edit'i**seçin.
2. Kullanıcı adından önce önek olarak "&#92;" ekleyin. Örneğin, **kullanıcı adı**yerine **.\username** kullanın.

## <a name="scaling"></a>Ölçeklendirme

### <a name="i-cannot-scale-beyond-x-instances"></a>X örneklerinin ötesine ölçeklendiremiyorum
Azure Aboneliğinizin kullanabileceğiniz çekirdek sayısıyla ilgili bir sınırı vardır. Kullanılabilir tüm çekirdekleri kullandıysanız ölçekleme çalışmaz. Örneğin, 100 çekirdek sınırınız varsa, bu, Bulut Hizmetiniz için 100 A1 boyutunda sanal makine örneğine veya 50 A2 boyutunda sanal makine örneğine sahip olabileceğiniz anlamına gelir.

### <a name="how-can-i-configure-auto-scale-based-on-memory-metrics"></a>Bellek ölçümlerine göre Otomatik Ölçeklendirmeyi nasıl yapılandırabilirim?

Bulut Hizmetleri için Bellek ölçümlerini temel alan otomatik ölçek şu anda desteklenmez. 

Bu sorunu aşmak için Uygulama Öngörüleri'ni kullanabilirsiniz. Otomatik Ölçek, Uygulama Öngörülerini Bir Metrik Kaynağı olarak destekler ve "Bellek" gibi konuk ölçümüne göre rol örneği sayısını ölçeklendirebilir.  Bulut Hizmeti proje paketi dosyanızda (*.cspkg) Uygulama Öngörüleri yapılandırmanız ve bu başarıyı uygulamak için hizmetteki Azure Tanılama uzantısını etkinleştirmeniz gerekir.

Bulut Hizmetlerinde Otomatik Ölçeklendirmek için Uygulama Öngörüleri aracılığıyla özel bir ölçümü nasıl kullanabileceğimiz hakkında daha fazla bilgi için [bkz.](../azure-monitor/platform/autoscale-custom-metric.md)

Azure Tanılama'nın Bulut Hizmetleri için Uygulama Öngörüleri ile nasıl entegre edilenhakkında daha fazla bilgi için, [Bkz. Bulut Hizmeti Gönder, Sanal Makine veya Hizmet Dokusu tanı verileri Application Insights'a](../azure-monitor/platform/diagnostics-extension-to-application-insights.md)

Bulut Hizmetleri için Uygulama Öngörüleri'ni etkinleştirme hakkında daha fazla bilgi için [Azure Bulut Hizmetleri için Uygulama Öngörüleri'ne](https://docs.microsoft.com/azure/application-insights/app-insights-cloudservices) bakın

Bulut Hizmetleri için Azure Tanılama Günlüğü'ne nasıl etkinleştirileceksiniz hakkında daha fazla bilgi için [bkz.](/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#turn-on-diagnostics-in-cloud-service-projects-before-you-deploy-them)

## <a name="generic"></a>Genel

### <a name="how-do-i-add-nosniff-to-my-website"></a>Web siteme nasıl "nosniff" eklerim?
İstemcilerin MIME türlerini koklamasını önlemek için *web.config* dosyanıza bir ayar ekleyin.

```xml
<configuration>
   <system.webServer>
      <httpProtocol>
         <customHeaders>
            <add name="X-Content-Type-Options" value="nosniff" />
         </customHeaders>
      </httpProtocol>
   </system.webServer>
</configuration>
```

Bunu IIS'de ayar olarak da ekleyebilirsiniz. [Ortak başlangıç görevleri](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) makalesi ile aşağıdaki komutu kullanın.

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

### <a name="how-do-i-customize-iis-for-a-web-role"></a>Bir web rolü için IIS'yi nasıl özelleştirebilirim?
[Ortak başlangıç görevleri](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) makalesindeki IIS başlangıç komut dosyasını kullanın.

### <a name="what-is-the-quota-limit-for-my-cloud-service"></a>Bulut Hizmetim için kota sınırı nedir?
[Hizmete özel sınırlara](../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits)bakın.

### <a name="why-does-the-drive-on-my-cloud-service-vm-show-very-little-free-disk-space"></a>Bulut Hizmeti VM'imdeki sürücü neden çok az boş disk alanı gösteriyor?
Bu beklenen davranıştır ve uygulamanızda herhangi bir soruna neden olmamalıdır. Azure PaaS VM'lerde %approot% sürücüsü için günlük leme açılır ve bu da aslında dosyaların normalde kapladığı alanın iki katını tüketir. Ancak aslında olmayan bir sorun haline bu açmak farkında olmak için birkaç şey vardır.

%approot% sürücü boyutu ,cspkg + max günlük boyutu + boş alan> marjı veya 1,5 GB (hangisi daha büyükse) boyutu olarak \<hesaplanır. VM'nizin boyutu bu hesaplamayla hiçbir ilgisi yoktur. (VM boyutu yalnızca geçici C boyutunu etkiler: sürücü.) 

%approot% sürücüsüne yazmak desteklenmez. Azure VM'ye yazıyorsanız, bunu geçici bir LocalStorage kaynağında (veya Blob depolama, Azure Dosyaları vb. gibi başka bir seçenekte) yapmanız gerekir. Yani %approot% klasöründeki boş alan miktarı anlamlı değildir. Uygulamanızın %approot% sürücüsüne yazıp yazmadığından emin değilseniz, hizmetinizin her zaman birkaç gün çalışmasına izin verebilir ve ardından "önce" ve "sonra" boyutlarını karşılaştırabilirsiniz. 

Azure% approot% sürücüsüne hiçbir şey yazmaz. VHD .cspkg'inizden oluşturulduktan ve Azure VM'ye monte edilip azure VM'ye monte edilebilip bu sürücüye yazabilecek tek şey uygulamanızdır. 

Günlük ayarları yapılandırılamaz olduğundan kapatamazsınız.

### <a name="how-can-i-add-an-antimalware-extension-for-my-cloud-services-in-an-automated-way"></a>Bulut Hizmetlerim için nasıl otomatik bir şekilde kötü amaçlı yazılımdan koruma uzantısı ekleyebilirim?

Başlangıç Görevi'nde PowerShell komut dosyasını kullanarak Kötü Amaçlı Yazılımdan Koruma uzantısını etkinleştirebilirsiniz. Uygulamak için bu makalelerdeki adımları izleyin: 
 
- [PowerShell başlangıç görevi oluşturma](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task)
- [Set-AzureServiceAntimalwareExtension](https://docs.microsoft.com/powershell/module/servicemanagement/azure/Set-AzureServiceAntimalwareExtension?view=azuresmps-4.0.0 )

Kötü Amaçlı Yazılımdan Koruma Dağıtım Senaryoları ve portaldan nasıl etkinleştirilen hakkında daha fazla bilgi için, [Kötü Amaçlı Yazılımdan Koruma Dağıtım Senaryoları'na](../security/fundamentals/antimalware.md#antimalware-deployment-scenarios)bakın.

### <a name="how-to-enable-server-name-indication-sni-for-cloud-services"></a>Bulut Hizmetleri için Sunucu Adı Göstergesi (SNI) nasıl etkinleştirilir?

Bulut Hizmetlerinde SNI'yi aşağıdaki yöntemlerden birini kullanarak etkinleştirebilirsiniz:

**Yöntem 1: PowerShell kullanın**

SNI bağlama aşağıdaki gibi bir Bulut Hizmeti rol örneği için bir başlangıç görevinde PowerShell cmdlet **New-WebBinding** kullanılarak yapılandırılabilir:
    
    New-WebBinding -Name $WebsiteName -Protocol "https" -Port 443 -IPAddress $IPAddress -HostHeader $HostHeader -SslFlags $sslFlags 
    
[Burada](https://technet.microsoft.com/library/ee790567.aspx)açıklandığı gibi, $sslFlags aşağıdaki değerlerden biri olabilir:

|Değer|Anlamı|
------|------
|0|SNI yok|
|1|SNI Etkin|
|2|Merkezi Sertifika Deposu'nun kullandığı SNI olmayan bağlama|
|3|Merkezi Sertifika deposu kullanan SNI bağlama|
 
**Yöntem 2: Kodu kullanma**

SNI bağlama da bu [blog yazısı](https://blogs.msdn.microsoft.com/jianwu/2014/12/17/expose-ssl-service-to-multi-domains-from-the-same-cloud-service/)açıklandığı gibi rol başlangıç kodu ile yapılandırılabilir:

    
    //<code snip> 
                    var serverManager = new ServerManager(); 
                    var site = serverManager.Sites[0]; 
                    var binding = site.Bindings.Add(":443:www.test1.com", newCert.GetCertHash(), "My"); 
                    binding.SetAttributeValue("sslFlags", 1); //enables the SNI 
                    serverManager.CommitChanges(); 
    //</code snip> 
    
Yukarıdaki yaklaşımlardan herhangi birini kullanarak, Belirli ana bilgisayar adlarının ilgili sertifikalarının (*.pfx) sni bağlamanın etkili olabilmesi için önce bir başlangıç görevi kullanarak veya kod yoluyla rol örneklerine yüklenmesi gerekir.

### <a name="how-can-i-add-tags-to-my-azure-cloud-service"></a>Azure Bulut Hizmetime nasıl etiket ekleyebilirim? 

Bulut Hizmeti klasik bir kaynaktır. Yalnızca Azure Kaynak Yöneticisi destek etiketleri aracılığıyla oluşturulan kaynaklar. Etiketleri Bulut Hizmeti gibi Klasik kaynaklara uygulayamazsınız. 

### <a name="the-azure-portal-doesnt-display-the-sdk-version-of-my-cloud-service-how-can-i-get-that"></a>Azure portalı Bulut Hizmetimin SDK sürümünü görüntülemez. Bunu nasıl alabilirim?

Bu özelliği Azure portalına getirmek için çalışıyoruz. Bu arada, SDK sürümünü almak için aşağıdaki PowerShell komutlarını kullanabilirsiniz:

    Get-AzureService -ServiceName "<Cloud Service name>" | Get-AzureDeployment | Where-Object -Property SdkVersion -NE -Value "" | select ServiceName,SdkVersion,OSVersion,Slot

### <a name="i-want-to-shut-down-the-cloud-service-for-several-months-how-to-reduce-the-billing-cost-of-cloud-service-without-losing-the-ip-address"></a>Bulut Servisini birkaç aylığına kapatmak istiyorum. IP adresini kaybetmeden Bulut Hizmetinin faturalandırma maliyetini nasıl düşürür?

Zaten dağıtılmış bir Bulut Hizmeti, kullandığı İşlem ve Depolama için faturalandırılır. Bu nedenle, Azure VM'yi kapatsanız bile, Depolama alanı için faturalandırılırsınız. 

Hizmetinizin IP adresini kaybetmeden faturanızı azaltmak için şunları yapabilirsiniz:

1. Dağıtımları silmeden önce [IP adresini ayırın.](../virtual-network/virtual-networks-reserved-public-ip.md)  Yalnızca bu IP adresi için faturalandırılırsınız. IP adresi faturalandırması hakkında daha fazla bilgi için [IP adresleri fiyatlandırması'na](https://azure.microsoft.com/pricing/details/ip-addresses/)bakın.
2. Dağıtımları silin. Xxx.cloudapp.net silmeyin, böylece ileride kullanabilirsiniz.
3. Aboneliğinizde rezerve ettiğiniz aynı rezerv IP'yi kullanarak Bulut Hizmetini yeniden dağıtmak istiyorsanız, [Bulut Hizmetleri ve Sanal Makineler için Ayrılmış IP adreslerine](https://azure.microsoft.com/blog/reserved-ip-addresses/)bakın.

