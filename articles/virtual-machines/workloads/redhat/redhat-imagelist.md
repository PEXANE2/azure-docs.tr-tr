---
title: Azure 'da kullanılabilen görüntüleri Red Hat Enterprise Linux
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
ms.date: 04/16/2020
ms.author: alsin
ms.openlocfilehash: f4ebf433237193c0db7859b029bb224c72c92eae
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87291025"
---
# <a name="red-hat-enterprise-linux-rhel-images-available-in-azure"></a>Azure 'da bulunan Red Hat Enterprise Linux (RHEL) görüntüleri
Azure, farklı kullanım durumları için çeşitli RHEL görüntüleri sunmaktadır.

> [!NOTE]
> Tüm RHEL görüntüleri Azure genel ve Azure Kamu bulutlarında kullanılabilir. Azure Çin bulutlarında mevcut değildir.

## <a name="list-of-rhel-images"></a>RHEL görüntülerinin listesi
Bu, Azure 'da kullanılabilen RHEL görüntülerinin bir listesidir. Aksi belirtilmedikçe, tüm görüntüler LVM bölümlidir ve normal RHEL depolarına (E4S değil) eklenir. Aşağıdaki görüntüler şu anda genel kullanım için kullanılabilir:

> [!NOTE]
> Ham görüntüler, LVM bölümlenmiş görüntülerin yerine artık üretilmez. LVM, önemli ölçüde daha esnek bölüm yeniden boyutlandırma seçenekleri de dahil olmak üzere eski ham (LVM olmayan) bölümleme şemasına birçok avantaj sağlar.

Sunduğu| SKU | Bölümleme | Sağlama | Notlar
:----|:----|:-------------|:-------------|:-----
RHEL          | 6.7      | Madde    | Linux Aracısı |
|             | 6.8      | Madde    | Linux Aracısı |
|             | 6,9      | Madde    | Linux Aracısı |
|             | 6,10     | Madde    | Linux Aracısı |
|             | 7-HAM    | Madde    | Linux Aracısı | RHEL 7. x resim ailesi. <br> Varsayılan olarak normal depolara eklenir (EUS değil).
|             | 7-LVM    | LVM    | Linux Aracısı | RHEL 7. x resim ailesi. <br> Varsayılan olarak normal depolara eklenir (EUS değil). Dağıtılacak standart bir RHEL görüntüsü arıyorsanız, bu görüntü kümesini ve/veya 2. kuşak karşılığı 'nı kullanın.
|             | 7lvm-Gen2| LVM    | Linux Aracısı | 2. nesil, RHEL 7. x resim ailesi. <br> Varsayılan olarak normal depolara eklenir (EUS değil). Dağıtılacak standart bir RHEL görüntüsü arıyorsanız, bu görüntü kümesini ve/veya 1. kuşak karşılığı kullanın.
|             | 7-RAW-CI | HAM-Cı | kabuğunuzda Cloud-init  | RHEL 7. x resim ailesi. <br> Varsayılan olarak normal depolara eklenir (EUS değil).
|             | 7.2      | Madde    | Linux Aracısı |
|             | 7.3      | Madde    | Linux Aracısı |
|             | 7.4      | Madde    | Linux Aracısı | 2019 Nisan itibariyle varsayılan olarak, EUS depolarına eklenmiş.
|             | 74-Gen2  | Madde    | Linux Aracısı | Varsayılan olarak, EUS depolarına eklendi.
|             | 7,5      | Madde    | Linux Aracısı | , Haziran 2019 itibariyle varsayılan olarak EUS depolarına eklendi.
|             | 75-Gen2  | Madde    | Linux Aracısı | Varsayılan olarak, EUS depolarına eklendi.
|             | 7,6      | Madde    | Linux Aracısı | Varsayılan olarak 2019 Mayıs itibariyle EUS depolarında kullanıma açıldı.
|             | 76-Gen2  | Madde    | Linux Aracısı | Varsayılan olarak, EUS depolarına eklendi.
|             | 7,7      | LVM    | Linux Aracısı | Varsayılan olarak, EUS depolarına eklendi.
|             | 77-Gen2  | LVM    | Linux Aracısı | Varsayılan olarak, EUS depolarına eklendi.
|             | 7,8      | LVM    | Linux Aracısı | Düzenli depolara ekli (RHEL 7,8 için kullanılamaz)
|             | 78-Gen2  | LVM    | Linux Aracısı | Düzenli depolara ekli (RHEL 7,8 için kullanılamaz)
|             | 8-LVM    | LVM    | Linux Aracısı | RHEL 8. x resim ailesi. Düzenli depolara iliştirilir.
|             | 8-LVM-Gen2| LVM    | Linux Aracısı | Hyper-V 2. nesil-RHEL 8. x resim ailesi. Düzenli depolara iliştirilir.
|             | 8        | LVM    | Linux Aracısı | RHEL 8,0 görüntüleri.
|             | 8-Gen2   | LVM    | Linux Aracısı | Hyper-V 2. nesil-RHEL 8,0 görüntüleri.
|             | 8.1      | LVM    | Linux Aracısı | RHEL 8,2 görüntüleri. Şu anda normal depolara eklenmiş.
|             | 81gen2   | LVM    | Linux Aracısı | Hyper-V 2. nesil-RHEL 8,1 görüntüleri. Şu anda normal depolara eklenmiş.
|             | 8,1-CI   | LVM    | Linux Aracısı | Sağlama Aracısı olarak Cloud-init kullanan RHEL 8,1 görüntüleri. Şu anda normal depolara eklenmiş.
|             | 81-CI-Gen2| LVM    | Linux Aracısı | Hyper-V 2. nesil-RHEL 8,1 görüntüleri, sağlama Aracısı olarak Cloud-init kullanıyor. Şu anda normal depolara eklenmiş.
|             | 8.2      | LVM    | Linux Aracısı | RHEL 8,2 görüntüleri. Şu anda normal depolara eklenmiş.
|             | 82gen2   | LVM    | Linux Aracısı | Hyper-V 2. nesil-RHEL 8,1 görüntüleri. Şu anda normal depolara eklenmiş.
RHEL-SAP      | 7.4      | LVM    | Linux Aracısı | SAP HANA ve Iş uygulamaları için RHEL 7,4. E4S depolarına bağlı olarak SAP ve RHEL için bir Premium ve temel işlem ücreti de ücretlendirilir.
|             | 74sap-Gen2| LVM    | Linux Aracısı | SAP HANA ve Iş uygulamaları için RHEL 7,4. 2. nesil görüntü. E4S depolarına bağlı olarak SAP ve RHEL için bir Premium ve temel işlem ücreti de ücretlendirilir.
|             | 7,5       | LVM    | Linux Aracısı | SAP HANA ve Iş uygulamaları için RHEL 7,5. E4S depolarına bağlı olarak SAP ve RHEL için bir Premium ve temel işlem ücreti de ücretlendirilir.
|             | 75sap-Gen2| LVM    | Linux Aracısı | SAP HANA ve Iş uygulamaları için RHEL 7,5. 2. nesil görüntü. E4S depolarına bağlı olarak SAP ve RHEL için bir Premium ve temel işlem ücreti de ücretlendirilir.
|             | 7,6       | LVM    | Linux Aracısı | SAP HANA ve Iş uygulamaları için RHEL 7,6. E4S depolarına bağlı olarak SAP ve RHEL için bir Premium ve temel işlem ücreti de ücretlendirilir.
|             | 76sap-Gen2| LVM    | Linux Aracısı | SAP HANA ve Iş uygulamaları için RHEL 7,6. 2. nesil görüntü. E4S depolarına bağlı olarak SAP ve RHEL için bir Premium ve temel işlem ücreti de ücretlendirilir.
|             | 7,7       | LVM    | Linux Aracısı | SAP HANA ve Iş uygulamaları için RHEL 7,7. E4S depolarına bağlı olarak SAP ve RHEL için bir Premium ve temel işlem ücreti de ücretlendirilir.
RHEL-SAP-HANA | 6.7       | Madde    | Linux Aracısı | SAP HANA için RHEL 6,7. RHEL-SAP görüntülerinin kullanımı güncel değil.
|             | 7.2       | LVM    | Linux Aracısı | SAP HANA için RHEL 7,2. RHEL-SAP görüntülerinin kullanımı güncel değil.
|             | 7.3       | LVM    | Linux Aracısı | SAP HANA için RHEL 7,3. RHEL-SAP görüntülerinin kullanımı güncel değil.
RHEL-SAP-APPS | 6.8       | Madde    | Linux Aracısı | SAP Business Applications için RHEL 6,8. RHEL-SAP görüntülerinin kullanımı güncel değil.
|             | 7.3       | LVM    | Linux Aracısı | SAP Business Applications için RHEL 7,3. RHEL-SAP görüntülerinin kullanımı güncel değil.
RHEL-HA       | 7.4       | LVM    | Linux Aracısı | HA eklentisi ile RHEL 7,4. Temel işlem ücretinin üzerine HA ve RHEL için Premium ücret alınacaktır.
|             | 7,5       | LVM    | Linux Aracısı | HA eklentisi ile RHEL 7,5. Temel işlem ücretinin üzerine HA ve RHEL için Premium ücret alınacaktır.
|             | 7,6       | LVM    | Linux Aracısı | HA eklentisi ile RHEL 7,6. Temel işlem ücretinin üzerine HA ve RHEL için Premium ücret alınacaktır.
RHEL-SAP-HA   | 7.4          | LVM    | Linux Aracısı | HA ve Update Services ile SAP için RHEL 7,4. E4S depolarına eklendi. SAP ve HA depoları ve RHEL 'nin yanı sıra temel işlem ücretleri üzerinde de Premium ücret ödeyecektir.
|             | 74sapha-Gen2 | LVM    | Linux Aracısı | HA ve Update Services ile SAP için RHEL 7,4. 2. nesil görüntü. E4S depolarına eklendi. SAP ve HA depoları ve RHEL 'nin yanı sıra temel işlem ücretleri üzerinde de Premium ücret ödeyecektir.
|             | 7,5          | LVM    | Linux Aracısı | HA ve Update Services ile SAP için RHEL 7,5. E4S depolarına eklendi. SAP ve HA depoları ve RHEL 'nin yanı sıra temel işlem ücretleri üzerinde de Premium ücret ödeyecektir.
|             | 7,6          | LVM    | Linux Aracısı | HA ve Update Services ile SAP için RHEL 7,6. E4S depolarına eklendi. SAP ve HA depoları ve RHEL 'nin yanı sıra temel işlem ücretleri üzerinde de Premium ücret ödeyecektir.
|             | 76sapha-Gen2 | LVM    | Linux Aracısı | HA ve Update Services ile SAP için RHEL 7,6. 2. nesil görüntü. E4S depolarına eklendi. SAP ve HA depoları ve RHEL 'nin yanı sıra temel işlem ücretleri üzerinde de Premium ücret ödeyecektir.
|             | 7,7          | LVM    | Linux Aracısı | HA ve Update Services ile SAP için RHEL 7,7. E4S depolarına eklendi. SAP ve HA depoları ve RHEL 'nin yanı sıra temel işlem ücretleri üzerinde de Premium ücret ödeyecektir.
|             | 77sapha-Gen2 | LVM    | Linux Aracısı | HA ve Update Services ile SAP için RHEL 7,7. 2. nesil görüntü. E4S depolarına eklendi. SAP ve HA depoları ve RHEL 'nin yanı sıra temel işlem ücretleri üzerinde de Premium ücret ödeyecektir.
RHEL-byos     |RHEL-lvm74| LVM    | Linux Aracısı | RHEL 7,4 BYOS görüntüleri, herhangi bir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm75| LVM    | Linux Aracısı | RHEL 7,5 BYOS görüntüleri, herhangi bir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm76| LVM    | Linux Aracısı | RHEL 7,6 BYOS görüntüleri, herhangi bir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm76-Gen2| LVM    | Linux Aracısı | RHEL 7,6 2. nesil KCG görüntüleri, hiçbir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm77| LVM    | Linux Aracısı | RHEL 7,7 BYOS görüntüleri, herhangi bir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm77-Gen2| LVM    | Linux Aracısı | RHEL 7,7 2. nesil KCG görüntüleri, hiçbir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm78| LVM    | Linux Aracısı | RHEL 7,8 BYOS görüntüleri, herhangi bir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm78-Gen2| LVM    | Linux Aracısı | RHEL 7,8 2. nesil KCG görüntüleri, hiçbir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm8 | LVM    | Linux Aracısı | RHEL 8,0 BYOS görüntüleri, herhangi bir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm8-Gen2 | LVM    | Linux Aracısı | RHEL 8,0 2. nesil KCG görüntüleri, hiçbir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm81 | LVM    | Linux Aracısı | RHEL 8,1 BYOS görüntüleri, herhangi bir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm81-Gen2 | LVM    | Linux Aracısı | RHEL 8,1 2. nesil KCG görüntüleri, hiçbir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm82 | LVM    | Linux Aracısı | RHEL 8,2 BYOS görüntüleri, herhangi bir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.
|             |RHEL-lvm82-Gen2 | LVM    | Linux Aracısı | RHEL 8,2 2. nesil KCG görüntüleri, hiçbir güncelleştirme kaynağına iliştirilmemiş bir RHEL Premium ücretlendirilmez.

> [!NOTE]
> RHEL-SAP-HANA ürün teklifi, Red Hat ile yaşam sonu olarak değerlendirilir. Mevcut dağıtımlar normal şekilde çalışmaya devam eder, ancak Red Hat müşterilerin RHEL-SAP-HANA görüntülerinden, SAP HANA depoları ve HA eklentisi de içeren RHEL-SAP-HA yansımalarına geçiş gerçekleştirmesini önerir. Red Hat 'in SAP bulut teklifleri hakkında daha fazla ayrıntıya [buradan](https://access.redhat.com/articles/3751271)ulaşabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure 'Da Red Hat görüntüleri](./redhat-images.md)hakkında daha fazla bilgi edinin.
* [Red Hat güncelleştirme altyapısı](./redhat-rhui.md)hakkında daha fazla bilgi edinin.
* [RHEL BYOS teklifi](./byos.md)hakkında daha fazla bilgi edinin.
* Tüm RHEL sürümleri için Red Hat destek ilkeleriyle ilgili bilgiler [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfasında bulunabilir.
