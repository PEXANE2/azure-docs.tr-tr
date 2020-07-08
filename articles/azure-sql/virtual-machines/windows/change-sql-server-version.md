---
title: SQL Server sürümden yerinde değişiklik
description: Azure 'da SQL Server sanal makinenizin sürümünü değiştirmeyi öğrenin.
services: virtual-machines-windows
documentationcenter: na
author: ramakoni1
manager: ramakoni1
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/08/2020
ms.author: RamaKoni
ms.reviewer: sqlblt, daleche
ms.custom: seo-lt-2019
ms.openlocfilehash: 63c4c7b596a32901d127642118d224c2b318510f
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85963028"
---
# <a name="in-place-change-of-sql-server-version-on-azure-vm"></a>Azure VM'deki SQL Server sürümünü yerinde değiştirme

[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Bu makalede, Microsoft Azure bir Windows sanal makinesinde (VM) Microsoft SQL Server sürümünün nasıl değiştirileceği açıklanır.

## <a name="prerequisites"></a>Ön koşullar

SQL Server yerinde yükseltmesi yapmak için aşağıdaki koşullar geçerlidir:

- SQL Server istenen sürümünün Kurulum medyası gereklidir. [Yazılım güvencesi](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) sahibi olan müşteriler, yükleme medyasını [toplu lisanslama merkezinden](https://www.microsoft.com/Licensing/servicecenter/default.aspx)elde edebilir. Yazılım Güvencesi sahibi olmayan müşteriler, SQL Server daha sonraki bir sürümü olan (genellikle C:\SQLServerFull içinde bulunur) bir Azure Marketi SQL Server VM görüntüsünden kurulum medyasını kullanabilir.
- Sürüm yükseltmeleri, [destek yükseltme yollarını](https://docs.microsoft.com/sql/database-engine/install-windows/supported-version-and-edition-upgrades-version-15?view=sql-server-ver15)izlemelidir.

## <a name="planning-for-version-change"></a>Sürüm değişikliğini planlama

Sürüm değişikliğini yapmadan önce aşağıdaki öğeleri incelemenizi öneririz:

1. Yükseltmeyi planladığınız sürümdeki yenilikleri inceleyin:

   - [SQL 2019](https://docs.microsoft.com/sql/sql-server/what-s-new-in-sql-server-ver15?view=sql-server-ver15) ' deki yenilikler
   - [SQL 2017](https://docs.microsoft.com/sql/sql-server/what-s-new-in-sql-server-2017?view=sql-server-ver15) ' deki yenilikler
   - [SQL 2016](https://docs.microsoft.com/sql/sql-server/what-s-new-in-sql-server-2016?view=sql-server-ver15) ' deki yenilikler
   - [SQL 2014](https://docs.microsoft.com/sql/sql-server/what-s-new-in-sql-server-2016?view=sql-server-2014) ' deki yenilikler

1. Yükseltmenin etkisini en aza indirmek için veritabanı uyumluluk modlarını kullanabilmeniz için, değiştirilecek sürümün [Uyumluluk sertifikasını](https://docs.microsoft.com/sql/database-engine/install-windows/compatibility-certification?view=sql-server-ver15) kontrol etmenizi öneririz.
1. Başarılı bir sonuç sağlamaya yardımcı olması için aşağıdaki makalelere göz atın:

   - [Video: modernize SQL Server | Pam Lahoud & Pedro Loümler | 20 yıllık GEÇIŞ](https://www.youtube.com/watch?v=5RPkuQHcxxs&feature=youtu.be)
   - [AB testi için Veritabanı Yükseltme Deneyimi Yardımcısı](https://docs.microsoft.com/sql/dea/database-experimentation-assistant-overview?view=sql-server-ver15)
   - [Sorgu Ayarlama Yardımcısı 'Nı kullanarak veritabanlarını yükseltme](https://docs.microsoft.com/sql/relational-databases/performance/upgrade-dbcompat-using-qta?view=sql-server-ver15)
   - [Veritabanı uyumluluk düzeyini değiştirme ve sorgu deposunu kullanma](https://docs.microsoft.com/sql/database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store?view=sql-server-ver15)

## <a name="upgrade-sql-version"></a>SQL sürümünü yükselt

> [!WARNING]
> SQL Server sürümünü yükseltmek, Analysis Services ve R hizmetleri gibi ilişkili hizmetlere ek olarak hizmeti SQL Server yeniden başlatacak.

SQL Server sürümünü yükseltmek için, SQL Server [yükseltme yolunu destekleyecek](https://docs.microsoft.com/sql/database-engine/install-windows/supported-version-and-edition-upgrades-version-15?view=sql-server-ver15) sonraki sürüm için SQL Server Kurulum medyasını edinin ve aşağıdaki adımları uygulayın:

1. İşlemi başlamadan önce sistem (tempdb hariç) ve kullanıcı veritabanları dahil veritabanlarını yedekleyin. Ayrıca, Azure Backup Hizmetleri 'ni kullanarak uygulamayla tutarlı bir VM düzeyi yedekleme de oluşturabilirsiniz.
1. SQL Server yükleme medyasından Setup.exe başlatın.
1. Yükleme Sihirbazı SQL Server yükleme merkezini başlatır. Mevcut bir SQL Server örneğini yükseltmek için, gezinti bölmesinde **yükleme** ' yi seçin ve ardından **SQL Server önceki bir sürümünden Yükselt**' i seçin.

   :::image type="content" source="./media/change-sql-server-version/upgrade.png" alt-text="SQL Server sürümünü yükseltmek için seçim":::

1. **Ürün anahtarı** sayfasında, SQL Server ücretsiz sürümüne yükseltme yapıp kullanmayacağınızı veya ürünün üretim sürümü IÇIN bir PID anahtarınız olduğunu göstermek için bir seçenek belirleyin. Daha fazla bilgi için, bkz. [SQL Server 2019 (15. x)](https://docs.microsoft.com/sql/sql-server/editions-and-components-of-sql-server-version-15?view=sql-server-ver15) ve [desteklenen sürüm ve sürüm yükseltmeleri (SQL Server 2016)](https://docs.microsoft.com/sql/database-engine/install-windows/supported-version-and-edition-upgrades?view=sql-server-ver15).
1. **Yükseltmeye hazırlanma** sayfasına ulaşana kadar **İleri** ' yi seçin ve ardından **Yükselt**' i seçin. Değişiklik etkinleşirken kurulum penceresi birkaç dakika yanıt vermeyi durdurabilir. **Komple** bir sayfa, yükseltmenin tamamlandığını doğrulayacaktır. Yükseltme için adım adım bir yordam için, [Tüm yordama](https://docs.microsoft.com/sql/database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup?view=sql-server-ver15#procedure)bakın.

   :::image type="content" source="./media/change-sql-server-version/complete-page.png" alt-text="Tüm sayfa":::

Sürümü değiştirmeye ek olarak SQL Server sürümünü değiştirdiyseniz, SQL sanal makine örneğini değiştirmek için sürümü de güncelleştirin ve **portalda sürümü ve sürümü doğrula** bölümüne başvurun.

   :::image type="content" source="./media/change-sql-server-version/change-portal.png" alt-text="Sürüm meta verilerini Değiştir":::

## <a name="downgrade-the-version-of-sql-server"></a>SQL Server sürümünün sürümü düşürme

SQL Server sürümünün indirgenmesini sağlamak için, SQL Server tamamen kaldırmanız ve istenen sürümü kullanarak yeniden yüklemeniz gerekir. Daha önceki bir sürümü daha yeni yüklenmiş önceki sürüme geri yükleyemeyeceksiniz, bu, yeni bir SQL Server yüklemesine benzerdir. Veritabanlarının sıfırdan yeniden oluşturulması gerekecektir. Yükseltme sırasında SQL Server sürümünü de değiştirdiyseniz, Azure portal SQL Server VM **Sürüm** özelliğini yeni sürüm değerine değiştirin. Bu, bu VM ile ilişkili meta verileri ve faturalandırmayı güncelleştirir.

> [!WARNING]
> SQL Server yerinde düşürme işlemi desteklenmiyor.

Aşağıdaki adımları izleyerek SQL Server sürümünü indirgeyebilmeniz gerekir:

1. [Yalnızca sonraki sürümde kullanılabilen](https://social.technet.microsoft.com/wiki/contents/articles/24222.find-enterprise-only-features-in-your-database.aspx)herhangi bir özelliği kullanmadığınız emin olun.
1. Sistem (tempdb hariç) ve kullanıcı veritabanları dahil tüm veritabanlarını yedekleyin.
1. Tüm gerekli sunucu düzeyi nesneleri (sunucu Tetikleyicileri, roller, oturum açmalar, bağlı sunucular, işler, kimlik bilgileri ve sertifikalar gibi) dışarı aktarın.
1. Daha önceki sürümde Kullanıcı veritabanlarınızı yeniden oluşturmak için betikleriniz yoksa, tüm nesneleri betiğe ve tüm verileri BCP.exe, SSIS veya DACPAC kullanarak dışarı aktarmanız gerekir.

   Hedef sürüm, bağımlı nesneler ve gelişmiş seçenekler olarak bu öğeler için komut dosyası oluştururken doğru seçenekleri seçtiğinizden emin olun.

   :::image type="content" source="./media/change-sql-server-version/scripting-options.png" alt-text="Betik seçenekleri":::

1. SQL Server ve tüm ilişkili hizmetleri tamamen kaldırın.
1. VM’yi yeniden başlatın.
1. SQL Server, programın istenen sürümü için medyayı kullanarak yükler.
1. En son hizmet paketlerini ve toplu güncelleştirmeleri yükler.
1. Tüm gerekli sunucu düzeyi nesneleri (adım 3 ' te dışarı aktarılan) içeri aktarın.
1. Tüm gerekli Kullanıcı veritabanlarını sıfırdan yeniden oluşturun (oluşturulan betikleri veya 4. adımdaki dosyaları kullanarak).

## <a name="verify-the-version-and-edition-in-the-portal"></a>Portalda sürümü ve sürümü doğrulama

SQL Server sürümünü değiştirdikten sonra, SQL Server sürümünü görüntülemek için Azure portal kullanabilmeniz için SQL Server VM [SQL VM kaynak sağlayıcısı](sql-vm-resource-provider-register.md) 'na yeniden kaydedin. Listelenen sürüm numarası artık SQL Server yüklemenizin yeni yükseltilen sürümünü yansıtmalıdır.

:::image type="content" source="./media/change-sql-server-version/verify-portal.png" alt-text="Sürümü doğrula":::

> [!NOTE]
> SQL VM kaynak sağlayıcısına zaten kaydolduysanız, [RP 'den kaydını kaldırın](sql-vm-resource-provider-register.md#unregister-from-rp) ve ardından [SQL VM kaynağını yeniden kaydedin](sql-vm-resource-provider-register.md#register-with-rp) . böylece, VM 'de yüklü olan SQL Server doğru sürümü ve sürümünü tespit edin. Bu, bu VM ile ilişkili meta verileri ve faturalandırma bilgilerini güncelleştirir.

## <a name="remarks"></a>Açıklamalar

- Yükseltme tamamlandıktan sonra yedeklemeleri/güncelleştirme istatistiklerini/dizinleri yeniden oluşturmayı/tutarlılığı kontrol etmenizi öneririz. Ayrıca, istediğiniz düzeyi yansıtdıklarından emin olmak için bireysel veritabanı uyumluluk düzeylerini de denetleyebilirsiniz.
- VM 'de SQL Server güncelleştirildikten sonra, Azure portal SQL Server **Sürüm** özelliğinin faturalandırma için yüklü sürüm numarasıyla eşleştiğinden emin olun.
- [Sürümü değiştirme](change-sql-server-edition.md#change-edition-in-portal) ÖZELLIĞI, SQL VM kaynak sağlayıcısı 'nın bir özelliğidir. Azure Marketi görüntüsünü Azure portal aracılığıyla dağıtmak, SQL Server VM otomatik olarak kaynak sağlayıcısıyla kaydeder. Ancak, kendi kendini yükleyen SQL Server müşterilerin SQL Server VM el ile [kaydetmesi](sql-vm-resource-provider-register.md)gerekir.
- SQL Server VM kaynağınızı düşürülebiliyorsanız, görüntünün sabit kodlanmış sürüm ayarı geri yüklenir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

- [Windows VM 'de SQL Server genel bakış](sql-server-on-azure-vm-iaas-what-is-overview.md)
- [Windows VM 'de SQL Server hakkında SSS](frequently-asked-questions-faq.md)
- [Windows VM üzerinde SQL Server için fiyatlandırma Kılavuzu](pricing-guidance.md)
- [Windows VM 'de SQL Server için sürüm notları](doc-changes-updates-release-notes.md)
