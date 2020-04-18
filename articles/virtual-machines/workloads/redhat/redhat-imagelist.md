---
title: Azure'da Red Hat Enterprise Linux görüntüleri | Microsoft Dokümanlar
description: Microsoft Azure'da Red Hat Enterprise Linux görüntüleri hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/16/2020
ms.author: alsin
ms.openlocfilehash: 028c30fced14a60af9f5683e6c6e087b15591735
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81605474"
---
# <a name="red-hat-enterprise-linux-rhel-images-available-in-azure"></a>Red Hat Enterprise Linux (RHEL) görüntüleri Azure'da kullanılabilir
Azure, farklı kullanım örnekleri için çeşitli RHEL görüntüleri sunar.

> [!NOTE]
> Tüm RHEL görüntüleri Azure genel ve Azure Resmi bulutlarda kullanılabilir. Azure Çin bulutlarında kullanılamazlar.

## <a name="list-of-rhel-images"></a>RHEL görüntüleri listesi
Bu, Azure'da bulunan RHEL görüntülerinin listesidir. Aksi belirtilmedikçe, tüm görüntüler LVM bölümlüdür ve normal RHEL depolarına eklenir (EUS değil, E4S değil). Aşağıdaki resimler şu anda genel kullanım için kullanılabilir:

Sunduğu| SKU | Bölümleme | Sağlama | Notlar
:----|:----|:-------------|:-------------|:-----
RHEL          | 6.7      | Ham    | Linux Aracısı |
|             | 6.8      | Ham    | Linux Aracısı |
|             | 6.9      | Ham    | Linux Aracısı |
|             | 6.10     | Ham    | Linux Aracısı |
|             | 7-HAM    | Ham    | Linux Aracısı | RHEL 7.x görüntü ailesi. <br> Varsayılan olarak normal depolara eklenir (EUS değil).
|             | 7-LVM    | Lvm    | Linux Aracısı | RHEL 7.x görüntü ailesi. <br> Varsayılan olarak normal depolara eklenir (EUS değil). Dağıtmak için standart bir RHEL görüntüsü arıyorsanız, bu görüntü kümesini ve/veya Nesil 2 muadili kullanın.
|             | 7lvm-gen2| Lvm    | Linux Aracısı | Nesil 2, RHEL 7.x görüntü ailesi. <br> Varsayılan olarak normal depolara eklenir (EUS değil). Dağıtmak için standart bir RHEL görüntüsü arıyorsanız, bu görüntü kümesini ve/veya Nesil 1 muadili kullanın.
|             | 7-RAW-CI | HAM-CI | bulut init  | RHEL 7.x görüntü ailesi. <br> Varsayılan olarak normal depolara eklenir (EUS değil).
|             | 7.2      | Ham    | Linux Aracısı |
|             | 7.3      | Ham    | Linux Aracısı |
|             | 7.4      | Ham    | Linux Aracısı | Nisan 2019 itibariyle varsayılan olarak EUS depolarına iliştirilmiştir.
|             | 74-gen2  | Ham    | Linux Aracısı | Varsayılan olarak EUS depolarına eklenir.
|             | 7,5      | Ham    | Linux Aracısı | Haziran 2019 itibariyle varsayılan olarak EUS depolarına eklenmiştir.
|             | 75-gen2  | Ham    | Linux Aracısı | Varsayılan olarak EUS depolarına eklenir.
|             | 7.6      | Ham    | Linux Aracısı | Mayıs 2019 itibariyle varsayılan olarak EUS depolarına iliştirilmiştir.
|             | 76-gen2  | Ham    | Linux Aracısı | Varsayılan olarak EUS depolarına eklenir.
|             | 7.7      | Lvm    | Linux Aracısı | Varsayılan olarak EUS depolarına eklenir.
|             | 8-LVM    | Lvm    | Linux Aracısı | RHEL 8.x görüntü ailesi. Düzenli depolara bağlı.
|             | 8-lvm-gen2| Lvm    | Linux Aracısı | Hyper-V Generation 2 - RHEL 8.x görüntü ailesi. Düzenli depolara bağlı.
|             | 8        | Lvm    | Linux Aracısı | RHEL 8.0 görüntüleri
|             | 8-gen2   | Lvm    | Linux Aracısı | Hyper-V Generation 2 - RHEL 8.0 görüntüleri.
|             | 8.1      | Lvm    | Linux Aracısı | RHEL 8.1 görüntüleri. Şu anda düzenli depolara bağlı.
|             | 81gen2   | Lvm    | Linux Aracısı | Hyper-V Generation 2 - RHEL 8.1 görüntüleri. Şu anda düzenli depolara bağlı.
RHEL-SAP      | 7.4      | Lvm    | Linux Aracısı | SAP HANA ve İş Uygulamaları için RHEL 7.4. E4S depolarına bağlı olarak, SAP ve RHEL için bir prim in yanı sıra temel işlem ücreti tahsil edilecektir.
|             | 74sap-gen2| Lvm    | Linux Aracısı | SAP HANA ve İş Uygulamaları için RHEL 7.4. Nesil 2 görüntü. E4S depolarına bağlı olarak, SAP ve RHEL için bir prim in yanı sıra temel işlem ücreti tahsil edilecektir.
|             | 7,5       | Lvm    | Linux Aracısı | SAP HANA ve İş Uygulamaları için RHEL 7.5. E4S depolarına bağlı olarak, SAP ve RHEL için bir prim in yanı sıra temel işlem ücreti tahsil edilecektir.
|             | 75sap-gen2| Lvm    | Linux Aracısı | SAP HANA ve İş Uygulamaları için RHEL 7.5. Nesil 2 görüntü. E4S depolarına bağlı olarak, SAP ve RHEL için bir prim in yanı sıra temel işlem ücreti tahsil edilecektir.
|             | 7.6       | Lvm    | Linux Aracısı | SAP HANA ve İş Uygulamaları için RHEL 7.6. E4S depolarına bağlı olarak, SAP ve RHEL için bir prim in yanı sıra temel işlem ücreti tahsil edilecektir.
|             | 76sap-gen2| Lvm    | Linux Aracısı | SAP HANA ve İş Uygulamaları için RHEL 7.6. Nesil 2 görüntü. E4S depolarına bağlı olarak, SAP ve RHEL için bir prim in yanı sıra temel işlem ücreti tahsil edilecektir.
|             | 7.7       | Lvm    | Linux Aracısı | SAP HANA ve İş Uygulamaları için RHEL 7.7. E4S depolarına bağlı olarak, SAP ve RHEL için bir prim in yanı sıra temel işlem ücreti tahsil edilecektir.
RHEL-SAP-HANA | 6.7       | Ham    | Linux Aracısı | SAP HANA için RHEL 6.7. RHEL-SAP görüntüleri lehine modası geçmiş.
|             | 7.2       | Lvm    | Linux Aracısı | SAP HANA için RHEL 7.2. RHEL-SAP görüntüleri lehine modası geçmiş.
|             | 7.3       | Lvm    | Linux Aracısı | SAP HANA için RHEL 7.3. RHEL-SAP görüntüleri lehine modası geçmiş.
RHEL-SAP-UYGULAMALARI | 6.8       | Ham    | Linux Aracısı | SAP İş Uygulamaları için RHEL 6.8. RHEL-SAP görüntüleri lehine modası geçmiş.
|             | 7.3       | Lvm    | Linux Aracısı | SAP İş Uygulamaları için RHEL 7.3. RHEL-SAP görüntüleri lehine modası geçmiş.
RHEL-HA       | 7.4       | Lvm    | Linux Aracısı | HA Eklentisi ile RHEL 7.4. Temel işlem ücretinin üzerine HA ve RHEL için bir prim tahsil edecektir.
|             | 7,5       | Lvm    | Linux Aracısı | HA Eklentisi ile RHEL 7.5. Temel işlem ücretinin üzerine HA ve RHEL için bir prim tahsil edecektir.
|             | 7.6       | Lvm    | Linux Aracısı | HA Eklentisi ile RHEL 7.6. Temel işlem ücretinin üzerine HA ve RHEL için bir prim tahsil edecektir.
RHEL-SAP-HA   | 7.4          | Lvm    | Linux Aracısı | HA ve Güncelleme Hizmetleri ile SAP için RHEL 7.4. E4S depolarına iliştirilmiştir. SAP ve HA depolarının yanı sıra TEMEL hesaplama ücretlerinin yanı sıra RHEL için de bir prim tahsil edecektir.
|             | 74sapha-gen2 | Lvm    | Linux Aracısı | HA ve Güncelleme Hizmetleri ile SAP için RHEL 7.4. Nesil 2 görüntü. E4S depolarına iliştirilmiştir. SAP ve HA depolarının yanı sıra TEMEL hesaplama ücretlerinin yanı sıra RHEL için de bir prim tahsil edecektir.
|             | 7,5          | Lvm    | Linux Aracısı | HA ve Güncelleme Hizmetleri ile SAP için RHEL 7.5. E4S depolarına iliştirilmiştir. SAP ve HA depolarının yanı sıra TEMEL hesaplama ücretlerinin yanı sıra RHEL için de bir prim tahsil edecektir.
|             | 7.6          | Lvm    | Linux Aracısı | HA ve Güncelleme Hizmetleri ile SAP için RHEL 7.6. E4S depolarına iliştirilmiştir. SAP ve HA depolarının yanı sıra TEMEL hesaplama ücretlerinin yanı sıra RHEL için de bir prim tahsil edecektir.
|             | 76sapha-gen2 | Lvm    | Linux Aracısı | HA ve Güncelleme Hizmetleri ile SAP için RHEL 7.6. Nesil 2 görüntü. E4S depolarına iliştirilmiştir. SAP ve HA depolarının yanı sıra TEMEL hesaplama ücretlerinin yanı sıra RHEL için de bir prim tahsil edecektir.
|             | 7.7          | Lvm    | Linux Aracısı | HA ve Güncelleme Hizmetleri ile SAP için RHEL 7.7. E4S depolarına iliştirilmiştir. SAP ve HA depolarının yanı sıra TEMEL hesaplama ücretlerinin yanı sıra RHEL için de bir prim tahsil edecektir.
|             | 77sapha-gen2 | Lvm    | Linux Aracısı | HA ve Güncelleme Hizmetleri ile SAP için RHEL 7.7. Nesil 2 görüntü. E4S depolarına iliştirilmiştir. SAP ve HA depolarının yanı sıra TEMEL hesaplama ücretlerinin yanı sıra RHEL için de bir prim tahsil edecektir.
rhel-byos     |rhel-lvm74| Lvm    | Linux Aracısı | Herhangi bir güncelleme kaynağına iliştirilmeyen RHEL 7.4 BYOS görüntüleri, RHEL primi ödemez.
|             |rhel-lvm75| Lvm    | Linux Aracısı | Herhangi bir güncelleme kaynağına iliştirilmeyen RHEL 7.5 BYOS görüntüleri, RHEL primi ödemez.
|             |rhel-lvm76| Lvm    | Linux Aracısı | Herhangi bir güncelleme kaynağına iliştirilmeyen RHEL 7.6 BYOS görüntüleri, RHEL primi ödemez.
|             |rhel-lvm77| Lvm    | Linux Aracısı | Herhangi bir güncelleme kaynağına iliştirilmeyen RHEL 7.7 BYOS görüntüleri, RHEL primi ödemez.
|             |rhel-lvm8 | Lvm    | Linux Aracısı | RHEL 8 BYOS görüntüleri (RHEL minör versiyonu görüntü sürüm değerinde gösterilir), herhangi bir güncelleştirme kaynağına iliştirilmez, BIR RHEL primi ücretlendirmez.

> [!NOTE]
> RHEL-SAP-HANA ürün teklifi Red Hat tarafından yaşamın sonu olarak kabul edilir. Varolan dağıtımlar normal şekilde çalışmaya devam eder, ancak Red Hat müşterilerin RHEL-SAP-HANA görüntülerinden SAP HANA depolarının yanı sıra HA eklentisini de içeren RHEL-SAP-HA görüntülerine geçiş yapmasını önerir. Red Hat'in SAP bulut teklifleri hakkında daha fazla bilgi için [burada.](https://access.redhat.com/articles/3751271)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure'daki Red Hat görüntüleri](./redhat-images.md)hakkında daha fazla bilgi edinin.
* Red Hat [Update Infrastructure](./redhat-rhui.md)hakkında daha fazla bilgi edinin.
* [RHEL BYOS teklifi](./byos.md)hakkında daha fazla bilgi edinin.
* RHEL'in tüm sürümleri için Red Hat destek politikaları hakkında bilgi [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata) sayfasında bulunabilir.
