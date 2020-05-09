---
title: Bölgeye göre kaynak kullanılabilirliği
description: Farklı Azure bölgelerindeki Azure Container Instances hizmeti için işlem ve bellek kaynaklarının kullanılabilirliği.
ms.topic: article
ms.date: 04/27/2020
ms.author: danlep
ms.openlocfilehash: 119c1202fe59e02a1b6ff802e3dbdc4d52133abe
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82583881"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>Azure bölgelerindeki Azure Container Instances kaynak kullanılabilirliği

Bu makalede, Azure bölgelerindeki ve hedef işletim sistemi tarafından Azure Container Instances işlem, bellek ve depolama kaynaklarının kullanılabilirliği ayrıntılı olarak açıklanır. 

Sunulan değerler, bir [kapsayıcı grubunun](container-instances-container-groups.md)dağıtımı başına kullanılabilir en fazla kaynak sayısıdır. Değerler yayın sırasında geçerli. 

> [!NOTE]
> Bu kaynak sınırları içinde oluşturulan kapsayıcı grupları dağıtım bölgesi içinde kullanılabilirliğine tabidir. Bir bölge ağı yük altında olduğunda, örnek dağıtırken hatayla karşılaşabilirsiniz. Bu tür bir dağıtım hatasını azaltmak için, daha düşük kaynak ayarlarına sahip örnekleri dağıtmaya çalışın veya daha sonra veya kullanılabilir kaynakları olan farklı bir bölgede dağıtımınızı daha sonra deneyin.

Dağıtımlarınızdaki kotalar ve diğer sınırlar hakkında daha fazla bilgi için bkz. [Azure Container Instances Için kotalar ve sınırlar](container-instances-quotas.md).

## <a name="availability---general"></a>Kullanılabilirlik-genel

Aşağıdaki bölgeler ve en fazla kaynaklar, Linux ve [desteklenen](container-instances-faq.md#what-windows-base-os-images-are-supported) Windows Server 2016 tabanlı kapsayıcılar ile kapsayıcı grupları için kullanılabilir.

| Bölgeler | İşletim Sistemi | En Fazla CPU | Maksimum bellek (GB) | Depolama (GB) |
| -------- | -- | :---: | :-----------: | :---: |
| Brezilya Güney, Kanada Orta, Orta Hindistan, Orta ABD, Doğu Asya, Doğu ABD, Doğu ABD 2, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Güney Hindistan, UK Güney, Batı Avrupa, Batı ABD, Batı ABD 2 | Linux | 4 | 16 | 50 |
| Avustralya Doğu, Japonya Doğu | Linux | 2 | 8 | 50 |
| Orta Kuzey ABD | Linux | 2 | 3,5 | 50 |
| Brezilya Güney, Japonya Doğu, Batı Avrupa | Windows | 4 | 16 | 20 |
| Doğu ABD, Batı ABD | Windows | 4 | 14 | 20 |
| Avustralya Doğu, Kanada Orta, Orta Hindistan, Orta ABD, Doğu Asya, Doğu ABD 2, Orta Kuzey ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Güney Hindistan, UK Güney, Batı ABD 2 | Windows | 2 | 3,5 | 20 |

## <a name="availability---windows-server-2019-ltsc-1809-deployments-preview"></a>Kullanılabilirlik-Windows Server 2019 LTSC, 1809 dağıtımları (Önizleme)

Windows Server 2019 tabanlı kapsayıcılar (Önizleme) ile kapsayıcı grupları için aşağıdaki bölgeler ve en fazla kaynaklar mevcuttur.

| Bölgeler | İşletim Sistemi | En Fazla CPU | Maksimum bellek (GB) | Depolama (GB) |
| -------- | -- | :---: | :-----------: | :---: |
| Avustralya Doğu, Brezilya Güney, Kanada Orta, Orta Hindistan, Orta ABD, Doğu Asya, Doğu ABD, Japonya Doğu, Orta Kuzey ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Güney Hindistan, UK Güney, Batı Avrupa | Windows | 4 | 16 | 20 |
| Doğu ABD 2, Batı ABD 2 | Windows | 2 | 3,5 | 20 |


## <a name="availability---virtual-network-deployment"></a>Kullanılabilirlik-sanal ağ dağıtımı

Aşağıdaki bölgeler ve en fazla kaynaklar, bir [Azure sanal ağında](container-instances-vnet.md)dağıtılan bir kapsayıcı grubu için kullanılabilir.

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

## <a name="availability---gpu-resources-preview"></a>Kullanılabilirlik-GPU kaynakları (Önizleme)

Aşağıdaki bölgeler ve en fazla kaynaklar, [GPU kaynaklarıyla](container-instances-gpu.md) dağıtılan bir kapsayıcı grubu için kullanılabilir (Önizleme).

> [!IMPORTANT]
> GPU kaynakları yalnızca istek üzerine kullanılabilir. GPU kaynaklarına erişim istemek için lütfen bir [Azure destek isteği][azure-support]gönderebilirsiniz.

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]
[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Aka.MS/aci/feedback](https://aka.ms/aci/feedback)adresinde ek bölgeler mi yoksa daha fazla kaynak kullanılabilirliği mi görmek istediğinizi takımın bilmesini sağlayın.

Kapsayıcı örneği dağıtımı sorunlarını giderme hakkında bilgi için bkz. [Azure Container Instances dağıtım sorunlarını giderme](container-instances-troubleshooting.md).


[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
