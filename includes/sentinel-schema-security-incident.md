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
ms.openlocfilehash: 63cb53dc60a718892d4bf86140e7fd51303bd61c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "88761737"
---
### <a name="the-data-model-of-the-schema"></a>Şemanın veri modeli

| Alan | Veri türü | Açıklama |
| ---- | ---- | ---- |
| **AdditionalData** | dynamic | Uyarı sayısı, yer işaretleri sayısı, açıklama sayısı, uyarı ürünlerinin adı ve tackler |
| **AlertId 'Ler** | dynamic | Olayın oluşturulduğu uyarılar |
| **Bookmarkıds** | dynamic | Yer işareti bulunan varlıklar |
| **Sınıflandırma** | string | Olay kapatma sınıflandırması |
| **ClassificationComment** | string | Olay kapatma sınıflandırma açıklaması |
| **ClassificationReason** | string | Olay kapatma sınıflandırma nedeni |
| **ClosedTime** | datetime | Olayın en son kapatıldığı zaman damgası (UTC) |
| **Açıklamalar** | dynamic | Olay açıklamaları |
| **CreatedTime** | datetime | Olayın oluşturulduğu zaman damgası (UTC) |
| **Açıklama** | string | Olay açıklaması |
| **FirstActivityTime** | datetime | İlk olay saati |
| **FirstModifiedTime** | datetime | Olayın ilk değiştirildiği zaman damgası (UTC) |
| **Incidentname** | string | İç GUID |
| **IncidentNumber** | int |  |
| **IncidentUrl** | string | Olaya bağla |
| **Etiketler** | dynamic | Etiketler |
| **LastActivityTime** | datetime | Son olay saati |
| **Zamanı** | datetime | Olayın son değiştirildiği zaman damgası (UTC) <br>(geçerli kayıt tarafından tanımlanan değişiklik) |
| **ModifiedBy** | string | Olayı değiştiren kullanıcı veya sistem |
| **Sahibi** | dynamic |  |
| **Relatedanaliz Ticruleıds** | dynamic | Olay uyarılarının tetiklendiği kurallar |
| **Önem Derecesi** | string | Olayın önem derecesi (yüksek/orta/düşük/bilgilendirici) |
| **SourceSystem** | string | Sabit (' Azure ') |
| **Durum** | string |  |
| **Değerine** | string |  |
| **TimeGenerated** | datetime | Geçerli kaydın oluşturulduğu zaman damgası (UTC) <br>(olayın değiştirilmesi sırasında) |
| **Başlık** | string | 
| **Tür** | string | Sabit (' Securityıncident ') |
|
