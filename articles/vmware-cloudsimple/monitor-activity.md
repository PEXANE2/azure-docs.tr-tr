---
title: Özel bulut etkinliğini izle
titleSuffix: Azure VMware Solution by CloudSimple
description: Azure VMware çözümünde, uyarılar, olaylar, görevler ve denetim dahil olmak üzere CloudSimple ortamı tarafından kullanılabilen bilgiler açıklanmaktadır.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/13/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1228f3a54e02d4fe7a5133e2bfba55c38e34718a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75459211"
---
# <a name="monitor-vmware-solution-by-cloudsimple-activity"></a>CloudSimple etkinliğine göre VMware çözümünü izleme

CloudSimple etkinlik günlükleri CloudSimple Portal 'da gerçekleştirilen işlemlere bir öngörüler sağlar.  Listede uyarılar, olaylar, görevler ve denetim bulunur.  Hangi işlemlerin ne zaman ve ne zaman yapılacağını belirlemek için etkinlik günlüklerini kullanın.  Etkinlik günlükleri, bir kullanıcı tarafından gerçekleştirilen okuma işlemlerini içermez.

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="access-the-cloudsimple-portal"></a>CloudSimple portalına erişim

[Cloudsimple portalına](access-cloudsimple-portal.md)erişin.

## <a name="activity-information"></a>Etkinlik bilgileri

Etkinlik sayfalarına erişmek için yan menüdeki **etkinlik** ' i seçin.

![Etkinlik sayfasına genel bakış](media/activity-page-overview.png)

Etkinlik sayfasındaki etkinliklerin ayrıntılarını görüntülemek için etkinliğini seçin. Sağ tarafta bir Ayrıntılar bölmesi açılır. Paneldeki eylemler etkinlik türüne bağlıdır. Paneli kapatmak için **X** simgesini tıklatın.

Görüntüyü sıralamak için bir sütun başlığına tıklayın.  Görüntülenecek belirli değerler için sütunları filtreleyebilirsiniz.  **CSV olarak indir** simgesine tıklayarak Etkinlik raporunu indirin.

## <a name="alerts"></a>Uyarılar

Uyarılar, CloudSimple ortamınızdaki önemli etkinliklerin bildirimidir.  Uyarılar, faturalandırma veya Kullanıcı erişimini etkileyen olayları içerir.

Uyarıları doğrulamak ve listeden kaldırmak için listeden bir veya daha fazla seçim yapın ve **Onayla**' ya tıklayın.

Aşağıdaki bilgi sütunları uyarılar için kullanılabilir. **Sütunları Düzenle** ' ye tıklayın ve görüntülemek istediğiniz sütunları seçin.

| Sütun | Açıklama |
------------ | ------------- |
| Uyarı Türü | Uyarı kategorisi.|
| Zaman | Uyarının gerçekleştiği zaman. |
| Önem Derecesi | Uyarının önemi.|
| Kaynak Adı | Kaynağa atanan, özel bulut adı gibi ad. |
| Kaynak Türü | Kaynak kategorisi: özel bulut, bulut rafı. |
| Kaynak kimliği | Kaynağın tanımlayıcısı. |
| Açıklama | Uyarının tetiklendiği açıklama. |
| Onaylandı | Uyarının kabul edilip edilmeyeceğini belirtir. |

## <a name="events"></a>Etkinlikler

Olaylar CloudSimple portalında Kullanıcı ve sistem etkinliğini gösterir. Olaylar sayfası, belirli bir kaynakla ilişkili etkinliği ve etkinin önem derecesini listeler.

Aşağıdaki bilgi sütunları uyarılar için kullanılabilir. **Sütunları Düzenle** ' ye tıklayın ve görüntülemek istediğiniz sütunları seçin.

| Sütun | Açıklama |
------------ | ------------- |
| Zaman | Olayın gerçekleştiği tarih ve saat. |
| Olay Türü | Olayı tanımlayan sayısal kod. |
| Önem Derecesi | Olay önem derecesi.|
| Kaynak Adı | Kaynağa atanan, özel bulut adı gibi ad. |
| Kaynak Türü | Kaynak kategorisi: özel bulut, bulut rafı. |
| Açıklama | Uyarının tetiklendiği açıklama. |

## <a name="tasks"></a>Görevler

Görevler, 30 saniye veya daha fazla tamamlanması beklenen özel bulut etkinlikleridir. (30 saniyeden az olması beklenen etkinlikler yalnızca olaylar olarak raporlanır.) Özel bulutunuzun görevlerinin ilerlemesini izlemek için görevler sayfalarını açın.

Aşağıdaki bilgi sütunları uyarılar için kullanılabilir. **Sütunları Düzenle** ' ye tıklayın ve görüntülemek istediğiniz sütunları seçin.

| Sütun | Açıklama |
------------ | ------------- |
| Görev Kimliği | Görevin benzersiz tanımlayıcısı. |
| İşlem | Görevin gerçekleştirdiği eylem. |
| Kullanıcı | Görevi tamamlamaya yönelik Kullanıcı atandı. |
| Kaynak Adı | Kaynağa atanan ad. |
| Kaynak Türü | Kaynak kategorisi: özel bulut, bulut rafı. |
| Kaynak kimliği | Kaynağın tanımlayıcısı. |
| Başlayın | Görevin başlangıç zamanı. |
| Bitiş | Görevin bitiş saati. |
| Durum | Geçerli görev durumu. |
| Geçen Süre | Görevin tamamlanması (tamamlandıysa) veya şu anda (devam ediyorsa) alınması için geçen süre. |
| Açıklama | Görev açıklaması. |

## <a name="audit"></a>Denetim

Denetim günlükleri Kullanıcı etkinliğini izler. Tüm kullanıcılar için Kullanıcı etkinliğini izlemek üzere Denetim günlüklerini kullanabilirsiniz.

Aşağıdaki bilgi sütunları uyarılar için kullanılabilir. **Sütunları Düzenle** ' ye tıklayın ve görüntülemek istediğiniz sütunları seçin.

| Sütun | Açıklama |
------------ | ------------- |
| Zaman | Denetim girişinin saati. |
| İşlem | Görevin gerçekleştirdiği eylem. |
| Kullanıcı | Göreve atanan kullanıcı. |
| Kaynak Adı | Kaynağa atanan ad. |
| Kaynak Türü | Kaynak kategorisi: özel bulut, bulut rafı. |
| Kaynak kimliği | Kaynağın tanımlayıcısı. |
| Sonuç | Etkinliğin sonucu, örneğin **başarılı**. |
| Geçen Süre | Görevin tamamlanma zamanı. |
| Açıklama | Eylemin açıklaması. |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'da VMware VM 'lerini kullanma](quickstart-create-vmware-virtual-machine.md)
* [Özel bulutlar](cloudsimple-private-cloud.md) hakkında daha fazla bilgi edinin
