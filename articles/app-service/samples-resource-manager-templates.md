---
title: Azure Kaynak Yöneticisi şablon örnekleri
description: Ortak Uygulama Hizmeti senaryolarından bazıları için Azure Kaynak Yöneticisi şablon örneklerini bulun. Uygulama Hizmeti dağıtımını veya yönetim görevlerinizi nasıl otomatikleştirebilirsiniz öğrenin.
author: tfitzmac
tags: azure-service-management
ms.topic: sample
ms.date: 01/04/2019
ms.author: tomfitz
ms.custom: mvc, fasttrack-edit
ms.openlocfilehash: 2d77dcbd07a67e3ba50d70770515416b45fbc7a3
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637918"
---
# <a name="azure-resource-manager-templates-for-app-service"></a>Uygulama Hizmeti için Azure Kaynak Yöneticisi şablonları

Aşağıdaki tabloda Azure Uygulama Hizmeti için Azure Kaynak Yöneticisi şablonlarına bağlantılar yer almaktadır. Uygulama şablonları oluştururken sık karşılaşılan hataları önleme yle ilgili öneriler için Azure [Kaynak Yöneticisi şablonlarıyla uygulamaları dağıtma kılavuzu'na](deploy-resource-manager-template.md)bakın.

Uygulama Hizmetleri kaynaklarının JSON sözdizimi ve özellikleri hakkında bilgi edinmek için [Microsoft.Web kaynak türlerine](/azure/templates/microsoft.web/allversions)bakın.

| | |
|-|-|
|**Uygulama dağıtma**||
| [Uygulama Hizmeti planı ve temel Linux uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-basic-linux) | Linux için yapılandırılan bir Uygulama Hizmeti uygulaması dağıtır. |
| [Uygulama Hizmeti planı ve temel Windows uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-basic-windows) | Windows için yapılandırılan bir Uygulama Hizmeti uygulaması dağıtır. |
| [GitHub deposuna bağlı uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-github-deploy)| GitHub'dan kod çeken bir Uygulama Hizmeti uygulaması dağıtıyor. |
| [Özel dağıtım yuvaları ile uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-custom-deployment-slots)| Özel dağıtım yuvaları/ortamları olan bir Uygulama Hizmeti uygulaması dağıtıyor. |
|**Uygulamayı yapılandırma**||
| [Key Vault'tan uygulama sertifikası](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault)| Bir Azure Key Vault sırrından bir Uygulama Hizmeti uygulama sertifikası dağıtıyor ve TLS/SSL bağlama için kullanır. |
| [Özel etki alanına sahip uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain)| Özel ana bilgisayar adı olan bir Uygulama Hizmeti uygulaması dağıtıyor. |
| [Özel etki alanı ve SSL ile uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain-and-ssl)| Özel ana bilgisayar adı olan bir Uygulama Hizmeti uygulaması dağıtlar ve TLS/SSL bağlama için Key Vault'tan bir uygulama sertifikası alır. |
| [GoLang uzantılı uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-with-golang)| Golang site uzantısı ile bir Uygulama Hizmeti uygulaması dağıtıyor. Daha sonra Azure’da Golang üzerinde geliştirilmiş web uygulamalarını çalıştırabilirsiniz. |
| [Java 8 ve Tomcat 8 ile uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-java-tomcat)| Java 8 ve Tomcat 8 etkin leştirilmiş bir Uygulama Hizmeti uygulaması dağıtıyor. Daha sonra Azure'da Java uygulamaları çalıştırabilirsiniz. |
| [Bölgesel VNet entegrasyonu ile uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-app-service-regional-vnet-integration)| Bölgesel VNet tümleştirmesi etkinleştirilmiş bir Uygulama Hizmeti uygulaması dağıtıyor. |
|**Uygulamayı koruma**||
| [Azure Uygulama Ağ Geçidi ile entegre uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-app-gateway-v2)| Bir Uygulama Hizmeti uygulaması ve Uygulama Ağ Geçidi dağır ve hizmet bitiş noktası ve erişim kısıtlamalarını kullanarak trafiği yalıtar. |
|**Bağlı kaynaklara sahip Linux uygulaması**||
| [MySQL ile Linux'ta Uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-mysql) | MySQL için Azure Veritabanı ile Linux'ta bir Uygulama Hizmeti uygulaması dağıtıyor. |
| [PostgreSQL ile Linux'ta Uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-postgresql) | PostgreSQL için Azure Veritabanı ile Linux'ta bir Uygulama Hizmeti uygulaması dağıtıyor. |
|**Bağlı kaynaklarla uygulama**||
| [MySQL ile Uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-mysql)| MySQL için Azure Veritabanı ile Windows'da bir Uygulama Hizmeti uygulaması dağıtır. |
| [PostgreSQL ile Uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-postgresql)| PostgreSQL için Azure Veritabanı ile Windows'da bir Uygulama Hizmeti uygulaması dağıtır. |
| [SQL veritabanı na sahip uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)| Temel hizmet düzeyinde bir Uygulama Hizmeti uygulaması ve bir SQL veritabanı dağıtLar. |
| [Blob depolama bağlantısı olan uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-blob-connection)| Azure Blob depolama bağlantı dizesiyle bir Uygulama Hizmeti uygulaması dağıtıyor. Daha sonra uygulamadan Blob depolama kullanabilirsiniz. |
| [Redis için Azure Önbelleği olan uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-redis-cache)| Redis için Azure Önbelleği ile bir Uygulama Hizmeti uygulaması dağıtın. |
|**App Service Ortamı**||
| [App Service ortamı v2 oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-create) | Sanal ağınızda bir App Service ortamı v2 oluşturur. |
| [ILB adresli bir App Service ortamı v2 oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-ilb-create/) | Sanal ağınızda özel bir iç yük dengeleyici adresine sahip App Service ortamı v2 oluşturur. |
| [Bir ILB App Service ortamı veya ILB App Service ortamı v2 için varsayılan SSL sertifikasını yapılandırma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-ase-ilb-configure-default-ssl) | Varsayılan TLS/SSL sertifikasını Bir ILB App Service ortamı veya ILB App Service ortamı v2 için yapılandırır. |
| | |
