---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 760bb5b62e9bba9b7a83f99760f7fe5d8c399dfb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188322"
---
1. Failover Cluster Manager'da **Rolleri**genişletin ve ardından kullanılabilirlik grubunuzu vurgulayın.  

2. **Kaynaklar** sekmesinde, dinleyici adını sağ tıklatın ve ardından **Özellikler'i**tıklatın.

3. **Bağımlılıklar** sekmesini tıklatın. Birden çok kaynak listeleniyorsa, IP adreslerinde OR değil, VE bağımlılıkları olduğunu doğrulayın.  

4. **Tamam**'a tıklayın.

5. Dinleyici adını sağ tıklatın ve sonra **Çevrimiçi Getir'i**tıklatın.

6. Dinleyici çevrimiçi olduktan sonra, **Kaynaklar** sekmesinde kullanılabilirlik grubunu sağ tıklatın ve ardından **Özellikler'i**tıklatın.
   
    ![Kullanılabilirlik grubu kaynağını yapılandırma](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. Dinleyici adı kaynağına (IP adresi kaynakları adı değil) bağımlılık oluşturun ve ardından **Tamam'ı**tıklatın.
   
    ![Dinleyici adına bağımlılık ekleme](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. SQL Server Management Studio'yu başlatın ve ardından birincil yinelemeye bağlanın.

9. **AlwaysOn Yüksek Kullanılabilirlik** > **Kullanılabilirlik Grupları** > **\<AvailabilityGroupName\>** > **Kullanılabilirlik Grubu Dinleyiciler**gidin.  
    Failover Cluster Manager'da oluşturduğunuz dinleyici adı görüntülenmelidir.

10. Dinleyici adını sağ tıklatın ve ardından **Özellikler'i**tıklatın.

11. Bağlantı **Noktası** kutusunda, daha önce kullandığınız $EndpointPort kullanarak kullanılabilirlik grubu dinleyicisinin bağlantı noktası numarasını belirtin (bu öğreticide, varsayılan 1433'tü) ve ardından **Tamam'ı**tıklatın.

