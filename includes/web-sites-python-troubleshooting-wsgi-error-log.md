---
title: dosya dahil etme
description: dosya dahil etme
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/11/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 020e59f029b09f3c7656f67039731e4141e68d31
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "67188975"
---
Python, uygulamanızı başlatırken bir hatayla karşılaşırsa yalnızca basit bir hata sayfası döndürülür (örneğin, bir iç sunucu hatası oluştuğundan sayfa görüntülenemiyor. ").

Python uygulama hatalarını yakalamak için:

1. Azure portal, Web uygulamanızda **Ayarlar**' ı seçin.
2. **Ayarlar** sekmesinde **uygulama ayarları**' nı seçin.
3. **Uygulama ayarları**' nın altında, aşağıdaki anahtar/değer çiftini girin:
    * Anahtar: WSGI_LOG
    * Değer: D:\home\site\wwwroot\logs.txt (dosya adı seçiminizi girin)

Şimdi Wwwroot klasöründeki logs.txt dosyasında hata görmeniz gerekir.
