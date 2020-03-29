---
title: Internet Explorer (MSAL.js) ile ilgili sorunlar | Azure
titleSuffix: Microsoft identity platform
description: Internet Explorer tarayıcısı ile JavaScript (MSAL.js) için Microsoft Kimlik Doğrulama Kitaplığını kullanın.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/16/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: c4f3c4153e1404a5576427be7ef218f5a669387e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76695866"
---
# <a name="known-issues-on-internet-explorer-and-microsoft-edge-browsers-msaljs"></a>Internet Explorer ve Microsoft Edge tarayıcılarında bilinen sorunlar (MSAL.js)

JavaScript için Microsoft Kimlik Doğrulama Kitaplığı (MSAL.js), Internet Explorer'da çalıştırılabilmek için [JavaScript ES5](https://fr.wikipedia.org/wiki/ECMAScript#ECMAScript_Edition_5_.28ES5.29) için oluşturulur. Ancak bilinmesi gereken birkaç şey var.

## <a name="run-an-app-in-internet-explorer"></a>Internet Explorer'da uygulama çalıştırma
Internet Explorer'da çalıştırılabilen uygulamalarda MSAL.js'yi kullanmayı planlıyorsanız, MSAL.js komut dosyasına başvurmadan önce söz polidoluna bir başvuru eklemeniz gerekir.

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js" class="pre"></script>
```

Bunun nedeni, Internet Explorer'ın JavaScript'in verdiği sözleri desteklememesidir.

## <a name="debugging-an-application-running-in-internet-explorer"></a>Internet Explorer'da çalışan bir uygulamanın hata ayıklama

### <a name="running-in-production"></a>Üretimde çalışan
Son kullanıcı nın açılır pencereleri kabul etmesi koşuluyla, uygulamanızı üretime dağıtmak (örneğin Azure Web uygulamalarında) normalde iyi çalışır. Internet Explorer 11 ile test ettik.

### <a name="running-locally"></a>Yerel olarak çalıştırma
Internet Explorer'da çalışan uygulamanızı yerel olarak çalıştırmak ve hata ayıklamak istiyorsanız, aşağıdaki hususlara dikkat etseniz *http://localhost:1234*gerekir (uygulamanızı aşağıdaki gibi çalıştırmak istediğinizi varsayalım):

- Internet Explorer,MSAL.js'lerin doğru çalışmasını engelleyen "korumalı mod" adlı bir güvenlik mekanizmasına sahiptir. Belirtiler arasında, oturum açmadan sonra, sayfa http://localhost:1234/null.

- Uygulamanızı yerel olarak çalıştırmak ve hata ayıklamak için bu "korumalı modu" devre dışı balmanız gerekir. Bunun için:

    1. Internet Explorer **Araçları'nı** (vites simgesi) tıklatın.
    1. **Internet Seçenekleri'ni** ve ardından **Güvenlik** sekmesini seçin.
    1. **Internet** bölgesini tıklatın ve **Korumalı Modu Etkinleştir'in**denetimini kaldırın (Internet Explorer'ı yeniden başlatmayı gerektirir) . Internet Explorer, bilgisayarınızın artık korunmadığı konusunda uyarır. **Tamam**'a tıklayın.
    1. Internet Explorer'ı yeniden başlatın.
    1. Uygulamanızı çalıştırın ve hata ayıklanın.

İşi bittiğinde, Internet Explorer güvenlik ayarlarını geri yükleyin.  **Ayarlar** -> **Internet Seçenekleri** -> **Güvenliği'ni** -> seçin**Tüm bölgeleri varsayılan düzeye sıfırlayın.**

## <a name="next-steps"></a>Sonraki adımlar
[Internet Explorer'da MSAL.js kullanırken bilinen sorunlar](msal-js-use-ie-browser.md)hakkında daha fazla bilgi edinin.