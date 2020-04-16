---
title: Azure Key Vault için sanal ağ hizmeti bitiş noktaları - Azure Key Vault | Microsoft Dokümanlar
description: Key Vault için sanal ağ hizmeti uç noktalarına genel bakış
services: key-vault
author: amitbapat
ms.author: ambapat
manager: rkarlin
ms.date: 01/02/2019
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.openlocfilehash: 2a68a50a5d15b9f38407c19494a39a14abfa0a5a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81432079"
---
# <a name="virtual-network-service-endpoints-for-azure-key-vault"></a>Azure Anahtar Kasası için sanal ağ hizmeti bitiş noktaları

Azure Key Vault'un sanal ağ hizmeti bitiş noktaları, belirli bir sanal ağa erişimi kısıtlamanızı sağlar. Uç noktalar ayrıca IPv4 (internet protokolü sürüm 4) adres aralıkları listesine erişimi kısıtlamanızı sağlar. Bu kaynakların dışından anahtar kasanıza bağlanan herhangi bir kullanıcının erişimi engellenir.

Bu kısıtlamanın önemli bir istisnası vardır. Bir kullanıcı güvenilen Microsoft hizmetlerine izin vermeyi kabul ettiyse, bu hizmetlerden gelen bağlantılara güvenlik duvarından izin verilir. Örneğin, bu hizmetler Office 365 Exchange Online, Office 365 SharePoint Online, Azure bilgi işlem, Azure Kaynak Yöneticisi ve Azure Yedekleme'yi içerir. Bu tür kullanıcıların hala geçerli bir Azure Etkin Dizin belirteci sunması ve istenen işlemi gerçekleştirmek için izinlere (erişim ilkeleri olarak yapılandırılan) sahip olması gerekir. Daha fazla bilgi için [Sanal ağ hizmeti uç noktalarına](../../virtual-network/virtual-network-service-endpoints-overview.md)bakın.

## <a name="usage-scenarios"></a>Kullanım senaryoları

Key Vault [güvenlik duvarlarını ve sanal ağları](network-security.md) varsayılan olarak tüm ağlardan (internet trafiği dahil) trafiğe erişimi engelleyecek şekilde yapılandırabilirsiniz. Belirli Azure sanal ağlarından ve genel internet IP adresi aralıklarından trafiğe erişim izni vererek uygulamalarınız için güvenli bir ağ sınırı oluşturabilirsiniz.

> [!NOTE]
> Key Vault güvenlik duvarları ve sanal ağ kuralları yalnızca Key Vault [veri düzlemi](secure-your-key-vault.md#data-plane-access-control) için geçerlidir. Anahtar Vault denetim düzlemi işlemleri (oluşturma, silme ve değiştirme işlemleri, erişim ilkeleri ayarlama, güvenlik duvarları oluşturma ve sanal ağ kuralları) güvenlik duvarlarından ve sanal ağ kurallarından etkilenmez.

Hizmet uçnoktalarını nasıl kullanabileceğinize dair bazı örnekler aşağıda verilmiştir:

* Şifreleme anahtarlarını, uygulama sırlarını ve sertifikalarını depolamak için Key Vault'u kullanıyorsunuz ve anahtar kasanıza genel internetten erişimi engellemek istiyorsunuz.
* Yalnızca uygulamanızın veya belirlenen ana bilgisayarların kısa bir listesinin anahtar kasanıza bağlanabilmesi için anahtar kasanıza erişimi kilitlemek istiyorsunuz.
* Azure sanal ağınızda çalışan bir uygulamanız var ve bu sanal ağ tüm gelen ve giden trafik için kilitlenir. Uygulamanızın sırları veya sertifikaları almak veya şifreleme anahtarlarını kullanmak için Key Vault'a bağlanması gerekir.

## <a name="configure-key-vault-firewalls-and-virtual-networks"></a>Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma

Güvenlik duvarlarını ve sanal ağları yapılandırmak için gereken adımlar aşağıda veda edilmiştir. Bu adımlar, PowerShell, Azure CLI veya Azure portalını kullanıyor olsanız da uygulanır.

1. Ayrıntılı erişim günlüklerini görmek için [Anahtar Kasa günlüğünü](logging.md)etkinleştirin. Bu, güvenlik duvarları ve sanal ağ kuralları önemli bir kasaya erişimi engellediğinde tanılamada yardımcı olur. (Bu adım isteğe bağlıdır, ancak şiddetle önerilir.)
2. Hedef sanal ağlar ve alt ağlar **için anahtar kasası için hizmet bitiş noktalarını** etkinleştirin.
3. Belirli sanal ağlardan, alt ağlardan ve IPv4 adres aralıklarından bu anahtar kasasına erişimi kısıtlamak için önemli bir kasa için güvenlik duvarları ve sanal ağ kuralları ayarlayın.
4. Bu anahtar kasasına güvenilen Microsoft hizmetleri tarafından erişilmesi gerekiyorsa, **Güvenilen Azure Hizmetlerinin** Key Vault'a bağlanmasına izin verme seçeneğini etkinleştirin.

Daha fazla bilgi için Azure [Anahtar Kasası güvenlik duvarlarını ve sanal ağları Yapılandırma'ya](network-security.md)bakın.

> [!IMPORTANT]
> Güvenlik duvarı kuralları yürürlüğe girdiğinde, kullanıcılar Yalnızca istekleri izin verilen sanal ağlardan veya IPv4 adres aralıklarından geldiğinde Key Vault [veri düzlemi](secure-your-key-vault.md#data-plane-access-control) işlemlerini gerçekleştirebilir. Bu, Azure portalından Key Vault'a erişmek için de geçerlidir. Kullanıcılar Azure portalından önemli bir kasaya göz atabilseler de, istemci makineleri izin verilenler listesinde değilse anahtarları, sırları veya sertifikaları listelemeyebilir. Bu, diğer Azure hizmetleri tarafından Key Vault Picker'ı da etkiler. Güvenlik duvarı kuralları istemci makinelerini engelliyorsa, kullanıcılar anahtar kasalarının listesini görebilir, ancak liste anahtarlarını göremeyebilir.


> [!NOTE]
> Aşağıdaki yapılandırma sınırlamaları hakkında bilgi olun:
> * En fazla 127 sanal ağ kuralı ve 127 IPv4 kuralına izin verilir. 
> * "/31" veya "/32" öneki boyutlarını kullanan küçük adres aralıkları desteklenmez. Bunun yerine, tek tek IP adresi kuralları kullanarak bu aralıkları yapılandırın.
> * IP ağ kurallarına yalnızca genel IP adresleri için izin verilir. Özel ağlar için ayrılmış IP adresi aralıklarına (RFC 1918'de tanımlandığı şekilde) IP kurallarında izin verilmez. Özel ağlar **10.**, **172.16-31**ve **192.168**ile başlayan adresleri içerir. 
> * Şu anda yalnızca IPv4 adresleri desteklenir.

## <a name="trusted-services"></a>Güvenilir hizmetler

Güvenilen Hizmetlere İzin Ver seçeneği etkinse, önemli bir kasaya erişmesine izin verilen güvenilir hizmetlerin listesi aşağıda veda **edilmiştir.**

|Güvenilir hizmet|Desteklenen kullanım senaryoları|
| --- | --- |
|Azure Sanal Makineler dağıtım hizmeti|[Müşteri tarafından yönetilen Key Vault'tan VM'lere sertifikaları dağıtın.](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)|
|Azure Kaynak Yöneticisi şablon dağıtım hizmeti|[Dağıtım sırasında güvenli değerleri geçirin.](../../azure-resource-manager/templates/key-vault-parameter.md)|
|Azure Disk Şifreleme birim şifreleme hizmeti|Sanal makine dağıtımı sırasında BitLocker Key (Windows VM) veya DM Passphrase (Linux VM) ve Anahtar Şifreleme Anahtarına erişime izin verin. Bu, [Azure Disk Şifreleme'yi](../../security/fundamentals/encryption-overview.md)etkinleştirir.|
|Azure Backup|Azure Sanal Makineler yedeklemesi sırasında Azure [Yedekleme'yi](../../backup/backup-introduction-to-azure-backup.md)kullanarak ilgili anahtarların ve sırların yedeklemesine ve geri yüklenmelerine izin verin.|
|Exchange Online & SharePoint Online|[Müşteri Anahtarı](/microsoft-365/compliance/customer-key-overview)ile Azure Depolama Hizmeti Şifrelemesi için müşteri anahtarına erişime izin verin.|
|Azure Information Protection|Azure Bilgi Koruması için kiracı anahtarına erişime izin [verin.](https://docs.microsoft.com/azure/information-protection/what-is-information-protection)|
|Azure App Service|[Azure Web App Sertifikasını Key Vault üzerinden dağıtın.](https://azure.github.io/AppService/2016/05/24/Deploying-Azure-Web-App-Certificate-through-Key-Vault.html)|
|Azure SQL Veritabanı|[Azure SQL Veritabanı ve Veri Ambarı için Kendi Anahtar desteğinizi getir ile Saydam Veri Şifreleme.](../../sql-database/transparent-data-encryption-byok-azure-sql.md?view=sql-server-2017&viewFallbackFrom=azuresqldb-current)|
|Azure Storage|[Azure Key Vault'ta müşteri tarafından yönetilen anahtarları kullanarak Depolama Hizmeti Şifrelemesi.](../../storage/common/storage-service-encryption-customer-managed-keys.md)|
|Azure Data Lake Store|[Azure Veri Gölü Deposu'ndaki verilerin](../../data-lake-store/data-lake-store-encryption.md) müşteri tarafından yönetilen bir anahtarla şifrelemi.|
|Azure Databricks|[Hızlı, kolay ve işbirlikçi Apache Spark tabanlı analiz hizmeti](../../azure-databricks/what-is-azure-databricks.md)|
|Azure API Management|[MSI kullanarak Key Vault'tan Özel Etki Alanı için sertifikaları dağıtma](../../api-management/api-management-howto-use-managed-service-identity.md#use-the-managed-service-identity-to-access-other-resources)|
|Azure Data Factory|[Veri Fabrikası'ndan Key Vault'ta veri deposu kimlik bilgilerini getir](https://go.microsoft.com/fwlink/?linkid=2109491)|
|Azure Event Hubs|[Müşteri tarafından yönetilen anahtarlar senaryosu için anahtar kasasına erişime izin verme](https://docs.microsoft.com/azure/event-hubs/configure-customer-managed-key)|
|Azure Service Bus|[Müşteri tarafından yönetilen anahtarlar senaryosu için anahtar kasasına erişime izin verme](https://docs.microsoft.com/azure/service-bus-messaging/configure-customer-managed-key)|
|Azure İçeri/Dışarı Aktarma| [Alma/Dışa Aktarma hizmeti için Azure Anahtar Kasası'nda müşteri tarafından yönetilen anahtarları kullanma](https://docs.microsoft.com/azure/storage/common/storage-import-export-encryption-key-portal)

> [!NOTE]
> İlgili hizmetlerin Key Vault'a erişebilmesi için ilgili Key Vault erişim ilkelerini ayarlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Anahtar kasanızı emniyete alaların](secure-your-key-vault.md))
* [Azure Key Vault güvenlik duvarlarını ve sanal ağları yapılandırma](network-security.md)
