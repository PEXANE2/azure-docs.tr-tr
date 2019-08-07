---
title: Azure Sentinel önizleme ile olayları araştırın | Microsoft Docs
description: Azure Sentinel ile olayları araştırmayla ilgili bilgi edinmek için bu öğreticiyi kullanın.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: a493cd67-dc70-4163-81b8-04a9bc0232ac
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/6/2019
ms.author: rkarlin
ms.openlocfilehash: bad3fddd6caf7e6eb455e59280f181c787b95a4e
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780393"
---
# <a name="tutorial-investigate-incidents-with-azure-sentinel-preview"></a>Öğretici: Azure Sentinel önizleme ile olayları araştırın

> [!IMPORTANT]
> Azure Sentinel Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu öğretici, Azure Sentinel ile tehditleri algılamanıza yardımcı olur.

[Veri kaynaklarınızı](quickstart-onboard.md) Azure Sentinel 'e bağladıktan sonra, şüpheli bir sorun olduğunda bildirim almak istersiniz. Bunu yapmanızı sağlamak için Azure Sentinel, ortamınızdaki aykırları ve tehditleri derinlemesine araştırmak için atayabileceğiniz ve kullanabileceğiniz olaylar üreten gelişmiş uyarı kuralları oluşturmanıza olanak sağlar. 

> [!div class="checklist"]
> * Olay oluşturma
> * Olayları araştırma
> * Tehditlere yanıt verme

## <a name="investigate-incidents"></a>Olayları araştırma

bir olay birden çok uyarı içerebilir. Bu, belirli bir araştırma için ilgili tüm kanıtın bir toplamadır. **analiz** sayfasında tanımladığınız uyarılara göre bir olay oluşturulur. Önem ve durum gibi uyarılarla ilgili özellikler olay düzeyinde ayarlanır. Azure Sentinel 'e hangi tehditleri aradığınızı ve bunların nasıl bulunacağını öğrendikten sonra, olayları araştırarak algılanan tehditleri izleyebilirsiniz. 

1. **Olayları**seçin. **Olaylar** sayfası, kaç olay olduğunu, kaç tane açık olduğunu, **devam**ediyor olarak ayarlamış olduğunuzu ve kaç tane kapatıldığını görmenizi sağlar. Her olay için, gerçekleştiği zamanı ve olayın durumunu görebilirsiniz. Öncelikle ne işleyileceğine karar vermek için önem derecesine bakın. **Olaylar** sayfasında, bir olayla ilgili tüm uyarıları görmek için **Uyarılar** sekmesine tıklayın. Daha önce olayın bir parçası olarak eşleştirilmiş varlıklar, **varlıklar** sekmesinde görüntülenebilir.  Olayları gerektiği gibi filtreleyebilirsiniz, örneğin durum veya önem derecesine göre. **Olaylar** sekmesine baktığınızda, **analiz**' de tanımlanan algılama kurallarınız tarafından tetiklenen uyarıları içeren açık olaylar görürsünüz. Üst kısımda etkin olaylarınızı, yeni olayları ve devam eden olayları görürsünüz. Ayrıca, tüm olaylarınızın önem derecesine göre genel bir görünümünü görebilirsiniz.

   ![Uyarı panosu](./media/tutorial-investigate-cases/cases.png)

2. Bir araştırmaya başlamak için, belirli bir olaya tıklayın. Sağ tarafta, olayla ilgili ayrıntılı bilgileri, dahil edilen varlıkların sayısının özetini ve ilgili varlık sayısını (eşlemenizi temel alarak) görebilirsiniz. Her olayın benzersiz bir KIMLIĞI vardır. Olayın önem derecesi, olayda bulunan en ciddi uyarıya göre belirlenir.  

1. Olaydaki uyarılar ve varlıklar hakkında daha fazla ayrıntı görüntülemek için olay sayfasında **tam ayrıntıları görüntüle** ' ye tıklayın ve olay bilgilerini özetleyen ilgili sekmeleri gözden geçirin.  Tam olay görünümü, uyarıdaki tüm kanıtları, ilişkili uyarıları ve varlıkları birleştirir.

1. **Uyarılar** sekmesinde, tetiklendikten sonra ve ayarladığınız eşiklerden ne kadar aşılacağı uyarısını gözden geçirin. Uyarı ile ilgili tüm bilgileri, uyarıyı tetikleyen sorguyu, sorgu başına döndürülen sonuç sayısını ve uyarılar üzerinde PlayBook 'ları çalıştırma özelliğini görebilirsiniz. Olay içinde daha fazla ayrıntıya geçmek için, isabet sayısına tıklayın. Bu, sonuçları oluşturan sorguyu ve Log Analytics uyarıyı tetikleyen sonuçları açar.

3. **Varlıklar** sekmesinde, uyarı kuralı tanımının bir parçası olarak eşleştirmekte olduğunuz tüm varlıkları görebilirsiniz. 

4. Bir olayı etkin bir şekilde araştırıyorsanız, siz kapatıncaya kadar olay durumunu **devam ediyor** olarak ayarlamanız iyi bir fikirdir. Ayrıca, **Kapatılan çözümlenmiş** 'in bir olayın işlendiğini belirten olayların durumu olduğu, **Kapatılan kapatılan** olayların durumu, işleme gerektirmeyen olaylardaki durum olduğunu da kapatabilirsiniz. Bir olayı kapatma nedeninizi açıklayan açıklamalar gereklidir.

5. Olaylar, belirli bir kullanıcıya atanabilir. Her olay için, olay **sahibi** alanını ayarlayarak bir sahip atayabilirsiniz. Tüm olaylar atanmamış olarak başlar. Sahip olduğunuz tüm olayları görmek için olaylara gidebilir ve adınızla filtre uygulayabilirsiniz. 

5. İnceleme haritasını ve düzeltme adımlarıyla ihlalin kapsamını görüntülemek için **Araştır** ' a tıklayın. 



## <a name="respond-to-threats"></a>Tehditlere yanıt verme

Azure Sentinel, PlayBook 'ları kullanarak tehditlere yanıt vermek için iki birincil seçenek sunar. Bir uyarı tetiklendiğinde bir PlayBook 'u otomatik olarak çalışacak şekilde ayarlayabilir veya bir uyarı karşılığında el ile bir PlayBook çalıştırabilirsiniz.

- PlayBook 'u yapılandırırken bir uyarı tetiklendiğinde otomatik olarak çalışacak bir PlayBook ayarlayabilirsiniz. 

- PlayBook 'ları **görüntüle** ' ye tıklayıp çalıştırmak için bir PlayBook seçerek, uyarının içinden el ile bir PlayBook çalıştırabilirsiniz.




## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure Sentinel kullanarak olayları araştırmaya nasıl başladığınızı öğrendiniz. [Otomatikleştirilmiş PlayBook 'lar kullanılarak tehditlere yanıt verme](tutorial-respond-threats-playbook.md)öğreticisine devam edin.
> [!div class="nextstepaction"]
> Tehditlere yönelik yanıtlarınızı otomatik hale getirmek için [tehditleri yanıtlayın](tutorial-respond-threats-playbook.md) .

