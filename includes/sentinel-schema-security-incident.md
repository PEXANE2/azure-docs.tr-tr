---
title: include dosyası
description: include dosyası
services: azure-sentinel
author: yelevin
ms.service: azure-sentinel
ms.topic: include
ms.date: 06/28/2020
ms.author: yelevin
ms.custom: include file
ms.openlocfilehash: 76020b3c1f28e5b5f6363aef181b76bc93a9613e
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87294495"
---
### <a name="the-data-model-of-the-schema"></a>Şemanın veri modeli

| Alan | Veri türü | Açıklama |
| ---- | ---- | ---- |
| **AdditionalData** | dinamik | Uyarı sayısı, yer işaretleri sayısı, açıklama sayısı, uyarı ürünlerinin adı ve tackler |
| **AlertId 'Ler** | dinamik | Olayın oluşturulduğu uyarılar |
| **Bookmarkıds** | dinamik | Yer işareti bulunan varlıklar |
| **Sınıflandırma** | dize | Olay kapatma sınıflandırması |
| **ClassificationComment** | dize | Olay kapatma sınıflandırma açıklaması |
| **ClassificationReason** | dize | Olay kapatma sınıflandırma nedeni |
| **ClosureTime** | datetime | Olayın en son kapatıldığı zaman damgası (UTC) |
| **Açıklamalar** | dinamik | Olay açıklamaları |
| **CreatedTime** | datetime | Olayın oluşturulduğu zaman damgası (UTC) |
| **Açıklama** | dize | Olay açıklaması |
| **FirstActivityTime** | datetime | İlk olay saati |
| **FirstModifiedTime** | datetime | Olayın ilk değiştirildiği zaman damgası (UTC) |
| **Incidentname** | dize | İç GUID |
| **IncidentNumber** | int |  |
| **IncidentUrl** | dize | Olaya bağla |
| **Etiketler** | dinamik | Etiketler |
| **LastActivityTime** | datetime | Son olay saati |
| **Zamanı** | datetime | Olayın son değiştirildiği zaman damgası (UTC) <br>(geçerli kayıt tarafından tanımlanan değişiklik) |
| **ModifiedBy** | dize | Olayı değiştiren kullanıcı veya sistem |
| **Sahibi** | dinamik |  |
| **Relatedanaliz Ticruleıds** | dinamik | Olay uyarılarının tetiklendiği kurallar |
| **Önem Derecesi** | dize | Olayın önem derecesi (yüksek/orta/düşük/bilgilendirici) |
| **SourceSystem** | dize | Sabit (' Azure ') |
| **Durum** | dize |  |
| **Değerine** | dize |  |
| **TimeGenerated** | datetime | Geçerli kaydın oluşturulduğu zaman damgası (UTC) <br>(olayın değiştirilmesi sırasında) |
| **Başlık** | dize | 
| **Tür** | dize | Sabit (' Securityıncident ') |
|
