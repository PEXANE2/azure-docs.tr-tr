---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 394b242ab46da7821f77e8d008836753f4e358e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188320"
---
Bu adımda, Failover Cluster Manager ve SQL Server Management Studio'da kullanılabilirlik grubu dinleyicisini el ile oluşturursunuz.

1. Failover Cluster Manager'ı birincil yinelemeyi barındıran düğümden açın.

2. **Ağlar** düğümünü seçin ve ardından küme ağ adını not edin. Bu ad, PowerShell komut dosyasındaki $ClusterNetworkName değişkeninde kullanılır.

3. Küme adını genişletin ve ardından **Görevler'i**tıklatın.

4. **Roller** bölmesinde, kullanılabilirlik grubu adını sağ tıklatın ve sonra Kaynak > **İstemci Erişim Noktası** **Ekle'yi**seçin.
   
    ![Kullanılabilirlik grubu için İstemci Erişim Noktası Ekle](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. **Ad** kutusunda, bu yeni dinleyici için bir ad oluşturun, **İleri'yi** iki kez tıklatın ve sonra **Bitir'i**tıklatın.  
    Bu noktada dinleyiciyi veya kaynağı çevrimiçi duruma getirmeyin.

6. **Kaynaklar** sekmesini tıklatın ve ardından yeni oluşturduğunuz istemci erişim noktasını genişletin. 
    Kümenizdeki her küme ağı için IP adresi kaynağı görüntülenir. Bu yalnızca Azure çözümüyse, yalnızca bir IP adresi kaynağı görüntülenir.

7. Aşağıdakilerden birini yapın:
   
   * Karma bir çözümü yapılandırmak için:
     
        a. Şirket içi alt ağınıza karşılık gelen IP adresi kaynağına sağ tıklayın ve ardından **Özellikler'i**seçin. IP adresi adını ve ağ adını not edin.
   
        b. **Statik IP Adresi'ni**seçin, kullanılmayan bir IP adresi atayın ve ardından **Tamam'ı**tıklatın.
 
   * Yalnızca Azure çözümlerini yapılandırmak için:

        a. Azure alt ağınıza karşılık gelen IP adresi kaynağına sağ tıklayın ve ardından **Özellikler'i**seçin.
       
       > [!NOTE]
       > Dinleyici daha sonra DHCP tarafından seçilen çakışan bir IP adresi nedeniyle çevrimiçi duruma gelmezse, bu özellikler penceresinde geçerli bir statik IP adresi yapılandırabilirsiniz.
       > 
       > 

       b. Aynı **IP Adresi** özellikleri **penceresinde, IP Adresi Adını**değiştirin.  
        Bu ad PowerShell komut dosyasının $IPResourceName değişkeninde kullanılır. Çözümünüz birden çok Azure sanal abuna yayılıyorsa, her BIR IP kaynağı için bu adımı yineleyin.

