---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 760bb5b62e9bba9b7a83f99760f7fe5d8c399dfb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188322"
---
1. Yük Devretme Kümesi Yöneticisi ' de **Roller**' i genişletin ve ardından kullanılabilirlik grubunuzu vurgulayın.  

2. **Kaynaklar** sekmesinde, dinleyici adına sağ tıklayın ve ardından **Özellikler**' e tıklayın.

3. **Bağımlılıklar** sekmesine tıklayın. Birden çok kaynak listeleniyorsa, IP adreslerinin, ve bağımlılıkları olduğunu doğrulayın.  

4. **Tamam**'a tıklayın.

5. Dinleyici adına sağ tıklayın ve ardından **çevrimiçi getir**' e tıklayın.

6. Dinleyici çevrimiçi olduktan sonra, **kaynaklar** sekmesinde kullanılabilirlik grubuna sağ tıklayın ve ardından **Özellikler**' e tıklayın.
   
    ![Kullanılabilirlik grubu kaynağını yapılandırma](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. Dinleyici adı kaynağında (IP adresi kaynak adı değil) bir bağımlılık oluşturun ve ardından **Tamam**' a tıklayın.
   
    ![Dinleyici adına bağımlılık ekleyin](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. SQL Server Management Studio başlatın ve ardından birincil çoğaltmaya bağlanın.

9. **AlwaysOn yüksek kullanılabilirlik**  >  **kullanılabilirlik grupları**  >  **\<AvailabilityGroupName\>**  >  **kullanılabilirlik grubu dinleyicileri**' ne gidin.  
    Yük Devretme Kümesi Yöneticisi içinde oluşturduğunuz dinleyici adı görüntülenmelidir.

10. Dinleyici adına sağ tıklayın ve ardından **Özellikler**' e tıklayın.

11. **Bağlantı noktası** kutusunda, daha önce kullandığınız $EndpointPort kullanarak kullanılabilirlik grubu dinleyicisinin bağlantı noktası numarasını belirtin (Bu öğreticide, 1433 varsayılandır) ve ardından **Tamam**' a tıklayın.

