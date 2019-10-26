---
title: Azure Storage ile özel uç noktaları kullanma | Microsoft Docs
description: Sanal ağlardan depolama hesaplarına güvenli erişim için özel uç noktalara genel bakış.
services: storage
author: santoshc
ms.service: storage
ms.topic: article
ms.date: 09/25/2019
ms.author: santoshc
ms.reviewer: santoshc
ms.subservice: common
ms.openlocfilehash: aec12cee7466e59389b28742bf66247751a0d22b
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72949475"
---
# <a name="using-private-endpoints-for-azure-storage-preview"></a>Azure depolama için özel uç noktaları kullanma (Önizleme)

Azure depolama, bir sanal ağdaki (VNet) istemciler için [Özel uç noktaların](../../private-link/private-endpoint-overview.md) kullanımını, bir depolama hesabındaki verilere [özel bir bağlantı](../../private-link/private-link-overview.md)üzerinden güvenli bir şekilde erişmesini sağlar. Özel uç nokta, depolama hesabı hizmetiniz için VNet adres alanından bir IP adresi kullanır. VNet ve depolama hesabı arasındaki ağ trafiği, VNet üzerinden ve Microsoft omurga ağındaki özel bir bağlantı üzerinden, genel İnternet 'ten etkilenme olasılığını ortadan kaldırır.

Depolama hesabınız için özel uç noktalar kullanmak şunları yapmanızı sağlar:
- Depolama hizmeti için genel uç noktasındaki tüm bağlantıları engelleyecek şekilde depolama hesabınızı güvenli hale getirerek depolama hesabınızın güvenliğini sağlayın.
- Sanal ağ (VNet) için güvenliği artırır. Bu, VNet 'ten veri alımını engellemeyi sağlar.
- [VPN](../../vpn-gateway/vpn-gateway-about-vpngateways.md) veya [ExpressRoute](../../expressroute/expressroute-locations.md) Ile özel eşleme ile sanal ağa bağlanan şirket içi ağlardan depolama hesaplarına güvenli bir şekilde bağlanın.

## <a name="conceptual-overview"></a>Kavramsal genel bakış
![Azure depolama için özel uç noktalara genel bakış](media/storage-private-endpoints/storage-private-endpoints-overview.jpg)

Özel uç nokta, bir Azure hizmeti için [sanal ağınızdaki](../../virtual-network/virtual-networks-overview.md) (VNet) özel bir ağ arabirimidir. VNet 'iniz ve Depolama hesabınızdaki istemciler arasında güvenli bağlantı sağlar. Özel uç noktaya sanal Ağınızın IP adresi aralığından bir IP adresi atanır. Özel uç nokta ve depolama hizmeti arasındaki bağlantı güvenli bir özel bağlantı kullanır.

VNet 'teki uygulamalar, diğer durumlarda kullandıkları aynı bağlantı dizelerini ve yetkilendirme mekanizmalarını kullanarak, Özel uç nokta üzerinden depolama hizmetine sorunsuz bir şekilde bağlanabilir. Özel uç noktalar, REST ve SMB dahil olmak üzere depolama hesabı tarafından desteklenen tüm protokollerle kullanılabilir.

VNet 'iniz içindeki bir depolama hizmeti için özel bir uç nokta oluşturduğunuzda, depolama hesabı sahibine onay için bir izin isteği gönderilir. Özel uç noktanın oluşturulmasını isteyen kullanıcı aynı zamanda depolama hesabının sahibiyseniz, bu onay isteği otomatik olarak onaylanır.

Depolama hesabı sahipleri, izin isteklerini onaylayabilir veya reddedebilir, Ayrıca, [Azure Portal](https://portal.azure.com)depolama hesabının ' özel uç noktaları ' sekmesinden özel uç noktaları görüntüleyebilir veya yönetebilir.

Depolama hesabınızı, varsayılan olarak genel bitiş noktası üzerinden erişimi reddedecek şekilde [yapılandırarak](storage-network-security.md#change-the-default-network-access-rule) , depolama hesabınızı yalnızca sanal ağınızdan gelen bağlantıları kabul edecek şekilde güvenli hale getirebilirsiniz. Depolama güvenlik duvarı kuralları yalnızca ortak uç noktasına uygulandığı için özel bir uç noktası olan VNet 'ten gelen trafiğe izin vermek için bir depolama güvenlik duvarı kuralına gerek yoktur. Bunun yerine özel uç noktalar, depolama hizmetine alt ağ erişimi sağlamak için izin akışını kullanır.

### <a name="private-endpoints-for-storage-service"></a>Depolama hizmeti için özel uç noktalar

Özel uç nokta oluştururken, bağlandığı depolama hesabını ve depolama hizmetini belirtmeniz gerekir. Her depolama hizmeti için, erişimin gerekli olduğu bir depolama hesabındaki, [BLOB](../blobs/storage-blobs-overview.md), [Data Lake Storage 2.](../blobs/data-lake-storage-introduction.md), [Dosya](../files/storage-files-introduction.md), [kuyruk](../queues/storage-queues-introduction.md), [tablo](../tables/table-storage-overview.md)veya [statik Web siteleri](../blobs/storage-blob-static-website.md)için özel bir uç nokta gerekir.

[Okuma Erişimli Coğrafi olarak yedekli depolama hesabı](storage-redundancy-grs.md#read-access-geo-redundant-storage)için okuma kullanılabilirliği sağlamak üzere hizmetin birincil ve ikincil örnekleri için ayrı özel uç noktalara ihtiyacınız vardır.

#### <a name="resources"></a>Kaynaklar

Depolama hesabınız için özel bir uç nokta oluşturma hakkında daha ayrıntılı bilgi için aşağıdaki makalelere bakın:

- [Azure portal depolama hesabı deneyiminden bir depolama hesabına özel olarak bağlanma](../../private-link/create-private-endpoint-storage-portal.md)
- [Azure portal özel bağlantı merkezini kullanarak özel bir uç nokta oluşturma](../../private-link/create-private-endpoint-portal.md)
- [Azure CLı kullanarak özel uç nokta oluşturma](../../private-link/create-private-endpoint-cli.md)
- [Azure PowerShell kullanarak özel uç nokta oluşturma](../../private-link/create-private-endpoint-powershell.md)

### <a name="dns-changes-for-private-endpoints"></a>Özel uç noktalar için DNS değişiklikleri

Bir depolama hizmeti için özel bir uç nokta oluşturduğunuzda, bu depolama uç noktası için DNS CNAME kaynak kaydını, '*Privatelink*' önekine sahip bir alt etki alanındaki diğer adla güncelleştiririz. Varsayılan olarak, sanal ağa bağlı [Özel BIR DNS bölgesi](../../dns/private-dns-overview.md) de oluşturacağız. Bu özel DNS bölgesi, '*Privatelink*' ön ekine sahip alt etki alanına karşılık gelir ve özel uç noktalar Için DNS A kaynak kayıtlarını içerir.

Depolama uç noktası URL 'sini özel uç noktanın oluşturulduğu VNet dışından çözümlediğinizde, depolama hizmeti için genel uç nokta olarak çözümlenmeye devam eder. Özel uç noktayı barındıran VNet 'ten çözümlendiğinde, depolama uç noktası URL 'SI özel uç noktanın IP adresine çözümlenir.

Yukarıdaki gösterilen örnek için, Özel uç noktayı barındıran VNet dışından çözümlendiğinde ' StorageAccountA ' depolama hesabı için DNS kaynak kayıtları şu şekilde olur:

| Adı                                                  | Tür  | Değer                                                 |
| :---------------------------------------------------- | :---: | :---------------------------------------------------- |
| ``StorageAccountA.blob.core.windows.net``             | CNAME | ``StorageAccountA.privatelink.blob.core.windows.net`` |
| ``StorageAccountA.privatelink.blob.core.windows.net`` | CNAME | \<genel uç nokta\>                                   |
| \<genel uç nokta\>                                   | A     | \<depolama hizmeti genel IP adresi\>                 |

Daha önce belirtildiği gibi, depolama güvenlik duvarını kullanarak genel uç nokta aracılığıyla tüm erişimi reddedebilirsiniz.

Özel uç noktasını barındıran VNet 'teki bir istemci tarafından çözümlendiğinde StorageAccountA için DNS kaynak kayıtları şu şekilde olur:

| Adı                                                  | Tür  | Değer                                                 |
| :---------------------------------------------------- | :---: | :---------------------------------------------------- |
| ``StorageAccountA.blob.core.windows.net``             | CNAME | ``StorageAccountA.privatelink.blob.core.windows.net`` |
| ``StorageAccountA.privatelink.blob.core.windows.net`` | A     | 10.1.1.5                                              |

Bu yaklaşım, Özel uç noktaları barındıran VNet 'ten aynı bağlantı dizesinin yanı sıra VNet dışındaki istemcileri kullanarak depolama hesabına erişim sağlar. VNet dışındaki tüm istemcilere erişimi reddetmek için depolama güvenlik duvarını kullanabilirsiniz.

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma ayrıntıları için bkz. [Azure özel bağlantı fiyatlandırması](https://azure.microsoft.com/pricing/details/private-link).

## <a name="known-issues"></a>Bilinen Sorunlar

### <a name="copy-blob-failures"></a>Blob başarısızlıklarını kopyalama

Şu anda, kaynak depolama hesabı bir güvenlik duvarı tarafından korunduğunda özel uç noktalar aracılığıyla erişilen depolama hesaplarına verilen [kopyalama blobu](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) komutları başarısız olur.

### <a name="storage-access-constraints-for-clients-in-vnets-with-private-endpoints"></a>Özel uç noktalarla VNET 'lerdeki istemciler için depolama erişimi kısıtlamaları

Özel uç noktaları olan diğer depolama hesaplarına erişirken, mevcut depolama özel uç noktası yüz kısıtlamalarına sahip VNET 'lerdeki istemciler. Örneğin, bir VNet N1 'nin bir depolama hesabı için a1, blob hizmeti için özel bir uç noktası olduğunu varsayalım. A2 depolama hesabı, blob hizmeti için bir VNet için bir özel uç nokta içeriyorsa, VNet N1 içindeki istemciler de özel bir uç nokta kullanarak a2 hesabındaki blob hizmetine da erişir. A2 depolama hesabı, blob hizmeti için özel uç nokta içermiyorsa, VNet N1 içindeki istemciler blob hizmetine özel bir uç nokta olmadan erişebilir.

Bu kısıtlama, a2 hesabı özel bir uç nokta oluşturduğunda yapılan DNS değişikliklerinin bir sonucudur.

### <a name="nsg-rules-on-subnets-with-private-endpoints"></a>Özel uç noktaları olan alt ağlardaki NSG kuralları

[Ağ güvenlik grubu](../../virtual-network/security-overview.md) (NSG) kuralları, şu anda özel uç noktaları olan alt ağlar için yapılandırılamaz. Bu sorun için sınırlı bir geçici çözüm, kaynak alt ağlardaki özel uç noktalar için erişim kurallarınızı uygulamaktır, ancak bu yaklaşım daha yüksek bir yönetim yükü gerektirebilir.
