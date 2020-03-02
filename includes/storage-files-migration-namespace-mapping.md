---
title: Bir klasör yapısını Azure Dosya Eşitleme topolojisine eşleme
description: Azure Dosya Eşitleme ile kullanmak için mevcut bir dosya ve klasör yapısını Azure dosya paylaşımlarına eşleme. Geçiş belgeleri arasında paylaşılan ortak bir metin bloğu.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 44fb95de88cc86c802e5ba72f0b5379b8708c506
ms.sourcegitcommit: 5192c04feaa3d1bd564efe957f200b7b1a93a381
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2020
ms.locfileid: "78209564"
---
Bu adımda, kaç tane Azure dosya paylaşımının gerekli olduğunu değerlendirirsiniz. Tek bir Windows Server (veya Cluster), 30 adede kadar Azure dosya paylaşımına eşitlenebilir.

StorSimple üzerinde, kullanıcılarınız ve uygulamalarınız için bir SMB paylaşımı olarak yerel olarak paylaştığınız daha fazla klasör olabilir. En kolay bir Azure dosya paylaşımıyla 1:1 eşlemek üzere şirket içi bir paylaşım için adım adım olacaktır. Bu sayı, tek bir Windows Server için 30 ' un altında, yani iki Windows sunucusu (60) ile aynı şekilde çalışıyorsanız, bir 1:1 eşlemesinin kullanılması önerilir.

30 ' dan fazla paylaşımınız varsa, şirket içi paylaşım 1:1 ' i bir Azure dosya paylaşımına eşleştirmek genellikle gereksizdir.
Aşağıdaki seçenekleri göz önünde bulundurun:

#### <a name="share-grouping"></a>Gruplamayı paylaşma

Örneğin, HR departmanınızda toplam 15 paylaşım varsa, tüm IK verileri tek bir Azure dosya paylaşımında depolamayı düşünebilirsiniz. Çoklu şirket içi paylaşımların tek bir Azure dosya paylaşımında depolanması, yerel Windows sunucunuzda olağan 15 SMB paylaşımlarını oluşturmanızı engellemez. Yalnızca bu 15 paylaşımların kök klasörlerini ortak bir klasör altında alt klasör olarak düzenlediğiniz anlamına gelir. Daha sonra bu ortak klasörü bir Azure dosya paylaşımıyla eşitleyebilirsiniz. Bu şekilde, bu şirket içi paylaşımlar grubu için bulutta yalnızca tek bir Azure dosya paylaşımı gereklidir.

#### <a name="volume-sync"></a>Birim eşitleme

Azure Dosya Eşitleme, bir birimin kökünün bir Azure dosya paylaşımında eşitlenmesini destekler.
Kök klasörünü eşitlediğiniz zaman, tüm alt klasörler ve dosyalar aynı Azure dosya paylaşımında sona alınacaktır.

#### <a name="other-best-practices-to-consider"></a>Dikkate alınması gereken diğer en iyi uygulamalar

Sunucu başına 30 Azure dosya paylaşma eşitleme sınırı dışında, önde gelen değerlendirme, eşitlemenin veriminden oluşur.

Sunucunuzda her biri kendi Azure dosya paylaşımıyla eşitlenen birden çok paylaşım olduğunda, eşitleme tüm bunlar için paralel olarak çalışabilir. Ölçek vektörü, bir eşitleme kapsamındaki tüm dosyaların boyutu değildir. Bu, işlenmesi gereken öğelerin (dosya ve klasör) sayısıdır.

Tek bir Azure dosya paylaşımında en fazla 100 TiB bulunabilir.
Azure Dosya Eşitleme, her Azure dosya paylaşımında 100.000.000 öğenin eşitlenmesini destekler.

Kök birimin eşitleniyor, her zaman en iyi yanıt olmayacaktır. Birden çok konumu eşitlerken yarar vardır. bunu yapmak, eşitleme kapsamı başına öğe sayısını daha düşük tutmaya yardımcı olur. Azure dosya eşitlemesini daha az sayıda öğe ile ayarlamak yalnızca eşitleme için önemli değildir, ancak aynı zamanda, sunucunuzu kaybetmeniz ve aynı Azure fil 'e bağlanan yeni bir tane sağlanması durumunda olağanüstü durum kurtarma hızını da faydalanır. e paylaşımlar.

Kaç tane Azure dosya paylaşımına ihtiyacınız olduğunu ve mevcut StorSimple verilerinizin hangi bölümlerinin Azure dosya paylaşımında biteceğini belirlemede yardımcı olması için yukarıdaki kavramların birleşimini kullanın.

Bir sonraki adımda, düşüncelerinizi kaydeden bir liste oluşturun. Aynı anda çok sayıda kaynak sağlarken eşleme planınızın ayrıntılarını kaybetmek kolay olduğundan, burada düzenlenmiş olarak kalmamak önemlidir.
