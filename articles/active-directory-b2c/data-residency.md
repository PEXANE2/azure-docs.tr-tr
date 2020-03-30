---
title: Bölge kullanılabilirliği ve veri ikamet
titleSuffix: Azure AD B2C
description: Azure Etkin Dizin Ii B2C önizleme kiracıları hakkında bölge kullanılabilirliği, veri ikametve bilgiler.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/26/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 3df0f581d0d2a1e5ca02202b4eeaede5a1dd5362
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188860"
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a>Azure Active Directory B2C: Bölge kullanılabilirliği & veri ihtisası

Bölge kullanılabilirliği ve veri ikamet, Azure'un geri kalanından Azure AD B2C'ye farklı şekilde uygulanan çok farklı iki kavramdır. Bu makalede, bu iki kavram arasındaki farklar açıklanmaktadır ve Azure ile Azure AD B2C'ye nasıl uygulandıklarını karşılaştırın.

Azure AD B2C genellikle dünya çapında **abd, Avrupa veya Asya Pasifik'te** **veri ikamet** seçeneği ile **kullanılabilir.**

[Bölge kullanılabilirliği,](#region-availability) bir hizmetin kullanıma sunulduğu yeranlamına gelir.

[Veri ihtisaskullanıcı](#data-residency) verilerinin depolandığı yeri ifade eder.

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure AD B2C, dünya çapında Azure genel bulutu üzerinden kullanılabilir.

Bu, genellikle *veri ihtisas*ile çift *kullanılabilirlik* diğer azure hizmetleri, ardından model farklıdır. Bunun örneklerini hem Azure'un [Bölgelere Göre Kullanılabilir Ürünleri](https://azure.microsoft.com/regions/services/) sayfasında hem de [Active Directory B2C fiyatlandırma hesaplayıcısında](https://azure.microsoft.com/pricing/details/active-directory-b2c/)görebilirsiniz.

## <a name="data-residency"></a>Veri yerleşimi

Azure AD B2C kullanıcı verilerini ABD, Avrupa veya Asya Pasifik bölgesinde depolar.

Veri ihtisası, Azure AD [B2C kiracısı oluşturduğunuzda](tutorial-create-tenant.md)seçtiğiniz ülke/bölge tarafından belirlenir:

![Önizleme kiracısının ekran görüntüsü](./media/data-residency/data-residency-b2c-tenant.png)

Veriler, aşağıdaki ülkeler/bölgeler için **ABD'de** bulunur:

> Amerika Birleşik Devletleri, Kanada, Kosta Rika, Dominik Cumhuriyeti, El Salvador, Guatemala, Meksika, Panama, Porto Riko ve Trinidad tobago &

Veriler aşağıdaki ülkeler/bölgeler için **Avrupa'da** bulunur:

> Cezayir, Avusturya, Azerbaycan, Bahreyn, Beyaz Rusya, Belçika, Bulgaristan, Hırvatistan, Kıbrıs, Çek Cumhuriyeti, Danimarka, Mısır, Estonya, Finlandiya, Fransa, Almanya, Yunanistan, Macaristan, İzlanda, İrlanda, İsrail, İtalya, Ürdün, Kazakistan, Kenya, Kuveyt, Letonya, Lübnan, Lihtenştayn, Litvanya, Lüksemburg, Kuzey Makedonya, Malta, Karadağ, Fas, Hollanda, Nijerya, Norveç, Umman, Pakistan, Polonya, Portekiz, Katar, Romanya, Rusya, Suudi Arabistan, Sırbistan, Slovakya, Slovenya, Güney Afrika, İspanya, İsveç, İsviçre, Tunus, Türkiye, Ukrayna, Birleşik Arap Emirlikleri ve Birleşik Krallık.

Veriler aşağıdaki ülkeler/bölgeler için **Asya Pasifik'te** bulunur:

> Afganistan, Hong Kong SAR, Hindistan, Endonezya, Japonya, Kore, Malezya, Filipinler, Singapur, Sri Lanka, Tayvan ve Tayland.

Aşağıdaki ülkeler/bölgeler listeye eklenme sürecindedir. Şimdilik, yukarıdaki ülkelerden/bölgelerden herhangi birini seçerek Azure AD B2C'yi kullanmaya devam edebilirsiniz.

> Arjantin, Avustralya, Brezilya, Şili, Kolombiya, Ekvador, Irak, Yeni Zelanda, Paraguay, Peru, Uruguay ve Venezuela.

## <a name="preview-tenant"></a>Önizleme kiracı

Azure AD B2C'nin önizleme döneminde bir B2C kiracısı oluşturduysanız, **kiracı türünüzün** **önizleme kiracısı**dediği olasıdır.

Bu durumda, yalnızca geliştirme ve sınama amacıyla kiracınızı kullanmanız gerekir. Üretim uygulamaları için önizleme kiracı kullanmayın.

Önizleme B2C kiracısından üretim ölçeğinde bir B2C kiracısına **geçiş yolu yoktur.** Üretim uygulamalarınız için yeni bir B2C kiracı oluşturmanız gerekir.

Bir önizleme B2C kiracı silmek ve aynı etki alanı adı ile bir üretim ölçekli B2C kiracı oluşturmak bilinen sorunlar vardır. *Farklı bir etki alanı adı olan üretim ölçeğinde bir B2C kiracı oluşturmanız gerekir.*

![Önizleme kiracısının ekran görüntüsü](./media/data-residency/preview-b2c-tenant.png)