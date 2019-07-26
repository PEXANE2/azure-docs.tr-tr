---
title: Azure tüketim API 'sine genel bakış | Microsoft Docs
description: Azure tüketim API 'Lerinin Azure kaynaklarınızın maliyet ve kullanım verilerine programlı bir şekilde erişmesini nasıl sağladığını öğrenin.
services: billing
documentationcenter: ''
author: bandersmsft
manager: dougeby
editor: ''
tags: billing
ms.assetid: 68825e85-de64-466d-b11a-8baffde836b5
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 6/07/2018
ms.author: banders
ms.openlocfilehash: 2a2922daef5e75af41252119f9a467fae976bcda
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443268"
---
# <a name="azure-consumption-api-overview"></a>Azure tüketim API 'sine genel bakış 

Azure Tüketim API'leri, Azure kaynaklarınızla ilgili maliyet ve kullanım verilerinize program aracılığıyla erişmenizi sağlar. Bu API 'Ler Şu anda yalnızca kurumsal kayıtları ve web doğrudan aboneliklerini destekler (birkaç özel durum ile). API 'Ler, diğer Azure aboneliği türlerini desteklemek için sürekli olarak güncelleştirilir.

Azure Tüketim API'leri şu verilere erişim sunar:
- Kurumsal ve Doğrudan Web Müşterileri 
    - Kullanım Ayrıntıları 
    - Market Ücretleri 
    - Rezervasyon Önerileri 
    - Rezervasyon Ayrıntıları 
    - Rezervasyon Özetleri 
- Yalnızca Kurumsal Müşteriler 
    - Fiyat listesi 
    - Bütçeler 
    - Bakiyeler 

## <a name="usage-details-api"></a>Kullanım ayrıntıları API 'SI

Tüm Azure 1 parti kaynakları için ücret ve kullanım verilerini almak üzere kullanım ayrıntıları API 'sini kullanın. Bilgiler, şu anda her gün kaynak başına ölçüm başına yayılan kullanım ayrıntıları kayıtları biçimindedir. Bilgiler, tüm kaynakların maliyetlerini eklemek veya belirli kaynaklardaki maliyetleri/kullanımı araştırmak için kullanılabilir. 

API içerir:

-   **Ölçüm düzeyi tüketim verileri** -kullanım maliyeti, ücreti yayan ölçüm ve ücretle ilgili Azure kaynağı gibi verileri görüntüleyin. Tüm kullanım ayrıntıları kayıtları günlük bir sepete eşlenir.
-   **Azure rol tabanlı Access Control** -aboneliğin kullanım verilerine hangi kullanıcıların veya uygulamaların erişebileceğini belirtmek için [Azure Portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet 'lerinde](https://docs.microsoft.com/powershell/azure/overview) erişim ilkelerini yapılandırın. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin. 
-   **Filtreleme** -aşağıdaki FILTRELERI kullanarak API sonuç kümesini daha küçük bir kullanım ayrıntıları kaydı kümesine kırpın:
    - Kullanım bitiş/kullanım başlangıcı
    - Kaynak Grubu
    - Kaynak Adı
-   **Veri toplama** -kullanım ayrıntılarını etiketlere veya filtre özelliklerine göre toplamak üzere ifadeler uygulamak için OData kullanın
-   **Farklı teklif türleri Için kullanım** -kullanım ayrıntısı bilgileri şu anda kurumsal ve web doğrudan müşterileri için kullanılabilir.

Daha fazla bilgi için bkz. [kullanım ayrıntıları API 'si](https://docs.microsoft.com/rest/api/consumption/usagedetails)için teknik belirtim.

## <a name="marketplace-charges-api"></a>Market ücretleri API 'SI

Market ücretleri API 'sini kullanarak tüm Market kaynaklarında (Azure 3. parti teklifleri) ücret ve kullanım verilerini alın. Bu veriler, tüm Market kaynakları genelinde maliyetleri eklemek veya belirli kaynaklardaki maliyetleri/kullanımı araştırmak için kullanılabilir. 

API içerir:

-   **Ölçüm düzeyi tüketim verileri** -Market kullanım maliyeti, ücreti yayan ölçüm ve ücretle ilgili olan kaynakları içeren verilere bakın. Tüm kullanım ayrıntıları kayıtları günlük bir sepete eşlenir.
-   **Azure rol tabanlı Access Control** -aboneliğin kullanım verilerine hangi kullanıcıların veya uygulamaların erişebileceğini belirtmek için [Azure Portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet 'lerinde](https://docs.microsoft.com/powershell/azure/overview) erişim ilkelerini yapılandırın. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin. 
-   **Filtreleme** -aşağıdaki FILTRELERI kullanarak API sonuç kümesini daha küçük bir market kaydı kümesine kırpın:
    - Kullanım başlatma/kullanım bitişi
    - Kaynak Grubu
    - Kaynak Adı
-   **Farklı teklif türleri Için kullanım** -Market bilgileri, kurumsal ve Web Direct müşterileri için şu anda kullanılabilir.

Daha fazla bilgi için [Market ücretleri API 'sinin](https://docs.microsoft.com/rest/api/consumption/marketplaces)teknik belirtimine bakın.

## <a name="balances-api"></a>Dengeleme API 'SI

Kurumsal müşteriler, bakiyeler, yeni satın alma işlemleri, Azure Marketi hizmeti ücretleri, ayarlamalar ve fazla kullanım ücretleri hakkında bilgi almak için bakiye API 'sini kullanabilir. Bu bilgileri geçerli fatura dönemi veya geçmişteki herhangi bir dönem için alabilirsiniz. Kuruluşlar, el ile hesaplanan Özet ücretleriyle bir karşılaştırma gerçekleştirmek için bu verileri kullanabilir. Bu API kaynağa özgü bilgiler ve maliyetlerin toplam görünümünü sağlamaz. 

API içerir:

-   **Azure rol tabanlı Access Control** -aboneliğin kullanım verilerine hangi kullanıcıların veya uygulamaların erişebileceğini belirtmek için [Azure Portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet 'lerinde](https://docs.microsoft.com/powershell/azure/overview) erişim ilkelerini yapılandırın. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin. 
-   **Yalnızca kurumsal müşteriler** Bu API yalnızca kurumsal müşteriler için kullanılabilir. 
    - Müşteriler bu API 'YI çağırmak için Kuruluş Yöneticisi izinlerine sahip olmalıdır 

Daha fazla bilgi için bkz. [Bakiye API 'si](https://docs.microsoft.com/rest/api/consumption/balances)için teknik belirtim.

## <a name="budgets-api"></a>Bütçe API 'SI

Kurumsal müşteriler bu API 'yi kullanarak kaynaklar, kaynak grupları veya faturalandırma ölçümleri için maliyet veya kullanım bütçeleri oluşturabilir. Bu bilgiler belirlendikten sonra, uyarı Kullanıcı tanımlı bütçe eşikleri aşıldığında bildirimde bulunabilir şekilde yapılandırılabilir. 

API içerir:

-   **Azure rol tabanlı Access Control** -aboneliğin kullanım verilerine hangi kullanıcıların veya uygulamaların erişebileceğini belirtmek için [Azure Portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet 'lerinde](https://docs.microsoft.com/powershell/azure/overview) erişim ilkelerini yapılandırın. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin. 
-   **Yalnızca kurumsal müşteriler** -bu API yalnızca şirket içi müşteriler için geçerlidir.
-   **Yapılandırılabilir bildirimler** -bütçe tekrar eklendiğinde bildirilecek kullanıcıları belirtin.
-   **Kullanım veya maliyet tabanlı bütçeler** -senaryonuzu gereken tüketim veya maliyet temelinde bütçenize göre oluşturun.
-   **Filtreleme** -aşağıdaki yapılandırılabilir filtreleri kullanarak bütçenizi, kaynakların daha küçük bir alt kümesine filtreleyin
    - Kaynak Grubu
    - Kaynak Adı
    - Metre
-   **Yapılandırılabilir bütçe** dönemi-bütçenin ne sıklıkta sıfırlanacağını ve bütçenin ne kadar süreyle geçerli olduğunu belirtin.

Daha fazla bilgi için bkz. [Bütçe API 'si](https://docs.microsoft.com/rest/api/consumption/budgets)için teknik belirtim.

## <a name="reservation-recommendations-api"></a>Ayırma önerileri API 'SI

Ayrılmış VM örnekleri satın alma önerilerini almak için bu API 'yi kullanın. Öneriler, müşterilerin beklenen maliyet tasarrufunu ve satın alma tutarlarını çözümlemesine olanak sağlayacak şekilde tasarlanmıştır. 

API içerir:

-   **Azure rol tabanlı Access Control** -aboneliğin kullanım verilerine hangi kullanıcıların veya uygulamaların erişebileceğini belirtmek için [Azure Portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet 'lerinde](https://docs.microsoft.com/powershell/azure/overview) erişim ilkelerini yapılandırın. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin. 
-   **Filtreleme** -aşağıdaki filtreleri kullanarak öneri sonuçlarınızı uyarlayabilirsiniz:
    - `Scope`
    - Geriye doğru dönme süresi
-   **Farklı teklif türleri Için rezervasyon** bilgileri-yalnızca kurumsal ve Web Direct müşterileri için rezervasyon bilgileri kullanılabilir.

Daha fazla bilgi için, bkz. [ayırma önerileri API 'si](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations)için teknik belirtim.

## <a name="reservation-details-api"></a>Ayırma ayrıntıları API 'SI

Ne kadar tüketim ayrılacağını ve ne kadar harcandığına ilişkin daha önce satın alınan VM ayırmaları hakkındaki bilgileri görmek için ayırma ayrıntıları API 'sini kullanın. Verileri her bir VM düzeyinde ayrıntı için görebilirsiniz. 

API içerir:

-   **Azure rol tabanlı Access Control** -aboneliğin kullanım verilerine hangi kullanıcıların veya uygulamaların erişebileceğini belirtmek için [Azure Portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet 'lerinde](https://docs.microsoft.com/powershell/azure/overview) erişim ilkelerini yapılandırın. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin. 
-   **Filtreleme** -aşağıdaki FILTREYI kullanarak API sonuç kümesini daha küçük bir ayırma kümesine kırpın:
    - Tarih aralığı
-   **Farklı teklif türleri Için rezervasyon** bilgileri-yalnızca kurumsal ve Web Direct müşterileri için rezervasyon bilgileri kullanılabilir.

Daha fazla bilgi için bkz. [ayırma ayrıntıları API 'si](https://docs.microsoft.com/rest/api/consumption/reservationsdetails)için teknik belirtim.

## <a name="reservation-summaries-api"></a>Ayırma özetleri API 'SI

Bu API 'yi, daha önce satın alınan ve ne kadar tüketim ayrıldığı ve toplamakta ne kadar tüketim kullanıldığı hakkında daha önce satın alınmış VM rezervasyonları hakkındaki toplu bilgileri görmek için kullanın. 

API içerir:

-   **Azure rol tabanlı Access Control** -aboneliğin kullanım verilerine hangi kullanıcıların veya uygulamaların erişebileceğini belirtmek için [Azure Portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet 'lerinde](https://docs.microsoft.com/powershell/azure/overview) erişim ilkelerini yapılandırın. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin. 
-   **Filtreleme** -günlük gren şu filtreyle kullanırken sonuçlarınızı uyarlayın:
    - Kullanım Tarihi
-   **Farklı teklif türleri Için rezervasyon** bilgileri-yalnızca kurumsal ve Web Direct müşterileri için rezervasyon bilgileri kullanılabilir.
-   **Günlük veya aylık toplamalar** – arayanlar, rezervasyon özet verilerinin günlük veya aylık grende mi olacağını belirtebilir.

Daha fazla bilgi için, bkz. [ayırma özetleri API 'si](https://docs.microsoft.com/rest/api/consumption/reservationssummaries)için teknik belirtim.

## <a name="price-sheet-api"></a>Fiyat listesi API 'SI
Kurumsal müşteri bu API 'YI kullanarak tüm ölçümler için özel fiyatlandırma elde edebilir. Kuruluşlar bu işlemi kullanım ayrıntıları ve pazar yerleri kullanım bilgileri ile birlikte kullanarak kullanım ve Market verilerini kullanarak maliyet hesaplamaları gerçekleştirebilir. 

API içerir:

-   **Azure rol tabanlı Access Control** -aboneliğin kullanım verilerine hangi kullanıcıların veya uygulamaların erişebileceğini belirtmek için [Azure Portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) veya [Azure PowerShell cmdlet 'lerinde](https://docs.microsoft.com/powershell/azure/overview) erişim ilkelerini yapılandırın. Çağıranlar, kimlik doğrulaması için standart Azure Active Directory belirteçleri kullanmanız gerekir. Arayanın belirli bir Azure aboneliği için kullanım verilerine erişim elde etmek için faturalandırma okuyucusu, okuyucu, sahibi veya katkıda bulunan rolüne ekleyin. 
-   **Yalnızca kurumsal müşteriler** -bu API yalnızca şirket içi müşteriler için geçerlidir. Web Direct müşterilerinin fiyatlandırma almak için RateCard API 'sini kullanması gerekir. 

Daha fazla bilgi için bkz. [Fiyat listesi API 'si](https://docs.microsoft.com/rest/api/consumption/pricesheet)için teknik belirtim.

## <a name="scenarios"></a>Senaryolar

Tüketim API 'Leri aracılığıyla mümkün kılınan bazı senaryolar aşağıda verilmiştir:

-   **Fatura mutabakatı** -Microsoft doğru miktarı mi ücretlendirdim?  Faturam nedir ve kendi kendinize hesaplayabilir miyim?
-   **Çapraz ücretler** -ne kadar ücret ödediğimize göre, Kuruluşum ne kadar ücret ödemem gerektiğini biliyorum mi?
-   **Maliyet iyileştirmesi** -ne kadar ücretlendirildiğimizi biliyorum... Azure 'da harcadığım paradan nasıl daha fazla bilgi alabilirim?
-   **Maliyet izleme** -zaman içinde Azure 'u ne kadar harcadığımı ve kullanmakta olduğumu görmek istiyorum. Eğilimleri nelerdir? Nasıl daha iyi yapabilirim?
-   **Aylık Azure harcaması** -geçerli aydan ne kadar harcasın? Azure 'un harcama ve/veya kullanımım üzerinde herhangi bir ayarlama yapmam gerekir mi? Aylık olarak Azure tüketdim.
-   **Uyarıları ayarlama** -bir bütçeye göre kaynak tabanlı tüketim veya parasal tabanlı uyarı ayarlamak istiyorum.

## <a name="next-steps"></a>Sonraki Adımlar

- Azure kullanımınız hakkında programlama yoluyla bilgi almak için Azure Faturalama API'leri kullanma hakkında daha fazla bilgi için bkz. [Azure Faturalandırma API 'Sine genel bakış](billing-usage-rate-card-overview.md).



