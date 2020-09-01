---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 08/03/2020
ms.author: alkohli
ms.openlocfilehash: 7ce49873b4e59bcf474deaea4420f56a72c9c589
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89087049"
---
VM boyutu, sanal makine için kullanılabilir hale getirilen CPU, GPU ve bellek gibi işlem kaynaklarının miktarını belirler. Sanal makinelerin iş yükü için uygun bir VM boyutu kullanılarak oluşturulması gerekir. Tüm makineler aynı donanımda çalışmaya rağmen, makine boyutları disk erişimi için farklı sınırlara sahiptir ve bu, VM 'leriniz genelinde genel disk erişimini yönetmenize yardımcı olabilir. İş yükü artarsa, mevcut sanal makine de yeniden boyutlandırılabilir.

Azure Stack Edge cihazında oluşturma için aşağıdaki standart Dv2 serisi VM 'Ler desteklenir.

### <a name="dv2-series"></a>Dv2 Serisi
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|-------------------|----|----|-----|----|------|------------|---------|
|**Standard_D1_v2** |1   |3,5 |50   |500 |3000  |4/4x500   |2 |
|**Standard_D2_v2** |2   |7   |100  |500 |6000  |8/8x500   |2 |
|**Standard_D3_v2** |4   |14  |200  |500 |12000 |16/16x500 |4 |
|**Standard_D4_v2** |8   |28  |400  |500 |24000 |32/32x500 |8 |
|**Standard_D5_v2** |16  |56  |800  |500 |48000 |64/64x500 |8 |

### <a name="dsv2-series"></a>DSv2 serisi
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|--------------------|----|----|----|-----|------|-------------|---------|
|**Standard_DS1_v2** |1   |3,5 |7   |1000 |4000  |4/4x2300   |2 |
|**Standard_DS2_v2** |2   |7   |14  |1000 |8000  |8/8x2300   |2 |
|**Standard_DS3_v2** |4   |14  |28  |1000 |16000 |16/16x2300 |4 |
|**Standard_DS4_v2** |8   |28  |56  |1000 |32000 |32/32x2300 |8 |
|**Standard_DS5_v2** |16  |56  |112 |1000 |64000 |64/64x2300 |8 |

### <a name="dv2-series"></a>Dv2 Serisi
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|--------------------|----|----|-----|----|-------|-------------|---------|
|**Standard_D11_v2** |2   |14  |100  |500 |6000   |8/8x500    |2 |
|**Standard_D12_v2** |4   |28  |200  |500 |12000  |16/16x500  |4 |
|**Standard_D13_v2** |8   |56  |400  |500 |24000  |32/32x500  |8 |
|**Standard_D14_v2** |16  |112 |800  |500 |48000  |64/64x500  |8 |


### <a name="dsv2-series"></a>DSv2 serisi
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | En yüksek işletim sistemi disk aktarım hızı (ıOPS) | En yüksek geçici depolama aktarım hızı (ıOPS) | Maksimum veri diski/aktarım hızı (ıOPS) | En fazla NIC |
|---------------------|----|----|-----|-----|-------|--------------|---------|
|**Standard_DS11_v2** |2   |14  |28   |1000 |8000   |4/4x2300    |2 |
|**Standard_DS12_v2** |4   |28  |56   |1000 |16000  |8/8x2300    |4 |
|**Standard_DS13_v2** |8   |56  |112  |1000 |32000  |16/16x2300  |8 |
|**Standard_DS14_v2** |16  |112 |224  |1000 |64000  |32/32x2300  |8 |

Daha fazla bilgi için [genel AMAÇLı VM boyutlarında Dv2 serisi](../articles/virtual-machines/dv2-dsv2-series.md#dv2-series)' ne gidin.
