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
ms.openlocfilehash: 08f4e76869f124d946566f64da394c895d0af308
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102206006"
---
Yerel terminal penceresine dönüp yerel Git deponuza bir Azure uzak deposu ekleyin. *\<deploymentLocalGitUrl-from-create-step>* [Web uygulaması oluştur](#create-a-web-app)listesinden kaydettiğiniz GIT uzak öğesinin URL 'siyle değiştirin.

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Git kimlik bilgileri Yöneticisi kimlik bilgilerini sizden isterse, Azure portal oturum açmak için kullandığınız kimlik bilgilerini değil **dağıtım kullanıcısı yapılandırma** bölümünde oluşturduğunuz kimlik bilgilerini girdiğinizden emin olun.

```bash
git push azure main
```

Bu komutun çalıştırılması birkaç dakika sürebilir. Çalıştırıldığında, aşağıdaki örneğe benzer bilgiler görüntüler:
