---
title: Bölgeye göre kaynak kullanılabilirliği
description: Farklı Azure bölgelerindeki Azure Kapsayıcı Örnekleri hizmeti için işlem ve bellek kaynaklarının kullanılabilirliği.
ms.topic: article
ms.date: 02/19/2020
ms.author: danlep
ms.openlocfilehash: c84ab7833f7c90d5d4b3c340c268fd9f75e20da9
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399392"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>Azure bölgelerindeki Azure Kapsayıcı Örnekleri için kaynak kullanılabilirliği

Bu makalede, Azure bölgelerinde ve hedef işletim sisteminde Azure Kapsayıcı Örnekleri bilgi işlem, bellek ve depolama kaynaklarının kullanılabilirliği ayrıntılı olarak açıklanmaktadır. 

Sunulan değerler, [bir kapsayıcı grubunun](container-instances-container-groups.md)dağıtımı başına kullanılabilen maksimum kaynaklardır. Değerler yayımı sırasında geçerlidir. 

> [!NOTE]
> Bu kaynak sınırları içinde oluşturulan kapsayıcı grupları dağıtım bölgesinde kullanılabilirlik durumuna bağlıdır. Bir bölge ağı yük altında olduğunda, örnek dağıtırken hatayla karşılaşabilirsiniz. Böyle bir dağıtım hatasını azaltmak için, örnekleri daha düşük kaynak ayarlarıyla dağıtmayı deneyin veya dağıtımınızı daha sonra veya kullanılabilir kaynaklarla farklı bir bölgede deneyin.

Dağıtımlarınızdaki kotalar ve diğer sınırlamalar hakkında bilgi için [Azure Kapsayıcı Örnekleri için Kotalar ve sınırlar'a](container-instances-quotas.md)bakın.

## <a name="availability---general"></a>Kullanılabilirlik - Genel

Aşağıdaki bölgeler ve maksimum kaynaklar Linux ve [desteklenen](container-instances-faq.md#what-windows-base-os-images-are-supported) Windows Server 2016 tabanlı kapsayıcılara sahip kapsayıcı grupları tarafından kullanılabilir.

| Bölgeler | İşletim Sistemi | En Fazla CPU | Maksimum Bellek (GB) | Depolama (GB) |
| -------- | -- | :---: | :-----------: | :---: |
| Brezilya Güney, Kanada Orta, Orta Hindistan, Orta ABD, Doğu Asya, Doğu ABD, Doğu ABD 2, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, Güney Hindistan, İngiltere Güney, Batı Avrupa, Batı ABD, Batı ABD 2 | Linux | 4 | 16 | 50 |
| Avustralya Doğu, Japonya Doğu | Linux | 2 | 8 | 50 |
| Orta Kuzey ABD | Linux | 2 | 3,5 | 50 |
| Brezilya Güney, Japonya Doğu, Batı Avrupa | Windows | 4 | 16 | 20 |
| Doğu ABD, Batı ABD | Windows | 4 | 14 | 20 |
| Avustralya Doğu, Kanada Orta, Orta Hindistan, Orta ABD, Doğu Asya, Doğu ABD 2, Kuzey Orta ABD, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, Güney Hindistan, İngiltere Güney, Batı ABD 2 | Windows | 2 | 3,5 | 20 |

## <a name="availability---windows-server-2019-ltsc-1809-deployments-preview"></a>Kullanılabilirlik - Windows Server 2019 LTSC, 1809 dağıtımları (önizleme)

Aşağıdaki bölgeler ve maksimum kaynaklar Windows Server 2019 tabanlı kapsayıcılara (önizleme) sahip kapsayıcı grupları tarafından kullanılabilir.

| Bölgeler | İşletim Sistemi | En Fazla CPU | Maksimum Bellek (GB) | Depolama (GB) |
| -------- | -- | :---: | :-----------: | :---: |
| Avustralya Doğu, Brezilya Güney, Kanada Orta, Orta Hindistan, Orta ABD, Doğu Asya, Doğu ABD, Japonya Doğu, Kuzey Orta ABD, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, Güney Hindistan, İngiltere Güney, Batı Avrupa | Windows | 4 | 16 | 20 |
| Doğu ABD 2, Batı ABD 2 | Windows | 2 | 3,5 | 20 |


## <a name="availability---virtual-network-deployment"></a>Kullanılabilirlik - Sanal ağ dağıtımı

Aşağıdaki bölgeler ve maksimum kaynaklar, [Azure sanal ağında](container-instances-vnet.md)dağıtılan bir kapsayıcı grubu tarafından kullanılabilir.

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

## <a name="availability---gpu-resources-preview"></a>Kullanılabilirlik - GPU kaynakları (önizleme)

Aşağıdaki bölgeler ve maksimum kaynaklar [GPU kaynakları](container-instances-gpu.md) (önizleme) ile dağıtılan bir kapsayıcı grubu için kullanılabilir.

> [!IMPORTANT]
> GPU kaynakları yalnızca istek üzerine kullanılabilir. GPU kaynaklarına erişim isteğinde bulunmak için lütfen bir [Azure destek isteği][azure-support]gönderin.

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]
[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ek bölgeler veya [aka.ms/aci/feedback'da](https://aka.ms/aci/feedback)kaynak kullanılabilirliğini artırmak istiyorsanız takıma bildirin.

Sorun giderme kapsayıcı örneği dağıtımı hakkında bilgi için, [Azure Kapsayıcı Örnekleri ile sorun giderme dağıtım sorunları](container-instances-troubleshooting.md)bakın.


[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
