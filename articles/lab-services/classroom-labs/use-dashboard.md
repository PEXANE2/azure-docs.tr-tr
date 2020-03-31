---
title: Azure Lab Hizmetleri'nde sınıf laboratuvarı için pano kullanma | Microsoft Dokümanlar
description: Azure Lab Hizmetleri'ndeki bir sınıf laboratuvarı için pano kullanmayı öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2020
ms.author: spelluru
ms.openlocfilehash: 363e5705e9ccf4aa207ff7e5cafb615bc01bc7d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77538949"
---
# <a name="dashboard-for-classroom-labs"></a>Sınıf laboratuvarları için pano
Bu makalede, Azure Lab Hizmetleri'ndeki bir sınıf laboratuvarının pano görünümü açıklanmaktadır. 

![Pano](../media/use-dashboard/dashboard.png)

## <a name="costs-and-billing-tile"></a>Maliyetler ve faturalandırma döşemesi
Bu döşeme aşağıdaki maliyet tahmini ayrıntılarını sağlar:

| Ayar | Değer | 
| ------- | ----- | 
| Kota saatleri | Bir kullanıcının VM'yi planlanan saatler dışında kullanabileceği maksimum saat sayısı. |
| Zamanlanan saatler | Laboratuvarda ayarlanan zamanlamaya göre tahakkuk edecek saatler. Bu değer yalnızca tüm zamanlama olaylarında ayarlanan bir kişi/tarih varsa kullanılabilir. |
| Saatler/kullanıcı | Kota saatleri ve zamanlanan saatlerin toplamı. |
| Maksimum kullanıcılar | İddia edilecek tüm sanal makinelere göre laboratuvarda maksimum kullanıcı sayısı. |
| Saatler x kullanıcılar | Saatler/kullanıcı kullanıcı sayısıile çarpılır. |
| Ayarlanmış kota | Belirli kullanıcılara eklenen kota saatlerinin toplamı. |
| Toplam saat * $/saat | Seçilen VM boyutuna göre saat başına maliyet. Bu fiyat gitmek gibi düzenli ödeme dayanmaktadır. |
| Toplam tahmini maliyet | Bu, geçerli ayarlara göre bu laboratuvar için en yüksek fiyattır. |

## <a name="template-tile"></a>Şablon döşemesi
Bu döşemede aşağıdaki bilgileri görürsünüz:

- Şablonun oluşturulduğu tarih 
- Şablonun en son yayımlandığı tarih 

Ayrıca, sınıf için [VM şablonlarını yönetebileceğiniz](how-to-create-manage-template.md) **Şablon** sayfasına gitmek için bir bağlantıda da vardır. 

## <a name="virtual-machine-pool-tile"></a>Sanal makine havuzu döşemesi

Bu döşemede aşağıdaki bilgileri görürsünüz:

- Öğrencilere (kullanıcılara) atanan sanal makine sayısı
- Henüz öğrencilere atanmamış sanal makinelerin sayısı

Ayrıca, laboratuvardaki [sanal makineler havuzunu yönetebileceğiniz](how-to-set-virtual-machine-passwords.md) **Sanal makine havuzu** sayfasına gitmek için bir bağlantıya sahiptir. 

## <a name="users-tile"></a>Kullanıcı döşemesi

Bu döşemede aşağıdaki bilgileri görürsünüz:

- Sınıfa kayıtlı kullanıcı sayısı
- Laboratuvara eklenen ancak sınıfa kayıtlı olmayan kullanıcı sayısı 

Ayrıca, laboratuvar için [kullanıcıları yönetebileceğiniz](how-to-configure-student-usage.md) **Kullanıcılar** sayfasına gitmek için bir bağlantı da vardır. 

## <a name="schedules-tile"></a>Zamanlamalar döşeme
Döşemedeki laboratuvar için geçerli zamanlanmış olayları görüyorsunuz. Ayrıca, [zamanlamaları oluşturabileceğiniz ve yönetebileceğiniz](how-to-create-schedules.md) **Zamanlama** sayfasına gitmek için bir bağlantıda da vardır. Döşeme, yalnızca iki zamanlanmış olayın ayrıntılarını ve laboratuvar için kalan zamanlanan olayların sayısını gösterir. 

![Zamanlanmış olaylar](../media/use-dashboard/scheduled-events.png)

