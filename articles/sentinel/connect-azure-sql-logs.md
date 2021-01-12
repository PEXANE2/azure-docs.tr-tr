---
title: Azure SQL veritabanı tanılama ve denetim günlüklerini Azure Sentinel 'e bağlama
description: Azure SQL veritabanı tanılama günlüklerini ve güvenlik denetim günlüklerini Azure Sentinel 'e bağlamayı öğrenin.
author: yelevin
manager: rkarlin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: how-to
ms.date: 01/06/2021
ms.author: yelevin
ms.openlocfilehash: df132c35ebb04596d91720431f5b08cb88e2abd9
ms.sourcegitcommit: 3af12dc5b0b3833acb5d591d0d5a398c926919c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104210"
---
# <a name="connect-azure-sql-database-diagnostics-and-auditing-logs"></a>Azure SQL veritabanı tanılamayı ve denetim günlüklerini bağlama

Azure SQL, Kullanıcı katılımı olmadan yükseltme, düzeltme eki uygulama ve izleme gibi birçok veritabanı yönetim işlevini işleyen, tam olarak yönetilen bir hizmet olarak platform (PaaS) veritabanı altyapısıdır. 

Azure SQL Veritabanı Bağlayıcısı, veritabanlarının denetim ve tanılama günlüklerini Sentinel 'e aktarmanıza olanak tanıyarak tüm örneklerinizin etkinliğini sürekli olarak izlemenize imkan tanır.

- Tanılama günlüklerini bağlamak, farklı veri türlerindeki veritabanı tanılama günlüklerini Sentinel çalışma alanınıza göndermenizi sağlar.

- Denetim günlüklerini bağlamak, tüm Azure SQL veritabanlarınızdaki güvenlik denetim günlüklerini sunucu düzeyinde akışla oluşturmanıza olanak sağlar.

[Azure SQL veritabanlarını izleme](../azure-sql/database/metrics-diagnostic-telemetry-logging-streaming-export-configure.md)hakkında daha fazla bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

- Azure Sentinel çalışma alanında okuma ve yazma izinlerine sahip olmanız gerekir.

- Denetim günlüklerini bağlamak için Azure SQL Server denetim ayarları 'nda okuma ve yazma izinlerinizin olması gerekir.

## <a name="connect-to-azure-sql-database"></a>Azure SQL Veritabanı'na bağlanma
    
1. Azure Sentinel gezinti menüsünde **veri bağlayıcıları**' nı seçin.

1. Veri bağlayıcıları galerisinden **Azure SQL veritabanı** ' nı seçin ve ardından Önizleme bölmesinde **bağlayıcı sayfasını aç**  ' ı seçin.

1. Bağlayıcı sayfasının **yapılandırma** bölümünde, bağlantı kurmak için kullanabileceğiniz iki günlük kategorisini aklınızda bulabilirsiniz.

### <a name="connect-diagnostics-logs"></a>Tanılama günlüklerini bağlama

1. **Tanılama günlükleri** altında **Azure SQL veritabanlarınızı her birinde El Ile tanılama günlüklerini etkinleştir**' i genişletin.

1. **Azure SQL kaynakları dikey** penceresini açmak IÇIN **Azure SQL >aç** bağlantısını seçin.

1. **(Isteğe bağlı)** Veritabanı kaynağınızı kolayca bulmak için üstteki filtreler çubuğunda **Filtre Ekle** ' yi seçin.
    1. **Filtre** açılan listesinde **kaynak türü**' nü seçin.
    1. **Değer** aşağı açılan listesinde, **Tümünü Seç**' ın Işaretini kaldırın ve ardından **SQL veritabanı**' nı seçin.
    1. **Uygula**’ya tıklayın.
    
1. Tanılama günlüklerini Azure Sentinel 'e göndermek istediğiniz veritabanı kaynağını seçin.

    > [!NOTE]
    > Günlüklerini toplamak istediğiniz her veritabanı kaynağı için, bu adımdan itibaren bu işlemi yinelemeniz gerekir.

1. Seçtiğiniz veritabanının kaynak sayfasından, gezinti menüsünde **izleme** altında **Tanılama ayarları**' nı seçin.

    1. Tablonun alt kısmındaki **+ Tanılama ayarı Ekle** bağlantısını seçin.

    1. **Tanılama ayarı** ekranında, **Tanılama ayarı adı** alanına bir ad girin.
    
    1. **Hedef ayrıntıları** sütununda **Log Analytics gönder çalışma alanı** onay kutusunu işaretleyin. Burada iki yeni alan görüntülenir. İlgili **aboneliği** ve **Log Analytics çalışma alanını** (Azure Sentinel 'in bulunduğu yer) seçin.

    1. **Kategori ayrıntıları** sütununda, almak istediğiniz günlük ve ölçüm türlerinin onay kutularını işaretleyin. Hem **günlük** hem de **ölçüm** altındaki tüm kullanılabilir türleri seçmenizi öneririz.

    1. Ekranın üst kısmındaki **Kaydet** ' i seçin.

- Alternatif olarak, tanılama günlüklerinizi bağlamak için sağlanan **PowerShell betiğini** kullanabilirsiniz.
    1. **Tanılama günlükleri** altında **PowerShell betiği tarafından etkinleştir**' i genişletin.

    1. Kod bloğunu kopyalayın ve PowerShell 'e yapıştırın.

### <a name="connect-audit-logs"></a>Denetim günlüklerini bağlama

1. **Denetim günlükleri (Önizleme)** altında, **tüm Azure SQL veritabanlarında (sunucu düzeyinde) denetim günlüklerini etkinleştir**' i genişletin.

1. **SQL Server** kaynak dikey penceresini açmak IÇIN **Azure SQL >aç** bağlantısını seçin.

1. Azure Sentinel 'e göndermek istediğiniz denetim günlüklerini içeren SQL Server 'ı seçin.

    > [!NOTE]
    > Günlüklerini toplamak istediğiniz her sunucu kaynağı için, bu adımdan itibaren bu işlemi yinelemeniz gerekir.

1. Seçtiğiniz sunucunun kaynak sayfasından, gezinti menüsündeki **güvenlik** altında **Denetim**' i seçin.

    1. **Azure SQL denetimini etkinleştir ' ın** üzerine geçiş yap **'** a gidin.

    1. **Denetim günlüğü hedefi** altında **Log Analytics (Önizleme)** öğesini seçin.
    
    1. Görüntülenen çalışma alanları listesinden çalışma alanınızı (Azure Sentinel 'in bulunduğu yer) seçin.

    1. Ekranın üst kısmındaki **Kaydet** ' i seçin.

- Alternatif olarak, tanılama günlüklerinizi bağlamak için sağlanan **PowerShell betiğini** kullanabilirsiniz.
    1. **Denetim günlükleri** altında **PowerShell betiği tarafından etkinleştir**' i genişletin.

    1. Kod bloğunu kopyalayın ve PowerShell 'e yapıştırın.


> [!NOTE]
>
> Bu veri bağlayıcısıyla bağlantı durumu göstergeleri (veri bağlayıcıları galerisindeki bir renk şeridi ve veri türü adlarının yanında bulunan bağlantı simgeleri), yalnızca geçen iki haftada bir noktada verilerin alınmış olması halinde *bağlı* (yeşil) olarak görünür. Veri alımı olmadan iki hafta geçtikten sonra bağlayıcının bağlantısı kesilmekte olarak gösterilir. Daha fazla veri geldiğinde *bağlantılı* durum döndürülür.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Azure SQL veritabanı tanılama ve denetim günlüklerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.