---
title: Azure Backup kullanarak SAP HANA veritabanlarını yedeklerken hata giderme Microsoft Docs
description: SAP HANA veritabanlarını yedeklemek için Azure Backup kullandığınızda oluşabilecek yaygın hataların nasıl giderileceği açıklanmaktadır.
services: backup
author: pvrk
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: pullabhk
ms.openlocfilehash: 32e814ea83f30b48af5ce507ce250f37a34390da
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249482"
---
# <a name="troubleshoot-backup-of-sap-hana-databases-on-azure"></a>Azure 'da SAP HANA veritabanlarının yedeklenmesi sorunlarını giderme

Bu makale, Azure sanal makinelerinde SAP HANA veritabanlarının yedeklenmesi için sorun giderme bilgileri sağlar.

## <a name="prerequisites"></a>Önkoşullar

[Önkoşulların](backup-azure-sap-hana-database.md#prerequisites)bir parçası olarak preregistration BETIĞININ, Hana 'nın yüklü olduğu sanal makinede çalıştırıldığınızdan emin olun.

### <a name="setting-up-permissions"></a>İzinleri ayarlama

Preregistration betiği şunları yapar:

1. HANA sisteminde AZUREWLBACKUPHANAUSER oluşturur ve gerekli rolleri ve izinleri ekler:
    - VERITABANı YÖNETICISI: geri yükleme sırasında yeni DBs oluşturmak için.
    - Katalog okuma: yedekleme kataloğunu okumak için.
    - SAP_INTERNAL_HANA_SUPPORT: birkaç özel tabloya erişmek için.
2. Tüm işlemleri işlemek için (veritabanı sorguları, geri yükleme işlemleri, yedek yapılandırma ve çalıştırma), HANA eklentisinin Hdbuserstore öğesine bir anahtar ekler.
   
   Anahtar oluşturmayı onaylamak için, SIDADDM kimlik bilgileriyle HANA makinesinde HDBSQL komutunu çalıştırın:

    ``` hdbsql
    hdbuserstore list
    ```
    
    Komut çıktısı {SID} {DBNAME} anahtarını, AZUREWLBACKUPHANAUSER olarak gösterilen kullanıcıyla görüntülemelidir.

> [!NOTE]
> **/Usr/SAP/{Sid}/Home/.HDB/** altında benzersiz bir ssfs dosyaları kümesine sahip olduğunuzdan emin olun. Bu yolda yalnızca bir klasör olmalıdır.

### <a name="setting-up-backint-parameters"></a>Backınt parametrelerini ayarlama

Yedekleme için bir veritabanı seçildikten sonra, Azure Backup hizmeti, geri Int parametreleri VERITABANı düzeyinde yapılandırır:

- [catalog_backup_using_backint: true]
- [enable_accumulated_catalog_backup: false]
- [parallel_data_backup_backint_channels: 1]
- [log_backup_timeout_s: 900)]
- [backint_response_timeout: 7200]

> [!NOTE]
> Bu parametrelerin konak düzeyinde mevcut *olmadığından* emin olun. Ana bilgisayar düzeyindeki Parametreler bu parametreleri geçersiz kılacak ve beklenmeyen davranışlara neden olabilir.

## <a name="common-user-errors"></a>Ortak Kullanıcı hataları

### <a name="usererrorinopeninghanaodbcconnection"></a>Usererrorınopeninghanaodbcconnection

data| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| HANA sistemine bağlanılamadı. Sisteminizin çalışır ve çalışır olduğunu doğrulayın.| HANA veritabanı çalışmadığından Azure Backup hizmeti HANA 'ya bağlanamaz. Ya da HANA çalışıyor ancak Azure Backup hizmetinin bağlanmasına izin vermiyor. | HANA veritabanının veya hizmetinin çalışıp çalışmadığını denetleyin. HANA veritabanı veya hizmeti çalışır duruma ayarlanmışsa, [tüm izinlerin ayarlanmış](#setting-up-permissions)olup olmadığını kontrol edin. Anahtar eksikse, yeni bir anahtar oluşturmak için preregistration betiğini yeniden çalıştırın. |

### <a name="usererrorinvalidbackintconfiguration"></a>Usererrorınvalidbackintconfiguration

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Geçersiz Backınt yapılandırması algılandı. Korumayı durdurun ve veritabanını yeniden yapılandırın.| Backınt parametreleri Azure Backup için yanlış belirtilmiştir. | [Parametrelerin ayarlanmış](#setting-up-backint-parameters)olup olmadığını denetleyin. KONAKTA Backınt tabanlı parametreler varsa, bunları kaldırın. Parametreler ana bilgisayar düzeyinde yoksa ancak bir veritabanı düzeyinde el ile değiştirilmişse, bunları daha önce açıklandığı gibi uygun değerlere dönüştürmeniz gerekir. Ya da, **korumayı durdurun ve Azure Portal yedekleme verilerini koruyun** ve sonra **yedeklemeyi devam ettirir**' ı seçin.|
