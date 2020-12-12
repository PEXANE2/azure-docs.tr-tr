---
title: 'Hızlı başlangıç: adanmış SQL havuzuyla verileri toplu yükleme'
description: Verileri Azure SYNAPSE Analytics 'te adanmış SQL havuzuna toplu olarak yüklemek için SYNAPSE Studio 'Yu kullanın.
services: synapse-analytics
author: kevinvngo
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: quickstart
ms.date: 12/11/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.openlocfilehash: 86ef610af605c657868824eefe2e6e706f6963ac
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2020
ms.locfileid: "97360195"
---
# <a name="quickstart-bulk-loading-with-synapse-sql"></a>Hızlı başlangıç: SYNAPSE SQL ile toplu yükleme

SYNAPSE Studio 'daki toplu yükleme Sihirbazı ile veri yükleme kolaydır. Toplu Yükleme Sihirbazı, [Copy ifadesiyle](/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest&preserve-view=true) bir T-SQL betiği oluştururken toplu yükleme verilerine kılavuzluk eder. 

## <a name="entry-points-to-the-bulk-load-wizard"></a>Toplu yükleme sihirbazına giriş noktaları

Özel SQL havuzlarını kullanarak verileri kolayca toplu olarak yükleyebilirsiniz ve SYNAPSE Studio içindeki aşağıdaki alanlara basit bir sağ tıklayın.

- Bir ![ depolama hesabındaki bir dosya veya klasöre sağ tıklayarak çalışma alanınıza bağlı bir Azure depolama hesabındaki dosya veya klasör](./sql/media/bulk-load/bulk-load-entry-point-0.png)

## <a name="prerequisites"></a>Önkoşullar

- Bu sihirbaz, kimlik doğrulaması için Azure AD geçişli geçiş kullanan bir COPY ifadesini oluşturur. Azure AD kullanıcılarınız, ADLS 2. hesabına en azından Depolama Blobu veri katılımcısı Azure rolüyle çalışma alanına [erişebilmelidir](
./sql-data-warehouse/quickstart-bulk-load-copy-tsql-examples.md#d-azure-active-directory-authentication) . 

- ' Ye yüklenecek yeni bir tablo oluşturuyorsanız [Copy ifadesini kullanmak](/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest&preserve-view=true#permissions) ve tablo oluşturmak için gerekli izinlere sahip olmanız gerekir.

- ADLS 2. hesabıyla ilişkili bağlı hizmetin yüklenecek dosya klasörüne **erişimi olmalıdır** /  . Örneğin, bağlantılı hizmet kimlik doğrulama mekanizması yönetilen kimlik ise, çalışma alanı yönetilen kimliği, depolama hesabında en az Depolama Blobu okuyucusu iznine sahip olmalıdır.

- Çalışma alanınızda VNet etkinse, kaynak veriler ve hata dosyası konumu için ADLS 2. hesabıyla ilişkili tümleşik çalışma zamanının etkileşimli yazma özelliğinin etkinleştirildiğinden emin olun. Etkileşimli yazma, yeniden şema algılama, kaynak dosya içeriklerinin önizlemesi ve sihirbazın içindeki ADLS 2. depolama hesaplarına göz atmak için gereklidir.

### <a name="steps"></a>Adımlar

1. Kaynak depolama konumu panelinden depolama hesabını ve yükleme yaptığınız dosyayı veya klasörü seçin. Sihirbaz, kaynak alanları dosyadan uygun hedef SQL veri türlerine eşlemek de dahil olmak üzere, otomatik olarak Parquet dosyalarını ve sınırlandırılmış metin (CSV) dosyalarını algılamaya çalışacaktır. 

   ![Kaynak konumu seçme](./sql/media/bulk-load/bulk-load-source-location.png)

2. Toplu yükleme işlemi sırasında reddedilen satırlar olduğunda hata ayarlarınız dahil olmak üzere dosya biçimi ayarlarını seçin. Ayrıca, COPY ifadesinin dosya biçimi ayarlarını yapılandırmanıza yardımcı olmak üzere dosyayı nasıl ayrıştırarak olduğunu görmek için "Verileri Önizle" seçeneğini de belirleyebilirsiniz. KOPYA ifadesinin dosyayı güncelleştirilmiş ayarla nasıl ayrıştıracağını görmek için, bir dosya biçimi ayarını her değiştirdiğinizde "Verileri Önizle" seçeneğini belirleyin:

   ![Verileri önizleme](./sql/media/bulk-load/bulk-load-file-format-settings-preview-data.png) 

> [!NOTE]  
>
> - Çoklu karakter alanı sonlandırıcılarını içeren verilerin önizlemesi toplu yükleme sihirbazında desteklenmez. Çoklu karakter alanı Sonlandırıcı belirtildiğinde toplu yükleme Sihirbazı, tek bir sütundaki verilerin önizlemesini alacak. 
> - "Sütun adlarını çıkar" seçeneği belirlendiğinde toplu yükleme Sihirbazı, "Ilk satır" alanı tarafından belirtilen ilk satırdaki sütun adlarını ayrıştırır. Toplu Yükleme Sihirbazı, bu üst bilgi satırını yoksaymak için COPY deyimindeki FIRSTROW değerini otomatik olarak 1 artırır. 
> - Çoklu karakter satır sonlandırıcılarını belirtmek COPY ifadesinde desteklenir; Ancak, bu, bir hatanın oluşturulduğu toplu yükleme sihirbazında desteklenmez.

3. Yükün mevcut bir tablo veya yeni tablo için olup olmayacağını dahil etmek için kullanmakta olduğunuz adanmış SQL havuzunu seçin: ![ hedef konum seçme](./sql/media/bulk-load/bulk-load-target-location.png)
4. Uygun sütun eşlemesine sahip olduğunuzdan emin olmak için "Sütun eşlemeyi Yapılandır" ı seçin. "Sütun adlarını çıkar" etkinse, sütun adları otomatik olarak algılanır. Yeni tablolar için, sütun eşlemesini yapılandırmak, hedef sütun veri türlerini güncelleştirmek için önemlidir:

   ![Sütun eşlemeyi yapılandırma](./sql/media/bulk-load/bulk-load-target-location-column-mapping.png)
5. "Betiği aç" ı seçin ve Data Lake 'ınızdan yüklenecek olan COPY ifadesiyle bir T-SQL betiği oluşturulacaktır: ![ SQL betiğini açma](./sql/media/bulk-load/bulk-load-target-final-script.png)

## <a name="next-steps"></a>Sonraki adımlar

- KOPYALAMA özellikleri hakkında daha fazla bilgi için [kopyalama ekstresi](/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest&preserve-view=true#syntax) makalesine bakın
- [Veri yüklemeye genel bakış](./sql-data-warehouse/design-elt-data-loading.md#what-is-elt) makalesini denetleyin
