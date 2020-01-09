---
title: Azure 'da görüntüleri Red Hat Enterprise Linux | Microsoft Docs
description: Microsoft Azure Red Hat Enterprise Linux görüntüleri hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/18/2019
ms.author: alsin
ms.openlocfilehash: 61991e271a68c9160a34d0de99fe4c9259ca41cb
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75486345"
---
# <a name="red-hat-enterprise-linux-rhel-images-available-in-azure"></a>Azure 'da bulunan Red Hat Enterprise Linux (RHEL) görüntüleri
Azure, farklı kullanım durumları için çeşitli RHEL görüntüleri sunmaktadır.

## <a name="list-of-rhel-images"></a>RHEL görüntülerinin listesi
Bu, Azure 'da kullanılabilen RHEL görüntülerinin bir listesidir. Aksi belirtilmedikçe, tüm görüntüler LVM bölümlidir ve normal RHEL depolarına (E4S değil) eklenir. Aşağıdaki görüntüler şu anda genel kullanım için kullanılabilir:

Teklif| SKU | Bölümleme | Sağlama | Notlar
:----|:----|:-------------|:-------------|:-----
RHEL          | 6.7      | RAW    | Linux Aracısı |
|             | 6.8      | RAW    | Linux Aracısı |
|             | 6.9      | RAW    | Linux Aracısı |
|             | 6.10     | RAW    | Linux Aracısı |
|             | 7-HAM    | RAW    | Linux Aracısı | RHEL 7. x resim ailesi. <br> Varsayılan olarak normal depolara eklenir (EUS değil).
|             | 7-LVM    | LVM    | Linux Aracısı | RHEL 7. x resim ailesi. <br> Varsayılan olarak normal depolara eklenir (EUS değil).
|             | 7-RAW-CI | HAM-Cı | kabuğunuzda Cloud-init  | RHEL 7. x resim ailesi. <br> Varsayılan olarak normal depolara eklenir (EUS değil).
|             | 7.2      | RAW    | Linux Aracısı |
|             | 7.3      | RAW    | Linux Aracısı |
|             | 7.4      | RAW    | Linux Aracısı | 2019 Nisan itibariyle varsayılan olarak, EUS depolarına eklenmiş.
|             | 7.5      | RAW    | Linux Aracısı | , Haziran 2019 itibariyle varsayılan olarak EUS depolarına eklendi.
|             | 7,6      | RAW    | Linux Aracısı | Varsayılan olarak 2019 Mayıs itibariyle EUS depolarında kullanıma açıldı.
|             | 7,7      | LVM    | Linux Aracısı | Varsayılan olarak, EUS depolarına eklendi.
|             | 8        | LVM    | Linux Aracısı | RHEL 8. x resim ailesi
|             | 8-Gen2   | LVM    | Linux Aracısı | Hyper-V 2. nesil-RHEL 8. x resim ailesi.
RHEL-SAP      | 7.4      | LVM    | Linux Aracısı | SAP HANA ve Iş uygulamaları için RHEL 7,4. E4S depolarına bağlı olarak SAP ve RHEL için bir Premium ve temel işlem ücreti de ücretlendirilir.
|             | 7.5      | LVM    | Linux Aracısı | SAP HANA ve Iş uygulamaları için RHEL 7,5. E4S depolarına bağlı olarak SAP ve RHEL için bir Premium ve temel işlem ücreti de ücretlendirilir.
|             | 7,6      | LVM    | Linux Aracısı | SAP HANA ve Iş uygulamaları için RHEL 7,5. E4S depolarına bağlı olarak SAP ve RHEL için bir Premium ve temel işlem ücreti de ücretlendirilir.
|             | 7,7      | LVM    | Linux Aracısı | SAP HANA ve Iş uygulamaları için RHEL 7,5. E4S depolarına bağlı olarak SAP ve RHEL için bir Premium ve temel işlem ücreti de ücretlendirilir.
RHEL-SAP-HANA | 6.7      | RAW    | Linux Aracısı | SAP HANA için RHEL 6,7. RHEL-SAP görüntülerinin kullanımı güncel değil.
|             | 7.2      | LVM    | Linux Aracısı | SAP HANA için RHEL 7,2. RHEL-SAP görüntülerinin kullanımı güncel değil.
|             | 7.3      | LVM    | Linux Aracısı | SAP HANA için RHEL 7,3. RHEL-SAP görüntülerinin kullanımı güncel değil.
RHEL-SAP-APPS | 6.8      | RAW    | Linux Aracısı | SAP Business Applications için RHEL 6,8. RHEL-SAP görüntülerinin kullanımı güncel değil.
|             | 7.3      | LVM    | Linux Aracısı | SAP Business Applications için RHEL 7,3. RHEL-SAP görüntülerinin kullanımı güncel değil.
RHEL-HA       | 7.4      | LVM    | Linux Aracısı | HA eklentisi ile RHEL 7,4. Temel işlem ücretinin üzerine HA ve RHEL için Premium ücret alınacaktır.
|             | 7.5      | LVM    | Linux Aracısı | HA eklentisi ile RHEL 7,5. Temel işlem ücretinin üzerine HA ve RHEL için Premium ücret alınacaktır.
|             | 7,6      | LVM    | Linux Aracısı | HA eklentisi ile RHEL 7,6. Temel işlem ücretinin üzerine HA ve RHEL için Premium ücret alınacaktır.
RHEL-SAP-HA   | 7.4      | LVM    | Linux Aracısı | HA eklentisi ile SAP için RHEL 7,4. E4S depolarına eklendi. SAP ve HA depoları ve RHEL 'nin yanı sıra temel işlem ücretleri üzerinde de Premium ücret ödeyecektir.
|             | 7.5      | LVM    | Linux Aracısı | HA eklentisi ile SAP için RHEL 7,5. E4S depolarına eklendi. SAP ve HA depoları ve RHEL 'nin yanı sıra temel işlem ücretleri üzerinde de Premium ücret ödeyecektir.
|             | 7,6      | LVM    | Linux Aracısı | HA eklentisi ile SAP için RHEL 7,6. E4S depolarına eklendi. SAP ve HA depoları ve RHEL 'nin yanı sıra temel işlem ücretleri üzerinde de Premium ücret ödeyecektir.
RHEL-byos     |RHEL-lvm74| LVM    | Linux Aracısı | RHEL 7,4 BYOS görüntüleri, herhangi bir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm75| LVM    | Linux Aracısı | RHEL 7,5 BYOS görüntüleri, herhangi bir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm76| LVM    | Linux Aracısı | RHEL 7,6 BYOS görüntüleri, herhangi bir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm77| LVM    | Linux Aracısı | RHEL 7,7 BYOS görüntüleri, herhangi bir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm8 | LVM    | Linux Aracısı | RHEL 8 KCG görüntüleri (görüntü sürümü değerinde RHEL alt sürümü gösterilir), herhangi bir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure 'Da Red Hat görüntüleri](./redhat-images.md)hakkında daha fazla bilgi edinin.
* [Red Hat güncelleştirme altyapısı](./redhat-rhui.md)hakkında daha fazla bilgi edinin.
* [RHEL BYOS teklifi](./redhat-byos.md)hakkında daha fazla bilgi edinin.
* Tüm RHEL sürümleri için Red Hat destek ilkeleriyle ilgili bilgiler [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfasında bulunabilir.