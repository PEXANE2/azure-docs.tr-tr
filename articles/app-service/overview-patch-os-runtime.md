---
title: İşletim sistemi ve çalışma zamanı yama cadence
description: Azure Uygulama Hizmeti'nin işletim sistemi ve çalışma saatlerini nasıl güncelleştirdiğini, uygulamalarınızın çalışma saatleri ve yama düzeyine sahip olduğunu ve güncelleme duyurularını nasıl alabileceğinizi öğrenin.
ms.topic: article
ms.date: 02/02/2018
ms.custom: seodec18
ms.openlocfilehash: 597964914f4022899ab027b735ec6932105497b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273632"
---
# <a name="os-and-runtime-patching-in-azure-app-service"></a>Azure Uygulama Hizmeti'nde işletim sistemi ve çalışma zamanı düzeltme

Bu makalede, [Uygulama Hizmeti'ndeki](overview.md)işletim sistemi veya yazılımla ilgili belirli sürüm bilgilerini nasıl elde edebilirsiniz gösterilmektedir. 

Uygulama Hizmeti, işletim sistemi ve uygulama yığınının sizin için Azure tarafından yönetildiği anlamına gelen bir Hizmet Olarak Platform'dur; yalnızca uygulamanızı ve verilerini yönetirsiniz. Os ve uygulama yığını üzerinde daha fazla denetim [Azure Sanal Makineler](https://docs.microsoft.com/azure/virtual-machines/)mevcuttur. Bunu göz önünde bulundurarak, yine de bir App Service kullanıcısı olarak aşağıdakiler gibi daha fazla bilgi edinmeniz yararlı olacaktır:

-   İşletim sistemi güncelleştirmeleri nasıl ve ne zaman uygulanır?
-   Uygulama Hizmeti önemli güvenlik açıklarına (sıfır gün gibi) karşı nasıl yamalı?
-   Uygulamalarınızı hangi işletim sistemi ve çalışma zamanı sürümleri çalıştırıyor?

Güvenlik nedenleriyle, güvenlik bilgilerinin belirli özellikleri yayımlanmamaktadır. Ancak makale, işlemdeki saydamlığı en üst düzeye çıkararak ve güvenlikle ilgili duyurular veya çalışma zamanı güncelleştirmeleri hakkında nasıl güncel kalabileceğinizi ortadan kaldırarak endişeleri hafifletmeyi amaçlamaktadır.

## <a name="how-and-when-are-os-updates-applied"></a>İşletim sistemi güncelleştirmeleri nasıl ve ne zaman uygulanır?

Azure, işletim sistemi yamalarını iki düzeyde yönetir: fiziksel sunucular ve Uygulama Hizmeti kaynaklarını çalıştıran konuk sanal makineler (VM'ler). Her ikisi de aylık olarak güncellenir ve bu da aylık [Patch Salı](https://technet.microsoft.com/security/bulletins.aspx) zamanlamasına göre hizalanır. Bu güncelleştirmeler, Azure hizmetlerinin yüksek kullanılabilirlik sla'sını garanti eden bir şekilde otomatik olarak uygulanır. 

Güncellemelerin nasıl uygulandığı hakkında ayrıntılı bilgi için, [App Service OS güncellemelerinin arkasındaki sihri demystifying](https://azure.github.io/AppService/2018/01/18/Demystifying-the-magic-behind-App-Service-OS-updates.html)bölümüne bakın.

## <a name="how-does-azure-deal-with-significant-vulnerabilities"></a>Azure önemli güvenlik açıklarıyla nasıl başa çıkıyor?

Ciddi güvenlik [açıkları, sıfır gün güvenlik açıkları](https://wikipedia.org/wiki/Zero-day_(computing))gibi anında yama gerektirdiğinde, yüksek öncelikli güncelleştirmeler duruma göre işlenir.

[Azure Güvenlik Blog'unu](https://azure.microsoft.com/blog/topics/security/)ziyaret ederek Azure'daki kritik güvenlik duyurularını güncel kalın. 

## <a name="when-are-supported-language-runtimes-updated-added-or-deprecated"></a>Desteklenen dil çalışma süreleri ne zaman güncellenir, eklenir veya küçümsenmez?

Desteklenen dil çalışma saatlerinin (büyük, küçük veya yama) yeni kararlı sürümleri, Uygulama Hizmeti örneklerine düzenli olarak eklenir. Bazı güncelleştirmeler varolan yüklemenin üzerine yazarken, diğerleri varolan sürümlerle yan yana yüklenir. Üzerine yazma yüklemesi, uygulamanızın güncelleştirilmiş çalışma zamanında otomatik olarak çalıştığı anlamına gelir. Yan yana yükleme, yeni bir çalışma zamanı sürümünden yararlanmak için uygulamanızı el ile geçirmeniz gerektiği anlamına gelir. Daha fazla bilgi için alt bölümlerden birine bakın.

Çalışma zamanı güncellemeleri ve amortismanlar burada duyurulur:

- https://azure.microsoft.com/updates/?product=app-service 
- https://github.com/Azure/app-service-announcements/issues

> [!NOTE] 
> Buradaki bilgiler, bir Uygulama Hizmeti uygulamasında yerleşik olan dil çalışma saatleri için geçerlidir. Örneğin, Uygulama Hizmeti'ne yüklediğiniz özel çalışma süresi, el ile yükseltmediğiniz sürece değişmeden kalır.
>
>

### <a name="new-patch-updates"></a>Yeni yama güncellemeleri

.NET, PHP, Java SDK veya Tomcat/İskele sürümündeki yama güncelleştirmeleri, yeni sürümle birlikte varolan yüklemenin üzerine yazılarak otomatik olarak uygulanır. Node.js yama güncelleştirmeleri varolan sürümlerle yan yana yüklenir (sonraki bölümdeki ana ve küçük sürümlere benzer). Yeni Python yama sürümleri site [uzantıları](https://azure.microsoft.com/blog/azure-web-sites-extensions/)aracılığıyla el ile yüklenebilir, dahili Python yüklemeleri ile yan yana.

### <a name="new-major-and-minor-versions"></a>Yeni büyük ve küçük versiyonlar

Yeni bir ana veya küçük sürüm eklendiğinde, varolan sürümlerle yan yana yüklenir. Uygulamanızı el ile yeni sürüme yükseltebilirsiniz. Çalışma zamanı sürümünü bir yapılandırma dosyasında yapılandırdıysanız (örneğin, `web.config` `package.json`aynı yöntemle yükseltmeniz gerekir). Çalışma zamanı sürümünüzü yapılandırmak için bir Uygulama Hizmeti ayarını kullandıysanız, azure [portalında](https://portal.azure.com) veya [Bulut Kabuğu'nda](../cloud-shell/overview.md)aşağıdaki örneklerde gösterildiği gibi bir [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) komutu çalıştırarak değiştirebilirsiniz:

```azurecli-interactive
az webapp config set --net-framework-version v4.7 --resource-group <groupname> --name <appname>
az webapp config set --php-version 7.0 --resource-group <groupname> --name <appname>
az webapp config appsettings set --settings WEBSITE_NODE_DEFAULT_VERSION=8.9.3 --resource-group <groupname> --name <appname>
az webapp config set --python-version 3.4 --resource-group <groupname> --name <appname>
az webapp config set --java-version 1.8 --java-container Tomcat --java-container-version 9.0 --resource-group <groupname> --name <appname>
```

### <a name="deprecated-versions"></a>Amortismana kılmış sürümler  

Eski bir sürüm amortismana girdiğinde, çalışma zamanı sürüm yükseltmenizi buna göre planlamak için kaldırma tarihi duyurulur. 

## <a name="how-can-i-query-os-and-runtime-update-status-on-my-instances"></a>Örneklerimde işletim sistemi ve çalışma zamanı güncelleştirme durumunu nasıl sorgulayabilirim?  

Kritik işletim sistemi bilgileri erişimden kilitlenirken [(Azure Uygulama Hizmeti'ndeki İşletim sistemi işlevine](operating-system-functionality.md)bakın), [Kudu konsolu,](https://github.com/projectkudu/kudu/wiki/Kudu-console) Işletim Sistemi sürümü ve çalışma zamanı sürümleriyle ilgili Uygulama Hizmeti örneğini sorgulamanızı sağlar. 

Aşağıdaki tablo, Windows sürümlerinin ve uygulamalarınızı çalıştıran dil çalışma zamanının nasıl yapılacağını gösterir:

| Bilgi | Nerede bulabilirim | 
|-|-|
| Windows sürümü | Bkz. `https://<appname>.scm.azurewebsites.net/Env.cshtml` (Sistem bilgisi altında) |
| .NET sürümü | At `https://<appname>.scm.azurewebsites.net/DebugConsole`, komut istemi aşağıdaki komutu çalıştırın: <br>`powershell -command "gci 'Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Net Framework Setup\NDP\CDF'"` |
| .NET Core sürümü | At `https://<appname>.scm.azurewebsites.net/DebugConsole`, komut istemi aşağıdaki komutu çalıştırın: <br> `dotnet --version` |
| PHP sürümü | At `https://<appname>.scm.azurewebsites.net/DebugConsole`, komut istemi aşağıdaki komutu çalıştırın: <br> `php --version` |
| Varsayılan Düğüm.js sürümü | Bulut [Kabuğu'nda](../cloud-shell/overview.md)aşağıdaki komutu çalıştırın: <br> `az webapp config appsettings list --resource-group <groupname> --name <appname> --query "[?name=='WEBSITE_NODE_DEFAULT_VERSION']"` |
| Python sürümü | At `https://<appname>.scm.azurewebsites.net/DebugConsole`, komut istemi aşağıdaki komutu çalıştırın: <br> `python --version` |  
| Java sürümü | At `https://<appname>.scm.azurewebsites.net/DebugConsole`, komut istemi aşağıdaki komutu çalıştırın: <br> `java -version` |  

> [!NOTE]  
> `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing\Packages` ["KB" yamaları](https://docs.microsoft.com/security-updates/SecurityBulletins/securitybulletins) üzerindeki bilgilerin depolandığı kayıt defteri konumuna erişim kilitlenir.
>
>

## <a name="more-resources"></a>Diğer kaynaklar

[Güven Merkezi: Güvenlik](https://www.microsoft.com/en-us/trustcenter/security)  
[Azure Uygulama Hizmetinde 64 bit ASP.NET Core](https://gist.github.com/glennc/e705cd85c9680d6a8f1bdb62099c7ac7)
