---
title: Azure Içeri/dışarı aktarma işlerinin durumunu görüntüleme | Microsoft Docs
description: Azure Içeri/dışarı aktarma işlerinin ve kullanılan sürücülerin durumunu görüntülemeyi öğrenin. Bir işi işlemek için ne kadar sürdüğünü etkileyen faktörleri anlayın.
author: alkohli
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 03/04/2021
ms.author: alkohli
ms.subservice: common
ms.custom: contperf-fy21q3
ms.openlocfilehash: 8ef18ea663f3a77589d61ed89c50df38f5cf0d0e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102176198"
---
# <a name="view-the-status-of-azure-importexport-jobs"></a>Azure İçeri/Dışarı Aktarma işlerinin durumunu görüntüleme

Bu makalede, Azure Içeri/dışarı aktarma işlerinin sürücü ve iş durumunun nasıl görüntüleneceği hakkında bilgi verilmektedir. Azure Içeri/dışarı aktarma hizmeti, büyük miktarlarda verileri Azure Blob 'larına ve Azure dosyalarına güvenli bir şekilde aktarmak için kullanılır. Hizmet, verileri Azure Blob depolamadan dışarı aktarmak için de kullanılır.  

## <a name="view-job-and-drive-status"></a>İşi ve sürücü durumunu görüntüleme
İçeri aktarma veya dışarı aktarma işlerinizin durumunu Azure portal **içeri/dışarı** aktarma sekmesinde izleyebilirsiniz.
1. Oturum açın https://portal.azure.com/ .
2. **İçeri/dışarı aktarma işleri** için arama yapın.

    ![İçeri/dışarı aktarma işlerinde ara](./media/storage-import-export-view-drive-status/open-import-export-tab.png)

 3. Sayfada Içeri/dışarı aktarma işlerinizin bir listesi görüntülenir.

    ![Iş durumunu görüntüle](./media/storage-import-export-view-drive-status/job-state.png)

4. İş ayrıntılarını görüntülemek için bir işi seçin ve tıklayın.

   ![Ayrıntılı iş durumunu görüntüle](./media/storage-import-export-view-drive-status/job-detail.png)
  
## <a name="view-job-status"></a>İş durumunu görüntüleme

Sürücünüzün işlemde bulunduğu yere bağlı olarak aşağıdaki iş durumlardan birini görürsünüz.

| İş Durumu | Description |
|:--- |:--- |
| Oluşturma | Bir iş oluşturulduktan sonra, durumu **oluşturma** olarak ayarlanır. İş **oluşturuluyor** durumundaysa, Içeri/dışarı aktarma hizmeti, sürücülerin veri merkezine gönderilmemiş olduğunu varsayar. Bir iş bu durumda iki hafta boyunca kalabilir ve sonrasında hizmet tarafından otomatik olarak silinir. |
| Gönderim | Paketinizi gönderdikten sonra, Azure portal izleme bilgilerini güncelleştirmeniz gerekir.  Bunu yapmak işi **Sevkiyat** durumuna getirir. İş, en fazla iki hafta boyunca **Sevkiyat** durumunda kalır. 
| Alındı | Tüm sürücüler veri merkezinde alındıktan sonra iş durumu **alındı** olarak ayarlanır. |
| Aktarılıyor | En az bir sürücü işlemeye başladıktan sonra, iş durumu **aktarma** olarak ayarlanır. Daha fazla bilgi için [Sürücü durumları](#view-drive-status)' na gidin. |
| Paketleme | Tüm sürücüler işlemeyi tamamladıktan sonra, diskler size geri gönderilene kadar iş **paketleme** durumuna yerleştirilir. |
| Tamamlandı | Tüm sürücüler size geri gönderildikten sonra, iş hatasız tamamlanırsa, iş **tamamlandı** olarak ayarlanır. İş, **tamamlandı** durumunda 90 gün sonra otomatik olarak silinir. |
| Kapatıldı | Tüm sürücüler size geri gönderildikten sonra, iş işleme sırasında herhangi bir hata oluşursa iş **kapalı** olarak ayarlanır. İş, **kapalı** durumda 90 gün sonra otomatik olarak silinir. |

## <a name="view-drive-status"></a>Sürücü durumunu görüntüleme

Aşağıdaki tabloda, bir içeri aktarma veya dışarı aktarma işi aracılığıyla geçiş yaparken tek bir sürücünün yaşam döngüsü açıklanmaktadır. Bir işteki her sürücünün geçerli durumu Azure portal görülür.

Aşağıdaki tabloda, bir işteki her bir sürücünün geçeme durumu açıklanmaktadır.

| Sürücü durumu | Description |
|:--- |:--- |
| Belirtilir | İçeri aktarma işi için, iş Azure portal oluşturulduğunda, bir sürücü için ilk durum **belirtilir**. Bir dışarı aktarma işi için, iş oluşturulduğunda bir sürücü belirtilmediğinden, ilk sürücü durumu **alınır**. |
| Alındı | Içeri/dışarı aktarma hizmeti bir içeri aktarma işi için sevkiyat şirketten alınan sürücüleri işlediğinde, sürücü **alındı** durumuna geçer. Bir dışarı aktarma işi için ilk sürücü durumu **alınan** durumudur. |
| NeverReceived | Bir işin paketi ulaştığında, ancak paket sürücüyü içermiyorsa, sürücü **Neverreceived** durumuna gider. Ayrıca, veri merkezi paketi henüz almamışsa ve hizmet gönderim bilgilerini en az iki hafta önce aldıysa bir sürücü bu duruma de getirilir. |
| Aktarılıyor | Hizmet, sürücüden verileri Azure depolama 'ya aktarmaya başladığında bir sürücü **aktarma** durumuna gider. |
| Tamamlandı | Hizmet, tüm verileri hatasız bir şekilde aktarmışsa, bir sürücü **tamamlandı** durumuna gider.
| Completedınfo | Bir sürücü, sürücüye veya sürücüden veri kopyalarken bir sorunla karşılaşmışsa, **Completedınfo** durumuna gider. Bilgiler, Blobların üzerine yazma hakkında hata, uyarı veya bilgi iletileri içerebilir.
| ShippedBack | Bir sürücü, veri merkezinden dönüş adresine geri gönderildiğinde **ShippedBack** durumuna gider. |

Azure portal bu görüntüde örnek bir işin sürücü durumu görüntülenir:

![Sürücü durumunu görüntüle](./media/storage-import-export-view-drive-status/drive-state.png)

Aşağıdaki tabloda, her durum için sürücü hatası durumları ve gerçekleştirilen eylemler açıklanmaktadır.

| Sürücü durumu | Olay | Çözüm/sonraki adım |
|:--- |:--- |:--- |
| Hiç alınmadı | **Neverreceived** olarak işaretlenen bir sürücü (iş sevkiyatının bir parçası olarak alınamadığı için) başka bir sevkiyata ulaştı. | İşlemler ekibi, sürücüyü **alındı** olarak kaydırır. |
| Yok | Başka bir işin parçası olarak veri merkezinde herhangi bir işin parçası olmayan bir sürücü ulaşır. | Sürücü ek sürücü olarak işaretlendi. Özgün paketle ilişkili iş tamamlandığında size döndürülür. |

## <a name="time-to-process-job"></a>İşi işleme süresi
İçeri/dışarı aktarma işinin işlenmesi için gereken süre, şöyle bir dizi etkene göre farklılık gösterir:

-  Sevkiyat Zamanı
-  Veri merkezinde yük
-  Kopyalandığı verilerin iş türü ve boyutu
-  Bir işteki disk sayısı. 

İçeri/dışarı aktarma hizmeti bir SLA 'ya sahip değil, ancak hizmet, diskler alındıktan sonra 7 güne kadar kopyayı tamamlamaya çalışır. Azure portal gönderilen duruma ek olarak, iş ilerlemesini izlemek için REST API 'Lerini kullanabilirsiniz. Kopyalama ilerleme durumunu yüzde olarak görüntülemek için, [işleri](/previous-versions/azure/dn529083(v=azure.100)) işleme API çağrısı ' nda tamamlanma yüzdesi parametresini kullanın.


## <a name="next-steps"></a>Sonraki adımlar

* [AzCopy komut satırı yardımcı programıyla veri aktarma](../storage/common/storage-use-azcopy-v10.md)
* [Azure Içeri aktarma dışarı aktarma REST API örneği](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/)