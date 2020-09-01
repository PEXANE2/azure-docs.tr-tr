---
title: Azure Güvenlik Merkezi 'nin varlık envanteri
description: Azure Güvenlik Merkezi 'nin varlık yönetimi deneyimi hakkında bilgi edinin ve tüm güvenlik merkezi tarafından izlenen kaynaklarınızın tamamına tam görünürlük sağlar.
author: memildin
manager: rkarlin
services: security-center
ms.author: memildin
ms.date: 08/11/2020
ms.service: security-center
ms.topic: conceptual
ms.openlocfilehash: f3a542cd62c3d593dbc0cce7982d47222e9a7c88
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89181125"
---
# <a name="explore-and-manage-your-resources-with-asset-inventory-and-management-tools"></a>Varlık envanteri ve yönetim araçlarıyla kaynaklarınızı bulun ve yönetin

Azure Güvenlik Merkezi 'nin varlık Envanteri sayfası, güvenlik merkezi 'ne bağladığınız kaynakların güvenlik duruşunu görüntülemek için tek bir sayfa sağlar. 

Güvenlik Merkezi, olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli olarak analiz eder. Daha sonra bu güvenlik açıklarını nasıl düzeltebileceğiniz konusunda öneriler sağlar.

Herhangi bir kaynakta bekleyen öneriler olduğunda, bu değişiklikler envanterde görüntülenir.

Bu görünümü ve bu soruları şu şekilde ele almak için kullanın:

- Standart katman aboneliklerimin hangi önerileri içerdiğinden emin misiniz?
- ' Production ' etiketine sahip makinelerimin Log Analytics Aracısı yok mu?
- Belirli bir etiketle etiketlenmiş olan makinelerimin sayısı, bekleyen önerilere sahip mi?
- Belirli bir kaynak grubundaki kaç kaynağın güvenlik açığı değerlendirme hizmetinden güvenlik bulguları var?

Bu aracın varlık yönetimi olanakları önemli ölçüde artar ve büyümeye devam eder. 


## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Önizleme|
|Fiyat|Ücretsiz|
|Gerekli roller ve izinler:|Tüm kullanıcılar|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![No](./media/icons/no-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||


## <a name="what-are-the-key-features-of-asset-inventory"></a>Varlık envanterinin temel özellikleri nelerdir?

Envanter sayfası aşağıdaki araçları sağlar:

- **Özetler** -herhangi bir filtre tanımladıktan önce, envanter görünümünün en üstündeki belirgin bir değer şeridi şunları gösterir:

    - **Toplam kaynak**: Güvenlik Merkezi 'ne bağlı kaynakların toplam sayısı.
    - **Sağlıksız kaynaklar**: etkin güvenlik önerilerine sahip kaynaklar. [Güvenlik önerileri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/security-center/security-center-recommendations).
    - **İzlenmeyen kaynaklar**: aracı izleme sorunları olan kaynaklar-Log Analytics Aracısı dağıtılır, ancak aracı veri göndermiyor veya başka sistem durumu sorunlarına sahip değildir.

- **Filtreler** -sayfanın en üstündeki birden çok filtre, yanıtlamaya çalıştığınız soruya göre kaynak listesini hızlı bir şekilde iyileştirmek için bir yol sağlar. Örneğin, *' Production ' etiketiyle makinelerimin Log Analytics Aracısı eksik* olduğu sorusuna yanıt vermek istiyorsanız, aşağıdaki küçük resimde gösterildiği gibi, **Aracı izleme** filtresini **Etiketler** filtresiyle birleştirebilirsiniz:

    ![İzlenmeyen üretim kaynaklarına filtreleme](./media/asset-inventory/filtering-to-prod-unmonitored.gif)

    Filtreleri uyguladıktan hemen sonra Özet değerleri sorgu sonuçlarıyla ilişkilendirilecek şekilde güncelleştirilir. 

- **Dışarı aktarma seçenekleri** -Inventory Seçili filtre seçeneklerinizin SONUÇLARıNı bir CSV dosyasına aktarma seçeneği sağlar. Ayrıca, KQL sorgusunu daha da belirginleştirmek, kaydetmek veya değiştirmek için sorgunun kendisini Azure Kaynak grafik Gezgini ' ne dışarı aktarabilirsiniz.

    ![Stokun dışarı aktarma seçenekleri](./media/asset-inventory/inventory-export-options.png)

    > [!TIP]
    > KQL belgeleri, dil için "fikir" almak için bazı basit sorgularla birlikte bazı örnek verilerle birlikte bir veritabanı sağlar. [Bu KQL öğreticisinde daha fazla bilgi edinin](https://docs.microsoft.com/azure/data-explorer/kusto/query/tutorial?pivots=azuredataexplorer).

- **Varlık yönetimi seçenekleri** -envanter, karmaşık bulma sorguları gerçekleştirmenize olanak tanır. Sorgularınızla eşleşen kaynakları bulduğunuz stok, şu gibi işlemler için kısayollar sağlar:

    - Filtrelenmiş kaynaklara Etiketler atama-etiketlemek istediğiniz kaynakların yanındaki onay kutularını seçin
    - Yeni sunucuları güvenlik merkezi 'ne **ekleme-Azure dışı sunucular Ekle** araç çubuğu düğmesini kullanma


## <a name="how-does-asset-inventory-work"></a>Varlık envanteri nasıl çalışır?

Varlık envanteri, güvenlik merkezi 'nin güvenlik sonrası verilerini birden çok aboneliğe sorgulama yeteneği sağlayan bir Azure hizmeti olan [Azure Kaynak Grafiği 'ni (arg)](https://docs.microsoft.com/azure/governance/resource-graph/)kullanır.

Bağımsız değişken, ölçeklendirerek sorgulama özelliği sayesinde etkili kaynak araştırması sağlamak üzere tasarlanmıştır.

Varlık envanteri, [kusto sorgu dilini (KQL)](https://docs.microsoft.com/azure/data-explorer/kusto/query/)kullanarak, diğer kaynak özellikleriyle ASC verileri çapraz bir şekilde vererek derin Öngörüler elde edebilir.


## <a name="how-to-use-asset-inventory"></a>Varlık envanterini kullanma

1. Güvenlik Merkezi 'nin kenar çubuğundan **Envanter**' ı seçin.

1. İsteğe bağlı olarak, belirli bir kaynağı göstermek için ada **göre filtrele** kutusuna adı girin.

1. Yapmak istediğiniz belirli bir sorguyu oluşturmak için filtrelerdeki ilgili seçenekleri seçin.

    ![Stokun filtreleri](./media/asset-inventory/inventory-filters.png)

    Varsayılan olarak, kaynaklar etkin güvenlik önerisi sayısına göre sıralanır.

    > [!IMPORTANT]
    > Her filtredeki seçenekler şu anda seçili olan aboneliklerdeki kaynaklara **ve** diğer filtrelerdeki seçimlerinize özgüdür.
    >
    > Örneğin, yalnızca bir abonelik seçtiyseniz ve aboneliğin (0 sağlıksız kaynak) düzeltilmesi için bekleyen güvenlik önerilerine sahip hiçbir kaynağı yoksa, **öneriler** filtresi hiçbir seçeneğe sahip olmayacaktır. 

1. **Güvenlik bulgularını filtre içeriyor** olarak kullanmak için, etkilenen kaynaklara filtre uygulamak için bir GÜVENLIK açığının kimlik, güvenlik denetımı veya CVE adından ücretsiz metin girin:

    !["Güvenlik bulguları" filtre içeriyor](./media/asset-inventory/security-findings-contain-elements.png)

    > [!TIP]
    > **Güvenlik bulguları içerir** ve **Etiketler** filtreleri yalnızca tek bir değeri kabul eder. Birden fazla filtrelemek için, filtre **Ekle**' yi kullanın.

1. **Fiyatlandırma katmanı** filtresini kullanmak için bir veya daha fazla seçenek (ücretsiz, kısmi veya standart) seçin:

    - Ücretsiz fiyatlandırma katmanında yer alan **ücretsiz** kaynaklar
    - Standart fiyatlandırma katmanındaki **Standart** kaynaklar
    - **Kısmi** -bu, standart fiyatlandırma katmanındaki ancak bazı isteğe bağlı güvenlik planlarının devre dışı bırakıldığı abonelikler için geçerlidir. Örneğin, aşağıdaki abonelik Standart katmanda, ancak standart katmanın beş öğesi devre dışı bırakıldı. 

        ![Standart (kısmi) fiyatlandırma katmanında abonelik](./media/asset-inventory/pricing-tier-partial.png)

1. Sorgunuzun sonuçlarını daha ayrıntılı incelemek için ilgilendiğiniz kaynakları seçin.

1. İsteğe bağlı olarak, kaynak Graph Explorer 'da sorguyu açmak için **Kaynak Grafiği Gezgininde görüntüle** ' yi seçin.

    ![Bağımsız değişken içinde envanter sorgusu](./media/asset-inventory/inventory-query-in-resource-graph-explorer.png)

1. Bazı filtreler tanımladıysanız ve sayfayı açık bıraktıysanız, Güvenlik Merkezi, sonuçları otomatik olarak güncelleştirmez. Sayfayı el ile yeniden yüklemediğiniz veya **Yenile**seçeneğini belirleyemediğiniz takdirde kaynaklardaki değişiklikler, görüntülenecek sonuçları etkilemez.


## <a name="faq---inventory"></a>SSS-envanter

### <a name="why-arent-all-of-my-subscriptions-machines-storage-accounts-etc-shown"></a>Neden Aboneliklerim, makineler, depolama hesapları vb. gösterilmemektedir?

Envanter görünümü, bir bulut güvenlik sonrası yönetimi (CSPM) perspektifinden kaynaklarınızı listeler. Filtreler ortamınızdaki her kaynağı döndürmez; yalnızca bekleyen (veya ' etkin ') önerilere sahip olanlar. 

Örneğin, dokuz aboneliğiniz varsa ancak şu anda yalnızca sekiz öneri varsa, **kaynak türü = abonelikler** 'e göre filtreleyerek yalnızca etkin önerilere sahip sekiz aboneliği görürsünüz:

![Etkin öneri olmadığında tüm alt öğeleri döndürülmedi](./media/asset-inventory/filtered-subscriptions-some.png)


### <a name="why-do-some-of-my-resources-show-blank-values-in-the-pricing-or-agent-monitoring-columns"></a>Kaynaklarım neden fiyatlandırma veya aracı izleme sütunlarında boş değerler gösteriyor?

Güvenlik Merkezi tarafından izlenen tüm kaynakların aracıları yoktur. Örneğin, Azure depolama hesapları veya diskler, Logic Apps, Data Lake Analizi ve Olay Hub 'ı gibi PaaS kaynakları.

Fiyatlandırma veya aracı izleme bir kaynak için uygun olmadığında, bu envanter sütunlarında hiçbir şey gösterilmez.

![Bazı kaynaklar, aracı izleme veya fiyatlandırma sütunlarında boş bilgi gösterir](./media/asset-inventory/agent-pricing-blanks.png)



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Güvenlik Merkezi 'nin varlık Envanteri sayfası açıklanmaktadır.

İlgili araçlar hakkında daha fazla bilgi için aşağıdaki sayfalara bakın:

- [Azure Kaynak Grafiği (ARG)](https://docs.microsoft.com/azure/governance/resource-graph/)

- [Kusto Sorgu Dili (KQL)](https://docs.microsoft.com/azure/data-explorer/kusto/query/)