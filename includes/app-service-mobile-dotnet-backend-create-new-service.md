---
title: include dosyası
description: include dosyası
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 06/20/2019
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: 72a69359d412a7560472fbb73ec525ab5d4a4fce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67325804"
---
1. [Azure portalında]oturum açın.

2. **Kaynak Oluştur'u**tıklatın.

3. Arama kutusuna **Web Uygulaması**yazın.
    
4. Sonuç listesinde, Market'ten **Web Uygulaması'nı** seçin.

5. **Abonelik** ve **Kaynak Grubunuzu** seçin (varolan bir kaynak grubu seçin _veya_ yeni bir tane oluşturun (uygulamanızla aynı adı kullanarak)).

6. Web uygulamanızın benzersiz bir **adı** seçin.

7. **Varsayılan Kod**olarak **Yayımla** seçeneğini seçin.

8. **Runtime yığınında,** **ASP.NET** veya **Düğüm**altında bir sürüm seçmeniz gerekir. Bir .NET arka uç oluşturuyorsanız, ASP.NET altında bir sürüm seçin. Aksi takdirde Düğüm tabanlı bir uygulamayı hedefliyorsanız, Düğüm'den sürümden birini seçin.

9. Linux veya Windows gibi doğru **İşletim Sistemini**seçin. 

10. Bu uygulamanın dağıtılmasını istediğiniz **Bölgeyi** seçin. 

11. Uygun **Uygulama Hizmet Planı'nı** seçin ve Gözden Geçir'e **vurun ve oluşturun.** 

12. **Kaynak Grubu**altında, varolan bir kaynak grubu seçin _veya_ yeni bir tane oluşturun (uygulamanızla aynı adı kullanarak).

13. **Oluştur'u**tıklatın. Devam etmeden önce hizmetin sorunsuz dağıtılması için birkaç dakika bekleyin. Durum güncelleştirmeleri için portal üst bilgisindeki Bildirimler (zil) simgesini izleyin.

14. Dağıtım tamamlandıktan **sonra, Dağıtım ayrıntıları** bölümüne tıklayın ve ardından **Microsoft.Web/site**türü kaynağına tıklayın. Sizi az önce oluşturduğunuz App Service Web App'e yönlendirecektir. 

15. **Ayarlar'ın** altındaki **Yapılandırma** bıçağına ve **Uygulama ayarlarında** **Yeni uygulama ayar** düğmesine tıklayın.

16. **Ekle/Yap uygulama ayar** sayfasında, **Ad'ı MobileAppsManagement_EXTENSION_VERSION** olarak ve Değer'i **en son** olarak girin ve Tamam'a basın. **Name**

Bu yeni oluşturulan App Service Web uygulamasını Mobil uygulama olarak kullanmaya hazırsınız.

<!-- URLs. -->
[Azure portalında]: https://portal.azure.com/