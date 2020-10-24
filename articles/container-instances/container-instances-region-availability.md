---
title: Bölgeye göre kaynak kullanılabilirliği
description: Farklı Azure bölgelerindeki Azure Container Instances hizmeti için işlem ve bellek kaynaklarının kullanılabilirliği.
ms.topic: article
ms.date: 04/27/2020
ms.custom: references_regions
ms.openlocfilehash: 1ed3f50198c0410d9c893fe87523fa214ca03d88
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92521467"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>Azure bölgelerindeki Azure Container Instances kaynak kullanılabilirliği

Bu makalede, Azure bölgelerindeki ve hedef işletim sistemi tarafından Azure Container Instances işlem, bellek ve depolama kaynaklarının kullanılabilirliği ayrıntılı olarak açıklanır. Azure Container Instances için kullanılabilir bölgelerin genel bir listesi için, bkz. [kullanılabilir bölgeler](https://azure.microsoft.com/regions/services/).

Sunulan değerler, bir [kapsayıcı grubunun](container-instances-container-groups.md)dağıtımı başına kullanılabilir en fazla kaynak sayısıdır. Değerler yayın sırasında geçerli.

> [!NOTE]
> Bu kaynak sınırları içinde oluşturulan kapsayıcı grupları dağıtım bölgesi içinde kullanılabilirliğine tabidir. Bir bölge ağı yük altında olduğunda, örnek dağıtırken hatayla karşılaşabilirsiniz. Bu tür bir dağıtım hatasını azaltmak için, daha düşük kaynak ayarlarına sahip örnekleri dağıtmaya çalışın veya daha sonra veya kullanılabilir kaynakları olan farklı bir bölgede dağıtımınızı daha sonra deneyin.

Dağıtımlarınızdaki kotalar ve diğer sınırlar hakkında daha fazla bilgi için bkz. [Azure Container Instances Için kotalar ve sınırlar](container-instances-quotas.md).

## <a name="linux-container-groups"></a>Linux kapsayıcı grupları

Aşağıdaki bölgeler ve en fazla kaynak, genel dağıtımlar, [Azure sanal ağ](container-instances-vnet.md) dağıtımları ve [GPU kaynaklarıyla](container-instances-gpu.md) olan dağıtımlar (Önizleme) ile birlikte Linux kapsayıcılarıyla kapsayıcı grupları için kullanılabilir.

> [!IMPORTANT]
> Bir bölgedeki en fazla kaynak, dağıtımınıza bağlı olarak farklı bir bölgedir. Örneğin, bir bölgenin bir Azure sanal ağ dağıtımında genel bir dağıtıma göre farklı bir en yüksek CPU ve bellek boyutu olabilir. Aynı bölge, GPU kaynaklarıyla bir dağıtım için farklı bir maksimum değerler kümesine de sahip olabilir. Bölgenizin en büyük değerleri için aşağıdaki tabloları denetlemeden önce dağıtım türünü doğrulayın.

| Bölge | En Fazla CPU | Maksimum bellek (GB) | VNET Max CPU 'SU | VNET maksimum bellek (GB) | Depolama (GB) | GPU SKU 'Ları (Önizleme) |
| -------- | :---: | :---: | :----: | :-----: | :-------: | :----: |
| Doğu Avustralya | 4 | 16 | 4 | 16 | 50 | Yok |
| Brezilya Güney | 4 | 16 | 2 | 8 | 50 | Yok |
| Orta Kanada | 4 | 16 | 4 | 16 | 50 | Yok |
| Orta Hindistan | 4 | 16 | Yok | Yok | 50 | V100 |
| Central US | 4 | 16 | 4 | 16 | 50 | Yok |
| Doğu Asya | 4 | 16 | 4 | 16 | 50 | Yok |
| Doğu ABD | 4 | 16 | 4 | 16 | 50 | K80, P100, V100 |
| Doğu ABD 2 | 4 | 16 | 4 | 16 | 50 | Yok |
| Doğu Japonya | 2 | 8 | 4 | 16 | 50 | Yok |
| Güney Kore - Orta | 4 | 16 | Yok | Yok | 50 | Yok |
| Orta Kuzey ABD | 2 | 3,5 | 4 | 16 | 50 | Yok |
| Kuzey Avrupa | 4 | 16 | 4 | 16 | 50 | K80 |
| Orta Güney ABD | 4 | 16 | 4 | 16 | 50 | Yok |
| Güneydoğu Asya | 4 | 16 | 4 | 16 | 50 | P100, V100 |
| Güney Hindistan | 4 | 16 | Yok | Yok | 50 | Yok |
| Güney Birleşik Krallık | 4 | 16 | 4 | 16 | 50 | Yok |
| Orta Batı ABD| 4 | 16 | 4 | 16 | 50 | K80, P100, V100 |
| West Europe | 4 | 16 | 4 | 16 | 50 | K80, P100, V100 |
| Batı ABD | 4 | 16 | 2 | 4 | 16| Yok |
| Batı ABD 2 | 4 | 16 | 4 | 16 | 50 | K80, P100, V100 |

Aşağıdaki maksimum kaynak, [GPU kaynaklarıyla](container-instances-gpu.md) dağıtılan bir kapsayıcı grubu için kullanılabilir (Önizleme).

| GPU SKU 'Ları | GPU sayısı | En Fazla CPU | Maksimum bellek (GB) | Depolama (GB) |
| --- | --- | --- | --- | --- |
| K80 | 1 | 6 | 56 | 50 |
| K80 | 2 | 12 | 112 | 50 |
| K80 | 4 | 24 | 224 | 50 |
| P100, V100 | 1 | 6 | 112 | 50 |
| P100, V100 | 2 | 12 | 224 | 50 |
| P100, V100 | 4 | 24 | 448 | 50 |

## <a name="windows-container-groups"></a>Windows kapsayıcı grupları

Aşağıdaki bölgeler ve en fazla kaynaklar, [desteklenen ve önizleme](container-instances-faq.md#what-windows-base-os-images-are-supported) Windows Server kapsayıcıları olan kapsayıcı grupları için kullanılabilir.

| Bölge | Windows Server 2016 Max CPU | Windows Server 2016 en fazla bellek (GB) | Windows Server 2019 LTSC en fazla CPU | Windows Server 2019 LTSC en fazla bellek (GB) | Depolama (GB) |
| -------- | :---: | :---: | :----: | :-----: | :-------: |
| Doğu Avustralya | 2 | 3,5 | 4 | 16 | 20 |
| Brezilya Güney | 4 | 16 | 4 | 16 | 20 |
| Orta Kanada | 2 | 3,5 | 4 | 16 | 20 |
| Orta Hindistan | 2 | 3,5 | 4 | 16 | 20 |
| Central US | 2 | 3,5 | 4 | 16 | 20 |
| Doğu Asya | 2 | 3,5 | 4 | 16 | 20 |
| Doğu ABD | 2 | 8 | 4 | 16 | 20 |
| Doğu ABD 2 | 2 | 3,5 | 2 | 3,5 | 20 |
| Orta Fransa | 4 | 16 | 4 | 16 | 20 |
| Doğu Japonya | 4 | 16 | 4 | 16 | 20 |
| Güney Kore - Orta | 4 | 16 | 4 | 16 | 20 |
| Orta Kuzey ABD | 2 | 3,5 | 4 | 16 | 20 |
| Kuzey Avrupa | 2 | 3,5 | 4 | 16 | 20 |
| Orta Güney ABD | 2 | 3,5 | 4 | 16 | 20 |
| Güney Hindistan | 2 | 3,5 | 4 | 16 | 20 |
| Güneydoğu Asya | 2 | 3,5 | 4 | 16 | 20 |
| Güney Birleşik Krallık | 2 | 3,5 | 4 | 16 | 20 |
| Orta Batı ABD | 4 | 16 | 4 | 16 | 20 |
| West Europe | 4 | 16 | 4 | 16 | 20 |
| Batı ABD | 4 | 14 | Yok | Yok | 20 |
| Batı ABD 2 | 2 | 3,5 | 2 | 3,5 | 20 |

## <a name="next-steps"></a>Sonraki adımlar

[Aka.MS/aci/feedback](https://aka.ms/aci/feedback)adresinde ek bölgeler mi yoksa daha fazla kaynak kullanılabilirliği mi görmek istediğinizi takımın bilmesini sağlayın.

Kapsayıcı örneği dağıtımı sorunlarını giderme hakkında bilgi için bkz. [Azure Container Instances dağıtım sorunlarını giderme](container-instances-troubleshooting.md).


[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
