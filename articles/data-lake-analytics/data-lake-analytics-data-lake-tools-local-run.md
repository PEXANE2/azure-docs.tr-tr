---
title: Azure Veri Gölü U-SQL komut dosyalarını yerel makinenizde çalıştırma
description: Yerel makinenizde U-SQL işlerini çalıştırmak için Visual Studio için Azure Veri Göl Araçlarını nasıl kullanacağınızı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "62113944"
---
# <a name="run-u-sql-scripts-on-your-local-machine"></a>Yerel makinenizde U-SQL komut dosyaları çalıştırma

U-SQL komut dosyaları geliştirdiğinizde, komut dosyalarını yerel olarak çalıştırarak zamandan ve masrafdan tasarruf edebilirsiniz. Visual Studio için Azure Veri Gölü Araçları, yerel makinenizde U-SQL komut dosyaları çalıştırmayı destekler. 

## <a name="basic-concepts-for-local-runs"></a>Yerel çalıştırmalar için temel kavramlar

Aşağıdaki grafik, yerel çalıştırma bileşenlerini ve bu bileşenlerin bulutla nasıl eşleştiğini gÜ

|Bileşen|Yerel çalıştırma|Bulut çalıştırma|
|---------|---------|---------|
|Depolama|Yerel veri kökü klasörü|Varsayılan Azure Veri Gölü Deposu hesabı|
|İşlem|U-SQL yerel çalıştırma motoru|Azure Veri Gölü Analytics hizmeti|
|Çalışma ortamı|Yerel makinede çalışma dizini|Azure Veri Gölü Analizi kümesi|

İzleyen bölümler yerel çalışan bileşenleri hakkında daha fazla bilgi sağlar.

### <a name="local-data-root-folders"></a>Yerel veri kökü klasörleri

Yerel veri kökü klasörü, yerel bilgi işlem hesabı için yerel bir **depodur.** Yerel makinenizdeki yerel dosya sistemindeki herhangi bir klasör yerel veri kökü klasörü olabilir. Bu, bir Veri Gölü Analizi hesabının varsayılan Azure Veri Gölü Deposu hesabıyla aynıdır. Farklı bir veri kökü klasörüne geçmek, farklı bir varsayılan depo hesabına geçmek gibidir. 

Veri kökü klasörü aşağıdaki gibi kullanılır:
- Meta verileri depolayın. Örnekler veritabanları, tablolar, tablo değerli işlevler ve derlemeler vardır.
- U-SQL komut dosyalarında göreli yollar olarak tanımlanan giriş ve çıkış yollarına bakın. Göreli yolları kullanarak U-SQL komut dosyalarınızı Azure'a dağıtmak daha kolaydır.

### <a name="u-sql-local-run-engines"></a>U-SQL yerel çalışma motorları

U-SQL yerel çalıştırma altyapısı, U-SQL işleri için yerel bir **işlem hesabıdır.** Kullanıcılar U-SQL işlerini Visual Studio için Azure Veri Gölü Araçları aracılığıyla yerel olarak çalıştırabilir. Yerel çalıştırmalar, Azure Veri Gölü U-SQL SDK komut satırı ve programlama arabirimleri aracılığıyla da desteklenir. Azure Veri [Gölü U-SQL SDK](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/)hakkında daha fazla bilgi edinin.

### <a name="working-directories"></a>Çalışma dizinleri

Bir U-SQL komut dosyası çalıştırdığınızda, derleme sonuçlarını önbelleğe almak, günlükleri çalıştırmak ve diğer işlevleri gerçekleştirmek için çalışan bir dizin klasörü gerekir. Visual Studio için Azure Veri Gölü Araçları'nda çalışma dizini U-SQL projesinin çalışma dizinidir. Altında yer `<U-SQL project root path>/bin/debug>`alır. Çalışma dizini, her yeni çalıştırma tetikleninher olduğunda temizlenir.

## <a name="local-runs-in-microsoft-visual-studio"></a>Microsoft Visual Studio'da yerel çalıştırmalar

Visual Studio için Azure Veri Gölü Araçları'nın yerleşik yerel çalışma motoru vardır. Araçlar motoru yerel bir işlem hesabı olarak yüzeye çıkar. Bir U-SQL komut dosyasını yerel olarak çalıştırmak için, komut dosyasının düzenleyici kenar boşluğu açılır menüsünde **Yerel makine** veya **Yerel proje** hesabını seçin. Ardından **Gönder'i**seçin.

![Yerel bir hesaba U-SQL komut dosyası gönderme](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-submit-script-to-local-account.png) 
 
## <a name="local-runs-with-a-local-machine-account"></a>Yerel makine hesabıyla yerel çalıştırmalar

**Yerel makine** hesabı, yerel mağaza hesabı olarak tek bir yerel veri kökü klasörüne sahip paylaşılan yerel bir işlem hesabıdır. Varsayılan olarak, veri kökü klasörü **C:\Kullanıcılar\<kullanıcı adı>\AppData\Local\USQLDataRoot**adresinde bulunur. Ayrıca **Araçlar** > **Veri Gölü** > **Seçenekleri ve Ayarları**ile yapılandırılabilir.

![Yerel veri kökü klasörünü yapılandırma](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-local-data-root.png)
  
Yerel bir çalıştırma için bir U-SQL projesi gereklidir. U-SQL projesinin çalışma dizini, U-SQL yerel çalışma dizini için kullanılır. Derleme sonuçları, çalışma günlükleri ve diğer iş çalıştırma ile ilgili dosyalar oluşturulur ve yerel çalışma sırasında çalışma dizini klasörü altında depolanır. Komut dosyasını her yeniden çalıştırdığınızda, çalışma dizinindeki tüm dosyalar temizlenir ve yeniden oluşturulur.

## <a name="local-runs-with-a-local-project-account"></a>Yerel proje hesabıyla yerel çalıştırmalar

**Yerel proje** hesabı, yalıtılmış yerel veri kökü klasörü olan her proje için proje yalıtılmış yerel bir bilgi işlem hesabıdır. Visual Studio'da Solution Explorer'da açılan her etkin `(Local-project: <project name>)` U-SQL projesinin karşılık gelen bir hesabı vardır. Hesaplar Visual Studio'daki Sunucu Gezgini'nde ve U-SQL komut dosyası düzenleyicisi kenar boşluğunda listelenir.  

**Yerel proje** hesabı temiz ve yalıtılmış bir geliştirme ortamı sağlar. **Yerel makine** hesabı, tüm yerel işler için meta verileri ve giriş ve çıktı verilerini depolayan paylaşılan bir yerel veri kökü klasörüne sahiptir. Ancak **Yerel proje** hesabı, u-SQL komut dosyası her çalıştırılsa u-SQL proje çalışma dizininin altında geçici bir yerel veri kökü klasörü oluşturur. Bu geçici veri kökü klasörü yeniden oluşturma veya yeniden çalıştırma olduğunda temizlenir. 

U-SQL projesi, yalıtılmış yerel çalışma ortamını bir proje başvurusu ve özelliği aracılığıyla yönetir. Hem projede hem de başvurulan veritabanı ortamlarında U-SQL komut dosyalarının giriş veri kaynaklarını yapılandırabilirsiniz.

### <a name="manage-the-input-data-source-for-a-local-project-account"></a>Yerel proje hesabının giriş veri kaynağını yönetme 

U-SQL projesi yerel bir veri kökü klasörü oluşturur ve **Yerel proje** hesabı için veri ayarlar. Geçici bir veri kökü klasörü temizlenir ve u-SQL proje çalışma dizini altında yeniden yeniden oluşturma ve yerel çalışma her gerçekleştiğinde yeniden oluşturulur. U-SQL projesi tarafından yapılandırılan tüm veri kaynakları, yerel iş çalıştırmadan önce bu geçici yerel veri kökü klasörüne kopyalanır. 

Veri kaynaklarınızın kök klasörünü yapılandırabilirsiniz. Sağ tıklayın **U-SQL projesi** > **Özellik** > **Test Veri Kaynağı**. **Yerel proje** hesabında bir U-SQL komut dosyası çalıştırdığınızda, Test **Veri Kaynağı** klasöründeki tüm dosyalar ve alt klasörler geçici yerel veri kökü klasörüne kopyalanır. Alt klasörlerin altındaki dosyalar dahildir. Yerel bir iş çalıştırdıktan sonra, çıktı sonuçları proje çalışma dizinindeki geçici yerel veri kökü klasörü altında da bulunabilir. Proje yeniden oluşturulup temizlendiğinde tüm bu çıktı silinir ve temizlenir. 

![Projenin test veri kaynağını yapılandırma](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-project-test-data-source.png)

### <a name="manage-a-referenced-database-environment-for-a-local-project-account"></a>Yerel proje hesabı için başvurulan veritabanı **ortamını** yönetme 

Bir U-SQL sorgusu U-SQL veritabanı nesneleri kullanır veya sorgular, u-SQL komut dosyası yerel olarak çalıştırmadan önce veritabanı ortamları yerel olarak hazır hale getirmeniz gerekir. Yerel **proje** hesabı için U-SQL veritabanı bağımlılıkları U-SQL proje başvuruları tarafından yönetilebilir. U-SQL projenize U-SQL veritabanı proje referansları ekleyebilirsiniz. **Yerel proje** hesabında U-SQL komut dosyalarını çalıştırmadan önce, başvurulan tüm veritabanları geçici yerel veri kökü klasörüne dağıtılır. Ve her çalışma için, geçici veri kökü klasörü yeni bir yalıtılmış ortam olarak temizlenir.

Bu ilgili makaleye bakın:
* [U-SQL veritabanı projelerinde](data-lake-analytics-data-lake-tools-develop-usql-database.md)U-SQL veritabanı tanımlarını ve başvurularını nasıl yöneteceklerini öğrenin.

## <a name="the-difference-between-local-machine-and-local-project-accounts"></a>**Yerel makine** ve **Yerel proje** hesapları arasındaki fark

**Yerel makine** hesabı, kullanıcıların yerel makinelerinde bir Azure Veri Gölü Analizi hesabı simüle eder. Aynı deneyimi bir Azure Veri Gölü Analytics hesabıyla paylaşır. **Yerel proje** hesabı, kullanıcı dostu bir yerel geliştirme ortamı sağlar. Bu ortam, kullanıcıların komut dosyalarını yerel olarak çalıştırmadan önce veritabanı başvurularını dağıtmasına ve veri girişine yardımcı olur. **Yerel makine** hesabı, tüm projeler aracılığıyla erişilebilen paylaşılan kalıcı bir ortam sağlar. **Yerel proje** hesabı, her proje için yalıtılmış bir geliştirme ortamı sağlar. Her koşu için yenilenir. **Yerel proje** hesabı, hızlı bir şekilde yeni değişiklikler uygulayarak daha hızlı bir geliştirme deneyimi sunar.

**Yerel makine** ve Yerel **proje** hesapları arasındaki diğer farklar aşağıdaki tabloda gösterilmiştir:

|Fark açısı|Yerel makine|Yerel proje|
|----------------|---------------|---------------|
|Yerel erişim|Tüm projeler tarafından erişilebilir.|Bu hesaba yalnızca ilgili proje erişebilir.|
|Yerel veri kökü klasörü|Kalıcı bir yerel klasör. **Araçlar** > **Veri Gölü** > **Seçenekleri ve Ayarları**ile yapılandırıldı.|U-SQL proje çalışma dizini altında her yerel çalıştırma için oluşturulan geçici bir klasör. Yeniden oluşturma veya yeniden çalıştırma olduğunda klasör temizlenir.|
|U-SQL komut dosyası için giriş verileri|Kalıcı yerel veri kökü klasörü altında göreli yol.|**U-SQL proje özelliği** > **Test Veri Kaynağı**üzerinden ayarlayın. Tüm dosyalar ve alt klasörler yerel çalışmadan önce geçici veri kökü klasörüne kopyalanır.|
|U-SQL komut dosyası için çıktı verileri|Kalıcı yerel veri kökü klasörü altında göreli yol.|Geçici veri kökü klasörüne çıktı. Yeniden oluşturma veya yeniden çalıştırma gerçekleştiğinde sonuçlar temizlenir.|
|Başvurulan veritabanı dağıtımı|Başvurulan veritabanları **Yerel makine** hesabına karşı çalışırken otomatik olarak dağıtılan değildir. Bir Azure Veri Gölü Analytics hesabına göndermek için de geçerlidir.|Başvurulan veritabanları yerel çalışmadan önce yerel **proje** hesabına otomatik olarak dağıtılır. Yeniden oluşturma veya yeniden çalıştırma gerçekleştiğinde tüm veritabanı ortamları temizlenir ve yeniden dağıtılır.|

## <a name="a-local-run-with-the-u-sql-sdk"></a>U-SQL SDK ile yerel bir çalışma

U-SQL komut dosyalarını Visual Studio'da yerel olarak çalıştırabilir ve Komut satırı ve programlama arabirimleriyle U-SQL komut dosyalarını yerel olarak çalıştırmak için Azure Veri Gölü U-SQL SDK'yı da kullanabilirsiniz. Bu arabirimler aracılığıyla, U-SQL yerel çalıştırmaları ve testlerini otomatikleştirebilirsiniz.

Azure Veri [Gölü U-SQL SDK](data-lake-analytics-u-sql-sdk.md)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Gölü Analitiği için BIR CI/CD ardışık yapı kurulumu nasıl.](data-lake-analytics-cicd-overview.md)
- [Azure Veri Gölü Analizi kodunuzu nasıl test emilir?](data-lake-analytics-cicd-test.md)
