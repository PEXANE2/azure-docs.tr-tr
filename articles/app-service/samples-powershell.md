---
title: PowerShell örnekleri
description: Ortak Uygulama Hizmeti senaryolarından bazıları için Azure PowerShell örneklerini bulun. Uygulama Hizmeti dağıtımını veya yönetim görevlerinizi nasıl otomatikleştirebilirsiniz öğrenin.
tags: azure-service-management
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.topic: sample
ms.date: 03/08/2017
ms.custom: mvc
ms.openlocfilehash: f289bd453f2387282402394c807fe9700151f221
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81532434"
---
# <a name="powershell-samples-for-azure-app-service"></a>Azure Uygulama Hizmeti için PowerShell örnekleri

Aşağıdaki tablo, Azure PowerShell kullanılarak oluşturulmuş PowerShell komut dosyalarına bağlantılar içerir.

| | |
|-|-|
|**Uygulama oluşturma**||
| [GitHub'dan dağıtım la uygulama oluşturma](./scripts/powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| GitHub'dan kod çeken bir Uygulama Hizmeti uygulaması oluşturur. |
| [GitHub'dan sürekli dağıtım içeren bir uygulama oluşturma](./scripts/powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| GitHub'dan sürekli kod dağıtan bir Uygulama Hizmeti uygulaması oluşturur. |
| [FTP ile bir uygulama oluşturun ve kod dağıtın](./scripts/powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | FTP kullanarak bir Uygulama Hizmeti uygulaması oluşturur ve yerel bir dizinden dosya yükler. |
| [Yerel bir Git deposundan bir uygulama oluşturma ve kod dağıtma](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Uygulama Hizmeti uygulaması oluşturur ve yerel bir Git deposundan kod itme yapılandırması. |
| [Bir uygulama oluşturma ve bir evreleme ortamına kod dağıtma](./scripts/powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Kod değişikliklerini evreleme için dağıtım yuvası na sahip bir Uygulama Hizmeti uygulaması oluşturur. |
|**Uygulama yapılandırma**||
| [Özel bir etki alanını bir uygulamaya haritalamak](./scripts/powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir Uygulama Hizmeti uygulaması oluşturur ve özel bir etki alanı adını eşler. |
| [Özel bir TLS/SSL sertifikasını bir uygulamaya bağlama](./scripts/powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir Uygulama Hizmeti uygulaması oluşturur ve tls/SSL sertifikasını özel bir etki alanı adının sertifikasına bağlar. |
|**Uygulama ölçeklendirme**||
| [Uygulamayı el ile ölçeklendirin](./scripts/powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Uygulama Hizmeti uygulaması oluşturur ve 2 örnek arasında ölçeklendirin. |
| [Yüksek kullanılabilirlik mimarisine sahip bir uygulamayı dünya çapında ölçeklendirin](./scripts/powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | İki farklı coğrafi bölgede iki Uygulama Hizmeti uygulaması oluşturur ve Azure Trafik Yöneticisi'ni kullanarak bunları tek bir uç noktadan kullanılabilir hale getirir. |
|**Uygulamayı kaynaklara bağlama**||
| [Uygulamayı SQL Veritabanına bağlama](./scripts/powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir Uygulama Hizmeti uygulaması ve bir SQL veritabanı oluşturur, ardından uygulama ayarlarına veritabanı bağlantı dizesini ekler. |
| [Uygulamayı depolama hesabına bağlama](./scripts/powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Bir Uygulama Hizmeti uygulaması ve bir depolama hesabı oluşturur, ardından uygulama ayarlarına depolama bağlantı dizesi ekler. |
|**Uygulamayı yedekleme ve geri yükleme**||
| [Uygulama yedekleme](./scripts/powershell-backup-onetime.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Uygulama Hizmeti uygulaması oluşturur ve bunun için tek seferlik yedekleme oluşturur. |
| [Uygulama için zamanlanmış yedekleme oluşturma](./scripts/powershell-backup-scheduled.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Uygulama Hizmeti uygulaması oluşturur ve bunun için zamanlanmış bir yedekleme oluşturur. |
| [Uygulamanın yedeklemesi silme](./scripts/powershell-backup-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir uygulama için varolan bir yedeklemeyi siler. |
| [Uygulamayı yedeklemeden geri yükleme](./scripts/powershell-backup-restore.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Daha önce tamamlanmış bir yedeklemeden bir uygulamayı geri yükler. |
| [Abonelikler arasında yedekleme yi geri yükleme](./scripts/powershell-backup-restore-diff-sub.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Başka bir abonelikteki bir yedeklemeden web uygulamasını geri yükler. |
|**Uygulama izleme**||
| [Web sunucusu günlükleriyle bir uygulamayı izleme](./scripts/powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir Uygulama Hizmeti uygulaması oluşturur, bunun için günlüğe kaydetmeyi sağlar ve günlükleri yerel makinenize indirir. |
| | |
