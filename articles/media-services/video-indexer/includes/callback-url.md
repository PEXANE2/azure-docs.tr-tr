---
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 11/13/2020
ms.author: juliako
ms.openlocfilehash: fbc77b960cac0db2d077345c74d64485bd7ad8bb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "95994511"
---
Aşağıdaki olaylar hakkında müşteriyi bilgilendirmek için kullanılan bir URL (POST isteği kullanılarak):

- Dizin oluşturma durum değişikliği: 
    - Özellikler:    
    
        |Ad|Açıklama|
        |---|---|
        |kimlik|Video KIMLIĞI|
        |state|Video durumu|  
    - Örnek: https: \/ /test.com/notifyme?projectName=MyProject&ID = 1234abcd&State = işlendi
- Videoda tanımlanan kişi:
  - Özellikler
    
      |Ad|Açıklama|
      |---|---|
      |kimlik| Video KIMLIĞI|
      |FaceID|Video dizininde görünen yüz KIMLIĞI|
      |Knownpersonıd|Bir yüz modeli içinde benzersiz olan kişi KIMLIĞI|
      |kişi adı|Kişinin adı|
        
    - Örnek: https: \/ /test.com/notifyme?projectName=MyProject&ID = 1234abcd&çok yönlü kimliği = 12&Knownpersonıd = CCA84350-89B7-4262-861C-3CAC796542A5&personName = Inigo_Montoya 
