---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/09/2019
ms.author: crdun
ms.openlocfilehash: 63c54f8af91b6b4a76ba49d5e6fc7b3cda9f5b98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66240339"
---
1. **Android Studio** kullanarak, **Projeyi içeri aktar (Eclipse ADT, Gradle, vb.)** kullanarak projeyi açın. JDK hatalarını önlemek için bu içeri aktarma seçimini yaptığınızdan emin olun.

2. Bu klasördeki dosyayı `ToDoActivity.java` açın - ZUMOAPPNAME/app/src/main/java/com/example/zumoappname. Uygulama `ZUMOAPPNAME`adı.

3. [Azure portalına](https://portal.azure.com/) gidin ve oluşturduğunuz mobil uygulamaya gidin. Bıçakta, `Overview` mobil uygulamanızın genel bitiş noktası olan URL'ye bakın. Örnek - uygulama adım "test123" için https://test123.azurewebsites.netsite adı olacaktır.

4. Yöntemde, `onCreate()` `ZUMOAPPURL` parametreyi yukarıdaki ortak bitiş noktasıyla değiştirin.
    
    `new MobileServiceClient("ZUMOAPPURL", this).withFilter(new ProgressFilter());` 
    
    Olur
    
    `new MobileServiceClient("https://test123.azurewebsites.net", this).withFilter(new ProgressFilter());`
    
5. Projeyi oluşturmak için **’Uygulamayı’ çalıştır** düğmesine basın ve uygulamayı Android benzeticisinde başlatın.

4. Uygulamada, *öğreticiyi tamamla* ve ardından 'Ekle' düğmesini tıklatmak gibi anlamlı bir metin yazın. Böylece, daha önce dağıttığınız Azure arka ucuna bir POST isteği gönderilir. Arka uç, verileri istekten TodoItem SQL tablosuna ekler ve yeni depolanan öğeler hakkındaki bilgileri mobil uygulamaya döndürür. Mobil uygulama bu verileri listede görüntüler.
    ![Quickstart Android](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)