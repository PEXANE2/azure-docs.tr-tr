---
title: Windows sanal masaüstü için veri konumları-Azure
description: Windows sanal masaüstü verilerinin ve meta verilerinin hangi konumlarına depolanabileceğini kısaca inceleyin.
author: Heidilohr
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: e626b7e729e394b1012848904f5ce12279c3ef24
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88010081"
---
# <a name="data-and-metadata-locations-for-windows-virtual-desktop"></a>Windows sanal masaüstü için veriler ve meta veri konumları

>[!IMPORTANT]
>Bu içerik Azure Resource Manager Windows sanal masaüstü nesneleri ile Windows sanal masaüstü için geçerlidir. Azure Resource Manager nesneleri olmadan Windows sanal masaüstü (klasik) kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/data-locations-2019.md)bakın.

Windows sanal masaüstü Şu anda tüm coğrafi konumlar için kullanılabilir. Yöneticiler, konak havuzu sanal makinelerini ve dosya sunucuları gibi ilişkili hizmetleri oluştururken Kullanıcı verilerinin depolayabileceği konumu seçebilir. Azure [veri merkezi haritasında](https://azuredatacentermap.azurewebsites.net/)Azure coğrafi lıkları hakkında daha fazla bilgi edinin.

>[!NOTE]
>Microsoft, sizin veya kullanıcılarınızın Kullanıcı ve uygulamaya özel verilerinize erişebileceği bölgeleri denetlemez veya sınırlamaz.

>[!IMPORTANT]
>Windows sanal masaüstü, bir veri merkezinde kiracı adları, ana bilgisayar havuzu adları, uygulama grubu adları ve Kullanıcı asıl adları gibi genel meta veri bilgilerini depolar. Bir müşteri bir hizmet nesnesi oluşturduğunda, hizmet nesnesi için bir konum girmeleri gerekir. Girdikleri konum, nesnenin meta verilerinin depolanacağı yeri belirler. Müşteri bir Azure bölgesi seçer ve meta veriler ilgili Coğrafya 'da depolanır. Tüm Azure bölgelerinin ve ilgili coğrafi bölgelerin listesi için bkz. [Azure geographiler](https://azure.microsoft.com/global-infrastructure/geographies/).

Şu anda, meta verileri yalnızca Birleşik Devletler (US) Azure Coğrafya 'da depolamayı destekliyoruz. Depolanan meta veriler Rest 'de şifrelenir ve coğrafi olarak yedekli yansıtmalar Coğrafya içinde tutulur. Uygulama ayarları ve Kullanıcı verileri gibi tüm müşteri verileri, müşterinin seçtiği ve hizmet tarafından yönetilmediği konumda bulunur. Hizmet büyüdükçe daha fazla coğrafi hale gelecektir.

Hizmet meta verileri olağanüstü durum kurtarma amaçları için Azure Coğrafya içinde çoğaltılır.