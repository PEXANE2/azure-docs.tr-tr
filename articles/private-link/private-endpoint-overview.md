---
title: Azure özel uç noktası nedir?
description: Azure özel uç noktası hakkında bilgi edinin
services: private-link
author: KumudD
ms.service: private-link
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: kumud
ms.openlocfilehash: 7394a9e1b1fa90741bc26fc0e9b6b7e6284d75e0
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71104689"
---
# <a name="what-is-azure-private-endpoint"></a>Azure özel uç noktası nedir?

Azure özel uç noktası, Azure özel bağlantısı tarafından desteklenen bir hizmete özel ve güvenli bir şekilde bağlanan bir ağ arabirimidir. Özel uç nokta, sanal ağınızdan bir özel IP adresi kullanarak hizmeti sanal ağınıza etkin bir şekilde getiriyor. Hizmet, Azure depolama, SQL, vb. gibi bir Azure hizmeti veya kendi [özel bağlantı hizmetiniz](private-link-service-overview.md)olabilir.
  
## <a name="private-endpoint-properties"></a>Özel uç nokta özellikleri 
 Özel bir uç nokta aşağıdaki özellikleri belirtir: 


|Özellik  |Description |
|---------|---------|
|Name    |    Kaynak grubu içinde benzersiz bir ad.      |
|Subnet    |  Sanal bir ağdan özel IP adresleri dağıtmak ve ayırmak için alt ağ. Alt ağ gereksinimleri için, bu makaledeki sınırlamalar bölümüne bakın.         |
|Özel bağlantı kaynağı    |   Kullanılabilir türler listesinden kaynak KIMLIĞI veya diğer ad kullanarak bağlanacak özel bağlantı kaynağı. Bu kaynağa gönderilen tüm trafik için benzersiz bir ağ tanımlayıcısı oluşturulacak.       |
|Hedef alt kaynak   |      Bağlanılacak alt kaynak. Her özel bağlantı kaynağı türü, tercihe göre seçim yapmak için farklı seçeneklere sahiptir.    |
|Bağlantı onay yöntemi    |  Otomatik veya el ile. Rol tabanlı erişim denetimi (RBAC) izinlerine bağlı olarak, Özel uç noktanız otomatik olarak onaylanabilir. RBAC olmadan bir özel bağlantı kaynağına bağlanmaya çalışırsanız, kaynağın sahibinin bağlantıyı onaylamasını sağlamak için el ile yöntemini kullanın.        |
|İstek Iletisi     |  İstenen bağlantıların el ile onaylanabilmesi için bir ileti belirtebilirsiniz. Bu ileti, belirli bir isteği tanımlamak için kullanılabilir.        |
|Bağlantı durumu   |   Özel uç noktanın etkin olup olmadığını belirten bir salt okunurdur özelliği. Trafiği göndermek için yalnızca onaylanan durumdaki özel uç noktalar kullanılabilir. Kullanılabilir ek durumlar: <br>-**Onaylandı**: Bağlantı otomatik olarak veya el ile onaylandı ve kullanılabilir hale gelmiştir.</br><br>-**Bekliyor**: Bağlantı el ile oluşturuldu ve özel bağlantı kaynağı sahibi tarafından onay bekliyor.</br><br>-**Reddedildi**: Bağlantı, özel bağlantı kaynağı sahibi tarafından reddedildi.</br><br>-**Bağlantısı kesik**: Bağlantı, özel bağlantı kaynağı sahibi tarafından kaldırıldı. Özel uç nokta bilgilendirici hale gelir ve temizlik için silinmelidir. </br>|

Özel uç noktalarla ilgili bazı önemli ayrıntılar aşağıda verilmiştir: 
- Özel uç nokta, özel bağlantı tarafından desteklenen [VPN](https://azure.microsoft.com/services/vpn-gateway/) veya [Express Route](https://azure.microsoft.com/services/expressroute/) ve Hizmetleri kullanarak, bölgesel olarak eşlenmiş sanal ağlar, genel olarak eşlenmiş sanal ağlar ve şirket içi olan sanal ağlar arasında bağlantı sağlar.
 
- Özel bir uç nokta oluştururken kaynağın yaşam döngüsü için bir ağ arabirimi de oluşturulur. Arabirimine, özel bağlantı hizmetiyle eşlenen alt ağdan özel bir IP adresi atanır.
 
- Özel uç noktanın sanal ağla aynı bölgede dağıtılması gerekir. 
 
- Özel bağlantı kaynağı, sanal ağ ve özel uç noktadan farklı bir bölgede dağıtılabilir.
 
- Aynı özel bağlantı kaynağı kullanılarak birden çok özel uç nokta oluşturulabilir. Ortak bir DNS sunucusu yapılandırması kullanan tek bir ağ için önerilen uygulama, DNS çözünürlüğünde yinelenen girişlerin veya çakışmaların oluşmaması için belirli bir özel bağlantı kaynağı için tek bir özel uç nokta kullanmaktır. 
 
- Aynı sanal ağ içindeki aynı veya farklı alt ağlarda birden çok özel uç nokta oluşturulabilir. Bir abonelikte oluşturabileceğiniz özel uç noktalar sayısı için sınırlar vardır. Ayrıntılar için bkz. [Azure Limitleri](https://docs.microsoft.com/azure/azure-subscription-service-limits.md#networking-limits).


 
## <a name="private-link-resource"></a>Özel bağlantı kaynağı 
Özel bir bağlantı kaynağı, belirli bir özel uç noktanın hedef hedefidir. Aşağıda, kullanılabilir özel bağlantı kaynağı türlerinin listesi verilmiştir: 
 
|Özel bağlantı kaynağı adı  |Kaynak türü   |Alt kaynaklar  |
|---------|---------|---------|
|**Özel bağlantı hizmeti** (Kendi hizmetiniz)   |  Microsoft. Network/privateLinkServices       | boş |
|**Azure SQL Veritabanı** | Microsoft. SQL/sunucuları    |  SQL Server (sqlServer)        |
|**Azure SQL Veri Ambarı** | Microsoft. SQL/sunucuları    |  SQL Server (sqlServer)        |
|**Azure Depolama**  | Microsoft.Storage/storageAccounts    |  Blob (blob, blob_secondary)<BR> Tablo (tablo, table_secondary)<BR> Kuyruk (Queue, queue_secondary)<BR> Dosya (dosya, file_secondary)<BR> Web (Web, web_secondary)        |
|**Azure Data Lake Storage 2.**  | Microsoft.Storage/storageAccounts    |  Blob (blob, blob_secondary)       |
 
 
## <a name="network-security-of-private-endpoints"></a>Özel uç noktaların ağ güvenliği 
Azure hizmetleri için özel uç noktalar kullanılırken, trafik belirli bir özel bağlantı kaynağıyla güvenli hale getirilir. Platform yalnızca belirtilen özel bağlantı kaynağına ulaşan ağ bağlantılarını doğrulamak için bir erişim denetimi gerçekleştirir. Aynı Azure hizmeti içindeki ek kaynaklara erişmek için ek özel uç noktalar gereklidir. 
 
Desteklenen bir Azure hizmetine bağlanmak için iş yüklerinizin genel uç noktalara erişmesini tamamen kilitleyemezsiniz. Bu denetim, aynı Azure hizmetinde barındırılan diğer kaynaklara erişimi engelleyen yerleşik bir savunma koruması sağlayarak kaynaklarınıza ek bir ağ güvenlik katmanı sağlar. 
 
## <a name="access-to-a-private-link-resource-using-approval-workflow"></a>Onay iş akışı kullanarak bir özel bağlantı kaynağına erişim 
Aşağıdaki bağlantı onay yöntemlerini kullanarak bir özel bağlantı kaynağına bağlanabilirsiniz:
- Belirli özel bağlantı kaynağına sahip olduğunuzda veya izniniz olduğunda **otomatik olarak** onaylandı. Gerekli olan izin, özel bağlantı kaynak türüne aşağıdaki biçimde dayalıdır: MICROSOFT. \<Sağlayıcı >/< resource_type >/privateendpointconnectionapproval/Action
- Gerekli izinlere sahip olmadığınız ve erişim istemek istediğiniz zaman **el ile** istek. Bir onay iş akışı başlatılacak. Özel uç nokta ve sonraki özel uç nokta bağlantısı "beklemede" durumunda oluşturulur. Özel bağlantı kaynağı sahibi bağlantıyı onaylamaya sorumludur. Onaylandıktan sonra, aşağıdaki onay iş akışı diyagramında gösterildiği gibi özel uç nokta trafiği normal şekilde göndermek üzere etkinleştirilir.  

![iş akışı onayı](media/private-endpoint-overview/private-link-paas-workflow.png)
 
Özel bağlantı kaynağı sahibi, özel bir uç nokta bağlantısı üzerinde aşağıdaki eylemleri gerçekleştirebilir: 
- Tüm özel uç nokta bağlantılarının ayrıntılarını gözden geçirin. 
- Özel bir uç nokta bağlantısını onaylayın. Karşılık gelen özel uç nokta, trafiği özel bağlantı kaynağına göndermek için etkinleştirilir. 
- Özel bir uç nokta bağlantısını reddedin. İlgili özel uç nokta, durumu yansıtacak şekilde güncelleştirilir.
- Özel bir uç nokta bağlantısını herhangi bir durumda silin. Karşılık gelen özel uç nokta, eylemi yansıtacak şekilde bağlantısı kesik bir durumla güncelleştirilir, Özel uç nokta sahibi kaynağı yalnızca bu noktada silebilir. 
 
> [!NOTE]
> Yalnızca onaylanan bir durumdaki özel uç nokta, belirli bir özel bağlantı kaynağına trafik gönderebilir. 

### <a name="connecting-using-alias"></a>Takma ad kullanarak bağlanma
Diğer ad, hizmet sahibi bir standart yük dengeleyicinin arkasında özel bağlantı hizmeti oluşturduğunda oluşturulan benzersiz bir addır. Hizmet sahibi, bu diğer adı tüketicileriyle çevrimdışı olarak paylaşabilir. Tüketiciler, kaynak URI 'sini veya diğer adı kullanarak özel bağlantı hizmetine bağlantı isteğinde bulunabilir. Diğer adı kullanarak bağlanmak istiyorsanız, el ile bağlantı onay yöntemi kullanarak özel uç nokta oluşturmanız gerekir. El ile bağlantı onay yöntemi kullanmak için, Özel uç nokta oluşturma akışı sırasında el ile istek parametresini true olarak ayarlayın. Ayrıntılar için [New-AzPrivateEndpoint](https://docs.microsoft.com/en-us/powershell/module/az.network/new-azprivateendpoint?view=azps-2.6.0) ve [az Network Private-Endpoint Create](https://docs.microsoft.com/en-us/cli/azure/network/private-endpoint?view=azure-cli-latest#az-network-private-endpoint-create) bölümüne bakın. 

## <a name="dns-configuration"></a>DNS yapılandırması 
Bağlantı dizesinin bir parçası olarak tam etki alanı adı (FQDN) kullanarak bir özel bağlantı kaynağına bağlanırken, DNS ayarlarınızı ayrılmış özel IP adresine çözümlemek üzere doğru şekilde yapılandırmak önemlidir. Mevcut Azure hizmetlerinde ortak bir uç nokta üzerinden bağlanılırken kullanılacak bir DNS yapılandırması zaten olabilir. Özel uç noktanız kullanılarak bağlanmak için bunun geçersiz kılınması gerekir. 
 
Özel uç nokta ile ilişkili ağ arabirimi, DNS 'nizi yapılandırmak için gerekli olan ve belirli bir özel bağlantı kaynağı için ayrılan FQDN ve özel IP adresleri dahil olmak üzere tam bilgi kümesini içerir. 
 
Özel uç noktalar için DNS ayarlarınızı yapılandırmak üzere aşağıdaki seçenekleri kullanabilirsiniz: 
- **Ana bilgisayar dosyasını kullanın (yalnızca test için önerilir)** . DNS 'yi geçersiz kılmak için bir sanal makinede ana bilgisayar dosyasını kullanabilirsiniz.  
- **Özel BIR DNS bölgesi kullanın**. Belirli bir özel uç nokta için DNS çözümlemesini geçersiz kılmak üzere özel DNS bölgelerini kullanabilirsiniz. Özel bir DNS bölgesi, belirli etki alanlarını çözümlemek için sanal ağınıza bağlanabilir.
- **Özel DNS sunucunuzu kullanın**. Belirli bir özel bağlantı kaynağı için DNS çözümlemesini geçersiz kılmak üzere kendi DNS sunucunuzu kullanabilirsiniz. DNS sunucunuz bir sanal ağ üzerinde barındırılıyorsa, tüm özel bağlantı kaynakları için yapılandırmayı basitleştirmek üzere özel bir DNS bölgesi kullanmak üzere bir DNS iletme kuralı oluşturabilirsiniz.
 
> [!IMPORTANT]
> Ortak uç noktaları çözümlemek için etkin olarak kullanılan bir bölgenin geçersiz kılınması önerilmez. Kaynaklara bağlantılar, DNS iletimi olmadan genel DNS 'e iletilmeksizin doğru şekilde çözümlenemez. Sorunları önlemek için, farklı bir etki alanı adı oluşturun veya aşağıdaki her hizmet için önerilen adı izleyin. 
 
Azure hizmetleri için aşağıdaki tabloda açıklandığı gibi önerilen bölge adlarını kullanın:

|Özel bağlantı kaynağı türü   |Alt kaynak  |Bölge adı  |
|---------|---------|---------|
|SQL DB/DW (Microsoft. SQL/Servers)    |  SQL Server (sqlServer)        |   privatelink.database.windows.net       |
|Depolama hesabı (Microsoft. Storage/storageAccounts)    |  Blob (blob, blob_secondary)        |    privatelink.blob.core.windows.net      |
|Depolama hesabı (Microsoft. Storage/storageAccounts)    |    Tablo (tablo, table_secondary)      |   privatelink.table.core.windows.net       |
|Depolama hesabı (Microsoft. Storage/storageAccounts)    |    Kuyruk (Queue, queue_secondary)     |   privatelink.queue.core.windows.net       |
|Depolama hesabı (Microsoft. Storage/storageAccounts)   |    Dosya (dosya, file_secondary)      |    privatelink.file.core.windows.net      |
|Depolama hesabı (Microsoft. Storage/storageAccounts)     |  Web (Web, web_secondary)        |    privatelink.web.core.windows.net      |
|Data Lake dosya sistemi Gen2 (Microsoft. Storage/storageAccounts)  |  Data Lake dosya sistemi Gen2 (DFS, dfs_secondary)        |     privatelink.dfs.core.windows.net     |
||||
 

Azure, çözümü önerilen etki alanı adlarına yönlendirmek için ortak DNS üzerinde kurallı bir ad DNS kaydı (CNAME) oluşturur. Özel uç noktalarınızın özel IP adresi ile çözümlemeyi geçersiz kılabileceksiniz. 
 
Uygulamalarınızın bağlantı URL 'sini değiştirmesi gerekmez. Genel bir DNS kullanarak çözümlemeye çalışırken, DNS sunucusu şimdi özel uç noktalarınıza çözümlenir. İşlem uygulamalarınızı etkilemez. 
 
## <a name="limitations"></a>Sınırlamalar
 
Aşağıdaki tabloda özel uç noktalar kullanılırken bilinen kısıtlamaların bir listesi yer almaktadır: 


|Sınırlama |Açıklama |Risk azaltma  |
|---------|---------|---------|
|Ağ güvenlik grubu (NSG) kuralları özel uç nokta için uygulanmıyor    |NSG özel uç noktalar üzerinde desteklenmez. Özel uç noktayı içeren alt ağlarda NSG ile ilişkili olabilir, kurallar özel uç nokta tarafından işlenen trafikte geçerli olmayacaktır. Bir alt ağda özel uç noktalar dağıtmak için [ağ ilkeleri zorlamasının devre dışı](disable-private-endpoint-network-policy.md) olması gerekir. NSG aynı alt ağda barındırılan diğer iş yükleri üzerinde de zorlanır.   | Kaynak istemcilerde giden trafik için NSG kurallarını kullanarak trafiği denetleyin.        |
|Hizmet uç noktası veya özel iş yükleri için etkinleştirilen alt ağlarda özel uç noktalar oluşturulamaz    |Özel uç noktalar, hizmet uç noktaları veya özel iş yükleri için temsilci atanmış alt ağlarda etkinleştirilmiş alt ağlarda dağıtılamaz|  Özel uç noktaları dağıtmak için ayrı bir alt ağ oluşturun.        |
|Özel uç nokta yalnızca aynı bölgedeki özel bağlantı hizmetine (müşteriye ait) eşlenebilir    |   Farklı bir bölgeden özel bir bağlantı hizmetine (kendinizinkini) bağlanma desteklenmez       |  Önizleme süresince özel bağlantı hizmetinizi aynı bölgede dağıtmanız gerekir.        |
|Özelleştirilmiş iş yükleri özel uç noktalara erişemez    |   Sanal ağınıza dağıtılan aşağıdaki hizmetler özel uç noktaları kullanarak herhangi bir özel bağlantı kaynağına erişemez:<br>Uygulama Hizmeti Planı</br>Azure Container Örneği</br>Azure NetApp Files</br>Azure Ayrılmış HSM<br>       |   Önizleme süresince risk azaltma.       |
|  Portal diğer adı kullanarak özel uç noktalar oluşturmayı desteklemez  |   Portal yalnızca kaynak URI 'sini kullanarak özel uç noktalar oluşturulmasına izin veriyor      | Özel uç nokta bağlantıları istemek için kaynak URI 'SI kullan        |

## <a name="next-steps"></a>Sonraki adımlar
- [Portal kullanarak SQL veritabanı sunucusu için özel bir uç nokta oluşturma](create-private-endpoint-portal.md)
- [PowerShell kullanarak SQL veritabanı sunucusu için özel bir uç nokta oluşturma](create-private-endpoint-powershell.md)
- [CLı kullanarak SQL veritabanı sunucusu için özel bir uç nokta oluşturma](create-private-endpoint-cli.md)
- [Portalı kullanarak depolama hesabı için özel bir uç nokta oluşturma](create-private-endpoint-storage-portal.md)
- [Azure PowerShell kullanarak kendi özel bağlantı hizmetinizi oluşturun](create-private-link-service-powershell.md)
