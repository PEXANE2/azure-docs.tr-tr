---
title: Azure Güvenlik Merkezi 'nde Uyarlamalı ağ sağlamlaştırma | Microsoft Docs
description: Ağ güvenlik grupları (NSG) kurallarınızı öğrenmek ve güvenlik durunuzu artırmak için gerçek trafik desenlerini nasıl kullanacağınızı öğrenin.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 09d62d23-ab32-41f0-a5cf-8d80578181dd
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/11/2020
ms.author: memildin
ms.openlocfilehash: e8aea9b8abb5926fdb73df7c140ecfec1114f7a0
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90894757"
---
# <a name="adaptive-network-hardening-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde Uyarlamalı ağ sağlamlaştırma
Azure Güvenlik Merkezi 'nde Uyarlamalı ağ sağlamlaştırma yapılandırma hakkında bilgi edinin.

## <a name="availability"></a>Kullanılabilirlik
|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel olarak kullanılabilir (GA)|
|Fiyat|[Sunucular Için Azure Defender](defender-for-servers-introduction.md) gerekir|
|Gerekli roller ve izinler:|Makinenin NSG 'leri üzerinde yazma izinleri|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Hayır](./media/icons/no-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||

## <a name="what-is-adaptive-network-hardening"></a>Uyarlamalı ağ sağlamlaştırma nedir?
Kaynaklara/kaynaklardan gelen trafiği filtrelemek için [ağ güvenlik grupları (NSG)](https://docs.microsoft.com/azure/virtual-network/security-overview) uygulama, ağ güvenlik duruşunuzu geliştirir. Ancak, NSG üzerinden akan gerçek trafiğin, tanımlanan NSG kurallarının bir alt kümesi olduğu bazı durumlar da olabilir. Bu durumlarda, güvenlik duruşunu artırmak, gerçek trafik desenlerine bağlı olarak NSG kurallarını sağlamlaştırma yoluyla elde edilebilir.

Uyarlamalı ağ sağlamlaştırma, NSG kurallarına daha fazla uyum sağlamak için öneriler sağlar. Gerçek trafik, bilinen güvenilen yapılandırma, tehdit bilgileri ve diğer güvenlik açığı göstergelerini gösteren bir makine öğrenimi algoritması kullanır ve yalnızca belirli IP/bağlantı noktası tanımlama bilgilerine giden trafiğe izin vermek için öneriler sağlar.

Örneğin, var olan NSG kuralı, 22 numaralı bağlantı noktasında 140.20.30.10/24 ' ten trafiğe izin vermek için olduğunu varsayalım. Analiz temelinde Uyarlamalı ağ sağlamlaştırma 'nin önerisi, daha dar bir IP aralığı olan 140.23.30.10/29 ' dan gelen trafiğe izin vermek ve bu bağlantı noktası için diğer tüm trafiği reddetmektir.

>[!TIP]
> Uyarlamalı ağ sağlamlaştırma önerileri yalnızca aşağıdaki özel bağlantı noktalarında desteklenir (hem UDP hem de TCP için): 13, 17, 19, 22, 23, 53, 69, 81, 111, 119, 123, 135, 137, 138, 139, 161, 162, 389, 445, 512, 514, 593, 636, 873, 1433, 1434, 1900, 2049, 2301, 2323, 2381, 3268, 3306, 3389, 4333, 5353, 5432, 5555, 5800, 5900, 5900, 5985, 5986, 6379 , 6379, 7000, 7001, 7199, 8081, 8089, 8545, 9042, 9160, 9300, 11211, 16379, 26379, 27017, 37215


![Ağ sağlamlaştırma görünümü](./media/security-center-adaptive-network-hardening/traffic-hardening.png)




## <a name="view-adaptive-network-hardening-alerts-and-rules"></a>Uyarlamalı ağ sağlamlaştırma uyarılarını ve kurallarını görüntüle

1. Güvenlik Merkezi 'nde **ağ**  ->  **Uyarlamalı ağ sağlamlaştırma**' ı seçin. Ağ VM 'Leri üç ayrı sekme altında listelenir:
   * **Sağlıksız kaynaklar**: Şu anda Uyarlamalı ağ sağlamlaştırma algoritması çalıştırılarak tetiklenen önerilere ve uyarılara sahip VM 'ler vardır. 
   * **Sağlıklı kaynaklar**: uyarılar ve öneriler olmadan VM 'ler.
   * **Taranmamış kaynaklar**: aşağıdaki nedenlerden biri nedeniyle Uyarlamalı ağ sağlamlaştırma algoritması üzerinde çalıştırılamaz VM 'ler:
      * **Sanal makineler klasik VM**'ler: yalnızca Azure Resource Manager VM 'ler desteklenir.
      * **Yeterli veri**yok: doğru trafik sağlamlaştırma önerilerini oluşturmak Için, güvenlik merkezi 'nin en az 30 günlük trafik verisi olması gerekir.
      * **VM, Azure Defender tarafından korunmuyor**: yalnızca [sunucular için Azure Defender](defender-for-servers-introduction.md) ile korunan VM 'ler bu özellik için uygundur.

     ![sağlıksız kaynaklar](./media/security-center-adaptive-network-hardening/unhealthy-resources.png)

2. **Sağlıksız kaynaklar** sekmesinden, uyarılarını ve uygulanacak önerilen sağlamlaştırma kurallarını görüntülemek IÇIN bir VM seçin.

    ![uyarıları sağlamlaştırma](./media/security-center-adaptive-network-hardening/anh-recommendation-rules.png)


## <a name="review-and-apply-adaptive-network-hardening-recommended-rules"></a>Uyarlamalı ağ sağlamlaştırma önerilen kuralları gözden geçirin ve uygulayın

1. **Sağlıksız kaynaklar** SEKMESINDEN bir VM seçin. Uyarılar ve önerilen sağlamlaştırma kuralları listelenir.

     ![Sağlamlaştırma kuralları](./media/security-center-adaptive-network-hardening/hardening-alerts.png)

   > [!NOTE]
   > **Kurallar** sekmesi, uyarlamalı ağ sağlamlaştırma 'in eklemenizi önerdiği kuralları listeler. **Uyarılar** sekmesi, trafik nedeniyle oluşturulan uyarıları, önerilen kurallarda ızın verilen IP aralığı içinde olmayan kaynağa akan şekilde listeler.

2. Bir kuralın parametrelerinden bazılarını değiştirmek istiyorsanız, [kuralı değiştirme](#modify-rule)bölümünde açıklandığı gibi değiştirebilirsiniz.
   > [!NOTE]
   > Ayrıca, bir kuralı [silebilir](#delete-rule) veya [ekleyebilirsiniz](#add-rule) .

3. NSG 'de uygulamak istediğiniz kuralları seçin ve **Uygula**' ya tıklayın.

      > [!NOTE]
      > Zorlanan kurallar VM 'yi koruyan NSG 'ler 'e eklenir. (VM, NIC ile ilişkili bir NSG veya VM 'nin bulunduğu alt ağ veya her ikisi de) ile korunabilir

    ![Kuralları zorla](./media/security-center-adaptive-network-hardening/enforce-hard-rule2.png)


### <a name="modify-a-rule"></a>Bir kuralı <a name ="modify-rule"> </a> değiştirme

Önerilen bir kuralın parametrelerini değiştirmek isteyebilirsiniz. Örneğin, önerilen IP aralıklarını değiştirmek isteyebilirsiniz.

Uyarlamalı ağ sağlamlaştırma kuralını değiştirmek için bazı önemli yönergeler:

* "İzin ver" kurallarının parametrelerini değiştirebilirsiniz. 
* "İzin ver" kurallarının "reddetme" kuralları haline gelmesini değiştiremezsiniz. 

  > [!NOTE]
  > "Reddetme" kuralları oluşturma ve değiştirme doğrudan NSG 'de yapılır. Daha fazla bilgi için bkz. [ağ güvenlik grubu oluşturma, değiştirme veya silme](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group).

* **Tüm trafiği reddet** kuralı, burada listelenecek olan tek tür "reddetme" kuralıdır ve değiştirilemez. Ancak, bunu silebilirsiniz (bkz. [bir kuralı silme](#delete-rule)).
  > [!NOTE]
  > Algoritmayı **çalıştırmanın bir sonucu** olarak, güvenlik merkezi 'nin var olan NSG yapılandırmasına bağlı olarak izin verilmesi gereken trafiği tanımlamaz. Bu nedenle önerilen kural, belirtilen bağlantı noktası için tüm trafiği reddetmektir. Bu kural türünün adı "*sistem tarafından oluşturulan*" olarak görüntülenir. Bu kural zorlandıktan sonra, NSG 'deki gerçek adı, protokol, trafik yönü, "reddetme" ve rastgele bir sayıdan oluşan bir dize olacaktır.

*Uyarlamalı ağ sağlamlaştırma kuralını değiştirmek için:*

1. Bir kuralın parametrelerinden bazılarını değiştirmek için, **kurallar** sekmesinde, kuralın satırının sonundaki üç noktaya (...) tıklayın ve **Düzenle**' ye tıklayın.

   ![S kuralını Düzenle](./media/security-center-adaptive-network-hardening/edit-hard-rule.png)

1. **Kural düzenleme** penceresinde, değiştirmek istediğiniz ayrıntıları güncelleştirin ve **Kaydet**' e tıklayın.

   > [!NOTE]
   > **Kaydet**'e tıkladıktan sonra kuralı başarıyla değiştirdiniz. *Ancak, bunu NSG 'ye uyguladınız.* Bunu uygulamak için listeden kuralı seçmeniz ve **Uygula** ' yı seçmeniz gerekir (sonraki adımda açıklandığı gibi).

   ![Kaydet seçiliyor](./media/security-center-adaptive-network-hardening/edit-hard-rule3.png)

3. Güncelleştirilmiş kuralı uygulamak için listeden, güncelleştirilmiş kuralı seçin ve **Uygula**' ya tıklayın.

    ![kuralı zorla](./media/security-center-adaptive-network-hardening/enforce-hard-rule.png)

### <a name="add-a-new-rule"></a>Yeni kural <a name ="add-rule"> </a> Ekle

Güvenlik Merkezi 'nin önerilmeyen bir "izin verme" kuralı ekleyebilirsiniz.

> [!NOTE]
> Buraya yalnızca "izin ver" kuralları eklenebilir. "Reddetme" kuralları eklemek istiyorsanız, bunu doğrudan NSG üzerinde yapabilirsiniz. Daha fazla bilgi için bkz. [ağ güvenlik grubu oluşturma, değiştirme veya silme](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group).

*Uyarlamalı ağ sağlamlaştırma kuralı eklemek için:*

1. **Kural Ekle** ' ye tıklayın (sol üst köşede bulunur).

   ![kural ekleme](./media/security-center-adaptive-network-hardening/add-hard-rule.png)

1. **Yeni kural** penceresinde, ayrıntıları girin ve **Ekle**' ye tıklayın.

   > [!NOTE]
   > **Ekle**' ye tıkladıktan sonra kuralı başarıyla eklediniz ve önerilen diğer kurallara göre listelenir. Ancak, bunu NSG 'ye uyguladınız. Etkinleştirmek için, listeden kuralı seçmeniz ve **Uygula** ' yı (sonraki adımda açıklandığı gibi) seçmeniz gerekir.

3. Yeni kuralı uygulamak için listeden yeni kuralı seçin ve **Uygula**' ya tıklayın.

    ![kuralı zorla](./media/security-center-adaptive-network-hardening/enforce-hard-rule.png)


### <a name="delete-a-rule"></a>Bir kuralı <a name ="delete-rule"> </a> silme

Gerektiğinde, geçerli oturum için önerilen bir kuralı silebilirsiniz. Örneğin, önerilen bir kuralı uygulamanın meşru trafiği engelleyeceğini belirleyebilirsiniz.

*Geçerli oturumunuz için uyarlamalı ağ sağlamlaştırma kuralını silmek için:*

1. **Kurallar** sekmesinde, kuralın satırının sonundaki üç noktaya (...) tıklayın ve **Sil**' e tıklayın.  

    ![Bir kuralı silme](./media/security-center-adaptive-network-hardening/delete-hard-rule.png)