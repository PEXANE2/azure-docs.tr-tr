---
title: Kimlik varlıkları
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 02/17/2021
ms.author: aahi
ms.openlocfilehash: a376b050d79709885e3542d330bb6b1eea48d046
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750314"
---
### <a name="financial-account-identification"></a>Finansal hesap kimliği

Bu varlık kategorisi, mali bilgileri ve resmi tanımlama biçimlerini içerir.

#### <a name="category-aba-routing-number"></a>Kategori: ABA Yönlendirme numarası

Bu kategori aşağıdaki varlığı içerir:

:::row:::
    :::column span="":::
        **Varlık**

        ABA Yönlendirme numarası

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        American banker Association (ABA) transit yönlendirme numaraları.
      
    :::column-end:::
:::row-end:::

#### <a name="category-swift-code"></a>Kategori: SWIFT kodu

Bu kategori aşağıdaki varlığı içerir:

:::row:::
    :::column span="":::
        **Varlık**

        SWIFT kodu

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        Ödeme yönergesi bilgileri için SWIFT kodları.
      
    :::column-end:::
:::row-end:::

#### <a name="category-credit-card"></a>Kategori: kredi kartı

Bu kategori aşağıdaki varlığı içerir:

:::row:::
    :::column span="":::
        **Varlık**

        Kredi kartı

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        Kredi kartı numaraları. 
      
    :::column-end:::
:::row-end:::

#### <a name="category-international-banking-account-number-iban"></a>Kategori: uluslararası bankacılık hesap numarası (IBAN) 

Bu kategori aşağıdaki varlığı içerir:

:::row:::
    :::column span="":::
        **Varlık**

        Kredi kartı

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        Ödeme yönergesi bilgileri için ıBAN kodları.
      
    :::column-end:::
:::row-end:::

### <a name="government-and-countryregion-specific-identification"></a>Kamu ve ülkeye/bölgeye özgü kimlik

> [!NOTE]
> Aşağıdaki mali ve ülkeye özgü varlıklar `domain=phi` parametresiyle döndürülmez:
> * Passport numaraları
> * Vergi kimlikleri

Aşağıdaki varlıklar, ülkeye göre gruplandırılır ve listelenir:

#### <a name="argentina"></a>Arjantin

:::row:::
    :::column span="":::
        **Varlık**

        Arjantin Ulusal kimlik (DNı) numarası

    :::column-end:::
:::row-end:::


#### <a name="austria"></a>Avusturya

:::row:::
    :::column span="":::
        **Varlık**

        Avusturya kimlik kartı

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Avusturya vergi kimlik numarası

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Avusturya değeri eklenen vergi (KDV) numarası

    :::column-end:::
:::row-end:::



#### <a name="australia"></a>Avustralya

:::row:::
    :::column span="":::
        **Varlık**

        Avustralya banka hesap numarası

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Avustralya iş numarası

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Avustralya şirket numarası

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Avustralya sürücüsünün lisansı  

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Avustralya tıbbi hesap numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Avustralya Passport numarası

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Avustralya Passport numarası

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Avustralya Vergi dosya numarası

    :::column-end:::

:::row-end:::


#### <a name="belgium"></a>Belçika

:::row:::
    :::column span="":::
        **Varlık**

        Belçika Ulusal numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Belçika değer katma vergisi (KDV) numarası

    :::column-end:::

:::row-end:::


#### <a name="brazil"></a>Brezilya 

:::row:::
    :::column span="":::
        **Varlık**

        Brezilya legal varlık numarası (CNPJ)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Brezilya CPF numarası

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Brezilya Ulusal KIMLIK kartı (RG)

    :::column-end:::
:::row-end:::

#### <a name="canada"></a>Kanada

:::row:::
    :::column span="":::
        **Varlık**

        Kanada banka hesap numarası

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Kanada sürücüsünün lisans numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Kanada sağlık hizmeti numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Kanada Passport numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Kanada Kişisel sağlık kimlik numarası (PHIN)

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Kanada Sosyal sigorta numarası

    :::column-end:::
:::row-end:::

#### <a name="chile"></a>Şili 

:::row:::
    :::column span="":::
        **Varlık**

        Şili kimlik kartı numarası

    :::column-end:::
:::row-end:::

#### <a name="china"></a>Çin

:::row:::
    :::column span="":::
        **Varlık**

        Çin yerleşik kimlik kartı (ÇHC) numarası

    :::column-end:::
:::row-end:::


#### <a name="european-union-eu"></a>Avrupa Birliği (AB)

:::row:::
    :::column span="":::
        **Varlık**

        AB banka kartı numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        AB sürücüsünün lisans numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        AB Ulusal kimlik numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        AB Passport numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        AB sosyal güvenlik numarası (SSN) veya eşdeğer KIMLIK

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        AB vergi kimlik numarası (TIN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        AB GPS koordinatları

    :::column-end:::
:::row-end:::

#### <a name="france"></a>Fransa

:::row:::
    :::column span="":::
        **Varlık**

        Fransa sürücüsünün lisans numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Fransa sağlık sigortası numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Fransa Ulusal KIMLIK kartı (CNı)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Fransa Passport numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Fransa sosyal güvenlik numarası (ıNSEE)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Fransa vergi kimlik numarası (Numéro SPI)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Fransa değeri eklenen vergi (KDV) numarası

    :::column-end:::
:::row-end:::

#### <a name="germany"></a>Almanya

:::row:::
    :::column span="":::
        **Varlık**

        Almanya sürücüsünün lisans numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Almanya kimlik kartı numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Almanya Passport numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Almanya vergi kimlik numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Almanya katma değer vergi numarası

    :::column-end:::
:::row-end:::

#### <a name="hong-kong"></a>Hong Kong

:::row:::
    :::column span="":::
        **Varlık**

        Hong Kong kimlik kartı (HKıD) numarası

    :::column-end:::
:::row-end:::

#### <a name="hungary"></a>Macaristan

:::row:::
    :::column span="":::
        **Varlık**

        Macaristan kişisel kimlik numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Macaristan vergi kimlik numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Macaristan değeri eklenen vergi numarası

    :::column-end:::
:::row-end:::

#### <a name="india"></a>Hindistan

:::row:::
    :::column span="":::
        **Varlık**

        Hindistan kalıcı hesap numarası (PAN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Hindistan benzersiz tanımlama (aadhaar) numarası

    :::column-end:::
:::row-end:::


#### <a name="indonesia"></a>Endonezya

:::row:::
    :::column span="":::
        **Varlık**

        Endonezya kimlik kartı (KTP) numarası

    :::column-end:::
:::row-end:::

#### <a name="ireland"></a>İrlanda

:::row:::
    :::column span="":::
        **Varlık**

        İrlanda Personal kamu hizmeti (PPS) numarası

    :::column-end:::
:::row-end:::

#### <a name="israel"></a>İsrail

:::row:::
    :::column span="":::
        **Varlık**

        İsrail Ulusal KIMLIĞI

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        İsrail banka hesap numarası

    :::column-end:::
:::row-end:::

#### <a name="italy"></a>İtalya

:::row:::
    :::column span="":::
        **Varlık**

        İtalya sürücüsünün lisans KIMLIĞI

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        İtalya mali kodu

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        İtalya değeri eklenen vergi numarası

    :::column-end:::
:::row-end:::


#### <a name="japan"></a>Japonya

:::row:::
    :::column span="":::
        **Varlık**

        Japon banka hesap numarası

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Japon sürücüsünün lisans numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Japonya "benim numaranız" (kişisel)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Japonya "My Number" (Kurumsal)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Japonya yerleşik kayıt numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Japon konut kartı numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Japonya sosyal sigorta numarası (SIN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Japonya Passport numarası

    :::column-end:::
:::row-end:::

#### <a name="luxembourg"></a>Lüksemburg

:::row:::
    :::column span="":::
        **Varlık**

        Lüksemburg ulusal kimlik numarası (doğal kişiler)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Lüksemburg ulusal kimlik numarası (doğal olmayan kişiler)

    :::column-end:::
:::row-end:::

#### <a name="malta"></a>Malta

:::row:::
    :::column span="":::
        **Varlık**

        Malta kimlik kartı numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Malta vergi kimlik numarası

    :::column-end:::
:::row-end:::


#### <a name="new-zealand"></a>Yeni Zelanda

:::row:::
    :::column span="":::
        **Varlık**

        Yeni Zelanda banka hesap numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Yeni Zelanda sürücüsünün lisans numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Yeni Zelanda Inland gelir numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Yeni Zelanda sağlık numarası Bakanlığı

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Yeni Zelanda sosyal Welfare numarası

    :::column-end:::
:::row-end:::


#### <a name="philippines"></a>Filipinler

:::row:::
    :::column span="":::
        **Varlık**

        Filipinler Birleşik çok amaçlı KIMLIK numarası

    :::column-end:::
:::row-end:::

#### <a name="portugal"></a>Portekiz 

:::row:::
    :::column span="":::
        **Varlık**

        Portekiz vatandaşlık kart numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Portekiz vergi kimlik numarası

    :::column-end:::
:::row-end:::

#### <a name="singapore"></a>Singapur

:::row:::
    :::column span="":::
        **Varlık**

        Singapur Ulusal kayıt KIMLIĞI kartı (NRIC) numarası

    :::column-end:::
:::row-end:::


#### <a name="south-africa"></a>Güney Afrika

:::row:::
    :::column span="":::
        **Varlık**

        Güney Afrika kimlik numarası

    :::column-end:::
:::row-end:::


#### <a name="south-korea"></a>Güney Kore

:::row:::
    :::column span="":::
        **Varlık**

        Güney Kore yerleşik kayıt numarası

    :::column-end:::
:::row-end:::

#### <a name="spain"></a>İspanya

:::row:::
    :::column span="":::
        **Varlık**

        İspanya DNı dili

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        İspanya sosyal güvenlik numarası (SSN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        İspanya vergi kimlik numarası

    :::column-end:::
:::row-end:::
 
#### <a name="switzerland"></a>İsviçre

:::row:::
    :::column span="":::
        **Varlık**

        İsviçre sosyal güvenlik numarası AHV

    :::column-end:::
:::row-end:::


#### <a name="taiwan"></a>Tayvan 

:::row:::
    :::column span="":::
        **Varlık**

        Tayvan Ulusal KIMLIĞI

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Tayvan yerleşik sertifikası (yay/TARC)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Tayvan Passport numarası

    :::column-end:::
:::row-end:::

#### <a name="united-kingdom"></a>Birleşik Krallık

:::row:::
    :::column span="":::
        **Varlık**

        Krallık Sürücünün lisans numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Krallık Elektrotoral toplama numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Krallık Ulusal Sistem Sağlığı Hizmeti (NHS) numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Krallık Ulusal sigorta numarası (NINO)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       Krallık veya ABD Passport numarası

    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

       Krallık Benzersiz vergi mükellefi başvuru numarası

    :::column-end:::

:::row-end:::


#### <a name="united-states"></a>Birleşik Devletler

:::row:::
    :::column span="":::
        **Varlık**

        ABD sosyal güvenlik numarası (SSN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       ABD sürücüsünün lisans numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       ABD veya Birleşik Krallık Passport numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       ABD bireysel vergi mükellefi kimlik numarası (ITIN)

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       ABD Ilaç zorlama Kurumu (DEA) numarası

    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

       ABD banka hesap numarası

    :::column-end:::
:::row-end:::
