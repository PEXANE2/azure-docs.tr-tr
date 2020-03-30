---
title: Özel uç noktaları kullanma
titleSuffix: Azure Storage
description: Sanal ağlardan depolama hesaplarına güvenli erişim için özel uç noktalara genel bakış.
services: storage
author: santoshc
ms.service: storage
ms.topic: article
ms.date: 03/12/2020
ms.author: santoshc
ms.reviewer: santoshc
ms.subservice: common
ms.openlocfilehash: c51f2db698f30368c9d4090d3d571fa0c131178a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299065"
---
# <a name="use-private-endpoints-for-azure-storage"></a>Azure Depolama için özel uç noktaları kullanma

Sanal ağdaki (VNet) [istemcilerin](../../private-link/private-endpoint-overview.md) [Özel Bağlantı](../../private-link/private-link-overview.md)üzerinden verilere güvenli bir şekilde erişmesine izin vermek için Azure Depolama hesaplarınız için özel uç noktaları kullanabilirsiniz. Özel bitiş noktası, depolama hesabı hizmetiniz için VNet adres alanından bir IP adresi kullanır. VNet'teki istemciler ile depolama hesabı arasındaki ağ trafiği, VNet üzerinden ve Microsoft omurga ağındaki özel bir bağlantıdan geçerek genel internetten açığa çıkarı ortadan kaldırır.

Depolama hesabınız için özel uç noktaları kullanmak şunları yapmanızı sağlar:

- Depolama hizmetinin genel bitiş noktasındaki tüm bağlantıları engelleyecek şekilde depolama güvenlik duvarını yapılandırarak depolama hesabınızı güvence altına alın.
- VNet'ten veri sızmasını engellemenizi sağlayarak sanal ağ (VNet) için güvenliği artırın.
- [VPN](../../vpn-gateway/vpn-gateway-about-vpngateways.md) veya [ExpressRoutes](../../expressroute/expressroute-locations.md) kullanarak VNet'e bağlanan şirket içi ağlardaki depolama hesaplarına güvenli bir şekilde bağlanın.

## <a name="conceptual-overview"></a>Kavramsal genel bakış

![Azure Depolama için özel uç noktalara genel bakış](media/storage-private-endpoints/storage-private-endpoints-overview.jpg)

Özel bitiş noktası, [Sanal Ağınızda](../../virtual-network/virtual-networks-overview.md) (VNet) bir Azure hizmeti için özel bir ağ arabirimidir. Depolama hesabınız için özel bir bitiş noktası oluşturduğunuzda, VNet'inizdeki istemciler ile depolama alanınız arasında güvenli bağlantı sağlar. Özel bitiş noktasına VNet'inizin IP adresi aralığından bir IP adresi atanır. Özel bitiş noktası ile depolama hizmeti arasındaki bağlantı, güvenli bir özel bağlantı kullanır.

VNet'teki uygulamalar, **aksi takdirde kullanacakları aynı bağlantı dizelerini ve yetkilendirme mekanizmalarını kullanarak,** özel bitiş noktası üzerinden depolama hizmetine sorunsuz bir şekilde bağlanabilir. Özel uç noktalar, REST ve SMB de dahil olmak üzere depolama hesabı tarafından desteklenen tüm protokollerle kullanılabilir.

[Hizmet Bitiş Noktaları'nı](../../virtual-network/virtual-network-service-endpoints-overview.md)kullanan alt ağlarda özel uç noktalar oluşturulabilir. Böylece bir alt ağdaki istemciler, başkalarına erişmek için hizmet bitiş noktalarını kullanırken özel bitiş noktasını kullanarak bir depolama hesabına bağlanabilir.

Sanal ağınızda depolama hizmeti için bir özel uç nokta oluşturduğunuzda depolama hesabı sahibine onaylaması için bir onay isteği gönderilir. Özel bitiş noktasının oluşturulmasını isteyen kullanıcı da depolama hesabının sahibiyse, bu onay isteği otomatik olarak onaylanır.

Depolama hesabı sahipleri, [Azure portalındaki](https://portal.azure.com)depolama hesabı için *'Özel uç noktalar'* sekmesi aracılığıyla onay isteklerini ve özel bitiş noktalarını yönetebilir.

> [!TIP]
> Depolama hesabınıza erişimi yalnızca özel bitiş noktası üzerinden kısıtlamak istiyorsanız, depolama güvenlik duvarını genel bitiş noktası üzerinden erişimi reddetmek veya denetlemek üzere yapılandırın.

Depolama güvenlik duvarını varsayılan olarak genel bitiş noktası üzerinden erişimi engelleyecek şekilde [yapılandırarak](storage-network-security.md#change-the-default-network-access-rule) depolama hesabınızı yalnızca VNet'inizden bağlantıları kabul etmek üzere güvenli hale getirebilirsiniz. Depolama güvenlik duvarı yalnızca ortak bitiş noktası üzerinden erişimi denetlediğinden, özel bitiş noktası olan bir VNet trafiğine izin vermek için güvenlik duvarı kuralına ihtiyacınız yoktur. Özel uç noktalar bunun yerine, alt ağların depolama hizmetine erişimini sağlamak için onay akışına güvenir.

### <a name="private-endpoints-for-azure-storage"></a>Azure Depolama için özel uç noktalar

Özel bitiş noktasını oluştururken, depolama hesabını ve bağlandığı depolama hizmetini belirtmeniz gerekir. Eğer erişmek için gereken bir depolama hesabında her depolama hizmeti için ayrı bir özel bitiş noktası gerekir, yani [Blobs](../blobs/storage-blobs-overview.md), [Veri Gölü Depolama Gen2](../blobs/data-lake-storage-introduction.md), [Dosyalar](../files/storage-files-introduction.md), [Kuyruklar](../queues/storage-queues-introduction.md), [Tablolar](../tables/table-storage-overview.md), veya Statik [Web Siteleri](../blobs/storage-blob-static-website.md).

> [!TIP]
> RA-GRS hesaplarında daha iyi okuma performansı için depolama hizmetinin ikincil örneği için ayrı bir özel bitiş noktası oluşturun.

Coğrafi yedekli depolama için yapılandırılan bir depolama hesabı nın bulunduğu ikincil bölgeye okuma erişimi için, hizmetin hem birincil hem de ikincil örnekleri için ayrı özel uç noktalara ihtiyacınız vardır. Başarısız olmak için ikincil örnek için özel bir bitiş noktası oluşturmanız **gerekmez.** Özel bitiş noktası, başarısız olduktan sonra yeni birincil örne otomatik olarak bağlanır. Depolama artıklığı seçenekleri hakkında daha fazla bilgi için [Azure Depolama artıklığı'na](storage-redundancy.md)bakın.

Depolama hesabınız için özel bir bitiş noktası oluşturma hakkında daha ayrıntılı bilgi için aşağıdaki makalelere bakın:

- [Azure portalındaki Depolama Hesabı deneyiminden bir depolama hesabına özel bağlanma](../../private-link/create-private-endpoint-storage-portal.md)
- [Azure portalındaki Özel Bağlantı Merkezi'ni kullanarak özel bir bitiş noktası oluşturma](../../private-link/create-private-endpoint-portal.md)
- [Azure CLI'yi kullanarak özel bir bitiş noktası oluşturma](../../private-link/create-private-endpoint-cli.md)
- [Azure PowerShell'i kullanarak özel bir bitiş noktası oluşturma](../../private-link/create-private-endpoint-powershell.md)

### <a name="connecting-to-private-endpoints"></a>Özel uç noktalara bağlanma

Özel bitiş noktasını kullanan bir VNet'teki istemciler, ortak bitiş noktasına bağlanan istemciler olarak depolama hesabı için aynı bağlantı dizesini kullanmalıdır. Bağlantıları özel bir bağlantı üzerinden VNet'ten depolama hesabına otomatik olarak yönlendirmek için DNS kararına güveniriz.

> [!IMPORTANT]
> Aksi takdirde kullanacağınız gibi, özel uç noktaları kullanarak depolama hesabına bağlanmak için aynı bağlantı dizesini kullanın. Lütfen '*privatelink*' alt etki alanı URL'sini kullanarak depolama hesabına bağlanmayın.

Varsayılan olarak, özel uç noktalar için gerekli güncelleştirmeleri içeren VNet'e bağlı özel bir [DNS bölgesi](../../dns/private-dns-overview.md) oluştururuz. Ancak, kendi DNS sunucunuzu kullanıyorsanız, DNS yapılandırmanızda ek değişiklikler yapmanız gerekebilir. Aşağıdaki [DNS değişiklikleri](#dns-changes-for-private-endpoints) yle ilgili bölümde özel uç noktalar için gerekli güncelleştirmeler açıklanmaktadır.

## <a name="dns-changes-for-private-endpoints"></a>Özel uç noktalar için DNS değişiklikleri

Özel bir bitiş noktası oluşturduğunuzda, depolama hesabının DNS CNAME kaynak kaydı, '*privatelink*' önekiyle bir alt etki alanında başka bir adla güncelleştirilir. Varsayılan olarak, özel uç noktaları için DNS kaynak kayıtları ile '*privatelink*' alt etki alanına karşılık gelen özel bir [DNS bölgesi](../../dns/private-dns-overview.md)de oluştururuz.

Depolama bitiş noktası URL'sini Özel bitiş noktasıyla VNet'in dışından çözdüğünüzde, depolama hizmetinin genel bitiş noktasına kadar çözülür. Özel bitiş noktasını barındıran VNet'ten çözüldüğünde, depolama bitiş noktası URL'si özel bitiş noktasının IP adresine giderilir.

Yukarıdaki resimdeki örnekte, özel bitiş noktasını barındıran VNet dışından çözüldüğünde depolama hesabı 'StorageAccountA' için DNS kaynak kayıtları aşağıdakiolacaktır:

| Adı                                                  | Tür  | Değer                                                 |
| :---------------------------------------------------- | :---: | :---------------------------------------------------- |
| ``StorageAccountA.blob.core.windows.net``             | CNAME | ``StorageAccountA.privatelink.blob.core.windows.net`` |
| ``StorageAccountA.privatelink.blob.core.windows.net`` | CNAME | \<depolama hizmeti genel bitiş noktası\>                   |
| \<depolama hizmeti genel bitiş noktası\>                   | A     | \<depolama hizmeti genel IP adresi\>                 |

Daha önce de belirtildiği gibi, depolama güvenlik duvarını kullanarak ortak bitiş noktası üzerinden VNet dışındaki istemcilerin erişimini reddedebilir veya denetleyebilirsiniz.

StorageAccountA için DNS kaynak kayıtları, özel bitiş noktasını barındıran VNet'teki bir istemci tarafından çözüldüğünde, aşağıdakiler olacaktır:

| Adı                                                  | Tür  | Değer                                                 |
| :---------------------------------------------------- | :---: | :---------------------------------------------------- |
| ``StorageAccountA.blob.core.windows.net``             | CNAME | ``StorageAccountA.privatelink.blob.core.windows.net`` |
| ``StorageAccountA.privatelink.blob.core.windows.net`` | A     | 10.1.1.5                                              |

Bu yaklaşım, özel uç noktaları barındıran VNet'teki istemciler ve VNet dışındaki istemciler için **aynı bağlantı dizesini kullanarak** depolama hesabına erişim sağlar.

Ağınızda özel bir DNS sunucusu kullanıyorsanız, istemcilerin depolama hesabı bitiş noktası için Özel bitiş noktası IP adresine ait FQDN'yi çözebilmesi gerekir. DNS sunucunuzu, özel bağlantı alt etki alanınızı VNet için özel DNS bölgesine devretmek üzere yapılandırmanız veya *'StorageAccountA.privatelink.blob.core.windows.net*' için A kayıtlarını özel bitiş noktası IP adresiyle yapılandırmanız gerekir.

> [!TIP]
> Özel veya şirket içi bir DNS sunucusu kullanırken, 'privatelink' alt etki alanında bulunan depolama hesabı adını özel bitiş noktası IP adresine çözümlemek için DNS sunucunuzu yapılandırmanız gerekir. Bunu, 'privatelink' alt etki alanını VNet'in özel DNS bölgesine atayarak veya DNS sunucunuzdaki DNS bölgesini yapılandırarak ve DNS A kayıtlarını ekleyerek yapabilirsiniz.

Depolama hizmetleri için özel uç noktalar için önerilen DNS bölge adları şunlardır:

| Depolama hizmeti        | Bölge adı                            |
| :--------------------- | :----------------------------------- |
| Blob hizmeti           | `privatelink.blob.core.windows.net`  |
| Data Lake Storage Gen2 | `privatelink.dfs.core.windows.net`   |
| Dosya hizmeti           | `privatelink.file.core.windows.net`  |
| Sıra hizmeti          | `privatelink.queue.core.windows.net` |
| Masa servisi          | `privatelink.table.core.windows.net` |
| Statik Web Siteleri        | `privatelink.web.core.windows.net`   |

Özel uç noktaları destekleyecek kendi DNS sunucunuzu yapılandırma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure sanal ağlarındaki kaynaklar için ad çözümlemesi](/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server)
- [Özel uç noktalar için DNS yapılandırması](/azure/private-link/private-endpoint-overview#dns-configuration)

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma ayrıntıları için Azure [Özel Bağlantı fiyatlandırması'na](https://azure.microsoft.com/pricing/details/private-link)bakın.

## <a name="known-issues"></a>Bilinen Sorunlar

Azure Depolama için özel uç noktalarla ilgili bilinen aşağıdaki sorunları unutmayın.

### <a name="copy-blob-support"></a>Blob desteğini kopyala

Depolama hesabı bir güvenlik duvarı tarafından korunuyorsa ve hesaba özel uç noktalardan erişilirse, bu hesap Bir [Kopya Blob](/rest/api/storageservices/copy-blob) işleminin kaynağı olarak hizmet veremez.

### <a name="storage-access-constraints-for-clients-in-vnets-with-private-endpoints"></a>Özel uç noktaları olan VNets'teki istemciler için depolama erişim kısıtlamaları

Varolan özel uç noktaları olan VNet'teki istemciler, özel uç noktaları olan diğer depolama hesaplarına erişirken kısıtlamalarla karşı karşıya kalır. Örneğin, Bir VNet N1'in Blob depolama alanı için a1 depolama hesabı için özel bir bitiş noktası olduğunu varsayalım. Depolama hesabı A2 Blob depolama için bir VNet N2 özel bir bitiş noktası varsa, o zaman VNet N1 istemcileri de özel bir bitiş noktası kullanarak hesap A2 Blob depolama erişmelidir. Depolama hesabı A2 Blob depolama için herhangi bir özel uç noktası yoksa, O zaman VNet N1 istemcileri özel bir bitiş noktası olmadan bu hesapblob depolama erişebilirsiniz.

Bu kısıtlama, A2 hesabı özel bir bitiş noktası oluşturduğunda yapılan DNS değişikliklerinin bir sonucudur.

### <a name="network-security-group-rules-for-subnets-with-private-endpoints"></a>Özel uç noktaları olan alt ağlar için Ağ Güvenlik Grubu kuralları

Şu anda, Ağ [Güvenlik Grubu](../../virtual-network/security-overview.md) (NSG) kurallarını ve özel uç noktaları için kullanıcı tanımlı yolları yapılandıramazsınız. Özel bitiş noktasını barındıran alt ağa uygulanan NSG kuralları özel bitiş noktasına uygulanır. Bu sorun için sınırlı bir geçici çözüm, kaynak alt ağlarda özel uç noktalar için erişim kurallarınızı uygulamaktır, ancak bu yaklaşım daha yüksek bir yönetim yükü gerektirebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](storage-network-security.md)
- [Blob depolama için güvenlik önerileri](../blobs/security-recommendations.md)
