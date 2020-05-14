---
title: SYNAPSE SQL ile toplu yükleme verileri
description: SYNAPSE SQL 'de verileri toplu olarak yüklemek için SYNAPSE Studio 'Yu kullanma
services: synapse-analytics
author: kevinvngo
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 05/06/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.openlocfilehash: 96e79fdfeed5b56a4e0a33229f419f439b20b04c
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83124537"
---
# <a name="bulk-loading-with-synapse-sql"></a>SYNAPSE SQL ile toplu yükleme

SYNAPSE Studio 'da toplu yükleme Sihirbazı kullanılırken veri yükleme hiçbir zaman daha kolay olmamıştı. Bu sihirbaz, toplu yükleme verilerine [kopyalama ifadesiyle](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) bir T-SQL betiği oluşturma sırasında size rehberlik edecektir. 

## <a name="entry-points-to-the-bulk-load-wizard"></a>Toplu yükleme sihirbazına giriş noktaları

Artık, SYNAPSE Studio içindeki aşağıdaki alanlara basit bir sağ tıklama ile SQL havuzları kullanarak verileri kolayca toplu olarak yükleyebilirsiniz:

- Bir ![ depolama hesabındaki bir dosya veya klasöre sağ tıklayarak çalışma alanınıza bağlı bir Azure depolama hesabındaki dosya veya klasör](./sql/media/bulk-load/bulk-load-entry-point-0.png)

## <a name="prerequisites"></a>Ön koşullar

- ADLS 2. hesaba en azından Depolama Blobu veri katılımcısı RBAC rolüyle çalışma alanına erişiminizin olması gerekir.

- Uygulamasına yüklenecek yeni bir tablo oluşturuyorsanız COPY ifadesini ve Create Table izinlerini [kullanmak için gerekli izinlere](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest#permissions) sahip olmanız gerekir.

- ADLS 2. hesabıyla ilişkili bağlı hizmetin yüklenecek dosya klasörüne **erişimi olmalıdır** / **folder** . Örneğin, bağlantılı hizmet kimlik doğrulama mekanizması yönetilen kimlik ise, çalışma alanı yönetilen kimliği, depolama hesabında en az Depolama Blobu okuyucusu iznine sahip olmalıdır.

- Çalışma alanınızda VNet etkinse, kaynak veriler ve hata dosyası konumu için ADLS 2. hesabıyla ilişkili tümleşik çalışma zamanının etkileşimli yazma özelliğinin etkinleştirildiğinden emin olun. Otomatik şema algılama, kaynak dosya içeriklerinin önizlemesi ve sihirbazdaki ADLS 2. depolama hesaplarına göz atmak için etkileşimli yazma gerekir.

### <a name="steps"></a>Adımlar

1. Kaynak depolama konumu panelinde depolama hesabını ve yükleme yaptığınız dosyayı veya klasörü seçin: ![ kaynak konumu seçme](./sql/media/bulk-load/bulk-load-source-location.png)

2. Reddedilen satırları (hata dosyası) yazmak istediğiniz depolama hesabı da dahil olmak üzere dosya biçimi ayarlarını seçin. Şu anda yalnızca CSV ve Parquet dosyaları desteklenir.

    ![Dosya biçimi ayarlarını seçme](./sql/media/bulk-load/bulk-load-file-format-settings.png)

3. COPY ifadesinin dosya biçimi ayarlarını yapılandırmanıza yardımcı olmak üzere dosyayı nasıl ayrıştırarak olduğunu görmek için "Verileri Önizle" seçeneğine tıklayabilirsiniz. KOPYA ifadesinin dosyayı güncelleştirilmiş ayarla nasıl ayrıştıracağını görmek için "Verileri Önizle" seçeneğine tıklayın: ![ verileri önizleme](./sql/media/bulk-load/bulk-load-file-format-settings-preview-data.png) 

4. Yükün mevcut bir tablo veya yeni tablo için olup olmayacağını dahil etmek için kullandığınız SQL havuzunu seçin: ![ hedef konum seçme](./sql/media/bulk-load/bulk-load-target-location.png)

5. Uygun sütun eşlemesine sahip olduğunuzdan emin olmak için "Sütun eşlemeyi Yapılandır" düğmesine tıklayın. Yeni tablolar için, sütun eşlemesini yapılandırmak, hedef sütun veri türlerini güncelleştirmek için önemlidir: ![ Sütun eşlemeyi yapılandırma](./sql/media/bulk-load/bulk-load-target-location-column-mapping.png)

6. "Betiği aç" seçeneğine tıkladığınızda bir T-SQL betiği, veri göümden yüklenecek olan COPY ifadesiyle birlikte oluşturulacaktır: ![ SQL betiğini açma](./sql/media/bulk-load/bulk-load-target-final-script.png)

## <a name="next-steps"></a>Sonraki adımlar

- KOPYALAMA özellikleri hakkında daha fazla bilgi için [kopyalama ekstresi](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest#syntax) makalesine bakın
- [Veri yüklemeye genel bakış](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/design-elt-data-loading#what-is-elt) makalesini denetleyin
