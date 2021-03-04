---
title: Mevcut bir Azure geçişi projesinde SQL Server örnekleri bulma
description: Mevcut bir Azure geçişi projesinde SQL Server örnekleri bulmayı öğrenin.
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: how-to
ms.date: 11/23/2020
ms.openlocfilehash: 3dc9b178b9aa22991230f4cc6a9d54b44cf09b4e
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102098817"
---
# <a name="discover-sql-server-instances-in-an-existing-project"></a>Var olan bir projede SQL Server örnekleri bulma 

Bu makalede, Azure SQL değerlendirme özelliğinin önizlemesinden önce oluşturulan bir [Azure geçiş](./migrate-services-overview.md) projesindeki SQL Server örneklerinin ve veritabanlarının nasıl keşfedileceğini açıklanmaktadır.

Şirket içi makinelerde çalışan SQL Server örnekleri ve veritabanlarını keşfetmek, bir geçiş yolunu Azure SQL 'e tanımlamasına ve uyarlamanıza yardımcı olur. Azure geçişi gereci, bu bulmayı etki alanı kimlik bilgilerini veya SQL Server örneklerine ve hedeflenen sunucularda çalışan veritabanlarına erişimi olan SQL Server kimlik doğrulama kimlik bilgilerini kullanarak gerçekleştirir. Bu bulma işlemi aracısız değildir, ancak hedef sunuculara hiçbir şey yüklenmez.

> [!Note]
> VMware ortamınızda çalışan SQL Server örnekleri ve veritabanlarının keşfi ve değerlendirmesi artık önizlemededir. Bu özelliği denemek için **Avustralya Doğu** bölgede bir proje oluşturmak üzere [**Bu bağlantıyı**](https://aka.ms/AzureMigrate/SQL) kullanın. Zaten Avustralya Doğu bir projeniz varsa ve bu özelliği denemek istiyorsanız, lütfen bu makaledeki [**önkoşulları**](how-to-discover-sql-existing-project.md) tamamladığınızdan emin olun.

## <a name="before-you-start"></a>Başlamadan önce

- Şunları yapmayı unutmayın: 
    - Bölgeniz için SQL değerlendirme özelliği duyurusunun önüne bir [Azure geçişi projesi](./create-manage-projects.md) oluşturdunuz
    - [Azure geçişi: bulma ve değerlendirme](./how-to-assess.md) aracı bir projeye eklendi
- [Uygulama bulma desteğini ve gereksinimlerini](./migrate-support-matrix-vmware.md#vmware-requirements)gözden geçirin.
-  Uygulama bulma 'yı çalıştırdığınız sunucuların PowerShell sürüm 2,0 veya üzeri yüklü olduğundan ve VMware araçları (10.2.0 ' den sonraki sürümler) yüklü olduğundan emin olun.
- Azure geçişi gerecini dağıtmaya yönelik [gereksinimleri](./migrate-appliance.md) kontrol edin.
- Kaynak oluşturmak için abonelikte [gerekli rollere](./create-manage-projects.md#verify-permissions) sahip olduğunuzu doğrulayın.
- Gerecin internet 'e erişebildiğinden emin olun

## <a name="enable-discovery-of-sql-server-instances-and-databases"></a>SQL Server örnekleri ve veritabanlarının bulunmasını etkinleştir

1. Azure geçişi projenizde şunlardan birini yapın
    - Hub kutucuğunda **etkinleştirilmemiş** veya :::image type="content" source="./media/how-to-discover-sql-existing-project/hub-not-enabled.png" alt-text="SQL bulma etkin olmayan Azure geçişi hub kutucuğunu"::: seçin
    - SQL örnekleri sütunundaki sunucu keşfi sayfasında hiçbir girişte **etkin değil** ' i seçin :::image type="content" source="./media/how-to-discover-sql-existing-project/discovery-not-enabled.png" alt-text="SQL bulma etkin değil, bulunan sunucuları Azure geçişi bulunan sunucuları dikey"::: penceresi
2. Bulma SQL Server örnekleri ve veritabanları, aşağıdaki adımları izleyin:
    - Gerekli kaynağı oluşturmak için **Yükselt**' i seçin.
        :::image type="content" source="./media/how-to-discover-sql-existing-project/discovery-upgrade-appliance.png" alt-text="Azure geçişi gerecini yükseltmek için düğme":::
    - Gereç üzerinde çalışan hizmetlerin en son sürümlere güncelleştirildiğini doğrulayın. Bunu yapmak için gereç sunucusundan gereç Yapılandırma Yöneticisini başlatın ve önkoşulları ayarla panelinden gereç hizmetlerini görüntüle ' yi seçin.
        - Gereç ve bileşenleri otomatik olarak güncelleştirilerek :::image type="content" source="./media/how-to-discover-sql-existing-project/appliance-services-version.png" alt-text="gereç sürümünü denetleyin":::
    - Gereç Yapılandırma Yöneticisi 'nin kimlik bilgilerini ve keşif kaynaklarını yönet panelinde, SQL Server örneği ve keşfedilecek veritabanları üzerinde sysadmin erişimi olan etki alanı veya SQL Server kimlik doğrulaması bilgilerini ekleyin. 
    Gerecin otomatik kimlik bilgisi eşleme özelliğinden yararlanabilir veya [burada](https://review.docs.microsoft.com/azure/migrate/tutorial-discover-vmware?branch=release-migrate-sql-scenario#start-continuous-discovery)vurgulanan kimlik bilgilerini ilgili sunucuyla el ile eşleyebilirsiniz.
        
    Bazı noktalara işaret eder:
    - Lütfen yazılım envanterinin zaten etkinleştirildiğinden emin olun veya aynı etkinleştirmek için etki alanı ya da etki alanı dışı kimlik bilgileri sağlayın. SQL Server örnekleri bulması için yazılım envanteri gerçekleştirilmelidir.
    - Gereç, eklenen etki alanı kimlik bilgilerini AD ile doğrulamaya çalışır. Lütfen gereç sunucusunda kimlik bilgileriyle ilişkili AD sunucusuna yönelik ağ görüşbir hat olduğundan emin olun. SQL Server kimlik doğrulamasıyla ilişkili kimlik bilgileri doğrulanmadı. 

3. İstenen kimlik bilgileri eklendikten sonra taramaya başlamak için lütfen bulmayı Başlat ' ı seçin.

> [!Note] 
>Azure SQL için değerlendirmeler oluşturmadan önce lütfen SQL bulmanın bir süre için çalışmasına izin verin. SQL Server örnekleri ve veritabanlarının keşfi tamamlanana izin verilmiyorsa, ilgili örnekler değerlendirme raporunda **bilinmiyor** olarak işaretlenir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL değerlendirmesi](./how-to-create-azure-sql-assessment.md) oluşturmayı öğrenin
- [Azure SQL değerlendirmeleri](./concepts-azure-sql-assessment-calculation.md) hakkında daha fazla bilgi edinin