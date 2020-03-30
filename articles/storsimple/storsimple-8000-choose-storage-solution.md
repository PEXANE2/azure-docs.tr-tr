---
title: Bir cihaz kullanarak Azure'a veri aktarımı seçenekleri | Microsoft Dokümanlar
description: Azure'a veri aktarmak için doğru cihazı nasıl seçeceğinizi öğrenin
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: article
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: 9456d975ffb6d7726166f9badbcb8fe0ccbe5256
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68965353"
---
# <a name="compare-storsimple-with-azure-file-sync-and-data-box-edge-data-transfer-options"></a>StorSimple’ı Azure Dosya Eşitleme ve Data Box Edge veri aktarma seçenekleriyle karşılaştırma 

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]
 
Bu belge, Azure'a şirket içi veri aktarımı seçeneklerine genel bir bakış sağlar ve karşılaştırma sağlar: Veri Kutusu Kenarı ve Azure Dosya Eşitlemi ile StorSimple 8000 serisi.

- **[Data Box Edge](/azure/databox-online/data-box-edge-overview)** – Data Box Edge, verileri Azure'a girip çıkan ve çıkan ve yükleme sırasında verileri ön işlemiçin AI özellikli Edge bilgi işlemciliğe sahip şirket içi bir ağ aygıtıdır. Veri Kutusu Ağ Geçidi, aynı veri aktarım özelliklerine sahip aygıtın sanal bir sürümüdür.
- **[Azure Dosya Eşitleme](/azure/storage/files/storage-sync-files-deployment-guide)** – Azure Dosya Eşitlemi, kurum içi bir dosya sunucusunun esnekliğini, performansını ve uyumluluğunu koruyarak kuruluşunuzun Dosya paylaşımlarını Azure Dosyaları'nda merkezileştirmek için kullanılabilir. Azure Dosya Eşitleme, Windows Server’ı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. Azure Dosya Eşitlemi'nin genel kullanılabilirliği 2018'in başlarında duyuruldu.
- **[StorSimple](/azure/storsimple/storsimple-overview)** – StorSimple, kuruluşların Azure depolamayla sıkı bir şekilde entegre olarak tek bir çözümde birincil depolama, veri koruma, arşivleme ve olağanüstü durum kurtarma için depolama altyapılarını birleştirmelerine yardımcı olan karma bir aygıttır. StorSimple için ürün yaşam döngüsü [burada](https://support.microsoft.com/lifecycle/search?alpha=Azure%20StorSimple%208000%20Series)bulunabilir.

## <a name="comparison-summary"></a>Karşılaştırma özeti

|                           |StorSimple 8000   |Azure Dosya Eşitleme   |Data Box Edge           |
|---------------------------|----------------------------------------|-------------------------------|-----------------------------------------|
|Genel Bakış         |Katmanlı hibrid depolama ve arşiv|Bulut katmanlama ve çok siteli eşitleme ile genel dosya sunucusu depolama.  |İşlem öncesi veriler için depolama çözümü ve ağ üzerinden Azure'a gönderilmesi.        |
|Senaryolar        |Dosya sunucusu, arşivleme, yedekleme hedefi |Dosya sunucusu, arşivleme (çok siteli)   |Veri aktarımı, ML inferencing, IoT, arşivleme dahil olmak üzere veri ön işleme    |
|Uç işlemi     |Kullanılamaz |Kullanılamaz |Azure IoT Edge kullanarak kapsayıcıçalıştırmayı destekler    |
|Form faktörü      |Fiziksel cihaz   |Windows Server'da yüklü aracı |Fiziksel cihaz   |
|Donanım         |Hizmetin bir parçası olarak Microsoft'tan sağlanan fiziksel aygıt | Sağlanan müşteri |Hizmetin bir parçası olarak Microsoft'tan sağlanan fiziksel aygıt  |
|Veri biçimi      |Özel biçim   |Dosyalar         |Blobs veya Dosyalar    |
|Protokol desteği |iSCSI          |Kobİ, NFS    | Kobİ veya NFS      |
|Fiyatlandırma          |[StorSimple](https://azure.microsoft.com/pricing/details/storsimple/) |[Azure Dosya Eşitleme](https://azure.microsoft.com/pricing/details/storage/files/)  |[Data Box Edge](https://azure.microsoft.com/pricing/details/storage/databox/edge/)  |

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Veri Kutusu Kenarı](/azure/databox-online/data-box-edge-overview) ve [Azure Veri Kutusu Ağ Geçidi](/azure/databox-online/data-box-gateway-overview) hakkında bilgi edinin
- Azure [Dosya Eşitleme](/azure/storage/files/storage-sync-files-deployment-guide) hakkında bilgi edinin
