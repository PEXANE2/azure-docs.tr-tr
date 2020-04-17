---
title: CLI Örnekleri
description: Ortak Uygulama Hizmeti senaryolarından bazıları için Azure CLI örneklerini bulun. Uygulama Hizmeti dağıtımını veya yönetim görevlerinizi nasıl otomatikleştirebilirsiniz öğrenin.
tags: azure-service-management
ms.assetid: 53e6a15a-370a-48df-8618-c6737e26acec
ms.topic: sample
ms.date: 12/12/2017
ms.custom: mvc
ms.openlocfilehash: 499bf25f06030ae9fc5c1f8381ef4cd6ff6873dc
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535630"
---
# <a name="cli-samples-for-azure-app-service"></a>Azure Uygulama Hizmeti için CLI örnekleri

Aşağıdaki tablo, Azure CLI’si kullanılarak oluşturulan bash komut dosyalarına yönelik bağlantılar içerir.

| | |
|-|-|
|**Uygulama oluşturma**||
| [FTP ile bir uygulama oluşturun ve dosyaları dağıtın](./scripts/cli-deploy-ftp.md?toc=%2fcli%2fazure%2ftoc.json)| Bir Uygulama Hizmeti uygulaması oluşturur ve FTP kullanarak bir dosyayı dağıtAbilir. |
| [GitHub'dan bir uygulama oluşturma ve kod dağıtma](./scripts/cli-deploy-github.md?toc=%2fcli%2fazure%2ftoc.json)| Bir Uygulama Hizmeti uygulaması oluşturur ve herkese açık bir GitHub deposundan kod dağıtır. |
| [GitHub'dan sürekli dağıtım içeren bir uygulama oluşturma](./scripts/cli-continuous-deployment-github.md?toc=%2fcli%2fazure%2ftoc.json)| Sahip olduğunuz Bir GitHub deposundan sürekli yayın içeren bir Uygulama Hizmeti uygulaması oluşturur. |
| [Yerel bir Git deposundan bir uygulama oluşturma ve kod dağıtma](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Uygulama Hizmeti uygulaması oluşturur ve yerel bir Git deposundan kod itme yapılandırması. |
| [Bir uygulama oluşturma ve bir evreleme ortamına kod dağıtma](./scripts/cli-deploy-staging-environment.md?toc=%2fcli%2fazure%2ftoc.json) | Kod değişikliklerini evreleme için dağıtım yuvası na sahip bir Uygulama Hizmeti uygulaması oluşturur. |
| [Docker konteynerinde ASP.NET Core uygulaması oluşturma](./scripts/cli-linux-docker-aspnetcore.md?toc=%2fcli%2fazure%2ftoc.json)| Linux'ta bir Uygulama Hizmeti uygulaması oluşturur ve Docker Hub'dan Docker görüntüsü yükler. |
|**Uygulama yapılandırma**||
| [Özel bir etki alanını bir uygulamaya haritalamak](./scripts/cli-configure-custom-domain.md?toc=%2fcli%2fazure%2ftoc.json)| Bir Uygulama Hizmeti uygulaması oluşturur ve özel bir etki alanı adını eşler. |
| [Özel bir TLS/SSL sertifikasını bir uygulamaya bağlama](./scripts/cli-configure-ssl-certificate.md?toc=%2fcli%2fazure%2ftoc.json)| Bir Uygulama Hizmeti uygulaması oluşturur ve tls/SSL sertifikasını özel bir etki alanı adının sertifikasına bağlar. |
|**Uygulama ölçeklendirme**||
| [Uygulamayı el ile ölçeklendirin](./scripts/cli-scale-manual.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Uygulama Hizmeti uygulaması oluşturur ve 2 örnek arasında ölçeklendirin. |
| [Yüksek kullanılabilirlik mimarisine sahip bir uygulamayı dünya çapında ölçeklendirin](./scripts/cli-scale-high-availability.md?toc=%2fcli%2fazure%2ftoc.json) | İki farklı coğrafi bölgede iki Uygulama Hizmeti uygulaması oluşturur ve Azure Trafik Yöneticisi'ni kullanarak bunları tek bir uç noktadan kullanılabilir hale getirir. |
|**Uygulamayı koru**||
| [Azure Uygulama Ağ Geçidi ile tümleştirme](./scripts/cli-integrate-app-service-with-application-gateway.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Uygulama Hizmeti uygulaması oluşturur ve hizmet bitiş noktası ve erişim kısıtlamalarını kullanarak Uygulama Ağ Geçidi ile tümleştirir. |
|**Uygulamayı kaynaklara bağlama**||
| [Uygulamayı SQL Veritabanına bağlama](./scripts/cli-connect-to-sql.md?toc=%2fcli%2fazure%2ftoc.json)| Bir Uygulama Hizmeti uygulaması ve bir SQL veritabanı oluşturur, ardından uygulama ayarlarına veritabanı bağlantı dizesini ekler. |
| [Uygulamayı depolama hesabına bağlama](./scripts/cli-connect-to-storage.md?toc=%2fcli%2fazure%2ftoc.json)| Bir Uygulama Hizmeti uygulaması ve bir depolama hesabı oluşturur, ardından uygulama ayarlarına depolama bağlantı dizesi ekler. |
| [Redis için bir Azure Önbelleğine uygulamayı bağlama](./scripts/cli-connect-to-redis.md?toc=%2fcli%2fazure%2ftoc.json) | Redis için bir Uygulama Hizmeti uygulaması ve Azure Önbelleği oluşturur, ardından uygulama ayarlarına redis bağlantı ayrıntılarını ekler.) |
| [Bir uygulamayı Cosmos DB'ye bağlayın](./scripts/cli-connect-to-documentdb.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Uygulama Hizmeti uygulaması ve Cosmos DB oluşturur, ardından uygulama ayarlarına Cosmos DB bağlantı ayrıntılarını ekler. |
|**Uygulamayı yedekleme ve geri yükleme**||
| [Uygulama yedekleme](./scripts/cli-backup-onetime.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Uygulama Hizmeti uygulaması oluşturur ve bunun için tek seferlik yedekleme oluşturur. |
| [Uygulama için zamanlanmış yedekleme oluşturma](./scripts/cli-backup-scheduled.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Uygulama Hizmeti uygulaması oluşturur ve bunun için zamanlanmış bir yedekleme oluşturur. |
| [Bir uygulamayı yedeklemeden geri yükleme](./scripts/cli-backup-restore.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Uygulama Hizmeti uygulamasını yedeklemeden geri yükler. |
|**Uygulama izleme**||
| [Web sunucusu günlükleriyle bir uygulamayı izleme](./scripts/cli-monitor.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Uygulama Hizmeti uygulaması oluşturur, bunun için günlüğe kaydetmeyi sağlar ve günlükleri yerel makinenize indirir. |
| | |