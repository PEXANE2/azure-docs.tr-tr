---
title: 'Sorun giderme hatası: Azure İşlevler Runtime erişilemez'
description: Geçersiz bir depolama hesabının nasıl giderilmeye başlandığını öğrenin.
author: alexkarcher-msft
ms.topic: article
ms.date: 09/05/2018
ms.author: alkarche
ms.openlocfilehash: 8fcd0661e2c7cab505121cf0d4d7b4c1d29017f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063790"
---
# <a name="troubleshoot-error-azure-functions-runtime-is-unreachable"></a>Sorun giderme hatası: "Azure İşlevler Çalışma Süresi erişilmazdir"

Bu makale, Azure portalında görünen aşağıdaki hata dizesini gidermenize yardımcı olur:

> "Hata: Azure İşlevler Runtime erişilemez. Depolama yapılandırması ile ilgili ayrıntılar için buraya tıklayın."

Bu sorun, Azure İşlevler Çalıştırıladığında ortaya çıkar. Sorunun en yaygın nedeni, işlev uygulamasının depolama hesabına erişimini kaybetmiş olmasıdır. Daha fazla bilgi için [Bkz. Depolama hesabı gereksinimleri.](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements)

Bu makalenin geri kalanı, her servis talebinin nasıl tanımlandığı ve çözüleceği de dahil olmak üzere, bu hatanın aşağıdaki nedenlerini gidermenize yardımcı olur.

## <a name="storage-account-was-deleted"></a>Depolama hesabı silindi

Her işlev uygulamasının çalışması için bir depolama hesabı gerektirir. Bu hesap silinirse, işleviniz çalışmaz.

Uygulama ayarlarınızda depolama hesabı adınızı bakarak başlayın. Bağlantı `AzureWebJobsStorage` `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` dizesinde sarılmış depolama hesabınızın adını veya adını içerir. Daha fazla bilgi için [Azure İşlevler için Uygulama ayarları başvurusuna](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#azurewebjobsstorage)bakın.

Depolama hesabınızı azure portalında arayararak hala var olup olmadığını görün. Silinmişse, depolama hesabını yeniden oluşturun ve depolama bağlantı dizelerinizi değiştirin. İşlev kodunuz kayboldu ve yeniden dağıtmanız gerekiyor.

## <a name="storage-account-application-settings-were-deleted"></a>Depolama hesabı uygulama ayarları silindi

Önceki adımda, bir depolama hesabı bağlantı dizesini bulamıyorsanız, büyük olasılıkla silinmiş veya üzerine yazılmış. Uygulama ayarlarını silme işlemi en sık uygulama ayarlarını ayarlamak için dağıtım yuvalarını veya Azure Kaynak Yöneticisi komut dosyalarını kullanırken gerçekleşir.

### <a name="required-application-settings"></a>Gerekli uygulama ayarları

* Gerekli:
    * [`AzureWebJobsStorage`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings#azurewebjobsstorage)
* Tüketim planı işlevleri için gereklidir:
    * [`WEBSITE_CONTENTAZUREFILECONNECTIONSTRING`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings)
    * [`WEBSITE_CONTENTSHARE`](https://docs.microsoft.com/azure/azure-functions/functions-app-settings)

Daha fazla bilgi için [Azure İşlevler için Uygulama ayarları başvurusuna](https://docs.microsoft.com/azure/azure-functions/functions-app-settings)bakın.

### <a name="guidance"></a>Rehber

* Bu ayarların hiçbiri için "yuva ayarı"nı denetlemeyin. Dağıtım yuvalarını değiştirirseniz, işlev uygulaması kırılır.
* Otomatik dağıtımların bir parçası olarak bu ayarları değiştirmeyin.
* Bu ayarlar oluşturma zamanında sağlanmalı ve geçerli olmalıdır. Bu ayarları içermeyen otomatik dağıtım, ayarlar daha sonra eklense bile çalışmayan bir işlev uygulamasıyla sonuçlanır.

## <a name="storage-account-credentials-are-invalid"></a>Depolama hesabı kimlik bilgileri geçersizdir

Depolama anahtarlarını yeniden oluşturursanız, daha önce tartışılan depolama hesabı bağlantı dizeleri güncelleştirilmelidir. Depolama anahtarı yönetimi hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)

## <a name="storage-account-is-inaccessible"></a>Depolama hesabına erişilemiyor

İşlev uygulamanız depolama hesabına erişebilmeli. Bir işlev uygulamasının bir depolama hesabına erişimini engelleyen sık karşılaşılan sorunlar şunlardır:

* İşlev uygulaması, depolama hesabına ve depolama hesabından trafiğin girmesine izin vermek için doğru ağ kuralları olmadan App Service Environment'ınıza dağıtılır.

* Depolama hesabı güvenlik duvarı etkindir ve işlevlere gelen ve gelen trafiğe izin verecek şekilde yapılandırılmamıştır. Daha fazla bilgi için bkz. [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

## <a name="daily-execution-quota-is-full"></a>Günlük yürütme kotası dolu

Günlük yürütme kotası yapılandırıldıysa, işlev uygulamanız geçici olarak devre dışı bırakılır ve bu da portal denetimlerinin çoğunun kullanılamamasına neden olur. 

[Azure portalındaki](https://portal.azure.com)kotayı doğrulamak için işlev uygulamanızdaki **Platform Özellikleri** > **İşlev Uygulama Ayarları'nı** seçin. Belirlediğiniz **Günlük Kullanım Kotasını** aştıysanız, aşağıdaki ileti görüntülenir:

  > "İşlev Uygulaması günlük kullanım kotası ulaştı ve sonraki 24 saat zaman dilimine kadar durduruldu."

Bu sorunu gidermek için günlük kotayı kaldırın veya artırın ve ardından uygulamanızı yeniden başlatın. Aksi takdirde, uygulamanızın yürütülmesi ertesi güne kadar engellenir.

## <a name="app-is-behind-a-firewall"></a>Uygulama bir güvenlik duvarının arkasında

İşlev çalışma süreniz aşağıdaki nedenlerden herhangi biri için erişilemez olabilir:

* İşlev uygulamanız [dahili olarak yüklü](../app-service/environment/create-ilb-ase.md) bir Uygulama Hizmeti Ortamında barındırılır ve gelen internet trafiğini engelleyecek şekilde yapılandırılır.

* İşlev uygulamanız, internet erişimini engellemek için yapılandırılan [gelen IP kısıtlamalarına](functions-networking-options.md#inbound-ip-restrictions) sahiptir. 

Azure portalı, işlevlerin listesini almak için doğrudan çalışan uygulamaya arama yapar ve HTTP çağrılarını Kudu bitiş noktasına yapar. **Platform Özellikleri** sekmesi altındaki platform düzeyinde ayarlar hala kullanılabilir.

Uygulama Hizmeti Ortamı yapılandırmanızı doğrulamak için:
1. Uygulama Hizmet Ortamının bulunduğu alt ağın ağ güvenlik grubuna (NSG) gidin.
1. Uygulamaya eriştiğiniz bilgisayarın genel IP'sinden gelen trafiğe izin vermek için gelen kuralları doğrulayın. 
   
Portalı, uygulamanızı çalıştıran sanal ağa veya sanal ağınızda çalışan sanal bir makineye bağlı bir bilgisayardan da kullanabilirsiniz. 

Gelen kural yapılandırması hakkında daha fazla bilgi için, [Bir Uygulama Hizmeti Ortamı için Ağ konularının](https://docs.microsoft.com/azure/app-service/environment/network-info#network-security-groups)"Ağ Güvenlik Grupları" bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

İşlev uygulamalarınızı izleme hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Azure İşlevlerini İzleme](functions-monitoring.md)
