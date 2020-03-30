---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/06/2019
ms.author: crdun
ms.openlocfilehash: 8d7731480b6239c572d39f52b6a0217d2ac48d25
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66240253"
---
1. İstemci projesindeki çözüm dosyasına (.sln) gidin ve Visual Studio'yu kullanarak açın.

2. Visual Studio'da başlatma okunun yanındaki açılır menüden çözüm platformunu (Android, iOS veya Windows) seçin. Yeşil ok üzerindeki açılır menüye tıklayarak belirli bir dağıtım cihazı veya öykünücüyü seçin. Varsayılan Android platformunu ve Ripple öykünücüsünü kullanabilirsiniz. Daha gelişmiş öğreticilerde (anında iletme bildirimleri gibi) desteklenen bir cihazı ve öykünücüyü seçmeniz istenecek.

3. Bu klasördeki dosyayı `ToDoActivity.java` açın - ZUMOAPPNAME/app/src/main/java/com/example/zumoappname. Uygulama `ZUMOAPPNAME`adı.

4. [Azure portalına](https://portal.azure.com/) gidin ve oluşturduğunuz mobil uygulamaya gidin. Bıçakta, `Overview` mobil uygulamanızın genel bitiş noktası olan URL'ye bakın. Örnek - uygulama adım "test123" için https://test123.azurewebsites.netsite adı olacaktır.

5. ZUMOAPPNAME/www/js/index.js adresindeki `index.js` dosyaya `onDeviceReady()` gidin ve `ZUMOAPPURL` yöntem olarak parametreyi yukarıdaki ortak bitiş noktasıyla değiştirin.

    `client = new WindowsAzure.MobileServiceClient('ZUMOAPPURL');`
    
    Olur
    
    `client = new WindowsAzure.MobileServiceClient('https://test123.azurewebsites.net');`
    
6. Cordova uygulamanızı derlemek ve çalıştırmak için F5'e basın veya yeşil oka tıklayın. Öykünücüde ağa erişim isteyen bir güvenlik iletişim kutusu görürseniz erişim isteğini kabul edin.

7. Uygulama cihazda veya emülatörde başlatıldıktan sonra, yeni **metin girin'e** *(öğreticiyi tamamla* ve **ekle** düğmesini tıklatmak gibi) anlamlı metin yazın.

Arka uç, istekten alınan verileri SQL Veritabanı'ndaki TodoItem tablosuna ekler ve yeni depolanan öğeler hakkındaki bilgileri de mobil uygulamaya geri döndürür. Mobil uygulama bu verileri listede görüntüler.

Diğer platformlar için 3 ile 5 arasındaki adımları tekrarlayabilirsiniz.