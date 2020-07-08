---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 394b242ab46da7821f77e8d008836753f4e358e2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188320"
---
Bu adımda, Yük Devretme Kümesi Yöneticisi ve SQL Server Management Studio ' de kullanılabilirlik grubu dinleyicisini el ile oluşturursunuz.

1. Birincil çoğaltmayı barındıran düğümden Yük Devretme Kümesi Yöneticisi açın.

2. **Ağlar** düğümünü seçin ve ardından Küme ağı adını aklınızda edin. Bu ad, PowerShell betiğinin $ClusterNetworkName değişkeninde kullanılır.

3. Küme adını genişletin ve ardından **Roller**' e tıklayın.

4. **Roller** bölmesinde, kullanılabilirlik grubu adına sağ tıklayın ve ardından **kaynak**  >  **istemci erişim noktası**Ekle ' yi seçin.
   
    ![Kullanılabilirlik grubu için Istemci erişim noktası ekle](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. **Ad** kutusunda, bu yeni dinleyici için bir ad oluşturun, Iki kez **İleri** ' ye ve ardından **son**' a tıklayın.  
    Bu noktada dinleyiciyi veya kaynağı çevrimiçi duruma getirmeyin.

6. **Kaynaklar** sekmesine tıklayın ve ardından yeni oluşturduğunuz istemci erişim noktasını genişletin. 
    Kümenizdeki her küme ağı için IP adresi kaynağı görüntülenir. Bu yalnızca bir Azure çözümünden yalnızca bir IP adresi kaynağı görüntülenir.

7. Aşağıdakilerden birini yapın:
   
   * Karma çözüm yapılandırmak için:
     
        a. Şirket içi alt ağa karşılık gelen IP adresi kaynağına sağ tıklayın ve ardından **Özellikler**' i seçin. IP adresi adı ve ağ adı ' nı aklınızda yapın.
   
        b. **STATIK IP adresi**' ni seçin, KULLANıLMAMıŞ bir IP adresi atayın ve ardından **Tamam**' a tıklayın.
 
   * Yalnızca bir Azure çözümünü yapılandırmak için:

        a. Azure alt ağına karşılık gelen IP adresi kaynağına sağ tıklayın ve ardından **Özellikler**' i seçin.
       
       > [!NOTE]
       > DHCP tarafından seçilen çakışan bir IP adresi nedeniyle dinleyici daha sonra çevrimiçi duruma gelmezse, bu Özellikler penceresinde geçerli bir statik IP adresi yapılandırabilirsiniz.
       > 
       > 

       b. Aynı **IP adresi** özellikleri penceresinde, **IP adresi adını**değiştirin.  
        Bu ad, PowerShell betiğinin $IPResourceName değişkeninde kullanılır. Çözümünüz birden çok Azure sanal ağına yayılırsa, her IP kaynağı için bu adımı tekrarlayın.

