---
title: Azure için ansi modül ve sürüm matrisi | Microsoft Dokümanlar
description: Azure için saklı modül ve sürüm matrisi
keywords: ansible, roller, matris, sürüm, azure, devops
ms.topic: reference
ms.date: 10/14/2019
ms.openlocfilehash: 54e27c7570ba1cdbce7355740181d68a2f3efbac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74155990"
---
# <a name="ansible-module-and-version-matrix"></a>Ansible modülü ve sürüm matrisi

Ansible, Azure kaynaklarının sağlanması nda ve yapılandırılmasında kullanılmak üzere bir modül paketi içerir. Bu kaynaklar sanal makineleri, ölçek kümelerini, ağ hizmetlerini ve kapsayıcı hizmetlerini içerir. Bu makalede, Azure için çeşitli Ansible modülleri ve içinde gönderiyaptıkları Ansible sürümleri listeleilmektedir.

## <a name="ansible-modules-for-azure"></a>Azure için ansi modüller

Aşağıdaki modüller doğrudan uzak ana bilgisayarlarda veya oyun kitapları aracılığıyla yürütülebilir.  

Bu modüller Ansible resmi sürümünden ve aşağıdaki Microsoft oyun kitabı rollerinden edinilebilir.

> [!NOTE]
> Ansible 2.9'dan itibaren, tüm *_facts modüllerinin adını Ansible adlandırma kuralına uymak için *_info olarak yeniden adlandırdık. Eski ve yeniden adlandırılmış modüller, bir amortisman uyarısı görmenin dışında, tüm modüller eskisi gibi çalışır.

| Azure için ansi modül                   |  Ansible 2.4 |  Ansible 2.5 |  Ansible 2.6 | Ansible 2.7 | Ansible 2.8 | Ansible 2.9 | Ansible Rolü | 
|---------------------------------------------|--------------|--------------|-----------------------------|-------------------------------------|--------------|--------------|--------------|  
| **İşlem**                    |           |                          |                          |                            |           |           |           |
| azure_rm_availabilityset                   | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_availabilityset_info              | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_batchaccount                       | -            | -                           | -            | -            | -            | Evet          | Evet          |
| azure_rm_deployment                         | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_deployment_info                   | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_functionapp                        | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_functionapp_info                  | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_gallery                            | -            | -                           | -            | -            | -            | Evet          | Evet          |
| azure_rm_gallery_info                       | -            | -                           | -            | -            | -            | Evet          | Evet          |
| azure_rm_galleryimage                       | -            | -                           | -            | -            | -            | Evet          | Evet          |
| azure_rm_galleryimage_info                  | -            | -                           | -            | -            | -            | Evet          | Evet          |
| azure_rm_galleryimageversion                | -            | -                           | -            | -            | -            | Evet          | Evet          |
| azure_rm_galleryimageversion_info           | -            | -                           | -            | -            | -            | Evet          | Evet          |
| azure_rm_image                              | -            | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_image_info                        | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_resource                           | -            | -                           | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_resource_info                     | -            | -                           | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_resourcegroup                      | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_resourcegroup_info                | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_snapshot                           | -            | -                           | -            | -            | -            | Evet          | Evet          |
| azure_rm_virtualmachine                     | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualmachine_info               | -            | -                           | -            | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualmachineextension            | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualmachineextension_info      | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_virtualmachineimage_info          | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualmachinescaleset             | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualmachinescaleset_info       | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualmachinescalesetextension    | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_virtualmachinescalesetextension_info | -            | -                        | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_virtualmachinescalesetinstance     | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_virtualmachinescalesetinstance_info | -            | -                         | -            | -            | Evet          | Evet          | Evet          |
| **Ağ Oluşturma**                              |              |                             |              |              |              |              |              |
| azure_rm_appgateway                         | -            | -                           | -            | Evet          | Evet          | Evet          | Evet          |
| azure_rm_applicationsecuritygroup           | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_applicationsecuritygroup_info     | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_cdnendpoint                        | -            | -                         | -          | -            | Evet          | Evet          | Evet          |
| azure_rm_cdnendpoint_info                  | -            | -                         | -          | -            | Evet          | Evet          | Evet          |
| azure_rm_cdnprofile                         | -            | -                         | -          | -            | Evet          | Evet          | Evet          |
| azure_rm_cdnprofile_info                   | -            | -                         | -          | -            | Evet          | Evet          | Evet          |
| azure_rm_dnsrecordset                       | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_dnsrecordset_info                 | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_dnszone                            | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_dnszone_info                      | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_firewall                           | -            | -                           | -            | -            | -            | Evet          | Evet          |
| azure_rm_firewall_info                      | -            | -                           | -            | -            | -            | Evet          | Evet          |
| azure_rm_loadbalancer                       | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_loadbalancer_info                 | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_networkinterface                   | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_networkinterface_info             | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_publicipaddress                    | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_publicipaddress_info              | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_route                              | -            | -                           | -            | Evet          | Evet          | Evet          | Evet          |
| azure_rm_routetable                         | -            | -                           | -            | Evet          | Evet          | Evet          | Evet          |
| azure_rm_routetable_info                   | -            | -                           | -            | Evet          | Evet          | Evet          | Evet          |
| azure_rm_securitygroup                      | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_securitygroup_info                 | -            | -                           | -            | -            | -            | Evet          | Evet          |
| azure_rm_subnet                             | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_subnet_info                       | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_trafficmanagerendpoint             | -            | -                         | -          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_trafficmanagerendpoint_info       | -            | -                         | -          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_trafficmanagerprofile              | -            | -                         | -          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_trafficmanagerprofile_info        | -            | -                         | -          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualnetwork                     | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualnetwork_info               | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_virtualnetworkgateway              | -            | -                         | -          | -            | Evet          | Evet          | Evet          |
| azure_rm_virtualnetworkpeering              | -            | -                         | -          | -            | Evet          | Evet          | Evet          |
| azure_rm_virtualnetworkpeering_info         | -            | -                         | -          | -            | -            | Evet          | Evet          |
| **Depolama**                    |           |                          |                          |                            |           |           |         |
| azure_rm_manageddisk                        | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_manageddisk_info                  | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_storageaccount                     | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_storageaccount_info               | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_storageblob                        | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| **Web**                    |           |                          |                          |                             |           |           |           |
| azure_rm_appserviceplan                     | -            | -                         | -          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_appserviceplan_info               | -            | -                         | -          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_webapp                             | -            | -                         | -          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_webapp_info                       | -            | -                         | -          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_webappslot                         | -            | -                         | -          | -            | Evet          | Evet          | Evet          |
| **Kapsayıcılar**                    |           |                          |                          |                            |           |           |          |
| azure_rm_acs                                | Evet          | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_aks                                | -            | -                           | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_aks_info                          | -            | -                           | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_aksversion_info                   | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_containerinstance                  | -            | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_containerinstance_info            | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_containerregistry                  | -            | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_containerregistry_info            | -            | -                           | -            | Evet          | Evet          | Evet          | Evet          |
| azure_rm_containerregistryreplication       | -            | -                           | -            | -            | -          | -          | Evet          |
| azure_rm_containerregistryreplication_info  | -            | -                           | -            | -            | -          | -          | Evet          |
| azure_rm_containerregistrywebhook           | -            | -                           | -            | -            | -          | -          | Evet          |
| azure_rm_containerregistrywebhook_info      | -            | -                           | -            | -            | -          | -          | Evet          |
| **Veritaban -ları**                    |           |                          |                          |                             |           |           |          |
| azure_rm_cosmosdbaccount                    | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_cosmosdbaccount_info              | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_mariadbconfiguration               | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_mariadbconfiguration_info         | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_mariadbdatabase                    | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_mariadbdatabase_info              | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_mariadbfirewallrule                | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_mariadbfirewallrule_info          | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_mariadbserver                      | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_mariadbserver_info                | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_mysqlconfiguration                 | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_mysqlconfiguration_info           | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_mysqldatabase                      | -            | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_mysqldatabase_info                | -            | -                           | -            | Evet          | Evet          | Evet          | Evet          |
| azure_rm_mysqlfirewallrule                  | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_mysqlfirewallrule_info            | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_mysqlserver                        | -            | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_mysqlserver_info                  | -            | -                           | -            | Evet          | Evet          | Evet          | Evet          |
| azure_rm_postgresqlconfiguration            | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_postgresqlconfiguration_info      | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_postgresqldatabase                 | -            | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_postgresqldatabase_info           | -            | -                           | -            | Evet          | Evet          | Evet          | Evet          |
| azure_rm_postgresqlfirewallrule             | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_postgresqlfirewallrule_info       | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_postgresqlserver                   | -            | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_postgresqlserver_info             | -            | -                           | -            | Evet          | Evet          | Evet          | Evet          |
| azure_rm_rediscache                         | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_rediscache_info                   | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_rediscachefirewallrule             | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_sqldatabase                        | -            | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_sqldatabase_info                  | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_sqlelasticpool                    | -            | -                           | -            | -            | -          | -          | Evet          |
| azure_rm_sqlelasticpool_info               | -            | -                           | -            | -            | -          | -          | Evet          |
| azure_rm_sqlfirewallrule                    | -            | -                           | -            | Evet          | Evet          | Evet          | Evet          |
| azure_rm_sqlfirewallrule_info              | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_sqlserver                          | -            | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_sqlserver_info                    | -            | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| **Analiz**                    |           |                          |                          |                             |           |           |          |
| azure_rm_hdinsightcluster                   | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_hdinsightcluster_info              | -            | -                           | -            | -            | -            | Evet          | Evet          |
| **Tümleştirme**                    |           |                          |                          |                             |           |           |          |
| azure_rm_servicebus                         | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_servicebus_info                   | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_servicebusqueue                    | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_servicebussaspolicy                | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_servicebustopic                    | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_servicebustopicsubscription        | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| **Güvenlik**                    |           |                          |                          |                             |           |           |           |
| azure_rm_keyvault                           | -            | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_keyvault_info                     | -            | -                           | -              | -          | Evet          | Evet          | Evet          |
| azure_rm_keyvaultkey                        | -            | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_keyvaultkey_info                   | -            | -                           | -            | -            | -            | Evet          | Evet          |
| azure_rm_keyvaultsecret                     | -            | Evet                         | Evet          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_roleassignment                     | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_roleassignment_info               | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_roledefinition                     | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_roledefinition_info               | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| **DevOps**               |           |                          |                          |                             |           |           |                  |
| azure_rm_devtestlab                         | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_devtestlab_info                   | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_devtestlabarmtemplate_info        | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_devtestlabartifact_info           | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_devtestlabartifactsource           | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_devtestlabartifactsource_info     | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_devtestlabcustomimage              | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_devtestlabcustomimage_info         | -            | -                           | -            | -            | -            | Evet          | Evet          |
| azure_rm_devtestlabenvironment              | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_devtestlabenvironment_info         | -            | -                           | -            | -            | -            | Evet          | Evet          |
| azure_rm_devtestlabpolicy                   | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_devtestlabpolicy_info              | -            | -                           | -            | -            | -            | Evet          | Evet          |
| azure_rm_devtestlabschedule                 | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_devtestlabschedule_info            | -            | -                           | -            | -            | -            | Evet          | Evet          |
| azure_rm_devtestlabvirtualmachine           | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_devtestlabvirtualmachine_info | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_devtestlabvirtualnetwork           | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_devtestlabvirtualnetwork_info     | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| **Azure İzleyici**                           |              |                           |            |              |                 |           |              |
| azure_rm_autoscale                          | -            | -                         | -          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_autoscale_info                    | -            | -                         | -          | Evet          | Evet          | Evet          | Evet          |
| azure_rm_loganalyticsworkspace              | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_loganalyticsworkspace_info        | -            | -                           | -            | -            | Evet          | Evet          | Evet          |
| azure_rm_monitorlogprofile                  | -            | -                           | -            | -            | -            | Evet          | Evet          |
| **Yönetim ve İdare**     |              |                           |            |            |            |            |              |
| azure_rm_automationaccount        | -            | -                         | -          | -          | -          | Evet        | Evet          |
| azure_rm_automationaccount_info   | -            | -                         | -          | -          | -          | Evet        | Evet          |
| azure_rm_lock                     | -            | -                         | -          | -          | -          | Evet        | Evet          |
| azure_rm_lock_info                | -            | -                         | -          | -          | -          | Evet        | Evet          |
| **Nesnelerin İnterneti**            |              |                           |            |            |            |            |              |
| azure_rm_iotdevice                | -            | -                         | -          | -          | -          | Evet        | Evet          |
| azure_rm_iotdevice_info           | -            | -                         | -          | -          | -          | Evet        | Evet          |
| azure_rm_iotdevicemodule          | -            | -                         | -          | -          | -          | Evet        | Evet          |
| azure_rm_iothub_info              | -            | -                         | -          | -          | -          | Evet        | Evet          |
| azure_rm_iothub_info              | -            | -                         | -          | -          | -          | Evet        | Evet          |
| azure_rm_iothubconsumergroup      | -            | -                         | -          | -          | -          | Evet        | Evet          |

## <a name="introduction-to-playbook-role-for-azure"></a>Azure için oyun kitabı rolüne giriş

[azure_preview_module oyun kitabı rolü,](https://galaxy.ansible.com/Azure/azure_preview_modules/) en son Azure modüllerini içerir. Güncelleştirmeler ve hata düzeltmeleri resmi Ansible sürümüdaha zamanında yapılır. Ansible'ı Azure kaynak sağlama amacıyla kullanıyorsanız, oyun kitabı `azure_preview_module` rolünü yüklemeniz için teşvik edilirsiniz.

Oyun `azure_preview_module` kitabı rolü her üç haftada bir yayınlanır.

## <a name="next-steps"></a>Sonraki adımlar

Oyun kitabı rolleri hakkında daha fazla bilgi için [bkz.](https://docs.ansible.com/ansible/latest/playbooks_reuse.html) 