---
title: Özel alt etki alanları
titleSuffix: Azure Cognitive Services
description: Her Bilişsel Hizmet kaynağı için özel alt alan adları Azure portalı, Azure Bulut Kabuğu veya Azure CLI üzerinden oluşturulur.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 07/24/2019
ms.author: erhopf
ms.openlocfilehash: 108e5ebfa34db711af4ff33cbf4fbba897b8947d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73647687"
---
# <a name="custom-subdomain-names-for-cognitive-services"></a>Bilişsel Hizmetler için özel alt alan adları

Azure Bilişsel Hizmetleri, [Azure portalı](https://portal.azure.com), Azure Bulut [Bulutu](https://azure.microsoft.com/features/cloud-shell/)veya [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)aracılığıyla oluşturulan her kaynak için özel alt alan adları kullanır. Belirli bir Azure bölgesindeki tüm müşteriler için yaygın olan bölgesel uç noktaların aksine, özel alt etki alanı adları kaynağa özgüdir. Kimlik doğrulaması için Azure Active Directory (Azure AD) gibi özellikleri etkinleştirmek için özel alt alan adları gereklidir.

## <a name="how-does-this-impact-existing-resources"></a>Bu, varolan kaynakları nasıl etkiler?

1 Temmuz 2019'dan önce oluşturulan Bilişsel Hizmetler kaynakları, ilgili hizmet için bölgesel uç noktaları kullanacaktır. Bu uç noktalar varolan ve yeni kaynaklarla çalışır.

Azure AD gibi özellikleri etkinleştirmek için varolan bir kaynağı özel alt alan adlarını etkinleştirmek için geçirmek istiyorsanız aşağıdaki yönergeleri izleyin:

1. Azure portalında oturum açın ve özel bir alt etki alanı adı eklemek istediğiniz Bilişsel Hizmetler kaynağını bulun.
2. Genel **Bakış** bıçağında, **Özel Alan Adı Oluştur'u**bulun ve seçin.
3. Bu, kaynağınız için benzersiz bir özel alt etki alanı oluşturmak için yönergeleri içeren bir panel açar.
   > [!WARNING]
   > Özel bir alt etki alanı adı oluşturduktan sonra **değiştirilemez.**

## <a name="do-i-need-to-update-my-existing-resources"></a>Varolan kaynaklarımı güncelleştirmem gerekiyor mu?

Hayır. Bölgesel bitiş noktası yeni ve varolan Bilişsel Hizmetler için çalışmaya devam edecektir ve özel alt etki alanı adı isteğe bağlıdır. Özel bir alt etki alanı adı eklense bile bölgesel bitiş noktası kaynakla çalışmaya devam edecektir.

## <a name="what-if-an-sdk-asks-me-for-the-region-for-a-resource"></a>Ya bir SDK benden kaynak isterse?

> [!WARNING]
> Konuşma Hizmetleri şu anda özel alt etki alanlarını **desteklemez.** Konuşma Hizmetlerini ve ilişkili SDK'ları kullanırken lütfen bölgesel uç noktaları kullanın.

Bölgesel uç noktalar ve özel alt alan adları hem desteklenir hem de birbirinin yerine kullanılabilir. Ancak, tam bitiş noktası gereklidir.

Azure [portalındaki](https://portal.azure.com)kaynağınız için **Genel Bakış** bıçağında bölge bilgileri mevcuttur. Bölgesel uç noktaların tam listesi için [bkz.](#is-there-a-list-of-regional-endpoints)

## <a name="are-custom-subdomain-names-regional"></a>Özel alt alan adları bölgesel mi?

Evet. Özel bir alt etki alanı adı kullanmak, Bilişsel Hizmetler kaynağınızın bölgesel yönlerini değiştirmez.

## <a name="what-are-the-requirements-for-a-custom-subdomain-name"></a>Özel bir alt etki alanı adı için gereksinimler nelerdir?

Özel bir alt etki alanı adı kaynağınıza özgüdür. Ad yalnızca alfanümerik karakterleri ve `-` karakteri içerebilir; uzunluğu 2 ile 64 karakter arasında olmalıdır ve `-`bir .

## <a name="can-i-change-a-custom-domain-name"></a>Özel bir etki alanı adını değiştirebilir miyim?

Hayır. Özel bir alt etki alanı adı oluşturulduktan ve bir kaynakla ilişkilendirildikten sonra değiştirilemez.

## <a name="can-i-reuse-a-custom-domain-name"></a>Özel bir etki alanı adını yeniden kullanabilir miyim?

Her özel alt etki alanı adı benzersizdir, bu nedenle Bilişsel Hizmetler kaynağına atadığınız özel bir alt etki alanı adını yeniden kullanmak için varolan kaynağı silmeniz gerekir. Kaynak silindikten sonra, özel alt etki alanı adını yeniden kullanabilirsiniz.

## <a name="is-there-a-list-of-regional-endpoints"></a>Bölgesel bitiş noktalarının bir listesi var mı?

Evet. Bu, Azure Bilişsel Hizmetler kaynaklarıyla kullanabileceğiniz bölgesel uç noktaların listesidir.

> [!NOTE]
> Çevirmen Metin API'si ve Bing Arama API'leri genel uç noktaları kullanır.

| Uç nokta türü | Bölge | Uç Nokta |
|---------------|--------|----------|
| Genel | Global (Çevirmen Metin & Bing) | `https://api.cognitive.microsoft.com` |
| | Doğu Avustralya | `https://australiaeast.api.cognitive.microsoft.com` |
| | Güney Brezilya | `https://brazilsouth.api.cognitive.microsoft.com` |
| | Orta Kanada | `https://canadacentral.api.cognitive.microsoft.com` |
| | Orta ABD | `https://centralus.api.cognitive.microsoft.com` |
| | Doğu Asya | `https://eastasia.api.cognitive.microsoft.com` |
| | Doğu ABD | `https://eastus.api.cognitive.microsoft.com` |
| | Doğu ABD 2 | `https://eastus2.api.cognitive.microsoft.com` |
| | Orta Fransa | `https://francecentral.api.cognitive.microsoft.com` |
| | Hindistan Orta | `https://centralindia.api.cognitive.microsoft.com` |
| | Doğu Japonya | `https://japaneast.api.cognitive.microsoft.com` |
| | Güney Kore - Orta | `https://koreacentral.api.cognitive.microsoft.com` |
| | Orta Kuzey ABD | `https://northcentralus.api.cognitive.microsoft.com` |
| | Kuzey Avrupa | `https://northeurope.api.cognitive.microsoft.com` |
| | Güney Afrika Kuzey | `https://southafricanorth.api.cognitive.microsoft.com` |
| | Orta Güney ABD | `https://southcentralus.api.cognitive.microsoft.com` |
| | Güneydoğu Asya | `https://southeastasia.api.cognitive.microsoft.com` |
| | Güney Birleşik Krallık | `https://uksouth.api.cognitive.microsoft.com` |
| | Orta Batı ABD | `https://westcentralus.api.cognitive.microsoft.com` |
| | Batı Avrupa | `https://westeurope.api.cognitive.microsoft.com` |
| | Batı ABD | `https://westus.api.cognitive.microsoft.com` |
| | Batı ABD 2 | `https://westus2.api.cognitive.microsoft.com` |
| US Gov | US Gov Virginia | `https://virginia.api.cognitive.microsoft.us` |
| Çin | Çin Doğu 2 | `https://chinaeast2.api.cognitive.azure.cn` |
| | Kuzey Çin | `https://chinanorth.api.cognitive.azure.cn` |

## <a name="see-also"></a>Ayrıca bkz.

* [Bilişsel Hizmetler nelerdir?](Welcome.md)
* [Kimlik doğrulaması](authentication.md)
