---
title: Sorun giderme Azure İşlevleri Çalışma Zamanı ulaşılamaz.
description: Geçersiz bir depolama hesabında sorun gidermeyi öğrenin.
author: alexkarcher-msft
ms.topic: article
ms.date: 09/05/2018
ms.author: alkarche
ms.openlocfilehash: 910b582cb40b9f8aff6a553621b4677d6b019826
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2020
ms.locfileid: "76963895"
---
# <a name="how-to-troubleshoot-functions-runtime-is-unreachable"></a>"İşlevler çalışma zamanına ulaşılamıyor" sorununu giderme

Bu makale, Azure portal görüntülendiğinde "işlevler çalışma zamanına ulaşılamıyor" hata iletisiyle ilgili sorunları gidermeye yöneliktir. Bu hata oluştuğunda, portalda aşağıdaki hata dizesinin görüntülendiğini görürsünüz.

`Error: Azure Functions Runtime is unreachable. Click here for details on storage configuration`

Azure İşlevleri Çalışma Zamanı başlayamediğinde bu durum oluşur. Bu hatanın oluşma en yaygın nedeni, işlev uygulamasının depolama hesabına erişimi kaybetmesi durumunda meydana gelir. Daha fazla bilgi için bkz. [depolama hesabı gereksinimleri](storage-considerations.md#storage-account-requirements).

Bu makalenin geri kalanında, her bir durumu belirleme ve çözme dahil olmak üzere bu hatanın aşağıdaki nedenlerini gidermenize yardımcı olur.

+ [Depolama hesabı silindi](#storage-account-deleted)
+ [Depolama hesabı uygulama ayarları silindi](#storage-account-application-settings-deleted)
+ [Depolama hesabı kimlik bilgileri geçersiz](#storage-account-credentials-invalid)
+ [Depolama hesabına erişilemiyor](#storage-account-inaccessible)
+ [Günlük yürütme kotası aşıldı](#daily-execution-quota-full)
+ [Uygulamanız bir güvenlik duvarının arkasında](#app-is-behind-a-firewall)


## <a name="storage-account-deleted"></a>Depolama hesabı silindi

Her işlev uygulamasının çalışması için bir depolama hesabının olması gerekir. Bu hesap silinirse Işleviniz çalışmaz.

### <a name="how-to-find-your-storage-account"></a>Depolama hesabınızı bulma

Uygulama ayarlarınızda depolama hesabı adınızı arayarak başlayın. `AzureWebJobsStorage` veya `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING`, bir bağlantı dizesinde Sarmalanan depolama hesabınızın adını içerecektir. [Burada uygulama ayarı başvurusu](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#azurewebjobsstorage)' na daha fazla bilgi okuyun.

Hala mevcut olup olmadığını görmek için Azure portal depolama hesabınızı arayın. Silinmişse, bir depolama hesabı oluşturmanız ve depolama bağlantı Dizelerinizin yerine getirmeniz gerekir. İşlev kodunuz kaybolur ve yeniden dağıtmanız gerekir.

## <a name="storage-account-application-settings-deleted"></a>Depolama hesabı uygulama ayarları silindi

Önceki adımda, büyük olasılıkla silinmiş veya üzerine yazılmış bir depolama hesabı bağlantı dizeniz yoksa. Uygulama ayarlarını yapmak için dağıtım yuvaları veya Azure Resource Manager betikleri kullanılırken uygulama ayarlarının silinmesi genellikle yapılır.

### <a name="required-application-settings"></a>Gerekli uygulama ayarları

* Gereklidir
    * [`AzureWebJobsStorage`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#azurewebjobsstorage)
* Tüketim planı Işlevleri için gereklidir
    * [`WEBSITE_CONTENTAZUREFILECONNECTIONSTRING`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings)
    * [`WEBSITE_CONTENTSHARE`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings)

[Bu uygulama ayarları hakkında buradan bilgi edinin](https://docs.microsoft.com/azure/azure-functions/functions-app-settings).

### <a name="guidance"></a>Kılavuz

* Bu ayarlardan herhangi biri için "yuva ayarı" ' nı denetmayın. Dağıtım yuvalarını, işlev uygulaması molalarını takas ettiğinizde.
* Bu ayarları otomatik dağıtımların parçası olarak değiştirmeyin.
* Bu ayarlar, oluşturma sırasında sağlanmalı ve geçerli olmalıdır. Bu ayarları içermeyen bir otomatik dağıtım, ayarlar daha sonra eklense bile çalıştırmayacak bir işlev uygulamasına neden olur.

## <a name="storage-account-credentials-invalid"></a>Depolama hesabı kimlik bilgileri geçersiz

Depolama anahtarlarını yeniden oluşturursanız yukarıdaki depolama hesabı bağlantı dizelerinin güncellenmesi gerekir. [Depolama anahtarı yönetimi hakkında buradan daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

## <a name="storage-account-inaccessible"></a>Depolama hesabına erişilemiyor

İşlev uygulamanız depolama hesabına erişebilmelidir. Bir depolama hesabına erişim Işlevi engelleyen yaygın sorunlar şunlardır:

+ depolama hesabından gelen ve giden trafiğe izin vermek için doğru ağ kuralları olmadan App Service ortamlara dağıtılan işlev uygulamaları (Ao).

+ Depolama hesabı güvenlik duvarı etkinleştirilir ve işlevlere ve işlevlerine giden trafiğe izin verecek şekilde yapılandırılmamıştır. Daha fazla bilgi edinmek için bkz. [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](../storage/common/storage-network-security.md).

## <a name="daily-execution-quota-full"></a>Günlük yürütme kotası dolu

Günlük yürütme kotayı yapılandırdıysanız, işlev uygulamanız geçici olarak devre dışı bırakılır ve bu da Portal denetimlerinin çoğunun kullanılamaz hale gelmesine neden olur. 

+ [Azure Portal](https://portal.azure.com)doğrulamak için, Işlev uygulamanızda **Platform özellikleri** > **işlev uygulaması ayarları** ' nı açın. Ayarladığınız **günlük kullanım kotasının** üzerinde olduğunuzda aşağıdaki iletiyi görürsünüz:

    `The function app has reached daily usage quota and has been stopped until the next 24 hours time frame.`

+ Bu sorunu çözmek için günlük kotayı kaldırın veya artırın ve uygulamanızı yeniden başlatın. Aksi takdirde, uygulamanızın yürütülmesi bir sonraki güne kadar engellenir.

## <a name="app-is-behind-a-firewall"></a>Uygulama bir güvenlik duvarının arkasında

İşlev uygulamanız [dahili yük dengeli bir App Service ortamı](../app-service/environment/create-ilb-ase.md) barındırılıyorsa ve gelen internet trafiğini engelleyecek şekilde yapılandırıldıysa veya internet erişimini engelleyecek [gelen IP kısıtlamalarına](functions-networking-options.md#inbound-ip-restrictions) sahip olduğunda, işleviniz çalışma zamanına erişilemeyecektir. Azure portal, işlev listesini getirmek için çalışan uygulamaya doğrudan çağrı yapar ve ayrıca KUDU uç noktasına HTTP çağrıları yapar. `Platform Features` sekmesi altındaki platform düzeyi ayarları kullanılabilir olmaya devam edecektir.

Ao yapılandırmanızı doğrulamak için, Ao 'un bulunduğu alt ağın NSG 'ye gidin ve uygulamaya eriştiğiniz bilgisayarın genel IP 'sinden gelen trafiğe izin vermek için gelen kuralları doğrulayın. Portalı, uygulamanızı çalıştıran sanal ağa veya sanal ağınızda çalışan bir sanal makineye bağlı bir bilgisayardan da kullanabilirsiniz. [Gelen kuralı yapılandırması hakkında buradan daha fazla bilgi edinin](../app-service/environment/network-info.md#network-security-groups)

## <a name="next-steps"></a>Sonraki Adımlar

İşlev uygulamalarınızı izleme hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Azure Işlevlerini izleme](functions-monitoring.md)
