---
title: VM'ler (Önizleme) bilinen sorunlar için Azure İzleyici | Microsoft Docs
description: Bu makalede, Azure 'da sistem durumunu, uygulama bağımlılığı bulmayı ve Azure VM işletim sisteminin performans izlemesini birleştiren VM'ler için Azure İzleyici, Azure 'daki bir çözüm olan bilinen sorunlar ele alınmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/02/2019
ms.openlocfilehash: 711b3707d536c4858578817589670edf0f467b64
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77670738"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>VM'ler (Önizleme) için Azure İzleyici ile ilgili bilinen sorunlar

Bu makalede, Azure 'da sistem durumunu, uygulama bileşenlerini bulmayı ve Azure VM işletim sisteminin performansını izlemeyi birleştiren VM'ler için Azure İzleyici, Azure 'daki bir çözüm olan bilinen sorunlar ele alınmaktadır. 

## <a name="health"></a>Durum 
Aşağıda durum özelliğinin geçerli sürümüyle ilgili bilinen sorunlar verilmiştir:

- Bir Azure VM kaldırılırsa veya silinirse, bir süre için VM listesi görünümünde görüntülenir. Ayrıca, kaldırılan veya silinen bir VM 'nin durumuna tıkladığınızda **sistem durumu tanılama** görünümü açılır ve sonra bir yükleme döngüsü başlatılır. Silinen sanal Makinenin adını seçerek, bir VM silinip silinmediğini belirten ileti ile bir bölme açılır.
- Portalı veya iş yükü İzleyicisi API bunları hemen güncelleştirebilir olsa bile bir eşiği güncelleştirme gibi yapılandırma değişiklikleri, 30 dakika kadar yararlanın. 
- Durum tanılama deneyimi diğer görünümlerden daha hızlı güncelleştirilir. Aralarında geçiş yaptığınızda bilgiler gecikiyor olabilir. 
- Linux VM 'Ler için, tek bir VM görünümüne yönelik durum ölçütlerini listeleme sayfasının başlığı, Kullanıcı tanımlı VM adı yerine sanal makinenin tüm etki alanı adına sahiptir. 
- Desteklenen yöntemlerden birini kullanarak bir VM için izlemeyi devre dışı bıraktıktan sonra yeniden dağıtmayı denerseniz, aynı çalışma alanında dağıtmanız gerekir. Farklı bir çalışma alanı seçer ve bu VM 'nin sistem durumunu görüntülemeyi denerseniz, bu durum tutarsız bir davranış gösterebilir.
- Çalışma alanınızdan çözüm bileşenlerini kaldırdıktan sonra, Azure sanal makinelerinizden sistem durumunu görmeye devam edebilirsiniz; Portalda herhangi bir görünüme gittiğinizde özellikle performans ve eşleme verileri. Veriler bir süre sonra performans ve harita görünümünden görünmeye devam eder; Ancak sistem durumu görünümü sanal makinelerinizin sistem durumunu göstermeye devam edecektir. **Şimdi dene** seçeneği yalnızca performans ve eşleme görünümlerinden yeniden eklenebilir.

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinelerinizin izlenmesini etkinleştirmeye yönelik gereksinimleri ve yöntemleri anlamak için, [VM'ler için Azure izleyici etkinleştir](vminsights-enable-overview.md)' i gözden geçirin.
