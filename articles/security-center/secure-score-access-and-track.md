---
title: Azure Güvenlik Merkezi 'nde güvenli puanınızı izleme
description: Azure Güvenlik Merkezi 'nde güvenli puanınızı erişmenin ve izlemenin birden çok yolu hakkında bilgi edinin.
author: memildin
ms.author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 02/25/2021
ms.openlocfilehash: 5efc48d348e9cfceab590bcfba8c621e7721376f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102107962"
---
# <a name="access-and-track-your-secure-score"></a>Güvenli puanınızı erişin ve izleyin

Aşağıdaki bölümlerde açıklandığı gibi, genel olarak güvenli puanınızı ve abonelik başına puanınızı Azure portal veya program aracılığıyla aracılığıyla bulabilirsiniz:

> [!TIP]
> Puanlarınızın nasıl hesaplandığı hakkında ayrıntılı bir açıklama için bkz. [hesaplamalar-puanınızı anlama](secure-score-security-controls.md#calculations---understanding-your-score).

## <a name="get-your-secure-score-from-the-portal"></a>Portaldan güvenli puanınızı alın

Güvenlik Merkezi, puanınızı portalda göze çarpacak şekilde görüntüler: Güvenlik Merkezi 'ne genel bakış sayfası ilk ana kutucuğudur. Bu kutucuğu seçtiğinizde, puanı abonelik tarafından kesilen puanı görebileceğiniz adanmış güvenli puan sayfasına götürür. Önceliklendirilmiş önerilerin ayrıntılı listesini görmek için tek bir abonelik seçin ve aboneliğin puanına sahip olacağını belirten olası etkileri görebilirsiniz. 

Daha sonra, güvenli puanınız Güvenlik Merkezi 'nin Portal sayfalarında aşağıdaki konumlarda gösterilmiştir.

- Güvenlik Merkezi 'nin **genel bakış** (ana pano) kutucuğunda:

    :::image type="content" source="./media/secure-score-security-controls/score-on-main-dashboard.png" alt-text="Güvenlik Merkezi 'ndeki panoda güvenli puan":::

- Adanmış **güvenli Puanlama** sayfasında, aboneliğiniz ve yönetim gruplarınız için güvenli puanı görebilirsiniz:

    :::image type="content" source="./media/secure-score-security-controls/score-on-dedicated-dashboard.png" alt-text="Güvenlik Merkezi 'nin güvenli Puanlama sayfasında abonelikler için güvenli puan":::

    :::image type="content" source="./media/secure-score-security-controls/secure-score-management-groups.png" alt-text="Güvenlik Merkezi 'nin güvenli Puanlama sayfasında yönetim gruplarının güvenli puanı":::

    > [!NOTE]
    > Yeterli izinlere sahip olmadığınız herhangi bir yönetim grubu puanlarını "kısıtlı" olarak gösterir. 

- **Öneriler** sayfasının en üstünde:

    :::image type="content" source="./media/secure-score-security-controls/score-on-recommendations-page.png" alt-text="Güvenlik Merkezi 'nin öneriler sayfasında güvenli puan":::

## <a name="get-your-secure-score-from-the-rest-api"></a>REST API güvenli puanınızı alın

Puanınızı güvenli Puanlama API 'SI aracılığıyla erişebilirsiniz. API yöntemleri, verileri sorgulama ve zaman içinde güvenli Puanlarınızın kendi raporlama mekanizmanızı oluşturma esnekliğini sağlar. Örneğin, belirli bir aboneliğin Puanını almak için [güvenli puanlar API](/rest/api/securitycenter/securescores) 'sini kullanabilirsiniz. Ayrıca, güvenlik denetimlerini ve aboneliklerinizin geçerli Puanını listelemek için [güvenli puan DENETIMLERI API](/rest/api/securitycenter/securescorecontrols) 'sini de kullanabilirsiniz.

![API aracılığıyla tek bir güvenli puan alma](media/secure-score-security-controls/single-secure-score-via-api.png)

Güvenli Puanlama API 'sinin üzerine inşa eden araçların örnekleri için bkz. [GitHub topluluğumuz güvenli Puanlama alanı](https://github.com/Azure/Azure-Security-Center/tree/master/Secure%20Score). 

## <a name="get-your-secure-score-from-azure-resource-graph"></a>Azure Kaynak Grafınızdan güvenli puanınızı alın

Azure Kaynak Grafiği, güçlü filtreleme, gruplama ve sıralama özelliklerine sahip bulut ortamlarınızdaki kaynak bilgilerine anında erişim sağlar. Azure aboneliklerindeki bilgileri programlı olarak veya Azure portal içinden sorgulamak için hızlı ve verimli bir yoldur. [Azure Kaynak Grafiği hakkında daha fazla bilgi edinin](../governance/resource-graph/index.yml).

Azure Kaynak Graf ile birden çok aboneliğin güvenli puanına erişmek için:

1. Azure portal **Azure Kaynak Grafiği Gezgini**' ni açın.

    :::image type="content" source="./media/security-center-identity-access/opening-resource-graph-explorer.png" alt-text="Azure Kaynak Grafiği Gezgini * * öneri sayfası başlatılıyor" :::

1. Kusto sorgunuzu girin (kılavuz için aşağıdaki örnekleri kullanarak).

    - Bu sorgu, abonelik KIMLIĞINI, geçerli puanı punto ve yüzde olarak ve abonelik için en fazla puanı döndürür. 

        ```kusto
        SecurityResources 
        | where type == 'microsoft.security/securescores' 
        | extend current = properties.score.current, max = todouble(properties.score.max)
        | project subscriptionId, current, max, percentage = ((current / max)*100)
        ```

    - Bu sorgu tüm güvenlik denetimlerinin durumunu döndürür. Her denetim için, sağlıksız kaynak sayısını, geçerli puanı ve en fazla puanı elde edersiniz. 

        ```kusto
        SecurityResources 
        | where type == 'microsoft.security/securescores/securescorecontrols'
        | extend SecureControl = properties.displayName, unhealthy = properties.unhealthyResourceCount, currentscore = properties.score.current, maxscore = properties.score.max
        | project SecureControl , unhealthy, currentscore, maxscore
        ```

1. **Sorguyu Çalıştır**' ı seçin.


## <a name="tracking-your-secure-score-over-time"></a>Zaman içinde güvenli puanınızı izleme

### <a name="secure-score-over-time-report-in-workbooks-page"></a>Çalışma kitapları sayfasında zaman Içindeki güvenli puan raporu

Güvenlik Merkezi çalışma kitapları sayfası, aboneliklerinizin ve güvenlik denetimlerinin puanlarını görsel olarak izlemeye yönelik kullanıma yönelik bir rapor içerir. [Güvenlik Merkezi verilerinin zengin, etkileşimli raporları oluşturma](custom-dashboards-azure-workbooks.md)bölümünde daha fazla bilgi edinin.

:::image type="content" source="media/custom-dashboards-azure-workbooks/secure-score-over-time-snip.png" alt-text="Azure Güvenlik Merkezi 'nin çalışma kitapları galerisinden zaman içindeki güvenli puan raporunun bir bölümü":::

### <a name="power-bi-pro-dashboards"></a>Power BI Pro panoları

Pro hesabıyla Power BI bir Kullanıcı kullanıyorsanız, zaman içinde güvenli puanı izlemek ve tüm değişiklikleri araştırmak için **zaman Içinde güvenli puan** Power BI panosunu kullanabilirsiniz.

> [!TIP]
> GitHub 'daki Azure Güvenlik Merkezi topluluğunun ayrılmış alanında bu panoyu ve güvenli Puanlama ile program aracılığıyla çalışmaya yönelik diğer araçları bulabilirsiniz: https://github.com/Azure/Azure-Security-Center/tree/master/Secure%20Score

Pano, güvenlik durumunuzu çözümlemenize yardımcı olacak aşağıdaki iki raporu içerir:

- **Kaynaklar Özeti** -kaynaklarınızın sistem durumuyla ilgili özetlenen verileri sağlar.
- **Güvenli puan özeti** -puanınızın ilerleme durumuyla ilgili özetlenmiş veriler sağlar. Puandaki değişiklikleri görüntülemek için "abonelik başına süre içinde güvenli puan" grafiğini kullanın. Puanınızın önemli bir değişikliği fark ederseniz, değişikliğe neden olabilecek değişiklikler için "güvenli puanınızı etkileyebilecek algılanan değişiklikler" tablosunu kontrol edin. Bu tablo, önerilerden biri için silinen kaynakları, yeni dağıtılan kaynakları veya güvenlik durumunun değiştiği kaynakları gösterir.

:::image type="content" source="./media/secure-score-security-controls/power-bi-secure-score-dashboard.png" alt-text="Zamana göre güvenli puanınızı izlemek ve değişiklikleri araştırmak için zaman Içinde isteğe bağlı güvenli Puanlama Power BI panosu":::


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, güvenli puanınızın nasıl erişebileceği ve izleneceği açıklanır. İlgili malzemeler için aşağıdaki makalelere bakın:

- [Önerinin farklı öğeleri hakkında bilgi edinin](security-center-recommendations.md)
- [Önerileri nasıl düzelteceğinizi öğrenin](security-center-remediate-recommendations.md)
- [Güvenli puanla programlama yoluyla çalışmaya yönelik GitHub tabanlı araçları görüntüleyin](https://github.com/Azure/Azure-Security-Center/tree/master/Secure%20Score)