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
ms.openlocfilehash: 8e0798f75aaa79031ca7cc03814282daa049fbfe
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89662875"
---
Bu varlık kategorisi, mali bilgileri ve resmi tanımlama biçimlerini içerir. Model sürümünden itibaren kullanılabilir `2019-10-01` . Alt türler aşağıda listelenmiştir. 

### <a name="financial-account-identification"></a>Finansal hesap kimliği

| Alt tür adı               | Description                                                                |
|----------------------------|----------------------------------------------------------------------------|
| ABA Yönlendirme Numarası        | American banker Association (ABA) transit yönlendirme numaraları.                  |
| SWIFT Kodu                 | Ödeme yönergesi bilgileri için SWIFT kodları.                           |
| Kredi Kartı                | Kredi kartı numaraları.                                                       |
| Uluslararası Bankacılık Hesap Numarası (IBAN)                  | Ödeme yönergesi bilgileri için ıBAN kodları.                            |


### <a name="government-and-countryregion-specific-identification"></a>Kamu ve ülkeye/bölgeye özgü kimlik

> [!NOTE]
> Aşağıdaki mali ve ülkeye özgü varlıklar `domain=phi` parametresiyle döndürülmez:
> * Passport numaraları
> * Vergi kimlikleri

Aşağıdaki varlıklar, ülkeye göre gruplandırılır ve listelenir:

Arjantin
* Arjantin Ulusal kimlik (DNı) numarası

Avusturya
* Avusturya kimlik kartı
* Avusturya vergi kimlik numarası
* Avusturya değeri eklenen vergi (KDV) numarası

Avustralya
* Avustralya banka hesap numarası
* Avustralya Iş numarası
* Avustralya şirket numarası
* Avustralya sürücüsünün lisans numarası
* Avustralya tıbbi hesap numarası
* Avustralya Passport numarası
* Avustralya Vergi dosya numarası

Belçika
* Belçika Ulusal numarası
* Belçika katma değer vergi numarası

Brezilya 
* Brezilya legal varlık numarası (CNPJ)
* Brezilya CPF numarası
* Brezilya Ulusal KIMLIK kartı (RG)

Bulgaristan
* Bulgaristan uniform kamu numarası

Kanada
* Kanada banka hesap numarası
* Kanada sürücüsünün lisans numarası
* Kanada Sistem Sağlığı Hizmeti numarası
* Kanada Passport numarası
* Kanada Kişisel sağlık kimlik numarası (PHIN)
* Kanada Sosyal sigorta numarası

Şili
* Kimlik kartı numarası 

Çin
* Çin yerleşik kimlik kartı (ÇHC) numarası

Hırvatistan
* Hırvatistan kimlik kartı numarası
* Hırvatistan Ulusal KIMLIK kartı numarası
* Hırvatistan kişisel kimlik (OıB) numarası

Kıbrıs
* Kıbrıs kimlik kartı numarası
* Kıbrıs vergi kimlik numarası

Çek Cumhuriyeti
* Çek kişisel kimlik numarası

Danimarka
* Danimarka kişisel kimlik numarası

Estonya
* Estonya kişisel kimlik kodu

Avrupa Birliği (AB)
* AB banka kartı numarası
* AB sürücüsünün lisans numarası
* AB Ulusal kimlik numarası
* AB Passport numarası
* AB sosyal güvenlik numarası (SSN) veya eşdeğer KIMLIK
* AB vergi kimlik numarası (TIN)

Finlandiya
* Finlandiya Avrupa sağlık sigortası numarası
* Finlandiya Ulusal KIMLIĞI
* Finlandiya Passport numarası

Fransa
* Fransa sürücüsünün lisans numarası
* Fransa sağlık sigortası numarası
* Fransa Ulusal KIMLIK kartı (CNı)
* Fransa Passport numarası
* Fransa sosyal güvenlik numarası (ıNSEE)
* Fransa vergi kimlik numarası (Numéro SPI)
* Fransa değeri eklenen vergi numarası

Almanya
* Almanya sürücüsünün lisans numarası
* Almanya kimlik kartı numarası
* Almanya Passport numarası
* Almanya vergi kimlik numarası
* Almanya katma değer vergi numarası

Yunanistan 
* Yunanistan Ulusal KIMLIK kartı numarası
* Yunanistan vergi kimlik numarası

Hong Kong
* Hong Kong kimlik kartı (HKıD) numarası

Macaristan
* Macaristan Ulusal kimlik numarası
* Macaristan vergi kimlik numarası
* Macaristan değeri eklenen vergi numarası

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
* İtalya mali kodu
* İtalya değeri eklenen vergi numarası

Japonya
* Japon banka hesap numarası
* Japon sürücüsünün lisans numarası
* Japon kişisel numarası
* Japon numarası kurumsal
* Japonya yerleşik kayıt numarası
* Japon konut kartı numarası
* Japonya sosyal sigorta numarası (SIN)
* Japonya Passport numarası

Letonya
* Letonya kişisel kodu

Litvanya
* Litvanya kişisel kodu

Luxemburg
* Luxemburg ulusal kimlik numarası (doğal kişiler)
* Luxemburg ulusal kimlik numarası (doğal olmayan kişiler)

Malezya
* Malezya kimlik kartı numarası

Malta
* Malta kimlik kartı numarası
* Malta vergi kimlik numarası

Hollanda
* Hollanda vatandaşlık hizmeti (BSN) numarası
* Hollanda vergi kimlik numarası
* Hollanda katma değer vergi numarası

Yeni Zelanda
* Yeni Zelanda banka hesap numarası
* Yeni Zelanda sürücüsünün lisans numarası
* Yeni Zelanda Inland gelir numarası
* Yeni Zelanda sağlık numarası Bakanlığı
* Yeni Zelanda sosyal Welfare numarası

Norveç
* Norveç kimlik numarası

Filipinler
* Filipinler Birleşik çok amaçlı KIMLIK numarası

Polonya
* Polonya kimlik kartı
* Polonya Ulusal KIMLIĞI (PESEL)
* Polonya Passport numarası
* Polonya REGON numarası
* Polonya vergi kimlik numarası

Portekiz 
* Portekiz vatandaşlık kart numarası
* Portekiz vergi kimlik numarası

Romanya
* Romanya kişisel sayısal kodu (CNP)

Rusya
* Rusça Passport numarası (Yurtiçi)
* Rusça Passport numarası (Uluslararası)

Suudi Arabistan
* Suudi Arabistan Ulusal KIMLIĞI

Singapur
* Singapur Ulusal kayıt KIMLIĞI kartı (NRIC) numarası

Slovakya 
* Slovakya kişisel numarası

Slovenya
* Slovenya vergi kimlik numarası
* Slovenya benzersiz ana Vatandaşlık numarası

Güney Afrika
* Güney Afrika kimlik numarası

Güney Kore
* Güney Kore yerleşik kayıt numarası

İspanya 
* İspanya DNı dili
* İspanya sosyal güvenlik numarası (SSN)
* İspanya vergi kimlik numarası

İsveç
* İsveç Ulusal KIMLIĞI
* İsveç Passport numarası
* İsveç vergi kimlik numarası

İsviçre
* İsviçre sosyal güvenlik numarası AHV

Tayvan 
* Tayvan Ulusal KIMLIĞI
* Tayvan yerleşik sertifikası (yay/TARC)
* Tayvan Passport numarası

Tayland
* Tay dili popülasyon kimlik kodu

Türkiye
* Türkçe Ulusal kimlik numarası

Ukrayna
* Ukrayna Passport numarası (Yurtiçi)
* Ukrayna Passport numarası (Uluslararası)

Birleşik Krallık
* Krallık Sürücünün lisans numarası
* Krallık Elektrotoral toplama numarası
* Krallık Ulusal Sistem Sağlığı Hizmeti (NHS) numarası
* Krallık Ulusal sigorta numarası (NINO)
* Krallık Passport numarası
* Krallık Benzersiz vergi mükellefi başvuru numarası

Birleşik Devletler
* ABD sosyal güvenlik numarası (SSN)
* ABD sürücüsünün lisans numarası
* ABD Passport numarası
* ABD bireysel vergi mükellefi kimlik numarası (ITIN)
* ABD Ilaç zorlama Kurumu (DEA) numarası
* ABD banka hesap numarası
