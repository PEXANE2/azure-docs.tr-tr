---
title: DevTest Labs 'de özel resimleri ve formülleri karşılaştırma | Microsoft Docs
description: Özel görüntüler ve formüller arasındaki farklar hakkında bilgi edinin. bu sayede ortamınıza en uygun olanı seçebilirsiniz.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: dac820d0600745906e4b1aeef95806ed5679488a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85484035"
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a>DevTest Labs 'de özel resimleri ve formülleri karşılaştırma
[Özel görüntüler](devtest-lab-create-template.md) ve [formüller](devtest-lab-manage-formulas.md) , [oluşturulan yeni sanal makinelerin](devtest-lab-add-vm.md)temeli olarak kullanılabilir. Bununla birlikte, özel görüntüler ve formüller arasındaki önemli ayrım, özel bir görüntünün bir VHD 'yi temel alan bir görüntü olduğu, ancak bir formülde VM boyutu, sanal ağ, alt ağ ve yapıtlar gibi önceden yapılandırılmış ayarların *yanı sıra* bir VHD 'yi temel alan bir görüntü olduğu sürece. Bu önceden yapılandırılmış ayarlar, VM oluşturma sırasında geçersiz kılınabilen varsayılan değerlerle ayarlanır. Bu makalede, formüllerin kullanımı ve özel görüntüleri kullanmak için bazı avantajlar (olumlu) ve dezavantajlar (dezavantajlar) açıklanmaktadır.

## <a name="custom-image-pros-and-cons"></a>Özel görüntü uzmanları ve dezavantajları
Özel görüntüler, istenen bir ortamdan sanal makine oluşturmanın statik, sabit bir yolunu sağlar. 

**Artıları**

* Bir özel görüntüden VM sağlama, sanal makine görüntüden çıktıktan sonra hiçbir şey değişmediği kadar hızlıdır. Diğer bir deyişle, özel görüntü yalnızca ayarları olmayan bir görüntü olduğu için uygulanacak bir ayar yoktur. 
* Tek bir özel görüntüden oluşturulan VM 'Ler aynıdır.

**Simgeler**

* Özel görüntünün bazı yönlerini güncelleştirmeniz gerekiyorsa görüntünün yeniden oluşturulması gerekir.  

## <a name="formula-pros-and-cons"></a>Formül uzmanları ve dezavantajları
Formüller, istenen yapılandırma/ayarlardan sanal makineler oluşturmak için dinamik bir yol sağlar.

**Artıları**

* Ortamdaki değişiklikler yapıtlar aracılığıyla anında yakalanabilir. Örneğin, yayın işlem hattınızdan en son BITS yüklenmiş bir VM 'nin yüklenmesini istiyorsanız veya deponuzdaki en son kodu listeliyorsanız, yalnızca en son bitleri dağıtan veya formüldeki en son kodu bir hedef temel görüntüyle birlikte listeleyen bir yapıt belirtebilirsiniz. Bu formül VM oluşturmak için kullanıldığında, en son bit/kod sanal makineye dağıtılır/kaydedilir. 
* Formüller, özel görüntülerin VM boyutları ve sanal ağ ayarları gibi sağlayamalamadığı varsayılan ayarları tanımlayabilir. 
* Bir formülde kaydedilen ayarlar varsayılan değer olarak gösterilir, ancak VM oluşturulduğunda değiştirilebilir. 

**Simgeler**

* Bir formülden VM oluşturmak, özel bir görüntüden VM oluşturmaktan daha uzun sürebilir.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri
* [Özel görüntüler veya formüller mi?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Sonraki adımlar
- [DevTest Labs SSS](devtest-lab-faq.md)