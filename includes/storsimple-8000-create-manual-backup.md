---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 4fc92931979aa367bdead435c3d6fd758d66a397
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188446"
---
#### <a name="to-create-a-manual-backup"></a>El ile yedekleme oluşturmak için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Tablosal cihaz listesinden cihazınızı seçin. **Ayarlar > Yönet > Yedekleme ilkeleri** seçeneğine gidin.

2. **Yedekleme ilkeleri** dikey penceresi, yedeklemek istediğiniz birimle ilgili ilke de dahil olmak üzere yedekleme ilkelerini tablosal olarak listeler. Yedeklemek istediğiniz birimle ilişkili ilkeyi seçin ve bağlam menüsünü açmak için ilkeye sağ tıklayın. Açılan listeden **Şimdi yedekle**’yi seçin.

    ![El ile yedekleme oluşturma](./media/storsimple-8000-create-manual-backup/createmanualbu1.png)

3. **Şimdi yedekle** dikey penceresinde aşağıdaki adımları gerçekleştirin:

    1. Açılan listeden uygun **Anlık görüntü türü**’nü seçin: **Yerel** anlık görüntü veya **Bulut** anlık görüntüsü. Yedekleme ve geri yükleme işlemlerinin hızlı olması için yerel anlık görüntüyü, veri dayanıklılığı için bulut anlık görüntüsünü seçin.

        ![El ile yedekleme oluşturma](./media/storsimple-8000-create-manual-backup/createmanualbu2.png)

    2. Anlık görüntü oluşturmak üzere bir iş başlatmak için **Tamam**’a tıklayın. İş başarıyla oluşturulduktan sonra sayfanın üstünde bir bildirim görürsünüz.

        ![El ile yedekleme oluşturma](./media/storsimple-8000-create-manual-backup/createmanualbu4.png)

    3. İşi izlemek için bildirime tıklayın. Bunu yaptığınızda, iş ilerleme durumunu görüntüleyebileceğiniz **İşler** dikey penceresine yönlendirilirsiniz.


5. Yedekleme işi tamamlandıktan sonra **Yedekleme kataloğu** sekmesine gidin.

6. Filtre seçimlerini uygun aygıt, yedekleme İlkesi ve zaman aralığına ayarlayın. Yedekleme, yedekleme kataloğunda görüntülenen yedekleme ayarları listesinde görüntülenmelidir.

