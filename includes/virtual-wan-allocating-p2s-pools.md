---
title: include dosyası
description: include dosyası
services: virtual-wan
author: wellee
ms.service: virtual-wan
ms.topic: include
ms.date: 01/12/2021
ms.author: wellee
ms.custom: include file
ms.openlocfilehash: 67a996dbe1eb7090ea5f9ee9f0880698f4ba74a3
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98919082"
---
Bu bölümde, seçilen sanal WAN hub 'ının Noktadan siteye VPN Gateway ölçek biriminin 40 ' den büyük veya bu değere eşit olduğu istemci adres alanlarını ayırmaya yönelik yönergeler ve gereksinimler açıklanmaktadır.

### <a name="background"></a>Arka Plan

Sanal WAN hub 'ındaki Noktadan siteye VPN ağ geçitleri birden çok örnek ile dağıtılır. Noktadan siteye VPN ağ geçidinin her örneği, en fazla 10.000 eşzamanlı Noktadan siteye kullanıcı bağlantısını destekleyebilir. Sonuç olarak, 40 'den büyük ölçek birimleri için sanal WAN, farklı ölçek birimleri için ayrılan minimum adres havuzu sayısını gerektiren ek kapasite dağıtmaları gerekir.

Örneğin, bir 100 ölçek birimi seçilirse, sanal hub 'daki Noktadan siteye VPN Gateway için 5 örnek dağıtılır. Bu dağıtım, 50.000 eşzamanlı bağlantıyı ve **en az** 5 farklı adres havuzunu destekleyebilir.

**Kullanılabilir ölçek birimleri**

| Ölçek birimi | Desteklenen en fazla Istemci | En az adres havuzu sayısı |
|--- |--- |--- |
| 40 | 20000 | 2 |
| 60 | 30000 | 3 |
| 80 | 40000 | 4 |
| 100 | 50000 | 5 |
| 120 | 60000 | 6 |
| 140 | 70000 | 7 |
| 160 | 80000 | 8 |
| 180 | 90000 | 9 |
| 200 | 100000 | 10 |

### <a name="specifying-address-pools"></a>Adres havuzlarını belirtme

Aşağıda adres havuzları seçme yönergeleri verilmiştir. Noktadan siteye VPN adresi havuzu atamalarının sanal WAN tarafından otomatik olarak yapıldığını unutmayın.

1. Bir ağ geçidi örneği en fazla 10.000 eşzamanlı bağlantı sağlar. Bu nedenle, her adres havuzunun en az 10.000 benzersiz RFC1918 IP adresi içermesi gerekir.
1. Birden çok adres havuzu aralığı otomatik olarak birleştirilir ve **tek** bir ağ geçidi örneğine atanır. Bu işlem, 10.000 ' den az IP adresi olan herhangi bir ağ geçidi örneği için hepsini bir kez deneme halinde yapılır. Örneğin, 5.000 adresine sahip bir havuz, 8.000 adresine sahip ve tek bir ağ geçidi örneğine atanmış başka bir havuz ile sanal WAN tarafından otomatik olarak birleştirilebilir.
1. Tek bir adres havuzu, sanal WAN tarafından yalnızca tek bir ağ geçidi örneğine atanır.
1. Adres havuzları farklı olmalıdır. Adres havuzları arasında örtüşme yok olabilir.

> [!NOTE] 
> Bir adres havuzu bakım yapılmakta olan bir ağ geçidi örneğiyle ilişkiliyse, adres havuzu başka bir örneğe yeniden atanamaz.

### <a name="example"></a>Örnek 

Aşağıdaki örnek 60 ölçek birimlerinin 30.000 ' e kadar bağlantıyı desteklediğini ancak ayrılan adres havuzlarının, 30.000 ' den az eşzamanlı bağlantıyla sonuçlanmasına neden olan bir durum açıklanır.

Bu kurulumda desteklenen eşzamanlı bağlantıların toplam sayısı 28.192 ' dir. İlk ağ geçidi örneği 10.000 adreslerini, ikinci örnek 8.192 bağlantılarını destekler ve üçüncü örnek de 10.000 adreslerini destekler.

| Adres Havuzu numarası | Adres Havuzu | Desteklenen bağlantılar |
|--- |--- |--- |
| 1 | 10.12.0.0/15 | 10000 |
| 2 | 10.13.0.0/19 | 8192 |
| 3 | 10.14.0.0/15 | 10000|

**Öneri #1: adres havuzu #2 en az 10.000 ayrı IP adresine sahip olduğundan emin olun. (örnek: 10.13.0.0/15)**

**Öneri #2: bir adres havuzu daha ekleyin. (örnek: adres havuzu #4 10.15.0.0/21, 2048 adresle). 2 ve 4 adres havuzları otomatik olarak birleştirilir ve bu ağ geçidi örneğinin 10.000 eşzamanlı bağlantıyı desteklemesini sağlar.**
