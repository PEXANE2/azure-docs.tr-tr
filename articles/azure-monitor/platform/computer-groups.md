---
title: Azure Izleyici günlük sorgularıyla bilgisayar grupları | Microsoft Docs
description: Azure Izleyici 'de bilgisayar grupları, günlük sorgularını belirli bir bilgisayar kümesiyle kapsamlarınıza izin verir.  Bu makalede, bilgisayar grupları oluşturmak için kullanabileceğiniz farklı yöntemler ve bunların bir günlük sorgusunda nasıl kullanılacağı açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/05/2019
ms.openlocfilehash: a005b6cec811b8a584123dc4c8abab77766961e0
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79274781"
---
# <a name="computer-groups-in-azure-monitor-log-queries"></a>Azure Izleyici günlük sorgularının bilgisayar grupları
Azure Izleyici 'de bilgisayar grupları, [günlük sorgularını](../log-query/log-query-overview.md) belirli bir bilgisayar kümesiyle kapsamlarınıza izin verir.  Her Grup ya da tanımladığınız bir sorgu kullanarak bilgisayarları veya grupları alarak farklı kaynaktaki doldurulur.  Grup bir günlük sorgusuna eklendiğinde, sonuçlar gruptaki bilgisayarlarla eşleşen kayıtlarla sınırlıdır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="creating-a-computer-group"></a>Bir bilgisayar grubu oluşturuluyor
Aşağıdaki tablodaki yöntemlerden birini kullanarak Azure Izleyici 'de bir bilgisayar grubu oluşturabilirsiniz.  Aşağıdaki bölümlerde her yöntemi hakkında ayrıntılı bilgi sağlanır. 

| Yöntem | Açıklama |
|:--- |:--- |
| Günlük sorgusu |Bilgisayarların listesini döndüren bir günlük sorgusu oluşturun. |
| Günlük Arama API’si |Günlük sorgusunun sonuçlarına göre program aracılığıyla bir bilgisayar grubu oluşturmak için günlük araması API 'sini kullanın. |
| Active Directory |Active Directory bir etki alanının üyesi olan herhangi bir aracı bilgisayarın Grup üyeliğini otomatik olarak tarayın ve her güvenlik grubu için Azure Izleyici 'de bir grup oluşturun. (Yalnızca Windows makineler)|
| Configuration Manager | Koleksiyonları Microsoft uç nokta Configuration Manager içeri aktarın ve Azure Izleyici 'de her biri için bir grup oluşturun. |
| Windows Server Update Services |Grupları hedeflemek için WSUS sunucularını veya istemcilerini otomatik olarak tarayın ve her biri için Azure Izleyici 'de bir grup oluşturun. |

### <a name="log-query"></a>Günlük sorgusu
Bir günlük sorgusundan oluşturulan bilgisayar grupları, tanımladığınız bir sorgu tarafından döndürülen tüm bilgisayarları içerir.  Bu sorguyu bilgisayar grubu grubun oluşturulmasından bu yana değişiklikler yansıtılır böylece her kullanılışında çalıştırılır.  

Bilgisayar grubu için herhangi bir sorgu kullanabilirsiniz, ancak `distinct Computer`kullanarak farklı bir bilgisayar kümesi döndürmesi gerekir.  Aşağıda, bilgisayar grubu olarak kullanabileceğiniz tipik örnek bir sorgu verilmiştir.

    Heartbeat | where Computer contains "srv" | distinct Computer

Azure portalında günlük araması bir bilgisayar grubu oluşturmak için aşağıdaki yordamı kullanın.

1. Azure portal **Azure izleyici** menüsünde **Günlükler** ' e tıklayın.
1. Grupta istediğiniz bilgisayarları döndüren bir sorgu oluşturun ve çalıştırın.
1. Ekranın en üstündeki **Kaydet** ' e tıklayın.
1. **Farklı kaydet** **işlevini işlevine** değiştirin ve **Bu sorguyu bir bilgisayar grubu olarak kaydet**' i seçin.
1. Tabloda açıklanan bilgisayar grubu için her bir özellik için değerler sağlayın ve **Kaydet**' e tıklayın.

Aşağıdaki tabloda, bir bilgisayar grubu tanımlayan özellikleri açıklanmaktadır.

| Özellik | Açıklama |
|:---|:---|
| Adı   | Portalda görüntülenecek sorgunun adı. |
| İşlev diğer adı | Bir sorguda bilgisayar grubu tanımlamak için kullanılan benzersiz bir diğer ad. |
| Kategori       | Portaldaki sorguları düzenlemek için kategori. |


### <a name="active-directory"></a>Active Directory
Azure Izleyici 'yi Active Directory grup üyeliklerini içeri aktarmak üzere yapılandırdığınızda, Windows etki alanına katılmış herhangi bir bilgisayarın Grup üyeliğini Log Analytics aracısıyla analiz eder.  Active Directory içindeki her güvenlik grubu için Azure Izleyici 'de bir bilgisayar grubu oluşturulur ve her Windows bilgisayarı, üyesi oldukları güvenlik gruplarına karşılık gelen bilgisayar gruplarına eklenir.  Bu üyelik sürekli olarak 4 saatte bir güncelleştirilir.  

> [!NOTE]
> İçeri aktarılan Active Directory grupları yalnızca Windows makinelerini içerir.

Azure Izleyici 'yi, Azure portal Log Analytics çalışma alanınızdaki **Gelişmiş ayarlardan** Active Directory güvenlik gruplarını içeri aktaracak şekilde yapılandırırsınız.  **Bilgisayar grupları**' nı seçin, **Active Directory**ve sonra **Active Directory grup üyeliklerini bilgisayarlardan içeri aktarın**.  Başka bir yapılandırma işlemi gerekmez.

![Bilgisayar grupları Active Directory'den](media/computer-groups/configure-activedirectory.png)

Grupları içeri aktardığınızda menü grubu üyeliği algılanan bilgisayarların sayısını ve içe aktarılan gruplarının sayısını listeler.  Bu bağlantılardan birine tıklayarak **Computergroup** kayıtlarını bu bilgilerle döndürebilirsiniz.

### <a name="windows-server-update-service"></a>Windows Server Update Service
Azure Izleyici 'yi WSUS grup üyeliklerini içeri aktarmak üzere yapılandırdığınızda, Log Analytics aracısına sahip bilgisayarların hedefleme grubu üyeliğini analiz eder.  İstemci tarafı hedefleme kullanıyorsanız, Azure Izleyici 'ye bağlı olan ve herhangi bir WSUS hedefleme grubunun parçası olan herhangi bir bilgisayar, Grup üyeliğini Azure Izleyici 'ye içeri aktarmıştır. Sunucu tarafı hedefleme kullanıyorsanız, Grup üyeliği bilgilerinin Azure Izleyici 'ye aktarılması için Log Analytics aracısının WSUS sunucusunda yüklü olması gerekir.  Bu üyelik sürekli olarak 4 saatte bir güncelleştirilir. 

Azure Izleyici 'yi, Azure portal Log Analytics çalışma alanınızdaki **Gelişmiş AYARLARDAN** WSUS gruplarını içeri aktaracak şekilde yapılandırırsınız.  **Bilgisayar grupları**, **WSUS**ve ardından **WSUS grup üyeliklerini içeri aktar**' ı seçin.  Başka bir yapılandırma işlemi gerekmez.

![WSUS bilgisayar grupları](media/computer-groups/configure-wsus.png)

Grupları içeri aktardığınızda menü grubu üyeliği algılanan bilgisayarların sayısını ve içe aktarılan gruplarının sayısını listeler.  Bu bağlantılardan birine tıklayarak **Computergroup** kayıtlarını bu bilgilerle döndürebilirsiniz.

### <a name="configuration-manager"></a>Configuration Manager
Azure Izleyicisini Configuration Manager koleksiyon üyeliklerini içeri aktaracak şekilde yapılandırdığınızda, her koleksiyon için bir bilgisayar grubu oluşturur.  Koleksiyon üyeliği bilgilerini, bilgisayar gruplarını güncel kalmasını sağlamak için 3 saatte alınır. 

Configuration Manager koleksiyonlarını içeri aktarabilmeniz [için önce Configuration Manager Azure izleyici 'ye bağlamanız](collect-sccm.md)gerekir.  

![SCCM bilgisayar grupları](media/computer-groups/configure-sccm.png)

Koleksiyonları içeri aktardığınızda menü grubu üyeliği algılanan bilgisayarların sayısını ve içe aktarılan gruplarının sayısını listeler.  Bu bağlantılardan birine tıklayarak **Computergroup** kayıtlarını bu bilgilerle döndürebilirsiniz.

## <a name="managing-computer-groups"></a>Bilgisayar gruplarını yönetme
Bir günlük sorgusundan oluşturulmuş bilgisayar gruplarını veya Azure portal Log Analytics çalışma alanınızdaki **Gelişmiş ayarlardan** günlük araması API 'sini görüntüleyebilirsiniz.  **Bilgisayar grupları** ' nı ve ardından **kayıtlı gruplar**' ı seçin.  

Bilgisayar grubunu silmek için **Kaldır** sütunundaki **x** simgesini tıklatın.  Grubun üyelerini döndüren grubun günlük aramasını çalıştırmak için bir grubun **üyeleri görüntüle** simgesine tıklayın.  Bir bilgisayar grubu değiştirilemez, ancak bunun yerine gerekir silin ve değiştirilen ayarlarla yeniden oluşturun.

![Kayıtlı bilgisayar grupları](media/computer-groups/configure-saved.png)


## <a name="using-a-computer-group-in-a-log-query"></a>Bir günlük sorgusunda bilgisayar grubu kullanma
Bir sorgudaki bir günlük sorgusundan oluşturulmuş bir bilgisayar grubunu, diğer adını bir işlev olarak düşünerek, genellikle aşağıdaki sözdizimi ile kullanabilirsiniz:

  `Table | where Computer in (ComputerGroup)`

Örneğin, aşağıdaki mycomputergroup adlı bir bilgisayar grubunda yalnızca bilgisayarlar için UpdateSummary kayıtları döndürmek için kullanabilirsiniz.
 
  `UpdateSummary | where Computer in (mycomputergroup)`


İçeri aktarılan bilgisayar grupları ve dahil edilen bilgisayarları **Computergroup** tablosunda depolanır.  Örneğin, aşağıdaki sorguyu bilgisayarların listesini etki alanı bilgisayarları grubunda Active Directory'den döndürecektir. 

  `ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "Domain Computers" | distinct Computer`

Aşağıdaki sorguda UpdateSummary kayıtları yalnızca bilgisayarlar için etki alanı bilgisayarları döndürür.

  ```
  let ADComputers = ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "Domain Computers" | distinct Computer;
  UpdateSummary | where Computer in (ADComputers)
  ```




## <a name="computer-group-records"></a>Bilgisayar Grup kaydı
Active Directory veya WSUS oluşturulan her bilgisayar grup üyeliğini için Log Analytics çalışma alanında bir kayıt oluşturulur.  Bu kayıtlar bir **bilgisayar grubu** türüne sahiptir ve aşağıdaki tabloda bulunan özelliklere sahiptir.  Günlük sorgularına dayalı bilgisayar grupları için kayıtlar oluşturulmaz.

| Özellik | Açıklama |
|:--- |:--- |
| `Type` |*ComputerGroup* |
| `SourceSystem` |*Dir* |
| `Computer` |Üye bilgisayar adıdır. |
| `Group` |Grubun adı. |
| `GroupFullName` |Kaynak ve kaynak adı da dahil olmak üzere grubun tam yolu. |
| `GroupSource` |Bu grup kaynak gelen toplanmış. <br><br>Active Directory<br>WSUS<br>WSUSClientTargeting |
| `GroupSourceName` |Grup toplandığı kaynağının adı.  Active Directory'de, bu etki alanı adıdır. |
| `ManagementGroupName` |SCOM aracıları için yönetim grubunun adı.  Diğer aracılar için bu, AOI-\<çalışma alanı KIMLIĞIDIR\> |
| `TimeGenerated` |Tarih ve saat bilgisayar grubu oluşturulduğunda veya güncelleştirildiğinde. |

## <a name="next-steps"></a>Sonraki adımlar
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin.  

