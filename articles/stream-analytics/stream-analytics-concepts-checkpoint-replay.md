---
title: Azure Akış Analizi'nde kurtarma kavramlarını kontrol noktası ve yeniden oynatma
description: Bu makalede, Azure Akış Analizi'nde denetim noktası ve yeniden iş kurtarma kavramları açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: f5bb2b97d7da770828c2f4f03167483ad2044c79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75426401"
---
# <a name="checkpoint-and-replay-concepts-in-azure-stream-analytics-jobs"></a>Azure Akış Analizi işlerinde denetim noktası ve yeniden oynatma kavramları
Bu makalede, Azure Akış Analizi'ndeki dahili denetim noktası ve yeniden oynatma kavramları ve bunların iş kurtarma üzerindeki etkisi açıklanmaktadır. Bir Akış Analizi işi her çalıştığında, durum bilgileri dahili olarak korunur. Bu durum bilgileri düzenli olarak bir denetim noktasına kaydedilir. Bazı senaryolarda, iş hatası veya yükseltme oluşursa, denetim noktası bilgileri iş kurtarma için kullanılır. Diğer durumlarda, denetim noktası kurtarma için kullanılamaz ve bir yeniden oynatma gereklidir.

## <a name="stateful-query-logicin-temporal-elements"></a>Zamansal öğelerde durumlu sorgu mantığı
Azure Akış Analizi işinin benzersiz yeteneklerinden biri, pencereli toplamalar, zamansal birleştirmeler ve zamansal analitik işlevler gibi durum sallanabilir işlemler gerçekleştirmektir. Bu işleçlerin her biri, iş çalıştığında durum bilgilerini tutar.Bu sorgu öğeleri için en büyük pencere boyutu yedi gündür. 

Zamansal pencere kavramı birkaç Akış Analizi sorgu öğesinde görünür:
1. Pencereli agregalar (GROUP BY yuvarlanma, atlamalı ve sürgülü pencereler)

2. Zamansal birleştirmeler (DATEDIFF ile Birleşin)

3. Zamansal analitik fonksiyonlar (LIMIT SÜRELİ ISFIRST, LAST ve LAG)


## <a name="job-recovery-from-node-failure-including-os-upgrade"></a>İşletim sistemi yükseltmesi de dahil olmak üzere düğüm hatasından kaynaklanan iş kurtarma
Bir Akış Analizi işi her çalıştığında, dahili olarak birden çok alt düğümde çalışmak üzere ölçeklendirilir. Her işçi düğümün durumu birkaç dakikada bir kontrol edilir, bu da bir hata oluşursa sistemin kurtarılmasına yardımcı olur.

Bazen, belirli bir alt düğüm başarısız olabilir veya bu alt düğüm için bir İşletim Sistemi yükseltmesi oluşabilir. Otomatik olarak kurtarmak için, Stream Analytics yeni bir sağlıklı düğüm edinir ve önceki alt düğümün durumu kullanılabilir en son denetim noktasından geri yüklenir. Çalışmaya devam etmek için, denetim noktasının alındığı andan itibaren durumu geri yüklemek için az miktarda yeniden oynatma gerekir. Genellikle, geri yükleme boşluğu yalnızca birkaç dakikadır. İş için yeterli Akış Birimi seçildiğinde, yeniden oynatma nın hızlı bir şekilde tamamlanması gerekir. 

Tamamen paralel bir sorguda, bir alt düğüm hatasından sonra yetişmek için gereken süre aşağıdakiyle orantılıdır:

[giriş olay hızı] x [boşluk uzunluğu] / [işlem bölümleri sayısı]

Düğüm hatası ve işletim sistemi yükseltmesi nedeniyle önemli bir işlem gecikmesi gözlemlediyseniz, sorguyu tamamen paralel yapmayı düşünün ve daha fazla Akış Birimi ayırmak için işi ölçeklendirin. Daha fazla bilgi [için, iş akışını artırmak için Azure Akış Analizi işini ölçeklendir'e](stream-analytics-scale-jobs.md)bakın.

Geçerli Akış Analizi, bu tür bir kurtarma işlemi gerçekleşirken bir rapor göstermez.

## <a name="job-recovery-from-a-service-upgrade"></a>Hizmet yükseltmesinden iş kurtarma 
Microsoft, Azure hizmetinde Akış Analizi işlerini çalıştıran ikili leri zaman zaman yükseltir. Bu zamanlarda, kullanıcıların çalışan işleri yeni sürüme yükseltilir ve iş otomatik olarak yeniden başlatılır. 

Şu anda, kurtarma denetim noktası biçimi yükseltmeleri arasında korunmuyor. Sonuç olarak, akış sorgusunun durumu tamamen yeniden oynatma tekniği kullanılarak geri yüklenmelidir. Stream Analytics işlerinin öncekiyle aynı girişi yeniden oynatmasına izin vermek için, kaynak verilerin bekletme ilkesini sorgunuzdaki en az pencere boyutlarına ayarlamak önemlidir. Kaynak veriler tam pencere boyutunu içerecek kadar geriye tutulamadığından, bunu yapmamak hizmet yükseltmesi sırasında yanlış veya kısmi sonuçlara neden olabilir.

Genel olarak, gerekli yeniden oynatma miktarı, ortalama olay hızıyla çarpılarak açılan pencerenin boyutuyla orantılıdır. Örnek olarak, saniyede 1000 olay giriş hızına sahip bir iş için, bir saatten büyük bir pencere boyutu büyük bir yeniden oynatma boyutuna sahip olarak kabul edilir. Tam ve doğru sonuçlar üretebilmek için durumu başlatması için bir saate kadar verinin yeniden işlenmesi gerekebilir, bu da uzun bir süre için gecikmiş çıktıya (çıktı yok) neden olabilir. Penceresi olmayan sorgular veya diğer `JOIN` zamansal işleçler, örneğin, `LAG`sıfır yeniden oynatma olurdu.

## <a name="estimate-replay-catch-up-time"></a>Yeniden oynatma yakalama süresini tahmin edin
Bir hizmet yükseltmesi nedeniyle gecikmenin uzunluğunu tahmin etmek için aşağıdaki tekniği uygulayabilirsiniz:

1. Giriş Olay Hub'ını, sorgunuzdaki en büyük pencere boyutunu beklenen olay hızıyla kapsayacak kadar yeterli veriyle yükleyin. Olayların zaman damgası, canlı giriş akışı gibi, bu süre boyunca duvar saati zamanına yakın olmalıdır. Örneğin, sorgunuzda 3 günlük bir süreniz varsa, olayları üç gün süreyle Olay Merkezi'ne gönderin ve etkinlik göndermeye devam edin. 

2. Başlangıç saati olarak **Şimdi'yi** kullanarak işe başlayın. 

3. Başlangıç saati ile ilk çıktının ne zaman oluşturulduğu arasındaki zamanı ölçün. Bir hizmet yükseltmesi sırasında işin ne kadar gecikmeye maruz kalacağının zamanı zordur.

4. Gecikme çok uzunsa, işinizi bölmeye çalışın ve SUs sayısını artırmak, böylece yük daha fazla düğümlere yayılır. Alternatif olarak, sorgunuzdaki pencere boyutlarını azaltmayı düşünün ve akış aşağı lavabosundaki Stream Analytics işi tarafından üretilen çıktıda (örneğin Azure SQL veritabanını kullanarak) daha fazla toplama veya diğer durumlu işlemler gerçekleştirin.

Görev açısından kritik işlerin yükseltimi sırasında genel hizmet kararlılığı yla ilgili endişeler için, eşleştirilmiş Azure bölgelerinde yinelenen işler çalıştırmayı düşünün. Daha fazla bilgi [için, hizmet güncellemeleri sırasında Garanti Akışı Analizi iş güvenilirliği](stream-analytics-job-reliability.md)bölümüne bakın.

## <a name="job-recovery-from-a-user-initiated-stop-and-start"></a>Bir kullanıcıdan başlatılan iş kurtarma durdurma ve başlatma
Akış işinde Sorgu sözdizimini düzenlemek veya giriş ve çıktıları ayarlamak için, değişiklikleri yapmak ve iş tasarımını yükseltmek için işin durdurulması gerekir. Bu tür senaryolarda, bir kullanıcı akış işini durdurup yeniden başlattığında, kurtarma senaryosu hizmet yükseltmesine benzer. 

Denetim noktası verileri, kullanıcı tarafından başlatılan iş yeniden başlatma için kullanılamaz. Böyle bir yeniden başlatma sırasında çıktının gecikmesini tahmin etmek için, önceki bölümde açıklandığı gibi aynı yordamı kullanın ve gecikme çok uzunsa benzer azaltma uygulayın.

## <a name="next-steps"></a>Sonraki adımlar
Güvenilirlik ve ölçeklenebilirlik hakkında daha fazla bilgi için şu makalelere bakın:
- [Öğretici: Azure Akış Analizi işleri için uyarılar ayarlama](stream-analytics-set-up-alerts.md)
- [İş verisini artırmak için Azure Akış Analizi işini ölçeklendirin](stream-analytics-scale-jobs.md)
- [Hizmet güncellemeleri sırasında Stream Analytics iş güvenilirliğini garanti edin](stream-analytics-job-reliability.md)
