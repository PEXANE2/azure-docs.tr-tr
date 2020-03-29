---
title: Sözcük hizalaması - Çevirmen Metin API
titleSuffix: Azure Cognitive Services
description: Hizalama bilgilerini almak için Çevir yöntemini kullanın ve isteğe bağlı hizalama parametresini ekleyin.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.custom: seodec18
ms.openlocfilehash: dd4ff1e39c062910f4627973c801dc3c51f345e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73837221"
---
# <a name="how-to-receive-word-alignment-information"></a>Sözcük hizalama bilgileri nasıl elde edilir?

## <a name="receiving-word-alignment-information"></a>Sözcük hizalama bilgilerini alma
Hizalama bilgilerini almak için Çevir yöntemini kullanın ve isteğe bağlı hizalama parametresini ekleyin.

## <a name="alignment-information-format"></a>Hizalama bilgi biçimi
Hizalama, kaynağın her sözcüğü için aşağıdaki biçimin dize değeri olarak döndürülür. Her sözcüğün bilgileri, Çince gibi uzaydan ayrılmış olmayan diller (komut dosyaları) da dahil olmak üzere bir boşlukla ayrılır:

[[KaynakTextStartIndex]:[SourceTextEndIndex]–[TgtTextStartIndex]:[TgtTextEndIndex]] *

Örnek hizalama dizesi: "0:0-7:10 1:2-11:20 3:4-0:3 3:4-4:6 5:5-21:21".

Başka bir deyişle, üst üste başlangıç ve bitiş dizini ayırır, tire dilleri ayırır ve boşluk sözcükleri ayırır. Bir sözcük diğer dilde sıfır, bir veya birden çok sözcük ile hizalanabilir ve hizalanmış sözcükler bitişik olmayabilir. Hizalama bilgisi olmadığında Hizalama öğesi boş olur. Yöntem bu durumda hiçbir hata döndürür.

## <a name="restrictions"></a>Kısıtlamalar
Hizalama yalnızca bu noktadadil çiftleri bir alt kümesi için döndürülür:
* İngilizce'den başka bir dile;
* Çince Basitleştirilmiş, Geleneksel Çince ve Letonca'dan İngilizce'ye kadar başka bir dilden İngilizce'ye
* Japonca'dan Korece'ye veya Korece'den Japonca'ya, cümle konserve bir çeviriyse hizalama bilgilerini almazsınız. Konserve çevirisi örneği "Bu bir test", "Seni seviyorum" ve diğer yüksek frekanslı cümlelerdir.

## <a name="example"></a>Örnek

Örnek JSON

```json
[
  {
    "translations": [
      {
        "text": "Kann ich morgen Ihr Auto fahren?",
        "to": "de",
        "alignment": {
          "proj": "0:2-0:3 4:4-5:7 6:10-25:30 12:15-16:18 17:19-20:23 21:28-9:14 29:29-31:31"
        }
      }
    ]
  }
]
```
