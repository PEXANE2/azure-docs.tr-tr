---
title: Azure Monitor günlük sorgularında bilgisayar grupları | Microsoft Dokümanlar
description: Azure Monitor'daki bilgisayar grupları, belirli bir bilgisayar kümesine günlük sorgularını kapsama almanızı sağlar.  Bu makalede, bilgisayar grupları oluşturmak için kullanabileceğiniz farklı yöntemler ve bunları günlük sorgusunda nasıl kullanacağınız açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/05/2019
ms.openlocfilehash: a005b6cec811b8a584123dc4c8abab77766961e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274781"
---
# <a name="computer-groups-in-azure-monitor-log-queries"></a>Azure Monitor günlük sorgularında bilgisayar grupları
Azure Monitor'daki bilgisayar grupları, belirli bir bilgisayar kümesine [günlük sorgularını](../log-query/log-query-overview.md) kapsama almanızı sağlar.  Her grup, tanımladığınız bir sorguyu kullanarak veya farklı kaynaklardan gruplar alarak bilgisayarlarla doldurulur.  Grup bir günlük sorgusuna dahil edildiğinde, sonuçlar gruptaki bilgisayarlarla eşleşen kayıtlarla sınırlıdır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="creating-a-computer-group"></a>Bilgisayar grubu oluşturma
Aşağıdaki tablodaki yöntemlerden herhangi birini kullanarak Azure Monitor'da bir bilgisayar grubu oluşturabilirsiniz.  Her yöntemle ilgili ayrıntılar aşağıdaki bölümlerde verilmiştir. 

| Yöntem | Açıklama |
|:--- |:--- |
| Günlük sorgusu |Bilgisayarların listesini döndüren bir günlük sorgusu oluşturun. |
| Günlük Arama API’si |Günlük sorgusunun sonuçlarına göre programlı bir bilgisayar grubu oluşturmak için Günlük Arama API'sını kullanın. |
| Active Directory |Etkin Dizin etki alanının üyesi olan tüm aracı bilgisayarların grup üyeliğini otomatik olarak tarayabilirsiniz ve her güvenlik grubu için Azure Monitor'da bir grup oluşturun. (Yalnızca Windows makineleri)|
| Configuration Manager | Koleksiyonları Microsoft Endpoint Configuration Manager'dan alSın ve her biri için Azure Monitor'da bir grup oluşturun. |
| Windows Server Update Services |Hedefleme grupları için WSUS sunucularını veya istemcilerini otomatik olarak tarayanın ve her biri için Azure Monitor'da bir grup oluşturun. |

### <a name="log-query"></a>Günlük sorgusu
Günlük sorgusundan oluşturulan bilgisayar grupları, tanımladığınız bir sorgu tarafından döndürülen tüm bilgisayarları içerir.  Bu sorgu, bilgisayar grubu her kullanıldığında çalıştırılır, böylece grup oluşturulduğundan sonraki değişiklikler yansıtılır.  

Bir bilgisayar grubu için herhangi bir sorgu kullanabilirsiniz, ancak kullanarak `distinct Computer`bilgisayarların farklı bir dizi döndürmeniz gerekir.  Aşağıda, bilgisayar grubu olarak kullanabileceğiniz tipik bir örnek sorgu verilmiştir.

    Heartbeat | where Computer contains "srv" | distinct Computer

Azure portalındaki günlük aramasından bir bilgisayar grubu oluşturmak için aşağıdaki yordamı kullanın.

1. Azure portalındaki **Azure Monitörü** menüsünde **Günlükler'i** tıklatın.
1. Grupta istediğiniz bilgisayarları döndüren bir sorgu oluşturun ve çalıştırın.
1. Ekranın üst kısmında **Kaydet'i** tıklatın.
1. **İşlev** **olarak Kaydet'i** değiştirin ve **bu sorguyı bilgisayar grubu olarak kaydet'** i seçin' i seçin.
1. Tabloda açıklanan bilgisayar grubu için her özellik için değerler sağlayın ve **Kaydet'i**tıklatın.

Aşağıdaki tabloda bir bilgisayar grubu tanımlayan özellikleri açıklanmaktadır.

| Özellik | Açıklama |
|:---|:---|
| Adı   | Portalda görüntülenecek sorgunun adı. |
| İşlev takma adı | Sorgudaki bilgisayar grubunu tanımlamak için kullanılan benzersiz bir takma ad. |
| Kategori       | Portaldaki sorguları düzenlemek için kategori. |


### <a name="active-directory"></a>Active Directory
Azure Monitor'u Active Directory grup üyeliklerini içe aktaracak şekilde yapılandırdığınızda, Log Analytics aracısıyla birleştirilmiş herhangi bir Windows etki alanının grup üyeliğini analiz eder.  Etkin Dizin'deki her güvenlik grubu için Azure Monitor'da bir bilgisayar grubu oluşturulur ve her Windows bilgisayarı üyesi oldukları güvenlik gruplarına karşılık gelen bilgisayar gruplarına eklenir.  Bu üyelik sürekli olarak her 4 saatte bir güncellenir.  

> [!NOTE]
> İçe aktarılan Etkin Dizin grupları yalnızca Windows makineleri içerir.

Azure portalındaki Log Analytics çalışma alanınızdaki **Gelişmiş ayarlardan** Etkin Dizin güvenlik gruplarını içe aktaracak şekilde Azure Monitörünü yapılandırAbilirsiniz.  **Bilgisayar Grupları,** **Active Directory'yi**seçin ve ardından **Bilgisayarlardan Etkin Dizin grup üyeliklerini içe aktarın.**  Başka bir yapılandırma işlemi gerekmez.

![Active Directory'den bilgisayar grupları](media/computer-groups/configure-activedirectory.png)

Gruplar içe aktarıldığında, menüde grup üyeliği algılanan bilgisayar sayısı ve alınan grup sayısı listelanır.  **ComputerGroup** kayıtlarını bu bilgilerle döndürmek için bu bağlantılardan herhangi birini tıklatabilirsiniz.

### <a name="windows-server-update-service"></a>Windows Server Güncelleme Hizmeti
Azure Monitor'u WSUS grup üyeliklerini içe aktaracak şekilde yapılandırdığınızda, Log Analytics aracısı ile tüm bilgisayarların hedefleme grubu üyeliğini analiz eder.  İstemci tarafı hedeflemekullanıyorsanız, Azure Monitor'a bağlı olan ve wsus hedefleme gruplarının bir parçası olan tüm bilgisayarların grup üyeliği Azure Monitor'a aktarılır. Sunucu tarafı hedeflemekullanıyorsanız, grup üyelik bilgilerinin Azure Monitor'a aktarılabilmesi için LOG Analytics aracısının WSUS sunucusuna yüklenmesi gerekir.  Bu üyelik sürekli olarak her 4 saatte bir güncellenir. 

Azure portalındaki Log Analytics çalışma alanınızdaki **Gelişmiş ayarlardan** WSUS gruplarını almak için Azure Monitor'u yapılandırAbilirsiniz.  **Bilgisayar Grupları,** **WSUS'u**seçin ve ardından **WSUS grup üyeliklerini içe aktarın.**  Başka bir yapılandırma işlemi gerekmez.

![WSUS'tan bilgisayar grupları](media/computer-groups/configure-wsus.png)

Gruplar içe aktarıldığında, menüde grup üyeliği algılanan bilgisayar sayısı ve alınan grup sayısı listelanır.  **ComputerGroup** kayıtlarını bu bilgilerle döndürmek için bu bağlantılardan herhangi birini tıklatabilirsiniz.

### <a name="configuration-manager"></a>Configuration Manager
Azure Monitor'u Configuration Manager koleksiyon üyeliklerini içe aktaracak şekilde yapılandırdığınızda, her koleksiyon için bir bilgisayar grubu oluşturur.  Koleksiyon üyelik bilgileri, bilgisayar gruplarını güncel tutmak için her 3 saatte bir alınır. 

Configuration Manager koleksiyonlarını içe aktarabilmek için Configuration [Manager'ı Azure Monitor'a bağlamanız](collect-sccm.md)gerekir.  

![SCCM'den bilgisayar grupları](media/computer-groups/configure-sccm.png)

Koleksiyonlar içe aktarıldığında, menüde grup üyeliği algılanan bilgisayar sayısı ve alınan grup sayısı listelanır.  **ComputerGroup** kayıtlarını bu bilgilerle döndürmek için bu bağlantılardan herhangi birini tıklatabilirsiniz.

## <a name="managing-computer-groups"></a>Bilgisayar gruplarını yönetme
Azure portalındaki Log Analytics çalışma alanınızdaki **Gelişmiş ayarlardan** günlük sorgusundan veya Günlük Arama API'sinden oluşturulan bilgisayar gruplarını görüntüleyebilirsiniz.  **Bilgisayar Grupları'nı** seçin ve sonra **Kaydedilen Gruplar.**  

Bilgisayar grubunu silmek için **Kaldır** sütunundaki **x'i** tıklatın.  Grubun üyelerini döndüren günlük aramasını çalıştırmak için bir grup için **Üyeleri Görüntüle** simgesini tıklatın.  Bir bilgisayar grubunu değiştiremezsiniz, ancak bunun yerine silip değiştirilen ayarlarla yeniden oluşturmanız gerekir.

![Kaydedilen bilgisayar grupları](media/computer-groups/configure-saved.png)


## <a name="using-a-computer-group-in-a-log-query"></a>Günlük sorgusunda bilgisayar grubu kullanma
Bir sorgudaki günlük sorgusundan oluşturulan bir Bilgisayar grubunu, genellikle aşağıdaki sözdizimi yle diğer adını bir işlev olarak değerlendirerek kullanırsınız:

  `Table | where Computer in (ComputerGroup)`

Örneğin, mycomputergroup adlı bir bilgisayar grubundaki yalnızca bilgisayarlar için UpdateSummary kayıtlarını döndürmek için aşağıdakileri kullanabilirsiniz.
 
  `UpdateSummary | where Computer in (mycomputergroup)`


İçe aktarılan bilgisayar grupları ve bunların dahil edilen bilgisayarları **ComputerGroup** tablosunda depolanır.  Örneğin, aşağıdaki sorgu, Etki Alanı Bilgisayarları grubundaki bilgisayarların listesini Active Directory'den döndürdü. 

  `ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "Domain Computers" | distinct Computer`

Aşağıdaki sorgu, Etki Alanı Bilgisayarlarında yalnızca bilgisayarlar için UpdateSummary kayıtlarını döndürdü.

  ```
  let ADComputers = ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "Domain Computers" | distinct Computer;
  UpdateSummary | where Computer in (ADComputers)
  ```




## <a name="computer-group-records"></a>Bilgisayar grubu kayıtları
Active Directory veya WSUS'tan oluşturulan her bilgisayar grubu üyeliği için Log Analytics çalışma alanında bir kayıt oluşturulur.  Bu kayıtların bir **ComputerGroup** türü vardır ve aşağıdaki tabloda özellikleri vardır.  Günlük sorgularına dayalı olarak bilgisayar grupları için kayıtlar oluşturulmaz.

| Özellik | Açıklama |
|:--- |:--- |
| `Type` |*Bilgisayar Grubu* |
| `SourceSystem` |*SourceSystem* |
| `Computer` |Üye bilgisayarın adı. |
| `Group` |Grubun adı. |
| `GroupFullName` |Kaynak ve kaynak adı da dahil olmak üzere gruba tam yol. |
| `GroupSource` |O grubun kaynağı toplandı. <br><br>ActiveDirectory<br>WSUS<br>WSUSClientHedefleme |
| `GroupSourceName` |Grubun toplandığı kaynağın adı.  Active Directory için bu etki alanı adıdır. |
| `ManagementGroupName` |SCOM aracıları için yönetim grubunun adı.  Diğer aracılar için bu AOI-\<çalışma alanı kimliğidir\> |
| `TimeGenerated` |Bilgisayar grubunun oluşturulduğu veya güncelleştirıldığı tarih ve saat. |

## <a name="next-steps"></a>Sonraki adımlar
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin.  

