---
title: Windows kapsayıcısında kendiliğinden konak Integration Runtime çalıştırma
description: Windows kapsayıcısında kendiliğinden konak Integration Runtime çalıştırma hakkında bilgi edinin.
services: data-factory
ms.author: abnarain
author: nabhishek
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/05/2020
ms.openlocfilehash: d6f292ff89a70de90e6b86f19f73de26963d997f
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87927584"
---
# <a name="how-to-run-self-hosted-integration-runtime-in-windows-container"></a>Windows kapsayıcısında kendiliğinden konak Integration Runtime çalıştırma

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, Windows kapsayıcısında kendiliğinden konak Integration Runtime çalıştırma açıklanmaktadır.
Azure Data Factory, şirket içinde barındırılan Integration Runtime resmi Windows kapsayıcı desteğini teslim ediyor. Docker Build kaynak kodunu indirebilir ve oluşturma ve çalıştırma sürecini kendi sürekli teslim işlem hattınızda birleştirebilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar 
- [Windows kapsayıcı gereksinimleri](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/system-requirements)
- Docker sürüm 2,3 ve üzeri 
- Şirket içinde barındırılan Integration Runtime sürüm 4.11.7512.1 ve üzeri 
## <a name="get-started"></a>başlarken 
1.  Docker 'ı yükleyip Windows kapsayıcısını etkinleştir 
2.  Kaynak kodu https://github.com/Azure/Azure-Data-Factory-Integration-Runtime-in-Windows-Container adresinden indirin
3.  ' SHIR ' klasöründeki en son sürüm KııR 'yi indirin 
4.  Klasörünüzü kabukta açın: 
```console
cd "yourFolderPath"
```

5.  Windows Docker görüntüsünü oluşturun: 
```console
docker build . -t "yourDockerImageName" 
```
6.  Docker kapsayıcısını Çalıştır: 
```console
docker run -d -e NODE_NAME="irNodeName" -e AUTH_KEY="IR_AUTHENTICATION_KEY" -e ENABLE_HA=true HA_PORT=8060 "yourDockerImageName"    
```
> [!NOTE]
> Bu komut için AUTH_KEY zorunludur. NODE_NAME, ENABLE_HA ve HA_PORT isteğe bağlıdır. Bu değeri ayarlamazsanız, komut varsayılan değerleri kullanır. ENABLE_HA varsayılan değeri false 'dur ve HA_PORT 8060 ' dir.

## <a name="container-health-check"></a>Kapsayıcı sistem durumu denetimi 
120 saniye başlangıç süresinden sonra, sistem durumu denetleyicisi 30 saniyede bir düzenli olarak çalışır. , Kapsayıcı altyapısına IR sistem durumu sağlar. 

## <a name="limitations"></a>Sınırlamalar
Şu anda, Windows kapsayıcısında şirket içinde barındırılan Integration Runtime çalıştırırken şu özellikleri desteklemiyoruz:
- HTTP proxy 'si 
- TLS/SSL sertifikasıyla şifrelenmiş düğüm düğümü iletişimi 
- Yedekleme oluşturma ve içeri aktarma 
- Daemon hizmeti 
- Otomatik güncelleştirme 

### <a name="next-steps"></a>Sonraki adımlar
- [Azure Data Factory tümleştirme çalışma zamanı kavramlarını](https://docs.microsoft.com/azure/data-factory/concepts-integration-runtime)gözden geçirin.
- [Azure Portal şirket içinde barındırılan tümleştirme çalışma zamanı oluşturmayı](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime)öğrenin.


