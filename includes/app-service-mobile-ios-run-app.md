---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: a5bde1a56bf6a1f5fca4b775c7c8e9bb7477eb6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66240264"
---
1. Xcode kullanarak indirilen istemci projesini açın.

2. [Azure portalına](https://portal.azure.com/) gidin ve oluşturduğunuz mobil uygulamaya gidin. Bıçakta, `Overview` mobil uygulamanızın genel bitiş noktası olan URL'ye bakın. Örnek - uygulama adım "test123" için https://test123.azurewebsites.netsite adı olacaktır.

3. Swift projesi için bu `ToDoTableViewController.swift` klasördeki dosyayı açın - ZUMOAPPNAME/ZUMOAPPNAME/ToDoTableViewController.swift. Uygulama `ZUMOAPPNAME`adı.

4. Yöntemde, `viewDidLoad()` `ZUMOAPPURL` parametreyi yukarıdaki ortak bitiş noktasıyla değiştirin.

    `let client = MSClient(applicationURLString: "ZUMOAPPURL")`

    Olur
    
    `let client = MSClient(applicationURLString: "https://test123.azurewebsites.net")`
    
5. Objective-C projesi için bu `QSTodoService.m` klasördeki dosyayı açın - ZUMOAPPNAME/ZUMOAPPNAME. Uygulama `ZUMOAPPNAME`adı.

6. Yöntemde, `init` `ZUMOAPPURL` parametreyi yukarıdaki ortak bitiş noktasıyla değiştirin.

    `self.client = [MSClient clientWithApplicationURLString:@"ZUMOAPPURL"];`

    Olur
    
    `self.client = [MSClient clientWithApplicationURLString:@"https://test123.azurewebsites.net"];`

7. Projeyi oluşturmak için **Çalıştır** düğmesine basın ve uygulamayı iOS benzeticisinde başlatın.

8. Uygulamada artı (**+**) simgesine tıklayın, *öğreticiyi tamamla*gibi anlamlı metin yazın ve ardından kaydet düğmesini tıklatın. Böylece, daha önce dağıttığınız Azure arka ucuna bir POST isteği gönderilir. Arka uç, verileri istekten TodoItem SQL tablosuna ekler ve yeni depolanan öğeler hakkındaki bilgileri mobil uygulamaya döndürür. Mobil uygulama bu verileri listede görüntüler.

   ![iOS’ta çalışan hızlı başlangıç uygulaması](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure portal]: https://portal.azure.com/
