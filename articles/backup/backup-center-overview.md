---
title: Yedekleme merkezine genel bakış
description: Bu makalede, Azure için yedekleme merkezi 'ne genel bakış sunulmaktadır.
ms.topic: conceptual
ms.date: 09/30/2020
ms.openlocfilehash: 0acf3a17e41b89b462a7fb07c5d3ebd55d4052af
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101725382"
---
# <a name="overview-of-backup-center"></a>Yedekleme merkezine genel bakış

Yedekleme merkezi, kuruluşların ölçekli yedeklemeleri yönetmek, izlemek, çalıştırmak ve analiz etmek için Azure 'da **tek bir birleştirilmiş yönetim deneyimi** sağlar. Bu nedenle, Azure 'un yerel yönetim deneyimleri ile tutarlıdır.

Yedekleme merkezi 'nin önemli avantajlarından bazıları şunlardır:

* **Yedeklemeleri yönetmek Için tek bir cam bölmesi** : yedekleme merkezi, büyük ve dağıtılmış bir Azure ortamında iyi çalışmak üzere tasarlanmıştır. Birden çok iş yükü türünü, kasalarını, abonelikleri, bölgeleri ve [Azure Kathouse](../lighthouse/overview.md) kiracılarını kapsayan yedeklemeleri verimli bir şekilde yönetmek Için yedekleme merkezi 'ni kullanabilirsiniz.
* **Veri kaynağı merkezli yönetim** – yedekleme merkezi, yedeklediğiniz veri kaynaklarında ortalanan görünümler ve filtreler sağlar (örneğin, VM 'ler ve veritabanları). Bu, bir kaynak sahibinin veya bir yedekleme yöneticisinin bir öğenin hangi kasaya yedeklenmek zorunda kalmadan öğelerin yedeklerini izlemelerine ve çalıştırabilmesine olanak sağlar. Bu tasarımın temel bir özelliği, veri kaynağı aboneliği, veri kaynağı kaynak grubu ve veri kaynağı etiketleri gibi veri kaynağına özgü özelliklerle görünümleri filtreleyebilme yeteneğidir. Örneğin, kuruluşunuz farklı departmanlara ait VM 'lere farklı Etiketler atama alıştırması yaptıysanız, yedekleme bilgilerini, kasanın etiketine odaklanmak zorunda kalmadan yedeklenmekte olan temel VM 'lerin etiketlerine göre filtrelemek için yedekleme merkezi 'ni kullanabilirsiniz.
* **Bağlı deneyimler** – yedekleme merkezi, var olan Azure hizmetlerine, ölçekte yönetimi sağlayan yerel tümleştirmeler sağlar. Örneğin, yedekleme merkezi, yedeklemelerinizi yönetmenize yardımcı olması için [Azure ilke](../governance/policy/overview.md) deneyimini kullanır. Ayrıca, yedeklemelerdeki ayrıntılı raporları görüntülemenize yardımcı olması için [Azure çalışma kitapları](../azure-monitor/visualize/workbooks-overview.md) ve [Azure izleyici günlükleri](../azure-monitor/logs/data-platform-logs.md) de yararlanır. Bu nedenle, yedekleme merkezi 'nin sunduğu değişen özellikleri kullanmak için yeni ilkeler öğrenmeniz gerekmez. Ayrıca, yedekleme merkezi 'nden topluluk kaynaklarını bulabilirsiniz.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

* Yedekleme merkezi şu anda Azure VM yedeklemesi, Azure SAP HANA 'da SQL VM yedeklemesi, Azure VM yedeklemesi, Azure dosya yedeklemesi, Azure Blobları yedeklemesi, Azure yönetilen diskler yedeklemesi ve PostgreSQL için Azure veritabanı sunucu yedeklemesi için desteklenmektedir.
* Desteklenen ve desteklenmeyen senaryoların ayrıntılı bir listesi için [destek matrisine](backup-center-support-matrix.md) bakın.

## <a name="get-started"></a>başlarken

Yedekleme merkezi 'ni kullanmaya başlamak için Azure portal **yedekleme merkezi** araması yapın ve **yedekleme merkezi (Önizleme)** panosuna gidin.

![Yedekleme merkezi arama](./media/backup-center-overview/backup-center-search.png)

Gördüğünüz ilk ekran **genel bakıştır**. İki kutucuk vardır: **işler** ve **yedekleme örnekleri**.

![Yedekleme merkezi kutucukları](./media/backup-center-overview/backup-center-overview-widgets.png)

**İşler** kutucuğunda, son 24 saat içinde yedeklemeleriniz genelinde tetiklenen tüm yedekleme ve geri yükleme işlerinin bir Özet görünümünü alırsınız. Tamamlanan, başarısız ve sürmekte olan işlerin sayısı hakkında bilgi görüntüleyebilirsiniz. Bu kutucuktaki sayıların seçilmesi, belirli bir veri kaynağı türü, işlem türü ve durum için iş hakkında daha fazla bilgi görüntülemenizi sağlar.

**Yedekleme örnekleri** kutucuğunda, yedeklemeinizdeki tüm yedekleme örneklerinin özetlenmiş bir görünümünü alırsınız. Örneğin, hala koruma için yapılandırılmış örnek sayısıyla karşılaştırıldığında, geçici olarak silinen durumda olan yedekleme örneklerinin sayısını görebilirsiniz. Bu kutucuktaki sayıların seçilmesi, belirli bir veri kaynağı türü ve koruma durumu için yedekleme örnekleri hakkında daha fazla bilgi görüntülemenizi sağlar.

Yedekleme merkezi 'nin yeteneklerini anlamak için aşağıdaki videoyu izleyin:

> [!VIDEO https://www.youtube.com/embed/pFRMBSXZcUk?t=497]

Yedekleme merkezi 'nin sağladığı farklı özellikleri anlamak için [sonraki adımları](#next-steps) izleyin ve yedeklemenizi verimli bir şekilde yönetmek için bu özellikleri nasıl kullanabileceğinizi öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

* [Yedeklemeleri izleme ve çalıştırma](backup-center-monitor-operate.md)
* [Yedeklemenizi yönetin](backup-center-govern-environment.md)
* [Yedeklemeleriniz hakkında Öngörüler edinin](backup-center-obtain-insights.md)
* [Yedekleme merkezi 'ni kullanarak eylem gerçekleştirme](backup-center-actions.md)