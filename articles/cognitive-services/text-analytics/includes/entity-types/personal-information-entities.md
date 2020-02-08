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
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2020
ms.locfileid: "77086877"
---
## <a name="personal-information-entity-types"></a>Kişisel bilgi varlık türleri:

### <a name="person"></a>Kişi
Metindeki kişi adlarını tanır.

Diller:
* Genel Önizleme: `English`

| Alt tür adı | Açıklama                                               | Model sürümünden itibaren kullanılabilir |
|--------------|-----------------------------------------------------------|----------------------------------------|
| Yok          | Tanınan adlar, örneğin `Bill Gates``Marie Curie` | `2020-02-01`                           |

### <a name="organization"></a>Kuruluş  

Kuruluşları, şirketleri, kurumları, şirketleri, sinek ve diğer kişi gruplarını tanıyın.

Diller: 

* Genel Önizleme: `English`

| Alt tür adı | Açıklama                                                                                       | Model sürümünden itibaren kullanılabilir|
|--------------|---------------------------------------------------------------------------------------------------|--------------|
| Yok          | kuruluşlar, örneğin `Microsoft`, `NASA`, `National Oceanic and Atmospheric Administration` | `2020-02-01` |

### <a name="phone-number"></a>Telefon numarası

Telefon numaraları (yalnızca ABD telefon numaraları). 

Diller:

* Genel Önizleme: `English`

| Alt tür adı | Açıklama                                    | Model sürümünden itibaren kullanılabilir |
|--------------|------------------------------------------------|----------------------------------------|
| Yok          | ABD telefon numaraları, örneğin `(312) 555-0176` | `2020-02-01`                           |

### <a name="email"></a>E-posta

E-posta adresi. 

Diller:

* Genel Önizleme: `English`

| Alt tür adı | Açıklama                                      | Model sürümünden itibaren kullanılabilir |
|--------------|--------------------------------------------------|----------------------------------------|
| Yok          | E-posta adresi, örneğin `support@contoso.com` | `2020-02-01`                           |

### <a name="url"></a>URL'si

Internet URL 'Leri.

Diller:

* Genel Önizleme: `English`

| Alt tür adı | Açıklama                                          | Model sürümünden itibaren kullanılabilir |
|--------------|------------------------------------------------------|----------------------------------------|
| Yok          | Web sitelerine yönelik URL 'Ler, örneğin `https://www.bing.com` | `2020-02-01`                           |

### <a name="ip-address"></a>IP Adresi

Internet Protokolü adresi

Diller:

* Genel Önizleme: `English`

| Alt tür adı | Açıklama                              | Model sürümünden itibaren kullanılabilir |
|--------------|------------------------------------------|----------------------------------------|
| Yok          | Örneğin, ağ adresi `10.0.0.101` | `2020-02-01`                           |

### <a name="quantity"></a>Miktar 

Sayısal miktarlar

Diller:

* Genel Önizleme: `English`

| Alt tür adı | Açıklama                   | Model sürümünden itibaren kullanılabilir |
|--------------|-------------------------------|----------------------------------------|
| Yaş          | `90 days old`, `30 years old` | `2020-02-01`                           |

### <a name="datetime"></a>DateTime

Tarih ve saat varlıkları

Diller:

* Genel Önizleme: `English`

| Alt tür adı | Açıklama                   | Model sürümünden itibaren kullanılabilir |
|--------------|-------------------------------|----------------------------------------|
| Tarih         | `May 2nd, 2017`, `05/02/2017` | `2020-02-01`                           |

### <a name="eu-gps-coordinates"></a>AB GPS koordinatları

 Avrupa Birliği içindeki konumların GPS koordinatları. 

Diller:

* Genel Önizleme: `English`

| Alt tür adı | Açıklama                               | Model sürümünden itibaren kullanılabilir |
|--------------|-------------------------------------------|----------------------------------------|
| Yok          | Avrupa Birliği içindeki GPS koordinatları | `2019-10-01`                           |

### <a name="azure-information"></a>Azure bilgileri

Kimlik doğrulama bilgileri ve bağlantı dizeleri dahil olmak üzere tanımlanabilir Azure bilgileri. 

* Model sürümü `2019-10-01`başlayarak kullanılabilir.

Diller:

* Genel Önizleme: `English`

| Alt tür adı                          | Açıklama                                                                 |
|---------------------------------------|-----------------------------------------------------------------------------|
| Azure DocumentDB kimlik doğrulama anahtarı             | Azure DocumentDB sunucusu için yetkilendirme anahtarı.                           |
| Azure ıAAS veritabanı bağlantı dizesi | Bir Azure hizmet olarak altyapı (IaaS) veritabanı için bağlantı dizesi. |
| Azure SQL bağlantı dizesi           | Azure SQL veritabanı için bağlantı dizesi.                                |
| Azure IoT bağlantı dizesi           | Azure nesnelerin Interneti (IoT) için bağlantı dizesi.                        |
| Azure yayımlama ayarı parolası        | Azure yayımlama ayarları için parola.                                        |
| Azure Redis Cache bağlantı dizesi   | Redsıs için bir Azure önbelleği bağlantı dizesi.                             |
| Azure SAS                             | Hizmet olarak Azure yazılım (SAS) için bağlantı dizesi.                     |
| Azure Service Bus bağlantı dizesi   | Azure Service Bus için bağlantı dizesi.                                |
| Azure depolama hesabı anahtarı             | Bir Azure depolama hesabı için hesap anahtarı.                                   |
| Azure depolama hesabı anahtarı (genel)   | Bir Azure depolama hesabı için genel hesap anahtarı.                           |
| SQL Server bağlantı dizesi          | Bir SQL Server için bağlantı dizesi.                                         |

### <a name="identification"></a>İsi

* Model sürümü `2019-10-01`başlayarak kullanılabilir.

Diller:

* Genel Önizleme: `English`

#### <a name="financial-account-identification"></a>Finansal hesap kimliği

| Alt tür adı               | Açıklama                                                                |
|----------------------------|----------------------------------------------------------------------------|
| ABA yönlendirme numaraları        | American banker Association (ABA) transit yönlendirme numaraları.                  |
| SWIFT kodu                 | Ödeme yönergesi bilgileri için SWIFT kodları.                           |
| Kredi Kartı                | Kredi kartı numaraları.                                                       |
| IBAN kodu                  | Ödeme yönergesi bilgileri için ıBAN kodları.                            |

#### <a name="government-and-country-specific-identification"></a>Kamu ve ülkeye özgü kimlik

Aşağıdaki varlıklar, ülkeye göre gruplandırılır ve listelenir:

Arjantin
* Ulusal kimlik (DNı) numarası

Avustralya
* Vergi dosya numarası 
* Sürücünün lisans KIMLIĞI
* Passport KIMLIĞI
* Tıbbi hesap numarası
* Banka hesap numaraları (örneğin, denetim, tasarruf ve borç hesapları)

Belçika
* Ulusal numara

Brezilya
* Yasal varlık numarası (CNPJ)
* CPF numarası
* Ulusal KIMLIK kartı (RG)

Kanada
* Passport KIMLIĞI
* Sürücünün lisans KIMLIĞI
* Sağlık sigortası numarası
* Kişisel sağlık KIMLIĞI numarası (PHIN)
* Sosyal güvenlik numarası
* Banka hesap numaraları (örneğin, denetim, tasarruf ve borç hesapları)

Şili
* Kimlik kartı numarası 

Çin
* Kimlik kartı numarası
* Yerleşik KIMLIK kartı (ÇHC) numarası

Hırvatistan
* KIMLIK kartı numarası
* Kişisel KIMLIK (OıB) numarası

Çek Cumhuriyeti
* Ulusal KIMLIK kartı numarası

Danimarka
* Kişisel KIMLIK numarası

Avrupa Birliği (AB)
* Ulusal KIMLIK numarası
* Passport KIMLIĞI
* Sürücünün lisans KIMLIĞI
* Sosyal güvenlik numarası (SSN) veya eşdeğer KIMLIK
* AB vergi kimlik numarası (TIN)
* AB banka kartı numarası

Finlandiya
* Ulusal KIMLIK numarası
* Passport KIMLIĞI

Fransa
* Ulusal KIMLIK kartı (CNı)
* Sosyal güvenlik numarası (ıNSEE)
* Passport KIMLIĞI
* Sürücünün lisans KIMLIĞI

Almanya
* KIMLIK kartı numarası
* Passport KIMLIĞI
* Sürücünün lisans KIMLIĞI

Yunanistan 
* Ulusal KIMLIK kartı numarası

Hong Kong
* KIMLIK kartı (HKıD) numarası

Hindistan
* Kalıcı hesap numarası (PAN)
* Benzersiz KIMLIK (aadhaar) numarası

Endonezya
* KIMLIK kartı numarası (KTP)

İrlanda
* Kişisel genel hizmet (PPS) numarası

İsrail
* Ulusal KIMLIK
* Banka hesap numaraları (örneğin, denetim, tasarruf ve borç hesapları)

İtalya
* Sürücünün lisans KIMLIĞI

Japonya
* Yerleşik kayıt numarası
* İkamet kartı numarası
* Sürücünün lisans KIMLIĞI
* Sosyal sigorta numarası (SIN)
* Passport KIMLIĞI
* Banka hesap numaraları (örneğin, denetim, tasarruf ve borç hesapları)

Malezya
* KIMLIK kartı numarası

Hollanda
* Vatandaşlık hizmeti (BSN) numarası

Yeni Zelanda
* Sağlık numarası için minbakanlığı

Norveç
* KIMLIK kartı numarası

Filipinler
* Birleşik çok amaçlı KIMLIK numarası

Polonya
* KIMLIK kartı numarası
* Ulusal KIMLIK (PESEL)
* Passport KIMLIĞI

Portekiz 
* Vatandaşlık kartı numarası

Suudi Arabistan
* Ulusal KIMLIK

Singapur
* Ulusal kayıt KIMLIĞI kartı (NRIC) numarası

Güney Afrika
* KIMLIK numarası
* Yerleşik kayıt numarası

Güney Kore
* Yerleşik kayıt numarası

İspanya 
* Sosyal güvenlik numarası (SSN)

İsveç
* Ulusal KIMLIK
* Passport KIMLIĞI

Tayvan 
* Ulusal KIMLIK
* Yerleşik sertifika (yay/TARC) numarası
* Passport KIMLIĞI

Tayland
* Popülasyon kimlik kodu

Birleşik Krallık
* Passport KIMLIĞI
* Sürücünün lisans KIMLIĞI
* Ulusal sigorta numarası (NINO)
* Ulusal Sistem Sağlığı Hizmeti (NHS) numarası

Amerika Birleşik Devletleri
* Sosyal güvenlik numarası (SSN)
* Sürücünün lisans KIMLIĞI
* Passport KIMLIĞI
* Elektrotoral toplama numarası
* Bireysel vergi KIMLIK numarası (ITIN)
* İlaç zorlama Kurumu (DEA) numarası
* Banka hesap numaraları (örneğin, denetim, tasarruf ve borç hesapları)
