---
title: DPM ile SharePoint çiftlerini Azure'a yedekleme
description: Bu makale, Bir SharePoint çiftliğinin DPM/Azure Yedekleme sunucu korumasına genel bir bakış sağlar
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: 0199495e3b0eb002e58c096ed9abf05d46f43f97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80054121"
---
# <a name="back-up-a-sharepoint-farm-to-azure-with-dpm"></a>DPM ile SharePoint çiftlerini Azure'a yedekleme

Diğer veri kaynaklarını yedeklemek gibi System Center Data Protection Manager (DPM) kullanarak bir SharePoint çiftliğini Microsoft Azure'a yedeklersiniz. Azure Yedekleme, günlük, haftalık, aylık veya yıllık yedekleme noktaları oluşturmak için yedekleme zamanlamasında esneklik sağlar ve çeşitli yedekleme noktaları için bekletme ilkesi seçenekleri sunar. DPM, yerel disk kopyalarını hızlı kurtarma zamanı hedefleri (RTO) için depolama ve kopyaları ekonomik ve uzun süreli bekletme için Azure'da depolama olanağı sağlar.

SharePoint'i DPM ile Azure'a yedeklemek, SharePoint'i Yerel Olarak DPM'ye yedeklemeye çok benzer bir işlemdir. Azure için özellikle dikkat edilmesi gereken hususlar bu makalede belirtilecektir.

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a>SharePoint desteklenen sürümler ve ilgili koruma senaryoları

Desteklenen SharePoint sürümleri ve bunları yedeklemek için gereken DPM sürümleri için bkz: [DPM ne yedekleyebilir?](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-2019#applications-backup)

## <a name="before-you-start"></a>Başlamadan önce

Bir SharePoint çiftliğini Azure'a yedeklemeden önce onaylamanız gereken birkaç şey vardır.

### <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce, iş yüklerini korumak [için Microsoft Azure Yedekleme'yi kullanmanın](backup-azure-dpm-introduction.md#prerequisites-and-limitations) tüm ön koşulları yerine getirildiğinden emin olun. Ön koşullar için bazı görevler şunlardır: yedek kasa oluşturmak, kasa kimlik bilgilerini indirmek, Azure Yedekleme Aracısı'nı yüklemek ve DPM/Azure Yedekleme Sunucusu'nu kasaya kaydettirmek.

Ek önkoşullar ve sınırlamalar DPM makalesi [ile SharePoint'i](https://docs.microsoft.com/system-center/dpm/back-up-sharepoint?view=sc-dpm-2019#prerequisites-and-limitations) Yedekle'de bulunabilir.

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma

SharePoint grubunu yedeklemek için ConfigureSharePoint.exe dosyasını kullanarak SharePoint için koruma yapılandırır ve ardından DPM’de bir koruma grubu oluşturursunuz. Yönergeler için DPM belgelerinde [Yedeklemeyi Yapılandır'a](https://docs.microsoft.com//system-center/dpm/back-up-sharepoint?view=sc-dpm-2019#configure-backup) bakın.

## <a name="monitoring"></a>İzleme

Yedekleme işini izlemek için, [DPM yedeklemesini izleme](https://docs.microsoft.com/system-center/dpm/back-up-sharepoint?view=sc-dpm-2019#monitoring) yönergelerini izleyin

## <a name="restore-sharepoint-data"></a>SharePoint verilerini geri yükleme

Bir SharePoint öğesini DPM'li bir diskten nasıl geri yükleyebilirsiniz öğrenmek için [bkz.](https://docs.microsoft.com/system-center/dpm/back-up-sharepoint?view=sc-dpm-2019#restore-sharepoint-data)

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a>DPM'yi kullanarak Azure'dan SharePoint veritabanını geri yükleme

1. Bir SharePoint içerik veritabanını kurtarmak için çeşitli kurtarma noktalarına (daha önce gösterildiği gibi) göz atın ve geri yüklemek istediğiniz kurtarma noktasını seçin.

    ![DPM SharePoint Protection8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. Kullanılabilir SharePoint katalog bilgilerini göstermek için SharePoint kurtarma noktasını çift tıklatın.

   > [!NOTE]
   > SharePoint çiftliği Azure'da uzun süreli bekletme için korunduğundan, DPM sunucusunda katalog bilgisi (meta veri) bulunmaz. Sonuç olarak, zaman içinde bir SharePoint içerik veritabanının kurtarılması gerektiğinde, SharePoint farm'ını yeniden kataloglamanız gerekir.
   >
   >
3. **Yeniden Katalog'u**tıklatın.

    ![DPM SharePoint Protection10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

    **Bulut Yeniden Katalog** durum penceresi açılır.

    ![DPM SharePoint Protection11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    Kataloglama tamamlandıktan sonra durum *Başarı*olarak değişir. **Kapat'ı**tıklatın.

    ![DPM SharePoint Protection12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. İçerik veritabanı yapısını almak için DPM **Kurtarma** sekmesinde gösterilen SharePoint nesnesini tıklatın. Öğeyi sağ tıklatın ve sonra **Kurtar'ı**tıklatın.

    ![DPM SharePoint Protection13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. Bu noktada, bir SharePoint içerik veritabanını diskten kurtarmak için bu makalenin önceki kurtarma adımlarını izleyin.

## <a name="switching-the-front-end-web-server"></a>Ön Uç Web Sunucusunu Değiştirme

Birden fazla ön uç web sunucunuz varsa ve DPM'nin çiftliği korumak için kullandığı sunucuyu değiştirmek istiyorsanız, [Ön Uç Web Sunucusunu Değiştirme'deki](https://docs.microsoft.com/system-center/dpm/back-up-sharepoint?view=sc-dpm-2019#switching-the-front-end-web-server)yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Yedekleme Sunucusu ve DPM - SSS](backup-azure-dpm-azure-server-faq.md)
* [System Center Data Protection Manager sorunlarını giderme](backup-azure-scdpm-troubleshooting.md)
