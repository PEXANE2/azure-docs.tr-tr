---
title: Azure Resource Manager şablonu örnekleri
description: Bazı yaygın App Service senaryolarından bazıları için Azure Resource Manager şablon örnekleri bulun. App Service dağıtımınızı veya yönetim görevlerinizi otomatikleştirmeyi öğrenin.
author: tfitzmac
tags: azure-service-management
ms.topic: sample
ms.date: 01/04/2019
ms.author: tomfitz
ms.custom: mvc, fasttrack-edit
ms.openlocfilehash: 4ac8e7d51ce9a3d5694f2853219bb04869a91734
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86517714"
---
# <a name="azure-resource-manager-templates-for-app-service"></a>App Service için Azure Resource Manager Şablonlar

Aşağıdaki tabloda, Azure App Service için Azure Resource Manager şablonlarının bağlantıları yer almaktadır. Uygulama şablonları oluştururken sık karşılaşılan hataların önlenmesiyle ilgili öneriler için bkz. [Azure Resource Manager şablonlarıyla uygulama dağıtma Kılavuzu](deploy-resource-manager-template.md).

App Services kaynakları için JSON sözdizimi ve özellikleri hakkında bilgi edinmek için bkz. [Microsoft. Web kaynak türleri](/azure/templates/microsoft.web/allversions).

| Uygulama dağıtma | Description |
|-|-|
| [App Service planı ve temel Linux uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-basic-linux) | Linux için yapılandırılmış bir App Service uygulaması dağıtır. |
| [App Service planı ve temel Windows uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-basic-windows) | Windows için yapılandırılmış bir App Service uygulamasını dağıtır. |
| [GitHub deposuna bağlı uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-github-deploy)| GitHub 'dan kod çeken bir App Service uygulaması dağıtır. |
| [Özel dağıtım yuvaları olan uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-custom-deployment-slots)| Özel dağıtım yuvaları/ortamları ile bir App Service uygulaması dağıtır. |
| [Özel uç nokta ile uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-private-endpoint-webapp)| Özel bir uç nokta ile App Service uygulaması dağıtır. |
|**Uygulama yapılandırma**| **Açıklama** |
| [Key Vault 'den uygulama sertifikası](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-certificate-from-key-vault)| Bir Azure Key Vault gizli dizi App Service uygulama sertifikası dağıtır ve bunu TLS/SSL bağlaması için kullanır. |
| [Özel etki alanı ve SSL içeren uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-custom-domain-and-ssl)| , Özel ana bilgisayar adına sahip bir App Service uygulaması dağıtır ve TLS/SSL bağlama için Key Vault bir uygulama sertifikası alır. |
| [GoLang uzantılı uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-with-golang)| Golang site uzantısıyla bir App Service uygulaması dağıtır. Daha sonra Azure’da Golang üzerinde geliştirilmiş web uygulamalarını çalıştırabilirsiniz. |
| [Java 8 ve Tomcat 8 ile uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-java-tomcat)| Java 8 ve Tomcat 8 etkin bir App Service uygulaması dağıtır. Daha sonra Azure'da Java uygulamaları çalıştırabilirsiniz. |
| [Bölgesel VNet tümleştirmesi ile uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-app-service-regional-vnet-integration)| Bölgesel VNet tümleştirmesi etkinleştirilmiş bir App Service uygulaması dağıtır. |
|**Uygulama koruma**| **Açıklama** |
| [Azure Application Gateway ile tümleştirilmiş uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-app-gateway-v2)| Bir App Service uygulaması ve bir Application Gateway dağıtır ve hizmet uç noktası ile erişim kısıtlamalarını kullanarak trafiği yalıtır. |
|**Bağlı kaynaklarla Linux uygulaması**| **Açıklama** |
| [MySQL ile Linux üzerinde uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-mysql) | Linux üzerinde MySQL için Azure veritabanı ile bir App Service uygulaması dağıtır. |
| [Linux üzerinde PostgreSQL ile uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-linux-managed-postgresql) | Linux üzerinde PostgreSQL için Azure veritabanı ile bir App Service uygulaması dağıtır. |
|**Bağlı kaynaklarla uygulama**| **Açıklama** |
| [MySQL ile uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-mysql)| Windows üzerinde MySQL için Azure veritabanı ile bir App Service uygulaması dağıtır. |
| [PostgreSQL ile uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webapp-managed-postgresql)| Windows üzerinde PostgreSQL için Azure veritabanı ile bir App Service uygulaması dağıtır. |
| [Azure SQL veritabanı 'nda veritabanı içeren uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)| Temel hizmet düzeyinde Azure SQL veritabanı 'nda bir App Service uygulaması ve veritabanı dağıtır. |
| [BLOB depolama bağlantısı olan uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-blob-connection)| Azure Blob depolama bağlantı dizesiyle bir App Service uygulaması dağıtır. Bundan sonra, uygulamadan BLOB depolama alanını kullanabilirsiniz. |
| [Redsıs için Azure önbelleği olan uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-with-redis-cache)| Redsıs için Azure önbelleği ile bir App Service uygulaması dağıtır. |
|**App Service Ortamı**| **Açıklama** |
| [App Service ortamı v2 oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-create) | Sanal ağınızda bir App Service ortamı v2 oluşturur. |
| [ILB adresli bir App Service ortamı v2 oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-asev2-ilb-create/) | Sanal ağınızda özel bir iç yük dengeleyici adresine sahip App Service ortamı v2 oluşturur. |
| [Bir ILB App Service ortamı veya ILB App Service ortamı v2 için varsayılan SSL sertifikasını yapılandırma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-ase-ilb-configure-default-ssl) | ILB App Service ortamı veya ıLB App Service ortamı v2 için varsayılan TLS/SSL sertifikasını yapılandırır. |
| | |
