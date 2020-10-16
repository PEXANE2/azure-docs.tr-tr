---
title: Bölge kullanılabilirliği ve veri yerleşikliği
titleSuffix: Azure AD B2C
description: Bölge kullanılabilirliği, veri yerleşimi ve Azure Active Directory B2C Preview kiracılar hakkında bilgiler.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 06/06/2020
ms.author: mimart
ms.subservice: B2C
ms.custom: references_regions
ms.openlocfilehash: f8c6f7daecd38babaa4f2961d04a6cd4c3b4dbed
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91840566"
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a>Azure Active Directory B2C: bölge kullanılabilirliği & veri yerleşimi

Bölge kullanılabilirliği ve veri yerleşimi, Azure 'un geri kalanından Azure AD B2C farklı şekilde uygulanan iki çok farklı kavramlardır. Bu makalede, bu iki kavram arasındaki farklar açıklanmakta ve bunların Azure için nasıl uygulandığı karşılaştırılır Azure AD B2C karşılaştırması açıklanmıştır.

Azure AD B2C, **Dünya çapında** **Birleşik Devletler, Avrupa veya Asya Pasifik** **veri** yerleşimi seçeneği ile birlikte kullanılabilir.

[Bölge kullanılabilirliği](#region-availability) , bir hizmetin kullanıma hazır olduğu yeri belirtir.

[Veri](#data-residency) yerleşimi, Kullanıcı verilerinin nerede depolanabileceğini belirtir.

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure AD B2C Azure genel bulutu aracılığıyla dünya çapında kullanılabilir.

Bu, modelden ve sonrasında, genellikle *veri*yerleşimi ile birkaç *kullanılabilirlik* sağlayan diğer Azure hizmetlerinden farklıdır. Bunun örneklerini [bölgeye göre Azure ürünlerinin](https://azure.microsoft.com/regions/services/) yanı sıra [Active Directory B2C Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/details/active-directory-b2c/)' da görebilirsiniz.

## <a name="data-residency"></a>Veri yerleşimi

Azure AD B2C, Kullanıcı verilerini Birleşik Devletler, Avrupa veya Asya Pasifik bölgesinde depolar.

Veri yerleşimi, [Azure AD B2C kiracı oluştururken](tutorial-create-tenant.md)seçtiğiniz ülke/bölge tarafından belirlenir:

![Bir kiracı oluştur formunun, ülke veya bölge seçme ekran görüntüsü.](./media/data-residency/data-residency-b2c-tenant.png)

Veriler aşağıdaki ülkeler/bölgeler için **Birleşik Devletler** bulunur:

> Birleşik Devletler (ABD), Kanada (CA), Kosta Rika (CR), Dominik Cumhuriyeti (DO), El Salvador (ZF), Guatemala (GT), Meksika (MX), Panama (PA), Porto Riko (PR) ve Trinidad & Tobago (TT)

Veriler aşağıdaki ülkeler/bölgeler için **Avrupa** 'da bulunur:

> Cezayir (DZ), Avusturya (AT), Azerbaycan (az), Bahreyn (BH), Belarus (BY), Belçika (BE), Bulgaristan (BG), Hırvatistan (HR), Kıbrıs (CY), Çek Cumhuriyeti (CZ), Danimarka (DK), Mısır (örn., Finlandiya (EE), Finlandiya (FT), Fransa (FR), Almanya (DE), Yunanistan (GR), Macaristan (HU), Voiceland (yanı), Irlanda (IE), Israil (IL), Italya (It), Ürdün (JO), Kazakistan (KZ), Kenya (KE), Kuveyt (KW), Litvanya (LV), Lübnan (LB), Liechtenstein (LI), Litvanya (LT) , Lüksemburg (LU), Kuzey Makedonya (ML), Malta (MT), Karadağ (ME), Fas (MA), Hollanda (NL), Nijerya (&), Norveç (NO), Umman (OM), Pakistan (PK), Polonya (PL), Portekiz (PT), Qtor (QA), Romanya (RO), Rusya (RU), Suudi Arabistan (SA), Sırbistan (RS), Slovakya (SK), Slovenya (ST), Güney Afrika (ZA), Ispanya (ES), Isveç (VAG), Isviçre (CH), Tunus (TN), Türkiye (TR), Ukrayna (UA), Birleşik Arap Emirlikleri (AE) ve Birleşik Krallık (GB)

Veriler aşağıdaki ülkeler/bölgeler için **Asya Pasifik** bulunur:

> Afganistan (AF), Hong Kong ÖIB (HK), Hindistan (ın), Endonezya (ID), Japonya (JP), Kore (KR), Malezya (MY), Filipinler (PH), Singapur (SG), Sri Lanka (LK), Tayvan (TW) ve Tayland (TH).

Aşağıdaki ülkeler/bölgeler listeye eklenmekte. Şimdilik, Yukarıdaki ülkelerin/bölgelerin birini seçerek Azure AD B2C kullanmaya devam edebilirsiniz.

> Arjantin, Avustralya, Brezilya, Şili, Kolombiya, Ekvador, Irak, Yeni Zelanda, Paraguay, Peru, Uruguay ve Venezuela.

## <a name="preview-tenant"></a>Önizleme kiracısı

Azure AD B2C's önizleme döneminde bir B2C kiracısı oluşturduysanız, büyük olasılıkla **kiracı türü** **Önizleme kiracısı**yazıyor.

Bu durumda, kiracınızı yalnızca geliştirme ve test amaçlarıyla kullanmanız gerekir. Üretim uygulamaları için bir önizleme kiracısı kullanmayın.

Önizleme B2C kiracısından üretim ölçeğinde B2C kiracısına **geçiş yolu** yoktur. Üretim uygulamalarınız için yeni bir B2C kiracısı oluşturmanız gerekir.

Bir önizleme B2C kiracısının silineceği ve aynı etki alanı adıyla bir üretim ölçeğinde B2C kiracısı oluşturabileceğiniz bilinen sorunlar vardır. *Farklı bir etki alanı adına sahip bir üretim ÖLÇEĞINDE B2C kiracısı oluşturmanız gerekir*.

![Bir kiracı türünün önizleme kiracısı olarak ekran görüntüsü.](./media/data-residency/preview-b2c-tenant.png)