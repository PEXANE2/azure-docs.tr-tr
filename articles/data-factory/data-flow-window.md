---
title: Azure Data Factory eşleme veri akışı pencere dönüştürmesi
description: Azure Data Factory eşleme veri akışı pencere dönüştürmesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: cf58db14398026da62ed13c66be815be8fe166a2
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029895"
---
# <a name="azure-data-factory-window-transformation"></a>Azure Data Factory pencere dönüştürmesi



Pencere dönüştürmesi, veri akışlarınızın pencere tabanlı toplamalarını tanımlayacaksınız. Ifade Oluşturucu 'da, müşteri adayı, öteleme, NTILE, CUMEDIST, derece, vb. gibi veri veya zaman pencerelerini (SQL OVER yan tümcesi) temel alan farklı toplamalar türlerini tanımlayabilirsiniz. Çıktındaki bu toplamaları içeren yeni bir alan oluşturulacaktır. İsteğe bağlı group by alanlarını da dahil edebilirsiniz.

![Pencere seçenekleri](media/data-flow/windows1.png "Windows 1")

## <a name="over"></a>Avantajları
Pencere dönüştürmelerinizin sütun verilerinin bölümlenmesini ayarlayın. SQL eşdeğeri SQL içindeki Over yan tümcesindeki ```Partition By``` ' dır. Bölümleme için kullanmak üzere bir hesaplama oluşturmak veya bir ifade oluşturmak isterseniz, bunu sütun adının üzerine gelerek ve "hesaplanan sütun" seçeneğini belirleyerek yapabilirsiniz.

![Pencere seçenekleri](media/data-flow/windows4.png "Windows 4")

## <a name="sort"></a>Sırala
Over yan tümcesinin başka bir bölümü ```Order By``` olarak ayarlanıyor. Bu, veri sıralama sıralamasını ayarlar. Ayrıca, bu sütun alanında sıralama için bir hesaplama değeri için bir ifade oluşturabilirsiniz.

![Pencere seçenekleri](media/data-flow/windows5.png "Windows 5")

## <a name="range-by"></a>Aralık ölçütü
Sonra pencere çerçevesini sınırsız veya sınırlı olarak ayarlayın. Sınırsız bir pencere çerçevesi ayarlamak için, her iki uçta da kaydırıcıyı sınırsız olarak ayarlayın. Sınırsız ve geçerli satır arasında bir ayar seçerseniz, başlangıç ve bitiş değerlerini uzaklığa göre ayarlamanız gerekir. Her iki değer de pozitif tamsayılar olacaktır. Verilerinizin göreli numaralarını veya değerlerini kullanabilirsiniz.

Pencere kaydırıcısının ayarlanacak iki değeri vardır: geçerli satırdan önceki değerler ve geçerli satırdan sonraki değerler. Başlangıç ve bitiş boşluğu, kaydırıcıda iki seçicinin eşleşir.

![Pencere seçenekleri](media/data-flow/windows6.png "Windows 6")

## <a name="window-columns"></a>Pencere sütunları
Son olarak, SıRALAMA, sayı, MIN, MAX, yoğun sıra, müşteri adayı, GECIKME, vb. gibi veri pencereleri ile kullanmak istediğiniz toplamaları tanımlamak için Ifade oluşturucusunu kullanın.

![Pencere seçenekleri](media/data-flow/windows7.png "Windows 7")

Ön planda, ADF veri akışı Ifade dilinde kullanabileceğiniz toplama ve analitik işlevlerin tam listesi aşağıda listelenmiştir: https://aka.ms/dataflowexpressions.

## <a name="next-steps"></a>Sonraki adımlar

Basit bir grup toplamayı arıyorsanız, [toplu dönüştürmeyi](data-flow-aggregate.md) kullanın
