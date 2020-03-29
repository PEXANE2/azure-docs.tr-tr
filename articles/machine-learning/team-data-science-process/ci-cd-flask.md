---
title: Azure Ardışık Hatları ile BIR CI/CD ardışık işlem oluşturma - Ekip Veri Bilimi Süreci
description: Docker ve Kubernetes kullanarak Yapay Zeka (AI) uygulamaları için sürekli entegrasyon ve sürekli teslimat boru hattı oluşturun.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=jainr, previous-ms.author=jainr
ms.openlocfilehash: 42433ec419ac9e02077cd0359e18b5114206f27d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721838"
---
# <a name="create-cicd-pipelines-for-ai-apps-using-azure-pipelines-docker-and-kubernetes"></a>Azure Pipelines, Docker ve Kubernetes kullanarak AI uygulamaları için CI/CD ardışık lıkları oluşturun

Yapay Zeka (AI) uygulaması, önceden eğitilmiş bir makine öğrenimi (ML) modeline gömülü uygulama kodudur. Bir AI uygulaması için her zaman iki çalışma akışı vardır: Veri bilimciler ML modelini oluşturur ve uygulama geliştiricileri uygulamayı oluşturur ve son kullanıcılara tüketir. Bu makalede, ML modelini uygulama kaynak koduna gömen bir AI uygulaması için sürekli tümleştirme ve sürekli teslimat (CI/CD) ardışık Örnek kod ve öğretici bir Python Flask web uygulaması kullanır ve özel bir Azure blob depolama hesabından önceden eğitilmiş bir model getirir. Ayrıca bir AWS S3 depolama hesabı kullanabilirsiniz.

> [!NOTE]
> Aşağıdaki işlem CI/CD yapmanın birkaç yollarından biridir. Bu araç ve ön koşullar için alternatifler vardır.

## <a name="source-code-tutorial-and-prerequisites"></a>Kaynak kodu, öğretici ve ön koşullar

[Kaynak kodunu](https://github.com/Azure/DevOps-For-AI-Apps) ve GitHub'dan ayrıntılı bir [öğretici](https://github.com/Azure/DevOps-For-AI-Apps/blob/master/Tutorial.md) indirebilirsiniz. Kendi uygulamanız için bir CI/CD ardışık uygulama için öğretici adımları izleyin.

İndirilen kaynak kodunu ve öğreticiyi kullanmak için aşağıdaki ön koşullara ihtiyacınız vardır: 

- [Kaynak kodu deposu](https://github.com/Azure/DevOps-For-AI-Apps) GitHub hesabınıza çatallandı
- [Azure Devops Kuruluşu](/azure/devops/organizations/accounts/create-organization-msa-or-work-student)
- [Azure CLI](/cli/azure/install-azure-cli)
- [Kubernetes (AKS) kümesi için Bir Azure Kapsayıcı Hizmeti](/azure/container-service/kubernetes/container-service-tutorial-kubernetes-deploy-cluster)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) komutları çalıştırmak ve AKS kümesinden yapılandırma getirmek 
- Azure [Kapsayıcı Kayıt Defteri (ACR) hesabı](/azure/container-registry/container-registry-get-started-portal)

## <a name="cicd-pipeline-summary"></a>CI/CD boru hattı özeti

Her yeni Git taahhüt Yapı boru hattı başladı. Yapı, en son ML modelini blob depolama hesabından güvenli bir şekilde çeker ve tek bir kapta uygulama koduyla paketler. Uygulama geliştirme ve veri bilimi iş akışlarının bu ayrıştırması, üretim uygulamasının her zaman en son ML modeliyle en son kodu çalıştırmasını sağlar. Uygulama testi geçerse, ardışık hatlar yapı görüntüsünü ACR'deki bir Docker konteynerinde güvenli bir şekilde saklar. Serbest bırakma ardışık hattı daha sonra AKS kullanarak kapsayıcı dağılar. 

## <a name="cicd-pipeline-steps"></a>CI/CD boru hattı adımları

Aşağıdaki diyagram ve adımlar CI/CD boru hattı mimarisini açıklar:

![CI/CD boru hattı mimarisi](./media/ci-cd-flask/architecture.png)

1. Geliştiriciler, seçtikleri IDE'deki uygulama kodu üzerinde çalışırlar.
2. Geliştiriciler kodu Azure Repos, GitHub veya diğer Git kaynak denetim sağlayıcısına adamaktadır. 
3. Ayrıca, veri bilim adamları ML modelini geliştirmek üzerinde çalışırlar.
4. Veri bilim adamları bir model deposuna bitmiş modeli yayınlamak, Bu durumda bir blob depolama hesabı. 
5. Azure Pipelines, Git commit'e dayalı bir yapı başlatıyor.
6. Yapı ardışık hattı blob depolamaen en son ML modelini çeker ve bir kapsayıcı oluşturur.
7. Boru hattı, yapı görüntüsünü ACR'deki özel görüntü deposuna iter.
8. Release boru hattı, başarılı yapıya göre başladı.
9. Boru hattı ACR'den en son görüntüyü çeker ve AKS'deki Kubernetes kümesi boyunca dağır.
10. Uygulama için kullanıcı istekleri DNS sunucusu üzerinden gidin.
11. DNS sunucusu istekleri bir yük dengeleyicisine iletır ve yanıtları kullanıcılara geri gönderir.

## <a name="see-also"></a>Ayrıca bkz.

- [Ekip Veri Bilimi Süreci (TDSP)](/azure/machine-learning/team-data-science-process/)
- [Azure Makine Öğrenimi (AML)](/azure/machine-learning/)
- [Azure DevOps](https://azure.microsoft.com/services/devops/)
- [Azure Kubernetes Hizmetleri (AKS)](/azure/aks/intro-kubernetes)
