---
title: 'Sorun giderme hatası: Azure İşlevleri Çalışma Zamanı ulaşılamıyor'
description: Geçersiz bir depolama hesabında sorun gidermeyi öğrenin.
author: alexkarcher-msft
ms.topic: article
ms.date: 09/05/2018
ms.author: alkarche
ms.openlocfilehash: 8fcd0661e2c7cab505121cf0d4d7b4c1d29017f8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77063790"
---
# <a name="troubleshoot-error-azure-functions-runtime-is-unreachable"></a>Sorun giderme hatası: "Azure İşlevleri Çalışma Zamanı erişilebilir değil"

Bu makale, Azure portal görüntülenen aşağıdaki hata dizesinin sorunlarını gidermenize yardımcı olur:

> "Hata: Azure İşlevleri Çalışma Zamanı ulaşılamaz. Depolama yapılandırmasıyla ilgili ayrıntılar için buraya tıklayın. "

Azure İşlevleri Çalışma Zamanı başlayamediğinde bu sorun oluşur. Sorunun en yaygın nedeni, işlev uygulamasının depolama hesabına erişiminin kaybedilme nedenidir. Daha fazla bilgi için bkz. [depolama hesabı gereksinimleri](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).

Bu makalenin geri kalanında, her bir durumu belirleme ve çözme dahil olmak üzere bu hatanın aşağıdaki nedenlerini gidermenize yardımcı olur.

## <a name="storage-account-was-deleted"></a>Depolama hesabı silindi

Her işlev uygulamasının çalışması için bir depolama hesabının olması gerekir. Bu hesap silinirse, işleviniz çalışmaz.

Uygulama ayarlarınızda depolama hesabı adınızı arayarak başlayın. `AzureWebJobsStorage`Ya da `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` bir bağlantı dizesinde sarmalanmış depolama hesabınızın adını içerir. Daha fazla bilgi için bkz. [Azure işlevleri Için uygulama ayarları başvurusu](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#azurewebjobsstorage).

Hala mevcut olup olmadığını görmek için Azure portal depolama hesabınızı arayın. Silinmişse, depolama hesabını yeniden oluşturun ve depolama bağlantı dizelerinizi değiştirin. İşlev kodunuz kaybolur ve yeniden dağıtmanız gerekir.

## <a name="storage-account-application-settings-were-deleted"></a>Depolama hesabı uygulama ayarları silindi

Önceki adımda, bir depolama hesabı bağlantı dizesi bulamazsanız muhtemelen silinmiş veya üzerine yazılmıştı. Uygulama ayarlarının silinmesi, dağıtım yuvaları veya Azure Resource Manager betikleri kullanırken uygulama ayarlarını ayarlamak için genellikle gerçekleşir.

### <a name="required-application-settings"></a>Gerekli uygulama ayarları

* Gerekli:
    * [`AzureWebJobsStorage`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#azurewebjobsstorage)
* Tüketim planı işlevleri için gereklidir:
    * [`WEBSITE_CONTENTAZUREFILECONNECTIONSTRING`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings)
    * [`WEBSITE_CONTENTSHARE`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings)

Daha fazla bilgi için bkz. [Azure işlevleri Için uygulama ayarları başvurusu](https://docs.microsoft.com/azure/azure-functions/functions-app-settings).

### <a name="guidance"></a>Rehber

* Bu ayarlardan herhangi biri için "yuva ayarı" ' nı denetmayın. Dağıtım yuvalarını takas ederseniz, işlev uygulaması kesilir.
* Bu ayarları otomatik dağıtımların parçası olarak değiştirmeyin.
* Bu ayarlar, oluşturma sırasında sağlanmalı ve geçerli olmalıdır. Bu ayarları içermeyen bir otomatik dağıtım, ayarlar daha sonra eklense bile çalıştırmayacak bir işlev uygulamasına neden olur.

## <a name="storage-account-credentials-are-invalid"></a>Depolama hesabı kimlik bilgileri geçersiz

Depolama anahtarlarını yeniden oluşturursanız, daha önce tartışılan depolama hesabı bağlantı dizeleri güncelleştirilmeleri gerekir. Depolama anahtarı yönetimi hakkında daha fazla bilgi için bkz. [Azure Storage hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

## <a name="storage-account-is-inaccessible"></a>Depolama hesabına erişilemiyor

İşlev uygulamanız depolama hesabına erişebilmelidir. Bir işlev uygulamasının bir depolama hesabına erişimini engelleyen yaygın sorunlar şunlardır:

* İşlev uygulaması, depolama hesabından gelen ve giden trafiğe izin vermek için doğru ağ kuralları olmadan App Service Ortamı dağıtılır.

* Depolama hesabı güvenlik duvarı etkinleştirilir ve işlevlere ve işlevlerine giden trafiğe izin verecek şekilde yapılandırılmamıştır. Daha fazla bilgi için bkz. [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

## <a name="daily-execution-quota-is-full"></a>Günlük yürütme kotası dolu

Günlük yürütme kotayı yapılandırdıysanız, işlev uygulamanız geçici olarak devre dışı bırakılır ve bu da Portal denetimlerinin çoğunun kullanılamaz hale gelmesine neden olur. 

[Azure Portal](https://portal.azure.com)kotayı doğrulamak için, işlev uygulamanızda **platform özellikleri**  >  **işlev uygulaması ayarları** ' nı seçin. Ayarladığınız **günlük kullanım kotasının** üzerinde çalışıyorsanız, aşağıdaki ileti görüntülenir:

  > "İşlev Uygulaması günlük kullanım kotasına ulaştı ve sonraki 24 saatlik zaman dilimine kadar durduruldu."

Bu sorunu çözmek için günlük kotasını kaldırın veya arttırın ve sonra uygulamanızı yeniden başlatın. Aksi takdirde, uygulamanızın yürütülmesi bir sonraki güne kadar engellenir.

## <a name="app-is-behind-a-firewall"></a>Uygulama bir güvenlik duvarının arkasında

İşlev çalışma zamanı, aşağıdaki nedenlerden biri için ulaşılamaz olabilir:

* İşlev uygulamanız, [dahili olarak yük dengeli bir App Service ortamı](../app-service/environment/create-ilb-ase.md) barındırılır ve gelen internet trafiğini engelleyecek şekilde yapılandırılmıştır.

* İşlev uygulamanızda internet erişimini engelleyecek şekilde yapılandırılmış [gelen IP kısıtlamaları](functions-networking-options.md#inbound-ip-restrictions) vardır. 

Azure portal işlevler listesini getirmek için doğrudan çalışan uygulamaya çağrı yapar ve kudu uç noktasına HTTP çağrıları yapar. **Platform özellikleri** sekmesi altındaki platform düzeyi ayarları hala kullanılabilir.

App Service Ortamı yapılandırmanızı doğrulamak için:
1. App Service Ortamı bulunduğu alt ağın ağ güvenlik grubu 'na (NSG) gidin.
1. Uygulamaya eriştiğiniz bilgisayarın genel IP 'sinden gelen trafiğe izin vermek için gelen kurallarını doğrulayın. 
   
Portalı, uygulamanızı çalıştıran sanal ağa veya sanal ağınızda çalışan bir sanal makineye bağlı bir bilgisayardan da kullanabilirsiniz. 

Gelen kuralı yapılandırması hakkında daha fazla bilgi için, [bir App Service ortamı Için ağ oluşturma konuları](https://docs.microsoft.com/azure/app-service/environment/network-info#network-security-groups)"ağ güvenlik grupları" bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

İşlev uygulamalarınızı izleme hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Azure İşlevlerini İzleme](functions-monitoring.md)
