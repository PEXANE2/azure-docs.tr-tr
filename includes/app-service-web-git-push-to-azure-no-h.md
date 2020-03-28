---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: e5ba08da5c58ff486bc26f2c771dfef55452629a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75681080"
---
Yerel terminal penceresine dönüp yerel Git deponuza bir Azure uzak deposu ekleyin. * \<DağıtımYerelGitUrl-from-create adım>* bir [web uygulaması oluştur'dan](#create-a-web-app)kaydettiğiniz Git uzaktan kumandasının URL'si ile değiştirin.

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Git Kimlik Bilgisi Yöneticisi sizden kimlik bilgileri istediğinde, Bir [dağıtım kullanıcısını Yapılandırma'da](/azure/app-service/containers/tutorial-python-postgresql-app#configure-a-deployment-user)oluşturduğunuz kimlik bilgilerini girdiğinden emin olun , Azure portalında oturum açmak için kullandığınız kimlik bilgilerini değil.

```bash
git push azure master
```

Bu komutun çalıştırılması birkaç dakika sürebilir. Çalıştırıldığında, aşağıdaki örneğe benzer bilgiler görüntüler:
