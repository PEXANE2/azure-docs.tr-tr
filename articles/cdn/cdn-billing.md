---
title: Azure CDN faturalandırmasını anlama | Microsoft Dokümanlar
description: Bu SSS, Azure CDN faturalandırmanın nasıl çalıştığını açıklar.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2019
ms.author: allensu
ms.openlocfilehash: d3a2dfba98f83d34c3e83ec865e3b692f7dbacd2
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81254232"
---
# <a name="understanding-azure-cdn-billing"></a>Azure CDN faturalamasını anlama

Bu SSS, Azure İçerik Dağıtım Ağı (CDN) tarafından barındırılan içeriğin faturalandırma yapısını açıklar.

## <a name="what-is-a-billing-region"></a>Faturalama bölgesi nedir?
Faturalama bölgesi, Azure CDN'den nesnelerin teslimi için hangi fiyatın ücretlendirildiğini belirlemek için kullanılan coğrafi bir alandır. Geçerli faturalandırma bölgeleri ve bölgeleri aşağıdaki gibidir:

- Bölge 1: Kuzey Amerika, Avrupa, Orta Doğu ve Afrika

- Bölge 2: Asya Pasifik (Japonya dahil)

- Bölge 3: Güney Amerika

- Bölge 4: Avustralya ve Yeni Zelanda

- Bölge 5: Hindistan

Durum noktası (POP) bölgeleri hakkında bilgi için [bölgeye göre Azure CDN POP konumları'na](https://docs.microsoft.com/azure/cdn/cdn-pop-locations)bakın. Örneğin, Meksika'da bulunan bir POP Kuzey Amerika bölgesindedir ve bu nedenle bölge 1'e dahildir. 

Azure CDN fiyatlandırması hakkında daha fazla bilgi için [İçerik Dağıtım Ağı fiyatlandırması'na](https://azure.microsoft.com/pricing/details/cdn/)bakın.

## <a name="how-are-delivery-charges-calculated-by-region"></a>Teslimat ücretleri bölgeye göre nasıl hesaplanır?
Azure CDN faturalandırma bölgesi, içeriği son kullanıcıya teslim eden kaynak sunucunun konumuna dayanır. İstemcinin hedefi (fiziksel konumu) faturalama bölgesi olarak kabul edilmez.

Örneğin, Meksika'da bulunan bir kullanıcı bir istek yayınlarsa ve bu istek, izleme veya trafik koşulları nedeniyle ABD POP'ta bulunan bir sunucu tarafından hizmet vereliyse, faturalandırma bölgesi ABD olur.

## <a name="what-is-a-billable-azure-cdn-transaction"></a>Faturalandırılabilir Azure CDN işlemi nedir?
CDN'de sona eren herhangi bir HTTP(S) isteği, tüm yanıt türlerini içeren faturalandırılabilir bir olaydır: başarı, başarısızlık veya diğer. Ancak, farklı yanıtlar farklı trafik tutarları oluşturabilir. Örneğin, *304 Değiştirilmemiş* ve yalnızca diğer üstbilgi yanıtları küçük bir üstbilgi yanıtı olduğundan çok az trafik oluşturur; benzer şekilde, hata yanıtları (örneğin, *404 Bulunamadı)* faturalandırılabilir, ancak küçük yanıt yükü nedeniyle küçük bir maliyete tabidir.

## <a name="what-other-azure-costs-are-associated-with-azure-cdn-use"></a>Azure CDN kullanımıyla ilişkili başka hangi Azure maliyetleri nelerdir?
Azure CDN'nin kullanılması, nesnelerinizin kaynağı olarak kullanılan hizmetlerde bazı kullanım ücretlerine de neden olur. Bu maliyetler genellikle genel CDN kullanım maliyetinin küçük bir kısmıdır.

İçeriğinizin kaynağı olarak Azure Blob depolama alanını kullanıyorsanız, önbellek doldurmaları için aşağıdaki depolama ücretlerine de tabi siniz:

- Kullanılan gerçek GB: Kaynak nesnelerinizin gerçek depolama alanı.

- İşlemler: Önbelleği doldurmak için gerektiği gibi.

- GB olarak aktarımlar: CDN önbelleklerini doldurmak için aktarılan veri miktarı.

> [!NOTE]
> Ekim 2019'dan itibaren Microsoft'tan Azure CDN kullanıyorsanız, Azure'da barındırılan Origins'ten CDN'lere veri aktarım maliyeti ücretsizdir. Verizon'dan Azure CDN ve Akamai'den Azure CDN aşağıda açıklanan fiyatlara tabidir.

Azure Depolama faturalandırması hakkında daha fazla bilgi için [bkz.](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/)

*Barındırılan hizmet teslimini*kullanıyorsanız, aşağıdaki gibi ücrete tabi olacaktır:

- Azure hesaplama süresi: Kaynak olarak hareket eden işlem örnekleri.

- Azure bilgi aktarımı: Azure CDN önbelleklerini doldurmak için bilgi işlem örneklerinden veri aktarımı.

Müşteriniz bayt aralığı isteklerini kullanıyorsa (kaynak hizmetine bakılmaksızın), aşağıdaki hususlar geçerlidir:

- Bayt *aralığı isteği* CDN'de faturalandırılabilir bir işlemdir. İstemci bir bayt aralığı isteği verdiğinde, bu istek nesnenin bir alt kümesi (aralığı) içindir. CDN, istenen içeriğin yalnızca kısmi bir bölümüyle yanıt verir. Bu kısmi yanıt faturalandırılabilir bir işlemdir ve aktarım tutarı aralık yanıtı (artı üstbilgi) boyutuyla sınırlıdır.

- Bir istek bir nesnenin yalnızca bir bölümü için geldiğinde (bayt aralığı üstbilgi belirterek), CDN tüm nesneyi önbelleğine getirebilir. Sonuç olarak, CDN'den faturalandırılabilir işlem kısmi bir yanıt için olsa da, kaynaktan faturalandırılabilir işlem nesnenin tam boyutunu içerebilir.

## <a name="how-much-transfer-activity-occurs-to-support-the-cache"></a>Önbelleği desteklemek için ne kadar aktarım etkinliği oluşur?
Bir CDN POP önbelleğini doldurmak için her zaman, önbelleğe alan nesne için kaynak için bir istekte bulunmaz. Sonuç olarak, kaynak, gözden kaçırdığı her önbellekte faturalandırılabilir bir işlem le sonuçlanabilir. Önbellek kaçıranların sayısı bir dizi etkene bağlıdır:

- İçerik nasıl önbelleğe alınabilir: İçeriğin yüksek TTL'si (zaman-canlı)/son kullanma değerleri varsa ve önbellekte popüler kalması için sık sık erişilirse, yükün büyük çoğunluğu CDN tarafından işlenir. Tipik bir iyi önbellek isabet oranı %90'ın üzerindedir, bu da istemci isteklerinin %10'dan azının önbellek kaçığı veya nesne yenilemesi için menşeine dönmesi gerektiği anlamına gelir.

- Nesneyi yüklemek için kaç düğüm gerekir: Bir düğüm bir nesneyi kaynaktan her yükledinde faturalandırılabilir bir işlem le karşılaşır. Sonuç olarak, daha fazla genel içerik (daha fazla düğümden erişilen) daha fazla faturalandırılabilir işlemle sonuçlanır.

- TTL etkisi: Bir nesne için daha yüksek bir TTL, nesnenin kaynağından daha az sıklıkta alınması gerektiği anlamına gelir. Ayrıca, tarayıcılar gibi istemcilerin nesneyi daha uzun süre önbelleğe alabileceği ve cdn'ye yapılan hareketleri azaltabileceği anlamına da gelir.

## <a name="which-origin-services-are-eligible-for-free-data-transfer-with-azure-cdn-from-microsoft"></a>Microsoft'tan Azure CDN ile hangi kaynak hizmetleri ücretsiz veri aktarımı için uygundur? 
CDN menşei olarak aşağıdaki Azure hizmetlerinden birini kullanıyorsanız, Kaynak'tan CDN PoP'larına Veri aktarımı ndan ücret alınmazsınız. 

- Azure Storage
- Azure Medya Hizmetleri
- Azure Sanal Makineler
- Sanal Ağ
- Load Balancer
- Application Gateway
- Azure DNS
- ExpressRoute
- VPN Gateway
- Traffic Manager
- Ağ İzleyicisi
- Azure Güvenlik Duvarı
- Azure Front Door Hizmeti
- Azure Bastion
- Azure Uygulaması hizmeti
- Azure İşlevleri
- Azure Data Factory
- Azure API Management
- Azure Batch 
- Azure Veri Gezgini
- HDInsight
- Azure Cosmos DB
- Azure Data Lake Store
- Azure Machine Learning 
- Azure SQL veritabanı
- Redis için Azure Önbelleği

## <a name="how-do-i-manage-my-costs-most-effectively"></a>Maliyetlerimi en etkili şekilde nasıl yönetirim?
İçeriğinizde mümkün olan en uzun TTL'yi ayarlayın. 
