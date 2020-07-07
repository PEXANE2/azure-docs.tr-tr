---
title: Windows sanal masaüstü için veri konumları-Azure
description: Windows sanal masaüstü verilerinin ve meta verilerinin hangi konumlarına depolanabileceğini kısaca inceleyin.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: b8cbee74a8f9639fc2c3838f38dabf2a62cf339e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82614453"
---
# <a name="data-locations-for-windows-virtual-desktop"></a>Windows sanal masaüstü için veri konumları

>[!IMPORTANT]
>Bu içerik, Windows sanal masaüstü nesneleri Azure Resource Manager desteklemeyen sonbahar 2019 sürümü için geçerlidir. Spring 2020 güncelleştirmesinde tanıtılan Azure Resource Manager Windows sanal masaüstü nesnelerini yönetmeye çalışıyorsanız, [Bu makaleye](../data-locations.md)bakın.

Windows sanal masaüstü Şu anda tüm coğrafi konumlar için kullanılabilir. Başlangıçta, hizmet meta verileri yalnızca Birleşik Devletler (US) Coğrafya 'da depolanabilir. Yöneticiler, konak havuzu sanal makinelerini ve dosya sunucuları gibi ilişkili hizmetleri oluştururken Kullanıcı verilerinin depolayabileceği konumu seçebilir. Azure [veri merkezi haritasında](https://azuredatacentermap.azurewebsites.net/)Azure coğrafi lıkları hakkında daha fazla bilgi edinin.

>[!NOTE]
>Microsoft, sizin veya kullanıcılarınızın Kullanıcı ve uygulamaya özel verilerinize erişebileceği bölgeleri denetlemez veya sınırlamaz.

>[!IMPORTANT]
>Windows sanal masaüstü, Birleşik Devletler bulunan bir veri merkezinde kiracı adları, ana bilgisayar havuzu adları, uygulama grubu adları ve Kullanıcı asıl adları gibi genel meta veri bilgilerini depolar. Depolanan meta veriler Rest 'de şifrelenir ve coğrafi olarak yedekli yansıtmalar Birleşik Devletler içinde tutulur. Uygulama ayarları ve Kullanıcı verileri gibi tüm müşteri verileri, müşterinin seçtiği ve hizmet tarafından yönetilmediği konumda bulunur.

Hizmet meta verileri olağanüstü durum kurtarma amaçları için Birleşik Devletler çoğaltılır.