---
title: Yönetilen Örnek saat dilimleri
description: Azure SQL Veritabanı Yönetilen Örneği saat dilimi özellikleri hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: ''
ms.date: 09/03/2019
ms.openlocfilehash: 0dbed3db8e106b9bfe1b48ff2b9bc52840fc4c3a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256100"
---
# <a name="time-zones-in-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı Yönetilen Örneği'nde saat dilimleri

Eşgüdümlü Evrensel Saat (UTC), bulut çözümlerinin veri katmanı için önerilen saat dilimidir. Azure SQL Veritabanı Yönetilen Örneği, tarih ve saat değerlerini depolayan ve tarih ve saat işlevlerini belirli bir saat diliminin örtülü bağlamıyla depolayan varolan uygulamaların gereksinimlerini karşılamak için bir saat dilimi seçeneği de sunar.

[GETDATE()](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql) veya CLR kodu gibi T-SQL işlevleri, örnek düzeyinde ayarlanan saat dilimini gözlemler. SQL Server Agent işleri de örneğin saat dilimine göre zamanlamaları izleyin.

  >[!NOTE]
  > Yönetilen Örnek, Azure SQL Veritabanı'nın saat dilimi ayarını destekleyen tek dağıtım seçeneğidir. Diğer dağıtım seçenekleri her zaman UTC'yi izler.
UTC olmayan bir saat diliminde tarih ve saat bilgilerini yorumlamanız [gerekiyorsa, SAAT DILIMI'ni](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql) tek ve birleştirilmiş SQL veritabanlarında kullanın.

## <a name="supported-time-zones"></a>Desteklenen saat dilimleri

Desteklenen saat dilimleri kümesi, yönetilen örneğin temel işletim sisteminden devralır. Yeni saat dilimi tanımlarını almak ve varolanlarla yapılan değişiklikleri yansıtmak için düzenli olarak güncelleştirilir.

[Gün ışığından yararlanma saati/saat dilimi değişiklikleri politikası,](https://aka.ms/time) 2010'dan itibaren tarihsel doğruluğu garanti eder.

Desteklenen saat dilimlerinin adlarının yer alan bir listesi [sys.time_zone_info](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql) sistem görünümünde açıklanır.

## <a name="set-a-time-zone"></a>Saat dilimi ayarlama

Yönetilen bir örneğin saat dilimi yalnızca örnek oluşturma sırasında ayarlanabilir. Varsayılan saat dilimi UTC'dir.

  >[!NOTE]
  > Varolan yönetilen bir örneğin saat dilimi değiştirilemez.

### <a name="set-the-time-zone-through-the-azure-portal"></a>Azure portalı üzerinden saat dilimini ayarlama

Yeni bir örnek için parametreler girdiğinizde, desteklenen saat dilimleri listesinden bir saat dilimi seçin.
  
![Örnek oluşturma sırasında bir saat dilimi ayarlama](media/sql-database-managed-instance-timezone/01-setting_timezone-during-instance-creation.png)

### <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

Örnek oluşturma sırasında saat dilimini ayarlamak için [Kaynak Yöneticisi şablonunuzda](https://aka.ms/sql-mi-create-arm-posh) saat dilimi özelliğini belirtin.

```json
"properties": {
                "administratorLogin": "[parameters('user')]",
                "administratorLoginPassword": "[parameters('pwd')]",
                "subnetId": "[parameters('subnetId')]",
                "storageSizeInGB": 256,
                "vCores": 8,
                "licenseType": "LicenseIncluded",
                "hardwareFamily": "Gen5",
                "collation": "Serbian_Cyrillic_100_CS_AS",
                "timezoneId": "Central European Standard Time"
            },

```

Bu makalenin sonunda, timezoneId özelliği için desteklenen değerlerin listesi yer almaktadır.

Belirtilmemişse, saat dilimi UTC olarak ayarlanır.

## <a name="check-the-time-zone-of-an-instance"></a>Bir örneğin saat dilimini denetleme

[CURRENT_TIMEZONE](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql) işlevi, örneğin saat diliminin görüntü adını döndürür.

## <a name="cross-feature-considerations"></a>Çapraz özellik hususları

### <a name="restore-and-import"></a>Geri yükleme ve alma

Bir yedekleme dosyasını geri yükleyebilir veya farklı saat dilimi ayarlarına sahip bir örnekten veya sunucudan yönetilen bir örne veri aktarabilirsiniz. Dikkatli olun. Uygulama davranışını ve sorguların ve raporların sonuçlarını analiz edin, tıpkı farklı saat dilimi ayarlarına sahip iki SQL Server örneği arasında veri aktarımı yaptığınızda olduğu gibi.

### <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme

Zamanında geri yükleme yaptığınızda, geri yükleme süresi UTC zamanı olarak yorumlanır. Bu şekilde gün ışığından yararlanma saati ve olası değişiklikler nedeniyle herhangi bir belirsizlik önlenir.

### <a name="auto-failover-groups"></a>Otomatik yük devretme grupları

Bir başarısız lık grubunda birincil ve ikincil bir örnekte aynı saat dilimini kullanmak zorlanmaz, ancak şiddetle tavsiye ediyoruz.

  >[!WARNING]
  > Başarısız bir grupta birincil ve ikincil örnek için aynı saat dilimini kullanmanızı şiddetle öneririz. Bazı nadir kullanım örnekleri nedeniyle birincil ve ikincil örnekler arasında aynı saat dilimini tutmak zorlanmaz. Manuel veya otomatik arıza durumunda, ikincil örneğin orijinal saat dilimini koruyacağını anlamak önemlidir.

## <a name="limitations"></a>Sınırlamalar

- Varolan yönetilen örneğin saat dilimi değiştirilemez.
- SQL Server Agent işlerinden başlatılan dış işlemler, örneğin saat dilimini gözlemlemez.

## <a name="list-of-supported-time-zones"></a>Desteklenen saat dilimleri listesi

| **Saat dilimi kimliği** | **Saat dilimi görüntü adı** |
| --- | --- |
| Dateline Standart Saat | (UTC-12:00) Uluslararası Tarih Hattı Batı |
| UTC-11 | (UTC-11:00) Eşgüdümlü Evrensel Zaman-11 |
| Aleutian Standart Saati | (UTC-10:00) Aleut Adaları |
| Hawaii Standart Saati | (UTC-10:00) Hawaii |
| Marquesas Standart Saati | (UTC-09:30) Marquesas Adaları |
| Alaska Standart Saati | (UTC-09:00) Alaska |
| UTC-09 | (UTC-09:00) Eşgüdümlü Evrensel Zaman-09 |
| Pasifik Standart Saati (Meksika) | (UTC-08:00) Baja Kaliforniya |
| UTC-08 | (UTC-08:00) Eşgüdümlü Evrensel Zaman-08 |
| Pasifik Standart Saati | (UTC-08:00) Pasifik Saati (ABD & Kanada) |
| ABD Dağ Standart Saati | (UTC-07:00) Arizona |
| Dağ Standart Saati (Meksika) | (UTC-07:00) Chihuahua, La Paz, Mazatlan |
| Dağ Standart Saati | (UTC-07:00) Dağ Saati (ABD & Kanada) |
| Orta Amerika Standart Saati | (UTC-06:00) Orta Amerika |
| Merkezi Standart Saat | (UTC-06:00) Merkezi Saat (ABD & Kanada) |
| Paskalya Adası Standart Saati | (UTC-06:00) Paskalya Adası |
| Merkezi Standart Saat (Meksika) | (UTC-06:00) Guadalajara, Mexico City, Monterrey |
| Kanada Merkezi Standart Saati | (UTC-06:00) Saskatchewan |
| SA Pasifik Standart Saati | (UTC-05:00) Bogota, Lima, Quito, Rio Branco |
| Doğu Standart Saati (Meksika) | (UTC-05:00) Chetumal |
| Doğu Standart Saati | (UTC-05:00) Doğu Saati (ABD & Kanada) |
| Haiti Standart Saati | (UTC-05:00) Haiti |
| Küba Standart Saati | (UTC-05:00) Havana |
| ABD Doğu Standart Saati | (UTC-05:00) Indiana (Doğu) |
| Turks Ve Caicos Standart Saati | (UTC-05:00) Turks ve Caicos Adaları |
| Paraguay Standart Saati | (UTC-04:00) Öztürk |
| Atlantik Standart Saati | (UTC-04:00) Atlantik Saati (Kanada) |
| Venezuela Standart Saati | (UTC-04:00) Caracas |
| Orta Brezilya Standart Saati | (UTC-04:00) Cuiabá |
| SA Batı Standart Saati | (UTC-04:00) Georgetown, La Paz, Manaus, San Juan |
| Pasifik SA Standart Saati | (UTC-04:00) Santiago |
| Newfoundland Standart Saati | (UTC-03:30) Newfoundland |
| Tocantins Standart Saati | (UTC-03:00) Araguaina |
| E. Güney Amerika Standart Saati | (UTC-03:00) Brasilia |
| SA Doğu Standart Saati | (UTC-03:00) Cayenne, Fortaleza |
| Arjantin Standart Saati | (UTC-03:00) Buenos Aires Şehri |
| Grönland Standart Saati | (UTC-03:00) Grönland |
| Montevideo Standart Saati | (UTC-03:00) Montevideo |
| Magallanes Standart Saati | (UTC-03:00) Punta Arenalar |
| Saint Pierre Standart Saati | (UTC-03:00) Aziz Pierre ve Miquelon |
| Bahia Standart Saati | (UTC-03:00) Salvador |
| UTC-02 | (UTC-02:00) Eşgüdümlü Evrensel Zaman-02 |
| Orta Atlantik Standart Saati | (UTC-02:00) Orta Atlantik - Eski |
| Azores Standart Saati | (UTC-01:00) Azores |
| Cape Verde Standart Saati | (UTC-01:00) Cabo Verde. |
| UTC | (UTC) Eşgüdümlü Evrensel Saat |
| GMT Standart Saati | (UTC+00:00) Dublin, Edinburgh, Lizbon, Londra |
| Greenwich Standart Saati | (UTC+00:00) Monrovia, Reykjavik |
| W. Avrupa Standart Saati | (UTC+01:00) Amsterdam, Berlin, Bern, Roma, Stockholm, Viyana |
| Orta Avrupa Standart Saati | (UTC+01:00) Belgrad, Bratislava, Budapeşte, Ljubljana, Prag |
| Romance Standart Zaman | (UTC+01:00) Brüksel, Kopenhag, Madrid, Paris |
| Fas Standart Saati | (UTC+01:00) Casablanca |
| Sao Tome Standart Saati | (UTC+01:00) Sao Tome |
| Orta Avrupa Standart Saati | (UTC+01:00) Saraybosna, Üsküp, Varşova, Zagreb |
| W. Orta Afrika Standart Saati | (UTC+01:00) Batı Orta Afrika |
| Ürdün Standart Saati | (UTC+02:00) Amman |
| GTB Standart Saati | (UTC+02:00) Atina, Bükreş |
| Orta Doğu Standart Saati | (UTC+02:00) Beyrut |
| Mısır Standart Saati | (UTC+02:00) Kahire |
| E. Avrupa Standart Saati | (UTC+02:00) Chisinau |
| Suriye Standart Saati | (UTC+02:00) Şam |
| Batı Şeria Standart Saati | (UTC+02:00) Gazze, El Halil |
| Güney Afrika Standart Saati | (UTC+02:00) Harare, Pretoria |
| FLE Standart Saati | (UTC+02:00) Helsinki, Kiev, Riga, Sofya, Tallinn, Vilnius |
| İsrail Standart Saati | (UTC+02:00) Kudüs |
| Kaliningrad Standart Saati | (UTC+02:00) Kaliningrad |
| Sudan Standart Saati | (UTC+02:00) Hartum |
| Libya Standart Saati | (UTC+02:00) Trablus |
| Namibya Standart Saati | (UTC+02:00) Windhoek |
| Arapça Standart Saat | (UTC+03:00) Bağdat |
| Türkiye Standart Saati | (UTC+03:00) Istanbul |
| Arap Standart Saati | (UTC+03:00) Kuveyt, Riyad |
| Beyaz Rusya Standart Saati | (UTC+03:00) Minsk |
| Rusya Standart Saati | (UTC+03:00) Moskova, St. Petersburg |
| E. Afrika Standart Saati | (UTC+03:00) Nairobi |
| İran Standart Saati | (UTC+03:30) Tahran |
| Arap Standart Saati | (UTC+04:00) Abu Dabi, Muscat |
| Astrakhan Standart Saati | (UTC+04:00) Astrakhan, Ulyanovsk |
| Azerbaycan Standart Saati | (UTC+04:00) Bakü |
| Rusya Saat Dilimi 3 | (UTC+04:00) Izhevsk, Samara |
| Mauritius Standart Saati | (UTC+04:00) Port Louis |
| Saratov Standart Saati | (UTC+04:00) Saratov |
| Gürcü Standart Saati | (UTC+04:00) Tiflis |
| Volgograd Standart Saati | (UTC+04:00) Volgograd |
| Kafkasya Standart Saati | (UTC+04:00) Erivan |
| Afganistan Standart Saati | (UTC+04:30) Kabil |
| Batı Asya Standart Saati | (UTC+05:00) Aşkabat, Taşkent |
| Ekaterinburg Standart Saati | (UTC+05:00) Ekaterinburg |
| Pakistan Standart Saati | (UTC+05:00) İslamabad, Karaçi |
| Hindistan Standart Saati | (UTC+05:30) Chennai, Kalküta, Mumbai, Yeni Delhi |
| Sri Lanka Standart Saati | (UTC+05:30) Sri Jayawardenepura |
| Nepal Standart Saati | (UTC+05:45) Katmandu |
| Orta Asya Standart Saati | (UTC+06:00) Astana |
| Bangladeş Standart Saati | (UTC+06:00) Dakka |
| Omsk Standart Saati | (UTC+06:00) Omsk |
| Myanmar Standart Saati | (UTC+06:30) Yangon (Rangoon) |
| SE Asya Standart Saati | (UTC+07:00) Bangkok, Hanoi, Jakarta |
| Altay Standart Saati | (UTC+07:00) Barnaul, Gorno-Altaysk |
| W. Moğolistan Standart Saati | (UTC+07:00) Hovd |
| Kuzey Asya Standart Saati | (UTC+07:00) Krasnoyarsk |
| N. Orta Asya Standart Saati | (UTC+07:00) Novosibirsk |
| Tomsk Standart Saati | (UTC+07:00) Tomsk |
| Çin Standart Saati | (UTC+08:00) Pekin, Chongqing, Hong Kong, Urumçi |
| Kuzey Asya Doğu Standart Saati | (UTC+08:00) ırkutsk |
| Singapur Standart Saati | (UTC+08:00) Kuala Lumpur, Singapur |
| W. Avustralya Standart Saati | (UTC+08:00) Perth |
| Taipei Standart Saati | (UTC+08:00) Taipei |
| Ulaanbaatar Standart Saati | (UTC+08:00) Ulaanbaatar |
| Aus Merkez W. Standart Saat | (UTC+08:45) Eucla |
| Transbaikal Standart Saati | (UTC+09:00) Chita |
| Tokyo Standart Saati | (UTC+09:00) Osaka, Sapporo, Tokyo |
| Kuzey Kore Standart Saati | (UTC+09:00) Pyongyang |
| Kore Standart Saati | (UTC+09:00) Seul |
| Yakutsk Standart Saati | (UTC+09:00) Yakutsk |
| Cen, ne oldu? Avustralya Standart Saati | (UTC+09:30) Adelaide |
| AUS Merkezi Standart Saati | (UTC+09:30) Darwin |
| E. Avustralya Standart Saati | (UTC+10:00) Brisbane |
| AUS Doğu Standart Saati | (UTC+10:00) Canberra, Melbourne, Sidney |
| Batı Pasifik Standart Saati | (UTC+10:00) Guam, Port Moresby |
| Tazmanya Standart Saati | (UTC+10:00) Hobart |
| Vladivostok Standart Saati | (UTC+10:00) Vladivostok |
| Lord Howe Standart Saati | (UTC+10:30) Lord Howe Adası |
| Bougainville Standart Saati | (UTC+11:00) Bougainville Adası |
| Rusya Saat Dilimi 10 | (UTC+11:00) Çokurdakh |
| Magadan Standart Saati | (UTC+11:00) Magadan |
| Norfolk Standart Saati | (UTC+11:00) Norfolk Adası |
| Sakhalin Standart Saati | (UTC+11:00) Sakhalin |
| Orta Pasifik Standart Saati | (UTC+11:00) Solomon Is., Yeni Kaledonya |
| Rusya Saat Dilimi 11 | (UTC+12:00) Anadyr, Petropavlovsk-Kamchatsky |
| Yeni Zelanda Standart Saati | (UTC+12:00) Auckland, Wellington |
| UTC+12 | (UTC+12:00) Eşgüdümlü Evrensel Zaman+12 |
| Fiji Standart Saati | (UTC+12:00) Fiji |
| Kamçatka Standart Saati | (UTC+12:00) Petropavlovsk-Kamchatsky - Eski |
| Chatham Adaları Standart Saat | (UTC+12:45) Chatham Adaları |
| UTC+13 | (UTC+13:00) Eşgüdümlü Evrensel Zaman+13 |
| Tonga Standart Saati | (UTC+13:00) Nuku'alofa |
| Samoa Standart Saati | (UTC+13:00) Samoa |
| Line Adaları Standart Saati | (UTC+14:00) Kiritimati Adası |

## <a name="see-also"></a>Ayrıca bkz. 

- [CURRENT_TIMEZONE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql)
- [AT TIME ZONE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql)
- [sys.time_zone_info (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql)
