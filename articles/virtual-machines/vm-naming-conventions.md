---
title: Azure VM boyutlarını adlandırma kuralları
description: Azure VM boyutları için kullanılan adlandırma kurallarını açıklar
ms.service: virtual-machines
subservice: sizes
author: mimckitt
ms.topic: conceptual
ms.date: 7/22/2020
ms.author: mimckitt
ms.custom: sttsinar
ms.openlocfilehash: 3af4387723f74c1e09597564bc5330183fe1df69
ms.sourcegitcommit: 14bf4129a73de2b51a575c3a0a7a3b9c86387b2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87439458"
---
# <a name="azure-virtual-machine-sizes-naming-conventions"></a>Azure sanal makine boyutları adlandırma kuralları

Bu sayfada, Azure VM 'Leri için kullanılan adlandırma kuralları özetlenmektedir. VM 'Ler, değişen özellikleri ve belirtimleri göstermek için bu adlandırma kurallarını kullanır.

## <a name="naming-convention-explanation"></a>Adlandırma kuralı açıklaması

**[Aile]**  +  **[Alt Aile *]**  +  **[sanal CPU sayısı]**  +  **[Eklenebilir Özellikler]**  +  **[Hızlandırıcı türü *]**  +  **[Sürüm]**

|Değer | Açıklama|
|---|---|
| Standard, Basic veya deneysel | "Standart", tüm GA VM boyutları için atanan varsayılan değerdir | 
| Family (Aile) | VM ailesi serisini belirtir| 
| * Alt Aile | Yalnızca özel VM farklılıkları için kullanılır|
| vCPU sayısı| VM 'nin sanal CPU sayısını belirtir |
| Eklenebilir Özellikler | Bir veya daha fazla küçük harf, ek özellikleri gösterir, örneğin: <br> a = AMD tabanlı işlemci <br> d = disk (yerel geçici disk var); Bu, daha yeni Azure VM 'Leri için bkz. [Ddv4 and Ddsv4-Series](./ddv4-ddsv4-series.md) <br> h = hazırda bekleme özellikli <br> i = yalıtılmış boyut <br> l = bellek yetersiz; bellek yoğunluğu boyutundan daha düşük bir bellek miktarı <br> d = bellek yoğunluğu; belirli boyuttaki en yüksek bellek miktarı <br> t = küçük bellek; belirli boyuttaki en küçük bellek miktarı <br> r = RDMA özellikli <br> s = [Ultra SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#ultra-disk) olası kullanımı da dahil olmak üzere Premium depolama kapasitesine sahiptir (örneğin, s özniteliği olmayan bazı yeni boyutlar Premium depolamayı desteklemeye devam edebilir, örn. M128, M64, vb.)<br> |
| * Hızlandırıcı türü | Özelleştirilmiş/GPU SKU 'Larında donanım hızlandırıcının türünü gösterir. Yalnızca S3 2020 ' den başlatılan yeni özelleştirilmiş/GPU SKU 'Larının adı içinde donanım hızlandırıcısı olacaktır. |
| Sürüm | VM ailesi serisinin sürümünü belirtir |

## <a name="example-breakdown"></a>Örnek döküm

**[Aile]**  +  **[Alt Aile *]**  +  **[sanal CPU sayısı]**  +  **[Eklenebilir Özellikler]**  +  **[Hızlandırıcı türü *]**  +  **[Sürüm]**

### <a name="example-1-m416ms_v2"></a>Örnek 1: M416ms_v2

|Değer | Açıklama|
|---|---|
| Family (Aile) | M | 
| vCPU sayısı | 416 |
| Eklenebilir Özellikler | d = bellek kullanımı <br> s = Premium depolama özellikli |
| Sürüm | v2 |

### <a name="example-2-nv16as_v4"></a>Örnek 2: NV16as_v4

|Değer | Açıklama|
|---|---|
| Family (Aile) | N | 
| Alt Aile | V |
| vCPU sayısı | 16 |
| Eklenebilir Özellikler | a = AMD tabanlı işlemci <br> s = Premium depolama özellikli |
| Sürüm | v4 |

### <a name="example-3-nc4as_t4_v3"></a>Örnek 3: NC4as_T4_v3

|Değer | Açıklama|
|---|---|
| Family (Aile) | N | 
| Alt Aile | C |
| vCPU sayısı | 4 |
| Eklenebilir Özellikler | a = AMD tabanlı işlemci <br> s = Premium depolama özellikli |
| Hızlandırıcı türü | T4 |
| Sürüm | v3 |

## <a name="next-steps"></a>Sonraki adımlar

Azure 'da kullanılabilir [VM boyutları](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) hakkında daha fazla bilgi edinin. 
