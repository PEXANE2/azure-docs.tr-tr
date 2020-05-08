---
title: Windows sanal masaüstü için veri konumları-Azure
description: Windows sanal masaüstü verilerinin ve meta verilerinin hangi konumlarına depolanabileceğini kısaca inceleyin.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 853f5766c4fd6344eecfd7be1d7911163133a8a5
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82611543"
---
# <a name="data-locations-for-windows-virtual-desktop"></a>Windows sanal masaüstü için veri konumları

>[!IMPORTANT]
>Bu içerik, Azure Resource Manager Windows sanal masaüstü nesneleriyle Spring 2020 güncelleştirmesine yöneliktir. Windows sanal masaüstü Fall 2019 sürümünü Azure Resource Manager nesneleri olmadan kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/data-locations-2019.md)bakın.
>
> Windows sanal masaüstü Spring 2020 güncelleştirmesi şu anda genel önizlemededir. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve bunu üretim iş yükleri için kullanmanızı önermiyoruz. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. 
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Windows sanal masaüstü Şu anda tüm coğrafi konumlar için kullanılabilir. Yöneticiler, konak havuzu sanal makinelerini ve dosya sunucuları gibi ilişkili hizmetleri oluştururken Kullanıcı verilerinin depolayabileceği konumu seçebilir. Azure [veri merkezi haritasında](https://azuredatacentermap.azurewebsites.net/)Azure coğrafi lıkları hakkında daha fazla bilgi edinin.

>[!NOTE]
>Microsoft, sizin veya kullanıcılarınızın Kullanıcı ve uygulamaya özel verilerinize erişebileceği bölgeleri denetlemez veya sınırlamaz.

>[!IMPORTANT]
>Windows sanal masaüstü, bir veri merkezinde kiracı adları, ana bilgisayar havuzu adları, uygulama grubu adları ve Kullanıcı asıl adları gibi genel meta veri bilgilerini depolar. Bir müşteri bir hizmet nesnesi oluşturduğunda, hizmet nesnesi için bir konum girmeleri gerekir. Girdikleri konum, nesnenin meta verilerinin depolanacağı yeri belirler. Müşteri bir Azure bölgesi seçer ve meta veriler ilgili Coğrafya 'da depolanır. Tüm Azure bölgelerinin ve ilgili coğrafi bölgelerin listesi için bkz. [Azure geographiler](https://azure.microsoft.com/global-infrastructure/geographies/).

Şu anda, meta verileri yalnızca Birleşik Devletler (US) Azure Coğrafya 'da depolamayı destekliyoruz. Depolanan meta veriler Rest 'de şifrelenir ve coğrafi olarak yedekli yansıtmalar Coğrafya içinde tutulur. Uygulama ayarları ve Kullanıcı verileri gibi tüm müşteri verileri, müşterinin seçtiği ve hizmet tarafından yönetilmediği konumda bulunur. Hizmet büyüdükçe daha fazla coğrafi hale gelecektir.

Hizmet meta verileri olağanüstü durum kurtarma amaçları için Azure Coğrafya içinde çoğaltılır.