---
title: Azure rezervasyonu kullanım ayrıntılarını indirmeyle ilgili sorunları giderme
description: Bu makale, ayrılmış örnek kullanım ayrıntılarını Azure portaldan indirememenizin nedenini anlayıp bununla ilgili sorunları gidermenize yardımcı olur.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: reservations
ms.author: banders
ms.reviewer: yashar
ms.topic: troubleshooting
ms.date: 10/30/2020
ms.openlocfilehash: 85584626b050be8052f59c80ab294cc62747daed
ms.sourcegitcommit: 4b76c284eb3d2b81b103430371a10abb912a83f4
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2020
ms.locfileid: "93147346"
---
# <a name="troubleshoot-azure-reservation-download-usage-details"></a>Azure rezervasyonu kullanım ayrıntılarını indirmeyle ilgili sorunları giderme

Bu makale, ayrılmış örnek kullanım ayrıntılarını Azure portaldan indirememenizin nedenini anlayıp bununla ilgili sorunları gidermenize yardımcı olur.

## <a name="symptoms"></a>Belirtiler

1. [Azure portalda](https://portal.azure.com/) oturum açın ve **Rezervasyonlar** ’a gidin.
1. Bir rezervasyon seçin.
1. Rezervasyon genel bakış sayfasının varsayılan görünümünde son yedi güne ait kullanım gösterilir. Rezervasyonun kullanım ayrıntılarını indirmek için **CSV olarak indir** ’i seçebilirsiniz.
1. Zaman aralığını değiştirdiğinizde **CSV olarak indir** seçeneği kullanılamaz.
    :::image type="content" source="./media/troubleshoot-download-usage/download-csv-unavailable.png" alt-text="CSV olarak indir seçeneğinin kullanılamadığını gösteren örnek" lightbox="./media/troubleshoot-download-usage/download-csv-unavailable.png" :::

## <a name="cause"></a>Nedeni

Teknik sınırlamalardan dolayı Azure, en fazla yedi günlük verilerin indirilmesini destekler. Çok sayıda rezervasyona sahip müşteriler için uzun dönemlerde büyük miktarda veri oluşturulur. API’ler aracılığıyla verilerin döndürülmesi, Azure kaynaklarının zorlanmasına neden olur.

## <a name="solution"></a>Çözüm

Müşterilerin daha uzun sürelere ait verileri indirmek istediğini anlıyoruz. Ancak, şu anda uzun süreli rezervasyon kullanım verilerini indirmek mümkün değildir.

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyonlar hakkında daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri nedir?](save-compute-costs-reservations.md)