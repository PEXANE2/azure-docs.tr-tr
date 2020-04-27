---
title: Yerel makinenizde Azure Data Lake U-SQL betikleri çalıştırın
description: Visual Studio için Azure Data Lake Araçları kullanarak yerel makinenizde U-SQL işleri çalıştırma hakkında bilgi edinin.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 07/03/2018
ms.openlocfilehash: 42e58125fcbc3ab411c0d7503c42c14c28178428
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "62113944"
---
# <a name="run-u-sql-scripts-on-your-local-machine"></a>Yerel makinenizde U-SQL betiklerini çalıştırma

U-SQL betikleri geliştirirken, betikleri yerel olarak çalıştırarak zaman ve gider tasarrufu yapabilirsiniz. Visual Studio için Azure Data Lake Araçları, yerel makinenizde U-SQL betikleri çalıştırmayı destekler. 

## <a name="basic-concepts-for-local-runs"></a>Yerel çalıştırmalar için temel kavramlar

Aşağıdaki grafikte yerel çalıştırma ve bu bileşenlerin bulut çalıştırmasına nasıl eşlenme bileşenleri gösterilmektedir.

|Bileşen|Yerel çalıştırma|Bulut çalıştırması|
|---------|---------|---------|
|Depolama|Yerel veri kök klasörü|Varsayılan Azure Data Lake Store hesabı|
|İşlem|U-SQL yerel çalıştırma altyapısı|Azure Data Lake Analytics hizmeti|
|Ortamı Çalıştır|Yerel makinede çalışma dizini|Azure Data Lake Analytics kümesi|

Aşağıdaki bölümler yerel çalıştırma bileşenleri hakkında daha fazla bilgi sağlar.

### <a name="local-data-root-folders"></a>Yerel veri kök klasörleri

Yerel bir veri kök klasörü, yerel işlem hesabı için **yerel bir depodır** . Yerel makinenizde yerel dosya sistemindeki herhangi bir klasör yerel bir veri kök klasörü olabilir. Bu, Data Lake Analytics hesabının varsayılan Azure Data Lake Store hesabıyla aynıdır. Farklı bir veri kök klasörüne geçiş yapmak, farklı bir varsayılan depolama hesabına geçiş gibidir. 

Veri kök klasörü aşağıdaki gibi kullanılır:
- Meta verileri depolayın. Örnekler veritabanları, tablolar, tablo değerli işlevler ve derlemelerdir.
- U-SQL betiklerine göreli yollar olarak tanımlanan giriş ve çıkış yollarını bulun. Göreli yollar kullanarak, U-SQL betiklerini Azure 'a dağıtmak daha kolay olur.

### <a name="u-sql-local-run-engines"></a>U-SQL yerel çalıştırma motorları

U-SQL yerel çalıştırma altyapısı, U-SQL işleri için **yerel bir işlem hesabıdır** . Kullanıcılar, Visual Studio için Azure Data Lake Araçları aracılığıyla yerel olarak U-SQL işleri çalıştırabilir. Yerel çalıştırmalar Ayrıca Azure Data Lake U-SQL SDK komut satırı ve programlama arabirimleri aracılığıyla desteklenir. [U-SQL SDK Azure Data Lake](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/)hakkında daha fazla bilgi edinin.

### <a name="working-directories"></a>Çalışma dizinleri

Bir U-SQL betiği çalıştırdığınızda, derleme sonuçlarını önbelleğe almak, günlükleri çalıştırmak ve diğer işlevleri gerçekleştirmek için çalışma dizini klasörü gerekir. Visual Studio için Azure Data Lake Araçları, çalışma dizini U-SQL projesinin çalışma dizinidir. Bu, altında `<U-SQL project root path>/bin/debug>`bulunur. Çalışma dizini her yeni bir çalışma tetiklendiğinde temizlenir.

## <a name="local-runs-in-microsoft-visual-studio"></a>Microsoft Visual Studio yerel çalıştırmalar

Visual Studio için Azure Data Lake Araçları yerleşik bir yerel çalıştırma altyapısına sahiptir. Araçlar, altyapıyı yerel bir işlem hesabı olarak yüzey. Bir U-SQL betiğini yerel olarak çalıştırmak için betiğin Düzenleyici kenar boşluğu açılan menüsünde **yerel-makine** veya **Yerel-proje** hesabını seçin. Sonra **Gönder**' i seçin.

![Yerel hesaba U-SQL betiği gönderme](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-submit-script-to-local-account.png) 
 
## <a name="local-runs-with-a-local-machine-account"></a>Yerel makine hesabıyla yerel çalıştırmalar

**Yerel makine** hesabı, yerel depolama hesabı olarak tek bir yerel veri kök klasörü olan paylaşılan bir yerel işlem hesabıdır. Varsayılan olarak, veri kök klasörü **C:\Users\<Kullanıcı adı> \AppData\Local\USQLDataRoot**konumunda bulunur. Ayrıca **Araçlar** > **Data Lake** > **Seçenekler ve ayarlar**aracılığıyla da yapılandırılabilir.

![Yerel veri kök klasörü yapılandırma](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-local-data-root.png)
  
Yerel çalıştırma için bir U-SQL projesi gereklidir. U-SQL projesinin çalışma dizini U-SQL yerel çalıştırma çalışma dizini için kullanılır. Derleme sonuçları, çalıştırma günlükleri ve diğer iş çalıştırmasıyla ilgili dosyalar yerel çalıştırma sırasında çalışma dizini klasörü altında oluşturulur ve depolanır. Betiği her yeniden çalıştırdığınızda, çalışma dizinindeki tüm dosyalar temizlenir ve yeniden oluşturulur.

## <a name="local-runs-with-a-local-project-account"></a>Yerel-proje hesabıyla yerel çalıştırmalar

**Yerel-proje** hesabı, yalıtılmış bir yerel veri kök klasörü olan her bir proje için, proje yalıtılmış bir yerel işlem hesabıdır. Visual Studio 'da Çözüm Gezgini açılan her bir etkin U-SQL projesinin karşılık gelen `(Local-project: <project name>)` bir hesabı vardır. Hesaplar hem Visual Studio 'da hem de U-SQL betik Düzenleyicisi kenar boşluğunda Sunucu Gezgini listelenir.  

**Yerel-proje** hesabı, temiz ve yalıtılmış bir geliştirme ortamı sağlar. Bir **yerel makine** hesabında, tüm yerel işler için meta verileri ve giriş ve çıkış verilerini depolayan bir paylaşılan yerel veri kök klasörü vardır. Ancak bir **Yerel-proje** hesabı, bir u-SQL betiği çalıştırıldığında bir u-SQL projesi çalışma dizini altında geçici bir yerel veri kök klasörü oluşturur. Yeniden derleme veya yeniden çalıştırma gerçekleştiğinde bu geçici veri kök klasörü temizlenir. 

Bir U-SQL projesi, yalıtılmış yerel çalıştırma ortamını bir proje başvurusu ve özelliği aracılığıyla yönetir. U-SQL betikleri için giriş veri kaynaklarını hem projede hem de başvurulan veritabanı ortamlarında yapılandırabilirsiniz.

### <a name="manage-the-input-data-source-for-a-local-project-account"></a>Yerel proje hesabı için giriş veri kaynağını yönetme 

Bir U-SQL projesi yerel bir veri kök klasörü oluşturur ve **Yerel-proje** hesabına yönelik verileri ayarlar. Geçici bir veri kök klasörü, yeniden derleme ve yerel çalıştırma gerçekleştiğinde U-SQL projesi çalışma dizini altında temizlenir ve yeniden oluşturulur. U-SQL projesi tarafından yapılandırılan tüm veri kaynakları, yerel iş çalışmadan önce bu geçici yerel veri kök klasörüne kopyalanır. 

Veri kaynaklarınızın kök klasörünü yapılandırabilirsiniz. **U-SQL proje** > **özelliği** > **test veri kaynağı**' na sağ tıklayın. **Yerel proje** hesabında bir U-SQL betiği çalıştırdığınızda, **test veri kaynağı** klasöründeki tüm dosyalar ve alt klasörler geçici yerel veri kök klasörüne kopyalanır. Alt klasörlerdeki dosyalar dahil edilmiştir. Yerel bir iş çalıştıktan sonra, çıkış sonuçları, proje çalışma dizinindeki geçici yerel veri kök klasörü altında da bulunabilir. Projenin yeniden oluşturulması ve temizlenmesi durumunda tüm bu çıktılar silinir ve temizlenir. 

![Projenin test veri kaynağını yapılandırma](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-project-test-data-source.png)

### <a name="manage-a-referenced-database-environment-for-a-local-project-account"></a>**Yerel proje** hesabı için başvurulan bir veritabanı ortamını yönetme 

U-SQL sorgusu veya U-SQL veritabanı nesneleri ile sorgu kullanıyorsa, U-SQL betiğini yerel olarak çalıştırmadan önce veritabanı ortamlarını yerel olarak hazırlamak zorundasınız. **Yerel-proje** hesabı için u-SQL veritabanı bağımlılıkları u-SQL proje başvuruları tarafından yönetilebilir. U-SQL projenize U-SQL veritabanı proje başvuruları ekleyebilirsiniz. **Yerel proje** hesabında U-SQL betiklerini çalıştırmadan önce, başvurulan tüm veritabanları geçici yerel veri kök klasörüne dağıtılır. Her çalıştırma için, geçici veri kök klasörü yeni bir yalıtılmış ortam olarak temizlenir.

Bu ilgili makaleye bakın:
* U-SQL veritabanı tanımlarını ve ' [u-SQL veritabanı projelerinde](data-lake-analytics-data-lake-tools-develop-usql-database.md)başvuruları yönetme hakkında bilgi edinin.

## <a name="the-difference-between-local-machine-and-local-project-accounts"></a>**Yerel makine** ve **Yerel-proje** hesapları arasındaki fark

Bir **yerel makine** hesabı, kullanıcıların yerel makinelerinde bir Azure Data Lake Analytics hesabının benzetimini yapar. Aynı deneyimi bir Azure Data Lake Analytics hesabıyla paylaşır. **Yerel-proje** hesabı, Kullanıcı dostu bir yerel geliştirme ortamı sağlar. Bu ortam, kullanıcıların betikleri yerel olarak çalıştırmadan önce veritabanı başvurularını ve giriş verilerini dağıtmasına yardımcı olur. Bir **yerel makine** hesabı, tüm projeler aracılığıyla erişilebilen paylaşılan kalıcı bir ortam sağlar. **Yerel-proje** hesabı her proje için yalıtılmış bir geliştirme ortamı sağlar. Her çalıştırma için yenilenir. **Yerel-proje** hesabı, yeni değişiklikleri hızlıca uygulayarak daha hızlı bir geliştirme deneyimi sağlar.

**Yerel makine** ve **Yerel-proje** hesapları arasında daha fazla farklılık aşağıdaki tabloda gösterilmiştir:

|Fark açısı|Yerel makine|Yerel-proje|
|----------------|---------------|---------------|
|Yerel erişim|Tüm projeler tarafından erişilebilir.|Yalnızca ilgili proje bu hesaba erişebilir.|
|Yerel veri kök klasörü|Kalıcı bir yerel klasör. **Araçlar** > **Data Lake**Data Lake > **Seçenekler ve ayarlar**aracılığıyla yapılandırılır.|U-SQL projesi çalışma dizini altında her yerel çalıştırma için oluşturulan geçici bir klasör. Yeniden oluşturma veya yeniden çalıştırma gerçekleştiğinde klasör temizlenir.|
|U-SQL betiği için giriş verileri|Kalıcı yerel veri kök klasörü altındaki göreli yol.|**U-SQL proje özelliği** > **test veri kaynağı**aracılığıyla ayarlayın. Tüm dosyalar ve alt klasörler, yerel çalıştırmadan önce geçici veri kök klasörüne kopyalanır.|
|U-SQL betiği için çıkış verileri|Kalıcı yerel veri kök klasörü altındaki göreli yol.|Geçici veri kök klasörüne çıktı. Yeniden oluşturma veya yeniden çalıştırma gerçekleştiğinde sonuçlar temizlenir.|
|Başvurulan veritabanı dağıtımı|Başvurulan veritabanları, **yerel makine** hesabında çalışırken otomatik olarak dağıtılmaz. Bu, bir Azure Data Lake Analytics hesabına gönderim için de aynıdır.|Başvurulan veritabanları yerel bir çalıştırmadan önce **Yerel-proje** hesabına otomatik olarak dağıtılır. Yeniden derleme veya yeniden çalıştırma gerçekleştiğinde tüm veritabanı ortamları temizlenir ve yeniden dağıtılır.|

## <a name="a-local-run-with-the-u-sql-sdk"></a>U-SQL SDK ile yerel çalıştırma

U-SQL betiklerini, Visual Studio 'da yerel olarak çalıştırabilir ve ayrıca, komut satırı ve programlama arabirimleriyle U-SQL betiklerini yerel olarak çalıştırmak için Azure Data Lake U-SQL SDK 'sını de kullanabilirsiniz. Bu arabirimler sayesinde U-SQL yerel çalıştırmalarını ve testlerini otomatikleştirebilirsiniz.

[U-SQL SDK Azure Data Lake](data-lake-analytics-u-sql-sdk.md)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Lake Analytics için BIR CI/CD işlem hattı ayarlama](data-lake-analytics-cicd-overview.md).
- [Azure Data Lake Analytics kodunuzu test etme](data-lake-analytics-cicd-test.md).
