---
title: include dosyası
description: include dosyası
services: machine-learning
ms.service: machine-learning
ms.custom: include file
ms.topic: include
author: xiaoharper
ms.author: zhanxia
ms.date: 10/18/2019
ms.openlocfilehash: e3cb977871af2e6cd7a59dd48505090dd29e8a76
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75541809"
---
1. [Azure Machine Learning stüdyosunda](https://ml.azure.com)oturum açın.

1. Çalışma alanınızı Enterprise sürümüne yükseltin.

    Yükseltmeden sonra, tüm görsel arabirim denemeleriniz tasarımcıda boru hattı taslaklarına dönüştürülür.
    
    > [!NOTE]
    > Görsel arabirim web hizmetlerini gerçek zamanlı uç noktalara dönüştürmek için Enterprise sürümüne yükseltme yapmanız gerekmez.
    
1. Boru hattı taslakları listenizi görüntülemek için çalışma alanının tasarımcı bölümüne gidin. 
    
    Dönüştürülmüş web hizmetleri **Endpoints** > **Gerçek zamanlı uç noktalarına**yönlendirilerek bulunabilir.

1. Açmak için bir boru hattı taslağı seçin.

    Dönüştürme işlemi sırasında bir hata varsa, sorunu gidermek için yönergeleri içeren bir hata iletisi görüntülenir. 

### <a name="known-issues"></a>Bilinen sorunlar

 El ile ele alınması gereken bilinen geçiş sorunları aşağıda verilmiştir:

- **Veri İçe Aktarma** veya **Dışa Aktarma Verileri** modülleri
        
    Denemede bir **İçe Aktarma Verisi** veya **Dışa Aktarma Verisi** modülü varsa, veri depolarını kullanmak için veri kaynağını güncelleştirmeniz gerekir. Bir veri deposu oluşturmayı öğrenmek için [Azure depolama hizmetlerinde Verilere Nasıl Erişilir'e](../articles/machine-learning/how-to-access-data.md)bakın. Bulut depolama hesabı bilgileriniz, kolaylık sağlamak için **İçe Aktarma Verileri** veya **Dışa Aktarma Verileri** modülünün açıklamalarına eklenmiştir. 
      