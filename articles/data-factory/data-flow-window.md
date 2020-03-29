---
title: Veri akışı Penceresi Dönüştürme eşleme
description: Azure Veri Fabrikası eşleme veri akışı Penceresi Dönüşümü
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/30/2019
ms.openlocfilehash: fa34def67d91332a00bf0ee92b365957a47f9616
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74931476"
---
# <a name="azure-data-factory-window-transformation"></a>Azure Veri Fabrikası Pencere Dönüşümü



Pencere dönüşümü, veri akışlarınızdaki sütunların pencere tabanlı toplayıcılarını tanımladığınız yerdir. İfade Oluşturucu'nda, LEAD, LAG, NTILE, CUMEDIST, RANK, vb. gibi veri veya zaman pencerelerini (SQL OVER yan tümcesi) temel alan farklı türde toplamalar tanımlayabilirsiniz. Çıktınızda bu toplamaları içeren yeni bir alan oluşturulur. İsteğe bağlı grup tabanlı alanları da ekleyebilirsiniz.

![Pencere seçenekleri](media/data-flow/windows1.png "windows 1")

## <a name="over"></a>Üzerinde
Pencere dönüşümünüz için sütun verilerinin bölümlemesini ayarlayın. SQL eşdeğeri ```Partition By``` SQL'deki Over yan tümcesidir. Bir hesaplama oluşturmak veya bölümleme için kullanılacak bir ifade oluşturmak istiyorsanız, bunu sütun adının üzerine gidip "hesaplanmış sütun" seçeneğini seçerek yapabilirsiniz.

![Pencere seçenekleri](media/data-flow/windows4.png "windows 4")

## <a name="sort"></a>Sırala
Over yan tümcesinin bir ```Order By```diğer bölümü de . Bu, veri sıralama sıralaması sıralamasını ayarlar. Sıralama için bu sütun alanında hesaplanmış değer için bir ifade de oluşturabilirsiniz.

![Pencere seçenekleri](media/data-flow/windows5.png "windows 5")

## <a name="range-by"></a>Aralığı na göre
Ardından, pencere çerçevesini Sınırsız veya Sınırlanmış olarak ayarlayın. Sınırsız pencere çerçevesi ayarlamak için kaydırıcıyı her iki uçta da Sınırsız olarak ayarlayın. Sınırsız ve Geçerli Satır arasında bir ayar seçerseniz, Ofset başlangıç ve bitiş değerlerini ayarlamanız gerekir. Her iki değer de pozitif tamsayılar olacaktır. Verilerinizdeki göreli sayıları veya değerleri kullanabilirsiniz.

Pencere kaydırıcısının ayarlayışı gereken iki değer vardır: geçerli satırdan önceki değerler ve geçerli satırdan sonraki değerler. Başlangıç ve Bitiş ofset kaydırıcıdaki iki seçiciyle eşleşir.

![Pencere seçenekleri](media/data-flow/windows6.png "windows 6")

## <a name="window-columns"></a>Pencere sütunları
Son olarak, RANK, COUNT, MIN, MAX, DENSE RANK, LEAD, LAG gibi veri pencereleri ile kullanmak istediğiniz toplamaları tanımlamak için İfade Oluşturucu'yu kullanın.

![Pencere seçenekleri](media/data-flow/windows7.png "windows 7")

İfade Oluşturucu su aracılığıyla ADF Veri Akışı İfade Dili'nde kullanabileceğiniz toplama ve analitik işlevlerin tam listesi burada listelenmiştir: https://aka.ms/dataflowexpressions.

## <a name="next-steps"></a>Sonraki adımlar

Basit bir grup-by toplama arıyorsanız, [Toplu dönüştürme](data-flow-aggregate.md)
