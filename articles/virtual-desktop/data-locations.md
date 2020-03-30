---
title: Windows Sanal Masaüstü için veri konumları - Azure
description: Windows Virtual Desktop verilerinin ve meta verilerinin hangi konumlarda depolandığına kısa bir genel bakış.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 5f48586f38c310c77c61f470d3e2fbc819c33f71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128046"
---
# <a name="data-locations-for-windows-virtual-desktop"></a>Windows Sanal Masaüstü için veri konumları

Windows Sanal Masaüstü şu anda tüm coğrafi konumlar için kullanılabilir. Başlangıçta, hizmet meta verileri yalnızca ABD (ABD) coğrafyasında depolanabilir. Yöneticiler, ana bilgisayar havuzu sanal makineleri ve dosya sunucuları gibi ilişkili hizmetleri oluştururken kullanıcı verilerini depolamak için konumu seçebilir. [Azure veri merkezi haritasında](https://azuredatacentermap.azurewebsites.net/)Azure coğrafyaları hakkında daha fazla bilgi edinin.

>[!NOTE]
>Microsoft, sizin veya kullanıcılarınızın kullanıcınıza ve uygulamaya özel verilerinize erişebileceği bölgeleri denetlemez veya sınırlamaz.

>[!IMPORTANT]
>Windows Sanal Masaüstü, ABD'de bulunan bir veri merkezinde kiracı adları, ana bilgisayar havuzu adları, uygulama grubu adları ve kullanıcı ana adları gibi genel meta veri bilgilerini depolar. Depolanan meta veriler istirahatte şifrelenir ve coğrafi yedekli aynalar Amerika Birleşik Devletleri içinde korunur. Uygulama ayarları ve kullanıcı verileri gibi tüm müşteri verileri müşterinin seçtiği konumda bulunur ve hizmet tarafından yönetilmez.

Hizmet meta verileri, olağanüstü durum kurtarma amacıyla ABD'de çoğaltılır.