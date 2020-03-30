---
title: Azure Güvenlik Merkezi'nde Uyarlanabilir Ağ Sertleştirme | Microsoft Dokümanlar
description: Ağ güvenlik gruplarınızı (NSG) kurallarınızı sertleştirmek ve güvenlik duruşunuzu daha da iyileştirmek için gerçek trafik modellerini nasıl kullanacağınızı öğrenin.
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
ms.openlocfilehash: a75be23e2e8215d86aebcfd7f4317f2f597d3c5b
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80385087"
---
# <a name="adaptive-network-hardening-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde Uyarlanabilir Ağ Sertleştirme
Azure Güvenlik Merkezi'nde Uyarlanabilir Ağ Sertleştirme'yi nasıl yapılandırılatırın öğrenin.

## <a name="what-is-adaptive-network-hardening"></a>Adaptif Ağ Sertleştirme nedir?
Kaynaklara gelen ve gelen trafiği filtrelemek için [ağ güvenlik grupları (NSG)](https://docs.microsoft.com/azure/virtual-network/security-overview) uygulamak, ağ güvenlik duruşunuzu geliştirir. Ancak, NSG üzerinden akan gerçek trafiğin tanımlanan NSG kurallarının bir alt kümesi olduğu bazı durumlar da olabilir. Bu gibi durumlarda, gerçek trafik modellerine göre NSG kurallarının sertleştirilmesiyle güvenlik duruşunun daha da iyileştirilmesi sağlanabilir.

Adaptif Ağ Sertleştirme, NSG kurallarını daha da sertleştirmek için öneriler sağlar. Gerçek trafikte, bilinen güvenilir yapılandırmayı, tehdit istihbaratını ve diğer uzlaşma göstergelerini etkileyen bir makine öğrenme algoritması kullanır ve ardından yalnızca belirli IP/bağlantı noktası tuples'inden gelen trafiğe izin vermek için öneriler sağlar.

Örneğin, mevcut NSG kuralının 22 nolu bağlantı noktasında 140.20.30.10/24'ten gelen trafiğe izin vermek olduğunu varsayalım. Adaptive Network Hardening tavsiyesi, analize dayalı, aralığı daraltmak ve 140.23.30.10/29 trafik izin - daha dar bir IP aralığı, ve bu bağlantı noktasına diğer tüm trafik inkar olacaktır.

>[!TIP]
> Adaptif Ağ Sertleştirme önerileri yalnızca aşağıdaki belirli bağlantı noktalarında desteklenir (hem UDP hem de TCP için): 13, 17, 19, 22, 23, 53, 69, 81, 111, 119, 123, 135, 137, 138, 139, 161, 162, 389, 445, 512, 514, 593, 636, 873, 1433, 1434, 1900, 204, 203, 203, 203 , 2323, 2381, 3268, 3306, 3389, 4333, 5353, 5432, 5555, 5800, 5900, 5900, 5985, 5986, 6379, 6379, 7000, 7001, 7199, 8081, 8089, 8545, 9042, 9160, 9300, 11211, 16379, 26379, 27017, 37215


![Ağ sertleştirme görünümü](./media/security-center-adaptive-network-hardening/traffic-hardening.png)


## <a name="view-adaptive-network-hardening-alerts-and-rules"></a>Uyarlanabilir Ağ Sertleştirme uyarılarını ve kurallarını görüntüleyin

1. Güvenlik Merkezi'nde Ağ**Uyarlamalı Ağ Sertleştirme'yi** **Networking** -> seçin. Ağ VM'leri üç ayrı sekme altında listelenir:
   * **Sağlıksız kaynaklar**: Şu anda Uyarlanabilir Ağ Sertleştirme algoritması çalıştırılarak tetiklenen öneriler ve uyarılar olan VM'ler. 
   * **Sağlıklı kaynaklar**: Uyarılar ve öneriler olmadan VM'ler.
   * **Taranmamış kaynaklar**: Adaptif Ağ Sertleştirme algoritmasının aşağıdaki nedenlerden dolayı çalıştırılamayacağı VM'ler:
      * **VM'ler Klasik VM'lerdir**: Yalnızca Azure Kaynak Yöneticisi VM'leri desteklenir.
      * **Yeterli veri yok**: Doğru trafik sertleştirme önerileri oluşturmak için Güvenlik Merkezi en az 30 günlük trafik verisi gerektirir.
      * **VM ASC standardı ile korunmaz**: Bu özellik için yalnızca Security Center'ın Standart fiyatlandırma katmanına ayarlanmış VM'ler yararlanabilir.

     ![sağlıksız kaynaklar](./media/security-center-adaptive-network-hardening/unhealthy-resources.png)

2. Sağlıksız **kaynaklar** sekmesinden, uyarılarını ve uygulanacak önerilen sertleştirme kurallarını görüntülemek için bir VM seçin.

    ![sertleştirme uyarıları](./media/security-center-adaptive-network-hardening/anh-recommendation-rules.png)


## <a name="review-and-apply-adaptive-network-hardening-recommended-rules"></a>Adaptive Network Hardening önerilen kuralları gözden geçirin ve uygulayın

1. Sağlıksız **kaynaklar** sekmesinden bir VM seçin. Uyarılar ve önerilen sertleştirme kuralları listelenir.

     ![sertleştirme kuralları](./media/security-center-adaptive-network-hardening/hardening-alerts.png)

   > [!NOTE]
   > **Kurallar** sekmesi, Bağdaştırıcı Ağ Sertleştirme'nin eklemenizi önerdiği kuralları listeler. **Uyarılar** sekmesi, önerilen kurallarda izin verilen IP aralığında olmayan kaynağa akan trafik nedeniyle oluşturulan uyarıları listeler.

2. Bir kuralın parametrelerinden bazılarını değiştirmek istiyorsanız, [kuralı değiştir'de](#modify-rule)açıklandığı gibi kuralı değiştirebilirsiniz.
   > [!NOTE]
   > Ayrıca bir kuralı [silebilir](#delete-rule) veya [ekleyebilirsiniz.](#add-rule)

3. NSG'de uygulamak istediğiniz kuralları seçin ve **Uygula'yı**tıklatın.

      > [!NOTE]
      > VM'yi koruyan NSG'ye zorunlu kurallar eklenir. (Bir VM, NIC'si ile ilişkili bir NSG veya VM'nin bulunduğu alt ağ veya her ikisi tarafından korunabilir)

    ![kuralları uygulamak](./media/security-center-adaptive-network-hardening/enforce-hard-rule2.png)


### <a name="modify-a-rule"></a>Kuralı <a name ="modify-rule"> </a> değiştirme

Önerilen bir kuralın parametrelerini değiştirmek isteyebilirsiniz. Örneğin, önerilen IP aralıklarını değiştirmek isteyebilirsiniz.

Uyarlanabilir Ağ Sertleştirme kuralını değiştirmek için bazı önemli yönergeler:

* Yalnızca "izin ver" kurallarının parametrelerini değiştirebilirsiniz. 
* "İzin ver" kurallarını değiştirip "reddet" kuralları olamaz. 

  > [!NOTE]
  > "Reddet" kurallarının oluşturulması ve değiştirilmesi doğrudan NSG'de yapılır. Daha fazla bilgi için bkz: [Ağ güvenlik grubu oluştur, değiştir veya sil.](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)

* Tüm trafik kuralını **reddet,** burada listelenen tek "reddet" kuralı türüdür ve değiştirilemez. Ancak, silebilirsiniz (bkz. [bir kuralı sil).](#delete-rule)
  > [!NOTE]
  > Algoritmayı çalıştıran bir sonucu olarak, Güvenlik Merkezi varolan NSG yapılandırmasını temel alan izin verilmesi gereken trafiği tanımlamadığında tüm trafik kuralını **reddet** önerilir. Bu nedenle, önerilen kural belirtilen bağlantı noktasına tüm trafiği reddetmektir. Bu kural türünün adı "*Sistem Oluşturuldu "* olarak görüntülenir. Bu kuralı uyguladıktan sonra, NSG'deki gerçek adı protokol, trafik yönü, "DENY" ve rasgele bir sayıdan oluşan bir dize olacaktır.

*Uyarlanabilir Ağ Sertleştirme kuralını değiştirmek için:*

1. **Kurallar** sekmesinde, bir kuralın bazı parametrelerini değiştirmek için, kuralın satırının sonundaki üç noktayı (...) tıklatın ve **Edit'i**tıklatın.

   ![kuralı nı edin](./media/security-center-adaptive-network-hardening/edit-hard-rule.png)

1. Kuralı **Nı Edit** penceresinde, değiştirmek istediğiniz ayrıntıları güncelleştirin ve **Kaydet'i**tıklatın.

   > [!NOTE]
   > **Kaydet'i**tıklattıktan sonra kuralı başarıyla değiştirdiniz. *Ancak, NSG'ye uygulamadın.* Bunu uygulamak için listedeki kuralı seçmeniz ve **Uygula'yı** (bir sonraki adımda açıklandığı gibi) tıklatmanız gerekir.

   ![kuralı nı edin](./media/security-center-adaptive-network-hardening/edit-hard-rule3.png)

3. Güncelleştirilmiş kuralı listeden uygulamak için güncelleştirilmiş kuralı seçin ve **Uygula'yı**tıklatın.

    ![kuralı uygulamak](./media/security-center-adaptive-network-hardening/enforce-hard-rule.png)

### <a name="add-a-new-rule"></a>Yeni bir <a name ="add-rule"> </a> kural ekleme

Güvenlik Merkezi tarafından önerilmeyen bir "izin ver" kuralı ekleyebilirsiniz.

> [!NOTE]
> Buraya yalnızca "izin ver" kuralları eklenebilir. "Reddet" kurallarını eklemek istiyorsanız, bunu doğrudan NSG'de yapabilirsiniz. Daha fazla bilgi için bkz: [Ağ güvenlik grubu oluştur, değiştir veya sil.](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)

*Uyarlanabilir Ağ Sertleştirme kuralı eklemek için:*

1. **Ekle kuralını** tıklatın (sol üst köşede bulunur).

   ![kural ekle](./media/security-center-adaptive-network-hardening/add-hard-rule.png)

1. Yeni **kural** penceresinde ayrıntıları girin ve **Ekle'yi**tıklatın.

   > [!NOTE]
   > **Ekle'yi**tıklattıktan sonra kuralı başarıyla eklediniz ve diğer önerilen kurallarla birlikte listelenir. Ancak, NSG üzerinde uygulamadı. Etkinleştirmek için listedeki kuralı seçmeniz ve **Uygula'yı** (bir sonraki adımda açıklandığı gibi) tıklatmanız gerekir.

3. Yeni kuralı uygulamak için, listeden yeni kuralı seçin ve **Uygula'yı**tıklatın.

    ![kuralı uygulamak](./media/security-center-adaptive-network-hardening/enforce-hard-rule.png)


### <a name="delete-a-rule"></a>Kuralı <a name ="delete-rule"> </a> silme

Gerektiğinde, geçerli oturum için önerilen kuralı silebilirsiniz. Örneğin, önerilen bir kural uygulamanın yasal trafiği engelleyebileceğini belirleyebilirsiniz.

*Geçerli oturumunuz için Uyarlanabilir Ağ Sertleştirme kuralını silmek için:*

1. **Kurallar** sekmesinde, kuralın satırının sonundaki üç noktayı (...) tıklatın ve **Sil'i**tıklatın.  

    ![sertleştirme kuralları](./media/security-center-adaptive-network-hardening/delete-hard-rule.png)