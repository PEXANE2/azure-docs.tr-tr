---
title: Akıllı gruplar
description: Akıllı gruplar, uyarı gürültüsünü azaltmanıza yardımcı olan uyarıların toplamalarına sahiptir
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: robb
ms.subservice: alerts
ms.openlocfilehash: 08657163eb170b8d3185ce15bd724f1f55bbe2bd
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71702866"
---
# <a name="smart-groups"></a>Akıllı gruplar
Uyarı ile ilgilenirken sık görülen bir zorluk, önemli olan ve akıllı grupların bu soruna çözüm olarak ne amaçla düşünülbildiğini öğrenmek için gürültü aracılığıyla karmaşıklaştırır.  

Akıllı gruplar, tek bir sorunu temsil eden ilgili uyarıları birleştirmek için makine öğrenimi algoritmaları kullanılarak otomatik olarak oluşturulur.  Bir uyarı oluşturulduğunda, algoritma onu yeni bir akıllı gruba veya geçmiş desenler, benzer özellikler ve benzer yapı gibi bilgilere göre mevcut bir akıllı gruba ekler. Örneğin, bir abonelikteki birkaç sanal makinede bulunan% CPU aynı anda birden çok ayrı uyarı ile önde dursa ve bu uyarılar geçmişte bir yerde gerçekleştiyse, bu uyarılar büyük olasılıkla tek bir akıllı grupta gruplandırılır ve bu durum, olası genel kök nedeni. Diğer bir deyişle, bir sorun giderme uyarısı için, akıllı gruplar yalnızca ilgili uyarıları tek bir toplu birim olarak yöneterek paraziti azaltmalarına izin vermez, bu da bunlara, uyarıları için olası ortak ana nedenlerle kılavuzluk eder.

Şu anda algoritma yalnızca bir abonelik içindeki aynı izleyici hizmetinden gelen uyarıları kabul eder. Akıllı gruplar, bu birleştirme sırasında en fazla% 99 uyarı gürültüsünü azaltabilir. Uyarıların bir gruba dahil edildiğini akıllı Grup ayrıntıları sayfasında görebilirsiniz.

Akıllı grupların ayrıntılarını görüntüleyebilir ve durumu benzer şekilde uyarılarla birlikte ayarlayabilirsiniz. Her uyarı bir ve yalnızca bir akıllı grubun üyesidir. 

## <a name="smart-group-state"></a>Akıllı Grup durumu
Akıllı Grup durumu, uyarı durumuna yönelik benzer bir kavramdır ve bu, çözümleme işlemini akıllı bir grup düzeyinde yönetmenize olanak sağlar. Uyarı durumuna benzer şekilde, bir akıllı grup oluşturulduğunda, bu, **onaylanan** veya **Kapatılan**olarak değiştirilebilen **Yeni** bir durum içerir.

Aşağıdaki akıllı grup durumları desteklenir.

| Eyalet | Açıklama |
|:---|:---|
| Yeni | Sorun algılandı ve henüz gözden geçirilmedi. |
| Mazsa | Bir yönetici akıllı grubu incelendi ve üzerinde çalışmaya başladı. |
| Kapandı | Sorun çözüldü. Bir akıllı grup kapatıldıktan sonra, başka bir durumla değiştirerek dosyayı yeniden açabilirsiniz. |

[Akıllı grubunuzun durumunu değiştirme hakkında bilgi edinin.](https://aka.ms/managing-alert-smart-group-states)

> [!NOTE]
>  Bir akıllı grubun durumunun değiştirilmesi, bağımsız üye uyarılarının durumunu değiştirmez.

## <a name="smart-group-details-page"></a>Akıllı Grup ayrıntıları sayfası

Akıllı Grup Ayrıntısı sayfası, bir akıllı grup seçtiğinizde görüntülenir. Bu, grubu oluşturmak için kullanılan düşünme dahil olmak üzere akıllı grup hakkında ayrıntılar sağlar ve durumunu değiştirmenizi sağlar.
 
![Akıllı Grup Ayrıntısı](media/alerts-smartgroups-overview/smart-group-detail.png)


Akıllı Grup Ayrıntısı sayfası aşağıdaki bölümleri içerir.

| Section | Açıklama |
|:---|:---|
| Uyarılar | Akıllı gruba dahil olan bireysel uyarıları listeler. Uyarı ayrıntısı sayfasını açmak için bir uyarı seçin. |
| Geçmiş | Akıllı grup tarafından gerçekleştirilen her eylemi ve üzerinde yapılan değişiklikleri listeler. Bu, şu anda durum değişiklikleri ve uyarı üyeliği değişiklikleri ile sınırlıdır. |

## <a name="smart-group-taxonomy"></a>Akıllı grup sınıflandırması

Bir akıllı grubun adı, ilk uyarısının adıdır. Akıllı grup oluşturamaz veya yeniden adlandıramazsınız.

## <a name="next-steps"></a>Sonraki adımlar

- [Akıllı grupları yönetme](https://aka.ms/managing-smart-groups)
- [Uyarınızı ve akıllı grup durumunu değiştirme](https://aka.ms/managing-alert-smart-group-states)


