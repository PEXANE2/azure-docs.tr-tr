---
title: ITSM ürününüzle LA çalışma alanına eşitlenen veriler
description: Bu makalede, ıTSM ürününüzle LA çalışma alanına eşitlenen verilere ilişkin bir genel bakış sunulmaktadır.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/29/2020
ms.custom: references_regions
ms.openlocfilehash: 83994c1b6e150342a777a079d79d6a594d30c3ff
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102041612"
---
# <a name="data-synced-from-your-itsm-product"></a>ITSM ürününüzle eşitlenen veriler

Olaylar ve değişiklik istekleri, bağlantı yapılandırmasına bağlı olarak ıTSM aracınızdan Log Analytics çalışma alanınıza eşitlenir ("verileri Eşitle" alanı kullanılarak):
* [ServiceNow](./itsmc-connections-servicenow.md)
* [System Center Service Manager](./itsmc-connections-scsm.md)
* [Cherwell](./itsmc-connections-cherwell.md)
* [Provance](./itsmc-connections-provance.md)

## <a name="synced-data"></a>Eşitlenmiş veriler

Bu bölümde, ıSMC tarafından toplanan verilerin bazı örnekleri gösterilmektedir.

**ServiceDesk_CL** alanlar, Log Analytics içe aktardığınız iş öğesi türüne bağlı olarak değişir. İki iş öğesi türü için bir alan listesi aşağıda verilmiştir:

**İş öğesi:** **Olaylar**  
ServiceDeskWorkItemType_s = "olay"

**Alanlar**

- ServiceDeskConnectionName
- Hizmet Masası KIMLIĞI
- Durum
- Aciliyet
- Etki
- Öncelik
- Önem Yükseltme
- Oluşturan
- Çözümleyen
- Kapatan
- Kaynak
- Atanan
- Kategori
- Başlık
- Açıklama
- Oluşturulduğu Tarihi
- Kapatılma Tarihi
- Çözümlenme Tarihi
- Son Değişiklik Tarihi
- Bilgisayar

**İş öğesi:** **değişiklik istekleri**

ServiceDeskWorkItemType_s = "ChangeRequest"

**Alanlar**
- ServiceDeskConnectionName
- Hizmet Masası KIMLIĞI
- Oluşturan
- Kapatan
- Kaynak
- Atanan
- Başlık
- Tür
- Kategori
- Durum
- Önem Yükseltme
- Çakışma durumu
- Aciliyet
- Öncelik
- Risk
- Etki
- Atanan
- Oluşturulduğu Tarihi
- Kapatılma Tarihi
- Son Değişiklik Tarihi
- İstenen Tarih
- Planlanan başlangıç tarihi
- Planlanan bitiş tarihi
- Çalışma başlangıç tarihi
- Çalışma bitiş tarihi
- Description
- Bilgisayar

## <a name="servicenow-example"></a>ServiceNow örneği 
### <a name="output-data-for-a-servicenow-incident"></a>ServiceNow olayı için çıkış verileri

| Log Analytics alanı | ServiceNow alanı |
|:--- |:--- |
| ServiceDeskId_s| Sayı |
| IncidentState_s | Durum |
| Urgency_s |Aciliyet |
| Impact_s |Etki|
| Priority_s | Öncelik |
| CreatedBy_s | Açan |
| ResolvedBy_s | Çözümleyen|
| ClosedBy_s  | Kapatan |
| Source_s| Kişi türü |
| AssignedTo_s | Atanan  |
| Category_s | Kategori |
| Title_s|  Kısa açıklama |
| Description_s|  Notlar |
| CreatedDate_t|  Makta |
| ClosedDate_t| kapalı|
| ResolvedDate_t|Çözümlendi|
| Bilgisayar  | Yapılandırma öğesi |

### <a name="output-data-for-a-servicenow-change-request"></a>ServiceNow değişiklik isteği için çıkış verileri

| Log Analytics | ServiceNow alanı |
|:--- |:--- |
| ServiceDeskId_s| Sayı |
| CreatedBy_s | İsteyen |
| ClosedBy_s | Kapatan |
| AssignedTo_s | Atanan  |
| Title_s|  Kısa açıklama |
| Type_s|  Tür |
| Category_s|  Kategori |
| CRState_s|  Durum|
| Urgency_s|  Aciliyet |
| Priority_s| Öncelik|
| Risk_s| Risk|
| Impact_s| Etki|
| RequestedDate_t  | İstek tarihine göre |
| ClosedDate_t | Kapatma tarihi |
| PlannedStartDate_t  | Planlanan başlangıç tarihi |
| PlannedEndDate_t  | Planlanan bitiş tarihi |
| WorkStartDate_t  | Gerçek başlangıç tarihi |
| WorkEndDate_t | Gerçek bitiş tarihi|
| Description_s | Description |
| Bilgisayar  | Yapılandırma öğesi |

## <a name="next-steps"></a>Sonraki adımlar

* [ITSM Bağlayıcısındaki sorunları giderme](./itsmc-resync-servicenow.md)
