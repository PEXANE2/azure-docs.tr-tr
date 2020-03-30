---
title: SQL bilgi korumayı özelleştirin - Azure Güvenlik Merkezi
description: Azure Güvenlik Merkezi'nde bilgi koruma ilkelerini nasıl özelleştirişleri öğrenin.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 2ebf2bc7-232a-45c4-a06a-b3d32aaf2500
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/29/2019
ms.author: memildin
ms.openlocfilehash: 9c776a32b4a35c72fc40a16afb87db9896a763cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75611075"
---
# <a name="customize-the-sql-information-protection-policy-in-azure-security-center-preview"></a>Azure Güvenlik Merkezi'nde SQL bilgi koruma ilkesini özelleştirme (Önizleme)
 
Azure Güvenlik Merkezi'nde tüm Azure kiracınız için bir SQL bilgi koruma ilkesi tanımlayabilir ve özelleştirebilirsiniz.

Bilgi koruması, Azure veri kaynaklarınızdaki hassas verileri keşfetmek, sınıflandırmak, etiketlemek ve raporlamak için gelişmiş bir güvenlik yeteneğidir. En hassas verilerinizi (iş, finans, sağlık, kişisel veriler, vb.) keşfetmek ve sınıflandırmak, organizasyonel bilgi koruma durumunuzda önemli bir rol oynayabilir. Şunlara altyapı sağlayabilir:
- Veri gizliliği standartlarının ve mevzuata uygunluk gerekliliklerinin karşılatına yardımcı olmak
- Hassas verilere anormal erişim konusunda izleme (denetleme) ve uyarı gibi güvenlik senaryoları
- Son derece hassas veriler içeren veri depolarının güvenliğini kontrol etme ve sertleştirme
 
[SQL Bilgi Koruması,](../sql-database/sql-database-data-discovery-and-classification.md) şu anda Azure SQL Veritabanı için desteklenen SQL veri depolarınız için bu paradigmayı uygular. SQL Bilgi Koruması, hassas olabilecek verileri otomatik olarak keşfeder ve sınıflar, hassas verileri sınıflandırma özellikleriyle sürekli etiketlemek için bir etiketleme mekanizması sağlar ve veritabanının sınıflandırma durumu. Ayrıca, sql sorgularının sonuç kümesi duyarlılığını hesaplar, böylece hassas verileri ayıklayan sorgular açıkça denetlenebilir ve veriler korunabilir. SQL Bilgi Koruması hakkında daha fazla bilgi için [Azure SQL Veritabanı Veri Bulma ve Sınıflandırma'ya](../sql-database/sql-database-data-discovery-and-classification.md)bakın.
 
Sınıflandırma mekanizması, sınıflandırma taksonomisi - **Etiketler** ve Bilgi **Türleri**oluşturan iki birincil yapıya dayanmaktadır.
- **Etiketler** – Sütunda depolanan verilerin duyarlılık düzeyini tanımlamak için kullanılan ana sınıflandırma öznitelikleri. 
- **Bilgi Türleri** – Sütunda depolanan veri türüne ek parçalılık sağlar.
 
Bilgi Koruması, varsayılan olarak kullanılan yerleşik bir etiket ve bilgi türleri kümesiyle birlikte gelir. Bu etiketleri ve türleri özelleştirmek için Güvenlik Merkezi'ndeki bilgi koruma ilkesini özelleştirebilirsiniz.
 
## <a name="customize-the-information-protection-policy"></a>Bilgi koruma ilkesini özelleştirme
Azure kiracınız için bilgi koruma ilkesini özelleştirmek için [kiracının kök yönetim grubunda yönetim ayrıcalıklarına](security-center-management-groups.md)sahip olmanız gerekir. 
 
1. Güvenlik Merkezi ana menüsünde, **KAYNAK GÜVENLİğİ HIJYENi** altında **Veri & depolama** gidin ve SQL Bilgi **Koruma** düğmesine tıklayın.

   ![Bilgi koruma ilkesini yapılandırma](./media/security-center-info-protection-policy/security-policy.png) 
 
2. SQL **Bilgi Koruması** sayfasında, geçerli etiket kümenizi görüntüleyebilirsiniz. Bunlar, verilerinizin duyarlılık düzeyini kategorilere ayırmak için kullanılan ana sınıflandırma öznitelikleridir. Buradan, kiracı için **Bilgi koruma etiketlerini** ve **Bilgi türlerini** yapılandırabilirsiniz. 
 
### <a name="customizing-labels"></a>Etiketleri özelleştirme
 
1. Varolan herhangi bir etiketi düzenleyebilir veya silebilir veya yeni bir etiket ekleyebilirsiniz. Varolan bir etiketi düzeltmek için, bu etiketi seçin ve sonra üstteki veya sağdaki bağlam menüsünden **Yapılandır'ı**tıklatın. Yeni bir etiket eklemek için, üst menü çubuğunda veya etiketler tablosunun alt kısmında **etiket oluştur'u** tıklatın.
2. **Yapılduyarlılık etiketi** ekranında, etiket adını ve açıklamasını oluşturabilir veya değiştirebilirsiniz. **Etkin** anahtarı açıp kapamak için etiketin etkin mi yoksa devre dışı mı olduğunu da ayarlayabilirsiniz. Son olarak, etiketle ilişkili bilgi türleri ekleyebilir veya kaldırabilirsiniz. Bu bilgi türüyle eşleşen tüm veriler, sınıflandırma önerilerinde ilişkili duyarlılık etiketini otomatik olarak içerir.
3. **Tamam**'a tıklayın.
 
   ![Duyarlılık etiketini yapılandırma](./media/security-center-info-protection-policy/config-sensitivity-label.png)
 
4. Etiketler artan duyarlılık sırasına göre listelenir. Etiketler arasındaki sıralamayı değiştirmek için, etiketleri tabloda yeniden sıralamak için sürükleyin veya sırayı değiştirmek için **Yukarı ve** Aşağı **Taşı** düğmelerini kullanın. 
 
    ![Bilgi koruma ilkesini yapılandırma](./media/security-center-info-protection-policy/move-up.png)
 
5. Bittiğinde ekranın üst kısmında **Kaydet'i** tıklattığından emin olun.
 
 
## <a name="adding-and-customizing-information-types"></a>Bilgi türleri ekleme ve özelleştirme
 
1. **Bilgi türlerini yönet'e**tıklayarak bilgi türlerini yönetebilir ve özelleştirebilirsiniz.
2. Yeni bir **Bilgi türü**eklemek için üst menüde **bilgi türü oluştur'u** seçin. **Bilgi türü**için bir ad, açıklama ve arama deseni dizelerini yapılandırabilirsiniz. Arama deseni dizeleri isteğe bağlı olarak joker karaktere sahip anahtar kelimeleri (sütunların meta verilerini temel alarak, otomatik bulma altyapısının veritabanlarınızdaki hassas verileri tanımlamak için kullandığı '%' karakterini kullanarak) kullanabilir.
 
    ![Bilgi koruma ilkesini yapılandırma](./media/security-center-info-protection-policy/info-types.png)
 
3. Ayrıca, ek arama desen dizeleri ekleyerek, varolan dizeleri bazı devre dışı bırakarak veya açıklama değiştirerek yerleşik **Bilgi türleri** yapılandırabilirsiniz. Yerleşik **Bilgi türlerini** silemez veya adlarını düzenleyemezsiniz. 
4. **Bilgi türleri** artan bulma sıralaması sırasına göre listelenir, yani listedeki daha yüksek türler önce eşleşmeyi deneyecektir. Bilgi türleri arasındaki sıralamayı değiştirmek için, türleri tablodaki doğru noktaya sürükleyin veya sırayı değiştirmek için **Yukarı ve** Aşağı **Taşı** düğmelerini kullanın. 
5. Bittiğinde **Tamam'ı** tıklatın.
6. Bilgi türlerinizi yönetmeyi tamamladıktan sonra, belirli bir etiket için **Yapılandır'ı** tıklatarak ve uygun şekilde bilgi türleri ekleyerek veya silerek ilgili türleri ilgili etiketlerle ilişkilendirdiğinden emin olun.
7. Tüm değişikliklerinizi uygulamak için ana **Etiketler** çubuğunda **Kaydet'i** tıklattığınızdan emin olun.
 
Bilgi koruma politikanız tam olarak tanımlandıktan ve kaydedildikten sonra, kiracınızdaki tüm Azure SQL veritabanlarındaki verilerin sınıflandırılması için geçerli olacaktır.
 
 
## <a name="next-steps"></a>Sonraki adımlar
 
Bu makalede, Azure Güvenlik Merkezi'nde bir SQL Bilgi Koruma ilkesi tanımlamayı öğrendiniz. SQL veritabanlarınızdaki hassas verileri sınıflandırmak ve korumak için SQL Bilgi Koruması'nı kullanma hakkında daha fazla bilgi edinmek için [Bkz.](../sql-database/sql-database-data-discovery-and-classification.md) 

Azure Güvenlik Merkezi'nde güvenlik ilkeleri ve veri güvenliği hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
 
- [Azure Güvenlik Merkezi'nde güvenlik ilkeleri belirleme:](tutorial-security-policy.md)Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini nasıl yapılandırıştıracaklarınızı öğrenin
- [Azure Güvenlik Merkezi veri güvenliği](security-center-data-security.md): Güvenlik Merkezi'nin verileri nasıl yönettiğini ve koruduğuhakkında bilgi edinin
