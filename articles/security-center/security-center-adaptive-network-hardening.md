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
ms.openlocfilehash: bc610fa1d7a5fa1a10db3298164404b92d5d9f85
ms.sourcegitcommit: d322d0a9d9479dbd473eae239c43707ac2c77a77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79139598"
---
# <a name="adaptive-network-hardening-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde Uyarlamalı ağ sağlamlaştırma
Azure Güvenlik Merkezi 'nde Uyarlamalı ağ sağlamlaştırma yapılandırma hakkında bilgi edinin.

## <a name="what-is-adaptive-network-hardening"></a>Uyarlamalı ağ sağlamlaştırma nedir?
Kaynaklara/kaynaklardan gelen trafiği filtrelemek için [ağ güvenlik grupları (NSG)](https://docs.microsoft.com/azure/virtual-network/security-overview) uygulama, ağ güvenlik duruşunuzu geliştirir. Ancak, NSG üzerinden akan gerçek trafiğin, tanımlanan NSG kurallarının bir alt kümesi olduğu bazı durumlar da olabilir. Bu durumlarda, güvenlik duruşunu artırmak, gerçek trafik desenlerine bağlı olarak NSG kurallarını sağlamlaştırma yoluyla elde edilebilir.

Uyarlamalı ağ sağlamlaştırma, NSG kurallarına daha fazla uyum sağlamak için öneriler sağlar. Gerçek trafik, bilinen güvenilen yapılandırma, tehdit bilgileri ve diğer güvenlik açığı göstergelerini gösteren bir makine öğrenimi algoritması kullanır ve yalnızca belirli IP/bağlantı noktası tanımlama bilgilerine giden trafiğe izin vermek için öneriler sağlar.

Örneğin, var olan NSG kuralı, 22 numaralı bağlantı noktasında 140.20.30.10/24 ' ten trafiğe izin vermek için olduğunu varsayalım. Analiz temelinde Uyarlamalı ağ sağlamlaştırma 'nin önerisi, daha dar bir IP aralığı olan 140.23.30.10/29 ' dan gelen trafiğe izin vermek ve bu bağlantı noktası için diğer tüm trafiği reddetmektir.

>[!TIP]
> Uyarlamalı ağ sağlamlaştırma önerileri yalnızca belirli bağlantı noktalarında desteklenir. Tam liste için [hangi bağlantı noktalarının desteklendiği](#which-ports-are-supported) hakkında bölümüne bakın. 


![Ağ sağlamlaştırma görünümü](./media/security-center-adaptive-network-hardening/traffic-hardening.png)


## <a name="view-adaptive-network-hardening-alerts-and-rules"></a>Uyarlamalı ağ sağlamlaştırma uyarılarını ve kurallarını görüntüle

1. Güvenlik Merkezi 'nde **ağ** -> **Uyarlamalı ağ sağlamlaştırma**' ı seçin. Ağ VM 'Leri üç ayrı sekme altında listelenir:
   * **Sağlıksız kaynaklar**: Şu anda Uyarlamalı ağ sağlamlaştırma algoritması çalıştırılarak tetiklenen önerilere ve uyarılara sahip VM 'ler vardır. 
   * **Sağlıklı kaynaklar**: uyarılar ve öneriler olmadan VM 'ler.
   * **Taranmamış kaynaklar**: aşağıdaki nedenlerden biri nedeniyle Uyarlamalı ağ sağlamlaştırma algoritması üzerinde çalıştırılamaz VM 'ler:
      * **Sanal makineler klasik VM**'ler: yalnızca Azure Resource Manager VM 'ler desteklenir.
      * **Yeterli veri**yok: doğru trafik sağlamlaştırma önerilerini oluşturmak Için, güvenlik merkezi 'nin en az 30 günlük trafik verisi olması gerekir.
      * **VM, ASC standardı tarafından korunmuyor**: yalnızca güvenlik merkezi 'nin standart fiyatlandırma katmanına ayarlanmış olan VM 'ler bu özellik için uygun değildir.

     ![sağlıksız kaynaklar](./media/security-center-adaptive-network-hardening/unhealthy-resources.png)

2. **Sağlıksız kaynaklar** sekmesinden, uyarılarını ve uygulanacak önerilen sağlamlaştırma kurallarını görüntülemek IÇIN bir VM seçin.

    ![uyarıları sağlamlaştırma](./media/security-center-adaptive-network-hardening/anh-recommendation-rules.png)


## <a name="review-and-apply-adaptive-network-hardening-recommended-rules"></a>Uyarlamalı ağ sağlamlaştırma önerilen kuralları gözden geçirin ve uygulayın

1. **Sağlıksız kaynaklar** SEKMESINDEN bir VM seçin. Uyarılar ve önerilen sağlamlaştırma kuralları listelenir.

     ![sağlamlaştırma kuralları](./media/security-center-adaptive-network-hardening/hardening-alerts.png)

   > [!NOTE]
   > **Kurallar** sekmesi, uyarlamalı ağ sağlamlaştırma 'in eklemenizi önerdiği kuralları listeler. **Uyarılar** sekmesi, trafik nedeniyle oluşturulan uyarıları, önerilen kurallarda ızın verilen IP aralığı içinde olmayan kaynağa akan şekilde listeler.

2. Bir kuralın parametrelerinden bazılarını değiştirmek istiyorsanız, [kuralı değiştirme](#modify-rule)bölümünde açıklandığı gibi değiştirebilirsiniz.
   > [!NOTE]
   > Ayrıca, bir kuralı [silebilir](#delete-rule) veya [ekleyebilirsiniz](#add-rule) .

3. NSG 'de uygulamak istediğiniz kuralları seçin ve **Uygula**' ya tıklayın.

      > [!NOTE]
      > Zorlanan kurallar VM 'yi koruyan NSG 'ler 'e eklenir. (VM, NIC ile ilişkili bir NSG veya VM 'nin bulunduğu alt ağ veya her ikisi de) ile korunabilir

    ![Kuralları zorla](./media/security-center-adaptive-network-hardening/enforce-hard-rule2.png)


### Bir kuralı <a name ="modify-rule"> </a> değiştirme

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

   ![Kuralı Düzenle](./media/security-center-adaptive-network-hardening/edit-hard-rule.png)

1. **Kural düzenleme** penceresinde, değiştirmek istediğiniz ayrıntıları güncelleştirin ve **Kaydet**' e tıklayın.

   > [!NOTE]
   > **Kaydet**'e tıkladıktan sonra kuralı başarıyla değiştirdiniz. *Ancak, bunu NSG 'ye uyguladınız.* Bunu uygulamak için listeden kuralı seçmeniz ve **Uygula** ' yı (sonraki adımda açıklandığı gibi) seçmeniz gerekir.

   ![Kuralı Düzenle](./media/security-center-adaptive-network-hardening/edit-hard-rule3.png)

3. Güncelleştirilmiş kuralı uygulamak için listeden, güncelleştirilmiş kuralı seçin ve **Uygula**' ya tıklayın.

    ![kuralı zorla](./media/security-center-adaptive-network-hardening/enforce-hard-rule.png)

### Yeni kural <a name ="add-rule"> </a> Ekle

Güvenlik Merkezi 'nin önerilmeyen bir "izin verme" kuralı ekleyebilirsiniz.

> [!NOTE]
> Buraya yalnızca "izin ver" kuralları eklenebilir. "Reddetme" kuralları eklemek istiyorsanız, bunu doğrudan NSG üzerinde yapabilirsiniz. Daha fazla bilgi için bkz. [ağ güvenlik grubu oluşturma, değiştirme veya silme](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group).

*Uyarlamalı ağ sağlamlaştırma kuralı eklemek için:*

1. **Kural Ekle** ' ye tıklayın (sol üst köşede bulunur).

   ![Kural Ekle](./media/security-center-adaptive-network-hardening/add-hard-rule.png)

1. **Yeni kural** penceresinde, ayrıntıları girin ve **Ekle**' ye tıklayın.

   > [!NOTE]
   > **Ekle**' ye tıkladıktan sonra kuralı başarıyla eklediniz ve önerilen diğer kurallara göre listelenir. Ancak, bunu NSG 'ye uyguladınız. Etkinleştirmek için, listeden kuralı seçmeniz ve **Uygula** ' yı (sonraki adımda açıklandığı gibi) seçmeniz gerekir.

3. Yeni kuralı uygulamak için listeden yeni kuralı seçin ve **Uygula**' ya tıklayın.

    ![kuralı zorla](./media/security-center-adaptive-network-hardening/enforce-hard-rule.png)


### Bir kuralı <a name ="delete-rule"> </a> silme

Gerektiğinde, geçerli oturum için önerilen bir kuralı silebilirsiniz. Örneğin, önerilen bir kuralı uygulamanın meşru trafiği engelleyeceğini belirleyebilirsiniz.

*Geçerli oturumunuz için uyarlamalı ağ sağlamlaştırma kuralını silmek için:*

1. **Kurallar** sekmesinde, kuralın satırının sonundaki üç noktaya (...) tıklayın ve **Sil**' e tıklayın.  

    ![sağlamlaştırma kuralları](./media/security-center-adaptive-network-hardening/delete-hard-rule.png)

 

## <a name="which-ports-are-supported"></a>Hangi bağlantı noktaları desteklenir?

Uyarlamalı ağ sağlamlaştırma önerileri yalnızca belirli bağlantı noktalarında desteklenir. Bu tablo size tam liste sağlar:

|Bağlantı noktası|Protokol|İlişkili hizmet|
|:---:|:----:|:----|
|13|UDP|Gündüz hizmeti|
|17|UDP|QOTD Protokolü|
|19|UDP|CHARGEN Protokolü|
|22|TCP|SSH|
|23|TCP|Sun|
|53|UDP|DNS|
|69|UDP|TFTP|
|81|TCP|Kötü amaçlı olabilecek (TOR çıkış düğümü)|
|111|TCP/UDP|PRC|
|119|TCP|NNTP|
|123|UDP|NTP|
|135|TCP/UDP|Uç nokta Eşleyici; PRC DCE|
|137|TCP/UDP|NetBIOS ad hizmeti|
|138|TCP/UDP|NetBIOS Datagram hizmeti|
|139|TCP|NetBIOS oturum hizmeti|
|161|TCP/UDP|SNMP|
|162|TCP/UDP|SNMP|
|389|TCP|LDAP|
|445|TCP|SMB|
|512|TCP|Rexec|
|514|TCP|Uzak kabuk|
|593|TCP/UDP|HTTP RPC|
|636|TCP|LDAP|
|873|TCP|rsync|
|1433|TCP|MS SQL|
|1434|UDP|MS SQL|
|1900|UDP|SSDP|
|1900|UDP|SSDP|
|2049|TCP/UDP|NFS|
|2301|TCP|Compaq yönetim hizmeti|
|2323|TCP|3B-nfsd|
|2381|TCP|Compaq yönetim hizmeti|
|3268|TCP|LDAP|
|3306|TCP|MySQL|
|3389|TCP|RDP|
|4333|TCP|mSQL|
|5353|UDP|mDNS|
|5432|TCP|PostgreSQL|
|5555|TCP|Kişisel aracı; HP OmniBack|
|5800|TCP|VNC|
|5900|TCP|Uzak framebuffer; VNC|
|5900|TCP|VNC|
|5985|TCP|Windows PowerShell|
|5986|TCP|Windows PowerShell|
|6379|TCP|Redis|
|6379|TCP|Redis|
|7000|TCP|Cassandra|
|7001|TCP|Cassandra|
|7199|TCP|Cassandra|
|8081|TCP|CosmosDB Sun proxy Yöneticisi|
|8089|TCP|Splunk|
|8545|TCP|Kötü amaçlı olabilecek (Cryptominer)|
|9042|TCP|Cassandra|
|9160|TCP|Cassandra|
|9300|TCP|Elasticsearch|
|11211|UDP|Memcached|
|16379|TCP|Redis|
|26379|TCP|Redis|
|27017|TCP|MongoDB|
|37215|TCP|Kötü amaçlı olabilecek|
||||