---
title: Log Analytics çalışma alanında Azure kaynak günlüklerini toplama
description: Azure kaynak günlüklerini Azure Monitor'da bir Günlük Analizi çalışma alanına nasıl aktartınız öğrenin.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 36bd464624118b7671a3879bcc1d34114bba9ce3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248599"
---
# <a name="collect-azure-platform-logs-in-log-analytics-workspace-in-azure-monitor"></a>Azure Monitor'da Log Analytics çalışma alanında Azure platform günlüklerini toplama
Azure Etkinliği günlüğü ve kaynak günlükleri de dahil olmak üzere Azure'daki [platform günlükleri,](platform-logs-overview.md) Azure kaynakları ve bağlı oldukları Azure platformu için ayrıntılı tanılama ve denetleme bilgileri sağlar. Bu makalede, güçlü günlük sorguları kullanarak Azure Monitor Günlükleri'nde toplanan diğer izleme verileriyle analiz etmenizi ve ayrıca uyarılar ve uyarılar gibi diğer Azure Monitor özelliklerinden yararlanmanızı sağlayan bir Log Analytics çalışma alanında kaynak günlükleri toplama özelliği açıklanmaktadır. Görsel. 


## <a name="what-you-can-do-with-platform-logs-in-a-workspace"></a>Çalışma alanında platform günlükleri ile yapabilecekler
Bir Log Analytics çalışma alanında platform günlüklerini toplamak, tüm Azure kaynaklarınızın günlüklerini birlikte analiz etmenizi ve Azure [Monitör Günlükleri'nde](data-platform-logs.md) bulunan ve aşağıdakileri içeren tüm özelliklerden yararlanmanızı sağlar:

* **Günlük sorguları** - Tanılama verilerinizi hızlı bir şekilde analiz etmek ve öngörüler elde etmek ve Azure Monitor'daki diğer kaynaklardan toplanan verilerle analiz etmek için güçlü bir sorgu dili kullanarak [günlük sorguları](../log-query/log-query-overview.md) oluşturun.
* **Uyarı -** [Azure Monitor'daki günlük uyarılarını](alerts-log.md)kullanarak kaynak günlüklerinizde tanımlanan kritik koşullar ve desenler hakkında proaktif bildirim alın.
* **Görselleştirmeler** - Günlük sorgusunun sonuçlarını Azure panosuna sabitleyin veya etkileşimli bir raporun parçası olarak çalışma kitabına ekleyin.

## <a name="prerequisites"></a>Ön koşullar
Zaten bir çalışma alanınız yoksa [yeni bir çalışma alanı oluşturmanız](../learn/quick-create-workspace.md) gerekir. Çalışma alanı, ayarı yapılandıran kullanıcı her iki aboneye de uygun RBAC erişimine sahip olduğu sürece, kaynak gönderen günlüklerle aynı abonelikte olmak zorunda değildir.

## <a name="create-a-diagnostic-setting"></a>Tanılama ayarı oluşturma
Azure kaynağı için tanılama ayarı oluşturarak platform günlüklerini Log Analytics çalışma alanına ve diğer hedeflere gönderin. Ayrıntılar [için Azure'da günlükleri ve ölçümleri toplamak için tanı lama ayarını oluştur'a](diagnostic-settings.md) bakın.


## <a name="activity-log-collection"></a>Etkinlik günlüğü koleksiyonu
Etkinlik günlüğünü herhangi bir abonelikten en fazla beş Log Analytics çalışma alanlarına gönderebilirsiniz. Günlük Analizi çalışma alanında toplanan kaynak günlüğü verileri **AzureActivity** tablosunda depolanır. 

## <a name="resource-log-collection-mode"></a>Kaynak günlüğü toplama modu
Log Analytics çalışma alanında toplanan kaynak günlüğü verileri, Azure [Monitör Günlüklerinin Yapısı'nda](../log-query/logs-structure.md)açıklandığı gibi tablolarda depolanır. Kaynak günlükleri tarafından kullanılan tablolar, kaynağın ne tür bir koleksiyon kullandığına bağlıdır:

- Azure tanılama - Yazılan tüm veriler _Azure Diagnostics_ tablosuna dır.
- Kaynağa özel - Veriler, kaynağın her kategorisi için tek tek tabloya yazılır.

### <a name="azure-diagnostics-mode"></a>Azure Tanılama modu 
Bu modda, herhangi bir [tanıa ayardaki](diagnostic-settings.md) tüm veriler _Azure Diagnostics_ tablosunda toplanır. Bu, günümüzde çoğu Azure hizmeti tarafından kullanılan eski yöntemdir.

Birden çok kaynak türü aynı tabloya veri gönderdiğinden, şema, toplanan tüm farklı veri türlerinin şemalarının üst kümesidir.

Tanılama ayarlarının aşağıdaki veri türleri için aynı çalışma alanında toplandığı aşağıdaki örneği göz önünde bulundurun:

- Hizmet 1 denetim günlükleri (A, B ve C sütunlarından oluşan bir şema)  
- Hizmet 1 hata günlükleri (D, E ve F sütunlarından oluşan bir şema)  
- Hizmet 2 denetim günlükleri (G, H ve I sütunlarından oluşan bir şema olması)  

Azure Diagnostics tablosu aşağıdaki gibi görünecektir:  

| ResourceProvider    | Kategori     | A  | B  | C  | D  | E  | F  | G  | H  | I  |
| -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
| Microsoft.Service1 | Denetim Kayıtları    | x1 | y1 | Z1 |    |    |    |    |    |    |
| Microsoft.Service1 | Hata Günlükleri    |    |    |    | Q1 | w1 | e1 |    |    |    |
| Microsoft.Service2 | Denetim Kayıtları    |    |    |    |    |    |    | j1 | k1 | l1 |
| Microsoft.Service1 | Hata Günlükleri    |    |    |    | q2 | w2 | e2 |    |    |    |
| Microsoft.Service2 | Denetim Kayıtları    |    |    |    |    |    |    | j3 | k3 | l3 |
| Microsoft.Service1 | Denetim Kayıtları    | x5 | y5 | z5 |    |    |    |    |    |    |
| ... |

### <a name="resource-specific"></a>Kaynağa Özel
Bu modda, tanılama ayarında seçilen her kategori için seçili çalışma alanında tek tek tablolar oluşturulur. Bu yöntem, günlük sorgularındaki verilerle çalışmayı çok daha kolay hale getirir, şemaların ve yapılarının daha iyi keşfedilebilirliğini sağlar, hem yutma gecikmesi hem de sorgu süreleri boyunca performansı artırır ve belirli bir tablo. Tüm Azure hizmetleri sonunda Kaynağa Özel moduna geçer. 

Yukarıdaki örnek, üç tablo nun oluşturulmasına neden olur:
 
- Tablo *Service1AuditLogs* aşağıdaki gibidir:

    | Kaynak Sağlayıcı | Kategori | A | B | C |
    | -- | -- | -- | -- | -- |
    | Hizmet1 | Denetim Kayıtları | x1 | y1 | Z1 |
    | Hizmet1 | Denetim Kayıtları | x5 | y5 | z5 |
    | ... |

- Tablo *Service1ErrorLogs* aşağıdaki gibidir:  

    | Kaynak Sağlayıcı | Kategori | D | E | F |
    | -- | -- | -- | -- | -- | 
    | Hizmet1 | Hata Günlükleri |  Q1 | w1 | e1 |
    | Hizmet1 | Hata Günlükleri |  q2 | w2 | e2 |
    | ... |

- Tablo *Service2AuditLogs* aşağıdaki gibidir:  

    | Kaynak Sağlayıcı | Kategori | G | H | I |
    | -- | -- | -- | -- | -- |
    | Servis2 | Denetim Kayıtları | j1 | k1 | l1|
    | Servis2 | Denetim Kayıtları | j3 | k3 | l3|
    | ... |



### <a name="select-the-collection-mode"></a>Toplama modunu seçin
Azure kaynaklarının çoğu, size seçenek bırakmadan **Azure Tanılama** veya **Kaynağa Özel modda** çalışma alanına veri yazar. Hangi modda kullandığı ayrıntılar için [her hizmetin belgelerine](diagnostic-logs-schema.md) bakın. Tüm Azure hizmetleri sonunda Kaynağa Özel modu kullanır. Bu geçişin bir parçası olarak, bazı kaynaklar tanılama ayarında bir mod seçmenize olanak sağlar. Verilerin yönetilmesini kolaylaştırdığı ve daha sonraki bir tarihte karmaşık geçişleri önlemenize yardımcı olabileceğinden, yeni tanılama ayarları için kaynağa özgü mod belirtmelisiniz.
  
   ![Tanı ayarları modu seçici](media/resource-logs-collect-workspace/diagnostic-settings-mode-selector.png)




> [!NOTE]
> Şu anda, **Azure tanılama** ve **Kaynağa özgü** mod yalnızca Azure portalındaki tanılama ayarını yapılandırırken seçilebilir. Ayarı CLI, PowerShell veya Rest API kullanarak yapılandırırsanız, varsayılan olarak **Azure tanılama**için.

Varolan bir tanılama ayarını kaynağa özgü modda değiştirebilirsiniz. Bu durumda, zaten toplanan veriler, çalışma alanı için bekletme ayarınıza göre kaldırılana kadar _Azure Diagnostics_ tablosunda kalır. Yeni veriler ilgili tabloda toplanır. Her iki tabloda da verileri sorgulamak için [birleşim](https://docs.microsoft.com/azure/kusto/query/unionoperator) işlecikullanın.

Kaynağa Özel modu destekleyen Azure hizmetleri yle ilgili duyurular için [Azure Güncelleştirmeleri](https://azure.microsoft.com/updates/) blogunu izlemeye devam edin.

### <a name="column-limit-in-azurediagnostics"></a>AzureDiagnostics'te sütun sınırı
Azure Monitör Günlükleri'ndeki tüm tablolar için 500 özellik sınırı vardır. Bu sınıra ulaşıldıktan sonra, ilk 500'ün dışında herhangi bir özelliği olan verileri içeren satırlar yutma zamanında bırakılır. *Azure Diagnostics* tablosu, bu sınıra yazı yazan tüm Azure hizmetlerinin özelliklerini içerdiğinden, özellikle bu sınıra açıktır.

Birden çok hizmetten kaynak günlükleri topluyorsanız, _AzureDiagnostics_ bu sınırı aşabilir ve veriler gözden kaçırılabilir. Tüm Azure hizmetleri kaynağa özgü modu destekleyene kadar, kaynakları 500 sütun sınırına ulaşma olasılığını azaltmak için birden çok çalışma alanlarına yazacak şekilde yapılandırmanız gerekir.

### <a name="azure-data-factory"></a>Azure Data Factory
Azure Veri Fabrikası, çok ayrıntılı günlükler kümesi nedeniyle, çok sayıda sütun yazdığı bilinen ve _Azure Diagnostics'in_ sınırını aşması için büyük bir neden olabilir. Kaynağa özel mod etkinleştirilmeden önce yapılandırılan tanılama ayarları için, herhangi bir faaliyete karşı benzersiz adlandırılmış kullanıcı parametresi için yeni bir sütun oluşturulur. Etkinlik giriş ve çıktılarının ayrıntılı doğası nedeniyle daha fazla sütun oluşturulur.
 
Kaynağa özel modu mümkün olan en kısa sürede kullanmak için günlüklerinizi geçirmelisiniz. Bunu hemen yapamıyorsanız, ara bir alternatif, bu günlüklerin çalışma alanlarınızda toplanan diğer günlük türlerini etkileme olasılığını en aza indirmek için Azure Veri Fabrikası günlüklerini kendi çalışma alanlarına yalıtmaktır.


## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak günlükleri hakkında daha fazla bilgi edinin.](platform-logs-overview.md)
* [Azure'da günlükleri ve ölçümleri toplamak için tanılama ayarı oluşturun.](diagnostic-settings.md)
