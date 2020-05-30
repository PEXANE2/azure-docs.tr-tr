---
title: Karma mod kullanımıyla ilgili bilinen sorunlar/geçiş sınırlamaları
description: Azure veritabanı geçiş hizmeti 'ni karma modda kullanmayla ilgili bilinen sorunlar/geçiş sınırlamaları hakkında bilgi edinin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: 5347cda14773583bcfe92a702e59d4967ce2ea09
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84196273"
---
# <a name="known-issuesmigration-limitations-with-using-hybrid-mode"></a>Karma mod kullanımıyla ilgili bilinen sorunlar/geçiş sınırlamaları

Azure veritabanı geçiş hizmeti 'ni karma modda kullanmayla ilişkili bilinen sorunlar ve sınırlamalar aşağıdaki bölümlerde açıklanmıştır.

## <a name="installer-fails-to-authenticate"></a>Yükleyici kimlik doğrulaması yapamıyor

Sertifikayı Uyarınıza yükledikten sonra, Azure ile kimlik doğrulaması yapabilmek için birkaç dakikalık bir gecikme olur. Yükleyici bir gecikme süresiyle yeniden denemeye çalışır, ancak yayma gecikmesi yeniden denemeden daha uzun olabildiğinde, bir **FailedToGetAccessTokenException** iletisi görürsünüz. Sertifika doğru Uyaruya yüklenmişse ve dmsSettings. JSON içinde doğru AppID sağlandıysa, install komutunu yeniden çalıştırmayı deneyin.

## <a name="service-offline-after-successful-installation"></a>Başarılı yüklemeden sonra "çevrimdışı" hizmeti

Yükleme işlemi başarıyla tamamlandıktan sonra hizmet çevrimdışı olarak görünüyorsa, aşağıdaki adımları kullanmayı deneyin.

1. Azure veritabanı geçiş hizmeti örneğinizin Azure portal, **karma** Ayarlar sekmesine gidin ve kayıtlı çalışanların kılavuzunu denetleyerek çalışanın kayıtlı olduğunu doğrulayın.

    Bu çalışanın durumu **çevrimiçi**olmalıdır, ancak bir sorun varsa **çevrimdışı** olarak görünür.

2. Çalışan bilgisayarda, aşağıdaki PowerShell komutunu çalıştırarak hizmetin durumunu denetleyin:

    ```
    Get-Service Scenario*
    ```

    Bu komut, çalışanı çalıştıran Windows hizmetinin durumunu sağlar. Yalnızca tek bir sonuç olmalıdır. Çalışan durdurulmuşsa, aşağıdaki PowerShell komutunu kullanarak yeniden başlatmayı deneyebilirsiniz:

    ```
    Start-Service Scenario*
    ```

    Ayrıca Windows Hizmetleri Kullanıcı arabirimindeki hizmeti de denetleyebilirsiniz.

3. Windows hizmeti çalışır ve durdurulmuş arasında döngü alıyorsa, çalışan çalışırken sorunlarla karşılaştı. Sorunu öğrenmek için Azure veritabanı geçiş hizmeti karma çalışan günlükleri ' ne bakın.

    - Yükleme işlemi günlükleri, yükleyici yürütülebilir dosyasının çalıştırıldığı klasörün içindeki "Günlükler" klasöründe depolanır.
    - Azure veritabanı geçiş hizmeti karma çalışan günlükleri, çalışan tarafından yüklendiği klasördeki **Workerlogs** klasöründe depolanır. Karma çalışan günlük dosyaları için varsayılan konum **C:\Program Files\DatabaseMigrationServiceHybrid\WorkerLogs**şeklindedir.

## <a name="using-your-own-signed-certificate"></a>Kendi imzalı sertifikanızı kullanma

GenerateCert eylemi tarafından oluşturulan sertifika, İç Güvenlik ilkelerinize bağlı olarak kabul edilemeyebilir, kendinden imzalı bir sertifikadır. Bu sertifikayı kullanmak yerine, kendi sertifikanızı girip, dmsSettings. json dosyasında parmak izini sağlayabilirsiniz. Bu sertifikanın, Azure veritabanı geçiş hizmeti karma çalışanını yüklemekte olduğunuz bilgisayara yüklenmiş ve yüklü olması gerekir. Ardından, bu sertifikayı özel anahtarla yerel makine sertifika deposuna yüklemelisiniz.

## <a name="running-the-worker-service-as-a-low-privilege-account"></a>Çalışan hizmetini düşük ayrıcalıklı bir hesap olarak çalıştırma

Varsayılan olarak, Azure veritabanı geçiş hizmeti karma çalışanı hizmeti yerel sistem hesabı olarak çalışır. Kullandığınız hesabın ağ izinleri olduğu sürece, bu hizmet için kullanılan hesabı değiştirebilirsiniz. Hizmeti ' farklı çalıştır ' hesabını değiştirmek için aşağıdaki işlemi kullanın.

1. Windows Hizmetleri aracılığıyla veya PowerShell 'deki Stop-Service komutunu kullanarak hizmeti durdurun.

2. Hizmeti farklı bir oturum açma hesabı kullanacak şekilde güncelleştirin.

3. Yerel bilgisayar sertifikaları için certmgr ' de, **DMS karma uygulama anahtarı** ve **DMS senaryo altyapısı anahtar çifti** sertifikaları için yeni hesaba özel anahtar izinleri verin.

    a. Aşağıdaki anahtarları görüntülemek için certmgr ' i açın:

    - DMS karma uygulama anahtarı
    - DMS karma çalışanı kurulum anahtarı
    - DMS senaryo altyapısı anahtar çifti

    b. **DMS karma uygulama anahtarı** girişine sağ tıklayın, **Tüm görevler**' in üzerine gelin ve ardından **özel anahtarları Yönet**' i seçin.

    c. **Güvenlik** sekmesinde, **Ekle**' yi seçin ve ardından hesabın adını girin.

    d. Yeni hesap için **DMS senaryo altyapısı anahtar çifti** sertifikasına özel anahtar izni vermek için aynı adımları kullanın.

## <a name="unregistering-the-worker-manually"></a>Çalışanın kaydını el ile silme

Artık çalışan bilgisayara erişiminiz yoksa, aşağıdaki adımları uygulayarak çalışan kaydını silip Azure veritabanı geçiş hizmeti örneğinizi yeniden kullanabilirsiniz:

1. Azure portal Azure veritabanı geçiş hizmeti örneğinize gidin ve **karma** ayarlar sayfasına gidin.

   Çalışan girdiniz listede görünür, durumu **çevrimdışı**olarak gösterir.

2. Çalışan girdisi listesinin en sağında, üç noktayı seçin ve ardından **kaydı sil**' i seçin.

## <a name="addressing-issues-for-specific-migration-scenarios"></a>Belirli geçiş senaryolarında adresleme sorunları

Aşağıdaki bölümler, çevrimiçi bir geçiş gerçekleştirmek için Azure veritabanı geçiş hizmeti karma modunu kullanmayla ilgili senaryoya özgü sorunları anlatmaktadır.

### <a name="online-migrations-to-azure-sql-managed-instance"></a>Azure SQL yönetilen örneğine çevrimiçi geçişler

**Yüksek CPU kullanımı**

**Sorun**: SQL yönetilen örneği için çevrimiçi geçişlerde, çok fazla yedek varsa veya yedeklemeler çok büyükse karma çalışanı çalıştıran bılgısayar yüksek CPU kullanımıyla karşılaşacaktır.

**Risk azaltma**: Bu sorunu azaltmak için sıkıştırılmış yedeklemeleri kullanın, geçişi birden çok paylaşım kullanacak şekilde bölüştürün veya karma çalışanı çalıştıran bilgisayarın ölçeğini ölçeklendirin.
