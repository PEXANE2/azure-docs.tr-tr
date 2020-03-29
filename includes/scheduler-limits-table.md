---
title: include dosyası
description: include dosyası
services: scheduler
ms.service: scheduler
author: derek1ee
ms.topic: include
ms.date: 08/16/2016
ms.author: deli
ms.custom: include file
ms.openlocfilehash: eb13d889cb72911e2268b7538a74336befe3320b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75392311"
---
Aşağıdaki tabloda Azure Zamanlayıcısı'ndaki ana kotalar, sınırlar, varsayılanlar ve azaltmaların her biri açıklanmaktadır.

| Kaynak | Sınırlama açıklaması |
| -------- | ----------------- |
| **İş boyutu** | Maksimum iş büyüklüğü 16.000'dir. BIR PUT veya PATCH işlemi bu sınırdan daha büyük bir iş boyutuyla sonuçlanırsa, 400 Kötü İstek durum kodu döndürülür. | 
| **İş koleksiyonları** | Azure aboneliği başına en fazla iş koleksiyonu sayısı 200.000'dir. | 
| **Koleksiyon başına işler** | Varsayılan olarak, en fazla iş sayısı ücretsiz bir iş koleksiyonunda beş iş ve standart bir iş koleksiyonunda 50 iştir. Bir iş koleksiyonundaki en fazla iş sayısını değiştirebilirsiniz. İş koleksiyonundaki tüm işler, iş koleksiyonunda ayarlanan değerle sınırlıdır. En yüksek iş kotası daha fazla iş oluşturmaya çalışırsanız, istek 409 Çakışma durum kodu ile başarısız olur. | 
| **Başlama zamanı** | En fazla "başlama saati" 18 aydır. |
| **Yineleme süresi** | Maksimum nüks süresi 18 aydır. | 
| **Frequency** | Varsayılan olarak, maksimum sıklık kotası ücretsiz bir iş koleksiyonunda bir saat ve standart bir iş koleksiyonunda bir dakikadır. <p>Bir iş koleksiyonundaki maksimum sıklığı maksimumdan daha düşük yapabilirsiniz. İş koleksiyonundaki tüm işler, iş koleksiyonunda ayarlanan değerle sınırlıdır. İş koleksiyonundaki en yüksek frekanstan daha yüksek sıklıkta bir iş oluşturmaya çalışırsanız, istek 409 Çakışma durum koduyla başarısız olur. | 
| **Vücut boyutu** | Bir istek için maksimum gövde boyutu 8.192 chars olduğunu. |
| **URL boyutu isteği** | İstek URL'si için maksimum boyut 2.048 karakterdir. |
| **Üstbilgi sayısı** | En yüksek üstbilgi sayısı 50 üstbilgidir. | 
| **Toplam üstbilgi boyutu** | Maksimum toplam üstbilgi boyutu 4.096 karakterdir. |
| **Zaman aşımı** | İstek zaman acısı statiktir, yani yapılandırılamaz. ve HTTP eylemleri için 60 saniyedir. Daha uzun süren işlemler için HTTP eşzamanlı protokollerini izleyin. Örneğin, hemen bir 202 döndürün, ancak arka planda çalışmaya devam edin. | 
| **İş geçmişi** | İş geçmişinde depolanan maksimum yanıt gövdesi 2.048 bayttır. |
| **İş geçmişi tutma** | İş geçmişi iki aya kadar veya son 1000 infaza kadar saklanır. | 
| **Tamamlanmış ve hatalı iş tutma** | Tamamlanan ve hatalı işler 60 gün tutulur. |
||| 

