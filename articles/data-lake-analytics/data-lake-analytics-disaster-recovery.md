---
title: Azure Veri Gölü Analitiği için olağanüstü durum kurtarma kılavuzu
description: Azure Veri Gölü Analizi hesaplarınız için olağanüstü durum kurtarma yı nasıl planlayınız öğrenin.
services: data-lake-analytics
author: MikeRys
ms.author: mrys
ms.reviewer: jasonwhowell
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: f9b22e6b806f76189134ec63c83d48f48bf95587
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73889772"
---
# <a name="disaster-recovery-guidance-for-azure-data-lake-analytics"></a>Azure Veri Gölü Analitiği için olağanüstü durum kurtarma kılavuzu

Azure Data Lake Analytics, büyük verileri kolaylaştıran, isteğe bağlı bir analiz işi hizmetidir. Donanım dağıtma, yapılandırma ve ayarlama işlemleri yerine, verilerinizi dönüştürmek ve değerli öngörüleri ayıklamak için sorgular yazarsınız. Analiz hizmeti sadece ne kadar güce ihtiyacınız olduğunu ayarlayarak her ölçekteki işin üstesinden gelmenizi sağlar. Yalnızca işiniz çalıştırıldığında ücret ödersiniz; bu da hizmeti uygun maliyetli kılar. Bu makalede, bölge genelinde nadir kesintilere veya yanlışlıkla silmelere karşı işlerinizi nasıl koruyacağınız konusunda rehberlik verilmektedir.

## <a name="disaster-recovery-guidance"></a>Olağanüstü durum kurtarma kılavuzu

Azure Veri Gölü Analizi'ni kullanırken, kendi olağanüstü durum kurtarma planınızı hazırlamanız çok önemlidir. Bu makalede, bir felaket kurtarma planı oluşturmak için size rehberlik eder. Kendi planınızı oluşturmanıza yardımcı olabilecek ek kaynaklar vardır:
- [Azure uygulamaları için hata ve olağanüstü durum kurtarma](/azure/architecture/reliability/disaster-recovery)
- [Azure dayanıklılık teknik kılavuzu](/azure/architecture/checklist/resiliency-per-service)

## <a name="best-practices-and-scenario-guidance"></a>En iyi uygulamalar ve senaryo kılavuzu

U-SQL tablolarını ve yapılandırılmamış verileri okuyan ve yazan bir bölgede, ADLA hesabında yinelenen bir U-SQL işini çalıştırabilirsiniz.  Şu adımları atarak bir felakete hazırlanın:

1. Kesinti sırasında kullanılacak ikincil bölgede ADLA ve ADLS hesapları oluşturun.

   > [!NOTE]
   > Hesap adları genel olarak benzersiz olduğundan, hangi hesabın ikincil olduğunu gösteren tutarlı bir adlandırma düzeni kullanın.

2. Yapılandırılmamış veriler [için, Azure Veri Gölü Depolama Gen1'deki veriler için Olağanüstü Durum kurtarma kılavuzuna](../data-lake-store/data-lake-store-disaster-recovery-guidance.md) başvurun

3. ADLA tablolarında ve veritabanlarında depolanan yapılandırılmış veriler için veritabanları, tablolar, tablo değerli işlevler ve derlemeler gibi meta veri yapılarının kopyalarını oluşturun. Üretimde değişiklikler olduğunda bu yapıları düzenli olarak yeniden eşitlemeniz gerekir. Örneğin, yeni eklenen verilerin verileri kopyalayıp ikincil tabloya ekleyerek ikincil bölgeye çoğaltılması gerekir.

   > [!NOTE]
   > Bu nesne adları ikincil hesaba kapsamlıdır ve genel olarak benzersiz değildir, bu nedenle birincil üretim hesabındaki adlara sahip olabilirler.

Bir kesinti sırasında, giriş yollarının ikincil bitiş noktasını işaret etmesi için komut dosyalarınızı güncelleştirmeniz gerekir. Daha sonra kullanıcılar işlerini ikincil bölgedeki ADLA hesabına gönderirler. İşin çıktısı daha sonra ikincil bölgedeki ADLA ve ADLS hesabına yazılacaktır.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Veri Gölü Depolama Gen1'deki veriler için olağanüstü durum kurtarma kılavuzu](../data-lake-store/data-lake-store-disaster-recovery-guidance.md)
