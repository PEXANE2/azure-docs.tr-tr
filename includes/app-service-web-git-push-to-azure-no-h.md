---
title: dosya dahil etme
description: dosya dahil etme
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 8539696f4521a1b4a2f56fe7d2936b45dec26ec9
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88077723"
---
Yerel terminal penceresine dönüp yerel Git deponuza bir Azure uzak deposu ekleyin. *\<deploymentLocalGitUrl-from-create-step>* [Web uygulaması oluştur](#create-a-web-app)listesinden kaydettiğiniz GIT uzak öğesinin URL 'siyle değiştirin.

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Git kimlik bilgileri Yöneticisi kimlik bilgilerini sizden isterse, Azure portal oturum açmak için kullandığınız kimlik bilgilerini değil **dağıtım kullanıcısı yapılandırma**bölümünde oluşturduğunuz kimlik bilgilerini girdiğinizden emin olun.

```bash
git push azure master
```

Bu komutun çalıştırılması birkaç dakika sürebilir. Çalıştırıldığında, aşağıdaki örneğe benzer bilgiler görüntüler:
