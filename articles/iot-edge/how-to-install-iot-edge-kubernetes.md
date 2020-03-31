---
title: Kubernetes'e IoT Edge nasıl yüklenir | Microsoft Dokümanlar
description: Yerel bir geliştirme kümesi ortamı nı kullanarak IoT Edge'in Kubernetes'e nasıl yükleyeceğimiz hakkında bilgi edinin
author: kgremban
manager: philmea
ms.author: veyalla
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 4b2068c3944f9e7616b0666c7bafcafc68ee0cd9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471294"
---
# <a name="how-to-install-iot-edge-on-kubernetes-preview"></a>Kubernetes'e IoT Edge nasıl yüklenir (Önizleme)

IoT Edge, esnek ve yüksek kullanılabilir bir altyapı katmanı olarak kubernetes ile entegre olabilir. Bu desteğin üst düzey bir IoT Edge çözümüne uyduğu yer:

![k8s giriş](./media/how-to-install-iot-edge-kubernetes/kubernetes-model.png)

>[!TIP]
>Bu entegrasyon için iyi bir zihinsel model başka bir çalışma ortamı IoT Edge uygulamaları Linux ve Windows ek olarak çalıştırabilirsiniz olarak Kubernetes düşünmektir.

## <a name="architecture"></a>Mimari 
Kubernetes'te IoT Edge, kenar iş yükü dağıtımları için *Özel Kaynak Tanımı* (CRD) sağlar. IoT Edge Agent, bulut tarafından yönetilen istenen durumu yerel küme durumuyla bağdaştıran bir *CRD denetleyicisi* rolünü üstlenir.

Modül ömrü, modül kullanılabilirliğini koruyan ve yerleşimlerini seçen Kubernetes zamanlayıcısı tarafından yönetilir. IoT Edge, üstte çalışan kenar uygulama platformlarını yönetir ve kenar kümesindeki durumla IoT Hub'da belirtilen istenilen durumu sürekli olarak uzlaştırarak. Uygulama modeli hala IoT Edge modülleri ve yolları dayalı tanıdık modelidir. IoT Edge Aracısı denetleyicisi, *otomatik* çeviri IoT Edge'in uygulama modelini bölmeler, dağıtımlar, hizmetler vb. gibi Kubernetes yerel yapılarına gerçekleştirir.

Burada üst düzey bir mimari diyagramı verilmiştir:

![kubernetes kemer](./media/how-to-install-iot-edge-kubernetes/publicpreview-refresh-kubernetes.png)

Kenar dağıtımının her bileşeni, aygıta özgü bir Kubernetes ad alanına kadar genişbir alana sahiptir ve bu da aynı küme kaynaklarını birden çok kenar aygıtı ve bunların dağıtımları arasında paylaşabilmekiçin mümkün kılar.

>[!NOTE]
>Kubernetes üzerinde IoT Edge [genel önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="tutorials-and-references"></a>Eğitimler ve referanslar 

Ayrıntılı eğitimler ve referanslar da dahil olmak üzere daha fazla bilgi için lütfen [Kubernetes önizleme dokümanları mini sitesine](https://aka.ms/edgek8sdoc) bakın.
