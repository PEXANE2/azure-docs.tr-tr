---
title: Kimlik varlıkları
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/29/2020
ms.author: aahi
ms.openlocfilehash: f07b71bf8996612798b87d32a21a15ec72db0b32
ms.sourcegitcommit: f0b206a6c6d51af096a4dc6887553d3de908abf3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140963"
---
Bu varlık kategorisi, mali bilgileri ve resmi tanımlama biçimlerini içerir. Model sürümünden itibaren kullanılabilir `2019-10-01` . Alt türler aşağıda listelenmiştir. 

### <a name="financial-account-identification"></a>Finansal hesap kimliği

| Alt tür adı               | Açıklama                                                                |
|----------------------------|----------------------------------------------------------------------------|
| ABA Yönlendirme Numarası        | American banker Association (ABA) transit yönlendirme numaraları.                  |
| SWIFT Kodu                 | Ödeme yönergesi bilgileri için SWIFT kodları.                           |
| Kredi Kartı                | Kredi kartı numaraları.                                                       |
| Uluslararası Bankacılık Hesap Numarası (IBAN)                  | Ödeme yönergesi bilgileri için ıBAN kodları.                            |


### <a name="government-and-countryregion-specific-identification"></a>Devlet ve ülkeye/bölgeye özgü kimlik

> [!NOTE]
> Aşağıdaki mali ve ülkeye özgü varlıklar `domain=phi` parametresiyle döndürülmez:
> * Passport numaraları
> * Vergi kimlikleri

Aşağıdaki varlıklar, ülkeye göre gruplandırılır ve listelenir:

Arjantin
* Arjantin Ulusal kimlik (DNı) numarası

Avustralya
* Avustralya Passport numarası
* Avustralya Vergi dosya numarası
* Avustralya sürücüsünün lisans numarası
* Avustralya tıbbi hesap numarası
* Avustralya banka hesap numarası

Belçika
* Belçika Ulusal numarası

Brezilya 
* Brezilya legal varlık numarası (CNPJ)
* Brezilya CPF numarası
* Brezilya Ulusal KIMLIK kartı (RG)

Kanada
* Kanada Sosyal sigorta numarası
* Kanada sürücüsünün lisans numarası
* Kanada banka hesap numarası
* Kanada Passport numarası
* Kanada Kişisel sağlık kimlik numarası (PHIN)
* Kanada Sistem Sağlığı Hizmeti numarası

Şili
* Kimlik kartı numarası 

Çin
* Çin yerleşik kimlik kartı (ÇHC) numarası

Hırvatistan
* Hırvatistan kimlik kartı numarası
* Hırvatistan kişisel kimlik (OıB) numarası

Çek Cumhuriyeti
* Çek kişisel kimlik numarası

Danimarka
* Danimarka kişisel kimlik numarası

Avrupa Birliği (AB)
* AB Ulusal kimlik numarası
* AB Passport numarası
* AB sürücüsünün lisans numarası
* AB sosyal güvenlik numarası (SSN) veya eşdeğer KIMLIK
* AB vergi kimlik numarası (TIN)
* AB banka kartı numarası

Finlandiya
* Finlandiya Ulusal KIMLIĞI
* Finlandiya Passport numarası

Fransa
* Fransa Ulusal KIMLIK kartı (CNı)
* Fransa sosyal güvenlik numarası (ıNSEE)
* Fransa Passport numarası
* Fransa sürücüsünün lisans numarası

Almanya
* Almanya kimlik kartı numarası
* Almanya Passport numarası
* Almanya sürücüsünün lisans numarası

Yunanistan 
* Yunanistan Ulusal KIMLIK kartı numarası

Hong Kong
* Hong Kong kimlik kartı (HKıD) numarası

Hindistan
* Hindistan kalıcı hesap numarası (PAN)
* Hindistan benzersiz tanımlama (aadhaar) numarası

Endonezya
* Endonezya kimlik kartı (KTP) numarası

İrlanda
* İrlanda Personal kamu hizmeti (PPS) numarası

İsrail
* İsrail Ulusal KIMLIĞI
* İsrail banka hesap numarası

İtalya
* İtalya sürücüsünün lisans KIMLIĞI

Japonya
* Japonya yerleşik kayıt numarası
* Japon konut kartı numarası
* Japon sürücüsünün lisans numarası
* Sosyal sigorta numarası (SIN)
* Japonya Passport numarası
* Japon banka hesap numarası

Malezya
* Malezya kimlik kartı numarası

Hollanda
* Hollanda vatandaşlık hizmeti (BSN) numarası

Yeni Zelanda
* Yeni Zelanda sağlık numarası Bakanlığı

Norveç
* Norveç kimlik numarası

Filipinler
* Filipinler Birleşik çok amaçlı KIMLIK numarası

Polonya
* Polonya kimlik kartı
* Polonya Ulusal KIMLIĞI (PESEL)
* Polonya Passport

Portekiz 
* Portekiz vatandaşlık kart numarası

Suudi Arabistan
* Suudi Arabistan Ulusal KIMLIĞI

Singapur
* Singapur Ulusal kayıt KIMLIĞI kartı (NRIC) numarası

Güney Afrika
* Güney Afrika kimlik numarası

Güney Kore
* Güney Kore yerleşik kayıt numarası

İspanya 
* İspanya sosyal güvenlik numarası (SSN)

İsveç
* İsveç Ulusal KIMLIĞI
* İsveç Passport numarası

Tayvan 
* Tayvan Ulusal KIMLIĞI
* Tayvan yerleşik sertifikası (yay/TARC)
* Tayvan Passport numarası

Tayland
* Tay dili popülasyon kimlik kodu

Birleşik Krallık
* Passport KIMLIĞI
* Krallık Sürücünün lisans numarası
* Krallık Ulusal sigorta numarası (NINO)
* Krallık Ulusal Sistem Sağlığı Hizmeti numarası
* Krallık Elektrotoral toplama numarası
* ABD/BIRLEŞIK KRALLıK Passport numarası

Birleşik Devletler
* ABD sosyal güvenlik numarası (SSN)
* ABD sürücüsünün lisans numarası
* ABD/BIRLEŞIK KRALLıK Passport numarası
* ABD bireysel vergi mükellefi kimlik numarası (ITIN)
* İlaç zorlama Kurumu (DEA) numarası
* ABD banka hesap numarası
