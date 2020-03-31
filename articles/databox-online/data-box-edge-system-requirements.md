---
title: Microsoft Azure Veri Kutusu Kenarı sistem gereksinimleri| Microsoft Dokümanlar
description: Azure Veri Kutusu Edge'iniz için yazılım ve ağ gereksinimleri hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/03/2019
ms.author: alkohli
ms.openlocfilehash: 458c062eef011363724cb894ce67ba75181ba8ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260234"
---
# <a name="azure-data-box-edge-system-requirements"></a>Azure Veri Kutusu Kenarı sistem gereksinimleri

Bu makalede, Microsoft Azure Veri Kutusu Kenarı çözümünüz ve Azure Veri Kutusu Kenarı'na bağlanan istemciler için önemli sistem gereksinimleri açıklanmaktadır. Veri Kutusu Kenarınızı dağıtmadan önce bilgileri dikkatle incelemenizi öneririz. Dağıtım ve sonraki işlem sırasında bu bilgilere gerektiği gibi geri başvurabilirsiniz.

Veri Kutusu Kenarı için sistem gereksinimleri şunlardır:

- **Ana bilgisayarlar için yazılım gereksinimleri** - desteklenen platformları, yerel yapılandırma ui tarayıcılarını, SMB istemcilerini ve aygıta erişen istemciler için ek gereksinimleri açıklar.
- **Aygıt için ağ gereksinimleri** - fiziksel aygıtın çalışması için herhangi bir ağ gereksinimleri hakkında bilgi sağlar.

## <a name="supported-os-for-clients-connected-to-device"></a>Aygıta bağlı istemciler için desteklenen işletim sistemi

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-supported-client-os.md)]

## <a name="supported-protocols-for-clients-accessing-device"></a>Aygıta erişen istemciler için desteklenen protokoller

[!INCLUDE [Supported protocols for clients accessing device](../../includes/data-box-edge-gateway-supported-client-protocols.md)]

## <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

[!INCLUDE [Supported storage accounts](../../includes/data-box-edge-gateway-supported-storage-accounts.md)]

## <a name="supported-storage-types"></a>Desteklenen depolama türleri

[!INCLUDE [Supported storage types](../../includes/data-box-edge-gateway-supported-storage-types.md)]

## <a name="supported-browsers-for-local-web-ui"></a>Yerel web ui için desteklenen tarayıcılar

[!INCLUDE [Supported browsers for local web UI](../../includes/data-box-edge-gateway-supported-browsers.md)]

## <a name="networking-port-requirements"></a>Ağ bağlantı noktası gereksinimleri

### <a name="port-requirements-for-data-box-edge"></a>Veri Kutusu Kenarı için bağlantı noktası gereksinimleri

Aşağıdaki tabloda, SMB, bulut veya yönetim trafiğine izin vermek için güvenlik duvarınızda açılması gereken bağlantı noktaları listelenir. Bu tabloda, *gelen* *istemcinin* cihazınıza erişmelerini talep ettiği yönü ifade eder. *Giden* veya *giden,* Veri Kutusu Edge cihazınızın verileri dışarıdan, dağıtımın ötesinde (örneğin, internete giden) gönderdiği yönü ifade eder.

[!INCLUDE [Port configuration for device](../../includes/data-box-edge-gateway-port-config.md)]

### <a name="port-requirements-for-iot-edge"></a>IoT Edge için bağlantı noktası gereksinimleri

Azure IoT Edge, desteklenen IoT Hub protokollerini kullanarak şirket içi Edge aygıtından Azure bulutuna giden iletişime olanak tanır. Gelen iletişim yalnızca Azure IoT Hub'ın iletileri Azure IoT Edge aygıtına (örneğin, Buluttan Aygıta ileti) itmesi gereken belirli senaryolar için gereklidir.

Azure IoT Edge çalışma süresine sahip sunucular için bağlantı noktası yapılandırması için aşağıdaki tabloyu kullanın:

| Liman no. | Veya dışarıda | Bağlantı noktası kapsamı | Gerekli | Rehber |
|----------|-----------|------------|----------|----------|
| TCP 443 (HTTPS)| Out       | WAN        | Evet      | IoT Edge sağlama için giden açık. Bu yapılandırma, el ile komut dosyaları veya Azure IoT Aygıt Sağlama Hizmeti (DPS) kullanırken gereklidir.|

Tam bilgi [için, IoT Edge dağıtımı için Güvenlik Duvarı ve bağlantı noktası yapılandırma kurallarına](https://docs.microsoft.com/azure/iot-edge/troubleshoot)gidin.

## <a name="url-patterns-for-firewall-rules"></a>Güvenlik duvarı kuralları için URL desenleri

Ağ yöneticileri genellikle gelen ve giden trafiği filtrelemek için URL desenlerini temel alan gelişmiş güvenlik duvarı kurallarını yapılandırabilir. Veri Kutusu Kenarı aygıtınız ve hizmetiniz Azure Hizmet Veri Hizmeti Veri Yolundaki Diğer Microsoft uygulamalarına, Azure Active Directory Access Denetimine, depolama hesaplarına ve Microsoft Update sunucularına bağlıdır. Bu uygulamalarla ilişkili URL desenleri güvenlik duvarı kurallarını yapılandırmak için kullanılabilir. Bu uygulamalarla ilişkili URL desenlerinin değişebileceğini anlamak önemlidir. Bu değişiklikler, ağ yöneticisinin Veri Kutusu Edge'iniz için güvenlik duvarı kurallarını gerektiği gibi ve gerektiğinde izlemesini ve güncelleştirmesini gerektirir.

Veri Kutusu Kenarı sabit IP adreslerine bağlı olarak, çoğu durumda güvenlik duvarı kurallarınızı giden trafik için ayarlamanızı öneririz. Ancak, güvenli ortamlar oluşturmak için gereken gelişmiş güvenlik duvarı kuralları ayarlamak için aşağıdaki bilgileri kullanabilirsiniz.

> [!NOTE]
> - Aygıt (kaynak) IP'leri her zaman bulut özellikli tüm ağ arabirimlerine ayarlanmalıdır.
> - Hedef IP'ler Azure [veri merkezi IP aralıklarına](https://www.microsoft.com/download/confirmation.aspx?id=41653)ayarlanmalıdır.

### <a name="url-patterns-for-gateway-feature"></a>Ağ geçidi özelliği için URL desenleri

[!INCLUDE [URL patterns for firewall](../../includes/data-box-edge-gateway-url-patterns-firewall.md)]

### <a name="url-patterns-for-compute-feature"></a>İşlem özelliği için URL desenleri

| URL deseni                      | Bileşen veya işlevsellik                     |   
|----------------------------------|---------------------------------------------|
| https:\//mcr.microsoft.com<br></br>https://\*.cdn.mscr.io | Microsoft kapsayıcı kayıt defteri (gerekli)               |
| https://\*.azurecr.io                     | Kişisel ve üçüncü taraf konteyner kayıtları (isteğe bağlı) | 
| https://\*.azure-devices.net              | IoT Hub erişimi (gerekli)                             | 

### <a name="url-patterns-for-gateway-for-azure-government"></a>Azure Kamu için ağ geçidi için URL desenleri

[!INCLUDE [Azure Government URL patterns for firewall](../../includes/data-box-edge-gateway-gov-url-patterns-firewall.md)]

### <a name="url-patterns-for-compute-for-azure-government"></a>Azure Kamu için işlem için URL desenleri

| URL deseni                      | Bileşen veya işlevsellik                     |  
|----------------------------------|---------------------------------------------|
| https:\//mcr.microsoft.com<br></br>https://\*.cdn.mscr.com | Microsoft kapsayıcı kayıt defteri (gerekli)               |
| https://\*.azure-devices.us              | IoT Hub erişimi (gerekli)           |
| https://\*.azurecr.us                    | Kişisel ve üçüncü taraf konteyner kayıtları (isteğe bağlı) | 

## <a name="internet-bandwidth"></a>İnternet bant genişliği

[!INCLUDE [Internet bandwidth](../../includes/data-box-edge-gateway-internet-bandwidth.md)]

## <a name="compute-sizing-considerations"></a>Boyutlandırma hususları hesaplama

Veri Kutusu Edge cihazınızda yeterli kapasite olduğundan ve cihazınızdan en iyi performansı elde etmek için çözümünüzü geliştirirken ve test ederken deneyiminizi kullanın.

Göz önünde bulundurmanız gereken faktörler şunlardır:

- Konteyner özellikleri - **Aşağıdakileri** düşünün.

    - İş yükünde kaç kapsayıcı var? Birkaç kaynak yoğun olanlara karşı hafif konteynerbir sürü olabilir.
    - Bu kapsayıcılara ayrılan kaynaklar nelerdir, tükettikleri kaynaklar nelerdir?
    - Konteynerleriniz kaç katmanı paylaşıyor?
    - Kullanılmayan konteynerler var mı? Durdurulan bir kapsayıcı yine de disk alanını kaplar.
    - Konteynerleriniz hangi dilde yazılır?
- **İşlenen verilerin boyutu** - Kapsayıcılarınız ne kadar veri işleyecek? Bu veriler disk alanını tüketecek mi yoksa veriler bellekte işlenecek mi?
- **Beklenen performans** - Çözümünüzün istenen performans özellikleri nelerdir? 

Çözümünüzün performansını anlamak ve iyileştirmek için şunları kullanabilirsiniz:

- Azure portalında bulunan işlem ölçümleri. Veri Kutusu Kenarı kaynağınıza gidin ve ardından **İzleme > Ölçümleri'ne**gidin. Kullanılabilir kaynakları ve kaynakların nasıl tüketilmelerini anlamak için **Edge bilgi** işlem - Bellek kullanımı ve Kenar bilgi işlem - Yüzde **CPU'ya** bakın.
- Aşağıdaki gibi aygıtın PowerShell arabirimi üzerinden kullanılabilen izleme komutları:

    - `dkrdbe stats`konteyner(ler) kaynak kullanım istatistiklerinin canlı akışını elde etmek için. Komut CPU, bellek kullanımı, bellek sınırı ve ağ IO ölçümlerini destekler.
    - `dkrdbe system df`kullanılan disk alanı miktarı ile ilgili bilgi almak için. 
    - `dkrdbe image prune`kullanılmayan görüntüleri temizlemek ve yer açmak için.
    - `dkrdbe ps --size`çalışan bir kapsayıcının yaklaşık boyutunu görüntülemek için. 

    Kullanılabilir komutlar hakkında daha fazla bilgi için Monitor'a gidin [ve bilgi işlem modüllerini giderin.](data-box-edge-connect-powershell-interface.md#monitor-and-troubleshoot-compute-modules)

Son olarak, çözümünüzü veri setinizde doğruladığınızdan ve üretime başlamadan önce Data Box Edge'deki performansı ölçtünüzü unutmayın.


## <a name="next-step"></a>Sonraki adım

- [Azure Veri Kutusu Kenarınızı Dağıtma](data-box-edge-deploy-prep.md)
