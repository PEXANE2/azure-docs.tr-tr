---
title: Kaynak sağlayıcısı işlemlerini Azure Resource Manager | Microsoft Docs
description: Microsoft Azure Kaynak Yöneticisi kaynak sağlayıcıları için kullanılabilir işlemleri listeler
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/02/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 9d4b4134fa26fd2cb904a862ac16544873bf8bcb
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75934472"
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Azure Resource Manager kaynak sağlayıcısı işlemleri

Bu makalede, her bir Azure Resource Manager kaynak sağlayıcısı için kullanılabilen işlemler listelenir. Bu işlemler, Azure 'daki kaynaklara ayrıntılı [rol tabanlı erişim denetimi (RBAC)](overview.md) sağlamak için [özel rollerde](custom-roles.md) kullanılabilir. İşlem dizeleri şu biçimdedir: `{Company}.{ProviderName}/{resourceType}/{action}`. Kaynak sağlayıcısı ad alanlarının Azure hizmetlerine nasıl eşlendiğini gösteren bir liste için bkz. [kaynak sağlayıcısını hizmetle eşleştirme](../azure-resource-manager/azure-services-resource-providers.md).

Kaynak sağlayıcısı işlemleri her zaman gelişmektedir. En son işlemleri almak için [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) veya [az Provider Operation List](/cli/azure/provider/operation#az-provider-operation-list)komutunu kullanın.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. AAD/domainServices/Delete | Etki alanı hizmetini Sil |
> | Eylem | Microsoft. AAD/domainServices/oucontainer/Delete | OU kapsayıcısını Sil |
> | Eylem | Microsoft. AAD/domainServices/oucontainer/Read | OU kapsayıcılarını oku |
> | Eylem | Microsoft. AAD/domainServices/oucontainer/Write | OU kapsayıcısı yaz |
> | Eylem | Microsoft. AAD/domainServices/Read | Etki alanı hizmetlerini oku |
> | Eylem | Microsoft. AAD/domainServices/Write | Etki alanı hizmeti yaz |
> | Eylem | Microsoft. AAD/konumlar/operationresults/Read |  |
> | Eylem | Microsoft. AAD/Işlemler/okuma |  |
> | Eylem | Microsoft. AAD/Register/ACTION | Etki alanı hizmetini Kaydet |
> | Eylem | Microsoft. AAD/Unregister/eylem | Etki alanı hizmetinin kaydını sil |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. aadihar/diagnosticsettings/Delete | Deleting a diagnostic setting |
> | Eylem | Microsoft. aadihar/diagnosticsettings/Read | Reading a diagnostic setting |
> | Eylem | Microsoft. aadihar/diagnosticsettings/Write | Writing a diagnostic setting |
> | Eylem | Microsoft. aadihar/diagnosticsettingscategories/Read | Reading a diagnostic setting categories |
> | Eylem | Microsoft. aadihar/metricDefinitions/oku | Kiracı düzeyinde ölçüm tanımlarını okuma |
> | Eylem | Microsoft. aadihar/ölçümler/okuma | Kiracı düzeyinde ölçümleri okuma |

## <a name="microsoftaddons"></a>Microsoft. addons

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. addons/işlemler/okuma | Desteklenen RP işlemlerini alır. |
> | Eylem | Microsoft. addons/Register/ACTION | Belirtilen aboneliği Microsoft. addons ile kaydedin |
> | Eylem | Microsoft. addons/supportProviders/listsupportplanınfo/Action | Belirtilen abonelik için geçerli destek planı bilgilerini listeler. |
> | Eylem | Microsoft. addons/supportProviders/supportPlanTypes/Delete | Belirtilen kurallı destek planını kaldırır |
> | Eylem | Microsoft. addons/supportProviders/supportPlanTypes/Read | Belirtilen kurallı destek planı durumunu alır. |
> | Eylem | Microsoft. addons/supportProviders/supportPlanTypes/Write | Belirtilen kurallı destek planı türünü ekler. |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/Action | Kiracı için yeni bir orman oluşturun. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/addomainservicemembers/Read | Belirtilen hizmet adı için tüm sunucuları alır. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/uyarılar/Read | Ormanın AlertId, uyarı verdi tarihi, uyarı son algılanan, uyarı açıklaması, son güncelleme, uyarı düzeyi, uyarı durumu, uyarı sorun giderme bağlantıları vb. gibi uyarı ayrıntılarını alır. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/Configuration/Read | Ormanın hizmet yapılandırmasını alır. Örnek-Orman adı, Işlev düzeyi, etki alanı adlandırma yöneticisi FSMO rolü, şema yöneticisi FSMO rolü vb. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/Delete | Hizmet ve sunucularını sistem durumu verileriyle birlikte siler. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/Dimensions/Read | Ormanın etki alanlarını ve site ayrıntılarını alır. Örnek-sistem durumu, etkin uyarılar, çözümlenen uyarılar, etki alanı Işlev düzeyi, orman, altyapı yöneticisi, PDC, RID Yöneticisi vb. gibi özellikler  |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/Özellikler/kullanıcıtercihi/okuma | Ormanın Kullanıcı tercihi ayarını alır.<br>Ldapbaşarılı fulbind, ntlmaıd 'ler, kerberosauthentications, addsinsıısesagentprivatebytes, ldaparamaları gibi örnek-MetricCounterName.<br>UI grafikleri vs. ayarları. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/forestsummary/Read | Belirtilen orman için orman adı, bu ormandaki etki alanı sayısı, site ve site ayrıntıları gibi orman özetini alır. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/metricmetadata/Read | Belirli bir hizmet için desteklenen ölçümlerin listesini alır.<br>Örneğin, extranet hesabı kilitleme Işlemleri, toplam başarısız Istek sayısı, bekleyen belirteç Istekleri (proxy), ADFS hizmeti için belirteç Istekleri/sn.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP etkin Iş parçacıkları, Kerberos kimlik doğrulamaları/sn, ADDomainService için ATQ Iş parçacığı toplamı.<br>Profil gecikmesini çalıştırın, TCP bağlantıları oluşturulmuştur, Insights Aracısı özel baytları, ADSync hizmeti için Azure AD 'ye Istatistikleri dışarı aktarın. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/ölçümler/gruplar/okuma | Hizmet verildiğinde, bu API ölçüm bilgilerini alır.<br>Örneğin, bu API ile ilgili bilgi almak için bu API kullanılabilir: extranet hesabı kilitleme Işlemleri, toplam başarısız Istek, bekleyen belirteç Istekleri (proxy) ve ADFederation hizmeti için belirteç Istekleri/sn.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP etkin Iş parçacıkları, Kerberos kimlik doğrulamaları/sn, ADDomain hizmeti için ATQ Iş parçacığı toplamı.<br>Profil gecikmesini çalıştırın, TCP bağlantıları oluşturulmuştur, Insights Aracısı özel baytları, Istatistikleri eşitleme hizmeti için Azure AD 'ye aktarın. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/premiumcheck/Read | Bu API, Premium kiracı için tüm eklendi ADDomainServices listesini alır. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/Read | Belirtilen hizmet adı için hizmet ayrıntılarını alır. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/replicationdetails/Read | Belirtilen hizmet adı için tüm sunucular için çoğaltma ayrıntılarını alır. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/ReplicationStatus/Read | Varsa, etki alanı denetleyicilerinin sayısını ve bunların çoğaltma hatalarını alır. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/replicationsummary/Read | Tüm etki alanı denetleyicisi listesini, belirtilen ormanın çoğaltma ayrıntılarının birlikte alır. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/servicemembers/Action | Hizmete bir sunucu örneği ekleyin. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/servicemembers/Credentials/Read | ADDomainService 'in sunucu kaydı sırasında, yeni sunucuları ekleme kimlik bilgilerini almak için bu API çağırılır. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/servicemembers/Delete | Belirli bir hizmet ve kiracı için bir sunucuyu siler. |
> | Eylem | Microsoft. ADHybridHealthService/addsservices/Write | Kiracı için ADDomainService örneğini oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. ADHybridHealthService/yapılandırma/eylem | Kiracı yapılandırmasını güncelleştirir. |
> | Eylem | Microsoft. ADHybridHealthService/yapılandırma/okuma | Kiracı yapılandırmasını okur. |
> | Eylem | Microsoft. ADHybridHealthService/yapılandırma/yazma | Kiracı yapılandırması oluşturur. |
> | Eylem | Microsoft. ADHybridHealthService/logs/Contents/Read | Blob 'da depolanan aracı yükleme ve kayıt günlüklerinin içeriğini alır. |
> | Eylem | Microsoft. ADHybridHealthService/logs/Read | Kiracının aracı yüklemesini ve kayıt günlüklerini alır. |
> | Eylem | Microsoft. ADHybridHealthService/işlemler/okuma | Sistem tarafından desteklenen işlemlerin listesini alır. |
> | Eylem | Microsoft. ADHybridHealthService/Register/Action | Adhibrit Sistem Sağlığı Hizmeti kaynak sağlayıcısını kaydeder ve Adhibrit Sistem Sağlığı Hizmeti kaynağının oluşturulmasına izin vermez. |
> | Eylem | Microsoft. ADHybridHealthService/Reports/availabledağıtımlar/okuma | DevOps tarafından müşteri olaylarını desteklemek için kullanılan kullanılabilir bölgelerin listesini alır. |
> | Eylem | Microsoft. ADHybridHealthService/Reports/badpassword/Read | Active Directory Federasyon Hizmeti tüm kullanıcılar için hatalı parola denemelerinin listesini alır. |
> | Eylem | Microsoft. ADHybridHealthService/Reports/badpassworduseridıservıce frekansı/okuma | Belirli bir kiracı için her gün için IP başına Kullanıcı başına Hatalı Kullanıcı adı/parola denemesi sıklığı ve yeni sıraya alınmış rapor işinin nihai sonucunu içeren blob SAS URI 'sini alır. |
> | Eylem | Microsoft. ADHybridHealthService/Reports/consentedtodevopstenants/Read | DevOps onaylı kiracılar listesini alır. Genellikle müşteri desteği için kullanılır. |
> | Eylem | Microsoft. ADHybridHealthService/Reports/ıdevops/Read | Kiracının DevOps consented olup olmadığını gösteren bir değer alır. |
> | Eylem | Microsoft. ADHybridHealthService/Reports/selectdevopstenant/Read | Seçili dev Ops kiracısı için UserID 'yi (ObjectID) güncelleştirir. |
> | Eylem | Microsoft. ADHybridHealthService/Reports/selecteddeployment/Read | Belirtilen kiracı için seçili dağıtımı alır. |
> | Eylem | Microsoft. ADHybridHealthService/Reports/tenantassigneddeployment/Read | Verilen Kiracı kimliği kiracı depolama konumunu alır. |
> | Eylem | Microsoft. ADHybridHealthService/Reports/updateselecteddeployment/Read | Verilerin erişilecek coğrafi konumunu alır. |
> | Eylem | Microsoft. ADHybridHealthService/Services/Action | Kiracıdaki bir hizmet örneğini güncelleştirir. |
> | Eylem | Microsoft. ADHybridHealthService/Services/uyarılar/okuma | Bir hizmet için uyarıları okur. |
> | Eylem | Microsoft. ADHybridHealthService/Services/uyarılar/okuma | Bir hizmet için uyarıları okur. |
> | Eylem | Microsoft. ADHybridHealthService/Services/checkservicefeatureavability/Read | Hizmet adı verildiğinde, bir hizmetin bu özelliği kullanmak için gereken her şey olup olmadığını doğrular. |
> | Eylem | Microsoft. ADHybridHealthService/Services/Delete | Kiracıdaki bir hizmet örneğini siler. |
> | Eylem | Microsoft. ADHybridHealthService/Services/exporterrors/Read | Belirli bir eşitleme hizmeti için dışarı aktarma hatalarını alır. |
> | Eylem | Microsoft. ADHybridHealthService/Services/exportstatus/Read | Belirli bir hizmet için dışarı aktarma durumunu alır. |
> | Eylem | Microsoft. ADHybridHealthService/Services/feedbacktype/görüşler/Read | Belirli bir hizmet ve sunucu için uyarı geri bildirimi alır. |
> | Eylem | Microsoft. ADHybridHealthService/Services/metricmetadata/Read | Belirli bir hizmet için desteklenen ölçümlerin listesini alır.<br>Örneğin, extranet hesabı kilitleme Işlemleri, toplam başarısız Istek sayısı, bekleyen belirteç Istekleri (proxy), ADFS hizmeti için belirteç Istekleri/sn.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP etkin Iş parçacıkları, Kerberos kimlik doğrulamaları/sn, ADDomainService için ATQ Iş parçacığı toplamı.<br>Profil gecikmesini çalıştırın, TCP bağlantıları oluşturulmuştur, Insights Aracısı özel baytları, ADSync hizmeti için Azure AD 'ye Istatistikleri dışarı aktarın. |
> | Eylem | Microsoft. ADHybridHealthService/Services/ölçümler/gruplar/ortalama/okuma | Hizmet verildiğinde, bu API belirli bir hizmet için ölçümlerin ortalamasını alır.<br>Örneğin, bu API ile ilgili bilgi almak için bu API kullanılabilir: extranet hesabı kilitleme Işlemleri, toplam başarısız Istek, bekleyen belirteç Istekleri (proxy) ve ADFederation hizmeti için belirteç Istekleri/sn.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP etkin Iş parçacıkları, Kerberos kimlik doğrulamaları/sn, ADDomain hizmeti için ATQ Iş parçacığı toplamı.<br>Profil gecikmesini çalıştırın, TCP bağlantıları oluşturulmuştur, Insights Aracısı özel baytları, Istatistikleri eşitleme hizmeti için Azure AD 'ye aktarın. |
> | Eylem | Microsoft. ADHybridHealthService/Services/ölçümler/gruplar/okuma | Hizmet verildiğinde, bu API ölçüm bilgilerini alır.<br>Örneğin, bu API ile ilgili bilgi almak için bu API kullanılabilir: extranet hesabı kilitleme Işlemleri, toplam başarısız Istek, bekleyen belirteç Istekleri (proxy) ve ADFederation hizmeti için belirteç Istekleri/sn.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP etkin Iş parçacıkları, Kerberos kimlik doğrulamaları/sn, ADDomain hizmeti için ATQ Iş parçacığı toplamı.<br>Profil gecikmesini çalıştırın, TCP bağlantıları oluşturulmuştur, Insights Aracısı özel baytları, Istatistikleri eşitleme hizmeti için Azure AD 'ye aktarın. |
> | Eylem | Microsoft. ADHybridHealthService/Services/ölçümler/gruplar/Sum/Read | Bu API, bir hizmet verildiğinde, belirli bir hizmet için ölçümlerin toplu görünümünü alır.<br>Örneğin, bu API ile ilgili bilgi almak için bu API kullanılabilir: extranet hesabı kilitleme Işlemleri, toplam başarısız Istek, bekleyen belirteç Istekleri (proxy) ve ADFederation hizmeti için belirteç Istekleri/sn.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP etkin Iş parçacıkları, Kerberos kimlik doğrulamaları/sn, ADDomain hizmeti için ATQ Iş parçacığı toplamı.<br>Profil gecikmesini çalıştırın, TCP bağlantıları oluşturulmuştur, Insights Aracısı özel baytları, Istatistikleri eşitleme hizmeti için Azure AD 'ye aktarın. |
> | Eylem | Microsoft. ADHybridHealthService/Services/monitoringconfiguration/Write | Bir hizmet için izleme yapılandırması ekleme veya güncelleştirme. |
> | Eylem | Microsoft. ADHybridHealthService/Services/monitoringconfigurations/Read | Belirli bir hizmet için izleme yapılandırmasını alır. |
> | Eylem | Microsoft. ADHybridHealthService/Services/monitoringconfigurations/Write | Bir hizmet için izleme yapılandırması ekleme veya güncelleştirme. |
> | Eylem | Microsoft. ADHybridHealthService/Services/premiumcheck/Read | Bu API, bir Premium kiracının tüm eklendi hizmetlerinin listesini alır. |
> | Eylem | Microsoft. ADHybridHealthService/Services/Read | Kiracıdaki hizmet örneklerini okur. |
> | Eylem | Microsoft. ADHybridHealthService/Services/Reports/Blobursıs/Read | Son 7 güne ait tüm riskli IP rapor URI 'Lerini alır. |
> | Eylem | Microsoft. ADHybridHealthService/Services/Reports/details/Read | Son 7 günde hatalı parola hatası olan ilk 50 kullanıcının raporunu alır |
> | Eylem | Microsoft. ADHybridHealthService/Services/Reports/Generatebloburı/Action | Riskli IP raporu oluşturur ve ona işaret eden bir URI döndürür. |
> | Eylem | Microsoft. ADHybridHealthService/Services/servicemembers/Action | Hizmette bir sunucu örneği oluşturur. |
> | Eylem | Microsoft. ADHybridHealthService/Services/servicemembers/uyarılar/Read | Bir sunucu için uyarıları okur. |
> | Eylem | Microsoft. ADHybridHealthService/Services/servicemembers/Credentials/Read | Sunucu kaydı sırasında, yeni sunucuları ekleme kimlik bilgilerini almak için bu API çağırılır. |
> | Eylem | Microsoft. ADHybridHealthService/Services/servicemembers/datayeniliği/okuma | Belirli bir sunucu için bu API, sunucular tarafından karşıya yüklenen veri türlerinin bir listesini ve her yükleme için en son saati alır. |
> | Eylem | Microsoft. ADHybridHealthService/Services/servicemembers/Delete | Hizmette bir sunucu örneğini siler. |
> | Eylem | Microsoft. ADHybridHealthService/Services/servicemembers/exportstatus/Read | Belirli bir eşitleme hizmeti için eşitleme dışa aktarma hata ayrıntılarını alır. |
> | Eylem | Microsoft. ADHybridHealthService/Services/servicemembers/ölçümler/gruplar/okuma | Hizmet verildiğinde, bu API ölçüm bilgilerini alır.<br>Örneğin, bu API ile ilgili bilgi almak için bu API kullanılabilir: extranet hesabı kilitleme Işlemleri, toplam başarısız Istek, bekleyen belirteç Istekleri (proxy) ve ADFederation hizmeti için belirteç Istekleri/sn.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP etkin Iş parçacıkları, Kerberos kimlik doğrulamaları/sn, ADDomain hizmeti için ATQ Iş parçacığı toplamı.<br>Profil gecikmesini çalıştırın, TCP bağlantıları oluşturulmuştur, Insights Aracısı özel baytları, Istatistikleri eşitleme hizmeti için Azure AD 'ye aktarın. |
> | Eylem | Microsoft. ADHybridHealthService/Services/servicemembers/ölçümler/okuma | Bağlayıcılar listesini alır ve belirtilen hizmet ve hizmet üyesinin profil adlarını çalıştırır. |
> | Eylem | Microsoft. ADHybridHealthService/Services/servicemembers/Read | Hizmet içindeki sunucu örneğini okur. |
> | Eylem | Microsoft. ADHybridHealthService/Services/servicemembers/ServiceConfiguration/Read | Belirli bir kiracının hizmet yapılandırmasını alır. |
> | Eylem | Microsoft. ADHybridHealthService/Services/tenantwhitelisteleme/okuma | Belirli bir kiracının özellik beyaz listeleme durumunu alır. |
> | Eylem | Microsoft. ADHybridHealthService/Services/Write | Kiracıda bir hizmet örneği oluşturur. |
> | Eylem | Microsoft. ADHybridHealthService/Unregister/Action | Adhibrit Sistem Sağlığı Hizmeti kaynak sağlayıcısı için aboneliğin kaydını siler. |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. danışman/yapılandırma/okuma | Yapılandırma al |
> | Eylem | Microsoft. Advisor/Configurations/Write | Yapılandırma oluşturur/güncelleştirir |
> | Eylem | Microsoft. Advisor/Generatereyorumları/eylem | Öneri durumunu al |
> | Eylem | Microsoft. Advisor/Generatereyorumları/okuma | Öneri durumunu al |
> | Eylem | Microsoft. Advisor/Metadata/Read | Meta verileri al |
> | Eylem | Microsoft. Advisor/işlemler/okuma | Microsoft Advisor 'ın işlemlerini alır |
> | Eylem | Microsoft. Advisor/öneriler/kullanılabilir/eylem | Yeni öneri Microsoft Advisor 'da kullanılabilir |
> | Eylem | Microsoft. Advisor/öneriler/okuma | Önerileri okur |
> | Eylem | Microsoft. Advisor/öneriler/gizlemeleri/silme | Deletes suppression |
> | Eylem | Microsoft. Advisor/öneriler/suppressions/okuma | Gets suppressions |
> | Eylem | Microsoft. Advisor/öneriler/suppressions/Write | Creates/updates suppressions |
> | Eylem | Microsoft.Advisor/register/action | Registers the subscription for the Microsoft Advisor |
> | Eylem | Microsoft.Advisor/suppressions/delete | Deletes suppression |
> | Eylem | Microsoft.Advisor/suppressions/read | Gets suppressions |
> | Eylem | Microsoft.Advisor/suppressions/write | Creates/updates suppressions |
> | Eylem | Microsoft.Advisor/unregister/action | Unregisters the subscription for the Microsoft Advisor |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.AlertsManagement/actionRules/delete | Delete action rule in a given subscription. |
> | Eylem | Microsoft.AlertsManagement/actionRules/read | Get all the action rules for the input filters. |
> | Eylem | Microsoft.AlertsManagement/actionRules/write | Create or update action rule in a given subscription |
> | Eylem | Microsoft.AlertsManagement/alerts/changestate/action | Change the state of the alert. |
> | Eylem | Microsoft.AlertsManagement/alerts/diagnostics/read | Get all diagnostics for the alert |
> | Eylem | Microsoft.AlertsManagement/alerts/history/read | Get history of the alert |
> | Eylem | Microsoft.AlertsManagement/alerts/read | Get all the alerts for the input filters. |
> | Eylem | Microsoft.AlertsManagement/alertsList/read | Get all the alerts for the input filters across subscriptions |
> | Eylem | Microsoft.AlertsManagement/alertsMetaData/read | Get alerts meta data for the input parameter. |
> | Eylem | Microsoft.AlertsManagement/alertsSummary/read | Get the summary of alerts |
> | Eylem | Microsoft.AlertsManagement/alertsSummaryList/read | Get the summary of alerts across subscriptions |
> | Eylem | Microsoft.AlertsManagement/Operations/read | Reads the operations provided |
> | Eylem | Microsoft.AlertsManagement/register/action | Registers the subscription for the Microsoft Alerts Management |
> | Eylem | Microsoft.AlertsManagement/smartDetectorAlertRules/delete | Delete Smart Detector alert rule in a given subscription |
> | Eylem | Microsoft.AlertsManagement/smartDetectorAlertRules/read | Get all the Smart Detector alert rules for the input filters |
> | Eylem | Microsoft.AlertsManagement/smartDetectorAlertRules/write | Create or update Smart Detector alert rule in a given subscription |
> | Eylem | Microsoft.AlertsManagement/smartGroups/changestate/action | Change the state of the smart group |
> | Eylem | Microsoft.AlertsManagement/smartGroups/history/read | Get history of the smart group |
> | Eylem | Microsoft.AlertsManagement/smartGroups/read | Get all the smart groups for the input filters |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.AnalysisServices/locations/checkNameAvailability/action | Checks that given Analysis Server name is valid and not in use. |
> | Eylem | Microsoft.AnalysisServices/locations/operationresults/read | Retrieves the information of the specified operation result. |
> | Eylem | Microsoft.AnalysisServices/locations/operationstatuses/read | Retrieves the information of the specified operation status. |
> | Eylem | Microsoft.AnalysisServices/operations/read | Retrieves the information of operations |
> | Eylem | Microsoft.AnalysisServices/register/action | Registers Analysis Services resource provider. |
> | Eylem | Microsoft.AnalysisServices/servers/delete | Deletes the Analysis Server. |
> | Eylem | Microsoft.AnalysisServices/servers/listGatewayStatus/action | List the status of the gateway associated with the server. |
> | Eylem | Microsoft.AnalysisServices/servers/read | Retrieves the information of the specified Analysis Server. |
> | Eylem | Microsoft.AnalysisServices/servers/resume/action | Resumes the Analysis Server. |
> | Eylem | Microsoft.AnalysisServices/servers/skus/read | Retrieve available SKU information for the server |
> | Eylem | Microsoft.AnalysisServices/servers/suspend/action | Suspends the Analysis Server. |
> | Eylem | Microsoft.AnalysisServices/servers/write | Creates or updates the specified Analysis Server. |
> | Eylem | Microsoft.AnalysisServices/skus/read | Retrieves the information of Skus |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ApiManagement/checkNameAvailability/read | Checks if provided service name is available |
> | Eylem | Microsoft.ApiManagement/operations/read | Read all API operations available for Microsoft.ApiManagement resource |
> | Eylem | Microsoft.ApiManagement/register/action | Register subscription for Microsoft.ApiManagement resource provider |
> | Eylem | Microsoft.ApiManagement/reports/read | Get reports aggregated by time periods, geographical region, developers, products, APIs, operations, subscription and byRequest. |
> | Eylem | Microsoft.ApiManagement/service/apis/delete | Deletes the specified API of the API Management service instance. |
> | Eylem | Microsoft.ApiManagement/service/apis/diagnostics/delete | Deletes the specified Diagnostic from an API. |
> | Eylem | Microsoft.ApiManagement/service/apis/diagnostics/read | Lists all diagnostics of an API. or Gets the details of the Diagnostic for an API specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/apis/diagnostics/write | Creates a new Diagnostic for an API or updates an existing one. or Updates the details of the Diagnostic for an API specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/attachments/delete | Deletes the specified comment from an Issue. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/attachments/read | Lists all attachments for the Issue associated with the specified API. or Gets the details of the issue Attachment for an API specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/attachments/write | Creates a new Attachment for the Issue in an API or updates an existing one. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/comments/delete | Deletes the specified comment from an Issue. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/comments/read | Lists all comments for the Issue associated with the specified API. or Gets the details of the issue Comment for an API specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/comments/write | Creates a new Comment for the Issue in an API or updates an existing one. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/delete | Deletes the specified Issue from an API. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/read | Lists all issues associated with the specified API. or Gets the details of the Issue for an API specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/apis/issues/write | Creates a new Issue for an API or updates an existing one. or Updates an existing issue for an API. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/delete | Deletes the specified operation in the API. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policies/delete | Deletes the policy configuration at the Api Operation. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policies/read | Get the list of policy configuration at the API Operation level. or Get the policy configuration at the API Operation level. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policies/write | Creates or updates policy configuration for the API Operation level. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policy/delete | Delete the policy configuration at Operation level |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policy/read | Get the policy configuration at Operation level |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/policy/write | Create policy configuration at Operation level |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/read | Lists a collection of the operations for the specified API. or Gets the details of the API Operation specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/tags/delete | Detach the tag from the Operation. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/tags/read | Lists all Tags associated with the Operation. or Get tag associated with the Operation. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/tags/write | Assign tag to the Operation. |
> | Eylem | Microsoft.ApiManagement/service/apis/operations/write | Creates a new operation in the API or updates an existing one. or Updates the details of the operation in the API specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/apis/operationsByTags/read | Lists a collection of operations associated with tags. |
> | Eylem | Microsoft.ApiManagement/service/apis/policies/delete | Deletes the policy configuration at the Api. |
> | Eylem | Microsoft.ApiManagement/service/apis/policies/read | Get the policy configuration at the API level. or Get the policy configuration at the API level. |
> | Eylem | Microsoft.ApiManagement/service/apis/policies/write | Creates or updates policy configuration for the API. |
> | Eylem | Microsoft.ApiManagement/service/apis/policy/delete | Delete the policy configuration at API level |
> | Eylem | Microsoft.ApiManagement/service/apis/policy/read | Get the policy configuration at API level |
> | Eylem | Microsoft.ApiManagement/service/apis/policy/write | Create policy configuration at API level |
> | Eylem | Microsoft.ApiManagement/service/apis/products/read | Lists all Products, which the API is part of. |
> | Eylem | Microsoft.ApiManagement/service/apis/read | Lists all APIs of the API Management service instance. or Gets the details of the API specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/apis/releases/delete | Removes all releases of the API or Deletes the specified release in the API. |
> | Eylem | Microsoft.ApiManagement/service/apis/releases/read | Lists all releases of an API.<br>An API release is created when making an API Revision current.<br>Releases are also used to rollback to previous revisions.<br>Results will be paged and can be constrained by the $top and $skip parameters.<br>or Returns the details of an API release. |
> | Eylem | Microsoft.ApiManagement/service/apis/releases/write | Creates a new Release for the API. or Updates the details of the release of the API specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/apis/revisions/delete | Removes all revisions of an API |
> | Eylem | Microsoft.ApiManagement/service/apis/revisions/read | Lists all revisions of an API. |
> | Eylem | Microsoft.ApiManagement/service/apis/schemas/delete | Deletes the schema configuration at the Api. |
> | Eylem | Microsoft.ApiManagement/service/apis/schemas/read | Get the schema configuration at the API level. or Get the schema configuration at the API level. |
> | Eylem | Microsoft.ApiManagement/service/apis/schemas/write | Creates or updates schema configuration for the API. |
> | Eylem | Microsoft.ApiManagement/service/apis/tagDescriptions/delete | Delete tag description for the Api. |
> | Eylem | Microsoft.ApiManagement/service/apis/tagDescriptions/read | Lists all Tags descriptions in scope of API. Model similar to swagger - tagDescription is defined on API level but tag may be assigned to the Operations or Get Tag description in scope of API |
> | Eylem | Microsoft.ApiManagement/service/apis/tagDescriptions/write | Create/Update tag description in scope of the Api. |
> | Eylem | Microsoft.ApiManagement/service/apis/tags/delete | Detach the tag from the Api. |
> | Eylem | Microsoft.ApiManagement/service/apis/tags/read | Lists all Tags associated with the API. or Get tag associated with the API. |
> | Eylem | Microsoft.ApiManagement/service/apis/tags/write | Assign tag to the Api. |
> | Eylem | Microsoft.ApiManagement/service/apis/write | Creates new or updates existing specified API of the API Management service instance. or Updates the specified API of the API Management service instance. |
> | Eylem | Microsoft.ApiManagement/service/apisByTags/read | Lists a collection of apis associated with tags. |
> | Eylem | Microsoft.ApiManagement/service/apiVersionSets/delete | Deletes specific Api Version Set. |
> | Eylem | Microsoft.ApiManagement/service/apiVersionSets/read | Lists a collection of API Version Sets in the specified service instance. or Gets the details of the Api Version Set specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/apiVersionSets/versions/read | Get list of version entities |
> | Eylem | Microsoft.ApiManagement/service/apiVersionSets/write | Creates or Updates a Api Version Set. or Updates the details of the Api VersionSet specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/applynetworkconfigurationupdates/action | Updates the Microsoft.ApiManagement resources running in Virtual Network to pick updated Network Settings. |
> | Eylem | Microsoft.ApiManagement/service/authorizationServers/delete | Deletes specific authorization server instance. |
> | Eylem | Microsoft.ApiManagement/service/authorizationServers/listSecrets/action | Gets secrets for the authorization server. |
> | Eylem | Microsoft.ApiManagement/service/authorizationServers/read | Lists a collection of authorization servers defined within a service instance. or Gets the details of the authorization server without secrets. |
> | Eylem | Microsoft.ApiManagement/service/authorizationServers/write | Creates new authorization server or updates an existing authorization server. or Updates the details of the authorization server specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/backends/delete | Deletes the specified backend. |
> | Eylem | Microsoft.ApiManagement/service/backends/read | Lists a collection of backends in the specified service instance. or Gets the details of the backend specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/backends/reconnect/action | Notifies the APIM proxy to create a new connection to the backend after the specified timeout. If no timeout was specified, timeout of 2 minutes is used. |
> | Eylem | Microsoft.ApiManagement/service/backends/write | Creates or Updates a backend. or Updates an existing backend. |
> | Eylem | Microsoft.ApiManagement/service/backup/action | Backup API Management Service to the specified container in a user provided storage account |
> | Eylem | Microsoft.ApiManagement/service/caches/delete | Deletes specific Cache. |
> | Eylem | Microsoft.ApiManagement/service/caches/read | Lists a collection of all external Caches in the specified service instance. or Gets the details of the Cache specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/caches/write | Creates or updates an External Cache to be used in Api Management instance. or Updates the details of the cache specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/certificates/delete | Deletes specific certificate. |
> | Eylem | Microsoft.ApiManagement/service/certificates/read | Lists a collection of all certificates in the specified service instance. or Gets the details of the certificate specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/certificates/write | Creates or updates the certificate being used for authentication with the backend. |
> | Eylem | Microsoft.ApiManagement/service/contentTypes/contentItems/delete | Removes specified content item. |
> | Eylem | Microsoft.ApiManagement/service/contentTypes/contentItems/read | Returns list of content items or Returns content item details |
> | Eylem | Microsoft.ApiManagement/service/contentTypes/contentItems/write | Creates new content item or Updates specified content item |
> | Eylem | Microsoft.ApiManagement/service/contentTypes/read | Returns list of content types |
> | Eylem | Microsoft.ApiManagement/service/delete | Delete API Management Service instance |
> | Eylem | Microsoft.ApiManagement/service/diagnostics/delete | Deletes the specified Diagnostic. |
> | Eylem | Microsoft.ApiManagement/service/diagnostics/read | Lists all diagnostics of the API Management service instance. or Gets the details of the Diagnostic specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/diagnostics/write | Creates a new Diagnostic or updates an existing one. or Updates the details of the Diagnostic specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/gateways/action | Retrieves gateway configuration. or Updates gateway heartbeat. |
> | Eylem | Microsoft.ApiManagement/service/gateways/apis/delete | Deletes the specified API from the specified Gateway. |
> | Eylem | Microsoft.ApiManagement/service/gateways/apis/read | Lists a collection of the APIs associated with a gateway. |
> | Eylem | Microsoft.ApiManagement/service/gateways/apis/write | Adds an API to the specified Gateway. |
> | Eylem | Microsoft.ApiManagement/service/gateways/delete | Deletes specific Gateway. |
> | Eylem | Microsoft.ApiManagement/service/gateways/hostnameConfigurations/read | Lists the collection of hostname configurations for the specified gateway. |
> | Eylem | Microsoft.ApiManagement/service/gateways/keys/action | Retrieves gateway keys. |
> | Eylem | Microsoft.ApiManagement/service/gateways/read | Lists a collection of gateways registered with service instance. or Gets the details of the Gateway specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/gateways/regeneratePrimaryKey/action | Regenerates primary gateway key invalidationg any tokens created with it. |
> | Eylem | Microsoft.ApiManagement/service/gateways/regenerateSecondaryKey/action | Regenerates secondary gateway key invalidationg any tokens created with it. |
> | Eylem | Microsoft.ApiManagement/service/gateways/token/action | Gets the Shared Access Authorization Token for the gateway. |
> | Eylem | Microsoft.ApiManagement/service/gateways/write | Creates or updates an Gateway to be used in Api Management instance. or Updates the details of the gateway specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/getssotoken/action | Gets SSO token that can be used to login into API Management Service Legacy portal as an administrator |
> | Eylem | Microsoft.ApiManagement/service/groups/delete | Deletes specific group of the API Management service instance. |
> | Eylem | Microsoft.ApiManagement/service/groups/read | Lists a collection of groups defined within a service instance. or Gets the details of the group specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/groups/users/delete | Remove existing user from existing group. |
> | Eylem | Microsoft.ApiManagement/service/groups/users/read | Lists a collection of user entities associated with the group. |
> | Eylem | Microsoft.ApiManagement/service/groups/users/write | Add existing user to existing group |
> | Eylem | Microsoft.ApiManagement/service/groups/write | Creates or Updates a group. or Updates the details of the group specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/identityProviders/delete | Deletes the specified identity provider configuration. |
> | Eylem | Microsoft.ApiManagement/service/identityProviders/listSecrets/action | Gets Identity Provider secrets. |
> | Eylem | Microsoft.ApiManagement/service/identityProviders/read | Lists a collection of Identity Provider configured in the specified service instance. or Gets the configuration details of the identity Provider without secrets. |
> | Eylem | Microsoft.ApiManagement/service/identityProviders/write | Creates or Updates the IdentityProvider configuration. or Updates an existing IdentityProvider configuration. |
> | Eylem | Microsoft.ApiManagement/service/issues/read | Lists a collection of issues in the specified service instance. or Gets API Management issue details |
> | Eylem | Microsoft.ApiManagement/service/locations/networkstatus/read | Gets the network access status of resources on which the service depends on in the location. |
> | Eylem | Microsoft.ApiManagement/service/loggers/delete | Deletes the specified logger. |
> | Eylem | Microsoft.ApiManagement/service/loggers/read | Lists a collection of loggers in the specified service instance. or Gets the details of the logger specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/loggers/write | Creates or Updates a logger. or Updates an existing logger. |
> | Eylem | Microsoft.ApiManagement/service/managedeployments/action | Change SKU/units, add/remove regional deployments of API Management Service |
> | Eylem | Microsoft.ApiManagement/service/namedValues/delete | Deletes specific named value from the API Management service instance. |
> | Eylem | Microsoft.ApiManagement/service/namedValues/listSecrets/action | Gets the secrets of the named value specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/namedValues/read | Lists a collection of named values defined within a service instance. or Gets the details of the named value specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/namedValues/write | Creates or updates named value. or Updates the specific named value. |
> | Eylem | Microsoft.ApiManagement/service/networkstatus/read | Gets the network access status of resources on which the service depends on. |
> | Eylem | Microsoft.ApiManagement/service/notifications/action | Sends notification to a specified user |
> | Eylem | Microsoft.ApiManagement/service/notifications/read | Lists a collection of properties defined within a service instance. or Gets the details of the Notification specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientEmails/delete | Removes the email from the list of Notification. |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientEmails/read | Gets the list of the Notification Recipient Emails subscribed to a notification. |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientEmails/write | Adds the Email address to the list of Recipients for the Notification. |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientUsers/delete | Removes the API Management user from the list of Notification. |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientUsers/read | Gets the list of the Notification Recipient User subscribed to the notification. |
> | Eylem | Microsoft.ApiManagement/service/notifications/recipientUsers/write | Adds the API Management User to the list of Recipients for the Notification. |
> | Eylem | Microsoft.ApiManagement/service/notifications/write | Create or Update API Management publisher notification. |
> | Eylem | Microsoft.ApiManagement/service/openidConnectProviders/delete | Deletes specific OpenID Connect Provider of the API Management service instance. |
> | Eylem | Microsoft.ApiManagement/service/openidConnectProviders/listSecrets/action | Gets specific OpenID Connect Provider secrets. |
> | Eylem | Microsoft.ApiManagement/service/openidConnectProviders/read | Lists of all the OpenId Connect Providers. or Gets specific OpenID Connect Provider without secrets. |
> | Eylem | Microsoft.ApiManagement/service/openidConnectProviders/write | Creates or updates the OpenID Connect Provider. or Updates the specific OpenID Connect Provider. |
> | Eylem | Microsoft.ApiManagement/service/operationresults/read | Gets current status of long running operation |
> | Eylem | Microsoft.ApiManagement/service/policies/delete | Deletes the global policy configuration of the Api Management Service. |
> | Eylem | Microsoft.ApiManagement/service/policies/read | Lists all the Global Policy definitions of the Api Management service. or Get the Global policy definition of the Api Management service. |
> | Eylem | Microsoft.ApiManagement/service/policies/write | Creates or updates the global policy configuration of the Api Management service. |
> | Eylem | Microsoft.ApiManagement/service/policy/delete | Delete the policy configuration at Tenant level |
> | Eylem | Microsoft.ApiManagement/service/policy/read | Get the policy configuration at Tenant level |
> | Eylem | Microsoft.ApiManagement/service/policy/write | Create policy configuration at Tenant level |
> | Eylem | Microsoft.ApiManagement/service/policyDescriptions/read | Lists all policy descriptions. |
> | Eylem | Microsoft.ApiManagement/service/policySnippets/read | Lists all policy snippets. |
> | Eylem | Microsoft.ApiManagement/service/portalsettings/read | Get Sign In Settings for the Portal or Get Sign Up Settings for the Portal or Get Delegation Settings for the Portal. |
> | Eylem | Microsoft.ApiManagement/service/portalsettings/write | Update Sign-In settings. or Create or Update Sign-In settings. or Update Sign Up settings or Update Sign Up settings or Update Delegation settings. or Create or Update Delegation settings. |
> | Eylem | Microsoft.ApiManagement/service/products/apis/delete | Deletes the specified API from the specified product. |
> | Eylem | Microsoft.ApiManagement/service/products/apis/read | Lists a collection of the APIs associated with a product. |
> | Eylem | Microsoft.ApiManagement/service/products/apis/write | Adds an API to the specified product. |
> | Eylem | Microsoft.ApiManagement/service/products/delete | Delete product. |
> | Eylem | Microsoft.ApiManagement/service/products/groups/delete | Deletes the association between the specified group and product. |
> | Eylem | Microsoft.ApiManagement/service/products/groups/read | Lists the collection of developer groups associated with the specified product. |
> | Eylem | Microsoft.ApiManagement/service/products/groups/write | Adds the association between the specified developer group with the specified product. |
> | Eylem | Microsoft.ApiManagement/service/products/policies/delete | Deletes the policy configuration at the Product. |
> | Eylem | Microsoft.ApiManagement/service/products/policies/read | Get the policy configuration at the Product level. or Get the policy configuration at the Product level. |
> | Eylem | Microsoft.ApiManagement/service/products/policies/write | Creates or updates policy configuration for the Product. |
> | Eylem | Microsoft.ApiManagement/service/products/policy/delete | Delete the policy configuration at Product level |
> | Eylem | Microsoft.ApiManagement/service/products/policy/read | Get the policy configuration at Product level |
> | Eylem | Microsoft.ApiManagement/service/products/policy/write | Create policy configuration at Product level |
> | Eylem | Microsoft.ApiManagement/service/products/read | Lists a collection of products in the specified service instance. or Gets the details of the product specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/products/subscriptions/read | Lists the collection of subscriptions to the specified product. |
> | Eylem | Microsoft.ApiManagement/service/products/tags/delete | Detach the tag from the Product. |
> | Eylem | Microsoft.ApiManagement/service/products/tags/read | Lists all Tags associated with the Product. or Get tag associated with the Product. |
> | Eylem | Microsoft.ApiManagement/service/products/tags/write | Assign tag to the Product. |
> | Eylem | Microsoft.ApiManagement/service/products/write | Creates or Updates a product. or Update existing product details. |
> | Eylem | Microsoft.ApiManagement/service/productsByTags/read | Lists a collection of products associated with tags. |
> | Eylem | Microsoft.ApiManagement/service/properties/delete | Deletes specific property from the API Management service instance. |
> | Eylem | Microsoft.ApiManagement/service/properties/listSecrets/action | Gets the secrets of the property specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/properties/read | Lists a collection of properties defined within a service instance. or Gets the details of the property specified by its identifier. |
> | Eylem | Microsoft.ApiManagement/service/properties/write | Creates or updates a property. or Updates the specific property. |
> | Eylem | Microsoft.ApiManagement/service/quotas/periods/read | Get quota counter value for period |
> | Eylem | Microsoft.ApiManagement/service/quotas/periods/write | Set quota counter current value |
> | Eylem | Microsoft.ApiManagement/service/quotas/read | Get values for quota |
> | Eylem | Microsoft.ApiManagement/service/quotas/write | Set quota counter current value |
> | Eylem | Microsoft.ApiManagement/service/read | Read metadata for an API Management Service instance |
> | Eylem | Microsoft.ApiManagement/service/regions/read | Lists all azure regions in which the service exists. |
> | Eylem | Microsoft.ApiManagement/service/reports/read | Get report aggregated by time periods or Get report aggregated by geographical region or Get report aggregated by developers.<br>ya da ürünlere göre toplanan raporları alın.<br>ya da API 'Ler tarafından toplanan veya işleme göre toplanan ya da bir abonelik tarafından toplanan rapor almak için rapor alın.<br>ya da istekleri bildiren verileri al |
> | Eylem | Microsoft.ApiManagement/service/restore/action | API Management hizmetini Kullanıcı tarafından sağlanmış bir depolama hesabındaki belirtilen kapsayıcıdan geri yükle |
> | Eylem | Microsoft. Apimanayönetimi/hizmet/abonelik/silme | Belirtilen aboneliği siler. |
> | Eylem | Microsoft. Apimanayönetimi/hizmet/abonelikler/Listgizlilikler/eylem | Belirtilen abonelik anahtarlarını alır. |
> | Eylem | Microsoft. Apimanayönetimi/hizmet/abonelikler/okuma | API Management hizmeti örneğinin tüm aboneliklerini listeler. veya belirtilen abonelik varlığını alır (anahtarlar olmadan). |
> | Eylem | Microsoft. Apimanayönetimi/hizmet/abonelikler/regeneratePrimaryKey/Action | API Management hizmet örneğinin mevcut aboneliğinin birincil anahtarını yeniden oluşturur. |
> | Eylem | Microsoft. Apimanayönetimi/hizmet/abonelikler/regenerateSecondaryKey/Action | API Management hizmet örneğinin mevcut aboneliğinin ikincil anahtarını yeniden oluşturur. |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/abonelikleri/yazma | Belirtilen kullanıcının aboneliğini belirtilen ürüne oluşturur veya güncelleştirir. ya da kendi tanımlayıcısı tarafından belirtilen aboneliğin ayrıntılarını güncelleştirir. |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/tagResources/Read | Etiketlerle ilişkili kaynakların koleksiyonunu listeler. |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/etiketleri/silme | API Management hizmet örneğinin belirli bir etiketini siler. |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/etiketleri/okuma | Bir hizmet örneği içinde tanımlanan etiketlerin koleksiyonunu listeler. ya da kendi tanımlayıcısı tarafından belirtilen etiketin ayrıntılarını alır. |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/etiketleri/yazma | Bir etiket oluşturur. ya da kendi tanımlayıcısı tarafından belirtilen etiketin ayrıntılarını güncelleştirir. |
> | Eylem | Microsoft. Apimanayönetimi/hizmet/şablonlar/silme | Varsayılan API Management e-posta şablonunu sıfırla |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/şablonları/okuma | Tüm e-posta şablonlarını alır veya API Management e-posta şablonu ayrıntılarını alır |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/şablonları/yazma | E-posta şablonu API Management API Management e-posta şablonu veya güncelleştirmeleri oluşturun veya güncelleştirin |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/kiracı/silme | Kiracı için ilke yapılandırmasını kaldırma |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/kiracı/dağıtım/eylem | Belirtilen git dalından veritabanındaki yapılandırmaya yapılan değişiklikleri uygulamak için bir dağıtım görevi çalıştırır. |
> | Eylem | Microsoft. Apimanayönetimi/hizmet/kiracı/Listgizlilikler/eylem | Kiracı erişim bilgileri al ayrıntıları |
> | Eylem | Microsoft. Apimanayönetimi/hizmet/kiracı/operationResults/Read | İşlem sonuçlarının listesini al veya belirli bir işlemin sonucunu al |
> | Eylem | Microsoft. Apimanayönetimi/hizmet/kiracı/okuma | API Management hizmetinin genel ilke tanımını alın. ya da kiracı erişim bilgileri al ayrıntıları |
> | Eylem | Microsoft. Apimanayönetimi/hizmet/kiracı/regeneratePrimaryKey/eylem | Birincil erişim anahtarını yeniden oluştur |
> | Eylem | Microsoft. Apimanayönetimi/hizmet/kiracı/regenerateSecondaryKey/Action | İkincil erişim anahtarını yeniden oluştur |
> | Eylem | Microsoft. Apimanayönetimi/hizmet/kiracı/kaydet/eylem | Depodaki belirtilen dala yapılandırma anlık görüntüsüne sahip COMMIT oluşturur |
> | Eylem | Microsoft. Apimanayönetimi/hizmet/kiracı/syncState/Read | Son git eşitlemesinin durumunu al |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/kiracı/doğrulama/eylem | Belirtilen git dalından değişiklikleri doğrular |
> | Eylem | Microsoft. Apimanayönetimi/hizmet/kiracı/yazma | Kiracı için ilke yapılandırması ayarlama veya kiracı erişim bilgilerini güncelleştirme ayrıntıları |
> | Eylem | Microsoft.ApiManagement/service/updatecertificate/action | API Management hizmeti için SSL sertifikası yükleme |
> | Eylem | Microsoft.ApiManagement/service/updatehostname/action | API Management hizmeti için özel etki alanı adlarını kurma, güncelleştirme veya kaldırma |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/eylemi | Yeni bir kullanıcı kaydı |
> | Eylem | Microsoft. Apimanaveya hizmet/Kullanıcı/teyitler/gönderme/eylem | Onay gönderir |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/silme | Belirli kullanıcıyı siler. |
> | Eylem | Microsoft. Apimanayönetimi/hizmet/kullanıcılar/generateSsoUrl/eylem | Belirli bir kullanıcıyı geliştirici portalına imzalamak için bir kimlik doğrulama belirteci içeren bir yeniden yönlendirme URL 'SI alır. |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/grupları/okuma | Tüm Kullanıcı gruplarını listeler. |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/kimlikleri/okuma | Tüm kullanıcı kimliklerinin listesi. |
> | Eylem | Microsoft.ApiManagement/service/users/keys/read | Kullanıcıyla ilişkili anahtarları al |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/okuma | Belirtilen hizmet örneğindeki kayıtlı kullanıcıların koleksiyonunu listeler. ya da kendi tanımlayıcısı tarafından belirtilen kullanıcının ayrıntılarını alır. |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/abonelikleri/okuma | Belirtilen kullanıcının aboneliklerinin koleksiyonunu listeler. |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/belirteci/eylemi | Kullanıcı için paylaşılan erişim yetkilendirme belirtecini alır. |
> | Eylem | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/yazma | Bir kullanıcı oluşturur veya güncelleştirir. ya da kendi tanımlayıcısı tarafından belirtilen kullanıcının ayrıntılarını günceller. |
> | Eylem | Microsoft.ApiManagement/service/write | API Management hizmeti 'nin yeni bir örneğini oluşturma |
> | Eylem | Microsoft. Apimanayönetimi/kaydını kaldırma/eylem | Microsoft. Apimana, kaynak sağlayıcısı için aboneliği kayıt kaldırmayı kaldır |

## <a name="microsoftauthorization"></a>Microsoft. Authorization

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. Authorization/classicAdministrators/Delete | Yöneticiyi abonelikten kaldırır. |
> | Eylem | Microsoft. Authorization/classicAdministrators/operationdurumlarının/Read | Aboneliğin yönetici işlem durumlarını alır. |
> | Eylem | Microsoft. Authorization/classicAdministrators/Read | Aboneliğin yöneticilerini okur. |
> | Eylem | Microsoft. Authorization/classicAdministrators/Write | Add or modify administrator to a subscription. |
> | Eylem | Microsoft.Authorization/denyAssignments/delete | Delete a deny assignment at the specified scope. |
> | Eylem | Microsoft.Authorization/denyAssignments/read | Get information about a deny assignment. |
> | Eylem | Microsoft.Authorization/denyAssignments/write | Create a deny assignment at the specified scope. |
> | Eylem | Microsoft.Authorization/elevateAccess/action | Çağrı yapana kiracı kapsamında Kullanıcı Erişimi Yöneticisi erişim izni verir |
> | Eylem | Microsoft.Authorization/locks/delete | Delete locks at the specified scope. |
> | Eylem | Microsoft.Authorization/locks/read | Gets locks at the specified scope. |
> | Eylem | Microsoft.Authorization/locks/write | Add locks at the specified scope. |
> | Eylem | Microsoft.Authorization/operations/read | Gets the list of operations |
> | Eylem | Microsoft.Authorization/permissions/read | Lists all the permissions the caller has at a given scope. |
> | Eylem | Microsoft.Authorization/policies/audit/action | Action taken as a result of evaluation of Azure Policy with 'audit' effect |
> | Eylem | Microsoft.Authorization/policies/auditIfNotExists/action | Action taken as a result of evaluation of Azure Policy with 'auditIfNotExists' effect |
> | Eylem | Microsoft.Authorization/policies/deny/action | Action taken as a result of evaluation of Azure Policy with 'deny' effect |
> | Eylem | Microsoft.Authorization/policies/deployIfNotExists/action | Action taken as a result of evaluation of Azure Policy with 'deployIfNotExists' effect |
> | Eylem | Microsoft.Authorization/policyAssignments/delete | Delete a policy assignment at the specified scope. |
> | Eylem | Microsoft.Authorization/policyAssignments/read | Get information about a policy assignment. |
> | Eylem | Microsoft.Authorization/policyAssignments/write | Create a policy assignment at the specified scope. |
> | Eylem | Microsoft.Authorization/policyDefinitions/delete | Delete a policy definition. |
> | Eylem | Microsoft.Authorization/policyDefinitions/read | Get information about a policy definition. |
> | Eylem | Microsoft.Authorization/policyDefinitions/write | Create a custom policy definition. |
> | Eylem | Microsoft.Authorization/policySetDefinitions/delete | Delete a policy set definition. |
> | Eylem | Microsoft.Authorization/policySetDefinitions/read | Get information about a policy set definition. |
> | Eylem | Microsoft.Authorization/policySetDefinitions/write | Create a custom policy set definition. |
> | Eylem | Microsoft.Authorization/providerOperations/read | Get operations for all resource providers which can be used in role definitions. |
> | Eylem | Microsoft.Authorization/roleAssignments/delete | Delete a role assignment at the specified scope. |
> | Eylem | Microsoft.Authorization/roleAssignments/read | Get information about a role assignment. |
> | Eylem | Microsoft.Authorization/roleAssignments/write | Create a role assignment at the specified scope. |
> | Eylem | Microsoft.Authorization/roleDefinitions/delete | Delete the specified custom role definition. |
> | Eylem | Microsoft.Authorization/roleDefinitions/read | Get information about a role definition. |
> | Eylem | Microsoft.Authorization/roleDefinitions/write | Create or update a custom role definition with specified permissions and assignable scopes. |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Automation/automationAccounts/agentRegistrationInformation/read | Read an Azure Automation DSC's registration information |
> | Eylem | Microsoft.Automation/automationAccounts/agentRegistrationInformation/regenerateKey/action | Writes a request to regenerate Azure Automation DSC keys |
> | Eylem | Microsoft.Automation/automationAccounts/certificates/delete | Deletes an Azure Automation certificate asset |
> | Eylem | Microsoft.Automation/automationAccounts/certificates/getCount/action | Reads the count of certificates |
> | Eylem | Microsoft.Automation/automationAccounts/certificates/read | Gets an Azure Automation certificate asset |
> | Eylem | Microsoft.Automation/automationAccounts/certificates/write | Creates or updates an Azure Automation certificate asset |
> | Eylem | Microsoft.Automation/automationAccounts/compilationjobs/read | Reads an Azure Automation DSC's Compilation |
> | Eylem | Microsoft.Automation/automationAccounts/compilationjobs/read | Reads an Azure Automation DSC's Compilation |
> | Eylem | Microsoft.Automation/automationAccounts/compilationjobs/write | Writes an Azure Automation DSC's Compilation |
> | Eylem | Microsoft.Automation/automationAccounts/compilationjobs/write | Writes an Azure Automation DSC's Compilation |
> | Eylem | Microsoft.Automation/automationAccounts/configurations/content/read | Reads the configuration media content |
> | Eylem | Microsoft.Automation/automationAccounts/configurations/delete | Deletes an Azure Automation DSC's content |
> | Eylem | Microsoft.Automation/automationAccounts/configurations/getCount/action | Reads the count of an Azure Automation DSC's content |
> | Eylem | Microsoft.Automation/automationAccounts/configurations/read | Gets an Azure Automation DSC's content |
> | Eylem | Microsoft.Automation/automationAccounts/configurations/write | Writes an Azure Automation DSC's content |
> | Eylem | Microsoft.Automation/automationAccounts/connections/delete | Deletes an Azure Automation connection asset |
> | Eylem | Microsoft.Automation/automationAccounts/connections/getCount/action | Reads the count of connections |
> | Eylem | Microsoft.Automation/automationAccounts/connections/read | Gets an Azure Automation connection asset |
> | Eylem | Microsoft.Automation/automationAccounts/connections/write | Creates or updates an Azure Automation connection asset |
> | Eylem | Microsoft.Automation/automationAccounts/connectionTypes/delete | Deletes an Azure Automation connection type asset |
> | Eylem | Microsoft.Automation/automationAccounts/connectionTypes/read | Gets an Azure Automation connection type asset |
> | Eylem | Microsoft.Automation/automationAccounts/connectionTypes/write | Creates an Azure Automation connection type asset |
> | Eylem | Microsoft.Automation/automationAccounts/credentials/delete | Deletes an Azure Automation credential asset |
> | Eylem | Microsoft.Automation/automationAccounts/credentials/getCount/action | Reads the count of credentials |
> | Eylem | Microsoft.Automation/automationAccounts/credentials/read | Gets an Azure Automation credential asset |
> | Eylem | Microsoft.Automation/automationAccounts/credentials/write | Creates or updates an Azure Automation credential asset |
> | Eylem | Microsoft.Automation/automationAccounts/delete | Deletes an Azure Automation account |
> | Eylem | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/delete | Deletes Hybrid Runbook Worker Resources |
> | Eylem | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Reads Hybrid Runbook Worker Resources |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/output/read | Gets the output of a job |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/read | Gets an Azure Automation job |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/resume/action | Resumes an Azure Automation job |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/runbookContent/action | Gets the content of the Azure Automation runbook at the time of the job execution |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/stop/action | Stops an Azure Automation job |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/streams/read | Gets an Azure Automation job stream |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/streams/read | Gets an Azure Automation job stream |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/suspend/action | Suspends an Azure Automation job |
> | Eylem | Microsoft.Automation/automationAccounts/jobs/write | Creates an Azure Automation job |
> | Eylem | Microsoft.Automation/automationAccounts/jobSchedules/delete | Deletes an Azure Automation job schedule |
> | Eylem | Microsoft.Automation/automationAccounts/jobSchedules/read | Gets an Azure Automation job schedule |
> | Eylem | Microsoft.Automation/automationAccounts/jobSchedules/write | Creates an Azure Automation job schedule |
> | Eylem | Microsoft.Automation/automationAccounts/linkedWorkspace/read | Gets the workspace linked to the automation account |
> | Eylem | Microsoft.Automation/automationAccounts/listKeys/action | Reads the Keys for the automation account |
> | Eylem | Microsoft.Automation/automationAccounts/modules/activities/read | Gets Azure Automation Activities |
> | Eylem | Microsoft.Automation/automationAccounts/modules/delete | Deletes an Azure Automation Powershell module |
> | Eylem | Microsoft.Automation/automationAccounts/modules/getCount/action | Gets the count of Powershell modules within the Automation Account |
> | Eylem | Microsoft.Automation/automationAccounts/modules/read | Gets an Azure Automation Powershell module |
> | Eylem | Microsoft.Automation/automationAccounts/modules/write | Creates or updates an Azure Automation Powershell module |
> | Eylem | Microsoft.Automation/automationAccounts/nodeConfigurations/delete | Deletes an Azure Automation DSC's node configuration |
> | Eylem | Microsoft.Automation/automationAccounts/nodeConfigurations/rawContent/action | Reads an Azure Automation DSC's node configuration content |
> | Eylem | Microsoft.Automation/automationAccounts/nodeConfigurations/read | Reads an Azure Automation DSC's node configuration |
> | Eylem | Microsoft.Automation/automationAccounts/nodeConfigurations/write | Writes an Azure Automation DSC's node configuration |
> | Eylem | Microsoft.Automation/automationAccounts/nodecounts/read | Reads node count summary for the specified type |
> | Eylem | Microsoft.Automation/automationAccounts/nodes/delete | Deletes Azure Automation DSC nodes |
> | Eylem | Microsoft.Automation/automationAccounts/nodes/read | Reads Azure Automation DSC nodes |
> | Eylem | Microsoft.Automation/automationAccounts/nodes/reports/content/read | Reads Azure Automation DSC report contents |
> | Eylem | Microsoft.Automation/automationAccounts/nodes/reports/read | Reads Azure Automation DSC reports |
> | Eylem | Microsoft.Automation/automationAccounts/nodes/write | Creates or updates Azure Automation DSC nodes |
> | Eylem | Microsoft.Automation/automationAccounts/objectDataTypes/fields/read | Gets Azure Automation TypeFields |
> | Eylem | Microsoft.Automation/automationAccounts/python2Packages/delete | Deletes an Azure Automation Python 2 package |
> | Eylem | Microsoft.Automation/automationAccounts/python2Packages/read | Gets an Azure Automation Python 2 package |
> | Eylem | Microsoft.Automation/automationAccounts/python2Packages/write | Creates or updates an Azure Automation Python 2 package |
> | Eylem | Microsoft.Automation/automationAccounts/python3Packages/delete | Deletes an Azure Automation Python 3 package |
> | Eylem | Microsoft.Automation/automationAccounts/python3Packages/read | Gets an Azure Automation Python 3 package |
> | Eylem | Microsoft.Automation/automationAccounts/python3Packages/write | Creates or updates an Azure Automation Python 3 package |
> | Eylem | Microsoft.Automation/automationAccounts/read | Gets an Azure Automation account |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/content/read | Gets the content of an Azure Automation runbook |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/delete | Deletes an Azure Automation runbook |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/content/write | Creates the content of an Azure Automation runbook draft |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/operationResults/read | Gets Azure Automation runbook draft operation results |
> | Eylem | Microsoft.Automation/automationAccounts/runbooks/draft/read | Bir Azure Otomasyonu runbook taslağı alır |
> | Eylem | Microsoft. Automation/automationAccounts/runbook 'lar/taslak/test Işi/okuma | Bir Azure Otomasyonu runbook taslağı test işini alır |
> | Eylem | Microsoft. Automation/automationAccounts/runbook 'lar/taslak/test Işi/özgeçmişi/eylemi | Bir Azure Otomasyonu runbook taslağı test işini sürdürür |
> | Eylem | Microsoft. Automation/automationAccounts/runbook 'lar/taslak/testJob/durdur/eylem | Bir Azure Otomasyonu runbook taslağı test işini durduruyor |
> | Eylem | Microsoft. Automation/automationAccounts/runbook 'lar/taslak/test Işi/askıya alma/eylem | Bir Azure Otomasyonu runbook taslağı test işini askıya alır |
> | Eylem | Microsoft. Automation/automationAccounts/runbook 'lar/taslak/test Işi/yazma | Bir Azure Otomasyonu runbook taslağı test işi oluşturur |
> | Eylem | Microsoft. Automation/automationAccounts/runbook 'lar/taslak/undoEdit/Action | Azure Otomasyonu runbook taslağında yapılan düzenlemeleri geri alma |
> | Eylem | Microsoft. Automation/automationAccounts/runbook 'lar/getCount/Action | Azure Otomasyonu runbook 'larının sayısını alır |
> | Eylem | Microsoft. Automation/automationAccounts/runbook 'lar/Yayımla/eylem | Bir Azure Otomasyonu runbook taslağı yayımlar |
> | Eylem | Microsoft. Automation/automationAccounts/runbook 'lar/okuma | Bir Azure Otomasyonu runbook 'unu alır |
> | Eylem | Microsoft. Automation/automationAccounts/Runbook/yazma | Bir Azure Otomasyonu runbook 'u oluşturur veya güncelleştirir |
> | Eylem | Microsoft. Automation/automationAccounts/zamanlamalar/silme | Bir Azure Otomasyonu zamanlama varlığını siler |
> | Eylem | Microsoft. Automation/automationAccounts/zamanlamalar/getCount/Action | Azure Otomasyonu zamanlamalarının sayısını alır |
> | Eylem | Microsoft. Automation/automationAccounts/zamanlamalar/okuma | Bir Azure Otomasyonu zamanlama varlığını alır |
> | Eylem | Microsoft. Automation/automationAccounts/zamanlamalar/yazma | Bir Azure Otomasyonu zamanlama varlığı oluşturur veya güncelleştirir |
> | Eylem | Microsoft. Automation/automationAccounts/Softwareupdateconfigurationmachinerbir/Read | Bir Azure Otomasyonu yazılım güncelleştirme yapılandırması makinesi çalıştırmasını alır |
> | Eylem | Microsoft. Automation/automationAccounts/Softwareupdateconfigurationçalıştırmaları/okuma | Bir Azure Otomasyonu yazılım güncelleştirme yapılandırması çalıştırmasını alır |
> | Eylem | Microsoft. Automation/automationAccounts/softwareUpdateConfigurations/Delete | Bir Azure Otomasyonu yazılım güncelleştirme yapılandırmasını siler |
> | Eylem | Microsoft. Automation/automationAccounts/softwareUpdateConfigurations/Delete | Bir Azure Otomasyonu yazılım güncelleştirme yapılandırmasını siler |
> | Eylem | Microsoft. Automation/automationAccounts/softwareUpdateConfigurations/Read | Bir Azure Otomasyonu yazılım güncelleştirme yapılandırmasını alır |
> | Eylem | Microsoft. Automation/automationAccounts/softwareUpdateConfigurations/Read | Bir Azure Otomasyonu yazılım güncelleştirme yapılandırmasını alır |
> | Eylem | Microsoft. Automation/automationAccounts/softwareUpdateConfigurations/Write | Azure Otomasyonu yazılım güncelleştirme yapılandırması oluşturur veya güncelleştirir |
> | Eylem | Microsoft. Automation/automationAccounts/softwareUpdateConfigurations/Write | Azure Otomasyonu yazılım güncelleştirme yapılandırması oluşturur veya güncelleştirir |
> | Eylem | Microsoft. Automation/automationAccounts/STATISTICS/Read | Azure Otomasyonu Istatistiklerini alır |
> | Eylem | Microsoft. Automation/automationAccounts/Updatedeploymentmachinerbir/Read | Azure Otomasyonu güncelleştirme dağıtım makinesi edinme |
> | Eylem | Microsoft. Automation/automationAccounts/updateManagementPatchJob/Read | Bir Azure Otomasyonu güncelleştirme yönetimi düzeltme eki işini alır |
> | Eylem | Microsoft. Automation/automationAccounts/kullanımlar/okuma | Azure Otomasyonu kullanımını alır |
> | Eylem | Microsoft. Automation/automationAccounts/değişkenler/Sil | Bir Azure Otomasyonu değişken varlığını siler |
> | Eylem | Microsoft. Automation/automationAccounts/değişkenler/okuma | Bir Azure Otomasyonu değişken varlığını okur |
> | Eylem | Microsoft. Automation/automationAccounts/değişkenler/yazma | Bir Azure Otomasyonu değişken varlığı oluşturur veya güncelleştirir |
> | Eylem | Microsoft. Automation/automationAccounts/izleyicileri/Delete | Azure Otomasyonu İzleyicisi işini silme |
> | Eylem | Microsoft. Automation/automationAccounts/izleyicileri/Read | Bir Azure Otomasyonu İzleyicisi işini alır |
> | Eylem | Microsoft. Automation/automationAccounts/izleyicileri/start/Action | Azure Otomasyonu izleyici işi başlatma |
> | Eylem | Microsoft. Automation/automationAccounts/izleyicileri/durdur/eylem | Azure Otomasyonu İzleyicisi işini durdurma |
> | Eylem | Microsoft. Automation/automationAccounts/izleyicileri/Streams/okuma | Bir Azure Otomasyonu İzleyicisi iş akışı alır |
> | Eylem | Microsoft. Automation/automationAccounts/izleyicileri/watcherActions/Delete | Azure Otomasyonu İzleyicisi iş eylemlerini silme |
> | Eylem | Microsoft. Automation/automationAccounts/izleyicileri/watcherActions/Read | Bir Azure Otomasyonu İzleyicisi iş eylemlerini alır |
> | Eylem | Microsoft. Automation/automationAccounts/izleyicileri/watcherActions/Write | Azure Otomasyonu İzleyicisi iş eylemleri oluşturma |
> | Eylem | Microsoft. Automation/automationAccounts/izleyicileri/Write | Bir Azure Otomasyonu izleyici işi oluşturur |
> | Eylem | Microsoft. Automation/automationAccounts/Web kancaları/eylem | Azure Otomasyonu Web kancası için URI oluşturur |
> | Eylem | Microsoft. Automation/automationAccounts/Web kancaları/silme | Bir Azure Otomasyonu web kancasını siler  |
> | Eylem | Microsoft. Automation/automationAccounts/Web kancaları/okuma | Bir Azure Otomasyonu web kancasını okur |
> | Eylem | Microsoft. Automation/automationAccounts/Web kancaları/yazma | Bir Azure Otomasyonu Web kancası oluşturur veya güncelleştirir |
> | Eylem | Microsoft. Automation/automationAccounts/Write | Bir Azure Otomasyonu hesabı oluşturur veya güncelleştirir |
> | Eylem | Microsoft. Automation/işlemler/okuma | Azure Otomasyonu kaynakları için kullanılabilir Işlemleri alır |
> | Eylem | Microsoft. Automation/Register/ACTION | Aboneliği Azure Otomasyonu 'na kaydeder |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. AzureActiveDirectory/b2cDirectories/Delete | B2C Dizin kaynağını Sil |
> | Eylem | Microsoft.AzureActiveDirectory/b2cDirectories/read | View B2C Directory resource |
> | Eylem | Microsoft.AzureActiveDirectory/b2cDirectories/write | Create or update B2C Directory resource |
> | Eylem | Microsoft.AzureActiveDirectory/b2ctenants/read | Lists all B2C tenants where the user is a member |
> | Eylem | Microsoft.AzureActiveDirectory/operations/read | Read all API operations available for Microsoft.AzureActiveDirectory resource provider |
> | Eylem | Microsoft.AzureActiveDirectory/register/action | Register subscription for Microsoft.AzureActiveDirectory resource provider |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.AzureStack/Operations/read | Gets the properties of a resource provider operation |
> | Eylem | Microsoft.AzureStack/register/action | Registers Subscription with Microsoft.AzureStack resource provider |
> | Eylem | Microsoft.AzureStack/registrations/customerSubscriptions/delete | Deletes an Azure Stack Customer Subscription |
> | Eylem | Microsoft.AzureStack/registrations/customerSubscriptions/read | Gets the properties of an Azure Stack Customer Subscription |
> | Eylem | Microsoft.AzureStack/registrations/customerSubscriptions/write | Creates or updates an Azure Stack Customer Subscription |
> | Eylem | Microsoft.AzureStack/registrations/delete | Deletes an Azure Stack registration |
> | Eylem | Microsoft.AzureStack/registrations/getActivationKey/action | Gets the latest Azure Stack activation key |
> | Eylem | Microsoft.AzureStack/registrations/products/getProduct/action | Retrieves Azure Stack Marketplace product |
> | Eylem | Microsoft.AzureStack/registrations/products/getProducts/action | Retrieves a list of Azure Stack Marketplace products |
> | Eylem | Microsoft.AzureStack/registrations/products/listDetails/action | Retrieves extended details for an Azure Stack Marketplace product |
> | Eylem | Microsoft.AzureStack/registrations/products/read | Gets the properties of an Azure Stack Marketplace product |
> | Eylem | Microsoft.AzureStack/registrations/products/uploadProductLog/action | Record Azure Stack Marketplace product operation status and timestamp |
> | Eylem | Microsoft.AzureStack/registrations/read | Gets the properties of an Azure Stack registration |
> | Eylem | Microsoft.AzureStack/registrations/write | Creates or updates an Azure Stack registration |
> | Eylem | Microsoft.AzureStack/verificationKeys/getCurrentKey/action | Gets the current version of Azure Stack signing public key |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Batch/batchAccounts/applications/delete | Deletes an application |
> | Eylem | Microsoft.Batch/batchAccounts/applications/read | Lists applications or gets the properties of an application |
> | Eylem | Microsoft.Batch/batchAccounts/applications/versions/activate/action | Activates an application package |
> | Eylem | Microsoft.Batch/batchAccounts/applications/versions/delete | Deletes an application package |
> | Eylem | Microsoft.Batch/batchAccounts/applications/versions/read | Gets the properties of an application package |
> | Eylem | Microsoft.Batch/batchAccounts/applications/versions/write | Creates a new application package or updates an existing application package |
> | Eylem | Microsoft.Batch/batchAccounts/applications/write | Creates a new application or updates an existing application |
> | Eylem | Microsoft.Batch/batchAccounts/certificateOperationResults/read | Gets the results of a long running certificate operation on a Batch account |
> | Eylem | Microsoft.Batch/batchAccounts/certificates/cancelDelete/action | Cancels the failed deletion of a certificate on a Batch account |
> | Eylem | Microsoft.Batch/batchAccounts/certificates/delete | Deletes a certificate from a Batch account |
> | Eylem | Microsoft.Batch/batchAccounts/certificates/read | Lists certificates on a Batch account or gets the properties of a certificate |
> | Eylem | Microsoft.Batch/batchAccounts/certificates/write | Creates a new certificate on a Batch account or updates an existing certificate |
> | Eylem | Microsoft.Batch/batchAccounts/delete | Deletes a Batch account |
> | DataAction | Microsoft.Batch/batchAccounts/jobs/delete | Deletes a job from a Batch account |
> | DataAction | Microsoft.Batch/batchAccounts/jobs/read | Lists jobs on a Batch account or gets the properties of a job |
> | DataAction | Microsoft.Batch/batchAccounts/jobs/write | Creates a new job on a Batch account or updates an existing job |
> | DataAction | Microsoft.Batch/batchAccounts/jobSchedules/delete | Deletes a job schedule from a Batch account |
> | DataAction | Microsoft.Batch/batchAccounts/jobSchedules/read | Lists job schedules on a Batch account or gets the properties of a job schedule |
> | DataAction | Microsoft.Batch/batchAccounts/jobSchedules/write | Creates a new job schedule on a Batch account or updates an existing job schedule |
> | Eylem | Microsoft.Batch/batchAccounts/listkeys/action | Lists access keys for a Batch account |
> | Eylem | Microsoft.Batch/batchAccounts/operationResults/read | Uzun süre çalışan bir Batch hesabı işleminin sonuçlarını alır |
> | Eylem | Microsoft.Batch/batchAccounts/poolOperationResults/read | Gets the results of a long running pool operation on a Batch account |
> | Eylem | Microsoft.Batch/batchAccounts/pools/delete | Deletes a pool from a Batch account |
> | Eylem | Microsoft.Batch/batchAccounts/pools/disableAutoscale/action | Disables automatic scaling for a Batch account pool |
> | Eylem | Microsoft.Batch/batchAccounts/pools/read | Lists pools on a Batch account or gets the properties of a pool |
> | Eylem | Microsoft.Batch/batchAccounts/pools/stopResize/action | Stops an ongoing resize operation on a Batch account pool |
> | Eylem | Microsoft.Batch/batchAccounts/pools/write | Creates a new pool on a Batch account or updates an existing pool |
> | Eylem | Microsoft.Batch/batchAccounts/read | Lists Batch accounts or gets the properties of a Batch account |
> | Eylem | Microsoft.Batch/batchAccounts/regeneratekeys/action | Batch hesabı için erişim anahtarlarını yeniden oluşturur |
> | Eylem | Microsoft. Batch/batchAccounts/syncAutoStorageKeys/Action | Bir Batch hesabı için yapılandırılan otomatik depolama hesabı için erişim anahtarlarını eşitler |
> | Eylem | Microsoft. Batch/batchAccounts/Write | Yeni bir Batch hesabı oluşturur veya var olan bir Batch hesabını güncelleştirir |
> | Eylem | Microsoft. Batch/konumlar/accountOperationResults/Read | Uzun süre çalışan bir Batch hesabı işleminin sonuçlarını alır |
> | Eylem | Microsoft. Batch/Locations/Checknameavaılabılıty/Action | Hesap adının geçerli ve kullanımda olup olmadığını denetler. |
> | Eylem | Microsoft. Batch/konumlar/kotalar/okuma | Belirtilen Azure bölgesindeki belirtilen aboneliğin toplu kotalarını alır |
> | Eylem | Microsoft. Batch/işlemler/okuma | Microsoft. Batch kaynak sağlayıcısında kullanılabilir işlemleri listeler |
> | Eylem | Microsoft. Batch/Register/ACTION | Batch kaynak sağlayıcısı için aboneliği kaydeder ve Batch hesaplarının oluşturulmasını etkinleştirilir |
> | Eylem | Microsoft. Batch/Unregister/eylem | Batch kaynak sağlayıcısı için aboneliğin kaydını siler Batch hesaplarının oluşturulmasını önler |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. faturalandırma/billingAccounts/sözleşmeleri/okuma |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/billingPermissions/Read |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/Billingprofiller/billingPermissions/Read |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/billingProfiles/müşteriler/okuma |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/billingProfiles/faturalar/fiyat listesi/download/ACTION |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/billingProfiles/ınvoicesections/billingPermissions/Read |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/billingProfiles/ınvoicesections/Billingabonelikabonelikleri/taşıma/eylem |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/billingProfiles/ınvoicesections/Billingabonelikabonelikleri/aktarım/eylem |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/billingProfiles/ınvoicesections/Billingabonelikleri/Validatemoveuygunluğu/eylemi |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/billingProfiles/ınvoicesections/ürünler/Move/Action |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/billingProfiles/ınvoicesections/ürünler/aktarım/eylem |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/billingProfiles/ınvoicesections/Products/Validatemoveuygunluk/eylem |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/billingProfiles/ınvoicesections/Read |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/billingProfiles/ınvoicesections/Write |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/billingProfiles/fiyat listesi/download/Action |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/billingProfiles/okuma |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/Billingprofiller/Write |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/Billingprofiller/Write |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/Billingabonelikleri/okuma |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/müşteriler/billingPermissions/Read |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/müşteriler/okuma |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/departmanlar/okuma |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/Kayıthesapsayısı/billingPermissions/Read |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/KayıtSayısı/okuma |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/kayıt/veya tüm Izinler/okuma |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/Listınvoicesectionswithcreatesubscriptionpermission/Action |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/ürünler/okuma |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/okuma |  |
> | Eylem | Microsoft. faturalandırma/billingAccounts/Write |  |
> | Eylem | Microsoft. faturalandırma/departmanlar/okuma |  |
> | Eylem | Microsoft. faturalandırma/faturalar/indir/eylem | Listeden indirme bağlantısını kullanarak fatura indir |
> | Eylem | Microsoft. faturalandırma/faturalar/okuma |  |
> | Eylem | Microsoft. faturalandırma/Register/ACTION |  |
> | Eylem | Microsoft. faturalandırma/validateAddress/ACTION |  |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. BingMaps/Mapapsıs/Delete | Silme Işlemi |
> | Eylem | Microsoft. BingMaps/Mapapsıs/Listgizlilikler/Action | Gizli dizileri listeleyin |
> | Eylem | Microsoft. BingMaps/Mapapsıs/listSingleSignOnToken/Action | Kaynak Için çoklu oturum açma yetkilendirme belirtecini oku |
> | Eylem | Microsoft. BingMaps/Mapapsıs/okuma | Okuma Işlemi |
> | Eylem | Microsoft. BingMaps/Mapapsıs/regenerateKey/Action | Anahtarı yeniden oluşturur |
> | Eylem | Microsoft. BingMaps/Mapapsıs/Write | Yazma Işlemi |
> | Eylem | Microsoft. BingMaps/Işlemler/okuma | İşlemin açıklaması. |

## <a name="microsoftblockchain"></a>Microsoft. Blockzinciri

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. Blockzincirine/blockchainMembers/Delete | Varolan bir blok zinciri üyesini siler. |
> | Eylem | Microsoft. Blockzincirine/blockchainMembers/listApiKeys/eylem | Varolan blok zinciri üye API anahtarlarını alır veya listeler. |
> | Eylem | Microsoft. Blockzincirine/blockchainMembers/Read | Varolan blok zinciri üyelerini alır veya listeler. |
> | DataAction | Microsoft. Blockzincirini/blockchainMembers/transactionNodes/Connect/ACTION | Bir blok zinciri üye Işlem düğümüne bağlanır. |
> | Eylem | Microsoft. Blockzincirini/blockchainMembers/transactionNodes/Delete | Varolan bir blok zinciri üye Işlem düğümünü siler. |
> | Eylem | Microsoft. Blockzincirine/blockchainMembers/transactionNodes/listApiKeys/eylem | Varolan blok zinciri üye Işlem düğümü API anahtarlarını alır veya listeler. |
> | Eylem | Microsoft. Blockzincirini/blockchainMembers/transactionNodes/Read | Varolan blok zinciri üye Işlem düğümlerini alır veya listeler. |
> | Eylem | Microsoft. Blockzincirine/blockchainMembers/transactionNodes/Write | Bir blok zinciri üye Işlem düğümü oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. Blockzincirine/blockchainMembers/Write | Bir blok zinciri üyesi oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. Blockzincirine/Cordadmembers/Delete | Varolan bir blok zinciri Corda üyesini siler. |
> | Eylem | Microsoft. Blockzinciri/Cordadmembers/Read | Varolan blok zinciri Corda üyelerini alır veya listeler. |
> | Eylem | Microsoft. Blockzinciri/Cordadmembers/Write | Bir blok zinciri Corda üyesi oluşturur veya güncelleştirir. |
> | Eylem | Microsoft.Blockchain/locations/blockchainMemberOperationResults/read | Gets the Operation Results of Blockchain Members. |
> | Eylem | Microsoft.Blockchain/locations/checkNameAvailability/action | Checks that resource name is valid and is not in use. |
> | Eylem | Microsoft.Blockchain/operations/read | List all Operations in Microsoft Blockchain Resource Provider. |
> | Eylem | Microsoft.Blockchain/register/action | Registers the subscription for the Blockchain Resource Provider. |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Blueprint/blueprintAssignments/assignmentOperations/read | Read any blueprint artifacts |
> | Eylem | Microsoft.Blueprint/blueprintAssignments/delete | Herhangi bir şema yapıtını silin |
> | Eylem | Microsoft.Blueprint/blueprintAssignments/read | Read any blueprint artifacts |
> | Eylem | Microsoft.Blueprint/blueprintAssignments/whoisblueprint/action | Get Azure Blueprints service principal object Id. |
> | Eylem | Microsoft.Blueprint/blueprintAssignments/write | Herhangi bir şema yapıtını oluşturun veya güncelleştirin |
> | Eylem | Microsoft.Blueprint/blueprints/artifacts/delete | Herhangi bir şema yapıtını silin |
> | Eylem | Microsoft.Blueprint/blueprints/artifacts/read | Read any blueprint artifacts |
> | Eylem | Microsoft.Blueprint/blueprints/artifacts/write | Herhangi bir şema yapıtını oluşturun veya güncelleştirin |
> | Eylem | Microsoft.Blueprint/blueprints/delete | Delete any blueprints |
> | Eylem | Microsoft.Blueprint/blueprints/read | Read any blueprints |
> | Eylem | Microsoft.Blueprint/blueprints/versions/artifacts/read | Read any blueprint artifacts |
> | Eylem | Microsoft.Blueprint/blueprints/versions/delete | Delete any blueprints |
> | Eylem | Microsoft.Blueprint/blueprints/versions/read | Read any blueprints |
> | Eylem | Microsoft.Blueprint/blueprints/versions/write | Create or update any blueprints |
> | Eylem | Microsoft.Blueprint/blueprints/write | Create or update any blueprints |
> | Eylem | Microsoft.Blueprint/register/action | Registers the Azure Blueprints Resource Provider |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.BotService/botServices/channels/delete | Delete a Bot Service |
> | Eylem | Microsoft.BotService/botServices/channels/read | Read a Bot Service |
> | Eylem | Microsoft.BotService/botServices/channels/write | Write a Bot Service |
> | Eylem | Microsoft.BotService/botServices/connections/delete | Delete a Bot Service |
> | Eylem | Microsoft.BotService/botServices/connections/read | Read a Bot Service |
> | Eylem | Microsoft.BotService/botServices/connections/write | Write a Bot Service |
> | Eylem | Microsoft.BotService/botServices/delete | Delete a Bot Service |
> | Eylem | Microsoft.BotService/botServices/read | Read a Bot Service |
> | Eylem | Microsoft.BotService/botServices/write | Write a Bot Service |
> | Eylem | Microsoft.BotService/locations/operationresults/read | Read the status of an asynchronous operation |
> | Eylem | Microsoft.BotService/Operations/read | Read the operations for all resource types |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Cache/checknameavailability/action | Checks if a name is available for use with a new Redis Cache |
> | Eylem | Microsoft.Cache/locations/operationresults/read | Gets the result of a long running operation for which the 'Location' header was previously returned to the client |
> | Eylem | Microsoft.Cache/operations/read | Lists the operations that 'Microsoft.Cache' provider supports. |
> | Eylem | Microsoft.Cache/redis/delete | Delete the entire Redis Cache |
> | Eylem | Microsoft.Cache/redis/export/action | Export Redis data to prefixed storage blobs in specified format |
> | Eylem | Microsoft.Cache/redis/firewallRules/delete | Delete IP firewall rules of a Redis Cache |
> | Eylem | Microsoft.Cache/redis/firewallRules/read | Get the IP firewall rules of a Redis Cache |
> | Eylem | Microsoft.Cache/redis/firewallRules/write | Edit the IP firewall rules of a Redis Cache |
> | Eylem | Microsoft.Cache/redis/forceReboot/action | Force reboot a cache instance, potentially with data loss. |
> | Eylem | Microsoft.Cache/redis/import/action | Import data of a specified format from multiple blobs into Redis |
> | Eylem | Microsoft.Cache/redis/linkedservers/delete | Delete Linked Server from a Redis Cache |
> | Eylem | Microsoft.Cache/redis/linkedservers/read | Get Linked Servers associated with a redis cache. |
> | Eylem | Microsoft.Cache/redis/linkedservers/write | Add Linked Server to a Redis Cache |
> | Eylem | Microsoft.Cache/redis/listKeys/action | View the value of Redis Cache access keys in the management portal |
> | Eylem | Microsoft.Cache/redis/listUpgradeNotifications/read | List the latest Upgrade Notifications for the cache tenant. |
> | Eylem | Microsoft.Cache/redis/metricDefinitions/read | Gets the available metrics for a Redis Cache |
> | Eylem | Microsoft.Cache/redis/patchSchedules/delete | Delete the patch schedule of a Redis Cache |
> | Eylem | Microsoft.Cache/redis/patchSchedules/read | Gets the patching schedule of a Redis Cache |
> | Eylem | Microsoft.Cache/redis/patchSchedules/write | Modify the patching schedule of a Redis Cache |
> | Eylem | Microsoft.Cache/redis/read | View the Redis Cache's settings and configuration in the management portal |
> | Eylem | Microsoft.Cache/redis/regenerateKey/action | Change the value of Redis Cache access keys in the management portal |
> | Eylem | Microsoft.Cache/redis/start/action | Start a cache instance. |
> | Eylem | Microsoft.Cache/redis/stop/action | Stop a cache instance. |
> | Eylem | Microsoft.Cache/redis/write | Modify the Redis Cache's settings and configuration in the management portal |
> | Eylem | Microsoft.Cache/register/action | Registers the 'Microsoft.Cache' resource provider with a subscription |
> | Eylem | Microsoft.Cache/unregister/action | Unregisters the 'Microsoft.Cache' resource provider with a subscription |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Capacity/appliedreservations/read | Tüm rezervasyonları oku |
> | Eylem | Microsoft.Capacity/calculateexchange/action | Computes the exchange amount and price of new purchase and returns policy Errors. |
> | Eylem | Microsoft.Capacity/calculateprice/action | Calculate any Reservation Price |
> | Eylem | Microsoft. Capacity/kataloglar/okuma | Ayırma kataloğunu okuyun |
> | Eylem | Microsoft. Capacity/checktekliflere/eyleme | Tüm abonelik tekliflerini denetleyin |
> | Eylem | Microsoft. Capacity/checkscopes/Action | Herhangi bir aboneliği denetleyin |
> | Eylem | Microsoft. Capacity/ticari cıalrezervationorders/Read | Herhangi bir kiracıda oluşturulan rezervasyon emirlerini al |
> | Eylem | Microsoft. Capacity/Exchange/eylem | Herhangi bir ayırmayı Exchange |
> | Eylem | Microsoft. Capacity/işlemler/okuma | Herhangi bir Işlemi okuyun |
> | Eylem | Microsoft. Capacity/Register/ACTION | Kapasite kaynak sağlayıcısını kaydeder ve kapasite kaynaklarının oluşturulmasına izin vermez. |
> | Eylem | Microsoft. Capacity/rezervationorders/ACTION | Tüm ayırmaları güncelleştirme |
> | Eylem | Microsoft. Capacity/rezervationorders/availablescopes/eylem | Kullanılabilir herhangi bir kapsamı bulun |
> | Eylem | Microsoft. Capacity/rezervationorders/hesaplaizterefund/eylem | Para iadesi tutarını ve yeni satın alma fiyatını hesaplar ve ilke hatalarını döndürür. |
> | Eylem | Microsoft. Capacity/rezervationorders/Delete | Tüm ayırmaları Sil |
> | Eylem | Microsoft. Capacity/rezervationorders/Merge/Action | Tüm ayırmaları birleştirme |
> | Eylem | Microsoft. Capacity/rezervationorders/Read | Tüm rezervasyonları oku |
> | Eylem | Microsoft. Capacity/rezervationorders/rezervasyonlar/eylem | Tüm ayırmaları güncelleştirme |
> | Eylem | Microsoft. Capacity/rezervationorders/rezervasyonlar/availablescopes/eylem | Kullanılabilir herhangi bir kapsamı bulun |
> | Eylem | Microsoft. Capacity/rezervationorders/rezervasyonlar/Delete | Tüm ayırmaları Sil |
> | Eylem | Microsoft. Capacity/rezervationorders/rezervasyonlar/okundu | Tüm rezervasyonları oku |
> | Eylem | Microsoft. Capacity/rezervationorders/rezervasyonlar/düzeltmeler/okuma | Tüm rezervasyonları oku |
> | Eylem | Microsoft. Capacity/rezervationorders/rezervasyonlar/Write | Herhangi bir ayırma oluşturun |
> | Eylem | Microsoft. Capacity/rezervationorders/Return/ACTION | Herhangi bir ayırmayı döndürme |
> | Eylem | Microsoft. Capacity/rezervationorders/Split/ACTION | Tüm ayırmaları Böl |
> | Eylem | Microsoft. Capacity/rezervationorders/Swap/ACTION | Tüm ayırmaları değiştirme |
> | Eylem | Microsoft. Capacity/rezervationorders/Write | Herhangi bir ayırma oluşturun |
> | Eylem | Microsoft. Capacity/kiracılar/Register/Action | Tüm kiracıyı Kaydet |
> | Eylem | Microsoft. Capacity/Unregister/eylem | Tüm kiracının kaydını sil |
> | Eylem | Microsoft. Capacity/validatereservationorder/eylem | Tüm ayırmaları doğrulama |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. CDN/cdnwebapplicationfirewallmanagedrulesets/Delete |  |
> | Eylem | Microsoft. CDN/cdnwebapplicationfirewallmanagedrulesets/Read |  |
> | Eylem | Microsoft. CDN/cdnwebapplicationfirewallmanagedrulesets/Write |  |
> | Eylem | Microsoft. CDN/cdnwebapplicationfirewallpolicies/Delete |  |
> | Eylem | Microsoft. CDN/cdnwebapplicationfirewallpolicies/okuma |  |
> | Eylem | Microsoft. CDN/cdnwebapplicationfirewallpolicies/Write |  |
> | Eylem | Microsoft. CDN/Checknameavaılabılıty/Action |  |
> | Eylem | Microsoft. CDN/CheckResourceUsage/eylem |  |
> | Eylem | Microsoft. CDN/edgenodes/Delete |  |
> | Eylem | Microsoft. CDN/edgenodes/okuma |  |
> | Eylem | Microsoft. CDN/edgenodes/yazma |  |
> | Eylem | Microsoft. CDN/operationresults/cdnwebapplicationfirewallpolicyresults/Delete |  |
> | Eylem | Microsoft. CDN/operationresults/cdnwebapplicationfirewallpolicyresults/Read |  |
> | Eylem | Microsoft. CDN/operationresults/cdnwebapplicationfirewallpolicyresults/Write |  |
> | Eylem | Microsoft. CDN/operationresults/Delete |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/CheckResourceUsage/Action |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/silme |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/CheckResourceUsage/Action |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/customdomainresults/Delete |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/customdomainresults/DisableCustomHttps/Action |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/customdomainresults/EnableCustomHttps/Action |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/customdomainresults/Read |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/customdomainresults/Write |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/Delete |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/Load/Action |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/originresults/Delete |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/originresults/Read |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/originresults/Write |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/Temizleme/eylem |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/Read |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/start/Action |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/durdur/Action |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/ValidateCustomDomain/ACTION |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/endpointresults/Write |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/GenerateSsoUri/eylem |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/GetSupportedOptimizationTypes/eylem |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/okuma |  |
> | Eylem | Microsoft. CDN/operationresults/profileresults/yazma |  |
> | Eylem | Microsoft. CDN/operationresults/Read |  |
> | Eylem | Microsoft. CDN/operationresults/Write |  |
> | Eylem | Microsoft. CDN/işlemler/okuma |  |
> | Eylem | Microsoft. CDN/Profiles/CheckResourceUsage/Action |  |
> | Eylem | Microsoft. CDN/profiller/silme |  |
> | Eylem | Microsoft. CDN/profiller/uç noktaları/CheckResourceUsage/ACTION |  |
> | Eylem | Microsoft. CDN/profiller/uç noktaları/customdomains/Delete |  |
> | Eylem | Microsoft. CDN/Profiles/uç noktaları/customdomains/DisableCustomHttps/eylem |  |
> | Eylem | Microsoft. CDN/Profiles/uç noktaları/customdomains/EnableCustomHttps/eylem |  |
> | Eylem | Microsoft. CDN/Profiles/uç noktaları/customdomains/Read |  |
> | Eylem | Microsoft. CDN/Profiles/uç noktaları/customdomains/Write |  |
> | Eylem | Microsoft. CDN/profiller/uç noktaları/silme |  |
> | Eylem | Microsoft. CDN/profiller/uç noktaları/yükleme/eylem |  |
> | Eylem | Microsoft. CDN/profiller/uç noktaları/kaynakları/Sil |  |
> | Eylem | Microsoft. CDN/profiller/uç noktaları/kaynaklar/okuma |  |
> | Eylem | Microsoft. CDN/profiller/uç noktaları/çıkış/yazma |  |
> | Eylem | Microsoft. CDN/profiller/uç noktaları/Temizleme/eylem |  |
> | Eylem | Microsoft. CDN/profiller/uç noktaları/okuma |  |
> | Eylem | Microsoft. CDN/profiller/uç noktaları/Başlat/eylem |  |
> | Eylem | Microsoft. CDN/profiller/uç noktaları/durdur/eylem |  |
> | Eylem | Microsoft. CDN/Profiles/uç noktaları/ValidateCustomDomain/ACTION |  |
> | Eylem | Microsoft. CDN/profiller/uç noktaları/yazma |  |
> | Eylem | Microsoft. CDN/Profiles/GenerateSsoUri/eylem |  |
> | Eylem | Microsoft. CDN/Profiles/GetSupportedOptimizationTypes/Action |  |
> | Eylem | Microsoft. CDN/profiller/okuma |  |
> | Eylem | Microsoft. CDN/profiller/yazma |  |
> | Eylem | Microsoft. CDN/Register/ACTION | CDN kaynak sağlayıcısı için aboneliği kaydeder ve CDN profillerinin oluşturulmasına izin vermez. |
> | Eylem | Microsoft. CDN/Validatearaştırma/eylem |  |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. CertificateRegistration/certificateOrders/sertifikalar/Sil | Mevcut bir sertifikayı silme |
> | Eylem | Microsoft. CertificateRegistration/certificateOrders/sertifikalar/okuma | Sertifika listesini al |
> | Eylem | Microsoft. CertificateRegistration/certificateOrders/Certificates/Write | Yeni bir sertifika ekleme veya var olan bir sertifikayı güncelleştirme |
> | Eylem | Microsoft. CertificateRegistration/certificateOrders/Delete | Var olan bir AppServiceCertificate silme |
> | Eylem | Microsoft. CertificateRegistration/certificateOrders/Read | CertificateOrders listesini alın |
> | Eylem | Microsoft. CertificateRegistration/certificateOrders/restart/Action | Mevcut bir sertifika siparişi 'ı yeniden gönderin |
> | Eylem | Microsoft. CertificateRegistration/certificateOrders/Yenile/eylem | Mevcut bir sertifika siparişini Yenile |
> | Eylem | Microsoft. CertificateRegistration/certificateOrders/resendEmail/Action | Sertifika e-postasını yeniden gönder |
> | Eylem | Microsoft. CertificateRegistration/certificateOrders/Resendrequestemaıls/eylem | İstek e-postalarını başka bir e-posta adresine yeniden |
> | Eylem | Microsoft. CertificateRegistration/certificateOrders/Resendrequestemaıls/eylem | Verilen App Service Sertifikası için site mühürlemek alın |
> | Eylem | Microsoft. CertificateRegistration/certificateOrders/retrieveCertificateActions/Action | Sertifika eylemlerinin listesini al |
> | Eylem | Microsoft. CertificateRegistration/certificateOrders/retrieveEmailHistory/Action | Sertifika e-posta geçmişini al |
> | Eylem | Microsoft. CertificateRegistration/certificateOrders/Verifydomainsahiplik/eylem | Etki alanı sahipliğini doğrulama |
> | Eylem | Microsoft. CertificateRegistration/certificateOrders/Write | Yeni bir sertifika siparişi ekleme veya var olanı güncelleştirme |
> | Eylem | Microsoft. CertificateRegistration/işlemler/okuma | App Service sertifika kaydından tüm işlemleri Listele |
> | Eylem | Microsoft. CertificateRegistration/provisionGlobalAppServicePrincipalInUserTenant/Action | Hizmet uygulaması sorumlusu için hizmet sorumlusu sağlama |
> | Eylem | Microsoft. CertificateRegistration/Register/ACTION | Abonelik için Microsoft sertifikaları kaynak sağlayıcısını kaydetme |
> | Eylem | Microsoft. CertificateRegistration/Validatecertificateregistrationınformation/Action | Sertifika satın alma nesnesini göndermeden doğrula |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. ClassicCompute/Capabilities/Read | Özellikleri gösterir |
> | Eylem | Microsoft. ClassicCompute/Checkdomainnameavaılabılıty/Action | Belirli bir etki alanı adının kullanılabilirliğini denetler. |
> | Eylem | Microsoft. ClassicCompute/Checkdomainnameavaılabılıty/Read | Belirli bir etki alanı adının kullanılabilirliğini alır. |
> | Eylem | Microsoft. ClassicCompute/domainNames/etkin/Write | Etkin etki alanı adını ayarlar. |
> | Eylem | Microsoft. ClassicCompute/domainNames/kullanılabilirliği \ kümeler/okundu | Kaynak için kullanılabilirlik kümesini görüntüleyin. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yetenekler/okuma | Etki alanı adı yeteneklerini gösterir |
> | Eylem | Microsoft. ClassicCompute/domainNames/Delete | Kaynaklar için etki alanı adlarını kaldırın. |
> | Eylem | Microsoft. ClassicCompute/domainNames/deploymentyuvaları/okuma | Dağıtım yuvalarını gösterir. |
> | Eylem | Microsoft. ClassicCompute/domainNames/deploymentyuvaları/roller/okuma | Etki alanı adının dağıtım yuvasında rol al |
> | Eylem | Microsoft. ClassicCompute/domainNames/deploymentyuvaları/roller/roleınstances/Read | Etki alanı adının dağıtım yuvasındaki rolün rol örneğini al |
> | Eylem | Microsoft. ClassicCompute/domainNames/deploymentyuvaları/durumu/okuma | Dağıtım yuvası durumunu alın. |
> | Eylem | Microsoft. ClassicCompute/domainNames/deploymentyuvaları/durum/yazma | Dağıtım yuvası durumunu ekleyin. |
> | Eylem | Microsoft. ClassicCompute/domainNames/deploymentyuvaları/upgradedomain/Read | Etki alanı adındaki dağıtım yuvası için yükseltme etki alanını al |
> | Eylem | Microsoft. ClassicCompute/domainNames/deploymentyuvaları/upgradedomain/Write | Etki alanı adında dağıtım yuvası için yükseltme etki alanını güncelleştir |
> | Eylem | Microsoft. ClassicCompute/domainNames/deploymentyuvaları/yazma | Dağıtımı oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. ClassicCompute/domainNames/Extensions/Sil | Etki alanı adı uzantılarını kaldırın. |
> | Eylem | Microsoft. ClassicCompute/domainNames/Extensions/Operationdurumlarının/Read | Etki alanı adları uzantılarının işlem durumunu okur. |
> | Eylem | Microsoft. ClassicCompute/domainNames/Extensions/Read | Etki alanı adı uzantılarını döndürür. |
> | Eylem | Microsoft. ClassicCompute/domainNames/Extensions/Write | Etki alanı adı uzantılarını ekleyin. |
> | Eylem | Microsoft. ClassicCompute/domainNames/internalLoadBalancers/Delete | Yeni bir iç yük dengesini kaldırın. |
> | Eylem | Microsoft. ClassicCompute/domainNames/internalLoadBalancers/Operationdurumlarının/Read | Etki alanı adları iç yük dengeleyiciler için işlem durumunu okur. |
> | Eylem | Microsoft. ClassicCompute/domainNames/internalLoadBalancers/Read | İç yük dengeleyicileri alır. |
> | Eylem | Microsoft. ClassicCompute/domainNames/internalLoadBalancers/Write | Yeni bir iç Yük Dengeleme oluşturur. |
> | Eylem | Microsoft. ClassicCompute/domainNames/loadBalancedEndpointSets/Operationdurumlarının/Read | Etki alanı adları yük dengeli uç nokta kümeleri için işlem durumunu okur. |
> | Eylem | Microsoft. ClassicCompute/domainNames/loadBalancedEndpointSets/Read | Yük dengeli uç nokta kümelerini alın. |
> | Eylem | Microsoft. ClassicCompute/domainNames/loadBalancedEndpointSets/Write | Yük dengeli uç nokta kümesini ekleyin. |
> | Eylem | Microsoft. ClassicCompute/domainNames/operationdurumlarının/Read | Etki alanı adının işlem durumunu alır. |
> | Eylem | Microsoft. ClassicCompute/domainNames/Operationdurumlarının/Read | Etki alanı adları uzantılarının işlem durumunu okur. |
> | Eylem | Microsoft. ClassicCompute/domainNames/Read | Kaynaklar için etki alanı adlarını döndürün. |
> | Eylem | Microsoft. ClassicCompute/domainNames/serviceCertificates/Delete | Kullanılan hizmet sertifikalarını silin. |
> | Eylem | Microsoft. ClassicCompute/domainNames/serviceCertificates/Operationdurumlarının/Read | Etki alanı adları hizmet sertifikaları için işlem durumunu okur. |
> | Eylem | Microsoft. ClassicCompute/domainNames/serviceCertificates/Read | Kullanılan hizmet sertifikalarını döndürür. |
> | Eylem | Microsoft. ClassicCompute/domainNames/serviceCertificates/Write | Kullanılan hizmet sertifikalarını ekleyin veya değiştirin. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/abortMigration/Action | Dağıtım yuvasının geçişini durdurur. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/commitMigration/Action | Bir dağıtım yuvasının geçişini kaydeder. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/Delete | Verilen bir dağıtım yuvasını siler. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/Operationdurumlarının/Read | Etki alanı adı yuvaları için işlem durumunu okur. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/prepareMigration/Action | Bir dağıtım yuvasının geçişini hazırlar. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/okuma | Dağıtım yuvalarını gösterir. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/extensionReferences/Delete | Dağıtım yuvası rolü için uzantı başvurusunu kaldırın. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/extensionReferences/Operationdurumlarının/Read | Etki alanı adı yuvaları rol uzantısı başvuruları için işlem durumunu okur. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/extensionReferences/Read | Dağıtım yuvası rolü için uzantı başvurusunu döndürür. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/extensionReferences/Write | Dağıtım yuvası rolü için uzantı başvurusunu ekleyin veya değiştirin. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/MetricDefinitions/okuma | Etki alanı adı için rol ölçümü tanımını alın. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/ölçümler/okuma | Etki alanı adı için rol ölçümünü al. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/operationdurumlarının/Read | Etki alanı adları yuva rolü için işlem durumunu alır. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Read | Tanılama ayarlarını alın. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/sağlayıcılar/Microsoft. Insights/metricDefinitions/Read | Ölçüm tanımlarını alır. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/okuma | Dağıtım yuvasının rolünü alın. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/Roleınstances/downloadremotedesktopconnectionfile/Action | Etki alanı adı yuva rolündeki rol örneği için Uzak Masaüstü bağlantı dosyasını indirir. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/Roleınstances/Operationdurumlarının/Read | Etki alanı adları yuva rolündeki rol örneği için işlem durumunu alır. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/Roleınstances/Read | Rol örneğini alın. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/Roleınstances/Rebuild/Action | Rol örneğini yeniden oluşturur. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/Roleınstances/ReImage/Action | Rol örneğini yeniden görüntüler. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/Roleınstances/restart/Action | Rol örneklerini yeniden başlatır. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/SKU/okuma | Dağıtım yuvası için rol SKU 'su alın. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/roller/Write | Dağıtım yuvası için rol ekleyin. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/start/Action | Bir dağıtım yuvası başlatır. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/eyalet/Başlat/yaz | Dağıtım yuvası durumunu Durduruldu olarak değiştirir. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/eyalet/durdur/yaz | Dağıtım yuvası durumunu başlatıldı olarak değiştirir. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/durdur/eylem | Dağıtım yuvasını askıya alır. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/upgradeDomain/Write | Etki alanını yükseltme. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/validateMigration/Action | Dağıtım yuvasının geçişini doğrular. |
> | Eylem | Microsoft. ClassicCompute/domainNames/yuvalar/Write | Dağıtımı oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. ClassicCompute/domainNames/Swap/Action | Hazırlama yuvasını üretim yuvasına değiştirir. |
> | Eylem | Microsoft.ClassicCompute/domainNames/write | Add or modify the domain names for resources. |
> | Eylem | Microsoft.ClassicCompute/moveSubscriptionResources/action | Move all classic resources to a different subscription. |
> | Eylem | Microsoft.ClassicCompute/operatingSystemFamilies/read | Lists the guest operating system families available in Microsoft Azure, and also lists the operating system versions available for each family. |
> | Eylem | Microsoft.ClassicCompute/operatingSystems/read | Lists the versions of the guest operating system that are currently available in Microsoft Azure. |
> | Eylem | Microsoft.ClassicCompute/operations/read | Gets the list of operations. |
> | Eylem | Microsoft.ClassicCompute/operationStatuses/read | Reads the operation status for the resource. |
> | Eylem | Microsoft.ClassicCompute/quotas/read | Aboneliğin kotasını alın. |
> | Eylem | Microsoft.ClassicCompute/register/action | Register to Classic Compute |
> | Eylem | Microsoft.ClassicCompute/resourceTypes/skus/read | Gets the Sku list for supported resource types. |
> | Eylem | Microsoft.ClassicCompute/validateSubscriptionMoveAvailability/action | Validate the subscription's availability for classic move operation. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/delete | Deletes the network security group associated with the virtual machine. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/read | Reads the operation status for the virtual machines associated network security groups. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/read | Gets the network security group associated with the virtual machine. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/write | Adds a network security group associated with the virtual machine. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/asyncOperations/read | Gets the possible async operations |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/attachDisk/action | Attaches a data disk to a virtual machine. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/capture/action | Capture a virtual machine. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/delete | Removes virtual machines. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/detachDisk/action | Detaches a data disk from virtual machine. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/diagnosticsettings/read | Get virtual machine diagnostics settings. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/disks/read | Retrieves list of data disks |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/downloadRemoteDesktopConnectionFile/action | Downloads the RDP file for virtual machine. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/extensions/operationStatuses/read | Reads the operation status for the virtual machines extensions. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/extensions/read | Gets the virtual machine extension. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/extensions/write | Puts the virtual machine extension. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/metricdefinitions/read | Get the virtual machine metric definition. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/metrics/read | Ölçümleri alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/delete | Deletes the network security group associated with the network interface. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/operationStatuses/read | Reads the operation status for the virtual machines associated network security groups. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/read | Gets the network security group associated with the network interface. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/write | Adds a network security group associated with the network interface. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/operationStatuses/read | Reads the operation status for the virtual machines. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/performMaintenance/action | Performs maintenance on the virtual machine. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/read | Tanılama ayarlarını alın. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Ölçüm tanımlarını alır. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/read | Retrieves list of virtual machines. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/redeploy/action | Redeploys the virtual machine. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/restart/action | Restarts virtual machines. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/shutdown/action | Shutdown the virtual machine. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/start/action | Sanal makineyi başlatın. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/stop/action | Stops the virtual machine. |
> | Eylem | Microsoft.ClassicCompute/virtualMachines/write | Add or modify virtual machines. |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ClassicNetwork/expressroutecrossconnections/operationstatuses/read | Get an express route cross connection operation status. |
> | Eylem | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/delete | Express Route çapraz bağlantı eşlemesini silin. |
> | Eylem | Microsoft. ClassicNetwork/expressroutecrossconnections/peerler/operationdurumlarının/Read | Bir Express Route çapraz bağlantı eşleme işlem durumunu alır. |
> | Eylem | Microsoft. ClassicNetwork/expressroutecrossconnections/peerler/okundu | Hızlı rota çapraz bağlantı eşlemesini alın. |
> | Eylem | Microsoft. ClassicNetwork/expressroutecrossconnections/peerler/yaz | Express Route çapraz bağlantı eşlemesi ekleyin. |
> | Eylem | Microsoft. ClassicNetwork/expressroutecrossconnections/Read | Express Route çapraz bağlantılarını alın. |
> | Eylem | Microsoft. ClassicNetwork/expressroutecrossconnections/Write | Express Route çapraz bağlantıları ekleyin. |
> | Eylem | Microsoft. ClassicNetwork/gatewaySupportedDevices/Read | Desteklenen cihazların listesini alır. |
> | Eylem | Microsoft. ClassicNetwork/networkSecurityGroups/Delete | Ağ güvenlik grubunu siler. |
> | Eylem | Microsoft. ClassicNetwork/networkSecurityGroups/Operationdurumlarının/Read | Ağ güvenlik grubu için işlem durumunu okur. |
> | Eylem | Microsoft. ClassicNetwork/networksecuritygroups/Providers/Microsoft. Insights/diagnosticSettings/Read | Ağ güvenlik grupları tanılama ayarlarını alır |
> | Eylem | Microsoft. ClassicNetwork/networksecuritygroups/Providers/Microsoft. Insights/diagnosticSettings/Write | Ağ güvenlik grupları tanılama ayarlarını oluşturur veya güncelleştirir, bu işlem Öngörüler kaynak sağlayıcısı tarafından belirlenir. |
> | Eylem | Microsoft. ClassicNetwork/networksecuritygroups/Providers/Microsoft. Insights/logDefinitions/Read | Ağ güvenlik grubu olaylarını alır |
> | Eylem | Microsoft. ClassicNetwork/networkSecurityGroups/Read | Ağ güvenlik grubunu alır. |
> | Eylem | Microsoft. ClassicNetwork/networkSecurityGroups/securityRules/Delete | Güvenlik kuralını siler. |
> | Eylem | Microsoft. ClassicNetwork/networkSecurityGroups/securityRules/Operationdurumlarının/Read | Ağ güvenlik grubu güvenlik kuralları için işlem durumunu okur. |
> | Eylem | Microsoft. ClassicNetwork/networkSecurityGroups/securityRules/Read | Güvenlik kuralını alır. |
> | Eylem | Microsoft. ClassicNetwork/networkSecurityGroups/securityRules/Write | Bir güvenlik kuralı ekler veya güncelleştirir. |
> | Eylem | Microsoft. ClassicNetwork/networkSecurityGroups/Write | Yeni bir ağ güvenlik grubu ekler. |
> | Eylem | Microsoft. ClassicNetwork/Operations/Read | Klasik ağ işlemlerini alın. |
> | Eylem | Microsoft. ClassicNetwork/Quotas/Read | Aboneliğin kotasını alın. |
> | Eylem | Microsoft. ClassicNetwork/Register/Action | Klasik ağa Kaydet |
> | Eylem | Microsoft. ClassicNetwork/Rezervedips/Delete | Ayrılmış bir IP 'yi silin. |
> | Eylem | Microsoft. ClassicNetwork/Rezervedips/JOIN/Action | Ayrılmış bir IP 'ye katılır |
> | Eylem | Microsoft.ClassicNetwork/reservedIps/link/action | Ayrılmış bir IP 'yi bağlama |
> | Eylem | Microsoft. ClassicNetwork/Rezervedips/Operationdurumlarının/Read | Ayrılmış IP 'ler için işlem durumunu okur. |
> | Eylem | Microsoft.ClassicNetwork/reservedIps/read | Ayrılmış IP 'Leri alır |
> | Eylem | Microsoft. ClassicNetwork/Rezervedips/Write | Yeni bir ayrılmış IP ekleyin |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/abortMigration/Action | Bir sanal ağın geçişini durdurur |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Capabilities/Read | Özellikleri gösterir |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Checkıpaddressavailability/eylem | Bir sanal ağdaki belirli bir IP adresinin kullanılabilirliğini denetler. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/commitMigration/Action | Bir sanal ağın geçişini kaydeder |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Delete | Sanal ağı siler. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Clientiptal edilmiş sertifikalar/Sil | Bir istemci sertifikasını iptal eder. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Clientiptal edilmiş sertifikalar/okuma | İptal edilen istemci sertifikalarını okuyun. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Clientiptal edilmiş sertifikalar/Write | Bir istemci sertifikasını iptal eder. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Clienentrootcertificates/Delete | Sanal ağ geçidi istemci sertifikasını siler. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Clienentrootcertificates/Download/Action | Sertifikayı parmak izine göre indirir. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Clienentrootcertificates/listPackage/Action | Sanal ağ geçidi sertifika paketini listeler. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Clienentrootcertificates/Read | İstemci kök sertifikalarını bulun. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Clienentrootcertificates/Write | Yeni bir istemci kök sertifikasını karşıya yükler. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Connections/Connect/Action | Bir siteyi site ağ geçidi bağlantısına bağlar. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Connections/Disconnect/Action | Bir siteden siteye ağ geçidi bağlantısı bağlantısını keser. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Connections/Read | Bağlantıların listesini alır. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Connections/test/Action | Bir siteyi site ağ geçidi bağlantısına sınar. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Delete | Sanal ağ geçidini siler. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Downloaddeviceconfigurationscrıpt/Action | Cihaz yapılandırma betiğini indirir. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Downloadagnostiği/Action | Ağ Geçidi tanılamayı indirir. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Listdevresi ServiceKey/Action | Devre hizmet anahtarını alır. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/listPackage/Action | Sanal ağ geçidi paketini listeler. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Operationdurumlarının/Read | Sanal ağ geçitleri için işlem durumunu okur. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Packages/Read | Sanal ağ geçidi paketini alır. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Read | Sanal ağ geçitlerini alır. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/startDiagnostics/Action | Sanal ağ geçidi için tanılamayı başlatır. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/stopDiagnostics/Action | Sanal ağ geçidi için tanılamayı sonlandırır. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Write | Bir sanal ağ geçidi ekler. |
> | Eylem | Microsoft.ClassicNetwork/virtualNetworks/join/action | Sanal ağı birleştirir. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Operationdurumlarının/Read | Sanal ağların işlem durumunu okur. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/peer/Action | Bir sanal ağı başka bir sanal ağla eşler. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/prepareMigration/Action | Bir sanal ağın geçişini hazırlar |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Read | Sanal ağı alın. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Remotevirtualnetworkpeeringproxy/Delete | Uzak sanal ağ eşleme ara sunucusunu siler. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Remotevirtualnetworkpeeringproxy/Read | Uzak sanal ağ eşleme ara sunucusunu alır. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Remotevirtualnetworkpeeringproxy/Write | Uzak sanal ağ eşleme proxy 'sini ekler veya güncelleştirir. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/altağ/Ilişkili Networksecuritygroups/Delete | Alt ağla ilişkili ağ güvenlik grubunu siler. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/altağ/Ilişkili Networksecuritygroups/Operationdurumlar/Read | Sanal ağ alt ağı ile ilişkili ağ güvenlik grubu için işlem durumunu okur. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/alt ağlar/Ilişkili olan Networksecuritygroups/Read | Alt ağla ilişkili ağ güvenlik grubunu alır. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/altağ/Ilişkili Networksecuritygroups/Write | Alt ağla ilişkili bir ağ güvenlik grubu ekler. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/validateMigration/Action | Bir sanal ağın geçişini doğrular |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Virtualnetworkpeerler/okuma | Sanal Ağ eşlemesini alır. |
> | Eylem | Microsoft. ClassicNetwork/virtualNetworks/Write | Yeni bir sanal ağ ekleyin. |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. ClassicStorage/Capabilities/Read | Özellikleri gösterir |
> | Eylem | Microsoft. ClassicStorage/checkStorageAccountAvailability/eylem | Bir depolama hesabının kullanılabilirliğini denetler. |
> | Eylem | Microsoft. ClassicStorage/checkStorageAccountAvailability/okuma | Bir depolama hesabının kullanılabilirliğini alın. |
> | Eylem | Microsoft. ClassicStorage/diskleri/okuma | Depolama hesabı diskini döndürür. |
> | Eylem | Microsoft. ClassicStorage/Images/operationdurumlarının/Read | Görüntü Işlemi durumunu alır. |
> | Eylem | Microsoft. ClassicStorage/Images/Read | Görüntüyü döndürür. |
> | Eylem | Microsoft. ClassicStorage/işlemler/okuma | Klasik depolama işlemlerini alır |
> | Eylem | Microsoft. ClassicStorage/Osımages/Read | İşletim sistemi görüntüsünü döndürür. |
> | Eylem | Microsoft. ClassicStorage/Osplatformımages/Read | İşletim sistemi platformu görüntüsünü alır. |
> | Eylem | Microsoft. ClassicStorage/Publicımages/Read | Ortak sanal makine görüntüsünü alır. |
> | Eylem | Microsoft. ClassicStorage/kotaları/Read | Aboneliğin kotasını alın. |
> | Eylem | Microsoft. ClassicStorage/Register/Action | Klasik depolamaya kaydolun |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/abortMigration/Action | Bir depolama hesabının geçişini durdurur. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/blobServices/Providers/Microsoft. Insights/diagnosticSettings/Read | Tanılama ayarlarını alın. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/blobServices/Providers/Microsoft. Insights/diagnosticSettings/Write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/blobServices/Providers/Microsoft. Insights/metricDefinitions/Read | Ölçüm tanımlarını alır. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/commitMigration/Action | Bir depolama hesabının geçişini kaydeder. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Delete | Depolama hesabını silin. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Disks/Delete | Belirli bir depolama hesabı diskini siler. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Disks/Operationdurumlarının/Read | Reads the operation status for the resource. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/disks/read | Depolama hesabı diskini döndürür. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/disk/Write | Bir depolama hesabı diski ekler. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/fileServices/Providers/Microsoft. Insights/diagnosticSettings/Read | Tanılama ayarlarını alın. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/fileServices/Providers/Microsoft. Insights/diagnosticSettings/Write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/fileServices/Providers/Microsoft. Insights/metricDefinitions/Read | Ölçüm tanımlarını alır. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Images/Delete | Verilen bir depolama hesabı görüntüsünü siler. Kullanım dışı. ' Microsoft. ClassicStorage/storageAccounts/Vmımages ' kullanın) |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Images/operationdurumlarının/Read | Depolama hesabı görüntü işlemi durumunu döndürür. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/images/read | Depolama hesabı görüntüsünü döndürür. Kullanım dışı. ' Microsoft. ClassicStorage/storageAccounts/Vmımages ' kullanın) |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Operationdurumlarının/Read | Reads the operation status for the resource. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Osımages/Delete | Belirli bir depolama hesabı işletim sistemi görüntüsünü siler. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Osımages/Read | Depolama hesabı işletim sistemi görüntüsünü döndürür. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Osımages/Write | Belirli bir depolama hesabı işletim sistemi görüntüsünü ekler. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/prepareMigration/Action | Bir depolama hesabının geçişini hazırlar. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Providers/Microsoft. Insights/diagnosticSettings/Read | Tanılama ayarlarını alın. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Providers/Microsoft. Insights/diagnosticSettings/Write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Providers/Microsoft. Insights/metricDefinitions/Read | Ölçüm tanımlarını alır. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/queueServices/Providers/Microsoft. Insights/diagnosticSettings/Read | Tanılama ayarlarını alın. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/queueServices/Providers/Microsoft. Insights/diagnosticSettings/Write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/queueServices/Providers/Microsoft. Insights/metricDefinitions/Read | Ölçüm tanımlarını alır. |
> | Eylem | Microsoft.ClassicStorage/storageAccounts/read | Verilen hesaba sahip depolama hesabını döndürün. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/regenerateKey/Action | Depolama hesabı için mevcut erişim anahtarlarını yeniden oluşturur. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Services/diagnosticSettings/Read | Tanılama ayarlarını alın. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Services/diagnosticSettings/Write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Services/metricDefinitions/Read | Ölçüm tanımlarını alır. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Services/ölçümler/okuma | Ölçümleri alır. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Services/Read | Kullanılabilir hizmetleri alın. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/tableServices/Providers/Microsoft. Insights/diagnosticSettings/Read | Tanılama ayarlarını alın. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/tableServices/Providers/Microsoft. Insights/diagnosticSettings/Write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/tableServices/Providers/Microsoft. Insights/metricDefinitions/Read | Ölçüm tanımlarını alır. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/validateMigration/Action | Bir depolama hesabının geçişini doğrular. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Vmımages/Delete | Belirli bir sanal makine görüntüsünü siler. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Vmımages/operationdurumlarının/Read | Belirli bir sanal makine görüntüsü işlem durumunu alır. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Vmımages/Read | Sanal makine görüntüsünü döndürür. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Vmımages/Write | Belirli bir sanal makine görüntüsünü ekler. |
> | Eylem | Microsoft. ClassicStorage/storageAccounts/Write | Yeni bir depolama hesabı ekler. |
> | Eylem | Microsoft. ClassicStorage/Vmımages/Read | Sanal makine görüntülerini listeler. |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | DataAction | Microsoft. Biliveservices/accounts/Anoyıalgılayıcı/timeseries/tüm/algılama/eylem | Bu işlem bir serinin tamamını kullanarak bir model oluşturur, her bir nokta aynı modelle algılanır.<br>Bu yöntemde, belirli bir noktadan önceki ve sonraki noktaları, bir anomali olup olmadığını belirlemekte kullanılır.<br>Tüm algılama, kullanıcıya zaman serisinin genel durumunu verebilir. |
> | DataAction | Microsoft. Biliveservices/accounts/Anoyıalgılayıcı/timeseries/son/algılama/eylem | Bu işlem, en son bir noktadan önceki noktaları kullanarak bir model oluşturur. Bu yöntemde, hedef noktanın bir anomali olup olmadığını belirlemekte yalnızca geçmiş noktaları kullanılır. Algılama en son noktası, iş ölçümlerinin gerçek zamanlı izlemenin senaryosuyla eşleşir. |
> | DataAction | Microsoft. Biliveservices/hesaplar/otomatik öneri/arama/eylem | Bu işlem, belirli bir sorgu veya kısmi sorgu için öneriler sağlar. |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/çözümle/eylem | Bu işlem, görüntü içeriğine göre zengin bir görsel özellikler kümesini ayıklar.  |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/areaofınterest/Action | Bu işlem görüntünün en önemli alanının etrafında bir sınırlayıcı kutu döndürür. |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/açıkla/eylem | Bu işlem, bir görüntünün bir açıklamasını tüm cümleler olan okunabilir dilde oluşturur.<br> Açıklama, işlem tarafından da döndürülen bir içerik etiketleri koleksiyonunu temel alır.<br>Her görüntü için birden fazla açıklama oluşturulabilir.<br> Açıklamalar, güven puanlarına göre sıralanır.<br>Tüm açıklamalar Ingilizce 'Dir. |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/Algıla/eylem | Bu işlem, belirtilen görüntüde nesne algılamayı gerçekleştirir.  |
> | DataAction | Microsoft. Biliveservices/accounts/ComputerVision/generatethumbnail/Action | Bu işlem, Kullanıcı tarafından belirtilen genişlik ve yükseklik ile bir küçük resim oluşturur.<br> Hizmet, varsayılan olarak görüntüyü inceler, ilgi bölgesini (ROI) belirler ve ilgi bölgesine göre akıllı kırpma koordinatları oluşturur.<br> Akıllı kırpma, giriş görüntüsünden farklı bir boyut oranı belirttiğinizde yardımcı olur |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/modeller/çözümle/eylem | Bu işlem, bir görüntü içindeki içeriği, etki alanına özgü bir model uygulayarak tanır.<br> Görüntü İşleme API'si tarafından desteklenen alana özgü modellerin listesi/modeller GET isteği kullanılarak alınabilir.<br> Şu anda, API aşağıdaki alana özgü modeller sağlar: Ünlüler, yer işaretleri. |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/modeller/okuma | Bu işlem, Görüntü İşleme API'si tarafından desteklenen alana özgü modellerin listesini döndürür.  Şu anda API, etki alanına özgü şu modelleri destekliyor: ünlül tanıyıcı, yer işareti tanıyıcı. |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/OCR/eylem | Optik karakter tanıma (OCR), görüntüdeki metni algılar ve tanınan karakterleri makine tarafından kullanılabilen bir karakter akışına ayıklar.    |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/Read/Analyze/Action | Metin ağırlıklı belgeler için optimize edilmiş, son derece optik karakter tanıma (OCR) algoritmalarını kullanan bir okuma işlemi gerçekleştirmek için bu arabirimi kullanın.<br>El ile yazılmış, yazdırılmış veya karışık belgeleri işleyebilir.<br>Okuma arabirimini kullandığınızda, yanıt ' Operation-Location ' adlı bir üst bilgi içerir.<br>' Işlem-konumu ' üst bilgisi, OCR sonuçlarına erişmek için okuma sonucu al işlemi için kullanmanız gereken URL 'YI içerir. |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/Read/analiz zeresults/Read | Bir okuma işleminin durumunu ve OCR sonucunu almak için bu arabirimi kullanın.  ' OperationId ' öğesini içeren URL, okuma işlemi ' Işlem-konumu ' yanıt üst bilgisinde döndürülür. |
> | DataAction | Microsoft. Biliveservices/accounts/ComputerVision/Read/Core/asyncbatchanalyze/Action | Son derece optik karakteri kullanan toplu bir dosya okuma işleminin sonucunu almak için bu arabirimi kullanın |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/Read/Operations/Read | Bu arabirim, okuma işleminin OCR sonuçlarını almak için kullanılır. Bu arabirimin URL 'SI toplu Işlem okuma dosyası arabiriminden döndürülen <b>"işlem-konum"</b> alanından alınmalıdır. |
> | DataAction | Microsoft. Biliveservices/accounts/ComputerVision/recognizetext/Action | Metin Tanıma işleminin sonucunu almak için bu arabirimi kullanın. Metin Tanıma arabirimini kullandığınızda, yanıt "Işlem-konum" adlı bir alan içerir. "Işlem-konum" alanı, Get Metin Tanıma Işlem sonucu işlemi için kullanmanız gereken URL 'YI içerir. |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/Tag/Action | Bu işlem, sağlanan görüntünün içeriğiyle ilgili olan sözcüklerin veya etiketlerin bir listesini oluşturur.<br>Görüntü İşleme API'si, resimlerde nesneler, canlı kullanımlar, manzara veya eylemler temelinde bulunan Etiketler döndürebilir.<br>Kategorilerin aksine, etiketler hiyerarşik bir sınıflandırma sistemine göre düzenlenmemiştir, ancak resim içeriğine karşılık gelir.<br>Etiketler belirsizliği önlemek veya bağlam sağlamak için ipuçları içerebilir, örneğin "cello" etiketine "müzik gereci" ipucu gelebilir.<br>Tüm Etiketler Ingilizce 'Dir. |
> | DataAction | Microsoft. Biliveservices/accounts/ComputerVision/textoperations/Read | Bu arabirim, metin işlemi sonucunu tanımak için kullanılır. Bu arabirimin URL 'SI, Metin Tanıma arabiriminden döndürülen <b>"işlem-konum"</b> alanından alınmalıdır. |
> | DataAction | Microsoft. Biliveservices/accounts/Contentmoderatör/imagelists/Action | Görüntü listesi oluştur. |
> | DataAction | Microsoft. Biliveservices/accounts/Contentmoderatör/imagelists/Delete | Görüntü listeleri-silme |
> | DataAction | Microsoft. Biliveservices/accounts/Contentmoderatör/imagelists/Images/Delete | Görüntü listenizden bir görüntüyü silin. Görüntü listesi, görüntü/eşleştirme API 'SI kullanılırken diğer görüntülerle benzer eşleştirme yapmak için kullanılabilir. Listenizden tüm görüntüleri silin. Görüntü listesi, görüntü/eşleştirme API 'SI kullanılırken diğer görüntülerle benzer eşleştirme yapmak için kullanılabilir. * |
> | DataAction | Microsoft. Biliveservices/accounts/Contentmoderatör/imagelists/Images/Read | Görüntü-tüm görüntü kimliklerini al |
> | DataAction | Microsoft. Biliveservices/accounts/Contentmoderatör/imagelists/Images/Write | Görüntü listenize bir resim ekleyin. Görüntü listesi, görüntü/eşleştirme API 'SI kullanılırken diğer görüntülerle benzer eşleştirme yapmak için kullanılabilir. |
> | DataAction | Microsoft. Biliveservices/accounts/Contentmoderatör/imagelists/Read | Görüntü listeleri-Get ayrıntıları-görüntü listeleri-tümünü al |
> | DataAction | Microsoft. Biliveservices/accounts/Contentmoderatör/imagelists/refreshındex/Action | Görüntü listeleri-arama dizinini Yenile |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/imagelists/write | Image Lists - Update Details |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/processimage/evaluate/action | Returns probabilities of the image containing racy or adult content. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/processimage/findfaces/action | Find faces in images. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/processimage/match/action | Fuzzily match an image against one of your custom Image Lists.<br>You can create and manage your custom image lists using this API.<br> |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/processimage/ocr/action | Returns any text found in the image for the language specified. If no language is specified in input then the detection defaults to English. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/processtext/detectlanguage/action | This operation will detect the language of given input content.<br>Returns the ISO 639-3 code for the predominant language comprising the submitted text.<br>Over 110 languages supported. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/processtext/screen/action | İşlem, 100’den fazla dilde küfür algılar ve özel ve paylaşılan kara listeyle eşleştirir. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/jobs/action | A job Id will be returned for the Image content posted on this endpoint.  |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/jobs/read | Get the Job Details for a Job Id. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/reviews/accesskey/read | Get the review content access key for your team. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/reviews/action | The reviews created would show up for Reviewers on your team. As Reviewers complete reviewing, results of the Review would be POSTED (i.e. HTTP POST) on the specified CallBackEndpoint. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/reviews/frames/read | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/reviews/frames/write | Use this method to add frames for a video review. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/reviews/publish/action | Video reviews are initially created in an unpublished state - which means it is not available for reviewers on your team to review yet. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/reviews/read | Returns review details for the review Id passed. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/reviews/transcript/action | This API adds a transcript file (text version of all the words spoken in a video) to a video review. The file should be a valid WebVTT format. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/reviews/transcriptmoderationresult/action | This API adds a transcript screen text result file for a video review. Transcript screen text result file is a result of Screen Text API . In order to generate transcript screen text result file , a transcript file has to be screened for profanity using Screen Text API. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/settings/templates/delete | Delete a template in your team |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/settings/templates/read | Returns an array of review templates provisioned on this team. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/settings/templates/write | Creates or updates the specified template |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/workflows/read | Get the details of a specific Workflow on your Team. Get all the Workflows available for you Team* |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/teams/workflows/write | Create a new workflow or update an existing one. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/action | Create term list. |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/bulkupdate/action | Term Lists - Bulk Update |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/delete | Term Lists - Delete |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/read | Term Lists - Get All - Term Lists - Get Details |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/refreshindex/action | Term Lists - Refresh Search Index |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/terms/delete | Term - Delete - Term - Delete All Terms |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/terms/read | Term - Get All Terms |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/terms/write | Term - Add Term |
> | DataAction | Microsoft.CognitiveServices/accounts/ContentModerator/termlists/write | Term Lists - Update Details |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/classify/iterations/image/action | Classify an image and saves the result. |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmin/sınıflandırma/yineleme/görüntü/NoStore/eylem | Sonucu kaydetmeden bir görüntüyü sınıflandırın. |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmin/sınıflandırma/yinelemeler/URL/eylem | Bir resim URL 'si sınıflandırın ve sonucu kaydeder. |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmin/sınıflandırma/yinelemeler/URL/NoStore/Action | Sonucu kaydetmeden bir resim URL 'si sınıflandırın. |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmin/algılama/yineleme/görüntü/eylem | Görüntüdeki nesneleri algılayın ve sonucu kaydeder. |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmin/algılama/yineleme/görüntü/NoStore/eylem | Sonuçları kaydetmeden görüntüdeki nesneleri algılayın. |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmin/algılama/yineleme/URL/eylem | Bir görüntü URL 'sindeki nesneleri algılayın ve sonucu kaydeder. |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmin/algılama/yineleme/URL/NoStore/Action | Sonuçları kaydetmeden resim URL 'sindeki nesneleri algıla. |
> | DataAction | Microsoft. Biliveservices/hesaplar/CustomVision. tahmin/etki alanları/okuma | Belirli bir etki alanı hakkında bilgi alın. Kullanılabilir etki alanlarının bir listesini alın. * |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmin/labeltekliflerini/ayarlama/eylem | Etiket teklifinin havuz boyutunu ayarlayın. |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmine/labeltekliflerini/ayarlamayı/okumayı | Bu proje için etiket teklifinin havuz boyutunu alın. |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmin/proje/geçiş/eylem | *NotDefined* |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmine/projelere/eyleme | Bir proje oluşturun. |
> | DataAction | Microsoft. Biliveservices/hesaplar/CustomVision. tahmin/projeler/silme | Belirli bir projeyi silin. |
> | DataAction | Microsoft. Biliveservices/hesaplar/CustomVision. tahmin/projeler/görüntüler/eylem | Bu API, gövde içeriğini multipart/form-Data ve Application/sekizli Stream olarak kabul eder. Çok parçalı kullanırken |
> | DataAction | Microsoft. Biliveservices/hesaplar/CustomVision. tahmin/projeler/resimler/silme | Eğitim görüntüleri kümesinden görüntüleri silin. |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmin/projeler/görüntüler/dosyalar/eylem | Bu API, görüntü oluşturmak için bir toplu dosya ve isteğe bağlı olarak Etiketler kabul eder. 64 görüntülük ve 20 etiketlik bir sınır bulunur. |
> | DataAction | Microsoft. Biliveservices/hesaplar/CustomVision. tahmin/projeler/görüntüler/kimlik/okuma | Bu API, belirtilen Etiketler ve isteğe bağlı yineleme için bir görüntü kümesi döndürür. Hiçbir yineleme belirtilmemişse, |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmine/projelere/görüntülere/tahminlere/eyleme | Bu API, belirtilen tahmini görüntülerden bir toplu görüntü oluşturur. 64 görüntülük ve 20 etiketlik bir sınır bulunur. |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmine/projelere/görüntülere/regiontekliflerini/eyleme | Bu API, bir görüntü için bölge önerilerini alır ve bu da bölge için bir yapılandırma ile birlikte yer alır. Hiçbir teklif bulunamazsa boş bir dizi döndürür. |
> | DataAction | Microsoft. Biliveservices/hesaplar/CustomVision. tahmin/projeler/görüntüler/bölgeler/eylem | Bu API, mevcut görüntüleri bölge bilgileriyle güncelleştirmek için bir dizi görüntü bölgesini ve isteğe bağlı olarak etiketleri kabul eder. |
> | DataAction | Microsoft. Biliveservices/hesaplar/CustomVision. tahmin/projeler/görüntüler/bölgeler/Sil | Bir görüntü bölgesi kümesini silin. |
> | DataAction | Microsoft. Biliveservices/hesaplar/CustomVision. tahmin/projeler/görüntüler/önerilen/eylem | Bu API, önerilen Etiketler kimliklerine göre filtrelenmiş etiketlenmemiş görüntüleri getirir. Görüntü bulunamazsa boş bir dizi döndürür. |
> | DataAction | Microsoft. Biliveservices/hesaplar/CustomVision. tahmin/projeler/görüntüler/önerilen/sayım/eylem | Bu API, belirli bir eşik için önerilen Etiketler başına etiketlenmemiş görüntü sayısını almak için TagId 'leri alır. |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmin/projeler/görüntüler/etiketli/Count/Read | The filtering is on an and/or relationship. For example, if the provided tag ids are for the "Dog" and |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmine/projelere/görüntülere/etiketlendi/Read | This API supports batching and range selection. By default it will only return first 50 images matching images. |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmine/projelere/görüntülere/etiketlere/eyleme | Bir resim kümesini bir Etiketler kümesiyle ilişkilendirin. |
> | DataAction | Microsoft. Biliveservices/hesaplar/CustomVision. tahmin/projeler/görüntüler/Etiketler/Sil | Bir resim kümesinden etiket kümesini kaldırın. |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmin/projeler/görüntüler/etiketsiz/Count/Read | Bu API, belirli bir proje için etiketi olmayan görüntüleri ve isteğe bağlı olarak bir yinelemeyi döndürür. Hiçbir yineleme belirtilmemişse, |
> | DataAction | Microsoft. Biliveservices/hesaplar/CustomVision. tahmin/projeler/görüntüler/etiketsiz/Read | This API supports batching and range selection. By default it will only return first 50 images matching images. |
> | DataAction | Microsoft. Biliveservices/hesaplar/CustomVision. tahmin/projeler/görüntüler/URL/eylem | This API accepts a batch of urls, and optionally tags, to create images. 64 görüntülük ve 20 etiketlik bir sınır bulunur. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/delete | Delete a specific iteration of a project. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/export/action | Export a trained iteration. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/export/read | Get the list of exports for a specific iteration. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/performance/images/count/read | The filtering is on an and/or relationship. For example, if the provided tag ids are for the "Dog" and |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/performance/images/read | This API supports batching and range selection. By default it will only return first 50 images matching images. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/performance/read | Get detailed performance information about an iteration. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/publish/action | Publish a specific iteration. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/publish/delete | Unpublish a specific iteration. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/read | Get a specific iteration. Get iterations for the project.* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/iterations/write | Update a specific iteration. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/predictions/delete | Delete a set of predicted images and their associated prediction results. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/predictions/query/action | Get images that were sent to your prediction endpoint. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/quicktest/image/action | Quick test an image. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/quicktest/url/action | Quick test an image url. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/read | Get a specific project. Get your projects.* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/tags/action | Create a tag for the project. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/tags/delete | Delete a tag from the project. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/tags/read | Get information about a specific tag. Get the tags for a given project and iteration.* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/tags/write | Update a tag. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/tagsandregions/suggestions/action | This API will get suggested tags and regions for an array/batch of untagged images along with confidences for the tags. It returns an empty array if no tags are found. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/train/action | Queues project for training. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/projects/write | Update a specific project. |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/quota/action | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/quota/delete | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/quota/refresh/write | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/usage/prediction/user/read | Get usage for prediction resource for Oxford user |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/usage/training/resource/tier/read | Get usage for training resource for Azure user |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/usage/training/user/read | Get usage for training resource for Oxford user |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/user/action | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/user/delete | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/user/state/write | Update user state |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/user/tier/write | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/users/read | *NotDefined* |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/whitelist/delete | Deletes a whitelisted user with specific capability |
> | DataAction | Microsoft.CognitiveServices/accounts/CustomVision.Prediction/whitelist/read | Belirli bir özelliği olan, daha önce listelenmiş kullanıcıların listesini alır |
> | DataAction | Microsoft. Biliveservices/accounts/CustomVision. tahmin/Whitelist/Write | Beyaz listede belirli bir özelliği olan bir kullanıcı güncelleştirir veya oluşturur |
> | Eylem | Microsoft. Biliveservices/hesaplar/Sil | API hesaplarını siler |
> | DataAction | Microsoft. Bilivehizmetleri/hesapları/EntitySearch/arama/eylem | Varlıkları alın ve belirli bir sorgu için sonuçları yerleştir. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/algılama/eylem | Bir görüntüdeki insan yüzlerini algılayın, yüz dikdörtgenlerini ve isteğe bağlı olarak Faceid, yer işaretleri ve öznitelikleri yapın. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/çok yönlü listeler/Sil | Belirtilen yüz listesini silin. Yüz listesindeki ilgili yüz görüntüleri de silinecek. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/çok yönlü listeler/persistedfaces/Sil | Belirtilen Facelid ve Persisitedçok yönlü kimliği ile bir yüz listesinden yüz silme. İlgili yüz görüntüsü de silinecek. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/çok yönlü listeler/persistedfaces/Write | Belirli bir yüz listesine, en fazla 1.000 yüze kadar bir yüz ekleyin. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/çok yönlü listeler/okundu | Yüz listesinin bir yüz listesinin Facelitıd, ad, userData ve yüzlerini alın. Yüz listeleri listesi ' Facelitıd, ad ve userData. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/çok yönlü listeler/yazma | Kullanıcı tarafından belirtilen Facelitıd, ad ve isteğe bağlı bir userData ile boş bir yüz listesi oluşturun. Ad ve userData gibi yüz listesinin güncelleştirme bilgilerine kadar en fazla 64 yüz listesi izin verilir. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/findsimilars/eylem | Çok yönlü bir diziden, yüz listesinden veya büyük bir yüz listesinden benzer görünümlü yüzeyleri aramak için bir sorgu yüzünün çok yönlü kimliği. FaceID |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/Grup/eylem | Aday yüzleri bir yüz benzerliğine göre gruplara bölün. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/tanımla/eylem | bir kişi grubundan veya büyük kişi grubundan belirli bir sorgu kişisinin en yakın eşleşmelerini bulmak için 1-çok tanımlama. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largeçok yönlü listeler/Sil | Belirtilen büyük bir yüz listesini silin. Büyük yüz listesindeki ilgili yüz görüntüleri de silinecek. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largeçok yönlü listeler/persistedfaces/Sil | Belirtilen Largefacelitıd ve Persisitedceıd kimliğine göre büyük bir yüz listesinden yüz silme. İlgili yüz görüntüsü de silinecek. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largeçok yönlü listeler/persistedfaces/okuma | Largefacelitıd ve persistedFaceId tarafından büyük yüz listesindeki kalıcı yüz listesini alın. Belirtilen büyük bir yüz listesinde yüzeyleri ' persistedFaceId ve userData listeleyin. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largeçok yönlü listeler/persistedfaces/Write | Belirli bir büyük yüz listesine en fazla 1.000.000 yüz ekleyerek bir yüz ekleyin. Büyük bir yüz listesindeki belirtilen yüzün userData alanını persistedFaceId göre güncelleştirin. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largeçok yönlü listeler/okundu | Büyük bir yüz listesinin Largefacelitıd, Name, userData alma. Büyük yüz listeleri, Largefacelitıd, Name ve userData bilgilerini listeleyin. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largeçok yönlü listeler/eğitme/eylem | Büyük bir yüz listesi eğitim görevi gönderme. Eğitim, yalnızca eğitilen büyük bir yüz listesinin kullanabileceği önemli bir adımdır. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largeçok yönlü listeler/eğitim/okuma | Büyük yüz listesi eğitim durumunu tamamlandı veya hala devam eden denetlemek için. Largecelist eğitimi zaman uyumsuz bir işlemdir |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largeçok yönlü listeler/yaz | Kullanıcı tarafından belirtilen Largefacelitıd, adı ve isteğe bağlı bir userData ile boş bir büyük yüz listesi oluşturun. Ad ve userData dahil olmak üzere büyük bir yüz listesinin güncelleştirme bilgileri. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largepersongroups/Delete | Belirtilen Persongroupıd ile mevcut bir büyük kişi grubunu silin. Bu büyük kişi grubundaki kalıcı veriler silinecek. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largepersongroups/kişiler/eylem | Belirtilen büyük kişi grubunda yeni bir kişi oluşturun. Bu kişiye yüz eklemek için lütfen çağırın |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largepersongroups/kişiler/Sil | Büyük bir kişi grubundan mevcut bir kişiyi silin. Kişi girişinde tüm depolanan kişi verileri ve yüz görüntüleri silinir. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largepersongroups/kişiler/persistedfaces/Delete | Büyük kişi grubundaki bir kişiden yüz silme. Bu yüz girişiyle ilgili yüz verileri ve görüntü de silinir. |
> | DataAction | Microsoft. Bilivehizmetleri/hesapları/yüz/largepersongroups/kişiler/persistedfaces/Read | Kişi yüz bilgilerini alın. Kalıcı kişi, Largepersongroupıd, PersonID ve persistedFaceId tarafından belirtilir. |
> | DataAction | Microsoft. Bilivehizmetleri/hesapları/yüz/largepersongroups/kişiler/persistedfaces/Write | Yüz tanıma veya doğrulama için bir kişiye yüz görüntüsünü büyük bir kişi grubuna ekleyin. Bir kişinin kalıcı yüz Kullanıcı adını güncelleştirme görüntüsü ile uğraşmak için. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largepersongroups/kişiler/okuma | Kişinin adını ve userData ve kayıtlı kişinin yüz görüntüsünü temsil eden kalıcı Factıd 'leri alın. Tüm kişilerin bilgilerini belirtilen büyük kişi grubunda PersonID, Name, userData ve persistedFaceIds dahil olmak üzere listeleyin. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largepersongroups/kişiler/yaz | Bir kişinin adını veya Kullanıcı adını güncelleştirin. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largepersongroups/Read | Adı ve userData dahil olmak üzere büyük bir kişi grubunun bilgilerini alın. Bu API, tüm mevcut büyük kişi gruplarının Largepeşinatlı, Name ve userData olan büyük kişi grubu bilgilerini döndürür. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largepersongroups/tren/eylem | Büyük bir kişi grubu eğitim görevi gönderme. Eğitim, yalnızca eğitilen büyük kişi grubunun kullanabileceği önemli bir adımdır. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largepersongroups/eğitim/okuma | Büyük kişi grubu eğitim durumunu tamamlandı veya hala devam eden denetlemek için. LargePersonGroup eğitimi zaman uyumsuz bir işlemdir |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/largepersongroups/Write | Kullanıcı tarafından belirtilen Largepersongroupıd, Name ve optional userData ile yeni bir büyük kişi grubu oluşturun. Mevcut bir büyük kişi grubunun adını ve userData öğesini güncelleştirin. Özellikler istek gövdesinde değilse değişmeden kalır. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/kişilik grupları/silme | Belirtilen Persongroupıd ile mevcut bir kişi grubunu silin. Bu kişi grubundaki kalıcı veriler silinecek. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/kişi/Grup/eylem | Belirtilen kişi grubunda yeni bir kişi oluşturun. Bu kişiye yüz eklemek için lütfen çağırın |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/kişi/kişi/silme | Mevcut bir kişiyi bir kişi grubundan silin. Kişi girişinde tüm depolanan kişi verileri ve yüz görüntüleri silinir. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/kişiler/kişiler/persistedfaces/Sil | Kişi grubundaki bir kişiden yüz silme. Bu yüz girişiyle ilgili yüz verileri ve görüntü de silinir. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/persongroups/kişiler/persistedfaces/Read | Kişi yüz bilgilerini alın. Kalıcı kişi, kişisgroupıd, PersonID ve persistedFaceId tarafından belirtilir. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/kişiler/kişiler/persistedfaces/Write | Yüz tanıma veya doğrulama için kişi grubuna bir yüz görüntüsü ekleyin. Birden çok güncelleştirme görüntüsü ile uğraşmak için kişi tarafından kalıcı bir yüz Kullanıcı bilgisi alanı. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/kişi/kişi/okuma | Kişinin adını ve userData ve kayıtlı kişinin yüz görüntüsünü temsil eden kalıcı Factıd 'leri alın. Belirtilen kişi grubundaki tüm kişilerin bilgilerini, tescilli ID, ad, userData ve persistedFaceIds dahil olmak üzere listeleyin. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/kişi/kişi/yazma | Bir kişinin adını veya Kullanıcı adını güncelleştirin. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/kişi grupları/okuma | Kişi grubu adı ve userData alma. Bu kişiye ait kişi bilgilerini almak için kişi gruplarının peşinatlı, adının ve userData listesini kullanın. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/kişilik grupları/eğitme/eylem | Bir kişi grubu eğitim görevi gönder. Eğitim, yalnızca eğitilen kişi grubunun kullanabileceği önemli bir adımdır. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/kişilik grupları/eğitim/okuma | Kişi grubu eğitim durumunu tamamlandı veya hala devam ediyor olarak denetlemek için. PersonGroup eğitimi, tetiklenen zaman uyumsuz bir işlemdir |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/kişi grupları/yazma | Belirtilen Persongroupıd, Name ve Kullanıcı tarafından belirtilen userData ile yeni bir kişi grubu oluşturun. Mevcut bir kişi grubunun adını ve userData öğesini güncelleştirin. Özellikler istek gövdesinde değilse değişmeden kalır. |
> | DataAction | Microsoft. Biliveservices/hesaplar/yüz/doğrulama/eylem | İki yüzün aynı kişiye ait olup olmadığını veya bir yüzün bir kişiye ait olup olmadığını doğrulayın. |
> | DataAction | Microsoft. Biliveservices/accounts/Formtanıyıcı/özel/modeller/çözümle/eylem | Belirli bir belgeden anahtar-değer çiftlerini ayıklayın. Giriş belgesi desteklenen içerik türlerinden biri olmalıdır-' Application/PDF ', ' image/jpeg ' veya ' image/png '. JSON içinde başarılı bir yanıt döndürülür. |
> | DataAction | Microsoft. Biliveservices/accounts/Formtanıyıcı/özel/modeller/silme | Model yapıtları silin. |
> | DataAction | Microsoft. Biliveservices/accounts/Formtanıyıcı/özel/modeller/anahtarlar/okuma | Modelin anahtarlarını alın. |
> | DataAction | Microsoft. Biliveservices/accounts/Formtanıyıcı/özel/modeller/okuma | Bir model hakkında bilgi alın. Eğitilen tüm özel modeller hakkında bilgi alın * |
> | DataAction | Microsoft. Biliveservices/accounts/Formtanıyıcı/özel/tren/eylem | Özel bir model oluşturun ve eğitme.<br>Eğitme isteği, dışarıdan erişilebilen bir Azure Depolama Blobu kapsayıcı URI (tercihen paylaşılan erişim Imzası URI 'si) veya yerel olarak bağlı bir sürücüdeki bir veri klasörü için geçerli yol olan bir kaynak parametre içermelidir.<br>Yerel yollar belirtildiğinde, Linux/UNIX yol biçimini izlemelidir ve giriş bağlama yapılandırması 'nın kökü olan mutlak bir yol olması gerekir |
> | DataAction | Microsoft. Biliveservices/accounts/Formtanıyıcı/önceden oluşturulan/makbuz/asyncbatchanalyze/Action | Verilen bir makbuz belgesinden alan metnini ve anlam değerlerini ayıklayın. Giriş resmi belgesi, desteklenen içerik türlerinden biri olmalıdır-JPEG, PNG, BMP, PDF veya TIFF. Başarı yanıtı, sonuçları zaman uyumsuz olarak almak için alma sonucunu al işleminin URL 'sini içeren ' Operation-Location ' adlı bir alan içeren bir JSON ' dır. |
> | DataAction | Microsoft. Biliveservices/accounts/Formtanıyıcı/önceden oluşturulan/makbuz/işlemler/eylem | Durumu sorgulayın ve bir analiz alma işleminin sonucunu alın. Bu arabirimin URL 'SI, ' Işlem-konumu ' başlığından, analiz alma yanıtında elde edilebilir. |
> | DataAction | Microsoft. Bilivehizmetleri/hesapları/ımagesearch/Ayrıntılar/eylem | Görüntüyü içeren Web sayfaları gibi bir görüntüyle ilgili öngörüleri döndürür. |
> | DataAction | Microsoft. Biliveservices/accounts/ımagesearch/Search/Action | Belirli bir sorgu için ilgili görüntüleri alın. |
> | DataAction | Microsoft. Biliveservices/accounts/ımagesearch/trbitiriliyor/Action | Şu anda popüler resimleri al. |
> | DataAction | Microsoft. Biliveservices/accounts/ImmersiveReader/getcontentmodelforreader/eylem | Bir tam ekran okuyucusu oturumu oluşturur |
> | DataAction | Microsoft. Biliveservices/accounts/ınktanıyıcı/Recognize/eylem | Bir dizi vuruş verisi, içeriği analiz eder ve tanınan metin dahil tanınan varlıkların bir listesini oluşturur. |
> | Eylem | Microsoft. Biliveservices/accounts/listKeys/Action | Anahtarları Listele |
> | DataAction | Microsoft. Biliveservices/accounts/Lua/tahmin/eylem | Verilen sorgu için yayımlanmış uç nokta tahminini alır. |
> | DataAction | Microsoft. Biliveservices/hesaplar/haber arama/categorysearch/Action | Bir belirtilen kategori için haberleri döndürür. |
> | DataAction | Microsoft. Biliveservices/hesaplar/haber arama/arama/eylem | Belirli bir sorgu için ilgili haber makalelerini alın. |
> | DataAction | Microsoft. Biliveservices/hesaplar/NewsSearch/trendingkonular/eylem | Bing tarafından tanımlanan popüler konuları alın. Bunlar, Bing giriş sayfasının alt kısmındaki başlık bölümünde gösterilen konulardır. |
> | Eylem | Microsoft. Biliveservices/hesaplar/okuma | API hesaplarını okur. |
> | Eylem | Microsoft. Biliveservices/accounts/regenerateKey/Action | Anahtarı yeniden oluştur |
> | Eylem | Microsoft. Biliveservices/hesaplar/SKU 'lar/okuma | Mevcut bir kaynak için kullanılabilir SKU 'Ları okur. |
> | DataAction | Microsoft. Biliveservices/accounts/SpellCheck/SpellCheck/Action | GET veya POST aracılığıyla bir yazım denetimi sorgusunun sonucunu elde edin. |
> | DataAction | Microsoft. Biliveservices/accounts/TextAnalytics/Entities/Action | API, belirli bir belgedeki bilinen varlıkların ve genel adlandırılmış varlıkların (\"kişi\", \"konum\", \"kuruluş\" vb.) listesini döndürür. |
> | DataAction | Microsoft. Biliveservices/accounts/TextAnalytics/keyphrases/Action | API, giriş metnindeki başlıca konuşma noktalarını gösteren bir dize listesi döndürür. |
> | DataAction | Microsoft. Biliveservices/accounts/TextAnalytics/Languages/eylem | API, algılanan dile ek olarak 0 ile 1 arasında bir sayısal puan döndürür. Puanın 1’e yakın olması, tanımlanan dilin %100 olasılıkla doğru olduğunu gösterir. Toplamda 120 dil desteklenir. |
> | DataAction | Microsoft. Biliveservices/hesaplar/TextAnalytics/yaklaşım/eylem | API, 0 ile 1 arasında bir sayısal puan döndürür.<br>1’e yakın puanlar olumlu yaklaşımı, 0’a yakın puanlar ise olumsuz yaklaşımı gösterir.<br>0,5 puanı, yaklaşım eksikliğinden (ör.<br>bir FACTOID Bildirisi). |
> | Eylem | Microsoft. Biliveservices/hesaplar/kullanımlar/okuma | Mevcut bir kaynak için kota kullanımını alın. |
> | DataAction | Microsoft. Biliveservices/hesaplar/VideoSearch/Ayrıntılar/eylem | İlgili videolar gibi bir video hakkında Öngörüler edinin. |
> | DataAction | Microsoft. Biliveservices/accounts/VideoSearch/Search/ACTION | Belirli bir sorguyla ilgili videoları alın. |
> | DataAction | Microsoft. Biliveservices/hesaplar/VideoSearch/trbitiriliyor/eylem | Şu anda popüler videoları alın. |
> | DataAction | Microsoft. Biliveservices/accounts/VisualSearch/Search/ACTION | Belirtilen görüntüyle ilgili etiketlerin listesini döndürür |
> | DataAction | Microsoft. Bilivehizmetleri/hesapları/Web araması/arama/eylem | Belirli bir sorgu için Web, resim, haber & Videolar sonuçları alın. |
> | Eylem | Microsoft. Biliveservices/hesaplar/yazma | API hesaplarını yazar. |
> | Eylem | Microsoft. Biliveservices/Checkdomainavaılabılıty/Action | Bir abonelik için kullanılabilir SKU 'Ları okur. |
> | Eylem | Microsoft. Biliveservices/Locations/Checkskuavaılabılıty/Action | Bir abonelik için kullanılabilir SKU 'Ları okur. |
> | Eylem | Microsoft. Biliveservices/Locations/Checkskuavaılabılıty/Action | Bir abonelik için kullanılabilir SKU 'Ları okur. |
> | Eylem | Microsoft. Biliveservices/Locations/Deletevirtualnetworkoralt ağları/eylemi | VirtualNetworks veya alt ağları silmenin Microsoft. Network 'e yönelik bildirim. |
> | Eylem | Microsoft. Bilivehizmetleri/konumları/operationresults/Read | Zaman uyumsuz bir işlemin durumunu okuyun. |
> | Eylem | Microsoft. Biliveservices/Işlemler/okuma | Tüm kullanılabilir işlemleri Listele |
> | Eylem | Microsoft. Biliveservices/Register/Action | Bilişsel hizmetler için aboneliği kaydeder |
> | Eylem | Microsoft. Biliveservices/Register/Action | Bilişsel hizmetler için aboneliği kaydeder |

## <a name="microsoftcommerce"></a>Microsoft. Commerce

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. Commerce/RateCard/okuma | Verilen abonelik için teklif verileri, kaynak/ölçüm meta verileri ve ücretler döndürür. |
> | Eylem | Microsoft. Commerce/Register/ACTION | Microsoft Commerce UsageAggregate için abonelik kaydetme |
> | Eylem | Microsoft. Commerce/Unregister/eylem | Microsoft Commerce Usagetoplamasını abonelik kaydını sil |
> | Eylem | Microsoft. Commerce/Usagetoplamalar/okuma | Microsoft Azure bir aboneliğe göre tüketimini alır. Sonuç, belirli bir zaman aralığında kullanım verilerini, aboneliği ve kaynakla ilgili bilgileri toplar. |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. COMPUTE/kullanılabilirliği Bilitysets/Delete | Kullanılabilirlik kümesini siler |
> | Eylem | Microsoft. COMPUTE/kullanılabilirliği Bilitysets/Read | Bir kullanılabilirlik kümesinin özelliklerini al |
> | Eylem | Microsoft. COMPUTE/kullanılabilirliği Bilitysets/vmSizes/Read | Kullanılabilirlik kümesinde bir sanal makine oluşturmak veya güncelleştirmek için kullanılabilir boyutları listeleyin |
> | Eylem | Microsoft. COMPUTE/kullanılabilirliği Bilitysets/Write | Yeni bir kullanılabilirlik kümesi oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft. COMPUTE/diskEncryptionSets/Delete | Disk şifreleme kümesini silme |
> | Eylem | Microsoft. COMPUTE/diskEncryptionSets/Read | Disk şifreleme kümesinin özelliklerini al |
> | Eylem | Microsoft. COMPUTE/diskEncryptionSets/Write | Yeni bir disk şifrelemesi kümesi oluşturma veya var olanı güncelleştirme |
> | Eylem | Microsoft. COMPUTE/Disks/beginGetAccess/Action | Blob erişimi için diskin SAS URI 'sini al |
> | Eylem | Microsoft. COMPUTE/diskler/Sil | Diski siler |
> | Eylem | Microsoft. COMPUTE/Disks/endGetAccess/Action | Diskin SAS URI 'sini iptal etme |
> | Eylem | Microsoft. COMPUTE/Disks/Read | Bir diskin özelliklerini al |
> | Eylem | Microsoft. COMPUTE/Disks/Write | Yeni bir disk oluşturur veya var olan bir diski güncelleştirir |
> | Eylem | Microsoft. COMPUTE/galeriler/uygulamalar/Sil | Galeri uygulamasını siler |
> | Eylem | Microsoft. COMPUTE/galeriler/uygulamalar/okuma | Galeri uygulamasının özelliklerini alır |
> | Eylem | Microsoft. COMPUTE/galeriler/uygulamalar/sürümler/Sil | Galeri uygulaması sürümünü siler |
> | Eylem | Microsoft. COMPUTE/galeriler/uygulamalar/sürümler/okuma | Galeri uygulama sürümünün özelliklerini alır |
> | Eylem | Microsoft. COMPUTE/galeriler/uygulamalar/sürümler/yazma | Yeni bir galeri uygulaması sürümü oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft. COMPUTE/galeriler/uygulamalar/yazma | Yeni bir galeri uygulaması oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft. COMPUTE/galeriler/silme | Galeriyi siler |
> | Eylem | Microsoft. COMPUTE/galeriler/resimler/silme | Galeri görüntüsünü siler |
> | Eylem | Microsoft. COMPUTE/galeriler/resimler/okuma | Galeri resminin özelliklerini alır |
> | Eylem | Microsoft. COMPUTE/galeriler/resimler/sürümler/Sil | Galeri görüntüsü sürümünü siler |
> | Eylem | Microsoft. COMPUTE/galeriler/resimler/sürümler/okuma | Galeri görüntüsü sürümünün özelliklerini alır |
> | Eylem | Microsoft. COMPUTE/galeriler/resimler/sürümler/yazma | Yeni bir galeri görüntüsü sürümü oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft. COMPUTE/galeriler/resimler/yazma | Yeni bir galeri görüntüsü oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft. COMPUTE/galeriler/okuma | Galerinin özelliklerini alır |
> | Eylem | Microsoft. COMPUTE/galeriler/yazma | Yeni bir galeri oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft. COMPUTE/hostGroups/Delete | Konak grubunu siler |
> | Eylem | Microsoft. COMPUTE/hostGroups/konaklar/Delete | Konağı siler |
> | Eylem | Microsoft. COMPUTE/hostGroups/konaklar/Read | Bir konağın özelliklerini al |
> | Eylem | Microsoft. COMPUTE/hostGroups/konaklar/Write | Yeni bir konak oluşturur veya var olan bir konağı güncelleştirir |
> | Eylem | Microsoft. COMPUTE/hostGroups/Read | Bir konak grubunun özelliklerini al |
> | Eylem | Microsoft. COMPUTE/hostGroups/Write | Yeni bir konak grubu oluşturur veya var olan bir konak grubunu güncelleştirir |
> | Eylem | Microsoft. COMPUTE/Images/Delete | Görüntüyü siler |
> | Eylem | Microsoft. COMPUTE/Images/Read | Görüntünün özelliklerini al |
> | Eylem | Microsoft. COMPUTE/Images/Write | Yeni bir görüntü oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft. COMPUTE/konumlar/capsOperations/Read | Zaman uyumsuz bir Caps işleminin durumunu alır |
> | Eylem | Microsoft. COMPUTE/Locations/diskOperations/Read | Zaman uyumsuz disk işleminin durumunu alır |
> | Eylem | Microsoft. COMPUTE/Locations/logAnalytics/getRequestRateByInterval/Action | Kısıtlama tanılamayı azaltmaya yardımcı olmak için zaman aralığına göre toplam istekleri göstermek üzere Günlükler oluşturun. |
> | Eylem | Microsoft. COMPUTE/Locations/logAnalytics/Getkısıtledrequests/Action | ResourceName, OperationName veya uygulanan kısıtlama Ilkesi tarafından gruplanmış kısıtlanmış isteklerin toplamlarını göstermek için Günlükler oluşturun. |
> | Eylem | Microsoft. COMPUTE/konumlar/işlemler/okuma | Gets the status of an asynchronous operation |
> | Eylem | Microsoft. COMPUTE/Locations/yayımcılar/ArtifactTypes/teklifler/Read | Platform görüntüsü teklifinin özelliklerini al |
> | Eylem | Microsoft. COMPUTE/Locations/yayımcılar/ArtifactTypes/teklifler/SKU/okuma | Platform görüntüsü SKU 'sunun özelliklerini al |
> | Eylem | Microsoft. COMPUTE/Locations/yayımcılar/ArtifactTypes/teklifler/SKU 'lar/sürümler/okuma | Platform görüntüsü sürümünün özelliklerini al |
> | Eylem | Microsoft. COMPUTE/Locations/yayımcılar/ArtifactTypes/türler/okuma | Vmexgeri türünün özelliklerini al |
> | Eylem | Microsoft. COMPUTE/Locations/yayımcılar/ArtifactTypes/türler/sürümler/okuma | Vmexgeri sürümünün özelliklerini al |
> | Eylem | Microsoft. COMPUTE/konumlar/yayımcılar/okuma | Yayımcının özelliklerini al |
> | Eylem | Microsoft. COMPUTE/Locations/runCommands/Read | Konumdaki kullanılabilir çalıştırma komutlarını listeler |
> | Eylem | Microsoft. COMPUTE/Locations/kullanımlar/Read | Bir konumdaki aboneliğin işlem kaynakları için hizmet sınırlarını ve geçerli kullanım miktarlarını alır |
> | Eylem | Microsoft. COMPUTE/konumlar/vmSizes/Read | Bir konumdaki kullanılabilir sanal makine boyutlarını listeler |
> | Eylem | Microsoft. COMPUTE/Locations/vsmOperations/okuma | Sanal makine ölçek kümesi için bir zaman uyumsuz işlemin durumunu sanal makine çalışma zamanı hizmet uzantısı ile alır |
> | Eylem | Microsoft. COMPUTE/işlemler/okuma | Microsoft. COMPUTE kaynak sağlayıcısında kullanılabilir işlemleri listeler |
> | Eylem | Microsoft. COMPUTE/proximityPlacementGroups/Delete | Yakınlık yerleşimi grubunu siler |
> | Eylem | Microsoft. COMPUTE/proximityPlacementGroups/Read | Bir yakınlık yerleşimi grubunun özelliklerini al |
> | Eylem | Microsoft. COMPUTE/proximityPlacementGroups/Write | Yeni bir yakınlık yerleşimi grubu oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft. COMPUTE/Register/ACTION | Aboneliği Microsoft. COMPUTE kaynak sağlayıcısına kaydeder |
> | Eylem | Microsoft. COMPUTE/restorePointCollections/Delete | Geri yükleme noktası koleksiyonunu ve içerilen geri yükleme noktalarını siler |
> | Eylem | Microsoft. COMPUTE/restorePointCollections/Read | Geri yükleme noktası koleksiyonunun özelliklerini al |
> | Eylem | Microsoft. COMPUTE/restorePointCollections/restorePoints/Delete | Geri yükleme noktasını siler |
> | Eylem | Microsoft. COMPUTE/restorePointCollections/restorePoints/Read | Geri yükleme noktasının özelliklerini al |
> | Eylem | Microsoft. COMPUTE/restorePointCollections/restorePoints/Retrievesasurin/Action | Blob SAS URI 'leriyle birlikte geri yükleme noktasının özelliklerini al |
> | Eylem | Microsoft. COMPUTE/restorePointCollections/restorePoints/Write | Creates a new restore point |
> | Eylem | Microsoft. COMPUTE/restorePointCollections/Write | Yeni bir geri yükleme noktası koleksiyonu oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft. COMPUTE/Sharedvmımages/Delete | Sharedvmımage 'ı siler |
> | Eylem | Microsoft. COMPUTE/Sharedvmımages/Read | Sharedvmımage 'ın özelliklerini al |
> | Eylem | Microsoft. COMPUTE/Sharedvmımages/sürümler/Delete | Sharedvmımageversion 'ı silme |
> | Eylem | Microsoft. COMPUTE/Sharedvmımages/sürümler/okuma | Sharedvmımageversion özelliklerini al |
> | Eylem | Microsoft. COMPUTE/Sharedvmımages/sürümler/Çoğalt/eylem | Sharedvmımageversion 'ı hedef bölgelere çoğaltma |
> | Eylem | Microsoft. COMPUTE/Sharedvmımages/sürümler/Write | Yeni bir Sharedvmımageversion oluşturun veya var olan bir sürümü güncelleştirin |
> | Eylem | Microsoft. COMPUTE/Sharedvmımages/Write | Yeni bir Sharedvmımage oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft. COMPUTE/SKU/okuma | Aboneliğiniz için kullanılabilen Microsoft. COMPUTE SKU 'Larının listesini alır |
> | Eylem | Microsoft. COMPUTE/Snapshot/beginGetAccess/Action | Blob erişimi için anlık görüntünün SAS URI 'sini alma |
> | Eylem | Microsoft. COMPUTE/Snapshot/Delete | Anlık görüntüyü silme |
> | Eylem | Microsoft. COMPUTE/Snapshot/endGetAccess/Action | Anlık görüntünün SAS URI 'sini iptal et |
> | Eylem | Microsoft. COMPUTE/Snapshot/Read | Anlık görüntünün özelliklerini al |
> | Eylem | Microsoft. COMPUTE/Snapshot/Write | Yeni bir anlık görüntü oluşturma veya var olanı güncelleştirme |
> | Eylem | Microsoft. COMPUTE/kaydını kaldırma/eylem | Microsoft. COMPUTE kaynak sağlayıcısı ile aboneliğin kaydını siler |
> | Eylem | Microsoft. COMPUTE/virtualMachines/yakala/eylem | Sanal sabit diskleri kopyalayarak sanal makineyi yakalar ve benzer sanal makineler oluşturmak için kullanılabilecek bir şablon oluşturur |
> | Eylem | Microsoft. COMPUTE/virtualMachines/convertToManagedDisks/Action | Sanal makinenin blob tabanlı disklerini yönetilen disklere dönüştürür |
> | Eylem | Microsoft. COMPUTE/virtualMachines/serbest bırakma/eylem | Sanal makineyi güçlendirir ve işlem kaynaklarını serbest bırakır |
> | Eylem | Microsoft.Compute/virtualMachines/delete | Sanal makineyi siler |
> | Eylem | Microsoft. COMPUTE/virtualMachines/Extensions/Sil | Sanal makine uzantısını siler |
> | Eylem | Microsoft. COMPUTE/virtualMachines/Extensions/okuma | Bir sanal makine uzantısının özelliklerini al |
> | Eylem | Microsoft. COMPUTE/virtualMachines/uzantılar/Write | Yeni bir sanal makine uzantısı oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft. COMPUTE/virtualMachines/Genelleştir/eylem | Sanal makinenin durumunu Genelleştirilmiş olarak ayarlar ve sanal makineyi yakalamaya hazırlar |
> | Eylem | Microsoft. COMPUTE/virtualMachines/InstanceView/Read | Sanal makinenin ve kaynaklarının ayrıntılı çalışma zamanı durumunu alır |
> | DataAction | Microsoft.Compute/virtualMachines/login/action | Bir sanal makinede normal kullanıcı olarak oturum açın |
> | DataAction | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Windows yönetici veya Linux kök kullanıcı ayrıcalıklarına sahip bir sanal makinede oturum açma |
> | Eylem | Microsoft. COMPUTE/virtualMachines/performMaintenance/Action | VM üzerinde bakım Işlemini gerçekleştirir. |
> | Eylem | Microsoft. COMPUTE/virtualMachines/powerOff/ACTION | Sanal makineyi kapatır. Sanal makinenin faturalandırılmaya devam edip etmediğini unutmayın. |
> | Eylem | Microsoft. COMPUTE/virtualMachines/okuma | Bir sanal makinenin özelliklerini al |
> | Eylem | Microsoft. COMPUTE/virtualMachines/yeniden dağıtma/eylem | Sanal makineyi yeniden dağıtır |
> | Eylem | Microsoft. COMPUTE/virtualMachines/ReImage/Action | Fark kayıt diski kullanan sanal makineyi yeniden görüntüler. |
> | Eylem | Microsoft. COMPUTE/virtualMachines/yeniden Başlat/eylem | Sanal makineyi yeniden başlatır |
> | Eylem | Microsoft. COMPUTE/virtualMachines/runCommand/Action | Sanal makinede önceden tanımlanmış bir betiği yürütür |
> | Eylem | Microsoft. COMPUTE/virtualMachines/Başlat/eylem | Sanal makineyi başlatır |
> | Eylem | Microsoft.Compute/virtualMachines/vmSizes/read | Sanal makinenin güncelleştirileceği kullanılabilir boyutları listeler |
> | Eylem | Microsoft. COMPUTE/virtualMachines/Write | Yeni bir sanal makine oluşturur veya var olan bir sanal makineyi güncelleştirir |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/ayırmayı kaldırma/eylem | Sanal makine ölçek kümesinin örnekleri için işlem kaynaklarını güçlendirir ve serbest bırakır  |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/Delete | Sanal makine ölçek kümesini siler |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/Delete/Action | Sanal makine ölçek kümesinin örneklerini siler |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/Extensions/Delete | Sanal makine ölçek kümesi uzantısını siler |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/Extensions/Read | Bir sanal makine ölçek kümesi uzantısının özelliklerini alır |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/Extensions/Write | Yeni bir sanal makine ölçek kümesi uzantısı oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/forceRecoveryServiceFabricPlatformUpdateDomainWalk/Action | Bir Service Fabric sanal makine ölçek kümesinin platform güncelleştirme etki alanlarına, takılı bekleyen bir güncelleştirmeyi tamamlayacak şekilde el ile kılavuzluk edin |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/InstanceView/Read | Sanal makine ölçek kümesinin örnek görünümünü alır |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/manualUpgrade/Action | Örnekleri, sanal makine ölçek kümesinin en son modeline el ile güncelleştirir |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/NetworkInterfaces/Read | Bir sanal makine ölçek kümesinin tüm ağ arabirimlerinin özelliklerini al |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/osRollingUpgrade/Action | Tüm sanal makine ölçek kümesi örneklerini kullanılabilir en son Platform görüntüsü işletim sistemi sürümüne taşımak için bir sıralı yükseltme başlatır. |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/osUpgradeHistory/Read | Bir sanal makine ölçek kümesi için işletim sistemi yükseltmelerinden oluşan geçmişi alır |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/performMaintenance/Action | Sanal makine ölçek kümesinin örneklerinde planlı bakım gerçekleştirir |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/powerOff/ACTION | Sanal makine ölçek kümesi örneklerinin kuvvetlerini kapatır |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/publicIPAddresses/Read | Bir sanal makine ölçek kümesinin tüm genel IP adreslerinin özelliklerini al |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/Read | Bir sanal makine ölçek kümesinin özelliklerini al |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/yeniden dağıtma/eylem | Sanal makine ölçek kümesinin örneklerini yeniden dağıtın |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/ReImage/Action | Sanal makine ölçek kümesinin örneklerini yeniden görüntüler |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/Reımageall/ACTION | Bir sanal makine ölçek kümesi örnekleri için tüm diskleri (işletim sistemi diski ve veri diskleri) yeniden görüntüler  |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/restart/Action | Sanal makine ölçek kümesinin örneklerini yeniden başlatır |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/Rollingyükseltmelerinde/iptal/eylem | Bir sanal makine ölçek kümesinin sıralı yükseltmesini iptal eder |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/Rollingyükseltmelerinde/okunan | Bir sanal makine ölçek kümesi için son sıralı yükseltme durumunu al |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/Scale/Action | Mevcut bir sanal makine ölçek kümesinin, belirtilen örnek sayısına göre ölçeklendirebildiğini/ölçeklendirdiğini doğrulayın |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/SKU 'lar/okuma | Mevcut bir sanal makine ölçek kümesi için geçerli SKU 'Ları listeler |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/start/Action | Sanal makine ölçek kümesinin örneklerini başlatır |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/serbest bırakma/eylem | VM Ölçek kümesindeki bir sanal makinenin işlem kaynaklarını güçlendirir ve serbest bırakır. |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/Delete | Bir VM Ölçek kümesindeki belirli bir sanal makineyi silin. |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/InstanceView/Read | Bir VM Ölçek kümesindeki bir sanal makinenin örnek görünümünü alır. |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/NetworkInterfaces/ipConfigurations/Publicıpaddresses/Read | Sanal makine ölçek kümesi kullanılarak oluşturulan genel IP adresinin özelliklerini al. Sanal makine ölçek kümesi, IP yapılandırması başına en çok bir genel IP (özel IP) oluşturabilir |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/NetworkInterfaces/ipConfigurations/Read | Sanal makine ölçek kümesi kullanılarak oluşturulan bir ağ arabiriminin bir veya tüm IP yapılandırmalarının özelliklerini alın. IP yapılandırması özel IP 'Leri temsil eder |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/NetworkInterfaces/Read | Sanal makine ölçek kümesi kullanılarak oluşturulan bir sanal makinenin ağ arabirimlerinin bir veya tüm özelliklerini al |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/performMaintenance/Action | Sanal makine ölçek kümesindeki bir sanal makine örneğinde planlanmış bakım gerçekleştirir |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/powerOff/ACTION | Bir VM Ölçek kümesindeki bir sanal makine örneğini güçlendirir. |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/Read | VM Ölçek kümesindeki bir sanal makinenin özelliklerini alır |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/yeniden dağıtma/eylem | Sanal makine ölçek kümesindeki bir sanal makine örneğini yeniden dağıtır |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/ReImage/Action | Sanal makine ölçek kümesindeki bir sanal makine örneğini yeniden görüntüler. |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/Reımageall/ACTION | Sanal makine ölçek kümesindeki sanal makine örneği için tüm diskleri (işletim sistemi diski ve veri diskleri) yeniden görüntüler. |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/restart/Action | Bir VM Ölçek kümesindeki bir sanal makine örneğini yeniden başlatır. |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/runCommand/Action | Bir sanal makine ölçek kümesindeki bir sanal makine örneğinde önceden tanımlanmış bir betiği yürütür. |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/start/Action | VM Ölçek kümesindeki bir sanal makine örneğini başlatır. |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/Write | Bir VM Ölçek kümesindeki bir sanal makinenin özelliklerini güncelleştirir |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/vmSizes/Read | Sanal makine ölçek kümesinde bir sanal makine oluşturmak veya güncelleştirmek için kullanılabilir boyutları listeleyin |
> | Eylem | Microsoft. COMPUTE/virtualMachineScaleSets/Write | Yeni bir sanal makine ölçek kümesi oluşturur veya mevcut olanı güncelleştirir |

## <a name="microsoftconsumption"></a>Microsoft. tüketim

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. tüketim/aggregmalyt/okuma | Yönetim grubu için Aggregmalyt listesini listeleyin. |
> | Eylem | Microsoft. tüketim/bakiye/okuma | Bir yönetim grubu için bir faturalandırma dönemi için kullanım özetini listeleyin. |
> | Eylem | Microsoft. tüketim/bütçe/silme | Bütçeleri bir aboneliğe veya bir yönetim grubuna göre silin. |
> | Eylem | Microsoft. tüketim/bütçeler/okuma | Bütçeleri bir aboneliğe veya bir yönetim grubuna göre listeleyin. |
> | Eylem | Microsoft. tüketim/bütçe/yazma | Bütçeleri bir aboneliğe veya bir yönetim grubuna göre oluşturur ve güncelleştirir. |
> | Eylem | Microsoft. tüketim/ücretler/okuma | Ücretleri listeleyin |
> | Eylem | Microsoft. tüketim/krediler/okuma | Kredileri Listele |
> | Eylem | Microsoft. tüketim/olaylar/okuma | Olayları listeleme |
> | Eylem | Microsoft. tüketim/externalBillingAccounts/Tag/Read | EA ve abonelikler için Etiketler listeleyin. |
> | Eylem | Microsoft. tüketim/Externalabonelikleri/etiketleri/okuma | EA ve abonelikler için Etiketler listeleyin. |
> | Eylem | Microsoft. tüketim/tahminler/okuma | Tahminleri listeleyin |
> | Eylem | Microsoft. tüketim/lotlar/okuma | Lotlar Listele |
> | Eylem | Microsoft. tüketim/pazar yerleri/okuma | EA ve WebDirect abonelikleri için bir kapsamın Market kaynak kullanımı ayrıntılarını listeleyin. |
> | Eylem | Microsoft. tüketim/operationresults/Read | Operationresults listeleme |
> | Eylem | Microsoft. tüketim/işlemler/okuma | Microsoft. tüketim kaynak sağlayıcısı tarafından desteklenen tüm işlemleri listeleyin. |
> | Eylem | Microsoft. tüketim/OperationStatus/Read | OperationStatus listeleme |
> | Eylem | Microsoft. tüketim/pricesheets/Read | Bir abonelik veya bir yönetim grubu için Pricesheets verilerini listeleyin. |
> | Eylem | Microsoft. tüketim/Register/ACTION | Tüketim RP 'ye kaydol |
> | Eylem | Microsoft. tüketim/Rezervationdetails/okuma | Ayrılmış örnekler için ayırma sırasına veya yönetim gruplarına göre kullanım ayrıntılarını listeleyin. Ayrıntı verileri günlük düzeyi başına örnek başına. |
> | Eylem | Microsoft. tüketim/Rezervationönerilere/okuma | Bir abonelik için ayrılmış örnekler için tek veya paylaşılan öneriler listeleyin. |
> | Eylem | Microsoft. tüketim/Rezervationözetler/okuma | Ayrılmış örnekler için kullanım özetini rezervasyon siparişi veya yönetim gruplarına göre listeleyin. Özet verileri aylık ya da günlük düzeyindedir. |
> | Eylem | Microsoft. tüketim/Rezervationtransactions/Read | Ayrılmış örnekler için yönetim gruplarına göre işlem geçmişini listeleyin. |
> | Eylem | Microsoft. tüketim/Etiketler/okuma | EA ve abonelikler için Etiketler listeleyin. |
> | Eylem | Microsoft. tüketim/kiracılar/okuma | Kiracılar Listele |
> | Eylem | Microsoft. tüketim/kiracılar/yazmaç/eylem | Bir kiracının Microsoft. tüketim kapsamı için kaydetme eylemi. |
> | Eylem | Microsoft. tüketim/hüküm/okuma | Bir abonelik veya bir yönetim grubu için koşulları listeleyin. |
> | Eylem | Microsoft. tüketim/usageDetails/okuma | EA ve WebDirect abonelikleri için bir kapsamın kullanım ayrıntılarını listeleyin. |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. Containerınstance/containerGroups/kapsayıcılar/buildlogs/Read | Belirli bir kapsayıcı için derleme günlüklerini al. |
> | Eylem | Microsoft. Containerınstance/containerGroups/kapsayıcılar/exec/Action | Belirli bir kapsayıcıya çalıştırılabilir. |
> | Eylem | Microsoft. Containerınstance/containerGroups/kapsayıcılar/Günlükler/okuma | Belirli bir kapsayıcı için günlükleri al. |
> | Eylem | Microsoft. Containerınstance/containerGroups/Delete | Belirli kapsayıcı grubunu silin. |
> | Eylem | Microsoft. Containerınstance/containerGroups/operationResults/Read | Zaman uyumsuz işlem sonucunu al |
> | Eylem | Microsoft. Containerınstance/containerGroups/Providers/Microsoft. Insights/diagnosticSettings/Read | Kapsayıcı grubu için tanılama ayarını alır. |
> | Eylem | Microsoft. Containerınstance/containerGroups/Providers/Microsoft. Insights/diagnosticSettings/Write | Kapsayıcı grubu için tanılama ayarını oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. Containerınstance/containerGroups/Providers/Microsoft. Insights/metricDefinitions/Read | Kapsayıcı grubu için kullanılabilir ölçümleri alır. |
> | Eylem | Microsoft. Containerınstance/containerGroups/Read | Tüm kapsayıcı gruplarını al. |
> | Eylem | Microsoft. Containerınstance/containerGroups/restart/Action | Belirli bir kapsayıcı grubunu yeniden başlatır. |
> | Eylem | Microsoft. Containerınstance/containerGroups/start/Action | Belirli bir kapsayıcı grubunu başlatır. |
> | Eylem | Microsoft. Containerınstance/containerGroups/durdur/eylem | Belirli bir kapsayıcı grubunu sonlandırır. İşlem kaynakları serbest bırakılır ve faturalandırma durdurulur. |
> | Eylem | Microsoft. Containerınstance/containerGroups/Write | Belirli bir kapsayıcı grubunu oluşturun veya güncelleştirin. |
> | Eylem | Microsoft. Containerınstance/Locations/Önbellekedimages/Read | Bir bölgedeki aboneliğin önbelleğe alınmış görüntülerini alır. |
> | Eylem | Microsoft. Containerınstance/Locations/Capabilities/Read | Bir bölgenin yeteneklerini alın. |
> | Eylem | Microsoft. Containerınstance/Locations/Deletevirtualnetworkoralt ağları/eylem | , Sanal ağın veya alt ağın silinmekte olduğunu Microsoft. Containerınstance 'a bildirir. |
> | Eylem | Microsoft. Containerınstance/Locations/işlemler/okuma | Azure Container Instance hizmeti için işlemleri listeleyin. |
> | Eylem | Microsoft. Containerınstance/Locations/kullanımlar/Read | Belirli bir bölgenin kullanımını alın. |
> | Eylem | Microsoft. Containerınstance/işlemler/okuma | Azure Container Instance hizmeti için işlemleri listeleyin. |
> | Eylem | Microsoft. Containerınstance/Register/ACTION | Kapsayıcı örneği kaynak sağlayıcısı için aboneliği kaydeder ve kapsayıcı gruplarının oluşturulmasına izin vermez. |
> | Eylem | Microsoft. Containerınstance/serviceassociationlinks/Delete | Azure Container Instance kaynak sağlayıcısı tarafından bir alt ağda oluşturulan hizmet ilişkisi bağlantısını silin. |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. ContainerRegistry/Checknameavaılabılıty/Read | Kapsayıcı kayıt defteri adının kullanıma hazır olup olmadığını denetler. |
> | Eylem | Microsoft. ContainerRegistry/Locations/Deletevirtualnetworkoralt ağları/eylem | Sanal ağ veya alt ağın silindiği Microsoft. ContainerRegistry öğesine bildirir |
> | Eylem | Microsoft. ContainerRegistry/konumlar/operationResults/Read | Zaman uyumsuz işlem sonucunu alır |
> | Eylem | Microsoft. ContainerRegistry/işlemler/okuma | Tüm kullanılabilir Azure Container Registry REST API işlemlerini listeler |
> | Eylem | Microsoft. ContainerRegistry/Register/ACTION | Kapsayıcı kayıt defteri kaynak sağlayıcısı için aboneliği kaydeder ve kapsayıcı kayıt defterlerinin oluşturulmasını sunar. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/yapıtlar/silme | Bir kapsayıcı kayıt defterinde yapıtı silin. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/derlemeler/iptal/eylem | Var olan bir derlemeyi iptal eder. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/derlemeler/getLogLink/Action | Derleme günlüklerini indirmek için bir bağlantı alır. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/derlemeler/okuma | Belirtilen derleme özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm derlemeleri listeler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/derlemeler/yazma | Bir kapsayıcı kayıt defteri için belirtilen parametrelerle bir derlemeyi güncelleştirir. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/buildTasks/Delete | Bir kapsayıcı kayıt defterinden derleme görevini siler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/buildTasks/listSourceRepositoryProperties/Action | Yapı görevinin kaynak denetim özelliklerini listeler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/buildTasks/Read | Belirtilen derleme görevinin özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm derleme görevlerini listeler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/buildTasks/Steps/Delete | Derleme görevinin yapı adımını siler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/buildTasks/Steps/listBuildArguments/ACTION | Gizli bağımsız değişkenler de dahil olmak üzere derleme adımının derleme bağımsız değişkenlerini listeler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/buildTasks/Steps/Read | Belirtilen derleme adımının özelliklerini alır veya belirtilen derleme görevinin tüm derleme adımlarını listeler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/buildTasks/Steps/Write | Belirtilen parametrelerle derleme görevi için derleme adımı oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/buildTasks/Write | Belirtilen parametrelerle bir kapsayıcı kayıt defteri için derleme görevi oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/silme | Bir kapsayıcı kayıt defterini siler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/eventGridFilters/Delete | Bir kapsayıcı kayıt defterinden bir olay Kılavuzu filtresini siler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/eventGridFilters/Read | Belirtilen olay Kılavuzu filtresinin özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm olay Kılavuzu filtrelerini listeler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/eventGridFilters/Write | Belirtilen parametrelerle bir kapsayıcı kayıt defteri için bir olay kılavuz filtresi oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/generateCredentials/eylem | Belirtilen kapsayıcı kayıt defterinin belirteci için anahtarlar oluşturun. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/getBuildSourceUploadUrl/Action | Kullanıcının kaynağı yükleyebilmesi için karşıya yükleme konumunu alır. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/ımportımage/eylem | Görüntüyü, belirtilen parametrelerle kapsayıcı kayıt defterine aktarın. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/listBuildSourceUploadUrl/Action | Bir kapsayıcı kayıt defteri için kaynak yükleme URL 'si konumunu al. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/listCredentials/Action | Belirtilen kapsayıcı kayıt defteri için oturum açma kimlik bilgilerini listeler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/listPolicies/Read | Belirtilen kapsayıcı kayıt defteri için ilkeleri listeler |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/Listkullanımlar/okuma | Belirtilen kapsayıcı kayıt defteri için kota kullanımlarını listeler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/meta veriler/okuma | Bir kapsayıcı kayıt defteri için belirli bir deponun meta verilerini alır |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/meta veri/yazma | Bir kapsayıcı kayıt defteri için bir deponun meta verilerini güncelleştirir |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/Operationdurumlarının/Read | Kayıt defteri zaman uyumsuz işlem durumunu alır |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/çekme/okuma | Bir kapsayıcı kayıt defterinden görüntüleri çekin veya alın. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/gönderme/yazma | Bir kapsayıcı kayıt defterine görüntü gönderin veya yazın. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/karantina/okuma | Kapsayıcı kayıt defterinden karantinaya alınmış görüntüleri çekme veya alma |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/karantina/yazma | Karantinaya alınan görüntülerin karantina durumunu yazma/değiştirme |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/queueBuild/Action | İstek parametrelerine göre yeni bir yapı oluşturur ve yapı kuyruğuna ekler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/okuma | Belirtilen kapsayıcı kayıt defterinin özelliklerini alır veya belirtilen kaynak grubu veya abonelik altındaki tüm kapsayıcı kayıt defterlerinin listesini görüntüler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/regenerateCredential/ACTION | Belirtilen kapsayıcı kayıt defteri için oturum açma kimlik bilgilerinden birini yeniden oluşturur. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/çoğaltmalar/silme | Bir kapsayıcı kayıt defterinden bir çoğaltmayı siler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/çoğaltmalar/Operationdurumlarının/Read | Çoğaltma zaman uyumsuz işlem durumunu alır |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/çoğaltmalar/okuma | Belirtilen çoğaltmanın özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm çoğaltmaları listeler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/çoğaltmalar/yazma | Belirtilen parametrelerle bir kapsayıcı kayıt defteri için çoğaltma oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/çalıştırmalar/iptal/eylem | Mevcut bir çalıştırmayı iptal edin. |
> | Eylem | Microsoft. ContainerRegistry/Registry/çalıştırmaları/listLogSasUrl/Action | Bir çalıştırmanın günlük SAS URL 'sini alır. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/çalıştırmalar/Read | Bir kapsayıcı kayıt defterinde veya liste çalıştırmalarından bir çalıştırmanın özelliklerini alır. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/çalıştırmalar/Write | Bir çalıştırmayı güncelleştirir. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/scheduleRun/Action | Bir kapsayıcı kayıt defterine karşı bir çalıştırma zamanlayın. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/Kapsamharitaları/silme | Bir kapsayıcı kayıt defterinden kapsam eşlemesini siler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/Kapsamharitaları/Operationdurumlarının/okunan | Kapsam eşlemesi zaman uyumsuz işlem durumunu alır. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/Kapsamharitaları/okuma | Belirtilen kapsam haritasının özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm kapsam haritalarını listeler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/Kapsamharitaları/yazma | Belirtilen parametrelerle bir kapsayıcı kayıt defteri için kapsam haritası oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/imzala/yaz | Kapsayıcı kayıt defteri için gönderme/çekme içeriği güven meta verileri. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/görevler/silme | Bir kapsayıcı kayıt defteri için bir görevi siler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/görevler/listDetails/Action | Bir kapsayıcı kayıt defteri için bir görevin tüm ayrıntılarını listeleyin. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/görevler/okuma | Bir kapsayıcı kayıt defteri için bir görev alır veya tüm görevleri listeler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/görevler/yazma | Bir kapsayıcı kayıt defteri için bir görev oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/belirteçler/silme | Bir kapsayıcı kayıt defterinden belirteç siler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/belirteçler/Operationdurumlarının/okunan | Belirteç zaman uyumsuz işlem durumunu alır. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/belirteçler/okuma | Belirtilen belirtecin özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm belirteçleri listeler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/belirteçler/yazma | Belirtilen parametrelerle bir kapsayıcı kayıt defteri için belirteç oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/updatePolicies/Write | Belirtilen kapsayıcı kayıt defteri için ilkeleri güncelleştirir |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/Web kancaları/silme | Bir Web kancasını kapsayıcı kayıt defterinden siler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/Web kancaları/getCallbackConfig/Action | Web kancası için hizmet URI 'SI ve özel üst bilgilerin yapılandırmasını alır. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/Web kancaları/listEvents/eylem | Belirtilen Web kancası için son olayları listeler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/Web kancaları/Operationdurumlarının/Read | Web kancası zaman uyumsuz işlem durumunu alır |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/Web kancaları/ping/eylem | Web kancasına gönderilecek bir ping olayını tetikler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/Web kancaları/okuma | Belirtilen kapsayıcı kayıt defteri için belirtilen Web kancasının özelliklerini alır veya tüm Web kancalarını listeler. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/Web kancaları/yazma | Belirtilen parametrelerle bir kapsayıcı kayıt defteri için Web kancası oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. ContainerRegistry/kayıt defterleri/yazma | Belirtilen parametrelerle bir kapsayıcı kayıt defteri oluşturur veya güncelleştirir. |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. ContainerService/containerServices/Delete | Bir kapsayıcı hizmetini siler |
> | Eylem | Microsoft. ContainerService/containerServices/Read | Kapsayıcı hizmeti al |
> | Eylem | Microsoft. ContainerService/containerServices/Write | Yeni bir kapsayıcı hizmeti oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft. ContainerService/konumlar/operationresults/Read | Zaman uyumsuz bir işlem sonucunun durumunu alır |
> | Eylem | Microsoft. ContainerService/konumlar/işlemler/okuma | Gets the status of an asynchronous operation |
> | Eylem | Microsoft. ContainerService/konumlar/orchestrators/okunan | Desteklenen düzenleyiciler listeler |
> | Eylem | Microsoft. ContainerService/Managedkümeler/accessProfiles/listCredential/Action | Liste kimlik bilgisini kullanarak rol adına göre yönetilen küme erişim profili al |
> | Eylem | Microsoft. ContainerService/Managedkümeler/accessProfiles/okuma | Rol adına göre yönetilen küme erişim profili al |
> | Eylem | Microsoft. ContainerService/Managedkümeler/agentPools/Delete | Bir aracı havuzunu siler |
> | Eylem | Microsoft. ContainerService/Managedkümeler/agentPools/okuma | Bir aracı havuzu alır |
> | Eylem | Microsoft. ContainerService/Managedkümeler/agentPools/upgradeProfiles/okuma | Aracı havuzunun yükseltme profilini alır |
> | Eylem | Microsoft. ContainerService/Managedkümeler/agentPools/Write | Yeni bir aracı havuzu oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft. ContainerService/Managedkümeler/kullanılabilirlik Bleagentpoolversions/Read | Kümenin kullanılabilir aracı havuzu sürümlerini alır |
> | Eylem | Microsoft. ContainerService/Managedkümeler/Sil | Yönetilen bir kümeyi siler |
> | Eylem | Microsoft. ContainerService/Managedkümeler/detektorları/okuma | Yönetilen küme algılayıcısı 'nı al |
> | Eylem | Microsoft. ContainerService/Managedkümeler/diagnosticsState/Read | Kümenin tanılama durumunu alır |
> | Eylem | Microsoft. ContainerService/Managedkümeler/listClusterAdminCredential/ACTION | Yönetilen kümenin clusterAdmin kimlik bilgisini listeleyin |
> | Eylem | Microsoft. ContainerService/Managedkümeler/listClusterMonitoringUserCredential/Action | Yönetilen kümenin clusterMonitoringUser kimlik bilgisini listeleyin |
> | Eylem | Microsoft. ContainerService/Managedkümeler/listClusterUserCredential/Action | Yönetilen kümenin clusterUser kimlik bilgisini listeleyin |
> | Eylem | Microsoft. ContainerService/Managedkümeler/privateEndpointConnectionsApproval/eylem | Determines if user is allowed to approve a private endpoint connection |
> | Eylem | Microsoft. ContainerService/Managedkümeler/okuma | Yönetilen bir küme al |
> | Eylem | Microsoft. ContainerService/Managedkümeler/resetAADProfile/Action | Yönetilen kümenin AAD profilini sıfırlayın |
> | Eylem | Microsoft. ContainerService/Managedkümeler/resetServicePrincipalProfile/Action | Yönetilen kümenin hizmet sorumlusu profilini sıfırlayın |
> | Eylem | Microsoft. ContainerService/Managedkümeler/rotateClusterCertificates/eylem | Yönetilen kümenin sertifikalarını döndürme |
> | Eylem | Microsoft. ContainerService/Managedkümeler/upgradeProfiles/okuma | Kümenin yükseltme profilini alır |
> | Eylem | Microsoft. ContainerService/Managedkümeler/Write | Yeni bir yönetilen küme oluşturur veya var olan bir kümeyi güncelleştirir |
> | Eylem | Microsoft. ContainerService/openShiftClusters/Delete | Açık bir kaydırma kümesini silme |
> | Eylem | Microsoft. ContainerService/openShiftClusters/Read | Açık bir Shift kümesi al |
> | Eylem | Microsoft. ContainerService/openShiftClusters/Write | Yeni bir açık vardiya kümesi oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft. ContainerService/openShiftManagedClusters/Delete | Açık bir vardiya yönetilen kümesini silme |
> | Eylem | Microsoft. ContainerService/openShiftManagedClusters/Read | Açık bir vardiya yönetilen kümesi al |
> | Eylem | Microsoft. ContainerService/openShiftManagedClusters/Write | Yeni bir açık vardiya yönetilen kümesi oluşturur veya mevcut olanı güncelleştirir |
> | Eylem | Microsoft. ContainerService/işlemler/okuma | Microsoft. ContainerService kaynak sağlayıcısında kullanılabilir işlemleri listeler |
> | Eylem | Microsoft. ContainerService/Register/Action | Aboneliği Microsoft. ContainerService kaynak sağlayıcısına kaydeder |
> | Eylem | Microsoft. ContainerService/Unregister/eylem | Microsoft. ContainerService kaynak sağlayıcısı ile aboneliğin kaydını siler |

## <a name="microsoftcostmanagement"></a>Microsoft. CostManagement

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. CostManagement/uyarılar/Write | Uyarıları güncelleştirin. |
> | Eylem | Microsoft. CostManagement/Cloudbağlayıcılar/Delete | Belirtilen cloudConnector 'ı silin. |
> | Eylem | Microsoft. CostManagement/Cloudkonnektörleri/okuma | Kimliği doğrulanmış kullanıcı için Cloudbağlayıcıları listeleyin. |
> | Eylem | Microsoft. CostManagement/Cloudbağlayıcılar/Write | Belirtilen cloudConnector 'ı oluşturun veya güncelleştirin. |
> | Eylem | Microsoft. CostManagement/Dimensions/Read | Desteklenen tüm boyutları bir kapsama göre listeleyin. |
> | Eylem | Microsoft. CostManagement/dışarı aktarmalar/eylem | Belirtilen dışarı aktarmayı çalıştırın. |
> | Eylem | Microsoft. CostManagement/dışarı aktarmalar/Sil | Belirtilen dışarı aktarmayı silin. |
> | Eylem | Microsoft. CostManagement/dışarı aktarmalar/okundu | Dışarı aktarmaları kapsama göre listeleyin. |
> | Eylem | Microsoft. CostManagement/dışarı aktarmalar/Çalıştır/eylem | Dışarı aktarmaları Çalıştır. |
> | Eylem | Microsoft. CostManagement/dışarı aktarmalar/yaz | Belirtilen dışarı aktarmayı oluşturun veya güncelleştirin. |
> | Eylem | Microsoft. CostManagement/externalBillingAccounts/Dimension/Read | Dış BillingAccounts için desteklenen tüm boyutları listeleyin. |
> | Eylem | Microsoft. CostManagement/externalBillingAccounts/Externalabonelikleri/okuma | Kimliği doğrulanmış kullanıcı için externalBillingAccount içindeki Externalaboneliklerinizi listeleyin. |
> | Eylem | Microsoft. CostManagement/externalBillingAccounts/tahmin/eylem | Dış BillingAccounts için kullanım verilerini tahmin edin. |
> | Eylem | Microsoft. CostManagement/externalBillingAccounts/tahmin/okuma | Dış BillingAccounts için kullanım verilerini tahmin edin. |
> | Eylem | Microsoft. CostManagement/externalBillingAccounts/sorgu/eylem | Dış BillingAccounts için kullanım verilerini sorgulama. |
> | Eylem | Microsoft. CostManagement/externalBillingAccounts/sorgu/okuma | Dış BillingAccounts için kullanım verilerini sorgulama. |
> | Eylem | Microsoft. CostManagement/externalBillingAccounts/Read | Kimliği doğrulanmış kullanıcı için externalBillingAccounts listesini listeleyin. |
> | Eylem | Microsoft. CostManagement/Externalabonelikleri/boyutlar/okuma | Dış abonelik için desteklenen tüm boyutları listeleyin. |
> | Eylem | Microsoft. CostManagement/Externalabonelikleriniz/tahmin/eylem | Dış BillingAccounts için kullanım verilerini tahmin edin. |
> | Eylem | Microsoft. CostManagement/Externalabonelikleriniz/tahmin/okuma | Dış BillingAccounts için kullanım verilerini tahmin edin. |
> | Eylem | Microsoft. CostManagement/Externalabonelikleri/sorgu/eylem | Dış abonelik için kullanım verilerini sorgula. |
> | Eylem | Microsoft. CostManagement/Externalabonelikleriniz/sorgu/okuma | Dış abonelik için kullanım verilerini sorgula. |
> | Eylem | Microsoft. CostManagement/Externalabonelikleri/okuma | Kimliği doğrulanmış kullanıcı için Externalaboneliklerinizi listeleyin. |
> | Eylem | Microsoft. CostManagement/Externalabonelikleri/yazma | ExternalSubscription ilişkili yönetim grubunu Güncelleştir |
> | Eylem | Microsoft. CostManagement/tahmin/eylem | Kullanım verilerini bir kapsama göre tahmin edin. |
> | Eylem | Microsoft. CostManagement/tahmin/okuma | Kullanım verilerini bir kapsama göre tahmin edin. |
> | Eylem | Microsoft. CostManagement/işlemler/okuma | Desteklenen tüm işlemleri Microsoft. CostManagement kaynak sağlayıcısına listeleyin. |
> | Eylem | Microsoft. CostManagement/Query/Action | Kullanım verilerini bir kapsama göre sorgulama. |
> | Eylem | Microsoft. CostManagement/Query/Read | Kullanım verilerini bir kapsama göre sorgulama. |
> | Eylem | Microsoft. CostManagement/Register/ACTION | Abonelik ile Microsoft. CostManagement kapsamı için kaydetme eylemi. |
> | Eylem | Microsoft. CostManagement/Reports/Action | Kullanım verileri hakkında raporları bir kapsama göre zamanlayın. |
> | Eylem | Microsoft. CostManagement/Reports/Read | Kullanım verileri hakkında raporları bir kapsama göre zamanlayın. |
> | Eylem | Microsoft. CostManagement/kiracılar/Register/Action | Bir kiracının Microsoft. CostManagement kapsamı için kaydetme eylemi. |
> | Eylem | Microsoft. CostManagement/views/Action | Görünüm Oluştur. |
> | Eylem | Microsoft. Maliyetyönetimi/görünümleri/silme | Kaydedilmiş görünümleri silin. |
> | Eylem | Microsoft. CostManagement/views/Read | Tüm kaydedilmiş görünümleri listeleyin. |
> | Eylem | Microsoft. CostManagement/views/Write | Güncelleştirme görünümü. |

## <a name="microsoftdatabox"></a>Microsoft. DataBox

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. DataBox/Jobs/bookShipmentPickUp/ACTION | İade sevk irsaliyeleri için bir çekme defterine izin verir. |
> | Eylem | Microsoft. DataBox/Jobs/iptal/eylem | Devam eden bir siparişi iptal eder. |
> | Eylem | Microsoft. DataBox/işler/Sil | Siparişleri silme |
> | Eylem | Microsoft. DataBox/Jobs/listCredentials/Action | Siparişle ilgili şifrelenmemiş kimlik bilgilerini listeler. |
> | Eylem | Microsoft. DataBox/Jobs/Read | Siparişleri listeleyin veya alın |
> | Eylem | Microsoft. DataBox/işler/yazma | Siparişleri oluşturma veya güncelleştirme |
> | Eylem | Microsoft. DataBox/Locations/Availablesku 'Lar/eylem | Bu yöntem, kullanılabilir SKU 'ların listesini döndürür. |
> | Eylem | Microsoft. DataBox/Locations/Availablesku 'Lar/okuma | Kullanılabilir SKU 'Ları listeleme veya edinme |
> | Eylem | Microsoft. DataBox/Locations/operationResults/Read | Işlem sonuçlarını listeleme veya edinme |
> | Eylem | Microsoft. DataBox/Locations/regionConfiguration/Action | Bu yöntem, bölgenin yapılandırmasını döndürür. |
> | Eylem | Microsoft. DataBox/Locations/validateAddress/Action | Sevkiyat adresini doğrular ve varsa alternatif adresler sağlar. |
> | Eylem | Microsoft. DataBox/Locations/Validategirişlerinde/Action | Bu yöntem, tüm doğrulama türlerini yapar. |
> | Eylem | Microsoft. DataBox/işlemler/okuma | Işlemleri listeleme veya edinme |
> | Eylem | Microsoft. DataBox/Register/Action | Sağlayıcıyı kaydet Microsoft. databox |
> | Eylem | Microsoft. DataBox/abonelikler/resourceGroups/Moveresonak/Action | Bu yöntem, kaynak taşımayı gerçekleştirir. |
> | Eylem | Microsoft. DataBox/abonelikler/resourceGroups/Validatemoveresonak/Action | Bu yöntem, kaynak taşımaya izin verilip verilmeyeceğini doğrular. |
> | Eylem | Microsoft. DataBox/kaydını kaldırma/eylem | Kaydettirme sağlayıcısını Kaldır Microsoft. databox |

## <a name="microsoftdataboxedge"></a>Microsoft. DataBoxEdge

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/uyarılar/okuma | Uyarıları listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/Delete | Bant genişliği zamanlamalarını siler |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/Read | Bant genişliği zamanlamalarını listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/Read | Bant genişliği zamanlamalarını listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/Write | Bant genişliği zamanlamalarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Delete | Data Box Edge cihazlarını siler |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/downloadUpdates/Action | Güncelleştirmeleri cihaza indir |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Gebir Dedınformation/Action | Kaynak genişletilmiş bilgilerini alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/ınstallupdates/Action | Güncelleştirmeleri cihaza yükler |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/işler/okuma | İşleri listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/networkSettings/Read | Cihaz ağ ayarlarını listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Nodes/Read | Düğümleri listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/operationsStatus/Read | İşlem durumunu listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/siparişler/Sil | Siparişleri siler |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/siparişler/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/siparişler/okuma | Siparişleri listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/siparişler/okuma | Siparişleri listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/siparişler/yazma | Siparişleri oluşturur veya güncelleştirir |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Read | Data Box Edge cihazlarını listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Read | Data Box Edge cihazlarını listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Read | Data Box Edge cihazlarını listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/roller/Delete | Rolleri siler |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/roller/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/roller/okuma | Rolleri listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/roller/okuma | Rolleri listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/roller/Write | Rolleri oluşturur veya güncelleştirir |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/scanForUpdates/Action | Güncelleştirmeleri Tara |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/securitySettings/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/securitySettings/Update/Action | Güvenlik ayarlarını güncelleştirme |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/paylaşımlar/silme | Paylaşımları siler |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/paylaşımlar/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/paylaşımlar/okuma | Paylaşımları listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/paylaşımlar/okuma | Paylaşımları listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/paylaşımlar/Yenile/eylem | Bulutta bulunan verilerle paylaşma meta verilerini yenileyin |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/paylaşımlar/yazma | Paylaşımları oluşturur veya güncelleştirir |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/Delete | Depolama hesabı kimlik bilgilerini siler |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/Read | Depolama hesabı kimlik bilgilerini listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/Read | Depolama hesabı kimlik bilgilerini listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/Write | Depolama hesabı kimlik bilgilerini oluşturur veya güncelleştirir |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Tetikleyiciler/silme | Tetikleyicileri siler |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Tetikleyiciler/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Tetikleyiciler/okuma | Tetikleyicileri listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Tetikleyiciler/okuma | Tetikleyicileri listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Tetikleyiciler/yazma | Tetikleyicileri oluşturur veya güncelleştirir |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/updateSummary/Read | Güncelleştirme özetini listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/uploadCertificate/Action | Cihaz kaydı için sertifikayı karşıya yükle |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/kullanıcılar/Sil | Paylaşma kullanıcılarını siler |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/kullanıcılar/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/kullanıcılar/okuma | Paylaşma kullanıcılarını listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/kullanıcılar/okuma | Paylaşma kullanıcılarını listeler veya alır |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/kullanıcılar/yazma | Paylaşma kullanıcıları oluşturur veya güncelleştirir |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Write | Data Box Edge cihazlarını oluşturur veya güncelleştirir |
> | Eylem | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Write | Data Box Edge cihazlarını oluşturur veya güncelleştirir |

## <a name="microsoftdatabricks"></a>Microsoft. Databricks

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. Databricks/Locations/getNetworkPolicies/Action | NRP tarafından kullanılan konuma göre bir alt ağ için ağ hedefi Ilkelerini al |
> | Eylem | Microsoft. Databricks/Locations/operationdurumlarının/Read | Reads the operation status for the resource. |
> | Eylem | Microsoft. Databricks/işlemler/okuma | Gets the list of operations. |
> | Eylem | Microsoft. Databricks/Register/ACTION | Databricks 'e kaydolun. |
> | Eylem | Microsoft. Databricks/Workspaces/Delete | Databricks çalışma alanını kaldırır. |
> | Eylem | Microsoft. Databricks/Workspaces/Providers/Microsoft. Insights/diagnosticSettings/Read | Databricks çalışma alanı için kullanılabilir tanılama ayarlarını ayarlar |
> | Eylem | Microsoft. Databricks/Workspaces/Providers/Microsoft. Insights/diagnosticSettings/Write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Eylem | Microsoft. Databricks/Workspaces/Providers/Microsoft. Insights/logDefinitions/Read | Databricks çalışma alanı için kullanılabilir günlük tanımlarını alır |
> | Eylem | Microsoft. Databricks/Workspaces/Read | Databricks çalışma alanlarının bir listesini alır. |
> | Eylem | Microsoft. Databricks/Workspaces/refreshPermissions/Action | Çalışma alanı için izinleri yenileme |
> | Eylem | Microsoft. Databricks/Workspaces/updateDenyAssignment/Action | Bir çalışma alanının yönetilen kaynak grubu için güncelleştirme reddetme ataması eylem değil |
> | Eylem | Microsoft. Databricks/Workspaces/Virtualnetworkpeerler/Sil | Deletes a virtual network peering |
> | Eylem | Microsoft. Databricks/Workspaces/Virtualnetworkpeerler/okuma | Sanal Ağ eşlemesini alır. |
> | Eylem | Microsoft. Databricks/Workspaces/Virtualnetworkpeerler/yaz | Sanal ağ eşlemesi ekleme veya değiştirme |
> | Eylem | Microsoft. Databricks/Workspaces/Write | Bir Databricks çalışma alanı oluşturur. |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. DataCatalog/Catalogs/Delete | Veri Kataloğu kaynak sağlayıcısı için Katalog kaynağını silin. |
> | Eylem | Microsoft. DataCatalog/Catalogs/Read | Veri Kataloğu kaynak sağlayıcısı için Katalog kaynağını okuyun. |
> | Eylem | Microsoft. DataCatalog/kataloglar/Write | Veri Kataloğu kaynak sağlayıcısı için Katalog kaynağı yazın. |
> | Eylem | Microsoft. DataCatalog/Checknameavaılabılıty/Read | Veri Kataloğu kaynak sağlayıcısı için katalog adı kullanılabilirliğini denetleyin. |
> | Eylem | Microsoft. DataCatalog/datacatalogs/Delete | Veri Kataloğu kaynak sağlayıcısı için DataCatalog kaynağını silin. |
> | Eylem | Microsoft. DataCatalog/datacatalogs/Read | Veri Kataloğu kaynak sağlayıcısı için DataCatalog kaynağını okuyun. |
> | Eylem | Microsoft. DataCatalog/datacatalogs/Write | Veri Kataloğu kaynak sağlayıcısı için DataCatalog kaynağını yazın. |
> | Eylem | Microsoft. DataCatalog/Operations/Read | Veri Kataloğu kaynak sağlayıcısındaki tüm kullanılabilir işlemleri okur. |
> | Eylem | Microsoft. DataCatalog/Register/ACTION | Veri Kataloğu kaynak sağlayıcısı için aboneliği kaydetme |
> | Eylem | Microsoft. DataCatalog/Unregister/ACTION | Veri Kataloğu kaynak sağlayıcısı aboneliğini silme |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. DataFactory/checkazuredatafactorynameavailability/Read | Data Factory adının kullanılabilir olup olmadığını denetler. |
> | Eylem | Microsoft. DataFactory/DataFactory/activitywindows/Read | Belirtilen parametrelerle Data Factory etkinlik pencerelerini okur. |
> | Eylem | Microsoft. DataFactory/DataFactory/datahatlarının/Activities/activitywindows/Read | Belirtilen parametrelerle işlem hattı etkinliği için etkinlik pencerelerini okur. |
> | Eylem | Microsoft. DataFactory/DataFactory/datahatlarının/activitywindows/Read | Belirtilen parametrelere sahip işlem hattı için etkinlik pencerelerini okur. |
> | Eylem | Microsoft. DataFactory/DataFactory/datahatlarının/Delete | Herhangi bir işlem hattını siler. |
> | Eylem | Microsoft. DataFactory/DataFactory/datahatlarını al/Duraklat/eylem | Tüm işlem hatlarını duraklatır. |
> | Eylem | Microsoft. DataFactory/DataFactory/datahatlarının/Read | Herhangi bir işlem hattını okur. |
> | Eylem | Microsoft. DataFactory/DataFactory/datahatlarının/CV/Action | Tüm işlem hatlarını sürdürür. |
> | Eylem | Microsoft. DataFactory/DataFactory/datahatlarının/Update/Action | Tüm işlem hatlarını güncelleştirir. |
> | Eylem | Microsoft. DataFactory/DataFactory/datahatlarının/Write | Herhangi bir işlem hattı oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. DataFactory/DataFactory/DataSet/activitywindows/Read | Belirtilen parametrelere sahip veri kümesi için etkinlik pencerelerini okur. |
> | Eylem | Microsoft. DataFactory/DataFactory/veri kümeleri/silme | Herhangi bir veri kümesini siler. |
> | Eylem | Microsoft. DataFactory/DataFactory/veri kümeleri/okuma | Tüm veri kümelerini okur. |
> | Eylem | Microsoft. DataFactory/DataFactory/DataSet/dilimleyicerbir/Read | Verilen veri kümesi için verilen başlangıç zamanına sahip veri dilimi çalıştırmasını okur. |
> | Eylem | Microsoft. DataFactory/DataFactory/veri kümeleri/dilimler/okuma | Verilen dönemdeki veri dilimlerini alır. |
> | Eylem | Microsoft. DataFactory/DataFactory/veri kümeleri/dilimler/Write | Veri diliminin durumunu güncelleştirin. |
> | Eylem | Microsoft. DataFactory/DataFactory/veri kümeleri/yazma | Herhangi bir veri kümesini oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. DataFactory/DataFactory/Delete | Data Factory siler. |
> | Eylem | Microsoft. DataFactory/DataFactory/Gateway/ConnectionInfo/Action | Herhangi bir ağ geçidi için bağlantı bilgilerini okur. |
> | Eylem | Microsoft. DataFactory/DataFactory/Gateway/Delete | Tüm ağ geçidini siler. |
> | Eylem | Microsoft. DataFactory/DataFactory/Gateway/listauthkeys/Action | Herhangi bir ağ geçidinin kimlik doğrulama anahtarlarını listeler. |
> | Eylem | Microsoft. DataFactory/DataFactory/Gateway/Read | Herhangi bir ağ geçidini okur. |
> | Eylem | Microsoft. DataFactory/DataFactory/Gateway/regenerateauthkey/Action | Herhangi bir ağ geçidinin kimlik doğrulama anahtarlarını yeniden oluşturur. |
> | Eylem | Microsoft. DataFactory/DataFactory/Gateway/Write | Herhangi bir ağ geçidini oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. DataFactory/DataFactory/linkedServices/Delete | Bağlı hizmeti siler. |
> | Eylem | Microsoft. DataFactory/DataFactory/linkedServices/Read | Herhangi bir bağlı hizmeti okur. |
> | Eylem | Microsoft. DataFactory/DataFactory/linkedServices/Write | Herhangi bir bağlı hizmeti oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. DataFactory/DataFactory/okuma | Data Factory okur. |
> | Eylem | Microsoft. DataFactory/DataFactory/çalıştırmalar/LogInfo/Read | Günlükleri içeren bir blob kapsayıcısına SAS URI 'sini okur. |
> | Eylem | Microsoft. DataFactory/DataFactory/tablolar/Sil | Herhangi bir veri kümesini siler. |
> | Eylem | Microsoft. DataFactory/DataFactory/tablolar/okuma | Tüm veri kümelerini okur. |
> | Eylem | Microsoft. DataFactory/DataFactory/Tables/Write | Herhangi bir veri kümesini oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. DataFactory/DataFactory/Write | Data Factory oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. DataFactory/Factory/addDataFlowToDebugSession/Action | Önizleme için hata ayıklama oturumuna veri akışı ekleyin. |
> | Eylem | Microsoft. DataFactory/Factory/cancelardışık düzen eylemsizlik/eylem | Çalıştırma KIMLIĞI tarafından belirtilen işlem hattı çalıştırmasını iptal eder. |
> | Eylem | Microsoft. DataFactory/Factory/Cancelsandboxardışık düzen eylemsizlik/eylem | İşlem hattı için bir hata ayıklama çalıştırmasını iptal eder. |
> | Eylem | Microsoft. DataFactory/Factory/createdataflowdebugsession/Action | Bir veri akışı hata ayıklama oturumu oluşturur. |
> | Eylem | Microsoft. DataFactory/Factory/dataakışlar/Delete | Veri akışını siler. |
> | Eylem | Microsoft. DataFactory/Factory/dataakışlar/okuma | Veri akışını okur. |
> | Eylem | Microsoft. DataFactory/Factory/dataakışlar/Write | Veri akışı oluştur veya güncelleştir |
> | Eylem | Microsoft. DataFactory/Factory/veri kümeleri/silme | Herhangi bir veri kümesini siler. |
> | Eylem | Microsoft. DataFactory/Factory/veri kümeleri/okuma | Tüm veri kümelerini okur. |
> | Eylem | Microsoft. DataFactory/Factory/veri kümeleri/yazma | Herhangi bir veri kümesini oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. DataFactory/Factory/debugardışık düzen eylemsizlik/iptal/eylem | İşlem hattı için bir hata ayıklama çalıştırmasını iptal eder. |
> | Eylem | Microsoft. DataFactory/Factory/Delete | Data Factory siler. |
> | Eylem | Microsoft. DataFactory/Factory/deletedataflowdebugsession/Action | Bir veri akışı hata ayıklama oturumunu siler. |
> | Eylem | Microsoft. DataFactory/Factory/executeDataFlowDebugCommand/Action | Veri akışı hata ayıklama komutunu yürütün. |
> | Eylem | Microsoft. DataFactory/Factory/getDataPlaneAccess/Action | ADF Datadüzlemi hizmetine erişimi alır. |
> | Eylem | Microsoft. DataFactory/Factory/getDataPlaneAccess/Read | ADF Datadüzlemi hizmetine erişimi okur. |
> | Eylem | Microsoft. DataFactory/Factory/getFeatureValue/Action | Belirli bir konum için pozlama denetim özelliği değeri alın. |
> | Eylem | Microsoft. DataFactory/Factory/getFeatureValue/Read | Belirli bir konum için pozlama denetimi özellik değerini okur. |
> | Eylem | Microsoft. DataFactory/Factory/getGitHubAccessToken/eylem | GitHub erişim belirtecini alır. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/silme | Tüm Integration Runtime siler. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationsms/getconnectionınfo/Read | Integration Runtime bağlantı bilgilerini okur. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/getObjectMetadata/eylem | Belirtilen Integration Runtime için SSIS Integration Runtime meta verilerini alın. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/GetStatus/Read | Integration Runtime durumunu okur. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/linkedIntegrationRuntime/Action | Belirtilen paylaşılan Integration Runtime bağlı Integration Runtime başvurusu oluştur. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/listauthkeys/Read | Herhangi bir Integration Runtime için kimlik doğrulama anahtarlarını listeler. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/monitoringdata/Read | Integration Runtime için Izleme verilerini alır. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/düğümleri/silme | Belirtilen Integration Runtime düğümünü siler. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/düğümler/IPAddress/Action | Integration Runtime belirtilen düğümü için IP adresini döndürür. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/düğümleri/okuma | Belirtilen Integration Runtime için düğümü okur. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/düğümleri/yazma | Şirket içinde barındırılan Integration Runtime düğümünü güncelleştirir. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/okuma | Integration Runtime okur. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/refreshObjectMetadata/Action | Belirtilen Integration Runtime için SSIS Integration Runtime meta verilerini yenile. |
> | Eylem | Microsoft. DataFactory/Factory/Integration, çalışma zamanları/regenerateauthkey/Action | Belirtilen Integration Runtime için kimlik doğrulama anahtarlarını yeniden oluşturur. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/removelmürekkepler/eylem | Belirtilen Integration Runtime bağlı Integration Runtime başvurularını kaldırır. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/Başlat/eylem | Integration Runtime başlatır. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/durdur/eylem | Integration Runtime sonlandırır. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/synccredentials/Action | Belirtilen Integration Runtime ait kimlik bilgilerini eşitler. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/yükseltme/eylem | Belirtilen Integration Runtime yükseltir. |
> | Eylem | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/yazma | Integration Runtime oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. DataFactory/Factory/linkedServices/Delete | Bağlı hizmeti siler. |
> | Eylem | Microsoft. DataFactory/Factory/linkedServices/Read | Bağlı hizmeti okur. |
> | Eylem | Microsoft. DataFactory/Factory/linkedServices/Write | Bağlı hizmet oluştur veya güncelleştir |
> | Eylem | Microsoft. DataFactory/Factory/operationResults/Read | İşlem sonuçlarını alır. |
> | Eylem | Microsoft. DataFactory/Factory/ardışık düzen eylemsizlik/activityçalıştırmaları/okuma | Belirtilen işlem hattı çalıştırma KIMLIĞI için etkinlik çalıştırmalarını okur. |
> | Eylem | Microsoft. DataFactory/Factory/ardışık düzen eylemsizlik/iptal/eylem | Çalıştırma KIMLIĞI tarafından belirtilen işlem hattı çalıştırmasını iptal eder. |
> | Eylem | Microsoft. DataFactory/Factory/ardışık düzen eylemsizlik/queryactivityçalıştırmaları/eylemi | Belirtilen işlem hattı çalıştırma KIMLIĞI için etkinliğin çalıştığını sorgular. |
> | Eylem | Microsoft. DataFactory/Factory/ardışık düzen eylemsizlik/queryactivityçalıştırmaları/okumak | Belirtilen işlem hattı çalıştırma KIMLIĞI için sorgu etkinliği çalışmalarının sonucunu okur. |
> | Eylem | Microsoft. DataFactory/Factory/ardışık düzen eylemsizlik/okunan | İşlem hattı çalıştırmalarını okur. |
> | Eylem | Microsoft. DataFactory/Factory/işlem hatları/createrun/eylem | İşlem hattı için bir çalıştırma oluşturur. |
> | Eylem | Microsoft. DataFactory/Factory/işlem hatları/silme | Ardışık düzeni siler. |
> | Eylem | Microsoft. DataFactory/Factory/işlem hatları/ardışık düzen eylemsizlik/activityçalıştırmaları/ilerlemesi/okuma | Etkinlik çalıştırmalarının Ilerlemesini alır. |
> | Eylem | Microsoft. DataFactory/Factory/işlem hatları/ardışık düzen eylemsizlik/Read | İşlem hattı çalıştırmasını okur. |
> | Eylem | Microsoft. DataFactory/Factory/işlem hatları/okuma | İşlem hattını okur. |
> | Eylem | Microsoft. DataFactory/Factory/işlem hatları/Sandbox/eylem | İşlem hattı için bir hata ayıklama çalıştırması ortamı oluşturur. |
> | Eylem | Microsoft. DataFactory/Factory/işlem hatları/Sandbox/oluşturma/eylem | İşlem hattı için bir hata ayıklama çalıştırması ortamı oluşturur. |
> | Eylem | Microsoft. DataFactory/Factory/işlem hatları/Sandbox/Çalıştır/eylem | İşlem hattı için bir hata ayıklama çalıştırması oluşturur. |
> | Eylem | Microsoft. DataFactory/Factory/işlem hatları/yazma | İşlem hattı oluştur veya güncelleştir |
> | Eylem | Microsoft. DataFactory/Factory/querydataflowdebugsessions/Action | Bir veri akışı hata ayıklama oturumu sorgular. |
> | Eylem | Microsoft. DataFactory/Factory/querydebugardışık düzen eylemsizlik/Action | Hata ayıklama işlem hattının çalıştığını sorgular. |
> | Eylem | Microsoft. DataFactory/Factory/querypipeleylemsizlik/Action | İşlem hattının çalıştırmalarını sorgular. |
> | Eylem | Microsoft. DataFactory/Factory/querypipeleylemsizlik/Read | Sorgu işlem hattı çalıştırmalarının sonucunu okur. |
> | Eylem | Microsoft. DataFactory/Factory/querytriggerçalıştırmaları/eylemi | Tetikleyici çalıştırmalarını sorgular. |
> | Eylem | Microsoft. DataFactory/Factory/querytriggerçalıştırmaları/okuma | Tetikleyici çalıştırmalarının sonucunu okur. |
> | Eylem | Microsoft. DataFactory/Factory/okuma | Data Factory okur. |
> | Eylem | Microsoft. DataFactory/Factory/sandboxardışık düzen eylemsizlik/eylem | Hata ayıklama işlem hattının çalıştığını sorgular. |
> | Eylem | Microsoft. DataFactory/Factory/sandboxardışık düzen eylemsizlik/Read | İşlem hattının hata ayıklama çalıştırma bilgisini alır. |
> | Eylem | Microsoft. DataFactory/Factory/sandboxardışık düzen eylemsizlik/Sandboxactivityçalıştırmaları/Read | Etkinliğin hata ayıklama çalıştırma bilgisini alır. |
> | Eylem | Microsoft. DataFactory/Factory/startdataflowdebugsession/Action | Bir veri akışı hata ayıklama oturumu başlatır. |
> | Eylem | Microsoft. DataFactory/Factory/triggerçalıştırmaları/okuma | Tetikleyici çalıştırmalarını okur. |
> | Eylem | Microsoft. DataFactory/Factory/Tetikleyiciler/silme | Tüm tetikleyiciyi siler. |
> | Eylem | Microsoft. DataFactory/Factory/Tetikleyiciler/geteventsubscriptionstatus/Action | Olay aboneliği durumu. |
> | Eylem | Microsoft. DataFactory/Factory/Tetikleyiciler/okuma | Herhangi bir tetikleyiciyi okur. |
> | Eylem | Microsoft. DataFactory/Factory/Tetikleyiciler/Başlat/eylem | Herhangi bir tetikleyiciyi başlatır. |
> | Eylem | Microsoft. DataFactory/Factory/Tetikleyiciler/durdur/eylem | Tüm tetikleyiciyi sonlandırır. |
> | Eylem | Microsoft. DataFactory/Factory/Triggers/SubscribeToEvents/Action | Olaylara abone olun. |
> | Eylem | Microsoft. DataFactory/Factory/Triggers/triggerçalıştırmaları/Read | Tetikleyici çalıştırmalarını okur. |
> | Eylem | Microsoft. DataFactory/Factory/Triggers/UnsubscribeFromEvents/Action | Etkinliklerden abonelik kaldırma. |
> | Eylem | Microsoft. DataFactory/Factory/Tetikleyiciler/Write | Herhangi bir tetikleyiciyi oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. DataFactory/Factory/Write | Data Factory oluşturun veya güncelleştirin |
> | Eylem | Microsoft. DataFactory/Locations/configureFactoryRepo/Action | Fabrika için depoyu yapılandırır. |
> | Eylem | Microsoft. DataFactory/Locations/getFeatureValue/Action | Belirli bir konum için pozlama denetim özelliği değeri alın. |
> | Eylem | Microsoft. DataFactory/Locations/getFeatureValue/Read | Belirli bir konum için pozlama denetimi özellik değerini okur. |
> | Eylem | Microsoft. DataFactory/işlemler/okuma | Microsoft Data Factory Provider 'daki tüm Işlemleri okur. |
> | Eylem | Microsoft. DataFactory/Register/ACTION | Data Factory kaynak sağlayıcısı için aboneliği kaydeder. |
> | Eylem | Microsoft. DataFactory/Unregister/eylem | Data Factory kaynak sağlayıcısı için aboneliğin kaydını siler. |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. DataLakeAnalytics/accounts/computePolicies/Delete | İşlem ilkesini silin. |
> | Eylem | Microsoft. DataLakeAnalytics/accounts/computePolicies/Read | Bir işlem ilkesi hakkında bilgi alın. |
> | Eylem | Microsoft. DataLakeAnalytics/accounts/computePolicies/Write | Bir işlem ilkesi oluşturun veya güncelleştirin. |
> | Eylem | Microsoft. DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | Bir DataLakeStore hesabının bir DataLakeAnalytics hesabıyla bağlantısını kaldırın. |
> | Eylem | Microsoft. DataLakeAnalytics/accounts/dataLakeStoreAccounts/Read | Bir DataLakeAnalytics hesabının bağlı DataLakeStore hesabı hakkında bilgi alın. |
> | Eylem | Microsoft. DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | Bir DataLakeAnalytics hesabının bağlı DataLakeStore hesabını oluşturun veya güncelleştirin. |
> | Eylem | Microsoft. DataLakeAnalytics/hesaplar/Sil | Bir DataLakeAnalytics hesabını silin. |
> | Eylem | Microsoft. DataLakeAnalytics/accounts/firewallRules/Delete | Bir güvenlik duvarı kuralını silin. |
> | Eylem | Microsoft. DataLakeAnalytics/accounts/firewallRules/Read | Bir güvenlik duvarı kuralı hakkında bilgi alın. |
> | Eylem | Microsoft. DataLakeAnalytics/accounts/firewallRules/Write | Bir güvenlik duvarı kuralı oluşturun veya güncelleştirin. |
> | Eylem | Microsoft. DataLakeAnalytics/hesaplar/operationResults/Read | Bir DataLakeAnalytics hesap işleminin sonucunu elde edin. |
> | Eylem | Microsoft. DataLakeAnalytics/hesaplar/okuma | Mevcut bir DataLakeAnalytics hesabı hakkında bilgi alın. |
> | Eylem | Microsoft. DataLakeAnalytics/hesaplar/storageAccounts/kapsayıcılar/listSasTokens/Action | Bir DataLakeAnalytics hesabının bağlı depolama hesabının depolama kapsayıcıları için SAS belirteçlerini listeleyin. |
> | Eylem | Microsoft. DataLakeAnalytics/hesaplar/storageAccounts/kapsayıcılar/okuma | Bir DataLakeAnalytics hesabının bağlı depolama hesabının kapsayıcılarını alın. |
> | Eylem | Microsoft. DataLakeAnalytics/hesaplar/storageAccounts/Delete | Bir depolama hesabının bir DataLakeAnalytics hesabından bağlantısını kaldırın. |
> | Eylem | Microsoft. DataLakeAnalytics/hesaplar/storageAccounts/Read | Bir DataLakeAnalytics hesabının bağlı depolama hesabı hakkında bilgi alın. |
> | Eylem | Microsoft. DataLakeAnalytics/hesaplar/storageAccounts/Write | Bir DataLakeAnalytics hesabı için bağlı bir depolama hesabı oluşturun veya güncelleştirin. |
> | Eylem | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Diğer kullanıcılar tarafından gönderilen işleri iptal etmek için izin verin. |
> | Eylem | Microsoft. DataLakeAnalytics/accounts/Transferanalizticsunonun/eylemi | Systemmaxanalizticsunkendisini DataLakeAnalytics hesapları arasında aktarın. |
> | Eylem | Microsoft. DataLakeAnalytics/accounts/virtualNetworkRules/Delete | Bir sanal ağ kuralını silin. |
> | Eylem | Microsoft. DataLakeAnalytics/accounts/virtualNetworkRules/Read | Bir sanal ağ kuralı hakkında bilgi alın. |
> | Eylem | Microsoft. DataLakeAnalytics/accounts/virtualNetworkRules/Write | Bir sanal ağ kuralı oluşturun veya güncelleştirin. |
> | Eylem | Microsoft. DataLakeAnalytics/hesaplar/yazma | Bir DataLakeAnalytics hesabı oluşturun veya güncelleştirin. |
> | Eylem | Microsoft. DataLakeAnalytics/konumlar/yetenek/okuma | DataLakeAnalytics kullanımıyla ilgili bir aboneliğin yetenek bilgilerini alın. |
> | Eylem | Microsoft. DataLakeAnalytics/konumlar/Checknameavaılabılıty/Action | Bir DataLakeAnalytics hesap adının kullanılabilirliğini denetleyin. |
> | Eylem | Microsoft. DataLakeAnalytics/konumlar/operationResults/Read | Bir DataLakeAnalytics hesap işleminin sonucunu elde edin. |
> | Eylem | Microsoft. DataLakeAnalytics/konumlar/kullanımlar/okuma | DataLakeAnalytics kullanımıyla ilgili bir aboneliğin kota kullanımları bilgilerini alın. |
> | Eylem | Microsoft. DataLakeAnalytics/işlemler/okuma | DataLakeAnalytics 'in kullanılabilir işlemlerini alın. |
> | Eylem | Microsoft. DataLakeAnalytics/Register/Action | Aboneliği DataLakeAnalytics 'e kaydedin. |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. DataLakeStore/hesaplar/Sil | Bir DataLakeStore hesabını silin. |
> | Eylem | Microsoft. DataLakeStore/accounts/Enablekeykasası/eylem | DataLakeStore hesabı için Anahtar Kasası 'nı etkinleştirin. |
> | Eylem | Microsoft. DataLakeStore/accounts/eventGridFilters/Delete | Bir EventGrid filtresini silin. |
> | Eylem | Microsoft. DataLakeStore/accounts/Eventgridfilter/Read | Bir EventGrid filtresi alın. |
> | Eylem | Microsoft. DataLakeStore/accounts/Eventgridfilter/Write | Bir EventGrid filtresi oluşturun veya güncelleştirin. |
> | Eylem | Microsoft. DataLakeStore/accounts/firewallRules/Delete | Bir güvenlik duvarı kuralını silin. |
> | Eylem | Microsoft. DataLakeStore/accounts/firewallRules/Read | Bir güvenlik duvarı kuralı hakkında bilgi alın. |
> | Eylem | Microsoft. DataLakeStore/accounts/firewallRules/Write | Bir güvenlik duvarı kuralı oluşturun veya güncelleştirin. |
> | Eylem | Microsoft. DataLakeStore/accounts/operationResults/Read | DataLakeStore hesabı işleminin sonucunu elde edin. |
> | Eylem | Microsoft. DataLakeStore/hesaplar/okuma | Mevcut bir DataLakeStore hesabı hakkında bilgi alın. |
> | Eylem | Microsoft. DataLakeStore/hesaplar/paylaşımlar/Sil | Bir paylaşma silin. |
> | Eylem | Microsoft. DataLakeStore/hesaplar/paylaşımlar/okuma | Bir paylaşma hakkında bilgi alın. |
> | Eylem | Microsoft. DataLakeStore/hesaplar/paylaşımlar/yaz | Bir paylaşma oluşturun veya güncelleştirin. |
> | Eylem | Microsoft.DataLakeStore/accounts/Superuser/action | Microsoft. Authorization/Roleatamalar/Write ile verildiğinde Süper Kullanıcı Data Lake Store verin. |
> | Eylem | Microsoft. DataLakeStore/accounts/trustedIdProviders/Delete | Güvenilen bir kimlik sağlayıcısını silin. |
> | Eylem | Microsoft. DataLakeStore/accounts/trustedIdProviders/Read | Güvenilen bir kimlik sağlayıcısı hakkında bilgi alın. |
> | Eylem | Microsoft. DataLakeStore/accounts/trustedIdProviders/Write | Güvenilen bir kimlik sağlayıcısı oluşturun veya güncelleştirin. |
> | Eylem | Microsoft. DataLakeStore/accounts/virtualNetworkRules/Delete | Bir sanal ağ kuralını silin. |
> | Eylem | Microsoft. DataLakeStore/accounts/virtualNetworkRules/Read | Bir sanal ağ kuralı hakkında bilgi alın. |
> | Eylem | Microsoft. DataLakeStore/accounts/virtualNetworkRules/Write | Bir sanal ağ kuralı oluşturun veya güncelleştirin. |
> | Eylem | Microsoft. DataLakeStore/hesaplar/yazma | Bir DataLakeStore hesabı oluşturun veya güncelleştirin. |
> | Eylem | Microsoft. DataLakeStore/konumlar/yetenek/okuma | DataLakeStore kullanımı ile ilgili bir aboneliğin yetenek bilgilerini alın. |
> | Eylem | Microsoft. DataLakeStore/Locations/Checknameavaılabılıty/Action | DataLakeStore hesap adının kullanılabilirliğini kontrol edin. |
> | Eylem | Microsoft. DataLakeStore/konumlar/operationResults/Read | DataLakeStore hesabı işleminin sonucunu elde edin. |
> | Eylem | Microsoft. DataLakeStore/konumlar/kullanımlar/okuma | DataLakeStore kullanımıyla ilgili bir aboneliğin kota kullanımları bilgilerini alın. |
> | Eylem | Microsoft. DataLakeStore/işlemler/okuma | DataLakeStore 'in kullanılabilir işlemlerini alın. |
> | Eylem | Microsoft. DataLakeStore/Register/ACTION | Aboneliği DataLakeStore 'ye kaydedin. |

## <a name="microsoftdatamigration"></a>Microsoft. DataMigration

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. DataMigration/konumlar/operationResults/Read | 202 kabul edilen bir Yanıtla ilgili uzun süreli bir işlemin durumunu alın |
> | Eylem | Microsoft. DataMigration/Locations/Operationdurumlarının/Read | 202 kabul edilen bir Yanıtla ilgili uzun süreli bir işlemin durumunu alın |
> | Eylem | Microsoft. DataMigration/Register/Action | Aboneliği Azure veritabanı geçiş hizmeti sağlayıcısına kaydeder |
> | Eylem | Microsoft. DataMigration/Services/addWorker/Action | Hizmetin kullanılabilir çalışanlarına bir DMS çalışanı ekler |
> | Eylem | Microsoft. DataMigration/hizmetler/checkStatus/Action | Hizmetin dağıtılıp dağıtılmadığını ve çalışıp çalışmadığını denetleyin |
> | Eylem | Microsoft. DataMigration/Services/configureWorker/Action | Bir DMS çalışanını hizmetin kullanılabilir çalışanları için yapılandırır |
> | Eylem | Microsoft. DataMigration/hizmetler/Sil | Bir kaynağı ve tüm alt öğelerini siler |
> | Eylem | Microsoft. DataMigration/Services/Projects/accessArtifacts/Action | Proje yapıtları almak veya koymak için kullanılabilecek bir URL Oluştur |
> | Eylem | Microsoft. DataMigration/hizmetler/projeler/Sil | Bir kaynağı ve tüm alt öğelerini siler |
> | Eylem | Microsoft. DataMigration/Services/Projects/Files/Delete | Bir kaynağı ve tüm alt öğelerini siler |
> | Eylem | Microsoft. DataMigration/Services/Projects/Files/Read | Kaynaklarla ilgili bilgileri okuyun |
> | Eylem | Microsoft. DataMigration/Services/Projects/Files/Read/action | Dosyanın içeriğini okumak için kullanılabilecek bir URL alın |
> | Eylem | Microsoft. DataMigration/Services/Projects/Files/readWrite/Action | Dosyanın içeriğini okumak veya yazmak için kullanılabilecek bir URL alın |
> | Eylem | Microsoft. DataMigration/Services/Projects/Files/Write | Kaynakları ve bunların özelliklerini oluşturun veya güncelleştirin |
> | Eylem | Microsoft. DataMigration/hizmetler/projeler/okuma | Kaynaklarla ilgili bilgileri okuyun |
> | Eylem | Microsoft. DataMigration/hizmetler/projeler/görevler/iptal/eylem | Şu anda çalışıyorsa, görevi iptal et |
> | Eylem | Microsoft. DataMigration/hizmetler/projeler/görevler/Sil | Bir kaynağı ve tüm alt öğelerini siler |
> | Eylem | Microsoft. DataMigration/hizmetler/projeler/görevler/okuma | Kaynaklarla ilgili bilgileri okuyun |
> | Eylem | Microsoft. DataMigration/hizmetler/projeler/görevler/yazma | Görevleri çalıştırma Azure veritabanı geçiş hizmeti görevleri |
> | Eylem | Microsoft. DataMigration/hizmetler/projeler/yazma | Görevleri çalıştırma Azure veritabanı geçiş hizmeti görevleri |
> | Eylem | Microsoft. DataMigration/Services/Read | Kaynaklarla ilgili bilgileri okuyun |
> | Eylem | Microsoft. DataMigration/Services/removeWorker/Action | Bir DMS çalışanını hizmetin kullanılabilir çalışanlarına kaldırır |
> | Eylem | Microsoft. DataMigration/Services/serviceTasks/Cancel/Action | Şu anda çalışıyorsa, görevi iptal et |
> | Eylem | Microsoft. DataMigration/Services/serviceTasks/Delete | Bir kaynağı ve tüm alt öğelerini siler |
> | Eylem | Microsoft. DataMigration/Services/serviceTasks/Read | Kaynaklarla ilgili bilgileri okuyun |
> | Eylem | Microsoft. DataMigration/Services/serviceTasks/Write | Görevleri çalıştırma Azure veritabanı geçiş hizmeti görevleri |
> | Eylem | Microsoft. DataMigration/hizmetler/yuvalar/silme | Bir kaynağı ve tüm alt öğelerini siler |
> | Eylem | Microsoft. DataMigration/hizmetler/yuvalar/okuma | Kaynaklarla ilgili bilgileri okuyun |
> | Eylem | Microsoft. DataMigration/hizmetler/yuvalar/yazma | Kaynakları ve bunların özelliklerini oluşturun veya güncelleştirin |
> | Eylem | Microsoft. DataMigration/hizmetler/Başlat/eylem | Bu hizmetin geçişleri yeniden işlemesini sağlamak için DMS hizmetini başlatın |
> | Eylem | Microsoft. DataMigration/hizmetler/durdur/eylem | Kendi maliyetini en aza indirmek için DMS hizmetini durdurun |
> | Eylem | Microsoft. DataMigration/Services/updateAgentConfig/Action | DMS Aracısı yapılandırmasını belirtilen değerlerle güncelleştirir. |
> | Eylem | Microsoft. DataMigration/hizmetler/Write | Kaynakları ve bunların özelliklerini oluşturun veya güncelleştirin |
> | Eylem | Microsoft. DataMigration/SKU 'lar/okuma | DMS kaynakları tarafından desteklenen SKU 'ların bir listesini alın. |

## <a name="microsoftdbformariadb"></a>Microsoft. Dbformarıdb

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. Dbformarıdb/Checknameavaılabılıty/Action | Verify whether given server name is available for provisioning worldwide for a given subscription. |
> | Eylem | Microsoft. Dbformarıdb/konumlar/azureAsyncOperation/Read | MariaDB sunucusu Işlem sonuçlarını döndür |
> | Eylem | Microsoft. Dbformarıdb/konumlar/operationResults/Read | ResourceGroup tabanlı MariaDB sunucusu Işlem sonuçlarını döndür |
> | Eylem | Microsoft. Dbformarıdb/konumlar/operationResults/Read | MariaDB sunucusu Işlem sonuçlarını döndür |
> | Eylem | Microsoft. Dbformarıdb/konumlar/Performancekatmanları/okuma | Kullanılabilir performans katmanları listesini döndürür. |
> | Eylem | Microsoft. Dbformarıdb/konumlar/privateEndpointConnectionAzureAsyncOperation/Read | Gets the result for a private endpoint connection operation |
> | Eylem | Microsoft. Dbformarıdb/konumlar/privateEndpointConnectionOperationResults/Read | Gets the result for a private endpoint connection operation |
> | Eylem | Microsoft. Dbformarıdb/konumlar/privateEndpointConnectionProxyAzureAsyncOperation/Read | Gets the result for a private endpoint connection proxy operation |
> | Eylem | Microsoft. Dbformarıdb/konumlar/privateEndpointConnectionProxyOperationResults/Read | Gets the result for a private endpoint connection proxy operation |
> | Eylem | Microsoft. Dbformarıdb/konumlar/securityAlertPoliciesAzureAsyncOperation/Read | Sunucu tehdit algılama işlemi sonucunun listesini döndürür. |
> | Eylem | Microsoft. Dbformarıdb/konumlar/Securityalcertpoliciesoperationresults/Read | Sunucu tehdit algılama işlemi sonucunun listesini döndürür. |
> | Eylem | Microsoft. Dbformarıdb/konumlar/serverKeyAzureAsyncOperation/Read | Gets in-progress operations on transparent data encryption server keys |
> | Eylem | Microsoft. Dbformarıdb/konumlar/serverKeyOperationResults/Read | Gets in-progress operations on transparent data encryption server keys |
> | Eylem | Microsoft. Dbformarıdb/işlemler/okuma | MariaDB Işlemlerinin listesini döndürün. |
> | Eylem | Microsoft. Dbformarıdb/Performancekatmanları/okuma | Kullanılabilir performans katmanları listesini döndürür. |
> | Eylem | Microsoft. Dbformarıdb/Register/ACTION | MariaDB kaynak sağlayıcısını Kaydet |
> | Eylem | Microsoft. Dbformarıdb/sunucular/Yöneticiler/Sil | MariaDB sunucusunun var olan bir yöneticisini siler. |
> | Eylem | Microsoft. Dbformarıdb/sunucular/Yöneticiler/okuma | MariaDB sunucu yöneticilerinin bir listesini alır. |
> | Eylem | Microsoft. Dbformarıdb/sunucular/Yöneticiler/yazma | MariaDB sunucu yöneticisini belirtilen parametrelerle oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. Dbformarıdb/Servers/danışmanörler/Createreyorumdedadctionsession/ACTION | Yeni bir öneri eylem oturumu oluştur |
> | Eylem | Microsoft. Dbformarıdb/sunucular/danışmanları/okuma | Danışmanlarının listesini döndürün |
> | Eylem | Microsoft. Dbformarıdb/sunucular/danışmanları/okuma | Danışman döndürme |
> | Eylem | Microsoft. Dbformarıdb/sunucular/danışmanları/recommendedActions/okuma | Önerilen eylemlerin listesini döndür |
> | Eylem | Microsoft. Dbformarıdb/sunucular/danışmanları/recommendedActions/okuma | Önerilen eylemlerin listesini döndür |
> | Eylem | Microsoft. Dbformarıdb/sunucular/danışmanları/recommendedActions/okuma | Önerilen eylemi döndürür |
> | Eylem | Microsoft. Dbformarıdb/sunucular/yapılandırma/okuma | Bir sunucu için yapılandırmaların listesini döndürün veya belirtilen yapılandırma için özellikleri alır. |
> | Eylem | Microsoft. Dbformarıdb/sunucu/yapılandırma/yazma | Belirtilen yapılandırma için değeri güncelleştir |
> | Eylem | Microsoft. Dbformarıdb/sunucular/veritabanları/silme | Var olan bir MariaDB veritabanını siler. |
> | Eylem | Microsoft. Dbformarıdb/sunucular/veritabanları/okuma | MariaDB veritabanlarının listesini döndürün veya belirtilen veritabanının özelliklerini alır. |
> | Eylem | Microsoft. Dbformarıdb/sunucular/veritabanları/yazma | Belirtilen parametrelerle bir MariaDB veritabanı oluşturur veya belirtilen veritabanı için özellikleri güncelleştirir. |
> | Eylem | Microsoft. Dbformarıdb/sunucu/silme | Deletes an existing server. |
> | Eylem | Microsoft. Dbformarıdb/Servers/firewallRules/Delete | Mevcut bir güvenlik duvarı kuralını siler. |
> | Eylem | Microsoft. Dbformarıdb/Servers/firewallRules/Read | Bir sunucu için güvenlik duvarı kuralları listesini döndürün veya belirtilen güvenlik duvarı kuralının özelliklerini alır. |
> | Eylem | Microsoft. Dbformarıdb/Servers/firewallRules/Write | Belirtilen parametrelerle bir güvenlik duvarı kuralı oluşturur veya var olan bir kuralı güncelleştirir. |
> | Eylem | Microsoft. Dbformarıdb/sunucular/anahtarlar/silme | Deletes an existing server key. |
> | Eylem | Microsoft. Dbformarıdb/sunucular/anahtarlar/okuma | Return the list of server keys or gets the properties for the specified server key. |
> | Eylem | Microsoft. Dbformarıdb/sunucular/anahtarlar/yazma | Creates a key with the specified parameters or update the properties or tags for the specified server key. |
> | Eylem | Microsoft. Dbformarıdb/Servers/logFiles/Read | MariaDB LogFiles listesini döndürün. |
> | Eylem | Microsoft. Dbformarıdb/Servers/Privateendpointconnectionproxy 'Leri/silme | Deletes an existing private endpoint connection proxy |
> | Eylem | Microsoft. Dbformarıdb/Servers/Privateendpointconnectionproxy/okuma | Returns the list of private endpoint connection proxies or gets the properties for the specified private endpoint connection proxy. |
> | Eylem | Microsoft. Dbformarıdb/Servers/Privateendpointconnectionproxy/doğrulama/eylem | Validates a private endpoint connection create call from NRP side |
> | Eylem | Microsoft. Dbformarıdb/Servers/Privateendpointconnectionproxy/yazma | Creates a private endpoint connection proxy with the specified parameters or updates the properties or tags for the specified private endpoint connection proxy. |
> | Eylem | Microsoft. Dbformarıdb/Servers/privateEndpointConnections/Delete | Deletes an existing private endpoint connection |
> | Eylem | Microsoft. Dbformarıdb/Servers/privateEndpointConnections/Read | Returns the list of private endpoint connections or gets the properties for the specified private endpoint connection. |
> | Eylem | Microsoft. Dbformarıdb/Servers/privateEndpointConnections/Write | Approves or rejects an existing private endpoint connection |
> | Eylem | Microsoft. Dbformarıdb/Servers/privateLinkResources/Read | İlgili MariaDB sunucusu için özel bağlantı kaynakları alın |
> | Eylem | Microsoft. Dbformarıdb/Servers/Providers/Microsoft. Insights/diagnosticSettings/Read | Kaynak için ayrılan ayarı alır |
> | Eylem | Microsoft. Dbformarıdb/Servers/Providers/Microsoft. Insights/diagnosticSettings/Write | Creates or updates the diagnostic setting for the resource |
> | Eylem | Microsoft. Dbformarıdb/Servers/Providers/Microsoft. Insights/logDefinitions/Read | MariaDB sunucuları için kullanılabilir günlükleri alır |
> | Eylem | Microsoft. Dbformarıdb/Servers/Providers/Microsoft. Insights/metricDefinitions/Read | Return types of metrics that are available for databases |
> | Eylem | Microsoft. Dbformarıdb/Servers/Querymetinler/eylem | Sorgu listesi için metin döndürme |
> | Eylem | Microsoft. Dbformarıdb/Servers/Querymetinler/eylem | Bir sorgunun metnini döndürme |
> | Eylem | Microsoft. Dbformarıdb/sunucular/okuma | Return the list of servers or gets the properties for the specified server. |
> | Eylem | Microsoft. Dbformarıdb/Servers/recoverableServers/Read | Kurtarılabilir MariaDB sunucu bilgilerini döndürün |
> | Eylem | Microsoft. Dbformarıdb/sunucular/çoğaltmalar/okuma | MariaDB sunucusunun okuma çoğaltmalarını al |
> | Eylem | Microsoft. Dbformarıdb/sunucular/yeniden Başlat/eylem | Belirli bir sunucuyu yeniden başlatır. |
> | Eylem | Microsoft. Dbformarıdb/Servers/Securityalcertpolicies/Read | Belirli bir sunucuda yapılandırılmış sunucu tehdit algılama ilkesinin ayrıntılarını alma |
> | Eylem | Microsoft. Dbformarıdb/Servers/Securityalcertpolicies/Write | Change the server threat detection policy for a given server |
> | Eylem | Microsoft. Dbformarıdb/Servers/Topquerystatıstıcs/Read | En üstteki sorgular için sorgu Istatistikleri listesini döndürür. |
> | Eylem | Microsoft. Dbformarıdb/Servers/Topquerystatıstıcs/Read | Sorgu Istatistiği döndürme |
> | Eylem | Microsoft. Dbformarıdb/Servers/updateConfigurations/Action | Belirtilen sunucu için Güncelleştirme yapılandırması |
> | Eylem | Microsoft. Dbformarıdb/Servers/virtualNetworkRules/Delete | Deletes an existing Virtual Network Rule |
> | Eylem | Microsoft. Dbformarıdb/Servers/virtualNetworkRules/Read | Return the list of virtual network rules or gets the properties for the specified virtual network rule. |
> | Eylem | Microsoft. Dbformarıdb/Servers/virtualNetworkRules/Write | Creates a virtual network rule with the specified parameters or update the properties or tags for the specified virtual network rule. |
> | Eylem | Microsoft. Dbformarıdb/sunucular/waitStatistics/okuma | Örnek için bekleme istatistiklerini döndür |
> | Eylem | Microsoft. Dbformarıdb/sunucular/waitStatistics/okuma | Bekleme istatistiği döndürme |
> | Eylem | Microsoft. Dbformarıdb/sunucu/yazma | Creates a server with the specified parameters or update the properties or tags for the specified server. |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. Dbformyısql/Checknameavaılabılıty/Action | Verify whether given server name is available for provisioning worldwide for a given subscription. |
> | Eylem | Microsoft. Dbformyısql/konumlar/azureAsyncOperation/Read | MySQL sunucusu Işlem sonuçlarını döndür |
> | Eylem | Microsoft. Dbformyısql/konumlar/operationResults/Read | ResourceGroup tabanlı MySQL Server Işlem sonuçlarını döndür |
> | Eylem | Microsoft. Dbformyısql/konumlar/operationResults/Read | MySQL sunucusu Işlem sonuçlarını döndür |
> | Eylem | Microsoft. Dbformyısql/konumlar/Performancekatmanları/okuma | Kullanılabilir performans katmanları listesini döndürür. |
> | Eylem | Microsoft. Dbformyısql/konumlar/privateEndpointConnectionAzureAsyncOperation/Read | Gets the result for a private endpoint connection operation |
> | Eylem | Microsoft. Dbformyısql/konumlar/privateEndpointConnectionOperationResults/Read | Gets the result for a private endpoint connection operation |
> | Eylem | Microsoft. Dbformyısql/konumlar/privateEndpointConnectionProxyAzureAsyncOperation/Read | Gets the result for a private endpoint connection proxy operation |
> | Eylem | Microsoft. Dbformyısql/konumlar/privateEndpointConnectionProxyOperationResults/Read | Gets the result for a private endpoint connection proxy operation |
> | Eylem | Microsoft. Dbformyısql/konumlar/securityAlertPoliciesAzureAsyncOperation/Read | Sunucu tehdit algılama işlemi sonucunun listesini döndürür. |
> | Eylem | Microsoft. Dbformyısql/konumlar/Securityalcertpoliciesoperationresults/Read | Sunucu tehdit algılama işlemi sonucunun listesini döndürür. |
> | Eylem | Microsoft. Dbformyısql/konumlar/Sunucukeyazureasyncoperation/Read | Gets in-progress operations on transparent data encryption server keys |
> | Eylem | Microsoft. Dbformyısql/konumlar/serverKeyOperationResults/Read | Gets in-progress operations on transparent data encryption server keys |
> | Eylem | Microsoft. Dbformyısql/işlemler/okuma | MySQL Işlemleri listesini döndürün. |
> | Eylem | Microsoft. Dbformyısql/Performancekatmanları/okuma | Kullanılabilir performans katmanları listesini döndürür. |
> | Eylem | Microsoft. Dbformyısql/Register/Action | MySQL kaynak sağlayıcısını Kaydet |
> | Eylem | Microsoft. Dbformyısql/Servers/Administrators/Delete | MySQL Server 'ın var olan bir yöneticisini siler. |
> | Eylem | Microsoft. Dbformyısql/Servers/Administrators/Read | MySQL Server yöneticilerinin bir listesini alır. |
> | Eylem | Microsoft. Dbformyısql/Servers/Administrators/Write | Belirtilen parametrelerle MySQL Server Yöneticisi oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. Dbformyısql/Servers/danışmanları/Createreyorumdedadctionsession/ACTION | Yeni bir öneri eylem oturumu oluştur |
> | Eylem | Microsoft. Dbformyısql/Servers/danışmanları/okuma | Danışmanlarının listesini döndürün |
> | Eylem | Microsoft. Dbformyısql/Servers/danışmanları/okuma | Danışman döndürme |
> | Eylem | Microsoft. Dbformyısql/Servers/danışmanları/recommendedActions/okuma | Önerilen eylemlerin listesini döndür |
> | Eylem | Microsoft. Dbformyısql/Servers/danışmanları/recommendedActions/okuma | Önerilen eylemlerin listesini döndür |
> | Eylem | Microsoft. Dbformyısql/Servers/danışmanları/recommendedActions/okuma | Önerilen eylemi döndürür |
> | Eylem | Microsoft. Dbformyısql/sunucular/yapılandırma/okuma | Bir sunucu için yapılandırmaların listesini döndürün veya belirtilen yapılandırma için özellikleri alır. |
> | Eylem | Microsoft. Dbformyısql/sunucular/yapılandırma/yazma | Belirtilen yapılandırma için değeri güncelleştir |
> | Eylem | Microsoft. Dbformyısql/Servers/veritabanları/Delete | Mevcut bir MySQL veritabanını siler. |
> | Eylem | Microsoft. Dbformyısql/Servers/veritabanları/okuma | MySQL veritabanlarının listesini döndürün veya belirtilen veritabanının özelliklerini alır. |
> | Eylem | Microsoft. Dbformyısql/Servers/veritabanları/Write | Belirtilen parametrelerle bir MySQL veritabanı oluşturur veya belirtilen veritabanı için özellikleri güncelleştirir. |
> | Eylem | Microsoft. Dbformyısql/Servers/Delete | Deletes an existing server. |
> | Eylem | Microsoft. Dbformyısql/Servers/firewallRules/Delete | Mevcut bir güvenlik duvarı kuralını siler. |
> | Eylem | Microsoft. Dbformyısql/Servers/firewallRules/Read | Bir sunucu için güvenlik duvarı kuralları listesini döndürün veya belirtilen güvenlik duvarı kuralının özelliklerini alır. |
> | Eylem | Microsoft. Dbformyısql/Servers/firewallRules/Write | Belirtilen parametrelerle bir güvenlik duvarı kuralı oluşturur veya var olan bir kuralı güncelleştirir. |
> | Eylem | Microsoft. Dbformyısql/Servers/anahtarlar/Delete | Deletes an existing server key. |
> | Eylem | Microsoft. Dbformyısql/sunucular/anahtarlar/okuma | Return the list of server keys or gets the properties for the specified server key. |
> | Eylem | Microsoft. Dbformyısql/Servers/anahtarlar/Write | Creates a key with the specified parameters or update the properties or tags for the specified server key. |
> | Eylem | Microsoft. Dbformyısql/Servers/logFiles/Read | MySQL LogFiles listesini döndürün. |
> | Eylem | Microsoft. Dbformyısql/Servers/Privateendpointconnectionproxy 'Leri/silme | Deletes an existing private endpoint connection proxy |
> | Eylem | Microsoft. Dbformyısql/Servers/Privateendpointconnectionproxy/okuma | Returns the list of private endpoint connection proxies or gets the properties for the specified private endpoint connection proxy. |
> | Eylem | Microsoft. Dbformyısql/Servers/Privateendpointconnectionproxy/doğrulama/eylem | Validates a private endpoint connection create call from NRP side |
> | Eylem | Microsoft. Dbformyısql/Servers/Privateendpointconnectionproxy/yazma | Creates a private endpoint connection proxy with the specified parameters or updates the properties or tags for the specified private endpoint connection proxy. |
> | Eylem | Microsoft. Dbformyısql/Servers/privateEndpointConnections/Delete | Deletes an existing private endpoint connection |
> | Eylem | Microsoft. Dbformyısql/Servers/privateEndpointConnections/Read | Returns the list of private endpoint connections or gets the properties for the specified private endpoint connection. |
> | Eylem | Microsoft. Dbformyısql/Servers/privateEndpointConnections/Write | Approves or rejects an existing private endpoint connection |
> | Eylem | Microsoft. Dbformyısql/Servers/privateLinkResources/Read | İlgili MySQL sunucusu için özel bağlantı kaynakları alın |
> | Eylem | Microsoft. Dbformyısql/Servers/Providers/Microsoft. Insights/diagnosticSettings/Read | Kaynak için ayrılan ayarı alır |
> | Eylem | Microsoft. Dbformyısql/Servers/Providers/Microsoft. Insights/diagnosticSettings/Write | Creates or updates the diagnostic setting for the resource |
> | Eylem | Microsoft. Dbformyısql/Servers/Providers/Microsoft. Insights/logDefinitions/Read | MySQL sunucuları için kullanılabilir günlükleri alır |
> | Eylem | Microsoft. Dbformyısql/Servers/Providers/Microsoft. Insights/metricDefinitions/Read | Return types of metrics that are available for databases |
> | Eylem | Microsoft. Dbformyısql/Servers/Querymetin'ler/eylem | Sorgu listesi için metin döndürme |
> | Eylem | Microsoft. Dbformyısql/Servers/Querymetin'ler/eylem | Bir sorgunun metnini döndürme |
> | Eylem | Microsoft. Dbformyısql/Servers/Read | Return the list of servers or gets the properties for the specified server. |
> | Eylem | Microsoft. Dbformyısql/Servers/recoverableServers/Read | Kurtarılabilir MySQL sunucusu bilgilerini döndür |
> | Eylem | Microsoft. Dbformyısql/sunucular/çoğaltmalar/okuma | MySQL sunucusunun okuma çoğaltmalarını al |
> | Eylem | Microsoft. Dbformyısql/sunucular/yeniden Başlat/eylem | Belirli bir sunucuyu yeniden başlatır. |
> | Eylem | Microsoft. Dbformyısql/Servers/Securityalcertpolicies/Read | Belirli bir sunucuda yapılandırılmış sunucu tehdit algılama ilkesinin ayrıntılarını alma |
> | Eylem | Microsoft. Dbformyısql/Servers/Securityalcertpolicies/Write | Change the server threat detection policy for a given server |
> | Eylem | Microsoft. Dbformyısql/Servers/Topquerystatıstıcs/Read | En üstteki sorgular için sorgu Istatistikleri listesini döndürür. |
> | Eylem | Microsoft. Dbformyısql/Servers/Topquerystatıstıcs/Read | Sorgu Istatistiği döndürme |
> | Eylem | Microsoft. Dbformyısql/Servers/updateConfigurations/Action | Belirtilen sunucu için Güncelleştirme yapılandırması |
> | Eylem | Microsoft. Dbformyısql/Servers/virtualNetworkRules/Delete | Deletes an existing Virtual Network Rule |
> | Eylem | Microsoft. Dbformyısql/Servers/virtualNetworkRules/Read | Return the list of virtual network rules or gets the properties for the specified virtual network rule. |
> | Eylem | Microsoft. Dbformyısql/Servers/virtualNetworkRules/Write | Creates a virtual network rule with the specified parameters or update the properties or tags for the specified virtual network rule. |
> | Eylem | Microsoft. Dbformyısql/Servers/Waitstatıstıcs/Read | Örnek için bekleme istatistiklerini döndür |
> | Eylem | Microsoft. Dbformyısql/Servers/Waitstatıstıcs/Read | Bekleme istatistiği döndürme |
> | Eylem | Microsoft. Dbformyısql/Servers/Write | Creates a server with the specified parameters or update the properties or tags for the specified server. |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. DBforPostgreSQL/Checknameavaılabılıty/Action | Verify whether given server name is available for provisioning worldwide for a given subscription. |
> | Eylem | Microsoft. DBforPostgreSQL/konumlar/azureAsyncOperation/Read | PostgreSQL sunucusu Işlem sonuçlarını döndür |
> | Eylem | Microsoft. DBforPostgreSQL/konumlar/operationResults/Read | ResourceGroup tabanlı PostgreSQL sunucusu Işlem sonuçlarını döndür |
> | Eylem | Microsoft. DBforPostgreSQL/konumlar/operationResults/Read | PostgreSQL sunucusu Işlem sonuçlarını döndür |
> | Eylem | Microsoft. DBforPostgreSQL/konumlar/Performancekatmanları/okuma | Kullanılabilir performans katmanları listesini döndürür. |
> | Eylem | Microsoft. DBforPostgreSQL/konumlar/privateEndpointConnectionAzureAsyncOperation/Read | Gets the result for a private endpoint connection operation |
> | Eylem | Microsoft. DBforPostgreSQL/konumlar/privateEndpointConnectionOperationResults/Read | Gets the result for a private endpoint connection operation |
> | Eylem | Microsoft. DBforPostgreSQL/konumlar/privateEndpointConnectionProxyAzureAsyncOperation/Read | Gets the result for a private endpoint connection proxy operation |
> | Eylem | Microsoft. DBforPostgreSQL/konumlar/privateEndpointConnectionProxyOperationResults/Read | Gets the result for a private endpoint connection proxy operation |
> | Eylem | Microsoft. DBforPostgreSQL/konumlar/securityAlertPoliciesAzureAsyncOperation/Read | Sunucu tehdit algılama işlemi sonucunun listesini döndürür. |
> | Eylem | Microsoft. DBforPostgreSQL/konumlar/Securityalcertpoliciesoperationresults/Read | Sunucu tehdit algılama işlemi sonucunun listesini döndürür. |
> | Eylem | Microsoft. DBforPostgreSQL/konumlar/serverKeyAzureAsyncOperation/Read | Gets in-progress operations on transparent data encryption server keys |
> | Eylem | Microsoft. DBforPostgreSQL/konumlar/serverKeyOperationResults/Read | Gets in-progress operations on transparent data encryption server keys |
> | Eylem | Microsoft. DBforPostgreSQL/işlemler/okuma | PostgreSQL Işlemleri listesini döndürün. |
> | Eylem | Microsoft. DBforPostgreSQL/Performancekatmanlarında/Read | Kullanılabilir performans katmanları listesini döndürür. |
> | Eylem | Microsoft. DBforPostgreSQL/Register/Action | PostgreSQL kaynak sağlayıcısını Kaydet |
> | Eylem | Microsoft. DBforPostgreSQL/sunucular/Yöneticiler/Sil | PostgreSQL sunucusunun var olan bir yöneticisini siler. |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Administrators/Read | PostgreSQL sunucu yöneticilerinin bir listesini alır. |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Administrators/Write | Belirtilen parametrelerle PostgreSQL Sunucu Yöneticisi oluşturur veya güncelleştirir. |
> | Eylem | Microsoft. DBforPostgreSQL/sunucular/danışmanlar/okundu | Danışmanlarının listesini döndürün |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/danışmanlar/recommendedActions/Read | Önerilen eylemlerin listesini döndür |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/danışmanlar/recommendedActionSessions/Action | Öneri yapın |
> | Eylem | Microsoft. DBforPostgreSQL/sunucular/konfigürasyonlar/okuma | Bir sunucu için yapılandırmaların listesini döndürün veya belirtilen yapılandırma için özellikleri alır. |
> | Eylem | Microsoft. DBforPostgreSQL/sunucular/konfigürasyonlar/yazma | Belirtilen yapılandırma için değeri güncelleştir |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/veritabanları/Delete | Var olan bir PostgreSQL veritabanını siler. |
> | Eylem | Microsoft. DBforPostgreSQL/sunucular/veritabanları/okuma | PostgreSQL veritabanlarının listesini döndürün veya belirtilen veritabanının özelliklerini alır. |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/veritabanları/Write | Belirtilen parametrelerle bir PostgreSQL veritabanı oluşturur veya belirtilen veritabanı için özellikleri güncelleştirir. |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Delete | Deletes an existing server. |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/firewallRules/Delete | Mevcut bir güvenlik duvarı kuralını siler. |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/firewallRules/Read | Bir sunucu için güvenlik duvarı kuralları listesini döndürün veya belirtilen güvenlik duvarı kuralının özelliklerini alır. |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/firewallRules/Write | Belirtilen parametrelerle bir güvenlik duvarı kuralı oluşturur veya var olan bir kuralı güncelleştirir. |
> | Eylem | Microsoft. DBforPostgreSQL/sunucular/anahtarlar/silme | Deletes an existing server key. |
> | Eylem | Microsoft. DBforPostgreSQL/sunucular/anahtarlar/okuma | Return the list of server keys or gets the properties for the specified server key. |
> | Eylem | Microsoft. DBforPostgreSQL/sunucular/anahtarlar/yazma | Creates a key with the specified parameters or update the properties or tags for the specified server key. |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/logFiles/Read | PostgreSQL LogFiles listesini döndürün. |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Privateendpointconnectionproxy 'Leri/silme | Deletes an existing private endpoint connection proxy |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Privateendpointconnectionproxy/okuma | Returns the list of private endpoint connection proxies or gets the properties for the specified private endpoint connection proxy. |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Privateendpointconnectionproxy/doğrulama/eylem | Validates a private endpoint connection create call from NRP side |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Privateendpointconnectionproxy/yazma | Creates a private endpoint connection proxy with the specified parameters or updates the properties or tags for the specified private endpoint connection proxy. |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/privateEndpointConnections/Delete | Deletes an existing private endpoint connection |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/privateEndpointConnections/Read | Returns the list of private endpoint connections or gets the properties for the specified private endpoint connection. |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/privateEndpointConnections/Write | Approves or rejects an existing private endpoint connection |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/privateLinkResources/Read | Karşılık gelen PostgreSQL sunucusu için özel bağlantı kaynakları alın |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Providers/Microsoft. Insights/diagnosticSettings/Read | Kaynak için ayrılan ayarı alır |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Providers/Microsoft. Insights/diagnosticSettings/Write | Creates or updates the diagnostic setting for the resource |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Providers/Microsoft. Insights/logDefinitions/Read | Postgres sunucuları için kullanılabilir günlükleri alır |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Providers/Microsoft. Insights/metricDefinitions/Read | Return types of metrics that are available for databases |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Querymetinler/eylem | Bir sorgunun metnini döndürme |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Querymetin/Read | Sorgu listesi için metin döndürme |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Read | Return the list of servers or gets the properties for the specified server. |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/recoverableServers/Read | Kurtarılabilir PostgreSQL sunucu bilgilerini döndürür |
> | Eylem | Microsoft. DBforPostgreSQL/sunucular/çoğaltmalar/okuma | Bir PostgreSQL sunucusunun okuma çoğaltmalarını al |
> | Eylem | Microsoft. DBforPostgreSQL/sunucular/yeniden Başlat/eylem | Belirli bir sunucuyu yeniden başlatır. |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Securityalcertpolicies/Read | Belirli bir sunucuda yapılandırılmış sunucu tehdit algılama ilkesinin ayrıntılarını alma |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Securityalcertpolicies/Write | Change the server threat detection policy for a given server |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Topquerystatıstıcs/Read | En üstteki sorgular için sorgu Istatistikleri listesini döndürür. |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/updateConfigurations/Action | Belirtilen sunucu için Güncelleştirme yapılandırması |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/virtualNetworkRules/Delete | Deletes an existing Virtual Network Rule |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/virtualNetworkRules/Read | Return the list of virtual network rules or gets the properties for the specified virtual network rule. |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/virtualNetworkRules/Write | Creates a virtual network rule with the specified parameters or update the properties or tags for the specified virtual network rule. |
> | Eylem | Microsoft. DBforPostgreSQL/sunucular/waitStatistics/okuma | Örnek için bekleme istatistiklerini döndür |
> | Eylem | Microsoft. DBforPostgreSQL/Servers/Write | Creates a server with the specified parameters or update the properties or tags for the specified server. |
> | Eylem | Microsoft. DBforPostgreSQL/serversv2/Configurations/Read | Bir sunucu için yapılandırmaların listesini döndürün veya belirtilen yapılandırma için özellikleri alır. |
> | Eylem | Microsoft. DBforPostgreSQL/serversv2/Configurations/Write | Belirtilen yapılandırma için değeri güncelleştir |
> | Eylem | Microsoft. DBforPostgreSQL/serversv2/Delete | Deletes an existing server. |
> | Eylem | Microsoft. DBforPostgreSQL/serversv2/firewallRules/Delete | Mevcut bir güvenlik duvarı kuralını siler. |
> | Eylem | Microsoft. DBforPostgreSQL/serversv2/firewallRules/Read | Bir sunucu için güvenlik duvarı kuralları listesini döndürün veya belirtilen güvenlik duvarı kuralının özelliklerini alır. |
> | Eylem | Microsoft. DBforPostgreSQL/serversv2/firewallRules/Write | Belirtilen parametrelerle bir güvenlik duvarı kuralı oluşturur veya var olan bir kuralı güncelleştirir. |
> | Eylem | Microsoft. DBforPostgreSQL/serversv2/Providers/Microsoft. Insights/diagnosticSettings/Read | Kaynak için ayrılan ayarı alır |
> | Eylem | Microsoft. DBforPostgreSQL/serversv2/Providers/Microsoft. Insights/diagnosticSettings/Write | Creates or updates the diagnostic setting for the resource |
> | Eylem | Microsoft. DBforPostgreSQL/serversv2/Providers/Microsoft. Insights/logDefinitions/Read | Postgres sunucuları için kullanılabilir günlükleri alır |
> | Eylem | Microsoft. DBforPostgreSQL/serversv2/Providers/Microsoft. Insights/metricDefinitions/Read | Return types of metrics that are available for databases |
> | Eylem | Microsoft. DBforPostgreSQL/serversv2/Read | Return the list of servers or gets the properties for the specified server. |
> | Eylem | Microsoft. DBforPostgreSQL/serversv2/updateConfigurations/Action | Belirtilen sunucu için Güncelleştirme yapılandırması |
> | Eylem | Microsoft. DBforPostgreSQL/serversv2/Write | Creates a server with the specified parameters or update the properties or tags for the specified server. |

## <a name="microsoftdevices"></a>Microsoft. Devices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft. Devices/Account/diagnosticSettings/Read | Gets the diagnostic setting for the resource |
> | Eylem | Microsoft. Devices/Account/diagnosticSettings/Write | Creates or updates the diagnostic setting for the resource |
> | Eylem | Microsoft. Devices/Account/logDefinitions/Read | Gets the available log definitions for the IotHub Service |
> | Eylem | Microsoft. Devices/Account/metricDefinitions/Read | Gets the available metrics for the IotHub service |
> | Eylem | Microsoft. Devices/Checknameavaılabılıty/Action | Iothub adının kullanılabilir olup olmadığını denetle |
> | Eylem | Microsoft. Devices/Checknameavaılabılıty/Action | Iothub adının kullanılabilir olup olmadığını denetle |
> | Eylem | Microsoft. Devices/Checkprovisioningservicenameavaılabılıty/Action | Sağlama hizmeti adının kullanılabilir olup olmadığını denetleyin |
> | Eylem | Microsoft. Devices/Checkprovisioningservicenameavaılabılıty/Action | Sağlama hizmeti adının kullanılabilir olup olmadığını denetleyin |
> | Eylem | Microsoft. Devices/Digitaltwıns/Delete | Var olan bir dijital TWINS hesabını ve tüm alt öğelerini Sil |
> | Eylem | Microsoft. Devices/Digitaltwıns/operationresults/Read | Bir işlemin durumunu dijital bir TWINS hesabına göre alma |
> | Eylem | Microsoft. Devices/Digitaltwıns/Read | Abonelikle ilişkili dijital TWINS hesaplarının bir listesini alır |
> | Eylem | Microsoft. Devices/Digitaltwıns/SKU/okuma | Dijital TWINS hesapları için geçerli SKU 'ların bir listesini alın |
> | Eylem | Microsoft. Devices/Digitaltwıns/Write | Yeni bir digitial Twins hesabı oluşturun |
> | Eylem | Microsoft. Devices/Elaun havuzları/diagnosticSettings/Read | Gets the diagnostic setting for the resource |
> | Eylem | Microsoft. Devices/Elaun havuzları/diagnosticSettings/Write | Creates or updates the diagnostic setting for the resource |
> | Eylem | Microsoft. Devices/Elaun havuzları/eventGridFilters/Delete | Elastik Havuz Event Grid filtresini siler |
> | Eylem | Microsoft. Devices/Elaun havuzları/eventGridFilters/Read | Elastik Havuz Event Grid filtresini alır |
> | Eylem | Microsoft. Devices/Elaun havuzları/eventGridFilters/Write | Yeni oluştur veya Elastik Havuz Event Grid filtresini Güncelleştir |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/sertifikalar/Sil | Deletes Certificate |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/Certificates/Generatedoğrulamaları Icationcode/Action | Generate Verification code |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/sertifikalar/okuma | Gets the Certificate |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/sertifikalar/doğrula/eylem | Verify Certificate resource |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/sertifikalar/yazma | Create or Update Certificate |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/Delete | Iothub kiracı kaynağını silme |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/diagnosticSettings/Read | Gets the diagnostic setting for the resource |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/diagnosticSettings/Write | Creates or updates the diagnostic setting for the resource |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/eventHubEndpoints/consumerGroups/Delete | Delete EventHub Consumer Group |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/eventHubEndpoints/consumerGroups/Read | Get EventHub Consumer Group(s) |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/eventHubEndpoints/consumerGroups/Write | Create EventHub Consumer Group |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/exportDevices/Action | Export Devices |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/getStats/Read | Iothub kiracı istatistikleri kaynağını alır |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/ımportdevices/Action | Import Devices |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/iotHubKeys/ListKeys/Action | Iothub kiracı anahtarını alır |
> | Eylem | Microsoft. Devices/Elaun havuzları/Iothubkiracılar/işler/okuma | Get Job(s) details submitted on given IotHub |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/listKeys/Action | Iothub kiracı anahtarlarını alır |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/logDefinitions/Read | Gets the available log definitions for the IotHub Service |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/metricDefinitions/okuma | Gets the available metrics for the IotHub service |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/Quotaölçümlerini/Read | Get Quota Metrics |
> | Eylem | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/Read | Gets the IotHub tenant resource |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testall/Action | Test a message against all existing Routes |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testnew/Action | Test a message against a provided test Route |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/routingEndpointsHealth/Read | Gets the health of all routing Endpoints for an IotHub |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/securitySettings/operationResults/Read | Get the result of the Async Put operation for Iot Tenant Hub SecuritySettings |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/securitySettings/Read | Get the Azure Security Center settings on Iot Tenant Hub |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/securitySettings/Write | Update the Azure Security Center settings on Iot Tenant Hub |
> | Eylem | Microsoft.Devices/elasticPools/iotHubTenants/Write | Create or Update the IotHub tenant resource |
> | Eylem | Microsoft.Devices/ElasticPools/metricDefinitions/read | Gets the available metrics for the IotHub service |
> | Eylem | Microsoft.Devices/iotHubs/certificates/Delete | Deletes Certificate |
> | Eylem | Microsoft.Devices/iotHubs/certificates/generateVerificationCode/Action | Generate Verification code |
> | Eylem | Microsoft.Devices/iotHubs/certificates/Read | Gets the Certificate |
> | Eylem | Microsoft.Devices/iotHubs/certificates/verify/Action | Verify Certificate resource |
> | Eylem | Microsoft.Devices/iotHubs/certificates/Write | Create or Update Certificate |
> | Eylem | Microsoft.Devices/iotHubs/Delete | Delete IotHub Resource |
> | Eylem | Microsoft.Devices/IotHubs/diagnosticSettings/read | Gets the diagnostic setting for the resource |
> | Eylem | Microsoft.Devices/IotHubs/diagnosticSettings/write | Creates or updates the diagnostic setting for the resource |
> | Eylem | Microsoft.Devices/iotHubs/eventGridFilters/Delete | Deletes the Event Grid filter |
> | Eylem | Microsoft.Devices/iotHubs/eventGridFilters/Read | Gets the Event Grid filter |
> | Eylem | Microsoft.Devices/iotHubs/eventGridFilters/Write | Create new or Update existing Event Grid filter |
> | Eylem | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Delete | Delete EventHub Consumer Group |
> | Eylem | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Read | Get EventHub Consumer Group(s) |
> | Eylem | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Write | Create EventHub Consumer Group |
> | Eylem | Microsoft.Devices/iotHubs/exportDevices/Action | Export Devices |
> | Eylem | Microsoft.Devices/iotHubs/importDevices/Action | Import Devices |
> | Eylem | Microsoft.Devices/iotHubs/iotHubKeys/listkeys/Action | Get IotHub Key for the given name |
> | Eylem | Microsoft.Devices/iotHubs/iotHubStats/Read | Get IotHub Statistics |
> | Eylem | Microsoft.Devices/iotHubs/jobs/Read | Get Job(s) details submitted on given IotHub |
> | Eylem | Microsoft.Devices/iotHubs/listkeys/Action | Get all IotHub Keys |
> | Eylem | Microsoft.Devices/IotHubs/logDefinitions/read | Gets the available log definitions for the IotHub Service |
> | Eylem | Microsoft.Devices/IotHubs/metricDefinitions/read | Gets the available metrics for the IotHub service |
> | Eylem | Microsoft.Devices/iotHubs/operationresults/Read | Get Operation Result (Obsolete API) |
> | Eylem | Microsoft.Devices/iotHubs/quotaMetrics/Read | Get Quota Metrics |
> | Eylem | Microsoft.Devices/iotHubs/Read | Gets the IotHub resource(s) |
> | Eylem | Microsoft.Devices/iotHubs/routing/$testall/Action | Test a message against all existing Routes |
> | Eylem | Microsoft.Devices/iotHubs/routing/$testnew/Action | Test a message against a provided test Route |
> | Eylem | Microsoft.Devices/iotHubs/routingEndpointsHealth/Read | Gets the health of all routing Endpoints for an IotHub |
> | Eylem | Microsoft.Devices/iotHubs/securitySettings/operationResults/Read | Get the result of the Async Put operation for Iot Hub SecuritySettings |
> | Eylem | Microsoft.Devices/iotHubs/securitySettings/Read | Get the Azure Security Center settings on Iot Hub |
> | Eylem | Microsoft.Devices/iotHubs/securitySettings/Write | Update the Azure Security Center settings on Iot Hub |
> | Eylem | Microsoft.Devices/iotHubs/skus/Read | Get valid IotHub Skus |
> | Eylem | Microsoft.Devices/iotHubs/Write | Create or update IotHub Resource |
> | Eylem | Microsoft.Devices/locations/operationresults/Read | Get Location based Operation Result |
> | Eylem | Microsoft.Devices/operationresults/Read | Get Operation Result |
> | Eylem | Microsoft.Devices/operations/Read | Get All ResourceProvider Operations |
> | Eylem | Microsoft.Devices/provisioningServices/certificates/Delete | Deletes Certificate |
> | Eylem | Microsoft.Devices/provisioningServices/certificates/generateVerificationCode/Action | Generate Verification code |
> | Eylem | Microsoft.Devices/provisioningServices/certificates/Read | Gets the Certificate |
> | Eylem | Microsoft.Devices/provisioningServices/certificates/verify/Action | Verify Certificate resource |
> | Eylem | Microsoft.Devices/provisioningServices/certificates/Write | Create or Update Certificate |
> | Eylem | Microsoft.Devices/provisioningServices/Delete | Delete IotDps resource |
> | Eylem | Microsoft.Devices/provisioningServices/diagnosticSettings/read | Gets the diagnostic setting for the resource |
> | Eylem | Microsoft.Devices/provisioningServices/diagnosticSettings/write | Creates or updates the diagnostic setting for the resource |
> | Eylem | Microsoft.Devices/provisioningServices/keys/listkeys/Action | Get IotDps Keys for key name |
> | Eylem | Microsoft.Devices/provisioningServices/listkeys/Action | Get all IotDps keys |
> | Eylem | Microsoft.Devices/provisioningServices/logDefinitions/read | Gets the available log definitions for the provisioning Service |
> | Eylem | Microsoft.Devices/provisioningServices/metricDefinitions/read | Gets the available metrics for the provisioning service |
> | Eylem | Microsoft.Devices/provisioningServices/operationresults/Read | Get DPS Operation Result |
> | Eylem | Microsoft.Devices/provisioningServices/Read | Get IotDps resource |
> | Eylem | Microsoft.Devices/provisioningServices/skus/Read | Get valid IotDps Skus |
> | Eylem | Microsoft.Devices/provisioningServices/Write | Create IotDps resource |
> | Eylem | Microsoft.Devices/register/action | Register the subscription for the IotHub resource provider and enables the creation of IotHub resources |
> | Eylem | Microsoft.Devices/usages/Read | Get subscription usage details for this provider. |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DevSpaces/controllers/delete | Delete Azure Dev Spaces Controller and dataplane services |
> | Eylem | Microsoft.DevSpaces/controllers/listConnectionDetails/action | List connection details for the Azure Dev Spaces Controller's infrastructure |
> | Eylem | Microsoft.DevSpaces/controllers/read | Read Azure Dev Spaces Controller properties |
> | Eylem | Microsoft.DevSpaces/controllers/write | Create or Update Azure Dev Spaces Controller properties |
> | Eylem | Microsoft.DevSpaces/locations/checkContainerHostMapping/action | Check existing controller mapping for a container host |
> | Eylem | Microsoft.DevSpaces/locations/operationresults/read | Read status for an asynchronous operation |
> | Eylem | Microsoft.DevSpaces/register/action | Register Microsoft Dev Spaces resource provider with a subscription |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DevTestLab/labCenters/delete | Delete lab centers. |
> | Eylem | Microsoft.DevTestLab/labCenters/read | Read lab centers. |
> | Eylem | Microsoft.DevTestLab/labCenters/write | Add or modify lab centers. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/armTemplates/read | Read azure resource manager templates. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/artifacts/GenerateArmTemplate/action | Generates an Azure Resource Manager template for the given artifact, uploads the required files to a storage account, and validates the generated artifact. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/artifacts/read | Read artifacts. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/delete | Delete artifact sources. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/read | Read artifact sources. |
> | Eylem | Microsoft.DevTestLab/labs/artifactSources/write | Add or modify artifact sources. |
> | Eylem | Microsoft.DevTestLab/labs/ClaimAnyVm/action | Claim a random claimable virtual machine in the lab. |
> | Eylem | Microsoft.DevTestLab/labs/costs/read | Read costs. |
> | Eylem | Microsoft.DevTestLab/labs/costs/write | Add or modify costs. |
> | Eylem | Microsoft.DevTestLab/labs/CreateEnvironment/action | Create virtual machines in a lab. |
> | Eylem | Microsoft.DevTestLab/labs/customImages/delete | Delete custom images. |
> | Eylem | Microsoft.DevTestLab/labs/customImages/read | Read custom images. |
> | Eylem | Microsoft.DevTestLab/labs/customImages/write | Add or modify custom images. |
> | Eylem | Microsoft.DevTestLab/labs/delete | Delete labs. |
> | Eylem | Microsoft.DevTestLab/labs/EnsureCurrentUserProfile/action | Ensure the current user has a valid profile in the lab. |
> | Eylem | Microsoft.DevTestLab/labs/ExportResourceUsage/action | Exports the lab resource usage into a storage account |
> | Eylem | Microsoft.DevTestLab/labs/formulas/delete | Delete formulas. |
> | Eylem | Microsoft.DevTestLab/labs/formulas/read | Read formulas. |
> | Eylem | Microsoft.DevTestLab/labs/formulas/write | Add or modify formulas. |
> | Eylem | Microsoft.DevTestLab/labs/galleryImages/read | Read gallery images. |
> | Eylem | Microsoft.DevTestLab/labs/GenerateUploadUri/action | Generate a URI for uploading custom disk images to a Lab. |
> | Eylem | Microsoft.DevTestLab/labs/ImportVirtualMachine/action | Import a virtual machine into a different lab. |
> | Eylem | Microsoft.DevTestLab/labs/ListVhds/action | List disk images available for custom image creation. |
> | Eylem | Microsoft.DevTestLab/labs/notificationChannels/delete | Delete notification channels. |
> | Eylem | Microsoft.DevTestLab/labs/notificationChannels/Notify/action | Send notification to provided channel. |
> | Eylem | Microsoft.DevTestLab/labs/notificationChannels/read | Read notification channels. |
> | Eylem | Microsoft.DevTestLab/labs/notificationChannels/write | Add or modify notification channels. |
> | Eylem | Microsoft.DevTestLab/labs/policySets/EvaluatePolicies/action | Evaluates lab policy. |
> | Eylem | Microsoft.DevTestLab/labs/policySets/policies/delete | Delete policies. |
> | Eylem | Microsoft.DevTestLab/labs/policySets/policies/read | Read policies. |
> | Eylem | Microsoft.DevTestLab/labs/policySets/policies/write | Add or modify policies. |
> | Eylem | Microsoft.DevTestLab/labs/policySets/read | Read policy sets. |
> | Eylem | Microsoft.DevTestLab/labs/read | Read labs. |
> | Eylem | Microsoft.DevTestLab/labs/schedules/delete | Delete schedules. |
> | Eylem | Microsoft.DevTestLab/labs/schedules/Execute/action | Execute a schedule. |
> | Eylem | Microsoft.DevTestLab/labs/schedules/ListApplicable/action | Lists all applicable schedules |
> | Eylem | Microsoft.DevTestLab/labs/schedules/read | Read schedules. |
> | Eylem | Microsoft.DevTestLab/labs/schedules/write | Add or modify schedules. |
> | Eylem | Microsoft.DevTestLab/labs/serviceRunners/delete | Delete service runners. |
> | Eylem | Microsoft.DevTestLab/labs/serviceRunners/read | Read service runners. |
> | Eylem | Microsoft.DevTestLab/labs/serviceRunners/write | Add or modify service runners. |
> | Eylem | Microsoft.DevTestLab/labs/sharedGalleries/delete | Delete shared galleries. |
> | Eylem | Microsoft.DevTestLab/labs/sharedGalleries/read | Read shared galleries. |
> | Eylem | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/delete | Delete shared images. |
> | Eylem | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/read | Read shared images. |
> | Eylem | Microsoft.DevTestLab/labs/sharedGalleries/sharedImages/write | Add or modify shared images. |
> | Eylem | Microsoft.DevTestLab/labs/sharedGalleries/write | Add or modify shared galleries. |
> | Eylem | Microsoft.DevTestLab/labs/users/delete | Delete user profiles. |
> | Eylem | Microsoft.DevTestLab/labs/users/disks/Attach/action | Attach and create the lease of the disk to the virtual machine. |
> | Eylem | Microsoft.DevTestLab/labs/users/disks/delete | Delete disks. |
> | Eylem | Microsoft.DevTestLab/labs/users/disks/Detach/action | Detach and break the lease of the disk attached to the virtual machine. |
> | Eylem | Microsoft.DevTestLab/labs/users/disks/read | Read disks. |
> | Eylem | Microsoft.DevTestLab/labs/users/disks/write | Add or modify disks. |
> | Eylem | Microsoft.DevTestLab/labs/users/environments/delete | Delete environments. |
> | Eylem | Microsoft.DevTestLab/labs/users/environments/read | Read environments. |
> | Eylem | Microsoft.DevTestLab/labs/users/environments/write | Add or modify environments. |
> | Eylem | Microsoft.DevTestLab/labs/users/read | Read user profiles. |
> | Eylem | Microsoft.DevTestLab/labs/users/secrets/delete | Delete secrets. |
> | Eylem | Microsoft.DevTestLab/labs/users/secrets/read | Read secrets. |
> | Eylem | Microsoft.DevTestLab/labs/users/secrets/write | Add or modify secrets. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/delete | Delete service fabrics. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/ListApplicableSchedules/action | Lists the applicable start/stop schedules, if any. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/read | Read service fabrics. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/delete | Delete schedules. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/Execute/action | Execute a schedule. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/read | Read schedules. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/write | Add or modify schedules. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/Start/action | Start a service fabric. |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/Stop/action | Stop a service fabric |
> | Eylem | Microsoft.DevTestLab/labs/users/serviceFabrics/write | Add or modify service fabrics. |
> | Eylem | Microsoft.DevTestLab/labs/users/write | Add or modify user profiles. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/AddDataDisk/action | Attach a new or existing data disk to virtual machine. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/ApplyArtifacts/action | Apply artifacts to virtual machine. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Claim/action | Take ownership of an existing virtual machine |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/ClearArtifactResults/action | Clears the artifact results of the virtual machine. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/delete | Delete virtual machines. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/DetachDataDisk/action | Detach the specified disk from the virtual machine. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/GetRdpFileContents/action | Gets a string that represents the contents of the RDP file for the virtual machine |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/ListApplicableSchedules/action | Lists the applicable start/stop schedules, if any. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/read | Read virtual machines. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Redeploy/action | Redeploy a virtual machine |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Resize/action | Resize Virtual Machine. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Restart/action | Restart a virtual machine. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/schedules/delete | Delete schedules. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/schedules/Execute/action | Execute a schedule. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/schedules/read | Read schedules. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/schedules/write | Add or modify schedules. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Start/action | Start a virtual machine. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/Stop/action | Bir sanal makineyi durdurma |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/TransferDisks/action | Transfers all data disks attached to the virtual machine to be owned by the current user. |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/UnClaim/action | Release ownership of an existing virtual machine |
> | Eylem | Microsoft.DevTestLab/labs/virtualMachines/write | Add or modify virtual machines. |
> | Eylem | Microsoft.DevTestLab/labs/virtualNetworks/bastionHosts/delete | Delete bastionhosts. |
> | Eylem | Microsoft.DevTestLab/labs/virtualNetworks/bastionHosts/read | Read bastionhosts. |
> | Eylem | Microsoft.DevTestLab/labs/virtualNetworks/bastionHosts/write | Add or modify bastionhosts. |
> | Eylem | Microsoft.DevTestLab/labs/virtualNetworks/delete | Delete virtual networks. |
> | Eylem | Microsoft.DevTestLab/labs/virtualNetworks/read | Read virtual networks. |
> | Eylem | Microsoft.DevTestLab/labs/virtualNetworks/write | Add or modify virtual networks. |
> | Eylem | Microsoft.DevTestLab/labs/vmPools/delete | Delete virtual machine pools. |
> | Eylem | Microsoft.DevTestLab/labs/vmPools/read | Read virtual machine pools. |
> | Eylem | Microsoft.DevTestLab/labs/vmPools/write | Add or modify virtual machine pools. |
> | Eylem | Microsoft.DevTestLab/labs/write | Add or modify labs. |
> | Eylem | Microsoft.DevTestLab/locations/operations/read | Read operations. |
> | Eylem | Microsoft.DevTestLab/register/action | Registers the subscription |
> | Eylem | Microsoft.DevTestLab/schedules/delete | Delete schedules. |
> | Eylem | Microsoft.DevTestLab/schedules/Execute/action | Execute a schedule. |
> | Eylem | Microsoft.DevTestLab/schedules/read | Read schedules. |
> | Eylem | Microsoft.DevTestLab/schedules/Retarget/action | Updates a schedule's target resource Id. |
> | Eylem | Microsoft.DevTestLab/schedules/write | Add or modify schedules. |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DocumentDB/databaseAccountNames/read | Checks for name availability. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/delete | Delete a collection. Only applicable to API types: 'mongodb'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/operationResults/read | Read status of the asynchronous operation. Only applicable to API types: 'mongodb'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/read | Read a collection or list all the collections. Only applicable to API types: 'mongodb'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings/operationResults/read | Read status of the asynchronous operation. Only applicable to API types: 'mongodb'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings/read | Read a collection throughput. Only applicable to API types: 'mongodb'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/settings/write | Update a collection throughput. Only applicable to API types: 'mongodb'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/collections/write | Create or update a collection. Only applicable to API types: 'mongodb'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/delete | Delete a container. Only applicable to API types: 'sql'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/operationResults/read | Read status of the asynchronous operation. Only applicable to API types: 'sql'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/read | Read a container or list all the containers. Only applicable to API types: 'sql'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings/operationResults/read | Read status of the asynchronous operation. Only applicable to API types: 'sql'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings/read | Read a container throughput. Only applicable to API types: 'sql'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/settings/write | Update a container throughput. Only applicable to API types: 'sql'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/containers/write | Create or update a container. Only applicable to API types: 'sql'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/delete | Delete a database. Only applicable to API types: 'sql', 'mongodb', 'gremlin'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/delete | Delete a graph. Only applicable to API types: 'gremlin'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/operationResults/read | Read status of the asynchronous operation. Only applicable to API types: 'gremlin'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/read | Read a graph or list all the graphs. Only applicable to API types: 'gremlin'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings/operationResults/read | Read status of the asynchronous operation. Only applicable to API types: 'gremlin'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings/read | Read a graph throughput. Only applicable to API types: 'gremlin'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/settings/write | Update a graph throughput. Only applicable to API types: 'gremlin'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/graphs/write | Create or update a graph. Only applicable to API types: 'gremlin'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/operationResults/read | Read status of the asynchronous operation. Only applicable to API types: 'sql', 'mongodb', 'gremlin'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/read | Read a database or list all the databases. Only applicable to API types: 'sql', 'mongodb', 'gremlin'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/settings/operationResults/read | Read status of the asynchronous operation. Only applicable to API types: 'sql', 'mongodb', 'gremlin'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/settings/read | Read a database throughput. Only applicable to API types: 'sql', 'mongodb', 'gremlin'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/settings/write | Update a database throughput. Only applicable to API types: 'sql', 'mongodb', 'gremlin'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/databases/write | Create a database. Only applicable to API types: 'sql', 'mongodb', 'gremlin'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/delete | Delete a keyspace. Only applicable to API types: 'cassandra'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/operationResults/read | Read status of the asynchronous operation. Only applicable to API types: 'cassandra'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/read | Read a keyspace or list all the keyspaces. Only applicable to API types: 'cassandra'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/settings/operationResults/read | Read status of the asynchronous operation. Only applicable to API types: 'cassandra'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/settings/read | Read a keyspace throughput. Only applicable to API types: 'cassandra'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/settings/write | Update a keyspace throughput. Only applicable to API types: 'cassandra'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/delete | Delete a table. Only applicable to API types: 'cassandra'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/operationResults/read | Read status of the asynchronous operation. Only applicable to API types: 'cassandra'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/read | Read a table or list all the tables. Only applicable to API types: 'cassandra'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/settings/operationResults/read | Read status of the asynchronous operation. Only applicable to API types: 'cassandra'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/settings/read | Read a table throughput. Only applicable to API types: 'cassandra'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/settings/write | Update a table throughput. Only applicable to API types: 'cassandra'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/tables/write | Create or update a table. Only applicable to API types: 'cassandra'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/keyspaces/write | Create a keyspace. Only applicable to API types: 'cassandra'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/tables/delete | Delete a table. Only applicable to API types: 'table'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/tables/operationResults/read | Read status of the asynchronous operation. Only applicable to API types: 'table'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/tables/read | Read a table or list all the tables. Only applicable to API types: 'table'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/tables/settings/operationResults/read | Read status of the asynchronous operation. Only applicable to API types: 'table'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/tables/settings/read | Read a table throughput. Only applicable to API types: 'table'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/tables/settings/write | Update a table throughput. Only applicable to API types: 'table'. Only applicable for setting types: 'throughput'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/apis/tables/write | Create or update a table. Only applicable to API types: 'table'. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/backup/action | Submit a request to configure backup |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/changeResourceGroup/action | Change resource group of a database account |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/metricDefinitions/read | Reads the collection metric definitions. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/metrics/read | Reads the collection metrics. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitionKeyRangeId/metrics/read | Read database account partition key level metrics |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/metrics/read | Read database account partition level metrics |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/read | Read database account partitions in a collection |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/usages/read | Read database account partition level usages |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/collections/usages/read | Reads the collection usages. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/metricDefinitions/read | Reads the database metric definitions |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/metrics/read | Reads the database metrics. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/databases/usages/read | Reads the database usages. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/delete | Deletes the database accounts. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/failoverPriorityChange/action | Change failover priorities of regions of a database account. This is used to perform manual failover operation |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/getBackupPolicy/action | Get the backup policy of database account |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/action | Get the connection strings for a database account |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/listKeys/action | List keys of a database account |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/metricDefinitions/read | Reads the database account metrics definitions. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/metrics/read | Reads the database account metrics. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/offlineRegion/action | Offline a region of a database account.  |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/onlineRegion/action | Online a region of a database account. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/operationResults/read | Read status of the asynchronous operation |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/percentile/metrics/read | Read latency metrics |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/percentile/read | Read percentiles of replication latencies |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/percentile/sourceRegion/targetRegion/metrics/read | Read latency metrics for a specific source and target region |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/percentile/targetRegion/metrics/read | Read latency metrics for a specific target region |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/delete | Delete a private endpoint connection proxy of Database Account |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/operationResults/read | Read Status of private endpoint connection proxy asynchronous operation |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/read | Read a private endpoint connection proxy of Database Account |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/validate/action | Validate a private endpoint connection proxy of Database Account |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnectionProxies/write | Create or update a private endpoint connection proxy of Database Account |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnections/delete | Delete a private endpoint connection of a Database Account |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnections/operationResults/read | Read Status of privateEndpointConnenctions asynchronous operation |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnections/read | Read a private endpoint connection or list all the private endpoint connections of a Database Account |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/privateEndpointConnections/write | Create or update a private endpoint connection of a Database Account |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/privateLinkResources/read | Read a private link resource or list all the private link resources of a Database Account |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/read | Reads a database account. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Reads the database account readonly keys. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/readonlykeys/read | Reads the database account readonly keys. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/regenerateKey/action | Rotate keys of a database account |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/metrics/read | Reads the regional collection metrics. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitionKeyRangeId/metrics/read | Read regional database account partition key level metrics |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/metrics/read | Read regional database account partition level metrics |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/read | Read regional database account partitions in a collection |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/region/metrics/read | Reads the region and database account metrics. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/restore/action | Submit a restore request |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/usages/read | Reads the database account usages. |
> | Eylem | Microsoft.DocumentDB/databaseAccounts/write | Update a database accounts. |
> | Eylem | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/action | Notifies Microsoft.DocumentDB that VirtualNetwork or Subnet is being deleted |
> | Eylem | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/operationResults/read | Read Status of deleteVirtualNetworkOrSubnets asynchronous operation |
> | Eylem | Microsoft.DocumentDB/locations/operationsStatus/read | Reads Status of Asynchronous Operations |
> | Eylem | Microsoft.DocumentDB/operationResults/read | Read status of the asynchronous operation |
> | Eylem | Microsoft.DocumentDB/operations/read | Read operations available for the Microsoft DocumentDB  |
> | Eylem | Microsoft.DocumentDB/register/action |  Register the Microsoft DocumentDB resource provider for the subscription |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.DomainRegistration/checkDomainAvailability/Action | Check if a domain is available for purchase |
> | Eylem | Microsoft.DomainRegistration/domains/Delete | Delete an existing domain. |
> | Eylem | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Delete | Delete ownership identifier |
> | Eylem | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | List ownership identifiers |
> | Eylem | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Get ownership identifier |
> | Eylem | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Write | Create or update identifier |
> | Eylem | Microsoft.DomainRegistration/domains/operationresults/Read | Get a domain operation |
> | Eylem | Microsoft.DomainRegistration/domains/Read | Get the list of domains |
> | Eylem | Microsoft.DomainRegistration/domains/Read | Get domain |
> | Eylem | Microsoft.DomainRegistration/domains/renew/Action | Renew an existing domain. |
> | Eylem | Microsoft.DomainRegistration/domains/Write | Add a new Domain or update an existing one |
> | Eylem | Microsoft.DomainRegistration/generateSsoRequest/Action | Generate a request for signing into domain control center. |
> | Eylem | Microsoft.DomainRegistration/listDomainRecommendations/Action | Retrieve the list domain recommendations based on keywords |
> | Eylem | Microsoft.DomainRegistration/operations/Read | List all operations from app service domain registration |
> | Eylem | Microsoft.DomainRegistration/register/action | Register the Microsoft Domains resource provider for the subscription |
> | Eylem | Microsoft.DomainRegistration/topLevelDomains/listAgreements/Action | List Agreement action |
> | Eylem | Microsoft.DomainRegistration/topLevelDomains/Read | Get toplevel domains |
> | Eylem | Microsoft.DomainRegistration/topLevelDomains/Read | Get toplevel domain |
> | Eylem | Microsoft.DomainRegistration/validateDomainRegistrationInformation/Action | Validate domain purchase object without submitting it |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.EventGrid/domains/delete | Delete a domain |
> | Eylem | Microsoft.EventGrid/domains/listKeys/action | List keys for a domain |
> | Eylem | Microsoft.EventGrid/domains/providers/Microsoft.Insights/logDefinitions/read | Allows access to diagnostic logs |
> | Eylem | Microsoft.EventGrid/domains/providers/Microsoft.Insights/metricDefinitions/read | Gets the available metrics for domains |
> | Eylem | Microsoft.EventGrid/domains/read | Read a domain |
> | Eylem | Microsoft.EventGrid/domains/regenerateKey/action | Regenerate key for a domain |
> | Eylem | Microsoft.EventGrid/domains/topics/delete | Delete a domain topic |
> | Eylem | Microsoft.EventGrid/domains/topics/read | Read a domain topic |
> | Eylem | Microsoft.EventGrid/domains/topics/write | Create or update a domain topic |
> | Eylem | Microsoft.EventGrid/domains/write | Create or update a domain |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/delete | Delete an eventSubscription |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/getFullUrl/action | Get full url for the event subscription |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/read | Gets the diagnostic setting for event subscriptions |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/write | Creates or updates the diagnostic setting for event subscriptions |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/metricDefinitions/read | Gets the available metrics for eventSubscriptions |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/read | Read an eventSubscription |
> | Eylem | Microsoft.EventGrid/eventSubscriptions/write | Create or update an eventSubscription |
> | Eylem | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/read | Gets the diagnostic setting for topics |
> | Eylem | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/write | Creates or updates the diagnostic setting for topics |
> | Eylem | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/metricDefinitions/read | Gets the available metrics for topics |
> | Eylem | Microsoft.EventGrid/extensionTopics/read | Read an extensionTopic. |
> | Eylem | Microsoft.EventGrid/locations/eventSubscriptions/read | List regional event subscriptions |
> | Eylem | Microsoft.EventGrid/locations/operationResults/read | Read the result of a regional operation |
> | Eylem | Microsoft.EventGrid/locations/operationsStatus/read | Read the status of a regional operation |
> | Eylem | Microsoft.EventGrid/locations/topictypes/eventSubscriptions/read | List regional event subscriptions by topictype |
> | Eylem | Microsoft.EventGrid/operationResults/read | Read the result of an operation |
> | Eylem | Microsoft.EventGrid/operations/read | List EventGrid operations. |
> | Eylem | Microsoft.EventGrid/operationsStatus/read | Read the status of an operation |
> | Eylem | Microsoft.EventGrid/register/action | Registers the subscription for the EventGrid resource provider. |
> | Eylem | Microsoft.EventGrid/topics/delete | Delete a topic |
> | Eylem | Microsoft.EventGrid/topics/listKeys/action | List keys for a topic |
> | Eylem | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/read | Gets the diagnostic setting for topics |
> | Eylem | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/write | Creates or updates the diagnostic setting for topics |
> | Eylem | Microsoft.EventGrid/topics/providers/Microsoft.Insights/logDefinitions/read | Allows access to diagnostic logs |
> | Eylem | Microsoft.EventGrid/topics/providers/Microsoft.Insights/metricDefinitions/read | Gets the available metrics for topics |
> | Eylem | Microsoft.EventGrid/topics/read | Read a topic |
> | Eylem | Microsoft.EventGrid/topics/regenerateKey/action | Regenerate key for a topic |
> | Eylem | Microsoft.EventGrid/topics/write | Create or update a topic |
> | Eylem | Microsoft.EventGrid/topictypes/eventSubscriptions/read | List global event subscriptions by topic type |
> | Eylem | Microsoft.EventGrid/topictypes/eventtypes/read | Read eventtypes supported by a topictype |
> | Eylem | Microsoft.EventGrid/topictypes/read | Read a topictype |
> | Eylem | Microsoft.EventGrid/unregister/action | Unregisters the subscription for the EventGrid resource provider. |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.EventHub/availableClusterRegions/read | Read operation to list available pre-provisioned clusters by Azure region. |
> | Eylem | Microsoft.EventHub/checkNameAvailability/action | Checks availability of namespace under given subscription. |
> | Eylem | Microsoft.EventHub/checkNamespaceAvailability/action | Checks availability of namespace under given subscription. This API is deprecated please use CheckNameAvailability instead. |
> | Eylem | Microsoft.EventHub/clusters/delete | Deletes an existing Cluster resource. |
> | Eylem | Microsoft.EventHub/clusters/namespaces/read | List namespace Azure Resource Manager IDs for namespaces within a cluster. |
> | Eylem | Microsoft.EventHub/clusters/operationresults/read | Get the status of an asynchronous cluster operation. |
> | Eylem | Microsoft.EventHub/clusters/providers/Microsoft.Insights/metricDefinitions/read | Get list of Cluster metrics Resource Descriptions |
> | Eylem | Microsoft.EventHub/clusters/read | Gets the Cluster Resource Description |
> | Eylem | Microsoft.EventHub/clusters/write | Creates or modifies an existing Cluster resource. |
> | Eylem | Microsoft.EventHub/locations/deleteVirtualNetworkOrSubnets/action | Deletes the VNet rules in EventHub Resource Provider for the specified VNet |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/action | Updates Namespace Authorization Rule. This API is deprecated. Please use a PUT call to update the Namespace Authorization Rule instead.. This operation is not supported on API version 2017-04-01. |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/delete | Delete Namespace Authorization Rule. The Default Namespace Authorization Rule cannot be deleted.  |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/listkeys/action | Get the Connection String to the Namespace |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/read | Get the list of Namespaces Authorization Rules description. |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/regenerateKeys/action | Regenerate the Primary or Secondary key to the Resource |
> | Eylem | Microsoft.EventHub/namespaces/authorizationRules/write | Create a Namespace level Authorization Rules and update its properties. The Authorization Rules Access Rights, the Primary and Secondary Keys can be updated. |
> | Eylem | Microsoft.EventHub/namespaces/Delete | Delete Namespace Resource |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Gets the authorization rules keys for the Disaster Recovery primary namespace |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/read | Get Disaster Recovery Primary Namespace's Authorization Rules |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/breakPairing/action | Disables Disaster Recovery and stops replicating changes from primary to secondary namespaces. |
> | Eylem | Microsoft.EventHub/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Checks availability of namespace alias under given subscription. |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/delete | Deletes the Disaster Recovery configuration associated with the namespace. This operation can only be invoked via the primary namespace. |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/failover/action | Invokes a GEO DR failover and reconfigures the namespace alias to point to the secondary namespace. |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/read | Gets the Disaster Recovery configuration associated with the namespace. |
> | Eylem | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/write | Creates or Updates the Disaster Recovery configuration associated with the namespace. |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/action | Operation to update EventHub. This operation is not supported on API version 2017-04-01. Authorization Rules. Please use a PUT call to update Authorization Rule. |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/delete | Operation to delete EventHub Authorization Rules |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/listkeys/action | Get the Connection String to EventHub |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/read |  Get the list of EventHub Authorization Rules |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/regenerateKeys/action | Regenerate the Primary or Secondary key to the Resource |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/write | Create EventHub Authorization Rules and Update its properties. The Authorization Rules Access Rights can be updated. |
> | Eylem | Microsoft.EventHub/namespaces/eventHubs/consumergroups/Delete | Operation to delete ConsumerGroup Resource |
> | Eylem | Microsoft.EventHub/namespaces/eventHubs/consumergroups/read | Get list of ConsumerGroup Resource Descriptions |
> | Eylem | Microsoft.EventHub/namespaces/eventHubs/consumergroups/write | Create or Update ConsumerGroup properties. |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/Delete | Operation to delete EventHub Resource |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/read | Get list of EventHub Resource Descriptions |
> | Eylem | Microsoft.EventHub/namespaces/eventhubs/write | Create or Update EventHub properties. |
> | Eylem | Microsoft.EventHub/namespaces/ipFilterRules/delete | Delete IP Filter Resource |
> | Eylem | Microsoft.EventHub/namespaces/ipFilterRules/read | Get IP Filter Resource |
> | Eylem | Microsoft.EventHub/namespaces/ipFilterRules/write | Create IP Filter Resource |
> | DataAction | Microsoft.EventHub/namespaces/messages/receive/action | İleti alma |
> | DataAction | Microsoft.EventHub/namespaces/messages/send/action | İleti gönderme |
> | Eylem | Microsoft.EventHub/namespaces/messagingPlan/read | Gets the Messaging Plan for a namespace.<br>This API is deprecated.<br>Properties exposed via the MessagingPlan resource are moved to the (parent) Namespace resource in later API versions..<br>This operation is not supported on API version 2017-04-01. |
> | Eylem | Microsoft.EventHub/namespaces/messagingPlan/write | Updates the Messaging Plan for a namespace.<br>This API is deprecated.<br>Properties exposed via the MessagingPlan resource are moved to the (parent) Namespace resource in later API versions..<br>This operation is not supported on API version 2017-04-01. |
> | Eylem | Microsoft.EventHub/namespaces/networkruleset/delete | Delete VNET Rule Resource |
> | Eylem | Microsoft.EventHub/namespaces/networkruleset/read | Gets NetworkRuleSet Resource |
> | Eylem | Microsoft.EventHub/namespaces/networkruleset/write | Create VNET Rule Resource |
> | Eylem | Microsoft.EventHub/namespaces/networkrulesets/delete | Delete VNET Rule Resource |
> | Eylem | Microsoft.EventHub/namespaces/networkrulesets/read | Gets NetworkRuleSet Resource |
> | Eylem | Microsoft.EventHub/namespaces/networkrulesets/write | Create VNET Rule Resource |
> | Eylem | Microsoft.EventHub/namespaces/operationresults/read | Get the status of Namespace operation |
> | Eylem | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Get list of Namespace diagnostic settings Resource Descriptions |
> | Eylem | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Get list of Namespace diagnostic settings Resource Descriptions |
> | Eylem | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/logDefinitions/read | Get list of Namespace logs Resource Descriptions |
> | Eylem | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Get list of Namespace metrics Resource Descriptions |
> | Eylem | Microsoft.EventHub/namespaces/read | Get the list of Namespace Resource Description |
> | Eylem | Microsoft.EventHub/namespaces/removeAcsNamepsace/action | Remove ACS namespace |
> | Eylem | Microsoft.EventHub/namespaces/virtualNetworkRules/delete | Delete VNET Rule Resource |
> | Eylem | Microsoft.EventHub/namespaces/virtualNetworkRules/read | Gets VNET Rule Resource |
> | Eylem | Microsoft.EventHub/namespaces/virtualNetworkRules/write | Create VNET Rule Resource |
> | Eylem | Microsoft.EventHub/namespaces/write | Create a Namespace Resource and Update its properties. Tags and Capacity of the Namespace are the properties which can be updated. |
> | Eylem | Microsoft.EventHub/operations/read | Get Operations |
> | Eylem | Microsoft.EventHub/register/action | Registers the subscription for the EventHub resource provider and enables the creation of EventHub resources |
> | Eylem | Microsoft.EventHub/sku/read | Get list of Sku Resource Descriptions |
> | Eylem | Microsoft.EventHub/sku/regions/read | Get list of SkuRegions Resource Descriptions |
> | Eylem | Microsoft.EventHub/unregister/action | Registers the EventHub Resource Provider |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Features/features/read | Gets the features of a subscription. |
> | Eylem | Microsoft.Features/operations/read | Gets the list of operations. |
> | Eylem | Microsoft.Features/providers/features/read | Gets the feature of a subscription in a given resource provider. |
> | Eylem | Microsoft.Features/providers/features/register/action | Registers the feature for a subscription in a given resource provider. |
> | Eylem | Microsoft.Features/providers/features/unregister/action | Unregisters the feature for a subscription in a given resource provider. |
> | Eylem | Microsoft.Features/register/action | Registers the feature of a subscription. |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.GuestConfiguration/guestConfigurationAssignments/delete | Delete guest configuration assignment. |
> | Eylem | Microsoft.GuestConfiguration/guestConfigurationAssignments/read | Get guest configuration assignment. |
> | Eylem | Microsoft.GuestConfiguration/guestConfigurationAssignments/reports/read | Get guest configuration assignment report. |
> | Eylem | Microsoft.GuestConfiguration/guestConfigurationAssignments/write | Create new guest configuration assignment. |
> | Eylem | Microsoft.GuestConfiguration/operations/read | Gets the operations for the Microsoft.GuestConfiguration resource provider |
> | Eylem | Microsoft.GuestConfiguration/register/action | Registers the subscription for the Microsoft.GuestConfiguration resource provider. |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.HDInsight/clusters/applications/delete | Delete Application for HDInsight Cluster |
> | Eylem | Microsoft.HDInsight/clusters/applications/read | Get Application for HDInsight Cluster |
> | Eylem | Microsoft.HDInsight/clusters/applications/write | Create or Update Application for HDInsight Cluster |
> | Eylem | Microsoft.HDInsight/clusters/changerdpsetting/action | Change RDP setting for HDInsight Cluster |
> | Eylem | Microsoft.HDInsight/clusters/configurations/action | Update HDInsight Cluster Configuration |
> | Eylem | Microsoft.HDInsight/clusters/configurations/action | Get HDInsight Cluster Configurations |
> | Eylem | Microsoft.HDInsight/clusters/configurations/read | Get HDInsight Cluster Configurations |
> | Eylem | Microsoft.HDInsight/clusters/delete | Delete a HDInsight Cluster |
> | Eylem | Microsoft.HDInsight/clusters/extensions/delete | Delete Cluster Extension for HDInsight Cluster |
> | Eylem | Microsoft.HDInsight/clusters/extensions/read | Get Cluster Extension for HDInsight Cluster |
> | Eylem | Microsoft.HDInsight/clusters/extensions/write | Create Cluster Extension for HDInsight Cluster |
> | Eylem | Microsoft.HDInsight/clusters/getGatewaySettings/action | Get gateway settings for HDInsight Cluster |
> | Eylem | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/read | Gets the diagnostic setting for the resource HDInsight Cluster |
> | Eylem | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/write | Creates or updates the diagnostic setting for the resource HDInsight Cluster |
> | Eylem | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/metricDefinitions/read | Gets the available metrics for HDInsight Cluster |
> | Eylem | Microsoft.HDInsight/clusters/read | Get details about HDInsight Cluster |
> | Eylem | Microsoft.HDInsight/clusters/roles/resize/action | Resize a HDInsight Cluster |
> | Eylem | Microsoft.HDInsight/clusters/updateGatewaySettings/action | Update gateway settings for HDInsight Cluster |
> | Eylem | Microsoft.HDInsight/clusters/write | Create or Update HDInsight Cluster |
> | Eylem | Microsoft.HDInsight/locations/capabilities/read | Get Subscription Capabilities |
> | Eylem | Microsoft.HDInsight/locations/checkNameAvailability/read | Check Name Availability |
> | Eylem | Microsoft.HDInsight/register/action | Register HDInsight resource provider for the subscription |
> | Eylem | Microsoft.HDInsight/unregister/action | Unregister HDInsight resource provider for the subscription |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ImportExport/jobs/delete | Deletes an existing job. |
> | Eylem | Microsoft.ImportExport/jobs/listBitLockerKeys/action | Gets the BitLocker keys for the specified job. |
> | Eylem | Microsoft.ImportExport/jobs/read | Gets the properties for the specified job or returns the list of jobs. |
> | Eylem | Microsoft.ImportExport/jobs/write | Creates a job with the specified parameters or update the properties or tags for the specified job. |
> | Eylem | Microsoft.ImportExport/locations/read | Gets the properties for the specified location or returns the list of locations. |
> | Eylem | Microsoft.ImportExport/operations/read | Gets the operations supported by the Resource Provider. |
> | Eylem | Microsoft.ImportExport/register/action | Registers the subscription for the import/export resource provider and enables the creation of import/export jobs. |

## <a name="microsoftinsights"></a>Microsoft.Insights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Insights/ActionGroups/Delete | Delete an action group |
> | Eylem | Microsoft.Insights/ActionGroups/Read | Read an action group |
> | Eylem | Microsoft.Insights/ActionGroups/Write | Create or update an action group |
> | Eylem | Microsoft.Insights/ActivityLogAlerts/Activated/Action | Activity Log Alert activated |
> | Eylem | Microsoft.Insights/ActivityLogAlerts/Delete | Delete an activity log alert |
> | Eylem | Microsoft.Insights/ActivityLogAlerts/Read | Read an activity log alert |
> | Eylem | Microsoft.Insights/ActivityLogAlerts/Write | Create or update an activity log alert |
> | Eylem | Microsoft.Insights/AlertRules/Activated/Action | Classic metric alert activated |
> | Eylem | Microsoft.Insights/AlertRules/Delete | Delete a classic metric alert |
> | Eylem | Microsoft.Insights/AlertRules/Incidents/Read | Read a classic metric alert incident |
> | Eylem | Microsoft.Insights/AlertRules/Read | Read a classic metric alert |
> | Eylem | Microsoft.Insights/AlertRules/Resolved/Action | Classic metric alert resolved |
> | Eylem | Microsoft.Insights/AlertRules/Throttled/Action | Classic metric alert rule throttled |
> | Eylem | Microsoft.Insights/AlertRules/Write | Create or update a classic metric alert |
> | Eylem | Microsoft.Insights/AutoscaleSettings/Delete | Delete an autoscale setting |
> | Eylem | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/diagnosticSettings/Read | Read a resource diagnostic setting |
> | Eylem | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/diagnosticSettings/Write | Create or update a resource diagnostic setting |
> | Eylem | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/logDefinitions/Read | Read log definitions |
> | Eylem | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read | Read metric definitions |
> | Eylem | Microsoft.Insights/AutoscaleSettings/Read | Read an autoscale setting |
> | Eylem | Microsoft.Insights/AutoscaleSettings/Scaledown/Action | Autoscale scale down initiated |
> | Eylem | Microsoft.Insights/AutoscaleSettings/ScaledownResult/Action | Autoscale scale down completed |
> | Eylem | Microsoft.Insights/AutoscaleSettings/Scaleup/Action | Autoscale scale up initiated |
> | Eylem | Microsoft.Insights/AutoscaleSettings/ScaleupResult/Action | Autoscale scale up completed |
> | Eylem | Microsoft.Insights/AutoscaleSettings/Write | Create or update an autoscale setting |
> | Eylem | Microsoft.Insights/Baseline/Read | Read a metric baseline (preview) |
> | Eylem | Microsoft.Insights/CalculateBaseline/Read | Calculate baseline for metric values (preview) |
> | Eylem | Microsoft.Insights/Components/AnalyticsItems/Delete | Deleting an Application Insights analytics item |
> | Eylem | Microsoft.Insights/Components/AnalyticsItems/Read | Reading an Application Insights analytics item |
> | Eylem | Microsoft.Insights/Components/AnalyticsItems/Write | Writing an Application Insights analytics item |
> | Eylem | Microsoft.Insights/Components/AnalyticsTables/Action | Application Insights analytics table action |
> | Eylem | Microsoft.Insights/Components/AnalyticsTables/Delete | Deleting an Application Insights analytics table schema |
> | Eylem | Microsoft.Insights/Components/AnalyticsTables/Read | Reading an Application Insights analytics table schema |
> | Eylem | Microsoft.Insights/Components/AnalyticsTables/Write | Writing an Application Insights analytics table schema |
> | Eylem | Microsoft.Insights/Components/Annotations/Delete | Deleting an Application Insights annotation |
> | Eylem | Microsoft.Insights/Components/Annotations/Read | Reading an Application Insights annotation |
> | Eylem | Microsoft.Insights/Components/Annotations/Write | Writing an Application Insights annotation |
> | Eylem | Microsoft.Insights/Components/Api/Read | Reading Application Insights component data API |
> | Eylem | Microsoft.Insights/Components/ApiKeys/Action | Generating an Application Insights API key |
> | Eylem | Microsoft.Insights/Components/ApiKeys/Delete | Deleting an Application Insights API key |
> | Eylem | Microsoft.Insights/Components/ApiKeys/Read | Reading an Application Insights API key |
> | Eylem | Microsoft.Insights/Components/BillingPlanForComponent/Read | Reading a billing plan for Application Insights component |
> | Eylem | Microsoft.Insights/Components/CurrentBillingFeatures/Read | Reading current billing features for Application Insights component |
> | Eylem | Microsoft.Insights/Components/CurrentBillingFeatures/Write | Writing current billing features for Application Insights component |
> | Eylem | Microsoft.Insights/Components/DailyCapReached/Action | Reached the daily cap for Application Insights component |
> | Eylem | Microsoft.Insights/Components/DailyCapWarningThresholdReached/Action | Reached the daily cap warning threshold for Application Insights component |
> | Eylem | Microsoft.Insights/Components/DefaultWorkItemConfig/Read | Reading an Application Insights default ALM integration configuration |
> | Eylem | Microsoft.Insights/Components/Delete | Deleting an application insights component configuration |
> | Eylem | Microsoft.Insights/Components/Events/Read | Get logs from Application Insights using OData query format |
> | Eylem | Microsoft.Insights/Components/ExportConfiguration/Action | Application Insights export settings action |
> | Eylem | Microsoft.Insights/Components/ExportConfiguration/Delete | Deleting Application Insights export settings |
> | Eylem | Microsoft.Insights/Components/ExportConfiguration/Read | Reading Application Insights export settings |
> | Eylem | Microsoft.Insights/Components/ExportConfiguration/Write | Writing Application Insights export settings |
> | Eylem | Microsoft.Insights/Components/ExtendQueries/Read | Reading Application Insights component extended query results |
> | Eylem | Microsoft.Insights/Components/Favorites/Delete | Deleting an Application Insights favorite |
> | Eylem | Microsoft.Insights/Components/Favorites/Read | Reading an Application Insights favorite |
> | Eylem | Microsoft.Insights/Components/Favorites/Write | Writing an Application Insights favorite |
> | Eylem | Microsoft.Insights/Components/FeatureCapabilities/Read | Reading Application Insights component feature capabilities |
> | Eylem | Microsoft.Insights/Components/GetAvailableBillingFeatures/Read | Reading Application Insights component available billing features |
> | Eylem | Microsoft.Insights/Components/GetToken/Read | Reading an Application Insights component token |
> | Eylem | Microsoft.Insights/Components/MetricDefinitions/Read | Reading Application Insights component metric definitions |
> | Eylem | Microsoft.Insights/Components/Metrics/Read | Reading Application Insights component metrics |
> | Eylem | Microsoft.Insights/Components/Move/Action | Move an Application Insights Component to another resource group or subscription |
> | Eylem | Microsoft.Insights/Components/MyAnalyticsItems/Delete | Deleting an Application Insights personal analytics item |
> | Eylem | Microsoft.Insights/Components/MyAnalyticsItems/Read | Reading an Application Insights personal analytics item |
> | Eylem | Microsoft.Insights/Components/MyAnalyticsItems/Write | Writing an Application Insights personal analytics item |
> | Eylem | Microsoft.Insights/Components/MyFavorites/Read | Reading an Application Insights personal favorite |
> | Eylem | Microsoft.Insights/Components/Operations/Read | Get status of long-running operations in Application Insights |
> | Eylem | Microsoft.Insights/Components/PricingPlans/Read | Reading an Application Insights component pricing plan |
> | Eylem | Microsoft.Insights/Components/PricingPlans/Write | Writing an Application Insights component pricing plan |
> | Eylem | Microsoft.Insights/Components/ProactiveDetectionConfigs/Read | Reading Application Insights proactive detection configuration |
> | Eylem | Microsoft.Insights/Components/ProactiveDetectionConfigs/Write | Writing Application Insights proactive detection configuration |
> | Eylem | Microsoft.Insights/Components/providers/Microsoft.Insights/MetricDefinitions/Read | Read metric definitions |
> | Eylem | Microsoft.Insights/Components/Purge/Action | Purging data from Application Insights |
> | Eylem | Microsoft.Insights/Components/Query/Read | Run queries against Application Insights logs |
> | Eylem | Microsoft.Insights/Components/QuotaStatus/Read | Reading Application Insights component quota status |
> | Eylem | Microsoft.Insights/Components/Read | Reading an application insights component configuration |
> | Eylem | Microsoft.Insights/Components/SyntheticMonitorLocations/Read | Reading Application Insights webtest locations |
> | Eylem | Microsoft.Insights/Components/Webtests/Read | Reading a webtest configuration |
> | Eylem | Microsoft.Insights/Components/WorkItemConfigs/Delete | Deleting an Application Insights ALM integration configuration |
> | Eylem | Microsoft.Insights/Components/WorkItemConfigs/Read | Reading an Application Insights ALM integration configuration |
> | Eylem | Microsoft.Insights/Components/WorkItemConfigs/Write | Writing an Application Insights ALM integration configuration |
> | Eylem | Microsoft.Insights/Components/Write | Writing to an application insights component configuration |
> | Eylem | Microsoft.Insights/DataCollectionRuleAssociations/Delete | Delete a resource's association with a data collection rule |
> | Eylem | Microsoft.Insights/DataCollectionRuleAssociations/Read | Read a resource's association with a data collection rule |
> | Eylem | Microsoft.Insights/DataCollectionRuleAssociations/Write | Create or update a resource's association with a data collection rule |
> | DataAction | Microsoft.Insights/DataCollectionRules/Data/Write | Send data to a data collection rule |
> | Eylem | Microsoft.Insights/DataCollectionRules/Delete | Delete a data collection rule |
> | Eylem | Microsoft.Insights/DataCollectionRules/Read | Read a data collection rule |
> | Eylem | Microsoft.Insights/DataCollectionRules/Write | Create or update a data collection rule |
> | Eylem | Microsoft.Insights/DiagnosticSettings/Delete | Delete a resource diagnostic setting |
> | Eylem | Microsoft.Insights/DiagnosticSettings/Read | Read a resource diagnostic setting |
> | Eylem | Microsoft.Insights/DiagnosticSettings/Write | Create or update a resource diagnostic setting |
> | Eylem | Microsoft.Insights/EventCategories/Read | Read available Activity Log event categories |
> | Eylem | Microsoft.Insights/eventtypes/digestevents/Read | Read management event type digest |
> | Eylem | Microsoft.Insights/eventtypes/values/Read | Read Activity Log events |
> | Eylem | Microsoft.Insights/ExtendedDiagnosticSettings/Delete | Delete a network flow log diagnostic setting |
> | Eylem | Microsoft.Insights/ExtendedDiagnosticSettings/Read | Read a network flow log diagnostic setting |
> | Eylem | Microsoft.Insights/ExtendedDiagnosticSettings/Write | Create or update a network flow log diagnostic setting |
> | Eylem | Microsoft.Insights/ListMigrationDate/Action | Get back Subscription migration date |
> | Eylem | Microsoft.Insights/ListMigrationDate/Read | Get back subscription migration date |
> | Eylem | Microsoft.Insights/LogDefinitions/Read | Read log definitions |
> | Eylem | Microsoft.Insights/LogProfiles/Delete | Delete an Activity Log log profile |
> | Eylem | Microsoft.Insights/LogProfiles/Read | Read an Activity Log log profile |
> | Eylem | Microsoft.Insights/LogProfiles/Write | Create or update an Activity Log log profile |
> | Eylem | Microsoft.Insights/Logs/ADAssessmentRecommendation/Read | Read data from the ADAssessmentRecommendation table |
> | Eylem | Microsoft.Insights/Logs/ADReplicationResult/Read | Read data from the ADReplicationResult table |
> | Eylem | Microsoft.Insights/Logs/ADSecurityAssessmentRecommendation/Read | Read data from the ADSecurityAssessmentRecommendation table |
> | Eylem | Microsoft.Insights/Logs/Alert/Read | Read data from the Alert table |
> | Eylem | Microsoft.Insights/Logs/AlertHistory/Read | Read data from the AlertHistory table |
> | Eylem | Microsoft.Insights/Logs/ApplicationInsights/Read | Read data from the ApplicationInsights table |
> | Eylem | Microsoft.Insights/Logs/AzureActivity/Read | Read data from the AzureActivity table |
> | Eylem | Microsoft.Insights/Logs/AzureMetrics/Read | Read data from the AzureMetrics table |
> | Eylem | Microsoft.Insights/Logs/BoundPort/Read | Read data from the BoundPort table |
> | Eylem | Microsoft.Insights/Logs/CommonSecurityLog/Read | Read data from the CommonSecurityLog table |
> | Eylem | Microsoft.Insights/Logs/ComputerGroup/Read | Read data from the ComputerGroup table |
> | Eylem | Microsoft.Insights/Logs/ConfigurationChange/Read | Read data from the ConfigurationChange table |
> | Eylem | Microsoft.Insights/Logs/ConfigurationData/Read | Read data from the ConfigurationData table |
> | Eylem | Microsoft.Insights/Logs/ContainerImageInventory/Read | Read data from the ContainerImageInventory table |
> | Eylem | Microsoft.Insights/Logs/ContainerInventory/Read | Read data from the ContainerInventory table |
> | Eylem | Microsoft.Insights/Logs/ContainerLog/Read | Read data from the ContainerLog table |
> | Eylem | Microsoft.Insights/Logs/ContainerServiceLog/Read | Read data from the ContainerServiceLog table |
> | Eylem | Microsoft.Insights/Logs/DeviceAppCrash/Read | Read data from the DeviceAppCrash table |
> | Eylem | Microsoft.Insights/Logs/DeviceAppLaunch/Read | Read data from the DeviceAppLaunch table |
> | Eylem | Microsoft.Insights/Logs/DeviceCalendar/Read | Read data from the DeviceCalendar table |
> | Eylem | Microsoft.Insights/Logs/DeviceCleanup/Read | Read data from the DeviceCleanup table |
> | Eylem | Microsoft.Insights/Logs/DeviceConnectSession/Read | Read data from the DeviceConnectSession table |
> | Eylem | Microsoft.Insights/Logs/DeviceEtw/Read | Read data from the DeviceEtw table |
> | Eylem | Microsoft.Insights/Logs/DeviceHardwareHealth/Read | Read data from the DeviceHardwareHealth table |
> | Eylem | Microsoft.Insights/Logs/DeviceHealth/Read | Read data from the DeviceHealth table |
> | Eylem | Microsoft.Insights/Logs/DeviceHeartbeat/Read | Read data from the DeviceHeartbeat table |
> | Eylem | Microsoft.Insights/Logs/DeviceSkypeHeartbeat/Read | Read data from the DeviceSkypeHeartbeat table |
> | Eylem | Microsoft.Insights/Logs/DeviceSkypeSignIn/Read | Read data from the DeviceSkypeSignIn table |
> | Eylem | Microsoft.Insights/Logs/DeviceSleepState/Read | Read data from the DeviceSleepState table |
> | Eylem | Microsoft.Insights/Logs/DHAppFailure/Read | Read data from the DHAppFailure table |
> | Eylem | Microsoft.Insights/Logs/DHAppReliability/Read | Read data from the DHAppReliability table |
> | Eylem | Microsoft.Insights/Logs/DHDriverReliability/Read | Read data from the DHDriverReliability table |
> | Eylem | Microsoft.Insights/Logs/DHLogonFailures/Read | Read data from the DHLogonFailures table |
> | Eylem | Microsoft.Insights/Logs/DHLogonMetrics/Read | Read data from the DHLogonMetrics table |
> | Eylem | Microsoft.Insights/Logs/DHOSCrashData/Read | Read data from the DHOSCrashData table |
> | Eylem | Microsoft.Insights/Logs/DHOSReliability/Read | Read data from the DHOSReliability table |
> | Eylem | Microsoft.Insights/Logs/DHWipAppLearning/Read | Read data from the DHWipAppLearning table |
> | Eylem | Microsoft.Insights/Logs/DnsEvents/Read | Read data from the DnsEvents table |
> | Eylem | Microsoft.Insights/Logs/DnsInventory/Read | Read data from the DnsInventory table |
> | Eylem | Microsoft.Insights/Logs/ETWEvent/Read | Read data from the ETWEvent table |
> | Eylem | Microsoft.Insights/Logs/Event/Read | Read data from the Event table |
> | Eylem | Microsoft.Insights/Logs/ExchangeAssessmentRecommendation/Read | Read data from the ExchangeAssessmentRecommendation table |
> | Eylem | Microsoft.Insights/Logs/ExchangeOnlineAssessmentRecommendation/Read | Read data from the ExchangeOnlineAssessmentRecommendation table |
> | Eylem | Microsoft.Insights/Logs/Heartbeat/Read | Read data from the Heartbeat table |
> | Eylem | Microsoft.Insights/Logs/IISAssessmentRecommendation/Read | Read data from the IISAssessmentRecommendation table |
> | Eylem | Microsoft.Insights/Logs/InboundConnection/Read | Read data from the InboundConnection table |
> | Eylem | Microsoft.Insights/Logs/KubeNodeInventory/Read | Read data from the KubeNodeInventory table |
> | Eylem | Microsoft.Insights/Logs/KubePodInventory/Read | Read data from the KubePodInventory table |
> | Eylem | Microsoft.Insights/Logs/LinuxAuditLog/Read | Read data from the LinuxAuditLog table |
> | Eylem | Microsoft.Insights/Logs/MAApplication/Read | Read data from the MAApplication table |
> | Eylem | Microsoft.Insights/Logs/MAApplicationHealth/Read | Read data from the MAApplicationHealth table |
> | Eylem | Microsoft.Insights/Logs/MAApplicationHealthAlternativeVersions/Read | Read data from the MAApplicationHealthAlternativeVersions table |
> | Eylem | Microsoft.Insights/Logs/MAApplicationHealthIssues/Read | Read data from the MAApplicationHealthIssues table |
> | Eylem | Microsoft.Insights/Logs/MAApplicationInstance/Read | Read data from the MAApplicationInstance table |
> | Eylem | Microsoft.Insights/Logs/MAApplicationInstanceReadiness/Read | Read data from the MAApplicationInstanceReadiness table |
> | Eylem | Microsoft.Insights/Logs/MAApplicationReadiness/Read | Read data from the MAApplicationReadiness table |
> | Eylem | Microsoft.Insights/Logs/MADeploymentPlan/Read | Read data from the MADeploymentPlan table |
> | Eylem | Microsoft.Insights/Logs/MADevice/Read | Read data from the MADevice table |
> | Eylem | Microsoft.Insights/Logs/MADevicePnPHealth/Read | Read data from the MADevicePnPHealth table |
> | Eylem | Microsoft.Insights/Logs/MADevicePnPHealthAlternativeVersions/Read | Read data from the MADevicePnPHealthAlternativeVersions table |
> | Eylem | Microsoft.Insights/Logs/MADevicePnPHealthIssues/Read | Read data from the MADevicePnPHealthIssues table |
> | Eylem | Microsoft.Insights/Logs/MADeviceReadiness/Read | Read data from the MADeviceReadiness table |
> | Eylem | Microsoft.Insights/Logs/MADriverInstanceReadiness/Read | Read data from the MADriverInstanceReadiness table |
> | Eylem | Microsoft.Insights/Logs/MADriverReadiness/Read | Read data from the MADriverReadiness table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAddin/Read | Read data from the MAOfficeAddin table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAddinHealth/Read | Read data from the MAOfficeAddinHealth table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAddinHealthIssues/Read | Read data from the MAOfficeAddinHealthIssues table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAddinInstance/Read | Read data from the MAOfficeAddinInstance table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAddinInstanceReadiness/Read | Read data from the MAOfficeAddinInstanceReadiness table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAddinReadiness/Read | Read data from the MAOfficeAddinReadiness table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeApp/Read | Read data from the MAOfficeApp table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAppHealth/Read | Read data from the MAOfficeAppHealth table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAppInstance/Read | Read data from the MAOfficeAppInstance table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeAppReadiness/Read | Read data from the MAOfficeAppReadiness table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeBuildInfo/Read | Read data from the MAOfficeBuildInfo table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeCurrencyAssessment/Read | Read data from the MAOfficeCurrencyAssessment table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeCurrencyAssessmentDailyCounts/Read | Read data from the MAOfficeCurrencyAssessmentDailyCounts table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeDeploymentStatus/Read | Read data from the MAOfficeDeploymentStatus table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeMacroHealth/Read | Read data from the MAOfficeMacroHealth table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeMacroHealthIssues/Read | Read data from the MAOfficeMacroHealthIssues table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeMacroIssueInstanceReadiness/Read | Read data from the MAOfficeMacroIssueInstanceReadiness table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeMacroIssueReadiness/Read | Read data from the MAOfficeMacroIssueReadiness table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeMacroSummary/Read | Read data from the MAOfficeMacroSummary table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeSuite/Read | Read data from the MAOfficeSuite table |
> | Eylem | Microsoft.Insights/Logs/MAOfficeSuiteInstance/Read | Read data from the MAOfficeSuiteInstance table |
> | Eylem | Microsoft.Insights/Logs/MAProposedPilotDevices/Read | Read data from the MAProposedPilotDevices table |
> | Eylem | Microsoft.Insights/Logs/MAWindowsBuildInfo/Read | Read data from the MAWindowsBuildInfo table |
> | Eylem | Microsoft.Insights/Logs/MAWindowsCurrencyAssessment/Read | Read data from the MAWindowsCurrencyAssessment table |
> | Eylem | Microsoft.Insights/Logs/MAWindowsCurrencyAssessmentDailyCounts/Read | Read data from the MAWindowsCurrencyAssessmentDailyCounts table |
> | Eylem | Microsoft.Insights/Logs/MAWindowsDeploymentStatus/Read | Read data from the MAWindowsDeploymentStatus table |
> | Eylem | Microsoft.Insights/Logs/MAWindowsSysReqInstanceReadiness/Read | Read data from the MAWindowsSysReqInstanceReadiness table |
> | Eylem | Microsoft.Insights/Logs/NetworkMonitoring/Read | Read data from the NetworkMonitoring table |
> | Eylem | Microsoft.Insights/Logs/OfficeActivity/Read | Read data from the OfficeActivity table |
> | Eylem | Microsoft.Insights/Logs/Operation/Read | Read data from the Operation table |
> | Eylem | Microsoft.Insights/Logs/OutboundConnection/Read | Read data from the OutboundConnection table |
> | Eylem | Microsoft.Insights/Logs/Perf/Read | Read data from the Perf table |
> | Eylem | Microsoft.Insights/Logs/ProtectionStatus/Read | Read data from the ProtectionStatus table |
> | Eylem | Microsoft.Insights/Logs/Read | Reading data from all your logs |
> | Eylem | Microsoft.Insights/Logs/ReservedAzureCommonFields/Read | Read data from the ReservedAzureCommonFields table |
> | Eylem | Microsoft.Insights/Logs/ReservedCommonFields/Read | Read data from the ReservedCommonFields table |
> | Eylem | Microsoft.Insights/Logs/SCCMAssessmentRecommendation/Read | Read data from the SCCMAssessmentRecommendation table |
> | Eylem | Microsoft.Insights/Logs/SCOMAssessmentRecommendation/Read | Read data from the SCOMAssessmentRecommendation table |
> | Eylem | Microsoft.Insights/Logs/SecurityAlert/Read | Read data from the SecurityAlert table |
> | Eylem | Microsoft.Insights/Logs/SecurityBaseline/Read | Read data from the SecurityBaseline table |
> | Eylem | Microsoft.Insights/Logs/SecurityBaselineSummary/Read | Read data from the SecurityBaselineSummary table |
> | Eylem | Microsoft.Insights/Logs/SecurityDetection/Read | Read data from the SecurityDetection table |
> | Eylem | Microsoft.Insights/Logs/SecurityEvent/Read | Read data from the SecurityEvent table |
> | Eylem | Microsoft.Insights/Logs/ServiceFabricOperationalEvent/Read | Read data from the ServiceFabricOperationalEvent table |
> | Eylem | Microsoft.Insights/Logs/ServiceFabricReliableActorEvent/Read | Read data from the ServiceFabricReliableActorEvent table |
> | Eylem | Microsoft.Insights/Logs/ServiceFabricReliableServiceEvent/Read | Read data from the ServiceFabricReliableServiceEvent table |
> | Eylem | Microsoft.Insights/Logs/SfBAssessmentRecommendation/Read | Read data from the SfBAssessmentRecommendation table |
> | Eylem | Microsoft.Insights/Logs/SfBOnlineAssessmentRecommendation/Read | Read data from the SfBOnlineAssessmentRecommendation table |
> | Eylem | Microsoft.Insights/Logs/SharePointOnlineAssessmentRecommendation/Read | Read data from the SharePointOnlineAssessmentRecommendation table |
> | Eylem | Microsoft.Insights/Logs/SPAssessmentRecommendation/Read | Read data from the SPAssessmentRecommendation table |
> | Eylem | Microsoft.Insights/Logs/SQLAssessmentRecommendation/Read | Read data from the SQLAssessmentRecommendation table |
> | Eylem | Microsoft.Insights/Logs/SQLQueryPerformance/Read | Read data from the SQLQueryPerformance table |
> | Eylem | Microsoft.Insights/Logs/Syslog/Read | Read data from the Syslog table |
> | Eylem | Microsoft.Insights/Logs/SysmonEvent/Read | Read data from the SysmonEvent table |
> | Eylem | Microsoft.Insights/Logs/Tables.Custom/Read | Reading data from any custom log |
> | Eylem | Microsoft.Insights/Logs/UAApp/Read | Read data from the UAApp table |
> | Eylem | Microsoft.Insights/Logs/UAComputer/Read | Read data from the UAComputer table |
> | Eylem | Microsoft.Insights/Logs/UAComputerRank/Read | Read data from the UAComputerRank table |
> | Eylem | Microsoft.Insights/Logs/UADriver/Read | Read data from the UADriver table |
> | Eylem | Microsoft.Insights/Logs/UADriverProblemCodes/Read | Read data from the UADriverProblemCodes table |
> | Eylem | Microsoft.Insights/Logs/UAFeedback/Read | Read data from the UAFeedback table |
> | Eylem | Microsoft.Insights/Logs/UAHardwareSecurity/Read | Read data from the UAHardwareSecurity table |
> | Eylem | Microsoft.Insights/Logs/UAIESiteDiscovery/Read | Read data from the UAIESiteDiscovery table |
> | Eylem | Microsoft.Insights/Logs/UAOfficeAddIn/Read | Read data from the UAOfficeAddIn table |
> | Eylem | Microsoft.Insights/Logs/UAProposedActionPlan/Read | Read data from the UAProposedActionPlan table |
> | Eylem | Microsoft.Insights/Logs/UASysReqIssue/Read | Read data from the UASysReqIssue table |
> | Eylem | Microsoft.Insights/Logs/UAUpgradedComputer/Read | Read data from the UAUpgradedComputer table |
> | Eylem | Microsoft.Insights/Logs/Update/Read | Read data from the Update table |
> | Eylem | Microsoft.Insights/Logs/UpdateRunProgress/Read | Read data from the UpdateRunProgress table |
> | Eylem | Microsoft.Insights/Logs/UpdateSummary/Read | Read data from the UpdateSummary table |
> | Eylem | Microsoft.Insights/Logs/Usage/Read | Read data from the Usage table |
> | Eylem | Microsoft.Insights/Logs/W3CIISLog/Read | Read data from the W3CIISLog table |
> | Eylem | Microsoft.Insights/Logs/WaaSDeploymentStatus/Read | Read data from the WaaSDeploymentStatus table |
> | Eylem | Microsoft.Insights/Logs/WaaSInsiderStatus/Read | Read data from the WaaSInsiderStatus table |
> | Eylem | Microsoft.Insights/Logs/WaaSUpdateStatus/Read | Read data from the WaaSUpdateStatus table |
> | Eylem | Microsoft.Insights/Logs/WDAVStatus/Read | Read data from the WDAVStatus table |
> | Eylem | Microsoft.Insights/Logs/WDAVThreat/Read | Read data from the WDAVThreat table |
> | Eylem | Microsoft.Insights/Logs/WindowsClientAssessmentRecommendation/Read | Read data from the WindowsClientAssessmentRecommendation table |
> | Eylem | Microsoft.Insights/Logs/WindowsFirewall/Read | Read data from the WindowsFirewall table |
> | Eylem | Microsoft.Insights/Logs/WindowsServerAssessmentRecommendation/Read | Read data from the WindowsServerAssessmentRecommendation table |
> | Eylem | Microsoft.Insights/Logs/WireData/Read | Read data from the WireData table |
> | Eylem | Microsoft.Insights/Logs/WUDOAggregatedStatus/Read | Read data from the WUDOAggregatedStatus table |
> | Eylem | Microsoft.Insights/Logs/WUDOStatus/Read | Read data from the WUDOStatus table |
> | Eylem | Microsoft.Insights/MetricAlerts/Delete | Delete a metric alert |
> | Eylem | Microsoft.Insights/MetricAlerts/Read | Read a metric alert |
> | Eylem | Microsoft.Insights/MetricAlerts/Status/Read | Read metric alert status |
> | Eylem | Microsoft.Insights/MetricAlerts/Write | Create or update a metric alert |
> | Eylem | Microsoft.Insights/MetricBaselines/Read | Read metric baselines |
> | Eylem | Microsoft.Insights/MetricDefinitions/Microsoft.Insights/Read | Read metric definitions |
> | Eylem | Microsoft.Insights/MetricDefinitions/providers/Microsoft.Insights/Read | Read metric definitions |
> | Eylem | Microsoft.Insights/MetricDefinitions/Read | Read metric definitions |
> | Eylem | Microsoft.Insights/Metricnamespaces/Read | Read metric namespaces |
> | Eylem | Microsoft.Insights/Metrics/Action | Metric Action |
> | Eylem | Microsoft.Insights/Metrics/Microsoft.Insights/Read | Read metrics |
> | Eylem | Microsoft.Insights/Metrics/providers/Metrics/Read | Read metrics |
> | Eylem | Microsoft.Insights/Metrics/Read | Read metrics |
> | DataAction | Microsoft.Insights/Metrics/Write | Write metrics |
> | Eylem | Microsoft.Insights/MigrateToNewpricingModel/Action | Migrate subscription to new pricing model |
> | Eylem | Microsoft.Insights/Operations/Read | Okuma İşlemleri |
> | Eylem | Microsoft.Insights/Register/Action | Register the Microsoft Insights provider |
> | Eylem | Microsoft.Insights/RollbackToLegacyPricingModel/Action | Rollback subscription to legacy pricing model |
> | Eylem | Microsoft.Insights/ScheduledQueryRules/Delete | Deleting a scheduled query rule |
> | Eylem | Microsoft.Insights/ScheduledQueryRules/Read | Reading a scheduled query rule |
> | Eylem | Microsoft.Insights/ScheduledQueryRules/Write | Writing a scheduled query rule |
> | Eylem | Microsoft.Insights/Tenants/Register/Action | Initializes the Microsoft Insights provider |
> | Eylem | Microsoft.Insights/Unregister/Action | Register the Microsoft Insights provider |
> | Eylem | Microsoft.Insights/Webtests/Delete | Deleting a webtest configuration |
> | Eylem | Microsoft.Insights/Webtests/GetToken/Read | Reading a webtest token |
> | Eylem | Microsoft.Insights/Webtests/MetricDefinitions/Read | Reading a webtest metric definitions |
> | Eylem | Microsoft.Insights/Webtests/Metrics/Read | Reading a webtest metrics |
> | Eylem | Microsoft.Insights/Webtests/Read | Reading a webtest configuration |
> | Eylem | Microsoft.Insights/Webtests/Write | Writing to a webtest configuration |
> | Eylem | Microsoft.Insights/Workbooks/Delete | Bir çalışma kitabını silme |
> | Eylem | Microsoft.Insights/Workbooks/Read | Read a workbook |
> | Eylem | Microsoft.Insights/Workbooks/Write | Create or update a workbook |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Intune/diagnosticsettings/delete | Deleting a diagnostic setting |
> | Eylem | Microsoft.Intune/diagnosticsettings/read | Reading a diagnostic setting |
> | Eylem | Microsoft.Intune/diagnosticsettings/write | Writing a diagnostic setting |
> | Eylem | Microsoft.Intune/diagnosticsettingscategories/read | Reading a diagnostic setting categories |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.IoTCentral/appTemplates/action | Gets all the available application templates on Azure IoT Central |
> | Eylem | Microsoft.IoTCentral/checkNameAvailability/action | Checks if an IoT Central Application name is available |
> | Eylem | Microsoft.IoTCentral/checkSubdomainAvailability/action | Checks if an IoT Central Application subdomain is available |
> | Eylem | Microsoft.IoTCentral/IoTApps/delete | Deletes an IoT Central Applications |
> | Eylem | Microsoft.IoTCentral/IoTApps/read | Gets a single IoT Central Application |
> | Eylem | Microsoft.IoTCentral/IoTApps/write | Creates or Updates an IoT Central Applications |
> | Eylem | Microsoft.IoTCentral/operations/read | Gets all the available operations on IoT Central Applications |
> | Eylem | Microsoft.IoTCentral/register/action | Register the subscription for Azure IoT Central resource provider |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.IoTSpaces/Graph/delete | Deletes Microsoft.IoTSpaces Graph resource |
> | Eylem | Microsoft.IoTSpaces/Graph/read | Gets the Microsoft.IoTSpaces Graph resource(s) |
> | Eylem | Microsoft.IoTSpaces/Graph/write | Create Microsoft.IoTSpaces Graph resource |
> | Eylem | Microsoft.IoTSpaces/register/action | Register subscription for Microsoft.IoTSpaces Graph resource provider to enable creating of resources |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.KeyVault/checkNameAvailability/read | Checks that a key vault name is valid and is not in use |
> | Eylem | Microsoft.KeyVault/deletedVaults/read | View the properties of soft deleted key vaults |
> | Eylem | Microsoft.KeyVault/hsmPools/delete | Delete an HSM pool |
> | Eylem | Microsoft.KeyVault/hsmPools/joinVault/action | Join a key vault to an HSM pool |
> | Eylem | Microsoft.KeyVault/hsmPools/read | View the properties of an HSM pool |
> | Eylem | Microsoft.KeyVault/hsmPools/write | Create a new HSM pool of update the properties of an existing HSM pool |
> | Eylem | Microsoft.KeyVault/locations/deletedVaults/purge/action | Purge a soft deleted key vault |
> | Eylem | Microsoft.KeyVault/locations/deletedVaults/read | View the properties of a soft deleted key vault |
> | Eylem | Microsoft.KeyVault/locations/deleteVirtualNetworkOrSubnets/action | Notifies Microsoft.KeyVault that a virtual network or subnet is being deleted |
> | Eylem | Microsoft.KeyVault/locations/operationResults/read | Check the result of a long run operation |
> | Eylem | Microsoft.KeyVault/operations/read | Lists operations available on Microsoft.KeyVault resource provider |
> | Eylem | Microsoft.KeyVault/register/action | Registers a subscription |
> | Eylem | Microsoft.KeyVault/unregister/action | Unregisters a subscription |
> | Eylem | Microsoft.KeyVault/vaults/accessPolicies/write | Update an existing access policy by merging or replacing, or add a new access policy to a vault. |
> | Eylem | Microsoft.KeyVault/vaults/delete | Bir anahtar kasasını silme |
> | Eylem | Microsoft.KeyVault/vaults/deploy/action | Enables access to secrets in a key vault when deploying Azure resources |
> | Eylem | Microsoft.KeyVault/vaults/eventGridFilters/delete | Notifies Microsoft.KeyVault that an EventGrid Subscription for Key Vault is being deleted |
> | Eylem | Microsoft.KeyVault/vaults/eventGridFilters/read | Notifies Microsoft.KeyVault that an EventGrid Subscription for Key Vault is being viewed |
> | Eylem | Microsoft.KeyVault/vaults/eventGridFilters/write | Notifies Microsoft.KeyVault that a new EventGrid Subscription for Key Vault is being created |
> | Eylem | Microsoft.KeyVault/vaults/read | View the properties of a key vault |
> | Eylem | Microsoft.KeyVault/vaults/secrets/read | View the properties of a secret, but not its value. |
> | Eylem | Microsoft.KeyVault/vaults/secrets/write | Create a new secret or update the value of an existing secret. |
> | Eylem | Microsoft.KeyVault/vaults/write | Create a new key vault or update the properties of an existing key vault |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Kusto/Clusters/Activate/action | Starts the cluster. |
> | Eylem | Microsoft.Kusto/Clusters/AttachedDatabaseConfigurations/delete | Deletes an attached database configuration resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/AttachedDatabaseConfigurations/read | Reads an attached database configuration resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/AttachedDatabaseConfigurations/write | Writes an attached database configuration resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/CheckNameAvailability/action | Checks the cluster name availability. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/AddPrincipals/action | Adds database principals. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/CheckNameAvailability/action | Checks name availability for a given type. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/DataConnections/delete | Deletes a data connections resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/DataConnections/read | Reads a data connections resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/DataConnections/write | Writes a data connections resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/DataConnectionValidation/action | Validates database data connection. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/delete | Deletes a database resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/EventHubConnections/delete | Deletes an Event Hub connections resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/EventHubConnections/read | Reads an Event Hub connections resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/EventHubConnections/write | Writes an Event Hub connections resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/EventHubConnectionValidation/action | Validates database Event Hub connection. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/ListPrincipals/action | Lists database principals. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/PrincipalAssignments/delete | Deletes a database principal assignments resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/PrincipalAssignments/read | Reads a database principal assignments resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/PrincipalAssignments/write | Writes a database principal assignments resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/read | Reads a database resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/RemovePrincipals/action | Removes database principals. |
> | Eylem | Microsoft.Kusto/Clusters/Databases/write | Writes a database resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/Deactivate/action | Stops the cluster. |
> | Eylem | Microsoft.Kusto/Clusters/delete | Deletes a cluster resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/DetachFollowerDatabases/action | Detaches follower's databases. |
> | Eylem | Microsoft.Kusto/Clusters/DiagnoseVirtualNetwork/action | Diagnoses network connectivity status for external resources on which the service is depedent on. |
> | Eylem | Microsoft.Kusto/Clusters/ListFollowerDatabases/action | Lists the follower's databases. |
> | Eylem | Microsoft.Kusto/Clusters/read | Reads a cluster resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/SKUs/read | Reads a cluster SKU resourceCopy. |
> | Eylem | Microsoft.Kusto/Clusters/Start/action | Starts the cluster. |
> | Eylem | Microsoft.Kusto/Clusters/Stop/action | Stops the cluster. |
> | Eylem | Microsoft.Kusto/Clusters/write | Writes a cluster resourceCopy. |
> | Eylem | Microsoft.Kusto/Locations/CheckNameAvailability/action | Checks resourceCopy name availability. |
> | Eylem | Microsoft.Kusto/Locations/GetNetworkPolicies/action | Gets Network Intent Policies |
> | Eylem | Microsoft.Kusto/locations/operationresults/read | Reads operations resourceCopys |
> | Eylem | Microsoft.Kusto/Operations/read | Reads operations resourceCopys |
> | Eylem | Microsoft.Kusto/register/action | Subscription Registration Action |
> | Eylem | Microsoft.Kusto/Register/action | Registers the subscription to the Kusto Resource Provider. |
> | Eylem | Microsoft.Kusto/SKUs/read | Reads a SKU resourceCopy. |
> | Eylem | Microsoft.Kusto/Unregister/action | Unregisters the subscription to the Kusto Resource Provider. |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.LabServices/labAccounts/CreateLab/action | Create a lab in a lab account. |
> | Eylem | Microsoft.LabServices/labAccounts/delete | Delete lab accounts. |
> | Eylem | Microsoft.LabServices/labAccounts/galleryImages/delete | Delete gallery images. |
> | Eylem | Microsoft.LabServices/labAccounts/galleryImages/read | Read gallery images. |
> | Eylem | Microsoft.LabServices/labAccounts/galleryImages/write | Add or modify gallery images. |
> | Eylem | Microsoft.LabServices/labAccounts/GetRegionalAvailability/action | Get regional availability information for each size category configured under a lab account |
> | Eylem | Microsoft.LabServices/labAccounts/labs/AddUsers/action | Add users to a lab |
> | Eylem | Microsoft.LabServices/labAccounts/labs/delete | Delete labs. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/delete | Delete environment setting. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/delete | Delete environments. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/read | Read environments. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/ResetPassword/action | Resets the user password on an environment |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Start/action | Starts an environment by starting all resources inside the environment. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Stop/action | Stops an environment by stopping all resources inside the environment |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/write | Add or modify environments. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/Publish/action | Provisions/deprovisions required resources for an environment setting based on current state of the lab/environment setting. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/read | Read environment setting. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/ResetPassword/action | Resets password on the template virtual machine. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/SaveImage/action | Saves current template image to the shared gallery in the lab account |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/delete | Delete schedules. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/read | Read schedules. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/write | Add or modify schedules. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/Start/action | Starts a template by starting all resources inside the template. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/Stop/action | Stops a template by stopping all resources inside the template. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/environmentSettings/write | Add or modify environment setting. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/read | Read labs. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/SendEmail/action | Send email with registration link to the lab |
> | Eylem | Microsoft.LabServices/labAccounts/labs/users/delete | Delete users. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/users/read | Read users. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/users/write | Add or modify users. |
> | Eylem | Microsoft.LabServices/labAccounts/labs/write | Add or modify labs. |
> | Eylem | Microsoft.LabServices/labAccounts/read | Read lab accounts. |
> | Eylem | Microsoft.LabServices/labAccounts/sharedGalleries/delete | Delete sharedgalleries. |
> | Eylem | Microsoft.LabServices/labAccounts/sharedGalleries/read | Read sharedgalleries. |
> | Eylem | Microsoft.LabServices/labAccounts/sharedGalleries/write | Add or modify sharedgalleries. |
> | Eylem | Microsoft.LabServices/labAccounts/sharedImages/delete | Delete sharedimages. |
> | Eylem | Microsoft.LabServices/labAccounts/sharedImages/read | Read sharedimages. |
> | Eylem | Microsoft.LabServices/labAccounts/sharedImages/write | Add or modify sharedimages. |
> | Eylem | Microsoft.LabServices/labAccounts/write | Add or modify lab accounts. |
> | Eylem | Microsoft.LabServices/locations/operations/read | Read operations. |
> | Eylem | Microsoft.LabServices/register/action | Registers the subscription |
> | Eylem | Microsoft.LabServices/users/ListAllEnvironments/action | List all Environments for the user |
> | Eylem | Microsoft.LabServices/users/Register/action | Register a user to a managed lab |
> | Eylem | Microsoft.LabServices/users/ResetPassword/action | Resets the user password on an environment |
> | Eylem | Microsoft.LabServices/users/StartEnvironment/action | Starts an environment by starting all resources inside the environment. |
> | Eylem | Microsoft.LabServices/users/StopEnvironment/action | Stops an environment by stopping all resources inside the environment |
> | Eylem | Microsoft.LabServices/users/UserSettings/action | Updates and returns personal user settings. |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Logic/integrationAccounts/agreements/delete | Deletes the agreement in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/agreements/listContentCallbackUrl/action | Gets the callback URL for agreement content in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/agreements/read | Reads the agreement in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/agreements/write | Creates or updates the agreement in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/assemblies/delete | Deletes the assembly in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/assemblies/listContentCallbackUrl/action | Gets the callback URL for assembly content in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/assemblies/read | Reads the assembly in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/assemblies/write | Creates or updates the assembly in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/batchConfigurations/delete | Deletes the batch configuration in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/batchConfigurations/read | Reads the batch configuration in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/batchConfigurations/write | Creates or updates the batch configuration in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/certificates/delete | Deletes the certificate in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/certificates/read | Reads the certificate in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/certificates/write | Creates or updates the certificate in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/delete | Deletes the integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/join/action | Joins the Integration Account. |
> | Eylem | Microsoft.Logic/integrationAccounts/listCallbackUrl/action | Gets the callback URL for integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/listKeyVaultKeys/action | Gets the keys in the key vault. |
> | Eylem | Microsoft.Logic/integrationAccounts/logTrackingEvents/action | Logs the tracking events in the integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/maps/delete | Deletes the map in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/maps/listContentCallbackUrl/action | Gets the callback URL for map content in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/maps/read | Reads the map in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/maps/write | Creates or updates the map in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/partners/delete | Deletes the partner in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/partners/listContentCallbackUrl/action | Gets the callback URL for partner content in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/partners/read | Reads the partner in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/partners/write | Creates or updates the partner in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/providers/Microsoft.Insights/logDefinitions/read | Reads the Integration Account log definitions. |
> | Eylem | Microsoft.Logic/integrationAccounts/read | Reads the integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/regenerateAccessKey/action | Regenerates the access key secrets. |
> | Eylem | Microsoft.Logic/integrationAccounts/rosettaNetProcessConfigurations/delete | Deletes the RosettaNet process configuration in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/rosettaNetProcessConfigurations/read | Reads the RosettaNet process configuration in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/rosettaNetProcessConfigurations/write | Creates or updates the  RosettaNet process configuration in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/schemas/delete | Deletes the schema in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/schemas/listContentCallbackUrl/action | Gets the callback URL for schema content in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/schemas/read | Reads the schema in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/schemas/write | Creates or updates the schema in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/sessions/delete | Deletes the session in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/sessions/read | Reads the batch configuration in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/sessions/write | Creates or updates the session in integration account. |
> | Eylem | Microsoft.Logic/integrationAccounts/write | Creates or updates the integration account. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/availableManagedApis/read | Reads the integration service environment available managed APIs. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/delete | Deletes the integration service environment. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/join/action | Joins the Integration Service Environment. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/managedApis/apiOperations/read | Reads the integration service environment managed API operation. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/managedApis/join/action | Joins the Integration Service Environment Managed API. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/managedApis/operationStatuses/read | Reads the integration service environment managed API operation statuses. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/managedApis/read | Reads the integration service environment managed API. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/managedApis/write | Creates or updates the integration service environment managed API. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/operationStatuses/read | Reads the integration service environment operation statuses. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/providers/Microsoft.Insights/metricDefinitions/read | Reads the integration service environment metric definitions. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/read | Reads the integration service environment. |
> | Eylem | Microsoft.Logic/integrationServiceEnvironments/write | Creates or updates the integration service environment. |
> | Eylem | Microsoft.Logic/locations/workflows/recommendOperationGroups/action | Gets the workflow recommend operation groups. |
> | Eylem | Microsoft.Logic/locations/workflows/validate/action | Validates the workflow. |
> | Eylem | Microsoft.Logic/operations/read | Gets the operation. |
> | Eylem | Microsoft.Logic/register/action | Registers the Microsoft.Logic resource provider for a given subscription. |
> | Eylem | Microsoft.Logic/workflows/accessKeys/delete | Deletes the access key. |
> | Eylem | Microsoft.Logic/workflows/accessKeys/list/action | Lists the access key secrets. |
> | Eylem | Microsoft.Logic/workflows/accessKeys/read | Reads the access key. |
> | Eylem | Microsoft.Logic/workflows/accessKeys/regenerate/action | Regenerates the access key secrets. |
> | Eylem | Microsoft.Logic/workflows/accessKeys/write | Creates or updates the access key. |
> | Eylem | Microsoft.Logic/workflows/delete | Deletes the workflow. |
> | Eylem | Microsoft.Logic/workflows/detectors/read | Reads the workflow detector. |
> | Eylem | Microsoft.Logic/workflows/disable/action | Disables the workflow. |
> | Eylem | Microsoft.Logic/workflows/enable/action | İş akışını etkinleştirir. |
> | Eylem | Microsoft.Logic/workflows/listCallbackUrl/action | Gets the callback URL for workflow. |
> | Eylem | Microsoft.Logic/workflows/listSwagger/action | Gets the workflow swagger definitions. |
> | Eylem | Microsoft.Logic/workflows/move/action | Moves Workflow from its existing subscription id, resource group, and/or name to a different subscription id, resource group, and/or name. |
> | Eylem | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/read | Reads the workflow diagnostic settings. |
> | Eylem | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/write | Creates or updates the workflow diagnostic setting. |
> | Eylem | Microsoft.Logic/workflows/providers/Microsoft.Insights/logDefinitions/read | Reads the workflow log definitions. |
> | Eylem | Microsoft.Logic/workflows/providers/Microsoft.Insights/metricDefinitions/read | Reads the workflow metric definitions. |
> | Eylem | Microsoft.Logic/workflows/read | Reads the workflow. |
> | Eylem | Microsoft.Logic/workflows/regenerateAccessKey/action | Regenerates the access key secrets. |
> | Eylem | Microsoft.Logic/workflows/run/action | Starts a run of the workflow. |
> | Eylem | Microsoft.Logic/workflows/runs/actions/listExpressionTraces/action | Gets the workflow run action expression traces. |
> | Eylem | Microsoft.Logic/workflows/runs/actions/read | Reads the workflow run action. |
> | Eylem | Microsoft.Logic/workflows/runs/actions/repetitions/listExpressionTraces/action | Gets the workflow run action repetition expression traces. |
> | Eylem | Microsoft.Logic/workflows/runs/actions/repetitions/read | Reads the workflow run action repetition. |
> | Eylem | Microsoft.Logic/workflows/runs/actions/repetitions/requestHistories/read | Reads the workflow run repetition action request history. |
> | Eylem | Microsoft.Logic/workflows/runs/actions/requestHistories/read | Reads the workflow run action request history. |
> | Eylem | Microsoft.Logic/workflows/runs/actions/scoperepetitions/read | Reads the workflow run action scope repetition. |
> | Eylem | Microsoft.Logic/workflows/runs/cancel/action | Cancels the run of a workflow. |
> | Eylem | Microsoft.Logic/workflows/runs/delete | Deletes a run of a workflow. |
> | Eylem | Microsoft.Logic/workflows/runs/operations/read | Reads the workflow run operation status. |
> | Eylem | Microsoft.Logic/workflows/runs/read | Reads the workflow run. |
> | Eylem | Microsoft.Logic/workflows/suspend/action | Suspends the workflow. |
> | Eylem | Microsoft.Logic/workflows/triggers/histories/read | Reads the trigger histories. |
> | Eylem | Microsoft.Logic/workflows/triggers/histories/resubmit/action | Resubmits the workflow trigger. |
> | Eylem | Microsoft.Logic/workflows/triggers/listCallbackUrl/action | Gets the callback URL for trigger. |
> | Eylem | Microsoft.Logic/workflows/triggers/read | Reads the trigger. |
> | Eylem | Microsoft.Logic/workflows/triggers/reset/action | Resets the trigger. |
> | Eylem | Microsoft.Logic/workflows/triggers/run/action | Executes the trigger. |
> | Eylem | Microsoft.Logic/workflows/triggers/setState/action | Sets the trigger state. |
> | Eylem | Microsoft.Logic/workflows/validate/action | Validates the workflow. |
> | Eylem | Microsoft.Logic/workflows/versions/read | Reads the workflow version. |
> | Eylem | Microsoft.Logic/workflows/versions/triggers/listCallbackUrl/action | Gets the callback URL for trigger. |
> | Eylem | Microsoft.Logic/workflows/write | Creates or updates the workflow. |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/move/action | Move any Machine Learning Commitment Plan Association |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/read | Read any Machine Learning Commitment Plan Association |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/delete | Delete any Machine Learning Commitment Plan |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/join/action | Join any Machine Learning Commitment Plan |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/read | Read any Machine Learning Commitment Plan |
> | Eylem | Microsoft.MachineLearning/commitmentPlans/write | Create or Update any Machine Learning Commitment Plan |
> | Eylem | Microsoft.MachineLearning/locations/operationresults/read | Get result of a Machine Learning Operation |
> | Eylem | Microsoft.MachineLearning/locations/operationsstatus/read | Get status of an ongoing Machine Learning Operation |
> | Eylem | Microsoft.MachineLearning/operations/read | Get Machine Learning Operations |
> | Eylem | Microsoft.MachineLearning/register/action | Registers the subscription for the machine learning web service resource provider and enables the creation of web services. |
> | Eylem | Microsoft.MachineLearning/skus/read | Get Machine Learning Commitment Plan SKUs |
> | Eylem | Microsoft.MachineLearning/webServices/action | Create regional Web Service Properties for supported regions |
> | Eylem | Microsoft.MachineLearning/webServices/delete | Delete any Machine Learning Web Service |
> | Eylem | Microsoft.MachineLearning/webServices/listkeys/read | Get keys to a Machine Learning Web Service |
> | Eylem | Microsoft.MachineLearning/webServices/read | Read any Machine Learning Web Service |
> | Eylem | Microsoft.MachineLearning/webServices/write | Create or Update any Machine Learning Web Service |
> | Eylem | Microsoft.MachineLearning/Workspaces/delete | Delete any Machine Learning Workspace |
> | Eylem | Microsoft.MachineLearning/Workspaces/listworkspacekeys/action | List keys for a Machine Learning Workspace |
> | Eylem | Microsoft.MachineLearning/Workspaces/read | Read any Machine Learning Workspace |
> | Eylem | Microsoft.MachineLearning/Workspaces/resyncstoragekeys/action | Resync keys of storage account configured for a Machine Learning Workspace |
> | Eylem | Microsoft.MachineLearning/Workspaces/write | Create or Update any Machine Learning Workspace |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MachineLearningServices/locations/computeoperationsstatus/read | Gets the status of a particular compute operation |
> | Eylem | Microsoft.MachineLearningServices/locations/quotas/read | Gets the currently assigned Workspace Quotas based on VMFamily. |
> | Eylem | Microsoft.MachineLearningServices/locations/updateQuotas/action | Update quota for each VM family in workspace. |
> | Eylem | Microsoft.MachineLearningServices/locations/usages/read | Usage report for aml compute resources in a subscription |
> | Eylem | Microsoft.MachineLearningServices/locations/vmsizes/read | Get supported vm sizes |
> | Eylem | Microsoft.MachineLearningServices/locations/workspaceOperationsStatus/read | Gets the status of a particular workspace operation |
> | Eylem | Microsoft.MachineLearningServices/register/action | Registers the subscription for the Machine Learning Services Resource Provider |
> | Eylem | Microsoft.MachineLearningServices/workspaces/computes/delete | Deletes the compute resources in Machine Learning Services Workspace(s) |
> | Eylem | Microsoft.MachineLearningServices/workspaces/computes/listKeys/action | List secrets for compute resources in Machine Learning Services Workspace |
> | Eylem | Microsoft.MachineLearningServices/workspaces/computes/listNodes/action | List nodes for compute resource in Machine Learning Services Workspace |
> | Eylem | Microsoft.MachineLearningServices/workspaces/computes/read | Gets the compute resources in Machine Learning Services Workspace(s) |
> | Eylem | Microsoft.MachineLearningServices/workspaces/computes/write | Creates or updates the compute resources in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datadriftdetectors/read | Gets data drift detectors in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datadriftdetectors/write | Creates or updates data drift detectors in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/registered/delete | Deletes registered datasets in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/registered/preview/read | Gets dataset preview for registered datasets in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/registered/profile/read | Gets dataset profiles for registered datasets in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/registered/profile/write | Creates or updates dataset profiles in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/registered/read | Gets registered datasets in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/registered/write | Creates or updates registered datasets in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/unregistered/delete | Deletes unregistered datasets in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/unregistered/preview/read | Gets dataset preview for unregistered datasets in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/unregistered/profile/read | Gets dataset profiles for unregistered datasets in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/unregistered/read | Gets unregistered datasets in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datasets/unregistered/write | Creates or updates unregistered datasets in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datastores/delete | Deletes datastores in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datastores/read | Gets datastores in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/datastores/write | Creates or updates datastores in Machine Learning Services Workspace(s) |
> | Eylem | Microsoft.MachineLearningServices/workspaces/delete | Deletes the Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/endpoints/pipelines/read | Gets published pipelines and pipeline endpoints  in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/endpoints/pipelines/write | Creates or updates published pipelines and pipeline endpoints in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/environments/build/action | Builds environments in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/environments/read | Gets environments in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/environments/readSecrets/action | Gets environments with secrets in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/environments/write | Creates or updates environments in Machine Learning Services Workspace(s) |
> | Eylem | Microsoft.MachineLearningServices/workspaces/eventGridFilters/read | Get an Event Grid filter for a particular workspace |
> | DataAction | Microsoft.MachineLearningServices/workspaces/experiments/delete | Deletes experiments in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/experiments/read | Gets experiments in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/experiments/runs/read | Gets runs in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/experiments/runs/scriptRun/submit/action | Creates or updates script runs in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/experiments/runs/write | Creates or updates runs in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/experiments/write | Creates or updates experiments in Machine Learning Services Workspace(s) |
> | Eylem | Microsoft.MachineLearningServices/workspaces/listKeys/action | List secrets for a Machine Learning Services Workspace |
> | DataAction | Microsoft.MachineLearningServices/workspaces/metadata/artifacts/delete | Deletes artifacts in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/metadata/artifacts/read | Gets artifacts in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/metadata/artifacts/write | Creates or updates artifacts in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/metadata/snapshots/delete | Deletes snapshots in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/metadata/snapshots/read | Gets snapshots in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/metadata/snapshots/write | Creates or updates snapshots in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/models/delete | Deletes models in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/models/download/action | Downloads models in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/models/package/action | Packages models in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/models/read | Gets models in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/models/write | Creates or updates models in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/modules/read | Gets modules in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/modules/write | Creates or updates module in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/pipelinedrafts/delete | Deletes pipeline drafts in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/pipelinedrafts/read | Gets pipeline drafts in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/pipelinedrafts/write | Creates or updates pipeline drafts in Machine Learning Services Workspace(s) |
> | Eylem | Microsoft.MachineLearningServices/workspaces/read | Gets the Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aci/delete | Deletes ACI services in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aci/listkeys/action | Lists keys for ACI services in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aci/write | Creates or updates ACI services in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aks/devtest/delete | Deletes devtest AKS services in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aks/devtest/listkeys/action | Lists keys for devtest AKS services in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aks/devtest/score/action | Scores devtest AKS services in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aks/devtest/write | Creates or updates devtest AKS services in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aks/prod/delete | Deletes prod AKS services in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aks/prod/listkeys/action | Lists keys for prod AKS services in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aks/prod/score/action | Scores prod AKS services in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/aks/prod/write | Creates or updates prod AKS services in Machine Learning Services Workspace(s) |
> | DataAction | Microsoft.MachineLearningServices/workspaces/services/read | Gets services in Machine Learning Services Workspace(s) |
> | Eylem | Microsoft.MachineLearningServices/workspaces/write | Creates or updates a Machine Learning Services Workspace(s) |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ManagedIdentity/identities/read | Gets an existing system assigned identity |
> | Eylem | Microsoft.ManagedIdentity/operations/read | Lists operations available on Microsoft.ManagedIdentity resource provider |
> | Eylem | Microsoft.ManagedIdentity/register/action | Registers the subscription for the managed identity resource provider |
> | Eylem | Microsoft.ManagedIdentity/userAssignedIdentities/assign/action | RBAC action for assigning an existing user assigned identity to a resource |
> | Eylem | Microsoft.ManagedIdentity/userAssignedIdentities/delete | Deletes an existing user assigned identity |
> | Eylem | Microsoft.ManagedIdentity/userAssignedIdentities/read | Gets an existing user assigned identity |
> | Eylem | Microsoft.ManagedIdentity/userAssignedIdentities/write | Creates a new user assigned identity or updates the tags associated with an existing user assigned identity |

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ManagedServices/marketplaceRegistrationDefinitions/read | Retrieves a list of Managed Services registration definitions. |
> | Eylem | Microsoft.ManagedServices/operations/read | Retrieves a list of Managed Services operations. |
> | Eylem | Microsoft.ManagedServices/operationStatuses/read | Reads the operation status for the resource. |
> | Eylem | Microsoft.ManagedServices/register/action | Register to Managed Services. |
> | Eylem | Microsoft.ManagedServices/registrationAssignments/delete | Removes Managed Services registration assignment. |
> | Eylem | Microsoft.ManagedServices/registrationAssignments/read | Retrieves a list of Managed Services registration assignments. |
> | Eylem | Microsoft.ManagedServices/registrationAssignments/write | Add or modify Managed Services registration assignment. |
> | Eylem | Microsoft.ManagedServices/registrationDefinitions/delete | Removes Managed Services registration definition. |
> | Eylem | Microsoft.ManagedServices/registrationDefinitions/read | Retrieves a list of Managed Services registration definitions. |
> | Eylem | Microsoft.ManagedServices/registrationDefinitions/write | Add or modify Managed Services registration definition. |
> | Eylem | Microsoft.ManagedServices/unregister/action | Unregister from Managed Services. |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Management/checkNameAvailability/action | Checks if the specified management group name is valid and unique. |
> | Eylem | Microsoft.Management/getEntities/action | List all entities (Management Groups, Subscriptions, etc.) for the authenticated user. |
> | Eylem | Microsoft.Management/managementGroups/delete | Delete management group. |
> | Eylem | Microsoft.Management/managementGroups/descendants/read | Gets all the descendants (Management Groups, Subscriptions) of a Management Group. |
> | Eylem | Microsoft.Management/managementGroups/read | List management groups for the authenticated user. |
> | Eylem | Microsoft.Management/managementGroups/subscriptions/delete | De-associates subscription from the management group. |
> | Eylem | Microsoft.Management/managementGroups/subscriptions/write | Associates existing subscription with the management group. |
> | Eylem | Microsoft.Management/managementGroups/write | Create or update a management group. |
> | Eylem | Microsoft.Management/register/action | Register the specified subscription with Microsoft.Management |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | DataAction | Microsoft.Maps/accounts/data/read | Grants data read access to a maps account. |
> | Eylem | Microsoft.Maps/accounts/delete | Delete a Maps Account. |
> | Eylem | Microsoft.Maps/accounts/eventGridFilters/delete | Delete an Event Grid filter |
> | Eylem | Microsoft.Maps/accounts/eventGridFilters/read | Get an Event Grid filter |
> | Eylem | Microsoft.Maps/accounts/eventGridFilters/write | Create or update an Event Grid filter |
> | Eylem | Microsoft.Maps/accounts/listKeys/action | List Maps Account keys |
> | Eylem | Microsoft.Maps/accounts/read | Get a Maps Account. |
> | Eylem | Microsoft.Maps/accounts/regenerateKey/action | Generate new Maps Account primary or secondary key |
> | Eylem | Microsoft.Maps/accounts/write | Create or update a Maps Account. |
> | Eylem | Microsoft.Maps/operations/read | Read the provider operations |
> | Eylem | Microsoft.Maps/register/action | Register the provider |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/read | Returns an Agreement. |
> | Eylem | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/write | Accepts a signed agreement. |
> | Eylem | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/importImage/action | Imports an image to the end user's ACR. |
> | Eylem | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/read | Returns a config. |
> | Eylem | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/write | Saves a config. |
> | Eylem | Microsoft.Marketplace/register/action | Registers Microsoft.Marketplace resource provider in the subscription. |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MarketplaceApps/ClassicDevServices/delete | Does a DELETE operation on a classic dev service resource. |
> | Eylem | Microsoft.MarketplaceApps/ClassicDevServices/listSecrets/action | Gets a classic dev service resource management keys. |
> | Eylem | Microsoft.MarketplaceApps/ClassicDevServices/listSingleSignOnToken/action | Gets the Single Sign On URL for a classic dev service. |
> | Eylem | Microsoft.MarketplaceApps/ClassicDevServices/read | Does a GET operation on a classic dev service. |
> | Eylem | Microsoft.MarketplaceApps/ClassicDevServices/regenerateKey/action | Generates a classic dev service resource management keys. |
> | Eylem | Microsoft.MarketplaceApps/Operations/read | Read the operations for all resource types. |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MarketplaceOrdering/agreements/offers/plans/cancel/action | Cancel an agreement for a given marketplace item |
> | Eylem | Microsoft.MarketplaceOrdering/agreements/offers/plans/read | Return an agreement for a given marketplace item |
> | Eylem | Microsoft.MarketplaceOrdering/agreements/offers/plans/sign/action | Sign an agreement for a given marketplace item |
> | Eylem | Microsoft.MarketplaceOrdering/agreements/read | Return all agreements under given subscription |
> | Eylem | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/read | Get an agreement for a given marketplace virtual machine item |
> | Eylem | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/write | Sign or Cancel an agreement for a given marketplace virtual machine item |
> | Eylem | Microsoft.MarketplaceOrdering/operations/read | List all possible operations in the API |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Media/checknameavailability/action | Checks if a Media Services account name is available |
> | Eylem | Microsoft.Media/mediaservices/accountfilters/delete | Delete any Account Filter |
> | Eylem | Microsoft.Media/mediaservices/accountfilters/read | Read any Account Filter |
> | Eylem | Microsoft.Media/mediaservices/accountfilters/write | Create or Update any Account Filter |
> | Eylem | Microsoft.Media/mediaservices/assets/assetfilters/delete | Delete any Asset Filter |
> | Eylem | Microsoft.Media/mediaservices/assets/assetfilters/read | Read any Asset Filter |
> | Eylem | Microsoft.Media/mediaservices/assets/assetfilters/write | Create or Update any Asset Filter |
> | Eylem | Microsoft.Media/mediaservices/assets/delete | Delete any Asset |
> | Eylem | Microsoft.Media/mediaservices/assets/getEncryptionKey/action | Get Asset Encryption Key |
> | Eylem | Microsoft.Media/mediaservices/assets/listContainerSas/action | List Asset Container SAS URLs |
> | Eylem | Microsoft.Media/mediaservices/assets/listStreamingLocators/action | List Streaming Locators for Asset |
> | Eylem | Microsoft.Media/mediaservices/assets/read | Read any Asset |
> | Eylem | Microsoft.Media/mediaservices/assets/write | Create or Update any Asset |
> | Eylem | Microsoft.Media/mediaservices/contentKeyPolicies/delete | Delete any Content Key Policy |
> | Eylem | Microsoft.Media/mediaservices/contentKeyPolicies/getPolicyPropertiesWithSecrets/action | Get Policy Properties With Secrets |
> | Eylem | Microsoft.Media/mediaservices/contentKeyPolicies/read | Read any Content Key Policy |
> | Eylem | Microsoft.Media/mediaservices/contentKeyPolicies/write | Create or Update any Content Key Policy |
> | Eylem | Microsoft.Media/mediaservices/delete | Delete any Media Services Account |
> | Eylem | Microsoft.Media/mediaservices/eventGridFilters/delete | Delete any Event Grid Filter |
> | Eylem | Microsoft.Media/mediaservices/eventGridFilters/read | Read any Event Grid Filter |
> | Eylem | Microsoft.Media/mediaservices/eventGridFilters/write | Create or Update any Event Grid Filter |
> | Eylem | Microsoft.Media/mediaservices/liveEventOperations/read | Read any Live Event Operation |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/delete | Delete any Live Event |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/liveOutputs/delete | Delete any Live Output |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/liveOutputs/read | Read any Live Output |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/liveOutputs/write | Create or Update any Live Output |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/read | Read any Live Event |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/reset/action | Reset any Live Event Operation |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/start/action | Start any Live Event Operation |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/stop/action | Stop any Live Event Operation |
> | Eylem | Microsoft.Media/mediaservices/liveEvents/write | Create or Update any Live Event |
> | Eylem | Microsoft.Media/mediaservices/liveOutputOperations/read | Read any Live Output Operation |
> | Eylem | Microsoft.Media/mediaservices/read | Read any Media Services Account |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpointOperations/read | Read any Streaming Endpoint Operation |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/delete | Delete any Streaming Endpoint |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/read | Read any Streaming Endpoint |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/scale/action | Scale any Streaming Endpoint Operation |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/start/action | Start any Streaming Endpoint Operation |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/stop/action | Stop any Streaming Endpoint Operation |
> | Eylem | Microsoft.Media/mediaservices/streamingEndpoints/write | Create or Update any Streaming Endpoint |
> | Eylem | Microsoft.Media/mediaservices/streamingLocators/delete | Delete any Streaming Locator |
> | Eylem | Microsoft.Media/mediaservices/streamingLocators/listContentKeys/action | List Content Keys |
> | Eylem | Microsoft.Media/mediaservices/streamingLocators/listPaths/action | List Paths |
> | Eylem | Microsoft.Media/mediaservices/streamingLocators/read | Read any Streaming Locator |
> | Eylem | Microsoft.Media/mediaservices/streamingLocators/write | Create or Update any Streaming Locator |
> | Eylem | Microsoft.Media/mediaservices/streamingPolicies/delete | Delete any Streaming Policy |
> | Eylem | Microsoft.Media/mediaservices/streamingPolicies/read | Read any Streaming Policy |
> | Eylem | Microsoft.Media/mediaservices/streamingPolicies/write | Create or Update any Streaming Policy |
> | Eylem | Microsoft.Media/mediaservices/syncStorageKeys/action | Synchronize the Storage Keys for an attached Azure Storage account |
> | Eylem | Microsoft.Media/mediaservices/transforms/delete | Delete any Transform |
> | Eylem | Microsoft.Media/mediaservices/transforms/jobs/cancelJob/action | Cancel Job |
> | Eylem | Microsoft.Media/mediaservices/transforms/jobs/delete | Delete any Job |
> | Eylem | Microsoft.Media/mediaservices/transforms/jobs/read | Read any Job |
> | Eylem | Microsoft.Media/mediaservices/transforms/jobs/write | Create or Update any Job |
> | Eylem | Microsoft.Media/mediaservices/transforms/read | Read any Transform |
> | Eylem | Microsoft.Media/mediaservices/transforms/write | Create or Update any Transform |
> | Eylem | Microsoft.Media/mediaservices/write | Create or Update any Media Services Account |
> | Eylem | Microsoft.Media/operations/read | Get Available Operations |
> | Eylem | Microsoft.Media/register/action | Registers the subscription for the Media Services resource provider and enables the creation of Media Services accounts |
> | Eylem | Microsoft.Media/unregister/action | Unregisters the subscription for the Media Services resource provider |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Migrate/assessmentprojects/assessmentOptions/read | Gets the assessment options which are available in the given location |
> | Eylem | Microsoft.Migrate/assessmentprojects/assessments/read | Lists assessments within a project |
> | Eylem | Microsoft.Migrate/assessmentprojects/delete | Deletes the assessment project |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/assessments/assessedmachines/read | Get the properties of an assessed machine |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/assessments/delete | Deletes an assessment |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/assessments/downloadurl/action | Downloads an assessment report's URL |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/assessments/read | Gets the properties of an assessment |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/assessments/write | Creates a new assessment or updates an existing assessment |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/delete | Deletes a group |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/read | Get the properties of a group |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/updateMachines/action | Update group by adding or removing machines |
> | Eylem | Microsoft.Migrate/assessmentprojects/groups/write | Creates a new group or updates an existing group |
> | Eylem | Microsoft.Migrate/assessmentprojects/hypervcollectors/delete | Deletes the HyperV collector |
> | Eylem | Microsoft.Migrate/assessmentprojects/hypervcollectors/read | Gets the properties of HyperV collector |
> | Eylem | Microsoft.Migrate/assessmentprojects/hypervcollectors/write | Creates a new HyperV collector or updates an existing HyperV collector |
> | Eylem | Microsoft.Migrate/assessmentprojects/importcollectors/delete | Deletes the Import collector |
> | Eylem | Microsoft.Migrate/assessmentprojects/importcollectors/read | Gets the properties of Import collector |
> | Eylem | Microsoft.Migrate/assessmentprojects/importcollectors/write | Creates a new Import collector or updates an existing Import collector |
> | Eylem | Microsoft.Migrate/assessmentprojects/machines/read | Gets the properties of a machine |
> | Eylem | Microsoft.Migrate/assessmentprojects/read | Gets the properties of assessment project |
> | Eylem | Microsoft.Migrate/assessmentprojects/servercollectors/read | Gets the properties of Server collector |
> | Eylem | Microsoft.Migrate/assessmentprojects/servercollectors/write | Creates a new Server collector or updates an existing Server collector |
> | Eylem | Microsoft.Migrate/assessmentprojects/vmwarecollectors/delete | Deletes the VMware collector |
> | Eylem | Microsoft.Migrate/assessmentprojects/vmwarecollectors/read | Gets the properties of VMware collector |
> | Eylem | Microsoft.Migrate/assessmentprojects/vmwarecollectors/write | Creates a new VMware collector or updates an existing VMware collector |
> | Eylem | Microsoft.Migrate/assessmentprojects/write | Creates a new assessment project or updates an existing assessment project |
> | Eylem | Microsoft.Migrate/locations/assessmentOptions/read | Gets the assessment options which are available in the given location |
> | Eylem | Microsoft.Migrate/locations/checknameavailability/action | Checks availability of the resource name for the given subscription in the given location |
> | Eylem | Microsoft.Migrate/migrateprojects/DatabaseInstances/read | Gets the properties of a database instance |
> | Eylem | Microsoft.Migrate/migrateprojects/Databases/read | Gets the properties of a database |
> | Eylem | Microsoft.Migrate/migrateprojects/delete | Deletes a migrate project |
> | Eylem | Microsoft.Migrate/migrateprojects/machines/read | Gets the properties of a machine |
> | Eylem | Microsoft.Migrate/migrateprojects/MigrateEvents/Delete | Deletes a migrate event |
> | Eylem | Microsoft.Migrate/migrateprojects/MigrateEvents/read | Gets the properties of a migrate events. |
> | Eylem | Microsoft.Migrate/migrateprojects/read | Gets the properties of migrate project |
> | Eylem | Microsoft.Migrate/migrateprojects/RefreshSummary/action | Refreshes the migrate project summary |
> | Eylem | Microsoft.Migrate/migrateprojects/registerTool/action | Registers tool to a migrate project |
> | Eylem | Microsoft.Migrate/migrateprojects/solutions/cleanupData/action | Clean up the migrate project solution data |
> | Eylem | Microsoft.Migrate/migrateprojects/solutions/Delete | Deletes a  migrate project solution |
> | Eylem | Microsoft.Migrate/migrateprojects/solutions/getconfig/action | Gets the migrate project solution configuration |
> | Eylem | Microsoft.Migrate/migrateprojects/solutions/read | Gets the properties of migrate project solution |
> | Eylem | Microsoft.Migrate/migrateprojects/solutions/write | Creates a new migrate project solution or updates an existing migrate project solution |
> | Eylem | Microsoft.Migrate/migrateprojects/write | Creates a new migrate project or updates an existing migrate project |
> | Eylem | Microsoft.Migrate/Operations/read | Lists operations available on Microsoft.Migrate resource provider |
> | Eylem | Microsoft.Migrate/projects/assessments/read | Lists assessments within a project |
> | Eylem | Microsoft.Migrate/projects/delete | Deletes the project |
> | Eylem | Microsoft.Migrate/projects/groups/assessments/assessedmachines/read | Get the properties of an assessed machine |
> | Eylem | Microsoft.Migrate/projects/groups/assessments/delete | Deletes an assessment |
> | Eylem | Microsoft.Migrate/projects/groups/assessments/downloadurl/action | Downloads an assessment report's URL |
> | Eylem | Microsoft.Migrate/projects/groups/assessments/read | Gets the properties of an assessment |
> | Eylem | Microsoft.Migrate/projects/groups/assessments/write | Creates a new assessment or updates an existing assessment |
> | Eylem | Microsoft.Migrate/projects/groups/delete | Deletes a group |
> | Eylem | Microsoft.Migrate/projects/groups/read | Get the properties of a group |
> | Eylem | Microsoft.Migrate/projects/groups/write | Creates a new group or updates an existing group |
> | Eylem | Microsoft.Migrate/projects/keys/action | Gets shared keys for the project |
> | Eylem | Microsoft.Migrate/projects/machines/read | Gets the properties of a machine |
> | Eylem | Microsoft.Migrate/projects/read | Gets the properties of a project |
> | Eylem | Microsoft.Migrate/projects/write | Creates a new project or updates an existing project |
> | Eylem | Microsoft.Migrate/register/action | Registers Subscription with Microsoft.Migrate resource provider |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.MixedReality/register/action | Registers a subscription for the Mixed Reality resource provider. |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/create/action | Create spatial anchors |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/delete | Delete spatial anchors |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Discover nearby spatial anchors |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Get properties of spatial anchors |
> | Eylem | Microsoft.MixedReality/spatialAnchorsAccounts/providers/Microsoft.Insights/diagnosticSettings/read | Gets the diagnostic setting for Microsoft.MixedReality/spatialAnchorsAccounts |
> | Eylem | Microsoft.MixedReality/spatialAnchorsAccounts/providers/Microsoft.Insights/diagnosticSettings/write | Creates or updates the diagnostic setting for Microsoft.MixedReality/spatialAnchorsAccounts |
> | Eylem | Microsoft.MixedReality/spatialAnchorsAccounts/providers/Microsoft.Insights/metricDefinitions/read | Gets the available metrics for Microsoft.MixedReality/spatialAnchorsAccounts |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Locate spatial anchors |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Submit diagnostics data to help improve the quality of the Azure Spatial Anchors service |
> | DataAction | Microsoft.MixedReality/SpatialAnchorsAccounts/write | Update spatial anchors properties |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.NetApp/locations/checkfilepathavailability/action | Check if file path is available |
> | Eylem | Microsoft.NetApp/locations/checknameavailability/action | Check if resource name is available |
> | Eylem | Microsoft.NetApp/locations/operationresults/read | Reads an operation result resource. |
> | Eylem | Microsoft.NetApp/locations/read | Reads an availability check resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/backupPolicies/delete | Deletes a backup policy resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/backupPolicies/read | Reads a backup policy resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/backupPolicies/write | Writes a backup policy resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/delete | Deletes a pool resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/read | Reads a pool resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/backups/delete | Deletes a backup resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/backups/read | Reads a backup resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/backups/write | Writes a backup resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/break/action | Break volume replication relations |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/delete | Deletes a volume resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/mountTargets/read | Reads a mount target resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/read | Reads a volume resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/ReplicationStatus/action | Reads the statuses of the Volume Replication. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/revert/action | Revert volume to specific snapshot |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/delete | Deletes a snapshot resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/read | Reads a snapshot resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/write | Writes a snapshot resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/write | Writes a volume resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/capacityPools/write | Writes a pool resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/delete | Deletes an account resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/read | Reads an account resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/vaults/read | Reads a vault resource. |
> | Eylem | Microsoft.NetApp/netAppAccounts/write | Writes an account resource. |
> | Eylem | Microsoft.NetApp/Operations/read | Reads an operation resources. |
> | Eylem | Microsoft.NetApp/register/action | Registers Subscription with Microsoft.NetApp resource provider |
> | Eylem | Microsoft.NetApp/unregister/action | Unregisters Subscription with Microsoft.NetApp resource provider |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Network/applicationGatewayAvailableRequestHeaders/read | Get Application Gateway available Request Headers |
> | Eylem | Microsoft.Network/applicationGatewayAvailableResponseHeaders/read | Get Application Gateway available Response Header |
> | Eylem | Microsoft.Network/applicationGatewayAvailableServerVariables/read | Get Application Gateway available Server Variables |
> | Eylem | Microsoft.Network/applicationGatewayAvailableSslOptions/predefinedPolicies/read | Application Gateway Ssl Predefined Policy |
> | Eylem | Microsoft.Network/applicationGatewayAvailableSslOptions/read | Application Gateway available Ssl Options |
> | Eylem | Microsoft.Network/applicationGatewayAvailableWafRuleSets/read | Gets Application Gateway Available Waf Rule Sets |
> | Eylem | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Joins an application gateway backend address pool. Not Alertable. |
> | Eylem | Microsoft.Network/applicationGateways/backendhealth/action | Gets an application gateway backend health |
> | Eylem | Microsoft.Network/applicationGateways/delete | Deletes an application gateway |
> | Eylem | Microsoft.Network/applicationGateways/getBackendHealthOnDemand/action | Gets an application gateway backend health on demand for given http setting and backend pool |
> | Eylem | Microsoft.Network/applicationGateways/read | Gets an application gateway |
> | Eylem | Microsoft.Network/applicationGateways/start/action | Starts an application gateway |
> | Eylem | Microsoft.Network/applicationGateways/stop/action | Stops an application gateway |
> | Eylem | Microsoft.Network/applicationGateways/write | Creates an application gateway or updates an application gateway |
> | Eylem | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/delete | Deletes an Application Gateway WAF policy |
> | Eylem | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/read | Gets an Application Gateway WAF policy |
> | Eylem | Microsoft.Network/ApplicationGatewayWebApplicationFirewallPolicies/write | Creates an Application Gateway WAF policy or updates an Application Gateway WAF policy |
> | Eylem | Microsoft.Network/applicationSecurityGroups/delete | Deletes an Application Security Group |
> | Eylem | Microsoft.Network/applicationSecurityGroups/joinIpConfiguration/action | Joins an IP Configuration to Application Security Groups. Not alertable. |
> | Eylem | Microsoft.Network/applicationSecurityGroups/joinNetworkSecurityRule/action | Joins a Security Rule to Application Security Groups. Not alertable. |
> | Eylem | Microsoft.Network/applicationSecurityGroups/listIpConfigurations/action | Lists IP Configurations in the ApplicationSecurityGroup |
> | Eylem | Microsoft.Network/applicationSecurityGroups/read | Gets an Application Security Group ID. |
> | Eylem | Microsoft.Network/applicationSecurityGroups/write | Creates an Application Security Group, or updates an existing Application Security Group. |
> | Eylem | Microsoft.Network/azureFirewallFqdnTags/read | Gets Azure Firewall FQDN Tags |
> | Eylem | Microsoft.Network/azurefirewalls/delete | Delete Azure Firewall |
> | Eylem | Microsoft.Network/azurefirewalls/read | Get Azure Firewall |
> | Eylem | Microsoft.Network/azurefirewalls/write | Creates or updates an Azure Firewall |
> | Eylem | Microsoft.Network/bastionHosts/createbsl/action | Creates shareable urls for the VMs under a bastion and returns the urls |
> | Eylem | Microsoft.Network/bastionHosts/delete | Deletes a Bastion Host |
> | Eylem | Microsoft.Network/bastionHosts/deletebsl/action | Deletes shareable urls for the provided VMs under a bastion |
> | Eylem | Microsoft.Network/bastionHosts/disconnectactivesessions/action | Disconnect given Active Sessions in the Bastion Host |
> | Eylem | Microsoft.Network/bastionHosts/getactivesessions/action | Get Active Sessions in the Bastion Host |
> | Eylem | Microsoft.Network/bastionHosts/getbsl/action | Returns the shareable urls for the specified VMs in a Bastion subnet provided their urls are created |
> | Eylem | Microsoft.Network/bastionHosts/read | Gets a Bastion Host |
> | Eylem | Microsoft.Network/bastionHosts/write | Create or Update a Bastion Host |
> | Eylem | Microsoft.Network/bgpServiceCommunities/read | Get Bgp Service Communities |
> | Eylem | Microsoft.Network/checkFrontDoorNameAvailability/action | Checks whether a Front Door name is available |
> | Eylem | Microsoft.Network/checkTrafficManagerNameAvailability/action | Checks the availability of a Traffic Manager Relative DNS name. |
> | Eylem | Microsoft.Network/connections/delete | Deletes VirtualNetworkGatewayConnection |
> | Eylem | Microsoft.Network/connections/read | Gets VirtualNetworkGatewayConnection |
> | Eylem | Microsoft.Network/connections/revoke/action | Marks an Express Route Connection status as Revoked |
> | Eylem | Microsoft.Network/connections/sharedkey/action | Get VirtualNetworkGatewayConnection SharedKey |
> | Eylem | Microsoft.Network/connections/sharedKey/read | Gets VirtualNetworkGatewayConnection SharedKey |
> | Eylem | Microsoft.Network/connections/sharedKey/write | Creates or updates an existing VirtualNetworkGatewayConnection SharedKey |
> | Eylem | Microsoft.Network/connections/startpacketcapture/action | Starts a Virtual Network Gateway Connection Packet Capture. |
> | Eylem | Microsoft.Network/connections/stoppacketcapture/action | Stops a Virtual Network Gateway Connection Packet Capture. |
> | Eylem | Microsoft.Network/connections/vpndeviceconfigurationscript/action | Gets Vpn Device Configuration of VirtualNetworkGatewayConnection |
> | Eylem | Microsoft.Network/connections/write | Creates or updates an existing VirtualNetworkGatewayConnection |
> | Eylem | Microsoft.Network/ddosCustomPolicies/delete | Deletes a DDoS customized policy |
> | Eylem | Microsoft.Network/ddosCustomPolicies/read | Gets a DDoS customized policy definition Definition |
> | Eylem | Microsoft.Network/ddosCustomPolicies/write | Creates a DDoS customized policy or updates an existing DDoS customized policy |
> | Eylem | Microsoft.Network/ddosProtectionPlans/delete | Deletes a DDoS Protection Plan |
> | Eylem | Microsoft.Network/ddosProtectionPlans/join/action | Joins a DDoS Protection Plan. Not alertable. |
> | Eylem | Microsoft.Network/ddosProtectionPlans/read | Gets a DDoS Protection Plan |
> | Eylem | Microsoft.Network/ddosProtectionPlans/write | Creates a DDoS Protection Plan or updates a DDoS Protection Plan  |
> | Eylem | Microsoft.Network/dnsoperationresults/read | Gets results of a DNS operation |
> | Eylem | Microsoft.Network/dnsoperationstatuses/read | Gets status of a DNS operation  |
> | Eylem | Microsoft.Network/dnszones/A/delete | Remove the record set of a given name and type ‘A’ from a DNS zone. |
> | Eylem | Microsoft.Network/dnszones/A/read | Get the record set of type ‘A’, in JSON format. The record set contains a list of records as well as the TTL, tags, and etag. |
> | Eylem | Microsoft.Network/dnszones/A/write | Create or update a record set of type ‘A’ within a DNS zone. The records specified will replace the current records in the record set. |
> | Eylem | Microsoft.Network/dnszones/AAAA/delete | Remove the record set of a given name and type ‘AAAA’ from a DNS zone. |
> | Eylem | Microsoft.Network/dnszones/AAAA/read | Get the record set of type ‘AAAA’, in JSON format. The record set contains a list of records as well as the TTL, tags, and etag. |
> | Eylem | Microsoft.Network/dnszones/AAAA/write | Create or update a record set of type ‘AAAA’ within a DNS zone. The records specified will replace the current records in the record set. |
> | Eylem | Microsoft.Network/dnszones/all/read | Gets DNS record sets across types |
> | Eylem | Microsoft.Network/dnszones/CAA/delete | Remove the record set of a given name and type ‘CAA’ from a DNS zone. |
> | Eylem | Microsoft.Network/dnszones/CAA/read | Get the record set of type ‘CAA’, in JSON format. The record set contains the TTL, tags, and etag. |
> | Eylem | Microsoft.Network/dnszones/CAA/write | Create or update a record set of type ‘CAA’ within a DNS zone. The records specified will replace the current records in the record set. |
> | Eylem | Microsoft.Network/dnszones/CNAME/delete | Remove the record set of a given name and type ‘CNAME’ from a DNS zone. |
> | Eylem | Microsoft.Network/dnszones/CNAME/read | Get the record set of type ‘CNAME’, in JSON format. The record set contains the TTL, tags, and etag. |
> | Eylem | Microsoft.Network/dnszones/CNAME/write | Create or update a record set of type ‘CNAME’ within a DNS zone. The records specified will replace the current records in the record set. |
> | Eylem | Microsoft.Network/dnszones/delete | Delete the DNS zone, in JSON format. The zone properties include tags, etag, numberOfRecordSets, and maxNumberOfRecordSets. |
> | Eylem | Microsoft.Network/dnszones/MX/delete | Remove the record set of a given name and type ‘MX’ from a DNS zone. |
> | Eylem | Microsoft.Network/dnszones/MX/read | Get the record set of type ‘MX’, in JSON format. The record set contains a list of records as well as the TTL, tags, and etag. |
> | Eylem | Microsoft.Network/dnszones/MX/write | Create or update a record set of type ‘MX’ within a DNS zone. The records specified will replace the current records in the record set. |
> | Eylem | Microsoft.Network/dnszones/NS/delete | Deletes the DNS record set of type NS |
> | Eylem | Microsoft.Network/dnszones/NS/read | Gets DNS record set of type NS |
> | Eylem | Microsoft.Network/dnszones/NS/write | Creates or updates DNS record set of type NS |
> | Eylem | Microsoft.Network/dnszones/PTR/delete | Remove the record set of a given name and type ‘PTR’ from a DNS zone. |
> | Eylem | Microsoft.Network/dnszones/PTR/read | Get the record set of type ‘PTR’, in JSON format. The record set contains a list of records as well as the TTL, tags, and etag. |
> | Eylem | Microsoft.Network/dnszones/PTR/write | Create or update a record set of type ‘PTR’ within a DNS zone. The records specified will replace the current records in the record set. |
> | Eylem | Microsoft.Network/dnszones/read | Get the DNS zone, in JSON format. The zone properties include tags, etag, numberOfRecordSets, and maxNumberOfRecordSets. Note that this command does not retrieve the record sets contained within the zone. |
> | Eylem | Microsoft.Network/dnszones/recordsets/read | Gets DNS record sets across types |
> | Eylem | Microsoft.Network/dnszones/SOA/read | Gets DNS record set of type SOA |
> | Eylem | Microsoft.Network/dnszones/SOA/write | Creates or updates DNS record set of type SOA |
> | Eylem | Microsoft.Network/dnszones/SRV/delete | Remove the record set of a given name and type ‘SRV’ from a DNS zone. |
> | Eylem | Microsoft.Network/dnszones/SRV/read | Get the record set of type ‘SRV’, in JSON format. The record set contains a list of records as well as the TTL, tags, and etag. |
> | Eylem | Microsoft.Network/dnszones/SRV/write | Create or update record set of type SRV |
> | Eylem | Microsoft.Network/dnszones/TXT/delete | Remove the record set of a given name and type ‘TXT’ from a DNS zone. |
> | Eylem | Microsoft.Network/dnszones/TXT/read | Get the record set of type ‘TXT’, in JSON format. The record set contains a list of records as well as the TTL, tags, and etag. |
> | Eylem | Microsoft.Network/dnszones/TXT/write | Create or update a record set of type ‘TXT’ within a DNS zone. The records specified will replace the current records in the record set. |
> | Eylem | Microsoft.Network/dnszones/write | Create or update a DNS zone within a resource group.  Used to update the tags on a DNS zone resource. Note that this command can not be used to create or update record sets within the zone. |
> | Eylem | Microsoft.Network/expressRouteCircuits/authorizations/delete | Deletes an ExpressRouteCircuit Authorization |
> | Eylem | Microsoft.Network/expressRouteCircuits/authorizations/read | Gets an ExpressRouteCircuit Authorization |
> | Eylem | Microsoft.Network/expressRouteCircuits/authorizations/write | Creates or updates an existing ExpressRouteCircuit Authorization |
> | Eylem | Microsoft.Network/expressRouteCircuits/delete | Deletes an ExpressRouteCircuit |
> | Eylem | Microsoft.Network/expressRouteCircuits/join/action | Joins an Express Route Circuit. Not alertable. |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/arpTables/read | Gets an ExpressRouteCircuit Peering ArpTable |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/connections/delete | Deletes an ExpressRouteCircuit Connection |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/connections/read | Gets an ExpressRouteCircuit Connection |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/connections/write | Creates or updates an existing ExpressRouteCircuit Connection Resource |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/delete | Deletes an ExpressRouteCircuit Peering |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/peerConnections/read | Gets Peer Express Route Circuit Connection |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/read | Gets an ExpressRouteCircuit Peering |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/routeTables/read | Gets an ExpressRouteCircuit Peering RouteTable |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/routeTablesSummary/read | Gets an ExpressRouteCircuit Peering RouteTable Summary |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/stats/read | Gets an ExpressRouteCircuit Peering Stat |
> | Eylem | Microsoft.Network/expressRouteCircuits/peerings/write | Creates or updates an existing ExpressRouteCircuit Peering |
> | Eylem | Microsoft.Network/expressRouteCircuits/read | Get an ExpressRouteCircuit |
> | Eylem | Microsoft.Network/expressRouteCircuits/stats/read | Gets an ExpressRouteCircuit Stat |
> | Eylem | Microsoft.Network/expressRouteCircuits/write | Creates or updates an existing ExpressRouteCircuit |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/join/action | Joins an Express Route Cross Connection. Not alertable. |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/arpTables/read | Gets an Express Route Cross Connection Peering Arp Table |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/delete | Deletes an Express Route Cross Connection Peering |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/read | Gets an Express Route Cross Connection Peering |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/routeTables/read | Gets an Express Route Cross Connection Peering Route Table |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/routeTableSummary/read | Gets an Express Route Cross Connection Peering Route Table Summary |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/peerings/write | Creates an Express Route Cross Connection Peering or Updates an existing Express Route Cross Connection Peering |
> | Eylem | Microsoft.Network/expressRouteCrossConnections/read | Get Express Route Cross Connection |
> | Eylem | Microsoft.Network/expressRouteGateways/expressRouteConnections/delete | Deletes an Express Route Connection |
> | Eylem | Microsoft.Network/expressRouteGateways/expressRouteConnections/read | Gets an Express Route Connection |
> | Eylem | Microsoft.Network/expressRouteGateways/expressRouteConnections/write | Creates an Express Route Connection or Updates an existing Express Route Connection |
> | Eylem | Microsoft.Network/expressRouteGateways/join/action | Joins an Express Route Gateway. Not alertable. |
> | Eylem | Microsoft.Network/expressRouteGateways/read | Get Express Route Gateway |
> | Eylem | Microsoft.Network/expressRoutePorts/delete | Deletes ExpressRoutePorts |
> | Eylem | Microsoft.Network/expressRoutePorts/join/action | Joins Express Route ports. Not alertable. |
> | Eylem | Microsoft.Network/expressRoutePorts/links/read | Gets ExpressRouteLink |
> | Eylem | Microsoft.Network/expressRoutePorts/read | Gets ExpressRoutePorts |
> | Eylem | Microsoft.Network/expressRoutePorts/write | Creates or updates ExpressRoutePorts |
> | Eylem | Microsoft.Network/expressRoutePortsLocations/read | Get Express Route Ports Locations |
> | Eylem | Microsoft.Network/expressRouteServiceProviders/read | Gets Express Route Service Providers |
> | Eylem | Microsoft.Network/firewallPolicies/delete | Deletes a Firewall Policy |
> | Eylem | Microsoft.Network/firewallPolicies/join/action | Joins a Firewall Policy. Not alertable. |
> | Eylem | Microsoft.Network/firewallPolicies/read | Gets a Firewall Policy |
> | Eylem | Microsoft.Network/firewallPolicies/ruleGroups/delete | Deletes a Firewall Policy Rule Group |
> | Eylem | Microsoft.Network/firewallPolicies/ruleGroups/read | Gets a Firewall Policy Rule Group |
> | Eylem | Microsoft.Network/firewallPolicies/ruleGroups/write | Creates a Firewall Policy Rule Group or Updates an existing Firewall Policy Rule Group |
> | Eylem | Microsoft.Network/firewallPolicies/write | Creates a Firewall Policy or Updates an existing Firewall Policy |
> | Eylem | Microsoft.Network/frontDoors/backendPools/delete | Deletes a backend pool |
> | Eylem | Microsoft.Network/frontDoors/backendPools/read | Gets a backend pool |
> | Eylem | Microsoft.Network/frontDoors/backendPools/write | Creates or updates a backend pool |
> | Eylem | Microsoft.Network/frontDoors/delete | Deletes a Front Door |
> | Eylem | Microsoft.Network/frontDoors/frontendEndpoints/delete | Deletes a frontend endpoint |
> | Eylem | Microsoft.Network/frontDoors/frontendEndpoints/disableHttps/action | Disables HTTPS on a Frontend Endpoint |
> | Eylem | Microsoft.Network/frontDoors/frontendEndpoints/enableHttps/action | Enables HTTPS on a Frontend Endpoint |
> | Eylem | Microsoft.Network/frontDoors/frontendEndpoints/read | Gets a frontend endpoint |
> | Eylem | Microsoft.Network/frontDoors/frontendEndpoints/write | Creates or updates a frontend endpoint |
> | Eylem | Microsoft.Network/frontDoors/healthProbeSettings/delete | Deletes health probe settings |
> | Eylem | Microsoft.Network/frontDoors/healthProbeSettings/read | Gets health probe settings |
> | Eylem | Microsoft.Network/frontDoors/healthProbeSettings/write | Creates or updates health probe settings |
> | Eylem | Microsoft.Network/frontDoors/loadBalancingSettings/delete | Creates or updates load balancing settings |
> | Eylem | Microsoft.Network/frontDoors/loadBalancingSettings/read | Gets load balancing settings |
> | Eylem | Microsoft.Network/frontDoors/loadBalancingSettings/write | Creates or updates load balancing settings |
> | Eylem | Microsoft.Network/frontDoors/purge/action | Purge cached content from a Front Door |
> | Eylem | Microsoft.Network/frontDoors/read | Gets a Front Door |
> | Eylem | Microsoft.Network/frontDoors/routingRules/delete | Deletes a routing rule |
> | Eylem | Microsoft.Network/frontDoors/routingRules/read | Gets a routing rule |
> | Eylem | Microsoft.Network/frontDoors/routingRules/write | Creates or updates a routing rule |
> | Eylem | Microsoft.Network/frontDoors/validateCustomDomain/action | Validates a frontend endpoint for a Front Door |
> | Eylem | Microsoft.Network/frontDoors/write | Creates or updates a Front Door |
> | Eylem | Microsoft.Network/frontDoorWebApplicationFirewallManagedRuleSets/read | Gets Web Application Firewall Managed Rule Sets |
> | Eylem | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/delete | Deletes a Web Application Firewall Policy |
> | Eylem | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/join/action | Joins a Web Application Firewall Policy. Not Alertable. |
> | Eylem | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/read | Gets a Web Application Firewall Policy |
> | Eylem | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/write | Creates or updates a Web Application Firewall Policy |
> | Eylem | Microsoft.Network/ipGroups/delete | Deletes an IpGroup |
> | Eylem | Microsoft.Network/ipGroups/join/action | Joins an IpGroup. Not alertable. |
> | Eylem | Microsoft.Network/ipGroups/read | Gets an IpGroup |
> | Eylem | Microsoft.Network/ipGroups/write | Creates an IpGroup or Updates An Existing IpGroups |
> | Eylem | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Joins a load balancer backend address pool. Not Alertable. |
> | Eylem | Microsoft.Network/loadBalancers/backendAddressPools/read | Gets a load balancer backend address pool definition |
> | Eylem | Microsoft.Network/loadBalancers/delete | Deletes a load balancer |
> | Eylem | Microsoft.Network/loadBalancers/frontendIPConfigurations/join/action | Joins a Load Balancer Frontend IP Configuration. Not alertable. |
> | Eylem | Microsoft.Network/loadBalancers/frontendIPConfigurations/read | Gets a load balancer frontend IP configuration definition |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Joins a load balancer inbound NAT pool. Not alertable. |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatPools/read | Gets a load balancer inbound nat pool definition |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatRules/delete | Deletes a load balancer inbound nat rule |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Joins a load balancer inbound nat rule. Not Alertable. |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatRules/read | Gets a load balancer inbound nat rule definition |
> | Eylem | Microsoft.Network/loadBalancers/inboundNatRules/write | Creates a load balancer inbound nat rule or updates an existing load balancer inbound nat rule |
> | Eylem | Microsoft.Network/loadBalancers/loadBalancingRules/read | Gets a load balancer load balancing rule definition |
> | Eylem | Microsoft.Network/loadBalancers/networkInterfaces/read | Gets references to all the network interfaces under a load balancer |
> | Eylem | Microsoft.Network/loadBalancers/outboundRules/read | Gets a load balancer outbound rule definition |
> | Eylem | Microsoft.Network/loadBalancers/probes/join/action | Allows using probes of a load balancer. For example, with this permission healthProbe property of VM scale set can reference the probe. Not alertable. |
> | Eylem | Microsoft.Network/loadBalancers/probes/read | Gets a load balancer probe |
> | Eylem | Microsoft.Network/loadBalancers/read | Gets a load balancer definition |
> | Eylem | Microsoft.Network/loadBalancers/virtualMachines/read | Gets references to all the virtual machines under a load balancer |
> | Eylem | Microsoft.Network/loadBalancers/write | Creates a load balancer or updates an existing load balancer |
> | Eylem | Microsoft.Network/localnetworkgateways/delete | Deletes LocalNetworkGateway |
> | Eylem | Microsoft.Network/localnetworkgateways/read | Gets LocalNetworkGateway |
> | Eylem | Microsoft.Network/localnetworkgateways/write | Creates or updates an existing LocalNetworkGateway |
> | Eylem | Microsoft.Network/locations/autoApprovedPrivateLinkServices/read | Gets Auto Approved Private Link Services |
> | Eylem | Microsoft.Network/locations/availableDelegations/read | Gets Available Delegations |
> | Eylem | Microsoft.Network/locations/availablePrivateEndpointTypes/read | Gets available Private Endpoint resources |
> | Eylem | Microsoft.Network/locations/availableServiceAliases/read | Gets Available Service Aliases |
> | Eylem | Microsoft.Network/locations/bareMetalTenants/action | Allocates or validates a Bare Metal Tenant |
> | Eylem | Microsoft.Network/locations/checkAcceleratedNetworkingSupport/action | Checks Accelerated Networking support |
> | Eylem | Microsoft.Network/locations/checkDnsNameAvailability/read | Checks if dns label is available at the specified location |
> | Eylem | Microsoft.Network/locations/checkPrivateLinkServiceVisibility/action | Checks Private Link Service Visibility |
> | Eylem | Microsoft.Network/locations/operationResults/read | Gets operation result of an async POST or DELETE operation |
> | Eylem | Microsoft.Network/locations/operations/read | Gets operation resource that represents status of an asynchronous operation |
> | Eylem | Microsoft.Network/locations/serviceTags/read | Get Service Tags |
> | Eylem | Microsoft.Network/locations/supportedVirtualMachineSizes/read | Gets supported virtual machines sizes |
> | Eylem | Microsoft.Network/locations/usages/read | Gets the resources usage metrics |
> | Eylem | Microsoft.Network/locations/virtualNetworkAvailableEndpointServices/read | Gets a list of available Virtual Network Endpoint Services |
> | Eylem | Microsoft.Network/networkIntentPolicies/delete | Deletes an Network Intent Policy |
> | Eylem | Microsoft.Network/networkIntentPolicies/read | Gets an Network Intent Policy Description |
> | Eylem | Microsoft.Network/networkIntentPolicies/write | Creates an Network Intent Policy or updates an existing Network Intent Policy |
> | Eylem | Microsoft.Network/networkInterfaces/delete | Deletes a network interface |
> | Eylem | Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action | Get Network Security Groups configured On Network Interface Of The Vm |
> | Eylem | Microsoft.Network/networkInterfaces/effectiveRouteTable/action | Get Route Table configured On Network Interface Of The Vm |
> | Eylem | Microsoft.Network/networkInterfaces/ipconfigurations/join/action | Joins a Network Interface IP Configuration. Not alertable. |
> | Eylem | Microsoft.Network/networkInterfaces/ipconfigurations/read | Gets a network interface ip configuration definition.  |
> | Eylem | Microsoft.Network/networkInterfaces/join/action | Joins a Virtual Machine to a network interface. Not Alertable. |
> | Eylem | Microsoft.Network/networkInterfaces/loadBalancers/read | Gets all the load balancers that the network interface is part of |
> | Eylem | Microsoft.Network/networkInterfaces/read | Gets a network interface definition.  |
> | Eylem | Microsoft.Network/networkInterfaces/tapConfigurations/delete | Deletes a Network Interface Tap Configuration. |
> | Eylem | Microsoft.Network/networkInterfaces/tapConfigurations/read | Gets a Network Interface Tap Configuration. |
> | Eylem | Microsoft.Network/networkInterfaces/tapConfigurations/write | Creates a Network Interface Tap Configuration or updates an existing Network Interface Tap Configuration. |
> | Eylem | Microsoft.Network/networkInterfaces/write | Creates a network interface or updates an existing network interface.  |
> | Eylem | Microsoft.Network/networkProfiles/delete | Deletes a Network Profile |
> | Eylem | Microsoft.Network/networkProfiles/read | Gets a Network Profile |
> | Eylem | Microsoft.Network/networkProfiles/removeContainers/action | Removes Containers |
> | Eylem | Microsoft.Network/networkProfiles/setContainers/action | Sets Containers |
> | Eylem | Microsoft.Network/networkProfiles/setNetworkInterfaces/action | Sets Container Network Interfaces |
> | Eylem | Microsoft.Network/networkProfiles/write | Creates or updates a Network Profile |
> | Eylem | Microsoft.Network/networkSecurityGroups/defaultSecurityRules/read | Gets a default security rule definition |
> | Eylem | Microsoft.Network/networkSecurityGroups/delete | Deletes a network security group |
> | Eylem | Microsoft.Network/networkSecurityGroups/join/action | Joins a network security group. Not Alertable. |
> | Eylem | Microsoft.Network/networkSecurityGroups/read | Gets a network security group definition |
> | Eylem | Microsoft.Network/networkSecurityGroups/securityRules/delete | Deletes a security rule |
> | Eylem | Microsoft.Network/networkSecurityGroups/securityRules/read | Gets a security rule definition |
> | Eylem | Microsoft.Network/networkSecurityGroups/securityRules/write | Creates a security rule or updates an existing security rule |
> | Eylem | Microsoft.Network/networkSecurityGroups/write | Creates a network security group or updates an existing network security group |
> | Eylem | Microsoft.Network/networkWatchers/availableProvidersList/action | Returns all available internet service providers for a specified Azure region. |
> | Eylem | Microsoft.Network/networkWatchers/azureReachabilityReport/action | Returns the relative latency score for internet service providers from a specified location to Azure regions. |
> | Eylem | Microsoft.Network/networkWatchers/configureFlowLog/action | Configures flow logging for a target resource. |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/delete | Deletes a Connection Monitor |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/query/action | Query monitoring connectivity between specified endpoints |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/read | Get Connection Monitor details |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/start/action | Start monitoring connectivity between specified endpoints |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/stop/action | Stop/pause monitoring connectivity between specified endpoints |
> | Eylem | Microsoft.Network/networkWatchers/connectionMonitors/write | Creates a Connection Monitor |
> | Eylem | Microsoft.Network/networkWatchers/connectivityCheck/action | Verifies the possibility of establishing a direct TCP connection from a virtual machine to a given endpoint including another VM or an arbitrary remote server. |
> | Eylem | Microsoft.Network/networkWatchers/delete | Deletes a network watcher |
> | Eylem | Microsoft.Network/networkWatchers/flowLogs/delete | Deletes a Flow Log |
> | Eylem | Microsoft.Network/networkWatchers/flowLogs/read | Get Flow Log details |
> | Eylem | Microsoft.Network/networkWatchers/flowLogs/write | Creates a Flow Log |
> | Eylem | Microsoft.Network/networkWatchers/ipFlowVerify/action | Returns whether the packet is allowed or denied to or from a particular destination. |
> | Eylem | Microsoft.Network/networkWatchers/lenses/delete | Deletes a Lens |
> | Eylem | Microsoft.Network/networkWatchers/lenses/query/action | Query monitoring network traffic on a specified endpoint |
> | Eylem | Microsoft.Network/networkWatchers/lenses/read | Get Lens details |
> | Eylem | Microsoft.Network/networkWatchers/lenses/start/action | Start monitoring network traffic on a specified endpoint |
> | Eylem | Microsoft.Network/networkWatchers/lenses/stop/action | Stop/pause monitoring network traffic on a specified endpoint |
> | Eylem | Microsoft.Network/networkWatchers/lenses/write | Creates a Lens |
> | Eylem | Microsoft.Network/networkWatchers/networkConfigurationDiagnostic/action | Diagnostic of network configuration. |
> | Eylem | Microsoft.Network/networkWatchers/nextHop/action | For a specified target and destination IP address, return the next hop type and next hope IP address. |
> | Eylem | Microsoft.Network/networkWatchers/packetCaptures/delete | Deletes a packet capture |
> | Eylem | Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Gets information about properties and status of a packet capture resource. |
> | Eylem | Microsoft.Network/networkWatchers/packetCaptures/read | Get the packet capture definition |
> | Eylem | Microsoft.Network/networkWatchers/packetCaptures/stop/action | Stop the running packet capture session. |
> | Eylem | Microsoft.Network/networkWatchers/packetCaptures/write | Creates a packet capture |
> | Eylem | Microsoft.Network/networkWatchers/pingMeshes/delete | Deletes a PingMesh |
> | Eylem | Microsoft.Network/networkWatchers/pingMeshes/read | Get PingMesh details |
> | Eylem | Microsoft.Network/networkWatchers/pingMeshes/start/action | Start PingMesh between specified VMs |
> | Eylem | Microsoft.Network/networkWatchers/pingMeshes/stop/action | Stop PingMesh between specified VMs |
> | Eylem | Microsoft.Network/networkWatchers/pingMeshes/write | Creates a PingMesh |
> | Eylem | Microsoft.Network/networkWatchers/queryConnectionMonitors/action | Batch query monitoring connectivity between specified endpoints |
> | Eylem | Microsoft.Network/networkWatchers/queryFlowLogStatus/action | Gets the status of flow logging on a resource. |
> | Eylem | Microsoft.Network/networkWatchers/queryTroubleshootResult/action | Gets the troubleshooting result from the previously run or currently running troubleshooting operation. |
> | Eylem | Microsoft.Network/networkWatchers/read | Get the network watcher definition |
> | Eylem | Microsoft.Network/networkWatchers/securityGroupView/action | View the configured and effective network security group rules applied on a VM. |
> | Eylem | Microsoft.Network/networkWatchers/topology/action | Gets a network level view of resources and their relationships in a resource group. |
> | Eylem | Microsoft.Network/networkWatchers/troubleshoot/action | Starts troubleshooting on a Networking resource in Azure. |
> | Eylem | Microsoft.Network/networkWatchers/write | Creates a network watcher or updates an existing network watcher |
> | Eylem | Microsoft.Network/operations/read | Get Available Operations |
> | Eylem | Microsoft.Network/p2sVpnGateways/delete | Deletes a P2SVpnGateway. |
> | Eylem | Microsoft.Network/p2sVpnGateways/disconnectp2svpnconnections/action | Disconnect p2s vpn connections |
> | Eylem | Microsoft.Network/p2sVpnGateways/generatevpnprofile/action | Generate Vpn Profile for P2SVpnGateway |
> | Eylem | Microsoft.Network/p2sVpnGateways/getp2svpnconnectionhealth/action | Gets a P2S Vpn Connection health for P2SVpnGateway |
> | Eylem | Microsoft.Network/p2sVpnGateways/getp2svpnconnectionhealthdetailed/action | Gets a P2S Vpn Connection health detailed for P2SVpnGateway |
> | Eylem | Microsoft.Network/p2sVpnGateways/read | Gets a P2SVpnGateway. |
> | Eylem | Microsoft.Network/p2sVpnGateways/write | Puts a P2SVpnGateway. |
> | Eylem | Microsoft.Network/privateDnsOperationResults/read | Gets results of a Private DNS operation |
> | Eylem | Microsoft.Network/privateDnsOperationStatuses/read | Gets status of a Private DNS operation |
> | Eylem | Microsoft.Network/privateDnsZones/A/delete | Remove the record set of a given name and type ‘A’ from a Private DNS zone. |
> | Eylem | Microsoft.Network/privateDnsZones/A/read | Get the record set of type ‘A’ within a Private DNS zone, in JSON format. The record set contains a list of records as well as the TTL, tags, and etag. |
> | Eylem | Microsoft.Network/privateDnsZones/A/write | Create or update a record set of type ‘A’ within a Private DNS zone. The records specified will replace the current records in the record set. |
> | Eylem | Microsoft.Network/privateDnsZones/AAAA/delete | Remove the record set of a given name and type ‘AAAA’ from a Private DNS zone. |
> | Eylem | Microsoft.Network/privateDnsZones/AAAA/read | Get the record set of type ‘AAAA’ within a Private DNS zone, in JSON format. The record set contains a list of records as well as the TTL, tags, and etag. |
> | Eylem | Microsoft.Network/privateDnsZones/AAAA/write | Create or update a record set of type ‘AAAA’ within a Private DNS zone. The records specified will replace the current records in the record set. |
> | Eylem | Microsoft.Network/privateDnsZones/ALL/read | Gets Private DNS record sets across types |
> | Eylem | Microsoft.Network/privateDnsZones/CNAME/delete | Remove the record set of a given name and type ‘CNAME’ from a Private DNS zone. |
> | Eylem | Microsoft.Network/privateDnsZones/CNAME/read | Get the record set of type ‘CNAME’ within a Private DNS zone, in JSON format. |
> | Eylem | Microsoft.Network/privateDnsZones/CNAME/write | Create or update a record set of type ‘CNAME’ within a Private DNS zone. |
> | Eylem | Microsoft.Network/privateDnsZones/delete | Delete a Private DNS zone. |
> | Eylem | Microsoft.Network/privateDnsZones/MX/delete | Remove the record set of a given name and type ‘MX’ from a Private DNS zone. |
> | Eylem | Microsoft.Network/privateDnsZones/MX/read | Get the record set of type ‘MX’ within a Private DNS zone, in JSON format. The record set contains a list of records as well as the TTL, tags, and etag. |
> | Eylem | Microsoft.Network/privateDnsZones/MX/write | Create or update a record set of type ‘MX’ within a Private DNS zone. The records specified will replace the current records in the record set. |
> | Eylem | Microsoft.Network/privateDnsZones/PTR/delete | Remove the record set of a given name and type ‘PTR’ from a Private DNS zone. |
> | Eylem | Microsoft.Network/privateDnsZones/PTR/read | Get the record set of type ‘PTR’ within a Private DNS zone, in JSON format. The record set contains a list of records as well as the TTL, tags, and etag. |
> | Eylem | Microsoft.Network/privateDnsZones/PTR/write | Create or update a record set of type ‘PTR’ within a Private DNS zone. The records specified will replace the current records in the record set. |
> | Eylem | Microsoft.Network/privateDnsZones/read | Get the Private DNS zone properties, in JSON format. Note that this command does not retrieve the virtual networks to which the Private DNS zone is linked or the record sets contained within the zone. |
> | Eylem | Microsoft.Network/privateDnsZones/recordsets/read | Gets Private DNS record sets across types |
> | Eylem | Microsoft.Network/privateDnsZones/SOA/read | Get the record set of type ‘SOA’ within a Private DNS zone, in JSON format. |
> | Eylem | Microsoft.Network/privateDnsZones/SOA/write | Update a record set of type ‘SOA’ within a Private DNS zone. |
> | Eylem | Microsoft.Network/privateDnsZones/SRV/delete | Remove the record set of a given name and type ‘SRV’ from a Private DNS zone. |
> | Eylem | Microsoft.Network/privateDnsZones/SRV/read | Get the record set of type ‘SRV’ within a Private DNS zone, in JSON format. The record set contains a list of records as well as the TTL, tags, and etag. |
> | Eylem | Microsoft.Network/privateDnsZones/SRV/write | Create or update a record set of type ‘SRV’ within a Private DNS zone. The records specified will replace the current records in the record set. |
> | Eylem | Microsoft.Network/privateDnsZones/TXT/delete | Remove the record set of a given name and type ‘TXT’ from a Private DNS zone. |
> | Eylem | Microsoft.Network/privateDnsZones/TXT/read | Get the record set of type ‘TXT’ within a Private DNS zone, in JSON format. The record set contains a list of records as well as the TTL, tags, and etag. |
> | Eylem | Microsoft.Network/privateDnsZones/TXT/write | Create or update a record set of type ‘TXT’ within a Private DNS zone. The records specified will replace the current records in the record set. |
> | Eylem | Microsoft.Network/privateDnsZones/virtualNetworkLinks/delete | Delete a Private DNS zone link to virtual network. |
> | Eylem | Microsoft.Network/privateDnsZones/virtualNetworkLinks/read | Get the Private DNS zone link to virtual network properties, in JSON format. |
> | Eylem | Microsoft.Network/privateDnsZones/virtualNetworkLinks/write | Create or update a Private DNS zone link to virtual network. |
> | Eylem | Microsoft.Network/privateDnsZones/write | Create or update a Private DNS zone within a resource group. Note that this command cannot be used to create or update virtual network links or record sets within the zone. |
> | Eylem | Microsoft.Network/privateEndpointRedirectMaps/read | Gets a Private Endpoint RedirectMap |
> | Eylem | Microsoft.Network/privateEndpointRedirectMaps/write | Creates Private Endpoint RedirectMap Or Updates An Existing Private Endpoint RedirectMap |
> | Eylem | Microsoft.Network/privateEndpoints/delete | Deletes an private endpoint resource. |
> | Eylem | Microsoft.Network/privateEndpoints/read | Gets an private endpoint resource. |
> | Eylem | Microsoft.Network/privateEndpoints/write | Creates a new private endpoint, or updates an existing private endpoint. |
> | Eylem | Microsoft.Network/privateLinkServices/delete | Deletes an private link service resource. |
> | Eylem | Microsoft.Network/privateLinkServices/privateEndpointConnections/delete | Deletes an private endpoint connection. |
> | Eylem | Microsoft.Network/privateLinkServices/privateEndpointConnections/read | Gets an private endpoint connection definition. |
> | Eylem | Microsoft.Network/privateLinkServices/privateEndpointConnections/write | Creates a new private endpoint connection, or updates an existing private endpoint connection. |
> | Eylem | Microsoft.Network/privateLinkServices/read | Gets an private link service resource. |
> | Eylem | Microsoft.Network/privateLinkServices/write | Creates a new private link service, or updates an existing private link service. |
> | Eylem | Microsoft.Network/publicIPAddresses/delete | Deletes a public Ip address. |
> | Eylem | Microsoft.Network/publicIPAddresses/join/action | Joins a public ip address. Not Alertable. |
> | Eylem | Microsoft.Network/publicIPAddresses/read | Gets a public ip address definition. |
> | Eylem | Microsoft.Network/publicIPAddresses/write | Creates a public Ip address or updates an existing public Ip address.  |
> | Eylem | Microsoft.Network/publicIPPrefixes/delete | Deletes A Public Ip Prefix |
> | Eylem | Microsoft.Network/publicIPPrefixes/join/action | Joins a PublicIPPrefix. Not alertable. |
> | Eylem | Microsoft.Network/publicIPPrefixes/read | Gets a Public Ip Prefix Definition |
> | Eylem | Microsoft.Network/publicIPPrefixes/write | Creates A Public Ip Prefix Or Updates An Existing Public Ip Prefix |
> | Eylem | Microsoft.Network/register/action | Registers the subscription |
> | Eylem | Microsoft.Network/routeFilters/delete | Deletes a route filter definition |
> | Eylem | Microsoft.Network/routeFilters/join/action | Joins a route filter. Not Alertable. |
> | Eylem | Microsoft.Network/routeFilters/read | Gets a route filter definition |
> | Eylem | Microsoft.Network/routeFilters/routeFilterRules/delete | Deletes a route filter rule definition |
> | Eylem | Microsoft.Network/routeFilters/routeFilterRules/read | Gets a route filter rule definition |
> | Eylem | Microsoft.Network/routeFilters/routeFilterRules/write | Creates a route filter rule or Updates an existing route filter rule |
> | Eylem | Microsoft.Network/routeFilters/write | Creates a route filter or Updates an existing route filter |
> | Eylem | Microsoft.Network/routeTables/delete | Deletes a route table definition |
> | Eylem | Microsoft.Network/routeTables/join/action | Joins a route table. Not Alertable. |
> | Eylem | Microsoft.Network/routeTables/read | Gets a route table definition |
> | Eylem | Microsoft.Network/routeTables/routes/delete | Deletes a route definition |
> | Eylem | Microsoft.Network/routeTables/routes/read | Gets a route definition |
> | Eylem | Microsoft.Network/routeTables/routes/write | Creates a route or Updates an existing route |
> | Eylem | Microsoft.Network/routeTables/write | Creates a route table or Updates an existing route table |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/delete | Deletes a Service Endpoint Policy |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/join/action | Joins a Service Endpoint Policy. Not alertable. |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/joinSubnet/action | Joins a Subnet To Service Endpoint Policies. Not alertable. |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/read | Gets a Service Endpoint Policy Description |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/delete | Deletes a Service Endpoint Policy Definition |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/read | Gets a Service Endpoint Policy Definition Description |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/write | Creates a Service Endpoint Policy Definition or updates an existing Service Endpoint Policy Definition |
> | Eylem | Microsoft.Network/serviceEndpointPolicies/write | Creates a Service Endpoint Policy or updates an existing Service Endpoint Policy |
> | Eylem | Microsoft.Network/trafficManagerGeographicHierarchies/read | Gets the Traffic Manager Geographic Hierarchy containing regions which can be used with the Geographic traffic routing method |
> | Eylem | Microsoft.Network/trafficManagerProfiles/azureEndpoints/delete | Deletes an Azure Endpoint from an existing Traffic Manager Profile. Traffic Manager will stop routing traffic to the deleted Azure Endpoint. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/azureEndpoints/read | Gets an Azure Endpoint which belongs to a Traffic Manager Profile, including all the properties of that Azure Endpoint. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/azureEndpoints/write | Add a new Azure Endpoint in an existing Traffic Manager Profile or update the properties of an existing Azure Endpoint in that Traffic Manager Profile. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/delete | Delete the Traffic Manager profile. All settings associated with the Traffic Manager profile will be lost, and the profile can no longer be used to route traffic. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/externalEndpoints/delete | Deletes an External Endpoint from an existing Traffic Manager Profile. Traffic Manager will stop routing traffic to the deleted External Endpoint. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/externalEndpoints/read | Gets an External Endpoint which belongs to a Traffic Manager Profile, including all the properties of that External Endpoint. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/externalEndpoints/write | Add a new External Endpoint in an existing Traffic Manager Profile or update the properties of an existing External Endpoint in that Traffic Manager Profile. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/heatMaps/read | Gets the Traffic Manager Heat Map for the given Traffic Manager profile which contains query counts and latency data by location and source IP. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/delete | Deletes an Nested Endpoint from an existing Traffic Manager Profile. Traffic Manager will stop routing traffic to the deleted Nested Endpoint. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/read | Gets an Nested Endpoint which belongs to a Traffic Manager Profile, including all the properties of that Nested Endpoint. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/write | Add a new Nested Endpoint in an existing Traffic Manager Profile or update the properties of an existing Nested Endpoint in that Traffic Manager Profile. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/read | Get the Traffic Manager profile configuration. This includes DNS settings, traffic routing settings, endpoint monitoring settings, and the list of endpoints routed by this Traffic Manager profile. |
> | Eylem | Microsoft.Network/trafficManagerProfiles/write | Create a Traffic Manager profile, or modify the configuration of an existing Traffic Manager profile.<br>This includes enabling or disabling a profile and modifying DNS settings, traffic routing settings, or endpoint monitoring settings.<br>Endpoints routed by the Traffic Manager profile can be added, removed, enabled or disabled. |
> | Eylem | Microsoft.Network/trafficManagerUserMetricsKeys/delete | Deletes the subscription-level key used for Realtime User Metrics collection. |
> | Eylem | Microsoft.Network/trafficManagerUserMetricsKeys/read | Gets the subscription-level key used for Realtime User Metrics collection. |
> | Eylem | Microsoft.Network/trafficManagerUserMetricsKeys/write | Creates a new subscription-level key to be used for Realtime User Metrics collection. |
> | Eylem | Microsoft.Network/unregister/action | Unregisters the subscription |
> | Eylem | Microsoft.Network/virtualHubs/delete | Deletes a Virtual Hub |
> | Eylem | Microsoft.Network/virtualHubs/effectiveRoutes/action | Gets effective route configured on Virtual Hub |
> | Eylem | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/delete | Deletes a HubVirtualNetworkConnection |
> | Eylem | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/read | Get a HubVirtualNetworkConnection |
> | Eylem | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/write | Create or update a HubVirtualNetworkConnection |
> | Eylem | Microsoft.Network/virtualHubs/read | Get a Virtual Hub |
> | Eylem | Microsoft.Network/virtualHubs/routeTables/delete | Delete a VirtualHubRouteTableV2 |
> | Eylem | Microsoft.Network/virtualHubs/routeTables/read | Get a VirtualHubRouteTableV2 |
> | Eylem | Microsoft.Network/virtualHubs/routeTables/write | Create or Update a VirtualHubRouteTableV2 |
> | Eylem | Microsoft.Network/virtualHubs/write | Create or update a Virtual Hub |
> | Eylem | microsoft.network/virtualnetworkgateways/connections/read | Get VirtualNetworkGatewayConnection |
> | Eylem | Microsoft.Network/virtualNetworkGateways/delete | Deletes a virtualNetworkGateway |
> | Eylem | microsoft.network/virtualnetworkgateways/disconnectvirtualnetworkgatewayvpnconnections/action | Disconnect virtual network gateway vpn connections |
> | Eylem | microsoft.network/virtualnetworkgateways/generatevpnclientpackage/action | Generate VpnClient package for virtualNetworkGateway |
> | Eylem | microsoft.network/virtualnetworkgateways/generatevpnprofile/action | Generate VpnProfile package for VirtualNetworkGateway |
> | Eylem | microsoft.network/virtualnetworkgateways/getadvertisedroutes/action | Gets virtualNetworkGateway advertised routes |
> | Eylem | microsoft.network/virtualnetworkgateways/getbgppeerstatus/action | Gets virtualNetworkGateway bgp peer status |
> | Eylem | microsoft.network/virtualnetworkgateways/getlearnedroutes/action | Gets virtualnetworkgateway learned routes |
> | Eylem | microsoft.network/virtualnetworkgateways/getvpnclientconnectionhealth/action | Get Per Vpn Client Connection Health for VirtualNetworkGateway |
> | Eylem | microsoft.network/virtualnetworkgateways/getvpnclientipsecparameters/action | Get Vpnclient Ipsec parameters for VirtualNetworkGateway P2S client. |
> | Eylem | microsoft.network/virtualnetworkgateways/getvpnprofilepackageurl/action | Gets the URL of a pre-generated vpn client profile package |
> | Eylem | Microsoft.Network/virtualNetworkGateways/read | Gets a VirtualNetworkGateway |
> | Eylem | microsoft.network/virtualnetworkgateways/reset/action | Resets a virtualNetworkGateway |
> | Eylem | microsoft.network/virtualnetworkgateways/resetvpnclientsharedkey/action | Reset Vpnclient shared key for VirtualNetworkGateway P2S client. |
> | Eylem | microsoft.network/virtualnetworkgateways/setvpnclientipsecparameters/action | Set Vpnclient Ipsec parameters for VirtualNetworkGateway P2S client. |
> | Eylem | microsoft.network/virtualnetworkgateways/startpacketcapture/action | Starts a Virtual Network Gateway Packet Capture. |
> | Eylem | microsoft.network/virtualnetworkgateways/stoppacketcapture/action | Stops a Virtual Network Gateway Packet Capture. |
> | Eylem | Microsoft.Network/virtualnetworkgateways/supportedvpndevices/action | Lists Supported Vpn Devices |
> | Eylem | Microsoft.Network/virtualNetworkGateways/write | Creates or updates a VirtualNetworkGateway |
> | Eylem | Microsoft.Network/virtualNetworks/BastionHosts/action | Gets Bastion Host references in a Virtual Network. |
> | Eylem | Microsoft.Network/virtualNetworks/bastionHosts/default/action | Gets Bastion Host references in a Virtual Network. |
> | Eylem | Microsoft.Network/virtualNetworks/checkIpAddressAvailability/read | Check if Ip Address is available at the specified virtual network |
> | Eylem | Microsoft.Network/virtualNetworks/delete | Deletes a virtual network |
> | Eylem | Microsoft.Network/virtualNetworks/join/action | Joins a virtual network. Not Alertable. |
> | Eylem | Microsoft.Network/virtualNetworks/peer/action | Peers a virtual network with another virtual network |
> | Eylem | Microsoft.Network/virtualNetworks/read | Get the virtual network definition |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/contextualServiceEndpointPolicies/delete | Deletes A Contextual Service Endpoint Policy |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/contextualServiceEndpointPolicies/read | Gets Contextual Service Endpoint Policies |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/contextualServiceEndpointPolicies/write | Creates a Contextual Service Endpoint Policy or updates an existing Contextual Service Endpoint Policy |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/delete | Deletes a virtual network subnet |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/join/action | Joins a virtual network. Not Alertable. |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Joins resource such as storage account or SQL database to a subnet. Not alertable. |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/prepareNetworkPolicies/action | Prepares a subnet by applying necessary Network Policies |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/read | Gets a virtual network subnet definition |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/unprepareNetworkPolicies/action | Unprepare a subnet by removing the applied Network Policies |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/virtualMachines/read | Gets references to all the virtual machines in a virtual network subnet |
> | Eylem | Microsoft.Network/virtualNetworks/subnets/write | Creates a virtual network subnet or updates an existing virtual network subnet |
> | Eylem | Microsoft.Network/virtualNetworks/usages/read | Get the IP usages for each subnet of the virtual network |
> | Eylem | Microsoft.Network/virtualNetworks/virtualMachines/read | Gets references to all the virtual machines in a virtual network |
> | Eylem | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/delete | Deletes a virtual network peering |
> | Eylem | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/read | Gets a virtual network peering definition |
> | Eylem | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write | Creates a virtual network peering or updates an existing virtual network peering |
> | Eylem | Microsoft.Network/virtualNetworks/write | Creates a virtual network or updates an existing virtual network |
> | Eylem | Microsoft.Network/virtualNetworkTaps/delete | Delete Virtual Network Tap |
> | Eylem | Microsoft.Network/virtualNetworkTaps/join/action | Joins a virtual network tap. Not Alertable. |
> | Eylem | Microsoft.Network/virtualNetworkTaps/read | Get Virtual Network Tap |
> | Eylem | Microsoft.Network/virtualNetworkTaps/write | Create or Update Virtual Network Tap |
> | Eylem | Microsoft.Network/virtualRouters/delete | Deletes A VirtualRouter |
> | Eylem | Microsoft.Network/virtualRouters/join/action | Joins A VirtualRouter. Not alertable. |
> | Eylem | Microsoft.Network/virtualRouters/peerings/delete | Deletes A VirtualRouterPeering |
> | Eylem | Microsoft.Network/virtualRouters/peerings/read | Gets A VirtualRouterPeering |
> | Eylem | Microsoft.Network/virtualRouters/peerings/write | Creates A VirtualRouterPeering or Updates An Existing VirtualRouterPeering |
> | Eylem | Microsoft.Network/virtualRouters/read | Gets A VirtualRouter |
> | Eylem | Microsoft.Network/virtualRouters/write | Creates A VirtualRouter or Updates An Existing VirtualRouter |
> | Eylem | Microsoft.Network/virtualWans/delete | Deletes a Virtual Wan |
> | Eylem | Microsoft.Network/virtualwans/generateVpnProfile/action | Generate VirtualWanVpnServerConfiguration VpnProfile |
> | Eylem | Microsoft.network/virtualWans/p2sVpnServerConfigurations/delete | Deletes a virtual Wan P2SVpnServerConfiguration |
> | Eylem | Microsoft.Network/virtualWans/p2sVpnServerConfigurations/read | Gets a virtual Wan P2SVpnServerConfiguration |
> | Eylem | Microsoft.network/virtualWans/p2sVpnServerConfigurations/write | Creates a virtual Wan P2SVpnServerConfiguration or updates an existing virtual Wan P2SVpnServerConfiguration |
> | Eylem | Microsoft.Network/virtualWans/read | Get a Virtual Wan |
> | Eylem | Microsoft.Network/virtualwans/supportedSecurityProviders/read | Gets supported VirtualWan Security Providers. |
> | Eylem | Microsoft.Network/virtualWans/virtualHubs/read | Gets all Virtual Hubs that reference a Virtual Wan. |
> | Eylem | Microsoft.Network/virtualwans/vpnconfiguration/action | Gets a Vpn Configuration |
> | Eylem | Microsoft.Network/virtualwans/vpnServerConfigurations/action | Get VirtualWanVpnServerConfigurations |
> | Eylem | Microsoft.Network/virtualWans/vpnSites/read | Gets all VPN Sites that reference a Virtual Wan. |
> | Eylem | Microsoft.Network/virtualWans/write | Create or update a Virtual Wan |
> | Eylem | Microsoft.Network/vpnGateways/delete | Deletes a VpnGateway. |
> | Eylem | microsoft.network/vpngateways/listvpnconnectionshealth/action | Gets connection health for all or a subset of connections on a VpnGateway |
> | Eylem | Microsoft.Network/vpnGateways/read | Gets a VpnGateway. |
> | Eylem | microsoft.network/vpngateways/reset/action | Resets a VpnGateway |
> | Eylem | microsoft.network/vpnGateways/vpnConnections/delete | Deletes a VpnConnection. |
> | Eylem | microsoft.network/vpnGateways/vpnConnections/read | Gets a VpnConnection. |
> | Eylem | microsoft.network/vpnGateways/vpnConnections/vpnLinkConnections/read | Gets a Vpn Link Connection |
> | Eylem | microsoft.network/vpnGateways/vpnConnections/write | Puts a VpnConnection. |
> | Eylem | Microsoft.Network/vpnGateways/write | Puts a VpnGateway. |
> | Eylem | Microsoft.Network/vpnServerConfigurations/delete | Delete VpnServerConfiguration |
> | Eylem | Microsoft.Network/vpnServerConfigurations/read | Get VpnServerConfiguration |
> | Eylem | Microsoft.Network/vpnServerConfigurations/write | Create or Update VpnServerConfiguration |
> | Eylem | Microsoft.Network/vpnsites/delete | Deletes a Vpn Site resource. |
> | Eylem | Microsoft.Network/vpnsites/read | Gets a Vpn Site resource. |
> | Eylem | microsoft.network/vpnSites/vpnSiteLinks/read | Gets a Vpn Site Link |
> | Eylem | Microsoft.Network/vpnsites/write | Creates or updates a Vpn Site resource. |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.NotificationHubs/CheckNamespaceAvailability/action | Checks whether or not a given Namespace resource name is available within the NotificationHub service. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/action | Get the list of Namespaces Authorization Rules description. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/delete | Delete Namespace Authorization Rule. The Default Namespace Authorization Rule cannot be deleted.  |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/listkeys/action | Get the Connection String to the Namespace |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/read | Get the list of Namespaces Authorization Rules description. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/regenerateKeys/action | Namespace Authorization Rule Regenerate Primary/SecondaryKey, Specify the Key that needs to be regenerated |
> | Eylem | Microsoft.NotificationHubs/Namespaces/authorizationRules/write | Create a Namespace level Authorization Rules and update its properties. The Authorization Rules Access Rights, the Primary and Secondary Keys can be updated. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/CheckNotificationHubAvailability/action | Checks whether or not a given NotificationHub name is available inside a Namespace. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/Delete | Delete Namespace Resource |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/action | Get the list of Notification Hub Authorization Rules |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/delete | Delete Notification Hub Authorization Rules |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/listkeys/action | Get the Connection String to the Notification Hub |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/read | Get the list of Notification Hub Authorization Rules |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/regenerateKeys/action | Notification Hub Authorization Rule Regenerate Primary/SecondaryKey, Specify the Key that needs to be regenerated |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/write | Create Notification Hub Authorization Rules and Update its properties. The Authorization Rules Access Rights, the Primary and Secondary Keys can be updated. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/debugSend/action | Send a test push notification. |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/Delete | Delete Notification Hub Resource |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/metricDefinitions/read | Get list of Namespace metrics Resource Descriptions |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/pnsCredentials/action | Get All Notification Hub PNS Credentials. This includes, WNS, MPNS, APNS, GCM and Baidu credentials |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/read | Get list of Notification Hub Resource Descriptions |
> | Eylem | Microsoft.NotificationHubs/Namespaces/NotificationHubs/write | Create a Notification Hub and Update its properties. Its properties mainly include PNS Credentials. Authorization Rules and TTL |
> | Eylem | Microsoft.NotificationHubs/Namespaces/read | Get the list of Namespace Resource Description |
> | Eylem | Microsoft.NotificationHubs/Namespaces/write | Create a Namespace Resource and Update its properties. Tags and Capacity of the Namespace are the properties which can be updated. |
> | Eylem | Microsoft.NotificationHubs/operationResults/read | Returns operation results for Notification Hubs provider |
> | Eylem | Microsoft.NotificationHubs/operations/read | Returns a list of supported operations for Notification Hubs provider |
> | Eylem | Microsoft.NotificationHubs/register/action | Registers the subscription for the NotificationHubs resource provider and enables the creation of Namespaces and NotificationHubs |
> | Eylem | Microsoft.NotificationHubs/unregister/action | Unregisters the subscription for the NotificationHubs resource provider and enables the creation of Namespaces and NotificationHubs |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.OffAzure/HyperVSites/clusters/read | Gets the properties of a Hyper-V cluster |
> | Eylem | Microsoft.OffAzure/HyperVSites/clusters/write | Creates or updates the Hyper-V cluster |
> | Eylem | Microsoft.OffAzure/HyperVSites/delete | Deletes the Hyper-V site |
> | Eylem | Microsoft.OffAzure/HyperVSites/healthsummary/read | Gets the health summary for Hyper-V resource |
> | Eylem | Microsoft.OffAzure/HyperVSites/hosts/read | Gets the properties of a Hyper-V host |
> | Eylem | Microsoft.OffAzure/HyperVSites/hosts/write | Creates or updates the Hyper-V host |
> | Eylem | Microsoft.OffAzure/HyperVSites/jobs/read | Gets the properties of a Hyper-V jobs |
> | Eylem | Microsoft.OffAzure/HyperVSites/machines/read | Gets the properties of a Hyper-V machines |
> | Eylem | Microsoft.OffAzure/HyperVSites/operationsstatus/read | Gets the properties of a Hyper-V operation status |
> | Eylem | Microsoft.OffAzure/HyperVSites/read | Gets the properties of a Hyper-V site |
> | Eylem | Microsoft.OffAzure/HyperVSites/refresh/action | Refreshes the objects within a Hyper-V site |
> | Eylem | Microsoft.OffAzure/HyperVSites/runasaccounts/read | Gets the properties of a Hyper-V run as accounts |
> | Eylem | Microsoft.OffAzure/HyperVSites/usage/read | Gets the usages of a Hyper-V site |
> | Eylem | Microsoft.OffAzure/HyperVSites/write | Creates or updates the Hyper-V site |
> | Eylem | Microsoft.OffAzure/Operations/read | Reads the exposed operations |
> | Eylem | Microsoft.OffAzure/register/action | Registers Subscription with Microsoft.OffAzure resource provider |
> | Eylem | Microsoft.OffAzure/ServerSites/jobs/read | Gets the properties of a Server jobs |
> | Eylem | Microsoft.OffAzure/ServerSites/machines/read | Gets the properties of a Server machines |
> | Eylem | Microsoft.OffAzure/ServerSites/machines/write | Write the properties of a Server machines |
> | Eylem | Microsoft.OffAzure/ServerSites/operationsstatus/read | Gets the properties of a Server operation status |
> | Eylem | Microsoft.OffAzure/ServerSites/read | Gets the properties of a Server site |
> | Eylem | Microsoft.OffAzure/ServerSites/refresh/action | Refreshes the objects within a Server site |
> | Eylem | Microsoft.OffAzure/ServerSites/runasaccounts/read | Gets the properties of a Server run as accounts |
> | Eylem | Microsoft.OffAzure/ServerSites/usage/read | Gets the usages of a Server site |
> | Eylem | Microsoft.OffAzure/ServerSites/write | Creates or updates the Server site |
> | Eylem | Microsoft.OffAzure/VMwareSites/delete | Deletes the VMware site |
> | Eylem | Microsoft.OffAzure/VMwareSites/healthsummary/read | Gets the health summary for VMware resource |
> | Eylem | Microsoft.OffAzure/VMwareSites/jobs/read | Gets the properties of a VMware jobs |
> | Eylem | Microsoft.OffAzure/VMwareSites/machines/read | Gets the properties of a VMware machines |
> | Eylem | Microsoft.OffAzure/VMwareSites/machines/start/action | Start VMware machines |
> | Eylem | Microsoft.OffAzure/VMwareSites/machines/stop/action | Stops the VMware machines |
> | Eylem | Microsoft.OffAzure/VMwareSites/operationsstatus/read | Gets the properties of a VMware operation status |
> | Eylem | Microsoft.OffAzure/VMwareSites/read | Gets the properties of a VMware site |
> | Eylem | Microsoft.OffAzure/VMwareSites/refresh/action | Refreshes the objects within a VMware site |
> | Eylem | Microsoft.OffAzure/VMwareSites/runasaccounts/read | Gets the properties of a VMware run as accounts |
> | Eylem | Microsoft.OffAzure/VMwareSites/usage/read | Gets the usages of a VMware site |
> | Eylem | Microsoft.OffAzure/VMwareSites/vcenters/read | Gets the properties of a VMware vCenter |
> | Eylem | Microsoft.OffAzure/VMwareSites/vcenters/write | Creates or updates the VMware vCenter |
> | Eylem | Microsoft.OffAzure/VMwareSites/write | Creates or updates the VMware site |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.OperationalInsights/linkTargets/read | Lists existing accounts that are not associated with an Azure subscription. To link this Azure subscription to a workspace, use a customer id returned by this operation in the customer id property of the Create Workspace operation. |
> | Eylem | microsoft.operationalinsights/operations/read | Lists all of the available OperationalInsights Rest API operations. |
> | Eylem | microsoft.operationalinsights/register/action | Rergisters the subscription. |
> | Eylem | Microsoft.OperationalInsights/register/action | Register a subscription to a resource provider. |
> | Eylem | microsoft.operationalinsights/unregister/action | Unregisters the subscription. |
> | Eylem | Microsoft.OperationalInsights/workspaces/analytics/query/action | Search using new engine. |
> | Eylem | Microsoft.OperationalInsights/workspaces/analytics/query/schema/read | Get search schema V2. |
> | Eylem | Microsoft.OperationalInsights/workspaces/api/query/action | Search using new engine. |
> | Eylem | Microsoft.OperationalInsights/workspaces/api/query/schema/read | Get search schema V2. |
> | Eylem | Microsoft.OperationalInsights/workspaces/configurationScopes/delete | Delete Configuration Scope |
> | Eylem | Microsoft.OperationalInsights/workspaces/configurationScopes/read | Get Configuration Scope |
> | Eylem | Microsoft.OperationalInsights/workspaces/configurationScopes/write | Set Configuration Scope |
> | Eylem | Microsoft.OperationalInsights/workspaces/datasources/delete | Delete datasources under a workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/datasources/read | Get datasources under a workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/datasources/write | Create/Update datasources under a workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/delete | Deletes a workspace. If the workspace was linked to an existing workspace at creation time then the workspace it was linked to is not deleted. |
> | Eylem | Microsoft.OperationalInsights/workspaces/gateways/delete | Removes a gateway configured for the workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/generateregistrationcertificate/action | Generates Registration Certificate for the workspace. This Certificate is used to connect Microsoft System Center Operation Manager to the workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/intelligencepacks/disable/action | Disables an intelligence pack for a given workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/intelligencepacks/enable/action | Enables an intelligence pack for a given workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/intelligencepacks/read | Lists all intelligence packs that are visible for a given workspace and also lists whether the pack is enabled or disabled for that workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/linkedServices/delete | Delete linked services under given workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/linkedServices/read | Get linked services under given workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/linkedServices/write | Create/Update linked services under given workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/listKeys/action | Retrieves the list keys for the workspace. These keys are used to connect Microsoft Operational Insights agents to the workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/listKeys/read | Retrieves the list keys for the workspace. These keys are used to connect Microsoft Operational Insights agents to the workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/managementGroups/read | Gets the names and metadata for System Center Operations Manager management groups connected to this workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/metricDefinitions/read | Get Metric Definitions under workspace |
> | Eylem | Microsoft.OperationalInsights/workspaces/notificationSettings/delete | Delete the user's notification settings for the workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/notificationSettings/read | Get the user's notification settings for the workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/notificationSettings/write | Set the user's notification settings for the workspace. |
> | Eylem | microsoft.operationalinsights/workspaces/operations/read | Gets the status of an OperationalInsights workspace operation. |
> | Eylem | Microsoft.OperationalInsights/workspaces/purge/action | Delete specified data from workspace |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesAccountLogon/read | Read data from the AADDomainServicesAccountLogon table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesAccountManagement/read | Read data from the AADDomainServicesAccountManagement table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesDirectoryServiceAccess/read | Read data from the AADDomainServicesDirectoryServiceAccess table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesLogonLogoff/read | Read data from the AADDomainServicesLogonLogoff table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesPolicyChange/read | Read data from the AADDomainServicesPolicyChange table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesPrivilegeUse/read | Read data from the AADDomainServicesPrivilegeUse table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AADDomainServicesSystemSecurity/read | Read data from the AADDomainServicesSystemSecurity table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ADAssessmentRecommendation/read | Read data from the ADAssessmentRecommendation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AddonAzureBackupAlerts/read | Read data from the AddonAzureBackupAlerts table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AddonAzureBackupJobs/read | Read data from the AddonAzureBackupJobs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AddonAzureBackupPolicy/read | Read data from the AddonAzureBackupPolicy table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AddonAzureBackupProtectedInstance/read | Read data from the AddonAzureBackupProtectedInstance table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AddonAzureBackupStorage/read | Read data from the AddonAzureBackupStorage table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ADFActivityRun/read | Read data from the ADFActivityRun table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ADFPipelineRun/read | Read data from the ADFPipelineRun table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ADFTriggerRun/read | Read data from the ADFTriggerRun table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ADReplicationResult/read | Read data from the ADReplicationResult table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ADSecurityAssessmentRecommendation/read | Read data from the ADSecurityAssessmentRecommendation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AegDeliveryFailureLogs/read | Read data from the AegDeliveryFailureLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AegPublishFailureLogs/read | Read data from the AegPublishFailureLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Alert/read | Read data from the Alert table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AlertHistory/read | Read data from the AlertHistory table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AmlComputeClusterEvent/read | Read data from the AmlComputeClusterEvent table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AmlComputeClusterNodeEvent/read | Read data from the AmlComputeClusterNodeEvent table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AmlComputeJobEvent/read | Read data from the AmlComputeJobEvent table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ApiManagementGatewayLogs/read | Read data from the ApiManagementGatewayLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AppCenterError/read | Read data from the AppCenterError table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ApplicationInsights/read | Read data from the ApplicationInsights table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AppPlatformLogsforSpring/read | Read data from the AppPlatformLogsforSpring table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AppPlatformSystemLogs/read | Read data from the AppPlatformSystemLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AppServiceAppLogs/read | Read data from the AppServiceAppLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AppServiceAuditLogs/read | Read data from the AppServiceAuditLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AppServiceConsoleLogs/read | Read data from the AppServiceConsoleLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AppServiceEnvironmentPlatformLogs/read | Read data from the AppServiceEnvironmentPlatformLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AppServiceHTTPLogs/read | Read data from the AppServiceHTTPLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AuditLogs/read | Read data from the AuditLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AutoscaleEvaluationsLog/read | Read data from the AutoscaleEvaluationsLog table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AutoscaleScaleActionsLog/read | Read data from the AutoscaleScaleActionsLog table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AWSCloudTrail/read | Read data from the AWSCloudTrail table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AzureActivity/read | Read data from the AzureActivity table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AzureAssessmentRecommendation/read | Read data from the AzureAssessmentRecommendation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/AzureMetrics/read | Read data from the AzureMetrics table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/BaiClusterEvent/read | Read data from the BaiClusterEvent table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/BaiClusterNodeEvent/read | Read data from the BaiClusterNodeEvent table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/BaiJobEvent/read | Read data from the BaiJobEvent table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/BlockchainApplicationLog/read | Read data from the BlockchainApplicationLog table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/BlockchainProxyLog/read | Read data from the BlockchainProxyLog table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/BoundPort/read | Read data from the BoundPort table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/CommonSecurityLog/read | Read data from the CommonSecurityLog table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ComputerGroup/read | Read data from the ComputerGroup table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ConfigurationChange/read | Read data from the ConfigurationChange table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ConfigurationData/read | Read data from the ConfigurationData table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ContainerImageInventory/read | Read data from the ContainerImageInventory table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ContainerInventory/read | Read data from the ContainerInventory table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ContainerLog/read | Read data from the ContainerLog table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ContainerNodeInventory/read | Read data from the ContainerNodeInventory table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ContainerRegistryLoginEvents/read | Read data from the ContainerRegistryLoginEvents table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ContainerRegistryRepositoryEvents/read | Read data from the ContainerRegistryRepositoryEvents table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ContainerServiceLog/read | Read data from the ContainerServiceLog table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/CoreAzureBackup/read | Read data from the CoreAzureBackup table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksAccounts/read | Read data from the DatabricksAccounts table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksClusters/read | Read data from the DatabricksClusters table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksDBFS/read | Read data from the DatabricksDBFS table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksInstancePools/read | Read data from the DatabricksInstancePools table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksJobs/read | Read data from the DatabricksJobs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksNotebook/read | Read data from the DatabricksNotebook table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksSecrets/read | Read data from the DatabricksSecrets table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksSQLPermissions/read | Read data from the DatabricksSQLPermissions table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksSSH/read | Read data from the DatabricksSSH table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksTables/read | Read data from the DatabricksTables table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DatabricksWorkspace/read | Read data from the DatabricksWorkspace table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceAppCrash/read | Read data from the DeviceAppCrash table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceAppLaunch/read | Read data from the DeviceAppLaunch table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceCalendar/read | Read data from the DeviceCalendar table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceCleanup/read | Read data from the DeviceCleanup table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceConnectSession/read | Read data from the DeviceConnectSession table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceEtw/read | Read data from the DeviceEtw table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceHardwareHealth/read | Read data from the DeviceHardwareHealth table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceHealth/read | Read data from the DeviceHealth table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceHeartbeat/read | Read data from the DeviceHeartbeat table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceSkypeHeartbeat/read | Read data from the DeviceSkypeHeartbeat table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceSkypeSignIn/read | Read data from the DeviceSkypeSignIn table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DeviceSleepState/read | Read data from the DeviceSleepState table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHAppFailure/read | Read data from the DHAppFailure table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHAppReliability/read | Read data from the DHAppReliability table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHCPActivity/read | Read data from the DHCPActivity table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHDriverReliability/read | Read data from the DHDriverReliability table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHLogonFailures/read | Read data from the DHLogonFailures table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHLogonMetrics/read | Read data from the DHLogonMetrics table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHOSCrashData/read | Read data from the DHOSCrashData table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHOSReliability/read | Read data from the DHOSReliability table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DHWipAppLearning/read | Read data from the DHWipAppLearning table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DnsEvents/read | Read data from the DnsEvents table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/DnsInventory/read | Read data from the DnsInventory table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ETWEvent/read | Read data from the ETWEvent table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Event/read | Read data from the Event table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ExchangeAssessmentRecommendation/read | Read data from the ExchangeAssessmentRecommendation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ExchangeOnlineAssessmentRecommendation/read | Read data from the ExchangeOnlineAssessmentRecommendation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/FailedIngestion/read | Read data from the FailedIngestion table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/FunctionAppLogs/read | Read data from the FunctionAppLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Heartbeat/read | Read data from the Heartbeat table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/HuntingBookmark/read | Read data from the HuntingBookmark table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/IISAssessmentRecommendation/read | Read data from the IISAssessmentRecommendation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/InboundConnection/read | Read data from the InboundConnection table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/InsightsMetrics/read | Read data from the InsightsMetrics table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/IntuneAuditLogs/read | Read data from the IntuneAuditLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/IntuneDeviceComplianceOrg/read | Read data from the IntuneDeviceComplianceOrg table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/IntuneOperationalLogs/read | Read data from the IntuneOperationalLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/KubeEvents/read | Read data from the KubeEvents table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/KubeHealth/read | Read data from the KubeHealth table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/KubeMonAgentEvents/read | Read data from the KubeMonAgentEvents table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/KubeNodeInventory/read | Read data from the KubeNodeInventory table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/KubePodInventory/read | Read data from the KubePodInventory table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/KubeServices/read | Read data from the KubeServices table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/LinuxAuditLog/read | Read data from the LinuxAuditLog table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAApplication/read | Read data from the MAApplication table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealth/read | Read data from the MAApplicationHealth table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealthAlternativeVersions/read | Read data from the MAApplicationHealthAlternativeVersions table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealthIssues/read | Read data from the MAApplicationHealthIssues table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAApplicationInstance/read | Read data from the MAApplicationInstance table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAApplicationInstanceReadiness/read | Read data from the MAApplicationInstanceReadiness table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAApplicationReadiness/read | Read data from the MAApplicationReadiness table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADeploymentPlan/read | Read data from the MADeploymentPlan table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADevice/read | Read data from the MADevice table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADeviceNotEnrolled/read | Read data from the MADeviceNotEnrolled table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADeviceNRT/read | Read data from the MADeviceNRT table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealth/read | Read data from the MADevicePnPHealth table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealthAlternativeVersions/read | Read data from the MADevicePnPHealthAlternativeVersions table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealthIssues/read | Read data from the MADevicePnPHealthIssues table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADeviceReadiness/read | Read data from the MADeviceReadiness table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADriverInstanceReadiness/read | Read data from the MADriverInstanceReadiness table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MADriverReadiness/read | Read data from the MADriverReadiness table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddin/read | Read data from the MAOfficeAddin table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinEntityHealth/read | Read data from the MAOfficeAddinEntityHealth table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealth/read | Read data from the MAOfficeAddinHealth table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealthEventNRT/read | Read data from the MAOfficeAddinHealthEventNRT table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealthIssues/read | Read data from the MAOfficeAddinHealthIssues table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinInstance/read | Read data from the MAOfficeAddinInstance table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinInstanceReadiness/read | Read data from the MAOfficeAddinInstanceReadiness table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinReadiness/read | Read data from the MAOfficeAddinReadiness table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeApp/read | Read data from the MAOfficeApp table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppCrashesNRT/read | Read data from the MAOfficeAppCrashesNRT table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppHealth/read | Read data from the MAOfficeAppHealth table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppInstance/read | Read data from the MAOfficeAppInstance table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppInstanceHealth/read | Read data from the MAOfficeAppInstanceHealth table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppReadiness/read | Read data from the MAOfficeAppReadiness table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppSessionsNRT/read | Read data from the MAOfficeAppSessionsNRT table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeBuildInfo/read | Read data from the MAOfficeBuildInfo table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeCurrencyAssessment/read | Read data from the MAOfficeCurrencyAssessment table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeCurrencyAssessmentDailyCounts/read | Read data from the MAOfficeCurrencyAssessmentDailyCounts table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeDeploymentStatus/read | Read data from the MAOfficeDeploymentStatus table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeDeploymentStatusNRT/read | Read data from the MAOfficeDeploymentStatusNRT table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroErrorNRT/read | Read data from the MAOfficeMacroErrorNRT table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroGlobalHealth/read | Read data from the MAOfficeMacroGlobalHealth table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroHealth/read | Read data from the MAOfficeMacroHealth table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroHealthIssues/read | Read data from the MAOfficeMacroHealthIssues table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroIssueInstanceReadiness/read | Read data from the MAOfficeMacroIssueInstanceReadiness table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroIssueReadiness/read | Read data from the MAOfficeMacroIssueReadiness table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroSummary/read | Read data from the MAOfficeMacroSummary table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeSuite/read | Read data from the MAOfficeSuite table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAOfficeSuiteInstance/read | Read data from the MAOfficeSuiteInstance table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAProposedPilotDevices/read | Read data from the MAProposedPilotDevices table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAWindowsBuildInfo/read | Read data from the MAWindowsBuildInfo table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAWindowsCurrencyAssessment/read | Read data from the MAWindowsCurrencyAssessment table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAWindowsCurrencyAssessmentDailyCounts/read | Read data from the MAWindowsCurrencyAssessmentDailyCounts table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAWindowsDeploymentStatus/read | Read data from the MAWindowsDeploymentStatus table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAWindowsDeploymentStatusNRT/read | Read data from the MAWindowsDeploymentStatusNRT table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MAWindowsSysReqInstanceReadiness/read | Read data from the MAWindowsSysReqInstanceReadiness table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/McasShadowItReporting/read | Read data from the McasShadowItReporting table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MicrosoftAzureBastionAuditLogs/read | Read data from the MicrosoftAzureBastionAuditLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MicrosoftDataShareReceivedSnapshotLog/read | Read data from the MicrosoftDataShareReceivedSnapshotLog table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MicrosoftDataShareSentSnapshotLog/read | Read data from the MicrosoftDataShareSentSnapshotLog table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MicrosoftDataShareShareLog/read | Read data from the MicrosoftDataShareShareLog table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MicrosoftDynamicsTelemetryPerformanceLogs/read | Read data from the MicrosoftDynamicsTelemetryPerformanceLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MicrosoftDynamicsTelemetrySystemMetricsLogs/read | Read data from the MicrosoftDynamicsTelemetrySystemMetricsLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MicrosoftHealthcareApisAuditLogs/read | Read data from the MicrosoftHealthcareApisAuditLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/MicrosoftInsightsAzureActivityLog/read | Read data from the MicrosoftInsightsAzureActivityLog table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/NetworkMonitoring/read | Read data from the NetworkMonitoring table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/OfficeActivity/read | Read data from the OfficeActivity table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Operation/read | Read data from the Operation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/OutboundConnection/read | Read data from the OutboundConnection table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Perf/read | Read data from the Perf table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ProtectionStatus/read | Read data from the ProtectionStatus table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/read | Run queries over the data in the workspace |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ReservedCommonFields/read | Read data from the ReservedCommonFields table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SCCMAssessmentRecommendation/read | Read data from the SCCMAssessmentRecommendation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SCOMAssessmentRecommendation/read | Read data from the SCOMAssessmentRecommendation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SecurityAlert/read | Read data from the SecurityAlert table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SecurityBaseline/read | Read data from the SecurityBaseline table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SecurityBaselineSummary/read | Read data from the SecurityBaselineSummary table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SecurityDetection/read | Read data from the SecurityDetection table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SecurityEvent/read | Read data from the SecurityEvent table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SecurityIoTRawEvent/read | Read data from the SecurityIoTRawEvent table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SecurityRecommendation/read | Read data from the SecurityRecommendation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ServiceFabricOperationalEvent/read | Read data from the ServiceFabricOperationalEvent table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ServiceFabricReliableActorEvent/read | Read data from the ServiceFabricReliableActorEvent table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ServiceFabricReliableServiceEvent/read | Read data from the ServiceFabricReliableServiceEvent table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SfBAssessmentRecommendation/read | Read data from the SfBAssessmentRecommendation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SfBOnlineAssessmentRecommendation/read | Read data from the SfBOnlineAssessmentRecommendation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SharePointOnlineAssessmentRecommendation/read | Read data from the SharePointOnlineAssessmentRecommendation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SignalRServiceDiagnosticLogs/read | Read data from the SignalRServiceDiagnosticLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SigninLogs/read | Read data from the SigninLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SPAssessmentRecommendation/read | Read data from the SPAssessmentRecommendation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SQLAssessmentRecommendation/read | Read data from the SQLAssessmentRecommendation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SQLQueryPerformance/read | Read data from the SQLQueryPerformance table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SqlThreatProtectionLoginAudits/read | Read data from the SqlThreatProtectionLoginAudits table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SqlVulnerabilityAssessmentResult/read | Read data from the SqlVulnerabilityAssessmentResult table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/StorageBlobLogs/read | Read data from the StorageBlobLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/StorageFileLogs/read | Read data from the StorageFileLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/StorageQueueLogs/read | Read data from the StorageQueueLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/StorageTableLogs/read | Read data from the StorageTableLogs table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SucceededIngestion/read | Read data from the SucceededIngestion table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Syslog/read | Read data from the Syslog table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/SysmonEvent/read | Read data from the SysmonEvent table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Tables.Custom/read | Reading data from any custom log |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/ThreatIntelligenceIndicator/read | Read data from the ThreatIntelligenceIndicator table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAApp/read | Read data from the UAApp table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAComputer/read | Read data from the UAComputer table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAComputerRank/read | Read data from the UAComputerRank table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UADriver/read | Read data from the UADriver table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UADriverProblemCodes/read | Read data from the UADriverProblemCodes table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAFeedback/read | Read data from the UAFeedback table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAHardwareSecurity/read | Read data from the UAHardwareSecurity table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAIESiteDiscovery/read | Read data from the UAIESiteDiscovery table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAOfficeAddIn/read | Read data from the UAOfficeAddIn table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAProposedActionPlan/read | Read data from the UAProposedActionPlan table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UASysReqIssue/read | Read data from the UASysReqIssue table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UAUpgradedComputer/read | Read data from the UAUpgradedComputer table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Update/read | Read data from the Update table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UpdateRunProgress/read | Read data from the UpdateRunProgress table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/UpdateSummary/read | Read data from the UpdateSummary table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/Usage/read | Read data from the Usage table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/VMBoundPort/read | Read data from the VMBoundPort table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/VMComputer/read | Read data from the VMComputer table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/VMConnection/read | Read data from the VMConnection table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/VMProcess/read | Read data from the VMProcess table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/W3CIISLog/read | Read data from the W3CIISLog table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WaaSDeploymentStatus/read | Read data from the WaaSDeploymentStatus table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WaaSInsiderStatus/read | Read data from the WaaSInsiderStatus table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WaaSUpdateStatus/read | Read data from the WaaSUpdateStatus table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WDAVStatus/read | Read data from the WDAVStatus table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WDAVThreat/read | Read data from the WDAVThreat table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WindowsClientAssessmentRecommendation/read | Read data from the WindowsClientAssessmentRecommendation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WindowsEvent/read | Read data from the WindowsEvent table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WindowsFirewall/read | Read data from the WindowsFirewall table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WindowsServerAssessmentRecommendation/read | Read data from the WindowsServerAssessmentRecommendation table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WireData/read | Read data from the WireData table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WorkloadMonitoringPerf/read | Read data from the WorkloadMonitoringPerf table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WUDOAggregatedStatus/read | Read data from the WUDOAggregatedStatus table |
> | Eylem | Microsoft.OperationalInsights/workspaces/query/WUDOStatus/read | Read data from the WUDOStatus table |
> | Eylem | Microsoft.OperationalInsights/workspaces/read | Gets an existing workspace |
> | Eylem | Microsoft.OperationalInsights/workspaces/regeneratesharedkey/action | Regenerates the specified workspace shared key |
> | Eylem | microsoft.operationalinsights/workspaces/rules/read | Get all alert rules. |
> | Eylem | Microsoft.OperationalInsights/workspaces/savedSearches/delete | Deletes a saved search query |
> | Eylem | Microsoft.OperationalInsights/workspaces/savedSearches/read | Gets a saved search query |
> | Eylem | microsoft.operationalinsights/workspaces/savedsearches/results/read | Get saved searches results. Kullanım Dışı |
> | Eylem | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/delete | Delete scheduled search actions. |
> | Eylem | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/read | Get scheduled search actions. |
> | Eylem | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/write | Create or update scheduled search actions. |
> | Eylem | microsoft.operationalinsights/workspaces/savedsearches/schedules/delete | Delete scheduled searches. |
> | Eylem | microsoft.operationalinsights/workspaces/savedsearches/schedules/read | Get scheduled searches. |
> | Eylem | microsoft.operationalinsights/workspaces/savedsearches/schedules/write | Create or update scheduled searches. |
> | Eylem | Microsoft.OperationalInsights/workspaces/savedSearches/write | Creates a saved search query |
> | Eylem | Microsoft.OperationalInsights/workspaces/schema/read | Gets the search schema for the workspace.  Search schema includes the exposed fields and their types. |
> | Eylem | Microsoft.OperationalInsights/workspaces/search/action | Executes a search query |
> | Eylem | microsoft.operationalinsights/workspaces/search/read | Get search results. Kullanım dışı. |
> | Eylem | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Retrieves the shared keys for the workspace. These keys are used to connect Microsoft Operational Insights agents to the workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Retrieves the shared keys for the workspace. These keys are used to connect Microsoft Operational Insights agents to the workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/delete | Deletes a storage configuration. This will stop Microsoft Operational Insights from reading data from the storage account. |
> | Eylem | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/read | Gets a storage configuration. |
> | Eylem | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/write | Creates a new storage configuration. These configurations are used to pull data from a location in an existing storage account. |
> | Eylem | Microsoft.OperationalInsights/workspaces/upgradetranslationfailures/read | Get Search Upgrade Translation Failure log for the workspace |
> | Eylem | Microsoft.OperationalInsights/workspaces/usages/read | Gets usage data for a workspace including the amount of data read by the workspace. |
> | Eylem | Microsoft.OperationalInsights/workspaces/write | Creates a new workspace or links to an existing workspace by providing the customer id from the existing workspace. |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.OperationsManagement/managementAssociations/delete | Delete existing Management Association |
> | Eylem | Microsoft.OperationsManagement/managementAssociations/read | Get Existing Management Association |
> | Eylem | Microsoft.OperationsManagement/managementAssociations/write | Create a new Management Association |
> | Eylem | Microsoft.OperationsManagement/managementConfigurations/delete | Delete existing Management Configuration |
> | Eylem | Microsoft.OperationsManagement/managementConfigurations/read | Get Existing Management Configuration |
> | Eylem | Microsoft.OperationsManagement/managementConfigurations/write | Create a new Management Configuration |
> | Eylem | Microsoft.OperationsManagement/register/action | Register a subscription to a resource provider. |
> | Eylem | Microsoft.OperationsManagement/solutions/delete | Delete existing OMS solution |
> | Eylem | Microsoft.OperationsManagement/solutions/read | Get exiting OMS solution |
> | Eylem | Microsoft.OperationsManagement/solutions/write | Create new OMS solution |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.PolicyInsights/asyncOperationResults/read | Gets the async operation result. |
> | DataAction | Microsoft.PolicyInsights/checkDataPolicyCompliance/action | Check the compliance status of a given component against data policies. |
> | Eylem | Microsoft.PolicyInsights/operations/read | Gets supported operations on Microsoft.PolicyInsights namespace |
> | DataAction | Microsoft.PolicyInsights/policyEvents/logDataEvents/action | Log the resource component policy events. |
> | Eylem | Microsoft.PolicyInsights/policyEvents/queryResults/action | Query information about policy events. |
> | Eylem | Microsoft.PolicyInsights/policyEvents/queryResults/read | Query information about policy events. |
> | Eylem | Microsoft.PolicyInsights/policyMetadata/read | Get Policy Metadata resources. |
> | Eylem | Microsoft.PolicyInsights/policyStates/queryResults/action | Query information about policy states. |
> | Eylem | Microsoft.PolicyInsights/policyStates/queryResults/read | Query information about policy states. |
> | Eylem | Microsoft.PolicyInsights/policyStates/summarize/action | Query summary information about policy latest states. |
> | Eylem | Microsoft.PolicyInsights/policyStates/summarize/read | Query summary information about policy latest states. |
> | Eylem | Microsoft.PolicyInsights/policyStates/triggerEvaluation/action | Triggers a new compliance evaluation for the selected scope. |
> | Eylem | Microsoft.PolicyInsights/policyTrackedResources/queryResults/read | Query information about resources required by DeployIfNotExists policies. |
> | Eylem | Microsoft.PolicyInsights/register/action | Registers the Microsoft Policy Insights resource provider and enables actions on it. |
> | Eylem | Microsoft.PolicyInsights/remediations/cancel/action | Cancel in-progress Microsoft Policy remediations. |
> | Eylem | Microsoft.PolicyInsights/remediations/delete | Delete policy remediations. |
> | Eylem | Microsoft.PolicyInsights/remediations/listDeployments/read | Lists the deployments required by a policy remediation. |
> | Eylem | Microsoft.PolicyInsights/remediations/read | Get policy remediations. |
> | Eylem | Microsoft.PolicyInsights/remediations/write | Create or update Microsoft Policy remediations. |
> | Eylem | Microsoft.PolicyInsights/unregister/action | Unregisters the Microsoft Policy Insights resource provider. |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Portal/consoles/delete | Removes the Cloud Shell instance. |
> | Eylem | Microsoft.Portal/consoles/read | Reads the Cloud Shell instance. |
> | Eylem | Microsoft.Portal/consoles/write | Create or update a Cloud Shell instance. |
> | Eylem | Microsoft.Portal/dashboards/delete | Removes the dashboard from the subscription. |
> | Eylem | Microsoft.Portal/dashboards/read | Reads the dashboards for the subscription. |
> | Eylem | Microsoft.Portal/dashboards/write | Add or modify dashboard to a subscription. |
> | Eylem | Microsoft.Portal/register/action | Register to Portal |
> | Eylem | Microsoft.Portal/usersettings/delete | Removes the Cloud Shell user settings. |
> | Eylem | Microsoft.Portal/usersettings/read | Reads the Cloud Shell user settings. |
> | Eylem | Microsoft.Portal/usersettings/write | Create or update Cloud Shell user setting. |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.PowerBIDedicated/capacities/delete | Deletes the Power BI Dedicated Capacity. |
> | Eylem | Microsoft.PowerBIDedicated/capacities/read | Retrieves the information of the specified Power BI Dedicated Capacity. |
> | Eylem | Microsoft.PowerBIDedicated/capacities/resume/action | Resumes the Capacity. |
> | Eylem | Microsoft.PowerBIDedicated/capacities/skus/read | Retrieve available SKU information for the capacity |
> | Eylem | Microsoft.PowerBIDedicated/capacities/suspend/action | Suspends the Capacity. |
> | Eylem | Microsoft.PowerBIDedicated/capacities/write | Creates or updates the specified Power BI Dedicated Capacity. |
> | Eylem | Microsoft.PowerBIDedicated/locations/checkNameAvailability/action | Checks that given Power BI Dedicated Capacity name is valid and not in use. |
> | Eylem | Microsoft.PowerBIDedicated/locations/operationresults/read | Retrieves the information of the specified operation result. |
> | Eylem | Microsoft.PowerBIDedicated/locations/operationstatuses/read | Retrieves the information of the specified operation status. |
> | Eylem | Microsoft.PowerBIDedicated/operations/read | Retrieves the information of operations |
> | Eylem | Microsoft.PowerBIDedicated/register/action | Registers Power BI Dedicated resource provider. |
> | Eylem | Microsoft.PowerBIDedicated/skus/read | Retrieves the information of Skus |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is internal operation used by service |
> | Eylem | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp is internal operation used by service |
> | Eylem | Microsoft.RecoveryServices/Locations/backupPreValidateProtection/action |  |
> | Eylem | Microsoft.RecoveryServices/Locations/backupProtectedItem/write | Create a backup Protected Item |
> | Eylem | Microsoft.RecoveryServices/Locations/backupProtectedItems/read | Returns the list of all Protected Items. |
> | Eylem | Microsoft.RecoveryServices/Locations/backupStatus/action | Check Backup Status for Recovery Services Vaults |
> | Eylem | Microsoft.RecoveryServices/Locations/backupValidateFeatures/action | Validate Features |
> | Eylem | Microsoft.RecoveryServices/locations/checkNameAvailability/action | Check Resource Name Availability is an API to check if resource name is available |
> | Eylem | Microsoft.RecoveryServices/locations/operationStatus/read | Gets Operation Status for a given Operation |
> | Eylem | Microsoft.RecoveryServices/operations/read | Operation returns the list of Operations for a Resource Provider |
> | Eylem | Microsoft.RecoveryServices/register/action | Registers subscription for given Resource Provider |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupconfig/read | Returns Configuration for Recovery Services Vault. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupconfig/write | Updates Configuration for Recovery Services Vault. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupEncryptionConfigs/read | Gets Backup Resource Encryption Configuration. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupEncryptionConfigs/write | Updates Backup Resource Encryption Configuration |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupEngines/read | Returns all the backup management servers registered with vault. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/delete | Delete a backup Protection Intent |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read | Get a backup Protection Intent |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Create a backup Protection Intent |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Returns status of the operation |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/operationsStatus/read | Returns status of the operation |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Get all protectable containers |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/delete | Deletes the registered Container |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action | Do inquiry for workloads within a container |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Get all items in a container |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Gets result of Operation performed on Protection Container. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationsStatus/read | Gets status of Operation performed on Protection Container. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Performs Backup for Protected Item. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/delete | Deletes Protected Item |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Gets Result of Operation Performed on Protected Items. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Returns the status of Operation performed on Protected Items. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Returns object details of the Protected Item |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Provision Instant Item Recovery for Protected Item |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Get Recovery Points for Protected Items. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Restore Recovery Points for Protected Items. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Revoke Instant Item Recovery for Protected Item |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Create a backup Protected Item |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Returns all registered containers |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write | Creates a registered container |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Refreshes the container list |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | Cancel the Job |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Returns the Result of Job Operation. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupJobs/operationsStatus/read | Returns the status of Job Operation. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupJobs/read | Returns all Job Objects |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Export Jobs |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Returns Backup Operation Result for Recovery Services Vault. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupOperations/read | Returns Backup Operation Status for Recovery Services Vault. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupPolicies/delete | Delete a Protection Policy |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Get Results of Policy Operation. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | Get Status of Policy Operation. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Returns all Protection Policies |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Creates Protection Policy |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Returns list of all Protectable Items. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Returns the list of all Protected Items. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Returns all containers belonging to the subscription |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | List all backup Protection Intents |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/action | Returns Security PIN Information for Recovery Services Vault. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupstorageconfig/read | Returns Storage Configuration for Recovery Services Vault. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupstorageconfig/write | Updates Storage Configuration for Recovery Services Vault. |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Returns summaries for Protected Items and Protected Servers for a Recovery Services . |
> | Eylem | Microsoft.RecoveryServices/Vaults/backupValidateOperation/action | Validate Operation on Protected Item |
> | Eylem | Microsoft.RecoveryServices/Vaults/certificates/write | The Update Resource Certificate operation updates the resource/vault credential certificate. |
> | Eylem | Microsoft.RecoveryServices/Vaults/delete | The Delete Vault operation deletes the specified Azure resource of type 'vault' |
> | Eylem | Microsoft.RecoveryServices/Vaults/extendedInformation/delete | The Get Extended Info operation gets an object's Extended Info representing the Azure resource of type ?vault? |
> | Eylem | Microsoft.RecoveryServices/Vaults/extendedInformation/read | The Get Extended Info operation gets an object's Extended Info representing the Azure resource of type ?vault? |
> | Eylem | Microsoft.RecoveryServices/Vaults/extendedInformation/write | The Get Extended Info operation gets an object's Extended Info representing the Azure resource of type ?vault? |
> | Eylem | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Gets the alerts for the Recovery services vault. |
> | Eylem | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Resolves the alert. |
> | Eylem | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/read | Gets the Recovery services vault notification configuration. |
> | Eylem | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/write | Configures e-mail notifications to Recovery services vault. |
> | Eylem | Microsoft.RecoveryServices/Vaults/read | The Get Vault operation gets an object representing the Azure resource of type 'vault' |
> | Eylem | Microsoft.RecoveryServices/Vaults/registeredIdentities/delete | The UnRegister Container operation can be used to unregister a container. |
> | Eylem | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | The Get Operation Results operation can be used get the operation status and result for the asynchronously submitted operation |
> | Eylem | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | The Get Containers operation can be used get the containers registered for a resource. |
> | Eylem | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | The Register Service Container operation can be used to register a container with Recovery Service. |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Read any Alerts Settings |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationAlertSettings/write | Create or Update any Alerts Settings |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationEvents/read | Read any Events |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Checks Consistency of the Fabric |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/delete | Delete any Fabrics |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/deployProcessServerImage/action | Deploy Process Server Image |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/migratetoaad/action | Migrate Fabric To AAD |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/operationresults/read | Track the results of an asynchronous operation on the resource Fabrics |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Read any Fabrics |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Reassociate Gateway |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/remove/action | Remove Fabric |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Renew Certificate for Fabric |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationLogicalNetworks/read | Read any Logical Networks |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Read any Networks |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/delete | Delete any Network Mappings |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Read any Network Mappings |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/write | Create or Update any Network Mappings |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/discoverProtectableItem/action | Discover Protectable Item |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/operationresults/read | Track the results of an asynchronous operation on the resource Protection Containers |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Read any Protection Containers |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/remove/action | Remove Protection Container |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/delete | Delete any Migration Items |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/migrate/action | Migrate Item |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/migrationRecoveryPoints/read | Read any Migration Recovery Points |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/operationresults/read | Track the results of an asynchronous operation on the resource Migration Items |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/read | Read any Migration Items |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/testMigrate/action | Test Migrate |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/testMigrateCleanup/action | Test Migrate Cleanup |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/write | Create or Update any Migration Items |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Read any Protectable Items |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/addDisks/action | Disk ekleme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Apply Recovery Point |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/delete | Delete any Protected Items |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Failover Commit |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/operationresults/read | Track the results of an asynchronous operation on the resource Protected Items |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Planlı Yük Devretme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Read any Protected Items |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Read any Replication Recovery Points |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/remove/action | Remove Protected Item |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/removeDisks/action | Remove disks |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Repair replication |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | ReProtect Protected Item |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/ResolveHealthErrors/action |  |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/submitFeedback/action | Submit Feedback |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/targetComputeSizes/read | Read any Target Compute Sizes |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Test Yük Devretmesi |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Test Failover Cleanup |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Yük Devretme |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Update Mobility Service |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/write | Create or Update any Protected Items |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/delete | Delete any Protection Container Mappings |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/operationresults/read | Track the results of an asynchronous operation on the resource Protection Container Mappings |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Read any Protection Container Mappings |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/remove/action | Remove Protection Container Mapping |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/write | Create or Update any Protection Container Mappings |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | Switch Protection Container |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/write | Create or Update any Protection Containers |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/delete | Delete any Recovery Services Providers |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/operationresults/read | Track the results of an asynchronous operation on the resource Recovery Services Providers |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Read any Recovery Services Providers |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Refresh Provider |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/remove/action | Remove Recovery Services Provider |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/write | Create or Update any Recovery Services Providers |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Read any Storage Classifications |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/delete | Delete any Storage Classification Mappings |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/operationresults/read | Track the results of an asynchronous operation on the resource Storage Classification Mappings |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Read any Storage Classification Mappings |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/write | Create or Update any Storage Classification Mappings |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/delete | Delete any vCenters |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/operationresults/read | Track the results of an asynchronous operation on the resource vCenters |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Read any vCenters |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/write | Create or Update any vCenters |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationFabrics/write | Create or Update any Fabrics |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationJobs/cancel/action | Cancel Job |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationJobs/operationresults/read | Track the results of an asynchronous operation on the resource Jobs |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationJobs/read | Read any Jobs |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationJobs/restart/action | Restart job |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationJobs/resume/action | Resume Job |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationMigrationItems/read | Read any Migration Items |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationNetworkMappings/read | Read any Network Mappings |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationNetworks/read | Read any Networks |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationOperationStatus/read | Read any Vault Replication Operation Status |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationPolicies/delete | Delete any Policies |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationPolicies/operationresults/read | Track the results of an asynchronous operation on the resource Policies |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Read any Policies |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationPolicies/write | Create or Update any Policies |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationProtectedItems/read | Read any Protected Items |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationProtectionContainerMappings/read | Read any Protection Container Mappings |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationProtectionContainers/read | Read any Protection Containers |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/delete | Delete any Recovery Plans |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Failover Commit Recovery Plan |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/operationresults/read | Track the results of an asynchronous operation on the resource Recovery Plans |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Planned Failover Recovery Plan |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Read any Recovery Plans |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | ReProtect Recovery Plan |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Test Failover Recovery Plan |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Test Failover Cleanup Recovery Plan |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Failover Recovery Plan |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/write | Create or Update any Recovery Plans |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationRecoveryServicesProviders/read | Read any Recovery Services Providers |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationStorageClassificationMappings/read | Read any Storage Classification Mappings |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationStorageClassifications/read | Read any Storage Classifications |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationSupportedOperatingSystems/read | Read any  |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationUsages/read | Read any Vault Replication Usages |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationVaultHealth/operationresults/read | Track the results of an asynchronous operation on the resource Vault Replication Health |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationVaultHealth/read | Read any Vault Replication Health |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationVaultHealth/refresh/action | Refresh Vault Health |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationVaultSettings/read | Read any  |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationVaultSettings/write | Create or Update any  |
> | Eylem | Microsoft.RecoveryServices/vaults/replicationvCenters/read | Read any vCenters |
> | Eylem | Microsoft.RecoveryServices/vaults/usages/read | Read any Vault Usages |
> | Eylem | Microsoft.RecoveryServices/Vaults/usages/read | Returns usage details for a Recovery Services Vault. |
> | Eylem | Microsoft.RecoveryServices/Vaults/vaultTokens/read | The Vault Token operation can be used to get Vault Token for vault level backend operations. |
> | Eylem | Microsoft.RecoveryServices/Vaults/write | Create Vault operation creates an Azure resource of type 'vault' |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Relay/checkNameAvailability/action | Checks availability of namespace under given subscription. |
> | Eylem | Microsoft.Relay/checkNamespaceAvailability/action | Checks availability of namespace under given subscription. This API is deprecated please use CheckNameAvailability instead. |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/action | Updates Namespace Authorization Rule. This API is deprecated. Please use a PUT call to update the Namespace Authorization Rule instead.. This operation is not supported on API version 2017-04-01. |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/delete | Delete Namespace Authorization Rule. The Default Namespace Authorization Rule cannot be deleted.  |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/listkeys/action | Get the Connection String to the Namespace |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/read | Get the list of Namespaces Authorization Rules description. |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/regenerateKeys/action | Regenerate the Primary or Secondary key to the Resource |
> | Eylem | Microsoft.Relay/namespaces/authorizationRules/write | Create a Namespace level Authorization Rules and update its properties. The Authorization Rules Access Rights, the Primary and Secondary Keys can be updated. |
> | Eylem | Microsoft.Relay/namespaces/Delete | Delete Namespace Resource |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Gets the authorization rules keys for the Disaster Recovery primary namespace |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/read | Get Disaster Recovery Primary Namespace's Authorization Rules |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/breakPairing/action | Disables Disaster Recovery and stops replicating changes from primary to secondary namespaces. |
> | Eylem | Microsoft.Relay/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Checks availability of namespace alias under given subscription. |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/delete | Deletes the Disaster Recovery configuration associated with the namespace. This operation can only be invoked via the primary namespace. |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/failover/action | Invokes a GEO DR failover and reconfigures the namespace alias to point to the secondary namespace. |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/read | Gets the Disaster Recovery configuration associated with the namespace. |
> | Eylem | Microsoft.Relay/namespaces/disasterRecoveryConfigs/write | Creates or Updates the Disaster Recovery configuration associated with the namespace. |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/action | Operation to update HybridConnection. This operation is not supported on API version 2017-04-01. Authorization Rules. Please use a PUT call to update Authorization Rule. |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/delete | Operation to delete HybridConnection Authorization Rules |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/listkeys/action | Get the Connection String to HybridConnection |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/read |  Get the list of HybridConnection Authorization Rules |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/regeneratekeys/action | Regenerate the Primary or Secondary key to the Resource |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/write | Create HybridConnection Authorization Rules and Update its properties. The Authorization Rules Access Rights can be updated. |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/Delete | Operation to delete HybridConnection Resource |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/read | Get list of HybridConnection Resource Descriptions |
> | Eylem | Microsoft.Relay/namespaces/HybridConnections/write | Create or Update HybridConnection properties. |
> | Eylem | Microsoft.Relay/namespaces/messagingPlan/read | Gets the Messaging Plan for a namespace.<br>This API is deprecated.<br>Properties exposed via the MessagingPlan resource are moved to the (parent) Namespace resource in later API versions..<br>This operation is not supported on API version 2017-04-01. |
> | Eylem | Microsoft.Relay/namespaces/messagingPlan/write | Updates the Messaging Plan for a namespace.<br>This API is deprecated.<br>Properties exposed via the MessagingPlan resource are moved to the (parent) Namespace resource in later API versions..<br>This operation is not supported on API version 2017-04-01. |
> | Eylem | Microsoft.Relay/namespaces/operationresults/read | Get the status of Namespace operation |
> | Eylem | Microsoft.Relay/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Get list of Namespace diagnostic settings Resource Descriptions |
> | Eylem | Microsoft.Relay/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Get list of Namespace diagnostic settings Resource Descriptions |
> | Eylem | Microsoft.Relay/namespaces/providers/Microsoft.Insights/logDefinitions/read | Get list of Namespace logs Resource Descriptions |
> | Eylem | Microsoft.Relay/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Get list of Namespace metrics Resource Descriptions |
> | Eylem | Microsoft.Relay/namespaces/read | Get the list of Namespace Resource Description |
> | Eylem | Microsoft.Relay/namespaces/removeAcsNamepsace/action | Remove ACS namespace |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/action | Operation to update WcfRelay. This operation is not supported on API version 2017-04-01. Authorization Rules. Please use a PUT call to update Authorization Rule. |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/delete | Operation to delete WcfRelay Authorization Rules |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/listkeys/action | Get the Connection String to WcfRelay |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/read |  Get the list of WcfRelay Authorization Rules |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/regeneratekeys/action | Regenerate the Primary or Secondary key to the Resource |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/write | Create WcfRelay Authorization Rules and Update its properties. The Authorization Rules Access Rights can be updated. |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/Delete | Operation to delete WcfRelay Resource |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/read | Get list of WcfRelay Resource Descriptions |
> | Eylem | Microsoft.Relay/namespaces/WcfRelays/write | Create or Update WcfRelay properties. |
> | Eylem | Microsoft.Relay/namespaces/write | Create a Namespace Resource and Update its properties. Tags and Capacity of the Namespace are the properties which can be updated. |
> | Eylem | Microsoft.Relay/operations/read | Get Operations |
> | Eylem | Microsoft.Relay/register/action | Registers the subscription for the Relay resource provider and enables the creation of Relay resources |
> | Eylem | Microsoft.Relay/unregister/action | Registers the subscription for the Relay resource provider and enables the creation of Relay resources |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ResourceHealth/AvailabilityStatuses/current/read | Gets the availability status for the specified resource |
> | Eylem | Microsoft.ResourceHealth/AvailabilityStatuses/read | Gets the availability statuses for all resources in the specified scope |
> | Eylem | Microsoft.ResourceHealth/events/read | Get Service Health Events for given subscription |
> | Eylem | Microsoft.Resourcehealth/healthevent/action | Denotes the change in health state for the specified resource |
> | Eylem | Microsoft.Resourcehealth/healthevent/Activated/action | Denotes the change in health state for the specified resource |
> | Eylem | Microsoft.Resourcehealth/healthevent/InProgress/action | Denotes the change in health state for the specified resource |
> | Eylem | Microsoft.Resourcehealth/healthevent/Pending/action | Denotes the change in health state for the specified resource |
> | Eylem | Microsoft.Resourcehealth/healthevent/Resolved/action | Denotes the change in health state for the specified resource |
> | Eylem | Microsoft.Resourcehealth/healthevent/Updated/action | Denotes the change in health state for the specified resource |
> | Eylem | Microsoft.ResourceHealth/impactedResources/read | Get Impacted Resources for given subscription |
> | Eylem | Microsoft.ResourceHealth/metadata/read | Gets Metadata |
> | Eylem | Microsoft.ResourceHealth/Notifications/read | Receives Azure Resource Manager notifications |
> | Eylem | Microsoft.ResourceHealth/Operations/read | Get the operations available for the Microsoft ResourceHealth |
> | Eylem | Microsoft.ResourceHealth/register/action | Registers the subscription for the Microsoft ResourceHealth |
> | Eylem | Microsoft.ResourceHealth/unregister/action | Unregisters the subscription for the Microsoft ResourceHealth |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Resources/calculateTemplateHash/action | Calculate the hash of provided template. |
> | Eylem | Microsoft.Resources/checkPolicyCompliance/read | Check the compliance status of a given resource against resource policies. |
> | Eylem | Microsoft.Resources/checkResourceName/action | Check the resource name for validity. |
> | Eylem | Microsoft.Resources/deployments/cancel/action | Cancels a deployment. |
> | Eylem | Microsoft.Resources/deployments/delete | Deletes a deployment. |
> | Eylem | Microsoft.Resources/deployments/exportTemplate/action | Export template for a deployment |
> | Eylem | Microsoft.Resources/deployments/operations/read | Gets or lists deployment operations. |
> | Eylem | Microsoft.Resources/deployments/operationstatuses/read | Gets or lists deployment operation statuses. |
> | Eylem | Microsoft.Resources/deployments/read | Gets or lists deployments. |
> | Eylem | Microsoft.Resources/deployments/validate/action | Validates an deployment. |
> | Eylem | Microsoft.Resources/deployments/whatIf/action | Predicts template deployment changes. |
> | Eylem | Microsoft.Resources/deployments/write | Creates or updates an deployment. |
> | Eylem | Microsoft.Resources/deploymentScripts/delete | Deletes a deployment script |
> | Eylem | Microsoft.Resources/deploymentScripts/logs/read | Gets or lists deployment script logs |
> | Eylem | Microsoft.Resources/deploymentScripts/read | Gets or lists deployment scripts |
> | Eylem | Microsoft.Resources/deploymentScripts/write | Creates or updates a deployment script |
> | Eylem | Microsoft.Resources/links/delete | Deletes a resource link. |
> | Eylem | Microsoft.Resources/links/read | Gets or lists resource links. |
> | Eylem | Microsoft.Resources/links/write | Creates or updates a resource link. |
> | Eylem | Microsoft.Resources/marketplace/purchase/action | Purchases a resource from the marketplace. |
> | Eylem | Microsoft.Resources/providers/read | Get the list of providers. |
> | Eylem | Microsoft.Resources/resources/read | Get the list of resources based upon filters. |
> | Eylem | Microsoft.Resources/subscriptions/locations/read | Gets the list of locations supported. |
> | Eylem | Microsoft.Resources/subscriptions/operationresults/read | Get the subscription operation results. |
> | Eylem | Microsoft.Resources/subscriptions/providers/read | Gets or lists resource providers. |
> | Eylem | Microsoft.Resources/subscriptions/read | Gets the list of subscriptions. |
> | Eylem | Microsoft.Resources/subscriptions/resourceGroups/delete | Deletes a resource group and all its resources. |
> | Eylem | Microsoft.Resources/subscriptions/resourcegroups/deployments/operations/read | Gets or lists deployment operations. |
> | Eylem | Microsoft.Resources/subscriptions/resourcegroups/deployments/operationstatuses/read | Gets or lists deployment operation statuses. |
> | Eylem | Microsoft.Resources/subscriptions/resourcegroups/deployments/read | Gets or lists deployments. |
> | Eylem | Microsoft.Resources/subscriptions/resourcegroups/deployments/write | Creates or updates an deployment. |
> | Eylem | Microsoft.Resources/subscriptions/resourceGroups/moveResources/action | Moves resources from one resource group to another. |
> | Eylem | Microsoft.Resources/subscriptions/resourceGroups/read | Gets or lists resource groups. |
> | Eylem | Microsoft.Resources/subscriptions/resourcegroups/resources/read | Gets the resources for the resource group. |
> | Eylem | Microsoft.Resources/subscriptions/resourceGroups/validateMoveResources/action | Validate move of resources from one resource group to another. |
> | Eylem | Microsoft.Resources/subscriptions/resourceGroups/write | Creates or updates a resource group. |
> | Eylem | Microsoft.Resources/subscriptions/resources/read | Gets resources of a subscription. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/delete | Deletes a subscription tag. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/read | Gets or lists subscription tags. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/tagValues/delete | Deletes a subscription tag value. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/tagValues/read | Gets or lists subscription tag values. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/tagValues/write | Adds a subscription tag value. |
> | Eylem | Microsoft.Resources/subscriptions/tagNames/write | Adds a subscription tag. |
> | Eylem | Microsoft.Resources/tags/delete | Removes all the tags on a resource. |
> | Eylem | Microsoft.Resources/tags/read | Gets all the tags on a resource. |
> | Eylem | Microsoft.Resources/tags/write | Updates the tags on a resource by replacing or merging existing tags with a new set of tags, or removing existing tags. |
> | Eylem | Microsoft.Resources/tenants/read | Gets the list of tenants. |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Scheduler/jobcollections/delete | Deletes job collection. |
> | Eylem | Microsoft.Scheduler/jobcollections/disable/action | Disables job collection. |
> | Eylem | Microsoft.Scheduler/jobcollections/enable/action | Enables job collection. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/delete | Deletes job. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/generateLogicAppDefinition/action | Generates Logic App definition based on a Scheduler Job. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/jobhistories/read | Gets job history. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/read | Gets job. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/run/action | Runs job. |
> | Eylem | Microsoft.Scheduler/jobcollections/jobs/write | Creates or updates job. |
> | Eylem | Microsoft.Scheduler/jobcollections/read | Get Job Collection |
> | Eylem | Microsoft.Scheduler/jobcollections/write | Creates or updates job collection. |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Search/checkNameAvailability/action | Checks availability of the service name. |
> | Eylem | Microsoft.Search/operations/read | Lists all of the available operations of the Microsoft.Search provider. |
> | Eylem | Microsoft.Search/register/action | Registers the subscription for the search resource provider and enables the creation of search services. |
> | Eylem | Microsoft.Search/searchServices/createQueryKey/action | Creates the query key. |
> | Eylem | Microsoft.Search/searchServices/delete | Deletes the search service. |
> | Eylem | Microsoft.Search/searchServices/deleteQueryKey/delete | Deletes the query key. |
> | Eylem | Microsoft.Search/searchServices/listAdminKeys/action | Reads the admin keys. |
> | Eylem | Microsoft.Search/searchServices/listQueryKeys/action | Returns the list of query API keys for the given Azure Search service. |
> | Eylem | Microsoft.Search/searchServices/listQueryKeys/read | Returns the list of query API keys for the given Azure Search service. |
> | Eylem | Microsoft.Search/searchServices/privateEndpointConnectionProxies/delete | Deletes an existing private endpoint connection proxy |
> | Eylem | Microsoft.Search/searchServices/privateEndpointConnectionProxies/read | Returns the list of private endpoint connection proxies or gets the properties for the specified private endpoint connection proxy |
> | Eylem | Microsoft.Search/searchServices/privateEndpointConnectionProxies/validate/action | Validates a private endpoint connection create call from NRP side |
> | Eylem | Microsoft.Search/searchServices/privateEndpointConnectionProxies/write | Creates a private endpoint connection proxy with the specified parameters or updates the properties or tags for the specified private endpoint connection proxy |
> | Eylem | Microsoft.Search/searchServices/read | Reads the search service. |
> | Eylem | Microsoft.Search/searchServices/regenerateAdminKey/action | Regenerates the admin key. |
> | Eylem | Microsoft.Search/searchServices/start/action | Starts the search service. |
> | Eylem | Microsoft.Search/searchServices/stop/action | Stops the search service. |
> | Eylem | Microsoft.Search/searchServices/write | Creates or updates the search service. |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Security/adaptiveNetworkHardenings/enforce/action | Enforces the given traffic hardening rules by creating matching security rules on the given Network Security Group(s) |
> | Eylem | Microsoft.Security/adaptiveNetworkHardenings/read | Gets Adaptive Network Hardening recommendations of an Azure protected resource |
> | Eylem | Microsoft.Security/advancedThreatProtectionSettings/read | Gets the Advanced Threat Protection Settings for the resource |
> | Eylem | Microsoft.Security/advancedThreatProtectionSettings/write | Updates the Advanced Threat Protection Settings for the resource |
> | Eylem | Microsoft.Security/alerts/read | Gets all available security alerts |
> | Eylem | Microsoft.Security/applicationWhitelistings/read | Gets the application whitelistings |
> | Eylem | Microsoft.Security/applicationWhitelistings/write | Creates a new application whitelisting or updates an existing one |
> | Eylem | Microsoft.Security/assessmentMetadata/read | Get available security assessment metadata on your subscription |
> | Eylem | Microsoft.Security/assessmentMetadata/write | Create or update a security assessment metadata |
> | Eylem | Microsoft.Security/assessments/read | Get security assessments on your subscription |
> | Eylem | Microsoft.Security/assessments/write | Create or update security assessments on your subscription |
> | Eylem | Microsoft.Security/complianceResults/read | Gets the compliance results for the resource |
> | Eylem | Microsoft.Security/informationProtectionPolicies/read | Gets the information protection policies for the resource |
> | Eylem | Microsoft.Security/informationProtectionPolicies/write | Updates the information protection policies for the resource |
> | Eylem | Microsoft.Security/locations/alerts/activate/action | Activate a security alert |
> | Eylem | Microsoft.Security/locations/alerts/dismiss/action | Dismiss a security alert |
> | Eylem | Microsoft.Security/locations/alerts/read | Gets all available security alerts |
> | Eylem | Microsoft.Security/locations/jitNetworkAccessPolicies/delete | Deletes the just-in-time network access policy |
> | Eylem | Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action | Initiates a just-in-time network access policy request |
> | Eylem | Microsoft.Security/locations/jitNetworkAccessPolicies/read | Gets the just-in-time network access policies |
> | Eylem | Microsoft.Security/locations/jitNetworkAccessPolicies/write | Creates a new just-in-time network access policy or updates an existing one |
> | Eylem | Microsoft.Security/locations/read | Gets the security data location |
> | Eylem | Microsoft.Security/locations/tasks/activate/action | Activate a security recommendation |
> | Eylem | Microsoft.Security/locations/tasks/dismiss/action | Dismiss a security recommendation |
> | Eylem | Microsoft.Security/locations/tasks/read | Gets all available security recommendations |
> | Eylem | Microsoft.Security/locations/tasks/resolve/action | Resolve a security recommendation |
> | Eylem | Microsoft.Security/locations/tasks/start/action | Start a security recommendation |
> | Eylem | Microsoft.Security/policies/read | Gets the security policy |
> | Eylem | Microsoft.Security/policies/write | Updates the security policy |
> | Eylem | Microsoft.Security/pricings/delete | Deletes the pricing settings for the scope |
> | Eylem | Microsoft.Security/pricings/read | Gets the pricing settings for the scope |
> | Eylem | Microsoft.Security/pricings/write | Updates the pricing settings for the scope |
> | Eylem | Microsoft.Security/register/action | Registers the subscription for Azure Security Center |
> | Eylem | Microsoft.Security/securityContacts/delete | Deletes the security contact |
> | Eylem | Microsoft.Security/securityContacts/read | Gets the security contact |
> | Eylem | Microsoft.Security/securityContacts/write | Updates the security contact |
> | Eylem | Microsoft.Security/securitySolutions/delete | Deletes a security solution |
> | Eylem | Microsoft.Security/securitySolutions/read | Gets the security solutions |
> | Eylem | Microsoft.Security/securitySolutions/write | Creates a new security solution or updates an existing one |
> | Eylem | Microsoft.Security/securitySolutionsReferenceData/read | Gets the security solutions reference data |
> | Eylem | Microsoft.Security/securityStatuses/read | Gets the security health statuses for Azure resources |
> | Eylem | Microsoft.Security/securityStatusesSummaries/read | Gets the security statuses summaries for the scope |
> | Eylem | Microsoft.Security/settings/read | Gets the settings for the scope |
> | Eylem | Microsoft.Security/settings/write | Updates the settings for the scope |
> | Eylem | Microsoft.Security/tasks/read | Gets all available security recommendations |
> | Eylem | Microsoft.Security/unregister/action | Unregisters the subscription from Azure Security Center |
> | Eylem | Microsoft.Security/webApplicationFirewalls/delete | Deletes a web application firewall |
> | Eylem | Microsoft.Security/webApplicationFirewalls/read | Gets the web application firewalls |
> | Eylem | Microsoft.Security/webApplicationFirewalls/write | Creates a new web application firewall or updates an existing one |
> | Eylem | Microsoft.Security/workspaceSettings/connect/action | Change workspace settings reconnection settings |
> | Eylem | Microsoft.Security/workspaceSettings/delete | Deletes the workspace settings |
> | Eylem | Microsoft.Security/workspaceSettings/read | Gets the workspace settings |
> | Eylem | Microsoft.Security/workspaceSettings/write | Updates the workspace settings |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.SecurityGraph/diagnosticsettings/delete | Deleting a diagnostic setting |
> | Eylem | Microsoft.SecurityGraph/diagnosticsettings/read | Reading a diagnostic setting |
> | Eylem | Microsoft.SecurityGraph/diagnosticsettings/write | Writing a diagnostic setting |
> | Eylem | Microsoft.SecurityGraph/diagnosticsettingscategories/read | Reading a diagnostic setting categories |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.SecurityInsights/Aggregations/read | Gets aggregated information |
> | Eylem | Microsoft.SecurityInsights/alertRules/actions/delete | Deletes the response actions of an alert rule |
> | Eylem | Microsoft.SecurityInsights/alertRules/actions/read | Gets the response actions of an alert rule |
> | Eylem | Microsoft.SecurityInsights/alertRules/actions/write | Updates the response actions of an alert rule |
> | Eylem | Microsoft.SecurityInsights/alertRules/delete | Deletes alert rules |
> | Eylem | Microsoft.SecurityInsights/alertRules/read | Gets the alert rules |
> | Eylem | Microsoft.SecurityInsights/alertRules/write | Updates alert rules |
> | Eylem | Microsoft.SecurityInsights/Bookmarks/delete | Deletes bookmarks |
> | Eylem | Microsoft.SecurityInsights/Bookmarks/expand/action | Gets related entities of an entity by a specific expansion |
> | Eylem | Microsoft.SecurityInsights/Bookmarks/read | Gets bookmarks |
> | Eylem | Microsoft.SecurityInsights/Bookmarks/write | Updates bookmarks |
> | Eylem | Microsoft.SecurityInsights/cases/comments/read | Gets the case comments |
> | Eylem | Microsoft.SecurityInsights/cases/comments/write | Creates the case comments |
> | Eylem | Microsoft.SecurityInsights/cases/delete | Deletes a case |
> | Eylem | Microsoft.SecurityInsights/cases/investigations/read | Gets the case investigations |
> | Eylem | Microsoft.SecurityInsights/cases/investigations/write | Updates the metadata of a case |
> | Eylem | Microsoft.SecurityInsights/cases/read | Gets a case |
> | Eylem | Microsoft.SecurityInsights/cases/write | Updates a case |
> | Eylem | Microsoft.SecurityInsights/dataConnectors/delete | Deletes a data connector |
> | Eylem | Microsoft.SecurityInsights/dataConnectors/read | Gets the data connectors |
> | Eylem | Microsoft.SecurityInsights/dataConnectors/write | Updates a data connector |
> | Eylem | Microsoft.SecurityInsights/register/action | Registers the subscription to Azure Sentinel |
> | Eylem | Microsoft.SecurityInsights/settings/read | Gets settings |
> | Eylem | Microsoft.SecurityInsights/settings/write | Güncelleştirme ayarları |
> | Eylem | Microsoft.SecurityInsights/unregister/action | Unregisters the subscription from Azure Sentinel |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ServiceBus/checkNameAvailability/action | Checks availability of namespace under given subscription. |
> | Eylem | Microsoft.ServiceBus/checkNamespaceAvailability/action | Checks availability of namespace under given subscription. This API is deprecated please use CheckNameAvailability instead. |
> | Eylem | Microsoft.ServiceBus/locations/deleteVirtualNetworkOrSubnets/action | Deletes the VNet rules in ServiceBus Resource Provider for the specified VNet |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/action | Updates Namespace Authorization Rule. This API is deprecated. Please use a PUT call to update the Namespace Authorization Rule instead.. This operation is not supported on API version 2017-04-01. |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/delete | Delete Namespace Authorization Rule. The Default Namespace Authorization Rule cannot be deleted.  |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/listkeys/action | Get the Connection String to the Namespace |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/read | Get the list of Namespaces Authorization Rules description. |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/regenerateKeys/action | Regenerate the Primary or Secondary key to the Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/authorizationRules/write | Create a Namespace level Authorization Rules and update its properties. The Authorization Rules Access Rights, the Primary and Secondary Keys can be updated. |
> | Eylem | Microsoft.ServiceBus/namespaces/Delete | Delete Namespace Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Gets the authorization rules keys for the Disaster Recovery primary namespace |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/read | Get Disaster Recovery Primary Namespace's Authorization Rules |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/breakPairing/action | Disables Disaster Recovery and stops replicating changes from primary to secondary namespaces. |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Checks availability of namespace alias under given subscription. |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/delete | Deletes the Disaster Recovery configuration associated with the namespace. This operation can only be invoked via the primary namespace. |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/failover/action | Invokes a GEO DR failover and reconfigures the namespace alias to point to the secondary namespace. |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/read | Gets the Disaster Recovery configuration associated with the namespace. |
> | Eylem | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/write | Creates or Updates the Disaster Recovery configuration associated with the namespace. |
> | Eylem | Microsoft.ServiceBus/namespaces/eventGridFilters/delete | Deletes the Event Grid filter associated with the namespace. |
> | Eylem | Microsoft.ServiceBus/namespaces/eventGridFilters/read | Gets the Event Grid filter associated with the namespace. |
> | Eylem | Microsoft.ServiceBus/namespaces/eventGridFilters/write | Creates or Updates the Event Grid filter associated with the namespace. |
> | Eylem | Microsoft.ServiceBus/namespaces/eventhubs/read | Get list of EventHub Resource Descriptions |
> | Eylem | Microsoft.ServiceBus/namespaces/ipFilterRules/delete | Delete IP Filter Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/ipFilterRules/read | Get IP Filter Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/ipFilterRules/write | Create IP Filter Resource |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/receive/action | İleti alma |
> | DataAction | Microsoft.ServiceBus/namespaces/messages/send/action | İleti gönderme |
> | Eylem | Microsoft.ServiceBus/namespaces/messagingPlan/read | Gets the Messaging Plan for a namespace.<br>This API is deprecated.<br>Properties exposed via the MessagingPlan resource are moved to the (parent) Namespace resource in later API versions..<br>This operation is not supported on API version 2017-04-01. |
> | Eylem | Microsoft.ServiceBus/namespaces/messagingPlan/write | Updates the Messaging Plan for a namespace.<br>This API is deprecated.<br>Properties exposed via the MessagingPlan resource are moved to the (parent) Namespace resource in later API versions..<br>This operation is not supported on API version 2017-04-01. |
> | Eylem | Microsoft.ServiceBus/namespaces/migrate/action | Migrate namespace operation |
> | Eylem | Microsoft.ServiceBus/namespaces/migrationConfigurations/delete | Deletes the Migration configuration. |
> | Eylem | Microsoft.ServiceBus/namespaces/migrationConfigurations/read | Gets the Migration configuration which indicates the state of the migration and pending replication operations |
> | Eylem | Microsoft.ServiceBus/namespaces/migrationConfigurations/revert/action | Reverts the standard to premium namespace migration |
> | Eylem | Microsoft.ServiceBus/namespaces/migrationConfigurations/upgrade/action | Assigns the DNS associated with the standard namespace to the premium namespace which completes the migration and stops the syncing resources from standard to premium namespace |
> | Eylem | Microsoft.ServiceBus/namespaces/migrationConfigurations/write | Creates or Updates Migration configuration. This will start synchronizing resources from the standard to the premium namespace |
> | Eylem | Microsoft.ServiceBus/namespaces/networkruleset/delete | Delete VNET Rule Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/networkruleset/read | Gets NetworkRuleSet Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/networkruleset/write | Create VNET Rule Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/networkrulesets/delete | Delete VNET Rule Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/networkrulesets/read | Gets NetworkRuleSet Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/networkrulesets/write | Create VNET Rule Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/operationresults/read | Get the status of Namespace operation |
> | Eylem | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Get list of Namespace diagnostic settings Resource Descriptions |
> | Eylem | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Get list of Namespace diagnostic settings Resource Descriptions |
> | Eylem | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/logDefinitions/read | Get list of Namespace logs Resource Descriptions |
> | Eylem | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Get list of Namespace metrics Resource Descriptions |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/action | Operation to update Queue. This operation is not supported on API version 2017-04-01. Authorization Rules. Please use a PUT call to update Authorization Rule. |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/delete | Operation to delete Queue Authorization Rules |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/listkeys/action | Get the Connection String to Queue |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/read |  Get the list of Queue Authorization Rules |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/regenerateKeys/action | Regenerate the Primary or Secondary key to the Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/authorizationRules/write | Create Queue Authorization Rules and Update its properties. The Authorization Rules Access Rights can be updated. |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/Delete | Operation to delete Queue Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/read | Get list of Queue Resource Descriptions |
> | Eylem | Microsoft.ServiceBus/namespaces/queues/write | Create or Update Queue properties. |
> | Eylem | Microsoft.ServiceBus/namespaces/read | Get the list of Namespace Resource Description |
> | Eylem | Microsoft.ServiceBus/namespaces/removeAcsNamepsace/action | Remove ACS namespace |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/action | Operation to update Topic. This operation is not supported on API version 2017-04-01. Authorization Rules. Please use a PUT call to update Authorization Rule. |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/delete | Operation to delete Topic Authorization Rules |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/listkeys/action | Get the Connection String to Topic |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/read |  Get the list of Topic Authorization Rules |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/regenerateKeys/action | Regenerate the Primary or Secondary key to the Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/authorizationRules/write | Create Topic Authorization Rules and Update its properties. The Authorization Rules Access Rights can be updated. |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/Delete | Operation to delete Topic Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/read | Get list of Topic Resource Descriptions |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/Delete | Operation to delete TopicSubscription Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/read | Get list of TopicSubscription Resource Descriptions |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/Delete | Operation to delete Rule Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/read | Get list of Rule Resource Descriptions |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/write | Create or Update Rule properties. |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/subscriptions/write | Create or Update TopicSubscription properties. |
> | Eylem | Microsoft.ServiceBus/namespaces/topics/write | Create or Update Topic properties. |
> | Eylem | Microsoft.ServiceBus/namespaces/virtualNetworkRules/delete | Delete VNET Rule Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/virtualNetworkRules/read | Gets VNET Rule Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/virtualNetworkRules/write | Create VNET Rule Resource |
> | Eylem | Microsoft.ServiceBus/namespaces/write | Create a Namespace Resource and Update its properties. Tags and Capacity of the Namespace are the properties which can be updated. |
> | Eylem | Microsoft.ServiceBus/operations/read | Get Operations |
> | Eylem | Microsoft.ServiceBus/register/action | Registers the subscription for the ServiceBus resource provider and enables the creation of ServiceBus resources |
> | Eylem | Microsoft.ServiceBus/sku/read | Get list of Sku Resource Descriptions |
> | Eylem | Microsoft.ServiceBus/sku/regions/read | Get list of SkuRegions Resource Descriptions |
> | Eylem | Microsoft.ServiceBus/unregister/action | Registers the subscription for the ServiceBus resource provider and enables the creation of ServiceBus resources |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/delete | Delete any Application |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/read | Read any Application |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/delete | Delete any Service |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/partitions/read | Read any Partition |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/partitions/replicas/read | Read any Replica |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/read | Read any Service |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/statuses/read | Read any Service Status |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/services/write | Create or Update any Service |
> | Eylem | Microsoft.ServiceFabric/clusters/applications/write | Create or Update any Application |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/delete | Delete any Application Type |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/read | Read any Application Type |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/versions/delete | Delete any Application Type Version |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/versions/read | Read any Application Type Version |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/versions/write | Create or Update any Application Type Version |
> | Eylem | Microsoft.ServiceFabric/clusters/applicationTypes/write | Create or Update any Application Type |
> | Eylem | Microsoft.ServiceFabric/clusters/delete | Delete any Cluster |
> | Eylem | Microsoft.ServiceFabric/clusters/nodes/read | Read any Node |
> | Eylem | Microsoft.ServiceFabric/clusters/read | Read any Cluster |
> | Eylem | Microsoft.ServiceFabric/clusters/statuses/read | Read any Cluster Status |
> | Eylem | Microsoft.ServiceFabric/clusters/write | Create or Update any Cluster |
> | Eylem | Microsoft.ServiceFabric/locations/clusterVersions/read | Read any Cluster Version |
> | Eylem | Microsoft.ServiceFabric/locations/environments/clusterVersions/read | Read any Cluster Version for a specific environment |
> | Eylem | Microsoft.ServiceFabric/locations/operationresults/read | Read any Operation Results |
> | Eylem | Microsoft.ServiceFabric/locations/operations/read | Read any Operations by location |
> | Eylem | Microsoft.ServiceFabric/operations/read | Read any Available Operations |
> | Eylem | Microsoft.ServiceFabric/register/action | Register any Action |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.SignalRService/locations/checknameavailability/action | Checks if a name is available for use with a new SignalR service |
> | Eylem | Microsoft.SignalRService/locations/operationresults/signalr/read | Query the status of an asynchronous operation |
> | Eylem | Microsoft.SignalRService/locations/operationStatuses/operationId/read | Query the status of an asynchronous operation |
> | Eylem | Microsoft.SignalRService/locations/usages/read | Get the quota usages for Azure SignalR service |
> | Eylem | Microsoft.SignalRService/operationresults/read | Query the status of an asynchronous operation |
> | Eylem | Microsoft.SignalRService/operations/read | List the operations for Azure SignalR service. |
> | Eylem | Microsoft.SignalRService/operationstatus/read | Query the status of an asynchronous operation |
> | Eylem | Microsoft.SignalRService/register/action | Registers the 'Microsoft.SignalRService' resource provider with a subscription |
> | Eylem | Microsoft.SignalRService/SignalR/delete | Delete the entire SignalR service |
> | Eylem | Microsoft.SignalRService/SignalR/eventGridFilters/delete | Delete an event grid filter from a SignalR. |
> | Eylem | Microsoft.SignalRService/SignalR/eventGridFilters/read | Get the properties of the specified event grid filter or lists all the event grid filters for the specified SignalR. |
> | Eylem | Microsoft.SignalRService/SignalR/eventGridFilters/write | Create or update an event grid filter for a SignalR with the specified parameters. |
> | Eylem | Microsoft.SignalRService/SignalR/listkeys/action | View the value of SignalR access keys in the management portal or through API |
> | Eylem | Microsoft.SignalRService/SignalR/privateEndpointConnectionProxies/delete | Delete a Private Endpoint Connection Proxy |
> | Eylem | Microsoft.SignalRService/SignalR/privateEndpointConnectionProxies/read | Read a Private Endpoint Connetion Proxy |
> | Eylem | Microsoft.SignalRService/SignalR/privateEndpointConnectionProxies/validate/action | Validate a Private Endpoint Connection Proxy |
> | Eylem | Microsoft.SignalRService/SignalR/privateEndpointConnectionProxies/write | Create a Private Endpoint Connection Proxy |
> | Eylem | Microsoft.SignalRService/SignalR/privateEndpointConnections/read | Read a Private Endpoint Connection |
> | Eylem | Microsoft.SignalRService/SignalR/privateEndpointConnections/write | Approve or reject a Private Endpoint Connection |
> | Eylem | Microsoft.SignalRService/SignalR/privateLinkResources/read | List all SignalR Private Link Resources |
> | Eylem | Microsoft.SignalRService/SignalR/read | View the SignalR's settings and configurations in the management portal or through API |
> | Eylem | Microsoft.SignalRService/SignalR/regeneratekey/action | Change the value of SignalR access keys in the management portal or through API |
> | Eylem | Microsoft.SignalRService/SignalR/restart/action | To restart an Azure SignalR service in the management portal or through API. There will be certain downtime. |
> | Eylem | Microsoft.SignalRService/SignalR/write | Modify the SignalR's settings and configurations in the management portal or through API |
> | Eylem | Microsoft.SignalRService/unregister/action | Unregisters the 'Microsoft.SignalRService' resource provider with a subscription |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Solutions/applicationDefinitions/applicationArtifacts/read | Lists application artifacts of application definition. |
> | Eylem | Microsoft.Solutions/applicationDefinitions/delete | Removes an application definition. |
> | Eylem | Microsoft.Solutions/applicationDefinitions/read | Retrieves a list of application definitions. |
> | Eylem | Microsoft.Solutions/applicationDefinitions/write | Add or modify an application definition. |
> | Eylem | Microsoft.Solutions/applications/applicationArtifacts/read | Lists application artifacts. |
> | Eylem | Microsoft.Solutions/applications/delete | Removes an application. |
> | Eylem | Microsoft.Solutions/applications/read | Retrieves a list of applications. |
> | Eylem | Microsoft.Solutions/applications/refreshPermissions/action | Refreshes application permission(s). |
> | Eylem | Microsoft.Solutions/applications/updateAccess/action | Updates application access. |
> | Eylem | Microsoft.Solutions/applications/write | Uygulama oluşturur. |
> | Eylem | Microsoft.Solutions/jitRequests/delete | Remove a JitRequest |
> | Eylem | Microsoft.Solutions/jitRequests/read | Retrieves a list of JitRequests |
> | Eylem | Microsoft.Solutions/jitRequests/write | Creates a JitRequest |
> | Eylem | Microsoft.Solutions/locations/operationStatuses/read | Reads the operation status for the resource. |
> | Eylem | Microsoft.Solutions/operations/read | Gets the list of operations. |
> | Eylem | Microsoft.Solutions/register/action | Register to Solutions. |
> | Eylem | Microsoft.Solutions/unregister/action | Unregisters from Solutions. |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Sql/checkNameAvailability/action | Verify whether given server name is available for provisioning worldwide for a given subscription. |
> | Eylem | Microsoft.Sql/instancePools/delete | Deletes an instance pool |
> | Eylem | Microsoft.Sql/instancePools/read | Gets an instance pool |
> | Eylem | Microsoft.Sql/instancePools/usages/read | Gets an instance pool's usage info |
> | Eylem | Microsoft.Sql/instancePools/write | Creates or updates an instance pool |
> | Eylem | Microsoft.Sql/locations/auditingSettingsAzureAsyncOperation/read | Retrieve result of the extended server blob auditing policy Set operation |
> | Eylem | Microsoft.Sql/locations/auditingSettingsOperationResults/read | Retrieve result of the server blob auditing policy Set operation |
> | Eylem | Microsoft.Sql/locations/capabilities/read | Gets the capabilities for this subscription in a given location |
> | Eylem | Microsoft.Sql/locations/databaseAzureAsyncOperation/read | Gets the status of a database operation. |
> | Eylem | Microsoft.Sql/locations/databaseOperationResults/read | Gets the status of a database operation. |
> | Eylem | Microsoft.Sql/locations/deletedServerAsyncOperation/read | Gets in-progress operations on deleted server |
> | Eylem | Microsoft.Sql/locations/deletedServerOperationResults/read | Gets in-progress operations on deleted server |
> | Eylem | Microsoft.Sql/locations/deletedServers/read | Return the list of deleted servers or gets the properties for the specified deleted server. |
> | Eylem | Microsoft.Sql/locations/deletedServers/recover/action | Recover a deleted server |
> | Eylem | Microsoft.Sql/locations/elasticPoolAzureAsyncOperation/read | Gets the azure async operation for an elastic pool async operation |
> | Eylem | Microsoft.Sql/locations/elasticPoolOperationResults/read | Gets the result of an elastic pool operation. |
> | Eylem | Microsoft.Sql/locations/encryptionProtectorAzureAsyncOperation/read | Gets in-progress operations on transparent data encryption encryption protector |
> | Eylem | Microsoft.Sql/locations/encryptionProtectorOperationResults/read | Gets in-progress operations on transparent data encryption encryption protector |
> | Eylem | Microsoft.Sql/locations/extendedAuditingSettingsAzureAsyncOperation/read | Retrieve result of the extended server blob auditing policy Set operation |
> | Eylem | Microsoft.Sql/locations/extendedAuditingSettingsOperationResults/read | Retrieve result of the extended server blob auditing policy Set operation |
> | Eylem | Microsoft.Sql/locations/firewallRulesAzureAsyncOperation/read | Gets the status of a firewall rule operation. |
> | Eylem | Microsoft.Sql/locations/firewallRulesOperationResults/read | Gets the status of a firewall rule operation. |
> | Eylem | Microsoft.Sql/locations/instanceFailoverGroups/delete | Deletes an existing instance failover group. |
> | Eylem | Microsoft.Sql/locations/instanceFailoverGroups/failover/action | Executes planned failover in an existing instance failover group. |
> | Eylem | Microsoft.Sql/locations/instanceFailoverGroups/forceFailoverAllowDataLoss/action | Executes forced failover in an existing instance failover group. |
> | Eylem | Microsoft.Sql/locations/instanceFailoverGroups/read | Returns the list of instance failover groups or gets the properties for the specified instance failover group. |
> | Eylem | Microsoft.Sql/locations/instanceFailoverGroups/write | Creates an instance failover group with the specified parameters or updates the properties or tags for the specified instance failover group. |
> | Eylem | Microsoft.Sql/locations/instancePoolAzureAsyncOperation/read | Gets the status of an instance pool operation |
> | Eylem | Microsoft.Sql/locations/instancePoolOperationResults/read | Gets the result for an instance pool operation |
> | Eylem | Microsoft.Sql/locations/interfaceEndpointProfileAzureAsyncOperation/read | Returns the details of a specific interface endpoint Azure async operation |
> | Eylem | Microsoft.Sql/locations/interfaceEndpointProfileOperationResults/read | Returns the details of the specified interface endpoint profile operation |
> | Eylem | Microsoft.Sql/locations/jobAgentAzureAsyncOperation/read | Gets the status of an job agent operation. |
> | Eylem | Microsoft.Sql/locations/jobAgentOperationResults/read | Gets the result of an job agent operation. |
> | Eylem | Microsoft.Sql/locations/longTermRetentionBackups/read | Lists the long term retention backups for every database on every server in a location |
> | Eylem | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionBackups/read | Lists the long term retention backups for every database on a server |
> | Eylem | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete | Deletes a long term retention backup |
> | Eylem | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/read | Lists the long term retention backups for a database |
> | Eylem | Microsoft.Sql/locations/managedDatabaseRestoreAzureAsyncOperation/completeRestore/action | Completes managed database restore operation |
> | Eylem | Microsoft.Sql/locations/managedInstanceEncryptionProtectorAzureAsyncOperation/read | Gets in-progress operations on transparent data encryption managed instance encryption protector |
> | Eylem | Microsoft.Sql/locations/managedInstanceEncryptionProtectorOperationResults/read | Gets in-progress operations on transparent data encryption managed instance encryption protector |
> | Eylem | Microsoft.Sql/locations/managedInstanceKeyAzureAsyncOperation/read | Gets in-progress operations on transparent data encryption managed instance keys |
> | Eylem | Microsoft.Sql/locations/managedInstanceKeyOperationResults/read | Gets in-progress operations on transparent data encryption managed instance keys |
> | Eylem | Microsoft.Sql/locations/managedInstanceLongTermRetentionPolicyAzureAsyncOperation/read | Gets the status of a long term retention policy operation for a managed database |
> | Eylem | Microsoft.Sql/locations/managedInstanceLongTermRetentionPolicyOperationResults/read | Gets the status of a long term retention policy operation for a managed database |
> | Eylem | Microsoft.Sql/locations/managedShortTermRetentionPolicyOperationResults/read | Gets the status of a short term retention policy operation |
> | Eylem | Microsoft.Sql/locations/managedTransparentDataEncryptionAzureAsyncOperation/read | Gets in-progress operations on managed database transparent data encryption |
> | Eylem | Microsoft.Sql/locations/managedTransparentDataEncryptionOperationResults/read | Gets in-progress operations on managed database transparent data encryption |
> | Eylem | Microsoft.Sql/locations/privateEndpointConnectionAzureAsyncOperation/read | Gets the result for a private endpoint connection operation |
> | Eylem | Microsoft.Sql/locations/privateEndpointConnectionOperationResults/read | Gets the result for a private endpoint connection operation |
> | Eylem | Microsoft.Sql/locations/privateEndpointConnectionProxyAzureAsyncOperation/read | Gets the result for a private endpoint connection proxy operation |
> | Eylem | Microsoft.Sql/locations/privateEndpointConnectionProxyOperationResults/read | Gets the result for a private endpoint connection proxy operation |
> | Eylem | Microsoft.Sql/locations/read | Gets the available locations for a given subscription |
> | Eylem | Microsoft.Sql/locations/serverKeyAzureAsyncOperation/read | Gets in-progress operations on transparent data encryption server keys |
> | Eylem | Microsoft.Sql/locations/serverKeyOperationResults/read | Gets in-progress operations on transparent data encryption server keys |
> | Eylem | Microsoft.Sql/locations/shortTermRetentionPolicyOperationResults/read | Gets the status of a short term retention policy operation |
> | Eylem | Microsoft.Sql/locations/syncAgentOperationResults/read | Retrieve result of the sync agent resource operation |
> | Eylem | Microsoft.Sql/locations/syncDatabaseIds/read | Retrieve the sync database ids for a particular region and subscription |
> | Eylem | Microsoft.Sql/locations/syncGroupOperationResults/read | Retrieve result of the sync group resource operation |
> | Eylem | Microsoft.Sql/locations/syncMemberOperationResults/read | Retrieve result of the sync member resource operation |
> | Eylem | Microsoft.Sql/locations/usages/read | Gets a collection of usage metrics for this subscription in a location |
> | Eylem | Microsoft.Sql/locations/virtualNetworkRulesAzureAsyncOperation/read | Returns the details of the specified virtual network rules azure async operation  |
> | Eylem | Microsoft.Sql/locations/virtualNetworkRulesOperationResults/read | Returns the details of the specified virtual network rules operation  |
> | Eylem | Microsoft.Sql/managedInstances/administrators/delete | Deletes an existing administrator of managed instance. |
> | Eylem | Microsoft.Sql/managedInstances/administrators/read | Gets a list of managed instance administrators. |
> | Eylem | Microsoft.Sql/managedInstances/administrators/write | Creates or updates managed instance administrator with the specified parameters. |
> | Eylem | Microsoft.Sql/managedInstances/databases/backupLongTermRetentionPolicies/read | Gets a long term retention policy for a managed database |
> | Eylem | Microsoft.Sql/managedInstances/databases/backupLongTermRetentionPolicies/write | Updates a long term retention policy for a managed database |
> | Eylem | Microsoft.Sql/managedInstances/databases/backupShortTermRetentionPolicies/read | Gets a short term retention policy for a managed database |
> | Eylem | Microsoft.Sql/managedInstances/databases/backupShortTermRetentionPolicies/write | Updates a short term retention policy for a managed database |
> | Eylem | Microsoft.Sql/managedInstances/databases/columns/read | Return a list of columns for a managed database |
> | Eylem | Microsoft.Sql/managedInstances/databases/completeRestore/action | Completes managed database restore operation |
> | Eylem | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/read | List sensitivity labels of a given database |
> | Eylem | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/write | Batch update sensitivity labels |
> | Eylem | Microsoft.Sql/managedInstances/databases/delete | Deletes an existing managed database |
> | Eylem | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/diagnosticSettings/read | Gets the diagnostic setting for the resource |
> | Eylem | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/diagnosticSettings/write | Creates or updates the diagnostic setting for the resource |
> | Eylem | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/logDefinitions/read | Gets the available logs for managed instance databases |
> | Eylem | Microsoft.Sql/managedInstances/databases/read | Gets existing managed database |
> | Eylem | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/read | List sensitivity labels of a given database |
> | Eylem | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/write | Batch update recommended sensitivity labels |
> | Eylem | Microsoft.Sql/managedInstances/databases/restoreDetails/read | Returns managed database restore details while restore is in progress. |
> | Eylem | Microsoft.Sql/managedInstances/databases/schemas/read | Get a managed database schema. (schema only) |
> | Eylem | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/read | Get a managed database column (schema only) |
> | Eylem | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/delete | Delete the sensitivity label of a given column |
> | Eylem | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/disable/action | Disable sensitivity recommendations on a given column |
> | Eylem | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/enable/action | Enable sensitivity recommendations on a given column |
> | Eylem | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/read | Get the sensitivity label of a given column |
> | Eylem | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/write | Create or update the sensitivity label of a given column |
> | Eylem | Microsoft.Sql/managedInstances/databases/schemas/tables/read | Get a managed database table (schema only) |
> | Eylem | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/read | Retrieve a list of managed database threat detection policies configured for a given server |
> | Eylem | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/write | Change the database threat detection policy for a given managed database |
> | Eylem | Microsoft.Sql/managedInstances/databases/securityEvents/read | Retrieves the managed database security events |
> | Eylem | Microsoft.Sql/managedInstances/databases/sensitivityLabels/read | List sensitivity labels of a given database |
> | Eylem | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/read | Retrieve details of the database Transparent Data Encryption on a given managed database |
> | Eylem | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/write | Change the database Transparent Data Encryption for a given managed database |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/delete | Remove the vulnerability assessment for a given database |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/read | Retrieve the vulnerability assessment policies on a givendatabase |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/delete | Remove the vulnerability assessment rule baseline for a given database |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/read | Get the vulnerability assessment rule baseline for a given database |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/write | Change the vulnerability assessment rule baseline for a given database |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/export/action | Convert an existing scan result to a human readable format. If already exists nothing happens |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/initiateScan/action | Execute vulnerability assessment database scan. |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/read | Return the list of database vulnerability assessment scan records or get the scan record for the specified scan ID. |
> | Eylem | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/write | Change the vulnerability assessment for a given database |
> | Eylem | Microsoft.Sql/managedInstances/databases/write | Creates a new database or updates an existing database. |
> | Eylem | Microsoft.Sql/managedInstances/delete | Deletes an existing  managed instance. |
> | Eylem | Microsoft.Sql/managedInstances/encryptionProtector/read | Returns a list of server encryption protectors or gets the properties for the specified server encryption protector. |
> | Eylem | Microsoft.Sql/managedInstances/encryptionProtector/revalidate/action | Update the properties for the specified Server Encryption Protector. |
> | Eylem | Microsoft.Sql/managedInstances/encryptionProtector/write | Update the properties for the specified Server Encryption Protector. |
> | Eylem | Microsoft.Sql/managedInstances/inaccessibleManagedDatabases/read | Gets a list of inaccessible managed databases in a managed instance |
> | Eylem | Microsoft.Sql/managedInstances/keys/delete | Deletes an existing Azure SQL Managed Instance  key. |
> | Eylem | Microsoft.Sql/managedInstances/keys/read | Return the list of managed instance keys or gets the properties for the specified managed instance key. |
> | Eylem | Microsoft.Sql/managedInstances/keys/write | Creates a key with the specified parameters or update the properties or tags for the specified managed instance key. |
> | Eylem | Microsoft.Sql/managedInstances/metricDefinitions/read | Get managed instance metric definitions |
> | Eylem | Microsoft.Sql/managedInstances/metrics/read | Get managed instance metrics |
> | Eylem | Microsoft.Sql/managedInstances/operations/cancel/action | Cancels Azure SQL Managed Instance pending asynchronous operation that is not finished yet. |
> | Eylem | Microsoft.Sql/managedInstances/operations/read | Get managed instance operations |
> | Eylem | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/diagnosticSettings/read | Gets the diagnostic setting for the resource |
> | Eylem | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/diagnosticSettings/write | Creates or updates the diagnostic setting for the resource |
> | Eylem | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/logDefinitions/read | Gets the available logs for managed instances |
> | Eylem | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/metricDefinitions/read | Return types of metrics that are available for managed instances |
> | Eylem | Microsoft.Sql/managedInstances/read | Return the list of managed instances or gets the properties for the specified managed instance. |
> | Eylem | Microsoft.Sql/managedInstances/recoverableDatabases/read | Returns a list of recoverable managed databases |
> | Eylem | Microsoft.Sql/managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies/read | Gets a short term retention policy for a dropped managed database |
> | Eylem | Microsoft.Sql/managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies/write | Updates a short term retention policy for a dropped managed database |
> | Eylem | Microsoft.Sql/managedInstances/restorableDroppedDatabases/read | Returns a list of restorable dropped managed databases. |
> | Eylem | Microsoft.Sql/managedInstances/securityAlertPolicies/read | Retrieve a list of managed server threat detection policies configured for a given server |
> | Eylem | Microsoft.Sql/managedInstances/securityAlertPolicies/write | Change the managed server threat detection policy for a given managed server |
> | Eylem | Microsoft.Sql/managedInstances/tdeCertificates/action | Create/Update TDE certificate |
> | Eylem | Microsoft.Sql/managedInstances/vulnerabilityAssessments/delete | Remove the vulnerability assessment for a given managed instance |
> | Eylem | Microsoft.Sql/managedInstances/vulnerabilityAssessments/read | Retrieve the vulnerability assessment policies on a given managed instance |
> | Eylem | Microsoft.Sql/managedInstances/vulnerabilityAssessments/write | Change the vulnerability assessment for a given managed instance |
> | Eylem | Microsoft.Sql/managedInstances/write | Creates a managed instance with the specified parameters or update the properties or tags for the specified managed instance. |
> | Eylem | Microsoft.Sql/operations/read | Gets available REST operations |
> | Eylem | Microsoft.Sql/privateEndpointConnectionsApproval/action | Determines if user is allowed to approve a private endpoint connection |
> | Eylem | Microsoft.Sql/register/action | Registers the subscription for the Microsoft SQL Database resource provider and enables the creation of Microsoft SQL Databases. |
> | Eylem | Microsoft.Sql/servers/administrators/delete | Deletes a specific Azure Active Directory administrator object |
> | Eylem | Microsoft.Sql/servers/administrators/read | Gets a specific Azure Active Directory administrator object |
> | Eylem | Microsoft.Sql/servers/administrators/write | Adds or updates a specific Azure Active Directory administrator object |
> | Eylem | Microsoft.Sql/servers/advisors/read | Returns list of advisors available for the server |
> | Eylem | Microsoft.Sql/servers/advisors/recommendedActions/read | Returns list of recommended actions of specified advisor for the server |
> | Eylem | Microsoft.Sql/servers/advisors/recommendedActions/write | Apply the recommended action on the server |
> | Eylem | Microsoft.Sql/servers/advisors/write | Updates auto-execute status of an advisor on server level. |
> | Eylem | Microsoft.Sql/servers/auditingPolicies/read | Retrieve details of the default server table auditing policy configured on a given server |
> | Eylem | Microsoft.Sql/servers/auditingPolicies/write | Change the default server table auditing for a given server |
> | Eylem | Microsoft.Sql/servers/auditingSettings/operationResults/read | Retrieve result of the server blob auditing policy Set operation |
> | Eylem | Microsoft.Sql/servers/auditingSettings/read | Retrieve details of the server blob auditing policy configured on a given server |
> | Eylem | Microsoft.Sql/servers/auditingSettings/write | Change the server blob auditing for a given server |
> | Eylem | Microsoft.Sql/servers/automaticTuning/read | Returns automatic tuning settings for the server |
> | Eylem | Microsoft.Sql/servers/automaticTuning/write | Updates automatic tuning settings for the server and returns updated settings |
> | Eylem | Microsoft.Sql/servers/backupLongTermRetentionVaults/delete | Deletes an existing backup archival vault. |
> | Eylem | Microsoft.Sql/servers/backupLongTermRetentionVaults/read | This operation is used to get a backup long term retention vault. It returns information about the vault registered to this server |
> | Eylem | Microsoft.Sql/servers/backupLongTermRetentionVaults/write | This operation is used to register a backup long term retention vault to a server |
> | Eylem | Microsoft.Sql/servers/communicationLinks/delete | Deletes an existing server communication link. |
> | Eylem | Microsoft.Sql/servers/communicationLinks/read | Return the list of communication links of a specified server. |
> | Eylem | Microsoft.Sql/servers/communicationLinks/write | Create or update a server communication link. |
> | Eylem | Microsoft.Sql/servers/connectionPolicies/read | Return the list of server connection policies of a specified server. |
> | Eylem | Microsoft.Sql/servers/connectionPolicies/write | Create or update a server connection policy. |
> | Eylem | Microsoft.Sql/servers/databases/advisors/read | Returns list of advisors available for the database |
> | Eylem | Microsoft.Sql/servers/databases/advisors/recommendedActions/read | Returns list of recommended actions of specified advisor for the database |
> | Eylem | Microsoft.Sql/servers/databases/advisors/recommendedActions/write | Apply the recommended action on the database |
> | Eylem | Microsoft.Sql/servers/databases/advisors/write | Update auto-execute status of an advisor on database level. |
> | Eylem | Microsoft.Sql/servers/databases/auditingPolicies/read | Retrieve details of the table auditing policy configured on a given database |
> | Eylem | Microsoft.Sql/servers/databases/auditingPolicies/write | Change the table auditing policy for a given database |
> | Eylem | Microsoft.Sql/servers/databases/auditingSettings/read | Retrieve details of the blob auditing policy configured on a given database |
> | Eylem | Microsoft.Sql/servers/databases/auditingSettings/write | Change the blob auditing policy for a given database |
> | Eylem | Microsoft.Sql/servers/databases/auditRecords/read | Retrieve the database blob audit records |
> | Eylem | Microsoft.Sql/servers/databases/automaticTuning/read | Returns automatic tuning settings for a database |
> | Eylem | Microsoft.Sql/servers/databases/automaticTuning/write | Updates automatic tuning settings for a database and returns updated settings |
> | Eylem | Microsoft.Sql/servers/databases/azureAsyncOperation/read | Gets the status of a database operation. |
> | Eylem | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/read | Return the list of backup archival policies of a specified database. |
> | Eylem | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/write | Create or update a database backup archival policy. |
> | Eylem | Microsoft.Sql/servers/databases/backupShortTermRetentionPolicies/read | Gets a short term retention policy for a database |
> | Eylem | Microsoft.Sql/servers/databases/backupShortTermRetentionPolicies/write | Updates a short term retention policy for a database |
> | Eylem | Microsoft.Sql/servers/databases/columns/read | Return a list of columns for a database |
> | Eylem | Microsoft.Sql/servers/databases/connectionPolicies/read | Retrieve details of the connection policy configured on a given database |
> | Eylem | Microsoft.Sql/servers/databases/connectionPolicies/write | Change connection policy for a given database |
> | Eylem | Microsoft.Sql/servers/databases/currentSensitivityLabels/read | List sensitivity labels of a given database |
> | Eylem | Microsoft.Sql/servers/databases/currentSensitivityLabels/write | Batch update sensitivity labels |
> | Eylem | Microsoft.Sql/servers/databases/dataMaskingPolicies/read | Return the list of database data masking policies. |
> | Eylem | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/delete | Delete data masking policy rule for a given database |
> | Eylem | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/read | Retrieve details of the data masking policy rule configured on a given database |
> | Eylem | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/write | Change data masking policy rule for a given database |
> | Eylem | Microsoft.Sql/servers/databases/dataMaskingPolicies/write | Change data masking policy for a given database |
> | Eylem | Microsoft.Sql/servers/databases/dataWarehouseQueries/dataWarehouseQuerySteps/read | Returns the distributed query step information of data warehouse query for selected step ID |
> | Eylem | Microsoft.Sql/servers/databases/dataWarehouseQueries/read | Returns the data warehouse distribution query information for selected query ID |
> | Eylem | Microsoft.Sql/servers/databases/dataWarehouseUserActivities/read | Retrieves the user activities of a SQL Data Warehouse instance which includes running and suspended queries |
> | Eylem | Microsoft.Sql/servers/databases/delete | Deletes an existing database. |
> | Eylem | Microsoft.Sql/servers/databases/export/action | Export Azure SQL Database |
> | Eylem | Microsoft.Sql/servers/databases/extendedAuditingSettings/read | Retrieve details of the extended blob auditing policy configured on a given database |
> | Eylem | Microsoft.Sql/servers/databases/extendedAuditingSettings/write | Change the extended blob auditing policy for a given database |
> | Eylem | Microsoft.Sql/servers/databases/extensions/read | Gets a collection of extensions for the database. |
> | Eylem | Microsoft.Sql/servers/databases/extensions/write | Change the extension for a given database |
> | Eylem | Microsoft.Sql/servers/databases/failover/action | Customer initiated database failover. |
> | Eylem | Microsoft.Sql/servers/databases/geoBackupPolicies/read | Retrieve geo backup policies for a given database |
> | Eylem | Microsoft.Sql/servers/databases/geoBackupPolicies/write | Create or update a database geobackup policy |
> | Eylem | Microsoft.Sql/servers/databases/importExportOperationResults/read | Gets in-progress import/export operations |
> | Eylem | Microsoft.Sql/servers/databases/maintenanceWindowOptions/read | Gets a list of available maintenance windows for a selected database. |
> | Eylem | Microsoft.Sql/servers/databases/maintenanceWindows/read | Gets maintenance windows settings for a selected database. |
> | Eylem | Microsoft.Sql/servers/databases/maintenanceWindows/write | Sets maintenance windows settings for a selected database. |
> | Eylem | Microsoft.Sql/servers/databases/metricDefinitions/read | Return types of metrics that are available for databases |
> | Eylem | Microsoft.Sql/servers/databases/metrics/read | Return metrics for databases |
> | Eylem | Microsoft.Sql/servers/databases/move/action | Change the name of an existing database. |
> | Eylem | Microsoft.Sql/servers/databases/operationResults/read | Gets the status of a database operation. |
> | Eylem | Microsoft.Sql/servers/databases/operations/cancel/action | Cancels Azure SQL Database pending asynchronous operation that is not finished yet. |
> | Eylem | Microsoft.Sql/servers/databases/operations/read | Return the list of operations performed on the database |
> | Eylem | Microsoft.Sql/servers/databases/pause/action | Pause Azure SQL Datawarehouse Database |
> | Eylem | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/read | Gets the diagnostic setting for the resource |
> | Eylem | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/write | Creates or updates the diagnostic setting for the resource |
> | Eylem | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/logDefinitions/read | Gets the available logs for databases |
> | Eylem | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/metricDefinitions/read | Return types of metrics that are available for databases |
> | Eylem | Microsoft.Sql/servers/databases/queryStore/queryTexts/read | Returns the collection of query texts that correspond to the specified parameters. |
> | Eylem | Microsoft.Sql/servers/databases/queryStore/read | Returns current values of Query Store settings for the database. |
> | Eylem | Microsoft.Sql/servers/databases/queryStore/write | Updates Query Store setting for the database |
> | Eylem | Microsoft.Sql/servers/databases/read | Return the list of databases or gets the properties for the specified database. |
> | Eylem | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/read | List sensitivity labels of a given database |
> | Eylem | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/write | Batch update recommended sensitivity labels |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/delete | Terminate the replication relationship forcefully and with potential data loss |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/failover/action | Failover after synchronizing all changes from the primary, making this database into the replication relationship\u0027s primary and making the remote primary into a secondary |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/forceFailoverAllowDataLoss/action | Failover immediately with potential data loss, making this database into the replication relationship\u0027s primary and making the remote primary into a secondary |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/read | Return the list of replication links or gets the properties for the specified replication links. |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/unlink/action | Terminate the replication relationship forcefully or after synchronizing with the partner |
> | Eylem | Microsoft.Sql/servers/databases/replicationLinks/updateReplicationMode/action | Update replication mode for link to synchronous or asynchronous mode |
> | Eylem | Microsoft.Sql/servers/databases/restorePoints/action | Creates a new restore point |
> | Eylem | Microsoft.Sql/servers/databases/restorePoints/delete | Deletes a restore point for the database. |
> | Eylem | Microsoft.Sql/servers/databases/restorePoints/read | Returns restore points for the database. |
> | Eylem | Microsoft.Sql/servers/databases/resume/action | Resume Azure SQL Datawarehouse Database |
> | Eylem | Microsoft.Sql/servers/databases/schemas/read | Get a database schema (schema only). |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Get a database column (schema only). |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/delete | Delete the sensitivity label of a given column |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/disable/action | Disable sensitivity recommendations on a given column |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/enable/action | Enable sensitivity recommendations on a given column |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/read | Get the sensitivity label of a given column |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/write | Create or update the sensitivity label of a given column |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/read | Get a database table (schema only). |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/read | Retrieve list of index recommendations on a database |
> | Eylem | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/write | Apply index recommendation |
> | Eylem | Microsoft.Sql/servers/databases/securityAlertPolicies/read | Retrieve a list of database threat detection policies configured for a given server |
> | Eylem | Microsoft.Sql/servers/databases/securityAlertPolicies/write | Change the database threat detection policy for a given database |
> | Eylem | Microsoft.Sql/servers/databases/securityMetrics/read | Gets a collection of database security metrics |
> | Eylem | Microsoft.Sql/servers/databases/sensitivityLabels/read | List sensitivity labels of a given database |
> | Eylem | Microsoft.Sql/servers/databases/serviceTierAdvisors/read | Return suggestion about scaling database up or down based on query execution statistics to improve performance or reduce cost |
> | Eylem | Microsoft.Sql/servers/databases/skus/read | Gets a collection of skus available for a database |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/cancelSync/action | Cancel sync group synchronization |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/delete | Deletes an existing sync group. |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/hubSchemas/read | Return the list of sync hub database schemas |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/logs/read | Return the list of sync group logs |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/read | Return the list of sync groups or gets the properties for the specified sync group. |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchema/action | Refresh sync hub database schema |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchemaOperationResults/read | Retrieve result of the sync hub schema refresh operation |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/delete | Deletes an existing sync member. |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/read | Return the list of sync members or gets the properties for the specified sync member. |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchema/action | Refresh sync member schema |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchemaOperationResults/read | Retrieve result of the sync member schema refresh operation |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/schemas/read | Return the list of sync member database schemas |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/syncMembers/write | Creates a sync member with the specified parameters or update the properties for the specified sync member. |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/triggerSync/action | Trigger sync group synchronization |
> | Eylem | Microsoft.Sql/servers/databases/syncGroups/write | Creates a sync group with the specified parameters or update the properties for the specified sync group. |
> | Eylem | Microsoft.Sql/servers/databases/topQueries/queryText/action | Returns the Transact-SQL text for selected query ID |
> | Eylem | Microsoft.Sql/servers/databases/topQueries/read | Returns aggregated runtime statistics for selected query in selected time period |
> | Eylem | Microsoft.Sql/servers/databases/topQueries/statistics/read | Returns aggregated runtime statistics for selected query in selected time period |
> | Eylem | Microsoft.Sql/servers/databases/transparentDataEncryption/operationResults/read | Gets in-progress operations on transparent data encryption |
> | Eylem | Microsoft.Sql/servers/databases/transparentDataEncryption/read | Retrieve status and details of transparent data encryption security feature for a given database |
> | Eylem | Microsoft.Sql/servers/databases/transparentDataEncryption/write | Change transparent data encryption state |
> | Eylem | Microsoft.Sql/servers/databases/upgradeDataWarehouse/action | Upgrade Azure SQL Datawarehouse Database |
> | Eylem | Microsoft.Sql/servers/databases/usages/read | Gets the Azure SQL Database usages information |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/delete | Remove the vulnerability assessment for a given database |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/read | Retrieve the vulnerability assessment policies on a givendatabase |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/delete | Remove the vulnerability assessment rule baseline for a given database |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/read | Get the vulnerability assessment rule baseline for a given database |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/write | Change the vulnerability assessment rule baseline for a given database |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/export/action | Convert an existing scan result to a human readable format. If already exists nothing happens |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/initiateScan/action | Execute vulnerability assessment database scan. |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/read | Return the list of database vulnerability assessment scan records or get the scan record for the specified scan ID. |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessments/write | Change the vulnerability assessment for a given database |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/action | Execute vulnerability assessment database scan. |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/operationResults/read | Retrieve the result of the database vulnerability assessment scan Execute operation |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/read | Retrieve details of the vulnerability assessment configured on a given database |
> | Eylem | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/write | Change the vulnerability assessment for a given database |
> | Eylem | Microsoft.Sql/servers/databases/workloadGroups/delete | Drops a specific workload group. |
> | Eylem | Microsoft.Sql/servers/databases/workloadGroups/read | Lists the workload groups for a selected database. |
> | Eylem | Microsoft.Sql/servers/databases/workloadGroups/workloadClassifiers/delete | Drops a specific workload classifier. |
> | Eylem | Microsoft.Sql/servers/databases/workloadGroups/workloadClassifiers/read | Lists the workload classifiers for a selected database. |
> | Eylem | Microsoft.Sql/servers/databases/workloadGroups/workloadClassifiers/write | Sets the properties for a specific workload classifier. |
> | Eylem | Microsoft.Sql/servers/databases/workloadGroups/write | Sets the properties for a specific workload group. |
> | Eylem | Microsoft.Sql/servers/databases/write | Creates a database with the specified parameters or update the properties or tags for the specified database. |
> | Eylem | Microsoft.Sql/servers/delete | Deletes an existing server. |
> | Eylem | Microsoft.Sql/servers/disasterRecoveryConfiguration/delete | Deletes an existing disaster recovery configurations for a given server |
> | Eylem | Microsoft.Sql/servers/disasterRecoveryConfiguration/failover/action | Failover a DisasterRecoveryConfiguration |
> | Eylem | Microsoft.Sql/servers/disasterRecoveryConfiguration/forceFailoverAllowDataLoss/action | Force Failover a DisasterRecoveryConfiguration |
> | Eylem | Microsoft.Sql/servers/disasterRecoveryConfiguration/read | Gets a collection of disaster recovery configurations that include this server |
> | Eylem | Microsoft.Sql/servers/disasterRecoveryConfiguration/write | Change server disaster recovery configuration |
> | Eylem | Microsoft.Sql/servers/elasticPoolEstimates/read | Returns list of elastic pool estimates already created for this server |
> | Eylem | Microsoft.Sql/servers/elasticPoolEstimates/write | Creates new elastic pool estimate for list of databases provided |
> | Eylem | Microsoft.Sql/servers/elasticPools/advisors/read | Returns list of advisors available for the elastic pool |
> | Eylem | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/read | Returns list of recommended actions of specified advisor for the elastic pool |
> | Eylem | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/write | Apply the recommended action on the elastic pool |
> | Eylem | Microsoft.Sql/servers/elasticPools/advisors/write | Update auto-execute status of an advisor on elastic pool level. |
> | Eylem | Microsoft.Sql/servers/elasticPools/databases/read | Gets a list of databases for an elastic pool |
> | Eylem | Microsoft.Sql/servers/elasticPools/delete | Delete existing elastic pool |
> | Eylem | Microsoft.Sql/servers/elasticPools/elasticPoolActivity/read | Retrieve activities and details on a given elastic database pool |
> | Eylem | Microsoft.Sql/servers/elasticPools/elasticPoolDatabaseActivity/read | Retrieve activities and details on a given database that is part of elastic database pool |
> | Eylem | Microsoft.Sql/servers/elasticPools/failover/action | Customer initiated elastic pool failover. |
> | Eylem | Microsoft.Sql/servers/elasticPools/metricDefinitions/read | Return types of metrics that are available for elastic database pools |
> | Eylem | Microsoft.Sql/servers/elasticPools/metrics/read | Return metrics for elastic database pools |
> | Eylem | Microsoft.Sql/servers/elasticPools/operations/cancel/action | Cancels Azure SQL elastic pool pending asynchronous operation that is not finished yet. |
> | Eylem | Microsoft.Sql/servers/elasticPools/operations/read | Return the list of operations performed on the elastic pool |
> | Eylem | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/read | Gets the diagnostic setting for the resource |
> | Eylem | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/write | Creates or updates the diagnostic setting for the resource |
> | Eylem | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/metricDefinitions/read | Return types of metrics that are available for elastic database pools |
> | Eylem | Microsoft.Sql/servers/elasticPools/read | Retrieve details of elastic pool on a given server |
> | Eylem | Microsoft.Sql/servers/elasticPools/skus/read | Gets a collection of skus available for an elastic pool |
> | Eylem | Microsoft.Sql/servers/elasticPools/write | Create a new or change properties of existing elastic pool |
> | Eylem | Microsoft.Sql/servers/encryptionProtector/read | Returns a list of server encryption protectors or gets the properties for the specified server encryption protector. |
> | Eylem | Microsoft.Sql/servers/encryptionProtector/revalidate/action | Update the properties for the specified Server Encryption Protector. |
> | Eylem | Microsoft.Sql/servers/encryptionProtector/write | Update the properties for the specified Server Encryption Protector. |
> | Eylem | Microsoft.Sql/servers/extendedAuditingSettings/read | Retrieve details of the extended server blob auditing policy configured on a given server |
> | Eylem | Microsoft.Sql/servers/extendedAuditingSettings/write | Change the extended server blob auditing for a given server |
> | Eylem | Microsoft.Sql/servers/failoverGroups/delete | Deletes an existing failover group. |
> | Eylem | Microsoft.Sql/servers/failoverGroups/failover/action | Executes planned failover in an existing failover group. |
> | Eylem | Microsoft.Sql/servers/failoverGroups/forceFailoverAllowDataLoss/action | Executes forced failover in an existing failover group. |
> | Eylem | Microsoft.Sql/servers/failoverGroups/read | Returns the list of failover groups or gets the properties for the specified failover group. |
> | Eylem | Microsoft.Sql/servers/failoverGroups/write | Creates a failover group with the specified parameters or updates the properties or tags for the specified failover group. |
> | Eylem | Microsoft.Sql/servers/firewallRules/delete | Deletes an existing server firewall rule. |
> | Eylem | Microsoft.Sql/servers/firewallRules/read | Return the list of server firewall rules or gets the properties for the specified server firewall rule. |
> | Eylem | Microsoft.Sql/servers/firewallRules/write | Creates a server firewall rule with the specified parameters, update the properties for the specified rule or overwrite all existing rules with new server firewall rule(s). |
> | Eylem | Microsoft.Sql/servers/import/action | Create a new database on the server and deploy schema and data from a DacPac package |
> | Eylem | Microsoft.Sql/servers/importExportOperationResults/read | Gets in-progress import/export operations |
> | Eylem | Microsoft.Sql/servers/inaccessibleDatabases/read | Return a list of inaccessible database(s) in a logical server. |
> | Eylem | Microsoft.Sql/servers/interfaceEndpointProfiles/delete | Deletes the specified interface endpoint profile |
> | Eylem | Microsoft.Sql/servers/interfaceEndpointProfiles/read | Returns the properties for the specified interface endpoint profile |
> | Eylem | Microsoft.Sql/servers/interfaceEndpointProfiles/write | Creates a interface endpoint profile with the specified parameters or updates the properties or tags for the specified interface endpoint |
> | Eylem | Microsoft.Sql/servers/jobAgents/delete | Deletes an Azure SQL DB job agent |
> | Eylem | Microsoft.Sql/servers/jobAgents/read | Gets an Azure SQL DB job agent |
> | Eylem | Microsoft.Sql/servers/jobAgents/write | Creates or updates an Azure SQL DB job agent |
> | Eylem | Microsoft.Sql/servers/keys/delete | Deletes an existing server key. |
> | Eylem | Microsoft.Sql/servers/keys/read | Return the list of server keys or gets the properties for the specified server key. |
> | Eylem | Microsoft.Sql/servers/keys/write | Creates a key with the specified parameters or update the properties or tags for the specified server key. |
> | Eylem | Microsoft.Sql/servers/operationResults/read | Gets in-progress server operations |
> | Eylem | Microsoft.Sql/servers/operations/read | Return the list of operations performed on the server |
> | Eylem | Microsoft.Sql/servers/privateEndpointConnectionProxies/delete | Deletes an existing private endpoint connection proxy |
> | Eylem | Microsoft.Sql/servers/privateEndpointConnectionProxies/read | Returns the list of private endpoint connection proxies or gets the properties for the specified private endpoint connection proxy. |
> | Eylem | Microsoft.Sql/servers/privateEndpointConnectionProxies/validate/action | Validates a private endpoint connection create call from NRP side |
> | Eylem | Microsoft.Sql/servers/privateEndpointConnectionProxies/write | Creates a private endpoint connection proxy with the specified parameters or updates the properties or tags for the specified private endpoint connection proxy. |
> | Eylem | Microsoft.Sql/servers/privateEndpointConnections/delete | Deletes an existing private endpoint connection |
> | Eylem | Microsoft.Sql/servers/privateEndpointConnections/read | Returns the list of private endpoint connections or gets the properties for the specified private endpoint connection. |
> | Eylem | Microsoft.Sql/servers/privateEndpointConnections/write | Approves or rejects an existing private endpoint connection |
> | Eylem | Microsoft.Sql/servers/privateEndpointConnectionsApproval/action | Determines if user is allowed to approve a private endpoint connection |
> | Eylem | Microsoft.Sql/servers/privateLinkResources/read | Get the private link resources for the corresponding sql server |
> | Eylem | Microsoft.Sql/servers/providers/Microsoft.Insights/metricDefinitions/read | Return types of metrics that are available for servers |
> | Eylem | Microsoft.Sql/servers/read | Return the list of servers or gets the properties for the specified server. |
> | Eylem | Microsoft.Sql/servers/recommendedElasticPools/databases/read | Retrieve metrics for recommended elastic database pools for a given server |
> | Eylem | Microsoft.Sql/servers/recommendedElasticPools/read | Retrieve recommendation for elastic database pools to reduce cost or improve performance based on historical resource utilization |
> | Eylem | Microsoft.Sql/servers/recoverableDatabases/read | This operation is used for disaster recovery of live database to restore database to last-known good backup point. It returns information about the last good backup but it doesn\u0027t actually restore the database. |
> | Eylem | Microsoft.Sql/servers/replicationLinks/read | Return the list of replication links or gets the properties for the specified replication links. |
> | Eylem | Microsoft.Sql/servers/restorableDroppedDatabases/read | Get a list of databases that were dropped on a given server that are still within retention policy. |
> | Eylem | Microsoft.Sql/servers/securityAlertPolicies/operationResults/read | Retrieve results of the server threat detection policy write operation |
> | Eylem | Microsoft.Sql/servers/securityAlertPolicies/read | Retrieve a list of server threat detection policies configured for a given server |
> | Eylem | Microsoft.Sql/servers/securityAlertPolicies/write | Change the server threat detection policy for a given server |
> | Eylem | Microsoft.Sql/servers/serviceObjectives/read | Retrieve list of service level objectives (also known as performance tiers) available on a given server |
> | Eylem | Microsoft.Sql/servers/syncAgents/delete | Deletes an existing sync agent. |
> | Eylem | Microsoft.Sql/servers/syncAgents/generateKey/action | Generate sync agent registration key |
> | Eylem | Microsoft.Sql/servers/syncAgents/linkedDatabases/read | Return the list of sync agent linked databases |
> | Eylem | Microsoft.Sql/servers/syncAgents/read | Return the list of sync agents or gets the properties for the specified sync agent. |
> | Eylem | Microsoft.Sql/servers/syncAgents/write | Creates a sync agent with the specified parameters or update the properties for the specified sync agent. |
> | Eylem | Microsoft.Sql/servers/tdeCertificates/action | Create/Update TDE certificate |
> | Eylem | Microsoft.Sql/servers/usages/read | Return server DTU quota and current DTU consumption by all databases within the server |
> | Eylem | Microsoft.Sql/servers/virtualNetworkRules/delete | Deletes an existing Virtual Network Rule |
> | Eylem | Microsoft.Sql/servers/virtualNetworkRules/read | Return the list of virtual network rules or gets the properties for the specified virtual network rule. |
> | Eylem | Microsoft.Sql/servers/virtualNetworkRules/write | Creates a virtual network rule with the specified parameters or update the properties or tags for the specified virtual network rule. |
> | Eylem | Microsoft.Sql/servers/vulnerabilityAssessments/delete | Remove the vulnerability assessment for a given server |
> | Eylem | Microsoft.Sql/servers/vulnerabilityAssessments/read | Retrieve the vulnerability assessment policies on a given server |
> | Eylem | Microsoft.Sql/servers/vulnerabilityAssessments/write | Change the vulnerability assessment for a given server |
> | Eylem | Microsoft.Sql/servers/write | Creates a server with the specified parameters or update the properties or tags for the specified server. |
> | Eylem | Microsoft.Sql/unregister/action | UnRegisters the subscription for the Microsoft SQL Database resource provider and enables the creation of Microsoft SQL Databases. |
> | Eylem | Microsoft.Sql/virtualClusters/delete | Deletes an existing virtual cluster. |
> | Eylem | Microsoft.Sql/virtualClusters/read | Return the list of virtual clusters or gets the properties for the specified virtual cluster. |
> | Eylem | Microsoft.Sql/virtualClusters/write | Updates virtual cluster tags. |

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.SqlVirtualMachine/locations/availabilityGroupListenerOperationResults/read | Get result of an availability group listener operation |
> | Eylem | Microsoft.SqlVirtualMachine/locations/sqlVirtualMachineGroupOperationResults/read | Get result of a SQL virtual machine group operation |
> | Eylem | Microsoft.SqlVirtualMachine/locations/sqlVirtualMachineOperationResults/read | Get result of SQL virtual machine operation |
> | Eylem | Microsoft.SqlVirtualMachine/operations/read |  |
> | Eylem | Microsoft.SqlVirtualMachine/register/action | Register subscription with Microsoft.SqlVirtualMachine resource provider |
> | Eylem | Microsoft.SqlVirtualMachine/sqlVirtualMachineGroups/availabilityGroupListeners/delete | Delete existing availability group listener |
> | Eylem | Microsoft.SqlVirtualMachine/sqlVirtualMachineGroups/availabilityGroupListeners/read | Retrieve details of SQL availability group listener on a given SQL virtual machine group |
> | Eylem | Microsoft.SqlVirtualMachine/sqlVirtualMachineGroups/availabilityGroupListeners/write | Create a new or changes properties of existing SQL availability group listener |
> | Eylem | Microsoft.SqlVirtualMachine/sqlVirtualMachineGroups/delete | Delete existing SQL virtual machine group |
> | Eylem | Microsoft.SqlVirtualMachine/sqlVirtualMachineGroups/read | Retrive details of SQL virtual machine group |
> | Eylem | Microsoft.SqlVirtualMachine/sqlVirtualMachineGroups/sqlVirtualMachines/read | List Sql virtual machines by a particular sql virtual virtual machine group |
> | Eylem | Microsoft.SqlVirtualMachine/sqlVirtualMachineGroups/write | Create a new or change properties of existing SQL virtual machine group |
> | Eylem | Microsoft.SqlVirtualMachine/sqlVirtualMachines/delete | Delete existing SQL virtual machine |
> | Eylem | Microsoft.SqlVirtualMachine/sqlVirtualMachines/read | Retrieve details of SQL virtual machine |
> | Eylem | Microsoft.SqlVirtualMachine/sqlVirtualMachines/write | Create a new or change properties of existing SQL virtual machine |
> | Eylem | Microsoft.SqlVirtualMachine/unregister/action | Unregister subscription with Microsoft.SqlVirtualMachine resource provider |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Storage/checknameavailability/read | Checks that account name is valid and is not in use. |
> | Eylem | Microsoft.Storage/locations/checknameavailability/read | Checks that account name is valid and is not in use. |
> | Eylem | Microsoft.Storage/locations/deleteVirtualNetworkOrSubnets/action | Notifies Microsoft.Storage that virtual network or subnet is being deleted |
> | Eylem | Microsoft.Storage/locations/usages/read | Returns the limit and the current usage count for resources in the specified subscription |
> | Eylem | Microsoft.Storage/operations/read | Polls the status of an asynchronous operation. |
> | Eylem | Microsoft.Storage/register/action | Registers the subscription for the storage resource provider and enables the creation of storage accounts. |
> | Eylem | Microsoft.Storage/skus/read | Lists the Skus supported by Microsoft.Storage. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/add/action | Returns the result of adding blob content |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Returns the result of deleting a blob |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/deleteBlobVersion/action | Returns the result of deleting a blob version |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/filter/action | Returns the list of blobs under an account with matching tags filter |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Returns a blob or a list of blobs |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/runAsSuperUser/action | Returns the result of the blob command |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags/read | Returns the result of reading blob tags |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/tags/write | Returns the result of writing blob tags |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Returns the result of writing a blob |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/clearLegalHold/action | Clear blob container legal hold |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Returns the result of deleting a container |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/delete | Delete blob container immutability policy |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/extend/action | Extend blob container immutability policy |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/lock/action | Lock blob container immutability policy |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/read | Get blob container immutability policy |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/write | Put blob container immutability policy |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/lease/action | Returns the result of leasing blob container |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/read | Returns a container |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/read | Returns list of containers |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/setLegalHold/action | Set blob container legal hold |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/write | Returns the result of patch blob container |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/containers/write | Returns the result of put blob container |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Returns a user delegation key for the blob service |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/read |  |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/read | Returns blob service properties or statistics |
> | Eylem | Microsoft.Storage/storageAccounts/blobServices/write | Returns the result of put blob service properties |
> | Eylem | Microsoft.Storage/storageAccounts/delete | Deletes an existing storage account. |
> | Eylem | Microsoft.Storage/storageAccounts/encryptionScopes/read |  |
> | Eylem | Microsoft.Storage/storageAccounts/encryptionScopes/read |  |
> | Eylem | Microsoft.Storage/storageAccounts/encryptionScopes/write |  |
> | Eylem | Microsoft.Storage/storageAccounts/encryptionScopes/write |  |
> | Eylem | Microsoft.Storage/storageAccounts/failover/action | Customer is able to control the failover in case of availability issues |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/actassuperuser/action | Get File Admin Privileges |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete | Returns the result of deleting a file/folder |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/modifypermissions/action | Returns the result of modifying permission on a file/folder |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read | Returns a file/folder or a list of files/folders |
> | DataAction | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write | Returns the result of writing a file or creating a folder |
> | Eylem | Microsoft.Storage/storageAccounts/fileServices/read |  |
> | Eylem | Microsoft.Storage/storageAccounts/fileServices/read | Get file service properties |
> | Eylem | Microsoft.Storage/storageAccounts/fileServices/shares/delete |  |
> | Eylem | Microsoft.Storage/storageAccounts/fileServices/shares/read |  |
> | Eylem | Microsoft.Storage/storageAccounts/fileServices/shares/read |  |
> | Eylem | Microsoft.Storage/storageAccounts/fileServices/shares/write |  |
> | Eylem | Microsoft.Storage/storageAccounts/fileServices/write |  |
> | Eylem | Microsoft.Storage/storageAccounts/listAccountSas/action | Returns the Account SAS token for the specified storage account. |
> | Eylem | Microsoft.Storage/storageAccounts/listkeys/action | Returns the access keys for the specified storage account. |
> | Eylem | Microsoft.Storage/storageAccounts/listServiceSas/action | Returns the Service SAS token for the specified storage account. |
> | Eylem | Microsoft.Storage/storageAccounts/managementPolicies/delete | Delete storage account management policies |
> | Eylem | Microsoft.Storage/storageAccounts/managementPolicies/read | Get storage management account policies |
> | Eylem | Microsoft.Storage/storageAccounts/managementPolicies/write | Put storage account management policies |
> | Eylem | Microsoft.Storage/storageAccounts/objectReplicationPolicies/delete |  |
> | Eylem | Microsoft.Storage/storageAccounts/objectReplicationPolicies/read |  |
> | Eylem | Microsoft.Storage/storageAccounts/objectReplicationPolicies/write |  |
> | Eylem | Microsoft.Storage/storageAccounts/privateEndpointConnectionProxies/delete | Delete Private Endpoint Connection Proxies |
> | Eylem | Microsoft.Storage/storageAccounts/privateEndpointConnectionProxies/read | Get Private Endpoint Connection Proxy |
> | Eylem | Microsoft.Storage/storageAccounts/privateEndpointConnectionProxies/write | Put Private Endpoint Connection Proxies |
> | Eylem | Microsoft.Storage/storageAccounts/privateEndpointConnections/delete | Delete Private Endpoint Connection |
> | Eylem | Microsoft.Storage/storageAccounts/privateEndpointConnections/read | Get Private Endpoint Connection |
> | Eylem | Microsoft.Storage/storageAccounts/privateEndpointConnections/write | Put Private Endpoint Connection |
> | Eylem | Microsoft.Storage/storageAccounts/PrivateEndpointConnectionsApproval/action | Approve Private Endpoint Connections |
> | Eylem | Microsoft.Storage/storageAccounts/privateLinkResources/read | Get StorageAccount groupids |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Returns the result of deleting a queue |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action | Returns the result of adding a message |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | Returns the result of deleting a message |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action | Returns the result of processing a message |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Returns a message |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | Returns the result of writing a message |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/queues/read | Returns a queue or a list of queues. |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/queues/write | Returns the result of writing a queue |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/read | Get Queue service properties |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/read | Returns queue service properties or statistics. |
> | Eylem | Microsoft.Storage/storageAccounts/queueServices/write | Returns the result of setting queue service properties |
> | Eylem | Microsoft.Storage/storageAccounts/read | Returns the list of storage accounts or gets the properties for the specified storage account. |
> | Eylem | Microsoft.Storage/storageAccounts/regeneratekey/action | Regenerates the access keys for the specified storage account. |
> | Eylem | Microsoft.Storage/storageAccounts/restoreBlobRanges/action | Restore blob ranges to the state of the specified time |
> | Eylem | Microsoft.Storage/storageAccounts/revokeUserDelegationKeys/action | Revokes all the user delegation keys for the specified storage account. |
> | Eylem | Microsoft.Storage/storageAccounts/services/diagnosticSettings/write | Create/Update storage account diagnostic settings. |
> | Eylem | Microsoft.Storage/storageAccounts/tableServices/read | Get Table service properties |
> | Eylem | Microsoft.Storage/storageAccounts/write | Creates a storage account with the specified parameters or update the properties or tags or adds custom domain for the specified storage account. |
> | Eylem | Microsoft.Storage/usages/read | Returns the limit and the current usage count for resources in the specified subscription |

## <a name="microsoftstoragesync"></a>microsoft.storagesync

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | microsoft.storagesync/locations/checkNameAvailability/action | Checks that storage sync service name is valid and is not in use. |
> | Eylem | microsoft.storagesync/locations/workflows/operations/read | Gets the status of an asynchronous operation |
> | Eylem | microsoft.storagesync/operations/read | Gets a list of the Supported Operations |
> | Eylem | microsoft.storagesync/register/action | Registers the subscription for the Storage Sync Provider |
> | Eylem | microsoft.storagesync/storageSyncServices/delete | Delete any Storage Sync Services |
> | Eylem | microsoft.storagesync/storageSyncServices/providers/Microsoft.Insights/metricDefinitions/read | Gets the available metrics for Storage Sync Services |
> | Eylem | microsoft.storagesync/storageSyncServices/read | Read any Storage Sync Services |
> | Eylem | microsoft.storagesync/storageSyncServices/registeredServers/delete | Delete any Registered Server |
> | Eylem | microsoft.storagesync/storageSyncServices/registeredServers/providers/Microsoft.Insights/metricDefinitions/read | Gets the available metrics for Registered Server |
> | Eylem | microsoft.storagesync/storageSyncServices/registeredServers/read | Read any Registered Server |
> | Eylem | microsoft.storagesync/storageSyncServices/registeredServers/write | Create or Update any Registered Server |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/delete | Delete any Cloud Endpoints |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/operationresults/read | Gets the status of an asynchronous backup/restore operation |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postbackup/action | Call this action after backup |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postrestore/action | Call this action after restore |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prebackup/action | Call this action before backup |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prerestore/action | Call this action before restore |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/read | Read any Cloud Endpoints |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/restoreheartbeat/action | Restore heartbeat |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/triggerChangeDetection/action | Call this action to trigger detection of changes on a cloud endpoint's file share |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/write | Create or Update any Cloud Endpoints |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/delete | Delete any Sync Groups |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/providers/Microsoft.Insights/metricDefinitions/read | Gets the available metrics for Sync Groups |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/read | Read any Sync Groups |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/delete | Delete any Server Endpoints |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/providers/Microsoft.Insights/metricDefinitions/read | Gets the available metrics for Server Endpoints |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/read | Read any Server Endpoints |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/recallAction/action | Call this action to recall files to a server |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/write | Create or Update any Server Endpoints |
> | Eylem | microsoft.storagesync/storageSyncServices/syncGroups/write | Create or Update any Sync Groups |
> | Eylem | microsoft.storagesync/storageSyncServices/workflows/operationresults/read | Gets the status of an asynchronous operation |
> | Eylem | microsoft.storagesync/storageSyncServices/workflows/operations/read | Gets the status of an asynchronous operation |
> | Eylem | microsoft.storagesync/storageSyncServices/workflows/read | Read Workflows |
> | Eylem | microsoft.storagesync/storageSyncServices/write | Create or Update any Storage Sync Services |
> | Eylem | microsoft.storagesync/unregister/action | Unregisters the subscription for the Storage Sync Provider |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.StorSimple/managers/accessControlRecords/delete | Deletes the Access Control Records |
> | Eylem | Microsoft.StorSimple/managers/accessControlRecords/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/accessControlRecords/read | Lists or gets the Access Control Records |
> | Eylem | Microsoft.StorSimple/managers/accessControlRecords/write | Create or update the Access Control Records |
> | Eylem | Microsoft.StorSimple/managers/alerts/read | Lists or gets the Alerts |
> | Eylem | Microsoft.StorSimple/managers/backups/read | Lists or gets the Backup Set |
> | Eylem | Microsoft.StorSimple/managers/bandwidthSettings/delete | Deletes an existing Bandwidth Settings (8000 Series Only) |
> | Eylem | Microsoft.StorSimple/managers/bandwidthSettings/operationResults/read | List the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/bandwidthSettings/read | List the Bandwidth Settings (8000 Series Only) |
> | Eylem | Microsoft.StorSimple/managers/bandwidthSettings/write | Creates a new or updates Bandwidth Settings (8000 Series Only) |
> | Eylem | Microsoft.StorSimple/managers/certificates/write | Create or update the Certificates |
> | Eylem | Microsoft.StorSimple/Managers/certificates/write | The Update Resource Certificate operation updates the resource/vault credential certificate. |
> | Eylem | Microsoft.StorSimple/managers/clearAlerts/action | Clear all the alerts associated with the device manager. |
> | Eylem | Microsoft.StorSimple/managers/cloudApplianceConfigurations/read | List the Cloud Appliance Supported Configurations |
> | Eylem | Microsoft.StorSimple/managers/configureDevice/action | Configures a device |
> | Eylem | Microsoft.StorSimple/managers/delete | Deletes the Device Managers |
> | Eylem | Microsoft.StorSimple/Managers/delete | The Delete Vault operation deletes the specified Azure resource of type 'vault' |
> | Eylem | Microsoft.StorSimple/managers/devices/alertSettings/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/alertSettings/read | Lists or gets the Alert Settings |
> | Eylem | Microsoft.StorSimple/managers/devices/alertSettings/write | Create or update the Alert Settings |
> | Eylem | Microsoft.StorSimple/managers/devices/authorizeForServiceEncryptionKeyRollover/action | Authorize for Service Encryption Key Rollover of Devices |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/backup/action | Take a manual backup to create an on-demand backup of all the volumes protected by the policy. |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/delete | Deletes an existing Backup Polices (8000 Series Only) |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/operationResults/read | List the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/read | List the Backup Polices (8000 Series Only) |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/delete | Deletes an existing Schedules |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/operationResults/read | List the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/read | List the Schedules |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/write | Creates a new or updates Schedules |
> | Eylem | Microsoft.StorSimple/managers/devices/backupPolicies/write | Creates a new or updates Backup Polices (8000 Series Only) |
> | Eylem | Microsoft.StorSimple/managers/devices/backups/delete | Deletes the Backup Set |
> | Eylem | Microsoft.StorSimple/managers/devices/backups/elements/clone/action | Clone a share or volume using a backup element. |
> | Eylem | Microsoft.StorSimple/managers/devices/backups/elements/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/backups/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/backups/read | Lists or gets the Backup Set |
> | Eylem | Microsoft.StorSimple/managers/devices/backups/restore/action | Restore all the volumes from a backup set. |
> | Eylem | Microsoft.StorSimple/managers/devices/backupScheduleGroups/delete | Deletes the Backup Schedule Groups |
> | Eylem | Microsoft.StorSimple/managers/devices/backupScheduleGroups/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/backupScheduleGroups/read | Lists or gets the Backup Schedule Groups |
> | Eylem | Microsoft.StorSimple/managers/devices/backupScheduleGroups/write | Create or update the Backup Schedule Groups |
> | Eylem | Microsoft.StorSimple/managers/devices/chapSettings/delete | Deletes the Chap Settings |
> | Eylem | Microsoft.StorSimple/managers/devices/chapSettings/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/chapSettings/read | Lists or gets the Chap Settings |
> | Eylem | Microsoft.StorSimple/managers/devices/chapSettings/write | Create or update the Chap Settings |
> | Eylem | Microsoft.StorSimple/managers/devices/deactivate/action | Deactivates a device. |
> | Eylem | Microsoft.StorSimple/managers/devices/delete | Deletes the Devices |
> | Eylem | Microsoft.StorSimple/managers/devices/disks/read | Lists or gets the Disks |
> | Eylem | Microsoft.StorSimple/managers/devices/download/action | Download updates for a device. |
> | Eylem | Microsoft.StorSimple/managers/devices/failover/action | Failover of the device. |
> | Eylem | Microsoft.StorSimple/managers/devices/failover/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/failoverTargets/read | Lists or gets the Failover targets of the devices |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/backup/action | Take backup of an File Server. |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/delete | Deletes the File Servers |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/metrics/read | Lists or gets the Metrics |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/metricsDefinitions/read | Lists or gets the Metrics Definitions |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/read | Lists or gets the File Servers |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/delete | Deletes the Shares |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/metrics/read | Lists or gets the Metrics |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/metricsDefinitions/read | Lists or gets the Metrics Definitions |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/read | Lists or gets the Shares |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/shares/write | Create or update the Shares |
> | Eylem | Microsoft.StorSimple/managers/devices/fileservers/write | Create or update the File Servers |
> | Eylem | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/changeControllerPowerState/action | Change controller power state of hardware component groups |
> | Eylem | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/operationResults/read | List the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/read | List the Hardware Component Groups |
> | Eylem | Microsoft.StorSimple/managers/devices/install/action | Install updates on a device. |
> | Eylem | Microsoft.StorSimple/managers/devices/installUpdates/action | Installs updates on the devices (8000 Series Only). |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/backup/action | Take backup of an iSCSI server. |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/delete | Deletes the iSCSI Servers |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/delete | Deletes the Disks |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metrics/read | Lists or gets the Metrics |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metricsDefinitions/read | Lists or gets the Metrics Definitions |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/read | Lists or gets the Disks |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/disks/write | Create or update the Disks |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/metrics/read | Lists or gets the Metrics |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/metricsDefinitions/read | Lists or gets the Metrics Definitions |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/read | Lists or gets the iSCSI Servers |
> | Eylem | Microsoft.StorSimple/managers/devices/iscsiservers/write | Create or update the iSCSI Servers |
> | Eylem | Microsoft.StorSimple/managers/devices/jobs/cancel/action | Cancel a running job |
> | Eylem | Microsoft.StorSimple/managers/devices/jobs/operationResults/read | List the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/jobs/read | Lists or gets the Jobs |
> | Eylem | Microsoft.StorSimple/managers/devices/listFailoverSets/action | List the failover sets for an existing device (8000 Series Only). |
> | Eylem | Microsoft.StorSimple/managers/devices/listFailoverTargets/action | List failover targets of the devices (8000 Series Only). |
> | Eylem | Microsoft.StorSimple/managers/devices/metrics/read | Lists or gets the Metrics |
> | Eylem | Microsoft.StorSimple/managers/devices/metricsDefinitions/read | Lists or gets the Metrics Definitions |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigration/action | Confirms a successful migration and commit it. |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigrationStatus/read | List the Confirm Migration Status |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchConfirmMigrationStatus/action | Fetch the confirm status of migration. |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationEstimate/action | Fetch the status for the migration estimation job. |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationStatus/action | Fetch the status for the migration. |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/import/action | Import source configurations for migration |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/migrationEstimate/read | List the Migration Estimate |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/migrationStatus/read | List the Migration Status |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/operationResults/read | List the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigration/action | Start migration using source configurations |
> | Eylem | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigrationEstimate/action | Start a job to estimate the duration of the migration process. |
> | Eylem | Microsoft.StorSimple/managers/devices/networkSettings/operationResults/read | List the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/networkSettings/read | Lists or gets the Network Settings |
> | Eylem | Microsoft.StorSimple/managers/devices/networkSettings/write | Creates a new or updates Network Settings |
> | Eylem | Microsoft.StorSimple/managers/devices/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/publicEncryptionKey/action | List public encryption key of the device manager |
> | Eylem | Microsoft.StorSimple/managers/devices/publishSupportPackage/action | Publish the support package for an existing device. A StorSimple support package is an easy-to-use mechanism that collects all relevant logs to assist Microsoft Support with troubleshooting any StorSimple device issues. |
> | Eylem | Microsoft.StorSimple/managers/devices/read | Lists or gets the Devices |
> | Eylem | Microsoft.StorSimple/managers/devices/scanForUpdates/action | Scan for updates in a device. |
> | Eylem | Microsoft.StorSimple/managers/devices/securitySettings/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/securitySettings/read | List the Security Settings |
> | Eylem | Microsoft.StorSimple/managers/devices/securitySettings/syncRemoteManagementCertificate/action | Synchronize the remote management certificate for a device. |
> | Eylem | Microsoft.StorSimple/managers/devices/securitySettings/update/action | Update the security settings. |
> | Eylem | Microsoft.StorSimple/managers/devices/securitySettings/write | Creates a new or updates Security Settings |
> | Eylem | Microsoft.StorSimple/managers/devices/sendTestAlertEmail/action | Send test alert email to configured email recipients. |
> | Eylem | Microsoft.StorSimple/managers/devices/shares/read | Lists or gets the Shares |
> | Eylem | Microsoft.StorSimple/managers/devices/timeSettings/operationResults/read | List the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/timeSettings/read | Lists or gets the Time Settings |
> | Eylem | Microsoft.StorSimple/managers/devices/timeSettings/write | Creates a new or updates Time Settings |
> | Eylem | Microsoft.StorSimple/managers/devices/updates/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/updateSummary/read | Lists or gets the Update Summary |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/delete | Deletes an existing Volume Containers (8000 Series Only) |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/metrics/read | List the Metrics |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/metricsDefinitions/read | List the Metrics Definitions |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/operationResults/read | List the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/read | List the Volume Containers (8000 Series Only) |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/delete | Deletes an existing Volumes |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metrics/read | List the Metrics |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metricsDefinitions/read | List the Metrics Definitions |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/operationResults/read | List the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/read | List the Volumes |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/write | Creates a new or updates Volumes |
> | Eylem | Microsoft.StorSimple/managers/devices/volumeContainers/write | Creates a new or updates Volume Containers (8000 Series Only) |
> | Eylem | Microsoft.StorSimple/managers/devices/volumes/read | List the Volumes |
> | Eylem | Microsoft.StorSimple/managers/devices/write | Create or update the Devices |
> | Eylem | Microsoft.StorSimple/managers/encryptionSettings/read | Lists or gets the Encryption Settings |
> | Eylem | Microsoft.StorSimple/managers/extendedInformation/delete | Deletes the Extended Vault Information |
> | Eylem | Microsoft.StorSimple/Managers/extendedInformation/delete | The Get Extended Info operation gets an object's Extended Info representing the Azure resource of type ?vault? |
> | Eylem | Microsoft.StorSimple/managers/extendedInformation/read | Lists or gets the Extended Vault Information |
> | Eylem | Microsoft.StorSimple/Managers/extendedInformation/read | The Get Extended Info operation gets an object's Extended Info representing the Azure resource of type ?vault? |
> | Eylem | Microsoft.StorSimple/managers/extendedInformation/write | Create or update the Extended Vault Information |
> | Eylem | Microsoft.StorSimple/Managers/extendedInformation/write | The Get Extended Info operation gets an object's Extended Info representing the Azure resource of type ?vault? |
> | Eylem | Microsoft.StorSimple/managers/features/read | List the Features |
> | Eylem | Microsoft.StorSimple/managers/fileservers/read | Lists or gets the File Servers |
> | Eylem | Microsoft.StorSimple/managers/getEncryptionKey/action | Get encryption key for the device manager. |
> | Eylem | Microsoft.StorSimple/managers/iscsiservers/read | Lists or gets the iSCSI Servers |
> | Eylem | Microsoft.StorSimple/managers/jobs/read | Lists or gets the Jobs |
> | Eylem | Microsoft.StorSimple/managers/listActivationKey/action | Gets the activation key of the StorSimple Device Manager. |
> | Eylem | Microsoft.StorSimple/managers/listPublicEncryptionKey/action | List public encryption keys of a StorSimple Device Manager. |
> | Eylem | Microsoft.StorSimple/managers/metrics/read | Lists or gets the Metrics |
> | Eylem | Microsoft.StorSimple/managers/metricsDefinitions/read | Lists or gets the Metrics Definitions |
> | Eylem | Microsoft.StorSimple/managers/migrateClassicToResourceManager/action | Migrate from Classic To Resource Manager |
> | Eylem | Microsoft.StorSimple/managers/migrationSourceConfigurations/read | List the Migration Source Configurations (8000 Series Only) |
> | Eylem | Microsoft.StorSimple/managers/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/provisionCloudAppliance/action | Create a new cloud appliance. |
> | Eylem | Microsoft.StorSimple/managers/read | Lists or gets the Device Managers |
> | Eylem | Microsoft.StorSimple/Managers/read | The Get Vault operation gets an object representing the Azure resource of type 'vault' |
> | Eylem | Microsoft.StorSimple/managers/regenerateActivationKey/action | Regenerate the Activation key for an existing StorSimple Device Manager. |
> | Eylem | Microsoft.StorSimple/managers/storageAccountCredentials/delete | Deletes the Storage Account Credentials |
> | Eylem | Microsoft.StorSimple/managers/storageAccountCredentials/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/storageAccountCredentials/read | Lists or gets the Storage Account Credentials |
> | Eylem | Microsoft.StorSimple/managers/storageAccountCredentials/write | Create or update the Storage Account Credentials |
> | Eylem | Microsoft.StorSimple/managers/storageDomains/delete | Deletes the Storage Domains |
> | Eylem | Microsoft.StorSimple/managers/storageDomains/operationResults/read | Lists or gets the Operation Results |
> | Eylem | Microsoft.StorSimple/managers/storageDomains/read | Lists or gets the Storage Domains |
> | Eylem | Microsoft.StorSimple/managers/storageDomains/write | Create or update the Storage Domains |
> | Eylem | Microsoft.StorSimple/managers/write | Create or update the Device Managers |
> | Eylem | Microsoft.StorSimple/Managers/write | Create Vault operation creates an Azure resource of type 'vault' |
> | Eylem | Microsoft.StorSimple/operations/read | Lists or gets the Operations |
> | Eylem | Microsoft.StorSimple/register/action | Register Provider Microsoft.StorSimple |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.StreamAnalytics/locations/quotas/Read | Read Stream Analytics Subscription Quota |
> | Eylem | Microsoft.StreamAnalytics/operations/Read | Read Stream Analytics Operations |
> | Eylem | Microsoft.StreamAnalytics/Register/action | Register subscription with Stream Analytics Resource Provider |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/Delete | Delete Stream Analytics Job |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/Delete | Delete Stream Analytics Job Function |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/operationresults/Read | Read operation results for Stream Analytics Job Function |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/Read | Read Stream Analytics Job Function |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/RetrieveDefaultDefinition/action | Retrieve Default Definition of a Stream Analytics Job Function |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/Test/action | Test Stream Analytics Job Function |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/functions/Write | Write Stream Analytics Job Function |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/Delete | Delete Stream Analytics Job Input |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/operationresults/Read | Read operation results for Stream Analytics Job Input |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/Read | Read Stream Analytics Job Input |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/Sample/action | Sample Stream Analytics Job Input |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/Test/action | Test Stream Analytics Job Input |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/inputs/Write | Write Stream Analytics Job Input |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/metricdefinitions/Read | Read Metric Definitions |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/operationresults/Read | Read operation results for Stream Analytics Job |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/outputs/Delete | Delete Stream Analytics Job Output |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/outputs/operationresults/Read | Read operation results for Stream Analytics Job Output |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/outputs/Read | Read Stream Analytics Job Output |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/outputs/Test/action | Test Stream Analytics Job Output |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/outputs/Write | Write Stream Analytics Job Output |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/read | Read diagnostic setting. |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/write | Write diagnostic setting. |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/logDefinitions/read | Gets the available logs for streamingjobs |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/metricDefinitions/read | Gets the available metrics for streamingjobs |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/PublishEdgePackage/action | Publish edge package for Stream Analytics Job |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/Read | Read Stream Analytics Job |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/Scale/action | Scale Stream Analytics Job |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/Start/action | Stream Analytics İşini Başlat |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/Stop/action | Stop Stream Analytics Job |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/transformations/Delete | Delete Stream Analytics Job Transformation |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/transformations/Read | Read Stream Analytics Job Transformation |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/transformations/Write | Write Stream Analytics Job Transformation |
> | Eylem | Microsoft.StreamAnalytics/streamingjobs/Write | Write Stream Analytics Job |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Subscription/cancel/action | Cancels the Subscription |
> | Eylem | Microsoft.Subscription/CreateSubscription/action | Bir Azure aboneliği oluşturun |
> | Eylem | Microsoft.Subscription/register/action | Registers Subscription with Microsoft.Subscription resource provider |
> | Eylem | Microsoft.Subscription/rename/action | Renames the subscription |
> | Eylem | Microsoft.Subscription/SubscriptionDefinitions/read | Get an Azure subscription definition within a management group. |
> | Eylem | Microsoft.Subscription/SubscriptionDefinitions/write | Create an Azure subscription definition |

## <a name="microsoftsupport"></a>Microsoft.Support

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.Support/register/action | Destek Kaynağı Sağlayıcısı'na kayıt yapar |
> | Eylem | Microsoft.Support/supportTickets/read | Durum, önem derecesi, kişi ayrıntıları ve iletişimler gibi Destek Biletleri ayrıntılarını alır veya aboneliklerdeki Destek Biletleri listesini alır. |
> | Eylem | Microsoft.Support/supportTickets/write | Destek Bileti oluşturur veya güncelleştirir. Teknik, Faturalama, Kotalar veya Abonelik Yönetimi konusunda bir Destek Bileti oluşturabilirsiniz. Var olan destek biletlerinin önem derecesini, iletişim bilgilerini ve iletişimlerini güncelleştirebilirsiniz. |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.TimeSeriesInsights/environments/accesspolicies/delete | Deletes the access policy. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/accesspolicies/read | Get the properties of an access policy. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/accesspolicies/write | Creates a new access policy for an environment, or updates an existing access policy. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/delete | Deletes the environment. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/eventsources/delete | Deletes the event source. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/eventsources/read | Get the properties of an event source. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/eventsources/write | Creates a new event source for an environment, or updates an existing event source. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/read | Get the properties of an environment. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/referencedatasets/delete | Deletes the reference data set. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/referencedatasets/read | Get the properties of a reference data set. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/referencedatasets/write | Creates a new reference data set for an environment, or updates an existing reference data set. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/status/read | Get the status of the environment, state of its associated operations like ingress. |
> | Eylem | Microsoft.TimeSeriesInsights/environments/write | Creates a new environment, or updates an existing environment. |
> | Eylem | Microsoft.TimeSeriesInsights/register/action | Registers the subscription for the Time Series Insights resource provider and enables the creation of Time Series Insights environments. |

## <a name="microsoftvisualstudio"></a>Microsoft.VisualStudio

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.VisualStudio/Account/Delete | Hesabı Sil |
> | Eylem | Microsoft.VisualStudio/Account/Extension/Read | Read Account/Extension |
> | Eylem | Microsoft.VisualStudio/Account/Project/Read | Read Account/Project |
> | Eylem | Microsoft.VisualStudio/Account/Project/Write | Set Account/Project |
> | Eylem | Microsoft.VisualStudio/Account/Read | Read Account |
> | Eylem | Microsoft.VisualStudio/Account/Write | Set Account |
> | Eylem | Microsoft.VisualStudio/Extension/Delete | Delete Extension |
> | Eylem | Microsoft.VisualStudio/Extension/Read | Read Extension |
> | Eylem | Microsoft.VisualStudio/Extension/Write | Set Extension |
> | Eylem | Microsoft.VisualStudio/Project/Delete | Delete Project |
> | Eylem | Microsoft.VisualStudio/Project/Read | Read Project |
> | Eylem | Microsoft.VisualStudio/Project/Write | Set Project |
> | Eylem | Microsoft.VisualStudio/Register/Action | Register Azure Subscription with Microsoft.VisualStudio provider |

## <a name="microsoftweb"></a>microsoft.web

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | microsoft.web/apimanagementaccounts/apiacls/read | Get Api Management Accounts Apiacls. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/apiacls/delete | Delete Api Management Accounts APIs Apiacls. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/apiacls/read | Get Api Management Accounts APIs Apiacls. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/apiacls/write | Update Api Management Accounts APIs Apiacls. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/connectionacls/read | Get Api Management Accounts APIs Connectionacls. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/connections/confirmconsentcode/action | Confirm Consent Code Api Management Accounts APIs Connections. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/delete | Delete Api Management Accounts APIs Connections Connectionacls. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/read | Get Api Management Accounts APIs Connections Connectionacls. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/write | Update Api Management Accounts APIs Connections Connectionacls. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/connections/delete | Delete Api Management Accounts APIs Connections. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/connections/getconsentlinks/action | Get Consent Links for Api Management Accounts APIs Connections. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/connections/listconnectionkeys/action | List Connection Keys Api Management Accounts APIs Connections. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/connections/listsecrets/action | List Secrets Api Management Accounts APIs Connections. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/connections/read | Get Api Management Accounts APIs Connections. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/connections/write | Update Api Management Accounts APIs Connections. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/delete | Delete Api Management Accounts APIs. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/delete | Delete Api Management Accounts APIs Localized Definitions. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/read | Get Api Management Accounts APIs Localized Definitions. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/write | Update Api Management Accounts APIs Localized Definitions. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/read | Get Api Management Accounts APIs. |
> | Eylem | microsoft.web/apimanagementaccounts/apis/write | Update Api Management Accounts APIs. |
> | Eylem | microsoft.web/apimanagementaccounts/connectionacls/read | Get Api Management Accounts Connectionacls. |
> | Eylem | microsoft.web/availablestacks/read | Get Available Stacks. |
> | Eylem | Microsoft.Web/certificates/Delete | Delete an existing certificate. |
> | Eylem | Microsoft.Web/certificates/Read | Get the list of certificates. |
> | Eylem | Microsoft.Web/certificates/Write | Add a new certificate or update an existing one. |
> | Eylem | microsoft.web/checknameavailability/read | Check if resource name is available. |
> | Eylem | microsoft.web/classicmobileservices/read | Get Classic Mobile Services. |
> | Eylem | Microsoft.Web/connectionGateways/Delete | Deletes a Connection Gateway. |
> | Eylem | Microsoft.Web/connectionGateways/Join/Action | Joins a Connection Gateway. |
> | Eylem | Microsoft.Web/connectionGateways/ListStatus/Action | Lists status of a Connection Gateway. |
> | Eylem | Microsoft.Web/connectionGateways/Move/Action | Moves a Connection Gateway. |
> | Eylem | Microsoft.Web/connectionGateways/Read | Get the list of Connection Gateways. |
> | Eylem | Microsoft.Web/connectionGateways/Write | Creates or updates a Connection Gateway. |
> | Eylem | microsoft.web/connections/confirmconsentcode/action | Confirm Connections Consent Code. |
> | Eylem | Microsoft.Web/connections/Delete | Deletes a Connection. |
> | Eylem | Microsoft.Web/connections/Join/Action | Joins a Connection. |
> | Eylem | microsoft.web/connections/listconsentlinks/action | List Consent Links for Connections. |
> | Eylem | Microsoft.Web/connections/Move/Action | Moves a Connection. |
> | Eylem | Microsoft.Web/connections/Read | Get the list of Connections. |
> | Eylem | Microsoft.Web/connections/Write | Creates or updates a Connection. |
> | Eylem | Microsoft.Web/customApis/Delete | Deletes a Custom API. |
> | Eylem | Microsoft.Web/customApis/extractApiDefinitionFromWsdl/Action | Extracts API definition from a WSDL. |
> | Eylem | Microsoft.Web/customApis/Join/Action | Joins a Custom API. |
> | Eylem | Microsoft.Web/customApis/listWsdlInterfaces/Action | Lists WSDL interfaces for a Custom API. |
> | Eylem | Microsoft.Web/customApis/Move/Action | Moves a Custom API. |
> | Eylem | Microsoft.Web/customApis/Read | Get the list of Custom API. |
> | Eylem | Microsoft.Web/customApis/Write | Creates or updates a Custom API. |
> | Eylem | Microsoft.Web/deletedSites/Read | Get the properties of a Deleted Web App |
> | Eylem | microsoft.web/deploymentlocations/read | Get Deployment Locations. |
> | Eylem | Microsoft.Web/geoRegions/Read | Get the list of Geo regions. |
> | Eylem | microsoft.web/hostingenvironments/capacities/read | Get Hosting Environments Capacities. |
> | Eylem | Microsoft.Web/hostingEnvironments/Delete | Delete an App Service Environment |
> | Eylem | microsoft.web/hostingenvironments/detectors/read | Get Hosting Environments Detectors. |
> | Eylem | microsoft.web/hostingenvironments/diagnostics/read | Get Hosting Environments Diagnostics. |
> | Eylem | microsoft.web/hostingenvironments/inboundnetworkdependenciesendpoints/read | Get the network endpoints of all inbound dependencies. |
> | Eylem | Microsoft.Web/hostingEnvironments/Join/Action | Joins an App Service Environment |
> | Eylem | microsoft.web/hostingenvironments/metricdefinitions/read | Get Hosting Environments Metric Definitions. |
> | Eylem | microsoft.web/hostingenvironments/multirolepools/metricdefinitions/read | Get Hosting Environments MultiRole Pools Metric Definitions. |
> | Eylem | microsoft.web/hostingenvironments/multirolepools/metrics/read | Get Hosting Environments MultiRole Pools Metrics. |
> | Eylem | Microsoft.Web/hostingEnvironments/multiRolePools/Read | Get the properties of a FrontEnd Pool in an App Service Environment |
> | Eylem | microsoft.web/hostingenvironments/multirolepools/skus/read | Get Hosting Environments MultiRole Pools SKUs. |
> | Eylem | microsoft.web/hostingenvironments/multirolepools/usages/read | Get Hosting Environments MultiRole Pools Usages. |
> | Eylem | Microsoft.Web/hostingEnvironments/multiRolePools/Write | Create a new FrontEnd Pool in an App Service Environment or update an existing one |
> | Eylem | microsoft.web/hostingenvironments/operations/read | Get Hosting Environments Operations. |
> | Eylem | microsoft.web/hostingenvironments/outboundnetworkdependenciesendpoints/read | Get the network endpoints of all outbound dependencies. |
> | Eylem | Microsoft.Web/hostingEnvironments/PrivateEndpointConnectionsApproval/action | Approve Private Endpoint Connections |
> | Eylem | Microsoft.Web/hostingEnvironments/Read | Get the properties of an App Service Environment |
> | Eylem | Microsoft.Web/hostingEnvironments/reboot/Action | Reboot all machines in an App Service Environment |
> | Eylem | microsoft.web/hostingenvironments/resume/action | Resume Hosting Environments. |
> | Eylem | microsoft.web/hostingenvironments/serverfarms/read | Get Hosting Environments App Service Plans. |
> | Eylem | microsoft.web/hostingenvironments/sites/read | Get Hosting Environments Web Apps. |
> | Eylem | microsoft.web/hostingenvironments/suspend/action | Suspend Hosting Environments. |
> | Eylem | microsoft.web/hostingenvironments/usages/read | Get Hosting Environments Usages. |
> | Eylem | microsoft.web/hostingenvironments/workerpools/metricdefinitions/read | Get Hosting Environments Workerpools Metric Definitions. |
> | Eylem | microsoft.web/hostingenvironments/workerpools/metrics/read | Get Hosting Environments Workerpools Metrics. |
> | Eylem | Microsoft.Web/hostingEnvironments/workerPools/Read | Get the properties of a Worker Pool in an App Service Environment |
> | Eylem | microsoft.web/hostingenvironments/workerpools/skus/read | Get Hosting Environments Workerpools SKUs. |
> | Eylem | microsoft.web/hostingenvironments/workerpools/usages/read | Get Hosting Environments Workerpools Usages. |
> | Eylem | Microsoft.Web/hostingEnvironments/workerPools/Write | Create a new Worker Pool in an App Service Environment or update an existing one |
> | Eylem | Microsoft.Web/hostingEnvironments/Write | Create a new App Service Environment or update existing one |
> | Eylem | microsoft.web/ishostingenvironmentnameavailable/read | Get if Hosting Environment Name is available. |
> | Eylem | microsoft.web/ishostnameavailable/read | Check if Hostname is Available. |
> | Eylem | microsoft.web/isusernameavailable/read | Check if Username is available. |
> | Eylem | Microsoft.Web/listSitesAssignedToHostName/Read | Get names of sites assigned to hostname. |
> | Eylem | microsoft.web/locations/apioperations/read | Get Locations API Operations. |
> | Eylem | microsoft.web/locations/connectiongatewayinstallations/read | Get Locations Connection Gateway Installations. |
> | Eylem | microsoft.web/locations/deleteVirtualNetworkOrSubnets/action | Vnet or subnet deletion notification for Locations. |
> | Eylem | microsoft.web/locations/extractapidefinitionfromwsdl/action | Extract Api Definition from WSDL for Locations. |
> | Eylem | microsoft.web/locations/listwsdlinterfaces/action | List WSDL Interfaces for Locations. |
> | Eylem | microsoft.web/locations/managedapis/apioperations/read | Get Locations Managed API Operations. |
> | Eylem | Microsoft.Web/locations/managedapis/Join/Action | Joins a Managed API. |
> | Eylem | microsoft.web/locations/managedapis/read | Get Locations Managed APIs. |
> | Eylem | microsoft.web/locations/operationResults/read | Get Operations. |
> | Eylem | microsoft.web/locations/operations/read | Get Operations. |
> | Eylem | microsoft.web/operations/read | Get Operations. |
> | Eylem | microsoft.web/publishingusers/read | Get Publishing Users. |
> | Eylem | microsoft.web/publishingusers/write | Update Publishing Users. |
> | Eylem | Microsoft.Web/recommendations/Read | Get the list of recommendations for subscriptions. |
> | Eylem | microsoft.web/register/action | Register Microsoft.Web resource provider for the subscription. |
> | Eylem | microsoft.web/resourcehealthmetadata/read | Get Resource Health Metadata. |
> | Eylem | microsoft.web/serverfarms/capabilities/read | Get App Service Plans Capabilities. |
> | Eylem | Microsoft.Web/serverfarms/Delete | Delete an existing App Service Plan |
> | Eylem | Microsoft.Web/serverfarms/eventGridFilters/delete | Delete Event Grid Filter on server farm. |
> | Eylem | Microsoft.Web/serverfarms/eventGridFilters/read | Get Event Grid Filter on server farm. |
> | Eylem | Microsoft.Web/serverfarms/eventGridFilters/write | Put Event Grid Filter on server farm. |
> | Eylem | microsoft.web/serverfarms/firstpartyapps/settings/delete | Delete App Service Plans First Party Apps Settings. |
> | Eylem | microsoft.web/serverfarms/firstpartyapps/settings/read | Get App Service Plans First Party Apps Settings. |
> | Eylem | microsoft.web/serverfarms/firstpartyapps/settings/write | Update App Service Plans First Party Apps Settings. |
> | Eylem | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/delete | Delete App Service Plans Hybrid Connection Namespaces Relays. |
> | Eylem | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/read | Get App Service Plans Hybrid Connection Namespaces Relays. |
> | Eylem | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/sites/read | Get App Service Plans Hybrid Connection Namespaces Relays Web Apps. |
> | Eylem | microsoft.web/serverfarms/hybridconnectionplanlimits/read | Get App Service Plans Hybrid Connection Plan Limits. |
> | Eylem | microsoft.web/serverfarms/hybridconnectionrelays/read | Get App Service Plans Hybrid Connection Relays. |
> | Eylem | microsoft.web/serverfarms/metricdefinitions/read | Get App Service Plans Metric Definitions. |
> | Eylem | microsoft.web/serverfarms/metrics/read | Get App Service Plans Metrics. |
> | Eylem | microsoft.web/serverfarms/operationresults/read | Get App Service Plans Operation Results. |
> | Eylem | Microsoft.Web/serverfarms/Read | Get the properties on an App Service Plan |
> | Eylem | Microsoft.Web/serverfarms/restartSites/Action | Restart all Web Apps in an App Service Plan |
> | Eylem | microsoft.web/serverfarms/sites/read | Get App Service Plans Web Apps. |
> | Eylem | microsoft.web/serverfarms/skus/read | Get App Service Plans SKUs. |
> | Eylem | microsoft.web/serverfarms/usages/read | Get App Service Plans Usages. |
> | Eylem | microsoft.web/serverfarms/virtualnetworkconnections/gateways/write | Update App Service Plans Virtual Network Connections Gateways. |
> | Eylem | microsoft.web/serverfarms/virtualnetworkconnections/read | Get App Service Plans Virtual Network Connections. |
> | Eylem | microsoft.web/serverfarms/virtualnetworkconnections/routes/delete | Delete App Service Plans Virtual Network Connections Routes. |
> | Eylem | microsoft.web/serverfarms/virtualnetworkconnections/routes/read | Get App Service Plans Virtual Network Connections Routes. |
> | Eylem | microsoft.web/serverfarms/virtualnetworkconnections/routes/write | Update App Service Plans Virtual Network Connections Routes. |
> | Eylem | microsoft.web/serverfarms/workers/reboot/action | Reboot App Service Plans Workers. |
> | Eylem | Microsoft.Web/serverfarms/Write | Create a new App Service Plan or update an existing one |
> | Eylem | microsoft.web/sites/analyzecustomhostname/read | Analyze Custom Hostname. |
> | Eylem | Microsoft.Web/sites/applySlotConfig/Action | Apply web app slot configuration from target slot to the current web app |
> | Eylem | Microsoft.Web/sites/backup/Action | Create a new web app backup |
> | Eylem | microsoft.web/sites/backup/read | Get Web Apps Backup. |
> | Eylem | microsoft.web/sites/backup/write | Update Web Apps Backup. |
> | Eylem | microsoft.web/sites/backups/action | Discovers an existing app backup that can be restored from a blob in Azure storage. |
> | Eylem | microsoft.web/sites/backups/delete | Delete Web Apps Backups. |
> | Eylem | microsoft.web/sites/backups/list/action | List Web Apps Backups. |
> | Eylem | Microsoft.Web/sites/backups/Read | Get the properties of a web app's backup |
> | Eylem | microsoft.web/sites/backups/restore/action | Restore Web Apps Backups. |
> | Eylem | microsoft.web/sites/backups/write | Update Web Apps Backups. |
> | Eylem | microsoft.web/sites/config/delete | Delete Web Apps Config. |
> | Eylem | Microsoft.Web/sites/config/list/Action | List Web App's security sensitive settings, such as publishing credentials, app settings and connection strings |
> | Eylem | Microsoft.Web/sites/config/Read | Get Web App configuration settings |
> | Eylem | microsoft.web/sites/config/snapshots/read | Get Web Apps Config Snapshots. |
> | Eylem | Microsoft.Web/sites/config/Write | Update Web App's configuration settings |
> | Eylem | microsoft.web/sites/containerlogs/action | Get Zipped Container Logs for Web App. |
> | Eylem | microsoft.web/sites/containerlogs/download/action | Download Web Apps Container Logs. |
> | Eylem | microsoft.web/sites/continuouswebjobs/delete | Delete Web Apps Continuous Web Jobs. |
> | Eylem | microsoft.web/sites/continuouswebjobs/read | Get Web Apps Continuous Web Jobs. |
> | Eylem | microsoft.web/sites/continuouswebjobs/start/action | Start Web Apps Continuous Web Jobs. |
> | Eylem | microsoft.web/sites/continuouswebjobs/stop/action | Stop Web Apps Continuous Web Jobs. |
> | Eylem | Microsoft.Web/sites/Delete | Delete an existing Web App |
> | Eylem | microsoft.web/sites/deployments/delete | Delete Web Apps Deployments. |
> | Eylem | microsoft.web/sites/deployments/log/read | Get Web Apps Deployments Log. |
> | Eylem | microsoft.web/sites/deployments/read | Get Web Apps Deployments. |
> | Eylem | microsoft.web/sites/deployments/write | Update Web Apps Deployments. |
> | Eylem | microsoft.web/sites/detectors/read | Get Web Apps Detectors. |
> | Eylem | microsoft.web/sites/diagnostics/analyses/execute/Action | Run Web Apps Diagnostics Analysis. |
> | Eylem | microsoft.web/sites/diagnostics/analyses/read | Get Web Apps Diagnostics Analysis. |
> | Eylem | microsoft.web/sites/diagnostics/aspnetcore/read | Get Web Apps Diagnostics for ASP.NET Core app. |
> | Eylem | microsoft.web/sites/diagnostics/autoheal/read | Get Web Apps Diagnostics Autoheal. |
> | Eylem | microsoft.web/sites/diagnostics/deployment/read | Get Web Apps Diagnostics Deployment. |
> | Eylem | microsoft.web/sites/diagnostics/deployments/read | Get Web Apps Diagnostics Deployments. |
> | Eylem | microsoft.web/sites/diagnostics/detectors/execute/Action | Run Web Apps Diagnostics Detector. |
> | Eylem | microsoft.web/sites/diagnostics/detectors/read | Get Web Apps Diagnostics Detector. |
> | Eylem | microsoft.web/sites/diagnostics/failedrequestsperuri/read | Get Web Apps Diagnostics Failed Requests Per Uri. |
> | Eylem | microsoft.web/sites/diagnostics/frebanalysis/read | Get Web Apps Diagnostics FREB Analysis. |
> | Eylem | microsoft.web/sites/diagnostics/loganalyzer/read | Get Web Apps Diagnostics Log Analyzer. |
> | Eylem | microsoft.web/sites/diagnostics/read | Get Web Apps Diagnostics Categories. |
> | Eylem | microsoft.web/sites/diagnostics/runtimeavailability/read | Get Web Apps Diagnostics Runtime Availability. |
> | Eylem | microsoft.web/sites/diagnostics/servicehealth/read | Get Web Apps Diagnostics Service Health. |
> | Eylem | microsoft.web/sites/diagnostics/sitecpuanalysis/read | Get Web Apps Diagnostics Site CPU Analysis. |
> | Eylem | microsoft.web/sites/diagnostics/sitecrashes/read | Get Web Apps Diagnostics Site Crashes. |
> | Eylem | microsoft.web/sites/diagnostics/sitelatency/read | Get Web Apps Diagnostics Site Latency. |
> | Eylem | microsoft.web/sites/diagnostics/sitememoryanalysis/read | Get Web Apps Diagnostics Site Memory Analysis. |
> | Eylem | microsoft.web/sites/diagnostics/siterestartsettingupdate/read | Get Web Apps Diagnostics Site Restart Setting Update. |
> | Eylem | microsoft.web/sites/diagnostics/siterestartuserinitiated/read | Get Web Apps Diagnostics Site Restart User Initiated. |
> | Eylem | microsoft.web/sites/diagnostics/siteswap/read | Get Web Apps Diagnostics Site Swap. |
> | Eylem | microsoft.web/sites/diagnostics/threadcount/read | Get Web Apps Diagnostics Thread Count. |
> | Eylem | microsoft.web/sites/diagnostics/workeravailability/read | Get Web Apps Diagnostics Workeravailability. |
> | Eylem | microsoft.web/sites/diagnostics/workerprocessrecycle/read | Get Web Apps Diagnostics Worker Process Recycle. |
> | Eylem | microsoft.web/sites/domainownershipidentifiers/read | Get Web Apps Domain Ownership Identifiers. |
> | Eylem | microsoft.web/sites/domainownershipidentifiers/write | Update Web Apps Domain Ownership Identifiers. |
> | Eylem | Microsoft.Web/sites/eventGridFilters/delete | Delete Event Grid Filter on web app. |
> | Eylem | Microsoft.Web/sites/eventGridFilters/read | Get Event Grid Filter on web app. |
> | Eylem | Microsoft.Web/sites/eventGridFilters/write | Put Event Grid Filter on web app. |
> | Eylem | microsoft.web/sites/extensions/delete | Delete Web Apps Site Extensions. |
> | Eylem | microsoft.web/sites/extensions/read | Get Web Apps Site Extensions. |
> | Eylem | microsoft.web/sites/extensions/write | Update Web Apps Site Extensions. |
> | Eylem | microsoft.web/sites/functions/action | Functions Web Apps. |
> | Eylem | microsoft.web/sites/functions/delete | Delete Web Apps Functions. |
> | Eylem | microsoft.web/sites/functions/keys/delete | Delete Function keys. |
> | Eylem | microsoft.web/sites/functions/keys/write | Update Function keys. |
> | Eylem | microsoft.web/sites/functions/listkeys/action | List Function keys. |
> | Eylem | microsoft.web/sites/functions/listsecrets/action | List Function secrets. |
> | Eylem | microsoft.web/sites/functions/masterkey/read | Get Web Apps Functions Masterkey. |
> | Eylem | microsoft.web/sites/functions/properties/read | Read Web Apps Functions Properties. |
> | Eylem | microsoft.web/sites/functions/properties/write | Update Web Apps Functions Properties. |
> | Eylem | microsoft.web/sites/functions/read | Get Web Apps Functions. |
> | Eylem | microsoft.web/sites/functions/token/read | Get Web Apps Functions Token. |
> | Eylem | microsoft.web/sites/functions/write | Update Web Apps Functions. |
> | Eylem | microsoft.web/sites/host/functionkeys/delete | Delete Functions Host Function keys. |
> | Eylem | microsoft.web/sites/host/functionkeys/write | Update Functions Host Function keys. |
> | Eylem | microsoft.web/sites/host/listkeys/action | List Functions Host keys. |
> | Eylem | microsoft.web/sites/host/listsyncstatus/action | List Sync Function Triggers Status. |
> | Eylem | microsoft.web/sites/host/properties/read | Read Web Apps Functions Host Properties. |
> | Eylem | microsoft.web/sites/host/sync/action | Sync Function Triggers. |
> | Eylem | microsoft.web/sites/host/systemkeys/delete | Delete Functions Host System keys. |
> | Eylem | microsoft.web/sites/host/systemkeys/write | Update Functions Host System keys. |
> | Eylem | microsoft.web/sites/hostnamebindings/delete | Delete Web Apps Hostname Bindings. |
> | Eylem | microsoft.web/sites/hostnamebindings/read | Get Web Apps Hostname Bindings. |
> | Eylem | microsoft.web/sites/hostnamebindings/write | Update Web Apps Hostname Bindings. |
> | Eylem | microsoft.web/sites/hostruntime/functions/keys/read | Get Web Apps Hostruntime Functions Keys. |
> | Eylem | Microsoft.Web/sites/hostruntime/host/_master/read | Get Function App's master key for admin operations |
> | Eylem | Microsoft.Web/sites/hostruntime/host/action | Perform Function App runtime action like sync triggers, add functions, invoke functions, delete functions etc. |
> | Eylem | microsoft.web/sites/hostruntime/host/read | Get Web Apps Hostruntime Host. |
> | Eylem | microsoft.web/sites/hybridconnection/delete | Delete Web Apps Hybrid Connection. |
> | Eylem | microsoft.web/sites/hybridconnection/read | Get Web Apps Hybrid Connection. |
> | Eylem | microsoft.web/sites/hybridconnection/write | Update Web Apps Hybrid Connection. |
> | Eylem | microsoft.web/sites/hybridconnectionnamespaces/relays/delete | Delete Web Apps Hybrid Connection Namespaces Relays. |
> | Eylem | microsoft.web/sites/hybridconnectionnamespaces/relays/listkeys/action | List Keys Web Apps Hybrid Connection Namespaces Relays. |
> | Eylem | microsoft.web/sites/hybridconnectionnamespaces/relays/read | Get Web Apps Hybrid Connection Namespaces Relays. |
> | Eylem | microsoft.web/sites/hybridconnectionnamespaces/relays/write | Update Web Apps Hybrid Connection Namespaces Relays. |
> | Eylem | microsoft.web/sites/hybridconnectionrelays/read | Get Web Apps Hybrid Connection Relays. |
> | Eylem | microsoft.web/sites/instances/deployments/delete | Delete Web Apps Instances Deployments. |
> | Eylem | microsoft.web/sites/instances/deployments/read | Get Web Apps Instances Deployments. |
> | Eylem | microsoft.web/sites/instances/extensions/log/read | Get Web Apps Instances Extensions Log. |
> | Eylem | microsoft.web/sites/instances/extensions/processes/read | Get Web Apps Instances Extensions Processes. |
> | Eylem | microsoft.web/sites/instances/extensions/read | Get Web Apps Instances Extensions. |
> | Eylem | microsoft.web/sites/instances/processes/delete | Delete Web Apps Instances Processes. |
> | Eylem | microsoft.web/sites/instances/processes/modules/read | Get Web Apps Instances Processes Modules. |
> | Eylem | microsoft.web/sites/instances/processes/read | Get Web Apps Instances Processes. |
> | Eylem | microsoft.web/sites/instances/processes/threads/read | Get Web Apps Instances Processes Threads. |
> | Eylem | microsoft.web/sites/instances/read | Get Web Apps Instances. |
> | Eylem | microsoft.web/sites/listsyncfunctiontriggerstatus/action | List Sync Function Trigger Status. |
> | Eylem | microsoft.web/sites/metricdefinitions/read | Get Web Apps Metric Definitions. |
> | Eylem | microsoft.web/sites/metrics/read | Get Web Apps Metrics. |
> | Eylem | microsoft.web/sites/metricsdefinitions/read | Get Web Apps Metrics Definitions. |
> | Eylem | microsoft.web/sites/migratemysql/action | Migrate MySql Web Apps. |
> | Eylem | microsoft.web/sites/migratemysql/read | Get Web Apps Migrate MySql. |
> | Eylem | microsoft.web/sites/networktrace/action | Network Trace Web Apps. |
> | Eylem | microsoft.web/sites/networktraces/operationresults/read | Get Web Apps Network Trace Operation Results. |
> | Eylem | microsoft.web/sites/newpassword/action | Newpassword Web Apps. |
> | Eylem | microsoft.web/sites/operationresults/read | Get Web Apps Operation Results. |
> | Eylem | microsoft.web/sites/operations/read | Get Web Apps Operations. |
> | Eylem | microsoft.web/sites/perfcounters/read | Get Web Apps Performance Counters. |
> | Eylem | microsoft.web/sites/premieraddons/delete | Delete Web Apps Premier Addons. |
> | Eylem | microsoft.web/sites/premieraddons/read | Get Web Apps Premier Addons. |
> | Eylem | microsoft.web/sites/premieraddons/write | Update Web Apps Premier Addons. |
> | Eylem | microsoft.web/sites/privateaccess/read | Get data around private site access enablement and authorized Virtual Networks that can access the site. |
> | Eylem | Microsoft.Web/sites/PrivateEndpointConnectionsApproval/action | Approve Private Endpoint Connections |
> | Eylem | microsoft.web/sites/processes/modules/read | Get Web Apps Processes Modules. |
> | Eylem | microsoft.web/sites/processes/read | Get Web Apps Processes. |
> | Eylem | microsoft.web/sites/processes/threads/read | Get Web Apps Processes Threads. |
> | Eylem | microsoft.web/sites/publiccertificates/delete | Delete Web Apps Public Certificates. |
> | Eylem | microsoft.web/sites/publiccertificates/read | Get Web Apps Public Certificates. |
> | Eylem | microsoft.web/sites/publiccertificates/write | Update Web Apps Public Certificates. |
> | Eylem | Microsoft.Web/sites/publish/Action | Publish a Web App |
> | Eylem | Microsoft.Web/sites/publishxml/Action | Get publishing profile xml for a Web App |
> | Eylem | microsoft.web/sites/publishxml/read | Get Web Apps Publishing XML. |
> | Eylem | Microsoft.Web/sites/Read | Get the properties of a Web App |
> | Eylem | microsoft.web/sites/recommendationhistory/read | Get Web Apps Recommendation History. |
> | Eylem | microsoft.web/sites/recommendations/disable/action | Disable Web Apps Recommendations. |
> | Eylem | Microsoft.Web/sites/recommendations/Read | Get the list of recommendations for web app. |
> | Eylem | microsoft.web/sites/recover/action | Recover Web Apps. |
> | Eylem | Microsoft.Web/sites/resetSlotConfig/Action | Reset web app configuration |
> | Eylem | microsoft.web/sites/resourcehealthmetadata/read | Get Web Apps Resource Health Metadata. |
> | Eylem | Microsoft.Web/sites/restart/Action | Restart a Web App |
> | Eylem | microsoft.web/sites/restore/read | Get Web Apps Restore. |
> | Eylem | microsoft.web/sites/restore/write | Restore Web Apps. |
> | Eylem | microsoft.web/sites/restorefrombackupblob/action | Restore Web App From Backup Blob. |
> | Eylem | microsoft.web/sites/restorefromdeletedapp/action | Restore Web Apps From Deleted App. |
> | Eylem | microsoft.web/sites/restoresnapshot/action | Restore Web Apps Snapshots. |
> | Eylem | microsoft.web/sites/siteextensions/delete | Delete Web Apps Site Extensions. |
> | Eylem | microsoft.web/sites/siteextensions/read | Get Web Apps Site Extensions. |
> | Eylem | microsoft.web/sites/siteextensions/write | Update Web Apps Site Extensions. |
> | Eylem | microsoft.web/sites/slots/analyzecustomhostname/read | Get Web Apps Slots Analyze Custom Hostname. |
> | Eylem | Microsoft.Web/sites/slots/applySlotConfig/Action | Apply web app slot configuration from target slot to the current slot. |
> | Eylem | Microsoft.Web/sites/slots/backup/Action | Create new Web App Slot backup. |
> | Eylem | microsoft.web/sites/slots/backup/read | Get Web Apps Slots Backup. |
> | Eylem | microsoft.web/sites/slots/backup/write | Update Web Apps Slots Backup. |
> | Eylem | microsoft.web/sites/slots/backups/action | Discover Web Apps Slots Backups. |
> | Eylem | microsoft.web/sites/slots/backups/delete | Delete Web Apps Slots Backups. |
> | Eylem | microsoft.web/sites/slots/backups/list/action | List Web Apps Slots Backups. |
> | Eylem | Microsoft.Web/sites/slots/backups/Read | Get the properties of a web app slots' backup |
> | Eylem | microsoft.web/sites/slots/backups/restore/action | Restore Web Apps Slots Backups. |
> | Eylem | microsoft.web/sites/slots/config/delete | Delete Web Apps Slots Config. |
> | Eylem | Microsoft.Web/sites/slots/config/list/Action | List Web App Slot's security sensitive settings, such as publishing credentials, app settings and connection strings |
> | Eylem | Microsoft.Web/sites/slots/config/Read | Get Web App Slot's configuration settings |
> | Eylem | Microsoft.Web/sites/slots/config/Write | Update Web App Slot's configuration settings |
> | Eylem | microsoft.web/sites/slots/containerlogs/action | Get Zipped Container Logs for Web App Slot. |
> | Eylem | microsoft.web/sites/slots/containerlogs/download/action | Download Web Apps Slots Container Logs. |
> | Eylem | microsoft.web/sites/slots/continuouswebjobs/delete | Delete Web Apps Slots Continuous Web Jobs. |
> | Eylem | microsoft.web/sites/slots/continuouswebjobs/read | Get Web Apps Slots Continuous Web Jobs. |
> | Eylem | microsoft.web/sites/slots/continuouswebjobs/start/action | Start Web Apps Slots Continuous Web Jobs. |
> | Eylem | microsoft.web/sites/slots/continuouswebjobs/stop/action | Stop Web Apps Slots Continuous Web Jobs. |
> | Eylem | Microsoft.Web/sites/slots/Delete | Delete an existing Web App Slot |
> | Eylem | microsoft.web/sites/slots/deployments/delete | Delete Web Apps Slots Deployments. |
> | Eylem | microsoft.web/sites/slots/deployments/log/read | Get Web Apps Slots Deployments Log. |
> | Eylem | microsoft.web/sites/slots/deployments/read | Get Web Apps Slots Deployments. |
> | Eylem | microsoft.web/sites/slots/deployments/write | Update Web Apps Slots Deployments. |
> | Eylem | microsoft.web/sites/slots/detectors/read | Get Web Apps Slots Detectors. |
> | Eylem | microsoft.web/sites/slots/diagnostics/analyses/execute/Action | Run Web Apps Slots Diagnostics Analysis. |
> | Eylem | microsoft.web/sites/slots/diagnostics/analyses/read | Get Web Apps Slots Diagnostics Analysis. |
> | Eylem | microsoft.web/sites/slots/diagnostics/aspnetcore/read | Get Web Apps Slots Diagnostics for ASP.NET Core app. |
> | Eylem | microsoft.web/sites/slots/diagnostics/autoheal/read | Get Web Apps Slots Diagnostics Autoheal. |
> | Eylem | microsoft.web/sites/slots/diagnostics/deployment/read | Get Web Apps Slots Diagnostics Deployment. |
> | Eylem | microsoft.web/sites/slots/diagnostics/deployments/read | Get Web Apps Slots Diagnostics Deployments. |
> | Eylem | microsoft.web/sites/slots/diagnostics/detectors/execute/Action | Run Web Apps Slots Diagnostics Detector. |
> | Eylem | microsoft.web/sites/slots/diagnostics/detectors/read | Get Web Apps Slots Diagnostics Detector. |
> | Eylem | microsoft.web/sites/slots/diagnostics/frebanalysis/read | Get Web Apps Slots Diagnostics FREB Analysis. |
> | Eylem | microsoft.web/sites/slots/diagnostics/loganalyzer/read | Get Web Apps Slots Diagnostics Log Analyzer. |
> | Eylem | microsoft.web/sites/slots/diagnostics/read | Get Web Apps Slots Diagnostics. |
> | Eylem | microsoft.web/sites/slots/diagnostics/runtimeavailability/read | Get Web Apps Slots Diagnostics Runtime Availability. |
> | Eylem | microsoft.web/sites/slots/diagnostics/servicehealth/read | Get Web Apps Slots Diagnostics Service Health. |
> | Eylem | microsoft.web/sites/slots/diagnostics/sitecpuanalysis/read | Get Web Apps Slots Diagnostics Site CPU Analysis. |
> | Eylem | microsoft.web/sites/slots/diagnostics/sitecrashes/read | Get Web Apps Slots Diagnostics Site Crashes. |
> | Eylem | microsoft.web/sites/slots/diagnostics/sitelatency/read | Get Web Apps Slots Diagnostics Site Latency. |
> | Eylem | microsoft.web/sites/slots/diagnostics/sitememoryanalysis/read | Get Web Apps Slots Diagnostics Site Memory Analysis. |
> | Eylem | microsoft.web/sites/slots/diagnostics/siterestartsettingupdate/read | Get Web Apps Slots Diagnostics Site Restart Setting Update. |
> | Eylem | microsoft.web/sites/slots/diagnostics/siterestartuserinitiated/read | Get Web Apps Slots Diagnostics Site Restart User Initiated. |
> | Eylem | microsoft.web/sites/slots/diagnostics/siteswap/read | Get Web Apps Slots Diagnostics Site Swap. |
> | Eylem | microsoft.web/sites/slots/diagnostics/threadcount/read | Get Web Apps Slots Diagnostics Thread Count. |
> | Eylem | microsoft.web/sites/slots/diagnostics/workeravailability/read | Get Web Apps Slots Diagnostics Workeravailability. |
> | Eylem | microsoft.web/sites/slots/diagnostics/workerprocessrecycle/read | Get Web Apps Slots Diagnostics Worker Process Recycle. |
> | Eylem | microsoft.web/sites/slots/domainownershipidentifiers/read | Get Web Apps Slots Domain Ownership Identifiers. |
> | Eylem | microsoft.web/sites/slots/functions/listsecrets/action | List Secrets Web Apps Slots Functions. |
> | Eylem | microsoft.web/sites/slots/functions/read | Get Web Apps Slots Functions. |
> | Eylem | microsoft.web/sites/slots/hostnamebindings/delete | Delete Web Apps Slots Hostname Bindings. |
> | Eylem | microsoft.web/sites/slots/hostnamebindings/read | Get Web Apps Slots Hostname Bindings. |
> | Eylem | microsoft.web/sites/slots/hostnamebindings/write | Update Web Apps Slots Hostname Bindings. |
> | Eylem | microsoft.web/sites/slots/hybridconnection/delete | Delete Web Apps Slots Hybrid Connection. |
> | Eylem | microsoft.web/sites/slots/hybridconnection/read | Get Web Apps Slots Hybrid Connection. |
> | Eylem | microsoft.web/sites/slots/hybridconnection/write | Update Web Apps Slots Hybrid Connection. |
> | Eylem | microsoft.web/sites/slots/hybridconnectionnamespaces/relays/delete | Delete Web Apps Slots Hybrid Connection Namespaces Relays. |
> | Eylem | microsoft.web/sites/slots/hybridconnectionnamespaces/relays/write | Update Web Apps Slots Hybrid Connection Namespaces Relays. |
> | Eylem | microsoft.web/sites/slots/hybridconnectionrelays/read | Get Web Apps Slots Hybrid Connection Relays. |
> | Eylem | microsoft.web/sites/slots/instances/deployments/read | Get Web Apps Slots Instances Deployments. |
> | Eylem | microsoft.web/sites/slots/instances/processes/delete | Delete Web Apps Slots Instances Processes. |
> | Eylem | microsoft.web/sites/slots/instances/processes/read | Get Web Apps Slots Instances Processes. |
> | Eylem | microsoft.web/sites/slots/instances/read | Get Web Apps Slots Instances. |
> | Eylem | microsoft.web/sites/slots/metricdefinitions/read | Get Web Apps Slots Metric Definitions. |
> | Eylem | microsoft.web/sites/slots/metrics/read | Get Web Apps Slots Metrics. |
> | Eylem | microsoft.web/sites/slots/migratemysql/read | Get Web Apps Slots Migrate MySql. |
> | Eylem | microsoft.web/sites/slots/networktrace/action | Network Trace Web Apps Slots. |
> | Eylem | microsoft.web/sites/slots/networktraces/operationresults/read | Get Web Apps Slots Network Trace Operation Results. |
> | Eylem | microsoft.web/sites/slots/newpassword/action | Newpassword Web Apps Slots. |
> | Eylem | microsoft.web/sites/slots/operationresults/read | Get Web Apps Slots Operation Results. |
> | Eylem | microsoft.web/sites/slots/operations/read | Get Web Apps Slots Operations. |
> | Eylem | microsoft.web/sites/slots/perfcounters/read | Get Web Apps Slots Performance Counters. |
> | Eylem | microsoft.web/sites/slots/phplogging/read | Get Web Apps Slots Phplogging. |
> | Eylem | microsoft.web/sites/slots/premieraddons/delete | Delete Web Apps Slots Premier Addons. |
> | Eylem | microsoft.web/sites/slots/premieraddons/read | Get Web Apps Slots Premier Addons. |
> | Eylem | microsoft.web/sites/slots/premieraddons/write | Update Web Apps Slots Premier Addons. |
> | Eylem | microsoft.web/sites/slots/processes/read | Get Web Apps Slots Processes. |
> | Eylem | microsoft.web/sites/slots/publiccertificates/delete | Delete Web Apps Slots Public Certificates. |
> | Eylem | microsoft.web/sites/slots/publiccertificates/read | Get Web Apps Slots Public Certificates. |
> | Eylem | microsoft.web/sites/slots/publiccertificates/write | Create or Update Web Apps Slots Public Certificates. |
> | Eylem | Microsoft.Web/sites/slots/publish/Action | Publish a Web App Slot |
> | Eylem | Microsoft.Web/sites/slots/publishxml/Action | Get publishing profile xml for Web App Slot |
> | Eylem | Microsoft.Web/sites/slots/Read | Get the properties of a Web App deployment slot |
> | Eylem | microsoft.web/sites/slots/recover/action | Recover Web Apps Slots. |
> | Eylem | Microsoft.Web/sites/slots/resetSlotConfig/Action | Reset web app slot configuration |
> | Eylem | microsoft.web/sites/slots/resourcehealthmetadata/read | Get Web Apps Slots Resource Health Metadata. |
> | Eylem | Microsoft.Web/sites/slots/restart/Action | Restart a Web App Slot |
> | Eylem | microsoft.web/sites/slots/restore/read | Get Web Apps Slots Restore. |
> | Eylem | microsoft.web/sites/slots/restore/write | Restore Web Apps Slots. |
> | Eylem | microsoft.web/sites/slots/restorefrombackupblob/action | Restore Web Apps Slot From Backup Blob. |
> | Eylem | microsoft.web/sites/slots/restorefromdeletedapp/action | Restore Web App Slots From Deleted App. |
> | Eylem | microsoft.web/sites/slots/restoresnapshot/action | Restore Web Apps Slots Snapshots. |
> | Eylem | microsoft.web/sites/slots/siteextensions/delete | Delete Web Apps Slots Site Extensions. |
> | Eylem | microsoft.web/sites/slots/siteextensions/read | Get Web Apps Slots Site Extensions. |
> | Eylem | microsoft.web/sites/slots/siteextensions/write | Update Web Apps Slots Site Extensions. |
> | Eylem | Microsoft.Web/sites/slots/slotsdiffs/Action | Get differences in configuration between web app and slots |
> | Eylem | Microsoft.Web/sites/slots/slotsswap/Action | Swap Web App deployment slots |
> | Eylem | microsoft.web/sites/slots/snapshots/read | Get Web Apps Slots Snapshots. |
> | Eylem | Microsoft.Web/sites/slots/sourcecontrols/Delete | Delete Web App Slot's source control configuration settings |
> | Eylem | Microsoft.Web/sites/slots/sourcecontrols/Read | Get Web App Slot's source control configuration settings |
> | Eylem | Microsoft.Web/sites/slots/sourcecontrols/Write | Update Web App Slot's source control configuration settings |
> | Eylem | Microsoft.Web/sites/slots/start/Action | Start a Web App Slot |
> | Eylem | Microsoft.Web/sites/slots/stop/Action | Stop a Web App Slot |
> | Eylem | microsoft.web/sites/slots/sync/action | Sync Web Apps Slots. |
> | Eylem | microsoft.web/sites/slots/triggeredwebjobs/delete | Delete Web Apps Slots Triggered WebJobs. |
> | Eylem | microsoft.web/sites/slots/triggeredwebjobs/read | Get Web Apps Slots Triggered WebJobs. |
> | Eylem | microsoft.web/sites/slots/triggeredwebjobs/run/action | Run Web Apps Slots Triggered WebJobs. |
> | Eylem | microsoft.web/sites/slots/usages/read | Get Web Apps Slots Usages. |
> | Eylem | microsoft.web/sites/slots/virtualnetworkconnections/delete | Delete Web Apps Slots Virtual Network Connections. |
> | Eylem | microsoft.web/sites/slots/virtualnetworkconnections/gateways/write | Update Web Apps Slots Virtual Network Connections Gateways. |
> | Eylem | microsoft.web/sites/slots/virtualnetworkconnections/read | Get Web Apps Slots Virtual Network Connections. |
> | Eylem | microsoft.web/sites/slots/virtualnetworkconnections/write | Update Web Apps Slots Virtual Network Connections. |
> | Eylem | microsoft.web/sites/slots/webjobs/read | Get Web Apps Slots WebJobs. |
> | Eylem | Microsoft.Web/sites/slots/Write | Create a new Web App Slot or update an existing one |
> | Eylem | Microsoft.Web/sites/slotsdiffs/Action | Get differences in configuration between web app and slots |
> | Eylem | Microsoft.Web/sites/slotsswap/Action | Swap Web App deployment slots |
> | Eylem | microsoft.web/sites/snapshots/read | Get Web Apps Snapshots. |
> | Eylem | Microsoft.Web/sites/sourcecontrols/Delete | Delete Web App's source control configuration settings |
> | Eylem | Microsoft.Web/sites/sourcecontrols/Read | Get Web App's source control configuration settings |
> | Eylem | Microsoft.Web/sites/sourcecontrols/Write | Update Web App's source control configuration settings |
> | Eylem | Microsoft.Web/sites/start/Action | Start a Web App |
> | Eylem | Microsoft.Web/sites/stop/Action | Stop a Web App |
> | Eylem | microsoft.web/sites/sync/action | Sync Web Apps. |
> | Eylem | microsoft.web/sites/syncfunctiontriggers/action | Sync Function Triggers. |
> | Eylem | microsoft.web/sites/triggeredwebjobs/delete | Delete Web Apps Triggered WebJobs. |
> | Eylem | microsoft.web/sites/triggeredwebjobs/history/read | Get Web Apps Triggered WebJobs History. |
> | Eylem | microsoft.web/sites/triggeredwebjobs/read | Get Web Apps Triggered WebJobs. |
> | Eylem | microsoft.web/sites/triggeredwebjobs/run/action | Run Web Apps Triggered WebJobs. |
> | Eylem | microsoft.web/sites/usages/read | Get Web Apps Usages. |
> | Eylem | microsoft.web/sites/virtualnetworkconnections/delete | Delete Web Apps Virtual Network Connections. |
> | Eylem | microsoft.web/sites/virtualnetworkconnections/gateways/read | Get Web Apps Virtual Network Connections Gateways. |
> | Eylem | microsoft.web/sites/virtualnetworkconnections/gateways/write | Update Web Apps Virtual Network Connections Gateways. |
> | Eylem | microsoft.web/sites/virtualnetworkconnections/read | Get Web Apps Virtual Network Connections. |
> | Eylem | microsoft.web/sites/virtualnetworkconnections/write | Update Web Apps Virtual Network Connections. |
> | Eylem | microsoft.web/sites/webjobs/read | Get Web Apps WebJobs. |
> | Eylem | Microsoft.Web/sites/Write | Create a new Web App or update an existing one |
> | Eylem | microsoft.web/skus/read | Get SKUs. |
> | Eylem | microsoft.web/sourcecontrols/read | Get Source Controls. |
> | Eylem | microsoft.web/sourcecontrols/write | Update Source Controls. |
> | Eylem | microsoft.web/unregister/action | Unregister Microsoft.Web resource provider for the subscription. |
> | Eylem | microsoft.web/validate/action | Validate . |
> | Eylem | microsoft.web/verifyhostingenvironmentvnet/action | Verify Hosting Environment Vnet. |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | İşlem | Açıklama |
> | --- | --- | --- |
> | Eylem | Microsoft.WorkloadMonitor/components/read | Gets components for the resource |
> | Eylem | Microsoft.WorkloadMonitor/componentsSummary/read | Gets summary of components |
> | Eylem | Microsoft.WorkloadMonitor/monitorInstances/read | Gets instances of monitors for the resource |
> | Eylem | Microsoft.WorkloadMonitor/monitorInstancesSummary/read | Gets summary of monitor instances |
> | Eylem | Microsoft.WorkloadMonitor/monitors/read | Gets monitors for the resource |
> | Eylem | Microsoft.WorkloadMonitor/monitors/write | Configure monitor for the resource |
> | Eylem | Microsoft.WorkloadMonitor/notificationSettings/read | Gets notification settings for the resource |
> | Eylem | Microsoft.WorkloadMonitor/notificationSettings/write | Configure notification settings for the resource |
> | Eylem | Microsoft.WorkloadMonitor/operations/read | Gets the supported operations |

## <a name="next-steps"></a>Sonraki adımlar

- [Match resource provider to service](../azure-resource-manager/azure-services-resource-providers.md)
- [Azure kaynakları için özel roller](custom-roles.md)
- [Azure kaynakları için yerleşik roller](built-in-roles.md)
