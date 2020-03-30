---
title: Microsoft Azure Veri Kutusu Ağ Geçidi sistem gereksinimleri| Microsoft Dokümanlar
description: Azure Veri Kutusu Ağ Geçidiniz için yazılım ve ağ gereksinimleri hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 05/08/2019
ms.author: alkohli
ms.openlocfilehash: e8932097bcdef782b1a551d386c2872e02d8abfd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260195"
---
# <a name="azure-data-box-gateway-system-requirements"></a>Azure Veri Kutusu Ağ Geçidi sistem gereksinimleri

Bu makalede, Microsoft Azure Veri Kutusu Ağ Geçidi çözümünüz ve Azure Veri Kutusu Ağ Geçidi'ne bağlanan istemciler için önemli sistem gereksinimleri açıklanmaktadır. Veri Kutusu Ağ Geçidinizi dağıtmadan önce bilgileri dikkatle gözden geçirmenizi ve dağıtım ve sonraki işlem sırasında gerekli olduğu şekilde geri başvurmanızı öneririz.

Veri Kutusu Ağ Geçidi sanal aygıtıiçin sistem gereksinimleri şunlardır:

- **Ana bilgisayarlar için yazılım gereksinimleri** - desteklenen platformları, yerel yapılandırma ui tarayıcılarını, SMB istemcilerini ve aygıta bağlanan ana bilgisayarlar için ek gereksinimleri açıklar.
- **Aygıt için ağ gereksinimleri** - sanal aygıtın çalışması için herhangi bir ağ gereksinimleri hakkında bilgi sağlar.


## <a name="specifications-for-the-virtual-device"></a>Sanal cihaz için teknik özellikler

Veri Kutusu Ağ Geçidi'nin temel ana bilgisayar sistemi, aşağıdaki kaynakları sanal aygıtınızı sağlamak için ayırabilir:

| Belirtimler                                          | Açıklama              |
|---------------------------------------------------------|--------------------------|
| Sanal işlemciler (çekirdekler)   | En az 4 |
| Bellek  | En az 8 GB|
| Kullanılabilirlik|Tek düğüm|
| Diskler| İşletim sistemi diski: 250 GB <br> Veri diski: En az 2 TB, ölçülü kaynak sağlamalı ve SSD destekli olmalıdır|
| Ağ arabirimleri|1 veya daha çok sanal ağ arabirimi|


## <a name="supported-os-for-clients-connected-to-device"></a>Aygıta bağlı istemciler için desteklenen işletim sistemi

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-supported-client-os.md)]

## <a name="supported-protocols-for-clients-accessing-device"></a>Aygıta erişen istemciler için desteklenen protokoller

[!INCLUDE [Supported protocols for clients accessing device](../../includes/data-box-edge-gateway-supported-client-protocols.md)]

## <a name="supported-virtualization-platforms-for-device"></a>Cihaz için desteklenen sanallaştırma platformları

| **İşletim sistemi/platform**  |**Sürümler**   |**Notlar**  |
|---------|---------|---------|
|Hyper-V  |  2012 R2 <br> 2016 <br> 2019 |         |
|VMware ESXi     | 6.0 <br> 6.5 <br> 6.7       |VMware araçları desteklenmez.         |


## <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

[!INCLUDE [Supported storage accounts](../../includes/data-box-edge-gateway-supported-storage-accounts.md)]


## <a name="supported-storage-types"></a>Desteklenen depolama türleri

[!INCLUDE [Supported storage types](../../includes/data-box-edge-gateway-supported-storage-types.md)]

## <a name="supported-browsers-for-local-web-ui"></a>Yerel web ui için desteklenen tarayıcılar

[!INCLUDE [Supported browsers for local web UI](../../includes/data-box-edge-gateway-supported-browsers.md)]

## <a name="networking-port-requirements"></a>Ağ bağlantı noktası gereksinimleri

Aşağıdaki tabloda, SMB, bulut veya yönetim trafiğine izin vermek için güvenlik duvarınızda açılması gereken bağlantı noktaları listelenir. Bu tabloda, *gelen* *istemcinin* cihazınıza erişmelerini talep ettiği yönü ifade eder. *Giden* veya *giden,* Veri Kutusu Ağ Geçidi aygıtınızın verileri dağıtımın ötesinde harici olarak gönderdiği yönü ifade eder: örneğin, Internet'e giden.

[!INCLUDE [Port configuration for device](../../includes/data-box-edge-gateway-port-config.md)]

## <a name="url-patterns-for-firewall-rules"></a>Güvenlik duvarı kuralları için URL desenleri

Ağ yöneticileri genellikle gelen ve giden trafiği filtrelemek için URL desenlerini temel alan gelişmiş güvenlik duvarı kurallarını yapılandırabilir. Veri Kutusu Ağ Geçidi aygıtınız ve Veri Kutusu Ağ Geçidi hizmeti, Azure Hizmet Veri Veri Yolundaki Veri Veri Gönderisi, Azure Etkin Dizin Erişim Denetimi, depolama hesapları ve Microsoft Update sunucuları gibi diğer Microsoft uygulamalarına bağlıdır. Bu uygulamalarla ilişkili URL desenleri güvenlik duvarı kurallarını yapılandırmak için kullanılabilir. Bu uygulamalarla ilişkili URL desenlerinin değişebileceğini anlamak önemlidir. Bu da ağ yöneticisinin, Gerektiğinde Veri Kutusu Ağ Geçidiniz için güvenlik duvarı kurallarını izlemesi ve güncelleştirmesini gerektirir.

Veri Kutusu Ağ Geçidi sabit IP adreslerine bağlı olarak, çoğu durumda güvenlik duvarı kurallarınızı giden trafik için ayarlamanızı öneririz. Ancak, güvenli ortamlar oluşturmak için gereken gelişmiş güvenlik duvarı kuralları ayarlamak için aşağıdaki bilgileri kullanabilirsiniz.

> [!NOTE]
> - Aygıt (kaynak) IP'leri her zaman bulut özellikli tüm ağ arabirimlerine ayarlanmalıdır.
> - Hedef IP'ler Azure [veri merkezi IP aralıklarına](https://www.microsoft.com/download/confirmation.aspx?id=41653)ayarlanmalıdır.

[!INCLUDE [URL patterns for firewall](../../includes/data-box-edge-gateway-url-patterns-firewall.md)]

### <a name="url-patterns-for-azure-government"></a>Azure Kamu için URL desenleri

[!INCLUDE [Azure Government URL patterns for firewall](../../includes/data-box-edge-gateway-gov-url-patterns-firewall.md)]

## <a name="internet-bandwidth"></a>İnternet bant genişliği

[!INCLUDE [Internet bandwidth](../../includes/data-box-edge-gateway-internet-bandwidth.md)]

## <a name="next-step"></a>Sonraki adım

* [Azure Veri Kutusu Ağ Geçidinizi Dağıtma](data-box-gateway-deploy-prep.md)

