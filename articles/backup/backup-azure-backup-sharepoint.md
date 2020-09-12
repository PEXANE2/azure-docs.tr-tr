---
title: DPM ile bir SharePoint grubunu Azure 'a yedekleme
description: Bu makalede, Azure 'da bir SharePoint grubunun DPM/Azure Backup sunucu korumasına genel bakış sunulmaktadır
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: a667626fb53920150a4b5a16759228aacbfae75d
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89375253"
---
# <a name="back-up-a-sharepoint-farm-to-azure-with-dpm"></a>DPM ile bir SharePoint grubunu Azure 'a yedekleme

System Center Data Protection Manager (DPM) kullanarak Microsoft Azure için bir SharePoint grubunu diğer veri kaynaklarını yedeklemekten çok benzer şekilde yedekleyerek. Azure Backup günlük, haftalık, aylık veya yıllık yedekleme noktaları oluşturmak için yedekleme zamanlamasında esneklik sağlar ve çeşitli yedekleme noktaları için bekletme ilkesi seçenekleri sunar. DPM, hızlı kurtarma zamanı hedeflerine (RTO) yönelik yerel disk kopyalarını depolamanıza ve ekonomik, uzun süreli saklama için Azure 'a kopya depolamanıza olanak sağlar.

SharePoint 'i DPM ile Azure 'a yedeklemek, SharePoint 'i yerel olarak DPM 'ye yedeklemeye yönelik çok benzer bir işlemdir. Bu makalede Azure için belirli hususlar belirtilecektir.

## <a name="sharepoint-supported-versions-and-related-protection-scenarios"></a>SharePoint desteklenen sürümleri ve ilgili koruma senaryoları

Desteklenen SharePoint sürümleri ve bunları yedeklemek için gereken DPM sürümleri için bkz: [DPM ne yedekleyebilir?](/system-center/dpm/dpm-protection-matrix?view=sc-dpm-2019#applications-backup)

## <a name="before-you-start"></a>Başlamadan önce

Bir SharePoint grubunu Azure 'a yedeklemebilmeniz için öncelikle onaylamanız gereken birkaç nokta vardır.

### <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce, iş yüklerini korumak için [Microsoft Azure Backup kullanmaya yönelik tüm önkoşulları](backup-azure-dpm-introduction.md#prerequisites-and-limitations) karşıladığınızdan emin olun. Önkoşullar için bazı görevler şunlardır: bir yedekleme Kasası oluşturma, kasa kimlik bilgilerini indirme, Azure Backup Aracısı yükleme ve DPM/Azure Backup Sunucusu 'i kasaya kaydetme.

Ek Önkoşullar ve sınırlamalar, [DPM Ile yedekleme](/system-center/dpm/back-up-sharepoint?view=sc-dpm-2019#prerequisites-and-limitations) makalesinde bulunabilir.

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma

SharePoint grubunu yedeklemek için ConfigureSharePoint.exe dosyasını kullanarak SharePoint için koruma yapılandırır ve ardından DPM’de bir koruma grubu oluşturursunuz. Yönergeler için bkz. DPM belgelerinde [yedeklemeyi yapılandırma](/system-center/dpm/back-up-sharepoint?view=sc-dpm-2019#configure-backup) .

## <a name="monitoring"></a>İzleme

Yedekleme işini izlemek için [DPM yedeklemesini izleme](/system-center/dpm/back-up-sharepoint?view=sc-dpm-2019#monitoring) bölümündeki yönergeleri izleyin.

## <a name="restore-sharepoint-data"></a>SharePoint verilerini geri yükleme

DPM ile bir diskten bir SharePoint öğesini geri yüklemeyi öğrenmek için bkz. [SharePoint verilerini geri yükleme](/system-center/dpm/back-up-sharepoint?view=sc-dpm-2019#restore-sharepoint-data).

## <a name="restore-a-sharepoint-database-from-azure-by-using-dpm"></a>DPM kullanarak bir SharePoint veritabanını Azure 'dan geri yükleme

1. Bir SharePoint içerik veritabanını kurtarmak için, çeşitli kurtarma noktalarına göz atarak (daha önce gösterildiği gibi) ve geri yüklemek istediğiniz kurtarma noktasını seçin.

    ![DPM SharePoint Protection8](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection9.png)
2. SharePoint kurtarma noktasına çift tıklayarak kullanılabilir SharePoint kataloğu bilgilerini görüntüleyin.

   > [!NOTE]
   > SharePoint grubu Azure 'da uzun süreli saklama için korunduğundan, DPM sunucusunda hiçbir katalog bilgisi (meta veri) yok. Sonuç olarak, her zaman bir noktadan SharePoint içerik veritabanının kurtarılması gerektiğinde SharePoint grubunu yeniden kataloglanmalıdır.
   >
   >
3. **Yeniden katalogla**' yı seçin.

    ![DPM SharePoint Protection10](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection12.png)

    **Bulut yeniden Kataloglandırma** durumu penceresi açılır.

    ![DPM SharePoint Protection11](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection13.png)

    Kataloglama tamamlandıktan sonra durum *başarılı*olarak değişir. **Kapat**’ı seçin.

    ![DPM SharePoint Protection12](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection14.png)
4. İçerik veritabanı yapısını almak için DPM **Kurtarma** sekmesinde gösterilen SharePoint nesnesini seçin. Öğeye sağ tıklayın ve ardından **kurtar**' ı seçin.

    ![DPM SharePoint Protection13](./media/backup-azure-backup-sharepoint/dpm-sharepoint-protection15.png)
5. Bu noktada, bir SharePoint içerik veritabanını diskten kurtarmak için bu makalenin önceki kısımlarında yer alarak kurtarma adımlarını izleyin.

## <a name="switching-the-front-end-web-server"></a>Ön uç Web sunucusunu değiştirme

Birden fazla ön uç Web sunucunuz varsa ve DPM 'nin grubu korumak için kullandığı sunucuyu değiştirmek istiyorsanız, [ön uç Web sunucusunu değiştirme](/system-center/dpm/back-up-sharepoint?view=sc-dpm-2019#switching-the-front-end-web-server)bölümündeki yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Backup Sunucusu ve DPM-SSS](backup-azure-dpm-azure-server-faq.md)
* [System Center Data Protection Manager sorunlarını giderme](backup-azure-scdpm-troubleshooting.md)
