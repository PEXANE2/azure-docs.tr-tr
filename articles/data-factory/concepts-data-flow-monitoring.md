---
title: Haritalama veri akışı Görsel İzleme
description: Azure Veri Fabrikası Veri Akışları görsel olarak nasıl izlenir?
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/07/2019
ms.openlocfilehash: b06000d6b3cd7a429b7ad0fc9c453595f21bd893
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418482"
---
# <a name="monitor-data-flows"></a>Veri Akışlarını İzle

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Veri akışınızı oluşturmayı ve hata ayıklamaişlemini tamamladıktan sonra, veri akışınızı bir ardışık alan bağlamında bir zamanlamada yürütmek üzere zamanlamak isteyeceksiniz. Tetikler'i kullanarak azure veri fabrikasından ardışık zamanlayabilirsiniz. Veya veri akışınızı boru hattı bağlamında sınamak için tek bir yürütme yürütmek için Azure Veri Fabrikası Boru Hattı Oluşturucusu'ndan Tetikle Şimdi seçeneğini kullanabilirsiniz.

Ardışık hattınızı çalıştırdığınızda, veri akışınız etkinliği de dahil olmak üzere ardışık ardışık ardışık ardışık alanda bulunan tüm etkinlikleri izleyebilirsiniz. Sol daki Azure Veri Fabrikası Kullanıcı Birası panelindeki monitör simgesine tıklayın. Aşağıdakine benzer bir ekran göreceksiniz. Vurgulanan simgeler, Veri Akışı etkinliği de dahil olmak üzere ardışık ardışık alandaki etkinlikleri ayrıntılı olarak ele almanızı sağlar.

![Veri Akışı İzleme](media/data-flow/mon001.png "Veri Akışını İzleme")

Çalışma süreleri ve durumu da dahil olmak üzere bu düzeyde istatistikleri göreceksiniz. Etkinlik düzeyindeki Run Kimliği, çalıştır kimliğinin ardışık hat lar düzeyinde olması yla farklıdır. Önceki düzeydeki Run Kimliği boru hattı içindir. Gözlük tıklattığınızda veri akışı yürütme hakkında derin ayrıntılar verecektir.

![Veri Akışı İzleme](media/data-flow/mon002.png "Veri Akışını İzleme")

Grafik düğüm izleme görünümünde olduğunuzda, veri akışı grafiğinizin yalnızca basitleştirilmiş bir görünüm sürümünü görürsünüz.

![Veri Akışı İzleme](media/data-flow/mon003.png "Veri Akışını İzleme")

## <a name="view-data-flow-execution-plans"></a>Veri Akışı Yürütme Planlarını Görüntüle

Veri Akışınız Spark'ta yürütüldüğünde, Azure Veri Fabrikası veri akışınızın tamamını temel alan en iyi kod yollarını belirler. Ayrıca, yürütme yolları farklı ölçekçıkış düğümleri ve veri bölümleri oluşabilir. Bu nedenle, izleme grafiği dönüşümlerinizin yürütme yolunu dikkate alarak akışınızın tasarımını temsil eder. Tek tek düğümleri tıklattığınızda, kümede birlikte yürütülen kodu temsil eden "gruplandırmalar" görürsünüz. Gördüğünüz zamanlamalar ve sayımlar, tasarımınızdaki tek tek adımların aksine bu grupları temsil eder.

![Veri Akışı İzleme](media/data-flow/mon004.png "Veri Akışını İzleme")

* İzleme penceresindeki açık alana tıkladığınızda, alt bölmedeki istatistikler her Lavabo için zamanlama ve satır sayımlarını ve dönüşüm soyundan gelen lavabo verilerine yol açan dönüşümleri görüntüler.

* Bireysel dönüşümleri seçtiğinizde, sağ panelde bölüm istatistikleri, sütun sayıları, çarpıklık (bölümler arasında dağıtılan veriler ne kadar eşittir) ve kurtosis (veri ne kadar dikenli) gösteren ek geri bildirim alırsınız.

* Düğüm görünümünde Lavabo'ya tıkladığınızda sütun soylarını görürsünüz. Lavaboya inmek için veri akışınız boyunca sütunların biriktiğinüç farklı yöntem vardır. Bunlar:

  * Hesaplanan: Sütunu koşullu işleme için veya veri akışınızdaki bir ifade içinde kullanırsınız, ancak onu Lavaboya koymayın
  * Türetilmiş: Sütun, akışınızda oluşturduğunuz yeni bir sütundur, yani Kaynak'ta bulunmaz
  * Eşlenen: Sütun kaynaktan kaynaklanan ve bir lavabo alanı için eşleme vardır
  * Veri akışı durumu: Yürütmenizin geçerli durumu
  * Küme başlatma süresi: Veri akışı yürütmeniz için JIT Spark bilgi işlem ortamını elde etmek için gereken süre
  * Dönüşüm sayısı: Akışınızda kaç dönüşüm adımı yürütülüyor
  
![Veri Akışı İzleme](media/data-flow/monitornew.png "Veri Akışı İzleme Yeni")  
  
## <a name="monitor-icons"></a>Monitör Simgeleri

Bu simge, dönüştürme verilerinin kümeüzerinde zaten önbelleğe alınmış olduğu anlamına gelir, bu nedenle zamanlamalar ve yürütme yolu bunu dikkate almıştır:

![Veri Akışı İzleme](media/data-flow/mon004.png "Veri Akışını İzleme")

Dönüşümde yeşil daire simgeleri de göreceksiniz. Bunlar, verilerin aktığı lavabo sayısının sayısını temsil eder.
