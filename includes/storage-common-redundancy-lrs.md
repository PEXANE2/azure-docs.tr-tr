---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/28/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: b4b050befe160d69fbc44b7f0a2ebbbbad3d705a
ms.sourcegitcommit: df7942ba1f28903ff7bef640ecef894e95f7f335
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69015926"
---
Yerel olarak yedekli depolama (LRS), belirli bir yıl boyunca nesnelerin en az% 99,999999999 (11 nines) dayanıklılığı sağlar. LRS, verilerinizi bir depolama ölçek birimine çoğaltarak bu nesne dayanıklılığı sağlar. Depolama hesabınızı oluşturduğunuz bölgede bulunan bir veri merkezi, depolama ölçek birimini barındırır. LRS depolama hesabına yönelik yazma isteği, yalnızca veriler tüm çoğaltmalara yazıldıktan sonra başarıyla döndürülür. Her çoğaltma ayrı hata etki alanlarında ve bir depolama ölçek birimi içindeki etki alanlarında yer alır.

Depolama ölçek birimi, depolama düğümlerinin bir raf koleksiyonudur. Hata etki alanı (FD), fiziksel bir hata birimi temsil eden bir düğüm grubudur. Bir hata etki alanını aynı fiziksel rafa ait düğümler olarak düşünün. Yükseltme etki alanı (UD), bir hizmet yükseltmesi (dağıtım) işlemi sırasında birlikte yükseltilen bir düğüm grubudur. Çoğaltmalar, tek bir depolama ölçek birimi içindeki UDs ve FDs 'ler arasında yayılır. Bu mimari, bir donanım hatası tek bir rafı etkiliyorsa veya bir hizmet yükseltmesi sırasında düğümler yükseltildiğinde verilerinizin kullanılabilir olmasını sağlar.

LRS, en düşük maliyetli çoğaltma seçeneğidir ve diğer seçeneklere kıyasla en az dayanıklılık sağlar. Veri merkezi düzeyinde bir olağanüstü durum oluşursa (örneğin, yangın veya taşması), tüm çoğaltmalar kaybolabilir veya kurtarılamaz olabilir. Bu riski azaltmak için Microsoft, bölgesel olarak yedekli depolama (ZRS), coğrafi olarak yedekli depolama (GRS) veya coğrafi bölge-yedekli depolama (GZRS) kullanmanızı önerir.

* Uygulamanız veri kaybı oluşursa kolayca yeniden oluşturabileceğiniz verileri depoluyorsa LRS 'yi kabul edebilirsiniz.
* Bazı uygulamalar veri idare gereksinimleri nedeniyle yalnızca bir ülke/bölge dahilinde verileri çoğaltmaya kısıtlıdır. Bazı durumlarda, GRS hesapları için verilerin çoğaltılacağı eşleştirilmiş bölgeler başka bir ülkede/bölgede olabilir. Eşleştirilmiş bölgeler hakkında daha fazla bilgi için bkz. [Azure bölgeleri](https://azure.microsoft.com/regions/).
