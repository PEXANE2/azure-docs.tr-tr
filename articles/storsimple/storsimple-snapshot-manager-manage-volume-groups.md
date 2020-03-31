---
title: StorSimple Snapshot Manager birim grupları | Microsoft Dokümanlar
description: Hacim grupları oluşturmak ve yönetmek için StorSimple Snapshot Manager MMC snap-in'in nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: twooley
manager: carmonm
editor: ''
ms.assetid: 7a232414-6a28-4b81-bd7b-cf61e28b33d7
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: twooley
ms.openlocfilehash: 5198729cf96fb48c5dcd05096c04ea4d77c26de5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75931487"
---
# <a name="use-storsimple-snapshot-manager-to-create-and-manage-volume-groups"></a>Birim grupları oluşturmak ve yönetmek için StorSimple Snapshot Manager'ı kullanın
## <a name="overview"></a>Genel Bakış
Birim gruplarına hacim atamak, birim grubu hakkındaki bilgileri görüntülemek, yedeklemeleri zamanlamak ve birim gruplarını düzenleyin için **Kapsam** bölmesindeki **Birim Grupları** düğümünü kullanabilirsiniz.

Birim grupları, yedeklemelerin uygulama tutarlı olduğundan emin olmak için kullanılan ilgili birimlerin havuzlarıdır. Daha fazla bilgi [için, Birimler ve birim grupları](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) ve Windows Birim Gölge Kopyalama Hizmeti ile [Tümleştirme](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service)bakın.

> [!IMPORTANT]
> * Bir birim grubundaki tüm birimler tek bir bulut hizmeti sağlayıcısından gelmelidir.
> * Birim gruplarını yapılandırdığınızda, kümepaylaşılan birimleri (CSV' ler) ve C'siz birimleri aynı birim grubunda karıştırmayın. StorSimple Snapshot Manager, aynı anlık görüntüdeki CV'lerin ve C'olmayanların bir karışımını desteklemez.

![Birim grupları düğümü](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**Şekil 1: StorSimple Snapshot Manager Birim Grupları düğümü** 

Bu öğretici, StorSimple Snapshot Manager'ı şu şekilde nasıl kullanabileceğinizi açıklar:

* Birim gruplarınızla ilgili bilgileri görüntüleme
* Birim grubu oluşturma
* Birim grubunu yedekleme
* Birim grubunu edin
* Birim grubunu silme

Tüm bu eylemler **Eylemler** bölmesinde de kullanılabilir.

## <a name="view-volume-groups"></a>Birim gruplarını görüntüleme
**Birim Grupları** düğümunu tıklattığınızda, **Sonuçlar** bölmesi yaptığınız sütun seçimlerine bağlı olarak her birim grubu hakkında aşağıdaki bilgileri gösterir. **(Sonuçlar** bölmesindeki sütunlar yapılandırılabilir. **Birimler** düğümüne sağ tıklayın, **Görünüm'ü**seçin ve ardından **Sütun Ekle/Kaldır'ı**seçin .)

| Sonuçlar sütunu | Açıklama |
|:--- |:--- |
| Adı |**Ad** sütunu ses grubunun adını içerir. |
| Uygulama |**Uygulamalar** sütunu, Windows ana bilgisayarda şu anda yüklenen ve çalışan VSS yazarlarının sayısını gösterir. |
| Seçildi |**Seçili** sütun, birim grubunda bulunan birim sayısını gösterir. Sıfır (0), birim grubundaki birimlerle hiçbir uygulamanın ilişkili olmadığını gösterir. |
| Alınan |**İçe aktarılan** sütun, alınan birim sayısını gösterir. **True**olarak ayarlandığında, bu sütun bir birim grubunun Azure portalından alındığını ve StorSimple Snapshot Manager'da oluşturulmadığını gösterir. |

> [!NOTE]
> StorSimple Snapshot Manager birim grupları da Azure portalındaki **Yedekleme İlkeleri** sekmesinde görüntülenir.
> 
> 

## <a name="create-a-volume-group"></a>Birim grubu oluşturma
Bir birim grubu oluşturmak için aşağıdaki yordamı kullanın.

#### <a name="to-create-a-volume-group"></a>Birim grubu oluşturmak için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** bölmesinde, **Birim Grupları'nı**sağ tıklatın ve ardından Birim **Grubu Oluştur'u**tıklatın.
   
    ![Birim grubu oluşturma](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
   
    **Birim grubu oluştur** iletişim kutusu görüntülenir.
   
    ![Birim grubu iletişim kutusu oluşturma](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png)
3. Aşağıdaki bilgileri girin:
   
   1. **Ad** kutusuna, yeni birim grubu için benzersiz bir ad yazın.
   2. **Uygulamalar** kutusunda, birim grubuna ekleyeceğiniz birimlerle ilişkili uygulamaları seçin.
      
       **Uygulamalar** kutusu yalnızca StorSimple birimlerini kullanan ve VSS yazarlarının etkinleştirilmiş uygulamalarını listeler. VsS yazarı, yalnızca yazarın farkında olduğu tüm birimler StorSimple birimleriyse etkinleştirilir. Uygulamalar kutusu boşsa, Azure StorSimple birimlerini kullanan ve desteklenen VSS yazarlarını kullanan hiçbir uygulama yüklenmez. (Şu anda Azure StorSimple Microsoft Exchange ve SQL Server'ı destekler.) VSS yazarları hakkında daha fazla bilgi için [Windows Birim Gölge Kopyalama Hizmeti ile Tümleştirme'ye](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service)bakın.
      
       Bir uygulama seçerseniz, uygulamayla ilişkili tüm birimler otomatik olarak seçilir. Tam tersine, belirli bir uygulamayla ilişkili birimleri seçerseniz, uygulama **Uygulamalar** kutusunda otomatik olarak seçilir. 
   3. **Birimler** kutusunda, birim grubuna eklemek için StorSimple birimlerini seçin. 
      
      * Tek veya birden çok bölümlü birimleri ekleyebilirsiniz. (Birden çok bölümlü birim, dinamik diskler veya birden çok bölümlü temel diskler olabilir.) Birden çok bölüm içeren bir birim tek bir birim olarak kabul edilir. Sonuç olarak, bir birim grubuna bölümlerden yalnızca birini eklerseniz, diğer tüm bölümler otomatik olarak aynı anda bu birim grubuna eklenir. Bir birim grubuna birden çok bölüm birimi ekledikten sonra, birden çok bölüm birimi tek bir birim olarak işlem göremeye devam eder.
      * Bunlara herhangi bir birim atamayarak boş birim grupları oluşturabilirsiniz. 
      * Küme paylaşılan birimleri (CSV' ler) ve CSV'leri aynı birim grubunda karıştırmayın. StorSimple Snapshot Manager, aynı anlık görüntüdeki CSV birimleri ve CSV olmayan birimlerin bir karışımını desteklemez.
4. Birim grubunu kaydetmek için **Tamam'ı** tıklatın.

## <a name="back-up-a-volume-group"></a>Birim grubunu yedekleme
Bir birim grubunu yedeklemek için aşağıdaki yordamı kullanın.

#### <a name="to-back-up-a-volume-group"></a>Birim grubunu yedeklemek için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** bölmesinde, Birim **Grupları** düğümünü genişletin, bir birim grup adını sağ tıklatın ve sonra Yedekleme **yi al'ı**tıklatın.
   
    ![Ses grubunu hemen yedekleme](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)
3. Yedekleme **Al** iletişim kutusunda Yerel **Anlık Görüntü** veya Bulut Anlık **Görüntüsü'nü**seçin ve ardından **Oluştur'u**tıklatın.
   
    ![Yedekleme iletişim kutusunu alma](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png)
4. Yedeklemenin çalıştığını doğrulamak için **İşler** düğümini genişletin ve ardından **Çalıştır'ı**tıklatın. Yedekleme listelenmelidir.
5. Tamamlanan anlık görüntügörüntülemek için **Yedek Katalog** düğümündekini genişletin, birim grup adını genişletin ve ardından Yerel Anlık **Görüntü** veya Bulut **Anlık Görüntüsü'ni**tıklatın. Başarılı bir şekilde tamamlanırsa yedekleme listelenir.

## <a name="edit-a-volume-group"></a>Birim grubunu edin
Bir birim grubunu yeniden sağlamak için aşağıdaki yordamı kullanın.

#### <a name="to-edit-a-volume-group"></a>Birim grubunu yeniden yapmak için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** bölmesinde, Birim **Grupları** düğümünü genişletin, bir birim grup adını sağ tıklatın ve sonra **Edit'i**tıklatın.
3. **Birim grubu oluştur **iletişim kutusu görüntülenir. **Ad,** Uygulamalar ve **Birimler** **girişlerini**değiştirebilirsiniz.
4. Değişikliklerinizi kaydetmek için **Tamam**’a tıklayın.

## <a name="delete-a-volume-group"></a>Birim grubunu silme
Bir birim grubunu silmek için aşağıdaki yordamı kullanın. 

> [!WARNING]
> Bu, ses grubuyla ilişkili tüm yedeklemeleri de siler.
> 
> 

#### <a name="to-delete-a-volume-group"></a>Bir birim grubunu silmek için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** bölmesinde, Birim **Grupları** düğümünü genişletin, bir birim grup adını sağ tıklatın ve sonra **Sil'i**tıklatın.
3. **Birim Grubu Sil** iletişim kutusu görüntülenir. Metin kutusuna **Onayla** yazın ve ardından **Tamam'ı**tıklatın.
   
    Silinen birim grubu **Sonuçlar** bölmesindeki listeden kaybolur ve bu birim grubuyla ilişkili tüm yedeklemeler yedekleme kataloğundan silinir.

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager'ı](storsimple-snapshot-manager-admin.md)nasıl kullanacağınızı öğrenin.
* [Yedekleme ilkeleri oluşturmak ve yönetmek için StorSimple Snapshot Manager'ı](storsimple-snapshot-manager-manage-backup-policies.md)nasıl kullanacağınızı öğrenin.

