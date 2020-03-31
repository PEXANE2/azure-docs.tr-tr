---
title: StorSimple 10 GbE arayüzleri için donanım | Microsoft Dokümanlar
description: Desteklenen küçük form faktörü takılabilir (SFP) alıcı-vericileri, kabloları ve anahtarları StorSimple aygıtınızdaki 10 GbE ağ arabirimi için açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: df8d40c7-f5ad-4f84-93eb-779fbd5f7243
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 7fafe177ea0c6c618dc4ab0727ba14c83cbb0102
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68965008"
---
# <a name="supported-hardware-for-the-10-gbe-network-interfaces-on-your-storsimple-device"></a>StorSimple cihazınızdaki 10 GbE ağ arabirimleri için desteklenen donanım

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede, Microsoft Azure StorSimple aygıtınızla çalışan tamamlayıcı donanım hakkında bilgi verilmektedir.

## <a name="list-of-devices-tested-by-microsoft"></a>Microsoft tarafından test edilen aygıtların listesi
Microsoft, aygıtlarla en iyi şekilde çalıştıklarından emin olmak için aşağıdaki küçük form faktörü takılabilir (SFP) alıcı-vericileri, kabloları ve anahtarlarını test etmiştir. (Yeni donanım sınanırken aşağıdaki tablolar güncelleştirilir.)

### <a name="sfp-transceivers"></a>SFP+ Alıcı-vericiler
| Marka | Model |
| --- | --- |
| Cisco |SFP-10G-SR |

### <a name="cables"></a>Kablolar
| S. Hayır. | Marka | Model |
| --- | --- | --- |
| 1. |Cisco |SFP-H10GB-CU1M |
| 2. |Cisco |SFP-H10GB-CU2M |
| 3. |Cisco |SFP-H10GB-CU3M |
| 4. |Tripp-Lite |N820-05M (OM3) |

### <a name="switches"></a>Anahtarlar
| S. Hayır. | Marka | Model |
| --- | --- | --- |
| 1. |Cisco |N3K-C3172PQ-10GE |
| 2. |Cisco |N3K-C3048-ZM-F |
| 3. |Cisco |N5K-C5596UP-FA |

## <a name="list-of-devices-tested-in-the-field"></a>Sahada test edilen cihazların listesi
Bu bölümde, StorSimple müşterileri tarafından alanında başarıyla dağıtılan aygıtların listesi yer almaktadır. Bunlar Microsoft tarafından sınanmamamıştır, ancak StorSimple aygıtınızla çalışma olasılığı yüksektir.

| Parametre | Değer |
| --- | --- |
| Değiştir |Juniper |
| Model değiştir |ex4550-32F |
| İşletim sistemi sürümünü değiştirme |Junos 12.3R9.4 |
| Bıçak modeli |Gemideki bağlantı noktaları (PIC 0) |
| Alıcı-verici yapmak |Juniper |
| Alıcı-verici modeli |Parça numarası 740-021308 <br></br> Parça numarası 740-030658 |
| Alıcı-verici firmware sürümü |Rev 01 Sürüm 0.0 (rapor) |
| Kablo modeli |Dubleks atlamacı LC/LC 50/125μ, OM3, LSZH |
| StorBasit model |8600 |
| StorSimple yazılım sürümü |6.3.9600.17491 |

## <a name="list-of-devices-tested-by-oem-provider-mellanox"></a>OEM sağlayıcısı (Mellanox) tarafından test edilen cihazların listesi
Mellanox, StorSimple cihazınızdaki 10 GbE ağ arabirimleri gibi Mellanox ağ arabirimleriyle en iyi şekilde çalışmasını sağlamak için aşağıdaki küçük form faktörü takılabilir (SFP) alıcı-vericileri, kabloları ve anahtarlarını test etmiştir.

### <a name="cables-and-modules-supported-by-mellanox"></a>Mellanox tarafından desteklenen kablolar ve modüller
Aşağıdaki tabloda Mellanox tarafından desteklenen kablolar ve modüller listelenmiştir. Bunlar Microsoft tarafından sınanmamamıştır, ancak StorSimple aygıtınızla çalışma olasılığı yüksektir.

| S. Hayır. | Hız | Model | Açıklama | Marka |
| --- | --- | --- | --- | --- |
| 1. |10 gbe |CAB-SFP-SFP-1M |pasif bakır kablo SFP+ 10 Gb/s 1m |Öz |
| 2. |10 gbe |CAB-SFP-SFP-2M |pasif bakır kablo SFP+ 10 Gb/s 2m |Öz |
| 3. |10 gbe |CAB-SFP-SFP-3M |pasif bakır kablo SFP+ 10 Gb/s 3m |Öz |
| 4. |10 gbe |CAB-SFP-SFP-5M |pasif bakır kablo SFP+ 10 Gb/s 5m |Öz |
| 5. |10 gbe |Cisco SFP-H10GBCU1M |Cisco SFP+ kablosu |Cisco |
| 6. |10 gbe |Cisco SFP-H10GBCU3M |Cisco SFP+ kablosu |Cisco |
| 7. |10 gbe |Cisco SFP-H10GBCU5M |Cisco SFP+ kablosu |Cisco |
| 8. |10 gbe |J9281B HP X242 10G |SFP+ - SFP+ 1m Bakır KabloYa Doğrudan Takma |Hp |
| 9. |10 gbe |455883-B21 HP BLc |10Gb SR SFP+ Tercih |Hp |
| 10. |10 gbe |455886-B21 HP BLc |10Gb LR SFP+ Tercih |Hp |
| 11. |10 gbe |487649-B21 HP BLc |SFP+ 0.5m 10GbE Bakır Kablo |Hp |
| 12. |10 gbe |487652-B21 HP BLc |SFP+ 1m 10GbE Bakır Kablo |Hp |
| 13. |10 gbe |487655-B21 HP BLc |SFP+ 3m 10GbE Bakır Kablo |Hp |
| 14. |10 gbe |487658-B21 HP BLc |SFP+ 7m 10GbE Bakır Kablo |Hp |
| 15. |10 gbe |537963-B21 HP BLc |SFP+ 5m 10GbE Bakır Kablo |Hp |
| 16. |10 gbe |AP784A HP |3m C serisi Pasif Bakır SFP+ Kablo |Hp |
| 17. |10 gbe |AP785A HP |5m C serisi Pasif Bakır SFP+ Kablo |Hp |
| 18. |10 gbe |AP818A HP |1m B serisi Aktif Bakır SFP+ Kablo |Hp |
| 19. |10 gbe |AP819A HP |3m B serisi Aktif Bakır SFP+ Kablo |Hp |
| 20. |10 gbe |J9150A HP |X132 10G SFP+ LC SR Alıcı-verici |Hp |
| 21. |10 gbe |J9151A HP |X132 10G SFP+ LC LR Alıcı-verici |Hp |
| 22. |10 gbe |J9283B HP |X242 10G SFP+ SFP+ 3m DAC Kablo |Hp |
| 23. |10 gbe |J9285B HP |X242 10G SFP+ SFP+ 7m DAC Kablo |Hp |
| 24. |10 gbe |JD095B HP |X240 10G SFP+ SFP+ 0.65m DAC Kablo |Hp |
| 25. |10 gbe |JD096B HP |X240 10G SFP+ SFP+ 1.2m DAC Kablo |Hp |
| 26. |10 gbe |JD097B HP |X240 10G SFP+ SFP+ 3m DAD Kablo |Hp |
| 27. |10 gbe |MAM1Q00A-QSA Mellanox |QSFP Için SFP+ Adaptörü |Mellanox Teknolojileri |
| 28. |10 gbe |MC2309124-006 Mt |Pasif Bakır Kablo 1x SFP+ Için QSFP 10Gb/s 24awg 7m |Mellanox Teknolojileri |
| 29. |10 gbe |MC2309124-007 Mt |Pasif Bakır Kablo 1x SFP+ Için QSFP 10Gb/s 24awg 7m |Mellanox Teknolojileri |
| 30. |10 gbe |MC2309130-003 Mt |Pasif Bakır Kablo 1x SFP+ Için QSFP 10Gb/s 30awg 3m |Mellanox Teknolojileri |
| 31. |10 gbe |MC2309130-00A Mt |Pasif Bakır Kablo 1x SFP+ Için QSFP 10Gb/s 30awg 0.5m |Mellanox Teknolojileri |
| 32. |10 gbe |MC3309124-005 Mt |Pasif Bakır Kablo 1x SFP+ 10Gb/s 24awg 5m |Mellanox Teknolojileri |
| 33. |10 gbe |MC3309124-007 Mt |Pasif Bakır Kablo 1x SFP+ 10Gb/s 24awg 7m |Mellanox Teknolojileri |
| 34. |10 gbe |MC3309130-003 Mt |Pasif Bakır Kablo 1x SFP+ 10Gb/s 30awg 3m |Mellanox Teknolojileri |
| 35. |10 gbe |MC3309130-00A Mt |Pasif Bakır Kablo 1x SFP+ 10Gb/s 30awg 0.5m |Mellanox Teknolojileri |

### <a name="switches-supported-by-mellanox"></a>Mellanox tarafından desteklenen anahtarlar
Aşağıdaki tabloda Mellanox tarafından desteklenen anahtarlar listelenir. Bunlar Microsoft tarafından sınanmamamıştır, ancak StorSimple aygıtınızla çalışma olasılığı yüksektir.

| S. Hayır. | Hız | Model | Açıklama | Marka |
| --- | --- | --- | --- | --- |
| 1. |10gbe |516733-B21 |HP ProCurve 6120XG 10GbE Ethernet Bıçak Anahtarı |Hp |
| 2. |10gbe |538113-B21 |HP 10GbE Geçiş Modülü (PTM) |Hp |
| 3. |10gbe |EN FAZLA 4093 |IBM PureFlex Sistem Kumaş EN4093 10 Gigabit Ölçeklenebilir Anahtar Modülü |IBM |
| 4. |1GbE |3020 |Cisco Catalyst 3020 1GbE anahtar bıçak |Cisco |
| 5. |1GbE |3020X |Cisco Catalyst 3020X 1GbE anahtar bıçak |Cisco |
| 6. |1GbE |438030-B21 |HP 1GbE anahtar modülü - GbE2c Layer 2/3 Ethernet Blade Switch |Hp |
| 7. |1GbE |6120G |HP ProCurve 6120G/XG 1GbE anahtar bıçak |Hp |

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple donanım bileşenleri ve durumu hakkında daha fazla bilgi edinin.](storsimple-monitor-hardware-status.md)

