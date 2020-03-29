---
title: Kişisel bilgiler için adlandırılmış varlıklar
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/06/2020
ms.author: aahi
ms.openlocfilehash: d678a29de9dea8a5a2f6d0259a452ca4c69feb03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77086877"
---
## <a name="personal-information-entity-types"></a>Kişisel bilgiler varlık türleri:

### <a name="person"></a>Kişi
Metindeki kişi adlarını tanıyın.

Diller:
* Genel önizleme:`English`

| Alt tür adı | Açıklama                                               | Model sürümüyle başlayan kullanılabilir |
|--------------|-----------------------------------------------------------|----------------------------------------|
| Yok          | Tanınan adlar, `Bill Gates`örneğin ,`Marie Curie` | `2020-02-01`                           |

### <a name="organization"></a>Kuruluş  

Kuruluşları, şirketleri, ajansları, şirketleri, kulüpleri ve diğer insan gruplarını tanıyın.

Diller: 

* Genel önizleme:`English`

| Alt tür adı | Açıklama                                                                                       | Model sürümüyle başlayan kullanılabilir|
|--------------|---------------------------------------------------------------------------------------------------|--------------|
| Yok          | kuruluşlar, örneğin `Microsoft` `NASA`,`National Oceanic and Atmospheric Administration` | `2020-02-01` |

### <a name="phone-number"></a>Telefon Numarası

Telefon numaraları (yalnızca ABD Telefon numaraları). 

Diller:

* Genel önizleme:`English`

| Alt tür adı | Açıklama                                    | Model sürümüyle başlayan kullanılabilir |
|--------------|------------------------------------------------|----------------------------------------|
| Yok          | Örneğin ABD telefon numaraları`(312) 555-0176` | `2020-02-01`                           |

### <a name="email"></a>Email

E-posta adresi. 

Diller:

* Genel önizleme:`English`

| Alt tür adı | Açıklama                                      | Model sürümüyle başlayan kullanılabilir |
|--------------|--------------------------------------------------|----------------------------------------|
| Yok          | Örneğin, e-posta adresi`support@contoso.com` | `2020-02-01`                           |

### <a name="url"></a>URL'si

İnternet URL'leri.

Diller:

* Genel önizleme:`English`

| Alt tür adı | Açıklama                                          | Model sürümüyle başlayan kullanılabilir |
|--------------|------------------------------------------------------|----------------------------------------|
| Yok          | Örneğin, web sitelerine URL'ler`https://www.bing.com` | `2020-02-01`                           |

### <a name="ip-address"></a>IP Adresi

İnternet Protokol Adresi

Diller:

* Genel önizleme:`English`

| Alt tür adı | Açıklama                              | Model sürümüyle başlayan kullanılabilir |
|--------------|------------------------------------------|----------------------------------------|
| Yok          | Örneğin ağ adresi`10.0.0.101` | `2020-02-01`                           |

### <a name="quantity"></a>Miktar 

Sayısal miktarlar

Diller:

* Genel önizleme:`English`

| Alt tür adı | Açıklama                   | Model sürümüyle başlayan kullanılabilir |
|--------------|-------------------------------|----------------------------------------|
| Yaş          | `90 days old`, `30 years old` | `2020-02-01`                           |

### <a name="datetime"></a>DateTime

Tarih ve Saat varlıkları

Diller:

* Genel önizleme:`English`

| Alt tür adı | Açıklama                   | Model sürümüyle başlayan kullanılabilir |
|--------------|-------------------------------|----------------------------------------|
| Tarih         | `May 2nd, 2017`, `05/02/2017` | `2020-02-01`                           |

### <a name="eu-gps-coordinates"></a>AB GPS Koordinatları

 Avrupa Birliği içindeki yerlerin GPS koordinatları. 

Diller:

* Genel önizleme:`English`

| Alt tür adı | Açıklama                               | Model sürümüyle başlayan kullanılabilir |
|--------------|-------------------------------------------|----------------------------------------|
| Yok          | Avrupa Birliği içinde GPS koordinatları | `2019-10-01`                           |

### <a name="azure-information"></a>Azure bilgileri

Kimlik doğrulama bilgileri ve bağlantı dizeleri de dahil olmak üzere tanımlanabilir Azure bilgileri. 

* Model sürümü `2019-10-01`ile başlayan kullanılabilir.

Diller:

* Genel önizleme:`English`

| Alt tür adı                          | Açıklama                                                                 |
|---------------------------------------|-----------------------------------------------------------------------------|
| Azure DocumentDB Auth Anahtarı             | Azure DocumentDB sunucusu için yetkilendirme anahtarı.                           |
| Azure IAAS Veritabanı Bağlantı Dizesi | Hizmet (IaaS) veritabanı olarak Azure Altyapısı için bağlantı dizesi. |
| Azure SQL Bağlantı Dizesi           | Azure SQL veritabanı için bağlantı dizesi.                                |
| Azure IoT Bağlantı Dizesi           | Nesnelerin Azure İnterneti (IoT) için bağlantı dizesi.                        |
| Azure Yayımlama Ayar Parolası        | Azure Yayımlama ayarları için parola.                                        |
| Azure Redis Önbellek Bağlantı Dizesi   | Redis için bir Azure Önbelleği için bağlantı dizesi.                             |
| Azure SAS                             | Hizmet Olarak Azure Yazılımı (SAS) için bağlantı dizesi.                     |
| Azure Servis Veri Çiş Bağlantı Dizesi   | Azure servis veri veri nesi için bağlantı dizesi.                                |
| Azure Depolama Hesap Anahtarı             | Azure depolama hesabının hesap anahtarı.                                   |
| Azure Depolama Hesap Anahtarı (Genel)   | Azure depolama hesabı için genel hesap anahtarı.                           |
| SQL Server Bağlantı Dizesi          | BIR SQL sunucusu için bağlantı dizesi.                                         |

### <a name="identification"></a>Kimlik

* Model sürümü `2019-10-01`ile başlayan kullanılabilir.

Diller:

* Genel önizleme:`English`

#### <a name="financial-account-identification"></a>Finansal Hesap Tanımlama

| Alt tür adı               | Açıklama                                                                |
|----------------------------|----------------------------------------------------------------------------|
| ABA Yönlendirme Numaraları        | Amerikan Bankacılar Birliği (ABA) transit yönlendirme numaraları.                  |
| SWIFT Kodu                 | Ödeme talimatı bilgileri için SWIFT kodları.                           |
| Kredi Kartı                | Kredi kartı numaraları.                                                       |
| IBAN Kodu                  | Ödeme talimatı bilgileri için IBAN kodları.                            |

#### <a name="government-and-country-specific-identification"></a>Hükümete ve ülkeye özel Tanımlama

Aşağıdaki varlıklar ülkelere göre gruplandırılır ve listelenir:

Arjantin
* Ulusal Kimlik (DNI) Numarası

Avustralya
* Vergi dosya numarası 
* Ehliyet numarası
* Pasaport Kimliği
* Tıbbi hesap numarası
* banka hesap numaraları (örneğin çek, tasarruf ve borç hesapları)

Belçika
* Ulusal sayı

Brezilya
* Tüzel Kişi Numarası (CNPJ)
* CPF numarası
* Ulusal Kimlik Kartı (RG)

Kanada
* Pasaport Kimliği
* Ehliyet numarası
* Sağlık Sigortası Numarası
* Kişisel sağlık kimlik numarası (PHIN)
* Sosyal Güvenlik Numarası
* banka hesap numaraları (örneğin çek, tasarruf ve borç hesapları)

Şili
* Kimlik kartı numarası 

Çin
* Kimlik kartı numarası
* Yerleşik kimlik kartı (PRC) numarası

Hırvatistan
* Kimlik kartı numarası
* Kişisel Kimlik (OIB) numarası

Çek Cumhuriyeti
* Ulusal kimlik kartı numarası

Danimarka
* Kişisel kimlik numarası

Avrupa Birliği (AB)
* Ulusal kimlik numarası
* Pasaport Kimliği
* Ehliyet numarası
* Sosyal Güvenlik Numarası (SSN) veya eşdeğer kimlik
* AB Vergi Kimlik Numarası (TIN)
* AB Banka Kartı Numarası

Finlandiya
* Ulusal kimlik numarası
* Pasaport Kimliği

Fransa
* Ulusal kimlik kartı (CNI)
* Sosyal Güvenlik numarası (INSEE)
* Pasaport Kimliği
* Ehliyet numarası

Almanya
* Kimlik Kartı numarası
* Pasaport Kimliği
* Ehliyet numarası

Yunanistan 
* Ulusal kimlik kartı numarası

Hong Kong
* Kimlik kartı (HKID) numarası

Hindistan
* Kalıcı Hesap numarası (PAN)
* Benzersiz Kimlik (Aadhaar) Numarası

Endonezya
* Kimlik kartı numarası (KTP)

İrlanda
* Kişisel Kamu Hizmeti (PPS) Numarası

İsrail
* Ulusal Kimlik
* banka hesap numaraları (örneğin çek, tasarruf ve borç hesapları)

İtalya
* Ehliyet numarası

Japonya
* Yerleşik kayıt numarası
* İkamet kartı numarası
* Ehliyet numarası
* Sosyal Sigorta Numarası (SIN)
* Pasaport Kimliği
* banka hesap numaraları (örneğin çek, tasarruf ve borç hesapları)

Malezya
* Kimlik kartı numarası

Hollanda
* Vatandaş Servisi (BSN) numarası

Yeni Zelanda
* Sağlık Bakanlığı Numarası

Norveç
* Kimlik kartı numarası

Filipinler
* Birleştirilmiş Çok Amaçlı Kimlik Numarası

Polonya
* Kimlik Kartı numarası
* Ulusal Kimlik (PESEL)
* Pasaport Kimliği

Portekiz 
* Vatandaş Kart Numarası

Suudi Arabistan
* Ulusal Kimlik

Singapur
* Ulusal Kayıt Kimlik Kartı (NRIC) numarası

Güney Afrika
* Kimlik Numarası
* Yerleşik Kayıt numarası

Güney Kore
* Yerleşik Kayıt Numarası

İspanya 
* Sosyal Güvenlik Numarası (SSN)

İsveç
* Ulusal Kimlik
* Pasaport Kimliği

Tayvan 
* Ulusal Kimlik
* İkamet Belgesi (ARC/TARC) numarası
* Pasaport Kimliği

Tayland
* Nüfus Tanımlama kodu

Birleşik Krallık
* Pasaport Kimliği
* Ehliyet numarası
* Ulusal Sigorta numarası (NINO)
* Ulusal Sağlık Servisi (NHS) numarası

Amerika Birleşik Devletleri
* Sosyal Güvenlik Numarası (SSN)
* Ehliyet numarası
* Pasaport Kimliği
* Seçim rulo numarası
* Bireysel Vergi Kimlik Numarası (ITIN)
* Uyuşturucu yla Mücadele Dairesi (DEA) numarası
* banka hesap numaraları (örneğin çek, tasarruf ve borç hesapları)
