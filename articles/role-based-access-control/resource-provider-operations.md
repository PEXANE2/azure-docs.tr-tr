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
ms.date: 08/27/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: dce9308230c2a82c66b1d8689123d9d34fc6e408
ms.sourcegitcommit: 8e1fb03a9c3ad0fc3fd4d6c111598aa74e0b9bd4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70114680"
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Azure Resource Manager kaynak sağlayıcısı işlemleri

Bu makalede, her bir Azure Resource Manager kaynak sağlayıcısı için kullanılabilen işlemler listelenir. Bu işlemler, Azure 'daki kaynaklara ayrıntılı [rol tabanlı erişim denetimi (RBAC)](overview.md) sağlamak için [özel rollerde](custom-roles.md) kullanılabilir. İşlem dizeleri aşağıdaki biçimdedir:`{Company}.{ProviderName}/{resourceType}/{action}`

Kaynak sağlayıcısı işlemleri her zaman gelişmektedir. En son işlemleri almak için [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) veya [az Provider Operation List](/cli/azure/provider/operation#az-provider-operation-list)komutunu kullanın.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. AAD/domainServices/Delete | Etki alanı hizmetini Sil |
> | Action | Microsoft. AAD/domainServices/oucontainer/Delete | OU kapsayıcısını Sil |
> | Action | Microsoft. AAD/domainServices/oucontainer/Read | OU kapsayıcılarını oku |
> | Action | Microsoft. AAD/domainServices/oucontainer/Write | OU kapsayıcısı yaz |
> | Action | Microsoft. AAD/domainServices/Read | Etki alanı hizmetlerini oku |
> | Action | Microsoft. AAD/domainServices/replicaSets/Delete | Küme sitesini sil |
> | Action | Microsoft. AAD/domainServices/replicaSets/Read | Küme sitesini oku |
> | Action | Microsoft. AAD/domainServices/replicaSets/Write | Küme sitesi yaz |
> | Action | Microsoft. AAD/domainServices/Write | Etki alanı hizmeti yaz |
> | Action | Microsoft. AAD/konumlar/operationresults/Read |  |
> | Action | Microsoft. AAD/Işlemler/okuma |  |
> | Action | Microsoft. AAD/Register/ACTION | Etki alanı hizmetini Kaydet |
> | Action | Microsoft. AAD/Unregister/eylem | Etki alanı hizmetinin kaydını sil |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. aadihar/diagnosticsettings/Delete | Bir tanılama ayarını silme |
> | Action | Microsoft. aadihar/diagnosticsettings/Read | Tanılama ayarını okuma |
> | Action | Microsoft. aadihar/diagnosticsettings/Write | Tanılama ayarı yazma |
> | Action | Microsoft. aadihar/diagnosticsettingscategories/Read | Tanılama ayarı kategorilerini okuma |

## <a name="microsoftaddons"></a>Microsoft. addons

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. addons/işlemler/okuma | Desteklenen RP işlemlerini alır. |
> | Action | Microsoft. addons/Register/ACTION | Belirtilen aboneliği Microsoft. addons ile kaydedin |
> | Action | Microsoft. addons/supportProviders/listsupportplanınfo/Action | Belirtilen abonelik için geçerli destek planı bilgilerini listeler. |
> | Action | Microsoft. addons/supportProviders/supportPlanTypes/Delete | Belirtilen kurallı destek planını kaldırır |
> | Action | Microsoft. addons/supportProviders/supportPlanTypes/Read | Belirtilen kurallı destek planı durumunu alır. |
> | Action | Microsoft. addons/supportProviders/supportPlanTypes/Write | Belirtilen kurallı destek planı türünü ekler. |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. ADHybridHealthService/addsservices/Action | Kiracı için yeni bir orman oluşturun. |
> | Action | Microsoft. ADHybridHealthService/addsservices/addomainservicemembers/Read | Belirtilen hizmet adı için tüm sunucuları alır. |
> | Action | Microsoft. ADHybridHealthService/addsservices/uyarılar/Read | Ormanın AlertId, uyarı verdi tarihi, uyarı son algılanan, uyarı açıklaması, son güncelleme, uyarı düzeyi, uyarı durumu, uyarı sorun giderme bağlantıları vb. gibi uyarı ayrıntılarını alır. |
> | Action | Microsoft. ADHybridHealthService/addsservices/Configuration/Read | Ormanın hizmet yapılandırmasını alır. Örnek-Orman adı, Işlev düzeyi, etki alanı adlandırma yöneticisi FSMO rolü, şema yöneticisi FSMO rolü vb. |
> | Action | Microsoft. ADHybridHealthService/addsservices/Delete | Hizmet ve sunucularını sistem durumu verileriyle birlikte siler. |
> | Action | Microsoft. ADHybridHealthService/addsservices/Dimensions/Read | Ormanın etki alanlarını ve site ayrıntılarını alır. Örnek-sistem durumu, etkin uyarılar, çözümlenen uyarılar, etki alanı Işlev düzeyi, orman, altyapı yöneticisi, PDC, RID Yöneticisi vb. gibi özellikler  |
> | Action | Microsoft. ADHybridHealthService/addsservices/Özellikler/kullanıcıtercihi/okuma | Ormanın Kullanıcı tercihi ayarını alır.<br>Ldapbaşarılı fulbind, ntlmaıd 'ler, kerberosauthentications, addsinsıısesagentprivatebytes, ldaparamaları gibi örnek-MetricCounterName.<br>UI grafikleri vs. ayarları. |
> | Action | Microsoft. ADHybridHealthService/addsservices/forestsummary/Read | Belirtilen orman için orman adı, bu ormandaki etki alanı sayısı, site ve site ayrıntıları gibi orman özetini alır. |
> | Action | Microsoft. ADHybridHealthService/addsservices/metricmetadata/Read | Belirli bir hizmet için desteklenen ölçümlerin listesini alır.<br>Örneğin, extranet hesabı kilitleme Işlemleri, toplam başarısız Istek sayısı, bekleyen belirteç Istekleri (proxy), ADFS hizmeti için belirteç Istekleri/sn.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP etkin Iş parçacıkları, Kerberos kimlik doğrulamaları/sn, ADDomainService için ATQ Iş parçacığı toplamı.<br>Profil gecikmesini çalıştırın, TCP bağlantıları oluşturulmuştur, Insights Aracısı özel baytları, ADSync hizmeti için Azure AD 'ye Istatistikleri dışarı aktarın. |
> | Action | Microsoft. ADHybridHealthService/addsservices/ölçümler/gruplar/okuma | Hizmet verildiğinde, bu API ölçüm bilgilerini alır.<br>Örneğin, bu API ile ilgili bilgi almak için kullanılabilir: Extranet hesabı kilitleme Işlemleri, toplam başarısız Istek sayısı, bekleyen belirteç Istekleri (proxy), ADFederation hizmeti için belirteç Istekleri/sn.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP etkin Iş parçacıkları, Kerberos kimlik doğrulamaları/sn, ADDomain hizmeti için ATQ Iş parçacığı toplamı.<br>Profil gecikmesini çalıştırın, TCP bağlantıları oluşturulmuştur, Insights Aracısı özel baytları, Istatistikleri eşitleme hizmeti için Azure AD 'ye aktarın. |
> | Action | Microsoft. ADHybridHealthService/addsservices/premiumcheck/Read | Bu API, Premium kiracı için tüm eklendi ADDomainServices listesini alır. |
> | Action | Microsoft. ADHybridHealthService/addsservices/Read | Belirtilen hizmet adı için hizmet ayrıntılarını alır. |
> | Action | Microsoft. ADHybridHealthService/addsservices/replicationdetails/Read | Belirtilen hizmet adı için tüm sunucular için çoğaltma ayrıntılarını alır. |
> | Action | Microsoft. ADHybridHealthService/addsservices/ReplicationStatus/Read | Varsa, etki alanı denetleyicilerinin sayısını ve bunların çoğaltma hatalarını alır. |
> | Action | Microsoft. ADHybridHealthService/addsservices/replicationsummary/Read | Tüm etki alanı denetleyicisi listesini, belirtilen ormanın çoğaltma ayrıntılarının birlikte alır. |
> | Action | Microsoft. ADHybridHealthService/addsservices/servicemembers/Action | Hizmete bir sunucu örneği ekleyin. |
> | Action | Microsoft. ADHybridHealthService/addsservices/servicemembers/Credentials/Read | ADDomainService 'in sunucu kaydı sırasında, yeni sunucuları ekleme kimlik bilgilerini almak için bu API çağırılır. |
> | Action | Microsoft. ADHybridHealthService/addsservices/servicemembers/Delete | Belirli bir hizmet ve kiracı için bir sunucuyu siler. |
> | Action | Microsoft. ADHybridHealthService/addsservices/Write | Kiracı için ADDomainService örneğini oluşturur veya güncelleştirir. |
> | Action | Microsoft. ADHybridHealthService/yapılandırma/eylem | Kiracı yapılandırmasını güncelleştirir. |
> | Action | Microsoft. ADHybridHealthService/yapılandırma/okuma | Kiracı yapılandırmasını okur. |
> | Action | Microsoft. ADHybridHealthService/yapılandırma/yazma | Kiracı yapılandırması oluşturur. |
> | Action | Microsoft. ADHybridHealthService/logs/Contents/Read | Blob 'da depolanan aracı yükleme ve kayıt günlüklerinin içeriğini alır. |
> | Action | Microsoft. ADHybridHealthService/logs/Read | Kiracının aracı yüklemesini ve kayıt günlüklerini alır. |
> | Action | Microsoft. ADHybridHealthService/işlemler/okuma | Sistem tarafından desteklenen işlemlerin listesini alır. |
> | Action | Microsoft. ADHybridHealthService/Register/Action | Adhibrit Sistem Sağlığı Hizmeti kaynak sağlayıcısını kaydeder ve Adhibrit Sistem Sağlığı Hizmeti kaynağının oluşturulmasına izin vermez. |
> | Action | Microsoft. ADHybridHealthService/Reports/availabledağıtımlar/okuma | DevOps tarafından müşteri olaylarını desteklemek için kullanılan kullanılabilir bölgelerin listesini alır. |
> | Action | Microsoft. ADHybridHealthService/Reports/badpassword/Read | Active Directory Federasyon Hizmeti tüm kullanıcılar için hatalı parola denemelerinin listesini alır. |
> | Action | Microsoft. ADHybridHealthService/Reports/badpassworduseridıservıce frekansı/okuma | Belirli bir kiracı için her gün için IP başına Kullanıcı başına Hatalı Kullanıcı adı/parola denemesi sıklığı ve yeni sıraya alınmış rapor işinin nihai sonucunu içeren blob SAS URI 'sini alır. |
> | Action | Microsoft. ADHybridHealthService/Reports/consentedtodevopstenants/Read | DevOps onaylı kiracılar listesini alır. Genellikle müşteri desteği için kullanılır. |
> | Action | Microsoft. ADHybridHealthService/Reports/ıdevops/Read | Kiracının DevOps consented olup olmadığını gösteren bir değer alır. |
> | Action | Microsoft. ADHybridHealthService/Reports/selectdevopstenant/Read | Seçili dev Ops kiracısı için UserID 'yi (ObjectID) güncelleştirir. |
> | Action | Microsoft. ADHybridHealthService/Reports/selecteddeployment/Read | Belirtilen kiracı için seçili dağıtımı alır. |
> | Action | Microsoft. ADHybridHealthService/Reports/tenantassigneddeployment/Read | Verilen Kiracı kimliği kiracı depolama konumunu alır. |
> | Action | Microsoft. ADHybridHealthService/Reports/updateselecteddeployment/Read | Verilerin erişilecek coğrafi konumunu alır. |
> | Action | Microsoft. ADHybridHealthService/Services/Action | Kiracıdaki bir hizmet örneğini güncelleştirir. |
> | Action | Microsoft. ADHybridHealthService/Services/uyarılar/okuma | Bir hizmet için uyarıları okur. |
> | Action | Microsoft. ADHybridHealthService/Services/uyarılar/okuma | Bir hizmet için uyarıları okur. |
> | Action | Microsoft. ADHybridHealthService/Services/checkservicefeatureavability/Read | Hizmet adı verildiğinde, bir hizmetin bu özelliği kullanmak için gereken her şey olup olmadığını doğrular. |
> | Action | Microsoft. ADHybridHealthService/Services/Delete | Kiracıdaki bir hizmet örneğini siler. |
> | Action | Microsoft. ADHybridHealthService/Services/exporterrors/Read | Belirli bir eşitleme hizmeti için dışarı aktarma hatalarını alır. |
> | Action | Microsoft. ADHybridHealthService/Services/exportstatus/Read | Belirli bir hizmet için dışarı aktarma durumunu alır. |
> | Action | Microsoft. ADHybridHealthService/Services/feedbacktype/görüşler/Read | Belirli bir hizmet ve sunucu için uyarı geri bildirimi alır. |
> | Action | Microsoft. ADHybridHealthService/Services/metricmetadata/Read | Belirli bir hizmet için desteklenen ölçümlerin listesini alır.<br>Örneğin, extranet hesabı kilitleme Işlemleri, toplam başarısız Istek sayısı, bekleyen belirteç Istekleri (proxy), ADFS hizmeti için belirteç Istekleri/sn.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP etkin Iş parçacıkları, Kerberos kimlik doğrulamaları/sn, ADDomainService için ATQ Iş parçacığı toplamı.<br>Profil gecikmesini çalıştırın, TCP bağlantıları oluşturulmuştur, Insights Aracısı özel baytları, ADSync hizmeti için Azure AD 'ye Istatistikleri dışarı aktarın. |
> | Action | Microsoft. ADHybridHealthService/Services/ölçümler/gruplar/ortalama/okuma | Hizmet verildiğinde, bu API belirli bir hizmet için ölçümlerin ortalamasını alır.<br>Örneğin, bu API ile ilgili bilgi almak için kullanılabilir: Extranet hesabı kilitleme Işlemleri, toplam başarısız Istek sayısı, bekleyen belirteç Istekleri (proxy), ADFederation hizmeti için belirteç Istekleri/sn.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP etkin Iş parçacıkları, Kerberos kimlik doğrulamaları/sn, ADDomain hizmeti için ATQ Iş parçacığı toplamı.<br>Profil gecikmesini çalıştırın, TCP bağlantıları oluşturulmuştur, Insights Aracısı özel baytları, Istatistikleri eşitleme hizmeti için Azure AD 'ye aktarın. |
> | Action | Microsoft. ADHybridHealthService/Services/ölçümler/gruplar/okuma | Hizmet verildiğinde, bu API ölçüm bilgilerini alır.<br>Örneğin, bu API ile ilgili bilgi almak için kullanılabilir: Extranet hesabı kilitleme Işlemleri, toplam başarısız Istek sayısı, bekleyen belirteç Istekleri (proxy), ADFederation hizmeti için belirteç Istekleri/sn.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP etkin Iş parçacıkları, Kerberos kimlik doğrulamaları/sn, ADDomain hizmeti için ATQ Iş parçacığı toplamı.<br>Profil gecikmesini çalıştırın, TCP bağlantıları oluşturulmuştur, Insights Aracısı özel baytları, Istatistikleri eşitleme hizmeti için Azure AD 'ye aktarın. |
> | Action | Microsoft. ADHybridHealthService/Services/ölçümler/gruplar/Sum/Read | Bu API, bir hizmet verildiğinde, belirli bir hizmet için ölçümlerin toplu görünümünü alır.<br>Örneğin, bu API ile ilgili bilgi almak için kullanılabilir: Extranet hesabı kilitleme Işlemleri, toplam başarısız Istek sayısı, bekleyen belirteç Istekleri (proxy), ADFederation hizmeti için belirteç Istekleri/sn.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP etkin Iş parçacıkları, Kerberos kimlik doğrulamaları/sn, ADDomain hizmeti için ATQ Iş parçacığı toplamı.<br>Profil gecikmesini çalıştırın, TCP bağlantıları oluşturulmuştur, Insights Aracısı özel baytları, Istatistikleri eşitleme hizmeti için Azure AD 'ye aktarın. |
> | Action | Microsoft. ADHybridHealthService/Services/monitoringconfiguration/Write | Bir hizmet için izleme yapılandırması ekleme veya güncelleştirme. |
> | Action | Microsoft. ADHybridHealthService/Services/monitoringconfigurations/Read | Belirli bir hizmet için izleme yapılandırmasını alır. |
> | Action | Microsoft. ADHybridHealthService/Services/monitoringconfigurations/Write | Bir hizmet için izleme yapılandırması ekleme veya güncelleştirme. |
> | Action | Microsoft. ADHybridHealthService/Services/premiumcheck/Read | Bu API, bir Premium kiracının tüm eklendi hizmetlerinin listesini alır. |
> | Action | Microsoft. ADHybridHealthService/Services/Read | Kiracıdaki hizmet örneklerini okur. |
> | Action | Microsoft. ADHybridHealthService/Services/Reports/Blobursıs/Read | Son 7 güne ait tüm riskli IP rapor URI 'Lerini alır. |
> | Action | Microsoft. ADHybridHealthService/Services/Reports/details/Read | Son 7 günde hatalı parola hatası olan ilk 50 kullanıcının raporunu alır |
> | Action | Microsoft. ADHybridHealthService/Services/Reports/Generatebloburı/Action | Riskli IP raporu oluşturur ve ona işaret eden bir URI döndürür. |
> | Action | Microsoft. ADHybridHealthService/Services/servicemembers/Action | Hizmette bir sunucu örneği oluşturur. |
> | Action | Microsoft. ADHybridHealthService/Services/servicemembers/uyarılar/Read | Bir sunucu için uyarıları okur. |
> | Action | Microsoft. ADHybridHealthService/Services/servicemembers/Credentials/Read | Sunucu kaydı sırasında, yeni sunucuları ekleme kimlik bilgilerini almak için bu API çağırılır. |
> | Action | Microsoft. ADHybridHealthService/Services/servicemembers/datayeniliği/okuma | Belirli bir sunucu için bu API, sunucular tarafından karşıya yüklenen veri türlerinin bir listesini ve her yükleme için en son saati alır. |
> | Action | Microsoft. ADHybridHealthService/Services/servicemembers/Delete | Hizmette bir sunucu örneğini siler. |
> | Action | Microsoft. ADHybridHealthService/Services/servicemembers/exportstatus/Read | Belirli bir eşitleme hizmeti için eşitleme dışa aktarma hata ayrıntılarını alır. |
> | Action | Microsoft. ADHybridHealthService/Services/servicemembers/ölçümler/gruplar/okuma | Hizmet verildiğinde, bu API ölçüm bilgilerini alır.<br>Örneğin, bu API ile ilgili bilgi almak için kullanılabilir: Extranet hesabı kilitleme Işlemleri, toplam başarısız Istek sayısı, bekleyen belirteç Istekleri (proxy), ADFederation hizmeti için belirteç Istekleri/sn.<br>NTLM kimlik doğrulamaları/sn, LDAP başarılı bağlamalar/sn, LDAP bağlama süresi, LDAP etkin Iş parçacıkları, Kerberos kimlik doğrulamaları/sn, ADDomain hizmeti için ATQ Iş parçacığı toplamı.<br>Profil gecikmesini çalıştırın, TCP bağlantıları oluşturulmuştur, Insights Aracısı özel baytları, Istatistikleri eşitleme hizmeti için Azure AD 'ye aktarın. |
> | Action | Microsoft. ADHybridHealthService/Services/servicemembers/ölçümler/okuma | Bağlayıcılar listesini alır ve belirtilen hizmet ve hizmet üyesinin profil adlarını çalıştırır. |
> | Action | Microsoft. ADHybridHealthService/Services/servicemembers/Read | Hizmet içindeki sunucu örneğini okur. |
> | Action | Microsoft. ADHybridHealthService/Services/servicemembers/ServiceConfiguration/Read | Belirli bir kiracının hizmet yapılandırmasını alır. |
> | Action | Microsoft. ADHybridHealthService/Services/tenantwhitelisteleme/okuma | Belirli bir kiracının özellik beyaz listeleme durumunu alır. |
> | Action | Microsoft. ADHybridHealthService/Services/Write | Kiracıda bir hizmet örneği oluşturur. |
> | Action | Microsoft. ADHybridHealthService/Unregister/Action | Adhibrit Sistem Sağlığı Hizmeti kaynak sağlayıcısı için aboneliğin kaydını siler. |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. danışman/yapılandırma/okuma | Yapılandırma al |
> | Action | Microsoft. Advisor/Configurations/Write | Yapılandırma oluşturur/güncelleştirir |
> | Action | Microsoft. Advisor/Generatereyorumları/eylem | Öneriler oluşturur |
> | Action | Microsoft. Advisor/Generatereyorumları/okuma | Öneri durumunu al |
> | Action | Microsoft. Advisor/Metadata/Read | Meta verileri al |
> | Action | Microsoft. Advisor/işlemler/okuma | Microsoft Advisor 'ın işlemlerini alır |
> | Action | Microsoft. Advisor/öneriler/kullanılabilir/eylem | Yeni öneri Microsoft Advisor 'da kullanılabilir |
> | Action | Microsoft. Advisor/öneriler/okuma | Önerileri okur |
> | Action | Microsoft. Advisor/öneriler/gizlemeleri/silme | Gizlemeleri siler |
> | Action | Microsoft. Advisor/öneriler/suppressions/okuma | Gizlemeleri alır |
> | Action | Microsoft. Advisor/öneriler/suppressions/Write | Gizlemeleri oluşturur/güncelleştirir |
> | Action | Microsoft. Advisor/Register/ACTION | Microsoft Advisor için aboneliği kaydeder |
> | Action | Microsoft. Advisor/suppressions/Delete | Gizlemeleri siler |
> | Action | Microsoft. Advisor/suppressions/okuma | Gizlemeleri alır |
> | Action | Microsoft. Advisor/suppressions/Write | Gizlemeleri oluşturur/güncelleştirir |
> | Action | Microsoft. Advisor/kaydı sil/eylem | Microsoft Advisor aboneliğinin kaydını siler |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. AlertsManagement/actionRules/Delete | Belirli bir abonelikteki eylem kuralını silin. |
> | Action | Microsoft. AlertsManagement/actionRules/Read | Giriş filtreleri için tüm eylem kurallarını alın. |
> | Action | Microsoft. AlertsManagement/actionRules/Write | Belirli bir abonelikte eylem kuralı oluştur veya güncelleştir |
> | Action | Microsoft. AlertsManagement/uyarılar/ChangeState/Action | Uyarının durumunu değiştirin. |
> | Action | Microsoft. AlertsManagement/Alerts/Diagnostic/Read | Uyarı için tüm tanılamayı al |
> | Action | Microsoft. AlertsManagement/uyarılar/History/Read | Uyarının geçmişini al |
> | Action | Microsoft. AlertsManagement/uyarılar/okuma | Giriş filtreleri için tüm uyarıları alın. |
> | Action | Microsoft. AlertsManagement/alertsList/Read | Abonelikler genelinde giriş filtreleri için tüm uyarıları al |
> | Action | Microsoft. AlertsManagement/alertsMetaData/okuma | Giriş parametresi için uyarılar meta verilerini alın. |
> | Action | Microsoft. AlertsManagement/alertsSummary/okuma | Uyarıların özetini al |
> | Action | Microsoft. AlertsManagement/alertsSummaryList/Read | Abonelikler arasında uyarıların özetini alın |
> | Action | Microsoft. AlertsManagement/Işlemler/okuma | Belirtilen işlemleri okur |
> | Action | Microsoft. AlertsManagement/Register/Action | Microsoft Uyarıları yönetimi için aboneliği kaydeder |
> | Action | Microsoft. AlertsManagement/smartDetectorAlertRules/Delete | Belirli bir abonelikte akıllı algılayıcı uyarı kuralını Sil |
> | Action | Microsoft. AlertsManagement/smartDetectorAlertRules/Read | Giriş filtreleri için tüm akıllı algılayıcı uyarısı kurallarını al |
> | Action | Microsoft. AlertsManagement/smartDetectorAlertRules/Write | Belirli bir abonelikte akıllı algılayıcı uyarı kuralı oluştur veya güncelleştir |
> | Action | Microsoft. AlertsManagement/smartGroups/ChangeState/Action | Akıllı grubun durumunu değiştirme |
> | Action | Microsoft. AlertsManagement/smartGroups/History/Read | Akıllı grup geçmişini al |
> | Action | Microsoft. AlertsManagement/smartGroups/Read | Giriş filtreleri için tüm akıllı grupları Al |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. AnalysisServices/Locations/Checknameavaılabılıty/Action | Verilen Analiz Sunucusu adının geçerli ve kullanımda olup olmadığını denetler. |
> | Action | Microsoft. AnalysisServices/konumlar/operationresults/Read | Belirtilen işlem sonucu bilgilerini alır. |
> | Action | Microsoft. AnalysisServices/Locations/operationdurumlarının/Read | Belirtilen işlem durumu bilgilerini alır. |
> | Action | Microsoft. AnalysisServices/işlemler/okuma | İşlem bilgilerini alır |
> | Action | Microsoft. AnalysisServices/Register/ACTION | Analysis Services kaynak sağlayıcısını kaydeder. |
> | Action | Microsoft. AnalysisServices/Servers/Delete | Analiz Sunucusu siler. |
> | Action | Microsoft. AnalysisServices/Servers/listGatewayStatus/Action | Sunucuyla ilişkili ağ geçidinin durumunu listeleyin. |
> | Action | Microsoft. AnalysisServices/Servers/Read | Belirtilen Analiz Sunucusu bilgilerini alır. |
> | Action | Microsoft. AnalysisServices/Servers/özgeçmişi/Action | Analiz Sunucusu devam ettirir. |
> | Action | Microsoft. AnalysisServices/Servers/SKU 'ları/okuma | Sunucu için kullanılabilir SKU bilgilerini alın |
> | Action | Microsoft. AnalysisServices/Servers/beklet/Action | Analiz Sunucusu askıya alır. |
> | Action | Microsoft. AnalysisServices/Servers/Write | Belirtilen Analiz Sunucusu oluşturur veya güncelleştirir. |
> | Action | Microsoft. AnalysisServices/SKU 'lar/okuma | SKU 'ların bilgilerini alır |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Apimanamı/Checknameavaılabılıty/Read | Sağlanan hizmet adının kullanılabilir olup olmadığını denetler |
> | Action | Microsoft. Apimanayönetimi/işlemler/okuma | Microsoft. Apimana, kaynağı için kullanılabilen tüm API işlemlerini okuyun |
> | Action | Microsoft. Apimanayönetimi/kaydet/eylem | Microsoft. Apimanayönetimi kaynak sağlayıcısı için aboneliği kaydedin |
> | Action | Microsoft. Apimanayönetimi/raporlar/okuma | Zaman dönemleri, coğrafi bölge, geliştiriciler, ürünler, API 'Ler, işlemler, abonelik ve byRequest tarafından toplanan raporları alın. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/silme | API Management hizmet örneğinin belirtilen API 'sini siler. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/tanılama/silme | Belirtilen tanılamayı bir API 'den siler. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/tanılama/okuma | Bir API 'nin tüm tanılamalarını listeler. ya da kendi tanımlayıcısı tarafından belirtilen bir API için tanı ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/tanılama/yazma | Bir API için yeni bir tanılama oluşturur veya var olan bir API 'yi güncelleştirir. ya da kendi tanımlayıcısı tarafından belirtilen bir API için tanı ayrıntılarını günceller. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/sorunlar/ekler/Sil | Bir sorundan belirtilen yorumu siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/sorunlar/ekler/okuma | Belirtilen API ile ilişkili sorun için tüm ekleri listeler. ya da kendi tanımlayıcısına göre belirtilen bir API için sorun ekinin ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/sorunlar/ekler/yazma | Bir API 'de sorun için yeni bir ek oluşturur veya var olanı güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/sorunlar/açıklamalar/silme | Bir sorundan belirtilen yorumu siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/sorunlar/açıklamalar/okuma | Belirtilen API ile ilişkili sorun için tüm açıklamaları listeler. ya da kendi tanımlayıcısına göre belirtilen bir API için sorun açıklamasının ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/sorunlar/açıklamalar/yazma | Bir API 'de sorun için yeni bir açıklama oluşturur veya var olan bir güncelleştirmeyi güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/sorunlar/silme | Belirtilen sorunu bir API 'den siler. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/sorunlar/okuma | Belirtilen API ile ilişkili tüm sorunları listeler. ya da kendi tanımlayıcısına göre belirtilen bir API için sorunun ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/sorunlar/yazma | Bir API için yeni bir sorun oluşturur veya var olan bir sorunu güncelleştirir. ya da bir API için mevcut bir sorunu güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/işlemler/silme | API 'de belirtilen işlemi siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/işlemler/ilkeler/silme | API Işlemindeki ilke yapılandırmasını siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/işlemler/ilkeler/okuma | API Işlem düzeyindeki ilke yapılandırması listesini alın. veya API Işlem düzeyinde ilke yapılandırmasını alın. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/işlemler/ilkeler/yazma | API Işlem düzeyi için ilke yapılandırması oluşturur veya güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/işlemler/ilke/silme | Işlem düzeyinde ilke yapılandırmasını silme |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/işlemler/ilke/okuma | Işlem düzeyinde ilke yapılandırmasını al |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/işlemler/ilke/yazma | Işlem düzeyinde ilke yapılandırması oluştur |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/işlemler/okuma | Belirtilen API için işlemlerin koleksiyonunu listeler. ya da kendi tanımlayıcısı tarafından belirtilen API Işleminin ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/işlemler/Etiketler/silme | Etiketi Işlemden ayırın. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/işlemler/Etiketler/okuma | Işlemle ilişkili tüm etiketleri listeler. veya Işlemle ilişkili Get etiketi. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/işlemler/Etiketler/yazma | Işleme etiket atayın. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/işlemler/yazma | API 'de yeni bir işlem oluşturur veya var olan bir işlemi güncelleştirir. veya işlemin ayrıntılarını, tanımlayıcısı tarafından belirtilen API 'de güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/operationsByTags/okuma | Etiketlerle ilişkili işlemler koleksiyonunu listeler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/ilkeler/silme | API 'de ilke yapılandırmasını siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/ilkeler/okuma | API düzeyinde ilke yapılandırmasını alın. veya API düzeyinde ilke yapılandırmasını alın. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/ilkeler/yazma | API için ilke yapılandırması oluşturur veya güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/ilke/silme | API düzeyinde ilke yapılandırmasını silme |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/ilke/okuma | API düzeyinde ilke yapılandırmasını al |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/ilke/yazma | API düzeyinde ilke yapılandırması oluştur |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/ürünler/okuma | API 'nin parçası olduğu tüm ürünleri listeler. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/okuma | API Management hizmet örneğinin tüm API 'Lerini listeler. ya da kendi tanımlayıcısı tarafından belirtilen API 'nin ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/yayınlar/silme | API 'nin tüm sürümlerini kaldırır veya API 'de belirtilen yayını siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/yayınlar/okuma | Bir API 'nin tüm sürümlerini listeler.<br>API 'nin düzeltilmesi geçerli olduğunda bir API yayını oluşturulur.<br>Sürümler, önceki düzeltmelere geri dönmek için de kullanılır.<br>Sonuçlar disk belleğine alınır ve $top ve $skip parametreleriyle kısıtlanabilir.<br>ya da bir API sürümünün ayrıntılarını döndürür. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/yayınlar/yazma | API için yeni bir yayın oluşturur. ya da kendi tanımlayıcısı tarafından belirtilen API sürümünün ayrıntılarını güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/düzeltme/silme | Bir API 'nin tüm düzeltmelerini kaldırır |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/düzeltmeleri/okuma | Bir API 'nin tüm düzeltmelerini listeler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/şema/silme | API 'de şema yapılandırmasını siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/şemalar/okuma | API düzeyinde şema yapılandırmasını alın. veya API düzeyinde şema yapılandırmasını alın. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/şemalar/yazma | API için şema yapılandırması oluşturur veya güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/tagDescriptions/Delete | API için etiket açıklamasını silin. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/tagDescriptions/Read | API kapsamındaki tüm etiket açıklamalarını listeler. Swagger-tagDescription benzerine benzer bir model API düzeyinde tanımlandı, ancak API kapsamındaki bir etiketi Işlemlere veya Get etiketi açıklamasına atanabilir |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/tagDescriptions/Write | API 'nin kapsamında etiket açıklaması oluştur/güncelleştir. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/etiketleri/silme | Etiketi API 'den ayırın. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/Etiketler/okuma | API ile ilişkili tüm etiketleri listeler. veya API ile ilişkili Get etiketi. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/API/Etiketler/yazma | API 'ye etiket atayın. |
> | Action | Microsoft. Apimanayönetimi/hizmet/API/yazma | API Management hizmet örneğinin yeni bir API 'sini oluşturur veya güncelleştirir. veya API Management hizmet örneğinin belirtilen API 'sini güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/apisByTags/okuma | Etiketlerle ilişkili API 'lerin bir koleksiyonunu listeler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/apiVersionSets/Delete | Belirli API sürüm kümesini siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/apiVersionSets/Read | Belirtilen hizmet örneğindeki API sürüm kümelerinin bir koleksiyonunu listeler. ya da kendi tanımlayıcısı tarafından belirtilen API sürümü kümesinin ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/apiVersionSets/sürümler/okuma | Sürüm varlıklarının listesini al |
> | Action | Microsoft. Apimanayönetimi/hizmet/apiVersionSets/Write | Bir API sürüm kümesi oluşturur veya güncelleştirir. ya da kendi tanımlayıcısı tarafından belirtilen API VersionSet 'in ayrıntılarını güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/Service/applynetworkconfigurationupdates/Action | Güncelleştirilmiş ağ ayarlarını seçmek için sanal ağda çalışan Microsoft. Apimana, kaynaklarını güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/authorizationServers/Sil | Belirli yetkilendirme sunucusu örneğini siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/authorizationServers/okuma | Bir hizmet örneği içinde tanımlanan yetkilendirme sunucularının bir koleksiyonunu listeler. ya da kendi tanımlayıcısı tarafından belirtilen yetkilendirme sunucusunun ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/authorizationServers/yazma | Yeni yetkilendirme sunucusu oluşturur veya var olan bir yetkilendirme sunucusunu güncelleştirir. ya da kendi tanımlayıcısı tarafından belirtilen yetkilendirme sunucusunun ayrıntılarını güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/arka uçlar/silme | Belirtilen arka ucu siler. |
> | Action | Microsoft. Apimanamı/hizmeti/backends/Read | Belirtilen hizmet örneğindeki arka uçların koleksiyonunu listeler. ya da kendi tanımlayıcısı tarafından belirtilen arka ucun ayrıntılarını alır. |
> | Action | Microsoft. Apimanaveya hizmet/arka uçlar/geri bağlanma/eylem | Belirtilen zaman aşımından sonra, APıM proxy 'sine, arka uca yeni bir bağlantı oluşturmasını bildirir. Zaman aşımı belirtilmemişse 2 dakikalık zaman aşımı kullanılır. |
> | Action | Microsoft. Apimanamı/hizmeti/backends/Write | Bir arka uç oluşturur veya güncelleştirir. veya var olan bir arka ucu güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/yedekleme/eylem | API Management hizmetini Kullanıcı tarafından sağlanmış bir depolama hesabında belirtilen kapsayıcıya Yedekle |
> | Action | Microsoft. Apimanayönetimi/hizmeti/önbellekleri/silme | Belirli bir önbelleği siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/önbellekler/okuma | Belirtilen hizmet örneğindeki tüm dış önbellekler için bir koleksiyon listeler. ya da kendi tanımlayıcısı tarafından belirtilen önbelleğin ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/önbellekler/yazma | API Management örneğinde kullanılmak üzere bir dış önbellek oluşturur veya güncelleştirir. ya da kendi tanımlayıcısı tarafından belirtilen önbelleğin ayrıntılarını güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/sertifikalar/silme | Belirli bir sertifikayı siler. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/sertifikaları/okuma | Belirtilen hizmet örneğindeki tüm sertifikaların koleksiyonunu listeler. ya da kendi tanımlayıcısı tarafından belirtilen sertifikanın ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/sertifikaları/yazma | Arka uçta kimlik doğrulama için kullanılan sertifikayı oluşturur veya güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/contentTypes/ContentItems/Delete | Belirtilen içerik öğesini kaldırır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/contentTypes/ContentItems/Read | İçerik öğelerinin listesini döndürür veya içerik öğesi ayrıntılarını döndürür |
> | Action | Microsoft. Apimanayönetimi/hizmet/contentTypes/ContentItems/Write | Yeni içerik öğesi oluşturur veya belirtilen içerik öğesini güncelleştirir |
> | Action | Microsoft. Apimanayönetimi/hizmet/contentTypes/Read | İçerik türleri listesini döndürür |
> | Action | Microsoft.ApiManagement/service/delete | API Management hizmeti örneğini Sil |
> | Action | Microsoft. Apimanayönetimi/hizmeti/Tanılama/silme | Belirtilen tanılamayı siler. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/Tanılama/okuma | API Management hizmet örneğinin tüm tanılamalarını listeler. ya da kendi tanımlayıcısı tarafından belirtilen Tanılamanın ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/tanılama/yazma | Yeni bir tanılama oluşturur veya var olan bir tanılamayı güncelleştirir. ya da kendi tanımlayıcısı tarafından belirtilen Tanılamanın ayrıntılarını günceller. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/ağ geçitleri/eylem | Ağ Geçidi yapılandırmasını alır. veya ağ geçidi sinyalini güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/ağ geçitleri/silme | Belirli ağ geçidini siler. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/ağ geçitleri/anahtarlar/eylem | Ağ Geçidi anahtarlarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/ağ geçitleri/okuma | Hizmet örneğine kayıtlı ağ geçitlerinin bir koleksiyonunu listeler. ya da kendi tanımlayıcısı tarafından belirtilen ağ geçidinin ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/ağ geçitleri/regeneratePrimaryKey/Action | Birincil ağ geçidi anahtarını doğrulama ile oluşturulan belirteçleri yeniden oluşturur. |
> | Action | Microsoft. Apimanayönetimi/hizmet/ağ geçitleri/regenerateSecondaryKey/Action | İkincil ağ geçidi anahtarı geçersiz hale gelir, bununla oluşturulan belirteçleri oluşturur. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/ağ geçitleri/belirteç/eylem | Ağ geçidinin paylaşılan erişim yetkilendirme belirtecini alır. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/ağ geçitleri/yazma | API Management örneğinde kullanılmak üzere bir ağ geçidi oluşturur veya güncelleştirir. ya da kendi tanımlayıcısı tarafından belirtilen ağ geçidinin ayrıntılarını güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/getssotoken/Action | API Management hizmeti eski portalda yönetici olarak oturum açmak için kullanılabilecek SSO belirtecini alır |
> | Action | Microsoft. Apimanayönetimi/hizmet/gruplar/Sil | API Management hizmet örneğinin belirli bir grubunu siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/gruplar/okuma | Bir hizmet örneği içinde tanımlı grupların koleksiyonunu listeler. ya da kendi tanımlayıcısı tarafından belirtilen grubun ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/gruplar/kullanıcılar/Sil | Mevcut gruptan mevcut kullanıcıyı kaldırın. |
> | Action | Microsoft. Apimanayönetimi/hizmet/gruplar/kullanıcılar/okuma | Grupla ilişkili Kullanıcı varlıklarının koleksiyonunu listeler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/gruplar/kullanıcılar/yazma | Mevcut kullanıcıyı mevcut gruba ekle |
> | Action | Microsoft. Apimanayönetimi/hizmet/gruplar/yazma | Bir grup oluşturur veya güncelleştirir. ya da kendi tanımlayıcısı tarafından belirtilen grubun ayrıntılarını günceller. |
> | Action | Microsoft. Apimanayönetimi/hizmet/IdentityProviders/Delete | Belirtilen kimlik sağlayıcısı yapılandırmasını siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/IdentityProviders/Read | Belirtilen hizmet örneğinde yapılandırılan bir kimlik sağlayıcısı koleksiyonunu listeler. veya belirtilen hizmet örneğinde yapılandırılan kimlik sağlayıcısının yapılandırma ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/IdentityProviders/Write | IdentityProvider yapılandırmasını oluşturur veya güncelleştirir. veya var olan bir IdentityProvider yapılandırmasını güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/sorunlar/okuma | Belirtilen hizmet örneğindeki sorunların koleksiyonunu listeler. veya API Management sorun ayrıntılarını alır |
> | Action | Microsoft. Apimanayönetimi/hizmet/konumlar/networkstatus/Read | Hizmetin konuma bağlı olduğu kaynakların ağ erişim durumunu alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/Günlükçüler/Sil | Belirtilen günlükçü 'yi siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/Günlükçüler/okuma | Belirtilen hizmet örneğindeki günlükçülerin bir koleksiyonunu listeler. ya da kendi tanımlayıcısı tarafından belirtilen günlükçü ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/Günlükçüler/yazma | Bir günlükçü oluşturur veya güncelleştirir. veya var olan bir günlükçü 'yi güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/managedağıtımlar/eylem | SKU/birimleri değiştirme, API Management hizmetinin bölgesel dağıtımlarını ekleme/kaldırma |
> | Action | Microsoft. Apimanayönetimi/hizmet/networkstatus/Read | Hizmetin bağımlı olduğu kaynakların ağ erişim durumunu alır. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/bildirimleri/eylemi | Belirtilen kullanıcıya bildirim gönderir |
> | Action | Microsoft. Apimanayönetimi/hizmeti/bildirimleri/okuma | Bir hizmet örneği içinde tanımlanan özelliklerin bir koleksiyonunu listeler. ya da kendi tanımlayıcısı tarafından belirtilen bildirimin ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/bildirimleri/recipientEmails/Delete | Bildirim listesinden e-postayı kaldırır. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/bildirimleri/recipientEmails/Read | Bir bildirime abone olan bildirim alıcısı e-postalarının listesini alır. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/bildirimleri/recipientEmails/Write | Bildirim için alıcı listesine e-posta adresini ekler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/bildirimler/alıcının kullanıcıları/silme | API Management kullanıcıyı bildirim listesinden kaldırır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/bildirimler/alıcının kullanıcıları/okuma | Bildirime abone olan bildirim alıcısı kullanıcısının listesini alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/bildirimler/alıcının kullanıcıları/yazma | Bildirim için API Management kullanıcıyı alıcı listesine ekler. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/bildirimleri/yazma | API Management yayımcı bildirimi oluşturun veya güncelleştirin. |
> | Action | Microsoft. Apimanayönetimi/Service/Openıdconnectproviders/Delete | API Management hizmet örneğinin belirli OpenID Connect sağlayıcısını siler. |
> | Action | Microsoft. Apimanayönetimi/Service/Openıdconnectproviders/Read | Tüm OpenID Connect sağlayıcılarının listeleri. veya belirli bir OpenID Connect sağlayıcısı alır. |
> | Action | Microsoft. Apimanayönetimi/Service/Openıdconnectproviders/Write | OpenID Connect sağlayıcısı oluşturur veya güncelleştirir. veya belirli OpenID Connect sağlayıcısı 'nı güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/operationresults/Read | Uzun süre çalışan işlemin geçerli durumunu alır |
> | Action | Microsoft. Apimanayönetimi/hizmeti/ilkeleri/silme | API Management hizmetinin genel ilke yapılandırmasını siler. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/ilkeleri/okuma | API Management hizmetinin tüm genel Ilke tanımlarını listeler. veya API Management hizmetinin genel ilke tanımını alın. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/ilkeleri/yazma | API Management hizmetinin genel ilke yapılandırmasını oluşturur veya güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/ilke/silme | Kiracı düzeyinde ilke yapılandırmasını silme |
> | Action | Microsoft. Apimanayönetimi/hizmet/ilke/okuma | İlke yapılandırmasını kiracı düzeyinde al |
> | Action | Microsoft. Apimanayönetimi/hizmet/ilke/yazma | Kiracı düzeyinde ilke yapılandırması oluştur |
> | Action | Microsoft. Apimanaya/Service/policySnippets/Read | Tüm ilke parçacıklarını listeler. |
> | Action | Microsoft. Apimanaveya hizmet/portalsettings/Read | Portalın oturum açma ayarlarını alın veya portalın kaydolma ayarlarını alın veya portalın yetkilendirme ayarlarını alın. |
> | Action | Microsoft. Apimanaveya hizmet/portalsettings/Write | Oturum açma ayarlarını güncelleştirin. ya da oturum açma ayarlarını oluşturun veya güncelleştirin. veya kaydolma ayarlarını güncelleştirebilir veya kaydolma ayarlarını güncelleştirebilir veya yetkilendirme ayarlarını güncelleştirebilirsiniz. veya, yetkilendirme ayarlarını oluşturun veya güncelleştirin. |
> | Action | Microsoft. Apimanayönetimi/hizmet/ürünler/API/silme | Belirtilen üründen belirtilen API 'yi siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/ürünler/API/okuma | Bir ürünle ilişkili API 'lerin bir koleksiyonunu listeler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/ürünler/API/yazma | Belirtilen ürüne bir API ekler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/ürünler/silme | Ürünü silin. |
> | Action | Microsoft. Apimanayönetimi/hizmet/ürünler/gruplar/Sil | Belirtilen grup ve ürün arasındaki ilişkiyi siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/ürünler/gruplar/okuma | Belirtilen ürünle ilişkili geliştirici gruplarının koleksiyonunu listeler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/ürünler/gruplar/yazma | Belirtilen geliştirici grubu arasındaki ilişkiyi belirtilen ürüne ekler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/ürünler/ilkeler/silme | Üründe ilke yapılandırmasını siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/ürünler/ilkeler/okuma | Ürün düzeyinde ilke yapılandırmasını alın. veya ürün düzeyinde ilke yapılandırmasını alın. |
> | Action | Microsoft. Apimanayönetimi/hizmet/ürünler/ilkeler/yazma | Ürün için ilke yapılandırması oluşturur veya güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/ürünler/ilke/silme | Ürün düzeyinde ilke yapılandırmasını silme |
> | Action | Microsoft. Apimanaize/hizmet/ürünler/ilke/okuma | Ürün düzeyinde ilke yapılandırmasını al |
> | Action | Microsoft. Apimanayönetimi/hizmet/ürünler/ilke/yazma | Ürün düzeyinde ilke yapılandırması oluştur |
> | Action | Microsoft. Apimanayönetimi/hizmet/ürünler/okuma | Belirtilen hizmet örneğindeki ürünlerin koleksiyonunu listeler. ya da kendi tanımlayıcısı tarafından belirtilen ürünün ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/ürünler/abonelikler/okuma | Belirtilen ürüne yönelik aboneliklerin koleksiyonunu listeler. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/ürünleri/etiketleri/silme | Etiketi üründen ayırın. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/ürünleri/etiketleri/okuma | Ürünle ilişkili tüm etiketleri listeler. veya ürünle ilişkili Get etiketi. |
> | Action | Microsoft. Apimanayönetimi/hizmet/ürünler/Etiketler/yazma | Ürüne etiket atayın. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/ürünleri/yazma | Bir ürünü oluşturur veya güncelleştirir. veya mevcut ürün ayrıntılarını güncelleştirin. |
> | Action | Microsoft. Apimanaya/Service/productsByTags/Read | Etiketlerle ilişkili ürünlerin koleksiyonunu listeler. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/özellikleri/silme | API Management hizmet örneğinden belirli bir özelliği siler. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/özellikleri/okuma | Bir hizmet örneği içinde tanımlanan özelliklerin bir koleksiyonunu listeler. ya da kendi tanımlayıcısı tarafından belirtilen özelliğin ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/özellikleri/yazma | Bir özelliği oluşturur veya güncelleştirir. veya belirli bir özelliği güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/kotalar/dönemler/okuma | Süre için kota sayacı değerini Al |
> | Action | Microsoft. Apimanayönetimi/hizmet/kotalar/dönemler/yazma | Kota sayacı geçerli değeri ayarla |
> | Action | Microsoft. Apimanayönetimi/hizmet/kotalar/okuma | Kota değerlerini al |
> | Action | Microsoft. Apimanayönetimi/hizmet/kotalar/yazma | Kota sayacı geçerli değeri ayarla |
> | Action | Microsoft.ApiManagement/service/read | API Management hizmet örneği için meta verileri oku |
> | Action | Microsoft. Apimanamı/hizmeti/bölgeleri/okuma | Hizmetin bulunduğu tüm Azure bölgelerini listeler. |
> | Action | Microsoft. Apimanaya/hizmet/rapor/okuma | Zaman dilimlerine göre toplanan raporu alın veya coğrafi bölgeye göre toplanan raporu alın veya geliştiriciler tarafından toplanan raporları alın.<br>ya da ürünlere göre toplanan raporları alın.<br>ya da API 'Ler tarafından toplanan veya işleme göre toplanan ya da bir abonelik tarafından toplanan rapor almak için rapor alın.<br>ya da istekleri bildiren verileri al |
> | Action | Microsoft.ApiManagement/service/restore/action | API Management hizmetini Kullanıcı tarafından sağlanmış bir depolama hesabındaki belirtilen kapsayıcıdan geri yükle |
> | Action | Microsoft. Apimanayönetimi/hizmet/abonelik/silme | Belirtilen aboneliği siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/abonelikler/okuma | API Management hizmeti örneğinin tüm aboneliklerini listeler. veya belirtilen abonelik varlığını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/abonelikler/regeneratePrimaryKey/Action | API Management hizmet örneğinin mevcut aboneliğinin birincil anahtarını yeniden oluşturur. |
> | Action | Microsoft. Apimanayönetimi/hizmet/abonelikler/regenerateSecondaryKey/Action | API Management hizmet örneğinin mevcut aboneliğinin ikincil anahtarını yeniden oluşturur. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/abonelikleri/yazma | Belirtilen kullanıcının aboneliğini belirtilen ürüne oluşturur veya güncelleştirir. ya da kendi tanımlayıcısı tarafından belirtilen aboneliğin ayrıntılarını güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/tagResources/Read | Etiketlerle ilişkili kaynakların koleksiyonunu listeler. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/etiketleri/silme | API Management hizmet örneğinin belirli bir etiketini siler. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/etiketleri/okuma | Bir hizmet örneği içinde tanımlanan etiketlerin koleksiyonunu listeler. ya da kendi tanımlayıcısı tarafından belirtilen etiketin ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/etiketleri/yazma | Bir etiket oluşturur. ya da kendi tanımlayıcısı tarafından belirtilen etiketin ayrıntılarını güncelleştirir. |
> | Action | Microsoft. Apimanayönetimi/hizmet/şablonlar/silme | Varsayılan API Management e-posta şablonunu sıfırla |
> | Action | Microsoft. Apimanayönetimi/hizmeti/şablonları/okuma | Tüm e-posta şablonlarını alır veya API Management e-posta şablonu ayrıntılarını alır |
> | Action | Microsoft. Apimanayönetimi/hizmeti/şablonları/yazma | E-posta şablonu API Management API Management e-posta şablonu veya güncelleştirmeleri oluşturun veya güncelleştirin |
> | Action | Microsoft. Apimanayönetimi/hizmeti/kiracı/silme | Kiracı için ilke yapılandırmasını kaldırma |
> | Action | Microsoft. Apimanayönetimi/hizmeti/kiracı/dağıtım/eylem | Belirtilen git dalından veritabanındaki yapılandırmaya yapılan değişiklikleri uygulamak için bir dağıtım görevi çalıştırır. |
> | Action | Microsoft. Apimanayönetimi/hizmet/kiracı/operationResults/Read | İşlem sonuçlarının listesini al veya belirli bir işlemin sonucunu al |
> | Action | Microsoft. Apimanayönetimi/hizmet/kiracı/okuma | API Management hizmetinin genel ilke tanımını alın. ya da kiracı erişim bilgileri al ayrıntıları |
> | Action | Microsoft. Apimanayönetimi/hizmet/kiracı/regeneratePrimaryKey/eylem | Birincil erişim anahtarını yeniden oluştur |
> | Action | Microsoft. Apimanayönetimi/hizmet/kiracı/regenerateSecondaryKey/Action | İkincil erişim anahtarını yeniden oluştur |
> | Action | Microsoft. Apimanayönetimi/hizmet/kiracı/kaydet/eylem | Depodaki belirtilen dala yapılandırma anlık görüntüsüne sahip COMMIT oluşturur |
> | Action | Microsoft. Apimanayönetimi/hizmet/kiracı/syncState/Read | Son git eşitlemesinin durumunu al |
> | Action | Microsoft. Apimanayönetimi/hizmeti/kiracı/doğrulama/eylem | Belirtilen git dalından değişiklikleri doğrular |
> | Action | Microsoft. Apimanayönetimi/hizmet/kiracı/yazma | Kiracı için ilke yapılandırması ayarlama veya kiracı erişim bilgilerini güncelleştirme ayrıntıları |
> | Action | Microsoft.ApiManagement/service/updatecertificate/action | API Management hizmeti için SSL sertifikası yükleme |
> | Action | Microsoft.ApiManagement/service/updatehostname/action | API Management hizmeti için özel etki alanı adlarını kurma, güncelleştirme veya kaldırma |
> | Action | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/eylemi | Yeni bir kullanıcı kaydı |
> | Action | Microsoft. Apimanaveya hizmet/Kullanıcı/teyitler/gönderme/eylem | Onay gönderir |
> | Action | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/silme | Belirli kullanıcıyı siler. |
> | Action | Microsoft. Apimanayönetimi/hizmet/kullanıcılar/generateSsoUrl/eylem | Belirli bir kullanıcıyı geliştirici portalına imzalamak için bir kimlik doğrulama belirteci içeren bir yeniden yönlendirme URL 'SI alır. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/grupları/okuma | Tüm Kullanıcı gruplarını listeler. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/kimlikleri/okuma | Tüm kullanıcı kimliklerinin listesi. |
> | Action | Microsoft.ApiManagement/service/users/keys/read | Kullanıcıyla ilişkili anahtarları al |
> | Action | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/okuma | Belirtilen hizmet örneğindeki kayıtlı kullanıcıların koleksiyonunu listeler. ya da kendi tanımlayıcısı tarafından belirtilen kullanıcının ayrıntılarını alır. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/abonelikleri/okuma | Belirtilen kullanıcının aboneliklerinin koleksiyonunu listeler. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/belirteci/eylemi | Kullanıcı için paylaşılan erişim yetkilendirme belirtecini alır. |
> | Action | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/yazma | Bir kullanıcı oluşturur veya güncelleştirir. ya da kendi tanımlayıcısı tarafından belirtilen kullanıcının ayrıntılarını günceller. |
> | Action | Microsoft.ApiManagement/service/write | API Management hizmeti 'nin yeni bir örneğini oluşturma |
> | Action | Microsoft. Apimanayönetimi/kaydını kaldırma/eylem | Microsoft. Apimana, kaynak sağlayıcısı için aboneliği kayıt kaldırmayı kaldır |

## <a name="microsoftauthorization"></a>Microsoft. Authorization

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Authorization/classicAdministrators/Delete | Yöneticiyi abonelikten kaldırır. |
> | Action | Microsoft. Authorization/classicAdministrators/operationdurumlarının/Read | Aboneliğin yönetici işlem durumlarını alır. |
> | Action | Microsoft. Authorization/classicAdministrators/Read | Aboneliğin yöneticilerini okur. |
> | Action | Microsoft. Authorization/classicAdministrators/Write | Bir aboneliğe yönetici ekleyin veya bir aboneliği değiştirin. |
> | Action | Microsoft. Authorization/Denyasatamaları/silme | Belirtilen kapsamdaki reddetme atamasını silin. |
> | Action | Microsoft. Authorization/Denyasatamaları/okuma | Reddetme atama hakkında bilgi alın. |
> | Action | Microsoft. Authorization/Denyasatamaları/yazma | Belirtilen kapsamda bir reddetme ataması oluşturun. |
> | Action | Microsoft. Authorization/yükseltir Teaccess/Action | Arayan kullanıcının kiracı kapsamındaki Kullanıcı erişimi yöneticisi erişimine izin verir |
> | Action | Microsoft. Authorization/kilitleri/silme | Belirtilen kapsamdaki kilitleri silin. |
> | Action | Microsoft. Authorization/kilitleri/okuma | Belirtilen kapsamdaki kilitleri alır. |
> | Action | Microsoft. Authorization/kilitleri/yazma | Belirtilen kapsamdaki kilitleri ekleyin. |
> | Action | Microsoft. Authorization/işlemler/okuma | İşlem listesini alır |
> | Action | Microsoft. Authorization/Permissions/Read | Arayanın verilen kapsamda sahip olduğu tüm izinleri listeler. |
> | Action | Microsoft. Authorization/Poliyasatamaları/silme | Belirtilen kapsamda bir ilke atamasını silin. |
> | Action | Microsoft. Authorization/Poliyasatamaları/okuma | İlke atama hakkında bilgi alın. |
> | Action | Microsoft. Authorization/Poliyasatamaları/yazma | Belirtilen kapsamda bir ilke ataması oluşturun. |
> | Action | Microsoft. Authorization/policyDefinitions/Delete | İlke tanımını silin. |
> | Action | Microsoft. Authorization/policyDefinitions/Read | Bir ilke tanımı hakkında bilgi alın. |
> | Action | Microsoft. Authorization/policyDefinitions/Write | Özel bir ilke tanımı oluşturun. |
> | Action | Microsoft. Authorization/policySetDefinitions/Delete | İlke kümesi tanımını silin. |
> | Action | Microsoft. Authorization/policySetDefinitions/Read | İlke kümesi tanımı hakkında bilgi alın. |
> | Action | Microsoft. Authorization/policySetDefinitions/Write | Özel bir ilke kümesi tanımı oluşturun. |
> | Action | Microsoft. Authorization/providerOperations/Read | Rol tanımlarında kullanılabilen tüm kaynak sağlayıcıları için işlemleri alın. |
> | Action | Microsoft. Authorization/Roleatamaları/silme | Belirtilen kapsamda bir rol atamasını silin. |
> | Action | Microsoft. Authorization/Roleatamalar/okuma | Rol ataması hakkında bilgi alın. |
> | Action | Microsoft.Authorization/roleAssignments/write | Belirtilen kapsamda bir rol ataması oluşturun. |
> | Action | Microsoft. Authorization/roleDefinitions/Delete | Belirtilen özel rol tanımını silin. |
> | Action | Microsoft. Authorization/roleDefinitions/Read | Rol tanımı hakkında bilgi alın. |
> | Action | Microsoft. Authorization/roleDefinitions/Write | Belirtilen izinlerle ve atanabilir kapsamlarla özel bir rol tanımı oluşturun veya güncelleştirin. |

## <a name="microsoftautomation"></a>Microsoft. Automation

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Automation/automationAccounts/Agentregistrationınformation/Read | Azure Automation DSC kayıt bilgilerini okuyun |
> | Action | Microsoft. Automation/automationAccounts/Agentregistrationınformation/regenerateKey/Action | Azure Automation DSC anahtarlarını yeniden üretme isteği yazar |
> | Action | Microsoft. Automation/automationAccounts/sertifikalar/Sil | Bir Azure Otomasyonu sertifika varlığını siler |
> | Action | Microsoft. Automation/automationAccounts/sertifikalar/getCount/Action | Sertifika sayısını okur |
> | Action | Microsoft. Automation/automationAccounts/sertifikalar/okuma | Bir Azure Otomasyonu sertifika varlığı alır |
> | Action | Microsoft. Automation/automationAccounts/sertifikalar/yazma | Bir Azure Otomasyonu sertifika varlığı oluşturur veya güncelleştirir |
> | Action | Microsoft. Automation/automationAccounts/compilationjobs/Read | Azure Automation DSC derlemesini okur |
> | Action | Microsoft. Automation/automationAccounts/compilationjobs/Read | Azure Automation DSC derlemesini okur |
> | Action | Microsoft. Automation/automationAccounts/compilationjobs/Write | Azure Automation DSC derlemesini yazar |
> | Action | Microsoft. Automation/automationAccounts/compilationjobs/Write | Azure Automation DSC derlemesini yazar |
> | Action | Microsoft. Automation/automationAccounts/Configurations/içerik/okuma | Yapılandırma medyası içeriğini okur |
> | Action | Microsoft. Automation/automationAccounts/Configurations/Delete | Azure Automation DSC içeriğini siler |
> | Action | Microsoft. Automation/automationAccounts/Configurations/getCount/Action | Azure Automation DSC içeriğinin sayısını okur |
> | Action | Microsoft. Automation/automationAccounts/Configurations/Read | Azure Automation DSC içeriğini alır |
> | Action | Microsoft. Automation/automationAccounts/Configurations/Write | Azure Automation DSC içeriğini Yazar |
> | Action | Microsoft. Automation/automationAccounts/bağlantılar/Sil | Bir Azure Otomasyonu bağlantı varlığını siler |
> | Action | Microsoft. Automation/automationAccounts/Connections/getCount/Action | Bağlantı sayısını okur |
> | Action | Microsoft. Automation/automationAccounts/Connections/Read | Bir Azure Otomasyonu bağlantı varlığı alır |
> | Action | Microsoft. Automation/automationAccounts/bağlantılar/yazma | Bir Azure Otomasyonu bağlantı varlığı oluşturur veya güncelleştirir |
> | Action | Microsoft. Automation/automationAccounts/connectionTypes/Delete | Bir Azure Otomasyonu bağlantı türü varlığını siler |
> | Action | Microsoft. Automation/automationAccounts/connectionTypes/Read | Bir Azure Otomasyonu bağlantı türü varlığını alır |
> | Action | Microsoft. Automation/automationAccounts/connectionTypes/Write | Bir Azure Otomasyonu bağlantı türü varlığı oluşturur |
> | Action | Microsoft. Automation/automationAccounts/kimlik bilgileri/sil | Bir Azure Otomasyonu kimlik bilgisi varlığını siler |
> | Action | Microsoft. Automation/automationAccounts/kimlik bilgileri/getCount/Action | Kimlik bilgilerinin sayısını okur |
> | Action | Microsoft. Automation/automationAccounts/kimlik bilgileri/okuma | Bir Azure Otomasyonu kimlik bilgisi varlığını alır |
> | Action | Microsoft. Automation/automationAccounts/kimlik bilgileri/yazma | Bir Azure Otomasyonu kimlik bilgisi varlığı oluşturur veya güncelleştirir |
> | Action | Microsoft. Automation/automationAccounts/Delete | Bir Azure Otomasyonu hesabını siler |
> | Action | Microsoft. Automation/automationAccounts/hybridRunbookWorkerGroups/Delete | Karma runbook çalışanı kaynaklarını siler |
> | Action | Microsoft. Automation/automationAccounts/hybridRunbookWorkerGroups/Read | Karma runbook çalışanı kaynaklarını okur |
> | Action | Microsoft. Automation/automationAccounts/Jobs/output/Read | Bir işin çıkışını alır |
> | Action | Microsoft. Automation/automationAccounts/Jobs/okuma | Bir Azure Otomasyonu işini alır |
> | Action | Microsoft. Automation/automationAccounts/Jobs/sürdürülecek/Action | Bir Azure Otomasyonu işini sürdürür |
> | Action | Microsoft. Automation/automationAccounts/Jobs/runbookContent/Action | Azure Otomasyonu runbook 'un içeriğini iş yürütme sırasında alır |
> | Action | Microsoft. Automation/automationAccounts/Jobs/durdur/eylem | Bir Azure Otomasyonu işini durduruyor |
> | Action | Microsoft. Automation/automationAccounts/Jobs/akışlar/okuma | Bir Azure Otomasyonu iş akışı alır |
> | Action | Microsoft. Automation/automationAccounts/Jobs/akışlar/okuma | Bir Azure Otomasyonu iş akışı alır |
> | Action | Microsoft. Automation/automationAccounts/Jobs/beklet/eylem | Bir Azure Otomasyonu işini askıya alır |
> | Action | Microsoft. Automation/automationAccounts/Jobs/Write | Azure Otomasyonu işi oluşturur |
> | Action | Microsoft. Automation/automationAccounts/Jobzamanlamalar/silme | Bir Azure Otomasyonu iş zamanlamasını siler |
> | Action | Microsoft. Automation/automationAccounts/Jobzamanlamalar/okuma | Bir Azure Otomasyonu iş zamanlaması alır |
> | Action | Microsoft.Automation/automationAccounts/jobSchedules/write | Bir Azure Otomasyonu iş zamanlaması oluşturur |
> | Action | Microsoft. Automation/automationAccounts/linkedWorkspace/Read | Otomasyon hesabına bağlı çalışma alanını alır |
> | Action | Microsoft. Automation/automationAccounts/listKeys/Action | Otomasyon hesabının anahtarlarını okur |
> | Action | Microsoft. Automation/automationAccounts/modüller/etkinlikler/okuma | Azure Otomasyonu etkinliklerini alır |
> | Action | Microsoft. Automation/automationAccounts/modüller/Sil | Bir Azure Otomasyonu PowerShell modülünü siler |
> | Action | Microsoft. Automation/automationAccounts/modüller/getCount/Action | Otomasyon hesabı içindeki PowerShell modüllerinin sayısını alır |
> | Action | Microsoft. Automation/automationAccounts/modüller/okuma | Bir Azure Otomasyonu PowerShell modülünü alır |
> | Action | Microsoft. Automation/automationAccounts/modüller/yazma | Bir Azure Otomasyonu PowerShell modülü oluşturur veya güncelleştirir |
> | Action | Microsoft. Automation/automationAccounts/nodeConfigurations/Delete | Azure Automation DSC 'nin düğüm yapılandırmasını siler |
> | Action | Microsoft. Automation/automationAccounts/nodeConfigurations/rawContent/Action | Azure Automation DSC 'nin düğüm yapılandırma içeriğini okur |
> | Action | Microsoft. Automation/automationAccounts/nodeConfigurations/Read | Azure Automation DSC 'in düğüm yapılandırmasını okur |
> | Action | Microsoft. Automation/automationAccounts/nodeConfigurations/Write | Azure Automation DSC 'ın düğüm yapılandırmasını yazar |
> | Action | Microsoft. Automation/automationAccounts/nodecounts/okuma | Belirtilen tür için düğüm sayısı özetini okur |
> | Action | Microsoft. Automation/automationAccounts/Nodes/Delete | Azure Automation DSC düğümlerini siler |
> | Action | Microsoft. Automation/automationAccounts/Nodes/Read | Azure Automation DSC düğümlerini okur |
> | Action | Microsoft. Automation/automationAccounts/Nodes/raporlar/içerik/okuma | Azure Automation DSC rapor içeriğini okur |
> | Action | Microsoft. Automation/automationAccounts/Nodes/Reports/Read | Azure Automation DSC raporlarını okur |
> | Action | Microsoft. Automation/automationAccounts/Nodes/Write | Azure Automation DSC düğümlerini oluşturur veya güncelleştirir |
> | Action | Microsoft. Automation/automationAccounts/Objectveri türleri/alanlar/okuma | Azure Otomasyonu TypeFields 'i alır |
> | Action | Microsoft. Automation/automationAccounts/python2Packages/Delete | Bir Azure Otomasyonu Python 2 paketini siler |
> | Action | Microsoft. Automation/automationAccounts/python2Packages/Read | Bir Azure Otomasyonu Python 2 paketini alır |
> | Action | Microsoft. Automation/automationAccounts/python2Packages/Write | Bir Azure Otomasyonu Python 2 paketi oluşturur veya güncelleştirir |
> | Action | Microsoft. Automation/automationAccounts/python3Packages/Delete | Bir Azure Otomasyonu Python 3 paketini siler |
> | Action | Microsoft. Automation/automationAccounts/python3Packages/Read | Bir Azure Otomasyonu Python 3 paketini alır |
> | Action | Microsoft. Automation/automationAccounts/python3Packages/Write | Bir Azure Otomasyonu Python 3 paketi oluşturur veya güncelleştirir |
> | Action | Microsoft. Automation/automationAccounts/Read | Bir Azure Otomasyonu hesabını alır |
> | Action | Microsoft. Automation/automationAccounts/runbook 'lar/içerik/okuma | Bir Azure Otomasyonu runbook 'unun içeriğini alır |
> | Action | Microsoft. Automation/automationAccounts/runbook 'lar/Sil | Bir Azure Otomasyonu runbook 'unu siler |
> | Action | Microsoft. Automation/automationAccounts/runbook 'lar/taslak/içerik/yazma | Bir Azure Otomasyonu runbook taslağının içeriğini oluşturur |
> | Action | Microsoft. Automation/automationAccounts/runbook 'lar/taslak/operationResults/Read | Azure Otomasyonu runbook taslağı işlem sonuçlarını alır |
> | Action | Microsoft. Automation/automationAccounts/runbook 'lar/taslak/okuma | Bir Azure Otomasyonu runbook taslağı alır |
> | Action | Microsoft. Automation/automationAccounts/runbook 'lar/taslak/test Işi/okuma | Bir Azure Otomasyonu runbook taslağı test işini alır |
> | Action | Microsoft. Automation/automationAccounts/runbook 'lar/taslak/test Işi/özgeçmişi/eylemi | Bir Azure Otomasyonu runbook taslağı test işini sürdürür |
> | Action | Microsoft. Automation/automationAccounts/runbook 'lar/taslak/testJob/durdur/eylem | Bir Azure Otomasyonu runbook taslağı test işini durduruyor |
> | Action | Microsoft. Automation/automationAccounts/runbook 'lar/taslak/test Işi/askıya alma/eylem | Bir Azure Otomasyonu runbook taslağı test işini askıya alır |
> | Action | Microsoft. Automation/automationAccounts/runbook 'lar/taslak/test Işi/yazma | Bir Azure Otomasyonu runbook taslağı test işi oluşturur |
> | Action | Microsoft. Automation/automationAccounts/runbook 'lar/taslak/undoEdit/Action | Azure Otomasyonu runbook taslağında yapılan düzenlemeleri geri alma |
> | Action | Microsoft. Automation/automationAccounts/runbook 'lar/getCount/Action | Azure Otomasyonu runbook 'larının sayısını alır |
> | Action | Microsoft. Automation/automationAccounts/runbook 'lar/Yayımla/eylem | Bir Azure Otomasyonu runbook taslağı yayımlar |
> | Action | Microsoft. Automation/automationAccounts/runbook 'lar/okuma | Bir Azure Otomasyonu runbook 'unu alır |
> | Action | Microsoft. Automation/automationAccounts/Runbook/yazma | Bir Azure Otomasyonu runbook 'u oluşturur veya güncelleştirir |
> | Action | Microsoft. Automation/automationAccounts/zamanlamalar/silme | Bir Azure Otomasyonu zamanlama varlığını siler |
> | Action | Microsoft. Automation/automationAccounts/zamanlamalar/getCount/Action | Azure Otomasyonu zamanlamalarının sayısını alır |
> | Action | Microsoft. Automation/automationAccounts/zamanlamalar/okuma | Bir Azure Otomasyonu zamanlama varlığını alır |
> | Action | Microsoft. Automation/automationAccounts/zamanlamalar/yazma | Bir Azure Otomasyonu zamanlama varlığı oluşturur veya güncelleştirir |
> | Action | Microsoft. Automation/automationAccounts/softwareUpdateConfigurations/Delete | Bir Azure Otomasyonu yazılım güncelleştirme yapılandırmasını siler |
> | Action | Microsoft. Automation/automationAccounts/softwareUpdateConfigurations/Read | Bir Azure Otomasyonu yazılım güncelleştirme yapılandırmasını alır |
> | Action | Microsoft. Automation/automationAccounts/softwareUpdateConfigurations/Write | Azure Otomasyonu yazılım güncelleştirme yapılandırması oluşturur veya güncelleştirir |
> | Action | Microsoft. Automation/automationAccounts/STATISTICS/Read | Azure Otomasyonu Istatistiklerini alır |
> | Action | Microsoft. Automation/automationAccounts/Updatedeploymentmachinerbir/Read | Azure Otomasyonu güncelleştirme dağıtım makinesi edinme |
> | Action | Microsoft. Automation/automationAccounts/updateManagementPatchJob/Read | Bir Azure Otomasyonu güncelleştirme yönetimi düzeltme eki işini alır |
> | Action | Microsoft. Automation/automationAccounts/kullanımlar/okuma | Azure Otomasyonu kullanımını alır |
> | Action | Microsoft. Automation/automationAccounts/değişkenler/Sil | Bir Azure Otomasyonu değişken varlığını siler |
> | Action | Microsoft. Automation/automationAccounts/değişkenler/okuma | Bir Azure Otomasyonu değişken varlığını okur |
> | Action | Microsoft. Automation/automationAccounts/değişkenler/yazma | Bir Azure Otomasyonu değişken varlığı oluşturur veya güncelleştirir |
> | Action | Microsoft. Automation/automationAccounts/izleyicileri/Delete | Azure Otomasyonu İzleyicisi işini silme |
> | Action | Microsoft. Automation/automationAccounts/izleyicileri/Read | Bir Azure Otomasyonu İzleyicisi işini alır |
> | Action | Microsoft. Automation/automationAccounts/izleyicileri/start/Action | Azure Otomasyonu izleyici işi başlatma |
> | Action | Microsoft. Automation/automationAccounts/izleyicileri/durdur/eylem | Azure Otomasyonu İzleyicisi işini durdurma |
> | Action | Microsoft. Automation/automationAccounts/izleyicileri/Streams/okuma | Bir Azure Otomasyonu İzleyicisi iş akışı alır |
> | Action | Microsoft. Automation/automationAccounts/izleyicileri/watcherActions/Delete | Azure Otomasyonu İzleyicisi iş eylemlerini silme |
> | Action | Microsoft. Automation/automationAccounts/izleyicileri/watcherActions/Read | Bir Azure Otomasyonu İzleyicisi iş eylemlerini alır |
> | Action | Microsoft. Automation/automationAccounts/izleyicileri/watcherActions/Write | Azure Otomasyonu İzleyicisi iş eylemleri oluşturma |
> | Action | Microsoft. Automation/automationAccounts/izleyicileri/Write | Bir Azure Otomasyonu izleyici işi oluşturur |
> | Action | Microsoft. Automation/automationAccounts/Web kancaları/eylem | Azure Otomasyonu Web kancası için URI oluşturur |
> | Action | Microsoft. Automation/automationAccounts/Web kancaları/silme | Bir Azure Otomasyonu web kancasını siler  |
> | Action | Microsoft. Automation/automationAccounts/Web kancaları/okuma | Bir Azure Otomasyonu web kancasını okur |
> | Action | Microsoft. Automation/automationAccounts/Web kancaları/yazma | Bir Azure Otomasyonu Web kancası oluşturur veya güncelleştirir |
> | Action | Microsoft. Automation/automationAccounts/Write | Bir Azure Otomasyonu hesabı oluşturur veya güncelleştirir |
> | Action | Microsoft. Automation/işlemler/okuma | Azure Otomasyonu kaynakları için kullanılabilir Işlemleri alır |
> | Action | Microsoft. Automation/Register/ACTION | Aboneliği Azure Otomasyonu 'na kaydeder |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. AzureActiveDirectory/b2cDirectories/Delete | B2C Dizini kaynağını sil |
> | Action | Microsoft. AzureActiveDirectory/b2cDirectories/Read | B2C Dizini kaynağını görüntüle |
> | Action | Microsoft. AzureActiveDirectory/b2cDirectories/Write | B2C dizin kaynağı oluştur veya güncelleştir |
> | Action | Microsoft. AzureActiveDirectory/b2ctenants/Read | Kullanıcının üye olduğu tüm B2C kiracılarını listeler |
> | Action | Microsoft. AzureActiveDirectory/işlemler/okuma | Microsoft. AzureActiveDirectory kaynak sağlayıcısı için kullanılabilen tüm API işlemlerini okuyun |
> | Action | Microsoft. AzureActiveDirectory/Register/ACTION | Microsoft. AzureActiveDirectory kaynak sağlayıcısı için aboneliği kaydetme |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. AzureStack/Işlemler/okuma | Bir kaynak sağlayıcısı işleminin özelliklerini alır |
> | Action | Microsoft. AzureStack/Register/Action | Aboneliği Microsoft. AzureStack kaynak sağlayıcısına kaydeder |
> | Action | Microsoft. AzureStack/kayıt/müşteri abonelikleri/silme | Bir Azure Stack müşteri aboneliğini siler |
> | Action | Microsoft. AzureStack/kayıt/müşteri abonelikleri/okuma | Azure Stack müşteri aboneliğinin özelliklerini alır |
> | Action | Microsoft. AzureStack/kayıt/müşteri abonelikleri/yazma | Bir Azure Stack müşteri aboneliği oluşturur veya güncelleştirir |
> | Action | Microsoft. AzureStack/kayıt/silme | Azure Stack kaydını siler |
> | Action | Microsoft. AzureStack/kayıtlar/getActivationKey/eylem | En son Azure Stack etkinleştirme anahtarını alır |
> | Action | Microsoft.AzureStack/registrations/products/listDetails/action | Azure Stack Market ürünü için genişletilmiş ayrıntıları alır |
> | Action | Microsoft.AzureStack/registrations/products/read | Azure Stack Market ürününün özelliklerini alır |
> | Action | Microsoft. AzureStack/kayıtlar/ürünler/uploadProductLog/Action | Market ürün işlemi durumunu ve zaman damgasını Azure Stack Kaydet |
> | Action | Microsoft.AzureStack/registrations/read | Azure Stack kaydın özelliklerini alır |
> | Action | Microsoft. AzureStack/kayıtlar/yazma | Azure Stack kaydı oluşturur veya güncelleştirir |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Batch/batchAccounts/uygulamalar/Sil | Bir uygulamayı siler |
> | Action | Microsoft. Batch/batchAccounts/uygulamalar/okuma | Uygulamaları listeler veya bir uygulamanın özelliklerini alır |
> | Action | Microsoft. Batch/batchAccounts/uygulamalar/sürümler/etkinleştir/eylem | Bir uygulama paketini etkinleştirir |
> | Action | Microsoft. Batch/batchAccounts/uygulamalar/sürümler/Sil | Uygulama paketini siler |
> | Action | Microsoft. Batch/batchAccounts/uygulamalar/sürümler/okuma | Uygulama paketinin özelliklerini alır |
> | Action | Microsoft. Batch/batchAccounts/uygulamalar/sürümler/yazma | Yeni bir uygulama paketi oluşturur veya var olan bir uygulama paketini güncelleştirir |
> | Action | Microsoft. Batch/batchAccounts/uygulamalar/yazma | Yeni bir uygulama oluşturur veya var olan bir uygulamayı güncelleştirir |
> | Action | Microsoft. Batch/batchAccounts/certificateOperationResults/Read | Bir Batch hesabında uzun süre çalışan bir sertifika işleminin sonuçlarını alır |
> | Action | Microsoft. Batch/batchAccounts/Certificates/cancelDelete/Action | Batch hesabındaki bir sertifikanın başarısız silme işlemini iptal eder |
> | Action | Microsoft. Batch/batchAccounts/sertifikalar/Sil | Bir toplu Iş hesabındaki bir sertifikayı siler |
> | Action | Microsoft. Batch/batchAccounts/sertifikalar/okuma | Bir Batch hesabındaki sertifikaları listeler veya bir sertifikanın özelliklerini alır |
> | Action | Microsoft. Batch/batchAccounts/sertifikalar/yazma | Batch hesabında yeni bir sertifika oluşturur veya mevcut bir sertifikayı güncelleştirir |
> | Action | Microsoft. Batch/batchAccounts/Delete | Batch hesabını siler |
> | DataAction | Microsoft. Batch/batchAccounts/Jobs/Delete | Batch hesabından bir işi siler |
> | DataAction | Microsoft. Batch/batchAccounts/Jobs/okuma | Bir Batch hesabındaki işleri listeler veya bir işin özelliklerini alır |
> | DataAction | Microsoft. Batch/batchAccounts/Jobs/Write | Batch hesabında yeni bir iş oluşturur veya var olan bir işi güncelleştirir |
> | DataAction | Microsoft. Batch/batchAccounts/Jobzamanlamalar/silme | Bir Batch hesabından iş zamanlaması siler |
> | DataAction | Microsoft. Batch/batchAccounts/Jobzamanlamalar/okuma | Bir Batch hesabındaki iş zamanlamalarını listeler veya iş zamanlamasının özelliklerini alır |
> | DataAction | Microsoft. Batch/batchAccounts/Jobzamanlamalar/yazma | Batch hesabında yeni bir iş zamanlaması oluşturur veya var olan bir iş zamanlamasını güncelleştirir |
> | Action | Microsoft. Batch/batchAccounts/ListKeys/ACTION | Batch hesabı için erişim anahtarlarını listeler |
> | Action | Microsoft. Batch/batchAccounts/operationResults/Read | Uzun süre çalışan bir Batch hesabı işleminin sonuçlarını alır |
> | Action | Microsoft. Batch/batchAccounts/poolOperationResults/Read | Bir Batch hesabında uzun süre çalışan havuz işleminin sonuçlarını alır |
> | Action | Microsoft. Batch/batchAccounts/havuzlar/Delete | Bir toplu Iş hesabındaki havuzu siler |
> | Action | Microsoft. Batch/batchAccounts/havuzlar/Disableotomatik ölçeklendirme/eylem | Batch hesap havuzu için otomatik ölçeklendirmeyi devre dışı bırakır |
> | Action | Microsoft. Batch/batchAccounts/havuzlar/Read | Bir Batch hesabındaki havuzları listeler veya havuzun özelliklerini alır |
> | Action | Microsoft. Batch/batchAccounts/havuzlar/stopResize/eylem | Batch hesap havuzunda devam eden bir yeniden boyutlandırma işlemini durduruyor |
> | Action | Microsoft. Batch/batchAccounts/havuzlar/Write | Batch hesabında yeni bir havuz oluşturur veya var olan bir havuzu güncelleştirir |
> | Action | Microsoft. Batch/batchAccounts/okuma | Batch hesaplarını listeler veya Batch hesabının özelliklerini alır |
> | Action | Microsoft. Batch/batchAccounts/regeneratekeys/ACTION | Batch hesabı için erişim anahtarlarını yeniden oluşturur |
> | Action | Microsoft. Batch/batchAccounts/syncAutoStorageKeys/Action | Bir Batch hesabı için yapılandırılan otomatik depolama hesabı için erişim anahtarlarını eşitler |
> | Action | Microsoft. Batch/batchAccounts/Write | Yeni bir Batch hesabı oluşturur veya var olan bir Batch hesabını güncelleştirir |
> | Action | Microsoft. Batch/konumlar/accountOperationResults/Read | Uzun süre çalışan bir Batch hesabı işleminin sonuçlarını alır |
> | Action | Microsoft. Batch/Locations/Checknameavaılabılıty/Action | Hesap adının geçerli ve kullanımda olup olmadığını denetler. |
> | Action | Microsoft. Batch/konumlar/kotalar/okuma | Belirtilen Azure bölgesindeki belirtilen aboneliğin toplu kotalarını alır |
> | Action | Microsoft. Batch/işlemler/okuma | Microsoft. Batch kaynak sağlayıcısında kullanılabilir işlemleri listeler |
> | Action | Microsoft. Batch/Register/ACTION | Batch kaynak sağlayıcısı için aboneliği kaydeder ve Batch hesaplarının oluşturulmasını etkinleştirilir |
> | Action | Microsoft. Batch/Unregister/eylem | Batch kaynak sağlayıcısı için aboneliğin kaydını siler Batch hesaplarının oluşturulmasını önler |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. faturalandırma/billingAccounts/sözleşmeleri/okuma |  |
> | Action | Microsoft. faturalandırma/billingAccounts/billingPermissions/Read |  |
> | Action | Microsoft. faturalandırma/billingAccounts/Billingprofiller/billingPermissions/Read |  |
> | Action | Microsoft. faturalandırma/billingAccounts/billingProfiles/müşteriler/okuma |  |
> | Action | Microsoft. faturalandırma/billingAccounts/billingProfiles/faturalar/fiyat listesi/download/ACTION |  |
> | Action | Microsoft. faturalandırma/billingAccounts/billingProfiles/ınvoicesections/billingPermissions/Read |  |
> | Action | Microsoft. faturalandırma/billingAccounts/billingProfiles/ınvoicesections/Read |  |
> | Action | Microsoft. faturalandırma/billingAccounts/billingProfiles/ınvoicesections/Write |  |
> | Action | Microsoft. faturalandırma/billingAccounts/billingProfiles/fiyat listesi/download/Action |  |
> | Action | Microsoft. faturalandırma/billingAccounts/billingProfiles/okuma |  |
> | Action | Microsoft. faturalandırma/billingAccounts/Billingprofiller/Write |  |
> | Action | Microsoft. faturalandırma/billingAccounts/müşteriler/billingPermissions/Read |  |
> | Action | Microsoft. faturalandırma/billingAccounts/müşteriler/okuma |  |
> | Action | Microsoft. faturalandırma/billingAccounts/departmanlar/okuma |  |
> | Action | Microsoft. faturalandırma/billingAccounts/Kayıthesapsayısı/billingPermissions/Read |  |
> | Action | Microsoft. faturalandırma/billingAccounts/KayıtSayısı/okuma |  |
> | Action | Microsoft. faturalandırma/billingAccounts/kayıt/veya tüm Izinler/okuma |  |
> | Action | Microsoft. faturalandırma/billingAccounts/ürünler/okuma |  |
> | Action | Microsoft. faturalandırma/billingAccounts/okuma |  |
> | Action | Microsoft. faturalandırma/billingAccounts/Write |  |
> | Action | Microsoft. faturalandırma/departmanlar/okuma |  |
> | Action | Microsoft. faturalandırma/Register/ACTION |  |
> | Action | Microsoft. faturalandırma/validateAddress/Write |  |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. BingMaps/Mapapsıs/Delete | İşlemi Siler |
> | Action | Microsoft. BingMaps/Mapapsıs/Listgizlilikler/Action | Parolaları Listeler |
> | Action | Microsoft. BingMaps/Mapapsıs/listSingleSignOnToken/Action | Kaynak Için çoklu oturum açma yetkilendirme belirtecini oku |
> | Action | Microsoft. BingMaps/Mapapsıs/okuma | İşlemi Okur |
> | Action | Microsoft. BingMaps/Mapapsıs/regenerateKey/Action | Anahtarı Yeniden Oluşturur |
> | Action | Microsoft. BingMaps/Mapapsıs/Write | İşlemi Yazar |
> | Action | Microsoft. BingMaps/Işlemler/okuma | İşlemin açıklaması. |

## <a name="microsoftblockchain"></a>Microsoft. Blockzinciri

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Blockzincirine/blockchainMembers/Delete | Varolan bir blok zinciri üyesini siler. |
> | Action | Microsoft. Blockzincirine/blockchainMembers/listApiKeys/eylem | Varolan blok zinciri üye API anahtarlarını alır veya listeler. |
> | Action | Microsoft. Blockzincirine/blockchainMembers/Read | Varolan blok zinciri üyelerini alır veya listeler. |
> | DataAction | Microsoft. Blockzincirini/blockchainMembers/transactionNodes/Connect/ACTION | Bir blok zinciri üye Işlem düğümüne bağlanır. |
> | Action | Microsoft. Blockzincirini/blockchainMembers/transactionNodes/Delete | Varolan bir blok zinciri üye Işlem düğümünü siler. |
> | Action | Microsoft. Blockzincirine/blockchainMembers/transactionNodes/listApiKeys/eylem | Varolan blok zinciri üye Işlem düğümü API anahtarlarını alır veya listeler. |
> | Action | Microsoft. Blockzincirini/blockchainMembers/transactionNodes/Read | Varolan blok zinciri üye Işlem düğümlerini alır veya listeler. |
> | Action | Microsoft. Blockzincirine/blockchainMembers/transactionNodes/Write | Bir blok zinciri üye Işlem düğümü oluşturur veya güncelleştirir. |
> | Action | Microsoft. Blockzincirine/blockchainMembers/Write | Bir blok zinciri üyesi oluşturur veya güncelleştirir. |
> | Action | Microsoft. Blockzinciri/konumlar/blockchainMemberOperationResults/Read | Blok zinciri üyelerinin Işlem sonuçlarını alır. |
> | Action | Microsoft. Blockzincirine/konumlarına/Checknameavaılabılıty/Action | Kaynak adının geçerli olduğunu ve kullanımda olup olmadığını denetler. |
> | Action | Microsoft. blok zinciri/işlemler/okuma | Microsoft blok zinciri kaynak sağlayıcısındaki tüm Işlemleri listeleyin. |
> | Action | Microsoft. Blockzincirine/Register/ACTION | Blok zinciri kaynak sağlayıcısı için aboneliği kaydeder. |

## <a name="microsoftblueprint"></a>Microsoft. Blueprint

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Blueprint/Blueprintasbir/atama/Read | Herhangi bir şema yapıtını okuyun |
> | Action | Microsoft. Blueprint/Blueprintasbir/Delete | Herhangi bir şema yapıtını silin |
> | Action | Microsoft. Blueprint/Blueprintasbir/Read | Herhangi bir şema yapıtını okuyun |
> | Action | Microsoft. Blueprint/Blueprintasbir/whoisblueprınt/Action | Azure şemaları hizmet sorumlusu nesne kimliğini alın. |
> | Action | Microsoft. Blueprint/Blueprintasbir/Write | Herhangi bir şema yapıtı oluşturun veya güncelleştirin |
> | Action | Microsoft. Blueprint/planlar/yapıtlar/silme | Herhangi bir şema yapıtını silin |
> | Action | Microsoft. Blueprint/planlar/yapıtlar/okuma | Herhangi bir şema yapıtını okuyun |
> | Action | Microsoft. Blueprint/planlar/yapıtlar/yazma | Herhangi bir şema yapıtı oluşturun veya güncelleştirin |
> | Action | Microsoft. BLUEPRINT/planlar/silme | Herhangi bir şemayı silin |
> | Action | Microsoft. Blueprint/planlar/okuma | Herhangi bir şemayı okuyun |
> | Action | Microsoft. Blueprint/planlar/sürümler/yapıtlar/okuma | Herhangi bir şema yapıtını okuyun |
> | Action | Microsoft. Blueprint/planlar/sürümler/Sil | Herhangi bir şemayı silin |
> | Action | Microsoft. Blueprint/planlar/sürümler/okuma | Herhangi bir şemayı okuyun |
> | Action | Microsoft. Blueprint/planlar/sürümler/Write | Herhangi bir şemayı oluşturun veya güncelleştirin |
> | Action | Microsoft. Blueprint/planlar/yazma | Herhangi bir şemayı oluşturun veya güncelleştirin |
> | Action | Microsoft. BLUEPRINT/Register/Action | Azure şemaları kaynak sağlayıcısını kaydeder |

## <a name="microsoftbotservice"></a>Microsoft. BotService

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. BotService/botServices/kanallar/Delete | Bir bot hizmetini silme |
> | Action | Microsoft. BotService/botServices/kanallar/Read | Bir bot hizmetini okuyun |
> | Action | Microsoft. BotService/botServices/kanallar/Write | Bir bot hizmeti yazın |
> | Action | Microsoft. BotService/botServices/Connections/Delete | Bir bot hizmetini silme |
> | Action | Microsoft. BotService/botServices/Connections/Read | Bir bot hizmetini okuyun |
> | Action | Microsoft. BotService/botServices/Connections/Write | Bir bot hizmeti yazın |
> | Action | Microsoft. BotService/botServices/Delete | Bir bot hizmetini silme |
> | Action | Microsoft. BotService/botServices/Read | Bir bot hizmetini okuyun |
> | Action | Microsoft. BotService/botServices/Write | Bir bot hizmeti yazın |
> | Action | Microsoft. BotService/konumlar/operationresults/Read | Zaman uyumsuz bir işlemin durumunu okuyun |
> | Action | Microsoft. BotService/Işlemler/okuma | Tüm kaynak türleri için işlemleri okuyun |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Cache/checknameavaılabılıty/Action | Bir adın yeni bir Redis Cache kullanılmak üzere kullanılabilir olup olmadığını denetler |
> | Action | Microsoft. Cache/Locations/operationresults/Read | ' Location ' üstbilgisinin istemciye daha önce döndürüldüğü uzun süren bir işlemin sonucunu alır |
> | Action | Microsoft. Cache/işlemler/okuma | ' Microsoft. cache ' sağlayıcısının desteklediği işlemleri listeler. |
> | Action | Microsoft. Cache/redsıs/Delete | Redis Cache'nin tamamını sil |
> | Action | Microsoft. Cache/redsıs/Export/ACTION | Redsıs verilerini belirtilen biçimde ön ek depolama bloblarına aktar |
> | Action | Microsoft. Cache/redsıs/firewallRules/Delete | Redis Cache IP güvenlik duvarı kurallarını silme |
> | Action | Microsoft. Cache/redsıs/firewallRules/Read | Redis Cache IP güvenlik duvarı kurallarını al |
> | Action | Microsoft. Cache/redsıs/firewallRules/Write | Redis Cache IP güvenlik duvarı kurallarını düzenleme |
> | Action | Microsoft. Cache/redsıs/forceReboot/Action | Bir önbellek örneğini yeniden başlatmaya zorla, büyük olasılıkla veri kaybı olabilir. |
> | Action | Microsoft. Cache/redsıs/Import/Action | Birden çok blobdan belirtilen biçimdeki verileri Redsıs 'e aktar |
> | Action | Microsoft. Cache/redsıs/LinkedServers/Delete | Redis Cache bağlı sunucuyu silme |
> | Action | Microsoft. Cache/redsıs/LinkedServers/okuma | Bir redsıs önbelleğiyle ilişkili bağlı sunucuları alın. |
> | Action | Microsoft. Cache/redsıs/LinkedServers/Write | Redis Cache bağlı sunucu ekleme |
> | Action | Microsoft. Cache/redsıs/listKeys/ACTION | Yönetim portalında Redis Cache erişim anahtarlarının değerini görüntüleme |
> | Action | Microsoft. Cache/redsıs/listUpgradeNotifications/Read | Önbellek kiracısı için en son yükseltme bildirimlerini listeleyin. |
> | Action | Microsoft. Cache/redsıs/metricDefinitions/okuma | Redis Cache için kullanılabilir ölçümleri alır |
> | Action | Microsoft. Cache/Reda/Patchzamanlamalar/silme | Redis Cache Patch zamanlamasını silme |
> | Action | Microsoft. Cache/redsıs/Patchzamanlamalar/okuma | Redis Cache düzeltme eki uygulama zamanlamasını alır |
> | Action | Microsoft. Cache/redsıs/Patchzamanlamalar/yazma | Redis Cache düzeltme eki uygulama zamanlamasını değiştirme |
> | Action | Microsoft. Cache/redsıs/Read | Yönetim portalında Redis Cache ayarlarını ve yapılandırmasını görüntüleme |
> | Action | Microsoft. Cache/redsıs/regenerateKey/Action | Yönetim portalında Redis Cache erişim anahtarlarının değerini değiştirme |
> | Action | Microsoft. Cache/redsıs/start/Action | Bir önbellek örneği başlatın. |
> | Action | Microsoft. Cache/reddir/durdur/eylem | Bir önbellek örneğini durdurun. |
> | Action | Microsoft. Cache/redsıs/Write | Yönetim portalında Redis Cache ayarlarını ve yapılandırmasını değiştirme |
> | Action | Microsoft. Cache/Register/ACTION | ' Microsoft. cache ' kaynak sağlayıcısını bir abonelikle kaydeder |
> | Action | Microsoft. Cache/Unregister/eylem | ' Microsoft. cache ' kaynak sağlayıcısının kaydını abonelikle siler |

## <a name="microsoftcapacity"></a>Microsoft. Capacity

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Capacity/appliedreservations/Read | Tüm rezervasyonları oku |
> | Action | Microsoft. Capacity/calculateexchange/Action | Exchange tutarını ve yeni satın alma fiyatını hesaplar ve ilke hatalarını döndürür. |
> | Action | Microsoft. Capacity/calculateprice/Action | Herhangi bir rezervasyon fiyatını hesapla |
> | Action | Microsoft. Capacity/kataloglar/okuma | Ayırma kataloğunu okuyun |
> | Action | Microsoft. Capacity/checktekliflere/eyleme | Tüm abonelik tekliflerini denetleyin |
> | Action | Microsoft. Capacity/checkscopes/Action | Herhangi bir aboneliği denetleyin |
> | Action | Microsoft. Capacity/ticari cıalrezervationorders/Read | Herhangi bir kiracıda oluşturulan rezervasyon emirlerini al |
> | Action | Microsoft. Capacity/Exchange/eylem | Herhangi bir ayırmayı Exchange |
> | Action | Microsoft. Capacity/işlemler/okuma | Herhangi bir Işlemi okuyun |
> | Action | Microsoft. Capacity/Register/ACTION | Kapasite kaynak sağlayıcısını kaydeder ve kapasite kaynaklarının oluşturulmasına izin vermez. |
> | Action | Microsoft. Capacity/rezervationorders/ACTION | Tüm ayırmaları güncelleştirme |
> | Action | Microsoft. Capacity/rezervationorders/availablescopes/eylem | Kullanılabilir herhangi bir kapsamı bulun |
> | Action | Microsoft. Capacity/rezervationorders/hesaplaizterefund/eylem | Para iadesi tutarını ve yeni satın alma fiyatını hesaplar ve ilke hatalarını döndürür. |
> | Action | Microsoft. Capacity/rezervationorders/Delete | Tüm ayırmaları Sil |
> | Action | Microsoft. Capacity/rezervationorders/Merge/Action | Tüm ayırmaları birleştirme |
> | Action | Microsoft. Capacity/rezervationorders/Read | Tüm rezervasyonları oku |
> | Action | Microsoft. Capacity/rezervationorders/rezervasyonlar/eylem | Tüm ayırmaları güncelleştirme |
> | Action | Microsoft. Capacity/rezervationorders/rezervasyonlar/Delete | Tüm ayırmaları Sil |
> | Action | Microsoft. Capacity/rezervationorders/rezervasyonlar/okundu | Tüm rezervasyonları oku |
> | Action | Microsoft. Capacity/rezervationorders/rezervasyonlar/düzeltmeler/okuma | Tüm rezervasyonları oku |
> | Action | Microsoft. Capacity/rezervationorders/rezervasyonlar/Write | Herhangi bir ayırma oluşturun |
> | Action | Microsoft. Capacity/rezervationorders/Return/ACTION | Herhangi bir ayırmayı döndürme |
> | Action | Microsoft. Capacity/rezervationorders/Split/ACTION | Tüm ayırmaları Böl |
> | Action | Microsoft. Capacity/rezervationorders/Swap/ACTION | Tüm ayırmaları değiştirme |
> | Action | Microsoft. Capacity/rezervationorders/Write | Herhangi bir ayırma oluşturun |
> | Action | Microsoft. Capacity/kiracılar/Register/Action | Tüm kiracıyı Kaydet |
> | Action | Microsoft. Capacity/Unregister/eylem | Tüm kiracının kaydını sil |
> | Action | Microsoft. Capacity/validatereservationorder/eylem | Tüm ayırmaları doğrulama |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. CDN/Checknameavaılabılıty/Action |  |
> | Action | Microsoft. CDN/CheckResourceUsage/eylem |  |
> | Action | Microsoft. CDN/edgenodes/Delete |  |
> | Action | Microsoft. CDN/edgenodes/okuma |  |
> | Action | Microsoft. CDN/edgenodes/yazma |  |
> | Action | Microsoft. CDN/operationresults/Delete |  |
> | Action | Microsoft. CDN/operationresults/profileresults/CheckResourceUsage/Action |  |
> | Action | Microsoft. CDN/operationresults/profileresults/silme |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/CheckResourceUsage/Action |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/customdomainresults/Delete |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/customdomainresults/DisableCustomHttps/Action |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/customdomainresults/EnableCustomHttps/Action |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/customdomainresults/Read |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/customdomainresults/Write |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/Delete |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/Load/Action |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/originresults/Delete |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/originresults/Read |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/originresults/Write |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/Temizleme/eylem |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/Read |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/start/Action |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/durdur/Action |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/ValidateCustomDomain/ACTION |  |
> | Action | Microsoft. CDN/operationresults/profileresults/endpointresults/Write |  |
> | Action | Microsoft. CDN/operationresults/profileresults/GenerateSsoUri/eylem |  |
> | Action | Microsoft. CDN/operationresults/profileresults/GetSupportedOptimizationTypes/eylem |  |
> | Action | Microsoft. CDN/operationresults/profileresults/okuma |  |
> | Action | Microsoft. CDN/operationresults/profileresults/yazma |  |
> | Action | Microsoft. CDN/operationresults/Read |  |
> | Action | Microsoft. CDN/operationresults/Write |  |
> | Action | Microsoft. CDN/işlemler/okuma |  |
> | Action | Microsoft. CDN/Profiles/CheckResourceUsage/Action |  |
> | Action | Microsoft. CDN/profiller/silme |  |
> | Action | Microsoft. CDN/profiller/uç noktaları/CheckResourceUsage/ACTION |  |
> | Action | Microsoft. CDN/profiller/uç noktaları/customdomains/Delete |  |
> | Action | Microsoft. CDN/Profiles/uç noktaları/customdomains/DisableCustomHttps/eylem |  |
> | Action | Microsoft. CDN/Profiles/uç noktaları/customdomains/EnableCustomHttps/eylem |  |
> | Action | Microsoft. CDN/Profiles/uç noktaları/customdomains/Read |  |
> | Action | Microsoft. CDN/Profiles/uç noktaları/customdomains/Write |  |
> | Action | Microsoft. CDN/profiller/uç noktaları/silme |  |
> | Action | Microsoft. CDN/profiller/uç noktaları/yükleme/eylem |  |
> | Action | Microsoft. CDN/profiller/uç noktaları/kaynakları/Sil |  |
> | Action | Microsoft. CDN/profiller/uç noktaları/kaynaklar/okuma |  |
> | Action | Microsoft. CDN/profiller/uç noktaları/çıkış/yazma |  |
> | Action | Microsoft. CDN/profiller/uç noktaları/Temizleme/eylem |  |
> | Action | Microsoft. CDN/profiller/uç noktaları/okuma |  |
> | Action | Microsoft. CDN/profiller/uç noktaları/Başlat/eylem |  |
> | Action | Microsoft. CDN/profiller/uç noktaları/durdur/eylem |  |
> | Action | Microsoft. CDN/Profiles/uç noktaları/ValidateCustomDomain/ACTION |  |
> | Action | Microsoft. CDN/profiller/uç noktaları/yazma |  |
> | Action | Microsoft. CDN/Profiles/GenerateSsoUri/eylem |  |
> | Action | Microsoft. CDN/Profiles/GetSupportedOptimizationTypes/Action |  |
> | Action | Microsoft. CDN/profiller/okuma |  |
> | Action | Microsoft. CDN/profiller/yazma |  |
> | Action | Microsoft. CDN/Register/ACTION | CDN kaynak sağlayıcısı için aboneliği kaydeder ve CDN profillerinin oluşturulmasına izin vermez. |
> | Action | Microsoft. CDN/Validatearaştırma/eylem |  |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. CertificateRegistration/certificateOrders/sertifikalar/Sil | Mevcut bir sertifikayı silme |
> | Action | Microsoft. CertificateRegistration/certificateOrders/sertifikalar/okuma | Sertifika listesini al |
> | Action | Microsoft. CertificateRegistration/certificateOrders/Certificates/Write | Yeni bir sertifika ekleme veya var olan bir sertifikayı güncelleştirme |
> | Action | Microsoft. CertificateRegistration/certificateOrders/Delete | Var olan bir AppServiceCertificate silme |
> | Action | Microsoft. CertificateRegistration/certificateOrders/Read | CertificateOrders listesini alın |
> | Action | Microsoft. CertificateRegistration/certificateOrders/restart/Action | Mevcut bir sertifika siparişi 'ı yeniden gönderin |
> | Action | Microsoft. CertificateRegistration/certificateOrders/Yenile/eylem | Mevcut bir sertifika siparişini Yenile |
> | Action | Microsoft. CertificateRegistration/certificateOrders/resendEmail/Action | Sertifika e-postasını yeniden gönder |
> | Action | Microsoft. CertificateRegistration/certificateOrders/Resendrequestemaıls/eylem | İstek e-postalarını başka bir e-posta adresine yeniden |
> | Action | Microsoft. CertificateRegistration/certificateOrders/Resendrequestemaıls/eylem | Verilen App Service Sertifikası için site mühürlemek alın |
> | Action | Microsoft. CertificateRegistration/certificateOrders/retrieveCertificateActions/Action | Sertifika eylemlerinin listesini al |
> | Action | Microsoft. CertificateRegistration/certificateOrders/retrieveEmailHistory/Action | Sertifika e-posta geçmişini al |
> | Action | Microsoft. CertificateRegistration/certificateOrders/Verifydomainsahiplik/eylem | Etki alanı sahipliğini doğrulama |
> | Action | Microsoft. CertificateRegistration/certificateOrders/Write | Yeni bir sertifika siparişi ekleme veya var olanı güncelleştirme |
> | Action | Microsoft. CertificateRegistration/işlemler/okuma | App Service sertifika kaydından tüm işlemleri Listele |
> | Action | Microsoft. CertificateRegistration/provisionGlobalAppServicePrincipalInUserTenant/Action | Hizmet uygulaması sorumlusu için hizmet sorumlusu sağlama |
> | Action | Microsoft. CertificateRegistration/Register/ACTION | Abonelik için Microsoft sertifikaları kaynak sağlayıcısını kaydetme |
> | Action | Microsoft. CertificateRegistration/Validatecertificateregistrationınformation/Action | Sertifika satın alma nesnesini göndermeden doğrula |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. ClassicCompute/Capabilities/Read | Özellikleri gösterir |
> | Action | Microsoft. ClassicCompute/Checkdomainnameavaılabılıty/Action | Belirli bir etki alanı adının kullanılabilirliğini denetler. |
> | Action | Microsoft. ClassicCompute/Checkdomainnameavaılabılıty/Read | Belirli bir etki alanı adının kullanılabilirliğini alır. |
> | Action | Microsoft. ClassicCompute/domainNames/etkin/Write | Etkin etki alanı adını ayarlar. |
> | Action | Microsoft. ClassicCompute/domainNames/kullanılabilirliği \ kümeler/okundu | Kaynak için kullanılabilirlik kümesini görüntüleyin. |
> | Action | Microsoft. ClassicCompute/domainNames/yetenekler/okuma | Etki alanı adı özelliklerini gösterir |
> | Action | Microsoft. ClassicCompute/domainNames/Delete | Kaynaklar için etki alanı adlarını kaldırın. |
> | Action | Microsoft. ClassicCompute/domainNames/deploymentyuvaları/okuma | Dağıtım yuvalarını gösterir. |
> | Action | Microsoft. ClassicCompute/domainNames/deploymentyuvaları/roller/okuma | Etki alanı adının dağıtım yuvasında rol al |
> | Action | Microsoft. ClassicCompute/domainNames/deploymentyuvaları/roller/roleınstances/Read | Etki alanı adının dağıtım yuvasındaki rolün rol örneğini al |
> | Action | Microsoft. ClassicCompute/domainNames/deploymentyuvaları/durumu/okuma | Dağıtım yuvası durumunu alın. |
> | Action | Microsoft. ClassicCompute/domainNames/deploymentyuvaları/durum/yazma | Dağıtım yuvası durumunu ekleyin. |
> | Action | Microsoft. ClassicCompute/domainNames/deploymentyuvaları/upgradedomain/Read | Etki alanı adındaki dağıtım yuvası için yükseltme etki alanını al |
> | Action | Microsoft. ClassicCompute/domainNames/deploymentyuvaları/upgradedomain/Write | Etki alanı adında dağıtım yuvası için yükseltme etki alanını güncelleştir |
> | Action | Microsoft. ClassicCompute/domainNames/deploymentyuvaları/yazma | Dağıtımı oluşturur veya güncelleştirir. |
> | Action | Microsoft. ClassicCompute/domainNames/Extensions/Sil | Etki alanı adı uzantılarını kaldırın. |
> | Action | Microsoft. ClassicCompute/domainNames/Extensions/Operationdurumlarının/Read | Etki alanı adları uzantılarının işlem durumunu okur. |
> | Action | Microsoft. ClassicCompute/domainNames/Extensions/Read | Etki alanı adı uzantılarını döndürür. |
> | Action | Microsoft. ClassicCompute/domainNames/Extensions/Write | Etki alanı adı uzantılarını ekleyin. |
> | Action | Microsoft. ClassicCompute/domainNames/internalLoadBalancers/Delete | Yeni bir iç yük dengesini kaldırın. |
> | Action | Microsoft. ClassicCompute/domainNames/internalLoadBalancers/Operationdurumlarının/Read | Etki alanı adları iç yük dengeleyiciler için işlem durumunu okur. |
> | Action | Microsoft. ClassicCompute/domainNames/internalLoadBalancers/Read | İç yük dengeleyicileri alır. |
> | Action | Microsoft. ClassicCompute/domainNames/internalLoadBalancers/Write | Yeni bir iç Yük Dengeleme oluşturur. |
> | Action | Microsoft. ClassicCompute/domainNames/loadBalancedEndpointSets/Operationdurumlarının/Read | Etki alanı adları yük dengeli uç nokta kümeleri için işlem durumunu okur. |
> | Action | Microsoft. ClassicCompute/domainNames/loadBalancedEndpointSets/Read | Yük dengeli uç nokta kümelerini alın. |
> | Action | Microsoft. ClassicCompute/domainNames/loadBalancedEndpointSets/Write | Yük dengeli uç nokta kümesini ekleyin. |
> | Action | Microsoft. ClassicCompute/domainNames/operationdurumlarının/Read | Etki alanı adının işlem durumunu alır. |
> | Action | Microsoft. ClassicCompute/domainNames/Operationdurumlarının/Read | Etki alanı adları uzantılarının işlem durumunu okur. |
> | Action | Microsoft. ClassicCompute/domainNames/Read | Kaynaklar için etki alanı adlarını döndürün. |
> | Action | Microsoft. ClassicCompute/domainNames/serviceCertificates/Delete | Kullanılan hizmet sertifikalarını silin. |
> | Action | Microsoft. ClassicCompute/domainNames/serviceCertificates/Operationdurumlarının/Read | Etki alanı adları hizmet sertifikaları için işlem durumunu okur. |
> | Action | Microsoft. ClassicCompute/domainNames/serviceCertificates/Read | Kullanılan hizmet sertifikalarını döndürür. |
> | Action | Microsoft. ClassicCompute/domainNames/serviceCertificates/Write | Kullanılan hizmet sertifikalarını ekleyin veya değiştirin. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/abortMigration/Action | Dağıtım yuvasının geçişini durdurur. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/commitMigration/Action | Bir dağıtım yuvasının geçişini kaydeder. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/Delete | Verilen bir dağıtım yuvasını siler. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/Operationdurumlarının/Read | Etki alanı adı yuvaları için işlem durumunu okur. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/prepareMigration/Action | Bir dağıtım yuvasının geçişini hazırlar. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/okuma | Dağıtım yuvalarını gösterir. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/extensionReferences/Delete | Dağıtım yuvası rolü için uzantı başvurusunu kaldırın. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/extensionReferences/Operationdurumlarının/Read | Etki alanı adı yuvaları rol uzantısı başvuruları için işlem durumunu okur. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/extensionReferences/Read | Dağıtım yuvası rolü için uzantı başvurusunu döndürür. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/extensionReferences/Write | Dağıtım yuvası rolü için uzantı başvurusunu ekleyin veya değiştirin. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/MetricDefinitions/okuma | Etki alanı adı için rol ölçümü tanımını alın. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/ölçümler/okuma | Etki alanı adı için rol ölçümünü al. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/operationdurumlarının/Read | Etki alanı adları yuva rolü için işlem durumunu alır. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Read | Tanılama ayarlarını alın. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/sağlayıcılar/Microsoft. Insights/metricDefinitions/Read | Ölçüm tanımlarını alır. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/okuma | Dağıtım yuvasının rolünü alın. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/Roleınstances/downloadremotedesktopconnectionfile/Action | Etki alanı adı yuva rolündeki rol örneği için Uzak Masaüstü bağlantı dosyasını indirir. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/Roleınstances/Operationdurumlarının/Read | Etki alanı adları yuva rolündeki rol örneği için işlem durumunu alır. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/Roleınstances/Read | Rol örneğini alın. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/Roleınstances/Rebuild/Action | Rol örneğini yeniden oluşturur. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/Roleınstances/ReImage/Action | Rol örneğini yeniden görüntüler. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/Roleınstances/restart/Action | Rol örneklerini yeniden başlatır. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/SKU/okuma | Dağıtım yuvası için rol SKU 'su alın. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/roller/Write | Dağıtım yuvası için rol ekleyin. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/start/Action | Bir dağıtım yuvası başlatır. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/eyalet/Başlat/yaz | Dağıtım yuvası durumunu Durduruldu olarak değiştirir. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/eyalet/durdur/yaz | Dağıtım yuvası durumunu başlatıldı olarak değiştirir. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/durdur/eylem | Dağıtım yuvasını askıya alır. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/upgradeDomain/Write | Etki alanını yükseltme. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/validateMigration/Action | Dağıtım yuvasının geçişini doğrular. |
> | Action | Microsoft. ClassicCompute/domainNames/yuvalar/Write | Dağıtımı oluşturur veya güncelleştirir. |
> | Action | Microsoft. ClassicCompute/domainNames/Swap/Action | Hazırlama yuvasını üretim yuvasına değiştirir. |
> | Action | Microsoft. ClassicCompute/domainNames/Write | Kaynaklar için etki alanı adlarını ekleyin veya değiştirin. |
> | Action | Microsoft. ClassicCompute/moveSubscriptionResources/Action | Tüm klasik kaynakları farklı bir aboneliğe taşıyın. |
> | Action | Microsoft. ClassicCompute/operatingSystemFamilies/Read | Microsoft Azure ' de bulunan konuk işletim sistemi ailelerini listeler ve ayrıca her bir aile için kullanılabilir işletim sistemi sürümlerini listeler. |
> | Action | Microsoft. ClassicCompute/operatingSystems/Read | Microsoft Azure ' de şu anda kullanılabilir olan konuk işletim sisteminin sürümlerini listeler. |
> | Action | Microsoft. ClassicCompute/işlemler/okuma | İşlem listesini alır. |
> | Action | Microsoft. ClassicCompute/Operationkıdurumlar/okuma | Kaynak için işlem durumunu okur. |
> | Action | Microsoft. ClassicCompute/kotalar/okundu | Aboneliğin kotasını alın. |
> | Action | Microsoft. ClassicCompute/Register/Action | Classic Compute'a Kaydol |
> | Action | Microsoft. ClassicCompute/resourceTypes/SKU/okuma | Desteklenen kaynak türleri için SKU listesini alır. |
> | Action | Microsoft. ClassicCompute/Validatesubscriptionmoveavaılabılıty/Action | Klasik taşıma işlemi için aboneliğin kullanılabilirliğini doğrulayın. |
> | Action | Microsoft. ClassicCompute/virtualMachines/Ilişkili Networksecuritygroups/Delete | Sanal makineyle ilişkilendirilmiş ağ güvenlik grubunu siler. |
> | Action | Microsoft. ClassicCompute/virtualMachines/Ilişkili Networksecuritygroups/Operationdurumlarının/Read | Sanal makineler ile ilişkili ağ güvenlik grupları için işlem durumunu okur. |
> | Action | Microsoft. ClassicCompute/virtualMachines/Ilişkili Networksecuritygroups/Read | Sanal makineyle ilişkili ağ güvenlik grubunu alır. |
> | Action | Microsoft. ClassicCompute/virtualMachines/Ilişkili Networksecuritygroups/Write | Sanal makineyle ilişkilendirilmiş bir ağ güvenlik grubu ekler. |
> | Action | Microsoft. ClassicCompute/virtualMachines/asyncOperations/Read | Olası zaman uyumsuz işlemleri alır |
> | Action | Microsoft. ClassicCompute/virtualMachines/attachDisk/eylem | Bir sanal makineye bir veri diski ekler. |
> | Action | Microsoft. ClassicCompute/virtualMachines/Capture/Action | Bir sanal makineyi yakala. |
> | Action | Microsoft. ClassicCompute/virtualMachines/Delete | Sanal makineleri kaldırır. |
> | Action | Microsoft. ClassicCompute/virtualMachines/çıkarılabilir disk/eylem | Bir veri diskini sanal makineden ayırır. |
> | Action | Microsoft. ClassicCompute/virtualMachines/diagnosticsettings/Read | Sanal makine tanılama ayarlarını al. |
> | Action | Microsoft. ClassicCompute/virtualMachines/diskler/okuma | Veri disklerinin listesini alır |
> | Action | Microsoft. ClassicCompute/virtualMachines/downloadRemoteDesktopConnectionFile/Action | Sanal makine için RDP dosyasını indirir. |
> | Action | Microsoft. ClassicCompute/virtualMachines/Extensions/Operationdurumlarının/Read | Sanal makine uzantılarının işlem durumunu okur. |
> | Action | Microsoft. ClassicCompute/virtualMachines/uzantılar/okuma | Sanal makine uzantısını alır. |
> | Action | Microsoft. ClassicCompute/virtualMachines/uzantılar/Write | Sanal makine uzantısını koyar. |
> | Action | Microsoft. ClassicCompute/virtualMachines/MetricDefinitions/Read | Sanal makine ölçüm tanımını alın. |
> | Action | Microsoft. ClassicCompute/virtualMachines/ölçümler/okuma | Ölçümleri alır. |
> | Action | Microsoft. ClassicCompute/virtualMachines/NetworkInterfaces/ile Ilişkili Networksecuritygroups/Delete | Ağ arabirimiyle ilişkili ağ güvenlik grubunu siler. |
> | Action | Microsoft. ClassicCompute/virtualMachines/NetworkInterfaces/ile Ilişkili Networksecuritygroups/Operationdurumlarının/Read | Sanal makineler ile ilişkili ağ güvenlik grupları için işlem durumunu okur. |
> | Action | Microsoft. ClassicCompute/virtualMachines/NetworkInterfaces/ile Ilişkili Networksecuritygroups/Read | Ağ arabirimiyle ilişkili ağ güvenlik grubunu alır. |
> | Action | Microsoft. ClassicCompute/virtualMachines/NetworkInterfaces/ile Ilişkili Networksecuritygroups/Write | Ağ arabirimiyle ilişkili bir ağ güvenlik grubu ekler. |
> | Action | Microsoft. ClassicCompute/virtualMachines/Operationdurumlarının/Read | Sanal makineler için işlem durumunu okur. |
> | Action | Microsoft. ClassicCompute/virtualMachines/performMaintenance/Action | Sanal makinede bakım gerçekleştirir. |
> | Action | Microsoft. ClassicCompute/virtualMachines/Providers/Microsoft. Insights/diagnosticSettings/Read | Tanılama ayarlarını alın. |
> | Action | Microsoft. ClassicCompute/virtualMachines/Providers/Microsoft. Insights/diagnosticSettings/Write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Action | Microsoft. ClassicCompute/virtualMachines/Providers/Microsoft. Insights/metricDefinitions/Read | Ölçüm tanımlarını alır. |
> | Action | Microsoft. ClassicCompute/virtualMachines/okuma | Sanal makinelerin listesini alır. |
> | Action | Microsoft. ClassicCompute/virtualMachines/yeniden dağıtma/eylem | Sanal makineyi yeniden dağıtır. |
> | Action | Microsoft. ClassicCompute/virtualMachines/restart/Action | Sanal makineleri yeniden başlatır. |
> | Action | Microsoft. ClassicCompute/virtualMachines/kapatması/eylemi | Sanal makineyi kapatır. |
> | Action | Microsoft. ClassicCompute/virtualMachines/Başlat/eylem | Sanal makineyi başlatın. |
> | Action | Microsoft. ClassicCompute/virtualMachines/durdur/eylem | Sanal makineyi sonlandırır. |
> | Action | Microsoft. ClassicCompute/virtualMachines/Write | Sanal makineler ekleyin veya değiştirin. |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. ClassicNetwork/expressroutecrossconnections/operationdurumlarının/Read | Bir Express Route çapraz bağlantı işlem durumunu alın. |
> | Action | Microsoft. ClassicNetwork/expressroutecrossconnections/peerler/Sil | Express Route çapraz bağlantı eşlemesini silin. |
> | Action | Microsoft. ClassicNetwork/expressroutecrossconnections/peerler/operationdurumlarının/Read | Bir Express Route çapraz bağlantı eşleme işlem durumunu alır. |
> | Action | Microsoft. ClassicNetwork/expressroutecrossconnections/peerler/okundu | Hızlı rota çapraz bağlantı eşlemesini alın. |
> | Action | Microsoft. ClassicNetwork/expressroutecrossconnections/peerler/yaz | Express Route çapraz bağlantı eşlemesi ekleyin. |
> | Action | Microsoft. ClassicNetwork/expressroutecrossconnections/Read | Express Route çapraz bağlantılarını alın. |
> | Action | Microsoft. ClassicNetwork/expressroutecrossconnections/Write | Express Route çapraz bağlantıları ekleyin. |
> | Action | Microsoft. ClassicNetwork/gatewaySupportedDevices/Read | Desteklenen cihazların listesini alır. |
> | Action | Microsoft. ClassicNetwork/networkSecurityGroups/Delete | Ağ güvenlik grubunu siler. |
> | Action | Microsoft. ClassicNetwork/networkSecurityGroups/Operationdurumlarının/Read | Ağ güvenlik grubu için işlem durumunu okur. |
> | Action | Microsoft. ClassicNetwork/networksecuritygroups/Providers/Microsoft. Insights/diagnosticSettings/Read | Ağ güvenlik grupları tanılama ayarlarını alır |
> | Action | Microsoft. ClassicNetwork/networksecuritygroups/Providers/Microsoft. Insights/diagnosticSettings/Write | Ağ güvenlik grupları tanılama ayarlarını oluşturur veya güncelleştirir, bu işlem Öngörüler kaynak sağlayıcısı tarafından belirlenir. |
> | Action | Microsoft. ClassicNetwork/networksecuritygroups/Providers/Microsoft. Insights/logDefinitions/Read | Ağ güvenlik grubu olaylarını alır |
> | Action | Microsoft. ClassicNetwork/networkSecurityGroups/Read | Ağ güvenlik grubunu alır. |
> | Action | Microsoft. ClassicNetwork/networkSecurityGroups/securityRules/Delete | Güvenlik kuralını siler. |
> | Action | Microsoft. ClassicNetwork/networkSecurityGroups/securityRules/Operationdurumlarının/Read | Ağ güvenlik grubu güvenlik kuralları için işlem durumunu okur. |
> | Action | Microsoft. ClassicNetwork/networkSecurityGroups/securityRules/Read | Güvenlik kuralını alır. |
> | Action | Microsoft. ClassicNetwork/networkSecurityGroups/securityRules/Write | Bir güvenlik kuralı ekler veya güncelleştirir. |
> | Action | Microsoft. ClassicNetwork/networkSecurityGroups/Write | Yeni bir ağ güvenlik grubu ekler. |
> | Action | Microsoft. ClassicNetwork/Operations/Read | Klasik ağ işlemlerini alın. |
> | Action | Microsoft. ClassicNetwork/Quotas/Read | Aboneliğin kotasını alın. |
> | Action | Microsoft. ClassicNetwork/Register/Action | Classic Network'e Kaydol |
> | Action | Microsoft. ClassicNetwork/Rezervedips/Delete | Ayrılmış bir IP 'yi silin. |
> | Action | Microsoft. ClassicNetwork/Rezervedips/JOIN/Action | Ayrılmış bir IP'ye katılın |
> | Action | Microsoft.ClassicNetwork/reservedIps/link/action | Ayrılmış bir IP'ye bağlantı oluşturun |
> | Action | Microsoft. ClassicNetwork/Rezervedips/Operationdurumlarının/Read | Ayrılmış IP 'ler için işlem durumunu okur. |
> | Action | Microsoft.ClassicNetwork/reservedIps/read | Ayrılmış IP'leri alır |
> | Action | Microsoft. ClassicNetwork/Rezervedips/Write | Yeni bir ayrılmış IP ekleyin |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/abortMigration/Action | Bir sanal ağın geçişini durdurur |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Capabilities/Read | Özellikleri gösterir |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Checkıpaddressavailability/eylem | Bir sanal ağdaki belirli bir IP adresinin kullanılabilirliğini denetler. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/commitMigration/Action | Bir sanal ağın geçişini kaydeder |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Delete | Sanal ağı siler. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Clientiptal edilmiş sertifikalar/Sil | Bir istemci sertifikasını iptal eder. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Clientiptal edilmiş sertifikalar/okuma | İptal edilen istemci sertifikalarını okuyun. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Clientiptal edilmiş sertifikalar/Write | Bir istemci sertifikasını iptal eder. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Clienentrootcertificates/Delete | Sanal ağ geçidi istemci sertifikasını siler. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Clienentrootcertificates/Download/Action | Sertifikayı parmak izine göre indirir. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Clienentrootcertificates/listPackage/Action | Sanal ağ geçidi sertifika paketini listeler. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Clienentrootcertificates/Read | İstemci kök sertifikalarını bulun. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Clienentrootcertificates/Write | Yeni bir istemci kök sertifikasını karşıya yükler. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Connections/Connect/Action | Bir siteyi site ağ geçidi bağlantısına bağlar. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Connections/Disconnect/Action | Bir siteden siteye ağ geçidi bağlantısı bağlantısını keser. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Connections/Read | Bağlantıların listesini alır. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Connections/test/Action | Bir siteyi site ağ geçidi bağlantısına sınar. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Delete | Sanal ağ geçidini siler. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Downloaddeviceconfigurationscrıpt/Action | Cihaz yapılandırma betiğini indirir. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Downloadagnostiği/Action | Ağ Geçidi tanılamayı indirir. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Listdevresi ServiceKey/Action | Devre hizmet anahtarını alır. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/listPackage/Action | Sanal ağ geçidi paketini listeler. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Operationdurumlarının/Read | Sanal ağ geçitleri için işlem durumunu okur. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Packages/Read | Sanal ağ geçidi paketini alır. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Read | Sanal ağ geçitlerini alır. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/startDiagnostics/Action | Sanal ağ geçidi için tanılamayı başlatır. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/stopDiagnostics/Action | Sanal ağ geçidi için tanılamayı sonlandırır. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Gateway/Write | Bir sanal ağ geçidi ekler. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/join/action | Sanal ağı birleştirir. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Operationdurumlarının/Read | Sanal ağların işlem durumunu okur. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/peer/Action | Bir sanal ağı başka bir sanal ağla eşler. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/prepareMigration/Action | Bir sanal ağın geçişini hazırlar |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Read | Sanal ağı alın. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Remotevirtualnetworkpeeringproxy/Delete | Uzak sanal ağ eşleme ara sunucusunu siler. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Remotevirtualnetworkpeeringproxy/Read | Uzak sanal ağ eşleme ara sunucusunu alır. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Remotevirtualnetworkpeeringproxy/Write | Uzak sanal ağ eşleme proxy 'sini ekler veya güncelleştirir. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/altağ/Ilişkili Networksecuritygroups/Delete | Alt ağla ilişkili ağ güvenlik grubunu siler. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/altağ/Ilişkili Networksecuritygroups/Operationdurumlar/Read | Sanal ağ alt ağı ile ilişkili ağ güvenlik grubu için işlem durumunu okur. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/alt ağlar/Ilişkili olan Networksecuritygroups/Read | Alt ağla ilişkili ağ güvenlik grubunu alır. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/altağ/Ilişkili Networksecuritygroups/Write | Alt ağla ilişkili bir ağ güvenlik grubu ekler. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/validateMigration/Action | Bir sanal ağın geçişini doğrular |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Virtualnetworkpeerler/okuma | Sanal Ağ eşlemesini alır. |
> | Action | Microsoft. ClassicNetwork/virtualNetworks/Write | Yeni bir sanal ağ ekleyin. |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. ClassicStorage/Capabilities/Read | Özellikleri gösterir |
> | Action | Microsoft. ClassicStorage/checkStorageAccountAvailability/eylem | Bir depolama hesabının kullanılabilirliğini denetler. |
> | Action | Microsoft. ClassicStorage/checkStorageAccountAvailability/okuma | Bir depolama hesabının kullanılabilirliğini alın. |
> | Action | Microsoft. ClassicStorage/diskleri/okuma | Depolama hesabı diskini döndürür. |
> | Action | Microsoft. ClassicStorage/Images/operationdurumlarının/Read | Görüntü Işlemi durumunu alır. |
> | Action | Microsoft. ClassicStorage/Images/Read | Görüntüyü döndürür. |
> | Action | Microsoft. ClassicStorage/işlemler/okuma | Klasik depolama işlemlerini alır |
> | Action | Microsoft. ClassicStorage/Osımages/Read | İşletim sistemi görüntüsünü döndürür. |
> | Action | Microsoft. ClassicStorage/Osplatformımages/Read | İşletim sistemi platformu görüntüsünü alır. |
> | Action | Microsoft. ClassicStorage/Publicımages/Read | Ortak sanal makine görüntüsünü alır. |
> | Action | Microsoft. ClassicStorage/kotaları/Read | Aboneliğin kotasını alın. |
> | Action | Microsoft. ClassicStorage/Register/Action | Klasik Depolamaya Kaydeder |
> | Action | Microsoft. ClassicStorage/storageAccounts/abortMigration/Action | Bir depolama hesabının geçişini durdurur. |
> | Action | Microsoft. ClassicStorage/storageAccounts/commitMigration/Action | Bir depolama hesabının geçişini kaydeder. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Delete | Depolama hesabını silin. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Disks/Delete | Belirli bir depolama hesabı diskini siler. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Disks/Operationdurumlarının/Read | Kaynak için işlem durumunu okur. |
> | Action | Microsoft.ClassicStorage/storageAccounts/disks/read | Depolama hesabı diskini döndürür. |
> | Action | Microsoft. ClassicStorage/storageAccounts/disk/Write | Bir depolama hesabı diski ekler. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Images/Delete | Verilen bir depolama hesabı görüntüsünü siler. Kullanım dışı. ' Microsoft. ClassicStorage/storageAccounts/Vmımages ' kullanın) |
> | Action | Microsoft. ClassicStorage/storageAccounts/Images/operationdurumlarının/Read | Depolama hesabı görüntü işlemi durumunu döndürür. |
> | Action | Microsoft.ClassicStorage/storageAccounts/images/read | Depolama hesabı görüntüsünü döndürür. Kullanım dışı. ' Microsoft. ClassicStorage/storageAccounts/Vmımages ' kullanın) |
> | Action | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Operationdurumlarının/Read | Kaynak için işlem durumunu okur. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Osımages/Delete | Belirli bir depolama hesabı işletim sistemi görüntüsünü siler. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Osımages/Read | Depolama hesabı işletim sistemi görüntüsünü döndürür. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Osımages/Write | Belirli bir depolama hesabı işletim sistemi görüntüsünü ekler. |
> | Action | Microsoft. ClassicStorage/storageAccounts/prepareMigration/Action | Bir depolama hesabının geçişini hazırlar. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Providers/Microsoft. Insights/diagnosticSettings/Read | Tanılama ayarlarını alın. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Providers/Microsoft. Insights/diagnosticSettings/Write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Providers/Microsoft. Insights/metricDefinitions/Read | Ölçüm tanımlarını alır. |
> | Action | Microsoft.ClassicStorage/storageAccounts/read | Verilen hesaba sahip depolama hesabını döndürün. |
> | Action | Microsoft. ClassicStorage/storageAccounts/regenerateKey/Action | Depolama hesabı için mevcut erişim anahtarlarını yeniden oluşturur. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Services/diagnosticSettings/Read | Tanılama ayarlarını alın. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Services/diagnosticSettings/Write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Services/metricDefinitions/Read | Ölçüm tanımlarını alır. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Services/ölçümler/okuma | Ölçümleri alır. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Services/Read | Kullanılabilir hizmetleri alın. |
> | Action | Microsoft. ClassicStorage/storageAccounts/validateMigration/Action | Bir depolama hesabının geçişini doğrular. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Vmımages/Delete | Belirli bir sanal makine görüntüsünü siler. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Vmımages/operationdurumlarının/Read | Belirli bir sanal makine görüntüsü işlem durumunu alır. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Vmımages/Read | Sanal makine görüntüsünü döndürür. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Vmımages/Write | Belirli bir sanal makine görüntüsünü ekler. |
> | Action | Microsoft. ClassicStorage/storageAccounts/Write | Yeni bir depolama hesabı ekler. |
> | Action | Microsoft. ClassicStorage/Vmımages/Read | Sanal makine görüntülerini listeler. |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | DataAction | Microsoft. Biliveservices/hesaplar/otomatik öneri/arama/eylem | Bu işlem, belirli bir sorgu veya kısmi sorgu için öneriler sağlar. |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/çözümle/eylem | Bu işlem, görüntü içeriğine göre zengin bir görsel özellikler kümesini ayıklar.  |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/areaofınterest/Action | Bu işlem görüntünün en önemli alanının etrafında bir sınırlayıcı kutu döndürür. |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/açıkla/eylem | Bu işlem, bir görüntünün bir açıklamasını tüm cümleler olan okunabilir dilde oluşturur.<br> Açıklama, işlem tarafından da döndürülen bir içerik etiketleri koleksiyonunu temel alır.<br>Her görüntü için birden fazla açıklama oluşturulabilir.<br> Açıklamalar, güven puanlarına göre sıralanır.<br>Tüm açıklamalar Ingilizce 'Dir. |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/Algıla/eylem | Bu işlem, belirtilen görüntüde nesne algılamayı gerçekleştirir.  |
> | DataAction | Microsoft. Biliveservices/accounts/ComputerVision/generatethumbnail/Action | Bu işlem, Kullanıcı tarafından belirtilen genişlik ve yükseklik ile bir küçük resim oluşturur.<br> Varsayılan olarak, hizmet görüntüyü analiz eder, ilgilendiğiniz bölgeyi (ROı) tanımlar ve yatırım getirisi temelinde akıllı kırpma koordinatları oluşturur.<br> Akıllı kırpma, giriş görüntüsünden farklı bir boyut oranı belirttiğinizde yardımcı olur |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/modeller/çözümle/eylem | Bu işlem, bir görüntü içindeki içeriği, etki alanına özgü bir model uygulayarak tanır.<br> Görüntü İşleme API'si tarafından desteklenen alana özgü modellerin listesi/modeller GET isteği kullanılarak alınabilir.<br> Şu anda, API aşağıdaki alana özgü modeller sağlar: Ünlüler, yer işaretleri. |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/modeller/okuma | Bu işlem, Görüntü İşleme API'si tarafından desteklenen alana özgü modellerin listesini döndürür.  Şu anda API, etki alanına özgü şu modelleri destekliyor: ünlül tanıyıcı, yer işareti tanıyıcı. |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/OCR/eylem | Optik karakter tanıma (OCR), görüntüdeki metni algılar ve tanınan karakterleri makine tarafından kullanılabilen bir karakter akışına ayıklar.    |
> | DataAction | Microsoft. Biliveservices/accounts/ComputerVision/recognizetext/Action | Metin Tanıma işleminin sonucunu almak için bu arabirimi kullanın. Metin Tanıma arabirimini kullandığınızda, yanıt "Işlem-konum" adlı bir alan içerir. "Işlem-konum" alanı, Get Metin Tanıma Işlem sonucu işlemi için kullanmanız gereken URL 'YI içerir. |
> | DataAction | Microsoft. Biliveservices/hesaplar/ComputerVision/Tag/Action | Bu işlem, sağlanan görüntünün içeriğiyle ilgili olan sözcüklerin veya etiketlerin bir listesini oluşturur.<br>Görüntü İşleme API'si, resimlerde nesneler, canlı kullanımlar, manzara veya eylemler temelinde bulunan Etiketler döndürebilir.<br>Kategorilerin aksine, etiketler hiyerarşik bir sınıflandırma sistemine göre düzenlenmemiştir, ancak resim içeriğine karşılık gelir.<br>Etiketler belirsizliği önlemek veya bağlam sağlamak için ipuçları içerebilir, örneğin "cello" etiketine "müzik gereci" ipucu gelebilir.<br>Tüm Etiketler Ingilizce 'Dir. |
> | DataAction | Microsoft. Biliveservices/accounts/ComputerVision/textoperations/Read | Bu arabirim, metin işlemi sonucunu tanımak için kullanılır. Bu arabirimin URL 'SI, Metin Tanıma arabiriminden döndürülen <b>"işlem-konum"</b> alanından alınmalıdır. |
> | Action | Microsoft. Biliveservices/hesaplar/Sil | API hesaplarını siler |
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
> | DataAction | Microsoft. Bilivehizmetleri/hesapları/ımagesearch/Ayrıntılar/eylem | Görüntüyü içeren Web sayfaları gibi bir görüntüyle ilgili öngörüleri döndürür. |
> | DataAction | Microsoft. Biliveservices/accounts/ımagesearch/Search/Action | Belirli bir sorgu için ilgili görüntüleri alın. |
> | DataAction | Microsoft. Biliveservices/accounts/ımagesearch/trbitiriliyor/Action | Şu anda popüler resimleri al. |
> | DataAction | Microsoft. Biliveservices/accounts/ImmersiveReader/getcontentmodelforreader/eylem | Bir tam ekran okuyucusu oturumu oluşturur |
> | DataAction | Microsoft. Biliveservices/accounts/ınktanıyıcı/Recognize/eylem | Bir dizi vuruş verisi, içeriği analiz eder ve tanınan metin dahil tanınan varlıkların bir listesini oluşturur. |
> | Action | Microsoft. Biliveservices/accounts/listKeys/Action | Anahtarları Listele |
> | DataAction | Microsoft. Biliveservices/accounts/Lua/tahmin/eylem | Verilen sorgu için yayımlanmış uç nokta tahminini alır. |
> | DataAction | Microsoft. Biliveservices/hesaplar/haber arama/categorysearch/Action | Bir belirtilen kategori için haberleri döndürür. |
> | DataAction | Microsoft. Biliveservices/hesaplar/haber arama/arama/eylem | Belirli bir sorgu için ilgili haber makalelerini alın. |
> | DataAction | Microsoft. Biliveservices/hesaplar/NewsSearch/trendingkonular/eylem | Bing tarafından tanımlanan popüler konuları alın. Bunlar, Bing giriş sayfasının alt kısmındaki başlık bölümünde gösterilen konulardır. |
> | Action | Microsoft. Biliveservices/hesaplar/okuma | API hesaplarını okur. |
> | Action | Microsoft. Biliveservices/accounts/regenerateKey/Action | Anahtarı Yeniden Oluştur |
> | Action | Microsoft. Biliveservices/hesaplar/SKU 'lar/okuma | Mevcut bir kaynak için kullanılabilir SKU 'Ları okur. |
> | DataAction | Microsoft. Biliveservices/accounts/SpellCheck/SpellCheck/Action | GET veya POST aracılığıyla bir yazım denetimi sorgusunun sonucunu elde edin. |
> | DataAction | Microsoft. Biliveservices/accounts/TextAnalytics/Entities/Action | API, belirli bir belgedeki bilinen varlıkların ve genel adlandırılmış varlıkların (\"kişi\", \"konum\", \"kuruluş\" vb.) listesini döndürür. |
> | DataAction | Microsoft. Biliveservices/accounts/TextAnalytics/keyphrases/Action | API, giriş metnindeki başlıca konuşma noktalarını gösteren bir dize listesi döndürür. |
> | DataAction | Microsoft. Biliveservices/accounts/TextAnalytics/Languages/eylem | API, algılanan dile ek olarak 0 ile 1 arasında bir sayısal puan döndürür. Puanın 1’e yakın olması, tanımlanan dilin %100 olasılıkla doğru olduğunu gösterir. Toplamda 120 dil desteklenir. |
> | DataAction | Microsoft. Biliveservices/hesaplar/TextAnalytics/yaklaşım/eylem | API, 0 ile 1 arasında bir sayısal puan döndürür.<br>1 ' e yakın puan pozitif yaklaşımı, 0 ' a yakın puan negatif yaklaşımı gösterir.<br>0,5 puanı, yaklaşım eksikliğinden (ör.<br>bir FACTOID Bildirisi). |
> | Action | Microsoft. Biliveservices/hesaplar/kullanımlar/okuma | Mevcut bir kaynak için kota kullanımını alın. |
> | DataAction | Microsoft. Biliveservices/hesaplar/VideoSearch/Ayrıntılar/eylem | İlgili videolar gibi bir video hakkında Öngörüler edinin. |
> | DataAction | Microsoft. Biliveservices/accounts/VideoSearch/Search/ACTION | Belirli bir sorguyla ilgili videoları alın. |
> | DataAction | Microsoft. Biliveservices/hesaplar/VideoSearch/trbitiriliyor/eylem | Şu anda popüler videoları alın. |
> | DataAction | Microsoft. Biliveservices/accounts/VisualSearch/Search/ACTION | Belirtilen görüntüyle ilgili etiketlerin listesini döndürür |
> | DataAction | Microsoft. Bilivehizmetleri/hesapları/Web araması/arama/eylem | Belirli bir sorgu için Web, resim, haber & Videolar sonuçları alın. |
> | Action | Microsoft. Biliveservices/hesaplar/yazma | API hesaplarını yazar. |
> | Action | Microsoft. Biliveservices/Checkdomainavaılabılıty/Action | Bir abonelik için kullanılabilir SKU 'Ları okur. |
> | Action | Microsoft. Biliveservices/Locations/Checkskuavaılabılıty/Action | Bir abonelik için kullanılabilir SKU 'Ları okur. |
> | Action | Microsoft. Biliveservices/Locations/Checkskuavaılabılıty/Action | Bir abonelik için kullanılabilir SKU 'Ları okur. |
> | Action | Microsoft. Biliveservices/Locations/Deletevirtualnetworkoralt ağları/eylemi | VirtualNetworks veya alt ağları silmenin Microsoft. Network 'e yönelik bildirim. |
> | Action | Microsoft. Biliveservices/Işlemler/okuma | Tüm kullanılabilir işlemleri listeleyin |
> | Action | Microsoft. Biliveservices/Register/Action | Bilişsel Hizmetler için Abonelik kaydeder |
> | Action | Microsoft. Biliveservices/Register/Action | Bilişsel Hizmetler için Abonelik kaydeder |

## <a name="microsoftcommerce"></a>Microsoft. Commerce

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Commerce/RateCard/okuma | Verilen abonelik için teklif verileri, kaynak/ölçüm meta verileri ve ücretler döndürür. |
> | Action | Microsoft. Commerce/Usagetoplamalar/okuma | Microsoft Azure bir aboneliğe göre tüketimini alır. Sonuç, belirli bir zaman aralığında kullanım verilerini, aboneliği ve kaynakla ilgili bilgileri toplar. |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. COMPUTE/kullanılabilirliği Bilitysets/Delete | Kullanılabilirlik kümesini siler |
> | Action | Microsoft. COMPUTE/kullanılabilirliği Bilitysets/Read | Bir kullanılabilirlik kümesinin özelliklerini al |
> | Action | Microsoft. COMPUTE/kullanılabilirliği Bilitysets/vmSizes/Read | Kullanılabilirlik kümesinde bir sanal makine oluşturmak veya güncelleştirmek için kullanılabilir boyutları listeleyin |
> | Action | Microsoft. COMPUTE/kullanılabilirliği Bilitysets/Write | Yeni bir kullanılabilirlik kümesi oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. COMPUTE/Disks/beginGetAccess/Action | Blob erişimi için diskin SAS URI 'sini al |
> | Action | Microsoft. COMPUTE/diskler/Sil | Diski siler |
> | Action | Microsoft. COMPUTE/Disks/endGetAccess/Action | Diskin SAS URI 'sini iptal etme |
> | Action | Microsoft. COMPUTE/Disks/Read | Bir diskin özelliklerini al |
> | Action | Microsoft. COMPUTE/Disks/Write | Yeni bir disk oluşturur veya var olan bir diski güncelleştirir |
> | Action | Microsoft. COMPUTE/galeriler/silme | Galeri'yi siler |
> | Action | Microsoft. COMPUTE/galeriler/resimler/silme | Galeri Görüntüsünü siler |
> | Action | Microsoft. COMPUTE/galeriler/resimler/okuma | Galeri resminin özelliklerini alır |
> | Action | Microsoft. COMPUTE/galeriler/resimler/sürümler/Sil | Galeri Görüntüsü Sürümünü siler |
> | Action | Microsoft. COMPUTE/galeriler/resimler/sürümler/okuma | Galeri görüntüsü sürümünün özelliklerini alır |
> | Action | Microsoft. COMPUTE/galeriler/resimler/sürümler/yazma | Yeni bir galeri görüntüsü sürümü oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. COMPUTE/galeriler/resimler/yazma | Yeni bir galeri görüntüsü oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. COMPUTE/galeriler/okuma | Galeri özelliklerini alır |
> | Action | Microsoft. COMPUTE/galeriler/yazma | Yeni bir galeri oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. COMPUTE/hostGroups/Delete | Konak grubunu siler |
> | Action | Microsoft. COMPUTE/hostGroups/Read | Bir konak grubunun özelliklerini al |
> | Action | Microsoft. COMPUTE/hostGroups/Write | Yeni bir konak grubu oluşturur veya var olan bir konak grubunu güncelleştirir |
> | Action | Microsoft. COMPUTE/konaklar/Sil | Konağı siler |
> | Action | Microsoft. COMPUTE/konaklar/okuma | Bir konağın özelliklerini al |
> | Action | Microsoft. COMPUTE/konaklar/yazma | Yeni bir konak oluşturur veya var olan bir konağı güncelleştirir |
> | Action | Microsoft. COMPUTE/Images/Delete | Görüntüyü siler |
> | Action | Microsoft. COMPUTE/Images/Read | Görüntünün özelliklerini al |
> | Action | Microsoft. COMPUTE/Images/Write | Yeni bir görüntü oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. COMPUTE/konumlar/capsOperations/Read | Zaman uyumsuz bir Caps işleminin durumunu alır |
> | Action | Microsoft. COMPUTE/Locations/diskOperations/Read | Zaman uyumsuz disk işleminin durumunu alır |
> | Action | Microsoft. COMPUTE/Locations/logAnalytics/getRequestRateByInterval/Action | Kısıtlama tanılamayı azaltmaya yardımcı olmak için zaman aralığına göre toplam istekleri göstermek üzere Günlükler oluşturun. |
> | Action | Microsoft. COMPUTE/Locations/logAnalytics/Getkısıtledrequests/Action | ResourceName, OperationName veya uygulanan kısıtlama Ilkesi tarafından gruplanmış kısıtlanmış isteklerin toplamlarını göstermek için Günlükler oluşturun. |
> | Action | Microsoft. COMPUTE/konumlar/işlemler/okuma | Zaman uyumsuz bir işlemin durumunu alır |
> | Action | Microsoft. COMPUTE/Locations/yayımcılar/ArtifactTypes/teklifler/Read | Platform görüntüsü teklifinin özelliklerini al |
> | Action | Microsoft. COMPUTE/Locations/yayımcılar/ArtifactTypes/teklifler/SKU/okuma | Platform görüntüsü SKU 'sunun özelliklerini al |
> | Action | Microsoft. COMPUTE/Locations/yayımcılar/ArtifactTypes/teklifler/SKU 'lar/sürümler/okuma | Platform görüntüsü sürümünün özelliklerini al |
> | Action | Microsoft. COMPUTE/Locations/yayımcılar/ArtifactTypes/türler/okuma | Vmexgeri türünün özelliklerini al |
> | Action | Microsoft. COMPUTE/Locations/yayımcılar/ArtifactTypes/türler/sürümler/okuma | Vmexgeri sürümünün özelliklerini al |
> | Action | Microsoft. COMPUTE/konumlar/yayımcılar/okuma | Yayımcının özelliklerini al |
> | Action | Microsoft. COMPUTE/Locations/runCommands/Read | Konumdaki kullanılabilir çalıştırma komutlarını listeler |
> | Action | Microsoft. COMPUTE/Locations/kullanımlar/Read | Bir konumdaki aboneliğin işlem kaynakları için hizmet sınırlarını ve geçerli kullanım miktarlarını alır |
> | Action | Microsoft. COMPUTE/konumlar/vmSizes/Read | Bir konumdaki kullanılabilir sanal makine boyutlarını listeler |
> | Action | Microsoft. COMPUTE/işlemler/okuma | Microsoft. COMPUTE kaynak sağlayıcısında kullanılabilir işlemleri listeler |
> | Action | Microsoft. COMPUTE/proximityPlacementGroups/Delete | Yakınlık Yerleştirme Grubunu siler |
> | Action | Microsoft. COMPUTE/proximityPlacementGroups/Read | Bir yakınlık yerleşimi grubunun özelliklerini al |
> | Action | Microsoft. COMPUTE/proximityPlacementGroups/Write | Yeni bir yakınlık yerleşimi grubu oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. COMPUTE/Register/ACTION | Aboneliği Microsoft. COMPUTE kaynak sağlayıcısına kaydeder |
> | Action | Microsoft. COMPUTE/restorePointCollections/Delete | Geri yükleme noktası koleksiyonunu ve içerilen geri yükleme noktalarını siler |
> | Action | Microsoft. COMPUTE/restorePointCollections/Read | Geri yükleme noktası koleksiyonunun özelliklerini al |
> | Action | Microsoft. COMPUTE/restorePointCollections/restorePoints/Delete | Geri yükleme noktasını siler |
> | Action | Microsoft. COMPUTE/restorePointCollections/restorePoints/Read | Geri yükleme noktasının özelliklerini al |
> | Action | Microsoft. COMPUTE/restorePointCollections/restorePoints/Retrievesasurin/Action | Blob SAS URI 'leriyle birlikte geri yükleme noktasının özelliklerini al |
> | Action | Microsoft. COMPUTE/restorePointCollections/restorePoints/Write | Yeni bir geri yükleme noktası oluşturur |
> | Action | Microsoft. COMPUTE/restorePointCollections/Write | Yeni bir geri yükleme noktası koleksiyonu oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. COMPUTE/Sharedvmımages/Delete | SharedVMImage'i siler |
> | Action | Microsoft. COMPUTE/Sharedvmımages/Read | Sharedvmımage 'ın özelliklerini al |
> | Action | Microsoft. COMPUTE/Sharedvmımages/sürümler/Delete | SharedVMImageVersion Sil |
> | Action | Microsoft. COMPUTE/Sharedvmımages/sürümler/okuma | Sharedvmımageversion özelliklerini al |
> | Action | Microsoft. COMPUTE/Sharedvmımages/sürümler/Çoğalt/eylem | Sharedvmımageversion 'ı hedef bölgelere çoğaltma |
> | Action | Microsoft. COMPUTE/Sharedvmımages/sürümler/Write | Yeni bir Sharedvmımageversion oluşturun veya var olan bir sürümü güncelleştirin |
> | Action | Microsoft. COMPUTE/Sharedvmımages/Write | Yeni bir Sharedvmımage oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. COMPUTE/SKU/okuma | Aboneliğiniz için kullanılabilen Microsoft. COMPUTE SKU 'Larının listesini alır |
> | Action | Microsoft. COMPUTE/Snapshot/beginGetAccess/Action | Blob erişimi için anlık görüntünün SAS URI 'sini alma |
> | Action | Microsoft. COMPUTE/Snapshot/Delete | Anlık Görüntüyü siler |
> | Action | Microsoft. COMPUTE/Snapshot/endGetAccess/Action | Anlık görüntünün SAS URI 'sini iptal et |
> | Action | Microsoft. COMPUTE/Snapshot/Read | Anlık görüntünün özelliklerini al |
> | Action | Microsoft. COMPUTE/Snapshot/Write | Yeni bir anlık görüntü oluşturma veya var olanı güncelleştirme |
> | Action | Microsoft. COMPUTE/kaydını kaldırma/eylem | Microsoft. COMPUTE kaynak sağlayıcısı ile aboneliğin kaydını siler |
> | Action | Microsoft. COMPUTE/virtualMachines/yakala/eylem | Sanal sabit diskleri kopyalayarak sanal makineyi yakalar ve benzer sanal makineler oluşturmak için kullanılabilecek bir şablon oluşturur |
> | Action | Microsoft. COMPUTE/virtualMachines/convertToManagedDisks/Action | Sanal makinenin blob tabanlı disklerini yönetilen disklere dönüştürür |
> | Action | Microsoft. COMPUTE/virtualMachines/serbest bırakma/eylem | Sanal makineyi güçlendirir ve işlem kaynaklarını serbest bırakır |
> | Action | Microsoft. COMPUTE/virtualMachines/Delete | Sanal makineyi siler |
> | Action | Microsoft. COMPUTE/virtualMachines/Extensions/Sil | Sanal makine uzantısını siler |
> | Action | Microsoft. COMPUTE/virtualMachines/Extensions/okuma | Bir sanal makine uzantısının özelliklerini al |
> | Action | Microsoft. COMPUTE/virtualMachines/uzantılar/Write | Yeni bir sanal makine uzantısı oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. COMPUTE/virtualMachines/Genelleştir/eylem | Sanal makinenin durumunu Genelleştirilmiş olarak ayarlar ve sanal makineyi yakalamaya hazırlar |
> | Action | Microsoft. COMPUTE/virtualMachines/InstanceView/Read | Sanal makinenin ve kaynaklarının ayrıntılı çalışma zamanı durumunu alır |
> | DataAction | Microsoft.Compute/virtualMachines/login/action | Bir sanal makinede normal kullanıcı olarak oturum açın |
> | DataAction | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Windows yönetici veya Linux kök kullanıcı ayrıcalıklarına sahip bir sanal makinede oturum açma |
> | Action | Microsoft. COMPUTE/virtualMachines/performMaintenance/Action | VM üzerinde bakım Işlemini gerçekleştirir. |
> | Action | Microsoft. COMPUTE/virtualMachines/powerOff/ACTION | Sanal makineyi kapatır. Sanal makinenin faturalandırılmaya devam edip etmediğini unutmayın. |
> | Action | Microsoft. COMPUTE/virtualMachines/okuma | Bir sanal makinenin özelliklerini al |
> | Action | Microsoft. COMPUTE/virtualMachines/yeniden dağıtma/eylem | Sanal makineyi yeniden dağıtır |
> | Action | Microsoft. COMPUTE/virtualMachines/ReImage/Action | Fark kayıt diski kullanan sanal makineyi yeniden görüntüler. |
> | Action | Microsoft. COMPUTE/virtualMachines/yeniden Başlat/eylem | Sanal makineyi yeniden başlatır |
> | Action | Microsoft. COMPUTE/virtualMachines/runCommand/Action | Sanal makinede önceden tanımlanmış bir betiği yürütür |
> | Action | Microsoft. COMPUTE/virtualMachines/Başlat/eylem | Sanal makineyi başlatır |
> | Action | Microsoft.Compute/virtualMachines/vmSizes/read | Sanal makinenin güncelleştirileceği kullanılabilir boyutları listeler |
> | Action | Microsoft. COMPUTE/virtualMachines/Write | Yeni bir sanal makine oluşturur veya var olan bir sanal makineyi güncelleştirir |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/ayırmayı kaldırma/eylem | Sanal makine ölçek kümesinin örnekleri için işlem kaynaklarını güçlendirir ve serbest bırakır  |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/Delete | Sanal makine ölçek kümesini siler |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/Delete/Action | Sanal makine ölçek kümesinin örneklerini siler |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/Extensions/Delete | Sanal makine ölçek kümesi uzantısını siler |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/Extensions/Read | Bir sanal makine ölçek kümesi uzantısının özelliklerini alır |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/Extensions/Write | Yeni bir sanal makine ölçek kümesi uzantısı oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/forceRecoveryServiceFabricPlatformUpdateDomainWalk/Action | Bir Service Fabric sanal makine ölçek kümesinin platform güncelleştirme etki alanlarına, takılı bekleyen bir güncelleştirmeyi tamamlayacak şekilde el ile kılavuzluk edin |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/InstanceView/Read | Sanal makine ölçek kümesinin örnek görünümünü alır |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/manualUpgrade/Action | Örnekleri, sanal makine ölçek kümesinin en son modeline el ile güncelleştirir |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/NetworkInterfaces/Read | Bir sanal makine ölçek kümesinin tüm ağ arabirimlerinin özelliklerini al |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/osRollingUpgrade/Action | Tüm sanal makine ölçek kümesi örneklerini kullanılabilir en son Platform görüntüsü işletim sistemi sürümüne taşımak için bir sıralı yükseltme başlatır. |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/osUpgradeHistory/Read | Bir sanal makine ölçek kümesi için işletim sistemi yükseltmelerinden oluşan geçmişi alır |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/performMaintenance/Action | Sanal makine ölçek kümesinin örneklerinde planlı bakım gerçekleştirir |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/powerOff/ACTION | Sanal makine ölçek kümesi örneklerinin kuvvetlerini kapatır |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/publicIPAddresses/Read | Bir sanal makine ölçek kümesinin tüm genel IP adreslerinin özelliklerini al |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/Read | Bir sanal makine ölçek kümesinin özelliklerini al |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/yeniden dağıtma/eylem | Sanal makine ölçek kümesinin örneklerini yeniden dağıtın |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/ReImage/Action | Sanal makine ölçek kümesinin örneklerini yeniden görüntüler |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/Reımageall/ACTION | Bir sanal makine ölçek kümesi örnekleri için tüm diskleri (işletim sistemi diski ve veri diskleri) yeniden görüntüler  |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/restart/Action | Sanal makine ölçek kümesinin örneklerini yeniden başlatır |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/Rollingyükseltmelerinde/iptal/eylem | Bir sanal makine ölçek kümesinin sıralı yükseltmesini iptal eder |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/Rollingyükseltmelerinde/okunan | Bir sanal makine ölçek kümesi için son sıralı yükseltme durumunu al |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/Scale/Action | Mevcut bir sanal makine ölçek kümesinin, belirtilen örnek sayısına göre ölçeklendirebildiğini/ölçeklendirdiğini doğrulayın |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/SKU 'lar/okuma | Mevcut bir sanal makine ölçek kümesi için geçerli SKU 'Ları listeler |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/start/Action | Sanal makine ölçek kümesinin örneklerini başlatır |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/serbest bırakma/eylem | VM Ölçek kümesindeki bir sanal makinenin işlem kaynaklarını güçlendirir ve serbest bırakır. |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/Delete | Bir VM Ölçek kümesindeki belirli bir sanal makineyi silin. |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/InstanceView/Read | Bir VM Ölçek kümesindeki bir sanal makinenin örnek görünümünü alır. |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/NetworkInterfaces/ipConfigurations/Publicıpaddresses/Read | Sanal makine ölçek kümesi kullanılarak oluşturulan genel IP adresinin özelliklerini al. Sanal makine ölçek kümesi, IP yapılandırması başına en çok bir genel IP (özel IP) oluşturabilir |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/NetworkInterfaces/ipConfigurations/Read | Sanal makine ölçek kümesi kullanılarak oluşturulan bir ağ arabiriminin bir veya tüm IP yapılandırmalarının özelliklerini alın. IP yapılandırması özel IP 'Leri temsil eder |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/NetworkInterfaces/Read | Sanal makine ölçek kümesi kullanılarak oluşturulan bir sanal makinenin ağ arabirimlerinin bir veya tüm özelliklerini al |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/performMaintenance/Action | Sanal makine ölçek kümesindeki bir sanal makine örneğinde planlanmış bakım gerçekleştirir |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/powerOff/ACTION | Bir VM Ölçek kümesindeki bir sanal makine örneğini güçlendirir. |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/Read | VM Ölçek kümesindeki bir sanal makinenin özelliklerini alır |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/yeniden dağıtma/eylem | Sanal makine ölçek kümesindeki bir sanal makine örneğini yeniden dağıtır |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/ReImage/Action | Sanal makine ölçek kümesindeki bir sanal makine örneğini yeniden görüntüler. |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/Reımageall/ACTION | Sanal makine ölçek kümesindeki sanal makine örneği için tüm diskleri (işletim sistemi diski ve veri diskleri) yeniden görüntüler. |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/restart/Action | Bir VM Ölçek kümesindeki bir sanal makine örneğini yeniden başlatır. |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/runCommand/Action | Bir sanal makine ölçek kümesindeki bir sanal makine örneğinde önceden tanımlanmış bir betiği yürütür. |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/start/Action | VM Ölçek kümesindeki bir sanal makine örneğini başlatır. |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/virtualMachines/Write | Bir VM Ölçek kümesindeki bir sanal makinenin özelliklerini güncelleştirir |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/vmSizes/Read | Sanal makine ölçek kümesinde bir sanal makine oluşturmak veya güncelleştirmek için kullanılabilir boyutları listeleyin |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/Write | Yeni bir sanal makine ölçek kümesi oluşturur veya mevcut olanı güncelleştirir |

## <a name="microsoftconsumption"></a>Microsoft. tüketim

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. tüketim/bakiye/okuma | Bir yönetim grubu için bir faturalandırma dönemi için kullanım özetini listeleyin. |
> | Action | Microsoft. tüketim/bütçe/silme | Bütçeleri bir aboneliğe veya bir yönetim grubuna göre silin. |
> | Action | Microsoft. tüketim/bütçeler/okuma | Bütçeleri bir aboneliğe veya bir yönetim grubuna göre listeleyin. |
> | Action | Microsoft. tüketim/bütçe/yazma | Bütçeleri bir aboneliğe veya bir yönetim grubuna göre oluşturur ve güncelleştirir. |
> | Action | Microsoft. tüketim/ücretler/okuma | Ücretleri listeleyin |
> | Action | Microsoft. tüketim/krediler/okuma | Kredileri Listele |
> | Action | Microsoft. tüketim/olaylar/okuma | Olayları listeleme |
> | Action | Microsoft. tüketim/tahminler/okuma | Tahminleri listeleyin |
> | Action | Microsoft. tüketim/lotlar/okuma | Lotlar Listele |
> | Action | Microsoft. tüketim/pazar yerleri/okuma | EA ve WebDirect abonelikleri için bir kapsamın Market kaynak kullanımı ayrıntılarını listeleyin. |
> | Action | Microsoft. tüketim/operationresults/Read | Operationresults listeleme |
> | Action | Microsoft. tüketim/işlemler/okuma | Microsoft. tüketim kaynak sağlayıcısı tarafından desteklenen tüm işlemleri listeleyin. |
> | Action | Microsoft. tüketim/OperationStatus/Read | OperationStatus listeleme |
> | Action | Microsoft. tüketim/pricesheets/Read | Bir abonelik veya bir yönetim grubu için Pricesheets verilerini listeleyin. |
> | Action | Microsoft. tüketim/Register/ACTION | Tüketim RP 'ye kaydol |
> | Action | Microsoft. tüketim/Rezervationdetails/okuma | Ayrılmış örnekler için ayırma sırasına veya yönetim gruplarına göre kullanım ayrıntılarını listeleyin. Ayrıntı verileri günlük düzeyi başına örnek başına. |
> | Action | Microsoft. tüketim/Rezervationönerilere/okuma | Bir abonelik için ayrılmış örnekler için tek veya paylaşılan öneriler listeleyin. |
> | Action | Microsoft. tüketim/Rezervationözetler/okuma | Ayrılmış örnekler için kullanım özetini rezervasyon siparişi veya yönetim gruplarına göre listeleyin. Özet verileri aylık ya da günlük düzeyindedir. |
> | Action | Microsoft. tüketim/Rezervationtransactions/Read | Ayrılmış örnekler için yönetim gruplarına göre işlem geçmişini listeleyin. |
> | Action | Microsoft. tüketim/Etiketler/okuma | EA ve abonelikler için Etiketler listeleyin. |
> | Action | Microsoft. tüketim/kiracılar/okuma | Kiracılar Listele |
> | Action | Microsoft. tüketim/kiracılar/yazmaç/eylem | Bir kiracının Microsoft. tüketim kapsamı için kaydetme eylemi. |
> | Action | Microsoft. tüketim/hüküm/okuma | Bir abonelik veya bir yönetim grubu için koşulları listeleyin. |
> | Action | Microsoft. tüketim/usageDetails/okuma | EA ve WebDirect abonelikleri için bir kapsamın kullanım ayrıntılarını listeleyin. |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Containerınstance/containerGroups/kapsayıcılar/exec/Action | Belirli bir kapsayıcıya çalıştırılabilir. |
> | Action | Microsoft. Containerınstance/containerGroups/kapsayıcılar/Günlükler/okuma | Belirli bir kapsayıcı için günlükleri al. |
> | Action | Microsoft. Containerınstance/containerGroups/Delete | Belirli kapsayıcı grubunu silin. |
> | Action | Microsoft. Containerınstance/containerGroups/Providers/Microsoft. Insights/diagnosticSettings/Read | Kapsayıcı grubu için tanılama ayarını alır. |
> | Action | Microsoft. Containerınstance/containerGroups/Providers/Microsoft. Insights/diagnosticSettings/Write | Kapsayıcı grubu için tanılama ayarını oluşturur veya güncelleştirir. |
> | Action | Microsoft. Containerınstance/containerGroups/Providers/Microsoft. Insights/metricDefinitions/Read | Kapsayıcı grubu için kullanılabilir ölçümleri alır. |
> | Action | Microsoft. Containerınstance/containerGroups/Read | Tüm kapsayıcı gruplarını al. |
> | Action | Microsoft. Containerınstance/containerGroups/restart/Action | Belirli bir kapsayıcı grubunu yeniden başlatır. |
> | Action | Microsoft. Containerınstance/containerGroups/start/Action | Belirli bir kapsayıcı grubunu başlatır. |
> | Action | Microsoft. Containerınstance/containerGroups/durdur/eylem | Belirli bir kapsayıcı grubunu sonlandırır. İşlem kaynakları serbest bırakılır ve faturalandırma durdurulur. |
> | Action | Microsoft. Containerınstance/containerGroups/Write | Belirli bir kapsayıcı grubunu oluşturun veya güncelleştirin. |
> | Action | Microsoft. Containerınstance/Locations/Önbellekedimages/Read | Bir bölgedeki aboneliğin önbelleğe alınmış görüntülerini alır. |
> | Action | Microsoft. Containerınstance/Locations/Capabilities/Read | Bir bölgenin yeteneklerini alın. |
> | Action | Microsoft. Containerınstance/Locations/Deletevirtualnetworkoralt ağları/eylem | , Sanal ağın veya alt ağın silinmekte olduğunu Microsoft. Containerınstance 'a bildirir. |
> | Action | Microsoft. Containerınstance/Locations/işlemler/okuma | Azure Container Instance hizmeti için işlemleri listeleyin. |
> | Action | Microsoft. Containerınstance/Locations/kullanımlar/Read | Belirli bir bölgenin kullanımını alın. |
> | Action | Microsoft. Containerınstance/işlemler/okuma | Azure Container Instance hizmeti için işlemleri listeleyin. |
> | Action | Microsoft. Containerınstance/Register/ACTION | Kapsayıcı örneği kaynak sağlayıcısı için aboneliği kaydeder ve kapsayıcı gruplarının oluşturulmasına izin vermez. |
> | Action | Microsoft. Containerınstance/serviceassociationlinks/Delete | Azure Container Instance kaynak sağlayıcısı tarafından bir alt ağda oluşturulan hizmet ilişkisi bağlantısını silin. |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. ContainerRegistry/Checknameavaılabılıty/Read | Kapsayıcı kayıt defteri adının kullanıma hazır olup olmadığını denetler. |
> | Action | Microsoft. ContainerRegistry/Locations/Deletevirtualnetworkoralt ağları/eylem | Sanal ağ veya alt ağın silindiği Microsoft. ContainerRegistry öğesine bildirir |
> | Action | Microsoft. ContainerRegistry/konumlar/operationResults/Read | Zaman uyumsuz işlem sonucunu alır |
> | Action | Microsoft. ContainerRegistry/işlemler/okuma | Tüm kullanılabilir Azure Container Registry REST API işlemlerini listeler |
> | Action | Microsoft. ContainerRegistry/Register/ACTION | Kapsayıcı kayıt defteri kaynak sağlayıcısı için aboneliği kaydeder ve kapsayıcı kayıt defterlerinin oluşturulmasını sunar. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/yapıtlar/silme | Bir kapsayıcı kayıt defterinde yapıtı silin. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/derlemeler/iptal/eylem | Var olan bir derlemeyi iptal eder. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/derlemeler/getLogLink/Action | Derleme günlüklerini indirmek için bir bağlantı alır. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/derlemeler/okuma | Belirtilen derleme özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm derlemeleri listeler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/derlemeler/yazma | Bir kapsayıcı kayıt defteri için belirtilen parametrelerle bir derlemeyi güncelleştirir. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/buildTasks/Delete | Bir kapsayıcı kayıt defterinden derleme görevini siler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/buildTasks/listSourceRepositoryProperties/Action | Yapı görevinin kaynak denetim özelliklerini listeler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/buildTasks/Read | Belirtilen derleme görevinin özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm derleme görevlerini listeler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/buildTasks/Steps/Delete | Derleme görevinin yapı adımını siler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/buildTasks/Steps/listBuildArguments/ACTION | Gizli bağımsız değişkenler de dahil olmak üzere derleme adımının derleme bağımsız değişkenlerini listeler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/buildTasks/Steps/Read | Belirtilen derleme adımının özelliklerini alır veya belirtilen derleme görevinin tüm derleme adımlarını listeler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/buildTasks/Steps/Write | Belirtilen parametrelerle derleme görevi için derleme adımı oluşturur veya güncelleştirir. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/buildTasks/Write | Belirtilen parametrelerle bir kapsayıcı kayıt defteri için derleme görevi oluşturur veya güncelleştirir. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/silme | Bir kapsayıcı kayıt defterini siler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/eventGridFilters/Delete | Bir kapsayıcı kayıt defterinden bir olay Kılavuzu filtresini siler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/eventGridFilters/Read | Belirtilen olay Kılavuzu filtresinin özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm olay Kılavuzu filtrelerini listeler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/eventGridFilters/Write | Belirtilen parametrelerle bir kapsayıcı kayıt defteri için bir olay kılavuz filtresi oluşturur veya güncelleştirir. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/getBuildSourceUploadUrl/Action | Kullanıcının kaynağı yükleyebilmesi için karşıya yükleme konumunu alır. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/ımportımage/eylem | Görüntüyü, belirtilen parametrelerle kapsayıcı kayıt defterine aktarın. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/listBuildSourceUploadUrl/Action | Bir kapsayıcı kayıt defteri için kaynak yükleme URL 'si konumunu al. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/listCredentials/Action | Belirtilen kapsayıcı kayıt defteri için oturum açma kimlik bilgilerini listeler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/listPolicies/Read | Belirtilen kapsayıcı kayıt defteri için ilkeleri listeler |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/Listkullanımlar/okuma | Belirtilen kapsayıcı kayıt defteri için kota kullanımlarını listeler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/meta veriler/okuma | Bir kapsayıcı kayıt defteri için belirli bir deponun meta verilerini alır |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/meta veri/yazma | Bir kapsayıcı kayıt defteri için bir deponun meta verilerini güncelleştirir |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/Operationdurumlarının/Read | Kayıt defteri zaman uyumsuz işlem durumunu alır |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/çekme/okuma | Bir kapsayıcı kayıt defterinden görüntüleri çekin veya alın. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/gönderme/yazma | Bir kapsayıcı kayıt defterine görüntü gönderin veya yazın. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/Quarantıneread/okuma | Kapsayıcı kayıt defterinden karantinaya alınmış görüntüleri çekme veya alma |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/quarantineWrite/Write | Karantinaya alınan görüntülerin karantina durumunu yazma/değiştirme |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/queueBuild/Action | İstek parametrelerine göre yeni bir yapı oluşturur ve yapı kuyruğuna ekler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/okuma | Belirtilen kapsayıcı kayıt defterinin özelliklerini alır veya belirtilen kaynak grubu veya abonelik altındaki tüm kapsayıcı kayıt defterlerinin listesini görüntüler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/regenerateCredential/ACTION | Belirtilen kapsayıcı kayıt defteri için oturum açma kimlik bilgilerinden birini yeniden oluşturur. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/çoğaltmalar/silme | Bir kapsayıcı kayıt defterinden bir çoğaltmayı siler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/çoğaltmalar/Operationdurumlarının/Read | Çoğaltma zaman uyumsuz işlem durumunu alır |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/çoğaltmalar/okuma | Belirtilen çoğaltmanın özelliklerini alır veya belirtilen kapsayıcı kayıt defteri için tüm çoğaltmaları listeler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/çoğaltmalar/yazma | Belirtilen parametrelerle bir kapsayıcı kayıt defteri için çoğaltma oluşturur veya güncelleştirir. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/çalıştırmalar/iptal/eylem | Mevcut bir çalıştırmayı iptal edin. |
> | Action | Microsoft. ContainerRegistry/Registry/çalıştırmaları/listLogSasUrl/Action | Bir çalıştırmanın günlük SAS URL 'sini alır. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/çalıştırmalar/Read | Bir kapsayıcı kayıt defterinde veya liste çalıştırmalarından bir çalıştırmanın özelliklerini alır. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/çalıştırmalar/Write | Bir çalıştırmayı güncelleştirir. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/scheduleRun/Action | Bir kapsayıcı kayıt defterine karşı bir çalıştırma zamanlayın. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/imzala/yaz | Kapsayıcı kayıt defteri için gönderme/çekme içeriği güven meta verileri. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/görevler/silme | Bir kapsayıcı kayıt defteri için bir görevi siler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/görevler/listDetails/Action | Bir kapsayıcı kayıt defteri için bir görevin tüm ayrıntılarını listeleyin. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/görevler/okuma | Bir kapsayıcı kayıt defteri için bir görev alır veya tüm görevleri listeler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/görevler/yazma | Bir kapsayıcı kayıt defteri için bir görev oluşturur veya güncelleştirir. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/updatePolicies/Write | Belirtilen kapsayıcı kayıt defteri için ilkeleri güncelleştirir |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/Web kancaları/silme | Bir Web kancasını kapsayıcı kayıt defterinden siler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/Web kancaları/getCallbackConfig/Action | Web kancası için hizmet URI 'SI ve özel üst bilgilerin yapılandırmasını alır. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/Web kancaları/listEvents/eylem | Belirtilen Web kancası için son olayları listeler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/Web kancaları/Operationdurumlarının/Read | Web kancası zaman uyumsuz işlem durumunu alır |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/Web kancaları/ping/eylem | Web kancasına gönderilecek bir ping olayını tetikler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/Web kancaları/okuma | Belirtilen kapsayıcı kayıt defteri için belirtilen Web kancasının özelliklerini alır veya tüm Web kancalarını listeler. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/Web kancaları/yazma | Belirtilen parametrelerle bir kapsayıcı kayıt defteri için Web kancası oluşturur veya güncelleştirir. |
> | Action | Microsoft. ContainerRegistry/kayıt defterleri/yazma | Belirtilen parametrelerle bir kapsayıcı kayıt defteri oluşturur veya güncelleştirir. |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. ContainerService/containerServices/Delete | Bir kapsayıcı hizmetini siler |
> | Action | Microsoft. ContainerService/containerServices/Read | Kapsayıcı hizmeti al |
> | Action | Microsoft. ContainerService/containerServices/Write | Yeni bir kapsayıcı hizmeti oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. ContainerService/konumlar/operationresults/Read | Zaman uyumsuz bir işlem sonucunun durumunu alır |
> | Action | Microsoft. ContainerService/konumlar/işlemler/okuma | Zaman uyumsuz bir işlemin durumunu alır |
> | Action | Microsoft. ContainerService/konumlar/orchestrators/okunan | Desteklenen düzenleyicileri listeler |
> | Action | Microsoft. ContainerService/Managedkümeler/accessProfiles/listCredential/Action | Liste kimlik bilgisini kullanarak rol adına göre yönetilen küme erişim profili al |
> | Action | Microsoft. ContainerService/Managedkümeler/accessProfiles/okuma | Rol adına göre yönetilen küme erişim profili al |
> | Action | Microsoft. ContainerService/Managedkümeler/agentPools/Delete | Bir aracı havuzunu siler |
> | Action | Microsoft. ContainerService/Managedkümeler/agentPools/okuma | Bir aracı havuzu alır |
> | Action | Microsoft. ContainerService/Managedkümeler/agentPools/upgradeProfiles/okuma | Aracı havuzunun yükseltme profilini alır |
> | Action | Microsoft. ContainerService/Managedkümeler/agentPools/Write | Yeni bir aracı havuzu oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. ContainerService/Managedkümeler/Sil | Yönetilen bir kümeyi siler |
> | Action | Microsoft. ContainerService/Managedkümeler/detektorları/okuma | Yönetilen küme algılayıcısı 'nı al |
> | Action | Microsoft. ContainerService/Managedkümeler/listClusterAdminCredential/ACTION | Yönetilen kümenin clusterAdmin kimlik bilgisini listeleyin |
> | Action | Microsoft. ContainerService/Managedkümeler/listClusterUserCredential/Action | Yönetilen kümenin clusterUser kimlik bilgisini listeleyin |
> | Action | Microsoft. ContainerService/Managedkümeler/privateEndpointConnectionsApproval/eylem | Kullanıcının özel bir uç nokta bağlantısını onaylamasını izin verilip verilmeyeceğini belirler |
> | Action | Microsoft. ContainerService/Managedkümeler/okuma | Yönetilen küme al |
> | Action | Microsoft. ContainerService/Managedkümeler/resetAADProfile/Action | Yönetilen kümenin AAD profilini sıfırlayın |
> | Action | Microsoft. ContainerService/Managedkümeler/resetServicePrincipalProfile/Action | Yönetilen kümenin hizmet sorumlusu profilini sıfırlayın |
> | Action | Microsoft. ContainerService/Managedkümeler/upgradeProfiles/okuma | Kümenin yükseltme profilini alır |
> | Action | Microsoft. ContainerService/Managedkümeler/Write | Yeni bir yönetilen küme oluşturur veya var olan bir kümeyi güncelleştirir |
> | Action | Microsoft. ContainerService/openShiftClusters/Delete | Açık bir kaydırma kümesini silme |
> | Action | Microsoft. ContainerService/openShiftClusters/Read | Açık bir Shift kümesi al |
> | Action | Microsoft. ContainerService/openShiftClusters/Write | Yeni bir açık vardiya kümesi oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. ContainerService/openShiftManagedClusters/Delete | Açık bir vardiya yönetilen kümesini silme |
> | Action | Microsoft. ContainerService/openShiftManagedClusters/Read | Açık bir vardiya yönetilen kümesi al |
> | Action | Microsoft. ContainerService/openShiftManagedClusters/Write | Yeni bir açık vardiya yönetilen kümesi oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. ContainerService/işlemler/okuma | Microsoft. ContainerService kaynak sağlayıcısında kullanılabilir işlemleri listeler |
> | Action | Microsoft. ContainerService/Register/Action | Aboneliği Microsoft. ContainerService kaynak sağlayıcısına kaydeder |
> | Action | Microsoft. ContainerService/Unregister/eylem | Microsoft. ContainerService kaynak sağlayıcısı ile aboneliğin kaydını siler |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Contentmoderatör/uygulamalar/Sil | İşlemi Siler |
> | Action | Microsoft. Contentmoderatör/uygulamalar/Listgizlilikler/Action | Parolaları Listele |
> | Action | Microsoft. Contentmoderatör/uygulamalar/listSingleSignOnToken/Action | Çoklu oturum açma belirteçlerini oku |
> | Action | Microsoft. Contentmoderatör/uygulamalar/okuma | İşlemi Okur |
> | Action | Microsoft. Contentmoderatör/uygulamalar/yazma | İşlemi Yazar |
> | Action | Microsoft. Contentmoderatör/uygulamalar/yazma | İşlemi Yazar |
> | Action | Microsoft. Contentmoderatör/listCommunicationPreference/eylem | İletişim tercihini Listele |
> | Action | Microsoft. Contentmoderatör/işlemler/okuma | okuma işlemleri |
> | Action | Microsoft. Contentmoderatör/updateCommunicationPreference/eylem | İletişim tercihini Güncelleştir |

## <a name="microsoftcostmanagement"></a>Microsoft. CostManagement

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. CostManagement/Cloudbağlayıcılar/Delete | Belirtilen cloudConnector 'ı silin. |
> | Action | Microsoft. CostManagement/Cloudkonnektörleri/okuma | Kimliği doğrulanmış kullanıcı için Cloudbağlayıcıları listeleyin. |
> | Action | Microsoft. CostManagement/Cloudbağlayıcılar/Write | Belirtilen cloudConnector 'ı oluşturun veya güncelleştirin. |
> | Action | Microsoft. CostManagement/Dimensions/Read | Desteklenen tüm boyutları bir kapsama göre listeleyin. |
> | Action | Microsoft. CostManagement/dışarı aktarmalar/eylem | Belirtilen dışarı aktarmayı çalıştırın. |
> | Action | Microsoft. CostManagement/dışarı aktarmalar/Sil | Belirtilen dışarı aktarmayı silin. |
> | Action | Microsoft. CostManagement/dışarı aktarmalar/okundu | Dışarı aktarmaları kapsama göre listeleyin. |
> | Action | Microsoft. CostManagement/dışarı aktarmalar/Çalıştır/eylem | Dışarı aktarmaları Çalıştır. |
> | Action | Microsoft. CostManagement/dışarı aktarmalar/yaz | Belirtilen dışarı aktarmayı oluşturun veya güncelleştirin. |
> | Action | Microsoft. CostManagement/externalBillingAccounts/Externalabonelikleri/okuma | Kimliği doğrulanmış kullanıcı için externalBillingAccount içindeki Externalaboneliklerinizi listeleyin. |
> | Action | Microsoft. CostManagement/externalBillingAccounts/Read | Kimliği doğrulanmış kullanıcı için externalBillingAccounts listesini listeleyin. |
> | Action | Microsoft. CostManagement/Externalabonelikleri/okuma | Kimliği doğrulanmış kullanıcı için Externalaboneliklerinizi listeleyin. |
> | Action | Microsoft. CostManagement/Externalabonelikleri/yazma | ExternalSubscription ilişkili yönetim grubunu Güncelleştir |
> | Action | Microsoft. CostManagement/Query/Action | Kullanım verilerini bir kapsama göre sorgulama. |
> | Action | Microsoft. CostManagement/Query/Read | Kullanım verilerini bir kapsama göre sorgulama. |
> | Action | Microsoft. CostManagement/Register/ACTION | Abonelik ile Microsoft. CostManagement kapsamı için kaydetme eylemi. |
> | Action | Microsoft. CostManagement/Reports/Action | Kullanım verileri hakkında raporları bir kapsama göre zamanlayın. |
> | Action | Microsoft. CostManagement/Reports/Read | Kullanım verileri hakkında raporları bir kapsama göre zamanlayın. |
> | Action | Microsoft. CostManagement/kiracılar/Register/Action | Bir kiracının Microsoft. CostManagement kapsamı için kaydetme eylemi. |

## <a name="microsoftdatabox"></a>Microsoft. DataBox

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. DataBox/Jobs/bookShipmentPickUp/ACTION | İade sevk irsaliyeleri için bir çekme defterine izin verir. |
> | Action | Microsoft. DataBox/Jobs/iptal/eylem | Devam eden bir siparişi iptal eder. |
> | Action | Microsoft. DataBox/işler/Sil | Siparişleri silme |
> | Action | Microsoft. DataBox/Jobs/listCredentials/Action | Siparişle ilgili şifrelenmemiş kimlik bilgilerini listeler. |
> | Action | Microsoft. DataBox/Jobs/Read | Siparişleri listeleyin veya alın |
> | Action | Microsoft. DataBox/işler/yazma | Siparişleri oluşturma veya güncelleştirme |
> | Action | Microsoft. DataBox/Locations/Availablesku 'Lar/eylem | Bu yöntem, kullanılabilir SKU 'ların listesini döndürür. |
> | Action | Microsoft. DataBox/Locations/Availablesku 'Lar/okuma | Kullanılabilir SKU 'Ları listeleme veya edinme |
> | Action | Microsoft. DataBox/Locations/operationResults/Read | Işlem sonuçlarını listeleme veya edinme |
> | Action | Microsoft. DataBox/Locations/regionConfiguration/Action | Bu yöntem, bölgenin yapılandırmasını döndürür. |
> | Action | Microsoft. DataBox/Locations/validateAddress/Action | Sevkiyat adresini doğrular ve varsa alternatif adresler sağlar. |
> | Action | Microsoft. DataBox/Locations/Validategirişlerinde/Action | Bu yöntem, tüm doğrulama türlerini yapar. |
> | Action | Microsoft. DataBox/işlemler/okuma | Işlemleri listeleme veya edinme |
> | Action | Microsoft. DataBox/Register/Action | Sağlayıcıyı kaydet Microsoft. databox |
> | Action | Microsoft. DataBox/abonelikler/resourceGroups/Moveresonak/Action |  |
> | Action | Microsoft. DataBox/abonelikler/resourceGroups/Validatemoveresonak/Action |  |
> | Action | Microsoft. DataBox/kaydını kaldırma/eylem | Kaydettirme sağlayıcısını Kaldır Microsoft. databox |

## <a name="microsoftdataboxedge"></a>Microsoft. DataBoxEdge

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/uyarılar/okuma | Uyarıları listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/uyarılar/okuma | Uyarıları listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/Delete | Bant genişliği zamanlamalarını siler |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/Read | Bant genişliği zamanlamalarını listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/Read | Bant genişliği zamanlamalarını listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/Write | Bant genişliği zamanlamalarını oluşturur veya güncelleştirir |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Delete | Data Box Edge cihazlarını siler |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/downloadUpdates/Action | Güncelleştirmeleri cihaza indir |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Gebir Dedınformation/Action | Genişletilmiş kaynak bilgilerini getirir |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/ınstallupdates/Action | Güncelleştirmeleri cihaza yükle |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/işler/okuma | İşleri listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/networkSettings/Read | Cihaz ağ ayarlarını listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/operationsStatus/Read | İşlem durumunu listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/siparişler/Sil | Siparişleri siler |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/siparişler/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/siparişler/okuma | Siparişleri listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/siparişler/okuma | Siparişleri listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/siparişler/yazma | Siparişleri oluşturur veya güncelleştirir |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Read | Data Box Edge cihazlarını listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Read | Data Box Edge cihazlarını listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Read | Data Box Edge cihazlarını listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/roller/Delete | Rolleri siler |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/roller/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/roller/okuma | Rolleri listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/roller/okuma | Rolleri listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/roller/Write | Rolleri oluşturur veya güncelleştirir |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/scanForUpdates/Action | Güncelleştirmeleri Tara |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/securitySettings/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/securitySettings/Update/Action | Güvenlik ayarlarını güncelleştir |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/paylaşımlar/silme | Paylaşımları siler |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/paylaşımlar/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/paylaşımlar/okuma | Paylaşımları listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/paylaşımlar/okuma | Paylaşımları listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/paylaşımlar/Yenile/eylem | Bulutta bulunan verilerle paylaşma meta verilerini yenileyin |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/paylaşımlar/yazma | Paylaşımları oluşturur veya güncelleştirir |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/Delete | Depolama hesabı kimlik bilgilerini siler |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/Read | Depolama hesabı kimlik bilgilerini listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/Read | Depolama hesabı kimlik bilgilerini listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/Write | Depolama hesabı kimlik bilgilerini oluşturur veya güncelleştirir |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Tetikleyiciler/silme | Tetikleyicileri siler |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Tetikleyiciler/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Tetikleyiciler/okuma | Tetikleyicileri listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Tetikleyiciler/okuma | Tetikleyicileri listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Tetikleyiciler/yazma | Tetikleyicileri oluşturur veya güncelleştirir |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/updateSummary/Read | Güncelleştirme özetini listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/uploadCertificate/Action | Cihaz kaydı için sertifikayı karşıya yükle |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/kullanıcılar/Sil | Paylaşma kullanıcılarını siler |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/kullanıcılar/operationResults/Read | İşlem sonucunu listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/kullanıcılar/okuma | Paylaşma kullanıcılarını listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/kullanıcılar/okuma | Paylaşma kullanıcılarını listeler veya alır |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/kullanıcılar/yazma | Paylaşma kullanıcıları oluşturur veya güncelleştirir |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Write | Data Box Edge cihazlarını oluşturur veya güncelleştirir |
> | Action | Microsoft. DataBoxEdge/dataBoxEdgeDevices/Write | Data Box Edge cihazlarını oluşturur veya güncelleştirir |

## <a name="microsoftdatabricks"></a>Microsoft. Databricks

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Databricks/Locations/getNetworkPolicies/Action | NRP tarafından kullanılan konuma göre bir alt ağ için ağ hedefi Ilkelerini al |
> | Action | Microsoft. Databricks/Register/ACTION | Databricks 'e kaydolun. |
> | Action | Microsoft. Databricks/Workspaces/Delete | Databricks çalışma alanını kaldırır. |
> | Action | Microsoft. Databricks/Workspaces/Providers/Microsoft. Insights/diagnosticSettings/Read | Databricks çalışma alanı için kullanılabilir tanılama ayarlarını ayarlar |
> | Action | Microsoft. Databricks/Workspaces/Providers/Microsoft. Insights/diagnosticSettings/Write | Tanılama ayarlarını ekleyin veya değiştirin. |
> | Action | Microsoft. Databricks/Workspaces/Providers/Microsoft. Insights/logDefinitions/Read | Databricks çalışma alanı için kullanılabilir günlük tanımlarını alır |
> | Action | Microsoft. Databricks/Workspaces/Read | Databricks çalışma alanlarının bir listesini alır. |
> | Action | Microsoft. Databricks/Workspaces/Write | Bir Databricks çalışma alanı oluşturur. |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. DataCatalog/Catalogs/Delete | Veri Kataloğu kaynak sağlayıcısı için Katalog kaynağını silin. |
> | Action | Microsoft. DataCatalog/Catalogs/Read | Veri Kataloğu kaynak sağlayıcısı için Katalog kaynağını okuyun. |
> | Action | Microsoft. DataCatalog/kataloglar/Write | Veri Kataloğu kaynak sağlayıcısı için Katalog kaynağı yazın. |
> | Action | Microsoft. DataCatalog/Checknameavaılabılıty/Read | Veri Kataloğu kaynak sağlayıcısı için katalog adı kullanılabilirliğini denetleyin. |
> | Action | Microsoft. DataCatalog/datacatalogs/Delete | Veri Kataloğu kaynak sağlayıcısı için DataCatalog kaynağını silin. |
> | Action | Microsoft. DataCatalog/datacatalogs/Read | Veri Kataloğu kaynak sağlayıcısı için DataCatalog kaynağını okuyun. |
> | Action | Microsoft. DataCatalog/datacatalogs/Write | Veri Kataloğu kaynak sağlayıcısı için DataCatalog kaynağını yazın. |
> | Action | Microsoft. DataCatalog/Operations/Read | Veri Kataloğu kaynak sağlayıcısındaki tüm kullanılabilir işlemleri okur. |
> | Action | Microsoft. DataCatalog/Register/ACTION | Veri Kataloğu kaynak sağlayıcısı için aboneliği kaydetme |
> | Action | Microsoft. DataCatalog/Unregister/ACTION | Veri Kataloğu kaynak sağlayıcısı aboneliğini silme |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. DataFactory/checkazuredatafactorynameavailability/Read | Data Factory adının kullanılabilir olup olmadığını denetler. |
> | Action | Microsoft. DataFactory/DataFactory/activitywindows/Read | Belirtilen parametrelerle Data Factory etkinlik pencerelerini okur. |
> | Action | Microsoft. DataFactory/DataFactory/datahatlarının/Activities/activitywindows/Read | Belirtilen parametrelerle işlem hattı etkinliği için etkinlik pencerelerini okur. |
> | Action | Microsoft. DataFactory/DataFactory/datahatlarının/activitywindows/Read | Belirtilen parametrelere sahip işlem hattı için etkinlik pencerelerini okur. |
> | Action | Microsoft. DataFactory/DataFactory/datahatlarının/Delete | Herhangi bir işlem hattını siler. |
> | Action | Microsoft. DataFactory/DataFactory/datahatlarını al/Duraklat/eylem | Tüm işlem hatlarını duraklatır. |
> | Action | Microsoft. DataFactory/DataFactory/datahatlarının/Read | Herhangi bir işlem hattını okur. |
> | Action | Microsoft. DataFactory/DataFactory/datahatlarının/CV/Action | Tüm işlem hatlarını sürdürür. |
> | Action | Microsoft. DataFactory/DataFactory/datahatlarının/Update/Action | Tüm işlem hatlarını güncelleştirir. |
> | Action | Microsoft. DataFactory/DataFactory/datahatlarının/Write | Herhangi bir işlem hattı oluşturur veya güncelleştirir. |
> | Action | Microsoft. DataFactory/DataFactory/DataSet/activitywindows/Read | Belirtilen parametrelere sahip veri kümesi için etkinlik pencerelerini okur. |
> | Action | Microsoft. DataFactory/DataFactory/veri kümeleri/silme | Herhangi bir veri kümesini siler. |
> | Action | Microsoft. DataFactory/DataFactory/veri kümeleri/okuma | Tüm veri kümelerini okur. |
> | Action | Microsoft. DataFactory/DataFactory/DataSet/dilimleyicerbir/Read | Verilen veri kümesi için verilen başlangıç zamanına sahip veri dilimi çalıştırmasını okur. |
> | Action | Microsoft. DataFactory/DataFactory/veri kümeleri/dilimler/okuma | Verilen dönemdeki veri dilimlerini alır. |
> | Action | Microsoft. DataFactory/DataFactory/veri kümeleri/dilimler/Write | Veri diliminin durumunu güncelleştirin. |
> | Action | Microsoft. DataFactory/DataFactory/veri kümeleri/yazma | Herhangi bir veri kümesini oluşturur veya güncelleştirir. |
> | Action | Microsoft. DataFactory/DataFactory/Delete | Data Factory siler. |
> | Action | Microsoft. DataFactory/DataFactory/Gateway/ConnectionInfo/Action | Herhangi bir ağ geçidi için bağlantı bilgilerini okur. |
> | Action | Microsoft. DataFactory/DataFactory/Gateway/Delete | Tüm ağ geçidini siler. |
> | Action | Microsoft. DataFactory/DataFactory/Gateway/listauthkeys/Action | Herhangi bir ağ geçidinin kimlik doğrulama anahtarlarını listeler. |
> | Action | Microsoft. DataFactory/DataFactory/Gateway/Read | Herhangi bir ağ geçidini okur. |
> | Action | Microsoft. DataFactory/DataFactory/Gateway/regenerateauthkey/Action | Herhangi bir ağ geçidinin kimlik doğrulama anahtarlarını yeniden oluşturur. |
> | Action | Microsoft. DataFactory/DataFactory/Gateway/Write | Herhangi bir ağ geçidini oluşturur veya güncelleştirir. |
> | Action | Microsoft. DataFactory/DataFactory/linkedServices/Delete | Bağlı hizmeti siler. |
> | Action | Microsoft. DataFactory/DataFactory/linkedServices/Read | Herhangi bir bağlı hizmeti okur. |
> | Action | Microsoft. DataFactory/DataFactory/linkedServices/Write | Herhangi bir bağlı hizmeti oluşturur veya güncelleştirir. |
> | Action | Microsoft. DataFactory/DataFactory/okuma | Data Factory okur. |
> | Action | Microsoft. DataFactory/DataFactory/çalıştırmalar/LogInfo/Read | Günlükleri içeren bir blob kapsayıcısına SAS URI 'sini okur. |
> | Action | Microsoft. DataFactory/DataFactory/tablolar/Sil | Herhangi bir veri kümesini siler. |
> | Action | Microsoft. DataFactory/DataFactory/tablolar/okuma | Tüm veri kümelerini okur. |
> | Action | Microsoft. DataFactory/DataFactory/Tables/Write | Herhangi bir veri kümesini oluşturur veya güncelleştirir. |
> | Action | Microsoft. DataFactory/DataFactory/Write | Data Factory oluşturur veya güncelleştirir. |
> | Action | Microsoft. DataFactory/Factory/cancelardışık düzen eylemsizlik/eylem | Çalıştırma KIMLIĞI tarafından belirtilen işlem hattı çalıştırmasını iptal eder. |
> | Action | Microsoft. DataFactory/Factory/Cancelsandboxardışık düzen eylemsizlik/eylem | İşlem hattı için bir hata ayıklama çalıştırmasını iptal eder. |
> | Action | Microsoft. DataFactory/Factory/createdataflowdebugsession/Action | Bir veri akışı hata ayıklama oturumu oluşturur. |
> | Action | Microsoft. DataFactory/Factory/dataakışlar/Delete | Veri akışını siler. |
> | Action | Microsoft. DataFactory/Factory/dataakışlar/okuma | Veri akışını okur. |
> | Action | Microsoft. DataFactory/Factory/dataakışlar/Write | Veri akışı oluştur veya güncelleştir |
> | Action | Microsoft. DataFactory/Factory/veri kümeleri/silme | Herhangi bir veri kümesini siler. |
> | Action | Microsoft. DataFactory/Factory/veri kümeleri/okuma | Tüm veri kümelerini okur. |
> | Action | Microsoft. DataFactory/Factory/veri kümeleri/yazma | Herhangi bir veri kümesini oluşturur veya güncelleştirir. |
> | Action | Microsoft. DataFactory/Factory/debugardışık düzen eylemsizlik/iptal/eylem | İşlem hattı için bir hata ayıklama çalıştırmasını iptal eder. |
> | Action | Microsoft. DataFactory/Factory/Delete | Data Factory siler. |
> | Action | Microsoft. DataFactory/Factory/deletedataflowdebugsession/Action | Bir veri akışı hata ayıklama oturumunu siler. |
> | Action | Microsoft. DataFactory/Factory/getDataPlaneAccess/Action | ADF Datadüzlemi hizmetine erişimi alır. |
> | Action | Microsoft. DataFactory/Factory/getDataPlaneAccess/Read | ADF Datadüzlemi hizmetine erişimi okur. |
> | Action | Microsoft. DataFactory/Factory/getFeatureValue/Action | Belirli bir konum için pozlama denetim özelliği değeri alın. |
> | Action | Microsoft. DataFactory/Factory/getFeatureValue/Read | Belirli bir konum için pozlama denetimi özellik değerini okur. |
> | Action | Microsoft. DataFactory/Factory/getGitHubAccessToken/eylem | GitHub erişim belirtecini alır. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/silme | Tüm Integration Runtime siler. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationsms/getconnectionınfo/Read | Integration Runtime bağlantı bilgilerini okur. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/getObjectMetadata/eylem | Belirtilen Integration Runtime için SSIS Integration Runtime meta verilerini alın. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/GetStatus/Read | Integration Runtime durumunu okur. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/linkedIntegrationRuntime/Action | Belirtilen paylaşılan Integration Runtime bağlı Integration Runtime başvurusu oluştur. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/listauthkeys/Read | Herhangi bir Integration Runtime için kimlik doğrulama anahtarlarını listeler. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/monitoringdata/Read | Integration Runtime için Izleme verilerini alır. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/düğümleri/silme | Belirtilen Integration Runtime düğümünü siler. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/düğümler/IPAddress/Action | Integration Runtime belirtilen düğümü için IP adresini döndürür. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/düğümleri/okuma | Belirtilen Integration Runtime için düğümü okur. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/düğümleri/yazma | Şirket içinde barındırılan Integration Runtime düğümünü güncelleştirir. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/okuma | Integration Runtime okur. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/refreshObjectMetadata/Action | Belirtilen Integration Runtime için SSIS Integration Runtime meta verilerini yenile. |
> | Action | Microsoft. DataFactory/Factory/Integration, çalışma zamanları/regenerateauthkey/Action | Belirtilen Integration Runtime için kimlik doğrulama anahtarlarını yeniden oluşturur. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/removelmürekkepler/eylem | Belirtilen Integration Runtime bağlı Integration Runtime başvurularını kaldırır. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/Başlat/eylem | Integration Runtime başlatır. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/durdur/eylem | Integration Runtime sonlandırır. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/synccredentials/Action | Belirtilen Integration Runtime ait kimlik bilgilerini eşitler. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/yükseltme/eylem | Belirtilen Integration Runtime yükseltir. |
> | Action | Microsoft. DataFactory/Factory/ıntegrationçalışma zamanları/yazma | Integration Runtime oluşturur veya güncelleştirir. |
> | Action | Microsoft. DataFactory/Factory/linkedServices/Delete | Bağlı hizmeti siler. |
> | Action | Microsoft. DataFactory/Factory/linkedServices/Read | Bağlı hizmeti okur. |
> | Action | Microsoft. DataFactory/Factory/linkedServices/Write | Bağlı hizmet oluştur veya güncelleştir |
> | Action | Microsoft. DataFactory/Factory/ardışık düzen eylemsizlik/activityçalıştırmaları/okuma | Belirtilen işlem hattı çalıştırma KIMLIĞI için etkinlik çalıştırmalarını okur. |
> | Action | Microsoft. DataFactory/Factory/ardışık düzen eylemsizlik/iptal/eylem | Çalıştırma KIMLIĞI tarafından belirtilen işlem hattı çalıştırmasını iptal eder. |
> | Action | Microsoft. DataFactory/Factory/ardışık düzen eylemsizlik/queryactivityçalıştırmaları/eylemi | Belirtilen işlem hattı çalıştırma KIMLIĞI için etkinliğin çalıştığını sorgular. |
> | Action | Microsoft. DataFactory/Factory/ardışık düzen eylemsizlik/queryactivityçalıştırmaları/okumak | Belirtilen işlem hattı çalıştırma KIMLIĞI için sorgu etkinliği çalışmalarının sonucunu okur. |
> | Action | Microsoft. DataFactory/Factory/ardışık düzen eylemsizlik/okunan | İşlem hattı çalıştırmalarını okur. |
> | Action | Microsoft. DataFactory/Factory/işlem hatları/createrun/eylem | İşlem hattı için bir çalıştırma oluşturur. |
> | Action | Microsoft. DataFactory/Factory/işlem hatları/silme | Ardışık düzeni siler. |
> | Action | Microsoft. DataFactory/Factory/işlem hatları/ardışık düzen eylemsizlik/activityçalıştırmaları/ilerlemesi/okuma | Etkinlik çalıştırmalarının Ilerlemesini alır. |
> | Action | Microsoft. DataFactory/Factory/işlem hatları/ardışık düzen eylemsizlik/Read | İşlem hattı çalıştırmasını okur. |
> | Action | Microsoft. DataFactory/Factory/işlem hatları/okuma | İşlem hattını okur. |
> | Action | Microsoft. DataFactory/Factory/işlem hatları/Sandbox/eylem | İşlem hattı için bir hata ayıklama çalıştırması ortamı oluşturur. |
> | Action | Microsoft. DataFactory/Factory/işlem hatları/Sandbox/oluşturma/eylem | İşlem hattı için bir hata ayıklama çalıştırması ortamı oluşturur. |
> | Action | Microsoft. DataFactory/Factory/işlem hatları/Sandbox/Çalıştır/eylem | İşlem hattı için bir hata ayıklama çalıştırması oluşturur. |
> | Action | Microsoft. DataFactory/Factory/işlem hatları/yazma | İşlem hattı oluştur veya güncelleştir |
> | Action | Microsoft. DataFactory/Factory/querydebugardışık düzen eylemsizlik/Action | Hata ayıklama işlem hattının çalıştığını sorgular. |
> | Action | Microsoft. DataFactory/Factory/querypipeleylemsizlik/Action | İşlem hattının çalıştırmalarını sorgular. |
> | Action | Microsoft. DataFactory/Factory/querypipeleylemsizlik/Read | Sorgu işlem hattı çalıştırmalarının sonucunu okur. |
> | Action | Microsoft. DataFactory/Factory/querytriggerçalıştırmaları/eylemi | Tetikleyici çalıştırmalarını sorgular. |
> | Action | Microsoft. DataFactory/Factory/querytriggerçalıştırmaları/okuma | Tetikleyici çalıştırmalarının sonucunu okur. |
> | Action | Microsoft. DataFactory/Factory/okuma | Data Factory okur. |
> | Action | Microsoft. DataFactory/Factory/sandboxardışık düzen eylemsizlik/Sandboxactivityçalıştırmaları/Read | Etkinliğin hata ayıklama çalıştırma bilgisini alır. |
> | Action | Microsoft. DataFactory/Factory/startdataflowdebugsession/Action | Bir veri akışı hata ayıklama oturumu başlatır. |
> | Action | Microsoft. DataFactory/Factory/triggerçalıştırmaları/okuma | Tetikleyici çalıştırmalarını okur. |
> | Action | Microsoft. DataFactory/Factory/Tetikleyiciler/silme | Tüm tetikleyiciyi siler. |
> | Action | Microsoft. DataFactory/Factory/Tetikleyiciler/okuma | Herhangi bir tetikleyiciyi okur. |
> | Action | Microsoft. DataFactory/Factory/Tetikleyiciler/Başlat/eylem | Herhangi bir tetikleyiciyi başlatır. |
> | Action | Microsoft. DataFactory/Factory/Tetikleyiciler/durdur/eylem | Tüm tetikleyiciyi sonlandırır. |
> | Action | Microsoft. DataFactory/Factory/Triggers/triggerçalıştırmaları/Read | Tetikleyici çalıştırmalarını okur. |
> | Action | Microsoft. DataFactory/Factory/Tetikleyiciler/Write | Herhangi bir tetikleyiciyi oluşturur veya güncelleştirir. |
> | Action | Microsoft. DataFactory/Factory/Write | Data Factory oluşturun veya güncelleştirin |
> | Action | Microsoft. DataFactory/Locations/configureFactoryRepo/Action | Fabrika için depoyu yapılandırır. |
> | Action | Microsoft. DataFactory/Locations/getFeatureValue/Action | Belirli bir konum için pozlama denetim özelliği değeri alın. |
> | Action | Microsoft. DataFactory/Locations/getFeatureValue/Read | Belirli bir konum için pozlama denetimi özellik değerini okur. |
> | Action | Microsoft. DataFactory/işlemler/okuma | Microsoft Data Factory Provider 'daki tüm Işlemleri okur. |
> | Action | Microsoft. DataFactory/Register/ACTION | Data Factory kaynak sağlayıcısı için aboneliği kaydeder. |
> | Action | Microsoft. DataFactory/Unregister/eylem | Data Factory kaynak sağlayıcısı için aboneliğin kaydını siler. |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. DataLakeAnalytics/accounts/computePolicies/Delete | İşlem ilkesini silin. |
> | Action | Microsoft. DataLakeAnalytics/accounts/computePolicies/Read | Bir işlem ilkesi hakkında bilgi alın. |
> | Action | Microsoft. DataLakeAnalytics/accounts/computePolicies/Write | Bir işlem ilkesi oluşturun veya güncelleştirin. |
> | Action | Microsoft. DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | Bir DataLakeStore hesabının bir DataLakeAnalytics hesabıyla bağlantısını kaldırın. |
> | Action | Microsoft. DataLakeAnalytics/accounts/dataLakeStoreAccounts/Read | Bir DataLakeAnalytics hesabının bağlı DataLakeStore hesabı hakkında bilgi alın. |
> | Action | Microsoft. DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | Bir DataLakeAnalytics hesabının bağlı DataLakeStore hesabını oluşturun veya güncelleştirin. |
> | Action | Microsoft. DataLakeAnalytics/hesaplar/Sil | Bir DataLakeAnalytics hesabını silin. |
> | Action | Microsoft. DataLakeAnalytics/accounts/firewallRules/Delete | Bir güvenlik duvarı kuralını silin. |
> | Action | Microsoft. DataLakeAnalytics/accounts/firewallRules/Read | Bir güvenlik duvarı kuralı hakkında bilgi alın. |
> | Action | Microsoft. DataLakeAnalytics/accounts/firewallRules/Write | Bir güvenlik duvarı kuralı oluşturun veya güncelleştirin. |
> | Action | Microsoft. DataLakeAnalytics/hesaplar/operationResults/Read | Bir DataLakeAnalytics hesap işleminin sonucunu elde edin. |
> | Action | Microsoft. DataLakeAnalytics/hesaplar/okuma | Mevcut bir DataLakeAnalytics hesabı hakkında bilgi alın. |
> | Action | Microsoft. DataLakeAnalytics/hesaplar/storageAccounts/kapsayıcılar/listSasTokens/Action | Bir DataLakeAnalytics hesabının bağlı depolama hesabının depolama kapsayıcıları için SAS belirteçlerini listeleyin. |
> | Action | Microsoft. DataLakeAnalytics/hesaplar/storageAccounts/kapsayıcılar/okuma | Bir DataLakeAnalytics hesabının bağlı depolama hesabının kapsayıcılarını alın. |
> | Action | Microsoft. DataLakeAnalytics/hesaplar/storageAccounts/Delete | Bir depolama hesabının bir DataLakeAnalytics hesabından bağlantısını kaldırın. |
> | Action | Microsoft. DataLakeAnalytics/hesaplar/storageAccounts/Read | Bir DataLakeAnalytics hesabının bağlı depolama hesabı hakkında bilgi alın. |
> | Action | Microsoft. DataLakeAnalytics/hesaplar/storageAccounts/Write | Bir DataLakeAnalytics hesabı için bağlı bir depolama hesabı oluşturun veya güncelleştirin. |
> | Action | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Diğer kullanıcılar tarafından gönderilen işleri iptal etmek için izin verin. |
> | Action | Microsoft. DataLakeAnalytics/accounts/Transferanalizticsunonun/eylemi | Systemmaxanalizticsunkendisini DataLakeAnalytics hesapları arasında aktarın. |
> | Action | Microsoft. DataLakeAnalytics/accounts/virtualNetworkRules/Delete | Bir sanal ağ kuralını silin. |
> | Action | Microsoft. DataLakeAnalytics/accounts/virtualNetworkRules/Read | Bir sanal ağ kuralı hakkında bilgi alın. |
> | Action | Microsoft. DataLakeAnalytics/accounts/virtualNetworkRules/Write | Bir sanal ağ kuralı oluşturun veya güncelleştirin. |
> | Action | Microsoft. DataLakeAnalytics/hesaplar/yazma | Bir DataLakeAnalytics hesabı oluşturun veya güncelleştirin. |
> | Action | Microsoft. DataLakeAnalytics/konumlar/yetenek/okuma | DataLakeAnalytics kullanımıyla ilgili bir aboneliğin yetenek bilgilerini alın. |
> | Action | Microsoft. DataLakeAnalytics/konumlar/Checknameavaılabılıty/Action | Bir DataLakeAnalytics hesap adının kullanılabilirliğini denetleyin. |
> | Action | Microsoft. DataLakeAnalytics/konumlar/operationResults/Read | Bir DataLakeAnalytics hesap işleminin sonucunu elde edin. |
> | Action | Microsoft. DataLakeAnalytics/konumlar/kullanımlar/okuma | DataLakeAnalytics kullanımıyla ilgili bir aboneliğin kota kullanımları bilgilerini alın. |
> | Action | Microsoft. DataLakeAnalytics/işlemler/okuma | DataLakeAnalytics 'in kullanılabilir işlemlerini alın. |
> | Action | Microsoft. DataLakeAnalytics/Register/Action | Aboneliği DataLakeAnalytics 'e kaydedin. |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. DataLakeStore/hesaplar/Sil | Bir DataLakeStore hesabını silin. |
> | Action | Microsoft. DataLakeStore/accounts/Enablekeykasası/eylem | DataLakeStore hesabı için Anahtar Kasası 'nı etkinleştirin. |
> | Action | Microsoft. DataLakeStore/accounts/eventGridFilters/Delete | Bir EventGrid filtresini silin. |
> | Action | Microsoft. DataLakeStore/accounts/Eventgridfilter/Read | Bir EventGrid filtresi alın. |
> | Action | Microsoft. DataLakeStore/accounts/Eventgridfilter/Write | Bir EventGrid filtresi oluşturun veya güncelleştirin. |
> | Action | Microsoft. DataLakeStore/accounts/firewallRules/Delete | Bir güvenlik duvarı kuralını silin. |
> | Action | Microsoft. DataLakeStore/accounts/firewallRules/Read | Bir güvenlik duvarı kuralı hakkında bilgi alın. |
> | Action | Microsoft. DataLakeStore/accounts/firewallRules/Write | Bir güvenlik duvarı kuralı oluşturun veya güncelleştirin. |
> | Action | Microsoft. DataLakeStore/accounts/operationResults/Read | DataLakeStore hesabı işleminin sonucunu elde edin. |
> | Action | Microsoft. DataLakeStore/hesaplar/okuma | Mevcut bir DataLakeStore hesabı hakkında bilgi alın. |
> | Action | Microsoft.DataLakeStore/accounts/Superuser/action | Microsoft. Authorization/Roleatamalar/Write ile verildiğinde Süper Kullanıcı Data Lake Store verin. |
> | Action | Microsoft. DataLakeStore/accounts/trustedIdProviders/Delete | Güvenilen bir kimlik sağlayıcısını silin. |
> | Action | Microsoft. DataLakeStore/accounts/trustedIdProviders/Read | Güvenilen bir kimlik sağlayıcısı hakkında bilgi alın. |
> | Action | Microsoft. DataLakeStore/accounts/trustedIdProviders/Write | Güvenilen bir kimlik sağlayıcısı oluşturun veya güncelleştirin. |
> | Action | Microsoft. DataLakeStore/accounts/virtualNetworkRules/Delete | Bir sanal ağ kuralını silin. |
> | Action | Microsoft. DataLakeStore/accounts/virtualNetworkRules/Read | Bir sanal ağ kuralı hakkında bilgi alın. |
> | Action | Microsoft. DataLakeStore/accounts/virtualNetworkRules/Write | Bir sanal ağ kuralı oluşturun veya güncelleştirin. |
> | Action | Microsoft. DataLakeStore/hesaplar/yazma | Bir DataLakeStore hesabı oluşturun veya güncelleştirin. |
> | Action | Microsoft. DataLakeStore/konumlar/yetenek/okuma | DataLakeStore kullanımı ile ilgili bir aboneliğin yetenek bilgilerini alın. |
> | Action | Microsoft. DataLakeStore/Locations/Checknameavaılabılıty/Action | DataLakeStore hesap adının kullanılabilirliğini kontrol edin. |
> | Action | Microsoft. DataLakeStore/konumlar/operationResults/Read | DataLakeStore hesabı işleminin sonucunu elde edin. |
> | Action | Microsoft. DataLakeStore/konumlar/kullanımlar/okuma | DataLakeStore kullanımıyla ilgili bir aboneliğin kota kullanımları bilgilerini alın. |
> | Action | Microsoft. DataLakeStore/işlemler/okuma | DataLakeStore 'in kullanılabilir işlemlerini alın. |
> | Action | Microsoft. DataLakeStore/Register/ACTION | Aboneliği DataLakeStore 'ye kaydedin. |

## <a name="microsoftdatamigration"></a>Microsoft. DataMigration

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. DataMigration/konumlar/operationResults/Read | 202 kabul edilen bir Yanıtla ilgili uzun süreli bir işlemin durumunu alın |
> | Action | Microsoft. DataMigration/Locations/Operationdurumlarının/Read | 202 kabul edilen bir Yanıtla ilgili uzun süreli bir işlemin durumunu alın |
> | Action | Microsoft. DataMigration/Register/Action | Aboneliği Azure veritabanı geçiş hizmeti sağlayıcısına kaydeder |
> | Action | Microsoft. DataMigration/Services/addWorker/Action | Hizmetin kullanılabilir çalışanlarına bir DMS çalışanı ekler |
> | Action | Microsoft. DataMigration/hizmetler/checkStatus/Action | Hizmetin dağıtılıp dağıtılmadığını ve çalışıp çalışmadığını denetleyin |
> | Action | Microsoft. DataMigration/Services/configureWorker/Action | Bir DMS çalışanını hizmetin kullanılabilir çalışanları için yapılandırır |
> | Action | Microsoft. DataMigration/hizmetler/Sil | Bir kaynağı ve tüm alt öğelerini siler |
> | Action | Microsoft. DataMigration/Services/Projects/accessArtifacts/Action | Proje yapıtları almak veya koymak için kullanılabilecek bir URL Oluştur |
> | Action | Microsoft. DataMigration/hizmetler/projeler/Sil | Bir kaynağı ve tüm alt öğelerini siler |
> | Action | Microsoft. DataMigration/Services/Projects/Files/Delete | Bir kaynağı ve tüm alt öğelerini siler |
> | Action | Microsoft. DataMigration/Services/Projects/Files/Read | Kaynaklar hakkındaki bilgileri okuyun |
> | Action | Microsoft. DataMigration/Services/Projects/Files/Read/action | Dosyanın içeriğini okumak için kullanılabilecek bir URL alın |
> | Action | Microsoft. DataMigration/Services/Projects/Files/readWrite/Action | Dosyanın içeriğini okumak veya yazmak için kullanılabilecek bir URL alın |
> | Action | Microsoft. DataMigration/Services/Projects/Files/Write | Kaynakları ve bunların özelliklerini oluşturun veya güncelleştirin |
> | Action | Microsoft. DataMigration/hizmetler/projeler/okuma | Kaynaklar hakkındaki bilgileri okuyun |
> | Action | Microsoft. DataMigration/hizmetler/projeler/görevler/iptal/eylem | Şu anda çalışıyorsa, görevi iptal et |
> | Action | Microsoft. DataMigration/hizmetler/projeler/görevler/Sil | Bir kaynağı ve tüm alt öğelerini siler |
> | Action | Microsoft. DataMigration/hizmetler/projeler/görevler/okuma | Kaynaklar hakkındaki bilgileri okuyun |
> | Action | Microsoft. DataMigration/hizmetler/projeler/görevler/yazma | Görevleri çalıştırma Azure veritabanı geçiş hizmeti görevleri |
> | Action | Microsoft. DataMigration/hizmetler/projeler/yazma | Görevleri çalıştırma Azure veritabanı geçiş hizmeti görevleri |
> | Action | Microsoft. DataMigration/Services/Read | Kaynaklar hakkındaki bilgileri okuyun |
> | Action | Microsoft. DataMigration/Services/removeWorker/Action | Bir DMS çalışanını hizmetin kullanılabilir çalışanlarına kaldırır |
> | Action | Microsoft. DataMigration/Services/serviceTasks/Cancel/Action | Şu anda çalışıyorsa, görevi iptal et |
> | Action | Microsoft. DataMigration/Services/serviceTasks/Delete | Bir kaynağı ve tüm alt öğelerini siler |
> | Action | Microsoft. DataMigration/Services/serviceTasks/Read | Kaynaklar hakkındaki bilgileri okuyun |
> | Action | Microsoft. DataMigration/Services/serviceTasks/Write | Görevleri çalıştırma Azure veritabanı geçiş hizmeti görevleri |
> | Action | Microsoft. DataMigration/hizmetler/yuvalar/silme | Bir kaynağı ve tüm alt öğelerini siler |
> | Action | Microsoft. DataMigration/hizmetler/yuvalar/okuma | Kaynaklar hakkındaki bilgileri okuyun |
> | Action | Microsoft. DataMigration/hizmetler/yuvalar/yazma | Kaynakları ve bunların özelliklerini oluşturun veya güncelleştirin |
> | Action | Microsoft. DataMigration/hizmetler/Başlat/eylem | Bu hizmetin geçişleri yeniden işlemesini sağlamak için DMS hizmetini başlatın |
> | Action | Microsoft. DataMigration/hizmetler/durdur/eylem | Kendi maliyetini en aza indirmek için DMS hizmetini durdurun |
> | Action | Microsoft. DataMigration/Services/updateAgentConfig/Action | DMS Aracısı yapılandırmasını belirtilen değerlerle güncelleştirir. |
> | Action | Microsoft. DataMigration/hizmetler/Write | Kaynakları ve bunların özelliklerini oluşturun veya güncelleştirin |
> | Action | Microsoft. DataMigration/SKU 'lar/okuma | DMS kaynakları tarafından desteklenen SKU 'ların bir listesini alın. |

## <a name="microsoftdbformariadb"></a>Microsoft. Dbformarıdb

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Dbformarıdb/Checknameavaılabılıty/Action | Belirli bir abonelik için verilen sunucu adının dünya genelinde sağlanması için kullanılabilir olup olmadığını doğrulayın. |
> | Action | Microsoft. Dbformarıdb/konumlar/azureAsyncOperation/Read | MariaDB sunucusu Işlem sonuçlarını döndür |
> | Action | Microsoft. Dbformarıdb/konumlar/operationResults/Read | ResourceGroup tabanlı MariaDB sunucusu Işlem sonuçlarını döndür |
> | Action | Microsoft. Dbformarıdb/konumlar/operationResults/Read | MariaDB sunucusu Işlem sonuçlarını döndür |
> | Action | Microsoft. Dbformarıdb/konumlar/Performancekatmanları/okuma | Kullanılabilir performans katmanları listesini döndürür. |
> | Action | Microsoft. Dbformarıdb/konumlar/securityAlertPoliciesAzureAsyncOperation/Read | Sunucu tehdit algılama işlemi sonucunun listesini döndürür. |
> | Action | Microsoft. Dbformarıdb/konumlar/Securityalcertpoliciesoperationresults/Read | Sunucu tehdit algılama işlemi sonucunun listesini döndürür. |
> | Action | Microsoft. Dbformarıdb/işlemler/okuma | MariaDB Işlemlerinin listesini döndürün. |
> | Action | Microsoft. Dbformarıdb/Performancekatmanları/okuma | Kullanılabilir performans katmanları listesini döndürür. |
> | Action | Microsoft. Dbformarıdb/Register/ACTION | MariaDB kaynak sağlayıcısını Kaydet |
> | Action | Microsoft. Dbformarıdb/sunucular/Yöneticiler/Sil | MariaDB sunucusunun var olan bir yöneticisini siler. |
> | Action | Microsoft. Dbformarıdb/sunucular/Yöneticiler/okuma | MariaDB sunucu yöneticilerinin bir listesini alır. |
> | Action | Microsoft. Dbformarıdb/sunucular/Yöneticiler/yazma | MariaDB sunucu yöneticisini belirtilen parametrelerle oluşturur veya güncelleştirir. |
> | Action | Microsoft. Dbformarıdb/Servers/danışmanörler/Createreyorumdedadctionsession/ACTION | Yeni bir öneri eylem oturumu oluştur |
> | Action | Microsoft. Dbformarıdb/sunucular/danışmanları/okuma | Danışmanlarının listesini döndürün |
> | Action | Microsoft. Dbformarıdb/sunucular/danışmanları/okuma | Danışman döndürme |
> | Action | Microsoft. Dbformarıdb/sunucular/danışmanları/recommendedActions/okuma | Önerilen eylemlerin listesini döndür |
> | Action | Microsoft. Dbformarıdb/sunucular/danışmanları/recommendedActions/okuma | Önerilen eylemlerin listesini döndür |
> | Action | Microsoft. Dbformarıdb/sunucular/danışmanları/recommendedActions/okuma | Önerilen eylemi döndürür |
> | Action | Microsoft. Dbformarıdb/sunucular/yapılandırma/okuma | Bir sunucu için yapılandırmaların listesini döndürün veya belirtilen yapılandırma için özellikleri alır. |
> | Action | Microsoft. Dbformarıdb/sunucu/yapılandırma/yazma | Belirtilen yapılandırma için değeri güncelleştir |
> | Action | Microsoft. Dbformarıdb/sunucular/veritabanları/silme | Var olan bir MariaDB veritabanını siler. |
> | Action | Microsoft. Dbformarıdb/sunucular/veritabanları/okuma | MariaDB veritabanlarının listesini döndürün veya belirtilen veritabanının özelliklerini alır. |
> | Action | Microsoft. Dbformarıdb/sunucular/veritabanları/yazma | Belirtilen parametrelerle bir MariaDB veritabanı oluşturur veya belirtilen veritabanı için özellikleri güncelleştirir. |
> | Action | Microsoft. Dbformarıdb/sunucu/silme | Var olan bir sunucuyu siler. |
> | Action | Microsoft. Dbformarıdb/Servers/firewallRules/Delete | Mevcut bir güvenlik duvarı kuralını siler. |
> | Action | Microsoft. Dbformarıdb/Servers/firewallRules/Read | Bir sunucu için güvenlik duvarı kuralları listesini döndürün veya belirtilen güvenlik duvarı kuralının özelliklerini alır. |
> | Action | Microsoft. Dbformarıdb/Servers/firewallRules/Write | Belirtilen parametrelerle bir güvenlik duvarı kuralı oluşturur veya var olan bir kuralı güncelleştirir. |
> | Action | Microsoft. Dbformarıdb/Servers/logFiles/Read | MariaDB LogFiles listesini döndürün. |
> | Action | Microsoft. Dbformarıdb/Servers/Providers/Microsoft. Insights/diagnosticSettings/Read | Kaynak için ayrılan ayarı alır |
> | Action | Microsoft. Dbformarıdb/Servers/Providers/Microsoft. Insights/diagnosticSettings/Write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. Dbformarıdb/Servers/Providers/Microsoft. Insights/logDefinitions/Read | MariaDB sunucuları için kullanılabilir günlükleri alır |
> | Action | Microsoft. Dbformarıdb/Servers/Providers/Microsoft. Insights/metricDefinitions/Read | Veritabanları için kullanılabilir ölçümlerin dönüş türleri |
> | Action | Microsoft. Dbformarıdb/Servers/Querymetinler/eylem | Sorgu listesi için metin döndürme |
> | Action | Microsoft. Dbformarıdb/Servers/Querymetinler/eylem | Bir sorgunun metnini döndürme |
> | Action | Microsoft. Dbformarıdb/sunucular/okuma | Sunucu listesini döndürün veya belirtilen sunucunun özelliklerini alır. |
> | Action | Microsoft. Dbformarıdb/Servers/recoverableServers/Read | Kurtarılabilir MariaDB sunucu bilgilerini döndürün |
> | Action | Microsoft. Dbformarıdb/sunucular/çoğaltmalar/okuma | MariaDB sunucusunun okuma çoğaltmalarını al |
> | Action | Microsoft. Dbformarıdb/sunucular/yeniden Başlat/eylem | Belirli bir sunucuyu yeniden başlatır. |
> | Action | Microsoft. Dbformarıdb/Servers/Securityalcertpolicies/Read | Belirli bir sunucuda yapılandırılmış sunucu tehdit algılama ilkesinin ayrıntılarını alma |
> | Action | Microsoft. Dbformarıdb/Servers/Securityalcertpolicies/Write | Belirli bir sunucu için sunucu tehdit algılama ilkesini değiştirme |
> | Action | Microsoft. Dbformarıdb/Servers/Topquerystatıstıcs/Read | En üstteki sorgular için sorgu Istatistikleri listesini döndürür. |
> | Action | Microsoft. Dbformarıdb/Servers/Topquerystatıstıcs/Read | Sorgu Istatistiği döndürme |
> | Action | Microsoft. Dbformarıdb/Servers/updateConfigurations/Action | Belirtilen sunucu için Güncelleştirme yapılandırması |
> | Action | Microsoft. Dbformarıdb/Servers/virtualNetworkRules/Delete | Var olan bir sanal ağ kuralını siler |
> | Action | Microsoft. Dbformarıdb/Servers/virtualNetworkRules/Read | Sanal ağ kurallarının listesini döndürün veya belirtilen sanal ağ kuralı için özellikleri alır. |
> | Action | Microsoft. Dbformarıdb/Servers/virtualNetworkRules/Write | Belirtilen parametrelere sahip bir sanal ağ kuralı oluşturur veya belirtilen sanal ağ kuralı için özellikleri veya etiketleri güncelleştirir. |
> | Action | Microsoft. Dbformarıdb/sunucular/waitStatistics/okuma | Örnek için bekleme istatistiklerini döndür |
> | Action | Microsoft. Dbformarıdb/sunucular/waitStatistics/okuma | Bekleme istatistiği döndürme |
> | Action | Microsoft. Dbformarıdb/sunucu/yazma | Belirtilen parametrelere sahip bir sunucu oluşturur veya belirtilen sunucu için özellikleri veya etiketleri güncelleştirir. |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Dbformyısql/Checknameavaılabılıty/Action | Belirli bir abonelik için verilen sunucu adının dünya genelinde sağlanması için kullanılabilir olup olmadığını doğrulayın. |
> | Action | Microsoft. Dbformyısql/konumlar/azureAsyncOperation/Read | MySQL sunucusu Işlem sonuçlarını döndür |
> | Action | Microsoft. Dbformyısql/konumlar/operationResults/Read | ResourceGroup tabanlı MySQL Server Işlem sonuçlarını döndür |
> | Action | Microsoft. Dbformyısql/konumlar/operationResults/Read | MySQL sunucusu Işlem sonuçlarını döndür |
> | Action | Microsoft. Dbformyısql/konumlar/Performancekatmanları/okuma | Kullanılabilir performans katmanları listesini döndürür. |
> | Action | Microsoft. Dbformyısql/konumlar/securityAlertPoliciesAzureAsyncOperation/Read | Sunucu tehdit algılama işlemi sonucunun listesini döndürür. |
> | Action | Microsoft. Dbformyısql/konumlar/Securityalcertpoliciesoperationresults/Read | Sunucu tehdit algılama işlemi sonucunun listesini döndürür. |
> | Action | Microsoft. Dbformyısql/işlemler/okuma | MySQL Işlemleri listesini döndürün. |
> | Action | Microsoft. Dbformyısql/Performancekatmanları/okuma | Kullanılabilir performans katmanları listesini döndürür. |
> | Action | Microsoft. Dbformyısql/Register/Action | MySQL kaynak sağlayıcısını Kaydet |
> | Action | Microsoft. Dbformyısql/Servers/Administrators/Delete | MySQL Server 'ın var olan bir yöneticisini siler. |
> | Action | Microsoft. Dbformyısql/Servers/Administrators/Read | MySQL Server yöneticilerinin bir listesini alır. |
> | Action | Microsoft. Dbformyısql/Servers/Administrators/Write | Belirtilen parametrelerle MySQL Server Yöneticisi oluşturur veya güncelleştirir. |
> | Action | Microsoft. Dbformyısql/Servers/danışmanları/Createreyorumdedadctionsession/ACTION | Yeni bir öneri eylem oturumu oluştur |
> | Action | Microsoft. Dbformyısql/Servers/danışmanları/okuma | Danışmanlarının listesini döndürün |
> | Action | Microsoft. Dbformyısql/Servers/danışmanları/okuma | Danışman döndürme |
> | Action | Microsoft. Dbformyısql/Servers/danışmanları/recommendedActions/okuma | Önerilen eylemlerin listesini döndür |
> | Action | Microsoft. Dbformyısql/Servers/danışmanları/recommendedActions/okuma | Önerilen eylemlerin listesini döndür |
> | Action | Microsoft. Dbformyısql/Servers/danışmanları/recommendedActions/okuma | Önerilen eylemi döndürür |
> | Action | Microsoft. Dbformyısql/sunucular/yapılandırma/okuma | Bir sunucu için yapılandırmaların listesini döndürün veya belirtilen yapılandırma için özellikleri alır. |
> | Action | Microsoft. Dbformyısql/sunucular/yapılandırma/yazma | Belirtilen yapılandırma için değeri güncelleştir |
> | Action | Microsoft. Dbformyısql/Servers/veritabanları/Delete | Mevcut bir MySQL veritabanını siler. |
> | Action | Microsoft. Dbformyısql/Servers/veritabanları/okuma | MySQL veritabanlarının listesini döndürün veya belirtilen veritabanının özelliklerini alır. |
> | Action | Microsoft. Dbformyısql/Servers/veritabanları/Write | Belirtilen parametrelerle bir MySQL veritabanı oluşturur veya belirtilen veritabanı için özellikleri güncelleştirir. |
> | Action | Microsoft. Dbformyısql/Servers/Delete | Var olan bir sunucuyu siler. |
> | Action | Microsoft. Dbformyısql/Servers/firewallRules/Delete | Mevcut bir güvenlik duvarı kuralını siler. |
> | Action | Microsoft. Dbformyısql/Servers/firewallRules/Read | Bir sunucu için güvenlik duvarı kuralları listesini döndürün veya belirtilen güvenlik duvarı kuralının özelliklerini alır. |
> | Action | Microsoft. Dbformyısql/Servers/firewallRules/Write | Belirtilen parametrelerle bir güvenlik duvarı kuralı oluşturur veya var olan bir kuralı güncelleştirir. |
> | Action | Microsoft. Dbformyısql/Servers/logFiles/Read | PostgreSQL LogFiles listesini döndürün. |
> | Action | Microsoft. Dbformyısql/Servers/Providers/Microsoft. Insights/diagnosticSettings/Read | Kaynak için ayrılan ayarı alır |
> | Action | Microsoft. Dbformyısql/Servers/Providers/Microsoft. Insights/diagnosticSettings/Write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. Dbformyısql/Servers/Providers/Microsoft. Insights/logDefinitions/Read | MySQL sunucuları için kullanılabilir günlükleri alır |
> | Action | Microsoft. Dbformyısql/Servers/Providers/Microsoft. Insights/metricDefinitions/Read | Veritabanları için kullanılabilir ölçümlerin dönüş türleri |
> | Action | Microsoft. Dbformyısql/Servers/Querymetin'ler/eylem | Sorgu listesi için metin döndürme |
> | Action | Microsoft. Dbformyısql/Servers/Querymetin'ler/eylem | Bir sorgunun metnini döndürme |
> | Action | Microsoft. Dbformyısql/Servers/Read | Sunucu listesini döndürün veya belirtilen sunucunun özelliklerini alır. |
> | Action | Microsoft. Dbformyısql/Servers/recoverableServers/Read | Kurtarılabilir MySQL sunucusu bilgilerini döndür |
> | Action | Microsoft. Dbformyısql/sunucular/çoğaltmalar/okuma | MySQL sunucusunun okuma çoğaltmalarını al |
> | Action | Microsoft. Dbformyısql/sunucular/yeniden Başlat/eylem | Belirli bir sunucuyu yeniden başlatır. |
> | Action | Microsoft. Dbformyısql/Servers/Securityalcertpolicies/Read | Belirli bir sunucuda yapılandırılmış sunucu tehdit algılama ilkesinin ayrıntılarını alma |
> | Action | Microsoft. Dbformyısql/Servers/Securityalcertpolicies/Write | Belirli bir sunucu için sunucu tehdit algılama ilkesini değiştirme |
> | Action | Microsoft. Dbformyısql/Servers/Topquerystatıstıcs/Read | En üstteki sorgular için sorgu Istatistikleri listesini döndürür. |
> | Action | Microsoft. Dbformyısql/Servers/Topquerystatıstıcs/Read | Sorgu Istatistiği döndürme |
> | Action | Microsoft. Dbformyısql/Servers/updateConfigurations/Action | Belirtilen sunucu için Güncelleştirme yapılandırması |
> | Action | Microsoft. Dbformyısql/Servers/virtualNetworkRules/Delete | Var olan bir sanal ağ kuralını siler |
> | Action | Microsoft. Dbformyısql/Servers/virtualNetworkRules/Read | Sanal ağ kurallarının listesini döndürün veya belirtilen sanal ağ kuralı için özellikleri alır. |
> | Action | Microsoft. Dbformyısql/Servers/virtualNetworkRules/Write | Belirtilen parametrelere sahip bir sanal ağ kuralı oluşturur veya belirtilen sanal ağ kuralı için özellikleri veya etiketleri güncelleştirir. |
> | Action | Microsoft. Dbformyısql/Servers/Waitstatıstıcs/Read | Örnek için bekleme istatistiklerini döndür |
> | Action | Microsoft. Dbformyısql/Servers/Waitstatıstıcs/Read | Bekleme istatistiği döndürme |
> | Action | Microsoft. Dbformyısql/Servers/Write | Belirtilen parametrelere sahip bir sunucu oluşturur veya belirtilen sunucu için özellikleri veya etiketleri güncelleştirir. |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. DBforPostgreSQL/Checknameavaılabılıty/Action | Belirli bir abonelik için verilen sunucu adının dünya genelinde sağlanması için kullanılabilir olup olmadığını doğrulayın. |
> | Action | Microsoft. DBforPostgreSQL/konumlar/azureAsyncOperation/Read | PostgreSQL sunucusu Işlem sonuçlarını döndür |
> | Action | Microsoft. DBforPostgreSQL/konumlar/operationResults/Read | ResourceGroup tabanlı PostgreSQL sunucusu Işlem sonuçlarını döndür |
> | Action | Microsoft. DBforPostgreSQL/konumlar/operationResults/Read | PostgreSQL sunucusu Işlem sonuçlarını döndür |
> | Action | Microsoft. DBforPostgreSQL/konumlar/Performancekatmanları/okuma | Kullanılabilir performans katmanları listesini döndürür. |
> | Action | Microsoft. DBforPostgreSQL/konumlar/privateEndpointConnectionAzureAsyncOperation/Read | Özel bir uç nokta bağlantısı işleminin sonucunu alır |
> | Action | Microsoft. DBforPostgreSQL/konumlar/privateEndpointConnectionOperationResults/Read | Özel bir uç nokta bağlantısı işleminin sonucunu alır |
> | Action | Microsoft. DBforPostgreSQL/konumlar/privateEndpointConnectionProxyAzureAsyncOperation/Read | Özel bir uç noktası bağlantı proxy 'si işleminin sonucunu alır |
> | Action | Microsoft. DBforPostgreSQL/konumlar/privateEndpointConnectionProxyOperationResults/Read | Özel bir uç noktası bağlantı proxy 'si işleminin sonucunu alır |
> | Action | Microsoft. DBforPostgreSQL/konumlar/securityAlertPoliciesAzureAsyncOperation/Read | Sunucu tehdit algılama işlemi sonucunun listesini döndürür. |
> | Action | Microsoft. DBforPostgreSQL/konumlar/Securityalcertpoliciesoperationresults/Read | Sunucu tehdit algılama işlemi sonucunun listesini döndürür. |
> | Action | Microsoft. DBforPostgreSQL/işlemler/okuma | PostgreSQL Işlemleri listesini döndürün. |
> | Action | Microsoft. DBforPostgreSQL/Performancekatmanlarında/Read | Kullanılabilir performans katmanları listesini döndürür. |
> | Action | Microsoft. DBforPostgreSQL/Register/Action | PostgreSQL kaynak sağlayıcısını Kaydet |
> | Action | Microsoft. DBforPostgreSQL/sunucular/Yöneticiler/Sil | PostgreSQL sunucusunun var olan bir yöneticisini siler. |
> | Action | Microsoft. DBforPostgreSQL/Servers/Administrators/Read | PostgreSQL sunucu yöneticilerinin bir listesini alır. |
> | Action | Microsoft. DBforPostgreSQL/Servers/Administrators/Write | Belirtilen parametrelerle PostgreSQL Sunucu Yöneticisi oluşturur veya güncelleştirir. |
> | Action | Microsoft. DBforPostgreSQL/sunucular/danışmanlar/okundu | Danışmanlarının listesini döndürün |
> | Action | Microsoft. DBforPostgreSQL/Servers/danışmanlar/recommendedActions/Read | Önerilen eylemlerin listesini döndür |
> | Action | Microsoft. DBforPostgreSQL/Servers/danışmanlar/recommendedActionSessions/Action | Öneri yapın |
> | Action | Microsoft. DBforPostgreSQL/sunucular/konfigürasyonlar/okuma | Bir sunucu için yapılandırmaların listesini döndürün veya belirtilen yapılandırma için özellikleri alır. |
> | Action | Microsoft. DBforPostgreSQL/sunucular/konfigürasyonlar/yazma | Belirtilen yapılandırma için değeri güncelleştir |
> | Action | Microsoft. DBforPostgreSQL/Servers/veritabanları/Delete | Var olan bir PostgreSQL veritabanını siler. |
> | Action | Microsoft. DBforPostgreSQL/sunucular/veritabanları/okuma | PostgreSQL veritabanlarının listesini döndürün veya belirtilen veritabanının özelliklerini alır. |
> | Action | Microsoft. DBforPostgreSQL/Servers/veritabanları/Write | Belirtilen parametrelerle bir PostgreSQL veritabanı oluşturur veya belirtilen veritabanı için özellikleri güncelleştirir. |
> | Action | Microsoft. DBforPostgreSQL/Servers/Delete | Var olan bir sunucuyu siler. |
> | Action | Microsoft. DBforPostgreSQL/Servers/firewallRules/Delete | Mevcut bir güvenlik duvarı kuralını siler. |
> | Action | Microsoft. DBforPostgreSQL/Servers/firewallRules/Read | Bir sunucu için güvenlik duvarı kuralları listesini döndürün veya belirtilen güvenlik duvarı kuralının özelliklerini alır. |
> | Action | Microsoft. DBforPostgreSQL/Servers/firewallRules/Write | Belirtilen parametrelerle bir güvenlik duvarı kuralı oluşturur veya var olan bir kuralı güncelleştirir. |
> | Action | Microsoft. DBforPostgreSQL/Servers/logFiles/Read | PostgreSQL LogFiles listesini döndürün. |
> | Action | Microsoft. DBforPostgreSQL/Servers/Privateendpointconnectionproxy 'Leri/silme | Var olan bir özel uç nokta bağlantı proxy 'sini siler |
> | Action | Microsoft. DBforPostgreSQL/Servers/Privateendpointconnectionproxy/okuma | Özel uç nokta bağlantı proxy 'leri listesini döndürür veya belirtilen özel uç nokta bağlantı proxy 'sinin özelliklerini alır. |
> | Action | Microsoft. DBforPostgreSQL/Servers/Privateendpointconnectionproxy/doğrulama/eylem | Özel bir uç nokta bağlantısını doğrular NRP tarafında çağrı oluştur |
> | Action | Microsoft. DBforPostgreSQL/Servers/Privateendpointconnectionproxy/yazma | Belirtilen parametrelerle özel bir uç nokta bağlantı proxy 'si oluşturur veya belirtilen özel uç nokta bağlantısı proxy 'si için özellikleri veya etiketleri güncelleştirir. |
> | Action | Microsoft. DBforPostgreSQL/Servers/privateEndpointConnections/Delete | Var olan bir özel uç nokta bağlantısını siler |
> | Action | Microsoft. DBforPostgreSQL/Servers/privateEndpointConnections/Read | Özel uç nokta bağlantılarının listesini döndürür veya belirtilen özel uç nokta bağlantısının özelliklerini alır. |
> | Action | Microsoft. DBforPostgreSQL/Servers/privateEndpointConnections/Write | Var olan bir özel uç nokta bağlantısını onaylar veya reddeder |
> | Action | Microsoft. DBforPostgreSQL/Servers/privateLinkResources/Read | Karşılık gelen PostgreSQL sunucusu için özel bağlantı kaynakları alın |
> | Action | Microsoft. DBforPostgreSQL/Servers/Providers/Microsoft. Insights/diagnosticSettings/Read | Kaynak için ayrılan ayarı alır |
> | Action | Microsoft. DBforPostgreSQL/Servers/Providers/Microsoft. Insights/diagnosticSettings/Write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. DBforPostgreSQL/Servers/Providers/Microsoft. Insights/logDefinitions/Read | Postgres sunucuları için kullanılabilir günlükleri alır |
> | Action | Microsoft. DBforPostgreSQL/Servers/Providers/Microsoft. Insights/metricDefinitions/Read | Veritabanları için kullanılabilir ölçümlerin dönüş türleri |
> | Action | Microsoft. DBforPostgreSQL/Servers/Querymetinler/eylem | Bir sorgunun metnini döndürme |
> | Action | Microsoft. DBforPostgreSQL/Servers/Querymetin/Read | Sorgu listesi için metin döndürme |
> | Action | Microsoft. DBforPostgreSQL/Servers/Read | Sunucu listesini döndürün veya belirtilen sunucunun özelliklerini alır. |
> | Action | Microsoft. DBforPostgreSQL/Servers/recoverableServers/Read | Kurtarılabilir PostgreSQL sunucu bilgilerini döndürür |
> | Action | Microsoft. DBforPostgreSQL/sunucular/çoğaltmalar/okuma | Bir PostgreSQL sunucusunun okuma çoğaltmalarını al |
> | Action | Microsoft. DBforPostgreSQL/sunucular/yeniden Başlat/eylem | Belirli bir sunucuyu yeniden başlatır. |
> | Action | Microsoft. DBforPostgreSQL/Servers/Securityalcertpolicies/Read | Belirli bir sunucuda yapılandırılmış sunucu tehdit algılama ilkesinin ayrıntılarını alma |
> | Action | Microsoft. DBforPostgreSQL/Servers/Securityalcertpolicies/Write | Belirli bir sunucu için sunucu tehdit algılama ilkesini değiştirme |
> | Action | Microsoft. DBforPostgreSQL/Servers/Topquerystatıstıcs/Read | En üstteki sorgular için sorgu Istatistikleri listesini döndürür. |
> | Action | Microsoft. DBforPostgreSQL/Servers/updateConfigurations/Action | Belirtilen sunucu için Güncelleştirme yapılandırması |
> | Action | Microsoft. DBforPostgreSQL/Servers/virtualNetworkRules/Delete | Var olan bir sanal ağ kuralını siler |
> | Action | Microsoft. DBforPostgreSQL/Servers/virtualNetworkRules/Read | Sanal ağ kurallarının listesini döndürün veya belirtilen sanal ağ kuralı için özellikleri alır. |
> | Action | Microsoft. DBforPostgreSQL/Servers/virtualNetworkRules/Write | Belirtilen parametrelere sahip bir sanal ağ kuralı oluşturur veya belirtilen sanal ağ kuralı için özellikleri veya etiketleri güncelleştirir. |
> | Action | Microsoft. DBforPostgreSQL/sunucular/waitStatistics/okuma | Örnek için bekleme istatistiklerini döndür |
> | Action | Microsoft. DBforPostgreSQL/Servers/Write | Belirtilen parametrelere sahip bir sunucu oluşturur veya belirtilen sunucu için özellikleri veya etiketleri güncelleştirir. |
> | Action | Microsoft. DBforPostgreSQL/serversv2/Configurations/Read | Bir sunucu için yapılandırmaların listesini döndürün veya belirtilen yapılandırma için özellikleri alır. |
> | Action | Microsoft. DBforPostgreSQL/serversv2/Configurations/Write | Belirtilen yapılandırma için değeri güncelleştir |
> | Action | Microsoft. DBforPostgreSQL/serversv2/Delete | Var olan bir sunucuyu siler. |
> | Action | Microsoft. DBforPostgreSQL/serversv2/firewallRules/Delete | Mevcut bir güvenlik duvarı kuralını siler. |
> | Action | Microsoft. DBforPostgreSQL/serversv2/firewallRules/Read | Bir sunucu için güvenlik duvarı kuralları listesini döndürün veya belirtilen güvenlik duvarı kuralının özelliklerini alır. |
> | Action | Microsoft. DBforPostgreSQL/serversv2/firewallRules/Write | Belirtilen parametrelerle bir güvenlik duvarı kuralı oluşturur veya var olan bir kuralı güncelleştirir. |
> | Action | Microsoft. DBforPostgreSQL/serversv2/Providers/Microsoft. Insights/diagnosticSettings/Read | Kaynak için ayrılan ayarı alır |
> | Action | Microsoft. DBforPostgreSQL/serversv2/Providers/Microsoft. Insights/diagnosticSettings/Write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. DBforPostgreSQL/serversv2/Providers/Microsoft. Insights/logDefinitions/Read | Postgres sunucuları için kullanılabilir günlükleri alır |
> | Action | Microsoft. DBforPostgreSQL/serversv2/Providers/Microsoft. Insights/metricDefinitions/Read | Veritabanları için kullanılabilir ölçümlerin dönüş türleri |
> | Action | Microsoft. DBforPostgreSQL/serversv2/Read | Sunucu listesini döndürün veya belirtilen sunucunun özelliklerini alır. |
> | Action | Microsoft. DBforPostgreSQL/serversv2/updateConfigurations/Action | Belirtilen sunucu için Güncelleştirme yapılandırması |
> | Action | Microsoft. DBforPostgreSQL/serversv2/Write | Belirtilen parametrelere sahip bir sunucu oluşturur veya belirtilen sunucu için özellikleri veya etiketleri güncelleştirir. |

## <a name="microsoftdevices"></a>Microsoft. Devices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Devices/Account/diagnosticSettings/Read | Kaynak için tanılama ayarını alır |
> | Action | Microsoft. Devices/Account/diagnosticSettings/Write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. Devices/Account/logDefinitions/Read | Iothub hizmeti için kullanılabilir günlük tanımlarını alır |
> | Action | Microsoft. Devices/Account/metricDefinitions/Read | Iothub hizmeti için kullanılabilir ölçümleri alır |
> | Action | Microsoft. Devices/Checknameavaılabılıty/Action | Iothub adının kullanılabilir olup olmadığını denetle |
> | Action | Microsoft. Devices/Checknameavaılabılıty/Action | Iothub adının kullanılabilir olup olmadığını denetle |
> | Action | Microsoft. Devices/Checkprovisioningservicenameavaılabılıty/Action | Sağlama hizmeti adının kullanılabilir olup olmadığını denetleyin |
> | Action | Microsoft. Devices/Checkprovisioningservicenameavaılabılıty/Action | Sağlama hizmeti adının kullanılabilir olup olmadığını denetleyin |
> | Action | Microsoft. Devices/Digitaltwıns/Delete | Var olan bir dijital TWINS hesabını ve tüm alt öğelerini Sil |
> | Action | Microsoft. Devices/Digitaltwıns/operationresults/Read | Bir işlemin durumunu dijital bir TWINS hesabına göre alma |
> | Action | Microsoft. Devices/Digitaltwıns/Read | Abonelikle ilişkili dijital TWINS hesaplarının bir listesini alır |
> | Action | Microsoft. Devices/Digitaltwıns/SKU/okuma | Dijital TWINS hesapları için geçerli SKU 'ların bir listesini alın |
> | Action | Microsoft. Devices/Digitaltwıns/Write | Yeni bir digitial Twins hesabı oluşturun |
> | Action | Microsoft. Devices/Elaun havuzları/diagnosticSettings/Read | Kaynak için tanılama ayarını alır |
> | Action | Microsoft. Devices/Elaun havuzları/diagnosticSettings/Write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/sertifikalar/Sil | Sertifikayı siler |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/Certificates/Generatedoğrulamaları Icationcode/Action | Doğrulama kodu oluştur |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/sertifikalar/okuma | Sertifikayı alır |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/sertifikalar/doğrula/eylem | Sertifika kaynağını doğrula |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/sertifikalar/yazma | Sertifika Oluştur veya güncelleştir |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/Delete | Iothub kiracı kaynağını silme |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/diagnosticSettings/Read | Kaynak için tanılama ayarını alır |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/diagnosticSettings/Write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/eventHubEndpoints/consumerGroups/Delete | EventHub tüketici grubunu sil |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/eventHubEndpoints/consumerGroups/Read | EventHub tüketici gruplarını al |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/eventHubEndpoints/consumerGroups/Write | EventHub Tüketici grubu oluştur |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/exportDevices/Action | Cihazları dışarı aktar |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/getStats/Read | Iothub kiracı istatistikleri kaynağını alır |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/ımportdevices/Action | Cihazları içeri aktar |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/iotHubKeys/ListKeys/Action | Iothub kiracı anahtarını alır |
> | Action | Microsoft. Devices/Elaun havuzları/Iothubkiracılar/işler/okuma | Verilen ıothub üzerinde gönderilen Iş ayrıntılarını al |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/listKeys/Action | Iothub kiracı anahtarlarını alır |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/logDefinitions/Read | Iothub hizmeti için kullanılabilir günlük tanımlarını alır |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/metricDefinitions/okuma | Iothub hizmeti için kullanılabilir ölçümleri alır |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/Quotaölçümlerini/Read | Kota ölçümlerini al |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/Read | Iothub kiracı kaynağını alır |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/yönlendirme/rotalar/$testall/Action | Tüm mevcut yollara karşı bir iletiyi test etme |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/yönlendirme/rotalar/$testnew/Action | Bir iletiyi belirtilen test rotasına karşı test etme |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/routingEndpointsHealth/Read | Bir ıothub için tüm yönlendirme uç noktalarının sistem durumunu alır |
> | Action | Microsoft. Devices/Elaun havuzları/ıothubkiracılar/Write | Iothub kiracı kaynağı oluşturma veya güncelleştirme |
> | Action | Microsoft. Devices/Elaun havuzları/metricDefinitions/okuma | Iothub hizmeti için kullanılabilir ölçümleri alır |
> | Action | Microsoft. Devices/iotHubs/sertifikalar/Sil | Sertifikayı siler |
> | Action | Microsoft. Devices/ıothubs/Certificates/Generatedoğrulamaları Icationcode/Action | Doğrulama kodu oluştur |
> | Action | Microsoft. Devices/iotHubs/sertifikalar/okuma | Sertifikayı alır |
> | Action | Microsoft. Devices/iotHubs/sertifikalar/doğrula/eylem | Sertifika kaynağını doğrula |
> | Action | Microsoft. Devices/iotHubs/sertifikalar/yazma | Sertifika Oluştur veya güncelleştir |
> | Action | Microsoft. Devices/ıothubs/Delete | Iothub kaynağını Sil |
> | Action | Microsoft. Devices/ıothubs/diagnosticSettings/Read | Kaynak için tanılama ayarını alır |
> | Action | Microsoft. Devices/ıothubs/diagnosticSettings/Write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. Devices/ıothubs/eventGridFilters/Delete | Event Grid filtresini siler |
> | Action | Microsoft. Devices/ıothubs/eventGridFilters/Read | Event Grid filtresini alır |
> | Action | Microsoft. Devices/ıothubs/eventGridFilters/Write | Yeni oluştur veya var olan Event Grid filtresini Güncelleştir |
> | Action | Microsoft. Devices/ıothubs/eventHubEndpoints/consumerGroups/Delete | EventHub tüketici grubunu sil |
> | Action | Microsoft. Devices/ıothubs/eventHubEndpoints/consumerGroups/Read | EventHub tüketici gruplarını al |
> | Action | Microsoft. Devices/ıothubs/eventHubEndpoints/consumerGroups/Write | EventHub Tüketici grubu oluştur |
> | Action | Microsoft. Devices/ıothubs/exportDevices/Action | Cihazları dışarı aktar |
> | Action | Microsoft. Devices/ıothubs/ımportdevices/Action | Cihazları içeri aktar |
> | Action | Microsoft. Devices/ıothubs/iotHubKeys/ListKeys/Action | Verilen ad için ıothub anahtarını al |
> | Action | Microsoft. Devices/ıothubs/iotHubStats/Read | Get IotHub Statistics |
> | Action | Microsoft. Devices/ıothubs/Jobs/Read | Verilen ıothub üzerinde gönderilen Iş ayrıntılarını al |
> | Action | Microsoft. Devices/ıothubs/ListKeys/Action | Tüm ıothub anahtarlarını al |
> | Action | Microsoft. Devices/ıothubs/logDefinitions/Read | Iothub hizmeti için kullanılabilir günlük tanımlarını alır |
> | Action | Microsoft. Devices/ıothubs/metricDefinitions/Read | Iothub hizmeti için kullanılabilir ölçümleri alır |
> | Action | Microsoft. Devices/ıothubs/operationresults/Read | Işlem sonucunu al (eski API) |
> | Action | Microsoft. Devices/ıothubs/Quotaölçümlerini/Read | Kota ölçümlerini al |
> | Action | Microsoft. Devices/ıothubs/Read | Iothub kaynak (ler) i alır |
> | Action | Microsoft. Devices/ıothubs/Routing/$testall/Action | Tüm mevcut yollara karşı bir iletiyi test etme |
> | Action | Microsoft. Devices/ıothubs/Routing/$testnew/Action | Bir iletiyi belirtilen test rotasına karşı test etme |
> | Action | Microsoft. Devices/ıothubs/routingEndpointsHealth/Read | Bir ıothub için tüm yönlendirme uç noktalarının sistem durumunu alır |
> | Action | Microsoft. Devices/ıothubs/SKU/okuma | Geçerli ıothub SKU 'Larını al |
> | Action | Microsoft. Devices/ıothubs/Write | Iothub kaynağı oluşturma veya güncelleştirme |
> | Action | Microsoft. cihazlar/konumlar/operationresults/Read | Konum tabanlı Işlem sonucunu al |
> | Action | Microsoft. Devices/operationresults/Read | İşlem Sonucunu Al |
> | Action | Microsoft. Devices/işlemler/okuma | Tüm ResourceProvider Işlemlerini al |
> | Action | Microsoft. Devices/provisioningServices/sertifikalar/Sil | Sertifikayı siler |
> | Action | Microsoft. Devices/provisioningServices/Certificates/Generatedoğrulamaları Icationcode/Action | Doğrulama kodu oluştur |
> | Action | Microsoft. Devices/provisioningServices/sertifikalar/okuma | Sertifikayı alır |
> | Action | Microsoft. Devices/provisioningServices/sertifikalar/doğrula/eylem | Sertifika kaynağını doğrula |
> | Action | Microsoft. Devices/provisioningServices/sertifikalar/yazma | Sertifika Oluştur veya güncelleştir |
> | Action | Microsoft. Devices/provisioningServices/Delete | Iotdps kaynağını Sil |
> | Action | Microsoft. Devices/provisioningServices/diagnosticSettings/Read | Kaynak için tanılama ayarını alır |
> | Action | Microsoft. Devices/provisioningServices/diagnosticSettings/Write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. Devices/provisioningServices/anahtarlar/ListKeys/Action | Anahtar adı için ıotdps anahtarlarını al |
> | Action | Microsoft. Devices/provisioningServices/ListKeys/Action | Tüm ıotdps anahtarlarını al |
> | Action | Microsoft. Devices/provisioningServices/logDefinitions/Read | Sağlama hizmeti için kullanılabilir günlük tanımlarını alır |
> | Action | Microsoft. Devices/provisioningServices/metricDefinitions/Read | Sağlama hizmeti için kullanılabilir ölçümleri alır |
> | Action | Microsoft. Devices/provisioningServices/operationresults/Read | DPS Işlem sonucunu al |
> | Action | Microsoft. Devices/provisioningServices/okuma | Iotdps kaynağını al |
> | Action | Microsoft. Devices/provisioningServices/SKU 'lar/okuma | Geçerli ıotdps SKU 'Larını al |
> | Action | Microsoft. Devices/provisioningServices/Write | Iotdps kaynağı oluşturma |
> | Action | Microsoft. Devices/Register/Action | Iothub kaynak sağlayıcısı için aboneliği kaydedin ve ıothub kaynaklarının oluşturulmasını etkinleştirilir |
> | Action | Microsoft. Devices/Register/Action | Iothub kaynak sağlayıcısı için aboneliği kaydedin ve ıothub kaynaklarının oluşturulmasını etkinleştirilir |
> | Action | Microsoft. Devices/kullanımlar/Read | Bu sağlayıcının abonelik kullanım ayrıntılarını al. |

## <a name="microsoftdevspaces"></a>Microsoft. DevSpaces

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. DevSpaces/denetleyiciler/silme | Azure Dev Spaces denetleyicisi ve veri düzlemi hizmetlerini silme |
> | Action | Microsoft. DevSpaces/denetleyiciler/listConnectionDetails/Action | Azure Dev Spaces denetleyicisinin altyapısının bağlantı ayrıntılarını listeleyin |
> | Action | Microsoft. DevSpaces/denetleyiciler/okuma | Azure Dev Spaces denetleyicisi özelliklerini okuyun |
> | Action | Microsoft. DevSpaces/denetleyiciler/yazma | Azure Dev Spaces denetleyicisi özellikleri oluştur veya güncelleştir |
> | Action | Microsoft. DevSpaces/konumlar/checkContainerHostMapping/ACTION | Bir kapsayıcı konağı için var olan denetleyici eşlemesini denetleyin |
> | Action | Microsoft. DevSpaces/konumlar/operationresults/Read | Zaman uyumsuz bir işlemin durumunu oku |
> | Action | Microsoft. DevSpaces/Register/ACTION | Microsoft dev Spaces kaynak sağlayıcısını abonelikle kaydetme |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. DevTestLab/labCenters/Delete | Laboratuvar merkezlerini silin. |
> | Action | Microsoft. DevTestLab/labCenters/okuma | Laboratuvar merkezlerini okuyun. |
> | Action | Microsoft. DevTestLab/labCenters/Write | Laboratuvar merkezleri ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/artifactSources/armTemplates/Read | Azure Resource Manager şablonlarını okuyun. |
> | Action | Microsoft. DevTestLab/Labs/artifactSources/yapıtlar/GenerateArmTemplate/Action | Belirtilen yapıt için ARM şablonu oluşturur, gerekli dosyaları bir depolama hesabına yükler ve oluşturulan yapıtı doğrular. |
> | Action | Microsoft. DevTestLab/Labs/artifactSources/yapıtlar/okuma | Yapıtları okuyun. |
> | Action | Microsoft. DevTestLab/Labs/artifactSources/Sil | Yapıt kaynaklarını silin. |
> | Action | Microsoft. DevTestLab/Labs/artifactSources/okuma | Yapıt kaynaklarını okuyun. |
> | Action | Microsoft. DevTestLab/Labs/artifactSources/yazma | Yapıt kaynakları ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/ClaimAnyVm/eylem | Laboratuvarda rastgele bir çakışan sanal makine talep edin. |
> | Action | Microsoft. DevTestLab/Labs/maliyetler/okuma | Maliyetleri okuyun. |
> | Action | Microsoft. DevTestLab/Labs/maliyetler/yazma | Maliyetleri ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/CreateEnvironment/eylem | Laboratuvarda sanal makineler oluşturun. |
> | Action | Microsoft. DevTestLab/Labs/CustomImages/Delete | Özel görüntüleri silin. |
> | Action | Microsoft. DevTestLab/Labs/CustomImages/Read | Özel görüntüleri okuyun. |
> | Action | Microsoft. DevTestLab/Labs/CustomImages/Write | Özel görüntüler ekleme veya değiştirme. |
> | Action | Microsoft. DevTestLab/Labs/Delete | Laboratuvarları silin. |
> | Action | Microsoft. DevTestLab/Labs/EnsureCurrentUserProfile/Action | Geçerli kullanıcının laboratuvarda geçerli bir profile sahip olduğundan emin olun. |
> | Action | Microsoft. DevTestLab/Labs/ExportResourceUsage/Action | Laboratuvar kaynak kullanımını bir depolama hesabına dışarı aktarır |
> | Action | Microsoft.DevTestLab/labs/formulas/delete | Formülleri silin. |
> | Action | Microsoft. DevTestLab/Labs/formüller/okuma | Formülleri okuyun. |
> | Action | Microsoft.DevTestLab/labs/formulas/write | Formüller ekleme veya değiştirme. |
> | Action | Microsoft. DevTestLab/Labs/Gallerımages/okuma | Galeri görüntülerini okuyun. |
> | Action | Microsoft. DevTestLab/Labs/GenerateUploadUri/Action | Özel disk görüntülerini laboratuvara yüklemek için bir URI oluşturun. |
> | Action | Microsoft. DevTestLab/Labs/ımportvirtualmachine/Action | Bir sanal makineyi farklı bir laboratuvara aktarın. |
> | Action | Microsoft. DevTestLab/Labs/Listvhd/eylem | Özel görüntü oluşturmak için kullanılabilir disk görüntülerini listeleyin. |
> | Action | Microsoft. DevTestLab/Labs/notificationChannels/Delete | Bildirim kanallarını silin. |
> | Action | Microsoft. DevTestLab/Labs/notificationChannels/Notify/ACTION | Belirtilen kanala bildirim gönderin. |
> | Action | Microsoft. DevTestLab/Labs/notificationChannels/Read | Bildirim kanallarını okuyun. |
> | Action | Microsoft. DevTestLab/Labs/notificationChannels/Write | Bildirim kanalları ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/policySets/EvaluatePolicies/Action | Laboratuvar ilkesini değerlendirir. |
> | Action | Microsoft. DevTestLab/Labs/policySets/policies/Delete | İlkeleri silin. |
> | Action | Microsoft. DevTestLab/Labs/policySets/policies/Read | İlkeleri okuyun. |
> | Action | Microsoft. DevTestLab/Labs/policySets/policies/Write | İlke ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/policySets/Read | İlke kümelerini okuyun. |
> | Action | Microsoft. DevTestLab/Labs/Read | Labs 'i okuyun. |
> | Action | Microsoft. DevTestLab/Labs/zamanlamalar/silme | Zamanlamaları silin. |
> | Action | Microsoft. DevTestLab/Labs/zamanlamalar/yürütme/eylem | Bir zamanlama yürütün. |
> | Action | Microsoft. DevTestLab/Labs/zamanlamalar/ListApplicable/Action | Tüm geçerli zamanlamaları listeler |
> | Action | Microsoft. DevTestLab/Labs/zamanlamalar/okuma | Zamanlamaları okuyun. |
> | Action | Microsoft. DevTestLab/Labs/zamanlamalar/yazma | Zamanlamaları ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/Servicerunanlar/Delete | Hizmet çalıştıranlar silme. |
> | Action | Microsoft. DevTestLab/Labs/Servicerunanlar/okuma | Hizmet çalıştıranlar bölümünü okuyun. |
> | Action | Microsoft. DevTestLab/Labs/Servicerunanlar/Write | Hizmet çalıştıranlar ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/Sharedgaleriler/silme | Paylaşılan galerileri silin. |
> | Action | Microsoft. DevTestLab/Labs/Sharedgaleriler/okuma | Paylaşılan galerileri okuyun. |
> | Action | Microsoft. DevTestLab/Labs/Sharedgaleriler/sharedImages/Delete | Paylaşılan görüntüleri silin. |
> | Action | Microsoft. DevTestLab/Labs/Sharedgaleriler/sharedImages/Read | Paylaşılan görüntüleri okuyun. |
> | Action | Microsoft. DevTestLab/Labs/Sharedgaleriler/sharedImages/Write | Paylaşılan görüntüleri ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/Sharedgaleriler/yazma | Paylaşılan galeriler ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/kullanıcılar/Sil | Kullanıcı profillerini silin. |
> | Action | Microsoft. DevTestLab/Labs/kullanıcılar/diskler/Iliştirme/eylem | Diskin sanal makineye kiralanmasını ekleyin ve oluşturun. |
> | Action | Microsoft. DevTestLab/Labs/kullanıcılar/diskler/Sil | Diskleri silin. |
> | Action | Microsoft. DevTestLab/Labs/kullanıcılar/diskler/ayırma/eylem | Sanal makineye bağlı disk kiralanmasını ayırın ve bölün. |
> | Action | Microsoft. DevTestLab/Labs/kullanıcılar/diskler/okuma | Diskleri okuyun. |
> | Action | Microsoft. DevTestLab/Labs/kullanıcılar/diskler/yazma | Diskleri ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/kullanıcılar/ortamlar/Sil | Ortamları silin. |
> | Action | Microsoft. DevTestLab/Labs/kullanıcılar/ortamlar/okuma | Ortamları okuyun. |
> | Action | Microsoft. DevTestLab/Labs/kullanıcılar/ortamlar/Write | Ortamları ekleme veya değiştirme. |
> | Action | Microsoft. DevTestLab/Labs/kullanıcılar/okuma | Kullanıcı profillerini okuyun. |
> | Action | Microsoft. DevTestLab/Labs/kullanıcılar/gizlilikler/Delete | Gizli dizileri silin. |
> | Action | Microsoft. DevTestLab/Labs/kullanıcılar/gizlilikler/Read | Gizli dizileri okuyun. |
> | Action | Microsoft. DevTestLab/Labs/kullanıcılar/gizlilikler/Write | Gizli dizileri ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/Users/Serviceyapılar/Sil | Hizmet yapılarını silin. |
> | Action | Microsoft. DevTestLab/Labs/Users/Serviceyapılar/Listapperepblezamanlamalar/eylem | Varsa, geçerli başlatma/durdurma zamanlamalarını listeler. |
> | Action | Microsoft. DevTestLab/Labs/Users/Serviceyapılar/okuma | Hizmet yapılarını okuyun. |
> | Action | Microsoft. DevTestLab/Labs/Users/Serviceyapılar/zamanlamalar/silme | Zamanlamaları silin. |
> | Action | Microsoft. DevTestLab/Labs/Users/Serviceyapılar/zamanlamalar/yürütme/eylem | Bir zamanlama yürütün. |
> | Action | Microsoft. DevTestLab/Labs/Users/Serviceyapılar/zamanlamalar/okuma | Zamanlamaları okuyun. |
> | Action | Microsoft. DevTestLab/Labs/Users/Serviceyapılar/zamanlamalar/yazma | Zamanlamaları ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/Users/Serviceyapılar/Başlat/eylem | Bir Service Fabric başlatın. |
> | Action | Microsoft. DevTestLab/Labs/Users/Serviceyapılar/durdur/eylem | Service Fabric 'i durdur |
> | Action | Microsoft. DevTestLab/Labs/Users/Serviceyapılar/yazma | Hizmet yapıları ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/kullanıcılar/yaz | Kullanıcı profilleri ekleme veya değiştirme. |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/AddDataDisk/eylem | Sanal makineye yeni veya mevcut bir veri diski ekleyin. |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/Applyartifdavranır/eylemi | Yapıtları sanal makineye uygulayın. |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/talep/eylem | Mevcut bir sanal makinenin sahipliğini al |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/Delete | Sanal makineleri silin. |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/çıkarılabilir veri diski/eylem | Belirtilen diski sanal makineden ayırın. |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/GetRdpFileContents/Action | Sanal makine için RDP dosyasının içeriğini temsil eden bir dize alır |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/Listapperepblezamanlamalar/eylem | Varsa, geçerli başlatma/durdurma zamanlamalarını listeler. |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/okuma | Sanal makineleri okuyun. |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/yeniden dağıtma/eylem | Bir sanal makineyi yeniden dağıtın |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/yeniden boyutlandır/eylem | Sanal makineyi yeniden boyutlandırın. |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/yeniden Başlat/eylem | Bir sanal makineyi yeniden başlatın. |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/zamanlamalar/silme | Zamanlamaları silin. |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/zamanlamalar/yürütme/eylem | Bir zamanlama yürütün. |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/zamanlamalar/okuma | Zamanlamaları okuyun. |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/zamanlamalar/yazma | Zamanlamaları ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/Başlat/eylem | Bir sanal makine başlatın. |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/durdur/eylem | Bir sanal makineyi durdur |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/TransferDisks/Action | Sanal makineye bağlı tüm veri disklerini geçerli kullanıcıya ait olacak şekilde aktarır. |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/talep/işlem | Mevcut bir sanal makinenin sahipliğini serbest bırakma |
> | Action | Microsoft. DevTestLab/Labs/virtualMachines/Write | Sanal makineler ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/virtualNetworks/Delete | Sanal ağları silin. |
> | Action | Microsoft. DevTestLab/Labs/virtualNetworks/Read | Sanal ağları okuyun. |
> | Action | Microsoft. DevTestLab/Labs/virtualNetworks/Write | Sanal ağları ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/vmPools/Delete | Sanal makine havuzlarını silin. |
> | Action | Microsoft. DevTestLab/Labs/vmPools/Read | Sanal makine havuzlarını okuyun. |
> | Action | Microsoft. DevTestLab/Labs/vmPools/Write | Sanal makine havuzlarını ekleyin veya değiştirin. |
> | Action | Microsoft. DevTestLab/Labs/Write | Labs ekleme veya değiştirme. |
> | Action | Microsoft. DevTestLab/konumlar/işlemler/okuma | Okuma işlemleri. |
> | Action | Microsoft. DevTestLab/Register/Action | Aboneliği kaydeder |
> | Action | Microsoft. DevTestLab/zamanlamalar/silme | Zamanlamaları silin. |
> | Action | Microsoft. DevTestLab/zamanlamalar/yürütme/eylem | Bir zamanlama yürütün. |
> | Action | Microsoft. DevTestLab/zamanlamalar/okuma | Zamanlamaları okuyun. |
> | Action | Microsoft. DevTestLab/zamanlamalar/yeniden hedefle/eylem | Bir zamanlamanın hedef kaynak kimliğini güncelleştirir. |
> | Action | Microsoft. DevTestLab/zamanlamalar/yazma | Zamanlamaları ekleyin veya değiştirin. |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. DocumentDB/databaseAccountNames/Read | Ad kullanılabilirliğini denetler. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/koleksiyonlar/Sil | Bir koleksiyonu silin. Yalnızca API türleri için geçerlidir: ' MongoDB '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/koleksiyonlar/operationResults/Read | Zaman uyumsuz işlemin durumunu okuyun. Yalnızca API türleri için geçerlidir: ' MongoDB '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/koleksiyonlar/okuma | Bir koleksiyonu okuyun veya tüm koleksiyonları listeleyin. Yalnızca API türleri için geçerlidir: ' MongoDB '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/koleksiyonlar/ayarlar/operationResults/Read | Zaman uyumsuz işlemin durumunu okuyun. Yalnızca API türleri için geçerlidir: ' MongoDB '. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/koleksiyonlar/ayarlar/okuma | Koleksiyon verimini okuyun. Yalnızca API türleri için geçerlidir: ' MongoDB '. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/koleksiyonlar/ayarlar/yazma | Koleksiyon verimini güncelleştirin. Yalnızca API türleri için geçerlidir: ' MongoDB '. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/koleksiyonlar/yazma | Koleksiyon oluşturun veya güncelleştirin. Yalnızca API türleri için geçerlidir: ' MongoDB '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/kapsayıcılar/silme | Kapsayıcıyı silin. Yalnızca API türleri için geçerlidir: ' SQL '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/kapsayıcılar/operationResults/Read | Zaman uyumsuz işlemin durumunu okuyun. Yalnızca API türleri için geçerlidir: ' SQL '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/kapsayıcılar/okuma | Bir kapsayıcıyı okuyun veya tüm kapsayıcıları listeleyin. Yalnızca API türleri için geçerlidir: ' SQL '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/kapsayıcılar/ayarlar/operationResults/Read | Zaman uyumsuz işlemin durumunu okuyun. Yalnızca API türleri için geçerlidir: ' SQL '. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/kapsayıcılar/ayarlar/okuma | Kapsayıcı verimini okuyun. Yalnızca API türleri için geçerlidir: ' SQL '. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/kapsayıcılar/ayarlar/yazma | Kapsayıcı verimini güncelleştirme. Yalnızca API türleri için geçerlidir: ' SQL '. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/kapsayıcılar/yazma | Bir kapsayıcı oluşturun veya güncelleştirin. Yalnızca API türleri için geçerlidir: ' SQL '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/Delete | Bir veritabanını silin. Yalnızca API türleri için geçerlidir: ' SQL ', ' MongoDB ', ' gremlin'. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/grafikler/silme | Bir grafiği silin. Yalnızca API türleri için geçerlidir: ' gremlin'. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/grafikler/operationResults/Read | Zaman uyumsuz işlemin durumunu okuyun. Yalnızca API türleri için geçerlidir: ' gremlin'. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/grafikler/okuma | Bir grafiği okuyun veya tüm grafikleri listeleyin. Yalnızca API türleri için geçerlidir: ' gremlin'. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/grafikler/ayarlar/operationResults/Read | Zaman uyumsuz işlemin durumunu okuyun. Yalnızca API türleri için geçerlidir: ' gremlin'. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/grafikler/ayarlar/okuma | Grafik aktarım hızını okuyun. Yalnızca API türleri için geçerlidir: ' gremlin'. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/grafikler/ayarlar/yazma | Grafik aktarım hızını güncelleştirin. Yalnızca API türleri için geçerlidir: ' gremlin'. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/grafikler/yazma | Grafik oluşturun veya güncelleştirin. Yalnızca API türleri için geçerlidir: ' gremlin'. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/operationResults/Read | Zaman uyumsuz işlemin durumunu okuyun. Yalnızca API türleri için geçerlidir: ' SQL ', ' MongoDB ', ' gremlin'. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/okuma | Bir veritabanını okuyun veya tüm veritabanlarını listeleyin. Yalnızca API türleri için geçerlidir: ' SQL ', ' MongoDB ', ' gremlin'. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/Settings/operationResults/Read | Zaman uyumsuz işlemin durumunu okuyun. Yalnızca API türleri için geçerlidir: ' SQL ', ' MongoDB ', ' gremlin'. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/ayarlar/okuma | Veritabanı verimini okuyun. Yalnızca API türleri için geçerlidir: ' SQL ', ' MongoDB ', ' gremlin'. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/ayarlar/yazma | Veritabanı verimini güncelleştirme. Yalnızca API türleri için geçerlidir: ' SQL ', ' MongoDB ', ' gremlin'. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/veritabanları/yazma | Bir veritabanı oluşturun. Yalnızca API türleri için geçerlidir: ' SQL ', ' MongoDB ', ' gremlin'. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/keyspaces/Delete | Anahtar alanını silin. Yalnızca API türleri için geçerlidir: ' Cassandra '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/keyspaces/operationResults/Read | Zaman uyumsuz işlemin durumunu okuyun. Yalnızca API türleri için geçerlidir: ' Cassandra '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/keyspaces/Read | Bir anahtar alanı okuyun veya tüm anahtar alanlarını listeleyin. Yalnızca API türleri için geçerlidir: ' Cassandra '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/keyspaces/Settings/operationResults/Read | Zaman uyumsuz işlemin durumunu okuyun. Yalnızca API türleri için geçerlidir: ' Cassandra '. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/keyspaces/Settings/Read | Anahtar alanı verimini okuyun. Yalnızca API türleri için geçerlidir: ' Cassandra '. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/keyspaces/Settings/Write | Anahtar alanı aktarım hızını güncelleştirin. Yalnızca API türleri için geçerlidir: ' Cassandra '. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/keyspaces/Tables/Delete | Bir tabloyu silin. Yalnızca API türleri için geçerlidir: ' Cassandra '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/keyspaces/Tables/operationResults/Read | Zaman uyumsuz işlemin durumunu okuyun. Yalnızca API türleri için geçerlidir: ' Cassandra '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/keyspaces/Tables/Read | Tablo okuyun veya tüm tabloları listeleyin. Yalnızca API türleri için geçerlidir: ' Cassandra '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/keyspaces/Tables/Settings/operationResults/Read | Zaman uyumsuz işlemin durumunu okuyun. Yalnızca API türleri için geçerlidir: ' Cassandra '. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/api/keyspaces/Tables/Settings/Read | Tablo verimini okuyun. Yalnızca API türleri için geçerlidir: ' Cassandra '. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API 'ler/keyspaces/tablolar/ayarlar/yazma | Tablo verimini güncelleştirme. Yalnızca API türleri için geçerlidir: ' Cassandra '. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API 'ler/keyspaces/tablolar/Write | Tablo oluşturun veya güncelleştirin. Yalnızca API türleri için geçerlidir: ' Cassandra '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/keyspaces/Write | Anahtar alanı oluşturun. Yalnızca API türleri için geçerlidir: ' Cassandra '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/Tables/Delete | Bir tabloyu silin. Yalnızca API türleri için geçerlidir: ' Table '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/Tables/operationResults/Read | Zaman uyumsuz işlemin durumunu okuyun. Yalnızca API türleri için geçerlidir: ' Table '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/Tables/Read | Tablo okuyun veya tüm tabloları listeleyin. Yalnızca API türleri için geçerlidir: ' Table '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/Tables/Settings/operationResults/Read | Zaman uyumsuz işlemin durumunu okuyun. Yalnızca API türleri için geçerlidir: ' Table '. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/Tables/Settings/Read | Tablo verimini okuyun. Yalnızca API türleri için geçerlidir: ' Table '. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/Tables/Settings/Write | Tablo verimini güncelleştirme. Yalnızca API türleri için geçerlidir: ' Table '. Yalnızca ayar türleri için geçerlidir: ' aktarım hızı '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/Tables/Write | Tablo oluşturun veya güncelleştirin. Yalnızca API türleri için geçerlidir: ' Table '. |
> | Action | Microsoft. DocumentDB/databaseAccounts/Backup/Action | Yedeklemeyi yapılandırmak için bir istek gönder |
> | Action | Microsoft. DocumentDB/databaseAccounts/changeResourceGroup/Action | Veritabanı hesabının kaynak grubunu değiştir |
> | Action | Microsoft. DocumentDB/databaseAccounts/veritabanları/koleksiyonlar/metricDefinitions/okuma | Koleksiyon ölçüm tanımlarını okur. |
> | Action | Microsoft. DocumentDB/databaseAccounts/veritabanları/koleksiyonlar/ölçümler/okuma | Koleksiyon ölçümlerini okur. |
> | Action | Microsoft. DocumentDB/databaseAccounts/veritabanları/koleksiyonlar/Partitionkeyrangeıd/ölçümler/okuma | Veritabanı hesabı bölüm anahtarı düzeyi ölçümlerini oku |
> | Action | Microsoft. DocumentDB/databaseAccounts/veritabanları/koleksiyonlar/bölümler/ölçümler/okuma | Veritabanı hesabı bölüm düzeyi ölçümlerini oku |
> | Action | Microsoft. DocumentDB/databaseAccounts/veritabanları/koleksiyonlar/bölümler/okuma | Bir koleksiyondaki veritabanı hesabı bölümlerini okuma |
> | Action | Microsoft. DocumentDB/databaseAccounts/veritabanları/koleksiyonlar/bölümler/kullanımlar/okuma | Veritabanı hesabı bölüm düzeyi kullanımlarını oku |
> | Action | Microsoft. DocumentDB/databaseAccounts/veritabanları/koleksiyonlar/kullanımlar/okuma | Koleksiyon kullanımlarını okur. |
> | Action | Microsoft. DocumentDB/databaseAccounts/veritabanları/metricDefinitions/okuma | Veritabanı ölçüm tanımlarını okur |
> | Action | Microsoft. DocumentDB/databaseAccounts/veritabanları/ölçümler/okuma | Veritabanı ölçümlerini okur. |
> | Action | Microsoft. DocumentDB/databaseAccounts/veritabanları/kullanımlar/okuma | Veritabanı kullanımlarını okur. |
> | Action | Microsoft. DocumentDB/databaseAccounts/Delete | Veritabanı hesaplarını siler. |
> | Action | Microsoft. DocumentDB/databaseAccounts/failoverPriorityChange/Action | Bir veritabanı hesabının bölgelerinin yük devretme önceliklerini değiştirme. Bu, el ile yük devretme işlemi gerçekleştirmek için kullanılır |
> | Action | Microsoft. DocumentDB/databaseAccounts/getBackupPolicy/Action | Veritabanı hesabının yedekleme ilkesini al |
> | Action | Microsoft. DocumentDB/databaseAccounts/listConnectionStrings/Action | Bir veritabanı hesabı için bağlantı dizelerini al |
> | Action | Microsoft. DocumentDB/databaseAccounts/listKeys/Action | Bir veritabanı hesabının anahtarlarını listeleme |
> | Action | Microsoft. DocumentDB/databaseAccounts/metricDefinitions/Read | Veritabanı hesabı ölçüm tanımlarını okur. |
> | Action | Microsoft. DocumentDB/databaseAccounts/ölçümler/okuma | Veritabanı hesabı ölçümlerini okur. |
> | Action | Microsoft. DocumentDB/databaseAccounts/offlineRegion/eylem | Veritabanı hesabının bir bölgesi çevrimdışı.  |
> | Action | Microsoft. DocumentDB/databaseAccounts/onlineRegion/ACTION | Veritabanı hesabının bir bölgesi çevrimiçi. |
> | Action | Microsoft. DocumentDB/databaseAccounts/operationResults/Read | Zaman uyumsuz işlemin durumunu oku |
> | Action | Microsoft. DocumentDB/databaseAccounts/yüzdebirlik/ölçümler/okuma | Gecikme süresi ölçümlerini okuyun |
> | Action | Microsoft. DocumentDB/databaseAccounts/yüzdebirlik/Read | Çoğaltma gecikmeleri yüzdebirlik değeri okuyun |
> | Action | Microsoft. DocumentDB/databaseAccounts/yüzdebirlik/sourceRegion/targetRegion/ölçümler/okuma | Belirli bir kaynak ve hedef bölge için gecikme süresi ölçümlerini okuyun |
> | Action | Microsoft. DocumentDB/databaseAccounts/yüzdebirlik/targetRegion/ölçümler/okuma | Belirli bir hedef bölge için gecikme süresi ölçümlerini okuyun |
> | Action | Microsoft. DocumentDB/databaseAccounts/Privateendpointconnectionproxy/Delete | Veritabanı hesabının özel bir uç noktası bağlantı proxy 'sini Sil |
> | Action | Microsoft. DocumentDB/databaseAccounts/Privateendpointconnectionproxy/okuma | Veritabanı hesabının özel bir uç noktası bağlantı proxy 'sini okuma |
> | Action | Microsoft. DocumentDB/databaseAccounts/Privateendpointconnectionproxy/doğrulama/eylem | Veritabanı hesabının özel bir uç noktası bağlantı proxy 'sini doğrulama |
> | Action | Microsoft. DocumentDB/databaseAccounts/Privateendpointconnectionproxy/yazma | Veritabanı hesabının özel uç nokta bağlantısı proxy 'sini oluşturma veya güncelleştirme |
> | Action | Microsoft. DocumentDB/databaseAccounts/Read | Bir veritabanı hesabını okur. |
> | Action | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Veritabanı hesabı salt okunur anahtarlarını okur. |
> | Action | Microsoft. DocumentDB/databaseAccounts/readonlykeys/Read | Veritabanı hesabı salt okunur anahtarlarını okur. |
> | Action | Microsoft. DocumentDB/databaseAccounts/regenerateKey/ACTION | Bir veritabanı hesabının anahtarlarını döndürme |
> | Action | Microsoft. DocumentDB/databaseAccounts/Region/veritabanları/koleksiyonlar/ölçümler/okuma | Bölgesel koleksiyon ölçümlerini okur. |
> | Action | Microsoft. DocumentDB/databaseAccounts/Region/veritabanları/koleksiyonlar/Partitionkeyrangeıd/ölçümler/okuma | Bölgesel veritabanı hesabı bölüm anahtarı düzeyi ölçümlerini oku |
> | Action | Microsoft. DocumentDB/databaseAccounts/Region/veritabanları/koleksiyonlar/bölümler/ölçümler/okuma | Bölgesel veritabanı hesabı bölüm düzeyi ölçümlerini oku |
> | Action | Microsoft. DocumentDB/databaseAccounts/Region/veritabanları/koleksiyonlar/bölümler/okuma | Bir koleksiyondaki bölgesel veritabanı hesabı bölümlerini okuma |
> | Action | Microsoft. DocumentDB/databaseAccounts/Region/ölçümler/okuma | Bölge ve veritabanı hesabı ölçümlerini okur. |
> | Action | Microsoft. DocumentDB/databaseAccounts/restore/Action | Geri yükleme isteği gönder |
> | Action | Microsoft. DocumentDB/databaseAccounts/kullanımlar/Read | Veritabanı hesabı kullanımlarını okur. |
> | Action | Microsoft. DocumentDB/databaseAccounts/Write | Bir veritabanı hesabını güncelleştirin. |
> | Action | Microsoft. DocumentDB/konumlar/Deletevirtualnetworkoralt ağları/eylem | VirtualNetwork veya subnet 'in silinmekte olduğunu Microsoft. DocumentDB 'ye bildirir |
> | Action | Microsoft. DocumentDB/konumlar/Deletevirtualnetworkoralt ağları/operationResults/Read | Deletevirtualnetworkoraltağlar zaman uyumsuz işleminin durumunu okuyun |
> | Action | Microsoft. DocumentDB/konumlar/operationsStatus/Read | Zaman uyumsuz Işlemlerin durumunu okur |
> | Action | Microsoft. DocumentDB/operationResults/Read | Zaman uyumsuz işlemin durumunu oku |
> | Action | Microsoft. DocumentDB/işlemler/okuma | Microsoft DocumentDB için kullanılabilen okuma işlemleri  |
> | Action | Microsoft. DocumentDB/Register/ACTION |  Abonelik için Microsoft DocumentDB kaynak sağlayıcısını kaydedin |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. DomainRegistration/Checkdomainavaılabılıty/Action | Bir etki alanının satın alma için kullanılabilir olup olmadığını denetleyin |
> | Action | Microsoft. DomainRegistration/Domains/Delete | Var olan bir etki alanını silin. |
> | Action | Microsoft. DomainRegistration/Domains/domainownershiptanýmlayýcýlarý/Delete | Sahiplik tanımlayıcısını sil |
> | Action | Microsoft. DomainRegistration/Domains/domainownershiptanýmlayýcýlarý/Read | Sahiplik tanımlayıcılarını listeleme |
> | Action | Microsoft. DomainRegistration/Domains/domainownershiptanýmlayýcýlarý/Read | Sahiplik tanımlayıcıyı al |
> | Action | Microsoft. DomainRegistration/Domains/domainownershiptanýmlayýcýlarý/Write | Tanımlayıcı oluştur veya güncelleştir |
> | Action | Microsoft. DomainRegistration/Domains/operationresults/Read | Etki alanı işlemi al |
> | Action | Microsoft. DomainRegistration/Domains/Read | Etki alanlarının listesini al |
> | Action | Microsoft. DomainRegistration/Domains/Read | Etki alanını al |
> | Action | Microsoft. DomainRegistration/etki alanları/Yenile/eylem | Var olan bir etki alanını yenileyin. |
> | Action | Microsoft. DomainRegistration/Domains/Write | Yeni bir etki alanı ekleme veya var olanı güncelleştirme |
> | Action | Microsoft. DomainRegistration/generateSsoRequest/ACTION | Etki alanı Denetim Merkezi 'nde oturum açmak için bir istek oluşturun. |
> | Action | Microsoft. DomainRegistration/Listdomainönerilere/eylem | Anahtar sözcüklere göre liste etki alanı önerilerini al |
> | Action | Microsoft. DomainRegistration/işlemler/okuma | App Service etki alanı kaydından tüm işlemleri Listele |
> | Action | Microsoft. DomainRegistration/Register/Action | Abonelik için Microsoft etki alanları kaynak sağlayıcısını kaydetme |
> | Action | Microsoft. DomainRegistration/topLevelDomains/Listagreleştir/eylem | Sözleşme eylemini listeleme |
> | Action | Microsoft. DomainRegistration/topLevelDomains/Read | TopLevel etki alanlarını al |
> | Action | Microsoft. DomainRegistration/topLevelDomains/Read | TopLevel etki alanını al |
> | Action | Microsoft. DomainRegistration/Validatedomainregistrationınformation/Action | Etki alanı satın alma nesnesini göndermeden doğrula |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. EventGrid/Domains/Delete | Etki alanını silme |
> | Action | Microsoft. EventGrid/Domains/listKeys/Action | Bir etki alanı için liste anahtarları |
> | Action | Microsoft. EventGrid/Domains/Providers/Microsoft. Insights/metricDefinitions/Read | Etki alanları için kullanılabilir ölçümleri alır |
> | Action | Microsoft. EventGrid/Domains/Read | Etki alanını okuma |
> | Action | Microsoft. EventGrid/Domains/regenerateKey/ACTION | Bir etki alanı için anahtarı yeniden oluştur |
> | Action | Microsoft. EventGrid/Domains/konular/Sil | Etki alanı konusunu silme |
> | Action | Microsoft. EventGrid/Domains/konular/okuma | Bir etki alanı konusunu okuyun |
> | Action | Microsoft. EventGrid/Domains/konular/yazma | Etki alanı konusu oluşturma veya güncelleştirme |
> | Action | Microsoft. EventGrid/Domains/Write | Etki alanı oluşturma veya güncelleştirme |
> | Action | Microsoft. EventGrid/Eventabonelikleri/silme | Bir eventSubscription silme |
> | Action | Microsoft.EventGrid/eventSubscriptions/getFullUrl/action | Olay aboneliği için tam URL al |
> | Action | Microsoft. EventGrid/Eventabonelikler/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Read | Olay abonelikleri için tanılama ayarını alır |
> | Action | Microsoft. EventGrid/Eventabonelikler/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Write | Olay abonelikleri için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. EventGrid/Eventabonelikleri/sağlayıcılar/Microsoft. Insights/metricDefinitions/okuma | Eventabonelikler için kullanılabilir ölçümleri alır |
> | Action | Microsoft. EventGrid/Eventaboneliklerde/okunan | Bir eventSubscription okuma |
> | Action | Microsoft. EventGrid/Eventaboneliklerin/Write | EventSubscription oluşturma veya güncelleştirme |
> | Action | Microsoft. EventGrid/Extensionkonular/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Read | Konular için tanılama ayarını alır |
> | Action | Microsoft. EventGrid/Extensionkonular/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Write | Konular için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. EventGrid/Extensionkonular/sağlayıcılar/Microsoft. Insights/metricDefinitions/okuma | Konular için kullanılabilir ölçümleri alır |
> | Action | Microsoft. EventGrid/Extensionkonuları/okuma | Bir extensionTopic okuyun. |
> | Action | Microsoft. EventGrid/Locations/Eventaboneliklerde/okunan | Bölgesel olay aboneliklerini listeleme |
> | Action | Microsoft. EventGrid/Locations/operationResults/Read | Bölgesel bir işlemin sonucunu oku |
> | Action | Microsoft. EventGrid/Locations/operationsStatus/Read | Bölgesel bir işlemin durumunu okuyun |
> | Action | Microsoft. EventGrid/Locations/topictypes/Eventabonelikleri/okuma | TopicType 'a göre bölgesel olay aboneliklerini listeleyin |
> | Action | Microsoft. EventGrid/operationResults/Read | İşlemin sonucunu oku |
> | Action | Microsoft. EventGrid/işlemler/okuma | EventGrid işlemlerini listeleyin. |
> | Action | Microsoft. EventGrid/operationsStatus/Read | İşlemin durumunu okuyun |
> | Action | Microsoft. EventGrid/Register/Action | EventGrid kaynak sağlayıcısı için aboneliği kaydeder. |
> | Action | Microsoft. EventGrid/konular/Sil | Konuyu silme |
> | Action | Microsoft.EventGrid/topics/listKeys/action | Konu başlığı anahtarlarını listeleyin |
> | Action | Microsoft. EventGrid/konular/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Read | Konular için tanılama ayarını alır |
> | Action | Microsoft. EventGrid/konular/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Write | Konular için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. EventGrid/konular/sağlayıcılar/Microsoft. Insights/metricDefinitions/okuma | Konular için kullanılabilir ölçümleri alır |
> | Action | Microsoft. EventGrid/konular/okuma | Konu başlığını okuyun |
> | Action | Microsoft.EventGrid/topics/regenerateKey/action | Konu için anahtarı yeniden oluştur |
> | Action | Microsoft. EventGrid/konular/yaz | Konu oluşturma veya güncelleştirme |
> | Action | Microsoft. EventGrid/topictypes/Eventaboneliklerde/okunan | Küresel olay aboneliklerini konu türüne göre Listele |
> | Action | Microsoft. EventGrid/topictypes/eventTypes/Read | Bir topıtype tarafından desteklenen eventTypes 'ı okuyun |
> | Action | Microsoft. EventGrid/topictypes/Read | Bir topıtype oku |
> | Action | Microsoft. EventGrid/Unregister/eylem | EventGrid kaynak sağlayıcısı için aboneliğin kaydını siler. |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. EventHub/Availableclusterregion/okuma | Azure bölgesi tarafından sağlanan önceden sağlanmış kümeleri listelemek için okuma işlemi. |
> | Action | Microsoft. EventHub/Checknameavaılabılıty/Action | Belirtilen abonelikte ad alanının kullanılabilirliğini denetler. |
> | Action | Microsoft. EventHub/checkNamespaceAvailability/Action | Belirtilen abonelikte ad alanının kullanılabilirliğini denetler. Bu API kullanım dışı, lütfen bunun yerine Checknameavaılabılıty kullanın. |
> | Action | Microsoft. EventHub/kümeler/Sil | Var olan bir küme kaynağını siler. |
> | Action | Microsoft. EventHub/kümeler/ad alanları/okuma | Bir küme içindeki ad alanları için ad alanı ARM kimliklerini listeleyin. |
> | Action | Microsoft. EventHub/kümeler/operationresults/Read | Zaman uyumsuz küme işleminin durumunu alın. |
> | Action | Microsoft. EventHub/kümeler/sağlayıcılar/Microsoft. Insights/metricDefinitions/okuma | Küme ölçümleri kaynak açıklamaları listesini al |
> | Action | Microsoft. EventHub/kümeler/okuma | Küme Kaynağı Açıklamasını alır |
> | Action | Microsoft. EventHub/kümeler/yazma | Var olan bir küme kaynağını oluşturur veya değiştirir. |
> | Action | Microsoft. EventHub/konumlar/Deletevirtualnetworkoralt ağları/eylem | Belirtilen VNet için EventHub kaynak sağlayıcısında VNet kurallarını siler |
> | Action | Microsoft. EventHub/ad alanları/authorizationRules/eylem | Güncelleştirme ad alanı yetkilendirme kuralı. Bu API kullanım dışıdır. Lütfen bunun yerine ad alanı yetkilendirme kuralını güncelleştirmek için bir PUT çağrısı kullanın. Bu işlem API sürüm 2017-04-01 ' de desteklenmiyor. |
> | Action | Microsoft. EventHub/ad alanları/authorizationRules/Sil | Ad alanı yetkilendirme kuralını silin. Varsayılan ad alanı yetkilendirme kuralı silinemez.  |
> | Action | Microsoft. EventHub/namespaces/authorizationRules/ListKeys/Action | Ad alanına bağlantı dizesi al |
> | Action | Microsoft. EventHub/ad alanları/authorizationRules/okuma | Ad alanı yetkilendirme kuralları açıklaması listesini alın. |
> | Action | Microsoft. EventHub/namespaces/authorizationRules/regenerateKeys/ACTION | Kaynağa birincil veya Ikincil anahtarı yeniden oluştur |
> | Action | Microsoft. EventHub/ad alanları/authorizationRules/Write | Ad alanı düzeyinde yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve Ikincil anahtarlar güncelleştirilebilen olabilir. |
> | Action | Microsoft. EventHub/ad alanları/silme | Ad Alanı Kaynağını silin |
> | Action | Microsoft. EventHub/namespaces/Disyıldız Recoveryconfigs/authorizationRules/ListKeys/Action | Olağanüstü durum kurtarma birincil ad alanı için yetkilendirme kuralları anahtarlarını alır |
> | Action | Microsoft. EventHub/namespaces/Disyıldız Recoveryconfigs/authorizationRules/Read | Olağanüstü durum kurtarma birincil ad alanının yetkilendirme kurallarını al |
> | Action | Microsoft. EventHub/namespaces/Disyıldız Recoveryconfigs/Breakeşleştirmeyi/eylemi | Olağanüstü durum kurtarmayı devre dışı bırakır ve birincil olan değişiklikleri ikincil ad alanlarına çoğaltmayı durduruyor. |
> | Action | Microsoft. EventHub/namespaces/disyıldız recoveryconfigs/Checknameavaılabılıty/Action | Belirtilen abonelik altında ad alanı diğer adının kullanılabilirliğini denetler. |
> | Action | Microsoft. EventHub/namespaces/Disyıldız Recoveryconfigs/Delete | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını siler. Bu işlem yalnızca birincil ad alanı aracılığıyla çağrılabilir. |
> | Action | Microsoft. EventHub/namespaces/Disyıldız Recoveryconfigs/yük devretme/eylem | COĞRAFI bir DR yük devretmesini çağırır ve ad alanı diğer adını ikincil ad alanına işaret edecek şekilde yeniden yapılandırır. |
> | Action | Microsoft. EventHub/namespaces/Disyıldız Recoveryconfigs/Read | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını alır. |
> | Action | Microsoft. EventHub/namespaces/Disyıldız Recoveryconfigs/Write | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını oluşturur veya güncelleştirir. |
> | Action | Microsoft. EventHub/namespaces/eventhubs/authorizationRules/Action | EventHub güncelleştirme işlemi. Bu işlem API sürüm 2017-04-01 ' de desteklenmiyor. Yetkilendirme kuralları. Lütfen yetkilendirme kuralını güncelleştirmek için bir PUT çağrısı kullanın. |
> | Action | Microsoft. EventHub/namespaces/eventhubs/authorizationRules/Delete | EventHub yetkilendirme kurallarını silme işlemi |
> | Action | Microsoft. EventHub/namespaces/eventhubs/authorizationRules/ListKeys/Action | EventHub bağlantı dizesini al |
> | Action | Microsoft. EventHub/namespaces/eventhubs/authorizationRules/Read |  EventHub yetkilendirme kurallarının listesini alın |
> | Action | Microsoft. EventHub/namespaces/eventhubs/authorizationRules/regenerateKeys/ACTION | Kaynağa birincil veya Ikincil anahtarı yeniden oluştur |
> | Action | Microsoft. EventHub/namespaces/eventhubs/authorizationRules/Write | EventHub yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilen olabilir. |
> | Action | Microsoft. EventHub/namespaces/eventHubs/consumergroups/Delete | ConsumerGroup kaynağını silme işlemi |
> | Action | Microsoft. EventHub/namespaces/eventHubs/consumergroups/Read | ConsumerGroup kaynak açıklamalarının listesini al |
> | Action | Microsoft. EventHub/namespaces/eventHubs/consumergroups/Write | ConsumerGroup özellikleri oluşturun veya güncelleştirin. |
> | Action | Microsoft. EventHub/namespaces/eventhubs/Delete | EventHub kaynağını silme işlemi |
> | Action | Microsoft. EventHub/namespaces/eventhubs/Read | EventHub kaynak açıklamalarının listesini al |
> | Action | Microsoft. EventHub/namespaces/eventhubs/Write | EventHub özelliklerini oluşturun veya güncelleştirin. |
> | Action | Microsoft. EventHub/namespaces/ıpfilterrules/Delete | IP Filtresi Kaynağını Sil |
> | Action | Microsoft. EventHub/namespaces/ıpfilterrules/Read | IP Filtresi Kaynağını Al |
> | Action | Microsoft. EventHub/namespaces/ıpfilterrules/Write | IP Filtresi Kaynağı Oluştur |
> | DataAction | Microsoft. EventHub/ad alanları/iletiler/alma/eylem | İleti alma |
> | DataAction | Microsoft. EventHub/ad alanları/iletiler/gönderme/eylem | İleti gönderme |
> | Action | Microsoft. EventHub/namespaces/messagingPlan/okuma | Bir ad alanı için mesajlaşma planını alır.<br>Bu API kullanım dışıdır.<br>MessagingPlan kaynağı aracılığıyla sunulan özellikler sonraki API sürümlerindeki (üst) ad alanı kaynağına taşınır.<br>Bu işlem API sürüm 2017-04-01 ' de desteklenmiyor. |
> | Action | Microsoft. EventHub/namespaces/messagingPlan/yazma | Bir ad alanı için mesajlaşma planını güncelleştirir.<br>Bu API kullanım dışıdır.<br>MessagingPlan kaynağı aracılığıyla sunulan özellikler sonraki API sürümlerindeki (üst) ad alanı kaynağına taşınır.<br>Bu işlem API sürüm 2017-04-01 ' de desteklenmiyor. |
> | Action | Microsoft. EventHub/namespaces/networkruleset/Delete | Sanal Ağ Kuralı Kaynağını Sil |
> | Action | Microsoft. EventHub/namespaces/networkruleset/Read | NetworkRuleSet kaynağını alır |
> | Action | Microsoft. EventHub/namespaces/networkruleset/Write | Sanal Ağ Kuralı Kaynağı Oluştur |
> | Action | Microsoft. EventHub/namespaces/networkrulesets/Delete | Sanal Ağ Kuralı Kaynağını Sil |
> | Action | Microsoft. EventHub/namespaces/networkrulesets/Read | NetworkRuleSet kaynağını alır |
> | Action | Microsoft. EventHub/namespaces/networkrulesets/Write | Sanal Ağ Kuralı Kaynağı Oluştur |
> | Action | Microsoft. EventHub/namespaces/operationresults/Read | Ad alanı işleminin durumunu al |
> | Action | Microsoft. EventHub/namespaces/Providers/Microsoft. Insights/diagnosticSettings/Read | Ad alanı Tanılama ayarları kaynak açıklamalarının listesini al |
> | Action | Microsoft. EventHub/namespaces/Providers/Microsoft. Insights/diagnosticSettings/Write | Ad alanı Tanılama ayarları kaynak açıklamalarının listesini al |
> | Action | Microsoft. EventHub/namespaces/Providers/Microsoft. Insights/logDefinitions/Read | Ad alanı günlükleri kaynak açıklamalarının listesini al |
> | Action | Microsoft. EventHub/namespaces/Providers/Microsoft. Insights/metricDefinitions/Read | Ad alanı ölçümleri kaynak açıklamalarının listesini al |
> | Action | Microsoft. EventHub/ad alanları/okuma | Ad alanı kaynağı açıklaması listesini al |
> | Action | Microsoft. EventHub/namespaces/removeAcsNamepsace/eylem | ACS ad alanını kaldırın |
> | Action | Microsoft. EventHub/namespaces/virtualNetworkRules/Delete | Sanal Ağ Kuralı Kaynağını Sil |
> | Action | Microsoft. EventHub/namespaces/virtualNetworkRules/Read | Sanal Ağ Kuralı Kaynağını alır |
> | Action | Microsoft. EventHub/namespaces/virtualNetworkRules/Write | Sanal Ağ Kuralı Kaynağı Oluştur |
> | Action | Microsoft. EventHub/ad alanları/yazma | Bir ad alanı kaynağı oluşturun ve özelliklerini güncelleştirin. Ad alanının etiketleri ve kapasitesi, güncelleştirilebilen özelliklerdir. |
> | Action | Microsoft. EventHub/işlemler/okuma | Alma İşlemleri |
> | Action | Microsoft. EventHub/Register/ACTION | EventHub kaynak sağlayıcısı için aboneliği kaydeder ve EventHub kaynaklarının oluşturulmasını sunar |
> | Action | Microsoft. EventHub/SKU/okuma | SKU kaynak açıklamaları listesini al |
> | Action | Microsoft. EventHub/SKU/bölge/okuma | Skuregion kaynak açıklamalarının listesini al |
> | Action | Microsoft. EventHub/kaydını kaldırma/eylem | EventHub Kaynak Sağlayıcısını kaydet |

## <a name="microsoftfeatures"></a>Microsoft. Features

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Features/Features/Read | Bir aboneliğin özelliklerini alır. |
> | Action | Microsoft. Features/işlemler/okuma | İşlem listesini alır. |
> | Action | Microsoft. Özellikler/sağlayıcılar/Özellikler/okuma | Belirli bir kaynak sağlayıcısındaki bir aboneliğin özelliğini alır. |
> | Action | Microsoft. Özellikler/sağlayıcılar/Özellikler/kaydet/eylem | Bir abonelik için özelliği belirli bir kaynak sağlayıcısına kaydeder. |
> | Action | Microsoft. Özellikler/sağlayıcılar/Özellikler/kayıt kaldırma/eylem | Belirli bir kaynak sağlayıcısındaki bir aboneliğin özelliğinin kaydını siler. |
> | Action | Microsoft. Features/Register/Action | Bir aboneliğin özelliğini kaydeder. |

## <a name="microsoftguestconfiguration"></a>Microsoft. GuestConfiguration

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. GuestConfiguration/Guestconfigurationatamaları/silme | Konuk yapılandırma atamasını silin. |
> | Action | Microsoft. GuestConfiguration/Guestconfigurationatamaları/okuma | Konuk yapılandırma atamasını al. |
> | Action | Microsoft. GuestConfiguration/Guestconfigurationatamaları/raporlar/okuma | Konuk yapılandırma atama raporunu al. |
> | Action | Microsoft. GuestConfiguration/Guestconfigurationatamaları/yazma | Yeni Konuk yapılandırma ataması oluşturun. |
> | Action | Microsoft. GuestConfiguration/Register/Action | Microsoft. GuestConfiguration kaynak sağlayıcısı için aboneliği kaydeder. |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. HDInsight/kümeler/uygulamalar/Sil | HDInsight kümesi için uygulamayı silme |
> | Action | Microsoft. HDInsight/kümeler/uygulamalar/okuma | HDInsight kümesi için uygulama al |
> | Action | Microsoft. HDInsight/kümeler/uygulamalar/yazma | HDInsight kümesi için uygulama oluşturma veya güncelleştirme |
> | Action | Microsoft. HDInsight/kümeler/changerdpsetting/Action | HDInsight kümesi için RDP ayarını değiştir |
> | Action | Microsoft. HDInsight/kümeler/konfigürasyonlar/eylem | HDInsight küme yapılandırmasını güncelleştirme |
> | Action | Microsoft. HDInsight/kümeler/konfigürasyonlar/eylem | HDInsight küme yapılandırmasını al |
> | Action | Microsoft. HDInsight/kümeler/yapılandırma/okuma | HDInsight küme yapılandırmasını al |
> | Action | Microsoft. HDInsight/kümeler/Sil | HDInsight kümesini silme |
> | Action | Microsoft. HDInsight/kümeler/uzantılar/Sil | HDInsight kümesi için küme uzantısını Sil |
> | Action | Microsoft. HDInsight/kümeler/uzantılar/okuma | HDInsight kümesi için küme uzantısı Al |
> | Action | Microsoft. HDInsight/kümeler/uzantılar/yazma | HDInsight kümesi için küme uzantısı oluştur |
> | Action | Microsoft. HDInsight/kümeler/getGatewaySettings/Action | HDInsight kümesi için ağ geçidi ayarlarını al |
> | Action | Microsoft. HDInsight/kümeler/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Read | Kaynak HDInsight kümesi için tanılama ayarını alır |
> | Action | Microsoft. HDInsight/kümeler/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Write | Kaynak HDInsight kümesi için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. HDInsight/kümeler/sağlayıcılar/Microsoft. Insights/metricDefinitions/okuma | HDInsight kümesi için kullanılabilir ölçümleri alır |
> | Action | Microsoft. HDInsight/kümeler/okuma | HDInsight kümesiyle ilgili ayrıntıları al |
> | Action | Microsoft. HDInsight/kümeler/roller/yeniden boyutlandır/eylem | HDInsight kümesini yeniden boyutlandırma |
> | Action | Microsoft. HDInsight/kümeler/updateGatewaySettings/Action | HDInsight kümesi için ağ geçidi ayarlarını güncelleştirme |
> | Action | Microsoft. HDInsight/kümeler/yazma | HDInsight kümesi oluşturma veya güncelleştirme |
> | Action | Microsoft. HDInsight/konumlar/yetenekler/okuma | Abonelik yeteneklerini al |
> | Action | Microsoft. HDInsight/Locations/Checknameavaılabılıty/Read | Ad Kullanılabilirliğini Denetle |
> | Action | Microsoft. HDInsight/Register/ACTION | Abonelik için HDInsight kaynak sağlayıcısını Kaydet |
> | Action | Microsoft. HDInsight/Unregister/eylem | Abonelik için HDInsight kaynak sağlayıcısı kaydını sil |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. ımportexport/işler/Sil | Var olan bir işi siler. |
> | Action | Microsoft. ımportexport/Jobs/listBitLockerKeys/Action | Belirtilen iş için BitLocker anahtarlarını alır. |
> | Action | Microsoft. ımportexport/Jobs/Read | Belirtilen işin özelliklerini alır veya işlerin listesini döndürür. |
> | Action | Microsoft. ımportexport/işler/yazma | Belirtilen parametrelere sahip bir iş oluşturur veya belirtilen iş için özellikleri veya etiketleri güncelleştirir. |
> | Action | Microsoft. ımportexport/konumlar/okuma | Belirtilen konum için özellikleri alır veya konumların listesini döndürür. |
> | Action | Microsoft. ımportexport/Register/Action | İçeri/dışarı aktarma kaynak sağlayıcısı için aboneliği kaydeder ve içeri/dışarı aktarma işlerinin oluşturulmasına izin verir. |

## <a name="microsoftinsights"></a>Microsoft.Insights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Insights/ActionGroups/Delete | Bir eylem grubunu silin |
> | Action | Microsoft. Insights/ActionGroups/Read | Bir eylem grubunu okuyun |
> | Action | Microsoft. Insights/ActionGroups/Write | Bir eylem grubu oluşturun veya güncelleştirin |
> | Action | Microsoft. Insights/ActivityLogAlerts/etkin/eylem | Etkinlik Günlüğü Uyarısı etkinleştirildi |
> | Action | Microsoft. Insights/ActivityLogAlerts/Delete | Bir etkinlik günlüğü uyarısını silin |
> | Action | Microsoft. Insights/ActivityLogAlerts/Read | Bir etkinlik günlüğü uyarısını okuyun |
> | Action | Microsoft. Insights/ActivityLogAlerts/Write | Etkinlik günlüğü uyarısı oluşturma veya güncelleştirme |
> | Action | Microsoft. Insights/AlertRules/Activated/eylem | Klasik ölçüm uyarısı etkinleştirildi |
> | Action | Microsoft. Insights/AlertRules/Delete | Bir klasik ölçüm uyarısını silin |
> | Action | Microsoft. Insights/AlertRules/olaylar/okuma | Klasik ölçüm uyarısı olayını okuyun |
> | Action | Microsoft. Insights/AlertRules/Read | Bir klasik ölçüm uyarısını okuyun |
> | Action | Microsoft. Insights/AlertRules/çözüldü/Action | Klasik ölçüm uyarısı çözümlendi |
> | Action | Microsoft. Insights/AlertRules/kısıt/eylem | Klasik ölçüm uyarısı kuralı kısıtlandı |
> | Action | Microsoft. Insights/AlertRules/Write | Klasik ölçüm uyarısı oluşturma veya güncelleştirme |
> | Action | Microsoft. Insights/oto Scalesettings/Delete | Bir otomatik ölçeklendirme ayarını silin |
> | Action | Microsoft. Insights/oto Scalesettings/Providers/Microsoft. Insights/diagnosticSettings/Read | Bir kaynak tanılama ayarını okuyun |
> | Action | Microsoft. Insights/oto Scalesettings/Providers/Microsoft. Insights/diagnosticSettings/Write | Kaynak tanılama ayarı oluştur veya güncelleştir |
> | Action | Microsoft. Insights/oto Scalesettings/Providers/Microsoft. Insights/logDefinitions/Read | Günlük tanımlarını oku |
> | Action | Microsoft. Insights/oto Scalesettings/Providers/Microsoft. Insights/MetricDefinitions/Read | Ölçüm tanımlarını oku |
> | Action | Microsoft. Insights/oto Scalesettings/Read | Bir otomatik ölçeklendirme ayarını okuyun |
> | Action | Microsoft. Insights/oto Scalesettings/Scaleaşağı/Action | Otomatik ölçeklendirme ölçeği azaltma işlemi başlatıldı |
> | Action | Microsoft. Insights/oto Scalesettings/ScaledownResult/Action | Otomatik ölçeklendirme ölçeği azaltma işlemi tamamlandı |
> | Action | Microsoft. Insights/oto Scalesettings/Scaleup/Action | Otomatik ölçeklendirme ölçeği artırma işlemi başlatıldı |
> | Action | Microsoft. Insights/oto Scalesettings/ScaleupResult/Action | Otomatik ölçeklendirme ölçeği artırma işlemi tamamlandı |
> | Action | Microsoft. Insights/oto Scalesettings/Write | Otomatik ölçeklendirme ayarı oluşturma veya güncelleştirme |
> | Action | Microsoft. Öngörüler/taban çizgisi/okuma | Ölçüm temelini okuma (Önizleme) |
> | Action | Microsoft. Insights/CalculateBaseline/Read | Ölçüm değerleri için taban çizgisini hesapla (Önizleme) |
> | Action | Microsoft. Insights/bileşenler/analiz Ticsitems/Delete | Application Insights Analytics öğesini silme |
> | Action | Microsoft. Insights/bileşenler/analiz Ticsitems/okuma | Application Insights Analytics öğesi okunuyor |
> | Action | Microsoft. Insights/bileşenler/analiz Ticsitems/yazma | Application Insights Analytics öğesi yazma |
> | Action | Microsoft. Insights/bileşenler/analiz Ticstables/Action | Application Insights analitik tablosu eylemi |
> | Action | Microsoft. Insights/bileşenler/analiz Ticstables/Delete | Application Insights Analytics tablo şemasını silme |
> | Action | Microsoft. Insights/bileşenler/analiz Ticstables/Read | Application Insights Analytics tablo şemasını okuma |
> | Action | Microsoft. Insights/bileşenler/analiz Ticstables/Write | Application Insights Analytics tablo şeması yazma |
> | Action | Microsoft. Insights/bileşenler/ek açıklamalar/Sil | Bir Application Insights ek açıklamasını silme |
> | Action | Microsoft. Insights/bileşenler/ek açıklamalar/okuma | Bir Application Insights ek açıklamasını okuma |
> | Action | Microsoft. Insights/bileşenler/ek açıklamalar/yazma | Bir Application Insights ek açıklamasını yazma |
> | Action | Microsoft. Insights/bileşenler/API/okuma | Application Insights bileşen verileri API 'SI okunuyor |
> | Action | Microsoft. Insights/bileşenler/ApiKeys/ACTION | Application Insights API anahtarı oluşturma |
> | Action | Microsoft. Insights/bileşenler/ApiKeys/Delete | Application Insights API anahtarını silme |
> | Action | Microsoft. Insights/bileşenler/ApiKeys/Read | Application Insights API anahtarı okunuyor |
> | Action | Microsoft. Insights/bileşenler/BillingPlanForComponent/Read | Application Insights bileşeni için faturalandırma planı okunuyor |
> | Action | Microsoft. Insights/bileşenler/CurrentBillingFeatures/Read | Application Insights bileşeni için geçerli faturalandırma özellikleri okunuyor |
> | Action | Microsoft. Insights/bileşenler/CurrentBillingFeatures/Write | Application Insights bileşeni için geçerli faturalandırma özellikleri yazılıyor |
> | Action | Microsoft. Insights/Components/Defaultworkıtemconfig/Read | Application Insights varsayılan bir ALM tümleştirme yapılandırması okunuyor |
> | Action | Microsoft. Insights/bileşenler/Sil | Bir Application Insights bileşen yapılandırmasını silme |
> | Action | Microsoft. Insights/bileşenler/olaylar/okuma | OData sorgu biçimini kullanarak Application Insights günlükleri al |
> | Action | Microsoft. Insights/bileşenler/ExportConfiguration/eylem | Application Insights dışa aktarım ayarları eylemi |
> | Action | Microsoft. Insights/bileşenler/ExportConfiguration/Delete | Application Insights dışa aktarma ayarlarını silme |
> | Action | Microsoft. Insights/bileşenler/ExportConfiguration/Read | Application Insights dışa aktarma ayarlarını okuma |
> | Action | Microsoft. Insights/bileşenler/ExportConfiguration/Write | Application Insights dışa aktarma ayarlarını yazma |
> | Action | Microsoft. Insights/bileşenler/ExtendQueries/Read | Application Insights bileşeni genişletilmiş sorgu sonuçları okunuyor |
> | Action | Microsoft. Insights/bileşenler/sık kullanılanlar/Sil | Application Insights sık kullanılanını silme |
> | Action | Microsoft. Insights/bileşenler/sık kullanılanlar/okuma | Application Insights sık kullanılanını okuma |
> | Action | Microsoft. Insights/bileşenler/sık kullanılanlar/yaz | Application Insights sık kullanılanını yazma |
> | Action | Microsoft. Insights/bileşenler/FeatureCapabilities/okuma | Application Insights bileşen özelliği özelliklerini okuma |
> | Action | Microsoft. Insights/bileşenler/GetAvailableBillingFeatures/Read | Application Insights bileşeni kullanılabilir faturalama özelliklerini okuma |
> | Action | Microsoft. Insights/bileşenler/GetToken/Read | Application Insights bileşen belirtecini okuma |
> | Action | Microsoft. Insights/bileşenler/MetricDefinitions/okuma | Application Insights bileşeni ölçüm tanımlarını okuma |
> | Action | Microsoft. Insights/bileşenler/ölçümler/okuma | Application Insights bileşen ölçümlerini okuma |
> | Action | Microsoft. Öngörüler/bileşenler/taşıma/eylem | Application Insights bileşenini başka bir kaynak grubuna veya aboneliğe taşıma |
> | Action | Microsoft. Insights/bileşenler/MyAnalyticsItems/Delete | Application Insights kişisel analiz öğesini silme |
> | Action | Microsoft. Insights/bileşenler/MyAnalyticsItems/Read | Application Insights kişisel analitik öğesini okuma |
> | Action | Microsoft. Insights/bileşenler/MyAnalyticsItems/Write | Application Insights kişisel analiz öğesi yazma |
> | Action | Microsoft. Insights/bileşenler/MyFavorites/Read | Application Insights kişisel sık kullanılanı okuma |
> | Action | Microsoft. Insights/bileşenler/Işlemler/okuma | Application Insights uzun süren işlemlerin durumunu al |
> | Action | Microsoft. Insights/bileşenler/Pricingplanlar/okuma | Application Insights bileşen fiyatlandırma planı okunuyor |
> | Action | Microsoft. Insights/bileşenler/Pricingplanlar/yazma | Application Insights bileşen fiyatlandırma planı yazma |
> | Action | Microsoft. Insights/bileşenler/ProactiveDetectionConfigs/Read | Proaktif algılama yapılandırmasını okuma Application Insights |
> | Action | Microsoft. Insights/bileşenler/ProactiveDetectionConfigs/Write | Proaktif algılama yapılandırması yazma Application Insights |
> | Action | Microsoft. Insights/bileşenler/sağlayıcılar/Microsoft. Insights/MetricDefinitions/okuma | Ölçüm tanımlarını oku |
> | Action | Microsoft. Insights/bileşenler/Temizleme/eylem | Application Insights verilerini temizleme |
> | Action | Microsoft. Insights/bileşenler/sorgu/okuma | Sorguları Application Insights günlüklere göre Çalıştır |
> | Action | Microsoft. Insights/bileşenler/QuotaStatus/Read | Application Insights bileşen kota durumu okunuyor |
> | Action | Microsoft. Insights/bileşenler/okuma | Bir Application Insights bileşen yapılandırmasını okuma |
> | Action | Microsoft. Insights/bileşenler/SyntheticMonitorLocations/Read | Application Insights web testi konumlarını okuma |
> | Action | Microsoft. Insights/bileşenler/Web testleri/okuma | Bir web testi yapılandırmasını okuma |
> | Action | Microsoft. Insights/bileşenler/Workıtemconfigs/Delete | Application Insights ALM tümleştirme yapılandırmasını silme |
> | Action | Microsoft. Insights/bileşenler/Workıtemconfigs/Read | Application Insights ALM tümleştirme yapılandırmasını okuma |
> | Action | Microsoft. Insights/bileşenler/Workıtemconfigs/Write | Application Insights ALM tümleştirme yapılandırması yazma |
> | Action | Microsoft. Insights/bileşenler/yazma | Bir Application Insights bileşen yapılandırmasına yazma |
> | Action | Microsoft. Insights/DiagnosticSettings/Delete | Bir kaynak tanılama ayarını silin |
> | Action | Microsoft. Insights/DiagnosticSettings/Read | Bir kaynak tanılama ayarını okuyun |
> | Action | Microsoft. Insights/DiagnosticSettings/Write | Kaynak tanılama ayarı oluştur veya güncelleştir |
> | Action | Microsoft. Insights/EventCategories/okuma | Kullanılabilir etkinlik günlüğü olay kategorilerini oku |
> | Action | Microsoft. Insights/eventTypes/digestevents/Read | Yönetim olayı türü özetini oku |
> | Action | Microsoft. Insights/eventTypes/Values/Read | Etkinlik Günlüğü etkinliklerini okuyun |
> | Action | Microsoft. Insights/ExtendedDiagnosticSettings/Delete | Ağ akışı günlük tanılama ayarını silme |
> | Action | Microsoft. Insights/ExtendedDiagnosticSettings/Read | Ağ akışı günlük tanılama ayarını oku |
> | Action | Microsoft. Insights/ExtendedDiagnosticSettings/Write | Ağ akışı günlük tanılama ayarını oluşturma veya güncelleştirme |
> | Action | Microsoft. Insights/ListMigrationDate/Action | Abonelik geçiş tarihini geri alın |
> | Action | Microsoft. Insights/ListMigrationDate/Read | Abonelik geçiş tarihini geri alın |
> | Action | Microsoft. Insights/LogDefinitions/okuma | Günlük tanımlarını oku |
> | Action | Microsoft. Insights/günlüğe profiller/Sil | Etkinlik günlüğü günlük profilini silme |
> | Action | Microsoft. Insights/günlüğe profiller/okuma | Etkinlik günlüğü günlük profilini okuyun |
> | Action | Microsoft. Insights/günlüğe profiller/yazma | Etkinlik günlüğü günlük profili oluşturma veya güncelleştirme |
> | Action | Microsoft. Insights/logs/Adassessmentönerisi/okuma | Adassessmentöneriyi tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/ADReplicationResult/Read | ADReplicationResult tablosundan verileri oku |
> | Action | Microsoft. Insights/logs/Adsecurityassessmentönerisi/okuma | Adsecurityassessmentönerisi tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/uyarı/okuma | Uyarı tablosundan verileri oku |
> | Action | Microsoft. Öngörüler/Günlükler/AlertHistory/Read | AlertHistory tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/ApplicationInsights/Read | ApplicationInsights tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/AzureActivity/Read | AzureActivity tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/AzureMetrics/Read | AzureMetrics tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/BoundPort/Read | BoundPort tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/CommonSecurityLog/Read | CommonSecurityLog tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/ComputerGroup/Read | ComputerGroup tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/ConfigurationChange/Read | ConfigurationChange tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/ConfigurationData/Read | ConfigurationData tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Containerımageınventory/Read | Containerımageınventory tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Containerınventory/Read | Containerınventory tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/ContainerLog/Read | ContainerLog tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/ContainerServiceLog/Read | ContainerServiceLog tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Deviceappkilitleniyor/Read | DeviceAppCrash tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Deviceapplayeniden Başlat/oku | Deviceapplayeniden Başlat tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/DeviceCalendar/okuma | DeviceCalendar tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/Devicectaanup/okuma | Devicectaanup tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/DeviceConnectSession/Read | DeviceConnectSession tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/DeviceEtw/okuma | DeviceEtw tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/DeviceHardwareHealth/Read | DeviceHardwareHealth tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/DeviceHealth/okuma | DeviceHealth tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Devicesinyal/okuma | Devicesinyal tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/DeviceSkypeHeartbeat/Read | DeviceSkypeHeartbeat tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/DeviceSkypeSignIn/Read | DeviceSkypeSignIn tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Devicestaepstate/Read | Devicestaepstate tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/Dnne hatası/okuma | Dılure tablosundan verileri oku |
> | Action | Microsoft. Insights/logs/Dakppresorumluluğun/okunan | Dappreyükümlülük tablosundan verileri oku |
> | Action | Microsoft. Insights/logs/Dhdrivergüvenilirlik/okuma | Dhdrivergüvenirlik tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Dhlogonbaşarısızlıkları/okuma | Dhlogonarızaları tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Dhlogonölçümler/okuma | Dhlogonölçümlerini tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Dhoatık Kıdata/Read | Dhoatık verileri tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Dhosgüvenilirliği/okuma | Dhosgüvenirlik tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/DHWipAppLearning/Read | DHWipAppLearning tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/DnsEvents/okuma | DnsEvents tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Dnsınventory/Read | DnsInventory tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Etwewevent/Read | ETWEvent tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/olay/okuma | Olay tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/ExchangeAssessmentRecommendation/Read | ExchangeAssessmentRecommendation tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/ExchangeOnlineAssessmentRecommendation/Read | ExchangeOnlineAssessmentRecommendation tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/sinyal/okuma | Sinyal tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/IISAssessmentRecommendation/Read | IISAssessmentRecommendation tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/ınboundconnection/Read | Inboundconnection tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Kubenodeınventory/Read | Kubenodeınventory tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Kubepodinvenfer/Read | KubePodInventory tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/LinuxAuditLog/Read | LinuxAuditLog tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/MAApplication/Read | MAApplication tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/MAApplicationHealth/Read | MAApplicationHealth tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/MAApplicationHealthAlternativeVersions/Read | MAApplicationHealthAlternativeVersions tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maapplicationhealthsorunlar/okuma | Maapplicationhealthsorunlar tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maapplicationınstance/Read | Maapplicationınstance tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maapplicationınstancereadsürekliliği/okuma | Maapplicationınstancereadılmi tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/MAApplicationReadiness/Read | MAApplicationReadiness tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/MADeploymentPlan/okuma | MADeploymentPlan tablosundan verileri okuma |
> | Action | Microsoft. Insights/Günlükler/MADevice/okuma | MADevice tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/MADevicePnPHealth/Read | MADevicePnPHealth tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/MADevicePnPHealthAlternativeVersions/Read | MADevicePnPHealthAlternativeVersions tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Madevicepnphealthsorunlar/okuma | Madevicepnphealthsorunlar tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/Madevicereadsürekliliği/okuma | Madevicereadtıo tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Madriverınstancereadsürekliliği/okuma | Madriverınstancereadılmi tablosundan verileri oku |
> | Action | Microsoft. Öngörüler/Günlükler/MADriverReadiness/Read | MADriverReadiness tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maofficeeklentisi/okuma | Maofficeeklenti tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maofficeaddınhealth/Read | Maofficeaddınhealth tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maofficeaddınhealthsorunlar/okuma | Maofficeaddınhealthsorunlar tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maofficeaddınınstance/Read | Maofficeaddınınstance tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maofficeaddınınstancereadsürekliliği/okuma | Maofficeaddınınstancereadılmi tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maofficeaddınreadiness/Read | Maofficeaddınreadiness tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maofficeuygulaması/okuma | Maofficeuygulama tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/MAOfficeAppHealth/Read | MAOfficeAppHealth tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maofficeappınstance/Read | Maofficeappınstance tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/MAOfficeAppReadiness/Read | MAOfficeAppReadiness tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maofficebuilınfo/Read | Maofficebuilınfo tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/MAOfficeCurrencyAssessment/Read | MAOfficeCurrencyAssessment tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/MAOfficeCurrencyAssessmentDailyCounts/Read | MAOfficeCurrencyAssessmentDailyCounts tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/MAOfficeDeploymentStatus/Read | MAOfficeDeploymentStatus tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maofficemakrosağlığı/okuma | Maofficemakrohealth tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maofficemakrohealthsorunlar/okuma | Maofficemacrohealthsorunlar tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maofficemakroıssueınstancereadsürekliliği/okuma | Maofficemacroıssueınstancectablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maofficemakroıssuereadsürekliliği/okuma | Maofficemacroıssuereadılmi tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maofficemakroözeti/okuma | MAOfficeMacroSummary tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/MAOfficeSuite/Read | MAOfficeSuite tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Maofficesuiteınstance/Read | Maofficesuiteınstance tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/MAProposedPilotDevices/Read | MAProposedPilotDevices tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Mawindowsbuilınfo/Read | Mawindowsbuilınfo tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Mawindowscurrencki/Read | Mawindowscurrencyaskment tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Mawindowscurrencyassessmentgünlükcounts/okuma | MAWindowsCurrencyAssessmentDailyCounts tablosundan verileri oku |
> | Action | Microsoft. Insights/logs/MAWindowsDeploymentStatus/Read | MAWindowsDeploymentStatus tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Mawindowssysreqınstancereadsürekliliği/okuma | Mawindowssysreqınstancereadtıı tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/NetworkMonitoring/Read | NetworkMonitoring tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Officeetkinliği/okuma | OfficeActivity tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/Işlem/okuma | Işlem tablosundan verileri oku |
> | Action | Microsoft. Insights/logs/OutboundConnection/Read | OutboundConnection tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/perf/okuma | Perf tablosundan verileri oku |
> | Action | Microsoft. Öngörüler/Günlükler/ProtectionStatus/Read | ProtectionStatus tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/okuma | Tüm günlüklerinizin verilerini okuma |
> | Action | Microsoft. Insights/logs/ReservedAzureCommonFields/Read | ReservedAzureCommonFields tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/ReservedCommonFields/Read | ReservedCommonFields tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Sccmassessmentönerisi/okuma | Sccmassessmentöneriyi tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/Scomassessmentönerisi/okuma | Scomassessmentöneriyi tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/SecurityAlert/Read | SecurityAlert tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/SecurityBaseline/okuma | SecurityBaseline tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/SecurityBaselineSummary/okuma | SecurityBaselineSummary tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/SecurityDetection/okuma | SecurityDetection tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/SecurityEvent/Read | SecurityEvent tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/ServiceFabricOperationalEvent/Read | ServiceFabricOperationalEvent tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/ServiceFabricReliableActorEvent/Read | ServiceFabricReliableActorEvent tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/ServiceFabricReliableServiceEvent/Read | ServiceFabricReliableServiceEvent tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Sfbassessmentönerisi/okuma | Sfbassessmentöneriyi tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/SfBOnlineAssessmentRecommendation/Read | SfBOnlineAssessmentRecommendation tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/SharePointOnlineAssessmentRecommendation/Read | SharePointOnlineAssessmentRecommendation tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Spassessmentönerisi/okuma | Spassessmentöneriyi tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Sqlassessmentönerisi/okuma | Sqlassessmentöneriyi tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/SQLQueryPerformance/Read | SQLQueryPerformance tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Syslog/Read | Syslog tablosundan veri okuma |
> | Action | Microsoft. Insights/logs/SysmonEvent/Read | SysmonEvent tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Tables. Custom/Read | Herhangi bir özel günlükteki verileri okuma |
> | Action | Microsoft. Insights/logs/UAApp/Read | UAApp tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/Uabilgisayar/okuma | UAComputer tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/UAComputerRank/Read | Uıacomputerrank tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/UADriver/okuma | Uıadriver tablosundan verileri oku |
> | Action | Microsoft. Insights/logs/UADriverProblemCodes/Read | Uıadriverproblemcodes tablosundan verileri oku |
> | Action | Microsoft. Insights/logs/UAFeedback/Read | UAFeedback tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/UAHardwareSecurity/Read | Uıahardwaresecurity tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Uaıesitediscovery/Read | Uıaııesitediscovery tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Uaofficeeklentisi/okuma | Uıaofficeaddın tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/UAProposedActionPlan/Read | UAProposedActionPlan tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Uasysreqıssue/Read | Uıasysreqıssue tablosundaki verileri oku |
> | Action | Microsoft. Insights/logs/Uayükseltilebilir Dedcomputer/Read | Uayükseltilebilir Dedcomputer tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/güncelleştirme/okuma | Güncelleştirme tablosundan verileri oku |
> | Action | Microsoft. Öngörüler/Günlükler/UpdateRunProgress/Read | UpdateRunProgress tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/UpdateSummary/Read | UpdateSummary tablosundan verileri oku |
> | Action | Microsoft. Öngörüler/Günlükler/kullanım/okuma | Kullanım tablosundan verileri oku |
> | Action | Microsoft. Insights/logs/W3CIISLog/Read | W3CIISLog tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/WaaSDeploymentStatus/Read | WaaSDeploymentStatus tablosundan verileri oku |
> | Action | Microsoft. Insights/logs/Waasınsiderstatus/Read | Waasınsiderstatus tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/WaaSUpdateStatus/Read | WaaSUpdateStatus tablosundan verileri oku |
> | Action | Microsoft. Insights/logs/WDAVStatus/Read | WDAVStatus tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/WDAVThreat/Read | WDAVThreat Table 'dan verileri okuma |
> | Action | Microsoft. Insights/logs/Windowsclientassessmentönerisi/okuma | Windowsclientassessmentöneriyi tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/WindowsFirewall/Read | WindowsFirewall tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/WindowsServerAssessmentRecommendation/Read | WindowsServerAssessmentRecommendation tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/kablolu veri/okuma | Kablolu veri tablosundan verileri okuma |
> | Action | Microsoft. Insights/logs/Wudoaggreg, Status/Read | Wudoaggreg, Status tablosundan verileri okuma |
> | Action | Microsoft. Öngörüler/Günlükler/WUDOStatus/okuma | WUDOStatus tablosundan verileri okuma |
> | Action | Microsoft. Insights/MetricAlerts/Delete | Bir ölçüm uyarısını silin |
> | Action | Microsoft. Insights/MetricAlerts/okuma | Bir ölçüm uyarısını okuyun |
> | Action | Microsoft. Insights/MetricAlerts/durum/okuma | Ölçüm uyarısı durumunu oku |
> | Action | Microsoft. Insights/MetricAlerts/Write | Ölçüm uyarısı oluşturma veya güncelleştirme |
> | Action | Microsoft. Insights/Metrictemeller/okuma | Ölçüm temellerini oku |
> | Action | Microsoft. Insights/MetricDefinitions/Microsoft. Insights/oku | Ölçüm tanımlarını oku |
> | Action | Microsoft. Insights/MetricDefinitions/sağlayıcılar/Microsoft. Insights/Read | Ölçüm tanımlarını oku |
> | Action | Microsoft. Insights/MetricDefinitions/okuma | Ölçüm tanımlarını oku |
> | Action | Microsoft. Insights/Metricnamespaces/Read | Ölçüm ad alanlarını oku |
> | Action | Microsoft. Insights/ölçümler/eylem | Ölçüm Eylemi |
> | Action | Microsoft. Insights/ölçümler/Microsoft. Öngörüler/okuma | Ölçümleri okuma |
> | Action | Microsoft. Insights/ölçümler/sağlayıcılar/ölçümler/okuma | Ölçümleri okuma |
> | Action | Microsoft. Insights/ölçümler/okuma | Ölçümleri okuma |
> | DataAction | Microsoft. Insights/ölçümler/yazma | Ölçümleri yaz |
> | Action | Microsoft. Insights/MigrateToNewpricingModel/Action | Aboneliği yeni fiyatlandırma modeline geçir |
> | Action | Microsoft. Insights/Işlemler/okuma | Okuma işlemleri |
> | Action | Microsoft.Insights/Register/Action | Microsoft Insights sağlayıcısını kaydedin |
> | Action | Microsoft. Insights/RollbackToLegacyPricingModel/Action | Aboneliği eski fiyatlandırma modeline geri al |
> | Action | Microsoft. Insights/ScheduledQueryRules/Delete | Zamanlanmış bir sorgu kuralı siliniyor |
> | Action | Microsoft. Insights/ScheduledQueryRules/Read | Zamanlanmış bir sorgu kuralı okunuyor |
> | Action | Microsoft. Insights/ScheduledQueryRules/Write | Zamanlanmış bir sorgu kuralı yazılıyor |
> | Action | Microsoft. Insights/kiracılar/Register/Action | Microsoft Insights sağlayıcısını başlatır |
> | Action | Microsoft. Öngörüler/kaydını kaldırma/eylem | Microsoft Insights sağlayıcısını kaydedin |
> | Action | Microsoft. Insights/Web testleri/silme | Bir web testi yapılandırmasını silme |
> | Action | Microsoft. Insights/webtests/GetToken/Read | Bir web testi belirtecini okuma |
> | Action | Microsoft. Insights/Web testleri/MetricDefinitions/okuma | Bir web testi ölçümünün tanımlarını okuma |
> | Action | Microsoft. Insights/webtests/ölçümler/okuma | Bir web testinin ölçümlerini okuma |
> | Action | Microsoft. Insights/Web testleri/okuma | Bir web testi yapılandırmasını okuma |
> | Action | Microsoft. Insights/Web testleri/yazma | Bir web testi yapılandırmasına yazma |
> | Action | Microsoft. Öngörüler/çalışma kitapları/silme | Çalışma kitabını silme |
> | Action | Microsoft. Insights/çalışma kitapları/okuma | Çalışma kitabını okuma |
> | Action | Microsoft. Insights/çalışma kitapları/yazma | Çalışma kitabı oluşturma veya güncelleştirme |

## <a name="microsoftintune"></a>Microsoft. Intune

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Intune/diagnosticsettings/Delete | Bir tanılama ayarını silme |
> | Action | Microsoft. Intune/diagnosticsettings/Read | Tanılama ayarını okuma |
> | Action | Microsoft. Intune/diagnosticsettings/Write | Tanılama ayarı yazma |
> | Action | Microsoft. Intune/diagnosticsettingscategories/Read | Tanılama ayarı kategorilerini okuma |

## <a name="microsoftiotcentral"></a>Microsoft. ıotcentral

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. ıotcentral/appTemplates/Action | Azure IoT Central tüm kullanılabilir uygulama şablonlarını alır |
> | Action | Microsoft. ıotcentral/Checknameavaılabılıty/Action | IoT Central bir uygulama adının kullanılabilir olup olmadığını denetler |
> | Action | Microsoft. ıotcentral/Checksubdomainavaılabılıty/Action | IoT Central uygulama alt etki alanının kullanılabilir olup olmadığını denetler |
> | Action | Microsoft. ıotcentral/ıotapps/Delete | IoT Central uygulamalarını siler |
> | Action | Microsoft. ıotcentral/ıotapps/Read | Tek bir IoT Central uygulaması alır |
> | Action | Microsoft. ıotcentral/ıotapps/Write | IoT Central uygulamalar oluşturur veya güncelleştirir |
> | Action | Microsoft. ıotcentral/işlemler/okuma | IoT Central uygulamalarda tüm kullanılabilir işlemleri alır |
> | Action | Microsoft. ıotcentral/Register/Action | Azure IoT Central kaynak sağlayıcısı için aboneliği kaydetme |

## <a name="microsoftiotspaces"></a>Microsoft. ıotspaces

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. ıotspaces/Graf/Delete | Microsoft. ıotspaces Graph kaynağını siler |
> | Action | Microsoft. ıotspaces/Graf/Read | Microsoft. ıotspaces grafik kaynakları alır |
> | Action | Microsoft. ıotspaces/Graf/Write | Microsoft. ıotspaces grafik kaynağı oluştur |
> | Action | Microsoft. ıotspaces/yazmaç/Action | Kaynak oluşturmayı etkinleştirmek için Microsoft. ıotspaces Graph kaynak sağlayıcısı için aboneliği kaydedin |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Keykasası/Checknameavaılabılıty/Read | Anahtar Kasası adının geçerli olduğunu ve kullanımda olup olmadığını denetler |
> | Action | Microsoft. Keykasası/Silinleults/okuma | Geçici olarak silinen anahtar kasalarının özelliklerini görüntüleyin |
> | Action | Microsoft. Keykasası/hsmPools/Delete | Bir HSM havuzunu silin |
> | Action | Microsoft. Keykasası/hsmPools/Joinkasa/eylem | Bir HSM havuzuna Anahtar Kasası ekleme |
> | Action | Microsoft. Keykasası/hsmPools/Read | Bir HSM havuzunun özelliklerini görüntüleme |
> | Action | Microsoft. Keykasası/hsmPools/Write | Yeni bir HSM havuzu oluşturun var olan bir HSM havuzunun özelliklerini güncelleştir |
> | Action | Microsoft. Keykasası/konumlar/Silinkaults/Temizleme/eylem | Geçici olarak silinen bir anahtar kasasını temizle |
> | Action | Microsoft. Keykasası/konumlar/Silinkaults/okuma | Geçici olarak silinen bir anahtar kasasının özelliklerini görüntüleyin |
> | Action | Microsoft. Keykasası/konumlar/Deletevirtualnetworkoralt ağları/eylem | Bir sanal ağ veya alt ağın silinmekte olduğunu Microsoft. Keykasasına bildirir |
> | Action | Microsoft. Keykasası/konumlar/operationResults/Read | Uzun bir çalıştırma işleminin sonucunu denetleyin |
> | Action | Microsoft. Keykasası/işlemler/okuma | Microsoft. Keykasası kaynak sağlayıcısında kullanılabilir işlemleri listeler |
> | Action | Microsoft. Keykasası/Register/ACTION | Bir aboneliği kaydeder |
> | Action | Microsoft. Keykasası/kaydını kaldırma/eylem | Abonelik kaydını siler |
> | Action | Microsoft. Keykasası/Vaults/accessPolicies/Write | Mevcut bir erişim ilkesini birleştirerek veya değiştirerek veya bir kasaya yeni bir erişim ilkesi ekleyerek güncelleştirin. |
> | Action | Microsoft. Keykasası/kasa/silme | Bir anahtar kasasını silin |
> | Action | Microsoft. Keykasası/Vaults/dağıtım/eylem | Azure kaynaklarını dağıttığınızda bir anahtar kasasındaki gizli dizileri erişimine izin sağlar |
> | Action | Microsoft. Keykasası/Vaults/eventGridFilters/Delete | Microsoft. Keykasasına Key Vault için bir EventGrid aboneliğinin silinmekte olduğunu bildirir |
> | Action | Microsoft. Keykasası/Vaults/eventGridFilters/Read | Microsoft. Keykasasına Key Vault için bir EventGrid aboneliğinin görüntülendiğini bildirir |
> | Action | Microsoft. Keykasası/Vaults/eventGridFilters/Write | Microsoft. Keykasasında Key Vault yeni bir EventGrid aboneliğinin oluşturulduğunu bildirir |
> | Action | Microsoft. Keykasası/Kasaults/okuma | Anahtar kasasının özelliklerini görüntüleme |
> | Action | Microsoft. Keykasası/Kasaults/gizlilikler/Read | Gizli anahtar özelliklerini görüntüleyin, ancak değerini değil. |
> | Action | Microsoft. Keykasası/kasa/gizli dizi/yazma | Yeni bir gizli dizi oluşturun veya var olan bir parolanın değerini güncelleştirin. |
> | Action | Microsoft. Keykasası/Vaults/yazma | Yeni bir Anahtar Kasası oluşturun veya var olan bir anahtar kasasının özelliklerini güncelleştirin |

## <a name="microsoftkusto"></a>Microsoft. kusto

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. kusto/kümeler/etkinleştir/eylem | Kümeyi başlatır. |
> | Action | Microsoft. kusto/kümeler/AttachedDatabaseConfigurations/Delete | Eklenmiş bir veritabanı yapılandırma kaynağını siler. |
> | Action | Microsoft. kusto/kümeler/AttachedDatabaseConfigurations/Read | Eklenmiş bir veritabanı yapılandırma kaynağını okur. |
> | Action | Microsoft. kusto/kümeler/AttachedDatabaseConfigurations/Write | Eklenmiş bir veritabanı yapılandırma kaynağı yazar. |
> | Action | Microsoft. kusto/kümeler/Checknameavaılabılıty/Action | Küme adının kullanılabilirliğini denetler. |
> | Action | Microsoft. kusto/kümeler/veritabanları/Addsorumlularını/eylemi | Veritabanı sorumlularını ekler. |
> | Action | Microsoft. kusto/kümeler/veritabanları/Checknameavaılabılıty/Action | Verilen bir tür için ad kullanılabilirliğini denetler. |
> | Action | Microsoft. kusto/kümeler/veritabanları/veri bağlantıları/silme | Bir veri bağlantıları kaynağını siler. |
> | Action | Microsoft. kusto/kümeler/veritabanları/veri bağlantıları/okuma | Bir veri bağlantıları kaynağını okur. |
> | Action | Microsoft. kusto/kümeler/veritabanları/veri bağlantıları/yazma | Bir veri bağlantıları kaynağı yazar. |
> | Action | Microsoft. kusto/kümeler/veritabanları/DataConnectionValidation/Action | Veritabanı veri bağlantısını doğrular. |
> | Action | Microsoft. kusto/kümeler/veritabanları/Sil | Bir veritabanı kaynağını siler. |
> | Action | Microsoft. kusto/kümeler/veritabanları/EventHubConnections/Delete | Bir olay hub 'ı bağlantı kaynağını siler. |
> | Action | Microsoft. kusto/kümeler/veritabanları/EventHubConnections/Read | Bir olay hub 'ı bağlantı kaynağını okur. |
> | Action | Microsoft. kusto/kümeler/veritabanları/EventHubConnections/Write | Bir olay hub 'ı bağlantı kaynağı yazar. |
> | Action | Microsoft. kusto/kümeler/veritabanları/EventHubConnectionValidation/Action | Veritabanı Olay Hub 'ı bağlantısını doğrular. |
> | Action | Microsoft. kusto/kümeler/veritabanları/Listsorumlularını/eylemi | Veritabanı sorumlularını listeler. |
> | Action | Microsoft. kusto/kümeler/veritabanları/okuma | Bir veritabanı kaynağını okur. |
> | Action | Microsoft. kusto/kümeler/veritabanları/Removesorumlular/eylem | Veritabanı sorumlularını kaldırır. |
> | Action | Microsoft. kusto/kümeler/veritabanları/yazma | Bir veritabanı kaynağı yazar. |
> | Action | Microsoft. kusto/kümeler/devre dışı bırakma/eylem | Kümeyi sonlandırır. |
> | Action | Microsoft. kusto/kümeler/Sil | Bir küme kaynağını siler. |
> | Action | Microsoft. kusto/kümeler/okuma | Bir küme kaynağını okur. |
> | Action | Microsoft. kusto/kümeler/SKU 'Lar/okuma | Küme SKU kaynağını okur. |
> | Action | Microsoft. kusto/kümeler/Başlat/eylem | Kümeyi başlatır. |
> | Action | Microsoft. kusto/kümeler/durdur/eylem | Kümeyi sonlandırır. |
> | Action | Microsoft. kusto/kümeler/yazma | Bir küme kaynağı yazar. |
> | Action | Microsoft. kusto/Locations/Checknameavaılabılıty/Action | Kaynak adı kullanılabilirliğini denetler. |
> | Action | Microsoft. kusto/konumlar/operationresults/Read | İşlem kaynaklarını okur |
> | Action | Microsoft. kusto/Işlemler/okuma | İşlem kaynaklarını okur |
> | Action | Microsoft. kusto/Register/ACTION | Abonelik kaydı eylemi |
> | Action | Microsoft. kusto/Register/ACTION | Aboneliği kusto kaynak sağlayıcısına kaydeder. |
> | Action | Microsoft. kusto/SKU/okuma | Bir SKU kaynağını okur. |
> | Action | Microsoft. kusto/Unregister/eylem | Kusto kaynak sağlayıcısına aboneliğin kaydını siler. |

## <a name="microsoftlabservices"></a>Microsoft. LabServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. LabServices/labAccounts/CreateLab/eylem | Laboratuvar hesabında laboratuvar oluşturun. |
> | Action | Microsoft. LabServices/labAccounts/Delete | Laboratuvar hesaplarını silin. |
> | Action | Microsoft. LabServices/labAccounts/Gallerımages/Delete | Galeri görüntülerini silin. |
> | Action | Microsoft. LabServices/labAccounts/Gallerımages/Read | Galeri görüntülerini okuyun. |
> | Action | Microsoft. LabServices/labAccounts/Gallerımages/Write | Galeri görüntülerini ekleyin veya değiştirin. |
> | Action | Microsoft. LabServices/labAccounts/GetRegionalAvailability/eylem | Laboratuvar hesabı altında yapılandırılan her boyut kategorisi için bölgesel kullanılabilirlik bilgilerini al |
> | Action | Microsoft. LabServices/labAccounts/Labs/AddUsers/Action | Laboratuvara Kullanıcı ekleme |
> | Action | Microsoft. LabServices/labAccounts/Labs/Delete | Laboratuvarları silin. |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/Delete | Ortam ayarını silin. |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/ortamlar/Sil | Ortamları silin. |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/ortamlar/okuma | Ortamları okuyun. |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/ortamlar/ResetPassword/Action | Bir ortamdaki Kullanıcı parolasını sıfırlar |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/ortamlar/start/Action | Ortamın içindeki tüm kaynakları başlatarak bir ortamı başlatır. |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/ortamlar/durdur/eylem | Ortamın içindeki tüm kaynakları durdurarak bir ortamı durduruyor |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/ortamlar/Write | Ortamları ekleme veya değiştirme. |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/Yayımla/eylem | Laboratuvar/ortam ayarının geçerli durumuna bağlı olarak bir ortam ayarı için gerekli kaynakları sağlar/hazırlar. |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/Read | Ortam ayarını okuyun. |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/ResetPassword/Action | Şablon sanal makinesindeki parolayı sıfırlar. |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/SaveImage/Action | Geçerli şablon görüntüsünü laboratuvar hesabındaki paylaşılan galeriye kaydeder |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/zamanlamalar/silme | Zamanlamaları silin. |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/zamanlamalar/okuma | Zamanlamaları okuyun. |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/zamanlamalar/yazma | Zamanlamaları ekleyin veya değiştirin. |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/start/Action | Şablon içindeki tüm kaynakları başlatarak bir şablonu başlatır. |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/durdur/eylem | Şablon içindeki tüm kaynakları durdurarak bir şablonu sonlandırır. |
> | Action | Microsoft. LabServices/labAccounts/Labs/environmentSettings/Write | Ortam ayarı ekleyin veya değiştirin. |
> | Action | Microsoft. LabServices/labAccounts/Labs/Read | Labs 'i okuyun. |
> | Action | Microsoft. LabServices/labAccounts/Labs/Sendeposta/eylem | Laboratuvara kayıt bağlantısı ile e-posta gönderin |
> | Action | Microsoft. LabServices/labAccounts/Labs/kullanıcılar/Sil | Kullanıcıları silin. |
> | Action | Microsoft. LabServices/labAccounts/Labs/kullanıcılar/okuma | Kullanıcıları okuyun. |
> | Action | Microsoft. LabServices/labAccounts/Labs/kullanıcılar/yazma | Kullanıcıları ekleyin veya değiştirin. |
> | Action | Microsoft. LabServices/labAccounts/Labs/Write | Labs ekleme veya değiştirme. |
> | Action | Microsoft. LabServices/labAccounts/okuma | Laboratuvar hesaplarını okuyun. |
> | Action | Microsoft. LabServices/labAccounts/Sharedgalerileri/silme | Sharedgalerileri silin. |
> | Action | Microsoft. LabServices/labAccounts/Sharedgaleriler/okuma | Sharedgaleriler ' i okuyun. |
> | Action | Microsoft. LabServices/labAccounts/Sharedgaleriler/yazma | Sharedgaleriler ekleyin veya değiştirin. |
> | Action | Microsoft. LabServices/labAccounts/sharedImages/Delete | Parça silme. |
> | Action | Microsoft. LabServices/labAccounts/sharedImages/Read | Köpkmages 'ları okuyun. |
> | Action | Microsoft. LabServices/labAccounts/sharedImages/Write | Parça ekleme veya değiştirme. |
> | Action | Microsoft. LabServices/labAccounts/Write | Laboratuvar hesapları ekleyin veya değiştirin. |
> | Action | Microsoft. LabServices/konumlar/işlemler/okuma | Okuma işlemleri. |
> | Action | Microsoft. LabServices/Register/ACTION | Aboneliği kaydeder |
> | Action | Microsoft. LabServices/Users/GetOperationBatchStatus/Action | Toplu işlem durumunu al |
> | Action | Microsoft. LabServices/kullanıcılar/GetOperationStatus/Action | Uzun süre çalışan işlemin durumunu alır |
> | Action | Microsoft. LabServices/Users/Getpersonal tercihleri/eylemi | Bir kullanıcı için kişisel tercihlerini al |
> | Action | Microsoft. LabServices/Users/ListAllEnvironments/ACTION | Kullanıcının tüm ortamlarını listeleyin |
> | Action | Microsoft. LabServices/kullanıcılar/Register/Action | Bir kullanıcıyı yönetilen laboratuvara kaydetme |
> | Action | Microsoft. LabServices/kullanıcılar/ResetPassword/Action | Bir ortamdaki Kullanıcı parolasını sıfırlar |
> | Action | Microsoft. LabServices/Users/StartEnvironment/eylem | Ortamın içindeki tüm kaynakları başlatarak bir ortamı başlatır. |
> | Action | Microsoft. LabServices/kullanıcılar/StopEnvironment/Action | Ortamın içindeki tüm kaynakları durdurarak bir ortamı durduruyor |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Logic/ıntegrationaccounts/sözleşmeleri/silme | Tümleştirme hesabındaki sözleşmeyi siler. |
> | Action | Microsoft. Logic/ıntegrationaccounts/sözleşmeleri/listContentCallbackUrl/eylem | Tümleştirme hesabındaki anlaşma içeriği için geri çağırma URL 'sini alır. |
> | Action | Microsoft. Logic/ıntegrationaccounts/sözleşmeleri/okuma | Tümleştirme hesabındaki sözleşmeyi okur. |
> | Action | Microsoft. Logic/ıntegrationaccounts/sözleşmeleri/yazma | Tümleştirme hesabındaki sözleşmeyi oluşturur veya güncelleştirir. |
> | Action | Microsoft. Logic/ıntegrationaccounts/derlemeler/Sil | Tümleştirme hesabındaki derlemeyi siler. |
> | Action | Microsoft. Logic/ıntegrationaccounts/derlemeler/listContentCallbackUrl/eylem | Tümleştirme hesabındaki derleme içeriği için geri çağırma URL 'sini alır. |
> | Action | Microsoft. Logic/ıntegrationaccounts/derlemeler/okuma | Tümleştirme hesabındaki derlemeyi okur. |
> | Action | Microsoft. Logic/ıntegrationaccounts/derlemeler/Write | Tümleştirme hesabındaki derlemeyi oluşturur veya güncelleştirir. |
> | Action | Microsoft. Logic/ıntegrationaccounts/batchConfigurations/Delete | Tümleştirme hesabındaki toplu yapılandırmayı siler. |
> | Action | Microsoft. Logic/ıntegrationaccounts/batchConfigurations/Read | Tümleştirme hesabındaki Batch yapılandırmasını okur. |
> | Action | Microsoft. Logic/ıntegrationaccounts/batchConfigurations/Write | Tümleştirme hesabındaki toplu yapılandırmayı oluşturur veya güncelleştirir. |
> | Action | Microsoft. Logic/ıntegrationaccounts/Certificates/Delete | Tümleştirme hesabındaki sertifikayı siler. |
> | Action | Microsoft. Logic/ıntegrationaccounts/Certificates/Read | Tümleştirme hesabındaki sertifikayı okur. |
> | Action | Microsoft. Logic/ıntegrationaccounts/Certificates/Write | Tümleştirme hesabındaki sertifikayı oluşturur veya güncelleştirir. |
> | Action | Microsoft. Logic/ıntegrationaccounts/Delete | Tümleştirme hesabını siler. |
> | Action | Microsoft. Logic/ıntegrationaccounts/JOIN/Action | Tümleştirme hesabını birleştirir. |
> | Action | Microsoft. Logic/ıntegrationaccounts/listCallbackUrl/eylem | Tümleştirme hesabı için geri çağırma URL 'sini alır. |
> | Action | Microsoft. Logic/ıntegrationaccounts/listKeyVaultKeys/Action | Anahtar kasasındaki anahtarları alır. |
> | Action | Microsoft. Logic/ıntegrationaccounts/logTrackingEvents/eylem | Tümleştirme hesabındaki izleme olaylarını günlüğe kaydeder. |
> | Action | Microsoft. Logic/ıntegrationaccounts/haritalar/Sil | Tümleştirme hesabındaki eşlemeyi siler. |
> | Action | Microsoft. Logic/ıntegrationaccounts/Maps/listContentCallbackUrl/eylem | Tümleştirme hesabındaki eşleme içeriği için geri çağırma URL 'sini alır. |
> | Action | Microsoft. Logic/ıntegrationaccounts/Maps/Read | Tümleştirme hesabındaki eşlemeyi okur. |
> | Action | Microsoft. Logic/ıntegrationaccounts/Maps/Write | Tümleştirme hesabındaki eşlemeyi oluşturur veya güncelleştirir. |
> | Action | Microsoft. Logic/ıntegrationaccounts/partner/Delete | Tümleştirme hesabındaki ortağı siler. |
> | Action | Microsoft. Logic/ıntegrationaccounts/Partners/listContentCallbackUrl/eylem | Tümleştirme hesabındaki iş ortağı içeriği için geri çağırma URL 'sini alır. |
> | Action | Microsoft. Logic/ıntegrationaccounts/partner/Read | Tümleştirme hesabındaki ortağı okur. |
> | Action | Microsoft. Logic/ıntegrationaccounts/partner/Write | Tümleştirme hesabındaki iş ortağını oluşturur veya güncelleştirir. |
> | Action | Microsoft. Logic/ıntegrationaccounts/Providers/Microsoft. Insights/logDefinitions/Read | Tümleştirme hesabı günlük tanımlarını okur. |
> | Action | Microsoft. Logic/ıntegrationaccounts/Read | Tümleştirme hesabını okur. |
> | Action | Microsoft. Logic/ıntegrationaccounts/regenerateAccessKey/Action | Erişim anahtarı gizli dizilerini yeniden oluşturur. |
> | Action | Microsoft. Logic/ıntegrationaccounts/şemalar/Delete | Tümleştirme hesabındaki şemayı siler. |
> | Action | Microsoft. Logic/ıntegrationaccounts/schemas/listContentCallbackUrl/eylem | Tümleştirme hesabındaki şema içeriği için geri çağırma URL 'sini alır. |
> | Action | Microsoft. Logic/ıntegrationaccounts/schemas/Read | Tümleştirme hesabındaki şemayı okur. |
> | Action | Microsoft. Logic/ıntegrationaccounts/şemalar/Write | Tümleştirme hesabındaki şemayı oluşturur veya güncelleştirir. |
> | Action | Microsoft. Logic/ıntegrationaccounts/Sessions/Delete | Tümleştirme hesabındaki oturumu siler. |
> | Action | Microsoft. Logic/ıntegrationaccounts/Sessions/Read | Tümleştirme hesabındaki Batch yapılandırmasını okur. |
> | Action | Microsoft. Logic/ıntegrationaccounts/Sessions/Write | Tümleştirme hesabındaki oturumu oluşturur veya güncelleştirir. |
> | Action | Microsoft. Logic/ıntegrationaccounts/Write | Tümleştirme hesabı oluşturur veya güncelleştirir. |
> | Action | Microsoft. Logic/ıntegrationserviceortamlarını/silme | Tümleştirme hizmeti ortamını siler. |
> | Action | Microsoft. Logic/ıntegrationserviceortamları/JOIN/Action | Tümleştirme Hizmeti Ortamı birleştirir. |
> | Action | Microsoft. Logic/ıntegrationserviceortamlarını/Managedap/apiOperations/Read | Tümleştirme hizmeti ortamı yönetilen API işlemini okur. |
> | Action | Microsoft. Logic/ıntegrationserviceortamlarını/Managedap/Read | Tümleştirme hizmeti ortamı yönetilen API 'sini okur. |
> | Action | Microsoft. Logic/ıntegrationserviceortamlarını/Operationdurumlarının/Read | Tümleştirme hizmeti ortamı işlem durumlarını okur. |
> | Action | Microsoft. Logic/ıntegrationserviceortamları/sağlayıcılar/Microsoft. Insights/metricDefinitions/okuma | Tümleştirme hizmeti ortamı ölçüm tanımlarını okur. |
> | Action | Microsoft. Logic/ıntegrationserviceortamları/okuma | Tümleştirme hizmeti ortamını okur. |
> | Action | Microsoft. Logic/ıntegrationserviceortamlar/Write | Tümleştirme hizmeti ortamını oluşturur veya güncelleştirir. |
> | Action | Microsoft. Logic/Locations/iş akışları/recommendOperationGroups/Action | İş akışı önerilir işlem grupları alır. |
> | Action | Microsoft. Logic/Locations/iş akışları/Validate/Action | İş akışını doğrular. |
> | Action | Microsoft. Logic/işlemler/okuma | İşlemi alır. |
> | Action | Microsoft. Logic/Register/ACTION | Belirli bir abonelik için Microsoft. Logic Resource sağlayıcısını kaydeder. |
> | Action | Microsoft. Logic/iş akışları/accessKeys/Delete | Erişim anahtarını siler. |
> | Action | Microsoft. Logic/iş akışları/accessKeys/List/Action | Erişim anahtarı gizli dizilerini listeler. |
> | Action | Microsoft. Logic/iş akışları/accessKeys/Read | Erişim anahtarını okur. |
> | Action | Microsoft. Logic/iş akışları/accessKeys/yeniden oluştur/eylem | Erişim anahtarı gizli dizilerini yeniden oluşturur. |
> | Action | Microsoft. Logic/iş akışları/accessKeys/Write | Erişim anahtarını oluşturur veya güncelleştirir. |
> | Action | Microsoft. Logic/iş akışları/silme | İş akışını siler. |
> | Action | Microsoft. Logic/iş akışları/devre dışı bırakma/eylem | İş akışını devre dışı bırakır. |
> | Action | Microsoft. Logic/iş akışları/etkinleştir/eylem | İş akışını izin vermez. |
> | Action | Microsoft. Logic/iş akışları/listCallbackUrl/eylem | İş akışı için geri çağırma URL 'sini alır. |
> | Action | Microsoft. Logic/iş akışları/listSwagger/eylem | İş akışı Swagger tanımlarını alır. |
> | Action | Microsoft. Logic/iş akışları/taşıma/eylem | Iş akışını var olan abonelik kimliği, kaynak grubu ve/veya adından farklı bir abonelik kimliğine, kaynak grubuna ve/veya ada taşıdıkça. |
> | Action | Microsoft. Logic/iş akışları/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Read | İş akışı tanılama ayarlarını okur. |
> | Action | Microsoft. Logic/iş akışları/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Write | İş akışı tanılama ayarını oluşturur veya güncelleştirir. |
> | Action | Microsoft. Logic/iş akışları/sağlayıcılar/Microsoft. Insights/logDefinitions/okuma | İş akışı günlük tanımlarını okur. |
> | Action | Microsoft. Logic/iş akışları/sağlayıcılar/Microsoft. Insights/metricDefinitions/Read | İş akışı ölçüm tanımlarını okur. |
> | Action | Microsoft. Logic/iş akışları/okuma | İş akışını okur. |
> | Action | Microsoft. Logic/iş akışları/regenerateAccessKey/Action | Erişim anahtarı gizli dizilerini yeniden oluşturur. |
> | Action | Microsoft. Logic/iş akışları/çalıştırma/eylem | İş akışının çalıştırılmasını başlatır. |
> | Action | Microsoft. Logic/iş akışları/çalıştırmalar/Actions/Listexpressionizlemeler/eylem | İş akışı çalıştırma eylemi ifade izlemelerini alır. |
> | Action | Microsoft. Logic/iş akışları/çalıştırmalar/eylemler/okuma | İş akışı çalıştırma eylemini okur. |
> | Action | Microsoft. Logic/iş akışları/çalıştırmalar/Actions/tekrarları/Listexpressionizlemeler/eylem | İş akışı çalıştırma eylemi yineleme ifadesi izlemelerini alır. |
> | Action | Microsoft. Logic/iş akışları/çalıştırmalar/Actions/tekrarları/Read | İş akışı çalıştırma eylemi tekrarlamayı okur. |
> | Action | Microsoft. Logic/iş akışları/çalıştırmalar/Actions/tekrarları/requestHistories/Read | İş akışı çalıştırma yineleme eylemi istek geçmişini okur. |
> | Action | Microsoft. Logic/iş akışları/çalıştırmalar/Actions/requestHistories/Read | İş akışı çalıştırma eylemi istek geçmişini okur. |
> | Action | Microsoft. Logic/iş akışları/çalıştırmalar/eylemler/scoperepetitions/Read | İş akışı çalıştırma eylemi kapsam tekrarlamayı okur. |
> | Action | Microsoft. Logic/iş akışları/çalıştırmalar/iptal/eylem | Bir iş akışının çalıştırılmasını iptal eder. |
> | Action | Microsoft. Logic/iş akışları/çalıştırmalar/Sil | Bir iş akışının çalıştırılmasını siler. |
> | Action | Microsoft. Logic/iş akışları/çalıştırmalar/işlemler/okuma | İş akışı çalıştırma işlemi durumunu okur. |
> | Action | Microsoft. Logic/iş akışları/çalıştırmalar/Read | İş akışı çalıştırmasını okur. |
> | Action | Microsoft. Logic/iş akışları/beklet/eylem | İş akışını askıya alır. |
> | Action | Microsoft. Logic/iş akışları/Tetikleyiciler/geçmiş/okuma | Tetikleyici geçmişlerini okur. |
> | Action | Microsoft. Logic/iş akışları/Tetikleyiciler/geçmiş/yeniden gönder/eylem | Resubmits iş akışı tetikleyicisi. |
> | Action | Microsoft. Logic/iş akışları/Tetikleyiciler/listCallbackUrl/eylem | Tetikleyici için geri çağırma URL 'sini alır. |
> | Action | Microsoft. Logic/iş akışları/Tetikleyiciler/okuma | Tetikleyiciyi okur. |
> | Action | Microsoft. Logic/iş akışları/Tetikleyiciler/sıfırlama/eylem | Tetikleyiciyi sıfırlar. |
> | Action | Microsoft. Logic/iş akışları/Tetikleyiciler/çalıştırma/eylem | Tetikleyiciyi yürütür. |
> | Action | Microsoft. Logic/iş akışları/Tetikleyiciler/setState/Action | Tetikleyici durumunu ayarlar. |
> | Action | Microsoft.Logic/workflows/validate/action | İş akışını doğrular. |
> | Action | Microsoft. Logic/iş akışları/sürümler/okuma | İş akışı sürümünü okur. |
> | Action | Microsoft. Logic/iş akışları/sürümler/Tetikleyiciler/listCallbackUrl/eylem | Tetikleyici için geri çağırma URL 'sini alır. |
> | Action | Microsoft. Logic/iş akışları/yazma | İş akışını oluşturur veya güncelleştirir. |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Machinöğrenim/Commitmentplanlar/commitmentAssociations/Move/Action | Tüm Machine Learning taahhüt planı Ilişkilendirmesini taşıyın |
> | Action | Microsoft. Machinöğrenim/Commitmentplanlar/commitmentAssociations/okuma | Machine Learning taahhüt planı Ilişkilendirmesini okuyun |
> | Action | Microsoft. Machinöğrenim/Commitmentplanlar/silme | Machine Learning taahhüt planını Sil |
> | Action | Microsoft. Machinöğrenim/Commitmentplanlar/Birleştir/eylem | Machine Learning taahhüt planına katılarak |
> | Action | Microsoft. Machinöğrenim/Commitmentplanlar/okuma | Machine Learning taahhüt planını okuyun |
> | Action | Microsoft. Machinöğrenim/Commitmentplanlar/yazma | Machine Learning taahhüt planı oluşturun veya güncelleştirin |
> | Action | Microsoft. Machinöğrenim/konumlar/operationresults/Read | Machine Learning bir Işlemin sonucunu alın |
> | Action | Microsoft. Machinöğrenim/konumlar/operationsstatus/Read | Devam eden Machine Learning Işleminin durumunu al |
> | Action | Microsoft. Machinöğrenim/işlemler/okuma | Machine Learning Işlemleri al |
> | Action | Microsoft. Machinöğretim/Register/ACTION | Machine Learning Web hizmeti kaynak sağlayıcısı için aboneliği kaydeder ve Web Hizmetleri oluşturmayı sunar. |
> | Action | Microsoft. Machinöğrenim/SKU/okuma | Machine Learning taahhüt planı SKU 'Larını al |
> | Action | Microsoft. Machinöğrenim/webServices/eylem | Desteklenen bölgeler için bölgesel Web hizmeti özellikleri oluşturma |
> | Action | Microsoft. Machinöğrenim/webServices/Delete | Tüm Machine Learning Web hizmetini silme |
> | Action | Microsoft. Machinöğrenim/webServices/ListKeys/Read | Machine Learning Web hizmetine anahtar al |
> | Action | Microsoft. Machinöğrenim/webServices/Read | Herhangi bir Machine Learning Web hizmetini okuyun |
> | Action | Microsoft. Machinöğrenim/webServices/Write | Herhangi bir Machine Learning Web hizmeti oluşturun veya güncelleştirin |
> | Action | Microsoft. Machinöğrenim/çalışma alanları/silme | Machine Learning Çalışma Alanı silme |
> | Action | Microsoft. Machinöğrenim/çalışma alanları/listworkspacekeys/eylem | Machine Learning Çalışma Alanı için anahtarları listeleyin |
> | Action | Microsoft. Machinöğrenim/çalışma alanları/okuma | Machine Learning Çalışma Alanı okuyun |
> | Action | Microsoft. Machinöğrenim/çalışma alanları/resyncstoragekeys/eylem | Machine Learning Çalışma Alanı için yapılandırılmış depolama hesabı anahtarlarını yeniden eşitleme |
> | Action | Microsoft. Machinöğrenim/çalışma alanları/yazma | Machine Learning Çalışma Alanı oluşturun veya güncelleştirin |

## <a name="microsoftmachinelearningservices"></a>Microsoft. MachineLearningServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. MachineLearningServices/konumlar/computeoperationsstatus/Read | Belirli bir işlem işleminin durumunu alır |
> | Action | Microsoft. MachineLearningServices/konumlar/kullanımlar/okuma | Bir abonelikteki AML işlem kaynakları için kullanım raporu |
> | Action | Microsoft. MachineLearningServices/konumlar/vmsizes/Read | Desteklenen VM boyutlarını al |
> | Action | Microsoft. MachineLearningServices/Locations/Workspace Operationsstatus/Read | Belirli bir çalışma alanı işleminin durumunu alır |
> | Action | Microsoft. MachineLearningServices/Register/Action | Machine Learning Services kaynak sağlayıcısı için aboneliği kaydeder |
> | Action | Microsoft. MachineLearningServices/çalışma alanları/hesaplar/Sil | Machine Learning Services çalışma alanındaki işlem kaynaklarını siler |
> | Action | Microsoft. MachineLearningServices/Workspaces/hesaplar/listKeys/Action | Machine Learning Services çalışma alanındaki işlem kaynakları için gizli dizileri listeleyin |
> | Action | Microsoft. MachineLearningServices/Workspaces/hesaplar/listNodes/Action | Machine Learning Services çalışma alanında işlem kaynağı için düğümleri listeleyin |
> | Action | Microsoft. MachineLearningServices/çalışma alanları/hesaplar/okundu | Machine Learning Services çalışma alanlarında işlem kaynaklarını alır |
> | Action | Microsoft. MachineLearningServices/çalışma alanları/hesaplar/yaz | Machine Learning Services çalışma alanlarında işlem kaynaklarını oluşturur veya güncelleştirir |
> | Action | Microsoft. MachineLearningServices/çalışma alanları/silme | Machine Learning Services çalışma alanlarını siler |
> | DataAction | Microsoft. MachineLearningServices/Workspaces/denemeleri/Read | Machine Learning Services çalışma alanlarında denemeleri alır |
> | DataAction | Microsoft. MachineLearningServices/Workspaces/denemeleri/Write | Machine Learning Services çalışma alanlarında denemeleri oluşturur veya güncelleştirir |
> | Action | Microsoft. MachineLearningServices/Workspaces/listKeys/Action | Machine Learning Services çalışma alanı için gizli dizileri listeleme |
> | Action | Microsoft. MachineLearningServices/çalışma alanları/okuma | Machine Learning Services çalışma alanlarını alır |
> | Action | Microsoft. MachineLearningServices/çalışma alanları/yazma | Machine Learning Services çalışma alanı oluşturur veya güncelleştirir |

## <a name="microsoftmanagedidentity"></a>Microsoft. Managedıdentity

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Managedıdentity/kimlikler/okuma | Mevcut bir sistem tarafından atanmış kimliği alır |
> | Action | Microsoft. Managedıdentity/Register/Action | Yönetilen kimlik kaynak sağlayıcısı için aboneliği kaydeder |
> | Action | Microsoft. Managedıdentity/Useratandıdentities/ata/eylem | Mevcut kullanıcı tarafından atanan kimliği bir kaynağa atamak için RBAC eylemi |
> | Action | Microsoft. Managedıdentity/Useratandıdentities/Delete | Mevcut kullanıcı tarafından atanan bir kimliği siler |
> | Action | Microsoft. Managedıdentity/Useratandıdentities/Read | Mevcut kullanıcı tarafından atanan kimliği alır |
> | Action | Microsoft. Managedıdentity/Useratandıdentities/Write | Yeni Kullanıcı tarafından atanmış bir kimlik oluşturur veya var olan bir kullanıcı tarafından atanan kimlikle ilişkili etiketleri güncelleştirir |

## <a name="microsoftmanagedservices"></a>Microsoft. ManagedServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. ManagedServices/Pazar Placeryumuristrationdefinitions/okuma | Yönetilen hizmetler kayıt tanımlarının bir listesini alır. |
> | Action | Microsoft. ManagedServices/Operationdurumlar/okuma | Kaynak için işlem durumunu okur. |
> | Action | Microsoft. ManagedServices/Register/ACTION | Yönetilen hizmetlere kaydolun. |
> | Action | Microsoft. ManagedServices/Registrationatamaları/silme | Yönetilen hizmetler kayıt atamasını kaldırır. |
> | Action | Microsoft. ManagedServices/Registrationatamaları/okuma | Yönetilen hizmetler kayıt atamalarının bir listesini alır. |
> | Action | Microsoft. ManagedServices/Registrationatamaları/yazma | Yönetilen hizmetler kayıt atamasını ekleyin veya değiştirin. |
> | Action | Microsoft. ManagedServices/registrationDefinitions/Sil | Yönetilen hizmetler kayıt tanımını kaldırır. |
> | Action | Microsoft. ManagedServices/registrationDefinitions/okuma | Yönetilen hizmetler kayıt tanımlarının bir listesini alır. |
> | Action | Microsoft. ManagedServices/registrationDefinitions/yaz | Yönetilen hizmetler kayıt tanımı ekleyin veya değiştirin. |
> | Action | Microsoft. ManagedServices/kaydını kaldırma/eylem | Yönetilen hizmetlerden kayıt silme. |

## <a name="microsoftmanagement"></a>Microsoft. Management

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Management/Checknameavaılabılıty/Action | Belirtilen yönetim grubu adının geçerli ve benzersiz olup olmadığını denetler. |
> | Action | Microsoft. Management/getEntities/Action | Kimliği doğrulanmış kullanıcı için tüm varlıkları (Yönetim Grupları, abonelikler vb.) listeleyin. |
> | Action | Microsoft. Management/managementGroups/Delete | Yönetim grubunu silin. |
> | Action | Microsoft. Management/managementGroups/Read | Kimliği doğrulanmış kullanıcı için Yönetim gruplarını listeleyin. |
> | Action | Microsoft. Management/managementGroups/abonelikler/Sil | Aboneliği yönetim grubundan kaldır. |
> | Action | Microsoft. Management/managementGroups/abonelikler/Write | Mevcut aboneliği yönetim grubuyla ilişkilendirir. |
> | Action | Microsoft. Management/managementGroups/Write | Bir yönetim grubu oluşturun veya güncelleştirin. |
> | Action | Microsoft. Management/Register/ACTION | Belirtilen aboneliği Microsoft. Management ile kaydet |

## <a name="microsoftmaps"></a>Microsoft. Maps

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | DataAction | Microsoft. Maps/hesaplar/veri/okuma | Haritalar hesabına veri okuma erişimi verir. |
> | Action | Microsoft. Maps/hesaplar/Sil | Haritalar hesabını silin. |
> | Action | Microsoft. Haritalar/hesaplar/eventGridFilters/Delete | Event Grid filtresini silme |
> | Action | Microsoft. Maps/accounts/Eventgridfilter/Read | Event Grid filtresi al |
> | Action | Microsoft. Maps/accounts/Eventgridfilter/Write | Event Grid filtresi oluşturma veya güncelleştirme |
> | Action | Microsoft. Haritalar/hesaplar/listKeys/ACTION | Liste haritaları hesap anahtarları |
> | Action | Microsoft. Maps/hesaplar/okuma | Haritalar hesabı alın. |
> | Action | Microsoft. Maps/accounts/regenerateKey/ACTION | Yeni haritalar hesabı birincil veya ikincil anahtar oluştur |
> | Action | Microsoft. Maps/hesaplar/yazma | Haritalar hesabı oluşturun veya güncelleştirin. |
> | Action | Microsoft. Maps/işlemler/okuma | Sağlayıcı işlemlerini okuyun |
> | Action | Microsoft. Maps/Register/ACTION | Sağlayıcıyı Kaydet |

## <a name="microsoftmarketplace"></a>Microsoft. Market

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Market/offerTypes/yayımcılar/teklifler/planlar/anlaşmalar/okuma | Bir sözleşme döndürür. |
> | Action | Microsoft. Market/offerTypes/yayımcılar/teklifler/planlar/anlaşmalar/yazma | İmzalı bir sözleşmeyi kabul eder. |
> | Action | Microsoft. Market/offerTypes/yayımcılar/teklifler/planlar/configs/ımportımage/Action | Son kullanıcının ACR 'ye bir görüntü aktarır. |
> | Action | Microsoft. Market/offerTypes/yayımcılar/teklifler/planlar/configs/Read | Bir yapılandırma döndürür. |
> | Action | Microsoft. Market/offerTypes/yayımcılar/teklifler/planlar/configs/Write | Bir yapılandırmayı kaydeder. |
> | Action | Microsoft. Market/Register/ACTION | Microsoft. Market kaynak sağlayıcısını abonelikte kaydeder. |

## <a name="microsoftmarketplaceapps"></a>Microsoft. MarketplaceApps

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. MarketplaceApps/ClassicDevServices/Delete | Klasik bir geliştirme hizmeti kaynağında SILME işlemi yapar. |
> | Action | Microsoft. MarketplaceApps/ClassicDevServices/Listgizlilikler/Action | Klasik bir geliştirme hizmeti kaynak yönetimi anahtarları alır. |
> | Action | Microsoft. MarketplaceApps/Classıdevservices/listSingleSignOnToken/Action | Klasik bir geliştirme hizmeti için çoklu oturum açma URL 'sini alır. |
> | Action | Microsoft. MarketplaceApps/ClassicDevServices/Read | Klasik bir geliştirme hizmetinde bir GET işlemi yapar. |
> | Action | Microsoft. MarketplaceApps/ClassicDevServices/regenerateKey/ACTION | Klasik bir geliştirme hizmeti kaynak yönetimi anahtarları oluşturur. |
> | Action | Microsoft. MarketplaceApps/Işlemler/okuma | Tüm kaynak türleri için işlemleri okuyun. |

## <a name="microsoftmarketplaceordering"></a>Microsoft. Marketplacesıralaması

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Marketplacesıralaması/sözleşmeleri/teklifler/planlar/iptal/eylem | Belirli bir Market öğesi için bir sözleşmeyi iptal etme |
> | Action | Microsoft. Marketplacesıralaması/sözleşmeleri/teklifler/planlar/okuma | Belirli bir Market öğesi için anlaşma döndürme |
> | Action | Microsoft. Marketplacesıralaması/sözleşmeleri/teklifler/planlar/imzala/eylem | Belirli bir Market öğesi için sözleşme imzalama |
> | Action | Microsoft. Marketplacesıralaması/sözleşmeleri/okumak | Belirtilen abonelik altındaki tüm anlaşmaları döndür |
> | Action | Microsoft. Marketplacesıralaması/offertypes/yayımcılar/teklifler/planlar/anlaşmalar/okuma | Belirli bir market sanal makine öğesi için anlaşma alın |
> | Action | Microsoft. Marketplacesıralaması/offertypes/yayımcılar/teklifler/planlar/anlaşmalar/yazma | Belirli bir market sanal makine öğesi için sözleşmeyi imzala veya Iptal etme |
> | Action | Microsoft. Marketplacesıralaması/işlemler/okuma | API 'de olası tüm işlemleri listeleme |

## <a name="microsoftmedia"></a>Microsoft. Media

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Media/checknameavaılabılıty/Action | Media Services hesap adının kullanılabilir olup olmadığını denetler |
> | Action | Microsoft. Media/Locations/Checknameavaılabılıty/Action | Media Services hesap adının kullanılabilir olup olmadığını denetler |
> | Action | Microsoft. Media/mediaservices/accountfilters/Delete | Hesap filtrelerini Sil |
> | Action | Microsoft. Media/mediaservices/accountfilters/okuma | Tüm hesap filtrelerini okuyun |
> | Action | Microsoft. Media/mediaservices/accountfilters/Write | Hesap Filtresi Oluştur veya güncelleştir |
> | Action | Microsoft. Media/mediaservices/varlıklar/assetfilters/Delete | Tüm varlık filtrelerini Sil |
> | Action | Microsoft. Media/mediaservices/varlıklar/assetfilters/Read | Tüm varlık filtrelerini okuyun |
> | Action | Microsoft. Media/mediaservices/varlıklar/assetfilters/Write | Herhangi bir varlık Filtresi Oluştur veya güncelleştir |
> | Action | Microsoft. Media/mediaservices/varlıklar/Sil | Herhangi bir varlığı silme |
> | Action | Microsoft. Media/mediaservices/varlıklar/getEncryptionKey/Action | Varlık şifreleme anahtarını al |
> | Action | Microsoft. Media/mediaservices/varlıklar/listContainerSas/eylem | Varlık kapsayıcısı SAS URL 'Lerini Listele |
> | Action | Microsoft. Media/mediaservices/varlıklar/Liststreamingkonumlandırıcı/eylem | Varlık için akış Bulıcıları Listele |
> | Action | Microsoft. Media/mediaservices/varlıklar/okuma | Herhangi bir varlığı okuyun |
> | Action | Microsoft. Media/mediaservices/varlıklar/yazma | Herhangi bir varlık oluşturun veya güncelleştirin |
> | Action | Microsoft. Media/mediaservices/contentKeyPolicies/Delete | Içerik anahtarı Ilkesini silme |
> | Action | Microsoft. Media/mediaservices/contentKeyPolicies/Getpolicypropertieswithgizlilikler/Action | Gizli dizileri olan Ilke özelliklerini al |
> | Action | Microsoft. Media/mediaservices/contentKeyPolicies/Read | Herhangi bir Içerik anahtarı Ilkesini okuyun |
> | Action | Microsoft. Media/mediaservices/contentKeyPolicies/Write | Içerik anahtar Ilkesi oluştur veya güncelleştir |
> | Action | Microsoft. Media/mediaservices/Delete | Media Services hesabı silme |
> | Action | Microsoft. Media/mediaservices/eventGridFilters/Delete | Event Grid filtresini silme |
> | Action | Microsoft. Media/mediaservices/eventGridFilters/Read | Herhangi bir Event Grid filtresini okuyun |
> | Action | Microsoft. Media/mediaservices/eventGridFilters/Write | Herhangi bir Event Grid filtresi oluşturun veya güncelleştirin |
> | Action | Microsoft. Media/mediaservices/liveEventOperations/Read | Canlı olay Işlemlerini okuyun |
> | Action | Microsoft. Media/mediaservices/liveEvents/Delete | Canlı etkinliği silme |
> | Action | Microsoft. Media/mediaservices/liveEvents/Liveçıktıları/Delete | Canlı çıktıyı silme |
> | Action | Microsoft. Media/mediaservices/liveEvents/Liveçıktıları/Read | Canlı çıktıyı okuyun |
> | Action | Microsoft. Media/mediaservices/liveEvents/Liveçıktıları/Write | Canlı çıkış oluştur veya güncelleştir |
> | Action | Microsoft. Media/mediaservices/liveEvents/Read | Tüm canlı olayları okuyun |
> | Action | Microsoft. Media/mediaservices/liveEvents/Reset/Action | Canlı olay Işlemlerini sıfırlayın |
> | Action | Microsoft. Media/mediaservices/liveEvents/Başlat/eylem | Tüm canlı olay Işlemlerini başlatın |
> | Action | Microsoft. Media/mediaservices/liveEvents/durdur/eylem | Canlı olay Işlemlerini durdur |
> | Action | Microsoft. Media/mediaservices/liveEvents/Write | Canlı bir olay oluşturun veya güncelleştirin |
> | Action | Microsoft. Media/mediaservices/liveOutputOperations/Read | Tüm canlı çıktı Işlemlerini okuyun |
> | Action | Microsoft. Media/mediaservices/okuma | Tüm Media Services hesaplarını okuyun |
> | Action | Microsoft. Media/mediaservices/streamingEndpointOperations/okuma | Tüm akış uç noktası Işlemlerini okuyun |
> | Action | Microsoft. Media/mediaservices/streamingEndpoints/Delete | Tüm akış uç noktasını sil |
> | Action | Microsoft. Media/mediaservices/streamingEndpoints/Read | Tüm akış uç noktasını okuyun |
> | Action | Microsoft. Media/mediaservices/streamingEndpoints/ölçek/eylem | Tüm akış uç noktası Işlemlerini ölçeklendirme |
> | Action | Microsoft. Media/mediaservices/streamingEndpoints/start/Action | Tüm akış uç noktası Işlemlerini başlatma |
> | Action | Microsoft. Media/mediaservices/streamingEndpoints/durdur/eylem | Tüm akış uç noktası Işlemlerini durdur |
> | Action | Microsoft. Media/mediaservices/streamingEndpoints/Write | Herhangi bir akış uç noktası oluşturun veya güncelleştirin |
> | Action | Microsoft. Media/mediaservices/Streamingkonumlandırıcı/silme | Tüm akış bulucuyu silme |
> | Action | Microsoft. Media/mediaservices/Streamingkonumlandırıcı/listContentKeys/ACTION | Liste Içerik anahtarları |
> | Action | Microsoft. Media/mediaservices/Streamingkonumlandırıcı/listPaths/Action | Liste yolları |
> | Action | Microsoft. Media/mediaservices/Streamingkonumlandırıcı/okuma | Herhangi bir akış bulucuyu okuyun |
> | Action | Microsoft. Media/mediaservices/Streamingkonumlandırıcı/yazma | Herhangi bir akış Bulucu oluşturma veya güncelleştirme |
> | Action | Microsoft. Media/mediaservices/streamingPolicies/Delete | Tüm akış Ilkelerini silme |
> | Action | Microsoft. Media/mediaservices/streamingPolicies/okuma | Herhangi bir akış Ilkesini okuyun |
> | Action | Microsoft. Media/mediaservices/streamingPolicies/Write | Herhangi bir akış Ilkesi oluşturun veya güncelleştirin |
> | Action | Microsoft. Media/mediaservices/syncStorageKeys/Action | Bağlı bir Azure depolama hesabı için depolama anahtarlarını eşitler |
> | Action | Microsoft. Media/mediaservices/dönüşümler/silme | Herhangi bir dönüşümü Sil |
> | Action | Microsoft. Media/mediaservices/dönüşümler/işler/cancelJob/Action | İşi İptal Et |
> | Action | Microsoft. Media/mediaservices/dönüşümler/işler/Sil | Herhangi bir Işi silme |
> | Action | Microsoft. Media/mediaservices/dönüşümler/işler/okuma | Herhangi bir Işi okuyun |
> | Action | Microsoft. Media/mediaservices/dönüşümler/işler/yazma | Herhangi bir Işi oluşturun veya güncelleştirin |
> | Action | Microsoft. Media/mediaservices/dönüşümler/okuma | Herhangi bir dönüşümü okuyun |
> | Action | Microsoft. Media/mediaservices/dönüşümler/yazma | Herhangi bir dönüşüm oluşturun veya güncelleştirin |
> | Action | Microsoft. Media/mediaservices/Write | Media Services hesabı oluşturun veya güncelleştirin |
> | Action | Microsoft. Media/işlemler/okuma | Kullanılabilir Işlemleri al |
> | Action | Microsoft. Media/Register/ACTION | Media Services kaynak sağlayıcısı için aboneliği kaydeder ve Media Services hesaplarının oluşturulmasını sunar |
> | Action | Microsoft. Media/Unregister/eylem | Media Services kaynak sağlayıcısı için aboneliğin kaydını siler |

## <a name="microsoftmigrate"></a>Microsoft. Migrate

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Migrate/assessmentprojects/assessmentOptions/Read | Verilen konumda kullanılabilen değerlendirme seçeneklerini alır |
> | Action | Microsoft. Migrate/assessmentprojects/değerlendirmeler/Read | Bir proje içindeki değerlendirmeleri listeler |
> | Action | Microsoft. Migrate/assessmentprojects/Delete | Değerlendirme projesini siler |
> | Action | Microsoft. Migrate/assessmentprojects/gruplar/değerlendirmeler/assessedmachines/Read | Değerlendirilen bir makinenin özelliklerini al |
> | Action | Microsoft. Migrate/assessmentprojects/gruplar/değerlendirmeler/silme | Bir iç değerlendirmeyi siler |
> | Action | Microsoft. Migrate/assessmentprojects/gruplar/değerlendirmeler/downloadurl/eylem | Bir değerlendirme raporunun URL 'sini indirir |
> | Action | Microsoft. Migrate/assessmentprojects/gruplar/değerlendirmeler/okuma | Bir değerlendirmenin özelliklerini alır |
> | Action | Microsoft. Migrate/assessmentprojects/gruplar/değerlendirmeler/yazma | Yeni bir değerlendirme oluşturur veya var olan bir değerlendirmeyi güncelleştirir |
> | Action | Microsoft. Migrate/assessmentprojects/gruplar/Sil | Bir grubu siler |
> | Action | Microsoft. Migrate/assessmentprojects/gruplar/okuma | Bir grubun özelliklerini al |
> | Action | Microsoft. Migrate/assessmentprojects/gruplar/updateMachines/eylem | Makine ekleyerek veya kaldırarak grup güncelleştirme |
> | Action | Microsoft. Migrate/assessmentprojects/gruplar/yaz | Yeni bir grup oluşturur veya var olan bir grubu güncelleştirir |
> | Action | Microsoft. Migrate/assessmentprojects/hiper sanal toplayıcılar/Sil | HyperV toplayıcısını siler |
> | Action | Microsoft. Migrate/assessmentprojects/hypervtoplayıcıları/Read | HyperV toplayıcısının özelliklerini alır |
> | Action | Microsoft. Migrate/assessmentprojects/hypervtoplayıcıları/Write | Yeni bir HyperV toplayıcısı oluşturur veya var olan bir HyperV toplayıcıyı güncelleştirir |
> | Action | Microsoft. Migrate/assessmentprojects/makineler/okuma | Makinenin özelliklerini alır |
> | Action | Microsoft. Migrate/assessmentprojects/Read | Değerlendirme projesinin özelliklerini alır |
> | Action | Microsoft. Migrate/assessmentprojects/vmwarecollectors/Delete | VMware toplayıcısını siler |
> | Action | Microsoft. Migrate/assessmentprojects/vmwarecollectors/Read | VMware toplayıcısının özelliklerini alır |
> | Action | Microsoft. Migrate/assessmentprojects/vmwarecollectors/Write | Yeni bir VMware toplayıcısı oluşturur veya var olan bir VMware toplayıcıyı güncelleştirir |
> | Action | Microsoft. Migrate/assessmentprojects/Write | Yeni bir değerlendirme projesi oluşturur veya var olan bir değerlendirme projesini güncelleştirir |
> | Action | Microsoft. Migrate/konumlar/assessmentOptions/Read | Verilen konumda kullanılabilen değerlendirme seçeneklerini alır |
> | Action | Microsoft. Migrate/Locations/checknameavaılabılıty/Action | Verilen konumdaki belirtilen aboneliğin kaynak adının kullanılabilirliğini denetler |
> | Action | Microsoft. Migrate/migrateprojects/Databaseınstances/Read | Bir veritabanı örneğinin özelliklerini alır |
> | Action | Microsoft. Migrate/migrateprojects/veritabanları/okuma | Bir veritabanının özelliklerini alır |
> | Action | Microsoft. Migrate/migrateprojects/Delete | Bir geçişi projeyi siler |
> | Action | Microsoft. Migrate/migrateprojeler/makineler/okuma | Makinenin özelliklerini alır |
> | Action | Microsoft. Migrate/migrateprojects/MigrateEvents/Delete | Geçiş olayını siler |
> | Action | Microsoft. Migrate/migrateprojeler/MigrateEvents/okuma | Geçiş olaylarının özelliklerini alır. |
> | Action | Microsoft. Migrate/migrateprojects/Read | Projeyi geçir özelliklerini alır |
> | Action | Microsoft. Migrate/migrateprojects/RefreshSummary/Action | Projeyi geçir özetini yeniler |
> | Action | Microsoft. Migrate/migrateprojects/registerTool/Action | Aracı bir geçiş projesine kaydeder |
> | Action | Microsoft. Migrate/migrateprojects/Solutions/cleanupData/Action | Projeyi geçir çözüm verilerini temizle |
> | Action | Microsoft. Migrate/migrateprojects/Solutions/Delete | Bir geçiş projesi çözümünü siler |
> | Action | Microsoft. Migrate/migrateprojects/Solutions/GetConfig/Action | Proje çözümünü geçir yapılandırmasını alır |
> | Action | Microsoft. Migrate/migrateprojects/Solutions/Read | Proje çözümünü geçirme özelliklerini alır |
> | Action | Microsoft. Migrate/migrateprojects/Solutions/Write | Yeni bir geçiş projesi çözümü oluşturur veya var olan bir geçiş projesi çözümünü güncelleştirir |
> | Action | Microsoft. Migrate/migrateprojects/Write | Yeni bir geçiş projesi oluşturur veya var olan bir geçişi projeyi güncelleştirir |
> | Action | Microsoft. Migrate/Işlemler/okuma | Microsoft. Migrate kaynak sağlayıcısı 'nda bulunan işlemleri listeler |
> | Action | Microsoft. Migrate/projeler/değerlendirmeler/okuma | Bir proje içindeki değerlendirmeleri listeler |
> | Action | Microsoft. Migrate/projeler/Sil | Projeyi siler |
> | Action | Microsoft. Migrate/projeler/gruplar/değerlendirmeler/assessedmachines/Read | Değerlendirilen bir makinenin özelliklerini al |
> | Action | Microsoft. Migrate/projeler/gruplar/değerlendirmeler/silme | Bir iç değerlendirmeyi siler |
> | Action | Microsoft. Migrate/projeler/gruplar/değerlendirmeler/downloadurl/eylem | Bir değerlendirme raporunun URL 'sini indirir |
> | Action | Microsoft. Migrate/projeler/gruplar/değerlendirmeler/okuma | Bir değerlendirmenin özelliklerini alır |
> | Action | Microsoft. Migrate/projeler/gruplar/değerlendirmeler/yazma | Yeni bir değerlendirme oluşturur veya var olan bir değerlendirmeyi güncelleştirir |
> | Action | Microsoft. Migrate/projeler/gruplar/Sil | Bir grubu siler |
> | Action | Microsoft. Migrate/projeler/gruplar/okuma | Bir grubun özelliklerini al |
> | Action | Microsoft. Migrate/projeler/gruplar/yaz | Yeni bir grup oluşturur veya var olan bir grubu güncelleştirir |
> | Action | Microsoft. Migrate/projeler/anahtarlar/eylem | Proje için paylaşılan anahtarları alır |
> | Action | Microsoft. Migrate/projeler/makineler/okuma | Makinenin özelliklerini alır |
> | Action | Microsoft. Migrate/projeler/okuma | Projenin özelliklerini alır |
> | Action | Microsoft. Migrate/projeler/yazma | Yeni bir proje oluşturur veya var olan bir projeyi güncelleştirir |
> | Action | Microsoft. Migrate/Register/ACTION | Aboneliği Microsoft. Migrate kaynak sağlayıcısına kaydeder |

## <a name="microsoftmixedreality"></a>Microsoft. MixedReality

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. MixedReality/Register/ACTION | Karma Gerçeklik kaynak sağlayıcısı için bir abonelik kaydeder. |
> | DataAction | Microsoft. MixedReality/SpatialAnchorsAccounts/Create/ACTION | Uzamsal bağlayıcı oluşturma |
> | DataAction | Microsoft. MixedReality/SpatialAnchorsAccounts/Delete | Uzamsal bağlayıcıları Sil |
> | DataAction | Microsoft. MixedReality/SpatialAnchorsAccounts/Discovery/Read | Yakın uzamsal bağlayıcıları bul |
> | DataAction | Microsoft. MixedReality/SpatialAnchorsAccounts/Özellikler/okuma | Uzamsal Tutturucuların özelliklerini al |
> | Action | Microsoft. MixedReality/spatialAnchorsAccounts/Providers/Microsoft. Insights/diagnosticSettings/Read | Microsoft. MixedReality/spatialAnchorsAccounts için tanılama ayarını alır |
> | Action | Microsoft. MixedReality/spatialAnchorsAccounts/Providers/Microsoft. Insights/diagnosticSettings/Write | Microsoft. MixedReality/spatialAnchorsAccounts için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. MixedReality/spatialAnchorsAccounts/Providers/Microsoft. Insights/metricDefinitions/okuma | Microsoft. MixedReality/spatialAnchorsAccounts için kullanılabilir ölçümleri alır |
> | DataAction | Microsoft. MixedReality/SpatialAnchorsAccounts/sorgu/okuma | Uzamsal bağlayıcıları bul |
> | DataAction | Microsoft. MixedReality/SpatialAnchorsAccounts/submitdiag/Read | Azure uzamsal bağlayıcı hizmeti 'nin kalitesini artırmaya yardımcı olmak için tanılama verileri gönderme |
> | DataAction | Microsoft. MixedReality/SpatialAnchorsAccounts/Write | Uzamsal Tutturucuların özelliklerini güncelleştirme |

## <a name="microsoftnetapp"></a>Microsoft. NetApp

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. NetApp/Locations/checkfilepathavaılabılıty/Action | Dosya yolunun kullanılabilir olup olmadığını denetleyin |
> | Action | Microsoft. NetApp/konumlar/checknameavaılabılıty/Action | Kaynak adının kullanılabilir olup olmadığını denetle |
> | Action | Microsoft. NetApp/konumlar/operationresults/Read | Bir işlem sonucu kaynağını okur. |
> | Action | Microsoft. NetApp/konumlar/okuma | Bir kullanılabilirlik denetimi kaynağını okur. |
> | Action | Microsoft. NetApp/netAppAccounts/Capacityhavuzlar/Delete | Havuz kaynağını siler. |
> | Action | Microsoft. NetApp/netAppAccounts/Capacityhavuzlar/Read | Bir havuz kaynağını okur. |
> | Action | Microsoft. NetApp/netAppAccounts/Capacityhavuzlar/birimler/Sil | Birim kaynağını siler. |
> | Action | Microsoft. NetApp/netAppAccounts/Capacityhavuzlar/Volumes/Mount hedefleri/okuma | Bağlama hedef kaynağını okur. |
> | Action | Microsoft. NetApp/netAppAccounts/Capacityhavuzlar/birimler/okuma | Bir birim kaynağını okur. |
> | Action | Microsoft. NetApp/netAppAccounts/Capacityhavuzlar/birimler/anlık görüntüler/Sil | Anlık görüntü kaynağını siler. |
> | Action | Microsoft. NetApp/netAppAccounts/Capacityhavuzlar/birimler/anlık görüntüler/okuma | Anlık görüntü kaynağını okur. |
> | Action | Microsoft. NetApp/netAppAccounts/Capacityhavuzlar/birimler/anlık görüntüler/yazma | Anlık görüntü kaynağı yazar. |
> | Action | Microsoft. NetApp/netAppAccounts/Capacityhavuzlar/birimler/yazma | Birim kaynağı yazar. |
> | Action | Microsoft. NetApp/netAppAccounts/Capacityhavuzlar/Write | Havuz kaynağı yazar. |
> | Action | Microsoft. NetApp/netAppAccounts/Delete | Bir hesap kaynağını siler. |
> | Action | Microsoft. NetApp/netAppAccounts/okuma | Bir hesap kaynağını okur. |
> | Action | Microsoft. NetApp/netAppAccounts/yazma | Bir hesap kaynağı yazar. |
> | Action | Microsoft. NetApp/Işlemler/okuma | İşlem kaynaklarını okur. |
> | Action | Microsoft. NetApp/Register/ACTION | Aboneliği Microsoft. NetApp kaynak sağlayıcısına kaydeder |
> | Action | Microsoft. NetApp/Unregister/eylem | Microsoft. NetApp kaynak sağlayıcısı ile aboneliğin kaydını siler |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Network/Applicationgatewaykullanılabilirliği Blerequestheaders/Read | Application Gateway kullanılabilir Istek üst bilgilerini al |
> | Action | Microsoft. Network/Applicationgatewaykullanılabilirliği Bleresponseheaders/okuma | Application Gateway kullanılabilir yanıt üst bilgisi al |
> | Action | Microsoft. Network/applicationGatewayAvailableServerVariables/Read | Application Gateway kullanılabilir sunucu değişkenlerini al |
> | Action | Microsoft. Network/applicationGatewayAvailableSslOptions/predefinedPolicies/Read | Application Gateway SSL önceden tanımlı Ilkesi |
> | Action | Microsoft. Network/applicationGatewayAvailableSslOptions/Read | Kullanılabilir SSL seçeneklerini Application Gateway |
> | Action | Microsoft. Network/applicationGatewayAvailableWafRuleSets/Read | Application Gateway kullanılabilir WAF kural kümelerini alır |
> | Action | Microsoft. Network/Applicationgateway/Backendavddresspoir/JOIN/Action | Bir uygulama ağ geçidi arka uç adres havuzunu birleştirir. Alertable değil. |
> | Action | Microsoft. Network/Applicationgateway/backendhealth/Action | Uygulama ağ geçidi arka uç durumunu alır |
> | Action | Microsoft. Network/Applicationgateway/Delete | Bir uygulama ağ geçidini siler |
> | Action | Microsoft. Network/Applicationgateway/getBackendHealthOnDemand/Action | Verilen http ayarı ve arka uç havuzu için isteğe bağlı bir uygulama ağ geçidi arka uç durumu alır |
> | Action | Microsoft. Network/Applicationgateway/Read | Bir uygulama ağ geçidini alır |
> | Action | Microsoft. Network/Applicationgateway/start/Action | Bir uygulama ağ geçidi başlatır |
> | Action | Microsoft. Network/Applicationgateway/stop/Action | Bir uygulama ağ geçidini durduruyor |
> | Action | Microsoft. Network/Applicationgateway/Write | Uygulama ağ geçidi oluşturur veya bir uygulama ağ geçidini güncelleştirir |
> | Action | Microsoft. Network/ApplicationGatewayWebApplicationFirewallPolicies/Delete | Application Gateway WAF ilkesini siler |
> | Action | Microsoft. Network/ApplicationGatewayWebApplicationFirewallPolicies/Read | Application Gateway WAF ilkesi alır |
> | Action | Microsoft. Network/ApplicationGatewayWebApplicationFirewallPolicies/Write | Bir Application Gateway WAF ilkesi oluşturur veya bir Application Gateway WAF ilkesini güncelleştirir |
> | Action | Microsoft. Network/applicationSecurityGroups/Delete | Bir uygulama güvenlik grubunu siler |
> | Action | Microsoft. Network/applicationSecurityGroups/Joinıp/Action | Bir IP yapılandırmasını uygulama güvenlik gruplarıyla birleştirir. Alertable değil. |
> | Action | Microsoft. Network/applicationSecurityGroups/joinNetworkSecurityRule/Action | Uygulama güvenlik gruplarına bir güvenlik kuralı birleştirir. Alertable değil. |
> | Action | Microsoft. Network/applicationSecurityGroups/Liztipconfigurations/Action | ApplicationSecurityGroup içindeki IP yapılandırmasını listeler |
> | Action | Microsoft. Network/applicationSecurityGroups/Read | Bir uygulama güvenlik grubu KIMLIĞI alır. |
> | Action | Microsoft. Network/applicationSecurityGroups/Write | Bir uygulama güvenlik grubu oluşturur veya var olan bir uygulama güvenlik grubunu güncelleştirir. |
> | Action | Microsoft. Network/azureFirewallFqdnTags/Read | Azure Güvenlik Duvarı FQDN etiketlerini alır |
> | Action | Microsoft. Network/azurefirewalls/Delete | Azure Güvenlik duvarını silme |
> | Action | Microsoft. Network/azurefirewalls/Read | Azure Güvenlik Duvarı 'Nı al |
> | Action | Microsoft. Network/azurefirewalls/Write | Bir Azure Güvenlik duvarı oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/Bastionkonakları/silme | Bir savunma konağını siler |
> | Action | Microsoft. Network/bastionHosts/okuma | Bir savunma ana bilgisayarı alır |
> | Action | Microsoft. Network/Bastionkonakları/yazma | Savunma Konağı oluşturma veya güncelleştirme |
> | Action | Microsoft. Network/bgpServiceCommunities/Read | BGP hizmeti topluluklarını al |
> | Action | Microsoft. Network/checkFrontDoorNameAvailability/Action | Bir ön kapı adının kullanılabilir olup olmadığını denetler |
> | Action | Microsoft. Network/checkTrafficManagerNameAvailability/Action | Traffic Manager göreli DNS adının kullanılabilirliğini denetler. |
> | Action | Microsoft. Network/Connections/Delete | Deletes VirtualNetworkGatewayConnection |
> | Action | Microsoft. Network/Connections/Read | Gets VirtualNetworkGatewayConnection |
> | Action | Microsoft. Network/Connections/revoke/Action | Bir Express Route bağlantı durumunu Iptal edildi olarak işaretler |
> | Action | Microsoft. Network/Connections/sharedkey/Action | VirtualNetworkGatewayConnection SharedKey al |
> | Action | Microsoft. Network/Connections/sharedKey/Read | Gets VirtualNetworkGatewayConnection SharedKey |
> | Action | Microsoft. Network/Connections/sharedKey/Write | Var olan bir VirtualNetworkGatewayConnection SharedKey oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/Connections/vpndeviceconfigurationscrıpt/Action | VirtualNetworkGatewayConnection 'un VPN cihaz yapılandırmasını alır |
> | Action | Microsoft. Network/Connections/Write | Var olan VirtualNetworkGatewayConnection 'ı oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/ddosCustomPolicies/Delete | DDoS özelleştirilmiş ilkeyi siler |
> | Action | Microsoft. Network/ddosCustomPolicies/Read | DDoS özelleştirilmiş ilke tanımı tanımını alır |
> | Action | Microsoft. Network/ddosCustomPolicies/Write | DDoS özelleştirilmiş bir ilke oluşturur veya var olan bir DDoS özelleştirilmiş ilkesini güncelleştirir |
> | Action | Microsoft. Network/Ddosprotectionplanlar/silme | DDoS koruma planını siler |
> | Action | Microsoft. Network/Ddosprotectionplanlar/JOIN/Action | DDoS koruma planına katılır. Alertable değil. |
> | Action | Microsoft. Network/Ddosprotectionplanlar/okuma | DDoS koruma planını alır |
> | Action | Microsoft. Network/Ddosprotectionplanlar/yazma | DDoS koruma planı oluşturur veya DDoS koruma planını güncelleştirir  |
> | Action | Microsoft. Network/dnsoperationresults/Read | Bir DNS işleminin sonuçlarını alır |
> | Action | Microsoft. Network/dnsoperationdurumlar/okuma | Bir DNS işleminin durumunu alır  |
> | Action | Microsoft. Network/dnszones/A/Delete | Verilen bir adın kayıt kümesini ve ' A ' türünü bir DNS bölgesinden kaldırın. |
> | Action | Microsoft. Network/dnszones/A/Read | ' A ' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi, TTL, Etiketler ve ETag 'in yanı sıra bir kayıt listesi içerir. |
> | Action | Microsoft. Network/dnszones/A/Write | DNS bölgesi içinde ' A ' türünde bir kayıt kümesi oluşturun veya güncelleştirin. Belirtilen kayıtlar, kayıt kümesindeki geçerli kayıtların yerini alır. |
> | Action | Microsoft. Network/dnszones/AAAA/Delete | Verilen bir adın kayıt kümesini ve ' AAAA ' türünü bir DNS bölgesinden kaldırın. |
> | Action | Microsoft. Network/dnszones/AAAA/Read | ' AAAA ' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi, TTL, Etiketler ve ETag 'in yanı sıra bir kayıt listesi içerir. |
> | Action | Microsoft. Network/dnszones/AAAA/Write | Bir DNS bölgesi içinde ' AAAA ' türünde bir kayıt kümesi oluşturun veya güncelleştirin. Belirtilen kayıtlar, kayıt kümesindeki geçerli kayıtların yerini alır. |
> | Action | Microsoft. Network/dnszones/All/Read | Türler genelinde DNS kayıt kümelerini alır |
> | Action | Microsoft. Network/dnszones/CAA/Delete | Verilen bir adın kayıt kümesini kaldırın ve bir DNS bölgesinden ' CAA ' yazın. |
> | Action | Microsoft. Network/dnszones/CAA/Read | ' CAA ' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi TTL, Etiketler ve ETag değerini içerir. |
> | Action | Microsoft. Network/dnszones/CAA/Write | Bir DNS bölgesi içinde ' CAA ' türünde bir kayıt kümesi oluşturun veya güncelleştirin. Belirtilen kayıtlar, kayıt kümesindeki geçerli kayıtların yerini alır. |
> | Action | Microsoft. Network/dnszones/CNAME/Delete | Verilen bir adın kayıt kümesini ve ' CNAME ' türünü bir DNS bölgesinden kaldırın. |
> | Action | Microsoft. Network/dnszones/CNAME/Read | ' CNAME ' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi TTL, Etiketler ve ETag değerini içerir. |
> | Action | Microsoft. Network/dnszones/CNAME/Write | Bir DNS bölgesi içinde ' CNAME ' türünde bir kayıt kümesi oluşturun veya güncelleştirin. Belirtilen kayıtlar, kayıt kümesindeki geçerli kayıtların yerini alır. |
> | Action | Microsoft. Network/dnszones/Delete | DNS bölgesini JSON biçiminde silin. Bölge özellikleri, Etiketler, ETag, Numberofkayıtkümeleri ve Maxnumberofkayıtkümelerdir. |
> | Action | Microsoft. Network/dnszones/MX/Delete | Verilen bir adın kayıt kümesini ve ' MX ' türünü bir DNS bölgesinden kaldırın. |
> | Action | Microsoft. Network/dnszones/MX/Read | ' MX ' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi, TTL, Etiketler ve ETag 'in yanı sıra bir kayıt listesi içerir. |
> | Action | Microsoft. Network/dnszones/MX/Write | Bir DNS bölgesi içinde ' MX ' türünde bir kayıt kümesi oluşturun veya güncelleştirin. Belirtilen kayıtlar, kayıt kümesindeki geçerli kayıtların yerini alır. |
> | Action | Microsoft. Network/dnszones/NS/Delete | NS türündeki DNS kayıt kümesini siler |
> | Action | Microsoft. Network/dnszones/NS/Read | NS türündeki DNS kayıt kümesini alır |
> | Action | Microsoft. Network/dnszones/NS/Write | NS türünde DNS kayıt kümesi oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/dnszones/PTR/Delete | Verilen bir adın kayıt kümesini ve ' PTR ' türünü bir DNS bölgesinden kaldırın. |
> | Action | Microsoft. Network/dnszones/PTR/Read | ' PTR ' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi, TTL, Etiketler ve ETag 'in yanı sıra bir kayıt listesi içerir. |
> | Action | Microsoft. Network/dnszones/PTR/Write | Bir DNS bölgesi içinde ' PTR ' türünde bir kayıt kümesi oluşturun veya güncelleştirin. Belirtilen kayıtlar, kayıt kümesindeki geçerli kayıtların yerini alır. |
> | Action | Microsoft. Network/dnszones/Read | JSON biçiminde DNS bölgesini alın. Bölge özellikleri, Etiketler, ETag, Numberofkayıtkümeleri ve Maxnumberofkayıtkümelerdir. Bu komutun bölge içinde yer alan kayıt kümelerini almayacağını unutmayın. |
> | Action | Microsoft. Network/dnszones/kayıt kümeleri/okuma | Türler genelinde DNS kayıt kümelerini alır |
> | Action | Microsoft. Network/dnszones/SOA/Read | SOA türündeki DNS kayıt kümesini alır |
> | Action | Microsoft. Network/dnszones/SOA/Write | SOA türünde DNS kayıt kümesi oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/dnszones/SRV/Delete | Verilen bir adın kayıt kümesini ve ' SRV ' türünü bir DNS bölgesinden kaldırın. |
> | Action | Microsoft. Network/dnszones/SRV/Read | ' SRV ' türündeki kayıt kümesini JSON biçiminde alın. Kayıt kümesi, TTL, Etiketler ve ETag 'in yanı sıra bir kayıt listesi içerir. |
> | Action | Microsoft. Network/dnszones/SRV/Write | SRV türü kayıt kümesi oluştur veya güncelleştir |
> | Action | Microsoft. Network/dnszones/TXT/Delete | Verilen ad ve ' TXT ' türünün kayıt kümesini bir DNS bölgesinden kaldırın. |
> | Action | Microsoft. Network/dnszones/TXT/Read | JSON biçiminde ' TXT ' türünün kayıt kümesini alın. Kayıt kümesi, TTL, Etiketler ve ETag 'in yanı sıra bir kayıt listesi içerir. |
> | Action | Microsoft. Network/dnszones/TXT/Write | Bir DNS bölgesi içinde ' TXT ' türünde bir kayıt kümesi oluşturun veya güncelleştirin. Belirtilen kayıtlar, kayıt kümesindeki geçerli kayıtların yerini alır. |
> | Action | Microsoft. Network/dnszones/Write | Bir kaynak grubu içinde bir DNS bölgesi oluşturun veya güncelleştirin.  Bir DNS bölgesi kaynağındaki etiketleri güncelleştirmek için kullanılır. Bu komutun bölge içindeki kayıt kümelerini oluşturmak veya güncelleştirmek için kullanılamayacağını unutmayın. |
> | Action | Microsoft. Network/Expressroutedevreleri/yetkilendirmeler/silme | Bir Expressroutedevyetkilendirmesini siler |
> | Action | Microsoft. Network/Expressroutedevreleri/yetkilendirmeler/okuma | Bir Expressroutedevresi yetkilendirmesi alır |
> | Action | Microsoft. Network/Expressroutedevreleri/yetkilendirmeler/yazma | Mevcut bir Expressroutedevresi yetkilendirmesi oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/Expressroutedevreleri/Delete | Bir Expressroutedevresini siler |
> | Action | Microsoft. Network/Expressroutedevreleri/JOIN/Action | Bir hızlı rota devresini birleştirir. Alertable değil. |
> | Action | Microsoft. Network/Expressroutedevreleri/peerler/arpTables/Read | Expressroutedevresi eşlemesi ArpTable alır |
> | Action | Microsoft. Network/Expressroutedevreleri/eşlemeleri/bağlantıları/silme | Expressroutedevconnection 'ı siler |
> | Action | Microsoft. Network/Expressroutedevreler/eşlemeler/bağlantılar/okuma | Bir Expressroutedevconnection alır |
> | Action | Microsoft. Network/Expressroutedevreler/eşlemeler/bağlantılar/yazma | Mevcut bir Expressroutedevconnection kaynağını oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/Expressroutedevreleri/eşlemeleri/silme | Bir Expressroutedeveşliğini siler |
> | Action | Microsoft. Network/Expressroutedevreleri/peerler/peerConnections/Read | Eş Express Route bağlantı hattı bağlantısını alır |
> | Action | Microsoft. Network/Expressroutedevreler/peerler/okuma | Expressroutedeveşliğini alır |
> | Action | Microsoft. Network/Expressroutedevreleri/peerler/routeTables/Read | Bir Expressroutedevresi eşleme RouteTable alır |
> | Action | Microsoft. Network/Expressroutedevreleri/peerler/routeTablesSummary/Read | Bir Expressroutedevresi eşlemesi RouteTable özetini alır |
> | Action | Microsoft. Network/Expressroutedevreler/eşlemeler/istatistikler/okuma | Expressroutedevresi eşleme stat alır |
> | Action | Microsoft. Network/Expressroutedevreleri/eşlemeleri/yazma | Mevcut bir Expressroutedeveşliğini oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/Expressroutedevreler/okuma | Expressroutedevresi al |
> | Action | Microsoft. Network/Expressroutedevreleri/stats/Read | Expressroutedevstat alır |
> | Action | Microsoft. Network/Expressroutedevreleri/Write | Mevcut bir Expressroutedevresini oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/expressRouteCrossConnections/JOIN/Action | Bir Express Route çapraz bağlantısını birleştirir. Alertable değil. |
> | Action | Microsoft. Network/expressRouteCrossConnections/peerler/arpTables/Read | Bir Express Route çapraz bağlantı eşleme ARP tablosu alır |
> | Action | Microsoft. Network/expressRouteCrossConnections/peerler/Sil | Bir Express Route çapraz bağlantı eşlemesini siler |
> | Action | Microsoft. Network/expressRouteCrossConnections/peerler/okundu | Bir Express Route çapraz bağlantı eşlemesi alır |
> | Action | Microsoft. Network/expressRouteCrossConnections/peerler/routeTables/Read | Bir Express Route çapraz bağlantı eşleme yolu tablosu alır |
> | Action | Microsoft. Network/expressRouteCrossConnections/peerler/routeTableSummary/Read | Bir Express Route çapraz bağlantı eşleme yolu tablosu özetini alır |
> | Action | Microsoft. Network/expressRouteCrossConnections/peerler/yaz | Bir Express Route çapraz bağlantı eşlemesi oluşturur veya var olan bir Express Route çapraz bağlantı eşlemesini güncelleştirir |
> | Action | Microsoft. Network/expressRouteCrossConnections/Read | Express Route çapraz bağlantı al |
> | Action | Microsoft. Network/Expressroutegateway/expressRouteConnections/Delete | Bir Express Route bağlantısını siler |
> | Action | Microsoft. Network/Expressroutegateway/expressRouteConnections/Read | Bir Express Route bağlantısı alır |
> | Action | Microsoft. Network/Expressroutegateway/expressRouteConnections/Write | Bir Express Route bağlantısı oluşturur veya var olan bir Express Route bağlantısını güncelleştirir |
> | Action | Microsoft. Network/Expressroutegateway/JOIN/Action | Bir Express Route ağ geçidini birleştirir. Alertable değil. |
> | Action | Microsoft. Network/Expressroutegateway/Read | Express Route ağ geçidini al |
> | Action | Microsoft. Network/expressRoutePorts/Delete | ExpressRoutePorts siler |
> | Action | Microsoft. Network/expressRoutePorts/JOIN/Action | Express Route bağlantı noktalarını birleştirir. Alertable değil. |
> | Action | Microsoft. Network/expressRoutePorts/bağlantılar/okuma | ExpressRouteLink alır |
> | Action | Microsoft. Network/expressRoutePorts/Read | ExpressRoutePorts alır |
> | Action | Microsoft. Network/expressRoutePorts/Write | Expressrouteport oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/expressRoutePortsLocations/Read | Hızlı rota bağlantı noktaları konumlarını al |
> | Action | Microsoft. Network/expressRouteServiceProviders/Read | Hızlı Yönlendirme Hizmeti sağlayıcılarını alır |
> | Action | Microsoft. Network/firewallPolicies/Delete | Bir güvenlik duvarı Ilkesini siler |
> | Action | Microsoft. Network/firewallPolicies/JOIN/Action | Bir güvenlik duvarı Ilkesini birleştirir. Alertable değil. |
> | Action | Microsoft. Network/firewallPolicies/Read | Bir güvenlik duvarı Ilkesi alır |
> | Action | Microsoft. Network/firewallPolicies/ruleGroups/Delete | Bir güvenlik duvarı Ilkesi kural grubunu siler |
> | Action | Microsoft. Network/firewallPolicies/ruleGroups/Read | Bir güvenlik duvarı Ilke kuralı grubunu alır |
> | Action | Microsoft. Network/firewallPolicies/ruleGroups/Write | Bir güvenlik duvarı Ilkesi kural grubu oluşturur veya var olan bir güvenlik duvarı Ilkesi kural grubunu güncelleştirir |
> | Action | Microsoft. Network/firewallPolicies/Write | Bir güvenlik duvarı Ilkesi oluşturur veya var olan bir güvenlik duvarı Ilkesini güncelleştirir |
> | Action | Microsoft. Network/Frontkapaklı/backendPools/Delete | Bir arka uç havuzunu siler |
> | Action | Microsoft. Network/Frontkapaklı/backendPools/Read | Bir arka uç havuzu alır |
> | Action | Microsoft. Network/Frontkapaklı/backendPools/Write | Bir arka uç havuzu oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/Frontkapaklı/Delete | Ön kapıyı siler |
> | Action | Microsoft. Network/Frontkapılar/ön uçlar/silme | Ön uç uç noktasını siler |
> | Action | Microsoft. Network/Frontkapaklı/frontendEndpoints/disableHttps/Action | Ön uç uç noktasında HTTPS 'yi devre dışı bırakır |
> | Action | Microsoft. Network/Frontkapaklı/frontendEndpoints/enableHttps/Action | Ön uç uç noktasında HTTPS 'yi etkinleştirilir |
> | Action | Microsoft. Network/Frontkapaklı/frontendEndpoints/Read | Ön uç uç noktası alır |
> | Action | Microsoft. Network/Frontkapıları/ön uçlar/yazındnoktaları/yazma | Ön uç uç noktası oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/Frontkapaklı/healthProbeSettings/Delete | Durum araştırma ayarlarını siler |
> | Action | Microsoft. Network/Frontkapaklı/healthProbeSettings/Read | Sistem durumu araştırma ayarlarını alır |
> | Action | Microsoft. Network/Frontkapaklı/healthProbeSettings/Write | Durum araştırma ayarlarını oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/Frontkapaklı/loadBalancingSettings/Delete | Yük Dengeleme ayarlarını oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/Frontkapaklı/loadBalancingSettings/Read | Yük Dengeleme ayarlarını alır |
> | Action | Microsoft. Network/Frontkapaklı/loadBalancingSettings/Write | Yük Dengeleme ayarlarını oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/Frontkapaklı/Temizleme/eylem | Ön kapıdan önbelleğe alınmış içeriği Temizleme |
> | Action | Microsoft. Network/Frontkapaklı/Read | Ön kapıyı alır |
> | Action | Microsoft. Network/Frontkapaklı/routingRules/Delete | Bir yönlendirme kuralını siler |
> | Action | Microsoft. Network/Frontkapaklı/routingRules/Read | Bir yönlendirme kuralı alır |
> | Action | Microsoft. Network/Frontkapaklı/routingRules/Write | Yönlendirme kuralı oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/Frontkapaklı/validateCustomDomain/Action | Ön kapı için bir ön uç uç noktasını doğrular |
> | Action | Microsoft. Network/Frontkapaklı/Write | Ön kapı oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/frontDoorWebApplicationFirewallManagedRuleSets/Read | Web uygulaması güvenlik duvarı yönetilen kural kümelerini alır |
> | Action | Microsoft. Network/frontDoorWebApplicationFirewallPolicies/Delete | Web uygulaması güvenlik duvarı Ilkesini siler |
> | Action | Microsoft. Network/frontDoorWebApplicationFirewallPolicies/Read | Web uygulaması güvenlik duvarı Ilkesini alır |
> | Action | Microsoft. Network/frontDoorWebApplicationFirewallPolicies/Write | Bir Web uygulaması güvenlik duvarı Ilkesi oluşturur veya güncelleştirir |
> | Action | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Yük dengeleyici arka uç adres havuzunu birleştirir. Alertable değil. |
> | Action | Microsoft. Network/loadBalancers/Backendadddresspospool/Read | Yük dengeleyici arka uç adres havuzu tanımını alır |
> | Action | Microsoft. Network/loadBalancers/Delete | Yük dengeleyiciyi siler |
> | Action | Microsoft. Network/loadBalancers/Frontendıpconfigurations/JOIN/Action | Load Balancer bir ön uç IP yapılandırmasına katılır. Alertable değil. |
> | Action | Microsoft. Network/loadBalancers/Frontendıpconfigurations/Read | Yük dengeleyici ön uç IP yapılandırma tanımını alır |
> | Action | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Yük dengeleyici gelen NAT havuzunu birleştirir. Alertable değil. |
> | Action | Microsoft. Network/loadBalancers/ınboundnatpools/Read | Yük dengeleyici gelen NAT havuzu tanımını alır |
> | Action | Microsoft. Network/loadBalancers/ınboundnatrules/Delete | Yük dengeleyici gelen NAT kuralını siler |
> | Action | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Yük dengeleyici gelen NAT kuralına katılır. Alertable değil. |
> | Action | Microsoft. Network/loadBalancers/ınboundnatrules/Read | Yük dengeleyici gelen NAT kuralı tanımını alır |
> | Action | Microsoft. Network/loadBalancers/ınboundnatrules/Write | Yük dengeleyici gelen NAT kuralı oluşturur veya var olan yük dengeleyici gelen NAT kuralını güncelleştirir |
> | Action | Microsoft. Network/loadBalancers/loadBalancingRules/Read | Yük dengeleyici Yük Dengeleme kuralı tanımını alır |
> | Action | Microsoft. Network/loadBalancers/NetworkInterfaces/Read | Yük dengeleyici altındaki tüm ağ arabirimlerine başvuruları alır |
> | Action | Microsoft. Network/loadBalancers/outboundRules/Read | Yük dengeleyici giden kuralı tanımını alır |
> | Action | Microsoft. Network/loadBalancers/araştırmaları/JOIN/Action | Yük dengeleyicinin araştırmalarını sağlar. Örneğin, bu izinle VM Ölçek kümesinin Healtharaştırma özelliği araştırmasına başvurabilir. Alertable değil. |
> | Action | Microsoft. Network/loadBalancers/araştırmaları/Read | Yük dengeleyici araştırması alır |
> | Action | Microsoft. Network/loadBalancers/Read | Yük dengeleyici tanımını alır |
> | Action | Microsoft. Network/loadBalancers/virtualMachines/Read | Yük dengeleyici kapsamındaki tüm sanal makinelere başvuruları alır |
> | Action | Microsoft. Network/loadBalancers/Write | Yük dengeleyici oluşturur veya var olan bir yük dengeleyiciyi güncelleştirir |
> | Action | Microsoft. Network/localnetworkgateway/Delete | LocalNetworkGateway 'i siler |
> | Action | Microsoft. Network/localnetworkgateway/Read | LocalNetworkGateway alır |
> | Action | Microsoft. Network/localnetworkgateway/Write | Mevcut bir LocalNetworkGateway oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/Locations/oto Approlarprivatelinkservices/Read | Otomatik onaylanan özel bağlantı hizmetlerini alır |
> | Action | Microsoft. Network/Locations/Availabletemsilciler/okuma | Kullanılabilir temsilcileri alır |
> | Action | Microsoft. Network/Locations/availablePrivateEndpointTypes/Read | Kullanılabilir özel uç nokta kaynaklarını alır |
> | Action | Microsoft. Network/Locations/bareMetalTenants/Action | Çıplak kiracıyı ayırır veya doğrular |
> | Action | Microsoft. Network/Locations/Checkivme Networkingsupport/Action | Hızlandırılmış ağ desteğini denetler |
> | Action | Microsoft. Network/Locations/Checkdnsnameavaılabılıty/Read | Belirtilen konumda DNS etiketinin kullanılabilir olup olmadığını denetler |
> | Action | Microsoft. Network/Locations/checkPrivateLinkServiceVisibility/eylem | Özel bağlantı hizmeti görünürlüğünü denetler |
> | Action | Microsoft. Network/Locations/operationResults/Read | Zaman uyumsuz POST veya DELETE işleminin işlem sonucunu alır |
> | Action | Microsoft. Network/Locations/Operations/Read | Zaman uyumsuz bir işlemin durumunu temsil eden işlem kaynağını alır |
> | Action | Microsoft. Network/Locations/serviceTags/Read | Hizmet etiketlerini al |
> | Action | Microsoft. Network/Locations/Supportedvirtualınines/Read | Desteklenen sanal makine boyutlarını alır |
> | Action | Microsoft. Network/Locations/kullanımlar/Read | Kaynak kullanım ölçümlerini alır |
> | Action | Microsoft. Network/Locations/virtualNetworkAvailableEndpointServices/Read | Kullanılabilir sanal ağ uç noktası hizmetlerinin bir listesini alır |
> | Action | Microsoft. Network/Networkyoğunlutpolicies/Delete | Bir ağ hedefi Ilkesini siler |
> | Action | Microsoft. Network/Networkyoğunlutpolicies/okuma | Bir ağ hedefi Ilke açıklamasını alır |
> | Action | Microsoft. Network/Networkyoğunlutpolicies/Write | Bir ağ hedefi Ilkesi oluşturur veya var olan bir ağ hedefi Ilkesini güncelleştirir |
> | Action | Microsoft. Network/NetworkInterfaces/Delete | Bir ağ arabirimini siler |
> | Action | Microsoft. Network/NetworkInterfaces/effectiveNetworkSecurityGroups/Action | VM 'Nin ağ arabiriminde yapılandırılan ağ güvenlik gruplarını al |
> | Action | Microsoft. Network/NetworkInterfaces/effectiveRouteTable/Action | VM 'Nin ağ arabiriminde yapılandırılan yol tablosunu al |
> | Action | Microsoft. Network/NetworkInterfaces/ipconfigurations/JOIN/Action | Bir ağ arabirimi IP yapılandırmasına katılır. Alertable değil. |
> | Action | Microsoft. Network/NetworkInterfaces/ipconfigurations/Read | Bir ağ arabirimi IP yapılandırma tanımını alır.  |
> | Action | Microsoft.Network/networkInterfaces/join/action | Bir sanal makineyi bir ağ arabirimine birleştirir. Alertable değil. |
> | Action | Microsoft. Network/NetworkInterfaces/loadBalancers/Read | Ağ arabiriminin parçası olduğu tüm yük dengeleyicileri alır |
> | Action | Microsoft. Network/NetworkInterfaces/Read | Bir ağ arabirimi tanımını alır.  |
> | Action | Microsoft. Network/NetworkInterfaces/tapConfigurations/Delete | Bir ağ arabirimini siler yapılandırma ' ya dokunun. |
> | Action | Microsoft. Network/NetworkInterfaces/tapConfigurations/Read | Yapılandırma ' ya bir ağ arabirimi alır. |
> | Action | Microsoft. Network/NetworkInterfaces/tapConfigurations/Write | Yapılandırma ' ya bir ağ arabirimi oluşturur veya var olan bir ağ arabirimini güncelleştirir yapılandırma ' ya dokunun. |
> | Action | Microsoft.Network/networkInterfaces/write | Ağ arabirimi oluşturur veya var olan bir ağ arabirimini güncelleştirir.  |
> | Action | Microsoft. Network/networkProfiles/Delete | Bir ağ profilini siler |
> | Action | Microsoft. Network/networkProfiles/Read | Ağ profilini alır |
> | Action | Microsoft. Network/networkProfiles/removeContainers/Action | Kapsayıcıları kaldırır |
> | Action | Microsoft. Network/networkProfiles/setContainers/Action | Kapsayıcıları ayarlar |
> | Action | Microsoft. Network/networkProfiles/Setnetworkınterfaces/Action | Kapsayıcı ağ arabirimlerini ayarlar |
> | Action | Microsoft. Network/networkProfiles/Write | Bir ağ profili oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/networkSecurityGroups/defaultSecurityRules/Read | Varsayılan bir güvenlik kuralı tanımını alır |
> | Action | Microsoft. Network/networkSecurityGroups/Delete | Bir ağ güvenlik grubunu siler |
> | Action | Microsoft.Network/networkSecurityGroups/join/action | Bir ağ güvenlik grubuna katılır. Alertable değil. |
> | Action | Microsoft.Network/networkSecurityGroups/read | Bir ağ güvenlik grubu tanımını alır |
> | Action | Microsoft. Network/networkSecurityGroups/securityRules/Delete | Bir güvenlik kuralını siler |
> | Action | Microsoft. Network/networkSecurityGroups/securityRules/Read | Bir güvenlik kuralı tanımını alır |
> | Action | Microsoft. Network/networkSecurityGroups/securityRules/Write | Bir güvenlik kuralı oluşturur veya var olan bir güvenlik kuralını güncelleştirir |
> | Action | Microsoft. Network/networkSecurityGroups/Write | Bir ağ güvenlik grubu oluşturur veya var olan bir ağ güvenlik grubunu güncelleştirir |
> | Action | Microsoft. Network/networkWatchers/availableProvidersList/Action | Belirtilen Azure bölgesi için kullanılabilir tüm internet hizmeti sağlayıcılarını döndürür. |
> | Action | Microsoft. Network/networkWatchers/azureReachabilityReport/Action | Belirli bir konumdan Azure bölgelerine Internet hizmet sağlayıcılarının göreli gecikme puanı döndürür. |
> | Action | Microsoft. Network/networkWatchers/Configuyeniden Akıtılme günlüğü/eylemi | Bir hedef kaynak için akış günlüğünü yapılandırır. |
> | Action | Microsoft. Network/networkWatchers/Connectionizleyicileri/silme | Bir bağlantı Izleyicisini siler |
> | Action | Microsoft. Network/networkWatchers/Connectionizleyicileri/sorgu/eylem | Belirtilen uç noktalar arasındaki sorgu izleme bağlantısı |
> | Action | Microsoft. Network/networkWatchers/Connectionmonitörleri/okuma | Bağlantı Izleyicisi ayrıntılarını al |
> | Action | Microsoft. Network/networkWatchers/Connectionizleyicileri/Başlat/eylem | Belirtilen uç noktalar arasındaki bağlantıyı izlemeye başla |
> | Action | Microsoft. Network/networkWatchers/Connectionizleyicileri/durdur/eylem | Belirtilen uç noktalar arasındaki izleme bağlantısını Durdur/Duraklat |
> | Action | Microsoft. Network/networkWatchers/Connectionmonitörleri/yazma | Bağlantı Izleyicisi oluşturur |
> | Action | Microsoft. Network/networkWatchers/connectivityCheck/Action | Bir sanal makineden, başka bir VM veya rastgele uzak sunucu dahil olmak üzere belirli bir uç noktasına doğrudan TCP bağlantısı kurma olasılığını doğrular. |
> | Action | Microsoft. Network/networkWatchers/Delete | Ağ İzleyicisi siler |
> | Action | Microsoft. Network/networkWatchers/ipFlowVerify/Action | Paketin belirli bir hedefe mi izin verileceğini yoksa reddetme mi olduğunu döndürür. |
> | Action | Microsoft. Network/networkWatchers/lenses/Delete | Bir lens siler |
> | Action | Microsoft. Network/networkWatchers/lenses/sorgu/eylem | Belirtilen bir uç noktada ağ trafiğini izleme sorgusu |
> | Action | Microsoft. Network/networkWatchers/lenses/okuma | Lens ayrıntılarını al |
> | Action | Microsoft. Network/networkWatchers/lenses/başlangıç/eylem | Belirtilen bir uç noktada ağ trafiğini izlemeye başla |
> | Action | Microsoft. Network/networkWatchers/lenses/durdur/eylem | Belirtilen bir uç noktada ağ trafiğini izlemeyi Durdur/Duraklat |
> | Action | Microsoft. Network/networkWatchers/lenses/yazma | Bir lens oluşturur |
> | Action | Microsoft. Network/networkWatchers/networkConfigurationDiagnostic/Action | Ağ yapılandırması tanılaması. |
> | Action | Microsoft. Network/networkWatchers/Sonrakii/eylem | Belirtilen hedef ve hedef IP adresi için sonraki atlama türünü ve sonraki Umarım IP adresini döndürün. |
> | Action | Microsoft. Network/networkWatchers/Packetyakalamaları/silme | Bir paket yakalamayı siler |
> | Action | Microsoft. Network/networkWatchers/Packetyakalamaları/queryStatus/Action | Bir paket yakalama kaynağının özellikleri ve durumu hakkında bilgi alır. |
> | Action | Microsoft. Network/networkWatchers/Packetyakalamaları/Read | Paket yakalama tanımını alma |
> | Action | Microsoft. Network/networkWatchers/Packetyakalamaları/durdur/eylem | Çalışan paket yakalama oturumunu durdurun. |
> | Action | Microsoft. Network/networkWatchers/Packetyakalamaları/yazma | Paket yakalama oluşturur |
> | Action | Microsoft. Network/networkWatchers/Pingkafesler/Sil | Pinga kafesi siler |
> | Action | Microsoft. Network/networkWatchers/Pingkafesler/okuma | Pingkafes ayrıntılarını al |
> | Action | Microsoft. Network/networkWatchers/Pingkafes/start/Action | Belirtilen VM 'Ler arasında Pingkafes'i Başlat |
> | Action | Microsoft. Network/networkWatchers/Pingkafesler/durdur/eylem | Belirtilen VM 'Ler arasında Pingkafesi durdur |
> | Action | Microsoft. Network/networkWatchers/Pingkafesler/yaz | Pingkafes oluşturur |
> | Action | Microsoft. Network/networkWatchers/Queryconnectionizleyicileri/eylemi | Belirtilen uç noktalar arasındaki bağlantıyı izleyen toplu iş sorgusu |
> | Action | Microsoft. Network/networkWatchers/queryFlowLogStatus/Action | Bir kaynaktaki akış günlüğünün durumunu alır. |
> | Action | Microsoft. Network/networkWatchers/Querybir TResult/Action | Daha önce çalıştırılan veya çalışmakta olan sorun giderme işleminin sorun giderme sonucunu alır. |
> | Action | Microsoft. Network/networkWatchers/Read | Ağ İzleyicisi tanımını al |
> | Action | Microsoft. Network/networkWatchers/securityGroupView/Action | Bir VM 'ye uygulanan yapılandırılmış ve etkin ağ güvenlik grubu kurallarını görüntüleyin. |
> | Action | Microsoft. Network/networkWatchers/topoloji/eylem | Kaynak grubundaki kaynakların ve bunların ilişkilerinin ağ düzeyi görünümünü alır. |
> | Action | Microsoft. Network/networkWatchers/sorun giderme/eylem | Azure 'daki bir ağ kaynağında sorun gidermeye başlar. |
> | Action | Microsoft. Network/networkWatchers/Write | Bir ağ izleyicisi oluşturur veya var olan bir ağ izleyicisini güncelleştirir |
> | Action | Microsoft. Network/Operations/Read | Kullanılabilir Işlemleri al |
> | Action | Microsoft. Network/p2sVpnGateways/Delete | Bir P2SVpnGateway siler. |
> | Action | Microsoft. Network/p2sVpnGateways/generatevpnprofile/Action | P2SVpnGateway için VPN profili oluşturma |
> | Action | Microsoft. Network/p2sVpnGateways/getp2svpnconnectionhealth/Action | P2SVpnGateway için P2S VPN bağlantısı durumunu alır |
> | Action | Microsoft. Network/p2sVpnGateways/getp2svpnconnectionhealthdetailed/Action | P2SVpnGateway için ayrıntılı bir P2S VPN bağlantısı durumunu alır |
> | Action | Microsoft. Network/p2sVpnGateways/Read | Bir P2SVpnGateway alır. |
> | Action | Microsoft. Network/p2sVpnGateways/Write | Bir P2SVpnGateway koyar. |
> | Action | Microsoft. Network/privateDnsOperationResults/Read | Özel DNS işleminin sonuçlarını alır |
> | Action | Microsoft. Network/privateDnsOperationStatuses/Read | Özel DNS işleminin durumunu alır |
> | Action | Microsoft. Network/privateDnsZones/A/Delete | Verilen bir ada sahip kayıt kümesini kaldırın ve bir Özel DNS bölgesinden ' A ' yazın. |
> | Action | Microsoft. Network/privateDnsZones/A/Read | ' A ' türündeki kayıt kümesini JSON biçiminde bir Özel DNS bölgesi içinde alın. Kayıt kümesi, TTL, Etiketler ve ETag 'in yanı sıra bir kayıt listesi içerir. |
> | Action | Microsoft. Network/privateDnsZones/A/Write | Özel DNS bölgesi içinde ' A ' türünde bir kayıt kümesi oluşturun veya güncelleştirin. Belirtilen kayıtlar, kayıt kümesindeki geçerli kayıtların yerini alır. |
> | Action | Microsoft. Network/privateDnsZones/AAAA/Delete | Verilen bir adın kayıt kümesini ve bir Özel DNS bölgesinden ' AAAA ' türünü kaldırın. |
> | Action | Microsoft. Network/privateDnsZones/AAAA/Read | ' AAAA ' türündeki kayıt kümesini JSON biçiminde bir Özel DNS bölgesi içinde alın. Kayıt kümesi, TTL, Etiketler ve ETag 'in yanı sıra bir kayıt listesi içerir. |
> | Action | Microsoft. Network/privateDnsZones/AAAA/Write | Bir Özel DNS bölgesi içinde ' AAAA ' türünde bir kayıt kümesi oluşturun veya güncelleştirin. Belirtilen kayıtlar, kayıt kümesindeki geçerli kayıtların yerini alır. |
> | Action | Microsoft. Network/privateDnsZones/ALL/Read | Türler arasında Özel DNS kayıt kümelerini alır |
> | Action | Microsoft. Network/privateDnsZones/CNAME/Delete | Verilen bir adın kayıt kümesini ve bir Özel DNS bölgesinden ' CNAME ' türünü kaldırın. |
> | Action | Microsoft. Network/privateDnsZones/CNAME/Read | JSON biçiminde bir Özel DNS bölgesi içindeki ' CNAME ' türündeki kayıt kümesini alın. |
> | Action | Microsoft. Network/privateDnsZones/CNAME/Write | Bir Özel DNS bölgesi içinde ' CNAME ' türünde bir kayıt kümesi oluşturun veya güncelleştirin. |
> | Action | Microsoft. Network/privateDnsZones/Delete | Özel DNS bölgesini silin. |
> | Action | Microsoft. Network/privateDnsZones/MX/Delete | Verilen bir adın kayıt kümesini ve bir Özel DNS bölgesinden ' MX ' türünü kaldırın. |
> | Action | Microsoft. Network/privateDnsZones/MX/Read | JSON biçiminde bir Özel DNS bölgesi içindeki ' MX ' türündeki kayıt kümesini alın. Kayıt kümesi, TTL, Etiketler ve ETag 'in yanı sıra bir kayıt listesi içerir. |
> | Action | Microsoft. Network/privateDnsZones/MX/Write | Bir Özel DNS bölgesi içinde ' MX ' türünde bir kayıt kümesi oluşturun veya güncelleştirin. Belirtilen kayıtlar, kayıt kümesindeki geçerli kayıtların yerini alır. |
> | Action | Microsoft. Network/privateDnsZones/PTR/Delete | Verilen bir adın kayıt kümesini ve bir Özel DNS bölgesinden ' PTR ' türünü kaldırın. |
> | Action | Microsoft. Network/privateDnsZones/PTR/Read | ' PTR ' türünün kayıt kümesini JSON biçiminde bir Özel DNS bölgesi içinde alın. Kayıt kümesi, TTL, Etiketler ve ETag 'in yanı sıra bir kayıt listesi içerir. |
> | Action | Microsoft. Network/privateDnsZones/PTR/Write | Bir Özel DNS bölgesi içinde ' PTR ' türünde bir kayıt kümesi oluşturun veya güncelleştirin. Belirtilen kayıtlar, kayıt kümesindeki geçerli kayıtların yerini alır. |
> | Action | Microsoft. Network/privateDnsZones/Read | Özel DNS bölgesi özelliklerini JSON biçiminde alın. Bu komutun Özel DNS bölgenin bağlı olduğu sanal ağları veya bölgedeki kayıt kümelerini almadığına unutmayın. |
> | Action | Microsoft. Network/privateDnsZones/kayıt kümeleri/okuma | Türler arasında Özel DNS kayıt kümelerini alır |
> | Action | Microsoft. Network/privateDnsZones/SOA/Read | ' SOA ' türündeki kayıt kümesini JSON biçiminde bir Özel DNS bölgesi içinde alın. |
> | Action | Microsoft. Network/privateDnsZones/SOA/Write | Özel DNS bölgesinde ' SOA ' türündeki bir kayıt kümesini güncelleştirin. |
> | Action | Microsoft. Network/privateDnsZones/SRV/Delete | Verilen bir adın kayıt kümesini ve bir Özel DNS bölgesinden ' SRV ' türünü kaldırın. |
> | Action | Microsoft. Network/privateDnsZones/SRV/Read | ' SRV ' türündeki kayıt kümesini JSON biçiminde bir Özel DNS bölgesi içinde alın. Kayıt kümesi, TTL, Etiketler ve ETag 'in yanı sıra bir kayıt listesi içerir. |
> | Action | Microsoft. Network/privateDnsZones/SRV/Write | Bir Özel DNS bölgesi içinde ' SRV ' türünde bir kayıt kümesi oluşturun veya güncelleştirin. Belirtilen kayıtlar, kayıt kümesindeki geçerli kayıtların yerini alır. |
> | Action | Microsoft. Network/privateDnsZones/TXT/Delete | Verilen bir adın kayıt kümesini ve bir Özel DNS bölgesinden ' TXT ' türünü kaldırın. |
> | Action | Microsoft. Network/privateDnsZones/TXT/Read | ' TXT ' türündeki kayıt kümesini JSON biçiminde bir Özel DNS bölgesi içinde alın. Kayıt kümesi, TTL, Etiketler ve ETag 'in yanı sıra bir kayıt listesi içerir. |
> | Action | Microsoft. Network/privateDnsZones/TXT/Write | Bir Özel DNS bölgesi içinde ' TXT ' türünde bir kayıt kümesi oluşturun veya güncelleştirin. Belirtilen kayıtlar, kayıt kümesindeki geçerli kayıtların yerini alır. |
> | Action | Microsoft. Network/privateDnsZones/virtualNetworkLinks/Delete | Sanal ağa Özel DNS bölgenin bağlantısını silin. |
> | Action | Microsoft. Network/privateDnsZones/virtualNetworkLinks/Read | JSON biçiminde sanal ağ özelliklerine Özel DNS bölgenin bağlantısını alın. |
> | Action | Microsoft. Network/privateDnsZones/virtualNetworkLinks/Write | Sanal ağa bir Özel DNS bölgesi bağlantısı oluşturun veya güncelleştirin. |
> | Action | Microsoft. Network/privateDnsZones/Write | Bir kaynak grubu içinde Özel DNS bölgesi oluşturun veya güncelleştirin. Bu komutun bölge içindeki sanal ağ bağlantılarını veya kayıt kümelerini oluşturmak veya güncelleştirmek için kullanılamayacağını unutmayın. |
> | Action | Microsoft. Network/privateEndpointRedirectMaps/Read | Özel bir uç nokta RedirectMap alır |
> | Action | Microsoft. Network/privateEndpointRedirectMaps/Write | Özel uç nokta RedirectMap oluşturur veya var olan bir özel uç nokta RedirectMap 'i güncelleştirir |
> | Action | Microsoft. Network/privateEndpoints/Delete | Özel bir uç nokta kaynağını siler. |
> | Action | Microsoft. Network/privateEndpoints/okuma | Özel bir uç nokta kaynağı alır. |
> | Action | Microsoft. Network/privateEndpoints/yazma | Yeni bir özel uç nokta oluşturur veya var olan bir özel uç noktayı güncelleştirir. |
> | Action | Microsoft. Network/privateLinkServices/Delete | Özel bir bağlantı hizmeti kaynağını siler. |
> | Action | Microsoft. Network/privateLinkServices/privateEndpointConnections/Delete | Özel bir uç nokta bağlantısını siler. |
> | Action | Microsoft. Network/privateLinkServices/privateEndpointConnections/Read | Özel bir uç nokta bağlantı tanımını alır. |
> | Action | Microsoft. Network/privateLinkServices/privateEndpointConnections/Write | Yeni bir özel uç nokta bağlantısı oluşturur veya var olan bir özel uç nokta bağlantısını güncelleştirir. |
> | Action | Microsoft. Network/privateLinkServices/Read | Özel bir bağlantı hizmeti kaynağı alır. |
> | Action | Microsoft. Network/privateLinkServices/Write | Yeni bir özel bağlantı hizmeti oluşturur veya var olan bir özel bağlantı hizmetini güncelleştirir. |
> | Action | Microsoft. Network/publicIPAddresses/Delete | Genel bir IP adresini siler. |
> | Action | Microsoft.Network/publicIPAddresses/join/action | Genel bir IP adresini birleştirir. Alertable değil. |
> | Action | Microsoft.Network/publicIPAddresses/read | Genel IP adresi tanımını alır. |
> | Action | Microsoft. Network/Publicıpaddresses/Write | Genel bir IP adresi oluşturur veya var olan bir genel IP adresini güncelleştirir.  |
> | Action | Microsoft. Network/Publicipöneklerini/silmeyi | Genel bir IP önekini siler |
> | Action | Microsoft. Network/Publicipöneklerini/JOIN/Action | Bir PublicIPPrefix öğesine katılır. Alertable değil. |
> | Action | Microsoft. Network/Publicipönekleri/okuma | Genel IP öneki tanımını alır |
> | Action | Microsoft. Network/Publicipöneklerini/yazma | Genel bir IP öneki oluşturur veya var olan bir genel IP önekini güncelleştirir |
> | Action | Microsoft. Network/Register/ACTION | Aboneliği kaydeder |
> | Action | Microsoft. Network/routeFilters/Delete | Bir rota filtresi tanımını siler |
> | Action | Microsoft. Network/routeFilters/JOIN/Action | Bir yol filtresi birleştirir. Alertable değil. |
> | Action | Microsoft. Network/routeFilters/Read | Rota filtresi tanımını alır |
> | Action | Microsoft. Network/routeFilters/routeFilterRules/Delete | Yol filtresi kuralı tanımını siler |
> | Action | Microsoft. Network/routeFilters/routeFilterRules/Read | Yol filtresi kuralı tanımını alır |
> | Action | Microsoft. Network/routeFilters/routeFilterRules/Write | Bir yol filtresi kuralı oluşturur veya var olan bir yol filtre kuralını güncelleştirir |
> | Action | Microsoft. Network/routeFilters/Write | Bir yol filtresi oluşturur veya var olan bir yol filtresini güncelleştirir |
> | Action | Microsoft. Network/routeTables/Delete | Rota tablosu tanımını siler |
> | Action | Microsoft. Network/routeTables/JOIN/Action | Bir yol tablosunu birleştirir. Alertable değil. |
> | Action | Microsoft. Network/routeTables/Read | Yol tablosu tanımını alır |
> | Action | Microsoft. Network/routeTables/rotalar/Delete | Bir yol tanımını siler |
> | Action | Microsoft. Network/routeTables/rotalar/Read | Bir yol tanımını alır |
> | Action | Microsoft. Network/routeTables/rotalar/Write | Bir rota oluşturur veya var olan bir yolu güncelleştirir |
> | Action | Microsoft. Network/routeTables/Write | Bir yol tablosu oluşturur veya var olan bir yol tablosunu güncelleştirir |
> | Action | Microsoft. Network/securegateyollarla/silme | Güvenli ağ geçidini Sil |
> | Action | Microsoft. Network/securegateyollar/okuma | Güvenli ağ geçidi al |
> | Action | Microsoft. Network/securegateyollar/yazma | Güvenli ağ geçidi oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/serviceEndpointPolicies/Delete | Hizmet uç noktası Ilkesini siler |
> | Action | Microsoft. Network/serviceEndpointPolicies/JOIN/Action | Hizmet uç noktası Ilkesini birleştirir. Alertable değil. |
> | Action | Microsoft. Network/serviceEndpointPolicies/joinSubnet/Action | Bir alt ağı hizmet uç noktası Ilkelerine birleştirir. Alertable değil. |
> | Action | Microsoft. Network/serviceEndpointPolicies/okuma | Hizmet uç noktası Ilke açıklamasını alır |
> | Action | Microsoft. Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/Delete | Hizmet uç noktası Ilke tanımını siler |
> | Action | Microsoft. Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/Read | Bir hizmet uç noktası Ilke tanımı açıklaması Al |
> | Action | Microsoft. Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/Write | Bir hizmet uç noktası Ilke tanımı oluşturur veya var olan bir hizmet uç noktası Ilke tanımını güncelleştirir |
> | Action | Microsoft. Network/serviceEndpointPolicies/Write | Bir hizmet uç noktası Ilkesi oluşturur veya var olan bir hizmet uç noktası Ilkesini güncelleştirir |
> | Action | Microsoft. Network/trafficManagerGeographicHierarchies/Read | Coğrafi trafik yönlendirme yöntemiyle kullanılabilecek bölgeleri içeren Traffic Manager coğrafi hiyerarşisini alır |
> | Action | Microsoft. Network/trafficManagerProfiles/azureEndpoints/Delete | Varolan bir Traffic Manager profilinden bir Azure Uç Noktası siler. Traffic Manager, silinen Azure Uç Noktası yönlendirme trafiğini durdurur. |
> | Action | Microsoft. Network/trafficManagerProfiles/azureEndpoints/Read | Bu Azure Uç Noktası tüm özellikleri de dahil olmak üzere bir Traffic Manager profiline ait bir Azure Uç Noktası alır. |
> | Action | Microsoft. Network/trafficManagerProfiles/azureEndpoints/Write | Var olan bir Traffic Manager profiline yeni bir Azure Uç Noktası ekleyin veya bu Traffic Manager profilinde mevcut bir Azure Uç Noktası özelliklerini güncelleştirin. |
> | Action | Microsoft. Network/trafficManagerProfiles/Delete | Traffic Manager profilini silin. Traffic Manager profiliyle ilişkili tüm ayarlar kaybedilir ve bu profil artık trafiği yönlendirmek için kullanılamaz. |
> | Action | Microsoft. Network/trafficManagerProfiles/externalEndpoints/Delete | Varolan bir Traffic Manager profilinden bir dış uç noktayı siler. Traffic Manager, silinen dış uç noktaya yönlendirme trafiğini durdurur. |
> | Action | Microsoft. Network/trafficManagerProfiles/externalEndpoints/Read | Bu dış uç noktanın tüm özellikleri dahil olmak üzere Traffic Manager profile ait olan bir dış uç noktayı alır. |
> | Action | Microsoft. Network/trafficManagerProfiles/externalEndpoints/Write | Mevcut bir Traffic Manager profilinde yeni bir dış uç nokta ekleyin veya bu Traffic Manager profilindeki mevcut bir dış noktanın özelliklerini güncelleştirin. |
> | Action | Microsoft. Network/trafficManagerProfiles/heatMaps/Read | Konuma ve kaynak IP 'ye göre sorgu sayılarını ve gecikme verilerini içeren verilen Traffic Manager profili için Traffic Manager ısı haritasını alır. |
> | Action | Microsoft. Network/trafficManagerProfiles/nestedEndpoints/Delete | Varolan bir Traffic Manager profilinden Iç Içe geçmiş bir uç noktayı siler. Traffic Manager, silinen Iç Içe geçmiş uç noktaya yönlendirme trafiğini durdurur. |
> | Action | Microsoft. Network/trafficManagerProfiles/nestedEndpoints/Read | Bu Iç Içe geçmiş uç noktanın tüm özellikleri dahil olmak üzere bir Traffic Manager profile ait olan Iç Içe geçmiş bir uç noktası alır. |
> | Action | Microsoft. Network/trafficManagerProfiles/nestedEndpoints/Write | Mevcut bir Traffic Manager profiline yeni bir Iç Içe geçmiş uç noktası ekleyin veya bu Traffic Manager profilinde mevcut Iç Içe geçmiş bir uç noktanın özelliklerini güncelleştirin. |
> | Action | Microsoft. Network/trafficManagerProfiles/Read | Traffic Manager profili yapılandırmasını alın. Buna DNS ayarları, trafik yönlendirme ayarları, uç nokta izleme ayarları ve bu Traffic Manager profili tarafından yönlendirilen uç noktaların listesi dahildir. |
> | Action | Microsoft. Network/trafficManagerProfiles/Write | Traffic Manager profili oluşturun veya var olan bir Traffic Manager profilinin yapılandırmasını değiştirin.<br>Bu, bir profilin etkinleştirilmesini veya devre dışı bırakılmasını ve DNS ayarlarını, trafik yönlendirme ayarlarını veya uç nokta izleme ayarlarını değiştirmeyi içerir.<br>Traffic Manager profili tarafından yönlendirilen uç noktalar eklenebilir, kaldırılabilir, etkinleştirilebilir veya devre dışı bırakılabilir. |
> | Action | Microsoft. Network/trafficManagerUserMetricsKeys/Delete | Gerçek zamanlı Kullanıcı ölçümleri koleksiyonu için kullanılan abonelik düzeyi anahtarını siler. |
> | Action | Microsoft. Network/trafficManagerUserMetricsKeys/Read | Gerçek zamanlı Kullanıcı ölçümleri koleksiyonu için kullanılan abonelik düzeyi anahtarını alır. |
> | Action | Microsoft. Network/trafficManagerUserMetricsKeys/Write | Gerçek zamanlı Kullanıcı ölçümleri koleksiyonu için kullanılacak yeni bir abonelik düzeyi anahtar oluşturur. |
> | Action | Microsoft. Network/Unregister/ACTION | Aboneliğin kaydını siler |
> | Action | Microsoft. Network/Virtualhub 'Ları/silme | Sanal hub 'ı siler |
> | Action | Microsoft. Network/Virtualhub 'Ları/hubVirtualNetworkConnections/Delete | Bir HubVirtualNetworkConnection 'ı siler |
> | Action | Microsoft. Network/Virtualhub 'Ları/hubVirtualNetworkConnections/Read | HubVirtualNetworkConnection alın |
> | Action | Microsoft. Network/Virtualhub 'Ları/hubVirtualNetworkConnections/Write | HubVirtualNetworkConnection oluşturma veya güncelleştirme |
> | Action | Microsoft. Network/Virtualhub 'Ları/okuma | Sanal hub al |
> | Action | Microsoft. Network/Virtualhub 'Ları/routeTables/Delete | Bir VirtualHubRouteTableV2 silme |
> | Action | Microsoft. Network/Virtualhub 'Ları/routeTables/Read | VirtualHubRouteTableV2 edinme |
> | Action | Microsoft. Network/Virtualhub 'Ları/routeTables/Write | VirtualHubRouteTableV2 oluşturma veya güncelleştirme |
> | Action | Microsoft. Network/Virtualhub/Write | Sanal hub oluşturma veya güncelleştirme |
> | Action | Microsoft. Network/virtualnetworkgateway/Connections/Read | Get VirtualNetworkGatewayConnection |
> | Action | Microsoft. Network/Virtualnetworkgateway/Delete | VirtualNetworkGateway siler |
> | Action | Microsoft. Network/virtualnetworkgateway/generatevpnclientpackage/Action | VirtualNetworkGateway için VpnClient paketi oluşturma |
> | Action | Microsoft. Network/virtualnetworkgateway/generatevpnprofile/Action | VirtualNetworkGateway için VpnProfile paketi oluşturma |
> | Action | Microsoft. Network/virtualnetworkgateway/getadvertisedroutes/Action | Tanıtılan virtualNetworkGateway yollarını alır |
> | Action | Microsoft. Network/virtualnetworkgateway/getbgppeerstatus/Action | VirtualNetworkGateway BGP eş durumunu alır |
> | Action | Microsoft. Network/virtualnetworkgateway/getlearnedroutes/Action | Virtualnetworkgateway öğrenilen yollarını alır |
> | Action | Microsoft. Network/virtualnetworkgateway/getvpnclientconnectionhealth/Action | VirtualNetworkGateway için VPN Istemci bağlantısı başına durum Al |
> | Action | Microsoft. Network/virtualnetworkgateway/getvpnclientipsecparameters/eylem | VirtualNetworkGateway P2S istemcisi için vpnclient IPSec parametrelerini alın. |
> | Action | Microsoft. Network/virtualnetworkgateway/getvpnprofilepackageurl/eylem | Önceden oluşturulmuş bir VPN istemci profili paketinin URL 'sini alır |
> | Action | Microsoft. Network/Virtualnetworkgateway/Read | VirtualNetworkGateway alır |
> | Action | Microsoft. Network/virtualnetworkgateway/Reset/Action | VirtualNetworkGateway 'i sıfırlar |
> | Action | Microsoft. Network/virtualnetworkgateway/resetvpnclientsharedkey/Action | VirtualNetworkGateway P2S istemcisi için vpnclient paylaşılan anahtarını sıfırlayın. |
> | Action | Microsoft. Network/virtualnetworkgateway/setvpnclientipsecparameters/eylem | VirtualNetworkGateway P2S istemcisi için vpnclient IPSec parametrelerini ayarlayın. |
> | Action | Microsoft. Network/virtualnetworkgateway/supportedvpndevices/Action | Desteklenen VPN cihazlarını listeler |
> | Action | Microsoft. Network/Virtualnetworkgateway/Write | VirtualNetworkGateway oluşturur veya güncelleştirir |
> | Action | Microsoft. Network/virtualNetworks/Bastionkonakları/eylem | Sanal bir ağdaki savunma ana bilgisayar başvurularını alır. |
> | Action | Microsoft. Network/virtualNetworks/Bastionkonakları/varsayılan/eylem | Sanal bir ağdaki savunma ana bilgisayar başvurularını alır. |
> | Action | Microsoft. Network/virtualNetworks/Checkıpaddressavailability/Read | IP adresinin belirtilen sanal ağda kullanılabilir olup olmadığını denetle |
> | Action | Microsoft. Network/virtualNetworks/Delete | Bir sanal ağı siler |
> | Action | Microsoft. Network/virtualNetworks/JOIN/Action | Bir sanal ağı birleştirir. Alertable değil. |
> | Action | Microsoft. Network/virtualNetworks/eş/eylem | Bir sanal ağı başka bir sanal ağla eşler |
> | Action | Microsoft. Network/virtualNetworks/Read | Sanal ağ tanımını al |
> | Action | Microsoft. Network/virtualNetworks/alt ağlar/Sil | Bir sanal ağ alt ağını siler |
> | Action | Microsoft. Network/virtualNetworks/alt ağlar/JOIN/Action | Bir sanal ağı birleştirir. Alertable değil. |
> | Action | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Depolama hesabı veya SQL veritabanı gibi kaynağı bir alt ağa birleştirir. Alertable değil. |
> | Action | Microsoft. Network/virtualNetworks/alt ağlar/hazırlık Ilkeleri/eylemi | Gerekli ağ Ilkelerini uygulayarak bir alt ağ hazırlar |
> | Action | Microsoft. Network/virtualNetworks/alt ağlar/okuma | Bir sanal ağ alt ağ tanımını alır |
> | Action | Microsoft. Network/virtualNetworks/alt ağlar/hazırlık Ilkeleri/eylemi | Uygulanan ağ Ilkelerini kaldırarak bir alt ağın hazırlamasını kaldır |
> | Action | Microsoft. Network/virtualNetworks/alt ağlar/virtualMachines/Read | Bir sanal ağ alt ağındaki tüm sanal makinelere başvuruları alır |
> | Action | Microsoft. Network/virtualNetworks/alt ağlar/yazma | Bir sanal ağ alt ağı oluşturur veya var olan bir sanal ağ alt ağını güncelleştirir |
> | Action | Microsoft. Network/virtualNetworks/kullanımlar/Read | Sanal ağın her bir alt ağı için IP kullanımlarını al |
> | Action | Microsoft. Network/virtualNetworks/virtualMachines/Read | Bir sanal ağdaki tüm sanal makinelere başvuruları alır |
> | Action | Microsoft. Network/virtualNetworks/Virtualnetworkpeerler/Sil | Sanal Ağ eşlemesini siler |
> | Action | Microsoft. Network/virtualNetworks/Virtualnetworkpeerler/okuma | Sanal ağ eşleme tanımını alır |
> | Action | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write | Bir sanal ağ eşlemesi oluşturur veya var olan bir sanal ağ eşlemesini güncelleştirir |
> | Action | Microsoft. Network/virtualNetworks/Write | Bir sanal ağ oluşturur veya var olan bir sanal ağı güncelleştirir |
> | Action | Microsoft. Network/virtualNetworkTaps/Delete | Sanal ağ dokunmayı Sil |
> | Action | Microsoft. Network/virtualNetworkTaps/JOIN/Action | Bir sanal ağa dokunmasına katılır. Alertable değil. |
> | Action | Microsoft. Network/virtualNetworkTaps/Read | Sanal ağ dokunmayı al |
> | Action | Microsoft. Network/virtualNetworkTaps/Write | Sanal ağ dokunma oluştur veya güncelleştir |
> | Action | Microsoft. Network/Virtualyönlendiriciler/Delete | VirtualRouter siler |
> | Action | Microsoft. Network/Virtualyönlendiriciler/JOIN/Action | Bir VirtualRouter birleştirir. Alertable değil. |
> | Action | Microsoft. Network/Virtualyönlendiricilerle/okunan | Bir VirtualRouter alır |
> | Action | Microsoft. Network/Virtualyönlendiriciler/Virtualrouterpeerler/Sil | Virtualroutereşlemesindeki eşlemeyi siler |
> | Action | Microsoft. Network/Virtualyönlendiriciler/Virtualrouterpeerler/okundu | Virtualroutereşleme alır |
> | Action | Microsoft. Network/Virtualyönlendiriciler/Virtualrouterpeerler/yaz | Virtualroutereşleme oluşturur veya var olan Virtualroutereşlemesindeki bir eşlemeyi güncelleştirir |
> | Action | Microsoft. Network/Virtualyönlendiriciler/Write | Bir Virtualyönlendirici oluşturur veya var olan bir VirtualRouter 'ı güncelleştirir |
> | Action | Microsoft. Network/Virtualwan/Delete | Bir sanal WAN siler |
> | Action | Microsoft. Network/virtualwan/generateVpnProfile/Action | VirtualWanVpnServerConfiguration VpnProfile oluştur |
> | Action | Microsoft. Network/Virtualwan/Read | Sanal WAN alın |
> | Action | Microsoft. Network/virtualwan/supportedSecurityProviders/Read | Desteklenen VirtualWan güvenlik sağlayıcılarını alır. |
> | Action | Microsoft. Network/Virtualwan/Virtualhub/Read | Bir sanal WAN 'a başvuran tüm sanal hub 'Ları alır. |
> | Action | Microsoft. Network/virtualwan/vpnconfiguration/Action | VPN yapılandırmasını alır |
> | Action | Microsoft. Network/virtualwan/vpnServerConfigurations/Action | VirtualWanVpnServerConfigurations al |
> | Action | Microsoft. Network/Virtualwan/vpnSites/Read | Bir sanal WAN 'a başvuran tüm VPN sitelerini alır. |
> | Action | Microsoft. Network/Virtualwan/Write | Sanal WAN oluşturma veya güncelleştirme |
> | Action | Microsoft. Network/Vpngateway/Delete | Bir VpnGateway siler. |
> | Action | Microsoft. Network/vpngateway/listvpnconnectionshegizli/Action | Bir VpnGateway 'de bir bağlantı kümesinin tümü veya bir alt kümesi için bağlantı durumunu alır |
> | Action | Microsoft. Network/Vpngateway/Read | Bir VpnGateway alır. |
> | Action | Microsoft. Network/vpngateway/Reset/Action | VpnGateway 'i sıfırlar |
> | Action | Microsoft. Network/Vpngateway/vpnConnections/Delete | Bir VpnConnection 'ı siler. |
> | Action | Microsoft. Network/Vpngateway/vpnConnections/Read | Bir VpnConnection alır. |
> | Action | Microsoft. Network/Vpngateway/vpnConnections/vpnLinkConnections/Read | VPN bağlantısı bağlantısını alır |
> | Action | Microsoft. Network/Vpngateway/vpnConnections/Write | Bir VpnConnection koyar. |
> | Action | Microsoft. Network/Vpngateway/Write | Bir VpnGateway koyar. |
> | Action | Microsoft. Network/vpnServerConfigurations/Delete | VpnServerConfiguration silme |
> | Action | Microsoft. Network/vpnServerConfigurations/Read | VpnServerConfiguration al |
> | Action | Microsoft. Network/vpnServerConfigurations/Write | VpnServerConfiguration oluştur veya güncelleştir |
> | Action | Microsoft. Network/vpnsites/Delete | Bir VPN sitesi kaynağını siler. |
> | Action | Microsoft. Network/vpnsites/Read | Bir VPN sitesi kaynağı alır. |
> | Action | Microsoft. Network/vpnSites/vpnSiteLinks/Read | VPN sitesi bağlantısını alır |
> | Action | Microsoft. Network/vpnsites/Write | Bir VPN sitesi kaynağı oluşturur veya güncelleştirir. |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Notificationhub 'Lar/CheckNamespaceAvailability/Action | Belirtilen bir ad alanı kaynak adının NotificationHub hizmeti içinde kullanılabilir olup olmadığını denetler. |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/authorizationRules/eylem | Ad alanı yetkilendirme kuralları açıklaması listesini alın. |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/authorizationRules/silme | Ad alanı yetkilendirme kuralını silin. Varsayılan ad alanı yetkilendirme kuralı silinemez.  |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/authorizationRules/ListKeys/Action | Ad alanına bağlantı dizesi al |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/authorizationRules/okuma | Ad alanı yetkilendirme kuralları açıklaması listesini alın. |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/authorizationRules/regenerateKeys/ACTION | Ad alanı yetkilendirme kuralı birincil/Ikincil anahtarı yeniden oluştur, yeniden oluşturulması gereken anahtarı belirtin |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/authorizationRules/yazma | Ad alanı düzeyinde yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve Ikincil anahtarlar güncelleştirilebilen olabilir. |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/Checbcertificate sertifikası/eylem | Bir ad alanı içinde belirli bir NotificationHub adının kullanılabilir olup olmadığını denetler. |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/silme | Ad Alanı Kaynağını silin |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/authorizationRules/Action | Bildirim Hub 'ı yetkilendirme kurallarının listesini alın |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/authorizationRules/Delete | Bildirim Hub'ı Yetkilendirme Kurallarını Sil |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/authorizationRules/ListKeys/Action | Bağlantı dizesini Bildirim Hub 'ına al |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/authorizationRules/Read | Bildirim Hub 'ı yetkilendirme kurallarının listesini alın |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/authorizationRules/regenerateKeys/ACTION | Bildirim Hub 'ı yetkilendirme kuralı birincil/Ikincil anahtarı yeniden oluşturma, yeniden oluşturulması gereken anahtarı belirtin |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/authorizationRules/Write | Bildirim Hub 'ı yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve Ikincil anahtarlar güncelleştirilebilen olabilir. |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/debugSend/Action | Test anında iletme bildirimi gönderin. |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar/silme | Bildirim Hub'ı Kaynağını Sil |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar/metricDefinitions/okuma | Ad alanı ölçümleri kaynak açıklamalarının listesini al |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/pnsCredentials/Action | Tüm Notification Hub 'ı kimlik bilgilerini alın. Bu, WNS, MPNS, APNS, GCM ve Baidu kimlik bilgilerini içerir |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/okuma | Bildirim Hub 'ı kaynak açıklamalarının listesini al |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/yazma | Bir Bildirim Hub 'ı oluşturun ve özelliklerini güncelleştirin. Özellikleri genellikle PNS kimlik bilgilerini içerir. Yetkilendirme kuralları ve TTL |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/okuma | Ad alanı kaynağı açıklaması listesini al |
> | Action | Microsoft. Notificationhub 'Lar/ad alanları/yazma | Bir ad alanı kaynağı oluşturun ve özelliklerini güncelleştirin. Ad alanının etiketleri ve kapasitesi, güncelleştirilebilen özelliklerdir. |
> | Action | Microsoft. Notificationhub 'Lar/operationResults/Read | Notification Hubs sağlayıcısı için işlem sonuçlarını döndürür |
> | Action | Microsoft. Notificationhub 'Lar/işlemler/okuma | Notification Hubs sağlayıcısı için desteklenen işlemlerin listesini döndürür |
> | Action | Microsoft. Notificationhub/Register/ACTION | Notificationhub 'Lar kaynak sağlayıcısı için aboneliği kaydeder ve ad alanları ile Notificationhub 'ların oluşturulmasını sunar |
> | Action | Microsoft. Notificationhub 'Lar/kaydını kaldırma/eylem | Notificationhub 'Lar kaynak sağlayıcısı için aboneliğin kaydını siler ve ad alanları ile Notificationhub 'ların oluşturulmasını etkinleştirilir |

## <a name="microsoftoffazure"></a>Microsoft. OffAzure

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. OffAzure/HyperVSites/kümeler/okuma | Hyper-V kümesinin özelliklerini alır |
> | Action | Microsoft. OffAzure/HyperVSites/kümeler/Write | Hyper-V kümesi oluşturur veya güncelleştirir |
> | Action | Microsoft. OffAzure/HyperVSites/Delete | Hyper-V sitesini siler |
> | Action | Microsoft. OffAzure/HyperVSites/healthsummary/Read | Hyper-V kaynağı için sistem durumu özetini alır |
> | Action | Microsoft. OffAzure/HyperVSites/konaklar/Read | Hyper-V konağının özelliklerini alır |
> | Action | Microsoft. OffAzure/HyperVSites/konaklar/Write | Hyper-V konağını oluşturur veya güncelleştirir |
> | Action | Microsoft. OffAzure/HyperVSites/Jobs/Read | Hyper-V işlerinin özelliklerini alır |
> | Action | Microsoft. OffAzure/HyperVSites/makineler/okuma | Hyper-V makinelerinin özelliklerini alır |
> | Action | Microsoft. OffAzure/HyperVSites/operationsstatus/Read | Hyper-V işlem durumunun özelliklerini alır |
> | Action | Microsoft. OffAzure/HyperVSites/Read | Hyper-V sitesinin özelliklerini alır |
> | Action | Microsoft. OffAzure/hiper sanal siteler/Yenile/eylem | Hyper-V sitesi içindeki nesneleri yeniler |
> | Action | Microsoft. OffAzure/HyperVSites/runasaccounts/Read | Hyper-V Farklı Çalıştır hesaplarının özelliklerini alır |
> | Action | Microsoft. OffAzure/hiper sanal siteler/kullanım/okuma | Hyper-V sitesinin kullanımlarını alır |
> | Action | Microsoft. OffAzure/HyperVSites/Write | Hyper-V sitesini oluşturur veya güncelleştirir |
> | Action | Microsoft. OffAzure/Işlemler/okuma | Gösterilen işlemleri okur |
> | Action | Microsoft. OffAzure/Register/ACTION | Aboneliği Microsoft. OffAzure kaynak sağlayıcısına kaydeder |
> | Action | Microsoft. OffAzure/VMwareSites/Delete | VMware sitesini siler |
> | Action | Microsoft. OffAzure/VMwareSites/healthsummary/Read | VMware kaynağı için sistem durumu özetini alır |
> | Action | Microsoft. OffAzure/VMwareSites/Jobs/Read | Bir VMware işlerinin özelliklerini alır |
> | Action | Microsoft. OffAzure/VMwareSites/makineler/okuma | VMware makinelerinin özelliklerini alır |
> | Action | Microsoft. OffAzure/VMwareSites/makineler/Başlat/eylem | VMware makinelerini başlatma |
> | Action | Microsoft. OffAzure/VMwareSites/makineler/durdur/eylem | VMware makinelerini durduruyor |
> | Action | Microsoft. OffAzure/VMwareSites/operationsstatus/Read | Bir VMware işlem durumunun özelliklerini alır |
> | Action | Microsoft. OffAzure/VMwareSites/Read | Bir VMware sitesinin özelliklerini alır |
> | Action | Microsoft. OffAzure/VMwareSites/Refresh/Action | VMware sitesi içindeki nesneleri yeniler |
> | Action | Microsoft. OffAzure/VMwareSites/runasaccounts/Read | VMware Farklı Çalıştır hesaplarının özelliklerini alır |
> | Action | Microsoft. OffAzure/VMwareSites/kullanım/okuma | Bir VMware sitesinin kullanımlarını alır |
> | Action | Microsoft. OffAzure/VMwareSites/vcenters/Read | VMware vCenter özelliklerini alır |
> | Action | Microsoft. OffAzure/VMwareSites/vcenters/Write | VMware vCenter 'ı oluşturur veya güncelleştirir |
> | Action | Microsoft. OffAzure/VMwareSites/Write | VMware sitesini oluşturur veya güncelleştirir |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Operationalınsights/linkTargets/okuma | Bir Azure aboneliği ile ilişkilendirilmemiş mevcut hesapları listeler. Bu Azure aboneliğini bir çalışma alanına bağlamak için, çalışma alanı oluştur işleminin müşteri kimliği özelliği içinde bu işlem tarafından döndürülen bir müşteri kimliği kullanın. |
> | Action | Microsoft. operationalınsights/işlemler/okuma | Tüm kullanılabilir Operationalınsights REST API işlemlerini listeler. |
> | Action | Microsoft. operationalınsights/Register/ACTION | Aboneliği yeniden yönlendirme. |
> | Action | Microsoft. Operationalınsights/Register/ACTION | Bir kaynak sağlayıcısına abonelik kaydedin. |
> | Action | Microsoft. operationalınsights/kaydını kaldırma/eylem | Aboneliğin kaydını siler. |
> | Action | Microsoft.OperationalInsights/workspaces/analytics/query/action | Yeni altyapıyı kullanarak arama yapın. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/analiz/sorgu/şema/okuma | Arama şeması v2 'yi alın. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/API/sorgu/eylem | Yeni altyapıyı kullanarak arama yapın. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/API/sorgu/şema/okuma | Arama şeması v2 'yi alın. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/configurationScopes/Sil | Yapılandırma kapsamını Sil |
> | Action | Microsoft. Operationalınsights/çalışma alanları/configurationScopes/okuma | Yapılandırma kapsamını al |
> | Action | Microsoft. Operationalınsights/çalışma alanları/configurationScopes/yaz | Yapılandırma kapsamını ayarla |
> | Action | Microsoft. Operationalınsights/çalışma alanları/veri kaynakları/Sil | Bir çalışma alanındaki veri kaynaklarını silin. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/veri kaynakları/okuma | Bir çalışma alanı altındaki veri kaynaklarını alın. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/veri kaynakları/yazma | Bir çalışma alanında veri kaynakları oluşturun/güncelleştirin. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/Sil | Bir çalışma alanını siler. Çalışma alanı, oluşturma sırasında var olan bir çalışma alanına bağlanmışsa, bağlantılı olduğu çalışma alanı silinmez. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/ağ geçitleri/silme | Çalışma alanı için yapılandırılmış bir ağ geçidini kaldırır. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/generateregistrationcertificate/Action | Çalışma alanı için kayıt sertifikası oluşturur. Bu sertifika, Microsoft System Center Operation Manager 'ı çalışma alanına bağlamak için kullanılır. |
> | Action | Microsoft. Operationalınsights/Workspaces/ıntelligencepacks/Disable/Action | Belirli bir çalışma alanı için bir zeka paketini devre dışı bırakır. |
> | Action | Microsoft. Operationalınsights/Workspaces/ıntelligencepacks/Enable/Action | Belirli bir çalışma alanı için bir zeka paketi sunar. |
> | Action | Microsoft.OperationalInsights/workspaces/intelligencepacks/read | Belirli bir çalışma alanı için görünür olan tüm akıllı zeka paketlerini listeler ve ayrıca paketin bu çalışma alanı için etkin veya devre dışı olduğunu listeler. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/linkedServices/Delete | Belirtilen çalışma alanı altındaki bağlı hizmetleri silin. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/linkedServices/okuma | Belirtilen çalışma alanı altındaki bağlı hizmetleri alın. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/linkedServices/Write | Belirtilen çalışma alanı altındaki bağlı hizmetleri oluşturun/güncelleştirin. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/listKeys/ACTION | Çalışma alanının liste anahtarlarını alır. Bu anahtarlar, Microsoft operasyonel içgörüler aracılarını çalışma alanına bağlamak için kullanılır. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/listKeys/Read | Çalışma alanının liste anahtarlarını alır. Bu anahtarlar, Microsoft operasyonel içgörüler aracılarını çalışma alanına bağlamak için kullanılır. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/managementGroups/okuma | Bu çalışma alanına bağlı System Center Operations Manager yönetim gruplarının adlarını ve meta verilerini alır. |
> | Action | Microsoft. Operationalınsights/Workspaces/metricDefinitions/okuma | Çalışma alanı altında ölçüm tanımlarını al |
> | Action | Microsoft. Operationalınsights/çalışma alanları/notificationSettings/Delete | Çalışma alanının kullanıcı bildirim ayarlarını silin. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/notificationSettings/Read | Çalışma alanının kullanıcı bildirim ayarlarını alın. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/notificationSettings/Write | Çalışma alanının kullanıcı bildirim ayarlarını belirleyin. |
> | Action | Microsoft. operationalınsights/çalışma alanları/işlemler/okuma | Bir Operationalınsights çalışma alanı işleminin durumunu alır. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/temizleme/eylem | Belirtilen verileri çalışma alanından Sil |
> | Action | Microsoft. Operationalınsights/Workspaces/sorgu/AADDomainServicesAccountLogon/Read | AADDomainServicesAccountLogon tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/AADDomainServicesAccountManagement/Read | AADDomainServicesAccountManagement tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/AADDomainServicesDirectoryServiceAccess/Read | AADDomainServicesDirectoryServiceAccess tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/AADDomainServicesLogonLogoff/Read | AADDomainServicesLogonLogoff tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/AADDomainServicesPolicyChange/Read | AADDomainServicesPolicyChange tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/AADDomainServicesPrivilegeUse/Read | AADDomainServicesPrivilegeUse tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/AADDomainServicesSystemSecurity/Read | AADDomainServicesSystemSecurity tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Adassessmentönerisi/okuma | Adassessmentöneriyi tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/AddonAzureBackupAlerts/okuma | AddonAzureBackupAlerts tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/AddonAzureBackupJobs/okuma | AddonAzureBackupJobs tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/AddonAzureBackupPolicy/okuma | AddonAzureBackupPolicy tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/AddonAzureBackupProtectedInstance/okuma | AddonAzureBackupProtectedInstance tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/AddonAzureBackupStorage/okuma | AddonAzureBackupStorage tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/ADFActivityRun/Read | ADFActivityRun tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Adfardışık düzen eylemsizlik/Read | Adfardışık düzen eylemsizlik tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/ADFTriggerRun/Read | ADFTriggerRun tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/ADReplicationResult/okundu | ADReplicationResult tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Adsecurityassessmentönerisi/okuma | Adsecurityassessmentönerisi tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/uyarı/okuma | Uyarı tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/AlertHistory/Read | AlertHistory tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/AmlComputeClusterEvent/okuma | AmlComputeClusterEvent tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/AmlComputeClusterNodeEvent/okuma | AmlComputeClusterNodeEvent tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/AmlComputeJobEvent/okuma | AmlComputeJobEvent tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/ApiManagementGatewayLogs/Read | ApiManagementGatewayLogs tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Appcenterhata/okuma | AppCenterError tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/ApplicationInsights/okuma | ApplicationInsights tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Appplatformlogsforyay/okuma | AppPlatformLogsforSpring tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/AuditLogs/okuma | AuditLogs tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/AutoscaleEvaluationsLog/okuma | AutoscaleEvaluationsLog tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/oto Scalescaleactionslog/Read | Oto Scalescaleactionslog tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Awscses Izi/okuma | Awscses Iziz tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/AzureActivity/okuma | AzureActivity tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/sorgu/Azureassessmentönerisi/okuma | Azureassessmentöneriyi tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/AzureMetrics/okuma | AzureMetrics tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/BaiClusterEvent/Read | BaiClusterEvent tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/BaiClusterNodeEvent/Read | BaiClusterNodeEvent tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/BaiJobEvent/okuma | BaiJobEvent tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/BlockchainApplicationLog/Read | BlockchainApplicationLog tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/BlockchainProxyLog/Read | BlockchainProxyLog tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/BoundPort/Read | BoundPort tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/CommonSecurityLog/Read | CommonSecurityLog tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/bilgisayar grubu/okuma | ComputerGroup tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/ConfigurationChange/Read | ConfigurationChange tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/ConfigurationData/Read | ConfigurationData tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Containerımageınventory/Read | Containerımageınventory tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Containerınventory/Read | Containerınventory tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/ContainerLog/Read | ContainerLog tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Containernodeınventory/Read | Containernodeınventory tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/sorgu/ContainerRegistryLoginEvents/Read | ContainerRegistryLoginEvents tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Containerregistrydepotoryevents/Read | Containerregistrydepotoryevents tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/ContainerServiceLog/okuma | ContainerServiceLog tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/sorgu/CoreAzureBackup/Read | CoreAzureBackup tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/DatabricksAccounts/okuma | DatabricksAccounts tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/DatabricksClusters/okuma | DatabricksClusters tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/DatabricksDBFS/Read | DatabricksDBFS tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Databricksınstancepools/Read | Databricksınstancepools tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Databricksk Işleri/okuma | DatabricksJobs tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Databricksnot/okuma | DatabricksNotebook tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Databricksgizlilikler/Read | Databricksgizlilikler tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Databricksssınpermissions/Read | Databricksstopermissions tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/DatabricksSSH/Read | DatabricksSSH tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/DatabricksTables/Read | DatabricksTables tablosundan veri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/DatabricksWorkspace/Read | DatabricksWorkspace tablosundan veri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/DeviceAppCrash/okuma | DeviceAppCrash tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Deviceapplayeniden Başlat/oku | Deviceapplayeniden Başlat tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/DeviceCalendar/Read | DeviceCalendar tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Devicectaanup/okuma | Devicectaanup tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/DeviceConnectSession/Read | DeviceConnectSession tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/DeviceEtw/okuma | DeviceEtw tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/DeviceHardwareHealth/Read | DeviceHardwareHealth tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/DeviceHealth/Read | DeviceHealth tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Devicesinyal/okuma | Devicesinyal tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/DeviceSkypeHeartbeat/okuma | DeviceSkypeHeartbeat tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/DeviceSkypeSignIn/okuma | DeviceSkypeSignIn tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Devicesliepstate/Read | Devicestaepstate tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Dnne hatası/okuma | Dılure tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Dakppresorumluluğun/okunan | Dappreyükümlülük tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Dhdrivergüvenirlik/okuma | Dhdrivergüvenirlik tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Dhlogonhatalarıyla/okuma | Dhlogonarızaları tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Dhlogonölçümlerini/Read | Dhlogonölçümlerini tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Dhoatık Kıdata/Read | Dhoatık verileri tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Dhosgüvenilirliği/okuma | Dhosgüvenirlik tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/DHWipAppLearning/okuma | DHWipAppLearning tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/DnsEvents/okuma | DnsEvents tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Dnsınventory/okuma | DnsInventory tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/ETWEvent/okuma | ETWEvent tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/olay/okuma | Olay tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/ExchangeAssessmentRecommendation/okuma | ExchangeAssessmentRecommendation tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/ExchangeOnlineAssessmentRecommendation/okuma | ExchangeOnlineAssessmentRecommendation tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/sinyal/okuma | Sinyal tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/HuntingBookmark/Read | HuntingBookmark tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/IISAssessmentRecommendation/okuma | IISAssessmentRecommendation tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/ınboundconnection/Read | Inboundconnection tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/ınsightsölçümlerini/Read | Insightsölçümlerini tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/ıntuneauditlogs/Read | Intuneauditlogs tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/ıntuneoperationallogs/Read | Intuneoperationallogs tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/KubeEvents/okuma | KubeEvents tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Kubenodeınventory/Read | Kubenodeınventory tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/KubePodInventory/okuma | KubePodInventory tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/KubeServices/Read | KubeServices tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/LinuxAuditLog/Read | LinuxAuditLog tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/sorgu/MAApplication/Read | MAApplication tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/MAApplicationHealth/Read | MAApplicationHealth tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/sorgu/MAApplicationHealthAlternativeVersions/Read | MAApplicationHealthAlternativeVersions tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Maapplicationhealthsorunlar/okuma | Maapplicationhealthsorunlar tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maapplicationınstance/Read | Maapplicationınstance tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maapplicationınstancereadsürekliliği/okuma | Maapplicationınstancereadılmi tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/MAApplicationReadiness/Read | MAApplicationReadiness tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/MADeploymentPlan/okuma | MADeploymentPlan tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/MADevice/okuma | MADevice tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Madevicenotentopla/Read | Madevicenotentoplatable 'dan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/MADeviceNRT/okuma | MADeviceNRT tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/sorgu/MADevicePnPHealth/Read | MADevicePnPHealth tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/MADevicePnPHealthAlternativeVersions/Read | MADevicePnPHealthAlternativeVersions tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Madevicepnphealthsorunlar/okuma | Madevicepnphealthsorunlar tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Madevicereadsürekliliği/okuma | Madevicereadtıo tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Madriverınstancereadsürekliliği/okuma | Madriverınstancereadılmi tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/MADriverReadiness/Read | MADriverReadiness tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficeaddın/Read | Maofficeeklenti tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficeaddınentityhealth/Read | Maofficeaddınentityhealth tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficeaddınhealth/Read | Maofficeaddınhealth tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/MAOfficeAddinHealthEventNRT/okuma | MAOfficeAddinHealthEventNRT tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficeaddınhealthsorunlar/okuma | Maofficeaddınhealthsorunlar tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficeaddınınstance/Read | Maofficeaddınınstance tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficeaddınınstancereadsürekliliği/okuma | Maofficeaddınınstancereadılmi tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficeaddınreadiness/Read | Maofficeaddınreadiness tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficeuygulaması/okuma | Maofficeuygulama tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/MAOfficeAppCrashesNRT/okuma | MAOfficeAppCrashesNRT tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/sorgu/MAOfficeAppHealth/Read | MAOfficeAppHealth tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficeappınstance/Read | Maofficeappınstance tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficeappınstancehealth/Read | Maofficeappınstancehealth tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/MAOfficeAppReadiness/Read | MAOfficeAppReadiness tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/MAOfficeAppSessionsNRT/okuma | MAOfficeAppSessionsNRT tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficebuilınfo/Read | Maofficebuilınfo tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/MAOfficeCurrencyAssessment/okuma | MAOfficeCurrencyAssessment tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/MAOfficeCurrencyAssessmentDailyCounts/okuma | MAOfficeCurrencyAssessmentDailyCounts tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/sorgu/MAOfficeDeploymentStatus/Read | MAOfficeDeploymentStatus tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/MAOfficeDeploymentStatusNRT/Read | MAOfficeDeploymentStatusNRT tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficemakroerrornrt/okuma | MAOfficeMacroErrorNRT tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficemakroglobalhealth/Read | Maofficemakroglobalhealth tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficemakrosağlığı/okuma | Maofficemakrohealth tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficemakrohealthsorunlar/okuma | Maofficemacrohealthsorunlar tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficemakroıssueınstancec/Read | Maofficemacroıssueınstancectablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficemakroıssuereadsürekliliği/okuma | Maofficemacroıssuereadılmi tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficemakroözeti/okuma | MAOfficeMacroSummary tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/MAOfficeSuite/okuma | MAOfficeSuite tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Maofficesuiteınstance/Read | Maofficesuiteınstance tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/MAProposedPilotDevices/okuma | MAProposedPilotDevices tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Mawindowsbuilınfo/Read | Mawindowsbuilınfo tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Mawindowscurrencment/Read | Mawindowscurrencyaskment tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Mawindowscurrencyassessmentgünlükcounts/okuma | MAWindowsCurrencyAssessmentDailyCounts tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/Workspaces/sorgu/MAWindowsDeploymentStatus/Read | MAWindowsDeploymentStatus tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/MAWindowsDeploymentStatusNRT/okuma | MAWindowsDeploymentStatusNRT tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Mawindowssysreqınstancereadsürekliliği/okuma | Mawindowssysreqınstancereadtıı tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/McasShadowItReporting/okuma | McasShadowItReporting tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Microservices4SpringApplicationLogs/okuma | Microservices4SpringApplicationLogs tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Microsoftinsıizsazureactivitylog/Read | Microsoftınsı, Sazureactivitylog tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/MicrosoftWebApplicationLog/Read | MicrosoftWebApplicationLog tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/MicrosoftWebFunctionExecutionLogs/Read | MicrosoftWebFunctionExecutionLogs tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/MicrosoftWebStdOutStdErrLog/okuma | MicrosoftWebStdOutStdErrLog tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/MicrosoftWebW3CLog/okuma | MicrosoftWebW3CLog tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/NetworkMonitoring/Read | NetworkMonitoring tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Officeetkinliği/okuma | OfficeActivity tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Işlem/okuma | Işlem tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/OutboundConnection/Read | OutboundConnection tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/perf/okuma | Perf tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/ProtectionStatus/Read | ProtectionStatus tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/okuma | Çalışma alanındaki veriler üzerinde sorgu çalıştırma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/ReservedAzureCommonFields/okuma | ReservedAzureCommonFields tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/ReservedCommonFields/Read | ReservedCommonFields tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Sccmassessmentönerisi/okuma | Sccmassessmentöneriyi tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Scomassessmentönerisi/okuma | Scomassessmentöneriyi tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/SecurityAlert/Read | SecurityAlert tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/SecurityBaseline/okuma | SecurityBaseline tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/SecurityBaselineSummary/Read | SecurityBaselineSummary tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/SecurityDetection/okuma | SecurityDetection tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/SecurityEvent/Read | SecurityEvent tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Securityıotrawevent/Read | Securityıotrawevent tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Securityönerisi/okuma | Securityöneriyi tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/ServiceFabricOperationalEvent/Read | ServiceFabricOperationalEvent tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/ServiceFabricReliableActorEvent/Read | ServiceFabricReliableActorEvent tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/ServiceFabricReliableServiceEvent/Read | ServiceFabricReliableServiceEvent tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Sfbassessmentönerisi/okuma | Sfbassessmentöneriyi tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/SfBOnlineAssessmentRecommendation/okuma | SfBOnlineAssessmentRecommendation tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/SharePointOnlineAssessmentRecommendation/okuma | SharePointOnlineAssessmentRecommendation tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Signınlogs/Read | Signınlogs tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Spassessmentönerisi/okuma | Spassessmentöneriyi tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Sqlassessmentönerisi/okuma | Sqlassessmentöneriyi tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/SQLQueryPerformance/Read | SQLQueryPerformance tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Sqlthreatprotectionlogindenetimleri/okuma | Sqlthreatprotectionlogindenetimleri tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/SQL,, ıyassessmentresult/Read | SQLbir bu sonuç tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Syslog/okuma | Syslog tablosundan veri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/SysmonEvent/Read | SysmonEvent tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/tablolar. Custom/Read | Herhangi bir özel günlükteki verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Threatıntelligenceındicator/Read | Threatıntelligenceındicator tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Uıaapp/Read | UAApp tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Uıacomputer/Read | UAComputer tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/UAComputerRank/Read | Uıacomputerrank tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/UADriver/okuma | Uıadriver tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Uıadriverproblemcodes/Read | Uıadriverproblemcodes tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/UAFeedback/Read | UAFeedback tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Uıahardwaresecurity/Read | Uıahardwaresecurity tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Uaıesitediscovery/Read | Uıaııesitediscovery tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Uaofficeaddın/Read | Uıaofficeaddın tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/UAProposedActionPlan/okuma | UAProposedActionPlan tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Uasysreqıssue/Read | Uıasysreqıssue tablosundaki verileri oku |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Uayükseltilebilir Dedcomputer/Read | Uayükseltilebilir Dedcomputer tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/güncelleştirme/okuma | Güncelleştirme tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/UpdateRunProgress/Read | UpdateRunProgress tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/UpdateSummary/Read | UpdateSummary tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/kullanım/okuma | Kullanım tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/VMBoundPort/Read | VMBoundPort tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/VMConnection/Read | VMConnection tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/W3CIISLog/okuma | W3CIISLog tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/WaaSDeploymentStatus/Read | WaaSDeploymentStatus tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Waasınsiderstatus/Read | Waasınsiderstatus tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/WaaSUpdateStatus/Read | WaaSUpdateStatus tablosundan verileri oku |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/WDAVStatus/Read | WDAVStatus tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/WDAVThreat/Read | WDAVThreat Table 'dan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/Windowsclientassessmentönerisi/okuma | Windowsclientassessmentöneriyi tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/WindowsEvent/okuma | WindowsEvent tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/WindowsFirewall/Read | WindowsFirewall tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/WindowsServerAssessmentRecommendation/okuma | WindowsServerAssessmentRecommendation tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/kablolu veri/okuma | Kablolu veri tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/Workspaces/Query/WorkloadMonitoringPerf/Read | WorkloadMonitoringPerf tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/Wudoaggregişlendiği durumu/okuma | Wudoaggreg, Status tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sorgu/WUDOStatus/okuma | WUDOStatus tablosundan verileri okuma |
> | Action | Microsoft. Operationalınsights/çalışma alanları/okuma | Mevcut bir çalışma alanını alır |
> | Action | Microsoft. Operationalınsights/Workspaces/regeneratesharedkey/Action | Belirtilen çalışma alanı paylaşılan anahtarını yeniden oluşturur |
> | Action | Microsoft. operationalınsights/çalışma alanları/kurallar/okuma | Tüm uyarı kurallarını al. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/Savedaramalar/silme | Kaydedilmiş bir arama sorgusunu siler |
> | Action | Microsoft. Operationalınsights/çalışma alanları/Savedaramalar/okuma | Kayıtlı bir arama sorgusu alır |
> | Action | Microsoft. operationalınsights/çalışma alanları/savedaramalar/sonuçlar/okuma | Kaydedilmiş aramaları al sonuçları. Kullanım Dışı |
> | Action | Microsoft. operationalınsights/çalışma alanları/savedaramalar/zamanlamalar/eylemler/silme | Zamanlanmış arama eylemlerini silin. |
> | Action | Microsoft. operationalınsights/çalışma alanları/savedaramalar/zamanlamalar/eylemler/okuma | Zamanlanmış arama eylemleri alın. |
> | Action | Microsoft. operationalınsights/çalışma alanları/savedaramalar/zamanlamalar/eylemler/yazma | Zamanlanmış arama eylemleri oluşturun veya güncelleştirin. |
> | Action | Microsoft. operationalınsights/çalışma alanları/savedaramalar/zamanlamalar/silme | Zamanlanan aramaları silin. |
> | Action | Microsoft. operationalınsights/çalışma alanları/savedaramalar/zamanlamalar/okuma | Zamanlanmış aramalar alın. |
> | Action | Microsoft. operationalınsights/çalışma alanları/savedaramalar/zamanlamalar/yazma | Zamanlanmış aramalar oluşturun veya güncelleştirin. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/Savedaramalar/yazma | Kaydedilmiş bir arama sorgusu oluşturur |
> | Action | Microsoft. Operationalınsights/çalışma alanları/şema/okuma | Çalışma alanının arama şemasını alır.  Arama şeması, sunulan alanları ve bunların türlerini içerir. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/arama/eylem | Arama sorgusu yürütür |
> | Action | Microsoft. operationalınsights/çalışma alanları/arama/okuma | Arama sonuçlarını al. Kullanım dışı. |
> | Action | Microsoft. Operationalınsights/Workspaces/sharedKeys/Action | Çalışma alanının paylaşılan anahtarlarını alır. Bu anahtarlar, Microsoft operasyonel içgörüler aracılarını çalışma alanına bağlamak için kullanılır. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/sharedKeys/Read | Çalışma alanının paylaşılan anahtarlarını alır. Bu anahtarlar, Microsoft operasyonel içgörüler aracılarını çalışma alanına bağlamak için kullanılır. |
> | Action | Microsoft. Operationalınsights/Workspaces/storageınsiizconfigs/Delete | Depolama yapılandırmasını siler. Bu, Microsoft operasyonel içgörüler 'in depolama hesabından veri okumasını durdurur. |
> | Action | Microsoft. Operationalınsights/Workspaces/storageınsiizconfigs/Read | Bir depolama yapılandırması alır. |
> | Action | Microsoft. Operationalınsights/Workspaces/storageınsiizconfigs/Write | Yeni bir depolama yapılandırması oluşturur. Bu yapılandırma, mevcut bir depolama hesabındaki bir konumdan veri çekmek için kullanılır. |
> | Action | Microsoft. Operationalınsights/Workspaces/upgradetranslationhatalarıyla/okuma | Çalışma alanı için arama yükseltme çevirisi hata günlüğünü al |
> | Action | Microsoft. Operationalınsights/çalışma alanları/kullanımlar/okuma | Çalışma alanı tarafından okunan veri miktarı dahil olmak üzere çalışma alanı için kullanım verilerini alır. |
> | Action | Microsoft. Operationalınsights/çalışma alanları/yazma | Mevcut çalışma alanından müşteri kimliğini sağlayarak yeni bir çalışma alanı veya mevcut bir çalışma alanına bağlantılar oluşturur. |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. OperationsManagement/managementAssociations/Delete | Varolan yönetim Ilişkilendirmesini Sil |
> | Action | Microsoft. OperationsManagement/managementAssociations/Read | Varolan yönetim Ilişkilendirmesini al |
> | Action | Microsoft. OperationsManagement/managementAssociations/Write | Yeni bir yönetim Ilişkilendirmesi oluştur |
> | Action | Microsoft. OperationsManagement/managementConfigurations/Delete | Varolan yönetim yapılandırmasını sil |
> | Action | Microsoft. OperationsManagement/managementConfigurations/okuma | Varolan yönetim yapılandırmasını al |
> | Action | Microsoft. OperationsManagement/managementConfigurations/Write | Yeni bir yönetim yapılandırması oluştur |
> | Action | Microsoft. OperationsManagement/Register/Action | Bir kaynak sağlayıcısına abonelik kaydedin. |
> | Action | Microsoft. OperationsManagement/Solutions/Delete | Mevcut OMS çözümünü Sil |
> | Action | Microsoft. OperationsManagement/Solutions/Read | OMS çözümünü çıkmadan al |
> | Action | Microsoft. OperationsManagement/Solutions/Write | Yeni OMS çözümü oluştur |

## <a name="microsoftpolicyinsights"></a>Microsoft. Poliyelei

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Poliyeleghts/asyncOperationResults/Read | Zaman uyumsuz işlem sonucunu alır. |
> | Action | Microsoft. Poliyeleghts/işlemler/okuma | Microsoft. Policınsıghts ad alanı üzerinde desteklenen işlemleri alır |
> | Action | Microsoft. Poliyeleghts/policyEvents/queryResults/Action | İlke olayları hakkında sorgu bilgileri. |
> | Action | Microsoft. Poliyeleghts/policyEvents/queryResults/Read | İlke olayları hakkında sorgu bilgileri. |
> | Action | Microsoft. Poliyeleghts/policyStates/queryResults/Action | İlke durumlarıyla ilgili bilgileri sorgulayın. |
> | Action | Microsoft. Poliyeleghts/policyStates/queryResults/Read | İlke durumlarıyla ilgili bilgileri sorgulayın. |
> | Action | Microsoft. Poliyeleghts/policyStates/özetleme/eylem | İlke en son durumlarıyla ilgili özet bilgileri sorgulayın. |
> | Action | Microsoft. Poliyeleghts/policyStates/özetleme/okuma | İlke en son durumlarıyla ilgili özet bilgileri sorgulayın. |
> | Action | Microsoft. Poliyeleghts/policyStates/triggerEvaluation/Action | Seçili kapsam için yeni bir uyumluluk değerlendirmesi tetikler. |
> | Action | Microsoft. Poliyeleghts/policyTrackedResources/queryResults/Read | DeployIfNotExists ilkeleri için gereken kaynaklarla ilgili bilgileri sorgulayın. |
> | Action | Microsoft. Poliyeleghts/Register/Action | Microsoft Policy Insights kaynak sağlayıcısını kaydeder ve üzerinde eylemler sunar. |
> | Action | Microsoft. Poliyeleghts/düzeltme/iptal/eylem | Devam eden Microsoft Ilke düzeltmelerinin iptali. |
> | Action | Microsoft. Poliyeleghts/düzeltme/silme | İlke düzeltmelerinin silme. |
> | Action | Microsoft. Poliyeleghts/düzeltmelere/Listdağıtımlar/okuma | Bir ilke düzeltmesi için gereken dağıtımları listeler. |
> | Action | Microsoft. Poliyeleghts/düzeltme/okuma | İlke düzeltmeleri alın. |
> | Action | Microsoft. Poliyeleghts/düzeltme/yazma | Microsoft Policy düzeltmelerinin oluşturun veya güncelleştirin. |
> | Action | Microsoft. Policınghts/Unregister/eylem | Microsoft Policy Insights kaynak sağlayıcısının kaydını siler. |

## <a name="microsoftportal"></a>Microsoft. Portal

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Portal/konsollar/silme | Cloud Shell örneğini kaldırır. |
> | Action | Microsoft. Portal/konsollar/okuma | Cloud Shell örneğini okur. |
> | Action | Microsoft. Portal/konsollar/yazma | Cloud Shell örneği oluşturun veya güncelleştirin. |
> | Action | Microsoft. Portal/panolar/silme | Panoyu abonelikten kaldırır. |
> | Action | Microsoft. Portal/panolar/okuma | Abonelik için panoları okur. |
> | Action | Microsoft. Portal/panolar/yazma | Bir aboneliğe Pano ekleme veya bu panoyu değiştirme. |
> | Action | Microsoft. Portal/kaydet/eylem | Portala Kaydet |
> | Action | Microsoft. Portal/UserSettings/Delete | Cloud Shell Kullanıcı ayarlarını kaldırır. |
> | Action | Microsoft. Portal/UserSettings/Read | Cloud Shell Kullanıcı ayarlarını okur. |
> | Action | Microsoft. Portal/UserSettings/Write | Cloud Shell Kullanıcı ayarı oluşturun veya güncelleştirin. |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Powerbiadanmış/kapasiteler/silme | Adanmış Power BI kapasitesini siler. |
> | Action | Microsoft. Powerbiadanmış/kapasiteler/okuma | Belirtilen Power BI adanmış kapasitenin bilgilerini alır. |
> | Action | Microsoft. Powerbiadanmış/kapasiteler/özgeçmişi/eylem | Kapasiteyi sürdürür. |
> | Action | Microsoft. Powerbiadanmış/kapasiteler/SKU/okuma | Kapasite için kullanılabilir SKU bilgilerini alın |
> | Action | Microsoft. Powerbiadanmış/kapasiteler/beklet/eylem | Kapasiteyi askıya alır. |
> | Action | Microsoft. Powerbiadanmış/kapasiteler/Write | Belirtilen Power BI adanmış kapasiteyi oluşturur veya güncelleştirir. |
> | Action | Microsoft. Powerbiadanmış/konumlar/Checknameavaılabılıty/Action | Verilen Power BI adanmış kapasite adının geçerli olduğunu ve kullanımda olup olmadığını denetler. |
> | Action | Microsoft. Powerbiadanmış/konumlar/operationresults/Read | Belirtilen işlem sonucu bilgilerini alır. |
> | Action | Microsoft. Powerbiadanmış/konumlar/operationdurumlar/okuma | Belirtilen işlem durumu bilgilerini alır. |
> | Action | Microsoft. Powerbiadanmış/işlemler/okuma | İşlem bilgilerini alır |
> | Action | Microsoft. Powerbiadanmış/yazmaç/eylem | Adanmış Power BI kaynak sağlayıcısını kaydeder. |
> | Action | Microsoft. Powerbiadanmış/SKU/okuma | SKU 'ların bilgilerini alır |

## <a name="microsoftrecoveryservices"></a>Microsoft. RecoveryServices

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir. |
> | Action | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp, hizmet tarafından kullanılan iç işlemdir |
> | Action | Microsoft. RecoveryServices/Locations/backupPreValidateProtection/Action |  |
> | Action | Microsoft. RecoveryServices/Locations/Backupkorunabilir/Write | Yedekleme korumalı öğesi oluştur |
> | Action | Microsoft. RecoveryServices/Locations/Backupkorunabilir/Read | Tüm Korumalı Öğelerin listesini döndürür. |
> | Action | Microsoft. RecoveryServices/konumlar/backupStatus/Action | Kurtarma Hizmetleri kasaları için yedekleme durumunu denetle |
> | Action | Microsoft. RecoveryServices/Locations/backupValidateFeatures/Action | Özellikleri doğrulama |
> | Action | Microsoft. RecoveryServices/Locations/Checknameavaılabılıty/Action | Kaynak adının kullanılabilirliğini denetle, kaynak adının kullanılabilir olup olmadığını denetlemek için bir API 'dir |
> | Action | Microsoft. RecoveryServices/konumlar/operationStatus/Read | Belirli bir Işlem için Işlem durumunu alır |
> | Action | Microsoft. RecoveryServices/işlemler/okuma | İşlem, bir kaynak sağlayıcısı için Işlem listesini döndürür |
> | Action | Microsoft. RecoveryServices/Register/Action | Belirtilen kaynak sağlayıcısı için aboneliği kaydeder |
> | Action | Microsoft. RecoveryServices/Vaults/backupconfig/Read | Kurtarma Hizmetleri Kasası için yapılandırmayı döndürür. |
> | Action | Microsoft. RecoveryServices/Vaults/backupconfig/Write | Kurtarma Hizmetleri Kasası için yapılandırmayı güncelleştirir. |
> | Action | Microsoft. RecoveryServices/Vaults/backupEngines/Read | Kasaya kayıtlı tüm yedekleme yönetimi sunucularının listesini döndürür. |
> | Action | Microsoft. RecoveryServices/Vaults/Backupdokuları/Backupprotectionhedefini/silme | Yedekleme koruma hedefini silme |
> | Action | Microsoft. RecoveryServices/Vaults/Backupdokuların/Backupprotectionamacını/Read | Yedekleme koruması hedefi al |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Yedekleme koruma hedefi oluşturma |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | İşlemin durumunu döndürür |
> | Action | Microsoft. RecoveryServices/Vaults/Backupyapılar/operationsStatus/Read | İşlemin durumunu döndürür |
> | Action | Microsoft. RecoveryServices/Vaults/Backupdokuları/korunabilir | Tüm korunabilir kapsayıcıları al |
> | Action | Microsoft. RecoveryServices/Vaults/Backupyapılar/protectionContainers/Delete | Kayıtlı kapsayıcıyı siler |
> | Action | Microsoft. RecoveryServices/Vaults/Backupyapılar/protectionContainers/sorgu/eylem | Bir kapsayıcı içindeki iş yükleri için sorgulama yap |
> | Action | Microsoft. RecoveryServices/Vaults/Backupyapılar/protectionContainers/öğeler/okuma | Bir kapsayıcıdaki tüm öğeleri Al |
> | Action | Microsoft. RecoveryServices/Vaults/Backupyapılar/protectionContainers/operationResults/Read | Koruma Kapsayıcısı üzerinde gerçekleştirilen İşlemin sonuçlarını alır. |
> | Action | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/operationsStatus/Read | Koruma kapsayıcısında gerçekleştirilen Işlemin durumunu alır. |
> | Action | Microsoft. RecoveryServices/Vaults/Backupyapılar/protectionContainers/korunabilir/yedekleme/eylem | Korumalı Öğe için Yedekleme gerçekleştirir. |
> | Action | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/Sil | Korumalı öğeyi siler |
> | Action | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/operationResults/Read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin Sonuçlarını alın. |
> | Action | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/operationsStatus/Read | Korumalı Öğeler üzerinde Gerçekleştirilen İşlemin durumunu döndürür. |
> | Action | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/Read | Korumalı öğenin nesne ayrıntılarını döndürür |
> | Action | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/provisionInstantItemRecovery/eylem | Korumalı öğe için anında öğe kurtarma sağla |
> | Action | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/kurtarma noktaları/okuma | Korumalı Öğeler için Kurtarma Noktalarını alın. |
> | Action | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/kurtarma noktaları/geri yükleme/eylem | Korumalı Öğeler için Kurtarma Noktalarını geri yükleyin. |
> | Action | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/revokeInstantItemRecovery/eylem | Korumalı öğe için anında öğe kurtarmayı iptal et |
> | Action | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/yazma | Yedekleme korumalı öğesi oluştur |
> | Action | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Tüm kayıtlı kapsayıcıları döndürür |
> | Action | Microsoft. RecoveryServices/Vaults/Backupyapılar/protectionContainers/Write | Kayıtlı bir kapsayıcı oluşturur |
> | Action | Microsoft. RecoveryServices/Vaults/Backupdokuları/refreshContainers/eylem | Kapsayıcı listesini yeniler |
> | Action | Microsoft. RecoveryServices/Vaults/backupJobs/iptal/eylem | Işi iptal et |
> | Action | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | İş İşleminin Sonucunu döndürür. |
> | Action | Microsoft. RecoveryServices/Vaults/backupJobs/operationsStatus/Read | Iş Işleminin durumunu döndürür. |
> | Action | Microsoft.RecoveryServices/Vaults/backupJobs/read | Tüm Iş nesnelerini döndürür |
> | Action | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Işleri dışarı aktar |
> | Action | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Kurtarma Hizmetleri Kasası için Yedekleme işleminin Sonucunu döndürür. |
> | Action | Microsoft. RecoveryServices/Vaults/backupOperations/Read | Kurtarma Hizmetleri Kasası için yedekleme Işlemi durumunu döndürür. |
> | Action | Microsoft. RecoveryServices/Vaults/backupPolicies/Delete | Koruma Ilkesini silme |
> | Action | Microsoft. RecoveryServices/Vaults/backupPolicies/operationResults/Read | İlke İşleminin Sonuçlarını alın. |
> | Action | Microsoft. RecoveryServices/Vaults/backupPolicies/işlemler/okuma | Ilke Işleminin durumunu alın. |
> | Action | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Tüm koruma Ilkelerini döndürür |
> | Action | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Koruma Ilkesi oluşturur |
> | Action | Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Tüm Korunabilir Öğelerin listesini döndürür. |
> | Action | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Tüm Korumalı Öğelerin listesini döndürür. |
> | Action | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Aboneliğe ait tüm kapsayıcıları döndürür |
> | Action | Microsoft. RecoveryServices/Vaults/Backupprotectionhedefleri/okuma | Tüm yedekleme koruma amaçlarını Listele |
> | Action | Microsoft. RecoveryServices/Vaults/backupSecurityPIN/Action | Kurtarma Hizmetleri Kasası için güvenlik PIN bilgilerini döndürür. |
> | Action | Microsoft. RecoveryServices/Vaults/backupstorageconfig/Read | Kurtarma Hizmetleri Kasası için depolama yapılandırmasını döndürür. |
> | Action | Microsoft. RecoveryServices/Vaults/backupstorageconfig/Write | Kurtarma Hizmetleri Kasası için depolama yapılandırmasını güncelleştirir. |
> | Action | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Kurtarma Hizmetleri için korumalı öğeler ve korumalı sunucular için özetler döndürür. |
> | Action | Microsoft. RecoveryServices/Vaults/backupValidateOperation/Action | Korumalı öğe üzerinde Işlemi doğrula |
> | Action | Microsoft.RecoveryServices/Vaults/certificates/write | Kaynak sertifikası güncelleştirme işlemi kaynak/kasa kimlik bilgisi sertifikasını güncelleştirir. |
> | Action | Microsoft. RecoveryServices/Vaults/Delete | Kasayı Sil işlemi, ' kasa ' türündeki belirtilen Azure kaynağını siler |
> | Action | Microsoft. RecoveryServices/Kasaults/Extendedınformation/Delete | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Action | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Action | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Action | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Kurtarma Hizmetleri Kasası için uyarıları alır. |
> | Action | Microsoft. RecoveryServices/Vaults/monitoringAlerts/Write | Uyarıyı çözer. |
> | Action | Microsoft. RecoveryServices/Vaults/monitoringConfigurations/okuma | Kurtarma Hizmetleri Kasası bildirim yapılandırmasını alır. |
> | Action | Microsoft. RecoveryServices/Vaults/monitoringConfigurations/Write | Kurtarma Hizmetleri kasasına e-posta bildirimlerini yapılandırır. |
> | Action | Microsoft.RecoveryServices/Vaults/read | Kasayı al işlemi, ' kasa ' türündeki Azure kaynağını temsil eden bir nesneyi alır |
> | Action | Microsoft. RecoveryServices/Kasaults/registeredIdentities/Delete | Kapsayıcı kaydını silme işlemi, bir kapsayıcının kaydını silmek için kullanılabilir. |
> | Action | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Işlem sonuçlarını al işlemi, zaman uyumsuz olarak gönderilen işlemin işlem durumunu ve sonucunu almak için kullanılabilir |
> | Action | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Kapsayıcıları al işlemi, bir kaynak için kayıtlı olan kapsayıcıları almak için kullanılabilir. |
> | Action | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Hizmet kapsayıcısını Kaydet işlemi, bir kapsayıcıyı kurtarma hizmeti 'ne kaydetmek için kullanılabilir. |
> | Action | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Tüm uyarı ayarlarını okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/replicationAlertSettings/Write | Tüm uyarı ayarlarını oluşturma veya güncelleştirme |
> | Action | Microsoft.RecoveryServices/vaults/replicationEvents/read | Tüm olayları okuyun |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Doku Tutarlılığını Denetler |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/silme | Tüm dokuları Sil |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuların/deployProcessServerImage/eylem | Işlem sunucusu görüntüsünü dağıt |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/migratetoaad/eylem | Dokuyu AAD 'ye geçir |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/operationresults/Read | Kaynak dokularındaki zaman uyumsuz bir işlemin sonuçlarını izleme |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Tüm yapıları okuyun |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Ağ geçidini yeniden ilişkilendir |
> | Action | Microsoft. RecoveryServices/kasaults/Replicationyapılar/kaldırma/eylem | Dokuyu kaldır |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Doku için Sertifikayı Yenile |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationLogicalNetworks/okuma | Tüm mantıksal ağları okuyun |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Tüm ağları okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationNetworks/replicationNetworkMappings/Delete | Tüm ağ eşlemelerini silme |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationNetworks/replicationNetworkMappings/Read | Tüm ağ eşlemelerini okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationNetworks/replicationNetworkMappings/Write | Herhangi bir ağ eşlemesi oluşturun veya güncelleştirin |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuları/replicationProtectionContainers/Discoverlıtableıtem/Action | Korunabilir öğeyi bul |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/operationresults/Read | Kaynak koruma kapsayıcılarındaki zaman uyumsuz bir işlemin sonuçlarını izleme |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Read | Tüm koruma kapsayıcılarını okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Remove/Action | Koruma kapsayıcısını kaldır |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationmigrationıtems/Delete | Tüm geçiş öğelerini silme |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationmigrationıtems/Migrate/Action | Öğeyi geçir |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationmigrationıtems/migrationRecoveryPoints/Read | Tüm geçiş kurtarma noktalarını okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationmigrationıtems/operationresults/Read | Kaynak geçiş öğelerinde zaman uyumsuz bir işlemin sonuçlarını izleyin |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationmigrationıtems/Read | Herhangi bir geçiş öğesini okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationmigrationıtems/testMigrate/Action | Test geçişi |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationmigrationıtems/testMigrateCleanup/Action | Test geçişini Temizleme |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationmigrationıtems/Write | Herhangi bir geçiş öğesi oluşturun veya güncelleştirin |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorunabilir | Korunabilir öğeleri oku |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorunabilir/addDisks/Action | Disk ekleme |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorunabilir/applyRecoveryPoint/Action | Kurtarma noktası Uygula |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorunabilir/Delete | Tüm korumalı öğeleri sil |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuları/replicationProtectionContainers/Replicationkorunabilir/işlem | Yük devretme yürütmesi |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorunabilir/operationresults/Read | Kaynak korumalı öğelerde zaman uyumsuz bir işlemin sonuçlarını izleme |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuların/replicationProtectionContainers/Replicationkorunabilir/Plannedyük devretme/eylem | Planlanan Yük Devretme |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuların/replicationProtectionContainers/Replicationkorunabilir/Read | Tüm korumalı öğeleri okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorunabilir/recoveryPoints/Read | Tüm çoğaltma kurtarma noktalarını okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorunabilir/kaldırma/eylem | Korumalı öğeyi kaldır |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorunabilir/removeDisks/Action | Diskleri kaldır |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuların/replicationProtectionContainers/Replicationkorunabilir/\ çoğaltma/eylem | Çoğaltmayı Onar |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorudıtems/reProtect/ACTION | Korumalı öğeyi yeniden koru |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorunabilir/ResolveHealthErrors/Action |  |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuları/replicationProtectionContainers/Replicationkorunabilir/geri bildirim/eylem | Geri Bildirim Gönder |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuları/replicationProtectionContainers/Replicationkorunabilir/targetComputeSizes/Read | Herhangi bir hedef Işlem boyutunu okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuları/replicationProtectionContainers/Replicationkorunabilir/TEMS/testFailover/ACTION | Test Yük Devretmesi |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuları/replicationProtectionContainers/Replicationkorunabilir/test Failovercleanup/Action | Yük devretme sınamasını Temizleme |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorunabilir/unplannedFailover/ACTION | Yük devret |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuların/replicationProtectionContainers/Replicationkorunabilir/Updatebir Dıtems/Update, Ityservice/Action | Mobility hizmetini Güncelleştir |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuların/replicationProtectionContainers/Replicationkorunabilir/Write | Korunan öğeleri oluşturma veya güncelleştirme |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/replicationProtectionContainerMappings/Delete | Tüm koruma kapsayıcısı eşlemelerini silme |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/replicationProtectionContainerMappings/operationresults/Read | Kaynak koruma kapsayıcısı eşlemelerinde zaman uyumsuz bir işlemin sonuçlarını izleme |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/replicationProtectionContainerMappings/Read | Tüm koruma kapsayıcısı eşlemelerini okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/replicationProtectionContainerMappings/Remove/Action | Koruma kapsayıcısı eşlemesini Kaldır |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/replicationProtectionContainerMappings/Write | Herhangi bir koruma kapsayıcısı eşlemesi oluşturun veya güncelleştirin |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/switchprotection/eylem | Koruma kapsayıcısını Değiştir |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Write | Herhangi bir koruma kapsayıcısı oluşturun veya güncelleştirin |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationRecoveryServicesProviders/Delete | Tüm kurtarma hizmetleri sağlayıcılarını silme |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationRecoveryServicesProviders/operationresults/Read | Kaynak kurtarma hizmetleri sağlayıcıları üzerinde zaman uyumsuz bir işlemin sonuçlarını izleme |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuların/replicationRecoveryServicesProviders/Read | Tüm kurtarma hizmetleri sağlayıcılarını okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationRecoveryServicesProviders/refreshProvider/Action | Sağlayıcıyı Yenile |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationRecoveryServicesProviders/Remove/Action | Kurtarma Hizmetleri sağlayıcısını kaldır |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuların/replicationRecoveryServicesProviders/Write | Herhangi bir kurtarma hizmetleri sağlayıcısı oluşturun veya güncelleştirin |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/Replicationstorageclassıfler/okuma | Tüm depolama sınıflandırmalarını okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuların/Replicationstorageclassıflıya/Replicationstorageclassıficationmappings/Delete | Tüm depolama sınıflandırması eşlemelerini silme |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuların/Replicationstorageclassıflıya/Replicationstorageclassıficationmappings/operationresults/Read | Kaynak depolama sınıflandırması eşlemelerinde zaman uyumsuz bir işlemin sonuçlarını izleyin |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuların/Replicationstorageclassıflıya/Replicationstorageclassıficationmappings/Read | Tüm depolama sınıflandırması eşlemelerini okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationdokuların/Replicationstorageclassıfmalar/Replicationstorageclassıficationmappings/Write | Tüm depolama sınıflandırması eşlemelerini oluşturun veya güncelleştirin |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationvCenters/Delete | Tüm sanal merkezleri silme |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationvCenters/operationresults/Read | Kaynak sanal merkezlerindeki zaman uyumsuz bir işlemin sonuçlarını izleme |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Tüm sanal merkezleri okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationvCenters/Write | Tüm sanal merkezleri oluşturun veya güncelleştirin |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationyapılar/yazma | Tüm yapılar oluşturun veya güncelleştirin |
> | Action | Microsoft. RecoveryServices/Vaults/replicationJobs/iptal/eylem | İşi İptal Et |
> | Action | Microsoft. RecoveryServices/Vaults/replicationJobs/operationresults/Read | Kaynak Işlerinde zaman uyumsuz bir işlemin sonuçlarını izleme |
> | Action | Microsoft.RecoveryServices/vaults/replicationJobs/read | Tüm Işleri okuyun |
> | Action | Microsoft. RecoveryServices/kasaults/replicationJobs/yeniden Başlat/eylem | İşi yeniden Başlat |
> | Action | Microsoft. RecoveryServices/kasaults/replicationJobs/özgeçmişi/eylem | Işi sürdürür |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationmigrationıtems/Read | Herhangi bir geçiş öğesini okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/replicationNetworkMappings/Read | Tüm ağ eşlemelerini okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/replicationNetworks/Read | Tüm ağları okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/replicationPolicies/Delete | Tüm Ilkeleri silme |
> | Action | Microsoft. RecoveryServices/Vaults/replicationPolicies/operationresults/Read | Kaynak Ilkelerinde zaman uyumsuz bir işlemin sonuçlarını izleme |
> | Action | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Tüm Ilkeleri okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/replicationPolicies/Write | Herhangi bir Ilke oluşturun veya güncelleştirin |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationkorunabilir/Read | Tüm korumalı öğeleri okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/replicationProtectionContainerMappings/Read | Tüm koruma kapsayıcısı eşlemelerini okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/replicationProtectionContainers/Read | Tüm koruma kapsayıcılarını okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationrecoveryplan/Delete | Tüm kurtarma planlarını silme |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Yük devretme yürütmesi kurtarma planı |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationrecoveryplan/operationresults/Read | Kaynak kurtarma planlarında zaman uyumsuz bir işlemin sonuçlarını izleme |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Planlı Yük devretme kurtarma planı |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Tüm kurtarma planlarını okuyun |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Kurtarma planını yeniden koru |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Yük devretme kurtarma planını sına |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Sınama yük devretmesi Temizleme kurtarma planı |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Yük devretme kurtarma planı |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationrecoveryplan/Write | Herhangi bir kurtarma planı oluşturun veya güncelleştirin |
> | Action | Microsoft. RecoveryServices/Vaults/replicationRecoveryServicesProviders/Read | Tüm kurtarma hizmetleri sağlayıcılarını okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationstorageclassıficationmappings/Read | Tüm depolama sınıflandırması eşlemelerini okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationstorageclassıfler/okuma | Tüm depolama sınıflandırmalarını okuyun |
> | Action | Microsoft. RecoveryServices/Vaults/replicationSupportedOperatingSystems/okuma | Her birini okuyun  |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationkullanımlar/okuma | Tüm kasa çoğaltma kullanımlarını okuyun |
> | Action | Microsoft. RecoveryServices/kasaults/Replicationerthegizli/operationresults/Read | Kaynak Kasası çoğaltma durumu üzerinde zaman uyumsuz bir işlemin sonuçlarını izleyin |
> | Action | Microsoft. RecoveryServices/Vaults/Replicationvaulthegizli/Read | Tüm kasa çoğaltma sistem durumunu okuyun |
> | Action | Microsoft. RecoveryServices/kasaults/Replicationvaulthegizli/Yenile/eylem | Kasa durumunu yenile |
> | Action | Microsoft. RecoveryServices/Vaults/replicationVaultSettings/Read | Her birini okuyun  |
> | Action | Microsoft. RecoveryServices/Vaults/replicationVaultSettings/Write | Oluşturma veya güncelleştirme  |
> | Action | Microsoft. RecoveryServices/Vaults/replicationvCenters/Read | Tüm sanal merkezleri okuyun |
> | Action | Microsoft. RecoveryServices/kasaults/kullanımlar/okuma | Tüm kasa kullanımlarını okuyun |
> | Action | Microsoft.RecoveryServices/Vaults/usages/read | Bir Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Action | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Kasa düzeyi arka uç işlemlerine ait kasa belirteci almak için kasa belirteci işlemi kullanılabilir. |
> | Action | Microsoft.RecoveryServices/Vaults/write | Kasa Oluştur işlemi, 'vault' türünde bir Azure kaynağı oluşturur |

## <a name="microsoftrelay"></a>Microsoft. Relay

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Relay/Checknameavaılabılıty/Action | Belirtilen abonelikte ad alanının kullanılabilirliğini denetler. |
> | Action | Microsoft. Relay/checkNamespaceAvailability/Action | Belirtilen abonelikte ad alanının kullanılabilirliğini denetler. Bu API kullanım dışı, lütfen bunun yerine Checknameavaılabılıty kullanın. |
> | Action | Microsoft. Relay/Namespace/authorizationRules/Action | Güncelleştirme ad alanı yetkilendirme kuralı. Bu API kullanım dışıdır. Lütfen bunun yerine ad alanı yetkilendirme kuralını güncelleştirmek için bir PUT çağrısı kullanın. Bu işlem API sürüm 2017-04-01 ' de desteklenmiyor. |
> | Action | Microsoft. Relay/Namespace/authorizationRules/Delete | Ad alanı yetkilendirme kuralını silin. Varsayılan ad alanı yetkilendirme kuralı silinemez.  |
> | Action | Microsoft. Relay/Namespace/authorizationRules/ListKeys/Action | Ad alanına bağlantı dizesi al |
> | Action | Microsoft. Relay/Namespace/authorizationRules/Read | Ad alanı yetkilendirme kuralları açıklaması listesini alın. |
> | Action | Microsoft. Relay/Namespace/authorizationRules/regenerateKeys/ACTION | Kaynağa birincil veya Ikincil anahtarı yeniden oluştur |
> | Action | Microsoft. Relay/Namespace/authorizationRules/Write | Ad alanı düzeyinde yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve Ikincil anahtarlar güncelleştirilebilen olabilir. |
> | Action | Microsoft. Relay/Namespace/Delete | Ad Alanı Kaynağını silin |
> | Action | Microsoft. Relay/namespaces/Disyıldız Recoveryconfigs/authorizationRules/ListKeys/Action | Olağanüstü durum kurtarma birincil ad alanı için yetkilendirme kuralları anahtarlarını alır |
> | Action | Microsoft. Relay/namespaces/Disyıldız Recoveryconfigs/authorizationRules/Read | Olağanüstü durum kurtarma birincil ad alanının yetkilendirme kurallarını al |
> | Action | Microsoft. Relay/namespaces/Diskalrecoveryconfigs/Breakeşleştirmeyi/eylemi | Olağanüstü durum kurtarmayı devre dışı bırakır ve birincil olan değişiklikleri ikincil ad alanlarına çoğaltmayı durduruyor. |
> | Action | Microsoft. Relay/namespaces/disyıldız recoveryconfigs/Checknameavaılabılıty/Action | Belirtilen abonelik altında ad alanı diğer adının kullanılabilirliğini denetler. |
> | Action | Microsoft. Relay/namespaces/Disyıldız Recoveryconfigs/Delete | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını siler. Bu işlem yalnızca birincil ad alanı aracılığıyla çağrılabilir. |
> | Action | Microsoft. Relay/namespaces/Disyıldız Recoveryconfigs/yük devretme/eylem | COĞRAFI bir DR yük devretmesini çağırır ve ad alanı diğer adını ikincil ad alanına işaret edecek şekilde yeniden yapılandırır. |
> | Action | Microsoft. Relay/namespaces/Disyıldız Recoveryconfigs/Read | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını alır. |
> | Action | Microsoft. Relay/namespaces/Disyıldız Recoveryconfigs/Write | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını oluşturur veya güncelleştirir. |
> | Action | Microsoft. Relay/Namespace/HybridConnections/authorizationRules/Action | HybridConnection 'ı güncelleştirme işlemi. Bu işlem API sürüm 2017-04-01 ' de desteklenmiyor. Yetkilendirme kuralları. Lütfen yetkilendirme kuralını güncelleştirmek için bir PUT çağrısı kullanın. |
> | Action | Microsoft. Relay/Namespace/HybridConnections/authorizationRules/Delete | HybridConnection yetkilendirme kurallarını silme işlemi |
> | Action | Microsoft. Relay/Namespace/HybridConnections/authorizationRules/ListKeys/Action | Bağlantı dizesini HybridConnection 'a al |
> | Action | Microsoft. Relay/Namespace/HybridConnections/authorizationRules/Read |  HybridConnection yetkilendirme kurallarının listesini alın |
> | Action | Microsoft. Relay/Namespace/HybridConnections/authorizationRules/regeneratekeys/Action | Kaynağa birincil veya Ikincil anahtarı yeniden oluştur |
> | Action | Microsoft. Relay/Namespace/HybridConnections/authorizationRules/Write | HybridConnection yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilen olabilir. |
> | Action | Microsoft. Relay/Namespace/HybridConnections/Delete | HybridConnection kaynağını silme işlemi |
> | Action | Microsoft. Relay/Namespace/HybridConnections/Read | HybridConnection kaynak açıklamaları listesini al |
> | Action | Microsoft. Relay/Namespace/HybridConnections/Write | HybridConnection özelliklerini oluşturun veya güncelleştirin. |
> | Action | Microsoft. Relay/namespaces/messagingPlan/okuma | Bir ad alanı için mesajlaşma planını alır.<br>Bu API kullanım dışıdır.<br>MessagingPlan kaynağı aracılığıyla sunulan özellikler sonraki API sürümlerindeki (üst) ad alanı kaynağına taşınır.<br>Bu işlem API sürüm 2017-04-01 ' de desteklenmiyor. |
> | Action | Microsoft. Relay/namespaces/messagingPlan/yazma | Bir ad alanı için mesajlaşma planını güncelleştirir.<br>Bu API kullanım dışıdır.<br>MessagingPlan kaynağı aracılığıyla sunulan özellikler sonraki API sürümlerindeki (üst) ad alanı kaynağına taşınır.<br>Bu işlem API sürüm 2017-04-01 ' de desteklenmiyor. |
> | Action | Microsoft. Relay/Namespace/operationresults/Read | Ad alanı işleminin durumunu al |
> | Action | Microsoft. Relay/namespaces/Providers/Microsoft. Insights/diagnosticSettings/Read | Ad alanı Tanılama ayarları kaynak açıklamalarının listesini al |
> | Action | Microsoft. Relay/namespaces/Providers/Microsoft. Insights/diagnosticSettings/Write | Ad alanı Tanılama ayarları kaynak açıklamalarının listesini al |
> | Action | Microsoft. Relay/namespaces/Providers/Microsoft. Insights/logDefinitions/Read | Ad alanı günlükleri kaynak açıklamalarının listesini al |
> | Action | Microsoft. Relay/namespaces/Providers/Microsoft. Insights/metricDefinitions/Read | Ad alanı ölçümleri kaynak açıklamalarının listesini al |
> | Action | Microsoft. Relay/Namespace/Read | Ad alanı kaynağı açıklaması listesini al |
> | Action | Microsoft. Relay/namespaces/removeAcsNamepsace/eylem | ACS ad alanını kaldırın |
> | Action | Microsoft. Relay/namespaces/Wcfreyerleştir/authorizationRules/Action | WcfRelay 'i güncelleştirme işlemi. Bu işlem API sürüm 2017-04-01 ' de desteklenmiyor. Yetkilendirme kuralları. Lütfen yetkilendirme kuralını güncelleştirmek için bir PUT çağrısı kullanın. |
> | Action | Microsoft. Relay/namespaces/Wcfreyerleştir/authorizationRules/Delete | WcfRelay yetkilendirme kurallarını silme işlemi |
> | Action | Microsoft. Relay/namespaces/Wcfreyerleştir/authorizationRules/ListKeys/Action | Bağlantı dizesini WcfRelay 'e al |
> | Action | Microsoft. Relay/namespaces/Wcfreyerleştir/authorizationRules/Read |  WcfRelay yetkilendirme kurallarının listesini alın |
> | Action | Microsoft. Relay/namespaces/Wcfreyerleştir/authorizationRules/regeneratekeys/ACTION | Kaynağa birincil veya Ikincil anahtarı yeniden oluştur |
> | Action | Microsoft. Relay/namespaces/Wcfrehera/authorizationRules/Write | WcfRelay yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilen olabilir. |
> | Action | Microsoft. Relay/namespaces/Wcfreyerleştir/Delete | WcfRelay kaynağını silme işlemi |
> | Action | Microsoft. Relay/namespaces/Wcfreyerleştir/Read | WcfRelay kaynak açıklamaları listesini al |
> | Action | Microsoft. Relay/namespaces/Wcfreyerleştir/Write | WcfRelay özelliklerini oluşturun veya güncelleştirin. |
> | Action | Microsoft. Relay/Namespace/Write | Bir ad alanı kaynağı oluşturun ve özelliklerini güncelleştirin. Ad alanının etiketleri ve kapasitesi, güncelleştirilebilen özelliklerdir. |
> | Action | Microsoft. Relay/işlemler/okuma | Alma İşlemleri |
> | Action | Microsoft. Relay/Register/ACTION | Geçiş kaynak sağlayıcısı için aboneliği kaydeder ve geçiş kaynaklarının oluşturulmasını sunar |
> | Action | Microsoft. Relay/Unregister/eylem | Geçiş kaynak sağlayıcısı için aboneliği kaydeder ve geçiş kaynaklarının oluşturulmasını sunar |

## <a name="microsoftresourcehealth"></a>Microsoft. ResourceHealth

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. ResourceHealth/kullanılabilirliği Bilitydurumlar/geçerli/okundu | Belirtilen kaynak için kullanılabilirlik durumunu alır |
> | Action | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Action | Microsoft. ResourceHealth/olayları/okuma | Belirtilen abonelik için hizmet durumu olaylarını al |
> | Action | Microsoft. Resourcehealth/healthevent/Action | Belirtilen kaynak için sistem durumunda değişiklik gösterir |
> | Action | Microsoft. Resourcehealth/healthevent/Activated/Action | Belirtilen kaynak için sistem durumunda değişiklik gösterir |
> | Action | Microsoft. Resourcehealth/healthevent/InProgress/Action | Belirtilen kaynak için sistem durumunda değişiklik gösterir |
> | Action | Microsoft. Resourcehealth/healthevent/bekliyor/Action | Belirtilen kaynak için sistem durumunda değişiklik gösterir |
> | Action | Microsoft. Resourcehealth/healthevent/çözüldü/Action | Belirtilen kaynak için sistem durumunda değişiklik gösterir |
> | Action | Microsoft. Resourcehealth/healthevent/Updated/Action | Belirtilen kaynak için sistem durumunda değişiklik gösterir |
> | Action | Microsoft. ResourceHealth/ımpactedresources/Read | Verilen abonelik için etkilenen kaynakları al |
> | Action | Microsoft. ResourceHealth/Metadata/Read | Meta verileri alır |
> | Action | Microsoft. ResourceHealth/Işlemler/okuma | Microsoft ResourceHealth için kullanılabilir işlemleri al |
> | Action | Microsoft. ResourceHealth/Register/ACTION | Microsoft ResourceHealth için aboneliği kaydeder |
> | Action | Microsoft. ResourceHealth/Unregister/ACTION | Microsoft ResourceHealth için aboneliğin kaydını siler |

## <a name="microsoftresources"></a>Microsoft. resources

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. resources/calculateTemplateHash/Action | Belirtilen şablonun karmasını hesaplayın. |
> | Action | Microsoft. resources/Checkpolicyuyumluluk/eylem | Kaynak ilkelerine karşı belirli bir kaynağın uyumluluk durumunu kontrol edin. |
> | Action | Microsoft. resources/checkResourceName/Action | Kaynak adını geçerlilik için denetleyin. |
> | Action | Microsoft. resources/dağıtımlar/iptal/eylem | Bir dağıtımı iptal eder. |
> | Action | Microsoft. resources/dağıtımlar/Sil | Bir dağıtımı siler. |
> | Action | Microsoft. resources/dağıtımlar/exportTemplate/Action | Dağıtım için şablonu dışarı aktarma |
> | Action | Microsoft. resources/dağıtımlar/işlemler/okuma | Dağıtım işlemlerini alır veya listeler. |
> | Action | Microsoft. resources/dağıtımlar/operationdurumlarının/Read | Dağıtım işlemi durumlarını alır veya listeler. |
> | Action | Microsoft. resources/dağıtımlar/okuma | Dağıtımları alır veya listeler. |
> | Action | Microsoft. resources/dağıtımlar/doğrulama/eylem | Bir dağıtımı doğrular. |
> | Action | Microsoft. resources/dağıtımlar/whatIf/Action | Şablon dağıtım değişikliklerini tahmin eder. |
> | Action | Microsoft. resources/dağıtımlar/yazma | Bir dağıtımı oluşturur veya güncelleştirir. |
> | Action | Microsoft. resources/bağlantılar/Sil | Bir kaynak bağlantısını siler. |
> | Action | Microsoft. resources/bağlantılar/okuma | Kaynak bağlantılarını alır veya listeler. |
> | Action | Microsoft. resources/bağlantılar/yazma | Kaynak bağlantısı oluşturur veya güncelleştirir. |
> | Action | Microsoft. resources/Market/satın alma/eylem | Market 'ten bir kaynak satın alır. |
> | Action | Microsoft. resources/sağlayıcılar/okuma | Sağlayıcıların listesini alın. |
> | Action | Microsoft. resources/kaynakları/okuma | Filtreleri temel alarak kaynak listesini alın. |
> | Action | Microsoft. resources/abonelikler/konumlar/okuma | Desteklenen konumların listesini alır. |
> | Action | Microsoft. resources/abonelikler/operationresults/Read | Abonelik işlem sonuçlarını alın. |
> | Action | Microsoft. resources/abonelikler/sağlayıcılar/okuma | Kaynak sağlayıcılarını alır veya listeler. |
> | Action | Microsoft. resources/abonelikler/okuma | Aboneliklerin listesini alır. |
> | Action | Microsoft. resources/abonelikler/resourceGroups/Delete | Bir kaynak grubunu ve tüm kaynaklarını siler. |
> | Action | Microsoft. resources/abonelikler/ResourceGroups/dağıtımlar/işlemler/okuma | Dağıtım işlemlerini alır veya listeler. |
> | Action | Microsoft. resources/abonelikler/ResourceGroups/dağıtımlar/operationdurumlarının/Read | Dağıtım işlemi durumlarını alır veya listeler. |
> | Action | Microsoft. resources/abonelikler/ResourceGroups/dağıtımlar/okuma | Dağıtımları alır veya listeler. |
> | Action | Microsoft. resources/abonelikler/ResourceGroups/dağıtımlar/Write | Bir dağıtımı oluşturur veya güncelleştirir. |
> | Action | Microsoft. resources/abonelikler/resourceGroups/Moveresonak/Action | Kaynakları bir kaynak grubundan diğerine taşıın. |
> | Action | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Action | Microsoft. resources/abonelikler/ResourceGroups/kaynaklar/okuma | Kaynak grubunun kaynaklarını alır. |
> | Action | Microsoft. resources/abonelikler/resourceGroups/Validatemoveresonak/Action | Kaynakları bir kaynak grubundan diğerine taşımayı doğrulayın. |
> | Action | Microsoft. resources/abonelikler/resourceGroups/Write | Bir kaynak grubu oluşturur veya güncelleştirir. |
> | Action | Microsoft. resources/abonelikler/kaynaklar/okuma | Bir aboneliğin kaynaklarını alır. |
> | Action | Microsoft. resources/abonelikler/tagNames/Delete | Bir abonelik etiketini siler. |
> | Action | Microsoft. resources/abonelikler/tagNames/Read | Abonelik etiketlerini alır veya listeler. |
> | Action | Microsoft. resources/abonelikler/tagNames/tagValues/Delete | Abonelik etiketi değerini siler. |
> | Action | Microsoft. resources/abonelikler/tagNames/tagValues/Read | Abonelik etiketi değerlerini alır veya listeler. |
> | Action | Microsoft. resources/abonelikler/tagNames/tagValues/Write | Abonelik etiketi değeri ekler. |
> | Action | Microsoft. resources/abonelikler/tagNames/Write | Abonelik etiketi ekler. |
> | Action | Microsoft. resources/etiketleri/silme | Bir kaynaktaki tüm etiketleri kaldırır. |
> | Action | Microsoft. resources/Tags/okuma | Bir kaynaktaki tüm etiketleri alır. |
> | Action | Microsoft. resources/Tags/Write | Varolan etiketleri yeni bir etiket kümesiyle değiştirerek veya birleştirerek ya da varolan etiketleri kaldırarak bir kaynaktaki etiketleri güncelleştirir. |
> | Action | Microsoft. resources/kiracılar/Read | Kiracılar listesini alır. |

## <a name="microsoftscheduler"></a>Microsoft. Scheduler

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Scheduler/jobcollections/Delete | İş koleksiyonunu siler. |
> | Action | Microsoft. Scheduler/jobcollections/Disable/Action | İş koleksiyonunu devre dışı bırakır. |
> | Action | Microsoft. Scheduler/jobcollections/Enable/Action | İş toplamayı etkinleştirilir. |
> | Action | Microsoft. Scheduler/jobcollections/Jobs/Delete | İşi siler. |
> | Action | Microsoft. Scheduler/jobcollections/Jobs/generateLogicAppDefinition/Action | Bir Zamanlayıcı Işini temel alarak mantıksal uygulama tanımı oluşturur. |
> | Action | Microsoft. Scheduler/jobcollections/Jobs/jobgeçmiş/Read | İş geçmişini alır. |
> | Action | Microsoft. Scheduler/jobcollections/Jobs/Read | İşi alır. |
> | Action | Microsoft. Scheduler/jobcollections/Jobs/Run/Action | İşi çalıştırır. |
> | Action | Microsoft. Scheduler/jobcollections/Jobs/Write | İş oluşturur veya güncelleştirir. |
> | Action | Microsoft. Scheduler/jobcollections/Read | İş Koleksiyonu Al |
> | Action | Microsoft. Scheduler/jobcollections/Write | İş koleksiyonu oluşturur veya güncelleştirir. |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Search/Checknameavaılabılıty/Action | Hizmet adının kullanılabilirliğini denetler. |
> | Action | Microsoft. Search/işlemler/okuma | Microsoft. Search sağlayıcısı 'nın tüm kullanılabilir işlemlerini listeler. |
> | Action | Microsoft. Search/Register/ACTION | Arama kaynak sağlayıcısı için aboneliği kaydeder ve arama hizmetleri oluşturmayı sunar. |
> | Action | Microsoft. Search/searchServices/createQueryKey/Action | Sorgu anahtarını oluşturur. |
> | Action | Microsoft. Search/searchServices/Delete | Arama hizmetini siler. |
> | Action | Microsoft. Search/searchServices/deleteQueryKey/Delete | Sorgu anahtarını siler. |
> | Action | Microsoft. Search/searchServices/listAdminKeys/Action | Yönetici anahtarlarını okur. |
> | Action | Microsoft. Search/searchServices/listQueryKeys/Action | Verilen Azure Search hizmeti için sorgu API 'SI anahtarlarının listesini döndürür. |
> | Action | Microsoft. Search/searchServices/listQueryKeys/Read | Verilen Azure Search hizmeti için sorgu API 'SI anahtarlarının listesini döndürür. |
> | Action | Microsoft. Search/searchServices/Read | Arama hizmetini okur. |
> | Action | Microsoft. Search/searchServices/regenerateAdminKey/Action | Yönetici anahtarını yeniden oluşturur. |
> | Action | Microsoft. Search/searchServices/start/Action | Arama hizmetini başlatır. |
> | Action | Microsoft. Search/searchServices/durdur/eylem | Arama hizmetini sonlandırır. |
> | Action | Microsoft. Search/searchServices/Write | Arama hizmetini oluşturur veya güncelleştirir. |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Security/adaptiveNetworkHardenings/enforce/Action | Verilen ağ güvenlik grupları üzerinde eşleşen güvenlik kuralları oluşturarak verilen trafik sağlamlaştırma kurallarını zorlar |
> | Action | Microsoft. Security/adaptiveNetworkHardenings/Read | Azure korumalı bir kaynağın Uyarlamalı ağ sağlamlaştırma önerilerini alır |
> | Action | Microsoft. Security/advancedThreatProtectionSettings/Read | Kaynak için Gelişmiş tehdit koruması ayarlarını alır |
> | Action | Microsoft. Security/advancedThreatProtectionSettings/Write | Kaynak için Gelişmiş tehdit koruması ayarlarını güncelleştirir |
> | Action | Microsoft. Security/alerts/Read | Tüm kullanılabilir güvenlik uyarılarını alır |
> | Action | Microsoft. Security/Applicationwhitedökümler/okuma | Uygulama beyaz listelerini alır |
> | Action | Microsoft. Security/Applicationwhitedökümler/Write | Yeni bir uygulama beyaz listeye oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. Security/Karmaşıkanceresults/okuma | Kaynak için uyumluluk sonuçlarını alır |
> | Action | Microsoft. Security/ınformationprotectionpolicies/Read | Kaynak için bilgi koruma ilkelerini alır |
> | Action | Microsoft. Security/ınformationprotectionpolicies/Write | Kaynak için bilgi koruma ilkelerini güncelleştirir |
> | Action | Microsoft. güvenlik/konumlar/uyarılar/etkinleştir/eylem | Güvenlik uyarısını etkinleştirme |
> | Action | Microsoft. güvenlik/konumlar/uyarılar/kapat/eylem | Güvenlik uyarısını kapat |
> | Action | Microsoft. güvenlik/konumlar/uyarılar/okuma | Tüm kullanılabilir güvenlik uyarılarını alır |
> | Action | Microsoft. Security/Locations/Jağaccesspolicies/Delete | Tam zamanında ağ erişim ilkesini siler |
> | Action | Microsoft. Security/Locations/Jağaccesspolicies/Initiate/Action | Tam zamanında ağ erişim ilkesi isteği başlatır |
> | Action | Microsoft. Security/Locations/Jağaccesspolicies/Read | Tam zamanında ağ erişim ilkelerini alır |
> | Action | Microsoft. Security/Locations/Jağaccesspolicies/Write | Yeni bir tam zamanında ağ erişim ilkesi oluşturur veya var olan bir ağı güncelleştirir |
> | Action | Microsoft. Security/Locations/Read | Güvenlik veri konumunu alır |
> | Action | Microsoft. güvenlik/konumlar/görevler/etkinleştir/eylem | Güvenlik önerisi etkinleştirme |
> | Action | Microsoft.Security/locations/tasks/dismiss/action | Güvenlik önerisini kapat |
> | Action | Microsoft. güvenlik/konumlar/görevler/okuma | Tüm kullanılabilir güvenlik önerilerini alır |
> | Action | Microsoft. güvenlik/konumlar/görevler/çözümle/eylem | Güvenlik önerisini çözümleyin |
> | Action | Microsoft. güvenlik/konumlar/görevler/Başlat/eylem | Güvenlik önerisi başlatma |
> | Action | Microsoft. Security/Policies/Read | Güvenlik ilkesini alır |
> | Action | Microsoft. Security/Policies/Write | Güvenlik ilkesini güncelleştirir |
> | Action | Microsoft. Security/pricler/Delete | Kapsamın fiyatlandırma ayarlarını siler |
> | Action | Microsoft. Security/pricler/okuma | Kapsamın fiyatlandırma ayarlarını alır |
> | Action | Microsoft. Security/pricler/Write | Kapsamın fiyatlandırma ayarlarını güncelleştirir |
> | Action | Microsoft. Security/Register/ACTION | Azure Güvenlik Merkezi için aboneliği kaydeder |
> | Action | Microsoft. Security/securityContacts/Delete | Güvenlik ilgili kişisini siler |
> | Action | Microsoft. Security/securityContacts/Read | Güvenlik ilgili kişisini alır |
> | Action | Microsoft. Security/securityContacts/Write | Güvenlik ilgili kişisini güncelleştirir |
> | Action | Microsoft. Security/securitySolutions/Delete | Bir güvenlik çözümünü siler |
> | Action | Microsoft. Security/securitySolutions/Read | Güvenlik çözümlerini alır |
> | Action | Microsoft. Security/securitySolutions/Write | Yeni bir güvenlik çözümü oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. Security/securitySolutionsReferenceData/Read | Güvenlik çözümleri başvuru verilerini alır |
> | Action | Microsoft. Security/Securitydurumlar/okuma | Azure kaynakları için güvenlik durumu durumlarını alır |
> | Action | Microsoft. Security/securityStatusesSummaries/Read | Kapsam için güvenlik durumları özetlerini alır |
> | Action | Microsoft. Security/Settings/Read | Kapsamın ayarlarını alır |
> | Action | Microsoft. Security/Settings/Write | Kapsamın ayarlarını güncelleştirir |
> | Action | Microsoft. Security/Tasks/Read | Tüm kullanılabilir güvenlik önerilerini alır |
> | Action | Microsoft. Security/Unregister/ACTION | Azure Güvenlik Merkezi ' nden aboneliğin kaydını siler |
> | Action | Microsoft. Security/Webapplicationgüvenlik duvarları/silme | Web uygulaması güvenlik duvarını siler |
> | Action | Microsoft. Security/Webapplicationgüvenlik duvarları/okuma | Web uygulaması güvenlik duvarlarını alır |
> | Action | Microsoft. Security/Webapplicationgüvenlik duvarları/yazma | Yeni bir Web uygulaması güvenlik duvarı oluşturur veya mevcut olanı güncelleştirir |
> | Action | Microsoft. Security/Workspace ayarları/Connect/Action | Çalışma alanı ayarlarının yeniden bağlanma ayarlarını değiştir |
> | Action | Microsoft. Security/Workspace ayarları/silme | Çalışma alanı ayarlarını siler |
> | Action | Microsoft. Security/Workspace ayarları/okuma | Çalışma alanı ayarlarını alır |
> | Action | Microsoft. Security/Workspace ayarları/yazma | Çalışma alanı ayarlarını güncelleştirir |

## <a name="microsoftsecuritygraph"></a>Microsoft. SecurityGraph

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. SecurityGraph/diagnosticsettings/Delete | Bir tanılama ayarını silme |
> | Action | Microsoft. SecurityGraph/diagnosticsettings/Read | Tanılama ayarını okuma |
> | Action | Microsoft. SecurityGraph/diagnosticsettings/Write | Tanılama ayarı yazma |
> | Action | Microsoft. SecurityGraph/diagnosticsettingscategories/Read | Tanılama ayarı kategorilerini okuma |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. ServiceBus/Checknameavaılabılıty/Action | Belirtilen abonelikte ad alanının kullanılabilirliğini denetler. |
> | Action | Microsoft. ServiceBus/checkNamespaceAvailability/Action | Belirtilen abonelikte ad alanının kullanılabilirliğini denetler. Bu API kullanım dışı, lütfen bunun yerine Checknameavaılabılıty kullanın. |
> | Action | Microsoft. ServiceBus/Locations/Deletevirtualnetworkoralt ağları/eylem | Belirtilen VNet için ServiceBus kaynak sağlayıcısında VNet kurallarını siler |
> | Action | Microsoft. ServiceBus/namespaces/authorizationRules/Action | Güncelleştirme ad alanı yetkilendirme kuralı. Bu API kullanım dışıdır. Lütfen bunun yerine ad alanı yetkilendirme kuralını güncelleştirmek için bir PUT çağrısı kullanın. Bu işlem API sürüm 2017-04-01 ' de desteklenmiyor. |
> | Action | Microsoft. ServiceBus/Namespace/authorizationRules/Delete | Ad alanı yetkilendirme kuralını silin. Varsayılan ad alanı yetkilendirme kuralı silinemez.  |
> | Action | Microsoft. ServiceBus/Namespace/authorizationRules/ListKeys/Action | Ad alanına bağlantı dizesi al |
> | Action | Microsoft. ServiceBus/Namespace/authorizationRules/Read | Ad alanı yetkilendirme kuralları açıklaması listesini alın. |
> | Action | Microsoft. ServiceBus/Namespace/authorizationRules/regenerateKeys/ACTION | Kaynağa birincil veya Ikincil anahtarı yeniden oluştur |
> | Action | Microsoft. ServiceBus/Namespace/authorizationRules/Write | Ad alanı düzeyinde yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları, birincil ve Ikincil anahtarlar güncelleştirilebilen olabilir. |
> | Action | Microsoft. ServiceBus/Namespace/Delete | Ad Alanı Kaynağını silin |
> | Action | Microsoft. ServiceBus/namespaces/Diskalrecoveryconfigs/authorizationRules/ListKeys/Action | Olağanüstü durum kurtarma birincil ad alanı için yetkilendirme kuralları anahtarlarını alır |
> | Action | Microsoft. ServiceBus/namespaces/Diskalrecoveryconfigs/authorizationRules/Read | Olağanüstü durum kurtarma birincil ad alanının yetkilendirme kurallarını al |
> | Action | Microsoft. ServiceBus/namespaces/Diskalrecoveryconfigs/Breakeşleştirmeyi/eylemini | Olağanüstü durum kurtarmayı devre dışı bırakır ve birincil olan değişiklikleri ikincil ad alanlarına çoğaltmayı durduruyor. |
> | Action | Microsoft. ServiceBus/namespaces/diskalrecoveryconfigs/Checknameavaılabılıty/Action | Belirtilen abonelik altında ad alanı diğer adının kullanılabilirliğini denetler. |
> | Action | Microsoft. ServiceBus/namespaces/Diskalrecoveryconfigs/Delete | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını siler. Bu işlem yalnızca birincil ad alanı aracılığıyla çağrılabilir. |
> | Action | Microsoft. ServiceBus/namespaces/Diskalrecoveryconfigs/yük devretme/eylem | COĞRAFI bir DR yük devretmesini çağırır ve ad alanı diğer adını ikincil ad alanına işaret edecek şekilde yeniden yapılandırır. |
> | Action | Microsoft. ServiceBus/namespaces/Diskalrecoveryconfigs/Read | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını alır. |
> | Action | Microsoft. ServiceBus/namespaces/Diskalrecoveryconfigs/Write | Ad alanıyla ilişkili olağanüstü durum kurtarma yapılandırmasını oluşturur veya güncelleştirir. |
> | Action | Microsoft. ServiceBus/namespaces/Eventgridfilter/Delete | Ad alanıyla ilişkili Event Grid filtresini siler. |
> | Action | Microsoft. ServiceBus/namespaces/Eventgridfilter/Read | Ad alanıyla ilişkili Event Grid filtresini alır. |
> | Action | Microsoft. ServiceBus/namespaces/Eventgridfilter/Write | Ad alanıyla ilişkili Event Grid filtresini oluşturur veya güncelleştirir. |
> | Action | Microsoft. ServiceBus/Namespace/eventhubs/Read | EventHub kaynak açıklamalarının listesini al |
> | Action | Microsoft. ServiceBus/namespaces/ıpfilterrules/Delete | IP Filtresi Kaynağını Sil |
> | Action | Microsoft. ServiceBus/namespaces/ıpfilterrules/Read | IP Filtresi Kaynağını Al |
> | Action | Microsoft. ServiceBus/namespaces/ıpfilterrules/Write | IP Filtresi Kaynağı Oluştur |
> | DataAction | Microsoft. ServiceBus/namespaces/iletiler/Receive/Action | İleti alma |
> | DataAction | Microsoft. ServiceBus/ad alanları/iletiler/gönderme/eylem | İleti gönderme |
> | Action | Microsoft. ServiceBus/namespaces/messagingPlan/okuma | Bir ad alanı için mesajlaşma planını alır.<br>Bu API kullanım dışıdır.<br>MessagingPlan kaynağı aracılığıyla sunulan özellikler sonraki API sürümlerindeki (üst) ad alanı kaynağına taşınır.<br>Bu işlem API sürüm 2017-04-01 ' de desteklenmiyor. |
> | Action | Microsoft. ServiceBus/namespaces/messagingPlan/yazma | Bir ad alanı için mesajlaşma planını güncelleştirir.<br>Bu API kullanım dışıdır.<br>MessagingPlan kaynağı aracılığıyla sunulan özellikler sonraki API sürümlerindeki (üst) ad alanı kaynağına taşınır.<br>Bu işlem API sürüm 2017-04-01 ' de desteklenmiyor. |
> | Action | Microsoft. ServiceBus/namespaces/Migrate/Action | Ad alanı geçiş işlemi |
> | Action | Microsoft. ServiceBus/namespaces/migrationConfigurations/Delete | Geçiş yapılandırmasını siler. |
> | Action | Microsoft. ServiceBus/ad alanları/migrationConfigurations/okuma | Geçişin durumunu ve bekleyen çoğaltma işlemlerini gösteren geçiş yapılandırmasını alır |
> | Action | Microsoft. ServiceBus/namespaces/migrationConfigurations/döndürülüyor/Action | Standart, Premium ad alanı geçişine geri döndürür |
> | Action | Microsoft. ServiceBus/namespaces/migrationConfigurations/Upgrade/Action | Standart ad alanı ile ilişkili DNS 'yi, geçişi tamamlayan Premium ad alanına ve standart olan kaynakları Standart ve Premium ad alanına eşitlemeyi durduran şekilde atar |
> | Action | Microsoft. ServiceBus/namespaces/migrationConfigurations/Write | Geçiş yapılandırması oluşturur veya güncelleştirir. Bu işlem, kaynakları standart olan Premium ad alanına eşitlemeye başlar |
> | Action | Microsoft. ServiceBus/namespaces/networkruleset/Delete | Sanal Ağ Kuralı Kaynağını Sil |
> | Action | Microsoft. ServiceBus/namespaces/networkruleset/Read | NetworkRuleSet kaynağını alır |
> | Action | Microsoft. ServiceBus/namespaces/networkruleset/Write | Sanal Ağ Kuralı Kaynağı Oluştur |
> | Action | Microsoft. ServiceBus/namespaces/networkrulesets/Delete | Sanal Ağ Kuralı Kaynağını Sil |
> | Action | Microsoft. ServiceBus/namespaces/networkrulesets/Read | NetworkRuleSet kaynağını alır |
> | Action | Microsoft. ServiceBus/namespaces/networkrulesets/Write | Sanal Ağ Kuralı Kaynağı Oluştur |
> | Action | Microsoft. ServiceBus/Namespace/operationresults/Read | Ad alanı işleminin durumunu al |
> | Action | Microsoft. ServiceBus/namespaces/Providers/Microsoft. Insights/diagnosticSettings/Read | Ad alanı Tanılama ayarları kaynak açıklamalarının listesini al |
> | Action | Microsoft. ServiceBus/namespaces/Providers/Microsoft. Insights/diagnosticSettings/Write | Ad alanı Tanılama ayarları kaynak açıklamalarının listesini al |
> | Action | Microsoft. ServiceBus/namespaces/Providers/Microsoft. Insights/logDefinitions/Read | Ad alanı günlükleri kaynak açıklamalarının listesini al |
> | Action | Microsoft. ServiceBus/namespaces/Providers/Microsoft. Insights/metricDefinitions/Read | Ad alanı ölçümleri kaynak açıklamalarının listesini al |
> | Action | Microsoft. ServiceBus/namespaces/kuyruklar/authorizationRules/Action | Kuyruğu güncelleştirme işlemi. Bu işlem API sürüm 2017-04-01 ' de desteklenmiyor. Yetkilendirme kuralları. Lütfen yetkilendirme kuralını güncelleştirmek için bir PUT çağrısı kullanın. |
> | Action | Microsoft. ServiceBus/namespaces/kuyruklar/authorizationRules/Delete | Kuyruk yetkilendirme kurallarını silme işlemi |
> | Action | Microsoft. ServiceBus/namespaces/kuyruklar/authorizationRules/ListKeys/Action | Sıraya yönelik bağlantı dizesini al |
> | Action | Microsoft. ServiceBus/namespaces/kuyruklar/authorizationRules/Read |  Kuyruk yetkilendirme kurallarının listesini alın |
> | Action | Microsoft. ServiceBus/namespaces/kuyruklar/authorizationRules/regenerateKeys/ACTION | Kaynağa birincil veya Ikincil anahtarı yeniden oluştur |
> | Action | Microsoft. ServiceBus/namespaces/kuyruklar/authorizationRules/Write | Sıra yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilen olabilir. |
> | Action | Microsoft. ServiceBus/namespaces/kuyruklar/Sil | Kuyruk kaynağını silme işlemi |
> | Action | Microsoft. ServiceBus/namespaces/kuyruklar/okuma | Kuyruk kaynağı açıklamalarının listesini al |
> | Action | Microsoft. ServiceBus/namespaces/kuyruklar/Write | Sıra özelliklerini oluşturun veya güncelleştirin. |
> | Action | Microsoft. ServiceBus/Namespace/Read | Ad alanı kaynağı açıklaması listesini al |
> | Action | Microsoft. ServiceBus/namespaces/removeAcsNamepsace/eylem | ACS ad alanını kaldırın |
> | Action | Microsoft. ServiceBus/namespaces/konular/authorizationRules/Action | Konuyu güncelleştirmek için işlem. Bu işlem API sürüm 2017-04-01 ' de desteklenmiyor. Yetkilendirme kuralları. Lütfen yetkilendirme kuralını güncelleştirmek için bir PUT çağrısı kullanın. |
> | Action | Microsoft. ServiceBus/namespaces/konular/authorizationRules/Delete | Konu yetkilendirme kurallarını silme işlemi |
> | Action | Microsoft. ServiceBus/namespaces/konular/authorizationRules/ListKeys/Action | Bağlantı dizesini konuya al |
> | Action | Microsoft. ServiceBus/namespaces/konular/authorizationRules/Read |  Konu yetkilendirme kurallarının listesini alın |
> | Action | Microsoft. ServiceBus/namespaces/konular/authorizationRules/regenerateKeys/ACTION | Kaynağa birincil veya Ikincil anahtarı yeniden oluştur |
> | Action | Microsoft. ServiceBus/namespaces/konular/authorizationRules/Write | Konu yetkilendirme kuralları oluşturun ve özelliklerini güncelleştirin. Yetkilendirme kuralları erişim hakları güncelleştirilebilen olabilir. |
> | Action | Microsoft. ServiceBus/namespaces/konular/Sil | Konu kaynağını silme işlemi |
> | Action | Microsoft. ServiceBus/namespaces/konular/okuma | Konu kaynağı açıklamalarının listesini al |
> | Action | Microsoft. ServiceBus/namespaces/konular/abonelikler/Sil | TopicSubscription kaynağını silme işlemi |
> | Action | Microsoft. ServiceBus/namespaces/konular/abonelikler/okuma | TopicSubscription kaynak açıklamalarının listesini al |
> | Action | Microsoft. ServiceBus/namespaces/konular/abonelikler/kurallar/Sil | Kural kaynağını silme işlemi |
> | Action | Microsoft. ServiceBus/namespaces/konular/abonelikler/kurallar/okuma | Kural kaynağı açıklamalarının listesini al |
> | Action | Microsoft. ServiceBus/namespaces/konular/abonelikler/kurallar/yazma | Kural özelliklerini oluşturun veya güncelleştirin. |
> | Action | Microsoft. ServiceBus/namespaces/konular/abonelikler/yazma | TopicSubscription özelliklerini oluşturun veya güncelleştirin. |
> | Action | Microsoft. ServiceBus/namespaces/konular/yaz | Konu özelliklerini oluşturun veya güncelleştirin. |
> | Action | Microsoft. ServiceBus/Namespace/virtualNetworkRules/Delete | Sanal Ağ Kuralı Kaynağını Sil |
> | Action | Microsoft. ServiceBus/Namespace/virtualNetworkRules/Read | Sanal Ağ Kuralı Kaynağını alır |
> | Action | Microsoft. ServiceBus/Namespace/virtualNetworkRules/Write | Sanal Ağ Kuralı Kaynağı Oluştur |
> | Action | Microsoft. ServiceBus/Namespace/Write | Bir ad alanı kaynağı oluşturun ve özelliklerini güncelleştirin. Ad alanının etiketleri ve kapasitesi, güncelleştirilebilen özelliklerdir. |
> | Action | Microsoft. ServiceBus/işlemler/okuma | Alma İşlemleri |
> | Action | Microsoft. ServiceBus/Register/ACTION | ServiceBus kaynak sağlayıcısı için aboneliği kaydeder ve ServiceBus kaynaklarının oluşturulmasını sunar |
> | Action | Microsoft. ServiceBus/SKU/okuma | SKU kaynak açıklamaları listesini al |
> | Action | Microsoft. ServiceBus/SKU/bölge/okuma | Skuregion kaynak açıklamalarının listesini al |
> | Action | Microsoft. ServiceBus/Unregister/ACTION | ServiceBus kaynak sağlayıcısı için aboneliği kaydeder ve ServiceBus kaynaklarının oluşturulmasını sunar |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. ServiceFabric/kümeler/uygulamalar/Sil | Herhangi Bir Uygulamayı Silin |
> | Action | Microsoft. ServiceFabric/kümeler/uygulamalar/okuma | Herhangi Bir Uygulamayı Okuyun |
> | Action | Microsoft. ServiceFabric/kümeler/uygulamalar/hizmetler/Sil | Herhangi Bir Hizmeti Silin |
> | Action | Microsoft. ServiceFabric/kümeler/uygulamalar/hizmetler/bölümler/okuma | Herhangi Bir Bölümü Okuyun |
> | Action | Microsoft. ServiceFabric/kümeler/uygulamalar/hizmetler/bölümler/çoğaltmalar/okuma | Herhangi Bir Çoğaltmayı Okuyun |
> | Action | Microsoft. ServiceFabric/kümeler/uygulamalar/hizmetler/okuma | Herhangi Bir Hizmeti Okuyun |
> | Action | Microsoft. ServiceFabric/kümeler/uygulamalar/hizmetler/durumlar/okuma | Tüm Hizmet Durumlarını Oku |
> | Action | Microsoft. ServiceFabric/kümeler/uygulamalar/hizmetler/yazma | Herhangi Bir Hizmet Oluşturun veya Güncelleştirin |
> | Action | Microsoft. ServiceFabric/kümeler/uygulamalar/yazma | Herhangi Bir Uygulama Oluşturun veya Güncelleştirin |
> | Action | Microsoft. ServiceFabric/kümeler/applicationTypes/Delete | Herhangi Bir Uygulama Türü Silin |
> | Action | Microsoft. ServiceFabric/kümeler/applicationTypes/Read | Herhangi Bir Uygulama Türünü Okuyun |
> | Action | Microsoft. ServiceFabric/kümeler/applicationTypes/sürümler/Delete | Herhangi Bir Uygulama Türü Sürümü Silin |
> | Action | Microsoft. ServiceFabric/kümeler/applicationTypes/sürümler/Read | Herhangi Bir Uygulama Türü Sürümünü Okuyun |
> | Action | Microsoft. ServiceFabric/kümeler/applicationTypes/sürümler/Write | Herhangi bir uygulama türü sürümü oluşturun veya güncelleştirin |
> | Action | Microsoft. ServiceFabric/kümeler/applicationTypes/Write | Herhangi bir uygulama türü oluşturun veya güncelleştirin |
> | Action | Microsoft. ServiceFabric/kümeler/Sil | Herhangi Bir Kümeyi Silin |
> | Action | Microsoft. ServiceFabric/kümeler/düğümler/okuma | Herhangi Bir Düğümü Okuyun |
> | Action | Microsoft. ServiceFabric/kümeler/okuma | Herhangi Bir Kümeyi Okuyun |
> | Action | Microsoft. ServiceFabric/kümeler/durumlar/okuma | Tüm Küme Durumlarını Oku |
> | Action | Microsoft. ServiceFabric/kümeler/yazma | Herhangi Bir Kümeyi Oluşturun veya Güncelleştirin |
> | Action | Microsoft. ServiceFabric/konumlar/Kümesürümleri/okuma | Herhangi bir Küme Sürümünü oku |
> | Action | Microsoft. ServiceFabric/konumlar/ortamlar/Kümesürümleri/okuma | Belirli bir ortam için herhangi bir küme sürümünü okuyun |
> | Action | Microsoft. ServiceFabric/konumlar/operationresults/Read | Herhangi Bir İşlem Sonucunu Okuyun |
> | Action | Microsoft. ServiceFabric/konumlar/işlemler/okuma | Konuma göre herhangi bir İşlemi okuyun |
> | Action | Microsoft. ServiceFabric/işlemler/okuma | Kullanılabilen Herhangi Bir İşlemi Okuyun |
> | Action | Microsoft. ServiceFabric/Register/ACTION | Herhangi Bir Eylemi Kaydedin |

## <a name="microsoftsignalrservice"></a>Microsoft. SignalRService

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. SignalRService/Locations/checknameavaılabılıty/Action | Bir adın yeni bir SignalR hizmeti ile kullanım için kullanılabilir olup olmadığını denetler |
> | Action | Microsoft. SignalRService/Locations/operationresults/SignalR/Read | Zaman uyumsuz bir işlemin durumunu sorgulama |
> | Action | Microsoft. SignalRService/Locations/Operationdurumlarının/operationId/Read | Zaman uyumsuz bir işlemin durumunu sorgulama |
> | Action | Microsoft. SignalRService/konumlar/kullanımlar/okuma | Azure SignalR hizmeti için kota kullanımlarını edinme |
> | Action | Microsoft. SignalRService/operationresults/Read | Zaman uyumsuz bir işlemin durumunu sorgulama |
> | Action | Microsoft. SignalRService/OperationStatus/Read | Zaman uyumsuz bir işlemin durumunu sorgulama |
> | Action | Microsoft. SignalRService/Register/Action | ' Microsoft. SignalRService ' kaynak sağlayıcısını bir abonelikle kaydeder |
> | Action | Microsoft. SignalRService/SignalR/Delete | SignalR hizmeti 'nin tamamını silme |
> | Action | Microsoft. SignalRService/SignalR/eventGridFilters/Delete | Bir SignalR 'den Event Grid filtresini silin. |
> | Action | Microsoft. SignalRService/SignalR/eventGridFilters/Read | Belirtilen bir olay Kılavuzu filtresinin özelliklerini alın veya belirtilen SignalR için tüm olay Kılavuzu filtrelerini listeleyin. |
> | Action | Microsoft. SignalRService/SignalR/eventGridFilters/Write | Bir SignalR için belirtilen parametrelerle bir olay Kılavuzu filtresi oluşturun veya güncelleştirin. |
> | Action | Microsoft. SignalRService/SignalR/ListKeys/Action | Yönetim portalında veya API aracılığıyla SignalR erişim anahtarlarının değerini görüntüleme |
> | Action | Microsoft. SignalRService/SignalR/Read | SignalR 'nin ayarlarını ve yapılandırmasını yönetim portalında veya API aracılığıyla görüntüleme |
> | Action | Microsoft. SignalRService/SignalR/RegenerateKey/Action | Yönetim portalında veya API aracılığıyla SignalR erişim anahtarlarının değerini değiştirme |
> | Action | Microsoft. SignalRService/SignalR/restart/Action | Yönetim portalında veya API aracılığıyla bir Azure SignalR hizmetini yeniden başlatmak için. Belirli kapalı kalma süresi olacaktır. |
> | Action | Microsoft. SignalRService/SignalR/Write | SignalR 'nin ayarlarını ve yapılandırmasını yönetim portalında veya API aracılığıyla değiştirme |
> | Action | Microsoft. SignalRService/Unregister/eylem | ' Microsoft. SignalRService ' kaynak sağlayıcısının aboneliğini bir abonelikle siler |

## <a name="microsoftsolutions"></a>Microsoft. Solutions

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Solutions/applicationDefinitions/Applicationyapay/Read | Uygulama tanımının uygulama yapılarını listeler. |
> | Action | Microsoft. Solutions/applicationDefinitions/Delete | Bir uygulama tanımını kaldırır. |
> | Action | Microsoft. Solutions/applicationDefinitions/Read | Uygulama tanımlarının bir listesini alır. |
> | Action | Microsoft. Solutions/applicationDefinitions/Write | Uygulama tanımı ekleme veya değiştirme. |
> | Action | Microsoft. Solutions/Applications/applicationArtifacts/Read | Uygulama yapılarını listeler. |
> | Action | Microsoft. Solutions/Applications/Delete | Bir uygulamayı kaldırır. |
> | Action | Microsoft. Solutions/Applications/Read | Uygulamaların bir listesini alır. |
> | Action | Microsoft. Solutions/Applications/refreshPermissions/Action | Uygulama izinlerini yeniler. |
> | Action | Microsoft. Solutions/Applications/updateAccess/Action | Uygulama erişimini güncelleştirir. |
> | Action | Microsoft. Solutions/Applications/Write | Uygulama oluşturur. |
> | Action | Microsoft. Solutions/Jrequests/Delete | Bir JitRequest'i kaldır |
> | Action | Microsoft. Solutions/Jrequests/Read | JitRequest listesi alır |
> | Action | Microsoft. Solutions/Jrequests/Write | Bir JitRequest oluşturur |
> | Action | Microsoft. Solutions/Locations/Operationdurumlarının/Read | Kaynak için işlem durumunu okur. |
> | Action | Microsoft. Solutions/Register/ACTION | Çözümlere kaydolun. |
> | Action | Microsoft. Solutions/Unregister/Action | Çözümlerdeki kaydını siler. |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. SQL/Checknameavaılabılıty/Action | Belirli bir abonelik için verilen sunucu adının dünya genelinde sağlanması için kullanılabilir olup olmadığını doğrulayın. |
> | Action | Microsoft. SQL/ınstancehavuzlar/Delete | Bir örnek havuzunu siler |
> | Action | Microsoft. SQL/ınstancepools/okuma | Bir örnek havuzu alır |
> | Action | Microsoft. SQL/ınstancepools/kullanımlar/okuma | Örnek havuzunun kullanım bilgilerini alır |
> | Action | Microsoft. SQL/ınstancehavuzlar/Write | Örnek havuzu oluşturur veya güncelleştirir |
> | Action | Microsoft. SQL/Locations/auditingSettingsAzureAsyncOperation/Read | Genişletilmiş sunucu blobu denetim ilkesi ayarlama işleminin sonucunu al |
> | Action | Microsoft. SQL/Locations/auditingSettingsOperationResults/Read | Sunucu blobu denetim ilkesi ayarlama işleminin sonucunu al |
> | Action | Microsoft. SQL/konumlar/yetenekler/okuma | Belirli bir konumdaki bu aboneliğin yeteneklerini alır |
> | Action | Microsoft. SQL/konumlar/databaseAzureAsyncOperation/Read | Bir veritabanı işleminin durumunu alır. |
> | Action | Microsoft. SQL/konumlar/databaseOperationResults/Read | Bir veritabanı işleminin durumunu alır. |
> | Action | Microsoft. SQL/konumlar/deletedServerAsyncOperation/Read | Silinen sunucuda devam eden işlemleri alır |
> | Action | Microsoft. SQL/Locations/Silinserveroperationresults/Read | Silinen sunucuda devam eden işlemleri alır |
> | Action | Microsoft. SQL/konumlar/deletedServers/okuma | Silinen sunucuların listesini döndürün veya belirtilen silinen sunucu için özellikleri alır. |
> | Action | Microsoft. SQL/konumlar/silinmeler/kurtarma/eylem | Silinen bir sunucuyu kurtarma |
> | Action | Microsoft. SQL/konumlar/elasticPoolAzureAsyncOperation/Read | Elastik havuz zaman uyumsuz işlemi için Azure zaman uyumsuz işlemini alır |
> | Action | Microsoft. SQL/Locations/Elayapışpooloperationresults/Read | Elastik havuz işleminin sonucunu alır. |
> | Action | Microsoft. SQL/Locations/Encryptionkorunabilir/Read | Saydam veri şifrelemesi şifreleme koruyucusuyla devam eden işlemleri alır |
> | Action | Microsoft. SQL/Locations/Encryptionkorunabilir sonuçları/okuma | Saydam veri şifrelemesi şifreleme koruyucusuyla devam eden işlemleri alır |
> | Action | Microsoft. SQL/konumlar/extendedAuditingSettingsAzureAsyncOperation/Read | Genişletilmiş sunucu blobu denetim ilkesi ayarlama işleminin sonucunu al |
> | Action | Microsoft. SQL/konumlar/extendedAuditingSettingsOperationResults/Read | Genişletilmiş sunucu blobu denetim ilkesi ayarlama işleminin sonucunu al |
> | Action | Microsoft. SQL/Locations/firewallRulesAzureAsyncOperation/Read | Bir güvenlik duvarı kuralı işleminin durumunu alır. |
> | Action | Microsoft. SQL/Locations/firewallRulesOperationResults/Read | Bir güvenlik duvarı kuralı işleminin durumunu alır. |
> | Action | Microsoft. SQL/Locations/ınstancefailovergroups/Delete | Var olan bir örnek yük devretme grubunu siler. |
> | Action | Microsoft. SQL/Locations/ınstancefailovergroups/yük devretme/eylem | Mevcut bir örnek yük devretme grubunda planlı yük devretmeyi yürütür. |
> | Action | Microsoft. SQL/Locations/ınstancefailovergroups/forceFailoverAllowDataLoss/eylem | Var olan bir örnek yük devretme grubunda zorla yük devretmeyi yürütür. |
> | Action | Microsoft. SQL/Locations/ınstancefailovergroups/Read | Örnek yük devretme grupları listesini döndürür veya belirtilen örnek yük devretme grubu için özellikleri alır. |
> | Action | Microsoft. SQL/Locations/ınstancefailovergroups/Write | Belirtilen parametrelerle bir örnek yük devretme grubu oluşturur veya belirtilen örnek yük devretme grubu için özellikleri veya etiketleri güncelleştirir. |
> | Action | Microsoft. SQL/konumlar/instancePoolAzureAsyncOperation/Read | Örnek havuzu işleminin durumunu alır |
> | Action | Microsoft. SQL/Locations/ınstancepooloperationresults/Read | Örnek havuzu işleminin sonucunu alır |
> | Action | Microsoft. SQL/Locations/ınterfaceendpointprofileazureasyncoperation/Read | Belirli bir arabirim uç noktası için Azure zaman uyumsuz işleminin ayrıntılarını döndürür |
> | Action | Microsoft. SQL/Locations/ınterfaceendpointprofileoperationresults/Read | Belirtilen arabirim uç noktası profili işleminin ayrıntılarını döndürür |
> | Action | Microsoft. SQL/konumlar/jobAgentAzureAsyncOperation/Read | Bir iş Aracısı işleminin durumunu alır. |
> | Action | Microsoft. SQL/konumlar/jobAgentOperationResults/Read | Bir iş Aracısı işleminin sonucunu alır. |
> | Action | Microsoft. SQL/konumlar/longTermRetentionBackups/Read | Bir konumdaki her sunucu için, her veritabanı için uzun süreli bekletme yedeklemelerini listeler |
> | Action | Microsoft. SQL/Locations/longTermRetentionServers/longTermRetentionBackups/Read | Bir sunucudaki her veritabanı için uzun süreli bekletme yedeklemelerini listeler |
> | Action | Microsoft. SQL/Locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/Delete | Uzun süreli bekletme yedeklemesini siler |
> | Action | Microsoft. SQL/Locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/Read | Bir veritabanı için uzun süreli bekletme yedeklemelerini listeler |
> | Action | Microsoft. SQL/Locations/managedDatabaseRestoreAzureAsyncOperation/tamamlana/eylem | Yönetilen veritabanı geri yükleme işlemini tamamlar |
> | Action | Microsoft. SQL/Locations/Managedınstanceencryptionkorunabilir Tokızureasyncoperation/Read | Saydam veri şifrelemesi yönetilen örnek şifreleme koruyucusu üzerinde devam eden işlemleri alır |
> | Action | Microsoft. SQL/Locations/Managedınstanceencryptionkorunabilir/Read | Saydam veri şifrelemesi yönetilen örnek şifreleme koruyucusu üzerinde devam eden işlemleri alır |
> | Action | Microsoft. SQL/konumlar/managedInstanceKeyAzureAsyncOperation/Read | Saydam veri şifrelemesi yönetilen örnek anahtarları üzerinde devam eden işlemleri alır |
> | Action | Microsoft. SQL/konumlar/managedInstanceKeyOperationResults/Read | Saydam veri şifrelemesi yönetilen örnek anahtarları üzerinde devam eden işlemleri alır |
> | Action | Microsoft. SQL/konumlar/managedTransparentDataEncryptionAzureAsyncOperation/Read | Yönetilen veritabanı saydam veri şifrelemesi üzerinde devam eden işlemleri alır |
> | Action | Microsoft. SQL/konumlar/managedTransparentDataEncryptionOperationResults/Read | Yönetilen veritabanı saydam veri şifrelemesi üzerinde devam eden işlemleri alır |
> | Action | Microsoft. SQL/konumlar/privateEndpointConnectionAzureAsyncOperation/Read | Özel bir uç nokta bağlantısı işleminin sonucunu alır |
> | Action | Microsoft. SQL/konumlar/privateEndpointConnectionOperationResults/Read | Özel bir uç nokta bağlantısı işleminin sonucunu alır |
> | Action | Microsoft. SQL/konumlar/privateEndpointConnectionProxyAzureAsyncOperation/Read | Özel bir uç noktası bağlantı proxy 'si işleminin sonucunu alır |
> | Action | Microsoft. SQL/konumlar/privateEndpointConnectionProxyOperationResults/Read | Özel bir uç noktası bağlantı proxy 'si işleminin sonucunu alır |
> | Action | Microsoft. SQL/konumlar/okuma | Belirli bir abonelik için kullanılabilir konumları alır |
> | Action | Microsoft. SQL/konumlar/serverKeyAzureAsyncOperation/Read | Saydam veri şifreleme sunucusu anahtarlarında devam eden işlemleri alır |
> | Action | Microsoft. SQL/konumlar/serverKeyOperationResults/Read | Saydam veri şifreleme sunucusu anahtarlarında devam eden işlemleri alır |
> | Action | Microsoft. SQL/konumlar/syncAgentOperationResults/Read | Eşitleme Aracısı kaynak işleminin sonucunu al |
> | Action | Microsoft. SQL/Locations/Syncdatabaseıds/Read | Belirli bir bölge ve abonelik için eşitleme veritabanı kimliklerini alma |
> | Action | Microsoft. SQL/Locations/syncGroupOperationResults/Read | Eşitleme grubu kaynağı işleminin sonucunu al |
> | Action | Microsoft. SQL/Locations/syncMemberOperationResults/Read | Eşitleme üyesi kaynağı işleminin sonucunu al |
> | Action | Microsoft. SQL/konumlar/kullanımlar/okuma | Bir konumdaki bu abonelik için kullanım ölçümlerinin bir koleksiyonunu alır |
> | Action | Microsoft. SQL/Locations/virtualNetworkRulesAzureAsyncOperation/Read | Belirtilen sanal ağ kurallarının ayrıntılarını döndürür Azure zaman uyumsuz işlemi  |
> | Action | Microsoft. SQL/konumlar/virtualNetworkRulesOperationResults/Read | Belirtilen sanal ağ kuralları işleminin ayrıntılarını döndürür  |
> | Action | Microsoft. SQL/ManagedInstances/Administrators/Delete | Yönetilen örneğin mevcut bir yöneticisini siler. |
> | Action | Microsoft. SQL/ManagedInstances/Administrators/Read | Yönetilen örnek yöneticilerinin bir listesini alır. |
> | Action | Microsoft. SQL/ManagedInstances/Administrators/Write | Yönetilen örnek yöneticisini belirtilen parametrelerle oluşturur veya güncelleştirir. |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/backupShortTermRetentionPolicies/Read | Yönetilen bir veritabanı için kısa süreli bekletme ilkesi alır |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/backupShortTermRetentionPolicies/Write | Yönetilen bir veritabanı için kısa süreli bekletme ilkesini güncelleştirir |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/currentSensitivityLabels/Read | Belirli bir veritabanının duyarlılık etiketlerini listeleyin |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/currentSensitivityLabels/Write | Batch güncelleştirmesi duyarlılık etiketleri |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/Sil | Varolan bir yönetilen veritabanını siler |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Read | Kaynak için tanılama ayarını alır |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/sağlayıcılar/Microsoft. Insights/logDefinitions/Read | Yönetilen örnek veritabanları için kullanılabilir günlükleri alır |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/okuma | Mevcut yönetilen veritabanını alır |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/recommendedSensitivityLabels/Read | Belirli bir veritabanının duyarlılık etiketlerini listeleyin |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/şemalar/okuma | Yönetilen bir veritabanı şeması alın. |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/şemalar/tablolar/sütunlar/okuma | Yönetilen veritabanı sütunu al |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels/Delete | Belirli bir sütunun duyarlılık etiketini Sil |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels/Disable/Action | Belirli bir sütunda duyarlılık önerilerini devre dışı bırak |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels/Enable/Action | Belirli bir sütunda duyarlılık önerilerini etkinleştir |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels/Read | Belirli bir sütunun duyarlılık etiketini al |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels/Write | Belirli bir sütunun duyarlılık etiketini oluştur veya güncelleştir |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/şemalar/tablolar/okuma | Yönetilen veritabanı tablosu al |
> | Action | Microsoft. SQL/ManagedInstances/Databases/Securityalcertpolicies/Read | Belirli bir sunucu için yapılandırılmış yönetilen veritabanı tehdit algılama ilkelerinin listesini alma |
> | Action | Microsoft. SQL/ManagedInstances/Databases/Securityalcertpolicies/Write | Belirli bir yönetilen veritabanı için veritabanı tehdit algılama ilkesini değiştirme |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/securityEvents/Read | Yönetilen veritabanı güvenlik olaylarını alır |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/sensitivityLabels/Read | Belirli bir veritabanının duyarlılık etiketlerini listeleyin |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/transparentDataEncryption/Read | Belirli bir yönetilen veritabanında veritabanı Saydam Veri Şifrelemesi ayrıntılarını alma |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/transparentDataEncryption/Write | Belirli bir yönetilen veritabanı için veritabanı Saydam Veri Şifrelemesi değiştirme |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/ | Belirli bir veritabanı için güvenlik açığı değerlendirmesini kaldırma |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/ | Bir givendatabase üzerinde güvenlik açığı değerlendirme ilkelerini alma |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/ek olarak, bu, kural/taban çizgileri/silme | Belirli bir veritabanı için güvenlik açığı değerlendirme kuralı temelini kaldır |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/ek olarak, bu, kurallar/temeller/okunan | Belirli bir veritabanı için güvenlik açığı değerlendirme kuralı temelini al |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/ek olarak, bu, kural/taban çizgileri/yazma | Belirli bir veritabanı için güvenlik açığı değerlendirme kuralı temelini değiştirme |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/bu işlem,, taramalar/taramalar/dışarı aktarma/eylem | Var olan bir tarama sonucunu okunabilir bir biçime dönüştürün. Zaten varsa, hiçbir şey olmaz |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/bu işlem, bir veya taramalar/ınitiatescan/ACTION | Güvenlik açığı değerlendirmesi veritabanı taramasını yürütün. |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/ | Veritabanı güvenlik açığı değerlendirmesi tarama kayıtlarının listesini döndürün veya belirtilen tarama KIMLIĞI için tarama kaydını alın. |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/ | Belirli bir veritabanı için güvenlik açığı değerlendirmesini değiştirme |
> | Action | Microsoft. SQL/ManagedInstances/veritabanları/yazma | Yeni bir veritabanı oluşturur veya var olan bir veritabanını güncelleştirir. |
> | Action | Microsoft. SQL/ManagedInstances/Delete | Varolan bir yönetilen örneği siler. |
> | Action | Microsoft. SQL/ManagedInstances/encryptionProtector/Read | Sunucu şifreleme koruyucuları listesini döndürür veya belirtilen sunucu şifreleme koruyucusu için özellikleri alır. |
> | Action | Microsoft. SQL/ManagedInstances/encryptionProtector/yeniden doğrula/eylem | Belirtilen sunucu şifreleme koruyucusu için özellikleri güncelleştirin. |
> | Action | Microsoft. SQL/ManagedInstances/encryptionProtector/Write | Belirtilen sunucu şifreleme koruyucusu için özellikleri güncelleştirin. |
> | Action | Microsoft. SQL/ManagedInstances/anahtarlar/Sil | Mevcut bir Azure SQL yönetilen örnek anahtarını siler. |
> | Action | Microsoft. SQL/ManagedInstances/anahtarlar/okuma | Yönetilen örnek anahtarlarının listesini döndürün veya belirtilen yönetilen örnek anahtarı için özellikleri alır. |
> | Action | Microsoft. SQL/ManagedInstances/anahtarlar/Write | Belirtilen parametrelerle bir anahtar oluşturur veya belirtilen yönetilen örnek anahtarı için özellikleri veya etiketleri güncelleştirir. |
> | Action | Microsoft. SQL/ManagedInstances/metricDefinitions/oku | Yönetilen örnek ölçüm tanımlarını al |
> | Action | Microsoft. SQL/ManagedInstances/ölçümler/okuma | Yönetilen örnek ölçümlerini al |
> | Action | Microsoft. SQL/ManagedInstances/Providers/Microsoft. Insights/diagnosticSettings/Read | Kaynak için tanılama ayarını alır |
> | Action | Microsoft. SQL/ManagedInstances/Providers/Microsoft. Insights/diagnosticSettings/Write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. SQL/ManagedInstances/Providers/Microsoft. Insights/logDefinitions/Read | Yönetilen örnekler için kullanılabilir günlükleri alır |
> | Action | Microsoft. SQL/ManagedInstances/Providers/Microsoft. Insights/metricDefinitions/Read | Yönetilen örnekler için kullanılabilen ölçümlerin dönüş türleri |
> | Action | Microsoft. SQL/ManagedInstances/Read | Yönetilen örneklerin listesini döndürün veya belirtilen yönetilen örnek için özellikleri alır. |
> | Action | Microsoft. SQL/ManagedInstances/recoverableDatabases/Read | Kurtarılabilir yönetilen veritabanlarının listesini döndürür |
> | Action | Microsoft. SQL/ManagedInstances/Restokbledroppeddatabases/backupShortTermRetentionPolicies/Read | Bırakılmış bir yönetilen veritabanı için kısa süreli bekletme ilkesi alır |
> | Action | Microsoft. SQL/ManagedInstances/Restokbledroppeddatabases/backupShortTermRetentionPolicies/Write | Bırakılmış bir yönetilen veritabanı için kısa süreli bekletme ilkesini güncelleştirir |
> | Action | Microsoft. SQL/ManagedInstances/Restokbledroppeddatabases/Read | Yeniden yüklenebilen, bırakılmış yönetilen veritabanlarının listesini döndürür. |
> | Action | Microsoft. SQL/ManagedInstances/Securityalcertpolicies/okuma | Belirli bir sunucu için yapılandırılmış yönetilen sunucu tehdit algılama ilkelerinin listesini alma |
> | Action | Microsoft. SQL/ManagedInstances/Securityalcertpolicies/Write | Belirli bir yönetilen sunucu için yönetilen sunucu tehdit algılama ilkesini değiştirme |
> | Action | Microsoft. SQL/ManagedInstances/tdeCertificates/ACTION | TDE sertifikası oluştur/güncelleştir |
> | Action | Microsoft. SQL/ManagedInstances/ | Belirli bir yönetilen örnek için güvenlik açığı değerlendirmesini kaldırma |
> | Action | Microsoft. SQL/ManagedInstances/ | Belirli bir yönetilen örnekteki güvenlik açığı değerlendirme ilkelerini alma |
> | Action | Microsoft. SQL/ManagedInstances/ | Belirli bir yönetilen örnek için güvenlik açığı değerlendirmesini değiştirme |
> | Action | Microsoft. SQL/ManagedInstances/Write | Belirtilen parametrelerle yönetilen bir örnek oluşturur veya belirtilen yönetilen örnek için özellikleri veya etiketleri güncelleştirir. |
> | Action | Microsoft. SQL/işlemler/okuma | Kullanılabilir REST işlemlerini alır |
> | Action | Microsoft. SQL/privateEndpointConnectionsApproval/eylem | Kullanıcının özel bir uç nokta bağlantısını onaylamasını izin verilip verilmeyeceğini belirler |
> | Action | Microsoft. SQL/Register/ACTION | Microsoft SQL veritabanı kaynak sağlayıcısı için aboneliği kaydeder ve Microsoft SQL veritabanlarının oluşturulmasına izin vermez. |
> | Action | Microsoft. SQL/Servers/Yönetimtoroperationresults/Read | Sunucu yöneticileri üzerinde devam eden işlemleri alır |
> | Action | Microsoft. SQL/Servers/Administrators/Delete | Sunucu Yöneticisini Sil |
> | Action | Microsoft. SQL/Servers/Administrators/Read | Sunucu Yöneticisi ayrıntılarını alma |
> | Action | Microsoft. SQL/Servers/Administrators/Write | Sunucu Yöneticisi oluştur veya güncelleştir |
> | Action | Microsoft. SQL/Servers/danışmanları/okuma | Sunucu için kullanılabilen danışmanlarının listesini döndürür |
> | Action | Microsoft. SQL/Servers/danışmanları/recommendedActions/Read | Sunucu için belirtilen danışman için önerilen eylemlerin listesini döndürür |
> | Action | Microsoft. SQL/Servers/danışmanları/recommendedActions/Write | Sunucuda önerilen eylemi Uygula |
> | Action | Microsoft. SQL/Servers/danışmanları/yazma | Güncelleştirme, sunucu düzeyinde bir danışman 'ın durumunu otomatik olarak yürütür. |
> | Action | Microsoft. SQL/Servers/auditingPolicies/Read | Belirli bir sunucuda yapılandırılan varsayılan sunucu tablosu denetim ilkesinin ayrıntılarını alın |
> | Action | Microsoft. SQL/Servers/auditingPolicies/Write | Belirli bir sunucu için varsayılan sunucu tablosu denetimini değiştirme |
> | Action | Microsoft. SQL/Servers/auditingSettings/operationResults/Read | Sunucu blobu denetim ilkesi ayarlama işleminin sonucunu al |
> | Action | Microsoft. SQL/Servers/auditingSettings/Read | Belirli bir sunucuda yapılandırılan sunucu blobu denetim ilkesinin ayrıntılarını alma |
> | Action | Microsoft. SQL/Servers/auditingSettings/Write | Belirli bir sunucu için sunucu blobu denetimini değiştirme |
> | Action | Microsoft. SQL/Servers/otomatik olarak ayarlama/okuma | Sunucu için otomatik ayarlama ayarlarını döndürür |
> | Action | Microsoft. SQL/Servers/otomatik olarak ayarlama/yazma | Sunucu için otomatik ayarlama ayarlarını güncelleştirir ve güncelleştirilmiş ayarları döndürür |
> | Action | Microsoft. SQL/Servers/backupLongTermRetentionVaults/Delete | Var olan bir yedek arşiv kasasını siler. |
> | Action | Microsoft. SQL/Servers/backupLongTermRetentionVaults/Read | Bu işlem, yedekleme uzun süreli saklama Kasası elde etmek için kullanılır. Bu sunucuya kayıtlı kasayla ilgili bilgileri döndürür |
> | Action | Microsoft. SQL/Servers/backupLongTermRetentionVaults/Write | Bu işlem, yedekleme uzun süreli saklama kasasını bir sunucuya kaydetmek için kullanılır |
> | Action | Microsoft. SQL/Servers/communicationLinks/Delete | Var olan bir sunucu iletişim bağlantısını siler. |
> | Action | Microsoft. SQL/Servers/communicationLinks/Read | Belirtilen bir sunucunun iletişim bağlantılarının listesini döndürür. |
> | Action | Microsoft. SQL/Servers/communicationLinks/Write | Sunucu iletişim bağlantısı oluşturun veya güncelleştirin. |
> | Action | Microsoft. SQL/Servers/connectionPolicies/Read | Belirtilen bir sunucunun sunucu bağlantı ilkelerinin listesini döndürür. |
> | Action | Microsoft. SQL/Servers/connectionPolicies/Write | Sunucu bağlantı ilkesi oluşturun veya güncelleştirin. |
> | Action | Microsoft. SQL/Servers/veritabanları/danışmanları/okuma | Veritabanı için kullanılabilen danışmanlarının listesini döndürür |
> | Action | Microsoft. SQL/Servers/veritabanları/danışmanları/recommendedActions/okuma | Veritabanı için belirtilen danışman için önerilen eylemlerin listesini döndürür |
> | Action | Microsoft. SQL/Servers/veritabanları/danışmanları/recommendedActions/yazma | Önerilen eylemi veritabanına Uygula |
> | Action | Microsoft. SQL/Servers/veritabanları/danışmanları/yazma | Bir danışman 'nin veritabanı düzeyinde otomatik yürütme durumunu güncelleştir. |
> | Action | Microsoft. SQL/Servers/veritabanları/auditingPolicies/Read | Belirli bir veritabanında yapılandırılan tablo denetim ilkesinin ayrıntılarını alma |
> | Action | Microsoft. SQL/Servers/veritabanları/auditingPolicies/Write | Belirli bir veritabanı için tablo Denetim ilkesini değiştirme |
> | Action | Microsoft. SQL/Servers/veritabanları/auditingSettings/Read | Belirli bir veritabanında yapılandırılan blob denetim ilkesinin ayrıntılarını alma |
> | Action | Microsoft. SQL/Servers/veritabanları/auditingSettings/Write | Belirli bir veritabanı için blob denetim ilkesini değiştirme |
> | Action | Microsoft.Sql/servers/databases/auditRecords/read | Veritabanı blobu denetim kayıtlarını alma |
> | Action | Microsoft. SQL/Servers/veritabanları/otomatik olarak ayarlama/okuma | Bir veritabanı için otomatik ayarlama ayarlarını döndürür |
> | Action | Microsoft. SQL/Servers/veritabanları/otomatik olarak ayarlama/yazma | Bir veritabanı için otomatik ayarlama ayarlarını güncelleştirir ve güncelleştirilmiş ayarları döndürür |
> | Action | Microsoft. SQL/Servers/veritabanları/azureAsyncOperation/Read | Bir veritabanı işleminin durumunu alır. |
> | Action | Microsoft. SQL/Servers/veritabanları/backupLongTermRetentionPolicies/Read | Belirtilen bir veritabanının yedekleme Arşivi ilkelerinin listesini döndürür. |
> | Action | Microsoft. SQL/Servers/veritabanları/backupLongTermRetentionPolicies/Write | Veritabanı yedekleme arşivi ilkesi oluşturun veya güncelleştirin. |
> | Action | Microsoft. SQL/Servers/veritabanları/backupShortTermRetentionPolicies/Read | Bir veritabanı için kısa süreli bekletme ilkesi alır |
> | Action | Microsoft. SQL/Servers/veritabanları/backupShortTermRetentionPolicies/Write | Bir veritabanı için kısa süreli saklama ilkesini güncelleştirir |
> | Action | Microsoft. SQL/Servers/veritabanları/connectionPolicies/okuma | Belirli bir veritabanında yapılandırılan bağlantı ilkesinin ayrıntılarını alma |
> | Action | Microsoft. SQL/Servers/veritabanları/connectionPolicies/Write | Belirli bir veritabanı için bağlantı ilkesini değiştirme |
> | Action | Microsoft. SQL/Servers/veritabanları/currentSensitivityLabels/Read | Belirli bir veritabanının duyarlılık etiketlerini listeleyin |
> | Action | Microsoft. SQL/Servers/veritabanları/currentSensitivityLabels/Write | Batch güncelleştirmesi duyarlılık etiketleri |
> | Action | Microsoft. SQL/Servers/veritabanları/dataMaskingPolicies/Read | Veritabanı veri maskeleme ilkelerinin listesini döndürün. |
> | Action | Microsoft. SQL/Servers/veritabanları/dataMaskingPolicies/Rules/Delete | Belirli bir veritabanı için veri maskeleme ilkesi kuralını Sil |
> | Action | Microsoft. SQL/Servers/veritabanları/dataMaskingPolicies/kurallar/okuma | Belirli bir veritabanında yapılandırılan veri maskeleme ilkesi kuralının ayrıntılarını alma |
> | Action | Microsoft. SQL/Servers/veritabanları/dataMaskingPolicies/Rules/Write | Belirli bir veritabanı için veri maskeleme ilke kuralını değiştirme |
> | Action | Microsoft. SQL/Servers/veritabanları/dataMaskingPolicies/Write | Belirli bir veritabanı için veri maskeleme ilkesini değiştirme |
> | Action | Microsoft. SQL/Servers/veritabanları/dataWarehouseQueries/dataWarehouseQuerySteps/Read | Seçilen adım KIMLIĞI için veri ambarı sorgusunun dağıtılmış sorgu adımı bilgilerini döndürür |
> | Action | Microsoft. SQL/Servers/veritabanları/dataWarehouseQueries/Read | Seçili sorgu KIMLIĞI için veri ambarı dağıtım sorgu bilgilerini döndürür |
> | Action | Microsoft. SQL/Servers/veritabanları/dataWarehouseUserActivities/okundu | Çalışan ve askıya alınmış sorguları içeren bir SQL veri ambarı örneğinin Kullanıcı etkinliklerini alır |
> | Action | Microsoft. SQL/Servers/veritabanları/Delete | Var olan bir veritabanını siler. |
> | Action | Microsoft. SQL/Servers/veritabanları/Export/Action | Azure SQL veritabanını dışarı aktarma |
> | Action | Microsoft. SQL/Servers/veritabanları/extendedAuditingSettings/Read | Belirli bir veritabanında yapılandırılan genişletilmiş blob denetim ilkesinin ayrıntılarını alma |
> | Action | Microsoft. SQL/Servers/veritabanları/extendedAuditingSettings/Write | Belirli bir veritabanı için genişletilmiş blob denetim ilkesini değiştirme |
> | Action | Microsoft. SQL/Servers/veritabanları/uzantılar/okuma | Veritabanı için uzantıların bir koleksiyonunu alır. |
> | Action | Microsoft. SQL/Servers/veritabanları/uzantılar/yazma | Belirli bir veritabanı için uzantıyı değiştirme |
> | Action | Microsoft. SQL/Servers/veritabanları/yük devretme/eylem | Müşterinin başlattığı veritabanını yük devretme. |
> | Action | Microsoft. SQL/Servers/veritabanları/geoBackupPolicies/okuma | Belirli bir veritabanı için coğrafi yedekleme ilkeleri alma |
> | Action | Microsoft. SQL/Servers/veritabanları/geoBackupPolicies/Write | Veritabanı coğrafi yedekleme ilkesi oluşturma veya güncelleştirme |
> | Action | Microsoft. SQL/Servers/veritabanları/ımportexportoperationresults/Read | Devam eden içeri/dışarı aktarma işlemlerini alır |
> | Action | Microsoft. SQL/Servers/veritabanları/maintenanceWindowOptions/Read | Seçili veritabanı için kullanılabilir bakım pencerelerinin bir listesini alır. |
> | Action | Microsoft. SQL/Servers/veritabanları/maintenanceWindows/Read | Seçilen bir veritabanı için bakım Windows ayarlarını alır. |
> | Action | Microsoft. SQL/Servers/veritabanları/maintenanceWindows/Write | Seçili veritabanı için bakım Windows ayarlarını ayarlar. |
> | Action | Microsoft. SQL/Servers/veritabanları/metricDefinitions/Read | Veritabanları için kullanılabilir ölçümlerin dönüş türleri |
> | Action | Microsoft. SQL/Servers/veritabanları/ölçümler/okuma | Veritabanları için ölçümleri döndürme |
> | Action | Microsoft. SQL/Servers/veritabanları/Move/Action | Varolan bir veritabanının adını değiştirin. |
> | Action | Microsoft. SQL/Servers/veritabanları/operationResults/Read | Bir veritabanı işleminin durumunu alır. |
> | Action | Microsoft. SQL/Servers/veritabanları/işlemler/iptal/eylem | Henüz bitmemiş Azure SQL veritabanı bekleyen zaman uyumsuz işlemi iptal eder. |
> | Action | Microsoft. SQL/Servers/veritabanları/işlemler/okuma | Veritabanında gerçekleştirilen işlemlerin listesini döndürün |
> | Action | Microsoft. SQL/Servers/veritabanları/Duraklat/eylem | Azure SQL veri ambarı veritabanını Duraklat |
> | Action | Microsoft. SQL/Servers/veritabanları/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Read | Kaynak için tanılama ayarını alır |
> | Action | Microsoft. SQL/Servers/veritabanları/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. SQL/Servers/veritabanları/sağlayıcılar/Microsoft. Insights/logDefinitions/Read | Veritabanları için kullanılabilir günlükleri alır |
> | Action | Microsoft. SQL/Servers/veritabanları/sağlayıcılar/Microsoft. Insights/metricDefinitions/okuma | Veritabanları için kullanılabilir ölçümlerin dönüş türleri |
> | Action | Microsoft. SQL/Servers/veritabanları/queryStore/Querymetin/Read | Belirtilen parametrelere karşılık gelen sorgu metinleri koleksiyonunu döndürür. |
> | Action | Microsoft. SQL/Servers/veritabanları/queryStore/Read | Veritabanı için sorgu deposu ayarlarının geçerli değerlerini döndürür. |
> | Action | Microsoft. SQL/Servers/veritabanları/queryStore/Write | Veritabanı için sorgu deposu ayarını güncelleştirir |
> | Action | Microsoft. SQL/Servers/veritabanları/okuma | Veritabanlarının listesini döndürün veya belirtilen veritabanının özelliklerini alır. |
> | Action | Microsoft. SQL/Servers/veritabanları/recommendedSensitivityLabels/Read | Belirli bir veritabanının duyarlılık etiketlerini listeleyin |
> | Action | Microsoft. SQL/Servers/veritabanları/replicationLinks/Delete | Çoğaltma ilişkisini zorla ve olası veri kaybı ile sonlandır |
> | Action | Microsoft. SQL/Servers/veritabanları/replicationLinks/yük devretme/eylem | Tüm değişiklikleri birinceden eşitlemeden sonra, bu veritabanını çoğaltma relationship\u0027s birincil öğesine ve uzak birincili ikincil haline getirmek için yük devretme |
> | Action | Microsoft. SQL/Servers/veritabanları/replicationLinks/forceFailoverAllowDataLoss/eylem | Olası veri kaybından hemen yük devretme, bu veritabanını çoğaltmaya yönelik olan relationship\u0027s birincili ve uzak birincil |
> | Action | Microsoft. SQL/Servers/veritabanları/replicationLinks/Read | Çoğaltma bağlantıları listesini döndürün veya belirtilen çoğaltma bağlantıları için özellikleri alır. |
> | Action | Microsoft. SQL/Servers/veritabanları/replicationLinks/link/Action | Çoğaltma ilişkisini zorla veya iş ortağıyla eşitlemeden sonra sonlandırın |
> | Action | Microsoft. SQL/Servers/veritabanları/replicationLinks/updateReplicationMode/Action | Zaman uyumlu veya zaman uyumsuz mod bağlantısı için çoğaltma modunu Güncelleştir |
> | Action | Microsoft. SQL/Servers/veritabanları/restorePoints/ACTION | Yeni bir geri yükleme noktası oluşturur |
> | Action | Microsoft. SQL/Servers/veritabanları/restorePoints/Delete | Veritabanı için bir geri yükleme noktası siler. |
> | Action | Microsoft. SQL/Servers/veritabanları/restorePoints/Read | Veritabanı için geri yükleme noktalarını döndürür. |
> | Action | Microsoft. SQL/Servers/veritabanları/özgeçmişi/Action | Azure SQL veri ambarı veritabanını sürdürür |
> | Action | Microsoft. SQL/Servers/veritabanları/şemalar/okuma | Veritabanı şeması alın. |
> | Action | Microsoft. SQL/Servers/veritabanları/şemalar/tablolar/sütunlar/okuma | Veritabanı sütunu al. |
> | Action | Microsoft. SQL/Servers/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels/Delete | Belirli bir sütunun duyarlılık etiketini Sil |
> | Action | Microsoft. SQL/Servers/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels/Disable/Action | Belirli bir sütunda duyarlılık önerilerini devre dışı bırak |
> | Action | Microsoft. SQL/Servers/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels/Enable/Action | Belirli bir sütunda duyarlılık önerilerini etkinleştir |
> | Action | Microsoft. SQL/Servers/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels/Read | Belirli bir sütunun duyarlılık etiketini al |
> | Action | Microsoft. SQL/Servers/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels/Write | Belirli bir sütunun duyarlılık etiketini oluştur veya güncelleştir |
> | Action | Microsoft. SQL/Servers/veritabanları/şemalar/tablolar/okuma | Veritabanı tablosu al. |
> | Action | Microsoft. SQL/Servers/veritabanları/şemalar/tablolar/recommendedIndexes/Read | Bir veritabanındaki dizin önerilerinin listesini alma |
> | Action | Microsoft. SQL/Servers/veritabanları/şemalar/tablolar/recommendedIndexes/Write | Dizin önerisi uygulama |
> | Action | Microsoft. SQL/Servers/Databases/Securityalcertpolicies/Read | Belirli bir sunucu için yapılandırılmış veritabanı tehdit algılama ilkelerinin listesini alma |
> | Action | Microsoft. SQL/Servers/veritabanları/Securityalcertpolicies/Write | Belirli bir veritabanı için veritabanı tehdit algılama ilkesini değiştirme |
> | Action | Microsoft. SQL/Servers/Databases/Securityölçümlerini/Read | Veritabanı güvenlik ölçümlerinin bir koleksiyonunu alır |
> | Action | Microsoft. SQL/Servers/veritabanları/sensitivityLabels/Read | Belirli bir veritabanının duyarlılık etiketlerini listeleyin |
> | Action | Microsoft. SQL/Servers/veritabanları/Servicetierdanışmanors/Read | Performansı artırmak veya maliyeti azaltmak için sorgu yürütme istatistiklerine bağlı olarak veritabanını ölçeklendirmeye veya azaltmasına ilişkin öneriyi döndürün |
> | Action | Microsoft. SQL/Servers/veritabanları/SKU/okuma | Bir veritabanı için kullanılabilen SKU 'ların bir koleksiyonunu alır |
> | Action | Microsoft. SQL/Servers/veritabanları/syncGroups/cancelSync/Action | Eşitleme grubu eşitlemesini iptal et |
> | Action | Microsoft. SQL/Servers/veritabanları/syncGroups/Delete | Varolan bir eşitleme grubunu siler. |
> | Action | Microsoft. SQL/Servers/veritabanları/syncGroups/hubSchemas/Read | Eşitleme Merkezi veritabanı şemaları listesini döndürün |
> | Action | Microsoft. SQL/Servers/veritabanları/syncGroups/logs/Read | Eşitleme grubu günlüklerinin listesini döndürün |
> | Action | Microsoft. SQL/Servers/veritabanları/syncGroups/Read | Eşitleme gruplarının listesini döndürün veya belirtilen eşitleme grubunun özelliklerini alır. |
> | Action | Microsoft. SQL/Servers/veritabanları/syncGroups/refreshHubSchema/Action | Eşitleme Merkezi veritabanı şemasını Yenile |
> | Action | Microsoft. SQL/Servers/veritabanları/syncGroups/refreshHubSchemaOperationResults/Read | Eşitleme Merkezi şema yenileme işleminin sonucunu al |
> | Action | Microsoft. SQL/Servers/veritabanları/syncGroups/syncMembers/Delete | Varolan bir eşitleme üyesini siler. |
> | Action | Microsoft. SQL/Servers/veritabanları/syncGroups/syncMembers/Read | Eşitleme üyelerinin listesini döndürün veya belirtilen eşitleme üyesinin özelliklerini alır. |
> | Action | Microsoft. SQL/Servers/veritabanları/syncGroups/syncMembers/refreshSchema/Action | Eşitleme üye şemasını Yenile |
> | Action | Microsoft. SQL/Servers/veritabanları/syncGroups/syncMembers/refreshSchemaOperationResults/Read | Eşitleme üye şeması yenileme işleminin sonucunu al |
> | Action | Microsoft. SQL/Servers/veritabanları/syncGroups/syncMembers/schemas/Read | Eşitleme üyesi veritabanı şemaları listesini döndür |
> | Action | Microsoft. SQL/Servers/veritabanları/syncGroups/syncMembers/Write | Belirtilen parametrelerle bir eşitleme üyesi oluşturur veya belirtilen eşitleme üyesinin özelliklerini güncelleştirir. |
> | Action | Microsoft. SQL/Servers/veritabanları/syncGroups/triggerSync/Action | Eşitleme grubu eşitlemesini tetikleme |
> | Action | Microsoft. SQL/Servers/veritabanları/syncGroups/Write | Belirtilen parametrelerle bir eşitleme grubu oluşturur veya belirtilen eşitleme grubunun özelliklerini güncelleştirir. |
> | Action | Microsoft. SQL/Servers/veritabanları/topQueries/queryText/Action | Seçili sorgu KIMLIĞI için Transact-SQL metnini döndürür |
> | Action | Microsoft. SQL/Servers/veritabanları/topQueries/Read | Seçili zaman döneminde seçili sorgu için toplu çalışma zamanı istatistiklerini döndürür |
> | Action | Microsoft. SQL/Servers/veritabanları/topQueries/STATISTICS/Read | Seçili zaman döneminde seçili sorgu için toplu çalışma zamanı istatistiklerini döndürür |
> | Action | Microsoft. SQL/Servers/veritabanları/transparentDataEncryption/operationResults/Read | Saydam veri şifrelemesi üzerinde devam eden işlemleri alır |
> | Action | Microsoft. SQL/Servers/veritabanları/transparentDataEncryption/Read | Belirli bir veritabanı için saydam veri şifreleme güvenlik özelliğinin durumunu ve ayrıntılarını alma |
> | Action | Microsoft. SQL/Servers/veritabanları/transparentDataEncryption/Write | Saydam veri şifreleme durumunu değiştirme |
> | Action | Microsoft. SQL/Servers/veritabanları/upgradeDataWarehouse/eylem | Azure SQL veri ambarı veritabanını yükselt |
> | Action | Microsoft. SQL/Servers/veritabanları/kullanımlar/okuma | Azure SQL veritabanı kullanımları bilgilerini alır |
> | Action | Microsoft. SQL/Servers/veritabanları/ | Belirli bir veritabanı için güvenlik açığı değerlendirmesini kaldırma |
> | Action | Microsoft. SQL/Servers/veritabanları/ | Bir givendatabase üzerinde güvenlik açığı değerlendirme ilkelerini alma |
> | Action | Microsoft. SQL/Servers/veritabanları/ek | Belirli bir veritabanı için güvenlik açığı değerlendirme kuralı temelini kaldır |
> | Action | Microsoft. SQL/Servers/veritabanları/ek | Belirli bir veritabanı için güvenlik açığı değerlendirme kuralı temelini al |
> | Action | Microsoft. SQL/Servers/veritabanları/ek | Belirli bir veritabanı için güvenlik açığı değerlendirme kuralı temelini değiştirme |
> | Action | Microsoft. SQL/Servers/veritabanları/ek bir işlem/tarama/dışarı aktarma/eylem | Var olan bir tarama sonucunu okunabilir bir biçime dönüştürün. Zaten varsa, hiçbir şey olmaz |
> | Action | Microsoft. SQL/Servers/veritabanları/bu işlem, ve taramalar/ınitiatescan/ACTION | Güvenlik açığı değerlendirmesi veritabanı taramasını yürütün. |
> | Action | Microsoft. SQL/Servers/veritabanları/ | Veritabanı güvenlik açığı değerlendirmesi tarama kayıtlarının listesini döndürün veya belirtilen tarama KIMLIĞI için tarama kaydını alın. |
> | Action | Microsoft. SQL/Servers/veritabanları/ | Belirli bir veritabanı için güvenlik açığı değerlendirmesini değiştirme |
> | Action | Microsoft. SQL/Servers/veritabanları/bu işlem | Güvenlik açığı değerlendirmesi veritabanı taramasını yürütün. |
> | Action | Microsoft. SQL/Servers/veritabanları/bu işlem, beklenen taramalar/operationResults/Read | Veritabanı güvenlik açığı değerlendirme taraması yürütme işleminin sonucunu alma |
> | Action | Microsoft. SQL/Servers/databases/, bir ıactive Settings/Read | Belirli bir veritabanında yapılandırılan güvenlik açığı değerlendirmesinin ayrıntılarını alın |
> | Action | Microsoft. SQL/Servers/databases/, bir ıactive Settings/Write | Belirli bir veritabanı için güvenlik açığı değerlendirmesini değiştirme |
> | Action | Microsoft. SQL/Servers/veritabanları/Write | Belirtilen parametrelerle bir veritabanı oluşturur veya belirtilen veritabanı için özellikleri veya etiketleri güncelleştirir. |
> | Action | Microsoft. SQL/Servers/Delete | Var olan bir sunucuyu siler. |
> | Action | Microsoft. SQL/Servers/Diskalrecoveryconfiguration/Delete | Belirli bir sunucu için mevcut bir olağanüstü durum kurtarma yapılandırmasını siler |
> | Action | Microsoft. SQL/Servers/Diskalrecoveryconfiguration/yük devretme/eylem | Diskalrecoveryconfiguration yük devretme |
> | Action | Microsoft. SQL/Servers/Diskalrecoveryconfiguration/forceFailoverAllowDataLoss/eylem | Diskalrecoveryconfiguration yük devretmeyi zorla |
> | Action | Microsoft. SQL/Servers/Diskalrecoveryconfiguration/Read | Bu sunucuyu içeren olağanüstü durum kurtarma yapılandırmalarının bir koleksiyonunu alır |
> | Action | Microsoft. SQL/Servers/Diskalrecoveryconfiguration/Write | Sunucu olağanüstü durum kurtarma yapılandırmasını değiştirme |
> | Action | Microsoft. SQL/Servers/Elakpoolelismates/Read | Bu sunucu için önceden oluşturulmuş elastik havuz tahminlerinin listesini döndürür |
> | Action | Microsoft. SQL/Servers/Elakpoolemates/Write | Belirtilen veritabanlarının listesi için yeni elastik havuz tahmini oluşturur |
> | Action | Microsoft. SQL/Servers/Elaor havuzları/danışmanları/okuma | Elastik havuz için kullanılabilen danışmanlarının listesini döndürür |
> | Action | Microsoft. SQL/Servers/Elaor havuzları/danışmanları/recommendedActions/okuma | Elastik havuz için belirtilen danışman önerilen eylemlerin listesini döndürür |
> | Action | Microsoft. SQL/Servers/Elaor havuzları/danışmanları/recommendedActions/yazma | Elastik havuzda önerilen eylemi uygulayın |
> | Action | Microsoft. SQL/Servers/Elaor havuzları/danışmanları/yazma | Elastik havuz düzeyinde bir danışman 'ın otomatik yürütülme durumunu güncelleştirin. |
> | Action | Microsoft. SQL/Servers/Elaun havuzları/veritabanları/okuma | Elastik havuz için veritabanlarının listesini alır |
> | Action | Microsoft. SQL/Servers/Elaun havuzları/Delete | Mevcut elastik havuzu Sil |
> | Action | Microsoft. SQL/Servers/Elaun havuzları/elasticPoolActivity/Read | Belirli bir elastik veritabanı havuzundaki etkinlikleri ve ayrıntıları alma |
> | Action | Microsoft. SQL/Servers/Elaetiketler/Elakpooldatabaseactivity/Read | Elastik veritabanı havuzunun bir parçası olan belirli bir veritabanında etkinlikleri ve ayrıntıları alma |
> | Action | Microsoft. SQL/Servers/Elaetiketler/yük devretme/eylem | Müşteri tarafından başlatılan elastik havuz yük devretme. |
> | Action | Microsoft. SQL/Servers/Elakıhavuzhavuzları/metricDefinitions/okuma | Elastik veritabanı havuzları için kullanılabilen ölçümlerin dönüş türleri |
> | Action | Microsoft. SQL/Servers/ela/Pools/ölçümler/okuma | Elastik veritabanı havuzları için dönüş ölçümleri |
> | Action | Microsoft. SQL/Servers/Elaetiketler/işlemler/iptal/eylem | Henüz bitmemiş Azure SQL elastik havuzu bekleyen zaman uyumsuz işlemini iptal eder. |
> | Action | Microsoft. SQL/Servers/ela/Pools/işlemler/okuma | Elastik havuzda gerçekleştirilen işlemlerin listesini döndürün |
> | Action | Microsoft. SQL/Servers/ela/Pools/Providers/Microsoft. Insights/diagnosticSettings/Read | Kaynak için tanılama ayarını alır |
> | Action | Microsoft. SQL/Servers/ela/Pools/Providers/Microsoft. Insights/diagnosticSettings/Write | Kaynak için tanılama ayarını oluşturur veya güncelleştirir |
> | Action | Microsoft. SQL/Servers/Elakıpools/Providers/Microsoft. Insights/metricDefinitions/Read | Elastik veritabanı havuzları için kullanılabilen ölçümlerin dönüş türleri |
> | Action | Microsoft. SQL/Servers/Elaun havuzları/okuma | Belirli bir sunucudaki elastik havuzun ayrıntılarını alma |
> | Action | Microsoft. SQL/Servers/ela/Pools/SKU 'lar/okuma | Elastik havuz için kullanılabilen SKU 'ların bir koleksiyonunu alır |
> | Action | Microsoft. SQL/Servers/Elaun havuzları/yazma | Mevcut elastik havuzun yeni veya değişiklik özelliklerini oluştur |
> | Action | Microsoft. SQL/Servers/encryptionProtector/Read | Sunucu şifreleme koruyucuları listesini döndürür veya belirtilen sunucu şifreleme koruyucusu için özellikleri alır. |
> | Action | Microsoft. SQL/Servers/encryptionProtector/yeniden doğrula/eylem | Belirtilen sunucu şifreleme koruyucusu için özellikleri güncelleştirin. |
> | Action | Microsoft. SQL/Servers/encryptionProtector/Write | Belirtilen sunucu şifreleme koruyucusu için özellikleri güncelleştirin. |
> | Action | Microsoft. SQL/Servers/extendedAuditingSettings/Read | Belirli bir sunucuda yapılandırılmış genişletilmiş sunucu blobu denetim ilkesinin ayrıntılarını alma |
> | Action | Microsoft. SQL/Servers/extendedAuditingSettings/Write | Belirli bir sunucu için genişletilmiş sunucu blobu denetimini değiştirme |
> | Action | Microsoft. SQL/Servers/failoverGroups/Delete | Var olan bir yük devretme grubunu siler. |
> | Action | Microsoft. SQL/Servers/failoverGroups/yük devretme/eylem | Var olan bir yük devretme grubunda planlı yük devretmeyi yürütür. |
> | Action | Microsoft. SQL/Servers/failoverGroups/forceFailoverAllowDataLoss/eylem | Var olan bir yük devretme grubunda zorla yük devretmeyi yürütür. |
> | Action | Microsoft. SQL/Servers/failoverGroups/Read | Yük devretme grupları listesini döndürür veya belirtilen yük devretme grubu için özellikleri alır. |
> | Action | Microsoft. SQL/Servers/failoverGroups/Write | Belirtilen parametrelerle bir yük devretme grubu oluşturur veya belirtilen yük devretme grubu için özellikleri veya etiketleri güncelleştirir. |
> | Action | Microsoft. SQL/Servers/firewallRules/Delete | Var olan bir sunucu güvenlik duvarı kuralını siler. |
> | Action | Microsoft. SQL/Servers/firewallRules/Read | Sunucu Güvenlik Duvarı kurallarının listesini döndürün veya belirtilen sunucu güvenlik duvarı kuralının özelliklerini alır. |
> | Action | Microsoft. SQL/Servers/firewallRules/Write | Belirtilen parametrelere sahip bir sunucu güvenlik duvarı kuralı oluşturur, belirtilen kural için özellikleri güncelleştirin veya yeni sunucu güvenlik duvarı kurallarıyla tüm var olan kuralların üzerine yazın. |
> | Action | Microsoft. SQL/Servers/Import/Action | Sunucuda yeni bir veritabanı oluşturun ve bir DacPac paketinden şemayı ve verileri dağıtın |
> | Action | Microsoft. SQL/Servers/ımportexportoperationresults/Read | Devam eden içeri/dışarı aktarma işlemlerini alır |
> | Action | Microsoft. SQL/Servers/ınterfaceendpointprofiles/Delete | Belirtilen arabirim uç noktası profilini siler |
> | Action | Microsoft. SQL/Servers/ınterfaceendpointprofiles/okuma | Belirtilen arabirim uç noktası profilinin özelliklerini döndürür |
> | Action | Microsoft. SQL/Servers/ınterfaceendpointprofiles/Write | Belirtilen parametrelerle bir arabirim uç noktası profili oluşturur veya belirtilen arabirim uç noktası için özellikleri veya etiketleri güncelleştirir |
> | Action | Microsoft. SQL/Servers/jobAgents/Delete | Azure SQL DB iş aracısını siler |
> | Action | Microsoft. SQL/Servers/jobAgents/Read | Azure SQL DB iş aracısını alır |
> | Action | Microsoft. SQL/Servers/jobAgents/Write | Bir Azure SQL DB iş Aracısı oluşturur veya güncelleştirir |
> | Action | Microsoft. SQL/Servers/anahtarlar/Delete | Var olan bir sunucu anahtarını siler. |
> | Action | Microsoft. SQL/sunucular/anahtarlar/okuma | Sunucu anahtarlarının listesini döndürün veya belirtilen sunucu anahtarı için özellikleri alır. |
> | Action | Microsoft. SQL/Servers/anahtarlar/Write | Belirtilen parametrelere sahip bir anahtar oluşturur veya belirtilen sunucu anahtarı için özellikleri veya etiketleri güncelleştirir. |
> | Action | Microsoft. SQL/Servers/operationResults/Read | Devam eden sunucu işlemlerini alır |
> | Action | Microsoft. SQL/Servers/Privateendpointconnectionproxy/Delete | Var olan bir özel uç nokta bağlantı proxy 'sini siler |
> | Action | Microsoft. SQL/Servers/Privateendpointconnectionproxy/okuma | Özel uç nokta bağlantı proxy 'leri listesini döndürür veya belirtilen özel uç nokta bağlantı proxy 'sinin özelliklerini alır. |
> | Action | Microsoft. SQL/Servers/Privateendpointconnectionproxy/Validate/Action | Özel bir uç nokta bağlantısını doğrular NRP tarafında çağrı oluştur |
> | Action | Microsoft. SQL/Servers/Privateendpointconnectionproxy/yazma | Belirtilen parametrelerle özel bir uç nokta bağlantı proxy 'si oluşturur veya belirtilen özel uç nokta bağlantısı proxy 'si için özellikleri veya etiketleri güncelleştirir. |
> | Action | Microsoft. SQL/Servers/privateEndpointConnections/Delete | Var olan bir özel uç nokta bağlantısını siler |
> | Action | Microsoft. SQL/Servers/privateEndpointConnections/Read | Özel uç nokta bağlantılarının listesini döndürür veya belirtilen özel uç nokta bağlantısının özelliklerini alır. |
> | Action | Microsoft. SQL/Servers/privateEndpointConnections/Write | Var olan bir özel uç nokta bağlantısını onaylar veya reddeder |
> | Action | Microsoft. SQL/Servers/privateEndpointConnectionsApproval/eylem | Kullanıcının özel bir uç nokta bağlantısını onaylamasını izin verilip verilmeyeceğini belirler |
> | Action | Microsoft. SQL/Servers/privateLinkResources/okuma | Karşılık gelen SQL Server için özel bağlantı kaynaklarını alın |
> | Action | Microsoft. SQL/Servers/Providers/Microsoft. Insights/metricDefinitions/Read | Sunucular için kullanılabilen ölçümlerin dönüş türleri |
> | Action | Microsoft. SQL/Servers/Read | Sunucu listesini döndürün veya belirtilen sunucunun özelliklerini alır. |
> | Action | Microsoft. SQL/Servers/recommendedElasticPools/Databases/Read | Belirli bir sunucu için önerilen elastik veritabanı havuzları için ölçümleri alma |
> | Action | Microsoft. SQL/Servers/recommendedElasticPools/Read | Maliyeti azaltmak veya geçmiş kaynak kullanımına göre performansı artırmak için esnek veritabanı havuzları için öneri alın |
> | Action | Microsoft. SQL/Servers/recoverableDatabases/Read | Bu işlem, veritabanını son bilinen iyi yedekleme noktasına geri yüklemek için canlı veritabanının olağanüstü durum kurtarması için kullanılır. Son iyi yedekleme hakkında bilgi döndürür ancak veritabanını gerçekten geri yüklemek mümkün değildir. |
> | Action | Microsoft. SQL/Servers/replicationLinks/Read | Çoğaltma bağlantıları listesini döndürün veya belirtilen çoğaltma bağlantıları için özellikleri alır. |
> | Action | Microsoft. SQL/Servers/Restokbledroppeddatabases/Read | Hala bekletme ilkesi içinde olan belirli bir sunucuda bırakılan veritabanlarının listesini alın. |
> | Action | Microsoft. SQL/Servers/Securityalcertpolicies/operationResults/Read | Sunucu tehdit algılama ilkesi yazma işleminin sonuçlarını al |
> | Action | Microsoft. SQL/Servers/Securityalcertpolicies/Read | Belirli bir sunucu için yapılandırılmış sunucu tehdit algılama ilkelerinin listesini alma |
> | Action | Microsoft. SQL/Servers/Securityalcertpolicies/Write | Belirli bir sunucu için sunucu tehdit algılama ilkesini değiştirme |
> | Action | Microsoft. SQL/Servers/Serviceamaçlar/okuma | Belirli bir sunucuda kullanılabilen hizmet düzeyi hedeflerinin (performans katmanları olarak da bilinir) listesini alma |
> | Action | Microsoft. SQL/Servers/syncAgents/Delete | Var olan bir eşitleme aracısını siler. |
> | Action | Microsoft. SQL/Servers/syncAgents/generateKey/Action | Eşitleme Aracısı kayıt anahtarı oluştur |
> | Action | Microsoft. SQL/Servers/syncAgents/Linkedveritabanları/okuma | Eşitleme Aracısı bağlantılı veritabanlarının listesini döndürür |
> | Action | Microsoft. SQL/Servers/syncAgents/Read | Eşitleme aracılarının listesini döndürün veya belirtilen eşitleme aracısının özelliklerini alır. |
> | Action | Microsoft. SQL/Servers/syncAgents/Write | Belirtilen parametrelerle bir eşitleme Aracısı oluşturur veya belirtilen eşitleme aracısının özelliklerini güncelleştirir. |
> | Action | Microsoft. SQL/Servers/tdeCertificates/ACTION | TDE sertifikası oluştur/güncelleştir |
> | Action | Microsoft. SQL/Servers/kullanımlar/Read | Sunucu DTU kotasını ve geçerli DTU 'ı sunucu içindeki tüm veritabanları tarafından geri alma |
> | Action | Microsoft. SQL/Servers/virtualNetworkRules/Delete | Var olan bir sanal ağ kuralını siler |
> | Action | Microsoft. SQL/Servers/virtualNetworkRules/Read | Sanal ağ kurallarının listesini döndürün veya belirtilen sanal ağ kuralı için özellikleri alır. |
> | Action | Microsoft. SQL/Servers/virtualNetworkRules/Write | Belirtilen parametrelere sahip bir sanal ağ kuralı oluşturur veya belirtilen sanal ağ kuralı için özellikleri veya etiketleri güncelleştirir. |
> | Action | Microsoft. SQL/Servers/ | Belirli bir sunucu için güvenlik açığı değerlendirmesini kaldırma |
> | Action | Microsoft. SQL/Servers/ | Belirli bir sunucuda güvenlik açığı değerlendirme ilkelerini alma |
> | Action | Microsoft. SQL/Servers/ | Belirli bir sunucu için güvenlik açığı değerlendirmesini değiştirme |
> | Action | Microsoft. SQL/Servers/Write | Belirtilen parametrelere sahip bir sunucu oluşturur veya belirtilen sunucu için özellikleri veya etiketleri güncelleştirir. |
> | Action | Microsoft. SQL/Unregister/eylem | Microsoft SQL veritabanı kaynak sağlayıcısı için aboneliğin kaydını siler ve Microsoft SQL veritabanlarının oluşturulmasına izin vermez. |
> | Action | Microsoft. SQL/Virtualkümeler/Delete | Var olan bir sanal kümeyi siler. |
> | Action | Microsoft. SQL/Virtualkümeler/okuma | Sanal kümelerin listesini döndürün veya belirtilen sanal kümenin özelliklerini alır. |
> | Action | Microsoft. SQL/Virtualkümeler/Write | Sanal küme etiketlerini güncelleştirir. |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Storage/checknameavaılabılıty/Read | Hesap adının geçerli olduğunu ve kullanımda olup olmadığını denetler. |
> | Action | Microsoft. Storage/Locations/checknameavaılabılıty/Read | Hesap adının geçerli olduğunu ve kullanımda olup olmadığını denetler. |
> | Action | Microsoft. Storage/Locations/Deletevirtualnetworkoralt ağları/eylem | Microsoft. Storage 'a sanal ağ veya alt ağın silindiğini bildirir |
> | Action | Microsoft. Storage/konumlar/kullanımlar/okuma | Belirtilen abonelikteki kaynaklar için sınırı ve geçerli kullanım sayısını döndürür |
> | Action | Microsoft. Storage/işlemler/okuma | Zaman uyumsuz bir işlemin durumunu yoklar. |
> | Action | Microsoft. Storage/Register/ACTION | Depolama kaynak sağlayıcısı için aboneliği kaydeder ve depolama hesaplarının oluşturulmasına izin vermez. |
> | Action | Microsoft. Storage/SKU/okuma | Microsoft. Storage tarafından desteklenen SKU 'Ları listeler. |
> | DataAction | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/ekleme/eylem | Blob içeriği ekleme sonucunu döndürür |
> | DataAction | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/Sil | Bir blobu silmenin sonucunu döndürür |
> | DataAction | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/deleteAutomaticSnapshot/Action | Otomatik anlık görüntü silmenin sonucunu döndürür |
> | DataAction | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/filtre/eylem | Eşleşen Etiketler filtresi olan bir hesap altındaki Blobların listesini döndürür |
> | DataAction | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/blob/okuma | Blob veya Blobların listesini döndürür |
> | DataAction | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/runAsSuperUser/Action | Blob komutunun sonucunu döndürür |
> | DataAction | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/blob/Etiketler/okuma | Blob etiketlerinin okunması sonucunu döndürür |
> | DataAction | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/blob/Etiketler/yazma | Blob etiketlerinin yazılması sonucunu döndürür |
> | DataAction | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/yazma | Blob yazma sonucunu döndürür |
> | Action | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/clearLegalHold/Action | Blob kapsayıcısı yasal tutmasını temizle |
> | Action | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Delete | Bir kapsayıcıyı silmenin sonucunu döndürür |
> | Action | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Imdeğiştirici Bilitypolicies/Delete | Blob kapsayıcısı değiştirilemezlik ilkesini sil |
> | Action | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Imdeğiştirici Bilitypolicies/Genişlet/eylem | Blob kapsayıcısı değiştirilemezlik ilkesini genişlet |
> | Action | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Imdeğiştirici Bilitypolicies/Lock/Action | Blob kapsayıcısı değitirilemezliğini kilitleme ilkesi |
> | Action | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Imdeğiştirici Bilitypolicies/Read | Blob kapsayıcısı değiştirilemezlik ilkesini al |
> | Action | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Imdeğiştirici Bilitypolicies/Write | Blob kapsayıcısı değitirilemezlik ilkesi koy |
> | Action | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/kira/eylem | Kira blob kapsayıcısının sonucunu döndürür |
> | Action | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/okuma | Bir kapsayıcı döndürür |
> | Action | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/okuma | Kapsayıcı listesini döndürür |
> | Action | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/setLegalHold/Action | Blob kapsayıcısı yasal tutma ayarla |
> | Action | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Write | Patch blob kapsayıcısının sonucunu döndürür |
> | Action | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Write | Put blob kapsayıcısının sonucunu döndürür |
> | Action | Microsoft. Storage/storageAccounts/blobServices/generateUserDelegationKey/Action | Blob hizmeti için bir Kullanıcı temsili anahtarı döndürür |
> | Action | Microsoft. Storage/storageAccounts/blobServices/okuma |  |
> | Action | Microsoft. Storage/storageAccounts/blobServices/okuma | Blob hizmeti özelliklerini veya istatistiklerini döndürür |
> | Action | Microsoft. Storage/storageAccounts/blobServices/Write | Put blob hizmeti özelliklerinin sonucunu döndürür |
> | Action | Microsoft. Storage/storageAccounts/Delete | Var olan bir depolama hesabını siler. |
> | Action | Microsoft. Storage/storageAccounts/encryptionScopes/Read |  |
> | Action | Microsoft. Storage/storageAccounts/encryptionScopes/Write |  |
> | Action | Microsoft. Storage/storageAccounts/yük devretme/eylem | Müşteri, kullanılabilirlik sorunları durumunda yük devretmeyi denetleyebilir |
> | DataAction | Microsoft. Storage/storageAccounts/fileServices/fileshares/Files/actassuperuser/Action | Dosya Yöneticisi ayrıcalıklarını al |
> | DataAction | Microsoft. Storage/storageAccounts/fileServices/fileshares/Files/Delete | Dosya/klasör silmenin sonucunu döndürür |
> | DataAction | Microsoft. Storage/storageAccounts/fileServices/fileshares/Files/modifypermissions/Action | Bir dosya/klasör üzerinde izin değiştirme sonucunu döndürür |
> | DataAction | Microsoft. Storage/storageAccounts/fileServices/fileshares/Files/Read | Dosya/klasör veya dosya/klasör listesi döndürür |
> | DataAction | Microsoft. Storage/storageAccounts/fileServices/fileshares/Files/Write | Bir dosya yazmanın veya bir klasör oluşturmanın sonucunu döndürür |
> | Action | Microsoft. Storage/storageAccounts/fileServices/Read |  |
> | Action | Microsoft. Storage/storageAccounts/fileServices/Read | Dosya hizmeti özelliklerini al |
> | Action | Microsoft. Storage/storageAccounts/fileServices/paylaşımlar/Sil |  |
> | Action | Microsoft. Storage/storageAccounts/fileServices/paylaşımlar/okuma |  |
> | Action | Microsoft. Storage/storageAccounts/fileServices/paylaşımlar/okuma |  |
> | Action | Microsoft. Storage/storageAccounts/fileServices/paylaşımlar/yazma |  |
> | Action | Microsoft. Storage/storageAccounts/fileServices/Write |  |
> | Action | Microsoft. Storage/storageAccounts/listAccountSas/eylem | Belirtilen depolama hesabı için hesap SAS belirtecini döndürür. |
> | Action | Microsoft. Storage/storageAccounts/ListKeys/Action | Belirtilen depolama hesabı için erişim anahtarlarını döndürür. |
> | Action | Microsoft. Storage/storageAccounts/listServiceSas/eylem | Belirtilen depolama hesabı için hizmet SAS belirtecini döndürür. |
> | Action | Microsoft. Storage/storageAccounts/managementPolicies/Delete | Depolama hesabı yönetim ilkelerini sil |
> | Action | Microsoft. Storage/storageAccounts/managementPolicies/okuma | Depolama yönetim hesabı ilkelerini alın |
> | Action | Microsoft. Storage/storageAccounts/managementPolicies/Write | Depolama hesabı yönetim ilkelerini yürürlüğe koy |
> | Action | Microsoft. Storage/storageAccounts/Privateendpointconnectionproxy/Delete | Özel uç nokta bağlantı proxy 'Lerini Sil |
> | Action | Microsoft. Storage/storageAccounts/Privateendpointconnectionproxy/okuma | Özel uç nokta bağlantısı proxy 'Si al |
> | Action | Microsoft. Storage/storageAccounts/Privateendpointconnectionproxy/yazma | Özel uç nokta bağlantı proxy 'Leri koy |
> | Action | Microsoft. Storage/storageAccounts/privateEndpointConnections/Delete | Özel uç nokta bağlantısını sil |
> | Action | Microsoft. Storage/storageAccounts/privateEndpointConnections/Read | Özel uç nokta bağlantısı al |
> | Action | Microsoft. Storage/storageAccounts/privateEndpointConnections/Write | Özel uç nokta bağlantısı koy |
> | Action | Microsoft. Storage/storageAccounts/PrivateEndpointConnectionsApproval/eylem | Özel uç nokta bağlantılarını Onayla |
> | Action | Microsoft. Storage/storageAccounts/privateLinkResources/Read | StorageAccount grup kimliklerini al |
> | Action | Microsoft. Storage/storageAccounts/queueServices/kuyruklar/Sil | Kuyruğu silmenin sonucunu döndürür |
> | DataAction | Microsoft. Storage/storageAccounts/queueServices/kuyruklar/iletiler/ekleme/eylem | İleti eklemenin sonucunu döndürür |
> | DataAction | Microsoft. Storage/storageAccounts/queueServices/kuyruklar/iletiler/Sil | İletiyi silmenin sonucunu döndürür |
> | DataAction | Microsoft. Storage/storageAccounts/queueServices/kuyruklar/iletiler/işlem/eylem | İleti işlemenin sonucunu döndürür |
> | DataAction | Microsoft. Storage/storageAccounts/queueServices/kuyruklar/iletiler/okundu | Bir ileti döndürür |
> | DataAction | Microsoft. Storage/storageAccounts/queueServices/kuyruklar/iletiler/yazma | İleti yazmanın sonucunu döndürür |
> | Action | Microsoft. Storage/storageAccounts/queueServices/kuyrukları/okuma | Kuyruğu veya kuyruk listesini döndürür. |
> | Action | Microsoft. Storage/storageAccounts/queueServices/kuyruklar/Write | Sıra yazmanın sonucunu döndürür |
> | Action | Microsoft. Storage/storageAccounts/queueServices/Read | Kuyruk hizmeti özelliklerini al |
> | Action | Microsoft. Storage/storageAccounts/queueServices/Read | Sıra hizmeti özelliklerini veya istatistiklerini döndürür. |
> | Action | Microsoft. Storage/storageAccounts/queueServices/Write | Sıra hizmeti özelliklerini ayarlamanın sonucunu döndürür |
> | Action | Microsoft. Storage/storageAccounts/Read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Action | Microsoft. Storage/storageAccounts/RegenerateKey/ACTION | Belirtilen depolama hesabı için erişim anahtarlarını yeniden oluşturur. |
> | Action | Microsoft. Storage/storageAccounts/restoreBlobRanges/eylem | Blob aralıklarını belirtilen sürenin durumuna geri yükle |
> | Action | Microsoft. Storage/storageAccounts/Iptal Keuserdelegationkeys/Action | Belirtilen depolama hesabı için tüm Kullanıcı temsili anahtarlarını iptal eder. |
> | Action | Microsoft. Storage/storageAccounts/Services/diagnosticSettings/Write | Depolama hesabı tanılama ayarlarını oluşturun/güncelleştirin. |
> | Action | Microsoft. Storage/storageAccounts/tableServices/Read | Tablo hizmeti özelliklerini al |
> | Action | Microsoft. Storage/storageAccounts/Write | Belirtilen parametrelerle bir depolama hesabı oluşturur veya özellikleri ya da etiketleri güncelleştirir veya belirtilen depolama hesabı için özel etki alanı ekler. |
> | Action | Microsoft. Storage/kullanımlar/okuma | Belirtilen abonelikteki kaynaklar için sınırı ve geçerli kullanım sayısını döndürür |

## <a name="microsoftstoragesync"></a>Microsoft. storagessync

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. storagessync/Locations/Checknameavaılabılıty/Action | Depolama eşitleme hizmeti adının geçerli olduğunu ve kullanımda olup olmadığını denetler. |
> | Action | Microsoft. storagessync/Locations/iş akışları/işlemler/okuma | Zaman uyumsuz bir işlemin durumunu alır |
> | Action | Microsoft. storagessync/işlemler/okuma | Desteklenen Işlemlerin bir listesini alır |
> | Action | Microsoft. storagessync/Register/ACTION | Depolama eşitleme sağlayıcısı için aboneliği kaydeder |
> | Action | Microsoft. storagessync/storageSyncServices/Delete | Tüm depolama eşitleme hizmetlerini silme |
> | Action | Microsoft. storagessync/storageSyncServices/Providers/Microsoft. Insights/metricDefinitions/Read | Depolama Eşitleme Hizmetleri için kullanılabilir ölçümleri alır |
> | Action | Microsoft. storagessync/storageSyncServices/Read | Tüm depolama eşitleme hizmetlerini okuyun |
> | Action | Microsoft. storagessync/storageSyncServices/registeredServers/Delete | Kayıtlı tüm sunucuyu Sil |
> | Action | Microsoft. storagessync/storageSyncServices/registeredServers/Providers/Microsoft. Insights/metricDefinitions/Read | Kayıtlı sunucu için kullanılabilir ölçümleri alır |
> | Action | Microsoft. storagessync/storageSyncServices/registeredServers/Read | Kayıtlı tüm sunucuları okuyun |
> | Action | Microsoft. storagessync/storageSyncServices/registeredServers/Write | Kayıtlı sunucu oluştur veya güncelleştir |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/cloudEndpoints/Delete | Tüm bulut uç noktalarını silme |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/cloudEndpoints/operationresults/Read | Zaman uyumsuz bir yedekleme/geri yükleme işleminin durumunu alır |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/cloudEndpoints/postbackup/Action | Yedeklemeden sonra bu eylemi çağır |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/cloudEndpoints/PostRestore/Action | Geri yüklemeden sonra bu eylemi çağır |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/cloudEndpoints/prebackup/Action | Yedeklemeden önce bu eylemi çağırın |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/cloudEndpoints/preresıte/Action | Geri yüklemeden önce bu eylemi çağırın |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/cloudEndpoints/Read | Tüm bulut uç noktalarını okuyun |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/cloudEndpoints/restoresinyal/eylem | Sinyal geri yükleme |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/cloudEndpoints/Write | Herhangi bir bulut uç noktası oluşturun veya güncelleştirin |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/Delete | Tüm eşitleme gruplarını silme |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/Providers/Microsoft. Insights/metricDefinitions/Read | Eşitleme grupları için kullanılabilir ölçümleri alır |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/Read | Tüm eşitleme gruplarını okuyun |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/serverEndpoints/Delete | Tüm sunucu uç noktalarını silme |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/serverEndpoints/Providers/Microsoft. Insights/metricDefinitions/Read | Sunucu uç noktaları için kullanılabilir ölçümleri alır |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/Sunucuendpoints/Read | Tüm sunucu uç noktalarını okuyun |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/Sunucuendpoints/yeniden hesaplama/eylem | Dosyaları bir sunucuya geri çekmek için bu eylemi çağırın |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/serverEndpoints/Write | Sunucu uç noktalarını oluşturma veya güncelleştirme |
> | Action | Microsoft. storagessync/storageSyncServices/syncGroups/Write | Tüm eşitleme gruplarını oluşturun veya güncelleştirin |
> | Action | Microsoft. storagessync/storageSyncServices/iş akışları/operationresults/Read | Zaman uyumsuz bir işlemin durumunu alır |
> | Action | Microsoft. storagessync/storageSyncServices/iş akışları/işlemler/okuma | Zaman uyumsuz bir işlemin durumunu alır |
> | Action | Microsoft. storagessync/storageSyncServices/iş akışları/okuma | Iş akışlarını oku |
> | Action | Microsoft. storagessync/storageSyncServices/Write | Depolama Eşitleme Hizmetleri oluşturun veya güncelleştirin |
> | Action | Microsoft. storagessync/Unregister/Action | Depolama eşitleme sağlayıcısı için aboneliğin kaydını siler |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. StorSimple/yöneticileri/accessControlRecords/Delete | Access Control kayıtlarını siler |
> | Action | Microsoft. StorSimple/yöneticileri/accessControlRecords/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/accessControlRecords/okuma | Access Control kayıtları listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/accessControlRecords/Write | Access Control kayıtlarını oluşturma veya güncelleştirme |
> | Action | Microsoft. StorSimple/yöneticileri/uyarıları/okuma | Uyarıları listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/yedeklemeleri/okuma | Yedekleme kümesini listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/bandwidthSettings/Delete | Var olan bir bant genişliği ayarlarını siler (yalnızca 8000 serisi) |
> | Action | Microsoft. StorSimple/yöneticileri/bandwidthSettings/operationResults/Read | Işlem sonuçlarını listeleme |
> | Action | Microsoft. StorSimple/yöneticileri/bandwidthSettings/Read | Bant genişliği ayarlarını listeleme (yalnızca 8000 serisi) |
> | Action | Microsoft. StorSimple/yöneticileri/bandwidthSettings/Write | Yeni bir veya güncelleştirme bant genişliği ayarları oluşturur (yalnızca 8000 serisi) |
> | Action | Microsoft. StorSimple/yöneticileri/sertifikaları/yazma | Sertifikaları oluşturma veya güncelleştirme |
> | Action | Microsoft. StorSimple/yöneticileri/sertifikaları/yazma | Kaynak sertifikası güncelleştirme işlemi kaynak/kasa kimlik bilgisi sertifikasını güncelleştirir. |
> | Action | Microsoft. StorSimple/yöneticileri/clearAlerts/Action | Cihaz yöneticisiyle ilişkili tüm uyarıları temizleyin. |
> | Action | Microsoft. StorSimple/yöneticileri/Cloudapplıanceconfigurations/Read | Bulut gereci desteklenen yapılandırmalarının listesini listeleyin |
> | Action | Microsoft. StorSimple/yöneticileri/configureDevice/Action | Bir cihazı yapılandırır |
> | Action | Microsoft. StorSimple/yöneticileri/silme | Cihaz yöneticilerini siler |
> | Action | Microsoft. StorSimple/yöneticileri/silme | Kasayı Sil işlemi, ' kasa ' türündeki belirtilen Azure kaynağını siler |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/alertSettings/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/alertSettings/Read | Uyarı ayarlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/alertSettings/Write | Uyarı ayarlarını oluşturma veya güncelleştirme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/authorizeForServiceEncryptionKeyRollover/Action | Cihazların hizmet şifreleme anahtarı geçişi için yetki verme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/backupPolicies/Backup/Action | İlke tarafından korunan tüm birimlerin isteğe bağlı yedeklemesini oluşturmak için el ile yedekleme yapın. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/backupPolicies/Delete | Var olan bir yedekleme Ilkelerini siler (yalnızca 8000 serisi) |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/backupPolicies/operationResults/Read | Işlem sonuçlarını listeleme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/backupPolicies/Read | Yedekleme Ilkelerini listeleyin (yalnızca 8000 serisi) |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/backupPolicies/zamanlamalar/silme | Var olan zamanlamaları siler |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/backupPolicies/zamanlamalar/operationResults/Read | Işlem sonuçlarını listeleme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/backupPolicies/zamanlamalar/okuma | Zamanlamaları listeleme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/backupPolicies/zamanlamalar/yazma | Yeni bir veya güncelleştirmeler zamanlaması oluşturur |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/backupPolicies/Write | Yeni veya güncelleştirme yedekleme Ilkeleri oluşturur (yalnızca 8000 serisi) |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/yedeklemeleri/silme | Yedekleme kümesini siler |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/yedeklemeleri/öğeleri/kopya/eylem | Bir yedekleme öğesi kullanarak bir paylaşma veya birimi klonlayın. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/yedeklemeleri/öğeleri/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/yedeklemeleri/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/yedeklemeleri/okuma | Yedekleme kümesini listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/yedeklemeleri/geri yükleme/eylem | Tüm birimleri bir yedekleme kümesinden geri yükleyin. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/backupScheduleGroups/Delete | Yedekleme zamanlama gruplarını siler |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/backupScheduleGroups/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/backupScheduleGroups/Read | Yedekleme zamanlaması gruplarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/backupScheduleGroups/Write | Yedekleme zamanlama gruplarını oluşturun veya güncelleştirin |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Bölümayarları/silme | CHAP ayarlarını siler |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Bölüsettings/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Bölüsettings/Read | CHAP ayarlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Bölüsettings/Write | CHAP ayarlarını oluşturma veya güncelleştirme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/devre dışı bırakma/eylem | Bir cihazı devre dışı bırakır. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/silme | Cihazları siler |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/diskleri/okuma | Diskleri listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/indirme/eylem | Bir cihaz için yükleme güncelleştirmelerini yükleyin. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/yük devretme/eylem | Cihazın yük devretmesi. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/yük devretme/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/failoverTargets/okuma | Cihazların yük devretme hedeflerini listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/dosya sunucuları/yedekleme/eylem | Bir dosya sunucusunun yedeğini alın. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/dosya sunucuları/silme | Dosya sunucularını siler |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/fileservers/ölçümler/okuma | Ölçümleri listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/dosya sunucuları/metricsDefinitions/okuma | Ölçüm tanımlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/fileservers/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/dosya sunucuları/okuma | Dosya sunucularını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/fileservers/paylaşımlar/Sil | Paylaşımları siler |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/fileservers/paylaşımlar/ölçümler/okuma | Ölçümleri listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazlar/fileservers/paylaşımlar/metricsDefinitions/okuma | Ölçüm tanımlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/fileservers/paylaşımlar/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/fileservers/paylaşımlar/okuma | Paylaşımları listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/fileservers/paylaşımlar/yazma | Paylaşımlar oluşturun veya güncelleştirin |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/fileservers/Write | Dosya sunucularını oluşturma veya güncelleştirme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/hardwareComponentGroups/changeControllerPowerState/Action | Donanım bileşen gruplarının denetleyici güç durumunu değiştirme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/hardwareComponentGroups/operationResults/Read | Işlem sonuçlarını listeleme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/hardwareComponentGroups/Read | Donanım bileşen gruplarını listeleyin |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Install/Action | Güncelleştirmeleri bir cihaza yükler. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/ınstallupdates/Action | Güncelleştirmeleri cihazlara (yalnızca 8000 seriler) yüklenir. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/SCC/yedekleme/eylem | Bir Iscsı sunucusunun yedeğini alın. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/SCC/Sil | Iscsı sunucularını siler |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/SCC/diskler/Sil | Diskleri siler |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/SCC/Ise/diskler/ölçümler/okuma | Ölçümleri listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/SCC/Ise/diskler/metricsDefinitions/okuma | Ölçüm tanımlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/SCC/Ise/diskler/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/SCC/diskler/okunan | Diskleri listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/SCC/diskler/yazma | Diskleri oluşturma veya güncelleştirme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/SCC/Ise/ölçümler/okuma | Ölçümleri listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/SCC/nSite/metricsDefinitions/okuma | Ölçüm tanımlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/SCC/Ise/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/SCC/okuma | Iscsı sunucularını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/SCC/yazma | Iscsı sunucularını oluşturma veya güncelleştirme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/işleri/iptal/eylem | Çalışan bir işi iptal etme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/işleri/operationResults/Read | Işlem sonuçlarını listeleme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/işleri/okuma | Işleri listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/listFailoverSets/Action | Mevcut bir cihaz için yük devretme kümelerini listeleyin (yalnızca 8000 serisi). |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/listFailoverTargets/eylem | Cihazların yük devretme hedeflerini listeleyin (yalnızca 8000 serisi). |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/ölçümleri/okuma | Ölçümleri listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/metricsDefinitions/okuma | Ölçüm tanımlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazlar/migrationSourceConfigurations/confirmMigration/Action | Başarılı bir geçiş onaylar ve bunu işleyin. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/migrationSourceConfigurations/confirmMigrationStatus/Read | Geçiş durumunu onaylayın listesini listeleyin |
> | Action | Microsoft. StorSimple/yöneticileri/cihazlar/migrationSourceConfigurations/fetchConfirmMigrationStatus/Action | Geçişin onaylama durumunu getirir. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazlar/migrationSourceConfigurations/Fetchmigrationtahmine/eylem | Geçiş tahmini işinin durumunu getirin. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazlar/migrationSourceConfigurations/fetchMigrationStatus/Action | Geçişin durumunu getirin. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazlar/migrationSourceConfigurations/Import/Action | Geçiş için kaynak konfigürasyonları içeri aktarma |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/migrationSourceConfigurations/Migrationtahmin/okuma | Geçiş tahminini listeleyin |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/migrationSourceConfigurations/migrationStatus/Read | Geçiş durumunu listeleyin |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/migrationSourceConfigurations/operationResults/Read | Işlem sonuçlarını listeleme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazlar/migrationSourceConfigurations/Başlangıçgeçişi/eylem | Kaynak yapılandırmasını kullanarak geçişi Başlat |
> | Action | Microsoft. StorSimple/yöneticileri/cihazlar/migrationSourceConfigurations/Startmigrationtahmine/eylem | Geçiş işleminin süresini tahmin etmek için bir iş başlatın. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/networkSettings/operationResults/Read | Işlem sonuçlarını listeleme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/networkSettings/Read | Ağ ayarlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/networkSettings/Write | Yeni bir ağ ayarları oluşturur veya güncelleştirir |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/publicEncryptionKey/Action | Cihaz yöneticisinin genel şifreleme anahtarını Listele |
> | Action | Microsoft. StorSimple/yöneticileri/cihazlar/publishSupportPackage/Action | Mevcut bir cihaz için destek paketini yayımlayın. StorSimple destek paketi, tüm StorSimple cihaz sorunlarını gidermeye yönelik Microsoft Desteği yardımcı olacak tüm ilgili günlükleri toplayan kullanımı kolay bir mekanizmadır. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/okuma | Cihazları listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/scanForUpdates/Action | Bir cihazdaki güncelleştirmeleri tarayın. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/securitySettings/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/securitySettings/Read | Güvenlik ayarlarını listeleyin |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/securitySettings/syncRemoteManagementCertificate/Action | Bir cihaz için uzaktan yönetim sertifikasını eşitler. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/securitySettings/Update/Action | Güvenlik ayarlarını güncelleştirin. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/securitySettings/Write | Yeni bir güvenlik ayarları oluşturur veya güncelleştirir |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/sendTestAlertEmail/eylem | Yapılandırılmış e-posta alıcılarına test uyarı e-postası gönderin. |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/paylaşımları/okuma | Paylaşımları listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/timeSettings/operationResults/Read | Işlem sonuçlarını listeleme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/timeSettings/Read | Zaman ayarlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/timeSettings/Write | Yeni veya güncelleştirme zaman ayarlarını oluşturur |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/güncelleştirmeler/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/updateSummary/Read | Güncelleştirme özetini listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Birimkapsayıcıları/silme | Var olan bir birim kapsayıcılarını siler (yalnızca 8000 serisi) |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Birimkapsayıcıları/ölçümleri/okuma | Ölçümleri listeleyin |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Birimkapsayıcıları/metricsDefinitions/okuma | Ölçüm tanımlarını listeleyin |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Volumecontainer/operationResults/Read | Işlem sonuçlarını listeleme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Birimkapsayıcıları/okuma | Birim kapsayıcılarını listeleme (yalnızca 8000 serisi) |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Birimkapsayıcıları/birimleri/silme | Varolan birimleri siler |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Birimkapsayıcıları/birimleri/ölçümleri/okuma | Ölçümleri listeleyin |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Birimkapsayıcıları/birimleri/metricsDefinitions/okuma | Ölçüm tanımlarını listeleyin |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Birimkapsayıcıları/birimleri/operationResults/Read | Işlem sonuçlarını listeleme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Birimkapsayıcıları/birimleri/okuma | Birimleri listeleme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Birimkapsayıcıları/birimleri/yazma | Yeni veya güncelleştirme birimleri oluşturur |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/Birimkapsayıcıları/yazma | Yeni veya güncelleştirme birimi kapsayıcıları oluşturur (yalnızca 8000 serisi) |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/birimleri/okuma | Birimleri listeleme |
> | Action | Microsoft. StorSimple/yöneticileri/cihazları/yazma | Cihazları oluşturma veya güncelleştirme |
> | Action | Microsoft. StorSimple/yöneticileri/encryptionSettings/Read | Şifreleme ayarlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/Extendedınformation/Delete | Genişletilmiş kasa bilgilerini siler |
> | Action | Microsoft. StorSimple/yöneticileri/Extendedınformation/Delete | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Action | Microsoft. StorSimple/yöneticileri/Extendedınformation/Read | Genişletilmiş kasa bilgilerini listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/Extendedınformation/Read | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Action | Microsoft. StorSimple/yöneticileri/Extendedınformation/Write | Genişletilmiş kasa bilgilerini oluşturma veya güncelleştirme |
> | Action | Microsoft. StorSimple/yöneticileri/Extendedınformation/Write | Daha Fazla Bilgi Al işlemi, ?vault? türündeki Azure kaynağını temsil eden nesnenin Daha Fazla Bilgi değerini alır. |
> | Action | Microsoft. StorSimple/yöneticileri/Özellikler/okuma | Özellikleri listeleyin |
> | Action | Microsoft. StorSimple/yöneticileri/fileservers/okuma | Dosya sunucularını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/getEncryptionKey/Action | Cihaz Yöneticisi için şifreleme anahtarı alın. |
> | Action | Microsoft. StorSimple/yöneticileri/SCC/okunan | Iscsı sunucularını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/işleri/okuma | Işleri listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/listActivationKey/Action | StorSimple Aygıt Yöneticisi etkinleştirme anahtarını alır. |
> | Action | Microsoft. StorSimple/yöneticileri/listPublicEncryptionKey/Action | StorSimple Aygıt Yöneticisi ortak şifreleme anahtarlarını listeleyin. |
> | Action | Microsoft. StorSimple/yöneticileri/ölçümleri/okuma | Ölçümleri listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/metricsDefinitions/okuma | Ölçüm tanımlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/migrateClassicToResourceManager/Action | Klasik Kaynak Yöneticisi cihazları |
> | Action | Microsoft. StorSimple/yöneticileri/migrationSourceConfigurations/okuma | Geçiş kaynağı yapılandırmasını listeleme (yalnızca 8000 serisi) |
> | Action | Microsoft. StorSimple/yöneticileri/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/provisionCloudAppliance/eylem | Yeni bir bulut gereci oluşturun. |
> | Action | Microsoft. StorSimple/yöneticileri/okuma | Cihaz yöneticilerini listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/okuma | Kasayı al işlemi, ' kasa ' türündeki Azure kaynağını temsil eden bir nesneyi alır |
> | Action | Microsoft. StorSimple/yöneticileri/regenerateActivationKey/Action | Mevcut bir StorSimple Aygıt Yöneticisi için etkinleştirme anahtarını yeniden oluşturun. |
> | Action | Microsoft. StorSimple/yöneticileri/storageAccountCredentials/Delete | Depolama hesabı kimlik bilgilerini siler |
> | Action | Microsoft. StorSimple/yöneticileri/storageAccountCredentials/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/storageAccountCredentials/Read | Depolama hesabı kimlik bilgilerini listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/storageAccountCredentials/Write | Depolama hesabı kimlik bilgilerini oluşturun veya güncelleştirin |
> | Action | Microsoft. StorSimple/yöneticileri/storageDomains/Delete | Depolama etki alanlarını siler |
> | Action | Microsoft. StorSimple/yöneticileri/storageDomains/operationResults/Read | Işlem sonuçlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/storageDomains/Read | Depolama alanlarını listeler veya alır |
> | Action | Microsoft. StorSimple/yöneticileri/storageDomains/Write | Depolama alanlarını oluşturma veya güncelleştirme |
> | Action | Microsoft. StorSimple/yöneticileri/yazma | Cihaz yöneticilerini oluşturma veya güncelleştirme |
> | Action | Microsoft. StorSimple/yöneticileri/yazma | Kasa Oluştur işlemi, 'vault' türünde bir Azure kaynağı oluşturur |
> | Action | Microsoft. StorSimple/işlemler/okuma | Işlemleri listeler veya alır |
> | Action | Microsoft. StorSimple/Register/ACTION | Sağlayıcıyı kaydet Microsoft. StorSimple |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. StreamAnalytics/konumlar/kotalar/okundu | Stream Analytics abonelik kotasını okuyun |
> | Action | Microsoft. StreamAnalytics/işlemler/okuma | Stream Analytics Işlemleri oku |
> | Action | Microsoft. StreamAnalytics/Register/ACTION | Aboneliği Stream Analytics kaynak sağlayıcısına Kaydet |
> | Action | Microsoft. StreamAnalytics/streamingjobs/Sil | Stream Analytics Işini Sil |
> | Action | Microsoft. StreamAnalytics/streamingjobs/işlevler/Sil | Stream Analytics Işi Işlevini Sil |
> | Action | Microsoft. StreamAnalytics/streamingjobs/işlevler/operationresults/Read | Stream Analytics Job Işlevi için işlem sonuçlarını oku |
> | Action | Microsoft. StreamAnalytics/streamingjobs/işlevler/okuma | Stream Analytics Işi Işlevini oku |
> | Action | Microsoft. StreamAnalytics/streamingjobs/işlevler/RetrieveDefaultDefinition/eylem | Stream Analytics Işi Işlevinin varsayılan tanımını alma |
> | Action | Microsoft. StreamAnalytics/streamingjobs/işlevler/test/eylem | Test Stream Analytics Işi Işlevi |
> | Action | Microsoft. StreamAnalytics/streamingjobs/işlevler/yazma | Stream Analytics Iş Işlevini yaz |
> | Action | Microsoft. StreamAnalytics/streamingjobs/giriş/silme | Stream Analytics Iş girişini sil |
> | Action | Microsoft. StreamAnalytics/streamingjobs/giriş/operationresults/Read | Stream Analytics Iş girişi için işlem sonuçlarını oku |
> | Action | Microsoft. StreamAnalytics/streamingjobs/giriş/okuma | Stream Analytics Iş girişini oku |
> | Action | Microsoft. StreamAnalytics/streamingjobs/girişler/örnek/eylem | Örnek Stream Analytics Işi girişi |
> | Action | Microsoft. StreamAnalytics/streamingjobs/girişler/test/eylem | Test Stream Analytics Işi girişi |
> | Action | Microsoft. StreamAnalytics/streamingjobs/giriş/yazma | Stream Analytics Işi girişi yazma |
> | Action | Microsoft. StreamAnalytics/streamingjobs/MetricDefinitions/okuma | Ölçüm tanımlarını oku |
> | Action | Microsoft. StreamAnalytics/streamingjobs/operationresults/Read | Stream Analytics Iş için işlem sonuçlarını oku |
> | Action | Microsoft. StreamAnalytics/streamingjobs/çıkışlar/silme | Stream Analytics Iş çıkışını Sil |
> | Action | Microsoft. StreamAnalytics/streamingjobs/çıkışlar/operationresults/Read | Stream Analytics Iş çıktısı için işlem sonuçlarını oku |
> | Action | Microsoft. StreamAnalytics/streamingjobs/çıktılar/okuma | Stream Analytics Iş çıktısını oku |
> | Action | Microsoft. StreamAnalytics/streamingjobs/çıktılar/test/eylem | Test Stream Analytics Iş çıktısı |
> | Action | Microsoft. StreamAnalytics/streamingjobs/çıktılar/yazma | Stream Analytics Iş çıkışı yaz |
> | Action | Microsoft. StreamAnalytics/streamingjobs/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Read | Tanılama ayarını oku. |
> | Action | Microsoft. StreamAnalytics/streamingjobs/sağlayıcılar/Microsoft. Insights/diagnosticSettings/Write | Tanılama ayarı yaz. |
> | Action | Microsoft. StreamAnalytics/streamingjobs/sağlayıcılar/Microsoft. Insights/logDefinitions/Read | Streamingjobs için kullanılabilir günlükleri alır |
> | Action | Microsoft. StreamAnalytics/streamingjobs/sağlayıcılar/Microsoft. Insights/metricDefinitions/okuma | Streamingjobs için kullanılabilir ölçümleri alır |
> | Action | Microsoft. StreamAnalytics/streamingjobs/okuma | Stream Analytics Işi oku |
> | Action | Microsoft. StreamAnalytics/streamingjobs/Başlat/eylem | Stream Analytics Işi Başlat |
> | Action | Microsoft. StreamAnalytics/streamingjobs/durdur/eylem | Stream Analytics Işini durdur |
> | Action | Microsoft. StreamAnalytics/streamingjobs/dönüşümler/silme | Stream Analytics Işi dönüşümünü Sil |
> | Action | Microsoft. StreamAnalytics/streamingjobs/dönüşümler/okuma | Stream Analytics Işi dönüşümünü oku |
> | Action | Microsoft. StreamAnalytics/streamingjobs/dönüşümler/yazma | Stream Analytics Işi dönüşümünü yaz |
> | Action | Microsoft. StreamAnalytics/streamingjobs/yazma | Stream Analytics Işi yaz |

## <a name="microsoftsubscription"></a>Microsoft. Subscription

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Subscription/Cancel/ACTION | Aboneliği iptal eder |
> | Action | Microsoft. Subscription/CreateSubscription/eylem | Bir Azure aboneliği oluşturun |
> | Action | Microsoft. Subscription/Register/ACTION | Aboneliği Microsoft. Subscription kaynak sağlayıcısına kaydeder |
> | Action | Microsoft. abonelik/yeniden adlandırma/eylem | Aboneliği yeniden adlandırır |
> | Action | Microsoft. Subscription/SubscriptionDefinitions/okuma | Bir yönetim grubu içinde bir Azure abonelik tanımı alın. |
> | Action | Microsoft. Subscription/SubscriptionDefinitions/yaz | Azure abonelik tanımı oluşturma |

## <a name="microsoftsupport"></a>Microsoft. support

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft.Support/register/action | Destek Kaynağı Sağlayıcısına Kayıt Yapar |
> | Action | Microsoft.Support/supportTickets/read | Durum, önem derecesi, kişi ayrıntıları ve iletişimler gibi Destek Biletleri ayrıntılarını alır veya aboneliklerdeki Destek Biletleri listesini alır. |
> | Action | Microsoft.Support/supportTickets/write | Destek Bileti oluşturur veya güncelleştirir. Teknik, Faturalama, Kotalar veya Abonelik Yönetimi konusunda bir Destek Bileti oluşturabilirsiniz. Var olan destek biletlerinin önem derecesini, iletişim bilgilerini ve iletişimlerini güncelleştirebilirsiniz. |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Timeseriesınsights/ortamlar/accesspolicies/Delete | Erişim ilkesini siler. |
> | Action | Microsoft. Timeseriesınsights/ortamlar/accesspolicies/Read | Erişim ilkesinin özelliklerini alın. |
> | Action | Microsoft. Timeseriesınsights/ortamlar/accesspolicies/Write | Bir ortam için yeni bir erişim ilkesi oluşturur veya var olan bir erişim ilkesini güncelleştirir. |
> | Action | Microsoft. Timeseriesınsights/ortamlar/Sil | Ortamı siler. |
> | Action | Microsoft. Timeseriesınsights/ortamlar/EventSources/Sil | Olay kaynağını siler. |
> | Action | Microsoft. Timeseriesınsights/ortamlar/EventSources/okuma | Bir olay kaynağının özelliklerini alın. |
> | Action | Microsoft. Timeseriesınsights/ortamlar/EventSources/yazma | Bir ortam için yeni bir olay kaynağı oluşturur veya var olan bir olay kaynağını güncelleştirir. |
> | Action | Microsoft. Timeseriesınsights/ortamlar/okuma | Bir ortamın özelliklerini alın. |
> | Action | Microsoft. Timeseriesınsights/ortamlar/referencedataset/Delete | Başvuru veri kümesini siler. |
> | Action | Microsoft. Timeseriesınsights/ortamlar/referencedataset/Read | Başvuru veri kümesinin özelliklerini alın. |
> | Action | Microsoft. Timeseriesınsights/ortamlar/referencedataset/Write | Bir ortam için yeni bir başvuru veri kümesi oluşturur veya var olan bir başvuru veri kümesini güncelleştirir. |
> | Action | Microsoft. Timeseriesınsights/ortamlar/Status/Read | Ortam durumunu, ilişkili işlemlerinin durumunu alma gibi bir duruma alın. |
> | Action | Microsoft. Timeseriesınsights/ortamlar/Write | Yeni bir ortam oluşturur veya var olan bir ortamı güncelleştirir. |
> | Action | Microsoft. Timeseriesınsights/Register/ACTION | Time Series Insights kaynak sağlayıcısı için aboneliği kaydeder ve Time Series Insights ortamların oluşturulmasını sunar. |

## <a name="microsoftvisualstudio"></a>Microsoft. VisualStudio

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. VisualStudio/Account/Delete | Hesabı Sil |
> | Action | Microsoft. VisualStudio/Account/EXTENSION/Read | Hesabı/uzantıyı oku |
> | Action | Microsoft. VisualStudio/Account/Project/Read | Hesabı/projeyi oku |
> | Action | Microsoft. VisualStudio/Account/Project/Write | Hesabı/projeyi ayarla |
> | Action | Microsoft. VisualStudio/Account/Read | Hesabı oku |
> | Action | Microsoft. VisualStudio/Account/Write | Hesabı ayarla |
> | Action | Microsoft. VisualStudio/Extension/Delete | Uzantıyı Sil |
> | Action | Microsoft. VisualStudio/Extension/Read | Uzantıyı oku |
> | Action | Microsoft. VisualStudio/Extension/Write | Uzantı ayarla |
> | Action | Microsoft. VisualStudio/Project/Delete | Projeyi Sil |
> | Action | Microsoft. VisualStudio/Project/Read | Projeyi oku |
> | Action | Microsoft. VisualStudio/Project/Write | Projeyi ayarla |
> | Action | Microsoft. VisualStudio/Register/ACTION | Azure aboneliğini Microsoft. VisualStudio sağlayıcısıyla kaydetme |

## <a name="microsoftweb"></a>microsoft.web

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. Web/apimanagementaccounts/apiacl/okuma | API yönetim hesapları Apiacl 'Lerini alın. |
> | Action | Microsoft. Web/apimanagementaccounts/API/apiacl/Delete | API Management hesapları API 'Leri silme Apiacl 'leri. |
> | Action | Microsoft. Web/apimanagementaccounts/API/apiacl/okuma | API Management hesapları API 'Leri al Apiacl 'leri. |
> | Action | Microsoft. Web/apimanagementaccounts/API/apiacl/Write | API Management hesapları API 'Leri güncelleştirme Apiacl 'Leri. |
> | Action | Microsoft. Web/apimanagementaccounts/API/connectionacl/okuma | API Management hesapları API 'Leri Connectionacl 'Ler alın. |
> | Action | Microsoft. Web/apimanagementaccounts/API 'ler/bağlantılar/confirmconsentcode/Action | Onay kodu API yönetim hesapları API 'Leri bağlantılarını onaylayın. |
> | Action | Microsoft. Web/apimanagementaccounts/API/Connections/connectionacl/Delete | API Management hesaplarını silme API 'Leri bağlantı ACL 'leri. |
> | Action | Microsoft. Web/apimanagementaccounts/API/Connections/connectionacl/okuma | API yönetim hesapları API 'Leri bağlantı ACL 'lerini alın. |
> | Action | Microsoft. Web/apimanagementaccounts/API/Connections/connectionacl/yazma | API Management hesaplarını güncelleştirme API 'Leri bağlantı ACL 'leri. |
> | Action | Microsoft. Web/apimanagementaccounts/API/bağlantılar/silme | API Management hesapları API 'Leri bağlantılarını silin. |
> | Action | Microsoft. Web/apimanagementaccounts/API/Connections/getconsentlinks/Action | API Management hesapları API 'Leri bağlantıları için onay bağlantıları alın. |
> | Action | Microsoft. Web/apimanagementaccounts/API 'ler/bağlantılar/listconnectionkeys/ACTION | Bağlantı anahtarları API yönetim hesapları API 'Leri bağlantılarını listeleyin. |
> | Action | Microsoft. Web/apimanagementaccounts/API/bağlantılar/listgizlilikler/Action | Gizli API yönetim hesapları API 'Leri bağlantılarını listeleyin. |
> | Action | Microsoft. Web/apimanagementaccounts/API 'ler/bağlantılar/okuma | API Management hesapları API 'Leri bağlantıları alın. |
> | Action | Microsoft. Web/apimanagementaccounts/API 'ler/bağlantılar/yazma | API Management hesapları API 'Leri bağlantılarını güncelleştirin. |
> | Action | Microsoft. Web/apimanagementaccounts/API/Delete | API Management hesapları API 'Lerini silin. |
> | Action | Microsoft. Web/apimanagementaccounts/API/localizeddefinitions/Delete | API yönetim hesapları API 'Leri yerelleştirilmiş tanımlarını silin. |
> | Action | Microsoft. Web/apimanagementaccounts/API/localizeddefinitions/oku | API yönetim hesapları API 'Leri yerelleştirilmiş tanımları alın. |
> | Action | Microsoft. Web/apimanagementaccounts/API/localizeddefinitions/yaz | API yönetim hesapları API 'Leri yerelleştirilmiş tanımları güncelleştirme. |
> | Action | Microsoft. Web/apimanagementaccounts/API/Read | API Management hesapları API 'Lerini alın. |
> | Action | Microsoft. Web/apimanagementaccounts/API/Write | API Management hesapları API 'Lerini güncelleştirin. |
> | Action | Microsoft. Web/apimanagementaccounts/connectionacl/okuma | API Management hesapları Connectionacl 'Leri alın. |
> | Action | Microsoft. Web/availableyığınları/okuma | Kullanılabilir yığınları alın. |
> | Action | Microsoft. Web/sertifikalar/Sil | Var olan bir sertifikayı silin. |
> | Action | Microsoft. Web/sertifikalar/okuma | Sertifikaların listesini alın. |
> | Action | Microsoft. Web/sertifikalar/yazma | Yeni bir sertifika ekleyin veya var olan bir sertifikayı güncelleştirin. |
> | Action | Microsoft. Web/checknameavaılabılıty/Read | Kaynak adının kullanılabilir olup olmadığını denetleyin. |
> | Action | Microsoft. Web/classımobileservices/Read | Klasik Mobile Services alın. |
> | Action | Microsoft. Web/Connectiongateway/Delete | Bir bağlantı ağ geçidini siler. |
> | Action | Microsoft. Web/Connectiongateway/JOIN/Action | Bir bağlantı ağ geçidini birleştirir. |
> | Action | Microsoft. Web/Connectiongateway/ListStatus/Action | Bir bağlantı ağ geçidinin durumunu listeler. |
> | Action | Microsoft. Web/Connectiongateway/Move/Action | Bir bağlantı ağ geçidini gider. |
> | Action | Microsoft. Web/Connectiongateway/Read | Bağlantı ağ geçitlerinin listesini alın. |
> | Action | Microsoft. Web/Connectiongateway/Write | Bir bağlantı ağ geçidi oluşturur veya güncelleştirir. |
> | Action | Microsoft. Web/Connections/confirmconsentcode/Action | Bağlantı onay kodunu onaylayın. |
> | Action | Microsoft. Web/bağlantılar/silme | Bir bağlantıyı siler. |
> | Action | Microsoft. Web/Connections/JOIN/Action | Bir bağlantıyı birleştirir. |
> | Action | Microsoft. Web/Connections/listconsentlinks/Action | Bağlantılar için Izin bağlantılarını listeleyin. |
> | Action | Microsoft. Web/bağlantılar/taşıma/eylem | Bir bağlantıyı gider. |
> | Action | Microsoft. Web/Connections/Read | Bağlantıların listesini alın. |
> | Action | Microsoft. Web/Connections/Write | Bir bağlantı oluşturur veya güncelleştirir. |
> | Action | Microsoft. Web/Customapsıs/Delete | Özel bir API 'YI siler. |
> | Action | Microsoft. Web/Customapsıs/extractApiDefinitionFromWsdl/Action | Bir WSDL 'den API tanımını ayıklar. |
> | Action | Microsoft. Web/Customapsıs/JOIN/Action | Özel bir API 'ye katılır. |
> | Action | Microsoft. Web/Customapsıs/Listwsdlınterfaces/eylem | Özel bir API için WSDL arabirimlerini listeler. |
> | Action | Microsoft. Web/Customapsıs/taşıma/eylem | Özel bir API 'YI taşıın. |
> | Action | Microsoft. Web/Customapsıs/okuma | Özel API 'nin listesini alın. |
> | Action | Microsoft. Web/Customapsıs/yazma | Özel bir API oluşturur veya güncelleştirir. |
> | Action | Microsoft. Web/deletedSites/Read | Silinen bir Web uygulamasının özelliklerini al |
> | Action | Microsoft. Web/deploymentlocations/okuma | Dağıtım konumlarını al. |
> | Action | Microsoft. Web/coğrafi bölgeler/okuma | Coğrafi bölgelerin listesini alın. |
> | Action | Microsoft. Web/hostingenvironments/kapasiteleri/okuma | Barındırma ortamları kapasitelerine ulaşın. |
> | Action | Microsoft. Web/hostingEnvironments/Delete | App Service Ortamı silme |
> | Action | Microsoft. Web/hostingenvironments/detektorlar/okuma | Barındırma ortamları algılayıcıları alın. |
> | Action | Microsoft. Web/hostingenvironments/Diagnostic/Read | Barındırma ortamları tanılamayı alın. |
> | Action | Microsoft. Web/hostingenvironments/ınboundnetworkbağımlıcıesendpoints/okuma | Tüm gelen bağımlılıkların ağ uç noktalarını alın. |
> | Action | Microsoft. Web/hostingEnvironments/JOIN/Action | Bir App Service Ortamı birleştirir |
> | Action | Microsoft. Web/hostingenvironments/MetricDefinitions/okuma | Barındırma ortamları ölçüm tanımlarını alın. |
> | Action | Microsoft. Web/hostingenvironments/multirolehavuzlar/MetricDefinitions/okuma | Barındırma ortamları çoklu rol havuzları ölçüm tanımlarını alın. |
> | Action | Microsoft. Web/hostingenvironments/multirolepools/ölçümler/okuma | Barındırma ortamları çoklu rol havuzları ölçümlerini alın. |
> | Action | Microsoft. Web/hostingEnvironments/multiRolePools/okuma | Bir App Service Ortamı ön uç havuzunun özelliklerini al |
> | Action | Microsoft. Web/hostingenvironments/multirolepools/SKU/okuma | Barındırma ortamları çoklu rol havuzları SKU 'Ları alın. |
> | Action | Microsoft. Web/hostingenvironments/multirolepools/kullanımlar/okuma | Barındırma ortamları çoklu rol havuzları kullanımları alın. |
> | Action | Microsoft. Web/hostingEnvironments/Multirolehavuzlar/Write | App Service Ortamı yeni bir ön uç havuzu oluşturma veya var olanı güncelleştirme |
> | Action | Microsoft. Web/hostingenvironments/işlemler/okuma | Barındırma ortamları Işlemlerini alın. |
> | Action | Microsoft. Web/hostingenvironments/outboundnetworkbağımlıcıesendpoints/okuma | Tüm giden bağımlılıkların ağ uç noktalarını alın. |
> | Action | Microsoft. Web/hostingEnvironments/PrivateEndpointConnectionsApproval/eylem | Özel uç nokta bağlantılarını Onayla |
> | Action | Microsoft. Web/hostingEnvironments/Read | App Service Ortamı özelliklerini al |
> | Action | Microsoft. Web/hostingEnvironments/reboot/eylem | App Service Ortamı tüm makineleri yeniden Başlat |
> | Action | Microsoft. Web/hostingenvironments/sürdürülecek/Action | Barındırma ortamlarını sürdürür. |
> | Action | Microsoft. Web/hostingenvironments/sunucugrupları/okuma | App Service planlarda barındırma ortamları alın. |
> | Action | Microsoft. Web/hostingenvironments/siteler/okuma | Barındırma ortamlarını Web Apps alın. |
> | Action | Microsoft. Web/hostingenvironments/beklet/eylem | Barındırma ortamlarını askıya alın. |
> | Action | Microsoft. Web/hostingenvironments/kullanımlar/okuma | Barındırma ortamları kullanımları alın. |
> | Action | Microsoft. Web/hostingenvironments/workerpools/MetricDefinitions/Read | Barındırma ortamları Workerpools ölçüm tanımlarını alın. |
> | Action | Microsoft. Web/hostingenvironments/workerpools/ölçümler/okuma | Barındırma ortamları için Workerpools ölçümlerini alın. |
> | Action | Microsoft. Web/hostingEnvironments/workerPools/Read | Bir App Service Ortamı çalışan havuzunun özelliklerini al |
> | Action | Microsoft. Web/hostingenvironments/workerpools/SKU/okuma | Barındırma ortamları Workerpools SKU 'Larını alın. |
> | Action | Microsoft. Web/hostingenvironments/workerpools/kullanımlar/Read | Çalışan ortamlarının kullanım havuzları kullanımlarını alın. |
> | Action | Microsoft. Web/hostingEnvironments/workerPools/Write | App Service Ortamı yeni bir çalışan havuzu oluşturma veya var olan bir çalışanı güncelleştirme |
> | Action | Microsoft. Web/hostingEnvironments/Write | Yeni bir App Service Ortamı oluşturma veya var olanı güncelleştirme |
> | Action | Microsoft. Web/ıhostingenvironmentnameavailable/Read | Barındırma ortamı adının kullanılabilir olup olmadığını öğrenin. |
> | Action | Microsoft. Web/ıhostnameavailable/Read | Ana bilgisayar adının kullanılabilir olup olmadığını denetleyin. |
> | Action | Microsoft. Web/ıusernameavailable/Read | Kullanıcı adının kullanılabilir olup olmadığını denetleyin. |
> | Action | Microsoft. Web/listSitesAssignedToHostName/Read | Ana bilgisayar adına atanan sitelerin adlarını alın. |
> | Action | Microsoft. Web/konumlar/apioperations/Read | Konumları al API Işlemleri. |
> | Action | Microsoft. Web/konumlar/connectiongatewaykurulumları/okuma | Konumları al bağlantı ağ geçidi yüklemeleri. |
> | Action | Microsoft. Web/konumlar/Deletevirtualnetworkoralt ağları/eylem | Konumlar için VNET veya alt ağ silme bildirimi. |
> | Action | Microsoft. Web/konumlar/extractapidefinitionfromwsdl/Action | Konumlar için WSDL 'den API tanımını ayıklayın. |
> | Action | Microsoft. Web/Locations/listwsdlınterfaces/Action | Konumlar için WSDL arabirimlerini listeleyin. |
> | Action | Microsoft. Web/konumlar/managedap/apioperations/Read | Konum yönetilen API Işlemlerini alın. |
> | Action | Microsoft. Web/Locations/managedap/JOIN/Action | Yönetilen bir API 'ye katılır. |
> | Action | Microsoft. Web/konumlar/managedap/okuma | Konumlara yönetilen API 'Ler alın. |
> | Action | Microsoft. Web/konumlar/operationResults/Read | Işlemleri al. |
> | Action | Microsoft. Web/konumlar/işlemler/okuma | Işlemleri al. |
> | Action | Microsoft. Web/işlemler/okuma | Işlemleri al. |
> | Action | Microsoft. Web/publishingusers/Read | Kullanıcıları yayımlamayı öğrenin. |
> | Action | Microsoft. Web/publishingusers/Write | Yayımlama kullanıcılarını güncelleştir. |
> | Action | Microsoft. Web/öneriler/okuma | Abonelikler için önerilerin listesini alın. |
> | Action | Microsoft. Web/Register/ACTION | Abonelik için Microsoft. Web kaynak sağlayıcısını kaydedin. |
> | Action | Microsoft. Web/resourcehealthmetadata/Read | Kaynak Durumu meta verilerini alın. |
> | Action | Microsoft. Web/sunucugrupları/özellikleri/okuma | App Service planları yeteneklerini alın. |
> | Action | Microsoft. Web/sunucugrupları/silme | Mevcut bir App Service planını Sil |
> | Action | Microsoft. Web/sunucugrupları/firstpartyapps/Settings/Delete | App Service planları birinci taraf uygulama ayarlarını silin. |
> | Action | Microsoft. Web/sunucugrupları/firstpartyapps/Settings/Read | App Service planlarını Ilk taraf uygulama ayarlarına alın. |
> | Action | Microsoft. Web/sunucugrupları/firstpartyapps/Settings/Write | Güncelleştirme App Service planları birinci taraf uygulama ayarları. |
> | Action | Microsoft. Web/sunucugrupları/hybridconnectionnamespaces/geçişler/Sil | Karma bağlantı ad alanları geçişlerinin App Service planlarını silin. |
> | Action | Microsoft. Web/sunucugrupları/hybridconnectionnamespaces/geçişler/okuma | App Service planları karma bağlantı ad alanları geçişleri alın. |
> | Action | Microsoft. Web/sunucugrupları/hybridconnectionnamespaces/geçişler/siteler/okuma | App Service planlarını karma bağlantı ad alanları geçişleri Web Apps alın. |
> | Action | Microsoft. Web/sunucugrupları/hybridconnectionplanlimit/Read | App Service planlarını karma bağlantı planı sınırlarına alın. |
> | Action | Microsoft. Web/sunucugrupları/hybridconnectiongeçişleri/okuma | App Service planları karma bağlantı geçişleri alın. |
> | Action | Microsoft. Web/sunucugrupları/MetricDefinitions/okuma | App Service planları ölçüm tanımlarını alın. |
> | Action | Microsoft. Web/sunucugrupları/ölçümleri/okuma | App Service plan ölçümleri alın. |
> | Action | Microsoft. Web/sunucugrupları/operationresults/Read | App Service planı Işlem sonuçlarını alın. |
> | Action | Microsoft. Web/sunucugrupları/okuma | App Service planında özellikleri al |
> | Action | Microsoft. Web/sunucugrupları/restartSites/Action | Bir App Service planındaki tüm Web Apps yeniden Başlat |
> | Action | Microsoft. Web/sunucugrupları/siteleri/okuma | App Service planlarını Web Apps alın. |
> | Action | Microsoft. Web/sunucugrupları/SKU 'lar/okuma | App Service planları SKU 'Larını alın. |
> | Action | Microsoft. Web/sunucugrupları/kullanımlar/okuma | App Service planları kullanımları alın. |
> | Action | Microsoft. Web/sunucugrupları/virtualnetworkconnections/Gateway/Write | Sanal ağ bağlantısı ağ geçitlerini güncelleştirme App Service planlar. |
> | Action | Microsoft. Web/sunucugrupları/virtualnetworkconnections/Read | Sanal ağ bağlantılarını App Service planları alın. |
> | Action | Microsoft. Web/sunucugrupları/virtualnetworkconnections/rotalar/Delete | App Service planları sanal ağ bağlantıları yollarını silin. |
> | Action | Microsoft. Web/sunucugrupları/virtualnetworkconnections/rotalar/Read | App Service planları sanal ağ bağlantıları yollarını alın. |
> | Action | Microsoft. Web/sunucugrupları/virtualnetworkconnections/rotalar/Write | Sanal ağ bağlantı yollarını güncelleştirme App Service planlar. |
> | Action | Microsoft. Web/sunucugrupları/çalışanlar/yeniden başlatma/eylem | App Service planları çalışanları 'nı yeniden başlatın. |
> | Action | Microsoft. Web/sunucugrupları/yazma | Yeni bir App Service planı oluşturma veya var olanı güncelleştirme |
> | Action | Microsoft. Web/Sites/çözümleyiciler ecustomhostname/Read | Özel ana bilgisayar adını çözümleyin. |
> | Action | Microsoft. Web/Sites/applySlotConfig/Action | Hedef yuvadan geçerli Web uygulamasına Web uygulaması yuva yapılandırması Uygula |
> | Action | Microsoft. Web/siteler/yedekleme/eylem | Yeni bir Web uygulaması yedeklemesi oluşturun |
> | Action | Microsoft. Web/siteler/yedekleme/okuma | Web Apps yedeklemesini alın. |
> | Action | Microsoft. Web/siteler/yedekleme/yazma | Web Apps Yedeklemeyi güncelleştirin. |
> | Action | Microsoft. Web/siteler/yedeklemeler/eylem | Azure Storage 'daki bir bloba geri yüklenebilir var olan bir uygulama yedeklemesini bulur. |
> | Action | Microsoft. Web/siteler/yedeklemeler/Sil | Web Apps yedeklemeleri silin. |
> | Action | Microsoft. Web/siteler/yedeklemeler/liste/eylem | Web Apps yedeklemeleri listeleyin. |
> | Action | Microsoft. Web/siteler/yedeklemeler/okuma | Bir Web uygulamasının yedeğinin özelliklerini al |
> | Action | Microsoft. Web/siteler/yedeklemeler/geri yükleme/eylem | Web Apps yedeklemeleri geri yükleyin. |
> | Action | Microsoft. Web/siteler/yedeklemeler/yazma | Web Apps yedeklemeleri güncelleştirin. |
> | Action | Microsoft. Web/siteler/yapılandırma/silme | Web Apps yapılandırmasını silin. |
> | Action | Microsoft. Web/siteler/yapılandırma/liste/eylem | Web uygulamasının, yayımlama kimlik bilgileri, uygulama ayarları ve bağlantı dizeleri gibi güvenlikle hassas ayarlarını listeleyin |
> | Action | Microsoft. Web/siteler/yapılandırma/okuma | Web uygulaması yapılandırma ayarlarını al |
> | Action | Microsoft. Web/siteler/yapılandırma/anlık görüntüler/okuma | Web Apps config anlık görüntülerini alın. |
> | Action | Microsoft. Web/siteler/yapılandırma/yazma | Web uygulamasının yapılandırma ayarlarını Güncelleştir |
> | Action | Microsoft. Web/Sites/containerlogs/Action | Web uygulaması için daraltılmış kapsayıcı günlüklerini al. |
> | Action | Microsoft. Web/Sites/containerlogs/Download/Action | Web Apps kapsayıcı günlüklerini indirin. |
> | Action | Microsoft. Web/Sites/continuouswebjobs/Delete | Web Apps sürekli Web Işlerini silin. |
> | Action | Microsoft. Web/Sites/continuouswebjobs/Read | Sürekli Web Işleri Web Apps alın. |
> | Action | Microsoft. Web/Sites/continuouswebjobs/start/Action | Sürekli Web Işleri Web Apps başlatın. |
> | Action | Microsoft. Web/Sites/continuouswebjobs/durdur/Action | Sürekli Web Işlerini Web Apps durdurun. |
> | Action | Microsoft. Web/Sites/Delete | Var olan bir Web uygulamasını silme |
> | Action | Microsoft. Web/siteler/dağıtımlar/Sil | Web Apps dağıtımlarını silin. |
> | Action | Microsoft. Web/siteler/dağıtımlar/günlük/okuma | Web Apps dağıtım günlüğünü alın. |
> | Action | Microsoft. Web/siteler/dağıtımlar/okuma | Web Apps dağıtımlarını alın. |
> | Action | Microsoft. Web/siteler/dağıtımlar/yazma | Web Apps dağıtımlarını güncelleştirin. |
> | Action | Microsoft. Web/Sites/detektorler/okuma | Web Apps detektorları alın. |
> | Action | Microsoft. Web/siteler/tanılama/çözümlemeler/yürütme/eylem | Web Apps tanılama analizini çalıştırın. |
> | Action | Microsoft. Web/siteler/Tanılamalar/çözümlemeler/okuma | Web Apps tanılama analizini alın. |
> | Action | Microsoft. Web/Sites/Diagnostic/aspnetcore/Read | ASP.NET Core uygulama için Web Apps tanılamayı alın. |
> | Action | Microsoft. Web/Sites/Diagnostics/oto Heal/Read | Web Apps tanılama oto Heal 'ı alın. |
> | Action | Microsoft. Web/siteler/tanılama/dağıtım/okuma | Web Apps tanılama dağıtımı alın. |
> | Action | Microsoft. Web/siteler/Tanılamalar/dağıtımlar/okuma | Web Apps tanılama dağıtımlarını alın. |
> | Action | Microsoft. Web/siteler/tanılama/algılayıcıları/yürütme/eylem | Web Apps tanılama algılayıcısı 'nı çalıştırın. |
> | Action | Microsoft. Web/siteler/Tanılamalar/algılayıcıları/okuma | Web Apps tanılama algılayıcısı alın. |
> | Action | Microsoft. Web/Sites/Diagnostic/failedrequestsperuri/Read | Uri başına başarısız Web Apps tanılama Isteği Isteklerini alın. |
> | Action | Microsoft. Web/Sites/Diagnostic/frebanalysis/Read | Web Apps tanılama FREB analizini alın. |
> | Action | Microsoft. Web/Sites/Diagnostic/loganalyzer/Read | Web Apps tanılama günlüğü Çözümleyicisi 'ni alın. |
> | Action | Microsoft. Web/Sites/Diagnostic/Read | Web Apps tanılama kategorilerini alın. |
> | Action | Microsoft. Web/Sites/Diagnostics/runtimeavaılabılıty/Read | Web Apps tanılama çalışma zamanı kullanılabilirliği alın. |
> | Action | Microsoft. Web/Sites/Diagnostic/servicehealth/Read | Web Apps tanılama hizmeti sistem durumunu alın. |
> | Action | Microsoft. Web/Sites/Diagnostic/sitecpuanalysis/Read | Web Apps tanılama sitesi CPU analizini alın. |
> | Action | Microsoft. Web/Sites/Diagnostics/sitecrash/Read | Web Apps tanılama sitesi kilitlenmelerini alın. |
> | Action | Microsoft. Web/siteler/tanılama/sitegecikmesi/okuma | Tanılama sitesi gecikmesini Web Apps alın. |
> | Action | Microsoft. Web/Sites/Diagnostic/sitememoryanalysis/Read | Web Apps tanılama sitesi bellek analizini alın. |
> | Action | Microsoft. Web/Sites/Diagnostics/sıterestartsettingupdate/Read | Web Apps tanılama sitesini yeniden başlatma ayarı güncelleştirmesi. |
> | Action | Microsoft. Web/Sites/Diagnostics/sıterestartuserbaşlatılmış/Read | Web Apps tanılama sitesini yeniden başlatma Kullanıcı tarafından başlatıldı. |
> | Action | Microsoft. Web/Sites/Diagnostics/siteswap/Read | Web Apps tanılama sitesini değiştirme 'yi alın. |
> | Action | Microsoft. Web/Sites/Diagnostic/ThreadCount/Read | Web Apps tanılama Iş parçacığı sayısı alın. |
> | Action | Microsoft. Web/Sites/Diagnostics/workeravaılabılıty/Read | Web Apps tanılama Workeravaılabılıty 'i alın. |
> | Action | Microsoft. Web/Sites/Diagnostic/workerprocessrecycle/Read | Web Apps tanılama çalışan Işlemi geri dönüşümü alın. |
> | Action | Microsoft. Web/Sites/domainownershiptanýmlayýcýlarý/Read | Web Apps etki alanı sahiplik tanımlayıcıları alın. |
> | Action | Microsoft. Web/Sites/domainownershiptanýmlayýcýlarý/Write | Web Apps etki alanı sahiplik tanımlayıcılarını güncelleştirin. |
> | Action | Microsoft. Web/siteler/uzantılar/silme | Web Apps site uzantılarını silin. |
> | Action | Microsoft. Web/Sites/Extensions/Read | Web Apps site uzantıları alın. |
> | Action | Microsoft. Web/Sites/Extensions/Write | Web Apps site uzantılarını güncelleştirin. |
> | Action | Microsoft. Web/siteler/işlevler/eylem | İşlevler Web Apps. |
> | Action | Microsoft. Web/siteler/işlevler/Sil | Web Apps Işlevlerini silin. |
> | Action | Microsoft. Web/Sites/Functions/listgizlilikler/Action | Gizli dizileri Web Apps listeleyin. |
> | Action | Microsoft. Web/siteler/işlevler/MasterKey/Read | Web Apps Işlevlerini MasterKey alın. |
> | Action | Microsoft. Web/siteler/işlevler/okuma | Web Apps Işlevleri alın. |
> | Action | Microsoft. Web/siteler/işlevler/belirteç/okuma | Web Apps Işlevleri belirteci alın. |
> | Action | Microsoft. Web/siteler/işlevler/yazma | Web Apps Işlevlerini güncelleştirin. |
> | Action | Microsoft. Web/Sites/hostnamebindings/Delete | Web Apps konak adı bağlamalarını silin. |
> | Action | Microsoft. Web/Sites/hostnamebindings/Read | Web Apps konak adı bağlamaları alın. |
> | Action | Microsoft. Web/Sites/hostnamebindings/Write | Web Apps konak adı bağlamalarını güncelleştirin. |
> | Action | Microsoft. Web/siteler/hostruntime/işlevler/anahtarlar/okuma | Web Apps Hostruntime Işlevleri anahtarlarını alın. |
> | Action | Microsoft. Web/Sites/hostruntime/Host/_master/Read | Yönetici işlemleri için İşlev Uygulaması ana anahtarını al |
> | Action | Microsoft. Web/Sites/hostruntime/Host/Action | Eşitleme Tetikleyicileri, işlevler eklemek, işlevleri çağırmak, işlevleri silmek gibi İşlev Uygulaması çalışma zamanı eylemi gerçekleştirin. |
> | Action | Microsoft. Web/Sites/hostruntime/Host/Read | Web Apps Hostruntime konağını alın. |
> | Action | Microsoft. Web/Sites/hybridconnection/Delete | Karma bağlantıyı Web Apps silin. |
> | Action | Microsoft. Web/Sites/hybridconnection/Read | Karma Bağlantı Web Apps alın. |
> | Action | Microsoft. Web/Sites/hybridconnection/Write | Karma bağlantıyı Web Apps güncelleştirin. |
> | Action | Microsoft. Web/Sites/hybridconnectionnamespaces/geçişler/Sil | Karma bağlantı ad alanları geçişleri Web Apps silin. |
> | Action | Microsoft. Web/Sites/hybridconnectionnamespaces/geçişler/ListKeys/Action | Karma bağlantı ad alanları geçişleri Web Apps anahtarları listeleyin. |
> | Action | Microsoft. Web/Sites/hybridconnectionnamespaces/geçişler/okundu | Karma bağlantı ad alanları geçişleri Web Apps alın. |
> | Action | Microsoft. Web/Sites/hybridconnectionnamespaces/geçişler/yaz | Karma bağlantı ad alanları geçişlerinin güncelleştirme Web Apps. |
> | Action | Microsoft. Web/Sites/hybridconnectiongeçişleri/okuma | Karma bağlantı geçişleri Web Apps alın. |
> | Action | Microsoft. Web/siteler/örnekler/dağıtımlar/Sil | Web Apps örnekleri dağıtımlarını silin. |
> | Action | Microsoft. Web/siteler/örnekler/dağıtımlar/okuma | Web Apps örnekleri dağıtımlarını alın. |
> | Action | Microsoft. Web/Sites/Instances/Extensions/log/Read | Web Apps örnekleri uzantıları günlüğünü alın. |
> | Action | Microsoft. Web/Sites/Instances/Extensions/süreçler/okuma | Web Apps örnekleri uzantı süreçlerini alın. |
> | Action | Microsoft. Web/Sites/Instances/Extensions/Read | Web Apps örnekleri uzantılarını alın. |
> | Action | Microsoft. Web/siteler/örnekler/süreçler/Sil | Web Apps örnekleri Işlemini silin. |
> | Action | Microsoft. Web/siteler/örnekler/süreçler/modüller/okuma | Web Apps örnekleri, modülleri Işler. |
> | Action | Microsoft. Web/siteler/örnekler/süreçler/okuma | Web Apps örnekleri Işlemini alın. |
> | Action | Microsoft. Web/siteler/örnekler/süreçler/iş parçacıkları/okuma | Web Apps örnekleri Iş parçacıklarını Işler. |
> | Action | Microsoft. Web/Sites/Instances/Read | Web Apps örnekleri alın. |
> | Action | Microsoft. Web/Sites/listsyncfunctiontriggerstatus/Action | Eşitleme Işlevi tetikleme durumu Web Apps listele. |
> | Action | Microsoft. Web/Sites/MetricDefinitions/Read | Web Apps ölçüm tanımlarını alın. |
> | Action | Microsoft. Web/siteler/ölçümler/okuma | Web Apps ölçümleri alın. |
> | Action | Microsoft. Web/Sites/metricsdefinitions/okuma | Web Apps ölçüm tanımlarını alın. |
> | Action | Microsoft. Web/Sites/migratemysql/Action | MySql Web Apps geçirin. |
> | Action | Microsoft. Web/Sites/migratemysql/Read | MySql Web Apps geçirin. |
> | Action | Microsoft. Web/Sites/networktrace/Action | Ağ Izleme Web Apps. |
> | Action | Microsoft. Web/Sites/networkizlemeler/operationresults/Read | Web Apps ağ Izleme Işlemi sonuçlarını alın. |
> | Action | Microsoft. Web/Sites/YeniParola/Action | YeniParola Web Apps. |
> | Action | Microsoft. Web/Sites/operationresults/Read | Web Apps Işlem sonuçlarını alın. |
> | Action | Microsoft. Web/Sites/işlemler/okuma | Web Apps Işlemleri alın. |
> | Action | Microsoft. Web/Sites/PerfCounters/okuma | Web Apps performans sayaçlarını alın. |
> | Action | Microsoft. Web/Sites/premieraddons/Delete | Web Apps Premier Eklentiler öğesini silin. |
> | Action | Microsoft. Web/Sites/premieraddons/Read | Web Apps Premier eklentilerden yararlanın. |
> | Action | Microsoft. Web/Sites/premieraddons/Write | Web Apps Premier eklentilerini güncelleştirin. |
> | Action | Microsoft. Web/Sites/privateaccess/Read | Siteye erişebilen özel site erişimi etkinleştirme ve yetkilendirilmiş sanal ağlarla ilgili veri alın. |
> | Action | Microsoft. Web/Sites/PrivateEndpointConnectionsApproval/eylem | Özel uç nokta bağlantılarını Onayla |
> | Action | Microsoft. Web/siteler/süreçler/modüller/okuma | Web Apps Işleme modüllerini alın. |
> | Action | Microsoft. Web/siteler/süreçler/okuma | Web Apps süreçlerini alın. |
> | Action | Microsoft. Web/siteler/süreçler/iş parçacıkları/okuma | Web Apps Işleme Iş parçacıklarını alın. |
> | Action | Microsoft. Web/Sites/publiccertificates/Delete | Web Apps ortak sertifikaları silme. |
> | Action | Microsoft. Web/Sites/publiccertificates/Read | Web Apps ortak sertifikaları alın. |
> | Action | Microsoft. Web/Sites/publiccertificates/Write | Ortak sertifikaları güncelleştirme Web Apps. |
> | Action | Microsoft. Web/siteler/Yayımla/eylem | Web uygulaması yayımlama |
> | Action | Microsoft. Web/Sites/publishxml/Action | Bir Web uygulaması için Yayımlama profili xml 'i al |
> | Action | Microsoft. Web/Sites/publishxml/Read | Yayımlama XML 'i Web Apps alın. |
> | Action | Microsoft. Web/Sites/Read | Bir Web uygulamasının özelliklerini al |
> | Action | Microsoft. Web/Sites/recommendationhistory/Read | Web Apps öneri geçmişini alın. |
> | Action | Microsoft. Web/siteler/öneriler/devre dışı bırakma/eylem | Web Apps önerilerini devre dışı bırakın. |
> | Action | Microsoft. Web/siteler/öneriler/okuma | Web uygulaması için önerilerin listesini alın. |
> | Action | Microsoft. Web/siteler/kurtar/eylem | Web Apps kurtarın. |
> | Action | Microsoft. Web/Sites/resetSlotConfig/Action | Web uygulaması yapılandırmasını sıfırlama |
> | Action | Microsoft. Web/Sites/resourcehealthmetadata/Read | Web Apps Kaynak Durumu meta verilerini alın. |
> | Action | Microsoft. Web/siteler/yeniden Başlat/eylem | Bir Web uygulamasını yeniden başlatma |
> | Action | Microsoft. Web/siteler/geri yükleme/okuma | Web Apps geri yüklemeyi alın. |
> | Action | Microsoft. Web/siteler/geri yükleme/yazma | Web Apps geri yükleyin. |
> | Action | Microsoft. Web/Sites/restorefrombackupblob/Action | Web uygulamasını yedekleme Blobundan geri yükleyin. |
> | Action | Microsoft. Web/Sites/restorefromdeletedapp/eylem | Web Apps silinen uygulamadan geri yükleyin. |
> | Action | Microsoft. Web/siteler/restoresnapshot/eylem | Web Apps anlık görüntülerini geri yükleyin. |
> | Action | Microsoft. Web/Sites/siteextensions/Sil | Web Apps site uzantılarını silin. |
> | Action | Microsoft. Web/Sites/siteextensions/okuma | Web Apps site uzantıları alın. |
> | Action | Microsoft. Web/Sites/siteextensions/Write | Web Apps site uzantılarını güncelleştirin. |
> | Action | Microsoft. Web/Sites/yuvaları/çözümleyiciler ecustomhostname/Read | Web Apps yuvaları özel ana bilgisayar adını analiz edin. |
> | Action | Microsoft. Web/Sites/yuvaları/applySlotConfig/Action | Hedef yuvadan geçerli yuvaya Web uygulaması yuva yapılandırması uygulayın. |
> | Action | Microsoft. Web/siteler/yuvalar/yedekleme/eylem | Yeni Web uygulaması yuvası yedeklemesi oluşturun. |
> | Action | Microsoft. Web/siteler/yuvalar/yedekleme/okuma | Web Apps yuvaları yedeklemesini alın. |
> | Action | Microsoft. Web/siteler/yuvalar/yedekleme/yazma | Web Apps yuvaları yedeklemesini güncelleştirin. |
> | Action | Microsoft. Web/siteler/yuvalar/yedeklemeler/eylem | Web Apps yuvaları yedeklemelerini bulun. |
> | Action | Microsoft. Web/siteler/yuvalar/yedeklemeler/silme | Web Apps yuvaları yedeklemelerini silin. |
> | Action | Microsoft. Web/siteler/yuvalar/yedeklemeler/liste/eylem | Web Apps yuvaları yedeklemelerini listeleyin. |
> | Action | Microsoft. Web/siteler/yuvalar/yedeklemeler/okuma | Web uygulaması yuvaları yedeklemesinin özelliklerini al |
> | Action | Microsoft. Web/siteler/yuvalar/yedeklemeler/geri yükleme/eylem | Web Apps yuvaları yedeklemelerini geri yükleyin. |
> | Action | Microsoft. Web/siteler/yuvalar/yapılandırma/silme | Web Apps yuvaları yapılandırmasını silin. |
> | Action | Microsoft. Web/siteler/yuvalar/yapılandırma/listeleme/eylem | Web uygulaması yuvasının kimlik bilgileri, uygulama ayarları ve bağlantı dizeleri gibi güvenlik duyarlı ayarlarını listeleyin |
> | Action | Microsoft. Web/siteler/yuvalar/yapılandırma/okuma | Web uygulaması yuvasının yapılandırma ayarlarını al |
> | Action | Microsoft. Web/siteler/yuvalar/yapılandırma/yazma | Web uygulaması yuvasının yapılandırma ayarlarını Güncelleştir |
> | Action | Microsoft. Web/Sites/yuvaları/containerlogs/Action | Web uygulaması yuvası için daraltılmış kapsayıcı günlüklerini al. |
> | Action | Microsoft. Web/Sites/yuvaları/containerlogs/Download/Action | Web Apps yuvaları kapsayıcı günlüklerini indirin. |
> | Action | Microsoft. Web/Sites/yuvaları/continuouswebjobs/Delete | Web Apps yuvaları sürekli Web Işlerini silin. |
> | Action | Microsoft. Web/Sites/yuvaları/continuouswebjobs/Read | Web Apps yuvaları sürekli Web Işleri alın. |
> | Action | Microsoft. Web/Sites/yuvaları/continuouswebjobs/start/Action | Web Apps yuvaları sürekli Web Işleri başlatın. |
> | Action | Microsoft. Web/siteler/yuvalar/continuouswebjobs/durdur/eylem | Web Apps yuvaları sürekli Web Işlerini durdurun. |
> | Action | Microsoft. Web/siteler/yuvalar/silme | Var olan bir Web uygulaması yuvasını Sil |
> | Action | Microsoft. Web/siteler/yuvalar/dağıtımlar/silme | Web Apps yuvaları dağıtımlarını silin. |
> | Action | Microsoft. Web/siteler/yuvalar/dağıtımlar/günlük/okuma | Web Apps yuvaları dağıtım günlüğünü alın. |
> | Action | Microsoft. Web/siteler/yuvalar/dağıtımlar/okuma | Web Apps yuvaları dağıtımlarını alın. |
> | Action | Microsoft. Web/siteler/yuvalar/dağıtımlar/yazma | Web Apps yuvaları dağıtımlarını güncelleştirin. |
> | Action | Microsoft. Web/siteler/yuvalar/detektorler/okuma | Web Apps yuvaları algılayıcıları alın. |
> | Action | Microsoft. Web/siteler/yuvalar/Tanılamalar/çözümlemeler/yürütme/eylem | Web Apps yuvaları tanılama analizini çalıştırın. |
> | Action | Microsoft. Web/siteler/yuvalar/Tanılamalar/çözümlemeler/okuma | Web Apps yuvaları tanılama analizini alın. |
> | Action | Microsoft. Web/Sites/yuvaları/tanılama/aspnetcore/Read | ASP.NET Core uygulama için Web Apps yuvaları tanılamayı alın. |
> | Action | Microsoft. Web/siteler/yuvalar/tanılama/oto Heal/okuma | Web Apps yuvaları tanılama, oto Heal al. |
> | Action | Microsoft. Web/siteler/yuvalar/tanılama/dağıtım/okuma | Web Apps yuvaları tanılama dağıtımı alın. |
> | Action | Microsoft. Web/siteler/yuvalar/Tanılamalar/dağıtımlar/okuma | Web Apps yuvaları tanılama dağıtımlarını alın. |
> | Action | Microsoft. Web/siteler/yuvalar/tanılama/detektorları/yürütme/eylem | Web Apps yuvası tanılama algılayıcısı 'nı çalıştırın. |
> | Action | Microsoft. Web/siteler/yuvalar/Tanılamalar/detektorler/okuma | Web Apps yuvaları tanılama algılayıcısı alın. |
> | Action | Microsoft. Web/siteler/yuvalar/tanılama/frebanalysis/okuma | Web Apps yuvaları tanılama FREB analizini alın. |
> | Action | Microsoft. Web/Sites/yuvaları/tanılama/loganalyzer/Read | Web Apps yuvaları tanılama günlüğü Çözümleyicisi 'ni alın. |
> | Action | Microsoft. Web/siteler/yuvalar/tanılama/okuma | Web Apps yuvaları tanılamayı alın. |
> | Action | Microsoft. Web/siteler/yuvalar/tanılama/runtimeavaılabılıty/Read | Web Apps yuvaları tanılama çalışma zamanı kullanılabilirliği alın. |
> | Action | Microsoft. Web/Sites/yuvaları/tanılama/servicehealth/Read | Web Apps yuvaları tanılama hizmeti durumu alın. |
> | Action | Microsoft. Web/Sites/yuvaları/tanılama/sitecpuanalysis/Read | Web Apps yuvaları tanılama sitesi CPU analizini alın. |
> | Action | Microsoft. Web/siteler/yuvalar/tanılama/sitekilitleniyor/okuma | Web Apps yuvaları tanılama site kilitlenmelerini alın. |
> | Action | Microsoft. Web/siteler/yuvalar/tanılama/sitegecikmesi/okuma | Web Apps yuvaları tanılama sitesi gecikmesi alın. |
> | Action | Microsoft. Web/siteler/yuvalar/tanılama/sitememoryanalysis/okuma | Web Apps yuvaları tanılama site belleği Analizi alın. |
> | Action | Microsoft. Web/Sites/yuvaları/Diagnostics/sıterestartsettingupdate/Read | Web Apps yuvaları tanılama sitesini yeniden Başlat ayarı güncelleştirmesi. |
> | Action | Microsoft. Web/Sites/yuvaları/Diagnostics/sıterestartuserbaşlatılmış/Read | Web Apps yuvaları tanılama sitesini yeniden başlatma Kullanıcı tarafından başlatıldı. |
> | Action | Microsoft. Web/Sites/yuvaları/Tanılamalar/siteswap/Read | Web Apps yuvaları tanılama sitesi değiştirme 'yi alın. |
> | Action | Microsoft. Web/Sites/yuvaları/tanılama/ThreadCount/Read | Web Apps yuvaları tanılama Iş parçacığı sayısı alın. |
> | Action | Microsoft. Web/Sites/yuvaları/tanılama/workeravaılabılıty/Read | Web Apps yuvaları tanılama Workeravaılabılıty 'i alın. |
> | Action | Microsoft. Web/Sites/yuvaları/tanılama/workerprocessrecycle/Read | Web Apps yuvaları tanılama çalışan Işlemi geri dönüşümü alın. |
> | Action | Microsoft. Web/Sites/yuvaları/domainownershiptanýmlayýcýlarý/Read | Web Apps yuvaları etki alanı sahiplik tanımlayıcıları alın. |
> | Action | Microsoft. Web/siteler/yuvalar/işlevler/listgizlilikler/Action | Gizli dizi Web Apps yuvaları Işlevlerini listeleyin. |
> | Action | Microsoft. Web/Sites/yuvaları/işlevleri/okuma | Web Apps yuvası Işlevleri alın. |
> | Action | Microsoft. Web/Sites/yuvaları/hostnamebindings/Delete | Web Apps yuvaları konak adı bağlamalarını silin. |
> | Action | Microsoft. Web/Sites/yuvaları/hostnamebindings/Read | Web Apps yuvası konak adı bağlamaları alın. |
> | Action | Microsoft. Web/Sites/yuvaları/hostnamebindings/Write | Web Apps yuvaları konak adı bağlamalarını güncelleştirin. |
> | Action | Microsoft. Web/Sites/yuvaları/hybridconnection/Delete | Web Apps yuvaların karma bağlantısını silin. |
> | Action | Microsoft. Web/Sites/yuvaları/hybridconnection/Read | Web Apps yuvası karma bağlantı alın. |
> | Action | Microsoft. Web/Sites/yuvaları/hybridconnection/Write | Web Apps yuvaların karma bağlantısını güncelleştirin. |
> | Action | Microsoft. Web/Sites/yuvaları/hybridconnectionnamespaces/geçişler/Sil | Web Apps yuvaları karma bağlantı ad alanları geçişleri silin. |
> | Action | Microsoft. Web/Sites/yuvaları/hybridconnectionnamespaces/geçişler/yaz | Web Apps yuvaları karma bağlantı ad alanları geçişleri güncelleştirme. |
> | Action | Microsoft. Web/Sites/yuvaları/hybridconnectiongeçişleri/okuma | Web Apps yuvaları karma bağlantı geçişleri alın. |
> | Action | Microsoft. Web/siteler/yuvalar/örnekler/dağıtımlar/okuma | Web Apps yuva örnekleri dağıtımlarını alın. |
> | Action | Microsoft. Web/siteler/yuvalar/örnek/işlem/silme | Web Apps yuvaları örnek süreçlerini silin. |
> | Action | Microsoft. Web/siteler/yuvalar/örnekler/süreçler/okuma | Web Apps yuvası örnek süreçlerini alın. |
> | Action | Microsoft. Web/siteler/yuvalar/örnekler/okuma | Web Apps yuvaları örnekleri alın. |
> | Action | Microsoft. Web/Sites/yuvaları/MetricDefinitions/Read | Web Apps yuvaları ölçüm tanımlarını alın. |
> | Action | Microsoft. Web/siteler/yuvalar/ölçümler/okuma | Web Apps yuvaları ölçümlerini alın. |
> | Action | Microsoft. Web/Sites/yuvaları/migratemysql/Read | Web Apps yuvaları MySql geçişini alın. |
> | Action | Microsoft. Web/Sites/yuvaları/networktrace/Action | Ağ Izleme Web Apps yuvaları. |
> | Action | Microsoft. Web/Sites/yuvaları/networkizlemeler/operationresults/Read | Web Apps Yuvaları ağ Izleme Işlemi sonuçları alın. |
> | Action | Microsoft. Web/siteler/yuvalar/YeniParola/eylem | YeniParola Web Apps yuvaları. |
> | Action | Microsoft. Web/Sites/yuvaları/operationresults/Read | Web Apps yuvaları Işlem sonuçlarını alın. |
> | Action | Microsoft. Web/siteler/yuvalar/işlemler/okuma | Web Apps yuvası Işlemleri alın. |
> | Action | Microsoft. Web/Sites/yuvaları/PerfCounters/okuma | Web Apps yuvaları performans sayaçlarını alın. |
> | Action | Microsoft. Web/siteler/yuvalar/phplogging/okuma | Web Apps yuvaları Phplogging alın. |
> | Action | Microsoft. Web/Sites/yuvaları/premieraddons/Delete | Web Apps yuvaları Premier eklentileri silin. |
> | Action | Microsoft. Web/Sites/yuvaları/premieraddons/Read | Web Apps yuvaları Premier eklentilerden yararlanın. |
> | Action | Microsoft. Web/Sites/yuvaları/premieraddons/Write | Web Apps yuvaları Premier Eklentiler ' i güncelleştirin. |
> | Action | Microsoft. Web/siteler/yuvalar/süreçler/okuma | Web Apps yuvası süreçlerini alın. |
> | Action | Microsoft. Web/Sites/yuvaları/publiccertificates/Delete | Web Apps yuvaları Genel sertifikalarını silin. |
> | Action | Microsoft. Web/Sites/yuvaları/publiccertificates/Read | Web Apps yuvaları Genel sertifikalarını alın. |
> | Action | Microsoft. Web/Sites/yuvaları/publiccertificates/Write | Web Apps yuvaları Genel sertifikalarını oluşturun veya güncelleştirin. |
> | Action | Microsoft. Web/siteler/yuvalar/yayımlama/eylem | Web uygulaması yuvası yayımlama |
> | Action | Microsoft. Web/Sites/yuvaları/publishxml/Action | Web uygulaması yuvası için Yayımlama profili xml 'i al |
> | Action | Microsoft. Web/siteler/yuvalar/okuma | Web uygulaması dağıtım yuvasının özelliklerini al |
> | Action | Microsoft. Web/siteler/yuvalar/kurtarma/eylem | Web Apps yuvaları kurtarın. |
> | Action | Microsoft. Web/Sites/yuvaları/resetSlotConfig/Action | Web uygulaması yuva yapılandırmasını Sıfırla |
> | Action | Microsoft. Web/Sites/yuvaları/resourcehealthmetadata/Read | Meta verileri Kaynak Durumu Web Apps yuvaları alın. |
> | Action | Microsoft. Web/siteler/yuvalar/yeniden başlatma/eylem | Web uygulaması yuvasını yeniden başlatma |
> | Action | Microsoft. Web/siteler/yuvalar/geri yükleme/okuma | Web Apps yuvaları geri yükleme alın. |
> | Action | Microsoft. Web/siteler/yuvalar/geri yükleme/yazma | Web Apps yuvalarını geri yükleyin. |
> | Action | Microsoft. Web/Sites/yuvaları/restorefrombackupblob/eylem | Web Apps yuvasını yedekleme Blobundan geri yükleyin. |
> | Action | Microsoft. Web/Sites/yuvaları/restorefromdeletedapp/eylem | Silinen uygulamadan Web uygulaması yuvalarını geri yükleyin. |
> | Action | Microsoft. Web/siteler/yuvalar/restoresnapshot/eylem | Web Apps yuvaları anlık görüntülerini geri yükleyin. |
> | Action | Microsoft. Web/Sites/yuvaları/siteextensions/Sil | Web Apps yuvaları site uzantılarını silin. |
> | Action | Microsoft. Web/Sites/yuvaları/siteextensions/okuma | Web Apps yuvaları site uzantıları alın. |
> | Action | Microsoft. Web/Sites/yuvaları/siteextensions/yaz | Web Apps yuvaları site uzantılarını güncelleştirin. |
> | Action | Microsoft. Web/siteler/yuvalar/slotsdiffs/eylem | Web uygulaması ve yuvaları arasındaki yapılandırmada farklılıklar alın |
> | Action | Microsoft. Web/Sites/yuvaları/slotsswap/eylem | Web uygulaması dağıtım yuvalarını değiştirme |
> | Action | Microsoft. Web/siteler/yuvalar/anlık görüntüler/okuma | Web Apps yuvaları anlık görüntülerini alın. |
> | Action | Microsoft. Web/siteler/yuvalar/sourcedenetimleri/silme | Web uygulaması yuvasının kaynak denetimi yapılandırma ayarlarını Sil |
> | Action | Microsoft. Web/siteler/yuvalar/sourcedenetimleri/okuma | Web uygulaması yuvasının kaynak denetimi yapılandırma ayarlarını al |
> | Action | Microsoft. Web/siteler/yuvalar/sourcedenetimleri/yazma | Web uygulaması yuvasının kaynak denetimi yapılandırma ayarlarını Güncelleştir |
> | Action | Microsoft. Web/siteler/yuvalar/Başlat/eylem | Web uygulaması yuvası başlatma |
> | Action | Microsoft. Web/siteler/yuvalar/durdur/eylem | Web uygulaması yuvasını durdur |
> | Action | Microsoft. Web/siteler/yuvalar/eşitleme/eylem | Web Apps yuvalarını eşitleyin. |
> | Action | Microsoft. Web/Sites/yuvaları/triggeredwebjobs/Delete | Web Işleri tetiklediği Web Apps yuvalarını silin. |
> | Action | Microsoft. Web/Sites/yuvaları/triggeredwebjobs/okuma | Web Işleri tetiklediği Web Apps yuvaları alma. |
> | Action | Microsoft. Web/Sites/yuvaları/triggeredwebjobs/Run/ACTION | Web Işleri tetiklediği Web Apps yuvaları çalıştırın. |
> | Action | Microsoft. Web/siteler/yuvalar/kullanımlar/okuma | Web Apps yuvası kullanımları alın. |
> | Action | Microsoft. Web/Sites/yuvaları/virtualnetworkconnections/Delete | Web Apps yuvaları sanal ağ bağlantılarını silin. |
> | Action | Microsoft. Web/Sites/yuvaları/virtualnetworkconnections/Gateway/Write | Web Apps yuvaları sanal ağ bağlantısı ağ geçitlerini güncelleştirin. |
> | Action | Microsoft. Web/Sites/yuvaları/virtualnetworkconnections/Read | Web Apps yuvaları sanal ağ bağlantıları alın. |
> | Action | Microsoft. Web/Sites/yuvaları/virtualnetworkconnections/Write | Web Apps yuvaları sanal ağ bağlantılarını güncelleştirin. |
> | Action | Microsoft. Web/siteler/yuvalar/Web işleri/okuma | Web Apps yuvaları Web Işleri alın. |
> | Action | Microsoft. Web/siteler/yuvalar/yazma | Yeni bir Web uygulaması yuvası oluşturun veya mevcut olanı güncelleştirin |
> | Action | Microsoft. Web/Sites/slotsdiffs/eylem | Web uygulaması ve yuvaları arasındaki yapılandırmada farklılıklar alın |
> | Action | Microsoft. Web/Sites/slotsswap/Action | Web uygulaması dağıtım yuvalarını değiştirme |
> | Action | Microsoft. Web/siteler/anlık görüntüler/okuma | Web Apps anlık görüntülerini alın. |
> | Action | Microsoft. Web/Sites/sourcedenetimleri/silme | Web uygulamasının kaynak denetimi yapılandırma ayarlarını Sil |
> | Action | Microsoft. Web/Sites/sourcedenetimleri/okuma | Web uygulamasının kaynak denetimi yapılandırma ayarlarını al |
> | Action | Microsoft. Web/Sites/sourcedenetimleri/yazma | Web uygulamasının kaynak denetimi yapılandırma ayarlarını Güncelleştir |
> | Action | Microsoft. Web/siteler/Başlat/eylem | Web uygulaması başlatma |
> | Action | Microsoft. Web/siteler/durdur/eylem | Bir Web uygulamasını durdur |
> | Action | Microsoft. Web/siteler/eşitleme/eylem | Eşitleme Web Apps. |
> | Action | Microsoft. Web/Sites/syncfunctiontrigger/Action | Web Apps için eşitleme Işlevi Tetikleyicileri. |
> | Action | Microsoft. Web/Sites/triggeredwebjobs/Delete | Web Apps tetiklenen Web Işlerini silin. |
> | Action | Microsoft. Web/Sites/triggeredwebjobs/geçmiş/okuma | Web Apps harekete geçirilmiş web Işleri geçmişi alın. |
> | Action | Microsoft. Web/Sites/triggeredwebjobs/okuma | Web Apps tetiklenen Web Işleri alın. |
> | Action | Microsoft. Web/Sites/triggeredwebjobs/Run/ACTION | Web Apps tetiklenen Web Işleri çalıştırın. |
> | Action | Microsoft. Web/Sites/kullanımlar/Read | Web Apps kullanımları alın. |
> | Action | Microsoft. Web/Sites/virtualnetworkconnections/Delete | Web Apps sanal ağ bağlantılarını silin. |
> | Action | Microsoft. Web/Sites/virtualnetworkconnections/Gateway/Read | Web Apps sanal ağ bağlantısı ağ geçitlerini alın. |
> | Action | Microsoft. Web/Sites/virtualnetworkconnections/Gateway/Write | Web Apps sanal ağ bağlantısı ağ geçitlerini güncelleştirin. |
> | Action | Microsoft. Web/Sites/virtualnetworkconnections/Read | Sanal ağ bağlantılarını Web Apps alın. |
> | Action | Microsoft. Web/Sites/virtualnetworkconnections/Write | Web Apps sanal ağ bağlantılarını güncelleştirin. |
> | Action | Microsoft. Web/siteler/Web işleri/okuma | Web Apps WebJobs alın. |
> | Action | Microsoft. Web/Sites/Write | Yeni bir Web uygulaması oluşturma veya var olanı güncelleştirme |
> | Action | Microsoft. Web/SKU/okuma | SKU 'Ları al. |
> | Action | Microsoft. Web/sourcecontrols/Read | Kaynak denetimlerini al. |
> | Action | Microsoft. Web/sourcecontrols/Write | Kaynak denetimlerini güncelleştir. |
> | Action | Microsoft. Web/Unregister/eylem | Abonelik için Microsoft. Web kaynak sağlayıcısı kaydını silin. |
> | Action | Microsoft. Web/Validate/ACTION | Doğrulamalısınız. |
> | Action | Microsoft. Web/verifyhostingenvironmentvnet/eylem | Barındırma ortamı VNET 'i doğrulayın. |

## <a name="microsoftworkloadmonitor"></a>Microsoft. WorkloadMonitor

> [!div class="mx-tdCol2BreakAll"]
> | Eylem türü | Çalışma | Açıklama |
> | --- | --- | --- |
> | Action | Microsoft. WorkloadMonitor/Components/Read | Kaynak için bileşenleri alır |
> | Action | Microsoft. WorkloadMonitor/componentsSummary/okuma | Bileşenlerin özetini alır |
> | Action | Microsoft. WorkloadMonitor/Monitorınstances/Read | Kaynak için izleyici örneklerini alır |
> | Action | Microsoft. WorkloadMonitor/Monitorınstancessummary/Read | İzleme örneklerinin özetini alır |
> | Action | Microsoft. WorkloadMonitor/izleyicileri/okundu | Kaynak için izleyicileri alır |
> | Action | Microsoft. WorkloadMonitor/izleyicileri/Write | Kaynak için izleyiciyi yapılandırın |
> | Action | Microsoft. WorkloadMonitor/notificationSettings/Read | Kaynak için bildirim ayarlarını alır |
> | Action | Microsoft. WorkloadMonitor/notificationSettings/Write | Kaynak için bildirim ayarlarını yapılandırın |
> | Action | Microsoft. WorkloadMonitor/işlemler/okuma | Desteklenen işlemleri alır |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için özel roller](custom-roles.md)
- [Azure kaynakları için yerleşik roller](built-in-roles.md)
