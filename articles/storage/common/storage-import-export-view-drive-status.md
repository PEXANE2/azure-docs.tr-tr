---
title: Azure İçe Alma/Dışa Aktarma işlerinin durumunu görüntüleme | Microsoft Dokümanlar
description: İçe Aktarma/Dışa Aktarma işlerinin ve kullanılan sürücülerin durumunu nasıl görüntüleyebilirsiniz öğrenin.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 222c893f06d9adf77f8a8124af18bc03c5d20bdf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72821433"
---
# <a name="view-the-status-of-azure-importexport-jobs"></a>Azure İçeri/Dışarı Aktarma işlerinin durumunu görüntüleme

Bu makalede, Azure İçe Alma/Dışa Aktarma işleri için sürücü ve iş durumunun nasıl görüntülenenekadar görüntülenemeye ilişkin bilgiler verilmektedir. Azure İçe Alma/Verme hizmeti, büyük miktarda veriyi Azure Blobs ve Azure Dosyalarına güvenli bir şekilde aktarmak için kullanılır. Hizmet, Azure Blob depolamadan veri dışa aktarmak için de kullanılır.  

## <a name="view-job-and-drive-status"></a>İş ve sürücü durumunu görüntüleme
Azure portalından alma veya dışa aktarma işlerinizin durumunu izleyebilirsiniz. **İçe/Aktar** sekmesini tıklatın. Sayfada işyerlerinizin listesi görünür.

![İş Durumunu Görüntüle](./media/storage-import-export-service/jobstate.png)

## <a name="view-job-status"></a>İş durumunu görüntüleme

Sürücünüzün işlemde nerede olduğuna bağlı olarak aşağıdaki iş durumlarından birini görürsünüz.

| İş Durumu | Açıklama |
|:--- |:--- |
| Oluşturma | Bir iş oluşturulduktan sonra, durumu **Oluşturma**olarak ayarlanır. İş **Oluşturma** durumundayken, İçe/Dışa Aktar hizmeti, sürücülerin veri merkezine sevk edilmedigini varsayar. Bir iş bu durumda iki haftaya kadar kalabilir ve ardından hizmet tarafından otomatik olarak silinir. |
| Sevkiyat | Paketinizi gönderip göndermeden sonra, izleme bilgilerini Azure portalında güncelleştirmeniz gerekir.  Bu da işi **Nakliye** durumuna dönüştürür. İş, nakliye **eyaletinde** iki haftaya kadar devam edecek. 
| Alındı | Tüm sürücüler veri merkezinde alındıktan sonra, iş durumu **Alınan**olarak ayarlanır. |
| Aktarma | En az bir sürücü işlemeye başladıktan sonra, iş durumu **Aktarım**olarak ayarlanır. Daha fazla bilgi için [Drive States'e](#view-drive-status)gidin. |
| Paketleme | Tüm sürücüler işlemi tamamladıktan sonra, sürücüler size geri gönderilene kadar iş **Paketleme** durumuna yerleştirilir. |
| Tamamlandı | Tüm sürücüler size geri gönderildikten sonra, iş hatasız tamamlandıysa, iş **Tamamlandı**olarak ayarlanır. Tamamlanan **durumda** 90 gün sonra iş otomatik olarak silinir. |
| Closed | Tüm sürücüler size geri sevk edildikten sonra, işin işlenmesi sırasında herhangi bir hata varsa, iş **Kapalı**olarak ayarlanır. İş, **Kapalı** durumda 90 gün sonra otomatik olarak silinir. |

## <a name="view-drive-status"></a>Sürücü durumunu görüntüleme

Aşağıdaki tabloda, bir alma veya dışa aktarma işi aracılığıyla geçiş olarak tek bir sürücünün yaşam döngüsü açıklanmaktadır. Bir işteki her sürücünün geçerli durumu Azure portalında görülür.

Aşağıdaki tabloda, bir işteki her sürücünün geçebileceği her durum açıklanmaktadır.

| Sürücü Durumu | Açıklama |
|:--- |:--- |
| Belirtilen | Bir alma işi için, iş Azure portalından oluşturulduğunda, sürücünün ilk durumu **belirtilir.** Bir dışa aktarma işi için, iş oluşturulduğunda sürücü belirtilmediğinden, ilk sürücü durumu **alınır.** |
| Alındı | Alma/Dışa Aktarma hizmeti, bir ithalat işi için sevkiyat şirketinden alınan sürücüleri işlediğinde alınan **duruma** geçiş ler. Bir dışa aktarma işi için, ilk sürücü durumu **Alınan** durumdur. |
| Hiç Alınmadı | Sürücü, bir iş paketi geldiğinde **NeverReceived** durumuna taşınır, ancak paket sürücüyü içermez. Hizmetin sevkiyat bilgilerini almasının üzerinden iki hafta geçmesine rağmen, paket henüz veri merkezine gelmemişse, bir sürücü de bu duruma taşınır. |
| Aktarma | Hizmet, sürücüden Azure Depolama'ya veri aktarmaya başladığında sürücü **Aktarım** durumuna geçer. |
| Tamamlandı | Hizmet, hatasız tüm verileri başarıyla aktardığında, sürücü **Tamamlanan** durumuna taşınır.
| TamamlandıMoreInfo | Bir sürücü, hizmet sürücüden veya sürücüye veri kopyalarken bazı sorunlarla karşılaştığında **CompletedMoreInfo** durumuna taşınır. Bilgiler, hatalar, uyarılar veya blobs üzerine yazma hakkında bilgilendiriciler iletileri içerebilir.
| Geri Sevkiyat | Bir sürücü, veri merkezinden geri dönüş adresine sevk edildiğinde **Sevk Geri Durumu'na** taşınır. |

Azure portalındaki bu görüntü, örnek bir işin sürücü durumunu görüntüler:

![Sürücü Durumunu Görüntüle](./media/storage-import-export-service/drivestate.png)

Aşağıdaki tabloda sürücü hatası durumları ve her durum için alınan eylemleri açıklanır.

| Sürücü Durumu | Olay | Çözünürlük / Sonraki adım |
|:--- |:--- |:--- |
| Hiç Alınmadı | **NeverReceived** olarak işaretlenmiş bir sürücü (işin sevkiyatının bir parçası olarak alınmadığından) başka bir sevkiyata gelir. | Operasyon ekibi sürücüyü **Alınan'a**taşır. |
| Yok | Herhangi bir işin parçası olmayan bir sürücü, başka bir işin parçası olarak veri merkezine gelir. | Sürücü ek sürücü olarak işaretlenir ve özgün paketle ilişkili iş tamamlandığında size döndürülür. |

## <a name="time-to-process-job"></a>İşi işleme zamanı
Bir alma/dışa aktarma işini işlemek için gereken süre, aşağıdaki gibi bir dizi etkene bağlı olarak değişir:

-  Sevkiyat süresi
-  Veri merkezine yük
-  Kopyalanan verilerin iş türü ve boyutu
-  Bir işteki disk sayısı. 

Alma/Dışa Aktarma hizmetinde SLA yoktur, ancak hizmet, diskler alındıktan sonra kopyayı 7 ila 10 gün içinde tamamlamaya çalışır. Azure Portalı'nda yayınlanan duruma ek olarak, REST API'leri iş ilerlemesini izlemek için kullanılabilir. [Liste İşleri](/previous-versions/azure/dn529083(v=azure.100)) çalışması API çağrısındaki yüzde tam parametresi yüzde kopya ilerleme sağlar.


## <a name="next-steps"></a>Sonraki adımlar

* [WAImportExport aracını ayarlama](storage-import-export-tool-how-to.md)
* [AzCopy komut satırı yardımcı programı ile veri aktarımı](storage-use-azcopy.md)
* [Azure İçe Aktarma REST API örneği](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/)
