---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/11/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 020e59f029b09f3c7656f67039731e4141e68d31
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188975"
---
Python uygulamanızı başlatırken bir hatayla karşılaşırsa, yalnızca basit bir hata sayfası döndürülür (örn. "İç sunucu hatası oluştuğu için sayfa görüntülenemez.").

Python uygulama hatalarını yakalamak için:

1. Azure portalında, web uygulamanızda **Ayarlar'ı**seçin.
2. **Ayarlar** sekmesinde **Uygulama ayarlarını**seçin.
3. **Uygulama ayarlarının**altında, aşağıdaki anahtar/değer çiftini girin:
    * Anahtar : WSGI_LOG
    * Değer : D:\home\site\wwwroot\logs.txt (dosya adı seçtiğiniz girin)

Şimdi wwwroot klasöründe logs.txt dosyasında hataları görmeniz gerekir.
