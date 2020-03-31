---
title: Özel Bulut etkinliğini izleme
titleSuffix: Azure VMware Solution by CloudSimple
description: Uyarılar, etkinlikler, görevler ve denetim de dahil olmak üzere CloudSimple ortamına göre Azure VMware Çözümü'ndeki etkinlikte bulunan bilgileri açıklar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/13/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1228f3a54e02d4fe7a5133e2bfba55c38e34718a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77019681"
---
# <a name="monitor-vmware-solution-by-cloudsimple-activity"></a>CloudSimple etkinliğine göre VMware Çözümünü izleyin

CloudSimple etkinlik günlükleri, CloudSimple portalında yapılan işlemlerhakkında bir fikir sağlar.  Liste uyarıları, olayları, görevleri ve denetimi içerir.  Kim, ne zaman ve hangi işlemlerin gerçekleştirildiyi belirlemek için etkinlik günlüklerini kullanın.  Etkinlik günlükleri, kullanıcı tarafından yapılan okuma işlemlerini içermez.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="access-the-cloudsimple-portal"></a>CloudSimple portalına erişim

[CloudSimple portalına](access-cloudsimple-portal.md)erişin.

## <a name="activity-information"></a>Etkinlik bilgileri

Etkinlik sayfalarına erişmek için yan menüde **Etkinlik'i** seçin.

![Etkinlik sayfasına genel bakış](media/activity-page-overview.png)

Etkinlik sayfasındaki etkinliklerle ilgili ayrıntıları görüntülemek için etkinliği seçin. Sağ tarafta bir detay paneli açılır. Paneldeki eylemler etkinliğin türüne bağlıdır. Paneli kapatmak için **X'i** tıklatın.

Ekranı sıralamak için sütun üstbilgisini tıklatın.  Sütunları görüntülemek için belirli değerler için filtreleyebilirsiniz.  **CSV** simgesi olarak İndir'e tıklayarak etkinlik raporunu indirin.

## <a name="alerts"></a>Uyarılar

Uyarılar, CloudSimple ortamınızdaki herhangi bir önemli etkinliğin bildirimleridir.  Uyarılar, faturalandırmayı veya kullanıcı erişimini etkileyen olayları içerir.

Uyarıları kabul etmek ve listeden kaldırmak için listeden bir veya daha fazlasını seçin ve **Kabul Et'i**tıklatın.

Uyarılar için aşağıdaki bilgi sütunları kullanılabilir. **Sütunları Edit'e** tıklayın ve görüntülemek istediğiniz sütunları seçin.

| Sütun | Açıklama |
------------ | ------------- |
| Uyarı Türü | Uyarı kategorisi.|
| Zaman | Uyarının oluştuğu saat. |
| Severity | Uyarının önemi.|
| Kaynak Adı | Özel Bulut adı gibi kaynağa atanan ad. |
| Kaynak Türü | Kaynak kategorisi: Özel Bulut, Bulut Raf. |
| Kaynak kimliği | Kaynağın tanımlayıcısı. |
| Açıklama | Uyarıyı neyin tetiklediğinin açıklaması. |
| Onaylandı | Uyarının kabul edilip edilmediğine dair bir belirti. |

## <a name="events"></a>Olaylar

Olaylar, CloudSimple portalında kullanıcı ve sistem etkinliğini gösterir. Etkinlikler sayfası, belirli bir kaynakla ilişkili etkinliği ve etkisinin önem derecesini listeler.

Uyarılar için aşağıdaki bilgi sütunları kullanılabilir. **Sütunları Edit'e** tıklayın ve görüntülemek istediğiniz sütunları seçin.

| Sütun | Açıklama |
------------ | ------------- |
| Zaman | Olayın oluştuğu tarih ve saat. |
| Olay Türü | Olayı tanımlayan sayısal kod. |
| Severity | Olay şiddeti.|
| Kaynak Adı | Özel Bulut adı gibi kaynağa atanan ad. |
| Kaynak Türü | Kaynak kategorisi: Özel Bulut, Bulut Raf. |
| Açıklama | Uyarıyı neyin tetiklediğinin açıklaması. |

## <a name="tasks"></a>Görevler

Görevler, tamamlanması 30 saniye veya daha uzun sürmesi beklenen Özel Bulut etkinlikleridir. (30 saniyeden kısa sürmesi beklenen etkinlikler yalnızca olay olarak bildirilir.) Özel Bulut'unuzun görevlerinin ilerlemesini izlemek için Görevler sayfalarını açın.

Uyarılar için aşağıdaki bilgi sütunları kullanılabilir. **Sütunları Edit'e** tıklayın ve görüntülemek istediğiniz sütunları seçin.

| Sütun | Açıklama |
------------ | ------------- |
| Görev Kimliği | Görev için benzersiz tanımlayıcı. |
| İşlem | Görevin gerçekleştirdiği eylem. |
| Kullanıcı | Görevi tamamlamak için atanan kullanıcı. |
| Kaynak Adı | Kaynağa atanan ad. |
| Kaynak Türü | Kaynak kategorisi: Özel Bulut, Bulut Raf. |
| Kaynak kimliği | Kaynağın tanımlayıcısı. |
| Başlangıç | Görev için başlangıç zamanı. |
| End | Göreviçin bitiş zamanı. |
| Durum | Geçerli görev durumu. |
| Geçen Süre | Görevin tamamlanması (tamamlandıysa) veya şu anda almakta olduğu süre (devam ediyorsa). |
| Açıklama | Görev tanımı. |

## <a name="audit"></a>Denetim

Denetim günlükleri kullanıcı etkinliğini izler. Tüm kullanıcıların kullanıcı etkinliğini izlemek için denetim günlüklerini kullanabilirsiniz.

Uyarılar için aşağıdaki bilgi sütunları kullanılabilir. **Sütunları Edit'e** tıklayın ve görüntülemek istediğiniz sütunları seçin.

| Sütun | Açıklama |
------------ | ------------- |
| Zaman | Denetim giriş saati. |
| İşlem | Görevin gerçekleştirdiği eylem. |
| Kullanıcı | Göreve atanan kullanıcı. |
| Kaynak Adı | Kaynağa atanan ad. |
| Kaynak Türü | Kaynak kategorisi: Özel Bulut, Bulut Raf. |
| Kaynak kimliği | Kaynağın tanımlayıcısı. |
| Sonuç | **Başarı**gibi etkinliğin sonucu. |
| Geçen Süre | Görevi tamamlama zamanı. |
| Açıklama | Eylemin açıklaması. |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da VMware sanal makinelerini kullanma](quickstart-create-vmware-virtual-machine.md)
* [Özel Bulutlar](cloudsimple-private-cloud.md) hakkında daha fazla bilgi edinin
