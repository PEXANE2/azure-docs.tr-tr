---
title: Hibrit modunu kullanmayla bilinen sorunlar/geçiş sınırlamaları
description: Azure Veritabanı Geçiş Hizmeti'ni karma modda kullanmayla ilgili bilinen sorunlar/geçiş sınırlamaları hakkında bilgi edinin.
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
ms.openlocfilehash: aedc7ea3d778d52f6f348837430987568af188ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77649611"
---
# <a name="known-issuesmigration-limitations-with-using-hybrid-mode"></a>Karma modu kullanarak bilinen sorunlar/geçiş sınırlamaları

Azure Veritabanı Geçiş Hizmeti'nin karma modda kullanılmasıyla ilgili bilinen sorunlar ve sınırlamalar aşağıdaki bölümlerde açıklanmıştır.

## <a name="installer-fails-to-authenticate"></a>Yükleyici kimlik doğrulaması başarısız olur

Sertifikayı AdApp'ınıza yükledikten sonra, Azure ile kimlik doğrulaması yapabilmek için birkaç dakikaya kadar gecikme olur. Yükleyici bazı gecikmelerle yeniden denemeyi denemeye çalışır, ancak yayılma gecikmesinin yeniden denemeden daha uzun olması mümkündür ve bir **FailedToGetAccessTokenException** iletisi görürsünüz. Sertifika doğru AdApp'a yüklendiyse ve doğru AppId dmsSettings.json'da sağlandıysa, yükleme komutunu yeniden çalıştırmayı deneyin.

## <a name="service-offline-after-successful-installation"></a>Başarılı yüklemeden sonra hizmet "çevrimdışı"

Yükleme işlemi başarıyla tamamlandıktan sonra hizmet çevrimdışı olarak gösteriliyorsa, aşağıdaki adımları kullanmayı deneyin.

1. Azure portalında, Azure Veritabanı Geçiş Hizmeti örneğinde, **Karma** ayarlar sekmesine gidin ve ardından kayıtlı çalışanların ızgarasını denetleyerek çalışanın kayıtlı olduğunu doğrulayın.

    Bu çalışanın durumu **Çevrimiçi**olmalıdır, ancak bir sorun varsa **Çevrimdışı** olarak gösterilebilir.

2. Alt bilgisayarda, aşağıdaki PowerShell komutunu çalıştırarak hizmetin durumunu denetleyin:

    ```
    Get-Service Scenario*
    ```

    Bu komut, alt çalışanı çalıştıran Windows hizmetinin durumunu verir. Sadece tek bir sonuç olmalı. İşçi durdurulursa, aşağıdaki PowerShell komutunu kullanarak yeniden başlatmayı deneyebilirsiniz:

    ```
    Start-Service Scenario*
    ```

    Windows Hizmetleri UI'deki hizmeti de denetleyebilirsiniz.

3. Windows hizmeti Çalıştır ve Durdurarasında döngüleri varsa, o zaman çalışan başlarken sorunlarla karşılaştı. Sorunu belirlemek için Azure Veritabanı Geçiş Hizmeti karma alt günlüğünü denetleyin.

    - Yükleme işlemi günlükleri, yükleyicinin çalıştırıldığı klasördeki "günlükler" klasöründe depolanır.
    - Azure Veritabanı Geçiş Hizmeti karma alt **günlükleri, İşçi Günlükleri** klasöründe, çalışanın yüklü olduğu klasörde depolanır. Karma alt günlük dosyaları için varsayılan konum **C:\Program Files\DatabaseMigrationServiceHybrid\WorkerLogs**olduğunu.

## <a name="using-your-own-signed-certificate"></a>Kendi imzalı sertifikanızı kullanma

Action GenerateCert tarafından oluşturulan sertifika, iç güvenlik ilkelerinize dayalı olarak kabul edilemeyecek, kendi imzalı bir sertifikadır. Bu sertifikayı kullanmak yerine, kendi sertifikanızı sağlayabilir ve dmsSettings.json'da parmak izini sağlayabilirsiniz. Bu sertifikanın AdApp'ınıza yüklenmesi ve Azure Veritabanı Geçiş Hizmeti karma işçisini yüklediğiniz bilgisayara yüklenmesi gerekir. Ardından, bu sertifikayı özel anahtarla birlikte Yerel Makine sertifika deposuna yükleyin.

## <a name="running-the-worker-service-as-a-low-privilege-account"></a>İşçi hizmetini düşük ayrıcalıklı bir hesap olarak çalıştırma

Varsayılan olarak, Azure Veritabanı Geçiş Hizmeti karma alt hizmet Yerel Sistem hesabı olarak çalışır. Kullandığınız hesabın ağ izinleri olduğu sürece bu hizmet için kullanılan hesabı değiştirebilirsiniz. Hizmet 'olarak çalıştır' hesabını değiştirmek için aşağıdaki işlemi kullanın.

1. Windows Hizmetleri aracılığıyla veya PowerShell'deki Hizmeti Durdur komutunu kullanarak hizmeti durdurun.

2. Farklı bir oturum açma hesabı kullanmak için hizmeti güncelleştirin.

3. Yerel Bilgisayar sertifikaları için certmgr olarak, **DMS Hybrid App Key** ve **DMS Scenario Engine Key Pair** sertifikaları için yeni hesaba özel anahtar izinleri verin.

    a. Aşağıdaki tuşları görüntülemek için certmgr'i açın:

    - DMS Hibrit Uygulama Anahtarı
    - DMS Hibrit İşçi Kurulum Anahtarı
    - DMS Senaryo Motoru Anahtar Çifti

    b. **DMS Hibrit Uygulama Anahtarı** girişine sağ tıklayın, **Tüm Görevler'e**işaret edin ve ardından Özel **Tuşları Yönet'i**seçin.

    c. **Güvenlik** sekmesinde **Ekle'yi**seçin ve ardından hesabın adını girin.

    d. **DMS Scenario Engine Engine Key Pair** sertifikasına yeni hesap için özel anahtar izni vermek için aynı adımları kullanın.

## <a name="unregistering-the-worker-manually"></a>İşçinin el ile kaydının silinme

Artık çalışan bilgisayara erişiminiz yoksa, aşağıdaki adımları gerçekleştirerek çalışanın kaydını kaldırabilir ve Azure Veritabanı Geçiş Hizmeti örneğini yeniden kullanabilirsiniz:

1. Azure portalında, Azure Veritabanı Geçiş Hizmeti örneğinize gidin ve ardından **Karma** ayarlar sayfasına gidin.

   İşçi girişiniz listede görünür ve durum **Çevrimdışı**olarak görüntülenir.

2. İşçi giriş listesinin en sağında, elipsleri seçin ve ardından **Kayıt Dışı'yı**seçin.

## <a name="addressing-issues-for-specific-migration-scenarios"></a>Belirli geçiş senaryoları için sorunları ele alma

Aşağıdaki bölümlerde, çevrimiçi geçiş gerçekleştirmek için Azure Veritabanı Geçiş Hizmeti karma modunu kullanmayla ilgili senaryoya özgü sorunlar açıklanmaktadır.

### <a name="online-migrations-to-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı'na çevrimiçi geçişler yönetilen örnek

**Yüksek CPU kullanımı**

**Sorun**: SQL Veritabanı yönetilen örneğine çevrimiçi geçişler için, hibrit alt kişiyi çalıştıran bilgisayar, çok fazla yedekleme varsa veya yedeklemeler çok büyükse yüksek CPU kullanımıyla karşılaşır.

**Azaltma**: Bu sorunu azaltmak için sıkıştırılmış yedeklemeler kullanın, geçişi birden çok paylaşım kullanacak şekilde bölün veya karma alt çalışan bilgisayarı ölçeklendirin.
