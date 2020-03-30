---
title: Akıllı gruplar
description: Akıllı Gruplar, uyarı gürültüsünü azaltmanıza yardımcı olan uyarı ların toplayıcılarıdır
ms.topic: conceptual
ms.subservice: alerts
ms.date: 05/15/2018
ms.openlocfilehash: 05b05f8bc079bb3768ac2f1a03593bc9260b41aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77665502"
---
# <a name="smart-groups"></a>Akıllı gruplar

Uyarılarla uğraşırken karşılaşılan ortak bir sorun, gerçekte neyin önemli olduğunu bulmak için gürültüyü elemektir - akıllı gruplar bu soruna çözüm olarak tasarlanmıştır.  

Akıllı gruplar, tek bir sorunu temsil eden ilgili uyarıları birleştirmek için makine öğrenme algoritmaları kullanılarak otomatik olarak oluşturulur.  Bir uyarı oluşturulduğunda, algoritma bunu geçmiş desenler, benzer özellikler ve benzer yapı gibi bilgilere dayalı olarak yeni bir akıllı gruba veya varolan bir akıllı gruba ekler. Örneğin, bir abonelikteki birkaç sanal makinede % CPU aynı anda birçok bireysel uyarıya yol açarsa ve bu tür uyarılar geçmişte herhangi bir zamanda birlikte oluştuysa, bu uyarılar büyük olasılıkla tek bir Smart Group'ta gruplandırılır ve potansiyel ortak kök neden. Bu, bir birinin sorun giderme uyarıları için, akıllı grupların yalnızca ilgili uyarıları tek bir toplu birim olarak yöneterek gürültüyü azaltmalarına izin vermekle kaçtığı, aynı zamanda onları uyarıları için olası ortak kök nedenlerine yönlendiren anlamına gelir.

Şu anda algoritma yalnızca bir abonelik içindeki aynı izleme hizmetinden gelen uyarıları dikkate alır. Akıllı gruplar bu konsolidasyon sayesinde uyarı gürültüsünün %99'a kadarını azaltabilir. Akıllı grup ayrıntıları sayfasında uyarıların bir gruba dahil edilmesinin nedenini görüntüleyebilirsiniz.

Akıllı grupların ayrıntılarını görüntüleyebilir ve durumu uyarılarla nasıl yapabileceğinize benzer şekilde ayarlayabilirsiniz. Her uyarı bir ve tek akıllı grubun üyesidir. 

## <a name="smart-group-state"></a>Akıllı grup durumu

Akıllı grup durumu, çözüm sürecini akıllı grup düzeyinde yönetmenize olanak tanıyan uyarı durumuna benzer bir kavramdır. Uyarı durumuna benzer şekilde, akıllı bir grup oluşturulduğunda, **Onaylanan** veya **Kapalı**olarak değiştirilebilen **Yeni** durum vardır.

Aşağıdaki akıllı grup durumları desteklenir.

| Durum | Açıklama |
|:---|:---|
| Yeni | Sorun yeni algılandı ve henüz gözden geçirilmedi. |
| Onaylandı | Bir yönetici akıllı grubu gözden geçirdi ve üzerinde çalışmaya başladı. |
| Closed | Sorun çözüldü. Akıllı bir grup kapatıldıktan sonra, başka bir duruma değiştirerek yeniden açabilirsiniz. |

[Akıllı grubunuzun durumunu nasıl değiştireceğinizi öğrenin.](https://aka.ms/managing-alert-smart-group-states)

> [!NOTE]
>  Akıllı bir grubun durumunu değiştirmek, tek tek üye uyarılarının durumunu değiştirmez.

## <a name="smart-group-details-page"></a>Akıllı grup ayrıntıları sayfası

Akıllı grup ayrıntı sayfası, akıllı bir grup seçtiğinizde görüntülenir. Grubu oluşturmak için kullanılan mantık da dahil olmak üzere akıllı grup hakkında ayrıntılar sağlar ve durumunu değiştirmenize olanak tanır.
 
![Akıllı grup detayı](media/alerts-smartgroups-overview/smart-group-detail.png)


Akıllı grup ayrıntı sayfası aşağıdaki bölümleri içerir.

| Section | Açıklama |
|:---|:---|
| Uyarılar | Akıllı gruba dahil edilen tek tek uyarıları listeler. Uyarı ayrıntı sayfasını açmak için bir uyarı seçin. |
| Geçmiş | Akıllı grup tarafından gerçekleştirilen her eylemi ve bu grupta yapılan değişiklikleri listeler. Bu, şu anda durum değişiklikleri ve uyarı üyelik değişiklikleri ile sınırlıdır. |

## <a name="smart-group-taxonomy"></a>Akıllı grup taksonomi

Akıllı bir grubun adı ilk uyarısının adıdır. Akıllı bir grup oluşturamaz veya yeniden adlandıramazsınız.

## <a name="next-steps"></a>Sonraki adımlar

- [Akıllı grupları yönetme](https://aka.ms/managing-smart-groups)
- [Uyarınızı ve akıllı grup durumunuzu değiştirme](https://aka.ms/managing-alert-smart-group-states)


