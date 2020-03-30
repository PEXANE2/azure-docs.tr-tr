---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/27/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 87457bb103f49be4ca3e7bf9f463c5bf63f3a119
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77597846"
---
Azure dosya paylaşımlarınızdaki verileri veri kaybına veya bozulmaya karşı korumak için, tüm Azure dosyası paylaşımları her dosyanın birden çok kopyasını yazıldığı gibi depolar. İş yükünüzün gereksinimlerine bağlı olarak, ek fazlalık dereceleri seçebilirsiniz. Azure Dosyaları şu anda aşağıdaki veri artıklık seçeneklerini destekler:

- **Yerel olarak yedekli**: Genellikle LRS olarak adlandırılan yerel gereksiz depolama alanı, her dosyanın bir Azure depolama kümesinde üç kez depolanması anlamına gelir. Bu, hatalı disk sürücüsü gibi donanım hataları nedeniyle veri kaybına karşı korur.
- **Bölge gereksiz**: Bölge yedekli depolama, genellikle ZRS olarak adlandırılır, her dosyaüç farklı Azure depolama kümeleri arasında üç kez depolanır anlamına gelir. Üç farklı Azure depolama kümesi, her biri dosyayı üç kez saklar ( tıpkı yerel olarak yedekli depolama da olduğu gibi ve farklı Azure *kullanılabilirlik bölgelerinde*fiziksel olarak yalıtılır). Kullanılabilirlik bölgeleri, Azure bölgesindeki benzersiz fiziksel konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Depolamaya yazma, üç kullanılabilirlik bölgesinde depolama kümelerine yazılmadan kabul edilmez. 
- **Coğrafi yedekli**: Genellikle GRS olarak adlandırılan coğrafi yedekli depolama, bir dosyanın birincil bölgedeki bir Azure depolama kümesiiçinde üç kez depolanması yla yerel olarak yedekli depolama gibidir. Tüm yazılar daha sonra eşzamanlı olarak Microsoft tanımlı ikincil bir bölgeye çoğaltılır. Coğrafi yedekli depolama, verilerinizin iki Azure bölgesi arasında 6 kopyasını nsağlar. Bir Azure bölgesinin doğal afet veya benzeri bir olay nedeniyle kalıcı olarak kaybolması gibi büyük bir felaket durumunda, Microsoft, ikincil etkinin tüm işlemlere hizmet veren birincil olacak şekilde bir hata gerçekleştirmesi durumunda. Birincil ve ikincil bölgeler arasındaki çoğaltma eşzamanlı olduğundan, büyük bir felaket durumunda, ikincil bölgeye henüz çoğaltılmayan veriler kaybolur. Ayrıca, coğrafi yedekli bir depolama hesabının el ile başarısız lığı da gerçekleştirebilirsiniz.
- **Coğrafi bölge yedekli**: Genellikle GZRS olarak adlandırılan coğrafi bölge yedekli depolama, bir dosyanın birincil bölgedeki 3 farklı depolama kümesinde dokuz kez depolanması yla bölge yedekli depolama gibidir. Tüm yazılar daha sonra eşzamanlı olarak Microsoft tanımlı ikincil bir bölgeye çoğaltılır. Coğrafi bölge yedekli depolama için arıza işlemi, coğrafi yedekli depolamaiçin olduğu gibi çalışır.

Standart Azure dosya paylaşımları dört artıklık türünü desteklerken, premium Azure dosya paylaşımları yalnızca yerel olarak yedekli ve bölge yedekli depolamayı destekler.

Genel amaçlı sürüm 2 (GPv2) depolama hesapları Azure Dosyaları tarafından desteklenmeyen iki ek artıklık seçeneği sağlar: genellikle RA-GRS olarak adlandırılan erişilebilir coğrafi yedekli depolamayı okuyun ve genellikle erişilebilir coğrafi bölge yedekli depolamayı okuyun RA-GZRS olarak adlandırılır. Azure dosya paylaşımlarını depolama hesaplarında bu seçenekler ayarlanmış olarak sağlayabilirsiniz, ancak Azure Dosyaları ikincil bölgeden okumayı desteklemez. Okunabilir coğrafi veya coğrafi bölge yedekli depolama hesaplarına dağıtılan Azure dosya paylaşımları, sırasıyla coğrafi yedekli veya coğrafi bölge yedekli depolama olarak faturalandırılır.